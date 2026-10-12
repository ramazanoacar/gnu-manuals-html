### 12.1 History [¶](#History-1)

GNUS was written by Masanobu UMEDA. When autumn crept up in
’94, Lars Magne Ingebrigtsen grew bored and decided to rewrite Gnus.

If you want to investigate the person responsible for this outrage,
you can point your (feh!) web browser to
<https://quimby.gnus.org/>. This used to be the primary
distribution point for the new and spiffy versions of Gnus, and was
known as The Site That Destroys Newsrcs And Drives People Mad, but
these days Gnus is developed in the Emacs repository.

During the first extended alpha period of development, the new Gnus was
called “(ding) Gnus”. *(ding)* is, of course, short for
*ding is not Gnus*, which is a total and utter lie, but who cares?
(Besides, the “Gnus” in this abbreviation should probably be
pronounced “news” as UMEDA intended, which makes it a more
appropriate name, don’t you think?)

In any case, after spending all that energy on coming up with a new and
spunky name, we decided that the name was *too* spunky, so we
renamed it back again to “Gnus”. But in mixed case. “Gnus” vs.
“GNUS”. New vs. old.

* [Gnus Versions](#Gnus-Versions)
* [Why?](#Why_003f)
* [Conformity](#Conformity)
* [Contributors](#Contributors)
* [New Features](#New-Features)

---

Next: [Why?](#Why_003f), Up: [History](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.1 Gnus Versions [¶](#Gnus-Versions-1)

The first “proper” release of Gnus 5 was done in November 1995 when it
was included in the Emacs 19.30 distribution (132 (ding) Gnus releases
plus 15 Gnus 5.0 releases).

In May 1996 the next Gnus generation (aka. “September Gnus” (after 99
releases)) was released under the name “Gnus 5.2” (40 releases).

On July 28th 1996 work on Red Gnus was begun, and it was released on
January 25th 1997 (after 84 releases) as “Gnus 5.4” (67 releases).

On September 13th 1997, Quassia Gnus was started and lasted 37 releases.
It was released as “Gnus 5.6” on March 8th 1998 (46 releases).

Gnus 5.6 begat Pterodactyl Gnus on August 29th 1998 and was released as
“Gnus 5.8” (after 99 releases and a CVS repository) on December 3rd
1999.

On the 26th of October 2000, Oort Gnus was begun and was released as
Gnus 5.10 on May 1st 2003 (24 releases).

On the January 4th 2004, No Gnus was begun.

Gnus 5.11 was bundled with GNU Emacs 22.1 in June 2007.

A version of No Gnus was released as Gnus 5.13 with GNU Emacs 23.1 in
July 2009.

On April 19, 2010 Gnus development was moved to Git.

On the January 31th 2012, Ma Gnus was begun.

Since then, Gnus has only been released together with Emacs.

If you happen upon a version of Gnus that has a prefixed name—“(ding)
Gnus”, “September Gnus”, “Red Gnus”, “Quassia Gnus”,
“Pterodactyl Gnus”, “Oort Gnus”, “No Gnus”, “Ma Gnus”—don’t
panic. Don’t let it know that you’re frightened. Back away. Slowly.
Whatever you do, don’t run. Walk away, calmly, until you’re out of
its reach. Find a proper released version of Gnus and snuggle up to
that instead.

---

Next: [Conformity](#Conformity), Previous: [Gnus Versions](#Gnus-Versions), Up: [History](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.2 Why? [¶](#Why_003f-1)

What’s the point of Gnus?

I want to provide a “rad”, “happening”, “way cool” and “hep”
newsreader, that lets you do anything you can think of. That was my
original motivation, but while working on Gnus, it has become clear to
me that this generation of newsreaders really belong in the stone age.
Newsreaders haven’t developed much since the infancy of the net. If the
volume continues to rise with the current rate of increase, all current
newsreaders will be pretty much useless. How do you deal with
newsgroups that have thousands of new articles each day? How do you
keep track of millions of people who post?

Gnus offers no real solutions to these questions, but I would very much
like to see Gnus being used as a testing ground for new methods of
reading and fetching news. Expanding on UMEDA-san’s wise decision
to separate the newsreader from the back ends, Gnus now offers a simple
interface for anybody who wants to write new back ends for fetching mail
and news from different sources. I have added hooks for customizations
everywhere I could imagine it being useful. By doing so, I’m inviting
every one of you to explore and invent.

May Gnus never be complete. `C-u 100 M-x all-hail-emacs`.

---

Next: [Contributors](#Contributors), Previous: [Why?](#Why_003f), Up: [History](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.3 Conformity [¶](#Conformity-1)

No rebels without a clue here, ma’am. We conform to all standards known
to (wo)man. Except for those standards and/or conventions we disagree
with, of course.

**RFC 822** [¶](#index-RFC-822)
:   There are no known breaches of this standard or its successors
    (currently RFCs 2822 and 5322).

**RFC 1036** [¶](#index-RFC-1036)
:   There are no known breaches of this (now-obsolete) standard, either.

**RFC 5536** [¶](#index-RFC-5536)
:   We do have some breaches of this standard, the successor of RFC 1036.

    *X-Newsreader*

    *User-Agent*
    :   These are considered to be “vanity headers”, while I consider them
        to be consumer information. After seeing so many badly formatted
        articles coming from `tin` and `Netscape` I know not to use
        either of those for posting articles. I would not have known that if
        it wasn’t for the `X-Newsreader` header.

**USEFOR** [¶](#index-USEFOR)
:   USEFOR was an IETF working group that produced Internet RFCs 5536 and 5537.
    The Gnus towers will look into implementing the changes embodied by these
    standards.

**MIME—RFC 2045–2049 etc** [¶](#index-MIME-2)
:   All the various MIME RFCs are supported.

**Disposition Notifications—RFC 2298**
:   Message Mode is able to request notifications from the receiver.

**PGP—RFC 1991 and RFC 2440** [¶](#index-RFC-1991)
:   RFC 1991 is the original PGP message specification,
    published as an informational RFC. RFC 2440 was the follow-up, now
    called Open PGP, and put on the Standards Track. Both document a
    non-MIME aware PGP format. Gnus supports both
    encoding (signing and encryption) and decoding (verification and
    decryption).

**PGP/MIME—RFC 2015/3156**
:   RFC 2015 (superseded by 3156 which references RFC 2440 instead of RFC
    1991) describes the MIME-wrapping around the RFC 1991/2440 format.
    Gnus supports both encoding and decoding.

**S/MIME—RFC 2633**
:   RFC 2633 describes the S/MIME format.

**IMAP—RFC 1730/2060, RFC 2195/4616, RFC 2086, RFC 2359, RFC 2595, RFC 1731**
:   RFC 1730 is IMAP version 4, updated somewhat by RFC 2060
    (IMAP 4 revision 1). RFC 2195 describes CRAM-MD5
    authentication for IMAP. RFC 2086 describes access control
    lists (ACLs) for IMAP. RFC 2359 describes a IMAP
    protocol enhancement. RFC 2595 describes the proper TLS
    integration (STARTTLS) with IMAP. RFC 1731 describes the
    GSSAPI/Kerberos4 mechanisms for IMAP. RFC 4616 describes
    AUTH=PLAIN authentication for IMAP.

If you ever notice Gnus acting non-compliant with regards to the texts
mentioned above, don’t hesitate to drop a note to Gnus Towers and let us
know.

---

Next: [New Features](#New-Features), Previous: [Conformity](#Conformity), Up: [History](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.4 Contributors [¶](#Contributors-1)

The new Gnus version couldn’t have been done without the help of all the
people on the (ding) mailing list. Every day for over a year I have
gotten billions of nice bug reports from them, filling me with joy,
every single one of them. Smooches. The people on the list have been
tried beyond endurance, what with my “oh, that’s a neat idea <type
type>, yup, I’ll release it right away <ship off> no wait, that doesn’t
work at all <type type>, yup, I’ll ship that one off right away <ship
off> no, wait, that absolutely does not work” policy for releases.
Micro$oft—bah. Amateurs. I’m *much* worse. (Or is that
“worser”? “much worser”? “worsest”?)

I would like to take this opportunity to thank the Academy for… oops,
wrong show.

* Masanobu UMEDA—the writer of the original GNUS.
* Shenghuo Zhu—uudecode.el, mm-uu.el, rfc1843.el,
  nnwarchive and many, many other things connected with MIME and
  other types of en/decoding, as well as general bug fixing, new
  functionality and stuff.
* Per Abrahamsen—custom, scoring, highlighting and SOUP code (as
  well as numerous other things).
* Luis Fernandes—design and graphics.
* Joe Reiss—creator of the smiley faces.
* Justin Sheehy—the FAQ maintainer.
* Erik Naggum—help, ideas, support, code and stuff.
* Wes Hardaker—`gnus-picon.el` and the manual section on
  *picons* (see [Picons](#Picons)).
* Kim-Minh Kaplan—further work on the picon code.
* Brad Miller—`gnus-gl.el` and the GroupLens manual section.
* Sudish Joseph—innumerable bug fixes.
* Ilja Weis—`gnus-topic.el`.
* Steven L. Baur—lots and lots and lots of bug detection and fixes.
* Vladimir Alexiev—the refcard and reference booklets.
* Felix Lee & Jamie Zawinski—I stole some pieces from the XGnus
  distribution by Felix Lee and JWZ.
* Scott Byer—`nnfolder.el` enhancements & rewrite.
* Peter Mutsaers—orphan article scoring code.
* Ken Raeburn—POP mail support.
* Hallvard B Furuseth—various bits and pieces, especially dealing with
  .newsrc files.
* Brian Edmonds—`gnus-bbdb.el`.
* David Moore—rewrite of `nnvirtual.el` and many other things.
* Kevin Davidson—came up with the name *ding*, so blame him.
* François Pinard—many, many interesting and thorough bug reports, as
  well as autoconf support.

This manual was proof-read by Adrian Aichner, with Ricardo Nassif, Mark
Borges, and Jost Krieger proof-reading parts of the manual.

The following people have contributed many patches and suggestions:

Christopher Davis,
Andrew Eskilsson,
Kai Grossjohann,
Kevin Greiner,
Jesper Harder,
Paul Jarc,
Simon Josefsson,
David Kågedal,
Richard Pieri,
Fabrice Popineau,
Daniel Quinlan,
Michael Shields,
Reiner Steib,
Jason L. Tibbitts, III,
Jack Vinson,
Katsumi Yamaoka, and
Teodor Zlatanov.

Also thanks to the following for patches and stuff:

Jari Aalto,
Adrian Aichner,
Vladimir Alexiev,
Russ Allbery,
Peter Arius,
Matt Armstrong,
Marc Auslander,
Miles Bader,
Alexei V. Barantsev,
Frank Bennett,
Robert Bihlmeyer,
Chris Bone,
Mark Borges,
Mark Boyns,
Lance A. Brown,
Rob Browning,
Kees de Bruin,
Martin Buchholz,
Joe Buehler,
Kevin Buhr,
Alastair Burt,
Joao Cachopo,
Zlatko Calusic,
Massimo Campostrini,
Castor,
David Charlap,
Dan Christensen,
Kevin Christian,
Jae-you Chung, James H. Cloos, Jr.,
Laura Conrad,
Michael R. Cook,
Glenn Coombs,
Andrew J. Cosgriff,
Neil Crellin,
Frank D. Cringle,
Geoffrey T. Dairiki,
Andre Deparade,
Ulrik Dickow,
Dave Disser,
Rui-Tao Dong, Joev Dubach,
Michael Welsh Duggan,
Dave Edmondson,
Paul Eggert,
Mark W. Eichin,
Karl Eichwalder,
Enami Tsugutomo, Michael Ernst,
Luc Van Eycken,
Sam Falkner,
Nelson Jose dos Santos Ferreira,
Sigbjorn Finne,
Sven Fischer,
Paul Fisher,
Decklin Foster,
Gary D. Foster,
Paul Franklin,
Guy Geens,
Arne Georg Gleditsch,
David S. Goldberg,
Michelangelo Grigni,
Dale Hagglund,
D. Hall,
Magnus Hammerin,
Kenichi Handa, Raja R. Harinath,
Yoshiki Hayashi, P. E. Jareth Hein,
Hisashige Kenji, Scott Hofmann,
Tassilo Horn,
Marc Horowitz,
Gunnar Horrigmo,
Richard Hoskins,
Brad Howes,
Miguel de Icaza,
François Felix Ingrand,
Tatsuya Ichikawa, Ishikawa Ichiro, Lee Iverson,
Iwamuro Motonori, Rajappa Iyer,
Andreas Jaeger,
Adam P. Jenkins,
Randell Jesup,
Fred Johansen,
Gareth Jones,
Greg Klanderman,
Karl Kleinpaste,
Michael Klingbeil,
Peter Skov Knudsen,
Shuhei Kobayashi, Petr Konecny,
Koseki Yoshinori, Thor Kristoffersen,
Jens Lautenbacher,
Martin Larose,
Seokchan Lee, Joerg Lenneis,
Carsten Leonhardt,
James LewisMoss,
Christian Limpach,
Markus Linnala,
Dave Love,
Mike McEwan,
Tonny Madsen,
Shlomo Mahlab,
Nat Makarevitch,
Istvan Marko,
David Martin,
Jason R. Mastaler,
Gordon Matzigkeit,
Timo Metzemakers,
Richard Mlynarik,
Lantz Moore,
Morioka Tomohiko, Erik Toubro Nielsen,
Hrvoje Nikšić,
Andy Norman,
Fred Oberhauser,
C. R. Oldham,
Alexandre Oliva,
Ken Olstad,
Masaharu Onishi, Hideki Ono, Ettore Perazzoli,
William Perry,
Stephen Peters,
Jens-Ulrik Holger Petersen,
Ulrich Pfeifer,
Matt Pharr,
Andy Piper,
John McClary Prevost,
Bill Pringlemeir,
Mike Pullen,
Jim Radford,
Colin Rafferty,
Lasse Rasinen,
Lars Balker Rasmussen,
Joe Reiss,
Renaud Rioboo,
Roland B. Roberts,
Bart Robinson,
Christian von Roques,
Markus Rost,
Jason Rumney,
Wolfgang Rupprecht,
Jay Sachs,
Dewey M. Sasser,
Conrad Sauerwald,
Loren Schall,
Dan Schmidt,
Ralph Schleicher,
Philippe Schnoebelen,
Andreas Schwab,
Randal L. Schwartz,
Danny Siu,
Matt Simmons,
Paul D. Smith,
Jeff Sparkes,
Toby Speight,
Michael Sperber,
Darren Stalder,
Richard Stallman,
Greg Stark,
Sam Steingold,
Paul Stevenson,
Jonas Steverud,
Paul Stodghill,
Kiyokazu Suto, Kurt Swanson,
Samuel Tardieu,
Teddy,
Chuck Thompson,
Tozawa Akihiko, Philippe Troin,
James Troup,
Trung Tran-Duc,
Jack Twilley,
Aaron M. Ucko,
Aki Vehtari,
Didier Verna,
Vladimir Volovich,
Jan Vroonhof,
Stefan Waldherr,
Pete Ware,
Barry A. Warsaw,
Christoph Wedler,
Joe Wells,
Lee Willis,
and
Lloyd Zusman.

For a full overview of what each person has done, the ChangeLogs
included in the Gnus alpha distributions should give ample reading
(550kB and counting).

Apologies to everybody that I’ve forgotten, of which there are many, I’m
sure.

Gee, that’s quite a list of people. I guess that must mean that there
actually are people who are using Gnus. Who’d’a thunk it!

---

Previous: [Contributors](#Contributors), Up: [History](#History)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5 New Features [¶](#New-Features-1)

For summaries of more recent changes, see the normal Emacs `NEWS` files.

These lists are, of course, just *short* overviews of the
*most* important new features. No, really. There are tons more.
Yes, we have feeping creaturism in full effect.

* [(ding) Gnus](#ding-Gnus)
* [September Gnus](#September-Gnus)
* [Red Gnus](#Red-Gnus)
* [Quassia Gnus](#Quassia-Gnus)
* [Pterodactyl Gnus](#Pterodactyl-Gnus)
* [Oort Gnus](#Oort-Gnus)
* [No Gnus](#No-Gnus)
* [Ma Gnus](#Ma-Gnus)

---

Next: [September Gnus](#September-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.1 (ding) Gnus [¶](#g_t_0028ding_0029-Gnus)

New features in Gnus 5.0/5.1:

* The look of all buffers can be changed by setting format-like variables
  (see [Group Buffer Format](#Group-Buffer-Format) and see [Summary Buffer Format](#Summary-Buffer-Format)).
* Local spool and several NNTP servers can be used at once
  (see [Select Methods](#Select-Methods)).
* You can combine groups into virtual groups (see [Virtual Groups](#Virtual-Groups)).
* You can read a number of different mail formats (see [Getting Mail](#Getting-Mail)).
  All the mail back ends implement a convenient mail expiry scheme
  (see [Expiring Mail](#Expiring-Mail)).
* Gnus can use various strategies for gathering threads that have lost
  their roots (thereby gathering loose sub-threads into one thread) or it
  can go back and retrieve enough headers to build a complete thread
  (see [Customizing Threading](#Customizing-Threading)).
* Killed groups can be displayed in the group buffer, and you can read
  them as well (see [Listing Groups](#Listing-Groups)).
* Gnus can do partial group updates—you do not have to retrieve the
  entire active file just to check for new articles in a few groups
  (see [The Active File](#The-Active-File)).
* Gnus implements a sliding scale of subscribedness to groups
  (see [Group Levels](#Group-Levels)).
* You can score articles according to any number of criteria
  (see [Scoring](#Scoring)). You can even get Gnus to find out how to score
  articles for you (see [Adaptive Scoring](#Adaptive-Scoring)).
* Gnus maintains a dribble buffer that is auto-saved the normal Emacs
  manner, so it should be difficult to lose much data on what you have
  read if your machine should go down (see [Auto Save](#Auto-Save)).
* Gnus now has its own startup file (`~/.gnus.el`) to avoid
  cluttering up the `.emacs` file.
* You can set the process mark on both groups and articles and perform
  operations on all the marked items (see [Process/Prefix](#Process_002fPrefix)).
* You can list subsets of groups according to, well, anything
  (see [Listing Groups](#Listing-Groups)).
* You can browse foreign servers and subscribe to groups from those
  servers (see [Browse Foreign Server](#Browse-Foreign-Server)).
* Gnus can fetch articles, asynchronously, on a second connection to the
  server (see [Asynchronous Article Fetching](#Asynchronous-Fetching)).
* You can cache articles locally (see [Article Caching](#Article-Caching)).
* The uudecode functions have been expanded and generalized
  (see [Decoding Articles](#Decoding-Articles)).
* You can still post uuencoded articles, which was a little-known feature
  of GNUS’ past (see [Uuencoding and Posting](#Uuencoding-and-Posting)).
* Fetching parents (and other articles) now actually works without
  glitches (see [Finding the Parent](#Finding-the-Parent)).
* Gnus can fetch FAQs and group descriptions (see [Group Information](#Group-Information)).
* Digests (and other files) can be used as the basis for groups
  (see [Document Groups](#Document-Groups)).
* Articles can be highlighted and customized (see [Customizing Articles](#Customizing-Articles)).
* URLs and other external references can be buttonized (see [Article Buttons](#Article-Buttons)).
* You can do lots of strange stuff with the Gnus window & frame
  configuration (see [Window Layout](#Window-Layout)).

---

Next: [Red Gnus](#Red-Gnus), Previous: [(ding) Gnus](#ding-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.2 September Gnus [¶](#September-Gnus-1)

New features in Gnus 5.2/5.3:

* A new message composition mode is used. All old customization variables
  for `mail-mode`, `rnews-reply-mode` and `gnus-msg` are
  now obsolete.
* Gnus is now able to generate *sparse* threads—threads where
  missing articles are represented by empty nodes (see [Customizing Threading](#Customizing-Threading)).

  ```
  (setq gnus-build-sparse-threads 'some)
  ```
* Outgoing articles are stored on a special archive server
  (see [Archived Messages](#Archived-Messages)).
* Partial thread regeneration now happens when articles are
  referred.
* Gnus can make use of GroupLens predictions.
* Picons (personal icons) can be displayed (see [Picons](#Picons)).
* A `trn`-like tree buffer can be displayed (see [Tree Display](#Tree-Display)).

  ```
  (setq gnus-use-trees t)
  ```
* An `nn`-like pick-and-read minor mode is available for the summary
  buffers (see [Pick and Read](#Pick-and-Read)).

  ```
  (add-hook 'gnus-summary-mode-hook 'gnus-pick-mode)
  ```
* In binary groups you can use a special binary minor mode (see [Binary Groups](#Binary-Groups)).
* Groups can be grouped in a folding topic hierarchy (see [Group Topics](#Group-Topics)).

  ```
  (add-hook 'gnus-group-mode-hook 'gnus-topic-mode)
  ```
* Gnus can re-send and bounce mail (see [Summary Mail Commands](#Summary-Mail-Commands)).
* Groups can now have a score, and bubbling based on entry frequency
  is possible (see [Group Score](#Group-Score)).

  ```
  (add-hook 'gnus-summary-exit-hook 'gnus-summary-bubble-group)
  ```
* Groups can be process-marked, and commands can be performed on
  groups of groups (see [Marking Groups](#Marking-Groups)).
* Caching is possible in virtual groups.
* `nndoc` now understands all kinds of digests, mail boxes, rnews
  news batches, ClariNet briefs collections, and just about everything
  else (see [Document Groups](#Document-Groups)).
* Gnus has a new back end (`nnsoup`) to create/read SOUP packets.
* The Gnus cache is much faster.
* Groups can be sorted according to many criteria (see [Sorting Groups](#Sorting-Groups)).
* New group parameters have been introduced to set list-addresses and
  expiry times (see [Group Parameters](#Group-Parameters)).
* All formatting specs allow specifying faces to be used
  (see [Formatting Fonts](#Formatting-Fonts)).
* There are several more commands for setting/removing/acting on process
  marked articles on the `M P` submap (see [Setting Process Marks](#Setting-Process-Marks)).
* The summary buffer can be limited to show parts of the available
  articles based on a wide range of criteria. These commands have been
  bound to keys on the `/` submap (see [Limiting](#Limiting)).
* Articles can be made persistent with the `*` command
  (see [Persistent Articles](#Persistent-Articles)).
* All functions for hiding article elements are now toggles.
* Article headers can be buttonized (see [Article Washing](#Article-Washing)).
* All mail back ends support fetching articles by `Message-ID`.
* Duplicate mail can now be treated properly (see [Duplicates](#Duplicates)).
* All summary mode commands are available directly from the article
  buffer (see [Article Keymap](#Article-Keymap)).
* Frames can be part of `gnus-buffer-configuration` (see [Window Layout](#Window-Layout)).
* Mail can be re-scanned by a daemonic process (see [Daemons](#Daemons)).
* Groups can be made permanently visible (see [Listing Groups](#Listing-Groups)).

  ```
  (setq gnus-permanently-visible-groups "^nnml:")
  ```
* Many new hooks have been introduced to make customizing easier.
* Gnus respects the `Mail-Copies-To` header.
* Threads can be gathered by looking at the `References` header
  (see [Customizing Threading](#Customizing-Threading)).

  ```
  (setq gnus-summary-thread-gathering-function
        'gnus-gather-threads-by-references)
  ```
* Read articles can be stored in a special backlog buffer to avoid
  refetching (see [Article Backlog](#Article-Backlog)).

  ```
  (setq gnus-keep-backlog 50)
  ```
* A clean copy of the current article is always stored in a separate
  buffer to allow easier treatment.
* Gnus can suggest where to save articles (see [Saving Articles](#Saving-Articles)).
* Gnus doesn’t have to do as much prompting when saving (see [Saving Articles](#Saving-Articles)).

  ```
  (setq gnus-prompt-before-saving t)
  ```
* `gnus-uu` can view decoded files asynchronously while fetching
  articles (see [Other Decode Variables](#Other-Decode-Variables)).

  ```
  (setq gnus-uu-grabbed-file-functions 'gnus-uu-grab-view)
  ```
* Filling in the article buffer now works properly on cited text
  (see [Article Washing](#Article-Washing)).
* Hiding cited text adds buttons to toggle hiding, and how much
  cited text to hide is now customizable (see [Article Hiding](#Article-Hiding)).

  ```
  (setq gnus-cited-lines-visible 2)
  ```
* Boring headers can be hidden (see [Article Hiding](#Article-Hiding)).
* Default scoring values can now be set from the menu bar.
* Further syntax checking of outgoing articles have been added.

---

Next: [Quassia Gnus](#Quassia-Gnus), Previous: [September Gnus](#September-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.3 Red Gnus [¶](#Red-Gnus-1)

New features in Gnus 5.4/5.5:

* `nntp.el` has been totally rewritten in an asynchronous fashion.
* Article prefetching functionality has been moved up into
  Gnus (see [Asynchronous Article Fetching](#Asynchronous-Fetching)).
* Scoring can now be performed with logical operators like `and`,
  `or`, `not`, and parent redirection (see [Advanced Scoring](#Advanced-Scoring)).
* Article washing status can be displayed in the
  article mode line (see [Misc Article](#Misc-Article)).
* `gnus.el` has been split into many smaller files.
* Suppression of duplicate articles based on Message-ID can be done
  (see [Duplicate Suppression](#Duplicate-Suppression)).

  ```
  (setq gnus-suppress-duplicates t)
  ```
* New variables for specifying what score and adapt files are to be
  considered home score and adapt files (see [Home Score File](#Home-Score-File)) have
  been added.
* `nndoc` was rewritten to be easily extensible (see [Document Server Internals](#Document-Server-Internals)).
* Groups can inherit group parameters from parent topics (see [Topic Parameters](#Topic-Parameters)).
* Article editing has been revamped and is now actually usable.
* Signatures can be recognized in more intelligent fashions
  (see [Article Signature](#Article-Signature)).
* Summary pick mode has been made to look more `nn`-like. Line
  numbers are displayed and the `.` command can be used to pick
  articles (`Pick and Read`).
* Commands for moving the `.newsrc.eld` from one server to
  another have been added (see [Changing Servers](#Changing-Servers)).
* There’s a way now to specify that “uninteresting” fields be suppressed
  when generating lines in buffers (see [Advanced Formatting](#Advanced-Formatting)).
* Several commands in the group buffer can be undone with `C-M-_`
  (see [Undo](#Undo)).
* Scoring can be done on words using the new score type `w`
  (see [Score File Format](#Score-File-Format)).
* Adaptive scoring can be done on a Subject word-by-word basis
  (see [Adaptive Scoring](#Adaptive-Scoring)).

  ```
  (setq gnus-use-adaptive-scoring '(word))
  ```
* Scores can be decayed (see [Score Decays](#Score-Decays)).

  ```
  (setq gnus-decay-scores t)
  ```
* Scoring can be performed using a regexp on the Date header. The Date is
  normalized to compact ISO 8601 format first (see [Score File Format](#Score-File-Format)).
* A new command has been added to remove all data on articles from
  the native server (see [Changing Servers](#Changing-Servers)).
* A new command for reading collections of documents
  (`nndoc` with `nnvirtual` on top) has been added—`C-M-d`
  (see [Really Various Summary Commands](#Really-Various-Summary-Commands)).
* Process mark sets can be pushed and popped (see [Setting Process Marks](#Setting-Process-Marks)).
* A new mail-to-news back end makes it possible to post even when the NNTP
  server doesn’t allow posting (see [Mail-To-News Gateways](#Mail_002dTo_002dNews-Gateways)).
* A new back end for reading searches from Web search engines
  (*DejaNews*, *Alta Vista*, *InReference*) has been added
  (see [Web Searches](#Web-Searches)).
* Groups inside topics can now be sorted using the standard sorting
  functions, and each topic can be sorted independently (see [Topic Sorting](#Topic-Sorting)).
* Subsets of the groups can be sorted independently (`Sorting
  Groups`).
* Cached articles can be pulled into the groups (see [Summary Generation Commands](#Summary-Generation-Commands)).
* Score files are now applied in a more reliable order (see [Score Variables](#Score-Variables)).
* Reports on where mail messages end up can be generated (see [Splitting Mail](#Splitting-Mail)).
* More hooks and functions have been added to remove junk from incoming
  mail before saving the mail (see [Washing Mail](#Washing-Mail)).
* Emphasized text can be properly fontisized:

---

Next: [Pterodactyl Gnus](#Pterodactyl-Gnus), Previous: [Red Gnus](#Red-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.4 Quassia Gnus [¶](#Quassia-Gnus-1)

New features in Gnus 5.6:

* New functionality for using Gnus as an offline newsreader has been
  added. A plethora of new commands and modes have been added.
  See [Gnus Unplugged](#Gnus-Unplugged), for the full story.
* The `nndraft` back end has returned, but works differently from
  before. All Message buffers are now also articles in the `nndraft`
  group, which is created automatically.
* `gnus-alter-header-function` can now be used to alter header
  values.
* `gnus-summary-goto-article` now accept Message-IDs.
* A new Message command for deleting text in the body of a message
  outside the region: `C-c C-v`.
* You can now post to component group in `nnvirtual` groups with
  `C-u C-c C-c`.
* `nntp-rlogin-program`—new variable to ease customization.
* `C-u C-c C-c` in `gnus-article-edit-mode` will now inhibit
  re-highlighting of the article buffer.
* New element in `gnus-boring-article-headers`—`long-to`.
* `M-i` symbolic prefix command. See [Symbolic Prefixes](#Symbolic-Prefixes), for
  details.
* `L` and `I` in the summary buffer now take the symbolic prefix
  `a` to add the score rule to the `all.SCORE` file.
* `gnus-simplify-subject-functions` variable to allow greater
  control over simplification.
* `A T`—new command for fetching the current thread.
* `/ T`—new command for including the current thread in the
  limit.
* `M-RET` is a new Message command for breaking cited text.
* ‘`\\1`’-expressions are now valid in `nnmail-split-methods`.
* The `custom-face-lookup` function has been removed.
  If you used this function in your initialization files, you must
  rewrite them to use `face-spec-set` instead.
* Canceling now uses the current select method. Symbolic prefix
  `a` forces normal posting method.
* New command to translate M\*\*\*\*\*\*\*\* sm\*rtq\*\*t\*s into proper
  text—`W d`.
* For easier debugging of `nntp`, you can set
  `nntp-record-commands` to a non-`nil` value.
* `nntp` now uses `~/.authinfo`, a `.netrc`-like file, for
  controlling where and how to send AUTHINFO to NNTP servers.
* A command for editing group parameters from the summary buffer
  has been added.
* A history of where mails have been split is available.
* A new article date command has been added—`article-date-iso8601`.
* Subjects can be simplified when threading by setting
  `gnus-score-thread-simplify`.
* A new function for citing in Message has been
  added—`message-cite-original-without-signature`.
* `article-strip-all-blank-lines`—new article command.
* A new Message command to kill to the end of the article has
  been added.
* A minimum adaptive score can be specified by using the
  `gnus-adaptive-word-minimum` variable.
* The “lapsed date” article header can be kept continually
  updated by the `gnus-start-date-timer` command.
* Web listserv archives can be read with the `nnlistserv` back end.
* Old dejanews archives can now be read by `nnweb`.

---

Next: [Oort Gnus](#Oort-Gnus), Previous: [Quassia Gnus](#Quassia-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.5 Pterodactyl Gnus [¶](#Pterodactyl-Gnus-1)

New features in Gnus 5.8:

* The mail-fetching functions have changed. See the manual for the
  many details. In particular, all procmail fetching variables are gone.

  If you used procmail like in

  ```
  (setq nnmail-use-procmail t)
  (setq nnmail-spool-file 'procmail)
  (setq nnmail-procmail-directory "~/mail/incoming/")
  (setq nnmail-procmail-suffix "\\.in")
  ```

  this now has changed to

  ```
  (setq mail-sources
        '((directory :path "~/mail/incoming/"
                     :suffix ".in")))
  ```

  See [Mail Source Specifiers](#Mail-Source-Specifiers).
* Gnus is now a MIME-capable reader. This affects many parts of
  Gnus, and adds a slew of new commands. See the manual for details.
* Gnus has also been multilingualized. This also affects too
  many parts of Gnus to summarize here, and adds many new variables.
* `gnus-auto-select-first` can now be a function to be
  called to position point.
* The user can now decide which extra headers should be included in
  summary buffers and NOV files.
* `gnus-article-display-hook` has been removed. Instead, a number
  of variables starting with `gnus-treat-` have been added.
* The Gnus posting styles have been redone again and now works in a
  subtly different manner.
* New web-based back ends have been added: `nnslashdot`,
  `nnwarchive` and `nnultimate`. nnweb has been revamped,
  again, to keep up with ever-changing layouts.
* Gnus can now read IMAP mail via `nnimap`.

---

Next: [No Gnus](#No-Gnus), Previous: [Pterodactyl Gnus](#Pterodactyl-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.6 Oort Gnus [¶](#Oort-Gnus-1)

New features in Gnus 5.10:

* Installation changes
  + Upgrading from previous (stable) version if you have used Oort.

    If you have tried Oort (the unstable Gnus branch leading to this
    release) but went back to a stable version, be careful when upgrading to
    this version. In particular, you will probably want to remove all
    `.marks` (nnml) and `.mrk` (nnfolder) files, so that flags are
    read from your `.newsrc.eld` instead of from the
    `.marks`/`.mrk` file where this release store flags. See a
    later entry for more information about marks. Note that downgrading
    isn’t save in general.
  + Lisp files are now installed in `.../site-lisp/gnus/` by default.
    It defaulted to `.../site-lisp/` formerly. In addition to this,
    the new installer issues a warning if other Gnus installations which
    will shadow the latest one are detected. You can then remove those
    shadows manually or remove them using `make
    remove-installed-shadows`.
  + `~/News/overview/` not used.

    As a result of the following change, the `~/News/overview/`
    directory is not used any more. You can safely delete the entire
    hierarchy.
  + `(require 'gnus-load)`

    If you use a stand-alone Gnus distribution, you’d better add
    `(require 'gnus-load)` into your `~/.emacs` after adding the Gnus
    lisp directory into load-path.

    File `gnus-load.el` contains autoload commands, functions and variables,
    some of which may not be included in distributions of Emacsen.
* New packages and libraries within Gnus
  + The revised Gnus FAQ is included in the manual,
    See [Frequently Asked Questions](#Frequently-Asked-Questions).
  + TLS wrapper shipped with Gnus

    TLS/SSL is now supported in IMAP and
    NNTP via GnuTLS.
  + Improved anti-spam features.

    Gnus is now able to take out spam from your mail and news streams
    using a wide variety of programs and filter rules. Among the supported
    methods are RBL blocklists, bogofilter and white/blacklists. Hooks
    for easy use of external packages such as SpamAssassin and Hashcash
    are also new. [Thwarting Email Spam](#Thwarting-Email-Spam) and [Spam Package](#Spam-Package).
  + Gnus supports server-side mail filtering using Sieve.

    Sieve rules can be added as Group Parameters for groups, and the
    complete Sieve script is generated using `D g` from the Group
    buffer, and then uploaded to the server using `C-c C-l` in the
    generated Sieve buffer. See [Sieve Commands](#Sieve-Commands), and the new Sieve
    manual [Top](https://www.gnu.org/software/emacs/manual/html_mono/sieve.html#Top) in Emacs Sieve.
* Changes in group mode
  + `gnus-group-read-ephemeral-group` can be called interactively,
    using `G M`.
  + Retrieval of charters and control messages

    There are new commands for fetching newsgroup charters (`H c`) and
    control messages (`H C`).
  + The new variable `gnus-parameters` can be used to set group parameters.

    Earlier this was done only via `G p` (or `G c`), which stored
    the parameters in `~/.newsrc.eld`, but via this variable you can
    enjoy the powers of customize, and simplified backups since you set the
    variable in `~/.gnus.el` instead of `~/.newsrc.eld`. The
    variable maps regular expressions matching group names to group
    parameters, a’la:

    ```
    (setq gnus-parameters
          '(("mail\\..*"
             (gnus-show-threads nil)
             (gnus-use-scoring nil))
            ("^nnimap:\\(foo.bar\\)$"
             (to-group . "\\1"))))
    ```
  + Unread count correct in nnimap groups.

    The estimated number of unread articles in the group buffer should now
    be correct for nnimap groups. This is achieved by calling
    `nnimap-fixup-unread-after-getting-new-news` from the
    `gnus-setup-news-hook` (called on startup) and
    `gnus-after-getting-new-news-hook` (called after getting new
    mail). If you have modified those variables from the default, you may
    want to add `nnimap-fixup-unread-after-getting-new-news` again. If
    you were happy with the estimate and want to save some (minimal) time
    when getting new mail, remove the function.
  + Group names are treated as UTF-8 by default.

    This is supposedly what USEFOR wanted to migrate to. See
    `gnus-group-name-charset-group-alist` and
    `gnus-group-name-charset-method-alist` for customization.
  + `gnus-group-charset-alist` and
    `gnus-group-ignored-charsets-alist`.

    The regexps in these variables are compared with full group names
    instead of real group names in 5.8. Users who customize these
    variables should change those regexps accordingly. For example:

    ```
    ("^han\\>" euc-kr) -> ("\\(^\\|:\\)han\\>" euc-kr)
    ```
  + Old intermediate incoming mail files (`Incoming*`) are deleted
    after a couple of days, not immediately. See [Mail Source Customization](#Mail-Source-Customization). (New in Gnus 5.10.10 / Emacs 22.2)
* Changes in summary and article mode
  + `F` (`gnus-article-followup-with-original`) and `R`
    (`gnus-article-reply-with-original`) only yank the text in the
    region if the region is active.
  + In draft groups, `e` is now bound to `gnus-draft-edit-message`.
    Use `B w` for `gnus-summary-edit-article` instead.
  + Article Buttons

    More buttons for URLs, mail addresses, Message-IDs, Info links, man
    pages and Emacs or Gnus related references. See [Article Buttons](#Article-Buttons). The
    variables `gnus-button-*-level` can be used to control the
    appearance of all article buttons. See [Article button levels](#Article-Button-Levels).
  + Single-part yenc encoded attachments can be decoded.
  + Picons

    The picons code has been reimplemented to work in GNU Emacs—some of
    the previous options have been removed or renamed.

    Picons are small “personal icons” representing users, domain and
    newsgroups, which can be displayed in the Article buffer.
    See [Picons](#Picons).
  + If the new option `gnus-treat-body-boundary` is non-`nil`, a
    boundary line is drawn at the end of the headers.
  + Signed article headers (X-PGP-Sig) can be verified with `W p`.
  + The Summary Buffer uses an arrow in the fringe to indicate the current
    article. Use `(setq gnus-summary-display-arrow nil)` to disable it.
  + Warn about email replies to news

    Do you often find yourself replying to news by email by mistake? Then
    the new option `gnus-confirm-mail-reply-to-news` is just the thing for
    you.
  + If the new option `gnus-summary-display-while-building` is
    non-`nil`, the summary buffer is shown and updated as it’s being
    built.
  + Gnus supports RFC 2369 mailing list headers, and adds a number of
    related commands in mailing list groups. See [Mailing List](#Mailing-List).
  + The Date header can be displayed in a format that can be read aloud
    in English. See [Article Date](#Article-Date).
  + diffs are automatically highlighted in groups matching
    `mm-uu-diff-groups-regexp`
  + Better handling of Microsoft citation styles

    Gnus now tries to recognize the mangled header block that some Microsoft
    mailers use to indicate that the rest of the message is a citation, even
    though it is not quoted in any way. The variable
    `gnus-cite-unsightly-citation-regexp` matches the start of these
    citations.

    The new command `W Y f`
    (`gnus-article-outlook-deuglify-article`) allows deuglifying broken
    Outlook (Express) articles.
  + `gnus-article-skip-boring`

    If you set `gnus-article-skip-boring` to `t`, then Gnus will
    not scroll down to show you a page that contains only boring text,
    which by default means cited text and signature. You can customize
    what is skippable using `gnus-article-boring-faces`.

    This feature is especially useful if you read many articles that
    consist of a little new content at the top with a long, untrimmed
    message cited below.
  + Smileys (‘`:-)`’, ‘`;-)`’ etc.) are now displayed graphically in
    Emacs too.

    Put `(setq gnus-treat-display-smileys nil)` in `~/.gnus.el` to
    disable it.
  + Face headers handling. See [Face](#Face).
  + In the summary buffer, the new command `/ N` inserts new messages
    and `/ o` inserts old messages.
  + Gnus decodes morse encoded messages if you press `W m`.
  + `gnus-summary-line-format`

    The default value changed to ‘`%U%R%z%I%(%[%4L: %-23,23f%]%)
    %s\n`’. Moreover `gnus-extra-headers`,
    `nnmail-extra-headers` and `gnus-ignored-from-addresses`
    changed their default so that the users name will be replaced by the
    recipient’s name or the group name posting to for NNTP
    groups.
  + Deleting of attachments.

    The command `gnus-mime-save-part-and-strip` (bound to `C-o`
    on MIME buttons) saves a part and replaces the part with an
    external one. `gnus-mime-delete-part` (bound to `d` on
    MIME buttons) removes a part. It works only on back ends
    that support editing.
  + `gnus-default-charset`

    The default value is determined from the
    `current-language-environment` variable, instead of
    `iso-8859-1`. Also the ‘`.*`’ item in
    `gnus-group-charset-alist` is removed.
  + Printing capabilities are enhanced.

    Gnus supports Muttprint natively with `O P` from the Summary and
    Article buffers. Also, each individual MIME part can be
    printed using `p` on the MIME button.
  + Extended format specs.

    Format spec ‘`%&user-date;`’ is added into
    `gnus-summary-line-format-alist`. Also, user defined extended
    format specs are supported. The extended format specs look like
    ‘`%u&foo;`’, which invokes function
    `gnus-user-format-function-foo`. Because ‘`&`’ is used as the
    escape character, old user defined format ‘`%u&`’ is no longer supported.
  + `/ *` (`gnus-summary-limit-include-cached`) is rewritten.

    It was aliased to `Y c`
    (`gnus-summary-insert-cached-articles`). The new function filters
    out other articles.
  + Some limiting commands accept a `C-u` prefix to negate the match.

    If `C-u` is used on subject, author or extra headers, i.e., `/
    s`, `/ a`, and `/ x`
    (`gnus-summary-limit-to-{subject,author,extra}`) respectively, the
    result will be to display all articles that do not match the expression.
  + Gnus inlines external parts (message/external).
* Changes in Message mode and related Gnus features
  + Delayed articles

    You can delay the sending of a message with `C-c C-j` in the Message
    buffer. The messages are delivered at specified time. This is useful
    for sending yourself reminders. See [Delayed Articles](#Delayed-Articles).
  + If the new option `nnml-use-compressed-files` is non-`nil`,
    the nnml back end allows compressed message files.
  + The new option `gnus-gcc-mark-as-read` automatically marks
    Gcc articles as read.
  + Externalizing of attachments

    If `gnus-gcc-externalize-attachments` or
    `message-fcc-externalize-attachments` is non-`nil`, attach
    local files as external parts.
  + The envelope sender address can be customized when using Sendmail.
    See [Mail Variables](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Mail-Variables) in Message Manual.
  + Gnus no longer generate the Sender: header automatically.

    Earlier it was generated when the user configurable email address was
    different from the Gnus guessed default user address. As the guessing
    algorithm is rarely correct these days, and (more controversially) the
    only use of the Sender: header was to check if you are entitled to
    cancel/supersede news (which is now solved by Cancel Locks instead,
    see another entry), generation of the header has been disabled by
    default. See the variables `message-required-headers`,
    `message-required-news-headers`, and
    `message-required-mail-headers`.
  + Features from third party `message-utils.el` added to `message.el`.

    Message now asks if you wish to remove ‘`(was: <old subject>)`’ from
    subject lines (see `message-subject-trailing-was-query`). `C-c
    M-m` and `C-c M-f` inserts markers indicating included text.
    `C-c C-f a` adds a X-No-Archive: header. `C-c C-f x` inserts
    appropriate headers and a note in the body for cross-postings and
    followups (see the variables `message-cross-post-*`).
  + References and X-Draft-From headers are no longer generated when you
    start composing messages and `message-generate-headers-first` is
    `nil`.
  + Easy inclusion of X-Faces headers. See [X-Face](#X_002dFace).
  + Group Carbon Copy (GCC) quoting

    To support groups that contains `SPC` and other weird characters, groups
    are quoted before they are placed in the Gcc: header. This means
    variables such as `gnus-message-archive-group` should no longer
    contain quote characters to make groups containing `SPC` work. Also, if
    you are using the string ‘`nnml:foo, nnml:bar`’ (indicating Gcc
    into two groups) you must change it to return the list
    `("nnml:foo" "nnml:bar")`, otherwise the Gcc: line will be quoted
    incorrectly. Note that returning the string ‘`nnml:foo, nnml:bar`’
    was incorrect earlier, it just didn’t generate any problems since it
    was inserted directly.
  + `message-insinuate-rmail`

    Adding `(message-insinuate-rmail)` and `(setq
    mail-user-agent 'gnus-user-agent)` in `.emacs` convinces Rmail to
    compose, reply and forward messages in message-mode, where you can
    enjoy the power of MML.
  + `message-minibuffer-local-map`

    The line below enables BBDB in resending a message:

    ```
    (keymap-set message-minibuffer-local-map "TAB" 'bbdb-complete-name)
    ```
  + `gnus-posting-styles`

    Add a new format of match like

    ```
    ((header "to" "larsi.*org")
     (Organization "Somewhere, Inc."))
    ```

    The old format like the lines below is obsolete, but still accepted.

    ```
    (header "to" "larsi.*org"
            (Organization "Somewhere, Inc."))
    ```
  + `message-ignored-news-headers` and `message-ignored-mail-headers`

    ‘`X-Draft-From`’ and ‘`X-Gnus-Agent-Meta-Information`’ have been
    added into these two variables. If you customized those, perhaps you
    need add those two headers too.
  + Gnus supports the “format=flowed” (RFC 2646) parameter. On
    composing messages, it is enabled by `use-hard-newlines`.
    Decoding format=flowed was present but not documented in earlier
    versions.
  + The option `mm-fill-flowed` can be used to disable treatment of
    “format=flowed” messages. Also, flowed text is disabled when sending
    inline PGP signed messages. See [Flowed text](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Flowed-text) in The Emacs MIME Manual. (New in Gnus 5.10.7)
  + Gnus supports the generation of RFC 2298 Disposition Notification requests.

    This is invoked with the `C-c M-n` key binding from message mode.
  + Message supports the Importance: (RFC 2156) header.

    In the message buffer, `C-c C-f C-i` or `C-c C-u` cycles through
    the valid values.
  + Gnus supports Cancel Locks in News.

    This means a header ‘`Cancel-Lock`’ is inserted in news posting. It is
    used to determine if you wrote an article or not (for canceling and
    superseding). Gnus generates a random password string the first time
    you post a message, and saves it in your `~/.emacs` using the Custom
    system. While the variable is called `canlock-password`, it is not
    security sensitive data. Publishing your canlock string on the web
    will not allow anyone to be able to anything she could not already do.
    The behavior can be changed by customizing `message-insert-canlock`.
  + Gnus supports PGP (RFC 1991/2440), PGP/MIME (RFC
    2015/3156) and S/MIME (RFC 2630–2633).

    It needs an external S/MIME and OpenPGP implementation, but no
    additional Lisp libraries. This add several menu items to the
    Attachments menu, and `C-c RET` key bindings, when composing
    messages. This also obsoletes `gnus-article-hide-pgp-hook`.
  + MML (Mime compose) prefix changed from `M-m` to `C-c
    C-m`.

    This change was made to avoid conflict with the standard binding of
    `back-to-indentation`, which is also useful in message mode.
  + The default for `message-forward-show-mml` changed to the symbol
    `best`.

    The behavior for the `best` value is to show MML (i.e.,
    convert to MIME) when appropriate. MML will not be
    used when forwarding signed or encrypted messages, as the conversion
    invalidate the digital signature.
  + If `auto-compression-mode` is enabled, attachments are automatically
    decompressed when activated.
  + Support for non-ASCII domain names

    Message supports non-ASCII domain names in From:, To: and
    Cc: and will query you whether to perform encoding when you try to
    send a message. The variable `message-use-idna` controls this.
    Gnus will also decode non-ASCII domain names in From:, To:
    and Cc: when you view a message. The variable `gnus-use-idna`
    controls this.
  + You can now drag and drop attachments to the Message buffer.
    See `mml-dnd-protocol-alist` and `mml-dnd-attach-options`.
    See [MIME](https://www.gnu.org/software/emacs/manual/html_mono/message.html#MIME) in Message Manual.
  + `auto-fill-mode` is enabled by default in Message mode.
    See `message-fill-column`. See [Message Headers](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Various-Message-Variables) in Message Manual.
* Changes in back ends
  + Gnus can display RSS newsfeeds as a newsgroup. See [RSS](#RSS).
  + The nndoc back end now supports mailman digests and exim bounces.
  + Gnus supports Maildir groups.

    Gnus includes a new back end `nnmaildir.el`. See [Maildir](#Maildir).
  + The nnml and nnfolder back ends store marks for each groups.

    This makes it possible to take backup of nnml/nnfolder servers/groups
    separately of `~/.newsrc.eld`, while preserving marks. It also
    makes it possible to share articles and marks between users (without
    sharing the `~/.newsrc.eld` file) within, e.g., a department. It
    works by storing the marks stored in `~/.newsrc.eld` in a per-group
    file `.marks` (for nnml) and `groupname.mrk` (for
    nnfolder, named groupname). If the nnml/nnfolder is moved to
    another machine, Gnus will automatically use the `.marks` or
    `.mrk` file instead of the information in `~/.newsrc.eld`.
    The new server variables `nnml-marks-is-evil` and
    `nnfolder-marks-is-evil` can be used to disable this feature.
* Appearance
  + The menu bar item (in Group and Summary buffer) named “Misc” has
    been renamed to “Gnus”.
  + The menu bar item (in Message mode) named “MML” has been
    renamed to “Attachments”. Note that this menu also contains security
    related stuff, like signing and encryption (see [Security](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Security) in Message Manual).
  + The tool bars have been updated to use GNOME icons in Group, Summary and
    Message mode. You can also customize the tool bars: `M-x
    customize-apropos RET -tool-bar$` should get you started. This is a new
    feature in Gnus 5.10.10.
  + The tool bar icons are now (de)activated correctly
    in the group buffer, see the variable `gnus-group-update-tool-bar`.
    Its default value depends on your Emacs version. This is a new feature
    in Gnus 5.10.9.
* Miscellaneous changes
  + `gnus-agent`

    The Gnus Agent has seen a major updated and is now enabled by default,
    and all nntp and nnimap servers from `gnus-select-method` and
    `gnus-secondary-select-method` are agentized by default. Earlier
    only the server in `gnus-select-method` was agentized by the
    default, and the agent was disabled by default. When the agent is
    enabled, headers are now also retrieved from the Agent cache instead
    of the back ends when possible. Earlier this only happened in the
    unplugged state. You can enroll or remove servers with `J a` and
    `J r` in the server buffer. Gnus will not download articles into
    the Agent cache, unless you instruct it to do so, though, by using
    `J u` or `J s` from the Group buffer. You revert to the old
    behavior of having the Agent disabled with `(setq gnus-agent
    nil)`. Note that putting `(gnus-agentize)` in `~/.gnus.el`
    is not needed any more.
  + Gnus reads the NOV and articles in the Agent if plugged.

    If one reads an article while plugged, and the article already exists
    in the Agent, it won’t get downloaded once more. `(setq
    gnus-agent-cache nil)` reverts to the old behavior.
  + Dired integration

    `gnus-dired-minor-mode` (see [Interaction with other modes](#Other-modes)) installs key
    bindings in dired buffers to send a file as an attachment, open a file
    using the appropriate mailcap entry, and print a file using the mailcap
    entry.
  + The format spec `%C` for positioning point has changed to `%*`.
  + `gnus-child-unplugged`

    A new command which starts Gnus offline in child mode.

---

Next: [Ma Gnus](#Ma-Gnus), Previous: [Oort Gnus](#Oort-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.7 No Gnus [¶](#No-Gnus-1)

New features in No Gnus:

* Supported Emacs versions
  The following Emacs versions are supported by No Gnus:
  + Emacs 22 and up
* Installation changes
  + Upgrading from previous (stable) version if you have used No Gnus.

    If you have tried No Gnus (the unstable Gnus branch leading to this
    release) but went back to a stable version, be careful when upgrading
    to this version. In particular, you will probably want to remove the
    `~/News/marks` directory (perhaps selectively), so that flags are
    read from your `~/.newsrc.eld` instead of from the stale marks
    file, where this release will store flags for nntp. See a later entry
    for more information about nntp marks. Note that downgrading isn’t
    safe in general.
  + Incompatibility when switching from Emacs 23 to Emacs 22
    In Emacs 23, Gnus uses Emacs’s new internal coding system `utf-8-emacs`
    for saving articles drafts and `~/.newsrc.eld`. These files may not
    be read correctly in Emacs 22 and below. If you want to use Gnus across
    different Emacs versions, you may set `mm-auto-save-coding-system`
    to `emacs-mule`.
  + Lisp files are now installed in `.../site-lisp/gnus/` by default.
    It defaulted to `.../site-lisp/` formerly. In addition to this,
    the new installer issues a warning if other Gnus installations which
    will shadow the latest one are detected. You can then remove those
    shadows manually or remove them using `make
    remove-installed-shadows`.
  + The installation directory’s name is allowed to have spaces and/or tabs.
* New packages and libraries within Gnus
  + New version of `nnimap`

    `nnimap` has been reimplemented in a mostly-compatible way. See
    the Gnus manual for a description of the new interface. In
    particular, `nnimap-inbox` and the client side split method has
    changed.
  + Gnus includes the Emacs Lisp SASL library.

    This provides a clean API to SASL mechanisms from
    within Emacs. The user visible aspects of this, compared to the earlier
    situation, include support for DIGEST-MD5 and
    NTLM. See [Emacs SASL](https://www.gnu.org/software/emacs/manual/html_mono/sasl.html#Top) in Emacs SASL.
  + ManageSieve connections uses the SASL library by default.

    The primary change this brings is support for DIGEST-MD5 and
    NTLM, when the server supports it.
  + Gnus includes a password cache mechanism in password.el.

    It is enabled by default (see `password-cache`), with a short
    timeout of 16 seconds (see `password-cache-expiry`). If
    PGG is used as the PGP back end, the PGP
    passphrase is managed by this mechanism. Passwords for ManageSieve
    connections are managed by this mechanism, after querying the user
    about whether to do so.
  + Using EasyPG with Gnus
    When EasyPG, is available, Gnus will use it instead of PGG.
    EasyPG is an Emacs user interface to GNU Privacy Guard. See [EasyPG Assistant user’s manual](https://www.gnu.org/software/emacs/manual/html_mono/epa.html#Top) in EasyPG Assistant user’s manual.
    EasyPG is included in Emacs 23 and available separately as well.
* Changes in group mode
  + Symbols like `gcc-self` now have the same precedence rules in
    `gnus-parameters` as other “real” variables: The last match
    wins instead of the first match.
  + Old intermediate incoming mail files (`Incoming*`) are deleted
    after a couple of days, not immediately. See [Mail Source Customization](#Mail-Source-Customization).
    (New in Gnus 5.10.10 / No Gnus 0.8)
* Changes in summary and article mode
  + There’s now only one variable that determines how HTML
    is rendered: `mm-text-html-renderer`.
  + Gnus now supports sticky article buffers. Those are article buffers
    that are not reused when you select another article. See [Sticky Articles](#Sticky-Articles).
  + Gnus can selectively display ‘`text/html`’ articles
    with a WWW browser with `K H`. See [MIME Commands](#MIME-Commands).
  + International host names (IDNA) can now be decoded
    inside article bodies using `W i`
    (`gnus-summary-idna-message`). This requires that GNU Libidn
    (<https://www.gnu.org/software/libidn/>) has been installed.
  + The non-ASCII group names handling has been much
    improved. The back ends that fully support non-ASCII group
    names are now `nntp`, `nnml`, and `nnrss`. Also the
    agent, the cache, and the marks features work with those back ends.
    See [Accessing groups of non-English names](#Non_002dASCII-Group-Names).
  + Gnus now displays DNS master files sent as text/dns
    using dns-mode.
  + Gnus supports new limiting commands in the Summary buffer:
    `/ r` (`gnus-summary-limit-to-replied`) and `/ R`
    (`gnus-summary-limit-to-recipient`). See [Limiting](#Limiting).
  + You can now fetch all ticked articles from the server using
    `Y t` (`gnus-summary-insert-ticked-articles`). See [Summary Generation Commands](#Summary-Generation-Commands).
  + Gnus supports a new sort command in the Summary buffer:
    `C-c C-s C-t` (`gnus-summary-sort-by-recipient`). See [Summary Sorting](#Summary-Sorting).
  + S/MIME now features LDAP user certificate searches.
    You need to configure the server in `smime-ldap-host-list`.
  + URLs inside OpenPGP headers are retrieved and imported
    to your PGP key ring when you click on them.
  + Picons can be displayed right from the textual address, see
    `gnus-picon-style`. See [Picons](#Picons).
  + ANSI SGR control sequences can be transformed
    using `W A`.

    ANSI sequences are used in some Chinese hierarchies for
    highlighting articles (`gnus-article-treat-ansi-sequences`).
  + Gnus now MIME decodes articles even when they lack "MIME-Version" header.
    This changes the default of `gnus-article-loose-mime`.
  + `gnus-decay-scores` can be a regexp matching score files.
    For example, set it to ‘`\\.ADAPT\\'`’ and only adaptive score files
    will be decayed. See [Score Decays](#Score-Decays).
  + Strings prefixing to the `To` and `Newsgroup` headers in
    summary lines when using `gnus-ignored-from-addresses` can be
    customized with `gnus-summary-to-prefix` and
    `gnus-summary-newsgroup-prefix`. See [To From Newsgroups](#To-From-Newsgroups).
  + You can replace MIME parts with external bodies.
    See `gnus-mime-replace-part` and `gnus-article-replace-part`.
    See [MIME Commands](#MIME-Commands), [Using MIME](#Using-MIME).
  + The option `mm-fill-flowed` can be used to disable treatment of
    format=flowed messages. Also, flowed text is disabled when sending
    inline PGP signed messages. See [Flowed text](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Flowed-text) in The Emacs MIME Manual. (New in Gnus 5.10.7)
  + Now the new command `S W`
    (`gnus-article-wide-reply-with-original`) for a wide reply in the
    article buffer yanks a text that is in the active region, if it is set,
    as well as the `R` (`gnus-article-reply-with-original`) command.
    Note that the `R` command in the article buffer no longer accepts a
    prefix argument, which was used to make it do a wide reply.
    See [Article Keymap](#Article-Keymap).
  + The new command `C-h b`
    (`gnus-article-describe-bindings`) used in the article buffer now
    shows not only the article commands but also the real summary commands
    that are accessible from the article buffer.
* Changes in Message mode
  + Gnus now defaults to saving all outgoing messages in per-month
    nnfolder archives.
  + Gnus now supports the “hashcash” client puzzle anti-spam mechanism.
  + You can now drag and drop attachments to the Message buffer.
    See `mml-dnd-protocol-alist` and `mml-dnd-attach-options`.
    See [MIME](https://www.gnu.org/software/emacs/manual/html_mono/message.html#MIME) in Message Manual.
  + The option `message-yank-empty-prefix` now controls how
    empty lines are prefixed in cited text. See [Insertion Variables](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Insertion-Variables) in Message Manual.
  + Gnus uses narrowing to hide headers in Message buffers.
    The `References` header is hidden by default. To make all
    headers visible, use `(setq message-hidden-headers nil)`.
    See [Message Headers](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Message-Headers) in Message Manual.
  + You can highlight different levels of citations like in the
    article buffer. See `gnus-message-highlight-citation`.
  + `auto-fill-mode` is enabled by default in Message mode.
    See `message-fill-column`. See [Message Headers](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Various-Message-Variables) in Message Manual.
  + You can now store signature files in a special directory
    named `message-signature-directory`.
  + The option `message-citation-line-format` controls the format
    of the "Whomever writes:" line. You need to set
    `message-citation-line-function` to
    `message-insert-formatted-citation-line` as well.
* Changes in Browse Server mode
  + Gnus’ sophisticated subscription methods are now available in
    Browse Server buffers as well using the variable
    `gnus-browse-subscribe-newsgroup-method`.
* Changes in back ends
  + The nntp back end stores article marks in `~/News/marks`.

    The directory can be changed using the (customizable) variable
    `nntp-marks-directory`, and marks can be disabled using the
    (back end) variable `nntp-marks-is-evil`. The advantage of this
    is that you can copy `~/News/marks` (using rsync, scp or
    whatever) to another Gnus installation, and it will realize what
    articles you have read and marked. The data in `~/News/marks`
    has priority over the same data in `~/.newsrc.eld`.
  + You can import and export your RSS subscriptions from
    OPML files. See [RSS](#RSS).
  + IMAP identity (RFC 2971) is supported.

    By default, Gnus does not send any information about itself, but you can
    customize it using the variable `nnimap-id`.
  + The `nnrss` back end now supports multilingual text.
    Non-ASCII group names for the `nnrss` groups are also
    supported. See [RSS](#RSS).
  + Retrieving mail with POP3 is supported over SSL/TLS and with StartTLS.
  + The nnml back end allows other compression programs beside `gzip`
    for compressed message files. See [Mail Spool](#Mail-Spool).
  + The nnml back end supports group compaction.

    This feature, accessible via the functions
    `gnus-group-compact-group` (`G z` in the group buffer) and
    `gnus-server-compact-server` (`z` in the server buffer)
    renumbers all articles in a group, starting from 1 and removing gaps.
    As a consequence, you get a correct total article count (until
    messages are deleted again).
* Appearance
  + The tool bar has been updated to use GNOME icons.
    You can also customize the tool bars: `M-x customize-apropos RET
    -tool-bar$` should get you started.
  + The tool bar icons are now (de)activated correctly
    in the group buffer, see the variable `gnus-group-update-tool-bar`.
    Its default value depends on your Emacs version.
* Miscellaneous changes
  + Having edited the select-method for the foreign server in the
    server buffer is immediately reflected to the subscription of the groups
    which use the server in question. For instance, if you change
    `nntp-via-address` into ‘`bar.example.com`’ from
    ‘`foo.example.com`’, Gnus will connect to the news host by way of the
    intermediate host ‘`bar.example.com`’ from next time.
  + The `all.SCORE` file can be edited from the group buffer
    using `W e`.
  + You can set `gnus-mark-copied-or-moved-articles-as-expirable`
    to a non-`nil` value so that articles that have been read may be
    marked as expirable automatically when copying or moving them to a group
    that has auto-expire turned on. The default is `nil` and copying
    and moving of articles behave as before; i.e., the expirable marks will
    be unchanged except that the marks will be removed when copying or
    moving articles to a group that has not turned auto-expire on.
    See [Expiring Mail](#Expiring-Mail).
  + NoCeM support has been removed.
  + Carpal mode has been removed.

---

Previous: [No Gnus](#No-Gnus), Up: [New Features](#New-Features)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.1.5.8 Ma Gnus [¶](#Ma-Gnus-1)

It’s really spelled 真Gnus. Ma Gnus was the code name for the
development version of Gnus started in 2012. These days, Gnus is only
released together with Emacs.

New features in Ma Gnus:

* Changes in summary and article mode
  + By default, MIME part buttons for attachments (if any) will
    appear in the end of the article header in addition to the bottom of the
    article body, so you can easily find them without scrolling the article
    again and again. See [MIME Commands](#MIME-Commands).
* Changes in Message mode and related Gnus features
  + The new hooks `gnus-gcc-pre-body-encode-hook` and
    `gnus-gcc-post-body-encode-hook` are run before/after encoding
    the message body of the Gcc copy of a sent message.
    See [Archived Messages](#Archived-Messages).

    For more recent changes, see the Emacs `NEWS` files.

---

Next: [Terminology](#Terminology), Previous: [History](#History), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.2 On Writing Manuals [¶](#On-Writing-Manuals-1)

I guess most manuals are written after-the-fact; documenting a program
that’s already there. This is not how this manual is written. When
implementing something, I write the manual entry for that something
straight away. I then see that it’s difficult to explain the
functionality, so I write how it’s supposed to be, and then I change the
implementation. Writing the documentation and writing the code go hand
in hand.

This, of course, means that this manual has no, or little, flow. It
documents absolutely everything in Gnus, but often not where you’re
looking for it. It is a reference manual, and not a guide to how to get
started with Gnus.

That would be a totally different book, that should be written using the
reference manual as source material. It would look quite different.

---

Next: [Customization](#Customization), Previous: [On Writing Manuals](#On-Writing-Manuals), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.3 Terminology [¶](#Terminology-1)

*news* [¶](#index-news)
:   This is what you are supposed to use this thing for—reading news.
    News is generally fetched from a nearby NNTP server, and is
    generally publicly available to everybody. If you post news, the entire
    world is likely to read just what you have written, and they’ll all
    snigger mischievously. Behind your back.

*mail* [¶](#index-mail-3)
:   Everything that’s delivered to you personally is mail. Some news/mail
    readers (like Gnus) blur the distinction between mail and news, but
    there is a difference. Mail is private. News is public. Mailing is
    not posting, and replying is not following up.

*reply* [¶](#index-reply-1)
:   Send a mail to the person who has written what you are reading.

*follow up* [¶](#index-follow-up)
:   Post an article to the current newsgroup responding to the article you
    are reading.

*back end* [¶](#index-back-end)
:   Gnus considers mail and news to be mostly the same, really. The only
    difference is how to access the actual articles. News articles are
    commonly fetched via the protocol NNTP, whereas mail
    messages could be read from a file on the local disk. The internal
    architecture of Gnus thus comprises a “front end” and a number of
    “back ends”. Internally, when you enter a group (by hitting
    `RET`, say), you thereby invoke a function in the front end in
    Gnus. The front end then “talks” to a back end and says things like
    “Give me the list of articles in the foo group” or “Show me article
    number 4711”.

    So a back end mainly defines either a protocol (the `nntp` back
    end accesses news via NNTP, the `nnimap` back end
    accesses mail via IMAP) or a file format and directory
    layout (the `nnspool` back end accesses news via the common
    “spool directory” format, the `nnml` back end access mail via a
    file format and directory layout that’s quite similar).

    Gnus does not handle the underlying media, so to speak—this is all
    done by the back ends. A back end is a collection of functions to
    access the articles.

    However, sometimes the term “back end” is also used where “server”
    would have been more appropriate. And then there is the term “select
    method” which can mean either. The Gnus terminology can be quite
    confusing.

*native* [¶](#index-native)
:   Gnus will always use one method (and back end) as the *native*, or
    default, way of getting news. Groups from the native select method
    have names like ‘`gnu.emacs.gnus`’.

*foreign* [¶](#index-foreign)
:   You can also have any number of foreign groups active at the same
    time. These are groups that use non-native non-secondary back ends
    for getting news. Foreign groups have names like
    ‘`nntp+news.gmane.io:gmane.emacs.gnus.devel`’.

*secondary* [¶](#index-secondary)
:   Secondary back ends are somewhere half-way between being native and
    being foreign, but they mostly act like they are native, but they, too
    have names like ‘`nntp+news.gmane.io:gmane.emacs.gnus.devel`’.

*article* [¶](#index-article)
:   A message that has been posted as news.

*mail message* [¶](#index-mail-message)
:   A message that has been mailed.

*message* [¶](#index-message)
:   A mail message or news article

*head* [¶](#index-head)
:   The top part of a message, where administrative information (etc.) is
    put.

*body* [¶](#index-body)
:   The rest of an article. Everything not in the head is in the
    body.

*header* [¶](#index-header)
:   A line from the head of an article.

*headers* [¶](#index-headers)
:   A collection of such lines, or a collection of heads. Or even a
    collection of NOV lines.

*NOV* [¶](#index-NOV-2)
:   NOV stands for News OverView, which is a type of news server
    header which provide data containing the condensed header information
    of articles. They are produced by the server itself; in the `nntp`
    back end Gnus uses the ones that the NNTP server makes, but
    Gnus makes them by itself for some backends (in particular, `nnml`).

    When Gnus enters a group, it asks the back end for the headers of all
    unread articles in the group. Most servers support the News OverView
    format, which is more compact and much faster to read and parse than the
    normal HEAD format.

    The NOV data consist of one or more text lines (see [Motion by Text Lines](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Text-Lines) in The Emacs Lisp Reference Manual)
    where each line has the header information of one article. The header
    information is a tab-separated series of the header’s contents including
    an article number, a subject, an author, a date, a message-id,
    references, etc.

    Those data enable Gnus to generate summary lines quickly. However, if
    the server does not support NOV or you disable it purposely or
    for some reason, Gnus will try to generate the header information by
    parsing each article’s headers one by one. It will take time.
    Therefore, it is not usually a good idea to set nn\*-nov-is-evil
    (see [Slow/Expensive Connection](#Slow_002fExpensive-Connection)) to a non-`nil` value unless you
    know that the server makes wrong NOV data.

*level* [¶](#index-levels)
:   Each group is subscribed at some *level* or other (1–9). The ones
    that have a lower level are “more” subscribed than the groups with a
    higher level. In fact, groups on levels 1–5 are considered
    *subscribed*; 6–7 are *unsubscribed*; 8 are *zombies*; and 9
    are *killed*. Commands for listing groups and scanning for new
    articles will all use the numeric prefix as *working level*.

*killed groups* [¶](#index-killed-groups)
:   No information on killed groups is stored or updated, which makes killed
    groups much easier to handle than subscribed groups.

*zombie groups* [¶](#index-zombie-groups-1)
:   Just like killed groups, only slightly less dead.

*active file* [¶](#index-active-file-1)
:   The news server has to keep track of what articles it carries, and what
    groups exist. All this information in stored in the active file, which
    is rather large, as you might surmise.

*bogus groups* [¶](#index-bogus-groups-1)
:   A group that exists in the `.newsrc` file, but isn’t known to the
    server (i.e., it isn’t in the active file), is a *bogus group*.
    This means that the group probably doesn’t exist (any more).

*activating* [¶](#index-activating-groups-1)
:   The act of asking the server for info on a group and computing the
    number of unread articles is called *activating the group*.
    Un-activated groups are listed with ‘`*`’ in the group buffer.

*spool* [¶](#index-spool)
:   News servers store their articles locally in one fashion or other.
    One old-fashioned storage method is to have just one file per
    article. That’s called a “traditional spool”.

*server* [¶](#index-server)
:   A machine one can connect to and get news (or mail) from.

*select method* [¶](#index-select-method)
:   A structure that specifies the back end, the server and the virtual
    server settings.

*virtual server* [¶](#index-virtual-server)
:   A named select method. Since a select method defines all there is to
    know about connecting to a (physical) server, taking the thing as a
    whole is a virtual server.

*washing* [¶](#index-washing-1)
:   Taking a buffer and running it through a filter of some sort. The
    result will (more often than not) be cleaner and more pleasing than the
    original.

*ephemeral groups* [¶](#index-ephemeral-groups)
:   Most groups store data on what articles you have read. *Ephemeral*
    groups are groups that will have no data stored—when you exit the
    group, it’ll disappear into the aether.

*solid groups* [¶](#index-solid-groups)
:   This is the opposite of ephemeral groups. All groups listed in the
    group buffer are solid groups.

*sparse articles* [¶](#index-sparse-articles)
:   These are article placeholders shown in the summary buffer when
    `gnus-build-sparse-threads` has been switched on.

*threading* [¶](#index-threading-1)
:   To put responses to articles directly after the articles they respond
    to—in a hierarchical fashion.

*root* [¶](#index-root)
:   The first article in a thread is the root. It is the ancestor of all
    articles in the thread.

*parent* [¶](#index-parent)
:   An article that has responses.

*child* [¶](#index-child-1)
:   An article that responds to a different article—its parent.

*digest* [¶](#index-digest)
:   A collection of messages in one file. The most common digest format is
    specified by RFC 1153.

*splitting* [¶](#index-splitting_002c-terminology)
:   The action of sorting your emails according to certain rules. Sometimes
    incorrectly called mail filtering.

---

Next: [Troubleshooting](#Troubleshooting), Previous: [Terminology](#Terminology), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
