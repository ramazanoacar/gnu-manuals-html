### 14.9 Prepare the Data for Display in a Graph

The `recursive-lengths-list-many-files` function returns a list
of numbers. Each number records the length of a function definition.
What we need to do now is transform this data into a list of numbers
suitable for generating a graph. The new list will tell how many
functions definitions contain less than 10 words and
symbols, how many contain between 10 and 19 words and symbols, how
many contain between 20 and 29 words and symbols, and so on.

In brief, we need to go through the lengths’ list produced by the
`recursive-lengths-list-many-files` function and count the number
of defuns within each range of lengths, and produce a list of those
numbers.

* [The Data for Display in Detail](#Data-for-Display-in-Detail)
* [Sorting Lists](#Sorting)
* [Making a List of Files](#Files-List)
* [Counting function definitions](#Counting-function-definitions)

---

Next: [Sorting Lists](#Sorting), Up: [Prepare the Data for Display in a Graph](#Prepare-the-data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### The Data for Display in Detail

Based on what we have done before, we can readily foresee that it
should not be too hard to write a function that CDRs down the
lengths’ list, looks at each element, determines which length range it
is in, and increments a counter for that range.

However, before beginning to write such a function, we should consider
the advantages of sorting the lengths’ list first, so the numbers are
ordered from smallest to largest. First, sorting will make it easier
to count the numbers in each range, since two adjacent numbers will
either be in the same length range or in adjacent ranges. Second, by
inspecting a sorted list, we can discover the highest and lowest
number, and thereby determine the largest and smallest length range
that we will need.

---

Next: [Making a List of Files](#Files-List), Previous: [The Data for Display in Detail](#Data-for-Display-in-Detail), Up: [Prepare the Data for Display in a Graph](#Prepare-the-data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.9.1 Sorting Lists

Emacs contains a function to sort lists, called (as you might guess)
`sort`. The `sort` function takes two arguments, the list
to be sorted, and a predicate that determines whether the first of
two list elements is less than the second.

As we saw earlier (see [Using the Wrong
Type Object as an Argument](#Wrong-Type-of-Argument)), a predicate is a function that
determines whether some property is true or false. The `sort`
function will reorder a list according to whatever property the
predicate uses; this means that `sort` can be used to sort
non-numeric lists by non-numeric criteria—it can, for example,
alphabetize a list.

The `<` function is used when sorting a numeric list. For example,

```
(sort '(4 8 21 17 33 7 21 7) '<)
```

produces this:

```
(4 7 7 8 17 21 21 33)
```

(Note that in this example, both the arguments are quoted so that the
symbols are not evaluated before being passed to `sort` as
arguments.)

Sorting the list returned by the
`recursive-lengths-list-many-files` function is straightforward;
it uses the `<` function:

```
(sort
 (recursive-lengths-list-many-files
  '("./lisp/macros.el"
    "./lisp/mailalias.el"
    "./lisp/hex-util.el"))
 '<)
```

which produces:

```
(29 32 38 71 82 90 95 178 180 218 263 283 321 324 480)
```

(Note that in this example, the first argument to `sort` is not
quoted, since the expression must be evaluated so as to produce the
list that is passed to `sort`.)

---

Next: [Counting function definitions](#Counting-function-definitions), Previous: [Sorting Lists](#Sorting), Up: [Prepare the Data for Display in a Graph](#Prepare-the-data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.9.2 Making a List of Files

The `recursive-lengths-list-many-files` function requires a list
of files as its argument. For our test examples, we constructed such
a list by hand; but the Emacs Lisp source directory is too large for
us to do for that. Instead, we will write a function to do the job
for us. In this function, we will use both a `while` loop and a
recursive call.

We did not have to write a function like this for older versions of
GNU Emacs, since they placed all the ‘`.el`’ files in one
directory. Instead, we were able to use the `directory-files`
function, which lists the names of files that match a specified
pattern within a single directory.

However, recent versions of Emacs place Emacs Lisp files in
sub-directories of the top level `lisp` directory. This
re-arrangement eases navigation. For example, all the mail related
files are in a `lisp` sub-directory called `mail`. But at
the same time, this arrangement forces us to create a file listing
function that descends into the sub-directories.

We can create this function, called `files-in-below-directory`,
using familiar functions such as `car`, `nthcdr`, and
`substring` in conjunction with an existing function called
`directory-files-and-attributes`. This latter function not only
lists all the filenames in a directory, including the names
of sub-directories, but also their attributes.

To restate our goal: to create a function that will enable us
to feed filenames to `recursive-lengths-list-many-files`
as a list that looks like this (but with more elements):

```
("./lisp/macros.el"
 "./lisp/mail/rmail.el"
 "./lisp/hex-util.el")
```

The `directory-files-and-attributes` function returns a list of
lists. Each of the lists within the main list consists of 13
elements. The first element is a string that contains the name of the
file—which, in GNU/Linux, may be a *directory file*, that is to
say, a file with the special attributes of a directory. The second
element of the list is `t` for a directory, a string
for symbolic link (the string is the name linked to), or `nil`.

For example, the first ‘`.el`’ file in the `lisp/` directory
is `abbrev.el`. Its name is
`/usr/local/share/emacs/22.1.1/lisp/abbrev.el` and it is not a
directory or a symbolic link.

This is how `directory-files-and-attributes` lists that file and
its attributes:

```
("abbrev.el"
nil
1
1000
100
```

```
(20615 27034 579989 697000)
(17905 55681 0 0)
(20615 26327 734791 805000)(17)
13188
"-rw-r--r--"
```

```
t
2971624
773)
```

On the other hand, `mail/` is a directory within the `lisp/`
directory. The beginning of its listing looks like this:

```
("mail"
t
…
)
```

(To learn about the different attributes, look at the documentation of
`file-attributes`. Bear in mind that the `file-attributes`
function does not list the filename, so its first element is
`directory-files-and-attributes`’s second element.)

We will want our new function, `files-in-below-directory`, to
list the ‘`.el`’ files in the directory it is told to check, and in
any directories below that directory.

This gives us a hint on how to construct
`files-in-below-directory`: within a directory, the function
should add ‘`.el`’ filenames to a list; and if, within a directory,
the function comes upon a sub-directory, it should go into that
sub-directory and repeat its actions.

However, we should note that every directory contains a name that
refers to itself, called `.` (“dot”), and a name that refers to
its parent directory, called `..` (“dot dot”). (In
`/`, the root directory, `..` refers to itself, since
`/` has no parent.) Clearly, we do not want our
`files-in-below-directory` function to enter those directories,
since they always lead us, directly or indirectly, to the current
directory.

Consequently, our `files-in-below-directory` function must do
several tasks:

* Check to see whether it is looking at a filename that ends in
  ‘`.el`’; and if so, add its name to a list.
* Check to see whether it is looking at a filename that is the name of a
  directory; and if so,
  + Check to see whether it is looking at `.` or `..`; and if
    so skip it.
  + Or else, go into that directory and repeat the process.

Let’s write a function definition to do these tasks. We will use a
`while` loop to move from one filename to another within a
directory, checking what needs to be done; and we will use a recursive
call to repeat the actions on each sub-directory. The recursive
pattern is Accumulate
(see [Recursive Pattern: *accumulate*](#Accumulate)),
using `append` as the combiner.

Here is the function:

```
(defun files-in-below-directory (directory)
  "List the .el files in DIRECTORY and in its sub-directories."
  ;; Although the function will be used non-interactively,
  ;; it will be easier to test if we make it interactive.
  ;; The directory will have a name such as
  ;;  "/usr/local/share/emacs/22.1.1/lisp/"
  (interactive "DDirectory name: ")
```

```
  (let (el-files-list
        (current-directory-list
         (directory-files-and-attributes directory t)))
    ;; while we are in the current directory
    (while current-directory-list
```

```
      (cond
       ;; check to see whether filename ends in '.el'
       ;; and if so, add its name to a list.
       ((equal ".el" (substring (car (car current-directory-list)) -3))
        (setq el-files-list
              (cons (car (car current-directory-list)) el-files-list)))
```

```
       ;; check whether filename is that of a directory
       ((eq t (car (cdr (car current-directory-list))))
        ;; decide whether to skip or recurse
        (if
            (equal "."
                   (substring (car (car current-directory-list)) -1))
            ;; then do nothing since filename is that of
            ;;   current directory or parent, "." or ".."
            ()
```

```
          ;; else descend into the directory and repeat the process
          (setq el-files-list
                (append
                 (files-in-below-directory
                  (car (car current-directory-list)))
                 el-files-list)))))
      ;; move to the next filename in the list; this also
      ;; shortens the list so the while loop eventually comes to an end
      (setq current-directory-list (cdr current-directory-list)))
    ;; return the filenames
    el-files-list))
```

The `files-in-below-directory` `directory-files` function
takes one argument, the name of a directory.

Thus, on my system,

```
(length
 (files-in-below-directory "/usr/local/share/emacs/22.1.1/lisp/"))
```

tells me that in and below my Lisp sources directory are 1031
‘`.el`’ files.

`files-in-below-directory` returns a list in reverse alphabetical
order. An expression to sort the list in alphabetical order looks
like this:

```
(sort
 (files-in-below-directory "/usr/local/share/emacs/22.1.1/lisp/")
 'string-lessp)
```

---

Previous: [Making a List of Files](#Files-List), Up: [Prepare the Data for Display in a Graph](#Prepare-the-data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.9.3 Counting function definitions

Our immediate goal is to generate a list that tells us how many
function definitions contain fewer than 10 words and symbols, how many
contain between 10 and 19 words and symbols, how many contain between
20 and 29 words and symbols, and so on.

With a sorted list of numbers, this is easy: count how many elements
of the list are smaller than 10, then, after moving past the numbers
just counted, count how many are smaller than 20, then, after moving
past the numbers just counted, count how many are smaller than 30, and
so on. Each of the numbers, 10, 20, 30, 40, and the like, is one
larger than the top of that range. We can call the list of such
numbers the `top-of-ranges` list.

If we wished, we could generate this list automatically, but it is
simpler to write a list manually. Here it is:

```
(defvar top-of-ranges
 '(10  20  30  40  50
   60  70  80  90 100
  110 120 130 140 150
  160 170 180 190 200
  210 220 230 240 250
  260 270 280 290 300)
 "List specifying ranges for `defuns-per-range'.")
```

To change the ranges, we edit this list.

Next, we need to write the function that creates the list of the
number of definitions within each range. Clearly, this function must
take the `sorted-lengths` and the `top-of-ranges` lists
as arguments.

The `defuns-per-range` function must do two things again and
again: it must count the number of definitions within a range
specified by the current top-of-range value; and it must shift to the
next higher value in the `top-of-ranges` list after counting the
number of definitions in the current range. Since each of these
actions is repetitive, we can use `while` loops for the job.
One loop counts the number of definitions in the range defined by the
current top-of-range value, and the other loop selects each of the
top-of-range values in turn.

Several entries of the `sorted-lengths` list are counted for each
range; this means that the loop for the `sorted-lengths` list
will be inside the loop for the `top-of-ranges` list, like a
small gear inside a big gear.

The inner loop counts the number of definitions within the range. It
is a simple counting loop of the type we have seen before.
(See [A loop with an incrementing counter](#Incrementing-Loop).)
The true-or-false test of the loop tests whether the value from the
`sorted-lengths` list is smaller than the current value of the
top of the range. If it is, the function increments the counter and
tests the next value from the `sorted-lengths` list.

The inner loop looks like this:

```
(while length-element-smaller-than-top-of-range
  (setq number-within-range (1+ number-within-range))
  (setq sorted-lengths (cdr sorted-lengths)))
```

The outer loop must start with the lowest value of the
`top-of-ranges` list, and then be set to each of the succeeding
higher values in turn. This can be done with a loop like this:

```
(while top-of-ranges
  body-of-loop…
  (setq top-of-ranges (cdr top-of-ranges)))
```

Put together, the two loops look like this:

```
(while top-of-ranges

  ;; Count the number of elements within the current range.
  (while length-element-smaller-than-top-of-range
    (setq number-within-range (1+ number-within-range))
    (setq sorted-lengths (cdr sorted-lengths)))

  ;; Move to next range.
  (setq top-of-ranges (cdr top-of-ranges)))
```

In addition, in each circuit of the outer loop, Emacs should record
the number of definitions within that range (the value of
`number-within-range`) in a list. We can use `cons` for
this purpose. (See [`cons`](#cons).)

The `cons` function works fine, except that the list it
constructs will contain the number of definitions for the highest
range at its beginning and the number of definitions for the lowest
range at its end. This is because `cons` attaches new elements
of the list to the beginning of the list, and since the two loops are
working their way through the lengths’ list from the lower end first,
the `defuns-per-range-list` will end up largest number first.
But we will want to print our graph with smallest values first and the
larger later. The solution is to reverse the order of the
`defuns-per-range-list`. We can do this using the
`nreverse` function, which reverses the order of a list.

For example,

```
(nreverse '(1 2 3 4))
```

produces:

```
(4 3 2 1)
```

Note that the `nreverse` function is destructive—that is,
it changes the list to which it is applied; this contrasts with the
`car` and `cdr` functions, which are non-destructive. In
this case, we do not want the original `defuns-per-range-list`,
so it does not matter that it is destroyed. (The `reverse`
function provides a reversed copy of a list, leaving the original list
as is.)

Put all together, the `defuns-per-range` looks like this:

```
(defun defuns-per-range (sorted-lengths top-of-ranges)
  "SORTED-LENGTHS defuns in each TOP-OF-RANGES range."
  (let ((top-of-range (car top-of-ranges))
        (number-within-range 0)
        defuns-per-range-list)
```

```

```

```
    ;; Outer loop.
    (while top-of-ranges
```

```

```

```
      ;; Inner loop.
      (while (and
              ;; Need number for numeric test.
              (car sorted-lengths)
              (< (car sorted-lengths) top-of-range))
```

```

```

```
        ;; Count number of definitions within current range.
        (setq number-within-range (1+ number-within-range))
        (setq sorted-lengths (cdr sorted-lengths)))

      ;; Exit inner loop but remain within outer loop.
```

```

```

```
      (setq defuns-per-range-list
            (cons number-within-range defuns-per-range-list))
      (setq number-within-range 0)      ; Reset count to zero.
```

```

```

```
      ;; Move to next range.
      (setq top-of-ranges (cdr top-of-ranges))
      ;; Specify next top of range value.
      (setq top-of-range (car top-of-ranges)))
```

```

```

```
    ;; Exit outer loop and count the number of defuns larger than
    ;;   the largest top-of-range value.
    (setq defuns-per-range-list
          (cons
           (length sorted-lengths)
           defuns-per-range-list))
```

```

```

```
    ;; Return a list of the number of definitions within each range,
    ;;   smallest to largest.
    (nreverse defuns-per-range-list)))
```

The function is straightforward except for one subtle feature. The
true-or-false test of the inner loop looks like this:

```
(and (car sorted-lengths)
     (< (car sorted-lengths) top-of-range))
```

instead of like this:

```
(< (car sorted-lengths) top-of-range)
```

The purpose of the test is to determine whether the first item in the
`sorted-lengths` list is less than the value of the top of the
range.

The simple version of the test works fine unless the
`sorted-lengths` list has a `nil` value. In that case, the
`(car sorted-lengths)` expression function returns
`nil`. The `<` function cannot compare a number to
`nil`, which is an empty list, so Emacs signals an error and
stops the function from attempting to continue to execute.

The `sorted-lengths` list always becomes `nil` when the
counter reaches the end of the list. This means that any attempt to
use the `defuns-per-range` function with the simple version of
the test will fail.

We solve the problem by using the `(car sorted-lengths)`
expression in conjunction with the `and` expression. The
`(car sorted-lengths)` expression returns a non-`nil`
value so long as the list has at least one number within it, but
returns `nil` if the list is empty. The `and` expression
first evaluates the `(car sorted-lengths)` expression, and
if it is `nil`, returns false *without* evaluating the
`<` expression. But if the `(car sorted-lengths)`
expression returns a non-`nil` value, the `and` expression
evaluates the `<` expression, and returns that value as the value
of the `and` expression.

This way, we avoid an error.

Here is a short test of the `defuns-per-range` function. First,
evaluate the expression that binds (a shortened)
`top-of-ranges` list to the list of values, then evaluate the
expression for binding the `sorted-lengths` list, and then
evaluate the `defuns-per-range` function.

```
;; (Shorter list than we will use later.)
(setq top-of-ranges
 '(110 120 130 140 150
   160 170 180 190 200))

(setq sorted-lengths
      '(85 86 110 116 122 129 154 176 179 200 265 300 300))

(defuns-per-range sorted-lengths top-of-ranges)
```

The list returned looks like this:

```
(2 2 2 0 0 1 0 2 0 0 4)
```

Indeed, there are two elements of the `sorted-lengths` list
smaller than 110, two elements between 110 and 119, two elements
between 120 and 129, and so on. There are four elements with a value
of 200 or larger.

---

Next: [Your `.emacs` File](#Emacs-Initialization), Previous: [Counting Words in a `defun`](#Words-in-a-defun), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 15 Readying a Graph

Our goal is to construct a graph showing the numbers of function
definitions of various lengths in the Emacs lisp sources.

As a practical matter, if you were creating a graph, you would
probably use a program such as `gnuplot` to do the job.
(`gnuplot` is nicely integrated into GNU Emacs.) In this case,
however, we create one from scratch, and in the process we will
re-acquaint ourselves with some of what we learned before and learn
more.

In this chapter, we will first write a simple graph printing function.
This first definition will be a *prototype*, a rapidly written
function that enables us to reconnoiter this unknown graph-making
territory. We will discover dragons, or find that they are myth.
After scouting the terrain, we will feel more confident and enhance
the function to label the axes automatically.

* [Printing the Columns of a Graph](#Columns-of-a-graph)
* [The `graph-body-print` Function](#graph_002dbody_002dprint)
* [The `recursive-graph-body-print` Function](#recursive_002dgraph_002dbody_002dprint)
* [Need for Printed Axes](#Printed-Axes)
* [Exercise](#Line-Graph-Exercise)

---

Next: [The `graph-body-print` Function](#graph_002dbody_002dprint), Up: [Readying a Graph](#Readying-a-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Printing the Columns of a Graph

Since Emacs is designed to be flexible and work with all kinds of
terminals, including character-only terminals, the graph will need to
be made from one of the typewriter symbols. An asterisk will do; as
we enhance the graph-printing function, we can make the choice of
symbol a user option.

We can call this function `graph-body-print`; it will take a
`numbers-list` as its only argument. At this stage, we will not
label the graph, but only print its body.

The `graph-body-print` function inserts a vertical column of
asterisks for each element in the `numbers-list`. The height of
each line is determined by the value of that element of the
`numbers-list`.

Inserting columns is a repetitive act; that means that this function can
be written either with a `while` loop or recursively.

Our first challenge is to discover how to print a column of asterisks.
Usually, in Emacs, we print characters onto a screen horizontally,
line by line, by typing. We have two routes we can follow: write our
own column-insertion function or discover whether one exists in Emacs.

To see whether there is one in Emacs, we can use the `M-x apropos`
command. This command is like the `C-h a` (`command-apropos`)
command, except that the latter finds only those functions that are
commands. The `M-x apropos` command lists all symbols that match
a regular expression, including functions that are not interactive.

What we want to look for is some command that prints or inserts
columns. Very likely, the name of the function will contain either
the word “print” or the word “insert” or the word “column”.
Therefore, we can simply type `M-x apropos RET
print\|insert\|column RET` and look at the result. On my system, this
command once took quite some time, and then produced a list of 79
functions and variables. Now it does not take much time at all and
produces a list of 211 functions and variables. Scanning down the
list, the only function that looks as if it might do the job is
`insert-rectangle`.

Indeed, this is the function we want; its documentation says:

```
insert-rectangle:
Insert text of RECTANGLE with upper left corner at point.
RECTANGLE's first line is inserted at point,
its second line is inserted at a point vertically under point, etc.
RECTANGLE should be a list of strings.
After this command, the mark is at the upper left corner
and point is at the lower right corner.
```

We can run a quick test, to make sure it does what we expect of it.

Here is the result of placing the cursor after the
`insert-rectangle` expression and typing `C-u C-x C-e`
(`eval-last-sexp`). The function inserts the strings
‘`"first"`’, ‘`"second"`’, and ‘`"third"`’ at and below
point. Also the function returns `nil`.

```
(insert-rectangle '("first" "second" "third"))first
                                              second
                                              thirdnil
```

Of course, we won’t be inserting the text of the
`insert-rectangle` expression itself into the buffer in which we
are making the graph, but will call the function from our program. We
shall, however, have to make sure that point is in the buffer at the
place where the `insert-rectangle` function will insert its
column of strings.

If you are reading this in Info, you can see how this works by
switching to another buffer, such as the `*scratch*` buffer,
placing point somewhere in the buffer, typing `M-:`, typing the
`insert-rectangle` expression into the minibuffer at the prompt,
and then typing `RET`. This causes Emacs to evaluate the
expression in the minibuffer, but to use as the value of point the
position of point in the `*scratch*` buffer. (`M-:` is the
key binding for `eval-expression`. Also, `nil` does not
appear in the `*scratch*` buffer since the expression is
evaluated in the minibuffer.)

We find when we do this that point ends up at the end of the last
inserted line—that is to say, this function moves point as a
side-effect. If we were to repeat the command, with point at this
position, the next insertion would be below and to the right of the
previous insertion. We don’t want this! If we are going to make a
bar graph, the columns need to be beside each other.

So we discover that each cycle of the column-inserting `while`
loop must reposition point to the place we want it, and that place
will be at the top, not the bottom, of the column. Moreover, we
remember that when we print a graph, we do not expect all the columns
to be the same height. This means that the top of each column may be
at a different height from the previous one. We cannot simply
reposition point to the same line each time, but moved over to the
right—or perhaps we can…

We are planning to make the columns of the bar graph out of asterisks.
The number of asterisks in the column is the number specified by the
current element of the `numbers-list`. We need to construct a
list of asterisks of the right length for each call to
`insert-rectangle`. If this list consists solely of the requisite
number of asterisks, then we will have to position point the right number
of lines above the base for the graph to print correctly. This could
be difficult.

Alternatively, if we can figure out some way to pass
`insert-rectangle` a list of the same length each time, then we
can place point on the same line each time, but move it over one
column to the right for each new column. If we do this, however, some
of the entries in the list passed to `insert-rectangle` must be
blanks rather than asterisks. For example, if the maximum height of
the graph is 5, but the height of the column is 3, then
`insert-rectangle` requires an argument that looks like this:

```
(" " " " "*" "*" "*")
```

This last proposal is not so difficult, so long as we can determine
the column height. There are two ways for us to specify the column
height: we can arbitrarily state what it will be, which would work
fine for graphs of that height; or we can search through the list of
numbers and use the maximum height of the list as the maximum height
of the graph. If the latter operation were difficult, then the former
procedure would be easiest, but there is a function built into Emacs
that determines the maximum of its arguments. We can use that
function. The function is called `max` and it returns the
largest of all its arguments, which must be numbers. Thus, for
example,

```
(max  3 4 6 5 7 3)
```

returns 7. (A corresponding function called `min` returns the
smallest of all its arguments.)

However, we cannot simply call `max` on the `numbers-list`;
the `max` function expects numbers as its argument, not a list of
numbers. Thus, the following expression,

```
(max  '(3 4 6 5 7 3))
```

produces the following error message;

```
Wrong type of argument:  number-or-marker-p, (3 4 6 5 7 3)
```

We need a function that passes a list of arguments to a function.
This function is `apply`. This function applies its first
argument (a function) to its remaining arguments, the last of which
may be a list.

For example,

```
(apply 'max 3 4 7 3 '(4 8 5))
```

returns 8.

(Incidentally, I don’t know how you would learn of this function
without a book such as this. It is possible to discover other
functions, like `search-forward` or `insert-rectangle`, by
guessing at a part of their names and then using `apropos`. Even
though its base in metaphor is clear—apply its first argument to
the rest—I doubt a novice would come up with that particular word
when using `apropos` or other aid. Of course, I could be wrong;
after all, the function was first named by someone who had to invent
it.)

The second and subsequent arguments to `apply` are optional, so
we can use `apply` to call a function and pass the elements of a
list to it, like this, which also returns 8:

```
(apply 'max '(4 8 5))
```

This latter way is how we will use `apply`. The
`recursive-lengths-list-many-files` function returns a numbers’
list to which we can apply `max` (we could also apply `max` to
the sorted numbers’ list; it does not matter whether the list is
sorted or not.)

Hence, the operation for finding the maximum height of the graph is this:

```
(setq max-graph-height (apply 'max numbers-list))
```

Now we can return to the question of how to create a list of strings
for a column of the graph. Told the maximum height of the graph
and the number of asterisks that should appear in the column, the
function should return a list of strings for the
`insert-rectangle` command to insert.

Each column is made up of asterisks or blanks. Since the function is
passed the value of the height of the column and the number of
asterisks in the column, the number of blanks can be found by
subtracting the number of asterisks from the height of the column.
Given the number of blanks and the number of asterisks, two
`while` loops can be used to construct the list:

```
;;; First version.
(defun column-of-graph (max-graph-height actual-height)
  "Return list of strings that is one column of a graph."
  (let ((insert-list nil)
        (number-of-top-blanks
         (- max-graph-height actual-height)))
```

```

```

```
    ;; Fill in asterisks.
    (while (> actual-height 0)
      (setq insert-list (cons "*" insert-list))
      (setq actual-height (1- actual-height)))
```

```

```

```
    ;; Fill in blanks.
    (while (> number-of-top-blanks 0)
      (setq insert-list (cons " " insert-list))
      (setq number-of-top-blanks
            (1- number-of-top-blanks)))
```

```

```

```
    ;; Return whole list.
    insert-list))
```

If you install this function and then evaluate the following
expression you will see that it returns the list as desired:

```
(column-of-graph 5 3)
```

returns

```
(" " " " "*" "*" "*")
```

As written, `column-of-graph` contains a major flaw: the symbols
used for the blank and for the marked entries in the column are
hard-coded as a space and asterisk. This is fine for a prototype,
but you, or another user, may wish to use other symbols. For example,
in testing the graph function, you may want to use a period in place
of the space, to make sure the point is being repositioned properly
each time the `insert-rectangle` function is called; or you might
want to substitute a ‘`+`’ sign or other symbol for the asterisk.
You might even want to make a graph-column that is more than one
display column wide. The program should be more flexible. The way to
do that is to replace the blank and the asterisk with two variables
that we can call `graph-blank` and `graph-symbol` and define
those variables separately.

Also, the documentation is not well written. These considerations
lead us to the second version of the function:

```
(defvar graph-symbol "*"
  "String used as symbol in graph, usually an asterisk.")
```

```

```

```
(defvar graph-blank " "
  "String used as blank in graph, usually a blank space.
graph-blank must be the same number of columns wide
as graph-symbol.")
```

(For an explanation of `defvar`, see
[Initializing a Variable with `defvar`](#defvar).)

```
;;; Second version.
(defun column-of-graph (max-graph-height actual-height)
  "Return MAX-GRAPH-HEIGHT strings; ACTUAL-HEIGHT are graph-symbols.
```

```
The graph-symbols are contiguous entries at the end
of the list.
The list will be inserted as one column of a graph.
The strings are either graph-blank or graph-symbol."
```

```

```

```
  (let ((insert-list nil)
        (number-of-top-blanks
         (- max-graph-height actual-height)))
```

```

```

```
    ;; Fill in graph-symbols.
    (while (> actual-height 0)
      (setq insert-list (cons graph-symbol insert-list))
      (setq actual-height (1- actual-height)))
```

```

```

```
    ;; Fill in graph-blanks.
    (while (> number-of-top-blanks 0)
      (setq insert-list (cons graph-blank insert-list))
      (setq number-of-top-blanks
            (1- number-of-top-blanks)))

    ;; Return whole list.
    insert-list))
```

If we wished, we could rewrite `column-of-graph` a third time to
provide optionally for a line graph as well as for a bar graph. This
would not be hard to do. One way to think of a line graph is that it
is no more than a bar graph in which the part of each bar that is
below the top is blank. To construct a column for a line graph, the
function first constructs a list of blanks that is one shorter than
the value, then it uses `cons` to attach a graph symbol to the
list; then it uses `cons` again to attach the top blanks to
the list.

It is easy to see how to write such a function, but since we don’t
need it, we will not do it. But the job could be done, and if it were
done, it would be done with `column-of-graph`. Even more
important, it is worth noting that few changes would have to be made
anywhere else. The enhancement, if we ever wish to make it, is
simple.

Now, finally, we come to our first actual graph printing function.
This prints the body of a graph, not the labels for the vertical and
horizontal axes, so we can call this `graph-body-print`.

---

Next: [The `recursive-graph-body-print` Function](#recursive_002dgraph_002dbody_002dprint), Previous: [Printing the Columns of a Graph](#Columns-of-a-graph), Up: [Readying a Graph](#Readying-a-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.1 The `graph-body-print` Function

After our preparation in the preceding section, the
`graph-body-print` function is straightforward. The function
will print column after column of asterisks and blanks, using the
elements of a numbers’ list to specify the number of asterisks in each
column. This is a repetitive act, which means we can use a
decrementing `while` loop or recursive function for the job. In
this section, we will write the definition using a `while` loop.

The `column-of-graph` function requires the height of the graph
as an argument, so we should determine and record that as a local variable.

This leads us to the following template for the `while` loop
version of this function:

```
(defun graph-body-print (numbers-list)
  "documentation…"
  (let ((height  …
         …))
```

```

```

```
    (while numbers-list
      insert-columns-and-reposition-point
      (setq numbers-list (cdr numbers-list)))))
```

We need to fill in the slots of the template.

Clearly, we can use the `(apply 'max numbers-list)` expression to
determine the height of the graph.

The `while` loop will cycle through the `numbers-list` one
element at a time. As it is shortened by the `(setq numbers-list
(cdr numbers-list))` expression, the CAR of each instance of the
list is the value of the argument for `column-of-graph`.

At each cycle of the `while` loop, the `insert-rectangle`
function inserts the list returned by `column-of-graph`. Since
the `insert-rectangle` function moves point to the lower right of
the inserted rectangle, we need to save the location of point at the
time the rectangle is inserted, move back to that position after the
rectangle is inserted, and then move horizontally to the next place
from which `insert-rectangle` is called.

If the inserted columns are one character wide, as they will be if
single blanks and asterisks are used, the repositioning command is
simply `(forward-char 1)`; however, the width of a column may be
greater than one. This means that the repositioning command should be
written `(forward-char symbol-width)`. The `symbol-width`
itself is the length of a `graph-blank` and can be found using
the expression `(length graph-blank)`. The best place to bind
the `symbol-width` variable to the value of the width of graph
column is in the varlist of the `let` expression.

These considerations lead to the following function definition:

```
(defun graph-body-print (numbers-list)
  "Print a bar graph of the NUMBERS-LIST.
The numbers-list consists of the Y-axis values."

  (let ((height (apply 'max numbers-list))
        (symbol-width (length graph-blank))
        from-position)
```

```

```

```
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
```

```
    ;; Place point for X axis labels.
    (forward-line height)
    (insert "\n")
))
```

The one unexpected expression in this function is the
`(sit-for 0)` expression in the `while` loop. This
expression makes the graph printing operation more interesting to
watch than it would be otherwise. The expression causes Emacs to
*sit* or do nothing for a zero length of time and then redraw the
screen. Placed here, it causes Emacs to redraw the screen column by
column. Without it, Emacs would not redraw the screen until the
function exits.

We can test `graph-body-print` with a short list of numbers.

1. Install `graph-symbol`, `graph-blank`,
   `column-of-graph`, which are in
   [Printing the Columns of a Graph](#Columns-of-a-graph),
   and `graph-body-print`.
2. Copy the following expression:

   ```
   (graph-body-print '(1 2 3 4 6 4 3 5 7 6 5 2 3))
   ```
3. Switch to the `*scratch*` buffer and place the cursor where you
   want the graph to start.
4. Type `M-:` (`eval-expression`).
5. Yank the `graph-body-print` expression into the minibuffer
   with `C-y` (`yank)`.
6. Press `RET` to evaluate the `graph-body-print` expression.

Emacs will print a graph like this:

```
                    *
                *   **
                *  ****
               *** ****
              ********* *
             ************
            *************
```

---

Next: [Need for Printed Axes](#Printed-Axes), Previous: [The `graph-body-print` Function](#graph_002dbody_002dprint), Up: [Readying a Graph](#Readying-a-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.2 The `recursive-graph-body-print` Function

The `graph-body-print` function may also be written recursively.
The recursive solution is divided into two parts: an outside wrapper
that uses a `let` expression to determine the values of several
variables that need only be found once, such as the maximum height of
the graph, and an inside function that is called recursively to print
the graph.

The wrapper is uncomplicated:

```
(defun recursive-graph-body-print (numbers-list)
  "Print a bar graph of the NUMBERS-LIST.
The numbers-list consists of the Y-axis values."
  (let ((height (apply 'max numbers-list))
        (symbol-width (length graph-blank))
        from-position)
    (recursive-graph-body-print-internal
     numbers-list
     height
     symbol-width)))
```

The recursive function is a little more difficult. It has four parts:
the do-again-test, the printing code, the recursive call, and the
next-step-expression. The do-again-test is a `when`
expression that determines whether the `numbers-list` contains
any remaining elements; if it does, the function prints one column of
the graph using the printing code and calls itself again. The
function calls itself again according to the value produced by the
next-step-expression which causes the call to act on a shorter
version of the `numbers-list`.

```
(defun recursive-graph-body-print-internal
  (numbers-list height symbol-width)
  "Print a bar graph.
Used within recursive-graph-body-print function."
```

```

```

```
  (when numbers-list
        (setq from-position (point))
        (insert-rectangle
         (column-of-graph height (car numbers-list)))
```

```
        (goto-char from-position)
        (forward-char symbol-width)
        (sit-for 0)     ; Draw graph column by column.
        (recursive-graph-body-print-internal
         (cdr numbers-list) height symbol-width)))
```

After installation, this expression can be tested; here is a sample:

```
(recursive-graph-body-print '(3 2 5 6 7 5 3 4 6 4 3 2 1))
```

Here is what `recursive-graph-body-print` produces:

```
                *
               **   *
              ****  *
              **** ***
            * *********
            ************
            *************
```

Either of these two functions, `graph-body-print` or
`recursive-graph-body-print`, create the body of a graph.

---

Next: [Exercise](#Line-Graph-Exercise), Previous: [The `recursive-graph-body-print` Function](#recursive_002dgraph_002dbody_002dprint), Up: [Readying a Graph](#Readying-a-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.3 Need for Printed Axes

A graph needs printed axes, so you can orient yourself. For a do-once
project, it may be reasonable to draw the axes by hand using Emacs’s
Picture mode; but a graph drawing function may be used more than once.

For this reason, I have written enhancements to the basic
`print-graph-body` function that automatically print labels for
the horizontal and vertical axes. Since the label printing functions
do not contain much new material, I have placed their description in
an appendix. See [A Graph with Labeled Axes](#Full-Graph).

---

Previous: [Need for Printed Axes](#Printed-Axes), Up: [Readying a Graph](#Readying-a-Graph)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 15.4 Exercise

Write a line graph version of the graph printing functions.

---

Next: [Debugging](#Debugging), Previous: [Readying a Graph](#Readying-a-Graph), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 16 Your `.emacs` File

“You don’t have to like Emacs to like it”—this seemingly
paradoxical statement is the secret of GNU Emacs. The plain, out-of-the-box
Emacs is a generic tool. Most people who use it customize
it to suit themselves.

GNU Emacs is mostly written in Emacs Lisp; this means that by writing
expressions in Emacs Lisp you can change or extend Emacs.

* [Emacs’s Default Configuration](#Default-Configuration)
* [Site-wide Initialization Files](#Site_002dwide-Init)
* [Specifying Variables using `defcustom`](#defcustom)
* [Beginning a `.emacs` File](#Beginning-init-File)
* [Text and Auto Fill Mode](#Text-and-Auto_002dfill)
* [Mail Aliases](#Mail-Aliases)
* [Indent Tabs Mode](#Indent-Tabs-Mode)
* [Some Key Bindings](#Key-Bindings)
* [Keymaps](#Keymaps)
* [Loading Files](#Loading-Files)
* [Autoloading](#Autoload)
* [A Simple Extension: `line-to-top-of-window`](#Simple-Extension)
* [X11 Colors](#X11-Colors)
* [Miscellaneous Settings for a `.emacs` File](#Miscellaneous)
* [A Modified Mode Line](#Mode-Line)

---

Next: [Site-wide Initialization Files](#Site_002dwide-Init), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Emacs’s Default Configuration

There are those who appreciate Emacs’s default configuration. After
all, Emacs starts you in C mode when you edit a C file, starts you in
Fortran mode when you edit a Fortran file, and starts you in
Fundamental mode when you edit an unadorned file. This all makes
sense, if you do not know who is going to use Emacs. Who knows what a
person hopes to do with an unadorned file? Fundamental mode is the
right default for such a file, just as C mode is the right default for
editing C code. (Enough programming languages have syntaxes
that enable them to share or nearly share features, so C mode is
now provided by CC mode, the C Collection.)

But when you do know who is going to use Emacs—you,
yourself—then it makes sense to customize Emacs.

For example, I seldom want Fundamental mode when I edit an
otherwise undistinguished file; I want Text mode. This is why I
customize Emacs: so it suits me.

You can customize and extend Emacs by writing or adapting a
`~/.emacs` file. This is your personal initialization file; its
contents, written in Emacs Lisp, tell Emacs what to do.[18](#FOOT18)

A `~/.emacs` file contains Emacs Lisp code. You can write this
code yourself; or you can use Emacs’s `customize` feature to write
the code for you. You can combine your own expressions and
auto-written Customize expressions in your `.emacs` file.

(I myself prefer to write my own expressions, except for those,
particularly fonts, that I find easier to manipulate using the
`customize` command. I combine the two methods.)

Most of this chapter is about writing expressions yourself. It
describes a simple `.emacs` file; for more information, see
[The Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-File) in The GNU Emacs Manual, and
[The Init File](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Init-File) in The GNU Emacs Lisp Reference
Manual.

---

Next: [Specifying Variables using `defcustom`](#defcustom), Previous: [Emacs’s Default Configuration](#Default-Configuration), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.1 Site-wide Initialization Files

In addition to your personal initialization file, Emacs automatically
loads various site-wide initialization files, if they exist. These
have the same form as your `.emacs` file, but are loaded by
everyone.

Two site-wide initialization files, `site-load.el` and
`site-init.el`, are loaded into Emacs and then dumped if a
dumped version of Emacs is created, as is most common. (Dumped
copies of Emacs load more quickly. However, once a file is loaded and
dumped, a change to it does not lead to a change in Emacs unless you
load it yourself or re-dump Emacs. See [Building
Emacs](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Building-Emacs) in The GNU Emacs Lisp Reference Manual, and the
`INSTALL` file.)

Three other site-wide initialization files are loaded automatically
each time you start Emacs, if they exist. These are
`site-start.el`, which is loaded *before* your `.emacs`
file, and `default.el`, and the terminal type file, which are both
loaded *after* your `.emacs` file.

Settings and definitions in your `.emacs` file will overwrite
conflicting settings and definitions in a `site-start.el` file,
if it exists; but the settings and definitions in a `default.el`
or terminal type file will overwrite those in your `.emacs` file.
(You can prevent interference from a terminal type file by setting
`term-file-prefix` to `nil`. See [A
Simple Extension](#Simple-Extension).)

The `INSTALL` file that comes in the distribution contains
descriptions of the `site-init.el` and `site-load.el` files.

The `loadup.el`, `startup.el`, and `loaddefs.el` files
control loading. These files are in the `lisp` directory of the
Emacs distribution and are worth perusing.

The `loaddefs.el` file contains a good many suggestions as to
what to put into your own `.emacs` file, or into a site-wide
initialization file.

---

Next: [Beginning a `.emacs` File](#Beginning-init-File), Previous: [Site-wide Initialization Files](#Site_002dwide-Init), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.2 Specifying Variables using `defcustom`

You can specify variables using `defcustom` so that you and
others can then use Emacs’s `customize` feature to set their
values. (You cannot use `customize` to write function
definitions; but you can write `defuns` in your `.emacs`
file. Indeed, you can write any Lisp expression in your `.emacs`
file.)

The `customize` feature depends on the `defcustom` macro.
Although you can use `defvar` or `setq` for variables that
users set, the `defcustom` macro is designed for the job.

You can use your knowledge of `defvar` for writing the
first three arguments for `defcustom`. The first argument to
`defcustom` is the name of the variable. The second argument is
the variable’s initial value, if any; and this value is set only if
the value has not already been set. The third argument is the
documentation.

The fourth and subsequent arguments to `defcustom` specify types
and options; these are not featured in `defvar`. (These
arguments are optional.)

Each of these arguments consists of a keyword followed by a value.
Each keyword starts with the colon character ‘`:`’.

For example, the customizable user option variable
`text-mode-hook` looks like this:

```
(defcustom text-mode-hook nil
  "Normal hook run when entering Text mode and many related modes."
  :type 'hook
  :options '(turn-on-auto-fill flyspell-mode)
  :group 'wp)
```

The name of the variable is `text-mode-hook`; it has no default
value; and its documentation string tells you what it does.

The `:type` keyword tells Emacs the kind of data to which
`text-mode-hook` should be set and how to display the value in a
Customization buffer.

The `:options` keyword specifies a suggested list of values for
the variable. Usually, `:options` applies to a hook.
The list is only a suggestion; it is not exclusive; a person who sets
the variable may set it to other values; the list shown following the
`:options` keyword is intended to offer convenient choices to a
user.

Finally, the `:group` keyword tells the Emacs Customization
command in which group the variable is located. This tells where to
find it.

The `defcustom` macro recognizes more than a dozen keywords.
For more information, see [Writing Customization
Definitions](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Customization) in The GNU Emacs Lisp Reference Manual.

Consider `text-mode-hook` as an example.

There are two ways to customize this variable. You can use the
customization command or write the appropriate expressions yourself.

Using the customization command, you can type:

```
M-x customize
```

and find that the group for editing files of text is called “Text”.
Enter that group. Text Mode Hook is the first member. You can click
on its various options, such as `turn-on-auto-fill`, to set the
values. After you click on the button to

```
Save for Future Sessions
```

Emacs will write an expression into your `.emacs` file.
It will look like this:

```
(custom-set-variables
  ;; custom-set-variables was added by Custom.
  ;; If you edit it by hand, you could mess it up, so be careful.
  ;; Your init file should contain only one such instance.
  ;; If there is more than one, they won't work right.
 '(text-mode-hook '(turn-on-auto-fill text-mode-hook-identify)))
```

(The `text-mode-hook-identify` function tells
`toggle-text-mode-auto-fill` which buffers are in Text mode.
It comes on automatically.)

The `custom-set-variables` function works somewhat differently
than a `setq`. While I have never learned the differences, I
modify the `custom-set-variables` expressions in my `.emacs`
file by hand: I make the changes in what appears to me to be a
reasonable manner and have not had any problems. Others prefer to use
the Customization command and let Emacs do the work for them.

Another `custom-set-…` function is `custom-set-faces`.
This function sets the various font faces. Over time, I have set a
considerable number of faces. Some of the time, I re-set them using
`customize`; other times, I simply edit the
`custom-set-faces` expression in my `.emacs` file itself.

The second way to customize your `text-mode-hook` is to set it
yourself in your `.emacs` file using code that has nothing to do
with the `custom-set-…` functions.

When you do this, and later use `customize`, you will see a
message that says

```
CHANGED outside Customize; operating on it here may be unreliable.
```

This message is only a warning. If you click on the button to

```
Save for Future Sessions
```

Emacs will write a `custom-set-…` expression near the end
of your `.emacs` file that will be evaluated after your
hand-written expression. It will, therefore, overrule your
hand-written expression. No harm will be done. When you do this,
however, be careful to remember which expression is active; if you
forget, you may confuse yourself.

So long as you remember where the values are set, you will have no
trouble. In any event, the values are always set in your
initialization file, which is usually called `.emacs`.

I myself use `customize` for hardly anything. Mostly, I write
expressions myself.

Incidentally, to be more complete concerning defines: `defsubst`
defines an inline function. The syntax is just like that of
`defun`. `defconst` defines a symbol as a constant. The
intent is that neither programs nor users should ever change a value
set by `defconst`. (You can change it; the value set is a
variable; but please do not.)

---

Next: [Text and Auto Fill Mode](#Text-and-Auto_002dfill), Previous: [Specifying Variables using `defcustom`](#defcustom), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.3 Beginning a `.emacs` File

When you start Emacs, it loads your `.emacs` file unless you tell
it not to by specifying ‘`-q`’ on the command line. (The
`emacs -q` command gives you a plain, out-of-the-box Emacs.)

A `.emacs` file contains Lisp expressions. Often, these are no
more than expressions to set values; sometimes they are function
definitions.

See [The Init File `~/.emacs`](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-File) in The GNU Emacs
Manual, for a short description of initialization files.

This chapter goes over some of the same ground, but is a walk among
extracts from a complete, long-used `.emacs` file—my own.

The first part of the file consists of comments: reminders to myself.
By now, of course, I remember these things, but when I started, I did
not.

```
;;;; Bob's .emacs file
; Robert J. Chassell
; 26 September 1985
```

Look at that date! I started this file a long time ago. I have been
adding to it ever since.

```
; Each section in this file is introduced by a
; line beginning with four semicolons; and each
; entry is introduced by a line beginning with
; three semicolons.
```

This describes the usual conventions for comments in Emacs Lisp.
Everything on a line that follows a semicolon is a comment. Two,
three, and four semicolons are used as subsection and section markers.
(See [Comments](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Comments) in The GNU Emacs Lisp Reference Manual, for
more about comments.)

```
;;;; The Help Key
; Control-h is the help key;
; after typing control-h, type a letter to
; indicate the subject about which you want help.
; For an explanation of the help facility,
; type control-h two times in a row.
```

Just remember: type `C-h` two times for help.

```
; To find out about any mode, type control-h m
; while in that mode.  For example, to find out
; about mail mode, enter mail mode and then type
; control-h m.
```

“Mode help”, as I call this, is very helpful. Usually, it tells you
all you need to know.

Of course, you don’t need to include comments like these in your
`.emacs` file. I included them in mine because I kept forgetting
about Mode help or the conventions for comments—but I was able to
remember to look here to remind myself.

---

Next: [Mail Aliases](#Mail-Aliases), Previous: [Beginning a `.emacs` File](#Beginning-init-File), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.4 Text and Auto Fill Mode

Now we come to the part that turns on Text mode and Auto Fill
mode[19](#FOOT19).

```
;;; Text mode and Auto Fill mode
;; The next two lines put Emacs into Text mode
;; and Auto Fill mode, and are for writers who
;; want to start writing prose rather than code.
(setq-default major-mode 'text-mode)
(add-hook 'text-mode-hook 'turn-on-auto-fill)
```

Here is the first part of this `.emacs` file that does something
besides remind a forgetful human!

The first of the two lines in parentheses tells Emacs to turn on Text
mode when you find a file, *unless* that file should go into some
other mode, such as C mode.

When Emacs reads a file, it looks at the extension to the file name,
if any. (The extension is the part that comes after a ‘`.`’.) If
the file ends with a ‘`.c`’ or ‘`.h`’ extension then Emacs turns
on C mode. Also, Emacs looks at first nonblank line of the file; if
the line says ‘`-*- C -*-`’, Emacs turns on C mode. Emacs
possesses a list of extensions and specifications that it uses
automatically. In addition, Emacs looks near the last page for a
per-buffer, local variables list, if any.

Now, back to the `.emacs` file.

Here is the line again; how does it work?

```
(setq-default major-mode 'text-mode)
```

This line is a short, but complete Emacs Lisp expression.

We are already familiar with `setq`. We use a similar macro
`setq-default` to set the following variable,
`major-mode`[20](#FOOT20), to the subsequent value, which is `text-mode`. The
single-quote before `text-mode` tells Emacs to deal directly with
the `text-mode` symbol, not with whatever it might stand for.
See [Setting the Value of a Variable](#setq), for a reminder of how
`setq` works. The main point is that there is no difference
between the procedure you use to set a value in your `.emacs`
file and the procedure you use anywhere else in Emacs.

Here is the next line:

```
(add-hook 'text-mode-hook 'turn-on-auto-fill)
```

In this line, the `add-hook` command adds
`turn-on-auto-fill` to the variable.

`turn-on-auto-fill` is the name of a program, that, you guessed
it!, turns on Auto Fill mode.

Every time Emacs turns on Text mode, Emacs runs the commands hooked
onto Text mode. So every time Emacs turns on Text mode, Emacs also
turns on Auto Fill mode.

In brief, the first line causes Emacs to enter Text mode when you edit a
file, unless the file name extension, a first non-blank line, or local
variables to tell Emacs otherwise.

Text mode among other actions, sets the syntax table to work
conveniently for writers. In Text mode, Emacs considers an apostrophe
as part of a word like a letter; but Emacs does not consider a period
or a space as part of a word. Thus, `M-f` moves you over
‘`it's`’. On the other hand, in C mode, `M-f` stops just after
the ‘`t`’ of ‘`it's`’.

The second line causes Emacs to turn on Auto Fill mode when it turns
on Text mode. In Auto Fill mode, Emacs automatically breaks a line
that is too wide and brings the excessively wide part of the line down
to the next line. Emacs breaks lines between words, not within them.

When Auto Fill mode is turned off, lines continue to the right as you
type them. Depending on how you set the value of
`truncate-lines`, the words you type either disappear off the
right side of the screen, or else are shown, in a rather ugly and
unreadable manner, as a continuation line on the screen.

In addition, in this part of my `.emacs` file, I tell the Emacs
fill commands to insert two spaces after a colon:

```
(setq colon-double-space t)
```

---

Next: [Indent Tabs Mode](#Indent-Tabs-Mode), Previous: [Text and Auto Fill Mode](#Text-and-Auto_002dfill), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.5 Mail Aliases

Here is a `setq` that turns on mail aliases, along with more
reminders.

```
;;; Message mode
; To enter message mode, type 'C-x m'
; To enter RMAIL (for reading mail),
; type 'M-x rmail'
(setq mail-aliases t)
```

This `setq` sets the value of the variable
`mail-aliases` to `t`. Since `t` means true, the line
says, in effect, “Yes, use mail aliases.”

Mail aliases are convenient short names for long email addresses or
for lists of email addresses. The file where you keep your aliases
is `~/.mailrc`. You write an alias like this:

```
alias geo george@foobar.wiz.edu
```

When you write a message to George, address it to ‘`geo`’; the
mailer will automatically expand ‘`geo`’ to the full address.

---

Next: [Some Key Bindings](#Key-Bindings), Previous: [Mail Aliases](#Mail-Aliases), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.6 Indent Tabs Mode

By default, Emacs inserts tabs in place of multiple spaces when it
formats a region. (For example, you might indent many lines of text
all at once with the `indent-region` command.) Tabs look fine on
a terminal or with ordinary printing, but they produce badly indented
output when you use TeX or Texinfo since TeX ignores tabs.

The following turns off Indent Tabs mode:

```
;;; Prevent Extraneous Tabs
(setq-default indent-tabs-mode nil)
```

Note that this line uses `setq-default` rather than the
`setq` that we have seen before; `setq-default`
sets values only in buffers that do not have their own local
values for the variable.

---

Next: [Keymaps](#Keymaps), Previous: [Indent Tabs Mode](#Indent-Tabs-Mode), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.7 Some Key Bindings

Now for some personal key bindings:

```
;;; Compare windows
(keymap-global-set "C-c w" 'compare-windows)
```

`compare-windows` is a nifty command that compares the text in
your current window with text in the next window. It makes the
comparison by starting at point in each window, moving over text in
each window as far as they match. I use this command all the time.

This also shows how to set a key globally, for all modes.

The key setting command is `keymap-global-set`. It is followed by
the key binding. In a `.emacs` file, the keybinding is written as
shown: `C-c` stands for Control-C, which means to press the control
key and the `c` key at the same time. The `w` means to press
the `w` key. The key binding is surrounded by double quotation
marks. (If you were binding a `META` key, rather than a `CTRL`
key, you would write `M-c` in your `.emacs` file.
See [Rebinding Keys in Your Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-Rebinding) in The GNU
Emacs Manual, for details.)

The command invoked by the keys is `compare-windows`. Note that
`compare-windows` is preceded by a single-quote; otherwise, Emacs
would first try to evaluate the symbol to determine its value.

These three things, the double quotation marks, the backslash before
the ‘`C`’, and the single-quote are necessary parts of
key binding that I tend to forget. Fortunately, I have come to
remember that I should look at my existing `.emacs` file, and
adapt what is there.

As for the key binding itself: `C-c w`. This combines the prefix
key, `C-c`, with a single character, in this case, `w`. This
set of keys, `C-c` followed by a single character, is strictly
reserved for individuals’ own use. (I call these *own* keys, since
these are for my own use.) You should always be able to create such a
key binding for your own use without stomping on someone else’s
key binding. If you ever write an extension to Emacs, please avoid
taking any of these keys for public use. Create a key like `C-c
C-w` instead. Otherwise, we will run out of own keys.

Here is another key binding, with a comment:

```
;;; Key binding for 'occur'
; I use occur a lot, so let's bind it to a key:
(keymap-global-set "C-c o" 'occur)
```

The `occur` command shows all the lines in the current buffer
that contain a match for a regular expression. When the region is
active, `occur` restricts matches to such region. Otherwise it
uses the entire buffer.
Matching lines are shown in a buffer called `*Occur*`.
That buffer serves as a menu to jump to occurrences.

Here is how to unbind a key, so it does not
work:

```
;;; Unbind 'C-x f'
(keymap-global-unset "C-x f")
```

There is a reason for this unbinding: I found I inadvertently typed
`C-x f` when I meant to type `C-x C-f`. Rather than find a
file, as I intended, I accidentally set the width for filled text,
almost always to a width I did not want. Since I hardly ever reset my
default width, I simply unbound the key.

The following rebinds an existing key:

```
;;; Rebind 'C-x C-b' for 'buffer-menu'
(keymap-global-set "C-x C-b" 'buffer-menu)
```

By default, `C-x C-b` runs the
`list-buffers` command. This command lists
your buffers in *another* window. Since I
almost always want to do something in that
window, I prefer the `buffer-menu`
command, which not only lists the buffers,
but moves point into that window.

* [Legacy Global Key Binding Commands](#Legacy-Global-Key-Binding-Commands)

#### 16.7.1 Legacy Global Key Binding Commands

Historically, keys are bound globally using a lower-level function,
`global-set-key`, which is now considered legacy. While you are
encouraged to use `keymap-global-set`, you likely would encounter
`global-set-key` in various places. The first example in this
section can be rewritten using `global-set-key` as:

```
(global-set-key "\C-cw" 'compare-windows)
```

It is very similar to `keymap-global-set`, with the keybinding
following a slightly different format. Control-C is represented by
`\C-c`, instead of `C-c`. There is no space between key
strokes, like `\C-c` and `w` in this example. Despite the
difference, in documentation, this is still written as `C-c w`
for readability.

Historically, keys are unbound globally using a lower-function,
`global-unset-key`, which is now considered legacy. Its key
binding format follows that of `global-set-key`. The key unbinding
example in this section can be rewritten as:

```
;;; Unbind 'C-x f'
(global-unset-key "\C-xf")
```

---

Next: [Loading Files](#Loading-Files), Previous: [Some Key Bindings](#Key-Bindings), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.8 Keymaps

Emacs uses *keymaps* to record which keys call which commands.
When you use `keymap-global-set` to set the key binding for a
single command in all parts of Emacs, you are specifying the key binding
in `current-global-map`.

Specific modes, such as C mode or Text mode, have their own keymaps;
the mode-specific keymaps override the global map that is shared by
all buffers.

The `keymap-global-set` function binds, or rebinds, the global
keymap. For example, the following binds the key `C-x C-b` to the
function `buffer-menu`:

```
(keymap-global-set "C-x C-b" 'buffer-menu)
```

Mode-specific keymaps are bound using the `keymap-set` function,
which takes a specific keymap as an argument, as well as the key and
the command. For example, the following expression binds the
`texinfo-insert-@group` command to `C-c C-c g`:

```
(keymap-set texinfo-mode-map "C-c C-c g" 'texinfo-insert-@group)
```

Historically, keymaps are bound using a lower-level function,
`define-key`, which is now considered legacy. While you are
encouraged to use `keymap-set`, you likely would encounter
`define-key` in various places. The above key binding can be
rewritten using `define-key` as:

```
(define-key texinfo-mode-map "\C-c\C-cg" 'texinfo-insert-@group)
```

The `texinfo-insert-@group` function itself is a little extension
to Texinfo mode that inserts ‘`@group`’ into a Texinfo file. I
use this command all the time and prefer to type the three strokes
`C-c C-c g` rather than the six strokes `@ g r o u p`.
(‘`@group`’ and its matching ‘`@end group`’ are commands that
keep all enclosed text together on one page; many multi-line examples
in this book are surrounded by ‘`@group … @end group`’.)

Here is the `texinfo-insert-@group` function definition:

```
(defun texinfo-insert-@group ()
  "Insert the string @group in a Texinfo buffer."
  (interactive)
  (beginning-of-line)
  (insert "@group\n"))
```

(Of course, I could have used Abbrev mode to save typing, rather than
write a function to insert a word; but I prefer key strokes consistent
with other Texinfo mode key bindings.)

You will see numerous `keymap-set` and `define-key`
expressions in `loaddefs.el` as well as in the various mode
libraries, such as `cc-mode.el` and `lisp-mode.el`.

See [Customizing Key Bindings](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Key-Bindings) in The GNU Emacs
Manual, and [Keymaps](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Keymaps) in The GNU Emacs Lisp
Reference Manual, for more information about keymaps.

---

Next: [Autoloading](#Autoload), Previous: [Keymaps](#Keymaps), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.9 Loading Files

Many people in the GNU Emacs community have written extensions to
Emacs. As time goes by, these extensions are often included in new
releases. For example, the Calendar and Diary packages are now part
of the standard GNU Emacs, as is Calc.

You can use a `load` command to evaluate a complete file and
thereby install all the functions and variables in the file into Emacs.
For example:

```
(load "~/emacs/slowsplit")
```

This evaluates, i.e., loads, the `slowsplit.el` file or if it
exists, the faster, byte compiled `slowsplit.elc` file from the
`emacs` sub-directory of your home directory. The file contains
the function `split-window-quietly`, which John Robinson wrote in
1989.

The `split-window-quietly` function splits a window with the
minimum of redisplay. I installed it in 1989 because it worked well
with the slow 1200 baud terminals I was then using. Nowadays, I only
occasionally come across such a slow connection, but I continue to use
the function because I like the way it leaves the bottom half of a
buffer in the lower of the new windows and the top half in the upper
window.

To replace the key binding for the default
`split-window-vertically`, you must bind the keys to
`split-window-quietly`, like this:

```
(keymap-global-set "C-x 2" 'split-window-quietly)
```

If you load many extensions, as I do, then instead of specifying the
exact location of the extension file, as shown above, you can specify
that directory as part of Emacs’s `load-path`. Then, when Emacs
loads a file, it will search that directory as well as its default
list of directories. (The default list is specified in `paths.h`
when Emacs is built.)

The following command adds your `~/emacs` directory to the
existing load path:

```
;;; Emacs Load Path
(setq load-path (cons "~/emacs" load-path))
```

Incidentally, `load-library` is an interactive interface to the
`load` function. The complete function looks like this:

```
(defun load-library (library)
  "Load the Emacs Lisp library named LIBRARY.
This is an interface to the function `load'.  LIBRARY is searched
for in `load-path', both with and without `load-suffixes' (as
well as `load-file-rep-suffixes').

See Info node `(emacs)Lisp Libraries' for more details.
See `load-file' for a different interface to `load'."
  (interactive
   (list (completing-read "Load library: "
                          (apply-partially 'locate-file-completion-table
                                           load-path
                                           (get-load-suffixes)))))
  (load library))
```

The name of the function, `load-library`, comes from the use of
“library” as a conventional synonym for “file”. The source for the
`load-library` command is in the `files.el` library.

Another interactive command that does a slightly different job is
`load-file`. See [Libraries of Lisp Code for
Emacs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Lisp-Libraries) in The GNU Emacs Manual, for information on the
distinction between `load-library` and this command.

---

Next: [A Simple Extension: `line-to-top-of-window`](#Simple-Extension), Previous: [Loading Files](#Loading-Files), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.10 Autoloading

Instead of installing a function by loading the file that contains it,
or by evaluating the function definition, you can make the function
available but not actually install it until it is first called. This
is called *autoloading*.

When you execute an autoloaded function, Emacs automatically evaluates
the file that contains the definition, and then calls the function.

Emacs starts quicker with autoloaded functions, since their libraries
are not loaded right away; but you need to wait a moment when you
first use such a function, while its containing file is evaluated.

Rarely used functions are frequently autoloaded. The
`loaddefs.el` library contains thousands of autoloaded functions,
from `5x5` to `zone`. Of course, you may
come to use a rare function frequently. When you do, you should
load that function’s file with a `load` expression in your
`.emacs` file.

In my `.emacs` file, I load 14 libraries that contain functions
that would otherwise be autoloaded. (Actually, it would have been
better to include these files in my dumped Emacs, but I forgot.
See [Building Emacs](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Building-Emacs) in The GNU Emacs Lisp
Reference Manual, and the `INSTALL` file for more about
dumping.)

You may also want to include autoloaded expressions in your `.emacs`
file. `autoload` is a built-in function that takes up to five
arguments, the final three of which are optional. The first argument
is the name of the function to be autoloaded; the second is the name
of the file to be loaded. The third argument is documentation for the
function, and the fourth tells whether the function can be called
interactively. The fifth argument tells what type of
object—`autoload` can handle a keymap or macro as well as a
function (the default is a function).

Here is a typical example:

```
(autoload 'html-helper-mode
  "html-helper-mode" "Edit HTML documents" t)
```

(`html-helper-mode` is an older alternative to `html-mode`,
which is a standard part of the distribution.)

This expression autoloads the `html-helper-mode` function. It
takes it from the `html-helper-mode.el` file (or from the byte
compiled version `html-helper-mode.elc`, if that exists.) The
file must be located in a directory specified by `load-path`.
The documentation says that this is a mode to help you edit documents
written in the HyperText Markup Language. You can call this mode
interactively by typing `M-x html-helper-mode`. (You need to
duplicate the function’s regular documentation in the autoload
expression because the regular function is not yet loaded, so its
documentation is not available.)

See [Autoload](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Autoload) in The GNU Emacs Lisp Reference
Manual, for more information.

---

Next: [X11 Colors](#X11-Colors), Previous: [Autoloading](#Autoload), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.11 A Simple Extension: `line-to-top-of-window`

Here is a simple extension to Emacs that moves the line that point is
on to the top of the window. I use this all the time, to make text
easier to read.

You can put the following code into a separate file and then load it
from your `.emacs` file, or you can include it within your
`.emacs` file.

Here is the definition:

```
;;; Line to top of window;
;;; replace three keystroke sequence  C-u 0 C-l
(defun line-to-top-of-window ()
  "Move the line that point is on to top of window."
  (interactive)
  (recenter 0))
```

Now for the key binding.

Function keys as well as mouse button events and non-ASCII
characters are written within square brackets, without quotation
marks.

I bind `line-to-top-of-window` to my `F6` function key like
this:

```
(keymap-global-set "<f6>" 'line-to-top-of-window)
```

For more information, see [Rebinding Keys in
Your Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-Rebinding) in The GNU Emacs Manual.

If you run two versions of GNU Emacs, such as versions 27 and 28, and
use one `.emacs` file, you can select which code to evaluate with
the following conditional:

```
(cond
 ((= 27 emacs-major-version)
  ;; evaluate version 27 code
  ( … ))
 ((= 28 emacs-major-version)
  ;; evaluate version 28 code
  ( … )))
```

For example, recent versions blink
their cursors by default. I hate such blinking, as well as other
features, so I placed the following in my `.emacs`
file[21](#FOOT21):

```
(when (>= emacs-major-version 21)
  (blink-cursor-mode 0)
  ;; Insert newline when you press 'C-n' (next-line)
  ;; at the end of the buffer
  (setq next-line-add-newlines t)
```

```
  ;; Turn on image viewing
  (auto-image-file-mode t)
```

```
  ;; Turn on menu bar (this bar has text)
  ;; (Use numeric argument to turn on)
  (menu-bar-mode 1)
```

```
  ;; Turn off tool bar (this bar has icons)
  ;; (Use numeric argument to turn on)
  (tool-bar-mode nil)
```

```
  ;; Turn off tooltip mode for tool bar
  ;; (This mode causes icon explanations to pop up)
  ;; (Use numeric argument to turn on)
  (tooltip-mode nil)
  ;; If tooltips turned on, make tips appear promptly
  (setq tooltip-delay 0.1)  ; default is 0.7 second
   )
```

---

Next: [Miscellaneous Settings for a `.emacs` File](#Miscellaneous), Previous: [A Simple Extension: `line-to-top-of-window`](#Simple-Extension), Up: [Your `.emacs` File](#Emacs-Initialization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
