### 2.5 Editing Types [¶](#Editing-Types-1)

The types in the previous section are used for general programming
purposes, and most of them are common to most Lisp dialects. Emacs Lisp
provides several additional data types for purposes connected with
editing.

* [Buffer Type](#Buffer-Type)
* [Marker Type](#Marker-Type)
* [Window Type](#Window-Type)
* [Frame Type](#Frame-Type)
* [Terminal Type](#Terminal-Type)
* [Window Configuration Type](#Window-Configuration-Type)
* [Frame Configuration Type](#Frame-Configuration-Type)
* [Process Type](#Process-Type)
* [Thread Type](#Thread-Type)
* [Mutex Type](#Mutex-Type)
* [Condition Variable Type](#Condition-Variable-Type)
* [Stream Type](#Stream-Type)
* [Keymap Type](#Keymap-Type)
* [Overlay Type](#Overlay-Type)
* [Font Type](#Font-Type)
* [Xwidget Type](#Xwidget-Type)

---

Next: [Marker Type](#Marker-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.1 Buffer Type [¶](#Buffer-Type-1)

A *buffer* is an object that holds text that can be edited
(see [Buffers](#Buffers)). Most buffers hold the contents of a disk file
(see [Files](#Files)) so they can be edited, but some are used for other
purposes. Most buffers are also meant to be seen by the user, and
therefore displayed, at some time, in a window (see [Windows](#Windows)). But
a buffer need not be displayed in any window. Each buffer has a
designated position called *point* (see [Positions](#Positions)); most
editing commands act on the contents of the current buffer in the
neighborhood of point. At any time, one buffer is the *current
buffer*.

The contents of a buffer are much like a string, but buffers are not
used like strings in Emacs Lisp, and the available operations are
different. For example, you can insert text efficiently into an
existing buffer, altering the buffer’s contents, whereas inserting
text into a string requires concatenating substrings, and the result
is an entirely new string object.

Many of the standard Emacs functions manipulate or test the
characters in the current buffer; a whole chapter in this manual is
devoted to describing these functions (see [Text](#Text)).

Several other data structures are associated with each buffer:

* a local syntax table (see [Syntax Tables](#Syntax-Tables));
* a local keymap (see [Keymaps](#Keymaps)); and,
* a list of buffer-local variable bindings (see [Buffer-Local Variables](#Buffer_002dLocal-Variables)).
* overlays (see [Overlays](#Overlays)).
* text properties for the text in the buffer (see [Text Properties](#Text-Properties)).

The local keymap and variable list contain entries that individually
override global bindings or values. These are used to customize the
behavior of programs in different buffers, without actually changing the
programs.

A buffer may be *indirect*, which means it shares the text
of another buffer, but presents it differently. See [Indirect Buffers](#Indirect-Buffers).

Buffers have no read syntax. They print in hash notation, showing the
buffer name.

```
(current-buffer)
     ⇒ #<buffer objects.texi>
```

---

Next: [Window Type](#Window-Type), Previous: [Buffer Type](#Buffer-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.2 Marker Type [¶](#Marker-Type-1)

A *marker* denotes a position in a specific buffer. Markers
therefore have two components: one for the buffer, and one for the
position. Changes in the buffer’s text automatically relocate the
position value as necessary to ensure that the marker always points
between the same two characters in the buffer.

Markers have no read syntax. They print in hash notation, giving the
current character position and the name of the buffer.

```
(point-marker)
     ⇒ #<marker at 10779 in objects.texi>
```

See [Markers](#Markers), for information on how to test, create, copy, and move
markers.

---

Next: [Frame Type](#Frame-Type), Previous: [Marker Type](#Marker-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.3 Window Type [¶](#Window-Type-1)

A *window* describes the portion of the screen that Emacs uses to
display buffers. Every live window (see [Basic Concepts of Emacs Windows](#Basic-Windows)) has one
associated buffer, whose contents appear in that window. By contrast, a
given buffer may appear in one window, no window, or several windows.
Windows are grouped on the screen into frames; each window belongs to
one and only one frame. See [Frame Type](#Frame-Type).

Though many windows may exist simultaneously, at any time one window
is designated the *selected window* (see [Selecting Windows](#Selecting-Windows)).
This is the window where the cursor is (usually) displayed when Emacs is
ready for a command. The selected window usually displays the current
buffer (see [The Current Buffer](#Current-Buffer)), but this is not necessarily the case.

Windows have no read syntax. They print in hash notation, giving the
window number and the name of the buffer being displayed. The window
numbers exist to identify windows uniquely, since the buffer displayed
in any given window can change frequently.

```
(selected-window)
     ⇒ #<window 1 on objects.texi>
```

See [Windows](#Windows), for a description of the functions that work on windows.

---

Next: [Terminal Type](#Terminal-Type), Previous: [Window Type](#Window-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.4 Frame Type [¶](#Frame-Type-1)

A *frame* is a screen area that contains one or more Emacs
windows; we also use the term “frame” to refer to the Lisp object
that Emacs uses to refer to the screen area.

Frames have no read syntax. They print in hash notation, giving the
frame’s title, plus its address in core (useful to identify the frame
uniquely).

```
(selected-frame)
     ⇒ #<frame emacs@psilocin.gnu.org 0xdac80>
```

See [Frames](#Frames), for a description of the functions that work on frames.

---

Next: [Window Configuration Type](#Window-Configuration-Type), Previous: [Frame Type](#Frame-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.5 Terminal Type [¶](#Terminal-Type-1)

A *terminal* is a device capable of displaying one or more
Emacs frames (see [Frame Type](#Frame-Type)).

Terminals have no read syntax. They print in hash notation giving
the terminal’s ordinal number and its TTY device file name.

```
(get-device-terminal nil)
     ⇒ #<terminal 1 on /dev/tty>
```

---

Next: [Frame Configuration Type](#Frame-Configuration-Type), Previous: [Terminal Type](#Terminal-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.6 Window Configuration Type [¶](#Window-Configuration-Type-1)

A *window configuration* stores information about the positions,
sizes, and contents of the windows in a frame, so you can recreate the
same arrangement of windows later.

Window configurations do not have a read syntax; their print syntax
looks like ‘`#<window-configuration>`’. See [Window Configurations](#Window-Configurations), for a description of several functions related to
window configurations.

---

Next: [Process Type](#Process-Type), Previous: [Window Configuration Type](#Window-Configuration-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.7 Frame Configuration Type [¶](#Frame-Configuration-Type-1)

A *frame configuration* stores information about the positions,
sizes, and contents of the windows in all frames. It is not a
primitive type—it is actually a list whose CAR is
`frame-configuration` and whose CDR is an alist. Each alist
element describes one frame, which appears as the CAR of that
element.

See [Frame Configurations](#Frame-Configurations), for a description of several functions
related to frame configurations.

---

Next: [Thread Type](#Thread-Type), Previous: [Frame Configuration Type](#Frame-Configuration-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.8 Process Type [¶](#Process-Type-1)

The word *process* usually means a running program. Emacs itself
runs in a process of this sort. However, in Emacs Lisp, a process is a
Lisp object that designates a subprocess created by the Emacs process.
Programs such as shells, GDB, ftp, and compilers, running in
subprocesses of Emacs, extend the capabilities of Emacs.
An Emacs subprocess takes textual input from Emacs and returns textual
output to Emacs for further manipulation. Emacs can also send signals
to the subprocess.

Process objects have no read syntax. They print in hash notation,
giving the name of the process:

```
(process-list)
     ⇒ (#<process shell>)
```

See [Processes](#Processes), for information about functions that create, delete,
return information about, send input or signals to, and receive output
from processes.

---

Next: [Mutex Type](#Mutex-Type), Previous: [Process Type](#Process-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.9 Thread Type [¶](#Thread-Type-1)

A *thread* in Emacs represents a separate thread of Emacs Lisp
execution. It runs its own Lisp program, has its own current buffer,
and can have subprocesses locked to it, i.e. subprocesses whose
output only this thread can accept. See [Threads](#Threads).

Thread objects have no read syntax. They print in hash notation,
giving the name of the thread (if it has been given a name) or its
address in core:

```
(all-threads)
    ⇒ (#<thread 0176fc40>)
```

---

Next: [Condition Variable Type](#Condition-Variable-Type), Previous: [Thread Type](#Thread-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.10 Mutex Type [¶](#Mutex-Type-1)

A *mutex* is an exclusive lock that threads can own and disown,
in order to synchronize between them. See [Mutexes](#Mutexes).

Mutex objects have no read syntax. They print in hash notation,
giving the name of the mutex (if it has been given a name) or its
address in core:

```
(make-mutex "my-mutex")
    ⇒ #<mutex my-mutex>
(make-mutex)
    ⇒ #<mutex 01c7e4e0>
```

---

Next: [Stream Type](#Stream-Type), Previous: [Mutex Type](#Mutex-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.11 Condition Variable Type [¶](#Condition-Variable-Type-1)

A *condition variable* is a device for a more complex thread
synchronization than the one supported by a mutex. A thread can wait
on a condition variable, to be woken up when some other thread
notifies the condition.

Condition variable objects have no read syntax. They print in hash
notation, giving the name of the condition variable (if it has been
given a name) or its address in core:

```
(make-condition-variable (make-mutex))
    ⇒ #<condvar 01c45ae8>
```

---

Next: [Keymap Type](#Keymap-Type), Previous: [Condition Variable Type](#Condition-Variable-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.12 Stream Type [¶](#Stream-Type-1)

A *stream* is an object that can be used as a source or sink for
characters—either to supply characters for input or to accept them as
output. Many different types can be used this way: markers, buffers,
strings, and functions. Most often, input streams (character sources)
obtain characters from the keyboard, a buffer, or a file, and output
streams (character sinks) send characters to a buffer, such as a
`*Help*` buffer, or to the echo area.

The object `nil`, in addition to its other meanings, may be used
as a stream. It stands for the value of the variable
`standard-input` or `standard-output`. Also, the object
`t` as a stream specifies input using the minibuffer
(see [Minibuffers](#Minibuffers)) or output in the echo area (see [The Echo Area](#The-Echo-Area)).

Streams have no special printed representation or read syntax, and
print as whatever primitive type they are.

See [Reading and Printing Lisp Objects](#Read-and-Print), for a description of functions
related to streams, including parsing and printing functions.

---

Next: [Overlay Type](#Overlay-Type), Previous: [Stream Type](#Stream-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.13 Keymap Type [¶](#Keymap-Type-1)

A *keymap* maps keys typed by the user to commands. This mapping
controls how the user’s command input is executed. A keymap is actually
a list whose CAR is the symbol `keymap`.

See [Keymaps](#Keymaps), for information about creating keymaps, handling prefix
keys, local as well as global keymaps, and changing key bindings.

---

Next: [Font Type](#Font-Type), Previous: [Keymap Type](#Keymap-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.14 Overlay Type [¶](#Overlay-Type-1)

An *overlay* specifies properties that apply to a part of a
buffer. Each overlay applies to a specified range of the buffer, and
contains a property list (a list whose elements are alternating property
names and values). Overlay properties are used to present parts of the
buffer temporarily in a different display style. Overlays have no read
syntax, and print in hash notation, giving the buffer name and range of
positions.

See [Overlays](#Overlays), for information on how you can create and use overlays.

---

Next: [Xwidget Type](#Xwidget-Type), Previous: [Overlay Type](#Overlay-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.15 Font Type [¶](#Font-Type-1)

A *font* specifies how to display text on a graphical terminal.
There are actually three separate font types—*font objects*,
*font specs*, and *font entities*—each of which has slightly
different properties. None of them have a read syntax; their print
syntax looks like ‘`#<font-object>`’, ‘`#<font-spec>`’, and
‘`#<font-entity>`’ respectively. See [Low-Level Font Representation](#Low_002dLevel-Font), for a
description of these Lisp objects.

---

Previous: [Font Type](#Font-Type), Up: [Editing Types](#Editing-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.5.16 Xwidget Type [¶](#Xwidget-Type-1)

An *xwidget* is a special display element, such as a web
browser, that can be embedded inside a buffer. Each window that
displays an xwidget will also have an *xwidget view*, which on
X-Windows corresponds to a single X window used to display the widget.

Neither of these objects are readable; their print syntaxes look like
‘`#<xwidget>`’ and ‘`#<xwidget-view>`’, respectively.
See [Embedded Native Widgets](#Xwidgets), for a more detailed description of xwidgets.

---

Next: [Type Predicates](#Type-Predicates), Previous: [Editing Types](#Editing-Types), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.6 Read Syntax for Circular Objects [¶](#Read-Syntax-for-Circular-Objects)

To represent shared or circular structures within a complex of Lisp
objects, you can use the reader constructs ‘`#n=`’ and
‘`#n#`’.

Use `#n=` before an object to label it for later reference;
subsequently, you can use `#n#` to refer the same object in
another place. Here, n is some integer. For example, here is how
to make a list in which the first element recurs as the third element:

```
(#1=(a) b #1#)
```

This differs from ordinary syntax such as this

```
((a) b (a))
```

which would result in a list whose first and third elements
look alike but are not the same Lisp object. This shows the difference:

```
(prog1 nil
  (setq x '(#1=(a) b #1#)))
(eq (nth 0 x) (nth 2 x))
     ⇒ t
(setq x '((a) b (a)))
(eq (nth 0 x) (nth 2 x))
     ⇒ nil
```

You can also use the same syntax to make a circular structure, which
appears as an element within itself. Here is an example:

```
#1=(a #1#)
```

This makes a list whose second element is the list itself.
Here’s how you can see that it really works:

```
(prog1 nil
  (setq x '#1=(a #1#)))
(eq x (cadr x))
     ⇒ t
```

The Lisp printer can produce this syntax to record circular and shared
structure in a Lisp object, if you bind the variable `print-circle`
to a non-`nil` value. See [Variables Affecting Output](#Output-Variables).

---

Next: [Equality Predicates](#Equality-Predicates), Previous: [Read Syntax for Circular Objects](#Circular-Objects), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.7 Type Predicates [¶](#Type-Predicates-1)

The Emacs Lisp interpreter itself does not perform type checking on
the actual arguments passed to functions when they are called. It could
not do so, since function arguments in Lisp do not have declared data
types, as they do in other programming languages. It is therefore up to
the individual function to test whether each actual argument belongs to
a type that the function can use.

All built-in functions do check the types of their actual arguments
when appropriate, and signal a `wrong-type-argument` error if an
argument is of the wrong type. For example, here is what happens if you
pass an argument to `+` that it cannot handle:

```
(+ 2 'a)
     error→ Wrong type argument: number-or-marker-p, a
```

If you want your program to handle different types differently, you
must do explicit type checking. The most common way to check the type
of an object is to call a *type predicate* function. Emacs has a
type predicate for each type, as well as some predicates for
combinations of types.

A type predicate function takes one argument; it returns `t` if
the argument belongs to the appropriate type, and `nil` otherwise.
Following a general Lisp convention for predicate functions, most type
predicates’ names end with ‘`p`’.

Here is an example which uses the predicates `listp` to check for
a list and `symbolp` to check for a symbol.

```
(defun add-on (x)
  (cond ((symbolp x)
         ;; If X is a symbol, put it on LIST.
         (setq list (cons x list)))
        ((listp x)
         ;; If X is a list, add its elements to LIST.
         (setq list (append x list)))
        (t
         ;; We handle only symbols and lists.
         (error "Invalid argument %s in add-on" x))))
```

Here is a table of predefined type predicates, in alphabetical order,
with references to further information.

`atom`
:   See [atom](#List_002drelated-Predicates).

`arrayp`
:   See [arrayp](#Array-Functions).

`bignump`
:   See [bignump](#Predicates-on-Numbers).

`bool-vector-p`
:   See [bool-vector-p](#Bool_002dVectors).

`booleanp`
:   See [booleanp](#nil-and-t).

`bufferp`
:   See [bufferp](#Buffer-Basics).

`byte-code-function-p`
:   See [byte-code-function-p](#Closure-Type).

`case-table-p`
:   See [case-table-p](#Case-Tables).

`char-or-string-p`
:   See [char-or-string-p](#Predicates-for-Strings).

`char-table-p`
:   See [char-table-p](#Char_002dTables).

`closurep`
:   See [closurep](#What-Is-a-Function).

`commandp`
:   See [commandp](#Interactive-Call).

`compiled-function-p`
:   See [compiled-function-p](#Closure-Type).

`condition-variable-p`
:   See [condition-variable-p](#Condition-Variables).

`consp`
:   See [consp](#List_002drelated-Predicates).

`custom-variable-p`
:   See [custom-variable-p](#Variable-Definitions).

`fixnump`
:   See [fixnump](#Predicates-on-Numbers).

`floatp`
:   See [floatp](#Predicates-on-Numbers).

`fontp`
:   See [Low-Level Font Representation](#Low_002dLevel-Font).

`frame-configuration-p`
:   See [frame-configuration-p](#Frame-Configurations).

`frame-live-p`
:   See [frame-live-p](#Deleting-Frames).

`framep`
:   See [framep](#Frames).

`functionp`
:   See [functionp](#Functions).

`hash-table-p`
:   See [hash-table-p](#Other-Hash).

`integer-or-marker-p`
:   See [integer-or-marker-p](#Predicates-on-Markers).

`integerp`
:   See [integerp](#Predicates-on-Numbers).

`interpreted-function-p`
:   See [interpreted-function-p](#What-Is-a-Function).

`keymapp`
:   See [keymapp](#Creating-Keymaps).

`keywordp`
:   See [Variables that Never Change](#Constant-Variables).

`listp`
:   See [listp](#List_002drelated-Predicates).

`markerp`
:   See [markerp](#Predicates-on-Markers).

`mutexp`
:   See [mutexp](#Mutexes).

`nlistp`
:   See [nlistp](#List_002drelated-Predicates).

`number-or-marker-p`
:   See [number-or-marker-p](#Predicates-on-Markers).

`numberp`
:   See [numberp](#Predicates-on-Numbers).

`obarrayp`
:   See [obarrayp](#Creating-Symbols).

`overlayp`
:   See [overlayp](#Overlays).

`processp`
:   See [processp](#Processes).

`recordp`
:   See [recordp](#Record-Type).

`sequencep`
:   See [sequencep](#Sequence-Functions).

`string-or-null-p`
:   See [string-or-null-p](#Predicates-for-Strings).

`stringp`
:   See [stringp](#Predicates-for-Strings).

`subrp`
:   See [subrp](#Function-Cells).

`symbolp`
:   See [symbolp](#Symbols).

`syntax-table-p`
:   See [syntax-table-p](#Syntax-Tables).

`threadp`
:   See [threadp](#Basic-Thread-Functions).

`vectorp`
:   See [vectorp](#Vectors).

`wholenump`
:   See [wholenump](#Predicates-on-Numbers).

`window-configuration-p`
:   See [window-configuration-p](#Window-Configurations).

`window-live-p`
:   See [window-live-p](#Deleting-Windows).

`windowp`
:   See [windowp](#Basic-Windows).

The most general way to check the type of an object is to call the
function `type-of`. Recall that each object belongs to one and
only one primitive type; `type-of` tells you which one (see [Lisp Data Types](#Lisp-Data-Types)). But `type-of` knows nothing about non-primitive
types. In most cases, it is preferable to use type predicates than
`type-of`.

Function: **type-of** object [¶](#index-type_002dof)
:   This function returns a symbol naming the primitive type of
    object. The value is one of the symbols `bool-vector`,
    `buffer`, `char-table`, `compiled-function`,
    `condition-variable`, `cons`, `finalizer`,
    `float`, `font-entity`, `font-object`,
    `font-spec`, `frame`, `hash-table`, `integer`,
    `marker`, `mutex`, `obarray`, `overlay`, `process`,
    `string`, `subr`, `symbol`, `thread`,
    `vector`, `window`, or `window-configuration`.
    However, if object is a record, the type specified by its first
    slot is returned; [Records](#Records).

    ```
    (type-of 1)
         ⇒ integer
    ```

    ```
    (type-of 'nil)
         ⇒ symbol
    (type-of '())    ; () is nil.
         ⇒ symbol
    (type-of '(x))
         ⇒ cons
    (type-of (record 'foo))
         ⇒ foo
    ```

Function: **cl-type-of** object [¶](#index-cl_002dtype_002dof)
:   This function returns a symbol naming *the* type of
    object. It usually behaves like `type-of`, except
    that it guarantees to return the most precise type possible, which also
    implies that the specific type it returns may change depending on the
    Emacs version. For this reason, as a rule you should never compare its
    return value against some fixed set of types.

    ```
    (cl-type-of 1)
         ⇒ fixnum
    ```

    ```
    (cl-type-of 'nil)
         ⇒ null
    (cl-type-of (record 'foo))
         ⇒ foo
    ```

---

Next: [Mutability](#Mutability), Previous: [Type Predicates](#Type-Predicates), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.8 Equality Predicates [¶](#Equality-Predicates-1)

Here we describe functions that test for equality between two
objects. Other functions test equality of contents between objects of
specific types, e.g., strings. For these predicates, see the
appropriate chapter describing the data type.

Function: **eq** object1 object2 [¶](#index-eq)
:   This function returns `t` if object1 and object2 are
    the same object, and `nil` otherwise.

    If object1 and object2 are symbols with the
    same name, they are normally the same object—but see [Creating and Interning Symbols](#Creating-Symbols) for exceptions. For other non-numeric types (e.g., lists, vectors,
    strings), two arguments with the same contents or elements are not
    necessarily `eq` to each other: they are `eq` only if they
    are the same object, meaning that a change in the contents of one will
    be reflected by the same change in the contents of the other.

    If object1 and object2 are numbers with differing types or values,
    then they cannot be the same object and `eq` returns `nil`.
    If they are fixnums with the same value,
    then they are the same object and `eq` returns `t`.
    If they were computed separately but happen to have the same value
    and the same non-fixnum numeric type, then they might or might not be
    the same object, and `eq` returns `t` or `nil`
    depending on whether the Lisp interpreter created one object or two.

    If object1 or object2 is a symbol with position, `eq`
    regards it as its bare symbol when `symbols-with-pos-enabled` is
    non-`nil` (see [Symbols with Position](#Symbols-with-Position)).

    ```
    (eq 'abc 'abc)  ⇒ t
    (eq 'abc 'ABC)  ⇒ nil
    (eq ?A ?A)      ⇒ t
    (eq 3 3)        ⇒ t
    ```

    Equal non-fixnum numbers may or may not be the same object:

    ```
    (eq 3.0 3.0)                    ⇒ t or nil
    (eq (expt 10 50) (expt 10 50))  ⇒ t or nil
    ```

    Newly created mutable objects are distinct:

    ```
    (eq (list 1 2 3) (list 1 2 3))      ⇒ nil
    (eq (point-marker) (point-marker))  ⇒ nil
    ```

    Equal constants of other types may or may not be the same object:

    ```
    (eq "abc" "abc")        ⇒ t or nil
    (eq '(1 2 3) '(1 2 3))  ⇒ t or nil
    (eq [1 2 3] [1 2 3])    ⇒ t or nil
    ```

    unless they are the same literal constant:

    ```
    (let ((x "abc"))    (eq x x))  ⇒ t
    (let ((x '(1 2 3))) (eq x x))  ⇒ t
    (let ((x [1 2 3]))  (eq x x))  ⇒ t
    ```

    The `make-symbol` function returns an uninterned symbol, distinct
    from the symbol that is used if you write the name in a Lisp expression.
    Distinct symbols with the same name are not `eq`. See [Creating and Interning Symbols](#Creating-Symbols).

    ```
    (eq (make-symbol "foo") 'foo)
         ⇒ nil
    ```

    The Emacs Lisp byte compiler may collapse identical literal objects,
    such as literal strings, into references to the same object, with the
    effect that the byte-compiled code will compare such objects as
    `eq`, while the interpreted version of the same code will not.
    Therefore, your code should never rely on objects with the same
    literal contents being either `eq` or not `eq`, it should
    instead use functions that compare object contents such as
    `equal`, described below. Similarly, your code should not modify
    literal objects (e.g., put text properties on literal strings), since
    doing that might affect other literal objects of the same contents, if
    the byte compiler collapses them.

Function: **equal** object1 object2 [¶](#index-equal)
:   This function returns `t` if object1 and object2 have
    equal components, and `nil` otherwise. Whereas `eq` tests
    if its arguments are the same object, `equal` looks inside
    nonidentical arguments to see if their elements or contents are the
    same. So, if two objects are `eq`, they are `equal`, but
    the converse is not always true.

    ```
    (equal 'foo 'foo)
         ⇒ t
    ```

    ```

    ```

    ```
    (equal 456 456)
         ⇒ t
    ```

    ```

    ```

    ```
    (equal "asdf" "asdf")
         ⇒ t
    ```

    ```

    ```

    ```
    (equal '(1 (2 (3))) '(1 (2 (3))))
         ⇒ t
    ```

    ```

    ```

    ```
    (equal [(1 2) 3] [(1 2) 3])
         ⇒ t
    ```

    ```

    ```

    ```
    (equal (point-marker) (point-marker))
         ⇒ t
    ```

    The `equal` function compares strings and bool-vectors by value.
    Numbers are compared by type and numeric value, using `eql`.
    Lists, cons cells, vectors, records, markers, char-tables, font objects,
    and function objects (closures)[2](#FOOT2) are compared
    recursively by using `equal` on their constituent parts.

    Comparison of strings is case-sensitive, but does not take account of
    text properties—it compares only the characters in the strings.
    See [Text Properties](#Text-Properties). Use `equal-including-properties` to also
    compare text properties. For technical reasons, a unibyte string and
    a multibyte string are `equal` if and only if they contain the
    same sequence of character codes and all these codes are in the range
    0 through 127 (ASCII).

    ```
    (equal "asdf" "ASDF")
         ⇒ nil
    ```

    If object1 or object2 contains symbols with position,
    `equal` treats them as if they were their bare symbols when
    `symbols-with-pos-enabled` is non-`nil`. Otherwise
    `equal` compares two symbols with position by
    comparing their components. See [Symbols with Position](#Symbols-with-Position).

    Other objects are considered `equal` only if they are `eq`.
    For example, two distinct buffers are never considered `equal`,
    even if their textual contents are the same.

For `equal`, equality is defined recursively; for example, given
two cons cells x and y, `(equal x y)`
returns `t` if and only if both the expressions below return
`t`:

```
(equal (car x) (car y))
(equal (cdr x) (cdr y))
```

Comparing very deeply nested objects may therefore cause deep recursion
that leads to an error.

Function: **equal-including-properties** object1 object2 [¶](#index-equal_002dincluding_002dproperties)
:   This function behaves like `equal` in all cases but also requires
    that for two strings to be equal, they have the same text properties.

    ```
    (equal "asdf" (propertize "asdf" 'asdf t))
         ⇒ t
    ```

    ```
    (equal-including-properties "asdf"
                                (propertize "asdf" 'asdf t))
         ⇒ nil
    ```

---

Next: [Type Hierarchy of Emacs Lisp Objects](#Type-Hierarchy), Previous: [Equality Predicates](#Equality-Predicates), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.9 Mutability [¶](#Mutability-1)

Some Lisp objects should never change. For example, the Lisp
expression `"aaa"` yields a string, but you should not change
its contents. And some objects cannot be changed; for example,
although you can create a new number by calculating one, Lisp provides
no operation to change the value of an existing number.

Other Lisp objects are *mutable*: it is safe to change their
values via destructive operations involving side effects. For
example, an existing marker can be changed by moving the marker to
point to somewhere else.

Although numbers never change and all markers are mutable,
some types have members some of which are mutable and others not. These
types include conses, vectors, and strings. For example,
although `"cons"` and `(symbol-name 'cons)` both yield
strings that should not be changed, `(copy-sequence "cons")` and
`(make-string 3 ?a)` both yield mutable strings that can be
changed via later calls to `aset`.

A mutable object stops being mutable if it is part of an expression
that is evaluated. For example:

```
(let* ((x (list 0.5))
       (y (eval (list 'quote x))))
  (setcar x 1.5) ;; The program should not do this.
  y)
```

Although the list `(0.5)` was mutable when it was created, it should not
have been changed via `setcar` because it was given to `eval`. The
reverse does not occur: an object that should not be changed never
becomes mutable afterwards.

If a program attempts to change objects that should not be
changed, the resulting behavior is undefined: the Lisp interpreter
might signal an error, or it might crash or behave unpredictably in
other ways.[3](#FOOT3)

When similar constants occur as parts of a program, the Lisp
interpreter might save time or space by reusing existing constants or
their components. For example, `(eq "abc" "abc")` returns
`t` if the interpreter creates only one instance of the string
literal `"abc"`, and returns `nil` if it creates two
instances. Lisp programs should be written so that they work
regardless of whether this optimization is in use.

---

Previous: [Mutability](#Mutability), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.10 Type Hierarchy of Emacs Lisp Objects [¶](#Type-Hierarchy-of-Emacs-Lisp-Objects)

Lisp object types are organized in a hierarchy, which means that types
can derive from other types. Objects of type B (which derives from type
A) inherit all the characteristics of type A. This also means that
every object of type B is at the same time an object of type A from
which it derives.

Every type derives from type `t`.

New types can be defined by the user through `defclass` or
`cl-defstruct`.

The Lisp Type Hierarchy for primitive types can be represented as
follows:

For example type `list` derives from (is a special kind of) type
`sequence` which itself derives from `t`.

---

Next: [Strings and Characters](#Strings-and-Characters), Previous: [Lisp Data Types](#Lisp-Data-Types), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 3 Numbers [¶](#Numbers-1)

GNU Emacs supports two numeric data types: *integers* and
*floating-point numbers*. Integers are whole numbers such as
−3, 0, 7, 13, and 511. Floating-point numbers are numbers with
fractional parts, such as −4.5, 0.0, and 2.71828. They can
also be expressed in exponential notation: ‘`1.5e2`’ is the same as
‘`150.0`’; here, ‘`e2`’ stands for ten to the second power, and
that is multiplied by 1.5. Integer computations are exact.
Floating-point computations often involve rounding errors, as the
numbers have a fixed amount of precision.

* [Integer Basics](#Integer-Basics)
* [Floating-Point Basics](#Float-Basics)
* [Type Predicates for Numbers](#Predicates-on-Numbers)
* [Comparison of Numbers](#Comparison-of-Numbers)
* [Numeric Conversions](#Numeric-Conversions)
* [Arithmetic Operations](#Arithmetic-Operations)
* [Rounding Operations](#Rounding-Operations)
* [Bitwise Operations on Integers](#Bitwise-Operations)
* [Standard Mathematical Functions](#Math-Functions)
* [Random Numbers](#Random-Numbers)

---

Next: [Floating-Point Basics](#Float-Basics), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.1 Integer Basics [¶](#Integer-Basics-1)

The Lisp reader reads an integer as a nonempty sequence
of decimal digits with optional initial sign and optional
final period.

```
 1               ; The integer 1.
 1.              ; The integer 1.
+1               ; Also the integer 1.
-1               ; The integer −1.
 0               ; The integer 0.
-0               ; The integer 0.
```

The syntax for integers in bases other than 10 consists of ‘`#`’
followed by a radix indication followed by one or more digits. The
radix indications are ‘`b`’ for binary, ‘`o`’ for octal,
‘`x`’ for hex, and ‘`radixr`’ for radix radix.
Thus, ‘`#binteger`’ reads
integer in binary, and ‘`#radixrinteger`’ reads
integer in radix radix. Allowed values of radix run
from 2 to 36, and allowed digits are the first radix characters
taken from ‘`0`’–‘`9`’, ‘`A`’–‘`Z`’.
Letter case is ignored and there is no initial sign or final period.
For example:

```
#b101100 ⇒ 44
#o54 ⇒ 44
#x2c ⇒ 44
#24r1k ⇒ 44
```

To understand how various functions work on integers, especially the
bitwise operators (see [Bitwise Operations on Integers](#Bitwise-Operations)), it is often helpful to
view the numbers in their binary form.

In binary, the decimal integer 5 looks like this:

```
...000101
```

(The ellipsis ‘`…`’ stands for a conceptually infinite number
of bits that match the leading bit; here, an infinite number of 0
bits. Later examples also use this ‘`…`’ notation.)

The integer −1 looks like this:

```
...111111
```

−1 is represented as all ones. (This is called *two’s
complement* notation.)

Subtracting 4 from −1 returns the negative integer −5.
In binary, the decimal integer 4 is 100. Consequently,
−5 looks like this:

```
...111011
```

Many of the functions described in this chapter accept markers for
arguments in place of numbers. (See [Markers](#Markers).) Since the actual
arguments to such functions may be either numbers or markers, we often
give these arguments the name number-or-marker. When the argument
value is a marker, its position value is used and its buffer is ignored.

In Emacs Lisp, text characters are represented by integers. Any
integer between zero and the value of `(max-char)`, inclusive, is
considered to be valid as a character. See [Character Codes](#Character-Codes).

Integers in Emacs Lisp are not limited to the machine word size.
Under the hood, though, there are two kinds of integers: smaller ones,
called *fixnums*, and larger ones, called *bignums*. Although
Emacs Lisp code ordinarily should not depend on whether an integer is
a fixnum or a bignum, older Emacs versions support only fixnums, some
functions in Emacs still accept only fixnums, and older Emacs Lisp
code may have trouble when given bignums. For example, while older
Emacs Lisp code could safely compare integers for numeric equality
with `eq`, the presence of bignums means that equality predicates
like `eql` and `=` should now be used to compare integers.

The range of values for bignums is limited by the amount of main
memory, by machine characteristics such as the size of the word used
to represent a bignum’s exponent, and by the `integer-width`
variable. These limits are typically much more generous than the
limits for fixnums. A bignum is never numerically equal to a fixnum;
Emacs always represents an integer in fixnum range as a fixnum, not a
bignum.

The range of values for a fixnum depends on the machine. The
minimum range is −536,870,912 to 536,870,911 (30 bits; i.e.,
−2\*\*29
to
2\*\*29 − 1),
but many machines provide a wider range.

Variable: **most-positive-fixnum** [¶](#index-most_002dpositive_002dfixnum)
:   The value of this variable is the greatest “small” integer that Emacs
    Lisp can handle. Typical values are
    2\*\*29 − 1
    on 32-bit and
    2\*\*61 − 1
    on 64-bit platforms.

Variable: **most-negative-fixnum** [¶](#index-most_002dnegative_002dfixnum)
:   The value of this variable is the numerically least “small” integer
    that Emacs Lisp can handle. It is negative. Typical values are
    −2\*\*29
    on 32-bit and
    −2\*\*61
    on 64-bit platforms.

Variable: **integer-width** [¶](#index-integer_002dwidth)
:   The value of this variable is a nonnegative integer that controls
    whether Emacs signals a range error when a large integer would be
    calculated. Integers with absolute values less than
    2\*\*n,
    where n is this variable’s value, do not signal a range error.
    Attempts to create larger integers typically signal a range error,
    although there might be no signal if a larger integer can be created cheaply.
    Setting this variable to a large number can be costly if a computation
    creates huge integers.

---

Next: [Type Predicates for Numbers](#Predicates-on-Numbers), Previous: [Integer Basics](#Integer-Basics), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.2 Floating-Point Basics [¶](#Floating_002dPoint-Basics)

Floating-point numbers are useful for representing numbers that are
not integral. The range of floating-point numbers is the same as the
range of the C data type `double` on the machine you are using.
On almost all computers supported by Emacs, this is IEEE
binary64 floating point format, which is standardized by
[IEEE Std
754-2019](https://standards.ieee.org/standard/754-2019.html) and is discussed further in David Goldberg’s paper
“[What Every Computer Scientist Should Know About Floating-Point
Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html)”. On modern platforms, floating-point operations follow
the IEEE-754 standard closely; however, results are not always rounded
correctly on some systems, notably 32-bit x86.

On some old computer systems, Emacs may not use IEEE floating-point.
We know of one such system on which Emacs runs correctly, but does not
follow IEEE-754: the VAX running NetBSD using GCC 10.4.0, where the
VAX ‘`D_Floating`’ format is used instead. IBM System/370-derived
mainframes and their XL/C compiler are also capable of utilizing a
hexadecimal floating point format, but Emacs has not yet been built in
such a configuration.

The read syntax for floating-point numbers requires either a decimal
point, an exponent, or both. Optional signs (‘`+`’ or ‘`-`’)
precede the number and its exponent. For example, ‘`1500.0`’,
‘`+15e2`’, ‘`15.0e+2`’, ‘`+1500000e-3`’, and ‘`.15e4`’ are
five ways of writing a floating-point number whose value is 1500.
They are all equivalent. Like Common Lisp, Emacs Lisp requires at
least one digit after a decimal point in a floating-point number that
does not have an exponent;
‘`1500.`’ is an integer, not a floating-point number.

Emacs Lisp treats `-0.0` as numerically equal to ordinary zero
with respect to numeric comparisons like `=`. This follows the
IEEE floating-point standard, which says `-0.0` and
`0.0` are numerically equal even though other operations can
distinguish them.

The IEEE floating-point standard supports positive
infinity and negative infinity as floating-point values. It also
provides for a class of values called NaN, or “not a number”;
numerical functions return such values in cases where there is no
correct answer. For example, `(/ 0.0 0.0)` returns a NaN.
A NaN is never numerically equal to any value, not even to itself.
NaNs carry a sign and a significand, and non-numeric functions treat
two NaNs as equal when their
signs and significands agree. Significands of NaNs are
machine-dependent, as are the digits in their string representation.

When NaNs and signed zeros are involved, non-numeric functions like
`eql`, `equal`, `sxhash-eql`, `sxhash-equal` and
`gethash` determine whether values are indistinguishable, not
whether they are numerically equal. For example, when x and
y are the same NaN, `(equal x y)` returns `t` whereas
`(= x y)` uses numeric comparison and returns `nil`;
conversely, `(equal 0.0 -0.0)` returns `nil` whereas
`(= 0.0 -0.0)` returns `t`.

Here are read syntaxes for these special floating-point values:

infinity
:   ‘`1.0e+INF`’ and ‘`-1.0e+INF`’

not-a-number
:   ‘`0.0e+NaN`’ and ‘`-0.0e+NaN`’

Infinities and NaNs are not available on legacy systems that lack
IEEE floating-point arithmetic. On a circa 1980 VAX, for example,
Lisp reads ‘`1.0e+INF`’ as a large but finite floating-point number,
and ‘`0.0e+NaN`’ as some other non-numeric Lisp object that provokes an
error if used numerically.

The following functions are specialized for handling floating-point
numbers:

Function: **isnan** x [¶](#index-isnan)
:   This predicate returns `t` if its floating-point argument is a NaN,
    `nil` otherwise.

Function: **frexp** x [¶](#index-frexp)
:   This function returns a cons cell `(s . e)`,
    where s and e are respectively the significand and
    exponent of the floating-point number x.

    If x is finite, then s is a floating-point number between 0.5
    (inclusive) and 1.0 (exclusive), e is an integer, and
    x = s \* 2\*\*e.
    If x is zero or infinity, then s is the same as x.
    If x is a NaN, then s is also a NaN.
    If x is zero, then e is 0.

Function: **ldexp** s e [¶](#index-ldexp)
:   Given a numeric significand s and an integer exponent e,
    this function returns the floating point number
    s \* 2\*\*e.

Function: **copysign** x1 x2 [¶](#index-copysign)
:   This function copies the sign of x2 to the value of x1,
    and returns the result. x1 and x2 must be floating point.

Function: **logb** x [¶](#index-logb)
:   This function returns the binary exponent of x. More precisely,
    if x is finite and nonzero, the value is the logarithm base 2 of
    *|x|*, rounded down to an integer. If x is zero or
    infinite, the value is infinity; if x is a NaN, the value is a
    NaN.

    ```
    (logb 10)
         ⇒ 3
    (logb 10.0e20)
         ⇒ 69
    (logb 0)
         ⇒ -1.0e+INF
    ```

---

Next: [Comparison of Numbers](#Comparison-of-Numbers), Previous: [Floating-Point Basics](#Float-Basics), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.3 Type Predicates for Numbers [¶](#Type-Predicates-for-Numbers)

The functions in this section test for numbers, or for a specific
type of number. The functions `integerp` and `floatp` can
take any type of Lisp object as argument (they would not be of much
use otherwise), but the `zerop` predicate requires a number as
its argument. See also `integer-or-marker-p` and
`number-or-marker-p`, in [Predicates on Markers](#Predicates-on-Markers).

Function: **bignump** object [¶](#index-bignump)
:   This predicate tests whether its argument is a large integer, and
    returns `t` if so, `nil` otherwise. Unlike small integers,
    large integers can be `=` or `eql` even if they are not `eq`.

Function: **fixnump** object [¶](#index-fixnump)
:   This predicate tests whether its argument is a small integer, and
    returns `t` if so, `nil` otherwise. Small integers can be
    compared with `eq`.

Function: **floatp** object [¶](#index-floatp)
:   This predicate tests whether its argument is floating point
    and returns `t` if so, `nil` otherwise.

Function: **integerp** object [¶](#index-integerp)
:   This predicate tests whether its argument is an integer, and returns
    `t` if so, `nil` otherwise.

Function: **numberp** object [¶](#index-numberp)
:   This predicate tests whether its argument is a number (either integer or
    floating point), and returns `t` if so, `nil` otherwise.

Function: **natnump** object [¶](#index-natnump)
:   This predicate (whose name comes from the phrase “natural number”)
    tests to see whether its argument is a nonnegative integer, and
    returns `t` if so, `nil` otherwise. 0 is considered
    non-negative.

    `wholenump` is a synonym for `natnump`.

Function: **zerop** number [¶](#index-zerop)
:   This predicate tests whether its argument is zero, and returns `t`
    if so, `nil` otherwise. The argument must be a number.

    `(zerop x)` is equivalent to `(= x 0)`.

Function: **plusp** number [¶](#index-plusp)
:   This predicate tests whether its argument is positive, and returns
    `t` if so, `nil` otherwise. The argument must be a number.

Function: **minusp** number [¶](#index-minusp)
:   This predicate tests whether its argument is negative, and returns
    `t` if so, `nil` otherwise. The argument must be a number.

Function: **oddp** integer [¶](#index-oddp)
:   This predicate tests whether its argument is an odd number, and returns
    `t` if so, `nil` otherwise. The argument must be an integer.

Function: **evenp** integer [¶](#index-evenp)
:   This predicate tests whether its argument is an even number, and returns
    `t` if so, `nil` otherwise. The argument must be an integer.

---

Next: [Numeric Conversions](#Numeric-Conversions), Previous: [Type Predicates for Numbers](#Predicates-on-Numbers), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.4 Comparison of Numbers [¶](#Comparison-of-Numbers-1)

To test numbers for numerical equality, you should normally use
`=` instead of non-numeric comparison predicates like `eq`,
`eql` and `equal`. Distinct floating-point and large
integer objects can be numerically equal. If you use `eq` to
compare them, you test whether they are the same *object*; if you
use `eql` or `equal`, you test whether their values are
*indistinguishable*. In contrast, `=` uses numeric
comparison, and sometimes returns `t` when a non-numeric
comparison would return `nil` and vice versa. See [Floating-Point Basics](#Float-Basics).

In Emacs Lisp, if two fixnums are numerically equal, they are the
same Lisp object. That is, `eq` is equivalent to `=` on
fixnums. It is sometimes convenient to use `eq` for comparing
an unknown value with a fixnum, because `eq` does not report an
error if the unknown value is not a number—it accepts arguments of
any type. By contrast, `=` signals an error if the arguments are
not numbers or markers. However, it is better programming practice to
use `=` if you can, even for comparing integers.

Sometimes it is useful to compare numbers with `eql` or `equal`,
which treat two numbers as equal if they have the same data type (both
integers, or both floating point) and the same value. By contrast,
`=` can treat an integer and a floating-point number as equal.
See [Equality Predicates](#Equality-Predicates).

There is another wrinkle: because floating-point arithmetic is not
exact, it is often a bad idea to check for equality of floating-point
values. Usually it is better to test for approximate equality.
Here’s a function to do this:

```
(defvar fuzz-factor 1.0e-6)
(defun approx-equal (x y)
  (or (= x y)
      (< (/ (abs (- x y))
            (max (abs x) (abs y)))
         fuzz-factor)))
```

Function: **=** number-or-marker &rest number-or-markers [¶](#index-_003d)
:   This function tests whether all its arguments are numerically equal,
    and returns `t` if so, `nil` otherwise.

Function: **eql** value1 value2 [¶](#index-eql)
:   This function acts like `eq` except when both arguments are
    numbers. It compares numbers by type and numeric value, so that
    `(eql 1.0 1)` returns `nil`, but `(eql 1.0 1.0)` and
    `(eql 1 1)` both return `t`. This can be used to compare
    large integers as well as small ones.
    Floating-point values with the same sign, exponent and fraction are `eql`.
    This differs from numeric comparison: `(eql 0.0 -0.0)` returns
    `nil` and `(eql 0.0e+NaN 0.0e+NaN)` returns `t`,
    whereas `=` does the opposite.

Function: **/=** number-or-marker1 number-or-marker2 [¶](#index-_002f_003d)
:   This function tests whether its arguments are numerically equal, and
    returns `t` if they are not, and `nil` if they are.

Function: **<** number-or-marker &rest number-or-markers [¶](#index-_003c)
:   This function tests whether each argument is strictly less than the
    following argument. It returns `t` if so, `nil` otherwise.

Function: **<=** number-or-marker &rest number-or-markers [¶](#index-_003c_003d)
:   This function tests whether each argument is less than or equal to
    the following argument. It returns `t` if so, `nil` otherwise.

Function: **>** number-or-marker &rest number-or-markers [¶](#index-_003e)
:   This function tests whether each argument is strictly greater than
    the following argument. It returns `t` if so, `nil` otherwise.

Function: **>=** number-or-marker &rest number-or-markers [¶](#index-_003e_003d)
:   This function tests whether each argument is greater than or equal to
    the following argument. It returns `t` if so, `nil` otherwise.

Function: **max** number-or-marker &rest numbers-or-markers [¶](#index-max)
:   This function returns the largest of its arguments.

    ```
    (max 20)
         ⇒ 20
    (max 1 2.5)
         ⇒ 2.5
    (max 1 3 2.5)
         ⇒ 3
    ```

Function: **min** number-or-marker &rest numbers-or-markers [¶](#index-min)
:   This function returns the smallest of its arguments.

    ```
    (min -4 1)
         ⇒ -4
    ```

Function: **abs** number [¶](#index-abs)
:   This function returns the absolute value of number.

---

Next: [Arithmetic Operations](#Arithmetic-Operations), Previous: [Comparison of Numbers](#Comparison-of-Numbers), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.5 Numeric Conversions [¶](#Numeric-Conversions-1)

To convert an integer to floating point, use the function `float`.

Function: **float** number [¶](#index-float)
:   This returns number converted to floating point.
    If number is already floating point, `float` returns
    it unchanged.

There are four functions to convert floating-point numbers to
integers; they differ in how they round. All accept an argument
number and an optional argument divisor. Both arguments
may be integers or floating-point numbers. divisor may also be
`nil`. If divisor is `nil` or omitted, these
functions convert number to an integer, or return it unchanged
if it already is an integer. If divisor is non-`nil`, they
divide number by divisor and convert the result to an
integer. If divisor is zero (whether integer or
floating point), Emacs signals an `arith-error` error.

Function: **truncate** number &optional divisor [¶](#index-truncate)
:   This returns number, converted to an integer by rounding towards
    zero.

    ```
    (truncate 1.2)
         ⇒ 1
    (truncate 1.7)
         ⇒ 1
    (truncate -1.2)
         ⇒ -1
    (truncate -1.7)
         ⇒ -1
    ```

Function: **floor** number &optional divisor [¶](#index-floor)
:   This returns number, converted to an integer by rounding downward
    (towards negative infinity).

    If divisor is specified, this uses the kind of division
    operation that corresponds to `mod`, rounding downward.

    ```
    (floor 1.2)
         ⇒ 1
    (floor 1.7)
         ⇒ 1
    (floor -1.2)
         ⇒ -2
    (floor -1.7)
         ⇒ -2
    (floor 5.99 3)
         ⇒ 1
    ```

Function: **ceiling** number &optional divisor [¶](#index-ceiling)
:   This returns number, converted to an integer by rounding upward
    (towards positive infinity).

    ```
    (ceiling 1.2)
         ⇒ 2
    (ceiling 1.7)
         ⇒ 2
    (ceiling -1.2)
         ⇒ -1
    (ceiling -1.7)
         ⇒ -1
    ```

Function: **round** number &optional divisor [¶](#index-round)
:   This returns number, converted to an integer by rounding towards the
    nearest integer. Rounding a value equidistant between two integers
    returns the even integer.

    ```
    (round 1.2)
         ⇒ 1
    (round 1.7)
         ⇒ 2
    (round -1.2)
         ⇒ -1
    (round -1.7)
         ⇒ -2
    ```

---

Next: [Rounding Operations](#Rounding-Operations), Previous: [Numeric Conversions](#Numeric-Conversions), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.6 Arithmetic Operations [¶](#Arithmetic-Operations-1)

Emacs Lisp provides the traditional four arithmetic operations
(addition, subtraction, multiplication, and division), as well as
remainder and modulus functions, and functions to add or subtract 1.
Except for `%`, each of these functions accepts both integer and
floating-point arguments, and returns a floating-point number if any
argument is floating point.

Function: **1+** number-or-marker [¶](#index-1_002b)
:   This function returns number-or-marker plus 1.
    For example,

    ```
    (setq foo 4)
         ⇒ 4
    (1+ foo)
         ⇒ 5
    ```

    This function is not analogous to the C operator `++`—it does not
    increment a variable. It just computes a sum. Thus, if we continue,

    ```
    foo
         ⇒ 4
    ```

    If you want to increment the variable, you must use `setq` (or
    `incf`), like this:

    ```
    (setq foo (1+ foo))
         ⇒ 5
    ```

Function: **1-** number-or-marker [¶](#index-1_002d)
:   This function returns number-or-marker minus 1.

Macro: **incf** place &optional delta [¶](#index-incf)
:   This macro increments the number stored in place by one, or
    by delta if specified. It returns the incremented value.

    place can be a symbol or a generalized variable,
    see [Generalized Variables](#Generalized-Variables). For example, ‘`(incf i)`’ is
    equivalent to ‘`(setq i (1+ i))`’, and ‘`(incf (car x) 2)`’ is equivalent to ‘`(setcar x (+ (car x) 2))`’.

Macro: **decf** place &optional delta [¶](#index-decf)
:   This macro decrements the number stored in place by one, or
    by delta if specified. It returns the decremented value.

Function: **+** &rest numbers-or-markers [¶](#index-_002b)
:   This function adds its arguments together. When given no arguments,
    `+` returns 0.

    ```
    (+)
         ⇒ 0
    (+ 1)
         ⇒ 1
    (+ 1 2 3 4)
         ⇒ 10
    ```

Function: **-** &optional number-or-marker &rest more-numbers-or-markers [¶](#index-_002d)
:   The `-` function serves two purposes: negation and subtraction.
    When `-` has a single argument, the value is the negative of the
    argument. When there are multiple arguments, `-` subtracts each of
    the more-numbers-or-markers from number-or-marker,
    cumulatively. If there are no arguments, the result is 0.

    ```
    (- 10 1 2 3 4)
         ⇒ 0
    (- 10)
         ⇒ -10
    (-)
         ⇒ 0
    ```

Function: **\*** &rest numbers-or-markers [¶](#index-_002a)
:   This function multiplies its arguments together, and returns the
    product. When given no arguments, `*` returns 1.

    ```
    (*)
         ⇒ 1
    (* 1)
         ⇒ 1
    (* 1 2 3 4)
         ⇒ 24
    ```

Function: **/** number &rest divisors [¶](#index-_002f)
:   With one or more divisors, this function divides number
    by each divisor in divisors in turn, and returns the quotient.
    With no divisors, this function returns 1/number, i.e.,
    the multiplicative inverse of number. Each argument may be a
    number or a marker.

    If all the arguments are integers, the result is an integer, obtained
    by rounding the quotient towards zero after each division.

    ```
    (/ 6 2)
         ⇒ 3
    ```

    ```
    (/ 5 2)
         ⇒ 2
    ```

    ```
    (/ 5.0 2)
         ⇒ 2.5
    ```

    ```
    (/ 5 2.0)
         ⇒ 2.5
    ```

    ```
    (/ 5.0 2.0)
         ⇒ 2.5
    ```

    ```
    (/ 4.0)
         ⇒ 0.25
    ```

    ```
    (/ 4)
         ⇒ 0
    ```

    ```
    (/ 25 3 2)
         ⇒ 4
    ```

    ```
    (/ -17 6)
         ⇒ -2
    ```

    If you divide an integer by the integer 0, Emacs signals an
    `arith-error` error (see [Errors](#Errors)). On systems using IEEE-754
    floating-point, floating-point division of a nonzero number by zero
    yields either positive or negative infinity (see [Floating-Point Basics](#Float-Basics));
    otherwise, an `arith-error` is signaled as usual.

Function: **%** dividend divisor [¶](#index-_0025)
:   This function returns the integer remainder after division of dividend
    by divisor. The arguments must be integers or markers.

    For any two integers dividend and divisor,

    ```
    (+ (% dividend divisor)
       (* (/ dividend divisor) divisor))
    ```

    always equals dividend if divisor is nonzero.

    ```
    (% 9 4)
         ⇒ 1
    (% -9 4)
         ⇒ -1
    (% 9 -4)
         ⇒ 1
    (% -9 -4)
         ⇒ -1
    ```

Function: **mod** dividend divisor [¶](#index-mod)
:   This function returns the value of dividend modulo divisor;
    in other words, the remainder after division of dividend
    by divisor, but with the same sign as divisor.
    The arguments must be numbers or markers.

    Unlike `%`, `mod` permits floating-point arguments; it
    rounds the quotient downward (towards minus infinity) to an integer,
    and uses that quotient to compute the remainder.

    If divisor is zero, `mod` signals an `arith-error`
    error if both arguments are integers, and returns a NaN otherwise.

    ```
    (mod 9 4)
         ⇒ 1
    ```

    ```
    (mod -9 4)
         ⇒ 3
    ```

    ```
    (mod 9 -4)
         ⇒ -3
    ```

    ```
    (mod -9 -4)
         ⇒ -1
    ```

    ```
    (mod 5.5 2.5)
         ⇒ .5
    ```

    For any two numbers dividend and divisor,

    ```
    (+ (mod dividend divisor)
       (* (floor dividend divisor) divisor))
    ```

    always equals dividend, subject to rounding error if either
    argument is floating point and to an `arith-error` if dividend is an
    integer and divisor is 0. For `floor`, see [Numeric Conversions](#Numeric-Conversions).

---

Next: [Bitwise Operations on Integers](#Bitwise-Operations), Previous: [Arithmetic Operations](#Arithmetic-Operations), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.7 Rounding Operations [¶](#Rounding-Operations-1)

The functions `ffloor`, `fceiling`, `fround`, and
`ftruncate` take a floating-point argument and return a floating-point
result whose value is a nearby integer. `ffloor` returns the
nearest integer below; `fceiling`, the nearest integer above;
`ftruncate`, the nearest integer in the direction towards zero;
`fround`, the nearest integer.

Function: **ffloor** float [¶](#index-ffloor)
:   This function rounds float to the next lower integral value, and
    returns that value as a floating-point number.

Function: **fceiling** float [¶](#index-fceiling)
:   This function rounds float to the next higher integral value, and
    returns that value as a floating-point number.

Function: **ftruncate** float [¶](#index-ftruncate)
:   This function rounds float towards zero to an integral value, and
    returns that value as a floating-point number.

Function: **fround** float [¶](#index-fround)
:   This function rounds float to the nearest integral value,
    and returns that value as a floating-point number.
    Rounding a value equidistant between two integers returns the even integer.

---

Next: [Standard Mathematical Functions](#Math-Functions), Previous: [Rounding Operations](#Rounding-Operations), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.8 Bitwise Operations on Integers [¶](#Bitwise-Operations-on-Integers)

In a computer, an integer is represented as a binary number, a
sequence of *bits* (digits which are either zero or one).
Conceptually the bit sequence is infinite on the left, with the
most-significant bits being all zeros or all ones. A bitwise
operation acts on the individual bits of such a sequence. For example,
*shifting* moves the whole sequence left or right one or more places,
reproducing the same pattern moved over.

The bitwise operations in Emacs Lisp apply only to integers.

Function: **ash** integer count [¶](#index-ash)
:   `ash` (*arithmetic shift*) shifts the bits in integer
    to the left count places, or to the right if count is
    negative. Left shifts introduce zero bits on the right; right shifts
    discard the rightmost bits. Considered as an integer operation,
    `ash` multiplies integer by
    2\*\*count,
    and then converts the result to an integer by rounding downward, toward
    minus infinity.

    Here are examples of `ash`, shifting a pattern of bits one place
    to the left and to the right. These examples show only the low-order
    bits of the binary pattern; leading bits all agree with the
    highest-order bit shown. As you can see, shifting left by one is
    equivalent to multiplying by two, whereas shifting right by one is
    equivalent to dividing by two and then rounding toward minus infinity.

    ```
    (ash 7 1) ⇒ 14
    ;; Decimal 7 becomes decimal 14.
    ...000111
         ⇒
    ...001110
    ```

    ```

    ```

    ```
    (ash 7 -1) ⇒ 3
    ...000111
         ⇒
    ...000011
    ```

    ```

    ```

    ```
    (ash -7 1) ⇒ -14
    ...111001
         ⇒
    ...110010
    ```

    ```

    ```

    ```
    (ash -7 -1) ⇒ -4
    ...111001
         ⇒
    ...111100
    ```

    Here are examples of shifting left or right by two bits:

    ```
                      ;         binary values
    (ash 5 2)         ;   5  =  ...000101
         ⇒ 20         ;      =  ...010100
    (ash -5 2)        ;  -5  =  ...111011
         ⇒ -20        ;      =  ...101100
    ```

    ```
    (ash 5 -2)
         ⇒ 1          ;      =  ...000001
    ```

    ```
    (ash -5 -2)
         ⇒ -2         ;      =  ...111110
    ```

Function: **lsh** integer count [¶](#index-lsh)
:   `lsh`, which is an abbreviation for *logical shift*, shifts the
    bits in integer to the left count places, or to the right
    if count is negative, bringing zeros into the vacated bits. If
    count is negative, then integer must be either a fixnum
    or a positive bignum, and `lsh` treats a negative fixnum as if it
    were unsigned by subtracting twice `most-negative-fixnum` before
    shifting, producing a nonnegative result. This quirky behavior dates
    back to when Emacs supported only fixnums; nowadays `ash` is a
    better choice.

    As `lsh` behaves like `ash` except when integer and
    count are both negative, the following examples focus on these
    exceptional cases. These examples assume 30-bit fixnums.

    ```
                     ;      binary values
    (ash -7 -1)      ; -7 = ...111111111111111111111111111001
         ⇒ -4        ;    = ...111111111111111111111111111100
    (lsh -7 -1)
         ⇒ 536870908 ;    = ...011111111111111111111111111100
    ```

    ```
    (ash -5 -2)      ; -5 = ...111111111111111111111111111011
         ⇒ -2        ;    = ...111111111111111111111111111110
    (lsh -5 -2)
         ⇒ 268435454 ;    = ...001111111111111111111111111110
    ```

Function: **logand** &rest ints-or-markers [¶](#index-logand)
:   This function returns the bitwise AND of the arguments: the nth
    bit is 1 in the result if, and only if, the nth bit is 1 in all
    the arguments.

    For example, using 4-bit binary numbers, the bitwise AND of 13 and
    12 is 12: 1101 combined with 1100 produces 1100.
    In both the binary numbers, the leftmost two bits are both 1
    so the leftmost two bits of the returned value are both 1.
    However, for the rightmost two bits, each is 0 in at least one of
    the arguments, so the rightmost two bits of the returned value are both 0.

    Therefore,

    ```
    (logand 13 12)
         ⇒ 12
    ```

    If `logand` is not passed any argument, it returns a value of
    −1. This number is an identity element for `logand`
    because its binary representation consists entirely of ones. If
    `logand` is passed just one argument, it returns that argument.

    ```
                       ;        binary values

    (logand 14 13)     ; 14  =  ...001110
                       ; 13  =  ...001101
         ⇒ 12         ; 12  =  ...001100
    ```

    ```

    ```

    ```
    (logand 14 13 4)   ; 14  =  ...001110
                       ; 13  =  ...001101
                       ;  4  =  ...000100
         ⇒ 4          ;  4  =  ...000100
    ```

    ```

    ```

    ```
    (logand)
         ⇒ -1         ; -1  =  ...111111
    ```

Function: **logior** &rest ints-or-markers [¶](#index-logior)
:   This function returns the bitwise inclusive OR of its arguments: the nth
    bit is 1 in the result if, and only if, the nth bit is 1 in at
    least one of the arguments. If there are no arguments, the result is 0,
    which is an identity element for this operation. If `logior` is
    passed just one argument, it returns that argument.

    ```
                       ;        binary values

    (logior 12 5)      ; 12  =  ...001100
                       ;  5  =  ...000101
         ⇒ 13         ; 13  =  ...001101
    ```

    ```

    ```

    ```
    (logior 12 5 7)    ; 12  =  ...001100
                       ;  5  =  ...000101
                       ;  7  =  ...000111
         ⇒ 15         ; 15  =  ...001111
    ```

Function: **logxor** &rest ints-or-markers [¶](#index-logxor)
:   This function returns the bitwise exclusive OR of its arguments: the
    nth bit is 1 in the result if, and only if, the nth bit is
    1 in an odd number of the arguments. If there are no arguments, the
    result is 0, which is an identity element for this operation. If
    `logxor` is passed just one argument, it returns that argument.

    ```
                       ;        binary values

    (logxor 12 5)      ; 12  =  ...001100
                       ;  5  =  ...000101
         ⇒ 9          ;  9  =  ...001001
    ```

    ```

    ```

    ```
    (logxor 12 5 7)    ; 12  =  ...001100
                       ;  5  =  ...000101
                       ;  7  =  ...000111
         ⇒ 14         ; 14  =  ...001110
    ```

Function: **lognot** integer [¶](#index-lognot)
:   This function returns the bitwise complement of its argument: the nth
    bit is one in the result if, and only if, the nth bit is zero in
    integer, and vice-versa. The result equals −1 −
    integer.

    ```
    (lognot 5)
         ⇒ -6
    ;;  5  =  ...000101
    ;; becomes
    ;; -6  =  ...111010
    ```

Function: **logcount** integer [¶](#index-logcount)
:   This function returns the *Hamming weight* of integer: the
    number of ones in the binary representation of integer.
    If integer is negative, it returns the number of zero bits in
    its two’s complement binary representation. The result is always
    nonnegative.

    ```
    (logcount 43)     ;  43 = ...000101011
         ⇒ 4
    (logcount -43)    ; -43 = ...111010101
         ⇒ 3
    ```

---

Next: [Random Numbers](#Random-Numbers), Previous: [Bitwise Operations on Integers](#Bitwise-Operations), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.9 Standard Mathematical Functions [¶](#Standard-Mathematical-Functions)

These mathematical functions allow integers as well as floating-point
numbers as arguments.

Function: **sin** arg [¶](#index-sin)

Function: **cos** arg [¶](#index-cos)

Function: **tan** arg [¶](#index-tan)
:   These are the basic trigonometric functions, with argument arg
    measured in radians.

Function: **asin** arg [¶](#index-asin)
:   The value of `(asin arg)` is a number between
    −pi/2
    and
    pi/2
    (inclusive) whose sine is arg. If arg is out of range
    (outside [−1, 1]), `asin` returns a NaN.

Function: **acos** arg [¶](#index-acos)
:   The value of `(acos arg)` is a number between 0 and
    pi
    (inclusive) whose cosine is arg. If arg is out of range
    (outside [−1, 1]), `acos` returns a NaN.

Function: **atan** y &optional x [¶](#index-atan)
:   The value of `(atan y)` is a number between
    −pi/2
    and
    pi/2
    (exclusive) whose tangent is y. If the optional second
    argument x is given, the value of `(atan y x)` is the
    angle in radians between the vector `[x, y]` and the
    `X` axis.

Function: **exp** arg [¶](#index-exp)
:   This is the exponential function; it returns *e* to the power
    arg.

Function: **log** arg &optional base [¶](#index-log)
:   This function returns the logarithm of arg, with base
    base. If you don’t specify base, the natural base
    *e* is used. If arg or base is negative, `log`
    returns a NaN.

Function: **expt** x y [¶](#index-expt)
:   This function returns x raised to power y. If both
    arguments are integers and y is nonnegative, the result is an
    integer; in this case, overflow signals an error, so watch out.
    If x is a finite negative number and y is a finite
    non-integer, `expt` returns a NaN.

Function: **sqrt** arg [¶](#index-sqrt)
:   This returns the square root of arg. If arg is finite
    and less than zero, `sqrt` returns a NaN.

In addition, Emacs defines the following common mathematical
constants:

Variable: **float-e** [¶](#index-float_002de)
:   The mathematical constant *e* (2.71828…).

Variable: **float-pi** [¶](#index-float_002dpi)
:   The mathematical constant *pi* (3.14159…).

---

Previous: [Standard Mathematical Functions](#Math-Functions), Up: [Numbers](#Numbers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.10 Random Numbers [¶](#Random-Numbers-1)

A deterministic computer program cannot generate true random
numbers. For most purposes, *pseudo-random numbers* suffice. A
series of pseudo-random numbers is generated in a deterministic
fashion. The numbers are not truly random, but they have certain
properties that mimic a random series. For example, all possible
values occur equally often in a pseudo-random series.

Pseudo-random numbers are generated from a *seed value*. Starting from
any given seed, the `random` function always generates the same
sequence of numbers. By default, Emacs initializes the random seed at
startup, in such a way that the sequence of values of `random`
(with overwhelming likelihood) differs in each Emacs run.
The random seed is typically initialized from system entropy;
however, on obsolescent platforms lacking entropy pools,
the seed is taken from less-random volatile data such as the current time.

Sometimes you want the random number sequence to be repeatable. For
example, when debugging a program whose behavior depends on the random
number sequence, it is helpful to get the same behavior in each
program run. To make the sequence repeat, execute `(random "")`.
This sets the seed to a constant value for your particular Emacs
executable (though it may differ for other Emacs builds). You can use
other strings to choose various seed values.

Function: **random** &optional limit [¶](#index-random)
:   This function returns a pseudo-random integer. Repeated calls return a
    series of pseudo-random integers.

    If limit is a positive integer, the value is chosen to be
    nonnegative and less than limit. Otherwise, the value might be
    any fixnum, i.e., any integer from `most-negative-fixnum` through
    `most-positive-fixnum` (see [Integer Basics](#Integer-Basics)).

    If limit is a string, it means to choose a new seed based on the
    string’s contents. This causes later calls to `random` to return
    a reproducible sequence of results.

    If limit is `t`, it means to choose a new seed as if Emacs
    were restarting. This causes later calls to `random` to return
    an unpredictable sequence of results.

If you need a random nonce for cryptographic purposes, using
`random` is typically not the best approach, for several reasons:

* Although you can use `(random t)` to consult system entropy,
  doing so can adversely affect other parts of your program that benefit
  from reproducible results.
* The system-dependent pseudo-random number generator (PRNG) used by
  `random` is not necessarily suitable for cryptography.
* A call to `(random t)` does not give direct access to system
  entropy; the entropy is passed through the system-dependent PRNG, thus
  possibly biasing the results.
* On typical platforms the random seed contains only 32 bits, which is
  typically narrower than an Emacs fixnum, and is not nearly enough for
  cryptographic purposes.
* A `(random t)` call leaves information about the nonce scattered
  about Emacs’s internal state, increasing the size of the internal
  attack surface.
* On obsolescent platforms lacking entropy pools, `(random t)` is
  seeded from a cryptographically weak source.

---

Next: [Lists](#Lists), Previous: [Numbers](#Numbers), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 4 Strings and Characters [¶](#Strings-and-Characters-1)

A string in Emacs Lisp is an array that contains an ordered sequence
of characters. Strings are used as names of symbols, buffers, and
files; to send messages to users; to hold text being copied between
buffers; and for many other purposes. Because strings are so important,
Emacs Lisp has many functions expressly for manipulating them. Emacs
Lisp programs use strings more often than individual characters.

See [Putting Keyboard Events in Strings](#Strings-of-Events), for special considerations for strings of
keyboard character events.

* [String and Character Basics](#String-Basics)
* [Predicates for Strings](#Predicates-for-Strings)
* [Creating Strings](#Creating-Strings)
* [Modifying Strings](#Modifying-Strings)
* [Comparison of Characters and Strings](#Text-Comparison)
* [Conversion of Characters and Strings](#String-Conversion)
* [Formatting Strings](#Formatting-Strings)
* [Custom Format Strings](#Custom-Format-Strings)
* [Case Conversion in Lisp](#Case-Conversion)
* [The Case Table](#Case-Tables)

---

Next: [Predicates for Strings](#Predicates-for-Strings), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
