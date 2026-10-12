### C.2 Making Additions to `gawk` [¶](#Making-Additions-to-gawk)

If you find that you want to enhance `gawk` in a significant
fashion, you are perfectly free to do so. That is the point of having
free software; the source code is available and you are free to change
it as you want (see [GNU General Public License](#Copying)).

This section discusses the ways you might want to change `gawk`
as well as any considerations you should bear in mind.

* [Accessing The `gawk` Git Repository](#Accessing-The-Source)
* [Adding New Features](#Adding-Code)
* [Porting `gawk` to a New Operating System](#New-Ports)
* [Why Generated Files Are Kept In Git](#Derived-Files)

---

Next: [Adding New Features](#Adding-Code), Up: [Making Additions to `gawk`](#Additions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.2.1 Accessing The `gawk` Git Repository [¶](#Accessing-The-gawk-Git-Repository)

As `gawk` is Free Software, the source code is always available.
[The `gawk` Distribution](#Gawk-Distribution) describes how to get and build the formal,
released versions of `gawk`.

However, if you want to modify `gawk` and contribute back your
changes, you will probably wish to work with the development version.
To do so, you will need to access the `gawk` source code
repository. The code is maintained using the
[Git distributed version control system](https://git-scm.com).
You will need to install it if your system doesn’t have it.
Once you have done so, use the command:

```
git clone git://git.savannah.gnu.org/gawk.git
```

This clones the `gawk` repository. If you are behind a
firewall that does not allow you to use the Git native protocol, you
can still access the repository using:

```
git clone https://git.savannah.gnu.org/git/gawk.git
```

(Using the `https` URL is considered to be more secure.)

Once you have made changes, you can use ‘`git diff`’ to produce a
patch, and send that to the `gawk` maintainer; see [Reporting Problems and Bugs](#Bugs),
for how to do that.

Once upon a time there was Git–CVS gateway for use by people who could
not install Git. However, this gateway no longer works, so you may have
better luck using a more modern version control system like Bazaar,
that has a Git plug-in for working with Git repositories.

---

Next: [Porting `gawk` to a New Operating System](#New-Ports), Previous: [Accessing The `gawk` Git Repository](#Accessing-The-Source), Up: [Making Additions to `gawk`](#Additions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.2.2 Adding New Features [¶](#Adding-New-Features)

You are free to add any new features you like to `gawk`.
However, if you want your changes to be incorporated into the `gawk`
distribution, there are several steps that you need to take in order to
make it possible to include them:

1. Discuss the proposed new feature with the `gawk` maintainer.
   The bug list may be used for this. Even if I don’t
   wish to include your feature, be aware that you are still free to
   add it and distribute your own “fork” of `gawk`.
2. Before building the new feature into `gawk` itself,
   consider writing it as an extension
   (see [Writing Extensions for `gawk`](#Dynamic-Extensions)).
   If that’s not possible, continue with the rest of the steps in this list.
3. Be prepared to sign the appropriate paperwork.
   In order for the FSF to distribute your changes, you must either place
   those changes in the public domain and submit a signed statement to that
   effect, or assign the copyright in your changes to the FSF.
   Both of these actions are easy to do and *many* people have done so
   already. If you have questions, please contact me
   (see [Reporting Problems and Bugs](#Bugs)),
   or “assign at gnu dot org”.
4. Get the latest version.
   It is much easier for me to integrate changes if they are relative to
   the most recent distributed version of `gawk`, or better yet,
   relative to the latest code in the Git repository. If your version of
   `gawk` is very old, I may not be able to integrate your changes at all.
   (See [Getting the `gawk` Distribution](#Getting),
   for information on getting the latest version of `gawk`.)
5. Follow the GNU Coding Standards.
   This document describes how GNU software should be written. If you haven’t
   read it, please do so, preferably *before* starting to modify `gawk`.
   (The GNU Coding Standards are available from
   the GNU Project’s
   [website](https://www.gnu.org/prep/standards/).
   Texinfo, Info, and DVI versions are also available.)
6. Use the `gawk` coding style.
   The C code for `gawk` follows the instructions in the
   GNU Coding Standards, with minor exceptions. The code is formatted
   using the traditional “K&R” style, particularly as regards to the placement
   of braces and the use of TABs. In brief, the coding rules for `gawk`
   are as follows:
   * Use ANSI/ISO style (prototype) function headers when defining functions.
   * Put the name of the function at the beginning of its own line.
   * Use ‘`#elif`’ instead of nesting ‘`#if`’ inside ‘`#else`’.
   * Put the return type of the function, even if it is `int`, on the
     line above the line with the name and arguments of the function.
   * Put spaces around parentheses used in control structures
     (`if`, `while`, `for`, `do`, and `switch`).
   * Do not parenthesize the expression used with `return`.
   * Do not put spaces in front of parentheses used in function calls.
   * Put spaces around all C operators and after commas in function calls.
   * Do not use the comma operator to produce multiple side effects, except
     in `for` loop initialization and increment parts, and in macro bodies.
   * Use real TABs for indenting, not spaces.
   * Use the “K&R” brace layout style.
   * Use comparisons against `NULL` and `'\0'` in the conditions of
     `if`, `while`, and `for` statements, as well as in the `case`s
     of `switch` statements, instead of just the
     plain pointer or character value.
   * Do not, *under any circumstances*, use the ‘`-1 == foo`’ or
     ‘`0 >= bar`’ style of comparison expressions. I have known about
     it for decades, and I understand why some people like it. Nonetheless,
     I abhor it with a passion, and code that uses it will never be accepted.
   * Use `true` and `false` for `bool` values,
     the `NULL` symbolic constant for pointer values,
     and the character constant `'\0'` where appropriate, instead of `1`
     and `0`.
   * Provide one-line descriptive comments for each function.
   * Do not use the `alloca()` function for allocating memory off the
     stack. Its use causes more portability trouble than is worth the minor
     benefit of not having to free the storage. Instead, use `malloc()`
     and `free()`.
   * Do not use comparisons of the form ‘`! strcmp(a, b)`’ or similar.
     As Henry Spencer once said, “`strcmp()` is not a boolean!”
     Instead, use ‘`strcmp(a, b) == 0`’.
   * If adding new bit flag values, use explicit hexadecimal constants
     (`0x001`, `0x002`, `0x004`, and so on) instead of
     shifting one left by successive amounts (‘`(1<<0)`’, ‘`(1<<1)`’,
     and so on).
   > **NOTE:** If I have to reformat your code to follow the coding style used in
   > `gawk`, I may not bother to integrate your changes at all.
7. Update the documentation.
   Along with your new code, please supply new sections and/or chapters
   for this Web page. If at all possible, please use real
   Texinfo, instead of just supplying unformatted ASCII text (although
   even that is better than no documentation at all).
   Conventions to be followed in GAWK: Effective AWK Programming are provided
   after the ‘`@bye`’ at the end of the Texinfo source file.
   If possible, please update the `man` page as well.

   You will also have to sign paperwork for your documentation changes.
8. Submit changes as unified diffs.
   Use ‘`diff -u -r -N`’ to compare
   the original `gawk` source tree with your version.
   I recommend using the GNU version of `diff`, or best of all,
   ‘`git diff`’ or ‘`git format-patch`’.
   Send the output produced by `diff` to me when you
   submit your changes.
   (See [Reporting Problems and Bugs](#Bugs), for the electronic mail
   information.)

   Using this format makes it easy for me to apply your changes to the
   master version of the `gawk` source code (using `patch`).
   If I have to apply the changes manually, using a text editor, I may
   not do so, particularly if there are lots of changes.
9. Include an entry for the `ChangeLog` file with your submission.
   This helps further minimize the amount of work I have to do,
   making it easier for me to accept patches.
   It is simplest if you just make this part of your diff.

Although this sounds like a lot of work, please remember that while you
may write the new code, I have to maintain it and support it. If it
isn’t possible for me to do that with a minimum of extra work, then I
probably will not.

---

Next: [Why Generated Files Are Kept In Git](#Derived-Files), Previous: [Adding New Features](#Adding-Code), Up: [Making Additions to `gawk`](#Additions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.2.3 Porting `gawk` to a New Operating System [¶](#Porting-gawk-to-a-New-Operating-System)

If you want to port `gawk` to a new operating system, there are
several steps:

1. Follow the guidelines in
   the previous section
   concerning coding style, submission of diffs, and so on.
2. Be prepared to sign the appropriate paperwork.
   In order for the FSF to distribute your code, you must either place
   your code in the public domain and submit a signed statement to that
   effect, or assign the copyright in your code to the FSF.
   Both of these actions are easy to do and *many* people have done so
   already. If you have questions, please contact me, or
   “gnu at gnu dot org”.
3. When doing a port, bear in mind that your code must coexist peacefully
   with the rest of `gawk` and the other ports. Avoid gratuitous
   changes to the system-independent parts of the code. If at all possible,
   avoid sprinkling ‘`#ifdef`’s just for your port throughout the
   code.

   If the changes needed for a particular system affect too much of the
   code, I probably will not accept them. In such a case, you can, of course,
   distribute your changes on your own, as long as you comply
   with the GPL
   (see [GNU General Public License](#Copying)).
4. A number of the files that come with `gawk` are maintained by other
   people. Thus, you should not change them
   unless it is for a very good reason; i.e., changes are not out of the
   question, but changes to these files are scrutinized extra carefully.
   These are all the files in the `support` directory
   within the `gawk` distribution. See there.
5. A number of other files are provided by the GNU
   Autotools (Autoconf, Automake, and GNU `gettext`).
   You should not change them either, unless it is for a very
   good reason. The files are
   `ABOUT-NLS`,
   `INSTALL`,
   and all the files in the `build-aux` directory.
6. Be willing to continue to maintain the port.
   Non-Unix operating systems are supported by volunteers who maintain
   the code needed to compile and run `gawk` on their systems. If no-one
   volunteers to maintain a port, it becomes unsupported and it may
   be necessary to remove it from the distribution.
7. Supply an appropriate `gawkmisc.???` file.
   Each port has its own `gawkmisc.???` that implements certain
   operating system specific functions. This is cleaner than a plethora of
   ‘`#ifdef`’s scattered throughout the code. The `gawkmisc.c` in
   the main source directory includes the appropriate
   `gawkmisc.???` file from each subdirectory.
   Be sure to update it as well.

   Each port’s `gawkmisc.???` file has a suffix reminiscent of the machine
   or operating system for the port—for example, `pc/gawkmisc.pc` and
   `vms/gawkmisc.vms`. The use of separate suffixes, instead of plain
   `gawkmisc.c`, makes it possible to move files from a port’s subdirectory
   into the main subdirectory, without accidentally destroying the real
   `gawkmisc.c` file. (Currently, this is only an issue for the
   PC operating system ports.)
8. Supply a `Makefile` as well as any other C source and header files that are
   necessary for your operating system. All your code should be in a
   separate subdirectory, with a name that is the same as, or reminiscent
   of, either your operating system or the computer system. If possible,
   try to structure things so that it is not necessary to move files out
   of the subdirectory into the main source directory. If that is not
   possible, then be sure to avoid using names for your files that
   duplicate the names of files in the main source directory.
9. Update the documentation.
   Please write a section (or sections) for this Web page describing the
   installation and compilation steps needed to compile and/or install
   `gawk` for your system.

Following these steps makes it much easier to integrate your changes
into `gawk` and have them coexist happily with other
operating systems’ code that is already there.

In the code that you supply and maintain, feel free to use a
coding style and brace layout that suits your taste.

---

Previous: [Porting `gawk` to a New Operating System](#New-Ports), Up: [Making Additions to `gawk`](#Additions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.2.4 Why Generated Files Are Kept In Git [¶](#Why-Generated-Files-Are-Kept-In-Git)

If you look at the `gawk` source in the Git
repository, you will notice that it includes files that are automatically
generated by GNU infrastructure tools, such as `Makefile.in` from
Automake and even `configure` from Autoconf.

This is different from many Free Software projects that do not store
the derived files, because that keeps the repository less cluttered,
and it is easier to see the substantive changes when comparing versions
and trying to understand what changed between commits.

However, there are several reasons why the `gawk` maintainer
likes to have everything in the repository.

First, because it is then easy to reproduce any given version completely,
without relying upon the availability of (older, likely obsolete, and
maybe even impossible to find) other tools.

As an extreme example, if you ever even think about trying to compile,
oh, say, the V7 `awk`, you will discover that not only do you
have to bootstrap the V7 `yacc` to do so, but you also need the
V7 `lex`. And the latter is pretty much impossible to bring up
on a modern GNU/Linux system.[122](#FOOT122)

(Or, let’s say `gawk` 1.2 required `bison` whatever-it-was
in 1989 and that there was no `awkgram.c` file in the repository. Is
there a guarantee that we could find that `bison` version? Or that
*it* would build?)

If the repository has all the generated files, then it’s easy to just check
them out and build. (Or *easier*, depending upon how far back we go.)

And that brings us to the second (and stronger) reason why all the files
really need to be in Git. It boils down to who do you cater
to—the `gawk` developer(s), or the user who just wants to check
out a version and try it out?

The `gawk` maintainer
wants it to be possible for any interested `awk` user in the
world to just clone the repository, check out the branch of interest and
build it, without their having to have the correct version(s) of the
autotools.[123](#FOOT123)
That is the point of the `bootstrap.sh` file. It touches the
various other files in the right order such that

```
# The canonical incantation for building GNU software:
./bootstrap.sh && ./configure && make
```

will *just work*.

This is extremely important for the `master` and
`gawk-X.Y-stable` branches.

Further, the `gawk` maintainer would argue that it’s also
important for the `gawk` developers. When he tried to check out
the `xgawk` branch[124](#FOOT124) to build it, he
couldn’t. (No `ltmain.sh` file, and he had no idea how to create it,
and that was not the only problem.)

He felt *extremely* frustrated. With respect to that branch,
the maintainer is no different than Jane User who wants to try to build
`gawk-4.1-stable` or `master` from the repository.

Thus, the maintainer thinks that it’s not just important, but critical,
that for any given branch, the above incantation *just works*.

A third reason to have all the files is that without them, using ‘`git
bisect`’ to try to find the commit that introduced a bug is exceedingly
difficult. The maintainer tried to do that on another project that
requires running bootstrapping scripts just to create `configure`
and so on; it was really painful. When the repository is self-contained,
using `git bisect` in it is very easy.

What are some of the consequences and/or actions to take?

1. We don’t mind that there are differing files in the different branches
   as a result of different versions of the autotools.
   1. It’s the maintainer’s job to merge them and he will deal with it.
   2. He is really good at ‘`git diff x y > /tmp/diff1 ; gvim /tmp/diff1`’ to
      remove the diffs that aren’t of interest in order to review code.
2. It would certainly help if everyone used the same versions of the GNU tools
   as he does, which in general are the latest released versions of
   Automake,
   Autoconf,
   `bison`,
   GNU `gettext`,
   and
   Libtool.

   Installing from source is quite easy. It’s how the maintainer worked for years
   (and still works).
   He had `/usr/local/bin` at the front of his `PATH` and just did:

   ```
   wget https://ftp.gnu.org/gnu/package/package-x.y.z.tar.gz
   tar -xpzvf package-x.y.z.tar.gz
   cd package-x.y.z
   ./configure && make && make check
   make install    # as root
   ```

   > **NOTE:** Because of the ‘`https://`’ URL, you may have to supply the
   > `--no-check-certificate` option to `wget` to download
   > the file.

Most of the above was originally written by the maintainer to other
`gawk` developers. It raised the objection from one of
the developers “… that anybody pulling down the source from
Git is not an end user.”

However, this is not true. There are “power `awk` users”
who can build `gawk` (using the magic incantation shown previously)
but who can’t program in C. Thus, the major branches should be
kept buildable all the time.

It was then suggested that there be a `cron` job to create
nightly tarballs of “the source.” Here, the problem is that there
are source trees, corresponding to the various branches! So,
nightly tarballs aren’t the answer, especially as the repository can go
for weeks without significant change being introduced.

Fortunately, the Git server can meet this need. For any given
branch named branchname, use:

```
wget https://git.savannah.gnu.org/cgit/gawk.git/snapshot/gawk-branchname.tar.gz
```

to retrieve a snapshot of the given branch.

---

Next: [Some Limitations of the Implementation](#Implementation-Limitations), Previous: [Making Additions to `gawk`](#Additions), Up: [Implementation Notes](#Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### C.3 Probable Future Extensions [¶](#Probable-Future-Extensions)

> *AWK is a language similar to PERL, only considerably more elegant.*

— *Arnold Robbins*

> *Hey!*

— *Larry Wall*

The `TODO` file in the `master` branch of the `gawk`
Git repository lists possible future enhancements. Some of these relate
to the source code, and others to possible new features. Please see
that file for the list.
See [Making Additions to `gawk`](#Additions),
if you are interested in tackling any of the projects listed there.

---

Next: [Extension API Design](#Extension-Design), Previous: [Probable Future Extensions](#Future-Extensions), Up: [Implementation Notes](#Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### C.4 Some Limitations of the Implementation [¶](#Some-Limitations-of-the-Implementation)

This following table describes limits of `gawk` on a Unix-like
system (although it is variable even then). Other systems may have
different limits.

| Item | Limit |
| --- | --- |
| Characters in a character class | 2^(number of bits per byte) |
| Length of input record in bytes | `ULONG_MAX` |
| Length of output record | Unlimited |
| Length of source line | Unlimited |
| Number of fields in a record | `ULONG_MAX` |
| Number of file redirections | Unlimited |
| Number of input records in one file | `MAX_LONG` |
| Number of input records total | `MAX_LONG` |
| Number of pipe redirections | min(number of processes per user, number of open files) |
| Numeric values | Double-precision floating point (if not using MPFR) |
| Size of a field in bytes | `ULONG_MAX` |
| Size of a literal string in bytes | `ULONG_MAX` |
| Size of a printf string in bytes | `ULONG_MAX` |

---

Next: [Summary](#Notes-summary), Previous: [Some Limitations of the Implementation](#Implementation-Limitations), Up: [Implementation Notes](#Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### C.5 Extension API Design [¶](#Extension-API-Design)

This section documents the design of the extension API,
including a discussion of some of the history and problems that needed
to be solved.

The first version of extensions for `gawk` was developed in
the mid-1990s and released with `gawk` 3.1 in the late 1990s.
The basic mechanisms and design remained unchanged for close to 15 years,
until 2012.

The old extension mechanism used data types and functions from
`gawk` itself, with a “clever hack” to install extension
functions.

`gawk` included some sample extensions, of which a few were
really useful. However, it was clear from the outset that the extension
mechanism was bolted onto the side and was not really well thought out.

* [Problems With The Old Mechanism](#Old-Extension-Problems)
* [Goals For A New Mechanism](#Extension-New-Mechanism-Goals)
* [Other Design Decisions](#Extension-Other-Design-Decisions)
* [Room For Future Growth](#Extension-Future-Growth)

---

Next: [Goals For A New Mechanism](#Extension-New-Mechanism-Goals), Up: [Extension API Design](#Extension-Design)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.5.1 Problems With The Old Mechanism [¶](#Problems-With-The-Old-Mechanism)

The old extension mechanism had several problems:

* It depended heavily upon `gawk` internals. Any time the
  `NODE` structure[125](#FOOT125) changed, an extension would have to be
  recompiled. Furthermore, to really write extensions required understanding
  something about `gawk`’s internal functions. There was some
  documentation in this Web page, but it was quite minimal.
* Being able to call into `gawk` from an extension required linker
  facilities that are common on Unix-derived systems but that did
  not work on MS-Windows systems; users wanting extensions on MS-Windows
  had to statically link them into `gawk`, even though MS-Windows supports
  dynamic loading of shared objects.
* The API would change occasionally as `gawk` changed; no compatibility
  between versions was ever offered or planned for.

Despite the drawbacks, the `xgawk` project developers forked
`gawk` and developed several significant extensions. They also
enhanced `gawk`’s facilities relating to file inclusion and
shared object access.

A new API was desired for a long time, but only in 2012 did the
`gawk` maintainer and the `xgawk` developers finally
start working on it together. More information about the `xgawk`
project is provided in [The `gawkextlib` Project](#gawkextlib).

---

Next: [Other Design Decisions](#Extension-Other-Design-Decisions), Previous: [Problems With The Old Mechanism](#Old-Extension-Problems), Up: [Extension API Design](#Extension-Design)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.5.2 Goals For A New Mechanism [¶](#Goals-For-A-New-Mechanism)

Some goals for the new API were:

* The API should be independent of `gawk` internals. Changes in
  `gawk` internals should not be visible to the writer of an
  extension function.
* The API should provide *binary* compatibility across `gawk`
  releases as long as the API itself does not change.
* The API should enable extensions written in C or C++ to have roughly the
  same “appearance” to `awk`-level code as `awk`
  functions do. This means that extensions should have:
  + The ability to access function parameters.
  + The ability to turn an undefined parameter into an array (call by reference).
  + The ability to create, access and update global variables.
  + Easy access to all the elements of an array at once (“array flattening”)
    in order to loop over all the element in an easy fashion for C code.
  + The ability to create arrays (including `gawk`’s true
    arrays of arrays).

Some additional important goals were:

* The API should use only features in ISO C 90, so that extensions
  can be written using the widest range of C and C++ compilers. The header
  should include the appropriate ‘`#ifdef __cplusplus`’ and ‘`extern "C"`’
  magic so that a C++ compiler could be used. (If using C++, the runtime
  system has to be smart enough to call any constructors and destructors,
  as `gawk` is a C program. As of this writing, this has not been
  tested.)
* The API mechanism should not require access to `gawk`’s
  symbols[126](#FOOT126) by the compile-time or dynamic linker,
  in order to enable creation of extensions that also work on MS-Windows.

During development, it became clear that there were other features
that should be available to extensions, which were also subsequently
provided:

* Extensions should have the ability to hook into `gawk`’s
  I/O redirection mechanism. In particular, the `xgawk`
  developers provided a so-called “open hook” to take over reading
  records. During development, this was generalized to allow
  extensions to hook into input processing, output processing, and
  two-way I/O.
* An extension should be able to provide a “call back” function
  to perform cleanup actions when `gawk` exits.
* An extension should be able to provide a version string so that
  `gawk`’s `--version` option can provide information
  about extensions as well.

The requirement to avoid access to `gawk`’s symbols is, at first
glance, a difficult one to meet.

One design, apparently used by Perl and Ruby and maybe others, would
be to make the mainline `gawk` code into a library, with the
`gawk` utility a small C `main()` function linked against
the library.

This seemed like the tail wagging the dog, complicating build and
installation and making a simple copy of the `gawk` executable
from one system to another (or one place to another on the same
system!) into a chancy operation.

Pat Rankin suggested the solution that was adopted.
See [How It Works at a High Level](#Extension-Mechanism-Outline), for the details.

---

Next: [Room For Future Growth](#Extension-Future-Growth), Previous: [Goals For A New Mechanism](#Extension-New-Mechanism-Goals), Up: [Extension API Design](#Extension-Design)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.5.3 Other Design Decisions [¶](#Other-Design-Decisions)

As an arbitrary design decision, extensions can read the values of
predefined variables and arrays (such as `ARGV` and `FS`), but cannot
change them, with the exception of `PROCINFO`.

The reason for this is to prevent an extension function from affecting
the flow of an `awk` program outside its control. While a real
`awk` function can do what it likes, that is at the discretion
of the programmer. An extension function should provide a service or
make a C API available for use within `awk`, and not mess with
`FS` or `ARGC` and `ARGV`.

In addition, it becomes easy to start down a slippery slope. How
much access to `gawk` facilities do extensions need?
Do they need `getline`? What about calling `gsub()` or
compiling regular expressions? What about calling into `awk`
functions? (*That* would be messy.)

In order to avoid these issues, the `gawk` developers chose
to start with the simplest, most basic features that are still truly useful.

Another decision is that although `gawk` provides nice things like
MPFR, and arrays indexed internally by integers, these features are not
being brought out to the API in order to keep things simple and close to
traditional `awk` semantics. (In fact, arrays indexed internally
by integers are so transparent that they aren’t even documented!)

Additionally, all functions in the API check that their pointer
input parameters are not `NULL`. If they are, they return an error.
(It is a good idea for extension code to verify that
pointers received from `gawk` are not `NULL`.
Such a thing should not happen, but the `gawk` developers
are only human, and they have been known to occasionally make
mistakes.)

With time, the API will undoubtedly evolve; the `gawk` developers
expect this to be driven by user needs. For now, the current API seems
to provide a minimal yet powerful set of features for creating extensions.

---

Previous: [Other Design Decisions](#Extension-Other-Design-Decisions), Up: [Extension API Design](#Extension-Design)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### C.5.4 Room For Future Growth [¶](#Room-For-Future-Growth)

The API can later be expanded, in at least the following way:

* `gawk` passes an “extension id” into the extension when it
  first loads the extension. The extension then passes this id back
  to `gawk` with each function call. This mechanism allows
  `gawk` to identify the extension calling into it, should it need
  to know.

Of course, as of this writing, no decisions have been made with respect
to the above.

---

Previous: [Extension API Design](#Extension-Design), Up: [Implementation Notes](#Notes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### C.6 Summary [¶](#Summary-19)

* `gawk`’s extensions can be disabled with either the
  `--traditional` option or with the `--posix` option.
  The `--parsedebug` option is available if `gawk` is
  compiled with ‘`-DDEBUG`’.
* The source code for `gawk` is maintained in a publicly
  accessible Git repository. Anyone may check it out and view the source.
* Contributions to `gawk` are welcome. Following the steps
  outlined in this chapter will make it easier to integrate
  your contributions into the code base.
  This applies both to new feature contributions and to ports to
  additional operating systems.
* `gawk` has some limits—generally those that are imposed by
  the machine architecture.
* The extension API design was intended to solve a number of problems
  with the previous extension mechanism, enable features needed by
  the `xgawk` project, and provide binary compatibility going forward.
* The previous extension mechanism is no longer supported and was
  removed from the code base with the 4.2 release.

---

Next: [Glossary](#Glossary), Previous: [Implementation Notes](#Notes), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix D Basic Programming Concepts [¶](#Basic-Programming-Concepts)

This appendix attempts to define some of the basic concepts
and terms that are used throughout the rest of this Web page.
As this Web page is specifically about `awk`,
and not about computer programming in general, the coverage here
is by necessity fairly cursory and simplistic.
(If you need more background, there are many
other introductory texts that you should refer to instead.)

* [What a Program Does](#Basic-High-Level)
* [Data Values in a Computer](#Basic-Data-Typing)

---

Next: [Data Values in a Computer](#Basic-Data-Typing), Up: [Basic Programming Concepts](#Basic-Concepts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### D.1 What a Program Does [¶](#What-a-Program-Does)

At the most basic level, the job of a program is to process
some input data and produce results.
See [Figure D.1](#figure_002dgeneral_002dflow).

**Figure D.1:** General Program Flow

The “program” in the figure can be either a compiled
program[127](#FOOT127)
(such as `ls`),
or it may be *interpreted*. In the latter case, a machine-executable
program such as `awk` reads your program, and then uses the
instructions in your program to process the data.

When you write a program, it usually consists
of the following, very basic set of steps,
as shown in [Figure D.2](#figure_002dprocess_002dflow):

**Figure D.2:** Basic Program Steps

Initialization
:   These are the things you do before actually starting to process
    data, such as checking arguments, initializing any data you need
    to work with, and so on.
    This step corresponds to `awk`’s `BEGIN` rule
    (see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)).

    If you were baking a cake, this might consist of laying out all the
    mixing bowls and the baking pan, and making sure you have all the
    ingredients that you need.

Processing
:   This is where the actual work is done. Your program reads data,
    one logical chunk at a time, and processes it as appropriate.

    In most programming languages, you have to manually manage the reading
    of data, checking to see if there is more each time you read a chunk.
    `awk`’s pattern-action paradigm
    (see [Getting Started with `awk`](#Getting-Started))
    handles the mechanics of this for you.

    In baking a cake, the processing corresponds to the actual labor:
    breaking eggs, mixing the flour, water, and other ingredients, and then putting the cake
    into the oven.

Clean Up
:   Once you’ve processed all the data, you may have things you need to
    do before exiting.
    This step corresponds to `awk`’s `END` rule
    (see [The `BEGIN` and `END` Special Patterns](#BEGIN_002fEND)).

    After the cake comes out of the oven, you still have to wrap it in
    plastic wrap to keep anyone from tasting it, as well as wash
    the mixing bowls and utensils.

An *algorithm* is a detailed set of instructions necessary to accomplish
a task, or process data. It is much the same as a recipe for baking
a cake. Programs implement algorithms. Often, it is up to you to design
the algorithm and implement it, simultaneously.

The “logical chunks” we talked about previously are called *records*,
similar to the records a company keeps on employees, a school keeps for
students, or a doctor keeps for patients.
Each record has many component parts, such as first and last names,
date of birth, address, and so on. The component parts are referred
to as the *fields* of the record.

The act of reading data is termed *input*, and that of
generating results, not too surprisingly, is termed *output*.
They are often referred to together as “input/output,”
and even more often, as “I/O” for short.
(You will also see “input” and “output” used as verbs.)

`awk` manages the reading of data for you, as well as the
breaking it up into records and fields. Your program’s job is to
tell `awk` what to do with the data. You do this by describing
*patterns* in the data to look for, and *actions* to execute
when those patterns are seen. This *data-driven* nature of
`awk` programs usually makes them both easier to write
and easier to read.

---

Previous: [What a Program Does](#Basic-High-Level), Up: [Basic Programming Concepts](#Basic-Concepts)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### D.2 Data Values in a Computer [¶](#Data-Values-in-a-Computer)

In a program,
you keep track of information and values in things called *variables*.
A variable is just a name for a given value, such as `first_name`,
`last_name`, `address`, and so on.
`awk` has several predefined variables, and it has
special names to refer to the current input record
and the fields of the record.
You may also group multiple
associated values under one name, as an array.

Data, particularly in `awk`, consists of either numeric
values, such as 42 or 3.1415927, or string values.
String values are essentially anything that’s not a number, such as a name.
Strings are sometimes referred to as *character data*, since they
store the individual characters that comprise them.
Individual variables, as well as numeric and string variables, are
referred to as *scalar* values.
Groups of values, such as arrays, are not scalars.

[A General Description of Computer Arithmetic](#Computer-Arithmetic), provided a basic introduction to numeric
types (integer and floating-point) and how they are used in a computer.
Please review that information, including a number of caveats that
were presented.

While you are probably used to the idea of a number without a value (i.e., zero),
it takes a bit more getting used to the idea of zero-length character data.
Nevertheless, such a thing exists.
It is called the *null string*.
The null string is character data that has no value.
In other words, it is empty. It is written in `awk` programs
like this: `""`.

Humans are used to working in decimal; i.e., base 10. In base 10,
numbers go from 0 to 9, and then “roll over” into the next
column. (Remember grade school? 42 = 4 x 10 + 2.)

There are other number bases though. Computers commonly use base 2
or *binary*, base 8 or *octal*, and base 16 or *hexadecimal*.
In binary, each column represents two times the value in the column to
its right. Each column may contain either a 0 or a 1.
Thus, binary 1010 represents (1 x 8) + (0 x 4) + (1 x 2)
+ (0 x 1), or decimal 10.
Octal and hexadecimal are discussed more in
[Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers).

At the very lowest level, computers store values as groups of binary digits,
or *bits*. Modern computers group bits into groups of eight, called *bytes*.
Advanced applications sometimes have to manipulate bits directly,
and `gawk` provides functions for doing so.

Programs are written in programming languages.
Hundreds, if not thousands, of programming languages exist.
One of the most popular is the C programming language.
The C language had a very strong influence on the design of
the `awk` language.

There have been several versions of C. The first is often referred to
as “K&R” C, after the initials of Brian Kernighan and Dennis Ritchie,
the authors of the first book on C. (Dennis Ritchie created the language,
and Brian Kernighan was one of the creators of `awk`.)

In the mid-1980s, an effort began to produce an international standard
for C. This work culminated in 1989, with the production of the ANSI
standard for C. This standard became an ISO standard in 1990.
In 1999, a revised ISO C standard was approved and released.
Where it makes sense, POSIX `awk` is compatible with 1999 ISO C.

---

Next: [GNU General Public License](#Copying), Previous: [Basic Programming Concepts](#Basic-Concepts), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Glossary [¶](#Glossary-1)

Action
:   A series of `awk` statements attached to a rule. If the rule’s
    pattern matches an input record, `awk` executes the
    rule’s action. Actions are always enclosed in braces.
    (See [Actions](#Action-Overview).)

Ada
:   A programming language originally defined by the U.S. Department of
    Defense for embedded programming. It was designed to enforce good
    Software Engineering practices.

Amazing `awk` Assembler [¶](#index-Spencer_002c-Henry)
:   Henry Spencer at the University of Toronto wrote a retargetable assembler
    completely as `sed` and `awk` scripts. It is thousands
    of lines long, including machine descriptions for several eight-bit
    microcomputers. It is a good example of a program that would have been
    better written in another language.

Amazingly Workable Formatter (`awf`) [¶](#index-amazingly-workable-formatter-_0028awf_0029)
:   Henry Spencer at the University of Toronto wrote a formatter that accepts
    a large subset of the ‘`nroff -ms`’ and ‘`nroff -man`’ formatting
    commands, using `awk` and `sh`.

Anchor
:   The regexp metacharacters ‘`^`’ and ‘`$`’, which force the match
    to the beginning or end of the string, respectively.

ANSI [¶](#index-ANSI)
:   The American National Standards Institute. This organization produces
    many standards, among them the standards for the C and C++ programming
    languages.
    These standards often become international standards as well. See also
    “ISO.”

Argument
:   An argument can be two different things. It can be an option or a
    file name passed to a command while invoking it from the command line, or
    it can be something passed to a *function* inside a program, e.g.
    inside `awk`.

    In the latter case, an argument can be passed to a function in two ways.
    Either it is given to the called function by value, i.e., a copy of the
    value of the variable is made available to the called function, but the
    original variable cannot be modified by the function itself; or it is
    given by reference, i.e., a pointer to the interested variable is passed to
    the function, which can then directly modify it. In `awk`
    scalars are passed by value, and arrays are passed by reference.
    See “Pass By Value/Reference.”

Array
:   A grouping of multiple values under the same name.
    Most languages just provide sequential arrays.
    `awk` provides associative arrays.

Assertion
:   A statement in a program that a condition is true at this point in the program.
    Useful for reasoning about how a program is supposed to behave.

Assignment
:   An `awk` expression that changes the value of some `awk`
    variable or data object. An object that you can assign to is called an
    *lvalue*. The assigned values are called *rvalues*.
    See [Assignment Expressions](#Assignment-Ops).

Associative Array
:   Arrays in which the indices may be numbers or strings, not just
    sequential integers in a fixed range.

`awk` Language
:   The language in which `awk` programs are written.

`awk` Program
:   An `awk` program consists of a series of *patterns* and
    *actions*, collectively known as *rules*. For each input record
    given to the program, the program’s rules are all processed in turn.
    `awk` programs may also contain function definitions.

`awk` Script
:   Another name for an `awk` program.

Bash
:   The GNU version of the standard shell
    (the **B**ourne-**A**gain **SH**ell).
    See also “Bourne Shell.”

Binary
:   Base-two notation, where the digits are `0`–`1`. Since
    electronic circuitry works “naturally” in base 2 (just think of Off/On),
    everything inside a computer is calculated using base 2. Each digit
    represents the presence (or absence) of a power of 2 and is called a
    *bit*. So, for example, the base-two number `10101` is
    the same as decimal 21, ((1 x 16) + (1 x 4) + (1 x 1)).

    Since base-two numbers quickly become
    very long to read and write, they are usually grouped by 3 (i.e., they are
    read as octal numbers), or by 4 (i.e., they are read as hexadecimal
    numbers). There is no direct way to insert base 2 numbers in a C program.
    If need arises, such numbers are usually inserted as octal or hexadecimal
    numbers. The number of base-two digits that fit into registers used for
    representing integer numbers in computers is a rough indication of the
    computing power of the computer itself. Most computers nowadays use 64
    bits for representing integer numbers in their registers, but 32-bit,
    16-bit and 8-bit registers have been widely used in the past.
    See [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers).

Bit
:   Short for “Binary Digit.”
    All values in computer memory ultimately reduce to binary digits: values
    that are either zero or one.
    Groups of bits may be interpreted differently—as integers,
    floating-point numbers, character data, addresses of other
    memory objects, or other data.
    `awk` lets you work with floating-point numbers and strings.
    `gawk` lets you manipulate bit values with the built-in
    functions described in
    [Bit-Manipulation Functions](#Bitwise-Functions).

    Computers are often defined by how many bits they use to represent integer
    values. Typical systems are 32-bit systems, but 64-bit systems are
    becoming increasingly popular, and 16-bit systems have essentially
    disappeared.

Boolean Expression
:   Named after the English mathematician Boole. See also “Logical Expression.”

Bourne Shell
:   The standard shell (`/bin/sh`) on Unix and Unix-like systems,
    originally written by Steven R. Bourne at Bell Laboratories.
    Many shells (Bash, `ksh`, `pdksh`, `zsh`) are
    generally upwardly compatible with the Bourne shell.

Braces
:   The characters ‘`{`’ and ‘`}`’. Braces are used in
    `awk` for delimiting actions, compound statements, and function
    bodies.

Bracket Expression
:   Inside a *regular expression*, an expression included in square
    brackets, meant to designate a single character as belonging to a
    specified character class. A bracket expression can contain a list of one
    or more characters, like ‘`[abc]`’, a range of characters, like
    ‘`[A-Z]`’, or a name, delimited by ‘`:`’, that designates a known set
    of characters, like ‘`[:digit:]`’. The form of bracket expression
    enclosed between ‘`:`’ is independent of the underlying representation
    of the character themselves, which could utilize the ASCII, EBCDIC, or
    Unicode codesets, depending on the architecture of the computer system, and on
    localization.
    See also “Regular Expression.”

Built-in Function
:   The `awk` language provides built-in functions that perform various
    numerical, I/O-related, and string computations. Examples are
    `sqrt()` (for the square root of a number) and `substr()` (for a
    substring of a string).
    `gawk` provides functions for timestamp management, bit manipulation,
    array sorting, type checking,
    and runtime string translation.
    (See [Built-in Functions](#Built_002din).)

Built-in Variable
:   `ARGC`,
    `ARGV`,
    `CONVFMT`,
    `ENVIRON`,
    `FILENAME`,
    `FNR`,
    `FS`,
    `NF`,
    `NR`,
    `OFMT`,
    `OFS`,
    `ORS`,
    `RLENGTH`,
    `RSTART`,
    `RS`,
    and
    `SUBSEP`
    are the variables that have special meaning to `awk`.
    In addition,
    `ARGIND`,
    `BINMODE`,
    `ERRNO`,
    `FIELDWIDTHS`,
    `FPAT`,
    `IGNORECASE`,
    `LINT`,
    `PROCINFO`,
    `RT`,
    and
    `TEXTDOMAIN`
    are the variables that have special meaning to `gawk`.
    Changing some of them affects `awk`’s running environment.
    (See [Predefined Variables](#Built_002din-Variables).)

C
:   The system programming language that most GNU software is written in. The
    `awk` programming language has C-like syntax, and this Web page
    points out similarities between `awk` and C when appropriate.

    In general, `gawk` attempts to be as similar to the 1990 version
    of ISO C as makes sense.

c.e.
:   See “Common Extension.”

C Shell
:   The C Shell (`csh` or its improved version, `tcsh`) is a Unix shell that was
    created by Bill Joy in the late 1970s. The C shell was differentiated from
    other shells by its interactive features and overall style, which
    looks more like C. The C Shell is not backward compatible with the Bourne
    Shell, so special attention is required when converting scripts
    written for other Unix shells to the C shell, especially with regard to the management of
    shell variables.
    See also “Bourne Shell.”

C++
:   A popular object-oriented programming language derived from C.

Character Class
:   See “Bracket Expression.”

Character List
:   See “Bracket Expression.”

Character Set [¶](#index-character-sets-_0028machine-character-encodings_0029-1)
:   The set of numeric codes used by a computer system to represent the
    characters (letters, numbers, punctuation, etc.) of a particular country
    or place. The most common character set in use today is ASCII (American
    Standard Code for Information Interchange). Many European
    countries use an extension of ASCII known as ISO-8859-1 (ISO Latin-1).
    The [Unicode character set](https://www.unicode.org) is
    increasingly popular and standard, and is particularly
    widely used on GNU/Linux systems.

CHEM [¶](#index-Kernighan_002c-Brian-7)
:   A preprocessor for `pic` that reads descriptions of molecules
    and produces `pic` input for drawing them.
    It was written in `awk`
    by Brian Kernighan and Jon Bentley, and is available from
    <https://netlib.org/typesetting/chem>.

Comparison Expression
:   A relation that is either true or false, such as ‘`a < b`’.
    Comparison expressions are used in `if`, `while`, `do`,
    and `for`
    statements, and in patterns to select which input records to process.
    (See [Variable Typing and Comparison Expressions](#Typing-and-Comparison).)

Compiler [¶](#index-compiled-programs-1)
:   A program that translates human-readable source code into
    machine-executable object code. The object code is then executed
    directly by the computer.
    See also “Interpreter.”

Common Extension
:   An extension to the `awk` language found in two or more
    `awk` implementations.

Complemented Bracket Expression
:   The negation of a *bracket expression*. All that is *not*
    described by a given bracket expression. The symbol ‘`^`’ precedes
    the negated bracket expression. E.g.: ‘`[^[:digit:]]`’
    designates whatever character is not a digit. ‘`[^bad]`’
    designates whatever character is not one of the letters ‘`b`’, ‘`a`’,
    or ‘`d`’.
    See “Bracket Expression.”

Compound Statement
:   A series of `awk` statements, enclosed in curly braces. Compound
    statements may be nested.
    (See [Control Statements in Actions](#Statements).)

Computed Regexps
:   See “Dynamic Regular Expressions.”

Concatenation
:   Concatenating two strings means sticking them together, one after another,
    producing a new string. For example, the string ‘`foo`’ concatenated with
    the string ‘`bar`’ gives the string ‘`foobar`’.
    (See [String Concatenation](#Concatenation).)

Conditional Expression
:   An expression using the ‘`?:`’ ternary operator, such as
    ‘`expr1 ? expr2 : expr3`’. The expression
    expr1 is evaluated; if the result is true, the value of the whole
    expression is the value of expr2; otherwise the value is
    expr3. In either case, only one of expr2 and expr3
    is evaluated. (See [Conditional Expressions](#Conditional-Exp).)

Control Statement
:   A control statement is an instruction to perform a given operation or a set
    of operations inside an `awk` program, if a given condition is
    true. Control statements are: `if`, `for`, `while`, and
    `do`
    (see [Control Statements in Actions](#Statements)).

Cookie [¶](#index-McIlroy_002c-Doug)
:   A peculiar goodie, token, saying or remembrance
    produced by or presented to a program. (With thanks to Professor Doug McIlroy.)

Coprocess
:   A subordinate program with which two-way communications is possible.

Curly Braces
:   See “Braces.”

Dark Corner
:   An area in the language where specifications often were (or still
    are) not clear, leading to unexpected or undesirable behavior.
    Such areas are marked in this Web page with
    “(d.c.)” in the text
    and are indexed under the heading “dark corner.”

Data Driven
:   A description of `awk` programs, where you specify the data you
    are interested in processing, and what to do when that data is seen.

Data Objects
:   These are numbers and strings of characters. Numbers are converted into
    strings and vice versa, as needed.
    (See [Conversion of Strings and Numbers](#Conversion).)

d.c.
:   See “Dark Corner.”

Deadlock
:   The situation in which two communicating processes are each waiting
    for the other to perform an action.

Debugger
:   A program used to help developers remove “bugs” from (de-bug)
    their programs.

Double Precision
:   An internal representation of numbers that can have fractional parts.
    Double precision numbers keep track of more digits than do single precision
    numbers, but operations on them are sometimes more expensive. This is the way
    `awk` stores numeric values. It is the C type `double`.

Dynamic Regular Expression
:   A dynamic regular expression is a regular expression written as an
    ordinary expression. It could be a string constant, such as
    `"foo"`, but it may also be an expression whose value can vary.
    (See [Using Dynamic Regexps](#Computed-Regexps).)

Empty String
:   See “Null String.”

Environment [¶](#index-environment_002c-definition-of)
:   A collection of strings, of the form ‘`name=val`’, that each
    program has available to it. Users generally place values into the
    environment in order to provide information to various programs. Typical
    examples are the environment variables `HOME` and `PATH`.

Epoch [¶](#index-epoch_002c-definition-of)
:   The date used as the “beginning of time” for timestamps.
    Time values in most systems are represented as seconds since the epoch,
    with library functions available for converting these values into
    standard date and time formats.

    The epoch on Unix and POSIX systems is 1970-01-01 00:00:00 UTC.
    See also “GMT” and “UTC.”

Escape Sequences [¶](#index-ASCII-10)
:   A special sequence of characters used for describing nonprinting
    characters, such as ‘`\n`’ for newline or ‘`\033`’ for the ASCII
    ESC (Escape) character. (See [Escape Sequences](#Escape-Sequences).)

Extension
:   An additional feature or change to a programming language or
    utility not defined by that language’s or utility’s standard.
    `gawk` has (too) many extensions over POSIX `awk`.

FDL
:   See “Free Documentation License.”

Field
:   When `awk` reads an input record, it splits the record into pieces
    separated by whitespace (or by a separator regexp that you can
    change by setting the predefined variable `FS`). Such pieces are
    called fields. If the pieces are of fixed length, you can use the built-in
    variable `FIELDWIDTHS` to describe their lengths.
    If you wish to specify the contents of fields instead of the field
    separator, you can use the predefined variable `FPAT` to do so.
    (See [Specifying How Fields Are Separated](#Field-Separators),
    [Reading Fixed-Width Data](#Constant-Size),
    and
    [Defining Fields by Content](#Splitting-By-Content).)

Flag
:   A variable whose truth value indicates the existence or nonexistence
    of some condition.

Floating-Point Number
:   Often referred to in mathematical terms as a “rational” or real number,
    this is just a number that can have a fractional part.
    See also “Double Precision” and “Single Precision.”

Format
:   Format strings control the appearance of output in the
    `strftime()` and `sprintf()` functions, and in the
    `printf` statement as well. Also, data conversions from numbers to strings
    are controlled by the format strings contained in the predefined variables
    `CONVFMT` and `OFMT`. (See [Format-Control Letters](#Control-Letters).)

Fortran
:   Shorthand for FORmula TRANslator, one of the first programming languages
    available for scientific calculations. It was created by John Backus,
    and has been available since 1957. It is still in use today.

Free Documentation License
:   This document describes the terms under which this Web page
    is published and may be copied. (See [GNU Free Documentation License](#GNU-Free-Documentation-License).)

Free Software Foundation [¶](#index-FSF-_0028Free-Software-Foundation_0029-2)
:   A nonprofit organization dedicated
    to the production and distribution of freely distributable software.
    It was founded by Richard M. Stallman, the author of the original
    Emacs editor. GNU Emacs is the most widely used version of Emacs today.

FSF
:   See “Free Software Foundation.”

Function [¶](#index-_0040include-directive-2)
:   A part of an `awk` program that can be invoked from every point of
    the program, to perform a task. `awk` has several built-in
    functions.
    Users can define their own functions in every part of the program.
    Function can be recursive, i.e., they may invoke themselves.
    See [Functions](#Functions).
    In `gawk` it is also possible to have functions shared
    among different programs, and included where required using the
    `@include` directive
    (see [Including Other Files into Your Program](#Include-Files)).
    In `gawk` the name of the function that should be invoked
    can be generated at run time, i.e., dynamically.
    The `gawk` extension API provides constructor functions
    (see [Constructor Functions](#Constructor-Functions)).

`gawk`
:   The GNU implementation of `awk`.

General Public License [¶](#index-GPL-_0028General-Public-License_0029-1)
:   This document describes the terms under which `gawk` and its source
    code may be distributed. (See [GNU General Public License](#Copying).)

GMT
:   “Greenwich Mean Time.”
    This is the old term for UTC.
    It is the time of day used internally for Unix and POSIX systems.
    See also “Epoch” and “UTC.”

GNU [¶](#index-FSF-_0028Free-Software-Foundation_0029-3)
:   “GNU’s not Unix”. An on-going project of the Free Software Foundation
    to create a complete, freely distributable, POSIX-compliant computing
    environment.

GNU/Linux
:   A variant of the GNU system using the Linux kernel, instead of the
    Free Software Foundation’s Hurd kernel.
    The Linux kernel is a stable, efficient, full-featured clone of Unix that has
    been ported to a variety of architectures.
    It is most popular on PC-class systems, but runs well on a variety of
    other systems too.
    The Linux kernel source code is available under the terms of the GNU General
    Public License, which is perhaps its most important aspect.

GPL
:   See “General Public License.”

Hexadecimal
:   Base 16 notation, where the digits are `0`–`9` and
    `A`–`F`, with ‘`A`’
    representing 10, ‘`B`’ representing 11, and so on, up to ‘`F`’ for 15.
    Hexadecimal numbers are written in C using a leading ‘`0x`’,
    to indicate their base. Thus, `0x12` is 18 ((1 x 16) + 2).
    See [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers).

I/O
:   Abbreviation for “Input/Output,” the act of moving data into and/or
    out of a running program.

Input Record
:   A single chunk of data that is read in by `awk`. Usually, an `awk` input
    record consists of one line of text.
    (See [How Input Is Split into Records](#Records).)

Integer
:   A whole number, i.e., a number that does not have a fractional part.

Internationalization
:   The process of writing or modifying a program so
    that it can use multiple languages without requiring
    further source code changes.

Interpreter [¶](#index-interpreted-programs-1)
:   A program that reads human-readable source code directly, and uses
    the instructions in it to process data and produce results.
    `awk` is typically (but not always) implemented as an interpreter.
    See also “Compiler.”

Interval Expression
:   A component of a regular expression that lets you specify repeated matches of
    some part of the regexp. Interval expressions were not originally available
    in `awk` programs.

ISO [¶](#index-ISO)
:   The International Organization for Standardization.
    This organization produces international standards for many things, including
    programming languages, such as C and C++.
    In the computer arena, important standards like those for C, C++, and POSIX
    become both American national and ISO international standards simultaneously.
    This Web page refers to Standard C as “ISO C” throughout.
    See [the ISO website](https://www.iso.org/iso/home/about.htm) for more
    information about the name of the organization and its language-independent
    three-letter acronym.

Java
:   A modern programming language originally developed by Sun Microsystems
    (now Oracle) supporting Object-Oriented programming. Although usually
    implemented by compiling to the instructions for a standard virtual
    machine (the JVM), the language can be compiled to native code.

Keyword
:   In the `awk` language, a keyword is a word that has special
    meaning. Keywords are reserved and may not be used as variable names.

    `gawk`’s keywords are:
    `BEGIN`,
    `BEGINFILE`,
    `END`,
    `ENDFILE`,
    `break`,
    `case`,
    `continue`,
    `default`,
    `delete`,
    `do…while`,
    `else`,
    `exit`,
    `for…in`,
    `for`,
    `function`,
    `func`,
    `if`,
    `next`,
    `nextfile`,
    `switch`,
    and
    `while`.

Korn Shell
:   The Korn Shell (`ksh`) is a Unix shell which was developed by David Korn at Bell
    Laboratories in the early 1980s. The Korn Shell is backward-compatible with the Bourne
    shell and includes many features of the C shell.
    See also “Bourne Shell.”

Lesser General Public License [¶](#index-LGPL-_0028Lesser-General-Public-License_0029)
:   This document describes the terms under which binary library archives
    or shared objects,
    and their source code may be distributed.

LGPL
:   See “Lesser General Public License.”

Linux
:   See “GNU/Linux.”

Localization
:   The process of providing the data necessary for an
    internationalized program to work in a particular language.

Logical Expression
:   An expression using the operators for logic, AND, OR, and NOT, written
    ‘`&&`’, ‘`||`’, and ‘`!`’ in `awk`. Often called *Boolean
    expressions*, after the mathematician who pioneered this kind of
    mathematical logic.

Lvalue
:   An expression that can appear on the left side of an assignment
    operator. In most languages, lvalues can be variables or array
    elements. In `awk`, a field designator can also be used as an
    lvalue.

Matching
:   The act of testing a string against a regular expression. If the
    regexp describes the contents of the string, it is said to *match* it.

Metacharacters
:   Characters used within a regexp that do not stand for themselves.
    Instead, they denote regular expression operations, such as repetition,
    grouping, or alternation.

Nesting
:   Nesting is where information is organized in layers, or where objects
    contain other similar objects.
    In `gawk` the `@include`
    directive can be nested. The “natural” nesting of arithmetic and
    logical operations can be changed using parentheses
    (see [Operator Precedence (How Operators Nest)](#Precedence)).

No-op
:   An operation that does nothing.

Null String
:   A string with no characters in it. It is represented explicitly in
    `awk` programs by placing two double quote characters next to
    each other (`""`). It can appear in input data by having two successive
    occurrences of the field separator appear next to each other.

Number
:   A numeric-valued data object. Modern `awk` implementations use
    double precision floating-point to represent numbers.
    Ancient `awk` implementations used single precision floating-point.

Octal
:   Base-eight notation, where the digits are `0`–`7`.
    Octal numbers are written in C using a leading ‘`0`’,
    to indicate their base. Thus, `013` is 11 ((1 x 8) + 3).
    See [Octal and Hexadecimal Numbers](#Nondecimal_002dnumbers).

Output Record
:   A single chunk of data that is written out by `awk`. Usually, an
    `awk` output record consists of one or more lines of text.
    See [How Input Is Split into Records](#Records).

Pattern
:   Patterns tell `awk` which input records are interesting to which
    rules.

    A pattern is an arbitrary conditional expression against which input is
    tested. If the condition is satisfied, the pattern is said to *match*
    the input record. A typical pattern might compare the input record against
    a regular expression. (See [Pattern Elements](#Pattern-Overview).)

PEBKAC
:   An acronym describing what is possibly the most frequent
    source of computer usage problems. (Problem Exists Between
    Keyboard And Chair.)

Plug-in
:   See “Extensions.”

POSIX
:   The name for a series of standards
    that specify a Portable Operating System interface. The “IX” denotes
    the Unix heritage of these standards. The main standard of interest for
    `awk` users is
    IEEE Standard for Information Technology, Standard 1003.1TM-2017
    (Revision of IEEE Std 1003.1-2008).
    The 2018 POSIX standard can be found online at
    <https://pubs.opengroup.org/onlinepubs/9699919799/>.

Precedence
:   The order in which operations are performed when operators are used
    without explicit parentheses.

Private
:   Variables and/or functions that are meant for use exclusively by library
    functions and not for the main `awk` program. Special care must be
    taken when naming such variables and functions.
    (See [Naming Library Function Global Variables](#Library-Names).)

Range (of input lines)
:   A sequence of consecutive lines from the input file(s). A pattern
    can specify ranges of input lines for `awk` to process or it can
    specify single lines. (See [Pattern Elements](#Pattern-Overview).)

Record
:   See “Input record” and “Output record.”

Recursion
:   When a function calls itself, either directly or indirectly.
    If this is clear, stop, and proceed to the next entry.
    Otherwise, refer to the entry for “recursion.”

Redirection
:   Redirection means performing input from something other than the standard input
    stream, or performing output to something other than the standard output stream.

    You can redirect input to the `getline` function using
    the ‘`<`’, ‘`|`’, and ‘`|&`’ operators.
    You can redirect the output of the `print` and `printf` statements
    to a file or a system command, using the ‘`>`’, ‘`>>`’, ‘`|`’, and ‘`|&`’
    operators.
    (See [Explicit Input with `getline`](#Getline),
    and [Redirecting Output of `print` and `printf`](#Redirection).)

Reference Counts
:   An internal mechanism in `gawk` to minimize the amount of memory
    needed to store the value of string variables. If the value assumed by
    a variable is used in more than one place, only one copy of the value
    itself is kept, and the associated reference count is increased when the
    same value is used by an additional variable, and decreased when the related
    variable is no longer in use. When the reference count goes to zero,
    the memory space used to store the value of the variable is freed.

Regexp
:   See “Regular Expression.”

Regular Expression
:   A regular expression (“regexp” for short) is a pattern that denotes a
    set of strings, possibly an infinite set. For example, the regular expression
    ‘`R.*xp`’ matches any string starting with the letter ‘`R`’
    and ending with the letters ‘`xp`’. In `awk`, regular expressions are
    used in patterns and in conditional expressions. Regular expressions may contain
    escape sequences. (See [Regular Expressions](#Regexp).)

Regular Expression Constant
:   A regular expression constant is a regular expression written within
    slashes, such as `/foo/`. This regular expression is chosen
    when you write the `awk` program and cannot be changed during
    its execution. (See [How to Use Regular Expressions](#Regexp-Usage).)

Regular Expression Operators
:   See “Metacharacters.”

Rounding
:   Rounding the result of an arithmetic operation can be tricky.
    More than one way of rounding exists, and in `gawk`
    it is possible to choose which method should be used in a program.
    See [Setting the Rounding Mode](#Setting-the-rounding-mode).

Rule
:   A segment of an `awk` program that specifies how to process single
    input records. A rule consists of a *pattern* and an *action*.
    `awk` reads an input record; then, for each rule, if the input record
    satisfies the rule’s pattern, `awk` executes the rule’s action.
    Otherwise, the rule does nothing for that input record.

Rvalue
:   A value that can appear on the right side of an assignment operator.
    In `awk`, essentially every expression has a value. These values
    are rvalues.

Scalar
:   A single value, be it a number or a string.
    Regular variables are scalars; arrays and functions are not.

Search Path
:   In `gawk`, a list of directories to search for `awk` program source files.
    In the shell, a list of directories to search for executable programs.

`sed`
:   See “Stream Editor.”

Seed
:   The initial value, or starting point, for a sequence of random numbers.

Shell
:   The command interpreter for Unix and POSIX-compliant systems.
    The shell works both interactively, and as a programming language
    for batch files, or shell scripts.

Short-Circuit
:   The nature of the `awk` logical operators ‘`&&`’ and ‘`||`’.
    If the value of the entire expression is determinable from evaluating just
    the lefthand side of these operators, the righthand side is not
    evaluated.
    (See [Boolean Expressions](#Boolean-Ops).)

Side Effect
:   A side effect occurs when an expression has an effect aside from merely
    producing a value. Assignment expressions, increment and decrement
    expressions, and function calls have side effects.
    (See [Assignment Expressions](#Assignment-Ops).)

Single Precision
:   An internal representation of numbers that can have fractional parts.
    Single precision numbers keep track of fewer digits than do double precision
    numbers, but operations on them are sometimes less expensive in terms of CPU time.
    This is the type used by some ancient versions of `awk` to store
    numeric values. It is the C type `float`.

Space
:   The character generated by hitting the space bar on the keyboard.

Special File
:   A file name interpreted internally by `gawk`, instead of being handed
    directly to the underlying operating system—for example, `/dev/stderr`.
    (See [Special File names in `gawk`](#Special-Files).)

Statement
:   An expression inside an `awk` program in the action part
    of a pattern–action rule, or inside an
    `awk` function. A statement can be a variable assignment,
    an array operation, a loop, etc.

Stream Editor
:   A program that reads records from an input stream and processes them one
    or more at a time. This is in contrast with batch programs, which may
    expect to read their input files in entirety before starting to do
    anything, as well as with interactive programs which require input from the
    user.

String
:   A datum consisting of a sequence of characters, such as ‘`I am a
    string`’. Constant strings are written with double quotes in the
    `awk` language and may contain escape sequences.
    (See [Escape Sequences](#Escape-Sequences).)

Tab
:   The character generated by hitting the `TAB` key on the keyboard.
    It usually expands to up to eight spaces upon output.

Text Domain
:   A unique name that identifies an application.
    Used for grouping messages that are translated at runtime
    into the local language.

Timestamp
:   A value in the “seconds since the epoch” format used by Unix
    and POSIX systems. Used for the `gawk` functions
    `mktime()`, `strftime()`, and `systime()`.
    See also “Epoch,” “GMT,” and “UTC.”

Unix [¶](#index-GNU_002fLinux-2)
:   A computer operating system originally developed in the early 1970’s at
    AT&T Bell Laboratories. It initially became popular in universities around
    the world and later moved into commercial environments as a software
    development system and network server system. There are many commercial
    versions of Unix, as well as several work-alike systems whose source code
    is freely available (such as GNU/Linux, [NetBSD](https://www.netbsd.org),
    [FreeBSD](https://www.freebsd.org), and [OpenBSD](https://www.openbsd.org)).

UTC
:   The accepted abbreviation for “Universal Coordinated Time.”
    This is standard time in Greenwich, England, which is used as a
    reference time for day and date calculations.
    See also “Epoch” and “GMT.”

Variable
:   A name for a value. In `awk`, variables may be either scalars
    or arrays.

Whitespace
:   A sequence of space, TAB, or newline characters occurring inside an input
    record or a string.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [Glossary](#Glossary), Up: [General Introduction](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## GNU General Public License [¶](#GNU-General-Public-License)

Version 3, 29 June 2007

```
Copyright © 2007 Free Software Foundation, Inc. https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.
```

### Preamble [¶](#Preamble)

The GNU General Public License is a free, copyleft license for
software and other kinds of works.

The licenses for most software and other practical works are designed
to take away your freedom to share and change the works. By contrast,
the GNU General Public License is intended to guarantee your freedom
to share and change all versions of a program—to make sure it remains
free software for all its users. We, the Free Software Foundation,
use the GNU General Public License for most of our software; it
applies also to any other work released this way by its authors. You
can apply it to your programs, too.

When we speak of free software, we are referring to freedom, not
price. Our General Public Licenses are designed to make sure that you
have the freedom to distribute copies of free software (and charge for
them if you wish), that you receive source code or can get it if you
want it, that you can change the software or use pieces of it in new
free programs, and that you know you can do these things.

To protect your rights, we need to prevent others from denying you
these rights or asking you to surrender the rights. Therefore, you
have certain responsibilities if you distribute copies of the
software, or if you modify it: responsibilities to respect the freedom
of others.

For example, if you distribute copies of such a program, whether
gratis or for a fee, you must pass on to the recipients the same
freedoms that you received. You must make sure that they, too,
receive or can get the source code. And you must show them these
terms so they know their rights.

Developers that use the GNU GPL protect your rights with two steps:
(1) assert copyright on the software, and (2) offer you this License
giving you legal permission to copy, distribute and/or modify it.

For the developers’ and authors’ protection, the GPL clearly explains
that there is no warranty for this free software. For both users’ and
authors’ sake, the GPL requires that modified versions be marked as
changed, so that their problems will not be attributed erroneously to
authors of previous versions.

Some devices are designed to deny users access to install or run
modified versions of the software inside them, although the
manufacturer can do so. This is fundamentally incompatible with the
aim of protecting users’ freedom to change the software. The
systematic pattern of such abuse occurs in the area of products for
individuals to use, which is precisely where it is most unacceptable.
Therefore, we have designed this version of the GPL to prohibit the
practice for those products. If such problems arise substantially in
other domains, we stand ready to extend this provision to those
domains in future versions of the GPL, as needed to protect the
freedom of users.

Finally, every program is threatened constantly by software patents.
States should not allow patents to restrict development and use of
software on general-purpose computers, but in those that do, we wish
to avoid the special danger that patents applied to a free program
could make it effectively proprietary. To prevent this, the GPL
assures that patents cannot be used to render the program non-free.

The precise terms and conditions for copying, distribution and
modification follow.
