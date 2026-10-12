Next: [Foreword to the Third Edition](#Foreword3), Up: [(dir)](/manual)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]# General Introduction [¶](#General-Introduction)

This file documents `awk`, a program that you can use to select
particular records in a file and perform operations upon them.

Copyright © 1989, 1991, 1992, 1993, 1996–2005, 2007, 2009–2026   
Free Software Foundation, Inc.

  
  

This is Edition 5.4 of GAWK: Effective AWK Programming: A User’s Guide for GNU Awk,
for the 5.4.0 (or later) version of the GNU
implementation of AWK.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being “GNU General Public License”, with the
Front-Cover Texts being “A GNU Manual”, and with the Back-Cover Texts
as in (a) below.
A copy of the license is included in the section entitled
“GNU Free Documentation License”.

1. The FSF’s Back-Cover Text is: “You have the freedom to
   copy and modify this GNU manual.”## Short Table of Contents

* + [Foreword to the Third Edition](#toc-Foreword-to-the-Third-Edition)
  + [Foreword to the Fourth Edition](#toc-Foreword-to-the-Fourth-Edition)
  + [Preface](#toc-Preface-1)
* [Part I:  
  The `awk` Language](#toc-Part-I_003a-The-awk-Language)
  + [1 Getting Started with `awk`](#toc-Getting-Started-with-awk)
  + [2 Running `awk` and `gawk`](#toc-Running-awk-and-gawk)
  + [3 Regular Expressions](#toc-Regular-Expressions)
  + [4 Reading Input Files](#toc-Reading-Input-Files)
  + [5 Printing Output](#toc-Printing-Output)
  + [6 Expressions](#toc-Expressions-1)
  + [7 Patterns, Actions, and Variables](#toc-Patterns_002c-Actions_002c-and-Variables)
  + [8 Arrays in `awk`](#toc-Arrays-in-awk)
  + [9 Functions](#toc-Functions-1)
* [Part II:  
  Problem Solving with `awk`](#toc-Part-II_003a-Problem-Solving-with-awk)
  + [10 A Library of `awk` Functions](#toc-A-Library-of-awk-Functions)
  + [11 Practical `awk` Programs](#toc-Practical-awk-Programs)
* [Part III:  
  Moving Beyond Standard `awk` with `gawk`](#toc-Part-III_003a-Moving-Beyond-Standard-awk-with-gawk)
  + [12 Advanced Features of `gawk`](#toc-Advanced-Features-of-gawk)
  + [13 Internationalization with `gawk`](#toc-Internationalization-with-gawk)
  + [14 Debugging `awk` Programs](#toc-Debugging-awk-Programs)
  + [15 Namespaces in `gawk`](#toc-Namespaces-in-gawk)
  + [16 Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#toc-Arithmetic-and-Arbitrary_002dPrecision-Arithmetic-with-gawk)
  + [17 Writing Extensions for `gawk`](#toc-Writing-Extensions-for-gawk)
* [Part IV:  
  Appendices](#toc-Part-IV_003a-Appendices)
  + [Appendix A The Evolution of the `awk` Language](#toc-The-Evolution-of-the-awk-Language)
  + [Appendix B Installing `gawk`](#toc-Installing-gawk)
  + [Appendix C Implementation Notes](#toc-Implementation-Notes)
  + [Appendix D Basic Programming Concepts](#toc-Basic-Programming-Concepts)
  + [Glossary](#toc-Glossary-1)
  + [GNU General Public License](#toc-GNU-General-Public-License)
  + [GNU Free Documentation License](#toc-GNU-Free-Documentation-License-1)
  + [Index](#toc-Index-1)## Table of Contents

* + [Foreword to the Third Edition](#Foreword3)
  + [Foreword to the Fourth Edition](#Foreword4)
  + [Preface](#Preface)
    - [History of `awk` and `gawk`](#History)
    - [A Rose by Any Other Name](#Names)
    - [Using This Book](#This-Manual)
    - [Typographical Conventions](#Conventions)
      * [Dark Corners](#Dark-Corners)
    - [The GNU Project and This Book](#Manual-History)
    - [How to Contribute](#How-To-Contribute)
    - [Acknowledgments](#Acknowledgments)
* [Part I:  
  The `awk` Language](#Part-I_003a-The-awk-Language)
  + [1 Getting Started with `awk`](#Getting-Started)
    - [1.1 How to Run `awk` Programs](#Running-gawk)
      * [1.1.1 One-Shot Throwaway `awk` Programs](#One_002dshot)
      * [1.1.2 Running `awk` Without Input Files](#Read-Terminal)
      * [1.1.3 Running Long Programs](#Long)
      * [1.1.4 Executable `awk` Programs](#Executable-Scripts)
      * [1.1.5 Comments in `awk` Programs](#Comments)
      * [1.1.6 Shell Quoting Issues](#Quoting)
        + [1.1.6.1 Quoting in MS-Windows Batch Files](#DOS-Quoting)
    - [1.2 Data files for the Examples](#Sample-Data-Files)
    - [1.3 Some Simple Examples](#Very-Simple)
    - [1.4 An Example with Two Rules](#Two-Rules)
    - [1.5 A More Complex Example](#More-Complex)
    - [1.6 `awk` Statements Versus Lines](#Statements_002fLines)
    - [1.7 Other Features of `awk`](#Other-Features)
    - [1.8 When to Use `awk`](#When)
    - [1.9 Summary](#Intro-Summary)
  + [2 Running `awk` and `gawk`](#Invoking-Gawk)
    - [2.1 Invoking `awk`](#Command-Line)
    - [2.2 Command-Line Options](#Options)
    - [2.3 Other Command-Line Arguments](#Other-Arguments)
    - [2.4 Naming Standard Input](#Naming-Standard-Input)
    - [2.5 The Environment Variables `gawk` Uses](#Environment-Variables)
      * [2.5.1 The `AWKPATH` Environment Variable](#AWKPATH-Variable)
      * [2.5.2 The `AWKLIBPATH` Environment Variable](#AWKLIBPATH-Variable)
      * [2.5.3 Other Environment Variables](#Other-Environment-Variables)
    - [2.6 `gawk`’s Exit Status](#Exit-Status)
    - [2.7 Including Other Files into Your Program](#Include-Files)
    - [2.8 Loading Dynamic Extensions into Your Program](#Loading-Shared-Libraries)
    - [2.9 Obsolete Options and/or Features](#Obsolete)
    - [2.10 Undocumented Options and Features](#Undocumented)
    - [2.11 Summary](#Invoking-Summary)
  + [3 Regular Expressions](#Regexp)
    - [3.1 How to Use Regular Expressions](#Regexp-Usage)
    - [3.2 Escape Sequences](#Escape-Sequences)
    - [3.3 Regular Expression Operators](#Regexp-Operators)
      * [3.3.1 Regexp Operators in `awk`](#Regexp-Operator-Details)
      * [3.3.2 Some Notes On Interval Expressions](#Interval-Expressions)
      * [3.3.3 Shortest Match, or “Non-greedy” Regexp Operators](#Non_002dgreedy-operators)
    - [3.4 Using Bracket Expressions](#Bracket-Expressions)
    - [3.5 How Much Text Matches?](#Leftmost-Longest)
    - [3.6 Using Dynamic Regexps](#Computed-Regexps)
    - [3.7 `gawk`-Specific Regexp Operators](#GNU-Regexp-Operators)
    - [3.8 Case Sensitivity in Matching](#Case_002dsensitivity)
    - [3.9 Selecting the Regexp Matching Engine](#Choosing-Matchers)
    - [3.10 Summary](#Regexp-Summary)
  + [4 Reading Input Files](#Reading-Files)
    - [4.1 How Input Is Split into Records](#Records)
      * [4.1.1 Record Splitting with Standard `awk`](#awk-split-records)
      * [4.1.2 Record Splitting with `gawk`](#gawk-split-records)
    - [4.2 Examining Fields](#Fields)
    - [4.3 Nonconstant Field Numbers](#Nonconstant-Fields)
    - [4.4 Changing the Contents of a Field](#Changing-Fields)
    - [4.5 Specifying How Fields Are Separated](#Field-Separators)
      * [4.5.1 Whitespace Normally Separates Fields](#Default-Field-Splitting)
      * [4.5.2 Using Regular Expressions to Separate Fields](#Regexp-Field-Splitting)
      * [4.5.3 Making Each Character a Separate Field](#Single-Character-Fields)
      * [4.5.4 Working With Comma Separated Value Files](#Comma-Separated-Fields)
      * [4.5.5 Setting `FS` from the Command Line](#Command-Line-Field-Separator)
      * [4.5.6 Making the Full Line Be a Single Field](#Full-Line-Fields)
      * [4.5.7 Field-Splitting Summary](#Field-Splitting-Summary)
    - [4.6 Reading Fixed-Width Data](#Constant-Size)
      * [4.6.1 Processing Fixed-Width Data](#Fixed-width-data)
      * [4.6.2 Skipping Intervening Fields](#Skipping-intervening)
      * [4.6.3 Capturing Optional Trailing Data](#Allowing-trailing-data)
      * [4.6.4 Field Values With Fixed-Width Data](#Fields-with-fixed-data)
    - [4.7 Defining Fields by Content](#Splitting-By-Content)
      * [4.7.1 `FS` Versus `FPAT`: A Subtle Difference](#FS-versus-FPAT)
    - [4.8 Checking How `gawk` Is Splitting Records](#Testing-field-creation)
    - [4.9 Multiple-Line Records](#Multiple-Line)
    - [4.10 Explicit Input with `getline`](#Getline)
      * [4.10.1 Using `getline` with No Arguments](#Plain-Getline)
      * [4.10.2 Using `getline` into a Variable](#Getline_002fVariable)
      * [4.10.3 Using `getline` from a File](#Getline_002fFile)
      * [4.10.4 Using `getline` into a Variable from a File](#Getline_002fVariable_002fFile)
      * [4.10.5 Using `getline` from a Pipe](#Getline_002fPipe)
      * [4.10.6 Using `getline` into a Variable from a Pipe](#Getline_002fVariable_002fPipe)
      * [4.10.7 Using `getline` from a Coprocess](#Getline_002fCoprocess)
      * [4.10.8 Using `getline` into a Variable from a Coprocess](#Getline_002fVariable_002fCoprocess)
      * [4.10.9 Points to Remember About `getline`](#Getline-Notes)
      * [4.10.10 Summary of `getline` Variants](#Getline-Summary)
    - [4.11 Reading Input with a Timeout](#Read-Timeout)
    - [4.12 Retrying Reads After Certain Input Errors](#Retrying-Input)
    - [4.13 Directories on the Command Line](#Command_002dline-directories)
    - [4.14 Summary](#Input-Summary)
    - [4.15 Exercises](#Input-Exercises)
  + [5 Printing Output](#Printing)
    - [5.1 The `print` Statement](#Print)
    - [5.2 `print` Statement Examples](#Print-Examples)
    - [5.3 Output Separators](#Output-Separators)
    - [5.4 Controlling Numeric Output with `print`](#OFMT)
    - [5.5 Using `printf` Statements for Fancier Printing](#Printf)
      * [5.5.1 Introduction to the `printf` Statement](#Basic-Printf)
      * [5.5.2 Format-Control Letters](#Control-Letters)
      * [5.5.3 Modifiers for `printf` Formats](#Format-Modifiers)
      * [5.5.4 Examples Using `printf`](#Printf-Examples)
    - [5.6 Redirecting Output of `print` and `printf`](#Redirection)
    - [5.7 Special Files for Standard Preopened Data Streams](#Special-FD)
    - [5.8 Special File names in `gawk`](#Special-Files)
      * [5.8.1 Accessing Other Open Files with `gawk`](#Other-Inherited-Files)
      * [5.8.2 Special Files for Network Communications](#Special-Network)
      * [5.8.3 Special File name Caveats](#Special-Caveats)
    - [5.9 Closing Input and Output Redirections](#Close-Files-And-Pipes)
      * [5.9.1 Using `close()`’s Return Value](#Close-Return-Value)
    - [5.10 Speeding Up Pipe Output](#Noflush)
    - [5.11 Enabling Nonfatal Output](#Nonfatal)
    - [5.12 Summary](#Output-Summary)
    - [5.13 Exercises](#Output-Exercises)
  + [6 Expressions](#Expressions)
    - [6.1 Constants, Variables, and Conversions](#Values)
      * [6.1.1 Constant Expressions](#Constants)
        + [6.1.1.1 Numeric and String Constants](#Scalar-Constants)
        + [6.1.1.2 Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers)
        + [6.1.1.3 Regular Expression Constants](#Regexp-Constants)
      * [6.1.2 Using Regular Expression Constants](#Using-Constant-Regexps)
        + [6.1.2.1 Standard Regular Expression Constants](#Standard-Regexp-Constants)
        + [6.1.2.2 Strongly Typed Regexp Constants](#Strong-Regexp-Constants)
      * [6.1.3 Variables](#Variables)
        + [6.1.3.1 Using Variables in a Program](#Using-Variables)
        + [6.1.3.2 Assigning Variables on the Command Line](#Assignment-Options)
      * [6.1.4 Conversion of Strings and Numbers](#Conversion)
        + [6.1.4.1 How `awk` Converts Between Strings and Numbers](#Strings-And-Numbers)
        + [6.1.4.2 Locales Can Influence Conversion](#Locale-influences-conversions)
    - [6.2 Operators: Doing Something with Values](#All-Operators)
      * [6.2.1 Arithmetic Operators](#Arithmetic-Ops)
      * [6.2.2 String Concatenation](#Concatenation)
      * [6.2.3 Assignment Expressions](#Assignment-Ops)
      * [6.2.4 Increment and Decrement Operators](#Increment-Ops)
    - [6.3 Truth Values and Conditions](#Truth-Values-and-Conditions)
      * [6.3.1 True and False in `awk`](#Truth-Values)
      * [6.3.2 Variable Typing and Comparison Expressions](#Typing-and-Comparison)
        + [6.3.2.1 String Type versus Numeric Type](#Variable-Typing)
        + [6.3.2.2 Comparison Operators](#Comparison-Operators)
        + [6.3.2.3 String Comparison Based on Locale Collating Order](#POSIX-String-Comparison)
      * [6.3.3 Boolean Expressions](#Boolean-Ops)
      * [6.3.4 Conditional Expressions](#Conditional-Exp)
    - [6.4 Function Calls](#Function-Calls)
    - [6.5 Operator Precedence (How Operators Nest)](#Precedence)
    - [6.6 Where You Are Makes a Difference](#Locales)
    - [6.7 Summary](#Expressions-Summary)
  + [7 Patterns, Actions, and Variables](#Patterns-and-Actions)
    - [7.1 Pattern Elements](#Pattern-Overview)
      * [7.1.1 Regular Expressions as Patterns](#Regexp-Patterns)
      * [7.1.2 Expressions as Patterns](#Expression-Patterns)
      * [7.1.3 Specifying Record Ranges with Patterns](#Ranges)
      * [7.1.4 The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)
        + [7.1.4.1 Startup and Cleanup Actions](#Using-BEGIN_002fEND)
        + [7.1.4.2 Input/Output from `BEGIN` and `END` Rules](#I_002fO-And-BEGIN_002fEND)
      * [7.1.5 The `BEGINFILE` and `ENDFILE` Special Patterns](#BEGINFILE_002fENDFILE)
      * [7.1.6 The Empty Pattern](#Empty)
    - [7.2 Using Shell Variables in Programs](#Using-Shell-Variables)
    - [7.3 Actions](#Action-Overview)
    - [7.4 Control Statements in Actions](#Statements)
      * [7.4.1 The `if`-`else` Statement](#If-Statement)
      * [7.4.2 The `while` Statement](#While-Statement)
      * [7.4.3 The `do`-`while` Statement](#Do-Statement)
      * [7.4.4 The `for` Statement](#For-Statement)
      * [7.4.5 The `switch` Statement](#Switch-Statement)
      * [7.4.6 The `break` Statement](#Break-Statement)
      * [7.4.7 The `continue` Statement](#Continue-Statement)
      * [7.4.8 The `next` Statement](#Next-Statement)
      * [7.4.9 The `nextfile` Statement](#Nextfile-Statement)
      * [7.4.10 The `exit` Statement](#Exit-Statement)
    - [7.5 Predefined Variables](#Built_002din-Variables)
      * [7.5.1 Built-in Variables That Control `awk`](#User_002dmodified)
      * [7.5.2 Built-in Variables That Convey Information](#Auto_002dset)
      * [7.5.3 Using `ARGC` and `ARGV`](#ARGC-and-ARGV)
    - [7.6 Summary](#Pattern-Action-Summary)
  + [8 Arrays in `awk`](#Arrays)
    - [8.1 The Basics of Arrays](#Array-Basics)
      * [8.1.1 Introduction to Arrays](#Array-Intro)
      * [8.1.2 Referring to an Array Element](#Reference-to-Elements)
      * [8.1.3 Assigning Array Elements](#Assigning-Elements)
      * [8.1.4 Basic Array Example](#Array-Example)
      * [8.1.5 Scanning All Elements of an Array](#Scanning-an-Array)
      * [8.1.6 Using Predefined Array Scanning Orders with `gawk`](#Controlling-Scanning)
    - [8.2 Using Numbers to Subscript Arrays](#Numeric-Array-Subscripts)
    - [8.3 Using Uninitialized Variables as Subscripts](#Uninitialized-Subscripts)
    - [8.4 The `delete` Statement](#Delete)
    - [8.5 Multidimensional Arrays](#Multidimensional)
      * [8.5.1 Scanning Multidimensional Arrays](#Multiscanning)
    - [8.6 Arrays of Arrays](#Arrays-of-Arrays)
    - [8.7 Summary](#Arrays-Summary)
  + [9 Functions](#Functions)
    - [9.1 Built-in Functions](#Built_002din)
      * [9.1.1 Calling Built-in Functions](#Calling-Built_002din)
      * [9.1.2 Generating Boolean Values](#Boolean-Functions)
      * [9.1.3 Numeric Functions](#Numeric-Functions)
      * [9.1.4 String-Manipulation Functions](#String-Functions)
        + [9.1.4.1 More about ‘`\`’ and ‘`&`’ with `sub()`, `gsub()`, and `gensub()`](#Gory-Details)
      * [9.1.5 Input/Output Functions](#I_002fO-Functions)
      * [9.1.6 Time Functions](#Time-Functions)
      * [9.1.7 Bit-Manipulation Functions](#Bitwise-Functions)
      * [9.1.8 Getting Type Information](#Type-Functions)
      * [9.1.9 String-Translation Functions](#I18N-Functions)
    - [9.2 User-Defined Functions](#User_002ddefined)
      * [9.2.1 Function Definition Syntax](#Definition-Syntax)
      * [9.2.2 Function Definition Examples](#Function-Example)
      * [9.2.3 Calling User-Defined Functions](#Function-Calling)
        + [9.2.3.1 Writing a Function Call](#Calling-A-Function)
        + [9.2.3.2 Controlling Variable Scope](#Variable-Scope)
        + [9.2.3.3 Passing Function Arguments by Value Or by Reference](#Pass-By-Value_002fReference)
        + [9.2.3.4 Other Points About Calling Functions](#Function-Caveats)
      * [9.2.4 The `return` Statement](#Return-Statement)
    - [9.3 Variable Typing Is Dynamic](#Dynamic-Typing)
      * [9.3.1 Dynamic Typing In Standard `awk`](#Dynamic-Typing-Awk)
      * [9.3.2 Dynamic Typing In `gawk`](#Dynamic-Typing-Gawk)
    - [9.4 A Note On Shadowed Variables](#Shadowed-Variables)
    - [9.5 Indirect Function Calls](#Indirect-Calls)
    - [9.6 Summary](#Functions-Summary)
* [Part II:  
  Problem Solving with `awk`](#Part-II_003a-Problem-Solving-with-awk)
  + [10 A Library of `awk` Functions](#Library-Functions)
    - [10.1 Naming Library Function Global Variables](#Library-Names)
    - [10.2 General Programming](#General-Functions)
      * [10.2.1 Converting Strings to Numbers](#Strtonum-Function)
      * [10.2.2 Assertions](#Assert-Function)
      * [10.2.3 Rounding Numbers](#Round-Function)
      * [10.2.4 The Cliff Random Number Generator](#Cliff-Random-Function)
      * [10.2.5 Translating Between Characters and Numbers](#Ordinal-Functions)
      * [10.2.6 Merging an Array into a String](#Join-Function)
      * [10.2.7 Managing the Time of Day](#Getlocaltime-Function)
      * [10.2.8 Reading a Whole File at Once](#Readfile-Function)
      * [10.2.9 Quoting Strings to Pass to the Shell](#Shell-Quoting)
      * [10.2.10 Checking Whether A Value Is Numeric](#Isnumeric-Function)
      * [10.2.11 Producing CSV Data](#To-CSV-Function)
    - [10.3 Data file Management](#Data-File-Management)
      * [10.3.1 Noting Data file Boundaries](#Filetrans-Function)
      * [10.3.2 Rereading the Current File](#Rewind-Function)
      * [10.3.3 Checking for Readable Data files](#File-Checking)
      * [10.3.4 Checking for Zero-Length Files](#Empty-Files)
      * [10.3.5 Treating Assignments as File names](#Ignoring-Assigns)
    - [10.4 Processing Command-Line Options](#Getopt-Function)
    - [10.5 Reading the User Database](#Passwd-Functions)
    - [10.6 Reading the Group Database](#Group-Functions)
    - [10.7 Traversing Arrays of Arrays](#Walking-Arrays)
    - [10.8 Summary](#Library-Functions-Summary)
    - [10.9 Exercises](#Library-Exercises)
  + [11 Practical `awk` Programs](#Sample-Programs)
    - [11.1 Running the Example Programs](#Running-Examples)
    - [11.2 Reinventing Wheels for Fun and Profit](#Clones)
      * [11.2.1 Cutting Out Fields and Columns](#Cut-Program)
      * [11.2.2 Searching for Regular Expressions in Files](#Egrep-Program)
      * [11.2.3 Printing Out User Information](#Id-Program)
      * [11.2.4 Splitting a Large File into Pieces](#Split-Program)
      * [11.2.5 Duplicating Output into Multiple Files](#Tee-Program)
      * [11.2.6 Printing Nonduplicated Lines of Text](#Uniq-Program)
      * [11.2.7 Counting Things](#Wc-Program)
        + [11.2.7.1 Modern Character Sets](#Bytes-vs_002e-Characters)
        + [11.2.7.2 A Brief Introduction To Extensions](#Using-extensions)
        + [11.2.7.3 Code for `wc.awk`](#wc-program)
    - [11.3 A Grab Bag of `awk` Programs](#Miscellaneous-Programs)
      * [11.3.1 Finding Duplicated Words in a Document](#Dupword-Program)
      * [11.3.2 An Alarm Clock Program](#Alarm-Program)
      * [11.3.3 Transliterating Characters](#Translate-Program)
      * [11.3.4 Printing Mailing Labels](#Labels-Program)
      * [11.3.5 Generating Word-Usage Counts](#Word-Sorting)
      * [11.3.6 Removing Duplicates from Unsorted Text](#History-Sorting)
      * [11.3.7 Extracting Programs from Texinfo Source Files](#Extract-Program)
      * [11.3.8 A Simple Stream Editor](#Simple-Sed)
      * [11.3.9 An Easy Way to Use Library Functions](#Igawk-Program)
      * [11.3.10 Finding Anagrams from a Dictionary](#Anagram-Program)
      * [11.3.11 And Now for Something Completely Different](#Signature-Program)
      * [11.3.12 Demonstrating Shortest and Longest Match Operators](#Shortest-match-program)
    - [11.4 Summary](#Programs-Summary)
    - [11.5 Exercises](#Programs-Exercises)
* [Part III:  
  Moving Beyond Standard `awk` with `gawk`](#Part-III_003a-Moving-Beyond-Standard-awk-with-gawk)
  + [12 Advanced Features of `gawk`](#Advanced-Features)
    - [12.1 Allowing Nondecimal Input Data](#Nondecimal-Data)
    - [12.2 Boolean Typed Values](#Boolean-Typed-Values)
    - [12.3 Controlling Array Traversal and Array Sorting](#Array-Sorting)
      * [12.3.1 Controlling Array Traversal](#Controlling-Array-Traversal)
      * [12.3.2 Sorting Array Values and Indices with `gawk`](#Array-Sorting-Functions)
    - [12.4 Two-Way Communications with Another Process](#Two_002dway-I_002fO)
    - [12.5 Using `gawk` for Network Programming](#TCP_002fIP-Networking)
    - [12.6 Profiling Your `awk` Programs](#Profiling)
    - [12.7 Preserving Data Between Runs](#Persistent-Memory)
    - [12.8 Builtin Features versus Extensions](#Extension-Philosophy)
    - [12.9 Summary](#Advanced-Features-Summary)
  + [13 Internationalization with `gawk`](#Internationalization)
    - [13.1 Internationalization and Localization](#I18N-and-L10N)
    - [13.2 GNU `gettext`](#Explaining-gettext)
    - [13.3 Internationalizing `awk` Programs](#Programmer-i18n)
    - [13.4 Translating `awk` Programs](#Translator-i18n)
      * [13.4.1 Extracting Marked Strings](#String-Extraction)
      * [13.4.2 Rearranging `printf` Arguments](#Printf-Ordering)
      * [13.4.3 `awk` Portability Issues](#I18N-Portability)
    - [13.5 A Simple Internationalization Example](#I18N-Example)
    - [13.6 `gawk` Can Speak Your Language](#Gawk-I18N)
    - [13.7 Summary](#I18N-Summary)
  + [14 Debugging `awk` Programs](#Debugger)
    - [14.1 Introduction to the `gawk` Debugger](#Debugging)
      * [14.1.1 Debugging in General](#Debugging-Concepts)
      * [14.1.2 Debugging Concepts](#Debugging-Terms)
      * [14.1.3 `awk` Debugging](#Awk-Debugging)
    - [14.2 Sample `gawk` Debugging Session](#Sample-Debugging-Session)
      * [14.2.1 How to Start the Debugger](#Debugger-Invocation)
      * [14.2.2 Finding the Bug](#Finding-The-Bug)
    - [14.3 Main Debugger Commands](#List-of-Debugger-Commands)
      * [14.3.1 Control of Breakpoints](#Breakpoint-Control)
      * [14.3.2 Control of Execution](#Debugger-Execution-Control)
      * [14.3.3 Viewing and Changing Data](#Viewing-And-Changing-Data)
      * [14.3.4 Working with the Stack](#Execution-Stack)
      * [14.3.5 Obtaining Information About the Program and the Debugger State](#Debugger-Info)
      * [14.3.6 Miscellaneous Commands](#Miscellaneous-Debugger-Commands)
    - [14.4 Readline Support](#Readline-Support)
    - [14.5 Limitations](#Limitations)
    - [14.6 Summary](#Debugging-Summary)
  + [15 Namespaces in `gawk`](#Namespaces)
    - [15.1 Standard `awk`’s Single Namespace](#Global-Namespace)
    - [15.2 Qualified Names](#Qualified-Names)
    - [15.3 The Default Namespace](#Default-Namespace)
    - [15.4 Changing The Namespace](#Changing-The-Namespace)
    - [15.5 Namespace and Component Naming Rules](#Naming-Rules)
    - [15.6 Internal Name Management](#Internal-Name-Management)
    - [15.7 Namespace Example](#Namespace-Example)
    - [15.8 Including A File Without Changing The Namespace](#Inclusion-For-Namespaces)
    - [15.9 Namespaces and Other `gawk` Features](#Namespace-And-Features)
    - [15.10 Summary](#Namespace-Summary)
  + [16 Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic)
    - [16.1 A General Description of Computer Arithmetic](#Computer-Arithmetic)
    - [16.2 Other Stuff to Know](#Math-Definitions)
    - [16.3 Arbitrary-Precision Arithmetic Features in `gawk`](#MPFR-features)
      * [16.3.1 Arbitrary Precision Arithmetic is On Parole!](#MPFR-On-Parole)
      * [16.3.2 Arbitrary Precision Introduction](#MPFR-Intro)
    - [16.4 Floating-Point Arithmetic: Caveat Emptor!](#FP-Math-Caution)
      * [16.4.1 Floating-Point Arithmetic Is Not Exact](#Inexactness-of-computations)
        + [16.4.1.1 Many Numbers Cannot Be Represented Exactly](#Inexact-representation)
        + [16.4.1.2 Be Careful Comparing Values](#Comparing-FP-Values)
        + [16.4.1.3 Errors Accumulate](#Errors-accumulate)
        + [16.4.1.4 Floating Point Values They Didn’t Talk About In School](#Strange-values)
      * [16.4.2 Getting the Accuracy You Need](#Getting-Accuracy)
      * [16.4.3 Try a Few Extra Bits of Precision and Rounding](#Try-To-Round)
      * [16.4.4 Setting the Precision](#Setting-precision)
      * [16.4.5 Setting the Rounding Mode](#Setting-the-rounding-mode)
    - [16.5 Arbitrary-Precision Integer Arithmetic with `gawk`](#Arbitrary-Precision-Integers)
    - [16.6 How To Check If MPFR Is Available](#Checking-for-MPFR)
    - [16.7 Standards Versus Existing Practice](#POSIX-Floating-Point-Problems)
    - [16.8 Summary](#Floating-point-summary)
  + [17 Writing Extensions for `gawk`](#Dynamic-Extensions)
    - [17.1 Introduction](#Extension-Intro)
    - [17.2 Extension Licensing](#Plugin-License)
    - [17.3 How It Works at a High Level](#Extension-Mechanism-Outline)
    - [17.4 API Description](#Extension-API-Description)
      * [17.4.1 Introduction](#Extension-API-Functions-Introduction)
      * [17.4.2 General-Purpose Data Types](#General-Data-Types)
      * [17.4.3 Memory Allocation Functions and Convenience Macros](#Memory-Allocation-Functions)
      * [17.4.4 Constructor Functions](#Constructor-Functions)
      * [17.4.5 Managing MPFR and GMP Values](#API-Ownership-of-MPFR-and-GMP-Values)
      * [17.4.6 Registration Functions](#Registration-Functions)
        + [17.4.6.1 Registering An Extension Function](#Extension-Functions)
        + [17.4.6.2 Registering An Exit Callback Function](#Exit-Callback-Functions)
        + [17.4.6.3 Registering An Extension Version String](#Extension-Version-String)
        + [17.4.6.4 Customized Input Parsers](#Input-Parsers)
        + [17.4.6.5 Customized Output Wrappers](#Output-Wrappers)
        + [17.4.6.6 Customized Two-way Processors](#Two_002dway-processors)
      * [17.4.7 Printing Messages](#Printing-Messages)
      * [17.4.8 Updating `ERRNO`](#Updating-ERRNO)
      * [17.4.9 Requesting Values](#Requesting-Values)
      * [17.4.10 Accessing and Updating Parameters](#Accessing-Parameters)
      * [17.4.11 Symbol Table Access](#Symbol-Table-Access)
        + [17.4.11.1 Variable Access and Update by Name](#Symbol-table-by-name)
        + [17.4.11.2 Variable Access and Update by Cookie](#Symbol-table-by-cookie)
        + [17.4.11.3 Creating and Using Cached Values](#Cached-values)
      * [17.4.12 Array Manipulation](#Array-Manipulation)
        + [17.4.12.1 Array Data Types](#Array-Data-Types)
        + [17.4.12.2 Array Functions](#Array-Functions)
        + [17.4.12.3 Working With All The Elements of an Array](#Flattening-Arrays)
        + [17.4.12.4 How To Create and Populate Arrays](#Creating-Arrays)
      * [17.4.13 Accessing and Manipulating Redirections](#Redirection-API)
      * [17.4.14 API Variables](#Extension-API-Variables)
        + [17.4.14.1 API Version Constants and Variables](#Extension-Versioning)
        + [17.4.14.2 GMP and MPFR Version Information](#Extension-GMP_002fMPFR-Versioning)
        + [17.4.14.3 Informational Variables](#Extension-API-Informational-Variables)
      * [17.4.15 Boilerplate Code](#Extension-API-Boilerplate)
      * [17.4.16 Changes From Version 1 of the API](#Changes-from-API-V1)
    - [17.5 How `gawk` Finds Extensions](#Finding-Extensions)
    - [17.6 Example: Some File Functions](#Extension-Example)
      * [17.6.1 Using `chdir()` and `stat()`](#Internal-File-Description)
      * [17.6.2 C Code for `chdir()` and `stat()`](#Internal-File-Ops)
      * [17.6.3 Integrating the Extensions](#Using-Internal-File-Ops)
    - [17.7 The Sample Extensions in the `gawk` Distribution](#Extension-Samples)
      * [17.7.1 File-Related Functions](#Extension-Sample-File-Functions)
      * [17.7.2 Interface to `fnmatch()`](#Extension-Sample-Fnmatch)
      * [17.7.3 Interface to `fork()`, `wait()`, and `waitpid()`](#Extension-Sample-Fork)
      * [17.7.4 Enabling In-Place File Editing](#Extension-Sample-Inplace)
      * [17.7.5 Character and Numeric values: `ord()` and `chr()`](#Extension-Sample-Ord)
      * [17.7.6 Reading Directories](#Extension-Sample-Readdir)
      * [17.7.7 Reversing Output](#Extension-Sample-Revout)
      * [17.7.8 Two-Way I/O Example](#Extension-Sample-Rev2way)
      * [17.7.9 Dumping and Restoring an Array](#Extension-Sample-Read-write-array)
      * [17.7.10 Reading an Entire File](#Extension-Sample-Readfile)
      * [17.7.11 Extension Time Functions](#Extension-Sample-Time)
      * [17.7.12 API Tests](#Extension-Sample-API-Tests)
    - [17.8 The `gawkextlib` Project](#gawkextlib)
    - [17.9 Summary](#Extension-summary)
    - [17.10 Exercises](#Extension-Exercises)
* [Part IV:  
  Appendices](#Part-IV_003a-Appendices)
  + [Appendix A The Evolution of the `awk` Language](#Language-History)
    - [A.1 Major Changes Between V7 and SVR3.1](#V7_002fSVR3_002e1)
    - [A.2 Changes Between SVR3.1 and SVR4](#SVR4)
    - [A.3 Changes Between SVR4 and POSIX `awk`](#POSIX)
    - [A.4 Extensions in Brian Kernighan’s `awk`](#BTL)
    - [A.5 Extensions in `gawk` Not in POSIX `awk`](#POSIX_002fGNU)
    - [A.6 History of `gawk` Features](#Feature-History)
    - [A.7 Common Extensions Summary](#Common-Extensions)
    - [A.8 Regexp Ranges and Locales: A Long Sad Story](#Ranges-and-Locales)
    - [A.9 Major Contributors to `gawk`](#Contributors)
    - [A.10 Summary](#History-summary)
  + [Appendix B Installing `gawk`](#Installation)
    - [B.1 The `gawk` Distribution](#Gawk-Distribution)
      * [B.1.1 Getting the `gawk` Distribution](#Getting)
      * [B.1.2 Extracting the Distribution](#Extracting)
      * [B.1.3 Contents of the `gawk` Distribution](#Distribution-contents)
    - [B.2 Compiling and Installing `gawk` on Unix-Like Systems](#Unix-Installation)
      * [B.2.1 Compiling `gawk` for Unix-Like Systems](#Quick-Installation)
        + [B.2.1.1 Building With MPFR](#Compiling-with-MPFR)
      * [B.2.2 Shell Startup Files](#Shell-Startup-Files)
      * [B.2.3 Additional Configuration Options](#Additional-Configuration-Options)
      * [B.2.4 The Configuration Process](#Configuration-Philosophy)
      * [B.2.5 Compiling from Git](#Compiling-from-Git)
      * [B.2.6 Building the Documentation](#Building-the-Documentation)
    - [B.3 Installation on Other Operating Systems](#Non_002dUnix-Installation)
      * [B.3.1 Installation on MS-Windows](#PC-Installation)
        + [B.3.1.1 Installing a Prepared Distribution for MS-Windows Systems](#PC-Binary-Installation)
        + [B.3.1.2 Compiling `gawk` for PC Operating Systems](#PC-Compiling)
        + [B.3.1.3 Using `gawk` on PC Operating Systems](#PC-Using)
        + [B.3.1.4 Using `gawk` In The Cygwin Environment](#Cygwin)
        + [B.3.1.5 Using `gawk` In The MSYS Environment](#MSYS)
      * [B.3.2 Compiling and Installing `gawk` on OpenVMS](#OpenVMS-Installation)
        + [B.3.2.1 Compiling `gawk` on OpenVMS](#OpenVMS-Compilation)
        + [B.3.2.2 Compiling `gawk` Dynamic Extensions on OpenVMS](#OpenVMS-Dynamic-Extensions)
        + [B.3.2.3 Installing `gawk` on OpenVMS](#OpenVMS-Installation-Details)
        + [B.3.2.4 Running `gawk` on OpenVMS](#OpenVMS-Running)
        + [B.3.2.5 The OpenVMS GNV Project](#OpenVMS-GNV)
    - [B.4 Reporting Problems and Bugs](#Bugs)
      * [B.4.1 Defining What Is and What Is Not A Bug](#Bug-definition)
      * [B.4.2 Submitting Bug Reports](#Bug-address)
      * [B.4.3 Please Don’t Post Bug Reports to USENET](#Usenet)
      * [B.4.4 What To Do If You Think There Is A Performance Issue](#Performance-bugs)
      * [B.4.5 Where To Send Non-bug Questions](#Asking-for-help)
      * [B.4.6 Reporting Problems with Non-Unix Ports](#Maintainers)
    - [B.5 Other Freely Available `awk` Implementations](#Other-Versions)
    - [B.6 Summary](#Installation-summary)
  + [Appendix C Implementation Notes](#Notes)
    - [C.1 Downward Compatibility and Debugging](#Compatibility-Mode)
    - [C.2 Making Additions to `gawk`](#Additions)
      * [C.2.1 Accessing The `gawk` Git Repository](#Accessing-The-Source)
      * [C.2.2 Adding New Features](#Adding-Code)
      * [C.2.3 Porting `gawk` to a New Operating System](#New-Ports)
      * [C.2.4 Why Generated Files Are Kept In Git](#Derived-Files)
    - [C.3 Probable Future Extensions](#Future-Extensions)
    - [C.4 Some Limitations of the Implementation](#Implementation-Limitations)
    - [C.5 Extension API Design](#Extension-Design)
      * [C.5.1 Problems With The Old Mechanism](#Old-Extension-Problems)
      * [C.5.2 Goals For A New Mechanism](#Extension-New-Mechanism-Goals)
      * [C.5.3 Other Design Decisions](#Extension-Other-Design-Decisions)
      * [C.5.4 Room For Future Growth](#Extension-Future-Growth)
    - [C.6 Summary](#Notes-summary)
  + [Appendix D Basic Programming Concepts](#Basic-Concepts)
    - [D.1 What a Program Does](#Basic-High-Level)
    - [D.2 Data Values in a Computer](#Basic-Data-Typing)
  + [Glossary](#Glossary)
  + [GNU General Public License](#Copying)
  + [GNU Free Documentation License](#GNU-Free-Documentation-License)
    - [ADDENDUM: How to use this License for your documents](#ADDENDUM_003a-How-to-use-this-License-for-your-documents)
  + [Index](#Index)

---

Next: [Foreword to the Fourth Edition](#Foreword4), Previous: [General Introduction](#Top), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Foreword to the Third Edition [¶](#Foreword-to-the-Third-Edition)

Arnold Robbins and I are good friends. We were introduced
in 1990
by circumstances—and our favorite programming language, AWK.
The circumstances started a couple of years
earlier. I was working at a new job and noticed an unplugged
Unix computer sitting in the corner. No one knew how to use it,
and neither did I. However,
a couple of days later, it was running, and
I was `root` and the one-and-only user.
That day, I began the transition from statistician to Unix programmer.

On one of many trips to the library or bookstore in search of
books on Unix, I found the gray AWK book, a.k.a.
Alfred V. Aho, Brian W. Kernighan, and
Peter J. Weinberger’s The AWK Programming Language (Addison-Wesley,
1988). `awk`’s simple programming paradigm—find a pattern in the
input and then perform an action—often reduced complex or tedious
data manipulations to a few lines of code. I was excited to try my
hand at programming in AWK.

Alas, the `awk` on my computer was a limited version of the
language described in the gray book. I discovered that my computer
had “old `awk`” and the book described
“new `awk`.”
I learned that this was typical; the old version refused to step
aside or relinquish its name. If a system had a new `awk`, it was
invariably called `nawk`, and few systems had it.
The best way to get a new `awk` was to `ftp` the source code for
`gawk` from `prep.ai.mit.edu`. `gawk` was a version of
new `awk` written by David Trueman and Arnold, and available under
the GNU General Public License.

(Incidentally,
it’s no longer difficult to find a new `awk`. `gawk` ships with
GNU/Linux, and you can download binaries or source code for almost
any system; my wife uses `gawk` on her VMS box.)

My Unix system started out unplugged from the wall; it certainly was not
plugged into a network. So, oblivious to the existence of `gawk`
and the Unix community in general, and desiring a new `awk`, I wrote
my own, called `mawk`.
Before I was finished, I knew about `gawk`,
but it was too late to stop, so I eventually posted
to a `comp.sources` newsgroup.

A few days after my posting, I got a friendly email
from Arnold introducing
himself. He suggested we share design and algorithms and
attached a draft of the POSIX standard so
that I could update `mawk` to support language extensions added
after publication of The AWK Programming Language.

Frankly, if our roles had
been reversed, I would not have been so open and we probably would
have never met. I’m glad we did meet.
He is an AWK expert’s AWK expert and a genuinely nice person.
Arnold contributes significant amounts of his
expertise and time to the Free Software Foundation.

This book is the `gawk` reference manual, but at its core it
is a book about AWK programming that
will appeal to a wide audience.
It is a definitive reference to the AWK language as defined by the
1987 Bell Laboratories release and codified in the 1992 POSIX Utilities
standard.

On the other hand, the novice AWK programmer can study
a wealth of practical programs that emphasize
the power of AWK’s basic idioms:
data-driven control flow, pattern matching with regular expressions,
and associative arrays.
Those looking for something new can try out `gawk`’s
interface to network protocols via special `/inet` files.

The programs in this book make clear that an AWK program is
typically much smaller and faster to develop than
a counterpart written in C.
Consequently, there is often a payoff to prototyping an
algorithm or design in AWK to get it running quickly and expose
problems early. Often, the interpreted performance is adequate
and the AWK prototype becomes the product.

The new `pgawk` (profiling `gawk`), produces
program execution counts.
I recently experimented with an algorithm that for
*n*
lines of input, exhibited
~ C n^2
performance, while
theory predicted
~ C n log n
behavior. A few minutes poring
over the `awkprof.out` profile pinpointed the problem to
a single line of code. `pgawk` is a welcome addition to
my programmer’s toolbox.

Arnold has distilled over a decade of experience writing and
using AWK programs, and developing `gawk`, into this book. If you use
AWK or want to learn how, then read this book.

```
Michael Brennan
Author of mawk
March 2001
```

---

Next: [Preface](#Preface), Previous: [Foreword to the Third Edition](#Foreword3), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Foreword to the Fourth Edition [¶](#Foreword-to-the-Fourth-Edition)

Some things don’t change. Thirteen years ago I wrote:
“If you use AWK or want to learn how, then read this book.”
True then, and still true today.

Learning to use a programming language is about more than mastering the
syntax. One needs to acquire an understanding of how to use the
features of the language to solve practical programming problems.
A focus of this book is many examples that show how to use AWK.

Some things do change. Our computers are much faster and have more memory.
Consequently, speed and storage inefficiencies of a high-level language
matter less. Prototyping in AWK and then rewriting in C for performance
reasons happens less, because more often the prototype is fast enough.

Of course, there are computing operations that are best done in C or C++.
With `gawk` 4.1 and later, you do not have to choose between writing
your program in AWK or in C/C++. You can write most of your
program in AWK and the aspects that require C/C++ capabilities can be written
in C/C++, and then the pieces glued together when the `gawk` module loads
the C/C++ module as a dynamic plug-in.
[Writing Extensions for `gawk`](#Dynamic-Extensions),
has all the
details, and, as expected, many examples to help you learn the ins and outs.

I enjoy programming in AWK and had fun (re)reading this book.
I think you will too.

```
Michael Brennan
Author of mawk
October 2014
```

---

Next: [Getting Started with `awk`](#Getting-Started), Previous: [Foreword to the Fourth Edition](#Foreword4), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Preface [¶](#Preface-1)

Several kinds of tasks occur repeatedly when working with text files.
You might want to extract certain lines and discard the rest. Or you
may need to make changes wherever certain patterns appear, but leave the
rest of the file alone. Such jobs are often easy with `awk`.
The `awk` utility interprets a special-purpose programming
language that makes it easy to handle simple data-reformatting jobs.

The GNU implementation of `awk` is called `gawk`; if you
invoke it with the proper options or environment variables,
it is fully compatible with
the POSIX[1](#FOOT1)
specification of the `awk` language
and with the Unix version of `awk` maintained
by Brian Kernighan.
This means that all
properly written `awk` programs should work with `gawk`.
So most of the time, we don’t distinguish between `gawk` and other
`awk` implementations.

Using `awk` you can:

* Manage small, personal databases
* Generate reports
* Validate data
* Produce indexes and perform other document-preparation tasks
* Experiment with algorithms that you can adapt later to other computer
  languages

In addition,
`gawk`
provides facilities that make it easy to:

* Extract bits and pieces of data for processing
* Sort data
* Perform simple network communications
* Profile and debug `awk` programs
* Extend the language with functions written in C or C++

This Web page teaches you about the `awk` language and
how you can use it effectively. You should already be familiar with basic
system commands, such as `cat` and `ls`,[2](#FOOT2) as well as basic shell
facilities, such as input/output (I/O) redirection and pipes.

Implementations of the `awk` language are available for many
different computing environments. This Web page, while describing
the `awk` language in general, also describes the particular
implementation of `awk` called `gawk` (which stands for
“GNU `awk`”). `gawk` runs on a broad range of Unix systems,
ranging from Intel-architecture PC-based computers
up through large-scale systems.
`gawk` has also been ported to macOS, z/OS,
Microsoft Windows
(all versions),
and OpenVMS.[3](#FOOT3)

* [History of `awk` and `gawk`](#History)
* [A Rose by Any Other Name](#Names)
* [Using This Book](#This-Manual)
* [Typographical Conventions](#Conventions)
* [The GNU Project and This Book](#Manual-History)
* [How to Contribute](#How-To-Contribute)
* [Acknowledgments](#Acknowledgments)

---

Next: [A Rose by Any Other Name](#Names), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### History of `awk` and `gawk` [¶](#History-of-awk-and-gawk)

| Recipe for a Programming Language |
| --- |
| |  |  |  | | --- | --- | --- | |  | 1 part `egrep` | 1 part `snobol` | |  | 2 parts `ed` | 3 parts C |   Blend all parts well using `lex` and `yacc`. Document minimally and release.  After eight years, add another part `egrep` and two more parts C. Document very well and release.  After 35 more years, add Unicode and CSV support, sprinkle lightly with a few choice features from `gawk`, document very well again, and release. |

The name `awk` comes from the initials of its designers: Alfred V.
Aho, Peter J. Weinberger, and Brian W. Kernighan. The original version of
`awk` was written in 1977 at AT&T Bell Laboratories.
In 1985, a new version made the programming
language more powerful, introducing user-defined functions, multiple input
streams, and computed regular expressions.
This new version became widely available with Unix System V
Release 3.1 (1987).
The version in System V Release 4 (1989) added some new features and cleaned
up the behavior in some of the “dark corners” of the language.
The specification for `awk` in the POSIX Command Language
and Utilities standard further clarified the language.
Both the `gawk` designers and the original `awk` designers at Bell Laboratories
provided feedback for the POSIX specification.

Paul Rubin wrote `gawk` in 1986.
Jay Fenlason completed it, with advice from Richard Stallman. John Woods
contributed parts of the code as well. In 1988 and 1989, David Trueman, with
help from me, thoroughly reworked `gawk` for compatibility
with the newer `awk`.
Circa 1994, I became the primary maintainer.
Current development focuses on bug fixes,
performance improvements, standards compliance, and, occasionally, new features.

In May 1997, Jürgen Kahrs felt the need for network access
from `awk`, and with a little help from me, set about adding
features to do this for `gawk`. At that time, he also
wrote the bulk of
TCP/IP Internetworking with `gawk`
(a separate document, available as part of the `gawk` distribution).
His code finally became part of the main `gawk` distribution
with `gawk` version 3.1.

John Haque rewrote the `gawk` internals, in the process providing
an `awk`-level debugger. This version became available as
`gawk` version 4.0 in 2011.

See [Major Contributors to `gawk`](#Contributors)
for a full list of those who have made important contributions to `gawk`.

---

Next: [Using This Book](#This-Manual), Previous: [History of `awk` and `gawk`](#History), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### A Rose by Any Other Name [¶](#A-Rose-by-Any-Other-Name)

The `awk` language has evolved over the years. Full details are
provided in [The Evolution of the `awk` Language](#Language-History).
The language described in this Web page
is often referred to as “new `awk`.”
By analogy, the original version of `awk` is
referred to as “old `awk`.”

On most current systems, when you run the `awk` utility
you get some version of new `awk`.[4](#FOOT4) If your system’s standard
`awk` is the old one, you will see something like this
if you try the following test program:

```
$ awk 1 /dev/null
error→ awk: syntax error near line 1
error→ awk: bailing out near line 1
```

In this case, you should find a version of new `awk`,
or just install `gawk`!

Throughout this Web page, whenever we refer to a language feature
that should be available in any complete implementation of POSIX `awk`,
we simply use the term `awk`. When referring to a feature that is
specific to the GNU implementation, we use the term `gawk`.

---

Next: [Typographical Conventions](#Conventions), Previous: [A Rose by Any Other Name](#Names), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Using This Book [¶](#Using-This-Book)

The term `awk` refers to a particular program as well as to the language you
use to tell this program what to do. When we need to be careful, we call
the language “the `awk` language,”
and the program “the `awk` utility.”
This Web page explains
both how to write programs in the `awk` language and how to
run the `awk` utility.
The term “`awk` program” refers to a program written by you in
the `awk` programming language.

Primarily, this Web page explains the features of `awk`
as defined in the POSIX standard. It does so in the context of the
`gawk` implementation. While doing so, it also
attempts to describe important differences between `gawk`
and other `awk`
implementations.[5](#FOOT5)
Finally, it notes any `gawk` features that are not in
the POSIX standard for `awk`.

This Web page has the difficult task of being both a tutorial and a reference.
If you are a novice, feel free to skip over details that seem too complex.
You should also ignore the many cross-references; they are for the
expert user and for the Info and
[HTML](https://www.gnu.org/software/gawk/manual/)
versions of the Web page.

There are sidebars
scattered throughout the Web page.
They add a more complete explanation of points that are relevant, but not likely
to be of interest on first reading.
All appear in the index, under the heading “sidebar.”

Most of the time, the examples use complete `awk` programs.
Some of the more advanced sections show only the part of the `awk`
program that illustrates the concept being described.

Although this Web page is aimed principally at people who have not been
exposed
to `awk`, there is a lot of information here that even the `awk`
expert should find useful. In particular, the description of POSIX
`awk` and the example programs in
[A Library of `awk` Functions](#Library-Functions), and
in
[Practical `awk` Programs](#Sample-Programs),
should be of interest.

This Web page is split into several parts, as follows:

* Part I describes the `awk` language and the `gawk` program in detail.
  It starts with the basics, and continues through all of the features of `awk`.
  It contains the following chapters:
  + [Getting Started with `awk`](#Getting-Started),
    provides the essentials you need to know to begin using `awk`.
  + [Running `awk` and `gawk`](#Invoking-Gawk),
    describes how to run `gawk`, the meaning of its
    command-line options, and how it finds `awk`
    program source files.
  + [Regular Expressions](#Regexp),
    introduces regular expressions in general, and in particular the flavors
    supported by POSIX `awk` and `gawk`.
  + [Reading Input Files](#Reading-Files),
    describes how `awk` reads your data.
    It introduces the concepts of records and fields, as well
    as the `getline` function.
    I/O redirection is first described here.
    Network I/O is also briefly introduced here.
  + [Printing Output](#Printing),
    describes how `awk` programs can produce output with
    `print` and `printf`.
  + [Expressions](#Expressions),
    describes expressions, which are the basic building blocks
    for getting most things done in a program.
  + [Patterns, Actions, and Variables](#Patterns-and-Actions),
    describes how to write patterns for matching records, actions for
    doing something when a record is matched, and the predefined variables
    `awk` and `gawk` use.
  + [Arrays in `awk`](#Arrays),
    covers `awk`’s one-and-only data structure: the associative array.
    Deleting array elements and whole arrays is described, as well as
    sorting arrays in `gawk`. The chapter also describes how
    `gawk` provides arrays of arrays.
  + [Functions](#Functions),
    describes the built-in functions `awk` and `gawk` provide,
    as well as how to define your own functions. It also discusses how
    `gawk` lets you call functions indirectly.
* Part II shows how to use `awk` and `gawk` for problem solving.
  There is lots of code here for you to read and learn from.
  This part contains the following chapters:
  + [A Library of `awk` Functions](#Library-Functions), provides a number of functions meant to
    be used from main `awk` programs.
  + [Practical `awk` Programs](#Sample-Programs),
    provides many sample `awk` programs.

  Reading these two chapters allows you to see `awk`
  solving real problems.
* Part III focuses on features specific to `gawk`.
  It contains the following chapters:
  + [Advanced Features of `gawk`](#Advanced-Features),
    describes a number of advanced features.
    Of particular note
    are the abilities to control the order of array traversal,
    have two-way communications with another process,
    perform TCP/IP networking, and
    profile your `awk` programs.
  + [Internationalization with `gawk`](#Internationalization),
    describes special features for translating program
    messages into different languages at runtime.
  + [Debugging `awk` Programs](#Debugger), describes the `gawk` debugger.
  + [Namespaces in `gawk`](#Namespaces), describes how `gawk` allows variables and/or
    functions of the same name to be in different namespaces.
  + [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic),
    describes advanced arithmetic facilities.
  + [Writing Extensions for `gawk`](#Dynamic-Extensions), describes how to add new variables and
    functions to `gawk` by writing extensions in C or C++.
* Part IV provides the appendices, the Glossary, and two licenses that cover
  the `gawk` source code and this Web page, respectively.
  It contains the following appendices:
  + [The Evolution of the `awk` Language](#Language-History),
    describes how the `awk` language has evolved since
    its first release to the present. It also describes how `gawk`
    has acquired features over time.
  + [Installing `gawk`](#Installation),
    describes how to get `gawk`, how to compile it
    on POSIX-compatible systems,
    and how to compile and use it on different
    non-POSIX systems. It also describes how to report bugs
    in `gawk` and where to get other freely
    available `awk` implementations.
  + [Implementation Notes](#Notes),
    describes how to disable `gawk`’s extensions, as
    well as how to contribute new code to `gawk`,
    and some possible future directions for `gawk` development.
  + [Basic Programming Concepts](#Basic-Concepts),
    provides some very cursory background material for those who
    are completely unfamiliar with computer programming.
  + The [Glossary](#Glossary), defines most, if not all, of the significant terms used
    throughout the Web page. If you find terms that you aren’t familiar with,
    try looking them up here.
  + [GNU General Public License](#Copying), and
    [GNU Free Documentation License](#GNU-Free-Documentation-License),
    present the licenses that cover the `gawk` source code
    and this Web page, respectively.

---

Next: [The GNU Project and This Book](#Manual-History), Previous: [Using This Book](#This-Manual), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Typographical Conventions [¶](#Typographical-Conventions)

This Web page is written in [Texinfo](https://www.gnu.org/software/texinfo/),
the GNU documentation formatting language.
A single Texinfo source file is used to produce both the printed and online
versions of the documentation.
Because of this, the typographical conventions
are slightly different than in other books you may have read.

Examples you would type at the command line are preceded by the common
shell primary and secondary prompts, ‘`$`’ and ‘`>`’, respectively.
Input that you type is shown `like this`.
Output from the command is preceded by the glyph “-|”.
This typically represents the command’s standard output.
Error messages and other output on the command’s standard error are preceded
by the glyph “error→”. For example:

```
$ echo hi on stdout
-| hi on stdout
$ echo hello on stderr 1>&2
error→ hello on stderr
```

In the text, almost anything related to programming, such as
command names,
variable and function names, and string, numeric and regexp constants
appear in `this font`. Code fragments
appear in the same font and quoted, ‘`like this`’.
Things that are replaced by the user or programmer
appear in this font.
Options look like this: `-f`.
File names are indicated like this: `/path/to/ourfile`.
Some things are
emphasized *like this*, and if a point needs to be made
strongly, it is done **like this**.
The first occurrence of
a new term is usually its *definition* and appears in the same
font as the previous occurrence of “definition” in this sentence.

Characters that you type at the keyboard look `like this`. In particular,
there are special characters called “control characters.” These are
characters that you type by holding down both the `CONTROL` key and
another key, at the same time. For example, a `Ctrl-d` is typed
by first pressing and holding the `CONTROL` key, next
pressing the `d` key, and finally releasing both keys.

For the sake of brevity, throughout this Web page, we refer to
Brian Kernighan’s version of `awk` as “BWK `awk`.”
(See [Other Freely Available `awk` Implementations](#Other-Versions) for information on his and other versions.)

* [Dark Corners](#Dark-Corners)

#### Dark Corners [¶](#Dark-Corners)

> *Dark corners are basically fractal—no matter how much
> you illuminate, there’s always a smaller but darker one.*

— *Brian Kernighan*

Until the POSIX standard (and GAWK: Effective AWK Programming),
many features of `awk` were either poorly documented or not
documented at all. Descriptions of such features
(often called “dark corners”) are noted in this Web page with
“(d.c.).”
They also appear in the index under the heading “dark corner.”

But, as noted by the opening quote, any coverage of dark
corners is by definition incomplete.

Extensions to the standard `awk` language that are supported by
more than one `awk` implementation are marked
“(c.e.),” and listed in the index under “common extensions”
and “extensions, common.”

---

Next: [How to Contribute](#How-To-Contribute), Previous: [Typographical Conventions](#Conventions), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### The GNU Project and This Book [¶](#The-GNU-Project-and-This-Book)

The Free Software Foundation (FSF) is a nonprofit organization dedicated
to the production and distribution of freely distributable software.
It was founded by Richard M. Stallman, the author of the original
Emacs editor. GNU Emacs is the most widely used version of Emacs today.

The GNU[6](#FOOT6)
Project is an ongoing effort on the part of the Free Software
Foundation to create a complete, freely distributable, POSIX-compliant
computing environment.
The FSF uses the GNU General Public License (GPL) to ensure that
its software’s
source code is always available to the end user.
A copy of the GPL is included
in this Web page
for your reference
(see [GNU General Public License](#Copying)).
The GPL applies to the C language source code for `gawk`.
To find out more about the FSF and the GNU Project online,
see [the GNU Project’s home page](https://www.gnu.org).
This Web page may also be read from
[GNU’s website](https://www.gnu.org/software/gawk/manual/).

A shell, an editor (Emacs), highly portable optimizing C, C++, and
Objective-C compilers, a symbolic debugger and dozens of large and
small utilities (such as `gawk`), have all been completed and are
freely available. The GNU operating
system kernel (the HURD), has been released but remains in an early
stage of development.

Until the GNU operating system is more fully developed, you should
consider using GNU/Linux, a freely distributable, Unix-like operating
system for Intel,
Power Architecture,
Sun SPARC, IBM S/390, and other
systems.[7](#FOOT7)
Many GNU/Linux distributions are
available for download from the Internet.

The Web page you are reading is actually free—at least, the
information in it is free to anyone. The machine-readable
source code for the Web page comes with `gawk`.
(Take a moment to check the Free Documentation
License in [GNU Free Documentation License](#GNU-Free-Documentation-License).)

The Web page itself has gone through multiple previous editions.
Paul Rubin wrote the very first draft of The GAWK Manual;
it was around 40 pages long.
Diane Close and Richard Stallman improved it, yielding a
version that was
around 90 pages and barely described the original, “old”
version of `awk`.

I started working with that version in the fall of 1988.
As work on it progressed,
the FSF published several preliminary versions (numbered 0.x).
In 1996, edition 1.0 was released with `gawk` 3.0.0.
The FSF published the first two editions under
the title The GNU Awk User’s Guide.

This edition maintains the basic structure of the previous editions.
For FSF edition 4.0, the content was thoroughly reviewed and updated. All
references to `gawk` versions prior to 4.0 were removed.
Of significant note for that edition was the addition of [Debugging `awk` Programs](#Debugger).

For FSF edition
5.0,
the content has been reorganized into parts,
and the major new additions are [Arithmetic and Arbitrary-Precision Arithmetic with `gawk`](#Arbitrary-Precision-Arithmetic),
and [Writing Extensions for `gawk`](#Dynamic-Extensions).

This Web page will undoubtedly continue to evolve. If you
find an error in the Web page, please report it! See [Reporting Problems and Bugs](#Bugs)
for information on submitting problem reports electronically.

---

Next: [Acknowledgments](#Acknowledgments), Previous: [The GNU Project and This Book](#Manual-History), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### How to Contribute [¶](#How-to-Contribute)

As the maintainer of GNU `awk`, I once thought that I would be
able to manage a collection of publicly available `awk` programs
and I even solicited contributions. Making things available on the Internet
helps keep the `gawk` distribution down to manageable size.

The initial collection of material, such as it is, is still available
at <ftp://ftp.freefriends.org/arnold/Awkstuff>.

In the hopes of doing something broader, I acquired the
`awklang.org` domain. Late in 2017, a volunteer took on the task
of managing it.

If you have written an interesting `awk` program that
you would like to share with the rest of the world, please see
<http://www.awklang.org> and use the “Contact” link.

If you have written a `gawk` extension, please see
[The `gawkextlib` Project](#gawkextlib).

---

Previous: [How to Contribute](#How-To-Contribute), Up: [Preface](#Preface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### Acknowledgments [¶](#Acknowledgments-1)

The initial draft of The GAWK Manual had the following acknowledgments:

> Many people need to be thanked for their assistance in producing this
> manual. Jay Fenlason contributed many ideas and sample programs. Richard
> Mlynarik and Robert Chassell gave helpful comments on drafts of this
> manual. The paper A Supplemental Document for AWK by John W.
> Pierce of the Chemistry Department at UC San Diego, pinpointed several
> issues relevant both to `awk` implementation and to this manual, that
> would otherwise have escaped us.

I would like to acknowledge Richard M. Stallman, for his vision of a
better world and for his courage in founding the FSF and starting the
GNU Project.

Earlier editions of this Web page had the following acknowledgements:

> The following people (in alphabetical order)
> provided helpful comments on various
> versions of this book:
> Rick Adams,
> Dr. Nelson H.F. Beebe,
> Karl Berry,
> Dr. Michael Brennan,
> Rich Burridge,
> Claire Cloutier,
> Diane Close,
> Scott Deifik,
> Christopher (“Topher”) Eliot,
> Jeffrey Friedl,
> Dr. Darrel Hankerson,
> Michal Jaegermann,
> Dr. Richard J. LeBlanc,
> Michael Lijewski,
> Pat Rankin,
> Miriam Robbins,
> Mary Sheehan,
> and
> Chuck Toporek.
>
>
>
> Robert J. Chassell provided much valuable advice on
> the use of Texinfo.
> He also deserves special thanks for
> convincing me *not* to title this Web page
> How to Gawk Politely.
> Karl Berry helped significantly with the TeX part of Texinfo.
>
>
>
>
>
> I would like to thank Marshall and Elaine Hartholz of Seattle and
> Dr. Bert and Rita Schreiber of Detroit for large amounts of quiet vacation
> time in their homes, which allowed me to make significant progress on
> this Web page and on `gawk` itself.
>
> Phil Hughes of SSC
> contributed in a very important way by loaning me his laptop GNU/Linux
> system, not once, but twice, which allowed me to do a lot of work while
> away from home.
>
> David Trueman deserves special credit; he has done a yeoman job
> of evolving `gawk` so that it performs well and without bugs.
> Although he is no longer involved with `gawk`,
> working with him on this project was a significant pleasure.
>
>
>
>
> The intrepid members of the GNITS mailing list, and most notably Ulrich
> Drepper, provided invaluable help and feedback for the design of the
> internationalization features.
>
> Chuck Toporek, Mary Sheehan, and Claire Cloutier of O’Reilly & Associates contributed
> significant editorial help for this Web page for the
> 3.1 release of `gawk`.

Dr. Nelson Beebe,
Andreas Buening,
Dr. Manuel Collado,
Antonio Colombo,
Stephen Davies,
Scott Deifik,
Akim Demaille,
Daniel Richard G.,
Juan Manuel Guerrero,
Darrel Hankerson,
Michal Jaegermann,
Jürgen Kahrs,
Stepan Kasal,
John Malmberg,
Chet Ramey,
Pat Rankin,
Andrew Schorr,
Corinna Vinschen,
and Eli Zaretskii
(in alphabetical order)
make up the current `gawk` “crack portability team.” Without
their hard work and help, `gawk` would not be nearly the robust,
portable program it is today. It has been and continues to be a pleasure
working with this team of fine people.

Notable code and documentation contributions were made by
a number of people. See [Major Contributors to `gawk`](#Contributors) for the full list.

Thanks to Michael Brennan for the Forewords.

Thanks to Patrice Dumas for the new `makeinfo` program.
Thanks to Karl Berry for his past work on Texinfo, and
to Gavin Smith, who continues to work to improve
the Texinfo markup language.

Robert P.J. Day, Michael Brennan, and Brian Kernighan kindly acted as
reviewers for the 2015 edition of this Web page. Their feedback
helped improve the final work.

I would also like to thank Brian Kernighan for his invaluable assistance during the
testing and debugging of `gawk`, and for his ongoing
help and advice in clarifying numerous points about the language.
We could not have done nearly as good a job on either `gawk`
or its documentation without his help.

Brian is in a class by himself as a programmer and technical
author. I have to thank him (yet again) for his ongoing friendship
and for being a role model to me for over 30 years!
Having him as a reviewer is an exciting privilege. It has also
been extremely humbling...

I must thank my wonderful wife, Miriam, for her patience through
the many versions of this project, for her proofreading,
and for sharing me with the computer.
I would like to thank my parents for their love, and for the grace with
which they raised and educated me.
Finally, I also must acknowledge my gratitude to G-d, for the many opportunities
He has sent my way, as well as for the gifts He has given me with which to
take advantage of those opportunities.

  
  

Arnold Robbins   
Nof Ayalon   
Israel   
March, 2020

---

Next: [Running `awk` and `gawk`](#Invoking-Gawk), Previous: [Preface](#Preface), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]# Part I: The `awk` Language [¶](#Part-I_003a-The-awk-Language)

---

* [Getting Started with `awk`](#Getting-Started)
* [Running `awk` and `gawk`](#Invoking-Gawk)
* [Regular Expressions](#Regexp)
* [Reading Input Files](#Reading-Files)
* [Printing Output](#Printing)
* [Expressions](#Expressions)
* [Patterns, Actions, and Variables](#Patterns-and-Actions)
* [Arrays in `awk`](#Arrays)
* [Functions](#Functions)## 1 Getting Started with `awk` [¶](#Getting-Started-with-awk)

The basic function of `awk` is to search files for lines (or other
units of text) that contain certain patterns. When a line matches one
of the patterns, `awk` performs specified actions on that line.
`awk` continues to process input lines in this way until it reaches
the end of the input files.

Programs in `awk` are different from programs in most other languages,
because `awk` programs are *data driven* (i.e., you describe
the data you want to work with and then what to do when you find it).
Most other languages are *procedural*; you have to describe, in great
detail, every step the program should take. When working with procedural
languages, it is usually much
harder to clearly describe the data your program will process.
For this reason, `awk` programs are often refreshingly easy to
read and write.

When you run `awk`, you specify an `awk` *program* that
tells `awk` what to do. The program consists of a series of
*rules* (it may also contain *function definitions*,
an advanced feature that we will ignore for now;
see [User-Defined Functions](#User_002ddefined)). Each rule specifies one
pattern to search for and one action to perform
upon finding the pattern.

Syntactically, a rule consists of a *pattern* followed by an
*action*. The action is enclosed in braces to separate it from the
pattern. Newlines usually separate rules. Therefore, an `awk`
program looks like this:

```
pattern { action }
pattern { action }
...
```

* [How to Run `awk` Programs](#Running-gawk)
* [Data files for the Examples](#Sample-Data-Files)
* [Some Simple Examples](#Very-Simple)
* [An Example with Two Rules](#Two-Rules)
* [A More Complex Example](#More-Complex)
* [`awk` Statements Versus Lines](#Statements_002fLines)
* [Other Features of `awk`](#Other-Features)
* [When to Use `awk`](#When)
* [Summary](#Intro-Summary)

---

Next: [Data files for the Examples](#Sample-Data-Files), Up: [Getting Started with `awk`](#Getting-Started)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 1.1 How to Run `awk` Programs [¶](#How-to-Run-awk-Programs)

There are several ways to run an `awk` program. If the program is
short, it is easiest to include it in the command that runs `awk`,
like this:

```
awk 'program' input-file1 input-file2 ...
```

When the program is long, it is usually more convenient to put it in a file
and run it with a command like this:

```
awk -f program-file input-file1 input-file2 ...
```

This section discusses both mechanisms, along with several
variations of each.

* [One-Shot Throwaway `awk` Programs](#One_002dshot)
* [Running `awk` Without Input Files](#Read-Terminal)
* [Running Long Programs](#Long)
* [Executable `awk` Programs](#Executable-Scripts)
* [Comments in `awk` Programs](#Comments)
* [Shell Quoting Issues](#Quoting)

---

Next: [Running `awk` Without Input Files](#Read-Terminal), Up: [How to Run `awk` Programs](#Running-gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.1 One-Shot Throwaway `awk` Programs [¶](#One_002dShot-Throwaway-awk-Programs)

Once you are familiar with `awk`, you will often type in simple
programs the moment you want to use them. Then you can write the
program as the first argument of the `awk` command, like this:

```
awk 'program' input-file1 input-file2 ...
```

where program consists of a series of patterns and
actions, as described earlier.

This command format instructs the *shell*, or command interpreter,
to start `awk` and use the program to process records in the
input file(s). There are single quotes around program so
the shell won’t interpret any `awk` characters as special shell
characters. The quotes also cause the shell to treat all of program as
a single argument for `awk`, and allow program to be more
than one line long.

This format is also useful for running short or medium-sized `awk`
programs from shell scripts, because it avoids the need for a separate
file for the `awk` program. A self-contained shell script is more
reliable because there are no other files to misplace.

Later in this chapter, in
[Some Simple Examples](#Very-Simple),
we’ll see examples of several short,
self-contained programs.

---

Next: [Running Long Programs](#Long), Previous: [One-Shot Throwaway `awk` Programs](#One_002dshot), Up: [How to Run `awk` Programs](#Running-gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.2 Running `awk` Without Input Files [¶](#Running-awk-Without-Input-Files)

You can also run `awk` without any input files. If you type the
following command line:

```
awk 'program'
```

`awk` applies the program to the *standard input*,
which usually means whatever you type on the keyboard. This continues
until you indicate end-of-file by typing `Ctrl-d`.
(On non-POSIX operating systems, the end-of-file character may be different.)

As an example, the following program prints a friendly piece of advice
(from Douglas Adams’s The Hitchhiker’s Guide to the Galaxy),
to keep you from worrying about the complexities of computer
programming:

```
$ awk 'BEGIN { print "Don\47t Panic!" }'
-| Don't Panic!
```

`awk` executes statements associated with `BEGIN` before
reading any input. If there are no other statements in your program,
as is the case here, `awk` just stops, instead of trying to read
input it doesn’t know how to process.
The ‘`\47`’ is a magic way (explained later) of getting a single quote into
the program, without having to engage in ugly shell quoting tricks.

> **NOTE:** If you use Bash as your shell, you should execute the
> command ‘`set +H`’ before running this program interactively, to
> disable the C shell-style command history, which treats ‘`!`’ as a
> special character. We recommend putting this command into your personal
> startup file.

This next simple `awk` program
emulates the `cat` utility; it copies whatever you type on the
keyboard to its standard output (why this works is explained shortly):

```
$ awk '{ print }'
Now is the time for all good men
-| Now is the time for all good men
to come to the aid of their country.
-| to come to the aid of their country.
Four score and seven years ago, ...
-| Four score and seven years ago, ...
What, me worry?
-| What, me worry?
Ctrl-d
```

---

Next: [Executable `awk` Programs](#Executable-Scripts), Previous: [Running `awk` Without Input Files](#Read-Terminal), Up: [How to Run `awk` Programs](#Running-gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.3 Running Long Programs [¶](#Running-Long-Programs)

Sometimes `awk` programs are very long. In these cases, it is
more convenient to put the program into a separate file. In order to tell
`awk` to use that file for its program, you type:

```
awk -f source-file input-file1 input-file2 ...
```

The `-f` instructs the `awk` utility to get the
`awk` program from the file source-file (see [Command-Line Options](#Options)).
Any file name can be used for source-file. For example, you
could put the program:

```
BEGIN { print "Don't Panic!" }
```

into the file `advice`. Then this command:

```
awk -f advice
```

does the same thing as this one:

```
awk 'BEGIN { print "Don\47t Panic!" }'
```

This was explained earlier
(see [Running `awk` Without Input Files](#Read-Terminal)).
Note that you don’t usually need single quotes around the file name that you
specify with `-f`, because most file names don’t contain any of the shell’s
special characters. (If your file names have spaces in them, then you
will need the single quotes.) Notice that in `advice`, the `awk`
program did not have single quotes around it. The quotes are only needed
for programs that are provided on the `awk` command line.
(Also, placing the program in a file allows us to use a literal single quote in the program
text, instead of the magic ‘`\47`’.)

If you want to clearly identify an `awk` program file as such,
you can add the extension `.awk` to the file name. This doesn’t
affect the execution of the `awk` program but it does make
“housekeeping” easier.

---

Next: [Comments in `awk` Programs](#Comments), Previous: [Running Long Programs](#Long), Up: [How to Run `awk` Programs](#Running-gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.4 Executable `awk` Programs [¶](#Executable-awk-Programs)

Once you have learned `awk`, you may want to write self-contained
`awk` scripts, using the ‘`#!`’ script mechanism. You can do
this on many systems.[8](#FOOT8)
For example, you could update the file `advice` to look like this:

```
#! /bin/awk -f

BEGIN { print "Don't Panic!" }
```

After making this file executable (with the `chmod` utility),
simply type ‘`advice`’
at the shell and the system arranges to run `awk` as if you had
typed ‘`awk -f advice`’:

```
$ chmod +x advice
$ ./advice
-| Don't Panic!
```

Self-contained `awk` scripts are useful when you want to write a
program that users can invoke without their having to know that the program is
written in `awk`.

| Understanding ‘`#!`’ |
| --- |
| `awk` is an *interpreted* language. This means that the `awk` utility reads your program and then processes your data according to the instructions in your program. (This is different from a *compiled* language such as C, where your program is first compiled into machine code that is executed directly by your system’s processor.) The `awk` utility is thus termed an *interpreter*. Many modern languages are interpreted.  The line beginning with ‘`#!`’ lists the full file name of an interpreter to run and a single optional initial command-line argument to pass to that interpreter. The operating system then runs the interpreter with the given argument and the full argument list of the executed program. The first argument in the list is the full file name of the `awk` program. The rest of the argument list contains either options to `awk`, or data files, or both. (Note that on many systems `awk` is found in `/usr/bin` instead of in `/bin`.)  Some systems limit the length of the interpreter name to 32 characters. Often, this can be dealt with by using a symbolic link.  You should not put more than one argument on the ‘`#!`’ line after the path to `awk`. It does not work. The operating system treats the rest of the line as a single argument and passes it to `awk`. Doing this leads to confusing behavior—most likely a usage diagnostic of some sort from `awk`.    Finally, the value of `ARGV[0]` (see [Predefined Variables](#Built_002din-Variables)) varies depending upon your operating system. Some systems put ‘`awk`’ there, some put the full pathname of `awk` (such as `/bin/awk`), and some put the name of your script (‘`advice`’). (d.c.) Don’t rely on the value of `ARGV[0]` to provide your script name. |

---

Next: [Shell Quoting Issues](#Quoting), Previous: [Executable `awk` Programs](#Executable-Scripts), Up: [How to Run `awk` Programs](#Running-gawk)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 1.1.5 Comments in `awk` Programs [¶](#Comments-in-awk-Programs)

A *comment* is some text that is included in a program for the sake
of human readers; it is not really an executable part of the program. Comments
can explain what the program does and how it works. Nearly all
programming languages have provisions for comments, as programs are
typically hard to understand without them.

In the `awk` language, a comment starts with the number sign
character (‘`#`’) and continues to the end of the line.
The ‘`#`’ does not have to be the first character on the line. The
`awk` language ignores the rest of a line following a number sign.
For example, we could have put the following into `advice`:

```
# This program prints a nice, friendly message.  It helps
