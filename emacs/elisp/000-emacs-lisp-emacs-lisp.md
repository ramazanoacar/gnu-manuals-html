# Emacs Lisp [¶](#Emacs-Lisp)

The GNU Emacs website is at
[https://www.gnu.org/software/emacs/](/software/emacs/).  
For information on using Emacs, refer to the
[Emacs Manual](/software/emacs/manual/emacs.html).  
To view this manual in other formats, click
[here](/software/emacs/manual/elisp.html).

This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 31.1.50.

Copyright © 1990–1996, 1998–2026 Free Software Foundation,
Inc.

> Permission is granted to copy, distribute and/or modify this document
> under the terms of the GNU Free Documentation License, Version 1.3 or
> any later version published by the Free Software Foundation; with the
> Invariant Sections being “GNU General Public License,” with the
> Front-Cover Texts being “A GNU Manual,” and with the Back-Cover
> Texts as in (a) below. A copy of the license is included in the
> section entitled “GNU Free Documentation License.”
>
> (a) The FSF’s Back-Cover Text is: “You have the freedom to copy and
> modify this GNU manual. Buying copies from the FSF supports it in
> developing GNU and promoting software freedom.”## Short Table of Contents

* [1 Introduction](#toc-Introduction-1)
* [2 Lisp Data Types](#toc-Lisp-Data-Types-1)
* [3 Numbers](#toc-Numbers-1)
* [4 Strings and Characters](#toc-Strings-and-Characters-1)
* [5 Lists](#toc-Lists-1)
* [6 Sequences, Arrays, and Vectors](#toc-Sequences_002c-Arrays_002c-and-Vectors)
* [7 Records](#toc-Records-1)
* [8 Hash Tables](#toc-Hash-Tables-1)
* [9 Symbols](#toc-Symbols-1)
* [10 Evaluation](#toc-Evaluation-1)
* [11 Control Structures](#toc-Control-Structures-1)
* [12 Variables](#toc-Variables-1)
* [13 Functions](#toc-Functions-1)
* [14 Macros](#toc-Macros-1)
* [15 Customization Settings](#toc-Customization-Settings)
* [16 Loading](#toc-Loading-1)
* [17 Byte Compilation](#toc-Byte-Compilation-1)
* [18 Compilation of Lisp to Native Code](#toc-Compilation-of-Lisp-to-Native-Code)
* [19 Debugging Lisp Programs](#toc-Debugging-Lisp-Programs)
* [20 Reading and Printing Lisp Objects](#toc-Reading-and-Printing-Lisp-Objects)
* [21 Minibuffers](#toc-Minibuffers-1)
* [22 Command Loop](#toc-Command-Loop-1)
* [23 Keymaps](#toc-Keymaps-1)
* [24 Major and Minor Modes](#toc-Major-and-Minor-Modes)
* [25 Documentation](#toc-Documentation-1)
* [26 Files](#toc-Files-1)
* [27 Backups and Auto-Saving](#toc-Backups-and-Auto_002dSaving-1)
* [28 Buffers](#toc-Buffers-1)
* [29 Windows](#toc-Windows-1)
* [30 Frames](#toc-Frames-1)
* [31 Positions](#toc-Positions-1)
* [32 Markers](#toc-Markers-1)
* [33 Text](#toc-Text-1)
* [34 Non-ASCII Characters](#toc-Non_002dASCII-Characters-1)
* [35 Searching and Matching](#toc-Searching-and-Matching-1)
* [36 Syntax Tables](#toc-Syntax-Tables-1)
* [37 Parsing Expression Grammars](#toc-Parsing-Expression-Grammars-1)
* [38 Parsing Program Source](#toc-Parsing-Program-Source-1)
* [39 Abbrevs and Abbrev Expansion](#toc-Abbrevs-and-Abbrev-Expansion)
* [40 Threads](#toc-Threads-1)
* [41 Processes](#toc-Processes-1)
* [42 Emacs Display](#toc-Emacs-Display)
* [43 Operating System Interface](#toc-Operating-System-Interface)
* [44 Preparing Lisp code for distribution](#toc-Preparing-Lisp-code-for-distribution)
* [Appendix A Emacs 30 Antinews](#toc-Emacs-30-Antinews)
* [Appendix B GNU Free Documentation License](#toc-GNU-Free-Documentation-License-1)
* [Appendix C GNU General Public License](#toc-GNU-General-Public-License)
* [Appendix D Tips and Conventions](#toc-Tips-and-Conventions)
* [Appendix E GNU Emacs Internals](#toc-GNU-Emacs-Internals-1)
* [Appendix F Standard Errors](#toc-Standard-Errors-1)
* [Appendix G Standard Keymaps](#toc-Standard-Keymaps-1)
* [Appendix H Standard Hooks](#toc-Standard-Hooks-1)
* [Index](#toc-Index-1)## Table of Contents

* [1 Introduction](#Introduction)
  + [1.1 Caveats](#Caveats)
  + [1.2 Lisp History](#Lisp-History)
  + [1.3 Conventions](#Conventions)
    - [1.3.1 Some Terms](#Some-Terms)
    - [1.3.2 `nil` and `t`](#nil-and-t)
    - [1.3.3 Evaluation Notation](#Evaluation-Notation)
    - [1.3.4 Printing Notation](#Printing-Notation)
    - [1.3.5 Error Messages](#Error-Messages)
    - [1.3.6 Buffer Text Notation](#Buffer-Text-Notation)
    - [1.3.7 Format of Descriptions](#Format-of-Descriptions)
      * [1.3.7.1 A Sample Function Description](#A-Sample-Function-Description)
      * [1.3.7.2 A Sample Variable Description](#A-Sample-Variable-Description)
  + [1.4 Version Information](#Version-Info)
  + [1.5 Acknowledgments](#Acknowledgments)
* [2 Lisp Data Types](#Lisp-Data-Types)
  + [2.1 Printed Representation and Read Syntax](#Printed-Representation)
  + [2.2 Special Read Syntax](#Special-Read-Syntax)
  + [2.3 Comments](#Comments)
  + [2.4 Programming Types](#Programming-Types)
    - [2.4.1 Integer Type](#Integer-Type)
    - [2.4.2 Floating-Point Type](#Floating_002dPoint-Type)
    - [2.4.3 Character Type](#Character-Type)
      * [2.4.3.1 Basic Char Syntax](#Basic-Char-Syntax)
      * [2.4.3.2 General Escape Syntax](#General-Escape-Syntax)
      * [2.4.3.3 Control-Character Syntax](#Ctl_002dChar-Syntax)
      * [2.4.3.4 Meta-Character Syntax](#Meta_002dChar-Syntax)
      * [2.4.3.5 Other Character Modifier Bits](#Other-Char-Bits)
    - [2.4.4 Symbol Type](#Symbol-Type)
    - [2.4.5 Sequence Types](#Sequence-Type)
    - [2.4.6 Cons Cell and List Types](#Cons-Cell-Type)
      * [2.4.6.1 Drawing Lists as Box Diagrams](#Box-Diagrams)
      * [2.4.6.2 Dotted Pair Notation](#Dotted-Pair-Notation)
      * [2.4.6.3 Association List Type](#Association-List-Type)
    - [2.4.7 Array Type](#Array-Type)
    - [2.4.8 String Type](#String-Type)
      * [2.4.8.1 Syntax for Strings](#Syntax-for-Strings)
      * [2.4.8.2 Non-ASCII Characters in Strings](#Non_002dASCII-in-Strings)
      * [2.4.8.3 Nonprinting Characters in Strings](#Nonprinting-Characters)
      * [2.4.8.4 Text Properties in Strings](#Text-Props-and-Strings)
    - [2.4.9 Vector Type](#Vector-Type)
    - [2.4.10 Char-Table Type](#Char_002dTable-Type)
    - [2.4.11 Bool-Vector Type](#Bool_002dVector-Type)
    - [2.4.12 Hash Table Type](#Hash-Table-Type)
    - [2.4.13 Function Type](#Function-Type)
    - [2.4.14 Macro Type](#Macro-Type)
    - [2.4.15 Primitive Function Type](#Primitive-Function-Type)
    - [2.4.16 Closure Function Type](#Closure-Type)
    - [2.4.17 Record Type](#Record-Type)
    - [2.4.18 Type Descriptors](#Type-Descriptors)
    - [2.4.19 Type Specifiers](#Type-Specifiers)
    - [2.4.20 Autoload Type](#Autoload-Type)
    - [2.4.21 Finalizer Type](#Finalizer-Type)
  + [2.5 Editing Types](#Editing-Types)
    - [2.5.1 Buffer Type](#Buffer-Type)
    - [2.5.2 Marker Type](#Marker-Type)
    - [2.5.3 Window Type](#Window-Type)
    - [2.5.4 Frame Type](#Frame-Type)
    - [2.5.5 Terminal Type](#Terminal-Type)
    - [2.5.6 Window Configuration Type](#Window-Configuration-Type)
    - [2.5.7 Frame Configuration Type](#Frame-Configuration-Type)
    - [2.5.8 Process Type](#Process-Type)
    - [2.5.9 Thread Type](#Thread-Type)
    - [2.5.10 Mutex Type](#Mutex-Type)
    - [2.5.11 Condition Variable Type](#Condition-Variable-Type)
    - [2.5.12 Stream Type](#Stream-Type)
    - [2.5.13 Keymap Type](#Keymap-Type)
    - [2.5.14 Overlay Type](#Overlay-Type)
    - [2.5.15 Font Type](#Font-Type)
    - [2.5.16 Xwidget Type](#Xwidget-Type)
  + [2.6 Read Syntax for Circular Objects](#Circular-Objects)
  + [2.7 Type Predicates](#Type-Predicates)
  + [2.8 Equality Predicates](#Equality-Predicates)
  + [2.9 Mutability](#Mutability)
  + [2.10 Type Hierarchy of Emacs Lisp Objects](#Type-Hierarchy)
* [3 Numbers](#Numbers)
  + [3.1 Integer Basics](#Integer-Basics)
  + [3.2 Floating-Point Basics](#Float-Basics)
  + [3.3 Type Predicates for Numbers](#Predicates-on-Numbers)
  + [3.4 Comparison of Numbers](#Comparison-of-Numbers)
  + [3.5 Numeric Conversions](#Numeric-Conversions)
  + [3.6 Arithmetic Operations](#Arithmetic-Operations)
  + [3.7 Rounding Operations](#Rounding-Operations)
  + [3.8 Bitwise Operations on Integers](#Bitwise-Operations)
  + [3.9 Standard Mathematical Functions](#Math-Functions)
  + [3.10 Random Numbers](#Random-Numbers)
* [4 Strings and Characters](#Strings-and-Characters)
  + [4.1 String and Character Basics](#String-Basics)
  + [4.2 Predicates for Strings](#Predicates-for-Strings)
  + [4.3 Creating Strings](#Creating-Strings)
  + [4.4 Modifying Strings](#Modifying-Strings)
  + [4.5 Comparison of Characters and Strings](#Text-Comparison)
  + [4.6 Conversion of Characters and Strings](#String-Conversion)
  + [4.7 Formatting Strings](#Formatting-Strings)
  + [4.8 Custom Format Strings](#Custom-Format-Strings)
  + [4.9 Case Conversion in Lisp](#Case-Conversion)
  + [4.10 The Case Table](#Case-Tables)
* [5 Lists](#Lists)
  + [5.1 Lists and Cons Cells](#Cons-Cells)
  + [5.2 Predicates on Lists](#List_002drelated-Predicates)
  + [5.3 Accessing Elements of Lists](#List-Elements)
  + [5.4 Building Cons Cells and Lists](#Building-Lists)
  + [5.5 Modifying List Variables](#List-Variables)
  + [5.6 Modifying Existing List Structure](#Modifying-Lists)
    - [5.6.1 Altering List Elements with `setcar`](#Setcar)
    - [5.6.2 Altering the CDR of a List](#Setcdr)
    - [5.6.3 Functions that Rearrange Lists](#Rearrangement)
  + [5.7 Using Lists as Sets](#Sets-And-Lists)
  + [5.8 Association Lists](#Association-Lists)
  + [5.9 Property Lists](#Property-Lists)
    - [5.9.1 Property Lists and Association Lists](#Plists-and-Alists)
    - [5.9.2 Property Lists Outside Symbols](#Plist-Access)
* [6 Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)
  + [6.1 Sequences](#Sequence-Functions)
  + [6.2 Arrays](#Arrays)
  + [6.3 Functions that Operate on Arrays](#Array-Functions)
  + [6.4 Vectors](#Vectors)
  + [6.5 Functions for Vectors](#Vector-Functions)
  + [6.6 Char-Tables](#Char_002dTables)
  + [6.7 Bool-vectors](#Bool_002dVectors)
  + [6.8 Managing a Fixed-Size Ring of Objects](#Rings)
* [7 Records](#Records)
  + [7.1 Record Functions](#Record-Functions)
  + [7.2 Backward Compatibility](#Backward-Compatibility)
* [8 Hash Tables](#Hash-Tables)
  + [8.1 Creating Hash Tables](#Creating-Hash)
  + [8.2 Hash Table Access](#Hash-Access)
  + [8.3 Defining Hash Comparisons](#Defining-Hash)
  + [8.4 Other Hash Table Functions](#Other-Hash)
* [9 Symbols](#Symbols)
  + [9.1 Symbol Components](#Symbol-Components)
  + [9.2 Defining Symbols](#Definitions)
  + [9.3 Creating and Interning Symbols](#Creating-Symbols)
  + [9.4 Symbol Properties](#Symbol-Properties)
    - [9.4.1 Accessing Symbol Properties](#Symbol-Plists)
    - [9.4.2 Standard Symbol Properties](#Standard-Properties)
  + [9.5 Shorthands](#Shorthands)
    - [9.5.1 Exceptions](#Exceptions)
  + [9.6 Symbols with Position](#Symbols-with-Position)
* [10 Evaluation](#Evaluation)
  + [10.1 Introduction to Evaluation](#Intro-Eval)
  + [10.2 Kinds of Forms](#Forms)
    - [10.2.1 Self-Evaluating Forms](#Self_002dEvaluating-Forms)
    - [10.2.2 Symbol Forms](#Symbol-Forms)
    - [10.2.3 Classification of List Forms](#Classifying-Lists)
    - [10.2.4 Symbol Function Indirection](#Function-Indirection)
    - [10.2.5 Evaluation of Function Forms](#Function-Forms)
    - [10.2.6 Lisp Macro Evaluation](#Macro-Forms)
    - [10.2.7 Special Forms](#Special-Forms)
    - [10.2.8 Autoloading](#Autoloading)
  + [10.3 Quoting](#Quoting)
  + [10.4 Backquote](#Backquote)
  + [10.5 Eval](#Eval)
  + [10.6 Deferred and Lazy Evaluation](#Deferred-Eval)
* [11 Control Structures](#Control-Structures)
  + [11.1 Sequencing](#Sequencing)
  + [11.2 Conditionals](#Conditionals)
  + [11.3 Constructs for Combining Conditions](#Combining-Conditions)
  + [11.4 Pattern-Matching Conditional](#Pattern_002dMatching-Conditional)
    - [11.4.1 The `pcase` macro](#pcase-Macro)
    - [11.4.2 Extending `pcase`](#Extending-pcase)
    - [11.4.3 Backquote-Style Patterns](#Backquote-Patterns)
    - [11.4.4 Destructuring with `pcase` Patterns](#Destructuring-with-pcase-Patterns)
    - [11.4.5 The `cond*` macro](#cond_002a-Macro)
  + [11.5 Iteration](#Iteration)
  + [11.6 Generators](#Generators)
  + [11.7 Nonlocal Exits](#Nonlocal-Exits)
    - [11.7.1 Explicit Nonlocal Exits: `catch` and `throw`](#Catch-and-Throw)
    - [11.7.2 Examples of `catch` and `throw`](#Examples-of-Catch)
    - [11.7.3 Errors](#Errors)
      * [11.7.3.1 How to Signal an Error](#Signaling-Errors)
      * [11.7.3.2 How Emacs Processes Errors](#Processing-of-Errors)
      * [11.7.3.3 Writing Code to Handle Errors](#Handling-Errors)
      * [11.7.3.4 Error Symbols and Condition Names](#Error-Symbols)
    - [11.7.4 Cleaning Up from Nonlocal Exits](#Cleanups)
  + [11.8 Conditional Compilation](#Conditional-Compilation)
* [12 Variables](#Variables)
  + [12.1 Global Variables](#Global-Variables)
  + [12.2 Variables that Never Change](#Constant-Variables)
  + [12.3 Local Variables](#Local-Variables)
  + [12.4 When a Variable is Void](#Void-Variables)
  + [12.5 Defining Global Variables](#Defining-Variables)
  + [12.6 Tips for Defining Variables Robustly](#Tips-for-Defining)
  + [12.7 Accessing Variable Values](#Accessing-Variables)
  + [12.8 Setting Variable Values](#Setting-Variables)
  + [12.9 Running a function when a variable is changed.](#Watching-Variables)
    - [12.9.1 Limitations](#Limitations)
  + [12.10 Scoping Rules for Variable Bindings](#Variable-Scoping)
    - [12.10.1 Lexical Binding](#Lexical-Binding)
    - [12.10.2 Dynamic Binding](#Dynamic-Binding)
    - [12.10.3 Proper Use of Dynamic Binding](#Dynamic-Binding-Tips)
    - [12.10.4 Selecting Lisp Dialect](#Selecting-Lisp-Dialect)
    - [12.10.5 Converting to Lexical Binding](#Converting-to-Lexical-Binding)
  + [12.11 Buffer-Local Variables](#Buffer_002dLocal-Variables)
    - [12.11.1 Introduction to Buffer-Local Variables](#Intro-to-Buffer_002dLocal)
    - [12.11.2 Creating and Deleting Buffer-Local Bindings](#Creating-Buffer_002dLocal)
    - [12.11.3 The Default Value of a Buffer-Local Variable](#Default-Value)
  + [12.12 File Local Variables](#File-Local-Variables)
  + [12.13 Directory Local Variables](#Directory-Local-Variables)
  + [12.14 Connection Local Variables](#Connection-Local-Variables)
    - [12.14.1 Connection Local Profiles](#Connection-Local-Profiles)
    - [12.14.2 Applying Connection Local Variables](#Applying-Connection-Local-Variables)
  + [12.15 Variable Aliases](#Variable-Aliases)
  + [12.16 Variables with Restricted Values](#Variables-with-Restricted-Values)
  + [12.17 Generalized Variables](#Generalized-Variables)
    - [12.17.1 The `setf` Macro](#Setting-Generalized-Variables)
    - [12.17.2 Defining new `setf` forms](#Adding-Generalized-Variables)
  + [12.18 Multisession Variables](#Multisession-Variables)
* [13 Functions](#Functions)
  + [13.1 What Is a Function?](#What-Is-a-Function)
  + [13.2 Lambda Expressions](#Lambda-Expressions)
    - [13.2.1 Components of a Lambda Expression](#Lambda-Components)
    - [13.2.2 A Simple Lambda Expression Example](#Simple-Lambda)
    - [13.2.3 Features of Argument Lists](#Argument-List)
    - [13.2.4 Documentation Strings of Functions](#Function-Documentation)
  + [13.3 Naming a Function](#Function-Names)
  + [13.4 Defining Functions](#Defining-Functions)
    - [13.4.1 Finding Definitions](#Finding-Definitions)
  + [13.5 Calling Functions](#Calling-Functions)
  + [13.6 Mapping Functions](#Mapping-Functions)
  + [13.7 Anonymous Functions](#Anonymous-Functions)
  + [13.8 Generic Functions](#Generic-Functions)
  + [13.9 Accessing Function Cell Contents](#Function-Cells)
  + [13.10 Closures](#Closures)
  + [13.11 Open Closures](#OClosures)
  + [13.12 Advising Emacs Lisp Functions](#Advising-Functions)
    - [13.12.1 Primitives to manipulate advices](#Core-Advising-Primitives)
    - [13.12.2 Advising Named Functions](#Advising-Named-Functions)
    - [13.12.3 Ways to compose advice](#Advice-Combinators)
    - [13.12.4 Adapting code using the old defadvice](#Porting-Old-Advice)
    - [13.12.5 Advice and Byte Code](#Advice-and-Byte-Code)
  + [13.13 Declaring Functions Obsolete](#Obsolete-Functions)
  + [13.14 Inline Functions](#Inline-Functions)
  + [13.15 The `declare` Form](#Declare-Form)
  + [13.16 Telling the Compiler that a Function is Defined](#Declaring-Functions)
  + [13.17 Determining whether a Function is Safe to Call](#Function-Safety)
  + [13.18 Other Topics Related to Functions](#Related-Topics)
* [14 Macros](#Macros)
  + [14.1 A Simple Example of a Macro](#Simple-Macro)
  + [14.2 Expansion of a Macro Call](#Expansion)
  + [14.3 Macros and Byte Compilation](#Compiling-Macros)
  + [14.4 Defining Macros](#Defining-Macros)
  + [14.5 Common Problems Using Macros](#Problems-with-Macros)
    - [14.5.1 Wrong Time](#Wrong-Time)
    - [14.5.2 Evaluating Macro Arguments Repeatedly](#Argument-Evaluation)
    - [14.5.3 Local Variables in Macro Expansions](#Surprising-Local-Vars)
    - [14.5.4 Evaluating Macro Arguments in Expansion](#Eval-During-Expansion)
    - [14.5.5 How Many Times is the Macro Expanded?](#Repeated-Expansion)
  + [14.6 Indenting Macros](#Indenting-Macros)
* [15 Customization Settings](#Customization)
  + [15.1 Common Item Keywords](#Common-Keywords)
  + [15.2 Defining Customization Groups](#Group-Definitions)
  + [15.3 Defining Customization Variables](#Variable-Definitions)
  + [15.4 Customization Types](#Customization-Types)
    - [15.4.1 Simple Types](#Simple-Types)
    - [15.4.2 Composite Types](#Composite-Types)
    - [15.4.3 Splicing into Lists](#Splicing-into-Lists)
    - [15.4.4 Type Keywords](#Type-Keywords)
    - [15.4.5 Defining New Types](#Defining-New-Types)
  + [15.5 Applying Customizations](#Applying-Customizations)
  + [15.6 Custom Themes](#Custom-Themes)
* [16 Loading](#Loading)
  + [16.1 How Programs Do Loading](#How-Programs-Do-Loading)
  + [16.2 Load Suffixes](#Load-Suffixes)
  + [16.3 Library Search](#Library-Search)
  + [16.4 Loading Non-ASCII Characters](#Loading-Non_002dASCII)
  + [16.5 Autoload](#Autoload)
    - [16.5.1 Autoload by Prefix](#Autoload-by-Prefix)
    - [16.5.2 When to Use Autoload](#When-to-Autoload)
  + [16.6 Repeated Loading](#Repeated-Loading)
  + [16.7 Features](#Named-Features)
  + [16.8 Which File Defined a Certain Symbol](#Where-Defined)
  + [16.9 Unloading](#Unloading)
  + [16.10 Hooks for Loading](#Hooks-for-Loading)
  + [16.11 Emacs Dynamic Modules](#Dynamic-Modules)
* [17 Byte Compilation](#Byte-Compilation)
  + [17.1 Performance of Byte-Compiled Code](#Speed-of-Byte_002dCode)
  + [17.2 Byte-Compilation Functions](#Compilation-Functions)
  + [17.3 Documentation Strings and Compilation](#Docs-and-Compilation)
  + [17.4 Evaluation During Compilation](#Eval-During-Compile)
  + [17.5 Compiler Errors](#Compiler-Errors)
  + [17.6 Closure Function Objects](#Closure-Objects)
  + [17.7 Disassembled Byte-Code](#Disassembly)
* [18 Compilation of Lisp to Native Code](#Native-Compilation)
  + [18.1 Native-Compilation Functions](#Native_002dCompilation-Functions)
  + [18.2 Native-Compilation Variables](#Native_002dCompilation-Variables)
* [19 Debugging Lisp Programs](#Debugging)
  + [19.1 The Lisp Debugger](#Debugger)
    - [19.1.1 Entering the Debugger on an Error](#Error-Debugging)
    - [19.1.2 Debugging Redisplay Errors](#Debugging-Redisplay)
    - [19.1.3 Debugging Infinite Loops](#Infinite-Loops)
    - [19.1.4 Entering the Debugger on a Function Call](#Function-Debugging)
    - [19.1.5 Entering the debugger when a variable is modified](#Variable-Debugging)
    - [19.1.6 Explicit Entry to the Debugger](#Explicit-Debug)
    - [19.1.7 Using the Debugger](#Using-Debugger)
    - [19.1.8 Backtraces](#Backtraces)
    - [19.1.9 Debugger Commands](#Debugger-Commands)
    - [19.1.10 Invoking the Debugger](#Invoking-the-Debugger)
    - [19.1.11 Internals of the Debugger](#Internals-of-Debugger)
  + [19.2 Edebug](#Edebug)
    - [19.2.1 Using Edebug](#Using-Edebug)
    - [19.2.2 Instrumenting for Edebug](#Instrumenting)
    - [19.2.3 Edebug Execution Modes](#Edebug-Execution-Modes)
    - [19.2.4 Jumping](#Jumping)
    - [19.2.5 Miscellaneous Edebug Commands](#Edebug-Misc)
    - [19.2.6 Breaks](#Breaks)
      * [19.2.6.1 Edebug Breakpoints](#Breakpoints)
      * [19.2.6.2 Global Break Condition](#Global-Break-Condition)
      * [19.2.6.3 Source Breakpoints](#Source-Breakpoints)
    - [19.2.7 Trapping Errors](#Trapping-Errors)
    - [19.2.8 Edebug Views](#Edebug-Views)
    - [19.2.9 Evaluation](#Edebug-Eval)
    - [19.2.10 Evaluation List Buffer](#Eval-List)
    - [19.2.11 Printing in Edebug](#Printing-in-Edebug)
    - [19.2.12 Trace Buffer](#Trace-Buffer)
    - [19.2.13 Coverage Testing](#Coverage-Testing)
    - [19.2.14 The Outside Context](#The-Outside-Context)
      * [19.2.14.1 Checking Whether to Stop](#Checking-Whether-to-Stop)
      * [19.2.14.2 Edebug Display Update](#Edebug-Display-Update)
      * [19.2.14.3 Edebug Recursive Edit](#Edebug-Recursive-Edit)
    - [19.2.15 Edebug and Macros](#Edebug-and-Macros)
      * [19.2.15.1 Instrumenting Macro Calls](#Instrumenting-Macro-Calls)
      * [19.2.15.2 Specification List](#Specification-List)
      * [19.2.15.3 Backtracking in Specifications](#Backtracking)
      * [19.2.15.4 Specification Examples](#Specification-Examples)
    - [19.2.16 Edebug Options](#Edebug-Options)
  + [19.3 Debugging Invalid Lisp Syntax](#Syntax-Errors)
    - [19.3.1 Excess Open Parentheses](#Excess-Open)
    - [19.3.2 Excess Close Parentheses](#Excess-Close)
  + [19.4 Test Coverage](#Test-Coverage)
  + [19.5 Profiling](#Profiling)
* [20 Reading and Printing Lisp Objects](#Read-and-Print)
  + [20.1 Introduction to Reading and Printing](#Streams-Intro)
  + [20.2 Input Streams](#Input-Streams)
  + [20.3 Input Functions](#Input-Functions)
  + [20.4 Output Streams](#Output-Streams)
  + [20.5 Output Functions](#Output-Functions)
  + [20.6 Variables Affecting Output](#Output-Variables)
  + [20.7 Overriding Output Variables](#Output-Overrides)
* [21 Minibuffers](#Minibuffers)
  + [21.1 Introduction to Minibuffers](#Intro-to-Minibuffers)
  + [21.2 Reading Text Strings with the Minibuffer](#Text-from-Minibuffer)
  + [21.3 Reading Lisp Objects with the Minibuffer](#Object-from-Minibuffer)
  + [21.4 Minibuffer History](#Minibuffer-History)
  + [21.5 Initial Input](#Initial-Input)
  + [21.6 Completion](#Completion)
    - [21.6.1 Basic Completion Functions](#Basic-Completion)
    - [21.6.2 Completion and the Minibuffer](#Minibuffer-Completion)
    - [21.6.3 Minibuffer Commands that Do Completion](#Completion-Commands)
    - [21.6.4 High-Level Completion Functions](#High_002dLevel-Completion)
    - [21.6.5 Reading File Names](#Reading-File-Names)
    - [21.6.6 Completion Variables](#Completion-Variables)
    - [21.6.7 Programmed Completion](#Programmed-Completion)
    - [21.6.8 Completion in Ordinary Buffers](#Completion-in-Buffers)
  + [21.7 Yes-or-No Queries](#Yes_002dor_002dNo-Queries)
  + [21.8 Asking Multiple-Choice Questions](#Multiple-Queries)
  + [21.9 Reading a Password](#Reading-a-Password)
  + [21.10 Minibuffer Commands](#Minibuffer-Commands)
  + [21.11 Minibuffer Windows](#Minibuffer-Windows)
  + [21.12 Minibuffer Contents](#Minibuffer-Contents)
  + [21.13 Recursive Minibuffers](#Recursive-Mini)
  + [21.14 Inhibiting Interaction](#Inhibiting-Interaction)
  + [21.15 Minibuffer Miscellany](#Minibuffer-Misc)
* [22 Command Loop](#Command-Loop)
  + [22.1 Command Loop Overview](#Command-Overview)
  + [22.2 Defining Commands](#Defining-Commands)
    - [22.2.1 Using `interactive`](#Using-Interactive)
    - [22.2.2 Code Characters for `interactive`](#Interactive-Codes)
    - [22.2.3 Examples of Using `interactive`](#Interactive-Examples)
    - [22.2.4 Specifying Modes For Commands](#Command-Modes)
    - [22.2.5 Select among Command Alternatives](#Generic-Commands)
  + [22.3 Interactive Call](#Interactive-Call)
  + [22.4 Distinguish Interactive Calls](#Distinguish-Interactive)
  + [22.5 Information from the Command Loop](#Command-Loop-Info)
  + [22.6 Adjusting Point After Commands](#Adjusting-Point)
  + [22.7 Input Events](#Input-Events)
    - [22.7.1 Keyboard Events](#Keyboard-Events)
    - [22.7.2 Function Keys](#Function-Keys)
    - [22.7.3 Mouse Events](#Mouse-Events)
    - [22.7.4 Click Events](#Click-Events)
    - [22.7.5 Drag Events](#Drag-Events)
    - [22.7.6 Button-Down Events](#Button_002dDown-Events)
    - [22.7.7 Repeat Events](#Repeat-Events)
    - [22.7.8 Motion Events](#Motion-Events)
    - [22.7.9 Touchscreen Events](#Touchscreen-Events)
    - [22.7.10 Focus Events](#Focus-Events)
    - [22.7.11 Xwidget events](#Xwidget-Events)
    - [22.7.12 Miscellaneous System Events](#Misc-Events)
    - [22.7.13 Event Examples](#Event-Examples)
    - [22.7.14 Classifying Events](#Classifying-Events)
    - [22.7.15 Accessing Mouse Events](#Accessing-Mouse)
    - [22.7.16 Accessing Scroll Bar Events](#Accessing-Scroll)
    - [22.7.17 Putting Keyboard Events in Strings](#Strings-of-Events)
  + [22.8 Reading Input](#Reading-Input)
    - [22.8.1 Key Sequence Input](#Key-Sequence-Input)
    - [22.8.2 Reading One Event](#Reading-One-Event)
    - [22.8.3 Modifying and Translating Input Events](#Event-Mod)
    - [22.8.4 Invoking the Input Method](#Invoking-the-Input-Method)
    - [22.8.5 Quoted Character Input](#Quoted-Character-Input)
    - [22.8.6 Miscellaneous Event Input Features](#Event-Input-Misc)
  + [22.9 Special Events](#Special-Events)
  + [22.10 Waiting for Elapsed Time or Input](#Waiting)
  + [22.11 Quitting](#Quitting)
  + [22.12 Prefix Command Arguments](#Prefix-Command-Arguments)
  + [22.13 Recursive Editing](#Recursive-Editing)
  + [22.14 Disabling Commands](#Disabling-Commands)
  + [22.15 Command History](#Command-History)
  + [22.16 Keyboard Macros](#Keyboard-Macros)
* [23 Keymaps](#Keymaps)
  + [23.1 Keymap Basics](#Keymap-Basics)
  + [23.2 Changing Key Bindings](#Changing-Key-Bindings)
  + [23.3 Format of Keymaps](#Format-of-Keymaps)
  + [23.4 Creating Keymaps](#Creating-Keymaps)
  + [23.5 Inheritance and Keymaps](#Inheritance-and-Keymaps)
  + [23.6 Prefix Keys](#Prefix-Keys)
  + [23.7 Active Keymaps](#Active-Keymaps)
  + [23.8 Searching the Active Keymaps](#Searching-Keymaps)
  + [23.9 Controlling the Active Keymaps](#Controlling-Active-Maps)
  + [23.10 Key Lookup](#Key-Lookup)
  + [23.11 Functions for Key Lookup](#Functions-for-Key-Lookup)
  + [23.12 Key Sequences](#Key-Sequences)
  + [23.13 Low-Level Key Binding](#Low_002dLevel-Key-Binding)
  + [23.14 Remapping Commands](#Remapping-Commands)
  + [23.15 Keymaps for Translating Sequences of Events](#Translation-Keymaps)
    - [23.15.1 Interaction with normal keymaps](#Interaction-with-normal-keymaps)
  + [23.16 Commands for Binding Keys](#Key-Binding-Commands)
  + [23.17 Scanning Keymaps](#Scanning-Keymaps)
  + [23.18 Menu Keymaps](#Menu-Keymaps)
    - [23.18.1 Defining Menus](#Defining-Menus)
      * [23.18.1.1 Simple Menu Items](#Simple-Menu-Items)
      * [23.18.1.2 Extended Menu Items](#Extended-Menu-Items)
      * [23.18.1.3 Menu Separators](#Menu-Separators)
      * [23.18.1.4 Alias Menu Items](#Alias-Menu-Items)
    - [23.18.2 Menus and the Mouse](#Mouse-Menus)
    - [23.18.3 Menus and the Keyboard](#Keyboard-Menus)
    - [23.18.4 Menu Example](#Menu-Example)
    - [23.18.5 The Menu Bar](#Menu-Bar)
    - [23.18.6 Tool bars](#Tool-Bar)
    - [23.18.7 Modifying Menus](#Modifying-Menus)
    - [23.18.8 Easy Menu](#Easy-Menu)
* [24 Major and Minor Modes](#Modes)
  + [24.1 Hooks](#Hooks)
    - [24.1.1 Running Hooks](#Running-Hooks)
    - [24.1.2 Setting Hooks](#Setting-Hooks)
  + [24.2 Major Modes](#Major-Modes)
    - [24.2.1 Major Mode Conventions](#Major-Mode-Conventions)
    - [24.2.2 How Emacs Chooses a Major Mode](#Auto-Major-Mode)
    - [24.2.3 Getting Help about a Major Mode](#Mode-Help)
    - [24.2.4 Defining Derived Modes](#Derived-Modes)
    - [24.2.5 Basic Major Modes](#Basic-Major-Modes)
    - [24.2.6 Mode Hooks](#Mode-Hooks)
    - [24.2.7 Tabulated List mode](#Tabulated-List-Mode)
    - [24.2.8 Generic Modes](#Generic-Modes)
    - [24.2.9 Major Mode Examples](#Example-Major-Modes)
  + [24.3 Minor Modes](#Minor-Modes)
    - [24.3.1 Conventions for Writing Minor Modes](#Minor-Mode-Conventions)
    - [24.3.2 Keymaps and Minor Modes](#Keymaps-and-Minor-Modes)
    - [24.3.3 Defining Minor Modes](#Defining-Minor-Modes)
  + [24.4 Mode Line Format](#Mode-Line-Format)
    - [24.4.1 Mode Line Basics](#Mode-Line-Basics)
    - [24.4.2 The Data Structure of the Mode Line](#Mode-Line-Data)
    - [24.4.3 The Top Level of Mode Line Control](#Mode-Line-Top)
    - [24.4.4 Variables Used in the Mode Line](#Mode-Line-Variables)
    - [24.4.5 `%`-Constructs in the Mode Line](#g_t_0025_002dConstructs)
    - [24.4.6 Properties in the Mode Line](#Properties-in-Mode)
    - [24.4.7 Window Header Lines](#Header-Lines)
    - [24.4.8 Window Tab Lines](#Tab-Lines)
    - [24.4.9 Emulating Mode Line Formatting](#Emulating-Mode-Line)
  + [24.5 Imenu](#Imenu)
  + [24.6 Outline Minor Mode](#Outline-Minor-Mode)
  + [24.7 Font Lock Mode](#Font-Lock-Mode)
    - [24.7.1 Font Lock Basics](#Font-Lock-Basics)
    - [24.7.2 Search-based Fontification](#Search_002dbased-Fontification)
    - [24.7.3 Customizing Search-Based Fontification](#Customizing-Keywords)
    - [24.7.4 Other Font Lock Variables](#Other-Font-Lock-Variables)
    - [24.7.5 Levels of Font Lock](#Levels-of-Font-Lock)
    - [24.7.6 Precalculated Fontification](#Precalculated-Fontification)
    - [24.7.7 Faces for Font Lock](#Faces-for-Font-Lock)
    - [24.7.8 Syntactic Font Lock](#Syntactic-Font-Lock)
    - [24.7.9 Multiline Font Lock Constructs](#Multiline-Font-Lock)
      * [24.7.9.1 Font Lock Multiline](#Font-Lock-Multiline)
      * [24.7.9.2 Region to Fontify after a Buffer Change](#Region-to-Refontify)
    - [24.7.10 Parser-based Font Lock](#Parser_002dbased-Font-Lock)
  + [24.8 Automatic Indentation of code](#Auto_002dIndentation)
    - [24.8.1 Simple Minded Indentation Engine](#SMIE)
      * [24.8.1.1 SMIE Setup and Features](#SMIE-setup)
      * [24.8.1.2 Operator Precedence Grammars](#Operator-Precedence-Grammars)
      * [24.8.1.3 Defining the Grammar of a Language](#SMIE-Grammar)
      * [24.8.1.4 Defining Tokens](#SMIE-Lexer)
      * [24.8.1.5 Living With a Weak Parser](#SMIE-Tricks)
      * [24.8.1.6 Specifying Indentation Rules](#SMIE-Indentation)
      * [24.8.1.7 Helper Functions for Indentation Rules](#SMIE-Indentation-Helpers)
      * [24.8.1.8 Sample Indentation Rules](#SMIE-Indentation-Example)
      * [24.8.1.9 Customizing Indentation](#SMIE-Customization)
    - [24.8.2 Parser-based Indentation](#Parser_002dbased-Indentation)
  + [24.9 Desktop Save Mode](#Desktop-Save-Mode)
* [25 Documentation](#Documentation)
  + [25.1 Documentation Basics](#Documentation-Basics)
  + [25.2 Access to Documentation Strings](#Accessing-Documentation)
  + [25.3 Substituting Key Bindings in Documentation](#Keys-in-Documentation)
  + [25.4 Text Quoting Style](#Text-Quoting-Style)
  + [25.5 Describing Characters for Help Messages](#Describing-Characters)
  + [25.6 Help Functions](#Help-Functions)
  + [25.7 Documentation Groups](#Documentation-Groups)
* [26 Files](#Files)
  + [26.1 Visiting Files](#Visiting-Files)
    - [26.1.1 Functions for Visiting Files](#Visiting-Functions)
    - [26.1.2 Subroutines of Visiting](#Subroutines-of-Visiting)
  + [26.2 Saving Buffers](#Saving-Buffers)
  + [26.3 Reading from Files](#Reading-from-Files)
  + [26.4 Writing to Files](#Writing-to-Files)
  + [26.5 File Locks](#File-Locks)
  + [26.6 Information about Files](#Information-about-Files)
    - [26.6.1 Testing Accessibility](#Testing-Accessibility)
    - [26.6.2 Distinguishing Kinds of Files](#Kinds-of-Files)
    - [26.6.3 Truenames](#Truenames)
    - [26.6.4 File Attributes](#File-Attributes)
    - [26.6.5 Extended File Attributes](#Extended-Attributes)
    - [26.6.6 Locating Files in Standard Places](#Locating-Files)
  + [26.7 Changing File Names and Attributes](#Changing-Files)
  + [26.8 Files and Secondary Storage](#Files-and-Storage)
  + [26.9 File Names](#File-Names)
    - [26.9.1 File Name Components](#File-Name-Components)
    - [26.9.2 Absolute and Relative File Names](#Relative-File-Names)
    - [26.9.3 Directory Names](#Directory-Names)
    - [26.9.4 Functions that Expand Filenames](#File-Name-Expansion)
    - [26.9.5 Generating Unique File Names](#Unique-File-Names)
    - [26.9.6 File Name Completion](#File-Name-Completion)
    - [26.9.7 Standard File Names](#Standard-File-Names)
  + [26.10 Contents of Directories](#Contents-of-Directories)
  + [26.11 Creating, Copying and Deleting Directories](#Create_002fDelete-Dirs)
  + [26.12 Making Certain File Names “Magic”](#Magic-File-Names)
  + [26.13 File Format Conversion](#Format-Conversion)
    - [26.13.1 Overview](#Format-Conversion-Overview)
    - [26.13.2 Round-Trip Specification](#Format-Conversion-Round_002dTrip)
    - [26.13.3 Piecemeal Specification](#Format-Conversion-Piecemeal)
* [27 Backups and Auto-Saving](#Backups-and-Auto_002dSaving)
  + [27.1 Backup Files](#Backup-Files)
    - [27.1.1 Making Backup Files](#Making-Backups)
    - [27.1.2 Backup by Renaming or by Copying?](#Rename-or-Copy)
    - [27.1.3 Making and Deleting Numbered Backup Files](#Numbered-Backups)
    - [27.1.4 Naming Backup Files](#Backup-Names)
  + [27.2 Auto-Saving](#Auto_002dSaving)
  + [27.3 Reverting](#Reverting)
* [28 Buffers](#Buffers)
  + [28.1 Buffer Basics](#Buffer-Basics)
  + [28.2 The Current Buffer](#Current-Buffer)
  + [28.3 Buffer Names](#Buffer-Names)
  + [28.4 Buffer File Name](#Buffer-File-Name)
  + [28.5 Buffer Modification](#Buffer-Modification)
  + [28.6 Buffer Modification Time](#Modification-Time)
  + [28.7 Read-Only Buffers](#Read-Only-Buffers)
  + [28.8 The Buffer List](#Buffer-List)
  + [28.9 Creating Buffers](#Creating-Buffers)
  + [28.10 Killing Buffers](#Killing-Buffers)
  + [28.11 Indirect Buffers](#Indirect-Buffers)
  + [28.12 Swapping Text Between Two Buffers](#Swapping-Text)
  + [28.13 The Buffer Gap](#Buffer-Gap)
* [29 Windows](#Windows)
  + [29.1 Basic Concepts of Emacs Windows](#Basic-Windows)
  + [29.2 Windows and Frames](#Windows-and-Frames)
  + [29.3 Selecting Windows](#Selecting-Windows)
  + [29.4 Window Sizes](#Window-Sizes)
  + [29.5 Resizing Windows](#Resizing-Windows)
  + [29.6 Preserving Window Sizes](#Preserving-Window-Sizes)
  + [29.7 Splitting Windows](#Splitting-Windows)
  + [29.8 Deleting Windows](#Deleting-Windows)
  + [29.9 Recombining Windows](#Recombining-Windows)
  + [29.10 Resurrecting Windows](#Resurrecting-Windows)
  + [29.11 Changing Window Layouts](#Changing-Window-Layouts)
  + [29.12 Cyclic Ordering of Windows](#Cyclic-Window-Ordering)
  + [29.13 Buffers and Windows](#Buffers-and-Windows)
  + [29.14 Switching to a Buffer in a Window](#Switching-Buffers)
  + [29.15 Displaying a Buffer in a Suitable Window](#Displaying-Buffers)
    - [29.15.1 Choosing a Window for Displaying a Buffer](#Choosing-Window)
    - [29.15.2 Action Functions for Buffer Display](#Buffer-Display-Action-Functions)
    - [29.15.3 Action Alists for Buffer Display](#Buffer-Display-Action-Alists)
    - [29.15.4 Additional Options for Displaying Buffers](#Choosing-Window-Options)
    - [29.15.5 Precedence of Action Functions](#Precedence-of-Action-Functions)
    - [29.15.6 The Zen of Buffer Display](#The-Zen-of-Buffer-Display)
  + [29.16 Window History](#Window-History)
  + [29.17 Dedicated Windows](#Dedicated-Windows)
  + [29.18 Quitting Windows](#Quitting-Windows)
  + [29.19 Side Windows](#Side-Windows)
    - [29.19.1 Displaying Buffers in Side Windows](#Displaying-Buffers-in-Side-Windows)
    - [29.19.2 Side Window Options and Functions](#Side-Window-Options-and-Functions)
    - [29.19.3 Frame Layouts with Side Windows](#Frame-Layouts-with-Side-Windows)
  + [29.20 Atomic Windows](#Atomic-Windows)
  + [29.21 Windows and Point](#Window-Point)
  + [29.22 The Window Start and End Positions](#Window-Start-and-End)
  + [29.23 Textual Scrolling](#Textual-Scrolling)
  + [29.24 Vertical Fractional Scrolling](#Vertical-Scrolling)
  + [29.25 Horizontal Scrolling](#Horizontal-Scrolling)
  + [29.26 Coordinates and Windows](#Coordinates-and-Windows)
  + [29.27 Mouse Window Auto-selection](#Mouse-Window-Auto_002dselection)
  + [29.28 Window Configurations](#Window-Configurations)
  + [29.29 Window Parameters](#Window-Parameters)
  + [29.30 Hooks for Window Scrolling and Changes](#Window-Hooks)
* [30 Frames](#Frames)
  + [30.1 Creating Frames](#Creating-Frames)
  + [30.2 Multiple Terminals](#Multiple-Terminals)
  + [30.3 Frame Geometry](#Frame-Geometry)
    - [30.3.1 Frame Layout](#Frame-Layout)
    - [30.3.2 Frame Font](#Frame-Font)
    - [30.3.3 Frame Position](#Frame-Position)
    - [30.3.4 Frame Size](#Frame-Size)
    - [30.3.5 Implied Frame Resizing](#Implied-Frame-Resizing)
  + [30.4 Frame Parameters](#Frame-Parameters)
    - [30.4.1 Access to Frame Parameters](#Parameter-Access)
    - [30.4.2 Initial Frame Parameters](#Initial-Parameters)
    - [30.4.3 Window Frame Parameters](#Window-Frame-Parameters)
      * [30.4.3.1 Basic Parameters](#Basic-Parameters)
      * [30.4.3.2 Position Parameters](#Position-Parameters)
      * [30.4.3.3 Size Parameters](#Size-Parameters)
      * [30.4.3.4 Layout Parameters](#Layout-Parameters)
      * [30.4.3.5 Buffer Parameters](#Buffer-Parameters)
      * [30.4.3.6 Frame Interaction Parameters](#Frame-Interaction-Parameters)
      * [30.4.3.7 Mouse Dragging Parameters](#Mouse-Dragging-Parameters)
      * [30.4.3.8 Window Management Parameters](#Management-Parameters)
      * [30.4.3.9 Cursor Parameters](#Cursor-Parameters)
      * [30.4.3.10 Font and Color Parameters](#Font-and-Color-Parameters)
    - [30.4.4 Geometry](#Geometry)
  + [30.5 Terminal Parameters](#Terminal-Parameters)
  + [30.6 Frame Titles](#Frame-Titles)
  + [30.7 Deleting Frames](#Deleting-Frames)
  + [30.8 Finding All Frames](#Finding-All-Frames)
  + [30.9 Splitting and Merging Frames](#Splitting-and-Merging-Frames)
  + [30.10 Minibuffers and Frames](#Minibuffers-and-Frames)
  + [30.11 Input Focus](#Input-Focus)
  + [30.12 Visibility of Frames](#Visibility-of-Frames)
  + [30.13 Raising, Lowering and Restacking Frames](#Raising-and-Lowering)
  + [30.14 Frame Configurations](#Frame-Configurations)
  + [30.15 Child Frames](#Child-Frames)
    - [30.15.1 Child Frame Operations](#Child-Frame-Operations)
    - [30.15.2 Child Frame Properties](#Child-Frame-Properties)
    - [30.15.3 Child Frame Peculiarities](#Child-Frame-Peculiarities)
  + [30.16 Mouse Tracking](#Mouse-Tracking)
  + [30.17 Mouse Position](#Mouse-Position)
  + [30.18 Pop-Up Menus](#Pop_002dUp-Menus)
  + [30.19 On-Screen Keyboards](#On_002dScreen-Keyboards)
  + [30.20 Dialog Boxes](#Dialog-Boxes)
  + [30.21 Pointer Shape](#Pointer-Shape)
  + [30.22 Window System Selections](#Window-System-Selections)
  + [30.23 Accessing Selections](#Accessing-Selections)
    - [30.23.1 X Selections](#X-Selections)
    - [30.23.2 Other Selections](#Other-Selections)
  + [30.24 Yanking Media](#Yanking-Media)
  + [30.25 Drag and Drop](#Drag-and-Drop)
  + [30.26 Color Names](#Color-Names)
  + [30.27 Text Terminal Colors](#Text-Terminal-Colors)
  + [30.28 X Resources](#Resources)
  + [30.29 Display Feature Testing](#Display-Feature-Testing)
* [31 Positions](#Positions)
  + [31.1 Point](#Point)
  + [31.2 Motion](#Motion)
    - [31.2.1 Motion by Characters](#Character-Motion)
    - [31.2.2 Motion by Words](#Word-Motion)
    - [31.2.3 Motion to an End of the Buffer](#Buffer-End-Motion)
    - [31.2.4 Motion by Text Lines](#Text-Lines)
    - [31.2.5 Motion by Screen Lines](#Screen-Lines)
    - [31.2.6 Moving over Balanced Expressions](#List-Motion)
    - [31.2.7 Skipping Characters](#Skipping-Characters)
  + [31.3 Excursions](#Excursions)
  + [31.4 Narrowing](#Narrowing)
* [32 Markers](#Markers)
  + [32.1 Overview of Markers](#Overview-of-Markers)
  + [32.2 Predicates on Markers](#Predicates-on-Markers)
  + [32.3 Functions that Create Markers](#Creating-Markers)
  + [32.4 Information from Markers](#Information-from-Markers)
  + [32.5 Marker Insertion Types](#Marker-Insertion-Types)
  + [32.6 Moving Marker Positions](#Moving-Markers)
  + [32.7 The Mark](#The-Mark)
  + [32.8 The Region](#The-Region)
* [33 Text](#Text)
  + [33.1 Examining Text Near Point](#Near-Point)
  + [33.2 Examining Buffer Contents](#Buffer-Contents)
  + [33.3 Comparing Text](#Comparing-Text)
  + [33.4 Inserting Text](#Insertion)
  + [33.5 User-Level Insertion Commands](#Commands-for-Insertion)
  + [33.6 Deleting Text](#Deletion)
  + [33.7 User-Level Deletion Commands](#User_002dLevel-Deletion)
  + [33.8 The Kill Ring](#The-Kill-Ring)
    - [33.8.1 Kill Ring Concepts](#Kill-Ring-Concepts)
    - [33.8.2 Functions for Killing](#Kill-Functions)
    - [33.8.3 Yanking](#Yanking)
    - [33.8.4 Functions for Yanking](#Yank-Commands)
    - [33.8.5 Low-Level Kill Ring](#Low_002dLevel-Kill-Ring)
    - [33.8.6 Internals of the Kill Ring](#Internals-of-Kill-Ring)
  + [33.9 Undo](#Undo)
  + [33.10 Maintaining Undo Lists](#Maintaining-Undo)
  + [33.11 Filling](#Filling)
  + [33.12 Margins for Filling](#Margins)
  + [33.13 Adaptive Fill Mode](#Adaptive-Fill)
  + [33.14 Auto Filling](#Auto-Filling)
  + [33.15 Sorting Text](#Sorting)
  + [33.16 Counting Columns](#Columns)
  + [33.17 Indentation](#Indentation)
    - [33.17.1 Indentation Primitives](#Primitive-Indent)
    - [33.17.2 Indentation Controlled by Major Mode](#Mode_002dSpecific-Indent)
    - [33.17.3 Indenting an Entire Region](#Region-Indent)
    - [33.17.4 Indentation Relative to Previous Lines](#Relative-Indent)
    - [33.17.5 Adjustable Tab Stops](#Indent-Tabs)
    - [33.17.6 Indentation-Based Motion Commands](#Motion-by-Indent)
  + [33.18 Case Changes](#Case-Changes)
  + [33.19 Text Properties](#Text-Properties)
    - [33.19.1 Examining Text Properties](#Examining-Properties)
    - [33.19.2 Changing Text Properties](#Changing-Properties)
    - [33.19.3 Text Property Search Functions](#Property-Search)
    - [33.19.4 Properties with Special Meanings](#Special-Properties)
    - [33.19.5 Formatted Text Properties](#Format-Properties)
    - [33.19.6 Stickiness of Text Properties](#Sticky-Properties)
    - [33.19.7 Lazy Computation of Text Properties](#Lazy-Properties)
    - [33.19.8 Defining Clickable Text](#Clickable-Text)
    - [33.19.9 Defining and Using Fields](#Fields)
    - [33.19.10 Why Text Properties are not Intervals](#Not-Intervals)
  + [33.20 Substituting for a Character Code](#Substitution)
  + [33.21 Registers](#Registers)
  + [33.22 Transposition of Text](#Transposition)
  + [33.23 Replacing Buffer Text](#Replacing)
  + [33.24 Dealing With Compressed Data](#Decompression)
  + [33.25 Base 64 Encoding](#Base-64)
  + [33.26 Checksum/Hash](#Checksum_002fHash)
  + [33.27 Suspicious Text](#Suspicious-Text)
  + [33.28 GnuTLS Cryptography](#GnuTLS-Cryptography)
    - [33.28.1 Format of GnuTLS Cryptography Inputs](#Format-of-GnuTLS-Cryptography-Inputs)
    - [33.28.2 GnuTLS Cryptographic Functions](#GnuTLS-Cryptographic-Functions)
  + [33.29 Database](#Database)
  + [33.30 Parsing HTML and XML](#Parsing-HTML_002fXML)
    - [33.30.1 Document Object Model](#Document-Object-Model)
  + [33.31 Parsing and generating JSON values](#Parsing-JSON)
  + [33.32 JSONRPC communication](#JSONRPC)
    - [33.32.1 Overview](#JSONRPC-Overview)
    - [33.32.2 Process-based JSONRPC connections](#Process_002dbased-JSONRPC-connections)
    - [33.32.3 JSONRPC JSON object format](#JSONRPC-JSON-object-format)
    - [33.32.4 Deferred JSONRPC requests](#JSONRPC-deferred-requests)
  + [33.33 Atomic Change Groups](#Atomic-Changes)
  + [33.34 Change Hooks](#Change-Hooks)
    - [33.34.1 Keeping track of buffer modifications](#Tracking-changes)
* [34 Non-ASCII Characters](#Non_002dASCII-Characters)
  + [34.1 Text Representations](#Text-Representations)
  + [34.2 Disabling Multibyte Characters](#Disabling-Multibyte)
  + [34.3 Converting Text Representations](#Converting-Representations)
  + [34.4 Selecting a Representation](#Selecting-a-Representation)
  + [34.5 Character Codes](#Character-Codes)
  + [34.6 Character Properties](#Character-Properties)
  + [34.7 Character Sets](#Character-Sets)
  + [34.8 Scanning for Character Sets](#Scanning-Charsets)
  + [34.9 Translation of Characters](#Translation-of-Characters)
  + [34.10 Coding Systems](#Coding-Systems)
    - [34.10.1 Basic Concepts of Coding Systems](#Coding-System-Basics)
    - [34.10.2 Encoding and I/O](#Encoding-and-I_002fO)
    - [34.10.3 Coding Systems in Lisp](#Lisp-and-Coding-Systems)
    - [34.10.4 User-Chosen Coding Systems](#User_002dChosen-Coding-Systems)
    - [34.10.5 Default Coding Systems](#Default-Coding-Systems)
    - [34.10.6 Specifying a Coding System for One Operation](#Specifying-Coding-Systems)
    - [34.10.7 Explicit Encoding and Decoding](#Explicit-Encoding)
    - [34.10.8 Terminal I/O Encoding](#Terminal-I_002fO-Encoding)
  + [34.11 Input Methods](#Input-Methods)
  + [34.12 Locales](#Locales)
* [35 Searching and Matching](#Searching-and-Matching)
  + [35.1 Searching for Strings](#String-Search)
  + [35.2 Searching and Case](#Searching-and-Case)
  + [35.3 Regular Expressions](#Regular-Expressions)
    - [35.3.1 Syntax of Regular Expressions](#Syntax-of-Regexps)
      * [35.3.1.1 Special Characters in Regular Expressions](#Regexp-Special)
      * [35.3.1.2 Character Classes](#Char-Classes)
      * [35.3.1.3 Backslash Constructs in Regular Expressions](#Regexp-Backslash)
    - [35.3.2 Complex Regexp Example](#Regexp-Example)
    - [35.3.3 The `rx` Structured Regexp Notation](#Rx-Notation)
      * [35.3.3.1 Constructs in `rx` regexps](#Rx-Constructs)
      * [35.3.3.2 Functions and macros using `rx` regexps](#Rx-Functions)
      * [35.3.3.3 Defining new `rx` forms](#Extending-Rx)
    - [35.3.4 Regular Expression Functions](#Regexp-Functions)
    - [35.3.5 Problems with Regular Expressions](#Regexp-Problems)
  + [35.4 Regular Expression Searching](#Regexp-Search)
  + [35.5 Longest-match searching for regular expression matches](#Longest-Match)
  + [35.6 The Match Data](#Match-Data)
    - [35.6.1 Replacing the Text that Matched](#Replacing-Match)
    - [35.6.2 Simple Match Data Access](#Simple-Match-Data)
    - [35.6.3 Accessing the Entire Match Data](#Entire-Match-Data)
    - [35.6.4 Saving and Restoring the Match Data](#Saving-Match-Data)
  + [35.7 Search and Replace](#Search-and-Replace)
  + [35.8 Standard Regular Expressions Used in Editing](#Standard-Regexps)
  + [35.9 Emacs versus POSIX Regular Expressions](#POSIX-Regexps)
* [36 Syntax Tables](#Syntax-Tables)
  + [36.1 Syntax Table Concepts](#Syntax-Basics)
  + [36.2 Syntax Descriptors](#Syntax-Descriptors)
    - [36.2.1 Table of Syntax Classes](#Syntax-Class-Table)
    - [36.2.2 Syntax Flags](#Syntax-Flags)
  + [36.3 Syntax Table Functions](#Syntax-Table-Functions)
  + [36.4 Syntax Properties](#Syntax-Properties)
  + [36.5 Motion and Syntax](#Motion-and-Syntax)
  + [36.6 Parsing Expressions](#Parsing-Expressions)
    - [36.6.1 Motion Commands Based on Parsing](#Motion-via-Parsing)
    - [36.6.2 Finding the Parse State for a Position](#Position-Parse)
    - [36.6.3 Parser State](#Parser-State)
    - [36.6.4 Low-Level Parsing](#Low_002dLevel-Parsing)
    - [36.6.5 Parameters to Control Parsing](#Control-Parsing)
  + [36.7 Syntax Table Internals](#Syntax-Table-Internals)
  + [36.8 Categories](#Categories)
* [37 Parsing Expression Grammars](#Parsing-Expression-Grammars)
  + [37.1 PEX Definitions](#PEX-Definitions)
  + [37.2 Parsing Actions](#Parsing-Actions)
  + [37.3 Writing PEG Rules](#Writing-PEG-Rules)
* [38 Parsing Program Source](#Parsing-Program-Source)
  + [38.1 Tree-sitter Language Grammar](#Language-Grammar)
  + [38.2 Using Tree-sitter Parser](#Using-Parser)
  + [38.3 Retrieving Nodes](#Retrieving-Nodes)
  + [38.4 Accessing Node Information](#Accessing-Node-Information)
  + [38.5 Pattern Matching Tree-sitter Nodes](#Pattern-Matching)
  + [38.6 User-defined “Things” and Navigation](#User_002ddefined-Things)
  + [38.7 Parsing Text in Multiple Languages](#Multiple-Languages)
  + [38.8 Developing major modes with tree-sitter](#Tree_002dsitter-Major-Modes)
  + [38.9 Tree-sitter C API Correspondence](#Tree_002dsitter-C-API)
* [39 Abbrevs and Abbrev Expansion](#Abbrevs)
  + [39.1 Abbrev Tables](#Abbrev-Tables)
  + [39.2 Defining Abbrevs](#Defining-Abbrevs)
  + [39.3 Saving Abbrevs in Files](#Abbrev-Files)
  + [39.4 Looking Up and Expanding Abbreviations](#Abbrev-Expansion)
  + [39.5 Standard Abbrev Tables](#Standard-Abbrev-Tables)
  + [39.6 Abbrev Properties](#Abbrev-Properties)
  + [39.7 Abbrev Table Properties](#Abbrev-Table-Properties)
* [40 Threads](#Threads)
  + [40.1 Basic Thread Functions](#Basic-Thread-Functions)
  + [40.2 Mutexes](#Mutexes)
  + [40.3 Condition Variables](#Condition-Variables)
  + [40.4 The Thread List](#The-Thread-List)
* [41 Processes](#Processes)
  + [41.1 Functions that Create Subprocesses](#Subprocess-Creation)
  + [41.2 Shell Arguments](#Shell-Arguments)
  + [41.3 Creating a Synchronous Process](#Synchronous-Processes)
  + [41.4 Creating an Asynchronous Process](#Asynchronous-Processes)
  + [41.5 Deleting Processes](#Deleting-Processes)
  + [41.6 Process Information](#Process-Information)
  + [41.7 Sending Input to Processes](#Input-to-Processes)
  + [41.8 Sending Signals to Processes](#Signals-to-Processes)
  + [41.9 Receiving Output from Processes](#Output-from-Processes)
    - [41.9.1 Process Buffers](#Process-Buffers)
    - [41.9.2 Process Filter Functions](#Filter-Functions)
    - [41.9.3 Decoding Process Output](#Decoding-Output)
    - [41.9.4 Accepting Output from Processes](#Accepting-Output)
    - [41.9.5 Processes and Threads](#Processes-and-Threads)
  + [41.10 Sentinels: Detecting Process Status Changes](#Sentinels)
  + [41.11 Querying Before Exit](#Query-Before-Exit)
  + [41.12 Accessing Other Processes](#System-Processes)
  + [41.13 Transaction Queues](#Transaction-Queues)
  + [41.14 Network Connections](#Network)
  + [41.15 Network Servers](#Network-Servers)
  + [41.16 Datagrams](#Datagrams)
  + [41.17 Low-Level Network Access](#Low_002dLevel-Network)
    - [41.17.1 `make-network-process`](#Network-Processes)
    - [41.17.2 Network Options](#Network-Options)
    - [41.17.3 Testing Availability of Network Features](#Network-Feature-Testing)
  + [41.18 Misc Network Facilities](#Misc-Network)
  + [41.19 Communicating with Serial Ports](#Serial-Ports)
  + [41.20 Packing and Unpacking Byte Arrays](#Byte-Packing)
    - [41.20.1 Describing Data Layout](#Bindat-Types)
    - [41.20.2 Functions to Unpack and Pack Bytes](#Bindat-Functions)
    - [41.20.3 Advanced data layout specifications](#Bindat-Computed-Types)
* [42 Emacs Display](#Display)
  + [42.1 Refreshing the Screen](#Refresh-Screen)
  + [42.2 Forcing Redisplay](#Forcing-Redisplay)
  + [42.3 Truncation](#Truncation)
  + [42.4 The Echo Area](#The-Echo-Area)
    - [42.4.1 Displaying Messages in the Echo Area](#Displaying-Messages)
    - [42.4.2 Reporting Operation Progress](#Progress)
    - [42.4.3 Logging Messages in `*Messages*`](#Logging-Messages)
    - [42.4.4 Echo Area Customization](#Echo-Area-Customization)
  + [42.5 Reporting Warnings](#Warnings)
    - [42.5.1 Warning Basics](#Warning-Basics)
    - [42.5.2 Warning Variables](#Warning-Variables)
    - [42.5.3 Warning Options](#Warning-Options)
    - [42.5.4 Delayed Warnings](#Delayed-Warnings)
  + [42.6 Invisible Text](#Invisible-Text)
  + [42.7 Selective Display](#Selective-Display)
  + [42.8 Temporary Displays](#Temporary-Displays)
  + [42.9 Overlays](#Overlays)
    - [42.9.1 Managing Overlays](#Managing-Overlays)
    - [42.9.2 Overlay Properties](#Overlay-Properties)
    - [42.9.3 Searching for Overlays](#Finding-Overlays)
  + [42.10 Size of Displayed Text](#Size-of-Displayed-Text)
  + [42.11 Line Height](#Line-Height)
  + [42.12 Faces](#Faces)
    - [42.12.1 Face Attributes](#Face-Attributes)
    - [42.12.2 Defining Faces](#Defining-Faces)
    - [42.12.3 Face Attribute Functions](#Attribute-Functions)
    - [42.12.4 Displaying Faces](#Displaying-Faces)
    - [42.12.5 Face Remapping](#Face-Remapping)
    - [42.12.6 Functions for Working with Faces](#Face-Functions)
    - [42.12.7 Automatic Face Assignment](#Auto-Faces)
    - [42.12.8 Basic Faces](#Basic-Faces)
    - [42.12.9 Font Selection](#Font-Selection)
    - [42.12.10 Looking Up Fonts](#Font-Lookup)
    - [42.12.11 Fontsets](#Fontsets)
    - [42.12.12 Low-Level Font Representation](#Low_002dLevel-Font)
  + [42.13 Fringes](#Fringes)
    - [42.13.1 Fringe Size and Position](#Fringe-Size_002fPos)
    - [42.13.2 Fringe Indicators](#Fringe-Indicators)
    - [42.13.3 Fringe Cursors](#Fringe-Cursors)
    - [42.13.4 Fringe Bitmaps](#Fringe-Bitmaps)
    - [42.13.5 Customizing Fringe Bitmaps](#Customizing-Bitmaps)
    - [42.13.6 The Overlay Arrow](#Overlay-Arrow)
  + [42.14 Scroll Bars](#Scroll-Bars)
  + [42.15 Window Dividers](#Window-Dividers)
  + [42.16 The `display` Property](#Display-Property)
    - [42.16.1 Display Specs That Replace The Text](#Replacing-Specs)
    - [42.16.2 Specified Spaces](#Specified-Space)
    - [42.16.3 Pixel Specification for Spaces](#Pixel-Specification)
    - [42.16.4 Other Display Specifications](#Other-Display-Specs)
    - [42.16.5 Displaying in the Margins](#Display-Margins)
  + [42.17 Images](#Images)
    - [42.17.1 Image Formats](#Image-Formats)
    - [42.17.2 Image Descriptors](#Image-Descriptors)
    - [42.17.3 XBM Images](#XBM-Images)
    - [42.17.4 XPM Images](#XPM-Images)
    - [42.17.5 ImageMagick Images](#ImageMagick-Images)
    - [42.17.6 SVG Images](#SVG-Images)
    - [42.17.7 Other Image Types](#Other-Image-Types)
    - [42.17.8 Defining Images](#Defining-Images)
    - [42.17.9 Showing Images](#Showing-Images)
    - [42.17.10 Multi-Frame Images](#Multi_002dFrame-Images)
    - [42.17.11 Image Cache](#Image-Cache)
  + [42.18 Icons](#Icons)
  + [42.19 Embedded Native Widgets](#Xwidgets)
  + [42.20 Buttons](#Buttons)
    - [42.20.1 Button Properties](#Button-Properties)
    - [42.20.2 Button Types](#Button-Types)
    - [42.20.3 Making Buttons](#Making-Buttons)
    - [42.20.4 Manipulating Buttons](#Manipulating-Buttons)
    - [42.20.5 Button Buffer Commands](#Button-Buffer-Commands)
  + [42.21 Abstract Display](#Abstract-Display)
    - [42.21.1 Abstract Display Functions](#Abstract-Display-Functions)
    - [42.21.2 Abstract Display Example](#Abstract-Display-Example)
  + [42.22 Blinking Parentheses](#Blinking)
  + [42.23 Character Display](#Character-Display)
    - [42.23.1 Usual Display Conventions](#Usual-Display)
    - [42.23.2 Display Tables](#Display-Tables)
    - [42.23.3 Active Display Table](#Active-Display-Table)
    - [42.23.4 Glyphs](#Glyphs)
    - [42.23.5 Glyphless Character Display](#Glyphless-Chars)
  + [42.24 Beeping](#Beeping)
  + [42.25 Window Systems](#Window-Systems)
  + [42.26 Tooltips](#Tooltips)
  + [42.27 Bidirectional Display](#Bidirectional-Display)
* [43 Operating System Interface](#System-Interface)
  + [43.1 Starting Up Emacs](#Starting-Up)
    - [43.1.1 Summary: Sequence of Actions at Startup](#Startup-Summary)
    - [43.1.2 The Init File](#Init-File)
    - [43.1.3 Terminal-Specific Initialization](#Terminal_002dSpecific)
    - [43.1.4 Command-Line Arguments](#Command_002dLine-Arguments)
  + [43.2 Getting Out of Emacs](#Getting-Out)
    - [43.2.1 Killing Emacs](#Killing-Emacs)
    - [43.2.2 Suspending Emacs](#Suspending-Emacs)
  + [43.3 Operating System Environment](#System-Environment)
  + [43.4 User Identification](#User-Identification)
  + [43.5 Time of Day](#Time-of-Day)
  + [43.6 Time Zone Rules](#Time-Zone-Rules)
  + [43.7 Time Conversion](#Time-Conversion)
  + [43.8 Parsing and Formatting Times](#Time-Parsing)
  + [43.9 Processor Run time](#Processor-Run-Time)
  + [43.10 Time Calculations](#Time-Calculations)
  + [43.11 Timers for Delayed Execution](#Timers)
  + [43.12 Idle Timers](#Idle-Timers)
  + [43.13 Terminal Input](#Terminal-Input)
    - [43.13.1 Input Modes](#Input-Modes)
    - [43.13.2 Recording Input](#Recording-Input)
  + [43.14 Terminal Output](#Terminal-Output)
  + [43.15 Sound Output](#Sound-Output)
  + [43.16 Operating on X11 Keysyms](#X11-Keysyms)
  + [43.17 Batch Mode](#Batch-Mode)
  + [43.18 Session Management](#Session-Management)
  + [43.19 Desktop Notifications](#Desktop-Notifications)
  + [43.20 Controlling System GUI Taskbar Features](#System-Taskbar)
  + [43.21 Block System Sleep and Process Sleep Events](#System-Sleep)
  + [43.22 Notifications on File Changes](#File-Notifications)
  + [43.23 Dynamically Loaded Libraries](#Dynamic-Libraries)
  + [43.24 Security Considerations](#Security-Considerations)
* [44 Preparing Lisp code for distribution](#Packaging)
  + [44.1 Packaging Basics](#Packaging-Basics)
  + [44.2 Simple Packages](#Simple-Packages)
  + [44.3 Multi-file Packages](#Multi_002dfile-Packages)
  + [44.4 Creating and Maintaining Package Archives](#Package-Archives)
  + [44.5 Interfacing to an archive web server](#Archive-Web-Server)
  + [44.6 Supporting older versions of Emacs](#Forwards_002dCompatibility)
  + [44.7 Get built-in packages and versions](#Package-Information)
* [Appendix A Emacs 30 Antinews](#Antinews)
* [Appendix B GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Appendix C GNU General Public License](#GPL)
* [Appendix D Tips and Conventions](#Tips)
  + [D.1 Emacs Lisp Coding Conventions](#Coding-Conventions)
  + [D.2 Key Binding Conventions](#Key-Binding-Conventions)
  + [D.3 Emacs Programming Tips](#Programming-Tips)
  + [D.4 Tips for Making Compiled Code Fast](#Compilation-Tips)
  + [D.5 Tips for Avoiding Compiler Warnings](#Warning-Tips)
  + [D.6 Tips for Documentation Strings](#Documentation-Tips)
  + [D.7 Tips for Documentation Groups](#Documentation-Group-Tips)
  + [D.8 Tips on Writing Comments](#Comment-Tips)
  + [D.9 Conventional Headers for Emacs Libraries](#Library-Headers)
* [Appendix E GNU Emacs Internals](#GNU-Emacs-Internals)
  + [E.1 Building Emacs](#Building-Emacs)
  + [E.2 Garbage Collection](#Garbage-Collection)
  + [E.3 Stack-allocated Objects](#Stack_002dallocated-Objects)
  + [E.4 Memory Usage](#Memory-Usage)
  + [E.5 C Dialect](#C-Dialect)
  + [E.6 Writing Emacs Primitives](#Writing-Emacs-Primitives)
  + [E.7 Writing Dynamically-Loaded Modules](#Writing-Dynamic-Modules)
    - [E.7.1 Module Initialization Code](#Module-Initialization)
    - [E.7.2 Writing Module Functions](#Module-Functions)
    - [E.7.3 Conversion Between Lisp and Module Values](#Module-Values)
    - [E.7.4 Miscellaneous Convenience Functions for Modules](#Module-Misc)
    - [E.7.5 Nonlocal Exits in Modules](#Module-Nonlocal)
  + [E.8 Object Internals](#Object-Internals)
    - [E.8.1 Buffer Internals](#Buffer-Internals)
    - [E.8.2 Window Internals](#Window-Internals)
    - [E.8.3 Process Internals](#Process-Internals)
  + [E.9 C Integer Types](#C-Integer-Types)
* [Appendix F Standard Errors](#Standard-Errors)
* [Appendix G Standard Keymaps](#Standard-Keymaps)
* [Appendix H Standard Hooks](#Standard-Hooks)
* [Index](#Index)

---

Next: [Lisp Data Types](#Lisp-Data-Types), Previous: [Emacs Lisp](#Top), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 1 Introduction [¶](#Introduction-1)

Most of the GNU Emacs text editor is written in the programming
language called Emacs Lisp. You can write new code in Emacs Lisp and
install it as an extension to the editor. However, Emacs Lisp is more
than a mere extension language; it is a full computer programming
language in its own right. You can use it as you would any other
programming language.

Because Emacs Lisp is designed for use in an editor, it has special
features for scanning and parsing text as well as features for handling
files, buffers, displays, subprocesses, and so on. Emacs Lisp is
closely integrated with the editing facilities; thus, editing commands
are functions that can also conveniently be called from Lisp programs,
and parameters for customization are ordinary Lisp variables.

This manual attempts to be a full description of Emacs Lisp. For a
beginner’s introduction to Emacs Lisp, see An Introduction to
Emacs Lisp Programming, by Bob Chassell, also published by the Free
Software Foundation. This manual presumes considerable familiarity with
the use of Emacs for editing; see The GNU Emacs Manual for this
basic information.

Generally speaking, the earlier chapters describe features of Emacs
Lisp that have counterparts in many programming languages, and later
chapters describe features that are peculiar to Emacs Lisp or relate
specifically to editing.

This is
the GNU Emacs Lisp Reference Manual,
corresponding to Emacs version 31.1.50.

* [Caveats](#Caveats)
* [Lisp History](#Lisp-History)
* [Conventions](#Conventions)
* [Version Information](#Version-Info)
* [Acknowledgments](#Acknowledgments)

---

Next: [Lisp History](#Lisp-History), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.1 Caveats [¶](#Caveats-1)

This manual has gone through numerous drafts. It is nearly complete
but not flawless. There are a few topics that are not covered, either
because we consider them secondary (such as most of the individual
modes) or because they are yet to be written. Because we are not able
to deal with them completely, we have left out several parts
intentionally.

The manual should be fully correct in what it does cover, and it is
therefore open to criticism on anything it says—from specific examples
and descriptive text, to the ordering of chapters and sections. If
something is confusing, or you find that you have to look at the sources
or experiment to learn something not covered in the manual, then perhaps
the manual should be fixed. Please let us know.

As you use this manual, we ask that you send corrections as soon as you
find them. If you think of a simple, real life example for a function
or group of functions, please make an effort to write it up and send it
in. Please reference any comments to the node name and function or
variable name, as appropriate. Also state the number of the edition
you are criticizing.

Please send comments and corrections using `M-x report-emacs-bug`.
For more details, See [Reporting Bugs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Bugs) in The GNU Emacs
Manual.

---

Next: [Conventions](#Conventions), Previous: [Caveats](#Caveats), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.2 Lisp History [¶](#Lisp-History-1)

Lisp (LISt Processing language) was first developed in the late 1950s
at the Massachusetts Institute of Technology for research in artificial
intelligence. The great power of the Lisp language makes it ideal
for other purposes as well, such as writing editing commands.

Dozens of Lisp implementations have been built over the years, each
with its own idiosyncrasies. Many of them were inspired by Maclisp,
which was written in the 1960s at MIT’s Project MAC. Eventually the
implementers of the descendants of Maclisp came together and developed a
standard for Lisp systems, called Common Lisp. In the meantime, Gerry
Sussman and Guy Steele at MIT developed a simplified but very powerful
dialect of Lisp, called Scheme.

GNU Emacs Lisp is largely inspired by Maclisp, and a little by Common
Lisp. If you know Common Lisp, you will notice many similarities.
However, many features of Common Lisp have been omitted or
simplified in order to reduce the memory requirements of GNU Emacs.
Sometimes the simplifications are so drastic that a Common Lisp user
might be very confused. We will occasionally point out how GNU Emacs
Lisp differs from Common Lisp. If you don’t know Common Lisp, don’t
worry about it; this manual is self-contained.

A certain amount of Common Lisp emulation is available via the
`cl-lib` library. See [Overview](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Top) in Common Lisp Extensions.

Emacs Lisp is not at all influenced by Scheme; but the GNU project has
an implementation of Scheme, called Guile. We use it in all new GNU
software that calls for extensibility.

---

Next: [Version Information](#Version-Info), Previous: [Lisp History](#Lisp-History), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.3 Conventions [¶](#Conventions-1)

This section explains the notational conventions that are used in this
manual. You may want to skip this section and refer back to it later.

* [Some Terms](#Some-Terms)
* [`nil` and `t`](#nil-and-t)
* [Evaluation Notation](#Evaluation-Notation)
* [Printing Notation](#Printing-Notation)
* [Error Messages](#Error-Messages)
* [Buffer Text Notation](#Buffer-Text-Notation)
* [Format of Descriptions](#Format-of-Descriptions)

---

Next: [`nil` and `t`](#nil-and-t), Up: [Conventions](#Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.1 Some Terms [¶](#Some-Terms-1)

Throughout this manual, the phrases “the Lisp reader” and “the Lisp
printer” refer to those routines in Lisp that convert textual
representations of Lisp objects into actual Lisp objects, and vice
versa. See [Printed Representation and Read Syntax](#Printed-Representation), for more details. You, the
person reading this manual, are thought of as the programmer and are
addressed as “you”. The user is the person who uses Lisp
programs, including those you write.

Examples of Lisp code are formatted like this: `(list 1 2 3)`.
Names that represent metasyntactic variables, or arguments to a function
being described, are formatted like this: first-number.

---

Next: [Evaluation Notation](#Evaluation-Notation), Previous: [Some Terms](#Some-Terms), Up: [Conventions](#Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.2 `nil` and `t` [¶](#nil-and-t-1)

In Emacs Lisp, the symbol `nil` has three separate meanings: it
is a symbol with the name ‘`nil`’; it is the logical truth value
false; and it is the empty list—the list of zero elements.
When used as a variable, `nil` always has the value `nil`.

As far as the Lisp reader is concerned, ‘`()`’ and ‘`nil`’ are
identical: they stand for the same object, the symbol `nil`. The
different ways of writing the symbol are intended entirely for human
readers. After the Lisp reader has read either ‘`()`’ or ‘`nil`’,
there is no way to determine which representation was actually written
by the programmer.

In this manual, we write `()` when we wish to emphasize that it
means the empty list, and we write `nil` when we wish to emphasize
that it means the truth value false. That is a good convention to use
in Lisp programs also.

```
(cons 'foo ())                ; Emphasize the empty list
(setq foo-flag nil)           ; Emphasize the truth value false
```

In contexts where a truth value is expected, any non-`nil` value
is considered to be true. However, `t` is the preferred way
to represent the truth value true. When you need to choose a
value that represents true, and there is no other basis for
choosing, use `t`. The symbol `t` always has the value
`t`.

In Emacs Lisp, `nil` and `t` are special symbols that always
evaluate to themselves. This is so that you do not need to quote them
to use them as constants in a program. An attempt to change their
values results in a `setting-constant` error. See [Variables that Never Change](#Constant-Variables).

Function: **booleanp** object [¶](#index-booleanp)
:   Return non-`nil` if object is one of the two canonical
    boolean values: `t` or `nil`.

---

Next: [Printing Notation](#Printing-Notation), Previous: [`nil` and `t`](#nil-and-t), Up: [Conventions](#Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.3 Evaluation Notation [¶](#Evaluation-Notation-1)

A Lisp expression that you can evaluate is called a *form*.
Evaluating a form always produces a result, which is a Lisp object. In
the examples in this manual, this is indicated with ‘`⇒`’:

```
(car '(1 2))
     ⇒ 1
```

You can read this as “`(car '(1 2))` evaluates to 1”.

When a form is a macro call, it expands into a new form for Lisp to
evaluate. We show the result of the expansion with
‘`→`’. We may or may not show the result of the
evaluation of the expanded form.

```
(third '(a b c))
     → (car (cdr (cdr '(a b c))))
     ⇒ c
```

To help describe one form, we sometimes show another form that
produces identical results. The exact equivalence of two forms is
indicated with ‘`≡`’.

```
(make-sparse-keymap) ≡ (list 'keymap)
```

---

Next: [Error Messages](#Error-Messages), Previous: [Evaluation Notation](#Evaluation-Notation), Up: [Conventions](#Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.4 Printing Notation [¶](#Printing-Notation-1)

Many of the examples in this manual print text when they are
evaluated. If you execute example code in a Lisp Interaction buffer
(such as the buffer `*scratch*`) by typing `C-j` after the
closing parenthesis of the example, the printed text is inserted into
the buffer. If you execute the example by other means (such as by
evaluating the function `eval-region`), the printed text is
displayed in the echo area.

Examples in this manual indicate printed text with ‘`-|`’,
irrespective of where that text goes. The value returned by
evaluating the form follows on a separate line with
‘`⇒`’.

```
(progn (prin1 'foo) (princ "\n") (prin1 'bar))
     -| foo
     -| bar
     ⇒ bar
```

---

Next: [Buffer Text Notation](#Buffer-Text-Notation), Previous: [Printing Notation](#Printing-Notation), Up: [Conventions](#Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.5 Error Messages [¶](#Error-Messages-1)

Some examples signal errors. This normally displays an error message
in the echo area. We show the error message on a line starting with
‘`error→`’. Note that ‘`error→`’ itself does not appear in
the echo area.

```
(+ 23 'x)
error→ Wrong type argument: number-or-marker-p, x
```

---

Next: [Format of Descriptions](#Format-of-Descriptions), Previous: [Error Messages](#Error-Messages), Up: [Conventions](#Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.6 Buffer Text Notation [¶](#Buffer-Text-Notation-1)

Some examples describe modifications to the contents of a buffer, by
showing the before and after versions of the text. These
examples show the contents of the buffer in question between two lines
of dashes containing the buffer name. In addition, ‘`∗`’
indicates the location of point. (The symbol for point, of course, is
not part of the text in the buffer; it indicates the place
*between* two characters where point is currently located.)

```
---------- Buffer: foo ----------
This is the ∗contents of foo.
---------- Buffer: foo ----------

(insert "changed ")
     ⇒ nil
---------- Buffer: foo ----------
This is the changed ∗contents of foo.
---------- Buffer: foo ----------
```

---

Previous: [Buffer Text Notation](#Buffer-Text-Notation), Up: [Conventions](#Conventions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.7 Format of Descriptions [¶](#Format-of-Descriptions-1)

Functions, variables, macros, commands, user options, and special
forms are described in this manual in a uniform format. The first
line of a description contains the name of the item followed by its
arguments, if any.
The category—function, variable, or whatever—appears at the
beginning of the line.
The description follows on succeeding lines, sometimes with examples.

* [A Sample Function Description](#A-Sample-Function-Description)
* [A Sample Variable Description](#A-Sample-Variable-Description)

---

Next: [A Sample Variable Description](#A-Sample-Variable-Description), Up: [Format of Descriptions](#Format-of-Descriptions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.7.1 A Sample Function Description [¶](#A-Sample-Function-Description-1)

In a function description, the name of the function being described
appears first. It is followed on the same line by a list of argument
names. These names are also used in the body of the description, to
stand for the values of the arguments.

The appearance of the keyword `&optional` in the argument list
indicates that the subsequent arguments may be omitted (omitted
arguments default to `nil`). Do not write `&optional` when
you call the function.

The keyword `&rest` (which must be followed by a single
argument name) indicates that any number of arguments can follow. The
single argument name following `&rest` receives, as its
value, a list of all the remaining arguments passed to the function.
Do not write `&rest` when you call the function.

Here is a description of an imaginary function `foo`:

Function: **foo** integer1 &optional integer2 &rest integers [¶](#index-foo)
:   The function `foo` subtracts integer1 from integer2,
    then adds all the rest of the arguments to the result. If integer2
    is not supplied, then the number 19 is used by default.

    ```
    (foo 1 5 3 9)
         ⇒ 16
    (foo 5)
         ⇒ 14
    ```

    More generally,

    ```
    (foo w x y...)
    ≡
    (+ (- x w) y...)
    ```

By convention, any argument whose name contains the name of a type
(e.g., integer, integer1 or buffer) is expected to
be of that type. A plural of a type (such as buffers) often
means a list of objects of that type. An argument named object
may be of any type. (For a list of Emacs object types, see [Lisp Data Types](#Lisp-Data-Types).) An argument with any other sort of name
(e.g., new-file) is specific to the function; if the function
has a documentation string, the type of the argument should be
described there (see [Documentation](#Documentation)).

See [Lambda Expressions](#Lambda-Expressions), for a more complete description of
arguments modified by `&optional` and `&rest`.

Command, macro, and special form descriptions have the same format,
but the word ‘`Function`’ is replaced by ‘`Command`’,
‘`Macro`’, or ‘`Special Form`’, respectively. Commands are
simply functions that may be called interactively; macros process
their arguments differently from functions (the arguments are not
evaluated), but are presented the same way.

The descriptions of macros and special forms use a more complex
notation to specify optional and repeated arguments, because they can
break the argument list down into separate arguments in more
complicated ways. ‘`[optional-arg]`’ means that
optional-arg is optional and ‘`repeated-args…`’
stands for zero or more arguments. Parentheses are used when several
arguments are grouped into additional levels of list structure. Here
is an example:

Special Form: **count-loop** (var [from to [inc]]) body… [¶](#index-count_002dloop)
:   This imaginary special form implements a loop that executes the
    body forms and then increments the variable var on each
    iteration. On the first iteration, the variable has the value
    from; on subsequent iterations, it is incremented by one (or by
    inc if that is given). The loop exits before executing body
    if var equals to. Here is an example:

    ```
    (count-loop (i 0 10)
      (prin1 i) (princ " ")
      (prin1 (aref vector i))
      (terpri))
    ```

    If from and to are omitted, var is bound to
    `nil` before the loop begins, and the loop exits if var is
    non-`nil` at the beginning of an iteration. Here is an example:

    ```
    (count-loop (done)
      (if (pending)
          (fixit)
        (setq done t)))
    ```

    In this special form, the arguments from and to are
    optional, but must both be present or both absent. If they are present,
    inc may optionally be specified as well. These arguments are
    grouped with the argument var into a list, to distinguish them
    from body, which includes all remaining elements of the form.

---

Previous: [A Sample Function Description](#A-Sample-Function-Description), Up: [Format of Descriptions](#Format-of-Descriptions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.3.7.2 A Sample Variable Description [¶](#A-Sample-Variable-Description-1)

A *variable* is a name that can be *bound* (or *set*) to
an object. The object to which a variable is bound is called a
*value*; we say also that variable holds that value.
Although nearly all variables can be set by the user, certain
variables exist specifically so that users can change them; these are
called *user options*. Ordinary variables and user options are
described using a format like that for functions, except that there
are no arguments.

Here is a description of the imaginary `electric-future-map`
variable.

Variable: **electric-future-map** [¶](#index-electric_002dfuture_002dmap)
:   The value of this variable is a full keymap used by Electric Command
    Future mode. The functions in this map allow you to edit commands you
    have not yet thought about executing.

User option descriptions have the same format, but ‘`Variable`’
is replaced by ‘`User Option`’.

---

Next: [Acknowledgments](#Acknowledgments), Previous: [Conventions](#Conventions), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
