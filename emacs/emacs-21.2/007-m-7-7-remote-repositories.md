### M.7.7 Remote Repositories

A common way of using CVS is to set up a central CVS repository on
some Internet host, then have each developer check out a personal
working copy of the files on his local machine. Committing changes to
the repository, and picking up changes from other users into one's own
working area, then works by direct interactions with the CVS server.

One difficulty is that access to the CVS server is often slow, and
that developers might need to work off-line as well. VC is designed
to reduce the amount of network interaction necessary.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.7.1 Version Backups](emacs.html#SEC150) |  | Keeping local copies of repository versions. |
> | [M.7.7.2 Local Version Control](emacs.html#SEC151) |  | Using another version system for local editing. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC149) ] | [ [>](emacs.html#SEC151) ] | [ [<<](emacs.html#SEC152) ] | [ [Up](emacs.html#SEC149) ] | [ [>>](emacs.html#SEC152) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.7.1 Version Backups

When VC sees that the CVS repository for a file is on a remote
machine, it automatically makes local backups of unmodified versions
of the file---*automatic version backups*. This means that you
can compare the file to the repository version (`C-x v =`), or
revert to that version (`C-x v u`), without any network
interactions.

The local copy of the unmodified file is called a *version
backup* to indicate that it corresponds exactly to a version that is
stored in the repository. Note that version backups are not the same
as ordinary Emacs backup files (see section [M.3.1 Backup Files](emacs.html#SEC114)). But they follow a
similar naming convention.

For a file that comes from a remote CVS repository, VC makes a
version backup whenever you save the first changes to the file, and
removes it after you have committed your modified version to the
repository. You can disable the making of automatic version backups by
setting `vc-cvs-stay-local` to `nil` (see section [M.7.10.3 Options specific for CVS](emacs.html#SEC162)).

The name of the automatic version backup for version version
of file file is `file.~version.~`. This is
almost the same as the name used by `C-x v ~` (see section [M.7.4 Examining And Comparing Old Versions](emacs.html#SEC137)), the only difference being the additional dot (`` `.' ``)
after the version number. This similarity is intentional, because
both kinds of files store the same kind of information. The file made
by `C-x v ~` acts as a *manual version backup*.

All the VC commands that operate on old versions of a file can use
both kinds of version backups. For instance, `C-x v ~` uses
either an automatic or a manual version backup, if possible, to get
the contents of the version you request. Likewise, `C-x v =` and
`C-x v u` use either an automatic or a manual version backup, if
one of them exists, to get the contents of a version to compare or
revert to. If you changed a file outside of Emacs, so that no
automatic version backup was created for the previous text, you can
create a manual backup of that version using `C-x v ~`, and thus
obtain the benefit of the local copy for Emacs commands.

The only difference in Emacs's handling of manual and automatic
version backups, once they exist, is that Emacs deletes automatic
version backups when you commit to the repository. By contrast,
manual version backups remain until you delete them.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC150) ] | [ [>](emacs.html#SEC152) ] | [ [<<](emacs.html#SEC152) ] | [ [Up](emacs.html#SEC149) ] | [ [>>](emacs.html#SEC152) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.7.2 Local Version Control

When you make many changes to a file that comes from a remote
repository, it can be convenient to have version control on your local
machine as well. You can then record intermediate versions, revert to
a previous state, etc., before you actually commit your changes to the
remote server.

VC lets you do this by putting a file under a second, local version
control system, so that the file is effectively registered in two
systems at the same time. For the description here, we will assume
that the remote system is CVS, and you use RCS locally, although the
mechanism works with any combination of version control systems
(*back ends*).

To make it work with other back ends, you must make sure that the
"more local" back end comes before the "more remote" back end in
the setting of `vc-handled-backends` (see section [M.7.10 Customizing VC](emacs.html#SEC159)). By
default, this variable is set up so that you can use remote CVS and
local RCS as described here.

To start using local RCS for a file that comes from a remote CVS
server, you must *register the file in RCS*, by typing `C-u
C-x v v rcs RET`. (In other words, use `vc-next-action` with a
prefix argument, and specify RCS as the back end.)

You can do this at any time; it does not matter whether you have
already modified the file with respect to the version in the CVS
repository. If possible, VC tries to make the RCS master start with
the unmodified repository version, then checks in any local changes
as a new version. This works if you have not made any changes yet, or
if the unmodified repository version exists locally as a version
backup (see section [M.7.7.1 Version Backups](emacs.html#SEC150)). If the unmodified version is not
available locally, the RCS master starts with the modified version;
the only drawback to this is that you cannot compare your changes
locally to what is stored in the repository.

The version number of the RCS master is derived from the current CVS
version, starting a branch from it. For example, if the current CVS
version is 1.23, the local RCS branch will be 1.23.1. Version 1.23 in
the RCS master will be identical to version 1.23 under CVS; your first
changes are checked in as 1.23.1.1. (If the unmodified file is not
available locally, VC will check in the modified file twice, both as
1.23 and 1.23.1.1, to make the revision numbers consistent.)

If you do not use locking under CVS (the default), locking is also
disabled for RCS, so that editing under RCS works exactly as under
CVS.

When you are done with local editing, you can commit the final version
back to the CVS repository by typing `C-u C-x v v cvs RET`.
This initializes the log entry buffer (see section [M.7.3.4 Features of the Log Entry Buffer](emacs.html#SEC136)) to contain
all the log entries you have recorded in the RCS master; you can edit
them as you wish, and then commit in CVS by typing `C-c C-c`. If
the commit is successful, VC removes the RCS master, so that the file
is once again registered under CVS only. (The RCS master is not
actually deleted, just renamed by appending `` `~' `` to the name, so
that you can refer to it later if you wish.)

While using local RCS, you can pick up recent changes from the CVS
repository into your local file, or commit some of your changes back
to CVS, without terminating local RCS version control. To do this,
switch to the CVS back end temporarily, with the `C-x v b` command:

`C-x v b`: Switch to another back end that the current file is registered under (`vc-switch-backend`). `C-u C-x v b backend RET`: Switch to backend for the current file.

`C-x v b` does not change the buffer contents, or any files; it
only changes VC's perspective on how to handle the file. Any
subsequent VC commands for that file will operate on the back end that
is currently selected.

If the current file is registered in more than one back end, typing
`C-x v b` "cycles" through all of these back ends. With a
prefix argument, it asks for the back end to use in the minibuffer.

Thus, if you are using local RCS, and you want to pick up some recent
changes in the file from remote CVS, first visit the file, then type
`C-x v b` to switch to CVS, and finally use `C-x v m
RET` to merge the news (see section [M.7.6.3 Merging Branches](emacs.html#SEC147)). You can then switch
back to RCS by typing `C-x v b` again, and continue to edit
locally.

But if you do this, the revision numbers in the RCS master no longer
correspond to those of CVS. Technically, this is not a problem, but
it can become difficult to keep track of what is in the CVS repository
and what is not. So we suggest that you return from time to time to
CVS-only operation, using `C-u C-x v v cvs RET`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC151) ] | [ [>](emacs.html#SEC153) ] | [ [<<](emacs.html#SEC155) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC155) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.8 Snapshots

A *snapshot* is a named set of file versions (one for each
registered file) that you can treat as a unit. One important kind of
snapshot is a *release*, a (theoretically) stable version of the
system that is ready for distribution to users.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.8.1 Making and Using Snapshots](emacs.html#SEC153) |  | The snapshot facilities. |
> | [M.7.8.2 Snapshot Caveats](emacs.html#SEC154) |  | Things to be careful of when using snapshots. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC152) ] | [ [>](emacs.html#SEC154) ] | [ [<<](emacs.html#SEC155) ] | [ [Up](emacs.html#SEC152) ] | [ [>>](emacs.html#SEC155) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.8.1 Making and Using Snapshots

There are two basic commands for snapshots; one makes a
snapshot with a given name, the other retrieves a named snapshot.

`C-x v s name RET`: Define the last saved versions of every registered file in or under the current directory as a snapshot named name (`vc-create-snapshot`). `C-x v r name RET`: For all registered files at or below the current directory level, select whatever versions correspond to the snapshot name (`vc-retrieve-snapshot`). This command reports an error if any files are locked at or below the current directory, without changing anything; this is to avoid overwriting work in progress.

A snapshot uses a very small amount of resources--just enough to record
the list of file names and which version belongs to the snapshot. Thus,
you need not hesitate to create snapshots whenever they are useful.

You can give a snapshot name as an argument to `C-x v =` or
`C-x v ~` (see section [M.7.4 Examining And Comparing Old Versions](emacs.html#SEC137)). Thus, you can use it to compare a
snapshot against the current files, or two snapshots against each other,
or a snapshot against a named version.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC153) ] | [ [>](emacs.html#SEC155) ] | [ [<<](emacs.html#SEC155) ] | [ [Up](emacs.html#SEC152) ] | [ [>>](emacs.html#SEC155) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.8.2 Snapshot Caveats

VC's snapshot facilities are modeled on RCS's named-configuration
support. They use RCS's native facilities for this, so under VC
snapshots made using RCS are visible even when you bypass VC.

For SCCS, VC implements snapshots itself. The files it uses contain
name/file/version-number triples. These snapshots are visible only
through VC.

A snapshot is a set of checked-in versions. So make sure that all the
files are checked in and not locked when you make a snapshot.

File renaming and deletion can create some difficulties with snapshots.
This is not a VC-specific problem, but a general design issue in version
control systems that no one has solved very well yet.

If you rename a registered file, you need to rename its master along
with it (the command `vc-rename-file` does this automatically). If
you are using SCCS, you must also update the records of the snapshot, to
mention the file by its new name (`vc-rename-file` does this,
too). An old snapshot that refers to a master file that no longer
exists under the recorded name is invalid; VC can no longer retrieve
it. It would be beyond the scope of this manual to explain enough about
RCS and SCCS to explain how to update the snapshots by hand.

Using `vc-rename-file` makes the snapshot remain valid for
retrieval, but it does not solve all problems. For example, some of the
files in your program probably refer to others by name. At the very
least, the makefile probably mentions the file that you renamed. If you
retrieve an old snapshot, the renamed file is retrieved under its new
name, which is not the name that the makefile expects. So the program
won't really work as retrieved.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC154) ] | [ [>](emacs.html#SEC156) ] | [ [<<](emacs.html#SEC163) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC159) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.9 Miscellaneous Commands and Features of VC

This section explains the less-frequently-used features of VC.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.9.1 Change Logs and VC](emacs.html#SEC156) |  | Generating a change log file from log entries. |
> | [M.7.9.2 Renaming VC Work Files and Master Files](emacs.html#SEC157) |  | A command to rename both the source and master file correctly. |
> | [M.7.9.3 Inserting Version Control Headers](emacs.html#SEC158) |  | Inserting version control headers into working files. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC155) ] | [ [>](emacs.html#SEC157) ] | [ [<<](emacs.html#SEC163) ] | [ [Up](emacs.html#SEC155) ] | [ [>>](emacs.html#SEC159) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.9.1 Change Logs and VC

If you use RCS or CVS for a program and also maintain a change log
file for it (see section [W.1 Change Logs](emacs.html#SEC333)), you can generate change log entries
automatically from the version control log entries:

`C-x v a`: Visit the current directory's change log file and, for registered files in that directory, create new entries for versions checked in since the most recent entry in the change log file. (`vc-update-change-log`). This command works with RCS or CVS only, not with SCCS. `C-u C-x v a`: As above, but only find entries for the current buffer's file. `M-1 C-x v a`: As above, but find entries for all the currently visited files that are maintained with version control. This works only with RCS, and it puts all entries in the log for the default directory, which may not be appropriate.

For example, suppose the first line of `ChangeLog' is dated
1999-04-10, and that the only check-in since then was by Nathaniel
Bowditch to `rcs2log' on 1999-05-22 with log text `` `Ignore log
messages that start with `#'.' ``. Then `C-x v a` visits
`ChangeLog' and inserts text like this:

|  |  |
| --- | --- |
|  | ``` 1999-05-22  Nathaniel Bowditch  <nat@apn.org>          * rcs2log: Ignore log messages that start with `#'. ``` |

You can then edit the new change log entry further as you wish.

Some of the new change log entries may duplicate what's already in
ChangeLog. You will have to remove these duplicates by hand.

Normally, the log entry for file `foo' is displayed as `` `*
foo: text of log entry' ``. The `` `:' `` after `foo' is omitted
if the text of the log entry starts with `` `(functionname):
' ``. For example, if the log entry for `vc.el' is
`` `(vc-do-command): Check call-process status.' ``, then the text in
`ChangeLog' looks like this:

|  |  |
| --- | --- |
|  | ``` 1999-05-06  Nathaniel Bowditch  <nat@apn.org>          * vc.el (vc-do-command): Check call-process status. ``` |

When `C-x v a` adds several change log entries at once, it groups
related log entries together if they all are checked in by the same
author at nearly the same time. If the log entries for several such
files all have the same text, it coalesces them into a single entry.
For example, suppose the most recent check-ins have the following log
entries:

```
* For `vc.texinfo': `Fix expansion typos.'
* For `vc.el': `Don't call expand-file-name.'
* For `vc-hooks.el': `Don't call expand-file-name.'
```

They appear like this in `ChangeLog':

|  |  |
| --- | --- |
|  | ``` 1999-04-01  Nathaniel Bowditch  <nat@apn.org>          * vc.texinfo: Fix expansion typos.          * vc.el, vc-hooks.el: Don't call expand-file-name. ``` |

Normally, `C-x v a` separates log entries by a blank line, but you
can mark several related log entries to be clumped together (without an
intervening blank line) by starting the text of each related log entry
with a label of the form `` `{clumpname} ' ``. The label
itself is not copied to `ChangeLog'. For example, suppose the log
entries are:

```
* For `vc.texinfo': `{expand} Fix expansion typos.'
* For `vc.el': `{expand} Don't call expand-file-name.'
* For `vc-hooks.el': `{expand} Don't call expand-file-name.'
```

Then the text in `ChangeLog' looks like this:

|  |  |
| --- | --- |
|  | ``` 1999-04-01  Nathaniel Bowditch  <nat@apn.org>          * vc.texinfo: Fix expansion typos.         * vc.el, vc-hooks.el: Don't call expand-file-name. ``` |

A log entry whose text begins with `` `#' `` is not copied to
`ChangeLog'. For example, if you merely fix some misspellings in
comments, you can log the change with an entry beginning with `` `#' ``
to avoid putting such trivia into `ChangeLog'.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC156) ] | [ [>](emacs.html#SEC158) ] | [ [<<](emacs.html#SEC158) ] | [ [Up](emacs.html#SEC155) ] | [ [>>](emacs.html#SEC159) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.9.2 Renaming VC Work Files and Master Files

When you rename a registered file, you must also rename its master
file correspondingly to get proper results. Use `vc-rename-file`
to rename the source file as you specify, and rename its master file
accordingly. It also updates any snapshots (see section [M.7.8 Snapshots](emacs.html#SEC152)) that
mention the file, so that they use the new name; despite this, the
snapshot thus modified may not completely work (see section [M.7.8.2 Snapshot Caveats](emacs.html#SEC154)).

You cannot use `vc-rename-file` on a file that is locked by
someone else.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC157) ] | [ [>](emacs.html#SEC159) ] | [ [<<](emacs.html#SEC163) ] | [ [Up](emacs.html#SEC155) ] | [ [>>](emacs.html#SEC159) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.9.3 Inserting Version Control Headers

Sometimes it is convenient to put version identification strings
directly into working files. Certain special strings called
*version headers* are replaced in each successive version by the
number of that version.

If you are using RCS, and version headers are present in your working
files, Emacs can use them to determine the current version and the
locking state of the files. This is more reliable than referring to the
master files, which is done when there are no version headers. Note
that in a multi-branch environment, version headers are necessary to
make VC behave correctly (see section [M.7.6.4 Multi-User Branching](emacs.html#SEC148)).

Searching for version headers is controlled by the variable
`vc-consult-headers`. If it is non-`nil` (the default),
Emacs searches for headers to determine the version number you are
editing. Setting it to `nil` disables this feature.

You can use the `C-x v h` command (`vc-insert-headers`) to
insert a suitable header string.

`C-x v h`: Insert headers in a file for use with your version-control system.

The default header string is `` `$Id$' `` for RCS and
`` `%W%' `` for SCCS. You can specify other headers to insert by
setting the variable `vc-header-alist`. Its value is a list of
elements of the form `(program . string)` where
program is `RCS` or `SCCS` and string is the
string to use.

Instead of a single string, you can specify a list of strings; then
each string in the list is inserted as a separate header on a line of
its own.

It is often necessary to use "superfluous" backslashes when
writing the strings that you put in this variable. For instance, you
might write `"$Id\$"` rather than `"$Id$"`. The extra
backslash prevents the string constant from being interpreted as a
header, if the Emacs Lisp file containing it is maintained with
version control.

Each header is inserted surrounded by tabs, inside comment delimiters,
on a new line at point. Normally the ordinary comment
start and comment end strings of the current mode are used, but for
certain modes, there are special comment delimiters for this purpose;
the variable `vc-comment-alist` specifies them. Each element of
this list has the form `(mode starter ender)`.

The variable `vc-static-header-alist` specifies further strings
to add based on the name of the buffer. Its value should be a list of
elements of the form `(regexp . format)`. Whenever
regexp matches the buffer name, format is inserted as part
of the header. A header line is inserted for each element that matches
the buffer name, and for each string specified by
`vc-header-alist`. The header line is made by processing the
string from `vc-header-alist` with the format taken from the
element. The default value for `vc-static-header-alist` is as follows:

|  |  |
| --- | --- |
|  | ``` (("\\.c$" .   "\n#ifndef lint\nstatic char vcid[] = \"\%s\";\n\ #endif /* lint */\n")) ``` |

It specifies insertion of text of this form:

|  |  |
| --- | --- |
|  | ``` #ifndef lint static char vcid[] = "string"; #endif /* lint */ ``` |

Note that the text above starts with a blank line.

If you use more than one version header in a file, put them close
together in the file. The mechanism in `revert-buffer` that
preserves markers may not handle markers positioned between two version
headers.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC158) ] | [ [>](emacs.html#SEC160) ] | [ [<<](emacs.html#SEC131) ] | [ [Up](emacs.html#SEC127) ] | [ [>>](emacs.html#SEC163) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### M.7.10 Customizing VC

The variable `vc-handled-backends` determines which version
control systems VC should handle. The default value is `(RCS CVS
SCCS)`, so it contains all three version systems that are currently
supported. If you want VC to ignore one or more of these systems,
exclude its name from the list.

The order of systems in the list is significant: when you visit a file
registered in more than one system (see section [M.7.7.2 Local Version Control](emacs.html#SEC151)),
VC uses the system that comes first in `vc-handled-backends` by
default. The order is also significant when you register a file for
the first time, see section [M.7.5.1 Registering a File for Version Control](emacs.html#SEC139) for details.

> |  |  |  |
> | --- | --- | --- |
> | [M.7.10.1 General Options](emacs.html#SEC160) |  | Options that apply to multiple back ends. |
> | [M.7.10.2 Options for RCS and SCCS](emacs.html#SEC161) |  |  |
> | [M.7.10.3 Options specific for CVS](emacs.html#SEC162) |  | Options for CVS. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC159) ] | [ [>](emacs.html#SEC161) ] | [ [<<](emacs.html#SEC131) ] | [ [Up](emacs.html#SEC159) ] | [ [>>](emacs.html#SEC163) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.10.1 General Options

Emacs normally does not save backup files for source files that are
maintained with version control. If you want to make backup files even
for files that use version control, set the variable
`vc-make-backup-files` to a non-`nil` value.

Normally the work file exists all the time, whether it is locked or
not. If you set `vc-keep-workfiles` to `nil`, then checking
in a new version with `C-x C-q` deletes the work file; but any
attempt to visit the file with Emacs creates it again. (With CVS, work
files are always kept.)

Editing a version-controlled file through a symbolic link can be
dangerous. It bypasses the version control system--you can edit the
file without locking it, and fail to check your changes in. Also,
your changes might overwrite those of another user. To protect against
this, VC checks each symbolic link that you visit, to see if it points
to a file under version control.

The variable `vc-follow-symlinks` controls what to do when a
symbolic link points to a version-controlled file. If it is `nil`,
VC only displays a warning message. If it is `t`, VC automatically
follows the link, and visits the real file instead, telling you about
this in the echo area. If the value is `ask` (the default), VC
asks you each time whether to follow the link.

If `vc-suppress-confirm` is non-`nil`, then `C-x C-q`
and `C-x v i` can save the current buffer without asking, and
`C-x v u` also operates without asking for confirmation. (This
variable does not affect `C-x v c`; that operation is so drastic
that it should always ask for confirmation.)

VC mode does much of its work by running the shell commands for RCS,
CVS and SCCS. If `vc-command-messages` is non-`nil`, VC
displays messages to indicate which shell commands it runs, and
additional messages when the commands finish.

You can specify additional directories to search for version control
programs by setting the variable `vc-path`. These directories
are searched before the usual search path. It is rarely necessary to
set this variable, because VC normally finds the proper files
automatically.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC160) ] | [ [>](emacs.html#SEC162) ] | [ [<<](emacs.html#SEC162) ] | [ [Up](emacs.html#SEC159) ] | [ [>>](emacs.html#SEC163) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.10.2 Options for RCS and SCCS

By default, RCS uses locking to coordinate the activities of several
users, but there is a mode called *non-strict locking* in which
you can check-in changes without locking the file first. Use
`` `rcs -U' `` to switch to non-strict locking for a particular file,
see the `rcs` manual page for details.

When deducing the version control state of an RCS file, VC first
looks for an RCS version header string in the file (see section [M.7.9.3 Inserting Version Control Headers](emacs.html#SEC158)). If there is no header string, VC normally looks at the
file permissions of the work file; this is fast. But there might be
situations when the file permissions cannot be trusted. In this case
the master file has to be consulted, which is rather expensive. Also
the master file can only tell you *if* there's any lock on the
file, but not whether your work file really contains that locked
version.

You can tell VC not to use version headers to determine the file
status by setting `vc-consult-headers` to `nil`. VC then
always uses the file permissions (if it is supposed to trust them), or
else checks the master file.

You can specify the criterion for whether to trust the file
permissions by setting the variable `vc-mistrust-permissions`.
Its value can be `t` (always mistrust the file permissions and
check the master file), `nil` (always trust the file
permissions), or a function of one argument which makes the decision.
The argument is the directory name of the `RCS' subdirectory. A
non-`nil` value from the function says to mistrust the file
permissions. If you find that the file permissions of work files are
changed erroneously, set `vc-mistrust-permissions` to `t`.
Then VC always checks the master file to determine the file's status.

VC determines the version control state of files under SCCS much as
with RCS. It does not consider SCCS version headers, though. Thus,
the variable `vc-mistrust-permissions` affects SCCS use, but
`vc-consult-headers` does not.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC161) ] | [ [>](emacs.html#SEC163) ] | [ [<<](emacs.html#SEC131) ] | [ [Up](emacs.html#SEC159) ] | [ [>>](emacs.html#SEC163) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### M.7.10.3 Options specific for CVS

By default, CVS does not use locking to coordinate the activities of
several users; anyone can change a work file at any time. However,
there are ways to restrict this, resulting in behavior that resembles
locking.

For one thing, you can set the `CVSREAD` environment variable
(the value you use makes no difference). If this variable is defined,
CVS makes your work files read-only by default. In Emacs, you must
type `C-x C-q` to make the file writable, so that editing works
in fact similar as if locking was used. Note however, that no actual
locking is performed, so several users can make their files writable
at the same time. When setting `CVSREAD` for the first time, make
sure to check out all your modules anew, so that the file protections
are set correctly.

Another way to achieve something similar to locking is to use the
*watch* feature of CVS. If a file is being watched, CVS makes it
read-only by default, and you must also use `C-x C-q` in Emacs to
make it writable. VC calls `cvs edit` to make the file writable,
and CVS takes care to notify other developers of the fact that you
intend to change the file. See the CVS documentation for details on
using the watch feature.

When a file's repository is on a remote machine, VC tries to keep
network interactions to a minimum. This is controlled by the variable
`vc-cvs-stay-local`. If it is `t` (the default), then VC uses
only the entry in the local CVS subdirectory to determine the file's
state (and possibly information returned by previous CVS commands). One
consequence of this is that when you have modified a file, and somebody
else has already checked in other changes to the file, you are not
notified of it until you actually try to commit. (But you can try to
pick up any recent changes from the repository first, using `C-x v m
RET`, see section [M.7.6.3 Merging Branches](emacs.html#SEC147)).

When `vc-cvs-stay-local` is `t`, VC also makes local
version backups, so that simple diff and revert operations are
completely local (see section [M.7.7.1 Version Backups](emacs.html#SEC150)).

On the other hand, if you set `vc-cvs-stay-local` to `nil`,
then VC queries the remote repository *before* it decides what to
do in `vc-next-action` (`C-x v v`), just as it does for local
repositories. It also does not make any version backups.

You can also set `vc-cvs-stay-local` to a regular expression
that is matched against the repository host name; VC then stays local
only for repositories from hosts that match the pattern.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC162) ] | [ [>](emacs.html#SEC164) ] | [ [<<](emacs.html#SEC164) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.8 File Directories

The file system groups files into *directories*. A *directory
listing* is a list of all the files in a directory. Emacs provides
commands to create and delete directories, and to make directory
listings in brief format (file names only) and verbose format (sizes,
dates, and authors included). There is also a directory browser called
Dired; see [AB. Dired, the Directory Editor](emacs.html#SEC396).

`C-x C-d dir-or-pattern RET`: Display a brief directory listing (`list-directory`). `C-u C-x C-d dir-or-pattern RET`: Display a verbose directory listing. `M-x make-directory RET dirname RET`: Create a new directory named dirname. `M-x delete-directory RET dirname RET`: Delete the directory named dirname. It must be empty, or you get an error.

The command to display a directory listing is `C-x C-d`
(`list-directory`). It reads using the minibuffer a file name
which is either a directory to be listed or a wildcard-containing
pattern for the files to be listed. For example,

|  |  |
| --- | --- |
|  | ``` C-x C-d /u2/emacs/etc RET ``` |

lists all the files in directory `/u2/emacs/etc'. Here is an
example of specifying a file name pattern:

|  |  |
| --- | --- |
|  | ``` C-x C-d /u2/emacs/src/*.c RET ``` |

Normally, `C-x C-d` displays a brief directory listing containing
just file names. A numeric argument (regardless of value) tells it to
make a verbose listing including sizes, dates, and owners (like
`` `ls -l' ``).

The text of a directory listing is obtained by running `ls` in an
inferior process. Two Emacs variables control the switches passed to
`ls`: `list-directory-brief-switches` is a string giving the
switches to use in brief listings (`"-CF"` by default), and
`list-directory-verbose-switches` is a string giving the switches to
use in a verbose listing (`"-l"` by default).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC163) ] | [ [>](emacs.html#SEC165) ] | [ [<<](emacs.html#SEC110) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.9 Comparing Files

The command `M-x diff` compares two files, displaying the
differences in an Emacs buffer named `` `*diff*' ``. It works by
running the `diff` program, using options taken from the variable
`diff-switches`. The value of `diff-switches` should be a
string; the default is `"-c"` to specify a context diff.

The buffer `` `*diff*' `` has Compilation mode as its major mode, so
you can use `` C-x ` `` to visit successive changed locations in the two
source files. You can also move to a particular hunk of changes and
type `RET` or `C-c C-c`, or click `Mouse-2` on it, to move
to the corresponding source location. You can also use the other
special commands of Compilation mode: `SPC` and `DEL` for
scrolling, and `M-p` and `M-n` for cursor motion.
See section [V.1 Running Compilations under Emacs](emacs.html#SEC317).

The command `M-x diff-backup` compares a specified file with its most
recent backup. If you specify the name of a backup file,
`diff-backup` compares it with the source file that it is a backup
of.

The command `M-x compare-windows` compares the text in the current
window with that in the next window. Comparison starts at point in each
window, and each starting position is pushed on the mark ring in its
respective buffer. Then point moves forward in each window, a character
at a time, until a mismatch between the two windows is reached. Then
the command is finished. For more information about windows in Emacs,
[O. Multiple Windows](emacs.html#SEC183).

With a numeric argument, `compare-windows` ignores changes in
whitespace. If the variable `compare-ignore-case` is
non-`nil`, it ignores differences in case as well.

Differences between versions of files are often distributed as
*patches*, which are the output from `diff` or a version
control system that uses `diff`. `M-x diff-mode` turns on
Diff mode, a major mode for viewing and editing patches, either as
"unified diffs" or "context diffs."

You can use `M-x smerge-mode` to turn on Smerge mode, a minor
mode for editing output from the `diff3` program. This is
typically the result of a failed merge from a version control system
"update" outside VC, due to conflicting changes to a file. Smerge
mode provides commands to resolve conflicts by selecting specific
changes.

See also [W.3 Merging Files with Emerge](emacs.html#SEC342), and section `Top' in The Ediff Manual, for
convenient facilities for merging two similar files.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC164) ] | [ [>](emacs.html#SEC166) ] | [ [<<](emacs.html#SEC112) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.10 Miscellaneous File Operations

Emacs has commands for performing many other operations on files.
All operate on one file; they do not accept wildcard file names.

`M-x view-file` allows you to scan or read a file by sequential
screenfuls. It reads a file name argument using the minibuffer. After
reading the file into an Emacs buffer, `view-file` displays the
beginning. You can then type `SPC` to scroll forward one windowful,
or `DEL` to scroll backward. Various other commands are provided
for moving around in the file, but none for changing it; type `?`
while viewing for a list of them. They are mostly the same as normal
Emacs cursor motion commands. To exit from viewing, type `q`.
The commands for viewing are defined by a special major mode called View
mode.

A related command, `M-x view-buffer`, views a buffer already present
in Emacs. See section [N.3 Miscellaneous Buffer Operations](emacs.html#SEC175).

`M-x insert-file` (also `C-x i`) inserts a copy of the
contents of the specified file into the current buffer at point,
leaving point unchanged before the contents and the mark after them.

`M-x write-region` is the inverse of `M-x insert-file`; it
copies the contents of the region into the specified file. `M-x
append-to-file` adds the text of the region to the end of the specified
file. See section [H.9 Accumulating Text](emacs.html#SEC67).

`M-x delete-file` deletes the specified file, like the `rm`
command in the shell. If you are deleting many files in one directory, it
may be more convenient to use Dired (see section [AB. Dired, the Directory Editor](emacs.html#SEC396)).

`M-x rename-file` reads two file names old and new using
the minibuffer, then renames file old as new. If the file name
new already exists, you must confirm with `yes` or renaming is not
done; this is because renaming causes the old meaning of the name new
to be lost. If old and new are on different file systems, the
file old is copied and deleted.

The similar command `M-x add-name-to-file` is used to add an
additional name to an existing file without removing its old name.
The new name is created as a "hard link" to the existing file.
The new name must belong on the same file system that the file is on.
On Windows, this command works only if the file resides in an NTFS
file system. On MS-DOS, it works by copying the file.

`M-x copy-file` reads the file old and writes a new file named
new with the same contents. Confirmation is required if a file named
new already exists, because copying has the consequence of overwriting
the old contents of the file new.

`M-x make-symbolic-link` reads two file names target and
linkname, then creates a symbolic link named linkname, which
points at target. The effect is that future attempts to open file
linkname will refer to whatever file is named target at the
time the opening is done, or will get an error if the name target is
not in use at that time. This command does not expand the argument
target, so that it allows you to specify a relative name
as the target of the link.

Confirmation is required when creating the link if linkname is
in use. Note that not all systems support symbolic links; on systems
that don't support them, this command is not defined.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC165) ] | [ [>](emacs.html#SEC167) ] | [ [<<](emacs.html#SEC112) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.11 Accessing Compressed Files

Emacs comes with a library that can automatically uncompress
compressed files when you visit them, and automatically recompress them
if you alter them and save them. To enable this feature, type the
command `M-x auto-compression-mode`. You can enable it permanently
by customizing the option `auto-compression-mode`.

When automatic compression (which implies automatic uncompression as
well) is enabled, Emacs recognizes compressed files by their file names.
File names ending in `` `.gz' `` indicate a file compressed with
`gzip`. Other endings indicate other compression programs.

Automatic uncompression and compression apply to all the operations in
which Emacs uses the contents of a file. This includes visiting it,
saving it, inserting its contents into a buffer, loading it, and byte
compiling it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC166) ] | [ [>](emacs.html#SEC168) ] | [ [<<](emacs.html#SEC112) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.12 File Archives

A file whose name ends in `` `.tar' `` is normally an *archive*
made by the `tar` program. Emacs views these files in a special
mode called Tar mode which provides a Dired-like list of the contents
(see section [AB. Dired, the Directory Editor](emacs.html#SEC396)). You can move around through the list just as you
would in Dired, and visit the subfiles contained in the archive.
However, not all Dired commands are available in Tar mode.

If you enable Auto Compression mode (see section [M.11 Accessing Compressed Files](emacs.html#SEC166)), then
Tar mode is used also for compressed archives--files with extensions
`` `.tgz' ``, `.tar.Z` and `.tar.gz`.

The keys `e`, `f` and `RET` all extract a component file
into its own buffer. You can edit it there and when you save the buffer
the edited version will replace the version in the Tar buffer. `v`
extracts a file into a buffer in View mode. `o` extracts the file
and displays it in another window, so you could edit the file and
operate on the archive simultaneously. `d` marks a file for
deletion when you later use `x`, and `u` unmarks a file, as in
Dired. `C` copies a file from the archive to disk and `R`
renames a file. `g` reverts the buffer from the archive on disk.

The keys `M`, `G`, and `O` change the file's permission
bits, group, and owner, respectively.

If your display supports colors and the mouse, moving the mouse
pointer across a file name highlights that file name, indicating that
you can click on it. Clicking `Mouse-2` on the highlighted file
name extracts the file into a buffer and displays that buffer.

Saving the Tar buffer writes a new version of the archive to disk with
the changes you made to the components.

You don't need the `tar` program to use Tar mode--Emacs reads
the archives directly. However, accessing compressed archives
requires the appropriate uncompression program.

A separate but similar Archive mode is used for archives produced by
the programs `arc`, `jar`, `lzh`, `zip`, and
`zoo`, which have extensions corresponding to the program names.

The key bindings of Archive mode are similar to those in Tar mode,
with the addition of the `m` key which marks a file for subsequent
operations, and `M-DEL` which unmarks all the marked files.
Also, the `a` key toggles the display of detailed file
information, for those archive types where it won't fit in a single
line. Operations such as renaming a subfile, or changing its mode or
owner, are supported only for some of the archive formats.

Unlike Tar mode, Archive mode runs the archiving program to unpack
and repack archives. Details of the program names and their options
can be set in the `` `Archive' `` Customize group. However, you don't
need these programs to look at the archive table of contents, only to
extract or manipulate the subfiles in the archive.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC167) ] | [ [>](emacs.html#SEC169) ] | [ [<<](emacs.html#SEC112) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.13 Remote Files

You can refer to files on other machines using a special file name syntax:

|  |  |
| --- | --- |
|  | ``` /host:filename /user@host:filename /user@host#port:filename ``` |

When you do this, Emacs uses the FTP program to read and write files on
the specified host. It logs in through FTP using your user name or the
name user. It may ask you for a password from time to time; this
is used for logging in on host. The form using port allows
you to access servers running on a non-default TCP port.

If you want to disable backups for remote files, set the variable
`ange-ftp-make-backup-files` to `nil`.

Normally, if you do not specify a user name in a remote file name,
that means to use your own user name. But if you set the variable
`ange-ftp-default-user` to a string, that string is used instead.
(The Emacs package that implements FTP file access is called
`ange-ftp`.)

To visit files accessible by anonymous FTP, you use special user
names `` `anonymous' `` or `` `ftp' ``. Passwords for these user names
are handled specially. The variable
`ange-ftp-generate-anonymous-password` controls what happens: if
the value of this variable is a string, then that string is used as
the password; if non-`nil` (the default), then the value of
`user-mail-address` is used; if `nil`, the user is prompted
for a password as normal.

Sometimes you may be unable to access files on a remote machine
because a *firewall* in between blocks the connection for security
reasons. If you can log in on a *gateway* machine from which the
target files *are* accessible, and whose FTP server supports
gatewaying features, you can still use remote file names; all you have
to do is specify the name of the gateway machine by setting the
variable `ange-ftp-gateway-host`, and set
`ange-ftp-smart-gateway` to `t`. Otherwise you may be able
to make remote file names work, but the procedure is complex. You can
read the instructions by typing `M-x finder-commentary RET
ange-ftp RET`.

You can entirely turn off the FTP file name feature by removing the
entries `ange-ftp-completion-hook-function` and
`ange-ftp-hook-function` from the variable
`file-name-handler-alist`. You can turn off the feature in
individual cases by quoting the file name with `` `/:' `` (see section [M.14 Quoted File Names](emacs.html#SEC169)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC168) ] | [ [>](emacs.html#SEC170) ] | [ [<<](emacs.html#SEC112) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.14 Quoted File Names

You can *quote* an absolute file name to prevent special
characters and syntax in it from having their special effects.
The way to do this is to add `` `/:' `` at the beginning.

For example, you can quote a local file name which appears remote, to
prevent it from being treated as a remote file name. Thus, if you have
a directory named `/foo:' and a file named `bar' in it, you
can refer to that file in Emacs as `` `/:/foo:/bar' ``.

`` `/:' `` can also prevent `` `~' `` from being treated as a special
character for a user's home directory. For example, `/:/tmp/~hack'
refers to a file whose name is `~hack' in directory `/tmp'.

Likewise, quoting with `` `/:' `` is one way to enter in the minibuffer
a file name that contains `` `$' ``. However, the `` `/:' `` must be at
the beginning of the minibuffer in order to quote `` `$' ``.

You can also quote wildcard characters with `` `/:' ``, for visiting.
For example, `/:/tmp/foo\*bar' visits the file `/tmp/foo\*bar'.
However, in most cases you can simply type the wildcard characters for
themselves. For example, if the only file name in `/tmp' that
starts with `` `foo' `` and ends with `` `bar' `` is `foo\*bar', then
specifying `/tmp/foo\*bar' will visit just `/tmp/foo\*bar'.
Another way is to specify `/tmp/foo[\*]bar'.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC169) ] | [ [>](emacs.html#SEC171) ] | [ [<<](emacs.html#SEC112) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.15 File Name Cache

You can use the *file name cache* to make it easy to locate a
file by name, without having to remember exactly where it is located.
When typing a file name in the minibuffer, `C-tab`
(`file-cache-minibuffer-complete`) completes it using the file
name cache. If you repeat `C-tab`, that cycles through the
possible completions of what you had originally typed. Note that the
`C-tab` character cannot be typed on most text-only
terminals.

The file name cache does not fill up automatically. Instead, you
load file names into the cache using these commands:

`M-x file-cache-add-directory RET directory RET`: Add each file name in directory to the file name cache. `M-x file-cache-add-directory-using-find RET directory RET`: Add each file name in directory and all of its nested subdirectories to the file name cache. `M-x file-cache-add-directory-using-locate RET directory RET`: Add each file name in directory and all of its nested subdirectories to the file name cache, using `locate` to find them all. `M-x file-cache-add-directory-list RET variable RET`: Add each file name in each directory listed in variable to the file name cache. variable should be a Lisp variable such as `load-path` or `exec-path`, whose value is a list of directory names. `M-x file-cache-clear-cache RET`: Clear the cache; that is, remove all file names from it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC170) ] | [ [>](emacs.html#SEC172) ] | [ [<<](emacs.html#SEC112) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.16 Convenience Features for Finding Files

If you enable Recentf mode, with `M-x recentf-mode`, the
`` `File' `` menu includes a submenu containing a list of recently
opened files. `M-x recentf-save-list` saves the current
`recent-file-list` to a file, and `M-x recentf-edit-list`
edits it.

When Auto-image-file minor mode is enabled, visiting an image file
displays it as an image, not as text. Likewise, inserting an image
file into a buffer inserts it as an image. This works only when Emacs
can display the relevant image type. The variables
`image-file-name-extensions` or `image-file-name-regexps`
control which file names are recognized as containing images.

The `M-x ffap` command generalizes `find-file` with more
powerful heuristic defaults (see section [AC.28.3 Finding Files and URLs at Point](emacs.html#SEC471)), often based on the text at
point. Partial Completion mode offers other features extending
`find-file`, which can be used with `ffap`.
See section [E.3.4 Completion Options](emacs.html#SEC38).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC171) ] | [ [>](emacs.html#SEC173) ] | [ [<<](emacs.html#SEC110) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# N. Using Multiple Buffers

The text you are editing in Emacs resides in an object called a
*buffer*. Each time you visit a file, a buffer is created to hold the
file's text. Each time you invoke Dired, a buffer is created to hold the
directory listing. If you send a message with `C-x m`, a buffer named
`` `*mail*' `` is used to hold the text of the message. When you ask for a
command's documentation, that appears in a buffer called `` `*Help*' ``.

At any time, one and only one buffer is *current*. It is also
called the *selected buffer*. Often we say that a command operates on
"the buffer" as if there were only one; but really this means that the
command operates on the current buffer (most commands do).

When Emacs has multiple windows, each window has its own chosen
buffer and displays it; at any time, only one of the windows is
selected, and its chosen buffer is the current buffer. Each window's
mode line normally displays the name of the window's chosen buffer
(see section [O. Multiple Windows](emacs.html#SEC183)).

Each buffer has a name, which can be of any length, and you can select
any buffer by giving its name. Most buffers are made by visiting files,
and their names are derived from the files' names. But you can also create
an empty buffer with any name you want. A newly started Emacs has a buffer
named `` `*scratch*' `` which can be used for evaluating Lisp expressions in
Emacs. The distinction between upper and lower case matters in buffer
names.

Each buffer records individually what file it is visiting, whether it is
modified, and what major mode and minor modes are in effect in it
(see section [R. Major Modes](emacs.html#SEC227)). Any Emacs variable can be made *local to* a
particular buffer, meaning its value in that buffer can be different from
the value in other buffers. See section [AD.2.4 Local Variables](emacs.html#SEC485).

A buffer's size cannot be larger than some maximum, which is defined
by the largest buffer position representable by the *Emacs integer*
data type. This is because Emacs tracks buffer positions using that
data type. For 32-bit machines, the largest buffer size is 128
megabytes.

> |  |  |  |
> | --- | --- | --- |
> | [N.1 Creating and Selecting Buffers](emacs.html#SEC173) |  | Creating a new buffer or reselecting an old one. |
> | [N.2 Listing Existing Buffers](emacs.html#SEC174) |  | Getting a list of buffers that exist. |
> | [N.3 Miscellaneous Buffer Operations](emacs.html#SEC175) |  | Renaming; changing read-onlyness; copying text. |
> | [N.4 Killing Buffers](emacs.html#SEC176) |  | Killing buffers you no longer need. |
> | [N.5 Operating on Several Buffers](emacs.html#SEC177) |  | How to go through the list of all buffers and operate variously on several of them. |
> | [N.6 Indirect Buffers](emacs.html#SEC178) |  | An indirect buffer shares the text of another buffer. |
> | [N.7 Convenience Features and Customization of Buffer Handling](emacs.html#SEC179) |  | Convenience and customization features for buffer handling. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC172) ] | [ [>](emacs.html#SEC174) ] | [ [<<](emacs.html#SEC172) ] | [ [Up](emacs.html#SEC172) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## N.1 Creating and Selecting Buffers

`C-x b buffer RET`: Select or create a buffer named buffer (`switch-to-buffer`). `C-x 4 b buffer RET`: Similar, but select buffer in another window (`switch-to-buffer-other-window`). `C-x 5 b buffer RET`: Similar, but select buffer in a separate frame (`switch-to-buffer-other-frame`).

To select the buffer named bufname, type `C-x b bufname
RET`. This runs the command `switch-to-buffer` with argument
bufname. You can use completion on an abbreviation for the buffer
name you want (see section [E.3 Completion](emacs.html#SEC34)). An empty argument to `C-x b`
specifies the buffer that was current most recently among those not
now displayed in any window.

To select a buffer in a window other than the current one, type
`C-x 4 b bufname RET`. This runs the command
`switch-to-buffer-other-window` which displays the buffer
bufname in another window. By default, if displaying the buffer
causes two vertically adjacent windows to be displayed, the heights of
those windows are evened out; to countermand that and preserve the
window configuration, set the variable `even-window-heights` to
`nil`.

Similarly, `C-x 5 b buffer RET` runs the command
`switch-to-buffer-other-frame` which selects a buffer in another
frame.

You can control how certain buffers are handled by these commands by
customizing the variables `special-display-buffer-names`,
`special-display-regexps`, `same-window-buffer-names`, and
`same-window-regexps`. See [O.5 Forcing Display in the Same Window](emacs.html#SEC188), and
[P.11 Special Buffer Frames](emacs.html#SEC202), for more about these variables. In
addition, if the value of `display-buffer-reuse-frames` is
non-`nil`, and the buffer you want to switch to is already
displayed in some frame, Emacs will raise that frame.

Most buffers are created by visiting files, or by Emacs commands that
want to display some text, but you can also create a buffer explicitly
by typing `C-x b bufname RET`. This makes a new, empty
buffer that is not visiting any file, and selects it for editing. Such
buffers are used for making notes to yourself. If you try to save one,
you are asked for the file name to use. The new buffer's major mode is
determined by the value of `default-major-mode` (see section [R. Major Modes](emacs.html#SEC227)).

Note that `C-x C-f`, and any other command for visiting a file,
can also be used to switch to an existing file-visiting buffer.
See section [M.2 Visiting Files](emacs.html#SEC112).

Emacs uses buffer names that start with a space for internal purposes.
It treats these buffers specially in minor ways--for example, by
default they do not record undo information. It is best to avoid using
such buffer names yourself.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC173) ] | [ [>](emacs.html#SEC175) ] | [ [<<](emacs.html#SEC175) ] | [ [Up](emacs.html#SEC172) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## N.2 Listing Existing Buffers

`C-x C-b`: List the existing buffers (`list-buffers`).

To display a list of all the buffers that exist, type `C-x C-b`.
Each line in the list shows one buffer's name, major mode and visited
file. The buffers are listed in the order that they were current; the
buffers that were current most recently come first.

`` `*' `` at the beginning of a line indicates the buffer is "modified."
If several buffers are modified, it may be time to save some with `C-x s`
(see section [M.3 Saving Files](emacs.html#SEC113)). `` `%' `` indicates a read-only buffer. `` `.' `` marks the
current buffer. Here is an example of a buffer list:

|  |  |
| --- | --- |
|  | ```  MR Buffer         Size  Mode           File  -- ------         ----  ----           ---- .*  emacs.tex      383402 Texinfo       /u2/emacs/man/emacs.tex     *Help*         1287  Fundamental	     files.el       23076 Emacs-Lisp     /u2/emacs/lisp/files.el   % RMAIL          64042 RMAIL          /u/rms/RMAIL  *% man            747   Dired          /u2/emacs/man/		     net.emacs      343885 Fundamental   /u/rms/net.emacs     fileio.c       27691 C              /u2/emacs/src/fileio.c     NEWS           67340 Text           /u2/emacs/etc/NEWS     *scratch*	   0	 Lisp Interaction ``` |

Note that the buffer `` `*Help*' `` was made by a help request; it is
not visiting any file. The buffer `man` was made by Dired on the
directory `/u2/emacs/man/'. You can list only buffers that are
visiting files by giving the command a prefix; for instance, by typing
`C-u C-x C-b`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC174) ] | [ [>](emacs.html#SEC176) ] | [ [<<](emacs.html#SEC176) ] | [ [Up](emacs.html#SEC172) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## N.3 Miscellaneous Buffer Operations

`C-x C-q`: Toggle read-only status of buffer (`vc-toggle-read-only`). `M-x rename-buffer RET name RET`: Change the name of the current buffer. `M-x rename-uniquely`: Rename the current buffer by adding `` `<number>' `` to the end. `M-x view-buffer RET buffer RET`: Scroll through buffer buffer.

A buffer can be *read-only*, which means that commands to change
its contents are not allowed. The mode line indicates read-only
buffers with `` `%%' `` or `` `%*' `` near the left margin. Read-only
buffers are usually made by subsystems such as Dired and Rmail that
have special commands to operate on the text; also by visiting a file
whose access control says you cannot write it.

If you wish to make changes in a read-only buffer, use the command
`C-x C-q` (`vc-toggle-read-only`). It makes a read-only buffer
writable, and makes a writable buffer read-only. In most cases, this
works by setting the variable `buffer-read-only`, which has a local
value in each buffer and makes the buffer read-only if its value is
non-`nil`. If the file is maintained with version control,
`C-x C-q` works through the version control system to change the
read-only status of the file as well as the buffer. See section [M.7 Version Control](emacs.html#SEC127).

`M-x rename-buffer` changes the name of the current buffer. Specify
the new name as a minibuffer argument. There is no default. If you
specify a name that is in use for some other buffer, an error happens and
no renaming is done.

`M-x rename-uniquely` renames the current buffer to a similar name
with a numeric suffix added to make it both different and unique. This
command does not need an argument. It is useful for creating multiple
shell buffers: if you rename the `` `*Shell*' `` buffer, then do `M-x
shell` again, it makes a new shell buffer named `` `*Shell*' ``;
meanwhile, the old shell buffer continues to exist under its new name.
This method is also good for mail buffers, compilation buffers, and most
Emacs features that create special buffers with particular names.

`M-x view-buffer` is much like `M-x view-file` (see section [M.10 Miscellaneous File Operations](emacs.html#SEC165)) except that it examines an already existing Emacs buffer.
View mode provides commands for scrolling through the buffer
conveniently but not for changing it. When you exit View mode with
`q`, that switches back to the buffer (and the position) which was
previously displayed in the window. Alternatively, if you exit View
mode with `e`, the buffer and the value of point that resulted from
your perusal remain in effect.

The commands `M-x append-to-buffer` and `M-x insert-buffer`
can be used to copy text from one buffer to another. See section [H.9 Accumulating Text](emacs.html#SEC67).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC175) ] | [ [>](emacs.html#SEC177) ] | [ [<<](emacs.html#SEC177) ] | [ [Up](emacs.html#SEC172) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## N.4 Killing Buffers

If you continue an Emacs session for a while, you may accumulate a
large number of buffers. You may then find it convenient to *kill*
the buffers you no longer need. On most operating systems, killing a
buffer releases its space back to the operating system so that other
programs can use it. Here are some commands for killing buffers:

`C-x k bufname RET`: Kill buffer bufname (`kill-buffer`). `M-x kill-some-buffers`: Offer to kill each buffer, one by one.

`C-x k` (`kill-buffer`) kills one buffer, whose name you
specify in the minibuffer. The default, used if you type just
`RET` in the minibuffer, is to kill the current buffer. If you
kill the current buffer, another buffer becomes current: one that was
current in the recent past but is not displayed in any window now. If
you ask to kill a file-visiting buffer that is modified (has unsaved
editing), then you must confirm with `yes` before the buffer is
killed.

The command `M-x kill-some-buffers` asks about each buffer, one by
one. An answer of `y` means to kill the buffer. Killing the current
buffer or a buffer containing unsaved changes selects a new buffer or asks
for confirmation just like `kill-buffer`.

The buffer menu feature (see section [N.5 Operating on Several Buffers](emacs.html#SEC177)) is also convenient
for killing various buffers.

If you want to do something special every time a buffer is killed, you
can add hook functions to the hook `kill-buffer-hook` (see section [AD.2.3 Hooks](emacs.html#SEC484)).

If you run one Emacs session for a period of days, as many people do,
it can fill up with buffers that you used several days ago. The command
`M-x clean-buffer-list` is a convenient way to purge them; it kills
all the unmodified buffers that you have not used for a long time. An
ordinary buffer is killed if it has not been displayed for three days;
however, you can specify certain buffers that should never be killed
automatically, and others that should be killed if they have been unused
for a mere hour.

You can also have this buffer purging done for you, every day at
midnight, by enabling Midnight mode. Midnight mode operates each day at
midnight; at that time, it runs `clean-buffer-list`, or whichever
functions you have placed in the normal hook `midnight-hook`
(see section [AD.2.3 Hooks](emacs.html#SEC484)).

To enable Midnight mode, use the Customization buffer to set the
variable `midnight-mode` to `t`. See section [AD.2.2 Easy Customization Interface](emacs.html#SEC479).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC176) ] | [ [>](emacs.html#SEC178) ] | [ [<<](emacs.html#SEC178) ] | [ [Up](emacs.html#SEC172) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## N.5 Operating on Several Buffers

The *buffer-menu* facility is like a "Dired for buffers"; it allows
you to request operations on various Emacs buffers by editing an Emacs
buffer containing a list of them. You can save buffers, kill them
(here called *deleting* them, for consistency with Dired), or display
them.

`M-x buffer-menu`: Begin editing a buffer listing all Emacs buffers.

The command `buffer-menu` writes a list of all Emacs buffers into
the buffer `` `*Buffer List*' ``, and selects that buffer in Buffer Menu
mode. The buffer is read-only, and can be changed only through the
special commands described in this section. The usual Emacs cursor
motion commands can be used in the `` `*Buffer List*' `` buffer. The
following commands apply to the buffer described on the current line.

`d`: Request to delete (kill) the buffer, then move down. The request shows as a `` `D' `` on the line, before the buffer name. Requested deletions take place when you type the `x` command. `C-d`: Like `d` but move up afterwards instead of down. `s`: Request to save the buffer. The request shows as an `` `S' `` on the line. Requested saves take place when you type the `x` command. You may request both saving and deletion for the same buffer. `x`: Perform previously requested deletions and saves. `u`: Remove any request made for the current line, and move down. `DEL`: Move to previous line and remove any request made for that line.

The `d`, `C-d`, `s` and `u` commands to add or remove
flags also move down (or up) one line. They accept a numeric argument
as a repeat count.

These commands operate immediately on the buffer listed on the current
line:

`~`: Mark the buffer "unmodified." The command `~` does this immediately when you type it. `%`: Toggle the buffer's read-only flag. The command `%` does this immediately when you type it. `t`: Visit the buffer as a tags table. See section [W.2.4 Selecting a Tags Table](emacs.html#SEC338).

There are also commands to select another buffer or buffers:

`q`: Quit the buffer menu--immediately display the most recent formerly visible buffer in its place. `RET`: `f`: Immediately select this line's buffer in place of the `` `*Buffer List*' `` buffer. `o`: Immediately select this line's buffer in another window as if by `C-x 4 b`, leaving `` `*Buffer List*' `` visible. `C-o`: Immediately display this line's buffer in another window, but don't select the window. `1`: Immediately select this line's buffer in a full-screen window. `2`: Immediately set up two windows, with this line's buffer in one, and the previously current buffer (aside from the buffer `` `*Buffer List*' ``) in the other. `b`: Bury the buffer listed on this line. `m`: Mark this line's buffer to be displayed in another window if you exit with the `v` command. The request shows as a `` `>' `` at the beginning of the line. (A single buffer may not have both a delete request and a display request.) `v`: Immediately select this line's buffer, and also display in other windows any buffers previously marked with the `m` command. If you have not marked any buffers, this command is equivalent to `1`.

All that `buffer-menu` does directly is create and switch to a
suitable buffer, and turn on Buffer Menu mode. Everything else
described above is implemented by the special commands provided in
Buffer Menu mode. One consequence of this is that you can switch from
the `` `*Buffer List*' `` buffer to another Emacs buffer, and edit there.
You can reselect the `` `*Buffer List*' `` buffer later, to perform the
operations already requested, or you can kill it, or pay no further
attention to it.

The only difference between `buffer-menu` and `list-buffers`
is that `buffer-menu` switches to the `` `*Buffer List*' `` buffer
in the selected window; `list-buffers` displays it in another
window. If you run `list-buffers` (that is, type `C-x C-b`)
and select the buffer list manually, you can use all of the commands
described here.

The buffer `` `*Buffer List*' `` is not updated automatically when
buffers are created and killed; its contents are just text. If you have
created, deleted or renamed buffers, the way to update `` `*Buffer
List*' `` to show what you have done is to type `g`
(`revert-buffer`) or repeat the `buffer-menu` command.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC177) ] | [ [>](emacs.html#SEC179) ] | [ [<<](emacs.html#SEC179) ] | [ [Up](emacs.html#SEC172) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## N.6 Indirect Buffers

An *indirect buffer* shares the text of some other buffer, which
is called the *base buffer* of the indirect buffer. In some ways it
is the analogue, for buffers, of a symbolic link between files.

`M-x make-indirect-buffer RET base-buffer RET indirect-name RET`: Create an indirect buffer named indirect-name whose base buffer is base-buffer. `M-x clone-indirect-buffer RET`: Create an indirect buffer that is a twin copy of the current buffer. `C-x 4 c`: Create an indirect buffer that is a twin copy of the current buffer, and select it in another window (`clone-indirect-buffer-other-window`).

The text of the indirect buffer is always identical to the text of its
base buffer; changes made by editing either one are visible immediately
in the other. But in all other respects, the indirect buffer and its
base buffer are completely separate. They have different names,
different values of point, different narrowing, different markers,
different major modes, and different local variables.

An indirect buffer cannot visit a file, but its base buffer can. If
you try to save the indirect buffer, that actually works by saving the
base buffer. Killing the base buffer effectively kills the indirect
buffer, but killing an indirect buffer has no effect on its base buffer.

One way to use indirect buffers is to display multiple views of an
outline. See section [T.8.4 Viewing One Outline in Multiple Views](emacs.html#SEC250).

A quick and handy way to make an indirect buffer is with the command
`M-x clone-indirect-buffer`. It creates and selects an indirect
buffer whose base buffer is the current buffer. With a numeric
argument, it prompts for the name of the indirect buffer; otherwise it
defaults to the name of the current buffer, modifying it by adding a
`` `<n>' `` prefix if required. `C-x 4 c`
(`clone-indirect-buffer-other-window`) works like `M-x
clone-indirect-buffer`, but it selects the cloned buffer in another
window. These commands come in handy if you want to create new
`` `*info*' `` or `` `*Help*' `` buffers, for example.

The more general way is with the command `M-x
make-indirect-buffer`. It creates an indirect buffer from buffer
base-buffer, under the name indirect-name. It prompts for
both base-buffer and indirect-name using the minibuffer.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC178) ] | [ [>](emacs.html#SEC180) ] | [ [<<](emacs.html#SEC172) ] | [ [Up](emacs.html#SEC172) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## N.7 Convenience Features and Customization of Buffer Handling

This section describes several modes and features that make it more
convenient to switch between buffers.

> |  |  |  |
> | --- | --- | --- |
> | [N.7.1 Making Buffer Names Unique](emacs.html#SEC180) |  | Buffer names can contain directory parts. |
> | [N.7.2 Switching Between Buffers using Substrings](emacs.html#SEC181) |  | Switching between buffers with substrings. |
> | [N.7.3 Customizing Buffer Menus](emacs.html#SEC182) |  | Configurable buffer menu. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC179) ] | [ [>](emacs.html#SEC181) ] | [ [<<](emacs.html#SEC172) ] | [ [Up](emacs.html#SEC179) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### N.7.1 Making Buffer Names Unique

When several buffers visit identically-named files, Emacs must give
the buffers distinct names. The usual method for making buffer names
unique adds `` `<2>' ``, `` `<3>' ``, etc. to the end of the buffer
names (all but one of them).

Other methods work by adding parts of each file's directory to the
buffer name. To select one, customize the variable
`uniquify-buffer-name-style` (see section [AD.2.2 Easy Customization Interface](emacs.html#SEC479)).

For instance, the `forward` naming method puts part of the
directory name at the beginning of the buffer name; using this method,
buffers visiting `/u/mernst/tmp/Makefile' and
`/usr/projects/zaphod/Makefile' would be named
`` `tmp/Makefile' `` and `` `zaphod/Makefile' ``, respectively (instead
of `` `Makefile' `` and `` `Makefile<2>' ``).

By contrast, the `post-forward` naming method would call the
buffers `` `Makefile|tmp' `` and `` `Makefile|zaphod' ``, and the
`reverse` naming method would call them `` `Makefile\tmp' `` and
`` `Makefile\zaphod' ``. The nontrivial difference between
`post-forward` and `reverse` occurs when just one directory
name is not enough to distinguish two files; then `reverse` puts
the directory names in reverse order, so that `/top/middle/file'
becomes `` `file\middle\top' ``, while `post-forward` puts them in
forward order after the file name, as in `` `file|top/middle' ``.

Which rule to follow for putting the directory names in the buffer
name is not very important if you are going to *look* at the
buffer names before you type one. But as an experienced user, if you
know the rule, you won't have to look. And then you may find that one
rule or another is easier for you to remember and utilize fast.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC180) ] | [ [>](emacs.html#SEC182) ] | [ [<<](emacs.html#SEC182) ] | [ [Up](emacs.html#SEC179) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
