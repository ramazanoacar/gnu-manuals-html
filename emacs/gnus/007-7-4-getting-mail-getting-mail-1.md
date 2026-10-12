### 7.4 Getting Mail [¶](#Getting-Mail-1)

Reading mail with a newsreader—isn’t that just plain WeIrD? But of
course.

* [Mail in a Newsreader](#Mail-in-a-Newsreader)
* [Getting Started Reading Mail](#Getting-Started-Reading-Mail)
* [Splitting Mail](#Splitting-Mail)
* [Mail Sources](#Mail-Sources)
* [Mail Back End Variables](#Mail-Back-End-Variables)
* [Fancy Mail Splitting](#Fancy-Mail-Splitting)
* [Group Mail Splitting](#Group-Mail-Splitting)
* [Incorporating Old Mail](#Incorporating-Old-Mail)
* [Expiring Mail](#Expiring-Mail)
* [Washing Mail](#Washing-Mail)
* [Duplicates](#Duplicates)
* [Not Reading Mail](#Not-Reading-Mail)
* [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)

---

Next: [Getting Started Reading Mail](#Getting-Started-Reading-Mail), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.1 Mail in a Newsreader [¶](#Mail-in-a-Newsreader-1)

If you are used to traditional mail readers, but have decided to switch
to reading mail with Gnus, you may find yourself experiencing something
of a culture shock.

Gnus does not behave like traditional mail readers. If you want to make
it behave that way, you can, but it’s an uphill battle.

Gnus, by default, handles all its groups using the same approach. This
approach is very newsreaderly—you enter a group, see the new/unread
messages, and when you read the messages, they get marked as read, and
you don’t see them any more. (Unless you explicitly ask for them.)

In particular, you do not do anything explicitly to delete messages.

Does this mean that all the messages that have been marked as read are
deleted? How awful!

But, no, it means that old messages are *expired* according to some
scheme or other. For news messages, the expire process is controlled by
the news administrator; for mail, the expire process is controlled by
you. The expire process for mail is covered in depth in [Expiring Mail](#Expiring-Mail).

What many Gnus users find, after using it a while for both news and
mail, is that the transport mechanism has very little to do with how
they want to treat a message.

Many people subscribe to several mailing lists. These are transported
via SMTP, and are therefore mail. But we might go for weeks without
answering, or even reading these messages very carefully. We may not
need to save them because if we should need to read one again, they are
archived somewhere else.

Some people have local news groups which have only a handful of readers.
These are transported via NNTP, and are therefore news. But we may need
to read and answer a large fraction of the messages very carefully in
order to do our work. And there may not be an archive, so we may need
to save the interesting messages the same way we would personal mail.

The important distinction turns out to be not the transport mechanism,
but other factors such as how interested we are in the subject matter,
or how easy it is to retrieve the message if we need to read it again.

Gnus provides many options for sorting mail into “groups” which behave
like newsgroups, and for treating each group (whether mail or news)
differently.

Some users never get comfortable using the Gnus (ahem) paradigm and wish
that Gnus should grow up and be a male, er, mail reader. It is possible
to whip Gnus into a more mailreaderly being, but, as said before, it’s
not easy. People who prefer proper mail readers should try VM
instead, which is an excellent, and proper, mail reader.

I don’t mean to scare anybody off, but I want to make it clear that you
may be required to learn a new way of thinking about messages. After
you’ve been subjected to The Gnus Way, you will come to love it. I can
guarantee it. (At least the guy who sold me the Emacs Subliminal
Brain-Washing Functions that I’ve put into Gnus did guarantee it. You
Will Be Assimilated. You Love Gnus. You Love The Gnus Mail Way.
You Do.)

---

Next: [Splitting Mail](#Splitting-Mail), Previous: [Mail in a Newsreader](#Mail-in-a-Newsreader), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.2 Getting Started Reading Mail [¶](#Getting-Started-Reading-Mail-1)

It’s quite easy to use Gnus to read your new mail. You just plonk the
mail back end of your choice into `gnus-secondary-select-methods`,
and things will happen automatically.

For instance, if you want to use `nnml` (which is a “one file per
mail” back end), you could put the following in your `~/.gnus.el` file:

```
(setq gnus-secondary-select-methods '((nnml "")))
```

Now, the next time you start Gnus, this back end will be queried for new
articles, and it will move all the messages in your spool file to its
directory, which is `~/Mail/` by default. The new group that will
be created (‘`mail.misc`’) will be subscribed, and you can read it
like any other group.

You will probably want to split the mail into several groups, though:

```
(setq nnmail-split-methods
      '(("junk" "^From:.*Lars Ingebrigtsen")
        ("crazy" "^Subject:.*die\\|^Organization:.*flabby")
        ("other" "")))
```

This will result in three new `nnml` mail groups being created:
‘`nnml:junk`’, ‘`nnml:crazy`’, and ‘`nnml:other`’. All the
mail that doesn’t fit into the first two groups will be placed in the
last group.

This should be sufficient for reading mail with Gnus. You might want to
give the other sections in this part of the manual a perusal, though.
Especially see [Choosing a Mail Back End](#Choosing-a-Mail-Back-End) and see [Expiring Mail](#Expiring-Mail).

---

Next: [Mail Sources](#Mail-Sources), Previous: [Getting Started Reading Mail](#Getting-Started-Reading-Mail), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.3 Splitting Mail [¶](#Splitting-Mail-1)

The `nnmail-split-methods` variable says how the incoming mail is
to be split into groups.

```
(setq nnmail-split-methods
  '(("mail.junk" "^From:.*Lars Ingebrigtsen")
    ("mail.crazy" "^Subject:.*die\\|^Organization:.*flabby")
    ("mail.other" "")))
```

This variable is a list of lists, where the first element of each of
these lists is the name of the mail group (they do not have to be called
something beginning with ‘`mail`’, by the way), and the second
element is a regular expression used on the header of each mail to
determine if it belongs in this mail group. The first string may
contain ‘`\\1`’ forms, like the ones used by `replace-match` to
insert sub-expressions from the matched text. For instance:

```
("list.\\1" "From:.* \\(.*\\)-list@majordomo.com")
```

In that case, `nnmail-split-lowercase-expanded` controls whether
the inserted text should be made lowercase. See [Fancy Mail Splitting](#Fancy-Mail-Splitting).

The second element can also be a function. In that case, it will be
called narrowed to the headers with the first element of the rule as the
argument. It should return a non-`nil` value if it thinks that the
mail belongs in that group.

The last of these groups should always be a general one, and the regular
expression should *always* be ‘`""`’ so that it matches any mails
that haven’t been matched by any of the other regexps. (These rules are
processed from the beginning of the alist toward the end.

If multiple rules match (excluding the general ‘`""`’ group), mail
is crossposted to all these groups. However, if
`nnmail-crosspost` is set to `nil`, the first rule to make a
match will “win”.

If no rule matched, the mail will end up in the ‘`bogus`’ group.
When new groups are created by splitting mail, you may want to run
`gnus-group-find-new-groups` to see the new groups. This also
applies to the ‘`bogus`’ group.

If you like to tinker with this yourself, you can set this variable to a
function of your choice. This function will be called without any
arguments in a buffer narrowed to the headers of an incoming mail
message. The function should return a list of group names that it
thinks should carry this mail message.

This variable can also be a fancy split method. For the syntax,
see [Fancy Mail Splitting](#Fancy-Mail-Splitting).

Note that the mail back ends are free to maul the poor, innocent,
incoming headers all they want to. They all add `Lines` headers;
some add `X-Gnus-Group` headers; most rename the Unix mbox
`FromSPC` line to something else.

The mail back ends all support cross-posting. If several regexps match,
the mail will be “cross-posted” to all those groups.
`nnmail-crosspost` says whether to use this mechanism or not. Note
that no articles are crossposted to the general (‘`""`’) group.

`nnmh` and `nnml` makes crossposts by creating hard links to
the crossposted articles. However, not all file systems support hard
links. If that’s the case for you, set
`nnmail-crosspost-link-function` to `copy-file`. (This
variable is `add-name-to-file` by default.)

If you wish to see where the previous mail split put the messages, you
can use the `M-x nnmail-split-history` command. If you wish to see
where re-spooling messages would put the messages, you can use
`gnus-summary-respool-trace` and related commands (see [Mail Group Commands](#Mail-Group-Commands)).

Header lines longer than the value of
`nnmail-split-header-length-limit` are excluded from the split
function.

By default, splitting does not decode headers, so you can not match on
non-ASCII strings. But it is useful if you want to match
articles based on the raw header data. To enable it, set the
`nnmail-mail-splitting-decodes` variable to a non-`nil` value.
In addition, the value of the `nnmail-mail-splitting-charset`
variable is used for decoding non-MIME encoded string when
`nnmail-mail-splitting-decodes` is non-`nil`. The default
value is `nil` which means not to decode non-MIME encoded
string. A suitable value for you will be `undecided` or be the
charset used normally in mails you are interested in.

By default, splitting is performed on all incoming messages. If you
specify a `directory` entry for the variable `mail-sources`
(see [Mail Source Specifiers](#Mail-Source-Specifiers)), however, then splitting does
*not* happen by default. You can set the variable
`nnmail-resplit-incoming` to a non-`nil` value to make
splitting happen even in this case. (This variable has no effect on
other kinds of entries.)

Gnus gives you all the opportunity you could possibly want for shooting
yourself in the foot. Let’s say you create a group that will contain
all the mail you get from your boss. And then you accidentally
unsubscribe from the group. Gnus will still put all the mail from your
boss in the unsubscribed group, and so, when your boss mails you “Have
that report ready by Monday or you’re fired!”, you’ll never see it and,
come Tuesday, you’ll still believe that you’re gainfully employed while
you really should be out collecting empty bottles to save up for next
month’s rent money.

---

Next: [Mail Back End Variables](#Mail-Back-End-Variables), Previous: [Splitting Mail](#Splitting-Mail), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.4 Mail Sources [¶](#Mail-Sources-1)

Mail can be gotten from many different sources—the mail spool, from
a POP mail server, from a procmail directory, or from a
maildir, for instance.

* [Mail Source Specifiers](#Mail-Source-Specifiers)
* [Function Interface](#Mail-Source-Functions)
* [Mail Source Customization](#Mail-Source-Customization)
* [Fetching Mail](#Fetching-Mail)

---

Next: [Function Interface](#Mail-Source-Functions), Up: [Mail Sources](#Mail-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.4.1 Mail Source Specifiers [¶](#Mail-Source-Specifiers-1)

You tell Gnus how to fetch mail by setting `mail-sources`
(see [Fetching Mail](#Fetching-Mail)) to a *mail source specifier*.

Here’s an example:

```
(pop :server "pop3.mailserver.com" :user "myname")
```

As can be observed, a mail source specifier is a list where the first
element is a *mail source type*, followed by an arbitrary number of
*keywords*. Keywords that are not explicitly specified are given
default values.

The `mail-sources` is global for all mail groups. You can specify
an additional mail source for a particular group by including the
`group` mail specifier in `mail-sources`, and setting a
`mail-source` group parameter (see [Group Parameters](#Group-Parameters)) specifying
a single mail source. When this is used, `mail-sources` is
typically just `((group))`; the `mail-source` parameter for a
group might look like this:

```
(mail-source . (file :path "home/user/spools/foo.spool"))
```

This means that the group’s (and only this group’s) messages will be
fetched from the spool file ‘`/user/spools/foo.spool`’.

The following mail source types are available:

`file`
:   Get mail from a single file; typically from the mail spool.

    Keywords:

    `:path`
    :   The file name. Defaults to the value of the `MAIL`
        environment variable or the value of `rmail-spool-directory`
        (usually something like `/usr/mail/spool/user-name`).

    `:prescript`

    `:postscript`
    :   Script run before/after fetching mail.

    An example file mail source:

    ```
    (file :path "/usr/spool/mail/user-name")
    ```

    Or using the default file name:

    ```
    (file)
    ```

    If the mail spool file is not located on the local machine, it’s best
    to use POP or IMAP or the like to fetch the mail.
    You can not use ange-ftp file names here—it has no way to lock the
    mail spool while moving the mail.

    If it’s impossible to set up a proper server, you can use ssh instead.

    ```
    (setq mail-sources
          '((file :prescript "ssh host bin/getmail >%t")))
    ```

    The ‘`getmail`’ script would look something like the following:

    ```
    #!/bin/sh
    #  getmail - move mail from spool to stdout
    #  flu@iki.fi

    MOVEMAIL=/usr/lib/emacs/20.3/i386-redhat-linux/movemail
    TMP=$HOME/Mail/tmp
    rm -f $TMP; $MOVEMAIL $MAIL $TMP >/dev/null && cat $TMP
    ```

    Alter this script to fit the ‘`movemail`’ and temporary
    file you want to use.

`directory` [¶](#index-nnmail_002dscan_002ddirectory_002dmail_002dsource_002donce)
:   Get mail from several files in a directory. This is typically used
    when you have procmail split the incoming mail into several files.
    That is, there is a one-to-one correspondence between files in that
    directory and groups, so that mail from the file `foo.bar.spool`
    will be put in the group `foo.bar`. (You can change the suffix
    to be used instead of `.spool`.) Setting
    `nnmail-scan-directory-mail-source-once` to non-`nil` forces
    Gnus to scan the mail source only once. This is particularly useful
    if you want to scan mail groups at a specified level.

    There is also the variable `nnmail-resplit-incoming`, if you set
    that to a non-`nil` value, then the normal splitting process is
    applied to all the files from the directory, [Splitting Mail](#Splitting-Mail).

    Keywords:

    `:path`
    :   The name of the directory where the files are. There is no default
        value.

    `:suffix`
    :   Only files ending with this suffix are used. The default is
        ‘`.spool`’.

    `:predicate`
    :   Only files that have this predicate return non-`nil` are returned.
        The default is `identity`. This is used as an additional
        filter—only files that have the right suffix *and* satisfy this
        predicate are considered.

    `:prescript`

    `:postscript`
    :   Script run before/after fetching mail.

    An example directory mail source:

    ```
    (directory :path "/home/user-name/procmail-dir/"
               :suffix ".prcml")
    ```

`pop`
:   Get mail from a POP server.

    Keywords:

    `:server`
    :   The name of the POP server. The default is taken from the
        `MAILHOST` environment variable.

    `:port`
    :   The port number of the POP server. This can be a number (e.g.,
        ‘`:port 1234`’) or a string (e.g., ‘`:port "pop3"`’). If it is a
        string, it should be a service name as listed in `/etc/services` on
        Unix systems. The default is ‘`"pop3"`’. On some systems you might
        need to specify it as ‘`"pop-3"`’ instead.

    `:user`
    :   The user name to give to the POP server. The default is the login
        name.

    `:password`
    :   The password to give to the POP server. If not specified,
        the user is prompted.

    `:program`
    :   The program to use to fetch mail from the POP server. This
        should be a `format`-like string. Here’s an example:

        ```
        fetchmail %u@%s -P %p %t
        ```

        The valid format specifier characters are:

        ‘`t`’
        :   The name of the file the mail is to be moved to. This must always be
            included in this string.

        ‘`s`’
        :   The name of the server.

        ‘`P`’
        :   The port number of the server.

        ‘`u`’
        :   The user name to use.

        ‘`p`’
        :   The password to use.

        The values used for these specs are taken from the values you give the
        corresponding keywords.

    `:prescript`
    :   A script to be run before fetching the mail. The syntax is the same as
        the `:program` keyword. This can also be a function to be run.

        One popular way to use this is to set up an SSH tunnel to access the
        POP server. Here’s an example:

        ```
        (pop :server "127.0.0.1"
             :port 1234
             :user "foo"
             :password "secret"
             :prescript
             "nohup ssh -f -L 1234:pop.server:110 remote.host sleep 3600 &")
        ```

    `:postscript`
    :   A script to be run after fetching the mail. The syntax is the same as
        the `:program` keyword. This can also be a function to be run.

    `:function`
    :   The function to use to fetch mail from the POP server. The
        function is called with one parameter—the name of the file where the
        mail should be moved to.

    `:authentication`
    :   This can be either the symbol `password` or the symbol `apop`
        and says what authentication scheme to use. The default is
        `password`.

    `:leave`
    :   Non-`nil` if the mail is to be left on the POP server
        after fetching. Only the built-in `pop3-movemail` program (the
        default) supports this keyword.

        If this is a number, leave mails on the server for this many days since
        you first checked new mails. In that case, mails once fetched will
        never be fetched again by the UIDL control. If this is
        `nil` (the default), mails will be deleted on the server right
        after fetching. If this is neither `nil` nor a number, all mails
        will be left on the server, and you will end up getting the same mails
        again and again.

        The `pop3-uidl-file` variable specifies the file to which the
        UIDL data are locally stored. The default value is
        `~/.pop3-uidl`.

        Note that POP servers maintain no state information between
        sessions, so what the client believes is there and what is actually
        there may not match up. If they do not, then you may get duplicate
        mails or the whole thing can fall apart and leave you with a corrupt
        mailbox.

    If the `:program` and `:function` keywords aren’t specified,
    `pop3-movemail` will be used.

    Here are some examples for getting mail from a POP server.

    Fetch from the default POP server, using the default user
    name, and default fetcher:

    ```
    (pop)
    ```

    Fetch from a named server with a named user and password:

    ```
    (pop :server "my.pop.server"
         :user "user-name" :password "secret")
    ```

    Leave mails on the server for 14 days:

    ```
    (pop :server "my.pop.server"
         :user "user-name" :password "secret"
         :leave 14)
    ```

    Use ‘`movemail`’ to move the mail:

    ```
    (pop :program "movemail po:%u %t %p")
    ```

`maildir`
:   Get mail from a maildir. This is a type of mailbox that is supported by
    at least qmail and postfix, where each file in a special directory
    contains exactly one mail.

    Keywords:

    `:path`
    :   The name of the directory where the mails are stored. The default is
        taken from the `MAILDIR` environment variable or
        `~/Maildir/`.

    `:subdirs`
    :   The subdirectories of the Maildir. The default is
        ‘`("new" "cur")`’.

        You can also get mails from remote hosts (because maildirs don’t suffer
        from locking problems).

    Two example maildir mail sources:

    ```
    (maildir :path "/home/user-name/Maildir/"
             :subdirs ("cur" "new"))
    ```

    ```
    (maildir :path "/user@remotehost.org:~/Maildir/"
             :subdirs ("new"))
    ```

`imap`
:   Get mail from an IMAP server. If you don’t want to use
    IMAP as intended, as a network mail reading protocol (i.e.,
    with nnimap), for some reason or other, Gnus lets you treat it similar
    to a POP server and fetches articles from a given
    IMAP mailbox. See [Using IMAP](#Using-IMAP), for more information.

    Keywords:

    `:server`
    :   The name of the IMAP server. The default is taken from the
        `MAILHOST` environment variable.

    `:port`
    :   The port number of the IMAP server. The default is ‘`143`’, or
        ‘`993`’ for TLS/SSL connections.

    `:user`
    :   The user name to give to the IMAP server. The default is the login
        name.

    `:password`
    :   The password to give to the IMAP server. If not specified, the user is
        prompted.

    `:stream`
    :   What stream to use for connecting to the server, this is one of the
        symbols in `imap-stream-alist`. Right now, this means
        ‘`gssapi`’, ‘`kerberos4`’, ‘`starttls`’, ‘`tls`’,
        ‘`ssl`’, ‘`shell`’ or the default ‘`network`’.

    `:authentication`
    :   Which authenticator to use for authenticating to the server, this is
        one of the symbols in `imap-authenticator-alist`. Right now,
        this means ‘`gssapi`’, ‘`kerberos4`’, ‘`digest-md5`’,
        ‘`cram-md5`’, ‘`plain`’, ‘`anonymous`’ or the default ‘`login`’.

    `:program`
    :   When using the ‘`shell`’ :stream, the contents of this variable is
        mapped into the `imap-shell-program` variable. This should be a
        `format`-like string (or list of strings). Here’s an example:

        ```
        ssh %s imapd
        ```

        Make sure nothing is interfering with the output of the program, e.g.,
        don’t forget to redirect the error output to the void. The valid format
        specifier characters are:

        ‘`s`’
        :   The name of the server.

        ‘`l`’
        :   User name from `imap-default-user`.

        ‘`p`’
        :   The port number of the server.

        The values used for these specs are taken from the values you give the
        corresponding keywords.

    `:mailbox`
    :   The name of the mailbox to get mail from. The default is ‘`INBOX`’
        which normally is the mailbox which receives incoming mail. Instead of
        a single mailbox, this can be a list of mailboxes to fetch mail from.

    `:predicate`
    :   The predicate used to find articles to fetch. The default, ‘`UNSEEN
        UNDELETED`’, is probably the best choice for most people, but if you
        sometimes peek in your mailbox with a IMAP client and mark some
        articles as read (or; SEEN) you might want to set this to ‘`1:*`’.
        Then all articles in the mailbox is fetched, no matter what. For a
        complete list of predicates, see RFC 2060 section 6.4.4.

    `:fetchflag`
    :   How to flag fetched articles on the server, the default ‘`\Deleted`’
        will mark them as deleted, an alternative would be ‘`\Seen`’ which
        would simply mark them as read. These are the two most likely choices,
        but more flags are defined in RFC 2060 section 2.3.2.

    `:dontexpunge`
    :   If non-`nil`, don’t remove all articles marked as deleted in the
        mailbox after finishing the fetch.

    An example IMAP mail source:

    ```
    (imap :server "mail.mycorp.com"
          :stream kerberos4
          :fetchflag "\\Seen")
    ```

`group`
:   Get the actual mail source from the `mail-source` group parameter,
    See [Group Parameters](#Group-Parameters).

*Common Keywords*
:   Common keywords can be used in any type of mail source.

    Keywords:

    `:plugged`
    :   If non-`nil`, fetch the mail even when Gnus is unplugged. If you
        use directory source to get mail, you can specify it as in this
        example:

        ```
        (setq mail-sources
              '((directory :path "/home/pavel/.Spool/"
                           :suffix ""
                           :plugged t)))
        ```

        Gnus will then fetch your mail even when you are unplugged. This is
        useful when you use local mail and news.

---

Next: [Mail Source Customization](#Mail-Source-Customization), Previous: [Mail Source Specifiers](#Mail-Source-Specifiers), Up: [Mail Sources](#Mail-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.4.2 Function Interface [¶](#Function-Interface)

Some of the above keywords specify a Lisp function to be executed.
For each keyword `:foo`, the Lisp variable `foo` is bound to
the value of the keyword while the function is executing. For example,
consider the following mail-source setting:

```
(setq mail-sources '((pop :user "jrl"
                          :server "pophost" :function fetchfunc)))
```

While the function `fetchfunc` is executing, the symbol `user`
is bound to `"jrl"`, and the symbol `server` is bound to
`"pophost"`. The symbols `port`, `password`,
`program`, `prescript`, `postscript`, `function`,
and `authentication` are also bound (to their default values).

See above for a list of keywords for each type of mail source.

---

Next: [Fetching Mail](#Fetching-Mail), Previous: [Function Interface](#Mail-Source-Functions), Up: [Mail Sources](#Mail-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.4.3 Mail Source Customization [¶](#Mail-Source-Customization-1)

The following is a list of variables that influence how the mail is
fetched. You would normally not need to set or change any of these
variables.

`mail-source-crash-box` [¶](#index-mail_002dsource_002dcrash_002dbox)
:   File where mail will be stored while processing it. The default is  
    `~/.emacs-mail-crash-box`.

`mail-source-delete-incoming` [¶](#index-mail_002dsource_002ddelete_002dincoming)
:   If non-`nil`, delete incoming files after handling them. If
    `t`, delete the files immediately, if `nil`, never delete any
    files. If a positive number, delete files older than number of days
    (the deletion will only happen when receiving new mail). You may also
    set `mail-source-delete-incoming` to `nil` and call
    `mail-source-delete-old-incoming` from a hook or interactively.

`mail-source-delete-old-incoming-confirm` [¶](#index-mail_002dsource_002ddelete_002dold_002dincoming_002dconfirm)
:   If non-`nil`, ask for confirmation before deleting old incoming
    files. This variable only applies when
    `mail-source-delete-incoming` is a positive number.

`mail-source-directory` [¶](#index-mail_002dsource_002ddirectory)
:   Directory where incoming mail source files (if any) will be stored. The
    default is `~/Mail/`. At present, the only thing this is used for
    is to say where the incoming files will be stored if the variable
    `mail-source-delete-incoming` is `nil` or a number.

`mail-source-incoming-file-prefix` [¶](#index-mail_002dsource_002dincoming_002dfile_002dprefix)
:   Prefix for file name for storing incoming mail. The default is
    `Incoming`, in which case files will end up with names like
    `Incoming30630D_` or `Incoming298602ZD`. This is really only
    relevant if `mail-source-delete-incoming` is `nil` or a
    number.

`mail-source-default-file-modes` [¶](#index-mail_002dsource_002ddefault_002dfile_002dmodes)
:   All new mail files will get this file mode. The default is `#o600`.

`mail-source-movemail-program` [¶](#index-mail_002dsource_002dmovemail_002dprogram)
:   If non-`nil`, name of program for fetching new mail. If
    `nil`, `movemail` in `exec-directory`.

---

Previous: [Mail Source Customization](#Mail-Source-Customization), Up: [Mail Sources](#Mail-Sources)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.4.4 Fetching Mail [¶](#Fetching-Mail-1)

The way to actually tell Gnus where to get new mail from is to set
`mail-sources` to a list of mail source specifiers
(see [Mail Source Specifiers](#Mail-Source-Specifiers)).

If this variable is `nil`, the mail back ends will never attempt to
fetch mail by themselves.

If you want to fetch mail both from your local spool as well as a
POP mail server, you’d say something like:

```
(setq mail-sources
      '((file)
        (pop :server "pop3.mail.server"
             :password "secret")))
```

Or, if you don’t want to use any of the keyword defaults:

```
(setq mail-sources
      '((file :path "/var/spool/mail/user-name")
        (pop :server "pop3.mail.server"
             :user "user-name"
             :port "pop3"
             :password "secret")))
```

When you use a mail back end, Gnus will slurp all your mail from your
inbox and plonk it down in your home directory. Gnus doesn’t move any
mail if you’re not using a mail back end—you have to do a lot of magic
invocations first. At the time when you have finished drawing the
pentagram, lightened the candles, and sacrificed the goat, you really
shouldn’t be too surprised when Gnus moves your mail.

---

Next: [Fancy Mail Splitting](#Fancy-Mail-Splitting), Previous: [Mail Sources](#Mail-Sources), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.5 Mail Back End Variables [¶](#Mail-Back-End-Variables-1)

These variables are (for the most part) pertinent to all the various
mail back ends.

`nnmail-read-incoming-hook` [¶](#index-nnmail_002dread_002dincoming_002dhook)
:   The mail back ends all call this hook after reading new mail. You can
    use this hook to notify any mail watch programs, if you want to.

`nnmail-split-hook` [¶](#index-gnus_002darticle_002ddecode_002dencoded_002dwords)
:   Hook run in the buffer where the mail headers of each message is kept
    just before the splitting based on these headers is done. The hook is
    free to modify the buffer contents in any way it sees fit—the buffer
    is discarded after the splitting has been done, and no changes performed
    in the buffer will show up in any files.
    `gnus-article-decode-encoded-words` is one likely function to add
    to this hook.

`nnmail-pre-get-new-mail-hook` [¶](#index-nnmail_002dpre_002dget_002dnew_002dmail_002dhook)

`nnmail-post-get-new-mail-hook`
:   These are two useful hooks executed when treating new incoming
    mail—`nnmail-pre-get-new-mail-hook` (is called just before
    starting to handle the new mail) and
    `nnmail-post-get-new-mail-hook` (is called when the mail handling
    is done). Here’s an example of using these two hooks to change the
    default file modes the new mail files get:

    ```
    (add-hook 'nnmail-pre-get-new-mail-hook
              (lambda () (set-default-file-modes #o700)))

    (add-hook 'nnmail-post-get-new-mail-hook
              (lambda () (set-default-file-modes #o775)))
    ```

`nnmail-use-long-file-names` [¶](#index-nnmail_002duse_002dlong_002dfile_002dnames)
:   If non-`nil`, the mail back ends will use long file and directory
    names. Groups like ‘`mail.misc`’ will end up in directories
    (assuming use of `nnml` back end) or files (assuming use of
    `nnfolder` back end) like `mail.misc`. If it is `nil`,
    the same group will end up in `mail/misc`.

`nnmail-delete-file-function` [¶](#index-nnmail_002ddelete_002dfile_002dfunction)
:   Function called to delete files. It is `delete-file` by default.

`nnmail-cache-accepted-message-ids` [¶](#index-nnmail_002dcache_002daccepted_002dmessage_002dids)
:   If non-`nil`, put the `Message-ID`s of articles imported into
    the back end (via `Gcc`, for instance) into the mail duplication
    discovery cache. The default is `nil`.

`nnmail-cache-ignore-groups` [¶](#index-nnmail_002dcache_002dignore_002dgroups)
:   This can be a regular expression or a list of regular expressions.
    Group names that match any of the regular expressions will never be
    recorded in the `Message-ID` cache.

    This can be useful, for example, when using Fancy Splitting
    (see [Fancy Mail Splitting](#Fancy-Mail-Splitting)) together with the function
    `nnmail-split-fancy-with-parent`.

---

Next: [Group Mail Splitting](#Group-Mail-Splitting), Previous: [Mail Back End Variables](#Mail-Back-End-Variables), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.6 Fancy Mail Splitting [¶](#Fancy-Mail-Splitting-1)

If the rather simple, standard method for specifying how to split mail
doesn’t allow you to do what you want, you can set
`nnmail-split-methods` to `nnmail-split-fancy`. Then you can
play with the `nnmail-split-fancy` variable.

Let’s look at an example value of this variable first:

```
;; Messages from the mailer daemon are not crossposted to any of
;; the ordinary groups.  Warnings are put in a separate group
;; from real errors.
(| ("from" mail (| ("subject" "warn.*" "mail.warning")
                   "mail.misc"))
   ;; Non-error messages are crossposted to all relevant
   ;; groups, but we don’t crosspost between the group for the
   ;; (ding) list and the group for other (ding) related mail.
   (& (| (any "ding@ifi\\.uio\\.no" "ding.list")
         ("subject" "ding" "ding.misc"))
      ;; Other mailing lists...
      (any "procmail@informatik\\.rwth-aachen\\.de" "procmail.list")
      (any "SmartList@informatik\\.rwth-aachen\\.de" "SmartList.list")
      ;; Both lists below have the same suffix, so prevent
      ;; cross-posting to mkpkg.list of messages posted only to
      ;; the bugs- list, but allow cross-posting when the
      ;; message was really cross-posted.
      (any "bugs-mypackage@somewhere" "mypkg.bugs")
      (any "mypackage@somewhere" - "bugs-mypackage" "mypkg.list")
      ;; People...
      (any "larsi@ifi\\.uio\\.no" "people.Lars_Magne_Ingebrigtsen"))
   ;; Unmatched mail goes to the catch all group.
   "misc.misc")
```

This variable has the format of a *split*. A split is a
(possibly) recursive structure where each split may contain other
splits. Here are the possible split syntaxes:

`group`
:   If the split is a string, that will be taken as a group name. Normal
    regexp match expansion will be done. See below for examples.

`(field value [- restrict […] ] split [invert-partial])`
:   The split can be a list containing at least three elements. If the
    first element field (a regexp matching a header) contains
    value (also a regexp) then store the message as specified by
    split.

    If restrict (yet another regexp) matches some string after
    field and before the end of the matched value, the
    split is ignored. If none of the restrict clauses match,
    split is processed.

    The last element invert-partial is optional. If it is
    non-`nil`, the match-partial-words behavior controlled by the
    variable `nnmail-split-fancy-match-partial-words` (see below) is
    be inverted. (New in Gnus 5.10.7)

`(| split …)`
:   If the split is a list, and the first element is `|` (vertical
    bar), then process each split until one of them matches. A
    split is said to match if it will cause the mail message to be
    stored in one or more groups.

`(& split …)`
:   If the split is a list, and the first element is `&`, then
    process all splits in the list.

`junk`
:   If the split is the symbol `junk`, then don’t save (i.e., delete)
    this message. Use with extreme caution.

`(: function arg1 arg2 …)`
:   If the split is a list, and the first element is ‘`:`’, then the
    second element will be called as a function with args given as
    arguments. The function should return a split.

    For instance, the following function could be used to split based on the
    body of the messages:

    ```
    (defun split-on-body ()
      (save-excursion
        (save-restriction
          (widen)
          (goto-char (point-min))
          (when (re-search-forward "Some.*string" nil t)
            "string.group"))))
    ```

    The buffer is narrowed to the header of the message in question when
    function is run. That’s why `(widen)` needs to be called
    after `save-excursion` and `save-restriction` in the example
    above. Also note that with the nnimap backend, message bodies will
    not be downloaded by default. You need to set
    `nnimap-split-download-body` to `t` to do that
    (see [Client-Side IMAP Splitting](#Client_002dSide-IMAP-Splitting)).

`(! func split)`
:   If the split is a list, and the first element is `!`, then
    split will be processed, and func will be called as a
    function with the result of split as argument. func
    should return a split.

`nil`
:   If the split is `nil`, it is ignored.

In these splits, field must match a complete field name.

Normally, value in these splits must match a complete *word*
according to the fundamental mode syntax table. In other words, all
value’s will be implicitly surrounded by `\<...\>` markers,
which are word delimiters. Therefore, if you use the following split,
for example,

```
(any "joe" "joemail")
```

messages sent from ‘`joedavis@foo.org`’ will normally not be filed
in ‘`joemail`’. If you want to alter this behavior, you can use any
of the following three ways:

1. You can set the `nnmail-split-fancy-match-partial-words` variable
   to non-`nil` in order to ignore word boundaries and instead the
   match becomes more like a grep. This variable controls whether partial
   words are matched during fancy splitting. The default value is
   `nil`.

   Note that it influences all value’s in your split rules.
2. value beginning with `.*` ignores word boundaries in front of
   a word. Similarly, if value ends with `.*`, word boundaries
   in the rear of a word will be ignored. For example, the value
   `"@example\\.com"` does not match ‘`foo@example.com`’ but
   `".*@example\\.com"` does.
3. You can set the invert-partial flag in your split rules of the
   ‘`(field value …)`’ types, aforementioned in this
   section. If the flag is set, word boundaries on both sides of a word
   are ignored even if `nnmail-split-fancy-match-partial-words` is
   `nil`. Contrarily, if the flag is set, word boundaries are not
   ignored even if `nnmail-split-fancy-match-partial-words` is
   non-`nil`. (New in Gnus 5.10.7)

field and value can also be Lisp symbols, in that case
they are expanded as specified by the variable
`nnmail-split-abbrev-alist`. This is an alist of cons cells,
where the CAR of a cell contains the key, and the CDR
contains the associated value. Predefined entries in
`nnmail-split-abbrev-alist` include:

`from`
:   Matches the ‘`From`’, ‘`Sender`’ and ‘`Resent-From`’ fields.

`to`
:   Matches the ‘`To`’, ‘`Cc`’, ‘`Apparently-To`’,
    ‘`Resent-To`’ and ‘`Resent-Cc`’ fields.

`any`
:   Is the union of the `from` and `to` entries.

`list`
:   Matches the ‘`List-ID`’, ‘`List-Post`’, ‘`X-Mailing-List`’,
    ‘`X-BeenThere`’ and ‘`X-Loop`’ fields.

`nnmail-split-fancy-syntax-table` is the syntax table in effect
when all this splitting is performed.

If you want to have Gnus create groups dynamically based on some
information in the headers (i.e., do `replace-match`-like
substitutions in the group names), you can say things like:

```
(any "debian-\\b\\(\\w+\\)@lists.debian.org" "mail.debian.\\1")
```

In this example, messages sent to ‘`debian-foo@lists.debian.org`’
will be filed in ‘`mail.debian.foo`’.

If the string contains the element ‘`\\&`’, then the previously
matched string will be substituted. Similarly, the elements ‘`\\1`’
up to ‘`\\9`’ will be substituted with the text matched by the
groupings 1 through 9.

Where `nnmail-split-lowercase-expanded` controls whether the
lowercase of the matched string should be used for the substitution.
Setting it as non-`nil` is useful to avoid the creation of multiple
groups when users send to an address using different case
(i.e., mailing-list@domain vs Mailing-List@Domain). The default value
is `t`.

`nnmail-split-fancy-with-parent` is a function which allows you to
split followups into the same groups their parents are in. Sometimes
you can’t make splitting rules for all your mail. For example, your
boss might send you personal mail regarding different projects you are
working on, and as you can’t tell your boss to put a distinguishing
string into the subject line, you have to resort to manually moving the
messages into the right group. With this function, you only have to do
it once per thread.

To use this feature, you have to set `nnmail-treat-duplicates`
and `nnmail-cache-accepted-message-ids` to a non-`nil`
value. And then you can include `nnmail-split-fancy-with-parent`
using the colon feature, like so:

```
(setq nnmail-treat-duplicates 'warn     ; or delete
      nnmail-cache-accepted-message-ids t
      nnmail-split-fancy
      '(| (: nnmail-split-fancy-with-parent)
          ;; other splits go here
        ))
```

This feature works as follows: when `nnmail-treat-duplicates` is
non-`nil`, Gnus records the message id of every message it sees
in the file specified by the variable
`nnmail-message-id-cache-file`, together with the group it is in
(the group is omitted for non-mail messages). When mail splitting is
invoked, the function `nnmail-split-fancy-with-parent` then looks
at the References (and In-Reply-To) header of each message to split
and searches the file specified by `nnmail-message-id-cache-file`
for the message ids. When it has found a parent, it returns the
corresponding group name unless the group name matches the regexp
`nnmail-split-fancy-with-parent-ignore-groups`. It is
recommended that you set `nnmail-message-id-cache-length` to a
somewhat higher number than the default so that the message ids are
still in the cache. (A value of 5000 appears to create a file some
300 kBytes in size.)
When `nnmail-cache-accepted-message-ids` is non-`nil`, Gnus
also records the message ids of moved articles, so that the followup
messages goes into the new group.

Also see the variable `nnmail-cache-ignore-groups` if you don’t
want certain groups to be recorded in the cache. For example, if all
outgoing messages are written to an “outgoing” group, you could set
`nnmail-cache-ignore-groups` to match that group name.
Otherwise, answers to all your messages would end up in the
“outgoing” group.

If `nnmail-debug-splitting` is non-`nil`, the mail splitting
code will log all splitting decisions to the ‘`*nnmail split*`’ buffer.

---

Next: [Incorporating Old Mail](#Incorporating-Old-Mail), Previous: [Fancy Mail Splitting](#Fancy-Mail-Splitting), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.7 Group Mail Splitting [¶](#Group-Mail-Splitting-1)

If you subscribe to dozens of mailing lists but you don’t want to
maintain mail splitting rules manually, group mail splitting is for you.
You just have to set `to-list` and/or `to-address` in group
parameters or group customization and set `nnmail-split-methods` to
`gnus-group-split`. This splitting function will scan all groups
for those parameters and split mail accordingly, i.e., messages posted
from or to the addresses specified in the parameters `to-list` or
`to-address` of a mail group will be stored in that group.

Sometimes, mailing lists have multiple addresses, and you may want mail
splitting to recognize them all: just set the `extra-aliases` group
parameter to the list of additional addresses and it’s done. If you’d
rather use a regular expression, set `split-regexp`.

All these parameters in a group will be used to create an
`nnmail-split-fancy` split, in which the field is ‘`any`’,
the value is a single regular expression that matches
`to-list`, `to-address`, all of `extra-aliases` and all
matches of `split-regexp`, and the split is the name of the
group. restricts are also supported: just set the
`split-exclude` parameter to a list of regular expressions.

If you can’t get the right split to be generated using all these
parameters, or you just need something fancier, you can set the
parameter `split-spec` to an `nnmail-split-fancy` split. In
this case, all other aforementioned parameters will be ignored by
`gnus-group-split`. In particular, `split-spec` may be set to
`nil`, in which case the group will be ignored by
`gnus-group-split`.

`gnus-group-split` will do cross-posting on all groups that match,
by defining a single `&` fancy split containing one split for each
group. If a message doesn’t match any split, it will be stored in the
group named in `gnus-group-split-default-catch-all-group`, unless
some group has `split-spec` set to `catch-all`, in which case
that group is used as the catch-all group. Even though this variable is
often used just to name a group, it may also be set to an arbitrarily
complex fancy split (after all, a group name is a fancy split), and this
may be useful to split mail that doesn’t go to any mailing list to
personal mail folders. Note that this fancy split is added as the last
element of a `|` split list that also contains a `&` split
with the rules extracted from group parameters.

It’s time for an example. Assume the following group parameters have
been defined:

```
nnml:mail.bar:
((to-address . "bar@femail.com")
 (split-regexp . ".*@femail\\.com"))
nnml:mail.foo:
((to-list . "foo@nowhere.gov")
 (extra-aliases "foo@localhost" "foo-redist@home")
 (split-exclude "bugs-foo" "rambling-foo")
 (admin-address . "foo-request@nowhere.gov"))
nnml:mail.others:
((split-spec . catch-all))
```

Setting `nnmail-split-methods` to `gnus-group-split` will
behave as if `nnmail-split-fancy` had been selected and variable
`nnmail-split-fancy` had been set as follows:

```
(| (& (any "\\(bar@femail\\.com\\|.*@femail\\.com\\)" "mail.bar")
      (any "\\(foo@nowhere\\.gov\\|foo@localhost\\|foo-redist@home\\)"
           - "bugs-foo" - "rambling-foo" "mail.foo"))
   "mail.others")
```

If you’d rather not use group splitting for all your mail groups, you
may use it for only some of them, by using `nnmail-split-fancy`
splits like this:

```
(: gnus-group-split-fancy groups no-crosspost catch-all)
```

groups may be a regular expression or a list of group names whose
parameters will be scanned to generate the output split.
no-crosspost can be used to disable cross-posting; in this case, a
single `|` split will be output. catch-all is the fall back
fancy split, used like `gnus-group-split-default-catch-all-group`.
If catch-all is `nil`, or if `split-regexp` matches the
empty string in any selected group, no catch-all split will be issued.
Otherwise, if some group has `split-spec` set to `catch-all`,
this group will override the value of the catch-all argument.

Unfortunately, scanning all groups and their parameters can be quite
slow, especially considering that it has to be done for every message.
But don’t despair! The function `gnus-group-split-setup` can be
used to enable `gnus-group-split` in a much more efficient way. It
sets `nnmail-split-methods` to `nnmail-split-fancy` and sets
`nnmail-split-fancy` to the split produced by
`gnus-group-split-fancy`. Thus, the group parameters are only
scanned once, no matter how many messages are split.

However, if you change group parameters, you’d have to update
`nnmail-split-fancy` manually. You can do it by running
`gnus-group-split-update`. If you’d rather have it updated
automatically, just tell `gnus-group-split-setup` to do it for
you. For example, add to your `~/.gnus.el`:

```
(gnus-group-split-setup auto-update catch-all)
```

If auto-update is non-`nil`, `gnus-group-split-update`
will be added to `gnus-get-top-new-news-hook`, so you won’t ever
have to worry about updating `nnmail-split-fancy` again. If you
don’t omit catch-all (it’s optional, equivalent to `nil`),
`gnus-group-split-default-catch-all-group` will be set to its
value.

Because you may want to change `nnmail-split-fancy` after it is set
by `gnus-group-split-update`, this function will run
`gnus-group-split-updated-hook` just before finishing.

---

Next: [Expiring Mail](#Expiring-Mail), Previous: [Group Mail Splitting](#Group-Mail-Splitting), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.8 Incorporating Old Mail [¶](#Incorporating-Old-Mail-1)

Most people have lots of old mail stored in various file formats. If
you have set up Gnus to read mail using one of the spiffy Gnus mail
back ends, you’ll probably wish to have that old mail incorporated into
your mail groups.

Doing so can be quite easy.

To take an example: You’re reading mail using `nnml`
(see [Mail Spool](#Mail-Spool)), and have set `nnmail-split-methods` to a
satisfactory value (see [Splitting Mail](#Splitting-Mail)). You have an old Unix mbox
file filled with important, but old, mail. You want to move it into
your `nnml` groups.

Here’s how:

1. Go to the group buffer.
2. Type `G f` and give the file name to the mbox file when prompted to create an
   `nndoc` group from the mbox file (see [Foreign Groups](#Foreign-Groups)).
3. Type `SPC` to enter the newly created group.
4. Type `M P b` to process-mark all articles in this group’s buffer
   (see [Setting Process Marks](#Setting-Process-Marks)).
5. Type `B r` to respool all the process-marked articles, and answer
   ‘`nnml`’ when prompted (see [Mail Group Commands](#Mail-Group-Commands)).

All the mail messages in the mbox file will now also be spread out over
all your `nnml` groups. Try entering them and check whether things
have gone without a glitch. If things look ok, you may consider
deleting the mbox file, but I wouldn’t do that unless I was absolutely
sure that all the mail has ended up where it should be.

Respooling is also a handy thing to do if you’re switching from one mail
back end to another. Just respool all the mail in the old mail groups
using the new mail back end.

---

Next: [Washing Mail](#Washing-Mail), Previous: [Incorporating Old Mail](#Incorporating-Old-Mail), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.9 Expiring Mail [¶](#Expiring-Mail-1)

Traditional mail readers have a tendency to remove mail articles when
you mark them as read, in some way. Gnus takes a fundamentally
different approach to mail reading.

Gnus basically considers mail just to be news that has been received in
a rather peculiar manner. It does not think that it has the power to
actually change the mail, or delete any mail messages. If you enter a
mail group, and mark articles as “read”, or kill them in some other
fashion, the mail articles will still exist on the system. I repeat:
Gnus will not delete your old, read mail. Unless you ask it to, of
course.

To make Gnus get rid of your unwanted mail, you have to mark the
articles as *expirable*. (With the default key bindings, this means
that you have to type `E`.) This does not mean that the articles
will disappear right away, however. In general, a mail article will be
deleted from your system if, 1) it is marked as expirable, AND 2) it is
more than one week old. If you do not mark an article as expirable, it
will remain on your system until hell freezes over. This bears
repeating one more time, with some spurious capitalizations: IF you do
NOT mark articles as EXPIRABLE, Gnus will NEVER delete those ARTICLES.

You do not have to mark articles as expirable by hand. Gnus provides
two features, called “auto-expire” and “total-expire”, that can help you
with this. In a nutshell, “auto-expire” means that Gnus hits `E`
for you when you select an article. And “total-expire” means that Gnus
considers all articles as expirable that are read. So, in addition to
the articles marked ‘`E`’, also the articles marked ‘`r`’,
‘`R`’, ‘`O`’, ‘`K`’, ‘`Y`’ (and so on) are considered
expirable. `gnus-auto-expirable-marks` has the full list of
these marks.

When should either auto-expire or total-expire be used? Most people
who are subscribed to mailing lists split each list into its own group
and then turn on auto-expire or total-expire for those groups.
(See [Splitting Mail](#Splitting-Mail), for more information on splitting each list
into its own group.)

Which one is better, auto-expire or total-expire? It’s not easy to
answer. Generally speaking, auto-expire is probably faster. Another
advantage of auto-expire is that you get more marks to work with: for
the articles that are supposed to stick around, you can still choose
between tick and dormant and read marks. But with total-expire, you
only have dormant and ticked to choose from. The advantage of
total-expire is that it works well with adaptive scoring (see [Adaptive Scoring](#Adaptive-Scoring)). Auto-expire works with normal scoring but not with adaptive
scoring.

Groups that match the regular expression
`gnus-auto-expirable-newsgroups` will have all articles that you
read marked as expirable automatically. All articles marked as
expirable have an ‘`E`’ in the first column in the summary buffer.

By default, if you have auto expiry switched on, Gnus will mark all the
articles you read as expirable, no matter if they were read or unread
before. To avoid having articles marked as read marked as expirable
automatically, you can put something like the following in your
`~/.gnus.el` file:

```
(remove-hook 'gnus-mark-article-hook
             'gnus-summary-mark-read-and-unread-as-read)
(add-hook 'gnus-mark-article-hook 'gnus-summary-mark-unread-as-read)
```

Note that making a group auto-expirable doesn’t mean that all read
articles are expired—only the articles marked as expirable
will be expired. Also note that using the `d` command won’t make
articles expirable—only semi-automatic marking of articles as read will
mark the articles as expirable in auto-expirable groups.

Let’s say you subscribe to a couple of mailing lists, and you want the
articles you have read to disappear after a while:

```
(setq gnus-auto-expirable-newsgroups
      "mail.nonsense-list\\|mail.nice-list")
```

Another way to have auto-expiry happen is to have the element
`auto-expire` in the group parameters of the group.

If you use adaptive scoring (see [Adaptive Scoring](#Adaptive-Scoring)) and
auto-expiring, you’ll have problems. Auto-expiring and adaptive scoring
don’t really mix very well.

The `nnmail-expiry-wait` variable supplies the default time an
expirable article has to live. The value of this variable can be
either a number of days (not necessarily an integer), or one of the
symbols `immediate` or `never`, meaning an article is
immediately or never expirable, respectively.

Gnus starts counting days from when the message *arrived*, not
from when it was sent. The default is seven days.

The `nnmail-expiry-wait-function` variable lets you fine-tune how
long articles are to live, based on what group they are in. When set
to a function, its returned value, if non-`nil`, overrides that
of `nnmail-expiry-wait`. Otherwise, the value of
`nnmail-expiry-wait` is used instead.

For example, let’s say you want to have a one month expiry period in
the ‘`mail.private`’ group, a one day expiry period in the
‘`mail.junk`’ group, and a six day expiry period everywhere else.
This can be achieved as follows:

```
(setq nnmail-expiry-wait-function
      (lambda (group)
        (cond ((string= group "mail.private")
               31)
              ((string= group "mail.junk")
               1)
              ((string= group "important")
               'never)
              (t
               6))))
```

The group names this function is fed are “unadorned” group
names—no ‘`nnml:`’ prefixes and the like.

As an alternative to the variables `nnmail-expiry-wait` or
`nnmail-expiry-wait-function`, you can also use the
`expiry-wait` group parameter to selectively change the expiry
period (see [Group Parameters](#Group-Parameters)).

The normal action taken when expiring articles is to delete them.
However, in some circumstances it might make more sense to move them
to other groups instead of deleting them. The variable
`nnmail-expiry-target` (and the `expiry-target` group
parameter) controls this. The variable supplies a default value for
all groups, which can be overridden for specific groups by the group
parameter. default value is `delete`, but this can also be a
string (which should be the name of the group the message should be
moved to), or a function (which will be called in a buffer narrowed to
the message in question, and with the name of the group being moved
from as its parameter) which should return a target—either a group
name or `delete`.

Here’s an example for specifying a group name:

```
(setq nnmail-expiry-target "nnml:expired")
```

Gnus provides a function `nnmail-fancy-expiry-target` which will
expire mail to groups according to the variable
`nnmail-fancy-expiry-targets`. Here’s an example:

```
 (setq nnmail-expiry-target 'nnmail-fancy-expiry-target
       nnmail-fancy-expiry-targets
       '((to-from "boss" "nnfolder:Work")
         ("subject" "IMPORTANT" "nnfolder:IMPORTANT.%Y.%b")
         ("from" ".*" "nnfolder:Archive-%Y")))
```

With this setup, any mail that has `IMPORTANT` in its Subject
header and was sent in the year `YYYY` and month `MMM`, will
get expired to the group `nnfolder:IMPORTANT.YYYY.MMM`. If its
From or To header contains the string `boss`, it will get expired
to `nnfolder:Work`. All other mail will get expired to
`nnfolder:Archive-YYYY`.

If `nnmail-keep-last-article` is non-`nil`, Gnus will never
expire the final article in a mail newsgroup. This is to make life
easier for procmail users.

By the way: That line up there, about Gnus never expiring non-expirable
articles, is a lie. If you put `total-expire` in the group
parameters, articles will not be marked as expirable, but all read
articles will be put through the expiry process. Use with extreme
caution. Even more dangerous is the
`gnus-total-expirable-newsgroups` variable. All groups that match
this regexp will have all read articles put through the expiry process,
which means that *all* old mail articles in the groups in question
will be deleted after a while. Use with extreme caution, and don’t come
crying to me when you discover that the regexp you used matched the
wrong group and all your important mail has disappeared. Be a
*man*! Or a *woman*! Whatever you feel more comfortable
with! So there!

Most people make most of their mail groups total-expirable, though.

If `gnus-inhibit-user-auto-expire` is non-`nil`, user marking
commands will not mark an article as expirable, even if the group has
auto-expire turned on.

The expirable marks of articles will be removed when copying or moving
them to a group in which auto-expire is not turned on. This is for
preventing articles from being expired unintentionally. On the other
hand, to a group that has turned auto-expire on, the expirable marks of
articles that are copied or moved will not be changed by default. I.e.,
when copying or moving to such a group, articles that were expirable
will be left expirable and ones that were not expirable will not be
marked as expirable. So, even though in auto-expire groups, some
articles will never get expired (unless you read them again). If you
don’t side with that behavior that unexpirable articles may be mixed
into auto-expire groups, you can set
`gnus-mark-copied-or-moved-articles-as-expirable` to a
non-`nil` value. In that case, articles that have been read will
be marked as expirable automatically when being copied or moved to a
group that has auto-expire turned on. The default value is `nil`.

---

Next: [Duplicates](#Duplicates), Previous: [Expiring Mail](#Expiring-Mail), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.10 Washing Mail [¶](#Washing-Mail-1)

Mailers and list servers are notorious for doing all sorts of really,
really stupid things with mail. “Hey, RFC 822 doesn’t explicitly
prohibit us from adding the string `wE aRe ElItE!!!!!1!!` to the
end of all lines passing through our server, so let’s do that!!!!1!”
Yes, but RFC 822 and its successors weren’t designed to be read by
morons. Things that were considered to be self-evident were not
discussed. So. Here we are.

Case in point: The German version of Microsoft Exchange adds ‘`AW:`’ to the subjects of replies instead of ‘`Re:` ’. I could pretend to
be shocked and dismayed by this, but I haven’t got the energy. It is to
laugh.

Gnus provides a plethora of functions for washing articles while
displaying them, but it might be nicer to do the filtering before
storing the mail to disk. For that purpose, we have three hooks and
various functions that can be put in these hooks.

`nnmail-prepare-incoming-hook` [¶](#index-nnmail_002dprepare_002dincoming_002dhook)
:   This hook is called before doing anything with the mail and is meant for
    grand, sweeping gestures. It is called in a buffer that contains all
    the new, incoming mail. Functions to be used include:

    `nnheader-ms-strip-cr` [¶](#index-nnheader_002dms_002dstrip_002dcr)
    :   Remove trailing carriage returns from each line. This is default on
        Emacs running on MS machines.

`nnmail-prepare-incoming-header-hook` [¶](#index-nnmail_002dprepare_002dincoming_002dheader_002dhook)
:   This hook is called narrowed to each header. It can be used when
    cleaning up the headers. Functions that can be used include:

    `nnmail-remove-leading-whitespace` [¶](#index-nnmail_002dremove_002dleading_002dwhitespace)
    :   Clear leading white space that “helpful” listservs have added to the
        headers to make them look nice. Aaah.

        (Note that this function works on both the header and the body of all
        messages, so it is a potentially dangerous function to use (if a body
        of a message contains something that looks like a header line). So
        rather than fix the bug, it is of course the right solution to make it
        into a feature by documenting it.)

    `nnmail-remove-list-identifiers` [¶](#index-nnmail_002dremove_002dlist_002didentifiers)
    :   Some list servers add an identifier—for example, ‘`(idm)`’—to the
        beginning of all `Subject` headers. I’m sure that’s nice for
        people who use stone age mail readers. This function will remove
        strings that match the `nnmail-list-identifiers` regexp, which can
        also be a list of regexp. `nnmail-list-identifiers` may not contain
        `\\(..\\)`.

        For instance, if you want to remove the ‘`(idm)`’ and the
        ‘`nagnagnag`’ identifiers:

        ```
        (setq nnmail-list-identifiers
              '("(idm)" "nagnagnag"))
        ```

        This can also be done non-destructively with
        `gnus-list-identifiers`, See [Article Hiding](#Article-Hiding).

    `nnmail-remove-tabs` [¶](#index-nnmail_002dremove_002dtabs)
    :   Translate all ‘`TAB`’ characters into ‘`SPC`’ characters.

    `nnmail-ignore-broken-references` [¶](#index-nnmail_002dignore_002dbroken_002dreferences)
    :   Some mail user agents (e.g., Eudora and Pegasus) produce broken
        `References` headers, but correct `In-Reply-To` headers. This
        function will get rid of the `References` header if the headers
        contain a line matching the regular expression
        `nnmail-broken-references-mailers`.

`nnmail-prepare-incoming-message-hook` [¶](#index-nnmail_002dprepare_002dincoming_002dmessage_002dhook)
:   This hook is called narrowed to each message. Functions to be used
    include:

    `article-de-quoted-unreadable` [¶](#index-article_002dde_002dquoted_002dunreadable)
    :   Decode Quoted Readable encoding.

---

Next: [Not Reading Mail](#Not-Reading-Mail), Previous: [Washing Mail](#Washing-Mail), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.11 Duplicates [¶](#Duplicates-1)

If you are a member of a couple of mailing lists, you will sometimes
receive two copies of the same mail. This can be quite annoying, so
`nnmail` checks for and treats any duplicates it might find. To do
this, it keeps a cache of old `Message-ID`s:
`nnmail-message-id-cache-file`, which is `~/.nnmail-cache` by
default. The approximate maximum number of `Message-ID`s stored
there is controlled by the `nnmail-message-id-cache-length`
variable, which is 1000 by default. (So 1000 `Message-ID`s will be
stored.) If all this sounds scary to you, you can set
`nnmail-treat-duplicates` to `warn` (which is what it is by
default), and `nnmail` won’t delete duplicate mails. Instead it
will insert a warning into the head of the mail saying that it thinks
that this is a duplicate of a different message.

This variable can also be a function. If that’s the case, the function
will be called from a buffer narrowed to the message in question with
the `Message-ID` as a parameter. The function must return either
`nil`, `warn`, or `delete`.

You can turn this feature off completely by setting the variable to
`nil`.

If you want all the duplicate mails to be put into a special
*duplicates* group, you could do that using the normal mail split
methods:

```
(setq nnmail-split-fancy
      '(| ;; Messages duplicates go to a separate group.
        ("gnus-warning" "duplicat\\(e\\|ion\\) of message" "duplicate")
        ;; Message from daemons, postmaster, and the like to another.
        (any mail "mail.misc")
        ;; Other rules.
        [...] ))
```

Or something like:

```
(setq nnmail-split-methods
      '(("duplicates" "^Gnus-Warning:.*duplicate")
        ;; Other rules.
        [...]))
```

Here’s a neat feature: If you know that the recipient reads her mail
with Gnus, and that she has `nnmail-treat-duplicates` set to
`delete`, you can send her as many insults as you like, just by
using a `Message-ID` of a mail that you know that she’s already
received. Think of all the fun! She’ll never see any of it! Whee!

---

Next: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End), Previous: [Duplicates](#Duplicates), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.12 Not Reading Mail [¶](#Not-Reading-Mail-1)

If you start using any of the mail back ends, they have the annoying
habit of assuming that you want to read mail with them. This might not
be unreasonable, but it might not be what you want.

If you set `mail-sources` to `nil`, none of the back ends
will ever attempt to read incoming mail, which should help.

This might be too much, if, for instance, you are reading mail quite
happily with `nnml` and just want to peek at some old (pre-Emacs
23) Rmail file you have stashed away with `nnbabyl`. All back ends have
variables called back-end-`get-new-mail`. If you want to disable
the `nnbabyl` mail reading, you edit the virtual server for the
group to have a setting where `nnbabyl-get-new-mail` to `nil`.

All the mail back ends will call `nn`\*`-prepare-save-mail-hook`
narrowed to the article to be saved before saving it when reading
incoming mail.

---

Previous: [Not Reading Mail](#Not-Reading-Mail), Up: [Getting Mail](#Getting-Mail)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13 Choosing a Mail Back End [¶](#Choosing-a-Mail-Back-End-1)

Gnus will read the mail spool when you activate a mail group. The mail
file is first copied to your home directory. What happens after that
depends on what format you want to store your mail in.

There are six different mail back ends in the standard Gnus, and more
back ends are available separately. The mail back end most people use
(because it is possibly the fastest) is `nnml` (see [Mail Spool](#Mail-Spool)).

* [Unix Mail Box](#Unix-Mail-Box)
* [Babyl](#Babyl)
* [Mail Spool](#Mail-Spool)
* [MH Spool](#MH-Spool)
* [Maildir](#Maildir)
* [Group parameters](#nnmaildir-Group-Parameters)
* [Article identification](#Article-Identification)
* [NOV data](#NOV-Data)
* [Article marks](#Article-Marks)
* [Mail Folders](#Mail-Folders)
* [Comparing Mail Back Ends](#Comparing-Mail-Back-Ends)

---

Next: [Babyl](#Babyl), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.1 Unix Mail Box [¶](#Unix-Mail-Box-1)

The *nnmbox* back end will use the standard Un\*x mbox file to store
mail. `nnmbox` will add extra headers to each mail article to say
which group it belongs in.

Virtual server settings:

`nnmbox-mbox-file` [¶](#index-nnmbox_002dmbox_002dfile-1)
:   The name of the mail box in the user’s home directory. Default is
    `~/mbox`.

`nnmbox-active-file` [¶](#index-nnmbox_002dactive_002dfile-1)
:   The name of the active file for the mail box. Default is
    `~/.mbox-active`.

`nnmbox-get-new-mail` [¶](#index-nnmbox_002dget_002dnew_002dmail-1)
:   If non-`nil`, `nnmbox` will read incoming mail and split it
    into groups. Default is `t`.

---

Next: [Mail Spool](#Mail-Spool), Previous: [Unix Mail Box](#Unix-Mail-Box), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.2 Babyl [¶](#Babyl-1)

The *nnbabyl* back end will use a Babyl mail box to store mail.
`nnbabyl` will add extra headers to each mail article to say which
group it belongs in.

Virtual server settings:

`nnbabyl-mbox-file` [¶](#index-nnbabyl_002dmbox_002dfile-1)
:   The name of the Babyl file. The default is `~/RMAIL`

`nnbabyl-active-file` [¶](#index-nnbabyl_002dactive_002dfile-1)
:   The name of the active file for the Babyl file. The default is
    `~/.rmail-active`

`nnbabyl-get-new-mail` [¶](#index-nnbabyl_002dget_002dnew_002dmail-1)
:   If non-`nil`, `nnbabyl` will read incoming mail. Default is
    `t`

---

Next: [MH Spool](#MH-Spool), Previous: [Babyl](#Babyl), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.3 Mail Spool [¶](#Mail-Spool-1)

The *nnml* spool mail format isn’t compatible with any other known
format. It should be used with some caution.

If you use this back end, Gnus will split all incoming mail into files,
one file for each mail, and put the articles into the corresponding
directories under the directory specified by the `nnml-directory`
variable. The default value is `~/Mail/`.

You do not have to create any directories beforehand; Gnus will take
care of all that.

If you have a strict limit as to how many files you are allowed to store
in your account, you should not use this back end. As each mail gets its
own file, you might very well occupy thousands of inodes within a few
weeks. If this is no problem for you, and it isn’t a problem for you
having your friendly systems administrator walking around, madly,
shouting “Who is eating all my inodes?! Who? Who!?!”, then you should
know that this is probably the fastest format to use. You do not have
to trudge through a big mbox file just to read your new mail.

`nnml` is probably the slowest back end when it comes to article
splitting. It has to create lots of files, and it also generates
NOV databases for the incoming mails. This makes it possibly the
fastest back end when it comes to reading mail.

Virtual server settings:

`nnml-directory` [¶](#index-nnml_002ddirectory-1)
:   All `nnml` directories will be placed under this directory. The
    default is the value of `message-directory` (whose default value
    is `~/Mail`).

`nnml-active-file` [¶](#index-nnml_002dactive_002dfile)
:   The active file for the `nnml` server. The default is
    `~/Mail/active`.

`nnml-newsgroups-file` [¶](#index-nnml_002dnewsgroups_002dfile)
:   The `nnml` group descriptions file. See [Newsgroups File Format](#Newsgroups-File-Format). The default is `~/Mail/newsgroups`.

`nnml-get-new-mail` [¶](#index-nnml_002dget_002dnew_002dmail-1)
:   If non-`nil`, `nnml` will read incoming mail. The default is
    `t`.

`nnml-nov-is-evil` [¶](#index-nnml_002dnov_002dis_002devil)
:   If non-`nil`, this back end will ignore any NOV files. The
    default is `nil`.

`nnml-nov-file-name` [¶](#index-nnml_002dnov_002dfile_002dname)
:   The name of the NOV files. The default is `.overview`.

`nnml-prepare-save-mail-hook` [¶](#index-nnml_002dprepare_002dsave_002dmail_002dhook)
:   Hook run narrowed to an article before saving.

`nnml-use-compressed-files` [¶](#index-nnml_002duse_002dcompressed_002dfiles)
:   If non-`nil`, `nnml` will allow using compressed message
    files. This requires `auto-compression-mode` to be enabled
    (see [Compressed Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Compressed-Files) in The Emacs Manual).
    If the value of `nnml-use-compressed-files` is a string, it is used
    as the file extension specifying the compression program. You can set it
    to ‘`.bz2`’ if your Emacs supports it. A value of `t` is
    equivalent to ‘`.gz`’.

`nnml-compressed-files-size-threshold` [¶](#index-nnml_002dcompressed_002dfiles_002dsize_002dthreshold)
:   Default size threshold for compressed message files. Message files with
    bodies larger than that many characters will be automatically compressed
    if `nnml-use-compressed-files` is non-`nil`.

If your `nnml` groups and NOV files get totally out of
whack, you can do a complete update by typing `M-x
nnml-generate-nov-databases`. This command will trawl through the
entire `nnml` hierarchy, looking at each and every article, so it
might take a while to complete. A better interface to this
functionality can be found in the server buffer (see [Server Commands](#Server-Commands)).

---

Next: [Maildir](#Maildir), Previous: [Mail Spool](#Mail-Spool), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.4 MH Spool [¶](#MH-Spool-1)

`nnmh` is just like `nnml`, except that is doesn’t generate
NOV databases and it doesn’t keep an active file or marks
file. This makes `nnmh` a *much* slower back end than
`nnml`, but it also makes it easier to write procmail scripts
for.

Virtual server settings:

`nnmh-directory` [¶](#index-nnmh_002ddirectory)
:   All `nnmh` directories will be located under this directory. The
    default is the value of `message-directory` (whose default is
    `~/Mail`)

`nnmh-get-new-mail` [¶](#index-nnmh_002dget_002dnew_002dmail-1)
:   If non-`nil`, `nnmh` will read incoming mail. The default is
    `t`.

`nnmh-be-safe` [¶](#index-nnmh_002dbe_002dsafe)
:   If non-`nil`, `nnmh` will go to ridiculous lengths to make
    sure that the articles in the folder are actually what Gnus thinks
    they are. It will check date stamps and stat everything in sight, so
    setting this to `t` will mean a serious slow-down. If you never
    use anything but Gnus to read the `nnmh` articles, you do not
    have to set this variable to `t`. The default is `nil`.

---

Next: [Group parameters](#nnmaildir-Group-Parameters), Previous: [MH Spool](#MH-Spool), Up: [Choosing a Mail Back End](#Choosing-a-Mail-Back-End)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.4.13.5 Maildir [¶](#Maildir-1)

`nnmaildir` stores mail in the maildir format, with each maildir
corresponding to a group in Gnus. This format is documented here:
<https://cr.yp.to/proto/maildir.html>. `nnmaildir`
also stores extra information in the `.nnmaildir/` directory
within a maildir.

Maildir format was designed to allow concurrent deliveries and
reading, without needing locks. With other back ends, you would have
your mail delivered to a spool of some kind, and then you would
configure Gnus to split mail from that spool into your groups. You
can still do that with `nnmaildir`, but the more common
configuration is to have your mail delivered directly to the maildirs
that appear as group in Gnus.

`nnmaildir` is designed to be perfectly reliable: `C-g` will
never corrupt its data in memory, and `SIGKILL` will never
corrupt its data in the filesystem.

`nnmaildir` stores article marks and NOV data in each
maildir. So you can copy a whole maildir from one Gnus setup to
another, and you will keep your marks.

Virtual server settings:

`directory`
:   For each of your `nnmaildir` servers (it’s very unlikely that
    you’d need more than one), you need to create a directory and populate
    it with maildirs or symlinks to maildirs (and nothing else; do not
    choose a directory already used for other purposes). Each maildir
    will be represented in Gnus as a newsgroup on that server; the
    filename of the symlink will be the name of the group. Any filenames
    in the directory starting with ‘`.`’ are ignored. The directory is
    scanned when you first start Gnus, and each time you type `g` in
    the group buffer; if any maildirs have been removed or added,
    `nnmaildir` notices at these times.

    The value of the `directory` parameter should be a Lisp form
    which is processed by `eval` and `expand-file-name` to get
    the path of the directory for this server. The form is `eval`ed
    only when the server is opened; the resulting string is used until the
    server is closed. (If you don’t know about forms and `eval`,
    don’t worry—a simple string will work.) This parameter is not
    optional; you must specify it. I don’t recommend using
    `"~/Mail"` or a subdirectory of it; several other parts of Gnus
    use that directory by default for various things, and may get confused
    if `nnmaildir` uses it too. `"~/.nnmaildir"` is a typical
    value.

`target-prefix`
:   This should be a Lisp form which is processed by `eval` and
    `expand-file-name`. The form is `eval`ed only when the
    server is opened; the resulting string is used until the server is
    closed.

    When you create a group on an `nnmaildir` server, the maildir is
    created with `target-prefix` prepended to its name, and a symlink
    pointing to that maildir is created, named with the plain group name.
    So if `directory` is `"~/.nnmaildir"` and
    `target-prefix` is `"../maildirs/"`, then when you create
    the group `foo`, `nnmaildir` will create
    `~/.nnmaildir/../maildirs/foo` as a maildir, and will create
    `~/.nnmaildir/foo` as a symlink pointing to
    `../maildirs/foo`.

    You can set `target-prefix` to a string without any slashes to
    create both maildirs and symlinks in the same `directory`; in
    this case, any maildirs found in `directory` whose names start
    with `target-prefix` will not be listed as groups (but the
    symlinks pointing to them will be).

    As a special case, if `target-prefix` is `""` (the default),
    then when you create a group, the maildir will be created in
    `directory` without a corresponding symlink. Beware that you
    cannot use `gnus-group-delete-group` on such groups without the
    `force` argument.
