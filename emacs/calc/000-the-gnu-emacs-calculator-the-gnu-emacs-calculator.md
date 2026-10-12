# The GNU Emacs Calculator [¶](#The-GNU-Emacs-Calculator)

*Calc* is an advanced desk calculator and mathematical tool
written by Dave Gillespie that runs as part of the GNU Emacs environment.

This manual, also written (mostly) by Dave Gillespie, is divided into
three major parts: “Getting Started,” the “Calc Tutorial,” and the
“Calc Reference.” The Tutorial introduces all the major aspects of
Calculator use in an easy, hands-on way. The remainder of the manual is
a complete reference to the features of the Calculator.

This file documents Calc, the GNU Emacs calculator, included with
GNU Emacs 31.1.50.

Copyright © 1990–1991, 2001–2026 Free Software Foundation,
Inc.

> Permission is granted to copy, distribute and/or modify this document
> under the terms of the GNU Free Documentation License, Version 1.3 or
> any later version published by the Free Software Foundation; with the
> Invariant Sections being just “GNU GENERAL PUBLIC LICENSE”, with the
> Front-Cover Texts being “A GNU Manual,” and with the Back-Cover
> Texts as in (a) below. A copy of the license is included in the section
> entitled “GNU Free Documentation License.”
>
> (a) The FSF’s Back-Cover Text is: “You have the freedom to copy and
> modify this GNU manual.”## Short Table of Contents

* [1 Getting Started](#toc-Getting-Started-1)
* [2 Tutorial](#toc-Tutorial-1)
* [3 Introduction](#toc-Introduction-1)
* [4 Data Types](#toc-Data-Types-1)
* [5 Stack and Trail Commands](#toc-Stack-and-Trail-Commands)
* [6 Mode Settings](#toc-Mode-Settings-1)
* [7 Arithmetic Functions](#toc-Arithmetic-Functions)
* [8 Scientific Functions](#toc-Scientific-Functions-1)
* [9 Vector/Matrix Functions](#toc-Vector_002fMatrix-Functions)
* [10 Algebra](#toc-Algebra-1)
* [11 Operating on Units](#toc-Operating-on-Units)
* [12 Storing and Recalling](#toc-Storing-and-Recalling)
* [13 Graphics](#toc-Graphics-1)
* [14 Kill and Yank Functions](#toc-Kill-and-Yank-Functions)
* [15 Keypad Mode](#toc-Keypad-Mode-1)
* [16 Embedded Mode](#toc-Embedded-Mode-1)
* [17 Programming](#toc-Programming-1)
* [Appendix A GNU GENERAL PUBLIC LICENSE](#toc-GNU-GENERAL-PUBLIC-LICENSE)
* [Appendix B GNU Free Documentation License](#toc-GNU-Free-Documentation-License-1)
* [Appendix C Customizing Calc](#toc-Customizing-Calc-1)
* [Appendix D Reporting Bugs](#toc-Reporting-Bugs-1)
* [Appendix E Calc Summary](#toc-Calc-Summary)
* [Index of Key Sequences](#toc-Index-of-Key-Sequences)
* [Index of Calculator Commands](#toc-Index-of-Calculator-Commands)
* [Index of Algebraic Functions](#toc-Index-of-Algebraic-Functions)
* [Concept Index](#toc-Concept-Index-1)
* [Index of Variables](#toc-Index-of-Variables)
* [Index of Lisp Math Functions](#toc-Index-of-Lisp-Math-Functions)## Table of Contents

* [1 Getting Started](#Getting-Started)
  + [1.1 What is Calc?](#What-is-Calc)
  + [1.2 About This Manual](#About-This-Manual)
  + [1.3 Notations Used in This Manual](#Notations-Used-in-This-Manual)
  + [1.4 A Demonstration of Calc](#Demonstration-of-Calc)
  + [1.5 Using Calc](#Using-Calc)
    - [1.5.1 Starting Calc](#Starting-Calc)
    - [1.5.2 The Standard Calc Interface](#The-Standard-Interface)
    - [1.5.3 Quick Mode (Overview)](#Quick-Mode-Overview)
    - [1.5.4 Keypad Mode (Overview)](#Keypad-Mode-Overview)
    - [1.5.5 Standalone Operation](#Standalone-Operation)
    - [1.5.6 Embedded Mode (Overview)](#Embedded-Mode-Overview)
    - [1.5.7 Other `C-x *` Commands](#Other-C_002dx-_002a-Commands)
  + [1.6 History and Acknowledgments](#History-and-Acknowledgments)
* [2 Tutorial](#Tutorial)
  + [2.1 Basic Tutorial](#Basic-Tutorial)
    - [2.1.1 RPN Calculations and the Stack](#RPN-Tutorial)
    - [2.1.2 Algebraic-Style Calculations](#Algebraic-Tutorial)
    - [2.1.3 Undo and Redo](#Undo-Tutorial)
    - [2.1.4 Mode-Setting Commands](#Modes-Tutorial)
  + [2.2 Arithmetic Tutorial](#Arithmetic-Tutorial)
  + [2.3 Vector/Matrix Tutorial](#Vector_002fMatrix-Tutorial)
    - [2.3.1 Vector Analysis](#Vector-Analysis-Tutorial)
    - [2.3.2 Matrices](#Matrix-Tutorial)
    - [2.3.3 Vectors as Lists](#List-Tutorial)
  + [2.4 Types Tutorial](#Types-Tutorial)
  + [2.5 Algebra and Calculus Tutorial](#Algebra-Tutorial)
    - [2.5.1 Basic Algebra](#Basic-Algebra-Tutorial)
    - [2.5.2 Rewrite Rules](#Rewrites-Tutorial)
  + [2.6 Programming Tutorial](#Programming-Tutorial)
  + [2.7 Answers to Exercises](#Answers-to-Exercises)
    - [2.7.1 RPN Tutorial Exercise 1](#RPN-Answer-1)
    - [2.7.2 RPN Tutorial Exercise 2](#RPN-Answer-2)
    - [2.7.3 RPN Tutorial Exercise 3](#RPN-Answer-3)
    - [2.7.4 RPN Tutorial Exercise 4](#RPN-Answer-4)
    - [2.7.5 Algebraic Entry Tutorial Exercise 1](#Algebraic-Answer-1)
    - [2.7.6 Algebraic Entry Tutorial Exercise 2](#Algebraic-Answer-2)
    - [2.7.7 Algebraic Entry Tutorial Exercise 3](#Algebraic-Answer-3)
    - [2.7.8 Modes Tutorial Exercise 1](#Modes-Answer-1)
    - [2.7.9 Modes Tutorial Exercise 2](#Modes-Answer-2)
    - [2.7.10 Modes Tutorial Exercise 3](#Modes-Answer-3)
    - [2.7.11 Modes Tutorial Exercise 4](#Modes-Answer-4)
    - [2.7.12 Arithmetic Tutorial Exercise 1](#Arithmetic-Answer-1)
    - [2.7.13 Arithmetic Tutorial Exercise 2](#Arithmetic-Answer-2)
    - [2.7.14 Vector Tutorial Exercise 1](#Vector-Answer-1)
    - [2.7.15 Vector Tutorial Exercise 2](#Vector-Answer-2)
    - [2.7.16 Matrix Tutorial Exercise 1](#Matrix-Answer-1)
    - [2.7.17 Matrix Tutorial Exercise 2](#Matrix-Answer-2)
    - [2.7.18 Matrix Tutorial Exercise 3](#Matrix-Answer-3)
    - [2.7.19 List Tutorial Exercise 1](#List-Answer-1)
    - [2.7.20 List Tutorial Exercise 2](#List-Answer-2)
    - [2.7.21 List Tutorial Exercise 3](#List-Answer-3)
    - [2.7.22 List Tutorial Exercise 4](#List-Answer-4)
    - [2.7.23 List Tutorial Exercise 5](#List-Answer-5)
    - [2.7.24 List Tutorial Exercise 6](#List-Answer-6)
    - [2.7.25 List Tutorial Exercise 7](#List-Answer-7)
    - [2.7.26 List Tutorial Exercise 8](#List-Answer-8)
    - [2.7.27 List Tutorial Exercise 9](#List-Answer-9)
    - [2.7.28 List Tutorial Exercise 10](#List-Answer-10)
    - [2.7.29 List Tutorial Exercise 11](#List-Answer-11)
    - [2.7.30 List Tutorial Exercise 12](#List-Answer-12)
    - [2.7.31 List Tutorial Exercise 13](#List-Answer-13)
    - [2.7.32 List Tutorial Exercise 14](#List-Answer-14)
    - [2.7.33 Types Tutorial Exercise 1](#Types-Answer-1)
    - [2.7.34 Types Tutorial Exercise 2](#Types-Answer-2)
    - [2.7.35 Types Tutorial Exercise 3](#Types-Answer-3)
    - [2.7.36 Types Tutorial Exercise 4](#Types-Answer-4)
    - [2.7.37 Types Tutorial Exercise 5](#Types-Answer-5)
    - [2.7.38 Types Tutorial Exercise 6](#Types-Answer-6)
    - [2.7.39 Types Tutorial Exercise 7](#Types-Answer-7)
    - [2.7.40 Types Tutorial Exercise 8](#Types-Answer-8)
    - [2.7.41 Types Tutorial Exercise 9](#Types-Answer-9)
    - [2.7.42 Types Tutorial Exercise 10](#Types-Answer-10)
    - [2.7.43 Types Tutorial Exercise 11](#Types-Answer-11)
    - [2.7.44 Types Tutorial Exercise 12](#Types-Answer-12)
    - [2.7.45 Types Tutorial Exercise 13](#Types-Answer-13)
    - [2.7.46 Types Tutorial Exercise 14](#Types-Answer-14)
    - [2.7.47 Types Tutorial Exercise 15](#Types-Answer-15)
    - [2.7.48 Algebra Tutorial Exercise 1](#Algebra-Answer-1)
    - [2.7.49 Algebra Tutorial Exercise 2](#Algebra-Answer-2)
    - [2.7.50 Algebra Tutorial Exercise 3](#Algebra-Answer-3)
    - [2.7.51 Algebra Tutorial Exercise 4](#Algebra-Answer-4)
    - [2.7.52 Rewrites Tutorial Exercise 1](#Rewrites-Answer-1)
    - [2.7.53 Rewrites Tutorial Exercise 2](#Rewrites-Answer-2)
    - [2.7.54 Rewrites Tutorial Exercise 3](#Rewrites-Answer-3)
    - [2.7.55 Rewrites Tutorial Exercise 4](#Rewrites-Answer-4)
    - [2.7.56 Rewrites Tutorial Exercise 5](#Rewrites-Answer-5)
    - [2.7.57 Rewrites Tutorial Exercise 6](#Rewrites-Answer-6)
    - [2.7.58 Programming Tutorial Exercise 1](#Programming-Answer-1)
    - [2.7.59 Programming Tutorial Exercise 2](#Programming-Answer-2)
    - [2.7.60 Programming Tutorial Exercise 3](#Programming-Answer-3)
    - [2.7.61 Programming Tutorial Exercise 4](#Programming-Answer-4)
    - [2.7.62 Programming Tutorial Exercise 5](#Programming-Answer-5)
    - [2.7.63 Programming Tutorial Exercise 6](#Programming-Answer-6)
    - [2.7.64 Programming Tutorial Exercise 7](#Programming-Answer-7)
    - [2.7.65 Programming Tutorial Exercise 8](#Programming-Answer-8)
    - [2.7.66 Programming Tutorial Exercise 9](#Programming-Answer-9)
    - [2.7.67 Programming Tutorial Exercise 10](#Programming-Answer-10)
    - [2.7.68 Programming Tutorial Exercise 11](#Programming-Answer-11)
    - [2.7.69 Programming Tutorial Exercise 12](#Programming-Answer-12)
* [3 Introduction](#Introduction)
  + [3.1 Basic Commands](#Basic-Commands)
  + [3.2 Help Commands](#Help-Commands)
  + [3.3 Stack Basics](#Stack-Basics)
  + [3.4 Numeric Entry](#Numeric-Entry)
  + [3.5 Algebraic Entry](#Algebraic-Entry)
  + [3.6 “Quick Calculator” Mode](#Quick-Calculator)
  + [3.7 Numeric Prefix Arguments](#Prefix-Arguments)
  + [3.8 Undoing Mistakes](#Undo)
  + [3.9 Error Messages](#Error-Messages)
  + [3.10 Multiple Calculators](#Multiple-Calculators)
  + [3.11 Troubleshooting Commands](#Troubleshooting-Commands)
    - [3.11.1 Autoloading Problems](#Autoloading-Problems)
    - [3.11.2 Recursion Depth](#Recursion-Depth)
    - [3.11.3 Caches](#Caches)
    - [3.11.4 Debugging Calc](#Debugging-Calc)
* [4 Data Types](#Data-Types)
  + [4.1 Integers](#Integers)
  + [4.2 Fractions](#Fractions)
  + [4.3 Floats](#Floats)
  + [4.4 Complex Numbers](#Complex-Numbers)
  + [4.5 Infinities](#Infinities)
  + [4.6 Vectors and Matrices](#Vectors-and-Matrices)
  + [4.7 Strings](#Strings)
  + [4.8 HMS Forms](#HMS-Forms)
  + [4.9 Date Forms](#Date-Forms)
  + [4.10 Modulo Forms](#Modulo-Forms)
  + [4.11 Error Forms](#Error-Forms)
  + [4.12 Interval Forms](#Interval-Forms)
  + [4.13 Incomplete Objects](#Incomplete-Objects)
  + [4.14 Variables](#Variables)
  + [4.15 Formulas](#Formulas)
* [5 Stack and Trail Commands](#Stack-and-Trail)
  + [5.1 Stack Manipulation Commands](#Stack-Manipulation)
  + [5.2 Editing Stack Entries](#Editing-Stack-Entries)
  + [5.3 Trail Commands](#Trail-Commands)
  + [5.4 Keep Arguments](#Keep-Arguments)
* [6 Mode Settings](#Mode-Settings)
  + [6.1 General Mode Commands](#General-Mode-Commands)
  + [6.2 Precision](#Precision)
  + [6.3 Inverse and Hyperbolic Flags](#Inverse-and-Hyperbolic)
  + [6.4 Calculation Modes](#Calculation-Modes)
    - [6.4.1 Angular Modes](#Angular-Modes)
    - [6.4.2 Polar Mode](#Polar-Mode)
    - [6.4.3 Fraction Mode](#Fraction-Mode)
    - [6.4.4 Infinite Mode](#Infinite-Mode)
    - [6.4.5 Symbolic Mode](#Symbolic-Mode)
    - [6.4.6 Matrix and Scalar Modes](#Matrix-Mode)
    - [6.4.7 Automatic Recomputation](#Automatic-Recomputation)
    - [6.4.8 Working Messages](#Working-Message)
  + [6.5 Simplification Modes](#Simplification-Modes)
  + [6.6 Declarations](#Declarations)
    - [6.6.1 Declaration Basics](#Declaration-Basics)
    - [6.6.2 Kinds of Declarations](#Kinds-of-Declarations)
    - [6.6.3 Functions for Declarations](#Functions-for-Declarations)
  + [6.7 Display Modes](#Display-Modes)
    - [6.7.1 Radix Modes](#Radix-Modes)
    - [6.7.2 Grouping Digits](#Grouping-Digits)
    - [6.7.3 Float Formats](#Float-Formats)
    - [6.7.4 Complex Formats](#Complex-Formats)
    - [6.7.5 Fraction Formats](#Fraction-Formats)
    - [6.7.6 HMS Formats](#HMS-Formats)
    - [6.7.7 Date Formats](#Date-Formats)
      * [6.7.7.1 ISO 8601](#ISO-8601)
      * [6.7.7.2 Date Formatting Codes](#Date-Formatting-Codes)
      * [6.7.7.3 Free-Form Dates](#Free_002dForm-Dates)
      * [6.7.7.4 Standard Date Formats](#Standard-Date-Formats)
    - [6.7.8 Truncating the Stack](#Truncating-the-Stack)
    - [6.7.9 Justification](#Justification)
    - [6.7.10 Labels](#Labels)
  + [6.8 Language Modes](#Language-Modes)
    - [6.8.1 Normal Language Modes](#Normal-Language-Modes)
    - [6.8.2 C, FORTRAN, and Pascal Modes](#C-FORTRAN-Pascal)
    - [6.8.3 TeX and LaTeX Language Modes](#TeX-and-LaTeX-Language-Modes)
    - [6.8.4 Eqn Language Mode](#Eqn-Language-Mode)
    - [6.8.5 Yacas Language Mode](#Yacas-Language-Mode)
    - [6.8.6 Maxima Language Mode](#Maxima-Language-Mode)
    - [6.8.7 Giac Language Mode](#Giac-Language-Mode)
    - [6.8.8 Mathematica Language Mode](#Mathematica-Language-Mode)
    - [6.8.9 Maple Language Mode](#Maple-Language-Mode)
    - [6.8.10 Compositions](#Compositions)
      * [6.8.10.1 Composition Basics](#Composition-Basics)
      * [6.8.10.2 Horizontal Compositions](#Horizontal-Compositions)
      * [6.8.10.3 Vertical Compositions](#Vertical-Compositions)
      * [6.8.10.4 Other Compositions](#Other-Compositions)
      * [6.8.10.5 Information about Compositions](#Information-about-Compositions)
      * [6.8.10.6 User-Defined Compositions](#User_002dDefined-Compositions)
    - [6.8.11 Syntax Tables](#Syntax-Tables)
      * [6.8.11.1 Syntax Table Basics](#Syntax-Table-Basics)
      * [6.8.11.2 Precedence](#Precedence-in-Syntax-Tables)
      * [6.8.11.3 Advanced Syntax Patterns](#Advanced-Syntax-Patterns)
      * [6.8.11.4 Conditional Syntax Rules](#Conditional-Syntax-Rules)
  + [6.9 The `Modes` Variable](#Modes-Variable)
  + [6.10 The Calc Mode Line](#Calc-Mode-Line)
* [7 Arithmetic Functions](#Arithmetic)
  + [7.1 Basic Arithmetic](#Basic-Arithmetic)
  + [7.2 Integer Truncation](#Integer-Truncation)
  + [7.3 Complex Number Functions](#Complex-Number-Functions)
  + [7.4 Conversions](#Conversions)
  + [7.5 Date Arithmetic](#Date-Arithmetic)
    - [7.5.1 Date Conversions](#Date-Conversions)
    - [7.5.2 Date Functions](#Date-Functions)
    - [7.5.3 Business Days](#Business-Days)
    - [7.5.4 Time Zones](#Time-Zones)
  + [7.6 Financial Functions](#Financial-Functions)
    - [7.6.1 Percentages](#Percentages)
    - [7.6.2 Future Value](#Future-Value)
    - [7.6.3 Present Value](#Present-Value)
    - [7.6.4 Related Financial Functions](#Related-Financial-Functions)
    - [7.6.5 Depreciation Functions](#Depreciation-Functions)
    - [7.6.6 Definitions](#Definitions-of-Financial-Functions)
  + [7.7 Binary Number Functions](#Binary-Functions)
* [8 Scientific Functions](#Scientific-Functions)
  + [8.1 Logarithmic Functions](#Logarithmic-Functions)
  + [8.2 Trigonometric/Hyperbolic Functions](#Trigonometric-and-Hyperbolic-Functions)
  + [8.3 Advanced Mathematical Functions](#Advanced-Math-Functions)
  + [8.4 Branch Cuts and Principal Values](#Branch-Cuts)
  + [8.5 Random Numbers](#Random-Numbers)
    - [8.5.1 Random Number Generator](#Random-Number-Generator)
  + [8.6 Combinatorial Functions](#Combinatorial-Functions)
  + [8.7 Probability Distribution Functions](#Probability-Distribution-Functions)
* [9 Vector/Matrix Functions](#Matrix-Functions)
  + [9.1 Packing and Unpacking](#Packing-and-Unpacking)
  + [9.2 Building Vectors](#Building-Vectors)
  + [9.3 Extracting Vector Elements](#Extracting-Elements)
  + [9.4 Manipulating Vectors](#Manipulating-Vectors)
  + [9.5 Vector and Matrix Arithmetic](#Vector-and-Matrix-Arithmetic)
  + [9.6 Set Operations using Vectors](#Set-Operations)
  + [9.7 Statistical Operations on Vectors](#Statistical-Operations)
    - [9.7.1 Single-Variable Statistics](#Single_002dVariable-Statistics)
    - [9.7.2 Paired-Sample Statistics](#Paired_002dSample-Statistics)
  + [9.8 Reducing and Mapping Vectors](#Reducing-and-Mapping)
    - [9.8.1 Specifying Operators](#Specifying-Operators)
    - [9.8.2 Mapping](#Mapping)
    - [9.8.3 Reducing](#Reducing)
    - [9.8.4 Nesting and Fixed Points](#Nesting-and-Fixed-Points)
    - [9.8.5 Generalized Products](#Generalized-Products)
  + [9.9 Vector and Matrix Display Formats](#Vector-and-Matrix-Formats)
* [10 Algebra](#Algebra)
  + [10.1 Selecting Sub-Formulas](#Selecting-Subformulas)
    - [10.1.1 Making Selections](#Making-Selections)
    - [10.1.2 Changing Selections](#Changing-Selections)
    - [10.1.3 Displaying Selections](#Displaying-Selections)
    - [10.1.4 Operating on Selections](#Operating-on-Selections)
    - [10.1.5 Rearranging Formulas using Selections](#Rearranging-with-Selections)
  + [10.2 Algebraic Manipulation](#Algebraic-Manipulation)
  + [10.3 Simplifying Formulas](#Simplifying-Formulas)
    - [10.3.1 Basic Simplifications](#Basic-Simplifications)
    - [10.3.2 Algebraic Simplifications](#Algebraic-Simplifications)
    - [10.3.3 “Unsafe” Simplifications](#Unsafe-Simplifications)
    - [10.3.4 Simplification of Units](#Simplification-of-Units)
  + [10.4 Polynomials](#Polynomials)
  + [10.5 Calculus](#Calculus)
    - [10.5.1 Differentiation](#Differentiation)
    - [10.5.2 Integration](#Integration)
    - [10.5.3 Customizing the Integrator](#Customizing-the-Integrator)
    - [10.5.4 Numerical Integration](#Numerical-Integration)
    - [10.5.5 Taylor Series](#Taylor-Series)
  + [10.6 Solving Equations](#Solving-Equations)
    - [10.6.1 Multiple Solutions](#Multiple-Solutions)
    - [10.6.2 Solving Systems of Equations](#Solving-Systems-of-Equations)
    - [10.6.3 Decomposing Polynomials](#Decomposing-Polynomials)
  + [10.7 Numerical Solutions](#Numerical-Solutions)
    - [10.7.1 Root Finding](#Root-Finding)
    - [10.7.2 Minimization](#Minimization)
    - [10.7.3 Systems of Equations](#Numerical-Systems-of-Equations)
  + [10.8 Curve Fitting](#Curve-Fitting)
    - [10.8.1 Linear Fits](#Linear-Fits)
    - [10.8.2 Polynomial and Multilinear Fits](#Polynomial-and-Multilinear-Fits)
    - [10.8.3 Error Estimates for Fits](#Error-Estimates-for-Fits)
    - [10.8.4 Standard Nonlinear Models](#Standard-Nonlinear-Models)
    - [10.8.5 Curve Fitting Details](#Curve-Fitting-Details)
    - [10.8.6 Polynomial Interpolation](#Interpolation)
  + [10.9 Summations](#Summations)
  + [10.10 Logical Operations](#Logical-Operations)
  + [10.11 Rewrite Rules](#Rewrite-Rules)
    - [10.11.1 Entering Rewrite Rules](#Entering-Rewrite-Rules)
    - [10.11.2 Basic Rewrite Rules](#Basic-Rewrite-Rules)
    - [10.11.3 Conditional Rewrite Rules](#Conditional-Rewrite-Rules)
    - [10.11.4 Algebraic Properties of Rewrite Rules](#Algebraic-Properties-of-Rewrite-Rules)
    - [10.11.5 Other Features of Rewrite Rules](#Other-Features-of-Rewrite-Rules)
    - [10.11.6 Composing Patterns in Rewrite Rules](#Composing-Patterns-in-Rewrite-Rules)
    - [10.11.7 Nested Formulas with Rewrite Rules](#Nested-Formulas-with-Rewrite-Rules)
    - [10.11.8 Multi-Phase Rewrite Rules](#Multi_002dPhase-Rewrite-Rules)
    - [10.11.9 Selections with Rewrite Rules](#Selections-with-Rewrite-Rules)
    - [10.11.10 Matching Commands](#Matching-Commands)
    - [10.11.11 Automatic Rewrites](#Automatic-Rewrites)
    - [10.11.12 Debugging Rewrites](#Debugging-Rewrites)
    - [10.11.13 Examples of Rewrite Rules](#Examples-of-Rewrite-Rules)
* [11 Operating on Units](#Units)
  + [11.1 Basic Operations on Units](#Basic-Operations-on-Units)
  + [11.2 The Units Table](#The-Units-Table)
  + [11.3 Predefined Units](#Predefined-Units)
  + [11.4 User-Defined Units](#User_002dDefined-Units)
  + [11.5 Logarithmic Units](#Logarithmic-Units)
  + [11.6 Musical Notes](#Musical-Notes)
* [12 Storing and Recalling](#Store-and-Recall)
  + [12.1 Storing Variables](#Storing-Variables)
  + [12.2 Recalling Variables](#Recalling-Variables)
  + [12.3 Other Operations on Variables](#Operations-on-Variables)
  + [12.4 The Let Command](#Let-Command)
  + [12.5 The Evaluates-To Operator](#Evaluates_002dTo-Operator)
* [13 Graphics](#Graphics)
  + [13.1 Basic Graphics](#Basic-Graphics)
  + [13.2 Three-Dimensional Graphics](#Three-Dimensional-Graphics)
  + [13.3 Managing Curves](#Managing-Curves)
  + [13.4 Graphics Options](#Graphics-Options)
  + [13.5 Graphical Devices](#Devices)
* [14 Kill and Yank Functions](#Kill-and-Yank)
  + [14.1 Killing from the Stack](#Killing-From-Stack)
  + [14.2 Yanking into the Stack](#Yanking-Into-Stack)
  + [14.3 Saving into Registers](#Saving-Into-Registers)
  + [14.4 Inserting from Registers](#Inserting-From-Registers)
  + [14.5 Grabbing from Other Buffers](#Grabbing-From-Buffers)
  + [14.6 Yanking into Other Buffers](#Yanking-Into-Buffers)
  + [14.7 X Cut and Paste](#X-Cut-and-Paste)
* [15 Keypad Mode](#Keypad-Mode)
  + [15.1 Main Menu](#Keypad-Main-Menu)
  + [15.2 Functions Menu](#Keypad-Functions-Menu)
  + [15.3 Binary Menu](#Keypad-Binary-Menu)
  + [15.4 Vectors Menu](#Keypad-Vectors-Menu)
  + [15.5 Modes Menu](#Keypad-Modes-Menu)
* [16 Embedded Mode](#Embedded-Mode)
  + [16.1 Basic Embedded Mode](#Basic-Embedded-Mode)
  + [16.2 More About Embedded Mode](#More-About-Embedded-Mode)
  + [16.3 Assignments in Embedded Mode](#Assignments-in-Embedded-Mode)
  + [16.4 Mode Settings in Embedded Mode](#Mode-Settings-in-Embedded-Mode)
  + [16.5 Customizing Embedded Mode](#Customizing-Embedded-Mode)
* [17 Programming](#Programming)
  + [17.1 Creating User Keys](#Creating-User-Keys)
  + [17.2 Programming with Keyboard Macros](#Keyboard-Macros)
    - [17.2.1 Naming Keyboard Macros](#Naming-Keyboard-Macros)
    - [17.2.2 Conditionals in Keyboard Macros](#Conditionals-in-Macros)
    - [17.2.3 Loops in Keyboard Macros](#Loops-in-Macros)
    - [17.2.4 Local Values in Macros](#Local-Values-in-Macros)
    - [17.2.5 Queries in Keyboard Macros](#Queries-in-Macros)
  + [17.3 Invocation Macros](#Invocation-Macros)
  + [17.4 Programming with Formulas](#Algebraic-Definitions)
  + [17.5 Programming Calc, with Lisp](#Lisp-Definitions)
    - [17.5.1 Defining New Functions](#Defining-Functions)
    - [17.5.2 Defining New Simple Commands](#Defining-Simple-Commands)
    - [17.5.3 Defining New Stack-Based Commands](#Defining-Stack-Commands)
    - [17.5.4 Argument Qualifiers](#Argument-Qualifiers)
    - [17.5.5 Example Definitions](#Example-Definitions)
      * [17.5.5.1 Bit-Counting](#Bit-Counting-Example)
      * [17.5.5.2 The Sine Function](#Sine-Example)
    - [17.5.6 Calling Calc from Your Lisp Programs](#Calling-Calc-from-Your-Programs)
      * [17.5.6.1 Additional Arguments to `calc-eval`](#Additional-Arguments-to-calc_002deval)
      * [17.5.6.2 Error Handling](#Error-Handling)
      * [17.5.6.3 Numbers Only](#Numbers-Only)
      * [17.5.6.4 Default Modes](#Default-Modes)
      * [17.5.6.5 Raw Numbers](#Raw-Numbers)
      * [17.5.6.6 Predicates](#Predicates-1)
      * [17.5.6.7 Variable Values](#Variable-Values)
      * [17.5.6.8 Stack Access](#Stack-Access)
      * [17.5.6.9 Keyboard Macros](#Keyboard-Macros-1)
      * [17.5.6.10 Lisp Evaluation](#Lisp-Evaluation)
      * [17.5.6.11 Example](#Example)
    - [17.5.7 Calculator Internals](#Internals)
      * [17.5.7.1 Data Type Formats](#Data-Type-Formats)
      * [17.5.7.2 Interactive Functions](#Interactive-Lisp-Functions)
      * [17.5.7.3 Stack-Oriented Functions](#Stack-Lisp-Functions)
      * [17.5.7.4 Predicates](#Predicates)
      * [17.5.7.5 Computational Functions](#Computational-Lisp-Functions)
      * [17.5.7.6 Vector Functions](#Vector-Lisp-Functions)
      * [17.5.7.7 Symbolic Functions](#Symbolic-Lisp-Functions)
      * [17.5.7.8 I/O and Formatting Functions](#Formatting-Lisp-Functions)
      * [17.5.7.9 Hooks](#Hooks)
* [Appendix A GNU GENERAL PUBLIC LICENSE](#Copying)
* [Appendix B GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Appendix C Customizing Calc](#Customizing-Calc)
* [Appendix D Reporting Bugs](#Reporting-Bugs)
* [Appendix E Calc Summary](#Summary)
* [Index of Key Sequences](#Key-Index)
* [Index of Calculator Commands](#Command-Index)
* [Index of Algebraic Functions](#Function-Index)
* [Concept Index](#Concept-Index)
* [Index of Variables](#Variable-Index)
* [Index of Lisp Math Functions](#Lisp-Function-Index)

---

Next: [Tutorial](#Tutorial), Previous: [The GNU Emacs Calculator](#Top), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 1 Getting Started [¶](#Getting-Started-1)

This chapter provides a general overview of Calc, the GNU Emacs
Calculator: What it is, how to start it and how to exit from it,
and what are the various ways that it can be used.

* [What is Calc?](#What-is-Calc)
* [About This Manual](#About-This-Manual)
* [Notations Used in This Manual](#Notations-Used-in-This-Manual)
* [A Demonstration of Calc](#Demonstration-of-Calc)
* [Using Calc](#Using-Calc)
* [History and Acknowledgments](#History-and-Acknowledgments)

---

Next: [About This Manual](#About-This-Manual), Previous: [Getting Started](#Getting-Started), Up: [Getting Started](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 1.1 What is Calc? [¶](#What-is-Calc_003f)

*Calc* is an advanced calculator and mathematical tool that runs as
part of the GNU Emacs environment. Very roughly based on the HP-28/48
series of calculators, its many features include:

* Choice of algebraic or Reverse Polish notation (RPN),
  i.e. stack-based, entry of calculations.
* Arbitrary precision integers and floating-point numbers.
* Arithmetic on rational numbers, complex numbers (rectangular and polar),
  error forms with standard deviations, open and closed intervals, vectors
  and matrices, dates and times, infinities, sets, quantities with units,
  and algebraic formulas.
* Mathematical operations such as logarithms and trigonometric functions.
* Programmer’s features (bitwise operations, non-decimal numbers).
* Financial functions such as future value and internal rate of return.
* Number theoretical features such as prime factorization and arithmetic
  modulo m for any m.
* Algebraic manipulation features, including symbolic calculus.
* Moving data to and from regular editing buffers.
* Embedded mode for manipulating Calc formulas and data directly
  inside any editing buffer.
* Graphics using GNUPLOT, a versatile (and free) plotting program.
* Easy programming using keyboard macros, algebraic formulas,
  algebraic rewrite rules, or extended Emacs Lisp.

Calc tries to include a little something for everyone; as a result it is
large and might be intimidating to the first-time user. If you plan to
use Calc only as a traditional desk calculator, all you really need to
read is the “Getting Started” chapter of this manual and possibly the
first few sections of the tutorial. As you become more comfortable with
the program you can learn its additional features. Calc does not
have the scope and depth of a fully-functional symbolic math package,
but Calc has the advantages of convenience, portability, and freedom.

---

Next: [Notations Used in This Manual](#Notations-Used-in-This-Manual), Previous: [What is Calc?](#What-is-Calc), Up: [Getting Started](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 1.2 About This Manual [¶](#About-This-Manual-1)

This document serves as a complete description of the GNU Emacs
Calculator. It works both as an introduction for novices and as
a reference for experienced users. While it helps to have some
experience with GNU Emacs in order to get the most out of Calc,
this manual ought to be readable even if you don’t know or use Emacs
regularly.

This manual is divided into three major parts: the “Getting
Started” chapter you are reading now, the Calc tutorial, and the Calc
reference manual.

If you are in a hurry to use Calc, there is a brief “demonstration”
below which illustrates the major features of Calc in just a couple of
pages. If you don’t have time to go through the full tutorial, this
will show you everything you need to know to begin.
See [A Demonstration of Calc](#Demonstration-of-Calc).

The tutorial chapter walks you through the various parts of Calc
with lots of hands-on examples and explanations. If you are new
to Calc and you have some time, try going through at least the
beginning of the tutorial. The tutorial includes about 70 exercises
with answers. These exercises give you some guided practice with
Calc, as well as pointing out some interesting and unusual ways
to use its features.

The reference section discusses Calc in complete depth. You can read
the reference from start to finish if you want to learn every aspect
of Calc. Or, you can look in the table of contents or the Concept
Index to find the parts of the manual that discuss the things you
need to know.

Every Calc keyboard command is listed in the Calc Summary, and also
in the Key Index. Algebraic functions, `M-x` commands, and
variables also have their own indices.

You can access this manual on-line at any time within Calc by pressing
the `h i` key sequence. Outside of the Calc window, you can press
`C-x * i` to read the manual on-line. From within Calc the command
`h t` will jump directly to the Tutorial; from outside of Calc the
command `C-x * t` will jump to the Tutorial and start Calc if
necessary. Pressing `h s` or `C-x * s` will take you directly
to the Calc Summary. Within Calc, you can also go to the part of the
manual describing any Calc key, function, or variable using
`h k`, `h f`, or `h v`, respectively. See [Help Commands](#Help-Commands).

The Calc manual can be printed, but because the manual is so large, you
should only make a printed copy if you really need it. To print the
manual, you will need the TeX typesetting program (this is a free
program by Donald Knuth at Stanford University) as well as the
`texindex` program and `texinfo.tex` file, both of which can
be obtained from the FSF as part of the `texinfo` package.
To print the Calc manual in one huge tome, you will need the
Emacs source, which contains the source code to this manual,
`calc.texi`. Change to the `doc/misc` subdirectory of the
Emacs source distribution, which contains source code for this manual,
and type `make calc.pdf`. (Don’t worry if you get some “overfull
box” warnings while TeX runs.) The result will be this entire
manual as a pdf file.

---

Next: [A Demonstration of Calc](#Demonstration-of-Calc), Previous: [About This Manual](#About-This-Manual), Up: [Getting Started](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 1.3 Notations Used in This Manual [¶](#Notations-Used-in-This-Manual-1)

This section describes the various notations that are used
throughout the Calc manual.

In keystroke sequences, uppercase letters mean you must hold down
the shift key while typing the letter. Keys pressed with Control
held down are shown as `C-x`. Keys pressed with Meta held down
are shown as `M-x`. Other notations are `RET` for the
Return key, `SPC` for the space bar, `TAB` for the Tab key,
`DEL` for the Delete key, and `LFD` for the Line-Feed key.
The `DEL` key is called Backspace on some keyboards, it is
whatever key you would use to correct a simple typing error when
regularly using Emacs.

(If you don’t have the `LFD` or `TAB` keys on your keyboard,
the `C-j` and `C-i` keys are equivalent to them, respectively.
If you don’t have a Meta key, look for Alt or Extend Char. You can
also press `ESC` or `C-[` first to get the same effect, so
that `M-x`, `ESC x`, and `C-[ x` are all equivalent.)

Sometimes the `RET` key is not shown when it is “obvious”
that you must press `RET` to proceed. For example, the `RET`
is usually omitted in key sequences like `M-x calc-keypad RET`.

Commands are generally shown like this: `p` (`calc-precision`)
or `C-x * k` (`calc-keypad`). This means that the command is
normally used by pressing the `p` key or `C-x * k` key sequence,
but it also has the full-name equivalent shown, e.g., `M-x calc-precision`.

Commands that correspond to functions in algebraic notation
are written: `C` (`calc-cos`) [`cos`]. This means
the `C` key is equivalent to `M-x calc-cos`, and that
the corresponding function in an algebraic-style formula would
be ‘`cos(x)`’.

A few commands don’t have key equivalents: `calc-sincos`
[`sincos`].

---

Next: [Using Calc](#Using-Calc), Previous: [Notations Used in This Manual](#Notations-Used-in-This-Manual), Up: [Getting Started](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 1.4 A Demonstration of Calc [¶](#A-Demonstration-of-Calc)

This section will show some typical small problems being solved with
Calc. The focus is more on demonstration than explanation, but
everything you see here will be covered more thoroughly in the
Tutorial.

To begin, start Emacs if necessary (usually the command `emacs`
does this), and type `C-x * c` to start the
Calculator. (You can also use `M-x calc` if this doesn’t work.
See [Starting Calc](#Starting-Calc), for various ways of starting the Calculator.)

Be sure to type all the sample input exactly, especially noting the
difference between lower-case and upper-case letters. Remember,
`RET`, `TAB`, `DEL`, and `SPC` are the Return, Tab,
Delete, and Space keys.

**RPN calculation.** In Reverse Polish notation (RPN), you type
the input number(s) first, then the command to operate on the numbers.

Type `2 RET 3 + Q` to compute
the square root of 2+3, which is 2.2360679775.

Type `P 2 ^` to compute
the value of ‘`pi`’ squared, 9.86960440109.

Type `TAB` to exchange the order of these two results.

Type `- I H S` to subtract these results and compute the Inverse
Hyperbolic sine of the difference, 2.72996136574.

Type `DEL` to erase this result.

**Algebraic calculation.** You can also enter calculations using
conventional “algebraic” notation. To enter an algebraic formula,
use the apostrophe key.

Type `' sqrt(2+3) RET` to compute
the square root of 2+3.

Type `' pi^2 RET` to enter
‘`pi`’ squared.
To evaluate this symbolic formula as a number, type `=`.

Type `' arcsinh($ - $$) RET` to subtract the second-most-recent
result from the most-recent and compute the Inverse Hyperbolic sine.

**Keypad mode.** If you are using the X window system, press
`C-x * k` to get Keypad mode. (If you don’t use X, skip to
the next section.)

Click on the `2`, `ENTER`, `3`, `+`, and `SQRT`
“buttons” using your left mouse button.

Click on `PI`, `2`, and `y^x`.

Click on `INV`, then `ENTER` to swap the two results.

Click on `-`, `INV`, `HYP`, and `SIN`.

Click on `<-` to erase the result, then click `OFF` to turn
the Keypad Calculator off.

**Grabbing data.** Type `C-x * x` if necessary to exit Calc.
Now select the following numbers as an Emacs region: “Mark” the
front of the list by typing `C-SPC` or `C-@` there,
then move to the other end of the list. (Either get this list from
the on-line copy of this manual, accessed by `C-x * i`, or just
type these numbers into a scratch file.) Now type `C-x * g` to
“grab” these numbers into Calc.

```
1.23  1.97
1.6   2
1.19  1.08
```

The result ‘`[1.23, 1.97, 1.6, 2, 1.19, 1.08]`’ is a Calc “vector.”
Type `V R +` to compute the sum of these numbers.

Type `U` to Undo this command, then type `V R *` to compute
the product of the numbers.

You can also grab data as a rectangular matrix. Place the cursor on
the upper-leftmost ‘`1`’ and set the mark, then move to just after
the lower-right ‘`8`’ and press `C-x * r`.

Type `v t` to transpose this
3x2
matrix into a
2x3
matrix. Type `v u` to unpack the rows into two separate
vectors. Now type `V R + TAB V R +` to compute the sums
of the two original columns. (There is also a special
grab-and-sum-columns command, `C-x * :`.)

**Units conversion.** Units are entered algebraically.
Type `' 43 mi/hr RET` to enter the quantity 43 miles-per-hour.
Type `u c km/hr RET`. Type `u c m/s RET`.

**Date arithmetic.** Type `t N` to get the current date and
time. Type `90 +` to find the date 90 days from now. Type
`' <25 dec 87> RET` to enter a date, then `- 7 /` to see how
many weeks have passed since then.

**Algebra.** Algebraic entries can also include formulas
or equations involving variables. Type `' [x + y = a, x y = 1] RET`
to enter a pair of equations involving three variables.
(Note the leading apostrophe in this example; also, note that the space
in ‘`x y`’ is required.) Type `a S x,y RET` to solve
these equations for the variables ‘`x`’ and ‘`y`’.

Type `d B` to view the solutions in more readable notation.
Type `d C` to view them in C language notation, `d T`
to view them in the notation for the TeX typesetting system,
and `d L` to view them in the notation for the LaTeX typesetting
system. Type `d N` to return to normal notation.

Type `7.5`, then `s l a RET` to let ‘`a = 7.5`’ in these formulas.
(That’s the letter `l`, not the numeral `1`.)

**Help functions.** You can read about any command in the on-line
manual. Type `C-x * c` to return to Calc after each of these
commands: `h k t N` to read about the `t N` command,
`h f sqrt RET` to read about the `sqrt` function, and
`h s` to read the Calc summary.

Press `DEL` repeatedly to remove any leftover results from the stack.
To exit from Calc, press `q` or `C-x * c` again.

---

Next: [History and Acknowledgments](#History-and-Acknowledgments), Previous: [A Demonstration of Calc](#Demonstration-of-Calc), Up: [Getting Started](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 1.5 Using Calc [¶](#Using-Calc-1)

Calc has several user interfaces that are specialized for
different kinds of tasks. As well as Calc’s standard interface,
there are Quick mode, Keypad mode, and Embedded mode.

* [Starting Calc](#Starting-Calc)
* [The Standard Calc Interface](#The-Standard-Interface)
* [Quick Mode (Overview)](#Quick-Mode-Overview)
* [Keypad Mode (Overview)](#Keypad-Mode-Overview)
* [Standalone Operation](#Standalone-Operation)
* [Embedded Mode (Overview)](#Embedded-Mode-Overview)
* [Other `C-x *` Commands](#Other-C_002dx-_002a-Commands)

---

Next: [The Standard Calc Interface](#The-Standard-Interface), Up: [Using Calc](#Using-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 1.5.1 Starting Calc [¶](#Starting-Calc-1)

On most systems, you can type `C-x *` to start the Calculator.
The key sequence `C-x *` is bound to the command `calc-dispatch`,
which can be rebound if convenient (see [Customizing Calc](#Customizing-Calc)).

When you press `C-x *`, Emacs waits for you to press a second key to
complete the command. In this case, you will follow `C-x *` with a
letter (upper- or lower-case, it doesn’t matter for `C-x *`) that says
which Calc interface you want to use.

To get Calc’s standard interface, type `C-x * c`. To get
Keypad mode, type `C-x * k`. Type `C-x * ?` to get a brief
list of the available options, and type a second `?` to get
a complete list.

To ease typing, `C-x * *` also works to start Calc. It starts the
same interface (either `C-x * c` or `C-x * k`) that you last
used, selecting the `C-x * c` interface by default.

If `C-x *` doesn’t work for you, you can always type explicit
commands like `M-x calc` (for the standard user interface) or
`M-x calc-keypad` (for Keypad mode). First type `M-x`
(that’s Meta with the letter `x`), then, at the prompt,
type the full command (like `calc-keypad`) and press Return.

The same commands (like `C-x * c` or `C-x * *`) that start
the Calculator also turn it off if it is already on.

---

Next: [Quick Mode (Overview)](#Quick-Mode-Overview), Previous: [Starting Calc](#Starting-Calc), Up: [Using Calc](#Using-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 1.5.2 The Standard Calc Interface [¶](#The-Standard-Calc-Interface)

Calc’s standard interface acts like a traditional RPN calculator,
operated by the normal Emacs keyboard. When you type `C-x * c`
to start the Calculator, the Emacs screen splits into two windows
with the file you were editing on top and Calc on the bottom.

```
...
--**-Emacs: myfile             (Fundamental)----All----------------------
--- Emacs Calculator Mode ---                   |Emacs Calculator Trail
2:  17.3                                        |    17.3
1:  -5                                          |    3
    .                                           |    2
                                                |    4
                                                |  * 8
                                                |  ->-5
                                                |
--%*-Calc: 12 Deg       (Calculator)----All----- --%*- *Calc Trail*
```

In this figure, the mode-line for `myfile` has moved up and the
“Calculator” window has appeared below it. As you can see, Calc
actually makes two windows side-by-side. The lefthand one is
called the *stack window* and the righthand one is called the
*trail window.* The stack holds the numbers involved in the
calculation you are currently performing. The trail holds a complete
record of all calculations you have done. In a desk calculator with
a printer, the trail corresponds to the paper tape that records what
you do.

In this case, the trail shows that four numbers (17.3, 3, 2, and 4)
were first entered into the Calculator, then the 2 and 4 were
multiplied to get 8, then the 3 and 8 were subtracted to get *-5*.
(The ‘`>`’ symbol shows that this was the most recent calculation.)
The net result is the two numbers 17.3 and *-5* sitting on the stack.

Most Calculator commands deal explicitly with the stack only, but
there is a set of commands that allow you to search back through
the trail and retrieve any previous result.

Calc commands use the digits, letters, and punctuation keys.
Shifted (i.e., upper-case) letters are different from lowercase
letters. Some letters are *prefix* keys that begin two-letter
commands. For example, `e` means “enter exponent” and shifted
`E` means ‘`e^x`’. With the `d` (“display modes”) prefix
the letter “e” takes on very different meanings: `d e` means
“engineering notation” and `d E` means “*eqn* language mode.”

There is nothing stopping you from switching out of the Calc
window and back into your editing window, say by using the Emacs
`C-x o` (`other-window`) command. When the cursor is
inside a regular window, Emacs acts just like normal. When the
cursor is in the Calc stack or trail windows, keys are interpreted
as Calc commands.

When you quit by pressing `C-x * c` a second time, the Calculator
windows go away but the actual Stack and Trail are not gone, just
hidden. When you press `C-x * c` once again you will get the
same stack and trail contents you had when you last used the
Calculator.

The Calculator does not remember its state between Emacs sessions.
Thus if you quit Emacs and start it again, `C-x * c` will give you
a fresh stack and trail. There is a command (`m m`) that lets
you save your favorite mode settings between sessions, though.
One of the things it saves is which user interface (standard or
Keypad) you last used; otherwise, a freshly started Emacs will
always treat `C-x * *` the same as `C-x * c`.

The `q` key is another equivalent way to turn the Calculator off.

If you type `C-x * b` first and then `C-x * c`, you get a
full-screen version of Calc (`full-calc`) in which the stack and
trail windows are still side-by-side but are now as tall as the whole
Emacs screen. When you press `q` or `C-x * c` again to quit,
the file you were editing before reappears. The `C-x * b` key
switches back and forth between “big” full-screen mode and the
normal partial-screen mode.

Finally, `C-x * o` (`calc-other-window`) is like `C-x * c`
except that the Calc window is not selected. The buffer you were
editing before remains selected instead. If you are in a Calc window,
then `C-x * o` will switch you out of it, being careful not to
switch you to the Calc Trail window. So `C-x * o` is a handy
way to switch out of Calc momentarily to edit your file; you can then
type `C-x * c` to switch back into Calc when you are done.

---

Next: [Keypad Mode (Overview)](#Keypad-Mode-Overview), Previous: [The Standard Calc Interface](#The-Standard-Interface), Up: [Using Calc](#Using-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 1.5.3 Quick Mode (Overview) [¶](#Quick-Mode-_0028Overview_0029)

*Quick mode* is a quick way to use Calc when you don’t need the
full complexity of the stack and trail. To use it, type `C-x * q`
(`quick-calc`) in any regular editing buffer.

Quick mode is very simple: It prompts you to type any formula in
standard algebraic notation (like ‘`4 - 2/3`’) and then displays
the result at the bottom of the Emacs screen (*3.33333333333*
in this case). You are then back in the same editing buffer you
were in before, ready to continue editing or to type `C-x * q`
again to do another quick calculation. The result of the calculation
will also be in the Emacs “kill ring” so that a `C-y` command
at this point will yank the result into your editing buffer.

Calc mode settings affect Quick mode, too, though you will have to
go into regular Calc (with `C-x * c`) to change the mode settings.

See [“Quick Calculator” Mode](#Quick-Calculator), for further information.

---

Next: [Standalone Operation](#Standalone-Operation), Previous: [Quick Mode (Overview)](#Quick-Mode-Overview), Up: [Using Calc](#Using-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 1.5.4 Keypad Mode (Overview) [¶](#Keypad-Mode-_0028Overview_0029)

*Keypad mode* is a mouse-based interface to the Calculator.
It is designed for use with terminals that support a mouse. If you
don’t have a mouse, you will have to operate Keypad mode with your
arrow keys (which is probably more trouble than it’s worth).

Type `C-x * k` to turn Keypad mode on or off. Once again you
get two new windows, this time on the righthand side of the screen
instead of at the bottom. The upper window is the familiar Calc
Stack; the lower window is a picture of a typical calculator keypad.

```
|--- Emacs Calculator Mode ---
|2:  17.3
|1:  -5
|    .
|--%*-Calc: 12 Deg       (Calcul
|----+----+--Calc---+----+----1
|FLR |CEIL|RND |TRNC|CLN2|FLT |
|----+----+----+----+----+----|
| LN |EXP |    |ABS |IDIV|MOD |
|----+----+----+----+----+----|
|SIN |COS |TAN |SQRT|y^x |1/x |
|----+----+----+----+----+----|
|  ENTER  |+/- |EEX |UNDO| <- |
|-----+---+-+--+--+-+---++----|
| INV |  7  |  8  |  9  |  /  |
|-----+-----+-----+-----+-----|
| HYP |  4  |  5  |  6  |  *  |
|-----+-----+-----+-----+-----|
|EXEC |  1  |  2  |  3  |  -  |
|-----+-----+-----+-----+-----|
| OFF |  0  |  .  | PI  |  +  |
|-----+-----+-----+-----+-----+
```

Keypad mode is much easier for beginners to learn, because there
is no need to memorize lots of obscure key sequences. But not all
commands in regular Calc are available on the Keypad. You can
always switch the cursor into the Calc stack window to use
standard Calc commands if you need. Serious Calc users, though,
often find they prefer the standard interface over Keypad mode.

To operate the Calculator, just click on the “buttons” of the
keypad using your left mouse button. To enter the two numbers
shown here you would click `1 7 . 3 ENTER 5 +/- ENTER`; to
add them together you would then click `+` (to get 12.3 on
the stack).

If you click the right mouse button, the top three rows of the
keypad change to show other sets of commands, such as advanced
math functions, vector operations, and operations on binary
numbers.

Because Keypad mode doesn’t use the regular keyboard, Calc leaves
the cursor in your original editing buffer. You can type in
this buffer in the usual way while also clicking on the Calculator
keypad. One advantage of Keypad mode is that you don’t need an
explicit command to switch between editing and calculating.

If you press `C-x * b` first, you get a full-screen Keypad mode
(`full-calc-keypad`) with three windows: The keypad in the lower
left, the stack in the lower right, and the trail on top.

See [Keypad Mode](#Keypad-Mode), for further information.

---

Next: [Embedded Mode (Overview)](#Embedded-Mode-Overview), Previous: [Keypad Mode (Overview)](#Keypad-Mode-Overview), Up: [Using Calc](#Using-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 1.5.5 Standalone Operation [¶](#Standalone-Operation-1)

If you are not in Emacs at the moment but you wish to use Calc,
you must start Emacs first. If all you want is to run Calc, you
can give the commands:

```
emacs -f full-calc
```

or

```
emacs -f full-calc-keypad
```

which run a full-screen Calculator (as if by `C-x * b C-x * c`) or
a full-screen X-based Calculator (as if by `C-x * b C-x * k`).
In standalone operation, quitting the Calculator (by pressing
`q` or clicking on the keypad `EXIT` button) quits Emacs
itself.

---

Next: [Other `C-x *` Commands](#Other-C_002dx-_002a-Commands), Previous: [Standalone Operation](#Standalone-Operation), Up: [Using Calc](#Using-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 1.5.6 Embedded Mode (Overview) [¶](#Embedded-Mode-_0028Overview_0029)

*Embedded mode* is a way to use Calc directly from inside an
editing buffer. Suppose you have a formula written as part of a
document like this:

```
The derivative of

                                   ln(ln(x))

is
```

and you wish to have Calc compute and format the derivative for
you and store this derivative in the buffer automatically. To
do this with Embedded mode, first copy the formula down to where
you want the result to be, leaving a blank line before and after the
formula:

```
The derivative of

                                   ln(ln(x))

is

                                   ln(ln(x))
```

Now, move the cursor onto this new formula and press `C-x * e`.
Calc will read the formula (using the surrounding blank lines to tell
how much text to read), then push this formula (invisibly) onto the Calc
stack. The cursor will stay on the formula in the editing buffer, but
the line with the formula will now appear as it would on the Calc stack
(in this case, it will be left-aligned) and the buffer’s mode line will
change to look like the Calc mode line (with mode indicators like
‘`12 Deg`’ and so on). Even though you are still in your editing
buffer, the keyboard now acts like the Calc keyboard, and any new result
you get is copied from the stack back into the buffer. To take the
derivative, you would type `a d x RET`.

```
The derivative of

                                   ln(ln(x))

is

1 / x ln(x)
```

(Note that by default, Calc gives division lower precedence than multiplication,
so that ‘`1 / x ln(x)`’ is equivalent to ‘`1 / (x ln(x))`’.)

To make this look nicer, you might want to press `d =` to center
the formula, and even `d B` to use Big display mode.

```
The derivative of

                                   ln(ln(x))

is
% [calc-mode: justify: center]
% [calc-mode: language: big]

                                       1
                                    -------
                                    x ln(x)
```

Calc has added annotations to the file to help it remember the modes
that were used for this formula. They are formatted like comments
in the TeX typesetting language, just in case you are using TeX or
LaTeX. (In this example TeX is not being used, so you might want
to move these comments up to the top of the file or otherwise put them
out of the way.)

As an extra flourish, we can add an equation number using a
righthand label: Type `d } (1) RET`.

```
% [calc-mode: justify: center]
% [calc-mode: language: big]
% [calc-mode: right-label: " (1)"]

                                       1
                                    -------                      (1)
                                    ln(x) x
```

To leave Embedded mode, type `C-x * e` again. The mode line
and keyboard will revert to the way they were before.

The related command `C-x * w` operates on a single word, which
generally means a single number, inside text. It searches for an
expression which “looks” like a number containing the point.
Here’s an example of its use (before you try this, remove the Calc
annotations or use a new buffer so that the extra settings in the
annotations don’t take effect):

```
A slope of one-third corresponds to an angle of 1 degrees.
```

Place the cursor on the ‘`1`’, then type `C-x * w` to enable
Embedded mode on that number. Now type `3 /` (to get one-third),
and `I T` (the Inverse Tangent converts a slope into an angle),
then `C-x * w` again to exit Embedded mode.

```
A slope of one-third corresponds to an angle of 18.4349488229 degrees.
```

See [Embedded Mode](#Embedded-Mode), for full details.

---

Previous: [Embedded Mode (Overview)](#Embedded-Mode-Overview), Up: [Using Calc](#Using-Calc)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

#### 1.5.7 Other `C-x *` Commands [¶](#Other-C_002dx-_002a-Commands-1)

Two more Calc-related commands are `C-x * g` and `C-x * r`,
which “grab” data from a selected region of a buffer into the
Calculator. The region is defined in the usual Emacs way, by
a “mark” placed at one end of the region, and the Emacs
cursor or “point” placed at the other.

The `C-x * g` command reads the region in the usual left-to-right,
top-to-bottom order. The result is packaged into a Calc vector
of numbers and placed on the stack. Calc (in its standard
user interface) is then started. Type `v u` if you want
to unpack this vector into separate numbers on the stack. Also,
`C-u C-x * g` interprets the region as a single number or
formula.

The `C-x * r` command reads a rectangle, with the point and
mark defining opposite corners of the rectangle. The result
is a matrix of numbers on the Calculator stack.

Complementary to these is `C-x * y`, which “yanks” the
value at the top of the Calc stack back into an editing buffer.
If you type `C-x * y` while in such a buffer, the value is
yanked at the current position. If you type `C-x * y` while
in the Calc buffer, Calc makes an educated guess as to which
editing buffer you want to use. The Calc window does not have
to be visible in order to use this command, as long as there
is something on the Calc stack.

Here, for reference, is the complete list of `C-x *` commands.
The shift, control, and meta keys are ignored for the keystroke
following `C-x *`.

Commands for turning Calc on and off:

`*`
:   Turn Calc on or off, employing the same user interface as last time.

`=, +, -, /, \, &, #`
:   Alternatives for `*`.

`C`
:   Turn Calc on or off using its standard bottom-of-the-screen
    interface. If Calc is already turned on but the cursor is not
    in the Calc window, move the cursor into the window.

`O`
:   Same as `C`, but don’t select the new Calc window. If
    Calc is already turned on and the cursor is in the Calc window,
    move it out of that window.

`B`
:   Control whether `C-x * c` and `C-x * k` use the full screen.

`Q`
:   Use Quick mode for a single short calculation.

`K`
:   Turn Calc Keypad mode on or off.

`E`
:   Turn Calc Embedded mode on or off at the current formula.

`J`
:   Turn Calc Embedded mode on or off, select the interesting part.

`W`
:   Turn Calc Embedded mode on or off at the current word (number).

`Z`
:   Turn Calc on in a user-defined way, as defined by a `Z I` command.

`X`
:   Quit Calc; turn off standard, Keypad, or Embedded mode if on.
    (This is like `q` or `OFF` inside of Calc.)

Commands for moving data into and out of the Calculator:

`G`
:   Grab the region into the Calculator as a vector.

`R`
:   Grab the rectangular region into the Calculator as a matrix.

`:`
:   Grab the rectangular region and compute the sums of its columns.

`_`
:   Grab the rectangular region and compute the sums of its rows.

`Y`
:   Yank a value from the Calculator into the current editing buffer.

Commands for use with Embedded mode:

`A`
:   “Activate” the current buffer. Locate all formulas that
    contain ‘`:=`’ or ‘`=>`’ symbols and record their locations
    so that they can be updated automatically as variables are changed.

`D`
:   Duplicate the current formula immediately below and select
    the duplicate.

`F`
:   Insert a new formula at the current point.

`N`
:   Move the cursor to the next active formula in the buffer.

`P`
:   Move the cursor to the previous active formula in the buffer.

`U`
:   Update (i.e., as if by the `=` key) the formula at the current point.

`` ` ``
:   Edit (as if by `calc-edit`) the formula at the current point.

Miscellaneous commands:

`I`
:   Run the Emacs Info system to read the Calc manual.
    (This is the same as `h i` inside of Calc.)

`T`
:   Run the Emacs Info system to read the Calc Tutorial.

`S`
:   Run the Emacs Info system to read the Calc Summary.

`L`
:   Load Calc entirely into memory. (Normally the various parts
    are loaded only as they are needed.)

`M`
:   Read a region of written keystroke names (like `C-n a b c RET`)
    and record them as the current keyboard macro.

`0`
:   (This is the “zero” digit key.) Reset the Calculator to
    its initial state: Empty stack, and initial mode settings.

---

Previous: [Using Calc](#Using-Calc), Up: [Getting Started](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]

### 1.6 History and Acknowledgments [¶](#History-and-Acknowledgments-1)

Calc was originally started as a two-week project to occupy a lull
in the author’s schedule. Basically, a friend asked if I remembered
the value of
‘`2^32`’.
I didn’t offhand, but I said, “that’s easy, just call up an
`xcalc`.” `Xcalc` duly reported that the answer to our
question was ‘`4.294967e+09`’—with no way to see the full ten
digits even though we knew they were there in the program’s memory! I
was so annoyed, I vowed to write a calculator of my own, once and for
all.

I chose Emacs Lisp, a) because I had always been curious about it
and b) because, being only a text editor extension language after
all, Emacs Lisp would surely reach its limits long before the project
got too far out of hand.

To make a long story short, Emacs Lisp turned out to be a distressingly
solid implementation of Lisp, and the humble task of calculating
turned out to be more open-ended than one might have expected.

Emacs Lisp didn’t have built-in floating point math (now it does), so
this had to be simulated in software. In fact, Emacs integers would
only comfortably fit six decimal digits or so (at the time)—not
enough for a decent calculator. So I had to write my own
high-precision integer code as well, and once I had this I figured
that arbitrary-size integers were just as easy as large integers.
Arbitrary floating-point precision was the logical next step. Also,
since the large integer arithmetic was there anyway it seemed only
fair to give the user direct access to it, which in turn made it
practical to support fractions as well as floats. All these features
inspired me to look around for other data types that might be worth
having.

Around this time, my friend Rick Koshi showed me his nifty new HP-28
calculator. It allowed the user to manipulate formulas as well as
numerical quantities, and it could also operate on matrices. I
decided that these would be good for Calc to have, too. And once
things had gone this far, I figured I might as well take a look at
serious algebra systems for further ideas. Since these systems did
far more than I could ever hope to implement, I decided to focus on
rewrite rules and other programming features so that users could
implement what they needed for themselves.

Rick complained that matrices were hard to read, so I put in code to
format them in a 2D style. Once these routines were in place, Big mode
was obligatory. Gee, what other language modes would be useful?

Scott Hemphill and Allen Knutson, two friends with a strong mathematical
bent, contributed ideas and algorithms for a number of Calc features
including modulo forms, primality testing, and float-to-fraction conversion.

Units were added at the eager insistence of Mass Sivilotti. Later,
Ulrich Müller at CERN and Przemek Klosowski at NIST provided invaluable
expert assistance with the units table. As far as I can remember, the
idea of using algebraic formulas and variables to represent units dates
back to an ancient article in Byte magazine about muMath, an early
algebra system for microcomputers.

Many people have contributed to Calc by reporting bugs and suggesting
features, large and small. A few deserve special mention: Tim Peters,
who helped develop the ideas that led to the selection commands, rewrite
rules, and many other algebra features; François
Pinard, who contributed an early prototype of the Calc Summary appendix
as well as providing valuable suggestions in many other areas of Calc;
Carl Witty, whose eagle eyes discovered many typographical and factual
errors in the Calc manual; Tim Kay, who drove the development of
Embedded mode; Ove Ewerlid, who made many suggestions relating to the
algebra commands and contributed some code for polynomial operations;
Randal Schwartz, who suggested the `calc-eval` function; Juha
Sarlin, who first worked out how to split Calc into quickly-loading
parts; Bob Weiner, who helped immensely with the Lucid Emacs port; and
Robert J. Chassell, who suggested the Calc Tutorial and exercises as
well as many other things.

Among the books used in the development of Calc were Knuth’s *Art
of Computer Programming* (especially volume II, *Seminumerical
Algorithms*); *Numerical Recipes* by Press, Flannery, Teukolsky,
and Vetterling; Bevington’s *Data Reduction and Error Analysis
for the Physical Sciences*; *Concrete Mathematics* by Graham,
Knuth, and Patashnik; Steele’s *Common Lisp, the Language*; the
*CRC Standard Math Tables* (William H. Beyer, ed.); and
Abramowitz and Stegun’s venerable *Handbook of Mathematical
Functions*. Also, of course, Calc could not have been written without
the excellent *GNU Emacs Lisp Reference Manual*, by Bil Lewis and
Dan LaLiberte.

Final thanks go to Richard Stallman, without whose fine implementations
of the Emacs editor, language, and environment, Calc would have been
finished in two weeks.

---

Next: [Introduction](#Introduction), Previous: [Getting Started](#Getting-Started), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## 2 Tutorial [¶](#Tutorial-1)

This chapter explains how to use Calc and its many features, in
a step-by-step, tutorial way. You are encouraged to run Calc and
work along with the examples as you read (see [Starting Calc](#Starting-Calc)).
If you are already familiar with advanced calculators, you may wish
to skip on to the rest of this manual.

This tutorial describes the standard user interface of Calc only.
The Quick mode and Keypad mode interfaces are fairly
self-explanatory. See [Embedded Mode](#Embedded-Mode), for a description of
the Embedded mode interface.

The easiest way to read this tutorial on-line is to have two windows on
your Emacs screen, one with Calc and one with the Info system. Press
`C-x * t` to set this up; the on-line tutorial will be opened in the
current window and Calc will be started in another window. From the
Info window, the command `C-x * c` can be used to switch to the Calc
window and `C-x * o` can be used to switch back to the Info window.
(If you have a printed copy of the manual you can use that instead; in
that case you only need to press `C-x * c` to start Calc.)

This tutorial is designed to be done in sequence. But the rest of this
manual does not assume you have gone through the tutorial. The tutorial
does not cover everything in the Calculator, but it touches on most
general areas.

You may wish to print out a copy of the Calc Summary and keep notes on
it as you learn Calc. See [About This Manual](#About-This-Manual), to see how to make a
printed summary. See [Calc Summary](#Summary).

* [Basic Tutorial](#Basic-Tutorial)
* [Arithmetic Tutorial](#Arithmetic-Tutorial)
* [Vector/Matrix Tutorial](#Vector_002fMatrix-Tutorial)
* [Types Tutorial](#Types-Tutorial)
* [Algebra and Calculus Tutorial](#Algebra-Tutorial)
* [Programming Tutorial](#Programming-Tutorial)
* [Answers to Exercises](#Answers-to-Exercises)

---

Next: [Arithmetic Tutorial](#Arithmetic-Tutorial), Up: [Tutorial](#Tutorial)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]
