Some kinds of files should not be converted at all, because their
contents are not really text. Therefore, Emacs on MS-DOS distinguishes
certain files as *binary files*. (This distinction is not part of
MS-DOS; it is made by Emacs only.) Binary files include executable
programs, compressed archives, etc. Emacs uses the file name to decide
whether to treat a file as binary: the variable
`file-name-buffer-file-type-alist` defines the file-name patterns
that indicate binary files. If a file name matches one of the patterns
for binary files (those whose associations are of the type
`(pattern . t)`, Emacs reads and writes that file using the
`no-conversion` coding system (see section [Q.7 Coding Systems](emacs.html#SEC220)) which turns
off *all* coding-system conversions, not only the EOL conversion.
`file-name-buffer-file-type-alist` also includes file-name patterns
for files which are known to be DOS-style text files with
carriage-return linefeed EOL format, such as `CONFIG.SYS'; Emacs
always writes those files with DOS-style EOLs.

If a file which belongs to an untranslated file system matches one of
the file-name patterns in `file-name-buffer-file-type-alist`, the
EOL conversion is determined by `file-name-buffer-file-type-alist`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC558) ] | [ [>](emacs.html#SEC560) ] | [ [<<](emacs.html#SEC560) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.5 Printing and MS-DOS

Printing commands, such as `lpr-buffer` (see section [AC.18 Hardcopy Output](emacs.html#SEC458)) and
`ps-print-buffer` (see section [AC.19 PostScript Hardcopy](emacs.html#SEC459)) can work in MS-DOS and
MS-Windows by sending the output to one of the printer ports, if a
Posix-style `lpr` program is unavailable. The same Emacs
variables control printing on all systems (see section [AC.18 Hardcopy Output](emacs.html#SEC458)), but in
some cases they have different default values on MS-DOS and
MS-Windows.

If you want to use your local printer, printing on it in the usual DOS
manner, then set the Lisp variable `lpr-command` to `""` (its
default value) and `printer-name` to the name of the printer
port--for example, `"PRN"`, the usual local printer port (that's
the default), or `"LPT2"`, or `"COM1"` for a serial printer.
You can also set `printer-name` to a file name, in which case
"printed" output is actually appended to that file. If you set
`printer-name` to `"NUL"`, printed output is silently
discarded (sent to the system null device).

On MS-Windows, when the Windows network software is installed, you can
also use a printer shared by another machine by setting
`printer-name` to the UNC share name for that printer--for example,
`"//joes_pc/hp4si"`. (It doesn't matter whether you use forward
slashes or backslashes here.) To find out the names of shared printers,
run the command `` `net view' `` at a DOS command prompt to obtain a list
of servers, and `` `net view server-name' `` to see the names of printers
(and directories) shared by that server. Alternatively, click the
`` `Network Neighborhood' `` icon on your desktop, and look for machines
which share their printers via the network.

If the printer doesn't appear in the output of `` `net view' ``, or
if setting `printer-name` to the UNC share name doesn't produce a
hardcopy on that printer, you can use the `` `net use' `` command to
connect a local print port such as `"LPT2"` to the networked
printer. For example, typing `net use LPT2:
\\joes_pc\hp4si`[(18)](emacs.html#FOOT18)
causes Windows to *capture* the LPT2 port and redirect the printed
material to the printer connected to the machine `joes_pc`.
After this command, setting `printer-name` to `"LPT2"`
should produce the hardcopy on the networked printer.

With some varieties of Windows network software, you can instruct
Windows to capture a specific printer port such as `"LPT2"`, and
redirect it to a networked printer via the `Control
Panel->Printers` applet instead of `` `net use' ``.

Some printers expect DOS codepage encoding of non-ASCII text, even
though they are connected to a Windows machine which uses a different
encoding for the same locale. For example, in the Latin-1 locale, DOS
uses codepage 850 whereas Windows uses codepage 1252. See section [AH.6 International Support on MS-DOS](emacs.html#SEC560). When you print to such printers from Windows, you can use the
`C-x RET c` (`universal-coding-system-argument`) command before
`M-x lpr-buffer`; Emacs will then convert the text to the DOS
codepage that you specify. For example, `C-x RET c cp850-dos RET
M-x lpr-region RET` will print the region while converting it to the
codepage 850 encoding. You may need to create the `cpnnn`
coding system with `M-x codepage-setup`.

If you set `printer-name` to a file name, it's best to use an
absolute file name. Emacs changes the working directory according to
the default directory of the current buffer, so if the file name in
`printer-name` is relative, you will end up with several such
files, each one in the directory of the buffer from which the printing
was done.

The commands `print-buffer` and `print-region` call the
`pr` program, or use special switches to the `lpr` program, to
produce headers on each printed page. MS-DOS and MS-Windows don't
normally have these programs, so by default, the variable
`lpr-headers-switches` is set so that the requests to print page
headers are silently ignored. Thus, `print-buffer` and
`print-region` produce the same output as `lpr-buffer` and
`lpr-region`, respectively. If you do have a suitable `pr`
program (for example, from GNU Textutils), set
`lpr-headers-switches` to `nil`; Emacs will then call
`pr` to produce the page headers, and print the resulting output as
specified by `printer-name`.

Finally, if you do have an `lpr` work-alike, you can set the
variable `lpr-command` to `"lpr"`. Then Emacs will use
`lpr` for printing, as on other systems. (If the name of the
program isn't `lpr`, set `lpr-command` to specify where to
find it.) The variable `lpr-switches` has its standard meaning
when `lpr-command` is not `""`. If the variable
`printer-name` has a string value, it is used as the value for the
`-P` option to `lpr`, as on Unix.

A parallel set of variables, `ps-lpr-command`,
`ps-lpr-switches`, and `ps-printer-name` (see section [AC.20 Variables for PostScript Hardcopy](emacs.html#SEC460)), defines how PostScript files should be printed. These
variables are used in the same way as the corresponding variables
described above for non-PostScript printing. Thus, the value of
`ps-printer-name` is used as the name of the device (or file) to
which PostScript output is sent, just as `printer-name` is used for
non-PostScript printing. (There are two distinct sets of variables in
case you have two printers attached to two different ports, and only one
of them is a PostScript printer.)

The default value of the variable `ps-lpr-command` is `""`,
which causes PostScript output to be sent to the printer port specified
by `ps-printer-name`, but `ps-lpr-command` can also be set to
the name of a program which will accept PostScript files. Thus, if you
have a non-PostScript printer, you can set this variable to the name of
a PostScript interpreter program (such as Ghostscript). Any switches
that need to be passed to the interpreter program are specified using
`ps-lpr-switches`. (If the value of `ps-printer-name` is a
string, it will be added to the list of switches as the value for the
`-P` option. This is probably only useful if you are using
`lpr`, so when using an interpreter typically you would set
`ps-printer-name` to something other than a string so it is
ignored.)

For example, to use Ghostscript for printing on an Epson printer
connected to the `` `LPT2' `` port, put this in your `\_emacs' file:

|  |  |
| --- | --- |
|  | ``` (setq ps-printer-name t)  ; Ghostscript doesn't understand -P (setq ps-lpr-command "c:/gs/gs386") (setq ps-lpr-switches '("-q" "-dNOPAUSE" 			"-sDEVICE=epson" 			"-r240x72" 			"-sOutputFile=LPT2" 			"-Ic:/gs")) ``` |

(This assumes that Ghostscript is installed in the `"c:/gs"'
directory.)

For backwards compatibility, the value of `dos-printer`
(`dos-ps-printer`), if it has a value, overrides the value of
`printer-name` (`ps-printer-name`), on MS-DOS and MS-Windows
only.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC559) ] | [ [>](emacs.html#SEC561) ] | [ [<<](emacs.html#SEC561) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.6 International Support on MS-DOS

Emacs on MS-DOS supports the same international character sets as it
does on GNU, Unix and other platforms (see section [Q. International Character Set Support](emacs.html#SEC213)), including
coding systems for converting between the different character sets.
However, due to incompatibilities between MS-DOS/MS-Windows and other systems,
there are several DOS-specific aspects of this support that you should
be aware of. This section describes these aspects.

`M-x dos-codepage-setup`: Set up Emacs display and coding systems as appropriate for the current DOS codepage. `M-x codepage-setup`: Create a coding system for a certain DOS codepage.

MS-DOS is designed to support one character set of 256 characters at
any given time, but gives you a variety of character sets to choose
from. The alternative character sets are known as *DOS codepages*.
Each codepage includes all 128 ASCII characters, but the other 128
characters (codes 128 through 255) vary from one codepage to another.
Each DOS codepage is identified by a 3-digit number, such as 850, 862,
etc.

In contrast to X, which lets you use several fonts at the same time,
MS-DOS normally doesn't allow use of several codepages in a single
session. MS-DOS was designed to load a single codepage at system
startup, and require you to reboot in order to change
it[(19)](emacs.html#FOOT19). Much the same limitation applies when you run DOS
executables on other systems such as MS-Windows.

If you invoke Emacs on MS-DOS with the `` `--unibyte' `` option
(see section [AE.2 Initial Options](emacs.html#SEC530)), Emacs does not perform any conversion of
non-ASCII characters. Instead, it reads and writes any non-ASCII
characters verbatim, and sends their 8-bit codes to the display
verbatim. Thus, unibyte Emacs on MS-DOS supports the current codepage,
whatever it may be, but cannot even represent any other characters.

For multibyte operation on MS-DOS, Emacs needs to know which
characters the chosen DOS codepage can display. So it queries the
system shortly after startup to get the chosen codepage number, and
stores the number in the variable `dos-codepage`. Some systems
return the default value 437 for the current codepage, even though the
actual codepage is different. (This typically happens when you use the
codepage built into the display hardware.) You can specify a different
codepage for Emacs to use by setting the variable `dos-codepage` in
your init file.

Multibyte Emacs supports only certain DOS codepages: those which can
display Far-Eastern scripts, like the Japanese codepage 932, and those
that encode a single ISO 8859 character set.

The Far-Eastern codepages can directly display one of the MULE
character sets for these countries, so Emacs simply sets up to use the
appropriate terminal coding system that is supported by the codepage.
The special features described in the rest of this section mostly
pertain to codepages that encode ISO 8859 character sets.

For the codepages which correspond to one of the ISO character sets,
Emacs knows the character set name based on the codepage number. Emacs
automatically creates a coding system to support reading and writing
files that use the current codepage, and uses this coding system by
default. The name of this coding system is `cpnnn`, where
nnn is the codepage number.[(20)](emacs.html#FOOT20)

All the `cpnnn` coding systems use the letter `` `D' `` (for
"DOS") as their mode-line mnemonic. Since both the terminal coding
system and the default coding system for file I/O are set to the proper
`cpnnn` coding system at startup, it is normal for the mode
line on MS-DOS to begin with `` `-DD\-' ``. See section [B.3 The Mode Line](emacs.html#SEC11).
Far-Eastern DOS terminals do not use the `cpnnn` coding
systems, and thus their initial mode line looks like the Emacs default.

Since the codepage number also indicates which script you are using,
Emacs automatically runs `set-language-environment` to select the
language environment for that script (see section [Q.3 Language Environments](emacs.html#SEC216)).

If a buffer contains a character belonging to some other ISO 8859
character set, not the one that the chosen DOS codepage supports, Emacs
displays it using a sequence of ASCII characters. For example, if the
current codepage doesn't have a glyph for the letter `` `ò' `` (small
`` `o' `` with a grave accent), it is displayed as `` `{`o}' ``, where
the braces serve as a visual indication that this is a single character.
(This may look awkward for some non-Latin characters, such as those from
Greek or Hebrew alphabets, but it is still readable by a person who
knows the language.) Even though the character may occupy several
columns on the screen, it is really still just a single character, and
all Emacs commands treat it as one.

Not all characters in DOS codepages correspond to ISO 8859
characters--some are used for other purposes, such as box-drawing
characters and other graphics. Emacs maps these characters to two
special character sets called `eight-bit-control` and
`eight-bit-graphic`, and displays them as their IBM glyphs.
However, you should be aware that other systems might display these
characters differently, so you should avoid them in text that might be
copied to a different operating system, or even to another DOS machine
that uses a different codepage.

Emacs supports many other characters sets aside from ISO 8859, but it
cannot display them on MS-DOS. So if one of these multibyte characters
appears in a buffer, Emacs on MS-DOS displays them as specified by the
`dos-unsupported-character-glyph` variable; by default, this glyph
is an empty triangle. Use the `C-u C-x =` command to display the
actual code and character set of such characters. See section [D.9 Cursor Position Information](emacs.html#SEC28).

By default, Emacs defines a coding system to support the current
codepage. To define a coding system for some other codepage (e.g., to
visit a file written on a DOS machine in another country), use the
`M-x codepage-setup` command. It prompts for the 3-digit code of
the codepage, with completion, then creates the coding system for the
specified codepage. You can then use the new coding system to read and
write files, but you must specify it explicitly for the file command
when you want to use it (see section [Q.9 Specifying a Coding System](emacs.html#SEC222)).

These coding systems are also useful for visiting a file encoded using
a DOS codepage, using Emacs running on some other operating system.

MS-Windows provides its own codepages, which are different from the
DOS codepages for the same locale. For example, DOS codepage 850
supports the same character set as Windows codepage 1252; DOS codepage
855 supports the same character set as Windows codepage 1251, etc.
The MS-Windows version of Emacs uses the current codepage for display
when invoked with the `` `-nw' `` option.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC560) ] | [ [>](emacs.html#SEC562) ] | [ [<<](emacs.html#SEC562) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.7 Subprocesses on MS-DOS

Because MS-DOS is a single-process "operating system,"
asynchronous subprocesses are not available. In particular, Shell
mode and its variants do not work. Most Emacs features that use
asynchronous subprocesses also don't work on MS-DOS, including
Shell mode and GUD. When in doubt, try and see; commands that
don't work output an error message saying that asynchronous processes
aren't supported.

Compilation under Emacs with `M-x compile`, searching files with
`M-x grep` and displaying differences between files with `M-x
diff` do work, by running the inferior processes synchronously. This
means you cannot do any more editing until the inferior process
finishes.

Spell checking also works, by means of special support for synchronous
invocation of the `ispell` program. This is slower than the
asynchronous invocation on other platforms

Instead of the Shell mode, which doesn't work on MS-DOS, you can use
the `M-x eshell` command. This invokes the Eshell package that
implements a Posix-like shell entirely in Emacs Lisp.

By contrast, Emacs compiled as native Windows application
**does** support asynchronous subprocesses. See section [AH.8 Subprocesses on Windows 9X/ME and Windows NT/2K](emacs.html#SEC562).

Printing commands, such as `lpr-buffer` (see section [AC.18 Hardcopy Output](emacs.html#SEC458)) and
`ps-print-buffer` (see section [AC.19 PostScript Hardcopy](emacs.html#SEC459)), work in MS-DOS by sending
the output to one of the printer ports. See section [AH.5 Printing and MS-DOS](emacs.html#SEC559).

When you run a subprocess synchronously on MS-DOS, make sure the
program terminates and does not try to read keyboard input. If the
program does not terminate on its own, you will be unable to terminate
it, because MS-DOS provides no general way to terminate a process.
Pressing `C-c` or `C-BREAK` might sometimes help in these
cases.

Accessing files on other machines is not supported on MS-DOS. Other
network-oriented commands such as sending mail, Web browsing, remote
login, etc., don't work either, unless network access is built into
MS-DOS with some network redirector.

Dired on MS-DOS uses the `ls-lisp` package where other
platforms use the system `ls` command. Therefore, Dired on
MS-DOS supports only some of the possible options you can mention in
the `dired-listing-switches` variable. The options that work are
`` `-A' ``, `` `-a' ``, `` `-c' ``, `` `-i' ``, `` `-r' ``, `` `-S' ``,
`` `-s' ``, `` `-t' ``, and `` `-u' ``.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC561) ] | [ [>](emacs.html#SEC563) ] | [ [<<](emacs.html#SEC563) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.8 Subprocesses on Windows 9X/ME and Windows NT/2K

Emacs compiled as a native Windows application (as opposed to the DOS
version) includes full support for asynchronous subprocesses.
In the Windows version, synchronous and asynchronous subprocesses work
fine on both
Windows 9X and Windows NT/2K as long as you run only 32-bit Windows
applications. However, when you run a DOS application in a subprocess,
you may encounter problems or be unable to run the application at all;
and if you run two DOS applications at the same time in two
subprocesses, you may have to reboot your system.

Since the standard command interpreter (and most command line utilities)
on Windows 95 are DOS applications, these problems are significant when
using that system. But there's nothing we can do about them; only
Microsoft can fix them.

If you run just one DOS application subprocess, the subprocess should
work as expected as long as it is "well-behaved" and does not perform
direct screen access or other unusual actions. If you have a CPU
monitor application, your machine will appear to be 100% busy even when
the DOS application is idle, but this is only an artifact of the way CPU
monitors measure processor load.

You must terminate the DOS application before you start any other DOS
application in a different subprocess. Emacs is unable to interrupt or
terminate a DOS subprocess. The only way you can terminate such a
subprocess is by giving it a command that tells its program to exit.

If you attempt to run two DOS applications at the same time in separate
subprocesses, the second one that is started will be suspended until the
first one finishes, even if either or both of them are asynchronous.

If you can go to the first subprocess, and tell it to exit, the second
subprocess should continue normally. However, if the second subprocess
is synchronous, Emacs itself will be hung until the first subprocess
finishes. If it will not finish without user input, then you have no
choice but to reboot if you are running on Windows 9X. If you are
running on Windows NT/2K, you can use a process viewer application to kill
the appropriate instance of ntvdm instead (this will terminate both DOS
subprocesses).

If you have to reboot Windows 9X in this situation, do not use the
`Shutdown` command on the `Start` menu; that usually hangs the
system. Instead, type `CTL-ALT-DEL` and then choose
`Shutdown`. That usually works, although it may take a few minutes
to do its job.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC562) ] | [ [>](emacs.html#SEC564) ] | [ [<<](emacs.html#SEC554) ] | [ [Up](emacs.html#SEC554) ] | [ [>>](emacs.html#SEC564) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AH.9 Using the System Menu on Windows

Emacs compiled as a native Windows application normally turns off the
Windows feature that tapping the `ALT`
key invokes the Windows menu. The reason is that the `ALT` also
serves as `META` in Emacs. When using Emacs, users often press the
`META` key temporarily and then change their minds; if this has the
effect of bringing up the Windows menu, it alters the meaning of
subsequent commands. Many users find this frustrating.

You can reenable Windows's default handling of tapping the `ALT` key
by setting `w32-pass-alt-to-system` to a non-`nil` value.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC563) ] | [ [>](emacs.html#SEC565) ] | [ [<<](emacs.html#SEC554) ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# The GNU Manifesto

> The GNU Manifesto which appears below was written by Richard Stallman at
> the beginning of the GNU project, to ask for participation and support.
> For the first few years, it was updated in minor ways to account for
> developments, but now it seems best to leave it unchanged as most people
> have seen it.
>
> Since that time, we have learned about certain common misunderstandings
> that different wording could help avoid. Footnotes added in 1993 help
> clarify these points.
>
> For up-to-date information about the available GNU software, please see
> the latest issue of the GNU's Bulletin. The list is much too long to
> include here.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC564) ] | [ [>](emacs.html#SEC566) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## What's GNU? Gnu's Not Unix!

GNU, which stands for Gnu's Not Unix, is the name for the complete
Unix-compatible software system which I am writing so that I can give it
away free to everyone who can use it.[(21)](emacs.html#FOOT21) Several other volunteers are helping
me. Contributions of time, money, programs and equipment are greatly
needed.

So far we have an Emacs text editor with Lisp for writing editor commands,
a source level debugger, a yacc-compatible parser generator, a linker, and
around 35 utilities. A shell (command interpreter) is nearly completed. A
new portable optimizing C compiler has compiled itself and may be released
this year. An initial kernel exists but many more features are needed to
emulate Unix. When the kernel and compiler are finished, it will be
possible to distribute a GNU system suitable for program development. We
will use TeX as our text formatter, but an nroff is being worked on. We
will use the free, portable X window system as well. After this we will
add a portable Common Lisp, an Empire game, a spreadsheet, and hundreds of
other things, plus on-line documentation. We hope to supply, eventually,
everything useful that normally comes with a Unix system, and more.

GNU will be able to run Unix programs, but will not be identical to Unix.
We will make all improvements that are convenient, based on our experience
with other operating systems. In particular, we plan to have longer
file names, file version numbers, a crashproof file system, file name
completion perhaps, terminal-independent display support, and perhaps
eventually a Lisp-based window system through which several Lisp programs
and ordinary Unix programs can share a screen. Both C and Lisp will be
available as system programming languages. We will try to support UUCP,
MIT Chaosnet, and Internet protocols for communication.

GNU is aimed initially at machines in the 68000/16000 class with virtual
memory, because they are the easiest machines to make it run on. The extra
effort to make it run on smaller machines will be left to someone who wants
to use it on them.

To avoid horrible confusion, please pronounce the `G' in the word `GNU'
when it is the name of this project.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC565) ] | [ [>](emacs.html#SEC567) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Why I Must Write GNU

I consider that the golden rule requires that if I like a program I must
share it with other people who like it. Software sellers want to divide
the users and conquer them, making each user agree not to share with
others. I refuse to break solidarity with other users in this way. I
cannot in good conscience sign a nondisclosure agreement or a software
license agreement. For years I worked within the Artificial Intelligence
Lab to resist such tendencies and other inhospitalities, but eventually
they had gone too far: I could not remain in an institution where such
things are done for me against my will.

So that I can continue to use computers without dishonor, I have decided to
put together a sufficient body of free software so that I will be able to
get along without any software that is not free. I have resigned from the
AI lab to deny MIT any legal excuse to prevent me from giving GNU away.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC566) ] | [ [>](emacs.html#SEC568) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Why GNU Will Be Compatible with Unix

Unix is not my ideal system, but it is not too bad. The essential features
of Unix seem to be good ones, and I think I can fill in what Unix lacks
without spoiling them. And a system compatible with Unix would be
convenient for many other people to adopt.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC567) ] | [ [>](emacs.html#SEC569) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## How GNU Will Be Available

GNU is not in the public domain. Everyone will be permitted to modify and
redistribute GNU, but no distributor will be allowed to restrict its
further redistribution. That is to say, proprietary modifications will not
be allowed. I want to make sure that all versions of GNU remain free.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC568) ] | [ [>](emacs.html#SEC570) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Why Many Other Programmers Want to Help

I have found many other programmers who are excited about GNU and want to
help.

Many programmers are unhappy about the commercialization of system
software. It may enable them to make more money, but it requires them to
feel in conflict with other programmers in general rather than feel as
comrades. The fundamental act of friendship among programmers is the
sharing of programs; marketing arrangements now typically used essentially
forbid programmers to treat others as friends. The purchaser of software
must choose between friendship and obeying the law. Naturally, many decide
that friendship is more important. But those who believe in law often do
not feel at ease with either choice. They become cynical and think that
programming is just a way of making money.

By working on and using GNU rather than proprietary programs, we can be
hospitable to everyone and obey the law. In addition, GNU serves as an
example to inspire and a banner to rally others to join us in sharing.
This can give us a feeling of harmony which is impossible if we use
software that is not free. For about half the programmers I talk to, this
is an important happiness that money cannot replace.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC569) ] | [ [>](emacs.html#SEC571) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## How You Can Contribute

I am asking computer manufacturers for donations of machines and money.
I'm asking individuals for donations of programs and work.

One consequence you can expect if you donate machines is that GNU will run
on them at an early date. The machines should be complete, ready to use
systems, approved for use in a residential area, and not in need of
sophisticated cooling or power.

I have found very many programmers eager to contribute part-time work for
GNU. For most projects, such part-time distributed work would be very hard
to coordinate; the independently-written parts would not work together.
But for the particular task of replacing Unix, this problem is absent. A
complete Unix system contains hundreds of utility programs, each of which
is documented separately. Most interface specifications are fixed by Unix
compatibility. If each contributor can write a compatible replacement for
a single Unix utility, and make it work properly in place of the original
on a Unix system, then these utilities will work right when put together.
Even allowing for Murphy to create a few unexpected problems, assembling
these components will be a feasible task. (The kernel will require closer
communication and will be worked on by a small, tight group.)

If I get donations of money, I may be able to hire a few people full or
part time. The salary won't be high by programmers' standards, but I'm
looking for people for whom building community spirit is as important as
making money. I view this as a way of enabling dedicated people to devote
their full energies to working on GNU by sparing them the need to make a
living in another way.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC570) ] | [ [>](emacs.html#SEC572) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Why All Computer Users Will Benefit

Once GNU is written, everyone will be able to obtain good system
software free, just like air.[(22)](emacs.html#FOOT22)

This means much more than just saving everyone the price of a Unix license.
It means that much wasteful duplication of system programming effort will
be avoided. This effort can go instead into advancing the state of the
art.

Complete system sources will be available to everyone. As a result, a user
who needs changes in the system will always be free to make them himself,
or hire any available programmer or company to make them for him. Users
will no longer be at the mercy of one programmer or company which owns the
sources and is in sole position to make changes.

Schools will be able to provide a much more educational environment by
encouraging all students to study and improve the system code. Harvard's
computer lab used to have the policy that no program could be installed on
the system if its sources were not on public display, and upheld it by
actually refusing to install certain programs. I was very much inspired by
this.

Finally, the overhead of considering who owns the system software and what
one is or is not entitled to do with it will be lifted.

Arrangements to make people pay for using a program, including licensing of
copies, always incur a tremendous cost to society through the cumbersome
mechanisms necessary to figure out how much (that is, which programs) a
person must pay for. And only a police state can force everyone to obey
them. Consider a space station where air must be manufactured at great
cost: charging each breather per liter of air may be fair, but wearing the
metered gas mask all day and all night is intolerable even if everyone can
afford to pay the air bill. And the TV cameras everywhere to see if you
ever take the mask off are outrageous. It's better to support the air
plant with a head tax and chuck the masks.

Copying all or parts of a program is as natural to a programmer as
breathing, and as productive. It ought to be as free.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC571) ] | [ [>](emacs.html#SEC573) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Some Easily Rebutted Objections to GNU's Goals

> "Nobody will use it if it is free, because that means they can't rely
> on any support."
>
> "You have to charge for the program to pay for providing the
> support."

If people would rather pay for GNU plus service than get GNU free without
service, a company to provide just service to people who have obtained GNU
free ought to be profitable.[(23)](emacs.html#FOOT23)

We must distinguish between support in the form of real programming work
and mere handholding. The former is something one cannot rely on from a
software vendor. If your problem is not shared by enough people, the
vendor will tell you to get lost.

If your business needs to be able to rely on support, the only way is to
have all the necessary sources and tools. Then you can hire any available
person to fix your problem; you are not at the mercy of any individual.
With Unix, the price of sources puts this out of consideration for most
businesses. With GNU this will be easy. It is still possible for there to
be no available competent person, but this problem cannot be blamed on
distribution arrangements. GNU does not eliminate all the world's problems,
only some of them.

Meanwhile, the users who know nothing about computers need handholding:
doing things for them which they could easily do themselves but don't know
how.

Such services could be provided by companies that sell just hand-holding
and repair service. If it is true that users would rather spend money and
get a product with service, they will also be willing to buy the service
having got the product free. The service companies will compete in quality
and price; users will not be tied to any particular one. Meanwhile, those
of us who don't need the service should be able to use the program without
paying for the service.

> "You cannot reach many people without advertising,
> and you must charge for the program to support that."
>
> "It's no use advertising a program people can get free."

There are various forms of free or very cheap publicity that can be used to
inform numbers of computer users about something like GNU. But it may be
true that one can reach more microcomputer users with advertising. If this
is really so, a business which advertises the service of copying and
mailing GNU for a fee ought to be successful enough to pay for its
advertising and more. This way, only the users who benefit from the
advertising pay for it.

On the other hand, if many people get GNU from their friends, and such
companies don't succeed, this will show that advertising was not really
necessary to spread GNU. Why is it that free market advocates don't
want to let the free market decide this?[(24)](emacs.html#FOOT24)

> "My company needs a proprietary operating system
> to get a competitive edge."

GNU will remove operating system software from the realm of competition.
You will not be able to get an edge in this area, but neither will your
competitors be able to get an edge over you. You and they will compete in
other areas, while benefiting mutually in this one. If your business is
selling an operating system, you will not like GNU, but that's tough on
you. If your business is something else, GNU can save you from being
pushed into the expensive business of selling operating systems.

I would like to see GNU development supported by gifts from many
manufacturers and users, reducing the cost to each.[(25)](emacs.html#FOOT25)

> "Don't programmers deserve a reward for their creativity?"

If anything deserves a reward, it is social contribution. Creativity can
be a social contribution, but only in so far as society is free to use the
results. If programmers deserve to be rewarded for creating innovative
programs, by the same token they deserve to be punished if they restrict
the use of these programs.

> "Shouldn't a programmer be able to ask for a reward for his creativity?"

There is nothing wrong with wanting pay for work, or seeking to maximize
one's income, as long as one does not use means that are destructive. But
the means customary in the field of software today are based on
destruction.

Extracting money from users of a program by restricting their use of it is
destructive because the restrictions reduce the amount and the ways that
the program can be used. This reduces the amount of wealth that humanity
derives from the program. When there is a deliberate choice to restrict,
the harmful consequences are deliberate destruction.

The reason a good citizen does not use such destructive means to become
wealthier is that, if everyone did so, we would all become poorer from the
mutual destructiveness. This is Kantian ethics; or, the Golden Rule.
Since I do not like the consequences that result if everyone hoards
information, I am required to consider it wrong for one to do so.
Specifically, the desire to be rewarded for one's creativity does not
justify depriving the world in general of all or part of that creativity.

> "Won't programmers starve?"

I could answer that nobody is forced to be a programmer. Most of us cannot
manage to get any money for standing on the street and making faces. But
we are not, as a result, condemned to spend our lives standing on the
street making faces, and starving. We do something else.

But that is the wrong answer because it accepts the questioner's implicit
assumption: that without ownership of software, programmers cannot possibly
be paid a cent. Supposedly it is all or nothing.

The real reason programmers will not starve is that it will still be
possible for them to get paid for programming; just not paid as much as
now.

Restricting copying is not the only basis for business in software. It is
the most common basis because it brings in the most money. If it were
prohibited, or rejected by the customer, software business would move to
other bases of organization which are now used less often. There are
always numerous ways to organize any kind of business.

Probably programming will not be as lucrative on the new basis as it is
now. But that is not an argument against the change. It is not considered
an injustice that sales clerks make the salaries that they now do. If
programmers made the same, that would not be an injustice either. (In
practice they would still make considerably more than that.)

> "Don't people have a right to control how their creativity is used?"

"Control over the use of one's ideas" really constitutes control over
other people's lives; and it is usually used to make their lives more
difficult.

People who have studied the issue of intellectual property rights carefully
(such as lawyers) say that there is no intrinsic right to intellectual
property. The kinds of supposed intellectual property rights that the
government recognizes were created by specific acts of legislation for
specific purposes.

For example, the patent system was established to encourage inventors to
disclose the details of their inventions. Its purpose was to help society
rather than to help inventors. At the time, the life span of 17 years for
a patent was short compared with the rate of advance of the state of the
art. Since patents are an issue only among manufacturers, for whom the
cost and effort of a license agreement are small compared with setting up
production, the patents often do not do much harm. They do not obstruct
most individuals who use patented products.

The idea of copyright did not exist in ancient times, when authors
frequently copied other authors at length in works of non-fiction. This
practice was useful, and is the only way many authors' works have survived
even in part. The copyright system was created expressly for the purpose
of encouraging authorship. In the domain for which it was
invented--books, which could be copied economically only on a printing
press--it did little harm, and did not obstruct most of the individuals
who read the books.

All intellectual property rights are just licenses granted by society
because it was thought, rightly or wrongly, that society as a whole would
benefit by granting them. But in any particular situation, we have to ask:
are we really better off granting such license? What kind of act are we
licensing a person to do?

The case of programs today is very different from that of books a hundred
years ago. The fact that the easiest way to copy a program is from one
neighbor to another, the fact that a program has both source code and
object code which are distinct, and the fact that a program is used rather
than read and enjoyed, combine to create a situation in which a person who
enforces a copyright is harming society as a whole both materially and
spiritually; in which a person should not do so regardless of whether the
law enables him to.

> "Competition makes things get done better."

The paradigm of competition is a race: by rewarding the winner, we
encourage everyone to run faster. When capitalism really works this way,
it does a good job; but its defenders are wrong in assuming it always works
this way. If the runners forget why the reward is offered and become
intent on winning, no matter how, they may find other strategies--such as,
attacking other runners. If the runners get into a fist fight, they will
all finish late.

Proprietary and secret software is the moral equivalent of runners in a
fist fight. Sad to say, the only referee we've got does not seem to
object to fights; he just regulates them ("For every ten yards you run,
you can fire one shot"). He really ought to break them up, and penalize
runners for even trying to fight.

> "Won't everyone stop programming without a monetary incentive?"

Actually, many people will program with absolutely no monetary incentive.
Programming has an irresistible fascination for some people, usually the
people who are best at it. There is no shortage of professional musicians
who keep at it even though they have no hope of making a living that way.

But really this question, though commonly asked, is not appropriate to the
situation. Pay for programmers will not disappear, only become less. So
the right question is, will anyone program with a reduced monetary
incentive? My experience shows that they will.

For more than ten years, many of the world's best programmers worked at the
Artificial Intelligence Lab for far less money than they could have had
anywhere else. They got many kinds of non-monetary rewards: fame and
appreciation, for example. And creativity is also fun, a reward in itself.

Then most of them left when offered a chance to do the same interesting
work for a lot of money.

What the facts show is that people will program for reasons other than
riches; but if given a chance to make a lot of money as well, they will
come to expect and demand it. Low-paying organizations do poorly in
competition with high-paying ones, but they do not have to do badly if the
high-paying ones are banned.

> "We need the programmers desperately. If they demand that we
> stop helping our neighbors, we have to obey."

You're never so desperate that you have to obey this sort of demand.
Remember: millions for defense, but not a cent for tribute!

> "Programmers need to make a living somehow."

In the short run, this is true. However, there are plenty of ways that
programmers could make a living without selling the right to use a program.
This way is customary now because it brings programmers and businessmen the
most money, not because it is the only way to make a living. It is easy to
find other ways if you want to find them. Here are a number of examples.

A manufacturer introducing a new computer will pay for the porting of
operating systems onto the new hardware.

The sale of teaching, hand-holding and maintenance services could also
employ programmers.

People with new ideas could distribute programs as freeware, asking for
donations from satisfied users, or selling hand-holding services. I have
met people who are already working this way successfully.

Users with related needs can form users' groups, and pay dues. A group
would contract with programming companies to write programs that the
group's members would like to use.

All sorts of development can be funded with a Software Tax:

> Suppose everyone who buys a computer has to pay x percent of
> the price as a software tax. The government gives this to
> an agency like the NSF to spend on software development.
>
> But if the computer buyer makes a donation to software development
> himself, he can take a credit against the tax. He can donate to
> the project of his own choosing--often, chosen because he hopes to
> use the results when it is done. He can take a credit for any amount
> of donation up to the total tax he had to pay.
>
> The total tax rate could be decided by a vote of the payers of
> the tax, weighted according to the amount they will be taxed on.
>
> The consequences:
>
> * The computer-using community supports software development.* This community decides what level of support is needed.* Users who care which projects their share is spent on
>       can choose this for themselves.

In the long run, making programs free is a step toward the post-scarcity
world, where nobody will have to work very hard just to make a living.
People will be free to devote themselves to activities that are fun, such
as programming, after spending the necessary ten hours a week on required
tasks such as legislation, family counseling, robot repair and asteroid
prospecting. There will be no need to be able to make a living from
programming.

We have already greatly reduced the amount of work that the whole society
must do for its actual productivity, but only a little of this has
translated itself into leisure for workers because much nonproductive
activity is required to accompany productive activity. The main causes of
this are bureaucracy and isometric struggles against competition. Free
software will greatly reduce these drains in the area of software
production. We must do this, in order for technical gains in productivity
to translate into less work for us.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC572) ] | [ [>](emacs.html#SEC574) ] | [ [<<](emacs.html#SEC7) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC575) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# Glossary
