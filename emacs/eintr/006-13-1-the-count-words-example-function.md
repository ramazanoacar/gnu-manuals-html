### 13.1 The `count-words-example` Function

A word count command could count words in a line, paragraph, region,
or buffer. What should the command cover? You could design the
command to count the number of words in a complete buffer. However,
the Emacs tradition encourages flexibility—you may want to count
words in just a section, rather than all of a buffer. So it makes
more sense to design the command to count the number of words in a
region. Once you have a command to count words in a region, you can,
if you wish, count words in a whole buffer by marking it with
`C-x h` (`mark-whole-buffer`).

Clearly, counting words is a repetitive act: starting from the
beginning of the region, you count the first word, then the second
word, then the third word, and so on, until you reach the end of the
region. This means that word counting is ideally suited to recursion
or to a `while` loop.

* [Designing `count-words-example`](#Design-count_002dwords_002dexample)
* [The Whitespace Bug in `count-words-example`](#Whitespace-Bug)

---

Next: [The Whitespace Bug in `count-words-example`](#Whitespace-Bug), Up: [The `count-words-example` Function](#count_002dwords_002dexample)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Designing `count-words-example`

First, we will implement the word count command with a `while`
loop, then with recursion. The command will, of course, be
interactive.

The template for an interactive function definition is, as always:

```
(defun name-of-function (argument-list)
  "documentation…"
  (interactive-expression…)
  body…)
```

What we need to do is fill in the slots.

The name of the function should be self-explanatory and easy
to remember. `count-words-region` is the obvious choice. Since
that name is used for the standard Emacs command to count words, we
will name our implementation `count-words-example`.

The function counts words within a region. This means that the
argument list must contain symbols that are bound to the two
positions, the beginning and end of the region. These two positions
can be called ‘`beginning`’ and ‘`end`’ respectively. The first
line of the documentation should be a single sentence, since that is
all that is printed as documentation by a command such as
`apropos`. The interactive expression will be of the form
‘`(interactive "r")`’, since that will cause Emacs to pass the
beginning and end of the region to the function’s argument list. All
this is routine.

The body of the function needs to be written to do three tasks:
first, to set up conditions under which the `while` loop can
count words, second, to run the `while` loop, and third, to send
a message to the user.

When a user calls `count-words-example`, point may be at the
beginning or the end of the region. However, the counting process
must start at the beginning of the region. This means we will want
to put point there if it is not already there. Executing
`(goto-char beginning)` ensures this. Of course, we will want to
return point to its expected position when the function finishes its
work. For this reason, the body must be enclosed in a
`save-excursion` expression.

The central part of the body of the function consists of a
`while` loop in which one expression jumps point forward word by
word, and another expression counts those jumps. The true-or-false-test
of the `while` loop should test true so long as point should jump
forward, and false when point is at the end of the region.

We could use `(forward-word 1)` as the expression for moving point
forward word by word, but it is easier to see what Emacs identifies as a
“word” if we use a regular expression search.

A regular expression search that finds the pattern for which it is
searching leaves point after the last character matched. This means
that a succession of successful word searches will move point forward
word by word.

As a practical matter, we want the regular expression search to jump
over whitespace and punctuation between words as well as over the
words themselves. A regexp that refuses to jump over interword
whitespace would never jump more than one word! This means that
the regexp should include the whitespace and punctuation that follows
a word, if any, as well as the word itself. (A word may end a buffer
and not have any following whitespace or punctuation, so that part of
the regexp must be optional.)

Thus, what we want for the regexp is a pattern defining one or more
word constituent characters followed, optionally, by one or more
characters that are not word constituents. The regular expression for
this is:

```
\w+\W*
```

The buffer’s syntax table determines which characters are and are not
word constituents. For more information about syntax,
see [Syntax Tables](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Syntax-Tables) in The GNU Emacs Lisp
Reference Manual.

The search expression looks like this:

```
(re-search-forward "\\w+\\W*")
```

(Note that paired backslashes precede the ‘`w`’ and ‘`W`’. A
single backslash has special meaning to the Emacs Lisp interpreter.
It indicates that the following character is interpreted differently
than usual. For example, the two characters, ‘`\n`’, stand for
‘`newline`’, rather than for a backslash followed by ‘`n`’. Two
backslashes in a row stand for an ordinary, unspecial backslash, so
Emacs Lisp interpreter ends of seeing a single backslash followed by a
letter. So it discovers the letter is special.)

We need a counter to count how many words there are; this variable
must first be set to 0 and then incremented each time Emacs goes
around the `while` loop. The incrementing expression is simply:

```
(setq count (1+ count))
```

Finally, we want to tell the user how many words there are in the
region. The `message` function is intended for presenting this
kind of information to the user. The message has to be phrased so
that it reads properly regardless of how many words there are in the
region: we don’t want to say that “there are 1 words in the region”.
The conflict between singular and plural is ungrammatical. We can
solve this problem by using a conditional expression that evaluates
different messages depending on the number of words in the region.
There are three possibilities: no words in the region, one word in the
region, and more than one word. This means that the `cond`
special form is appropriate.

All this leads to the following function definition:

```
;;; First version; has bugs!
(defun count-words-example (beginning end)
  "Print number of words in the region.
Words are defined as at least one word-constituent
character followed by at least one character that
is not a word-constituent.  The buffer's syntax
table determines which characters these are."
  (interactive "r")
  (message "Counting words in region ... ")
```

```

```

```
;;; 1. Set up appropriate conditions.
  (save-excursion
    (goto-char beginning)
    (let ((count 0))
```

```

```

```
;;; 2. Run the while loop.
      (while (< (point) end)
        (re-search-forward "\\w+\\W*")
        (setq count (1+ count)))
```

```

```

```
;;; 3. Send a message to the user.
      (cond ((zerop count)
             (message
              "The region does NOT have any words."))
            ((= 1 count)
             (message
              "The region has 1 word."))
            (t
             (message
              "The region has %d words." count))))))
```

As written, the function works, but not in all circumstances.

---

Previous: [Designing `count-words-example`](#Design-count_002dwords_002dexample), Up: [The `count-words-example` Function](#count_002dwords_002dexample)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 13.1.1 The Whitespace Bug in `count-words-example`

The `count-words-example` command described in the preceding
section has two bugs, or rather, one bug with two manifestations.
First, if you mark a region containing only whitespace in the middle
of some text, the `count-words-example` command tells you that the
region contains one word! Second, if you mark a region containing
only whitespace at the end of the buffer or the accessible portion of
a narrowed buffer, the command displays an error message that looks
like this:

```
Search failed: "\\w+\\W*"
```

If you are reading this in Info in GNU Emacs, you can test for these
bugs yourself.

First, evaluate the function in the usual manner to install it.

If you wish, you can also install this key binding by evaluating it:

```
(keymap-global-set "C-c =" 'count-words-example)
```

To conduct the first test, set mark and point to the beginning and end
of the following line and then type `C-c =` (or `M-x
count-words-example` if you have not bound `C-c =`):

```
    one   two  three
```

Emacs will tell you, correctly, that the region has three words.

Repeat the test, but place mark at the beginning of the line and place
point just *before* the word ‘`one`’. Again type the command
`C-c =` (or `M-x count-words-example`). Emacs should tell you
that the region has no words, since it is composed only of the
whitespace at the beginning of the line. But instead Emacs tells you
that the region has one word!

For the third test, copy the sample line to the end of the
`*scratch*` buffer and then type several spaces at the end of the
line. Place mark right after the word ‘`three`’ and point at the
end of line. (The end of the line will be the end of the buffer.)
Type `C-c =` (or `M-x count-words-example`) as you did before.
Again, Emacs should tell you that the region has no words, since it is
composed only of the whitespace at the end of the line. Instead,
Emacs displays an error message saying ‘`Search failed`’.

The two bugs stem from the same problem.

Consider the first manifestation of the bug, in which the command
tells you that the whitespace at the beginning of the line contains
one word. What happens is this: The `M-x count-words-example`
command moves point to the beginning of the region. The `while`
tests whether the value of point is smaller than the value of
`end`, which it is. Consequently, the regular expression search
looks for and finds the first word. It leaves point after the word.
`count` is set to one. The `while` loop repeats; but this
time the value of point is larger than the value of `end`, the
loop is exited; and the function displays a message saying the number
of words in the region is one. In brief, the regular expression
search looks for and finds the word even though it is outside
the marked region.

In the second manifestation of the bug, the region is whitespace at
the end of the buffer. Emacs says ‘`Search failed`’. What happens
is that the true-or-false-test in the `while` loop tests true, so
the search expression is executed. But since there are no more words
in the buffer, the search fails.

In both manifestations of the bug, the search extends or attempts to
extend outside of the region.

The solution is to limit the search to the region—this is a fairly
simple action, but as you may have come to expect, it is not quite as
simple as you might think.

As we have seen, the `re-search-forward` function takes a search
pattern as its first argument. But in addition to this first,
mandatory argument, it accepts three optional arguments. The optional
second argument bounds the search. The optional third argument, if
`t`, causes the function to return `nil` rather than signal
an error if the search fails. The optional fourth argument is a
repeat count. (In Emacs, you can see a function’s documentation by
typing `C-h f`, the name of the function, and then `RET`.)

In the `count-words-example` definition, the value of the end of
the region is held by the variable `end` which is passed as an
argument to the function. Thus, we can add `end` as an argument
to the regular expression search expression:

```
(re-search-forward "\\w+\\W*" end)
```

However, if you make only this change to the `count-words-example`
definition and then test the new version of the definition on a
stretch of whitespace, you will receive an error message saying
‘`Search failed`’.

What happens is this: the search is limited to the region, and fails
as you expect because there are no word-constituent characters in the
region. Since it fails, we receive an error message. But we do not
want to receive an error message in this case; we want to receive the
message “The region does NOT have any words.”

The solution to this problem is to provide `re-search-forward`
with a third argument of `t`, which causes the function to return
`nil` rather than signal an error if the search fails.

However, if you make this change and try it, you will see the message
“Counting words in region ... ” and … you will keep on seeing
that message …, until you type `C-g` (`keyboard-quit`).

Here is what happens: the search is limited to the region, as before,
and it fails because there are no word-constituent characters in the
region, as expected. Consequently, the `re-search-forward`
expression returns `nil`. It does nothing else. In particular,
it does not move point, which it does as a side effect if it finds the
search target. After the `re-search-forward` expression returns
`nil`, the next expression in the `while` loop is evaluated.
This expression increments the count. Then the loop repeats. The
true-or-false-test tests true because the value of point is still less
than the value of end, since the `re-search-forward` expression
did not move point. … and the cycle repeats …

The `count-words-example` definition requires yet another
modification, to cause the true-or-false-test of the `while` loop
to test false if the search fails. Put another way, there are two
conditions that must be satisfied in the true-or-false-test before the
word count variable is incremented: point must still be within the
region and the search expression must have found a word to count.

Since both the first condition and the second condition must be true
together, the two expressions, the region test and the search
expression, can be joined with an `and` special form and embedded in
the `while` loop as the true-or-false-test, like this:

```
(and (< (point) end) (re-search-forward "\\w+\\W*" end t))
```

The `re-search-forward` expression returns `t` if the search
succeeds and as a side effect moves point. Consequently, as words are
found, point is moved through the region. When the search expression
fails to find another word, or when point reaches the end of the
region, the true-or-false-test tests false, the `while` loop
exits, and the `count-words-example` function displays one or
other of its messages.

After incorporating these final changes, the `count-words-example`
works without bugs (or at least, without bugs that I have found!).
Here is what it looks like:

```
;;; Final version: while
(defun count-words-example (beginning end)
  "Print number of words in the region."
  (interactive "r")
  (message "Counting words in region ... ")
```

```

```

```
;;; 1. Set up appropriate conditions.
  (save-excursion
    (let ((count 0))
      (goto-char beginning)
```

```

```

```
;;; 2. Run the while loop.
      (while (and (< (point) end)
                  (re-search-forward "\\w+\\W*" end t))
        (setq count (1+ count)))
```

```

```

```
;;; 3. Send a message to the user.
      (cond ((zerop count)
             (message
              "The region does NOT have any words."))
            ((= 1 count)
             (message
              "The region has 1 word."))
            (t
             (message
              "The region has %d words." count))))))
```

---

Next: [Exercise: Counting Punctuation](#Counting-Exercise), Previous: [The `count-words-example` Function](#count_002dwords_002dexample), Up: [Counting via Repetition and Regexps](#Counting-Words)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.2 Count Words Recursively

You can write the function for counting words recursively as well as
with a `while` loop. Let’s see how this is done.

First, we need to recognize that the `count-words-example`
function has three jobs: it sets up the appropriate conditions for
counting to occur; it counts the words in the region; and it sends a
message to the user telling how many words there are.

If we write a single recursive function to do everything, we will
receive a message for every recursive call. If the region contains 13
words, we will receive thirteen messages, one right after the other.
We don’t want this! Instead, we must write two functions to do the
job, one of which (the recursive function) will be used inside of the
other. One function will set up the conditions and display the
message; the other will return the word count.

Let us start with the function that causes the message to be displayed.
We can continue to call this `count-words-example`.

This is the function that the user will call. It will be interactive.
Indeed, it will be similar to our previous versions of this
function, except that it will call `recursive-count-words` to
determine how many words are in the region.

We can readily construct a template for this function, based on our
previous versions:

```
;; Recursive version; uses regular expression search
(defun count-words-example (beginning end)
  "documentation…"
  (interactive-expression…)
```

```
;;; 1. Set up appropriate conditions.
  (explanatory message)
  (set-up functions…
```

```
;;; 2. Count the words.
    recursive call
```

```
;;; 3. Send a message to the user.
    message providing word count))
```

The definition looks straightforward, except that somehow the count
returned by the recursive call must be passed to the message
displaying the word count. A little thought suggests that this can be
done by making use of a `let` expression: we can bind a variable
in the varlist of a `let` expression to the number of words in
the region, as returned by the recursive call; and then the
`cond` expression, using binding, can display the value to the
user.

Often, one thinks of the binding within a `let` expression as
somehow secondary to the primary work of a function. But in this
case, what you might consider the primary job of the function,
counting words, is done within the `let` expression.

Using `let`, the function definition looks like this:

```
(defun count-words-example (beginning end)
  "Print number of words in the region."
  (interactive "r")
```

```

```

```
;;; 1. Set up appropriate conditions.
  (message "Counting words in region ... ")
  (save-excursion
    (goto-char beginning)
```

```

```

```
;;; 2. Count the words.
    (let ((count (recursive-count-words end)))
```

```

```

```
;;; 3. Send a message to the user.
      (cond ((zerop count)
             (message
              "The region does NOT have any words."))
            ((= 1 count)
             (message
              "The region has 1 word."))
            (t
             (message
              "The region has %d words." count))))))
```

Next, we need to write the recursive counting function.

A recursive function has at least three parts: the do-again-test, the
next-step-expression, and the recursive call.

The do-again-test determines whether the function will or will not be
called again. Since we are counting words in a region and can use a
function that moves point forward for every word, the do-again-test
can check whether point is still within the region. The do-again-test
should find the value of point and determine whether point is before,
at, or after the value of the end of the region. We can use the
`point` function to locate point. Clearly, we must pass the
value of the end of the region to the recursive counting function as an
argument.

In addition, the do-again-test should also test whether the search finds a
word. If it does not, the function should not call itself again.

The next-step-expression changes a value so that when the recursive
function is supposed to stop calling itself, it stops. More
precisely, the next-step-expression changes a value so that at the
right time, the do-again-test stops the recursive function from
calling itself again. In this case, the next-step-expression can be
the expression that moves point forward, word by word.

The third part of a recursive function is the recursive call.

Somewhere, we also need a part that does the work of the
function, a part that does the counting. A vital part!

But already, we have an outline of the recursive counting function:

```
(defun recursive-count-words (region-end)
  "documentation…"
   do-again-test
   next-step-expression
   recursive call)
```

Now we need to fill in the slots. Let’s start with the simplest cases
first: if point is at or beyond the end of the region, there cannot
be any words in the region, so the function should return zero.
Likewise, if the search fails, there are no words to count, so the
function should return zero.

On the other hand, if point is within the region and the search
succeeds, the function should call itself again.

Thus, the do-again-test should look like this:

```
(and (< (point) region-end)
     (re-search-forward "\\w+\\W*" region-end t))
```

Note that the search expression is part of the do-again-test—the
function returns `t` if its search succeeds and `nil` if it
fails. (See [The Whitespace Bug in
`count-words-example`](#Whitespace-Bug), for an explanation of how
`re-search-forward` works.)

The do-again-test is the true-or-false test of an `if` clause.
Clearly, if the do-again-test succeeds, the then-part of the `if`
clause should call the function again; but if it fails, the else-part
should return zero since either point is outside the region or the
search failed because there were no words to find.

But before considering the recursive call, we need to consider the
next-step-expression. What is it? Interestingly, it is the search
part of the do-again-test.

In addition to returning `t` or `nil` for the
do-again-test, `re-search-forward` moves point forward as a side
effect of a successful search. This is the action that changes the
value of point so that the recursive function stops calling itself
when point completes its movement through the region. Consequently,
the `re-search-forward` expression is the next-step-expression.

In outline, then, the body of the `recursive-count-words`
function looks like this:

```
(if do-again-test-and-next-step-combined
    ;; then
    recursive-call-returning-count
  ;; else
  return-zero)
```

How to incorporate the mechanism that counts?

If you are not used to writing recursive functions, a question like
this can be troublesome. But it can and should be approached
systematically.

We know that the counting mechanism should be associated in some way
with the recursive call. Indeed, since the next-step-expression moves
point forward by one word, and since a recursive call is made for
each word, the counting mechanism must be an expression that adds one
to the value returned by a call to `recursive-count-words`.

Consider several cases:

* If there are two words in the region, the function should return
  a value resulting from adding one to the value returned when it counts
  the first word, plus the number returned when it counts the remaining
  words in the region, which in this case is one.
* If there is one word in the region, the function should return
  a value resulting from adding one to the value returned when it counts
  that word, plus the number returned when it counts the remaining
  words in the region, which in this case is zero.
* If there are no words in the region, the function should return zero.

From the sketch we can see that the else-part of the `if` returns
zero for the case of no words. This means that the then-part of the
`if` must return a value resulting from adding one to the value
returned from a count of the remaining words.

The expression will look like this, where `1+` is a function that
adds one to its argument.

```
(1+ (recursive-count-words region-end))
```

The whole `recursive-count-words` function will then look like
this:

```
(defun recursive-count-words (region-end)
  "documentation…"

;;; 1. do-again-test
  (if (and (< (point) region-end)
           (re-search-forward "\\w+\\W*" region-end t))
```

```

```

```
;;; 2. then-part: the recursive call
      (1+ (recursive-count-words region-end))

;;; 3. else-part
    0))
```

Let’s examine how this works:

If there are no words in the region, the else part of the `if`
expression is evaluated and consequently the function returns zero.

If there is one word in the region, the value of point is less than
the value of `region-end` and the search succeeds. In this case,
the true-or-false-test of the `if` expression tests true, and the
then-part of the `if` expression is evaluated. The counting
expression is evaluated. This expression returns a value (which will
be the value returned by the whole function) that is the sum of one
added to the value returned by a recursive call.

Meanwhile, the next-step-expression has caused point to jump over the
first (and in this case only) word in the region. This means that
when `(recursive-count-words region-end)` is evaluated a second
time, as a result of the recursive call, the value of point will be
equal to or greater than the value of region end. So this time,
`recursive-count-words` will return zero. The zero will be added
to one, and the original evaluation of `recursive-count-words`
will return one plus zero, which is one, which is the correct amount.

Clearly, if there are two words in the region, the first call to
`recursive-count-words` returns one added to the value returned
by calling `recursive-count-words` on a region containing the
remaining word—that is, it adds one to one, producing two, which is
the correct amount.

Similarly, if there are three words in the region, the first call to
`recursive-count-words` returns one added to the value returned
by calling `recursive-count-words` on a region containing the
remaining two words—and so on and so on.

With full documentation the two functions look like this:

The recursive function:

```
(defun recursive-count-words (region-end)
  "Number of words between point and REGION-END."
```

```

```

```
;;; 1. do-again-test
  (if (and (< (point) region-end)
           (re-search-forward "\\w+\\W*" region-end t))
```

```

```

```
;;; 2. then-part: the recursive call
      (1+ (recursive-count-words region-end))

;;; 3. else-part
    0))
```

The wrapper:

```
;;; Recursive version
(defun count-words-example (beginning end)
  "Print number of words in the region.
```

```

```

```
Words are defined as at least one word-constituent
character followed by at least one character that is
not a word-constituent.  The buffer's syntax table
determines which characters these are."
```

```
  (interactive "r")
  (message "Counting words in region ... ")
  (save-excursion
    (goto-char beginning)
    (let ((count (recursive-count-words end)))
```

```
      (cond ((zerop count)
             (message
              "The region does NOT have any words."))
```

```
            ((= 1 count)
             (message "The region has 1 word."))
            (t
             (message
              "The region has %d words." count))))))
```

---

Previous: [Count Words Recursively](#recursive_002dcount_002dwords), Up: [Counting via Repetition and Regexps](#Counting-Words)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 13.3 Exercise: Counting Punctuation

Using a `while` loop, write a function to count the number of
punctuation marks in a region—period, comma, semicolon, colon,
exclamation mark, and question mark. Do the same using recursion.

---

Next: [Readying a Graph](#Readying-a-Graph), Previous: [Counting via Repetition and Regexps](#Counting-Words), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 14 Counting Words in a `defun`

Our next project is to count the number of words in a function
definition. Clearly, this can be done using some variant of
`count-words-example`. See [Counting via
Repetition and Regexps](#Counting-Words). If we are just going to count the words in
one definition, it is easy enough to mark the definition with the
`C-M-h` (`mark-defun`) command, and then call
`count-words-example`.

However, I am more ambitious: I want to count the words and symbols in
every definition in the Emacs sources and then print a graph that
shows how many functions there are of each length: how many contain 40
to 49 words or symbols, how many contain 50 to 59 words or symbols,
and so on. I have often been curious how long a typical function is,
and this will tell.

* [Divide and Conquer](#Divide-and-Conquer)
* [What to Count?](#Words-and-Symbols)
* [What Constitutes a Word or Symbol?](#Syntax)
* [The `count-words-in-defun` Function](#count_002dwords_002din_002ddefun)
* [Count Several `defuns` Within a File](#Several-defuns)
* [Find a File](#Find-a-File)
* [`lengths-list-file` in Detail](#lengths_002dlist_002dfile)
* [Count Words in `defuns` in Different Files](#Several-files)
* [Recursively Count Words in Different Files](#Several-files-recursively)
* [Prepare the Data for Display in a Graph](#Prepare-the-data)

---

Next: [What to Count?](#Words-and-Symbols), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Divide and Conquer

Described in one phrase, the histogram project is daunting; but
divided into numerous small steps, each of which we can take one at a
time, the project becomes less fearsome. Let us consider what the
steps must be:

* First, write a function to count the words in one definition. This
  includes the problem of handling symbols as well as words.
* Second, write a function to list the number of words in each function
  in a file. This function can use the `count-words-in-defun`
  function.
* Third, write a function to list the number of words in each function
  in each of several files. This entails automatically finding the
  various files, switching to them, and counting the words in the
  definitions within them.
* Fourth, write a function to convert the list of numbers that we
  created in step three to a form that will be suitable for printing as
  a graph.
* Fifth, write a function to print the results as a graph.

This is quite a project! But if we take each step slowly, it will not
be difficult.

---

Next: [What Constitutes a Word or Symbol?](#Syntax), Previous: [Divide and Conquer](#Divide-and-Conquer), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.1 What to Count?

When we first start thinking about how to count the words in a
function definition, the first question is (or ought to be) what are
we going to count? When we speak of “words” with respect to a Lisp
function definition, we are actually speaking, in large part, of
symbols. For example, the following `multiply-by-seven`
function contains the five symbols `defun`,
`multiply-by-seven`, `number`, `*`, and `7`. In
addition, in the documentation string, it contains the four words
‘`Multiply`’, ‘`NUMBER`’, ‘`by`’, and ‘`seven`’. The
symbol ‘`number`’ is repeated, so the definition contains a total
of ten words and symbols.

```
(defun multiply-by-seven (number)
  "Multiply NUMBER by seven."
  (* 7 number))
```

However, if we mark the `multiply-by-seven` definition with
`C-M-h` (`mark-defun`), and then call
`count-words-example` on it, we will find that
`count-words-example` claims the definition has eleven words, not
ten! Something is wrong!

The problem is twofold: `count-words-example` does not count the
‘`*`’ as a word, and it counts the single symbol,
`multiply-by-seven`, as containing three words. The hyphens are
treated as if they were interword spaces rather than intraword
connectors: ‘`multiply-by-seven`’ is counted as if it were written
‘`multiply by seven`’.

The cause of this confusion is the regular expression search within
the `count-words-example` definition that moves point forward word
by word. In the canonical version of `count-words-example`, the
regexp is:

```
"\\w+\\W*"
```

This regular expression is a pattern defining one or more word
constituent characters possibly followed by one or more characters
that are not word constituents. What is meant by “word constituent
characters” brings us to the issue of syntax, which is worth a section
of its own.

---

Next: [The `count-words-in-defun` Function](#count_002dwords_002din_002ddefun), Previous: [What to Count?](#Words-and-Symbols), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.2 What Constitutes a Word or Symbol?

Emacs treats different characters as belonging to different
*syntax categories*. For example, the regular expression,
‘`\\w+`’, is a pattern specifying one or more *word
constituent* characters. Word constituent characters are members of
one syntax category. Other syntax categories include the class of
punctuation characters, such as the period and the comma, and the
class of whitespace characters, such as the blank space and the tab
character. (For more information, see [Syntax
Tables](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Syntax-Tables) in The GNU Emacs Lisp Reference Manual.)

Syntax tables specify which characters belong to which categories.
Usually, a hyphen is not specified as a word constituent character.
Instead, it is specified as being in the class of characters that are
part of symbol names but not words. This means that the
`count-words-example` function treats it in the same way it treats
an interword white space, which is why `count-words-example`
counts ‘`multiply-by-seven`’ as three words.

There are two ways to cause Emacs to count ‘`multiply-by-seven`’ as
one symbol: modify the syntax table or modify the regular expression.

We could redefine a hyphen as a word constituent character by
modifying the syntax table that Emacs keeps for each mode. This
action would serve our purpose, except that a hyphen is merely the
most common character within symbols that is not typically a word
constituent character; there are others, too.

Alternatively, we can redefine the regexp used in the
`count-words-example` definition so as to include symbols. This
procedure has the merit of clarity, but the task is a little tricky.

The first part is simple enough: the pattern must match at least one
character that is a word or symbol constituent. Thus:

```
"\\(\\w\\|\\s_\\)+"
```

The ‘`\\(`’ is the first part of the grouping construct that
includes the ‘`\\w`’ and the ‘`\\s_`’ as alternatives, separated
by the ‘`\\|`’. The ‘`\\w`’ matches any word-constituent
character and the ‘`\\s_`’ matches any character that is part of a
symbol name but not a word-constituent character. The ‘`+`’
following the group indicates that the word or symbol constituent
characters must be matched at least once.

However, the second part of the regexp is more difficult to design.
What we want is to follow the first part with optionally one or more
characters that are not constituents of a word or symbol. At first,
I thought I could define this with the following:

```
"\\(\\W\\|\\S_\\)*"
```

The upper case ‘`W`’ and ‘`S`’ match characters that are
*not* word or symbol constituents. Unfortunately, this
expression matches any character that is either not a word constituent
or not a symbol constituent. This matches any character!

I then noticed that every word or symbol in my test region was
followed by white space (blank space, tab, or newline). So I tried
placing a pattern to match one or more blank spaces after the pattern
for one or more word or symbol constituents. This failed, too. Words
and symbols are often separated by whitespace, but in actual code
parentheses may follow symbols and punctuation may follow words. So
finally, I designed a pattern in which the word or symbol constituents
are followed optionally by characters that are not white space and
then followed optionally by white space.

Here is the full regular expression:

```
"\\(\\w\\|\\s_\\)+[^ \t\n]*[ \t\n]*"
```

---

Next: [Count Several `defuns` Within a File](#Several-defuns), Previous: [What Constitutes a Word or Symbol?](#Syntax), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.3 The `count-words-in-defun` Function

We have seen that there are several ways to write a
`count-words-region` function. To write a
`count-words-in-defun`, we need merely adapt one of these
versions.

The version that uses a `while` loop is easy to understand, so I
am going to adapt that. Because `count-words-in-defun` will be
part of a more complex program, it need not be interactive and it need
not display a message but just return the count. These considerations
simplify the definition a little.

On the other hand, `count-words-in-defun` will be used within a
buffer that contains function definitions. Consequently, it is
reasonable to ask that the function determine whether it is called
when point is within a function definition, and if it is, to return
the count for that definition. This adds complexity to the
definition, but saves us from needing to pass arguments to the
function.

These considerations lead us to prepare the following template:

```
(defun count-words-in-defun ()
  "documentation…"
  (set up…
     (while loop…)
   return count)
```

As usual, our job is to fill in the slots.

First, the set up.

We are presuming that this function will be called within a buffer
containing function definitions. Point will either be within a
function definition or not. For `count-words-in-defun` to work,
point must move to the beginning of the definition, a counter must
start at zero, and the counting loop must stop when point reaches the
end of the definition.

The `beginning-of-defun` function searches backwards for an
opening delimiter such as a ‘`(`’ at the beginning of a line, and
moves point to that position, or else to the limit of the search. In
practice, this means that `beginning-of-defun` moves point to the
beginning of an enclosing or preceding function definition, or else to
the beginning of the buffer. We can use `beginning-of-defun` to
place point where we wish to start.

The `while` loop requires a counter to keep track of the words or
symbols being counted. A `let` expression can be used to create
a local variable for this purpose, and bind it to an initial value of zero.

The `end-of-defun` function works like `beginning-of-defun`
except that it moves point to the end of the definition.
`end-of-defun` can be used as part of an expression that
determines the position of the end of the definition.

The set up for `count-words-in-defun` takes shape rapidly: first
we move point to the beginning of the definition, then we create a
local variable to hold the count, and finally, we record the position
of the end of the definition so the `while` loop will know when to stop
looping.

The code looks like this:

```
(beginning-of-defun)
(let ((count 0)
      (end (save-excursion (end-of-defun) (point))))
```

The code is simple. The only slight complication is likely to concern
`end`: it is bound to the position of the end of the definition
by a `save-excursion` expression that returns the value of point
after `end-of-defun` temporarily moves it to the end of the
definition.

The second part of the `count-words-in-defun`, after the set up,
is the `while` loop.

The loop must contain an expression that jumps point forward word by
word and symbol by symbol, and another expression that counts the
jumps. The true-or-false-test for the `while` loop should test
true so long as point should jump forward, and false when point is at
the end of the definition. We have already redefined the regular
expression for this, so the loop is straightforward:

```
(while (and (< (point) end)
            (re-search-forward
             "\\(\\w\\|\\s_\\)+[^ \t\n]*[ \t\n]*" end t))
  (setq count (1+ count)))
```

The third part of the function definition returns the count of words
and symbols. This part is the last expression within the body of the
`let` expression, and can be, very simply, the local variable
`count`, which when evaluated returns the count.

Put together, the `count-words-in-defun` definition looks like this:

```
(defun count-words-in-defun ()
  "Return the number of words and symbols in a defun."
  (beginning-of-defun)
  (let ((count 0)
        (end (save-excursion (end-of-defun) (point))))
```

```
    (while
        (and (< (point) end)
             (re-search-forward
              "\\(\\w\\|\\s_\\)+[^ \t\n]*[ \t\n]*"
              end t))
      (setq count (1+ count)))
    count))
```

How to test this? The function is not interactive, but it is easy to
put a wrapper around the function to make it interactive; we can use
almost the same code as for the recursive version of
`count-words-example`:

```
;;; Interactive version.
(defun count-words-defun ()
  "Number of words and symbols in a function definition."
  (interactive)
  (message
   "Counting words and symbols in function definition ... ")
```

```
  (let ((count (count-words-in-defun)))
    (cond
     ((zerop count)
      (message
       "The definition does NOT have any words or symbols."))
```

```
     ((= 1 count)
      (message
       "The definition has 1 word or symbol."))
     (t
      (message
       "The definition has %d words or symbols." count)))))
```

Let’s reuse `C-c =` as a convenient key binding:

```
(keymap-global-set "C-c =" 'count-words-defun)
```

Now we can try out `count-words-defun`: install both
`count-words-in-defun` and `count-words-defun`, and set the
key binding. Then copy the following to an Emacs Lisp buffer (like,
for instance, `*scratch*`), place the cursor within the
definition, and use the `C-c =` command.

```
(defun multiply-by-seven (number)
  "Multiply NUMBER by seven."
  (* 7 number))
     ⇒ 10
```

Success! The definition has 10 words and symbols.

The next problem is to count the numbers of words and symbols in
several definitions within a single file.

---

Next: [Find a File](#Find-a-File), Previous: [The `count-words-in-defun` Function](#count_002dwords_002din_002ddefun), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.4 Count Several `defuns` Within a File

A file such as `simple.el` may have a hundred or more function
definitions within it. Our long term goal is to collect statistics on
many files, but as a first step, our immediate goal is to collect
statistics on one file.

The information will be a series of numbers, each number being the
length of a function definition. We can store the numbers in a list.

We know that we will want to incorporate the information regarding one
file with information about many other files; this means that the
function for counting definition lengths within one file need only
return the list of lengths. It need not and should not display any
messages.

The word count commands contain one expression to jump point forward
word by word and another expression to count the jumps. The function
to return the lengths of definitions can be designed to work the same
way, with one expression to jump point forward definition by
definition and another expression to construct the lengths’ list.

This statement of the problem makes it elementary to write the
function definition. Clearly, we will start the count at the
beginning of the file, so the first command will be `(goto-char
(point-min))`. Next, we start the `while` loop; and the
true-or-false test of the loop can be a regular expression search for
the next function definition—so long as the search succeeds, point
is moved forward and then the body of the loop is evaluated. The body
needs an expression that constructs the lengths’ list. `cons`,
the list construction command, can be used to create the list. That
is almost all there is to it.

Here is what this fragment of code looks like:

```
(goto-char (point-min))
(while (re-search-forward "^(defun" nil t)
  (setq lengths-list
        (cons (count-words-in-defun) lengths-list)))
```

What we have left out is the mechanism for finding the file that
contains the function definitions.

In previous examples, we either used this, the Info file, or we
switched back and forth to some other buffer, such as the
`*scratch*` buffer.

Finding a file is a new process that we have not yet discussed.

---

Next: [`lengths-list-file` in Detail](#lengths_002dlist_002dfile), Previous: [Count Several `defuns` Within a File](#Several-defuns), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.5 Find a File

To find a file in Emacs, you use the `C-x C-f` (`find-file`)
command. This command is almost, but not quite right for the lengths
problem.

Let’s look at the source for `find-file`:

```
(defun find-file (filename)
  "Edit file FILENAME.
Switch to a buffer visiting file FILENAME,
creating one if none already exists."
  (interactive "FFind file: ")
  (switch-to-buffer (find-file-noselect filename)))
```

(The most recent version of the `find-file` function definition
permits you to specify optional wildcards to visit multiple files;
that makes the definition more complex and we will not discuss it
here, since it is not relevant. You can see its source using either
`M-.` (`xref-find-definitions`) or `C-h f`
(`describe-function`).)

The definition I am showing possesses short but complete documentation
and an interactive specification that prompts you for a file name when
you use the command interactively. The body of the definition
contains two functions, `find-file-noselect` and
`switch-to-buffer`.

According to its documentation as shown by `C-h f` (the
`describe-function` command), the `find-file-noselect`
function reads the named file into a buffer and returns the buffer.
(Its most recent version includes an optional wildcards argument,
too, as well as another to read a file literally and another to
suppress warning messages. These optional arguments are irrelevant.)

However, the `find-file-noselect` function does not select the
buffer in which it puts the file. Emacs does not switch its attention
(or yours if you are using `find-file-noselect`) to the selected
buffer. That is what `switch-to-buffer` does: it switches the
buffer to which Emacs attention is directed; and it switches the
buffer displayed in the window to the new buffer. We have discussed
buffer switching elsewhere. (See [Switching Buffers](#Switching-Buffers).)

In this histogram project, we do not need to display each file on the
screen as the program determines the length of each definition within
it. Instead of employing `switch-to-buffer`, we can work with
`set-buffer`, which redirects the attention of the computer
program to a different buffer but does not redisplay it on the screen.
So instead of calling on `find-file` to do the job, we must write
our own expression.

The task is easy: use `find-file-noselect` and `set-buffer`.

---

Next: [Count Words in `defuns` in Different Files](#Several-files), Previous: [Find a File](#Find-a-File), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.6 `lengths-list-file` in Detail

The core of the `lengths-list-file` function is a `while`
loop containing a function to move point forward defun by defun, and
a function to count the number of words and symbols in each defun.
This core must be surrounded by functions that do various other tasks,
including finding the file, and ensuring that point starts out at the
beginning of the file. The function definition looks like this:

```
(defun lengths-list-file (filename)
  "Return list of definitions' lengths within FILE.
The returned list is a list of numbers.
Each number is the number of words or
symbols in one function definition."
```

```
  (message "Working on `%s' ... " filename)
  (save-excursion
    (let ((buffer (find-file-noselect filename))
          (lengths-list))
      (set-buffer buffer)
      (setq buffer-read-only t)
      (widen)
      (goto-char (point-min))
      (while (re-search-forward "^(defun" nil t)
        (setq lengths-list
              (cons (count-words-in-defun) lengths-list)))
      (kill-buffer buffer)
      lengths-list)))
```

The function is passed one argument, the name of the file on which it
will work. It has four lines of documentation, but no interactive
specification. Since people worry that a computer is broken if they
don’t see anything going on, the first line of the body is a
message.

The next line contains a `save-excursion` that returns Emacs’s
attention to the current buffer when the function completes. This is
useful in case you embed this function in another function that
presumes point is restored to the original buffer.

In the varlist of the `let` expression, Emacs finds the file and
binds the local variable `buffer` to the buffer containing the
file. At the same time, Emacs creates `lengths-list` as a local
variable.

Next, Emacs switches its attention to the buffer.

In the following line, Emacs makes the buffer read-only. Ideally,
this line is not necessary. None of the functions for counting words
and symbols in a function definition should change the buffer.
Besides, the buffer is not going to be saved, even if it were changed.
This line is entirely the consequence of great, perhaps excessive,
caution. The reason for the caution is that this function and those
it calls work on the sources for Emacs and it is inconvenient if they
are inadvertently modified. It goes without saying that I did not
realize a need for this line until an experiment went awry and started
to modify my Emacs source files …

Next comes a call to widen the buffer if it is narrowed. This
function is usually not needed—Emacs creates a fresh buffer if none
already exists; but if a buffer visiting the file already exists Emacs
returns that one. In this case, the buffer may be narrowed and must
be widened. If we wanted to be fully user-friendly, we would
arrange to save the restriction and the location of point, but we
won’t.

The `(goto-char (point-min))` expression moves point to the
beginning of the buffer.

Then comes a `while` loop in which the work of the function is
carried out. In the loop, Emacs determines the length of each
definition and constructs a lengths’ list containing the information.

Emacs kills the buffer after working through it. This is to save
space inside of Emacs. My version of GNU Emacs 19 contained over 300
source files of interest; GNU Emacs 22 contains over a thousand source
files. Another function will apply `lengths-list-file` to each
of the files.

Finally, the last expression within the `let` expression is the
`lengths-list` variable; its value is returned as the value of
the whole function.

You can try this function by installing it in the usual fashion. Then
place your cursor after the following expression and type `C-x
C-e` (`eval-last-sexp`).

```
(lengths-list-file
 "/usr/local/share/emacs/22.1/lisp/emacs-lisp/debug.el")
```

You may need to change the pathname of the file; the one here is for
GNU Emacs version 22.1. To change the expression, copy it to
the `*scratch*` buffer and edit it.

Also, to see the full length of the list, rather than a truncated
version, you may have to evaluate the following:

```
(custom-set-variables '(eval-expression-print-length nil))
```

(See [Specifying Variables using `defcustom`](#defcustom).
Then evaluate the `lengths-list-file` expression.)

The lengths’ list for `debug.el` takes less than a second to
produce and looks like this in GNU Emacs 22:

```
(83 113 105 144 289 22 30 97 48 89 25 52 52 88 28 29 77 49 43 290 232 587)
```

(Using my old machine, the version 19 lengths’ list for `debug.el`
took seven seconds to produce and looked like this:

```
(75 41 80 62 20 45 44 68 45 12 34 235)
```

The newer version of `debug.el` contains more defuns than the
earlier one; and my new machine is much faster than the old one.)

Note that the length of the last definition in the file is first in
the list.

---

Next: [Recursively Count Words in Different Files](#Several-files-recursively), Previous: [`lengths-list-file` in Detail](#lengths_002dlist_002dfile), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.7 Count Words in `defuns` in Different Files

In the previous section, we created a function that returns a list of
the lengths of each definition in a file. Now, we want to define a
function to return a master list of the lengths of the definitions in
a list of files.

Working on each of a list of files is a repetitious act, so we can use
either a `while` loop or recursion.

* [Determine the lengths of `defuns`](#lengths_002dlist_002dmany_002dfiles)
* [The `append` Function](#append)

---

Next: [The `append` Function](#append), Up: [Count Words in `defuns` in Different Files](#Several-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Determine the lengths of `defuns`

The design using a `while` loop is routine. The argument passed
to the function is a list of files. As we saw earlier (see [A `while` Loop and a List](#Loop-Example)), you can write a `while` loop so that the body of the
loop is evaluated if such a list contains elements, but to exit the
loop if the list is empty. For this design to work, the body of the
loop must contain an expression that shortens the list each time the
body is evaluated, so that eventually the list is empty. The usual
technique is to set the value of the list to the value of the CDR
of the list each time the body is evaluated.

The template looks like this:

```
(while test-whether-list-is-empty
  body…
  set-list-to-cdr-of-list)
```

Also, we remember that a `while` loop returns `nil` (the
result of evaluating the true-or-false-test), not the result of any
evaluation within its body. (The evaluations within the body of the
loop are done for their side effects.) However, the expression that
sets the lengths’ list is part of the body—and that is the value
that we want returned by the function as a whole. To do this, we
enclose the `while` loop within a `let` expression, and
arrange that the last element of the `let` expression contains
the value of the lengths’ list. (See [Loop
Example with an Incrementing Counter](#Incrementing-Example).)

These considerations lead us directly to the function itself:

```
;;; Use while loop.
(defun lengths-list-many-files (list-of-files)
  "Return list of lengths of defuns in LIST-OF-FILES."
```

```
  (let (lengths-list)

;;; true-or-false-test
    (while list-of-files
      (setq lengths-list
            (append
             lengths-list

;;; Generate a lengths’ list.
             (lengths-list-file
              (expand-file-name (car list-of-files)))))
```

```

```

```
;;; Make files’ list shorter.
      (setq list-of-files (cdr list-of-files)))

;;; Return final value of lengths’ list.
    lengths-list))
```

`expand-file-name` is a built-in function that converts a file
name to the absolute, long, path name form. The function employs the
name of the directory in which the function is called.

Thus, if `expand-file-name` is called on `debug.el` when
Emacs is visiting the
`/usr/local/share/emacs/22.1.1/lisp/emacs-lisp/` directory,

```
debug.el
```

becomes

```
/usr/local/share/emacs/22.1.1/lisp/emacs-lisp/debug.el
```

The only other new element of this function definition is the as yet
unstudied function `append`, which merits a short section for
itself.

---

Previous: [Determine the lengths of `defuns`](#lengths_002dlist_002dmany_002dfiles), Up: [Count Words in `defuns` in Different Files](#Several-files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 14.7.1 The `append` Function

The `append` function attaches one list to another. Thus,

```
(append '(1 2 3 4) '(5 6 7 8))
```

produces the list

```
(1 2 3 4 5 6 7 8)
```

This is exactly how we want to attach two lengths’ lists produced by
`lengths-list-file` to each other. The results contrast with
`cons`,

```
(cons '(1 2 3 4) '(5 6 7 8))
```

which constructs a new list in which the first argument to `cons`
becomes the first element of the new list:

```
((1 2 3 4) 5 6 7 8)
```

---

Next: [Prepare the Data for Display in a Graph](#Prepare-the-data), Previous: [Count Words in `defuns` in Different Files](#Several-files), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 14.8 Recursively Count Words in Different Files

Besides a `while` loop, you can work on each of a list of files
with recursion. A recursive version of `lengths-list-many-files`
is short and simple.

The recursive function has the usual parts: the do-again-test, the
next-step-expression, and the recursive call. The do-again-test
determines whether the function should call itself again, which it
will do if the `list-of-files` contains any remaining elements;
the next-step-expression resets the `list-of-files` to the
CDR of itself, so eventually the list will be empty; and the
recursive call calls itself on the shorter list. The complete
function is shorter than this description!

```
(defun recursive-lengths-list-many-files (list-of-files)
  "Return list of lengths of each defun in LIST-OF-FILES."
  (if list-of-files                     ; do-again-test
      (append
       (lengths-list-file
        (expand-file-name (car list-of-files)))
       (recursive-lengths-list-many-files
        (cdr list-of-files)))))
```

In a sentence, the function returns the lengths’ list for the first of
the `list-of-files` appended to the result of calling itself on
the rest of the `list-of-files`.

Here is a test of `recursive-lengths-list-many-files`, along with
the results of running `lengths-list-file` on each of the files
individually.

Install `recursive-lengths-list-many-files` and
`lengths-list-file`, if necessary, and then evaluate the
following expressions. You may need to change the files’ pathnames;
those here work when this Info file and the Emacs sources are located
in their customary places. To change the expressions, copy them to
the `*scratch*` buffer, edit them, and then evaluate them.

The results are shown after the ‘`⇒`’. (These results are
for files from Emacs version 22.1.1; files from other versions of
Emacs may produce different results.)

```
(cd "/usr/local/share/emacs/22.1.1/")

(lengths-list-file "./lisp/macros.el")
     ⇒ (283 263 480 90)
```

```

```

```
(lengths-list-file "./lisp/mail/mailalias.el")
     ⇒ (38 32 29 95 178 180 321 218 324)
```

```

```

```
(lengths-list-file "./lisp/hex-util.el")
     ⇒ (82 71)
```

```

```

```
  (recursive-lengths-list-many-files
   '("./lisp/macros.el"
     "./lisp/mail/mailalias.el"
     "./lisp/hex-util.el"))
       ⇒ (283 263 480 90 38 32 29 95 178 180 321 218 324 82 71)
```

The `recursive-lengths-list-many-files` function produces the
output we want.

The next step is to prepare the data in the list for display in a graph.

---

Previous: [Recursively Count Words in Different Files](#Several-files-recursively), Up: [Counting Words in a `defun`](#Words-in-a-defun)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
