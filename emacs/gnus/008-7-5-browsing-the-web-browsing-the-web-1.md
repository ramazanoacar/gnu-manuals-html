`directory-files`
:   This should be a function with the same interface as
    `directory-files` (such as `directory-files` itself). It is
    used to scan the server’s `directory` for maildirs. This
    parameter is optional; the default is
    `nnheader-directory-files-safe` if
    `nnheader-directory-files-is-safe` is `nil`, and
    `directory-files` otherwise.
    (`nnheader-directory-files-is-safe` is checked only once when the
    server is opened; if you want to check it each time the directory is
    scanned, you’ll have to provide your own function that does that.)

`get-new-mail`
:   If non-`nil`, then after scanning for new mail in the group
    maildirs themselves as usual, this server will also incorporate mail
    the conventional Gnus way, from `mail-sources` according to
    `nnmail-split-methods` or `nnmail-split-fancy`. The default
    value is `nil`.

    Do *not* use the same maildir both in `mail-sources` and as
    an `nnmaildir` group. The results might happen to be useful, but
    that would be by chance, not by design, and the results might be
    different in the future.

---

Next: [Article identification](#Article-Identification), Previous: [Maildir](#Maildir), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.6 Group parameters [¶](#Group-parameters)

`nnmaildir` uses several group parameters. It’s safe to ignore
all this; the default behavior for `nnmaildir` is the same as the
default behavior for other mail back ends: articles are deleted after
one week, etc. Except for the expiry parameters, all this
functionality is unique to `nnmaildir`, so you can ignore it if
you’re just trying to duplicate the behavior you already have with
another back end.

If the value of any of these parameters is a vector, the first element
is evaluated as a Lisp form and the result is used, rather than the
original value. If the value is not a vector, the value itself is
evaluated as a Lisp form. (This is why these parameters use names
different from those of other, similar parameters supported by other
back ends: they have different, though similar, meanings.) (For
numbers, strings, `nil`, and `t`, you can ignore the
`eval` business again; for other values, remember to use an extra
quote and wrap the value in a vector when appropriate.)

`expire-age`
:   An integer specifying the minimum age, in seconds, of an article
    before it will be expired, or the symbol `never` to specify that
    articles should never be expired. If this parameter is not set,
    `nnmaildir` falls back to the usual
    `nnmail-expiry-wait`(`-function`) variables (the
    `expiry-wait` group parameter overrides `nnmail-expiry-wait`
    and makes `nnmail-expiry-wait-function` ineffective). If you
    wanted a value of 3 days, you could use something like `[(* 3 24
    60 60)]`; `nnmaildir` will evaluate the form and use the result.
    An article’s age is measured starting from the article file’s
    modification time. Normally, this is the same as the article’s
    delivery time, but editing an article makes it younger. Moving an
    article (other than via expiry) may also make an article younger.

`expire-group`
:   If this is set to a string such as a full Gnus group name, like

    ```
    "backend+server.address.string:group.name"
    ```

    and if it is not the name of the same group that the parameter belongs
    to, then articles will be moved to the specified group during expiry
    before being deleted. *If this is set to an `nnmaildir`
    group, the article will be just as old in the destination group as it
    was in the source group.* So be careful with `expire-age` in the
    destination group. If this is set to the name of the same group that
    the parameter belongs to, then the article is not expired at all. If
    you use the vector form, the first element is evaluated once for each
    article. So that form can refer to
    `nnmaildir-article-file-name`, etc., to decide where to put the
    article. *Even if this parameter is not set, `nnmaildir`
    does not fall back to the `expiry-target` group parameter or the
    `nnmail-expiry-target` variable.*

`read-only`
:   If this is set to `t`, `nnmaildir` will treat the articles
    in this maildir as read-only. This means: articles are not renamed
    from `new/` into `cur/`; articles are only found in
    `new/`, not `cur/`; articles are never deleted; articles
    cannot be edited. `new/` is expected to be a symlink to the
    `new/` directory of another maildir—e.g., a system-wide mailbox
    containing a mailing list of common interest. Everything in the
    maildir outside `new/` is *not* treated as read-only, so for
    a shared mailbox, you do still need to set up your own maildir (or
    have write permission to the shared mailbox); your maildir just won’t
    contain extra copies of the articles.

`directory-files`
:   A function with the same interface as `directory-files`. It is
    used to scan the directories in the maildir corresponding to this
    group to find articles. The default is the function specified by the
    server’s `directory-files` parameter.

`distrust-Lines:`
:   If non-`nil`, `nnmaildir` will always count the lines of an
    article, rather than use the `Lines:` header field. If
    `nil`, the header field will be used if present.

`always-marks`
:   A list of mark symbols, such as `['(read expire)]`. Whenever
    Gnus asks `nnmaildir` for article marks, `nnmaildir` will
    say that all articles have these marks, regardless of whether the
    marks stored in the filesystem say so. This is a proof-of-concept
    feature that will probably be removed eventually; it ought to be done
    in Gnus proper, or abandoned if it’s not worthwhile.

`never-marks`
:   A list of mark symbols, such as `['(tick expire)]`. Whenever
    Gnus asks `nnmaildir` for article marks, `nnmaildir` will
    say that no articles have these marks, regardless of whether the marks
    stored in the filesystem say so. `never-marks` overrides
    `always-marks`. This is a proof-of-concept feature that will
    probably be removed eventually; it ought to be done in Gnus proper, or
    abandoned if it’s not worthwhile.

`nov-cache-size`
:   An integer specifying the size of the NOV memory cache. To
    speed things up, `nnmaildir` keeps NOV data in memory
    for a limited number of articles in each group. (This is probably not
    worthwhile, and will probably be removed in the future.) This
    parameter’s value is noticed only the first time a group is seen after
    the server is opened—i.e., when you first start Gnus, typically.
    The NOV cache is never resized until the server is closed
    and reopened. The default is an estimate of the number of articles
    that would be displayed in the summary buffer: a count of articles
    that are either marked with `tick` or not marked with
    `read`, plus a little extra.

---

Next: [NOV data](#NOV-Data), Previous: [Group parameters](#nnmaildir-Group-Parameters), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.7 Article identification [¶](#Article-identification)

Articles are stored in the `cur/` subdirectory of each maildir.
Each article file is named like `uniq:info`, where `uniq`
contains no colons. `nnmaildir` ignores, but preserves, the
`:info` part. (Other maildir readers typically use this part of
the filename to store marks.) The `uniq` part uniquely
identifies the article, and is used in various places in the
`.nnmaildir/` subdirectory of the maildir to store information
about the corresponding article. The full pathname of an article is
available in the variable `nnmaildir-article-file-name` after you
request the article in the summary buffer.

---

Next: [Article marks](#Article-Marks), Previous: [Article identification](#Article-Identification), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.8 NOV data [¶](#NOV-data)

An article identified by `uniq` has its NOV data (used
to generate lines in the summary buffer) stored in
`.nnmaildir/nov/uniq`. There is no
`nnmaildir-generate-nov-databases` function. (There isn’t much
need for it—an article’s NOV data is updated automatically
when the article or `nnmail-extra-headers` has changed.) You can
force `nnmaildir` to regenerate the NOV data for a
single article simply by deleting the corresponding NOV
file, but *beware*: this will also cause `nnmaildir` to
assign a new article number for this article, which may cause trouble
with `seen` marks, the Agent, and the cache.

---

Next: [Mail Folders](#Mail-Folders), Previous: [NOV data](#NOV-Data), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.9 Article marks [¶](#Article-marks)

An article identified by `uniq` is considered to have the mark
`flag` when the file `.nnmaildir/marks/flag/uniq` exists.
When Gnus asks `nnmaildir` for a group’s marks, `nnmaildir`
looks for such files and reports the set of marks it finds. When Gnus
asks `nnmaildir` to store a new set of marks, `nnmaildir`
creates and deletes the corresponding files as needed. (Actually,
rather than create a new file for each mark, it just creates hard
links to `.nnmaildir/markfile`, to save inodes.)

You can invent new marks by creating a new directory in
`.nnmaildir/marks/`. You can tar up a maildir and remove it from
your server, untar it later, and keep your marks. You can add and
remove marks yourself by creating and deleting mark files. If you do
this while Gnus is running and your `nnmaildir` server is open,
it’s best to exit all summary buffers for `nnmaildir` groups and
type `s` in the group buffer first, and to type `g` or
`M-g` in the group buffer afterwards. Otherwise, Gnus might not
pick up the changes, and might undo them.

---

Next: [Comparing Mail Back Ends](#Comparing-Mail-Back-Ends), Previous: [Article marks](#Article-Marks), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.10 Mail Folders [¶](#Mail-Folders-1)

`nnfolder` is a back end for storing each mail group in a
separate file. Each file is in the standard Un\*x mbox format.
`nnfolder` will add extra headers to keep track of article
numbers and arrival dates.

Virtual server settings:

`nnfolder-directory` [¶](#index-nnfolder_002ddirectory)
:   All the `nnfolder` mail boxes will be stored under this
    directory. The default is the value of `message-directory`
    (whose default is `~/Mail`)

`nnfolder-active-file` [¶](#index-nnfolder_002dactive_002dfile)
:   The name of the active file. The default is `~/Mail/active`.

`nnfolder-newsgroups-file` [¶](#index-nnfolder_002dnewsgroups_002dfile)
:   The name of the group descriptions file. See [Newsgroups File Format](#Newsgroups-File-Format). The default is `~/Mail/newsgroups`

`nnfolder-get-new-mail` [¶](#index-nnfolder_002dget_002dnew_002dmail-1)
:   If non-`nil`, `nnfolder` will read incoming mail. The
    default is `t`

`nnfolder-save-buffer-hook` [¶](#index-nnfolder_002dsave_002dbuffer_002dhook)
:   Hook run before saving the folders. Note that Emacs does the normal
    backup renaming of files even with the `nnfolder` buffers. If
    you wish to switch this off, you could say something like the
    following in your `.emacs` file:

    ```
    (defun turn-off-backup ()
      (set (make-local-variable 'backup-inhibited) t))

    (add-hook 'nnfolder-save-buffer-hook 'turn-off-backup)
    ```

`nnfolder-delete-mail-hook` [¶](#index-nnfolder_002ddelete_002dmail_002dhook)
:   Hook run in a buffer narrowed to the message that is to be deleted.
    This function can be used to copy the message to somewhere else, or to
    extract some information from it before removing it.

`nnfolder-nov-is-evil` [¶](#index-nnfolder_002dnov_002dis_002devil)
:   If non-`nil`, this back end will ignore any NOV files. The
    default is `nil`.

`nnfolder-nov-file-suffix` [¶](#index-nnfolder_002dnov_002dfile_002dsuffix)
:   The extension for NOV files. The default is `.nov`.

`nnfolder-nov-directory` [¶](#index-nnfolder_002dnov_002ddirectory)
:   The directory where the NOV files should be stored. If
    `nil`, `nnfolder-directory` is used.

If you have lots of `nnfolder`-like files you’d like to read with
`nnfolder`, you can use the `M-x nnfolder-generate-active-file`
command to make `nnfolder` aware of all likely files in
`nnfolder-directory`. This only works if you use long file names,
though.

---

Previous: [Mail Folders](#Mail-Folders), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.11 Comparing Mail Back Ends [¶](#Comparing-Mail-Back-Ends-1)

First, just for terminology, the *back end* is the common word for a
low-level access method—a transport, if you will, by which something
is acquired. The sense is that one’s mail has to come from somewhere,
and so selection of a suitable back end is required in order to get that
mail within spitting distance of Gnus.

The same concept exists for Usenet itself: Though access to articles is
typically done by NNTP these days, once upon a midnight dreary, everyone
in the world got at Usenet by running a reader on the machine where the
articles lay (the machine which today we call an NNTP server), and
access was by the reader stepping into the articles’ directory spool
area directly. One can still select between either the `nntp` or
`nnspool` back ends, to select between these methods, if one happens
actually to live on the server (or can see its spool directly, anyway,
via NFS).

The goal in selecting a mail back end is to pick one which
simultaneously represents a suitable way of dealing with the original
format plus leaving mail in a form that is convenient to use in the
future. Here are some high and low points on each:

`nnmbox`
:   UNIX systems have historically had a single, very common, and well-defined
    format. All messages arrive in a single *spool file*, and
    they are delineated by a line whose regular expression matches
    ‘`^From_`’. (My notational use of ‘`_`’ is to indicate a space,
    to make it clear in this instance that this is not the RFC-specified
    ‘`From:`’ header.) Because Emacs and therefore Gnus emanate
    historically from the Unix environment, it is simplest if one does not
    mess a great deal with the original mailbox format, so if one chooses
    this back end, Gnus’ primary activity in getting mail from the real spool
    area to Gnus’ preferred directory is simply to copy it, with no
    (appreciable) format change in the process. It is the “dumbest” way
    to move mail into availability in the Gnus environment. This makes it
    fast to move into place, but slow to parse, when Gnus has to look at
    what’s where.

`nnbabyl`
:   Once upon a time, there was the DEC-10 and DEC-20, running operating
    systems called TOPS and related things, and the usual (only?) mail
    reading environment was a thing called Babyl. I don’t know what format
    was used for mail landing on the system, but Babyl had its own internal
    format to which mail was converted, primarily involving creating a
    spool-file-like entity with a scheme for inserting Babyl-specific
    headers and status bits above the top of each message in the file.
    Rmail was Emacs’s first mail reader, it was written by Richard Stallman,
    and Stallman came out of that TOPS/Babyl environment, so he wrote Rmail
    to understand the mail files folks already had in existence. Gnus (and
    VM, for that matter) continue to support this format because it’s
    perceived as having some good qualities in those mailer-specific
    headers/status bits stuff. Rmail itself still exists as well, of
    course, and is still maintained within Emacs. Since Emacs 23, it
    uses standard mbox format rather than Babyl.

    Both of the above forms leave your mail in a single file on your
    file system, and they must parse that entire file each time you take a
    look at your mail.

`nnml`
:   `nnml` is the back end which smells the most as though you were
    actually operating with an `nnspool`-accessed Usenet system. (In
    fact, I believe `nnml` actually derived from `nnspool` code,
    lo these years ago.) One’s mail is taken from the original spool file,
    and is then cut up into individual message files, 1:1. It maintains a
    Usenet-style active file (analogous to what one finds in an INN- or
    CNews-based news system in (for instance) `/var/lib/news/active`,
    or what is returned via the ‘`NNTP LIST`’ verb) and also creates
    *overview* files for efficient group entry, as has been defined for
    NNTP servers for some years now. It is slower in mail-splitting,
    due to the creation of lots of files, updates to the `nnml` active
    file, and additions to overview files on a per-message basis, but it is
    extremely fast on access because of what amounts to the indexing support
    provided by the active file and overviews.

    `nnml` costs *inodes* in a big way; that is, it soaks up the
    resource which defines available places in the file system to put new
    files. Sysadmins take a dim view of heavy inode occupation within
    tight, shared file systems. But if you live on a personal machine where
    the file system is your own and space is not at a premium, `nnml`
    wins big.

    It is also problematic using this back end if you are living in a
    FAT16-based Windows world, since much space will be wasted on all these
    tiny files.

`nnmh`
:   The Rand MH mail-reading system has been around UNIX systems for a very
    long time; it operates by splitting one’s spool file of messages into
    individual files, but with little or no indexing support—`nnmh`
    is considered to be semantically equivalent to “`nnml` without
    active file or overviews”. This is arguably the worst choice, because
    one gets the slowness of individual file creation married to the
    slowness of access parsing when learning what’s new in one’s groups.

`nnfolder`
:   Basically the effect of `nnfolder` is `nnmbox` (the first
    method described above) on a per-group basis. That is, `nnmbox`
    itself puts *all* one’s mail in one file; `nnfolder` provides a
    little bit of optimization to this so that each of one’s mail groups has
    a Unix mail box file. It’s faster than `nnmbox` because each group
    can be parsed separately, and still provides the simple Unix mail box
    format requiring minimal effort in moving the mail around. In addition,
    it maintains an “active” file making it much faster for Gnus to figure
    out how many messages there are in each separate group.

    If you have groups that are expected to have a massive amount of
    messages, `nnfolder` is not the best choice, but if you receive
    only a moderate amount of mail, `nnfolder` is probably the most
    friendly mail back end all over.

`nnmaildir`
:   For configuring expiry and other things, `nnmaildir` uses
    incompatible group parameters, slightly different from those of other
    mail back ends.

    `nnmaildir` is largely similar to `nnml`, with some notable
    differences. Each message is stored in a separate file, but the
    filename is unrelated to the article number in Gnus. `nnmaildir`
    also stores the equivalent of `nnml`’s overview files in one file
    per article, so it uses about twice as many inodes as `nnml`.
    (Use `df -i` to see how plentiful your inode supply is.) If this
    slows you down or takes up very much space, use a non-block-structured
    file system.

    Since maildirs don’t require locking for delivery, the maildirs you use
    as groups can also be the maildirs your mail is directly delivered to.
    This means you can skip Gnus’ mail splitting if your mail is already
    organized into different mailboxes during delivery. A `directory`
    entry in `mail-sources` would have a similar effect, but would
    require one set of mailboxes for spooling deliveries (in mbox format,
    thus damaging message bodies), and another set to be used as groups (in
    whatever format you like). A maildir has a built-in spool, in the
    `new/` subdirectory. Beware that currently, mail moved from
    `new/` to `cur/` instead of via mail splitting will not
    undergo treatment such as duplicate checking.

    `nnmaildir` stores article marks for a given group in the
    corresponding maildir, in a way designed so that it’s easy to manipulate
    them from outside Gnus. You can tar up a maildir, unpack it somewhere
    else, and still have your marks.

    `nnmaildir` uses a significant amount of memory to speed things up.
    (It keeps in memory some of the things that `nnml` stores in files
    and that `nnmh` repeatedly parses out of message files.) If this
    is a problem for you, you can set the `nov-cache-size` group
    parameter to something small (0 would probably not work, but 1 probably
    would) to make it use less memory. This caching will probably be
    removed in the future.

    Startup is likely to be slower with `nnmaildir` than with other
    back ends. Everything else is likely to be faster, depending in part
    on your file system.

    `nnmaildir` does not use `nnoo`, so you cannot use `nnoo`
    to write an `nnmaildir`-derived back end.

---

Next: [Other Sources](#Other-Sources), Previous: [Getting Mail](#Getting-Mail), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.5 Browsing the Web [¶](#Browsing-the-Web-1)

Web-based discussion forums are getting more and more popular. On many
subjects, the web-based forums have become the most important forums,
eclipsing the importance of mailing lists and news groups. The reason
is easy to understand—they are friendly to new users; you just point
and click, and there’s the discussion. With mailing lists, you have to
go through a cumbersome subscription procedure, and most people don’t
even know what a news group is.

The problem with this scenario is that web browsers are not very good at
being newsreaders. They do not keep track of what articles you’ve read;
they do not allow you to score on subjects you’re interested in; they do
not allow off-line browsing; they require you to click around and drive
you mad in the end.

So—if web browsers suck at reading discussion forums, why not use Gnus
to do it instead?

Gnus has been getting a bit of a collection of back ends for providing
interfaces to these sources.

The main caveat with all these web sources is that they probably won’t
work for a very long time. Gleaning information from the HTML data
is guesswork at best, and when the layout is altered, the Gnus back end
will fail. If you have reasonably new versions of these back ends,
though, you should be ok.

One thing all these Web methods have in common is that the Web sources
are often down, unavailable or just plain too slow to be fun. In those
cases, it makes a lot of sense to let the Gnus Agent (see [Gnus Unplugged](#Gnus-Unplugged)) handle downloading articles, and then you can read them at
leisure from your local disk. No more World Wide Wait for you.

* [Web Searches](#Web-Searches)
* [RSS](#RSS)
* [Atom](#Atom)

---

Next: [RSS](#RSS), Up: [Browsing the Web](#Browsing-the-Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.5.1 Web Searches [¶](#Web-Searches-1)

It’s, like, too neat to search the Usenet for articles that match a
string, but it, like, totally *sucks*, like, totally, to use one of
those, like, Web browsers, and you, like, have to, rilly, like, look at
the commercials, so, like, with Gnus you can do *rad*, rilly,
searches without having to use a browser.

The `nnweb` back end allows an easy interface to the mighty search
engine. You create an `nnweb` group, enter a search pattern, and
then enter the group and read the articles like you would any normal
group. The `G w` command in the group buffer (see [Foreign Groups](#Foreign-Groups)) will do this in an easy-to-use fashion.

`nnweb` groups don’t really lend themselves to being solid
groups—they have a very fleeting idea of article numbers. In fact,
each time you enter an `nnweb` group (not even changing the search
pattern), you are likely to get the articles ordered in a different
manner. Not even using duplicate suppression (see [Duplicate Suppression](#Duplicate-Suppression)) will help, since `nnweb` doesn’t even know the
`Message-ID` of the articles before reading them using some search
engines (Google, for instance). The only possible way to keep track
of which articles you’ve read is by scoring on the `Date`
header—mark all articles posted before the last date you read the
group as read.

If the search engine changes its output substantially, `nnweb`
won’t be able to parse it and will fail. One could hardly fault the Web
providers if they were to do this—their *raison d’être* is to
make money off of advertisements, not to provide services to the
community. Since `nnweb` washes the ads off all the articles, one
might think that the providers might be somewhat miffed. We’ll see.

Virtual server variables:

`nnweb-type` [¶](#index-nnweb_002dtype)
:   What search engine type is being used. The currently supported types
    are `google` and `dejanews`. Note that
    `dejanews` is an alias to `google`.

`nnweb-search` [¶](#index-nnweb_002dsearch)
:   The search string to feed to the search engine.

`nnweb-max-hits` [¶](#index-nnweb_002dmax_002dhits)
:   Advisory maximum number of hits per search to display. The default is
    999.

`nnweb-type-definition` [¶](#index-nnweb_002dtype_002ddefinition)
:   Type-to-definition alist. This alist says what `nnweb` should do
    with the various search engine types. The following elements must be
    present:

    `article`
    :   Function to decode the article and provide something that Gnus
        understands.

    `map`
    :   Function to create an article number to message header and URL alist.

    `search`
    :   Function to send the search string to the search engine.

    `address`
    :   The address the aforementioned function should send the search string
        to.

    `id`
    :   Format string URL to fetch an article by `Message-ID`.

---

Next: [Atom](#Atom), Previous: [Web Searches](#Web-Searches), Up: [Browsing the Web](#Browsing-the-Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.5.2 RSS [¶](#RSS-1)

Some web sites have an RDF Site Summary (RSS).
RSS is a format for summarizing headlines from news related
sites (such as BBC or CNN). But basically anything list-like can be
presented as an RSS feed: weblogs, changelogs or recent
changes to a wiki (e.g., <https://cliki.net/site/recent-changes>).

RSS has a quite regular and nice interface, and it’s
possible to get the information Gnus needs to keep groups updated.

Use `G R` from the group buffer to subscribe to a feed—you will be
prompted for the location, the title and the description of the feed.
The title, which allows any characters, will be used for the group name
and the name of the group data file. The description can be omitted.

An easy way to get started with `nnrss` is to say something like
the following in the group buffer: `B nnrss RET RET y`, then
subscribe to groups.

The `nnrss` back end saves the group data file in
`nnrss-directory` (see below) for each `nnrss` group. File
names containing non-ASCII characters will be encoded by the
coding system specified with the `nnmail-pathname-coding-system`
variable or other. Also See [Accessing groups of non-English names](#Non_002dASCII-Group-Names), for more
information.

The `nnrss` back end generates ‘`multipart/alternative`’
MIME articles in which each contains a ‘`text/plain`’ part
and a ‘`text/html`’ part.

You can also use the following commands to import and export your
subscriptions from a file in OPML format (Outline Processor
Markup Language).

Function: **nnrss-opml-import** file [¶](#index-nnrss_002dopml_002dimport)
:   Prompt for an OPML file, and subscribe to each feed in the
    file.

Function: **nnrss-opml-export** [¶](#index-nnrss_002dopml_002dexport)
:   Write your current RSS subscriptions to a buffer in
    OPML format.

The following `nnrss` variables can be altered:

`nnrss-directory` [¶](#index-nnrss_002ddirectory)
:   The directory where `nnrss` stores its files. The default is
    `~/News/rss/`.

`nnrss-file-coding-system` [¶](#index-nnrss_002dfile_002dcoding_002dsystem)
:   The coding system used when reading and writing the `nnrss` groups
    data files. The default is the value of
    `mm-universal-coding-system` (which defaults to `utf-8-emacs`).

`nnrss-ignore-article-fields` [¶](#index-nnrss_002dignore_002darticle_002dfields)
:   Some feeds update constantly article fields during their publications,
    e.g., to indicate the number of comments. However, if there is
    a difference between the local article and the distant one, the latter
    is considered to be new. To avoid this and discard some fields, set this
    variable to the list of fields to be ignored. The default is
    `'(slash:comments)`.

`nnrss-use-local` [¶](#index-nnrss_002duse_002dlocal)
:   If you set `nnrss-use-local` to `t`, `nnrss` will read
    the feeds from local files in `nnrss-directory`. You can use
    the command `nnrss-generate-download-script` to generate a
    download script using `wget`.

The following code may be helpful, if you want to show the description in
the summary buffer.

```
(add-to-list 'nnmail-extra-headers nnrss-description-field)
(setq gnus-summary-line-format "%U%R%z%I%(%[%4L: %-15,15f%]%) %s%uX\n")

(defun gnus-user-format-function-X (header)
  (let ((descr
         (assq nnrss-description-field (mail-header-extra header))))
    (if descr (concat "\n\t" (cdr descr)) "")))
```

The following code may be useful to open an nnrss url directly from the
summary buffer.

```
(require 'browse-url)

(defun browse-nnrss-url (arg)
  (interactive "p")
  (let ((url (assq nnrss-url-field
                   (mail-header-extra
                    (gnus-data-header
                     (assq (gnus-summary-article-number)
                           gnus-newsgroup-data))))))
    (if url
        (progn
          (browse-url (cdr url))
          (gnus-summary-mark-as-read-forward 1))
      (gnus-summary-scroll-up arg))))

(with-eval-after-load "gnus"
  (keymap-set gnus-summary-mode-map "RET" 'browse-nnrss-url))
(add-to-list 'nnmail-extra-headers nnrss-url-field)
```

Even if you have added ‘`text/html`’ to the
`mm-discouraged-alternatives` variable (see [Display Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Display-Customization) in The Emacs MIME
Manual) since you don’t want to see HTML parts, it might be
more useful especially in `nnrss` groups to display
‘`text/html`’ parts. Here’s an example of setting
`mm-discouraged-alternatives` as a group parameter (see [Group Parameters](#Group-Parameters)) in order to display ‘`text/html`’ parts only in
`nnrss` groups:

```
;; Set the default value of mm-discouraged-alternatives.
(with-eval-after-load "gnus-sum"
  (add-to-list
   'gnus-newsgroup-variables
   '(mm-discouraged-alternatives
     . '("text/html" "image/.*"))))

;; Display ‘text/html’ parts in nnrss groups.
(add-to-list
 'gnus-parameters
 '("\\`nnrss:" (mm-discouraged-alternatives nil)))
```

---

Previous: [RSS](#RSS), Up: [Browsing the Web](#Browsing-the-Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.5.3 Atom [¶](#Atom-1)

Some web sites provide an Atom Syndication Format feed. Atom is a web
feed format similar in function to RDF Site Summary (see [RSS](#RSS)).

The `nnatom` back end allows you to add HTTP or local Atom feeds as
Gnus servers, by adding them to `gnus-secondary-select-methods` or
as foreign servers by pressing "B" in the `*Group*` buffer, for
example (see [Finding the News](#Finding-the-News)). The address of each server is its
feed’s location (though the address shouldn’t be prefixed with <http://> or
<https://>) and each server contains a single group which holds the
feed’s entries.

Features of `nnatom` include:

* Server data is saved per-server in the `atom` sub-directory of
  `gnus-directory`.
* An article part is generated for both the summary and the content for
  each entry in the feed. Content of all MIME types should be displayed
  correctly through Gnus (as long as they are supported and the feed
  specifies a MIME type).
* Article modification and publish dates are tracked, and articles are
  updated if changed.

---

Next: [Virtual Groups](#Virtual-Groups), Previous: [Browsing the Web](#Browsing-the-Web), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.6 Other Sources [¶](#Other-Sources-1)

Gnus can do more than just read news or mail. The methods described
below allow Gnus to view directories and files as if they were
newsgroups.

* [Directory Groups](#Directory-Groups)
* [Anything Groups](#Anything-Groups)
* [Document Groups](#Document-Groups)
* [Mail-To-News Gateways](#Mail_002dTo_002dNews-Gateways)
* [The Empty Backend](#The-Empty-Backend)

---

Next: [Anything Groups](#Anything-Groups), Up: [Other Sources](#Other-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.6.1 Directory Groups [¶](#Directory-Groups-1)

If you have a directory that has lots of articles in separate files in
it, you might treat it as a newsgroup. The files have to have numerical
names, of course.

This might be an opportune moment to mention `ange-ftp`, that
most wonderful of all wonderful Emacs packages. When I wrote
`nndir`, I didn’t think much about it—a back end to read
directories. Big deal.

`ange-ftp` changes that picture dramatically. For instance, if you
enter the `ange-ftp` file name
`/ftp.hpc.uh.edu:/pub/emacs/ding-list/` as the directory name,
`ange-ftp` will actually allow you to read this directory over at
‘`sina`’ as a newsgroup. Distributed news ahoy!

`nndir` will use NOV files if they are present.

`nndir` is a “read-only” back end—you can’t delete or expire
articles with this method. You can use `nnmh` or `nnml` for
whatever you use `nndir` for, so you could switch to any of those
methods if you feel the need to have a non-read-only `nndir`.

---

Next: [Document Groups](#Document-Groups), Previous: [Directory Groups](#Directory-Groups), Up: [Other Sources](#Other-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.6.2 Anything Groups [¶](#Anything-Groups-1)

From the `nndir` back end (which reads a single spool-like
directory), it’s just a hop and a skip to `nneething`, which
pretends that any arbitrary directory is a newsgroup. Strange, but
true.

When `nneething` is presented with a directory, it will scan this
directory and assign article numbers to each file. When you enter such
a group, `nneething` must create “headers” that Gnus can use.
After all, Gnus is a newsreader, in case you’re forgetting.
`nneething` does this in a two-step process. First, it snoops each
file in question. If the file looks like an article (i.e., the first
few lines look like headers), it will use this as the head. If this is
just some arbitrary file without a head (e.g., a C source file),
`nneething` will cobble up a header out of thin air. It will use
file ownership, name and date and do whatever it can with these
elements.

All this should happen automatically for you, and you will be presented
with something that looks very much like a newsgroup. Totally like a
newsgroup, to be precise. If you select an article, it will be displayed
in the article buffer, just as usual.

If you select a line that represents a directory, Gnus will pop you into
a new summary buffer for this `nneething` group. And so on. You can
traverse the entire disk this way, if you feel like, but remember that
Gnus is not dired, really, and does not intend to be, either.

There are two overall modes to this action—ephemeral or solid. When
doing the ephemeral thing (i.e., `G D` from the group buffer), Gnus
will not store information on what files you have read, and what files
are new, and so on. If you create a solid `nneething` group the
normal way with `G m`, Gnus will store a mapping table between
article numbers and file names, and you can treat this group like any
other groups. When you activate a solid `nneething` group, you will
be told how many unread articles it contains, etc., etc.

Some variables:

`nneething-map-file-directory` [¶](#index-nneething_002dmap_002dfile_002ddirectory)
:   All the mapping files for solid `nneething` groups will be stored
    in this directory, which defaults to `~/.nneething/`.

`nneething-exclude-files` [¶](#index-nneething_002dexclude_002dfiles)
:   All files that match this regexp will be ignored. Nice to use to exclude
    auto-save files and the like, which is what it does by default.

`nneething-include-files` [¶](#index-nneething_002dinclude_002dfiles)
:   Regexp saying what files to include in the group. If this variable is
    non-`nil`, only files matching this regexp will be included.

`nneething-map-file` [¶](#index-nneething_002dmap_002dfile)
:   Name of the map files.

---

Next: [Mail-To-News Gateways](#Mail_002dTo_002dNews-Gateways), Previous: [Anything Groups](#Anything-Groups), Up: [Other Sources](#Other-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.6.3 Document Groups [¶](#Document-Groups-1)

`nndoc` is a cute little thing that will let you read a single file
as a newsgroup. Several files types are supported:

`babyl` [¶](#index-Babyl)
:   The Babyl format.

`mbox` [¶](#index-mbox)
:   The standard Unix mbox file.

`mmdf` [¶](#index-MMDF-mail-box)
:   The MMDF mail box format.

`news`
:   Several news articles appended into a file.

`rnews` [¶](#index-rnews-batch-files)
:   The rnews batch transport format.

`nsmail`
:   Netscape mail boxes.

`mime-parts`
:   MIME multipart messages.

`standard-digest`
:   The standard (RFC 1153) digest format.

`mime-digest`
:   A MIME digest of messages.

`lanl-gov-announce`
:   Announcement messages from LANL Gov Announce.

`git` [¶](#index-git-commit-messages)
:   `git` commit messages.

`rfc822-forward` [¶](#index-forwarded-messages)
:   A message forwarded according to RFC 822 or its successors.

`outlook`
:   The Outlook mail box.

`oe-dbx`
:   The Outlook Express dbx mail box.

`exim-bounce`
:   A bounce message from the Exim MTA.

`forward`
:   A message forwarded according to informal rules.

`rfc934`
:   An RFC934-forwarded message.

`mailman`
:   A mailman digest.

`clari-briefs`
:   A digest of Clarinet brief news items.

`slack-digest`
:   Non-standard digest format—matches most things, but does it badly.

`mail-in-mail`
:   The last resort.

You can also use the special “file type” `guess`, which means
that `nndoc` will try to guess what file type it is looking at.
`digest` means that `nndoc` should guess what digest type the
file is.

`nndoc` will not try to change the file or insert any extra headers into
it—it will simply, like, let you use the file as the basis for a
group. And that’s it.

If you have some old archived articles that you want to insert into your
new & spiffy Gnus mail back end, `nndoc` can probably help you with
that. Say you have an old `RMAIL` file with mail that you now want
to split into your new `nnml` groups. You look at that file using
`nndoc` (using the `G f` command in the group buffer
(see [Foreign Groups](#Foreign-Groups))), set the process mark on all the articles in
the buffer (`M P b`, for instance), and then re-spool (`B r`)
using `nnml`. If all goes well, all the mail in the `RMAIL`
file is now also stored in lots of `nnml` directories, and you can
delete that pesky `RMAIL` file. If you have the guts!

Virtual server variables:

`nndoc-article-type` [¶](#index-nndoc_002darticle_002dtype)
:   This should be one of `mbox`, `babyl`, `digest`,
    `news`, `rnews`, `mmdf`, `forward`, `rfc934`,
    `rfc822-forward`, `mime-parts`, `standard-digest`,
    `slack-digest`, `clari-briefs`, `nsmail`, `outlook`,
    `oe-dbx`, `mailman`, and `mail-in-mail` or `guess`.

`nndoc-post-type` [¶](#index-nndoc_002dpost_002dtype)
:   This variable says whether Gnus is to consider the group a news group or
    a mail group. There are two valid values: `mail` (the default)
    and `news`.

* [Document Server Internals](#Document-Server-Internals)

---

Up: [Document Groups](#Document-Groups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.6.3.1 Document Server Internals [¶](#Document-Server-Internals-1)

Adding new document types to be recognized by `nndoc` isn’t
difficult. You just have to whip up a definition of what the document
looks like, write a predicate function to recognize that document type,
and then hook into `nndoc`.

First, here’s an example document type definition:

```
(mmdf
 (article-begin .  "^\^A\^A\^A\^A\n")
 (body-end .  "^\^A\^A\^A\^A\n"))
```

The definition is simply a unique *name* followed by a series of
regexp pseudo-variable settings. Below are the possible
variables—don’t be daunted by the number of variables; most document
types can be defined with very few settings:

`first-article`
:   If present, `nndoc` will skip past all text until it finds
    something that match this regexp. All text before this will be
    totally ignored.

`article-begin`
:   This setting has to be present in all document type definitions. It
    says what the beginning of each article looks like. To do more
    complicated things that cannot be dealt with a simple regexp, you can
    use `article-begin-function` instead of this.

`article-begin-function`
:   If present, this should be a function that moves point to the beginning
    of each article. This setting overrides `article-begin`.

`head-begin`
:   If present, this should be a regexp that matches the head of the
    article. To do more complicated things that cannot be dealt with a
    simple regexp, you can use `head-begin-function` instead of this.

`head-begin-function`
:   If present, this should be a function that moves point to the head of
    the article. This setting overrides `head-begin`.

`head-end`
:   This should match the end of the head of the article. It defaults to
    ‘`^$`’—the empty line.

`body-begin`
:   This should match the beginning of the body of the article. It defaults
    to ‘`^\n`’. To do more complicated things that cannot be dealt with
    a simple regexp, you can use `body-begin-function` instead of this.

`body-begin-function`
:   If present, this function should move point to the beginning of the body
    of the article. This setting overrides `body-begin`.

`body-end`
:   If present, this should match the end of the body of the article. To do
    more complicated things that cannot be dealt with a simple regexp, you
    can use `body-end-function` instead of this.

`body-end-function`
:   If present, this function should move point to the end of the body of
    the article. This setting overrides `body-end`.

`file-begin`
:   If present, this should match the beginning of the file. All text
    before this regexp will be totally ignored.

`file-end`
:   If present, this should match the end of the file. All text after this
    regexp will be totally ignored.

So, using these variables `nndoc` is able to dissect a document
file into a series of articles, each with a head and a body. However, a
few more variables are needed since not all document types are all that
news-like—variables needed to transform the head or the body into
something that’s palatable for Gnus:

`prepare-body-function`
:   If present, this function will be called when requesting an article. It
    will be called with point at the start of the body, and is useful if the
    document has encoded some parts of its contents.

`article-transform-function`
:   If present, this function is called when requesting an article. It’s
    meant to be used for more wide-ranging transformation of both head and
    body of the article.

`generate-head-function`
:   If present, this function is called to generate a head that Gnus can
    understand. It is called with the article number as a parameter, and is
    expected to generate a nice head for the article in question. It is
    called when requesting the headers of all articles.

`generate-article-function`
:   If present, this function is called to generate an entire article that
    Gnus can understand. It is called with the article number as a
    parameter when requesting all articles.

`dissection-function`
:   If present, this function is called to dissect a document by itself,
    overriding `first-article`, `article-begin`,
    `article-begin-function`, `head-begin`,
    `head-begin-function`, `head-end`, `body-begin`,
    `body-begin-function`, `body-end`, `body-end-function`,
    `file-begin`, and `file-end`.

Let’s look at the most complicated example I can come up with—standard
digests:

```
(standard-digest
 (first-article . ,(concat "^" (make-string 70 ?-) "\n\n+"))
 (article-begin . ,(concat "\n\n" (make-string 30 ?-) "\n\n+"))
 (prepare-body-function . nndoc-unquote-dashes)
 (body-end-function . nndoc-digest-body-end)
 (head-end . "^ ?$")
 (body-begin . "^ ?\n")
 (file-end . "^End of .*digest.*[0-9].*\n\\*\\*\\|^End of.*Digest *$")
 (subtype digest guess))
```

We see that all text before a 70-width line of dashes is ignored; all
text after a line that starts with that ‘`^End of`’ is also ignored;
each article begins with a 30-width line of dashes; the line separating
the head from the body may contain a single space; and that the body is
run through `nndoc-unquote-dashes` before being delivered.

To hook your own document definition into `nndoc`, use the
`nndoc-add-type` function. It takes two parameters—the first
is the definition itself and the second (optional) parameter says
where in the document type definition alist to put this definition.
The alist is traversed sequentially, and
`nndoc-type-type-p` is called for a given type type.
So `nndoc-mmdf-type-p` is called to see whether a document is of
`mmdf` type, and so on. These type predicates should return
`nil` if the document is not of the correct type; `t` if it
is of the correct type; and a number if the document might be of the
correct type. A high number means high probability; a low number
means low probability with ‘`0`’ being the lowest valid number.

---

Next: [The Empty Backend](#The-Empty-Backend), Previous: [Document Groups](#Document-Groups), Up: [Other Sources](#Other-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.6.4 Mail-To-News Gateways [¶](#Mail_002dTo_002dNews-Gateways-1)

If your local `nntp` server doesn’t allow posting, for some reason
or other, you can post using one of the numerous mail-to-news gateways.
The `nngateway` back end provides the interface.

Note that you can’t read anything from this back end—it can only be
used to post with.

Server variables:

`nngateway-address` [¶](#index-nngateway_002daddress)
:   This is the address of the mail-to-news gateway.

`nngateway-header-transformation` [¶](#index-nngateway_002dheader_002dtransformation)
:   News headers often have to be transformed in some odd way or other
    for the mail-to-news gateway to accept it. This variable says what
    transformation should be called, and defaults to
    `nngateway-simple-header-transformation`. The function is called
    narrowed to the headers to be transformed and with one parameter—the
    gateway address.

    This default function just inserts a new `To` header based on the
    `Newsgroups` header and the gateway address.
    For instance, an article with this `Newsgroups` header:

    ```
    Newsgroups: alt.religion.emacs
    ```

    will get this `To` header inserted:

    ```
    To: alt-religion-emacs@GATEWAY
    ```

    The following pre-defined functions exist:

    `nngateway-simple-header-transformation`
    :   Creates a `To` header that looks like
        newsgroup@`nngateway-address`.

    `nngateway-mail2news-header-transformation`
    :   Creates a `To` header that looks like
        `nngateway-address`.

Here’s an example:

```
(setq gnus-post-method
      '(nngateway
        "mail2news@replay.com"
        (nngateway-header-transformation
         nngateway-mail2news-header-transformation)))
```

So, to use this, simply say something like:

```
(setq gnus-post-method '(nngateway "GATEWAY.ADDRESS"))
```

---

Previous: [Mail-To-News Gateways](#Mail_002dTo_002dNews-Gateways), Up: [Other Sources](#Other-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.6.5 The Empty Backend [¶](#The-Empty-Backend-1)

`nnnil` is a backend that can be used as a placeholder if you
have to specify a backend somewhere, but don’t really want to. The
classical example is if you don’t want to have a primary select
methods, but want to only use secondary ones:

```
(setq gnus-select-method '(nnnil ""))
(setq gnus-secondary-select-methods
      '((nnimap "foo")
        (nnml "")))
```

---

Next: [Email Based Diary](#Email-Based-Diary), Previous: [Other Sources](#Other-Sources), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.7 Virtual Groups [¶](#Virtual-Groups-1)

Gnus allows combining articles from many sources, and combinations of
whole groups together into virtual groups.

* [Select Groups](#Selection-Groups)
* [Combined Groups](#Combined-Groups)

---

Next: [Combined Groups](#Combined-Groups), Up: [Virtual Groups](#Virtual-Groups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.7.1 Select Groups [¶](#Select-Groups)

Gnus provides the *nnselect* method for creating virtual groups
composed of collections of messages, even when these messages come
from groups that span multiple servers and backends. For the most
part these virtual groups behave like any other group: messages may be
threaded, marked, moved, deleted, copied, etc.; groups may be
ephemeral or persistent; groups may be created via
`gnus-group-make-group` or browsed as foreign via
`gnus-group-browse-foreign-server`.

The key to using an nnselect group is specifying the messages to
include. Each nnselect group has a group parameter
`nnselect-specs` which is an alist with two elements: a function
`nnselect-function`; and arguments `nnselect-args` to be
passed to the function, if any.

The function `nnselect-function` must return a vector. Each
element of this vector is in turn a 3-element vector corresponding to
one message. The 3 elements are: the fully-qualified group name; the
message number; and a "score" that can be used for additional sorting.
The values for the score are arbitrary, and are not used directly by
the nnselect method—they may, for example, all be set to 100.

Here is an example:

```
(nnselect-specs
 (nnselect-function . identity)
 (nnselect-args
  . [["nnimap+work:mail" 595 100]
     ["nnimap+home:sent" 223 100]
     ["nntp+news.gmane.io:gmane.emacs.gnus.general" 23666 100]]))
```

The function is the identity and the argument is just the list of
messages to include in the virtual group.

Or we may wish to create a group from the results of a search query:

```
(nnselect-specs
 (nnselect-function . gnus-search-run-query)
 (nnselect-args
  (search-query-spec
   (query . "mark:flag"))
  (search-group-spec
   ("nnimap:home")
   ("nnimap:work"))))
```

This creates a group including all flagged messages from all groups on
two IMAP servers, "home" and "work".

And one last example. Here is a function that runs a search query to
find all messages that have been received recently from certain groups:

```
(defun my-recent-email (args)
  (let ((query-spec
         (list
          (cons 'query
                (format-time-string "SENTSINCE %d-%b-%Y"
                                    (time-subtract nil
                                                   (days-to-time (car args)))))
          (cons 'criteria "")))
        (group-spec (cadr args)))
    (gnus-search-run-query (list (cons 'search-query-spec query-spec)
                                 (cons 'search-group-spec group-spec)))))
```

Then the following `nnselect-specs`:

```
(nnselect-specs
 (nnselect-function . my-recent-email)
 (nnselect-args . (7 (("nnimap:home") ("nnimap:work")))))
```

will provide a group composed of all messages on the home and work
servers received in the last 7 days.

Refreshing the selection of an nnselect group by running the
`nnselect-function` may take a long time to complete.
Consequently nnselect groups are not refreshed by default when
`gnus-group-get-new-news` is invoked. In those cases where
running the function is not too time-consuming, a non-`nil` group
parameter of `nnselect-rescan` will allow automatic refreshing.
A refresh can always be invoked manually through
`gnus-group-get-new-news-this-group`.

By default a compressed version of the selection is stored (for
permanent groups) along with other group information in the newsrc.
For cases where this might be undesirable (for example if the
selection is a very long list that doesn’t compress well) a
non-`nil` group parameter of `nnselect-always-regenerate`
will prevent the list from being stored, and instead regenerate the
list each time it is needed. If more flexibility is desired,
`nnselect-get-artlist-override-function` and
`nnselect-store-artlist-override-function` may be set to
functions that get and store the list of articles.

Gnus includes engines for searching a variety of backends. While the
details of each search engine vary, the result of a search is always a
vector of the sort used by the nnselect method, and the results of
queries are usually viewed using an nnselect group. Indeed the
standard search function `gnus-group-read-ephemeral-search-group`
just creates an ephemeral nnselect group with the appropriate search
query as the `nnselect-specs`.

---

Previous: [Select Groups](#Selection-Groups), Up: [Virtual Groups](#Virtual-Groups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.7.2 Combined Groups [¶](#Combined-Groups-1)

An *nnvirtual group* is really nothing more than a collection of
other groups.

For instance, if you are tired of reading many small groups, you can
put them all in one big group, and then grow tired of reading one
big, unwieldy group. The joys of computing!

You specify `nnvirtual` as the method. The address should be a
regexp to match component groups.

All marks in the virtual group will stick to the articles in the
component groups. So if you tick an article in a virtual group, the
article will also be ticked in the component group from whence it
came. (And vice versa—marks from the component groups will also be
shown in the virtual group.). To create an empty virtual group, run
`G V` (`gnus-group-make-empty-virtual`) in the group buffer
and edit the method regexp with `M-e`
(`gnus-group-edit-group-method`)

Here’s an example `nnvirtual` method that collects all Andrea Dworkin
newsgroups into one, big, happy newsgroup:

```
(nnvirtual "^alt\\.fan\\.andrea-dworkin$\\|^rec\\.dworkin.*")
```

The component groups can be native or foreign; everything should work
smoothly, but if your computer explodes, it was probably my fault.

Collecting the same group from several servers might actually be a good
idea if users have set the Distribution header to limit distribution.
If you would like to read ‘`soc.motss`’ both from a server in Japan
and a server in Norway, you could use the following as the group regexp:

```
"^nntp\\+server\\.jp:soc\\.motss$\\|^nntp\\+server\\.no:soc\\.motss$"
```

(Remember, though, that if you’re creating the group with `G m`, you
shouldn’t double the backslashes, and you should leave off the quote
characters at the beginning and the end of the string.)

This should work kinda smoothly—all articles from both groups should
end up in this one, and there should be no duplicates. Threading (and
the rest) will still work as usual, but there might be problems with the
sequence of articles. Sorting on date might be an option here
(see [Selecting a Group](#Selecting-a-Group)).

One limitation, however—all groups included in a virtual
group have to be alive (i.e., subscribed or unsubscribed). Killed or
zombie groups can’t be component groups for `nnvirtual` groups.

If the `nnvirtual-always-rescan` variable is non-`nil` (which
is the default), `nnvirtual` will always scan groups for unread
articles when entering a virtual group. If this variable is `nil`
and you read articles in a component group after the virtual group has
been activated, the read articles from the component group will show up
when you enter the virtual group. You’ll also see this effect if you
have two virtual groups that have a component group in common. If
that’s the case, you should set this variable to `t`. Or you can
just tap `M-g` on the virtual group every time before you enter
it—it’ll have much the same effect.

`nnvirtual` can have both mail and news groups as component groups.
When responding to articles in `nnvirtual` groups, `nnvirtual`
has to ask the back end of the component group the article comes from
whether it is a news or mail back end. However, when you do a `^`,
there is typically no sure way for the component back end to know this,
and in that case `nnvirtual` tells Gnus that the article came from a
not-news back end. (Just to be on the safe side.)

`C-c C-n` in the message buffer will insert the `Newsgroups`
line from the article you respond to in these cases.

`nnvirtual` groups do not inherit anything but articles and marks
from component groups—group parameters, for instance, are not
inherited.

---

Next: [Gnus Unplugged](#Gnus-Unplugged), Previous: [Virtual Groups](#Virtual-Groups), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.8 Email Based Diary [¶](#Email-Based-Diary-1)

This section describes a special mail back end called `nndiary`,
and its companion library `gnus-diary`. It is “special” in the
sense that it is not meant to be one of the standard alternatives for
reading mail with Gnus. (See [Choosing a Mail Back End](#Choosing-a-Mail-Back-End), for
description of the standard mail back ends.)
Instead, it is used to treat *some* of your mails in a special way,
namely, as event reminders.

Here is a typical scenario:

* You’ve got a date with Andy Mc Dowell or Bruce Willis (select according
  to your sexual preference) in one month. You don’t want to forget it.
* So you send a “reminder” message (actually, a diary one) to yourself.
* You forget all about it and keep on getting and reading new mail, as usual.
* From time to time, as you type `g` in the group buffer and as the date
  is getting closer, the message will pop up again to remind you of your
  appointment, just as if it were new and unread.
* Read your “new” messages, this one included, and start dreaming again
  of the night you’re gonna have.
* Once the date is over (you actually fell asleep just after dinner), the
  message will be automatically deleted if it is marked as expirable.

The Gnus Diary back end has the ability to handle regular appointments
(that wouldn’t ever be deleted) as well as punctual ones, operates as a
real mail back end and is configurable in many ways. All of this is
explained in the sections below.

* [The NNDiary Back End](#The-NNDiary-Back-End)
* [The Gnus Diary Library](#The-Gnus-Diary-Library)

---

Next: [The Gnus Diary Library](#The-Gnus-Diary-Library), Up: [Email Based Diary](#Email-Based-Diary)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.1 The NNDiary Back End [¶](#The-NNDiary-Back-End-1)

`nndiary` is a back end very similar to `nnml` (see [Mail Spool](#Mail-Spool)). Actually, it could appear as a mix of `nnml` and
`nndraft`. If you know `nnml`, you’re already familiar with
the message storing scheme of `nndiary`: one file per message, one
directory per group.

Before anything, there is one requirement to be able to run
`nndiary` properly: you *must* use the group timestamp feature
of Gnus. This adds a timestamp to each group’s parameters. [Group Timestamp](#Group-Timestamp) to see how it’s done.

* [Diary Messages](#Diary-Messages)
* [Running NNDiary](#Running-NNDiary)
* [Customizing NNDiary](#Customizing-NNDiary)

---

Next: [Running NNDiary](#Running-NNDiary), Up: [The NNDiary Back End](#The-NNDiary-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.1.1 Diary Messages [¶](#Diary-Messages-1)

`nndiary` messages are just normal ones, except for the mandatory
presence of 7 special headers. These headers are of the form
`X-Diary-<something>`, `<something>` being one of
`Minute`, `Hour`, `Dom`, `Month`, `Year`,
`Time-Zone` and `Dow`. `Dom` means “Day of Month”, and
`Dow` means “Day of Week”. These headers actually behave like
crontab specifications and define the event date(s):

* For all headers except the `Time-Zone` one, a header value is
  either a star (meaning all possible values), or a list of fields
  (separated by a comma).
* A field is either an integer, or a range.
* A range is two integers separated by a dash.
* Possible integer values are 0–59 for `Minute`, 0–23 for
  `Hour`, 1–31 for `Dom`, 1–12 for `Month`, above 1971
  for `Year` and 0–6 for `Dow` (0 meaning Sunday).
* As a special case, a star in either `Dom` or `Dow` doesn’t
  mean “all possible values”, but “use only the other field”. Note
  that if both are star’ed, the use of either one gives the same result.
* The `Time-Zone` header is special in that it can only have one
  value (`GMT`, for instance). A star doesn’t mean “all possible
  values” (because it makes no sense), but “the current local time
  zone”. Most of the time, you’ll be using a star here. However, for a
  list of available time zone values, see the variable
  `nndiary-headers`.

As a concrete example, here are the diary headers to add to your message
for specifying “Each Monday and each 1st of month, at 12:00, 20:00,
21:00, 22:00, 23:00 and 24:00, from 1999 to 2010” (I’ll let you find
what to do then):

```
X-Diary-Minute: 0
X-Diary-Hour: 12, 20-24
X-Diary-Dom: 1
X-Diary-Month: *
X-Diary-Year: 1999-2010
X-Diary-Dow: 1
X-Diary-Time-Zone: *
```

---

Next: [Customizing NNDiary](#Customizing-NNDiary), Previous: [Diary Messages](#Diary-Messages), Up: [The NNDiary Back End](#The-NNDiary-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.1.2 Running NNDiary [¶](#Running-NNDiary-1)

`nndiary` has two modes of operation: “traditional” (the default)
and “autonomous”. In traditional mode, `nndiary` does not get new
mail by itself. You have to move (`B m`) or copy (`B c`) mails
from your primary mail back end to nndiary groups in order to handle them
as diary messages. In autonomous mode, `nndiary` retrieves its own
mail and handles it independently from your primary mail back end.

One should note that Gnus is not inherently designed to allow several
“master” mail back ends at the same time. However, this does make
sense with `nndiary`: you really want to send and receive diary
messages to your diary groups directly. So, `nndiary` supports
being sort of a “second primary mail back end” (to my knowledge, it is
the only back end offering this feature). However, there is a limitation
(which I hope to fix some day): respooling doesn’t work in autonomous
mode.

In order to use `nndiary` in autonomous mode, you have several
things to do:

* Allow `nndiary` to retrieve new mail by itself. Put the following
  line in your `~/.gnus.el` file:

  ```
  (setq nndiary-get-new-mail t)
  ```
* You must arrange for diary messages (those containing `X-Diary-*`
  headers) to be split in a private folder *before* Gnus treat them.
  Again, this is needed because Gnus cannot (yet ?) properly handle
  multiple primary mail back ends. Getting those messages from a separate
  source will compensate this misfeature to some extent.

  As an example, here’s my procmailrc entry to store diary files in
  `~/.nndiary` (the default `nndiary` mail source file):

  ```
  :0 HD :
  * ^X-Diary
  .nndiary
  ```

Once this is done, you might want to customize the following two options
that affect the diary mail retrieval and splitting processes:

Variable: **nndiary-mail-sources** [¶](#index-nndiary_002dmail_002dsources)
:   This is the diary-specific replacement for the standard
    `mail-sources` variable. It obeys the same syntax, and defaults to
    `(file :path "~/.nndiary")`.

Variable: **nndiary-split-methods** [¶](#index-nndiary_002dsplit_002dmethods)
:   This is the diary-specific replacement for the standard
    `nnmail-split-methods` variable. It obeys the same syntax.

Finally, you may add a permanent `nndiary` virtual server
(something like `(nndiary "diary")` should do) to your
`gnus-secondary-select-methods`.

Hopefully, almost everything (see the TODO section in
`nndiary.el`) will work as expected when you restart Gnus: in
autonomous mode, typing `g` and `M-g` in the group buffer, will
also get your new diary mails and split them according to your
diary-specific rules, `F` will find your new diary groups etc.

---

Previous: [Running NNDiary](#Running-NNDiary), Up: [The NNDiary Back End](#The-NNDiary-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.1.3 Customizing NNDiary [¶](#Customizing-NNDiary-1)

Now that `nndiary` is up and running, it’s time to customize it.
The custom group is called `nndiary` (no, really ?!). You should
browse it to figure out which options you’d like to tweak. The following
two variables are probably the only ones you will want to change:

Variable: **nndiary-reminders** [¶](#index-nndiary_002dreminders)
:   This is the list of times when you want to be reminded of your
    appointments (e.g., 3 weeks before, then 2 days before, then 1 hour
    before and that’s it). Remember that “being reminded” means that the
    diary message will pop up as brand new and unread again when you get new
    mail.

Variable: **nndiary-week-starts-on-monday** [¶](#index-nndiary_002dweek_002dstarts_002don_002dmonday)
:   Rather self-explanatory. Otherwise, Sunday is assumed (this is the
    default).

---

Previous: [The NNDiary Back End](#The-NNDiary-Back-End), Up: [Email Based Diary](#Email-Based-Diary)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.2 The Gnus Diary Library [¶](#The-Gnus-Diary-Library-1)

Using `nndiary` manually (I mean, writing the headers by hand and
so on) would be rather boring. Fortunately, there is a library called
`gnus-diary` written on top of `nndiary`, that does many
useful things for you.

In order to use it, add the following line to your `~/.gnus.el` file:

```
(require 'gnus-diary)
```

Also, you shouldn’t use any `gnus-user-format-function-[d|D]`
(see [Summary Buffer Lines](#Summary-Buffer-Lines)). `gnus-diary` provides both of these
(sorry if you used them before).

* [Diary Summary Line Format](#Diary-Summary-Line-Format)
* [Diary Articles Sorting](#Diary-Articles-Sorting)
* [Diary Headers Generation](#Diary-Headers-Generation)
* [Diary Group Parameters](#Diary-Group-Parameters)

---

Next: [Diary Articles Sorting](#Diary-Articles-Sorting), Up: [The Gnus Diary Library](#The-Gnus-Diary-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.2.1 Diary Summary Line Format [¶](#Diary-Summary-Line-Format-1)

Displaying diary messages in standard summary line format (usually
something like ‘`From Joe: Subject`’) is pretty useless. Most of
the time, you’re the one who wrote the message, and you mostly want to
see the event’s date.

`gnus-diary` provides two supplemental user formats to be used in
summary line formats. `D` corresponds to a formatted time string
for the next occurrence of the event (e.g., “Sat, Sep 22 01, 12:00”),
while `d` corresponds to an approximate remaining time until the
next occurrence of the event (e.g., “in 6 months, 1 week”).

For example, here’s how Joe’s birthday is displayed in my
`nndiary+diary:birthdays` summary buffer (note that the message is
expirable, but will never be deleted, as it specifies a periodic event):

```
   E  Sat, Sep 22 01, 12:00: Joe's birthday (in 6 months, 1 week)
```

In order to get something like the above, you would normally add the
following line to your diary groups’parameters:

```
(gnus-summary-line-format "%U%R%z %uD: %(%s%) (%ud)\n")
```

However, `gnus-diary` does it automatically (see [Diary Group Parameters](#Diary-Group-Parameters)). You can however customize the provided summary line format
with the following user options:

Variable: **gnus-diary-summary-line-format** [¶](#index-gnus_002ddiary_002dsummary_002dline_002dformat)
:   Defines the summary line format used for diary groups (see [Summary Buffer Lines](#Summary-Buffer-Lines)). `gnus-diary` uses it to automatically update the
    diary groups’parameters.

Variable: **gnus-diary-time-format** [¶](#index-gnus_002ddiary_002dtime_002dformat)
:   Defines the format to display dates in diary summary buffers. This is
    used by the `D` user format. See the docstring for details.

Variable: **gnus-diary-delay-format-function** [¶](#index-gnus_002ddiary_002ddelay_002dformat_002dfunction)
:   Defines the format function to use for displaying delays (remaining
    times) in diary summary buffers. This is used by the `d` user
    format. There are currently built-in functions for English and French;
    you can also define your own. See the docstring for details.

---

Next: [Diary Headers Generation](#Diary-Headers-Generation), Previous: [Diary Summary Line Format](#Diary-Summary-Line-Format), Up: [The Gnus Diary Library](#The-Gnus-Diary-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.2.2 Diary Articles Sorting [¶](#Diary-Articles-Sorting-1)

`gnus-diary` provides new sorting functions (see [Sorting the Summary Buffer](#Sorting-the-Summary-Buffer) ) called `gnus-summary-sort-by-schedule`,
`gnus-thread-sort-by-schedule` and
`gnus-article-sort-by-schedule`. These functions let you organize
your diary summary buffers from the closest event to the farthest one.

`gnus-diary` automatically installs
`gnus-summary-sort-by-schedule` as a menu item in the summary
buffer’s “sort” menu, and the two others as the primary (hence
default) sorting functions in the group parameters (see [Diary Group Parameters](#Diary-Group-Parameters)).

---

Next: [Diary Group Parameters](#Diary-Group-Parameters), Previous: [Diary Articles Sorting](#Diary-Articles-Sorting), Up: [The Gnus Diary Library](#The-Gnus-Diary-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.2.3 Diary Headers Generation [¶](#Diary-Headers-Generation-1)

`gnus-diary` provides a function called
`gnus-diary-check-message` to help you handle the `X-Diary-*`
headers. This function ensures that the current message contains all the
required diary headers, and prompts you for values or corrections if
needed.

This function is hooked into the `nndiary` back end, so that
moving or copying an article to a diary group will trigger it
automatically. It is also bound to `C-c C-f d` in
`message-mode` and `article-edit-mode` in order to ease the
process of converting a usual mail to a diary one.

This function takes a prefix argument which will force prompting of
all diary headers, regardless of their presence or validity. That way,
you can very easily reschedule an already valid diary message, for
instance.

---

Previous: [Diary Headers Generation](#Diary-Headers-Generation), Up: [The Gnus Diary Library](#The-Gnus-Diary-Library)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.8.2.4 Diary Group Parameters [¶](#Diary-Group-Parameters-1)

When you create a new diary group, or visit one, `gnus-diary`
automatically checks your group parameters and if needed, sets the
summary line format to the diary-specific value, installs the
diary-specific sorting functions, and also adds the different
`X-Diary-*` headers to the group’s posting-style. It is then easier
to send a diary message, because if you use `C-u a` or `C-u m`
on a diary group to prepare a message, these headers will be inserted
automatically (although not filled with proper values yet).

`nndiary` is a *real* mail back end. You really send real diary
messages for real. This means for instance that you can give
appointments to anybody (provided they use Gnus and `nndiary`) by
sending the diary message to them as well.

---

Previous: [Email Based Diary](#Email-Based-Diary), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
