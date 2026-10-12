### 26.9 Installing to Hard-Coded Locations [¶](#Installing-to-Hard_002dCoded-Locations)

```
My package needs to install some configuration file.  I tried to use
the following rule, but ‘make distcheck’ fails.  Why?
```

```
# Do not do this.
install-data-local:
        $(INSTALL_DATA) $(srcdir)/afile $(DESTDIR)/etc/afile
```

```
My package needs to populate the installation directory of another
package at install-time.  I can easily compute that installation
directory in configure, but if I install files therein,
‘make distcheck’ fails.  How else should I do it?
```

These two setups share their symptoms: ‘`make distcheck`’ fails
because they are installing files to hard-coded paths. In the latter
case the path is not hard-coded in the package, but we can consider it
to be hard-coded in the system (or in whichever tool that supplies the
path). As long as the path does not use any of the standard directory
variables (‘`$(prefix)`’, ‘`$(bindir)`’, ‘`$(datadir)`’,
etc.), the effect will be the same: user-installations are impossible.

As a (non-root) user who wants to install a package, you usually have no
right to install anything in `/usr` or `/usr/local`. So you
do something like ‘`./configure --prefix ~/usr`’ to install a
package in your own `~/usr` tree.

If a package attempts to install something to some hard-coded path
(e.g., `/etc/afile`), regardless of this `--prefix` setting,
then the installation will fail. ‘`make distcheck`’ performs such
a `--prefix` installation, hence it will fail too.

Now, there are some easy solutions.

The above `install-data-local` example for installing
`/etc/afile` would be better replaced by

```
sysconf_DATA = afile
```

By default `sysconfdir` will be ‘`$(prefix)/etc`’, because
this is what the GNU Standards require. When such a package is
installed on an FHS compliant system, the installer will have to set
‘`--sysconfdir=/etc`’. As the maintainer of the package you
should not be concerned by such site policies: use the appropriate
standard directory variable to install your files so that the installer
can easily redefine these variables to match their site conventions.

Installing files that should be used by another package is slightly
more involved. Let’s take an example and assume you want to install
a shared library that is a Python extension module. If you ask Python
where to install the library, it will answer something like this:

```
% python -c 'from distutils import sysconfig;
             print sysconfig.get_python_lib(1,0)'
/usr/lib/python2.5/site-packages
```

If you indeed use this absolute path to install your shared library,
non-root users will not be able to install the package; hence
distcheck fails.

Let’s do better. The ‘`sysconfig.get_python_lib()`’ function
accepts a third argument that will replace Python’s installation
prefix.

```
% python -c 'from distutils import sysconfig;
             print sysconfig.get_python_lib(1,0,"${exec_prefix}")'
${exec_prefix}/lib/python2.5/site-packages
```

You can also use this new path. If you do

* root users can install your package with the same `--prefix`
  as Python (you get the behavior of the previous attempt)
* non-root users can install your package too; they will have the
  extension module in a place that is not searched by Python but they
  can work around this using environment variables (and if you installed
  scripts that use this shared library, it’s easy to tell Python where to
  look in the beginning of your script, so the script works in both
  cases).

The `AM_PATH_PYTHON` macro uses similar commands to define
‘`$(pythondir)`’ and ‘`$(pyexecdir)`’ (see [Python](#Python)).

Of course not all tools are as advanced as Python regarding that
substitution of prefix. So another strategy is to figure out the
part of the installation directory that must be preserved. For
instance, here is how `AM_PATH_LISPDIR` (see [Emacs Lisp](#Emacs-Lisp))
computes ‘`$(lispdir)`’:

```
$EMACS -batch -no-site-file -eval '(while load-path
  (princ (concat (car load-path) "\n"))
  (setq load-path (cdr load-path)))' >conftest.out
lispdir=`sed -n
  -e 's,/$,,'
  -e '/.*\/lib\/x*emacs\/site-lisp$/{
        s,.*/lib/\(x*emacs/site-lisp\)$,${libdir}/\1,;p;q;
      }'
  -e '/.*\/share\/x*emacs\/site-lisp$/{
        s,.*/share/\(x*emacs/site-lisp\),${datarootdir}/\1,;p;q;
      }'
  conftest.out`
```

That is, it just picks the first directory that looks like
`*/lib/*emacs/site-lisp` or `*/share/*emacs/site-lisp` in
the search path of emacs, and then substitutes ‘`${libdir}`’ or
‘`${datadir}`’ appropriately.

The emacs case looks complicated because it processes a list and
expects two possible layouts; otherwise it’s easy, and the benefits
for non-root users are worth the extra `sed` invocation.

---

Next: [Reporting Bugs](#Reporting-Bugs), Previous: [Installing to Hard-Coded Locations](#Hard_002dCoded-Install-Paths), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.10 Debugging Make Rules [¶](#Debugging-Make-Rules-1)

The rules and dependency trees generated by `automake` can get
rather complex, and leave the developer head-scratching when things
don’t work as expected. Besides the debug options provided by the
`make` command (see [Options Summary](https://www.gnu.org/software/make/manual/make.html#Options-Summary) in The GNU Make
Manual), here’s a couple of further hints for debugging makefiles
generated by `automake` effectively:

* If less verbose output has been enabled in the package with the use
  of silent rules (see [How Automake can help in silencing Make](#Automake-Silent-Rules)), you’ll probably want
  undo that and see the actual commands being run: see [Unsilencing Automake](#Unsilencing-Automake).
* `make -n` can help show what would be done without actually doing
  it. However, this *still executes* commands prefixed with
  ‘`+`’, and, when using GNU `make`, commands that contain
  the strings ‘`$(MAKE)`’ or ‘`${MAKE}`’ (see [Instead of
  Execution](https://www.gnu.org/software/make/manual/make.html#Instead-of-Execution) in The GNU Make Manual). Typically, this is helpful
  to show what recursive rules would do, but it means that, in your own
  rules, you should not mix such recursion with actions that change any
  files.[6](#FOOT6) Furthermore,
  GNU `make` will update prerequisites for the `Makefile`
  file itself even with `-n` (see [Remaking Makefiles](https://www.gnu.org/software/make/manual/make.html#Remaking-Makefiles) in The GNU Make Manual).
* `make SHELL="/bin/bash -vx"` can help debug complex rules.
  See [The Make Macro SHELL](https://www.gnu.org/software/autoconf/manual/autoconf.html#The-Make-Macro-SHELL) in The Autoconf Manual, for some
  portability quirks associated with this construct.
* `echo 'print: ; @echo "$(VAR)"' | make -f Makefile -f - print`
  can be handy to examine the expanded value of variables. You may need
  to use a target other than ‘`print`’ if that is already used or a
  file with that name exists.
* <http://bashdb.sourceforge.net/remake/> provides a modified
  GNU `make` command called `remake` that copes with
  complex GNU `make`-specific Makefiles and allows tracing
  execution, examining variables, and calling rules interactively, much like
  a debugger.

---

Previous: [Debugging Make Rules](#Debugging-Make-Rules), Up: [Frequently Asked Questions about Automake](#FAQ)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### 26.11 Reporting Bugs [¶](#Reporting-Bugs-1)

Most nontrivial software has bugs. Automake is no exception. We
cannot promise we can or will fix a bug, and we might not even agree
that it is a bug, but we want to hear about problems you
encounter. Often we agree they are bugs and want to fix them.

So, to make it possible for us to fix a bug, please report it.
If you can, though, it is helpful if you check if it is already known.
You can look at the [GNU Bug Tracker](https://debbugs.gnu.org/)
and the [bug-automake mailing list archives](https://lists.gnu.org/archive/html/bug-automake/) for previous bug reports. (We
previously used a Gnats database for bug tracking, but it is no longer
online.)

If the bug is not already known, it should be reported. To report
bugs in a way that is useful and efficient, please read
[How
to Report Bugs Effectively](https://www.chiark.greenend.org.uk/~sgtatham/bugs.html) and
[How to Ask
Questions the Smart Way](http://catb.org/~esr/faqs/smart-questions.html). Good bug reports save time for everyone.

For a bug report, a feature request or other suggestions, please send
email to [bug-automake@gnu.org](mailto:bug-automake@gnu.org). This will then open a new
bug in the [bug tracker](https://debbugs.gnu.org/automake). Be
sure to include the versions of Autoconf and Automake that you use and
the kind of system you’re on. Ideally, post a minimal
`Makefile.am` and `configure.ac` that reproduces the problem
you encounter. If you have encountered test suite failures, please
attach the `test-suite.log` file.

---

Next: [Indices](#Indices), Previous: [Frequently Asked Questions about Automake](#FAQ), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## Appendix A Copying This Manual [¶](#Copying-This-Manual-1)

* [GNU Free Documentation License](#GNU-Free-Documentation-License)

---

Up: [Copying This Manual](#Copying-This-Manual)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### A.1 GNU Free Documentation License [¶](#GNU-Free-Documentation-License-1)

Version 1.3, 3 November 2008

```
Copyright © 2000–2026 Free Software Foundation, Inc.
https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.
```

0. PREAMBLE

   The purpose of this License is to make a manual, textbook, or other
   functional and useful document *free* in the sense of freedom: to
   assure everyone the effective freedom to copy and redistribute it,
   with or without modifying it, either commercially or noncommercially.
   Secondarily, this License preserves for the author and publisher a way
   to get credit for their work, while not being considered responsible
   for modifications made by others.

   This License is a kind of “copyleft”, which means that derivative
   works of the document must themselves be free in the same sense. It
   complements the GNU General Public License, which is a copyleft
   license designed for free software.

   We have designed this License in order to use it for manuals for free
   software, because free software needs free documentation: a free
   program should come with manuals providing the same freedoms that the
   software does. But this License is not limited to software manuals;
   it can be used for any textual work, regardless of subject matter or
   whether it is published as a printed book. We recommend this License
   principally for works whose purpose is instruction or reference.
1. APPLICABILITY AND DEFINITIONS

   This License applies to any manual or other work, in any medium, that
   contains a notice placed by the copyright holder saying it can be
   distributed under the terms of this License. Such a notice grants a
   world-wide, royalty-free license, unlimited in duration, to use that
   work under the conditions stated herein. The “Document”, below,
   refers to any such manual or work. Any member of the public is a
   licensee, and is addressed as “you”. You accept the license if you
   copy, modify or distribute the work in a way requiring permission
   under copyright law.

   A “Modified Version” of the Document means any work containing the
   Document or a portion of it, either copied verbatim, or with
   modifications and/or translated into another language.

   A “Secondary Section” is a named appendix or a front-matter section
   of the Document that deals exclusively with the relationship of the
   publishers or authors of the Document to the Document’s overall
   subject (or to related matters) and contains nothing that could fall
   directly within that overall subject. (Thus, if the Document is in
   part a textbook of mathematics, a Secondary Section may not explain
   any mathematics.) The relationship could be a matter of historical
   connection with the subject or with related matters, or of legal,
   commercial, philosophical, ethical or political position regarding
   them.

   The “Invariant Sections” are certain Secondary Sections whose titles
   are designated, as being those of Invariant Sections, in the notice
   that says that the Document is released under this License. If a
   section does not fit the above definition of Secondary then it is not
   allowed to be designated as Invariant. The Document may contain zero
   Invariant Sections. If the Document does not identify any Invariant
   Sections then there are none.

   The “Cover Texts” are certain short passages of text that are listed,
   as Front-Cover Texts or Back-Cover Texts, in the notice that says that
   the Document is released under this License. A Front-Cover Text may
   be at most 5 words, and a Back-Cover Text may be at most 25 words.

   A “Transparent” copy of the Document means a machine-readable copy,
   represented in a format whose specification is available to the
   general public, that is suitable for revising the document
   straightforwardly with generic text editors or (for images composed of
   pixels) generic paint programs or (for drawings) some widely available
   drawing editor, and that is suitable for input to text formatters or
   for automatic translation to a variety of formats suitable for input
   to text formatters. A copy made in an otherwise Transparent file
   format whose markup, or absence of markup, has been arranged to thwart
   or discourage subsequent modification by readers is not Transparent.
   An image format is not Transparent if used for any substantial amount
   of text. A copy that is not “Transparent” is called “Opaque”.

   Examples of suitable formats for Transparent copies include plain
   ASCII without markup, Texinfo input format, LaTeX input
   format, SGML or XML using a publicly available
   DTD, and standard-conforming simple HTML,
   PostScript or PDF designed for human modification. Examples
   of transparent image formats include PNG, XCF and
   JPG. Opaque formats include proprietary formats that can be
   read and edited only by proprietary word processors, SGML or
   XML for which the DTD and/or processing tools are
   not generally available, and the machine-generated HTML,
   PostScript or PDF produced by some word processors for
   output purposes only.

   The “Title Page” means, for a printed book, the title page itself,
   plus such following pages as are needed to hold, legibly, the material
   this License requires to appear in the title page. For works in
   formats which do not have any title page as such, “Title Page” means
   the text near the most prominent appearance of the work’s title,
   preceding the beginning of the body of the text.

   The “publisher” means any person or entity that distributes copies
   of the Document to the public.

   A section “Entitled XYZ” means a named subunit of the Document whose
   title either is precisely XYZ or contains XYZ in parentheses following
   text that translates XYZ in another language. (Here XYZ stands for a
   specific section name mentioned below, such as “Acknowledgements”,
   “Dedications”, “Endorsements”, or “History”.) To “Preserve the Title”
   of such a section when you modify the Document means that it remains a
   section “Entitled XYZ” according to this definition.

   The Document may include Warranty Disclaimers next to the notice which
   states that this License applies to the Document. These Warranty
   Disclaimers are considered to be included by reference in this
   License, but only as regards disclaiming warranties: any other
   implication that these Warranty Disclaimers may have is void and has
   no effect on the meaning of this License.
2. VERBATIM COPYING

   You may copy and distribute the Document in any medium, either
   commercially or noncommercially, provided that this License, the
   copyright notices, and the license notice saying this License applies
   to the Document are reproduced in all copies, and that you add no other
   conditions whatsoever to those of this License. You may not use
   technical measures to obstruct or control the reading or further
   copying of the copies you make or distribute. However, you may accept
   compensation in exchange for copies. If you distribute a large enough
   number of copies you must also follow the conditions in section 3.

   You may also lend copies, under the same conditions stated above, and
   you may publicly display copies.
3. COPYING IN QUANTITY

   If you publish printed copies (or copies in media that commonly have
   printed covers) of the Document, numbering more than 100, and the
   Document’s license notice requires Cover Texts, you must enclose the
   copies in covers that carry, clearly and legibly, all these Cover
   Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on
   the back cover. Both covers must also clearly and legibly identify
   you as the publisher of these copies. The front cover must present
   the full title with all words of the title equally prominent and
   visible. You may add other material on the covers in addition.
   Copying with changes limited to the covers, as long as they preserve
   the title of the Document and satisfy these conditions, can be treated
   as verbatim copying in other respects.

   If the required texts for either cover are too voluminous to fit
   legibly, you should put the first ones listed (as many as fit
   reasonably) on the actual cover, and continue the rest onto adjacent
   pages.

   If you publish or distribute Opaque copies of the Document numbering
   more than 100, you must either include a machine-readable Transparent
   copy along with each Opaque copy, or state in or with each Opaque copy
   a computer-network location from which the general network-using
   public has access to download using public-standard network protocols
   a complete Transparent copy of the Document, free of added material.
   If you use the latter option, you must take reasonably prudent steps,
   when you begin distribution of Opaque copies in quantity, to ensure
   that this Transparent copy will remain thus accessible at the stated
   location until at least one year after the last time you distribute an
   Opaque copy (directly or through your agents or retailers) of that
   edition to the public.

   It is requested, but not required, that you contact the authors of the
   Document well before redistributing any large number of copies, to give
   them a chance to provide you with an updated version of the Document.
4. MODIFICATIONS

   You may copy and distribute a Modified Version of the Document under
   the conditions of sections 2 and 3 above, provided that you release
   the Modified Version under precisely this License, with the Modified
   Version filling the role of the Document, thus licensing distribution
   and modification of the Modified Version to whoever possesses a copy
   of it. In addition, you must do these things in the Modified Version:

   1. Use in the Title Page (and on the covers, if any) a title distinct
      from that of the Document, and from those of previous versions
      (which should, if there were any, be listed in the History section
      of the Document). You may use the same title as a previous version
      if the original publisher of that version gives permission.
   2. List on the Title Page, as authors, one or more persons or entities
      responsible for authorship of the modifications in the Modified
      Version, together with at least five of the principal authors of the
      Document (all of its principal authors, if it has fewer than five),
      unless they release you from this requirement.
   3. State on the Title page the name of the publisher of the
      Modified Version, as the publisher.
   4. Preserve all the copyright notices of the Document.
   5. Add an appropriate copyright notice for your modifications
      adjacent to the other copyright notices.
   6. Include, immediately after the copyright notices, a license notice
      giving the public permission to use the Modified Version under the
      terms of this License, in the form shown in the Addendum below.
   7. Preserve in that license notice the full lists of Invariant Sections
      and required Cover Texts given in the Document’s license notice.
   8. Include an unaltered copy of this License.
   9. Preserve the section Entitled “History”, Preserve its Title, and add
      to it an item stating at least the title, year, new authors, and
      publisher of the Modified Version as given on the Title Page. If
      there is no section Entitled “History” in the Document, create one
      stating the title, year, authors, and publisher of the Document as
      given on its Title Page, then add an item describing the Modified
      Version as stated in the previous sentence.
   10. Preserve the network location, if any, given in the Document for
       public access to a Transparent copy of the Document, and likewise
       the network locations given in the Document for previous versions
       it was based on. These may be placed in the “History” section.
       You may omit a network location for a work that was published at
       least four years before the Document itself, or if the original
       publisher of the version it refers to gives permission.
   11. For any section Entitled “Acknowledgements” or “Dedications”, Preserve
       the Title of the section, and preserve in the section all the
       substance and tone of each of the contributor acknowledgements and/or
       dedications given therein.
   12. Preserve all the Invariant Sections of the Document,
       unaltered in their text and in their titles. Section numbers
       or the equivalent are not considered part of the section titles.
   13. Delete any section Entitled “Endorsements”. Such a section
       may not be included in the Modified Version.
   14. Do not retitle any existing section to be Entitled “Endorsements” or
       to conflict in title with any Invariant Section.
   15. Preserve any Warranty Disclaimers.

   If the Modified Version includes new front-matter sections or
   appendices that qualify as Secondary Sections and contain no material
   copied from the Document, you may at your option designate some or all
   of these sections as invariant. To do this, add their titles to the
   list of Invariant Sections in the Modified Version’s license notice.
   These titles must be distinct from any other section titles.

   You may add a section Entitled “Endorsements”, provided it contains
   nothing but endorsements of your Modified Version by various
   parties—for example, statements of peer review or that the text has
   been approved by an organization as the authoritative definition of a
   standard.

   You may add a passage of up to five words as a Front-Cover Text, and a
   passage of up to 25 words as a Back-Cover Text, to the end of the list
   of Cover Texts in the Modified Version. Only one passage of
   Front-Cover Text and one of Back-Cover Text may be added by (or
   through arrangements made by) any one entity. If the Document already
   includes a cover text for the same cover, previously added by you or
   by arrangement made by the same entity you are acting on behalf of,
   you may not add another; but you may replace the old one, on explicit
   permission from the previous publisher that added the old one.

   The author(s) and publisher(s) of the Document do not by this License
   give permission to use their names for publicity for or to assert or
   imply endorsement of any Modified Version.
5. COMBINING DOCUMENTS

   You may combine the Document with other documents released under this
   License, under the terms defined in section 4 above for modified
   versions, provided that you include in the combination all of the
   Invariant Sections of all of the original documents, unmodified, and
   list them all as Invariant Sections of your combined work in its
   license notice, and that you preserve all their Warranty Disclaimers.

   The combined work need only contain one copy of this License, and
   multiple identical Invariant Sections may be replaced with a single
   copy. If there are multiple Invariant Sections with the same name but
   different contents, make the title of each such section unique by
   adding at the end of it, in parentheses, the name of the original
   author or publisher of that section if known, or else a unique number.
   Make the same adjustment to the section titles in the list of
   Invariant Sections in the license notice of the combined work.

   In the combination, you must combine any sections Entitled “History”
   in the various original documents, forming one section Entitled
   “History”; likewise combine any sections Entitled “Acknowledgements”,
   and any sections Entitled “Dedications”. You must delete all
   sections Entitled “Endorsements.”
6. COLLECTIONS OF DOCUMENTS

   You may make a collection consisting of the Document and other documents
   released under this License, and replace the individual copies of this
   License in the various documents with a single copy that is included in
   the collection, provided that you follow the rules of this License for
   verbatim copying of each of the documents in all other respects.

   You may extract a single document from such a collection, and distribute
   it individually under this License, provided you insert a copy of this
   License into the extracted document, and follow this License in all
   other respects regarding verbatim copying of that document.
7. AGGREGATION WITH INDEPENDENT WORKS

   A compilation of the Document or its derivatives with other separate
   and independent documents or works, in or on a volume of a storage or
   distribution medium, is called an “aggregate” if the copyright
   resulting from the compilation is not used to limit the legal rights
   of the compilation’s users beyond what the individual works permit.
   When the Document is included in an aggregate, this License does not
   apply to the other works in the aggregate which are not themselves
   derivative works of the Document.

   If the Cover Text requirement of section 3 is applicable to these
   copies of the Document, then if the Document is less than one half of
   the entire aggregate, the Document’s Cover Texts may be placed on
   covers that bracket the Document within the aggregate, or the
   electronic equivalent of covers if the Document is in electronic form.
   Otherwise they must appear on printed covers that bracket the whole
   aggregate.
8. TRANSLATION

   Translation is considered a kind of modification, so you may
   distribute translations of the Document under the terms of section 4.
   Replacing Invariant Sections with translations requires special
   permission from their copyright holders, but you may include
   translations of some or all Invariant Sections in addition to the
   original versions of these Invariant Sections. You may include a
   translation of this License, and all the license notices in the
   Document, and any Warranty Disclaimers, provided that you also include
   the original English version of this License and the original versions
   of those notices and disclaimers. In case of a disagreement between
   the translation and the original version of this License or a notice
   or disclaimer, the original version will prevail.

   If a section in the Document is Entitled “Acknowledgements”,
   “Dedications”, or “History”, the requirement (section 4) to Preserve
   its Title (section 1) will typically require changing the actual
   title.
9. TERMINATION

   You may not copy, modify, sublicense, or distribute the Document
   except as expressly provided under this License. Any attempt
   otherwise to copy, modify, sublicense, or distribute it is void, and
   will automatically terminate your rights under this License.

   However, if you cease all violation of this License, then your license
   from a particular copyright holder is reinstated (a) provisionally,
   unless and until the copyright holder explicitly and finally
   terminates your license, and (b) permanently, if the copyright holder
   fails to notify you of the violation by some reasonable means prior to
   60 days after the cessation.

   Moreover, your license from a particular copyright holder is
   reinstated permanently if the copyright holder notifies you of the
   violation by some reasonable means, this is the first time you have
   received notice of violation of this License (for any work) from that
   copyright holder, and you cure the violation prior to 30 days after
   your receipt of the notice.

   Termination of your rights under this section does not terminate the
   licenses of parties who have received copies or rights from you under
   this License. If your rights have been terminated and not permanently
   reinstated, receipt of a copy of some or all of the same material does
   not give you any rights to use it.
10. FUTURE REVISIONS OF THIS LICENSE

    The Free Software Foundation may publish new, revised versions
    of the GNU Free Documentation License from time to time. Such new
    versions will be similar in spirit to the present version, but may
    differ in detail to address new problems or concerns. See
    <https://www.gnu.org/copyleft/>.

    Each version of the License is given a distinguishing version number.
    If the Document specifies that a particular numbered version of this
    License “or any later version” applies to it, you have the option of
    following the terms and conditions either of that specified version or
    of any later version that has been published (not as a draft) by the
    Free Software Foundation. If the Document does not specify a version
    number of this License, you may choose any version ever published (not
    as a draft) by the Free Software Foundation. If the Document
    specifies that a proxy can decide which future versions of this
    License can be used, that proxy’s public statement of acceptance of a
    version permanently authorizes you to choose that version for the
    Document.
11. RELICENSING

    “Massive Multiauthor Collaboration Site” (or “MMC Site”) means any
    World Wide Web server that publishes copyrightable works and also
    provides prominent facilities for anybody to edit those works. A
    public wiki that anybody can edit is an example of such a server. A
    “Massive Multiauthor Collaboration” (or “MMC”) contained in the
    site means any set of copyrightable works thus published on the MMC
    site.

    “CC-BY-SA” means the Creative Commons Attribution-Share Alike 3.0
    license published by Creative Commons Corporation, a not-for-profit
    corporation with a principal place of business in San Francisco,
    California, as well as future copyleft versions of that license
    published by that same organization.

    “Incorporate” means to publish or republish a Document, in whole or
    in part, as part of another Document.

    An MMC is “eligible for relicensing” if it is licensed under this
    License, and if all works that were first published under this License
    somewhere other than this MMC, and subsequently incorporated in whole
    or in part into the MMC, (1) had no cover texts or invariant sections,
    and (2) were thus incorporated prior to November 1, 2008.

    The operator of an MMC Site may republish an MMC contained in the site
    under CC-BY-SA on the same site at any time before August 1, 2009,
    provided the MMC is eligible for relicensing.

### ADDENDUM: How to use this License for your documents [¶](#ADDENDUM_003a-How-to-use-this-License-for-your-documents)

To use this License in a document you have written, include a copy of
the License in the document and put the following copyright and
license notices just after the title page:

```
  Copyright (C)  year  your name.
  Permission is granted to copy, distribute and/or modify this document
  under the terms of the GNU Free Documentation License, Version 1.3
  or any later version published by the Free Software Foundation;
  with no Invariant Sections, no Front-Cover Texts, and no Back-Cover
  Texts.  A copy of the license is included in the section entitled ``GNU
  Free Documentation License''.
```

If you have Invariant Sections, Front-Cover Texts and Back-Cover Texts,
replace the “with…Texts.” line with this:

```
    with the Invariant Sections being list their titles, with
    the Front-Cover Texts being list, and with the Back-Cover Texts
    being list.
```

If you have Invariant Sections without Cover Texts, or some other
combination of the three, merge those two alternatives to suit the
situation.

If your document contains nontrivial examples of program code, we
recommend releasing these examples in parallel under your choice of
free software license, such as the GNU General Public License,
to permit their use in free software.

---

Previous: [Copying This Manual](#Copying-This-Manual), Up: [GNU Automake](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]## Appendix B Indices [¶](#Indices-1)

* [Macro Index](#Macro-Index)
* [Variable Index](#Variable-Index)
* [General Index](#General-Index)

---

Next: [Variable Index](#Variable-Index), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### B.1 Macro Index [¶](#Macro-Index-1)

|  |  |
| --- | --- |
| Jump to: | [**\_**](#Macro-Index_fn_symbol-1)     [**A**](#Macro-Index_fn_letter-A)   [**L**](#Macro-Index_fn_letter-L)   [**M**](#Macro-Index_fn_letter-M) |

|  |  |
| --- | --- |
| --- | |
| \_ | |
| [`_AM_DEPENDENCIES`](#index-_005fAM_005fDEPENDENCIES) | [Private Macros](#Private-Macros) |
| --- | |
| A | |
| [`AC_CANONICAL_BUILD`](#index-AC_005fCANONICAL_005fBUILD) | [Optional](#Optional) |
| [`AC_CANONICAL_HOST`](#index-AC_005fCANONICAL_005fHOST) | [Optional](#Optional) |
| [`AC_CANONICAL_TARGET`](#index-AC_005fCANONICAL_005fTARGET) | [Optional](#Optional) |
| [`AC_CONFIG_AUX_DIR`](#index-AC_005fCONFIG_005fAUX_005fDIR-1) | [Optional](#Optional) |
| [`AC_CONFIG_AUX_DIR`](#index-AC_005fCONFIG_005fAUX_005fDIR) | [Subpackages](#Subpackages) |
| [`AC_CONFIG_FILES`](#index-AC_005fCONFIG_005fFILES) | [Requirements](#Requirements) |
| [`AC_CONFIG_FILES`](#index-AC_005fCONFIG_005fFILES-1) | [Basics of Distribution](#Basics-of-Distribution) |
| [`AC_CONFIG_HEADERS`](#index-AC_005fCONFIG_005fHEADERS) | [Optional](#Optional) |
| [`AC_CONFIG_LIBOBJ_DIR`](#index-AC_005fCONFIG_005fLIBOBJ_005fDIR-1) | [Optional](#Optional) |
| [`AC_CONFIG_LIBOBJ_DIR`](#index-AC_005fCONFIG_005fLIBOBJ_005fDIR) | [LIBOBJS](#LIBOBJS) |
| [`AC_CONFIG_LINKS`](#index-AC_005fCONFIG_005fLINKS) | [Optional](#Optional) |
| [`AC_CONFIG_SUBDIRS`](#index-AC_005fCONFIG_005fSUBDIRS) | [Subpackages](#Subpackages) |
| [`AC_CONFIG_SUBDIRS`](#index-AC_005fCONFIG_005fSUBDIRS-1) | [The dist Hook](#The-dist-Hook) |
| [`AC_DEFUN`](#index-AC_005fDEFUN) | [Extending aclocal](#Extending-aclocal) |
| [`AC_F77_LIBRARY_LDFLAGS`](#index-AC_005fF77_005fLIBRARY_005fLDFLAGS) | [Optional](#Optional) |
| [`AC_FC_SRCEXT`](#index-AC_005fFC_005fSRCEXT) | [Optional](#Optional) |
| [`AC_INIT`](#index-AC_005fINIT) | [Public Macros](#Public-Macros) |
| [`AC_LIBOBJ`](#index-AC_005fLIBOBJ-2) | [Optional](#Optional) |
| [`AC_LIBOBJ`](#index-AC_005fLIBOBJ) | [LTLIBOBJS](#LTLIBOBJS) |
| [`AC_LIBOBJ`](#index-AC_005fLIBOBJ-1) | [LIBOBJS](#LIBOBJS) |
| [`AC_LIBSOURCE`](#index-AC_005fLIBSOURCE-1) | [Optional](#Optional) |
| [`AC_LIBSOURCE`](#index-AC_005fLIBSOURCE) | [LIBOBJS](#LIBOBJS) |
| [`AC_LIBSOURCES`](#index-AC_005fLIBSOURCES) | [Optional](#Optional) |
| [`AC_OUTPUT`](#index-AC_005fOUTPUT) | [Requirements](#Requirements) |
| [`AC_PREREQ`](#index-AC_005fPREREQ) | [Extending aclocal](#Extending-aclocal) |
| [`AC_PROG_CXX`](#index-AC_005fPROG_005fCXX) | [Optional](#Optional) |
| [`AC_PROG_F77`](#index-AC_005fPROG_005fF77) | [Optional](#Optional) |
| [`AC_PROG_FC`](#index-AC_005fPROG_005fFC) | [Optional](#Optional) |
| [`AC_PROG_LEX`](#index-AC_005fPROG_005fLEX) | [Public Macros](#Public-Macros) |
| [`AC_PROG_LEX`](#index-AC_005fPROG_005fLEX-1) | [Optional](#Optional) |
| [`AC_PROG_LIBTOOL`](#index-AC_005fPROG_005fLIBTOOL) | [Optional](#Optional) |
| [`AC_PROG_OBJC`](#index-AC_005fPROG_005fOBJC) | [Optional](#Optional) |
| [`AC_PROG_OBJCXX`](#index-AC_005fPROG_005fOBJCXX) | [Optional](#Optional) |
| [`AC_PROG_RANLIB`](#index-AC_005fPROG_005fRANLIB) | [Optional](#Optional) |
| [`AC_PROG_YACC`](#index-AC_005fPROG_005fYACC) | [Optional](#Optional) |
| [`AC_REQUIRE_AUX_FILE`](#index-AC_005fREQUIRE_005fAUX_005fFILE) | [Optional](#Optional) |
| [`AC_SUBST`](#index-AC_005fSUBST) | [Optional](#Optional) |
| [`AM_COND_IF`](#index-AM_005fCOND_005fIF-1) | [Optional](#Optional) |
| [`AM_COND_IF`](#index-AM_005fCOND_005fIF) | [Usage of Conditionals](#Usage-of-Conditionals) |
| [`AM_COND_IF`](#index-AM_005fCOND_005fIF-2) | [Usage of Conditionals](#Usage-of-Conditionals) |
| [`AM_CONDITIONAL`](#index-AM_005fCONDITIONAL-1) | [Optional](#Optional) |
| [`AM_CONDITIONAL`](#index-AM_005fCONDITIONAL) | [Usage of Conditionals](#Usage-of-Conditionals) |
| [`AM_CONDITIONAL`](#index-AM_005fCONDITIONAL-2) | [Usage of Conditionals](#Usage-of-Conditionals) |
| [`AM_DEP_TRACK`](#index-AM_005fDEP_005fTRACK) | [Private Macros](#Private-Macros) |
| [`AM_GNU_GETTEXT`](#index-AM_005fGNU_005fGETTEXT) | [Optional](#Optional) |
| [`AM_GNU_GETTEXT_INTL_SUBDIR`](#index-AM_005fGNU_005fGETTEXT_005fINTL_005fSUBDIR) | [Optional](#Optional) |
| [`AM_INIT_AUTOMAKE`](#index-AM_005fINIT_005fAUTOMAKE) | [Requirements](#Requirements) |
| [`AM_INIT_AUTOMAKE`](#index-AM_005fINIT_005fAUTOMAKE-1) | [Public Macros](#Public-Macros) |
| [`AM_MAINTAINER_MODE`](#index-AM_005fMAINTAINER_005fMODE) | [Rebuilding](#Rebuilding) |
| [`AM_MAINTAINER_MODE`](#index-AM_005fMAINTAINER_005fMODE-1) | [maintainer-mode](#maintainer_002dmode) |
| [`AM_MAINTAINER_MODE([default-mode])`](#index-AM_005fMAINTAINER_005fMODE_0028_005bdefault_002dmode_005d_0029) | [Optional](#Optional) |
| [`AM_MAKE_INCLUDE`](#index-AM_005fMAKE_005fINCLUDE) | [Private Macros](#Private-Macros) |
| [`AM_MISSING_PROG`](#index-AM_005fMISSING_005fPROG) | [Public Macros](#Public-Macros) |
| [`AM_OPTIONAL_AUTOMAKE`](#index-AM_005fOPTIONAL_005fAUTOMAKE) | [Public Macros](#Public-Macros) |
| [`AM_OUTPUT_DEPENDENCY_COMMANDS`](#index-AM_005fOUTPUT_005fDEPENDENCY_005fCOMMANDS) | [Private Macros](#Private-Macros) |
| [`AM_PATH_LISPDIR`](#index-AM_005fPATH_005fLISPDIR) | [Public Macros](#Public-Macros) |
| [`AM_PATH_PYTHON`](#index-AM_005fPATH_005fPYTHON) | [Python](#Python) |
| [`AM_PROG_AR`](#index-AM_005fPROG_005fAR) | [Public Macros](#Public-Macros) |
| [`AM_PROG_AS`](#index-AM_005fPROG_005fAS) | [Public Macros](#Public-Macros) |
| [`AM_PROG_CC_C_O`](#index-AM_005fPROG_005fCC_005fC_005fO) | [Public Macros](#Public-Macros) |
| [`AM_PROG_GCJ`](#index-AM_005fPROG_005fGCJ) | [Public Macros](#Public-Macros) |
| [`AM_PROG_INSTALL_STRIP`](#index-AM_005fPROG_005fINSTALL_005fSTRIP) | [Private Macros](#Private-Macros) |
| [`AM_PROG_LEX`](#index-AM_005fPROG_005fLEX) | [Public Macros](#Public-Macros) |
| [`AM_PROG_MKDIR_P`](#index-AM_005fPROG_005fMKDIR_005fP) | [Obsolete Macros](#Obsolete-Macros) |
| [`AM_PROG_UPC`](#index-AM_005fPROG_005fUPC) | [Public Macros](#Public-Macros) |
| [`AM_PROG_VALAC`](#index-AM_005fPROG_005fVALAC) | [Vala Support](#Vala-Support) |
| [`AM_SANITY_CHECK`](#index-AM_005fSANITY_005fCHECK) | [Private Macros](#Private-Macros) |
| [`AM_SET_DEPDIR`](#index-AM_005fSET_005fDEPDIR) | [Private Macros](#Private-Macros) |
| [`AM_SILENT_RULES`](#index-AM_005fSILENT_005fRULES) | [Public Macros](#Public-Macros) |
| [`AM_SUBST_NOTMAKE(var)`](#index-AM_005fSUBST_005fNOTMAKE_0028var_0029) | [Optional](#Optional) |
| [`AM_WITH_DMALLOC`](#index-AM_005fWITH_005fDMALLOC) | [Public Macros](#Public-Macros) |
| --- | |
| L | |
| [`LT_INIT`](#index-LT_005fINIT) | [Optional](#Optional) |
| --- | |
| M | |
| [`m4_include`](#index-m4_005finclude) | [Basics of Distribution](#Basics-of-Distribution) |
| [`m4_include`](#index-m4_005finclude-1) | [Optional](#Optional) |
| --- | |

|  |  |
| --- | --- |
| Jump to: | [**\_**](#Macro-Index_fn_symbol-1)     [**A**](#Macro-Index_fn_letter-A)   [**L**](#Macro-Index_fn_letter-L)   [**M**](#Macro-Index_fn_letter-M) |

---

Next: [General Index](#General-Index), Previous: [Macro Index](#Macro-Index), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

### B.2 Variable Index [¶](#Variable-Index-1)

|  |  |
| --- | --- |
| Jump to: | [**\_**](#Variable-Index_vr_symbol-1)     [**A**](#Variable-Index_vr_letter-A)   [**B**](#Variable-Index_vr_letter-B)   [**C**](#Variable-Index_vr_letter-C)   [**D**](#Variable-Index_vr_letter-D)   [**E**](#Variable-Index_vr_letter-E)   [**F**](#Variable-Index_vr_letter-F)   [**G**](#Variable-Index_vr_letter-G)   [**H**](#Variable-Index_vr_letter-H)   [**I**](#Variable-Index_vr_letter-I)   [**J**](#Variable-Index_vr_letter-J)   [**L**](#Variable-Index_vr_letter-L)   [**M**](#Variable-Index_vr_letter-M)   [**N**](#Variable-Index_vr_letter-N)   [**O**](#Variable-Index_vr_letter-O)   [**P**](#Variable-Index_vr_letter-P)   [**R**](#Variable-Index_vr_letter-R)   [**S**](#Variable-Index_vr_letter-S)   [**T**](#Variable-Index_vr_letter-T)   [**U**](#Variable-Index_vr_letter-U)   [**V**](#Variable-Index_vr_letter-V)   [**W**](#Variable-Index_vr_letter-W)   [**X**](#Variable-Index_vr_letter-X)   [**Y**](#Variable-Index_vr_letter-Y)   [**Z**](#Variable-Index_vr_letter-Z) |

|  |  |
| --- | --- |
| --- | |
| \_ | |
| [`_DATA`](#index-_005fDATA) | [Data](#Data) |
| [`_HEADERS`](#index-_005fHEADERS) | [Headers](#Headers) |
| [`_LIBRARIES`](#index-_005fLIBRARIES) | [A Library](#A-Library) |
| [`_LISP`](#index-_005fLISP) | [Emacs Lisp](#Emacs-Lisp) |
| [`_LOG_COMPILE`](#index-_005fLOG_005fCOMPILE) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`_LOG_COMPILER`](#index-_005fLOG_005fCOMPILER) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`_LOG_DRIVER`](#index-_005fLOG_005fDRIVER) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`_LOG_DRIVER_FLAGS`](#index-_005fLOG_005fDRIVER_005fFLAGS) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`_LOG_FLAGS`](#index-_005fLOG_005fFLAGS) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`_LTLIBRARIES`](#index-_005fLTLIBRARIES) | [Libtool Libraries](#Libtool-Libraries) |
| [`_MANS`](#index-_005fMANS) | [Man Pages](#Man-Pages) |
| [`_PROGRAMS`](#index-_005fPROGRAMS) | [Uniform](#Uniform) |
| [`_PROGRAMS`](#index-_005fPROGRAMS-1) | [Program Sources](#Program-Sources) |
| [`_PYTHON`](#index-_005fPYTHON) | [Python](#Python) |
| [`_SCRIPTS`](#index-_005fSCRIPTS) | [Scripts](#Scripts) |
| [`_SOURCES`](#index-_005fSOURCES) | [Program Sources](#Program-Sources) |
| [`_SOURCES`](#index-_005fSOURCES-1) | [Program Sources](#Program-Sources) |
| [`_SOURCES`](#index-_005fSOURCES-2) | [Default \_SOURCES](#Default-_005fSOURCES) |
| [`_TEXINFOS`](#index-_005fTEXINFOS) | [Texinfo](#Texinfo) |
| [`_TEXINFOS`](#index-_005fTEXINFOS-1) | [Texinfo](#Texinfo) |
| --- | |
| A | |
| [`A68`](#index-A68) | [Algol 68 Support](#Algol-68-Support) |
| [`A68COMPILE`](#index-A68COMPILE) | [Algol 68 Support](#Algol-68-Support) |
| [`A68FLAGS`](#index-A68FLAGS) | [Algol 68 Support](#Algol-68-Support) |
| [`A68LINK`](#index-A68LINK) | [Algol 68 Support](#Algol-68-Support) |
| [`ACLOCAL_AUTOMAKE_DIR`](#index-ACLOCAL_005fAUTOMAKE_005fDIR) | [aclocal Options](#aclocal-Options) |
| [`ACLOCAL_PATH`](#index-ACLOCAL_005fPATH-1) | [aclocal Options](#aclocal-Options) |
| [`ALLOCA`](#index-ALLOCA) | [LTLIBOBJS](#LTLIBOBJS) |
| [`ALLOCA`](#index-ALLOCA-1) | [LIBOBJS](#LIBOBJS) |
| [`AM_A68FLAGS`](#index-AM_005fA68FLAGS) | [Algol 68 Support](#Algol-68-Support) |
| [`AM_CCASFLAGS`](#index-AM_005fCCASFLAGS) | [Assembly Support](#Assembly-Support) |
| [`AM_CFLAGS`](#index-AM_005fCFLAGS) | [Program Variables](#Program-Variables) |
| [`AM_COLOR_TESTS`](#index-AM_005fCOLOR_005fTESTS) | [Scripts-based Testsuites](#Scripts_002dbased-Testsuites) |
| [`AM_CPPFLAGS`](#index-AM_005fCPPFLAGS) | [Program Variables](#Program-Variables) |
| [`AM_CPPFLAGS`](#index-AM_005fCPPFLAGS-1) | [Assembly Support](#Assembly-Support) |
| [`AM_CXXFLAGS`](#index-AM_005fCXXFLAGS) | [C++ Support](#C_002b_002b-Support) |
| [`AM_DEFAULT_SOURCE_EXT`](#index-AM_005fDEFAULT_005fSOURCE_005fEXT) | [Default \_SOURCES](#Default-_005fSOURCES) |
| [`AM_DEFAULT_V`](#index-AM_005fDEFAULT_005fV) | [Automake Silent Rules](#Automake-Silent-Rules) |
| [`AM_DEFAULT_VERBOSITY`](#index-AM_005fDEFAULT_005fVERBOSITY) | [Automake Silent Rules](#Automake-Silent-Rules) |
| [`AM_DIST_TAR_IGNORE_STDERR`](#index-AM_005fDIST_005fTAR_005fIGNORE_005fSTDERR) | [The Types of Distributions](#The-Types-of-Distributions) |
| [`AM_DISTCHECK_CONFIGURE_FLAGS`](#index-AM_005fDISTCHECK_005fCONFIGURE_005fFLAGS) | [DISTCHECK\_CONFIGURE\_FLAGS](#DISTCHECK_005fCONFIGURE_005fFLAGS) |
| [`AM_DISTCHECK_DVI_TARGET`](#index-AM_005fDISTCHECK_005fDVI_005fTARGET) | [dvi and distcheck](#dvi-and-distcheck) |
| [`AM_ETAGSFLAGS`](#index-AM_005fETAGSFLAGS) | [Tags](#Tags) |
| [`AM_ext_LOG_DRIVER_FLAGS`](#index-AM_005fext_005fLOG_005fDRIVER_005fFLAGS) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`AM_ext_LOG_FLAGS`](#index-AM_005fext_005fLOG_005fFLAGS) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`AM_FCFLAGS`](#index-AM_005fFCFLAGS) | [Fortran 9x Support](#Fortran-9x-Support) |
| [`AM_FFLAGS`](#index-AM_005fFFLAGS) | [Fortran 77 Support](#Fortran-77-Support) |
| [`AM_GCJFLAGS`](#index-AM_005fGCJFLAGS) | [Java Support with gcj](#Java-Support-with-gcj) |
| [`AM_INSTALLCHECK_STD_OPTIONS_EXEMPT`](#index-AM_005fINSTALLCHECK_005fSTD_005fOPTIONS_005fEXEMPT) | [List of Automake options](#List-of-Automake-options) |
| [`AM_JAVACFLAGS`](#index-AM_005fJAVACFLAGS) | [Java](#Java) |
| [`AM_LDFLAGS`](#index-AM_005fLDFLAGS) | [Linking](#Linking) |
| [`AM_LDFLAGS`](#index-AM_005fLDFLAGS-1) | [Program Variables](#Program-Variables) |
| [`AM_LFLAGS`](#index-AM_005fLFLAGS) | [Yacc and Lex](#Yacc-and-Lex) |
| [`AM_LIBTOOLFLAGS`](#index-AM_005fLIBTOOLFLAGS) | [Libtool Flags](#Libtool-Flags) |
| [`AM_LOG_DRIVER_FLAGS`](#index-AM_005fLOG_005fDRIVER_005fFLAGS) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`AM_LOG_FLAGS`](#index-AM_005fLOG_005fFLAGS) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`AM_MAKEFLAGS`](#index-AM_005fMAKEFLAGS) | [Subdirectories](#Subdirectories) |
| [`AM_MAKEINFOFLAGS`](#index-AM_005fMAKEINFOFLAGS) | [Texinfo](#Texinfo) |
| [`AM_MAKEINFOHTMLFLAGS`](#index-AM_005fMAKEINFOHTMLFLAGS) | [Texinfo](#Texinfo) |
| [`AM_OBJCFLAGS`](#index-AM_005fOBJCFLAGS) | [Objective C Support](#Objective-C-Support) |
| [`AM_OBJCXXFLAGS`](#index-AM_005fOBJCXXFLAGS) | [Objective C++ Support](#Objective-C_002b_002b-Support) |
| [`AM_RFLAGS`](#index-AM_005fRFLAGS) | [Fortran 77 Support](#Fortran-77-Support) |
| [`AM_RUNTESTFLAGS`](#index-AM_005fRUNTESTFLAGS) | [DejaGnu Tests](#DejaGnu-Tests) |
| [`AM_TESTS_ENVIRONMENT`](#index-AM_005fTESTS_005fENVIRONMENT) | [Testsuite Environment Overrides](#Testsuite-Environment-Overrides) |
| [`AM_TESTS_FD_REDIRECT`](#index-AM_005fTESTS_005fFD_005fREDIRECT) | [Testsuite Environment Overrides](#Testsuite-Environment-Overrides) |
| [`AM_TESTSUITE_SUMMARY_HEADER`](#index-AM_005fTESTSUITE_005fSUMMARY_005fHEADER) | [Scripts-based Testsuites](#Scripts_002dbased-Testsuites) |
| [`AM_TEXI2FLAGS`](#index-AM_005fTEXI2FLAGS) | [Texinfo](#Texinfo) |
| [`AM_UPCFLAGS`](#index-AM_005fUPCFLAGS) | [Unified Parallel C Support](#Unified-Parallel-C-Support) |
| [`AM_UPDATE_INFO_DIR`](#index-AM_005fUPDATE_005fINFO_005fDIR) | [Texinfo](#Texinfo) |
| [`AM_V`](#index-AM_005fV) | [Automake Silent Rules](#Automake-Silent-Rules) |
| [`AM_V_at`](#index-AM_005fV_005fat) | [Automake Silent Rules](#Automake-Silent-Rules) |
| [`AM_V_GEN`](#index-AM_005fV_005fGEN) | [Automake Silent Rules](#Automake-Silent-Rules) |
| [`AM_VALAFLAGS`](#index-AM_005fVALAFLAGS) | [Vala Support](#Vala-Support) |
| [`AM_YFLAGS`](#index-AM_005fYFLAGS) | [Yacc and Lex](#Yacc-and-Lex) |
| [`AR`](#index-AR) | [Public Macros](#Public-Macros) |
| [`AUTOCONF`](#index-AUTOCONF) | [automake Invocation](#automake-Invocation) |
| [`AUTOM4TE`](#index-AUTOM4TE) | [aclocal Invocation](#aclocal-Invocation) |
| [`AUTOMAKE_JOBS`](#index-AUTOMAKE_005fJOBS) | [automake Invocation](#automake-Invocation) |
| [`AUTOMAKE_LIBDIR`](#index-AUTOMAKE_005fLIBDIR) | [automake Invocation](#automake-Invocation) |
| [`AUTOMAKE_OPTIONS`](#index-AUTOMAKE_005fOPTIONS) | [Public Macros](#Public-Macros) |
| [`AUTOMAKE_OPTIONS`](#index-AUTOMAKE_005fOPTIONS-1) | [Dependencies](#Dependencies) |
| [`AUTOMAKE_OPTIONS`](#index-AUTOMAKE_005fOPTIONS-2) | [List of Automake options](#List-of-Automake-options) |
| --- | |
| B | |
| [`bin_PROGRAMS`](#index-bin_005fPROGRAMS) | [Program Sources](#Program-Sources) |
| [`bin_SCRIPTS`](#index-bin_005fSCRIPTS) | [Scripts](#Scripts) |
| [`build_triplet`](#index-build_005ftriplet) | [Optional](#Optional) |
| [`BUILT_SOURCES`](#index-BUILT_005fSOURCES) | [Sources](#Sources) |
| [`BUILT_SOURCES, and ‘dist’ target`](#index-BUILT_005fSOURCES_002c-and-dist-target) | [List of Automake options](#List-of-Automake-options) |
| [`BZIP2`](#index-BZIP2) | [The Types of Distributions](#The-Types-of-Distributions) |
| [`BZIP3`](#index-BZIP3) | [The Types of Distributions](#The-Types-of-Distributions) |
| --- | |
| C | |
| [`CC`](#index-CC) | [Program Variables](#Program-Variables) |
| [`CCAS`](#index-CCAS) | [Public Macros](#Public-Macros) |
| [`CCAS`](#index-CCAS-1) | [Assembly Support](#Assembly-Support) |
| [`CCASFLAGS`](#index-CCASFLAGS) | [Public Macros](#Public-Macros) |
| [`CCASFLAGS`](#index-CCASFLAGS-1) | [Assembly Support](#Assembly-Support) |
| [`CFLAGS`](#index-CFLAGS) | [Program Variables](#Program-Variables) |
| [`check_`](#index-check_005f) | [Uniform](#Uniform) |
| [`check_LTLIBRARIES`](#index-check_005fLTLIBRARIES) | [Libtool Convenience Libraries](#Libtool-Convenience-Libraries) |
| [`check_PROGRAMS`](#index-check_005fPROGRAMS) | [Program Sources](#Program-Sources) |
| [`check_PROGRAMS`](#index-check_005fPROGRAMS-1) | [Default \_SOURCES](#Default-_005fSOURCES) |
| [`check_PROGRAMS`](#index-check_005fPROGRAMS-2) | [Scripts-based Testsuites](#Scripts_002dbased-Testsuites) |
| [`check_SCRIPTS`](#index-check_005fSCRIPTS) | [Scripts](#Scripts) |
| [`CLASSPATH_ENV`](#index-CLASSPATH_005fENV) | [Java](#Java) |
| [`CLEANFILES`](#index-CLEANFILES) | [Clean](#Clean) |
| [`COMPILE`](#index-COMPILE) | [Program Variables](#Program-Variables) |
| [`CONFIG_STATUS_DEPENDENCIES`](#index-CONFIG_005fSTATUS_005fDEPENDENCIES) | [Rebuilding](#Rebuilding) |
| [`CONFIGURE_DEPENDENCIES`](#index-CONFIGURE_005fDEPENDENCIES) | [Rebuilding](#Rebuilding) |
| [`CPPFLAGS`](#index-CPPFLAGS) | [Program Variables](#Program-Variables) |
| [`CPPFLAGS`](#index-CPPFLAGS-1) | [Assembly Support](#Assembly-Support) |
| [`CSCOPE`](#index-CSCOPE) | [Tags](#Tags) |
| [`CSCOPE_ARGS`](#index-CSCOPE_005fARGS) | [Tags](#Tags) |
| [`CSCOPEFLAGS`](#index-CSCOPEFLAGS) | [Tags](#Tags) |
| [`CTAGS`](#index-CTAGS) | [Tags](#Tags) |
| [`CTAGS_ARGS`](#index-CTAGS_005fARGS) | [Tags](#Tags) |
| [`CTAGSFLAGS`](#index-CTAGSFLAGS) | [Tags](#Tags) |
| [`CXX`](#index-CXX) | [C++ Support](#C_002b_002b-Support) |
| [`CXXCOMPILE`](#index-CXXCOMPILE) | [C++ Support](#C_002b_002b-Support) |
| [`CXXFLAGS`](#index-CXXFLAGS) | [C++ Support](#C_002b_002b-Support) |
| [`CXXLINK`](#index-CXXLINK) | [C++ Support](#C_002b_002b-Support) |
| [`CXXLINK`](#index-CXXLINK-1) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| --- | |
| D | |
| [`DATA`](#index-DATA) | [Uniform](#Uniform) |
| [`DATA`](#index-DATA-1) | [Data](#Data) |
| [`data_DATA`](#index-data_005fDATA) | [Data](#Data) |
| [`DEFS`](#index-DEFS) | [Program Variables](#Program-Variables) |
| [`DEJATOOL`](#index-DEJATOOL) | [DejaGnu Tests](#DejaGnu-Tests) |
| [`DESTDIR`](#index-DESTDIR) | [DESTDIR](#DESTDIR) |
| [`DESTDIR`](#index-DESTDIR-1) | [Staged Installs](#Staged-Installs) |
| [`DISABLE_HARD_ERRORS`](#index-DISABLE_005fHARD_005fERRORS) | [Scripts-based Testsuites](#Scripts_002dbased-Testsuites) |
| [`dist_`](#index-dist_005f) | [Alternative](#Alternative) |
| [`dist_`](#index-dist_005f-1) | [Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control) |
| [`dist_lisp_LISP`](#index-dist_005flisp_005fLISP) | [Emacs Lisp](#Emacs-Lisp) |
| [`dist_noinst_LISP`](#index-dist_005fnoinst_005fLISP) | [Emacs Lisp](#Emacs-Lisp) |
| [`DIST_SUBDIRS`](#index-DIST_005fSUBDIRS) | [Subdirectories with AM\_CONDITIONAL](#Subdirectories-with-AM_005fCONDITIONAL) |
| [`DIST_SUBDIRS`](#index-DIST_005fSUBDIRS-1) | [Basics of Distribution](#Basics-of-Distribution) |
| [`DISTCHECK_CONFIGURE_FLAGS`](#index-DISTCHECK_005fCONFIGURE_005fFLAGS) | [DISTCHECK\_CONFIGURE\_FLAGS](#DISTCHECK_005fCONFIGURE_005fFLAGS) |
| [`distcleancheck_listfiles`](#index-distcleancheck_005flistfiles) | [distcleancheck](#distcleancheck) |
| [`distcleancheck_listfiles`](#index-distcleancheck_005flistfiles-1) | [Errors with distclean](#Errors-with-distclean) |
| [`DISTCLEANFILES`](#index-DISTCLEANFILES) | [Clean](#Clean) |
| [`DISTCLEANFILES`](#index-DISTCLEANFILES-1) | [distcleancheck](#distcleancheck) |
| [`distdir`](#index-distdir-1) | [The dist Hook](#The-dist-Hook) |
| [`distdir`](#index-distdir-2) | [Third-Party Makefiles](#Third_002dParty-Makefiles) |
| [`distuninstallcheck_listfiles`](#index-distuninstallcheck_005flistfiles) | [distuninstallcheck](#distuninstallcheck) |
| [`doc_DATA`](#index-doc_005fDATA) | [Data](#Data) |
| [`DVIPS`](#index-DVIPS) | [Texinfo](#Texinfo) |
| --- | |
| E | |
| [`EMACS`](#index-EMACS) | [Public Macros](#Public-Macros) |
| [`EMPTY_AUTOMAKE_TARGETS`](#index-EMPTY_005fAUTOMAKE_005fTARGETS) | [Third-Party Makefiles](#Third_002dParty-Makefiles) |
| [`ETAGS`](#index-ETAGS) | [Tags](#Tags) |
| [`ETAGS_ARGS`](#index-ETAGS_005fARGS) | [Tags](#Tags) |
| [`ETAGSFLAGS`](#index-ETAGSFLAGS) | [Tags](#Tags) |
| [`EXPECT`](#index-EXPECT) | [DejaGnu Tests](#DejaGnu-Tests) |
| [`ext_LOG_COMPILE`](#index-ext_005fLOG_005fCOMPILE) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`ext_LOG_COMPILER`](#index-ext_005fLOG_005fCOMPILER) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`ext_LOG_DRIVER`](#index-ext_005fLOG_005fDRIVER) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`ext_LOG_DRIVER_FLAGS`](#index-ext_005fLOG_005fDRIVER_005fFLAGS) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`ext_LOG_FLAGS`](#index-ext_005fLOG_005fFLAGS) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`EXTRA_DIST`](#index-EXTRA_005fDIST) | [Basics of Distribution](#Basics-of-Distribution) |
| [`EXTRA_maude_DEPENDENCIES`](#index-EXTRA_005fmaude_005fDEPENDENCIES) | [Linking](#Linking) |
| [`EXTRA_maude_DEPENDENCIES`](#index-EXTRA_005fmaude_005fDEPENDENCIES-1) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`EXTRA_maude_SOURCES`](#index-EXTRA_005fmaude_005fSOURCES) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`EXTRA_PROGRAMS`](#index-EXTRA_005fPROGRAMS) | [Conditional Programs](#Conditional-Programs) |
| --- | |
| F | |
| [`F77`](#index-F77) | [Fortran 77 Support](#Fortran-77-Support) |
| [`F77COMPILE`](#index-F77COMPILE) | [Fortran 77 Support](#Fortran-77-Support) |
| [`F77LINK`](#index-F77LINK) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| [`FC`](#index-FC) | [Fortran 9x Support](#Fortran-9x-Support) |
| [`FCCOMPILE`](#index-FCCOMPILE) | [Fortran 9x Support](#Fortran-9x-Support) |
| [`FCFLAGS`](#index-FCFLAGS) | [Fortran 9x Support](#Fortran-9x-Support) |
| [`FCLINK`](#index-FCLINK) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| [`FCLINK`](#index-FCLINK-1) | [Fortran 9x Support](#Fortran-9x-Support) |
| [`FFLAGS`](#index-FFLAGS) | [Fortran 77 Support](#Fortran-77-Support) |
| [`FLIBS`](#index-FLIBS) | [Mixing Fortran 77 With C and C++](#Mixing-Fortran-77-With-C-and-C_002b_002b) |
| [`FLINK`](#index-FLINK) | [Fortran 77 Support](#Fortran-77-Support) |
| --- | |
| G | |
| [`GCJ`](#index-GCJ) | [Public Macros](#Public-Macros) |
| [`GCJFLAGS`](#index-GCJFLAGS) | [Public Macros](#Public-Macros) |
| [`GCJFLAGS`](#index-GCJFLAGS-1) | [Java Support with gcj](#Java-Support-with-gcj) |
| [`GCJLINK`](#index-GCJLINK) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| [`GTAGS_ARGS`](#index-GTAGS_005fARGS) | [Tags](#Tags) |
| [`GZIP_ENV`](#index-GZIP_005fENV) | [The Types of Distributions](#The-Types-of-Distributions) |
| --- | |
| H | |
| [`HEADERS`](#index-HEADERS) | [Uniform](#Uniform) |
| [`host_triplet`](#index-host_005ftriplet) | [Optional](#Optional) |
| --- | |
| I | |
| [`IGNORE_SKIPPED_LOGS`](#index-IGNORE_005fSKIPPED_005fLOGS) | [Parallel Test Basics](#Parallel-Test-Basics) |
| [`include_HEADERS`](#index-include_005fHEADERS) | [Headers](#Headers) |
| [`INCLUDES`](#index-INCLUDES) | [Program Variables](#Program-Variables) |
| [`info_TEXINFOS`](#index-info_005fTEXINFOS) | [Texinfo](#Texinfo) |
| --- | |
| J | |
| [`JAVA`](#index-JAVA) | [Uniform](#Uniform) |
| [`JAVAC`](#index-JAVAC) | [Java](#Java) |
| [`JAVACFLAGS`](#index-JAVACFLAGS) | [Java](#Java) |
| [`JAVAROOT`](#index-JAVAROOT) | [Java](#Java) |
| --- | |
| L | |
| [`LDADD`](#index-LDADD) | [Linking](#Linking) |
| [`LDFLAGS`](#index-LDFLAGS) | [Program Variables](#Program-Variables) |
| [`LFLAGS`](#index-LFLAGS) | [Yacc and Lex](#Yacc-and-Lex) |
| [`lib_LIBRARIES`](#index-lib_005fLIBRARIES) | [A Library](#A-Library) |
| [`lib_LTLIBRARIES`](#index-lib_005fLTLIBRARIES) | [Libtool Libraries](#Libtool-Libraries) |
| [`libexec_PROGRAMS`](#index-libexec_005fPROGRAMS) | [Program Sources](#Program-Sources) |
| [`libexec_SCRIPTS`](#index-libexec_005fSCRIPTS) | [Scripts](#Scripts) |
| [`LIBOBJS`](#index-LIBOBJS) | [Optional](#Optional) |
| [`LIBOBJS`](#index-LIBOBJS-1) | [LTLIBOBJS](#LTLIBOBJS) |
| [`LIBOBJS`](#index-LIBOBJS-2) | [LIBOBJS](#LIBOBJS) |
| [`LIBRARIES`](#index-LIBRARIES) | [Uniform](#Uniform) |
| [`LIBS`](#index-LIBS) | [Program Variables](#Program-Variables) |
| [`LIBTOOLFLAGS`](#index-LIBTOOLFLAGS) | [Libtool Flags](#Libtool-Flags) |
| [`LINK`](#index-LINK) | [Program Variables](#Program-Variables) |
| [`LINK`](#index-LINK-1) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| [`LISP`](#index-LISP) | [Uniform](#Uniform) |
| [`lisp_DATA`](#index-lisp_005fDATA) | [Data](#Data) |
| [`lisp_LISP`](#index-lisp_005fLISP) | [Emacs Lisp](#Emacs-Lisp) |
| [`lispdir`](#index-lispdir) | [Public Macros](#Public-Macros) |
| [`localstate_DATA`](#index-localstate_005fDATA) | [Data](#Data) |
| [`LOG_COMPILE`](#index-LOG_005fCOMPILE) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`LOG_COMPILER`](#index-LOG_005fCOMPILER) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`LOG_DRIVER`](#index-LOG_005fDRIVER) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`LOG_DRIVER_FLAGS`](#index-LOG_005fDRIVER_005fFLAGS) | [Declaring Custom Test Drivers](#Declaring-Custom-Test-Drivers) |
| [`LOG_FLAGS`](#index-LOG_005fFLAGS) | [Parallel Test Runners](#Parallel-Test-Runners) |
| [`LTALLOCA`](#index-LTALLOCA) | [LTLIBOBJS](#LTLIBOBJS) |
| [`LTALLOCA`](#index-LTALLOCA-1) | [LIBOBJS](#LIBOBJS) |
| [`LTLIBOBJS`](#index-LTLIBOBJS) | [LTLIBOBJS](#LTLIBOBJS) |
| [`LTLIBOBJS`](#index-LTLIBOBJS-1) | [LIBOBJS](#LIBOBJS) |
| [`LTLIBRARIES`](#index-LTLIBRARIES) | [Uniform](#Uniform) |
| [`LZIP_OPT`](#index-LZIP_005fOPT) | [The Types of Distributions](#The-Types-of-Distributions) |
| --- | |
| M | |
| [`MAINTAINERCLEANFILES`](#index-MAINTAINERCLEANFILES) | [Clean](#Clean) |
| [`MAKE`](#index-MAKE) | [Subdirectories](#Subdirectories) |
| [`MAKEINFO`](#index-MAKEINFO) | [Texinfo](#Texinfo) |
| [`MAKEINFOFLAGS`](#index-MAKEINFOFLAGS) | [Texinfo](#Texinfo) |
| [`MAKEINFOHTML`](#index-MAKEINFOHTML) | [Texinfo](#Texinfo) |
| [`man_MANS`](#index-man_005fMANS) | [Man Pages](#Man-Pages) |
| [`MANS`](#index-MANS) | [Uniform](#Uniform) |
| [`maude_AR`](#index-maude_005fAR) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_CCASFLAGS`](#index-maude_005fCCASFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_CFLAGS`](#index-maude_005fCFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_CPPFLAGS`](#index-maude_005fCPPFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_CXXFLAGS`](#index-maude_005fCXXFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_DEPENDENCIES`](#index-maude_005fDEPENDENCIES) | [Linking](#Linking) |
| [`maude_DEPENDENCIES`](#index-maude_005fDEPENDENCIES-1) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_FFLAGS`](#index-maude_005fFFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_GCJFLAGS`](#index-maude_005fGCJFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_LDADD`](#index-maude_005fLDADD) | [Linking](#Linking) |
| [`maude_LDADD`](#index-maude_005fLDADD-1) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_LDFLAGS`](#index-maude_005fLDFLAGS) | [Linking](#Linking) |
| [`maude_LDFLAGS`](#index-maude_005fLDFLAGS-1) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_LFLAGS`](#index-maude_005fLFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_LIBADD`](#index-maude_005fLIBADD) | [A Library](#A-Library) |
| [`maude_LIBADD`](#index-maude_005fLIBADD-1) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_LIBTOOLFLAGS`](#index-maude_005fLIBTOOLFLAGS) | [Libtool Flags](#Libtool-Flags) |
| [`maude_LIBTOOLFLAGS`](#index-maude_005fLIBTOOLFLAGS-1) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_LINK`](#index-maude_005fLINK) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_OBJCFLAGS`](#index-maude_005fOBJCFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_OBJCXXFLAGS`](#index-maude_005fOBJCXXFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_RANLIB`](#index-maude_005fRANLIB) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_RFLAGS`](#index-maude_005fRFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_SHORTNAME`](#index-maude_005fSHORTNAME) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_SOURCES`](#index-maude_005fSOURCES) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_UPCFLAGS`](#index-maude_005fUPCFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`maude_YFLAGS`](#index-maude_005fYFLAGS) | [Program and Library Variables](#Program-and-Library-Variables) |
| [`MISSING`](#index-MISSING) | [Public Macros](#Public-Macros) |
| [`MKDIR_P`](#index-MKDIR_005fP) | [Obsolete Macros](#Obsolete-Macros) |
| [`mkdir_p`](#index-mkdir_005fp) | [Obsolete Macros](#Obsolete-Macros) |
| [`MOSTLYCLEANFILES`](#index-MOSTLYCLEANFILES) | [Clean](#Clean) |
| --- | |
| N | |
| [`nobase_`](#index-nobase_005f) | [Alternative](#Alternative) |
| [`nodist_`](#index-nodist_005f) | [Alternative](#Alternative) |
| [`nodist_`](#index-nodist_005f-1) | [Fine-grained Distribution Control](#Fine_002dgrained-Distribution-Control) |
| [`noinst_`](#index-noinst_005f) | [Uniform](#Uniform) |
| [`noinst_HEADERS`](#index-noinst_005fHEADERS) | [Headers](#Headers) |
| [`noinst_HEADERS`](#index-noinst_005fHEADERS-1) | [Headers](#Headers) |
| [`noinst_LIBRARIES`](#index-noinst_005fLIBRARIES) | [A Library](#A-Library) |
| [`noinst_LISP`](#index-noinst_005fLISP) | [Emacs Lisp](#Emacs-Lisp) |
| [`noinst_LTLIBRARIES`](#index-noinst_005fLTLIBRARIES) | [Libtool Convenience Libraries](#Libtool-Convenience-Libraries) |
| [`noinst_PROGRAMS`](#index-noinst_005fPROGRAMS) | [Program Sources](#Program-Sources) |
| [`noinst_SCRIPTS`](#index-noinst_005fSCRIPTS) | [Scripts](#Scripts) |
| [`notrans_`](#index-notrans_005f) | [Man Pages](#Man-Pages) |
| --- | |
| O | |
| [`OBJC`](#index-OBJC) | [Objective C Support](#Objective-C-Support) |
| [`OBJCCOMPILE`](#index-OBJCCOMPILE) | [Objective C Support](#Objective-C-Support) |
| [`OBJCFLAGS`](#index-OBJCFLAGS) | [Objective C Support](#Objective-C-Support) |
| [`OBJCLINK`](#index-OBJCLINK) | [Objective C Support](#Objective-C-Support) |
| [`OBJCLINK`](#index-OBJCLINK-1) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| [`OBJCXX`](#index-OBJCXX) | [Objective C++ Support](#Objective-C_002b_002b-Support) |
| [`OBJCXXCOMPILE`](#index-OBJCXXCOMPILE) | [Objective C++ Support](#Objective-C_002b_002b-Support) |
| [`OBJCXXFLAGS`](#index-OBJCXXFLAGS) | [Objective C++ Support](#Objective-C_002b_002b-Support) |
| [`OBJCXXLINK`](#index-OBJCXXLINK) | [Objective C++ Support](#Objective-C_002b_002b-Support) |
| [`OBJCXXLINK`](#index-OBJCXXLINK-1) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| [`oldinclude_HEADERS`](#index-oldinclude_005fHEADERS) | [Headers](#Headers) |
| --- | |
| P | |
| [`PACKAGE`](#index-PACKAGE) | [Basics of Distribution](#Basics-of-Distribution) |
| [`pkgdata_DATA`](#index-pkgdata_005fDATA) | [Data](#Data) |
| [`pkgdata_SCRIPTS`](#index-pkgdata_005fSCRIPTS) | [Scripts](#Scripts) |
| [`pkgdatadir`](#index-pkgdatadir) | [Uniform](#Uniform) |
| [`pkginclude_HEADERS`](#index-pkginclude_005fHEADERS) | [Headers](#Headers) |
| [`pkgincludedir`](#index-pkgincludedir) | [Uniform](#Uniform) |
| [`pkglib_LIBRARIES`](#index-pkglib_005fLIBRARIES) | [A Library](#A-Library) |
| [`pkglib_LTLIBRARIES`](#index-pkglib_005fLTLIBRARIES) | [Libtool Libraries](#Libtool-Libraries) |
| [`pkglibdir`](#index-pkglibdir) | [Uniform](#Uniform) |
| [`pkglibexec_PROGRAMS`](#index-pkglibexec_005fPROGRAMS) | [Program Sources](#Program-Sources) |
| [`pkglibexec_SCRIPTS`](#index-pkglibexec_005fSCRIPTS) | [Scripts](#Scripts) |
| [`pkglibexecdir`](#index-pkglibexecdir) | [Uniform](#Uniform) |
| [`pkgpyexecdir`](#index-pkgpyexecdir) | [Python](#Python) |
| [`pkgpythondir`](#index-pkgpythondir) | [Python](#Python) |
| [`PROGRAMS`](#index-PROGRAMS) | [Uniform](#Uniform) |
| [`PROGRAMS`](#index-PROGRAMS-1) | [Uniform](#Uniform) |
| [`pyexecdir`](#index-pyexecdir) | [Python](#Python) |
| [`PYTHON`](#index-PYTHON) | [Uniform](#Uniform) |
| [`PYTHON`](#index-PYTHON-1) | [Python](#Python) |
| [`PYTHON_EXEC_PREFIX`](#index-PYTHON_005fEXEC_005fPREFIX) | [Python](#Python) |
| [`PYTHON_PLATFORM`](#index-PYTHON_005fPLATFORM) | [Python](#Python) |
| [`PYTHON_PREFIX`](#index-PYTHON_005fPREFIX) | [Python](#Python) |
| [`PYTHON_VERSION`](#index-PYTHON_005fVERSION) | [Python](#Python) |
| [`pythondir`](#index-pythondir) | [Python](#Python) |
| --- | |
| R | |
| [`RECHECK_LOGS`](#index-RECHECK_005fLOGS) | [Parallel Test Selection](#Parallel-Test-Selection) |
| [`RFLAGS`](#index-RFLAGS) | [Fortran 77 Support](#Fortran-77-Support) |
| [`RUNTEST`](#index-RUNTEST) | [DejaGnu Tests](#DejaGnu-Tests) |
| [`RUNTESTDEFAULTFLAGS`](#index-RUNTESTDEFAULTFLAGS) | [DejaGnu Tests](#DejaGnu-Tests) |
| [`RUNTESTFLAGS`](#index-RUNTESTFLAGS) | [DejaGnu Tests](#DejaGnu-Tests) |
| --- | |
| S | |
| [`sbin_PROGRAMS`](#index-sbin_005fPROGRAMS) | [Program Sources](#Program-Sources) |
| [`sbin_SCRIPTS`](#index-sbin_005fSCRIPTS) | [Scripts](#Scripts) |
| [`SCRIPTS`](#index-SCRIPTS) | [Uniform](#Uniform) |
| [`SCRIPTS`](#index-SCRIPTS-1) | [Scripts](#Scripts) |
| [`sharedstate_DATA`](#index-sharedstate_005fDATA) | [Data](#Data) |
| [`SOURCES`](#index-SOURCES) | [Program Sources](#Program-Sources) |
| [`SOURCES`](#index-SOURCES-1) | [Default \_SOURCES](#Default-_005fSOURCES) |
| [`SUBDIRS`](#index-SUBDIRS) | [Subdirectories](#Subdirectories) |
| [`SUBDIRS`](#index-SUBDIRS-1) | [Basics of Distribution](#Basics-of-Distribution) |
| [`SUBDIRS, setting when selecting tests`](#index-SUBDIRS_002c-setting-when-selecting-tests) | [Parallel Test Selection](#Parallel-Test-Selection) |
| [`SUFFIXES`](#index-SUFFIXES) | [Suffixes](#Suffixes) |
| [`sys.exec_prefix Python variable`](#index-sys_002eexec_005fprefix-Python-variable) | [Python](#Python) |
| [`sys.platform Python variable`](#index-sys_002eplatform-Python-variable) | [Python](#Python) |
| [`sys.prefix Python variable`](#index-sys_002eprefix-Python-variable) | [Python](#Python) |
| [`sys.version_info Python variable`](#index-sys_002eversion_005finfo-Python-variable) | [Python](#Python) |
| [`sysconf_DATA`](#index-sysconf_005fDATA) | [Data](#Data) |
| --- | |
| T | |
| [`TAGS_DEPENDENCIES`](#index-TAGS_005fDEPENDENCIES) | [Tags](#Tags) |
| [`TAR`](#index-TAR) | [Basics of Distribution](#Basics-of-Distribution) |
| [`TAR_OPTIONS`](#index-TAR_005fOPTIONS) | [Basics of Distribution](#Basics-of-Distribution) |
| [`target_triplet`](#index-target_005ftriplet) | [Optional](#Optional) |
| [`TEST_EXTENSIONS`](#index-TEST_005fEXTENSIONS) | [Parallel Test Basics](#Parallel-Test-Basics) |
| [`TEST_LOGS`](#index-TEST_005fLOGS) | [Parallel Test Basics](#Parallel-Test-Basics) |
| [`TEST_SUITE_LOG`](#index-TEST_005fSUITE_005fLOG) | [Parallel Test Basics](#Parallel-Test-Basics) |
| [`TESTS`](#index-TESTS) | [Scripts-based Testsuites](#Scripts_002dbased-Testsuites) |
| [`TESTS`](#index-TESTS-1) | [Parallel Test Basics](#Parallel-Test-Basics) |
| [`TESTS_ENVIRONMENT`](#index-TESTS_005fENVIRONMENT) | [Testsuite Environment Overrides](#Testsuite-Environment-Overrides) |
| [`TEXI2DVI`](#index-TEXI2DVI) | [Texinfo](#Texinfo) |
| [`TEXI2PDF`](#index-TEXI2PDF) | [Texinfo](#Texinfo) |
| [`TEXINFO_TEX`](#index-TEXINFO_005fTEX) | [Texinfo](#Texinfo) |
| [`TEXINFOS`](#index-TEXINFOS) | [Uniform](#Uniform) |
| [`TEXINFOS`](#index-TEXINFOS-1) | [Texinfo](#Texinfo) |
| [`top_distdir`](#index-top_005fdistdir) | [The dist Hook](#The-dist-Hook) |
| [`top_distdir`](#index-top_005fdistdir-1) | [Third-Party Makefiles](#Third_002dParty-Makefiles) |
| --- | |
| U | |
| [`UPC`](#index-UPC) | [Public Macros](#Public-Macros) |
| [`UPC`](#index-UPC-1) | [Unified Parallel C Support](#Unified-Parallel-C-Support) |
| [`UPCCOMPILE`](#index-UPCCOMPILE) | [Unified Parallel C Support](#Unified-Parallel-C-Support) |
| [`UPCFLAGS`](#index-UPCFLAGS) | [Unified Parallel C Support](#Unified-Parallel-C-Support) |
| [`UPCLINK`](#index-UPCLINK) | [Unified Parallel C Support](#Unified-Parallel-C-Support) |
| [`UPCLINK`](#index-UPCLINK-1) | [How the Linker is Chosen](#How-the-Linker-is-Chosen) |
| --- | |
| V | |
| [`V`](#index-V) | [Automake Silent Rules](#Automake-Silent-Rules) |
| [`VALAC`](#index-VALAC) | [Vala Support](#Vala-Support) |
| [`VALAFLAGS`](#index-VALAFLAGS) | [Vala Support](#Vala-Support) |
| [`VERBOSE`](#index-VERBOSE) | [Parallel Test Basics](#Parallel-Test-Basics) |
| [`VERSION`](#index-VERSION) | [Basics of Distribution](#Basics-of-Distribution) |
| --- | |
| W | |
| [`WARNINGS`](#index-WARNINGS) | [automake Invocation](#automake-Invocation) |
| [`WARNINGS`](#index-WARNINGS-1) | [aclocal Options](#aclocal-Options) |
| [`WITH_DMALLOC`](#index-WITH_005fDMALLOC) | [Public Macros](#Public-Macros) |
| --- | |
| X | |
| [`XFAIL_TESTS`](#index-XFAIL_005fTESTS) | [Scripts-based Testsuites](#Scripts_002dbased-Testsuites) |
| [`XZ_OPT`](#index-XZ_005fOPT) | [The Types of Distributions](#The-Types-of-Distributions) |
| --- | |
| Y | |
| [`YACC`](#index-YACC) | [Optional](#Optional) |
| [`YFLAGS`](#index-YFLAGS) | [Yacc and Lex](#Yacc-and-Lex) |
| --- | |
| Z | |
| [`ZSTD_CLEVEL`](#index-ZSTD_005fCLEVEL) | [The Types of Distributions](#The-Types-of-Distributions) |
| [`ZSTD_OPT`](#index-ZSTD_005fOPT) | [The Types of Distributions](#The-Types-of-Distributions) |
| --- | |

|  |  |
| --- | --- |
| Jump to: | [**\_**](#Variable-Index_vr_symbol-1)     [**A**](#Variable-Index_vr_letter-A)   [**B**](#Variable-Index_vr_letter-B)   [**C**](#Variable-Index_vr_letter-C)   [**D**](#Variable-Index_vr_letter-D)   [**E**](#Variable-Index_vr_letter-E)   [**F**](#Variable-Index_vr_letter-F)   [**G**](#Variable-Index_vr_letter-G)   [**H**](#Variable-Index_vr_letter-H)   [**I**](#Variable-Index_vr_letter-I)   [**J**](#Variable-Index_vr_letter-J)   [**L**](#Variable-Index_vr_letter-L)   [**M**](#Variable-Index_vr_letter-M)   [**N**](#Variable-Index_vr_letter-N)   [**O**](#Variable-Index_vr_letter-O)   [**P**](#Variable-Index_vr_letter-P)   [**R**](#Variable-Index_vr_letter-R)   [**S**](#Variable-Index_vr_letter-S)   [**T**](#Variable-Index_vr_letter-T)   [**U**](#Variable-Index_vr_letter-U)   [**V**](#Variable-Index_vr_letter-V)   [**W**](#Variable-Index_vr_letter-W)   [**X**](#Variable-Index_vr_letter-X)   [**Y**](#Variable-Index_vr_letter-Y)   [**Z**](#Variable-Index_vr_letter-Z) |

---

Previous: [Variable Index](#Variable-Index), Up: [Indices](#Indices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
