### 19.3 Debugging Invalid Lisp Syntax [¶](#Debugging-Invalid-Lisp-Syntax)

The Lisp reader reports invalid syntax, but cannot say where the real
problem is. For example, the error ‘`End of file during parsing`’ in
evaluating an expression indicates an excess of open parentheses (or
square brackets). The reader detects this imbalance at the end of the
file, but it cannot figure out where the close parenthesis should have
been. Likewise, ‘`Invalid read syntax: ")"`’ indicates an excess close
parenthesis or missing open parenthesis, but does not say where the
missing parenthesis belongs. How, then, to find what to change?

If the problem is not simply an imbalance of parentheses, a useful
technique is to try `C-M-e` (`end-of-defun`, see [Moving by
Defuns](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Moving-by-Defuns) in The GNU Emacs Manual) at the beginning of each defun,
and see if it goes to the place where that defun appears to end. If
it does not, there is a problem in that defun.

However, unmatched parentheses are the most common syntax errors in
Lisp, and we can give further advice for those cases. (In addition,
just moving point through the code with Show Paren mode enabled might
find the mismatch.)

* [Excess Open Parentheses](#Excess-Open)
* [Excess Close Parentheses](#Excess-Close)

---

Next: [Excess Close Parentheses](#Excess-Close), Up: [Debugging Invalid Lisp Syntax](#Syntax-Errors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.3.1 Excess Open Parentheses [¶](#Excess-Open-Parentheses)

The first step is to find the defun that is unbalanced. If there is
an excess open parenthesis, the way to do this is to go to the end of
the file and type `C-u C-M-u` (`backward-up-list`,
see [Moving by Parens](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Moving-by-Parens) in The GNU Emacs Manual). This will
move you to the beginning of the first defun that is unbalanced.

The next step is to determine precisely what is wrong. There is no
way to be sure of this except by studying the program, but often the
existing indentation is a clue to where the parentheses should have
been. The easiest way to use this clue is to reindent with `C-M-q`
(`indent-pp-sexp`, see [Multi-line Indent](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Multi_002dline-Indent) in The GNU
Emacs Manual) and see what moves. **But don’t do this yet!**
Keep reading, first.

Before you do this, make sure the defun has enough close parentheses.
Otherwise, `C-M-q` will get an error, or will reindent all the rest
of the file until the end. So move to the end of the defun and insert a
close parenthesis there. Don’t use `C-M-e` (`end-of-defun`) to
move there, since that too will fail to work until the defun is balanced.

Now you can go to the beginning of the defun and type `C-M-q`.
Usually all the lines from a certain point to the end of the function
will shift to the right. There is probably a missing close parenthesis,
or a superfluous open parenthesis, near that point. (However, don’t
assume this is true; study the code to make sure.) Once you have found
the discrepancy, undo the `C-M-q` with `C-_` (`undo`),
since the old indentation is probably appropriate to the intended
parentheses.

After you think you have fixed the problem, use `C-M-q` again. If
the old indentation actually fit the intended nesting of parentheses,
and you have put back those parentheses, `C-M-q` should not change
anything.

---

Previous: [Excess Open Parentheses](#Excess-Open), Up: [Debugging Invalid Lisp Syntax](#Syntax-Errors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 19.3.2 Excess Close Parentheses [¶](#Excess-Close-Parentheses)

To deal with an excess close parenthesis, first go to the beginning
of the file, then type `C-u -1 C-M-u` (`backward-up-list`
with an argument of −1) to find the end of the first unbalanced
defun.

Then find the actual matching close parenthesis by typing `C-M-f`
(`forward-sexp`, see [Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Expressions) in The GNU Emacs Manual)
at the beginning of that defun. This will leave you somewhere short of
the place where the defun ought to end. It is possible that you will
find a spurious close parenthesis in that vicinity.

If you don’t see a problem at that point, the next thing to do is to
type `C-M-q` (`indent-pp-sexp`) at the beginning of the defun.
A range of lines will probably shift left; if so, the missing open
parenthesis or spurious close parenthesis is probably near the first of
those lines. (However, don’t assume this is true; study the code to
make sure.) Once you have found the discrepancy, undo the `C-M-q`
with `C-_` (`undo`), since the old indentation is probably
appropriate to the intended parentheses.

After you think you have fixed the problem, use `C-M-q` again. If
the old indentation actually fits the intended nesting of parentheses,
and you have put back those parentheses, `C-M-q` should not change
anything.

---

Next: [Profiling](#Profiling), Previous: [Debugging Invalid Lisp Syntax](#Syntax-Errors), Up: [Debugging Lisp Programs](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 19.4 Test Coverage [¶](#Test-Coverage-1)

You can do coverage testing for a file of Lisp code by loading the
`testcover` library and using the command `M-x
testcover-start RET file RET` to instrument the
code. Then test your code by calling it one or more times. Then use
the command `M-x testcover-mark-all` to display colored highlights
on the code to show where coverage is insufficient. The command
`M-x testcover-next-mark` will move point forward to the next
highlighted spot.

Normally, a red highlight indicates the form was never completely
evaluated; a brown highlight means it always evaluated to the same
value (meaning there has been little testing of what is done with the
result). However, the red highlight is skipped for forms that can’t
possibly complete their evaluation, such as `error`. The brown
highlight is skipped for forms that are expected to always evaluate to
the same value, such as `(setq x 14)`.

For difficult cases, you can add do-nothing macros to your code to
give advice to the test coverage tool.

Macro: **1value** form [¶](#index-1value)
:   Evaluate form and return its value, but inform coverage testing
    that form’s value should always be the same.

Macro: **noreturn** form [¶](#index-noreturn)
:   Evaluate form, informing coverage testing that form should
    never return. If it ever does return, you get a run-time error.

Edebug also has a coverage testing feature (see [Coverage Testing](#Coverage-Testing)). These features partly duplicate each other, and it would
be cleaner to combine them.

---

Previous: [Test Coverage](#Test-Coverage), Up: [Debugging Lisp Programs](#Debugging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 19.5 Profiling [¶](#Profiling-1)

If your program is working correctly, but not fast enough, and you
want to make it run more quickly or efficiently, the first thing to do
is *profile* your code so that you know where it spends most of
the execution time. If you find that one particular function is
responsible for a significant portion of the execution time, you can
start looking for ways to optimize that piece.

Emacs has built-in support for this. To begin profiling, type
`M-x profiler-start`. You can choose to sample CPU usage
periodically (`cpu`), when memory is allocated (`memory`),
or both. Then run the code you’d like to speed up. After that, type
`M-x profiler-report` to display a summary buffer for CPU usage
sampled by each type (cpu and memory) that you chose to profile. The
names of the report buffers include the times at which the reports
were generated, so you can generate another report later on without
erasing previous results. When you have finished profiling, type
`M-x profiler-stop` (there is a small overhead associated with
profiling, so we don’t recommend leaving it active except when you are
actually running the code you want to examine).

The profiler report buffer shows, on each line, a function that was
called, preceded by how much CPU resources it used in
absolute and percentage terms since profiling started. If a given
line has a ‘`+`’ symbol to the left of the function name, you can
expand that line by typing `RET`, in order to see the
function(s) called by the higher-level function. Use a prefix
argument (`C-u RET`) to see the whole call tree below a
function. Pressing `RET` again will collapse back to the
original state.

Press `j` (`profiler-report-find-entry`) or `mouse-2` to
jump to the definition of a function at point. Press `d`
(`profiler-report-describe-entry`) to view a function’s
documentation. You can save a profile to a file using `C-x C-w`
(`profiler-report-write-profile`) and read a saved profile with
`M-x profiler-find-profile` or `M-x profiler-find-profile-other-window`. You can compare two profiles
using `=` (`profiler-report-compare-profile`).

The `elp` library offers an alternative approach, which is useful
when you know in advance which Lisp function(s) you want to profile.
Using that library, you begin by setting `elp-function-list` to
the list of function symbols—those are the functions you want to
profile. Then type `M-x elp-instrument-list RET nil RET` to arrange for profiling those functions. After running
the code you want to profile, invoke `M-x elp-results` to
display the current results. See the file `elp.el` for more
detailed instructions. This approach is limited to profiling
functions written in Lisp, it cannot profile Emacs primitives.

You can measure the time it takes to evaluate individual Emacs Lisp
forms using the `benchmark` library. See the function
`benchmark-call` as well as the macros `benchmark-run`,
`benchmark-run-compiled`, and `benchmark-progn` in
`benchmark.el`. You can also use the `benchmark` command
for timing forms interactively.

To profile Emacs at the level of its C code, you can build it using the
`--enable-profiling` option of `configure`. When Emacs
exits, it generates a file `gmon.out` that you can examine using
the `gprof` utility. This feature is mainly useful for
debugging Emacs. It actually stops the Lisp-level `M-x
profiler-…` commands described above from working.

---

Next: [Minibuffers](#Minibuffers), Previous: [Debugging Lisp Programs](#Debugging), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 20 Reading and Printing Lisp Objects [¶](#Reading-and-Printing-Lisp-Objects)

*Printing* and *reading* are the operations of converting Lisp
objects to textual form and vice versa. They use the printed
representations and read syntax described in [Lisp Data Types](#Lisp-Data-Types).

This chapter describes the Lisp functions for reading and printing.
It also describes *streams*, which specify where to get the text (if
reading) or where to put it (if printing).

* [Introduction to Reading and Printing](#Streams-Intro)
* [Input Streams](#Input-Streams)
* [Input Functions](#Input-Functions)
* [Output Streams](#Output-Streams)
* [Output Functions](#Output-Functions)
* [Variables Affecting Output](#Output-Variables)
* [Overriding Output Variables](#Output-Overrides)

---

Next: [Input Streams](#Input-Streams), Up: [Reading and Printing Lisp Objects](#Read-and-Print)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 20.1 Introduction to Reading and Printing [¶](#Introduction-to-Reading-and-Printing)

*Reading* a Lisp object means parsing a Lisp expression in textual
form and producing a corresponding Lisp object. This is how Lisp
programs get into Lisp from files of Lisp code. We call the text the
*read syntax* of the object. For example, the text ‘`(a . 5)`’
is the read syntax for a cons cell whose CAR is `a` and whose
CDR is the number 5.

*Printing* a Lisp object means producing text that represents that
object—converting the object to its *printed representation*
(see [Printed Representation and Read Syntax](#Printed-Representation)). Printing the cons cell described
above produces the text ‘`(a . 5)`’.

Reading and printing are more or less inverse operations: printing the
object that results from reading a given piece of text often produces
the same text, and reading the text that results from printing an object
usually produces a similar-looking object. For example, printing the
symbol `foo` produces the text ‘`foo`’, and reading that text
returns the symbol `foo`. Printing a list whose elements are
`a` and `b` produces the text ‘`(a b)`’, and reading that
text produces a list (but not the same list) with elements `a`
and `b`.

However, these two operations are not precisely inverse to each other.
There are three kinds of exceptions:

* Printing can produce text that cannot be read. For example, buffers,
  windows, frames, subprocesses and markers print as text that starts
  with ‘`#`’; if you try to read this text, you get an error. There is
  no way to read those data types.
* One object can have multiple textual representations. For example,
  ‘`1`’ and ‘`01`’ represent the same integer, and ‘`(a b)`’ and
  ‘`(a . (b))`’ represent the same list. Reading will accept any of
  the alternatives, but printing must choose one of them.
* Comments can appear at certain points in the middle of an object’s
  read sequence without affecting the result of reading it.

---

Next: [Input Functions](#Input-Functions), Previous: [Introduction to Reading and Printing](#Streams-Intro), Up: [Reading and Printing Lisp Objects](#Read-and-Print)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 20.2 Input Streams [¶](#Input-Streams-1)

Most of the Lisp functions for reading text take an *input stream*
as an argument. The input stream specifies where or how to get the
characters of the text to be read. Here are the possible types of input
stream:

buffer [¶](#index-buffer-input-stream)
:   The input characters are read from buffer, starting with the
    character directly after point. Point advances as characters are read.

marker [¶](#index-marker-input-stream)
:   The input characters are read from the buffer that marker is in,
    starting with the character directly after the marker. The marker
    position advances as characters are read. The value of point in the
    buffer has no effect when the stream is a marker.

string [¶](#index-string-input-stream)
:   The input characters are taken from string, starting at the first
    character in the string and using as many characters as required.

function [¶](#index-function-input-stream)
:   The input characters are generated by function, which must support
    two kinds of calls:

    * When it is called with no arguments, it should return the next character.
    * When it is called with one argument (always a character), function
      should save the argument and arrange to return it on the next call.
      This is called *unreading* the character; it happens when the Lisp
      reader reads one character too many and wants to put it back where it
      came from. In this case, it makes no difference what value
      function returns.

`t` [¶](#index-t-input-stream)
:   `t` used as a stream means that the input is read from the
    minibuffer. In fact, the minibuffer is invoked once and the text
    given by the user is made into a string that is then used as the
    input stream. If Emacs is running in batch mode (see [Batch Mode](#Batch-Mode)),
    standard input is used instead of the minibuffer. For example,

    ```
    (message "%s" (read t))
    ```

    will in batch mode read a Lisp expression from standard input and
    print the result to standard output.

`nil` [¶](#index-nil-input-stream)
:   `nil` supplied as an input stream means to use the value of
    `standard-input` instead; that value is the *default input
    stream*, and must be a non-`nil` input stream.

symbol
:   A symbol as input stream is equivalent to the symbol’s function
    definition (if any).

Here is an example of reading from a stream that is a buffer, showing
where point is located before and after:

```
---------- Buffer: foo ----------
This∗ is the contents of foo.
---------- Buffer: foo ----------
```

```

```

```
(read (get-buffer "foo"))
     ⇒ is
```

```
(read (get-buffer "foo"))
     ⇒ the
```

```

```

```
---------- Buffer: foo ----------
This is the∗ contents of foo.
---------- Buffer: foo ----------
```

Note that the first read skips a space. Reading skips any amount of
whitespace preceding the significant text.

Here is an example of reading from a stream that is a marker,
initially positioned at the beginning of the buffer shown. The value
read is the symbol `This`.

```
---------- Buffer: foo ----------
This is the contents of foo.
---------- Buffer: foo ----------
```

```

```

```
(setq m (set-marker (make-marker) 1 (get-buffer "foo")))
     ⇒ #<marker at 1 in foo>
```

```
(read m)
     ⇒ This
```

```
m
     ⇒ #<marker at 5 in foo>   ;; Before the first space.
```

Here we read from the contents of a string:

```
(read "(When in) the course")
     ⇒ (When in)
```

The following example reads from the minibuffer. The
prompt is: ‘`Lisp expression:`’. (That is always the prompt
used when you read from the stream `t`.) The user’s input is shown
following the prompt.

```
(read t)
     ⇒ 23
---------- Buffer: Minibuffer ----------
Lisp expression: 23 RET
---------- Buffer: Minibuffer ----------
```

Finally, here is an example of a stream that is a function, named
`useless-stream`. Before we use the stream, we initialize the
variable `useless-list` to a list of characters. Then each call to
the function `useless-stream` obtains the next character in the list
or unreads a character by adding it to the front of the list.

```
(setq useless-list (append "XY()" nil))
     ⇒ (88 89 40 41)
```

```

```

```
(defun useless-stream (&optional unread)
  (if unread
      (setq useless-list (cons unread useless-list))
    (prog1 (car useless-list)
           (setq useless-list (cdr useless-list)))))
     ⇒ useless-stream
```

Now we read using the stream thus constructed:

```
(read 'useless-stream)
     ⇒ XY
```

```

```

```
useless-list
     ⇒ (40 41)
```

Note that the open and close parentheses remain in the list. The Lisp
reader encountered the open parenthesis, decided that it ended the
input, and unread it. Another attempt to read from the stream at this
point would read ‘`()`’ and return `nil`.

---

Next: [Output Streams](#Output-Streams), Previous: [Input Streams](#Input-Streams), Up: [Reading and Printing Lisp Objects](#Read-and-Print)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 20.3 Input Functions [¶](#Input-Functions-1)

This section describes the Lisp functions and variables that pertain
to reading.

In the functions below, stream stands for an input stream (see
the previous section). If stream is `nil` or omitted, it
defaults to the value of `standard-input`.

An `end-of-file` error is signaled if reading encounters an
unterminated list, vector, or string.

Function: **read** &optional stream [¶](#index-read)
:   This function reads one textual Lisp expression from stream,
    returning it as a Lisp object. This is the basic Lisp input function.

Function: **read-from-string** string &optional start end [¶](#index-read_002dfrom_002dstring)
:   This function reads the first textual Lisp expression from the text in
    string. It returns a cons cell whose CAR is that expression,
    and whose CDR is an integer giving the position of the next
    remaining character in the string (i.e., the first one not read).

    If start is supplied, then reading begins at index start in
    the string (where the first character is at index 0). If you specify
    end, then reading is forced to stop just before that index, as if
    the rest of the string were not there.

    For example:

    ```
    (read-from-string "(setq x 55) (setq y 5)")
         ⇒ ((setq x 55) . 11)
    ```

    ```
    (read-from-string "\"A short string\"")
         ⇒ ("A short string" . 16)
    ```

    ```

    ```

    ```
    ;; Read starting at the first character.
    (read-from-string "(list 112)" 0)
         ⇒ ((list 112) . 10)
    ```

    ```
    ;; Read starting at the second character.
    (read-from-string "(list 112)" 1)
         ⇒ (list . 5)
    ```

    ```
    ;; Read starting at the seventh character,
    ;;   and stopping at the ninth.
    (read-from-string "(list 112)" 6 8)
         ⇒ (11 . 8)
    ```

Function: **read-positioning-symbols** &optional stream [¶](#index-read_002dpositioning_002dsymbols)
:   This function reads one textual expression from stream, like
    `read` does, but additionally positions the read symbols to the
    positions in stream where they occurred. Only the symbol
    `nil` is not positioned, this for efficiency reasons.
    See [Symbols with Position](#Symbols-with-Position). This function is used by the byte
    compiler.

Variable: **standard-input** [¶](#index-standard_002dinput)
:   This variable holds the default input stream—the stream that
    `read` uses when the stream argument is `nil`.
    The default is `t`, meaning use the minibuffer.

Variable: **read-circle** [¶](#index-read_002dcircle)
:   If non-`nil`, this variable enables the reading of circular and
    shared structures. See [Read Syntax for Circular Objects](#Circular-Objects). Its default value is
    `t`.

When reading or writing from the standard input/output streams of the
Emacs process in batch mode, it is sometimes required to make sure any
arbitrary binary data will be read/written verbatim, and/or that no
translation of newlines to or from CR-LF pairs is performed. This
issue does not exist on POSIX hosts, only on MS-Windows and MS-DOS.
The following function allows you to control the I/O mode of any
standard stream of the Emacs process.

Function: **set-binary-mode** stream mode [¶](#index-set_002dbinary_002dmode)
:   Switch stream into binary or text I/O mode. If mode is
    non-`nil`, switch to binary mode, otherwise switch to text mode.
    The value of stream can be one of `stdin`, `stdout`,
    or `stderr`. This function flushes any pending output data of
    stream as a side effect, and returns the previous value of I/O
    mode for stream. On POSIX hosts, it always returns a
    non-`nil` value and does nothing except flushing pending output.

Function: **readablep** object [¶](#index-readablep)
:   This predicate says whether object has *readable syntax*,
    i.e., it can be written out and then read back by the Emacs Lisp
    reader. If it can’t, this function returns `nil`; if it can,
    this function returns a printed representation (via `prin1`,
    see [Output Functions](#Output-Functions)) of object.

---

Next: [Output Functions](#Output-Functions), Previous: [Input Functions](#Input-Functions), Up: [Reading and Printing Lisp Objects](#Read-and-Print)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 20.4 Output Streams [¶](#Output-Streams-1)

An output stream specifies what to do with the characters produced
by printing. Most print functions accept an output stream as an
optional argument. Here are the possible types of output stream:

buffer [¶](#index-buffer-output-stream)
:   The output characters are inserted into buffer at point.
    Point advances as characters are inserted.

marker [¶](#index-marker-output-stream)
:   The output characters are inserted into the buffer that marker
    points into, at the marker position. The marker position advances as
    characters are inserted. The value of point in the buffer has no effect
    on printing when the stream is a marker, and this kind of printing
    does not move point (except that if the marker points at or before the
    position of point, point advances with the surrounding text, as
    usual).

function [¶](#index-function-output-stream)
:   The output characters are passed to function, which is responsible
    for storing them away. It is called with a single character as
    argument, as many times as there are characters to be output, and
    is responsible for storing the characters wherever you want to put them.

`t` [¶](#index-t-output-stream)
:   The output characters are displayed in the echo area. If Emacs is
    running in batch mode (see [Batch Mode](#Batch-Mode)), the output is written to
    the standard output descriptor instead.

`nil` [¶](#index-nil-output-stream)
:   `nil` specified as an output stream means to use the value of the
    `standard-output` variable instead; that value is the
    *default output stream*, and must not be `nil`.

symbol
:   A symbol as output stream is equivalent to the symbol’s function
    definition (if any).

Many of the valid output streams are also valid as input streams. The
difference between input and output streams is therefore more a matter
of how you use a Lisp object, than of different types of object.

Here is an example of a buffer used as an output stream. Point is
initially located as shown immediately before the ‘`h`’ in
‘`the`’. At the end, point is located directly before that same
‘`h`’.

```
---------- Buffer: foo ----------
This is t∗he contents of foo.
---------- Buffer: foo ----------
```

```
(print "This is the output" (get-buffer "foo"))
     ⇒ "This is the output"
```

```
---------- Buffer: foo ----------
This is t
"This is the output"
∗he contents of foo.
---------- Buffer: foo ----------
```

Now we show a use of a marker as an output stream. Initially, the
marker is in buffer `foo`, between the ‘`t`’ and the ‘`h`’ in
the word ‘`the`’. At the end, the marker has advanced over the
inserted text so that it remains positioned before the same ‘`h`’.
Note that the location of point, shown in the usual fashion, has no
effect.

```
---------- Buffer: foo ----------
This is the ∗output
---------- Buffer: foo ----------
```

```

```

```
(setq m (copy-marker 10))
     ⇒ #<marker at 10 in foo>
```

```

```

```
(print "More output for foo." m)
     ⇒ "More output for foo."
```

```

```

```
---------- Buffer: foo ----------
This is t
"More output for foo."
he ∗output
---------- Buffer: foo ----------
```

```

```

```
m
     ⇒ #<marker at 34 in foo>
```

The following example shows output to the echo area:

```
(print "Echo Area output" t)
     ⇒ "Echo Area output"
---------- Echo Area ----------
"Echo Area output"
---------- Echo Area ----------
```

Finally, we show the use of a function as an output stream. The
function `eat-output` takes each character that it is given and
conses it onto the front of the list `last-output` (see [Building Cons Cells and Lists](#Building-Lists)). At the end, the list contains all the characters output, but
in reverse order.

```
(setq last-output nil)
     ⇒ nil
```

```

```

```
(defun eat-output (c)
  (setq last-output (cons c last-output)))
     ⇒ eat-output
```

```

```

```
(print "This is the output" #'eat-output)
     ⇒ "This is the output"
```

```

```

```
last-output
     ⇒ (10 34 116 117 112 116 117 111 32 101 104
    116 32 115 105 32 115 105 104 84 34 10)
```

Now we can put the output in the proper order by reversing the list:

```
(concat (nreverse last-output))
     ⇒ "
\"This is the output\"
"
```

Calling `concat` converts the list to a string so you can see its
contents more clearly.

Function: **external-debugging-output** character [¶](#index-external_002ddebugging_002doutput)
:   This function can be useful as an output stream when debugging. It
    writes character to the standard error stream.

    For example

    ```
    (print "This is the output" #'external-debugging-output)
    -| This is the output
    ⇒ "This is the output"
    ```

---

Next: [Variables Affecting Output](#Output-Variables), Previous: [Output Streams](#Output-Streams), Up: [Reading and Printing Lisp Objects](#Read-and-Print)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 20.5 Output Functions [¶](#Output-Functions-1)

This section describes the Lisp functions for printing Lisp
objects—converting objects into their printed representation.

Some of the Emacs printing functions add quoting characters to the
output when necessary so that it can be read properly. The quoting
characters used are ‘`"`’ and ‘`\`’; they distinguish strings from
symbols, and prevent punctuation characters in strings and symbols from
being taken as delimiters when reading. See [Printed Representation and Read Syntax](#Printed-Representation),
for full details. You specify quoting or no quoting by the choice of
printing function.

If the text is to be read back into Lisp, then you should print with
quoting characters to avoid ambiguity. Likewise, if the purpose is to
describe a Lisp object clearly for a Lisp programmer. However, if the
purpose of the output is to look nice for humans, then it is usually
better to print without quoting.

Lisp objects can refer to themselves. Printing a self-referential
object in the normal way would require an infinite amount of text, and
the attempt could cause infinite recursion. Emacs detects such
recursion and prints ‘`#level`’ instead of recursively printing
an object already being printed. For example, here ‘`#0`’ indicates
a recursive reference to the object at level 0 of the current print
operation:

```
(setq foo (list nil))
     ⇒ (nil)
(setcar foo foo)
     ⇒ (#0)
```

In the functions below, stream stands for an output stream.
(See the previous section for a description of output streams. Also
See [external-debugging-output](#external_002ddebugging_002doutput), a useful stream value for debugging.)
If stream is `nil` or omitted, it defaults to the value of
`standard-output`.

Function: **print** object &optional stream [¶](#index-print)
:   The `print` function is a convenient way of printing. It outputs
    the printed representation of object to stream, printing in
    addition one newline before object and another after it. Quoting
    characters are used. `print` returns object. For example:

    ```
    (progn (print 'The\ cat\ in)
           (print "the hat")
           (print " came back"))
         -|
         -| The\ cat\ in
         -|
         -| "the hat"
         -|
         -| " came back"
         ⇒ " came back"
    ```

Function: **prin1** object &optional stream overrides [¶](#index-prin1)
:   This function outputs the printed representation of object to
    stream. It does not print newlines to separate output as
    `print` does, but it does use quoting characters just like
    `print`. It returns object.

    ```
    (progn (prin1 'The\ cat\ in)
           (prin1 "the hat")
           (prin1 " came back"))
         -| The\ cat\ in"the hat"" came back"
         ⇒ " came back"
    ```

    If overrides is non-`nil`, it should either be `t`
    (which tells `prin1` to use the defaults for all printer related
    variables), or a list of settings. See [Overriding Output Variables](#Output-Overrides), for details.

Function: **princ** object &optional stream [¶](#index-princ)
:   This function outputs the printed representation of object to
    stream. It returns object.

    This function is intended to produce output that is readable by people,
    not by `read`, so it doesn’t insert quoting characters and doesn’t
    put double-quotes around the contents of strings. It does not add any
    spacing between calls.

    ```
    (progn
      (princ 'The\ cat)
      (princ " in the \"hat\""))
         -| The cat in the "hat"
         ⇒ " in the \"hat\""
    ```

Function: **terpri** &optional stream ensure [¶](#index-terpri)
:   This function outputs a newline to stream. The name stands for
    “terminate print”. If ensure is non-`nil` no newline is printed
    if stream is already at the beginning of a line. Note in this
    case stream can not be a function and an error is signaled if
    it is. This function returns `t` if a newline is printed.

Function: **write-char** character &optional stream [¶](#index-write_002dchar)
:   This function outputs character to stream. It returns
    character.

Function: **flush-standard-output** [¶](#index-flush_002dstandard_002doutput)
:   If you have Emacs-based batch scripts that send output to the
    terminal, Emacs will automatically display the output whenever you
    write a newline characters to `standard-output`. This function
    allows you to flush to `standard-output` without sending a
    newline character first, which enables you to display incomplete
    lines.

Function: **prin1-to-string** object &optional noescape overrides [¶](#index-prin1_002dto_002dstring)
:   This function returns a string containing the text that `prin1`
    would have printed for the same argument.

    ```
    (prin1-to-string 'foo)
         ⇒ "foo"
    ```

    ```
    (prin1-to-string (mark-marker))
         ⇒ "#<marker at 2773 in strings.texi>"
    ```

    If overrides is non-`nil`, it should either be `t`
    (which tells `prin1` to use the defaults for all printer related
    variables), or a list of settings. See [Overriding Output Variables](#Output-Overrides), for details.

    If noescape is non-`nil`, that inhibits use of quoting
    characters in the output. (This argument is supported in Emacs versions
    19 and later.)

    ```
    (prin1-to-string "foo")
         ⇒ "\"foo\""
    ```

    ```
    (prin1-to-string "foo" t)
         ⇒ "foo"
    ```

    See `format`, in [Formatting Strings](#Formatting-Strings), for other ways to obtain
    the printed representation of a Lisp object as a string.

Macro: **with-output-to-string** body… [¶](#index-with_002doutput_002dto_002dstring)
:   This macro executes the body forms with `standard-output` set
    up to feed output into a string. Then it returns that string.

    For example, if the current buffer name is ‘`foo`’,

    ```
    (with-output-to-string
      (princ "The buffer is ")
      (princ (buffer-name)))
    ```

    returns `"The buffer is foo"`.

Function: **pp** object &optional stream [¶](#index-pp)
:   This function outputs object to stream, just like
    `prin1`, but does it in a prettier way. That is, it’ll
    indent and fill the object to make it more readable for humans.

If you need to use binary I/O in batch mode, e.g., use the functions
described in this section to write out arbitrary binary data or avoid
conversion of newlines on non-POSIX hosts, see [set-binary-mode](#Input-Functions).

---

Next: [Overriding Output Variables](#Output-Overrides), Previous: [Output Functions](#Output-Functions), Up: [Reading and Printing Lisp Objects](#Read-and-Print)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 20.6 Variables Affecting Output [¶](#Variables-Affecting-Output)

Variable: **standard-output** [¶](#index-standard_002doutput)
:   The value of this variable is the default output stream—the stream
    that print functions use when the stream argument is `nil`.
    The default is `t`, meaning display in the echo area.

Variable: **print-quoted** [¶](#index-print_002dquoted)
:   If this is non-`nil`, that means to print quoted forms using
    abbreviated reader syntax, e.g., `(quote foo)` prints as
    `'foo`, and `(function foo)` as `#'foo`. The default
    is `t`.

Variable: **print-escape-newlines** [¶](#index-print_002descape_002dnewlines)
:   If this variable is non-`nil`, then newline characters in strings
    are printed as ‘`\n`’ and formfeeds are printed as ‘`\f`’.
    Normally these characters are printed as actual newlines and formfeeds.

    This variable affects the print functions `prin1` and `print`
    that print with quoting. It does not affect `princ`. Here is an
    example using `prin1`:

    ```
    (prin1 "a\nb")
         -| "a
         -| b"
         ⇒ "a
    b"
    ```

    ```

    ```

    ```
    (let ((print-escape-newlines t))
      (prin1 "a\nb"))
         -| "a\nb"
         ⇒ "a
    b"
    ```

    In the second expression, the local binding of
    `print-escape-newlines` is in effect during the call to
    `prin1`, but not during the printing of the result.

Variable: **print-escape-control-characters** [¶](#index-print_002descape_002dcontrol_002dcharacters)
:   If this variable is non-`nil`, control characters in strings are
    printed as backslash sequences by the print functions `prin1` and
    `print` that print with quoting. If this variable and
    `print-escape-newlines` are both non-`nil`, the latter takes
    precedences for newlines and formfeeds.

Variable: **print-escape-nonascii** [¶](#index-print_002descape_002dnonascii)
:   If this variable is non-`nil`, then unibyte non-ASCII
    characters in strings are unconditionally printed as backslash sequences
    by the print functions `prin1` and `print` that print with
    quoting.

    Those functions also use backslash sequences for unibyte non-ASCII
    characters, regardless of the value of this variable, when the output
    stream is a multibyte buffer or a marker pointing into one.

Variable: **print-escape-multibyte** [¶](#index-print_002descape_002dmultibyte)
:   If this variable is non-`nil`, then multibyte non-ASCII
    characters in strings are unconditionally printed as backslash sequences
    by the print functions `prin1` and `print` that print with
    quoting.

    Those functions also use backslash sequences for multibyte
    non-ASCII characters, regardless of the value of this variable,
    when the output stream is a unibyte buffer or a marker pointing into
    one.

Variable: **print-charset-text-property** [¶](#index-print_002dcharset_002dtext_002dproperty)
:   This variable controls printing of ‘charset’ text property on printing
    a string. The value should be `nil`, `t`, or
    `default`.

    If the value is `nil`, `charset` text properties are never
    printed. If `t`, they are always printed.

    If the value is `default`, only print `charset` text
    properties if there is an “unexpected” `charset` property. For
    ascii characters, all charsets are considered “expected”.
    Otherwise, the expected `charset` property of a character is
    given by `char-charset`.

Variable: **print-length** [¶](#index-print_002dlength)
:   The value of this variable is the maximum number of elements to print in
    any list, vector or bool-vector. If an object being printed has more
    than this many elements, it is abbreviated with an ellipsis.

    If the value is `nil` (the default), then there is no limit.

    ```
    (setq print-length 2)
         ⇒ 2
    ```

    ```
    (print '(1 2 3 4 5))
         -| (1 2 ...)
         ⇒ (1 2 ...)
    ```

Variable: **print-level** [¶](#index-print_002dlevel)
:   The value of this variable is the maximum depth of nesting of
    parentheses and brackets when printed. Any list or vector at a depth
    exceeding this limit is abbreviated with an ellipsis. A value of
    `nil` (which is the default) means no limit.

User Option: **eval-expression-print-length** [¶](#index-eval_002dexpression_002dprint_002dlength)

User Option: **eval-expression-print-level** [¶](#index-eval_002dexpression_002dprint_002dlevel)
:   These are the values for `print-length` and `print-level`
    used by `eval-expression`, and thus, indirectly, by many
    interactive evaluation commands (see [Evaluating
    Emacs Lisp Expressions](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Lisp-Eval) in The GNU Emacs Manual).

These variables are used for detecting and reporting circular
and shared structure:

Variable: **print-circle** [¶](#index-print_002dcircle)
:   If non-`nil`, this variable enables detection of circular and
    shared structure in printing. See [Read Syntax for Circular Objects](#Circular-Objects).

Variable: **print-unreadable-function** [¶](#index-print_002dunreadable_002dfunction)
:   By default, Emacs prints unreadable objects as ‘`#<...>"`’. For
    instance:

    ```
    (prin1-to-string (make-marker))
         ⇒ "#<marker in no buffer>"
    ```

    If this variable is non-`nil`, it should be a function that will
    be called to handle printing of these objects. The function will be
    called with two arguments: the object and the noescape flag used by
    the printing functions (see [Output Functions](#Output-Functions)).

    The function should return either `nil` (print the object as
    usual), or a string (which will be printed), or any other object
    (don’t print the object). For instance:

    ```
    (let ((print-unreadable-function
           (lambda (object escape) "hello")))
      (prin1-to-string (make-marker)))
         ⇒ "hello"
    ```

Variable: **print-gensym** [¶](#index-print_002dgensym)
:   If non-`nil`, this variable enables detection of uninterned symbols
    (see [Creating and Interning Symbols](#Creating-Symbols)) in printing. When this is enabled,
    uninterned symbols print with the prefix ‘`#:`’, which tells the Lisp
    reader to produce an uninterned symbol.

Variable: **print-continuous-numbering** [¶](#index-print_002dcontinuous_002dnumbering)
:   If non-`nil`, that means number continuously across print calls.
    This affects the numbers printed for ‘`#n=`’ labels and
    ‘`#m#`’ references.
    Don’t set this variable with `setq`; you should only bind it
    temporarily to `t` with `let`. When you do that, you should
    also bind `print-number-table` to `nil`.

Variable: **print-number-table** [¶](#index-print_002dnumber_002dtable)
:   This variable holds a vector used internally by printing to implement
    the `print-circle` feature. You should not use it except
    to bind it to `nil` when you bind `print-continuous-numbering`.

Variable: **float-output-format** [¶](#index-float_002doutput_002dformat)
:   This variable specifies how to print floating-point numbers. The
    default is `nil`, meaning use the shortest output
    that represents the number without losing information.

    To control output format more precisely, you can put a string in this
    variable. The string should hold a ‘`%`’-specification to be used
    in the C function `sprintf`. For further restrictions on what
    you can use, see the variable’s documentation string.

Variable: **print-integers-as-characters** [¶](#index-print_002dintegers_002das_002dcharacters)
:   When this variable is non-`nil`, integers that represent
    graphic base characters will be printed using Lisp character syntax
    (see [Basic Char Syntax](#Basic-Char-Syntax)). Other numbers are printed the usual way.
    For example, the list `(4 65 -1 10)` would be printed as
    ‘`(4 ?A -1 ?\n)`’.

    More precisely, values printed in character syntax are those
    representing characters belonging to the Unicode general categories
    Letter, Number, Punctuation, Symbol and Private-use
    (see [Character Properties](#Character-Properties)), as well as the control characters
    having their own escape syntax such as newline.

User Option: **pp-default-function** [¶](#index-pp_002ddefault_002dfunction)
:   This user variable specifies the function used by `pp` to prettify
    its output. By default it uses `pp-fill` which attempts to
    strike a good balance between speed and generating natural looking output
    that fits within `fill-column`. The previous default was
    `pp-28`, which tends to be faster but generate output that looks
    less natural and is less compact.

---

Previous: [Variables Affecting Output](#Output-Variables), Up: [Reading and Printing Lisp Objects](#Read-and-Print)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 20.7 Overriding Output Variables [¶](#Overriding-Output-Variables)

The previous section (see [Variables Affecting Output](#Output-Variables)) lists the numerous
variables that control how the Emacs Lisp printer formats data for
outputs. These are generally available for users to change, but
sometimes you want to output data in the default format, or override
the user settings in some other way. For instance, if you’re storing
Emacs Lisp data in a file, you don’t want that data to be shortened by
a `print-length` setting.

The `prin1` and `prin1-to-string` functions therefore have
an optional overrides argument. This argument can either be
`t` (which means that all printing variables should be reset to
the default values), or a list of settings for some of the variables.
Each element in the list can be either `t` (which means “reset
to defaults”, and will usually be the first element of the list), or
a pair whose `car` is a symbol that stands for an output variable
and whose `cdr` is the value for that variable.

For instance, this prints using nothing but defaults:

```
(prin1 object nil t)
```

This prints object using the current printing settings, but
overrides the value of `print-length` to be 5:

```
(prin1 object nil '((length . 5)))
```

And finally, this prints object using only default settings, but
with `print-length` bound to 5:

```
(prin1 object nil '(t (length . 5)))
```

Below is a list of symbols that can be used, and which variables they
map to:

`length`
:   This overrides `print-length`.

`level`
:   This overrides `print-level`.

`circle`
:   This overrides `print-circle`.

`quoted`
:   This overrides `print-quoted`.

`escape-newlines`
:   This overrides `print-escape-newlines`.

`escape-control-characters`
:   This overrides `print-escape-control-characters`.

`escape-nonascii`
:   This overrides `print-escape-nonascii`.

`escape-multibyte`
:   This overrides `print-escape-multibyte`.

`charset-text-property`
:   This overrides `print-charset-text-property`.

`unreadable-function`
:   This overrides `print-unreadable-function`.

`gensym`
:   This overrides `print-gensym`.

`continuous-numbering`
:   This overrides `print-continuous-numbering`.

`number-table`
:   This overrides `print-number-table`.

`float-format`
:   This overrides `float-output-format`.

`integers-as-characters`
:   This overrides `print-integers-as-characters`.

`symbols-bare`
:   This overrides `print-symbols-bare`.

In the future, more overrides may be offered that do not map directly
to a variable, but can only be used via this parameter.

---

Next: [Command Loop](#Command-Loop), Previous: [Reading and Printing Lisp Objects](#Read-and-Print), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 21 Minibuffers [¶](#Minibuffers-1)

A *minibuffer* is a special buffer that Emacs commands use to
read arguments more complicated than the single numeric prefix
argument. These arguments include file names, buffer names, and
command names (as in `M-x`). The minibuffer is displayed on the
bottom line of the frame, in the same place as the echo area
(see [The Echo Area](#The-Echo-Area)), but only while it is in use for reading an
argument.

* [Introduction to Minibuffers](#Intro-to-Minibuffers)
* [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer)
* [Reading Lisp Objects with the Minibuffer](#Object-from-Minibuffer)
* [Minibuffer History](#Minibuffer-History)
* [Initial Input](#Initial-Input)
* [Completion](#Completion)
* [Yes-or-No Queries](#Yes_002dor_002dNo-Queries)
* [Asking Multiple-Choice Questions](#Multiple-Queries)
* [Reading a Password](#Reading-a-Password)
* [Minibuffer Commands](#Minibuffer-Commands)
* [Minibuffer Windows](#Minibuffer-Windows)
* [Minibuffer Contents](#Minibuffer-Contents)
* [Recursive Minibuffers](#Recursive-Mini)
* [Inhibiting Interaction](#Inhibiting-Interaction)
* [Minibuffer Miscellany](#Minibuffer-Misc)

---

Next: [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.1 Introduction to Minibuffers [¶](#Introduction-to-Minibuffers)

In most ways, a minibuffer is a normal Emacs buffer. Most operations
*within* a buffer, such as editing commands, work normally in a
minibuffer. However, many operations for managing buffers do not apply
to minibuffers. The name of a minibuffer always has the form ‘`*Minibuf-number*`’, and it cannot be changed. Minibuffers are
displayed only in special windows used only for minibuffers; these
windows always appear at the bottom of a frame. (Sometimes frames have
no minibuffer window, and sometimes a special kind of frame contains
nothing but a minibuffer window; see [Minibuffers and Frames](#Minibuffers-and-Frames).)

The text in the minibuffer always starts with the *prompt string*,
the text that was specified by the program that is using the minibuffer
to tell the user what sort of input to type. This text is marked
read-only so you won’t accidentally delete or change it. It is also
marked as a field (see [Defining and Using Fields](#Fields)), so that certain motion functions,
including `beginning-of-line`, `forward-word`,
`forward-sentence`, and `forward-paragraph`, stop at the
boundary between the prompt and the actual text.

The minibuffer’s window is normally a single line; it grows
automatically if the contents require more space. Whilst the minibuffer
is active, you can explicitly resize its window temporarily with the
window sizing commands; the window reverts to its normal size when the
minibuffer is exited. When the minibuffer is not active, you can resize
its window permanently by using the window sizing commands in the
frame’s other window, or dragging the mode line with the mouse. (Due to
details of the current implementation, for this to work
`resize-mini-windows` must be `nil`.) If the frame contains
just a minibuffer window, you can change its size by changing the
frame’s size.

Use of the minibuffer reads input events, and that alters the values
of variables such as `this-command` and `last-command`
(see [Information from the Command Loop](#Command-Loop-Info)). Your program should bind them around the
code that uses the minibuffer, if you do not want that to change them.

Under some circumstances, a command can use a minibuffer even if
there is an active minibuffer; such a minibuffer is called a
*recursive minibuffer*. The first minibuffer is named
‘`*Minibuf-1*`’. Recursive minibuffers are named by
incrementing the number at the end of the name. (The names begin with
a space so that they won’t show up in normal buffer lists.) Of
several recursive minibuffers, the innermost (or most recently
entered) is the *active minibuffer*–it is the one you can
terminate by typing `RET` (`exit-minibuffer`) in. We usually
call this *the* minibuffer. You can permit or forbid recursive
minibuffers by setting the variable
`enable-recursive-minibuffers`, or by putting properties of that
name on command symbols (See [Recursive Minibuffers](#Recursive-Mini).)

Like other buffers, a minibuffer uses a local keymap
(see [Keymaps](#Keymaps)) to specify special key bindings. The function that
invokes the minibuffer also sets up its local map according to the job
to be done. See [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer), for the non-completion
minibuffer local maps. See [Minibuffer Commands that Do Completion](#Completion-Commands), for the minibuffer
local maps for completion.

An active minibuffer usually has major mode `minibuffer-mode`.
This is an Emacs internal mode without any special features. To
customize the setup of minibuffers, we suggest you use
`minibuffer-setup-hook` (see [Minibuffer Miscellany](#Minibuffer-Misc)) rather than
`minibuffer-mode-hook`, since the former is run later, after the
minibuffer has been fully initialized.

When a minibuffer is inactive, its major mode is
`minibuffer-inactive-mode`, with keymap
`minibuffer-inactive-mode-map`. This is only really useful if
the minibuffer is in a separate frame. See [Minibuffers and Frames](#Minibuffers-and-Frames).

When Emacs is running in batch mode, any request to read from the
minibuffer actually reads a line from the standard input descriptor that
was supplied when Emacs was started. This supports only basic input:
none of the special minibuffer features (history, completion, etc.)
are available in batch mode.

---

Next: [Reading Lisp Objects with the Minibuffer](#Object-from-Minibuffer), Previous: [Introduction to Minibuffers](#Intro-to-Minibuffers), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.2 Reading Text Strings with the Minibuffer [¶](#Reading-Text-Strings-with-the-Minibuffer)

The most basic primitive for minibuffer input is
`read-from-minibuffer`, which can be used to read either a string
or a Lisp object in textual form. The function `read-regexp` is
used for reading regular expressions (see [Regular Expressions](#Regular-Expressions)),
which are a special kind of string. There are also specialized
functions for reading commands, variables, file names, etc.
(see [Completion](#Completion)).

In most cases, you should not call minibuffer input functions in the
middle of a Lisp function. Instead, do all minibuffer input as part of
reading the arguments for a command, in the `interactive`
specification. See [Defining Commands](#Defining-Commands).

Function: **read-from-minibuffer** prompt &optional initial keymap read history default inherit-input-method [¶](#index-read_002dfrom_002dminibuffer)
:   This function is the most general way to get input from the
    minibuffer. By default, it accepts arbitrary text and returns it as a
    string; however, if read is non-`nil`, then it uses
    `read` to convert the text into a Lisp object (see [Input Functions](#Input-Functions)).

    The first thing this function does is to activate a minibuffer and
    display it with prompt (which must be a string) as the
    prompt. Then the user can edit text in the minibuffer.

    When the user types a command to exit the minibuffer,
    `read-from-minibuffer` constructs the return value from the text in
    the minibuffer. Normally it returns a string containing that text.
    However, if read is non-`nil`, `read-from-minibuffer`
    reads the text and returns the resulting Lisp object, unevaluated.
    (See [Input Functions](#Input-Functions), for information about reading.)

    The argument default specifies default values to make available
    through the history commands. It should be a string, a list of
    strings, or `nil`. The string or strings become the minibuffer’s
    “future history”, available to the user with `M-n`. In
    addition, if the call provides completion (e.g., via the keymap
    argument), the completion candidates are added to the “future
    history” when the values in default are exhausted by `M-n`;
    see [minibuffer-default-add-function](#Minibuffer-History).

    If read is non-`nil`, then default is also used
    as the input to `read`, if the user enters empty input.
    If default is a list of strings, the first string is used as the input.
    If default is `nil`, empty input results in an `end-of-file` error.
    However, in the usual case (where read is `nil`),
    `read-from-minibuffer` ignores default when the user enters
    empty input and returns an empty string, `""`. In this respect,
    it differs from all the other minibuffer input functions in this chapter.

    If keymap is non-`nil`, that keymap is the local keymap to
    use in the minibuffer. If keymap is omitted or `nil`, the
    value of `minibuffer-local-map` is used as the keymap. Specifying
    a keymap is the most important way to customize the minibuffer for
    various applications such as completion.

    The argument history specifies a history list variable to use
    for saving the input and for history commands used in the minibuffer.
    It defaults to `minibuffer-history`. If history is the
    symbol `t`, history is not recorded. You can optionally specify
    a starting position in the history list as well. See [Minibuffer History](#Minibuffer-History).

    If the variable `minibuffer-allow-text-properties` is
    non-`nil`, either let-bound or buffer-local in the minibuffer,
    then the string that is returned includes whatever text
    properties were present in the minibuffer. Otherwise all the text
    properties are stripped when the value is returned. (By default this
    variable is `nil`.)

    The text properties in `minibuffer-prompt-properties` are applied
    to the prompt. By default, this property list defines a face to use
    for the prompt. This face, if present, is applied to the end of the
    face list and merged before display.

    If the user wants to completely control the look of the prompt, the
    most convenient way to do that is to specify the `default` face
    at the end of all face lists. For instance:

    ```
    (read-from-minibuffer
     (concat
      (propertize "Bold" 'face '(bold default))
      (propertize " and normal: " 'face '(default))))
    ```

    If the argument inherit-input-method is non-`nil`, then the
    minibuffer inherits the current input method (see [Input Methods](#Input-Methods)) and
    the setting of `enable-multibyte-characters` (see [Text Representations](#Text-Representations)) from whichever buffer was current before entering the
    minibuffer.

    Use of initial is mostly deprecated; we recommend using
    a non-`nil` value only in conjunction with specifying a cons cell
    for history. See [Initial Input](#Initial-Input).

Function: **read-string** prompt &optional initial history default inherit-input-method [¶](#index-read_002dstring)
:   This function reads a string from the minibuffer and returns it. The
    arguments prompt, initial, history and
    inherit-input-method are used as in `read-from-minibuffer`.
    The keymap used is `minibuffer-local-map`.

    The optional argument default is used as in
    `read-from-minibuffer`, except that, if non-`nil`, it also
    specifies a default value to return if the user enters null input. As
    in `read-from-minibuffer` it should be a string, a list of
    strings, or `nil`, which is equivalent to an empty string. When
    default is a string, that string is the default value. When it
    is a list of strings, the first string is the default value. (All
    these strings are available to the user in the “future minibuffer
    history”.)

    This function works by calling the
    `read-from-minibuffer` function:

    ```
    (read-string prompt initial history default inherit)
    ≡
    (let ((value
           (read-from-minibuffer prompt initial nil nil
                                 history default inherit)))
      (if (and (equal value "") default)
          (if (consp default) (car default) default)
        value))
    ```

    If you have a long string (for instance, one that is several lines
    long) that you wish to edit, using `read-string` may not be
    ideal. In that case, popping to a new, normal buffer where the user
    can edit the string may be more convenient, and you can use the
    `read-string-from-buffer` function to do that.

Function: **read-regexp** prompt &optional defaults history [¶](#index-read_002dregexp)
:   This function reads a regular expression as a string from the
    minibuffer and returns it. If the minibuffer prompt string
    prompt does not end in ‘`:`’ (followed by optional
    whitespace), the function adds ‘`:` ’ to the end, preceded by the
    default return value (see below), if that is non-empty.

    The optional argument defaults controls the default value to
    return if the user enters null input, and should be one of: a string;
    `nil`, which is equivalent to an empty string; a list of strings;
    or a symbol.

    If defaults is a symbol, `read-regexp` consults the value
    of the variable `read-regexp-defaults-function` (see below), and
    if that is non-`nil` uses it in preference to defaults.
    The value in this case should be either:

    * `regexp-history-last`, which means to use the first element of
      the appropriate minibuffer history list (see below).
    * A function of no arguments, whose return value (which should be
      `nil`, a string, or a list of strings) becomes the value of
      defaults.

    `read-regexp` now ensures that the result of processing
    defaults is a list (i.e., if the value is `nil` or a
    string, it converts it to a list of one element). To this list,
    `read-regexp` then appends a few potentially useful candidates for
    input. These are:

    * The word or symbol at point.
    * The last regexp used in an incremental search.
    * The last string used in an incremental search.
    * The last string or pattern used in query-replace commands.

    The function now has a list of regular expressions that it passes to
    `read-from-minibuffer` to obtain the user’s input. The first
    element of the list is the default result in case of empty input. All
    elements of the list are available to the user as the “future
    minibuffer history” list (see [future list](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Minibuffer-History) in The GNU Emacs Manual).

    The optional argument history, if non-`nil`, is a symbol
    specifying a minibuffer history list to use (see [Minibuffer History](#Minibuffer-History)). If it is omitted or `nil`, the history list defaults
    to `regexp-history`.

    The user can use the `M-s c` command to indicate whether case
    folding should be on or off. If the user has used this command, the
    returned string will have the text property `case-fold` set to
    either `fold` or `inhibit-fold`. It is up to the caller of
    `read-regexp` to actually use this value, and the convenience
    function `read-regexp-case-fold-search` is provided for that. A
    typical usage pattern here might look like:

    ```
    (let* ((regexp (read-regexp "Search for: "))
           (case-fold-search (read-regexp-case-fold-search regexp)))
      (re-search-forward regexp))
    ```

User Option: **read-regexp-defaults-function** [¶](#index-read_002dregexp_002ddefaults_002dfunction)
:   The function `read-regexp` may use the value of this variable to
    determine its list of default regular expressions. If non-`nil`,
    the value of this variable should be either:

    * The symbol `regexp-history-last`.
    * A function of no arguments that returns either `nil`, a string,
      or a list of strings.

    See `read-regexp` above for details of how these values are used.

Variable: **minibuffer-allow-text-properties** [¶](#index-minibuffer_002dallow_002dtext_002dproperties)
:   If this variable is `nil`, the default, then
    `read-from-minibuffer` and all functions that do minibuffer input
    strip all text properties from the minibuffer input before returning it.

    However, `read-minibuffer` and related functions (see [Reading Lisp Objects With the Minibuffer](#Object-from-Minibuffer)), remove the
    text properties unconditionally, regardless of the value of this
    variable.

    If this variable is non-`nil`, either let-bound or buffer-local in
    the minibuffer, then `read-from-minibuffer`, `read-string`,
    and all related functions preserve text properties. But functions that
    do minibuffer input with completion remove the `face` property
    while preserving other text properties.

    ```
    (minibuffer-with-setup-hook
        (lambda ()
          (setq-local minibuffer-allow-text-properties t))
      (completing-read
       "String: " (list (propertize "foobar" 'face 'baz 'data 'zot))))
    => #("foobar" 0 6 (data zot))
    ```

    In this example, the user typed ‘`foo`’ and then hit the `TAB`
    key, and all text properties are preserved except the `face`
    property.

Variable: **minibuffer-local-map** [¶](#index-minibuffer_002dlocal_002dmap)
:   This
    is the default local keymap for reading from the minibuffer. By
    default, it makes the following bindings:

    `C-j`
    :   `exit-minibuffer`

    `RET`
    :   `exit-minibuffer`

    `M-<`
    :   `minibuffer-beginning-of-buffer`

    `C-g`
    :   `abort-recursive-edit`

    `M-n`

    `DOWN`
    :   `next-history-element`

    `M-p`

    `UP`
    :   `previous-history-element`

    `M-s`
    :   `next-matching-history-element`

    `M-r`
    :   `previous-matching-history-element`

    The variable `minibuffer-mode-map` is an alias for this variable.

Function: **read-no-blanks-input** prompt &optional initial inherit-input-method [¶](#index-read_002dno_002dblanks_002dinput)
:   This function reads a string from the minibuffer, but does not allow
    whitespace characters as part of the input: instead, those characters
    terminate the input. The arguments prompt, initial, and
    inherit-input-method are used as in `read-from-minibuffer`.

    This is a simplified interface to the `read-from-minibuffer`
    function, and passes the value of the `minibuffer-local-ns-map`
    keymap as the keymap argument for that function. Since the keymap
    `minibuffer-local-ns-map` does not rebind `C-q`, it *is*
    possible to put a space into the string, by quoting it.

Variable: **minibuffer-local-ns-map** [¶](#index-minibuffer_002dlocal_002dns_002dmap)
:   This built-in variable is the keymap used as the minibuffer local keymap
    in the function `read-no-blanks-input`. By default, it makes the
    following bindings, in addition to those of `minibuffer-local-map`:

    `SPC` [¶](#index-SPC-in-minibuffer)
    :   `exit-minibuffer`

    `TAB` [¶](#index-TAB-in-minibuffer)
    :   `exit-minibuffer`

    `?` [¶](#index-_003f-in-minibuffer)
    :   `self-insert-and-exit`

Function: **format-prompt** prompt default &rest format-args [¶](#index-format_002dprompt)
:   Format prompt with default value default according to the
    `minibuffer-default-prompt-format` variable.

    `minibuffer-default-prompt-format` is a format string (defaulting
    to ‘`" (default %s)"`’ that says how the “default” bit in prompts
    like ‘`"Local filename (default somefile): "`’ are to be formatted.

    To allow the users to customize how this is displayed, code that
    prompts the user for a value (and has a default) should look something
    along the lines of this code snippet:

    ```
    (read-file-name
     (format-prompt "Local filename" file)
     nil file)
    ```

    If format-args is `nil`, prompt is used as a literal
    string. If format-args is non-`nil`, prompt is used
    as a format control string, and prompt and format-args are
    passed to `format` (see [Formatting Strings](#Formatting-Strings)).

    `minibuffer-default-prompt-format` can be ‘`""`’, in which
    case no default values are displayed.

    If default is `nil`, there is no default value, and
    therefore no “default value” string is included in the result value.
    If default is a non-`nil` list, the first element of the
    list is used in the prompt.

    Both prompt and `minibuffer-default-prompt-format` are run
    through `substitute-command-keys` (see [Substituting Key Bindings in Documentation](#Keys-in-Documentation)).

Variable: **read-minibuffer-restore-windows** [¶](#index-read_002dminibuffer_002drestore_002dwindows)
:   If this option is non-`nil` (the default), getting input from the
    minibuffer will restore, on exit, the window configurations of the frame
    where the minibuffer was entered from and, if it is different, the frame
    that owns the minibuffer window. This means that if, for example, a
    user splits a window while getting input from the minibuffer on the same
    frame, that split will be undone when exiting the minibuffer.

    If this option is `nil`, no such restorations are done. Hence, the
    window split mentioned above will persist after exiting the minibuffer.

---

Next: [Minibuffer History](#Minibuffer-History), Previous: [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.3 Reading Lisp Objects with the Minibuffer [¶](#Reading-Lisp-Objects-with-the-Minibuffer)

This section describes functions for reading Lisp objects with the
minibuffer.

Function: **read-minibuffer** prompt &optional initial [¶](#index-read_002dminibuffer)
:   This function reads a Lisp object using the minibuffer, and returns it
    without evaluating it. The arguments prompt and initial are
    used as in `read-from-minibuffer`.

    This is a simplified interface to the
    `read-from-minibuffer` function:

    ```
    (read-minibuffer prompt initial)
    ≡
    (let (minibuffer-allow-text-properties)
      (read-from-minibuffer prompt initial nil t))
    ```

    Here is an example in which we supply the string `"(testing)"` as
    initial input:

    ```
    (read-minibuffer
     "Enter an expression: " (format "%s" '(testing)))

    ;; Here is how the minibuffer is displayed:
    ```

    ```

    ```

    ```
    ---------- Buffer: Minibuffer ----------
    Enter an expression: (testing)∗
    ---------- Buffer: Minibuffer ----------
    ```

    The user can type `RET` immediately to use the initial input as a
    default, or can edit the input.

Function: **eval-minibuffer** prompt &optional initial [¶](#index-eval_002dminibuffer)
:   This function reads a Lisp expression using the minibuffer, evaluates
    it, then returns the result. The arguments prompt and
    initial are used as in `read-from-minibuffer`.

    This function simply evaluates the result of a call to
    `read-minibuffer`:

    ```
    (eval-minibuffer prompt initial)
    ≡
    (eval (read-minibuffer prompt initial))
    ```

Function: **edit-and-eval-command** prompt form [¶](#index-edit_002dand_002deval_002dcommand)
:   This function reads a Lisp expression in the minibuffer, evaluates it,
    then returns the result. The difference between this command and
    `eval-minibuffer` is that here the initial form is not
    optional and it is treated as a Lisp object to be converted to printed
    representation rather than as a string of text. It is printed with
    `prin1`, so if it is a string, double-quote characters (‘`"`’)
    appear in the initial text. See [Output Functions](#Output-Functions).

    In the following example, we offer the user an expression with initial
    text that is already a valid form:

    ```
    (edit-and-eval-command "Please edit: " '(forward-word 1))

    ;; After evaluation of the preceding expression,
    ;;   the following appears in the minibuffer:
    ```

    ```

    ```

    ```
    ---------- Buffer: Minibuffer ----------
    Please edit: (forward-word 1)∗
    ---------- Buffer: Minibuffer ----------
    ```

    Typing `RET` right away would exit the minibuffer and evaluate the
    expression, thus moving point forward one word.

---

Next: [Initial Input](#Initial-Input), Previous: [Reading Lisp Objects with the Minibuffer](#Object-from-Minibuffer), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.4 Minibuffer History [¶](#Minibuffer-History-1)

A *minibuffer history list* records previous minibuffer inputs
so the user can reuse them conveniently. It is a variable whose value
is a list of strings (previous inputs), most recent first.

There are many separate minibuffer history lists, used for different
kinds of inputs. It’s the Lisp programmer’s job to specify the right
history list for each use of the minibuffer.

You specify a minibuffer history list with the optional history
argument to `read-from-minibuffer` or `completing-read`.
Here are the possible values for it:

variable
:   Use variable (a symbol) as the history list.

(variable . startpos)
:   Use variable (a symbol) as the history list, and assume that the
    initial history position is startpos (a nonnegative integer).

    Specifying 0 for startpos is equivalent to just specifying the
    symbol variable. `previous-history-element` will display
    the most recent element of the history list in the minibuffer. If you
    specify a positive startpos, the minibuffer history functions
    behave as if `(elt variable (1- startpos))` were the
    history element currently shown in the minibuffer.

    For consistency, you should also specify that element of the history
    as the initial minibuffer contents, using the initial argument
    to the minibuffer input function (see [Initial Input](#Initial-Input)).

If you don’t specify history, then the default history list
`minibuffer-history` is used. For other standard history lists,
see below. You can also create your own history list variable; just
initialize it to `nil` before the first use. If the variable is
buffer local, then each buffer will have its own input history list.

Both `read-from-minibuffer` and `completing-read` add new
elements to the history list automatically, and provide commands to
allow the user to reuse items on the list (see [Minibuffer Commands](#Minibuffer-Commands)). The only thing your program needs to do to use a history
list is to initialize it and to pass its name to the input functions
when you wish. But it is safe to modify the list by hand when the
minibuffer input functions are not using it.

By default, when `M-n` (`next-history-element`,
see [next-history-element](#Minibuffer-Commands)) reaches the end of
the list of default values provided by the command which initiated
reading input from the minibuffer, `M-n` adds all of the
completion candidates, as specified by
`minibuffer-completion-table` (see [Minibuffer Commands that Do Completion](#Completion-Commands)), to
the list of defaults, so that all those candidates are available as
“future history”. Your program can control that via the variable
`minibuffer-default-add-function`: if its value is not a
function, this automatic addition is disabled, and you can also set
this variable to your own function which adds only some candidates, or
some other values, to the “future history”.

Emacs functions that add a new element to a history list can also
delete old elements if the list gets too long. The variable
`history-length` specifies the maximum length for most history
lists. To specify a different maximum length for a particular history
list, put the length in the `history-length` property of the
history list symbol. The variable `history-delete-duplicates`
specifies whether to delete duplicates in history.

Function: **add-to-history** history-var newelt &optional maxelt keep-all [¶](#index-add_002dto_002dhistory)
:   This function adds a new element newelt, if it isn’t the empty
    string, to the history list stored in the variable history-var,
    and returns the updated history list. It limits the list length to
    the value of maxelt (if non-`nil`) or `history-length`
    (described below). The possible values of maxelt have the same
    meaning as the values of `history-length`.
    history-var cannot refer to a lexical variable.

    Normally, `add-to-history` removes duplicate members from the
    history list if `history-delete-duplicates` is non-`nil`.
    However, if keep-all is non-`nil`, that says not to remove
    duplicates, and to add newelt to the list even if it is empty.

Variable: **history-add-new-input** [¶](#index-history_002dadd_002dnew_002dinput)
:   If the value of this variable is `nil`, standard functions that
    read from the minibuffer don’t add new elements to the history list.
    This lets Lisp programs explicitly manage input history by using
    `add-to-history`. The default value is `t`.

User Option: **history-length** [¶](#index-history_002dlength)
:   The value of this variable specifies the maximum length for all
    history lists that don’t specify their own maximum lengths. If the
    value is `t`, that means there is no maximum (don’t delete old
    elements). If a history list variable’s symbol has a non-`nil`
    `history-length` property, it overrides this variable for that
    particular history list.

User Option: **history-delete-duplicates** [¶](#index-history_002ddelete_002dduplicates)
:   If the value of this variable is `t`, that means when adding a
    new history element, all previous identical elements are deleted.

Here are some of the standard minibuffer history list variables:

Variable: **minibuffer-history** [¶](#index-minibuffer_002dhistory)
:   The default history list for minibuffer history input.

Variable: **query-replace-history** [¶](#index-query_002dreplace_002dhistory)
:   A history list for arguments to `query-replace` (and similar
    arguments to other commands).

Variable: **file-name-history** [¶](#index-file_002dname_002dhistory)
:   A history list for file-name arguments.

Variable: **buffer-name-history** [¶](#index-buffer_002dname_002dhistory)
:   A history list for buffer-name arguments.

Variable: **regexp-history** [¶](#index-regexp_002dhistory)
:   A history list for regular expression arguments.

Variable: **extended-command-history** [¶](#index-extended_002dcommand_002dhistory)
:   A history list for arguments that are names of extended commands.

Variable: **shell-command-history** [¶](#index-shell_002dcommand_002dhistory)
:   A history list for arguments that are shell commands.

Variable: **read-expression-history** [¶](#index-read_002dexpression_002dhistory)
:   A history list for arguments that are Lisp expressions to evaluate.

Variable: **face-name-history** [¶](#index-face_002dname_002dhistory)
:   A history list for arguments that are faces.

Variable: **custom-variable-history** [¶](#index-custom_002dvariable_002dhistory)
:   A history list for variable-name arguments read by
    `read-variable`.

Variable: **read-number-history** [¶](#index-read_002dnumber_002dhistory)
:   A history list for numbers read by `read-number`.

Variable: **goto-line-history** [¶](#index-goto_002dline_002dhistory)
:   A history list for arguments to `goto-line`. This variable can
    be made local in every buffer by customizing the user option
    `goto-line-history-local`.

---

Next: [Completion](#Completion), Previous: [Minibuffer History](#Minibuffer-History), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
