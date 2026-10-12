### 12.4 Customization [¶](#Customization-1)

All variables are properly documented elsewhere in this manual. This
section is designed to give general pointers on how to customize Gnus
for some quite common situations.

* [Slow/Expensive Connection](#Slow_002fExpensive-Connection)
* [Slow Terminal Connection](#Slow-Terminal-Connection)
* [Little Disk Space](#Little-Disk-Space)
* [Slow Machine](#Slow-Machine)

---

Next: [Slow Terminal Connection](#Slow-Terminal-Connection), Up: [Customization](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.4.1 Slow/Expensive Connection [¶](#Slow_002fExpensive-Connection-1)

If you run Emacs on a machine locally, and get your news from a machine
over some very thin strings, you want to cut down on the amount of data
Gnus has to get from the server.

`gnus-read-active-file`
:   Set this to `nil`, which will inhibit Gnus from requesting the
    entire active file from the server. This file is often very large. You
    also have to set `gnus-check-new-newsgroups` and
    `gnus-check-bogus-newsgroups` to `nil` to make sure that Gnus
    doesn’t suddenly decide to fetch the active file anyway.

`gnus-nov-is-evil` [¶](#index-gnus_002dnov_002dis_002devil)
:   Usually this one must *always* be `nil` (which is the
    default). If, for example, you wish to not use NOV
    (see [Terminology](#Terminology)) with the `nntp` back end (see [Crosspost Handling](#Crosspost-Handling)), set `nntp-nov-is-evil` to a non-`nil` value
    instead of setting this. But you normally do not need to set
    `nntp-nov-is-evil` since Gnus by itself will detect whether the
    NNTP server supports NOV. Anyway, grabbing article
    headers from the NNTP server will not be very fast if you tell
    Gnus not to use NOV.

    As the variables for the other back ends, there are
    `nndiary-nov-is-evil`, `nndir-nov-is-evil`,
    `nnfolder-nov-is-evil`, `nnml-nov-is-evil`, and
    `nnspool-nov-is-evil`. Note that a non-`nil` value for
    `gnus-nov-is-evil` overrides all those variables.

---

Next: [Little Disk Space](#Little-Disk-Space), Previous: [Slow/Expensive Connection](#Slow_002fExpensive-Connection), Up: [Customization](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.4.2 Slow Terminal Connection [¶](#Slow-Terminal-Connection-1)

Let’s say you use your home computer for dialing up the system that runs
Emacs and Gnus. If your modem is slow, you want to reduce (as much as
possible) the amount of data sent over the wires.

`gnus-auto-center-summary`
:   Set this to `nil` to inhibit Gnus from re-centering the summary
    buffer all the time. If it is `vertical`, do only vertical
    re-centering. If it is neither `nil` nor `vertical`, do both
    horizontal and vertical recentering.

`gnus-visible-headers`
:   Cut down on the headers included in the articles to the
    minimum. You can, in fact, make do without them altogether—most of the
    useful data is in the summary buffer, anyway. Set this variable to
    ‘`^NEVVVVER`’ or ‘`From:`’, or whatever you feel you need.

    Use the following to enable all the available hiding features:

    ```
    (setq gnus-treat-hide-headers 'head
          gnus-treat-hide-signature t
          gnus-treat-hide-citation t)
    ```

`gnus-use-full-window`
:   By setting this to `nil`, you can make all the windows smaller.
    While this doesn’t really cut down much generally, it means that you
    have to see smaller portions of articles before deciding that you didn’t
    want to read them anyway.

`gnus-thread-hide-subtree`
:   If this is non-`nil`, all threads in the summary buffer will be
    hidden initially.

`gnus-updated-mode-lines`
:   If this is `nil`, Gnus will not put information in the buffer mode
    lines, which might save some time.

---

Next: [Slow Machine](#Slow-Machine), Previous: [Slow Terminal Connection](#Slow-Terminal-Connection), Up: [Customization](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.4.3 Little Disk Space [¶](#Little-Disk-Space-1)

The startup files can get rather large, so you may want to cut their
sizes a bit if you are running out of space.

`gnus-save-newsrc-file`
:   If this is `nil`, Gnus will never save `.newsrc`—it will
    only save `.newsrc.eld`. This means that you will not be able to
    use any other newsreaders than Gnus. This variable is `t` by
    default.

`gnus-read-newsrc-file`
:   If this is `nil`, Gnus will never read `.newsrc`—it will
    only read `.newsrc.eld`. This means that you will not be able to
    use any other newsreaders than Gnus. This variable is `t` by
    default.

`gnus-save-killed-list`
:   If this is `nil`, Gnus will not save the list of dead groups. You
    should also set `gnus-check-new-newsgroups` to `ask-server`
    and `gnus-check-bogus-newsgroups` to `nil` if you set this
    variable to `nil`. This variable is `t` by default.

---

Previous: [Little Disk Space](#Little-Disk-Space), Up: [Customization](#Customization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.4.4 Slow Machine [¶](#Slow-Machine-1)

If you have a slow machine, or are just really impatient, there are a
few things you can do to make Gnus run faster.

Set `gnus-check-new-newsgroups` and
`gnus-check-bogus-newsgroups` to `nil` to make startup faster.

Set `gnus-show-threads`, `gnus-use-cross-reference` and
`gnus-nov-is-evil` to `nil` to make entering and exiting the
summary buffer faster. Also see [Slow/Expensive Connection](#Slow_002fExpensive-Connection).

---

Next: [Gnus Reference Guide](#Gnus-Reference-Guide), Previous: [Customization](#Customization), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.5 Troubleshooting [¶](#Troubleshooting-1)

Gnus works *so* well straight out of the box—I can’t imagine any
problems, really.

Ahem.

1. Make sure your computer is switched on.
2. Read the help group (`G h` in the group buffer) for a
   FAQ and a how-to.
3. Gnus works on many recursive structures, and in some extreme (and very
   rare) cases Gnus may recurse down “too deeply” and Emacs will beep at
   you. If this happens to you, set `max-lisp-eval-depth` to 2000 or
   something like that.

If all else fails, report the problem as a bug.

If you find a bug in Gnus, you can report it with the `M-x
gnus-bug` command. `M-x toggle-debug-on-error`, and send me the
backtrace. I will fix bugs, but I can only fix them if you send me a
precise description as to how to reproduce the bug.

You really can never be too detailed in a bug report. Always use the
`M-x gnus-bug` command when you make bug reports, even if it creates
a 10Kb mail each time you use it, and even if you have sent me your
environment 500 times before. I don’t care. I want the full info each
time.

It is also important to remember that I have no memory whatsoever. If
you send a bug report, and I send you a reply, and then you just send
back “No, it’s not! Moron!”, I will have no idea what you are
insulting me about. Always over-explain everything. It’s much easier
for all of us—if I don’t have all the information I need, I will just
mail you and ask for more info, and everything takes more time.

If the problem you’re seeing is very visual, and you can’t quite explain
it, copy the Emacs window to a file (with `xwd`, for instance), put
it somewhere it can be reached, and include the URL of the picture in
the bug report.

If you would like to contribute a patch to fix bugs or make
improvements, please produce the patch using ‘`diff -u`’.

If you want to debug your problem further before reporting, possibly
in order to solve the problem yourself and send a patch, you can use
edebug. Debugging Lisp code is documented in the Elisp manual
(see [Debugging Lisp Programs](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Debugging) in The GNU Emacs
Lisp Reference Manual). To get you started with edebug, consider if
you discover some weird behavior when pressing `c`, the first
step is to do `C-h k c` and click on the hyperlink in
the documentation buffer that leads you to the function definition,
then press `M-x edebug-defun RET` with point inside that function,
return to Gnus and press `c` to invoke the code. You will be
placed in the lisp buffer and can single step using `SPC` and
evaluate expressions using `M-:` or inspect variables using
`C-h v`, abort execution with `q`, and resume execution with
`c` or `g`.

Sometimes, a problem do not directly generate an Emacs Lisp error but
manifests itself by causing Gnus to be very slow. In these cases, you
can use `M-x toggle-debug-on-quit` and press `C-g` when things are
slow, and then try to analyze the backtrace (repeating the procedure
helps isolating the real problem areas).

A fancier approach is to use the elisp profiler, ELP. The profiler is
(or should be) fully documented elsewhere, but to get you started
there are a few steps that need to be followed. First, instrument the
part of Gnus you are interested in for profiling, e.g., `M-x
elp-instrument-package RET gnus` or `M-x elp-instrument-package
RET message`. Then perform the operation that is slow and press
`M-x elp-results`. You will then see which operations that takes
time, and can debug them further. If the entire operation takes much
longer than the time spent in the slowest function in the profiler
output, you probably profiled the wrong part of Gnus. To reset
profiling statistics, use `M-x elp-reset-all`. `M-x
elp-restore-all` is supposed to remove profiling, but given the
complexities and dynamic code generation in Gnus, it might not always
work perfectly.

If you just need help, you are better off asking on
‘`gnu.emacs.gnus`’. I’m not very helpful. You can also ask on
[the ding mailing list](mailto:ding@gnus.org). Write to
[ding-request@gnus.org](mailto:ding-request@gnus.org) to subscribe.

---

Next: [Emacs for Heathens](#Emacs-for-Heathens), Previous: [Troubleshooting](#Troubleshooting), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.6 Gnus Reference Guide [¶](#Gnus-Reference-Guide-1)

It is my hope that other people will figure out smart stuff that Gnus
can do, and that other people will write those smart things as well. To
facilitate that I thought it would be a good idea to describe the inner
workings of Gnus. And some of the not-so-inner workings, while I’m at
it.

You can never expect the internals of a program not to change, but I
will be defining (in some details) the interface between Gnus and its
back ends (this is written in stone), the format of the score files
(ditto), data structures (some are less likely to change than others)
and general methods of operation.

* [Gnus Utility Functions](#Gnus-Utility-Functions)
* [Back End Interface](#Back-End-Interface)
* [Score File Syntax](#Score-File-Syntax)
* [Headers](#Headers)
* [Ranges](#Ranges)
* [Group Info](#Group-Info)
* [Extended Interactive](#Extended-Interactive)
* [Various File Formats](#Various-File-Formats)

---

Next: [Back End Interface](#Back-End-Interface), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.1 Gnus Utility Functions [¶](#Gnus-Utility-Functions-1)

When writing small functions to be run from hooks (and stuff), it’s
vital to have access to the Gnus internal functions and variables.
Below is a list of the most common ones.

`gnus-newsgroup-name` [¶](#index-gnus_002dnewsgroup_002dname)
:   This variable holds the name of the current newsgroup.

`gnus-find-method-for-group` [¶](#index-gnus_002dfind_002dmethod_002dfor_002dgroup)
:   A function that returns the select method for group.

`gnus-group-real-name` [¶](#index-gnus_002dgroup_002dreal_002dname)
:   Takes a full (prefixed) Gnus group name, and returns the unprefixed
    name.

`gnus-group-prefixed-name` [¶](#index-gnus_002dgroup_002dprefixed_002dname)
:   Takes an unprefixed group name and a select method, and returns the full
    (prefixed) Gnus group name.

`gnus-get-info` [¶](#index-gnus_002dget_002dinfo)
:   Returns the group info list for group (see [Group Info](#Group-Info)).

`gnus-group-unread` [¶](#index-gnus_002dgroup_002dunread)
:   The number of unread articles in group, or `t` if that is
    unknown.

`gnus-active` [¶](#index-gnus_002dactive)
:   The active entry (i.e., a cons cell containing the lowest and highest
    article numbers) for group.

`gnus-set-active` [¶](#index-gnus_002dset_002dactive)
:   Set the active entry for group.

`gnus-add-current-to-buffer-list` [¶](#index-gnus_002dadd_002dcurrent_002dto_002dbuffer_002dlist)
:   Adds the current buffer to the list of buffers to be killed on Gnus
    exit.

`gnus-continuum-version` [¶](#index-gnus_002dcontinuum_002dversion)
:   Takes a Gnus version string as a parameter and returns a floating point
    number. Earlier versions will always get a lower number than later
    versions.

`gnus-group-read-only-p` [¶](#index-gnus_002dgroup_002dread_002donly_002dp)
:   Says whether group is read-only or not.

`gnus-news-group-p` [¶](#index-gnus_002dnews_002dgroup_002dp)
:   Says whether group came from a news back end.

`gnus-ephemeral-group-p` [¶](#index-gnus_002dephemeral_002dgroup_002dp)
:   Says whether group is ephemeral or not.

`gnus-server-to-method` [¶](#index-gnus_002dserver_002dto_002dmethod)
:   Returns the select method corresponding to server.

`gnus-server-equal` [¶](#index-gnus_002dserver_002dequal)
:   Says whether two virtual servers are essentially equal. For instance,
    two virtual servers may have server parameters in different order, but
    this function will consider them equal.

`gnus-group-native-p` [¶](#index-gnus_002dgroup_002dnative_002dp)
:   Says whether group is native or not.

`gnus-group-secondary-p` [¶](#index-gnus_002dgroup_002dsecondary_002dp)
:   Says whether group is secondary or not.

`gnus-group-foreign-p` [¶](#index-gnus_002dgroup_002dforeign_002dp)
:   Says whether group is foreign or not.

`gnus-group-find-parameter` [¶](#index-gnus_002dgroup_002dfind_002dparameter)
:   Returns the parameter list of group (see [Group Parameters](#Group-Parameters)).
    If given a second parameter, returns the value of that parameter for
    group.

`gnus-group-set-parameter` [¶](#index-gnus_002dgroup_002dset_002dparameter)
:   Takes three parameters; group, parameter and value.

`gnus-narrow-to-body` [¶](#index-gnus_002dnarrow_002dto_002dbody)
:   Narrows the current buffer to the body of the article.

`gnus-check-backend-function` [¶](#index-gnus_002dcheck_002dbackend_002dfunction)
:   Takes two parameters, function and group. If the back end
    group comes from supports function, return non-`nil`.

    ```
    (gnus-check-backend-function "request-scan" "nnml:misc")
    ⇒ t
    ```

`gnus-read-method` [¶](#index-gnus_002dread_002dmethod)
:   Prompts the user for a select method.

---

Next: [Score File Syntax](#Score-File-Syntax), Previous: [Gnus Utility Functions](#Gnus-Utility-Functions), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2 Back End Interface [¶](#Back-End-Interface-1)

Gnus doesn’t know anything about NNTP, spools, mail or virtual
groups. It only knows how to talk to *virtual servers*. A virtual
server is a *back end* and some *back end variables*. As examples
of the first, we have `nntp`, `nnspool` and `nnmbox`. As
examples of the latter we have `nntp-port-number` and
`nnmbox-directory`.

When Gnus asks for information from a back end—say `nntp`—on
something, it will normally include a virtual server name in the
function parameters. (If not, the back end should use the “current”
virtual server.) For instance, `nntp-request-list` takes a virtual
server as its only (optional) parameter. If this virtual server hasn’t
been opened, the function should fail.

Note that a virtual server name has no relation to some physical server
name. Take this example:

```
(nntp "odd-one"
      (nntp-address "ifi.uio.no")
      (nntp-port-number 4324))
```

Here the virtual server name is ‘`odd-one`’ while the name of
the physical server is ‘`ifi.uio.no`’.

The back ends should be able to switch between several virtual servers.
The standard back ends implement this by keeping an alist of virtual
server environments that they pull down/push up when needed.

There are two groups of interface functions: *required functions*,
which must be present, and *optional functions*, which Gnus will
always check for presence before attempting to call ’em.

All these functions are expected to return data in the buffer
`nntp-server-buffer` (‘ `*nntpd*`’), which is somewhat
unfortunately named, but we’ll have to live with it. When I talk about
*resulting data*, I always refer to the data in that buffer. When I
talk about *return value*, I talk about the function value returned by
the function call. Functions that fail should return `nil` as the
return value.

Some back ends could be said to be *server-forming* back ends, and
some might be said not to be. The latter are back ends that generally
only operate on one group at a time, and have no concept of “server”;
they have a group, and they deliver info on that group and nothing
more.

Gnus identifies each message by way of group name and article number. A
few remarks about these article numbers might be useful. First of all,
the numbers are positive integers. Secondly, it is normally not
possible for later articles to “re-use” older article numbers without
confusing Gnus. That is, if a group has ever contained a message
numbered 42, then no other message may get that number, or Gnus will get
mightily confused.[4](#FOOT4)
Third, article numbers must be assigned in order of arrival in the
group; this is not necessarily the same as the date of the message.

The previous paragraph already mentions all the “hard” restrictions that
article numbers must fulfill. But it seems that it might be useful to
assign *consecutive* article numbers, for Gnus gets quite confused
if there are holes in the article numbering sequence. However, due to
the “no-reuse” restriction, holes cannot be avoided altogether. It’s
also useful for the article numbers to start at 1 to avoid running out
of numbers as long as possible.

Note that by convention, back ends are named `nnsomething`, but
Gnus also comes with some `nnnotbackends`, such as
`nnheader.el`, `nnmail.el` and `nnoo.el`.

In the examples and definitions I will refer to the imaginary back end
`nnchoke`.

* [Required Back End Functions](#Required-Back-End-Functions)
* [Optional Back End Functions](#Optional-Back-End-Functions)
* [Error Messaging](#Error-Messaging)
* [Writing New Back Ends](#Writing-New-Back-Ends)
* [Hooking New Back Ends Into Gnus](#Hooking-New-Back-Ends-Into-Gnus)
* [Mail-like Back Ends](#Mail_002dlike-Back-Ends)
* [Web Feed Back Ends](#Web-Feed-Back-Ends)

---

Next: [Optional Back End Functions](#Optional-Back-End-Functions), Up: [Back End Interface](#Back-End-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2.1 Required Back End Functions [¶](#Required-Back-End-Functions-1)

`(nnchoke-retrieve-headers articles &optional group server fetch-old)`
:   articles is either a range of article numbers or a list of
    `Message-ID`s. Current back ends do not fully support either—only
    sequences (lists) of article numbers, and most back ends do not support
    retrieval of `Message-ID`s. But they should try for both.

    The result data should either be HEADs or NOV lines, and the result
    value should either be `headers` or `nov` to reflect this.
    This might later be expanded to `various`, which will be a mixture
    of HEADs and NOV lines, but this is currently not supported by Gnus.

    If fetch-old is non-`nil` it says to try fetching “extra
    headers”, in some meaning of the word. This is generally done by
    fetching (at most) fetch-old extra headers less than the smallest
    article number in `articles`, and filling the gaps as well. The
    presence of this parameter can be ignored if the back end finds it
    cumbersome to follow the request. If this is non-`nil` and not a
    number, do maximum fetches.

    Here’s an example HEAD:

    ```
    221 1056 Article retrieved.
    Path: ifi.uio.no!sturles
    From: sturles@ifi.uio.no (Sturle Sunde)
    Newsgroups: ifi.discussion
    Subject: Re: Something very droll
    Date: 27 Oct 1994 14:02:57 +0100
    Organization: Dept. of Informatics, University of Oslo, Norway
    Lines: 26
    Message-ID: <38o8e1$a0o@holmenkollen.ifi.uio.no>
    References: <38jdmq$4qu@visbur.ifi.uio.no>
    NNTP-Posting-Host: holmenkollen.ifi.uio.no
    .
    ```

    So a `headers` return value would imply that there’s a number of
    these in the data buffer.

    Here’s a BNF definition of such a buffer:

    ```
    headers        = *head
    head           = error / valid-head
    error-message  = [ "4" / "5" ] 2number " " <error message> eol
    valid-head     = valid-message *header "." eol
    valid-message  = "221 " <number> " Article retrieved." eol
    header         = <text> eol
    ```

    (The version of extended BNF used here is ABNF, the one used in Internet RFCs.
    See RFC 5234.)

    If the return value is `nov`, the data buffer should contain
    *network overview database* lines. These are basically fields
    separated by tabs.

    ```
    nov-buffer = *nov-line
    nov-line   = field 7*8[ <TAB> field ] eol
    field      = <text except TAB>
    ```

    For a closer look at what should be in those fields,
    see [Headers](#Headers).

`(nnchoke-open-server server &optional definitions)`
:   server is here the virtual server name. definitions is a
    list of `(VARIABLE VALUE)` pairs that define this virtual server.

    If the server can’t be opened, no error should be signaled. The back end
    may then choose to refuse further attempts at connecting to this
    server. In fact, it should do so.

    If the server is opened already, this function should return a
    non-`nil` value. There should be no data returned.

`(nnchoke-close-server &optional server)`
:   Close connection to server and free all resources connected
    to it. Return `nil` if the server couldn’t be closed for some
    reason.

    There should be no data returned.

`(nnchoke-request-close)`
:   Close connection to all servers and free all resources that the back end
    have reserved. All buffers that have been created by that back end
    should be killed. (Not the `nntp-server-buffer`, though.) This
    function is generally only called when Gnus is shutting down.

    There should be no data returned.

`(nnchoke-server-opened &optional server)`
:   If server is the current virtual server, and the connection to the
    physical server is alive, then this function should return a
    non-`nil` value. This function should under no circumstances
    attempt to reconnect to a server we have lost connection to.

    There should be no data returned.

`(nnchoke-status-message &optional server)`
:   This function should return the last error message from server.

    There should be no data returned.

`(nnchoke-request-article article &optional group server to-buffer)`
:   The result data from this function should be the article specified by
    article. This might either be a `Message-ID` or a number.
    It is optional whether to implement retrieval by `Message-ID`, but
    it would be nice if that were possible.

    If to-buffer is non-`nil`, the result data should be returned
    in this buffer instead of the normal data buffer. This is to make it
    possible to avoid copying large amounts of data from one buffer to
    another, while Gnus mainly requests articles to be inserted directly
    into its article buffer.

    If it is at all possible, this function should return a cons cell where
    the `car` is the group name the article was fetched from, and the `cdr` is
    the article number. This will enable Gnus to find out what the real
    group and article numbers are when fetching articles by
    `Message-ID`. If this isn’t possible, `t` should be returned
    on successful article retrieval.

`(nnchoke-request-group group &optional server fast info)`
:   Get data on group. This function also has the side effect of
    making group the current group.

    If fast, don’t bother to return useful data, just make group
    the current group.

    If info, it allows the backend to update the group info
    structure.

    Here’s an example of some result data and a definition of the same:

    ```
    211 56 1000 1059 ifi.discussion
    ```

    The first number is the status, which should be 211. Next is the
    total number of articles in the group, the lowest article number, the
    highest article number, and finally the group name. Note that the total
    number of articles may be less than one might think while just
    considering the highest and lowest article numbers, but some articles
    may have been canceled. Gnus just discards the total-number, so
    whether one should take the bother to generate it properly (if that is a
    problem) is left as an exercise to the reader. If the group contains no
    articles, the lowest article number should be reported as 1 and the
    highest as 0.

    ```
    group-status = [ error / info ] eol
    error        = [ "4" / "5" ] 2<number> " " <Error message>
    info         = "211 " 3* [ <number> " " ] <string>
    ```

`(nnchoke-close-group group &optional server)`
:   Close group and free any resources connected to it. This will be
    a no-op on most back ends.

    There should be no data returned.

`(nnchoke-request-list &optional server)`
:   Return a list of all groups available on server. And that means
    *all*.

    Here’s an example from a server that only carries two groups:

    ```
    ifi.test 0000002200 0000002000 y
    ifi.discussion 3324 3300 n
    ```

    On each line we have a group name, then the highest article number in
    that group, the lowest article number, and finally a flag. If the group
    contains no articles, the lowest article number should be reported as 1
    and the highest as 0.

    ```
    active-file = *active-line
    active-line = name " " <number> " " <number> " " flags eol
    name        = <string>
    flags       = "n" / "y" / "m" / "x" / "j" / "=" name
    ```

    The flag says whether the group is read-only (‘`n`’), is moderated
    (‘`m`’), is dead (‘`x`’), is aliased to some other group
    (‘`=other-group`’) or none of the above (‘`y`’).

`(nnchoke-request-post &optional server)`
:   This function should post the current buffer. It might return whether
    the posting was successful or not, but that’s not required. If, for
    instance, the posting is done asynchronously, it has generally not been
    completed by the time this function concludes. In that case, this
    function should set up some kind of sentinel to beep the user loud and
    clear if the posting could not be completed.

    There should be no result data from this function.

---

Next: [Error Messaging](#Error-Messaging), Previous: [Required Back End Functions](#Required-Back-End-Functions), Up: [Back End Interface](#Back-End-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2.2 Optional Back End Functions [¶](#Optional-Back-End-Functions-1)

`(nnchoke-retrieve-groups groups &optional server)`
:   groups is a list of groups, and this function should request data
    on all those groups. How it does it is of no concern to Gnus, but it
    should attempt to do this in a speedy fashion.

    The return value of this function can be either `active` or
    `group`, which says what the format of the result data is. The
    former is in the same format as the data from
    `nnchoke-request-list`, while the latter is a buffer full of lines
    in the same format as `nnchoke-request-group` gives.

    ```
    group-buffer = *active-line / *group-status
    ```

`(nnchoke-retrieve-group-data-early server &optional infos)`
:   This is for starting an early async retrieval from server to
    update infos, its current group info structure list. The return
    value is a token for the corresponding
    `nnchoke-finish-retrieve-group-infos`.

`(nnchoke-finish-retrieve-group-infos server infos data)`
:   This will be called later to conclude the async retrieval of
    `nnchoke-retrieve-group-data-early` – whose token is passed as
    data – and should update infos.

`(nnchoke-request-update-info group info &optional server)`
:   A Gnus group info (see [Group Info](#Group-Info)) is handed to the back end for
    alterations. This comes in handy if the back end really carries all
    the information (as is the case with virtual and imap groups). This
    function should destructively alter the info to suit its needs, and
    should return a non-`nil` value (exceptionally,
    `nntp-request-update-info` always returns `nil` not to waste
    the network resources).

    There should be no result data from this function.

`(nnchoke-request-type group &optional article)`
:   When the user issues commands for “sending news” (`F` in the
    summary buffer, for instance), Gnus has to know whether the article the
    user is following up on is news or mail. This function should return
    `news` if article in group is news, `mail` if it
    is mail and `unknown` if the type can’t be decided. (The
    article parameter is necessary in `nnvirtual` groups which
    might very well combine mail groups and news groups.) Both group
    and article may be `nil`.

    There should be no result data from this function.

`(nnchoke-request-set-mark group action &optional server)`
:   Set/remove/add marks on articles. Normally Gnus handles the article
    marks (such as read, ticked, expired etc.) internally, and store them in
    `~/.newsrc.eld`. Some back ends (such as IMAP) however carry
    all information about the articles on the server, so Gnus need to
    propagate the mark information to the server.

    action is a list of mark setting requests, having this format:

    ```
    (range action mark)
    ```

    range is a range of articles you wish to update marks on.
    action is `add` or `del`, used to add marks or remove
    marks (preserving all marks not mentioned). mark is a list of
    marks; where each mark is a symbol. Currently used marks are
    `read`, `tick`, `reply`, `expire`, `killed`,
    `dormant`, `save`, `download`, `unsend`, and
    `forward`, but your back end should, if possible, not limit
    itself to these.

    Given contradictory actions, the last action in the list should be the
    effective one. That is, if your action contains a request to add the
    `tick` mark on article 1 and, later in the list, a request to
    remove the mark on the same article, the mark should in fact be removed.

    An example action list:

    ```
    (((5 12 30) 'del '(tick))
     ((10 . 90) 'add '(read expire))
     ((92 94) 'del '(read)))
    ```

    The function should return a range of articles it wasn’t able to set the
    mark on (currently not used for anything).

    There should be no result data from this function.

`(nnchoke-request-update-mark group article mark)`
:   If the user tries to set a mark that the back end doesn’t like, this
    function may change the mark. Gnus will use whatever this function
    returns as the mark for article instead of the original
    mark. If the back end doesn’t care, it must return the original
    mark, and not `nil` or any other type of garbage.

    The only use for this I can see is what `nnvirtual` does with
    it—if a component group is auto-expirable, marking an article as read
    in the virtual group should result in the article being marked as
    expirable.

    There should be no result data from this function.

`(nnchoke-request-scan &optional group server)`
:   This function may be called at any time (by Gnus or anything else) to
    request that the back end check for incoming articles, in one way or
    another. A mail back end will typically read the spool file or query
    the POP server when this function is invoked. The
    group doesn’t have to be heeded—if the back end decides that
    it is too much work just scanning for a single group, it may do a
    total scan of all groups. It would be nice, however, to keep things
    local if that’s practical.

    There should be no result data from this function.

`(nnchoke-request-group-description group &optional server)`
:   The result data from this function should be a description of
    group.

    ```
    description-line = name <TAB> description eol
    name             = <string>
    description      = <text>
    ```

`(nnchoke-request-list-newsgroups &optional server)`
:   The result data from this function should be the description of all
    groups available on the server.

    ```
    description-buffer = *description-line
    ```

`(nnchoke-request-newgroups date &optional server)`
:   The result data from this function should be all groups that were
    created after ‘`date`’, which is in normal human-readable date format
    (i.e., the date format used in mail and news headers, and returned by
    the function `message-make-date` by default). The data should be
    in the active buffer format.

    It is okay for this function to return “too many” groups; some back ends
    might find it cheaper to return the full list of groups, rather than
    just the new groups. But don’t do this for back ends with many groups.
    Normally, if the user creates the groups herself, there won’t be too
    many groups, so `nnml` and the like are probably safe. But for
    back ends like `nntp`, where the groups have been created by the
    server, it is quite likely that there can be many groups.

`(nnchoke-request-create-group group &optional server)`
:   This function should create an empty group with name group.

    There should be no return data.

`(nnchoke-request-expire-articles articles &optional group server force)`
:   This function should run the expiry process on all articles in the
    articles range (which is currently a simple list of article
    numbers.) It is left up to the back end to decide how old articles
    should be before they are removed by this function. If force is
    non-`nil`, all articles should be deleted, no matter how new
    they are.

    This function should return a list of articles that it did not/was not
    able to delete.

    There should be no result data returned.

`(nnchoke-request-move-article article group server accept-form &optional last)`
:   This function should move article (which is a number) from
    group by calling accept-form.

    This function should ready the article in question for moving by
    removing any header lines it has added to the article, and generally
    should “tidy up” the article. Then it should `eval`
    accept-form in the buffer where the “tidy” article is. This
    will do the actual copying. If this `eval` returns a
    non-`nil` value, the article should be removed.

    If last is `nil`, that means that there is a high likelihood
    that there will be more requests issued shortly, so that allows some
    optimizations.

    The function should return a cons where the `car` is the group name and
    the `cdr` is the article number that the article was entered as.

    There should be no data returned.

`(nnchoke-request-accept-article group &optional server last)`
:   This function takes the current buffer and inserts it into group.
    If last in `nil`, that means that there will be more calls to
    this function in short order.

    The function should return a cons where the `car` is the group name and
    the `cdr` is the article number that the article was entered as.

    The group should exist before the back end is asked to accept the
    article for that group.

    There should be no data returned.

`(nnchoke-request-replace-article article group buffer)`
:   This function should remove article (which is a number) from
    group and insert buffer there instead.

    There should be no data returned.

`(nnchoke-request-delete-group group force &optional server)`
:   This function should delete group. If force, it should
    really delete all the articles in the group, and then delete the group
    itself. (If there is such a thing as “the group itself”.)

    There should be no data returned.

`(nnchoke-request-rename-group group new-name &optional server)`
:   This function should rename group into new-name. All
    articles in group should move to new-name.

    There should be no data returned.

---

Next: [Writing New Back Ends](#Writing-New-Back-Ends), Previous: [Optional Back End Functions](#Optional-Back-End-Functions), Up: [Back End Interface](#Back-End-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2.3 Error Messaging [¶](#Error-Messaging-1)

The back ends should use the function `nnheader-report` to report
error conditions—they should not raise errors when they aren’t able to
perform a request. The first argument to this function is the back end
symbol, and the rest are interpreted as arguments to `format` if
there are multiple of them, or just a string if there is one of them.
This function must always returns `nil`.

```
(nnheader-report 'nnchoke "You did something totally bogus")

(nnheader-report 'nnchoke "Could not request group %s" group)
```

Gnus, in turn, will call `nnheader-get-report` when it gets a
`nil` back from a server, and this function returns the most
recently reported message for the back end in question. This function
takes one argument—the server symbol.

Internally, these functions access back-end`-status-string`,
so the `nnchoke` back end will have its error message stored in
`nnchoke-status-string`.

---

Next: [Hooking New Back Ends Into Gnus](#Hooking-New-Back-Ends-Into-Gnus), Previous: [Error Messaging](#Error-Messaging), Up: [Back End Interface](#Back-End-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2.4 Writing New Back Ends [¶](#Writing-New-Back-Ends-1)

Many back ends are quite similar. `nnml` is just like
`nnspool`, but it allows you to edit the articles on the server.
`nnmh` is just like `nnml`, but it doesn’t use an active file,
and it doesn’t maintain overview databases. `nndir` is just like
`nnml`, but it has no concept of “groups”, and it doesn’t allow
editing articles.

It would make sense if it were possible to “inherit” functions from
back ends when writing new back ends. And, indeed, you can do that if you
want to. (You don’t have to if you don’t want to, of course.)

All the back ends declare their public variables and functions by using a
package called `nnoo`.

To inherit functions from other back ends (and allow other back ends to
inherit functions from the current back end), you should use the
following macros:

`nnoo-declare`
:   This macro declares the first parameter to be a child of the subsequent
    parameters. For instance:

    ```
    (nnoo-declare nndir
      nnml nnmh)
    ```

    `nndir` has declared here that it intends to inherit functions from
    both `nnml` and `nnmh`.

`defvoo`
:   This macro is equivalent to `defvar`, but registers the variable as
    a public server variable. Most state-oriented variables should be
    declared with `defvoo` instead of `defvar`.

    In addition to the normal `defvar` parameters, it takes a list of
    variables in the parent back ends to map the variable to when executing
    a function in those back ends.

    ```
    (defvoo nndir-directory nil
      "Where nndir will look for groups."
      nnml-current-directory nnmh-current-directory)
    ```

    This means that `nnml-current-directory` will be set to
    `nndir-directory` when an `nnml` function is called on behalf
    of `nndir`. (The same with `nnmh`.)

`nnoo-define-basics`
:   This macro defines some common functions that almost all back ends should
    have.

    ```
    (nnoo-define-basics nndir)
    ```

`deffoo`
:   This macro is just like `defun` and takes the same parameters. In
    addition to doing the normal `defun` things, it registers the
    function as being public so that other back ends can inherit it.

`nnoo-map-functions`
:   This macro allows mapping of functions from the current back end to
    functions from the parent back ends.

    ```
    (nnoo-map-functions nndir
      (nnml-retrieve-headers 0 nndir-current-group 0 0)
      (nnmh-request-article 0 nndir-current-group 0 0))
    ```

    This means that when `nndir-retrieve-headers` is called, the first,
    third, and fourth parameters will be passed on to
    `nnml-retrieve-headers`, while the second parameter is set to the
    value of `nndir-current-group`.

`nnoo-import`
:   This macro allows importing functions from back ends. It should be the
    last thing in the source file, since it will only define functions that
    haven’t already been defined.

    ```
    (nnoo-import nndir
      (nnmh
       nnmh-request-list
       nnmh-request-newgroups)
      (nnml))
    ```

    This means that calls to `nndir-request-list` should just be passed
    on to `nnmh-request-list`, while all public functions from
    `nnml` that haven’t been defined in `nndir` yet should be
    defined now.

Below is a slightly shortened version of the `nndir` back end.

```
;;; nndir.el — single directory newsgroup access for Gnus
;; Copyright (C) 1995,1996 Free Software Foundation, Inc.

;;; Code:

(require 'nnheader)
(require 'nnmh)
(require 'nnml)
(require 'nnoo)
(eval-when-compile (require 'cl))

(nnoo-declare nndir
  nnml nnmh)

(defvoo nndir-directory nil
  "Where nndir will look for groups."
  nnml-current-directory nnmh-current-directory)

(defvoo nndir-nov-is-evil nil
  "Non-nil means that nndir will never retrieve NOV headers."
  nnml-nov-is-evil)

(defvoo nndir-current-group ""
  nil
  nnml-current-group nnmh-current-group)
(defvoo nndir-top-directory nil nil nnml-directory nnmh-directory)
(defvoo nndir-get-new-mail nil nil nnml-get-new-mail nnmh-get-new-mail)

(defvoo nndir-status-string "" nil nnmh-status-string)

;;; Interface functions.

(nnoo-define-basics nndir)

(deffoo nndir-open-server (server &optional defs)
  (setq nndir-directory
        (or (cadr (assq 'nndir-directory defs))
            server))
  (unless (assq 'nndir-directory defs)
    (push `(nndir-directory ,server) defs))
  (push `(nndir-current-group
          ,(file-name-nondirectory
            (directory-file-name nndir-directory)))
        defs)
  (push `(nndir-top-directory
          ,(file-name-directory (directory-file-name nndir-directory)))
        defs)
  (nnoo-change-server 'nndir server defs))

(nnoo-map-functions nndir
  (nnml-retrieve-headers 0 nndir-current-group 0 0)
  (nnmh-request-article 0 nndir-current-group 0 0)
  (nnmh-request-group nndir-current-group 0 0)
  (nnmh-close-group nndir-current-group 0))

(nnoo-import nndir
  (nnmh
   nnmh-status-message
   nnmh-request-list
   nnmh-request-newgroups))

(provide 'nndir)
```

---

Next: [Mail-like Back Ends](#Mail_002dlike-Back-Ends), Previous: [Writing New Back Ends](#Writing-New-Back-Ends), Up: [Back End Interface](#Back-End-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2.5 Hooking New Back Ends Into Gnus [¶](#Hooking-New-Back-Ends-Into-Gnus-1)

Having Gnus start using your new back end is rather easy—you just
declare it with the `gnus-declare-backend` functions. This will
enter the back end into the `gnus-valid-select-methods` variable.

`gnus-declare-backend` takes two parameters—the back end name and
an arbitrary number of *abilities*.

Here’s an example:

```
(gnus-declare-backend "nnchoke" 'mail 'respool 'address)
```

The above line would then go in the `nnchoke.el` file.

The abilities can be:

`mail`
:   This is a mailish back end—followups should (probably) go via mail.

`post`
:   This is a newsish back end—followups should (probably) go via news.

`post-mail`
:   This back end supports both mail and news.

`none`
:   This is neither a post nor mail back end—it’s something completely
    different.

`respool`
:   It supports respooling—or rather, it is able to modify its source
    articles and groups.

`address`
:   The name of the server should be in the virtual server name. This is
    true for almost all back ends.

`prompt-address`
:   The user should be prompted for an address when doing commands like
    `B` in the group buffer. This is true for back ends like
    `nntp`, but not `nnmbox`, for instance.

---

Next: [Web Feed Back Ends](#Web-Feed-Back-Ends), Previous: [Hooking New Back Ends Into Gnus](#Hooking-New-Back-Ends-Into-Gnus), Up: [Back End Interface](#Back-End-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2.6 Mail-like Back Ends [¶](#Mail_002dlike-Back-Ends-1)

One of the things that separate the mail back ends from the rest of the
back ends is the heavy dependence by most of the mail back ends on
common functions in `nnmail.el`. For instance, here’s the
definition of `nnml-request-scan`:

```
(deffoo nnml-request-scan (&optional group server)
  (setq nnml-article-file-alist nil)
  (nnmail-get-new-mail 'nnml 'nnml-save-nov nnml-directory group))
```

It simply calls `nnmail-get-new-mail` with a few parameters,
and `nnmail` takes care of all the moving and splitting of the
mail.

This function takes four parameters.

method
:   This should be a symbol to designate which back end is responsible for
    the call.

exit-function
:   This function should be called after the splitting has been performed.

temp-directory
:   Where the temporary files should be stored.

group
:   This optional argument should be a group name if the splitting is to be
    performed for one group only.

`nnmail-get-new-mail` will call back-end`-save-mail` to
save each article. back-end`-active-number` will be called to
find the article number assigned to this article.

The function also uses the following variables:
back-end`-get-new-mail` (to see whether to get new mail for
this back end); and back-end`-group-alist` and
back-end`-active-file` to generate the new active file.
back-end`-group-alist` should be a group-active alist, like
this:

```
(("a-group" (1 . 10))
 ("some-group" (34 . 39)))
```

---

Previous: [Mail-like Back Ends](#Mail_002dlike-Back-Ends), Up: [Back End Interface](#Back-End-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.2.7 Web Feed Back Ends [¶](#Web-Feed-Back-Ends-1)

If you want to write a back end for a new type of web feed (RSS,
Atom), or some other type of feed, an “abstract” back end
(`nnfeed`) exists to enable the creation of such back ends with
minimal knowledge of Gnus.

`nnfeed` defines a generic parser, which uses functions stored in
server variables to parse information from a feed (`nnfeed`
itself doesn’t actually define any such functions though).

The data parsed from the feed is stored in server variables (and
stored per-feed in a sub-directory of `gnus-directory` whose name
corresponds to the name of the back end).

A Gnus back end interface is also defined, which uses the data parsed
from the feed.

Therefore, a new back end only needs to inherit from `nnfeed`,
define (fairly) generic parsing functions for the feed type and setup
the required server variables.

`nnfeed` was originally created to support Atom Syndication
Format feeds (see [Atom](#Atom)), but is very generic (as of writing this,
no standard web feed exists which can meaningfully use all the
features supported): it supports multiple groups contained in a single
feed, it allows for situations when the entire feed can’t (or
shouldn’t) be read ahead of time and it allows for very advanced
customization of the actual printing of articles from parsed data
(while providing a reasonably powerful default method).

Further implementation details are available in the documentation
strings of the various `nnfeed-*` server variables and
the commentary and other comments of `nnfeed.el`.

---

Next: [Headers](#Headers), Previous: [Back End Interface](#Back-End-Interface), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.3 Score File Syntax [¶](#Score-File-Syntax-1)

Score files are meant to be easily parsable, but yet extremely
malleable. It was decided that something that had the same read syntax
as an Emacs Lisp list would fit that spec.

Here’s a typical score file:

```
(("summary"
  ("Windows 95" -10000 nil s)
  ("Gnus"))
 ("from"
  ("Lars" -1000))
 (mark -100))
```

BNF definition of a score file:

```
score-file      = "" / "(" *element ")"
element         = rule / atom
rule            = string-rule / number-rule / date-rule
string-rule     = "(" quote string-header quote space *string-match ")"
number-rule     = "(" quote number-header quote space *number-match ")"
date-rule       = "(" quote date-header quote space *date-match ")"
quote           = <ascii 34>
string-header   = "subject" / "from" / "references" / "message-id" /
                  "xref" / "body" / "head" / "all" / "followup"
number-header   = "lines" / "chars"
date-header     = "date"
string-match    = "(" quote <string> quote [ "" / [ space score [ "" /
                  space date [ "" / [ space string-match-t ] ] ] ] ] ")"
score           = "nil" / <integer>
date            = "nil" / <natural number>
string-match-t  = "nil" / "s" / "substring" / "S" / "Substring" /
                  "r" / "regex" / "R" / "Regex" /
                  "e" / "exact" / "E" / "Exact" /
                  "f" / "fuzzy" / "F" / "Fuzzy"
number-match    = "(" <integer> [ "" / [ space score [ "" /
                  space date [ "" / [ space number-match-t ] ] ] ] ] ")"
number-match-t  = "nil" / "=" / "<" / ">" / ">=" / "<="
date-match      = "(" quote <string> quote [ "" / [ space score [ "" /
                  space date [ "" / [ space date-match-t ] ] ] ] ")"
date-match-t    = "nil" / "at" / "before" / "after"
atom            = "(" [ required-atom / optional-atom ] ")"
required-atom   = mark / expunge / mark-and-expunge / files /
                  exclude-files / read-only / touched
optional-atom   = adapt / local / eval
mark            = "mark" space nil-or-number
nil-or-number   = "nil" / <integer>
expunge         = "expunge" space nil-or-number
mark-and-expunge = "mark-and-expunge" space nil-or-number
files           = "files" *[ space <string> ]
exclude-files   = "exclude-files" *[ space <string> ]
read-only       = "read-only" [ space "nil" / space "t" ]
adapt        = "adapt" [ space "ignore" / space "t" / space adapt-rule ]
adapt-rule      = "(" *[ <string> *[ "(" <string> <integer> ")" ] ")"
local           = "local" *[ space "(" <string> space <form> ")" ]
eval            = "eval" space <form>
space           = *[ " " / <TAB> / <NEWLINE> ]
```

Any unrecognized elements in a score file should be ignored, but not
discarded.

As you can see, white space is needed, but the type and amount of white
space is irrelevant. This means that formatting of the score file is
left up to the programmer—if it’s simpler to just spew it all out on
one looong line, then that’s ok.

The meaning of the various atoms are explained elsewhere in this
manual (see [Score File Format](#Score-File-Format)).

---

Next: [Ranges](#Ranges), Previous: [Score File Syntax](#Score-File-Syntax), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.4 Headers [¶](#Headers-1)

Internally Gnus uses a format for storing article headers that
corresponds to the NOV format in a mysterious fashion. One could
almost suspect that the author looked at the NOV specification and
just shamelessly *stole* the entire thing, and one would be right.

*Header* is a severely overloaded term. “Header” is used in
RFC 5536 to talk about lines in the head of an article (e.g.,
`From`). It is used by many people as a synonym for
“head”—“the header and the body”. (That should be avoided, in my
opinion.) And Gnus uses a format internally that it calls “header”,
which is what I’m talking about here. This is a 9-element vector,
basically, with each header (ouch) having one slot.

These slots are, in order: `number`, `subject`, `from`,
`date`, `id`, `references`, `chars`, `lines`,
`xref`, and `extra`. There are macros for accessing and
setting these slots—they all have predictable names beginning with
`mail-header-` and `mail-header-set-`, respectively.

All these slots contain strings, except the `extra` slot, which
contains an alist of header/value pairs (see [To From Newsgroups](#To-From-Newsgroups)).

---

Next: [Group Info](#Group-Info), Previous: [Headers](#Headers), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.5 Ranges [¶](#Ranges-1)

GNUS introduced a concept that I found so useful that I’ve started
using it a lot and have elaborated on it greatly.

The question is simple: If you have a large amount of objects that are
identified by numbers (say, articles, to take a *wild* example)
that you want to qualify as being “included”, a normal sequence isn’t
very useful. (A 200,000 length sequence is a bit long-winded.)

The solution is as simple as the question: You just collapse the
sequence.

```
(1 2 3 4 5 6 10 11 12)
```

is transformed into

```
((1 . 6) (10 . 12))
```

To avoid having those nasty ‘`(13 . 13)`’ elements to denote a
lonesome object, a ‘`13`’ is a valid element:

```
((1 . 6) 7 (10 . 12))
```

This means that comparing two ranges to find out whether they are equal
is slightly tricky:

```
((1 . 5) 7 8 (10 . 12))
```

and

```
((1 . 5) (7 . 8) (10 . 12))
```

are equal. In fact, any non-descending list is a range:

```
(1 2 3 4 5)
```

is a perfectly valid range, although a pretty long-winded one. This is
also valid:

```
(1 . 5)
```

and is equal to the previous range.

Here’s a BNF definition of ranges. Of course, one must remember the
semantic requirement that the numbers are non-descending. (Any number
of repetition of the same number is allowed, but apt to disappear in
range handling.)

```
range           = simple-range / normal-range
simple-range    = "(" number " . " number ")"
normal-range    = "(" start-contents ")"
contents        = "" / simple-range *[ " " contents ] /
                  number *[ " " contents ]
```

Gnus currently uses ranges to keep track of read articles and article
marks. I plan on implementing a number of range operators in C if The
Powers That Be are willing to let me. (I haven’t asked yet, because I
need to do some more thinking on what operators I need to make life
totally range-based without ever having to convert back to normal
sequences.)

---

Next: [Extended Interactive](#Extended-Interactive), Previous: [Ranges](#Ranges), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.6 Group Info [¶](#Group-Info-1)

Gnus stores all permanent info on groups in a *group info* list.
This list is from three to six elements (or more) long and exhaustively
describes the group.

Here are two example group infos; one is a very simple group while the
second is a more complex one:

```
("no.group" 5 ((1 . 54324)))

("nnml:my.mail" 3 ((1 . 5) 9 (20 . 55))
                ((tick (15 . 19)) (replied 3 6 (19 . 3)))
                (nnml "")
                ((auto-expire . t) (to-address . "ding@gnus.org")))
```

The first element is the *group name*—as Gnus knows the group,
anyway. The second element is the *subscription level*, which
normally is a small integer. (It can also be the *rank*, which is a
cons cell where the `car` is the level and the `cdr` is the
score.) The third element is a list of ranges of read articles. The
fourth element is a list of lists of article marks of various kinds.
The fifth element is the select method (or virtual server, if you like).
The sixth element is a list of *group parameters*, which is what
this section is about.

Any of the last three elements may be missing if they are not required.
In fact, the vast majority of groups will normally only have the first
three elements, which saves quite a lot of cons cells.

Here’s a BNF definition of the group info format:

```
info          = "(" group space ralevel space read
                [ "" / [ space marks-list [ "" / [ space method [ "" /
                space parameters ] ] ] ] ] ")"
group         = quote <string> quote
ralevel       = rank / level
level         = <integer in the range of 1 to inf>
rank          = "(" level "." score ")"
score         = <integer in the range of 1 to inf>
read          = range
marks-lists   = nil / "(" *marks ")"
marks         = "(" <string> range ")"
method        = "(" <string> *elisp-forms ")"
parameters    = "(" *elisp-forms ")"
```

Actually that ‘`marks`’ rule is a fib. A ‘`marks`’ is a
‘`<string>`’ consed on to a ‘`range`’, but that’s a bitch to say
in pseudo-BNF.

If you have a Gnus info and want to access the elements, Gnus offers a
series of macros for getting/setting these elements.

`gnus-info-group` [¶](#index-gnus_002dinfo_002dgroup)

`gnus-info-set-group`
:   Get/set the group name.

`gnus-info-rank` [¶](#index-gnus_002dinfo_002drank)

`gnus-info-set-rank`
:   Get/set the group rank (see [Group Score](#Group-Score)).

`gnus-info-level` [¶](#index-gnus_002dinfo_002dlevel)

`gnus-info-set-level`
:   Get/set the group level.

`gnus-info-score` [¶](#index-gnus_002dinfo_002dscore)

`gnus-info-set-score`
:   Get/set the group score (see [Group Score](#Group-Score)).

`gnus-info-read` [¶](#index-gnus_002dinfo_002dread)

`gnus-info-set-read`
:   Get/set the ranges of read articles.

`gnus-info-marks` [¶](#index-gnus_002dinfo_002dmarks)

`gnus-info-set-marks`
:   Get/set the lists of ranges of marked articles.

`gnus-info-method` [¶](#index-gnus_002dinfo_002dmethod)

`gnus-info-set-method`
:   Get/set the group select method.

`gnus-info-params` [¶](#index-gnus_002dinfo_002dparams)

`gnus-info-set-params`
:   Get/set the group parameters.

All the getter functions take one parameter—the info list. The setter
functions take two parameters—the info list and the new value.

The last three elements in the group info aren’t mandatory, so it may be
necessary to extend the group info before setting the element. If this
is necessary, you can just pass on a non-`nil` third parameter to
the three final setter functions to have this happen automatically.

---

Next: [Various File Formats](#Various-File-Formats), Previous: [Group Info](#Group-Info), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.7 Extended Interactive [¶](#Extended-Interactive-1)

Gnus extends the standard Emacs `interactive` specification
slightly to allow easy use of the symbolic prefix (see [Symbolic Prefixes](#Symbolic-Prefixes)). Here’s an example of how this is used:

```
(defun gnus-summary-increase-score (&optional score symp)
  (interactive (gnus-interactive "P\ny"))
  ...
  )
```

The best thing to do would have been to implement
`gnus-interactive` as a macro which would have returned an
`interactive` form, but this isn’t possible since Emacs checks
whether a function is interactive or not by simply doing an `assq`
on the lambda form. So, instead we have `gnus-interactive`
function that takes a string and returns values that are usable to
`interactive`.

This function accepts (almost) all normal `interactive` specs, but
adds a few more.

‘`y`’ [¶](#index-gnus_002dcurrent_002dprefix_002dsymbol)
:   The current symbolic prefix—the `gnus-current-prefix-symbol`
    variable.

‘`Y`’ [¶](#index-gnus_002dcurrent_002dprefix_002dsymbols)
:   A list of the current symbolic prefixes—the
    `gnus-current-prefix-symbol` variable.

‘`A`’
:   The current article number—the `gnus-summary-article-number`
    function.

‘`H`’
:   The current article header—the `gnus-summary-article-header`
    function.

‘`g`’
:   The current group name—the `gnus-group-group-name`
    function.

---

Previous: [Extended Interactive](#Extended-Interactive), Up: [Gnus Reference Guide](#Gnus-Reference-Guide)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.8 Various File Formats [¶](#Various-File-Formats-1)

* [Active File Format](#Active-File-Format)
* [Newsgroups File Format](#Newsgroups-File-Format)

---

Next: [Newsgroups File Format](#Newsgroups-File-Format), Up: [Various File Formats](#Various-File-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.8.1 Active File Format [¶](#Active-File-Format-1)

The active file lists all groups available on the server in
question. It also lists the highest and lowest current article numbers
in each group.

Here’s an excerpt from a typical active file:

```
soc.motss 296030 293865 y
alt.binaries.pictures.fractals 3922 3913 n
comp.sources.unix 1605 1593 m
comp.binaries.ibm.pc 5097 5089 y
no.general 1000 900 y
```

Here’s a pseudo-BNF definition of this file:

```
active      = *group-line
group-line  = group spc high-number spc low-number spc flag <NEWLINE>
group       = <non-white-space string>
spc         = " "
high-number = <non-negative integer>
low-number  = <positive integer>
flag        = "y" / "n" / "m" / "j" / "x" / "=" group
```

For a full description of this file, see the manual pages for
‘`innd`’, in particular ‘`active(5)`’.

---

Previous: [Active File Format](#Active-File-Format), Up: [Various File Formats](#Various-File-Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.6.8.2 Newsgroups File Format [¶](#Newsgroups-File-Format-1)

The newsgroups file lists groups along with their descriptions. Not all
groups on the server have to be listed, and not all groups in the file
have to exist on the server. The file is meant purely as information to
the user.

The format is quite simple; a group name, a tab, and the description.
Here’s the definition:

```
newsgroups    = *line
line          = group tab description <NEWLINE>
group         = <non-white-space string>
tab           = <TAB>
description   = <string>
```

---

Next: [Frequently Asked Questions](#Frequently-Asked-Questions), Previous: [Gnus Reference Guide](#Gnus-Reference-Guide), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 12.7 Emacs for Heathens [¶](#Emacs-for-Heathens-1)

Believe it or not, but some people who use Gnus haven’t really used
Emacs much before they embarked on their journey on the Gnus Love Boat.
If you are one of those unfortunates to whom “`C-M-a`”, “kill the
region”, and “set `gnus-flargblossen` to an alist where the key
is a regexp that is used for matching on the group name” are magical
phrases with little or no meaning, then this appendix is for you. If
you are already familiar with Emacs, just ignore this and go fondle your
cat instead.

* [Keystrokes](#Keystrokes)
* [Emacs Lisp](#Emacs-Lisp)

---

Next: [Emacs Lisp](#Emacs-Lisp), Up: [Emacs for Heathens](#Emacs-for-Heathens)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.7.1 Keystrokes [¶](#Keystrokes-1)

* Q: What is an experienced Emacs user?
* A: A person who wishes that the terminal had pedals.

Yes, when you use Emacs, you are apt to use the control key, the shift
key and the meta key a lot. This is very annoying to some people
(notably `vi`le users), and the rest of us just love the hell out
of it. Just give up and submit. Emacs really does stand for
“Escape-Meta-Alt-Control-Shift”, and not “Editing Macros”, as you
may have heard from other disreputable sources (like the Emacs author).

The shift keys are normally located near your pinky fingers, and are
normally used to get capital letters and stuff. You probably use it all
the time. The control key is normally marked “CTRL” or something like
that. The meta key is, funnily enough, never marked as such on any
keyboard. The one I’m currently at has a key that’s marked “Alt”,
which is the meta key on this keyboard. It’s usually located somewhere
to the left hand side of the keyboard, usually on the bottom row.

Now, us Emacs people don’t say “press the meta-control-m key”,
because that’s just too inconvenient. We say “press the `C-M-m`
key”. `M-` is the prefix that means “meta” and “C-” is the
prefix that means “control”. So “press `C-k`” means “press
down the control key, and hold it down while you press `k`”.
“Press `C-M-k`” means “press down and hold down the meta key and
the control key and then press `k`”. Simple, ay?

This is somewhat complicated by the fact that not all keyboards have a
meta key. In that case you can use the “escape” key. Then `M-k`
means “press escape, release escape, press `k`”. That’s much more
work than if you have a meta key, so if that’s the case, I respectfully
suggest you get a real keyboard with a meta key. You can’t live without
it.

---

Previous: [Keystrokes](#Keystrokes), Up: [Emacs for Heathens](#Emacs-for-Heathens)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 12.7.2 Emacs Lisp [¶](#Emacs-Lisp-1)

Emacs is the King of Editors because it’s really a Lisp interpreter.
Each and every key you tap runs some Emacs Lisp code snippet, and since
Emacs Lisp is an interpreted language, that means that you can configure
any key to run any arbitrary code. You just, like, do it.

Gnus is written in Emacs Lisp, and is run as a bunch of interpreted
functions. (These are byte-compiled for speed, but it’s still
interpreted.) If you decide that you don’t like the way Gnus does
certain things, it’s trivial to have it do something a different way.
(Well, at least if you know how to write Lisp code.) However, that’s
beyond the scope of this manual, so we are simply going to talk about
some common constructs that you normally use in your `~/.gnus.el`
file to customize Gnus. (You can also use the `~/.emacs` file, but
in order to set things of Gnus up, it is much better to use the
`~/.gnus.el` file, See [Startup Files](#Startup-Files).)

If you want to set the variable `gnus-florgbnize` to four (4), you
write the following:

```
(setq gnus-florgbnize 4)
```

This function (really “special form”) `setq` is the one that can
set a variable to some value. This is really all you need to know. Now
you can go and fill your `~/.gnus.el` file with lots of these to
change how Gnus works.

If you have put that thing in your `~/.gnus.el` file, it will be
read and `eval`ed (which is Lisp-ese for “run”) the next time you
start Gnus. If you want to change the variable right away, simply say
`C-x C-e` after the closing parenthesis. That will `eval` the
previous “form”, which is a simple `setq` statement here.

Go ahead—just try it, if you’re located at your Emacs. After you
`C-x C-e`, you will see ‘`4`’ appear in the echo area, which
is the return value of the form you `eval`ed.

Some pitfalls:

If the manual says “set `gnus-read-active-file` to `some`”,
that means:

```
(setq gnus-read-active-file 'some)
```

On the other hand, if the manual says “set `gnus-nntp-server-file` to
‘`/etc/nntpserver`’”, that means:

```
(setq gnus-nntp-server-file "/etc/nntpserver")
```

So be careful not to mix up strings (the latter) with symbols (the
former). The manual is unambiguous, but it can be confusing.

---

Previous: [Emacs for Heathens](#Emacs-for-Heathens), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
