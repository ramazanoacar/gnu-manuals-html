# An Introduction to Programming in Emacs Lisp

The GNU Emacs website is at
[https://www.gnu.org/software/emacs/](/software/emacs/).  
To view this manual in other formats, click
[here](/software/emacs/manual/eintr.html).

This is An Introduction to Programming in Emacs Lisp, for
people who are not programmers.

  
  

Distributed with Emacs version 30.2.

  

Copyright © 1990–1995, 1997, 2001–2025 Free Software
Foundation, Inc.

  

Printed copies available from <https://shop.fsf.org/>. Published by:

```
GNU Press,                        https://www.fsf.org/licensing/gnu-press/
a division of the                 email: sales@fsf.org
Free Software Foundation, Inc.    Tel: +1 (617) 542-5942
31 Milk Street, # 960789          Fax: +1 (617) 542-2652
Boston, MA 02196 USA
```

  

ISBN 1-882114-43-4

> Permission is granted to copy, distribute and/or modify this document
> under the terms of the GNU Free Documentation License, Version 1.3 or
> any later version published by the Free Software Foundation; there
> being no Invariant Section, with the Front-Cover Texts being “A GNU
> Manual”, and with the Back-Cover Texts as in (a) below. A copy of
> the license is included in the section entitled “GNU Free
> Documentation License”.
>
> (a) The FSF’s Back-Cover Text is: “You have the freedom to
> copy and modify this GNU manual. Buying copies from the FSF
> supports it in developing GNU and promoting software freedom.”

This master menu first lists each chapter and index; then it lists
every node in every chapter.## Table of Contents

* [Preface](#Preface)
  + [Why Study Emacs Lisp?](#Why)
  + [On Reading this Text](#On-Reading-this-Text)
  + [For Whom This is Written](#Who-You-Are)
  + [Lisp History](#Lisp-History)
  + [A Note for Novices](#Note-for-Novices)
  + [Thank You](#Thank-You)
* [1 List Processing](#List-Processing)
  + [1.1 Lisp Lists](#Lisp-Lists)
    - [Numbers, Lists inside of Lists](#Numbers-Lists)
    - [1.1.1 Lisp Atoms](#Lisp-Atoms)
    - [1.1.2 Whitespace in Lists](#Whitespace-in-Lists)
    - [1.1.3 GNU Emacs Helps You Type Lists](#Typing-Lists)
  + [1.2 Run a Program](#Run-a-Program)
  + [1.3 Generate an Error Message](#Making-Errors)
  + [1.4 Symbol Names and Function Definitions](#Names-_0026-Definitions)
  + [1.5 The Lisp Interpreter](#Lisp-Interpreter)
    - [Complications](#Complications)
    - [1.5.1 Byte Compiling](#Byte-Compiling)
  + [1.6 Evaluation](#Evaluation)
    - [How the Lisp Interpreter Acts](#How-the-Interpreter-Acts)
    - [1.6.1 Evaluating Inner Lists](#Evaluating-Inner-Lists)
  + [1.7 Variables](#Variables)
    - [`fill-column`, an Example Variable](#fill_002dcolumn-Example)
    - [1.7.1 Error Message for a Symbol Without a Function](#Void-Function)
    - [1.7.2 Error Message for a Symbol Without a Value](#Void-Variable)
  + [1.8 Arguments](#Arguments)
    - [1.8.1 Arguments’ Data Types](#Data-types)
    - [1.8.2 An Argument as the Value of a Variable or List](#Args-as-Variable-or-List)
    - [1.8.3 Variable Number of Arguments](#Variable-Number-of-Arguments)
    - [1.8.4 Using the Wrong Type Object as an Argument](#Wrong-Type-of-Argument)
    - [1.8.5 The `message` Function](#message)
  + [1.9 Setting the Value of a Variable](#setq)
    - [1.9.1 Using `setq`](#Using-setq)
    - [1.9.2 Counting](#Counting)
  + [1.10 Summary](#Summary)
  + [1.11 Exercises](#Error-Message-Exercises)
* [2 Practicing Evaluation](#Practicing-Evaluation)
  + [How to Evaluate](#How-to-Evaluate)
  + [2.1 Buffer Names](#Buffer-Names)
  + [2.2 Getting Buffers](#Getting-Buffers)
  + [2.3 Switching Buffers](#Switching-Buffers)
  + [2.4 Buffer Size and the Location of Point](#Buffer-Size-_0026-Locations)
  + [2.5 Exercise](#Evaluation-Exercise)
* [3 How To Write Function Definitions](#Writing-Defuns)
  + [An Aside about Primitive Functions](#Primitive-Functions)
  + [3.1 The `defun` Macro](#defun)
  + [3.2 Install a Function Definition](#Install)
    - [The effect of installation](#Effect-of-installation)
    - [3.2.1 Change a Function Definition](#Change-a-defun)
  + [3.3 Make a Function Interactive](#Interactive)
    - [An Interactive `multiply-by-seven`, An Overview](#Interactive-multiply_002dby_002dseven)
    - [3.3.1 An Interactive `multiply-by-seven`](#multiply_002dby_002dseven-in-detail)
  + [3.4 Different Options for `interactive`](#Interactive-Options)
  + [3.5 Install Code Permanently](#Permanent-Installation)
  + [3.6 `let`](#let)
    - [`let` Prevents Confusion](#Prevent-confusion)
    - [3.6.1 The Parts of a `let` Expression](#Parts-of-let-Expression)
    - [3.6.2 Sample `let` Expression](#Sample-let-Expression)
    - [3.6.3 Uninitialized Variables in a `let` Statement](#Uninitialized-let-Variables)
    - [3.6.4 How `let` Binds Variables](#How-let-Binds-Variables)
      * [Differences Between Lexical and Dynamic Binding](#Lexical-_0026-Dynamic-Binding-Differences)
      * [Example of Lexical vs. Dynamic Binding](#Lexical-vs-Dynamic-Binding-Example)
  + [3.7 The `if` Special Form](#if)
    - [`if` in more detail](#if-in-more-detail)
    - [3.7.1 The `type-of-animal` Function in Detail](#type_002dof_002danimal-in-detail)
  + [3.8 If–then–else Expressions](#else)
  + [3.9 Truth and Falsehood in Emacs Lisp](#Truth-_0026-Falsehood)
    - [An explanation of `nil`](#nil-explained)
  + [3.10 `save-excursion`](#save_002dexcursion)
    - [Point and Mark](#Point-and-mark)
    - [3.10.1 Template for a `save-excursion` Expression](#Template-for-save_002dexcursion)
  + [3.11 Review](#Review)
  + [3.12 Exercises](#defun-Exercises)
* [4 A Few Buffer-Related Functions](#Buffer-Walk-Through)
  + [4.1 Finding More Information](#Finding-More)
  + [4.2 A Simplified `beginning-of-buffer` Definition](#simplified_002dbeginning_002dof_002dbuffer)
  + [4.3 The Definition of `mark-whole-buffer`](#mark_002dwhole_002dbuffer)
    - [An overview of `mark-whole-buffer`](#mark_002dwhole_002dbuffer-overview)
    - [4.3.1 Body of `mark-whole-buffer`](#Body-of-mark_002dwhole_002dbuffer)
  + [4.4 The Definition of `append-to-buffer`](#append_002dto_002dbuffer)
    - [An Overview of `append-to-buffer`](#append_002dto_002dbuffer-overview)
    - [4.4.1 The `append-to-buffer` Interactive Expression](#append-interactive)
    - [4.4.2 The Body of `append-to-buffer`](#append_002dto_002dbuffer-body)
    - [4.4.3 `save-excursion` in `append-to-buffer`](#append-save_002dexcursion)
  + [4.5 Review](#Buffer-Related-Review)
  + [4.6 Exercises](#Buffer-Exercises)
* [5 A Few More Complex Functions](#More-Complex)
  + [5.1 The Definition of `copy-to-buffer`](#copy_002dto_002dbuffer)
  + [5.2 The Definition of `insert-buffer`](#insert_002dbuffer)
    - [The Code for `insert-buffer`](#insert_002dbuffer-code)
    - [5.2.1 The Interactive Expression in `insert-buffer`](#insert_002dbuffer-interactive)
      * [A Read-only Buffer](#Read_002donly-buffer)
      * [‘`b`’ in an Interactive Expression](#b-for-interactive)
    - [5.2.2 The Body of the `insert-buffer` Function](#insert_002dbuffer-body)
    - [5.2.3 `insert-buffer` With an `if` Instead of an `or`](#if-_0026-or)
    - [5.2.4 The `or` in the Body](#Insert-or)
    - [5.2.5 The `let` Expression in `insert-buffer`](#Insert-let)
    - [5.2.6 New Body for `insert-buffer`](#New-insert_002dbuffer)
  + [5.3 Complete Definition of `beginning-of-buffer`](#beginning_002dof_002dbuffer)
    - [5.3.1 Optional Arguments](#Optional-Arguments)
    - [5.3.2 `beginning-of-buffer` with an Argument](#beginning_002dof_002dbuffer-opt-arg)
      * [Disentangle `beginning-of-buffer`](#Disentangle-beginning_002dof_002dbuffer)
      * [What happens in a large buffer](#Large-buffer-case)
      * [What happens in a small buffer](#Small-buffer-case)
    - [5.3.3 The Complete `beginning-of-buffer`](#beginning_002dof_002dbuffer-complete)
  + [5.4 Review](#Second-Buffer-Related-Review)
  + [5.5 `optional` Argument Exercise](#optional-Exercise)
* [6 Narrowing and Widening](#Narrowing-_0026-Widening)
  + [The Advantages of Narrowing](#Narrowing-advantages)
  + [6.1 The `save-restriction` Special Form](#save_002drestriction)
  + [6.2 `what-line`](#what_002dline)
  + [6.3 Exercise with Narrowing](#narrow-Exercise)
* [7 `car`, `cdr`, `cons`: Fundamental Functions](#car-cdr-_0026-cons)
  + [Strange Names](#Strange-Names)
  + [7.1 `car` and `cdr`](#car-_0026-cdr)
  + [7.2 `cons`](#cons)
    - [Build a list](#Build-a-list)
    - [7.2.1 Find the Length of a List: `length`](#length)
  + [7.3 `nthcdr`](#nthcdr)
  + [7.4 `nth`](#nth)
  + [7.5 `setcar`](#setcar)
  + [7.6 `setcdr`](#setcdr)
  + [7.7 Exercise](#cons-Exercise)
* [8 Cutting and Storing Text](#Cutting-_0026-Storing-Text)
  + [Storing Text in a List](#Storing-Text)
  + [8.1 `zap-to-char`](#zap_002dto_002dchar)
    - [The Complete `zap-to-char` Implementation](#Complete-zap_002dto_002dchar)
    - [8.1.1 The `interactive` Expression](#zap_002dto_002dchar-interactive)
    - [8.1.2 The Body of `zap-to-char`](#zap_002dto_002dchar-body)
    - [8.1.3 The `search-forward` Function](#search_002dforward)
    - [8.1.4 The `progn` Special Form](#progn)
    - [8.1.5 Summing up `zap-to-char`](#Summing-up-zap_002dto_002dchar)
  + [8.2 `kill-region`](#kill_002dregion)
    - [The Complete `kill-region` Definition](#Complete-kill_002dregion)
    - [8.2.1 `condition-case`](#condition_002dcase)
    - [8.2.2 Lisp macro](#Lisp-macro)
  + [8.3 `copy-region-as-kill`](#copy_002dregion_002das_002dkill)
    - [The complete `copy-region-as-kill` function definition](#Complete-copy_002dregion_002das_002dkill)
    - [8.3.1 The Body of `copy-region-as-kill`](#copy_002dregion_002das_002dkill-body)
      * [`last-command` and `this-command`](#last_002dcommand-_0026-this_002dcommand)
      * [The `kill-append` function](#kill_002dappend-function)
      * [The `kill-new` function](#kill_002dnew-function)
  + [8.4 Digression into C](#Digression-into-C)
  + [8.5 Initializing a Variable with `defvar`](#defvar)
    - [Seeing the Current Value of a Variable](#See-variable-current-value)
    - [8.5.1 `defvar` and an asterisk](#defvar-and-asterisk)
  + [8.6 Review](#cons-_0026-search_002dfwd-Review)
  + [8.7 Searching Exercises](#search-Exercises)
* [9 How Lists are Implemented](#List-Implementation)
  + [Lists diagrammed](#Lists-diagrammed)
  + [9.1 Symbols as a Chest of Drawers](#Symbols-as-Chest)
  + [9.2 Exercise](#List-Exercise)
* [10 Yanking Text Back](#Yanking)
  + [10.1 Kill Ring Overview](#Kill-Ring-Overview)
  + [10.2 The `kill-ring-yank-pointer` Variable](#kill_002dring_002dyank_002dpointer)
  + [10.3 Exercises with `yank` and `nthcdr`](#yank-nthcdr-Exercises)
* [11 Loops and Recursion](#Loops-_0026-Recursion)
  + [11.1 `while`](#while)
    - [Looping with `while`](#Looping-with-while)
    - [11.1.1 A `while` Loop and a List](#Loop-Example)
    - [11.1.2 An Example: `print-elements-of-list`](#print_002delements_002dof_002dlist)
    - [11.1.3 A Loop with an Incrementing Counter](#Incrementing-Loop)
    - [Details of an Incrementing Loop](#Incrementing-Loop-Details)
      * [Example with incrementing counter](#Incrementing-Example)
      * [The parts of the function definition](#Inc-Example-parts)
      * [Putting the function definition together](#Inc-Example-altogether)
    - [11.1.4 Loop with a Decrementing Counter](#Decrementing-Loop)
      * [Example with decrementing counter](#Decrementing-Example)
      * [The parts of the function definition](#Dec-Example-parts)
      * [Putting the function definition together](#Dec-Example-altogether)
  + [11.2 Save your time: `dolist` and `dotimes`](#dolist-dotimes)
    - [The `dolist` Macro](#dolist)
    - [The `dotimes` Macro](#dotimes)
  + [11.3 Recursion](#Recursion)
    - [11.3.1 Building Robots: Extending the Metaphor](#Building-Robots)
    - [11.3.2 The Parts of a Recursive Definition](#Recursive-Definition-Parts)
    - [11.3.3 Recursion with a List](#Recursion-with-list)
    - [11.3.4 Recursion in Place of a Counter](#Recursive-triangle-function)
      * [An argument of 1 or 2](#Recursive-Example-arg-of-1-or-2)
      * [An argument of 3 or 4](#Recursive-Example-arg-of-3-or-4)
    - [11.3.5 Recursion Example Using `cond`](#Recursion-with-cond)
    - [11.3.6 Recursive Patterns](#Recursive-Patterns)
      * [Recursive Pattern: *every*](#Every)
      * [Recursive Pattern: *accumulate*](#Accumulate)
      * [Recursive Pattern: *keep*](#Keep)
    - [11.3.7 Recursion without Deferments](#No-Deferment)
    - [11.3.8 No Deferment Solution](#No-deferment-solution)
  + [11.4 Looping Exercise](#Looping-exercise)
* [12 Regular Expression Searches](#Regexp-Search)
  + [12.1 The Regular Expression for `sentence-end`](#sentence_002dend)
  + [12.2 The `re-search-forward` Function](#re_002dsearch_002dforward)
  + [12.3 `forward-sentence`](#forward_002dsentence)
    - [Complete `forward-sentence` function definition](#Complete-forward_002dsentence)
    - [The `while` loops](#fwd_002dsentence-while-loops)
    - [The regular expression search](#fwd_002dsentence-re_002dsearch)
  + [12.4 `forward-paragraph`: a Goldmine of Functions](#forward_002dparagraph)
    - [Shortened `forward-paragraph` function definition](#forward_002dparagraph-in-brief)
    - [The `let*` expression](#fwd_002dpara-let)
    - [The forward motion `while` loop](#fwd_002dpara-while)
  + [12.5 Review](#Regexp-Review)
  + [12.6 Exercises with `re-search-forward`](#re_002dsearch-Exercises)
* [13 Counting via Repetition and Regexps](#Counting-Words)
  + [Counting words](#Why-Count-Words)
  + [13.1 The `count-words-example` Function](#count_002dwords_002dexample)
    - [Designing `count-words-example`](#Design-count_002dwords_002dexample)
    - [13.1.1 The Whitespace Bug in `count-words-example`](#Whitespace-Bug)
  + [13.2 Count Words Recursively](#recursive_002dcount_002dwords)
  + [13.3 Exercise: Counting Punctuation](#Counting-Exercise)
* [14 Counting Words in a `defun`](#Words-in-a-defun)
  + [Divide and Conquer](#Divide-and-Conquer)
  + [14.1 What to Count?](#Words-and-Symbols)
  + [14.2 What Constitutes a Word or Symbol?](#Syntax)
  + [14.3 The `count-words-in-defun` Function](#count_002dwords_002din_002ddefun)
  + [14.4 Count Several `defuns` Within a File](#Several-defuns)
  + [14.5 Find a File](#Find-a-File)
  + [14.6 `lengths-list-file` in Detail](#lengths_002dlist_002dfile)
  + [14.7 Count Words in `defuns` in Different Files](#Several-files)
    - [Determine the lengths of `defuns`](#lengths_002dlist_002dmany_002dfiles)
    - [14.7.1 The `append` Function](#append)
  + [14.8 Recursively Count Words in Different Files](#Several-files-recursively)
  + [14.9 Prepare the Data for Display in a Graph](#Prepare-the-data)
    - [The Data for Display in Detail](#Data-for-Display-in-Detail)
    - [14.9.1 Sorting Lists](#Sorting)
    - [14.9.2 Making a List of Files](#Files-List)
    - [14.9.3 Counting function definitions](#Counting-function-definitions)
* [15 Readying a Graph](#Readying-a-Graph)
  + [Printing the Columns of a Graph](#Columns-of-a-graph)
  + [15.1 The `graph-body-print` Function](#graph_002dbody_002dprint)
  + [15.2 The `recursive-graph-body-print` Function](#recursive_002dgraph_002dbody_002dprint)
  + [15.3 Need for Printed Axes](#Printed-Axes)
  + [15.4 Exercise](#Line-Graph-Exercise)
* [16 Your `.emacs` File](#Emacs-Initialization)
  + [Emacs’s Default Configuration](#Default-Configuration)
  + [16.1 Site-wide Initialization Files](#Site_002dwide-Init)
  + [16.2 Specifying Variables using `defcustom`](#defcustom)
  + [16.3 Beginning a `.emacs` File](#Beginning-init-File)
  + [16.4 Text and Auto Fill Mode](#Text-and-Auto_002dfill)
  + [16.5 Mail Aliases](#Mail-Aliases)
  + [16.6 Indent Tabs Mode](#Indent-Tabs-Mode)
  + [16.7 Some Key Bindings](#Key-Bindings)
    - [16.7.1 Legacy Global Key Binding Commands](#Legacy-Global-Key-Binding-Commands)
  + [16.8 Keymaps](#Keymaps)
  + [16.9 Loading Files](#Loading-Files)
  + [16.10 Autoloading](#Autoload)
  + [16.11 A Simple Extension: `line-to-top-of-window`](#Simple-Extension)
  + [16.12 X11 Colors](#X11-Colors)
  + [16.13 Miscellaneous Settings for a `.emacs` File](#Miscellaneous)
  + [16.14 A Modified Mode Line](#Mode-Line)
* [17 Debugging](#Debugging)
  + [17.1 `debug`](#debug)
  + [17.2 `debug-on-entry`](#debug_002don_002dentry)
  + [17.3 `debug-on-quit` and `(debug)`](#debug_002don_002dquit)
  + [17.4 The `edebug` Source Level Debugger](#edebug)
  + [17.5 Debugging Exercises](#Debugging-Exercises)
* [18 Conclusion](#Conclusion)
* [Appendix A The `the-the` Function](#the_002dthe)
* [Appendix B Handling the Kill Ring](#Kill-Ring)
  + [What the Kill Ring Does](#What-the-Kill-Ring-Does)
  + [B.1 The `current-kill` Function](#current_002dkill)
    - [The code for `current-kill`](#Code-for-current_002dkill)
    - [`current-kill` in Outline](#Understanding-current_002dkill)
      * [The Body of `current-kill`](#Body-of-current_002dkill)
      * [Digression about the word “error”](#Digression-concerning-error)
      * [Determining the Element](#Determining-the-Element)
  + [B.2 `yank`](#yank)
  + [B.3 `yank-pop`](#yank_002dpop)
  + [B.4 The `ring.el` File](#ring-file)
* [Appendix C A Graph with Labeled Axes](#Full-Graph)
  + [Labeled Example Graph](#Labeled-Example)
  + [C.1 The `print-graph` Varlist](#print_002dgraph-Varlist)
  + [C.2 The `print-Y-axis` Function](#print_002dY_002daxis)
    - [The `print-Y-axis` Function in Detail](#print_002dY_002daxis-in-Detail)
    - [What height should the label be?](#Height-of-label)
    - [C.2.1 Side Trip: Compute a Remainder](#Compute-a-Remainder)
    - [C.2.2 Construct a Y Axis Element](#Y-Axis-Element)
    - [C.2.3 Create a Y Axis Column](#Y_002daxis_002dcolumn)
    - [C.2.4 The Not Quite Final Version of `print-Y-axis`](#print_002dY_002daxis-Penultimate)
  + [C.3 The `print-X-axis` Function](#print_002dX_002daxis)
    - [Similarities and differences](#Similarities-differences)
    - [C.3.1 X Axis Tic Marks](#X-Axis-Tic-Marks)
  + [C.4 Printing the Whole Graph](#Print-Whole-Graph)
    - [Changes for the Final Version](#The-final-version)
    - [C.4.1 Testing `print-graph`](#Test-print_002dgraph)
    - [C.4.2 Graphing Numbers of Words and Symbols](#Graphing-words-in-defuns)
    - [C.4.3 A `lambda` Expression: Useful Anonymity](#lambda)
    - [C.4.4 The `mapcar` Function](#mapcar)
    - [C.4.5 Another Bug … Most Insidious](#Another-Bug)
    - [C.4.6 The Printed Graph](#Final-printed-graph)
* [Appendix D Free Software and Free Manuals](#Free-Software-and-Free-Manuals)
* [Appendix E GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Index](#Index)
* [About the Author](#About-the-Author)## Short Table of Contents

* [Preface](#toc-Preface-1)
* [1 List Processing](#toc-List-Processing-1)
* [2 Practicing Evaluation](#toc-Practicing-Evaluation-1)
* [3 How To Write Function Definitions](#toc-How-To-Write-Function-Definitions)
* [4 A Few Buffer-Related Functions](#toc-A-Few-Buffer_002dRelated-Functions)
* [5 A Few More Complex Functions](#toc-A-Few-More-Complex-Functions)
* [6 Narrowing and Widening](#toc-Narrowing-and-Widening)
* [7 `car`, `cdr`, `cons`: Fundamental Functions](#toc-car_002c-cdr_002c-cons_003a-Fundamental-Functions)
* [8 Cutting and Storing Text](#toc-Cutting-and-Storing-Text)
* [9 How Lists are Implemented](#toc-How-Lists-are-Implemented)
* [10 Yanking Text Back](#toc-Yanking-Text-Back)
* [11 Loops and Recursion](#toc-Loops-and-Recursion)
* [12 Regular Expression Searches](#toc-Regular-Expression-Searches)
* [13 Counting via Repetition and Regexps](#toc-Counting-via-Repetition-and-Regexps)
* [14 Counting Words in a `defun`](#toc-Counting-Words-in-a-defun)
* [15 Readying a Graph](#toc-Readying-a-Graph-1)
* [16 Your `.emacs` File](#toc-Your-_002eemacs-File)
* [17 Debugging](#toc-Debugging-1)
* [18 Conclusion](#toc-Conclusion-1)
* [Appendix A The `the-the` Function](#toc-The-the_002dthe-Function)
* [Appendix B Handling the Kill Ring](#toc-Handling-the-Kill-Ring)
* [Appendix C A Graph with Labeled Axes](#toc-A-Graph-with-Labeled-Axes)
* [Appendix D Free Software and Free Manuals](#toc-Free-Software-and-Free-Manuals-1)
* [Appendix E GNU Free Documentation License](#toc-GNU-Free-Documentation-License-1)
* [Index](#toc-Index-1)
* [About the Author](#toc-About-the-Author-1)

---

Next: [List Processing](#List-Processing), Previous: [An Introduction to Programming in Emacs Lisp](#Top), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Preface

Most of the GNU Emacs integrated environment is written in the programming
language called Emacs Lisp. The code written in this programming
language is the software—the sets of instructions—that tell the
computer what to do when you give it commands. Emacs is designed so
that you can write new code in Emacs Lisp and easily install it as an
extension to the editor.

(GNU Emacs is sometimes called an “extensible editor”, but it does
much more than provide editing capabilities. It is better to refer to
Emacs as an “extensible computing environment”. However, that
phrase is quite a mouthful. It is easier to refer to Emacs simply as
an editor. Moreover, everything you do in Emacs—find the Mayan date
and phases of the moon, simplify polynomials, debug code, manage
files, read letters, write books—all these activities are kinds of
editing in the most general sense of the word.)

* [Why Study Emacs Lisp?](#Why)
* [On Reading this Text](#On-Reading-this-Text)
* [For Whom This is Written](#Who-You-Are)
* [Lisp History](#Lisp-History)
* [A Note for Novices](#Note-for-Novices)
* [Thank You](#Thank-You)

---

Next: [On Reading this Text](#On-Reading-this-Text), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Why Study Emacs Lisp?

Although Emacs Lisp is usually thought of in association only with Emacs,
it is a full computer programming language. You can use Emacs Lisp as
you would any other programming language.

Perhaps you want to understand programming; perhaps you want to extend
Emacs; or perhaps you want to become a programmer. This introduction to
Emacs Lisp is designed to get you started: to guide you in learning the
fundamentals of programming, and more importantly, to show you how you
can teach yourself to go further.

---

Next: [For Whom This is Written](#Who-You-Are), Previous: [Why Study Emacs Lisp?](#Why), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### On Reading this Text

All through this document, you will see little sample programs you can
run inside of Emacs. If you read this document in Info inside of GNU
Emacs, you can run the programs as they appear. (This is easy to do and
is explained when the examples are presented.) Alternatively, you can
read this introduction as a printed book while sitting beside a computer
running Emacs. (This is what I like to do; I like printed books.) If
you don’t have a running Emacs beside you, you can still read this book,
but in this case, it is best to treat it as a novel or as a travel guide
to a country not yet visited: interesting, but not the same as being
there.

Much of this introduction is dedicated to walkthroughs or guided tours
of code used in GNU Emacs. These tours are designed for two purposes:
first, to give you familiarity with real, working code (code you use
every day); and, second, to give you familiarity with the way Emacs
works. It is interesting to see how a working environment is
implemented.
Also, I
hope that you will pick up the habit of browsing through source code.
You can learn from it and mine it for ideas. Having GNU Emacs is like
having a dragon’s cave of treasures.

In addition to learning about Emacs as an editor and Emacs Lisp as a
programming language, the examples and guided tours will give you an
opportunity to get acquainted with Emacs as a Lisp programming
environment. GNU Emacs supports programming and provides tools that
you will want to become comfortable using, such as `M-.` (the key
which invokes the `xref-find-definitions` command). You will
also learn about buffers and other objects that are part of the
environment. Learning about these features of Emacs is like learning
new routes around your home town.

Finally, I hope to convey some of the skills for using Emacs to
learn aspects of programming that you don’t know. You can often use
Emacs to help you understand what puzzles you or to find out how to do
something new. This self-reliance is not only a pleasure, but an
advantage.

---

Next: [Lisp History](#Lisp-History), Previous: [On Reading this Text](#On-Reading-this-Text), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### For Whom This is Written

This text is written as an elementary introduction for people who are
not programmers. If you are a programmer, you may not be satisfied with
this primer. The reason is that you may have become expert at reading
reference manuals and be put off by the way this text is organized.

An expert programmer who reviewed this text said to me:

> *I prefer to learn from reference manuals. I “dive into” each
> paragraph, and “come up for air” between paragraphs.*
>
> *When I get to the end of a paragraph, I assume that subject is
> done, finished, that I know everything I need (with the
> possible exception of the case when the next paragraph starts talking
> about it in more detail). I expect that a well written reference manual
> will not have a lot of redundancy, and that it will have excellent
> pointers to the (one) place where the information I want is.*

This introduction is not written for this person!

Firstly, I try to say everything at least three times: first, to
introduce it; second, to show it in context; and third, to show it in a
different context, or to review it.

Secondly, I hardly ever put all the information about a subject in one
place, much less in one paragraph. To my way of thinking, that imposes
too heavy a burden on the reader. Instead I try to explain only what
you need to know at the time. (Sometimes I include a little extra
information so you won’t be surprised later when the additional
information is formally introduced.)

When you read this text, you are not expected to learn everything the
first time. Frequently, you need make only a nodding
acquaintance with some of the items mentioned. My hope is that I have
structured the text and given you enough hints that you will be alert to
what is important, and concentrate on it.

You will need to dive into some paragraphs; there is no other way
to read them. But I have tried to keep down the number of such
paragraphs. This book is intended as an approachable hill, rather than
as a daunting mountain.

This book, An Introduction to Programming in Emacs Lisp, has a
companion document,
[The GNU Emacs Lisp Reference Manual](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Top) in The GNU
Emacs Lisp Reference Manual.
The reference manual has more detail than this introduction. In the
reference manual, all the information about one topic is concentrated
in one place. You should turn to it if you are like the programmer
quoted above. And, of course, after you have read this
Introduction, you will find the Reference Manual useful
when you are writing your own programs.

---

Next: [A Note for Novices](#Note-for-Novices), Previous: [For Whom This is Written](#Who-You-Are), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Lisp History

Lisp was first developed in the late 1950s at the Massachusetts
Institute of Technology for research in artificial intelligence. The
great power of the Lisp language makes it superior for other purposes as
well, such as writing editor commands and integrated environments.

GNU Emacs Lisp is largely inspired by Maclisp, which was written at MIT
in the 1960s. It is somewhat inspired by Common Lisp, which became a
standard in the 1980s. However, Emacs Lisp is much simpler than Common
Lisp. (The standard Emacs distribution contains an optional extensions
file, `cl-lib.el`, that adds many Common Lisp features to Emacs Lisp.)

---

Next: [Thank You](#Thank-You), Previous: [Lisp History](#Lisp-History), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A Note for Novices

If you don’t know GNU Emacs, you can still read this document
profitably. However, I recommend you learn Emacs, if only to learn to
move around your computer screen. You can teach yourself how to use
Emacs with the built-in tutorial. To use it, type `C-h t`. (This
means you press and release the `CTRL` key and the `h` at the
same time, and then press and release `t`.)

Also, I often refer to one of Emacs’s standard commands by listing the
keys which you press to invoke the command and then giving the name of
the command in parentheses, like this: `C-M-\`
(`indent-region`). What this means is that the
`indent-region` command is customarily invoked by typing
`C-M-\`. (You can, if you wish, change the keys that are typed to
invoke the command; this is called *rebinding*. See [Keymaps](#Keymaps).) The abbreviation `C-M-\` means that you type your
`CTRL` key, `META` key, and `\` key all at the same time.
(On many modern keyboards the `META` key is labeled
`ALT`.)
Sometimes a combination like this is called a keychord, since it is
similar to the way you play a chord on a piano. If your keyboard does
not have a `META` key, the `ESC` key prefix is used in place
of it. In this case, `C-M-\` means that you press and release your
`ESC` key and then type the `CTRL` key and the `\` key at
the same time. But usually `C-M-\` means press the `CTRL` key
along with the key that is labeled `ALT` and, at the same time,
press the `\` key.

In addition to typing a lone keychord, you can prefix what you type
with `C-u`, which is called the *universal argument*. The
`C-u` keychord passes an argument to the subsequent command.
Thus, to indent a region of plain text by 6 spaces, mark the region,
and then type `C-u 6 C-M-\`. (If you do not specify a number,
Emacs either passes the number 4 to the command or otherwise runs the
command differently than it would otherwise.) See [Numeric Arguments](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Arguments) in The GNU Emacs Manual.

If you are reading this in Info using GNU Emacs, you can read through
this whole document just by pressing the space bar, `SPC`.
(To learn about Info, type `C-h i` and then select Info.)

A note on terminology: when I use the word Lisp alone, I often am
referring to the various dialects of Lisp in general, but when I speak
of Emacs Lisp, I am referring to GNU Emacs Lisp in particular.

---

Previous: [A Note for Novices](#Note-for-Novices), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Thank You

My thanks to all who helped me with this book. My especial thanks to
Jim Blandy, Noah Friedman, Jim Kingdon, Roland
McGrath, Frank Ritter, Randy Smith, Richard M. Stallman, and Melissa Weisshaus. My thanks also go to both
Philip Johnson and David Stampe for their patient
encouragement. My mistakes are my own.

Robert J. Chassell
bob@gnu.org

---

Next: [Practicing Evaluation](#Practicing-Evaluation), Previous: [Preface](#Preface), Up: [An Introduction to Programming in Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 1 List Processing

To the untutored eye, Lisp is a strange programming language. In Lisp
code there are parentheses everywhere. Some people even claim that
the name stands for “Lots of Isolated Silly Parentheses”. But the
claim is unwarranted. Lisp stands for LISt Processing, and the
programming language handles *lists* (and lists of lists) by
putting them between parentheses. The parentheses mark the boundaries
of the list. Sometimes a list is preceded by an apostrophe ‘`'`’,
called a *single-quote* in Lisp.[1](#FOOT1) Lists are the basis
of Lisp.

* [Lisp Lists](#Lisp-Lists)
* [Run a Program](#Run-a-Program)
* [Generate an Error Message](#Making-Errors)
* [Symbol Names and Function Definitions](#Names-_0026-Definitions)
* [The Lisp Interpreter](#Lisp-Interpreter)
* [Evaluation](#Evaluation)
* [Variables](#Variables)
* [Arguments](#Arguments)
* [Setting the Value of a Variable](#setq)
* [Summary](#Summary)
* [Exercises](#Error-Message-Exercises)

---

Next: [Run a Program](#Run-a-Program), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.1 Lisp Lists

In Lisp, a list looks like this: `'(rose violet daisy buttercup)`.
This list is preceded by a single apostrophe. It could just as well be
written as follows, which looks more like the kind of list you are likely
to be familiar with:

```
'(rose
  violet
  daisy
  buttercup)
```

The elements of this list are the names of the four different flowers,
separated from each other by whitespace and surrounded by parentheses,
like flowers in a field with a stone wall around them.

* [Numbers, Lists inside of Lists](#Numbers-Lists)
* [Lisp Atoms](#Lisp-Atoms)
* [Whitespace in Lists](#Whitespace-in-Lists)
* [GNU Emacs Helps You Type Lists](#Typing-Lists)

---

Next: [Lisp Atoms](#Lisp-Atoms), Up: [Lisp Lists](#Lisp-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Numbers, Lists inside of Lists

Lists can also have numbers in them, as in this list: `(+ 2 2)`.
This list has a plus-sign, ‘`+`’, followed by two ‘`2`’s, each
separated by whitespace.

In Lisp, both data and programs are represented the same way; that is,
they are both lists of words, numbers, or other lists, separated by
whitespace and surrounded by parentheses. (Since a program looks like
data, one program may easily serve as data for another; this is a very
powerful feature of Lisp.) (Incidentally, these two parenthetical
remarks are *not* Lisp lists, because they contain ‘`;`’ and
‘`.`’ as punctuation marks.)

Here is another list, this time with a list inside of it:

```
'(this list has (a list inside of it))
```

The components of this list are the words ‘`this`’, ‘`list`’,
‘`has`’, and the list ‘`(a list inside of it)`’. The interior
list is made up of the words ‘`a`’, ‘`list`’, ‘`inside`’,
‘`of`’, ‘`it`’.

---

Next: [Whitespace in Lists](#Whitespace-in-Lists), Previous: [Numbers, Lists inside of Lists](#Numbers-Lists), Up: [Lisp Lists](#Lisp-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.1 Lisp Atoms

In Lisp, what we have been calling words are called *atoms*. This
term comes from the historical meaning of the word atom, which means
“indivisible”. As far as Lisp is concerned, the words we have been
using in the lists cannot be divided into any smaller parts and still
mean the same thing as part of a program; likewise with numbers and
single character symbols like ‘`+`’. On the other hand, unlike an
ancient atom, a list can be split into parts. (See [`car` `cdr` & `cons` Fundamental Functions](#car-cdr-_0026-cons).)

In a list, atoms are separated from each other by whitespace. They can be
right next to a parenthesis.

Technically speaking, a list in Lisp consists of parentheses surrounding
atoms separated by whitespace or surrounding other lists or surrounding
both atoms and other lists. A list can have just one atom in it or
have nothing in it at all. A list with nothing in it looks like this:
`()`, and is called the *empty list*. Unlike anything else, an
empty list is considered both an atom and a list at the same time.

The printed representation of both atoms and lists are called
*symbolic expressions* or, more concisely, *s-expressions*.
The word *expression* by itself can refer to either the printed
representation, or to the atom or list as it is held internally in the
computer. Often, people use the term *expression*
indiscriminately. (Also, in many texts, the word *form* is used
as a synonym for expression.)

Incidentally, the atoms that make up our universe were named such when
they were thought to be indivisible; but it has been found that physical
atoms are not indivisible. Parts can split off an atom or it can
fission into two parts of roughly equal size. Physical atoms were named
prematurely, before their truer nature was found. In Lisp, certain
kinds of atom, such as an array, can be separated into parts; but the
mechanism for doing this is different from the mechanism for splitting a
list. As far as list operations are concerned, the atoms of a list are
unsplittable.

As in English, the meanings of the component letters of a Lisp atom
are different from the meaning the letters make as a word. For
example, the word for the South American sloth, the ‘`ai`’, is
completely different from the two words, ‘`a`’, and ‘`i`’.

There are many kinds of atom in nature but only a few in Lisp: for
example, *numbers*, such as 37, 511, or 1729, and *symbols*, such
as ‘`+`’, ‘`foo`’, or ‘`forward-line`’. The words we have
listed in the examples above are all symbols. In everyday Lisp
conversation, the word “atom” is not often used, because programmers
usually try to be more specific about what kind of atom they are dealing
with. Lisp programming is mostly about symbols (and sometimes numbers)
within lists. (Incidentally, the preceding three word parenthetical
remark is a proper list in Lisp, since it consists of atoms, which in
this case are symbols, separated by whitespace and enclosed by
parentheses, without any non-Lisp punctuation.)

Text between double quotation marks—even sentences or
paragraphs—is also an atom. Here is an example:

```
'(this list includes "text between quotation marks.")
```

In Lisp, all of the quoted text including the punctuation mark and the
blank spaces is a single atom. This kind of atom is called a
*string* (for “string of characters”) and is the sort of thing that
is used for messages that a computer can print for a human to read.
Strings are a different kind of atom than numbers or symbols and are
used differently.

---

Next: [GNU Emacs Helps You Type Lists](#Typing-Lists), Previous: [Lisp Atoms](#Lisp-Atoms), Up: [Lisp Lists](#Lisp-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.2 Whitespace in Lists

The amount of whitespace in a list does not matter. From the point of view
of the Lisp language,

```
'(this list
   looks like this)
```

is exactly the same as this:

```
'(this list looks like this)
```

Both examples show what to Lisp is the same list, the list made up of
the symbols ‘`this`’, ‘`list`’, ‘`looks`’, ‘`like`’, and
‘`this`’ in that order.

Extra whitespace and newlines are designed to make a list more readable
by humans. When Lisp reads the expression, it gets rid of all the extra
whitespace (but it needs to have at least one space between atoms in
order to tell them apart.)

Odd as it seems, the examples we have seen cover almost all of what Lisp
lists look like! Every other list in Lisp looks more or less like one
of these examples, except that the list may be longer and more complex.
In brief, a list is between parentheses, a string is between quotation
marks, a symbol looks like a word, and a number looks like a number.
(For certain situations, square brackets, dots and a few other special
characters may be used; however, we will go quite far without them.)

---

Previous: [Whitespace in Lists](#Whitespace-in-Lists), Up: [Lisp Lists](#Lisp-Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.3 GNU Emacs Helps You Type Lists

When you type a Lisp expression in GNU Emacs using either Lisp
Interaction mode or Emacs Lisp mode, you have available to you several
commands to format the Lisp expression so it is easy to read. For
example, pressing the `TAB` key automatically indents the line the
cursor is on by the right amount. A command to properly indent the
code in a region is customarily bound to `C-M-\`. Indentation is
designed so that you can see which elements of a list belong to which
list—elements of a sub-list are indented more than the elements of
the enclosing list.

In addition, when you type a closing parenthesis, Emacs momentarily
jumps the cursor back to the matching opening parenthesis, so you can
see which one it is. This is very useful, since every list you type
in Lisp must have its closing parenthesis match its opening
parenthesis. (See [Major Modes](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Major-Modes) in The GNU Emacs
Manual, for more information about Emacs’s modes.)

---

Next: [Generate an Error Message](#Making-Errors), Previous: [Lisp Lists](#Lisp-Lists), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.2 Run a Program

A list in Lisp—any list—is a program ready to run. If you run it
(for which the Lisp jargon is *evaluate*), the computer will do one
of three things: do nothing except return to you the list itself; send
you an error message; or, treat the first symbol in the list as a
command to do something. (Usually, of course, it is the last of these
three things that you really want!)

The single apostrophe, `'`, that I put in front of some of the
example lists in preceding sections is called a *quote*; when it
precedes a list, it tells Lisp to do nothing with the list, other than
take it as it is written. But if there is no quote preceding a list,
the first item of the list is special: it is a command for the computer
to obey. (In Lisp, these commands are called *functions*.) The list
`(+ 2 2)` shown above did not have a quote in front of it, so Lisp
understands that the `+` is an instruction to do something with the
rest of the list: add the numbers that follow.

If you are reading this inside of GNU Emacs in Info, here is how you can
evaluate such a list: place your cursor immediately after the right
hand parenthesis of the following list and then type `C-x C-e`:

```
(+ 2 2)
```

You will see the number `4` appear in the echo area[2](#FOOT2). (What you have just done is evaluate the list. The echo area is
the line at the bottom of the screen that displays or echoes text.)
Now try the same thing with a quoted list: place the cursor right
after the following list and type `C-x C-e`:

```
'(this is a quoted list)
```

You will see `(this is a quoted list)` appear in the echo area.

In both cases, what you are doing is giving a command to the program
inside of GNU Emacs called the *Lisp interpreter*—giving the
interpreter a command to evaluate the expression. The name of the Lisp
interpreter comes from the word for the task done by a human who comes
up with the meaning of an expression—who interprets it.

You can also evaluate an atom that is not part of a list—one that is
not surrounded by parentheses; again, the Lisp interpreter translates
from the humanly readable expression to the language of the computer.
But before discussing this (see [Variables](#Variables)), we will discuss what the
Lisp interpreter does when you make an error.

---

Next: [Symbol Names and Function Definitions](#Names-_0026-Definitions), Previous: [Run a Program](#Run-a-Program), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.3 Generate an Error Message

Partly so you won’t worry if you do it accidentally, we will now give
a command to the Lisp interpreter that generates an error message.
This is a harmless activity; and indeed, we will often try to generate
error messages intentionally. Once you understand the jargon, error
messages can be informative. Instead of being called “error”
messages, they should be called “help” messages. They are like
signposts to a traveler in a strange country; deciphering them can be
hard, but once understood, they can point the way.

The error message is generated by a built-in GNU Emacs debugger. We
will enter the debugger. You get out of the debugger by typing `q`.

What we will do is evaluate a list that is not quoted and does not
have a meaningful command as its first element. Here is a list almost
exactly the same as the one we just used, but without the single-quote
in front of it. Position the cursor right after it and type `C-x
C-e`:

```
(this is an unquoted list)
```

A `*Backtrace*` window will open up and you should see the
following in it:

```
---------- Buffer: *Backtrace* ----------
Debugger entered--Lisp error: (void-function this)
  (this is an unquoted list)
  eval((this is an unquoted list) nil)
  elisp--eval-last-sexp(nil)
  eval-last-sexp(nil)
  funcall-interactively(eval-last-sexp nil)
  call-interactively(eval-last-sexp nil nil)
  command-execute(eval-last-sexp)
---------- Buffer: *Backtrace* ----------
```

Your cursor will be in this window (you may have to wait a few seconds
before it becomes visible). To quit the debugger and make the
debugger window go away, type:

```
q
```

Please type `q` right now, so you become confident that you can
get out of the debugger. Then, type `C-x C-e` again to re-enter
it.

Based on what we already know, we can almost read this error message.

You read the `*Backtrace*` buffer from the bottom up; it tells
you what Emacs did. When you typed `C-x C-e`, you made an
interactive call to the command `eval-last-sexp`. `eval` is
an abbreviation for “evaluate” and `sexp` is an abbreviation for
“symbolic expression”. The command means “evaluate last symbolic
expression”, which is the expression just before your cursor.

Each line above tells you what the Lisp interpreter evaluated next.
The most recent action is at the top. The buffer is called the
`*Backtrace*` buffer because it enables you to track Emacs
backwards.

At the top of the `*Backtrace*` buffer, you see the line:

```
Debugger entered--Lisp error: (void-function this)
```

The Lisp interpreter tried to evaluate the first atom of the list, the
word ‘`this`’. It is this action that generated the error message
‘`void-function this`’.

The message contains the words ‘`void-function`’ and ‘`this`’.

The word ‘`function`’ was mentioned once before. It is a very
important word. For our purposes, we can define it by saying that a
*function* is a set of instructions to the computer that tell the
computer to do something.

Now we can begin to understand the error message: ‘`void-function
this`’. The function (that is, the word ‘`this`’) does not have a
definition of any set of instructions for the computer to carry out.

The slightly odd word, ‘`void-function`’, is designed to cover the
way Emacs Lisp is implemented, which is that when a symbol does not
have a function definition attached to it, the place that should
contain the instructions is void.

On the other hand, since we were able to add 2 plus 2 successfully, by
evaluating `(+ 2 2)`, we can infer that the symbol `+` must
have a set of instructions for the computer to obey and those
instructions must be to add the numbers that follow the `+`.

It is possible to prevent Emacs entering the debugger in cases like
this. We do not explain how to do that here, but we will mention what
the result looks like, because you may encounter a similar situation
if there is a bug in some Emacs code that you are using. In such
cases, you will see only one line of error message; it will appear in
the echo area and look like this:

```
Symbol's function definition is void: this
```

The message goes away as soon as you type a key, even just to
move the cursor.

We know the meaning of the word ‘`Symbol`’. It refers to the first
atom of the list, the word ‘`this`’. The word ‘`function`’
refers to the instructions that tell the computer what to do.
(Technically, the symbol tells the computer where to find the
instructions, but this is a complication we can ignore for the
moment.)

The error message can be understood: ‘`Symbol's function
definition is void: this`’. The symbol (that is, the word
‘`this`’) lacks instructions for the computer to carry out.

---

Next: [The Lisp Interpreter](#Lisp-Interpreter), Previous: [Generate an Error Message](#Making-Errors), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.4 Symbol Names and Function Definitions

We can articulate another characteristic of Lisp based on what we have
discussed so far—an important characteristic: a symbol, like
`+`, is not itself the set of instructions for the computer to
carry out. Instead, the symbol is used, perhaps temporarily, as a way
of locating the definition or set of instructions. What we see is the
name through which the instructions can be found. Names of people
work the same way. I can be referred to as ‘`Bob`’; however, I am
not the letters ‘`B`’, ‘`o`’, ‘`b`’ but am, or was, the
consciousness consistently associated with a particular life-form.
The name is not me, but it can be used to refer to me.

In Lisp, one set of instructions can be attached to several names.
For example, the computer instructions for adding numbers can be
linked to the symbol `plus` as well as to the symbol `+`
(and are in some dialects of Lisp). Among humans, I can be referred
to as ‘`Robert`’ as well as ‘`Bob`’ and by other words as well.

On the other hand, a symbol can have only one function definition
attached to it at a time. Otherwise, the computer would be confused as
to which definition to use. If this were the case among people, only
one person in the world could be named ‘`Bob`’. However, the function
definition to which the name refers can be changed readily.
(See [Install a Function Definition](#Install).)

Since Emacs Lisp is large, it is customary to name symbols in a way
that identifies the part of Emacs to which the function belongs.
Thus, all the names for functions that deal with Texinfo start with
‘`texinfo-`’ and those for functions that deal with reading mail
start with ‘`rmail-`’.

---

Next: [Evaluation](#Evaluation), Previous: [Symbol Names and Function Definitions](#Names-_0026-Definitions), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.5 The Lisp Interpreter

Based on what we have seen, we can now start to figure out what the
Lisp interpreter does when we command it to evaluate a list.
First, it looks to see whether there is a quote before the list; if
there is, the interpreter just gives us the list. On the other
hand, if there is no quote, the interpreter looks at the first element
in the list and sees whether it has a function definition. If it does,
the interpreter carries out the instructions in the function definition.
Otherwise, the interpreter prints an error message.

This is how Lisp works. Simple. There are added complications which we
will get to in a minute, but these are the fundamentals. Of course, to
write Lisp programs, you need to know how to write function definitions
and attach them to names, and how to do this without confusing either
yourself or the computer.

* [Complications](#Complications)
* [Byte Compiling](#Byte-Compiling)

---

Next: [Byte Compiling](#Byte-Compiling), Up: [The Lisp Interpreter](#Lisp-Interpreter)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### Complications

Now, for the first complication. In addition to lists, the Lisp
interpreter can evaluate a symbol that is not quoted and does not have
parentheses around it. The Lisp interpreter will attempt to determine
the symbol’s value as a *variable*. This situation is described
in the section on variables. (See [Variables](#Variables).)

The second complication occurs because some functions are unusual and
do not work in the usual manner. Those that don’t are called
*special forms*. They are used for special jobs, like defining a
function, and there are not many of them. In the next few chapters,
you will be introduced to several of the more important special forms.

As well as special forms, there are also *macros*. A macro
is a construct defined in Lisp, which differs from a function in that it
translates a Lisp expression into another expression that is to be
evaluated in place of the original expression. (See [Lisp macro](#Lisp-macro).)

For the purposes of this introduction, you do not need to worry too much
about whether something is a special form, macro, or ordinary function.
For example, `if` is a special form (see [The `if` Special Form](#if)), but `when`
is a macro (see [Lisp macro](#Lisp-macro)). In earlier versions of Emacs,
`defun` was a special form, but now it is a macro (see [The `defun` Macro](#defun)).
It still behaves in the same way.

The final complication is this: if the function that the
Lisp interpreter is looking at is not a special form, and if it is part
of a list, the Lisp interpreter looks to see whether the list has a list
inside of it. If there is an inner list, the Lisp interpreter first
figures out what it should do with the inside list, and then it works on
the outside list. If there is yet another list embedded inside the
inner list, it works on that one first, and so on. It always works on
the innermost list first. The interpreter works on the innermost list
first, to evaluate the result of that list. The result may be
used by the enclosing expression.

Otherwise, the interpreter works left to right, from one expression to
the next.

---

Previous: [Complications](#Complications), Up: [The Lisp Interpreter](#Lisp-Interpreter)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.5.1 Byte Compiling

One other aspect of interpreting: the Lisp interpreter is able to
interpret two kinds of entity: humanly readable code, on which we will
focus exclusively, and specially processed code, called *byte
compiled* code, which is not humanly readable. Byte compiled code
runs faster than humanly readable code.

You can transform humanly readable code into byte compiled code by
running one of the compile commands such as `byte-compile-file`.
Byte compiled code is usually stored in a file that ends with a
`.elc` extension rather than a `.el` extension. You will
see both kinds of file in the `emacs/lisp` directory; the files
to read are those with `.el` extensions.

As a practical matter, for most things you might do to customize or
extend Emacs, you do not need to byte compile; and I will not discuss
the topic here. See [Byte Compilation](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Byte-Compilation) in The GNU Emacs Lisp Reference Manual, for a full description of byte
compilation.

---

Next: [Variables](#Variables), Previous: [The Lisp Interpreter](#Lisp-Interpreter), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.6 Evaluation

When the Lisp interpreter works on an expression, the term for the
activity is called *evaluation*. We say that the interpreter
“evaluates the expression”. I’ve used this term several times before.
The word comes from its use in everyday language, “to ascertain the
value or amount of; to appraise”, according to Webster’s New
Collegiate Dictionary.

* [How the Lisp Interpreter Acts](#How-the-Interpreter-Acts)
* [Evaluating Inner Lists](#Evaluating-Inner-Lists)

---

Next: [Evaluating Inner Lists](#Evaluating-Inner-Lists), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### How the Lisp Interpreter Acts

After evaluating an expression, the Lisp interpreter will most likely
*return* the value that the computer produces by carrying out the
instructions it found in the function definition, or perhaps it will
give up on that function and produce an error message. (The interpreter
may also find itself tossed, so to speak, to a different function or it
may attempt to repeat continually what it is doing for ever and ever in
an infinite loop. These actions are less common; and
we can ignore them.) Most frequently, the interpreter returns a value.

At the same time the interpreter returns a value, it may do something
else as well, such as move a cursor or copy a file; this other kind of
action is called a *side effect*. Actions that we humans think are
important, such as printing results, are often side effects to the
Lisp interpreter. It is fairly easy to learn to use side effects.

In summary, evaluating a symbolic expression most commonly causes the
Lisp interpreter to return a value and perhaps carry out a side effect;
or else produce an error.

---

Previous: [How the Lisp Interpreter Acts](#How-the-Interpreter-Acts), Up: [Evaluation](#Evaluation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.6.1 Evaluating Inner Lists

If evaluation applies to a list that is inside another list, the outer
list may use the value returned by the first evaluation as information
when the outer list is evaluated. This explains why inner expressions
are evaluated first: the values they return are used by the outer
expressions.

We can investigate this process by evaluating another addition example.
Place your cursor after the following expression and type `C-x C-e`:

```
(+ 2 (+ 3 3))
```

The number 8 will appear in the echo area.

What happens is that the Lisp interpreter first evaluates the inner
expression, `(+ 3 3)`, for which the value 6 is returned; then it
evaluates the outer expression as if it were written `(+ 2 6)`, which
returns the value 8. Since there are no more enclosing expressions to
evaluate, the interpreter prints that value in the echo area.

Now it is easy to understand the name of the command invoked by the
keystrokes `C-x C-e`: the name is `eval-last-sexp`. The
letters `sexp` are an abbreviation for “symbolic expression”, and
`eval` is an abbreviation for “evaluate”. The command
evaluates the last symbolic expression.

As an experiment, you can try evaluating the expression by putting the
cursor at the beginning of the next line immediately following the
expression, or inside the expression.

Here is another copy of the expression:

```
(+ 2 (+ 3 3))
```

If you place the cursor at the beginning of the blank line that
immediately follows the expression and type `C-x C-e`, you will
still get the value 8 printed in the echo area. Now try putting the
cursor inside the expression. If you put it right after the next to
last parenthesis (so it appears to sit on top of the last parenthesis),
you will get a 6 printed in the echo area! This is because the command
evaluates the expression `(+ 3 3)`.

Now put the cursor immediately after a number. Type `C-x C-e` and
you will get the number itself. In Lisp, if you evaluate a number, you
get the number itself—this is how numbers differ from symbols. If you
evaluate a list starting with a symbol like `+`, you will get a
value returned that is the result of the computer carrying out the
instructions in the function definition attached to that name. If a
symbol by itself is evaluated, something different happens, as we will
see in the next section.

---

Next: [Arguments](#Arguments), Previous: [Evaluation](#Evaluation), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.7 Variables

In Emacs Lisp, a symbol can have a value attached to it just as it can
have a function definition attached to it. The two are different.
The function definition is a set of instructions that a computer will
obey. A value, on the other hand, is something, such as number or a
name, that can vary (which is why such a symbol is called a variable).
The value of a symbol can be any expression in Lisp, such as a symbol,
number, list, or string. A symbol that has a value is often called a
*variable*.

A symbol can have both a function definition and a value attached to
it at the same time. Or it can have just one or the other.
The two are separate. This is somewhat similar
to the way the name Cambridge can refer to the city in Massachusetts
and have some information attached to the name as well, such as
“great programming center”.

Another way to think about this is to imagine a symbol as being a chest
of drawers. The function definition is put in one drawer, the value in
another, and so on. What is put in the drawer holding the value can be
changed without affecting the contents of the drawer holding the
function definition, and vice versa.

* [`fill-column`, an Example Variable](#fill_002dcolumn-Example)
* [Error Message for a Symbol Without a Function](#Void-Function)
* [Error Message for a Symbol Without a Value](#Void-Variable)

---

Next: [Error Message for a Symbol Without a Function](#Void-Function), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### `fill-column`, an Example Variable

The variable `fill-column` illustrates a symbol with a value
attached to it: in every GNU Emacs buffer, this symbol is set to some
value, usually 72 or 70, but sometimes to some other value. To find the
value of this symbol, evaluate it by itself. If you are reading this in
Info inside of GNU Emacs, you can do this by putting the cursor after
the symbol and typing `C-x C-e`:

```
fill-column
```

After I typed `C-x C-e`, Emacs printed the number 72 in my echo
area. This is the value for which `fill-column` is set for me as I
write this. It may be different for you in your Info buffer. Notice
that the value returned as a variable is printed in exactly the same way
as the value returned by a function carrying out its instructions. From
the point of view of the Lisp interpreter, a value returned is a value
returned. What kind of expression it came from ceases to matter once
the value is known.

A symbol can have any value attached to it or, to use the jargon, we can
*bind* the variable to a value: to a number, such as 72; to a
string, `"such as this"`; to a list, such as `(spruce pine
oak)`; we can even bind a variable to a function definition.

A symbol can be bound to a value in several ways. See [Setting the Value of a Variable](#setq), for information about one way to do
this.

---

Next: [Error Message for a Symbol Without a Value](#Void-Variable), Previous: [`fill-column`, an Example Variable](#fill_002dcolumn-Example), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.7.1 Error Message for a Symbol Without a Function

When we evaluated `fill-column` to find its value as a variable,
we did not place parentheses around the word. This is because we did
not intend to use it as a function name.

If `fill-column` were the first or only element of a list, the
Lisp interpreter would attempt to find the function definition
attached to it. But `fill-column` has no function definition.
Try evaluating this:

```
(fill-column)
```

You will create a `*Backtrace*` buffer that says:

```
---------- Buffer: *Backtrace* ----------
Debugger entered--Lisp error: (void-function fill-column)
  (fill-column)
  eval((fill-column) nil)
  elisp--eval-last-sexp(nil)
  eval-last-sexp(nil)
  funcall-interactively(eval-last-sexp nil)
  call-interactively(eval-last-sexp nil nil)
  command-execute(eval-last-sexp)
---------- Buffer: *Backtrace* ----------
```

(Remember, to quit the debugger and make the debugger window go away,
type `q` in the `*Backtrace*` buffer.)

---

Previous: [Error Message for a Symbol Without a Function](#Void-Function), Up: [Variables](#Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.7.2 Error Message for a Symbol Without a Value

If you attempt to evaluate a symbol that does not have a value bound to
it, you will receive an error message. You can see this by
experimenting with our 2 plus 2 addition. In the following expression,
put your cursor right after the `+`, before the first number 2,
type `C-x C-e`:

```
(+ 2 2)
```

In GNU Emacs 22, you will create a `*Backtrace*` buffer that
says:

```
---------- Buffer: *Backtrace* ----------
Debugger entered--Lisp error: (void-variable +)
  eval(+)
  elisp--eval-last-sexp(nil)
  eval-last-sexp(nil)
  funcall-interactively(eval-last-sexp nil)
  call-interactively(eval-last-sexp nil nil)
  command-execute(eval-last-sexp)
---------- Buffer: *Backtrace* ----------
```

(Again, you can quit the debugger by
typing `q` in the `*Backtrace*` buffer.)

This backtrace is different from the very first error message we saw,
which said, ‘`Debugger entered--Lisp error: (void-function this)`’.
In this case, the function does not have a value as a variable; while
in the other error message, the function (the word ‘`this`’) did not
have a definition.

In this experiment with the `+`, what we did was cause the Lisp
interpreter to evaluate the `+` and look for the value of the
variable instead of the function definition. We did this by placing the
cursor right after the symbol rather than after the parenthesis of the
enclosing list as we did before. As a consequence, the Lisp interpreter
evaluated the preceding s-expression, which in this case was
`+` by itself.

Since `+` does not have a value bound to it, just the function
definition, the error message reported that the symbol’s value as a
variable was void.

---

Next: [Setting the Value of a Variable](#setq), Previous: [Variables](#Variables), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.8 Arguments

To see how information is passed to functions, let’s look again at
our old standby, the addition of two plus two. In Lisp, this is written
as follows:

```
(+ 2 2)
```

If you evaluate this expression, the number 4 will appear in your echo
area. What the Lisp interpreter does is add the numbers that follow
the `+`.

The numbers added by `+` are called the *arguments* of the
function `+`. These numbers are the information that is given to
or *passed* to the function.

The word “argument” comes from the way it is used in mathematics and
does not refer to a disputation between two people; instead it refers to
the information presented to the function, in this case, to the
`+`. In Lisp, the arguments to a function are the atoms or lists
that follow the function. The values returned by the evaluation of
these atoms or lists are passed to the function. Different functions
require different numbers of arguments; some functions require none at
all.[3](#FOOT3)

* [Arguments’ Data Types](#Data-types)
* [An Argument as the Value of a Variable or List](#Args-as-Variable-or-List)
* [Variable Number of Arguments](#Variable-Number-of-Arguments)
* [Using the Wrong Type Object as an Argument](#Wrong-Type-of-Argument)
* [The `message` Function](#message)

---

Next: [An Argument as the Value of a Variable or List](#Args-as-Variable-or-List), Up: [Arguments](#Arguments)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.8.1 Arguments’ Data Types

The type of data that should be passed to a function depends on what
kind of information it uses. The arguments to a function such as
`+` must have values that are numbers, since `+` adds numbers.
Other functions use different kinds of data for their arguments.

For example, the `concat` function links together or unites two or
more strings of text to produce a string. The arguments are strings.
Concatenating the two character strings `abc`, `def` produces
the single string `abcdef`. This can be seen by evaluating the
following:

```
(concat "abc" "def")
```

The value produced by evaluating this expression is `"abcdef"`.

A function such as `substring` uses both a string and numbers as
arguments. The function returns a part of the string, a *substring* of
the first argument. This function takes three arguments. Its first
argument is the string of characters, the second and third arguments
are numbers that indicate the beginning (inclusive) and end
(exclusive) of the substring. The numbers are a count of the number
of characters (including spaces and punctuation) from the beginning of
the string. Note that the characters in a string are numbered from
zero, not one.

For example, if you evaluate the following:

```
(substring "The quick brown fox jumped." 16 19)
```

you will see `"fox"` appear in the echo area. The arguments are the
string and the two numbers.

Note that the string passed to `substring` is a single atom even
though it is made up of several words separated by spaces. Lisp counts
everything between the two quotation marks as part of the string,
including the spaces. You can think of the `substring` function as
a kind of atom smasher since it takes an otherwise indivisible atom
and extracts a part. However, `substring` is only able to extract
a substring from an argument that is a string, not from another type of
atom such as a number or symbol.

---

Next: [Variable Number of Arguments](#Variable-Number-of-Arguments), Previous: [Arguments’ Data Types](#Data-types), Up: [Arguments](#Arguments)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.8.2 An Argument as the Value of a Variable or List

An argument can be a symbol that returns a value when it is evaluated.
For example, when the symbol `fill-column` by itself is evaluated,
it returns a number. This number can be used in an addition.

Position the cursor after the following expression and type `C-x
C-e`:

```
(+ 2 fill-column)
```

The value will be a number two more than what you get by evaluating
`fill-column` alone. For me, this is 74, because my value of
`fill-column` is 72.

As we have just seen, an argument can be a symbol that returns a value
when evaluated. In addition, an argument can be a list that returns a
value when it is evaluated. For example, in the following expression,
the arguments to the function `concat` are the strings
`"The "` and `" red foxes."` and the list
`(number-to-string (+ 2 fill-column))`.

```
(concat "The " (number-to-string (+ 2 fill-column)) " red foxes.")
```

If you evaluate this expression—and if, as with my Emacs,
`fill-column` evaluates to 72—`"The 74 red foxes."` will
appear in the echo area. (Note that you must put spaces after the
word ‘`The`’ and before the word ‘`red`’ so they will appear in
the final string. The function `number-to-string` converts the
integer that the addition function returns to a string.
`number-to-string` is also known as `int-to-string`.)

---

Next: [Using the Wrong Type Object as an Argument](#Wrong-Type-of-Argument), Previous: [An Argument as the Value of a Variable or List](#Args-as-Variable-or-List), Up: [Arguments](#Arguments)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.8.3 Variable Number of Arguments

Some functions, such as `concat`, `+` or `*`, take any
number of arguments. (The `*` is the symbol for multiplication.)
This can be seen by evaluating each of the following expressions in
the usual way. What you will see in the echo area is printed in this
text after ‘`⇒`’, which you may read as “evaluates to”.

In the first set, the functions have no arguments:

```
(+)       ⇒ 0

(*)       ⇒ 1
```

In this set, the functions have one argument each:

```
(+ 3)     ⇒ 3

(* 3)     ⇒ 3
```

In this set, the functions have three arguments each:

```
(+ 3 4 5) ⇒ 12

(* 3 4 5) ⇒ 60
```

---

Next: [The `message` Function](#message), Previous: [Variable Number of Arguments](#Variable-Number-of-Arguments), Up: [Arguments](#Arguments)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.8.4 Using the Wrong Type Object as an Argument

When a function is passed an argument of the wrong type, the Lisp
interpreter produces an error message. For example, the `+`
function expects the values of its arguments to be numbers. As an
experiment we can pass it the quoted symbol `hello` instead of a
number. Position the cursor after the following expression and type
`C-x C-e`:

```
(+ 2 'hello)
```

When you do this you will generate an error message. What has happened
is that `+` has tried to add the 2 to the value returned by
`'hello`, but the value returned by `'hello` is the symbol
`hello`, not a number. Only numbers can be added. So `+`
could not carry out its addition.

You will create and enter a `*Backtrace*` buffer that says:

```
---------- Buffer: *Backtrace* ----------
Debugger entered--Lisp error:
         (wrong-type-argument number-or-marker-p hello)
  +(2 hello)
  eval((+ 2 'hello) nil)
  elisp--eval-last-sexp(t)
  eval-last-sexp(nil)
  funcall-interactively(eval-print-last-sexp nil)
  call-interactively(eval-print-last-sexp nil nil)
  command-execute(eval-print-last-sexp)
---------- Buffer: *Backtrace* ----------
```

As usual, the error message tries to be helpful and makes sense after you
learn how to read it.[4](#FOOT4)

The first part of the error message is straightforward; it says
‘`wrong type argument`’. Next comes the mysterious jargon word
‘`number-or-marker-p`’. This word is trying to tell you what
kind of argument the `+` expected.

The symbol `number-or-marker-p` says that the Lisp interpreter is
trying to determine whether the information presented it (the value of
the argument) is a number or a marker (a special object representing a
buffer position). What it does is test to see whether the `+` is
being given numbers to add. It also tests to see whether the
argument is something called a marker, which is a specific feature of
Emacs Lisp. (In Emacs, locations in a buffer are recorded as markers.
When the mark is set with the `C-@` or `C-SPC` command,
its position is kept as a marker. The mark can be considered a
number—the number of characters the location is from the beginning
of the buffer.) In Emacs Lisp, `+` can be used to add the
numeric value of marker positions as numbers.

The ‘`p`’ of `number-or-marker-p` is the embodiment of a
practice started in the early days of Lisp programming. The ‘`p`’
stands for *predicate*. In the jargon used by the early Lisp
researchers, a predicate refers to a function to determine whether some
property is true or false. So the ‘`p`’ tells us that
`number-or-marker-p` is the name of a function that determines
whether it is true or false that the argument supplied is a number or
a marker. Other Lisp symbols that end in ‘`p`’ include `zerop`,
a function that tests whether its argument has the value of zero, and
`listp`, a function that tests whether its argument is a list.

Finally, the last part of the error message is the symbol `hello`.
This is the value of the argument that was passed to `+`. If the
addition had been passed the correct type of object, the value passed
would have been a number, such as 37, rather than a symbol like
`hello`. But then you would not have got the error message.

---

Previous: [Using the Wrong Type Object as an Argument](#Wrong-Type-of-Argument), Up: [Arguments](#Arguments)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.8.5 The `message` Function

Like `+`, the `message` function takes a variable number of
arguments. It is used to send messages to the user and is so useful
that we will describe it here.

A message is printed in the echo area. For example, you can print a
message in your echo area by evaluating the following list:

```
(message "This message appears in the echo area!")
```

The whole string between double quotation marks is a single argument
and is printed *in toto*. (Note that in this example, the message
itself will appear in the echo area within double quotes; that is
because you see the value returned by the `message` function. In
most uses of `message` in programs that you write, the text will
be printed in the echo area as a side-effect, without the quotes.
See [`multiply-by-seven` in
detail](#multiply_002dby_002dseven-in-detail), for an example of this.)

However, if there is a ‘`%s`’ in the quoted string of characters, the
`message` function does not print the ‘`%s`’ as such, but looks
to the argument that follows the string. It evaluates the second
argument and prints the value at the location in the string where the
‘`%s`’ is.

You can see this by positioning the cursor after the following
expression and typing `C-x C-e`:

```
(message "The name of this buffer is: %s." (buffer-name))
```

In Info, `"The name of this buffer is: *info*."` will appear in the
echo area. The function `buffer-name` returns the name of the
buffer as a string, which the `message` function inserts in place
of `%s`.

To print a value as an integer, use ‘`%d`’ in the same way as
‘`%s`’. For example, to print a message in the echo area that
states the value of the `fill-column`, evaluate the following:

```
(message "The value of fill-column is %d." fill-column)
```

On my system, when I evaluate this list, `"The value of
fill-column is 72."` appears in my echo area[5](#FOOT5).

If there is more than one ‘`%s`’ in the quoted string, the value of
the first argument following the quoted string is printed at the
location of the first ‘`%s`’ and the value of the second argument is
printed at the location of the second ‘`%s`’, and so on.

For example, if you evaluate the following,

```
(message "There are %d %s in the office!"
         (- fill-column 14) "pink elephants")
```

a rather whimsical message will appear in your echo area. On my system
it says, `"There are 58 pink elephants in the office!"`.

The expression `(- fill-column 14)` is evaluated and the resulting
number is inserted in place of the ‘`%d`’; and the string in double
quotes, `"pink elephants"`, is treated as a single argument and
inserted in place of the ‘`%s`’. (That is to say, a string between
double quotes evaluates to itself, like a number.)

Finally, here is a somewhat complex example that not only illustrates
the computation of a number, but also shows how you can use an
expression within an expression to generate the text that is substituted
for ‘`%s`’:

```
(message "He saw %d %s"
         (- fill-column 32)
         (concat "red "
                 (substring
                  "The quick brown foxes jumped." 16 21)
                 " leaping."))
```

In this example, `message` has three arguments: the string,
`"He saw %d %s"`, the expression, `(- fill-column 32)`, and
the expression beginning with the function `concat`. The value
resulting from the evaluation of `(- fill-column 32)` is inserted
in place of the ‘`%d`’; and the value returned by the expression
beginning with `concat` is inserted in place of the ‘`%s`’.

When your fill column is 70 and you evaluate the expression, the
message `"He saw 38 red foxes leaping."` appears in your echo
area.

---

Next: [Summary](#Summary), Previous: [Arguments](#Arguments), Up: [List Processing](#List-Processing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
