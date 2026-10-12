### 10.19 Spam Package [¶](#Spam-Package-1)

The Spam package provides Gnus with a centralized mechanism for
detecting and filtering spam. It filters new mail, and processes
messages according to whether they are spam or ham. (*Ham* is the
name used throughout this manual to indicate non-spam messages.)

* [Spam Package Introduction](#Spam-Package-Introduction)
* [Filtering Incoming Mail](#Filtering-Incoming-Mail)
* [Detecting Spam in Groups](#Detecting-Spam-in-Groups)
* [Spam and Ham Processors](#Spam-and-Ham-Processors)
* [Spam Package Configuration Examples](#Spam-Package-Configuration-Examples)
* [Spam Back Ends](#Spam-Back-Ends)
* [Extending the Spam package](#Extending-the-Spam-package)
* [Spam Statistics Package](#Spam-Statistics-Package)

---

Next: [Filtering Incoming Mail](#Filtering-Incoming-Mail), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.1 Spam Package Introduction [¶](#Spam-Package-Introduction-1)

You must read this section to understand how the Spam package works.
Do not skip, speed-read, or glance through this section.

Make sure you read the section on the `spam.el` sequence of
events. See [Extending the Spam package](#Extending-the-Spam-package).

To use the Spam package, you **must** first run the function
`spam-initialize`:

```
(spam-initialize)
```

This autoloads `spam.el` and installs the various hooks necessary
to let the Spam package do its job. In order to make use of the Spam
package, you have to set up certain group parameters and variables,
which we will describe below. All of the variables controlling the
Spam package can be found in the ‘`spam`’ customization group.

There are two “contact points” between the Spam package and the rest
of Gnus: checking new mail for spam, and leaving a group.

Checking new mail for spam is done in one of two ways: while splitting
incoming mail, or when you enter a group.

The first way, checking for spam while splitting incoming mail, is
suited to mail back ends such as `nnml` or `nnimap`, where
new mail appears in a single spool file. The Spam package processes
incoming mail, and sends mail considered to be spam to a designated
“spam” group. See [Filtering Incoming Mail](#Filtering-Incoming-Mail).

The second way is suited to back ends such as `nntp`, which have
no incoming mail spool, or back ends where the server is in charge of
splitting incoming mail. In this case, when you enter a Gnus group,
the unseen or unread messages in that group are checked for spam.
Detected spam messages are marked as spam. See [Detecting Spam in Groups](#Detecting-Spam-in-Groups).

In either case, you have to tell the Spam package what method to use
to detect spam messages. There are several methods, or *spam back
ends* (not to be confused with Gnus back ends!) to choose from: spam
“blacklists” and “whitelists”, dictionary-based filters, and so
forth. See [Spam Back Ends](#Spam-Back-Ends).

In the Gnus summary buffer, messages that have been identified as spam
always appear with a ‘`$`’ symbol.

The Spam package divides Gnus groups into three categories: ham
groups, spam groups, and unclassified groups. You should mark each of
the groups you subscribe to as either a ham group or a spam group,
using the `spam-contents` group parameter (see [Group Parameters](#Group-Parameters)). Spam groups have a special property: when you enter a
spam group, all unseen articles are marked as spam. Thus, mail split
into a spam group is automatically marked as spam.

Identifying spam messages is only half of the Spam package’s job. The
second half comes into play whenever you exit a group buffer. At this
point, the Spam package does several things:

First, it calls *spam and ham processors* to process the articles
according to whether they are spam or ham. There is a pair of spam
and ham processors associated with each spam back end, and what the
processors do depends on the back end. At present, the main role of
spam and ham processors is for dictionary-based spam filters: they add
the contents of the messages in the group to the filter’s dictionary,
to improve its ability to detect future spam. The `spam-process`
group parameter specifies what spam processors to use. See [Spam and Ham Processors](#Spam-and-Ham-Processors).

If the spam filter failed to mark a spam message, you can mark it
yourself, so that the message is processed as spam when you exit the
group:

`$` [¶](#index-_0024-_0028Summary_0029)

`M-d`

`M s x`

`S x`
:   Mark current article as spam, showing it with the ‘`$`’ mark
    (`gnus-summary-mark-as-spam`).

Similarly, you can unmark an article if it has been erroneously marked
as spam. See [Setting Marks](#Setting-Marks).

Normally, a ham message found in a non-ham group is not processed as
ham—the rationale is that it should be moved into a ham group for
further processing (see below). However, you can force these articles
to be processed as ham by setting
`spam-process-ham-in-spam-groups` and
`spam-process-ham-in-nonham-groups`.

The second thing that the Spam package does when you exit a group is
to move ham articles out of spam groups, and spam articles out of ham
groups. Ham in a spam group is moved to the group specified by the
variable `gnus-ham-process-destinations`, or the group parameter
`ham-process-destination`. Spam in a ham group is moved to the
group specified by the variable `gnus-spam-process-destinations`,
or the group parameter `spam-process-destination`. If these
variables are not set, the articles are left in their current group.
If an article cannot be moved (e.g., with a read-only backend such
as NNTP), it is copied.

If an article is moved to another group, it is processed again when
you visit the new group. Normally, this is not a problem, but if you
want each article to be processed only once, load the
`gnus-registry.el` package and set the variable
`spam-log-to-registry` to `t`. See [Spam Package Configuration Examples](#Spam-Package-Configuration-Examples).

Normally, spam groups ignore `gnus-spam-process-destinations`.
However, if you set `spam-move-spam-nonspam-groups-only` to
`nil`, spam will also be moved out of spam groups, depending on
the `spam-process-destination` parameter.

The final thing the Spam package does is to mark spam articles as
expired, which is usually the right thing to do.

If all this seems confusing, don’t worry. Soon it will be as natural
as typing Lisp one-liners on a neural interface… err, sorry, that’s
50 years in the future yet. Just trust us, it’s not so bad.

---

Next: [Detecting Spam in Groups](#Detecting-Spam-in-Groups), Previous: [Spam Package Introduction](#Spam-Package-Introduction), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.2 Filtering Incoming Mail [¶](#Filtering-Incoming-Mail-1)

To use the Spam package to filter incoming mail, you must first set up
fancy mail splitting. See [Fancy Mail Splitting](#Fancy-Mail-Splitting). The Spam package
defines a special splitting function that you can add to your fancy
split variable (either `nnmail-split-fancy` or
`nnimap-split-fancy`, depending on your mail back end):

```
(: spam-split)
```

The `spam-split` function scans incoming mail according to your
chosen spam back end(s), and sends messages identified as spam to a
spam group. By default, the spam group is a group named ‘`spam`’,
but you can change this by customizing `spam-split-group`. Make
sure the contents of `spam-split-group` are an unqualified group
name. For instance, in an `nnimap` server ‘`your-server`’,
the value ‘`spam`’ means ‘`nnimap+your-server:spam`’. The value
‘`nnimap+server:spam`’ is therefore wrong—it gives the group
‘`nnimap+your-server:nnimap+server:spam`’.

`spam-split` does not modify the contents of messages in any way.

Note for IMAP users: if you use the `spam-check-bogofilter`,
`spam-check-ifile`, and `spam-check-stat` spam back ends,
you should also set the variable `nnimap-split-download-body` to
`t`. See [Client-Side IMAP Splitting](#Client_002dSide-IMAP-Splitting).

You have to specify one or more spam back ends for `spam-split`
to use, by setting the `spam-use-*` variables. See [Spam Back Ends](#Spam-Back-Ends). Normally, `spam-split` simply uses all the spam back ends
you enabled in this way. However, you can tell `spam-split` to
use only some of them. Why this is useful? Suppose you are using the
`spam-use-regex-headers` and `spam-use-blackholes` spam back
ends, and the following split rule:

```
 nnimap-split-fancy '(|
                      (any "ding" "ding")
                      (: spam-split)
                      ;; default mailbox
                      "mail")
```

The problem is that you want all ding messages to make it to the ding
folder. But that will let obvious spam (for example, spam detected by
SpamAssassin, and `spam-use-regex-headers`) through, when it’s
sent to the ding list. On the other hand, some messages to the ding
list are from a mail server in the blackhole list, so the invocation
of `spam-split` can’t be before the ding rule.

The solution is to let SpamAssassin headers supersede ding rules, and
perform the other `spam-split` rules (including a second
invocation of the regex-headers check) after the ding rule. This is
done by passing a parameter to `spam-split`:

```
nnimap-split-fancy
      '(|
        ;; spam detected by spam-use-regex-headers goes to ‘regex-spam’
        (: spam-split "regex-spam" 'spam-use-regex-headers)
        (any "ding" "ding")
        ;; all other spam detected by spam-split goes to spam-split-group
        (: spam-split)
        ;; default mailbox
        "mail")
```

This lets you invoke specific `spam-split` checks depending on
your particular needs, and target the results of those checks to a
particular spam group. You don’t have to throw all mail into all the
spam tests. Another reason why this is nice is that messages to
mailing lists you have rules for don’t have to have resource-intensive
blackhole checks performed on them. You could also specify different
spam checks for your nnmail split vs. your nnimap split. Go crazy.

You should set the `spam-use-*` variables for whatever spam back
ends you intend to use. The reason is that when loading
`spam.el`, some conditional loading is done depending on what
`spam-use-xyz` variables you have set. See [Spam Back Ends](#Spam-Back-Ends).

---

Next: [Spam and Ham Processors](#Spam-and-Ham-Processors), Previous: [Filtering Incoming Mail](#Filtering-Incoming-Mail), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.3 Detecting Spam in Groups [¶](#Detecting-Spam-in-Groups-1)

To detect spam when visiting a group, set the group’s
`spam-autodetect` and `spam-autodetect-methods` group
parameters. These are accessible with `G c` or `G p`, as
usual (see [Group Parameters](#Group-Parameters)).

You should set the `spam-use-*` variables for whatever spam back
ends you intend to use. The reason is that when loading
`spam.el`, some conditional loading is done depending on what
`spam-use-xyz` variables you have set.

By default, only unseen articles are processed for spam. You can
force Gnus to recheck all messages in the group by setting the
variable `spam-autodetect-recheck-messages` to `t`.

If you use the `spam-autodetect` method of checking for spam, you
can specify different spam detection methods for different groups.
For instance, the ‘`ding`’ group may have `spam-use-BBDB` as
the autodetection method, while the ‘`suspect`’ group may have the
`spam-use-blacklist` and `spam-use-bogofilter` methods
enabled. Unlike with `spam-split`, you don’t have any control
over the *sequence* of checks, but this is probably unimportant.

---

Next: [Spam Package Configuration Examples](#Spam-Package-Configuration-Examples), Previous: [Detecting Spam in Groups](#Detecting-Spam-in-Groups), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.4 Spam and Ham Processors [¶](#Spam-and-Ham-Processors-1)

Spam and ham processors specify special actions to take when you exit
a group buffer. Spam processors act on spam messages, and ham
processors on ham messages. At present, the main role of these
processors is to update the dictionaries of dictionary-based spam back
ends such as Bogofilter (see [Bogofilter](#Bogofilter)) and the Spam Statistics
package (see [Spam Statistics Filtering](#Spam-Statistics-Filtering)).

The spam and ham processors that apply to each group are determined by
the group’s `spam-process` group parameter. If this group
parameter is not defined, they are determined by the variable
`gnus-spam-process-newsgroups`.

Gnus learns from the spam you get. You have to collect your spam in
one or more spam groups, and set or customize the variable
`spam-junk-mailgroups` as appropriate. You can also declare
groups to contain spam by setting their group parameter
`spam-contents` to `gnus-group-spam-classification-spam`, or
by customizing the corresponding variable
`gnus-spam-newsgroup-contents`. The `spam-contents` group
parameter and the `gnus-spam-newsgroup-contents` variable can
also be used to declare groups as *ham* groups if you set their
classification to `gnus-group-spam-classification-ham`. If
groups are not classified by means of `spam-junk-mailgroups`,
`spam-contents`, or `gnus-spam-newsgroup-contents`, they are
considered *unclassified*. All groups are unclassified by
default.

In spam groups, all messages are considered to be spam by default:
they get the ‘`$`’ mark (`gnus-spam-mark`) when you enter the
group. If you have seen a message, had it marked as spam, then
unmarked it, it won’t be marked as spam when you enter the group
thereafter. You can disable that behavior, so all unread messages
will get the ‘`$`’ mark, if you set the
`spam-mark-only-unseen-as-spam` parameter to `nil`. You
should remove the ‘`$`’ mark when you are in the group summary
buffer for every message that is not spam after all. To remove the
‘`$`’ mark, you can use `M-u` to “unread” the article, or
`d` for declaring it read the non-spam way. When you leave a
group, all spam-marked (‘`$`’) articles are sent to a spam
processor which will study them as spam samples.

Messages may also be deleted in various other ways, and unless
`ham-marks` group parameter gets overridden below, marks ‘`R`’
and ‘`r`’ for default read or explicit delete, marks ‘`X`’ and
‘`K`’ for automatic or explicit kills, as well as mark ‘`Y`’ for
low scores, are all considered to be associated with articles which
are not spam. This assumption might be false, in particular if you
use kill files or score files as means for detecting genuine spam, you
should then adjust the `ham-marks` group parameter.

Variable: **ham-marks** [¶](#index-ham_002dmarks)
:   You can customize this group or topic parameter to be the list of
    marks you want to consider ham. By default, the list contains the
    deleted, read, killed, kill-filed, and low-score marks (the idea is
    that these articles have been read, but are not spam). It can be
    useful to also include the tick mark in the ham marks. It is not
    recommended to make the unread mark a ham mark, because it normally
    indicates a lack of classification. But you can do it, and we’ll be
    happy for you.

Variable: **spam-marks** [¶](#index-spam_002dmarks)
:   You can customize this group or topic parameter to be the list of
    marks you want to consider spam. By default, the list contains only
    the spam mark. It is not recommended to change that, but you can if
    you really want to.

When you leave *any* group, regardless of its
`spam-contents` classification, all spam-marked articles are sent
to a spam processor, which will study these as spam samples. If you
explicit kill a lot, you might sometimes end up with articles marked
‘`K`’ which you never saw, and which might accidentally contain
spam. Best is to make sure that real spam is marked with ‘`$`’,
and nothing else.

When you leave a *spam* group, all spam-marked articles are
marked as expired after processing with the spam processor. This is
not done for *unclassified* or *ham* groups. Also, any
**ham** articles in a spam group will be moved to a location
determined by either the `ham-process-destination` group
parameter or a match in the `gnus-ham-process-destinations`
variable, which is a list of regular expressions matched with group
names (it’s easiest to customize this variable with `M-x
customize-variable RET gnus-ham-process-destinations`). Each
group name list is a standard Lisp list, if you prefer to customize
the variable manually. If the `ham-process-destination`
parameter is not set, ham articles are left in place. If the
`spam-mark-ham-unread-before-move-from-spam-group` parameter is
set, the ham articles are marked as unread before being moved.

If ham can not be moved—because of a read-only back end such as
NNTP, for example, it will be copied.

Note that you can use multiples destinations per group or regular
expression! This enables you to send your ham to a regular mail
group and to a *ham training* group.

When you leave a *ham* group, all ham-marked articles are sent to
a ham processor, which will study these as non-spam samples.

By default the variable `spam-process-ham-in-spam-groups` is
`nil`. Set it to `t` if you want ham found in spam groups
to be processed. Normally this is not done, you are expected instead
to send your ham to a ham group and process it there.

By default the variable `spam-process-ham-in-nonham-groups` is
`nil`. Set it to `t` if you want ham found in non-ham (spam
or unclassified) groups to be processed. Normally this is not done,
you are expected instead to send your ham to a ham group and process
it there.

When you leave a *ham* or *unclassified* group, all
**spam** articles are moved to a location determined by either
the `spam-process-destination` group parameter or a match in the
`gnus-spam-process-destinations` variable, which is a list of
regular expressions matched with group names (it’s easiest to
customize this variable with `M-x customize-variable RET
gnus-spam-process-destinations`). Each group name list is a standard
Lisp list, if you prefer to customize the variable manually. If the
`spam-process-destination` parameter is not set, the spam
articles are only expired. The group name is fully qualified, meaning
that if you see ‘`nntp:servername`’ before the group name in the
group buffer then you need it here as well.

If spam can not be moved—because of a read-only back end such as
NNTP, for example, it will be copied.

Note that you can use multiples destinations per group or regular
expression! This enables you to send your spam to multiple *spam
training* groups.

The problem with processing ham and spam is that Gnus doesn’t track
this processing by default. Enable the `spam-log-to-registry`
variable so `spam.el` will use `gnus-registry.el` to track
what articles have been processed, and avoid processing articles
multiple times. Keep in mind that if you limit the number of registry
entries, this won’t work as well as it does without a limit.

Set this variable if you want only unseen articles in spam groups to
be marked as spam. By default, it is set. If you set it to
`nil`, unread articles will also be marked as spam.

Set this variable if you want ham to be unmarked before it is moved
out of the spam group. This is very useful when you use something
like the tick mark ‘`!`’ to mark ham—the article will be placed
in your `ham-process-destination`, unmarked as if it came fresh
from the mail server.

When autodetecting spam, this variable tells `spam.el` whether
only unseen articles or all unread articles should be checked for
spam. It is recommended that you leave it off.

---

Next: [Spam Back Ends](#Spam-Back-Ends), Previous: [Spam and Ham Processors](#Spam-and-Ham-Processors), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.5 Spam Package Configuration Examples [¶](#Spam-Package-Configuration-Examples-1)

#### Ted’s setup [¶](#Ted_0027s-setup)

From Ted Zlatanov <tzz@lifelogs.com>.

```
;; for gnus-registry-split-fancy-with-parent and spam autodetection
;; see gnus-registry.el for more information
(gnus-registry-initialize)
(spam-initialize)

(setq
 spam-log-to-registry t     ; for spam autodetection
 spam-use-BBDB t
 spam-use-regex-headers t   ; catch X-Spam-Flag (SpamAssassin)
 ;; all groups with ‘spam’ in the name contain spam
 gnus-spam-newsgroup-contents
  '(("spam" gnus-group-spam-classification-spam))
 ;; see documentation for these
 spam-move-spam-nonspam-groups-only nil
 spam-mark-only-unseen-as-spam t
 spam-mark-ham-unread-before-move-from-spam-group t
 ;; understand what this does before you copy it to your own setup!
 ;; for nnimap you’ll probably want to set nnimap-split-methods, see the manual
 nnimap-split-fancy '(|
                      ;; trace references to parents and put in their group
                      (: gnus-registry-split-fancy-with-parent)
                      ;; this will catch server-side SpamAssassin tags
                      (: spam-split 'spam-use-regex-headers)
                      (any "ding" "ding")
                      ;; note that spam by default will go to ‘spam’
                      (: spam-split)
                      ;; default mailbox
                      "mail"))

;; my parameters, set with G p

;; all nnml groups, and all nnimap groups except
;; ‘nnimap+mail.lifelogs.com:train’ and
;; ‘nnimap+mail.lifelogs.com:spam’: any spam goes to nnimap training,
;; because it must have been detected manually

((spam-process-destination . "nnimap+mail.lifelogs.com:train"))

;; all NNTP groups
;; autodetect spam with the blacklist and ham with the BBDB
((spam-autodetect-methods spam-use-blacklist spam-use-BBDB)
;; send all spam to the training group
 (spam-process-destination . "nnimap+mail.lifelogs.com:train"))

;; only some NNTP groups, where I want to autodetect spam
((spam-autodetect . t))

;; my nnimap ‘nnimap+mail.lifelogs.com:spam’ group

;; this is a spam group
((spam-contents gnus-group-spam-classification-spam)

 ;; any spam (which happens when I enter for all unseen messages,
 ;; because of the gnus-spam-newsgroup-contents setting above), goes to
 ;; ‘nnimap+mail.lifelogs.com:train’ unless I mark it as ham

 (spam-process-destination "nnimap+mail.lifelogs.com:train")

 ;; any ham goes to my ‘nnimap+mail.lifelogs.com:mail’ folder, but
 ;; also to my ‘nnimap+mail.lifelogs.com:trainham’ folder for training

 (ham-process-destination "nnimap+mail.lifelogs.com:mail"
                          "nnimap+mail.lifelogs.com:trainham")
 ;; in this group, only ‘!’ marks are ham
 (ham-marks
  (gnus-ticked-mark))
 ;; remembers senders in the blacklist on the way out—this is
 ;; definitely not needed, it just makes me feel better
 (spam-process (gnus-group-spam-exit-processor-blacklist)))

;; Later, on the IMAP server I use the ‘train’ group for training
;; SpamAssassin to recognize spam, and the ‘trainham’ group fora
;; recognizing ham—but Gnus has nothing to do with it.
```

#### Using `spam.el` on an IMAP server with a statistical filter on the server [¶](#Using-spam_002eel-on-an-IMAP-server-with-a-statistical-filter-on-the-server)

From Reiner Steib <reiner.steib@gmx.de>.

My provider has set up bogofilter (in combination with DCC) on
the mail server (IMAP). Recognized spam goes to
‘`spam.detected`’, the rest goes through the normal filter rules,
i.e., to ‘`some.folder`’ or to ‘`INBOX`’. Training on false
positives or negatives is done by copying or moving the article to
‘`training.ham`’ or ‘`training.spam`’ respectively. A cron job on
the server feeds those to bogofilter with the suitable ham or spam
options and deletes them from the ‘`training.ham`’ and
‘`training.spam`’ folders.

With the following entries in `gnus-parameters`, `spam.el`
does most of the job for me:

```
   ("nnimap:spam\\.detected"
    (gnus-article-sort-functions '(gnus-article-sort-by-chars))
    (ham-process-destination "nnimap:INBOX" "nnimap:training.ham")
    (spam-contents gnus-group-spam-classification-spam))
   ("nnimap:\\(INBOX\\|other-folders\\)"
    (spam-process-destination . "nnimap:training.spam")
    (spam-contents gnus-group-spam-classification-ham))
```

* **The Spam folder:**

  In the folder ‘`spam.detected`’, I have to check for false positives
  (i.e., legitimate mails, that were wrongly judged as spam by
  bogofilter or DCC).

  Because of the `gnus-group-spam-classification-spam` entry, all
  messages are marked as spam (with `$`). When I find a false
  positive, I mark the message with some other ham mark
  (`ham-marks`, [Spam and Ham Processors](#Spam-and-Ham-Processors)). On group exit,
  those messages are copied to both groups, ‘`INBOX`’ (where I want
  to have the article) and ‘`training.ham`’ (for training bogofilter)
  and deleted from the ‘`spam.detected`’ folder.

  The `gnus-article-sort-by-chars` entry simplifies detection of
  false positives for me. I receive lots of worms (sweN, …), that all
  have a similar size. Grouping them by size (i.e., chars) makes finding
  other false positives easier. (Of course worms aren’t *spam*
  strictly speaking. Anyhow, bogofilter is an excellent tool for
  filtering those unwanted mails for me.)
* **Ham folders:**

  In my ham folders, I just hit `S x`
  (`gnus-summary-mark-as-spam`) whenever I see an unrecognized spam
  mail (false negative). On group exit, those messages are moved to
  ‘`training.spam`’.

#### Reporting spam articles in Gmane groups with `spam-report.el` [¶](#Reporting-spam-articles-in-Gmane-groups-with-spam_002dreport_002eel)

From Reiner Steib <reiner.steib@gmx.de>.

With following entry in `gnus-parameters`, `S x`
(`gnus-summary-mark-as-spam`) marks articles in `gmane.*`
groups as spam and reports the to Gmane at group exit:

```
   ("^gmane\\."
    (spam-process (gnus-group-spam-exit-processor-report-gmane)))
```

Additionally, I use `(setq spam-report-gmane-use-article-number nil)`
because I don’t read the groups directly from news.gmane.io, but
through my local news server (leafnode). I.e., the article numbers are
not the same as on news.gmane.io, thus `spam-report.el` has to check
the `X-Report-Spam` header to find the correct number.

---

Next: [Extending the Spam package](#Extending-the-Spam-package), Previous: [Spam Package Configuration Examples](#Spam-Package-Configuration-Examples), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6 Spam Back Ends [¶](#Spam-Back-Ends-1)

The spam package offers a variety of back ends for detecting spam.
Each back end defines a set of methods for detecting spam
(see [Filtering Incoming Mail](#Filtering-Incoming-Mail), see [Detecting Spam in Groups](#Detecting-Spam-in-Groups)),
and a pair of spam and ham processors (see [Spam and Ham Processors](#Spam-and-Ham-Processors)).

* [Blacklists and Whitelists](#Blacklists-and-Whitelists)
* [BBDB Whitelists](#BBDB-Whitelists)
* [Gmane Spam Reporting](#Gmane-Spam-Reporting)
* [Blackholes](#Blackholes)
* [Regular Expressions Header Matching](#Regular-Expressions-Header-Matching)
* [Bogofilter](#Bogofilter)
* [SpamAssassin back end](#SpamAssassin-back-end)
* [ifile spam filtering](#ifile-spam-filtering)
* [Spam Statistics Filtering](#Spam-Statistics-Filtering)
* [Using SpamOracle with Gnus](#SpamOracle)

---

Next: [BBDB Whitelists](#BBDB-Whitelists), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.1 Blacklists and Whitelists [¶](#Blacklists-and-Whitelists-1)

Variable: **spam-use-blacklist** [¶](#index-spam_002duse_002dblacklist)
:   Set this variable to `t` if you want to use blacklists when
    splitting incoming mail. Messages whose senders are in the blacklist
    will be sent to the `spam-split-group`. This is an explicit
    filter, meaning that it acts only on mail senders *declared* to
    be spammers.

Variable: **spam-use-whitelist** [¶](#index-spam_002duse_002dwhitelist)
:   Set this variable to `t` if you want to use whitelists when
    splitting incoming mail. Messages whose senders are not in the
    whitelist will be sent to the next spam-split rule. This is an
    explicit filter, meaning that unless someone is in the whitelist, their
    messages are not assumed to be spam or ham.

Variable: **spam-use-whitelist-exclusive** [¶](#index-spam_002duse_002dwhitelist_002dexclusive)
:   Set this variable to `t` if you want to use whitelists as an
    implicit filter, meaning that every message will be considered spam
    unless the sender is in the whitelist. Use with care.

Variable: **gnus-group-spam-exit-processor-blacklist** [¶](#index-gnus_002dgroup_002dspam_002dexit_002dprocessor_002dblacklist)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, the senders of
    spam-marked articles will be added to the blacklist.

    *WARNING*

    Instead of the obsolete
    `gnus-group-spam-exit-processor-blacklist`, it is recommended
    that you use `(spam spam-use-blacklist)`. Everything will work
    the same way, we promise.

Variable: **gnus-group-ham-exit-processor-whitelist** [¶](#index-gnus_002dgroup_002dham_002dexit_002dprocessor_002dwhitelist)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, the senders of
    ham-marked articles in *ham* groups will be added to the
    whitelist.

    *WARNING*

    Instead of the obsolete
    `gnus-group-ham-exit-processor-whitelist`, it is recommended
    that you use `(ham spam-use-whitelist)`. Everything will work
    the same way, we promise.

Blacklists are lists of regular expressions matching addresses you
consider to be spam senders. For instance, to block mail from any
sender at ‘`vmadmin.com`’, you can put ‘`vmadmin.com`’ in your
blacklist. You start out with an empty blacklist. Blacklist entries
use the Emacs regular expression syntax.

Conversely, whitelists tell Gnus what addresses are considered
legitimate. All messages from whitelisted addresses are considered
non-spam. Also see [BBDB Whitelists](#BBDB-Whitelists). Whitelist entries use the
Emacs regular expression syntax.

The blacklist and whitelist file locations can be customized with the
`spam-directory` variable (`~/News/spam` by default), or
the `spam-whitelist` and `spam-blacklist` variables
directly. The whitelist and blacklist files will by default be in the
`spam-directory` directory, named `whitelist` and
`blacklist` respectively.

---

Next: [Gmane Spam Reporting](#Gmane-Spam-Reporting), Previous: [Blacklists and Whitelists](#Blacklists-and-Whitelists), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.2 BBDB Whitelists [¶](#BBDB-Whitelists-1)

Variable: **spam-use-BBDB** [¶](#index-spam_002duse_002dBBDB)
:   Analogous to `spam-use-whitelist` (see [Blacklists and Whitelists](#Blacklists-and-Whitelists)), but uses the BBDB as the source of whitelisted
    addresses, without regular expressions. You must have the BBDB loaded
    for `spam-use-BBDB` to work properly. Messages whose senders are
    not in the BBDB will be sent to the next spam-split rule. This is an
    explicit filter, meaning that unless someone is in the BBDB, their
    messages are not assumed to be spam or ham.

Variable: **spam-use-BBDB-exclusive** [¶](#index-spam_002duse_002dBBDB_002dexclusive)
:   Set this variable to `t` if you want to use the BBDB as an
    implicit filter, meaning that every message will be considered spam
    unless the sender is in the BBDB. Use with care. Only sender
    addresses in the BBDB will be allowed through; all others will be
    classified as spammers.

    While `spam-use-BBDB-exclusive` *can* be used as an alias
    for `spam-use-BBDB` as far as `spam.el` is concerned, it is
    *not* a separate back end. If you set
    `spam-use-BBDB-exclusive` to `t`, *all* your BBDB splitting
    will be exclusive.

Variable: **gnus-group-ham-exit-processor-BBDB** [¶](#index-gnus_002dgroup_002dham_002dexit_002dprocessor_002dBBDB)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, the senders of
    ham-marked articles in *ham* groups will be added to the
    BBDB.

    *WARNING*

    Instead of the obsolete
    `gnus-group-ham-exit-processor-BBDB`, it is recommended
    that you use `(ham spam-use-BBDB)`. Everything will work
    the same way, we promise.

---

Next: [Blackholes](#Blackholes), Previous: [BBDB Whitelists](#BBDB-Whitelists), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.3 Gmane Spam Reporting [¶](#Gmane-Spam-Reporting-1)

Variable: **gnus-group-spam-exit-processor-report-gmane** [¶](#index-gnus_002dgroup_002dspam_002dexit_002dprocessor_002dreport_002dgmane)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, the spam-marked
    articles groups will be reported to the Gmane administrators via a
    HTTP request.

    Gmane is at <https://gmane.io>.

    *WARNING*

    Instead of the obsolete
    `gnus-group-spam-exit-processor-report-gmane`, it is recommended
    that you use `(spam spam-use-gmane)`. Everything will work the
    same way, we promise.

Variable: **spam-report-gmane-use-article-number** [¶](#index-spam_002dreport_002dgmane_002duse_002darticle_002dnumber)
:   This variable is `t` by default. Set it to `nil` if you are
    running your own news server, for instance, and the local article
    numbers don’t correspond to the Gmane article numbers. When
    `spam-report-gmane-use-article-number` is `nil`,
    `spam-report.el` will fetch the number from the article headers.

Variable: **spam-report-user-mail-address** [¶](#index-spam_002dreport_002duser_002dmail_002daddress)
:   Mail address exposed in the User-Agent spam reports to Gmane. It allows
    the Gmane administrators to contact you in case of misreports. The
    default is `user-mail-address`.

---

Next: [Regular Expressions Header Matching](#Regular-Expressions-Header-Matching), Previous: [Gmane Spam Reporting](#Gmane-Spam-Reporting), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.4 Blackholes [¶](#Blackholes-1)

Variable: **spam-use-blackholes** [¶](#index-spam_002duse_002dblackholes)
:   This option is disabled by default. You can let Gnus consult the
    blackhole-type distributed spam processing systems (DCC, for instance)
    when you set this option. The variable `spam-blackhole-servers`
    holds the list of blackhole servers Gnus will consult. The current
    list is fairly comprehensive, but make sure to let us know if it
    contains outdated servers.

    The blackhole check uses the `dig.el` package, but you can tell
    `spam.el` to use `dns.el` instead for better performance if
    you set `spam-use-dig` to `nil`. It is not recommended at
    this time to set `spam-use-dig` to `nil` despite the
    possible performance improvements, because some users may be unable to
    use it, but you can try it and see if it works for you.

Variable: **spam-blackhole-servers** [¶](#index-spam_002dblackhole_002dservers)
:   The list of servers to consult for blackhole checks.

Variable: **spam-blackhole-good-server-regex** [¶](#index-spam_002dblackhole_002dgood_002dserver_002dregex)
:   A regular expression for IPs that should not be checked against the
    blackhole server list. When set to `nil`, it has no effect.

Variable: **spam-use-dig** [¶](#index-spam_002duse_002ddig)
:   Use the `dig.el` package instead of the `dns.el` package.
    The default setting of `t` is recommended.

Blackhole checks are done only on incoming mail. There is no spam or
ham processor for blackholes.

---

Next: [Bogofilter](#Bogofilter), Previous: [Blackholes](#Blackholes), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.5 Regular Expressions Header Matching [¶](#Regular-Expressions-Header-Matching-1)

Variable: **spam-use-regex-headers** [¶](#index-spam_002duse_002dregex_002dheaders)
:   This option is disabled by default. You can let Gnus check the
    message headers against lists of regular expressions when you set this
    option. The variables `spam-regex-headers-spam` and
    `spam-regex-headers-ham` hold the list of regular expressions.
    Gnus will check against the message headers to determine if the
    message is spam or ham, respectively.

Variable: **spam-regex-headers-spam** [¶](#index-spam_002dregex_002dheaders_002dspam)
:   The list of regular expressions that, when matched in the headers of
    the message, positively identify it as spam.

Variable: **spam-regex-headers-ham** [¶](#index-spam_002dregex_002dheaders_002dham)
:   The list of regular expressions that, when matched in the headers of
    the message, positively identify it as ham.

Regular expression header checks are done only on incoming mail.
There is no specific spam or ham processor for regular expressions.

---

Next: [SpamAssassin back end](#SpamAssassin-back-end), Previous: [Regular Expressions Header Matching](#Regular-Expressions-Header-Matching), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.6 Bogofilter [¶](#Bogofilter-1)

Variable: **spam-use-bogofilter** [¶](#index-spam_002duse_002dbogofilter)
:   Set this variable if you want `spam-split` to use Eric S. Raymond’s
    speedy Bogofilter.

    With a minimum of care for associating the ‘`$`’ mark for spam
    articles only, Bogofilter training all gets fairly automatic. You
    should do this until you get a few hundreds of articles in each
    category, spam or not. The command `S t` in summary mode, either
    for debugging or for curiosity, shows the *spamicity* score of
    the current article (between 0.0 and 1.0).

    Bogofilter determines if a message is spam based on a specific
    threshold. That threshold can be customized, consult the Bogofilter
    documentation.

    If the `bogofilter` executable is not in your path, Bogofilter
    processing will be turned off.

    You should not enable this if you use `spam-use-bogofilter-headers`.

`M s t` [¶](#index-M-s-t)

`S t`
:   Get the Bogofilter spamicity score (`spam-bogofilter-score`).

Variable: **spam-use-bogofilter-headers** [¶](#index-spam_002duse_002dbogofilter_002dheaders)
:   Set this variable if you want `spam-split` to use Eric S. Raymond’s
    speedy Bogofilter, looking only at the message headers. It works
    similarly to `spam-use-bogofilter`, but the `X-Bogosity` header
    must be in the message already. Normally you would do this with a
    procmail recipe or something similar; consult the Bogofilter
    installation documents for details.

    You should not enable this if you use `spam-use-bogofilter`.

Variable: **gnus-group-spam-exit-processor-bogofilter** [¶](#index-gnus_002dgroup_002dspam_002dexit_002dprocessor_002dbogofilter)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, spam-marked articles
    will be added to the Bogofilter spam database.

    *WARNING*

    Instead of the obsolete
    `gnus-group-spam-exit-processor-bogofilter`, it is recommended
    that you use `(spam spam-use-bogofilter)`. Everything will work
    the same way, we promise.

Variable: **gnus-group-ham-exit-processor-bogofilter** [¶](#index-gnus_002dgroup_002dham_002dexit_002dprocessor_002dbogofilter)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, the ham-marked
    articles in *ham* groups will be added to the Bogofilter database
    of non-spam messages.

    *WARNING*

    Instead of the obsolete
    `gnus-group-ham-exit-processor-bogofilter`, it is recommended
    that you use `(ham spam-use-bogofilter)`. Everything will work
    the same way, we promise.

Variable: **spam-bogofilter-database-directory** [¶](#index-spam_002dbogofilter_002ddatabase_002ddirectory)
:   This is the directory where Bogofilter will store its databases. It
    is not specified by default, so Bogofilter will use its own default
    database directory.

The Bogofilter mail classifier is similar to `ifile` in intent and
purpose. A ham and a spam processor are provided, plus the
`spam-use-bogofilter` and `spam-use-bogofilter-headers`
variables to indicate to spam-split that Bogofilter should either be
used, or has already been used on the article. The 0.9.2.1 version of
Bogofilter was used to test this functionality.

---

Next: [ifile spam filtering](#ifile-spam-filtering), Previous: [Bogofilter](#Bogofilter), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.7 SpamAssassin back end [¶](#SpamAssassin-back-end-1)

Variable: **spam-use-spamassassin** [¶](#index-spam_002duse_002dspamassassin)
:   Set this variable if you want `spam-split` to use SpamAssassin.

    SpamAssassin assigns a score to each article based on a set of rules
    and tests, including a Bayesian filter. The Bayesian filter can be
    trained by associating the ‘`$`’ mark for spam articles. The
    spam score can be viewed by using the command `S t` in summary
    mode.

    If you set this variable, each article will be processed by
    SpamAssassin when `spam-split` is called. If your mail is
    preprocessed by SpamAssassin, and you want to just use the
    SpamAssassin headers, set `spam-use-spamassassin-headers`
    instead.

    You should not enable this if you use
    `spam-use-spamassassin-headers`.

Variable: **spam-use-spamassassin-headers** [¶](#index-spam_002duse_002dspamassassin_002dheaders)
:   Set this variable if your mail is preprocessed by SpamAssassin and
    want `spam-split` to split based on the SpamAssassin headers.

    You should not enable this if you use `spam-use-spamassassin`.

Variable: **spam-spamassassin-program** [¶](#index-spam_002dspamassassin_002dprogram)
:   This variable points to the SpamAssassin executable. If you have
    `spamd` running, you can set this variable to the `spamc`
    executable for faster processing. See the SpamAssassin documentation
    for more information on `spamd`/`spamc`.

SpamAssassin is a powerful and flexible spam filter that uses a wide
variety of tests to identify spam. A ham and a spam processors are
provided, plus the `spam-use-spamassassin` and
`spam-use-spamassassin-headers` variables to indicate to
spam-split that SpamAssassin should be either used, or has already
been used on the article. The 2.63 version of SpamAssassin was used
to test this functionality.

---

Next: [Spam Statistics Filtering](#Spam-Statistics-Filtering), Previous: [SpamAssassin back end](#SpamAssassin-back-end), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.8 ifile spam filtering [¶](#ifile-spam-filtering-1)

Variable: **spam-use-ifile** [¶](#index-spam_002duse_002difile)
:   Enable this variable if you want `spam-split` to use `ifile`, a
    statistical analyzer similar to Bogofilter.

Variable: **spam-ifile-all-categories** [¶](#index-spam_002difile_002dall_002dcategories)
:   Enable this variable if you want `spam-use-ifile` to give you all
    the ifile categories, not just spam/non-spam. If you use this, make
    sure you train ifile as described in its documentation.

Variable: **spam-ifile-spam-category** [¶](#index-spam_002difile_002dspam_002dcategory)
:   This is the category of spam messages as far as ifile is concerned.
    The actual string used is irrelevant, but you probably want to leave
    the default value of ‘`spam`’.

Variable: **spam-ifile-database** [¶](#index-spam_002difile_002ddatabase)
:   This is the filename for the ifile database. It is not specified by
    default, so ifile will use its own default database name.

The ifile mail classifier is similar to Bogofilter in intent and
purpose. A ham and a spam processor are provided, plus the
`spam-use-ifile` variable to indicate to spam-split that ifile
should be used. The 1.2.1 version of ifile was used to test this
functionality.

---

Next: [Using SpamOracle with Gnus](#SpamOracle), Previous: [ifile spam filtering](#ifile-spam-filtering), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.9 Spam Statistics Filtering [¶](#Spam-Statistics-Filtering-1)

This back end uses the Spam Statistics Emacs Lisp package to perform
statistics-based filtering (see [Spam Statistics Package](#Spam-Statistics-Package)). Before
using this, you may want to perform some additional steps to
initialize your Spam Statistics dictionary. See [Creating a spam-stat dictionary](#Creating-a-spam_002dstat-dictionary).

Variable: **spam-use-stat** [¶](#index-spam_002duse_002dstat-1)

Variable: **gnus-group-spam-exit-processor-stat** [¶](#index-gnus_002dgroup_002dspam_002dexit_002dprocessor_002dstat)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, the spam-marked
    articles will be added to the spam-stat database of spam messages.

    *WARNING*

    Instead of the obsolete
    `gnus-group-spam-exit-processor-stat`, it is recommended
    that you use `(spam spam-use-stat)`. Everything will work
    the same way, we promise.

Variable: **gnus-group-ham-exit-processor-stat** [¶](#index-gnus_002dgroup_002dham_002dexit_002dprocessor_002dstat)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameters or the
    `gnus-spam-process-newsgroups` variable. When this symbol is
    added to a group’s `spam-process` parameter, the ham-marked
    articles in *ham* groups will be added to the spam-stat database
    of non-spam messages.

    *WARNING*

    Instead of the obsolete
    `gnus-group-ham-exit-processor-stat`, it is recommended
    that you use `(ham spam-use-stat)`. Everything will work
    the same way, we promise.

This enables `spam.el` to cooperate with `spam-stat.el`.
`spam-stat.el` provides an internal (Lisp-only) spam database,
which unlike ifile or Bogofilter does not require external programs.
A spam and a ham processor, and the `spam-use-stat` variable for
`spam-split` are provided.

---

Previous: [Spam Statistics Filtering](#Spam-Statistics-Filtering), Up: [Spam Back Ends](#Spam-Back-Ends)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.6.10 Using SpamOracle with Gnus [¶](#Using-SpamOracle-with-Gnus)

An easy way to filter out spam is to use SpamOracle. SpamOracle is an
statistical mail filtering tool written by Xavier Leroy and needs to be
installed separately.

There are several ways to use SpamOracle with Gnus. In all cases, your
mail is piped through SpamOracle in its *mark* mode. SpamOracle will
then enter an ‘`X-Spam`’ header indicating whether it regards the
mail as a spam mail or not.

One possibility is to run SpamOracle as a `:prescript` from the
See [Mail Source Specifiers](#Mail-Source-Specifiers), (see [SpamAssassin, Vipul’s Razor, DCC, etc](#SpamAssassin)). This method has
the advantage that the user can see the *X-Spam* headers.

The easiest method is to make `spam.el` (see [Spam Package](#Spam-Package))
call SpamOracle.

To enable SpamOracle usage by `spam.el`, set the variable
`spam-use-spamoracle` to `t` and configure the
`nnmail-split-fancy` or `nnimap-split-fancy`. See [Spam Package](#Spam-Package). In this example the ‘`INBOX`’ of an nnimap server is
filtered using SpamOracle. Mails recognized as spam mails will be
moved to `spam-split-group`, ‘`Junk`’ in this case. Ham
messages stay in ‘`INBOX`’:

```
(setq spam-use-spamoracle t
      spam-split-group "Junk"
      ;; for nnimap you’ll probably want to set nnimap-split-methods, see the manual
      nnimap-split-inbox '("INBOX")
      nnimap-split-fancy '(| (: spam-split) "INBOX"))
```

Variable: **spam-use-spamoracle** [¶](#index-spam_002duse_002dspamoracle-1)
:   Set to `t` if you want Gnus to enable spam filtering using
    SpamOracle.

Variable: **spam-spamoracle-binary** [¶](#index-spam_002dspamoracle_002dbinary)
:   Gnus uses the SpamOracle binary called `spamoracle` found in the
    user’s PATH. Using the variable `spam-spamoracle-binary`, this
    can be customized.

Variable: **spam-spamoracle-database** [¶](#index-spam_002dspamoracle_002ddatabase)
:   By default, SpamOracle uses the file `~/.spamoracle.db` as a database to
    store its analysis. This is controlled by the variable
    `spam-spamoracle-database` which defaults to `nil`. That means
    the default SpamOracle database will be used. In case you want your
    database to live somewhere special, set
    `spam-spamoracle-database` to this path.

SpamOracle employs a statistical algorithm to determine whether a
message is spam or ham. In order to get good results, meaning few
false hits or misses, SpamOracle needs training. SpamOracle learns
the characteristics of your spam mails. Using the *add* mode
(training mode) one has to feed good (ham) and spam mails to
SpamOracle. This can be done by pressing `|` in the Summary
buffer and pipe the mail to a SpamOracle process or using
`spam.el`’s spam- and ham-processors, which is much more
convenient. For a detailed description of spam- and ham-processors,
See [Spam Package](#Spam-Package).

Variable: **gnus-group-spam-exit-processor-spamoracle** [¶](#index-gnus_002dgroup_002dspam_002dexit_002dprocessor_002dspamoracle)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameter or the
    `gnus-spam-process-newsgroups` variable. When this symbol is added
    to a group’s `spam-process` parameter, spam-marked articles will be
    sent to SpamOracle as spam samples.

    *WARNING*

    Instead of the obsolete
    `gnus-group-spam-exit-processor-spamoracle`, it is recommended
    that you use `(spam spam-use-spamoracle)`. Everything will work
    the same way, we promise.

Variable: **gnus-group-ham-exit-processor-spamoracle** [¶](#index-gnus_002dgroup_002dham_002dexit_002dprocessor_002dspamoracle)
:   Add this symbol to a group’s `spam-process` parameter by
    customizing the group parameter or the
    `gnus-spam-process-newsgroups` variable. When this symbol is added
    to a group’s `spam-process` parameter, the ham-marked articles in
    *ham* groups will be sent to the SpamOracle as samples of ham
    messages.

    *WARNING*

    Instead of the obsolete
    `gnus-group-ham-exit-processor-spamoracle`, it is recommended
    that you use `(ham spam-use-spamoracle)`. Everything will work
    the same way, we promise.

*Example:* These are the Group Parameters of a group that has been
classified as a ham group, meaning that it should only contain ham
messages.

```
 ((spam-contents gnus-group-spam-classification-ham)
  (spam-process ((ham spam-use-spamoracle)
                 (spam spam-use-spamoracle))))
```

For this group the `spam-use-spamoracle` is installed for both
ham and spam processing. If the group contains spam message
(e.g., because SpamOracle has not had enough sample messages yet) and
the user marks some messages as spam messages, these messages will be
processed by SpamOracle. The processor sends the messages to
SpamOracle as new samples for spam.

---

Next: [Spam Statistics Package](#Spam-Statistics-Package), Previous: [Spam Back Ends](#Spam-Back-Ends), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.7 Extending the Spam package [¶](#Extending-the-Spam-package-1)

Say you want to add a new back end called blackbox. For filtering
incoming mail, provide the following:

1. Code

   ```
   (defvar spam-use-blackbox nil
     "True if blackbox should be used.")
   ```

   Write `spam-check-blackbox` if Blackbox can check incoming mail.

   Write `spam-blackbox-register-routine` and
   `spam-blackbox-unregister-routine` using the bogofilter
   register/unregister routines as a start, or other register/unregister
   routines more appropriate to Blackbox, if Blackbox can
   register/unregister spam and ham.
2. Functionality

   The `spam-check-blackbox` function should return ‘`nil`’ or
   `spam-split-group`, observing the other conventions. See the
   existing `spam-check-*` functions for examples of what you can
   do, and stick to the template unless you fully understand the reasons
   why you aren’t.

For processing spam and ham messages, provide the following:

1. Code

   Note you don’t have to provide a spam or a ham processor. Only
   provide them if Blackbox supports spam or ham processing.

   Also, ham and spam processors are being phased out as single
   variables. Instead the form `(spam spam-use-blackbox)` or
   `(ham spam-use-blackbox)` is favored. For now, spam/ham
   processor variables are still around but they won’t be for long.

   ```
   (defvar gnus-group-spam-exit-processor-blackbox "blackbox-spam"
     "The Blackbox summary exit spam processor.
   Only applicable to spam groups.")

   (defvar gnus-group-ham-exit-processor-blackbox "blackbox-ham"
     "The whitelist summary exit ham processor.
   Only applicable to non-spam (unclassified and ham) groups.")
   ```
2. Gnus parameters

   Add

   ```
   (const :tag "Spam: Blackbox" (spam spam-use-blackbox))
   (const :tag "Ham: Blackbox"  (ham spam-use-blackbox))
   ```

   to the `spam-process` group parameter in `gnus.el`. Make
   sure you do it twice, once for the parameter and once for the
   variable customization.

   Add

   ```
   (variable-item spam-use-blackbox)
   ```

   to the `spam-autodetect-methods` group parameter in
   `gnus.el` if Blackbox can check incoming mail for spam contents.

   Finally, use the appropriate `spam-install-*-backend` function in
   `spam.el`. Here are the available functions.

   1. `spam-install-backend-alias`

      This function will simply install an alias for a back end that does
      everything like the original back end. It is currently only used to
      make `spam-use-BBDB-exclusive` act like `spam-use-BBDB`.
   2. `spam-install-nocheck-backend`

      This function installs a back end that has no check function, but can
      register/unregister ham or spam. The `spam-use-gmane` back end is
      such a back end.
   3. `spam-install-checkonly-backend`

      This function will install a back end that can only check incoming mail
      for spam contents. It can’t register or unregister messages.
      `spam-use-blackholes` is such a back end.
   4. `spam-install-statistical-checkonly-backend`

      This function installs a statistical back end (one which requires the
      full body of a message to check it) that can only check incoming mail
      for contents. `spam-use-regex-body` is such a filter.
   5. `spam-install-statistical-backend`

      This function install a statistical back end with incoming checks and
      registration/unregistration routines. `spam-use-bogofilter` is
      set up this way.
   6. `spam-install-backend`

      This is the most normal back end installation, where a back end that can
      check and register/unregister messages is set up without statistical
      abilities. The `spam-use-BBDB` is such a back end.
   7. `spam-install-mover-backend`

      Mover back ends are internal to `spam.el` and specifically move
      articles around when the summary is exited. You will very probably
      never install such a back end.

---

Previous: [Extending the Spam package](#Extending-the-Spam-package), Up: [Spam Package](#Spam-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.8 Spam Statistics Package [¶](#Spam-Statistics-Package-1)

Paul Graham has written an excellent essay about spam filtering using
statistics: [A Plan for
Spam](https://www.paulgraham.com/spam.html). In it he describes the inherent deficiency of rule-based
filtering as used by SpamAssassin, for example: Somebody has to write
the rules, and everybody else has to install these rules. You are
always late. It would be much better, he argues, to filter mail based
on whether it somehow resembles spam or non-spam. One way to measure
this is word distribution. He then goes on to describe a solution
that checks whether a new mail resembles any of your other spam mails
or not.

The basic idea is this: Create a two collections of your mail, one
with spam, one with non-spam. Count how often each word appears in
either collection, weight this by the total number of mails in the
collections, and store this information in a dictionary. For every
word in a new mail, determine its probability to belong to a spam or a
non-spam mail. Use the 15 most conspicuous words, compute the total
probability of the mail being spam. If this probability is higher
than a certain threshold, the mail is considered to be spam.

The Spam Statistics package adds support to Gnus for this kind of
filtering. It can be used as one of the back ends of the Spam package
(see [Spam Package](#Spam-Package)), or by itself.

Before using the Spam Statistics package, you need to set it up.
First, you need two collections of your mail, one with spam, one with
non-spam. Then you need to create a dictionary using these two
collections, and save it. And last but not least, you need to use
this dictionary in your fancy mail splitting rules.

* [Creating a spam-stat dictionary](#Creating-a-spam_002dstat-dictionary)
* [Splitting mail using spam-stat](#Splitting-mail-using-spam_002dstat)
* [Low-level interface to the spam-stat dictionary](#Low_002dlevel-interface-to-the-spam_002dstat-dictionary)

---

Next: [Splitting mail using spam-stat](#Splitting-mail-using-spam_002dstat), Up: [Spam Statistics Package](#Spam-Statistics-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.8.1 Creating a spam-stat dictionary [¶](#Creating-a-spam_002dstat-dictionary-1)

Before you can begin to filter spam based on statistics, you must
create these statistics based on two mail collections, one with spam,
one with non-spam. These statistics are then stored in a dictionary
for later use. In order for these statistics to be meaningful, you
need several hundred emails in both collections.

Gnus currently supports only the nnml back end for automated dictionary
creation. The nnml back end stores all mails in a directory, one file
per mail. Use the following:

Function: **spam-stat-process-spam-directory** [¶](#index-spam_002dstat_002dprocess_002dspam_002ddirectory)
:   Create spam statistics for every file in this directory. Every file
    is treated as one spam mail.

Function: **spam-stat-process-non-spam-directory** [¶](#index-spam_002dstat_002dprocess_002dnon_002dspam_002ddirectory)
:   Create non-spam statistics for every file in this directory. Every
    file is treated as one non-spam mail.

Variable: **spam-stat-process-directory-age** [¶](#index-spam_002dstat_002dprocess_002ddirectory_002dage)
:   Maximum age of files to be processed, in days. Without this filter,
    re-training spam-stat with several thousand messages could take a long
    time. The default is 90, but you might want to set this to a bigger
    value during the initial training.

Usually you would call `spam-stat-process-spam-directory` on a
directory such as `~/Mail/mail/spam` (this usually corresponds to
the group ‘`nnml:mail.spam`’), and you would call
`spam-stat-process-non-spam-directory` on a directory such as
`~/Mail/mail/misc` (this usually corresponds to the group
‘`nnml:mail.misc`’).

When you are using IMAP, you won’t have the mails available
locally, so that will not work. One solution is to use the Gnus Agent
to cache the articles. Then you can use directories such as
`"~/News/agent/nnimap/mail.yourisp.com/personal_spam"` for
`spam-stat-process-spam-directory`. See [Agent as Cache](#Agent-as-Cache).

Variable: **spam-stat** [¶](#index-spam_002dstat-1)
:   This variable holds the hash-table with all the statistics—the
    dictionary we have been talking about. For every word in either
    collection, this hash-table stores a vector describing how often the
    word appeared in spam and often it appeared in non-spam mails.

If you want to regenerate the statistics from scratch, you need to
reset the dictionary.

Function: **spam-stat-reset** [¶](#index-spam_002dstat_002dreset)
:   Reset the `spam-stat` hash-table, deleting all the statistics.

When you are done, you must save the dictionary. The dictionary may
be rather large. If you will not update the dictionary incrementally
(instead, you will recreate it once a month, for example), then you
can reduce the size of the dictionary by deleting all words that did
not appear often enough or that do not clearly belong to only spam or
only non-spam mails.

Function: **spam-stat-reduce-size** [¶](#index-spam_002dstat_002dreduce_002dsize)
:   Reduce the size of the dictionary. Use this only if you do not want
    to update the dictionary incrementally.

Function: **spam-stat-save** [¶](#index-spam_002dstat_002dsave)
:   Save the dictionary.

Variable: **spam-stat-file** [¶](#index-spam_002dstat_002dfile)
:   The filename used to store the dictionary. This defaults to
    `~/.spam-stat.el`.

---

Next: [Low-level interface to the spam-stat dictionary](#Low_002dlevel-interface-to-the-spam_002dstat-dictionary), Previous: [Creating a spam-stat dictionary](#Creating-a-spam_002dstat-dictionary), Up: [Spam Statistics Package](#Spam-Statistics-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.8.2 Splitting mail using spam-stat [¶](#Splitting-mail-using-spam_002dstat-1)

This section describes how to use the Spam statistics
*independently* of the See [Spam Package](#Spam-Package).

First, add the following to your `~/.gnus.el` file:

```
(require 'spam-stat)
(spam-stat-load)
```

This will load the necessary Gnus code, and the dictionary you
created.

Next, you need to adapt your fancy splitting rules: You need to
determine how to use `spam-stat`. The following examples are for
the nnml back end. Using the nnimap back end works just as well. Just
use `nnimap-split-fancy` instead of `nnmail-split-fancy`.

In the simplest case, you only have two groups, ‘`mail.misc`’ and
‘`mail.spam`’. The following expression says that mail is either
spam or it should go into ‘`mail.misc`’. If it is spam, then
`spam-stat-split-fancy` will return ‘`mail.spam`’.

```
(setq nnmail-split-fancy
      `(| (: spam-stat-split-fancy)
          "mail.misc"))
```

Variable: **spam-stat-split-fancy-spam-group** [¶](#index-spam_002dstat_002dsplit_002dfancy_002dspam_002dgroup)
:   The group to use for spam. Default is ‘`mail.spam`’.

If you also filter mail with specific subjects into other groups, use
the following expression. Only mails not matching the regular
expression are considered potential spam.

```
(setq nnmail-split-fancy
      `(| ("Subject" "\\bspam-stat\\b" "mail.emacs")
          (: spam-stat-split-fancy)
          "mail.misc"))
```

If you want to filter for spam first, then you must be careful when
creating the dictionary. Note that `spam-stat-split-fancy` must
consider both mails in ‘`mail.emacs`’ and in ‘`mail.misc`’ as
non-spam, therefore both should be in your collection of non-spam
mails, when creating the dictionary!

```
(setq nnmail-split-fancy
      `(| (: spam-stat-split-fancy)
          ("Subject" "\\bspam-stat\\b" "mail.emacs")
          "mail.misc"))
```

You can combine this with traditional filtering. Here, we move all
HTML-only mails into the ‘`mail.spam.filtered`’ group. Note that since
`spam-stat-split-fancy` will never see them, the mails in
‘`mail.spam.filtered`’ should be neither in your collection of spam mails,
nor in your collection of non-spam mails, when creating the
dictionary!

```
(setq nnmail-split-fancy
      `(| ("Content-Type" "text/html" "mail.spam.filtered")
          (: spam-stat-split-fancy)
          ("Subject" "\\bspam-stat\\b" "mail.emacs")
          "mail.misc"))
```

---

Previous: [Splitting mail using spam-stat](#Splitting-mail-using-spam_002dstat), Up: [Spam Statistics Package](#Spam-Statistics-Package)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.19.8.3 Low-level interface to the spam-stat dictionary [¶](#Low_002dlevel-interface-to-the-spam_002dstat-dictionary-1)

The main interface to using `spam-stat`, are the following functions:

Function: **spam-stat-buffer-is-spam** [¶](#index-spam_002dstat_002dbuffer_002dis_002dspam)
:   Called in a buffer, that buffer is considered to be a new spam mail.
    Use this for new mail that has not been processed before.

Function: **spam-stat-buffer-is-no-spam** [¶](#index-spam_002dstat_002dbuffer_002dis_002dno_002dspam)
:   Called in a buffer, that buffer is considered to be a new non-spam
    mail. Use this for new mail that has not been processed before.

Function: **spam-stat-buffer-change-to-spam** [¶](#index-spam_002dstat_002dbuffer_002dchange_002dto_002dspam)
:   Called in a buffer, that buffer is no longer considered to be normal
    mail but spam. Use this to change the status of a mail that has
    already been processed as non-spam.

Function: **spam-stat-buffer-change-to-non-spam** [¶](#index-spam_002dstat_002dbuffer_002dchange_002dto_002dnon_002dspam)
:   Called in a buffer, that buffer is no longer considered to be spam but
    normal mail. Use this to change the status of a mail that has already
    been processed as spam.

Function: **spam-stat-save** [¶](#index-spam_002dstat_002dsave-1)
:   Save the hash table to the file. The filename used is stored in the
    variable `spam-stat-file`.

Function: **spam-stat-load** [¶](#index-spam_002dstat_002dload)
:   Load the hash table from a file. The filename used is stored in the
    variable `spam-stat-file`.

Function: **spam-stat-score-word** [¶](#index-spam_002dstat_002dscore_002dword)
:   Return the spam score for a word.

Function: **spam-stat-score-buffer** [¶](#index-spam_002dstat_002dscore_002dbuffer)
:   Return the spam score for a buffer.

Function: **spam-stat-split-fancy** [¶](#index-spam_002dstat_002dsplit_002dfancy)
:   Use this function for fancy mail splitting. Add the rule ‘`(:
    spam-stat-split-fancy)`’ to `nnmail-split-fancy`

Make sure you load the dictionary before using it. This requires the
following in your `~/.gnus.el` file:

```
(require 'spam-stat)
(spam-stat-load)
```

Typical test will involve calls to the following functions:

```
Reset: (setq spam-stat (make-hash-table :test 'equal))
Learn spam: (spam-stat-process-spam-directory "~/Mail/mail/spam")
Learn non-spam: (spam-stat-process-non-spam-directory "~/Mail/mail/misc")
Save table: (spam-stat-save)
File size: (file-attribute-size (file-attributes spam-stat-file))
Number of words: (hash-table-count spam-stat)
Test spam: (spam-stat-test-directory "~/Mail/mail/spam")
Test non-spam: (spam-stat-test-directory "~/Mail/mail/misc")
Reduce table size: (spam-stat-reduce-size)
Save table: (spam-stat-save)
File size: (file-attribute-size (file-attributes spam-stat-file))
Number of words: (hash-table-count spam-stat)
Test spam: (spam-stat-test-directory "~/Mail/mail/spam")
Test non-spam: (spam-stat-test-directory "~/Mail/mail/misc")
```

Here is how you would create your dictionary:

```
Reset: (setq spam-stat (make-hash-table :test 'equal))
Learn spam: (spam-stat-process-spam-directory "~/Mail/mail/spam")
Learn non-spam: (spam-stat-process-non-spam-directory "~/Mail/mail/misc")
Repeat for any other non-spam group you need...
Reduce table size: (spam-stat-reduce-size)
Save table: (spam-stat-save)
```

---

Next: [The Gnus Cloud](#The-Gnus-Cloud), Previous: [Spam Package](#Spam-Package), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
