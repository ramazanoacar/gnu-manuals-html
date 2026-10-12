### Z.4.4 Mail Mode Miscellany

`C-c C-t`: Move to the beginning of the message body text (`mail-text`). `C-c C-w`: Insert the file `~/.signature' at the end of the message text (`mail-signature`). `C-c C-i file RET`: Insert the contents of file at the end of the outgoing message (`mail-attach-file`). `M-x ispell-message`: Perform spelling correction on the message text, but not on citations from other messages.

`C-c C-t` (`mail-text`) moves point to just after the header
separator line--that is, to the beginning of the message body text.

`C-c C-w` (`mail-signature`) adds a standard piece of text at
the end of the message to say more about who you are. The text comes
from the file `~/.signature' in your home directory. To insert
your signature automatically, set the variable `mail-signature` to
`t`; after that, starting a mail message automatically inserts the
contents of your `~/.signature' file. If you want to omit your
signature from a particular message, delete it from the buffer before
you send the message.

You can also set `mail-signature` to a string; then that string
is inserted automatically as your signature when you start editing a
message to send. If you set it to some other Lisp expression, the
expression is evaluated each time, and its value (which should be a
string) specifies the signature.

You can do spelling correction on the message text you have written
with the command `M-x ispell-message`. If you have yanked an
incoming message into the outgoing draft, this command skips what was
yanked, but it checks the text that you yourself inserted. (It looks
for indentation or `mail-yank-prefix` to distinguish the cited
lines from your input.) See section [L.4 Checking and Correcting Spelling](emacs.html#SEC109).

To include a file in the outgoing message, you can use `C-x i`,
the usual command to insert a file in the current buffer. But it is
often more convenient to use a special command, `C-c C-i`
(`mail-attach-file`). This command inserts the file contents at
the end of the buffer, after your signature if any, with a delimiter
line that includes the file name.

Turning on Mail mode (which `C-x m` does automatically) runs the
normal hooks `text-mode-hook` and `mail-mode-hook`.
Initializing a new outgoing message runs the normal hook
`mail-setup-hook`; if you want to add special fields to your mail
header or make other changes to the appearance of the mail buffer, use
that hook. See section [AD.2.3 Hooks](emacs.html#SEC484).

The main difference between these hooks is just when they are
invoked. Whenever you type `M-x mail`, `mail-mode-hook` runs
as soon as the `` `*mail*' `` buffer is created. Then the
`mail-setup` function inserts the default contents of the buffer.
After these default contents are inserted, `mail-setup-hook` runs.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC371) ] | [ [>](emacs.html#SEC373) ] | [ [<<](emacs.html#SEC373) ] | [ [Up](emacs.html#SEC363) ] | [ [>>](emacs.html#SEC374) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Z.5 Mail Amusements

`M-x spook` adds a line of randomly chosen keywords to an outgoing
mail message. The keywords are chosen from a list of words that suggest
you are discussing something subversive.

The idea behind this feature is the suspicion that the
NSA[(13)](emacs.html#FOOT13) snoops on
all electronic mail messages that contain keywords suggesting they might
find them interesting. (The NSA says they don't, but that's what they
*would* say.) The idea is that if lots of people add suspicious
words to their messages, the NSA will get so busy with spurious input
that they will have to give up reading it all.

Here's how to insert spook keywords automatically whenever you start
entering an outgoing message:

|  |  |
| --- | --- |
|  | ``` (add-hook 'mail-setup-hook 'spook) ``` |

Whether or not this confuses the NSA, it at least amuses people.

You can use the `fortune` program to put a "fortune cookie"
message into outgoing mail. To do this, add
`fortune-to-signature` to `mail-setup-hook`:

|  |  |
| --- | --- |
|  | ``` (add-hook 'mail-setup-hook 'fortune-to-signature) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC372) ] | [ [>](emacs.html#SEC374) ] | [ [<<](emacs.html#SEC363) ] | [ [Up](emacs.html#SEC363) ] | [ [>>](emacs.html#SEC374) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Z.6 Mail-Composition Methods

In this chapter we have described the usual Emacs mode for editing
and sending mail--Mail mode. Emacs has alternative facilities for
editing and sending mail, including
MH-E and Message mode, not documented in this manual.
See section `MH-E' in The Emacs Interface to MH. See section `Message' in Message Manual. You can choose any of them as your preferred method.
The commands `C-x m`, `C-x 4 m` and `C-x 5 m` use
whichever agent you have specified, as do various other Emacs commands
and facilities that send mail.

To specify your mail-composition method, customize the variable
`mail-user-agent`. Currently legitimate values include
`sendmail-user-agent` (Mail mode), `mh-e-user-agent`,
`message-user-agent` and `gnus-user-agent`.

If you select a different mail-composition method, the information
in this chapter about the `` `*mail*' `` buffer and Mail mode does not
apply; the other methods use a different format of text in a different
buffer, and their commands are different as well.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC373) ] | [ [>](emacs.html#SEC375) ] | [ [<<](emacs.html#SEC363) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AA. Reading Mail with Rmail

Rmail is an Emacs subsystem for reading and disposing of mail that you
receive. Rmail stores mail messages in files called Rmail files.
Reading the message in an Rmail file is done in a special major mode,
Rmail mode, which redefines most letters to run commands for managing
mail. The command `rmail-mode` is used to switch into Rmail mode,
and it runs the hook `rmail-mode-hook` as usual, but don't run this
command by hand; it can't do a reasonable job unless the buffer is
visiting a proper Rmail file.

> |  |  |  |
> | --- | --- | --- |
> | [AA.1 Basic Concepts of Rmail](emacs.html#SEC375) |  | Basic concepts of Rmail, and simple use. |
> | [AA.2 Scrolling Within a Message](emacs.html#SEC376) |  | Scrolling through a message. |
> | [AA.3 Moving Among Messages](emacs.html#SEC377) |  | Moving to another message. |
> | [AA.4 Deleting Messages](emacs.html#SEC378) |  | Deleting and expunging messages. |
> | [AA.5 Rmail Files and Inboxes](emacs.html#SEC379) |  | How mail gets into the Rmail file. |
> | [AA.6 Multiple Rmail Files](emacs.html#SEC380) |  | Using multiple Rmail files. |
> | [AA.7 Copying Messages Out to Files](emacs.html#SEC381) |  | Copying message out to files. |
> | [AA.8 Labels](emacs.html#SEC382) |  | Classifying messages by labeling them. |
> | [AA.9 Rmail Attributes](emacs.html#SEC383) |  | Certain standard labels, called attributes. |
> | [AA.10 Sending Replies](emacs.html#SEC384) |  | Sending replies to messages you are viewing. |
> | [AA.11 Summaries](emacs.html#SEC385) |  | Summaries show brief info on many messages. |
> | [AA.12 Sorting the Rmail File](emacs.html#SEC388) |  | Sorting messages in Rmail. |
> | [AA.13 Display of Messages](emacs.html#SEC389) |  | How Rmail displays a message; customization. |
> | [AA.14 Rmail and Coding Systems](emacs.html#SEC390) |  | How Rmail handles decoding character sets. |
> | [AA.15 Editing Within a Message](emacs.html#SEC391) |  | Editing message text and headers in Rmail. |
> | [AA.16 Digest Messages](emacs.html#SEC392) |  | Extracting the messages from a digest message. |
> | [AA.17 Converting an Rmail File to Inbox Format](emacs.html#SEC393) |  | Converting an Rmail file to mailbox format. |
> | [AA.18 Reading Rot13 Messages](emacs.html#SEC394) |  | Reading messages encoded in the rot13 code. |
> | [AA.19 `movemail` and POP](emacs.html#SEC395) |  | More details of fetching new mail. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC374) ] | [ [>](emacs.html#SEC376) ] | [ [<<](emacs.html#SEC374) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.1 Basic Concepts of Rmail

Using Rmail in the simplest fashion, you have one Rmail file
`~/RMAIL' in which all of your mail is saved. It is called your
*primary Rmail file*. The command `M-x rmail` reads your primary
Rmail file, merges new mail in from your inboxes, displays the first
message you haven't read yet, and lets you begin reading. The variable
`rmail-file-name` specifies the name of the primary Rmail file.

Rmail uses narrowing to hide all but one message in the Rmail file.
The message that is shown is called the *current message*. Rmail
mode's special commands can do such things as delete the current
message, copy it into another file, send a reply, or move to another
message. You can also create multiple Rmail files and use Rmail to move
messages between them.

Within the Rmail file, messages are normally arranged sequentially in
order of receipt; you can specify other ways to sort them. Messages are
assigned consecutive integers as their *message numbers*. The
number of the current message is displayed in Rmail's mode line,
followed by the total number of messages in the file. You can move to a
message by specifying its message number with the `j` key
(see section [AA.3 Moving Among Messages](emacs.html#SEC377)).

Following the usual conventions of Emacs, changes in an Rmail file
become permanent only when the file is saved. You can save it with
`s` (`rmail-expunge-and-save`), which also expunges deleted
messages from the file first (see section [AA.4 Deleting Messages](emacs.html#SEC378)). To save the
file without expunging, use `C-x C-s`. Rmail also saves the Rmail
file after merging new mail from an inbox file (see section [AA.5 Rmail Files and Inboxes](emacs.html#SEC379)).

You can exit Rmail with `q` (`rmail-quit`); this expunges and
saves the Rmail file and then switches to another buffer. But there is
no need to "exit" formally. If you switch from Rmail to editing in
other buffers, and never happen to switch back, you have exited. (The
Rmail command `b`, `rmail-bury`, does this for you.) Just make
sure to save the Rmail file eventually (like any other file you have
changed). `C-x s` is a good enough way to do this
(see section [M.3 Saving Files](emacs.html#SEC113)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC375) ] | [ [>](emacs.html#SEC377) ] | [ [<<](emacs.html#SEC377) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.2 Scrolling Within a Message

When Rmail displays a message that does not fit on the screen, you
must scroll through it to read the rest. You could do this with
`C-v`, `M-v` and `M-<`, but in Rmail scrolling is so
frequent that it deserves to be easier to type.

`SPC`: Scroll forward (`scroll-up`). `DEL`: Scroll backward (`scroll-down`). `.`: Scroll to start of message (`rmail-beginning-of-message`).

Since the most common thing to do while reading a message is to scroll
through it by screenfuls, Rmail makes `SPC` and `DEL` synonyms of
`C-v` (`scroll-up`) and `M-v` (`scroll-down`)

The command `.` (`rmail-beginning-of-message`) scrolls back to the
beginning of the selected message. This is not quite the same as `M-<`:
for one thing, it does not set the mark; for another, it resets the buffer
boundaries to the current message if you have changed them.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC376) ] | [ [>](emacs.html#SEC378) ] | [ [<<](emacs.html#SEC378) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.3 Moving Among Messages

The most basic thing to do with a message is to read it. The way to
do this in Rmail is to make the message current. The usual practice is
to move sequentially through the file, since this is the order of
receipt of messages. When you enter Rmail, you are positioned at the
first message that you have not yet made current (that is, the first one
that has the `` `unseen' `` attribute; see section [AA.9 Rmail Attributes](emacs.html#SEC383)). Move
forward to see the other new messages; move backward to re-examine old
messages.

`n`: Move to the next nondeleted message, skipping any intervening deleted messages (`rmail-next-undeleted-message`). `p`: Move to the previous nondeleted message (`rmail-previous-undeleted-message`). `M-n`: Move to the next message, including deleted messages (`rmail-next-message`). `M-p`: Move to the previous message, including deleted messages (`rmail-previous-message`). `j`: Move to the first message. With argument n, move to message number n (`rmail-show-message`). `>`: Move to the last message (`rmail-last-message`). `<`: Move to the first message (`rmail-first-message`). `M-s regexp RET`: Move to the next message containing a match for regexp (`rmail-search`). `- M-s regexp RET`: Move to the previous message containing a match for regexp.

`n` and `p` are the usual way of moving among messages in
Rmail. They move through the messages sequentially, but skip over
deleted messages, which is usually what you want to do. Their command
definitions are named `rmail-next-undeleted-message` and
`rmail-previous-undeleted-message`. If you do not want to skip
deleted messages--for example, if you want to move to a message to
undelete it--use the variants `M-n` and `M-p`
(`rmail-next-message` and `rmail-previous-message`). A
numeric argument to any of these commands serves as a repeat
count.

In Rmail, you can specify a numeric argument by typing just the
digits. You don't need to type `C-u` first.

The `M-s` (`rmail-search`) command is Rmail's version of
search. The usual incremental search command `C-s` works in Rmail,
but it searches only within the current message. The purpose of
`M-s` is to search for another message. It reads a regular
expression (see section [K.5 Syntax of Regular Expressions](emacs.html#SEC97)) nonincrementally, then searches starting at
the beginning of the following message for a match. It then selects
that message. If regexp is empty, `M-s` reuses the regexp
used the previous time.

To search backward in the file for another message, give `M-s` a
negative argument. In Rmail you can do this with `- M-s`.

It is also possible to search for a message based on labels.
See section [AA.8 Labels](emacs.html#SEC382).

To move to a message specified by absolute message number, use `j`
(`rmail-show-message`) with the message number as argument. With
no argument, `j` selects the first message. `<`
(`rmail-first-message`) also selects the first message. `>`
(`rmail-last-message`) selects the last message.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC377) ] | [ [>](emacs.html#SEC379) ] | [ [<<](emacs.html#SEC379) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.4 Deleting Messages

When you no longer need to keep a message, you can *delete* it. This
flags it as ignorable, and some Rmail commands pretend it is no longer
present; but it still has its place in the Rmail file, and still has its
message number.

*Expunging* the Rmail file actually removes the deleted messages.
The remaining messages are renumbered consecutively. Expunging is the only
action that changes the message number of any message, except for
undigestifying (see section [AA.16 Digest Messages](emacs.html#SEC392)).

`d`: Delete the current message, and move to the next nondeleted message (`rmail-delete-forward`). `C-d`: Delete the current message, and move to the previous nondeleted message (`rmail-delete-backward`). `u`: Undelete the current message, or move back to a deleted message and undelete it (`rmail-undelete-previous-message`). `x`: Expunge the Rmail file (`rmail-expunge`).

There are two Rmail commands for deleting messages. Both delete the
current message and select another message. `d`
(`rmail-delete-forward`) moves to the following message, skipping
messages already deleted, while `C-d` (`rmail-delete-backward`)
moves to the previous nondeleted message. If there is no nondeleted
message to move to in the specified direction, the message that was just
deleted remains current. A numeric argument to either command reverses
the direction of motion after deletion.

Whenever Rmail deletes a message, it runs the hook
`rmail-delete-message-hook`. When the hook functions are invoked,
the message has been marked deleted, but it is still the current message
in the Rmail buffer.

To make all the deleted messages finally vanish from the Rmail file,
type `x` (`rmail-expunge`). Until you do this, you can still
*undelete* the deleted messages. The undeletion command, `u`
(`rmail-undelete-previous-message`), is designed to cancel the
effect of a `d` command in most cases. It undeletes the current
message if the current message is deleted. Otherwise it moves backward
to previous messages until a deleted message is found, and undeletes
that message.

You can usually undo a `d` with a `u` because the `u`
moves back to and undeletes the message that the `d` deleted. But
this does not work when the `d` skips a few already-deleted messages
that follow the message being deleted; then the `u` command
undeletes the last of the messages that were skipped. There is no clean
way to avoid this problem. However, by repeating the `u` command,
you can eventually get back to the message that you intend to
undelete. You can also select a particular deleted message with
the `M-p` command, then type `u` to undelete it.

A deleted message has the `` `deleted' `` attribute, and as a result
`` `deleted' `` appears in the mode line when the current message is
deleted. In fact, deleting or undeleting a message is nothing more than
adding or removing this attribute. See section [AA.9 Rmail Attributes](emacs.html#SEC383).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC378) ] | [ [>](emacs.html#SEC380) ] | [ [<<](emacs.html#SEC380) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.5 Rmail Files and Inboxes

The operating system places incoming mail for you in a file that we
call your *inbox*. When you start up Rmail, it runs a C program
called `movemail` to copy the new messages from your inbox into
your primary Rmail file, which also contains other messages saved from
previous Rmail sessions. It is in this file that you actually read the
mail with Rmail. This operation is called *getting new mail*. You
can get new mail at any time in Rmail by typing `g`.

The variable `rmail-primary-inbox-list` contains a list of the
files which are inboxes for your primary Rmail file. If you don't set
this variable explicitly, it is initialized from the `MAIL`
environment variable, or, as a last resort, set to `nil`, which
means to use the default inbox. The default inbox is
`/var/mail/username', `/usr/spool/mail/username',
or `/usr/mail/username', depending on your operating system.

To see what the default is on your system, use `C-h v
rmail-primary-inbox RET`. You can specify the inbox file(s) for
any Rmail file with the command `set-rmail-inbox-list`; see
[AA.6 Multiple Rmail Files](emacs.html#SEC380).

There are two reasons for having separate Rmail files and inboxes.

1. The inbox file format varies between operating systems and according to
   the other mail software in use. Only one part of Rmail needs to know
   about the alternatives, and it need only understand how to convert all
   of them to Rmail's own format.

   - It is very cumbersome to access an inbox file without danger of losing
     mail, because it is necessary to interlock with mail delivery.
     Moreover, different operating systems use different interlocking
     techniques. The strategy of moving mail out of the inbox once and for
     all into a separate Rmail file avoids the need for interlocking in all
     the rest of Rmail, since only Rmail operates on the Rmail file.

Rmail was written to use Babyl format as its internal format. Since
then, we have recognized that the usual inbox format on Unix and GNU
systems is adequate for the job, and we plan to change Rmail to use that
as its internal format. However, the Rmail file will still be separate
from the inbox file, even on systems where their format is the same.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC379) ] | [ [>](emacs.html#SEC381) ] | [ [<<](emacs.html#SEC381) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.6 Multiple Rmail Files

Rmail operates by default on your *primary Rmail file*, which is named
`~/RMAIL' and receives your incoming mail from your system inbox file.
But you can also have other Rmail files and edit them with Rmail. These
files can receive mail through their own inboxes, or you can move messages
into them with explicit Rmail commands (see section [AA.7 Copying Messages Out to Files](emacs.html#SEC381)).

`i file RET`: Read file into Emacs and run Rmail on it (`rmail-input`). `M-x set-rmail-inbox-list RET files RET`: Specify inbox file names for current Rmail file to get mail from. `g`: Merge new mail from current Rmail file's inboxes (`rmail-get-new-mail`). `C-u g file RET`: Merge new mail from inbox file file.

To run Rmail on a file other than your primary Rmail file, you may use
the `i` (`rmail-input`) command in Rmail. This visits the file
in Rmail mode. You can use `M-x rmail-input` even when not in
Rmail.

The file you read with `i` should normally be a valid Rmail file.
If it is not, Rmail tries to decompose it into a stream of messages in
various known formats. If it succeeds, it converts the whole file to an
Rmail file. If you specify a file name that doesn't exist, `i`
initializes a new buffer for creating a new Rmail file.

You can also select an Rmail file from a menu. Choose first the menu
bar Classify item, then from the Classify menu choose the Input Rmail
File item; then choose the Rmail file you want. The variables
`rmail-secondary-file-directory` and
`rmail-secondary-file-regexp` specify which files to offer in the
menu: the first variable says which directory to find them in; the
second says which files in that directory to offer (all those that match
the regular expression). These variables also apply to choosing a file
for output (see section [AA.7 Copying Messages Out to Files](emacs.html#SEC381)).

Each Rmail file can contain a list of inbox file names; you can specify
this list with `M-x set-rmail-inbox-list RET files
RET`. The argument can contain any number of file names, separated
by commas. It can also be empty, which specifies that this file should
have no inboxes. Once a list of inboxes is specified, the Rmail file
remembers it permanently until you specify a different list.

As a special exception, if your primary Rmail file does not specify any
inbox files, it uses your standard system inbox.

The `g` command (`rmail-get-new-mail`) merges mail into the
current Rmail file from its specified inboxes. If the Rmail file
has no inboxes, `g` does nothing. The command `M-x rmail`
also merges new mail into your primary Rmail file.

To merge mail from a file that is not the usual inbox, give the
`g` key a numeric argument, as in `C-u g`. Then it reads a file
name and merges mail from that file. The inbox file is not deleted or
changed in any way when `g` with an argument is used. This is,
therefore, a general way of merging one file of messages into another.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC380) ] | [ [>](emacs.html#SEC382) ] | [ [<<](emacs.html#SEC382) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.7 Copying Messages Out to Files

These commands copy messages from an Rmail file into another file.

`o file RET`: Append a copy of the current message to the file file, using Rmail file format by default (`rmail-output-to-rmail-file`). `C-o file RET`: Append a copy of the current message to the file file, using system inbox file format by default (`rmail-output`). `w file RET`: Output just the message body to the file file, taking the default file name from the message `` `Subject' `` header.

The commands `o` and `C-o` copy the current message into a
specified file. This file may be an Rmail file or it may be in system
inbox format; the output commands ascertain the file's format and write
the copied message in that format.

The `o` and `C-o` commands differ in two ways: each has its
own separate default file name, and each specifies a choice of format to
use when the file does not already exist. The `o` command uses
Rmail format when it creates a new file, while `C-o` uses system
inbox format for a new file. The default file name for `o` is the
file name used last with `o`, and the default file name for
`C-o` is the file name used last with `C-o`.

If the output file is an Rmail file currently visited in an Emacs buffer,
the output commands copy the message into that buffer. It is up to you
to save the buffer eventually in its file.

Sometimes you may receive a message whose body holds the contents of a
file. You can save the body to a file (excluding the message header)
with the `w` command (`rmail-output-body-to-file`). Often
these messages contain the intended file name in the `` `Subject' ``
field, so the `w` command uses the `` `Subject' `` field as the
default for the output file name. However, the file name is read using
the minibuffer, so you can specify a different name if you wish.

You can also output a message to an Rmail file chosen with a menu.
Choose first the menu bar Classify item, then from the Classify menu
choose the Output Rmail File menu item; then choose the Rmail file you want.
This outputs the current message to that file, like the `o` command.
The variables `rmail-secondary-file-directory` and
`rmail-secondary-file-regexp` specify which files to offer in the
menu: the first variable says which directory to find them in; the
second says which files in that directory to offer (all those that match
the regular expression).

Copying a message gives the original copy of the message the
`` `filed' `` attribute, so that `` `filed' `` appears in the mode line
when such a message is current. If you like to keep just a single copy
of every mail message, set the variable `rmail-delete-after-output`
to `t`; then the `o` and `C-o` commands delete the original
message after copying it. (You can undelete the original afterward if
you wish.)

Copying messages into files in system inbox format uses the header
fields that are displayed in Rmail at the time. Thus, if you use the
`t` command to view the entire header and then copy the message, the
entire header is copied. See section [AA.13 Display of Messages](emacs.html#SEC389).

The variable `rmail-output-file-alist` lets you specify
intelligent defaults for the output file, based on the contents of the
current message. The value should be a list whose elements have this
form:

|  |  |
| --- | --- |
|  | ``` (regexp . name-exp) ``` |

If there's a match for regexp in the current message, then the
default file name for output is name-exp. If multiple elements
match the message, the first matching element decides the default file
name. The subexpression name-exp may be a string constant giving
the file name to use, or more generally it may be any Lisp expression
that returns a file name as a string. `rmail-output-file-alist`
applies to both `o` and `C-o`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC381) ] | [ [>](emacs.html#SEC383) ] | [ [<<](emacs.html#SEC383) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.8 Labels

Each message can have various *labels* assigned to it as a means
of classification. Each label has a name; different names are different
labels. Any given label is either present or absent on a particular
message. A few label names have standard meanings and are given to
messages automatically by Rmail when appropriate; these special labels
are called *attributes*.
(See section [AA.9 Rmail Attributes](emacs.html#SEC383).)
All other labels are assigned only by users.

`a label RET`: Assign the label label to the current message (`rmail-add-label`). `k label RET`: Remove the label label from the current message (`rmail-kill-label`). `C-M-n labels RET`: Move to the next message that has one of the labels labels (`rmail-next-labeled-message`). `C-M-p labels RET`: Move to the previous message that has one of the labels labels (`rmail-previous-labeled-message`). `C-M-l labels RET`: Make a summary of all messages containing any of the labels labels (`rmail-summary-by-labels`).

The `a` (`rmail-add-label`) and `k`
(`rmail-kill-label`) commands allow you to assign or remove any
label on the current message. If the label argument is empty, it
means to assign or remove the same label most recently assigned or
removed.

Once you have given messages labels to classify them as you wish, there
are two ways to use the labels: in moving and in summaries.

The command `C-M-n labels RET`
(`rmail-next-labeled-message`) moves to the next message that has
one of the labels labels. The argument labels specifies one
or more label names, separated by commas. `C-M-p`
(`rmail-previous-labeled-message`) is similar, but moves backwards
to previous messages. A numeric argument to either command serves as a
repeat count.

The command `C-M-l labels RET`
(`rmail-summary-by-labels`) displays a summary containing only the
messages that have at least one of a specified set of labels. The
argument labels is one or more label names, separated by commas.
See section [AA.11 Summaries](emacs.html#SEC385), for information on summaries.

If the labels argument to `C-M-n`, `C-M-p` or
`C-M-l` is empty, it means to use the last set of labels specified
for any of these commands.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC382) ] | [ [>](emacs.html#SEC384) ] | [ [<<](emacs.html#SEC374) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.9 Rmail Attributes

Some labels such as `` `deleted' `` and `` `filed' `` have built-in
meanings and are assigned to or removed from messages automatically at
appropriate times; these labels are called *attributes*. Here is a
list of Rmail attributes:

`` `unseen' ``: Means the message has never been current. Assigned to messages when they come from an inbox file, and removed when a message is made current. When you start Rmail, it initially shows the first message that has this attribute. `` `deleted' ``: Means the message is deleted. Assigned by deletion commands and removed by undeletion commands (see section [AA.4 Deleting Messages](emacs.html#SEC378)). `` `filed' ``: Means the message has been copied to some other file. Assigned by the file output commands (see section [AA.6 Multiple Rmail Files](emacs.html#SEC380)). `` `answered' ``: Means you have mailed an answer to the message. Assigned by the `r` command (`rmail-reply`). See section [AA.10 Sending Replies](emacs.html#SEC384). `` `forwarded' ``: Means you have forwarded the message. Assigned by the `f` command (`rmail-forward`). See section [AA.10 Sending Replies](emacs.html#SEC384). `` `edited' ``: Means you have edited the text of the message within Rmail. See section [AA.15 Editing Within a Message](emacs.html#SEC391). `` `resent' ``: Means you have resent the message. Assigned by the command `M-x rmail-resend`. See section [AA.10 Sending Replies](emacs.html#SEC384).

All other labels are assigned or removed only by the user, and have no
standard meaning.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC383) ] | [ [>](emacs.html#SEC385) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC396) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.10 Sending Replies

Rmail has several commands that use Mail mode to send outgoing mail.
See section [Z. Sending Mail](emacs.html#SEC363), for information on using Mail mode, including
certain features meant to work with Rmail. What this section documents
are the special commands of Rmail for entering Mail mode. Note that the
usual keys for sending mail---`C-x m`, `C-x 4 m`, and `C-x 5
m`---are available in Rmail mode and work just as they usually do.

`m`: Send a message (`rmail-mail`). `c`: Continue editing the already started outgoing message (`rmail-continue`). `r`: Send a reply to the current Rmail message (`rmail-reply`). `f`: Forward the current message to other users (`rmail-forward`). `C-u f`: Resend the current message to other users (`rmail-resend`). `M-m`: Try sending a bounced message a second time (`rmail-retry-failure`).

The most common reason to send a message while in Rmail is to reply to
the message you are reading. To do this, type `r`
(`rmail-reply`). This displays the `` `*mail*' `` buffer in another
window, much like `C-x 4 m`, but preinitializes the `` `Subject' ``,
`` `To' ``, `` `CC' `` and `` `In-reply-to' `` header fields based on the
message you are replying to. The `` `To' `` field starts out as the
address of the person who sent the message you received, and the
`` `CC' `` field starts out with all the other recipients of that
message.

You can exclude certain recipients from being placed automatically in
the `` `CC' ``, using the variable `rmail-dont-reply-to-names`. Its
value should be a regular expression (as a string); any recipient that
the regular expression matches, is excluded from the `` `CC' `` field.
The default value matches your own name, and any name starting with
`` `info-' ``. (Those names are excluded because there is a convention
of using them for large mailing lists to broadcast announcements.)

To omit the `` `CC' `` field completely for a particular reply, enter
the reply command with a numeric argument: `C-u r` or `1 r`.

Once the `` `*mail*' `` buffer has been initialized, editing and
sending the mail goes as usual (see section [Z. Sending Mail](emacs.html#SEC363)). You can edit the
presupplied header fields if they are not right for you. You can also
use the commands of Mail mode (see section [Z.4 Mail Mode](emacs.html#SEC367)), including `C-c
C-y` which yanks in the message that you are replying to. You can
switch to the Rmail buffer, select a different message there, switch
back, and yank the new current message.

Sometimes a message does not reach its destination. Mailers usually
send the failed message back to you, enclosed in a *failure
message*. The Rmail command `M-m` (`rmail-retry-failure`)
prepares to send the same message a second time: it sets up a
`` `*mail*' `` buffer with the same text and header fields as before. If
you type `C-c C-c` right away, you send the message again exactly
the same as the first time. Alternatively, you can edit the text or
headers and then send it. The variable
`rmail-retry-ignored-headers`, in the same format as
`rmail-ignored-headers` (see section [AA.13 Display of Messages](emacs.html#SEC389)), controls which
headers are stripped from the failed message when retrying it.

Another frequent reason to send mail in Rmail is to *forward* the
current message to other users. `f` (`rmail-forward`) makes
this easy by preinitializing the `` `*mail*' `` buffer with the current
message as the text, and a subject designating a forwarded message. All
you have to do is fill in the recipients and send. When you forward a
message, recipients get a message which is "from" you, and which has
the original message in its contents.

Forwarding a message encloses it between two delimiter lines. It also
modifies every line that starts with a dash, by inserting `` `- ' ``
at the start of the line. When you receive a forwarded message, if it
contains something besides ordinary text--for example, program source
code--you might find it useful to undo that transformation. You can do
this by selecting the forwarded message and typing `M-x
unforward-rmail-message`. This command extracts the original forwarded
message, deleting the inserted `` `- ' `` strings, and inserts it
into the Rmail file as a separate message immediately following the
current one.

*Resending* is an alternative similar to forwarding; the
difference is that resending sends a message that is "from" the
original sender, just as it reached you--with a few added header fields
`` `Resent-From' `` and `` `Resent-To' `` to indicate that it came via
you. To resend a message in Rmail, use `C-u f`. (`f` runs
`rmail-forward`, which is programmed to invoke `rmail-resend`
if you provide a numeric argument.)

The `m` (`rmail-mail`) command is used to start editing an
outgoing message that is not a reply. It leaves the header fields empty.
Its only difference from `C-x 4 m` is that it makes the Rmail buffer
accessible for `C-c C-y`, just as `r` does. Thus, `m` can be
used to reply to or forward a message; it can do anything `r` or `f`
can do.

The `c` (`rmail-continue`) command resumes editing the
`` `*mail*' `` buffer, to finish editing an outgoing message you were
already composing, or to alter a message you have sent.

If you set the variable `rmail-mail-new-frame` to a
non-`nil` value, then all the Rmail commands to start sending a
message create a new frame to edit it in. This frame is deleted when
you send the message, or when you use the `` `Cancel' `` item in the
`` `Mail' `` menu.

All the Rmail commands to send a message use the mail-composition
method that you have chosen (see section [Z.6 Mail-Composition Methods](emacs.html#SEC373)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC384) ] | [ [>](emacs.html#SEC386) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC374) ] | [ [>>](emacs.html#SEC388) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AA.11 Summaries

A *summary* is a buffer containing one line per message to give
you an overview of the mail in an Rmail file. Each line shows the
message number and date, the sender, the line count, the labels, and
the subject. Moving point in the summary buffer selects messages as
you move to their summary lines. Almost all Rmail commands are valid
in the summary buffer also; when used there, they apply to the message
described by the current line of the summary.

A summary buffer applies to a single Rmail file only; if you are
editing multiple Rmail files, each one can have its own summary buffer.
The summary buffer name is made by appending `` `-summary' `` to the
Rmail buffer's name. Normally only one summary buffer is displayed at a
time.

> |  |  |  |
> | --- | --- | --- |
> | [AA.11.1 Making Summaries](emacs.html#SEC386) |  | Making various sorts of summaries. |
> | [AA.11.2 Editing in Summaries](emacs.html#SEC387) |  | Manipulating messages from the summary. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC385) ] | [ [>](emacs.html#SEC387) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC385) ] | [ [>>](emacs.html#SEC388) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AA.11.1 Making Summaries

Here are the commands to create a summary for the current Rmail file.
Once the Rmail file has a summary buffer, changes in the Rmail file
(such as deleting or expunging messages, and getting new mail)
automatically update the summary.

`h`: `C-M-h`: Summarize all messages (`rmail-summary`). `l labels RET`: `C-M-l labels RET`: Summarize messages that have one or more of the specified labels (`rmail-summary-by-labels`). `C-M-r rcpts RET`: Summarize messages that have one or more of the specified recipients (`rmail-summary-by-recipients`). `C-M-t topic RET`: Summarize messages that have a match for the specified regexp topic in their subjects (`rmail-summary-by-topic`). `C-M-s regexp`: Summarize messages whose headers and the subject line match the specified regular expression regexp (`rmail-summary-by-regexp`).

The `h` or `C-M-h` (`rmail-summary`) command fills the summary buffer
for the current Rmail file with a summary of all the messages in the file.
It then displays and selects the summary buffer in another window.

`C-M-l labels RET` (`rmail-summary-by-labels`) makes
a partial summary mentioning only the messages that have one or more of the
labels labels. labels should contain label names separated by
commas.

`C-M-r rcpts RET` (`rmail-summary-by-recipients`)
makes a partial summary mentioning only the messages that have one or more
of the recipients rcpts. rcpts should contain mailing
addresses separated by commas.

`C-M-t topic RET` (`rmail-summary-by-topic`)
makes a partial summary mentioning only the messages whose subjects have
a match for the regular expression topic.

`C-M-s regexp RET` (`rmail-summary-by-regexp`)
makes a partial summary which mentions only the messages whose headers
(including the date and the subject lines) match the regular
expression regexp.

Note that there is only one summary buffer for any Rmail file; making one
kind of summary discards any previously made summary.

The variable `rmail-summary-window-size` says how many lines to
use for the summary window. The variable
`rmail-summary-line-count-flag` controls whether the summary line
for a message should include the line count of the message.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC386) ] | [ [>](emacs.html#SEC388) ] | [ [<<](emacs.html#SEC376) ] | [ [Up](emacs.html#SEC385) ] | [ [>>](emacs.html#SEC388) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
