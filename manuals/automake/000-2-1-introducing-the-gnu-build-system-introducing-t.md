Next: [Introduction](#Introduction), Up: [(dir)](/manual)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]# GNU Automake [¶](#GNU-Automake)

This manual is for GNU Automake (version 1.19.92,
3 September 2026), a program that creates GNU standards-compliant
Makefiles from template files.

Copyright © 1995–2026 Free Software Foundation, Inc.

> Permission is granted to copy, distribute and/or modify this document
> under the terms of the GNU Free Documentation License,
> Version 1.3 or any later version published by the Free Software
> Foundation; with no Invariant Sections, with no Front-Cover texts,
> and with no Back-Cover Texts. A copy of the license is included in the
> section entitled “GNU Free Documentation License.”## Short Table of Contents

* [1 Introduction](#toc-Introduction)
* [2 An Introduction to the Autotools](#toc-An-Introduction-to-the-Autotools)
* [3 General ideas](#toc-General-ideas)
* [4 Some example packages](#toc-Some-example-packages)
* [5 Creating a `Makefile.in`: Invoking `automake`](#toc-Creating-a-Makefile_002ein_003a-Invoking-automake)
* [6 Scanning `configure.ac`, using `aclocal`](#toc-Scanning-configure_002eac_002c-using-aclocal)
* [7 Directories](#toc-Directories)
* [8 Building Programs and Libraries](#toc-Building-Programs-and-Libraries)
* [9 Other Derived Objects](#toc-Other-Derived-Objects)
* [10 Other GNU Tools](#toc-Other-GNU-Tools)
* [11 Building documentation](#toc-Building-documentation)
* [12 What Gets Installed](#toc-What-Gets-Installed)
* [13 What Gets Cleaned](#toc-What-Gets-Cleaned)
* [14 What Goes in a Distribution](#toc-What-Goes-in-a-Distribution)
* [15 Support for test suites](#toc-Support-for-test-suites)
* [16 Rebuilding Makefiles](#toc-Rebuilding-Makefiles)
* [17 Changing Automake’s Behavior](#toc-Changing-Automake_0027s-Behavior)
* [18 Miscellaneous Rules](#toc-Miscellaneous-Rules)
* [19 Include](#toc-Include)
* [20 Conditionals](#toc-Conditionals)
* [21 Silencing `make`](#toc-Silencing-make)
* [22 When Automake Isn’t Enough](#toc-When-Automake-Isn_0027t-Enough)
* [23 Distributing `Makefile.in`s](#toc-Distributing-Makefile_002eins)
* [24 Automake API Versioning](#toc-Automake-API-Versioning)
* [25 Upgrading a Package to a Newer Automake Version](#toc-Upgrading-a-Package-to-a-Newer-Automake-Version)
* [26 Frequently Asked Questions about Automake](#toc-Frequently-Asked-Questions-about-Automake)
* [Appendix A Copying This Manual](#toc-Copying-This-Manual)
* [Appendix B Indices](#toc-Indices)## Table of Contents

* [1 Introduction](#Introduction)
* [2 An Introduction to the Autotools](#Autotools-Introduction)
  + [2.1 Introducing the GNU Build System](#GNU-Build-System)
  + [2.2 Use Cases for the GNU Build System](#Use-Cases)
    - [2.2.1 Basic Installation](#Basic-Installation)
    - [2.2.2 Standard `Makefile` Targets](#Standard-Targets)
    - [2.2.3 Standard Directory Variables](#Standard-Directory-Variables)
    - [2.2.4 Standard Configuration Variables](#Standard-Configuration-Variables)
    - [2.2.5 Overriding Default Configuration Setting with `config.site`](#config_002esite)
    - [2.2.6 Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds)
    - [2.2.7 Two-Part Installation](#Two_002dPart-Install)
    - [2.2.8 Cross-Compilation](#Cross_002dCompilation)
    - [2.2.9 Renaming Programs at Install Time](#Renaming)
    - [2.2.10 Building Binary Packages Using DESTDIR](#DESTDIR)
    - [2.2.11 Preparing Distributions](#Preparing-Distributions)
    - [2.2.12 Automatic Dependency Tracking](#Dependency-Tracking)
    - [2.2.13 Nested Packages](#Nested-Packages)
  + [2.3 How Autotools Help](#Why-Autotools)
  + [2.4 A Small Hello World](#Hello-World)
    - [2.4.1 Creating `amhello-1.0.tar.gz`](#Creating-amhello)
    - [2.4.2 `amhello`’s `configure.ac` Setup Explained](#amhello_0027s-configure_002eac-Setup-Explained)
    - [2.4.3 `amhello`’s `Makefile.am` Setup Explained](#amhello_0027s-Makefile_002eam-Setup-Explained)
* [3 General ideas](#Generalities)
  + [3.1 General Operation](#General-Operation)
  + [3.2 Strictness](#Strictness)
  + [3.3 The Uniform Naming Scheme](#Uniform)
  + [3.4 Staying below the command line length limit](#Length-Limitations)
  + [3.5 How derived variables are named](#Canonicalization)
  + [3.6 Variables reserved for the user](#User-Variables)
  + [3.7 Programs automake might require](#Auxiliary-Programs)
* [4 Some example packages](#Examples)
  + [4.1 A simple example, start to finish](#Complete)
  + [4.2 Building true and false](#true)
* [5 Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation)
* [6 Scanning `configure.ac`, using `aclocal`](#configure)
  + [6.1 Configuration requirements](#Requirements)
  + [6.2 Other things Automake recognizes](#Optional)
  + [6.3 Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)
    - [6.3.1 aclocal Options](#aclocal-Options)
    - [6.3.2 Macro Search Path](#Macro-Search-Path)
    - [6.3.3 Writing your own aclocal macros](#Extending-aclocal)
    - [6.3.4 Handling Local Macros](#Local-Macros)
    - [6.3.5 Serial Numbers](#Serials)
    - [6.3.6 The Future of `aclocal`](#Future-of-aclocal)
  + [6.4 Autoconf macros supplied with Automake](#Macros)
    - [6.4.1 Public Macros](#Public-Macros)
    - [6.4.2 Obsolete Macros](#Obsolete-Macros)
    - [6.4.3 Private Macros](#Private-Macros)
* [7 Directories](#Directories)
  + [7.1 Recursing subdirectories](#Subdirectories)
  + [7.2 Conditional Subdirectories](#Conditional-Subdirectories)
    - [7.2.1 `SUBDIRS` vs. `DIST_SUBDIRS`](#SUBDIRS-vs-DIST_005fSUBDIRS)
    - [7.2.2 Subdirectories with `AM_CONDITIONAL`](#Subdirectories-with-AM_005fCONDITIONAL)
    - [7.2.3 Subdirectories with `AC_SUBST`](#Subdirectories-with-AC_005fSUBST)
    - [7.2.4 Unconfigured Subdirectories](#Unconfigured-Subdirectories)
  + [7.3 An Alternative Approach to Subdirectories](#Alternative)
  + [7.4 Nesting Packages](#Subpackages)
* [8 Building Programs and Libraries](#Programs)
  + [8.1 Building a program](#A-Program)
    - [8.1.1 Defining program sources](#Program-Sources)
    - [8.1.2 Linking the program](#Linking)
    - [8.1.3 Conditional compilation of sources](#Conditional-Sources)
    - [8.1.4 Conditional compilation of programs](#Conditional-Programs)
  + [8.2 Building a library](#A-Library)
  + [8.3 Building a Shared Library](#A-Shared-Library)
    - [8.3.1 The Libtool Concept](#Libtool-Concept)
    - [8.3.2 Building Libtool Libraries](#Libtool-Libraries)
    - [8.3.3 Building Libtool Libraries Conditionally](#Conditional-Libtool-Libraries)
    - [8.3.4 Libtool Libraries with Conditional Sources](#Conditional-Libtool-Sources)
    - [8.3.5 Libtool Convenience Libraries](#Libtool-Convenience-Libraries)
    - [8.3.6 Libtool Modules](#Libtool-Modules)
    - [8.3.7 `_LIBADD`, `_LDFLAGS`, and `_LIBTOOLFLAGS`](#Libtool-Flags)
    - [8.3.8 `LTLIBOBJS` and `LTALLOCA`](#LTLIBOBJS)
    - [8.3.9 Common Issues Related to Libtool’s Use](#Libtool-Issues)
      * [8.3.9.1 Error: ‘`required file './ltmain.sh' not found`’—`libtoolize` needed](#Error-required-file-ltmain_002esh-not-found)
      * [8.3.9.2 Error: ‘`Libtool library used but 'LIBTOOL' is undefined`’—`libtoolize` redux](#Libtool-library-used-but-LIBTOOL-is-undefined)
      * [8.3.9.3 Objects ‘`created with both libtool and without`’](#Objects-created-both-with-libtool-and-without)
  + [8.4 Program and Library Variables](#Program-and-Library-Variables)
  + [8.5 Default `_SOURCES`](#Default-_005fSOURCES)
  + [8.6 Special handling for `LIBOBJS` and `ALLOCA`](#LIBOBJS)
  + [8.7 Variables used when building a program](#Program-Variables)
  + [8.8 Yacc and Lex support](#Yacc-and-Lex)
    - [8.8.1 Linking Multiple Yacc Parsers](#Linking-Multiple-Yacc-Parsers)
  + [8.9 C++ Support](#C_002b_002b-Support)
  + [8.10 Objective C Support](#Objective-C-Support)
  + [8.11 Objective C++ Support](#Objective-C_002b_002b-Support)
  + [8.12 Unified Parallel C Support](#Unified-Parallel-C-Support)
  + [8.13 Assembly Support](#Assembly-Support)
  + [8.14 Fortran 77 Support](#Fortran-77-Support)
    - [8.14.1 Preprocessing Fortran 77](#Preprocessing-Fortran-77)
    - [8.14.2 Compiling Fortran 77 Files](#Compiling-Fortran-77-Files)
    - [8.14.3 Mixing Fortran 77 With C and C++](#Mixing-Fortran-77-With-C-and-C_002b_002b)
      * [8.14.3.1 How the Linker is Chosen](#How-the-Linker-is-Chosen)
  + [8.15 Fortran 9x Support](#Fortran-9x-Support)
    - [8.15.1 Compiling Fortran 9x Files](#Compiling-Fortran-9x-Files)
  + [8.16 Compiling Java sources using gcj](#Java-Support-with-gcj)
  + [8.17 Vala Support](#Vala-Support)
  + [8.18 Algol 68 Support](#Algol-68-Support)
  + [8.19 Support for Other Languages](#Support-for-Other-Languages)
  + [8.20 Automatic dependency tracking](#Dependencies)
  + [8.21 Support for executable extensions](#EXEEXT)
* [9 Other Derived Objects](#Other-Objects)
  + [9.1 Executable Scripts](#Scripts)
  + [9.2 Header files](#Headers)
  + [9.3 Architecture-independent data files](#Data)
  + [9.4 Built Sources](#Sources)
    - [9.4.1 Built Sources Example](#Built-Sources-Example)
* [10 Other GNU Tools](#Other-GNU-Tools)
  + [10.1 Emacs Lisp](#Emacs-Lisp)
  + [10.2 Gettext](#gettext)
  + [10.3 Libtool](#Libtool)
  + [10.4 Java bytecode compilation (deprecated)](#Java)
  + [10.5 Python](#Python)
    - [10.5.1 Supported versions](#Supported-Python-versions)
* [11 Building documentation](#Documentation)
  + [11.1 Texinfo](#Texinfo)
  + [11.2 Man Pages](#Man-Pages)
* [12 What Gets Installed](#Install)
  + [12.1 Basics of Installation](#Basics-of-Installation)
  + [12.2 The Two Parts of Install](#The-Two-Parts-of-Install)
  + [12.3 Extending Installation](#Extending-Installation)
  + [12.4 Staged Installs](#Staged-Installs)
  + [12.5 Install Rules for the User](#Install-Rules-for-the-User)
* [13 What Gets Cleaned](#Clean)
* [14 What Goes in a Distribution](#Dist)
  + [14.1 Basics of Distribution](#Basics-of-Distribution)
  + [14.2 Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control)
  + [14.3 The dist Hook](#The-dist-Hook)
  + [14.4 Checking the Distribution](#Checking-the-Distribution)
    - [14.4.1 `DISTCHECK_CONFIGURE_FLAGS`](#DISTCHECK_005fCONFIGURE_005fFLAGS)
    - [14.4.2 `distcheck-hook`](#distcheck_002dhook)
    - [14.4.3 `dvi` and `distcheck`](#dvi-and-distcheck)
    - [14.4.4 `distcleancheck`](#distcleancheck)
    - [14.4.5 `distuninstallcheck`](#distuninstallcheck)
    - [14.4.6 Errors with `distclean`](#Errors-with-distclean)
  + [14.5 The Types of Distributions](#The-Types-of-Distributions)
* [15 Support for test suites](#Tests)
  + [15.1 Generalities about Testing](#Generalities-about-Testing)
  + [15.2 Simple Tests](#Simple-Tests)
    - [15.2.1 Scripts-based Testsuites](#Scripts_002dbased-Testsuites)
      * [15.2.1.1 Testsuite Environment Overrides](#Testsuite-Environment-Overrides)
    - [15.2.2 Older (and discouraged) Serial Test Harness](#Serial-Test-Harness)
    - [15.2.3 Parallel Test Harness](#Parallel-Test-Harness)
      * [15.2.3.1 Parallel Test Basics](#Parallel-Test-Basics)
      * [15.2.3.2 Parallel Test Runners](#Parallel-Test-Runners)
      * [15.2.3.3 Parallel Test Selection](#Parallel-Test-Selection)
      * [15.2.3.4 Parallel Test Dependencies](#Parallel-Test-Dependencies)
      * [15.2.3.5 Parallel Test Variables](#Parallel-Test-Variables)
  + [15.3 Custom Test Drivers](#Custom-Test-Drivers)
    - [15.3.1 Overview of Custom Test Drivers Support](#Overview-of-Custom-Test-Drivers-Support)
    - [15.3.2 Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers)
    - [15.3.3 API for Custom Test Drivers](#API-for-Custom-Test-Drivers)
      * [15.3.3.1 Command-line arguments for test drivers](#Command_002dline-arguments-for-test-drivers)
      * [15.3.3.2 Log files generation and test results recording](#Log-files-generation-and-test-results-recording)
      * [15.3.3.3 Testsuite progress output](#Testsuite-progress-output)
  + [15.4 Using the TAP test protocol](#Using-the-TAP-test-protocol)
    - [15.4.1 Introduction to TAP](#Introduction-to-TAP)
    - [15.4.2 Use TAP with the Automake test harness](#Use-TAP-with-the-Automake-test-harness)
      * [15.4.2.1 TAP setup and examples](#TAP-setup-and-examples)
      * [15.4.2.2 TAP stderr examples](#TAP-stderr-examples)
      * [15.4.2.3 TAP prefixing stderr](#TAP-prefixing-stderr)
    - [15.4.3 Incompatibility with other TAP parsers and drivers](#Incompatibility-with-other-TAP-parsers-and-drivers)
    - [15.4.4 Links and external resources on TAP](#Links-and-external-resources-on-TAP)
  + [15.5 DejaGnu Tests](#DejaGnu-Tests)
  + [15.6 Install Tests](#Install-Tests)
* [16 Rebuilding Makefiles](#Rebuilding)
* [17 Changing Automake’s Behavior](#Options)
  + [17.1 Options generalities](#Options-generalities)
  + [17.2 List of Automake options](#List-of-Automake-options)
* [18 Miscellaneous Rules](#Miscellaneous)
  + [18.1 Interfacing to `etags`](#Tags)
  + [18.2 Handling new file extensions](#Suffixes)
* [19 Include](#Include)
* [20 Conditionals](#Conditionals)
  + [20.1 Usage of Conditionals](#Usage-of-Conditionals)
  + [20.2 Limits of Conditionals](#Limits-of-Conditionals)
* [21 Silencing `make`](#Silencing-Make)
  + [21.1 Make is verbose by default](#Make-verbosity)
  + [21.2 Standard and generic ways to silence Make](#Tricks-For-Silencing-Make)
  + [21.3 How Automake can help in silencing Make](#Automake-Silent-Rules)
  + [21.4 Unsilencing Automake](#Unsilencing-Automake)
* [22 When Automake Isn’t Enough](#Not-Enough)
  + [22.1 Extending Automake Rules](#Extending)
  + [22.2 Third-Party `Makefile`s](#Third_002dParty-Makefiles)
* [23 Distributing `Makefile.in`s](#Distributing)
* [24 Automake API Versioning](#API-Versioning)
* [25 Upgrading a Package to a Newer Automake Version](#Upgrading)
* [26 Frequently Asked Questions about Automake](#FAQ)
  + [26.1 Version control and generated files](#Version-Control)
  + [26.2 `missing` and `AM_MAINTAINER_MODE`](#maintainer_002dmode)
  + [26.3 Why doesn’t Automake support wildcards?](#Wildcards)
  + [26.4 Limitations on File Names](#Limitations-on-File-Names)
  + [26.5 Flag Variables Ordering](#Flag-Variables-Ordering)
  + [26.6 Why are object files sometimes renamed?](#Renamed-Objects)
  + [26.7 Per-Object Flags Emulation](#Per_002dObject-Flags)
  + [26.8 Handling Tools that Produce Many Outputs](#Multiple-Outputs)
  + [26.9 Installing to Hard-Coded Locations](#Hard_002dCoded-Install-Paths)
  + [26.10 Debugging Make Rules](#Debugging-Make-Rules)
  + [26.11 Reporting Bugs](#Reporting-Bugs)
* [Appendix A Copying This Manual](#Copying-This-Manual)
  + [A.1 GNU Free Documentation License](#GNU-Free-Documentation-License)
* [Appendix B Indices](#Indices)
  + [B.1 Macro Index](#Macro-Index)
  + [B.2 Variable Index](#Variable-Index)
  + [B.3 General Index](#General-Index)

---

Next: [An Introduction to the Autotools](#Autotools-Introduction), Previous: [GNU Automake](#Top), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 1 Introduction [¶](#Introduction-1)

Automake is a tool for automatically generating `Makefile.in`s
from files called `Makefile.am`. Each `Makefile.am` is
basically a series of `make` variable
definitions[1](#FOOT1), with rules being thrown in
occasionally. The generated `Makefile.in`s are compliant with
the GNU Makefile standards.

The GNU Makefile Standards Document
(see [Makefile Conventions](https://www.gnu.org/prep/standards/standards.html#Makefile-Conventions) in The GNU Coding Standards)
is long, complicated, and subject to change. The goal of Automake is to
remove the burden of Makefile maintenance from the back of the
individual GNU maintainer (and put it on the back of the Automake
maintainers).

The typical Automake input file is simply a series of variable definitions.
Each such file is processed to create a `Makefile.in`.

Automake does constrain a project in certain ways; for instance, it
assumes that the project uses Autoconf (see [Introduction](https://www.gnu.org/software/autoconf/manual/autoconf.html#Top) in The Autoconf Manual), and enforces certain restrictions on
the `configure.ac` contents.

Automake requires `perl` in order to generate the
`Makefile.in`s. However, the distributions created by Automake are
fully GNU standards-compliant, and do not require `perl` in order
to be built.

For more information on bug reports, See [Reporting Bugs](#Reporting-Bugs).

---

Next: [General ideas](#Generalities), Previous: [Introduction](#Introduction), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 2 An Introduction to the Autotools [¶](#An-Introduction-to-the-Autotools)

If you are new to Automake, maybe you know that it is part of a set of
tools called *The Autotools*. Maybe you’ve already delved into a
package full of files named `configure`, `configure.ac`,
`Makefile.in`, `Makefile.am`, `aclocal.m4`, …,
some of them claiming to be *generated by* Autoconf or Automake.
But the exact purpose of these files and their relations is probably
fuzzy. The goal of this chapter is to introduce you to this machinery,
to show you how it works and how powerful it is. If you’ve never
installed or seen such a package, do not worry: this chapter will walk
you through it.

If you need some teaching material, more illustrations, or a less
`automake`-centered continuation, some slides for this
introduction are available in Alexandre Duret-Lutz’s
[Autotools Tutorial](https://www.lrde.epita.fr/~adl/autotools.html).
This chapter is the written version of the first part of his tutorial.

* [Introducing the GNU Build System](#GNU-Build-System)
* [Use Cases for the GNU Build System](#Use-Cases)
* [How Autotools Help](#Why-Autotools)
* [A Small Hello World](#Hello-World)

---

Next: [Use Cases for the GNU Build System](#Use-Cases), Up: [An Introduction to the Autotools](#Autotools-Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.1 Introducing the GNU Build System [¶](#Introducing-the-GNU-Build-System)

It is a truth universally acknowledged, that as a developer in
possession of a new package, you must be in want of a build system.

In the Unix world, such a build system is traditionally achieved using
the command `make` (see [Overview](https://www.gnu.org/software/make/manual/make.html#Top) in The GNU Make
Manual). You express the recipe to build your package in a
`Makefile`. This file is a set of rules to build the files in
the package. For instance the program `prog` may be built by
running the linker on the files `main.o`, `foo.o`, and
`bar.o`; the file `main.o` may be built by running the
compiler on `main.c`; etc. Each time `make` is run, it
reads `Makefile`, checks the existence and modification time of
the files mentioned, decides what files need to be built (or rebuilt),
and runs the associated commands.

When a package needs to be built on a different platform than the one
it was developed on, its `Makefile` usually needs to be adjusted.
For instance the compiler may have another name or require more
options. In 1991, David J. MacKenzie got tired of customizing
`Makefile` for the 20 platforms he had to deal with. Instead, he
handcrafted a little shell script called `configure` to
automatically adjust the `Makefile` (see [Genesis](https://www.gnu.org/software/autoconf/manual/autoconf.html#Genesis) in The Autoconf Manual). Compiling his package was now
as simple as running `./configure && make`.

Today this process has been standardized in the GNU project. The GNU
Coding Standards (see [The Release Process](https://www.gnu.org/prep/standards/standards.html#Managing-Releases) in The GNU Coding Standards) explains how each package of the
GNU project should have a `configure` script, and the minimal
interface it should have. The `Makefile` too should follow some
established conventions. The result? A unified build system that
makes all packages almost indistinguishable by the installer. In its
simplest scenario, all the installer has to do is to unpack the
package, run `./configure && make && make install`, and repeat
with the next package to install.

We call this build system the *GNU Build System*, since it was
grown out of the GNU project. However, it is used by a vast number of
other packages: following any existing convention has its advantages.

The Autotools are tools that will create a GNU Build System for your
package. Autoconf mostly focuses on `configure` and Automake on
`Makefile`s. It is entirely possible to create a GNU Build
System without the help of these tools, but it is rather burdensome
and error-prone. We will discuss this again after some illustration
of the GNU Build System in action.

---

Next: [How Autotools Help](#Why-Autotools), Previous: [Introducing the GNU Build System](#GNU-Build-System), Up: [An Introduction to the Autotools](#Autotools-Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.2 Use Cases for the GNU Build System [¶](#Use-Cases-for-the-GNU-Build-System)

In this section we explore several use cases for the GNU Build System.
You can replay all of these examples on the `amhello-1.0.tar.gz`
package distributed with Automake. If Automake is installed on your
system, you should find a copy of this file in
`prefix/share/doc/automake/amhello-1.0.tar.gz`, where
prefix is the installation prefix specified during configuration
(prefix defaults to `/usr/local`, however if Automake was
installed by some GNU/Linux distribution it most likely has been set
to `/usr`). If you do not have a copy of Automake installed,
you can find a copy of this file inside the `doc/` directory of
the Automake package.

Some of the following use cases present features that are in fact
extensions to the GNU Build System. Read: they are not specified by
the GNU Coding Standards, but they are nonetheless part of the build
system created by the Autotools. To keep things simple, we do not
point out the difference. Our objective is to show you many of the
features that the build system created by the Autotools will offer to
you.

* [Basic Installation](#Basic-Installation)
* [Standard `Makefile` Targets](#Standard-Targets)
* [Standard Directory Variables](#Standard-Directory-Variables)
* [Standard Configuration Variables](#Standard-Configuration-Variables)
* [Overriding Default Configuration Setting with `config.site`](#config_002esite)
* [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds)
* [Two-Part Installation](#Two_002dPart-Install)
* [Cross-Compilation](#Cross_002dCompilation)
* [Renaming Programs at Install Time](#Renaming)
* [Building Binary Packages Using DESTDIR](#DESTDIR)
* [Preparing Distributions](#Preparing-Distributions)
* [Automatic Dependency Tracking](#Dependency-Tracking)
* [Nested Packages](#Nested-Packages)

---

Next: [Standard `Makefile` Targets](#Standard-Targets), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.1 Basic Installation [¶](#Basic-Installation-1)

The most common installation procedure looks as follows.

```
~ % tar zxf amhello-1.0.tar.gz
~ % cd amhello-1.0
~/amhello-1.0 % ./configure
...
config.status: creating Makefile
config.status: creating src/Makefile
...
~/amhello-1.0 % make
...
~/amhello-1.0 % make check
...
~/amhello-1.0 % su
Password:
/home/adl/amhello-1.0 # make install
...
/home/adl/amhello-1.0 # exit
~/amhello-1.0 % make installcheck
...
```

The user first unpacks the package. Here, and in the following
examples, we will use the non-portable `tar zxf` command for
simplicity. On a system without GNU `tar` installed, this
command should read `gunzip -c amhello-1.0.tar.gz | tar xf -`.

The user then enters the newly created directory to run the
`configure` script. This script probes the system for various
features, and finally creates the `Makefile`s. In this toy
example there are only two `Makefile`s, but in real-world projects,
there may be many more, usually one `Makefile` per directory.

It is now possible to run `make`. This will construct all the
programs, libraries, and scripts that need to be constructed for the
package. In our example, this compiles the `hello` program.
All files are constructed in place, in the source tree; we will see
later how this can be changed.

`make check` causes the package’s tests to be run. This step is
not mandatory, but it is often good to make sure the programs that
have been built behave as they should, before you decide to install
them. Our example does not contain any tests, so running `make
check` is a no-op.

After everything has been built, and maybe tested, it is time to
install it on the system. That means copying the programs,
libraries, header files, scripts, and other data files from the
source directory to their final destination on the system. The
command `make install` will do that. However, by default
everything will be installed in subdirectories of `/usr/local`:
binaries will go into `/usr/local/bin`, libraries will end up in
`/usr/local/lib`, etc. This destination is usually not writable
by any user, so we assume that we have to become root before we can
run `make install`. In our example, running `make install`
will copy the program `hello` into `/usr/local/bin`
and `README` into `/usr/local/share/doc/amhello`.

A last and optional step is to run `make installcheck`. This
command may run tests on the installed files. `make check` tests
the files in the source tree, while `make installcheck` tests
their installed copies. The tests run by the latter can be different
from those run by the former. For instance, there are tests that
cannot be run in the source tree. Conversely, some packages are set
up so that `make installcheck` will run the very same tests as
`make check`, only on different files (non-installed
vs. installed). It can make a difference, for instance when the
source tree’s layout is different from that of the installation.
Furthermore it may help to diagnose an incomplete installation.

Presently most packages do not have any `installcheck` tests
because the existence of `installcheck` is little known, and its
usefulness is neglected. Our toy package is no better: `make
installcheck` does nothing.

---

Next: [Standard Directory Variables](#Standard-Directory-Variables), Previous: [Basic Installation](#Basic-Installation), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.2 Standard `Makefile` Targets [¶](#Standard-Makefile-Targets)

So far we have come across four ways to run `make` in the GNU
Build System: `make`, `make check`, `make install`, and
`make installcheck`. The words `check`, `install`, and
`installcheck`, passed as arguments to `make`, are called
*targets*. `make` is a shorthand for `make all`,
`all` being the default target in the GNU Build System.

Here is a list of the most commonly useful targets that the GNU Coding
Standards specify.

`make all` [¶](#index-all)
:   Build programs, libraries, documentation, etc. (same as `make`).

`make install` [¶](#index-install)
:   Install what needs to be installed, copying the files from the
    package’s tree to system-wide directories.

`make uninstall` [¶](#index-uninstall)
:   The opposite of `make install`: erase the installed files.
    (This needs to be run from the same build tree that was installed.)

`make clean` [¶](#index-clean)
:   Erase from the build tree the files built by `make all`.

`make distclean` [¶](#index-distclean)
:   Additionally erase anything `./configure` created.

`make check` [¶](#index-check)
:   Run the test suite, if any.

`make installcheck` [¶](#index-installcheck)
:   Check the installed programs or libraries, if supported.

`make dist` [¶](#index-dist)
:   Recreate `package-version.tar.gz` from all the source
    files.

---

Next: [Standard Configuration Variables](#Standard-Configuration-Variables), Previous: [Standard `Makefile` Targets](#Standard-Targets), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.3 Standard Directory Variables [¶](#Standard-Directory-Variables-1)

The GNU Coding Standards also specify a hierarchy of variables to
denote installation directories. Some of these are:

| Directory variable | Default value |
| --- | --- |
| `prefix` | `/usr/local` |
| `exec_prefix` | `${prefix}` |
| `bindir` | `${exec_prefix}/bin` |
| `libdir` | `${exec_prefix}/lib` |
| … |
| `includedir` | `${prefix}/include` |
| `datarootdir` | `${prefix}/share` |
| `datadir` | `${datarootdir}` |
| `mandir` | `${datarootdir}/man` |
| `infodir` | `${datarootdir}/info` |
| `docdir` | `${datarootdir}/doc/${PACKAGE}` |
| … |

Each of these directories has a role which is often obvious from its
name. In a package, any installable file will be installed in one of
these directories. For instance in `amhello-1.0`, the program
`hello` is to be installed in bindir, the directory for
binaries. The default value for this directory is
`/usr/local/bin`, but the user can supply a different value when
calling `configure`. Also the file `README` will be
installed into docdir, which defaults to
`/usr/local/share/doc/amhello`.

As a user, if you wish to install a package on your own account, you
could proceed as follows:

```
~/amhello-1.0 % ./configure --prefix ~/usr
...
~/amhello-1.0 % make
...
~/amhello-1.0 % make install
...
```

This would install `~/usr/bin/hello` and
`~/usr/share/doc/amhello/README`.

The list of all such directory options is shown by
`./configure --help`.

---

Next: [Overriding Default Configuration Setting with `config.site`](#config_002esite), Previous: [Standard Directory Variables](#Standard-Directory-Variables), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.4 Standard Configuration Variables [¶](#Standard-Configuration-Variables-1)

The GNU Coding Standards also define a set of standard configuration
variables used during the build. Here are some:

`CC`
:   C compiler command

`CFLAGS`
:   C compiler flags

`CXX`
:   C++ compiler command

`CXXFLAGS`
:   C++ compiler flags

`LDFLAGS`
:   linker flags

`CPPFLAGS`
:   C/C++ preprocessor flags

…

`configure` usually does a good job at setting appropriate
values for these variables, but there are cases where you may want to
override them. For instance you may have several versions of a
compiler installed and would like to use another one, you may have
header files installed outside the default search path of the
compiler, or even libraries out of the way of the linker.

Here is how one would call `configure` to force it to use
`gcc-3` as C compiler, use header files from
`~/usr/include` when compiling, and libraries from
`~/usr/lib` when linking.

```
~/amhello-1.0 % ./configure --prefix ~/usr CC=gcc-3 \
CPPFLAGS=-I$HOME/usr/include LDFLAGS=-L$HOME/usr/lib
```

Again, a full list of these variables appears in the output of
`./configure --help`.

---

Next: [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds), Previous: [Standard Configuration Variables](#Standard-Configuration-Variables), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.5 Overriding Default Configuration Setting with `config.site` [¶](#Overriding-Default-Configuration-Setting-with-config_002esite)

When installing several packages using the same setup, it can be
convenient to create a file to capture common settings.
If a file named `prefix/share/config.site` exists,
`configure` will source it at the beginning of its execution.

Recall the command from the previous section:

```
~/amhello-1.0 % ./configure --prefix ~/usr CC=gcc-3 \
CPPFLAGS=-I$HOME/usr/include LDFLAGS=-L$HOME/usr/lib
```

Assuming we are installing many package in `~/usr`, and will
always want to use these definitions of `CC`, `CPPFLAGS`, and
`LDFLAGS`, we can automate this by creating the following
`~/usr/share/config.site` file:

```
test -z "$CC" && CC=gcc-3
test -z "$CPPFLAGS" && CPPFLAGS=-I$HOME/usr/include
test -z "$LDFLAGS" && LDFLAGS=-L$HOME/usr/lib
```

Now, any time a `configure` script is using the `~/usr`
prefix, it will execute the above `config.site` and define
these three variables.

```
~/amhello-1.0 % ./configure --prefix ~/usr
configure: loading site script /home/adl/usr/share/config.site
...
```

See [Setting Site Defaults](https://www.gnu.org/software/autoconf/manual/autoconf.html#Site-Defaults) in The Autoconf
Manual, for more information about this feature.

---

Next: [Two-Part Installation](#Two_002dPart-Install), Previous: [Overriding Default Configuration Setting with `config.site`](#config_002esite), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.6 Parallel Build Trees (a.k.a. VPATH Builds) [¶](#Parallel-Build-Trees-_0028a_002ek_002ea_002e-VPATH-Builds_0029)

The GNU Build System distinguishes two trees: the source tree, and
the build tree. These are two directories that may be the same, or
different.

The source tree is rooted in the directory containing the
`configure` script. It contains all the source files (those that are
distributed), and may be arranged using several subdirectories.

The build tree is rooted in the current directory at the time `configure`
was run, and is populated with all object files, programs, libraries,
and other derived files built from the sources (and hence not
distributed). The build tree usually has the same subdirectory layout
as the source tree; its subdirectories are created automatically by
the build system.

If `configure` is executed in its own directory, the source and
build trees are combined: derived files are constructed in the same
directories as their sources. This was the case in our first
installation example (see [Basic Installation](#Basic-Installation)).

A common request from users is that they want to confine all derived
files to a single directory, to keep their source directories
uncluttered. Here is how we could run `configure` to create
everything in a build tree (that is, subdirectory) called `build/`.

```
~ % tar zxf ~/amhello-1.0.tar.gz
~ % cd amhello-1.0
~/amhello-1.0 % mkdir build && cd build
~/amhello-1.0/build % ../configure
...
~/amhello-1.0/build % make
...
```

These setups, where source and build trees are different, are often
called *parallel builds* or *VPATH builds*. The expression
*parallel build* is misleading: the word *parallel* is a
reference to the way the build tree shadows the source tree, it is not
about some concurrency in the way build commands are run. For this
reason we refer to such setups using the name *VPATH builds* in
the following. *VPATH* is the name of the `make` feature
used by the `Makefile`s to allow these builds (see [`VPATH` Search Path for All Prerequisites](https://www.gnu.org/software/make/manual/make.html#General-Search) in The
GNU Make Manual).

VPATH builds have other interesting uses. One is to build the same
sources with multiple configurations. For instance:

```
~ % tar zxf ~/amhello-1.0.tar.gz
~ % cd amhello-1.0
~/amhello-1.0 % mkdir debug optim && cd debug
~/amhello-1.0/debug % ../configure CFLAGS='-g -O0'
...
~/amhello-1.0/debug % make
...
~/amhello-1.0/debug % cd ../optim
~/amhello-1.0/optim % ../configure CFLAGS='-O3 -fomit-frame-pointer'
...
~/amhello-1.0/optim % make
...
```

With network file systems, a similar approach can be used to build the
same sources on different machines. For instance, suppose that the
sources are installed on a directory shared by two hosts: `HOST1`
and `HOST2`, which may be different platforms.

```
~ % cd /nfs/src
/nfs/src % tar zxf ~/amhello-1.0.tar.gz
```

On the first host, you could create a local build directory:

```
[HOST1] ~ % mkdir /tmp/amh && cd /tmp/amh
[HOST1] /tmp/amh % /nfs/src/amhello-1.0/configure
...
[HOST1] /tmp/amh % make && sudo make install
...
```

(Here we assume that the installer has configured `sudo` so it
can execute `make install` with root privileges; it is more convenient
than using `su` like in [Basic Installation](#Basic-Installation)).

On the second host, you would do exactly the same, possibly at
the same time:

```
[HOST2] ~ % mkdir /tmp/amh && cd /tmp/amh
[HOST2] /tmp/amh % /nfs/src/amhello-1.0/configure
...
[HOST2] /tmp/amh % make && sudo make install
...
```

In this scenario, nothing forbids the `/nfs/src/amhello-1.0`
directory from being read-only. In fact VPATH builds are also a means
of building packages from a read-only medium such as a CD-ROM. (The
FSF used to sell CD-ROMs with unpacked source code, before the GNU
project grew so big.)

---

Next: [Cross-Compilation](#Cross_002dCompilation), Previous: [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.7 Two-Part Installation [¶](#Two_002dPart-Installation)

In our last example (see [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds)), a source tree was shared
by two hosts, but compilation and installation were done separately on
each host.

The GNU Build System also supports networked setups where part of the
installed files should be shared among multiple hosts. It does so
by distinguishing architecture-dependent files from
architecture-independent files, and providing two `Makefile`
targets to install each of these classes of files.

These targets are `install-exec` for architecture-dependent files
and `install-data` for architecture-independent files.
The command we used up to now, `make install`, can be thought of
as a shorthand for `make install-exec install-data`.

From the GNU Build System point of view, the distinction between
architecture-dependent files and architecture-independent files is
based exclusively on the directory variable used to specify their
installation destination. In the list of directory variables we
provided earlier (see [Standard Directory Variables](#Standard-Directory-Variables)), all the
variables based on exec-prefix designate architecture-dependent
directories whose files will be installed by `make install-exec`.
The others designate architecture-independent directories and will
serve files installed by `make install-data`. See [The Two Parts of Install](#The-Two-Parts-of-Install), for more details.

Here is how we could revisit our two-host installation example,
assuming that (1) we want to install the package directly in
`/usr`, and (2) the directory `/usr/share` is shared by the
two hosts.

On the first host we would run

```
[HOST1] ~ % mkdir /tmp/amh && cd /tmp/amh
[HOST1] /tmp/amh % /nfs/src/amhello-1.0/configure --prefix /usr
...
[HOST1] /tmp/amh % make && sudo make install
...
```

On the second host, however, we need only install the
architecture-specific files.

```
[HOST2] ~ % mkdir /tmp/amh && cd /tmp/amh
[HOST2] /tmp/amh % /nfs/src/amhello-1.0/configure --prefix /usr
...
[HOST2] /tmp/amh % make && sudo make install-exec
...
```

In packages that have installation checks, it would make sense to run
`make installcheck` (see [Basic Installation](#Basic-Installation)) to verify that
the package works correctly despite the apparent partial installation.

---

Next: [Renaming Programs at Install Time](#Renaming), Previous: [Two-Part Installation](#Two_002dPart-Install), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.8 Cross-Compilation [¶](#Cross_002dCompilation-1)

To *cross-compile* is to build on one platform a binary that will
run on another platform. When speaking of cross-compilation, it is
important to distinguish between the *build platform* on which
the compilation is performed, and the *host platform* on which the
resulting executable is expected to run. The following
`configure` options are used to specify each of them:

`--build=build` [¶](#index-_002d_002dbuild_003dbuild)
:   The system on which the package is built.

`--host=host` [¶](#index-_002d_002dhost_003dhost)
:   The system where built programs and libraries will run.

When the `--host` is used, `configure` will search for
the cross-compiling suite for this platform. Cross-compilation tools
commonly have their target architecture as prefix of their name. For
instance my cross-compiler for MinGW32 has its binaries called
`i586-mingw32msvc-gcc`, `i586-mingw32msvc-ld`,
`i586-mingw32msvc-as`, etc.

Here is how we could build `amhello-1.0` for
`i586-mingw32msvc` on a GNU/Linux PC.

```
~/amhello-1.0 % ./configure --build i686-pc-linux-gnu --host i586-mingw32msvc
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for gawk... gawk
checking whether make sets $(MAKE)... yes
checking for i586-mingw32msvc-strip... i586-mingw32msvc-strip
checking for i586-mingw32msvc-gcc... i586-mingw32msvc-gcc
checking for C compiler default output file name... a.exe
checking whether the C compiler works... yes
checking whether we are cross compiling... yes
checking for suffix of executables... .exe
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether i586-mingw32msvc-gcc accepts -g... yes
checking for i586-mingw32msvc-gcc option to accept ANSI C...
...
~/amhello-1.0 % make
...
~/amhello-1.0 % cd src; file hello.exe
hello.exe: MS Windows PE 32-bit Intel 80386 console executable not relocatable
```

The `--host` and `--build` options are usually all we
need for cross-compiling. The only exception is if the package being
built is itself a cross-compiler: we need a third option to specify
its target architecture.

`--target=target` [¶](#index-_002d_002dtarget_003dtarget)
:   When building compiler tools: the system for which the tools will
    create output.

For instance when installing GCC, the GNU Compiler Collection, we can
use `--target=target` to specify that we want to build
GCC as a cross-compiler for target. Mixing `--build` and
`--target`, we can cross-compile a cross-compiler; such a
three-way cross-compilation is known as a *Canadian cross*.

See [Specifying the System Type](https://www.gnu.org/software/autoconf/manual/autoconf.html#Specifying-Names) in The
Autoconf Manual, for more information about these `configure`
options.

---

Next: [Building Binary Packages Using DESTDIR](#DESTDIR), Previous: [Cross-Compilation](#Cross_002dCompilation), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.9 Renaming Programs at Install Time [¶](#Renaming-Programs-at-Install-Time)

The GNU Build System provides means to automatically rename
executables and manpages before they are installed (see [Man Pages](#Man-Pages)).
This is especially convenient
when installing a GNU package on a system that already has a
proprietary implementation you do not want to overwrite. For instance,
you may want to install GNU `tar` as `gtar` so you can
distinguish it from your vendor’s `tar`.

This can be done using one of these three `configure` options.

`--program-prefix=prefix` [¶](#index-_002d_002dprogram_002dprefix_003dprefix)
:   Prepend prefix to installed program names.

`--program-suffix=suffix` [¶](#index-_002d_002dprogram_002dsuffix_003dsuffix)
:   Append suffix to installed program names.

`--program-transform-name=program` [¶](#index-_002d_002dprogram_002dtransform_002dname_003dprogram)
:   Run `sed program` on installed program names.

The following commands would install `hello`
as `/usr/local/bin/test-hello`, for instance.

```
~/amhello-1.0 % ./configure --program-prefix test-
...
~/amhello-1.0 % make
...
~/amhello-1.0 % sudo make install
...
```

---

Next: [Preparing Distributions](#Preparing-Distributions), Previous: [Renaming Programs at Install Time](#Renaming), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.10 Building Binary Packages Using DESTDIR [¶](#Building-Binary-Packages-Using-DESTDIR)

The GNU Build System’s `make install` and `make uninstall`
interface does not exactly fit the needs of a system administrator
who has to deploy and upgrade packages on lots of hosts. In other
words, the GNU Build System does not replace a package manager.

Such package managers usually need to know which files have been
installed by a package, so a mere `make install` is
inappropriate.

The `DESTDIR` variable can be used to perform a staged
installation. The package should be configured as if it was going to
be installed in its final location (e.g., `--prefix /usr`), but
when running `make install`, the `DESTDIR` should be set to
the absolute name of a directory into which the installation will be
diverted. From this directory it is easy to review which files are
being installed where, and finally copy them to their final location
by some means.

For instance here is how we could create a binary package containing a
snapshot of all the files to be installed.

```
~/amhello-1.0 % ./configure --prefix /usr
...
~/amhello-1.0 % make
...
~/amhello-1.0 % make DESTDIR=$HOME/inst install
...
~/amhello-1.0 % cd ~/inst
~/inst % find . -type f -print > ../files.lst
~/inst % tar zcvf ~/amhello-1.0-i686.tar.gz `cat ../files.lst`
./usr/bin/hello
./usr/share/doc/amhello/README
```

After this example, `amhello-1.0-i686.tar.gz` is ready to be
decompressed in `/` on many hosts. (Using `` `cat ../files.lst` ``
instead of ‘`.`’ as argument for `tar` avoids entries for
each subdirectory in the archive: we would not like `tar` to
restore the modification time of `/`, `/usr/`, etc.)

Note that when building packages for several architectures, it might
be convenient to use `make install-data` and `make
install-exec` (see [Two-Part Installation](#Two_002dPart-Install)) to gather
architecture-independent files in a single package.

See [What Gets Installed](#Install), for more information.

---

Next: [Automatic Dependency Tracking](#Dependency-Tracking), Previous: [Building Binary Packages Using DESTDIR](#DESTDIR), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.11 Preparing Distributions [¶](#Preparing-Distributions-1)

We have already mentioned `make dist`. This target collects all
your source files and the necessary parts of the build system to
create a tarball named `package-version.tar.gz`.

Another, more useful command is `make distcheck`. The
`distcheck` target constructs
`package-version.tar.gz` just as well as `dist`,
but it additionally ensures most of the use cases presented so far
work:

* It attempts a full compilation of the package (see [Basic Installation](#Basic-Installation)): unpacking the newly constructed tarball, running
  `make`, `make dvi`, `make check`, `make install`,
  as well as `make installcheck`, and even `make dist`,
* it tests VPATH builds with read-only source tree (see [Parallel Build Trees (a.k.a. VPATH Builds)](#VPATH-Builds)),
* it makes sure `make clean`, `make distclean`, and `make
  uninstall` do not omit any file (see [Standard `Makefile` Targets](#Standard-Targets)),
* and it checks that `DESTDIR` installations work (see [Building Binary Packages Using DESTDIR](#DESTDIR)).

All of these actions are performed in a temporary directory, so that
no root privileges are required. The exact location and the exact
structure of such a subdirectory (where the extracted sources are
placed, how the temporary build and install directories are named and
how deeply they are nested, etc.) is to be considered an
implementation detail, which can change at any time; so do not rely on
it.

Releasing a package that fails `make distcheck` means that one of
the scenarios we presented will not work and some users will be
disappointed. Therefore it is a good practice to release a package
only after a successful `make distcheck`. This of course does
not imply that the package will be flawless, but at least it will
prevent some of the embarrassing errors you may find in packages
released by people who have never heard about `distcheck` (like
`DESTDIR` not working because of a typo, or a distributed file
being erased by `make clean`, or even `VPATH` builds not
working).

See [Creating `amhello-1.0.tar.gz`](#Creating-amhello), to recreate `amhello-1.0.tar.gz` using
`make distcheck`. See [Checking the Distribution](#Checking-the-Distribution), for more
information about `distcheck`.

---

Next: [Nested Packages](#Nested-Packages), Previous: [Preparing Distributions](#Preparing-Distributions), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.12 Automatic Dependency Tracking [¶](#Automatic-Dependency-Tracking)

Dependency tracking is performed as a side-effect of compilation.
Each time the build system compiles a source file, it computes its
list of dependencies (in C these are the header files included by the
source being compiled). Later, any time `make` is run and a
dependency appears to have changed, the dependent files will be
rebuilt.

Automake generates code for automatic dependency tracking by default,
unless the developer chooses to override it; for more information,
see [Automatic dependency tracking](#Dependencies).

When `configure` is executed, you can see it probing each
compiler for the dependency mechanism it supports (several mechanisms
can be used):

```
~/amhello-1.0 % ./configure --prefix /usr
...
checking dependency style of gcc... gcc3
...
```

Because dependencies are only computed as a side-effect of the
compilation, no dependency information exists the first time a package
is built. This is OK because all the files need to be built anyway:
`make` does not have to decide which files need to be rebuilt.
In fact, dependency tracking is completely useless for one-time builds
and there is a `configure` option to disable this:

`--disable-dependency-tracking` [¶](#index-_002d_002ddisable_002ddependency_002dtracking)
:   Speed up one-time builds.

Some compilers do not offer any practical way to derive the list of
dependencies as a side-effect of the compilation, requiring a separate
run (maybe of another tool) to compute these dependencies. The
performance penalty implied by these methods is important enough to
disable them by default. The option `--enable-dependency-tracking`
must be passed to `configure` to activate them.

`--enable-dependency-tracking` [¶](#index-_002d_002denable_002ddependency_002dtracking)
:   Do not reject slow dependency extractors.

See [Dependency Tracking Evolution](https://www.gnu.org/software/automake/history/automake-history.html#Dependency-Tracking-Evolution) in Brief History of Automake, for some discussion about
the different dependency tracking schemes used by Automake over the years.

---

Previous: [Automatic Dependency Tracking](#Dependency-Tracking), Up: [Use Cases for the GNU Build System](#Use-Cases)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.2.13 Nested Packages [¶](#Nested-Packages-1)

Although nesting packages isn’t something we would recommend to
someone who is discovering the Autotools, it is a nice feature worthy
of mention in this small advertising tour.

Autoconfiscated packages (meaning packages with a build system created
by Autoconf and friends) can be nested to arbitrary depth.

A typical setup is that package A will distribute one of the libraries
it needs in a subdirectory. This library B is a complete package with
its own GNU Build System. The `configure` script of A will
run the `configure` script of B as part of its execution;
building and installing A will also build and install B. Generating a
distribution for A will also include B.

It is possible to gather several packages like this. GCC is a heavy
user of this feature. This gives installers a single package to
configure, build and install, while it allows developers to work on
subpackages independently.

When configuring nested packages, the `configure` options
given to the top-level `configure` are passed recursively to
nested `configure`s. A package that does not understand an
option will ignore it, assuming it is meaningful to some other
package.

The command `configure --help=recursive` can be used to display
the options supported by all the included packages.

See [Nesting Packages](#Subpackages), for an example setup.

---

Next: [A Small Hello World](#Hello-World), Previous: [Use Cases for the GNU Build System](#Use-Cases), Up: [An Introduction to the Autotools](#Autotools-Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.3 How Autotools Help [¶](#How-Autotools-Help)

There are several reasons why you may not want to implement the GNU
Build System yourself (read: write a `configure` script and
`Makefile`s yourself).

* As we have seen, the GNU Build System has a lot of
  features (see [Use Cases for the GNU Build System](#Use-Cases)).
  Some users may expect features you have not implemented because
  you did not need them.
* Implementing these features portably is difficult and exhausting.
  Think of writing portable shell scripts, and portable
  `Makefile`s, for systems you may not have handy. See [Portable Shell Programming](https://www.gnu.org/software/autoconf/manual/autoconf.html#Portable-Shell) in The Autoconf Manual, to
  convince yourself.
* You will have to upgrade your setup to follow changes to the GNU
  Coding Standards.

The GNU Autotools take all this burden off your back and provide:

* Tools to create a portable, complete, and self-contained GNU Build
  System, from simple instructions.
  *Self-contained* meaning the resulting build system does not
  require the GNU Autotools.
* A central place where fixes and improvements are made:
  a bug-fix for a portability issue will benefit every package.

Yet there also exist reasons why you may want NOT to use the
Autotools... For instance you may be already using (or used to)
another incompatible build system. Autotools will only be useful if
you do accept the concepts of the GNU Build System. People who have their
own idea of how a build system should work will feel frustrated by the
Autotools.

---

Previous: [How Autotools Help](#Why-Autotools), Up: [An Introduction to the Autotools](#Autotools-Introduction)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 2.4 A Small Hello World [¶](#A-Small-Hello-World)

In this section we recreate the `amhello-1.0` package from
scratch. The first subsection shows how to call the Autotools to
instantiate the GNU Build System, while the second explains the
meaning of the `configure.ac` and `Makefile.am` files read
by the Autotools.

* [Creating `amhello-1.0.tar.gz`](#Creating-amhello)
* [`amhello`’s `configure.ac` Setup Explained](#amhello_0027s-configure_002eac-Setup-Explained)
* [`amhello`’s `Makefile.am` Setup Explained](#amhello_0027s-Makefile_002eam-Setup-Explained)

---

Next: [`amhello`’s `configure.ac` Setup Explained](#amhello_0027s-configure_002eac-Setup-Explained), Up: [A Small Hello World](#Hello-World)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.4.1 Creating `amhello-1.0.tar.gz` [¶](#Creating-amhello_002d1_002e0_002etar_002egz)

Here is how we can recreate `amhello-1.0.tar.gz` from scratch.
The package is simple enough so that we will only need to write 5
files. (You may copy them from the final `amhello-1.0.tar.gz`
that is distributed with Automake if you do not want to write them.)

Create the following files in an empty directory.

* `src/main.c` is the source file for the `hello` program. We
  store it in the `src/` subdirectory, because later, when the package
  evolves, it will ease the addition of a `man/` directory for man
  pages, a `data/` directory for data files, etc.

  ```
  ~/amhello % cat src/main.c
  #include <config.h>
  #include <stdio.h>

  int
  main (void)
  {
    puts ("Hello World!");
    puts ("This is " PACKAGE_STRING ".");
    return 0;
  }
  ```
* `README` contains some very limited documentation for our little
  package.

  ```
  ~/amhello % cat README
  This is a demonstration package for GNU Automake.
  Type 'info Automake' to read the Automake manual.
  ```
* `Makefile.am` and `src/Makefile.am` contain Automake
  instructions for these two directories.

  ```
  ~/amhello % cat src/Makefile.am
  bin_PROGRAMS = hello
  hello_SOURCES = main.c
  ~/amhello % cat Makefile.am
  SUBDIRS = src
  dist_doc_DATA = README
  ```
* Finally, `configure.ac` contains Autoconf instructions to
  create the `configure` script.

  ```
  ~/amhello % cat configure.ac
  AC_INIT([amhello], [1.0], [bug-automake@gnu.org])
  AM_INIT_AUTOMAKE([-Wall -Werror foreign])
  AC_PROG_CC
  AC_CONFIG_HEADERS([config.h])
  AC_CONFIG_FILES([
   Makefile
   src/Makefile
  ])
  AC_OUTPUT
  ```

Once you have these five files, it is time to run the Autotools to
instantiate the build system. Do this using the `autoreconf`
command as follows:

```
~/amhello % autoreconf --install
configure.ac: installing './install-sh'
configure.ac: installing './missing'
configure.ac: installing './compile'
src/Makefile.am: installing './depcomp'
```

At this point the build system is complete.

In addition to the three scripts mentioned in its output, you can see
that `autoreconf` created four other files: `configure`,
`config.h.in`, `Makefile.in`, and `src/Makefile.in`.
The latter three files are templates that will be adapted to the
system by `configure` under the names `config.h`,
`Makefile`, and `src/Makefile`. Let’s do this:

```
~/amhello % ./configure
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking for gcc... gcc
checking for C compiler default output file name... a.out
checking whether the C compiler works... yes
checking whether we are cross compiling... no
checking for suffix of executables...
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking for style of include used by make... GNU
checking dependency style of gcc... gcc3
configure: creating ./config.status
config.status: creating Makefile
config.status: creating src/Makefile
config.status: creating config.h
config.status: executing depfiles commands
```

You can see `Makefile`, `src/Makefile`, and `config.h`
being created at the end after `configure` has probed the
system. It is now possible to run all the targets we wish
(see [Standard `Makefile` Targets](#Standard-Targets)). For instance:

```
~/amhello % make
...
~/amhello % src/hello
Hello World!
This is amhello 1.0.
~/amhello % make distcheck
...
=============================================
amhello-1.0 archives ready for distribution:
amhello-1.0.tar.gz
=============================================
```

Running `autoreconf` is only needed initially, when
the GNU Build System does not exist. When you later change some
instructions in a `Makefile.am` or `configure.ac`, the
relevant part of the build system will be regenerated automatically
when you execute `make`.

`autoreconf` is a script that calls `autoconf`,
`automake`, and related commands in the right order. If you
are beginning with these tools, it is not important to figure out in
which order all of these tools should be invoked and why. (But for the
curious: the basic idea is to run:

1. `aclocal` (see [Auto-generating aclocal.m4: Invoking `aclocal`](#aclocal-Invocation)),
2. `autoconf` (see [The Autoconf Manual](https://www.gnu.org/software/autoconf/manual/autoconf.html#Top)),
3. (if needed) `autoheader` (part of the
   Autoconf distribution), and
4. `automake` (see [Creating a `Makefile.in`: Invoking `automake`](#automake-Invocation)).

If needed, tools related to Gettext (see [GNU gettext
utilities](https://www.gnu.org/software/gettext/manual/gettext.html#Top)) and Libtool (see [The Libtool Manual](https://www.gnu.org/software/libtool/manual/libtool.html#Top)) are also
invoked at different points. Thus, as you can see, using
`autoreconf` is quite a bit more convenient.

Nevertheless, because Autoconf and Automake have separate manuals, an
essential idea to understand is that Autoconf is in charge of creating
`configure` from `configure.ac`, while Automake is in charge
of creating `Makefile.in`s from `Makefile.am`s and
`configure.ac`. This should at least direct you to the right
manual when seeking answers.

---

Next: [`amhello`’s `Makefile.am` Setup Explained](#amhello_0027s-Makefile_002eam-Setup-Explained), Previous: [Creating `amhello-1.0.tar.gz`](#Creating-amhello), Up: [A Small Hello World](#Hello-World)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.4.2 `amhello`’s `configure.ac` Setup Explained [¶](#amhello_0027s-configure_002eac-Setup-Explained-1)

Let us begin with the contents of `configure.ac`.

```
AC_INIT([amhello], [1.0], [bug-automake@gnu.org])
AM_INIT_AUTOMAKE([-Wall -Werror foreign])
AC_PROG_CC
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([
 Makefile
 src/Makefile
])
AC_OUTPUT
```

This file is read by both `autoconf` (to create
`configure`) and `automake` (to create the various
`Makefile.in`s). It contains a series of M4 macros that will be
expanded as shell code to finally form the `configure` script.
We will not elaborate on the syntax of this file, because the Autoconf
manual has a whole section about it (see [Writing `configure.ac`](https://www.gnu.org/software/autoconf/manual/autoconf.html#Writing-Autoconf-Input) in The Autoconf Manual).

The macros prefixed with `AC_` are Autoconf macros, documented
in the Autoconf manual (see [Autoconf Macro
Index](https://www.gnu.org/software/autoconf/manual/autoconf.html#Autoconf-Macro-Index) in The Autoconf Manual). The macros that start with
`AM_` are Automake macros, documented later in this manual
(see [Macro Index](#Macro-Index)).

The first two lines of `configure.ac` initialize Autoconf and
Automake. `AC_INIT` takes in as parameters the name of the package,
its version number, and a contact address for bug-reports about the
package (this address is output at the end of `./configure
--help`, for instance). When adapting this setup to your own package,
by all means please do not blindly copy Automake’s address: use the
mailing list of your package, or your own mail address.

The argument to `AM_INIT_AUTOMAKE` is a list of options for
`automake` (see [Changing Automake’s Behavior](#Options)). `-Wall` and
`-Werror` ask `automake` to turn on all warnings and
report them as errors. We are speaking of *Automake* warnings
here, such as dubious instructions in `Makefile.am`. This has
absolutely nothing to do with how the compiler will be called, even
though it may support options with similar names. Using `-Wall
-Werror` is a safe setting when starting to work on a package: you do
not want to miss any issues. Later you may decide to relax things a
bit. The `foreign` option tells Automake that this package
will not follow the GNU Standards. GNU packages should always
distribute additional files such as `ChangeLog`, `AUTHORS`,
etc. We do not want `automake` to complain about these
missing files in our small example.

The `AC_PROG_CC` line causes the `configure` script to
search for a C compiler and define the variable `CC` with its
name. The `src/Makefile.in` file generated by Automake uses the
variable `CC` to build `hello`, so when `configure`
creates `src/Makefile` from `src/Makefile.in`, it will define
`CC` with the value it has found. If Automake is asked to create
a `Makefile.in` that uses `CC` but `configure.ac` does
not define it, it will suggest you add a call to `AC_PROG_CC`.

The `AC_CONFIG_HEADERS([config.h])` invocation causes the
`configure` script to create a `config.h` file gathering
‘`#define`’s defined by other macros in `configure.ac`. In our
case, the `AC_INIT` macro already defined a few of them. Here
is an excerpt of `config.h` after `configure` has run:

```
...
/* Define to the address where bug reports for this package should be sent. */
#define PACKAGE_BUGREPORT "bug-automake@gnu.org"

/* Define to the full name and version of this package. */
#define PACKAGE_STRING "amhello 1.0"
...
```

As you probably noticed, `src/main.c` includes `config.h` so
it can use `PACKAGE_STRING`. In a real-world project,
`config.h` can grow quite large, with one ‘`#define`’ per
feature probed on the system.

The `AC_CONFIG_FILES` macro declares the list of files that
`configure` should create from their `*.in` templates.
Automake also scans this list to find the `Makefile.am` files it must
process. (This is important to remember: when adding a new directory
to your project, you should add its `Makefile` to this list,
otherwise Automake will never process the new `Makefile.am` you
wrote in that directory.)

Finally, the `AC_OUTPUT` line is a closing command that actually
produces the part of the script in charge of creating the files
registered with `AC_CONFIG_HEADERS` and `AC_CONFIG_FILES`.

When starting a new project, we suggest you start with such a simple
`configure.ac`, and gradually add the other tests it requires.
The command `autoscan` can also suggest a few of the tests
your package may need (see [Using
`autoscan` to Create `configure.ac`](https://www.gnu.org/software/autoconf/manual/autoconf.html#autoscan-Invocation) in The
Autoconf Manual).

---

Previous: [`amhello`’s `configure.ac` Setup Explained](#amhello_0027s-configure_002eac-Setup-Explained), Up: [A Small Hello World](#Hello-World)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 2.4.3 `amhello`’s `Makefile.am` Setup Explained [¶](#amhello_0027s-Makefile_002eam-Setup-Explained-1)

We now turn to `src/Makefile.am`. This file contains
Automake instructions to build and install `hello`.

```
bin_PROGRAMS = hello
hello_SOURCES = main.c
```

A `Makefile.am` has the same syntax as an ordinary
`Makefile`. When `automake` processes a
`Makefile.am` it copies the entire file into the output
`Makefile.in` (that will be later turned into `Makefile` by
`configure`) but will react to certain variable definitions
by generating some build rules and other variables.
Often `Makefile.am`s contain only a list of variable definitions as
above, but they can also contain other variable and rule definitions that
`automake` will pass along without interpretation.

Variables that end with `_PROGRAMS` are special variables
that list programs that the resulting `Makefile` should build.
In Automake speak, this `_PROGRAMS` suffix is called a
*primary*; Automake recognizes other primaries such as
`_SCRIPTS`, `_DATA`, `_LIBRARIES`, etc. corresponding
to different types of files.

The ‘`bin`’ part of the `bin_PROGRAMS` tells
`automake` that the resulting programs should be installed in
bindir. Recall that the GNU Build System uses a set of variables
to denote destination directories and allow users to customize these
locations (see [Standard Directory Variables](#Standard-Directory-Variables)). Any such directory
variable can be put in front of a primary (omitting the `dir`
suffix) to tell `automake` where to install the listed files.

Programs need to be built from source files, so for each program
`prog` listed in a `_PROGRAMS` variable,
`automake` will look for another variable named
`prog_SOURCES` listing its source files. There may be more
than one source file: they will all be compiled and linked together.

Automake also knows that source files need to be distributed when
creating a tarball (unlike built programs). So a side-effect of this
`hello_SOURCES` declaration is that `main.c` will be
part of the tarball created by `make dist`.

Finally here are some explanations regarding the top-level
`Makefile.am`.

```
SUBDIRS = src
dist_doc_DATA = README
```

`SUBDIRS` is a special variable listing all directories that
`make` should recurse into before processing the current
directory. So this line is responsible for `make` building
`src/hello` even though we run it from the top-level. This line
also causes `make install` to install `src/hello` before
installing `README` (not that this order matters).

The line `dist_doc_DATA = README` causes `README` to be
distributed and installed in docdir. Files listed with the
`_DATA` primary are not automatically part of the tarball built
with `make dist`, so we add the `dist_` prefix so they get
distributed. However, for `README` it would not have been
necessary: `automake` automatically distributes any
`README` file it encounters (the list of other files
automatically distributed is presented by `automake --help`).
The only important effect of this second line is therefore to install
`README` during `make install`.

One thing not covered in this example is accessing the installation
directory values (see [Standard Directory Variables](#Standard-Directory-Variables)) from your
program code, that is, converting them into defined macros. For this,
see [Defining Directories](https://www.gnu.org/software/autoconf/manual/autoconf.html#Defining-Directories) in The Autoconf Manual.

---

Next: [Some example packages](#Examples), Previous: [An Introduction to the Autotools](#Autotools-Introduction), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## 3 General ideas [¶](#General-ideas)

The following sections cover a few basic ideas that will help you
understand how Automake works.

* [General Operation](#General-Operation)
* [Strictness](#Strictness)
* [The Uniform Naming Scheme](#Uniform)
* [Staying below the command line length limit](#Length-Limitations)
* [How derived variables are named](#Canonicalization)
* [Variables reserved for the user](#User-Variables)
* [Programs automake might require](#Auxiliary-Programs)

---

Next: [Strictness](#Strictness), Up: [General ideas](#Generalities)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.1 General Operation [¶](#General-Operation-1)

Automake works by reading a `Makefile.am` and generating a
`Makefile.in`. Certain variables and rules defined in the
`Makefile.am` instruct Automake to generate more specialized code;
for instance, a `bin_PROGRAMS` variable definition will cause rules
for compiling and linking programs to be generated.

The variable definitions and rules in the `Makefile.am` are
copied mostly verbatim into the generated file, with all variable
definitions preceding all rules. This allows you to add almost
arbitrary code into the generated `Makefile.in`. For instance,
the Automake distribution includes a non-standard rule for the
`git-dist` target, which the Automake maintainer uses to make
distributions from the source control system.

Note that most GNU Make extensions are not recognized by Automake. Using
such extensions in a `Makefile.am` will lead to errors or confusing
behavior.

A special exception is that the GNU Make append operator, ‘`+=`’, is
supported. This operator appends its right hand argument to the variable
specified on the left. Automake will translate the operator into
an ordinary ‘`=`’ operator; ‘`+=`’ will thus work with any make program.

Automake tries to keep comments grouped with any adjoining rules or
variable definitions.

Generally, Automake is not particularly smart in the parsing of unusual
`Makefile` constructs, so you’re advised to avoid fancy constructs or
“creative” use of whitespace.
For example, `TAB` characters cannot be used between a target name
and the following “`:`” character, and variable assignments
shouldn’t be indented with `TAB` characters.
Also, using more complex macros in target names can cause trouble:

```
% cat Makefile.am
$(FOO:=x): bar
% automake
Makefile.am:1: bad characters in variable name '$(FOO'
Makefile.am:1: ':='-style assignments are not portable
```

A rule defined in `Makefile.am` generally overrides any such
rule of a similar name that would be automatically generated by
`automake`. Although this is a supported feature, it is generally
best to avoid making use of it, as sometimes the generated rules are
very particular.

Similarly, a variable defined in `Makefile.am` or
`AC_SUBST`ed from `configure.ac` will override any
definition of the variable that `automake` would ordinarily
create. This feature is often more useful than the ability to
override a rule. Be warned that many of the variables generated by
`automake` are considered to be for internal use only, and their
names might change in future releases.

When examining a variable definition, Automake will recursively examine
variables referenced in the definition. For example, if Automake is
looking at the content of `foo_SOURCES` in this snippet

```
xs = a.c b.c
foo_SOURCES = c.c $(xs)
```

it would use the files `a.c`, `b.c`, and `c.c` as the
contents of `foo_SOURCES`.

Automake also allows a form of comment that is *not* copied into
the output; all lines beginning with ‘`##`’ (leading spaces allowed)
are completely ignored by Automake.

It is customary to make the first line of `Makefile.am` read:

```
## Process this file with automake to produce Makefile.in
```

---

Next: [The Uniform Naming Scheme](#Uniform), Previous: [General Operation](#General-Operation), Up: [General ideas](#Generalities)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 3.2 Strictness [¶](#Strictness-1)

While Automake is intended to be used by maintainers of GNU packages, it
does make some effort to accommodate those who wish to use it, but do
not want to use all the GNU conventions.

To this end, Automake supports three levels of *strictness*—how
stringently Automake should enforce conformance with GNU conventions.
Each strictness level can be selected using an option of the same name;
see [Changing Automake’s Behavior](#Options).

The strictness levels are:

`gnu` [¶](#index-Strictness_002c-gnu)
:   This is the default level of strictness. Automake will check for
    basic compliance with the GNU standards for software packaging.
    See [The GNU Coding Standards](https://www.gnu.org/prep/standards/standards.html#Top), for full details
    of these standards. Currently the following checks are made:

    * The files `INSTALL`, `NEWS`, `README`, `AUTHORS`,
      and `ChangeLog`, plus a license file, are required at the topmost
      directory of the package. Any one of `COPYING`, `COPYING.LIB`,
      `COPYING.LESSER`, `COPYINGv2`, `COPYINGv3`,
      `COPYING.LESSERv2` and `COPYING.LESSERv3` is accepted as the
      license file.

      If the `--add-missing` option is given, `automake` will
      add a generic version of the `INSTALL` file as well as the
      `COPYING` file containing the text of the current version of the
      GNU General Public License existing at the time of this Automake release
      (version 3 as this is written,
      <https://www.gnu.org/copyleft/gpl.html>).
      However, `automake` never overwrites any of the license files
      listed above, and adds no `COPYING` file when one of them is
      already present.
    * The options `no-installman` and `no-installinfo` are
      prohibited.

    Future versions of Automake will add more checks at this level of
    strictness; it is advisable to be familiar with the precise requirements
    of the GNU standards.

    Future versions of Automake may, at this level of strictness, require
    certain non-standard GNU tools to be available to maintainer-only
    `Makefile` rules. For instance, in the future `pathchk`
    (see [pathchk invocation](https://www.gnu.org/software/coreutils/manual/coreutils.html#pathchk-invocation) in GNU Coreutils)
    may be required to run ‘`make dist`’.

`foreign` [¶](#index-Strictness_002c-foreign)
:   Automake will check for only those things that are absolutely
    required for proper operation. For instance, whereas GNU standards
    dictate the existence of a `NEWS` file, it will not be required in
    this mode. This strictness will also turn off some warnings by default
    (among them, portability warnings).

`gnits` [¶](#index-Strictness_002c-gnits)
:   Automake will check for compliance to the as-yet-unwritten *Gnits
    standards*. These are based on the GNU standards, but are even more
    detailed. Unless you are a Gnits standards contributor, it is
    recommended that you avoid this option until such time as the Gnits
    standard is published (which is unlikely to ever happen).

    Currently, `--gnits` does all the checks that
    `--gnu` does, and checks the following as well:

    * ‘`make installcheck`’ will check to make sure that the
      `--help` and `--version` print a usage message and a
      version string, respectively. This is the `std-options` option
      (see [Changing Automake’s Behavior](#Options)).
    * ‘`make dist`’ will check to make sure the `NEWS` file has been
      updated to the current version.
    * `VERSION` is checked to make sure its format complies with Gnits
      standards.
    * If `VERSION` indicates that this is an alpha release, and the file
      `README-alpha` appears in the topmost directory of a package, then
      it is included in the distribution. This is done in `--gnits`
      mode, and no other, because this mode is the only one where version
      number formats are constrained, and hence the only mode where Automake
      can automatically determine whether `README-alpha` should be
      included.
    * The file `THANKS` is required.

---

Next: [Staying below the command line length limit](#Length-Limitations), Previous: [Strictness](#Strictness), Up: [General ideas](#Generalities)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
