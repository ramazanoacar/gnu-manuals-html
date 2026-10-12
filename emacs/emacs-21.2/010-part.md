A more powerful technique is composition: converting sequences of
characters into one letter. Many European input methods use composition
to produce a single non-ASCII letter from a sequence that consists of a
letter followed by accent characters (or vice versa). For example, some
methods convert the sequence `a'` into a single accented letter.
These input methods have no special commands of their own; all they do
is compose sequences of printing characters.

The input methods for syllabic scripts typically use mapping followed
by composition. The input methods for Thai and Korean work this way.
First, letters are mapped into symbols for particular sounds or tone
marks; then, sequences of these which make up a whole syllable are
mapped into one syllable sign.

Chinese and Japanese require more complex methods. In Chinese input
methods, first you enter the phonetic spelling of a Chinese word (in
input method `chinese-py`, among others), or a sequence of
portions of the character (input methods `chinese-4corner` and
`chinese-sw`, and others). One input sequence typically
corresponds to many possible Chinese characters. You select the one
you mean using keys such as `C-f`, `C-b`, `C-n`,
`C-p`, and digits, which have special meanings in this situation.

The possible characters are conceptually arranged in several rows,
with each row holding up to 10 alternatives. Normally, Emacs displays
just one row at a time, in the echo area; `(i/j)`
appears at the beginning, to indicate that this is the ith row
out of a total of j rows. Type `C-n` or `C-p` to
display the next row or the previous row.

Type `C-f` and `C-b` to move forward and backward among
the alternatives in the current row. As you do this, Emacs highlights
the current alternative with a special color; type `C-SPC`
to select the current alternative and use it as input. The
alternatives in the row are also numbered; the number appears before
the alternative. Typing a digit n selects the nth
alternative of the current row and uses it as input.

`TAB` in these Chinese input methods displays a buffer showing
all the possible characters at once; then clicking `Mouse-2` on
one of them selects that alternative. The keys `C-f`, `C-b`,
`C-n`, `C-p`, and digits continue to work as usual, but they
do the highlighting in the buffer showing the possible characters,
rather than in the echo area.

In Japanese input methods, first you input a whole word using
phonetic spelling; then, after the word is in the buffer, Emacs
converts it into one or more characters using a large dictionary. One
phonetic spelling corresponds to a number of different Japanese words;
to select one of them, use `C-n` and `C-p` to cycle through
the alternatives.

Sometimes it is useful to cut off input method processing so that the
characters you have just entered will not combine with subsequent
characters. For example, in input method `latin-1-postfix`, the
sequence `e '` combines to form an `` `e' `` with an accent. What if
you want to enter them as separate characters?

One way is to type the accent twice; this is a special feature for
entering the separate letter and accent. For example, `e ' '` gives
you the two characters `` `e'' ``. Another way is to type another letter
after the `e`---something that won't combine with that--and
immediately delete it. For example, you could type `e e DEL
'` to get separate `` `e' `` and `` `'' ``.

Another method, more general but not quite as easy to type, is to use
`C-\ C-\` between two characters to stop them from combining. This
is the command `C-\` (`toggle-input-method`) used twice.
See section [Q.5 Selecting an Input Method](emacs.html#SEC218).

`C-\ C-\` is especially useful inside an incremental search,
because it stops waiting for more characters to combine, and starts
searching for what you have already entered.

The variables `input-method-highlight-flag` and
`input-method-verbose-flag` control how input methods explain
what is happening. If `input-method-highlight-flag` is
non-`nil`, the partial sequence is highlighted in the buffer (for
most input methods--some disable this feature). If
`input-method-verbose-flag` is non-`nil`, the list of
possible characters to type next is displayed in the echo area (but
not when you are in the minibuffer).

Input methods are implemented in the separate Leim package: they are
available only if the system administrator used Leim when building
Emacs. If Emacs was built without Leim, you will find that no input
methods are defined.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC217) ] | [ [>](emacs.html#SEC219) ] | [ [<<](emacs.html#SEC219) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.5 Selecting an Input Method

`C-\`: Enable or disable use of the selected input method. `C-x RET C-\ method RET`: Select a new input method for the current buffer. `C-h I method RET`: `C-h C-\ method RET`: Describe the input method method (`describe-input-method`). By default, it describes the current input method (if any). This description should give you the full details of how to use any particular input method. `M-x list-input-methods`: Display a list of all the supported input methods.

To choose an input method for the current buffer, use `C-x
RET C-\` (`set-input-method`). This command reads the
input method name from the minibuffer; the name normally starts with the
language environment that it is meant to be used with. The variable
`current-input-method` records which input method is selected.

Input methods use various sequences of ASCII characters to stand for
non-ASCII characters. Sometimes it is useful to turn off the input
method temporarily. To do this, type `C-\`
(`toggle-input-method`). To reenable the input method, type
`C-\` again.

If you type `C-\` and you have not yet selected an input method,
it prompts for you to specify one. This has the same effect as using
`C-x RET C-\` to specify an input method.

When invoked with a numeric argument, as in `C-u C-\`,
`toggle-input-method` always prompts you for an input method,
suggesting the most recently selected one as the default.

Selecting a language environment specifies a default input method for
use in various buffers. When you have a default input method, you can
select it in the current buffer by typing `C-\`. The variable
`default-input-method` specifies the default input method
(`nil` means there is none).

In some language environments, which support several different input
methods, you might want to use an input method different from the
default chosen by `set-language-environment`. You can instruct
Emacs to select a different default input method for a certain
language environment, if you wish, by using
`set-language-environment-hook` (see section [set-language-environment-hook](emacs.html#SEC216)). For example:

|  |  |
| --- | --- |
|  | ``` (defun my-chinese-setup ()   "Set up my private Chinese environment."   (if (equal current-language-environment "Chinese-GB")       (setq default-input-method "chinese-tonepy"))) (add-hook 'set-language-environment-hook 'my-chinese-setup) ``` |

This sets the default input method to be `chinese-tonepy`
whenever you choose a Chinese-GB language environment.

Some input methods for alphabetic scripts work by (in effect)
remapping the keyboard to emulate various keyboard layouts commonly used
for those scripts. How to do this remapping properly depends on your
actual keyboard layout. To specify which layout your keyboard has, use
the command `M-x quail-set-keyboard-layout`.

To display a list of all the supported input methods, type `M-x
list-input-methods`. The list gives information about each input
method, including the string that stands for it in the mode line.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC218) ] | [ [>](emacs.html#SEC220) ] | [ [<<](emacs.html#SEC220) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.6 Unibyte and Multibyte Non-ASCII characters

When multibyte characters are enabled, character codes 0240 (octal)
through 0377 (octal) are not really legitimate in the buffer. The valid
non-ASCII printing characters have codes that start from 0400.

If you type a self-inserting character in the range 0240 through
0377, or if you use `C-q` to insert one, Emacs assumes you
intended to use one of the ISO Latin-n character sets, and
converts it to the Emacs code representing that Latin-n
character. You select *which* ISO Latin character set to use
through your choice of language environment
(see section [Q.3 Language Environments](emacs.html#SEC216)).
If you do not specify a choice, the default is Latin-1.

If you insert a character in the range 0200 through 0237, which
forms the `eight-bit-control` character set, it is inserted
literally. You should normally avoid doing this since buffers
containing such characters have to be written out in either the
`emacs-mule` or `raw-text` coding system, which is usually
not what you want.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC219) ] | [ [>](emacs.html#SEC221) ] | [ [<<](emacs.html#SEC221) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.7 Coding Systems

Users of various languages have established many more-or-less standard
coding systems for representing them. Emacs does not use these coding
systems internally; instead, it converts from various coding systems to
its own system when reading data, and converts the internal coding
system to other coding systems when writing data. Conversion is
possible in reading or writing files, in sending or receiving from the
terminal, and in exchanging data with subprocesses.

Emacs assigns a name to each coding system. Most coding systems are
used for one language, and the name of the coding system starts with the
language name. Some coding systems are used for several languages;
their names usually start with `` `iso' ``. There are also special
coding systems `no-conversion`, `raw-text` and
`emacs-mule` which do not convert printing characters at all.

A special class of coding systems, collectively known as
*codepages*, is designed to support text encoded by MS-Windows and
MS-DOS software. To use any of these systems, you need to create it
with `M-x codepage-setup`. See section [AH.6 International Support on MS-DOS](emacs.html#SEC560). After
creating the coding system for the codepage, you can use it as any
other coding system. For example, to visit a file encoded in codepage
850, type `C-x RET c cp850 RET C-x C-f filename
RET`.

In addition to converting various representations of non-ASCII
characters, a coding system can perform end-of-line conversion. Emacs
handles three different conventions for how to separate lines in a file:
newline, carriage-return linefeed, and just carriage-return.

`C-h C coding RET`: Describe coding system coding. `C-h C RET`: Describe the coding systems currently in use. `M-x list-coding-systems`: Display a list of all the supported coding systems.

The command `C-h C` (`describe-coding-system`) displays
information about particular coding systems. You can specify a coding
system name as the argument; alternatively, with an empty argument, it
describes the coding systems currently selected for various purposes,
both in the current buffer and as the defaults, and the priority list
for recognizing coding systems (see section [Q.8 Recognizing Coding Systems](emacs.html#SEC221)).

To display a list of all the supported coding systems, type `M-x
list-coding-systems`. The list gives information about each coding
system, including the letter that stands for it in the mode line
(see section [B.3 The Mode Line](emacs.html#SEC11)).

Each of the coding systems that appear in this list--except for
`no-conversion`, which means no conversion of any kind--specifies
how and whether to convert printing characters, but leaves the choice of
end-of-line conversion to be decided based on the contents of each file.
For example, if the file appears to use the sequence carriage-return
linefeed to separate lines, DOS end-of-line conversion will be used.

Each of the listed coding systems has three variants which specify
exactly what to do for end-of-line conversion:

`...-unix`: Don't do any end-of-line conversion; assume the file uses newline to separate lines. (This is the convention normally used on Unix and GNU systems.) `...-dos`: Assume the file uses carriage-return linefeed to separate lines, and do the appropriate conversion. (This is the convention normally used on Microsoft systems.[(6)](emacs.html#FOOT6)) `...-mac`: Assume the file uses carriage-return to separate lines, and do the appropriate conversion. (This is the convention normally used on the Macintosh system.)

These variant coding systems are omitted from the
`list-coding-systems` display for brevity, since they are entirely
predictable. For example, the coding system `iso-latin-1` has
variants `iso-latin-1-unix`, `iso-latin-1-dos` and
`iso-latin-1-mac`.

The coding system `raw-text` is good for a file which is mainly
ASCII text, but may contain byte values above 127 which are not meant to
encode non-ASCII characters. With `raw-text`, Emacs copies those
byte values unchanged, and sets `enable-multibyte-characters` to
`nil` in the current buffer so that they will be interpreted
properly. `raw-text` handles end-of-line conversion in the usual
way, based on the data encountered, and has the usual three variants to
specify the kind of end-of-line conversion to use.

In contrast, the coding system `no-conversion` specifies no
character code conversion at all--none for non-ASCII byte values and
none for end of line. This is useful for reading or writing binary
files, tar files, and other files that must be examined verbatim. It,
too, sets `enable-multibyte-characters` to `nil`.

The easiest way to edit a file with no conversion of any kind is with
the `M-x find-file-literally` command. This uses
`no-conversion`, and also suppresses other Emacs features that
might convert the file contents before you see them. See section [M.2 Visiting Files](emacs.html#SEC112).

The coding system `emacs-mule` means that the file contains
non-ASCII characters stored with the internal Emacs encoding. It
handles end-of-line conversion based on the data encountered, and has
the usual three variants to specify the kind of end-of-line conversion.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC220) ] | [ [>](emacs.html#SEC222) ] | [ [<<](emacs.html#SEC222) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.8 Recognizing Coding Systems

Emacs tries to recognize which coding system to use for a given text
as an integral part of reading that text. (This applies to files
being read, output from subprocesses, text from X selections, etc.)
Emacs can select the right coding system automatically most of the
time--once you have specified your preferences.

Some coding systems can be recognized or distinguished by which byte
sequences appear in the data. However, there are coding systems that
cannot be distinguished, not even potentially. For example, there is no
way to distinguish between Latin-1 and Latin-2; they use the same byte
values with different meanings.

Emacs handles this situation by means of a priority list of coding
systems. Whenever Emacs reads a file, if you do not specify the coding
system to use, Emacs checks the data against each coding system,
starting with the first in priority and working down the list, until it
finds a coding system that fits the data. Then it converts the file
contents assuming that they are represented in this coding system.

The priority list of coding systems depends on the selected language
environment (see section [Q.3 Language Environments](emacs.html#SEC216)). For example, if you use
French, you probably want Emacs to prefer Latin-1 to Latin-2; if you use
Czech, you probably want Latin-2 to be preferred. This is one of the
reasons to specify a language environment.

However, you can alter the priority list in detail with the command
`M-x prefer-coding-system`. This command reads the name of a coding
system from the minibuffer, and adds it to the front of the priority
list, so that it is preferred to all others. If you use this command
several times, each use adds one element to the front of the priority
list.

If you use a coding system that specifies the end-of-line conversion
type, such as `iso-8859-1-dos`, what this means is that Emacs
should attempt to recognize `iso-8859-1` with priority, and should
use DOS end-of-line conversion when it does recognize `iso-8859-1`.

Sometimes a file name indicates which coding system to use for the
file. The variable `file-coding-system-alist` specifies this
correspondence. There is a special function
`modify-coding-system-alist` for adding elements to this list. For
example, to read and write all `` `.txt' `` files using the coding system
`china-iso-8bit`, you can execute this Lisp expression:

|  |  |
| --- | --- |
|  | ``` (modify-coding-system-alist 'file "\\.txt\\'" 'china-iso-8bit) ``` |

The first argument should be `file`, the second argument should be
a regular expression that determines which files this applies to, and
the third argument says which coding system to use for these files.

Emacs recognizes which kind of end-of-line conversion to use based on
the contents of the file: if it sees only carriage-returns, or only
carriage-return linefeed sequences, then it chooses the end-of-line
conversion accordingly. You can inhibit the automatic use of
end-of-line conversion by setting the variable `inhibit-eol-conversion`
to non-`nil`. If you do that, DOS-style files will be displayed
with the `` `^M' `` characters visible in the buffer; some people
prefer this to the more subtle `` `(DOS)' `` end-of-line type
indication near the left edge of the mode line (see section [eol-mnemonic](emacs.html#SEC11)).

By default, the automatic detection of coding system is sensitive to
escape sequences. If Emacs sees a sequence of characters that begin
with an escape character, and the sequence is valid as an ISO-2022
code, that tells Emacs to use one of the ISO-2022 encodings to decode
the file.

However, there may be cases that you want to read escape sequences
in a file as is. In such a case, you can set the variable
`inhibit-iso-escape-detection` to non-`nil`. Then the code
detection ignores any escape sequences, and never uses an ISO-2022
encoding. The result is that all escape sequences become visible in
the buffer.

The default value of `inhibit-iso-escape-detection` is
`nil`. We recommend that you not change it permanently, only for
one specific operation. That's because many Emacs Lisp source files
in the Emacs distribution contain non-ASCII characters encoded in the
coding system `iso-2022-7bit`, and they won't be
decoded correctly when you visit those files if you suppress the
escape sequence detection.

You can specify the coding system for a particular file using the
`` `-*-...-*-' `` construct at the beginning of a file, or a
local variables list at the end (see section [AD.2.5 Local Variables in Files](emacs.html#SEC486)). You do this
by defining a value for the "variable" named `coding`. Emacs
does not really have a variable `coding`; instead of setting a
variable, this uses the specified coding system for the file. For
example, `` `-*-mode: C; coding: latin-1;-*-' `` specifies use of the
Latin-1 coding system, as well as C mode. When you specify the coding
explicitly in the file, that overrides
`file-coding-system-alist`.

The variables `auto-coding-alist` and
`auto-coding-regexp-alist` are the strongest way to specify the
coding system for certain patterns of file names, or for files
containing certain patterns; these variables even override
`` `-*-coding:-*-' `` tags in the file itself. Emacs uses
`auto-coding-alist` for tar and archive files, to prevent it
from being confused by a `` `-*-coding:-*-' `` tag in a member of the
archive and thinking it applies to the archive file as a whole.
Likewise, Emacs uses `auto-coding-regexp-alist` to ensure that
RMAIL files, whose names in general don't match any particular pattern,
are decoded correctly.

If Emacs recognizes the encoding of a file incorrectly, you can
reread the file using the correct coding system by typing `C-x
RET c coding-system RET M-x revert-buffer
RET`. To see what coding system Emacs actually used to decode
the file, look at the coding system mnemonic letter near the left edge
of the mode line (see section [B.3 The Mode Line](emacs.html#SEC11)), or type `C-h C RET`.

Once Emacs has chosen a coding system for a buffer, it stores that
coding system in `buffer-file-coding-system` and uses that coding
system, by default, for operations that write from this buffer into a
file. This includes the commands `save-buffer` and
`write-region`. If you want to write files from this buffer using
a different coding system, you can specify a different coding system for
the buffer using `set-buffer-file-coding-system` (see section [Q.9 Specifying a Coding System](emacs.html#SEC222)).

You can insert any possible character into any Emacs buffer, but
most coding systems can only handle some of the possible characters.
This means that it is possible for you to insert characters that
cannot be encoded with the coding system that will be used to save the
buffer. For example, you could start with an ASCII file and insert a
few Latin-1 characters into it, or you could edit a text file in
Polish encoded in `iso-8859-2` and add some Russian words to it.
When you save the buffer, Emacs cannot use the current value of
`buffer-file-coding-system`, because the characters you added
cannot be encoded by that coding system.

When that happens, Emacs tries the most-preferred coding system (set
by `M-x prefer-coding-system` or `M-x
set-language-environment`), and if that coding system can safely
encode all of the characters in the buffer, Emacs uses it, and stores
its value in `buffer-file-coding-system`. Otherwise, Emacs
displays a list of coding systems suitable for encoding the buffer's
contents, and asks you to choose one of those coding systems.

If you insert the unsuitable characters in a mail message, Emacs
behaves a bit differently. It additionally checks whether the
most-preferred coding system is recommended for use in MIME messages;
if not, Emacs tells you that the most-preferred coding system is
not recommended and prompts you for another coding system. This is so
you won't inadvertently send a message encoded in a way that your
recipient's mail software will have difficulty decoding. (If you do
want to use the most-preferred coding system, you can still type its
name in response to the question.)

When you send a message with Mail mode (see section [Z. Sending Mail](emacs.html#SEC363)), Emacs has
four different ways to determine the coding system to use for encoding
the message text. It tries the buffer's own value of
`buffer-file-coding-system`, if that is non-`nil`. Otherwise,
it uses the value of `sendmail-coding-system`, if that is
non-`nil`. The third way is to use the default coding system for
new files, which is controlled by your choice of language environment,
if that is non-`nil`. If all of these three values are `nil`,
Emacs encodes outgoing mail using the Latin-1 coding system.

When you get new mail in Rmail, each message is translated
automatically from the coding system it is written in, as if it were a
separate file. This uses the priority list of coding systems that you
have specified. If a MIME message specifies a character set, Rmail
obeys that specification, unless `rmail-decode-mime-charset` is
`nil`.

For reading and saving Rmail files themselves, Emacs uses the coding
system specified by the variable `rmail-file-coding-system`. The
default value is `nil`, which means that Rmail files are not
translated (they are read and written in the Emacs internal character
code).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC221) ] | [ [>](emacs.html#SEC223) ] | [ [<<](emacs.html#SEC213) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.9 Specifying a Coding System

In cases where Emacs does not automatically choose the right coding
system, you can use these commands to specify one:

`C-x RET f coding RET`: Use coding system coding for the visited file in the current buffer. `C-x RET c coding RET`: Specify coding system coding for the immediately following command. `C-x RET k coding RET`: Use coding system coding for keyboard input. `C-x RET t coding RET`: Use coding system coding for terminal output. `C-x RET p input-coding RET output-coding RET`: Use coding systems input-coding and output-coding for subprocess input and output in the current buffer. `C-x RET x coding RET`: Use coding system coding for transferring selections to and from other programs through the window system. `C-x RET X coding RET`: Use coding system coding for transferring *one* selection--the next one--to or from the window system.

The command `C-x RET f` (`set-buffer-file-coding-system`)
specifies the file coding system for the current buffer--in other
words, which coding system to use when saving or rereading the visited
file. You specify which coding system using the minibuffer. Since this
command applies to a file you have already visited, it affects only the
way the file is saved.

Another way to specify the coding system for a file is when you visit
the file. First use the command `C-x RET c`
(`universal-coding-system-argument`); this command uses the
minibuffer to read a coding system name. After you exit the minibuffer,
the specified coding system is used for *the immediately following
command*.

So if the immediately following command is `C-x C-f`, for example,
it reads the file using that coding system (and records the coding
system for when the file is saved). Or if the immediately following
command is `C-x C-w`, it writes the file using that coding system.
Other file commands affected by a specified coding system include
`C-x C-i` and `C-x C-v`, as well as the other-window variants of
`C-x C-f`.

`C-x RET c` also affects commands that start subprocesses,
including `M-x shell` (see section [AC.15 Running Shell Commands from Emacs](emacs.html#SEC442)).

However, if the immediately following command does not use the coding
system, then `C-x RET c` ultimately has no effect.

An easy way to visit a file with no conversion is with the `M-x
find-file-literally` command. See section [M.2 Visiting Files](emacs.html#SEC112).

The variable `default-buffer-file-coding-system` specifies the
choice of coding system to use when you create a new file. It applies
when you find a new file, and when you create a buffer and then save it
in a file. Selecting a language environment typically sets this
variable to a good choice of default coding system for that language
environment.

The command `C-x RET t` (`set-terminal-coding-system`)
specifies the coding system for terminal output. If you specify a
character code for terminal output, all characters output to the
terminal are translated into that coding system.

This feature is useful for certain character-only terminals built to
support specific languages or character sets--for example, European
terminals that support one of the ISO Latin character sets. You need to
specify the terminal coding system when using multibyte text, so that
Emacs knows which characters the terminal can actually handle.

By default, output to the terminal is not translated at all, unless
Emacs can deduce the proper coding system from your terminal type or
your locale specification (see section [Q.3 Language Environments](emacs.html#SEC216)).

The command `C-x RET k` (`set-keyboard-coding-system`)
or the Custom option `keyboard-coding-system`
specifies the coding system for keyboard input. Character-code
translation of keyboard input is useful for terminals with keys that
send non-ASCII graphic characters--for example, some terminals designed
for ISO Latin-1 or subsets of it.

By default, keyboard input is not translated at all.

There is a similarity between using a coding system translation for
keyboard input, and using an input method: both define sequences of
keyboard input that translate into single characters. However, input
methods are designed to be convenient for interactive use by humans, and
the sequences that are translated are typically sequences of ASCII
printing characters. Coding systems typically translate sequences of
non-graphic characters.

The command `C-x RET x` (`set-selection-coding-system`)
specifies the coding system for sending selected text to the window
system, and for receiving the text of selections made in other
applications. This command applies to all subsequent selections, until
you override it by using the command again. The command `C-x
RET X` (`set-next-selection-coding-system`) specifies the
coding system for the next selection made in Emacs or read by Emacs.

The command `C-x RET p` (`set-buffer-process-coding-system`)
specifies the coding system for input and output to a subprocess. This
command applies to the current buffer; normally, each subprocess has its
own buffer, and thus you can use this command to specify translation to
and from a particular subprocess by giving the command in the
corresponding buffer.

The default for translation of process input and output depends on the
current language environment.

The variable `file-name-coding-system` specifies a coding system
to use for encoding file names. If you set the variable to a coding
system name (as a Lisp symbol or a string), Emacs encodes file names
using that coding system for all file operations. This makes it
possible to use non-ASCII characters in file names--or, at least, those
non-ASCII characters which the specified coding system can encode.

If `file-name-coding-system` is `nil`, Emacs uses a default
coding system determined by the selected language environment. In the
default language environment, any non-ASCII characters in file names are
not encoded specially; they appear in the file system using the internal
Emacs representation.

**Warning:** if you change `file-name-coding-system` (or the
language environment) in the middle of an Emacs session, problems can
result if you have already visited files whose names were encoded using
the earlier coding system and cannot be encoded (or are encoded
differently) under the new coding system. If you try to save one of
these buffers under the visited file name, saving may use the wrong file
name, or it may get an error. If such a problem happens, use `C-x
C-w` to specify a new file name for that buffer.

The variable `locale-coding-system` specifies a coding system
to use when encoding and decoding system strings such as system error
messages and `format-time-string` formats and time stamps. That
coding system is also used for decoding non-ASCII keyboard input on X
Window systems. You should choose a coding system that is compatible
with the underlying system's text representation, which is normally
specified by one of the environment variables `LC_ALL`,
`LC_CTYPE`, and `LANG`. (The first one, in the order
specified above, whose value is nonempty is the one that determines
the text representation.)

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC222) ] | [ [>](emacs.html#SEC224) ] | [ [<<](emacs.html#SEC215) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.10 Fontsets

A font for X typically defines shapes for a single alphabet or script.
Therefore, displaying the entire range of scripts that Emacs supports
requires a collection of many fonts. In Emacs, such a collection is
called a *fontset*. A fontset is defined by a list of fonts, each
assigned to handle a range of character codes.

Each fontset has a name, like a font. The available X fonts are
defined by the X server; fontsets, however, are defined within Emacs
itself. Once you have defined a fontset, you can use it within Emacs by
specifying its name, anywhere that you could use a single font. Of
course, Emacs fontsets can use only the fonts that the X server
supports; if certain characters appear on the screen as hollow boxes,
this means that the fontset in use for them has no font for those
characters.[(7)](emacs.html#FOOT7)

Emacs creates two fontsets automatically: the *standard fontset*
and the *startup fontset*. The standard fontset is most likely to
have fonts for a wide variety of non-ASCII characters; however, this is
not the default for Emacs to use. (By default, Emacs tries to find a
font that has bold and italic variants.) You can specify use of the
standard fontset with the `` `-fn' `` option, or with the `` `Font' `` X
resource (see section [AE.7 Font Specification Options](emacs.html#SEC537)). For example,

|  |  |
| --- | --- |
|  | ``` emacs -fn fontset-standard ``` |

A fontset does not necessarily specify a font for every character
code. If a fontset specifies no font for a certain character, or if it
specifies a font that does not exist on your system, then it cannot
display that character properly. It will display that character as an
empty box instead.

The fontset height and width are determined by the ASCII characters
(that is, by the font used for ASCII characters in that fontset). If
another font in the fontset has a different height, or a different
width, then characters assigned to that font are clipped to the
fontset's size. If `highlight-wrong-size-font` is non-`nil`,
a box is displayed around these wrong-size characters as well.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC223) ] | [ [>](emacs.html#SEC225) ] | [ [<<](emacs.html#SEC215) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.11 Defining fontsets

Emacs creates a standard fontset automatically according to the value
of `standard-fontset-spec`. This fontset's name is

|  |  |
| --- | --- |
|  | ``` -*-fixed-medium-r-normal-*-16-*-*-*-*-*-fontset-standard ``` |

or just `` `fontset-standard' `` for short.

Bold, italic, and bold-italic variants of the standard fontset are
created automatically. Their names have `` `bold' `` instead of
`` `medium' ``, or `` `i' `` instead of `` `r' ``, or both.

If you specify a default ASCII font with the `` `Font' `` resource or
the `` `-fn' `` argument, Emacs generates a fontset from it
automatically. This is the *startup fontset* and its name is
`fontset-startup`. It does this by replacing the foundry,
family, add\_style, and average\_width fields of the
font name with `` `*' ``, replacing charset\_registry field with
`` `fontset' ``, and replacing charset\_encoding field with
`` `startup' ``, then using the resulting string to specify a fontset.

For instance, if you start Emacs this way,

|  |  |
| --- | --- |
|  | ``` emacs -fn "*courier-medium-r-normal--14-140-*-iso8859-1" ``` |

Emacs generates the following fontset and uses it for the initial X
window frame:

|  |  |
| --- | --- |
|  | ``` -*-*-medium-r-normal-*-14-140-*-*-*-*-fontset-startup ``` |

With the X resource `` `Emacs.Font' ``, you can specify a fontset name
just like an actual font name. But be careful not to specify a fontset
name in a wildcard resource like `` `Emacs*Font' ``---that wildcard
specification matches various other resources, such as for menus, and
menus cannot handle fontsets.

You can specify additional fontsets using X resources named
`` `Fontset-n' ``, where n is an integer starting from 0.
The resource value should have this form:

|  |  |
| --- | --- |
|  | ``` fontpattern, [charsetname:fontname]... ``` |

fontpattern should have the form of a standard X font name, except
for the last two fields. They should have the form
`` `fontset-alias' ``.

The fontset has two names, one long and one short. The long name is
fontpattern. The short name is `` `fontset-alias' ``. You
can refer to the fontset by either name.

The construct `` `charset:font' `` specifies which font to
use (in this fontset) for one particular character set. Here,
charset is the name of a character set, and font is the
font to use for that character set. You can use this construct any
number of times in defining one fontset.

For the other character sets, Emacs chooses a font based on
fontpattern. It replaces `` `fontset-alias' `` with values
that describe the character set. For the ASCII character font,
`` `fontset-alias' `` is replaced with `` `ISO8859-1' ``.

In addition, when several consecutive fields are wildcards, Emacs
collapses them into a single wildcard. This is to prevent use of
auto-scaled fonts. Fonts made by scaling larger fonts are not usable
for editing, and scaling a smaller font is not useful because it is
better to use the smaller font in its own size, which is what Emacs
does.

Thus if fontpattern is this,

|  |  |
| --- | --- |
|  | ``` -*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24 ``` |

the font specification for ASCII characters would be this:

|  |  |
| --- | --- |
|  | ``` -*-fixed-medium-r-normal-*-24-*-ISO8859-1 ``` |

and the font specification for Chinese GB2312 characters would be this:

|  |  |
| --- | --- |
|  | ``` -*-fixed-medium-r-normal-*-24-*-gb2312*-* ``` |

You may not have any Chinese font matching the above font
specification. Most X distributions include only Chinese fonts that
have `` `song ti' `` or `` `fangsong ti' `` in family field. In
such a case, `` `Fontset-n' `` can be specified as below:

|  |  |
| --- | --- |
|  | ``` Emacs.Fontset-0: -*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24,\         chinese-gb2312:-*-*-medium-r-normal-*-24-*-gb2312*-* ``` |

Then, the font specifications for all but Chinese GB2312 characters have
`` `fixed' `` in the family field, and the font specification for
Chinese GB2312 characters has a wild card `` `*' `` in the family
field.

The function that processes the fontset resource value to create the
fontset is called `create-fontset-from-fontset-spec`. You can also
call this function explicitly to create a fontset.

See section [AE.7 Font Specification Options](emacs.html#SEC537), for more information about font naming in X.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC224) ] | [ [>](emacs.html#SEC226) ] | [ [<<](emacs.html#SEC215) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.12 Undisplayable Characters

Your terminal may be unable to display some non-ASCII
characters. Most non-windowing terminals can only use a single
character set (use the variable `default-terminal-coding-system`
(see section [Q.9 Specifying a Coding System](emacs.html#SEC222)) to tell Emacs which one); characters which
can't be encoded in that coding system are displayed as `` `?' `` by
default.

Windowing terminals can display a broader range of characters, but
you may not have fonts installed for all of them; characters that have
no font appear as a hollow box.

If you use Latin-1 characters but your terminal can't display
Latin-1, you can arrange to display mnemonic ASCII sequences
instead, e.g. `` `"o' `` for o-umlaut. Load the library
`iso-ascii' to do this.

If your terminal can display Latin-1, you can display characters
from other European character sets using a mixture of equivalent
Latin-1 characters and ASCII mnemonics. Use the Custom option
`latin1-display` to enable this. The mnemonic ASCII
sequences mostly correspond to those of the prefix input methods.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC225) ] | [ [>](emacs.html#SEC227) ] | [ [<<](emacs.html#SEC215) ] | [ [Up](emacs.html#SEC213) ] | [ [>>](emacs.html#SEC227) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Q.13 Single-byte Character Set Support

The ISO 8859 Latin-n character sets define character codes in
the range 0240 to 0377 octal (160 to 255 decimal) to handle the
accented letters and punctuation needed by various European languages
(and some non-European ones). If you disable multibyte characters,
Emacs can still handle *one* of these character codes at a time.
To specify *which* of these codes to use, invoke `M-x
set-language-environment` and specify a suitable language environment
such as `` `Latin-n' ``.

For more information about unibyte operation, see [Q.2 Enabling Multibyte Characters](emacs.html#SEC215). Note particularly that you probably want to ensure that
your initialization files are read as unibyte if they contain non-ASCII
characters.

Emacs can also display those characters, provided the terminal or font
in use supports them. This works automatically. Alternatively, if you
are using a window system, Emacs can also display single-byte characters
through fontsets, in effect by displaying the equivalent multibyte
characters according to the current language environment. To request
this, set the variable `unibyte-display-via-language-environment`
to a non-`nil` value.

If your terminal does not support display of the Latin-1 character
set, Emacs can display these characters as ASCII sequences which at
least give you a clear idea of what the characters are. To do this,
load the library `iso-ascii`. Similar libraries for other
Latin-n character sets could be implemented, but we don't have
them yet.

Normally non-ISO-8859 characters (decimal codes between 128 and 159
inclusive) are displayed as octal escapes. You can change this for
non-standard "extended" versions of ISO-8859 character sets by using the
function `standard-display-8bit` in the `disp-table` library.

There are several ways you can input single-byte non-ASCII
characters:

* If your keyboard can generate character codes 128 (decimal) and up,
  representing non-ASCII characters, you can type those character codes
  directly.

  On a windowing terminal, you should not need to do anything special to
  use these keys; they should simply work. On a text-only terminal, you
  should use the command `M-x set-keyboard-coding-system` or the
  Custom option `keyboard-coding-system` to specify which coding
  system your keyboard uses (see section [Q.9 Specifying a Coding System](emacs.html#SEC222)). Enabling this
  feature will probably require you to use `ESC` to type Meta
  characters; however, on a Linux console or in `xterm`, you can
  arrange for Meta to be converted to `ESC` and still be able type
  8-bit characters present directly on the keyboard or using
  `Compose` or `AltGr` keys. See section [B.5 Kinds of User Input](emacs.html#SEC13).

  * You can use an input method for the selected language environment.
    See section [Q.4 Input Methods](emacs.html#SEC217). When you use an input method in a unibyte buffer,
    the non-ASCII character you specify with it is converted to unibyte.

    * For Latin-1 only, you can use the
      key `C-x 8` as a "compose character" prefix for entry of
      non-ASCII Latin-1 printing characters. `C-x 8` is good for
      insertion (in the minibuffer as well as other buffers), for searching,
      and in any other context where a key sequence is allowed.

      `C-x 8` works by loading the `iso-transl` library. Once that
      library is loaded, the `ALT` modifier key, if you have one, serves
      the same purpose as `C-x 8`; use `ALT` together with an accent
      character to modify the following letter. In addition, if you have keys
      for the Latin-1 "dead accent characters," they too are defined to
      compose with the following character, once `iso-transl` is loaded.
      Use `C-x 8 C-h` to list the available translations as mnemonic
      command names.

      * For Latin-1, Latin-2 and Latin-3, `M-x iso-accents-mode` enables
        a minor mode that works much like the `latin-1-prefix` input
        method, but does not depend on having the input methods installed. This
        mode is buffer-local. It can be customized for various languages with
        `M-x iso-accents-customize`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC226) ] | [ [>](emacs.html#SEC228) ] | [ [<<](emacs.html#SEC213) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC229) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# R. Major Modes

Emacs provides many alternative *major modes*, each of which
customizes Emacs for editing text of a particular sort. The major modes
are mutually exclusive, and each buffer has one major mode at any time.
The mode line normally shows the name of the current major mode, in
parentheses (see section [B.3 The Mode Line](emacs.html#SEC11)).

The least specialized major mode is called *Fundamental mode*.
This mode has no mode-specific redefinitions or variable settings, so
that each Emacs command behaves in its most general manner, and each
option is in its default state. For editing text of a specific type
that Emacs knows about, such as Lisp code or English text, you should
switch to the appropriate major mode, such as Lisp mode or Text mode.

Selecting a major mode changes the meanings of a few keys to become
more specifically adapted to the language being edited. The ones that
are changed frequently are `TAB`, `DEL`, and `C-j`. The
prefix key `C-c` normally contains mode-specific commands. In
addition, the commands which handle comments use the mode to determine
how comments are to be delimited. Many major modes redefine the
syntactical properties of characters appearing in the buffer.
See section [AD.6 The Syntax Table](emacs.html#SEC504).

The major modes fall into three major groups. The first group
contains modes for normal text, either plain or with mark-up. It
includes Text mode, HTML mode, SGML mode, TeX mode and Outline
mode. The second group contains modes for specific programming
languages. These include Lisp mode (which has several variants), C
mode, Fortran mode, and others. The remaining major modes are not
intended for use on users' files; they are used in buffers created for
specific purposes by Emacs, such as Dired mode for buffers made by
Dired (see section [AB. Dired, the Directory Editor](emacs.html#SEC396)), Mail mode for buffers made by `C-x m`
(see section [Z. Sending Mail](emacs.html#SEC363)), and Shell mode for buffers used for
communicating with an inferior shell process (see section [AC.15.2 Interactive Inferior Shell](emacs.html#SEC444)).

Most programming-language major modes specify that only blank lines
separate paragraphs. This is to make the paragraph commands useful.
(See section [T.3 Paragraphs](emacs.html#SEC236).) They also cause Auto Fill mode to use the
definition of `TAB` to indent the new lines it creates. This is
because most lines in a program are usually indented
(see section [S. Indentation](emacs.html#SEC229)).

> |  |  |  |
> | --- | --- | --- |
> | [R.1 How Major Modes are Chosen](emacs.html#SEC228) |  | How major modes are specified or chosen. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC227) ] | [ [>](emacs.html#SEC229) ] | [ [<<](emacs.html#SEC227) ] | [ [Up](emacs.html#SEC227) ] | [ [>>](emacs.html#SEC229) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## R.1 How Major Modes are Chosen

You can select a major mode explicitly for the current buffer, but
most of the time Emacs determines which mode to use based on the file
name or on special text in the file.

Explicit selection of a new major mode is done with a `M-x` command.
From the name of a major mode, add `-mode` to get the name of a
command to select that mode. Thus, you can enter Lisp mode by executing
`M-x lisp-mode`.

When you visit a file, Emacs usually chooses the right major mode based
on the file's name. For example, files whose names end in `` `.c' `` are
edited in C mode. The correspondence between file names and major modes is
controlled by the variable `auto-mode-alist`. Its value is a list in
which each element has this form,

|  |  |
| --- | --- |
|  | ``` (regexp . mode-function) ``` |

or this form,

|  |  |
| --- | --- |
|  | ``` (regexp mode-function flag) ``` |

For example, one element normally found in the list has the form
`("\\.c\\'" . c-mode)`, and it is responsible for selecting C
mode for files whose names end in `.c'. (Note that `` `\\' `` is
needed in Lisp syntax to include a `` `\' `` in the string, which must
be used to suppress the special meaning of `` `.' `` in regexps.) If the
element has the form `(regexp mode-function
flag)` and flag is non-`nil`, then after calling
mode-function, the suffix that matched regexp is discarded
and the list is searched again for another match.

You can specify which major mode should be used for editing a certain
file by a special sort of text in the first nonblank line of the file. The
mode name should appear in this line both preceded and followed by
`` `-*-' ``. Other text may appear on the line as well. For example,

|  |  |
| --- | --- |
|  | ``` ;-*-Lisp-*- ``` |

tells Emacs to use Lisp mode. Such an explicit specification overrides
any defaults based on the file name. Note how the semicolon is used
to make Lisp treat this line as a comment.

Another format of mode specification is

|  |  |
| --- | --- |
|  | ``` -*- mode: modename;-*- ``` |

which allows you to specify local variables as well, like this:

|  |  |
| --- | --- |
|  | ``` -*- mode: modename; var: value; ... -*- ``` |

See section [AD.2.5 Local Variables in Files](emacs.html#SEC486), for more information about this.

When a file's contents begin with `` `#!' ``, it can serve as an
executable shell command, which works by running an interpreter named on
the file's first line. The rest of the file is used as input to the
interpreter.

When you visit such a file in Emacs, if the file's name does not
specify a major mode, Emacs uses the interpreter name on the first line
to choose a mode. If the first line is the name of a recognized
interpreter program, such as `` `perl' `` or `` `tcl' ``, Emacs uses a
mode appropriate for programs for that interpreter. The variable
`interpreter-mode-alist` specifies the correspondence between
interpreter program names and major modes.

When the first line starts with `` `#!' ``, you cannot (on many
systems) use the `` `-*-' `` feature on the first line, because the
system would get confused when running the interpreter. So Emacs looks
for `` `-*-' `` on the second line in such files as well as on the
first line.

When you visit a file that does not specify a major mode to use, or
when you create a new buffer with `C-x b`, the variable
`default-major-mode` specifies which major mode to use. Normally
its value is the symbol `fundamental-mode`, which specifies
Fundamental mode. If `default-major-mode` is `nil`, the major
mode is taken from the previously current buffer.

If you change the major mode of a buffer, you can go back to the major
mode Emacs would choose automatically: use the command `M-x
normal-mode` to do this. This is the same function that
`find-file` calls to choose the major mode. It also processes
the file's local variables list (if any).

The commands `C-x C-w` and `set-visited-file-name` change to
a new major mode if the new file name implies a mode (see section [M.3 Saving Files](emacs.html#SEC113)).
However, this does not happen if the buffer contents specify a major
mode, and certain "special" major modes do not allow the mode to
change. You can turn off this mode-changing feature by setting
`change-major-mode-with-file-name` to `nil`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC228) ] | [ [>](emacs.html#SEC230) ] | [ [<<](emacs.html#SEC227) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC233) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# S. Indentation

This chapter describes the Emacs commands that add, remove, or
adjust indentation.

`TAB`: Indent the current line "appropriately" in a mode-dependent fashion. `C-j`: Perform `RET` followed by `TAB` (`newline-and-indent`). `M-^`: Merge the previous and the current line (`delete-indentation`). This would cancel out the effect of `C-j`. `C-M-o`: Split the current line at point; text on the line after point becomes a new line indented to the same column where point is located (`split-line`). `M-m`: Move (forward or back) to the first nonblank character on the current line (`back-to-indentation`). `C-M-\`: Indent several lines to the same column (`indent-region`). `C-x TAB`: Shift a block of lines rigidly right or left (`indent-rigidly`). `M-i`: Indent from point to the next prespecified tab stop column (`tab-to-tab-stop`). `M-x indent-relative`: Indent from point to under an indentation point in the previous line.

Most programming languages have some indentation convention. For Lisp
code, lines are indented according to their nesting in parentheses. The
same general idea is used for C code, though many details are different.

Whatever the language, to indent a line, use the `TAB` command. Each
major mode defines this command to perform the sort of indentation
appropriate for the particular language. In Lisp mode, `TAB` aligns
the line according to its depth in parentheses. No matter where in the
line you are when you type `TAB`, it aligns the line as a whole. In C
mode, `TAB` implements a subtle and sophisticated indentation style that
knows about many aspects of C syntax.

In Text mode, `TAB` runs the command `tab-to-tab-stop`, which
indents to the next tab stop column. You can set the tab stops with
`M-x edit-tab-stops`.

Normally, `TAB` inserts an optimal mix of tabs and spaces for
the intended indentation. See section [S.3 Tabs vs. Spaces](emacs.html#SEC232), for how to prevent use
of tabs.

> |  |  |  |
> | --- | --- | --- |
> | [S.1 Indentation Commands and Techniques](emacs.html#SEC230) |  | Various commands and techniques for indentation. |
> | [S.2 Tab Stops](emacs.html#SEC231) |  | You can set arbitrary "tab stops" and then indent to the next tab stop when you want to. |
> | [S.3 Tabs vs. Spaces](emacs.html#SEC232) |  | You can request indentation using just spaces. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC229) ] | [ [>](emacs.html#SEC231) ] | [ [<<](emacs.html#SEC229) ] | [ [Up](emacs.html#SEC229) ] | [ [>>](emacs.html#SEC233) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## S.1 Indentation Commands and Techniques

To move over the indentation on a line, do `M-m`
(`back-to-indentation`). This command, given anywhere on a line,
positions point at the first nonblank character on the line.

To insert an indented line before the current line, do `C-a C-o
TAB`. To make an indented line after the current line, use
`C-e C-j`.

If you just want to insert a tab character in the buffer, you can type
`C-q TAB`.

`C-M-o` (`split-line`) moves the text from point to the end of
the line vertically down, so that the current line becomes two lines.
`C-M-o` first moves point forward over any spaces and tabs. Then it
inserts after point a newline and enough indentation to reach the same
column point is on. Point remains before the inserted newline; in this
regard, `C-M-o` resembles `C-o`.

To join two lines cleanly, use the `M-^`
(`delete-indentation`) command. It deletes the indentation at the
front of the current line, and the line boundary as well, replacing them
with a single space. As a special case (useful for Lisp code) the
single space is omitted if the characters to be joined are consecutive
open parentheses or closing parentheses, or if the junction follows
another newline. To delete just the indentation of a line, go to the
beginning of the line and use `M-\`
(`delete-horizontal-space`), which deletes all spaces and tabs
around the cursor.

If you have a fill prefix, `M-^` deletes the fill prefix if it
appears after the newline that is deleted. See section [T.5.4 The Fill Prefix](emacs.html#SEC242).

There are also commands for changing the indentation of several lines
at once. `C-M-\` (`indent-region`) applies to all the lines
that begin in the region; it indents each line in the "usual" way, as
if you had typed `TAB` at the beginning of the line. A numeric
argument specifies the column to indent to, and each line is shifted
left or right so that its first nonblank character appears in that
column. `C-x TAB` (`indent-rigidly`) moves all of the
lines in the region right by its argument (left, for negative
arguments). The whole group of lines moves rigidly sideways, which is
how the command gets its name.

`M-x indent-relative` indents at point based on the previous line
(actually, the last nonempty line). It inserts whitespace at point, moving
point, until it is underneath an indentation point in the previous line.
An indentation point is the end of a sequence of whitespace or the end of
the line. If point is farther right than any indentation point in the
previous line, the whitespace before point is deleted and the first
indentation point then applicable is used. If no indentation point is
applicable even then, `indent-relative` runs `tab-to-tab-stop`
(see section [S.2 Tab Stops](emacs.html#SEC231)),
unless it is called with a numeric argument, in which case it does
nothing.

`indent-relative` is the definition of `TAB` in Indented Text
mode. See section [T. Commands for Human Languages](emacs.html#SEC233).

See section [T.11.6 Indentation in Formatted Text](emacs.html#SEC264), for another way of specifying the
indentation for part of your text.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC230) ] | [ [>](emacs.html#SEC232) ] | [ [<<](emacs.html#SEC229) ] | [ [Up](emacs.html#SEC229) ] | [ [>>](emacs.html#SEC233) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## S.2 Tab Stops

For typing in tables, you can use Text mode's definition of `TAB`,
`tab-to-tab-stop`. This command inserts indentation before point,
enough to reach the next tab stop column. If you are not in Text mode,
this command can be found on the key `M-i`.

You can specify the tab stops used by `M-i`. They are stored in a
variable called `tab-stop-list`, as a list of column-numbers in
increasing order.

The convenient way to set the tab stops is with `M-x
edit-tab-stops`, which creates and selects a buffer containing a
description of the tab stop settings. You can edit this buffer to
specify different tab stops, and then type `C-c C-c` to make those
new tab stops take effect. `edit-tab-stops` records which buffer
was current when you invoked it, and stores the tab stops back in that
buffer; normally all buffers share the same tab stops and changing them
in one buffer affects all, but if you happen to make
`tab-stop-list` local in one buffer then `edit-tab-stops` in
that buffer will edit the local settings.

Here is what the text representing the tab stops looks like for ordinary
tab stops every eight columns.

|  |  |
| --- | --- |
|  | ```         :       :       :       :       :       : 0         1         2         3         4 0123456789012345678901234567890123456789012345678 To install changes, type C-c C-c ``` |

The first line contains a colon at each tab stop. The remaining lines
are present just to help you see where the colons are and know what to do.

Note that the tab stops that control `tab-to-tab-stop` have nothing
to do with displaying tab characters in the buffer. See section [J.12 Customization of Display](emacs.html#SEC89),
for more information on that.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC231) ] | [ [>](emacs.html#SEC233) ] | [ [<<](emacs.html#SEC229) ] | [ [Up](emacs.html#SEC229) ] | [ [>>](emacs.html#SEC233) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## S.3 Tabs vs. Spaces

Emacs normally uses both tabs and spaces to indent lines. If you
prefer, all indentation can be made from spaces only. To request
this, set `indent-tabs-mode` to `nil`. This is a per-buffer
variable, so altering the variable affects only the current buffer,
but there is a default value which you can change as well.
See section [AD.2.4 Local Variables](emacs.html#SEC485).

There are also commands to convert tabs to spaces or vice versa, always
preserving the columns of all nonblank text. `M-x tabify` scans the
region for sequences of spaces, and converts sequences of at least three
spaces to tabs if that can be done without changing indentation. `M-x
untabify` changes all tabs in the region to appropriate numbers of spaces.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC232) ] | [ [>](emacs.html#SEC234) ] | [ [<<](emacs.html#SEC229) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# T. Commands for Human Languages

The term *text* has two widespread meanings in our area of the
computer field. One is data that is a sequence of characters. Any file
that you edit with Emacs is text, in this sense of the word. The other
meaning is more restrictive: a sequence of characters in a human language
for humans to read (possibly after processing by a text formatter), as
opposed to a program or commands for a program.

Human languages have syntactic/stylistic conventions that can be
supported or used to advantage by editor commands: conventions involving
words, sentences, paragraphs, and capital letters. This chapter
describes Emacs commands for all of these things. There are also
commands for *filling*, which means rearranging the lines of a
paragraph to be approximately equal in length. The commands for moving
over and killing words, sentences and paragraphs, while intended
primarily for editing text, are also often useful for editing programs.

Emacs has several major modes for editing human-language text. If the
file contains text pure and simple, use Text mode, which customizes
Emacs in small ways for the syntactic conventions of text. Outline mode
provides special commands for operating on text with an outline
structure.

For text which contains embedded commands for text formatters, Emacs
has other major modes, each for a particular text formatter. Thus, for
input to TeX, you would use TeX
mode.
For input to nroff, use Nroff mode.

Instead of using a text formatter, you can edit formatted text in
WYSIWYG style ("what you see is what you get"), with Enriched mode.
Then the formatting appears on the screen in Emacs while you edit.

The "automatic typing" features may be useful when writing text.
See section `Autotyping' in Features for Automatic Typing.

> |  |  |  |
> | --- | --- | --- |
> | [T.1 Words](emacs.html#SEC234) |  | Moving over and killing words. |
> | [T.2 Sentences](emacs.html#SEC235) |  | Moving over and killing sentences. |
> | [T.3 Paragraphs](emacs.html#SEC236) |  | Moving over paragraphs. |
> | [T.4 Pages](emacs.html#SEC237) |  | Moving over pages. |
> | [T.5 Filling Text](emacs.html#SEC238) |  | Filling or justifying text. |
> | [T.6 Case Conversion Commands](emacs.html#SEC244) |  | Changing the case of text. |
> | [T.7 Text Mode](emacs.html#SEC245) |  | The major modes for editing text files. |
> | [T.8 Outline Mode](emacs.html#SEC246) |  | Editing outlines. |
> | [T.9 TeX Mode](emacs.html#SEC252) |  | Editing input to the formatter TeX. |
> | [T.10 Nroff Mode](emacs.html#SEC257) |  | Editing input to the formatter nroff. |
> | [T.11 Editing Formatted Text](emacs.html#SEC258) |  | Editing formatted text directly in WYSIWYG fashion. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC233) ] | [ [>](emacs.html#SEC235) ] | [ [<<](emacs.html#SEC233) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.1 Words

Emacs has commands for moving over or operating on words. By convention,
the keys for them are all Meta characters.

`M-f`: Move forward over a word (`forward-word`). `M-b`: Move backward over a word (`backward-word`). `M-d`: Kill up to the end of a word (`kill-word`). `M-DEL`: Kill back to the beginning of a word (`backward-kill-word`). `M-@`: Mark the end of the next word (`mark-word`). `M-t`: Transpose two words or drag a word across other words (`transpose-words`).

Notice how these keys form a series that parallels the character-based
`C-f`, `C-b`, `C-d`, `DEL` and `C-t`. `M-@` is
cognate to `C-@`, which is an alias for `C-SPC`.

The commands `M-f` (`forward-word`) and `M-b`
(`backward-word`) move forward and backward over words. These
Meta characters are thus analogous to the corresponding control
characters, `C-f` and `C-b`, which move over single characters
in the text. The analogy extends to numeric arguments, which serve as
repeat counts. `M-f` with a negative argument moves backward, and
`M-b` with a negative argument moves forward. Forward motion
stops right after the last letter of the word, while backward motion
stops right before the first letter.

`M-d` (`kill-word`) kills the word after point. To be
precise, it kills everything from point to the place `M-f` would
move to. Thus, if point is in the middle of a word, `M-d` kills
just the part after point. If some punctuation comes between point and the
next word, it is killed along with the word. (If you wish to kill only the
next word but not the punctuation before it, simply do `M-f` to get
the end, and kill the word backwards with `M-DEL`.)
`M-d` takes arguments just like `M-f`.

`M-DEL` (`backward-kill-word`) kills the word before
point. It kills everything from point back to where `M-b` would
move to. If point is after the space in `` `FOO, BAR' ``, then
`` `FOO, ' `` is killed. (If you wish to kill just `` `FOO' ``, and
not the comma and the space, use `M-b M-d` instead of
`M-DEL`.)

`M-t` (`transpose-words`) exchanges the word before or
containing point with the following word. The delimiter characters between
the words do not move. For example, `` `FOO, BAR' `` transposes into
`` `BAR, FOO' `` rather than `` `BAR FOO,' ``. See section [L.2 Transposing Text](emacs.html#SEC107), for
more on transposition and on arguments to transposition commands.

To operate on the next n words with an operation which applies
between point and mark, you can either set the mark at point and then move
over the words, or you can use the command `M-@` (`mark-word`)
which does not move point, but sets the mark where `M-f` would move
to. `M-@` accepts a numeric argument that says how many words to
scan for the place to put the mark. In Transient Mark mode, this command
activates the mark.

The word commands' understanding of syntax is completely controlled by
the syntax table. Any character can, for example, be declared to be a word
delimiter. See section [AD.6 The Syntax Table](emacs.html#SEC504).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC234) ] | [ [>](emacs.html#SEC236) ] | [ [<<](emacs.html#SEC236) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.2 Sentences

The Emacs commands for manipulating sentences and paragraphs are mostly
on Meta keys, so as to be like the word-handling commands.

`M-a`: Move back to the beginning of the sentence (`backward-sentence`). `M-e`: Move forward to the end of the sentence (`forward-sentence`). `M-k`: Kill forward to the end of the sentence (`kill-sentence`). `C-x DEL`: Kill back to the beginning of the sentence (`backward-kill-sentence`).

The commands `M-a` and `M-e` (`backward-sentence` and
`forward-sentence`) move to the beginning and end of the current
sentence, respectively. They were chosen to resemble `C-a` and
`C-e`, which move to the beginning and end of a line. Unlike them,
`M-a` and `M-e` if repeated or given numeric arguments move over
successive sentences.

Moving backward over a sentence places point just before the first
character of the sentence; moving forward places point right after the
punctuation that ends the sentence. Neither one moves over the
whitespace at the sentence boundary.

Just as `C-a` and `C-e` have a kill command, `C-k`, to go
with them, so `M-a` and `M-e` have a corresponding kill command
`M-k` (`kill-sentence`) which kills from point to the end of
the sentence. With minus one as an argument it kills back to the
beginning of the sentence. Larger arguments serve as a repeat count.
There is also a command, `C-x DEL`
(`backward-kill-sentence`), for killing back to the beginning of a
sentence. This command is useful when you change your mind in the
middle of composing text.

The sentence commands assume that you follow the American typist's
convention of putting two spaces at the end of a sentence; they consider
a sentence to end wherever there is a `` `.' ``, `` `?' `` or `` `!' ``
followed by the end of a line or two spaces, with any number of
`` `)' ``, `` `]' ``, `` `'' ``, or `` `"' `` characters allowed in between.
A sentence also begins or ends wherever a paragraph begins or ends.

The variable `sentence-end` controls recognition of the end of a
sentence. It is a regexp that matches the last few characters of a
sentence, together with the whitespace following the sentence. Its
normal value is

|  |  |
| --- | --- |
|  | ``` "[.?!][]\"')]*\\($\\| $\\|\t\\|  \\)[ \t\n]*" ``` |

This example is explained in the section on regexps. See section [K.5 Syntax of Regular Expressions](emacs.html#SEC97).

If you want to use just one space between sentences, you should
set `sentence-end` to this value:

|  |  |
| --- | --- |
|  | ``` "[.?!][]\"')]*\\($\\|\t\\| \\)[ \t\n]*" ``` |

You should also set the variable `sentence-end-double-space` to
`nil` so that the fill commands expect and leave just one space at
the end of a sentence. Note that this makes it impossible to
distinguish between periods that end sentences and those that indicate
abbreviations.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC235) ] | [ [>](emacs.html#SEC237) ] | [ [<<](emacs.html#SEC237) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.3 Paragraphs

The Emacs commands for manipulating paragraphs are also Meta keys.

`M-{`: Move back to previous paragraph beginning (`backward-paragraph`). `M-}`: Move forward to next paragraph end (`forward-paragraph`). `M-h`: Put point and mark around this or next paragraph (`mark-paragraph`).

`M-{` moves to the beginning of the current or previous
paragraph, while `M-}` moves to the end of the current or next
paragraph. Blank lines and text-formatter command lines separate
paragraphs and are not considered part of any paragraph. In Fundamental
mode, but not in Text mode, an indented line also starts a new
paragraph. (If a paragraph is preceded by a blank line, these commands
treat that blank line as the beginning of the paragraph.)

In major modes for programs, paragraphs begin and end only at blank
lines. This makes the paragraph commands continue to be useful even
though there are no paragraphs per se.

When there is a fill prefix, then paragraphs are delimited by all lines
which don't start with the fill prefix. See section [T.5 Filling Text](emacs.html#SEC238).

When you wish to operate on a paragraph, you can use the command
`M-h` (`mark-paragraph`) to set the region around it. Thus,
for example, `M-h C-w` kills the paragraph around or after point.
The `M-h` command puts point at the beginning and mark at the end of
the paragraph point was in. In Transient Mark mode, it activates the
mark. If point is between paragraphs (in a run of blank lines, or at a
boundary), the paragraph following point is surrounded by point and
mark. If there are blank lines preceding the first line of the
paragraph, one of these blank lines is included in the region.

The precise definition of a paragraph boundary is controlled by the
variables `paragraph-separate` and `paragraph-start`. The
value of `paragraph-start` is a regexp that should match any line
that either starts or separates paragraphs. The value of
`paragraph-separate` is another regexp that should match only lines
that separate paragraphs without being part of any paragraph (for
example, blank lines). Lines that start a new paragraph and are
contained in it must match only `paragraph-start`, not
`paragraph-separate`. For example, in Fundamental mode,
`paragraph-start` is `"[ \t\n\f]"`, and
`paragraph-separate` is `"[ \t\f]*$"`.

Normally it is desirable for page boundaries to separate paragraphs.
The default values of these variables recognize the usual separator for
pages.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC236) ] | [ [>](emacs.html#SEC238) ] | [ [<<](emacs.html#SEC238) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.4 Pages

Files are often thought of as divided into *pages* by the
*formfeed* character (ASCII control-L, octal code 014). When you
print hardcopy for a file, this character forces a page break; thus,
each page of the file goes on a separate page on paper. Most Emacs
commands treat the page-separator character just like any other
character: you can insert it with `C-q C-l`, and delete it with
`DEL`. Thus, you are free to paginate your file or not. However,
since pages are often meaningful divisions of the file, Emacs provides
commands to move over them and operate on them.

`C-x [`: Move point to previous page boundary (`backward-page`). `C-x ]`: Move point to next page boundary (`forward-page`). `C-x C-p`: Put point and mark around this page (or another page) (`mark-page`). `C-x l`: Count the lines in this page (`count-lines-page`).

The `C-x [` (`backward-page`) command moves point to immediately
after the previous page delimiter. If point is already right after a page
delimiter, it skips that one and stops at the previous one. A numeric
argument serves as a repeat count. The `C-x ]` (`forward-page`)
command moves forward past the next page delimiter.

The `C-x C-p` command (`mark-page`) puts point at the
beginning of the current page and the mark at the end. The page
delimiter at the end is included (the mark follows it). The page
delimiter at the front is excluded (point follows it). In Transient
Mark mode, this command activates the mark.

`C-x C-p C-w` is a handy way to kill a page to move it
elsewhere. If you move to another page delimiter with `C-x [` and
`C-x ]`, then yank the killed page, all the pages will be properly
delimited once again. The reason `C-x C-p` includes only the
following page delimiter in the region is to ensure that.

A numeric argument to `C-x C-p` is used to specify which page to go
to, relative to the current one. Zero means the current page. One means
the next page, and -1 means the previous one.

The `C-x l` command (`count-lines-page`) is good for deciding
where to break a page in two. It displays in the echo area the total number
of lines in the current page, and then divides it up into those preceding
the current line and those following, as in

|  |  |
| --- | --- |
|  | ``` Page has 96 (72+25) lines ``` |

Notice that the sum is off by one; this is correct if point is not at the
beginning of a line.

The variable `page-delimiter` controls where pages begin. Its
value is a regexp that matches the beginning of a line that separates
pages. The normal value of this variable is `"^\f"`, which
matches a formfeed character at the beginning of a line.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC237) ] | [ [>](emacs.html#SEC239) ] | [ [<<](emacs.html#SEC244) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC244) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.5 Filling Text

*Filling* text means breaking it up into lines that fit a
specified width. Emacs does filling in two ways. In Auto Fill mode,
inserting text with self-inserting characters also automatically fills
it. There are also explicit fill commands that you can use when editing
text leaves it unfilled. When you edit formatted text, you can specify
a style of filling for each portion of the text (see section [T.11 Editing Formatted Text](emacs.html#SEC258)).
