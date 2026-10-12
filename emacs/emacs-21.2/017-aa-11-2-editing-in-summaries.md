### AA.11.2 Editing in Summaries

You can use the Rmail summary buffer to do almost anything you can do
in the Rmail buffer itself. In fact, once you have a summary buffer,
there's no need to switch back to the Rmail buffer.

You can select and display various messages in the Rmail buffer, from
the summary buffer, just by moving point in the summary buffer to
different lines. It doesn't matter what Emacs command you use to move
point; whichever line point is on at the end of the command, that
message is selected in the Rmail buffer.

Almost all Rmail commands work in the summary buffer as well as in the
Rmail buffer. Thus, `d` in the summary buffer deletes the current
message, `u` undeletes, and `x` expunges. `o` and `C-o`
output the current message to a file; `r` starts a reply to it. You
can scroll the current message while remaining in the summary buffer
using `SPC` and `DEL`.

The Rmail commands to move between messages also work in the summary
buffer, but with a twist: they move through the set of messages included
in the summary. They also ensure the Rmail buffer appears on the screen
(unlike cursor motion commands, which update the contents of the Rmail
buffer but don't display it in a window unless it already appears).
Here is a list of these commands:

`n`: Move to next line, skipping lines saying `deleted', and select its message. `p`: Move to previous line, skipping lines saying `deleted', and select its message. `M-n`: Move to next line and select its message. `M-p`: Move to previous line and select its message. `>`: Move to the last line, and select its message. `<`: Move to the first line, and select its message. `M-s pattern RET`: Search through messages for pattern starting with the current message; select the message found, and move point in the summary buffer to that message's line.

Deletion, undeletion, and getting new mail, and even selection of a
different message all update the summary buffer when you do them in the
Rmail buffer. If the variable `rmail-redisplay-summary` is
non-`nil`, these actions also bring the summary buffer back onto
the screen.

When you are finished using the summary, type `Q`
(`rmail-summary-wipe`) to delete the summary buffer's window. You
can also exit Rmail while in the summary: `q`
(`rmail-summary-quit`) deletes the summary window, then exits from
Rmail by saving the Rmail file and switching to another buffer.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC387) ] | [ [>](emacs.html#SEC389) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.12 Sorting the Rmail File

`M-x rmail-sort-by-date`: Sort messages of current Rmail file by date. `M-x rmail-sort-by-subject`: Sort messages of current Rmail file by subject. `M-x rmail-sort-by-author`: Sort messages of current Rmail file by author's name. `M-x rmail-sort-by-recipient`: Sort messages of current Rmail file by recipient's names. `M-x rmail-sort-by-correspondent`: Sort messages of current Rmail file by the name of the other correspondent. `M-x rmail-sort-by-lines`: Sort messages of current Rmail file by size (number of lines). `M-x rmail-sort-by-keywords RET labels RET`: Sort messages of current Rmail file by labels. The argument labels should be a comma-separated list of labels. The order of these labels specifies the order of messages; messages with the first label come first, messages with the second label come second, and so on. Messages which have none of these labels come last.

The Rmail sort commands perform a *stable sort*: if there is no
reason to prefer either one of two messages, their order remains
unchanged. You can use this to sort by more than one criterion. For
example, if you use `rmail-sort-by-date` and then
`rmail-sort-by-author`, messages from the same author appear in
order by date.

With a numeric argument, all these commands reverse the order of
comparison. This means they sort messages from newest to oldest, from
biggest to smallest, or in reverse alphabetical order.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC388) ] | [ [>](emacs.html#SEC390) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.13 Display of Messages

Rmail reformats the header of each message before displaying it for
the first time. Reformatting hides uninteresting header fields to
reduce clutter. You can use the `t` command to show the entire
header or to repeat the header reformatting operation.

`t`: Toggle display of complete header (`rmail-toggle-header`).

Reformatting the header involves deleting most header fields, on the
grounds that they are not interesting. The variable
`rmail-ignored-headers` holds a regular expression that specifies
which header fields to hide in this way--if it matches the beginning of
a header field, that whole field is hidden.

Rmail saves the complete original header before reformatting; to see
it, use the `t` command (`rmail-toggle-header`). This
discards the reformatted headers of the current message and displays it
with the original header. Repeating `t` reformats the message
again. Selecting the message again also reformats.

One consequence of this is that if you edit the reformatted header
(using `e`; see section [AA.15 Editing Within a Message](emacs.html#SEC391)), subsequent use of `t` will
discard your edits. On the other hand, if you use `e` after
`t`, to edit the original (unreformatted) header, those changes are
permanent.

When the `t` command has a prefix argument, a positive argument
means to show the reformatted header, and a zero or negative argument
means to show the full header.

When the terminal supports multiple fonts or colors, Rmail
highlights certain header fields that are especially interesting--by
default, the `` `From' `` and `` `Subject' `` fields. The variable
`rmail-highlighted-headers` holds a regular expression that
specifies the header fields to highlight; if it matches the beginning
of a header field, that whole field is highlighted.

If you specify unusual colors for your text foreground and
background, the colors used for highlighting may not go well with
them. If so, specify different colors for the face
`rmail-highlight-face`. See section [J.1 Using Multiple Typefaces](emacs.html#SEC78), for how to do this. To
turn off highlighting entirely in Rmail, set
`rmail-highlighted-headers` to `nil`.

You can highlight and activate URLs in incoming messages by adding
the function `goto-address` to the hook
`rmail-show-message-hook`. Then you can browse these URLs by
clicking on them with `Mouse-2` or by moving to one and typing
`C-c RET`. See section [Activating URLs](emacs.html#SEC470).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC389) ] | [ [>](emacs.html#SEC391) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.14 Rmail and Coding Systems

Rmail automatically decodes messages which contain non-ASCII
characters, just as Emacs does with files you visit and with subprocess
output. Rmail uses the standard `` `charset=charset' `` header in
the message, if any, to determine how the message was encoded by the
sender. It maps charset into the corresponding Emacs coding
system (see section [Q.7 Coding Systems](emacs.html#SEC220)), and uses that coding system to decode
message text. If the message header doesn't have the `` `charset' ``
specification, or if charset is not recognized,
Rmail chooses the coding system with the usual Emacs heuristics and
defaults (see section [Q.8 Recognizing Coding Systems](emacs.html#SEC221)).

Occasionally, a message is decoded incorrectly, either because Emacs
guessed the wrong coding system in the absence of the `` `charset' ``
specification, or because the specification was inaccurate. For
example, a misconfigured mailer could send a message with a
`` `charset=iso-8859-1' `` header when the message is actually encoded
in `koi8-r`. When you see the message text garbled, or some of
its characters displayed as empty boxes, this may have happened.

You can correct the problem by decoding the message again using the
right coding system, if you can figure out or guess which one is
right. To do this, invoke the `M-x rmail-redecode-body` command.
It reads the name of a coding system, encodes the message body using
whichever coding system was used to decode it before, then redecodes
it using the coding system you specified. If you specified the right
coding system, the result should be readable.

Decoding and encoding using the wrong coding system is lossless for
most encodings, in particular with 8-bit encodings such as iso-8859 or
koi8. So, if the initial attempt to redecode the message didn't
result in a legible text, you can try other coding systems until you
succeed.

With some coding systems, notably those from the iso-2022 family,
information can be lost in decoding, so that encoding the message
again won't bring back the original incoming text. In such a case,
`rmail-redecode-body` cannot work. However, the problems that
call for use of `rmail-redecode-body` rarely occur with those
coding systems. So in practice the command works when you need it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC390) ] | [ [>](emacs.html#SEC392) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.15 Editing Within a Message

Most of the usual Emacs commands are available in Rmail mode, though a
few, such as `C-M-n` and `C-M-h`, are redefined by Rmail for
other purposes. However, the Rmail buffer is normally read only, and
most of the letters are redefined as Rmail commands. If you want to
edit the text of a message, you must use the Rmail command `e`.

`e`: Edit the current message as ordinary text.

The `e` command (`rmail-edit-current-message`) switches from
Rmail mode into Rmail Edit mode, another major mode which is nearly the
same as Text mode. The mode line indicates this change.

In Rmail Edit mode, letters insert themselves as usual and the Rmail
commands are not available. When you are finished editing the message and
are ready to go back to Rmail, type `C-c C-c`, which switches back to
Rmail mode. Alternatively, you can return to Rmail mode but cancel all the
editing that you have done, by typing `C-c C-]`.

Entering Rmail Edit mode runs the hook `text-mode-hook`; then it
runs the hook `rmail-edit-mode-hook` (see section [AD.2.3 Hooks](emacs.html#SEC484)). It adds the
attribute `` `edited' `` to the message. It also displays the full
headers of the message, so that you can edit the headers as well as the
body of the message, and your changes in the headers will be
permanent.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC391) ] | [ [>](emacs.html#SEC393) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.16 Digest Messages

A *digest message* is a message which exists to contain and carry
several other messages. Digests are used on some moderated mailing
lists; all the messages that arrive for the list during a period of time
such as one day are put inside a single digest which is then sent to the
subscribers. Transmitting the single digest uses much less computer
time than transmitting the individual messages even though the total
size is the same, because the per-message overhead in network mail
transmission is considerable.

When you receive a digest message, the most convenient way to read it is
to *undigestify* it: to turn it back into many individual messages.
Then you can read and delete the individual messages as it suits you.

To do this, select the digest message and type the command `M-x
undigestify-rmail-message`. This extracts the submessages as separate
Rmail messages, and inserts them following the digest. The digest
message itself is flagged as deleted.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC392) ] | [ [>](emacs.html#SEC394) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.17 Converting an Rmail File to Inbox Format

The command `M-x unrmail` converts a file in Rmail format to inbox
format (also known as the system mailbox, or mbox, format), so that
you can use it with other mail-editing tools. You must specify two
arguments, the name of the Rmail file and the name to use for the
converted file. `M-x unrmail` does not alter the Rmail file itself.

`M-x unrmail` is useful if you can run Emacs on the machine
where the Rmail file resides, or can access the Rmail file remotely
(see section [M.13 Remote Files](emacs.html#SEC168)) from a machine where Emacs is installed. If
accessing Rmail files from Emacs is impossible, you can use the
`b2m` program instead. `b2m` is part of the Emacs
distribution, it is installed into the same directory where all the
other auxiliary programs (`etags` etc.) are installed, and its
source is available in the Emacs source distribution, so that you
could copy the source to the target machine and compile it there.

To convert a file `babyl-file' into `mbox-file',
invoke `b2m` like this:

|  |  |
| --- | --- |
|  | ```  b2m < babyl-file > mbox-file ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC393) ] | [ [>](emacs.html#SEC395) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.18 Reading Rot13 Messages

Mailing list messages that might offend some readers are sometimes
encoded in a simple code called *rot13*---so named because it
rotates the alphabet by 13 letters. This code is not for secrecy, as it
provides none; rather, it enables those who might be offended to avoid
ever seeing the real text of the message.

To view a buffer using the rot13 code, use the command `M-x
rot13-other-window`. This displays the current buffer in another window
which applies the code when displaying the text.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC394) ] | [ [>](emacs.html#SEC396) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.19 `movemail` and POP

When getting new mail, Rmail first copies the new mail from the inbox
file to the Rmail file; then it saves the Rmail file; then it truncates
the inbox file. This way, a system crash may cause duplication of mail
between the inbox and the Rmail file, but cannot lose mail. If
`rmail-preserve-inbox` is non-`nil`, then Rmail will copy new
mail from the inbox file to the Rmail file without truncating the inbox
file. You may wish to set this, for example, on a portable computer you
use to check your mail via POP while traveling, so that your mail will
remain on the server and you can save it later on your workstation.

In some cases, Rmail copies the new mail from the inbox file
indirectly. First it runs the `movemail` program to move the mail
from the inbox to an intermediate file called
`~/.newmail-inboxname'. Then Rmail merges the new mail from
that file, saves the Rmail file, and only then deletes the intermediate
file. If there is a crash at the wrong time, this file continues to
exist, and Rmail will use it again the next time it gets new mail from
that inbox.

If Rmail is unable to convert the data in
`~/.newmail-inboxname' into Babyl format, it renames the file
to `~/RMAILOSE.n' (n is an integer chosen to make the
name unique) so that Rmail will not have trouble with the data again.
You should look at the file, find whatever message confuses Rmail
(probably one that includes the control-underscore character, octal code
037), and delete it. Then you can use `1 g` to get new mail from
the corrected file.

Some sites use a method called POP for accessing users' inbox data
instead of storing the data in inbox files. `movemail` can work
with POP if you compile it with the macro `MAIL_USE_POP` defined.
(You can achieve that by specifying `` `--with-pop' `` when you run
`configure` during the installation of Emacs.)
`movemail` only works with POP3, not with older
versions of POP.

Assuming you have compiled and installed `movemail`
appropriately, you can specify a POP inbox by using a "file name" of
the form `` `po:username' ``, in the inbox list of an Rmail file.
`movemail` handles such a name by opening a connection to the POP
server. The `MAILHOST` environment variable specifies the machine
to look for the server on; alternatively, you can specify the POP server
host name as part of the mailbox name using the syntax
`` `po:username:hostname' ``.

Accessing mail via POP may require a password. If the variable
`rmail-pop-password` is non-`nil`, it specifies the password
to use for POP. Alternatively, if `rmail-pop-password-required` is
non-`nil`, then Rmail asks you for the password to use.

If you need to pass additional command-line flags to `movemail`,
set the variable `rmail-movemail-flags` a list of the flags you
wish to use. Do not use this variable to pass the `` `-p' `` flag to
preserve your inbox contents; use `rmail-preserve-inbox` instead.

The `movemail` program installed at your site may support
Kerberos authentication. If it is
supported, it is used by default whenever you attempt to retrieve
POP mail when `rmail-pop-password` and
`rmail-pop-password-required` are unset.

Some POP servers store messages in reverse order. If your server does
this, and you would rather read your mail in the order in which it was
received, you can tell `movemail` to reverse the order of
downloaded messages by adding the `` `-r' `` flag to
`rmail-movemail-flags`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC395) ] | [ [>](emacs.html#SEC397) ] | [ [<<](emacs.html#SEC374) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AB. Dired, the Directory Editor

Dired makes an Emacs buffer containing a listing of a directory, and
optionally some of its subdirectories as well. You can use the normal
Emacs commands to move around in this buffer, and special Dired commands
to operate on the files listed.

The Dired buffer is "read-only," and inserting text in it is not
useful, so ordinary printing characters such as `d` and `x` are
used for special Dired commands. Some Dired commands *mark* or
*flag* the *current file* (that is, the file on the current
line); other commands operate on the marked files or on the flagged
files.

The Dired-X package provides various extra features for Dired mode.
See section `Top' in Dired Extra Version 2 User's Manual.

> |  |  |  |
> | --- | --- | --- |
> | [AB.1 Entering Dired](emacs.html#SEC397) |  | How to invoke Dired. |
> | [AB.2 Navigation in the Dired Buffer](emacs.html#SEC398) |  | Special motion commands in the Dired buffer. |
> | [AB.3 Deleting Files with Dired](emacs.html#SEC399) |  | Deleting files with Dired. |
> | [AB.4 Flagging Many Files at Once](emacs.html#SEC400) |  | Flagging files based on their names. |
> | [AB.5 Visiting Files in Dired](emacs.html#SEC401) |  | Other file operations through Dired. |
> | [AB.6 Dired Marks vs. Flags](emacs.html#SEC402) |  | Flagging for deletion vs marking. |
> | [AB.7 Operating on Files](emacs.html#SEC403) |  | How to copy, rename, print, compress, etc. either one file or several files. |
> | [AB.8 Shell Commands in Dired](emacs.html#SEC404) |  | Running a shell command on the marked files. |
> | [AB.9 Transforming File Names in Dired](emacs.html#SEC405) |  | Using patterns to rename multiple files. |
> | [AB.10 File Comparison with Dired](emacs.html#SEC406) |  | Running `diff' by way of Dired. |
> | [AB.11 Subdirectories in Dired](emacs.html#SEC407) |  | Adding subdirectories to the Dired buffer. |
> | [AB.12 Moving Over Subdirectories](emacs.html#SEC408) |  | Moving across subdirectories, and up and down. |
> | [AB.13 Hiding Subdirectories](emacs.html#SEC409) |  | Making subdirectories visible or invisible. |
> | [AB.14 Updating the Dired Buffer](emacs.html#SEC410) |  | Discarding lines for files of no interest. |
> | [AB.15 Dired and `find`](emacs.html#SEC411) |  | Using `find' to choose the files for Dired. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC396) ] | [ [>](emacs.html#SEC398) ] | [ [<<](emacs.html#SEC396) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.1 Entering Dired

To invoke Dired, do `C-x d` or `M-x dired`. The command reads
a directory name or wildcard file name pattern as a minibuffer argument
to specify which files to list. Where `dired` differs from
`list-directory` is in putting the buffer into Dired mode so that
the special commands of Dired are available.

The variable `dired-listing-switches` specifies the options to
give to `ls` for listing directory; this string *must* contain
`` `-l' ``. If you use a numeric prefix argument with the `dired`
command, you can specify the `ls` switches with the minibuffer
before you enter the directory specification.

To display the Dired buffer in another window rather than in the
selected window, use `C-x 4 d` (`dired-other-window`) instead
of `C-x d`. `C-x 5 d` (`dired-other-frame`) uses a
separate frame to display the Dired buffer.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC397) ] | [ [>](emacs.html#SEC399) ] | [ [<<](emacs.html#SEC399) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.2 Navigation in the Dired Buffer

All the usual Emacs cursor motion commands are available in Dired
buffers. Some special-purpose cursor motion commands are also
provided. The keys `C-n` and `C-p` are redefined to put the
cursor at the beginning of the file name on the line, rather than at the
beginning of the line.

For extra convenience, `SPC` and `n` in Dired are equivalent
to `C-n`. `p` is equivalent to `C-p`. (Moving by lines is
so common in Dired that it deserves to be easy to type.) `DEL`
(move up and unflag) is often useful simply for moving up.

Some additional navigation commands are available when the Dired
buffer includes several directories. See section [AB.12 Moving Over Subdirectories](emacs.html#SEC408).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC398) ] | [ [>](emacs.html#SEC400) ] | [ [<<](emacs.html#SEC400) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.3 Deleting Files with Dired

One of the most frequent uses of Dired is to first *flag* files for
deletion, then delete the files that were flagged.

`d`: Flag this file for deletion. `u`: Remove deletion flag on this line. `DEL`: Move point to previous line and remove the deletion flag on that line. `x`: Delete the files that are flagged for deletion.

You can flag a file for deletion by moving to the line describing the
file and typing `d` (`dired-flag-file-deletion`). The deletion flag is visible as a `` `D' `` at
the beginning of the line. This command moves point to the next line,
so that repeated `d` commands flag successive files. A numeric
argument serves as a repeat count.

The variable `dired-recursive-deletes` controls whether the
delete command will delete non-empty directories (including their
contents). The default is to delete only empty directories.

The files are flagged for deletion rather than deleted immediately to
reduce the danger of deleting a file accidentally. Until you direct
Dired to delete the flagged files, you can remove deletion flags using
the commands `u` and `DEL`. `u` (`dired-unmark`) works
just like `d`, but removes flags rather than making flags.
`DEL` (`dired-unmark-backward`) moves upward, removing flags;
it is like `u` with argument -1.

To delete the flagged files, type `x` (`dired-do-flagged-delete`).
(This is also known as *expunging*.)
This command first displays a list of all the file names flagged for
deletion, and requests confirmation with `yes`. If you confirm,
Dired deletes the flagged files, then deletes their lines from the text
of the Dired buffer. The shortened Dired buffer remains selected.

If you answer `no` or quit with `C-g` when asked to confirm, you
return immediately to Dired, with the deletion flags still present in
the buffer, and no files actually deleted.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC399) ] | [ [>](emacs.html#SEC401) ] | [ [<<](emacs.html#SEC401) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.4 Flagging Many Files at Once

`#`: Flag all auto-save files (files whose names start and end with `` `#' ``) for deletion (see section [M.5 Auto-Saving: Protection Against Disasters](emacs.html#SEC122)). `~`: Flag all backup files (files whose names end with `` `~' ``) for deletion (see section [M.3.1 Backup Files](emacs.html#SEC114)). `&`: Flag for deletion all files with certain kinds of names, names that suggest you could easily create the files again. `. (Period)`: Flag excess numeric backup files for deletion. The oldest and newest few backup files of any one file are exempt; the middle ones are flagged. `% d regexp RET`: Flag for deletion all files whose names match the regular expression regexp.

The `#`, `~`, `&`, and `.` commands flag many files for
deletion, based on their file names. These commands are useful
precisely because they do not themselves delete any files; you can
remove the deletion flags from any flagged files that you really wish to
keep.

`&` (`dired-flag-garbage-files`) flags files whose names
match the regular expression specified by the variable
`dired-garbage-files-regexp`. By default, this matches certain
files produced by TeX, `` `.bak' `` files, and the `` `.orig' `` and
`` `.rej' `` files produced by `patch`.

`#` (`dired-flag-auto-save-files`) flags for deletion all
files whose names look like auto-save files (see section [M.5 Auto-Saving: Protection Against Disasters](emacs.html#SEC122))---that
is, files whose names begin and end with `` `#' ``.

`~` (`dired-flag-backup-files`) flags for deletion all files
whose names say they are backup files (see section [M.3.1 Backup Files](emacs.html#SEC114))---that is, files
whose names end in `` `~' ``.

`.` (period, `dired-clean-directory`) flags just some of the
backup files for deletion: all but the oldest few and newest few backups
of any one file. Normally `dired-kept-versions` (**not**
`kept-new-versions`; that applies only when saving) specifies the
number of newest versions of each file to keep, and
`kept-old-versions` specifies the number of oldest versions to
keep.

Period with a positive numeric argument, as in `C-u 3 .`,
specifies the number of newest versions to keep, overriding
`dired-kept-versions`. A negative numeric argument overrides
`kept-old-versions`, using minus the value of the argument to
specify the number of oldest versions of each file to keep.

The `% d` command flags all files whose names match a specified
regular expression (`dired-flag-files-regexp`). Only the
non-directory part of the file name is used in matching. You can use
`` `^' `` and `` `$' `` to anchor matches. You can exclude subdirectories
by hiding them (see section [AB.13 Hiding Subdirectories](emacs.html#SEC409)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC400) ] | [ [>](emacs.html#SEC402) ] | [ [<<](emacs.html#SEC402) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.5 Visiting Files in Dired

There are several Dired commands for visiting or examining the files
listed in the Dired buffer. All of them apply to the current line's
file; if that file is really a directory, these commands invoke Dired on
that subdirectory (making a separate Dired buffer).

`f`: Visit the file described on the current line, like typing `C-x C-f` and supplying that file name (`dired-find-file`). See section [M.2 Visiting Files](emacs.html#SEC112). `RET`: `e`: Equivalent to `f`. `a`: Like `f`, but replaces the contents of the Dired buffer with that of an alternate file or directory (`dired-find-alternate-file`). `o`: Like `f`, but uses another window to display the file's buffer (`dired-find-file-other-window`). The Dired buffer remains visible in the first window. This is like using `C-x 4 C-f` to visit the file. See section [O. Multiple Windows](emacs.html#SEC183). `C-o`: Visit the file described on the current line, and display the buffer in another window, but do not select that window (`dired-display-file`). `Mouse-2`: Visit the file named by the line you click on (`dired-mouse-find-file-other-window`). This uses another window to display the file, like the `o` command. `v`: View the file described on the current line, using `M-x view-file` (`dired-view-file`). Viewing a file is like visiting it, but is slanted toward moving around in the file conveniently and does not allow changing the file. See section [Miscellaneous File Operations](emacs.html#SEC165). `^`: Visit the parent directory of the current directory (`dired-up-directory`). This is more convenient than moving to the parent directory's line and typing `f` there.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC401) ] | [ [>](emacs.html#SEC403) ] | [ [<<](emacs.html#SEC403) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.6 Dired Marks vs. Flags

Instead of flagging a file with `` `D' ``, you can *mark* the
file with some other character (usually `` `*' ``). Most Dired
commands to operate on files use the files marked with `` `*' ``, the
exception being `x` which deletes the flagged files.

Here are some commands for marking with `` `*' ``, or for unmarking or
operating on marks. (See section [AB.3 Deleting Files with Dired](emacs.html#SEC399), for commands to flag and
unflag files.)

`m`: `* m`: Mark the current file with `` `*' `` (`dired-mark`). With a numeric argument n, mark the next n files starting with the current file. (If n is negative, mark the previous -n files.) `* *`: Mark all executable files with `` `*' `` (`dired-mark-executables`). With a numeric argument, unmark all those files. `* @`: Mark all symbolic links with `` `*' `` (`dired-mark-symlinks`). With a numeric argument, unmark all those files. `* /`: Mark with `` `*' `` all files which are actually directories, except for `.' and `..' (`dired-mark-directories`). With a numeric argument, unmark all those files. `* s`: Mark all the files in the current subdirectory, aside from `.' and `..' (`dired-mark-subdir-files`). `u`: `* u`: Remove any mark on this line (`dired-unmark`). `DEL`: `* DEL`: Move point to previous line and remove any mark on that line (`dired-unmark-backward`). `* !`: Remove all marks from all the files in this Dired buffer (`dired-unmark-all-marks`). `* ? markchar`: Remove all marks that use the character markchar (`dired-unmark-all-files`). The argument is a single character--do not use `RET` to terminate it. See the description of the `* c` command below, which lets you replace one mark character with another. With a numeric argument, this command queries about each marked file, asking whether to remove its mark. You can answer `y` meaning yes, `n` meaning no, or `!` to remove the marks from the remaining files without asking about them. `* C-n`: Move down to the next marked file (`dired-next-marked-file`) A file is "marked" if it has any kind of mark. `* C-p`: Move up to the previous marked file (`dired-prev-marked-file`) `* t`: Toggle all marks (`dired-do-toggle`): files marked with `` `*' `` become unmarked, and unmarked files are marked with `` `*' ``. Files marked in any other way are not affected. `* c old-markchar new-markchar`: Replace all marks that use the character old-markchar with marks that use the character new-markchar (`dired-change-marks`). This command is the primary way to create or use marks other than `` `*' `` or `` `D' ``. The arguments are single characters--do not use `RET` to terminate them. You can use almost any character as a mark character by means of this command, to distinguish various classes of files. If old-markchar is a space (`` ` ' ``), then the command operates on all unmarked files; if new-markchar is a space, then the command unmarks the files it acts on. To illustrate the power of this command, here is how to put `` `D' `` flags on all the files that have no marks, while unflagging all those that already have `` `D' `` flags: | | | | --- | --- | | | ``` * c D t * c SPC D * c t SPC ``` | This assumes that no files were already marked with `` `t' ``. `% m regexp RET`: `* % regexp RET`: Mark (with `` `*' ``) all files whose names match the regular expression regexp (`dired-mark-files-regexp`). This command is like `% d`, except that it marks files with `` `*' `` instead of flagging with `` `D' ``. See section [AB.4 Flagging Many Files at Once](emacs.html#SEC400). Only the non-directory part of the file name is used in matching. Use `` `^' `` and `` `$' `` to anchor matches. Exclude subdirectories by hiding them (see section [AB.13 Hiding Subdirectories](emacs.html#SEC409)). `% g regexp RET`: Mark (with `` `*' ``) all files whose *contents* contain a match for the regular expression regexp (`dired-mark-files-containing-regexp`). This command is like `% m`, except that it searches the file contents instead of the file name. `C-_`: Undo changes in the Dired buffer, such as adding or removing marks (`dired-undo`). *This command does not revert the actual file operations, nor recover lost files!* It just undoes changes in the buffer itself. For example, if used after renaming one or more files, `dired-undo` restores the original names, which will get the Dired buffer out of sync with the actual contents of the directory.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC402) ] | [ [>](emacs.html#SEC404) ] | [ [<<](emacs.html#SEC404) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.7 Operating on Files

This section describes the basic Dired commands to operate on one file
or several files. All of these commands are capital letters; all of
them use the minibuffer, either to read an argument or to ask for
confirmation, before they act. All of them give you several ways to
specify which files to manipulate:

* If you give the command a numeric prefix argument n, it operates
  on the next n files, starting with the current file. (If n
  is negative, the command operates on the -n files preceding
  the current line.)

  * Otherwise, if some files are marked with `` `*' ``, the command operates
    on all those files.

    * Otherwise, the command operates on the current file only.

Commands which ask for a destination directory, such as those which
copy and rename files or create links for them, try to guess the default
target directory for the operation. Normally, they suggest the Dired
buffer's default directory, but if the variable `dired-dwim-target`
is non-`nil`, and if there is another Dired buffer displayed in the
next window, that other buffer's directory is suggested instead.

Here are the file-manipulating commands that operate on files in this
way. (Some other Dired commands, such as `!` and the `` `%' ``
commands, also use these conventions to decide which files to work on.)

`C new RET`: Copy the specified files (`dired-do-copy`). The argument new is the directory to copy into, or (if copying a single file) the new name. If `dired-copy-preserve-time` is non-`nil`, then copying with this command sets the modification time of the new file to be the same as that of the old file. The variable `dired-recursive-copies` controls whether directories are copied recursively. The default is to not copy recursively, which means that directories cannot be copied. `D`: Delete the specified files (`dired-do-delete`). Like the other commands in this section, this command operates on the *marked* files, or the next n files. By contrast, `x` (`dired-do-flagged-delete`) deletes all *flagged* files. `R new RET`: Rename the specified files (`dired-do-rename`). The argument new is the directory to rename into, or (if renaming a single file) the new name. Dired automatically changes the visited file name of buffers associated with renamed files so that they refer to the new names. `H new RET`: Make hard links to the specified files (`dired-do-hardlink`). The argument new is the directory to make the links in, or (if making just one link) the name to give the link. `S new RET`: Make symbolic links to the specified files (`dired-do-symlink`). The argument new is the directory to make the links in, or (if making just one link) the name to give the link. `M modespec RET`: Change the mode (also called "permission bits") of the specified files (`dired-do-chmod`). This uses the `chmod` program, so modespec can be any argument that `chmod` can handle. `G newgroup RET`: Change the group of the specified files to newgroup (`dired-do-chgrp`). `O newowner RET`: Change the owner of the specified files to newowner (`dired-do-chown`). (On most systems, only the superuser can do this.) The variable `dired-chown-program` specifies the name of the program to use to do the work (different systems put `chown` in different places). `P command RET`: Print the specified files (`dired-do-print`). You must specify the command to print them with, but the minibuffer starts out with a suitable guess made using the variables `lpr-command` and `lpr-switches` (the same variables that `lpr-buffer` uses; see section [AC.18 Hardcopy Output](emacs.html#SEC458)). `Z`: Compress the specified files (`dired-do-compress`). If the file appears to be a compressed file already, it is uncompressed instead. `L`: Load the specified Emacs Lisp files (`dired-do-load`). See section [V.7 Libraries of Lisp Code for Emacs](emacs.html#SEC328). `B`: Byte compile the specified Emacs Lisp files (`dired-do-byte-compile`). See section `Byte Compilation' in The Emacs Lisp Reference Manual. `A regexp RET`: Search all the specified files for the regular expression regexp (`dired-do-search`). This command is a variant of `tags-search`. The search stops at the first match it finds; use `M-,` to resume the search and find the next match. See section [W.2.6 Searching and Replacing with Tags Tables](emacs.html#SEC340). `Q regexp RET to RET`: Perform `query-replace-regexp` on each of the specified files, replacing matches for regexp with the string to (`dired-do-query-replace-regexp`). This command is a variant of `tags-query-replace`. If you exit the query replace loop, you can use `M-,` to resume the scan and replace more matches. See section [W.2.6 Searching and Replacing with Tags Tables](emacs.html#SEC340).

One special file-operation command is `+`
(`dired-create-directory`). This command reads a directory name and
creates the directory if it does not already exist.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC403) ] | [ [>](emacs.html#SEC405) ] | [ [<<](emacs.html#SEC405) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.8 Shell Commands in Dired

The Dired command `!` (`dired-do-shell-command`) reads a shell
command string in the minibuffer and runs that shell command on all the
specified files. `X` is a synonym for `!`. You can specify the
files to operate on in the usual ways for Dired commands
(see section [AB.7 Operating on Files](emacs.html#SEC403)). There are two ways of applying a shell
command to multiple files:

* If you use `` `*' `` in the shell command, then it runs just once, with
  the list of file names substituted for the `` `*' ``. The order of file
  names is the order of appearance in the Dired buffer.

  Thus, `! tar cf foo.tar * RET` runs `tar` on the entire
  list of file names, putting them into one tar file `foo.tar'.

  * If the command string doesn't contain `` `*' ``, then it runs once
    *for each file*, with the file name added at the end.

    For example, `! uudecode RET` runs `uudecode` on each
    file.

What if you want to run the shell command once for each file, with the
file name inserted in the middle? You can use `` `?' `` in the command
instead of `` `*' ``. The current file name is substituted for
`` `?' ``. You can use `` `?' `` more than once. For instance, here is
how to uuencode each file, making the output file name by appending
`` `.uu' `` to the input file name:

|  |  |
| --- | --- |
|  | ``` uuencode ? ? > ?.uu ``` |

To use the file names in a more complicated fashion, you can use a
shell loop. For example, this shell command is another way to
uuencode each file:

|  |  |
| --- | --- |
|  | ``` for file in *; do uuencode "$file" "$file" >"$file".uu; done ``` |

The working directory for the shell command is the top-level directory
of the Dired buffer.

The `!` command does not attempt to update the Dired buffer to show
new or modified files, because it doesn't really understand shell
commands, and does not know what files the shell command changed. Use
the `g` command to update the Dired buffer (see section [AB.14 Updating the Dired Buffer](emacs.html#SEC410)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC404) ] | [ [>](emacs.html#SEC406) ] | [ [<<](emacs.html#SEC396) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.9 Transforming File Names in Dired

This section describes Dired commands which alter file names in a
systematic way.

Like the basic Dired file-manipulation commands (see section [AB.7 Operating on Files](emacs.html#SEC403)), the commands described here operate either on the next
n files, or on all files marked with `` `*' ``, or on the current
file. (To mark files, use the commands described in [AB.6 Dired Marks vs. Flags](emacs.html#SEC402).)

All of the commands described in this section work
*interactively*: they ask you to confirm the operation for each
candidate file. Thus, you can select more files than you actually
need to operate on (e.g., with a regexp that matches many files), and
then refine the selection by typing `y` or `n` when the
command prompts for confirmation.

`% u`: Rename each of the selected files to an upper-case name (`dired-upcase`). If the old file names are `Foo' and `bar', the new names are `FOO' and `BAR'. `% l`: Rename each of the selected files to a lower-case name (`dired-downcase`). If the old file names are `Foo' and `bar', the new names are `foo' and `bar'. `% R from RET to RET`: `% C from RET to RET`: `% H from RET to RET`: `% S from RET to RET`: These four commands rename, copy, make hard links and make soft links, in each case computing the new name by regular-expression substitution from the name of the old file.

The four regular-expression substitution commands effectively perform
a search-and-replace on the selected file names in the Dired buffer.
They read two arguments: a regular expression from, and a
substitution pattern to.

The commands match each "old" file name against the regular
expression from, and then replace the matching part with to.
You can use `` `\&' `` and `` `\digit' `` in to to refer to
all or part of what the pattern matched in the old file name, as in
`replace-regexp` (see section [K.7.2 Regexp Replacement](emacs.html#SEC101)). If the regular expression
matches more than once in a file name, only the first match is replaced.

For example, `% R ^.*$ RET x-\& RET` renames each
selected file by prepending `` `x-' `` to its name. The inverse of this,
removing `` `x-' `` from the front of each file name, is also possible:
one method is `% R ^x-\(.*\)$ RET \1 RET`; another is
`% R ^x- RET RET`. (Use `` `^' `` and `` `$' `` to anchor
matches that should span the whole filename.)

Normally, the replacement process does not consider the files'
directory names; it operates on the file name within the directory. If
you specify a numeric argument of zero, then replacement affects the
entire absolute file name including directory name. (Non-zero
argument specifies the number of files to operate on.)

Often you will want to select the set of files to operate on using the
same regexp that you will use to operate on them. To do this,
mark those files with `% m regexp RET`, then use the
same regular expression in the command to operate on the files. To make
this easier, the `%` commands to operate on files use the last
regular expression specified in any `%` command as a default.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC405) ] | [ [>](emacs.html#SEC407) ] | [ [<<](emacs.html#SEC398) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.10 File Comparison with Dired

Here are two Dired commands that compare specified files using
`diff`.

`=`: Compare the current file (the file at point) with another file (the file at the mark) using the `diff` program (`dired-diff`). The file at the mark is the first argument of `diff`, and the file at point is the second argument. Use `C-SPC` (`set-mark-command`) to set the mark at the first file's line (see section [H.1 Setting the Mark](emacs.html#SEC53)), since `dired-diff` ignores the files marked with the Dired's `m` command. `M-=`: Compare the current file with its latest backup file (`dired-backup-diff`). If the current file is itself a backup, compare it with the file it is a backup of; this way, you can compare a file with any backup version of your choice. The backup file is the first file given to `diff`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC406) ] | [ [>](emacs.html#SEC408) ] | [ [<<](emacs.html#SEC398) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.11 Subdirectories in Dired

A Dired buffer displays just one directory in the normal case;
but you can optionally include its subdirectories as well.

The simplest way to include multiple directories in one Dired buffer is
to specify the options `` `-lR' `` for running `ls`. (If you give a
numeric argument when you run Dired, then you can specify these options
in the minibuffer.) That produces a recursive directory listing showing
all subdirectories at all levels.

But usually all the subdirectories are too many; usually you will
prefer to include specific subdirectories only. You can do this with
the `i` command:

`i`: Insert the contents of a subdirectory later in the buffer.

Use the `i` (`dired-maybe-insert-subdir`) command on a line
that describes a file which is a directory. It inserts the contents of
that directory into the same Dired buffer, and moves there. Inserted
subdirectory contents follow the top-level directory of the Dired
buffer, just as they do in `` `ls -lR' `` output.

If the subdirectory's contents are already present in the buffer, the
`i` command just moves to it.

In either case, `i` sets the Emacs mark before moving, so `C-u
C-SPC` takes you back to the old position in the buffer (the line
describing that subdirectory).

Use the `l` command (`dired-do-redisplay`) to update the
subdirectory's contents. Use `C-u k` on the subdirectory header
line to delete the subdirectory. See section [AB.14 Updating the Dired Buffer](emacs.html#SEC410).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC407) ] | [ [>](emacs.html#SEC409) ] | [ [<<](emacs.html#SEC398) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.12 Moving Over Subdirectories

When a Dired buffer lists subdirectories, you can use the page motion
commands `C-x [` and `C-x ]` to move by entire directories
(see section [T.4 Pages](emacs.html#SEC237)).

The following commands move across, up and down in the tree of
directories within one Dired buffer. They move to *directory header
lines*, which are the lines that give a directory's name, at the
beginning of the directory's contents.

`C-M-n`: Go to next subdirectory header line, regardless of level (`dired-next-subdir`). `C-M-p`: Go to previous subdirectory header line, regardless of level (`dired-prev-subdir`). `C-M-u`: Go up to the parent directory's header line (`dired-tree-up`). `C-M-d`: Go down in the directory tree, to the first subdirectory's header line (`dired-tree-down`). `<`: Move up to the previous directory-file line (`dired-prev-dirline`). These lines are the ones that describe a directory as a file in its parent directory. `>`: Move down to the next directory-file line (`dired-prev-dirline`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC408) ] | [ [>](emacs.html#SEC410) ] | [ [<<](emacs.html#SEC398) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.13 Hiding Subdirectories

*Hiding* a subdirectory means to make it invisible, except for its
header line, via selective display (see section [J.9 Selective Display](emacs.html#SEC86)).

`$`: Hide or reveal the subdirectory that point is in, and move point to the next subdirectory (`dired-hide-subdir`). A numeric argument serves as a repeat count. `M-$`: Hide all subdirectories in this Dired buffer, leaving only their header lines (`dired-hide-all`). Or, if any subdirectory is currently hidden, make all subdirectories visible again. You can use this command to get an overview in very deep directory trees or to move quickly to subdirectories far away.

Ordinary Dired commands never consider files inside a hidden
subdirectory. For example, the commands to operate on marked files
ignore files in hidden directories even if they are marked. Thus you
can use hiding to temporarily exclude subdirectories from operations
without having to remove the markers.

The subdirectory hiding commands toggle; that is, they hide what was
visible, and show what was hidden.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC409) ] | [ [>](emacs.html#SEC411) ] | [ [<<](emacs.html#SEC398) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.14 Updating the Dired Buffer

This section describes commands to update the Dired buffer to reflect
outside (non-Dired) changes in the directories and files, and to delete
part of the Dired buffer.

`g`: Update the entire contents of the Dired buffer (`revert-buffer`). `l`: Update the specified files (`dired-do-redisplay`). `k`: Delete the specified *file lines*---not the files, just the lines (`dired-do-kill-lines`). `s`: Toggle between alphabetical order and date/time order (`dired-sort-toggle-or-edit`). `C-u s switches RET`: Refresh the Dired buffer using switches as `dired-listing-switches`.

Type `g` (`revert-buffer`) to update the contents of the
Dired buffer, based on changes in the files and directories listed.
This preserves all marks except for those on files that have vanished.
Hidden subdirectories are updated but remain hidden.

To update only some of the files, type `l`
(`dired-do-redisplay`). Like the Dired file-operating commands,
this command operates on the next n files (or previous
-n files), or on the marked files if any, or on the
current file. Updating the files means reading their current status,
then updating their lines in the buffer to indicate that status.

If you use `l` on a subdirectory header line, it updates the
contents of the corresponding subdirectory.

To delete the specified *file lines* from the buffer--not
delete the files--type `k` (`dired-do-kill-lines`). Like
the file-operating commands, this command operates on the next n
files, or on the marked files if any; but it does not operate on the
current file as a last resort.

If you kill the line for a file that is a directory, the directory's
contents are also deleted from the buffer. Typing `C-u k` on the
header line for a subdirectory is another way to delete a subdirectory
from the Dired buffer.

The `g` command brings back any individual lines that you have
killed in this way, but not subdirectories--you must use `i` to
reinsert a subdirectory.

The files in a Dired buffers are normally listed in alphabetical order
by file names. Alternatively Dired can sort them by date/time. The
Dired command `s` (`dired-sort-toggle-or-edit`) switches
between these two sorting modes. The mode line in a Dired buffer
indicates which way it is currently sorted--by name, or by date.

`C-u s switches RET` lets you specify a new value for
`dired-listing-switches`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC410) ] | [ [>](emacs.html#SEC412) ] | [ [<<](emacs.html#SEC398) ] | [ [Up](emacs.html#SEC396) ] | [ [>>](emacs.html#SEC412) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AB.15 Dired and `find`

You can select a set of files for display in a Dired buffer more
flexibly by using the `find` utility to choose the files.

To search for files with names matching a wildcard pattern use
`M-x find-name-dired`. It reads arguments directory and
pattern, and chooses all the files in directory or its
subdirectories whose individual names match pattern.

The files thus chosen are displayed in a Dired buffer in which the
ordinary Dired commands are available.

If you want to test the contents of files, rather than their names,
use `M-x find-grep-dired`. This command reads two minibuffer
arguments, directory and regexp; it chooses all the files in
directory or its subdirectories that contain a match for
regexp. It works by running the programs `find` and
`grep`. See also `M-x grep-find`, in [V.1 Running Compilations under Emacs](emacs.html#SEC317).
Remember to write the regular expression for `grep`, not for Emacs.
(An alternative method of showing files whose contents match a given
regexp is the `% g regexp` command, see [AB.6 Dired Marks vs. Flags](emacs.html#SEC402).)

The most general command in this series is `M-x find-dired`, which
lets you specify any condition that `find` can test. It takes two
minibuffer arguments, directory and find-args; it runs
`find` in directory, passing find-args to tell
`find` what condition to test. To use this command, you need to
know how to use `find`.

`M-x locate` provides a similar interface to the `locate`
program. `M-x locate-with-filter` is similar, but keeps only lines
matching a given regular expression.

The format of listing produced by these commands is controlled by the
variable `find-ls-option`, whose default value specifies using
options `` `-ld' `` for `ls`. If your listings are corrupted, you
may need to change the value of this variable.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC411) ] | [ [>](emacs.html#SEC413) ] | [ [<<](emacs.html#SEC396) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AC. The Calendar and the Diary

Emacs provides the functions of a desk calendar, with a diary of
planned or past events. It also has facilities for managing your
appointments, and keeping track of how much time you spend working on
certain projects.

To enter the calendar, type `M-x calendar`; this displays a
three-month calendar centered on the current month, with point on the
current date. With a numeric argument, as in `C-u M-x calendar`, it
prompts you for the month and year to be the center of the three-month
calendar. The calendar uses its own buffer, whose major mode is
Calendar mode.

`Mouse-2` in the calendar brings up a menu of operations on a
particular date; `C-Mouse-3` brings up a menu of commonly used
calendar features that are independent of any particular date. To exit
the calendar, type `q`. See section `Calendar' in The Emacs Lisp Reference Manual, for customization
information about the calendar and diary.

> |  |  |  |
> | --- | --- | --- |
> | [AC.1 Movement in the Calendar](emacs.html#SEC413) |  | Moving through the calendar; selecting a date. |
> | [AC.2 Scrolling in the Calendar](emacs.html#SEC417) |  | Bringing earlier or later months onto the screen. |
> | [AC.3 Counting Days](emacs.html#SEC418) |  | How many days are there between two dates? |
> | [AC.4 Miscellaneous Calendar Commands](emacs.html#SEC419) |  | Exiting or recomputing the calendar. |
> | [AC.5 LaTeX Calendar](emacs.html#SEC420) |  | Print a calendar using LaTeX. |
> | [AC.6 Holidays](emacs.html#SEC421) |  | Displaying dates of holidays. |
> | [AC.7 Times of Sunrise and Sunset](emacs.html#SEC422) |  | Displaying local times of sunrise and sunset. |
> | [AC.8 Phases of the Moon](emacs.html#SEC423) |  | Displaying phases of the moon. |
> | [AC.9 Conversion To and From Other Calendars](emacs.html#SEC424) |  | Converting dates to other calendar systems. |
> | [AC.10 The Diary](emacs.html#SEC429) |  | Displaying events from your diary. |
> | [AC.11 Appointments](emacs.html#SEC435) |  | Reminders when it's time to do something. |
> | [AC.12 Daylight Savings Time](emacs.html#SEC436) |  | How to specify when daylight savings time is active. |
> | [AC.13 Summing Time Intervals](emacs.html#SEC437) |  | Keeping track of time intervals. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC412) ] | [ [>](emacs.html#SEC414) ] | [ [<<](emacs.html#SEC412) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC417) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.1 Movement in the Calendar

Calendar mode lets you move through the calendar in logical units of
time such as days, weeks, months, and years. If you move outside the
three months originally displayed, the calendar display "scrolls"
automatically through time to make the selected date visible. Moving to
a date lets you view its holidays or diary entries, or convert it to other
calendars; moving longer time periods is also useful simply to scroll the
calendar.

> |  |  |  |
> | --- | --- | --- |
> | [AC.1.1 Motion by Standard Lengths of Time](emacs.html#SEC414) |  | Moving by days, weeks, months, and years. |
> | [AC.1.2 Beginning or End of Week, Month or Year](emacs.html#SEC415) |  | Moving to start/end of weeks, months, and years. |
> | [AC.1.3 Specified Dates](emacs.html#SEC416) |  | Moving to the current date or another specific date. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC413) ] | [ [>](emacs.html#SEC415) ] | [ [<<](emacs.html#SEC412) ] | [ [Up](emacs.html#SEC413) ] | [ [>>](emacs.html#SEC417) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.1.1 Motion by Standard Lengths of Time

The commands for movement in the calendar buffer parallel the
commands for movement in text. You can move forward and backward by
days, weeks, months, and years.

`C-f`: Move point one day forward (`calendar-forward-day`). `C-b`: Move point one day backward (`calendar-backward-day`). `C-n`: Move point one week forward (`calendar-forward-week`). `C-p`: Move point one week backward (`calendar-backward-week`). `M-}`: Move point one month forward (`calendar-forward-month`). `M-{`: Move point one month backward (`calendar-backward-month`). `C-x ]`: Move point one year forward (`calendar-forward-year`). `C-x [`: Move point one year backward (`calendar-backward-year`).

The day and week commands are natural analogues of the usual Emacs
commands for moving by characters and by lines. Just as `C-n`
usually moves to the same column in the following line, in Calendar
mode it moves to the same day in the following week. And `C-p`
moves to the same day in the previous week.

The arrow keys are equivalent to `C-f`, `C-b`, `C-n` and
`C-p`, just as they normally are in other modes.

The commands for motion by months and years work like those for
weeks, but move a larger distance. The month commands `M-}` and
`M-{` move forward or backward by an entire month's time. The
year commands `C-x ]` and `C-x [` move forward or backward a
whole year.

The easiest way to remember these commands is to consider months and
years analogous to paragraphs and pages of text, respectively. But the
commands themselves are not quite analogous. The ordinary Emacs paragraph
commands move to the beginning or end of a paragraph, whereas these month
and year commands move by an entire month or an entire year, which usually
involves skipping across the end of a month or year.

All these commands accept a numeric argument as a repeat count.
For convenience, the digit keys and the minus sign specify numeric
arguments in Calendar mode even without the Meta modifier. For example,
`100 C-f` moves point 100 days forward from its present location.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC414) ] | [ [>](emacs.html#SEC416) ] | [ [<<](emacs.html#SEC416) ] | [ [Up](emacs.html#SEC413) ] | [ [>>](emacs.html#SEC417) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.1.2 Beginning or End of Week, Month or Year

A week (or month, or year) is not just a quantity of days; we think of
weeks (months, years) as starting on particular dates. So Calendar mode
provides commands to move to the beginning or end of a week, month or
year:

`C-a`: Move point to start of week (`calendar-beginning-of-week`). `C-e`: Move point to end of week (`calendar-end-of-week`). `M-a`: Move point to start of month (`calendar-beginning-of-month`). `M-e`: Move point to end of month (`calendar-end-of-month`). `M-<`: Move point to start of year (`calendar-beginning-of-year`). `M->`: Move point to end of year (`calendar-end-of-year`).

These commands also take numeric arguments as repeat counts, with the
repeat count indicating how many weeks, months, or years to move
backward or forward.

By default, weeks begin on Sunday. To make them begin on Monday
instead, set the variable `calendar-week-start-day` to 1.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC415) ] | [ [>](emacs.html#SEC417) ] | [ [<<](emacs.html#SEC412) ] | [ [Up](emacs.html#SEC413) ] | [ [>>](emacs.html#SEC417) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
