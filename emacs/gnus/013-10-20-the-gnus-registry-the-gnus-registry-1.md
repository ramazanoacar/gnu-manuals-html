### 10.20 The Gnus Registry [¶](#The-Gnus-Registry-1)

The Gnus registry is a package that tracks messages by their
Message-ID across all backends. This allows Gnus users to do several
cool things, be the envy of the locals, get free haircuts, and be
experts on world issues. Well, maybe not all of those, but the
features are pretty cool.

Although they will be explained in detail shortly, here’s a quick list
of said features in case your attention span is... never mind.

1. Split messages to their parent

   This keeps discussions in the same group. You can use the subject and
   the sender in addition to the Message-ID. Several strategies are
   available.
2. Refer to messages by ID

   Commands like `gnus-summary-refer-parent-article` can take
   advantage of the registry to jump to the referred article, regardless
   of the group the message is in.
3. Store custom flags and keywords

   The registry can store custom flags and keywords for a message. For
   instance, you can mark a message “To-Do” this way and the flag will
   persist whether the message is in the nnimap, nnml, nnmaildir,
   etc. backends.
4. Store arbitrary data

   Through a simple Lisp API, the registry can remember any data for a
   message. A built-in inverse map, when activated, allows quick lookups
   of all messages matching a particular set of criteria.

* [Gnus Registry Setup](#Gnus-Registry-Setup)
* [Fetching by `Message-ID` Using the Registry](#Registry-Article-Refer-Method)
* [Fancy splitting to parent](#Fancy-splitting-to-parent)
* [Store custom flags and keywords](#Store-custom-flags-and-keywords)
* [Store arbitrary data](#Store-arbitrary-data)

---

Next: [Fetching by `Message-ID` Using the Registry](#Registry-Article-Refer-Method), Up: [The Gnus Registry](#The-Gnus-Registry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.20.1 Gnus Registry Setup [¶](#Gnus-Registry-Setup-1)

Fortunately, setting up the Gnus registry is pretty easy:

```
(setq gnus-registry-max-entries 2500)

(gnus-registry-initialize)
```

This adds registry saves to Gnus newsrc saves (which happen on exit
and when you press `s` from the `*Group*` buffer). It also
adds registry calls to article actions in Gnus (copy, move, etc.) so
it’s not easy to undo the initialization. See
`gnus-registry-initialize` for the gory details.

Here are other settings used by the author of the registry (understand
what they do before you copy them blindly).

```
(setq
 gnus-registry-split-strategy 'majority
 gnus-registry-ignored-groups '(("nntp" t)
                                ("nnrss" t)
                                ("spam" t)
                                ("train" t))
 gnus-registry-max-entries 500000
 ;; this is the default
 gnus-registry-track-extra '(sender subject))
```

They say: keep a lot of messages around, track messages by sender and
subject (not just parent Message-ID), and when the registry splits
incoming mail, use a majority rule to decide where messages should go
if there’s more than one possibility. In addition, the registry
should ignore messages in groups that match “nntp”, “nnrss”,
“spam”, or “train.”

You are doubtless impressed by all this, but you ask: “I am a Gnus
user, I customize to live. Give me more.” Here you go, these are
the general settings.

Variable: **gnus-registry-unfollowed-groups** [¶](#index-gnus_002dregistry_002dunfollowed_002dgroups)
:   The groups that will not be followed by
    `gnus-registry-split-fancy-with-parent`. They will still be
    remembered by the registry. This is a list of regular expressions.
    By default any group name that ends with “delayed”, “drafts”,
    “queue”, or “INBOX”, belongs to the nnmairix backend, or contains
    the word “archive” is not followed.

Variable: **gnus-registry-max-entries** [¶](#index-gnus_002dregistry_002dmax_002dentries)
:   The number (an integer or `nil` for unlimited) of entries the
    registry will keep. If the registry has reached or exceeded this
    size, it will reject insertion of new entries.

Variable: **gnus-registry-register-all** [¶](#index-gnus_002dregistry_002dregister_002dall)
:   If this option is non-`nil`, the registry will register all messages, as
    you see them. This is important to making split-to-parent and
    Message-ID references work correctly, as the registry needs to know
    where all messages are, but it can slow down group opening and the
    saving of Gnus. If this option is `nil`, entries must be created
    manually, for instance by storing a custom flag or keyword for the
    message.

Variable: **gnus-registry-prune-factor** [¶](#index-gnus_002dregistry_002dprune_002dfactor)
:   This option (a float between 0 and 1) controls how much the registry
    is cut back during pruning. In order to prevent constant pruning, the
    registry will be pruned back to less than
    `gnus-registry-max-entries`. This option controls exactly how
    much less: the target is calculated as the maximum number of entries
    minus the maximum number times this factor. The default is 0.1:
    i.e., if your registry is limited to 50000 entries, pruning will try to
    cut back to 45000 entries. Entries with keys marked as precious will
    not be pruned.

Variable: **gnus-registry-default-sort-function** [¶](#index-gnus_002dregistry_002ddefault_002dsort_002dfunction)
:   This option specifies how registry entries are sorted during pruning.
    If a function is given, it should sort least valuable entries first,
    as pruning starts from the beginning of the list. The default value
    is `gnus-registry-sort-by-creation-time`, which proposes the
    oldest entries for pruning. Set to `nil` to perform no sorting,
    which will speed up the pruning process.

Variable: **gnus-registry-cache-file** [¶](#index-gnus_002dregistry_002dcache_002dfile)
:   The file where the registry will be stored between Gnus sessions. By
    default the file name is `.gnus.registry.eieio` in the same
    directory as your `.newsrc.eld`.

---

Next: [Fancy splitting to parent](#Fancy-splitting-to-parent), Previous: [Gnus Registry Setup](#Gnus-Registry-Setup), Up: [The Gnus Registry](#The-Gnus-Registry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.20.2 Fetching by `Message-ID` Using the Registry [¶](#Fetching-by-Message_002dID-Using-the-Registry)

The registry knows how to map each `Message-ID` to the group it’s
in. This can be leveraged to enhance the “article refer method”,
the thing that tells Gnus how to look up an article given its
Message-ID (see [Finding the Parent](#Finding-the-Parent)).

The `nnregistry` refer method does exactly that. It has the
advantage that an article may be found regardless of the group it’s
in—provided its `Message-ID` is known to the registry. It can
be enabled by augmenting the start-up file with something along these
lines:

```
;; Keep enough entries to have a good hit rate when referring to an
;; article using the registry.  Use long group names so that Gnus
;; knows where the article is.
(setq gnus-registry-max-entries 2500)

(gnus-registry-initialize)

(setq gnus-refer-article-method
      '(current
        (nnregistry)
        (nnweb "google" (nnweb-type google))))
```

The example above instructs Gnus to first look up the article in the
current group, or, alternatively, using the registry, and finally, if
all else fails, using Google.

---

Next: [Store custom flags and keywords](#Store-custom-flags-and-keywords), Previous: [Fetching by `Message-ID` Using the Registry](#Registry-Article-Refer-Method), Up: [The Gnus Registry](#The-Gnus-Registry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.20.3 Fancy splitting to parent [¶](#Fancy-splitting-to-parent-1)

Simply put, this lets you put followup e-mail where it belongs.

Every message has a Message-ID, which is unique, and the registry
remembers it. When the message is moved or copied, the registry will
notice this and offer the new group as a choice to the splitting
strategy.

When a followup is made, usually it mentions the original message’s
Message-ID in the headers. The registry knows this and uses that
mention to find the group where the original message lives. You only
have to put a rule like this:

```
(setq nnimap-my-split-fancy '(|

      ;; split to parent: you need this
      (: gnus-registry-split-fancy-with-parent)

      ;; other rules, as an example
      (: spam-split)
      ;; default mailbox
      "mail")
```

in your fancy split setup.

If `gnus-registry-register-all` is non-`nil` (the default),
the registry will perform splitting for all messages. If it is
`nil`, splitting will only happen for children of messages you’ve
explicitly registered.

In addition, you may want to customize the following variables.

Variable: **gnus-registry-track-extra** [¶](#index-gnus_002dregistry_002dtrack_002dextra)
:   This is a list of symbols, so it’s best to change it from the
    Customize interface. By default it’s `(subject sender recipient)`,
    which may work for you. It can be annoying if your mail flow is large
    and people don’t stick to the same groups.

    When you decide to stop tracking any of those extra data, you can use
    the command `gnus-registry-remove-extra-data` to purge it from
    the existing registry entries.

Variable: **gnus-registry-split-strategy** [¶](#index-gnus_002dregistry_002dsplit_002dstrategy)
:   This is a symbol, so it’s best to change it from the Customize
    interface. By default it’s `nil`, but you may want to set it to
    `majority` or `first` to split by sender or subject based on
    the majority of matches or on the first found. I find `majority`
    works best.

---

Next: [Store arbitrary data](#Store-arbitrary-data), Previous: [Fancy splitting to parent](#Fancy-splitting-to-parent), Up: [The Gnus Registry](#The-Gnus-Registry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.20.4 Store custom flags and keywords [¶](#Store-custom-flags-and-keywords-1)

The registry lets you set custom flags and keywords per message. You
can use the Gnus->Registry Marks menu or the `M M x` keyboard
shortcuts, where `x` is the first letter of the mark’s name.

Variable: **gnus-registry-marks** [¶](#index-gnus_002dregistry_002dmarks)
:   The custom marks that the registry can use. You can modify the
    default list, if you like. If you do, you’ll have to exit Emacs
    before they take effect (you can also unload the registry and reload
    it or evaluate the specific macros you’ll need, but you probably don’t
    want to bother). Use the Customize interface to modify the list.

    By default this list has the `Important`, `Work`,
    `Personal`, `To-Do`, and `Later` marks. They all have
    keyboard shortcuts like `M M i` for Important, using the first
    letter.

Function: **gnus-registry-set-article-mark** [¶](#index-gnus_002dregistry_002dset_002darticle_002dmark)
:   Call this function to mark an article with a custom registry mark. It
    will offer the available marks for completion.

You can use `defalias` to install a summary line formatting
function that will show the registry marks. There are two flavors of
this function, either showing the marks as single characters, using
their `:char` property, or showing the marks as full strings.

```
;; show the marks as single characters (see the :char property in
;; 'gnus-registry-marks'):
;; (defalias 'gnus-user-format-function-M 'gnus-registry-article-marks-to-chars)

;; show the marks by name (see 'gnus-registry-marks'):
;; (defalias 'gnus-user-format-function-M 'gnus-registry-article-marks-to-names)
```

---

Previous: [Store custom flags and keywords](#Store-custom-flags-and-keywords), Up: [The Gnus Registry](#The-Gnus-Registry)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.20.5 Store arbitrary data [¶](#Store-arbitrary-data-1)

The registry has a simple API that uses a Message-ID as the key to
store arbitrary data (as long as it can be converted to a list for
storage).

Function: **gnus-registry-set-id-key** (id key value) [¶](#index-gnus_002dregistry_002dset_002did_002dkey)
:   Store `value` under `key` for message `id`.

Function: **gnus-registry-get-id-key** (id key) [¶](#index-gnus_002dregistry_002dget_002did_002dkey)
:   Get the data under `key` for message `id`. If the option
    `gnus-registry-register-all` is non-`nil`, this function will also
    create an entry for `id` if one doesn’t exist.

Variable: **gnus-registry-extra-entries-precious** [¶](#index-gnus_002dregistry_002dextra_002dentries_002dprecious)
:   If any extra entries are precious, their presence will make the
    registry keep the whole entry forever, even if there are no groups for
    the Message-ID and if the size limit of the registry is reached. By
    default this is just `(mark)` so the custom registry marks are
    precious.

---

Next: [System Sleep Integration](#System-Sleep-Integration), Previous: [The Gnus Registry](#The-Gnus-Registry), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.21 The Gnus Cloud [¶](#The-Gnus-Cloud-1)

The Gnus Cloud is a way to synchronize marks and general files and
data across multiple machines.

Very often, you want all your marks (what articles you’ve read, which
ones were important, and so on) to be synchronized between several
machines. With IMAP, that’s built into the protocol, so you can read
nnimap groups from many machines and they are automatically
synchronized. But NNTP, nnrss, and many other backends do not store
marks, so you have to do it locally.

The Gnus Cloud package stores the marks, plus any files you choose, on
an IMAP server in a special folder. It’s like a
DropTorrentSyncBoxOakTree(TM).[3](#FOOT3)

* [Gnus Cloud Setup](#Gnus-Cloud-Setup)
* [Gnus Cloud Usage](#Gnus-Cloud-Usage)

---

Next: [Gnus Cloud Usage](#Gnus-Cloud-Usage), Up: [The Gnus Cloud](#The-Gnus-Cloud)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.21.1 Gnus Cloud Setup [¶](#Gnus-Cloud-Setup-1)

Setting up the Gnus Cloud takes less than a minute. From the Group
buffer:

Press `^` to go to the Server buffer. Here you’ll see all the
servers that Gnus knows. See [Server Buffer](#Server-Buffer).

Then press `i` to mark any servers as cloud-synchronized (their marks are synchronized).

Then press `I` to mark a single server as the cloud host (it must
be an IMAP server, and will host a special IMAP folder with all the
synchronization data). This will set the variable
`gnus-cloud-method` (using the Customize facilities), then ask
you to optionally upload your first CloudSynchronizationDataPack(TM).

---

Previous: [Gnus Cloud Setup](#Gnus-Cloud-Setup), Up: [The Gnus Cloud](#The-Gnus-Cloud)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.21.2 Gnus Cloud Usage [¶](#Gnus-Cloud-Usage-1)

After setting up, you can use these shortcuts from the Group buffer:

`~ RET`

`~ d` [¶](#index-gnus_002dcloud_002ddownload_002dall_002ddata)
:   Download the latest Gnus Cloud data.

`~ u`

`~ ~` [¶](#index-gnus_002dcloud_002dupload_002dall_002ddata)
:   Upload the local Gnus Cloud data. Creates a new
    CloudSynchronizationDataPack(TM).

But wait, there’s more. Of course there’s more. So much more. You can
customize all of the following.

Variable: **gnus-cloud-synced-files** [¶](#index-gnus_002dcloud_002dsynced_002dfiles)
:   These are the files that will be part of every
    CloudSynchronizationDataPack(TM). They are included in every upload,
    so don’t synchronize a lot of large files. Files under 100Kb are best.

Variable: **gnus-cloud-storage-method** [¶](#index-gnus_002dcloud_002dstorage_002dmethod)
:   This is a choice from several storage methods. It’s highly recommended
    to use the EPG facilities. It will be automatic if have GnuPG
    installed and EPG loaded. Otherwise, you could use Base64+gzip,
    Base64, or no encoding.

Variable: **gnus-cloud-interactive** [¶](#index-gnus_002dcloud_002dinteractive)
:   When this is set, and by default it is, the Gnus Cloud package will
    ask you for confirmation here and there. Leave it on until you’re
    comfortable with the package.

Variable: **gnus-cloud-method** [¶](#index-gnus_002dcloud_002dmethod)
:   The name of the IMAP server to store the
    CloudSynchronizationDataPack(TM)s. It’s easiest to set this from the
    Server buffer (see [Gnus Cloud Setup](#Gnus-Cloud-Setup)).

---

Next: [Interaction with other modes](#Other-modes), Previous: [The Gnus Cloud](#The-Gnus-Cloud), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.22 System Sleep Integration [¶](#System-Sleep-Integration-1)

When using laptops or other systems that have a sleep or hibernate
functionality, it’s possible for long-running server connections to
become “hung”, requiring the user to manually close and re-open the
connections after the system resumes. Using the system sleep library,
Gnus can automatically close all server connections before the system
goes to sleep. To enable this, set `gnus-close-on-sleep` to a
non-`nil` value.

---

Next: [Various Various](#Various-Various), Previous: [System Sleep Integration](#System-Sleep-Integration), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.23 Interaction with other modes [¶](#Interaction-with-other-modes)

* [Dired](#Dired)

#### 10.23.1 Dired [¶](#Dired)

`gnus-dired-minor-mode` provides some useful functions for dired
buffers. It is enabled with

```
(add-hook 'dired-mode-hook 'turn-on-gnus-dired-mode)
```

`C-c C-m C-a` [¶](#index-gnus_002ddired_002dattach)
:   Send dired’s marked files as an attachment (`gnus-dired-attach`).
    The function prompts for a message buffer, and by default attaches files
    to the end of that buffer; customize `gnus-dired-attach-at-end` to
    place the attachments at point instead.

`C-c C-m C-l` [¶](#index-gnus_002ddired_002dfind_002dfile_002dmailcap)
:   Visit a file according to the appropriate mailcap entry
    (`gnus-dired-find-file-mailcap`). With prefix, open file in a new
    buffer.

`C-c C-m C-p` [¶](#index-gnus_002ddired_002dprint)
:   Print file according to the mailcap entry (`gnus-dired-print`). If
    there is no print command, print in a PostScript image.

---

Previous: [Interaction with other modes](#Other-modes), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.24 Various Various [¶](#Various-Various-1)

`gnus-home-directory` [¶](#index-gnus_002dhome_002ddirectory)
:   All Gnus file and directory variables will be initialized from this
    variable, which defaults to `~/`.

`gnus-directory` [¶](#index-gnus_002ddirectory)
:   Most Gnus storage file and directory variables will be initialized from
    this variable, which defaults to the `SAVEDIR` environment
    variable, or `~/News/` if that variable isn’t set.

    Note that Gnus is mostly loaded when the `~/.gnus.el` file is read.
    This means that other directory variables that are initialized from this
    variable won’t be set properly if you set this variable in
    `~/.gnus.el`. Set this variable in `.emacs` instead.

`gnus-default-directory` [¶](#index-gnus_002ddefault_002ddirectory)
:   Not related to the above variable at all—this variable says what the
    default directory of all Gnus buffers should be. If you issue commands
    like `C-x C-f`, the prompt you’ll get starts in the current buffer’s
    default directory. If this variable is `nil` (which is the
    default), the default directory will be the default directory of the
    buffer you were in when you started Gnus.

`gnus-verbose` [¶](#index-gnus_002dverbose)
:   This variable is an integer between zero and ten. The higher the value,
    the more messages will be displayed. If this variable is zero, Gnus
    will never flash any messages, if it is seven (which is the default),
    most important messages will be shown, and if it is ten, Gnus won’t ever
    shut up, but will flash so many messages it will make your head swim.

`gnus-verbose-backends` [¶](#index-gnus_002dverbose_002dbackends)
:   This variable works the same way as `gnus-verbose`, but it applies
    to the Gnus back ends instead of Gnus proper.

`gnus-add-timestamp-to-message` [¶](#index-gnus_002dadd_002dtimestamp_002dto_002dmessage)
:   This variable controls whether to add timestamps to messages that are
    controlled by `gnus-verbose` and `gnus-verbose-backends` and
    are issued. The default value is `nil` which means never to add
    timestamp. If it is `log`, add timestamps to only the messages
    that go into the `*Messages*` buffer. If it is neither `nil` nor
    `log`, add timestamps not only to log messages but also to the ones
    displayed in the echo area.

`nnheader-max-head-length` [¶](#index-nnheader_002dmax_002dhead_002dlength)
:   When the back ends read straight heads of articles, they all try to read
    as little as possible. This variable (default 8192) specifies
    the absolute max length the back ends will try to read before giving up
    on finding a separator line between the head and the body. If this
    variable is `nil`, there is no upper read bound. If it is
    `t`, the back ends won’t try to read the articles piece by piece,
    but read the entire articles. This makes sense with some versions of
    `ange-ftp`.

`nnheader-head-chop-length` [¶](#index-nnheader_002dhead_002dchop_002dlength)
:   This variable (default 2048) says how big a piece of each article to
    read when doing the operation described above.

`nnheader-file-name-translation-alist` [¶](#index-nnheader_002dfile_002dname_002dtranslation_002dalist)
:   This is an alist that says how to translate characters in file names.
    For instance, if ‘`:`’ is invalid as a file character in file names
    on your system (you MS Windows user you), you could say something like:

    ```
    (setq nnheader-file-name-translation-alist
          '((?: . ?_)))
    ```

    In fact, this is (part of) the default value for this variable on MS
    Windows (phooey) systems.

`gnus-hidden-properties` [¶](#index-gnus_002dhidden_002dproperties)
:   This is a list of properties to use to hide “invisible” text. It is
    `(invisible t intangible t)` by default on most systems, which
    makes invisible text invisible and intangible.

`gnus-parse-headers-hook` [¶](#index-gnus_002dparse_002dheaders_002dhook-1)
:   A hook called before parsing headers. It can be used, for instance, to
    gather statistics on the headers fetched, or perhaps you’d like to prune
    some headers. I don’t see why you’d want that, though.

`gnus-shell-command-separator` [¶](#index-gnus_002dshell_002dcommand_002dseparator)
:   String used to separate two shell commands. The default is ‘`;`’.

`gnus-invalid-group-regexp` [¶](#index-gnus_002dinvalid_002dgroup_002dregexp)
:   Regexp to match “invalid” group names when querying user for a group
    name. The default value catches some **really** invalid group
    names who could possibly mess up Gnus internally (like allowing
    ‘`:`’ in a group name, which is normally used to delimit method and
    group).

    IMAP users might want to allow ‘`/`’ in group names though.

`gnus-safe-html-newsgroups` [¶](#index-gnus_002dsafe_002dhtml_002dnewsgroups)
:   Groups in which links in html articles are considered all safe. The
    value may be a regexp matching those groups, a list of group names, or
    `nil`. This overrides `mm-w3m-safe-url-regexp`. The default
    value is `` "\\`nnrss[+:]" ``. This is effective only when emacs-w3m
    renders html articles, i.e., in the case `mm-text-html-renderer` is
    set to `w3m`. See [Display Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Display-Customization) in The Emacs MIME Manual.

---

Next: [Appendices](#Appendices), Previous: [Various](#Various), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 11 The End [¶](#The-End-1)

Well, that’s the manual—you can get on with your life now. Keep in
touch. Say hello to your cats from me.

My **ghod**—I just can’t stand goodbyes. Sniffle.

Ol’ Charles Reznikoff said it pretty well, so I leave the floor to him:

> **Te Deum**
>
>   
>
> Not because of victories   
> I sing,  
> having none,  
> but for the common sunshine,  
> the breeze,  
> the largess of the spring.
>
>   
>
> Not for victory  
> but for the day’s work done  
> as well as I was able;  
> not for a seat upon the dais  
> but at the common table.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [The End](#The-End), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 12 Appendices [¶](#Appendices-1)

* [History](#History)
* [On Writing Manuals](#On-Writing-Manuals)
* [Terminology](#Terminology)
* [Customization](#Customization)
* [Troubleshooting](#Troubleshooting)
* [Gnus Reference Guide](#Gnus-Reference-Guide)
* [Emacs for Heathens](#Emacs-for-Heathens)
* [Frequently Asked Questions](#Frequently-Asked-Questions)

---

Next: [On Writing Manuals](#On-Writing-Manuals), Up: [Appendices](#Appendices)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
