### 4.27 Various Summary Stuff [¶](#Various-Summary-Stuff-1)

`gnus-summary-display-while-building` [¶](#index-gnus_002dsummary_002ddisplay_002dwhile_002dbuilding)
:   If non-`nil`, show and update the summary buffer as it’s being
    built. If `t`, update the buffer after every line is inserted.
    If the value is an integer, n, update the display every n
    lines. The default is `nil`.

`gnus-summary-display-arrow` [¶](#index-gnus_002dsummary_002ddisplay_002darrow)
:   If non-`nil`, display an arrow in the fringe to indicate the
    current article.

`gnus-summary-mode-hook` [¶](#index-gnus_002dsummary_002dmode_002dhook)
:   This hook is called when creating a summary mode buffer.

`gnus-summary-generate-hook` [¶](#index-gnus_002dsummary_002dgenerate_002dhook)
:   This is called as the last thing before doing the threading and the
    generation of the summary buffer. It’s quite convenient for customizing
    the threading variables based on what data the newsgroup has. This hook
    is called from the summary buffer after most summary buffer variables
    have been set.

`gnus-summary-prepare-hook` [¶](#index-gnus_002dsummary_002dprepare_002dhook)
:   It is called after the summary buffer has been generated. You might use
    it to, for instance, highlight lines or modify the look of the buffer in
    some other ungodly manner. I don’t care.

`gnus-summary-prepared-hook` [¶](#index-gnus_002dsummary_002dprepared_002dhook)
:   A hook called as the very last thing after the summary buffer has been
    generated.

`gnus-summary-ignore-duplicates` [¶](#index-gnus_002dsummary_002dignore_002dduplicates)
:   When Gnus discovers two articles that have the same `Message-ID`,
    it has to do something drastic. No articles are allowed to have the
    same `Message-ID`, but this may happen when reading mail from some
    sources. Gnus allows you to customize what happens with this variable.
    If it is `nil` (which is the default), Gnus will rename the
    `Message-ID` (for display purposes only) and display the article as
    any other article. If this variable is `t`, it won’t display the
    article—it’ll be as if it never existed.

`gnus-alter-articles-to-read-function` [¶](#index-gnus_002dalter_002darticles_002dto_002dread_002dfunction)
:   This function, which takes two parameters (the group name and the list
    of articles to be selected), is called to allow the user to alter the
    list of articles to be selected.

    For instance, the following function adds the list of cached articles to
    the list in one particular group:

    ```
    (defun my-add-cached-articles (group articles)
      (if (string= group "some.group")
          (append gnus-newsgroup-cached articles)
        articles))
    ```

`gnus-newsgroup-variables` [¶](#index-gnus_002dnewsgroup_002dvariables)
:   A list of newsgroup (summary buffer) local variables, or cons of
    variables and their default expressions to be evalled (when the default
    values are not `nil`), that should be made global while the summary
    buffer is active.

    Note: The default expressions will be evaluated (using function
    `eval`) before assignment to the local variable rather than just
    assigned to it. If the default expression is the symbol `global`,
    that symbol will not be evaluated but the global value of the local
    variable will be used instead.

    These variables can be used to set variables in the group parameters
    while still allowing them to affect operations done in other
    buffers. For example:

    ```
    (setq gnus-newsgroup-variables
          '(message-use-followup-to
            (gnus-visible-headers .
     "^From:\\|^Newsgroups:\\|^Subject:\\|^Date:\\|^To:")))
    ```

    Also see [Group Parameters](#Group-Parameters).

* [Summary Group Information](#Summary-Group-Information)
* [Searching for Articles](#Searching-for-Articles)
* [Summary Generation Commands](#Summary-Generation-Commands)
* [Really Various Summary Commands](#Really-Various-Summary-Commands)

---

Next: [Searching for Articles](#Searching-for-Articles), Up: [Various Summary Stuff](#Various-Summary-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.27.1 Summary Group Information [¶](#Summary-Group-Information-1)

`H d` [¶](#index-H-d-_0028Summary_0029)
:   Give a brief description of the current group
    (`gnus-summary-describe-group`). If given a prefix, force
    rereading the description from the server.

`H h` [¶](#index-H-h-_0028Summary_0029)
:   Give an extremely brief description of the most important summary
    keystrokes (`gnus-summary-describe-briefly`).

`H i` [¶](#index-H-i-_0028Summary_0029)
:   Go to the Gnus info node (`gnus-info-find-node`).

---

Next: [Summary Generation Commands](#Summary-Generation-Commands), Previous: [Summary Group Information](#Summary-Group-Information), Up: [Various Summary Stuff](#Various-Summary-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.27.2 Searching for Articles [¶](#Searching-for-Articles-1)

`M-s M-s` [¶](#index-M_002ds-M_002ds-_0028Summary_0029)
:   Search through all subsequent (raw) articles for a regexp
    (`gnus-summary-search-article-forward`).

`M-s M-r` [¶](#index-M_002ds-M_002dr-_0028Summary_0029)
:   Search through all previous (raw) articles for a regexp
    (`gnus-summary-search-article-backward`).

`M-S` [¶](#index-M_002dS-_0028Summary_0029)
:   Repeat the previous search forwards
    (`gnus-summary-repeat-search-article-forward`).

`M-R` [¶](#index-M_002dR-_0028Summary_0029)
:   Repeat the previous search backwards
    (`gnus-summary-repeat-search-article-backward`).

`&` [¶](#index-_0026-_0028Summary_0029)
:   This command will prompt you for a header, a regular expression to match
    on this field, and a command to be executed if the match is made
    (`gnus-summary-execute-command`). If the header is an empty
    string, the match is done on the entire article. If given a prefix,
    search backward instead.

    For instance, `& RET some.*string RET #` will put the
    process mark on all articles that have heads or bodies that match
    ‘`some.*string`’.

`M-&` [¶](#index-M_002d_0026-_0028Summary_0029)
:   Perform any operation on all articles that have been marked with
    the process mark (`gnus-summary-universal-argument`).

---

Next: [Really Various Summary Commands](#Really-Various-Summary-Commands), Previous: [Searching for Articles](#Searching-for-Articles), Up: [Various Summary Stuff](#Various-Summary-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.27.3 Summary Generation Commands [¶](#Summary-Generation-Commands-1)

`Y g` [¶](#index-Y-g-_0028Summary_0029)
:   Regenerate the current summary buffer (`gnus-summary-prepare`).

`Y c` [¶](#index-Y-c-_0028Summary_0029)
:   Pull all cached articles (for the current group) into the summary buffer
    (`gnus-summary-insert-cached-articles`).

`Y d` [¶](#index-Y-d-_0028Summary_0029)
:   Pull all dormant articles (for the current group) into the summary buffer
    (`gnus-summary-insert-dormant-articles`).

`Y t` [¶](#index-Y-t-_0028Summary_0029)
:   Pull all ticked articles (for the current group) into the summary buffer
    (`gnus-summary-insert-ticked-articles`).

---

Previous: [Summary Generation Commands](#Summary-Generation-Commands), Up: [Various Summary Stuff](#Various-Summary-Stuff)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.27.4 Really Various Summary Commands [¶](#Really-Various-Summary-Commands-1)

`A D` [¶](#index-C_002dd-_0028Summary_0029)

`C-d`
:   If the current article is a collection of other articles (for instance,
    a digest), you might use this command to enter a group based on that
    article (`gnus-summary-enter-digest-group`). Gnus will try to
    guess what article type is currently displayed unless you give a prefix
    to this command, which forces a “digest” interpretation. Basically,
    whenever you see a message that is a collection of other messages of
    some format, you `C-d` and read these messages in a more convenient
    fashion.

    The variable `gnus-auto-select-on-ephemeral-exit` controls what
    article should be selected after exiting a digest group. Valid values
    include:

    `next`
    :   Select the next article.

    `next-unread`
    :   Select the next unread article.

    `next-noselect`
    :   Move the cursor to the next article. This is the default.

    `next-unread-noselect`
    :   Move the cursor to the next unread article.

    If it has any other value or there is no next (unread) article, the
    article selected before entering to the digest group will appear.

`C-M-d` [¶](#index-C_002dM_002dd-_0028Summary_0029)
:   This command is very similar to the one above, but lets you gather
    several documents into one biiig group
    (`gnus-summary-read-document`). It does this by opening several
    `nndoc` groups for each document, and then opening an
    `nnvirtual` group on top of these `nndoc` groups. This
    command understands the process/prefix convention
    (see [Process/Prefix](#Process_002fPrefix)).

`C-t` [¶](#index-C_002dt-_0028Summary_0029)
:   Toggle truncation of summary lines
    (`gnus-summary-toggle-truncation`). This will probably confuse the
    line centering function in the summary buffer, so it’s not a good idea
    to have truncation switched off while reading articles.

`=` [¶](#index-_003d-_0028Summary_0029)
:   Expand the summary buffer window (`gnus-summary-expand-window`).
    If given a prefix, force an `article` window configuration.

`C-M-e` [¶](#index-C_002dM_002de-_0028Summary_0029)
:   Edit the group parameters (see [Group Parameters](#Group-Parameters)) of the current
    group (`gnus-summary-edit-parameters`).

`C-M-a` [¶](#index-C_002dM_002da-_0028Summary_0029)
:   Customize the group parameters (see [Group Parameters](#Group-Parameters)) of the current
    group (`gnus-summary-customize-parameters`).

---

Next: [Crosspost Handling](#Crosspost-Handling), Previous: [Various Summary Stuff](#Various-Summary-Stuff), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.28 Exiting the Summary Buffer [¶](#Exiting-the-Summary-Buffer-1)

Exiting from the summary buffer will normally update all info on the
group and return you to the group buffer.

`Z Z` [¶](#index-Z-Z-_0028Summary_0029)

`Z Q`

`q`
:   Exit the current group and update all information on the group
    (`gnus-summary-exit`). `gnus-summary-prepare-exit-hook` is
    called before doing much of the exiting, which calls
    `gnus-summary-expire-articles` by default.
    `gnus-summary-exit-hook` is called after finishing the exit
    process. `gnus-group-no-more-groups-hook` is run when returning to
    group mode having no more (unread) groups.

`Z E` [¶](#index-Z-E-_0028Summary_0029)

`Q`
:   Exit the current group without updating any information on the group
    (`gnus-summary-exit-no-update`).

`Z c` [¶](#index-Z-c-_0028Summary_0029)

`c`
:   Mark all unticked articles in the group as read and then exit
    (`gnus-summary-catchup-and-exit`).

`Z C` [¶](#index-Z-C-_0028Summary_0029)
:   Mark all articles, even the ticked ones, as read and then exit
    (`gnus-summary-catchup-all-and-exit`).

`Z n` [¶](#index-Z-n-_0028Summary_0029)
:   Mark all articles as read and go to the next group
    (`gnus-summary-catchup-and-goto-next-group`).

`Z p` [¶](#index-Z-p-_0028Summary_0029)
:   Mark all articles as read and go to the previous group
    (`gnus-summary-catchup-and-goto-prev-group`).

`Z R` [¶](#index-Z-R-_0028Summary_0029)

`C-x C-s`
:   Exit this group, and then enter it again
    (`gnus-summary-reselect-current-group`). If given a prefix, select
    all articles, both read and unread.

`Z G` [¶](#index-Z-G-_0028Summary_0029)

`M-g`
:   Exit the group, check for new articles in the group, and select the
    group (`gnus-summary-rescan-group`). If given a prefix, select all
    articles, both read and unread.

`Z N` [¶](#index-Z-N-_0028Summary_0029)
:   Exit the group and go to the next group
    (`gnus-summary-next-group`).

`Z P` [¶](#index-Z-P-_0028Summary_0029)
:   Exit the group and go to the previous group
    (`gnus-summary-prev-group`).

`Z s` [¶](#index-Z-s-_0028Summary_0029)
:   Save the current number of read/marked articles in the dribble buffer
    and then save the dribble buffer (`gnus-summary-save-newsrc`). If
    given a prefix, also save the `.newsrc` file(s). Using this
    command will make exit without updating (the `Q` command) worthless.

`gnus-exit-group-hook` is called when you exit the current group
with an “updating” exit. For instance `Q`
(`gnus-summary-exit-no-update`) does not call this hook.

If you’re in the habit of exiting groups, and then changing your mind
about it, you might set `gnus-kill-summary-on-exit` to `nil`.
If you do that, Gnus won’t kill the summary buffer when you exit it.
(Quelle surprise!) Instead it will change the name of the buffer to
something like `*Dead Summary ... *` and install a minor mode
called `gnus-dead-summary-mode`. Now, if you switch back to this
buffer, you’ll find that all keys are mapped to a function called
`gnus-summary-wake-up-the-dead`. So tapping any keys in a dead
summary buffer will result in a live, normal summary buffer.

There will never be more than one dead summary buffer at any one time.

The data on the current group will be updated (which articles you have
read, which articles you have replied to, etc.) when you exit the
summary buffer. If the `gnus-use-cross-reference` variable is
`t` (which is the default), articles that are cross-referenced to
this group and are marked as read, will also be marked as read in the
other subscribed groups they were cross-posted to. If this variable is
neither `nil` nor `t`, the article will be marked as read in
both subscribed and unsubscribed groups (see [Crosspost Handling](#Crosspost-Handling)).

---

Next: [Duplicate Suppression](#Duplicate-Suppression), Previous: [Exiting the Summary Buffer](#Exiting-the-Summary-Buffer), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.29 Crosspost Handling [¶](#Crosspost-Handling-1)

Marking cross-posted articles as read ensures that you’ll never have to
read the same article more than once. Unless, of course, somebody has
posted it to several groups separately. Posting the same article to
several groups (not cross-posting) is called *spamming*, and you are
by law required to send nasty-grams to anyone who perpetrates such a
heinous crime.

Remember: Cross-posting is kinda ok, but posting the same article
separately to several groups is not. Massive cross-posting (aka.
*velveeta*) is to be avoided at all costs, and you can even use the
`gnus-summary-mail-crosspost-complaint` command to complain about
excessive crossposting (see [Summary Mail Commands](#Summary-Mail-Commands)).

One thing that may cause Gnus to not do the cross-posting thing
correctly is if you use an NNTP server that supports XOVER
(which is very nice, because it speeds things up considerably) which
does not include the `Xref` header in its NOV lines. This is
Evil, but all too common, alas, alack. Gnus tries to Do The Right Thing
even with XOVER by registering the `Xref` lines of all
articles you actually read, but if you kill the articles, or just mark
them as read without reading them, Gnus will not get a chance to snoop
the `Xref` lines out of these articles, and will be unable to use
the cross-reference mechanism.

To check whether your NNTP server includes the `Xref` header
in its overview files, try ‘`telnet your.nntp.server nntp`’,
‘`MODE READER`’ on `inn` servers, and then say ‘`LIST
overview.fmt`’. This may not work, but if it does, and the last line you
get does not read ‘`Xref:full`’, then you should shout and whine at
your news admin until she includes the `Xref` header in the
overview files.

If you want Gnus to get the `Xref`s right all the time, you have to
set `nntp-nov-is-evil` to `t`, which slows things down
considerably. Also see [Slow/Expensive Connection](#Slow_002fExpensive-Connection).

C’est la vie.

For an alternative approach, see [Duplicate Suppression](#Duplicate-Suppression).

---

Next: [Security](#Security), Previous: [Crosspost Handling](#Crosspost-Handling), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.30 Duplicate Suppression [¶](#Duplicate-Suppression-1)

By default, Gnus tries to make sure that you don’t have to read the same
article more than once by utilizing the crossposting mechanism
(see [Crosspost Handling](#Crosspost-Handling)). However, that simple and efficient
approach may not work satisfactory for some users for various
reasons.

1. The NNTP server may fail to generate the `Xref` header. This
   is evil and not very common.
2. The NNTP server may fail to include the `Xref` header in the
   `.overview` data bases. This is evil and all too common, alas.
3. You may be reading the same group (or several related groups) from
   different NNTP servers.
4. You may be getting mail that duplicates articles posted to groups.

I’m sure there are other situations where `Xref` handling fails as
well, but these four are the most common situations.

If, and only if, `Xref` handling fails for you, then you may
consider switching on *duplicate suppression*. If you do so, Gnus
will remember the `Message-ID`s of all articles you have read or
otherwise marked as read, and then, as if by magic, mark them as read
all subsequent times you see them—in *all* groups. Using this
mechanism is quite likely to be somewhat inefficient, but not overly
so. It’s certainly preferable to reading the same articles more than
once.

Duplicate suppression is not a very subtle instrument. It’s more like a
sledge hammer than anything else. It works in a very simple
fashion—if you have marked an article as read, it adds this Message-ID
to a cache. The next time it sees this Message-ID, it will mark the
article as read with the ‘`M`’ mark. It doesn’t care what group it
saw the article in.

`gnus-suppress-duplicates` [¶](#index-gnus_002dsuppress_002dduplicates)
:   If non-`nil`, suppress duplicates.

`gnus-save-duplicate-list` [¶](#index-gnus_002dsave_002dduplicate_002dlist)
:   If non-`nil`, save the list of duplicates to a file. This will
    make startup and shutdown take longer, so the default is `nil`.
    However, this means that only duplicate articles read in a single Gnus
    session are suppressed.

`gnus-duplicate-list-length` [¶](#index-gnus_002dduplicate_002dlist_002dlength)
:   This variable says how many `Message-ID`s to keep in the duplicate
    suppression list. The default is 10000.

`gnus-duplicate-file` [¶](#index-gnus_002dduplicate_002dfile)
:   The name of the file to store the duplicate suppression list in. The
    default is `~/News/suppression`.

If you have a tendency to stop and start Gnus often, setting
`gnus-save-duplicate-list` to `t` is probably a good idea. If
you leave Gnus running for weeks on end, you may have it `nil`. On
the other hand, saving the list makes startup and shutdown much slower,
so that means that if you stop and start Gnus often, you should set
`gnus-save-duplicate-list` to `nil`. Uhm. I’ll leave this up
to you to figure out, I think.

---

Next: [Mailing List](#Mailing-List), Previous: [Duplicate Suppression](#Duplicate-Suppression), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.31 Security [¶](#Security-1)

Gnus is able to verify signed messages or decrypt encrypted messages.
The formats that are supported are PGP, PGP/MIME
and S/MIME, however you need some external programs to get
things to work:

1. To handle PGP and PGP/MIME messages, you have to
   install an OpenPGP implementation such as GnuPG. The Lisp interface
   to GnuPG included with Emacs is called EasyPG (see [EasyPG](https://www.gnu.org/software/emacs/manual/html_mono/epa.html#Top) in EasyPG Assistant user’s manual), but Mailcrypt is also
   supported.
2. To handle S/MIME message, you need to install OpenSSL. OpenSSL 0.9.6
   or newer is recommended.

The variables that control security functionality on reading/composing
messages include:

`mm-verify-option` [¶](#index-mm_002dverify_002doption)
:   Option of verifying signed parts. `never`, not verify;
    `always`, always verify; `known`, only verify known
    protocols. Otherwise, ask user.

`mm-decrypt-option` [¶](#index-mm_002ddecrypt_002doption)
:   Option of decrypting encrypted parts. `never`, no decryption;
    `always`, always decrypt; `known`, only decrypt known
    protocols. Otherwise, ask user.

`mm-sign-option` [¶](#index-mm_002dsign_002doption)
:   Option of creating signed parts. `nil`, use default signing
    keys; `guided`, ask user to select signing keys from the menu.

`mm-encrypt-option` [¶](#index-mm_002dencrypt_002doption)
:   Option of creating encrypted parts. `nil`, use the first
    public-key matching the ‘`From:`’ header as the recipient;
    `guided`, ask user to select recipient keys from the menu.

`mml1991-use` [¶](#index-mml1991_002duse)
:   Symbol indicating elisp interface to OpenPGP implementation for
    PGP messages. The default is `epg`, but
    `mailcrypt` is also supported although deprecated. By default,
    Gnus uses the first available interface in this order.

`mml2015-use` [¶](#index-mml2015_002duse)
:   Symbol indicating elisp interface to OpenPGP implementation for
    PGP/MIME messages. The default is `epg`, but
    `mailcrypt` is also supported although deprecated. By default,
    Gnus uses the first available interface in this order.

By default the buttons that display security information are not
shown, because they clutter reading the actual e-mail. You can type
`K b` manually to display the information. Use the
`gnus-buttonized-mime-types` and
`gnus-unbuttonized-mime-types` variables to control this
permanently. [MIME Commands](#MIME-Commands) for further details, and hints on
how to customize these variables to always display security
information.

Snarfing OpenPGP keys (i.e., importing keys from articles into your
key ring) is not supported explicitly through a menu item or command,
rather Gnus do detect and label keys as ‘`application/pgp-keys`’,
allowing you to specify whatever action you think is appropriate
through the usual MIME infrastructure. You can use a
`~/.mailcap` entry (see [mailcap](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#mailcap) in The
Emacs MIME Manual) such as the following to import keys using GNU
Privacy Guard when you click on the MIME button
(see [Using MIME](#Using-MIME)).

```
application/pgp-keys; gpg --import --interactive --verbose; needsterminal
```

This happens to also be the default action defined in
`mailcap-mime-data`.

More information on how to set things for sending outgoing signed and
encrypted messages up can be found in the message manual
(see [Security](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Security) in Message Manual).

---

Previous: [Security](#Security), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.32 Mailing List [¶](#Mailing-List-1)

Gnus understands some mailing list fields of RFC 2369. To enable it,
add a `to-list` group parameter (see [Group Parameters](#Group-Parameters)),
possibly using `A M` (`gnus-mailing-list-insinuate`) in the
summary buffer.

That enables the following commands to the summary buffer:

`C-c C-n h` [¶](#index-C_002dc-C_002dn-h-_0028Summary_0029)
:   Send a message to fetch mailing list help, if List-Help field exists.

`C-c C-n s` [¶](#index-C_002dc-C_002dn-s-_0028Summary_0029)
:   Send a message to subscribe the mailing list, if List-Subscribe field exists.

`C-c C-n u` [¶](#index-C_002dc-C_002dn-u-_0028Summary_0029)
:   Send a message to unsubscribe the mailing list, if List-Unsubscribe
    field exists.

`C-c C-n p` [¶](#index-C_002dc-C_002dn-p-_0028Summary_0029)
:   Post to the mailing list, if List-Post field exists.

`C-c C-n o` [¶](#index-C_002dc-C_002dn-o-_0028Summary_0029)
:   Send a message to the mailing list owner, if List-Owner field exists.

`C-c C-n a` [¶](#index-C_002dc-C_002dn-a-_0028Summary_0029)
:   Browse the mailing list archive, if List-Archive field exists.

---

Next: [Composing Messages](#Composing-Messages), Previous: [Summary Buffer](#Summary-Buffer), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 5 Article Buffer [¶](#Article-Buffer-1)

The articles are displayed in the article buffer, of which there is only
one. All the summary buffers share the same article buffer unless you
tell Gnus otherwise.

* [Hiding Headers](#Hiding-Headers)
* [Using MIME](#Using-MIME)
* [HTML](#HTML)
* [Customizing Articles](#Customizing-Articles)
* [Article Keymap](#Article-Keymap)
* [Misc Article](#Misc-Article)

---

Next: [Using MIME](#Using-MIME), Up: [Article Buffer](#Article-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.1 Hiding Headers [¶](#Hiding-Headers-1)

The top section of each article is the *head*. (The rest is the
*body*, but you may have guessed that already.)

There is a lot of useful information in the head: the name of the person
who wrote the article, the date it was written and the subject of the
article. That’s well and nice, but there’s also lots of information
most people do not want to see—what systems the article has passed
through before reaching you, the `Message-ID`, the
`References`, etc. ad nauseam—and you’ll probably want to get rid
of some of those lines. If you want to keep all those lines in the
article buffer, you can set `gnus-show-all-headers` to `t`.

Gnus provides you with two variables for sifting headers:

`gnus-visible-headers` [¶](#index-gnus_002dvisible_002dheaders)
:   If this variable is non-`nil`, it should be a regular expression
    that says what headers you wish to keep in the article buffer. All
    headers that do not match this variable will be hidden.

    For instance, if you only want to see the name of the person who wrote
    the article and the subject, you’d say:

    ```
    (setq gnus-visible-headers "^From:\\|^Subject:")
    ```

    This variable can also be a list of regexps to match headers to
    remain visible.

`gnus-ignored-headers` [¶](#index-gnus_002dignored_002dheaders)
:   This variable is the reverse of `gnus-visible-headers`. If this
    variable is set (and `gnus-visible-headers` is `nil`), it
    should be a regular expression that matches all lines that you want to
    hide. All lines that do not match this variable will remain visible.

    For instance, if you just want to get rid of the `References` line
    and the `Xref` line, you might say:

    ```
    (setq gnus-ignored-headers "^References:\\|^Xref:")
    ```

    This variable can also be a list of regexps to match headers to
    be removed.

    Note that if `gnus-visible-headers` is non-`nil`, this
    variable will have no effect.

Gnus can also sort the headers for you. (It does this by default.) You
can control the sorting by setting the `gnus-sorted-header-list`
variable. It is a list of regular expressions that says in what order
the headers are to be displayed.

For instance, if you want the name of the author of the article first,
and then the subject, you might say something like:

```
(setq gnus-sorted-header-list '("^From:" "^Subject:"))
```

Any headers that are to remain visible, but are not listed in this
variable, will be displayed in random order after all the headers listed in this variable.

You can hide further boring headers by setting
`gnus-treat-hide-boring-headers` to `head`. What this function
does depends on the `gnus-boring-article-headers` variable. It’s a
list, but this list doesn’t actually contain header names. Instead it
lists various *boring conditions* that Gnus can check and remove
from sight.

These conditions are:

`empty`
:   Remove all empty headers.

`followup-to`
:   Remove the `Followup-To` header if it is identical to the
    `Newsgroups` header.

`reply-to`
:   Remove the `Reply-To` header if it lists the same addresses as
    the `From` header, or if the `broken-reply-to` group
    parameter is set.

`newsgroups`
:   Remove the `Newsgroups` header if it only contains the current group
    name.

`to-address`
:   Remove the `To` header if it only contains the address identical to
    the current group’s `to-address` parameter.

`to-list`
:   Remove the `To` header if it only contains the address identical to
    the current group’s `to-list` parameter.

`cc-list`
:   Remove the `Cc` header if it only contains the address identical to
    the current group’s `to-list` parameter.

`date`
:   Remove the `Date` header if the article is less than three days
    old.

`long-to`
:   Remove the `To` and/or `Cc` header if it is very long.

`many-to`
:   Remove all `To` and/or `Cc` headers if there are more than one.

To include these three elements, you could say something like:

```
(setq gnus-boring-article-headers
      '(empty followup-to reply-to))
```

This is also the default value for this variable.

---

Next: [HTML](#HTML), Previous: [Hiding Headers](#Hiding-Headers), Up: [Article Buffer](#Article-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.2 Using MIME [¶](#Using-MIME-1)

Mime is a standard for waving your hands through the air, aimlessly,
while people stand around yawning.

MIME, however, is a standard for encoding your articles, aimlessly,
while all newsreaders die of fear.

MIME may specify what character set the article uses, the encoding
of the characters, and it also makes it possible to embed pictures and
other naughty stuff in innocent-looking articles.

Gnus pushes MIME articles through `gnus-display-mime-function`
to display the MIME parts. This is `gnus-display-mime` by
default, which creates a bundle of clickable buttons that can be used to
display, save and manipulate the MIME objects.

The following commands are available when you have placed point over a
MIME button:

`RET (Article)` [¶](#index-gnus_002darticle_002dpress_002dbutton)

`BUTTON-2 (Article)`
:   Toggle displaying of the MIME object
    (`gnus-article-press-button`). If built-in viewers can not display
    the object, Gnus resorts to external viewers in the `mailcap`
    files. If a viewer has the ‘`copiousoutput`’ specification, the
    object is displayed inline.

`M-RET (Article)` [¶](#index-gnus_002dmime_002dview_002dpart)

`v (Article)`
:   Prompt for a method, and then view the MIME object using this
    method (`gnus-mime-view-part`).

`t (Article)` [¶](#index-t-_0028Article_0029)
:   View the MIME object as if it were a different MIME media type
    (`gnus-mime-view-part-as-type`).

`C (Article)` [¶](#index-C-_0028Article_0029)
:   Prompt for a charset, and then view the MIME object using this
    charset (`gnus-mime-view-part-as-charset`).

`o (Article)` [¶](#index-o-_0028Article_0029)
:   Prompt for a file name, and then save the MIME object
    (`gnus-mime-save-part`).

`C-o (Article)` [¶](#index-C_002do-_0028Article_0029)
:   Prompt for a file name, then save the MIME object and strip it from
    the article. Then proceed to article editing, where a reasonable
    suggestion is being made on how the altered article should look
    like. The stripped MIME object will be referred via the
    message/external-body MIME type.
    (`gnus-mime-save-part-and-strip`).

`r (Article)` [¶](#index-r-_0028Article_0029)
:   Prompt for a file name, replace the MIME object with an
    external body referring to the file via the message/external-body
    MIME type. (`gnus-mime-replace-part`).

`d (Article)` [¶](#index-d-_0028Article_0029)
:   Delete the MIME object from the article and replace it with some
    information about the removed MIME object
    (`gnus-mime-delete-part`).

`c (Article)` [¶](#index-c-_0028Article_0029)
:   Copy the MIME object to a fresh buffer and display this buffer
    (`gnus-mime-copy-part`). If given a prefix, copy the raw contents
    without decoding. If given a numerical prefix, you can do semi-manual
    charset stuff (see `gnus-summary-show-article-charset-alist` in
    [Scrolling the Article](#Paging-the-Article)). Compressed files like `.gz` and
    `.bz2` are automatically decompressed if
    `auto-compression-mode` is enabled (see [Accessing Compressed Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Compressed-Files) in The Emacs Editor).

`p (Article)` [¶](#index-p-_0028Article_0029)
:   Print the MIME object (`gnus-mime-print-part`). This
    command respects the ‘`print=`’ specifications in the
    `.mailcap` file.

`i (Article)` [¶](#index-i-_0028Article_0029)
:   Insert the contents of the MIME object into the buffer
    (`gnus-mime-inline-part`) as ‘`text/plain`’. If given a prefix, insert
    the raw contents without decoding. If given a numerical prefix, you can
    do semi-manual charset stuff (see
    `gnus-summary-show-article-charset-alist` in [Scrolling the Article](#Paging-the-Article)). Compressed files like `.gz` and `.bz2` are
    automatically decompressed depending on `jka-compr` regardless of
    `auto-compression-mode` (see [Accessing
    Compressed Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Compressed-Files) in The Emacs Editor).

`E (Article)` [¶](#index-E-_0028Article_0029)
:   View the MIME object with an internal viewer. If no internal
    viewer is available, use an external viewer
    (`gnus-mime-view-part-internally`).

`e (Article)` [¶](#index-e-_0028Article_0029)
:   View the MIME object with an external viewer.
    (`gnus-mime-view-part-externally`).

`| (Article)` [¶](#index-_007c-_0028Article_0029)
:   Output the MIME object to a process (`gnus-mime-pipe-part`).

`. (Article)` [¶](#index-_002e-_0028Article_0029)
:   Interactively run an action on the MIME object
    (`gnus-mime-action-on-part`).

Gnus will display some MIME objects automatically. The way Gnus
determines which parts to do this with is described in the Emacs
MIME manual.

It might be best to just use the toggling functions from the article
buffer to avoid getting nasty surprises. (For instance, you enter the
group ‘`alt.sing-a-long`’ and, before you know it, MIME has
decoded the sound file in the article and some horrible sing-a-long song
comes screaming out your speakers, and you can’t find the volume button,
because there isn’t one, and people are starting to look at you, and you
try to stop the program, but you can’t, and you can’t find the program
to control the volume, and everybody else in the room suddenly decides
to look at you disdainfully, and you’ll feel rather stupid.)

Any similarity to real events and people is purely coincidental. Ahem.

Also see [MIME Commands](#MIME-Commands).

---

Next: [Customizing Articles](#Customizing-Articles), Previous: [Using MIME](#Using-MIME), Up: [Article Buffer](#Article-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.3 HTML [¶](#HTML-1)

Gnus can display HTML articles nicely formatted in the
article buffer. There are many methods for doing that, but two of
them are kind of default methods.

If your Emacs copy has been built with libxml2 support, then Gnus uses
Emacs’s built-in, plain elisp Simple HTML Renderer `shr`
[2](#FOOT2) which is also used by Emacs’
browser EWW (see [EWW](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#EWW) in The Emacs Manual).

If your Emacs copy lacks libxml2 support but you have `w3m`
installed on your system, Gnus uses that to render HTML mail
and display the results in the article buffer (`gnus-w3m`).

For a complete overview, consult See [Display
Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Display-Customization) in The Emacs MIME Manual. This section only
describes the default method.

`mm-text-html-renderer` [¶](#index-mm_002dtext_002dhtml_002drenderer)
:   If set to `shr`, Gnus uses its own simple HTML
    renderer. If set to `gnus-w3m`, it uses `w3m`.

`gnus-blocked-images` [¶](#index-gnus_002dblocked_002dimages)
:   External images that have URLs that match this regexp won’t
    be fetched and displayed. For instance, to block all URLs
    that have the string “ads” in them, do the following:

    ```
    (setq gnus-blocked-images "ads")
    ```

    This can also be a function to be evaluated. If so, it will be
    called with the group name as the parameter. The default value is
    `gnus-block-private-groups`, which will return ‘`"."`’ for
    anything that isn’t a newsgroup. This means that no external images
    will be fetched as a result of reading mail, so that nobody can use
    web bugs (and the like) to track whether you’ve read email.

    If you have specific private groups that you want to have treated as
    if they were public groups, you can add the name of that group to the
    `gnus-global-groups` list.

    Also see [Misc Article](#Misc-Article) for `gnus-inhibit-images`.

`gnus-html-frame-width` [¶](#index-gnus_002dhtml_002dframe_002dwidth)
:   The width to use when rendering HTML. The default is 70.

`gnus-max-image-proportion` [¶](#index-gnus_002dmax_002dimage_002dproportion)
:   How big pictures displayed are in relation to the window they’re in.
    A value of 0.7 (the default) means that they are allowed to take up
    70% of the width and height of the window. If they are larger than
    this, and Emacs supports it, then the images will be rescaled down to
    fit these criteria.

`gnus-article-show-cursor` [¶](#index-gnus_002darticle_002dshow_002dcursor)
:   If non-`nil`, display the cursor in the article buffer even when
    the article buffer isn’t the current buffer.

To use this, make sure that you have `w3m` and `curl`
installed. If you have, then Gnus should display HTML
automatically.

---

Next: [Article Keymap](#Article-Keymap), Previous: [HTML](#HTML), Up: [Article Buffer](#Article-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.4 Customizing Articles [¶](#Customizing-Articles-1)

A slew of functions for customizing how the articles are to look like
exist. You can call these functions interactively
(see [Article Washing](#Article-Washing)), or you can have them
called automatically when you select the articles.

To have them called automatically, you should set the corresponding
“treatment” variable. For instance, to have headers hidden, you’d set
`gnus-treat-hide-headers`. Below is a list of variables that can
be set, but first we discuss the values these variables can have.

Note: Some values, while valid, make little sense. Check the list below
for sensible values.

1. `nil`: Don’t do this treatment.
2. `t`: Do this treatment on all body parts.
3. `head`: Do the treatment on the headers.
4. `first`: Do this treatment on the first body part.
5. `last`: Do this treatment on the last body part.
6. An integer: Do this treatment on all body parts that have a length less
   than this number.
7. A list of strings: Do this treatment on all body parts that are in
   articles that are read in groups that have names that match one of the
   regexps in the list.
8. A list where the first element is not a string:

   The list is evaluated recursively. The first element of the list is a
   predicate. The following predicates are recognized: `or`,
   `and`, `not` and `typep`. Here’s an example:

   ```
   (or last
       (typep "text/x-vcard"))
   ```
9. A function: the function is called with no arguments and should return
   `nil` or non-`nil`. The current article is available in the
   buffer named by `gnus-article-buffer`.

You may have noticed that the word *part* is used here. This refers
to the fact that some messages are MIME multipart articles that may
be divided into several parts. Articles that are not multiparts are
considered to contain just a single part.

Are the treatments applied to all sorts of multipart parts? Yes, if you
want to, but by default, only ‘`text/plain`’ parts are given the
treatment. This is controlled by the `gnus-article-treat-types`
variable, which is a list of regular expressions that are matched to the
type of the part. This variable is ignored if the value of the
controlling variable is a predicate list, as described above.

The following treatment options are available. The easiest way to
customize this is to examine the `gnus-article-treat` customization
group. Values in parenthesis are suggested sensible values. Others are
possible but those listed are probably sufficient for most people.

`gnus-treat-buttonize (t, integer)`

`gnus-treat-buttonize-head (head)`
:   See [Article Buttons](#Article-Buttons).

`gnus-treat-capitalize-sentences (t, integer)`

`gnus-treat-overstrike (t, integer)`

`gnus-treat-strip-cr (t, integer)`

`gnus-treat-emojize-symbols (t, integer)`

`gnus-treat-strip-headers-in-body (t, integer)`

`gnus-treat-strip-leading-blank-lines (t, first, integer)`

`gnus-treat-strip-multiple-blank-lines (t, integer)`

`gnus-treat-strip-pem (t, last, integer)`

`gnus-treat-strip-trailing-blank-lines (t, last, integer)`

`gnus-treat-unsplit-urls (t, integer)`

`gnus-treat-wash-html (t, integer)`
:   See [Article Washing](#Article-Washing).

`gnus-treat-date (head)`
:   This will transform/add date headers according to the
    `gnus-article-date-headers` variable. This is a list of Date
    headers to display. The formats available are:

    `ut`
    :   Universal time, aka GMT, aka ZULU.

    `local`
    :   The user’s local time zone.

    `english`
    :   A semi-readable English sentence.

    `lapsed`
    :   The time elapsed since the message was posted.

    `combined-lapsed`
    :   Both the original date header and a (shortened) elapsed time.

    `combined-local-lapsed`
    :   Both the time in the user’s local time zone a (shortened) elapsed
        time.

    `original`
    :   The original date header.

    `iso8601`
    :   ISO8601 format, i.e., “2010-11-23T22:05:21”.

    `user-defined`
    :   A format done according to the `gnus-article-time-format`
        variable.

    See [Article Date](#Article-Date).

`gnus-treat-from-picon (head)`

`gnus-treat-mail-picon (head)`

`gnus-treat-newsgroups-picon (head)`
:   See [Picons](#Picons).

`gnus-treat-from-gravatar (head)`

`gnus-treat-mail-gravatar (head)`
:   See [Gravatars](#Gravatars).

`gnus-treat-display-smileys (t, integer)`

`gnus-treat-body-boundary (head)`
:   Adds a delimiter between header and body, the string used as delimiter
    is controlled by `gnus-body-boundary-delimiter`.

    See [Smileys](#Smileys).

`gnus-treat-display-x-face (head)` [¶](#index-gnus_002dtreat_002ddisplay_002dx_002dface)
:   See [X-Face](#X_002dFace).

`gnus-treat-display-face (head)` [¶](#index-gnus_002dtreat_002ddisplay_002dface)
:   See [Face](#Face).

`gnus-treat-emphasize (t, head, integer)` [¶](#index-gnus_002dtreat_002dfill_002darticle)

`gnus-treat-fill-article (t, integer)` [¶](#index-gnus_002dtreat_002dfill_002dlong_002dlines)

`gnus-treat-fill-long-lines (t, integer)` [¶](#index-gnus_002dtreat_002dhide_002dboring_002dheaders)

`gnus-treat-hide-boring-headers (head)` [¶](#index-gnus_002dtreat_002dhide_002dcitation)

`gnus-treat-hide-citation (t, integer)` [¶](#index-gnus_002dtreat_002dhide_002dcitation_002dmaybe)

`gnus-treat-hide-citation-maybe (t, integer)` [¶](#index-gnus_002dtreat_002dhide_002dheaders)

`gnus-treat-hide-headers (head)` [¶](#index-gnus_002dtreat_002dhide_002dsignature)

`gnus-treat-hide-signature (t, last)` [¶](#index-gnus_002dtreat_002dstrip_002dbanner)

`gnus-treat-strip-banner (t, last)` [¶](#index-gnus_002dtreat_002dstrip_002dlist_002didentifiers)

`gnus-treat-strip-list-identifiers (head)`
:   See [Article Hiding](#Article-Hiding).

`gnus-treat-highlight-citation (t, integer)` [¶](#index-gnus_002dtreat_002dhighlight_002dheaders)

`gnus-treat-highlight-headers (head)` [¶](#index-gnus_002dtreat_002dhighlight_002dsignature)

`gnus-treat-highlight-signature (t, last, integer)`
:   See [Article Highlighting](#Article-Highlighting).

`gnus-treat-ansi-sequences (t)` [¶](#index-gnus_002dtreat_002dx_002dpgp_002dsig)

`gnus-treat-x-pgp-sig (head)`

`gnus-treat-unfold-headers (head)` [¶](#index-gnus_002dtreat_002dfold_002dheaders)

`gnus-treat-fold-headers (head)` [¶](#index-gnus_002dtreat_002dfold_002dnewsgroups)

`gnus-treat-fold-newsgroups (head)` [¶](#index-gnus_002dtreat_002dleading_002dwhitespace)

`gnus-treat-leading-whitespace (head)`
:   See [Article Header](#Article-Header).

You can, of course, write your own functions to be called from
`gnus-part-display-hook`. The functions are called narrowed to the
part, and you can do anything you like, pretty much. There is no
information that you have to keep in the buffer—you can change
everything.

---

Next: [Misc Article](#Misc-Article), Previous: [Customizing Articles](#Customizing-Articles), Up: [Article Buffer](#Article-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.5 Article Keymap [¶](#Article-Keymap-1)

Most of the keystrokes in the summary buffer can also be used in the
article buffer. They should behave as if you typed them in the summary
buffer, which means that you don’t actually have to have a summary
buffer displayed while reading. You can do it all from the article
buffer.

The key `v` is reserved for users. You can bind it to some
command or better use it as a prefix key.

A few additional keystrokes are available:

`SPC` [¶](#index-SPC-_0028Article_0029)
:   Scroll forwards one page (`gnus-article-next-page`).
    This is exactly the same as `h SPC h`.

`DEL` [¶](#index-DEL-_0028Article_0029)
:   Scroll backwards one page (`gnus-article-prev-page`).
    This is exactly the same as `h DEL h`.

`C-c ^` [¶](#index-C_002dc-_005e-_0028Article_0029)
:   If point is in the neighborhood of a `Message-ID` and you press
    `C-c ^`, Gnus will try to get that article from the server
    (`gnus-article-refer-article`).

`C-c C-m` [¶](#index-C_002dc-C_002dm-_0028Article_0029)
:   Send a reply to the address near point (`gnus-article-mail`). If
    given a prefix, include the mail.

`s` [¶](#index-s-_0028Article_0029)
:   Reconfigure the buffers so that the summary buffer becomes visible
    (`gnus-article-show-summary`).

`?` [¶](#index-_003f-_0028Article_0029)
:   Give a very brief description of the available keystrokes
    (`gnus-article-describe-briefly`).

`TAB` [¶](#index-TAB-_0028Article_0029)
:   Go to the next button, if any (`gnus-article-next-button`). This
    only makes sense if you have buttonizing turned on.

`M-TAB` [¶](#index-M_002dTAB-_0028Article_0029)
:   Go to the previous button, if any (`gnus-article-prev-button`).

`R` [¶](#index-R-_0028Article_0029)
:   Send a reply to the current article and yank the current article
    (`gnus-article-reply-with-original`). If the region is active,
    only yank the text in the region.

`S W` [¶](#index-S-W-_0028Article_0029)
:   Send a wide reply to the current article and yank the current article
    (`gnus-article-wide-reply-with-original`). If the region is
    active, only yank the text in the region.

`F` [¶](#index-F-_0028Article_0029)
:   Send a followup to the current article and yank the current article
    (`gnus-article-followup-with-original`). If the region is active,
    only yank the text in the region.

---

Previous: [Article Keymap](#Article-Keymap), Up: [Article Buffer](#Article-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.6 Misc Article [¶](#Misc-Article-1)

`gnus-single-article-buffer` [¶](#index-gnus_002dsingle_002darticle_002dbuffer)
:   If non-`nil`, use the same article buffer for all the groups.
    (This is the default.) If `nil`, each group will have its own
    article buffer.

`gnus-widen-article-window` [¶](#index-gnus_002dwiden_002darticle_002dwindow)
:   If non-`nil`, selecting the article buffer with the `h`
    command will “widen” the article window to take the entire frame.

`gnus-article-decode-hook` [¶](#index-MIME-1)
:   Hook used to decode MIME articles. The default value is
    `(article-decode-charset article-decode-encoded-words)`

`gnus-article-prepare-hook` [¶](#index-gnus_002darticle_002dprepare_002dhook)
:   This hook is called right after the article has been inserted into the
    article buffer. It is mainly intended for functions that do something
    depending on the contents; it should probably not be used for changing
    the contents of the article buffer.

`gnus-article-mode-hook` [¶](#index-gnus_002darticle_002dmode_002dhook)
:   Hook called in article mode buffers.

`gnus-article-mode-syntax-table` [¶](#index-gnus_002darticle_002dmode_002dsyntax_002dtable)
:   Syntax table used in article buffers. It is initialized from
    `text-mode-syntax-table`.

`gnus-article-over-scroll` [¶](#index-gnus_002darticle_002dover_002dscroll)
:   If non-`nil`, allow scrolling the article buffer even when there
    no more new text to scroll in. The default is `nil`.

`gnus-article-mode-line-format` [¶](#index-gnus_002darticle_002dmode_002dline_002dformat)
:   This variable is a format string along the same lines as
    `gnus-summary-mode-line-format` (see [Summary Buffer Mode Line](#Summary-Buffer-Mode-Line)). It accepts the same format specifications as that variable,
    with two extensions:

    ‘`w`’
    :   The *wash status* of the article. This is a short string with one
        character for each possible article wash operation that may have been
        performed. The characters and their meaning:

        ‘`c`’
        :   Displayed when cited text may be hidden in the article buffer.

        ‘`h`’
        :   Displayed when headers are hidden in the article buffer.

        ‘`p`’
        :   Displayed when article is digitally signed or encrypted, and Gnus has
            hidden the security headers. (N.B. does not tell anything about
            security status, i.e., good or bad signature.)

        ‘`s`’
        :   Displayed when the signature has been hidden in the Article buffer.

        ‘`o`’
        :   Displayed when Gnus has treated overstrike characters in the article buffer.

        ‘`e`’
        :   Displayed when Gnus has treated emphasized strings in the article buffer.

    ‘`m`’
    :   The number of MIME parts in the article.

`gnus-break-pages`
:   Controls whether *page breaking* is to take place. If this variable
    is non-`nil`, the articles will be divided into pages whenever a
    page delimiter appears in the article. If this variable is `nil`,
    paging will not be done.

`gnus-page-delimiter` [¶](#index-gnus_002dpage_002ddelimiter)
:   This is the delimiter mentioned above. By default, it is ‘`^L`’
    (formfeed).

`gnus-use-idna` [¶](#index-IDNA)
:   This variable controls whether Gnus performs IDNA decoding of
    internationalized domain names inside ‘`From`’, ‘`To`’ and
    ‘`Cc`’ headers. See [IDNA](https://www.gnu.org/software/emacs/manual/html_mono/message.html#IDNA) in The Message Manual,
    for how to compose such messages. This requires
    [GNU Libidn](https://www.gnu.org/software/libidn/), and this
    variable is only enabled if you have installed it.

`gnus-inhibit-images` [¶](#index-gnus_002dinhibit_002dimages)
:   If this is non-`nil`, inhibit displaying of images inline in the
    article body. It is effective to images that are in articles as
    MIME parts, and images in HTML articles rendered
    when `mm-text-html-renderer` (see [Display Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Display-Customization) in The Emacs MIME Manual) is
    `shr` or `gnus-w3m`.

---

Next: [Select Methods](#Select-Methods), Previous: [Article Buffer](#Article-Buffer), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 6 Composing Messages [¶](#Composing-Messages-1)

All commands for posting and mailing will put you in a message buffer
where you can edit the article all you like, before you send the
article by pressing `C-c C-c`. See [Overview](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Top) in Message Manual. Where the message will be posted/mailed to depends
on your setup (see [Posting Server](#Posting-Server)).

Also see [Canceling Articles](#Canceling-and-Superseding) for information on how to
remove articles you shouldn’t have posted.

* [Mail](#Mail)
* [Posting Server](#Posting-Server)
* [POP before SMTP](#POP-before-SMTP)
* [Mail and Post](#Mail-and-Post)
* [Archived Messages](#Archived-Messages)
* [Posting Styles](#Posting-Styles)
* [Drafts](#Drafts)
* [Rejected Articles](#Rejected-Articles)
* [Signing and encrypting](#Signing-and-encrypting)

---

Next: [Posting Server](#Posting-Server), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.1 Mail [¶](#Mail-1)

Variables for customizing outgoing mail:

`gnus-uu-digest-headers` [¶](#index-gnus_002duu_002ddigest_002dheaders)
:   List of regexps to match headers included in digested messages. The
    headers will be included in the sequence they are matched. If
    `nil` include all headers.

`gnus-add-to-list` [¶](#index-gnus_002dadd_002dto_002dlist-1)
:   If non-`nil`, add a `to-list` group parameter to mail groups
    that have none when you do a `a`.

`gnus-confirm-mail-reply-to-news` [¶](#index-gnus_002dconfirm_002dmail_002dreply_002dto_002dnews)
:   If non-`nil`, Gnus will ask you for a confirmation when you are
    about to reply to news articles by mail. If it is `nil`, nothing
    interferes in what you want to do. This can also be a function
    receiving the group name as the only parameter which should return
    non-`nil` if a confirmation is needed, or a regular expression
    matching group names, where confirmation should be asked for.

    If you find yourself never wanting to reply to mail, but occasionally
    press `R` anyway, this variable might be for you.

`gnus-confirm-treat-mail-like-news` [¶](#index-gnus_002dconfirm_002dtreat_002dmail_002dlike_002dnews)
:   If non-`nil`, Gnus also requests confirmation according to
    `gnus-confirm-mail-reply-to-news` when replying to mail. This is
    useful for treating mailing lists like newsgroups.

---

Next: [POP before SMTP](#POP-before-SMTP), Previous: [Mail](#Mail), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.2 Posting Server [¶](#Posting-Server-1)

When you press those magical `C-c C-c` keys to ship off your latest
(extremely intelligent, of course) article, where does it go?

Thank you for asking. I hate you.

It can be quite complicated.

When posting news, Message usually invokes `message-send-news`
(see [News Variables](https://www.gnu.org/software/emacs/manual/html_mono/message.html#News-Variables) in Message Manual).
Normally, Gnus will post using the same select method as you’re
reading from (which might be convenient if you’re reading lots of
groups from different private servers). However. If the server
you’re reading from doesn’t allow posting, just reading, you probably
want to use some other server to post your (extremely intelligent and
fabulously interesting) articles. You can then set the
`gnus-post-method` to some other method:

```
(setq gnus-post-method '(nnspool ""))
```

Now, if you’ve done this, and then this server rejects your article, or
this server is down, what do you do then? To override this variable you
can use a non-zero prefix to the `C-c C-c` command to force using
the “current” server, to get back the default behavior, for posting.

If you give a zero prefix (i.e., `C-u 0 C-c C-c`) to that command,
Gnus will prompt you for what method to use for posting.

You can also set `gnus-post-method` to a list of select methods.
If that’s the case, Gnus will always prompt you for what method to use
for posting.

Finally, if you want to always post using the native select method,
you can set this variable to `native`.

When sending mail, Message invokes the function specified by the
variable `message-send-mail-function`. Gnus tries to set it to a
value suitable for your system.
See [Mail Variables](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Mail-Variables) in Message manual, for more
information.

---

Next: [Mail and Post](#Mail-and-Post), Previous: [Posting Server](#Posting-Server), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.3 POP before SMTP [¶](#POP-before-SMTP-1)

Does your ISP use POP-before-SMTP
authentication? This authentication method simply requires you to
contact the POP server before sending email. To do that,
put the following lines in your `~/.gnus.el` file:

```
(add-hook 'message-send-mail-hook 'mail-source-touch-pop)
```

The `mail-source-touch-pop` function does POP
authentication according to the value of `mail-sources` without
fetching mails, just before sending a mail. See [Mail Sources](#Mail-Sources).

If you have two or more POP mail servers set in
`mail-sources`, you may want to specify one of them to
`mail-source-primary-source` as the POP mail server to be
used for the POP-before-SMTP authentication. If it
is your primary POP mail server (i.e., you are fetching mails
mainly from that server), you can set it permanently as follows:

```
(setq mail-source-primary-source
      '(pop :server "pop3.mail.server"
            :password "secret"))
```

Otherwise, bind it dynamically only when performing the
POP-before-SMTP authentication as follows:

```
(add-hook 'message-send-mail-hook
          (lambda ()
            (let ((mail-source-primary-source
                   '(pop :server "pop3.mail.server"
                         :password "secret")))
              (mail-source-touch-pop))))
```

---

Next: [Archived Messages](#Archived-Messages), Previous: [POP before SMTP](#POP-before-SMTP), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.4 Mail and Post [¶](#Mail-and-Post-1)

Here’s a list of variables relevant to both mailing and
posting:

`gnus-mailing-list-groups` [¶](#index-gnus_002dmailing_002dlist_002dgroups)
:   If your news server offers groups that are really mailing lists
    gatewayed to the NNTP server, you can read those groups without
    problems, but you can’t post/followup to them without some difficulty.
    One solution is to add a `to-address` to the group parameters
    (see [Group Parameters](#Group-Parameters)). An easier thing to do is set the
    `gnus-mailing-list-groups` to a regexp that matches the groups that
    really are mailing lists. Then, at least, followups to the mailing
    lists will work most of the time. Posting to these groups (`a`) is
    still a pain, though.

`gnus-user-agent` [¶](#index-gnus_002duser_002dagent)
:   This variable controls which information should be exposed in the
    User-Agent header. It can be a list of symbols or a string. Valid
    symbols are `gnus` (show Gnus version) and `emacs` (show
    Emacs version). In addition to the Emacs version, you can add
    `config` (show system configuration) or `type` (show system
    type). If you set it to a string, be sure to use a valid format, see
    RFC 2616.

You may want to do spell-checking on messages that you send out. Or, if
you don’t want to spell-check by hand, you could add automatic
spell-checking via the `ispell` package:

```
(add-hook 'message-send-hook 'ispell-message)
```

If you want to change the `ispell` dictionary based on what group
you’re in, you could say something like the following:

```
(add-hook 'gnus-select-group-hook
          (lambda ()
            (cond
             ((string-match
               "^de\\." (gnus-group-real-name gnus-newsgroup-name))
              (ispell-change-dictionary "deutsch"))
             (t
              (ispell-change-dictionary "english")))))
```

Modify to suit your needs.

If `gnus-message-highlight-citation` is `t`, different levels of
citations are highlighted like in Gnus article buffers also in message
mode buffers.

---

Next: [Posting Styles](#Posting-Styles), Previous: [Mail and Post](#Mail-and-Post), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.5 Archived Messages [¶](#Archived-Messages-1)

Gnus provides a few different methods for storing the mail and news you
send. The default method is to use the *archive virtual server* to
store the messages. If you want to disable this completely, the
`gnus-message-archive-group` variable should be `nil`. The
default is `"sent.%Y-%m"`, which gives you one archive group per month.

For archiving interesting messages in a group you read, see the
`B c` (`gnus-summary-copy-article`) command (see [Mail Group Commands](#Mail-Group-Commands)).

`gnus-message-archive-method` says what virtual server Gnus is to
use to store sent messages. The default is `"archive"`, and when
actually being used it is expanded into:

```
(nnfolder "archive"
          (nnfolder-directory   "~/Mail/archive")
          (nnfolder-active-file "~/Mail/archive/active")
          (nnfolder-get-new-mail nil)
          (nnfolder-inhibit-expiry t))
```

> Note: a server like this is saved in the `~/.newsrc.eld` file first
> so that it may be used as a real method of the server which is named
> `"archive"` (that is, for the case where
> `gnus-message-archive-method` is set to `"archive"`) ever
> since. If it once has been saved, it will never be updated by default
> even if you change the value of `gnus-message-archive-method`
> afterward. Therefore, the server `"archive"` doesn’t necessarily
> mean the `nnfolder` server like this at all times. If you want the
> saved method to reflect always the value of
> `gnus-message-archive-method`, set the
> `gnus-update-message-archive-method` variable to a non-`nil`
> value. The default value of this variable is `nil`.

You can, however, use any mail select method (`nnml`,
`nnmbox`, etc.). `nnfolder` is a quite likable select method
for doing this sort of thing, though. If you don’t like the default
directory chosen, you could say something like:

```
(setq gnus-message-archive-method
      '(nnfolder "archive"
                 (nnfolder-inhibit-expiry t)
                 (nnfolder-active-file "~/News/sent-mail/active")
                 (nnfolder-directory "~/News/sent-mail/")))
```

Gnus will insert `Gcc` headers in all outgoing messages that point
to one or more group(s) on that server. Which group to use is
determined by the `gnus-message-archive-group` variable.

This variable can be used to do the following:

a string
:   Messages will be saved in that group.

    Note that you can include a select method in the group name, then the
    message will not be stored in the select method given by
    `gnus-message-archive-method`, but in the select method specified
    by the group name, instead. Suppose `gnus-message-archive-method`
    has the default value shown above. Then setting
    `gnus-message-archive-group` to `"foo"` means that outgoing
    messages are stored in ‘`nnfolder+archive:foo`’, but if you use the
    value `"nnml:foo"`, then outgoing messages will be stored in
    ‘`nnml:foo`’.

a list of strings
:   Messages will be saved in all those groups.

an alist of regexps, functions and forms
:   When a key “matches”, the result is used.

`nil`
:   No message archiving will take place.

Let’s illustrate:

Just saving to a single group called ‘`MisK`’:

```
(setq gnus-message-archive-group "MisK")
```

Saving to two groups, ‘`MisK`’ and ‘`safe`’:

```
(setq gnus-message-archive-group '("MisK" "safe"))
```

Save to different groups based on what group you are in:

```
(setq gnus-message-archive-group
      '(("^alt" "sent-to-alt")
        ("mail" "sent-to-mail")
        (".*" "sent-to-misc")))
```

More complex stuff:

```
(setq gnus-message-archive-group
      '((if (message-news-p)
            "misc-news"
          "misc-mail")))
```

How about storing all news messages in one file, but storing all mail
messages in one file per month:

```
(setq gnus-message-archive-group
      '((if (message-news-p)
            "misc-news"
          (concat "mail." (format-time-string "%Y-%m")))))
```

Now, when you send a message off, it will be stored in the appropriate
group. (If you want to disable storing for just one particular message,
you can just remove the `Gcc` header that has been inserted.) The
archive group will appear in the group buffer the next time you start
Gnus, or the next time you press `F` in the group buffer. You can
enter it and read the articles in it just like you’d read any other
group. If the group gets really big and annoying, you can simply rename
if (using `G r` in the group buffer) to something
nice—‘`misc-mail-september-1995`’, or whatever. New messages will
continue to be stored in the old (now empty) group.

`gnus-gcc-mark-as-read` [¶](#index-gnus_002dgcc_002dmark_002das_002dread)
:   If non-`nil`, automatically mark `Gcc` articles as read.

`gnus-gcc-externalize-attachments` [¶](#index-gnus_002dgcc_002dexternalize_002dattachments)
:   If `nil`, attach files as normal parts in Gcc copies; if a regexp
    and matches the Gcc group name, attach files as external parts; if it is
    `all`, attach local files as external parts; if it is other
    non-`nil`, the behavior is the same as `all`, but it may be
    changed in the future.

`gnus-gcc-self-resent-messages` [¶](#index-gnus_002dgcc_002dself_002dresent_002dmessages)
:   Like the `gcc-self` group parameter, applied only for unmodified
    messages that `gnus-summary-resend-message` (see [Summary Mail Commands](#Summary-Mail-Commands)) resends. Non-`nil` value of this variable takes
    precedence over any existing `Gcc` header.

    If this is `none`, no `Gcc` copy will be made. If this is
    `t`, messages resent will be `Gcc` copied to the current
    group. If this is a string, it specifies a group to which resent
    messages will be `Gcc` copied. If this is `nil`, `Gcc`
    will be done according to existing `Gcc` header(s), if any. If
    this is `no-gcc-self`, that is the default, resent messages will be
    `Gcc` copied to groups that existing `Gcc` header specifies,
    except for the current group.

`gnus-gcc-pre-body-encode-hook` [¶](#index-gnus_002dgcc_002dpost_002dbody_002dencode_002dhook)

`gnus-gcc-post-body-encode-hook`
:   These hooks are run before/after encoding the message body of the Gcc
    copy of a sent message. The current buffer (when the hook is run)
    contains the message including the message header. Changes made to
    the message will only affect the Gcc copy, but not the original
    message. You can use these hooks to edit the copy (and influence
    subsequent transformations), e.g., remove MML secure tags
    (see [Signing and encrypting](#Signing-and-encrypting)).

---

Next: [Drafts](#Drafts), Previous: [Archived Messages](#Archived-Messages), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.6 Posting Styles [¶](#Posting-Styles-1)

All them variables, they make my head swim.

So what if you want a different `Organization` and signature based
on what groups you post to? And you post both from your home machine
and your work machine, and you want different `From` lines, and so
on?

One way to do stuff like that is to write clever hooks that change the
variables you need to have changed. That’s a bit boring, so somebody
came up with the bright idea of letting the user specify these things in
a handy alist. Here’s an example of a `gnus-posting-styles`
variable:

```
((".*"
  (signature "Peace and happiness")
  (organization "What me?"))
 ("^comp"
  (signature "Death to everybody"))
 ("comp.emacs.i-love-it"
  (organization "Emacs is it")))
```

As you might surmise from this example, this alist consists of several
*styles*. Each style will be applicable if the first element
“matches”, in some form or other. The entire alist will be iterated
over, from the beginning towards the end, and each match will be
applied, which means that attributes in later styles that match override
the same attributes in earlier matching styles. So
‘`comp.programming.literate`’ will have the ‘`Death to everybody`’
signature and the ‘`What me?`’ `Organization` header.

The first element in each style is called the `match`. If it’s a
string, then Gnus will try to regexp match it against the group name.
If it is the form `(header match regexp)`, then Gnus
will look in the original article for a header whose name is
match and compare that regexp. match and
regexp are strings. (The original article is the one you are
replying or following up to. If you are not composing a reply or a
followup, then there is nothing to match against.) If the
`match` is a function symbol, that function will be called with
no arguments. If it’s a variable symbol, then the variable will be
referenced. If it’s a list, then that list will be `eval`ed. In
any case, if this returns a non-`nil` value, then the style is
said to *match*.

Each style may contain an arbitrary amount of *attributes*. Each
attribute consists of a `(name value)` pair. In
addition, you can also use the `(name :file value)`
form or the `(name :value value)` form. Where
`:file` signifies value represents a file name and its
contents should be used as the attribute value, `:value` signifies
value does not represent a file name explicitly. The attribute
name can be one of:

* `signature`
* `signature-file`
* `x-face-file`
* `address`, overriding `user-mail-address`
* `name`, overriding `(user-full-name)`
* `body`

Note that the `signature-file` attribute honors the variable
`message-signature-directory`.

The attribute name can also be a string or a symbol. In that case,
this will be used as a header name, and the value will be inserted in
the headers of the article; if the value is `nil`, the header
name will be removed. If the attribute name is `eval`, the form
is evaluated, and the result is thrown away.

The attribute value can be a string, a function with zero arguments
(the return value will be used), a variable (its value will be used)
or a list (it will be `eval`ed and the return value will be
used). The functions and sexps are called/`eval`ed in the
message buffer that is being set up.

In the case of a string value, if the `match` is a regular
expression, or if it takes the form `(header match
regexp)`, a ‘`gnus-match-substitute-replacement`’ is proceed
on the value to replace the positional parameters ‘`\n`’ by
the corresponding parenthetical matches (see [Replacing the Text that Matched](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Replacing-Match) in The Emacs Lisp Reference
Manual.)

If you wish to check whether the message you are about to compose is
meant to be a news article or a mail message, you can check the values
of the `message-news-p` and `message-mail-p` functions.

So here’s a new example:

```
(setq gnus-posting-styles
      '((".*"
         (signature-file "~/.signature")
         (name "User Name")
         (x-face-file "~/.xface")
         (x-url (getenv "WWW_HOME"))
         (organization "People's Front Against MWM"))
        ("^rec.humor"
         (signature my-funny-signature-randomizer))
        ((equal (system-name) "gnarly")  ;; A form
         (signature my-quote-randomizer))
        (message-news-p        ;; A function symbol
         (signature my-news-signature))
        (window-system         ;; A value symbol
         ("X-Window-System" (format "%s" window-system)))
        ;; If I’m replying to Larsi, set the Organization header.
        ((header "from" "larsi.*org")
         (Organization "Somewhere, Inc."))
        ;; Reply to a message from the same subaddress the message
        ;; was sent to.
        ((header "x-original-to" "me\\(\\+.+\\)@example.org")
         (address "me\\1@example.org"))
        ((posting-from-work-p) ;; A user defined function
         (signature-file "~/.work-signature")
         (address "user@bar.foo")
         (body "You are fired.\n\nSincerely, your boss.")
         ("X-Message-SMTP-Method" "smtp smtp.example.org 587")
         (organization "Important Work, Inc"))
        ("nnml:.*"
         (From (with-current-buffer gnus-article-buffer
                 (message-fetch-field "to"))))
        ("^nn.+:"
         (signature-file "~/.mail-signature"))))
```

The ‘`nnml:.*`’ rule means that you use the `To` address as the
`From` address in all your outgoing replies, which might be handy
if you fill many roles.
You may also use `message-alternative-emails` instead.
See [Message Headers](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Message-Headers) in Message Manual.

Of particular interest in the “work-mail” style is the
‘`X-Message-SMTP-Method`’ header. It specifies how to send the
outgoing email. You may want to sent certain emails through certain
SMTP servers due to company policies, for instance.
See [Message Variables](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Mail-Variables) in Message Manual.

---

Next: [Rejected Articles](#Rejected-Articles), Previous: [Posting Styles](#Posting-Styles), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.7 Drafts [¶](#Drafts-1)

If you are writing a message (mail or news) and suddenly remember that
you have a steak in the oven (or some pesto in the food processor, you
craaazy vegetarians), you’ll probably wish there was a method to save
the message you are writing so that you can continue editing it some
other day, and send it when you feel its finished.

Well, don’t worry about it. Whenever you start composing a message of
some sort using the Gnus mail and post commands, the buffer you get will
automatically associate to an article in a special *draft* group.
If you save the buffer the normal way (`C-x C-s`, for instance), the
article will be saved there. (Auto-save files also go to the draft
group.)

The draft group is a special group (which is implemented as an
`nndraft` group, if you absolutely have to know) called
‘`nndraft:drafts`’. The variable `nndraft-directory` says where
`nndraft` is to store its files. What makes this group special is
that you can’t tick any articles in it or mark any articles as
read—all articles in the group are permanently unread.

If the group doesn’t exist, it will be created and you’ll be subscribed
to it. The only way to make it disappear from the Group buffer is to
unsubscribe it. The special properties of the draft group comes from
a group property (see [Group Parameters](#Group-Parameters)), and if lost the group
behaves like any other group. This means the commands below will not
be available. To restore the special properties of the group, the
simplest way is to kill the group, using `C-k`, and restart
Gnus. The group is automatically created again with the
correct parameters. The content of the group is not lost.

When you want to continue editing the article, you simply enter the
draft group and push `D e` (`gnus-draft-edit-message`) to do
that. You will be placed in a buffer where you left off.

Rejected articles will also be put in this draft group (see [Rejected Articles](#Rejected-Articles)).

If you have lots of rejected messages you want to post (or mail) without
doing further editing, you can use the `D s` command
(`gnus-draft-send-message`). This command understands the
process/prefix convention (see [Process/Prefix](#Process_002fPrefix)). The `D S`
command (`gnus-draft-send-all-messages`) will ship off all messages
in the buffer.

If you have some messages that you wish not to send, you can use the
`D t` (`gnus-draft-toggle-sending`) command to mark the message
as unsendable. This is a toggling command.

Finally, if you want to delete a draft, use the normal `B DEL`
command (see [Mail Group Commands](#Mail-Group-Commands)).

---

Next: [Signing and encrypting](#Signing-and-encrypting), Previous: [Drafts](#Drafts), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 6.8 Rejected Articles [¶](#Rejected-Articles-1)

Sometimes a news server will reject an article. Perhaps the server
doesn’t like your face. Perhaps it just feels miserable. Perhaps
*there be demons*. Perhaps you have included too much cited text.
Perhaps the disk is full. Perhaps the server is down.

These situations are, of course, totally beyond the control of Gnus.
(Gnus, of course, loves the way you look, always feels great, has angels
fluttering around inside of it, doesn’t care about how much cited text
you include, never runs full and never goes down.) So Gnus saves these
articles until some later time when the server feels better.

The rejected articles will automatically be put in a special draft group
(see [Drafts](#Drafts)). When the server comes back up again, you’d then
typically enter that group and send all the articles off.

---

Previous: [Rejected Articles](#Rejected-Articles), Up: [Composing Messages](#Composing-Messages)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
