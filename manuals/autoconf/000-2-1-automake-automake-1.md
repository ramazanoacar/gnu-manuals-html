Next: [Introduction](#Introduction), Up: [(dir)](/manual)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]# Autoconf [¶](#Autoconf)

This manual (17 March 2026) is for GNU Autoconf
(version 2.73),
a package for creating scripts to configure source code packages using
templates and an M4 macro package.

Copyright © 1992–1996, 1998–2017, 2020–2026 Free Software
Foundation, Inc.

> Permission is granted to copy, distribute and/or modify this document
> under the terms of the GNU Free Documentation License,
> Version 1.3 or any later version published by the Free Software
> Foundation; with no Invariant Sections, no Front-Cover texts, and
> no Back-Cover Texts. A copy of the license is included in the section
> entitled “GNU Free Documentation License.”## Table of Contents

* [1 Introduction](#Introduction)
* [2 The GNU Build System](#The-GNU-Build-System)
  + [2.1 Automake](#Automake)
  + [2.2 Gnulib](#Gnulib)
  + [2.3 Libtool](#Libtool)
  + [2.4 Pointers](#Pointers)
* [3 Making `configure` Scripts](#Making-configure-Scripts)
  + [3.1 Writing `configure.ac`](#Writing-Autoconf-Input)
    - [3.1.1 A Shell Script Compiler](#Shell-Script-Compiler)
    - [3.1.2 The Autoconf Language](#Autoconf-Language)
    - [3.1.3 Standard `configure.ac` Layout](#Autoconf-Input-Layout)
  + [3.2 Using `autoscan` to Create `configure.ac`](#autoscan-Invocation)
  + [3.3 Using `ifnames` to List Conditionals](#ifnames-Invocation)
  + [3.4 Using `autoconf` to Create `configure`](#autoconf-Invocation)
  + [3.5 Using `autoreconf` to Update `configure` Scripts](#autoreconf-Invocation)
* [4 Initialization and Output Files](#Setup)
  + [4.1 Initializing `configure`](#Initializing-configure)
  + [4.2 Dealing with Autoconf versions](#Versioning)
  + [4.3 Notices in `configure`](#Notices)
  + [4.4 Configure Input: Source Code, Macros, and Auxiliary Files](#Input)
  + [4.5 Outputting Files](#Output)
  + [4.6 Performing Configuration Actions](#Configuration-Actions)
  + [4.7 Creating Configuration Files](#Configuration-Files)
  + [4.8 Substitutions in Makefiles](#Makefile-Substitutions)
    - [4.8.1 Preset Output Variables](#Preset-Output-Variables)
    - [4.8.2 Installation Directory Variables](#Installation-Directory-Variables)
    - [4.8.3 Changed Directory Variables](#Changed-Directory-Variables)
    - [4.8.4 Build Directories](#Build-Directories)
    - [4.8.5 Automatic Remaking](#Automatic-Remaking)
  + [4.9 Configuration Header Files](#Configuration-Headers)
    - [4.9.1 Configuration Header Templates](#Header-Templates)
    - [4.9.2 Using `autoheader` to Create `config.h.in`](#autoheader-Invocation)
    - [4.9.3 Autoheader Macros](#Autoheader-Macros)
  + [4.10 Running Arbitrary Configuration Commands](#Configuration-Commands)
  + [4.11 Creating Configuration Links](#Configuration-Links)
  + [4.12 Configuring Other Packages in Subdirectories](#Subdirectories)
  + [4.13 Default Prefix](#Default-Prefix)
* [5 Existing Tests](#Existing-Tests)
  + [5.1 Common Behavior](#Common-Behavior)
    - [5.1.1 Standard Symbols](#Standard-Symbols)
    - [5.1.2 Default Includes](#Default-Includes)
  + [5.2 Alternative Programs](#Alternative-Programs)
    - [5.2.1 Particular Program Checks](#Particular-Programs)
    - [5.2.2 Generic Program and File Checks](#Generic-Programs)
  + [5.3 Files](#Files)
  + [5.4 Library Files](#Libraries)
  + [5.5 Library Functions](#Library-Functions)
    - [5.5.1 Portability of C Functions](#Function-Portability)
    - [5.5.2 Particular Function Checks](#Particular-Functions)
    - [5.5.3 Generic Function Checks](#Generic-Functions)
  + [5.6 Header Files](#Header-Files)
    - [5.6.1 Portability of Headers](#Header-Portability)
    - [5.6.2 Particular Header Checks](#Particular-Headers)
    - [5.6.3 Generic Header Checks](#Generic-Headers)
  + [5.7 Declarations](#Declarations)
    - [5.7.1 Particular Declaration Checks](#Particular-Declarations)
    - [5.7.2 Generic Declaration Checks](#Generic-Declarations)
  + [5.8 Structures](#Structures)
    - [5.8.1 Particular Structure Checks](#Particular-Structures)
    - [5.8.2 Generic Structure Checks](#Generic-Structures)
  + [5.9 Types](#Types)
    - [5.9.1 Particular Type Checks](#Particular-Types)
    - [5.9.2 Generic Type Checks](#Generic-Types)
  + [5.10 Compilers and Preprocessors](#Compilers-and-Preprocessors)
    - [5.10.1 Specific Compiler Characteristics](#Specific-Compiler-Characteristics)
    - [5.10.2 Generic Compiler Characteristics](#Generic-Compiler-Characteristics)
    - [5.10.3 C Compiler Characteristics](#C-Compiler)
    - [5.10.4 C++ Compiler Characteristics](#C_002b_002b-Compiler)
    - [5.10.5 Objective C Compiler Characteristics](#Objective-C-Compiler)
    - [5.10.6 Objective C++ Compiler Characteristics](#Objective-C_002b_002b-Compiler)
    - [5.10.7 Erlang Compiler and Interpreter Characteristics](#Erlang-Compiler-and-Interpreter)
    - [5.10.8 Fortran Compiler Characteristics](#Fortran-Compiler)
    - [5.10.9 Go Compiler Characteristics](#Go-Compiler)
    - [5.10.10 Algol 68 Compiler](#Algol-68-Compiler)
  + [5.11 System Services](#System-Services)
  + [5.12 C and POSIX Variants](#C-and-POSIX-Variants)
  + [5.13 Erlang Libraries](#Erlang-Libraries)
* [6 Writing Tests](#Writing-Tests)
  + [6.1 Language Choice](#Language-Choice)
  + [6.2 Writing Test Programs](#Writing-Test-Programs)
    - [6.2.1 Guidelines for Test Programs](#Guidelines)
    - [6.2.2 Test Functions](#Test-Functions)
    - [6.2.3 Generating Sources](#Generating-Sources)
  + [6.3 Running the Preprocessor](#Running-the-Preprocessor)
  + [6.4 Running the Compiler](#Running-the-Compiler)
  + [6.5 Running the Linker](#Running-the-Linker)
  + [6.6 Checking Runtime Behavior](#Runtime)
  + [6.7 Multiple Cases](#Multiple-Cases)
* [7 Results of Tests](#Results)
  + [7.1 Defining C Preprocessor Symbols](#Defining-Symbols)
  + [7.2 Setting Output Variables](#Setting-Output-Variables)
  + [7.3 Special Characters in Output Variables](#Special-Chars-in-Variables)
  + [7.4 Caching Results](#Caching-Results)
    - [7.4.1 Cache Variable Names](#Cache-Variable-Names)
    - [7.4.2 Cache Files](#Cache-Files)
    - [7.4.3 Cache Checkpointing](#Cache-Checkpointing)
  + [7.5 Printing Messages](#Printing-Messages)
* [8 Programming in M4](#Programming-in-M4)
  + [8.1 M4 Quotation](#M4-Quotation)
    - [8.1.1 Active Characters](#Active-Characters)
    - [8.1.2 One Macro Call](#One-Macro-Call)
    - [8.1.3 Quoting and Parameters](#Quoting-and-Parameters)
    - [8.1.4 Quotation and Nested Macros](#Quotation-and-Nested-Macros)
    - [8.1.5 `changequote` is Evil](#Changequote-is-Evil)
    - [8.1.6 Quadrigraphs](#Quadrigraphs)
    - [8.1.7 Dealing with unbalanced parentheses](#Balancing-Parentheses)
    - [8.1.8 Quotation Rule Of Thumb](#Quotation-Rule-Of-Thumb)
  + [8.2 Using `autom4te`](#Using-autom4te)
    - [8.2.1 Invoking `autom4te`](#autom4te-Invocation)
    - [8.2.2 Customizing `autom4te`](#Customizing-autom4te)
  + [8.3 Programming in M4sugar](#Programming-in-M4sugar)
    - [8.3.1 Redefined M4 Macros](#Redefined-M4-Macros)
    - [8.3.2 Diagnostic messages from M4sugar](#Diagnostic-Macros)
    - [8.3.3 Diversion support](#Diversion-support)
    - [8.3.4 Conditional constructs](#Conditional-constructs)
    - [8.3.5 Looping constructs](#Looping-constructs)
    - [8.3.6 Evaluation Macros](#Evaluation-Macros)
    - [8.3.7 String manipulation in M4](#Text-processing-Macros)
    - [8.3.8 Arithmetic computation in M4](#Number-processing-Macros)
    - [8.3.9 Set manipulation in M4](#Set-manipulation-Macros)
    - [8.3.10 Forbidden Patterns](#Forbidden-Patterns)
  + [8.4 Debugging via autom4te](#Debugging-via-autom4te)
* [9 Programming in M4sh](#Programming-in-M4sh)
  + [9.1 Common Shell Constructs](#Common-Shell-Constructs)
  + [9.2 Support for indirect variable names](#Polymorphic-Variables)
  + [9.3 Initialization Macros](#Initialization-Macros)
  + [9.4 File Descriptor Macros](#File-Descriptor-Macros)
* [10 Writing Autoconf Macros](#Writing-Autoconf-Macros)
  + [10.1 Macro Definitions](#Macro-Definitions)
  + [10.2 Macro Names](#Macro-Names)
  + [10.3 Dependencies Between Macros](#Dependencies-Between-Macros)
    - [10.3.1 Prerequisite Macros](#Prerequisite-Macros)
    - [10.3.2 Suggested Ordering](#Suggested-Ordering)
    - [10.3.3 One-Shot Macros](#One_002dShot-Macros)
  + [10.4 Obsoleting Macros](#Obsoleting-Macros)
  + [10.5 Coding Style](#Coding-Style)
* [11 Portable Shell Programming](#Portable-Shell)
  + [11.1 Systemology](#Systemology)
  + [11.2 Shellology](#Shellology)
  + [11.3 Invoking the Shell](#Invoking-the-Shell)
  + [11.4 Here-Documents](#Here_002dDocuments)
  + [11.5 File Descriptors](#File-Descriptors)
  + [11.6 Signal Handling](#Signal-Handling)
  + [11.7 File System Conventions](#File-System-Conventions)
  + [11.8 Shell Pattern Matching](#Shell-Pattern-Matching)
  + [11.9 Shell Substitutions](#Shell-Substitutions)
  + [11.10 Assignments](#Assignments)
  + [11.11 Parentheses in Shell Scripts](#Parentheses)
  + [11.12 Special Shell Variables](#Special-Shell-Variables)
  + [11.13 Shell Functions](#Shell-Functions)
  + [11.14 Limitations of Shell Builtins](#Limitations-of-Builtins)
  + [11.15 Limitations of Usual Tools](#Limitations-of-Usual-Tools)
* [12 Portable Make Programming](#Portable-Make)
  + [12.1 `$<` in Ordinary Make Rules](#g_t_0024_003c-in-Ordinary-Make-Rules)
  + [12.2 Failure in Make Rules](#Failure-in-Make-Rules)
  + [12.3 Makefile Command Line Prefixes](#Command-Line-Prefixes)
  + [12.4 Special Characters in Make Macro Names](#Special-Chars-in-Names)
  + [12.5 Backslash-Newline Before Empty Lines](#Backslash_002dNewline_002dEmpty)
  + [12.6 Backslash-Newline in Make Comments](#Backslash_002dNewline-Comments)
  + [12.7 `make macro=value` and Submakes](#Macros-and-Submakes)
  + [12.8 The Make Macro MAKEFLAGS](#The-Make-Macro-MAKEFLAGS)
  + [12.9 The Make Macro `SHELL`](#The-Make-Macro-SHELL)
  + [12.10 Parallel Make](#Parallel-Make)
  + [12.11 Comments in Make Rules](#Comments-in-Make-Rules)
  + [12.12 Newlines in Make Rules](#Newlines-in-Make-Rules)
  + [12.13 Comments in Make Macros](#Comments-in-Make-Macros)
  + [12.14 Trailing whitespace in Make Macros](#Trailing-whitespace-in-Make-Macros)
  + [12.15 Command-line Macros and whitespace](#Command_002dline-Macros-and-whitespace)
  + [12.16 The `obj/` Subdirectory and Make](#obj_002f-and-Make)
  + [12.17 Exit Status of `make -k`](#make-_002dk-Status)
  + [12.18 `VPATH` and Make](#VPATH-and-Make)
    - [12.18.1 Variables listed in `VPATH`](#Variables-listed-in-VPATH)
    - [12.18.2 `VPATH` and Double-colon Rules](#VPATH-and-Double_002dcolon)
    - [12.18.3 `$<` Not Supported in Explicit Rules](#g_t_0024_003c-in-Explicit-Rules)
    - [12.18.4 Automatic Rule Rewriting](#Automatic-Rule-Rewriting)
    - [12.18.5 Make Target Lookup](#Make-Target-Lookup)
  + [12.19 Single Suffix Rules and Separated Dependencies](#Single-Suffix-Rules)
  + [12.20 Timestamp Resolution and Make](#Timestamps-and-Make)
* [13 Portable C and C++ Programming](#Portable-C-and-C_002b_002b)
  + [13.1 Varieties of Unportability](#Varieties-of-Unportability)
  + [13.2 Integer Overflow](#Integer-Overflow)
    - [13.2.1 Basics of Integer Overflow](#Integer-Overflow-Basics)
    - [13.2.2 Examples of Code Assuming Wraparound Overflow](#Signed-Overflow-Examples)
    - [13.2.3 Optimizations That Break Wraparound Arithmetic](#Optimization-and-Wraparound)
    - [13.2.4 Practical Advice for Signed Overflow Issues](#Signed-Overflow-Advice)
    - [13.2.5 Signed Integer Division and Integer Overflow](#Signed-Integer-Division)
  + [13.3 Preprocessor Arithmetic](#Preprocessor-Arithmetic)
  + [13.4 Properties of Null Pointers](#Null-Pointers)
  + [13.5 Buffer Overruns and Subscript Errors](#Buffer-Overruns)
  + [13.6 Volatile Objects](#Volatile-Objects)
  + [13.7 Floating Point Portability](#Floating-Point-Portability)
  + [13.8 Exiting Portably](#Exiting-Portably)
* [14 Manual Configuration](#Manual-Configuration)
  + [14.1 Specifying target triplets](#Specifying-Target-Triplets)
  + [14.2 Getting the Canonical System Type](#Canonicalizing)
  + [14.3 Using the System Type](#Using-System-Type)
* [15 Site Configuration](#Site-Configuration)
  + [15.1 Controlling Help Output](#Help-Formatting)
  + [15.2 Working With External Software](#External-Software)
  + [15.3 Choosing Package Options](#Package-Options)
  + [15.4 Making Your Help Strings Look Pretty](#Pretty-Help-Strings)
  + [15.5 Controlling Checking of `configure` Options](#Option-Checking)
  + [15.6 Configuring Site Details](#Site-Details)
  + [15.7 Transforming Program Names When Installing](#Transforming-Names)
    - [15.7.1 Transformation Options](#Transformation-Options)
    - [15.7.2 Transformation Examples](#Transformation-Examples)
    - [15.7.3 Transformation Rules](#Transformation-Rules)
  + [15.8 Setting Site Defaults](#Site-Defaults)
* [16 Running `configure` Scripts](#Running-configure-Scripts)
  + [16.1 Basic Installation](#Basic-Installation)
  + [16.2 Installation Prerequisites](#Installation-Prerequisites)
  + [16.3 Compilers and Options](#Compilers-and-Options)
  + [16.4 Compiling For Multiple Architectures](#Multiple-Architectures)
  + [16.5 Installation Names](#Installation-Names)
  + [16.6 Optional Features](#Optional-Features)
  + [16.7 Specifying a System Type](#System-Types)
  + [16.8 Sharing Defaults](#Sharing-Defaults)
  + [16.9 Defining Variables](#Defining-Variables)
  + [16.10 `configure` Invocation](#configure-Invocation)
* [17 config.status Invocation](#config_002estatus-Invocation)
* [18 Obsolete Constructs](#Obsolete-Constructs)
  + [18.1 Obsolete `config.status` Invocation](#Obsolete-config_002estatus-Use)
  + [18.2 `acconfig.h`](#acconfig-Header)
  + [18.3 Using `autoupdate` to Modernize `configure.ac`](#autoupdate-Invocation)
  + [18.4 Obsolete Macros](#Obsolete-Macros)
  + [18.5 Upgrading From Version 1](#Autoconf-1)
    - [18.5.1 Changed File Names](#Changed-File-Names)
    - [18.5.2 Changed Makefiles](#Changed-Makefiles)
    - [18.5.3 Changed Macros](#Changed-Macros)
    - [18.5.4 Changed Results](#Changed-Results)
    - [18.5.5 Changed Macro Writing](#Changed-Macro-Writing)
  + [18.6 Upgrading From Version 2.13](#Autoconf-2_002e13)
    - [18.6.1 Changed Quotation](#Changed-Quotation)
    - [18.6.2 New Macros](#New-Macros)
    - [18.6.3 Hosts and Cross-Compilation](#Hosts-and-Cross_002dCompilation)
    - [18.6.4 `AC_LIBOBJ` vs. `LIBOBJS`](#AC_005fLIBOBJ-vs-LIBOBJS)
    - [18.6.5 `AC_ACT_IFELSE` vs. `AC_TRY_ACT`](#AC_005fACT_005fIFELSE-vs-AC_005fTRY_005fACT)
* [19 Generating Test Suites with Autotest](#Using-Autotest)
  + [19.1 Using an Autotest Test Suite](#Using-an-Autotest-Test-Suite)
    - [19.1.1 `testsuite` Scripts](#testsuite-Scripts)
    - [19.1.2 Autotest Logs](#Autotest-Logs)
  + [19.2 Writing `testsuite.at`](#Writing-Testsuites)
  + [19.3 Running `testsuite` Scripts](#testsuite-Invocation)
  + [19.4 Making `testsuite` Scripts](#Making-testsuite-Scripts)
* [20 Frequent Autoconf Questions, with answers](#FAQ)
  + [20.1 Distributing `configure` Scripts](#Distributing)
  + [20.2 Why Require GNU M4?](#Why-GNU-M4)
  + [20.3 How Can I Bootstrap?](#Bootstrapping)
  + [20.4 Why Not Imake?](#Why-Not-Imake)
  + [20.5 How Do I `#define` Installation Directories?](#Defining-Directories)
  + [20.6 What is `autom4te.cache`?](#Autom4te-Cache)
  + [20.7 Header Present But Cannot Be Compiled](#Present-But-Cannot-Be-Compiled)
  + [20.8 Expanded Before Required](#Expanded-Before-Required)
  + [20.9 Debugging `configure` scripts](#Debugging)
* [21 History of Autoconf](#History)
  + [21.1 Genesis](#Genesis)
  + [21.2 Exodus](#Exodus)
  + [21.3 Leviticus](#Leviticus)
  + [21.4 Numbers](#Numbers)
  + [21.5 Deuteronomy](#Deuteronomy)
* [Appendix A GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Appendix B Indices](#Indices)
  + [B.1 Environment Variable Index](#Environment-Variable-Index)
  + [B.2 Output Variable Index](#Output-Variable-Index)
  + [B.3 Preprocessor Symbol Index](#Preprocessor-Symbol-Index)
  + [B.4 Cache Variable Index](#Cache-Variable-Index)
  + [B.5 Autoconf Macro Index](#Autoconf-Macro-Index)
  + [B.6 M4 Macro Index](#M4-Macro-Index)
  + [B.7 Autotest Macro Index](#Autotest-Macro-Index)
  + [B.8 Program and Function Index](#Program-_0026-Function-Index)
  + [B.9 Concept Index](#Concept-Index)

---

Next: [The GNU Build System](#The-GNU-Build-System), Previous: [Autoconf](#Top), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 1 Introduction [¶](#Introduction-1)

A physicist, an engineer, and a computer scientist were discussing the
nature of God. “Surely a Physicist,” said the physicist, “because
early in the Creation, God made Light; and you know, Maxwell’s
equations, the dual nature of electromagnetic waves, the relativistic
consequences...” “An Engineer!,” said the engineer, “because
before making Light, God split the Chaos into Land and Water; it takes
a hell of an engineer to handle that big amount of mud, and orderly
separation of solids from liquids...” The computer scientist
shouted: “And the Chaos, where do you think it was coming from, hmm?”

—Anonymous

Autoconf is a tool for producing shell scripts that automatically
configure software source code packages to adapt to many kinds of
POSIX-like systems. The configuration scripts produced by Autoconf
are independent of Autoconf when they are run, so their users do not
need to have Autoconf.

The configuration scripts produced by Autoconf require no manual user
intervention when run; they do not normally even need an argument
specifying the system type. Instead, they individually test for the
presence of each feature that the software package they are for might need.
(Before each check, they print a one-line message stating what they are
checking for, so the user doesn’t get too bored while waiting for the
script to finish.) As a result, they deal well with systems that are
hybrids or customized from the more common POSIX variants. There is
no need to maintain files that list the features supported by each
release of each variant of POSIX.

For each software package that Autoconf is used with, it creates a
configuration script from a template file that lists the system features
that the package needs or can use. After the shell code to recognize
and respond to a system feature has been written, Autoconf allows it to
be shared by many software packages that can use (or need) that feature.
If it later turns out that the shell code needs adjustment for some
reason, it needs to be changed in only one place; all of the
configuration scripts can be regenerated automatically to take advantage
of the updated code.

Those who do not understand Autoconf are condemned to reinvent it, poorly.
The primary goal of Autoconf is making the *user’s* life easier;
making the *maintainer’s* life easier is only a secondary goal.
Put another way, the primary goal is not to make the generation of
`configure` automatic for package maintainers (although patches
along that front are welcome, since package maintainers form the user
base of Autoconf); rather, the goal is to make `configure`
painless, portable, and predictable for the end user of each
*autoconfiscated* package. And to this degree, Autoconf is highly
successful at its goal—most complaints to the Autoconf list are
about difficulties in writing Autoconf input, and not in the behavior of
the resulting `configure`. Even packages that don’t use Autoconf
will generally provide a `configure` script, and the most common
complaint about these alternative home-grown scripts is that they fail
to meet one or more of the GNU Coding Standards (see [Configuration](https://www.gnu.org/prep/standards/standards.html#Configuration) in The GNU Coding Standards) that users
have come to expect from Autoconf-generated `configure` scripts.

The Metaconfig package is similar in purpose to Autoconf, but the
scripts it produces require manual user intervention, which is quite
inconvenient when configuring large source trees. Unlike Metaconfig
scripts, Autoconf scripts can support cross-compiling, if some care is
taken in writing them.

Autoconf does not solve all problems related to making portable
software packages—for a more complete solution, it should be used in
concert with other GNU build tools like Automake and
Libtool. These other tools take on jobs like the creation of a
portable, recursive makefile with all of the standard targets,
linking of shared libraries, and so on. See [The GNU Build System](#The-GNU-Build-System),
for more information.

Autoconf imposes some restrictions on the names of macros used with
`#if` in C programs (see [Preprocessor Symbol Index](#Preprocessor-Symbol-Index)).

Autoconf requires GNU M4 version 1.4.8 or later in order to
generate the scripts. It uses features that some versions of M4,
including GNU M4 1.3, do not have. Autoconf works better
with GNU M4 version 1.4.16 or later, though this is not
required.

See [Upgrading From Version 1](#Autoconf-1), for information about upgrading from version 1.
See [History of Autoconf](#History), for the story of Autoconf’s development. See [Frequent Autoconf Questions, with answers](#FAQ),
for answers to some common questions about Autoconf.

See the [Autoconf web page](https://www.gnu.org/software/autoconf/) for up-to-date information, details on the mailing
lists, pointers to a list of known bugs, etc.

Mail suggestions to [the Autoconf mailing
list](mailto:autoconf@gnu.org). Past suggestions are
[archived](https://lists.gnu.org/archive/html/autoconf/).

Mail bug reports to [the
Autoconf Bugs mailing list](mailto:bug-autoconf@gnu.org). Past bug reports are
[archived](https://lists.gnu.org/archive/html/bug-autoconf/).

If possible, first check that your bug is
not already solved in current development versions, and that it has not
been reported yet. Be sure to include all the needed information and a
short `configure.ac` that demonstrates the problem.

Autoconf’s development tree is accessible via `git`; see the
[Autoconf
Summary](https://savannah.gnu.org/projects/autoconf/) for details, or view
[the actual
repository](https://git.savannah.gnu.org/cgit/autoconf.git). Patches relative to the current `git` version can
be sent for review to the [Autoconf
Patches mailing list](mailto:autoconf-patches@gnu.org), with discussion on prior patches
[archived](https://lists.gnu.org/archive/html/autoconf-patches/); and all commits are posted in the read-only
[Autoconf Commit mailing list](mailto:autoconf-commit@gnu.org), which is
also [archived](https://lists.gnu.org/archive/html/autoconf-commit/).

Because of its mission, the Autoconf package itself
includes only a set of often-used
macros that have already demonstrated their usefulness. Nevertheless,
if you wish to share your macros, or find existing ones, see the
[Autoconf Macro
Archive](https://www.gnu.org/software/autoconf-archive/), which is kindly run by [Peter Simons](mailto:simons@cryp.to).

---

Next: [Making `configure` Scripts](#Making-configure-Scripts), Previous: [Introduction](#Introduction), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 2 The GNU Build System [¶](#The-GNU-Build-System-1)

Autoconf solves an important problem—reliable discovery of
system-specific build and runtime information—but this is only one
piece of the puzzle for the development of portable software. To this
end, the GNU project has developed a suite of integrated
utilities to finish the job Autoconf started: the GNU build
system, whose most important components are Autoconf, Automake, and
Libtool. In this chapter, we introduce you to those tools, point you
to sources of more information, and try to convince you to use the
entire GNU build system for your software.

* [Automake](#Automake)
* [Gnulib](#Gnulib)
* [Libtool](#Libtool)
* [Pointers](#Pointers)

---

Next: [Gnulib](#Gnulib), Up: [The GNU Build System](#The-GNU-Build-System)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.1 Automake [¶](#Automake-1)

The ubiquity of `make` means that a makefile is almost the
only viable way to distribute automatic build rules for software, but
one quickly runs into its numerous limitations. Its lack of
support for automatic dependency tracking, recursive builds in
subdirectories, reliable timestamps (e.g., for network file systems), and
so on, mean that developers must painfully (and often incorrectly)
reinvent the wheel for each project. Portability is non-trivial, thanks
to the quirks of `make` on many systems. On top of all this is the
manual labor required to implement the many standard targets that users
have come to expect (`make install`, `make distclean`,
`make uninstall`, etc.). Since you are, of course, using Autoconf,
you also have to insert repetitive code in your `Makefile.in` to
recognize `@CC@`, `@CFLAGS@`, and other substitutions
provided by `configure`. Into this mess steps *Automake*.

Automake allows you to specify your build needs in a `Makefile.am`
file with a vastly simpler and more powerful syntax than that of a plain
makefile, and then generates a portable `Makefile.in` for
use with Autoconf. For example, the `Makefile.am` to build and
install a simple “Hello world” program might look like:

```
bin_PROGRAMS = hello
hello_SOURCES = hello.c
```

The resulting `Makefile.in` (~400 lines) automatically supports all
the standard targets, the substitutions provided by Autoconf, automatic
dependency tracking, `VPATH` building, and so on. `make`
builds the `hello` program, and `make install` installs it
in `/usr/local/bin` (or whatever prefix was given to
`configure`, if not `/usr/local`).

The benefits of Automake increase for larger packages (especially ones
with subdirectories), but even for small programs the added convenience
and portability can be substantial. And that’s not all...

---

Next: [Libtool](#Libtool), Previous: [Automake](#Automake), Up: [The GNU Build System](#The-GNU-Build-System)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.2 Gnulib [¶](#Gnulib-1)

GNU software has a well-deserved reputation for running on
many different types of systems. While our primary goal is to write
software for the GNU system, many users and developers have
been introduced to us through the systems that they were already using.

Gnulib is a central location for common GNU code, intended to
be shared among free software packages. Its components are typically
shared at the source level, rather than being a library that gets built,
installed, and linked against. The idea is to copy files from Gnulib
into your own source tree. There is no distribution tarball; developers
should just grab source modules from the repository. The source files
are available online, under various licenses, mostly GNU
GPL or GNU LGPL.

Gnulib modules typically contain C source code along with Autoconf
macros used to configure the source code. For example, the Gnulib
`stdckdint` module implements a `stdckdint.h` header that nearly
conforms to C23, even on older hosts that lack `stdckdint.h`.
This module contains a source file for the replacement header, along
with an Autoconf macro that arranges to use the replacement header on
older systems.

For more information, consult the Gnulib website,
<https://www.gnu.org/software/gnulib/>.

---

Next: [Pointers](#Pointers), Previous: [Gnulib](#Gnulib), Up: [The GNU Build System](#The-GNU-Build-System)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.3 Libtool [¶](#Libtool-1)

Often, one wants to build not only programs, but libraries, so that
other programs can benefit from the fruits of your labor. Ideally, one
would like to produce *shared* (dynamically linked) libraries,
which can be used by multiple programs without duplication on disk or in
memory and can be updated independently of the linked programs.
Producing shared libraries portably, however, is the stuff of
nightmares—each system has its own incompatible tools, compiler flags,
and magic incantations. Fortunately, GNU provides a solution:
*Libtool*.

Libtool handles all the requirements of building shared libraries for
you, and at this time seems to be the *only* way to do so with any
portability. It also handles many other headaches, such as: the
interaction of Make rules with the variable suffixes of
shared libraries, linking reliably with shared libraries before they are
installed by the superuser, and supplying a consistent versioning system
(so that different versions of a library can be installed or upgraded
without breaking binary compatibility). Although Libtool, like
Autoconf, can be used without Automake, it is most simply utilized in
conjunction with Automake—there, Libtool is used automatically
whenever shared libraries are needed, and you need not know its syntax.

---

Previous: [Libtool](#Libtool), Up: [The GNU Build System](#The-GNU-Build-System)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.4 Pointers [¶](#Pointers-1)

Developers who are used to the simplicity of `make` for small
projects on a single system might be daunted at the prospect of
learning to use Automake and Autoconf. As your software is
distributed to more and more users, however, you otherwise
quickly find yourself putting lots of effort into reinventing the
services that the GNU build tools provide, and making the
same mistakes that they once made and overcame. (Besides, since
you’re already learning Autoconf, Automake is a piece of cake.)

There are a number of places that you can go to for more information on
the GNU build tools.

* Web

  The project home pages for
  [Autoconf](https://www.gnu.org/software/autoconf/),
  [Automake](https://www.gnu.org/software/automake/),
  [Gnulib](https://www.gnu.org/software/gnulib/), and
  [Libtool](https://www.gnu.org/software/libtool/).
* Automake Manual

  See [Automake](https://www.gnu.org/software/automake/manual/automake.html#Top) in GNU Automake, for more
  information on Automake.
* Books

  The book GNU Autoconf, Automake and
  Libtool[1](#FOOT1) describes the complete GNU
  build environment. You can also find
  [the entire book on-line](https://www.sourceware.org/autobook/).

---

Next: [Initialization and Output Files](#Setup), Previous: [The GNU Build System](#The-GNU-Build-System), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 3 Making `configure` Scripts [¶](#Making-configure-Scripts-1)

The configuration scripts that Autoconf produces are by convention
called `configure`. When run, `configure` creates several
files, replacing configuration parameters in them with appropriate
values. The files that `configure` creates are:

* one or more `Makefile` files, usually one in each subdirectory of the
  package (see [Substitutions in Makefiles](#Makefile-Substitutions));
* optionally, a C header file, the name of which is configurable,
  containing `#define` directives (see [Configuration Header Files](#Configuration-Headers));
* a shell script called `config.status` that, when run, recreates
  the files listed above (see [config.status Invocation](#config_002estatus-Invocation));
* an optional shell script normally called `config.cache`
  (created when using ‘`configure --config-cache`’) that
  saves the results of running many of the tests (see [Cache Files](#Cache-Files));
* a file called `config.log` containing any messages produced by
  compilers, to help debugging if `configure` makes a mistake.

To create a `configure` script with Autoconf, you need
to write an Autoconf input file `configure.ac` and run
`autoconf` on it. If you write your own feature tests to
supplement those that come with Autoconf, you might also write files
called `aclocal.m4` and `acsite.m4`. If you use a C header
file to contain `#define` directives, you might also run
`autoheader`, and you can distribute the generated file
`config.h.in` with the package.

Here is a diagram showing how the files that can be used in
configuration are produced. Programs that are executed are suffixed by
‘`*`’. Optional files are enclosed in square brackets (‘`[]`’).
`autoconf` and `autoheader` also read the installed Autoconf
macro files (by reading `autoconf.m4`).

Files used in preparing a software package for distribution, when using
just Autoconf:

```
your source files --> [autoscan*] --> [configure.scan] --> configure.ac
```

```
configure.ac --.
               |   .------> autoconf* -----> configure
[aclocal.m4] --+---+
               |   `-----> [autoheader*] --> [config.h.in]
[acsite.m4] ---'
```

```
Makefile.in
```

Additionally, if you use Automake, the following additional productions
come into play:

```
[acinclude.m4] --.
                 |
[local macros] --+--> aclocal* --> aclocal.m4
                 |
configure.ac ----'
```

```

```

```
configure.ac --.
               +--> automake* --> Makefile.in
Makefile.am ---'
```

Files used in configuring a software package:

```
                       .-------------> [config.cache]
configure* ------------+-------------> config.log
                       |
[config.h.in] -.       v            .-> [config.h] -.
               +--> config.status* -+               +--> make*
Makefile.in ---'                    `-> Makefile ---'
```

* [Writing `configure.ac`](#Writing-Autoconf-Input)
* [Using `autoscan` to Create `configure.ac`](#autoscan-Invocation)
* [Using `ifnames` to List Conditionals](#ifnames-Invocation)
* [Using `autoconf` to Create `configure`](#autoconf-Invocation)
* [Using `autoreconf` to Update `configure` Scripts](#autoreconf-Invocation)

---

Next: [Using `autoscan` to Create `configure.ac`](#autoscan-Invocation), Up: [Making `configure` Scripts](#Making-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.1 Writing `configure.ac` [¶](#Writing-configure_002eac)

To produce a `configure` script for a software package, create a
file called `configure.ac` that contains invocations of the
Autoconf macros that test the system features your package needs or can
use. Autoconf macros already exist to check for many features; see
[Existing Tests](#Existing-Tests), for their descriptions. For most other features,
you can use Autoconf template macros to produce custom checks; see
[Writing Tests](#Writing-Tests), for information about them. For especially tricky
or specialized features, `configure.ac` might need to contain some
hand-crafted shell commands; see [Portable Shell
Programming](#Portable-Shell). The `autoscan` program can give you a good start
in writing `configure.ac` (see [Using `autoscan` to Create `configure.ac`](#autoscan-Invocation), for more
information).

Previous versions of Autoconf promoted the name `configure.in`,
which is somewhat ambiguous (the tool needed to process this file is not
described by its extension), and introduces a slight confusion with
`config.h.in` and so on (for which ‘`.in`’ means “to be
processed by `configure`”). Using `configure.ac` is now
preferred, while the use of `configure.in` will cause warnings
from `autoconf`.

* [A Shell Script Compiler](#Shell-Script-Compiler)
* [The Autoconf Language](#Autoconf-Language)
* [Standard `configure.ac` Layout](#Autoconf-Input-Layout)

---

Next: [The Autoconf Language](#Autoconf-Language), Up: [Writing `configure.ac`](#Writing-Autoconf-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 3.1.1 A Shell Script Compiler [¶](#A-Shell-Script-Compiler)

Just as for any other computer language, in order to properly program
`configure.ac` in Autoconf you must understand *what* problem
the language tries to address and *how* it does so.

The problem Autoconf addresses is that the world is a mess. After all,
you are using Autoconf in order to have your package compile easily on
all sorts of different systems, some of them being extremely hostile.
Autoconf itself bears the price for these differences: `configure`
must run on all those systems, and thus `configure` must limit itself
to their lowest common denominator of features.

Naturally, you might then think of shell scripts; who needs
`autoconf`? A set of properly written shell functions is enough to
make it easy to write `configure` scripts by hand. Sigh!
Unfortunately, even in 2008, where shells without any function support are
far and few between, there are pitfalls to avoid when making use of them.
Also, finding a Bourne shell that accepts shell functions is not trivial,
even though there is almost always one on interesting porting targets.

So, what is really needed is some kind of compiler, `autoconf`,
that takes an Autoconf program, `configure.ac`, and transforms it
into a portable shell script, `configure`.

How does `autoconf` perform this task?

There are two obvious possibilities: creating a brand new language or
extending an existing one. The former option is attractive: all
sorts of optimizations could easily be implemented in the compiler and
many rigorous checks could be performed on the Autoconf program
(e.g., rejecting any non-portable construct). Alternatively, you can
extend an existing language, such as the `sh` (Bourne shell)
language.

Autoconf does the latter: it is a layer on top of `sh`. It was
therefore most convenient to implement `autoconf` as a macro
expander: a program that repeatedly performs *macro expansions* on
text input, replacing macro calls with macro bodies and producing a pure
`sh` script in the end. Instead of implementing a dedicated
Autoconf macro expander, it is natural to use an existing
general-purpose macro language, such as M4, and implement the extensions
as a set of M4 macros.

---

Next: [Standard `configure.ac` Layout](#Autoconf-Input-Layout), Previous: [A Shell Script Compiler](#Shell-Script-Compiler), Up: [Writing `configure.ac`](#Writing-Autoconf-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 3.1.2 The Autoconf Language [¶](#The-Autoconf-Language)

The Autoconf language differs from many other computer
languages because it treats actual code the same as plain text. Whereas
in C, for instance, data and instructions have different syntactic
status, in Autoconf their status is rigorously the same. Therefore, we
need a means to distinguish literal strings from text to be expanded:
quotation.

When calling macros that take arguments, there must not be any white
space between the macro name and the open parenthesis.

```
AC_INIT ([oops], [1.0]) # incorrect
AC_INIT([hello], [1.0]) # good
```

Arguments should
be enclosed within the quote characters ‘`[`’ and ‘`]`’, and be
separated by commas. Any leading blanks or newlines in arguments are ignored,
unless they are quoted. You should always quote an argument that
might contain a macro name, comma, parenthesis, or a leading blank or
newline. This rule applies recursively for every macro
call, including macros called from other macros. For more details on
quoting rules, see [Programming in M4](#Programming-in-M4).

For instance:

```
AC_CHECK_HEADER([stdio.h],
                [AC_DEFINE([HAVE_STDIO_H], [1],
                   [Define to 1 if you have <stdio.h>.])],
                [AC_MSG_ERROR([sorry, can't do anything for you])])
```

is quoted properly. You may safely simplify its quotation to:

```
AC_CHECK_HEADER([stdio.h],
                [AC_DEFINE([HAVE_STDIO_H], 1,
                   [Define to 1 if you have <stdio.h>.])],
                [AC_MSG_ERROR([sorry, can't do anything for you])])
```

because ‘`1`’ cannot contain a macro call. Here, the argument of
`AC_MSG_ERROR` must be quoted; otherwise, its comma would be
interpreted as an argument separator. Also, the second and third arguments
of ‘`AC_CHECK_HEADER`’ must be quoted, since they contain
macro calls. The three arguments ‘`HAVE_STDIO_H`’, ‘`stdio.h`’,
and ‘`Define to 1 if you have <stdio.h>.`’ do not need quoting, but
if you unwisely defined a macro with a name like ‘`Define`’ or
‘`stdio`’ then they would need quoting. Cautious Autoconf users
would keep the quotes, but many Autoconf users find such precautions
annoying, and would rewrite the example as follows:

```
AC_CHECK_HEADER(stdio.h,
                [AC_DEFINE(HAVE_STDIO_H, 1,
                   [Define to 1 if you have <stdio.h>.])],
                [AC_MSG_ERROR([sorry, can't do anything for you])])
```

This is safe, so long as you adopt good naming conventions and do not
define macros with names like ‘`HAVE_STDIO_H`’, ‘`stdio`’, or
‘`h`’. Though it is also safe here to omit the quotes around
‘`Define to 1 if you have <stdio.h>.`’ this is not recommended, as
message strings are more likely to inadvertently contain commas.

The following example is wrong and dangerous, as it is underquoted:

```
AC_CHECK_HEADER(stdio.h,
                AC_DEFINE(HAVE_STDIO_H, 1,
                   Define to 1 if you have <stdio.h>.),
                AC_MSG_ERROR([sorry, can't do anything for you]))
```

In other cases, you may want to use text that also resembles a macro
call. You must quote that text (whether just the potential problem, or
the entire line) even when it is not passed as a macro argument; and you
may also have to use `m4_pattern_allow` (see [Forbidden Patterns](#Forbidden-Patterns)), to declare your intention that the resulting configure file
will have a literal that resembles what would otherwise be reserved for
a macro name. For example:

```
dnl Simulate a possible future autoconf macro
m4_define([AC_DC], [oops])
dnl Underquoted:
echo "Hard rock was here!  --AC_DC"
dnl Correctly quoted:
m4_pattern_allow([AC_DC])
echo "Hard rock was here!  --[AC_DC]"
[echo "Hard rock was here!  --AC_DC"]
```

which results in this text in `configure`:

```
echo "Hard rock was here!  --oops"
echo "Hard rock was here!  --AC_DC"
echo "Hard rock was here!  --AC_DC"
```

When you use the same text in a macro argument, you must therefore have
an extra quotation level (since one is stripped away by the macro
substitution). In general, then, it is a good idea to *use double
quoting for all literal string arguments*, either around just the
problematic portions, or over the entire argument:

```
m4_pattern_allow([AC_DC])
AC_MSG_WARN([[AC_DC] stinks  --Iron Maiden])
AC_MSG_WARN([[AC_DC stinks  --Iron Maiden]])
```

It is also possible to avoid the problematic patterns in the first
place, by the use of additional escaping (either a quadrigraph, or
creative shell constructs), in which case it is no longer necessary to
use `m4_pattern_allow`:

```
echo "Hard rock was here!  --AC""_DC"
AC_MSG_WARN([[AC@&t@_DC stinks  --Iron Maiden]])
```

You are now able to understand one of the constructs of Autoconf that
has been continually misunderstood... The rule of thumb is that
*whenever you expect macro expansion, expect quote expansion*;
i.e., expect one level of quotes to be lost. For instance:

```
AC_COMPILE_IFELSE(AC_LANG_SOURCE([char b[10];]), [],
 [AC_MSG_ERROR([you lose])])
```

is incorrect: here, the first argument of `AC_LANG_SOURCE` is
‘`char b[10];`’ and is expanded once, which results in
‘`char b10;`’; and the `AC_LANG_SOURCE` is also expanded prior
to being passed to `AC_COMPILE_IFELSE`. (There was an idiom common
in Autoconf’s past to
address this issue via the M4 `changequote` primitive, but do not
use it!) Let’s take a closer look: the author meant the first argument
to be understood as a literal, and therefore it must be quoted twice;
likewise, the intermediate `AC_LANG_SOURCE` macro should be quoted
once so that it is only expanded after the rest of the body of
`AC_COMPILE_IFELSE` is in place:

```
AC_COMPILE_IFELSE([AC_LANG_SOURCE([[char b[10];]])], [],
  [AC_MSG_ERROR([you lose])])
```

Voilà, you actually produce ‘`char b[10];`’ this time!

On the other hand, descriptions (e.g., the last parameter of
`AC_DEFINE` or `AS_HELP_STRING`) are not literals—they
are subject to line breaking, for example—and should not be double quoted.
Even if these descriptions are short and are not actually broken, double
quoting them yields weird results.

Some macros take optional arguments, which this documentation represents
as [arg] (not to be confused with the quote characters). You may
just leave them empty, or use ‘`[]`’ to make the emptiness of the
argument explicit, or you may simply omit the trailing commas. The
three lines below are equivalent:

```
AC_CHECK_HEADERS([stdio.h], [], [], [])
AC_CHECK_HEADERS([stdio.h],,,)
AC_CHECK_HEADERS([stdio.h])
```

It is best to put each macro call on its own line in
`configure.ac`. Most of the macros don’t add extra newlines; they
rely on the newline after the macro call to terminate the commands.
This approach makes the generated `configure` script a little
easier to read by not inserting lots of blank lines. It is generally
safe to set shell variables on the same line as a macro call, because
the shell allows assignments without intervening newlines.

You can include comments in `configure.ac` files by starting them
with the ‘`#`’. For example, it is helpful to begin
`configure.ac` files with a line like this:

```
# Process this file with autoconf to produce a configure script.
```

---

Previous: [The Autoconf Language](#Autoconf-Language), Up: [Writing `configure.ac`](#Writing-Autoconf-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 3.1.3 Standard `configure.ac` Layout [¶](#Standard-configure_002eac-Layout)

The order in which `configure.ac` calls the Autoconf macros is not
important, with a few exceptions. Every `configure.ac` must
contain a call to `AC_INIT` before the checks, and a call to
`AC_OUTPUT` at the end (see [Outputting Files](#Output)). Additionally, some macros
rely on other macros having been called first, because they check
previously set values of some variables to decide what to do. These
macros are noted in the individual descriptions (see [Existing Tests](#Existing-Tests)), and they also warn you when `configure` is created if they
are called out of order.

To encourage consistency, here is a suggested order for calling the
Autoconf macros. Generally speaking, the things near the end of this
list are those that could depend on things earlier in it. For example,
library functions could be affected by types and libraries.

```
Autoconf requirements
AC_INIT(package, version, bug-report-address)
information on the package
checks for programs
checks for libraries
checks for header files
checks for types
checks for structures
checks for compiler characteristics
checks for library functions
checks for system services
AC_CONFIG_FILES([file...])
AC_OUTPUT
```

---

Next: [Using `ifnames` to List Conditionals](#ifnames-Invocation), Previous: [Writing `configure.ac`](#Writing-Autoconf-Input), Up: [Making `configure` Scripts](#Making-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.2 Using `autoscan` to Create `configure.ac` [¶](#Using-autoscan-to-Create-configure_002eac)

The `autoscan` program can help you create and/or maintain a
`configure.ac` file for a software package. `autoscan`
examines source files in the directory tree rooted at a directory given
as a command line argument, or the current directory if none is given.
It searches the source files for common portability problems and creates
a file `configure.scan` which is a preliminary `configure.ac`
for that package, and checks a possibly existing `configure.ac` for
completeness.

When using `autoscan` to create a `configure.ac`, you
should manually examine `configure.scan` before renaming it to
`configure.ac`; it probably needs some adjustments.
Occasionally, `autoscan` outputs a macro in the wrong order
relative to another macro, so that `autoconf` produces a warning;
you need to move such macros manually. Also, if you want the package to
use a configuration header file, you must add a call to
`AC_CONFIG_HEADERS` (see [Configuration Header Files](#Configuration-Headers)). You might
also have to change or add some `#if` directives to your program in
order to make it work with Autoconf (see [Using `ifnames` to List Conditionals](#ifnames-Invocation), for
information about a program that can help with that job).

When using `autoscan` to maintain a `configure.ac`, simply
consider adding its suggestions. The file `autoscan.log`
contains detailed information on why a macro is requested.

`autoscan` uses several data files (installed along with Autoconf)
to determine which macros to output when it finds particular symbols in
a package’s source files. These data files all have the same format:
each line consists of a symbol, one or more blanks, and the Autoconf macro to
output if that symbol is encountered. Lines starting with ‘`#`’ are
comments.

`autoscan` accepts the following options:

`--help`

`-h`
:   Print a summary of the command line options and exit.

`--version`

`-V`
:   Print the version number of Autoconf and exit.

`--verbose`

`-v`
:   Print the names of the files it examines and the potentially interesting
    symbols it finds in them. This output can be voluminous.

`--debug`

`-d`
:   Don’t remove temporary files.

`--include=dir`

`-I dir`
:   Append dir to the include path. Multiple invocations accumulate.

`--prepend-include=dir`

`-B dir`
:   Prepend dir to the include path. Multiple invocations accumulate.

---

Next: [Using `autoconf` to Create `configure`](#autoconf-Invocation), Previous: [Using `autoscan` to Create `configure.ac`](#autoscan-Invocation), Up: [Making `configure` Scripts](#Making-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.3 Using `ifnames` to List Conditionals [¶](#Using-ifnames-to-List-Conditionals)

`ifnames` can help you write `configure.ac` for a software
package. It prints the identifiers that the package already uses in C
preprocessor conditionals. If a package has already been set up to have
some portability, `ifnames` can thus help you figure out what its
`configure` needs to check for. It may help fill in some gaps in a
`configure.ac` generated by `autoscan` (see [Using `autoscan` to Create `configure.ac`](#autoscan-Invocation)).

`ifnames` scans all of the C source files named on the command line
(or the standard input, if none are given) and writes to the standard
output a sorted list of all the identifiers that appear in those files
in `#if`, `#ifdef`, `#ifndef`, `#elif`,
`#elifdef`, or `#elifndef` directives.
It prints each identifier on a line, followed by a
space-separated list of the files in which that identifier occurs.

`ifnames` accepts the following options:

`--help`

`-h`
:   Print a summary of the command line options and exit.

`--version`

`-V`
:   Print the version number of Autoconf and exit.

---

Next: [Using `autoreconf` to Update `configure` Scripts](#autoreconf-Invocation), Previous: [Using `ifnames` to List Conditionals](#ifnames-Invocation), Up: [Making `configure` Scripts](#Making-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.4 Using `autoconf` to Create `configure` [¶](#Using-autoconf-to-Create-configure)

To create `configure` from `configure.ac`, run the
`autoconf` program with no arguments. `autoconf` processes
`configure.ac` with the M4 macro processor, using the
Autoconf macros. If you give `autoconf` an argument, it reads that
file instead of `configure.ac` and writes the configuration script
to the standard output instead of to `configure`. If you give
`autoconf` the argument `-`, it reads from the standard
input instead of `configure.ac` and writes the configuration script
to the standard output.

The Autoconf macros are defined in several files. Some of the files are
distributed with Autoconf; `autoconf` reads them first. Then it
looks for the optional file `acsite.m4` in the directory that
contains the distributed Autoconf macro files, and for the optional file
`aclocal.m4` in the current directory. Those files can contain
your site’s or the package’s own Autoconf macro definitions
(see [Writing Autoconf Macros](#Writing-Autoconf-Macros), for more information). If a macro is
defined in more than one of the files that `autoconf` reads, the
last definition it reads overrides the earlier ones.

`autoconf` accepts the following options:

`--help`

`-h`
:   Print a summary of the command line options and exit.

`--version`

`-V`
:   Print the version number of Autoconf and exit.

`--verbose`

`-v`
:   Report processing steps.

`--debug`

`-d`
:   Don’t remove the temporary files.

`--force`

`-f`
:   Remake `configure` even if newer than its input files.

`--include=dir`

`-I dir`
:   Append dir to the include path. Multiple invocations accumulate.

`--prepend-include=dir`

`-B dir`
:   Prepend dir to the include path. Multiple invocations accumulate.

`--output=file`

`-o file`
:   Save output (script or trace) to file. The file `-` stands
    for the standard output.

`--warnings=category[,category...]` [¶](#index-WARNINGS)

`-Wcategory[,category...]`
:   Enable or disable warnings related to each category.
    See [m4\_warn](#m4_005fwarn), for a comprehensive list of categories.
    Special values include:

    ‘`all`’
    :   Enable all categories of warnings.

    ‘`none`’
    :   Disable all categories of warnings.

    ‘`error`’
    :   Treat all warnings as errors.

    ‘`no-category`’
    :   Disable warnings falling into category.

    The environment variable `WARNINGS` may also be set to a
    comma-separated list of warning categories to enable or disable.
    It is interpreted exactly the same way as the argument of
    `--warnings`, but unknown categories are silently ignored.
    The command line takes precedence; for instance, if `WARNINGS`
    is set to `obsolete`, but `-Wnone` is given on the
    command line, no warnings will be issued.

    Some categories of warnings are on by default.
    Again, for details see [m4\_warn](#m4_005fwarn).

`--trace=macro[:format]`

`-t macro[:format]`
:   Do not create the `configure` script, but list the calls to
    macro according to the format. Multiple `--trace`
    arguments can be used to list several macros. Multiple `--trace`
    arguments for a single macro are not cumulative; instead, you should
    just make format as long as needed.

    The format is a regular string, with newlines if desired, and
    several special escape codes. It defaults to ‘`$f:$l:$n:$%`’; see
    [Invoking `autom4te`](#autom4te-Invocation), for details on the format.

`--initialization`

`-i`
:   By default, `--trace` does not trace the initialization of the
    Autoconf macros (typically the `AC_DEFUN` definitions). This
    results in a noticeable speedup, but can be disabled by this option.

It is often necessary to check the content of a `configure.ac`
file, but parsing it yourself is extremely fragile and error-prone. It
is suggested that you rely upon `--trace` to scan
`configure.ac`. For instance, to find the list of variables that
are substituted, use:

```
$ autoconf -t AC_SUBST
configure.ac:28:AC_SUBST:SHELL
configure.ac:28:AC_SUBST:PATH_SEPARATOR
More traces deleted
```

The example below highlights the difference between ‘`$@`’,
‘`$*`’, and ‘`$%`’.

```
$ cat configure.ac
AC_DEFINE(This, is, [an
[example]])
$ autoconf -t 'AC_DEFINE:@: $@
*: $*
%: $%'
@: [This],[is],[an
[example]]
*: This,is,an
[example]
%: This:is:an [example]
```

The format gives you a lot of freedom:

```
$ autoconf -t 'AC_SUBST:$$ac_subst{"$1"} = "$f:$l";'
$ac_subst{"SHELL"} = "configure.ac:28";
$ac_subst{"PATH_SEPARATOR"} = "configure.ac:28";
More traces deleted
```

A long separator can be used to improve the readability of complex
structures, and to ease their parsing (for instance when no single
character is suitable as a separator):

```
$ autoconf -t 'AM_MISSING_PROG:${|:::::|}*'
ACLOCAL|:::::|aclocal|:::::|$missing_dir
AUTOCONF|:::::|autoconf|:::::|$missing_dir
AUTOMAKE|:::::|automake|:::::|$missing_dir
More traces deleted
```

---

Previous: [Using `autoconf` to Create `configure`](#autoconf-Invocation), Up: [Making `configure` Scripts](#Making-configure-Scripts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.5 Using `autoreconf` to Update `configure` Scripts [¶](#Using-autoreconf-to-Update-configure-Scripts)

Installing the various components of the GNU Build System can be
tedious: running `autopoint` for Gettext, `automake` for
`Makefile.in` etc. in each directory. It may be needed either
because some tools such as `automake` have been updated on your
system, or because some of the sources such as `configure.ac` have
been updated, or finally, simply in order to install the GNU Build
System in a fresh tree.

`autoreconf` runs `autoconf`, `autoheader`,
`aclocal`, `automake`, `libtoolize`, `intltoolize`,
`gtkdocize`, and `autopoint` (when appropriate) repeatedly
to update the GNU Build System in the specified directories and their
subdirectories (see [Configuring Other Packages in Subdirectories](#Subdirectories)). By default, it only remakes
those files that are older than their sources.

If you install a new version of some tool, you can make
`autoreconf` remake *all* of the files by giving it the
`--force` option.

The environment variables `AUTOM4TE`, `AUTOCONF`,
`AUTOHEADER`, `AUTOMAKE`, `ACLOCAL`, `AUTOPOINT`,
`LIBTOOLIZE`, `INTLTOOLIZE`, `GTKDOCIZE`, `M4`, and
`MAKE` may be used to override the invocation of the respective
tools.

Except for `AUTOM4TE` and `M4`, setting any of these enviroment
variables to ‘`true`’, ‘`false`’, ‘`:`’, or the empty string
has the same effect as passing ‘`--exclude=cmd`’ on the command
line, where cmd is the lowercased version of the command’s name;
see the description of the `--exclude` option, below.

See [Automatic Remaking](#Automatic-Remaking), for Make rules to automatically
rebuild `configure` scripts when their source files change. That
method handles the timestamps of configuration header templates
properly, but does not pass `--autoconf-dir=dir` or
`--localdir=dir`.

Gettext supplies the `autopoint` command to add translation
infrastructure to a source package. If you use `autopoint`,
your `configure.ac` should invoke `AM_GNU_GETTEXT` and
one of `AM_GNU_GETTEXT_VERSION(gettext-version)` or
`AM_GNU_GETTEXT_REQUIRE_VERSION(min-gettext-version)`.
See [Invoking the `autopoint` Program](https://www.gnu.org/software/gettext/manual/gettext.html#autopoint-Invocation) in GNU `gettext` utilities, for further details.

`autoreconf` accepts the following options:

`--help`

`-h`
:   Print a summary of the command line options and exit.

`--version`

`-V`
:   Print the version number of Autoconf and exit.

`--verbose`

`-v`
:   Print the name of each directory `autoreconf` examines and the
    commands it runs. If given two or more times, pass `--verbose`
    to subordinate tools that support it.

`--debug`

`-d`
:   Don’t remove the temporary files.

`--force`

`-f`
:   Consider all generated and standard auxiliary files to be obsolete.
    This remakes even `configure` scripts and configuration headers
    that are newer than their input files (`configure.ac` and, if
    present, `aclocal.m4`).

    If deemed appropriate, this option triggers calls to ‘`automake
    --force-missing`’. Passing both `--force` and `--install`
    to `autoreconf` will in turn undo any customizations to standard
    files. Note that the macro `AM_INIT_AUTOMAKE` has some options
    which change the set of files considered to be standard.

`--replace-handwritten`

`-R`
:   Replace files that *could* be generated but appear to have been
    written by hand. Currently this only affects the treatment of the
    primary configuration header template (usually `config.h.in`;
    see [Configuration Header Templates](#Header-Templates)). In the future it may also affect
    `aclocal.m4` and others.

`--install`

`-i`
:   Install any missing standard auxiliary files in the package. By
    default, files are copied; this can be changed with `--symlink`.

    If deemed appropriate, this option triggers calls to
    ‘`automake --add-missing`’,
    ‘`libtoolize`’, ‘`autopoint`’, etc.

`--no-recursive`
:   Do not rebuild files in subdirectories to configure (see [Configuring Other Packages in Subdirectories](#Subdirectories),
    macro `AC_CONFIG_SUBDIRS`).

`--symlink`

`-s`
:   When used with `--install`, install symbolic links to the missing
    auxiliary files instead of copying them.

`--make`

`-m`
:   When the directories were configured, update the configuration by
    running ‘`./config.status --recheck && ./config.status`’, and then
    run ‘`make`’.

`--exclude=cmd[,cmd...]`

`-x cmd[,cmd...]`
:   Do not run any cmd in the list, even if it seems to be needed.
    cmd can be ‘`autoconf`’, ‘`autoheader`’, ‘`automake`’,
    ‘`aclocal`’, ‘`autopoint`’, ‘`libtoolize`’, ‘`intltoolize`’,
    ‘`gtkdocize`’, or ‘`make`’.

    ‘`--exclude make`’ has priority over ‘`--make`’.

`--include=dir`

`-I dir`
:   Append dir to the include path. Multiple invocations accumulate.
    Passed on to `aclocal`, `autoconf` and
    `autoheader` internally.

`--prepend-include=dir`

`-B dir`
:   Prepend dir to the include path. Multiple invocations accumulate.
    Passed on to `autoconf` and `autoheader` internally.

`--warnings=category[,category...]` [¶](#index-WARNINGS-1)

`-Wcategory[,category...]`
:   Enable or disable warnings related to each category.
    See [m4\_warn](#m4_005fwarn), for a comprehensive list of categories.
    Special values include:

    ‘`all`’
    :   Enable all categories of warnings.

    ‘`none`’
    :   Disable all categories of warnings.

    ‘`error`’
    :   Treat all warnings as errors.

    ‘`no-category`’
    :   Disable warnings falling into category.

    The environment variable `WARNINGS` may also be set to a
    comma-separated list of warning categories to enable or disable.
    It is interpreted exactly the same way as the argument of
    `--warnings`, but unknown categories are silently ignored.
    The command line takes precedence; for instance, if `WARNINGS`
    is set to `obsolete`, but `-Wnone` is given on the
    command line, no warnings will be issued.

    Some categories of warnings are on by default.
    Again, for details see [m4\_warn](#m4_005fwarn).

If you want `autoreconf` to pass flags that are not listed here
on to `aclocal`, set `ACLOCAL_AMFLAGS` in your `Makefile.am`.
Due to a limitation in the Autoconf implementation these flags currently
must be set on a single line in `Makefile.am`, without any
backslash-newlines or makefile comments.
Also, be aware that future Automake releases might
start flagging `ACLOCAL_AMFLAGS` as obsolescent, or even remove
support for it.

---

Next: [Existing Tests](#Existing-Tests), Previous: [Making `configure` Scripts](#Making-configure-Scripts), Up: [Autoconf](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 4 Initialization and Output Files [¶](#Initialization-and-Output-Files)

Autoconf-generated `configure` scripts need some information about
how to initialize, such as how to find the package’s source files and
about the output files to produce. The following sections describe the
initialization and the creation of output files.

* [Initializing `configure`](#Initializing-configure)
* [Dealing with Autoconf versions](#Versioning)
* [Notices in `configure`](#Notices)
* [Configure Input: Source Code, Macros, and Auxiliary Files](#Input)
* [Outputting Files](#Output)
* [Performing Configuration Actions](#Configuration-Actions)
* [Creating Configuration Files](#Configuration-Files)
* [Substitutions in Makefiles](#Makefile-Substitutions)
* [Configuration Header Files](#Configuration-Headers)
* [Running Arbitrary Configuration Commands](#Configuration-Commands)
* [Creating Configuration Links](#Configuration-Links)
* [Configuring Other Packages in Subdirectories](#Subdirectories)
* [Default Prefix](#Default-Prefix)

---

Next: [Dealing with Autoconf versions](#Versioning), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.1 Initializing `configure` [¶](#Initializing-configure-1)

Every `configure` script must call `AC_INIT` before doing
anything else that produces output. Calls to silent macros, such as
`AC_DEFUN`, may also occur prior to `AC_INIT`, although these
are generally used via `aclocal.m4`, since that is implicitly
included before the start of `configure.ac`. The only other
required macro is `AC_OUTPUT` (see [Outputting Files](#Output)).

Macro: **AC\_INIT**(package, version, [bug-report], [tarname], [url]) [¶](#index-AC_005fINIT-2)
:   Process any command-line arguments and perform initialization
    and verification.

    Set the name of the package and its version. These are
    typically used in `--version` support, including that of
    `configure`. The optional argument bug-report should be
    the email to which users should send bug reports. The package
    tarname differs from package: the latter designates the full
    package name (e.g., ‘`GNU Autoconf`’), while the former is meant for
    distribution tar ball names (e.g., ‘`autoconf`’). It defaults to
    package with ‘`GNU` ’ stripped, lower-cased, and all characters
    other than alphanumerics and underscores are changed to ‘`-`’. If
    provided, url should be the home page for the package.

    Leading and trailing whitespace is stripped from all the arguments to
    `AC_INIT`, and interior whitespace is collapsed to a single space.
    This means that, for instance, if you want to put several email
    addresses in bug-report, you can put each one on its own line:

    ```
    # We keep having problems with the mail hosting for
    # gnomovision.example, so give people an alternative.
    AC_INIT([Gnomovision], [17.0.1], [
        bugs@gnomovision.example
        or gnomo-bugs@reliable-email.example
    ])
    ```

    The arguments to `AC_INIT` may be computed by M4, when
    `autoconf` is run. For instance, if you want to include the
    package’s version number in the tarname, but you don’t want to
    repeat it, you can use a helper macro:

    ```
    m4_define([gnomo_VERSION], [17.0.1])
    AC_INIT([Gnomovision],
            m4_defn([gnomo_VERSION]),
            [bugs@gnomovision.example],
            [gnomo-]m4_defn([gnomo_VERSION]))
    ```

    This uses `m4_defn` to produce the expansion of
    `gnomo_VERSION` *as a quoted string*, so that if there happen
    to be any more M4 macro names in `gnomo_VERSION`, they will not be
    expanded. See [Renaming Macros](https://www.gnu.org/software/m4/manual/m4.html#Defn) in GNU m4 macro processor.

    Continuing this example, if you don’t want to embed the version number
    in `configure.ac` at all, you can use `m4_esyscmd` to look it
    up somewhere else when `autoconf` is run:

    ```
    m4_define([gnomo_VERSION],
      m4_esyscmd([build-aux/git-version-gen .tarball-version]))
    AC_INIT([Gnomovision],
            m4_defn([gnomo_VERSION]),
            [bugs@gnomovision.example],
            [gnomo-]m4_defn([gnomo_VERSION]))
    ```

    This uses the utility script `git-version-gen` to look up
    the package’s version in its version control metadata. This script
    is part of Gnulib (see [Gnulib](#Gnulib)).

    The arguments to `AC_INIT` are written into `configure` in
    several different places. Therefore, we strongly recommend that you
    write any M4 logic in `AC_INIT` arguments to be evaluated
    *before* `AC_INIT` itself is evaluated. For instance, in the
    above example, the second argument to `m4_define` is *not*
    quoted, so the `m4_esyscmd` is evaluated only once, and
    `gnomo_VERSION` is defined to the output of the command. If the
    second argument to `m4_define` were quoted, `m4_esyscmd` would
    be evaluated each time the version or tarname arguments were
    written to `configure`, and the command would be run repeatedly.

    In some of the places where the arguments to `AC_INIT` are used,
    within `configure`, shell evaluation cannot happen. Therefore, the
    arguments to `AC_INIT` may *not* be computed when
    `configure` is run. If they contain any construct that isn’t
    always treated as literal by the shell (e.g. variable expansions),
    `autoconf` will issue an error.

    The tarname argument is used to construct filenames. It should
    not contain wildcard characters, white space, or anything else that
    could be troublesome as part of a file or directory name.

    Some of M4’s active characters (notably parentheses, square brackets,
    ‘`,`’ and ‘`#`’) commonly appear in URLs and lists of email
    addresses. If any of these characters appear in an argument to AC\_INIT,
    that argument will probably need to be double-quoted to avoid errors
    and mistranscriptions. See [M4 Quotation](#M4-Quotation).

    The following M4 macros (e.g., `AC_PACKAGE_NAME`), output variables
    (e.g., `PACKAGE_NAME`), and preprocessor symbols (e.g.,
    `PACKAGE_NAME`), are defined by `AC_INIT`:

    `AC_PACKAGE_NAME`, `PACKAGE_NAME` [¶](#index-AC_005fPACKAGE_005fNAME)
    :   Exactly package.

    `AC_PACKAGE_TARNAME`, `PACKAGE_TARNAME` [¶](#index-AC_005fPACKAGE_005fTARNAME)
    :   Exactly tarname, possibly generated from package.

    `AC_PACKAGE_VERSION`, `PACKAGE_VERSION` [¶](#index-AC_005fPACKAGE_005fVERSION)
    :   Exactly version.

    `AC_PACKAGE_STRING`, `PACKAGE_STRING` [¶](#index-AC_005fPACKAGE_005fSTRING)
    :   Exactly ‘`package version`’.

    `AC_PACKAGE_BUGREPORT`, `PACKAGE_BUGREPORT` [¶](#index-AC_005fPACKAGE_005fBUGREPORT)
    :   Exactly bug-report, if one was provided. Typically an email
        address, or URL to a bug management web page.

    `AC_PACKAGE_URL`, `PACKAGE_URL` [¶](#index-AC_005fPACKAGE_005fURL)
    :   Exactly url, if one was provided. If url was empty, but
        package begins with ‘`GNU` ’, then this defaults to
        ‘`https://www.gnu.org/software/tarname/`’, otherwise, no URL is
        assumed.

If your `configure` script does its own option processing, it
should inspect ‘`$@`’ or ‘`$*`’ immediately after calling
`AC_INIT`, because other Autoconf macros liberally use the
`set` command to process strings, and this has the side effect
of updating ‘`$@`’ and ‘`$*`’. However, we suggest that you use
standard macros like `AC_ARG_ENABLE` instead of attempting to
implement your own option processing. See [Site Configuration](#Site-Configuration).

---

Next: [Notices in `configure`](#Notices), Previous: [Initializing `configure`](#Initializing-configure), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.2 Dealing with Autoconf versions [¶](#Dealing-with-Autoconf-versions)

The following optional macros can be used to help choose the minimum
version of Autoconf that can successfully compile a given
`configure.ac`.

Macro: **AC\_PREREQ**(version) [¶](#index-AC_005fPREREQ-1)
:   Ensure that a recent enough version of Autoconf is being used. If the
    version of Autoconf being used to create `configure` is
    earlier than version, print an error message to the standard
    error output and exit with failure (exit status is 63). For example:

    ```
    AC_PREREQ([2.73])
    ```

    This macro may be used before `AC_INIT`.

Macro: **AC\_AUTOCONF\_VERSION** [¶](#index-AC_005fAUTOCONF_005fVERSION-1)
:   This macro was introduced in Autoconf 2.62. It identifies the version
    of Autoconf that is currently parsing the input file, in a format
    suitable for `m4_version_compare` (see [m4\_version\_compare](#m4_005fversion_005fcompare)); in
    other words, for this release of Autoconf, its value is
    ‘`2.73`’. One potential use of this macro is for writing
    conditional fallbacks based on when a feature was added to Autoconf,
    rather than using `AC_PREREQ` to require the newer version of
    Autoconf. However, remember that the Autoconf philosophy favors feature
    checks over version checks.

    You should not expand this macro directly; use
    ‘`m4_defn([AC_AUTOCONF_VERSION])`’ instead. This is because some
    users might
    have a beta version of Autoconf installed, with arbitrary letters
    included in its version string. This means it is possible for the
    version string to contain the name of a defined macro, such that
    expanding `AC_AUTOCONF_VERSION` would trigger the expansion of that
    macro during rescanning, and change the version string to be different
    than what you intended to check.

---

Next: [Configure Input: Source Code, Macros, and Auxiliary Files](#Input), Previous: [Dealing with Autoconf versions](#Versioning), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 4.3 Notices in `configure` [¶](#Notices-in-configure)

The following macros manage version numbers for `configure`
scripts. Using them is optional.

Macro: **AC\_COPYRIGHT**(copyright-notice) [¶](#index-AC_005fCOPYRIGHT-1)
:   State that, in addition to the Free Software Foundation’s copyright on
    the Autoconf macros, parts of your `configure` are covered by the
    copyright-notice.

    The copyright-notice shows up in both the head of
    `configure` and in ‘`configure --version`’.

Macro: **AC\_REVISION**(revision-info) [¶](#index-AC_005fREVISION-1)
:   Copy revision stamp revision-info into the `configure`
    script, with any dollar signs or double-quotes removed. This macro lets
    you put a revision stamp from `configure.ac` into `configure`
    that the traditional version control systems RCS and CVS can update,
    without these systems changing it again when you check in the resulting
    `configure`. That way, you can determine easily which revision of
    `configure.ac` a particular `configure` corresponds to.

    For example, this line in `configure.ac`:

    ```
    AC_REVISION([$Revision: 1.1 $])
    ```

    produces this in `configure`:

    ```
    #!/bin/sh
    # From configure.ac Revision: 1.30
    ```

---

Next: [Outputting Files](#Output), Previous: [Notices in `configure`](#Notices), Up: [Initialization and Output Files](#Setup)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
