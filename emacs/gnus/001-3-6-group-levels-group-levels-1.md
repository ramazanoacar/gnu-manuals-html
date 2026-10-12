### 3.6 Group Levels [¶](#Group-Levels-1)

All groups have a level of *subscribedness*. For instance, if a
group is on level 2, it is more subscribed than a group on level 5. You
can ask Gnus to just list groups on a given level or lower
(see [Listing Groups](#Listing-Groups)), or to just check for new articles in groups on
a given level or lower (see [Scanning New Messages](#Scanning-New-Messages)).

Remember: The higher the level of the group, the less important it is.

`S l` [¶](#index-S-l-_0028Group_0029)
:   Set the level of the current group. If a numeric prefix is given, the
    next n groups will have their levels set. The user will be
    prompted for a level.

Gnus considers groups from levels 1 to
`gnus-level-subscribed` (inclusive) (default 5) to be subscribed,
`gnus-level-subscribed` (exclusive) and
`gnus-level-unsubscribed` (inclusive) (default 7) to be
unsubscribed, `gnus-level-zombie` to be zombies (walking dead)
(default 8) and `gnus-level-killed` to be killed (completely dead)
(default 9). Gnus treats subscribed and unsubscribed groups exactly the
same, but zombie and killed groups store no information on what articles
you have read, etc. This distinction between dead and living
groups isn’t done because it is nice or clever, it is done purely for
reasons of efficiency.

It is recommended that you keep all your mail groups (if any) on quite
low levels (e.g., 1 or 2).

Maybe the following description of the default behavior of Gnus helps to
understand what these levels are all about. By default, Gnus shows you
subscribed nonempty groups, but by hitting `L` you can have it show
empty subscribed groups and unsubscribed groups, too. Type `l` to
go back to showing nonempty subscribed groups again. Thus, unsubscribed
groups are hidden, in a way.

Zombie and killed groups are similar to unsubscribed groups in that they
are hidden by default. But they are different from subscribed and
unsubscribed groups in that Gnus doesn’t ask the news server for
information (number of messages, number of unread messages) on zombie
and killed groups. Normally, you use `C-k` to kill the groups you
aren’t interested in. If most groups are killed, Gnus is faster.

Why does Gnus distinguish between zombie and killed groups? Well, when
a new group arrives on the server, Gnus by default makes it a zombie
group. This means that you are normally not bothered with new groups,
but you can type `A z` to get a list of all new groups. Subscribe
the ones you like and kill the ones you don’t want. (`A k` shows a
list of killed groups.)

If you want to play with the level variables, you should show some care.
Set them once, and don’t touch them ever again. Better yet, don’t touch
them at all unless you know exactly what you’re doing.

Two closely related variables are `gnus-level-default-subscribed`
(default 3) and `gnus-level-default-unsubscribed` (default 6),
which are the levels that new groups will be put on if they are
(un)subscribed. These two variables should, of course, be inside the
relevant valid ranges.

If `gnus-keep-same-level` is non-`nil`, some movement commands
will only move to groups of the same level (or lower). In
particular, going from the last article in one group to the next group
will go to the next group of the same level (or lower). This might be
handy if you want to read the most important groups before you read the
rest.

If this variable is `best`, Gnus will make the next newsgroup the
one with the best level.

All groups with a level less than or equal to
`gnus-group-default-list-level` will be listed in the group buffer
by default.
This variable can also be a function. In that case, that function will
be called and the result will be used as value.

If `gnus-group-list-inactive-groups` is non-`nil`, non-active
groups will be listed along with the unread groups. This variable is
`t` by default. If it is `nil`, inactive groups won’t be
listed.

If `gnus-group-use-permanent-levels` is non-`nil`, once you
give a level prefix to `g` or `l`, all subsequent commands will
use this level as the “work” level.

Gnus will normally just activate (i.e., query the server about) groups
on level `gnus-activate-level` or less. If you don’t want to
activate unsubscribed groups, for instance, you might set this variable
to 5. The default is 6.

---

Next: [Marking Groups](#Marking-Groups), Previous: [Group Levels](#Group-Levels), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.7 Group Score [¶](#Group-Score-1)

You would normally keep important groups on high levels, but that scheme
is somewhat restrictive. Don’t you wish you could have Gnus sort the
group buffer according to how often you read groups, perhaps? Within
reason?

This is what *group score* is for. You can have Gnus assign a score
to each group through the mechanism described below. You can then sort
the group buffer based on this score. Alternatively, you can sort on
score and then level. (Taken together, the level and the score is
called the *rank* of the group. A group that is on level 4 and has
a score of 1 has a higher rank than a group on level 5 that has a score
of 300. (The level is the most significant part and the score is the
least significant part.))

If you want groups you read often to get higher scores than groups you
read seldom you can add the `gnus-summary-bubble-group` function to
the `gnus-summary-exit-hook` hook. This will result (after
sorting) in a bubbling sort of action. If you want to see that in
action after each summary exit, you can add
`gnus-group-sort-groups-by-rank` or
`gnus-group-sort-groups-by-score` to the same hook, but that will
slow things down somewhat.

---

Next: [Foreign Groups](#Foreign-Groups), Previous: [Group Score](#Group-Score), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.8 Marking Groups [¶](#Marking-Groups-1)

If you want to perform some command on several groups, and they appear
subsequently in the group buffer, you would normally just give a
numerical prefix to the command. Most group commands will then do your
bidding on those groups.

However, if the groups are not in sequential order, you can still
perform a command on several groups. You simply mark the groups first
with the process mark and then execute the command.

`#` [¶](#index-M-m-_0028Group_0029)

`M m`
:   Toggle the process mark for the current group
    (`gnus-group-mark-group`).  
    If `gnus-process-mark-toggle` is `nil`, set the process mark
    for the current group.

`M-#` [¶](#index-M-u-_0028Group_0029)

`M u`
:   Remove the process mark, if any, from the current group
    (`gnus-group-unmark-group`).

`M U` [¶](#index-M-U-_0028Group_0029)
:   Remove the process mark from all groups (`gnus-group-unmark-all-groups`).

`M w` [¶](#index-M-w-_0028Group_0029)
:   Mark groups in region (`gnus-group-mark-region`).

`M b` [¶](#index-M-b-_0028Group_0029)
:   Mark all groups in the buffer (`gnus-group-mark-buffer`).

`M r` [¶](#index-M-r-_0028Group_0029)
:   Mark all groups that match some regular expression
    (`gnus-group-mark-regexp`).

Also see [Process/Prefix](#Process_002fPrefix).

If you want to execute some command on all groups that have been marked
with the process mark, you can use the `M-&`
(`gnus-group-universal-argument`) command. It will prompt you for
the command to be executed.

---

Next: [Group Parameters](#Group-Parameters), Previous: [Marking Groups](#Marking-Groups), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.9 Foreign Groups [¶](#Foreign-Groups-1)

If you recall how to subscribe to servers (see [Finding the News](#Finding-the-News))
you will remember that `gnus-secondary-select-methods` and
`gnus-select-method` let you write a definition in Emacs Lisp of
what servers you want to see when you start up. The alternate
approach is to use foreign servers and groups. “Foreign” here means
they are not coming from the select methods. All foreign server
configuration and subscriptions are stored only in the
`~/.newsrc.eld` file.

Below are some group mode commands for making and editing general foreign
groups, as well as commands to ease the creation of a few
special-purpose groups. All these commands insert the newly created
groups under point—`gnus-subscribe-newsgroup-method` is not
consulted.

Changes from the group editing commands are stored in
`~/.newsrc.eld` (`gnus-startup-file`). An alternative is the
variable `gnus-parameters`, See [Group Parameters](#Group-Parameters).

`G m` [¶](#index-G-m-_0028Group_0029)
:   Make a new group (`gnus-group-make-group`). Gnus will prompt you
    for a name, a method and possibly an *address*. For an easier way
    to subscribe to NNTP groups (see [Browse Foreign Server](#Browse-Foreign-Server)).

`G M` [¶](#index-G-M-_0028Group_0029)
:   Make an ephemeral group (`gnus-group-read-ephemeral-group`). Gnus
    will prompt you for a name, a method and an *address*.

`G r` [¶](#index-G-r-_0028Group_0029)
:   Rename the current group to something else
    (`gnus-group-rename-group`). This is valid only on some
    groups—mail groups mostly. This command might very well be quite slow
    on some back ends.

`G c` [¶](#index-G-c-_0028Group_0029)
:   Customize the group parameters (`gnus-group-customize`).

`G e` [¶](#index-G-e-_0028Group_0029)
:   Enter a buffer where you can edit the select method of the current
    group (`gnus-group-edit-group-method`).

`G p` [¶](#index-G-p-_0028Group_0029)
:   Enter a buffer where you can edit the group parameters
    (`gnus-group-edit-group-parameters`).

`G E` [¶](#index-G-E-_0028Group_0029)
:   Enter a buffer where you can edit the group info
    (`gnus-group-edit-group`).

`G d` [¶](#index-G-d-_0028Group_0029)
:   Make a directory group (see [Directory Groups](#Directory-Groups)). You will be prompted
    for the directory’s name (`gnus-group-make-directory-group`).

`G h` [¶](#index-G-h-_0028Group_0029)
:   Make the Gnus help group (`gnus-group-make-help-group`).

`G D` [¶](#index-G-D-_0028Group_0029)
:   Read an arbitrary directory as if it were a newsgroup with the
    `nneething` back end (`gnus-group-enter-directory`).
    See [Anything Groups](#Anything-Groups).

`G f` [¶](#index-G-f-_0028Group_0029)
:   Make a group based on some file or other
    (`gnus-group-make-doc-group`). If you give a prefix to this
    command, you will be prompted for a file name and a file type.
    Currently supported types are `mbox`, `babyl`,
    `digest`, `news`, `rnews`, `mmdf`, `forward`,
    `rfc934`, `rfc822-forward`, `mime-parts`,
    `standard-digest`, `slack-digest`, `clari-briefs`,
    `nsmail`, `outlook`, `oe-dbx`, and `mailman`. If
    you run this command without a prefix, Gnus will guess at the file
    type. See [Document Groups](#Document-Groups).

`G u` [¶](#index-G-u-_0028Group_0029)
:   Create one of the groups mentioned in `gnus-useful-groups`
    (`gnus-group-make-useful-group`).

`G w` [¶](#index-G-w-_0028Group_0029)
:   Make an ephemeral group based on a web search
    (`gnus-group-make-web-group`). If you give a prefix to this
    command, make a solid group instead. You will be prompted for the
    search engine type and the search string. Valid search engine types
    include `google` and `dejanews`.
    See [Web Searches](#Web-Searches).

    If you use the `google` search engine, you can limit the search
    to a particular group by using a match string like
    ‘`shaving group:alt.sysadmin.recovery`’.

`G R` [¶](#index-G-R-_0028Group_0029)
:   Make a group based on an RSS feed
    (`gnus-group-make-rss-group`). You will be prompted for an URL.
    See [RSS](#RSS).

`G DEL` [¶](#index-G-DEL-_0028Group_0029)
:   This function will delete the current group
    (`gnus-group-delete-group`). If given a prefix, this function will
    actually delete all the articles in the group, and forcibly remove the
    group itself from the face of the Earth. Use a prefix only if you are
    absolutely sure of what you are doing. This command can’t be used on
    read-only groups (like `nntp` groups), though.

`G V` [¶](#index-G-V-_0028Group_0029)
:   Make a new, fresh, empty `nnvirtual` group
    (`gnus-group-make-empty-virtual`). See [Virtual Groups](#Virtual-Groups).

`G v` [¶](#index-G-v-_0028Group_0029)
:   Add the current group to an `nnvirtual` group
    (`gnus-group-add-to-virtual`). Uses the process/prefix convention.

See [Select Methods](#Select-Methods), for more information on the various select
methods.

If `gnus-activate-foreign-newsgroups` is a positive number,
Gnus will check all foreign groups with this level or lower at startup.
This might take quite a while, especially if you subscribe to lots of
groups from different NNTP servers. Also see [Group Levels](#Group-Levels);
`gnus-activate-level` also affects activation of foreign
newsgroups.

The following commands create ephemeral groups. They can be called not
only from the Group buffer, but in any Gnus buffer.

`gnus-read-ephemeral-emacs-bug-group` [¶](#index-gnus_002dread_002dephemeral_002demacs_002dbug_002dgroup)
:   Read an Emacs bug report in an ephemeral group. Gnus will prompt for
    multiple bug numbers. The default is the number at point. The
    URL template is specified in
    `gnus-bug-group-download-format-alist`.

`gnus-read-ephemeral-debian-bug-group` [¶](#index-gnus_002dread_002dephemeral_002ddebian_002dbug_002dgroup)
:   Read a Debian bug report in an ephemeral group. Analog to
    `gnus-read-ephemeral-emacs-bug-group`.

Some of these command are also useful for article buttons, See [Article Buttons](#Article-Buttons).

Here is an example:

```
(require 'gnus-art)
(add-to-list
 'gnus-button-alist
 '("#\\([0-9]+\\)\\>" 1
   (string-match "\\<emacs\\>" (or gnus-newsgroup-name ""))
   gnus-read-ephemeral-emacs-bug-group 1))
```

---

Next: [Listing Groups](#Listing-Groups), Previous: [Foreign Groups](#Foreign-Groups), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.10 Group Parameters [¶](#Group-Parameters-1)

The group parameters store information local to a particular group.

Use the `G p` or the `G c` command to edit group parameters of a
group. (`G p` presents you with a Lisp-based interface, `G c`
presents you with a Customize-like interface. The latter helps avoid
silly Lisp errors.) You might also be interested in reading about topic
parameters (see [Topic Parameters](#Topic-Parameters)).
Additionally, you can set group parameters via the
`gnus-parameters` variable, see below.

Here’s an example group parameter list:

```
((to-address . "ding@gnus.org")
 (auto-expire . t))
```

We see that each element consists of a “dotted pair”—the thing before
the dot is the key, while the thing after the dot is the value. All the
parameters have this form *except* local variable specs, which are
not dotted pairs, but proper lists.

Some parameters have correspondent customizable variables, each of which
is an alist of regexps and values.

The following group parameters can be used:

`to-address` [¶](#index-to_002daddress)
:   Address used by when doing followups and new posts.

    ```
    (to-address . "some@where.com")
    ```

    This is primarily useful in mail groups that represent closed mailing
    lists—mailing lists where it’s expected that everybody that writes to
    the mailing list is subscribed to it. Since using this parameter
    ensures that the mail only goes to the mailing list itself, it means
    that members won’t receive two copies of your followups.

    Using `to-address` will actually work whether the group is foreign
    or not. Let’s say there’s a group on the server that is called
    ‘`fa.4ad-l`’. This is a real newsgroup, but the server has gotten
    the articles from a mail-to-news gateway. Posting directly to this
    group is therefore impossible—you have to send mail to the mailing
    list address instead.

    See also `gnus-parameter-to-address-alist`.

`to-list` [¶](#index-to_002dlist)
:   Address used when doing `a` in that group.

    ```
    (to-list . "some@where.com")
    ```

    It is totally ignored
    when doing a followup—except that if it is present in a news group,
    you’ll get mail group semantics when doing `f`.

    If you do an `a` command in a mail group and you have neither a
    `to-list` group parameter nor a `to-address` group parameter,
    then a `to-list` group parameter will be added automatically upon
    sending the message if `gnus-add-to-list` is set to `t`.

    If this variable is set, `gnus-mailing-list-mode` is turned on when
    entering summary buffer.

`subscribed` [¶](#index-subscribed)
:   If this parameter is set to `t`, Gnus will consider the
    to-address and to-list parameters for this group as addresses of
    mailing lists you are subscribed to. Giving Gnus this information is
    (only) a first step in getting it to generate correct Mail-Followup-To
    headers for your posts to these lists. The second step is to put the
    following in your `.gnus.el`

    ```
    (setq message-subscribed-address-functions
          '(gnus-find-subscribed-addresses))
    ```

    See [Mailing Lists](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Mailing-Lists) in The Message Manual, for
    a complete treatment of available MFT support.

`visible` [¶](#index-visible)
:   If the group parameter list has the element `(visible . t)`,
    that group will always be visible in the Group buffer, regardless
    of whether it has any unread articles.

    This parameter cannot be set via `gnus-parameters`. See
    `gnus-permanently-visible-groups` as an alternative.

`broken-reply-to` [¶](#index-broken_002dreply_002dto)
:   Elements like `(broken-reply-to . t)` signals that `Reply-To`
    headers in this group are to be ignored, and for the header to be hidden
    if `reply-to` is part of `gnus-boring-article-headers`. This
    can be useful if you’re reading a mailing list group where the listserv
    has inserted `Reply-To` headers that point back to the listserv
    itself. That is broken behavior. So there!

`to-group` [¶](#index-to_002dgroup)
:   Elements like `(to-group . "some.group.name")` means that all
    posts in that group will be sent to `some.group.name`.

`newsgroup` [¶](#index-newsgroup)
:   If you have `(newsgroup . t)` in the group parameter list, Gnus
    will treat all responses as if they were responses to news articles.
    This can be useful if you have a mail group that’s really a mirror of a
    news group.

`gcc-self` [¶](#index-gcc_002dself)
:   If `(gcc-self . t)` is present in the group parameter list, newly
    composed messages will be `gcc`d to the current group. If
    `(gcc-self . none)` is present, no `Gcc:` header will be
    generated, if `(gcc-self . "group")` is present, this string will
    be inserted literally as a `Gcc:` header. It should be a group
    name. The `gcc-self` value may also be a list of strings and
    `t`, e.g., `(gcc-self "group1" "group2" t)` means to
    `gcc` the newly composed message into the groups `"group1"`
    and `"group2"`, and into the current group. The `gcc-self`
    parameter takes precedence over any default `Gcc` rules as
    described later (see [Archived Messages](#Archived-Messages)), with the exception for
    messages to resend.

    **Caveat**: Adding `(gcc-self . t)` to the parameter list of
    `nntp` groups (or the like) isn’t valid. An `nntp` server
    doesn’t accept articles.

`auto-expire` [¶](#index-auto_002dexpire)
:   If the group parameter has an element that looks like `(auto-expire
    . t)`, all articles read will be marked as expirable. For an
    alternative approach, see [Expiring Mail](#Expiring-Mail).

    See also `gnus-auto-expirable-newsgroups`.

`total-expire` [¶](#index-total_002dexpire)
:   If the group parameter has an element that looks like
    `(total-expire . t)`, all read articles will be put through the
    expiry process, even if they are not marked as expirable. Use with
    caution. Unread, ticked and dormant articles are not eligible for
    expiry.

    See also `gnus-total-expirable-newsgroups`.

`expiry-wait` [¶](#index-expiry_002dwait)
:   If the group parameter has an element that looks like
    `(expiry-wait . 10)`, this value will override any
    `nnmail-expiry-wait` and `nnmail-expiry-wait-function`
    settings (see [Expiring Mail](#Expiring-Mail)) when expiring expirable messages.
    The value can be either a number of days (not necessarily an integer),
    or one of the symbols `never` or `immediate`.

`expiry-target` [¶](#index-expiry_002dtarget)
:   Where expired messages end up. This parameter overrides
    `nnmail-expiry-target`.

`score-file` [¶](#index-score-file-group-parameter)
:   Elements that look like `(score-file . "file")` will make
    `file` into the current score file for the group in question. All
    interactive score entries will be put into this file.

`adapt-file` [¶](#index-adapt-file-group-parameter)
:   Elements that look like `(adapt-file . "file")` will make
    `file` into the current adaptive file for the group in question.
    All adaptive score entries will be put into this file.

`admin-address` [¶](#index-admin_002daddress)
:   When unsubscribing from a mailing list you should never send the
    unsubscription notice to the mailing list itself. Instead, you’d send
    messages to the administrative address. This parameter allows you to
    put the admin address somewhere convenient.

`display` [¶](#index-display)
:   Elements that look like `(display . MODE)` say which articles to
    display on entering the group. Valid values are:

    `all`
    :   Display all articles, both read and unread.

    `an integer`
    :   Display the last integer articles in the group. This is the same as
        entering the group with `C-u integer`.

    `default`
    :   Display the default visible articles, which normally includes unread and
        ticked articles.

    `an array`
    :   Display articles that satisfy a predicate.

        Here are some examples:

        `[unread]`
        :   Display only unread articles.

        `[not expire]`
        :   Display everything except expirable articles.

        `[and (not reply) (not expire)]`
        :   Display everything except expirable and articles you’ve already
            responded to.

        The available operators are `not`, `and` and `or`.
        Predicates include `tick`, `unsend`, `undownload`,
        `unread`, `dormant`, `expire`, `reply`,
        `killed`, `bookmark`, `score`, `save`,
        `cache`, `forward`, and `unseen`.

    The `display` parameter works by limiting the summary buffer to
    the subset specified. You can pop the limit by using the `/ w`
    command (see [Limiting](#Limiting)).

`comment` [¶](#index-comment)
:   Elements that look like `(comment . "This is a comment")` are
    arbitrary comments on the group. You can display comments in the
    group line (see [Group Line Specification](#Group-Line-Specification)).

`charset` [¶](#index-charset)
:   Elements that look like `(charset . iso-8859-1)` will make
    `iso-8859-1` the default charset; that is, the charset that will be
    used for all articles that do not specify a charset.

    See also `gnus-group-charset-alist`.

`ignored-charsets` [¶](#index-ignored_002dcharset)
:   Elements that look like `(ignored-charsets x-unknown iso-8859-1)`
    will make `iso-8859-1` and `x-unknown` ignored; that is, the
    default charset will be used for decoding articles.

    See also `gnus-group-ignored-charsets-alist`.

`posting-style` [¶](#index-posting_002dstyle)
:   You can store additional posting style information for this group
    here (see [Posting Styles](#Posting-Styles)). The format is that of an entry in the
    `gnus-posting-styles` alist, except that there’s no regexp matching
    the group name (of course). Style elements in this group parameter will
    take precedence over the ones found in `gnus-posting-styles`.

    For instance, if you want a funky name and signature in this group only,
    instead of hacking `gnus-posting-styles`, you could put something
    like this in the group parameters:

    ```
    (posting-style
      (name "Funky Name")
      ("X-Message-SMTP-Method" "smtp smtp.example.org 587")
      ("X-My-Header" "Funky Value")
      (signature "Funky Signature"))
    ```

    If you’re using topics to organize your group buffer
    (see [Group Topics](#Group-Topics)), note that posting styles can also be set in
    the topics parameters. Posting styles in topic parameters apply to all
    groups in this topic. More precisely, the posting-style settings for a
    group result from the hierarchical merging of all posting-style
    entries in the parameters of this group and all the topics it belongs
    to.

`post-method` [¶](#index-post_002dmethod)
:   If it is set, the value is used as the method for posting message
    instead of `gnus-post-method`.

`mail-source` [¶](#index-mail_002dsource)
:   If it is set, and the setting of `mail-sources` includes a
    `group` mail source (see [Mail Sources](#Mail-Sources)), the value is a
    mail source for this group.

`banner` [¶](#index-banner)
:   An item like `(banner . regexp)` causes any part of an article
    that matches the regular expression regexp to be stripped. Instead of
    regexp, you can also use the symbol `signature` which strips the
    last signature or any of the elements of the alist
    `gnus-article-banner-alist`.

`sieve` [¶](#index-sieve)
:   This parameter contains a Sieve test that should match incoming mail
    that should be placed in this group. From this group parameter, a
    Sieve ‘`IF`’ control structure is generated, having the test as the
    condition and ‘`fileinto "group.name";`’ as the body.

    For example, if the ‘`INBOX.list.sieve`’ group has the `(sieve
    address "sender" "sieve-admin@extundo.com")` group parameter, when
    translating the group parameter into a Sieve script (see [Sieve Commands](#Sieve-Commands)) the following Sieve code is generated:

    ```
    if address "sender" "sieve-admin@extundo.com" {
            fileinto "INBOX.list.sieve";
    }
    ```

    To generate tests for multiple email-addresses use a group parameter
    like `(sieve address "sender" ("name@one.org" "else@two.org"))`.
    When generating a sieve script (see [Sieve Commands](#Sieve-Commands)) Sieve code
    like the following is generated:

    ```
    if address "sender" ["name@one.org", "else@two.org"] {
            fileinto "INBOX.list.sieve";
    }
    ```

    You can also use regexp expansions in the rules:

    ```
    (sieve header :regex "list-id" "<c++std-\\1.accu.org>")
    ```

    See [Sieve Commands](#Sieve-Commands), for commands and variables that might be of
    interest in relation to the sieve parameter.

    The Sieve language is described in RFC 3028. See [Top](https://www.gnu.org/software/emacs/manual/html_mono/sieve.html#Top) in Emacs Sieve.

`match-list` [¶](#index-match_002dlist)
:   If this parameter is set to `t` and `nnmail-split-method` is
    set to `gnus-group-split`, Gnus will match `to-address`,
    `to-list`, `extra-aliases` and `split-regexp` against
    the `list` split abbreviation. The split regexp is modified to
    match either a `@` or a dot `.` in mail addresses to
    conform to RFC2919 `List-ID`.

    See `nnmail-split-abbrev-alist` for the regular expression
    matching mailing-list headers.

    See [Group Mail Splitting](#Group-Mail-Splitting), for details on how to automatically split
    on group parameters.

`(agent parameters)`
:   If the agent has been enabled, you can set any of its parameters to
    control the behavior of the agent in individual groups. See Agent
    Parameters in [Category Syntax](#Category-Syntax). Most users will choose to set
    agent parameters in either an agent category or group topic to
    minimize the configuration effort.

`(variable form)`
:   You can use the group parameters to set variables local to the group you
    are entering. If you want to turn threading off in ‘`news.answers`’,
    you could put `(gnus-show-threads nil)` in the group parameters of
    that group. `gnus-show-threads` will be made into a local variable
    in the summary buffer you enter, and the form `nil` will be
    `eval`ed there.

    Note that this feature sets the variable locally to the summary buffer
    if and only if variable has been bound as a variable. Otherwise,
    only evaluating the form will take place. So, you may want to bind the
    variable in advance using `defvar` or other if the result of the
    form needs to be set to it.

    But some variables are evaluated in the article buffer, or in the
    message buffer (of a reply or followup or otherwise newly created
    message). As a workaround, it might help to add the variable in
    question to `gnus-newsgroup-variables`. See [Various Summary Stuff](#Various-Summary-Stuff). So if you want to set `message-from-style` via the group
    parameters, then you may need the following statement elsewhere in your
    `~/.gnus.el` file:

    ```
    (add-to-list 'gnus-newsgroup-variables 'message-from-style)
    ```

    A use for this feature is to remove a mailing list identifier tag in
    the subject fields of articles. E.g., if the news group

    ```
    nntp+news.gnus.org:gmane.text.docbook.apps
    ```

    has the tag ‘`DOC-BOOK-APPS:`’ in the subject of all articles, this
    tag can be removed from the article subjects in the summary buffer for
    the group by putting `(gnus-list-identifiers "DOCBOOK-APPS:")`
    into the group parameters for the group.

    This can also be used as a group-specific hook function. If you want to
    hear a beep when you enter a group, you could put something like
    `(dummy-variable (ding))` in the parameters of that group. If
    `dummy-variable` has been bound (see above), it will be set to the
    (meaningless) result of the `(ding)` form.

    Alternatively, since the VARIABLE becomes local to the group, this
    pattern can be used to temporarily change a hook. For example, if the
    following is added to a group parameter

    ```
    (gnus-summary-prepared-hook
      (lambda nil (keymap-local-set "d" (local-key-binding "n"))))
    ```

    when the group is entered, the ’d’ key will not mark the article as
    expired.

Group parameters can be set via the `gnus-parameters` variable too.
But some variables, such as `visible`, have no effect (For this
case see `gnus-permanently-visible-groups` as an alternative.).
For example:

```
(setq gnus-parameters
      '(("mail\\..*"
         (gnus-show-threads nil)
         (gnus-use-scoring nil)
         (gnus-summary-line-format
          "%U%R%z%I%(%[%d:%ub%-23,23f%]%) %s\n")
         (gcc-self . t)
         (display . all))

        ("^nnimap:\\(foo.bar\\)$"
         (to-group . "\\1"))

        ("mail\\.me"
         (gnus-use-scoring t))

        ("list\\..*"
         (total-expire . t)
         (broken-reply-to . t))))
```

All clauses that match the group name will be used, but the last
setting “wins”. So if you have two clauses that both match the
group name, and both set, say `display`, the last setting will
override the first.

Parameters that are strings will be subjected to regexp substitution,
as the `to-group` example shows.

By default, whether comparing the group name and one of those regexps
specified in `gnus-parameters` is done in a case-sensitive manner
or a case-insensitive manner depends on the value of
`case-fold-search` at the time when the comparison is done. The
value of `case-fold-search` is typically `t`; it means, for
example, the element `("INBOX\\.FOO" (total-expire . t))` might be
applied to both the ‘`INBOX.FOO`’ group and the ‘`INBOX.foo`’
group. If you want to make those regexps always case-sensitive, set the
value of the `gnus-parameters-case-fold-search` variable to
`nil`. Otherwise, set it to `t` if you want to compare them
always in a case-insensitive manner.

You can define different sorting to different groups via
`gnus-parameters`. Here is an example to sort an NNTP
group by reverse date to see the latest news at the top and an
RSS group by subject. In this example, the first group is the
Debian daily news group `gmane.linux.debian.user.news` from
news.gmane.io. The RSS group corresponds to the Debian
weekly news RSS feed
<https://packages.debian.org/unstable/newpkg_main.en.rdf>,
See [RSS](#RSS).

```
(setq
 gnus-parameters
 '(("nntp.*gmane\\.debian\\.user\\.news"
    (gnus-show-threads nil)
    (gnus-article-sort-functions '((not gnus-article-sort-by-date)))
    (gnus-use-adaptive-scoring nil)
    (gnus-use-scoring nil))
   ("nnrss.*debian"
    (gnus-show-threads nil)
    (gnus-article-sort-functions 'gnus-article-sort-by-subject)
    (gnus-use-adaptive-scoring nil)
    (gnus-use-scoring t)
    (gnus-score-find-score-files-function 'gnus-score-find-single)
    (gnus-summary-line-format "%U%R%z%d %I%(%[ %s %]%)\n"))))
```

---

Next: [Sorting Groups](#Sorting-Groups), Previous: [Group Parameters](#Group-Parameters), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.11 Listing Groups [¶](#Listing-Groups-1)

These commands all list various slices of the groups available.

`l` [¶](#index-A-s-_0028Group_0029)

`A s`
:   List all groups that have unread articles
    (`gnus-group-list-groups`). If the numeric prefix is used, this
    command will list only groups of level ARG and lower. By default, it
    only lists groups of level five (i.e.,
    `gnus-group-default-list-level`) or lower (i.e., just subscribed
    groups).

`L` [¶](#index-A-u-_0028Group_0029)

`A u`
:   List all groups, whether they have unread articles or not
    (`gnus-group-list-all-groups`). If the numeric prefix is used,
    this command will list only groups of level ARG and lower. By default,
    it lists groups of level seven or lower (i.e., just subscribed and
    unsubscribed groups).

`A l` [¶](#index-A-l-_0028Group_0029)
:   List all unread groups on a specific level
    (`gnus-group-list-level`). If given a prefix, also list the groups
    with no unread articles.

`A k` [¶](#index-A-k-_0028Group_0029)
:   List all killed groups (`gnus-group-list-killed`). If given a
    prefix argument, really list all groups that are available, but aren’t
    currently (un)subscribed. This could entail reading the active file
    from the server.

`A z` [¶](#index-A-z-_0028Group_0029)
:   List all zombie groups (`gnus-group-list-zombies`).

`A m` [¶](#index-A-m-_0028Group_0029)
:   List all unread, subscribed groups with names that match a regexp
    (`gnus-group-list-matching`).

`A M` [¶](#index-A-M-_0028Group_0029)
:   List groups that match a regexp (`gnus-group-list-all-matching`).

`A A` [¶](#index-A-A-_0028Group_0029)
:   List absolutely all groups in the active file(s) of the
    server(s) you are connected to (`gnus-group-list-active`). This
    might very well take quite a while. It might actually be a better idea
    to do a `A M` to list all matching, and just give ‘`.`’ as the
    thing to match on. Also note that this command may list groups that
    don’t exist (yet)—these will be listed as if they were killed groups.
    Take the output with some grains of salt.

`A a` [¶](#index-A-a-_0028Group_0029)
:   List all groups that have names that match a regexp
    (`gnus-group-apropos`).

`A d` [¶](#index-A-d-_0028Group_0029)
:   List all groups that have names or descriptions that match a regexp
    (`gnus-group-description-apropos`).

`A c` [¶](#index-A-c-_0028Group_0029)
:   List all groups with cached articles (`gnus-group-list-cached`).

`A ?` [¶](#index-A-_003f-_0028Group_0029)
:   List all groups with dormant articles (`gnus-group-list-dormant`).

`A !` [¶](#index-A-_0021-_0028Group_0029)
:   List all groups with ticked articles (`gnus-group-list-ticked`).

`A /` [¶](#index-A-_002f-_0028Group_0029)
:   Further limit groups within the current selection
    (`gnus-group-list-limit`). If you’ve first limited to groups
    with dormant articles with `A ?`, you can then further limit with
    `A / c`, which will then limit to groups with cached articles,
    giving you the groups that have both dormant articles and cached
    articles.

`A f` [¶](#index-A-f-_0028Group_0029)
:   Flush groups from the current selection (`gnus-group-list-flush`).

`A p` [¶](#index-A-p-_0028Group_0029)
:   List groups plus the current selection (`gnus-group-list-plus`).

Groups that match the `gnus-permanently-visible-groups` regexp will
always be shown, whether they have unread articles or not. You can also
add the `visible` element to the group parameters in question to
get the same effect.

Groups that have just ticked articles in it are normally listed in the
group buffer. If `gnus-list-groups-with-ticked-articles` is
`nil`, these groups will be treated just like totally empty
groups. It is `t` by default.

---

Next: [Group Maintenance](#Group-Maintenance), Previous: [Listing Groups](#Listing-Groups), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.12 Sorting Groups [¶](#Sorting-Groups-1)

The `C-c C-s` (`gnus-group-sort-groups`) command sorts the
group buffer according to the function(s) given by the
`gnus-group-sort-function` variable. Available sorting functions
include:

`gnus-group-sort-by-alphabet` [¶](#index-gnus_002dgroup_002dsort_002dby_002dalphabet)
:   Sort the group names alphabetically. This is the default.

`gnus-group-sort-by-real-name` [¶](#index-gnus_002dgroup_002dsort_002dby_002dreal_002dname)
:   Sort the group alphabetically on the real (unprefixed) group names.

`gnus-group-sort-by-level` [¶](#index-gnus_002dgroup_002dsort_002dby_002dlevel)
:   Sort by group level.

`gnus-group-sort-by-score` [¶](#index-gnus_002dgroup_002dsort_002dby_002dscore)
:   Sort by group score. See [Group Score](#Group-Score).

`gnus-group-sort-by-rank` [¶](#index-gnus_002dgroup_002dsort_002dby_002drank)
:   Sort by group score and then the group level. The level and the score
    are, when taken together, the group’s *rank*. See [Group Score](#Group-Score).

`gnus-group-sort-by-unread` [¶](#index-gnus_002dgroup_002dsort_002dby_002dunread)
:   Sort by number of unread articles.

`gnus-group-sort-by-method` [¶](#index-gnus_002dgroup_002dsort_002dby_002dmethod)
:   Sort alphabetically on the select method.

`gnus-group-sort-by-server` [¶](#index-gnus_002dgroup_002dsort_002dby_002dserver)
:   Sort alphabetically on the Gnus server name.

`gnus-group-sort-function` can also be a list of sorting
functions. In that case, the most significant sort key function must be
the last one.

There are also a number of commands for sorting directly according to
some sorting criteria:

`G S a` [¶](#index-G-S-a-_0028Group_0029)
:   Sort the group buffer alphabetically by group name
    (`gnus-group-sort-groups-by-alphabet`).

`G S u` [¶](#index-G-S-u-_0028Group_0029)
:   Sort the group buffer by the number of unread articles
    (`gnus-group-sort-groups-by-unread`).

`G S l` [¶](#index-G-S-l-_0028Group_0029)
:   Sort the group buffer by group level
    (`gnus-group-sort-groups-by-level`).

`G S v` [¶](#index-G-S-v-_0028Group_0029)
:   Sort the group buffer by group score
    (`gnus-group-sort-groups-by-score`). See [Group Score](#Group-Score).

`G S r` [¶](#index-G-S-r-_0028Group_0029)
:   Sort the group buffer by group rank
    (`gnus-group-sort-groups-by-rank`). See [Group Score](#Group-Score).

`G S m` [¶](#index-G-S-m-_0028Group_0029)
:   Sort the group buffer alphabetically by back end name  
    (`gnus-group-sort-groups-by-method`).

`G S n` [¶](#index-G-S-n-_0028Group_0029)
:   Sort the group buffer alphabetically by real (unprefixed) group name
    (`gnus-group-sort-groups-by-real-name`).

All the commands below obey the process/prefix convention
(see [Process/Prefix](#Process_002fPrefix)).

When given a symbolic prefix (see [Symbolic Prefixes](#Symbolic-Prefixes)), all these
commands will sort in reverse order.

You can also sort a subset of the groups:

`G P a` [¶](#index-G-P-a-_0028Group_0029)
:   Sort the groups alphabetically by group name
    (`gnus-group-sort-selected-groups-by-alphabet`).

`G P u` [¶](#index-G-P-u-_0028Group_0029)
:   Sort the groups by the number of unread articles
    (`gnus-group-sort-selected-groups-by-unread`).

`G P l` [¶](#index-G-P-l-_0028Group_0029)
:   Sort the groups by group level
    (`gnus-group-sort-selected-groups-by-level`).

`G P v` [¶](#index-G-P-v-_0028Group_0029)
:   Sort the groups by group score
    (`gnus-group-sort-selected-groups-by-score`). See [Group Score](#Group-Score).

`G P r` [¶](#index-G-P-r-_0028Group_0029)
:   Sort the groups by group rank
    (`gnus-group-sort-selected-groups-by-rank`). See [Group Score](#Group-Score).

`G P m` [¶](#index-G-P-m-_0028Group_0029)
:   Sort the groups alphabetically by back end name  
    (`gnus-group-sort-selected-groups-by-method`).

`G P n` [¶](#index-G-P-n-_0028Group_0029)
:   Sort the groups alphabetically by real (unprefixed) group name
    (`gnus-group-sort-selected-groups-by-real-name`).

`G P s` [¶](#index-G-P-s-_0028Group_0029)
:   Sort the groups according to `gnus-group-sort-function`.

And finally, note that you can use `C-k` and `C-y` to manually
move groups around.

---

Next: [Browse Foreign Server](#Browse-Foreign-Server), Previous: [Sorting Groups](#Sorting-Groups), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.13 Group Maintenance [¶](#Group-Maintenance-1)

`b` [¶](#index-b-_0028Group_0029)
:   Find bogus groups and delete them
    (`gnus-group-check-bogus-groups`).

`F` [¶](#index-F-_0028Group_0029)
:   Find new groups and process them (`gnus-group-find-new-groups`).
    With 1 `C-u`, use the `ask-server` method to query the server
    for new groups. With 2 `C-u`’s, use most complete method possible
    to query the server for new groups, and subscribe the new groups as
    zombies.

`C-c C-x` [¶](#index-C_002dc-C_002dx-_0028Group_0029)
:   Run all expirable articles in the current group through the expiry
    process (if any) (`gnus-group-expire-articles`). That is, delete
    all expirable articles in the group that have been around for a while.
    (see [Expiring Mail](#Expiring-Mail)).

`C-c C-M-x` [¶](#index-C_002dc-C_002dM_002dx-_0028Group_0029)
:   Run all expirable articles in all groups through the expiry process
    (`gnus-group-expire-all-groups`).

---

Next: [Exiting Gnus](#Exiting-Gnus), Previous: [Group Maintenance](#Group-Maintenance), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.14 Browse Foreign Server [¶](#Browse-Foreign-Server-1)

`B` [¶](#index-B-_0028Group_0029-1)
:   You will be queried for a select method and a server name. Gnus will
    then attempt to contact this server and let you browse the groups there
    (`gnus-group-browse-foreign-server`).

A new buffer with a list of available groups will appear. This buffer
will use the `gnus-browse-mode`. This buffer looks a bit (well,
a lot) like a normal group buffer.

Here’s a list of keystrokes available in the browse mode:

`n` [¶](#index-n-_0028Browse_0029)
:   Go to the next group (`gnus-group-next-group`).

`p` [¶](#index-p-_0028Browse_0029)
:   Go to the previous group (`gnus-group-prev-group`).

`SPC` [¶](#index-SPC-_0028Browse_0029)
:   Enter the current group and display the first article
    (`gnus-browse-read-group`).

`RET` [¶](#index-RET-_0028Browse_0029)
:   Enter the current group (`gnus-browse-select-group`).

`u` [¶](#index-u-_0028Browse_0029)
:   Toggle subscription of the current group
    (`gnus-browse-toggle-subscription`). You
    can affect the way the new group is entered into the Group buffer
    using the variable `gnus-browse-subscribe-newsgroup-method`. See
    see [Subscription Methods](#Subscription-Methods) for available options.

`l` [¶](#index-q-_0028Browse_0029)

`q`
:   Exit browse mode (`gnus-browse-exit`).

`d` [¶](#index-d-_0028Browse_0029)
:   Describe the current group (`gnus-browse-describe-group`).

`?` [¶](#index-_003f-_0028Browse_0029)
:   Describe browse mode briefly (well, there’s not much to describe, is
    there) (`gnus-browse-describe-briefly`).

`DEL` [¶](#index-DEL-_0028Browse_0029)
:   This function will delete the current group
    (`gnus-browse-delete-group`). If given a prefix, this function
    will actually delete all the articles in the group, and forcibly
    remove the group itself from the face of the Earth. Use a prefix only
    if you are absolutely sure of what you are doing.

---

Next: [Group Topics](#Group-Topics), Previous: [Browse Foreign Server](#Browse-Foreign-Server), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.15 Exiting Gnus [¶](#Exiting-Gnus-1)

Yes, Gnus is ex(c)iting.

`z` [¶](#index-z-_0028Group_0029)
:   Suspend Gnus (`gnus-group-suspend`). This doesn’t really exit Gnus,
    but it kills all buffers except the Group buffer. I’m not sure why this
    is a gain, but then who am I to judge?

`q` [¶](#index-q-_0028Group_0029)
:   Quit Gnus (`gnus-group-exit`).

`Q` [¶](#index-Q-_0028Group_0029)
:   Quit Gnus without saving the `.newsrc` files (`gnus-group-quit`).
    The dribble file will be saved, though (see [Auto Save](#Auto-Save)).

`gnus-suspend-gnus-hook` is called when you suspend Gnus and
`gnus-exit-gnus-hook` is called when you quit Gnus, while
`gnus-after-exiting-gnus-hook` is called as the final item when
exiting Gnus.

---

Next: [Accessing groups of non-English names](#Non_002dASCII-Group-Names), Previous: [Exiting Gnus](#Exiting-Gnus), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.16 Group Topics [¶](#Group-Topics-1)

If you read lots and lots of groups, it might be convenient to group
them hierarchically according to topics. You put your Emacs groups over
here, your sex groups over there, and the rest (what, two groups or so?)
you put in some misc section that you never bother with anyway. You can
even group the Emacs sex groups as a sub-topic to either the Emacs
groups or the sex groups—or both! Go wild!

Here’s an example:

```
Gnus
  Emacs -- I wuw it!
     3: comp.emacs
     2: alt.religion.emacs
    Naughty Emacs
     452: alt.sex.emacs
       0: comp.talk.emacs.recovery
  Misc
     8: comp.binaries.fractals
    13: comp.sources.unix
```

To get this *fab* functionality you simply turn on (ooh!) the
`gnus-topic` minor mode—type `t` in the group buffer. (This
is a toggling command.)

Go ahead, just try it. I’ll still be here when you get back. La de
dum… Nice tune, that… la la la… What, you’re back?
Yes, and now press `l`. There. All your groups are now listed
under ‘`misc`’. Doesn’t that make you feel all warm and fuzzy?
Hot and bothered?

If you want this permanently enabled, you should add that minor mode to
the hook for the group mode. Put the following line in your
`~/.gnus.el` file:

```
(add-hook 'gnus-group-mode-hook 'gnus-topic-mode)
```

* [Topic Commands](#Topic-Commands)
* [Topic Variables](#Topic-Variables)
* [Topic Sorting](#Topic-Sorting)
* [Topic Topology](#Topic-Topology)
* [Topic Parameters](#Topic-Parameters)

---

Next: [Topic Variables](#Topic-Variables), Up: [Group Topics](#Group-Topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.16.1 Topic Commands [¶](#Topic-Commands-1)

When the topic minor mode is turned on, a new `T` submap will be
available. In addition, a few of the standard keys change their
definitions slightly.

In general, the following kinds of operations are possible on topics.
First of all, you want to create topics. Secondly, you want to put
groups in topics and to move them around until you have an order you
like. The third kind of operation is to show/hide parts of the whole
shebang. You might want to hide a topic including its subtopics and
groups, to get a better overview of the other groups.

Here is a list of the basic keys that you might need to set up topics
the way you like.

`T n` [¶](#index-T-n-_0028Topic_0029)
:   Prompt for a new topic name and create it
    (`gnus-topic-create-topic`).

`T TAB` [¶](#index-T-TAB-_0028Topic_0029)

`TAB`
:   “Indent” the current topic so that it becomes a sub-topic of the
    previous topic (`gnus-topic-indent`). If given a prefix,
    “un-indent” the topic instead.

`M-TAB` [¶](#index-M_002dTAB-_0028Topic_0029)
:   “Un-indent” the current topic so that it becomes a sub-topic of the
    parent of its current parent (`gnus-topic-unindent`).

The following two keys can be used to move groups and topics around.
They work like the well-known cut and paste. `C-k` is like cut and
`C-y` is like paste. Of course, this being Emacs, we use the terms
kill and yank rather than cut and paste.

`C-k` [¶](#index-C_002dk-_0028Topic_0029)
:   Kill a group or topic (`gnus-topic-kill-group`). All groups in the
    topic will be removed along with the topic.

`C-y` [¶](#index-C_002dy-_0028Topic_0029)
:   Yank the previously killed group or topic
    (`gnus-topic-yank-group`). Note that all topics will be yanked
    before all groups.

    So, to move a topic to the beginning of the list of topics, just hit
    `C-k` on it. This is like the “cut” part of cut and paste. Then,
    move the cursor to the beginning of the buffer (just below the “Gnus”
    topic) and hit `C-y`. This is like the “paste” part of cut and
    paste. Like I said—E-Z.

    You can use `C-k` and `C-y` on groups as well as on topics. So
    you can move topics around as well as groups.

After setting up the topics the way you like them, you might wish to
hide a topic, or to show it again. That’s why we have the following
key.

`RET`

`SPC`
:   Either select a group or fold a topic (`gnus-topic-select-group`).
    When you perform this command on a group, you’ll enter the group, as
    usual. When done on a topic line, the topic will be folded (if it was
    visible) or unfolded (if it was folded already). So it’s basically a
    toggling command on topics. In addition, if you give a numerical
    prefix, group on that level (and lower) will be displayed.

Now for a list of other commands, in no particular order.

`T m` [¶](#index-T-m-_0028Topic_0029)
:   Move the current group to some other topic
    (`gnus-topic-move-group`). This command uses the process/prefix
    convention (see [Process/Prefix](#Process_002fPrefix)).

`T j` [¶](#index-T-j-_0028Topic_0029)
:   Go to a topic (`gnus-topic-jump-to-topic`).

`T c` [¶](#index-T-c-_0028Topic_0029)
:   Copy the current group to some other topic
    (`gnus-topic-copy-group`). This command uses the process/prefix
    convention (see [Process/Prefix](#Process_002fPrefix)).

`T h` [¶](#index-T-h-_0028Topic_0029)
:   Hide the current topic (`gnus-topic-hide-topic`). If given
    a prefix, hide the topic permanently.

`T s` [¶](#index-T-s-_0028Topic_0029)
:   Show the current topic (`gnus-topic-show-topic`). If given
    a prefix, show the topic permanently.

`T D` [¶](#index-T-D-_0028Topic_0029)
:   Remove a group from the current topic (`gnus-topic-remove-group`).
    This command is mainly useful if you have the same group in several
    topics and wish to remove it from one of the topics. You may also
    remove a group from all topics, but in that case, Gnus will add it to
    the root topic the next time you start Gnus. In fact, all new groups
    (which, naturally, don’t belong to any topic) will show up in the root
    topic.

    This command uses the process/prefix convention
    (see [Process/Prefix](#Process_002fPrefix)).

`T M` [¶](#index-T-M-_0028Topic_0029)
:   Move all groups that match some regular expression to a topic
    (`gnus-topic-move-matching`).

`T C` [¶](#index-T-C-_0028Topic_0029)
:   Copy all groups that match some regular expression to a topic
    (`gnus-topic-copy-matching`).

`T H` [¶](#index-T-H-_0028Topic_0029)
:   Toggle hiding empty topics
    (`gnus-topic-toggle-display-empty-topics`).

`T #` [¶](#index-T-_0023-_0028Topic_0029)
:   Toggle the process mark for all groups in the current topic
    (`gnus-topic-mark-topic`). This command works recursively on
    sub-topics unless given a prefix.  
    If `gnus-process-mark-toggle` is `nil`, set the process mark
    for the current topic.

`T M-#` [¶](#index-T-M_002d_0023-_0028Topic_0029)
:   Remove the process mark from all groups in the current topic
    (`gnus-topic-unmark-topic`). This command works recursively on
    sub-topics unless given a prefix.

`C-c C-x` [¶](#index-C_002dc-C_002dx-_0028Topic_0029)
:   Run all expirable articles in the current group or topic through the
    expiry process (if any)
    (`gnus-topic-expire-articles`). (see [Expiring Mail](#Expiring-Mail)).

`T r` [¶](#index-T-r-_0028Topic_0029)
:   Rename a topic (`gnus-topic-rename`).

`T DEL` [¶](#index-T-DEL-_0028Topic_0029)
:   Delete an empty topic (`gnus-topic-delete`).

`A T` [¶](#index-A-T-_0028Topic_0029)
:   List all groups that Gnus knows about in a topics-ified way
    (`gnus-topic-list-active`).

`T M-n` [¶](#index-T-M_002dn-_0028Topic_0029)
:   Go to the next topic (`gnus-topic-goto-next-topic`).

`T M-p` [¶](#index-T-M_002dp-_0028Topic_0029)
:   Go to the previous topic (`gnus-topic-goto-previous-topic`).

`G p` [¶](#index-G-p-_0028Topic_0029)
:   Edit the topic parameters (`gnus-topic-edit-parameters`).
    See [Topic Parameters](#Topic-Parameters).

---

Next: [Topic Sorting](#Topic-Sorting), Previous: [Topic Commands](#Topic-Commands), Up: [Group Topics](#Group-Topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.16.2 Topic Variables [¶](#Topic-Variables-1)

The previous section told you how to tell Gnus which topics to display.
This section explains how to tell Gnus what to display about each topic.

The topic lines themselves are created according to the
`gnus-topic-line-format` variable (see [Formatting Variables](#Formatting-Variables)).
Valid elements are:

‘`i`’
:   Indentation.

‘`n`’
:   Topic name.

‘`v`’
:   Visibility.

‘`l`’
:   Level.

‘`g`’
:   Number of groups in the topic.

‘`G`’
:   Number of groups in the topic and all its subtopics.

‘`a`’
:   Number of unread articles in the topic.

‘`A`’
:   Number of unread articles in the topic and all its subtopics.

Each sub-topic (and the groups in the sub-topics) will be indented with
`gnus-topic-indent-level` times the topic level number of spaces.
The default is 2.

`gnus-topic-mode-hook` is called in topic minor mode buffers.

The `gnus-topic-display-empty-topics` says whether to display even
topics that have no unread articles in them. The default is `t`.

If `gnus-topic-display-predicate` is non-`nil`, it should be
a function that says whether the topic is to be displayed or not.
The function will be called with one parameter (the name of the topic)
and should return non-`nil` is the topic is to be displayed.

For instance, if you don’t even want to be reminded that work exists
outside of office hours, you can gather all the work-related groups
into a topic called ‘`"Work"`’, and then say something like the
following:

```
(setq gnus-topic-display-predicate
      (lambda (name)
        (or (not (equal name "Work"))
            (< 090000
               (string-to-number (format-time-string "%H%M%S"))
               170000))))
```

---

Next: [Topic Topology](#Topic-Topology), Previous: [Topic Variables](#Topic-Variables), Up: [Group Topics](#Group-Topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.16.3 Topic Sorting [¶](#Topic-Sorting-1)

You can sort the groups in each topic individually with the following
commands:

`T S a` [¶](#index-T-S-a-_0028Topic_0029)
:   Sort the current topic alphabetically by group name
    (`gnus-topic-sort-groups-by-alphabet`).

`T S u` [¶](#index-T-S-u-_0028Topic_0029)
:   Sort the current topic by the number of unread articles
    (`gnus-topic-sort-groups-by-unread`).

`T S l` [¶](#index-T-S-l-_0028Topic_0029)
:   Sort the current topic by group level
    (`gnus-topic-sort-groups-by-level`).

`T S v` [¶](#index-T-S-v-_0028Topic_0029)
:   Sort the current topic by group score
    (`gnus-topic-sort-groups-by-score`). See [Group Score](#Group-Score).

`T S r` [¶](#index-T-S-r-_0028Topic_0029)
:   Sort the current topic by group rank
    (`gnus-topic-sort-groups-by-rank`). See [Group Score](#Group-Score).

`T S m` [¶](#index-T-S-m-_0028Topic_0029)
:   Sort the current topic alphabetically by back end name
    (`gnus-topic-sort-groups-by-method`).

`T S e` [¶](#index-T-S-e-_0028Topic_0029)
:   Sort the current topic alphabetically by server name
    (`gnus-topic-sort-groups-by-server`).

`T S s` [¶](#index-T-S-s-_0028Topic_0029)
:   Sort the current topic according to the function(s) given by the
    `gnus-group-sort-function` variable
    (`gnus-topic-sort-groups`).

When given a prefix argument, all these commands will sort in reverse
order. See [Sorting Groups](#Sorting-Groups), for more information about group
sorting.

---

Next: [Topic Parameters](#Topic-Parameters), Previous: [Topic Sorting](#Topic-Sorting), Up: [Group Topics](#Group-Topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.16.4 Topic Topology [¶](#Topic-Topology-1)

So, let’s have a look at an example group buffer:

```
Gnus
  Emacs -- I wuw it!
     3: comp.emacs
     2: alt.religion.emacs
    Naughty Emacs
     452: alt.sex.emacs
       0: comp.talk.emacs.recovery
  Misc
     8: comp.binaries.fractals
    13: comp.sources.unix
```

So, here we have one top-level topic (‘`Gnus`’), two topics under
that, and one sub-topic under one of the sub-topics. (There is always
just one (1) top-level topic). This topology can be expressed as
follows:

```
(("Gnus" visible)
 (("Emacs -- I wuw it!" visible)
  (("Naughty Emacs" visible)))
 (("Misc" visible)))
```

This is in fact how the variable `gnus-topic-topology` would look
for the display above. That variable is saved in the `.newsrc.eld`
file, and shouldn’t be messed with manually—unless you really want
to. Since this variable is read from the `.newsrc.eld` file,
setting it in any other startup files will have no effect.

This topology shows what topics are sub-topics of what topics (right),
and which topics are visible. Two settings are currently
allowed—`visible` and `invisible`.

---

Previous: [Topic Topology](#Topic-Topology), Up: [Group Topics](#Group-Topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.16.5 Topic Parameters [¶](#Topic-Parameters-1)

All groups in a topic will inherit group parameters from the parent
(and ancestor) topic parameters. All valid group parameters are valid
topic parameters (see [Group Parameters](#Group-Parameters)). When the agent is
enabled, all agent parameters (See Agent Parameters in [Category Syntax](#Category-Syntax)) are also valid topic parameters.

In addition, the following parameters are only valid as topic
parameters:

`subscribe`
:   When subscribing new groups by topic (see [Subscription Methods](#Subscription-Methods)), the
    `subscribe` topic parameter says what groups go in what topic. Its
    value should be a regexp to match the groups that should go in that
    topic.

`subscribe-level`
:   When subscribing new groups by topic (see the `subscribe` parameter),
    the group will be subscribed with the level specified in the
    `subscribe-level` instead of `gnus-level-default-subscribed`.

Group parameters (of course) override topic parameters, and topic
parameters in sub-topics override topic parameters in super-topics. You
know. Normal inheritance rules. (*Rules* is here a noun, not a
verb, although you may feel free to disagree with me here.)

```
Gnus
  Emacs
     3: comp.emacs
     2: alt.religion.emacs
   452: alt.sex.emacs
    Relief
     452: alt.sex.emacs
       0: comp.talk.emacs.recovery
  Misc
     8: comp.binaries.fractals
    13: comp.sources.unix
   452: alt.sex.emacs
```

The ‘`Emacs`’ topic has the topic parameter `(score-file
. "emacs.SCORE")`; the ‘`Relief`’ topic has the topic parameter
`(score-file . "relief.SCORE")`; and the ‘`Misc`’ topic has the
topic parameter `(score-file . "emacs.SCORE")`. In addition,
  
 ‘`alt.religion.emacs`’ has the group parameter `(score-file
. "religion.SCORE")`.

Now, when you enter ‘`alt.sex.emacs`’ in the ‘`Relief`’ topic, you
will get the `relief.SCORE` home score file. If you enter the same
group in the ‘`Emacs`’ topic, you’ll get the `emacs.SCORE` home
score file. If you enter the group ‘`alt.religion.emacs`’, you’ll
get the `religion.SCORE` home score file.

This seems rather simple and self-evident, doesn’t it? Well, yes. But
there are some problems, especially with the `total-expiry`
parameter. Say you have a mail group in two topics; one with
`total-expiry` and one without. What happens when you do `M-x
gnus-expire-all-expirable-groups`? Gnus has no way of telling which one
of these topics you mean to expire articles from, so anything may
happen. In fact, I hereby declare that it is *undefined* what
happens. You just have to be careful if you do stuff like that.

---

Next: [Misc Group Stuff](#Misc-Group-Stuff), Previous: [Group Topics](#Group-Topics), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.17 Accessing groups of non-English names [¶](#Accessing-groups-of-non_002dEnglish-names)

There are some news servers that provide groups of which the names are
expressed with their native languages in the world. For instance, in a
certain news server there are some newsgroups of which the names are
spelled in Chinese, where people are talking in Chinese. You can, of
course, subscribe to such news groups using Gnus. Currently Gnus
supports non-ASCII group names not only with the `nntp`
back end but also with the `nnml` back end and the `nnrss`
back end.

Every such group name is encoded by a certain charset in the server
side (in an NNTP server its administrator determines the
charset, but for groups in the other back ends it is determined by you).
Gnus has to display the decoded ones for you in the group buffer and the
article buffer, and needs to use the encoded ones when communicating
with servers. However, Gnus doesn’t know what charset is used for each
non-ASCII group name. The following two variables are just
the ones for telling Gnus what charset should be used for each group:

`gnus-group-name-charset-method-alist` [¶](#index-gnus_002dgroup_002dname_002dcharset_002dmethod_002dalist)
:   An alist of select methods and charsets. The default value is
    `nil`. The names of groups in the server specified by that select
    method are all supposed to use the corresponding charset. For example:

    ```
    (setq gnus-group-name-charset-method-alist
          '(((nntp "news.com.cn") . cn-gb-2312)))
    ```

    Charsets specified for groups with this variable are preferred to the
    ones specified for the same groups with the
    `gnus-group-name-charset-group-alist` variable (see below).

    A select method can be very long, like:

    ```
    (nntp "gmane"
          (nntp-address "news.gmane.io")
          (nntp-end-of-line "\n")
          (nntp-open-connection-function
           nntp-open-via-rlogin-and-telnet)
          (nntp-via-rlogin-command "ssh")
          (nntp-via-rlogin-command-switches
           ("-C" "-t" "-e" "none"))
          (nntp-via-address ...))
    ```

    In that case, you can truncate it into `(nntp "gmane")` in this
    variable. That is, it is enough to contain only the back end name and
    the server name.

`gnus-group-name-charset-group-alist` [¶](#index-UTF_002d8-group-names)
:   An alist of regexp of group name and the charset for group names.
    `((".*" . utf-8))` is the default value if UTF-8 is supported,
    otherwise the default is `nil`. For example:

    ```
    (setq gnus-group-name-charset-group-alist
          '(("\\.com\\.cn:" . cn-gb-2312)
            (".*" . utf-8)))
    ```

    Note that this variable is ignored if the match is made with
    `gnus-group-name-charset-method-alist`.

Those two variables are used also to determine the charset for encoding
and decoding non-ASCII group names that are in the back ends
other than `nntp`. It means that it is you who determine it. If
you do nothing, the charset used for group names in those back ends will
all be `utf-8` because of the last element of
`gnus-group-name-charset-group-alist`.

There is one more important variable for non-ASCII group
names:

`nnmail-pathname-coding-system` [¶](#index-nnmail_002dpathname_002dcoding_002dsystem)
:   The value of this variable should be a coding system or `nil`. The
    default is `nil` in Emacs.

    The `nnml` back end, the `nnrss` back end, the agent, and
    the cache use non-ASCII group names in those files and
    directories. This variable overrides the value of
    `file-name-coding-system` which specifies the coding system used
    when encoding and decoding those file names and directory names.

    Emacs uses the value of `default-file-name-coding-system` if
    `file-name-coding-system` is `nil` or it is bound to the
    value of `nnmail-pathname-coding-system` which is `nil`.

    Normally the value of `default-file-name-coding-system` is
    initialized according to the locale, so you will need to do nothing if
    the value is suitable to encode and decode non-ASCII group
    names.

    The value of this variable (or `default-file-name-coding-system`)
    does not necessarily need to be the same value that is determined by
    `gnus-group-name-charset-method-alist` and
    `gnus-group-name-charset-group-alist`.

    If `default-file-name-coding-system` or this variable is
    initialized by default to `iso-latin-1-unix` for example, although you
    want to subscribe to the groups spelled in Chinese, that is the most
    typical case where you have to customize
    `nnmail-pathname-coding-system`. The `utf-8-unix` coding system is
    a good candidate for it. Otherwise, you may change the locale in your
    system so that `default-file-name-coding-system` or this variable
    may be initialized to an appropriate value.

Note that when you copy or move articles from a non-ASCII
group to another group, the charset used to encode and decode group
names should be the same in both groups. Otherwise the Newsgroups
header will be displayed incorrectly in the article buffer.

---

Previous: [Accessing groups of non-English names](#Non_002dASCII-Group-Names), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.18 Misc Group Stuff [¶](#Misc-Group-Stuff-1)

`v` [¶](#index-v-_0028Group_0029)
:   The key `v` is reserved for users. You can bind it to some
    command or better use it as a prefix key. For example:

    ```
    (keymap-set gnus-group-mode-map "v j d"
      (lambda ()
        (interactive)
        (gnus-group-jump-to-group "nndraft:drafts")))
    ```

    On keys reserved for users in Emacs and on key bindings in general
    See [Keymaps](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Keymaps) in The Emacs Editor.

`^` [¶](#index-_005e-_0028Group_0029)
:   Enter the server buffer (`gnus-group-enter-server-mode`).
    See [Server Buffer](#Server-Buffer).

`a` [¶](#index-a-_0028Group_0029)
:   Start composing a message (a news by default)
    (`gnus-group-post-news`). If given a prefix, post to the group
    under the point. If the prefix is 1, prompt for a group to post to.
    Contrary to what the name of this function suggests, the prepared
    article might be a mail instead of a news, if a mail group is specified
    with the prefix argument. See [Composing Messages](#Composing-Messages).

`m` [¶](#index-m-_0028Group_0029)
:   Mail a message somewhere (`gnus-group-mail`). If given a prefix,
    use the posting style of the group under the point. If the prefix is 1,
    prompt for a group name to find the posting style.
    See [Composing Messages](#Composing-Messages).

`i` [¶](#index-i-_0028Group_0029)
:   Start composing a news (`gnus-group-news`). If given a prefix,
    post to the group under the point. If the prefix is 1, prompt
    for group to post to. See [Composing Messages](#Composing-Messages).

    This function actually prepares a news even when using mail groups.
    This is useful for “posting” messages to mail groups without actually
    sending them over the network: they’re just saved directly to the group
    in question. The corresponding back end must have a request-post method
    for this to work though.

`G z` [¶](#index-G-z-_0028Group_0029)
:   Compact the group under point (`gnus-group-compact-group`).
    Currently implemented only in nnml (see [Mail Spool](#Mail-Spool)). This removes
    gaps between article numbers, hence getting a correct total article
    count.

Variables for the group buffer:

`gnus-group-mode-hook` [¶](#index-gnus_002dgroup_002dmode_002dhook)
:   is called after the group buffer has been
    created.

`gnus-group-prepare-hook` [¶](#index-gnus_002dgroup_002dprepare_002dhook)
:   is called after the group buffer is
    generated. It may be used to modify the buffer in some strange,
    unnatural way.

`gnus-group-prepared-hook` [¶](#index-gnus_002dgroup_002dprepared_002dhook)
:   is called as the very last thing after the group buffer has been
    generated. It may be used to move point around, for instance.

`gnus-permanently-visible-groups` [¶](#index-gnus_002dpermanently_002dvisible_002dgroups-1)
:   Groups matching this regexp will always be listed in the group buffer,
    whether they are empty or not.

* [Scanning New Messages](#Scanning-New-Messages)
* [Group Information](#Group-Information)
* [Group Timestamp](#Group-Timestamp)
* [File Commands](#File-Commands)
* [Sieve Commands](#Sieve-Commands)

---

Next: [Group Information](#Group-Information), Up: [Misc Group Stuff](#Misc-Group-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.18.1 Scanning New Messages [¶](#Scanning-New-Messages-1)

`g` [¶](#index-g-_0028Group_0029)
:   Check the server(s) for new articles. If the numerical prefix is used,
    this command will check only groups of level arg and lower
    (`gnus-group-get-new-news`). If given a non-numerical prefix, this
    command will force a total re-reading of the active file(s) from the
    back end(s).

`M-g` [¶](#index-M_002dg-_0028Group_0029)
:   Check whether new articles have arrived in the current group
    (`gnus-group-get-new-news-this-group`).
    `gnus-goto-next-group-when-activating` says whether this command is
    to move point to the next group or not. It is `t` by default.

`C-c M-g` [¶](#index-C_002dc-M_002dg-_0028Group_0029)
:   Activate absolutely all groups (`gnus-activate-all-groups`).

`R` [¶](#index-R-_0028Group_0029)
:   Restart Gnus (`gnus-group-restart`). This saves the `.newsrc`
    file(s), closes the connection to all servers, clears up all run-time
    Gnus variables, and then starts Gnus all over again.

`gnus-get-new-news-hook` is run just before checking for new news.

`gnus-after-getting-new-news-hook` is run after checking for new
news.

---

Next: [Group Timestamp](#Group-Timestamp), Previous: [Scanning New Messages](#Scanning-New-Messages), Up: [Misc Group Stuff](#Misc-Group-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.18.2 Group Information [¶](#Group-Information-1)

`H d`

`C-c C-d`
:   Describe the current group (`gnus-group-describe-group`). If given
    a prefix, force Gnus to re-read the description from the server.

`M-d` [¶](#index-M_002dd-_0028Group_0029)
:   Describe all groups (`gnus-group-describe-all-groups`). If given a
    prefix, force Gnus to re-read the description file from the server.

`H v` [¶](#index-V-_0028Group_0029)

`V`
:   Display current Gnus version numbers (`gnus-version`).

`?` [¶](#index-_003f-_0028Group_0029)
:   Give a very short help message (`gnus-group-describe-briefly`).

`C-c C-i` [¶](#index-C_002dc-C_002di-_0028Group_0029)
:   Go to the Gnus info node (`gnus-info-find-node`).

---

Next: [File Commands](#File-Commands), Previous: [Group Information](#Group-Information), Up: [Misc Group Stuff](#Misc-Group-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.18.3 Group Timestamp [¶](#Group-Timestamp-1)

It can be convenient to let Gnus keep track of when you last read a
group. To set the ball rolling, you should add
`gnus-group-set-timestamp` to `gnus-select-group-hook`:

```
(add-hook 'gnus-select-group-hook 'gnus-group-set-timestamp)
```

After doing this, each time you enter a group, it’ll be recorded.

This information can be displayed in various ways—the easiest is to
use the ‘`%d`’ spec in the group line format:

```
(setq gnus-group-line-format
      "%M\%S\%p\%P\%5y: %(%-40,40g%) %d\n")
```

This will result in lines looking like:

```
*        0: mail.ding                                19961002T012943
         0: custom                                   19961002T012713
```

As you can see, the date is displayed in compact ISO 8601 format. This
may be a bit too much, so to just display the date, you could say
something like:

```
(setq gnus-group-line-format
      "%M\%S\%p\%P\%5y: %(%-40,40g%) %6,6~(cut 2)d\n")
```

If you would like greater control of the time format, you can use a
user-defined format spec. Something like the following should do the
trick:

```
(setq gnus-group-line-format
      "%M\%S\%p\%P\%5y: %(%-40,40g%) %ud\n")
(defun gnus-user-format-function-d (headers)
  (let ((time (gnus-group-timestamp gnus-tmp-group)))
    (if time
        (format-time-string "%b %d  %H:%M" time)
      "")))
```

To see what variables are dynamically bound (like
`gnus-tmp-group`), you have to look at the source code. The
variable names aren’t guaranteed to be stable over Gnus versions,
either.

---

Next: [Sieve Commands](#Sieve-Commands), Previous: [Group Timestamp](#Group-Timestamp), Up: [Misc Group Stuff](#Misc-Group-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.18.4 File Commands [¶](#File-Commands-1)

`r` [¶](#index-r-_0028Group_0029)
:   Re-read the init file (`gnus-init-file`, which defaults to
    `~/.gnus.el`) (`gnus-group-read-init-file`).

`s` [¶](#index-s-_0028Group_0029)
:   Save the `.newsrc.eld` file (and `.newsrc` if wanted)
    (`gnus-group-save-newsrc`). If given a prefix, force saving the
    file(s) whether Gnus thinks it is necessary or not.

---

Previous: [File Commands](#File-Commands), Up: [Misc Group Stuff](#Misc-Group-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.18.5 Sieve Commands [¶](#Sieve-Commands-1)

Sieve is a server-side mail filtering language. In Gnus you can use
the `sieve` group parameter (see [Group Parameters](#Group-Parameters)) to specify
sieve rules that should apply to each group. Gnus provides two
commands to translate all these group parameters into a proper Sieve
script that can be transferred to the server somehow.

The generated Sieve script is placed in `gnus-sieve-file` (by
default `~/.sieve`). The Sieve code that Gnus generate is placed
between two delimiters, `gnus-sieve-region-start` and
`gnus-sieve-region-end`, so you may write additional Sieve code
outside these delimiters that will not be removed the next time you
regenerate the Sieve script.

The variable `gnus-sieve-crosspost` controls how the Sieve script
is generated. If it is non-`nil` (the default) articles is
placed in all groups that have matching rules, otherwise the article
is only placed in the group with the first matching rule. For
example, the group parameter ‘`(sieve address "sender"
"owner-ding@hpc.uh.edu")`’ will generate the following piece of Sieve
code if `gnus-sieve-crosspost` is `nil`. (When
`gnus-sieve-crosspost` is non-`nil`, it looks the same
except that the line containing the call to `stop` is removed.)

```
if address "sender" "owner-ding@hpc.uh.edu" {
        fileinto "INBOX.ding";
        stop;
}
```

See [Top](https://www.gnu.org/software/emacs/manual/html_mono/sieve.html#Top) in Emacs Sieve.

`D g` [¶](#index-D-g-_0028Group_0029)
:   Regenerate a Sieve script from the `sieve` group parameters and
    put you into the `gnus-sieve-file` without saving it.

`D u` [¶](#index-D-u-_0028Group_0029)
:   Regenerates the Gnus managed part of `gnus-sieve-file` using the
    `sieve` group parameters, save the file and upload it to the
    server using the `sieveshell` program.

---

Next: [Article Buffer](#Article-Buffer), Previous: [Group Buffer](#Group-Buffer), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 4 Summary Buffer [¶](#Summary-Buffer-1)

A line for each article is displayed in the summary buffer. You can
move around, read articles, post articles and reply to articles.

The most common way to a summary buffer is to select a group from the
group buffer (see [Selecting a Group](#Selecting-a-Group)).

You can have as many summary buffers open as you wish.

You can customize the Summary Mode tool bar, see `M-x
customize-apropos RET gnus-summary-tool-bar`.

The key `v` is reserved for users. You can bind it to some
command or better use it as a prefix key. For example:

```
(keymap-set gnus-summary-mode-map "v -" "LrS") ;; lower subthread
```

* [Summary Buffer Format](#Summary-Buffer-Format)
* [Summary Maneuvering](#Summary-Maneuvering)
* [Choosing Articles](#Choosing-Articles)
* [Scrolling the Article](#Paging-the-Article)
* [Reply, Followup and Post](#Reply-Followup-and-Post)
* [Delayed Articles](#Delayed-Articles)
* [Marking Articles](#Marking-Articles)
* [Limiting](#Limiting)
* [Threading](#Threading)
* [Sorting the Summary Buffer](#Sorting-the-Summary-Buffer)
* [Asynchronous Article Fetching](#Asynchronous-Fetching)
* [Article Caching](#Article-Caching)
* [Persistent Articles](#Persistent-Articles)
* [Sticky Articles](#Sticky-Articles)
* [Article Backlog](#Article-Backlog)
* [Saving Articles](#Saving-Articles)
* [Decoding Articles](#Decoding-Articles)
* [Article Treatment](#Article-Treatment)
* [MIME Commands](#MIME-Commands)
* [Charsets](#Charsets)
* [Article Commands](#Article-Commands)
* [Summary Sorting](#Summary-Sorting)
* [Finding the Parent](#Finding-the-Parent)
* [Alternative Approaches](#Alternative-Approaches)
* [Tree Display](#Tree-Display)
* [Mail Group Commands](#Mail-Group-Commands)
* [Various Summary Stuff](#Various-Summary-Stuff)
* [Exiting the Summary Buffer](#Exiting-the-Summary-Buffer)
* [Crosspost Handling](#Crosspost-Handling)
* [Duplicate Suppression](#Duplicate-Suppression)
* [Security](#Security)
* [Mailing List](#Mailing-List)

---

Next: [Summary Maneuvering](#Summary-Maneuvering), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
