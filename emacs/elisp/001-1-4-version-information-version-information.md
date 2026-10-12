### 1.4 Version Information [¶](#Version-Information)

These facilities provide information about which version of Emacs is
in use.

Command: **emacs-version** &optional here [¶](#index-emacs_002dversion)
:   This function returns a string describing the version of Emacs that is
    running. It is useful to include this string in bug reports.

    ```
    (emacs-version)
      ⇒ "GNU Emacs 26.1 (build 1, x86_64-unknown-linux-gnu,
                 GTK+ Version 3.16) of 2017-06-01"
    ```

    If here is non-`nil`, it inserts the text in the buffer
    before point, and returns `nil`. When this function is called
    interactively, it prints the same information in the echo area, but
    giving a prefix argument makes here non-`nil`.

Variable: **emacs-build-time** [¶](#index-emacs_002dbuild_002dtime)
:   The value of this variable indicates the time at which Emacs was
    built. It uses the style of
    `current-time` (see [Time of Day](#Time-of-Day)), or is `nil`
    if the information is not available.

    ```
    emacs-build-time
         ⇒ (25194 55894 8547 617000)
    ```

    (This timestamp is `(1651169878008547617 . 1000000000)`
    if `current-time-list` was `nil` when Emacs was built.)

Variable: **emacs-version** [¶](#index-emacs_002dversion-1)
:   The value of this variable is the version of Emacs being run. It is a
    string such as `"26.1"`. A value with three numeric components,
    such as `"26.0.91"`, indicates an unreleased test version.
    (Prior to Emacs 26.1, the string includes an extra final component
    with the integer that is now stored in `emacs-build-number`;
    e.g., `"25.1.1"`.)

Variable: **emacs-major-version** [¶](#index-emacs_002dmajor_002dversion)
:   The major version number of Emacs, as an integer. For Emacs version
    23.1, the value is 23.

Variable: **emacs-minor-version** [¶](#index-emacs_002dminor_002dversion)
:   The minor version number of Emacs, as an integer. For Emacs version
    23.1, the value is 1.

Variable: **emacs-build-number** [¶](#index-emacs_002dbuild_002dnumber)
:   An integer that increments each time Emacs is built in the same
    directory (without cleaning). This is only of relevance when
    developing Emacs.

Variable: **emacs-repository-version** [¶](#index-emacs_002drepository_002dversion)
:   A string that gives the repository revision from which Emacs was
    built. If Emacs was built outside revision control, the value is
    `nil`.

Variable: **emacs-repository-branch** [¶](#index-emacs_002drepository_002dbranch)
:   A string that gives the repository branch from which Emacs was built.
    In the most cases this is `"master"`. If Emacs was built outside
    revision control, the value is `nil`.

---

Previous: [Version Information](#Version-Info), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.5 Acknowledgments [¶](#Acknowledgments-1)

This manual was originally written by Robert Krawitz, Bil Lewis, Dan
LaLiberte, Richard M. Stallman and Chris Welty, the volunteers of
the GNU manual group, in an effort extending over several years.
Robert J. Chassell helped to review and edit the manual, with the
support of the Defense Advanced Research Projects Agency, ARPA Order
6082, arranged by Warren A. Hunt, Jr. of Computational Logic,
Inc. Additional sections have since been written by Miles Bader, Lars
Brinkhoff, Chong Yidong, Kenichi Handa, Lute Kamstra, Juri Linkov,
Glenn Morris, Thien-Thi Nguyen, Dan Nicolaescu, Martin Rudalics, Kim
F. Storm, Luc Teirlinck, and Eli Zaretskii, and others.

Corrections were supplied by Drew Adams, Juanma Barranquero, Karl
Berry, Jim Blandy, Bard Bloom, Stephane Boucher, David Boyes, Alan
Carroll, Richard Davis, Lawrence R. Dodd, Peter Doornbosch, David
A. Duff, Chris Eich, Beverly Erlebacher, David Eckelkamp, Ralf Fassel,
Eirik Fuller, Stephen Gildea, Bob Glickstein, Eric Hanchrow, Jesper
Harder, George Hartzell, Nathan Hess, Masayuki Ida, Dan Jacobson, Jak
Kirman, Bob Knighten, Frederick M. Korz, Joe Lammens, Glenn M. Lewis,
K. Richard Magill, Brian Marick, Roland McGrath, Stefan Monnier, Skip
Montanaro, John Gardiner Myers, Thomas A. Peterson, Francesco Potortì,
Friedrich Pukelsheim, Arnold D. Robbins, Raul Rockwell, Jason Rumney,
Per Starbäck, Shinichirou Sugou, Kimmo Suominen, Edward Tharp, Bill
Trost, Rickard Westman, Jean White, Eduard Wiebe, Matthew Wilding,
Carl Witty, Dale Worley, Rusty Wright, and David D. Zuhn.

For a more complete list of contributors, please see the relevant
change log entries in the Emacs source repository.

---

Next: [Numbers](#Numbers), Previous: [Introduction](#Introduction), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 2 Lisp Data Types [¶](#Lisp-Data-Types-1)

A Lisp *object* is a piece of data used and manipulated by Lisp
programs. For our purposes, a *type* or *data type* is a set of
possible objects.

Every object belongs to at least one type. Objects of the same type
have similar structures and may usually be used in the same contexts.
Types can overlap, and objects can belong to two or more types.
Consequently, we can ask whether an object belongs to a particular type,
but not for *the* type of an object.

A few fundamental object types are built into Emacs. These, from
which all other types are constructed, are called *primitive types*.
Each object belongs to one and only one primitive type. These types
include *integer*, *float*, *cons*, *symbol*,
*string*, *vector*, *hash-table*, *subr*,
*byte-code function*, and *record*, plus several special
types, such as *buffer*, that are related to editing.
(See [Editing Types](#Editing-Types).)

Each primitive type has a corresponding Lisp function that checks
whether an object is a member of that type.

Lisp is unlike many other languages in that its objects are
*self-typing*: the primitive type of each object is implicit in
the object itself. For example, if an object is a vector, nothing can
treat it as a number; Lisp knows it is a vector, not a number.

In most languages, the programmer must declare the data type of each
variable, and the type is known by the compiler but not represented in
the data. Such type declarations do not exist in Emacs Lisp. A Lisp
variable can have any type of value, and it remembers whatever value
you store in it, type and all. (Actually, a small number of Emacs
Lisp variables can only take on values of a certain type.
See [Variables with Restricted Values](#Variables-with-Restricted-Values).)

This chapter describes the purpose, printed representation, and read
syntax of each of the standard types in GNU Emacs Lisp. Details on how
to use these types can be found in later chapters.

* [Printed Representation and Read Syntax](#Printed-Representation)
* [Special Read Syntax](#Special-Read-Syntax)
* [Comments](#Comments)
* [Programming Types](#Programming-Types)
* [Editing Types](#Editing-Types)
* [Read Syntax for Circular Objects](#Circular-Objects)
* [Type Predicates](#Type-Predicates)
* [Equality Predicates](#Equality-Predicates)
* [Mutability](#Mutability)
* [Type Hierarchy of Emacs Lisp Objects](#Type-Hierarchy)

---

Next: [Special Read Syntax](#Special-Read-Syntax), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.1 Printed Representation and Read Syntax [¶](#Printed-Representation-and-Read-Syntax)

The *printed representation* of an object is the format of the
output generated by the Lisp printer (the function `prin1`) for
that object. Every data type has a unique printed representation.
The *read syntax* of an object is the format of the input accepted
by the Lisp reader (the function `read`) for that object. This
is not necessarily unique; many kinds of object have more than one
syntax. See [Reading and Printing Lisp Objects](#Read-and-Print).

In most cases, an object’s printed representation is also a read
syntax for the object. However, some types have no read syntax, since
it does not make sense to enter objects of these types as constants in
a Lisp program. These objects are printed in *hash notation*,
which consists of the characters ‘`#<`’, a descriptive string
(typically the type name followed by the name of the object), and a
closing ‘`>`’. (This is called “hash notation” because it begins
with the ‘`#`’ character, known as “hash” or “number sign”).
For example:

```
(current-buffer)
     ⇒ #<buffer objects.texi>
```

Hash notation cannot be read at all, so the Lisp reader signals the
error `invalid-read-syntax` whenever it encounters ‘`#<`’.

We describe the read syntax and the printed representation of each
Lisp data type where we describe that data type, in the following
sections of this chapter. For example, see [String Type](#String-Type), and its
subsections for the read syntax and printed representation of strings;
see [Vector Type](#Vector-Type) for the same information about vectors; etc.

In other languages, an expression is text; it has no other form. In
Lisp, an expression is primarily a Lisp object and only secondarily the
text that is the object’s read syntax. Often there is no need to
emphasize this distinction, but you must keep it in the back of your
mind, or you will occasionally be very confused.

When you evaluate an expression interactively, the Lisp interpreter
first reads the textual representation of it, producing a Lisp object,
and then evaluates that object (see [Evaluation](#Evaluation)). However,
evaluation and reading are separate activities. Reading returns the
Lisp object represented by the text that is read; the object may or may
not be evaluated later. See [Input Functions](#Input-Functions), for a description of
`read`, the basic function for reading objects.

---

Next: [Comments](#Comments), Previous: [Printed Representation and Read Syntax](#Printed-Representation), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.2 Special Read Syntax [¶](#Special-Read-Syntax-1)

Emacs Lisp represents many special objects and constructs via
special hash notations.

‘`#<…>`’
:   Objects that have no read syntax are presented like this
    (see [Printed Representation and Read Syntax](#Printed-Representation)).

‘`##`’
:   The printed representation of an interned symbol whose name is an
    empty string (see [Symbol Type](#Symbol-Type)).

‘`#'`’
:   This is a shortcut for `function`, see [Anonymous Functions](#Anonymous-Functions).

‘`#:`’
:   The printed representation of an uninterned symbol whose name is
    foo is ‘`#:foo`’ (see [Symbol Type](#Symbol-Type)).

‘`#N`’
:   When printing circular structures, this construct is used to represent
    where the structure loops back onto itself, and ‘`N`’ is the
    starting list count:

    ```
    (let ((a (list 1)))
      (setcdr a a))
    => (1 . #0)
    ```

‘`#N=`’

‘`#N#`’
:   ‘`#N=`’ gives the name to an object, and ‘`#N#`’ represents that
    object, so when reading back the object, they will be the same object
    instead of copies (see [Read Syntax for Circular Objects](#Circular-Objects)).

‘`#xN`’
:   ‘`N`’ represented as a hexadecimal number (‘`#x2a`’).

‘`#oN`’
:   ‘`N`’ represented as an octal number (‘`#o52`’).

‘`#bN`’
:   ‘`N`’ represented as a binary number (‘`#b101010`’).

‘`#(…)`’
:   String text properties (see [Text Properties in Strings](#Text-Props-and-Strings)).

‘`#^`’
:   A char table (see [Char-Table Type](#Char_002dTable-Type)).

‘`#s(hash-table …)`’
:   A hash table (see [Hash Table Type](#Hash-Table-Type)).

‘`?C`’
:   A character (see [Basic Char Syntax](#Basic-Char-Syntax)).

‘`#$`’
:   The current file name in byte-compiled files (see [Documentation Strings and Compilation](#Docs-and-Compilation)). This is not meant to be used in Emacs Lisp source
    files.

‘`#@N`’
:   Skip the next ‘`N`’ characters (see [Comments](#Comments)). This is used in
    byte-compiled files, and is not meant to be used in Emacs Lisp source
    files.

‘`#f`’
:   Indicates that the following form isn’t readable by the Emacs Lisp
    reader. This is only in text for display purposes (when that would
    look prettier than alternative ways of indicating an unreadable form)
    and will never appear in any Lisp file.

---

Next: [Programming Types](#Programming-Types), Previous: [Special Read Syntax](#Special-Read-Syntax), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.3 Comments [¶](#Comments-1)

A *comment* is text that is written in a program only for the
sake of humans that read the program, and that has no effect on the
meaning of the program. In Lisp, an unescaped semicolon (‘`;`’)
starts a comment if it is not within a string or character constant.
The comment continues to the end of line. The Lisp reader discards
comments; they do not become part of the Lisp objects which represent
the program within the Lisp system.

The ‘`#@count`’ construct, which skips the next count
characters, is useful for program-generated comments containing binary
data. The Emacs Lisp byte compiler uses this in its output files
(see [Byte Compilation](#Byte-Compilation)). It isn’t meant for source files, however.

See [Tips on Writing Comments](#Comment-Tips), for conventions for formatting comments.

---

Next: [Editing Types](#Editing-Types), Previous: [Comments](#Comments), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.4 Programming Types [¶](#Programming-Types-1)

There are two general categories of types in Emacs Lisp: those having
to do with Lisp programming, and those having to do with editing. The
former exist in many Lisp implementations, in one form or another. The
latter are unique to Emacs Lisp.

* [Integer Type](#Integer-Type)
* [Floating-Point Type](#Floating_002dPoint-Type)
* [Character Type](#Character-Type)
* [Symbol Type](#Symbol-Type)
* [Sequence Types](#Sequence-Type)
* [Cons Cell and List Types](#Cons-Cell-Type)
* [Array Type](#Array-Type)
* [String Type](#String-Type)
* [Vector Type](#Vector-Type)
* [Char-Table Type](#Char_002dTable-Type)
* [Bool-Vector Type](#Bool_002dVector-Type)
* [Hash Table Type](#Hash-Table-Type)
* [Function Type](#Function-Type)
* [Macro Type](#Macro-Type)
* [Primitive Function Type](#Primitive-Function-Type)
* [Closure Function Type](#Closure-Type)
* [Record Type](#Record-Type)
* [Type Descriptors](#Type-Descriptors)
* [Type Specifiers](#Type-Specifiers)
* [Autoload Type](#Autoload-Type)
* [Finalizer Type](#Finalizer-Type)

---

Next: [Floating-Point Type](#Floating_002dPoint-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.1 Integer Type [¶](#Integer-Type-1)

Under the hood, there are two kinds of integers—small integers,
called *fixnums*, and large integers, called *bignums*.

The range of values for a fixnum depends on the machine. The
minimum range is −536,870,912 to 536,870,911 (30 bits; i.e.,
−2\*\*29
to
2\*\*29 − 1)
but many machines provide a wider range.

Bignums can have arbitrary precision. Operations that overflow a
fixnum will return a bignum instead.

All numbers can be compared with `eql` or `=`; fixnums can
also be compared with `eq`. To test whether an integer is a fixnum or a
bignum, you can compare it to `most-negative-fixnum` and
`most-positive-fixnum`, or you can use the convenience predicates
`fixnump` and `bignump` on any object.

The read syntax for integers is a sequence of (base ten) digits with an
optional sign at the beginning and an optional period at the end. The
printed representation produced by the Lisp interpreter never has a
leading ‘`+`’ or a final ‘`.`’.

```
-1               ; The integer −1.
1                ; The integer 1.
1.               ; Also the integer 1.
+1               ; Also the integer 1.
```

See [Numbers](#Numbers), for more information.

---

Next: [Character Type](#Character-Type), Previous: [Integer Type](#Integer-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.2 Floating-Point Type [¶](#Floating_002dPoint-Type-1)

Floating-point numbers are the computer equivalent of scientific
notation; you can think of a floating-point number as a fraction
together with a power of ten. The precise number of significant
figures and the range of possible exponents is machine-specific; Emacs
uses the C data type `double` to store the value, and internally
this records a power of 2 rather than a power of 10.

The printed representation for floating-point numbers requires either
a decimal point (with at least one digit following), an exponent, or
both. For example, ‘`1500.0`’, ‘`+15e2`’, ‘`15.0e+2`’,
‘`+1500000e-3`’, and ‘`.15e4`’ are five ways of writing a floating-point
number whose value is 1500. They are all equivalent.

See [Numbers](#Numbers), for more information.

---

Next: [Symbol Type](#Symbol-Type), Previous: [Floating-Point Type](#Floating_002dPoint-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.3 Character Type [¶](#Character-Type-1)

A *character* in Emacs Lisp is nothing more than an integer. In
other words, characters are represented by their character codes. For
example, the character `A` is represented as the integer 65.
That is also their usual printed representation; see [Basic Char Syntax](#Basic-Char-Syntax).

Individual characters are used occasionally in programs, but it is
more common to work with *strings*, which are sequences composed
of characters. See [String Type](#String-Type).

Characters in strings and buffers are currently limited to the range
of 0 to 4194303—twenty two bits (see [Character Codes](#Character-Codes)). Codes 0
through 127 are ASCII codes; the rest are
non-ASCII (see [Non-ASCII Characters](#Non_002dASCII-Characters)). Characters that
represent keyboard input have a much wider range, to encode modifier
keys such as Control, Meta and Shift.

There are special functions for producing a human-readable textual
description of a character for the sake of messages. See [Describing Characters for Help Messages](#Describing-Characters).

* [Basic Char Syntax](#Basic-Char-Syntax)
* [General Escape Syntax](#General-Escape-Syntax)
* [Control-Character Syntax](#Ctl_002dChar-Syntax)
* [Meta-Character Syntax](#Meta_002dChar-Syntax)
* [Other Character Modifier Bits](#Other-Char-Bits)

---

Next: [General Escape Syntax](#General-Escape-Syntax), Up: [Character Type](#Character-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.3.1 Basic Char Syntax [¶](#Basic-Char-Syntax-1)

Since characters are really integers, the printed representation of
a character is a decimal number. This is also a possible read syntax
for a character, but writing characters that way in Lisp programs is
not clear programming. You should *always* use the special read
syntax formats that Emacs Lisp provides for characters. These syntax
formats start with a question mark.

The usual read syntax for alphanumeric characters is a question mark
followed by the character; thus, ‘`?A`’ for the character
`A`, ‘`?B`’ for the character `B`, and ‘`?a`’ for the
character `a`.

For example:

```
?Q ⇒ 81     ?q ⇒ 113
```

You can use the same syntax for punctuation characters. However, if
the punctuation character has a special syntactic meaning in Lisp, you
must quote it with a ‘`\`’. For example, ‘`?\(`’ is the way to
write the open-paren character. Likewise, if the character is
‘`\`’, you must use a second ‘`\`’ to quote it: ‘`?\\`’.

You can express the characters control-g, backspace, tab, newline,
vertical tab, formfeed, space, return, del, and escape as ‘`?\a`’,
‘`?\b`’, ‘`?\t`’, ‘`?\n`’, ‘`?\v`’, ‘`?\f`’,
‘`?\s`’, ‘`?\r`’, ‘`?\d`’, and ‘`?\e`’, respectively.
(‘`?\s`’ followed by a dash has a different meaning—it applies
the Super modifier to the following character.) Thus,

```
?\a ⇒ 7                 ; control-g, C-g
?\b ⇒ 8                 ; backspace, BS, C-h
?\t ⇒ 9                 ; tab, TAB, C-i
?\n ⇒ 10                ; newline, C-j
?\v ⇒ 11                ; vertical tab, C-k
?\f ⇒ 12                ; formfeed character, C-l
?\r ⇒ 13                ; carriage return, RET, C-m
?\e ⇒ 27                ; escape character, ESC, C-[
?\s ⇒ 32                ; space character, SPC
?\\ ⇒ 92                ; backslash character, \
?\d ⇒ 127               ; delete character, DEL
```

These sequences which start with backslash are also known as
*escape sequences*, because backslash plays the role of an
escape character; this has nothing to do with the
character `ESC`. ‘`\s`’ is meant for use in character
constants; in string constants, just write the space.

A backslash is allowed, and harmless, preceding any character
without a special escape meaning; thus, ‘`?\+`’ is equivalent to
‘`?+`’. There is no reason to add a backslash before most
characters. However, you must add a backslash before any of the
characters ‘`()[]\;"`’, and you should add a backslash before any
of the characters ‘`` |'`#., ``’ to avoid confusing the Emacs commands
for editing Lisp code. You should also add a backslash before Unicode
characters which resemble the previously mentioned ASCII
ones, to avoid confusing people reading your code. Emacs will
highlight some non-escaped commonly confused characters such as
‘`‘`’ to encourage this. You can also add a backslash before whitespace
characters such as space and tab. However, it is
cleaner to use one of the easily readable escape sequences, such as
‘`\t`’ or ‘`\s`’, instead of an actual whitespace character such
as a tab or a space. (If you do write backslash followed by a space,
you should write an extra space after the character constant to
separate it from the following text.)

---

Next: [Control-Character Syntax](#Ctl_002dChar-Syntax), Previous: [Basic Char Syntax](#Basic-Char-Syntax), Up: [Character Type](#Character-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.3.2 General Escape Syntax [¶](#General-Escape-Syntax-1)

In addition to the specific escape sequences for special important
control characters, Emacs provides several types of escape syntax that
you can use to specify non-ASCII text characters.

1. You can specify characters by their Unicode names, if any.
   `?\N{NAME}` represents the Unicode character named
   NAME. Thus, ‘`?\N{LATIN SMALL LETTER A WITH GRAVE}`’ is
   equivalent to `?à` and denotes the Unicode character U+00E0. To
   simplify entering multi-line strings, you can replace spaces in the
   names by non-empty sequences of whitespace (e.g., newlines).
2. You can specify characters by their Unicode values.
   `?\N{U+X}` represents a character with Unicode code point
   X, where X is a hexadecimal number. Also,
   `?\uxxxx` and `?\Uxxxxxxxx` represent code
   points xxxx and xxxxxxxx, respectively, where each x
   is a single hexadecimal digit. For example, `?\N{U+E0}`,
   `?\u00e0` and `?\U000000E0` are all equivalent to
   `?à` and to ‘`?\N{LATIN SMALL LETTER A WITH GRAVE}`’. The
   Unicode Standard defines code points only up to ‘`U+10ffff`’,
   so if you specify a code point higher than that, Emacs signals an
   error.
3. You can specify characters by their hexadecimal character
   codes. A hexadecimal escape sequence consists of a backslash,
   ‘`x`’, and the hexadecimal character code. Thus, ‘`?\x41`’ is
   the character `A`, ‘`?\x1`’ is the character `C-a`, and
   `?\xe0` is the character `à` (`a` with grave accent).
   You can use one or more hex digits after ‘`x`’, so you can
   represent any character code in this way.
4. You can specify characters by their character code in
   octal. An octal escape sequence consists of a backslash followed by
   up to three octal digits; thus, ‘`?\101`’ for the character
   `A`, ‘`?\001`’ for the character `C-a`, and `?\002`
   for the character `C-b`. Only characters up to octal code 777 can
   be specified this way.

These escape sequences may also be used in strings. See [Non-ASCII Characters in Strings](#Non_002dASCII-in-Strings).

---

Next: [Meta-Character Syntax](#Meta_002dChar-Syntax), Previous: [General Escape Syntax](#General-Escape-Syntax), Up: [Character Type](#Character-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.3.3 Control-Character Syntax [¶](#Control_002dCharacter-Syntax)

Control characters can be represented using yet another read syntax.
This consists of a question mark followed by a backslash, caret, and the
corresponding non-control character, in either upper or lower case. For
example, both ‘`?\^I`’ and ‘`?\^i`’ are valid read syntax for the
character `C-i`, the character whose value is 9.

Instead of the ‘`^`’, you can use ‘`C-`’; thus, ‘`?\C-i`’ is
equivalent to ‘`?\^I`’ and to ‘`?\^i`’:

```
?\^I ⇒ 9     ?\C-I ⇒ 9
```

In strings and buffers, the only control characters allowed are those
that exist in ASCII; but for keyboard input purposes, you can turn
any character into a control character with ‘`C-`’. The character
codes for these non-ASCII control characters include the
2\*\*26
bit as well as the code for the corresponding non-control character.
Not all text terminals can generate non-ASCII control
characters, but it is straightforward to generate them using X and
other window systems.

For historical reasons, Emacs treats the `DEL` character as
the control equivalent of `?`:

```
?\^? ⇒ 127     ?\C-? ⇒ 127
```

As a result, it is currently not possible to represent the character
`Control-?`, which is a meaningful input character under X, using
‘`\C-`’. It is not easy to change this, as various Lisp files refer
to `DEL` in this way.

For representing control characters to be found in files or strings,
we recommend the ‘`^`’ syntax; for control characters in keyboard
input, we prefer the ‘`C-`’ syntax. Which one you use does not
affect the meaning of the program, but may guide the understanding of
people who read it.

---

Next: [Other Character Modifier Bits](#Other-Char-Bits), Previous: [Control-Character Syntax](#Ctl_002dChar-Syntax), Up: [Character Type](#Character-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.3.4 Meta-Character Syntax [¶](#Meta_002dCharacter-Syntax)

A *meta character* is a character typed with the `META`
modifier key. The integer that represents such a character has the
2\*\*27
bit set. We use high bits for this and other modifiers to make
possible a wide range of basic character codes.

In a string, the
2\*\*7
bit attached to an ASCII character indicates a meta
character; thus, the meta characters that can fit in a string have
codes in the range from 128 to 255, and are the meta versions of the
ordinary ASCII characters. See [Putting Keyboard Events in Strings](#Strings-of-Events), for
details about `META`-handling in strings.

The read syntax for meta characters uses ‘`\M-`’. For example,
‘`?\M-A`’ stands for `M-A`. You can use ‘`\M-`’ together with
octal character codes (see below), with ‘`\C-`’, or with any other
syntax for a character. Thus, you can write `M-A` as ‘`?\M-A`’,
or as ‘`?\M-\101`’. Likewise, you can write `C-M-b` as
‘`?\M-\C-b`’, ‘`?\C-\M-b`’, or ‘`?\M-\002`’.

---

Previous: [Meta-Character Syntax](#Meta_002dChar-Syntax), Up: [Character Type](#Character-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.3.5 Other Character Modifier Bits [¶](#Other-Character-Modifier-Bits)

The case of a graphic character is indicated by its character code;
for example, ASCII distinguishes between the characters ‘`a`’
and ‘`A`’. But ASCII has no way to represent whether a control
character is upper case or lower case. Emacs uses the
2\*\*25
bit to indicate that the shift key was used in typing a control
character. This distinction is possible only on a graphical display
such as a GUI display on X; text terminals do not report the
distinction. The Lisp syntax for the shift bit is ‘`\S-`’; thus,
‘`?\C-\S-o`’ or ‘`?\C-\S-O`’ represents the shifted-control-o
character.

The X Window System defines three other
modifier bits that can be set
in a character: *hyper*, *super* and *alt*. The syntaxes
for these bits are ‘`\H-`’, ‘`\s-`’ and ‘`\A-`’. (Case is
significant in these prefixes.) Thus, ‘`?\H-\M-\A-x`’ represents
`Alt-Hyper-Meta-x`. (Note that ‘`\s`’ with no following ‘`-`’
represents the space character.)
Numerically, the
bit values are 2\*\*22 for alt, 2\*\*23 for super and 2\*\*24 for hyper.

---

Next: [Sequence Types](#Sequence-Type), Previous: [Character Type](#Character-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.4 Symbol Type [¶](#Symbol-Type-1)

A *symbol* in GNU Emacs Lisp is an object with a name. The
symbol name serves as the printed representation of the symbol. In
ordinary Lisp use, with one single obarray (see [Creating and Interning Symbols](#Creating-Symbols)),
a symbol’s name is unique—no two symbols have the same name.

A symbol can serve as a variable, as a function name, or to hold a
property list. Or it may serve only to be distinct from all other Lisp
objects, so that its presence in a data structure may be recognized
reliably. In a given context, usually only one of these uses is
intended. But you can use one symbol in all of these ways,
independently.

A symbol whose name starts with a colon (‘`:`’) is called a
*keyword symbol*. These symbols automatically act as constants,
and are normally used only by comparing an unknown symbol with a few
specific alternatives. See [Variables that Never Change](#Constant-Variables).

A symbol name can contain any characters whatever. Most symbol names
are written with letters, digits, and the punctuation characters
‘`-+=*/`’. Such names require no special punctuation; the characters
of the name suffice as long as the name does not look like a number.
(If it does, write a ‘`\`’ at the beginning of the name to force
interpretation as a symbol.) The characters ‘`_~!@$%^&:<>{}?`’ are
less often used but also require no special punctuation. Any other
characters may be included in a symbol’s name by escaping them with a
backslash. In contrast to its use in strings, however, a backslash in
the name of a symbol simply quotes the single character that follows the
backslash. For example, in a string, ‘`\t`’ represents a tab
character; in the name of a symbol, however, ‘`\t`’ merely quotes the
letter ‘`t`’. To have a symbol with a tab character in its name, you
must actually use a tab (preceded with a backslash). But it’s rare to
do such a thing.

> **Common Lisp note:** In Common Lisp, lower case letters are always
> folded to upper case, unless they are explicitly escaped. In Emacs
> Lisp, upper case and lower case letters are distinct.

Here are several examples of symbol names. Note that the ‘`+`’ in
the fourth example is escaped to prevent it from being read as a number.
This is not necessary in the sixth example because the rest of the name
makes it invalid as a number.

```
foo                 ; A symbol named ‘foo’.
FOO                 ; A symbol named ‘FOO’, different from ‘foo’.
```

```
1+                  ; A symbol named ‘1+’
                    ;   (not ‘+1’, which is an integer).
```

```
\+1                 ; A symbol named ‘+1’
                    ;   (not a very readable name).
```

```
\(*\ 1\ 2\)         ; A symbol named ‘(* 1 2)’ (a worse name).
+-*/_~!@$%^&=:<>{}  ; A symbol named ‘+-*/_~!@$%^&=:<>{}’.
                    ;   These characters need not be escaped.
```

As an exception to the rule that a symbol’s name serves as its
printed representation, ‘`##`’ is the printed representation for an
interned symbol whose name is an empty string. Furthermore,
‘`#:foo`’ is the printed representation for an uninterned
symbol whose name is foo. (Normally, the Lisp reader interns
all symbols; see [Creating and Interning Symbols](#Creating-Symbols).)

---

Next: [Cons Cell and List Types](#Cons-Cell-Type), Previous: [Symbol Type](#Symbol-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.5 Sequence Types [¶](#Sequence-Types)

A *sequence* is a Lisp object that represents an ordered set of
elements. There are two kinds of sequence in Emacs Lisp: *lists*
and *arrays*.

Lists are the most commonly-used sequences. A list can hold
elements of any type, and its length can be easily changed by adding
or removing elements. See the next subsection for more about lists.

Arrays are fixed-length sequences. They are further subdivided into
strings, vectors, char-tables and bool-vectors. Vectors can hold
elements of any type, whereas string elements must be characters, and
bool-vector elements must be `t` or `nil`. Char-tables are
like vectors except that they are indexed by any valid character code.
The characters in a string can have text properties like characters in
a buffer (see [Text Properties](#Text-Properties)), but vectors do not support text
properties, even when their elements happen to be characters.

Lists, strings and the other array types also share important
similarities. For example, all have a length l, and all have
elements which can be indexed from zero to l minus one. Several
functions, called sequence functions, accept any kind of sequence.
For example, the function `length` reports the length of any kind
of sequence. See [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors).

It is generally impossible to read the same sequence twice, since
sequences are always created anew upon reading. If you read the read
syntax for a sequence twice, you get two sequences with equal contents.
There is one exception: the empty list `()` always stands for the
same object, `nil`.

---

Next: [Array Type](#Array-Type), Previous: [Sequence Types](#Sequence-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.6 Cons Cell and List Types [¶](#Cons-Cell-and-List-Types)

A *cons cell* is an object that consists of two slots, called
the CAR slot and the CDR slot. Each slot can *hold* any
Lisp object. We also say that the CAR of this cons cell is
whatever object its CAR slot currently holds, and likewise for
the CDR.

A *list* is a series of cons cells, linked together so that the
CDR slot of each cons cell holds either the next cons cell or the
empty list. The empty list is actually the symbol `nil`.
See [Lists](#Lists), for details. Because most cons cells are used as part
of lists, we refer to any structure made out of cons cells as a
*list structure*.

> A note to C programmers: a Lisp list thus works as a *linked list*
> built up of cons cells. Because pointers in Lisp are implicit, we do
> not distinguish between a cons cell slot holding a value versus
> pointing to the value.

Because cons cells are so central to Lisp, we also have a word for
an object which is not a cons cell. These objects are called
*atoms*.

The read syntax and printed representation for lists are identical, and
consist of a left parenthesis, an arbitrary number of elements, and a
right parenthesis. Here are examples of lists:

```
(A 2 "A")            ; A list of three elements.
()                   ; A list of no elements (the empty list).
nil                  ; A list of no elements (the empty list).
("A ()")             ; A list of one element: the string "A ()".
(A ())               ; A list of two elements: A and the empty list.
(A nil)              ; Equivalent to the previous.
((A B C))            ; A list of one element
                     ;   (which is a list of three elements).
```

Upon reading, each object inside the parentheses becomes an element
of the list. That is, a cons cell is made for each element. The
CAR slot of the cons cell holds the element, and its CDR
slot refers to the next cons cell of the list, which holds the next
element in the list. The CDR slot of the last cons cell is set to
hold `nil`.

The names CAR and CDR derive from the history of Lisp. The
original Lisp implementation ran on an IBM 704 computer which
divided words into two parts, the address and the
decrement; CAR was an instruction to extract the contents of
the address part of a register, and CDR an instruction to extract
the contents of the decrement. By contrast, cons cells are named
for the function `cons` that creates them, which in turn was named
for its purpose, the construction of cells.

* [Drawing Lists as Box Diagrams](#Box-Diagrams)
* [Dotted Pair Notation](#Dotted-Pair-Notation)
* [Association List Type](#Association-List-Type)

---

Next: [Dotted Pair Notation](#Dotted-Pair-Notation), Up: [Cons Cell and List Types](#Cons-Cell-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.6.1 Drawing Lists as Box Diagrams [¶](#Drawing-Lists-as-Box-Diagrams)

A list can be illustrated by a diagram in which the cons cells are
shown as pairs of boxes, like dominoes. (The Lisp reader cannot read
such an illustration; unlike the textual notation, which can be
understood by both humans and computers, the box illustrations can be
understood only by humans.) This picture represents the three-element
list `(rose violet buttercup)`:

```
    --- ---      --- ---      --- ---
   |   |   |--> |   |   |--> |   |   |--> nil
    --- ---      --- ---      --- ---
     |            |            |
     |            |            |
      --> rose     --> violet   --> buttercup
```

In this diagram, each box represents a slot that can hold or refer to
any Lisp object. Each pair of boxes represents a cons cell. Each arrow
represents a reference to a Lisp object, either an atom or another cons
cell.

In this example, the first box, which holds the CAR of the first
cons cell, refers to or holds `rose` (a symbol). The second
box, holding the CDR of the first cons cell, refers to the next
pair of boxes, the second cons cell. The CAR of the second cons
cell is `violet`, and its CDR is the third cons cell. The
CDR of the third (and last) cons cell is `nil`.

Here is another diagram of the same list, `(rose violet
buttercup)`, sketched in a different manner:

```
 ---------------       ----------------       -------------------
| car   | cdr   |     | car    | cdr   |     | car       | cdr   |
| rose  |   o-------->| violet |   o-------->| buttercup |  nil  |
|       |       |     |        |       |     |           |       |
 ---------------       ----------------       -------------------
```

A list with no elements in it is the *empty list*; it is identical
to the symbol `nil`. In other words, `nil` is both a symbol
and a list.

Here is the list `(A ())`, or equivalently `(A nil)`,
depicted with boxes and arrows:

```
    --- ---      --- ---
   |   |   |--> |   |   |--> nil
    --- ---      --- ---
     |            |
     |            |
      --> A        --> nil
```

Here is a more complex illustration, showing the three-element list,
`((pine needles) oak maple)`, the first element of which is a
two-element list:

```
    --- ---      --- ---      --- ---
   |   |   |--> |   |   |--> |   |   |--> nil
    --- ---      --- ---      --- ---
     |            |            |
     |            |            |
     |             --> oak      --> maple
     |
     |     --- ---      --- ---
      --> |   |   |--> |   |   |--> nil
           --- ---      --- ---
            |            |
            |            |
             --> pine     --> needles
```

The same list represented in the second box notation looks like this:

```
 --------------       --------------       --------------
| car   | cdr  |     | car   | cdr  |     | car   | cdr  |
|   o   |   o------->| oak   |   o------->| maple |  nil |
|   |   |      |     |       |      |     |       |      |
 -- | ---------       --------------       --------------
    |
    |
    |        --------------       ----------------
    |       | car   | cdr  |     | car     | cdr  |
     ------>| pine  |   o------->| needles |  nil |
            |       |      |     |         |      |
             --------------       ----------------
```

---

Next: [Association List Type](#Association-List-Type), Previous: [Drawing Lists as Box Diagrams](#Box-Diagrams), Up: [Cons Cell and List Types](#Cons-Cell-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.6.2 Dotted Pair Notation [¶](#Dotted-Pair-Notation-1)

*Dotted pair notation* is a general syntax for cons cells that
represents the CAR and CDR explicitly. In this syntax,
`(a . b)` stands for a cons cell whose CAR is
the object a and whose CDR is the object b. Dotted
pair notation is more general than list syntax because the CDR
does not have to be a list. However, it is more cumbersome in cases
where list syntax would work. In dotted pair notation, the list
‘`(1 2 3)`’ is written as ‘`(1 . (2 . (3 . nil)))`’. For
`nil`-terminated lists, you can use either notation, but list
notation is usually clearer and more convenient. When printing a
list, the dotted pair notation is only used if the CDR of a cons
cell is not a list.

Here’s an example using boxes to illustrate dotted pair notation.
This example shows the pair `(rose . violet)`:

```
    --- ---
   |   |   |--> violet
    --- ---
     |
     |
      --> rose
```

You can combine dotted pair notation with list notation to represent
conveniently a chain of cons cells with a non-`nil` final CDR.
You write a dot after the last element of the list, followed by the
CDR of the final cons cell. For example, `(rose violet
. buttercup)` is equivalent to `(rose . (violet . buttercup))`.
The object looks like this:

```
    --- ---      --- ---
   |   |   |--> |   |   |--> buttercup
    --- ---      --- ---
     |            |
     |            |
      --> rose     --> violet
```

The syntax `(rose . violet . buttercup)` is invalid because
there is nothing that it could mean. If anything, it would say to put
`buttercup` in the CDR of a cons cell whose CDR is already
used for `violet`.

The list `(rose violet)` is equivalent to `(rose . (violet))`,
and looks like this:

```
    --- ---      --- ---
   |   |   |--> |   |   |--> nil
    --- ---      --- ---
     |            |
     |            |
      --> rose     --> violet
```

Similarly, the three-element list `(rose violet buttercup)`
is equivalent to `(rose . (violet . (buttercup)))`.
It looks like this:

```
    --- ---      --- ---      --- ---
   |   |   |--> |   |   |--> |   |   |--> nil
    --- ---      --- ---      --- ---
     |            |            |
     |            |            |
      --> rose     --> violet   --> buttercup
```

---

Previous: [Dotted Pair Notation](#Dotted-Pair-Notation), Up: [Cons Cell and List Types](#Cons-Cell-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.6.3 Association List Type [¶](#Association-List-Type-1)

An *association list* or *alist* is a specially-constructed
list whose elements are cons cells. In each element, the CAR is
considered a *key*, and the CDR is considered an
*associated value*. (In some cases, the associated value is stored
in the CAR of the CDR.) Association lists are often used as
stacks, since it is easy to add or remove associations at the front of
the list.

For example,

```
(setq alist-of-colors
      '((rose . red) (lily . white) (buttercup . yellow)))
```

sets the variable `alist-of-colors` to an alist of three elements. In the
first element, `rose` is the key and `red` is the value.

See [Association Lists](#Association-Lists), for a further explanation of alists and for
functions that work on alists. See [Hash Tables](#Hash-Tables), for another kind of
lookup table, which is much faster for handling a large number of keys.

---

Next: [String Type](#String-Type), Previous: [Cons Cell and List Types](#Cons-Cell-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.7 Array Type [¶](#Array-Type-1)

An *array* is composed of an arbitrary number of slots for
holding or referring to other Lisp objects, arranged in a contiguous block of
memory. Accessing any element of an array takes approximately the same
amount of time. In contrast, accessing an element of a list requires
time proportional to the position of the element in the list. (Elements
at the end of a list take longer to access than elements at the
beginning of a list.)

Emacs defines four types of array: strings, vectors, bool-vectors, and
char-tables.

A string is an array of characters and a vector is an array of
arbitrary objects. A bool-vector can hold only `t` or `nil`.
These kinds of array may have any length up to the largest fixnum,
subject to system architecture limits and available memory.
Char-tables are sparse arrays indexed by any valid character code; they
can hold arbitrary objects.

The first element of an array has index zero, the second element has
index 1, and so on. This is called *zero-origin* indexing. For
example, an array of four elements has indices 0, 1, 2, and 3. The
largest possible index value is one less than the length of the array.
Once an array is created, its length is fixed.

All Emacs Lisp arrays are one-dimensional. (Most other programming
languages support multidimensional arrays, but they are not essential;
you can get the same effect with nested one-dimensional arrays.) Each
type of array has its own read syntax; see the following sections for
details.

The array type is a subset of the sequence type, and contains the
string type, the vector type, the bool-vector type, and the char-table
type.

---

Next: [Vector Type](#Vector-Type), Previous: [Array Type](#Array-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.8 String Type [¶](#String-Type-1)

A *string* is an array of characters. Strings are used for many
purposes in Emacs, as can be expected in a text editor; for example, as
the names of Lisp symbols, as messages for the user, and to represent
text extracted from buffers. Strings in Lisp are constants: evaluation
of a string returns the same string.

See [Strings and Characters](#Strings-and-Characters), for functions that operate on strings.

* [Syntax for Strings](#Syntax-for-Strings)
* [Non-ASCII Characters in Strings](#Non_002dASCII-in-Strings)
* [Nonprinting Characters in Strings](#Nonprinting-Characters)
* [Text Properties in Strings](#Text-Props-and-Strings)

---

Next: [Non-ASCII Characters in Strings](#Non_002dASCII-in-Strings), Up: [String Type](#String-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.8.1 Syntax for Strings [¶](#Syntax-for-Strings-1)

The read syntax for a string is a double-quote, an arbitrary number
of characters, and another double-quote, `"like this"`. To
include a double-quote in a string, precede it with a backslash; thus,
`"\""` is a string containing just one double-quote
character. Likewise, you can include a backslash by preceding it with
another backslash, like this: `"this \\ is a single embedded
backslash"`.

Since a string is an array of characters, you can specify the string
characters using the read syntax of characters, but without the
leading question mark. This is useful for including in string
constants characters that don’t stand for themselves. Thus, control
characters can be specified as escape sequences that start with a
backslash; for example, `"foo\r"` yields ‘`foo`’ followed by
the carriage return character. See [Basic Char Syntax](#Basic-Char-Syntax), for escape
sequences of other control characters. Similarly, you can use the
special read syntax for control characters (see [Control-Character Syntax](#Ctl_002dChar-Syntax)),
as in `"foo\^Ibar"`, which produces a tab character embedded
within a string. You can also use the escape sequences for non-ASCII
characters described in [General Escape Syntax](#General-Escape-Syntax), as in
`"\N{LATIN SMALL LETTER A WITH GRAVE}"` and `"\u00e0"`
(however, see a caveat with non-ASCII characters in [Non-ASCII Characters in Strings](#Non_002dASCII-in-Strings)).

The newline character is not special in the read syntax for strings;
if you write a new line between the double-quotes, it becomes a
character in the string. But an escaped newline—one that is preceded
by ‘`\`’—does not become part of the string; i.e., the Lisp reader
ignores an escaped newline while reading a string. An escaped space
‘`\`’ is likewise ignored.

```
"It is useful to include newlines
in documentation strings,
but the newline is \
ignored if escaped."
     ⇒ "It is useful to include newlines
in documentation strings,
but the newline is ignored if escaped."
```

---

Next: [Nonprinting Characters in Strings](#Nonprinting-Characters), Previous: [Syntax for Strings](#Syntax-for-Strings), Up: [String Type](#String-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.8.2 Non-ASCII Characters in Strings [¶](#Non_002dASCII-Characters-in-Strings)

There are two text representations for non-ASCII
characters in Emacs strings: multibyte and unibyte (see [Text Representations](#Text-Representations)). Roughly speaking, unibyte strings store raw bytes,
while multibyte strings store human-readable text. Each character in
a unibyte string is a byte, i.e., its value is between 0 and 255. By
contrast, each character in a multibyte string may have a value
between 0 to 4194303 (see [Character Type](#Character-Type)). In both cases,
characters above 127 are non-ASCII.

You can include a non-ASCII character in a string constant
by writing it literally. If the string constant is read from a
multibyte source, such as a multibyte buffer or string, or a file that
would be visited as multibyte, then Emacs reads each
non-ASCII character as a multibyte character and
automatically makes the string a multibyte string. If the string
constant is read from a unibyte source, then Emacs reads the
non-ASCII character as unibyte, and makes the string
unibyte.

Instead of writing a character literally into a multibyte string,
you can write it as its character code using an escape sequence.
See [General Escape Syntax](#General-Escape-Syntax), for details about escape sequences.

If you use any Unicode-style escape sequence ‘`\uNNNN`’ or
‘`\U00NNNNNN`’ in a string constant (even for an ASCII
character), Emacs automatically assumes that it is multibyte.

You can also use hexadecimal escape sequences (‘`\xn`’) and
octal escape sequences (‘`\n`’) in string constants.
**But beware:** If a string constant contains octal escape
sequences or one- or two-digit hexadecimal escape sequences, and these
escape sequences all specify unibyte characters (i.e., codepoints less
than 256), and there are no other literal non-ASCII
characters or Unicode-style escape sequences in the string, then Emacs
automatically assumes that it is a unibyte string. That is to say, it
assumes that all non-ASCII characters occurring in the
string are 8-bit raw bytes.

In hexadecimal and octal escape sequences, the escaped character
code may contain a variable number of digits, so the first subsequent
character which is not a valid hexadecimal or octal digit terminates
the escape sequence. If the next character in a string could be
interpreted as a hexadecimal or octal digit, write ‘`\`’
(backslash and space) to terminate the escape sequence. For example,
‘`\xe0\`’ represents one character, ‘`a`’ with grave
accent. ‘`\`’ in a string constant is just like
backslash-newline; it does not contribute any character to the string,
but it does terminate any preceding hex escape.

---

Next: [Text Properties in Strings](#Text-Props-and-Strings), Previous: [Non-ASCII Characters in Strings](#Non_002dASCII-in-Strings), Up: [String Type](#String-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.8.3 Nonprinting Characters in Strings [¶](#Nonprinting-Characters-in-Strings)

You can use the same backslash escape-sequences in a string constant
as in character literals (but do not use the question mark that begins a
character constant). For example, you can write a string containing the
nonprinting characters tab and `C-a`, with commas and spaces between
them, like this: `"\t, \C-a"`. See [Character Type](#Character-Type), and its
subsections for a description of the various kinds of read syntax for
characters.

However, not all of the characters you can write with backslash
escape-sequences are valid in strings. The only control characters that
a string can hold are the ASCII control characters. Strings do not
distinguish case in ASCII control characters.

Properly speaking, strings cannot hold meta characters; but when a
string is to be used as a key sequence, there is a special convention
that provides a way to represent meta versions of ASCII
characters in a string. If you use the ‘`\M-`’ syntax to indicate
a meta character in a string constant, this sets the
2\*\*7
bit of the character in the string. If the string is used in
`define-key` or `lookup-key`, this numeric code is translated
into the equivalent meta character. See [Character Type](#Character-Type).

Strings cannot hold characters that have the hyper, super, or alt
modifiers.

---

Previous: [Nonprinting Characters in Strings](#Nonprinting-Characters), Up: [String Type](#String-Type)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.8.4 Text Properties in Strings [¶](#Text-Properties-in-Strings)

A string can hold properties for the characters it contains, in
addition to the characters themselves. This enables programs that copy
text between strings and buffers to copy the text’s properties with no
special effort. See [Text Properties](#Text-Properties), for an explanation of what text
properties mean. Strings with text properties use a special read and
print syntax:

```
#("characters" property-data...)
```

where property-data consists of zero or more elements, in groups
of three as follows:

```
beg end plist
```

The elements beg and end are integers, and together specify
a range of indices in the string; plist is the property list for
that range. For example,

```
#("foo bar" 0 3 (face bold) 3 4 nil 4 7 (face italic))
```

represents a string whose textual contents are ‘`foo bar`’, in which
the first three characters have a `face` property with value
`bold`, and the last three have a `face` property with value
`italic`. (The fourth character has no text properties, so its
property list is `nil`. It is not actually necessary to mention
ranges with `nil` as the property list, since any characters not
mentioned in any range will default to having no properties.)

---

Next: [Char-Table Type](#Char_002dTable-Type), Previous: [String Type](#String-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.9 Vector Type [¶](#Vector-Type-1)

A *vector* is a one-dimensional array of elements of any type. It
takes a constant amount of time to access any element of a vector. (In
a list, the access time of an element is proportional to the distance of
the element from the beginning of the list.)

The printed representation of a vector consists of a left square
bracket, the elements, and a right square bracket. This is also the
read syntax. Like numbers and strings, vectors are considered constants
for evaluation.

```
[1 "two" (three)]      ; A vector of three elements.
     ⇒ [1 "two" (three)]
```

See [Vectors](#Vectors), for functions that work with vectors.

---

Next: [Bool-Vector Type](#Bool_002dVector-Type), Previous: [Vector Type](#Vector-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.10 Char-Table Type [¶](#Char_002dTable-Type-1)

A *char-table* is a one-dimensional array of elements of any type,
indexed by character codes. Char-tables have certain extra features to
make them more useful for many jobs that involve assigning information
to character codes—for example, a char-table can have a parent to
inherit from, a default value, and a small number of extra slots to use for
special purposes. A char-table can also specify a single value for
a whole character set.

The printed representation of a char-table is like a vector
except that there is an extra ‘`#^`’ at the beginning.[1](#FOOT1)

See [Char-Tables](#Char_002dTables), for special functions to operate on char-tables.
Uses of char-tables include:

* Case tables (see [The Case Table](#Case-Tables)).
* Character category tables (see [Categories](#Categories)).
* Display tables (see [Display Tables](#Display-Tables)).
* Syntax tables (see [Syntax Tables](#Syntax-Tables)).

---

Next: [Hash Table Type](#Hash-Table-Type), Previous: [Char-Table Type](#Char_002dTable-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.11 Bool-Vector Type [¶](#Bool_002dVector-Type-1)

A *bool-vector* is a one-dimensional array whose elements must
be `t` or `nil`.

The printed representation of a bool-vector is like a string, except
that it begins with ‘`#&`’ followed by the length. The string
constant that follows actually specifies the contents of the bool-vector
as a bitmap—each character in the string contains 8 bits, which
specify the next 8 elements of the bool-vector (1 stands for `t`,
and 0 for `nil`). The least significant bits of the character
correspond to the lowest indices in the bool-vector.

```
(make-bool-vector 3 t)
     ⇒ #&3"^G"
(make-bool-vector 3 nil)
     ⇒ #&3"^@"
```

These results make sense, because the binary code for ‘`C-g`’ is
111 and ‘`C-@`’ is the character with code 0.

If the length is not a multiple of 8, the printed representation
shows extra elements, but these extras really make no difference. For
instance, in the next example, the two bool-vectors are equal, because
only the first 3 bits are used:

```
(equal #&3"\377" #&3"\007")
     ⇒ t
```

---

Next: [Function Type](#Function-Type), Previous: [Bool-Vector Type](#Bool_002dVector-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.12 Hash Table Type [¶](#Hash-Table-Type-1)

A hash table is a very fast kind of lookup table, somewhat like an
alist in that it maps keys to corresponding values, but much faster.
The printed representation of a hash table specifies its properties
and contents, like this:

```
(make-hash-table)
     ⇒ #s(hash-table)
```

See [Hash Tables](#Hash-Tables), for more information about hash tables.

---

Next: [Macro Type](#Macro-Type), Previous: [Hash Table Type](#Hash-Table-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.13 Function Type [¶](#Function-Type-1)

Lisp functions are executable code, just like functions in other
programming languages. In Lisp, unlike most languages, functions are
also Lisp objects. A non-compiled function in Lisp is a lambda
expression: that is, a list whose first element is the symbol
`lambda` (see [Lambda Expressions](#Lambda-Expressions)).

In most programming languages, it is impossible to have a function
without a name. In Lisp, a function has no intrinsic name. A lambda
expression can be called as a function even though it has no name; to
emphasize this, we also call it an *anonymous function*
(see [Anonymous Functions](#Anonymous-Functions)). A named function in Lisp is just a
symbol with a valid function in its function cell (see [Defining Functions](#Defining-Functions)).

Most of the time, functions are called when their names are written in
Lisp expressions in Lisp programs. However, you can construct or obtain
a function object at run time and then call it with the primitive
functions `funcall` and `apply`. See [Calling Functions](#Calling-Functions).

---

Next: [Primitive Function Type](#Primitive-Function-Type), Previous: [Function Type](#Function-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.14 Macro Type [¶](#Macro-Type-1)

A *Lisp macro* is a user-defined construct that extends the Lisp
language. It is represented as an object much like a function, but with
different argument-passing semantics. A Lisp macro has the form of a
list whose first element is the symbol `macro` and whose CDR
is a Lisp function object, including the `lambda` symbol.

Lisp macro objects are usually defined with the built-in
`defmacro` macro, but any list that begins with `macro` is a
macro as far as Emacs is concerned. See [Macros](#Macros), for an explanation
of how to write a macro.

**Warning**: Lisp macros and keyboard macros (see [Keyboard Macros](#Keyboard-Macros)) are entirely different things. When we use the word “macro”
without qualification, we mean a Lisp macro, not a keyboard macro.

---

Next: [Closure Function Type](#Closure-Type), Previous: [Macro Type](#Macro-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.15 Primitive Function Type [¶](#Primitive-Function-Type-1)

A *primitive function* is a function callable from Lisp but
written in the C programming language. Primitive functions are also
called *subrs* or *built-in functions*. (The word “subr” is
derived from “subroutine”.) Most primitive functions evaluate all
their arguments when they are called. A primitive function that does
not evaluate all its arguments is called a *special form*
(see [Special Forms](#Special-Forms)).

It does not matter to the caller of a function whether the function is
primitive. However, this does matter if you try to redefine a primitive
with a function written in Lisp. The reason is that the primitive
function may be called directly from C code. Calls to the redefined
function from Lisp will use the new definition, but calls from C code
may still use the built-in definition. Therefore, **we discourage
redefinition of primitive functions**.

The term *function* refers to all Emacs functions, whether written
in Lisp or C. See [Function Type](#Function-Type), for information about the
functions written in Lisp.

Primitive functions have no read syntax and print in hash notation
with the name of the subroutine.

```
(symbol-function 'car)          ; Access the function cell
                                ;   of the symbol.
     ⇒ #<subr car>
(subrp (symbol-function 'car))  ; Is this a primitive function?
     ⇒ t                       ; Yes.
```

---

Next: [Record Type](#Record-Type), Previous: [Primitive Function Type](#Primitive-Function-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.16 Closure Function Type [¶](#Closure-Function-Type)

*Closures* are function objects produced when turning a function
definition into a function value. Closures are used both for
byte-compiled Lisp functions as well as for interpreted Lisp functions.
Closures can be produced by byte-compiling Lisp code (see [Byte Compilation](#Byte-Compilation)) or simply by evaluating a lambda expression without
compiling it, resulting in an interpreted function. Internally,
a closure is much like a vector; however, the evaluator
handles this data type specially when it appears in a function call.
See [Closure Function Objects](#Closure-Objects).

The printed representation and read syntax for a byte-code function
object is like that for a vector, with an additional ‘`#`’ before the
opening ‘`[`’. When printed for human consumption, it is printed as
a special kind of list with an additional ‘`#f`’ before the opening
‘`(`’.

---

Next: [Type Descriptors](#Type-Descriptors), Previous: [Closure Function Type](#Closure-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.17 Record Type [¶](#Record-Type-1)

A *record* is much like a `vector`. However, the first
element is used to hold its type as returned by `type-of`. The
purpose of records is to allow programmers to create objects with new
types that are not built into Emacs.

See [Records](#Records), for functions that work with records.

---

Next: [Type Specifiers](#Type-Specifiers), Previous: [Record Type](#Record-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.18 Type Descriptors [¶](#Type-Descriptors-1)

A *type descriptor* is a `record` which holds information
about a type. The first slot in the record must be a symbol naming the type,
and `type-of` relies on this to return the type of `record`
objects. No other type descriptor slot is used by Emacs; they are
free for use by Lisp extensions.

An example of a type descriptor is any instance of
`cl-structure-class`.

---

Next: [Autoload Type](#Autoload-Type), Previous: [Type Descriptors](#Type-Descriptors), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.19 Type Specifiers [¶](#Type-Specifiers-1)

A type specifier is an expression that denotes a type. A type
represents a set of possible values. Type specifiers can be classified
into primitive types and compound types.

Type specifiers are used for several purposes, including: documenting
function interfaces through declarations (see [The `declare` Form](#Declare-Form)),
specifying structure slot types (see [Structures](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Structures) in Common Lisp
Extensions for GNU Emacs Lisp), performing type checks with
`cl-the` (see [Declarations](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Declarations) in Common Lisp Extensions for GNU
Emacs Lisp), and aiding the native compiler (see [Compilation of Lisp to Native Code](#Native-Compilation)) in optimizing code generation and inferring function
signatures.

Primitive type specifiers
:   Primitive types specifiers are the basic types (i.e. not composed by other
    type specifiers).

    Built-in primitive types (like `integer`, `float`,
    `string` etc.) are listed in [Type Hierarchy of Emacs Lisp Objects](#Type-Hierarchy).

Compound type specifiers
:   Compound types serve the purpose of defining more complex or precise
    type specifications by combining or modifying simpler types.

    List of compound type specifiers:

    `(or type-1 … type-n)`
    :   The `or` type specifier describes a type that satisfies at least
        one of the given types.

    `(and type-1 … type-n)`
    :   Similarly the `and` type specifier describes a type that satisfies
        all of the given types.

    `(not type)`
    :   The `not` type specifier defines any type except the specified one.

    `(member value-1 … value-n)`
    :   The `member` type specifier allows to specify a type that includes
        only the explicitly listed values.

    `(function (arg-1-type … arg-n-type) return-type)` [¶](#index-declared-type-of-a-function)
    :   The `function` type specifier is used to describe the argument
        types and the return type of a function. Argument types can be interleaved
        with symbols `&optional` and `&rest` to match the function’s
        arguments (see [Features of Argument Lists](#Argument-List)).

        The following type specifier represents a function whose first parameter
        is of type `symbol`, the second optional parameter is of type
        `float`, and which returns an `integer`:

        ```
         (function (symbol &optional float) integer)
        ```

    `(integer lower-bound upper-bound)`
    :   The `integer` type specifier can also be used as a compound type
        specifier to define a subset of integer values by specifying a range.
        This allows to precisely control which integers are valid for a given
        type.

        lower-bound is the minimum integer value in the range and
        upper-bound the maximum. You can use `*` instead of the
        lower or upper bound to indicate no limit.

        The following represents all integers from -10 to 10:

        ```
        (integer -10 10)
        ```

        The following represents the single value of 10:

        ```
        (integer 10 10)
        ```

        The following represents all the integers from negative infinity to 10:

        ```
        (integer * 10)
        ```

---

Next: [Finalizer Type](#Finalizer-Type), Previous: [Type Specifiers](#Type-Specifiers), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.20 Autoload Type [¶](#Autoload-Type-1)

An *autoload object* is a list whose first element is the symbol
`autoload`. It is stored as the function definition of a symbol,
where it serves as a placeholder for the real definition. The autoload
object says that the real definition is found in a file of Lisp code
that should be loaded when necessary. It contains the name of the file,
plus some other information about the real definition.

After the file has been loaded, the symbol should have a new function
definition that is not an autoload object. The new definition is then
called as if it had been there to begin with. From the user’s point of
view, the function call works as expected, using the function definition
in the loaded file.

An autoload object is usually created with the function
`autoload`, which stores the object in the function cell of a
symbol. See [Autoload](#Autoload), for more details.

---

Previous: [Autoload Type](#Autoload-Type), Up: [Programming Types](#Programming-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.21 Finalizer Type [¶](#Finalizer-Type-1)

A *finalizer object* helps Lisp code clean up after objects that
are no longer needed. A finalizer holds a Lisp function object.
When a finalizer object becomes unreachable after a garbage collection
pass, Emacs calls the finalizer’s associated function object.
When deciding whether a finalizer is reachable, Emacs does not count
references from finalizer objects themselves, allowing you to use
finalizers without having to worry about accidentally capturing
references to finalized objects themselves.

Errors in finalizers are printed to `*Messages*`. Emacs runs
a given finalizer object’s associated function exactly once, even
if that function fails.

Function: **make-finalizer** function [¶](#index-make_002dfinalizer)
:   Make a finalizer that will run function. function will be
    called after garbage collection when the returned finalizer object
    becomes unreachable. If the finalizer object is reachable only
    through references from finalizer objects, it does not count as
    reachable for the purpose of deciding whether to run function.
    function will be run once per finalizer object.

---

Next: [Read Syntax for Circular Objects](#Circular-Objects), Previous: [Programming Types](#Programming-Types), Up: [Lisp Data Types](#Lisp-Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
