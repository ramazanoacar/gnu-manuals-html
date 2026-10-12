### M.3.1 Backup Files

On most operating systems, rewriting a file automatically destroys all
record of what the file used to contain. Thus, saving a file from Emacs
throws away the old contents of the file--or it would, except that
Emacs carefully copies the old contents to another file, called the
*backup* file, before actually saving.

For most files, the variable `make-backup-files` determines
whether to make backup files. On most operating systems, its default
value is `t`, so that Emacs does write backup files.

For files managed by a version control system (see section [M.7 Version Control](emacs.html#SEC127)), the variable `vc-make-backup-files` determines whether
to make backup files. By default it is `nil`, since backup files
are redundant when you store all the previous versions in a version
control system. See section [M.7.10.1 General Options](emacs.html#SEC160).

The default value of the `backup-enable-predicate` variable
prevents backup files being written for files in the directories used
for temporary files, specified by `temporary-file-directory` or
`small-temporary-file-directory`.

At your option, Emacs can keep either a single backup file or a series of
numbered backup files for each file that you edit.

Emacs makes a backup for a file only the first time the file is saved
from one buffer. No matter how many times you save a file, its backup file
continues to contain the contents from before the file was visited.
Normally this means that the backup file contains the contents from before
the current editing session; however, if you kill the buffer and then visit
the file again, a new backup file will be made by the next save.

You can also explicitly request making another backup file from a
buffer even though it has already been saved at least once. If you save
the buffer with `C-u C-x C-s`, the version thus saved will be made
into a backup file if you save the buffer again. `C-u C-u C-x C-s`
saves the buffer, but first makes the previous file contents into a new
backup file. `C-u C-u C-u C-x C-s` does both things: it makes a
backup from the previous contents, and arranges to make another from the
newly saved contents if you save again.

> |  |  |  |
> | --- | --- | --- |
> | [M.3.1.1 Single or Numbered Backups](emacs.html#SEC115) |  | How backup files are named; choosing single or numbered backup files. |
> | [M.3.1.2 Automatic Deletion of Backups](emacs.html#SEC116) |  | Emacs deletes excess numbered backups. |
> | [M.3.1.3 Copying vs. Renaming](emacs.html#SEC117) |  | Backups can be made by copying or renaming. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC114) ] | [ [>](emacs.html#SEC116) ] | [ [<<](emacs.html#SEC121) ] | [ [Up](emacs.html#SEC114) ] | [ [>>](emacs.html#SEC118) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.3.1.1 Single or Numbered Backups

If you choose to have a single backup file (this is the default),
the backup file's name is normally constructed by appending `` `~' `` to the
file name being edited; thus, the backup file for `eval.c' would
be `eval.c~'.

You can change this behavior by defining the variable
`make-backup-file-name-function` to a suitable function.
Alternatively you can customize the variable
`backup-directory-alist` to specify that files matching certain
patterns should be backed up in specific directories.

A typical use is to add an element `("." . dir)` to make
all backups in the directory with absolute name dir; Emacs
modifies the backup file names to avoid clashes between files with the
same names originating in different directories. Alternatively,
adding, say, `("." . ".~")` would make backups in the invisible
subdirectory `.~' of the original file's directory. Emacs
creates the directory, if necessary, to make the backup.

If access control stops Emacs from writing backup files under the usual
names, it writes the backup file as `%backup%~' in your home
directory. Only one such file can exist, so only the most recently
made such backup is available.

If you choose to have a series of numbered backup files, backup file
names contain `` `.~' ``, the number, and another `` `~' `` after the
original file name. Thus, the backup files of `eval.c' would be
called `eval.c.~1~', `eval.c.~2~', and so on, all the way
through names like `eval.c.~259~' and beyond. The variable
`backup-directory-alist` applies to numbered backups just as
usual.

The choice of single backup or numbered backups is controlled by the
variable `version-control`. Its possible values are

`t`: Make numbered backups. `nil`: Make numbered backups for files that have numbered backups already. Otherwise, make single backups. `never`: Never make numbered backups; always make single backups.

You can set `version-control` locally in an individual buffer to
control the making of backups for that buffer's file. For example,
Rmail mode locally sets `version-control` to `never` to make sure
that there is only one backup for an Rmail file. See section [AD.2.4 Local Variables](emacs.html#SEC485).

If you set the environment variable `VERSION_CONTROL`, to tell
various GNU utilities what to do with backup files, Emacs also obeys the
environment variable by setting the Lisp variable `version-control`
accordingly at startup. If the environment variable's value is `` `t' ``
or `` `numbered' ``, then `version-control` becomes `t`; if the
value is `` `nil' `` or `` `existing' ``, then `version-control`
becomes `nil`; if it is `` `never' `` or `` `simple' ``, then
`version-control` becomes `never`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC115) ] | [ [>](emacs.html#SEC117) ] | [ [<<](emacs.html#SEC117) ] | [ [Up](emacs.html#SEC114) ] | [ [>>](emacs.html#SEC118) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.3.1.2 Automatic Deletion of Backups

To prevent excessive consumption of disk space, Emacs can delete numbered
backup versions automatically. Generally Emacs keeps the first few backups
and the latest few backups, deleting any in between. This happens every
time a new backup is made.

The two variables `kept-old-versions` and
`kept-new-versions` control this deletion. Their values are,
respectively, the number of oldest (lowest-numbered) backups to keep
and the number of newest (highest-numbered) ones to keep, each time a
new backup is made. The backups in the middle (excluding those oldest
and newest) are the excess middle versions--those backups are
deleted. These variables' values are used when it is time to delete
excess versions, just after a new backup version is made; the newly
made backup is included in the count in `kept-new-versions`. By
default, both variables are 2.

If `delete-old-versions` is non-`nil`, Emacs deletes the
excess backup files silently. If it is `nil`, the default, Emacs
asks you whether it should delete the excess backup versions.

Dired's `.` (Period) command can also be used to delete old versions.
See section [AB.3 Deleting Files with Dired](emacs.html#SEC399).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC116) ] | [ [>](emacs.html#SEC118) ] | [ [<<](emacs.html#SEC121) ] | [ [Up](emacs.html#SEC114) ] | [ [>>](emacs.html#SEC118) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.3.1.3 Copying vs. Renaming

Backup files can be made by copying the old file or by renaming it.
This makes a difference when the old file has multiple names (hard
links). If the old file is renamed into the backup file, then the
alternate names become names for the backup file. If the old file is
copied instead, then the alternate names remain names for the file
that you are editing, and the contents accessed by those names will be
the new contents.

The method of making a backup file may also affect the file's owner
and group. If copying is used, these do not change. If renaming is used,
you become the file's owner, and the file's group becomes the default
(different operating systems have different defaults for the group).

Having the owner change is usually a good idea, because then the owner
always shows who last edited the file. Also, the owners of the backups
show who produced those versions. Occasionally there is a file whose
owner should not change; it is a good idea for such files to contain
local variable lists to set `backup-by-copying-when-mismatch`
locally (see section [AD.2.5 Local Variables in Files](emacs.html#SEC486)).

The choice of renaming or copying is controlled by four variables.
Renaming is the default choice. If the variable
`backup-by-copying` is non-`nil`, copying is used. Otherwise,
if the variable `backup-by-copying-when-linked` is non-`nil`,
then copying is used for files that have multiple names, but renaming
may still be used when the file being edited has only one name. If the
variable `backup-by-copying-when-mismatch` is non-`nil`, then
copying is used if renaming would cause the file's owner or group to
change. `backup-by-copying-when-mismatch` is `t` by default
if you start Emacs as the superuser. The fourth variable,
`backup-by-copying-when-privileged-mismatch`, gives the highest
numeric user-id for which `backup-by-copying-when-mismatch` will be
forced on. This is useful when low-numbered user-ids are assigned to
special system users, such as `root`, `bin`, `daemon`,
etc., which must maintain ownership of files.

When a file is managed with a version control system (see section [M.7 Version Control](emacs.html#SEC127)), Emacs does not normally make backups in the usual way for
that file. But check-in and check-out are similar in some ways to
making backups. One unfortunate similarity is that these operations
typically break hard links, disconnecting the file name you visited from
any alternate names for the same file. This has nothing to do with
Emacs--the version control system does it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC117) ] | [ [>](emacs.html#SEC119) ] | [ [<<](emacs.html#SEC119) ] | [ [Up](emacs.html#SEC113) ] | [ [>>](emacs.html#SEC121) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.3.2 Protection against Simultaneous Editing

Simultaneous editing occurs when two users visit the same file, both
make changes, and then both save them. If nobody were informed that
this was happening, whichever user saved first would later find that his
changes were lost.

On some systems, Emacs notices immediately when the second user starts
to change the file, and issues an immediate warning. On all systems,
Emacs checks when you save the file, and warns if you are about to
overwrite another user's changes. You can prevent loss of the other
user's work by taking the proper corrective action instead of saving the
file.

When you make the first modification in an Emacs buffer that is
visiting a file, Emacs records that the file is *locked* by you.
(It does this by creating a symbolic link in the same directory with a
different name.) Emacs removes the lock when you save the changes. The
idea is that the file is locked whenever an Emacs buffer visiting it has
unsaved changes.

If you begin to modify the buffer while the visited file is locked by
someone else, this constitutes a *collision*. When Emacs detects a
collision, it asks you what to do, by calling the Lisp function
`ask-user-about-lock`. You can redefine this function for the sake
of customization. The standard definition of this function asks you a
question and accepts three possible answers:

`s`: Steal the lock. Whoever was already changing the file loses the lock, and you gain the lock. `p`: Proceed. Go ahead and edit the file despite its being locked by someone else. `q`: Quit. This causes an error (`file-locked`), and the buffer contents remain unchanged--the modification you were trying to make does not actually take place.

Note that locking works on the basis of a file name; if a file has
multiple names, Emacs does not realize that the two names are the same file
and cannot prevent two users from editing it simultaneously under different
names. However, basing locking on names means that Emacs can interlock the
editing of new files that will not really exist until they are saved.

Some systems are not configured to allow Emacs to make locks, and
there are cases where lock files cannot be written. In these cases,
Emacs cannot detect trouble in advance, but it still can detect the
collision when you try to save a file and overwrite someone else's
changes.

If Emacs or the operating system crashes, this may leave behind lock
files which are stale, so you may occasionally get warnings about
spurious collisions. When you determine that the collision is spurious,
just use `p` to tell Emacs to go ahead anyway.

Every time Emacs saves a buffer, it first checks the last-modification
date of the existing file on disk to verify that it has not changed since the
file was last visited or saved. If the date does not match, it implies
that changes were made in the file in some other way, and these changes are
about to be lost if Emacs actually does save. To prevent this, Emacs
displays a warning message and asks for confirmation before saving.
Occasionally you will know why the file was changed and know that it does
not matter; then you can answer `yes` and proceed. Otherwise, you should
cancel the save with `C-g` and investigate the situation.

The first thing you should do when notified that simultaneous editing
has already taken place is to list the directory with `C-u C-x C-d`
(see section [M.8 File Directories](emacs.html#SEC163)). This shows the file's current author. You
should attempt to contact him to warn him not to continue editing.
Often the next step is to save the contents of your Emacs buffer under a
different name, and use `diff` to compare the two files.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC118) ] | [ [>](emacs.html#SEC120) ] | [ [<<](emacs.html#SEC120) ] | [ [Up](emacs.html#SEC113) ] | [ [>>](emacs.html#SEC121) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.3.3 Shadowing Files

`M-x shadow-initialize`: Set up file shadowing. `M-x shadow-define-literal-group`: Declare a single file to be shared between sites. `M-x shadow-define-regexp-group`: Make all files that match each of a group of files be shared between hosts. `M-x shadow-define-cluster RET name RET`: Define a shadow file cluster name. `M-x shadow-copy-files`: Copy all pending shadow files. `M-x shadow-cancel`: Cancel the instruction to shadow some files.

You can arrange to keep identical *shadow* copies of certain files
in more than one place--possibly on different machines. To do this,
first you must set up a *shadow file group*, which is a set of
identically-named files shared between a list of sites. The file
group is permanent and applies to further Emacs sessions as well as
the current one. Once the group is set up, every time you exit Emacs,
it will copy the file you edited to the other files in its group. You
can also do the copying without exiting Emacs, by typing `M-x
shadow-copy-files`.

To set up a shadow file group, use `M-x
shadow-define-literal-group` or `M-x shadow-define-regexp-group`.
See their documentation strings for further information.

Before copying a file to its shadows, Emacs asks for confirmation.
You can answer "no" to bypass copying of this file, this time. If
you want to cancel the shadowing permanently for a certain file, use
`M-x shadow-cancel` to eliminate or change the shadow file group.

A *shadow cluster* is a group of hosts that share directories, so
that copying to or from one of them is sufficient to update the file
on all of them. Each shadow cluster has a name, and specifies the
network address of a primary host (the one we copy files to), and a
regular expression that matches the host names of all the other hosts
in the cluster. You can define a shadow cluster with `M-x
shadow-define-cluster`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC119) ] | [ [>](emacs.html#SEC121) ] | [ [<<](emacs.html#SEC121) ] | [ [Up](emacs.html#SEC113) ] | [ [>>](emacs.html#SEC121) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.3.4 Updating Time Stamps Automatically

You can arrange to put a time stamp in a file, so that it will be updated
automatically each time you edit and save the file. The time stamp
has to be in the first eight lines of the file, and you should
insert it like this:

|  |  |
| --- | --- |
|  | ``` Time-stamp: <> ``` |

or like this:

|  |  |
| --- | --- |
|  | ``` Time-stamp: "" ``` |

Then add the hook function `time-stamp` to the hook
`write-file-hooks`; that hook function will automatically update
the time stamp, inserting the current date and time when you save the
file. You can also use the command `M-x time-stamp` to update the
time stamp manually. For other customizations, see the Custom group
`time-stamp`. Note that non-numeric fields in the time stamp are
formatted according to your locale setting (see section [AE.5 Environment Variables](emacs.html#SEC533)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC120) ] | [ [>](emacs.html#SEC122) ] | [ [<<](emacs.html#SEC122) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.4 Reverting a Buffer

If you have made extensive changes to a file and then change your mind
about them, you can get rid of them by reading in the previous version
of the file. To do this, use `M-x revert-buffer`, which operates on
the current buffer. Since reverting a buffer unintentionally could lose
a lot of work, you must confirm this command with `yes`.

`revert-buffer` keeps point at the same distance (measured in
characters) from the beginning of the file. If the file was edited only
slightly, you will be at approximately the same piece of text after
reverting as before. If you have made drastic changes, the same value of
point in the old file may address a totally different piece of text.

Reverting marks the buffer as "not modified" until another change is
made.

Some kinds of buffers whose contents reflect data bases other than files,
such as Dired buffers, can also be reverted. For them, reverting means
recalculating their contents from the appropriate data base. Buffers
created explicitly with `C-x b` cannot be reverted; `revert-buffer`
reports an error when asked to do so.

When you edit a file that changes automatically and frequently--for
example, a log of output from a process that continues to run--it may be
useful for Emacs to revert the file without querying you, whenever you
visit the file again with `C-x C-f`.

To request this behavior, set the variable `revert-without-query`
to a list of regular expressions. When a file name matches one of these
regular expressions, `find-file` and `revert-buffer` will
revert it automatically if it has changed--provided the buffer itself
is not modified. (If you have edited the text, it would be wrong to
discard your changes.)

You may find it useful to have Emacs revert files automatically when
they change. Two minor modes are available to do this. In Global
Auto-Revert mode, Emacs periodically checks all file buffers and
reverts any when the corresponding file has changed. The local
variant, Auto-Revert mode, applies only to buffers in which it was
activated. Checking the files is done at intervals determined by the
variable `auto-revert-interval`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC121) ] | [ [>](emacs.html#SEC123) ] | [ [<<](emacs.html#SEC126) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC126) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.5 Auto-Saving: Protection Against Disasters

Emacs saves all the visited files from time to time (based on counting
your keystrokes) without being asked. This is called *auto-saving*.
It prevents you from losing more than a limited amount of work if the
system crashes.

When Emacs determines that it is time for auto-saving, each buffer is
considered, and is auto-saved if auto-saving is turned on for it and it
has been changed since the last time it was auto-saved. The message
`` `Auto-saving...' `` is displayed in the echo area during auto-saving,
if any files are actually auto-saved. Errors occurring during
auto-saving are caught so that they do not interfere with the execution
of commands you have been typing.

> |  |  |  |
> | --- | --- | --- |
> | [M.5.1 Auto-Save Files](emacs.html#SEC123) |  | The file where auto-saved changes are actually made until you save the file. |
> | [M.5.2 Controlling Auto-Saving](emacs.html#SEC124) |  | Controlling when and how often to auto-save. |
> | [M.5.3 Recovering Data from Auto-Saves](emacs.html#SEC125) |  | Recovering text from auto-save files. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC122) ] | [ [>](emacs.html#SEC124) ] | [ [<<](emacs.html#SEC126) ] | [ [Up](emacs.html#SEC122) ] | [ [>>](emacs.html#SEC126) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.5.1 Auto-Save Files

Auto-saving does not normally save in the files that you visited, because
it can be very undesirable to save a program that is in an inconsistent
state when you have made half of a planned change. Instead, auto-saving
is done in a different file called the *auto-save file*, and the
visited file is changed only when you request saving explicitly (such as
with `C-x C-s`).

Normally, the auto-save file name is made by appending `` `#' `` to the
front and rear of the visited file name. Thus, a buffer visiting file
`foo.c' is auto-saved in a file `#foo.c#'. Most buffers that
are not visiting files are auto-saved only if you request it explicitly;
when they are auto-saved, the auto-save file name is made by appending
`` `#%' `` to the front and `` `#' `` to the rear of buffer name. For
example, the `` `*mail*' `` buffer in which you compose messages to be
sent is auto-saved in a file named `#%\*mail\*#'. Auto-save file
names are made this way unless you reprogram parts of Emacs to do
something different (the functions `make-auto-save-file-name` and
`auto-save-file-name-p`). The file name to be used for auto-saving
in a buffer is calculated when auto-saving is turned on in that buffer.

When you delete a substantial part of the text in a large buffer, auto
save turns off temporarily in that buffer. This is because if you
deleted the text unintentionally, you might find the auto-save file more
useful if it contains the deleted text. To reenable auto-saving after
this happens, save the buffer with `C-x C-s`, or use `C-u 1 M-x
auto-save`.

If you want auto-saving to be done in the visited file rather than
in a separate auto-save file, set the variable
`auto-save-visited-file-name` to a non-`nil` value. In this
mode, there is no real difference between auto-saving and explicit
saving.

A buffer's auto-save file is deleted when you save the buffer in its
visited file. To inhibit this, set the variable `delete-auto-save-files`
to `nil`. Changing the visited file name with `C-x C-w` or
`set-visited-file-name` renames any auto-save file to go with
the new visited name.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC123) ] | [ [>](emacs.html#SEC125) ] | [ [<<](emacs.html#SEC125) ] | [ [Up](emacs.html#SEC122) ] | [ [>>](emacs.html#SEC126) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.5.2 Controlling Auto-Saving

Each time you visit a file, auto-saving is turned on for that file's
buffer if the variable `auto-save-default` is non-`nil` (but not
in batch mode; see section [C. Entering and Exiting Emacs](emacs.html#SEC17)). The default for this variable is
`t`, so auto-saving is the usual practice for file-visiting buffers.
Auto-saving can be turned on or off for any existing buffer with the
command `M-x auto-save-mode`. Like other minor mode commands, `M-x
auto-save-mode` turns auto-saving on with a positive argument, off with a
zero or negative argument; with no argument, it toggles.

Emacs does auto-saving periodically based on counting how many characters
you have typed since the last time auto-saving was done. The variable
`auto-save-interval` specifies how many characters there are between
auto-saves. By default, it is 300.

Auto-saving also takes place when you stop typing for a while. The
variable `auto-save-timeout` says how many seconds Emacs should
wait before it does an auto save (and perhaps also a garbage
collection). (The actual time period is longer if the current buffer is
long; this is a heuristic which aims to keep out of your way when you
are editing long buffers, in which auto-save takes an appreciable amount
of time.) Auto-saving during idle periods accomplishes two things:
first, it makes sure all your work is saved if you go away from the
terminal for a while; second, it may avoid some auto-saving while you
are actually typing.

Emacs also does auto-saving whenever it gets a fatal error. This
includes killing the Emacs job with a shell command such as `` `kill
%emacs' ``, or disconnecting a phone line or network connection.

You can request an auto-save explicitly with the command `M-x
do-auto-save`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC124) ] | [ [>](emacs.html#SEC126) ] | [ [<<](emacs.html#SEC126) ] | [ [Up](emacs.html#SEC122) ] | [ [>>](emacs.html#SEC126) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.5.3 Recovering Data from Auto-Saves

You can use the contents of an auto-save file to recover from a loss
of data with the command `M-x recover-file RET file
RET`. This visits file and then (after your confirmation)
restores the contents from its auto-save file `#file#'.
You can then save with `C-x C-s` to put the recovered text into
file itself. For example, to recover file `foo.c' from its
auto-save file `#foo.c#', do:

|  |  |
| --- | --- |
|  | ``` M-x recover-file RET foo.c RET yes RET C-x C-s ``` |

Before asking for confirmation, `M-x recover-file` displays a
directory listing describing the specified file and the auto-save file,
so you can compare their sizes and dates. If the auto-save file
is older, `M-x recover-file` does not offer to read it.

If Emacs or the computer crashes, you can recover all the files you
were editing from their auto save files with the command `M-x
recover-session`. This first shows you a list of recorded interrupted
sessions. Move point to the one you choose, and type `C-c C-c`.

Then `recover-session` asks about each of the files that were
being edited during that session, asking whether to recover that file.
If you answer `y`, it calls `recover-file`, which works in its
normal fashion. It shows the dates of the original file and its
auto-save file, and asks once again whether to recover that file.

When `recover-session` is done, the files you've chosen to
recover are present in Emacs buffers. You should then save them. Only
this--saving them--updates the files themselves.

Emacs records interrupted sessions for later recovery in files named
`~/.emacs.d/auto-save-list/.saves-pid-hostname'. The
`` `~/.emacs.d/auto-save-list/.saves-' `` portion of these names comes
from the value of `auto-save-list-file-prefix`. You can record
sessions in a different place by customizing that variable. If you
set `auto-save-list-file-prefix` to `nil` in your
`.emacs' file, sessions are not recorded for recovery.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC125) ] | [ [>](emacs.html#SEC127) ] | [ [<<](emacs.html#SEC127) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.6 File Name Aliases

Symbolic links and hard links both make it possible for several file
names to refer to the same file. Hard links are alternate names that
refer directly to the file; all the names are equally valid, and no one
of them is preferred. By contrast, a symbolic link is a kind of defined
alias: when `foo' is a symbolic link to `bar', you can use
either name to refer to the file, but `bar' is the real name, while
`foo' is just an alias. More complex cases occur when symbolic
links point to directories.

If you visit two names for the same file, normally Emacs makes
two different buffers, but it warns you about the situation.

Normally, if you visit a file which Emacs is already visiting under
a different name, Emacs displays a message in the echo area and uses
the existing buffer visiting that file. This can happen on systems
that support symbolic links, or if you use a long file name on a
system that truncates long file names. You can suppress the message by
setting the variable `find-file-suppress-same-file-warnings` to a
non-`nil` value. You can disable this feature entirely by setting
the variable `find-file-existing-other-name` to `nil`: then
if you visit the same file under two different names, you get a separate
buffer for each file name.

If the variable `find-file-visit-truename` is non-`nil`,
then the file name recorded for a buffer is the file's *truename*
(made by replacing all symbolic links with their target names), rather
than the name you specify. Setting `find-file-visit-truename` also
implies the effect of `find-file-existing-other-name`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC126) ] | [ [>](emacs.html#SEC128) ] | [ [<<](emacs.html#SEC163) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC163) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.7 Version Control

*Version control systems* are packages that can record multiple
versions of a source file, usually storing the unchanged parts of the
file just once. Version control systems also record history information
such as the creation time of each version, who created it, and a
description of what was changed in that version.

The Emacs version control interface is called VC. Its commands work
with three version control systems--RCS, CVS, and SCCS. The GNU
project recommends RCS and CVS, which are free software and available
from the Free Software Foundation. We also have free software to
replace SCCS, known as CSSC; if you are using SCCS and don't want to
make the incompatible change to RCS or CVS, you can switch to CSSC.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.1 Introduction to Version Control](emacs.html#SEC128) |  | How version control works in general. |
> | [M.7.2 Version Control and the Mode Line](emacs.html#SEC131) |  | How the mode line shows version control status. |
> | [M.7.3 Basic Editing under Version Control](emacs.html#SEC132) |  | How to edit a file under version control. |
> | [M.7.4 Examining And Comparing Old Versions](emacs.html#SEC137) |  | Examining and comparing old versions. |
> | [M.7.5 The Secondary Commands of VC](emacs.html#SEC138) |  | The commands used a little less frequently. |
> | [M.7.6 Multiple Branches of a File](emacs.html#SEC144) |  | Multiple lines of development. |
> | [M.7.7 Remote Repositories](emacs.html#SEC149) |  | Efficient access to remote CVS servers. |
> | [M.7.8 Snapshots](emacs.html#SEC152) |  | Sets of file versions treated as a unit. |
> | [M.7.9 Miscellaneous Commands and Features of VC](emacs.html#SEC155) |  | Various other commands and features of VC. |
> | [M.7.10 Customizing VC](emacs.html#SEC159) |  | Variables that change VC's behavior. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC127) ] | [ [>](emacs.html#SEC129) ] | [ [<<](emacs.html#SEC163) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC131) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.1 Introduction to Version Control

VC allows you to use a version control system from within Emacs,
integrating the version control operations smoothly with editing. VC
provides a uniform interface to version control, so that regardless of
which version control system is in use, you can use it the same way.

This section provides a general overview of version control, and
describes the version control systems that VC supports. You can skip
this section if you are already familiar with the version control system
you want to use.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.1.1 Supported Version Control Systems](emacs.html#SEC129) |  | Supported version control back-end systems. |
> | [M.7.1.2 Concepts of Version Control](emacs.html#SEC130) |  | Words and concepts related to version control. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC128) ] | [ [>](emacs.html#SEC130) ] | [ [<<](emacs.html#SEC163) ] | [ [Up](emacs.html#SEC128) ] | [ [>>](emacs.html#SEC131) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.1.1 Supported Version Control Systems

VC currently works with three different version control systems or
"back ends": RCS, CVS, and SCCS.

RCS is a free version control system that is available from the Free
Software Foundation. It is perhaps the most mature of the supported
back ends, and the VC commands are conceptually closest to RCS. Almost
everything you can do with RCS can be done through VC.

CVS is built on top of RCS, and extends the features of RCS, allowing
for more sophisticated release management, and concurrent multi-user
development. VC supports basic editing operations under CVS, but for
some less common tasks you still need to call CVS from the command line.
Note also that before using CVS you must set up a repository, which is a
subject too complex to treat here.

SCCS is a proprietary but widely used version control system. In
terms of capabilities, it is the weakest of the three that VC
supports. VC compensates for certain features missing in SCCS
(snapshots, for example) by implementing them itself, but some other VC
features, such as multiple branches, are not available with SCCS. You
should use SCCS only if for some reason you cannot use RCS.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC129) ] | [ [>](emacs.html#SEC131) ] | [ [<<](emacs.html#SEC163) ] | [ [Up](emacs.html#SEC128) ] | [ [>>](emacs.html#SEC131) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.1.2 Concepts of Version Control

When a file is under version control, we also say that it is
*registered* in the version control system. Each registered file
has a corresponding *master file* which represents the file's
present state plus its change history--enough to reconstruct the
current version or any earlier version. Usually the master file also
records a *log entry* for each version, describing in words what was
changed in that version.

The file that is maintained under version control is sometimes called
the *work file* corresponding to its master file. You edit the work
file and make changes in it, as you would with an ordinary file. (With
SCCS and RCS, you must *lock* the file before you start to edit it.)
After you are done with a set of changes, you *check the file in*,
which records the changes in the master file, along with a log entry for
them.

With CVS, there are usually multiple work files corresponding to a
single master file--often each user has his own copy. It is also
possible to use RCS in this way, but this is not the usual way to use
RCS.

A version control system typically has some mechanism to coordinate
between users who want to change the same file. One method is
*locking* (analogous to the locking that Emacs uses to detect
simultaneous editing of a file, but distinct from it). The other method
is to merge your changes with other people's changes when you check them
in.

With version control locking, work files are normally read-only so
that you cannot change them. You ask the version control system to make
a work file writable for you by locking it; only one user can do
this at any given time. When you check in your changes, that unlocks
the file, making the work file read-only again. This allows other users
to lock the file to make further changes. SCCS always uses locking, and
RCS normally does.

The other alternative for RCS is to let each user modify the work file
at any time. In this mode, locking is not required, but it is
permitted; check-in is still the way to record a new version.

CVS normally allows each user to modify his own copy of the work file
at any time, but requires merging with changes from other users at
check-in time. However, CVS can also be set up to require locking.
(see section [M.7.10.3 Options specific for CVS](emacs.html#SEC162)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC130) ] | [ [>](emacs.html#SEC132) ] | [ [<<](emacs.html#SEC132) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC163) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.2 Version Control and the Mode Line

When you visit a file that is under version control, Emacs indicates
this on the mode line. For example, `` `RCS-1.3' `` says that RCS is
used for that file, and the current version is 1.3.

The character between the back-end name and the version number
indicates the version control status of the file. `` `-' `` means that
the work file is not locked (if locking is in use), or not modified (if
locking is not in use). `` `:' `` indicates that the file is locked, or
that it is modified. If the file is locked by some other user (for
instance, `` `jim' ``), that is displayed as `` `RCS:jim:1.3' ``.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC131) ] | [ [>](emacs.html#SEC133) ] | [ [<<](emacs.html#SEC137) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC137) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.3 Basic Editing under Version Control

The principal VC command is an all-purpose command that performs
either locking or check-in, depending on the situation.

`C-x C-q`: `C-x v v`: Perform the next logical version control operation on this file.

Strictly speaking, the command for this job is `vc-next-action`,
bound to `C-x v v`. However, the normal meaning of `C-x C-q` is
to make a read-only buffer writable, or vice versa; we have extended it
to do the same job properly for files managed by version control, by
performing the appropriate version control operations. When you type
`C-x C-q` on a registered file, it acts like `C-x v v`.

The precise action of this command depends on the state of the file,
and whether the version control system uses locking or not. SCCS and
RCS normally use locking; CVS normally does not use locking.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.3.1 Basic Version Control with Locking](emacs.html#SEC133) |  | RCS in its default mode, SCCS, and optionally CVS. |
> | [M.7.3.2 Basic Version Control without Locking](emacs.html#SEC134) |  | Without locking: default mode for CVS. |
> | [M.7.3.3 Advanced Control in `C-x C-q`](emacs.html#SEC135) |  | Advanced features available with a prefix argument. |
> | [M.7.3.4 Features of the Log Entry Buffer](emacs.html#SEC136) |  | Features available in log entry buffers. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC132) ] | [ [>](emacs.html#SEC134) ] | [ [<<](emacs.html#SEC137) ] | [ [Up](emacs.html#SEC132) ] | [ [>>](emacs.html#SEC137) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.3.1 Basic Version Control with Locking

If locking is used for the file (as with SCCS, and RCS in its default
mode), `C-x C-q` can either lock a file or check it in:

* If the file is not locked, `C-x C-q` locks it, and
  makes it writable so that you can change it.

  * If the file is locked by you, and contains changes, `C-x C-q` checks
    in the changes. In order to do this, it first reads the log entry
    for the new version. See section [M.7.3.4 Features of the Log Entry Buffer](emacs.html#SEC136).

    * If the file is locked by you, but you have not changed it since you
      locked it, `C-x C-q` releases the lock and makes the file read-only
      again.

      * If the file is locked by some other user, `C-x C-q` asks you whether
        you want to "steal the lock" from that user. If you say yes, the file
        becomes locked by you, but a message is sent to the person who had
        formerly locked the file, to inform him of what has happened.

These rules also apply when you use CVS in locking mode, except
that there is no such thing as stealing a lock.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC133) ] | [ [>](emacs.html#SEC135) ] | [ [<<](emacs.html#SEC135) ] | [ [Up](emacs.html#SEC132) ] | [ [>>](emacs.html#SEC137) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.3.2 Basic Version Control without Locking

When there is no locking--the default for CVS--work files are always
writable; you do not need to do anything before you begin to edit a
file. The status indicator on the mode line is `` `-' `` if the file is
unmodified; it flips to `` `:' `` as soon as you save any changes in the
work file.

Here is what `C-x C-q` does when using CVS:

* If some other user has checked in changes into the master file, Emacs
  asks you whether you want to merge those changes into your own work
  file. You must do this before you can check in your own changes. (To
  pick up any recent changes from the master file *without* trying
  to commit your own changes, type `C-x v m RET`.)
  See section [M.7.6.3 Merging Branches](emacs.html#SEC147).

  * If there are no new changes in the master file, but you have made
    modifications in your work file, `C-x C-q` checks in your changes.
    In order to do this, it first reads the log entry for the new version.
    See section [M.7.3.4 Features of the Log Entry Buffer](emacs.html#SEC136).

    * If the file is not modified, the `C-x C-q` does nothing.

These rules also apply when you use RCS in the mode that does not
require locking, except that automatic merging of changes from the
master file is not implemented. Unfortunately, this means that nothing
informs you if another user has checked in changes in the same file
since you began editing it, and when this happens, his changes will be
effectively removed when you check in your version (though they will
remain in the master file, so they will not be entirely lost). You must
therefore verify the current version is unchanged, before you check in your
changes. We hope to eliminate this risk and provide automatic merging
with RCS in a future Emacs version.

In addition, locking is possible with RCS even in this mode, although
it is not required; `C-x C-q` with an unmodified file locks the
file, just as it does with RCS in its normal (locking) mode.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC134) ] | [ [>](emacs.html#SEC136) ] | [ [<<](emacs.html#SEC136) ] | [ [Up](emacs.html#SEC132) ] | [ [>>](emacs.html#SEC137) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.3.3 Advanced Control in `C-x C-q`

When you give a prefix argument to `vc-next-action` (`C-u
C-x C-q`), it still performs the next logical version control
operation, but accepts additional arguments to specify precisely how
to do the operation.

* If the file is modified (or locked), you can specify the version
  number to use for the new version that you check in. This is one way
  to create a new branch (see section [M.7.6 Multiple Branches of a File](emacs.html#SEC144)).

  * If the file is not modified (and unlocked), you can specify the
    version to select; this lets you start working from an older version,
    or on another branch. If you do not enter any version, that takes you
    to the highest version on the current branch; therefore `C-u C-x
    C-q RET` is a convenient way to get the latest version of a file from
    the repository.

    * Instead of the version number, you can also specify the name of a
      version control system. This is useful when one file is being managed
      with two version control systems at the same time (see section [M.7.7.2 Local Version Control](emacs.html#SEC151)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC135) ] | [ [>](emacs.html#SEC137) ] | [ [<<](emacs.html#SEC137) ] | [ [Up](emacs.html#SEC132) ] | [ [>>](emacs.html#SEC137) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.3.4 Features of the Log Entry Buffer

When you check in changes, `C-x C-q` first reads a log entry. It
pops up a buffer called `` `*VC-Log*' `` for you to enter the log entry.
When you are finished, type `C-c C-c` in the `` `*VC-Log*' `` buffer.
That is when check-in really happens.

To abort check-in, just **don't** type `C-c C-c` in that
buffer. You can switch buffers and do other editing. As long as you
don't try to check in another file, the entry you were editing remains
in the `` `*VC-Log*' `` buffer, and you can go back to that buffer at any
time to complete the check-in.

If you change several source files for the same reason, it is often
convenient to specify the same log entry for many of the files. To do
this, use the history of previous log entries. The commands `M-n`,
`M-p`, `M-s` and `M-r` for doing this work just like the
minibuffer history commands (except that these versions are used outside
the minibuffer).

Each time you check in a file, the log entry buffer is put into VC Log
mode, which involves running two hooks: `text-mode-hook` and
`vc-log-mode-hook`. See section [AD.2.3 Hooks](emacs.html#SEC484).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC136) ] | [ [>](emacs.html#SEC138) ] | [ [<<](emacs.html#SEC138) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC163) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.4 Examining And Comparing Old Versions

One of the convenient features of version control is the ability
to examine any version of a file, or compare two versions.

`C-x v ~ version RET`: Examine version version of the visited file, in a buffer of its own. `C-x v =`: Compare the current buffer contents with the latest checked-in version of the file. `C-u C-x v = file RET oldvers RET newvers RET`: Compare the specified two versions of file. `C-x v g`: Display the result of the CVS annotate command using colors.

To examine an old version in its entirety, visit the file and then type
`C-x v ~ version RET` (`vc-version-other-window`).
This puts the text of version version in a file named
`filename.~version~', and visits it in its own buffer
in a separate window. (In RCS, you can also select an old version
and create a branch from it. See section [M.7.6 Multiple Branches of a File](emacs.html#SEC144).)

It is usually more convenient to compare two versions of the file,
with the command `C-x v =` (`vc-diff`). Plain `C-x v =`
compares the current buffer contents (saving them in the file if
necessary) with the last checked-in version of the file. `C-u C-x
v =`, with a numeric argument, reads a file name and two version
numbers, then compares those versions of the specified file. Both
forms display the output in a special buffer in another window.

You can specify a checked-in version by its number; an empty input
specifies the current contents of the work file (which may be different
from all the checked-in versions). You can also specify a snapshot name
(see section [M.7.8 Snapshots](emacs.html#SEC152)) instead of one or both version numbers.

If you supply a directory name instead of the name of a registered
file, this command compares the two specified versions of all registered
files in that directory and its subdirectories.

`C-x v =` works by running a variant of the `diff` utility
designed to work with the version control system in use. When you
invoke `diff` this way, in addition to the options specified by
`diff-switches` (see section [M.9 Comparing Files](emacs.html#SEC164)), it receives those
specified by `vc-diff-switches`, plus those specified for the
specific back end by `vc-backend-diff-switches`. For
instance, when the version control back end is RCS, `diff` uses
the options in `vc-rcs-diff-switches`. The
`` `vc...diff-switches' `` variables are `nil` by default.

Unlike the `M-x diff` command, `C-x v =` does not try to
locate the changes in the old and new versions. This is because
normally one or both versions do not exist as files when you compare
them; they exist only in the records of the master file.
See section [M.9 Comparing Files](emacs.html#SEC164), for more information about `M-x diff`.

For CVS-controlled files, you can display the result of the CVS
annotate command, using colors to enhance the visual appearance. Use
the command `M-x vc-annotate` to do this. It creates a new buffer
to display file's text, colored to show how old each part is. Text
colored red is new, blue means old, and intermediate colors indicate
intermediate ages. By default, the time scale is 360 days, so that
everything more than one year old is shown in blue.

When you give a prefix argument to this command, it uses the
minibuffer to read two arguments: which version number to display and
annotate (instead of the current file contents), and a stretch factor
for the time scale. A stretch factor of 0.1 means that the color
range from red to blue spans the past 36 days instead of 360 days. A
stretch factor greater than 1 means the color range spans more than a
year.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC137) ] | [ [>](emacs.html#SEC139) ] | [ [<<](emacs.html#SEC144) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC144) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.5 The Secondary Commands of VC

This section explains the secondary commands of VC; those that you might
use once a day.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.5.1 Registering a File for Version Control](emacs.html#SEC139) |  | Putting a file under version control. |
> | [M.7.5.2 VC Status Commands](emacs.html#SEC140) |  | Viewing the VC status of files. |
> | [M.7.5.3 Undoing Version Control Actions](emacs.html#SEC141) |  | Cancelling changes before or after check-in. |
> | [M.7.5.4 Dired under VC](emacs.html#SEC142) |  | Listing files managed by version control. |
> | [M.7.5.5 VC Dired Commands](emacs.html#SEC143) |  | Commands to use in a VC Dired buffer. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC138) ] | [ [>](emacs.html#SEC140) ] | [ [<<](emacs.html#SEC144) ] | [ [Up](emacs.html#SEC138) ] | [ [>>](emacs.html#SEC144) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.5.1 Registering a File for Version Control

You can put any file under version control by simply visiting it, and
then typing `C-x v i` (`vc-register`).

`C-x v i`: Register the visited file for version control.

To register the file, Emacs must choose which version control system
to use for it. If the file's directory already contains files
registered in a version control system, Emacs uses that system. If
there is more than one system in use for a directory, Emacs uses the one
that appears first in `vc-handled-backends` (see section [M.7.10 Customizing VC](emacs.html#SEC159)).
On the other hand, if there are no files already registered,
Emacs uses the first system from `vc-handled-backends` that could
register the file--for example, you cannot register a file under CVS if
its directory is not already part of a CVS tree.

With the default value of `vc-handled-backends`, this means
that Emacs uses RCS if there are any files under RCS control, CVS if
there are any files under CVS, SCCS if any files are under SCCS, or
RCS as the ultimate default.

If locking is in use, `C-x v i` leaves the file unlocked and
read-only. Type `C-x C-q` if you wish to start editing it. After
registering a file with CVS, you must subsequently commit the initial
version by typing `C-x C-q`.

The initial version number for a newly registered file is 1.1, by
default. You can specify a different default by setting the variable
`vc-default-init-version`, or you can give `C-x v i` a numeric
argument; then it reads the initial version number for this particular
file using the minibuffer.

If `vc-initial-comment` is non-`nil`, `C-x v i` reads an
initial comment to describe the purpose of this source file. Reading
the initial comment works like reading a log entry (see section [M.7.3.4 Features of the Log Entry Buffer](emacs.html#SEC136)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC139) ] | [ [>](emacs.html#SEC141) ] | [ [<<](emacs.html#SEC141) ] | [ [Up](emacs.html#SEC138) ] | [ [>>](emacs.html#SEC144) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.5.2 VC Status Commands

`C-x v l`: Display version control state and change history.

To view the detailed version control status and history of a file,
type `C-x v l` (`vc-print-log`). It displays the history of
changes to the current file, including the text of the log entries. The
output appears in a separate window.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC140) ] | [ [>](emacs.html#SEC142) ] | [ [<<](emacs.html#SEC142) ] | [ [Up](emacs.html#SEC138) ] | [ [>>](emacs.html#SEC144) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.5.3 Undoing Version Control Actions

`C-x v u`: Revert the buffer and the file to the last checked-in version. `C-x v c`: Remove the last-entered change from the master for the visited file. This undoes your last check-in.

If you want to discard your current set of changes and revert to the
last version checked in, use `C-x v u` (`vc-revert-buffer`).
This leaves the file unlocked; if locking is in use, you must first lock
the file again before you change it again. `C-x v u` requires
confirmation, unless it sees that you haven't made any changes since the
last checked-in version.

`C-x v u` is also the command to unlock a file if you lock it and
then decide not to change it.

To cancel a change that you already checked in, use `C-x v c`
(`vc-cancel-version`). This command discards all record of the
most recent checked-in version. `C-x v c` also offers to revert
your work file and buffer to the previous version (the one that precedes
the version that is deleted).

If you answer `no`, VC keeps your changes in the buffer, and locks
the file. The no-revert option is useful when you have checked in a
change and then discover a trivial error in it; you can cancel the
erroneous check-in, fix the error, and check the file in again.

When `C-x v c` does not revert the buffer, it unexpands all
version control headers in the buffer instead (see section [M.7.9.3 Inserting Version Control Headers](emacs.html#SEC158)).
This is because the buffer no longer corresponds to any existing
version. If you check it in again, the check-in process will expand the
headers properly for the new version number.

However, it is impossible to unexpand the RCS `` `$Log$' `` header
automatically. If you use that header feature, you have to unexpand it
by hand--by deleting the entry for the version that you just canceled.

Be careful when invoking `C-x v c`, as it is easy to lose a lot of
work with it. To help you be careful, this command always requires
confirmation with `yes`. Note also that this command is disabled
under CVS, because canceling versions is very dangerous and discouraged
with CVS.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC141) ] | [ [>](emacs.html#SEC143) ] | [ [<<](emacs.html#SEC143) ] | [ [Up](emacs.html#SEC138) ] | [ [>>](emacs.html#SEC144) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.5.4 Dired under VC

The VC Dired Mode described here works with all the version control
systems that VC supports. Another more powerful facility, designed
specifically for CVS, is called PCL-CVS. See section `About PCL-CVS' in PCL-CVS -- The Emacs Front-End to CVS.

When you are working on a large program, it is often useful to find
out which files have changed within an entire directory tree, or to view
the status of all files under version control at once, and to perform
version control operations on collections of files. You can use the
command `C-x v d` (`vc-directory`) to make a directory listing
that includes only files relevant for version control.

`C-x v d` creates a buffer which uses VC Dired Mode. This looks
much like an ordinary Dired buffer (see section [AB. Dired, the Directory Editor](emacs.html#SEC396)); however, normally it
shows only the noteworthy files (those locked or not up-to-date). This
is called *terse display*. If you set the variable
`vc-dired-terse-display` to `nil`, then VC Dired shows all
relevant files--those managed under version control, plus all
subdirectories (*full display*). The command `v t` in a VC
Dired buffer toggles between terse display and full display (see section [M.7.5.5 VC Dired Commands](emacs.html#SEC143)).

By default, VC Dired produces a recursive listing of noteworthy or
relevant files at or below the given directory. You can change this by
setting the variable `vc-dired-recurse` to `nil`; then VC
Dired shows only the files in the given directory.

The line for an individual file shows the version control state in the
place of the hard link count, owner, group, and size of the file. If
the file is unmodified, in sync with the master file, the version
control state shown is blank. Otherwise it consists of text in
parentheses. Under RCS and SCCS, the name of the user locking the file
is shown; under CVS, an abbreviated version of the `` `cvs status' ``
output is used. Here is an example using RCS:

|  |  |
| --- | --- |
|  | ```   /home/jim/project:    -rw-r--r-- (jim)      Apr  2 23:39 file1   -r--r--r--            Apr  5 20:21 file2 ``` |

The files `` `file1' `` and `` `file2' `` are under version control,
`` `file1' `` is locked by user jim, and `` `file2' `` is unlocked.

Here is an example using CVS:

|  |  |
| --- | --- |
|  | ```   /home/joe/develop:    -rw-r--r-- (modified) Aug  2  1997 file1.c   -rw-r--r--            Apr  4 20:09 file2.c   -rw-r--r-- (merge)    Sep 13  1996 file3.c ``` |

Here `` `file1.c' `` is modified with respect to the repository, and
`` `file2.c' `` is not. `` `file3.c' `` is modified, but other changes
have also been checked in to the repository--you need to merge them
with the work file before you can check it in.

When VC Dired displays subdirectories (in the "full" display mode),
it omits some that should never contain any files under version control.
By default, this includes Version Control subdirectories such as
`` `RCS' `` and `` `CVS' ``; you can customize this by setting the
variable `vc-directory-exclusion-list`.

You can fine-tune VC Dired's format by typing `C-u C-x v d`---as in
ordinary Dired, that allows you to specify additional switches for the
`` `ls' `` command.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC142) ] | [ [>](emacs.html#SEC144) ] | [ [<<](emacs.html#SEC144) ] | [ [Up](emacs.html#SEC138) ] | [ [>>](emacs.html#SEC144) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.5.5 VC Dired Commands

All the usual Dired commands work normally in VC Dired mode, except
for `v`, which is redefined as the version control prefix. You can
invoke VC commands such as `vc-diff` and `vc-print-log` by
typing `v =`, or `v l`, and so on. Most of these commands apply
to the file name on the current line.

The command `v v` (`vc-next-action`) operates on all the
marked files, so that you can lock or check in several files at once.
If it operates on more than one file, it handles each file according to
its current state; thus, it might lock one file, but check in another
file. This could be confusing; it is up to you to avoid confusing
behavior by marking a set of files that are in a similar state.

If any files call for check-in, `v v` reads a single log entry,
then uses it for all the files being checked in. This is convenient for
registering or checking in several files at once, as part of the same
change.

You can toggle between terse display (only locked files, or files not
up-to-date) and full display at any time by typing `v t`
(`vc-dired-toggle-terse-mode`). There is also a special command
`* l` (`vc-dired-mark-locked`), which marks all files currently
locked (or, with CVS, all files not up-to-date). Thus, typing `* l
t k` is another way to delete from the buffer all files except those
currently locked.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC143) ] | [ [>](emacs.html#SEC145) ] | [ [<<](emacs.html#SEC149) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC149) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.6 Multiple Branches of a File

One use of version control is to maintain multiple "current"
versions of a file. For example, you might have different versions of a
program in which you are gradually adding various unfinished new
features. Each such independent line of development is called a
*branch*. VC allows you to create branches, switch between
different branches, and merge changes from one branch to another.
Please note, however, that branches are only supported for RCS at the
moment.

A file's main line of development is usually called the *trunk*.
The versions on the trunk are normally numbered 1.1, 1.2, 1.3, etc. At
any such version, you can start an independent branch. A branch
starting at version 1.2 would have version number 1.2.1.1, and consecutive
versions on this branch would have numbers 1.2.1.2, 1.2.1.3, 1.2.1.4,
and so on. If there is a second branch also starting at version 1.2, it
would consist of versions 1.2.2.1, 1.2.2.2, 1.2.2.3, etc.

If you omit the final component of a version number, that is called a
*branch number*. It refers to the highest existing version on that
branch--the *head version* of that branch. The branches in the
example above have branch numbers 1.2.1 and 1.2.2.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.6.1 Switching between Branches](emacs.html#SEC145) |  | How to get to another existing branch. |
> | [M.7.6.2 Creating New Branches](emacs.html#SEC146) |  | How to start a new branch. |
> | [M.7.6.3 Merging Branches](emacs.html#SEC147) |  | Transferring changes between branches. |
> | [M.7.6.4 Multi-User Branching](emacs.html#SEC148) |  | Multiple users working at multiple branches in parallel. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC144) ] | [ [>](emacs.html#SEC146) ] | [ [<<](emacs.html#SEC149) ] | [ [Up](emacs.html#SEC144) ] | [ [>>](emacs.html#SEC149) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.6.1 Switching between Branches

To switch between branches, type `C-u C-x C-q` and specify the
version number you want to select. This version is then visited
*unlocked* (write-protected), so you can examine it before locking
it. Switching branches in this way is allowed only when the file is not
locked.

You can omit the minor version number, thus giving only the branch
number; this takes you to the head version on the chosen branch. If you
only type `RET`, Emacs goes to the highest version on the trunk.

After you have switched to any branch (including the main branch), you
stay on it for subsequent VC commands, until you explicitly select some
other branch.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC145) ] | [ [>](emacs.html#SEC147) ] | [ [<<](emacs.html#SEC147) ] | [ [Up](emacs.html#SEC144) ] | [ [>>](emacs.html#SEC149) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.6.2 Creating New Branches

To create a new branch from a head version (one that is the latest in
the branch that contains it), first select that version if necessary,
lock it with `C-x C-q`, and make whatever changes you want. Then,
when you check in the changes, use `C-u C-x C-q`. This lets you
specify the version number for the new version. You should specify a
suitable branch number for a branch starting at the current version.
For example, if the current version is 2.5, the branch number should be
2.5.1, 2.5.2, and so on, depending on the number of existing branches at
that point.

To create a new branch at an older version (one that is no longer the
head of a branch), first select that version (see section [M.7.6.1 Switching between Branches](emacs.html#SEC145)), then lock it with `C-x C-q`. You'll be asked to
confirm, when you lock the old version, that you really mean to create a
new branch--if you say no, you'll be offered a chance to lock the
latest version instead.

Then make your changes and type `C-x C-q` again to check in a new
version. This automatically creates a new branch starting from the
selected version. You need not specially request a new branch, because
that's the only way to add a new version at a point that is not the head
of a branch.

After the branch is created, you "stay" on it. That means that
subsequent check-ins create new versions on that branch. To leave the
branch, you must explicitly select a different version with `C-u C-x
C-q`. To transfer changes from one branch to another, use the merge
command, described in the next section.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC146) ] | [ [>](emacs.html#SEC148) ] | [ [<<](emacs.html#SEC148) ] | [ [Up](emacs.html#SEC144) ] | [ [>>](emacs.html#SEC149) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.6.3 Merging Branches

When you have finished the changes on a certain branch, you will
often want to incorporate them into the file's main line of development
(the trunk). This is not a trivial operation, because development might
also have proceeded on the trunk, so that you must *merge* the
changes into a file that has already been changed otherwise. VC allows
you to do this (and other things) with the `vc-merge` command.

`C-x v m (vc-merge)`: Merge changes into the work file.

`C-x v m` (`vc-merge`) takes a set of changes and merges it
into the current version of the work file. It firsts asks you in the
minibuffer where the changes should come from. If you just type
`RET`, Emacs merges any changes that were made on the same branch
since you checked the file out (we call this *merging the news*).
This is the common way to pick up recent changes from the repository,
regardless of whether you have already changed the file yourself.

You can also enter a branch number or a pair of version numbers in
the minibuffer. Then `C-x v m` finds the changes from that
branch, or the differences between the two versions you specified, and
merges them into the current version of the current file.

As an example, suppose that you have finished a certain feature on
branch 1.3.1. In the meantime, development on the trunk has proceeded
to version 1.5. To merge the changes from the branch to the trunk,
first go to the head version of the trunk, by typing `C-u C-x C-q
RET`. Version 1.5 is now current. If locking is used for the file,
type `C-x C-q` to lock version 1.5 so that you can change it. Next,
type `C-x v m 1.3.1 RET`. This takes the entire set of changes on
branch 1.3.1 (relative to version 1.3, where the branch started, up to
the last version on the branch) and merges it into the current version
of the work file. You can now check in the changed file, thus creating
version 1.6 containing the changes from the branch.

It is possible to do further editing after merging the branch, before
the next check-in. But it is usually wiser to check in the merged
version, then lock it and make the further changes. This will keep
a better record of the history of changes.

When you merge changes into a file that has itself been modified, the
changes might overlap. We call this situation a *conflict*, and
reconciling the conflicting changes is called *resolving a
conflict*.

Whenever conflicts occur during merging, VC detects them, tells you
about them in the echo area, and asks whether you want help in merging.
If you say yes, it starts an Ediff session (see section `Ediff' in The Ediff Manual).

If you say no, the conflicting changes are both inserted into the
file, surrounded by *conflict markers*. The example below shows how
a conflict region looks; the file is called `` `name' `` and the current
master file version with user B's changes in it is 1.11.

|  |  |
| --- | --- |
|  | ``` <<<<<<< name   User A's version =======   User B's version >>>>>>> 1.11 ``` |

Then you can resolve the conflicts by editing the file manually. Or
you can type `M-x vc-resolve-conflicts` after visiting the file.
This starts an Ediff session, as described above. Don't forget to
check in the merged version afterwards.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC147) ] | [ [>](emacs.html#SEC149) ] | [ [<<](emacs.html#SEC149) ] | [ [Up](emacs.html#SEC144) ] | [ [>>](emacs.html#SEC149) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.6.4 Multi-User Branching

It is often useful for multiple developers to work simultaneously on
different branches of a file. CVS allows this by default; for RCS, it
is possible if you create multiple source directories. Each source
directory should have a link named `RCS' which points to a common
directory of RCS master files. Then each source directory can have its
own choice of selected versions, but all share the same common RCS
records.

This technique works reliably and automatically, provided that the
source files contain RCS version headers (see section [M.7.9.3 Inserting Version Control Headers](emacs.html#SEC158)). The
headers enable Emacs to be sure, at all times, which version number is
present in the work file.

If the files do not have version headers, you must instead tell Emacs
explicitly in each session which branch you are working on. To do this,
first find the file, then type `C-u C-x C-q` and specify the correct
branch number. This ensures that Emacs knows which branch it is using
during this particular editing session.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC148) ] | [ [>](emacs.html#SEC150) ] | [ [<<](emacs.html#SEC152) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC152) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
