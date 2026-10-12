# 

* guile 3.0.11guile 3.0.11

  + [3.0.11](../../3.0.11/en/guile.html)
  + [3.0.9](../../3.0.9/en/guile.html)
  + [2.2.7](../../2.2.7/en/guile.html)
  + [2.2.4](../../2.2.4/en/guile.html)
  + [2.0.14](../../2.0.14/en/guile.html)
  + [multiple pages](html_node)
  + [PDF](guile.pdf)

Next: [Preface](#Preface), Previous: [(dir)](dir.html#Top), Up: [(dir)](/en/guile)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]# The Guile Reference Manual [¶](#The-Guile-Reference-Manual)

This manual documents Guile version 3.0.11.

Copyright (C) 1996-1997, 2000-2005, 2009-2023 Free Software Foundation,
Inc.   
Copyright (C) 2021 Maxime Devos  
Copyright (C) 2024 Tomas Volf

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with no
Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts. A
copy of the license is included in the section entitled “GNU Free
Documentation License.”

Additionally, the documentation of the SRFI 64 module is adapted from
its specification text, which is made available under the following
Expat license:

> Permission is hereby granted, free of charge, to any person obtaining a
> copy of this software and associated documentation files (the
> "Software"), to deal in the Software without restriction, including
> without limitation the rights to use, copy, modify, merge, publish,
> distribute, sublicense, and/or sell copies of the Software, and to
> permit persons to whom the Software is furnished to do so, subject to
> the following conditions:
>
> The above copyright notice and this permission notice shall be included
> in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
> OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
> MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
> NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
> LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
> OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
> WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Additionally, the documentation of the SRFI 197 module is adapted from
its specification text, which is made available under the following
MIT license:

> Permission is hereby granted, free of charge, to any person obtaining a
> copy of this software and associated documentation files (the
> "Software"), to deal in the Software without restriction, including
> without limitation the rights to use, copy, modify, merge, publish,
> distribute, sublicense, and/or sell copies of the Software, and to
> permit persons to whom the Software is furnished to do so, subject to
> the following conditions:
>
> The above copyright notice and this permission notice shall be included
> in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
> OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
> MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
> NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
> LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
> OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
> WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

  ## Table of Contents

* [Preface](#Preface)
  + [Contributors to this Manual](#Contributors)
  + [The Guile License](#Guile-License)
* [1 Introduction](#Introduction)
  + [1.1 Guile and Scheme](#Guile-and-Scheme)
  + [1.2 Combining with C Code](#Combining-with-C)
  + [1.3 Guile and the GNU Project](#Guile-and-the-GNU-Project)
  + [1.4 Interactive Programming](#Interactive-Programming)
  + [1.5 Supporting Multiple Languages](#Supporting-Multiple-Languages)
  + [1.6 Obtaining and Installing Guile](#Obtaining-and-Installing-Guile)
  + [1.7 Organisation of this Manual](#Organisation-of-this-Manual)
  + [1.8 Typographical Conventions](#Typographical-Conventions)
* [2 Hello Guile!](#Hello-Guile_0021)
  + [2.1 Running Guile Interactively](#Running-Guile-Interactively)
  + [2.2 Running Guile Scripts](#Running-Guile-Scripts)
  + [2.3 Linking Guile into Programs](#Linking-Guile-into-Programs)
  + [2.4 Writing Guile Extensions](#Writing-Guile-Extensions)
  + [2.5 Using the Guile Module System](#Using-the-Guile-Module-System)
    - [2.5.1 Using Modules](#Using-Modules)
    - [2.5.2 Writing new Modules](#Writing-new-Modules)
    - [2.5.3 Putting Extensions into Modules](#Putting-Extensions-into-Modules)
  + [2.6 Reporting Bugs](#Reporting-Bugs)
* [3 Hello Scheme!](#Hello-Scheme_0021)
  + [3.1 Data Types, Values and Variables](#About-Data)
    - [3.1.1 Latent Typing](#Latent-Typing)
    - [3.1.2 Values and Variables](#Values-and-Variables)
    - [3.1.3 Defining and Setting Variables](#Definition)
  + [3.2 The Representation and Use of Procedures](#About-Procedures)
    - [3.2.1 Procedures as Values](#Procedures-as-Values)
    - [3.2.2 Simple Procedure Invocation](#Simple-Invocation)
    - [3.2.3 Creating and Using a New Procedure](#Creating-a-Procedure)
    - [3.2.4 Lambda Alternatives](#Lambda-Alternatives)
  + [3.3 Expressions and Evaluation](#About-Expressions)
    - [3.3.1 Evaluating Expressions and Executing Programs](#Evaluating)
      * [3.3.1.1 Evaluating Literal Data](#Eval-Literal)
      * [3.3.1.2 Evaluating a Variable Reference](#Eval-Variable)
      * [3.3.1.3 Evaluating a Procedure Invocation Expression](#Eval-Procedure)
      * [3.3.1.4 Evaluating Special Syntactic Expressions](#Eval-Special)
    - [3.3.2 Tail calls](#Tail-Calls)
    - [3.3.3 Using the Guile REPL](#The-REPL)
    - [3.3.4 Summary of Common Syntax](#Syntax-Summary)
  + [3.4 The Concept of Closure](#About-Closure)
    - [3.4.1 Names, Locations, Values and Environments](#About-Environments)
    - [3.4.2 Local Variables and Environments](#Local-Variables)
    - [3.4.3 Environment Chaining](#Chaining)
    - [3.4.4 Lexical Scope](#Lexical-Scope)
      * [3.4.4.1 An Example of Non-Lexical Scoping](#Scoping-Example)
    - [3.4.5 Closure](#Closure)
    - [3.4.6 Example 1: A Serial Number Generator](#Serial-Number)
    - [3.4.7 Example 2: A Shared Persistent Variable](#Shared-Variable)
    - [3.4.8 Example 3: The Callback Closure Problem](#Callback-Closure)
    - [3.4.9 Example 4: Object Orientation](#OO-Closure)
  + [3.5 Further Reading](#Further-Reading)
* [4 Programming in Scheme](#Programming-in-Scheme)
  + [4.1 Guile’s Implementation of Scheme](#Guile-Scheme)
  + [4.2 Invoking Guile](#Invoking-Guile)
    - [4.2.1 Command-line Options](#Command_002dline-Options)
    - [4.2.2 Environment Variables](#Environment-Variables)
  + [4.3 Guile Scripting](#Guile-Scripting)
    - [4.3.1 The Top of a Script File](#The-Top-of-a-Script-File)
    - [4.3.2 The Meta Switch](#The-Meta-Switch)
    - [4.3.3 Command Line Handling](#Command-Line-Handling)
    - [4.3.4 Scripting Examples](#Scripting-Examples)
  + [4.4 Using Guile Interactively](#Using-Guile-Interactively)
    - [4.4.1 The Init File, `~/.guile`](#Init-File)
    - [4.4.2 Readline](#Readline)
    - [4.4.3 Value History](#Value-History)
    - [4.4.4 REPL Commands](#REPL-Commands)
      * [4.4.4.1 Help Commands](#Help-Commands)
      * [4.4.4.2 Module Commands](#Module-Commands)
      * [4.4.4.3 Language Commands](#Language-Commands)
      * [4.4.4.4 Compile Commands](#Compile-Commands)
      * [4.4.4.5 Profile Commands](#Profile-Commands)
      * [4.4.4.6 Debug Commands](#Debug-Commands)
      * [4.4.4.7 Inspect Commands](#Inspect-Commands)
      * [4.4.4.8 System Commands](#System-Commands)
    - [4.4.5 Error Handling](#Error-Handling)
    - [4.4.6 Interactive Debugging](#Interactive-Debugging)
  + [4.5 Using Guile in Emacs](#Using-Guile-in-Emacs)
  + [4.6 Using Guile Tools](#Using-Guile-Tools)
  + [4.7 Installing Site Packages](#Installing-Site-Packages)
  + [4.8 Distributing Guile Code](#Distributing-Guile-Code)
* [5 Programming in C](#Programming-in-C)
  + [5.1 Parallel Installations](#Parallel-Installations)
  + [5.2 Linking Programs With Guile](#Linking-Programs-With-Guile)
    - [5.2.1 Guile Initialization Functions](#Guile-Initialization-Functions)
    - [5.2.2 A Sample Guile Main Program](#A-Sample-Guile-Main-Program)
    - [5.2.3 Building the Example with Make](#Building-the-Example-with-Make)
    - [5.2.4 Building the Example with Autoconf](#Building-the-Example-with-Autoconf)
  + [5.3 Linking Guile with Libraries](#Linking-Guile-with-Libraries)
    - [5.3.1 A Sample Guile Extension](#A-Sample-Guile-Extension)
  + [5.4 General concepts for using libguile](#General-Libguile-Concepts)
    - [5.4.1 Dynamic Types](#Dynamic-Types)
    - [5.4.2 Garbage Collection](#Garbage-Collection)
    - [5.4.3 Control Flow](#Control-Flow)
    - [5.4.4 Asynchronous Signals](#Asynchronous-Signals)
    - [5.4.5 Multi-Threading](#Multi_002dThreading)
  + [5.5 Defining New Foreign Object Types](#Defining-New-Foreign-Object-Types)
    - [5.5.1 Defining Foreign Object Types](#Defining-Foreign-Object-Types)
    - [5.5.2 Creating Foreign Objects](#Creating-Foreign-Objects)
    - [5.5.3 Type Checking of Foreign Objects](#Type-Checking-of-Foreign-Objects)
    - [5.5.4 Foreign Object Memory Management](#Foreign-Object-Memory-Management)
    - [5.5.5 Foreign Objects and Scheme](#Foreign-Objects-and-Scheme)
  + [5.6 Function Snarfing](#Function-Snarfing)
  + [5.7 An Overview of Guile Programming](#Programming-Overview)
    - [5.7.1 How One Might Extend Dia Using Guile](#Extending-Dia)
      * [5.7.1.1 Deciding Why You Want to Add Guile](#Dia-Objective)
      * [5.7.1.2 Four Steps Required to Add Guile](#Dia-Steps)
      * [5.7.1.3 How to Represent Dia Data in Scheme](#Dia-Objects)
      * [5.7.1.4 Writing Guile Primitives for Dia](#Dia-Primitives)
      * [5.7.1.5 Providing a Hook for the Evaluation of Scheme Code](#Dia-Hook)
      * [5.7.1.6 Top-level Structure of Guile-enabled Dia](#Dia-Structure)
      * [5.7.1.7 Going Further with Dia and Guile](#Dia-Advanced)
    - [5.7.2 Why Scheme is More Hackable Than C](#Scheme-vs-C)
    - [5.7.3 Example: Using Guile for an Application Testbed](#Testbed-Example)
    - [5.7.4 A Choice of Programming Options](#Programming-Options)
      * [5.7.4.1 What Functionality is Already Available?](#Available-Functionality)
      * [5.7.4.2 Functional and Performance Constraints](#Basic-Constraints)
      * [5.7.4.3 Your Preferred Programming Style](#Style-Choices)
      * [5.7.4.4 What Controls Program Execution?](#Program-Control)
    - [5.7.5 How About Application Users?](#User-Programming)
  + [5.8 Autoconf Support](#Autoconf-Support)
    - [5.8.1 Autoconf Background](#Autoconf-Background)
    - [5.8.2 Autoconf Macros](#Autoconf-Macros)
    - [5.8.3 Using Autoconf Macros](#Using-Autoconf-Macros)
* [6 API Reference](#API-Reference)
  + [6.1 Overview of the Guile API](#API-Overview)
  + [6.2 Deprecation](#Deprecation)
  + [6.3 The SCM Type](#The-SCM-Type)
  + [6.4 Initializing Guile](#Initialization)
  + [6.5 Snarfing Macros](#Snarfing-Macros)
  + [6.6 Data Types](#Data-Types)
    - [6.6.1 Booleans](#Booleans)
    - [6.6.2 Numerical data types](#Numbers)
      * [6.6.2.1 Scheme’s Numerical “Tower”](#Numerical-Tower)
      * [6.6.2.2 Integers](#Integers)
      * [6.6.2.3 Real and Rational Numbers](#Reals-and-Rationals)
      * [6.6.2.4 Complex Numbers](#Complex-Numbers)
      * [6.6.2.5 Exact and Inexact Numbers](#Exactness)
      * [6.6.2.6 Read Syntax for Numerical Data](#Number-Syntax)
      * [6.6.2.7 Operations on Integer Values](#Integer-Operations)
      * [6.6.2.8 Comparison Predicates](#Comparison)
      * [6.6.2.9 Converting Numbers To and From Strings](#Conversion)
      * [6.6.2.10 Complex Number Operations](#Complex)
      * [6.6.2.11 Arithmetic Functions](#Arithmetic)
      * [6.6.2.12 Scientific Functions](#Scientific)
      * [6.6.2.13 Bitwise Operations](#Bitwise-Operations)
      * [6.6.2.14 Random Number Generation](#Random)
    - [6.6.3 Characters](#Characters)
    - [6.6.4 Character Sets](#Character-Sets)
      * [6.6.4.1 Character Set Predicates/Comparison](#Character-Set-Predicates_002fComparison)
      * [6.6.4.2 Iterating Over Character Sets](#Iterating-Over-Character-Sets)
      * [6.6.4.3 Creating Character Sets](#Creating-Character-Sets)
      * [6.6.4.4 Querying Character Sets](#Querying-Character-Sets)
      * [6.6.4.5 Character-Set Algebra](#Character_002dSet-Algebra)
      * [6.6.4.6 Standard Character Sets](#Standard-Character-Sets)
    - [6.6.5 Strings](#Strings)
      * [6.6.5.1 String Read Syntax](#String-Syntax)
      * [6.6.5.2 String Predicates](#String-Predicates)
      * [6.6.5.3 String Constructors](#String-Constructors)
      * [6.6.5.4 List/String conversion](#List_002fString-Conversion)
      * [6.6.5.5 String Selection](#String-Selection)
      * [6.6.5.6 String Modification](#String-Modification)
      * [6.6.5.7 String Comparison](#String-Comparison)
      * [6.6.5.8 String Searching](#String-Searching)
      * [6.6.5.9 Alphabetic Case Mapping](#Alphabetic-Case-Mapping)
      * [6.6.5.10 Reversing and Appending Strings](#Reversing-and-Appending-Strings)
      * [6.6.5.11 Mapping, Folding, and Unfolding](#Mapping-Folding-and-Unfolding)
      * [6.6.5.12 Miscellaneous String Operations](#Miscellaneous-String-Operations)
      * [6.6.5.13 Representing Strings as Bytes](#Representing-Strings-as-Bytes)
      * [6.6.5.14 Conversion to/from C](#Conversion-to_002ffrom-C)
      * [6.6.5.15 String Internals](#String-Internals)
    - [6.6.6 Symbols](#Symbols)
      * [6.6.6.1 Symbols as Discrete Data](#Symbol-Data)
      * [6.6.6.2 Symbols as Lookup Keys](#Symbol-Keys)
      * [6.6.6.3 Symbols as Denoting Variables](#Symbol-Variables)
      * [6.6.6.4 Operations Related to Symbols](#Symbol-Primitives)
      * [6.6.6.5 Extended Read Syntax for Symbols](#Symbol-Read-Syntax)
      * [6.6.6.6 Uninterned Symbols](#Symbol-Uninterned)
    - [6.6.7 Keywords](#Keywords)
      * [6.6.7.1 Why Use Keywords?](#Why-Use-Keywords_003f)
      * [6.6.7.2 Coding With Keywords](#Coding-With-Keywords)
      * [6.6.7.3 Keyword Read Syntax](#Keyword-Read-Syntax)
      * [6.6.7.4 Keyword Procedures](#Keyword-Procedures)
    - [6.6.8 Pairs](#Pairs)
    - [6.6.9 Lists](#Lists)
      * [6.6.9.1 List Read Syntax](#List-Syntax)
      * [6.6.9.2 List Predicates](#List-Predicates)
      * [6.6.9.3 List Constructors](#List-Constructors)
      * [6.6.9.4 List Selection](#List-Selection)
      * [6.6.9.5 Append and Reverse](#Append_002fReverse)
      * [6.6.9.6 List Modification](#List-Modification)
      * [6.6.9.7 List Searching](#List-Searching)
      * [6.6.9.8 List Mapping](#List-Mapping)
    - [6.6.10 Vectors](#Vectors)
      * [6.6.10.1 Read Syntax for Vectors](#Vector-Syntax)
      * [6.6.10.2 Dynamic Vector Creation and Validation](#Vector-Creation)
      * [6.6.10.3 Accessing and Modifying Vector Contents](#Vector-Accessors)
      * [6.6.10.4 Vector Accessing from C](#Vector-Accessing-from-C)
      * [6.6.10.5 Uniform Numeric Vectors](#Uniform-Numeric-Vectors)
    - [6.6.11 Bit Vectors](#Bit-Vectors)
    - [6.6.12 Bytevectors](#Bytevectors)
      * [6.6.12.1 Endianness](#Bytevector-Endianness)
      * [6.6.12.2 Manipulating Bytevectors](#Bytevector-Manipulation)
      * [6.6.12.3 Interpreting Bytevector Contents as Integers](#Bytevectors-as-Integers)
      * [6.6.12.4 Converting Bytevectors to/from Integer Lists](#Bytevectors-and-Integer-Lists)
      * [6.6.12.5 Interpreting Bytevector Contents as Floating Point Numbers](#Bytevectors-as-Floats)
      * [6.6.12.6 Interpreting Bytevector Contents as Unicode Strings](#Bytevectors-as-Strings)
      * [6.6.12.7 Accessing Bytevectors with the Array API](#Bytevectors-as-Arrays)
      * [6.6.12.8 Accessing Bytevectors with the SRFI-4 API](#Bytevectors-as-Uniform-Vectors)
      * [6.6.12.9 Bytevector Procedures in R7RS](#Bytevector-Procedures-in-R7RS)
      * [6.6.12.10 Bytevector Slices](#Bytevector-Slices)
    - [6.6.13 Arrays](#Arrays)
      * [6.6.13.1 Array Syntax](#Array-Syntax)
      * [6.6.13.2 Array Procedures](#Array-Procedures)
      * [6.6.13.3 Shared Arrays](#Shared-Arrays)
      * [6.6.13.4 Arrays as arrays of arrays](#Arrays-as-arrays-of-arrays)
      * [6.6.13.5 Accessing Arrays from C](#Accessing-Arrays-from-C)
    - [6.6.14 VLists](#VLists)
    - [6.6.15 Record Overview](#Record-Overview)
    - [6.6.16 SRFI-9 Records](#SRFI_002d9-Records)
      * [Non-toplevel Record Definitions](#Non_002dtoplevel-Record-Definitions)
      * [Custom Printers](#Custom-Printers)
      * [Functional “Setters”](#Functional-_0060_0060Setters_0027_0027)
    - [6.6.17 Records](#Records)
    - [6.6.18 Structures](#Structures)
      * [6.6.18.1 Vtables](#Vtables)
      * [6.6.18.2 Structure Basics](#Structure-Basics)
      * [6.6.18.3 Vtable Contents](#Vtable-Contents)
      * [6.6.18.4 Meta-Vtables](#Meta_002dVtables)
      * [6.6.18.5 Vtable Example](#Vtable-Example)
    - [6.6.19 Dictionary Types](#Dictionary-Types)
    - [6.6.20 Association Lists](#Association-Lists)
      * [6.6.20.1 Alist Key Equality](#Alist-Key-Equality)
      * [6.6.20.2 Adding or Setting Alist Entries](#Adding-or-Setting-Alist-Entries)
      * [6.6.20.3 Retrieving Alist Entries](#Retrieving-Alist-Entries)
      * [6.6.20.4 Removing Alist Entries](#Removing-Alist-Entries)
      * [6.6.20.5 Sloppy Alist Functions](#Sloppy-Alist-Functions)
      * [6.6.20.6 Alist Example](#Alist-Example)
    - [6.6.21 VList-Based Hash Lists or “VHashes”](#VHashes)
    - [6.6.22 Hash Tables](#Hash-Tables)
      * [6.6.22.1 Hash Table Examples](#Hash-Table-Examples)
      * [6.6.22.2 Hash Table Reference](#Hash-Table-Reference)
    - [6.6.23 Other Types](#Other-Types)
  + [6.7 Procedures](#Procedures)
    - [6.7.1 Lambda: Basic Procedure Creation](#Lambda)
    - [6.7.2 Primitive Procedures](#Primitive-Procedures)
    - [6.7.3 Compiled Procedures](#Compiled-Procedures)
    - [6.7.4 Optional Arguments](#Optional-Arguments)
      * [6.7.4.1 lambda\* and define\*.](#lambda_002a-and-define_002a)
      * [6.7.4.2 (ice-9 optargs)](#ice_002d9-optargs)
    - [6.7.5 Case-lambda](#Case_002dlambda)
    - [6.7.6 Higher-Order Functions](#Higher_002dOrder-Functions)
    - [6.7.7 Procedure Properties and Meta-information](#Procedure-Properties)
    - [6.7.8 Procedures with Setters](#Procedures-with-Setters)
    - [6.7.9 Inlinable Procedures](#Inlinable-Procedures)
  + [6.8 Macros](#Macros)
    - [6.8.1 Defining Macros](#Defining-Macros)
    - [6.8.2 Syntax-rules Macros](#Syntax-Rules)
      * [6.8.2.1 Patterns](#Patterns)
      * [6.8.2.2 Hygiene](#Hygiene)
      * [6.8.2.3 Shorthands](#Shorthands)
      * [6.8.2.4 Reporting Syntax Errors in Macros](#Reporting-Syntax-Errors-in-Macros)
      * [6.8.2.5 Specifying a Custom Ellipsis Identifier](#Specifying-a-Custom-Ellipsis-Identifier)
      * [6.8.2.6 Further Information](#Further-Information)
    - [6.8.3 Support for the `syntax-case` System](#Syntax-Case)
      * [6.8.3.1 Why `syntax-case`?](#Why-syntax_002dcase_003f)
      * [6.8.3.2 Custom Ellipsis Identifiers for syntax-case Macros](#Custom-Ellipsis-Identifiers-for-syntax_002dcase-Macros)
      * [6.8.3.3 Syntax objects can be data too](#Syntax-objects-can-be-data-too)
    - [6.8.4 Syntax Transformer Helpers](#Syntax-Transformer-Helpers)
    - [6.8.5 Lisp-style Macro Definitions](#Defmacros)
    - [6.8.6 Identifier Macros](#Identifier-Macros)
    - [6.8.7 Syntax Parameters](#Syntax-Parameters)
    - [6.8.8 Eval-when](#Eval-When)
    - [6.8.9 Macro Expansion](#Macro-Expansion)
    - [6.8.10 Hygiene and the Top-Level](#Hygiene-and-the-Top_002dLevel)
    - [6.8.11 Internal Macros](#Internal-Macros)
  + [6.9 General Utility Functions](#Utility-Functions)
    - [6.9.1 Equality](#Equality)
    - [6.9.2 Object Properties](#Object-Properties)
    - [6.9.3 Sorting](#Sorting)
    - [6.9.4 Copying Deep Structures](#Copying)
    - [6.9.5 General String Conversion](#General-Conversion)
    - [6.9.6 Hooks](#Hooks)
      * [6.9.6.1 Hook Usage by Example](#Hook-Example)
      * [6.9.6.2 Hook Reference](#Hook-Reference)
      * [6.9.6.3 Hooks For C Code.](#C-Hooks)
      * [6.9.6.4 Hooks for Garbage Collection](#GC-Hooks)
      * [6.9.6.5 Hooks into the Guile REPL](#REPL-Hooks)
  + [6.10 Definitions and Variable Bindings](#Binding-Constructs)
    - [6.10.1 Top Level Variable Definitions](#Top-Level)
    - [6.10.2 Local Variable Bindings](#Local-Bindings)
    - [6.10.3 Internal definitions](#Internal-Definitions)
    - [6.10.4 Querying variable bindings](#Binding-Reflection)
    - [6.10.5 Binding multiple return values](#Binding-Multiple-Values)
  + [6.11 Controlling the Flow of Program Execution](#Control-Mechanisms)
    - [6.11.1 Sequencing and Splicing](#begin)
    - [6.11.2 Simple Conditional Evaluation](#Conditionals)
    - [6.11.3 Conditional Evaluation of a Sequence of Expressions](#and-or)
    - [6.11.4 Iteration mechanisms](#while-do)
    - [6.11.5 Prompts](#Prompts)
      * [6.11.5.1 Prompt Primitives](#Prompt-Primitives)
      * [6.11.5.2 Shift, Reset, and All That](#Shift-and-Reset)
    - [6.11.6 Continuations](#Continuations)
    - [6.11.7 Returning and Accepting Multiple Values](#Multiple-Values)
    - [6.11.8 Exceptions](#Exceptions)
      * [6.11.8.1 Exception Objects](#Exception-Objects)
      * [6.11.8.2 Raising and Handling Exceptions](#Raising-and-Handling-Exceptions)
      * [6.11.8.3 Throw and Catch](#Throw-and-Catch)
      * [6.11.8.4 Exceptions and C](#Exceptions-and-C)
    - [6.11.9 Procedures for Signaling Errors](#Error-Reporting)
    - [6.11.10 Dynamic Wind](#Dynamic-Wind)
    - [6.11.11 Fluids and Dynamic States](#Fluids-and-Dynamic-States)
    - [6.11.12 Parameters](#Parameters)
    - [6.11.13 How to Handle Errors](#Handling-Errors)
      * [6.11.13.1 C Support](#C-Support)
      * [6.11.13.2 Signaling Type Errors](#Signaling-Type-Errors)
    - [6.11.14 Continuation Barriers](#Continuation-Barriers)
  + [6.12 Input and Output](#Input-and-Output)
    - [6.12.1 Ports](#Ports)
    - [6.12.2 Binary I/O](#Binary-I_002fO)
    - [6.12.3 Encoding](#Encoding)
    - [6.12.4 Textual I/O](#Textual-I_002fO)
    - [6.12.5 Simple Textual Output](#Simple-Output)
    - [6.12.6 Buffering](#Buffering)
    - [6.12.7 Random Access](#Random-Access)
    - [6.12.8 Line Oriented and Delimited Text](#Line_002fDelimited)
    - [6.12.9 Default Ports for Input, Output and Errors](#Default-Ports)
    - [6.12.10 Types of Port](#Port-Types)
      * [6.12.10.1 File Ports](#File-Ports)
      * [6.12.10.2 Bytevector Ports](#Bytevector-Ports)
      * [6.12.10.3 String Ports](#String-Ports)
      * [6.12.10.4 Custom Ports](#Custom-Ports)
      * [6.12.10.5 Soft Ports](#Soft-Ports)
      * [6.12.10.6 Void Ports](#Void-Ports)
      * [6.12.10.7 Low-Level Custom Ports](#Low_002dLevel-Custom-Ports)
      * [6.12.10.8 Low-Level Custom Ports in C](#Low_002dLevel-Custom-Ports-in-C)
    - [6.12.11 Venerable Port Interfaces](#Venerable-Port-Interfaces)
    - [6.12.12 Using Ports from C](#Using-Ports-from-C)
    - [6.12.13 Non-Blocking I/O](#Non_002dBlocking-I_002fO)
    - [6.12.14 Handling of Unicode Byte Order Marks](#BOM-Handling)
  + [6.13 Regular Expressions](#Regular-Expressions)
    - [6.13.1 Regexp Functions](#Regexp-Functions)
    - [6.13.2 Match Structures](#Match-Structures)
    - [6.13.3 Backslash Escapes](#Backslash-Escapes)
  + [6.14 LALR(1) Parsing](#LALR_00281_0029-Parsing)
  + [6.15 PEG Parsing](#PEG-Parsing)
    - [6.15.1 PEG Syntax Reference](#PEG-Syntax-Reference)
    - [6.15.2 PEG API Reference](#PEG-API-Reference)
    - [6.15.3 PEG Tutorial](#PEG-Tutorial)
    - [6.15.4 PEG Internals](#PEG-Internals)
  + [6.16 Reading and Evaluating Scheme Code](#Read_002fLoad_002fEval_002fCompile)
    - [6.16.1 Scheme Syntax: Standard and Guile Extensions](#Scheme-Syntax)
      * [6.16.1.1 Expression Syntax](#Expression-Syntax)
      * [6.16.1.2 Comments](#Comments)
      * [6.16.1.3 Block Comments](#Block-Comments)
      * [6.16.1.4 Case Sensitivity](#Case-Sensitivity)
      * [6.16.1.5 Keyword Syntax](#Keyword-Syntax)
      * [6.16.1.6 Reader Extensions](#Reader-Extensions)
    - [6.16.2 Reading Scheme Code](#Scheme-Read)
    - [6.16.3 Reading Scheme Code, For the Compiler](#Annotated-Scheme-Read)
    - [6.16.4 Writing Scheme Values](#Scheme-Write)
    - [6.16.5 Procedures for On the Fly Evaluation](#Fly-Evaluation)
    - [6.16.6 Compiling Scheme Code](#Compilation)
    - [6.16.7 Loading Scheme Code from File](#Loading)
    - [6.16.8 Load Paths](#Load-Paths)
    - [6.16.9 Character Encoding of Source Files](#Character-Encoding-of-Source-Files)
    - [6.16.10 Delayed Evaluation](#Delayed-Evaluation)
    - [6.16.11 Local Evaluation](#Local-Evaluation)
    - [6.16.12 Local Inclusion](#Local-Inclusion)
    - [6.16.13 Sandboxed Evaluation](#Sandboxed-Evaluation)
    - [6.16.14 REPL Servers](#REPL-Servers)
    - [6.16.15 Cooperative REPL Servers](#Cooperative-REPL-Servers)
  + [6.17 Memory Management and Garbage Collection](#Memory-Management)
    - [6.17.1 Function related to Garbage Collection](#Garbage-Collection-Functions)
    - [6.17.2 Memory Blocks](#Memory-Blocks)
    - [6.17.3 Weak References](#Weak-References)
      * [6.17.3.1 Weak hash tables](#Weak-hash-tables)
      * [6.17.3.2 Weak vectors](#Weak-vectors)
    - [6.17.4 Guardians](#Guardians)
  + [6.18 Modules](#Modules)
    - [6.18.1 General Information about Modules](#General-Information-about-Modules)
    - [6.18.2 Using Guile Modules](#Using-Guile-Modules)
    - [6.18.3 Creating Guile Modules](#Creating-Guile-Modules)
    - [6.18.4 Modules and the File System](#Modules-and-the-File-System)
    - [6.18.5 R6RS Version References](#R6RS-Version-References)
    - [6.18.6 R6RS Libraries](#R6RS-Libraries)
    - [6.18.7 Variables](#Variables)
    - [6.18.8 Module System Reflection](#Module-System-Reflection)
    - [6.18.9 Declarative Modules](#Declarative-Modules)
    - [6.18.10 Accessing Modules from C](#Accessing-Modules-from-C)
    - [6.18.11 provide and require](#provide-and-require)
    - [6.18.12 Environments](#Environments)
  + [6.19 Foreign Function Interface](#Foreign-Function-Interface)
    - [6.19.1 Foreign Libraries](#Foreign-Libraries)
    - [6.19.2 Foreign Extensions](#Foreign-Extensions)
    - [6.19.3 Foreign Pointers](#Foreign-Pointers)
    - [6.19.4 Foreign Types](#Foreign-Types)
    - [6.19.5 Foreign Functions](#Foreign-Functions)
    - [6.19.6 Void Pointers and Byte Access](#Void-Pointers-and-Byte-Access)
    - [6.19.7 Foreign Structs](#Foreign-Structs)
    - [6.19.8 More Foreign Functions](#More-Foreign-Functions)
  + [6.20 Foreign Objects](#Foreign-Objects)
  + [6.21 Smobs](#Smobs)
  + [6.22 Threads, Mutexes, Asyncs and Dynamic Roots](#Scheduling)
    - [6.22.1 Threads](#Threads)
    - [6.22.2 Thread-Local Variables](#Thread-Local-Variables)
    - [6.22.3 Asynchronous Interrupts](#Asyncs)
    - [6.22.4 Atomics](#Atomics)
    - [6.22.5 Mutexes and Condition Variables](#Mutexes-and-Condition-Variables)
    - [6.22.6 Blocking in Guile Mode](#Blocking)
    - [6.22.7 Futures](#Futures)
    - [6.22.8 Parallel forms](#Parallel-Forms)
  + [6.23 Configuration, Features and Runtime Options](#Options-and-Config)
    - [6.23.1 Configuration, Build and Installation](#Build-Config)
    - [6.23.2 Feature Tracking](#Feature-Tracking)
      * [6.23.2.1 Feature Manipulation](#Feature-Manipulation)
      * [6.23.2.2 Common Feature Symbols](#Common-Feature-Symbols)
    - [6.23.3 Runtime Options](#Runtime-Options)
      * [6.23.3.1 Examples of option use](#Examples-of-option-use)
  + [6.24 Support for Other Languages](#Other-Languages)
    - [6.24.1 Using Other Languages](#Using-Other-Languages)
    - [6.24.2 Emacs Lisp](#Emacs-Lisp)
      * [6.24.2.1 Nil](#Nil)
      * [6.24.2.2 Dynamic Binding](#Dynamic-Binding)
      * [6.24.2.3 Other Elisp Features](#Other-Elisp-Features)
    - [6.24.3 ECMAScript](#ECMAScript)
  + [6.25 Support for Internationalization](#Internationalization)
    - [6.25.1 Internationalization with Guile](#i18n-Introduction)
    - [6.25.2 Text Collation](#Text-Collation)
    - [6.25.3 Character Case Mapping](#Character-Case-Mapping)
    - [6.25.4 Number Input and Output](#Number-Input-and-Output)
    - [6.25.5 Accessing Locale Information](#Accessing-Locale-Information)
    - [6.25.6 Gettext Support](#Gettext-Support)
  + [6.26 Debugging Infrastructure](#Debugging)
    - [6.26.1 Simple Debugging](#Simple-Debugging)
    - [6.26.2 Evaluation and the Scheme Stack](#Evaluation-Model)
      * [6.26.2.1 Stack Capture](#Stack-Capture)
      * [6.26.2.2 Stacks](#Stacks)
      * [6.26.2.3 Frames](#Frames)
    - [6.26.3 Source Properties](#Source-Properties)
    - [6.26.4 Programmatic Error Handling](#Programmatic-Error-Handling)
      * [6.26.4.1 Catching Exceptions](#Catching-Exceptions)
      * [6.26.4.2 Pre-Unwind Debugging](#Pre_002dUnwind-Debugging)
      * [6.26.4.3 call-with-error-handling](#Standard-Error-Handling)
      * [6.26.4.4 Stack Overflow](#Stack-Overflow)
      * [6.26.4.5 Debug options](#Debug-Options)
    - [6.26.5 Traps](#Traps)
      * [6.26.5.1 VM Hooks](#VM-Hooks)
      * [6.26.5.2 Trap Interface](#Trap-Interface)
      * [6.26.5.3 Low-Level Traps](#Low_002dLevel-Traps)
      * [6.26.5.4 Tracing Traps](#Tracing-Traps)
      * [6.26.5.5 Trap States](#Trap-States)
      * [6.26.5.6 High-Level Traps](#High_002dLevel-Traps)
    - [6.26.6 GDB Support](#GDB-Support)
  + [6.27 Code Coverage Reports](#Code-Coverage)
* [7 Guile Modules](#Guile-Modules)
  + [7.1 SLIB](#SLIB)
    - [7.1.1 SLIB installation](#SLIB-installation)
    - [7.1.2 JACAL](#JACAL)
  + [7.2 POSIX System Calls and Networking](#POSIX)
    - [7.2.1 POSIX Interface Conventions](#Conventions)
    - [7.2.2 Ports and File Descriptors](#Ports-and-File-Descriptors)
    - [7.2.3 File System](#File-System)
    - [7.2.4 User Information](#User-Information)
    - [7.2.5 Time](#Time)
    - [7.2.6 Runtime Environment](#Runtime-Environment)
    - [7.2.7 Processes](#Processes)
    - [7.2.8 Signals](#Signals)
    - [7.2.9 Terminals and Ptys](#Terminals-and-Ptys)
    - [7.2.10 Pipes](#Pipes)
    - [7.2.11 Networking](#Networking)
      * [7.2.11.1 Network Address Conversion](#Network-Address-Conversion)
      * [7.2.11.2 Network Databases](#Network-Databases)
      * [7.2.11.3 Network Socket Address](#Network-Socket-Address)
      * [7.2.11.4 Network Sockets and Communication](#Network-Sockets-and-Communication)
      * [7.2.11.5 Network Socket Examples](#Internet-Socket-Examples)
    - [7.2.12 System Identification](#System-Identification)
    - [7.2.13 Locales](#Locales)
    - [7.2.14 Encryption](#Encryption)
  + [7.3 HTTP, the Web, and All That](#Web)
    - [7.3.1 Types and the Web](#Types-and-the-Web)
    - [7.3.2 Universal Resource Identifiers](#URIs)
    - [7.3.3 The Hyper-Text Transfer Protocol](#HTTP)
    - [7.3.4 HTTP Headers](#HTTP-Headers)
      * [7.3.4.1 HTTP Header Types](#HTTP-Header-Types)
      * [7.3.4.2 General Headers](#General-Headers)
      * [7.3.4.3 Entity Headers](#Entity-Headers)
      * [7.3.4.4 Request Headers](#Request-Headers)
      * [7.3.4.5 Response Headers](#Response-Headers)
    - [7.3.5 Transfer Codings](#Transfer-Codings)
    - [7.3.6 HTTP Requests](#Requests)
      * [7.3.6.1 An Important Note on Character Sets](#An-Important-Note-on-Character-Sets)
      * [7.3.6.2 Request API](#Request-API)
    - [7.3.7 HTTP Responses](#Responses)
    - [7.3.8 Web Client](#Web-Client)
    - [7.3.9 Web Server](#Web-Server)
    - [7.3.10 Web Examples](#Web-Examples)
      * [7.3.10.1 Hello, World!](#Hello_002c-World_0021)
      * [7.3.10.2 Inspecting the Request](#Inspecting-the-Request)
      * [7.3.10.3 Higher-Level Interfaces](#Higher_002dLevel-Interfaces)
      * [7.3.10.4 Conclusion](#Conclusion)
  + [7.4 The (ice-9 getopt-long) Module](#getopt_002dlong)
    - [7.4.1 A Short getopt-long Example](#getopt_002dlong-Example)
    - [7.4.2 How to Write an Option Specification](#Option-Specification)
    - [7.4.3 Expected Command Line Format](#Command-Line-Format)
    - [7.4.4 Reference Documentation for `getopt-long`](#getopt_002dlong-Reference)
    - [7.4.5 Reference Documentation for `option-ref`](#option_002dref-Reference)
  + [7.5 SRFI Support Modules](#SRFI-Support)
    - [7.5.1 About SRFI Usage](#About-SRFI-Usage)
    - [7.5.2 SRFI-0 - cond-expand](#SRFI_002d0)
    - [7.5.3 SRFI-1 - List library](#SRFI_002d1)
      * [7.5.3.1 Constructors](#SRFI_002d1-Constructors)
      * [7.5.3.2 Predicates](#SRFI_002d1-Predicates)
      * [7.5.3.3 Selectors](#SRFI_002d1-Selectors)
      * [7.5.3.4 Length, Append, Concatenate, etc.](#SRFI_002d1-Length-Append-etc)
      * [7.5.3.5 Fold, Unfold & Map](#SRFI_002d1-Fold-and-Map)
      * [7.5.3.6 Filtering and Partitioning](#SRFI_002d1-Filtering-and-Partitioning)
      * [7.5.3.7 Searching](#SRFI_002d1-Searching)
      * [7.5.3.8 Deleting](#SRFI_002d1-Deleting)
      * [7.5.3.9 Association Lists](#SRFI_002d1-Association-Lists)
      * [7.5.3.10 Set Operations on Lists](#SRFI_002d1-Set-Operations)
    - [7.5.4 SRFI-2 - and-let\*](#SRFI_002d2)
    - [7.5.5 SRFI-4 - Homogeneous numeric vector datatypes](#SRFI_002d4)
      * [7.5.5.1 SRFI-4 - Overview](#SRFI_002d4-Overview)
      * [7.5.5.2 SRFI-4 - API](#SRFI_002d4-API)
      * [7.5.5.3 SRFI-4 - Relation to bytevectors](#SRFI_002d4-and-Bytevectors)
      * [7.5.5.4 SRFI-4 - Guile extensions](#SRFI_002d4-Extensions)
    - [7.5.6 SRFI-6 - Basic String Ports](#SRFI_002d6)
    - [7.5.7 SRFI-8 - receive](#SRFI_002d8)
    - [7.5.8 SRFI-9 - define-record-type](#SRFI_002d9)
    - [7.5.9 SRFI-10 - Hash-Comma Reader Extension](#SRFI_002d10)
    - [7.5.10 SRFI-11 - let-values](#SRFI_002d11)
    - [7.5.11 SRFI-13 - String Library](#SRFI_002d13)
    - [7.5.12 SRFI-14 - Character-set Library](#SRFI_002d14)
    - [7.5.13 SRFI-16 - case-lambda](#SRFI_002d16)
    - [7.5.14 SRFI-17 - Generalized set!](#SRFI_002d17)
    - [7.5.15 SRFI-18 - Multithreading support](#SRFI_002d18)
      * [7.5.15.1 SRFI-18 Threads](#SRFI_002d18-Threads)
      * [7.5.15.2 SRFI-18 Mutexes](#SRFI_002d18-Mutexes)
      * [7.5.15.3 SRFI-18 Condition variables](#SRFI_002d18-Condition-variables)
      * [7.5.15.4 SRFI-18 Time](#SRFI_002d18-Time)
      * [7.5.15.5 SRFI-18 Exceptions](#SRFI_002d18-Exceptions)
    - [7.5.16 SRFI-19 - Time/Date Library](#SRFI_002d19)
      * [7.5.16.1 SRFI-19 Introduction](#SRFI_002d19-Introduction)
      * [7.5.16.2 SRFI-19 Time](#SRFI_002d19-Time)
      * [7.5.16.3 SRFI-19 Date](#SRFI_002d19-Date)
      * [7.5.16.4 SRFI-19 Time/Date conversions](#SRFI_002d19-Time_002fDate-conversions)
      * [7.5.16.5 SRFI-19 Date to string](#SRFI_002d19-Date-to-string)
      * [7.5.16.6 SRFI-19 String to date](#SRFI_002d19-String-to-date)
    - [7.5.17 SRFI-23 - Error Reporting](#SRFI_002d23)
    - [7.5.18 SRFI-26 - specializing parameters](#SRFI_002d26)
    - [7.5.19 SRFI-27 - Sources of Random Bits](#SRFI_002d27)
      * [7.5.19.1 The Default Random Source](#SRFI_002d27-Default-Random-Source)
      * [7.5.19.2 Random Sources](#SRFI_002d27-Random-Sources)
      * [7.5.19.3 Obtaining random number generator procedures](#SRFI_002d27-Random-Number-Generators)
    - [7.5.20 SRFI-28 - Basic Format Strings](#SRFI_002d28)
    - [7.5.21 SRFI-30 - Nested Multi-line Comments](#SRFI_002d30)
    - [7.5.22 SRFI-31 - A special form ‘rec’ for recursive evaluation](#SRFI_002d31)
    - [7.5.23 SRFI-34 - Exception handling for programs](#SRFI_002d34)
    - [7.5.24 SRFI-35 - Conditions](#SRFI_002d35)
    - [7.5.25 SRFI-37 - args-fold](#SRFI_002d37)
    - [7.5.26 SRFI-38 - External Representation for Data With Shared Structure](#SRFI_002d38)
    - [7.5.27 SRFI-39 - Parameters](#SRFI_002d39)
    - [7.5.28 SRFI-41 - Streams](#SRFI_002d41)
      * [7.5.28.1 SRFI-41 Stream Fundamentals](#SRFI_002d41-Stream-Fundamentals)
      * [7.5.28.2 SRFI-41 Stream Primitives](#SRFI_002d41-Stream-Primitives)
      * [7.5.28.3 SRFI-41 Stream Library](#SRFI_002d41-Stream-Library)
    - [7.5.29 SRFI-42 - Eager Comprehensions](#SRFI_002d42)
    - [7.5.30 SRFI-43 - Vector Library](#SRFI_002d43)
      * [7.5.30.1 SRFI-43 Constructors](#SRFI_002d43-Constructors)
      * [7.5.30.2 SRFI-43 Predicates](#SRFI_002d43-Predicates)
      * [7.5.30.3 SRFI-43 Selectors](#SRFI_002d43-Selectors)
      * [7.5.30.4 SRFI-43 Iteration](#SRFI_002d43-Iteration)
      * [7.5.30.5 SRFI-43 Searching](#SRFI_002d43-Searching)
      * [7.5.30.6 SRFI-43 Mutators](#SRFI_002d43-Mutators)
      * [7.5.30.7 SRFI-43 Conversion](#SRFI_002d43-Conversion)
    - [7.5.31 SRFI-45 - Primitives for Expressing Iterative Lazy Algorithms](#SRFI_002d45)
    - [7.5.32 SRFI-46 Basic syntax-rules Extensions](#SRFI_002d46)
    - [7.5.33 SRFI-55 - Requiring Features](#SRFI_002d55)
    - [7.5.34 SRFI-60 - Integers as Bits](#SRFI_002d60)
    - [7.5.35 SRFI-61 - A more general `cond` clause](#SRFI_002d61)
    - [7.5.36 SRFI-62 - S-expression comments.](#SRFI_002d62)
    - [7.5.37 SRFI-64: A Scheme API for Test Suites](#SRFI_002d64)
      * [7.5.37.1 SRFI-64 Abstract](#SRFI_002d64-Abstract)
      * [7.5.37.2 SRFI-64 Rationale](#SRFI_002d64-Rationale)
      * [7.5.37.3 SRFI-64 Writing Basic Test Suites](#SRFI_002d64-Writing-Basic-Test-Suites)
      * [7.5.37.4 SRFI-64 Conditonal Test Suites and Other Advanced Features](#SRFI_002d64-Conditonal-Test-Suites-and-Other-Advanced-Features)
      * [7.5.37.5 SRFI-64 Test Runner](#SRFI_002d64-Test-Runner)
      * [7.5.37.6 SRFI-64 Test Results](#SRFI_002d64-Test-Results)
      * [7.5.37.7 SRFI-64 Writing a New Test Runner](#SRFI_002d64-Writing-a-New-Test-Runner)
    - [7.5.38 SRFI-67 - Compare procedures](#SRFI_002d67)
    - [7.5.39 SRFI-69 - Basic hash tables](#SRFI_002d69)
      * [7.5.39.1 Creating hash tables](#SRFI_002d69-Creating-hash-tables)
      * [7.5.39.2 Accessing table items](#SRFI_002d69-Accessing-table-items)
      * [7.5.39.3 Table properties](#SRFI_002d69-Table-properties)
      * [7.5.39.4 Hash table algorithms](#SRFI_002d69-Hash-table-algorithms)
    - [7.5.40 SRFI-71 - Extended let-syntax for multiple values](#SRFI_002d71)
    - [7.5.41 SRFI-87 => in case clauses](#SRFI_002d87)
    - [7.5.42 SRFI-88 Keyword Objects](#SRFI_002d88)
    - [7.5.43 SRFI-98 Accessing environment variables.](#SRFI_002d98)
    - [7.5.44 SRFI-105 Curly-infix expressions.](#SRFI_002d105)
    - [7.5.45 SRFI-111 Boxes.](#SRFI_002d111)
    - [7.5.46 SRFI-119 Wisp: simpler indentation-sensitive Scheme.](#SRFI_002d119)
    - [7.5.47 Transducers](#SRFI_002d171)
      * [7.5.47.1 SRFI-171 General Discussion](#SRFI_002d171-General-Discussion)
      * [7.5.47.2 Applying Transducers](#SRFI_002d171-Applying-Transducers)
      * [7.5.47.3 Reducers](#SRFI_002d171-Reducers)
      * [7.5.47.4 Transducers](#SRFI_002d171-Transducers)
      * [7.5.47.5 Helper functions for writing transducers](#SRFI_002d171-Helpers)
    - [7.5.48 SRFI-197: Pipeline Operators](#SRFI_002d197)
      * [7.5.48.1 Acknowledgements](#Acknowledgements)
    - [7.5.49 SRFI-207 String-notated bytevectors](#SRFI_002d207)
      * [7.5.49.1 External Notation](#SRFI_002d207-External-Notation)
      * [7.5.49.2 Constructors](#SRFI_002d207-Contructors)
      * [7.5.49.3 Conversion](#SRFI_002d207-Conversion)
      * [7.5.49.4 Selection](#SRFI_002d207-Selection)
      * [7.5.49.5 Replacement](#SRFI_002d207-Replacement)
      * [7.5.49.6 Comparison](#SRFI_002d207-Comparison)
      * [7.5.49.7 Searching](#SRFI_002d207-Searching)
      * [7.5.49.8 Joining And Splitting](#SRFI_002d207-Joining-And-Splitting)
      * [7.5.49.9 I/O](#SRFI_002d207-I_002fO)
      * [7.5.49.10 Exceptions](#SRFI_002d207-Exceptions)
      * [7.5.49.11 Acknowledgements](#SRFI_002d207-Acknowledgements)
    - [7.5.50 SRFI-244 - Multiple-value Definitions](#SRFI_002d244)
  + [7.6 R6RS Support](#R6RS-Support)
    - [7.6.1 Incompatibilities with the R6RS](#R6RS-Incompatibilities)
    - [7.6.2 R6RS Standard Libraries](#R6RS-Standard-Libraries)
      * [7.6.2.1 Library Usage](#Library-Usage)
      * [7.6.2.2 rnrs base](#rnrs-base)
      * [7.6.2.3 rnrs unicode](#rnrs-unicode)
      * [7.6.2.4 rnrs bytevectors](#rnrs-bytevectors)
      * [7.6.2.5 rnrs lists](#rnrs-lists)
      * [7.6.2.6 rnrs sorting](#rnrs-sorting)
      * [7.6.2.7 rnrs control](#rnrs-control)
      * [7.6.2.8 R6RS Records](#R6RS-Records)
      * [7.6.2.9 rnrs records syntactic](#rnrs-records-syntactic)
      * [7.6.2.10 rnrs records procedural](#rnrs-records-procedural)
      * [7.6.2.11 rnrs records inspection](#rnrs-records-inspection)
      * [7.6.2.12 rnrs exceptions](#rnrs-exceptions)
      * [7.6.2.13 rnrs conditions](#rnrs-conditions)
      * [7.6.2.14 I/O Conditions](#R6RS-I_002fO-Conditions)
      * [7.6.2.15 Transcoders](#R6RS-Transcoders)
      * [7.6.2.16 rnrs io ports](#rnrs-io-ports)
      * [7.6.2.17 R6RS File Ports](#R6RS-File-Ports)
      * [7.6.2.18 rnrs io simple](#rnrs-io-simple)
      * [7.6.2.19 rnrs files](#rnrs-files)
      * [7.6.2.20 rnrs programs](#rnrs-programs)
      * [7.6.2.21 rnrs arithmetic fixnums](#rnrs-arithmetic-fixnums)
      * [7.6.2.22 rnrs arithmetic flonums](#rnrs-arithmetic-flonums)
      * [7.6.2.23 rnrs arithmetic bitwise](#rnrs-arithmetic-bitwise)
      * [7.6.2.24 rnrs syntax-case](#rnrs-syntax_002dcase)
      * [7.6.2.25 rnrs hashtables](#rnrs-hashtables)
      * [7.6.2.26 rnrs enums](#rnrs-enums)
      * [7.6.2.27 rnrs](#rnrs)
      * [7.6.2.28 rnrs eval](#rnrs-eval)
      * [7.6.2.29 rnrs mutable-pairs](#rnrs-mutable_002dpairs)
      * [7.6.2.30 rnrs mutable-strings](#rnrs-mutable_002dstrings)
      * [7.6.2.31 rnrs r5rs](#rnrs-r5rs)
  + [7.7 R7RS Support](#R7RS-Support)
    - [7.7.1 Incompatibilities with the R7RS](#R7RS-Incompatibilities)
    - [7.7.2 R7RS Standard Libraries](#R7RS-Standard-Libraries)
  + [7.8 Pattern Matching](#Pattern-Matching)
  + [7.9 Readline Support](#Readline-Support)
    - [7.9.1 Loading Readline Support](#Loading-Readline-Support)
    - [7.9.2 Readline Options](#Readline-Options)
    - [7.9.3 Readline Functions](#Readline-Functions)
      * [7.9.3.1 Readline Port](#Readline-Port)
      * [7.9.3.2 Completion](#Completion)
  + [7.10 Pretty Printing](#Pretty-Printing)
  + [7.11 Formatted Output](#Formatted-Output)
  + [7.12 File Tree Walk](#File-Tree-Walk)
  + [7.13 Queues](#Queues)
  + [7.14 Streams](#Streams)
  + [7.15 Buffered Input](#Buffered-Input)
  + [7.16 Expect](#Expect)
  + [7.17 `sxml-match`: Pattern Matching of SXML](#sxml_002dmatch)
    - [Syntax](#Syntax)
    - [Matching XML Elements](#Matching-XML-Elements)
    - [Ellipses in Patterns](#Ellipses-in-Patterns)
    - [Ellipses in Quasiquote’d Output](#Ellipses-in-Quasiquote_0027d-Output)
    - [Matching Nodesets](#Matching-Nodesets)
    - [Matching the “Rest” of a Nodeset](#Matching-the-_0060_0060Rest_0027_0027-of-a-Nodeset)
    - [Matching the Unmatched Attributes](#Matching-the-Unmatched-Attributes)
    - [Default Values in Attribute Patterns](#Default-Values-in-Attribute-Patterns)
    - [Guards in Patterns](#Guards-in-Patterns)
    - [Catamorphisms](#Catamorphisms)
    - [Named-Catamorphisms](#Named_002dCatamorphisms)
    - [`sxml-match-let` and `sxml-match-let*`](#sxml_002dmatch_002dlet-and-sxml_002dmatch_002dlet_002a)
  + [7.18 The Scheme shell (scsh)](#The-Scheme-shell-_0028scsh_0029)
  + [7.19 Curried Definitions](#Curried-Definitions)
  + [7.20 Statprof](#Statprof)
  + [7.21 SXML](#SXML)
    - [7.21.1 SXML Overview](#SXML-Overview)
    - [7.21.2 Reading and Writing XML](#Reading-and-Writing-XML)
    - [7.21.3 SSAX: A Functional XML Parsing Toolkit](#SSAX)
      * [7.21.3.1 History](#History-1)
      * [7.21.3.2 Implementation](#Implementation)
      * [7.21.3.3 Usage](#Usage)
    - [7.21.4 Transforming SXML](#Transforming-SXML)
      * [7.21.4.1 Overview](#Overview)
      * [7.21.4.2 Usage](#Usage-1)
    - [7.21.5 SXML Tree Fold](#SXML-Tree-Fold)
      * [7.21.5.1 Overview](#Overview-1)
      * [7.21.5.2 Usage](#Usage-2)
    - [7.21.6 SXPath](#SXPath)
      * [7.21.6.1 Overview](#Overview-2)
      * [7.21.6.2 Basic Converters and Applicators](#Basic-Converters-and-Applicators)
      * [7.21.6.3 Converter Combinators](#Converter-Combinators)
    - [7.21.7 (sxml ssax input-parse)](#sxml-ssax-input_002dparse)
      * [7.21.7.1 Overview](#Overview-3)
      * [7.21.7.2 Usage](#Usage-3)
    - [7.21.8 (sxml apply-templates)](#sxml-apply_002dtemplates)
      * [7.21.8.1 Overview](#Overview-4)
      * [7.21.8.2 Usage](#Usage-4)
  + [7.22 Texinfo Processing](#Texinfo-Processing)
    - [7.22.1 (texinfo)](#texinfo)
      * [7.22.1.1 Overview](#Overview-5)
      * [7.22.1.2 Usage](#Usage-5)
    - [7.22.2 (texinfo docbook)](#texinfo-docbook)
      * [7.22.2.1 Overview](#Overview-6)
      * [7.22.2.2 Usage](#Usage-6)
    - [7.22.3 (texinfo html)](#texinfo-html)
      * [7.22.3.1 Overview](#Overview-7)
      * [7.22.3.2 Usage](#Usage-7)
    - [7.22.4 (texinfo indexing)](#texinfo-indexing)
      * [7.22.4.1 Overview](#Overview-8)
      * [7.22.4.2 Usage](#Usage-8)
    - [7.22.5 (texinfo string-utils)](#texinfo-string_002dutils)
      * [7.22.5.1 Overview](#Overview-9)
      * [7.22.5.2 Usage](#Usage-9)
    - [7.22.6 (texinfo plain-text)](#texinfo-plain_002dtext)
      * [7.22.6.1 Overview](#Overview-10)
      * [7.22.6.2 Usage](#Usage-10)
    - [7.22.7 (texinfo serialize)](#texinfo-serialize)
      * [7.22.7.1 Overview](#Overview-11)
      * [7.22.7.2 Usage](#Usage-11)
    - [7.22.8 (texinfo reflection)](#texinfo-reflection)
      * [7.22.8.1 Overview](#Overview-12)
      * [7.22.8.2 Usage](#Usage-12)
* [8 GOOPS](#GOOPS)
  + [8.1 Copyright Notice](#Copyright-Notice)
  + [8.2 Class Definition](#Class-Definition)
  + [8.3 Instance Creation and Slot Access](#Instance-Creation)
  + [8.4 Slot Options](#Slot-Options)
  + [8.5 Illustrating Slot Description](#Slot-Description-Example)
  + [8.6 Methods and Generic Functions](#Methods-and-Generic-Functions)
    - [8.6.1 Accessors](#Accessors)
    - [8.6.2 Extending Primitives](#Extending-Primitives)
    - [8.6.3 Merging Generics](#Merging-Generics)
    - [8.6.4 Next-method](#Next_002dmethod)
    - [8.6.5 method\* and define-method\*](#method_002a-and-define_002dmethod_002a)
      * [8.6.5.1 Advanced argument handling in method and define-method](#Advanced-argument-handling-in-method-and-define_002dmethod)
      * [8.6.5.2 Type dispatch and redefinition for advanced argument handling](#Type-dispatch-and-redefinition-for-advanced-argument-handling)
      * [8.6.5.3 next-method call in method\*](#next_002dmethod-call-in-method_002a)
      * [8.6.5.4 Advanced argument handling design choices](#Advanced-argument-handling-design-choices)
    - [8.6.6 Generic Function and Method Examples](#Generic-Function-and-Method-Examples)
    - [8.6.7 Handling Invocation Errors](#Handling-Invocation-Errors)
  + [8.7 Inheritance](#Inheritance)
    - [8.7.1 Class Precedence List](#Class-Precedence-List)
    - [8.7.2 Sorting Methods](#Sorting-Methods)
    - [8.7.3 Inheritance and accessors](#Inheritance-and-accessors)
  + [8.8 Introspection](#Introspection)
    - [8.8.1 Classes](#Classes)
    - [8.8.2 Instances](#Instances)
    - [8.8.3 Slots](#Slots)
    - [8.8.4 Generic Functions](#Generic-Functions)
    - [8.8.5 Accessing Slots](#Accessing-Slots)
  + [8.9 Error Handling](#GOOPS-Error-Handling)
  + [8.10 GOOPS Object Miscellany](#GOOPS-Object-Miscellany)
  + [8.11 The Metaobject Protocol](#The-Metaobject-Protocol)
    - [8.11.1 Metaobjects and the Metaobject Protocol](#Metaobjects-and-the-Metaobject-Protocol)
    - [8.11.2 Metaclasses](#Metaclasses)
    - [8.11.3 MOP Specification](#MOP-Specification)
    - [8.11.4 Instance Creation Protocol](#Instance-Creation-Protocol)
    - [8.11.5 Class Definition Protocol](#Class-Definition-Protocol)
    - [8.11.6 Customizing Class Definition](#Customizing-Class-Definition)
    - [8.11.7 Method Definition](#Method-Definition)
    - [8.11.8 Method Definition Internals](#Method-Definition-Internals)
    - [8.11.9 Generic Function Internals](#Generic-Function-Internals)
    - [8.11.10 Generic Function Invocation](#Generic-Function-Invocation)
  + [8.12 Redefining a Class](#Redefining-a-Class)
    - [8.12.1 Redefinable Classes](#Redefinable-Classes)
    - [8.12.2 Default Class Redefinition Behavior](#Default-Class-Redefinition-Behavior)
    - [8.12.3 Customizing Class Redefinition](#Customizing-Class-Redefinition)
  + [8.13 Changing the Class of an Instance](#Changing-the-Class-of-an-Instance)
* [9 Guile Implementation](#Guile-Implementation)
  + [9.1 A Brief History of Guile](#History)
    - [9.1.1 The Emacs Thesis](#The-Emacs-Thesis)
    - [9.1.2 Early Days](#Early-Days)
    - [9.1.3 A Scheme of Many Maintainers](#A-Scheme-of-Many-Maintainers)
    - [9.1.4 A Timeline of Selected Guile Releases](#A-Timeline-of-Selected-Guile-Releases)
    - [9.1.5 Status, or: Your Help Needed](#Status)
  + [9.2 Data Representation](#Data-Representation)
    - [9.2.1 A Simple Representation](#A-Simple-Representation)
    - [9.2.2 Faster Integers](#Faster-Integers)
    - [9.2.3 Cheaper Pairs](#Cheaper-Pairs)
    - [9.2.4 Conservative Garbage Collection](#Conservative-GC)
    - [9.2.5 The SCM Type in Guile](#The-SCM-Type-in-Guile)
      * [9.2.5.1 Relationship Between `SCM` and `scm_t_bits`](#Relationship-Between-SCM-and-scm_005ft_005fbits)
      * [9.2.5.2 Immediate Objects](#Immediate-Objects)
      * [9.2.5.3 Non-Immediate Objects](#Non_002dImmediate-Objects)
      * [9.2.5.4 Allocating Heap Objects](#Allocating-Heap-Objects)
      * [9.2.5.5 Heap Object Type Information](#Heap-Object-Type-Information)
      * [9.2.5.6 Accessing Heap Object Fields](#Accessing-Heap-Object-Fields)
  + [9.3 A Virtual Machine for Guile](#A-Virtual-Machine-for-Guile)
    - [9.3.1 Why a VM?](#Why-a-VM_003f)
    - [9.3.2 VM Concepts](#VM-Concepts)
    - [9.3.3 Stack Layout](#Stack-Layout)
    - [9.3.4 Variables and the VM](#Variables-and-the-VM)
    - [9.3.5 Compiled Procedures are VM Programs](#VM-Programs)
    - [9.3.6 Object File Format](#Object-File-Format)
    - [9.3.7 Instruction Set](#Instruction-Set)
      * [9.3.7.1 Call and Return Instructions](#Call-and-Return-Instructions)
      * [9.3.7.2 Function Prologue Instructions](#Function-Prologue-Instructions)
      * [9.3.7.3 Shuffling Instructions](#Shuffling-Instructions)
      * [9.3.7.4 Trampoline Instructions](#Trampoline-Instructions)
      * [9.3.7.5 Non-Local Control Flow Instructions](#Non_002dLocal-Control-Flow-Instructions)
      * [9.3.7.6 Instrumentation Instructions](#Instrumentation-Instructions)
      * [9.3.7.7 Intrinsic Call Instructions](#Intrinsic-Call-Instructions)
      * [9.3.7.8 Constant Instructions](#Constant-Instructions)
      * [9.3.7.9 Memory Access Instructions](#Memory-Access-Instructions)
      * [9.3.7.10 Atomic Memory Access Instructions](#Atomic-Memory-Access-Instructions)
      * [9.3.7.11 Tagging and Untagging Instructions](#Tagging-and-Untagging-Instructions)
      * [9.3.7.12 Integer Arithmetic Instructions](#Integer-Arithmetic-Instructions)
      * [9.3.7.13 Floating-Point Arithmetic Instructions](#Floating_002dPoint-Arithmetic-Instructions)
      * [9.3.7.14 Comparison Instructions](#Comparison-Instructions)
      * [9.3.7.15 Branch Instructions](#Branch-Instructions)
      * [9.3.7.16 Raw Memory Access Instructions](#Raw-Memory-Access-Instructions)
    - [9.3.8 Just-In-Time Native Code](#Just_002dIn_002dTime-Native-Code)
  + [9.4 Compiling to the Virtual Machine](#Compiling-to-the-Virtual-Machine)
    - [9.4.1 Compiler Tower](#Compiler-Tower)
    - [9.4.2 The Scheme Compiler](#The-Scheme-Compiler)
    - [9.4.3 Tree-IL](#Tree_002dIL)
    - [9.4.4 Continuation-Passing Style](#Continuation_002dPassing-Style)
      * [9.4.4.1 An Introduction to CPS](#An-Introduction-to-CPS)
      * [9.4.4.2 CPS in Guile](#CPS-in-Guile)
      * [9.4.4.3 Building CPS](#Building-CPS)
      * [9.4.4.4 CPS Soup](#CPS-Soup)
      * [9.4.4.5 Compiling CPS](#Compiling-CPS)
    - [9.4.5 Bytecode](#Bytecode)
    - [9.4.6 Writing New High-Level Languages](#Writing-New-High_002dLevel-Languages)
    - [9.4.7 Extending the Compiler](#Extending-the-Compiler)
* [Appendix A GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Concept Index](#Concept-Index)
* [Procedure Index](#Procedure-Index)
* [Variable Index](#Variable-Index)
* [Type Index](#Type-Index)
* [R5RS Index](#R5RS-Index)

---

Next: [Introduction](#Introduction), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## Preface [¶](#Preface-1)

This manual describes how to use Guile, GNU’s Ubiquitous Intelligent
Language for Extensions. It relates particularly to Guile version
3.0.11.

* [Contributors to this Manual](#Contributors)
* [The Guile License](#Guile-License)

---

Next: [The Guile License](#Guile-License), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### Contributors to this Manual [¶](#Contributors-to-this-Manual)

Like Guile itself, the Guile reference manual is a living entity, cared
for by many people over a long period of time. As such, it is hard to
identify individuals of whom to say “yes, this single person wrote the
manual.”

Still, among the many contributions, some caretakers stand out. First
among them is Neil Jerram, who has worked on this document for over ten
years. Neil’s attention both to detail and to the big picture have made
a real difference in the understanding of a generation of Guile hackers.

Next we should note Marius Vollmer’s effect on this document. Marius
maintained Guile during a period in which Guile’s API was
clarified—put to the fire, so to speak—and he had the good sense to
effect the same change on the manual.

Martin Grabmueller made substantial contributions throughout the manual
in preparation for the Guile 1.6 release, including filling out a lot of
the documentation of Scheme data types, control mechanisms and
procedures. In addition, he wrote the documentation for Guile’s SRFI
modules and modules associated with the Guile REPL.

Ludovic Courtès and Andy Wingo, who co-maintain Guile since 2010,
along with Mark Weaver, have also made their dent in the manual, writing
documentation for new modules and subsystems that arrived with Guile
2.0. Ludovic, Andy, and Mark are also responsible for ensuring that the
existing text retains its relevance as Guile evolves. See [Reporting Bugs](#Reporting-Bugs), for more information on reporting problems in this manual.

The content for the first versions of this manual incorporated and was
inspired by documents from Aubrey Jaffer, author of the SCM system on
which Guile was based, and from Tom Lord, Guile’s first maintainer.
Although most of this text has been rewritten, all of it was important,
and some of the structure remains.

The manual for the first versions of Guile were largely written, edited,
and compiled by Mark Galassi and Jim Blandy. In particular, Jim wrote
the original tutorial on Guile’s data representation and the C API for
accessing Guile objects.

Significant portions were also contributed by Thien-Thi Nguyen, Kevin
Ryde, Mikael Djurfeldt, Christian Lynbech, Julian Graham, Gary Houston,
Tim Pierce, and a few dozen more. You, reader, are most welcome to join
their esteemed ranks. Visit Guile’s web site at
<http://www.gnu.org/software/guile/> to find out how to get
involved.

---

Previous: [Contributors to this Manual](#Contributors), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### The Guile License [¶](#The-Guile-License)

Guile is Free Software. Guile is copyrighted, not public domain, and
there are restrictions on its distribution or redistribution, but
these restrictions are designed to permit everything a cooperating
person would want to do.

* The Guile library (libguile) and supporting files are published under
  the terms of the GNU Lesser General Public License version 3 or later.
  See the files `COPYING.LESSER` and `COPYING`.
* The Guile readline module is published under the terms of the GNU
  General Public License version 3 or later. See the file `COPYING`.
* The manual you’re now reading is published under the terms of the GNU
  Free Documentation License (see [GNU Free Documentation License](#GNU-Free-Documentation-License)).

C code linking to the Guile library is subject to terms of that
library. Basically such code may be published on any terms, provided
users can re-link against a new or modified version of Guile.

C code linking to the Guile readline module is subject to the terms of
that module. Basically such code must be published on Free terms.

Scheme level code written to be run by Guile (but not derived from
Guile itself) is not restricted in any way, and may be published on any
terms. We encourage authors to publish on Free terms.

You must be aware there is no warranty whatsoever for Guile. This is
described in full in the licenses.

---

Next: [Hello Guile!](#Hello-Guile_0021), Previous: [Preface](#Preface), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## 1 Introduction [¶](#Introduction-1)

Guile is an implementation of the Scheme programming language. Scheme
(<http://schemers.org/>) is an elegant and conceptually simple
dialect of Lisp, originated by Guy Steele and Gerald Sussman, and since
evolved by the series of reports known as RnRS (the
Revised^n
Reports on Scheme).

There are many Scheme implementations, with different
characteristics and with communities and academic activities around
them, and the language develops as a result of the interplay between
these. Guile’s particular characteristics are that

* it is easy to combine with other code written in C
* it has a historical and continuing connection with the GNU Project
* it emphasizes interactive and incremental programming
* it actually supports several languages, not just Scheme.

The next few sections explain what we mean by these points. The sections after
that cover how you can obtain and install Guile, and the typographical
conventions that we use in this manual.

* [Guile and Scheme](#Guile-and-Scheme)
* [Combining with C Code](#Combining-with-C)
* [Guile and the GNU Project](#Guile-and-the-GNU-Project)
* [Interactive Programming](#Interactive-Programming)
* [Supporting Multiple Languages](#Supporting-Multiple-Languages)
* [Obtaining and Installing Guile](#Obtaining-and-Installing-Guile)
* [Organisation of this Manual](#Organisation-of-this-Manual)
* [Typographical Conventions](#Typographical-Conventions)

---

Next: [Combining with C Code](#Combining-with-C), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.1 Guile and Scheme [¶](#Guile-and-Scheme-1)

Guile implements Scheme as described in the
Revised^5
Report on the Algorithmic Language Scheme (usually known as
R5RS), providing clean and general data and control
structures. Guile goes beyond the rather austere language presented
in R5RS, extending it with a module system, full access to
POSIX system calls, networking support, multiple threads,
dynamic linking, a foreign function call interface, powerful string
processing, and many other features needed for programming in the real
world.

In 2007, the Scheme community agreed upon and published R6RS, a
significant installment in the RnRS series. R6RS expands the core
Scheme language, and standardises many non-core functions that
implementations—including Guile—have previously done in different
ways. Over time, Guile has been updated to incorporate almost all of
the features of R6RS, and to adjust some existing features to conform to
the R6RS specification. See [R6RS Support](#R6RS-Support), for full details.

In parallel to official standardization efforts, the SRFI process
(<http://srfi.schemers.org/>) standardises interfaces for many
practical needs, such as multithreaded programming and multidimensional
arrays. Guile supports many SRFIs, as documented in detail in [SRFI Support Modules](#SRFI-Support).

The process that led to the R6RS standard brought a split in the Scheme
community to the surface. The implementors that wrote R6RS considered
that it was impossible to write useful, portable programs in R5RS, and
that only an ambitious standard could solve this problem. However, part
of the Scheme world saw the R6RS effort as too broad, and as having
included some components that would never be adopted by more
minimalistic Scheme implementations. This second group succeeded in
taking control of the official Scheme standardization track and in 2013
released a more limited R7RS, essentially consisting of R5RS, plus a
module system. Guile supports R7RS also. See [R7RS Support](#R7RS-Support).

With R6RS and R7RS, the unified Scheme standardization process appears
to have more or less run its course. There will continue to be more
code written in terms of both systems, and modules defined using the
SRFI process, and Guile will support both. However for future
directions, Guile takes inspiration from other related language
communities: Racket, Clojure, Concurrent ML, and so on.

In summary, Guile supports writing and running code written to the R5RS,
R6RS, and R7RS Scheme standards, and also supports a number of SRFI
modules. However for most users, until a need for cross-implementation
portability has been identified, we recommend using the parts of Guile
that are useful in solving the problem at hand, regardless of whether
they proceed from a standard or whether they are Guile-specific.

---

Next: [Guile and the GNU Project](#Guile-and-the-GNU-Project), Previous: [Guile and Scheme](#Guile-and-Scheme), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.2 Combining with C Code [¶](#Combining-with-C-Code)

Like a shell, Guile can run interactively—reading expressions from the user,
evaluating them, and displaying the results—or as a script interpreter,
reading and executing Scheme code from a file. Guile also provides an object
library, *libguile*, that allows other applications to easily incorporate a
complete Scheme interpreter. An application can then use Guile as an extension
language, a clean and powerful configuration language, or as multi-purpose
“glue”, connecting primitives provided by the application. It is easy to call
Scheme code from C code and vice versa, giving the application designer full
control of how and when to invoke the interpreter. Applications can add new
functions, data types, control structures, and even syntax to Guile, creating a
domain-specific language tailored to the task at hand, but based on a robust
language design.

This kind of combination is helped by four aspects of Guile’s design
and history. First is that Guile has always been targeted as an
extension language. Hence its C API has always been of great
importance, and has been developed accordingly. Second and third are
rather technical points—that Guile uses conservative garbage
collection, and that it implements the Scheme concept of continuations
by copying and reinstating the C stack—but whose practical
consequence is that most existing C code can be glued into Guile as
is, without needing modifications to cope with strange Scheme
execution flows. Last is the module system, which helps extensions to
coexist without stepping on each others’ toes.

Guile’s module system allows one to break up a large program into
manageable sections with well-defined interfaces between them.
Modules may contain a mixture of interpreted and compiled code; Guile
can use either static or dynamic linking to incorporate compiled code.
Modules also encourage developers to package up useful collections of
routines for general distribution; as of this writing, one can find
Emacs interfaces, database access routines, compilers, GUI
toolkit interfaces, and HTTP client functions, among others.

---

Next: [Interactive Programming](#Interactive-Programming), Previous: [Combining with C Code](#Combining-with-C), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.3 Guile and the GNU Project [¶](#Guile-and-the-GNU-Project-1)

Guile was conceived by the GNU Project following the fantastic success
of Emacs Lisp as an extension language within Emacs. Just as Emacs
Lisp allowed complete and unanticipated applications to be written
within the Emacs environment, the idea was that Guile should do the
same for other GNU Project applications. This remains true today.

The idea of extensibility is closely related to the GNU project’s
primary goal, that of promoting software freedom. Software freedom
means that people receiving a software package can modify or enhance
it to their own desires, including in ways that may not have occurred
at all to the software’s original developers. For programs written in
a compiled language like C, this freedom covers modifying and
rebuilding the C code; but if the program also provides an extension
language, that is usually a much friendlier and lower-barrier-of-entry
way for the user to start making their own changes.

Guile is now used by GNU project applications such as AutoGen, Lilypond, Denemo,
Mailutils, TeXmacs and Gnucash, and we hope that there will be many more in
future.

---

Next: [Supporting Multiple Languages](#Supporting-Multiple-Languages), Previous: [Guile and the GNU Project](#Guile-and-the-GNU-Project), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.4 Interactive Programming [¶](#Interactive-Programming-1)

Non-free software has no interest in its users being able to see how it works.
They are supposed to just accept it, or to report problems and hope that the
source code owners will choose to work on them.

Free software aims to work reliably just as much as non-free software does, but
it should also empower its users by making its workings available. This is
useful for many reasons, including education, auditing and enhancements, as well
as for debugging problems.

The ideal free software system achieves this by making it easy for interested
users to see the source code for a feature that they are using, and to follow
through that source code step-by-step, as it runs. In Emacs, good examples of
this are the source code hyperlinks in the help system, and `edebug`.
Then, for bonus points and maximising the ability for the user to experiment
quickly with code changes, the system should allow parts of the source code to
be modified and reloaded into the running program, to take immediate effect.

Guile is designed for this kind of interactive programming, and this
distinguishes it from many Scheme implementations that instead prioritise
running a fixed Scheme program as fast as possible—because there are
tradeoffs between performance and the ability to modify parts of an already
running program. There are faster Schemes than Guile, but Guile is a GNU
project and so prioritises the GNU vision of programming freedom and
experimentation.

---

Next: [Obtaining and Installing Guile](#Obtaining-and-Installing-Guile), Previous: [Interactive Programming](#Interactive-Programming), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.5 Supporting Multiple Languages [¶](#Supporting-Multiple-Languages-1)

Since the 2.0 release, Guile’s architecture supports compiling any language to
its core virtual machine bytecode, and Scheme is just one of the supported
languages. Other supported languages are Emacs Lisp, ECMAScript (commonly known
as Javascript) and Brainfuck, and work is under discussion for Lua, Ruby and
Python.

This means that users can program applications which use Guile in the language
of their choice, rather than having the tastes of the application’s author
imposed on them.

---

Next: [Organisation of this Manual](#Organisation-of-this-Manual), Previous: [Supporting Multiple Languages](#Supporting-Multiple-Languages), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.6 Obtaining and Installing Guile [¶](#Obtaining-and-Installing-Guile-1)

Guile can be obtained from the main GNU archive site
<ftp://ftp.gnu.org> or any of its mirrors. The file will be named
guile-version.tar.gz. The current version is 3.0.11, so the
file you should grab is:

<ftp://ftp.gnu.org/gnu/guile/guile-3.0.11.tar.gz>

To unbundle Guile use the instruction

```
zcat guile-3.0.11.tar.gz | tar xvf -
```

which will create a directory called `guile-3.0.11` with
all the sources. You can look at the file `INSTALL` for detailed
instructions on how to build and install Guile, but you should be able
to just do

```
cd guile-3.0.11
./configure
make
make install
```

This will install the Guile executable `guile`, the Guile library
`libguile` and various associated header files and support libraries. It
will also install the Guile reference manual.

Since this manual frequently refers to the Scheme “standard”, also
known as R5RS, or the
“Revised^5 Report on the Algorithmic Language Scheme”,
we have included the report in the Guile distribution; see
[Introduction](/r5rs/latest/en/r5rs.html#Top) in Revised(5) Report on the Algorithmic
Language Scheme.
This will also be installed in your info directory.

---

Next: [Typographical Conventions](#Typographical-Conventions), Previous: [Obtaining and Installing Guile](#Obtaining-and-Installing-Guile), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.7 Organisation of this Manual [¶](#Organisation-of-this-Manual-1)

The rest of this manual is organised into the following chapters.

**Chapter 2: Hello Guile!**
:   A whirlwind tour shows how Guile can be used interactively and as
    a script interpreter, how to link Guile into your own applications,
    and how to write modules of interpreted and compiled code for use with
    Guile. Everything introduced here is documented again and in full by
    the later parts of the manual.

**Chapter 3: Hello Scheme!**
:   For readers new to Scheme, this chapter provides an introduction to the basic
    ideas of the Scheme language. This material would apply to any Scheme
    implementation and so does not make reference to anything Guile-specific.

**Chapter 4: Programming in Scheme**
:   Provides an overview of programming in Scheme with Guile. It covers how to
    invoke the `guile` program from the command-line and how to write scripts
    in Scheme. It also introduces the extensions that Guile offers beyond standard
    Scheme.

**Chapter 5: Programming in C**
:   Provides an overview of how to use Guile in a C program. It
    discusses the fundamental concepts that you need to understand to
    access the features of Guile, such as dynamic types and the garbage
    collector. It explains in a tutorial like manner how to define new
    data types and functions for the use by Scheme programs.

**Chapter 6: Guile API Reference**
:   This part of the manual documents the Guile API in
    functionality-based groups with the Scheme and C interfaces presented
    side by side.

**Chapter 7: Guile Modules**
:   Describes some important modules, distributed as part of the Guile
    distribution, that extend the functionality provided by the Guile
    Scheme core.

**Chapter 8: GOOPS**
:   Describes GOOPS, an object oriented extension to Guile that provides
    classes, multiple inheritance and generic functions.

---

Previous: [Organisation of this Manual](#Organisation-of-this-Manual), Up: [Introduction](#Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 1.8 Typographical Conventions [¶](#Typographical-Conventions-1)

In examples and procedure descriptions and all other places where the
evaluation of Scheme expression is shown, we use some notation for
denoting the output and evaluation results of expressions.

The symbol ‘`⇒`’ is used to tell which value is returned by
an evaluation:

```
(+ 1 2)
⇒ 3
```

Some procedures produce some output besides returning a value. This
is denoted by the symbol ‘`⊣`’.

```
(begin (display 1) (newline) 'hooray)
⊣ 1
⇒ hooray
```

As you can see, this code prints ‘`1`’ (denoted by
‘`⊣`’), and returns `hooray` (denoted by
‘`⇒`’).

---

Next: [Hello Scheme!](#Hello-Scheme_0021), Previous: [Introduction](#Introduction), Up: [The Guile Reference Manual](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]## 2 Hello Guile! [¶](#Hello-Guile_0021-1)

This chapter presents a quick tour of all the ways that Guile can be
used. There are additional examples in the `examples/`
directory in the Guile source distribution. It also explains how best to report
any problems that you find.

The following examples assume that Guile has been installed in
`/usr/local/`.

* [Running Guile Interactively](#Running-Guile-Interactively)
* [Running Guile Scripts](#Running-Guile-Scripts)
* [Linking Guile into Programs](#Linking-Guile-into-Programs)
* [Writing Guile Extensions](#Writing-Guile-Extensions)
* [Using the Guile Module System](#Using-the-Guile-Module-System)
* [Reporting Bugs](#Reporting-Bugs)

---

Next: [Running Guile Scripts](#Running-Guile-Scripts), Up: [Hello Guile!](#Hello-Guile_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 2.1 Running Guile Interactively [¶](#Running-Guile-Interactively-1)

In its simplest form, Guile acts as an interactive interpreter for the
Scheme programming language, reading and evaluating Scheme expressions
the user enters from the terminal. Here is a sample interaction between
Guile and a user; the user’s input appears after the `$` and
`scheme@(guile-user)>` prompts:

```
$ guile
scheme@(guile-user)> (+ 1 2 3)                ; add some numbers
$1 = 6
scheme@(guile-user)> (define (factorial n)    ; define a function
                       (if (zero? n) 1 (* n (factorial (- n 1)))))
scheme@(guile-user)> (factorial 20)
$2 = 2432902008176640000
scheme@(guile-user)> (getpwnam "root")        ; look in /etc/passwd
$3 = #("root" "x" 0 0 "root" "/root" "/bin/bash")
scheme@(guile-user)> C-d
$
```

---

Next: [Linking Guile into Programs](#Linking-Guile-into-Programs), Previous: [Running Guile Interactively](#Running-Guile-Interactively), Up: [Hello Guile!](#Hello-Guile_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 2.2 Running Guile Scripts [¶](#Running-Guile-Scripts-1)

Like AWK, Perl, or any shell, Guile can interpret script files. A Guile
script is simply a file of Scheme code with some extra information at
the beginning which tells the operating system how to invoke Guile, and
then tells Guile how to handle the Scheme code.

Here is a trivial Guile script. See [Guile Scripting](#Guile-Scripting), for more details.

```
#!/usr/local/bin/guile -s
!#
(display "Hello, world!")
(newline)
```

---

Next: [Writing Guile Extensions](#Writing-Guile-Extensions), Previous: [Running Guile Scripts](#Running-Guile-Scripts), Up: [Hello Guile!](#Hello-Guile_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 2.3 Linking Guile into Programs [¶](#Linking-Guile-into-Programs-1)

The Guile interpreter is available as an object library, to be linked
into applications using Scheme as a configuration or extension
language.

Here is `simple-guile.c`, source code for a program that will
produce a complete Guile interpreter. In addition to all usual
functions provided by Guile, it will also offer the function
`my-hostname`.

```
#include <stdlib.h>
#include <libguile.h>

static SCM
my_hostname (void)
{
  char *s = getenv ("HOSTNAME");
  if (s == NULL)
    return SCM_BOOL_F;
  else
    return scm_from_locale_string (s);
}

static void
inner_main (void *data, int argc, char **argv)
{
  scm_c_define_gsubr ("my-hostname", 0, 0, 0, my_hostname);
  scm_shell (argc, argv);
}

int
main (int argc, char **argv)
{
  scm_boot_guile (argc, argv, inner_main, 0);
  return 0; /* never reached */
}
```

When Guile is correctly installed on your system, the above program
can be compiled and linked like this:

```
$ gcc -o simple-guile simple-guile.c \
    `pkg-config --cflags --libs guile-3.0`
```

When it is run, it behaves just like the `guile` program except
that you can also call the new `my-hostname` function.

```
$ ./simple-guile
scheme@(guile-user)> (+ 1 2 3)
$1 = 6
scheme@(guile-user)> (my-hostname)
"burns"
```

---

Next: [Using the Guile Module System](#Using-the-Guile-Module-System), Previous: [Linking Guile into Programs](#Linking-Guile-into-Programs), Up: [Hello Guile!](#Hello-Guile_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

### 2.4 Writing Guile Extensions [¶](#Writing-Guile-Extensions-1)

You can link Guile into your program and make Scheme available to the
users of your program. You can also link your library into Guile and
make its functionality available to all users of Guile.

A library that is linked into Guile is called an *extension*, but it
really just is an ordinary object library.

The following example shows how to write a simple extension for Guile
that makes the `j0` function available to Scheme code.

```
#include <math.h>
#include <libguile.h>

SCM
j0_wrapper (SCM x)
{
  return scm_from_double (j0 (scm_to_double (x)));
}

void
init_bessel ()
{
  scm_c_define_gsubr ("j0", 1, 0, 0, j0_wrapper);
}
```

This C source file needs to be compiled into a shared library. Here is
how to do it on GNU/Linux:

```
gcc `pkg-config --cflags guile-3.0` \
  -shared -o libguile-bessel.so -fPIC bessel.c
```

For creating shared libraries portably, we recommend the use of GNU
Libtool (see [Introduction](https://www.gnu.org/software/libtool/manual/libtool.html#Top) in GNU Libtool).

A shared library can be loaded into a running Guile process with the
function `load-extension`. The `j0` is then immediately
available:

```
$ guile
scheme@(guile-user)> (load-extension "./libguile-bessel" "init_bessel")
scheme@(guile-user)> (j0 2)
$1 = 0.223890779141236
```

For more on how to install your extension, see [Installing Site Packages](#Installing-Site-Packages).

---

Next: [Reporting Bugs](#Reporting-Bugs), Previous: [Writing Guile Extensions](#Writing-Guile-Extensions), Up: [Hello Guile!](#Hello-Guile_0021)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
