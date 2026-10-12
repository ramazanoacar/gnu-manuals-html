### 6.9 Signing and encrypting [¶](#Signing-and-encrypting-1)

Gnus can digitally sign and encrypt your messages, using vanilla
PGP format or PGP/MIME or S/MIME. For
decoding such messages, see the `mm-verify-option` and
`mm-decrypt-option` options (see [Security](#Security)).

Often, you would like to sign replies to people who send you signed
messages. Even more often, you might want to encrypt messages which
are in reply to encrypted messages. Gnus offers
`gnus-message-replysign` to enable the former, and
`gnus-message-replyencrypt` for the latter. In addition, setting
`gnus-message-replysignencrypted` (on by default) will sign
automatically encrypted messages.

Instructing MML to perform security operations on a
MIME part is done using the `C-c C-m s` key map for
signing and the `C-c C-m c` key map for encryption, as follows.

`C-c C-m s s` [¶](#index-C_002dc-C_002dm-s-s-_0028Message_0029)
:   Digitally sign current message using S/MIME.

`C-c C-m s o` [¶](#index-C_002dc-C_002dm-s-o-_0028Message_0029)
:   Digitally sign current message using PGP.

`C-c C-m s p` [¶](#index-C_002dc-C_002dm-s-p-_0028Message_0029)
:   Digitally sign current message using PGP/MIME.

`C-c C-m c s` [¶](#index-C_002dc-C_002dm-c-s-_0028Message_0029)
:   Digitally encrypt current message using S/MIME.

`C-c C-m c o` [¶](#index-C_002dc-C_002dm-c-o-_0028Message_0029)
:   Digitally encrypt current message using PGP.

`C-c C-m c p` [¶](#index-C_002dc-C_002dm-c-p-_0028Message_0029)
:   Digitally encrypt current message using PGP/MIME.

`C-c C-m C-n` [¶](#index-C_002dc-C_002dm-C_002dn-_0028Message_0029)
:   Remove security related MML tags from message.

See [Security](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Security) in Message Manual, for more information.

---

Next: [Scoring](#Scoring), Previous: [Composing Messages](#Composing-Messages), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 7 Select Methods [¶](#Select-Methods-1)

A *foreign group* is a group not read by the usual (or
default) means. It could be, for instance, a group from a different
NNTP server, it could be a virtual group, or it could be your own
personal mail group.

A foreign group (or any group, really) is specified by a *name* and
a *select method*. To take the latter first, a select method is a
list where the first element says what back end to use (e.g., `nntp`,
`nnspool`, `nnml`) and the second element is the *server
name*. There may be additional elements in the select method, where the
value may have special meaning for the back end in question.

One could say that a select method defines a *virtual server*—so
we do just that (see [Server Buffer](#Server-Buffer)).

The *name* of the group is the name the back end will recognize the
group as.

For instance, the group ‘`soc.motss`’ on the NNTP server
‘`some.where.edu`’ will have the name ‘`soc.motss`’ and select
method `(nntp "some.where.edu")`. Gnus will call this group
‘`nntp+some.where.edu:soc.motss`’, even though the `nntp`
back end just knows this group as ‘`soc.motss`’.

The different methods all have their peculiarities, of course.

* [Server Buffer](#Server-Buffer)
* [Getting News](#Getting-News)
* [Using IMAP](#Using-IMAP)
* [Getting Mail](#Getting-Mail)
* [Browsing the Web](#Browsing-the-Web)
* [Other Sources](#Other-Sources)
* [Virtual Groups](#Virtual-Groups)
* [Email Based Diary](#Email-Based-Diary)
* [Gnus Unplugged](#Gnus-Unplugged)

---

Next: [Getting News](#Getting-News), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.1 Server Buffer [¶](#Server-Buffer-1)

Traditionally, a *server* is a machine or a piece of software that
one connects to, and then requests information from. Gnus does not
connect directly to any real servers, but does all transactions through
one back end or other. But that’s just putting one layer more between
the actual media and Gnus, so we might just as well say that each
back end represents a virtual server.

For instance, the `nntp` back end may be used to connect to several
different actual NNTP servers, or, perhaps, to many different ports
on the same actual NNTP server. You tell Gnus which back end to
use, and what parameters to set by specifying a *select method*.

These select method specifications can sometimes become quite
complicated—say, for instance, that you want to read from the
NNTP server ‘`news.funet.fi`’ on port number 13, which
hangs if queried for NOV headers and has a buggy select. Ahem.
Anyway, if you had to specify that for each group that used this
server, that would be too much work, so Gnus offers a way of naming
select methods, which is what you do in the server buffer.

To enter the server buffer, use the `^`
(`gnus-group-enter-server-mode`) command in the group buffer.

`gnus-server-mode-hook` is run when creating the server buffer.

* [Server Buffer Format](#Server-Buffer-Format)
* [Server Commands](#Server-Commands)
* [Example Methods](#Example-Methods)
* [Creating a Virtual Server](#Creating-a-Virtual-Server)
* [Server Variables](#Server-Variables)
* [Servers and Methods](#Servers-and-Methods)
* [Unavailable Servers](#Unavailable-Servers)

---

Next: [Server Commands](#Server-Commands), Up: [Server Buffer](#Server-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.1 Server Buffer Format [¶](#Server-Buffer-Format-1)

You can change the look of the server buffer lines by changing the
`gnus-server-line-format` variable. This is a `format`-like
variable, with some simple extensions:

‘`h`’
:   How the news is fetched—the back end name.

‘`n`’
:   The name of this server.

‘`w`’
:   Where the news is to be fetched from—the address.

‘`s`’
:   The opened/closed/denied status of the server.

‘`a`’
:   Whether this server is agentized.

The mode line can also be customized by using the
`gnus-server-mode-line-format` variable (see [Mode Line Formatting](#Mode-Line-Formatting)). The following specs are understood:

‘`S`’
:   Server name.

‘`M`’
:   Server method.

Also see [Formatting Variables](#Formatting-Variables).

---

Next: [Example Methods](#Example-Methods), Previous: [Server Buffer Format](#Server-Buffer-Format), Up: [Server Buffer](#Server-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.2 Server Commands [¶](#Server-Commands-1)

The following key bindings are available in the server buffer. Be aware
that some of the commands will only work on servers that you’ve added
through this interface (with `a`), not with servers you’ve defined
in your init files.

`v` [¶](#index-v-_0028Server_0029)
:   The key `v` is reserved for users. You can bind it to some
    command or better use it as a prefix key.

`a` [¶](#index-a-_0028Server_0029)
:   Add a new server (`gnus-server-add-server`).

`e` [¶](#index-e-_0028Server_0029)
:   Edit a server (`gnus-server-edit-server`).

`S` [¶](#index-S-_0028Server_0029)
:   Show the definition of a server (`gnus-server-show-server`).

`SPC` [¶](#index-SPC-_0028Server_0029)
:   Browse the current server (`gnus-server-read-server`).

`q` [¶](#index-q-_0028Server_0029)
:   Return to the group buffer (`gnus-server-exit`).

`k` [¶](#index-k-_0028Server_0029)
:   Kill the current server (`gnus-server-kill-server`).

`y` [¶](#index-y-_0028Server_0029)
:   Yank the previously killed server (`gnus-server-yank-server`).

`c` [¶](#index-c-_0028Server_0029)
:   Copy the current server (`gnus-server-copy-server`).

`l` [¶](#index-l-_0028Server_0029)
:   List all servers (`gnus-server-list-servers`).

`s` [¶](#index-s-_0028Server_0029)
:   Request that the server scan its sources for new articles
    (`gnus-server-scan-server`). This is mainly sensible with mail
    servers.

`g` [¶](#index-g-_0028Server_0029)
:   Request that the server regenerate all its data structures
    (`gnus-server-regenerate-server`). This can be useful if you have
    a mail back end that has gotten out of sync.

`z` [¶](#index-z-_0028Server_0029)
:   Compact all groups in the server under point
    (`gnus-server-compact-server`). Currently implemented only in
    nnml (see [Mail Spool](#Mail-Spool)). This removes gaps between article numbers,
    hence getting a correct total article count.

Some more commands for closing, disabling, and re-opening servers are
listed in [Unavailable Servers](#Unavailable-Servers).

---

Next: [Creating a Virtual Server](#Creating-a-Virtual-Server), Previous: [Server Commands](#Server-Commands), Up: [Server Buffer](#Server-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.3 Example Methods [¶](#Example-Methods-1)

Most select methods are pretty simple and self-explanatory:

```
(nntp "news.funet.fi")
```

Reading directly from the spool is even simpler:

```
(nnspool "")
```

As you can see, the first element in a select method is the name of the
back end, and the second is the *address*, or *name*, if you
will.

After these two elements, there may be an arbitrary number of
`(variable form)` pairs.

To go back to the first example—imagine that you want to read from
port 15 on that machine. This is what the select method should
look like then:

```
(nntp "news.funet.fi" (nntp-port-number 15))
```

You should read the documentation to each back end to find out what
variables are relevant, but here’s an `nnmh` example:

`nnmh` is a mail back end that reads a spool-like structure. Say
you have two structures that you wish to access: One is your private
mail spool, and the other is a public one. Here’s the possible spec for
your private mail:

```
(nnmh "private" (nnmh-directory "~/private/mail/"))
```

(This server is then called ‘`private`’, but you may have guessed
that.)

Here’s the method for a public spool:

```
(nnmh "public"
      (nnmh-directory "/usr/information/spool/")
      (nnmh-get-new-mail nil))
```

If you are behind a firewall and only have access to the NNTP
server from the firewall machine, you can instruct Gnus to `rlogin`
on the firewall machine and connect with
[netcat](https://netcat.sourceforge.net/) from there to the
NNTP server.
Doing this can be rather fiddly, but your virtual server definition
should probably look something like this:

```
(nntp "firewall"
      (nntp-open-connection-function nntp-open-via-rlogin-and-netcat)
      (nntp-via-address "the.firewall.machine")
      (nntp-address "the.real.nntp.host"))
```

If you want to use the wonderful `ssh` program to provide a
compressed connection over the modem line, you could add the following
configuration to the example above:

```
      (nntp-via-rlogin-command "ssh")
```

See also `nntp-via-rlogin-command-switches`. Here’s an example for
an indirect connection:

```
(setq gnus-select-method
      '(nntp "indirect"
             (nntp-address "news.server.example")
             (nntp-via-user-name "intermediate_user_name")
             (nntp-via-address "intermediate.host.example")
             (nntp-via-rlogin-command "ssh")
             (nntp-via-rlogin-command-switches ("-C"))
             (nntp-open-connection-function nntp-open-via-rlogin-and-netcat)))
```

This means that you have to have set up `ssh-agent` correctly to
provide automatic authorization, of course.

If you’re behind a firewall, but have direct access to the outside world
through a wrapper command like "runsocks", you could open a socksified
netcat connection to the news server as follows:

```
(nntp "outside"
      (nntp-pre-command "runsocks")
      (nntp-open-connection-function nntp-open-netcat-stream)
      (nntp-address "the.news.server"))
```

---

Next: [Server Variables](#Server-Variables), Previous: [Example Methods](#Example-Methods), Up: [Server Buffer](#Server-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.4 Creating a Virtual Server [¶](#Creating-a-Virtual-Server-1)

If you’re saving lots of articles in the cache by using persistent
articles, you may want to create a virtual server to read the cache.

First you need to add a new server. The `a` command does that. It
would probably be best to use `nnml` to read the cache. You
could also use `nnspool` or `nnmh`, though.

Type `a nnml RET cache RET`.

You should now have a brand new `nnml` virtual server called
‘`cache`’. You now need to edit it to have the right definitions.
Type `e` to edit the server. You’ll be entered into a buffer that
will contain the following:

```
(nnml "cache")
```

Change that to:

```
(nnml "cache"
         (nnml-directory "~/News/cache/")
         (nnml-active-file "~/News/cache/active"))
```

Type `C-c C-c` to return to the server buffer. If you now press
`RET` over this virtual server, you should be entered into a browse
buffer, and you should be able to enter any of the groups displayed.

---

Next: [Servers and Methods](#Servers-and-Methods), Previous: [Creating a Virtual Server](#Creating-a-Virtual-Server), Up: [Server Buffer](#Server-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.5 Server Variables [¶](#Server-Variables-1)

One sticky point when defining variables (both on back ends and in Emacs
in general) is that some variables are typically initialized from other
variables when the definition of the variables is being loaded. If you
change the “base” variable after the variables have been loaded, you
won’t change the “derived” variables.

This typically affects directory and file variables. For instance,
`nnml-directory` is `~/Mail/` by default, and all `nnml`
directory variables are initialized from that variable, so
`nnml-active-file` will be `~/Mail/active`. If you define a
new virtual `nnml` server, it will *not* suffice to set just
`nnml-directory`—you have to explicitly set all the file
variables to be what you want them to be. For a complete list of
variables for each back end, see each back end’s section later in this
manual, but here’s an example `nnml` definition:

```
(nnml "public"
      (nnml-directory "~/my-mail/")
      (nnml-active-file "~/my-mail/active")
      (nnml-newsgroups-file "~/my-mail/newsgroups"))
```

Server variables are often called *server parameters*.

---

Next: [Unavailable Servers](#Unavailable-Servers), Previous: [Server Variables](#Server-Variables), Up: [Server Buffer](#Server-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.6 Servers and Methods [¶](#Servers-and-Methods-1)

Wherever you would normally use a select method
(e.g., `gnus-secondary-select-method`, in the group select method,
when browsing a foreign server) you can use a virtual server name
instead. This could potentially save lots of typing. And it’s nice all
over.

---

Previous: [Servers and Methods](#Servers-and-Methods), Up: [Server Buffer](#Server-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.1.7 Unavailable Servers [¶](#Unavailable-Servers-1)

If a server seems to be unreachable, Gnus will mark that server as
`denied`. That means that any subsequent attempt to make contact
with that server will just be ignored. “It can’t be opened,” Gnus
will tell you, without making the least effort to see whether that is
actually the case or not.

That might seem quite naughty, but it does make sense most of the time.
Let’s say you have 10 groups subscribed to on server
‘`nephelococcygia.com`’. This server is located somewhere quite far
away from you and the machine is quite slow, so it takes 1 minute just
to find out that it refuses connection to you today. If Gnus were to
attempt to do that 10 times, you’d be quite annoyed, so Gnus won’t
attempt to do that. Once it has gotten a single “connection refused”,
it will regard that server as “down”.

So, what happens if the machine was only feeling unwell temporarily?
How do you test to see whether the machine has come up again?

You jump to the server buffer (see [Server Buffer](#Server-Buffer)) and poke it
with the following commands:

`O` [¶](#index-O-_0028Server_0029)
:   Try to establish connection to the server on the current line
    (`gnus-server-open-server`).

`C` [¶](#index-C-_0028Server_0029)
:   Close the connection (if any) to the server
    (`gnus-server-close-server`).

`D` [¶](#index-D-_0028Server_0029)
:   Mark the current server as unreachable
    (`gnus-server-deny-server`). This will effectively disable the
    server.

`M-o` [¶](#index-M_002do-_0028Server_0029)
:   Open the connections to all servers in the buffer
    (`gnus-server-open-all-servers`).

`M-c` [¶](#index-M_002dc-_0028Server_0029)
:   Close the connections to all servers in the buffer
    (`gnus-server-close-all-servers`).

`R` [¶](#index-R-_0028Server_0029)
:   Remove all marks to whether Gnus was denied connection from any servers
    (`gnus-server-remove-denials`).

`c` [¶](#index-c-_0028Server_0029-1)
:   Copy a server and give it a new name
    (`gnus-server-copy-server`). This can be useful if you have a
    complex method definition, and want to use the same definition towards
    a different (physical) server.

`L` [¶](#index-L-_0028Server_0029)
:   Set server status to offline (`gnus-server-offline-server`).

---

Next: [Using IMAP](#Using-IMAP), Previous: [Server Buffer](#Server-Buffer), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.2 Getting News [¶](#Getting-News-1)

A newsreader is normally used for reading news. Gnus currently provides
only two methods of getting news—it can read from an NNTP server,
or it can read from a local spool.

* [NNTP](#NNTP)
* [News Spool](#News-Spool)

---

Next: [News Spool](#News-Spool), Up: [Getting News](#Getting-News)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.2.1 NNTP [¶](#NNTP-1)

Subscribing to a foreign group from an NNTP server is rather easy.
You just specify `nntp` as method and the address of the NNTP
server as the, uhm, address.

If the NNTP server is located at a non-standard port, setting the
third element of the select method to this port number should allow you
to connect to the right port. You’ll have to edit the group info for
that (see [Foreign Groups](#Foreign-Groups)).

The name of the foreign group can be the same as a native group. In
fact, you can subscribe to the same group from as many different servers
you feel like. There will be no name collisions.

The following variables can be used to create a virtual `nntp`
server:

`nntp-server-opened-hook` [¶](#index-nntp_002dserver_002dopened_002dhook)
:   is run after a connection has been made. It can be used to send
    commands to the NNTP server after it has been contacted. By
    default it sends the command `MODE READER` to the server with the
    `nntp-send-mode-reader` function. This function should always be
    present in this hook.

`nntp-authinfo-function` [¶](#index-nntp_002dauthinfo_002dfunction)
:   This function will be used to send ‘`AUTHINFO`’ to the NNTP
    server. The default function is `nntp-send-authinfo`, which looks
    through your `~/.authinfo` for applicable entries. If none
    are found, it will prompt you for a login name and a password. The
    format of the `~/.authinfo` file is (almost) the same as the
    `ftp` `~/.netrc` file, which is defined in the `ftp`
    manual page, but here are the salient facts:

    1. The file contains one or more line, each of which define one server.
    2. Each line may contain an arbitrary number of token/value pairs.

       The valid tokens include ‘`machine`’, ‘`login`’, ‘`password`’,
       ‘`default`’. In addition Gnus introduces two new tokens, not present
       in the original `.netrc`/`ftp` syntax, namely ‘`port`’ and
       ‘`force`’. (This is the only way the `.authinfo` file format
       deviates from the `.netrc` file format.) ‘`port`’ is used to
       indicate what port on the server the credentials apply to and
       ‘`force`’ is explained below.

    Here’s an example file:

    ```
    machine news.uio.no login larsi password geheimnis
    machine nntp.ifi.uio.no login larsi force yes
    ```

    The token/value pairs may appear in any order; ‘`machine`’ doesn’t
    have to be first, for instance.

    In this example, both login name and password have been supplied for the
    former server, while the latter has only the login name listed, and the
    user will be prompted for the password. The latter also has the
    ‘`force`’ tag, which means that the authinfo will be sent to the
    nntp server upon connection; the default (i.e., when there is not
    ‘`force`’ tag) is to not send authinfo to the nntp server
    until the nntp server asks for it.

    You can also add ‘`default`’ lines that will apply to all servers
    that don’t have matching ‘`machine`’ lines.

    ```
    default force yes
    ```

    This will force sending ‘`AUTHINFO`’ commands to all servers not
    previously mentioned.

    Remember to not leave the `~/.authinfo` file world-readable.

`nntp-server-action-alist` [¶](#index-nntp_002dserver_002daction_002dalist)
:   This is a list of regexps to match on server types and actions to be
    taken when matches are made. For instance, if you want Gnus to beep
    every time you connect to innd, you could say something like:

    ```
    (setq nntp-server-action-alist
          '(("innd" (ding))))
    ```

    You probably don’t want to do that, though.

    The default value is

    ```
    '(("nntpd 1\\.5\\.11t"
       (remove-hook 'nntp-server-opened-hook
                    'nntp-send-mode-reader)))
    ```

    This ensures that Gnus doesn’t send the `MODE READER` command to
    nntpd 1.5.11t, since that command chokes that server, I’ve been told.

`nntp-maximum-request` [¶](#index-nntp_002dmaximum_002drequest)
:   If the NNTP server doesn’t support NOV headers, this back end
    will collect headers by sending a series of `head` commands. To
    speed things up, the back end sends lots of these commands without
    waiting for reply, and then reads all the replies. This is controlled
    by the `nntp-maximum-request` variable, and is 400 by default. If
    your network is buggy, you should set this to 1.

`nntp-connection-timeout` [¶](#index-nntp_002dconnection_002dtimeout)
:   If you have lots of foreign `nntp` groups that you connect to
    regularly, you’re sure to have problems with NNTP servers not
    responding properly, or being too loaded to reply within reasonable
    time. This is can lead to awkward problems, which can be helped
    somewhat by setting `nntp-connection-timeout`. This is an integer
    that says how many seconds the `nntp` back end should wait for a
    connection before giving up. If it is `nil`, which is the default,
    no timeouts are done.

`nntp-nov-is-evil` [¶](#index-nntp_002dnov_002dis_002devil)
:   If the NNTP server does not support NOV, you could set this
    variable to `t`, but `nntp` usually checks automatically whether NOV
    can be used.

`nntp-xover-commands` [¶](#index-nntp_002dxover_002dcommands)
:   List of strings used as commands to fetch NOV lines from a
    server. The default value of this variable is `("XOVER"
    "XOVERVIEW")`.

`nntp-nov-gap` [¶](#index-nntp_002dnov_002dgap)
:   `nntp` normally sends just one big request for NOV lines to
    the server. The server responds with one huge list of lines. However,
    if you have read articles 2–5000 in the group, and only want to read
    article 1 and 5001, that means that `nntp` will fetch 4999 NOV
    lines that you will not need. This variable says how
    big a gap between two consecutive articles is allowed to be before the
    `XOVER` request is split into several request. Note that if your
    network is fast, setting this variable to a really small number means
    that fetching will probably be slower. If this variable is `nil`,
    `nntp` will never split requests. The default is 5.

`nntp-xref-number-is-evil` [¶](#index-nntp_002dxref_002dnumber_002dis_002devil)
:   When Gnus refers to an article having the `Message-ID` that a user
    specifies or having the `Message-ID` of the parent article of the
    current one (see [Finding the Parent](#Finding-the-Parent)), Gnus sends a `HEAD`
    command to the NNTP server to know where it is, and the server
    returns the data containing the pairs of a group and an article number
    in the `Xref` header. Gnus normally uses the article number to
    refer to the article if the data shows that that article is in the
    current group, while it uses the `Message-ID` otherwise. However,
    some news servers, e.g., ones running Diablo, run multiple engines
    having the same articles but article numbers are not kept synchronized
    between them. In that case, the article number that appears in the
    `Xref` header varies by which engine is chosen, so you cannot refer
    to the parent article that is in the current group, for instance. If
    you connect to such a server, set this variable to a non-`nil`
    value, and Gnus never uses article numbers. For example:

    ```
    (setq gnus-select-method
          '(nntp "newszilla"
                 (nntp-address "newszilla.example.com")
                 (nntp-xref-number-is-evil t)
                 ...))
    ```

    The default value of this server variable is `nil`.

`nntp-prepare-server-hook` [¶](#index-nntp_002dprepare_002dserver_002dhook)
:   A hook run before attempting to connect to an NNTP server.

`nntp-record-commands` [¶](#index-nntp_002drecord_002dcommands)
:   If non-`nil`, `nntp` will log all commands it sends to the
    NNTP server (along with a timestamp) in the `*nntp-log*`
    buffer. This is useful if you are debugging a Gnus/NNTP connection
    that doesn’t seem to work.

`nntp-open-connection-function` [¶](#index-nntp_002dopen_002dconnection_002dfunction)
:   It is possible to customize how the connection to the nntp server will
    be opened. If you specify an `nntp-open-connection-function`
    parameter, Gnus will use that function to establish the connection.
    Seven pre-made functions are supplied. These functions can be grouped
    in two categories: direct connection functions (four pre-made), and
    indirect ones (three pre-made).

`nntp-never-echoes-commands` [¶](#index-nntp_002dnever_002dechoes_002dcommands)
:   Non-`nil` means the nntp server never echoes commands. It is
    reported that some nntps server doesn’t echo commands. So, you may want
    to set this to non-`nil` in the method for such a server setting
    `nntp-open-connection-function` to `nntp-open-tls-stream` for
    example. The default value is `nil`. Note that the
    `nntp-open-connection-functions-never-echo-commands` variable
    overrides the `nil` value of this variable.

`nntp-open-connection-functions-never-echo-commands` [¶](#index-nntp_002dopen_002dconnection_002dfunctions_002dnever_002decho_002dcommands)
:   List of functions that never echo commands. Add or set a function which
    you set to `nntp-open-connection-function` to this list if it does
    not echo commands. Note that a non-`nil` value of the
    `nntp-never-echoes-commands` variable overrides this variable. The
    default value is `(nntp-open-network-stream)`.

`nntp-prepare-post-hook` [¶](#index-nntp_002dprepare_002dpost_002dhook)
:   A hook run just before posting an article. If there is no
    `Message-ID` header in the article and the news server provides the
    recommended ID, it will be added to the article before running this
    hook. It is useful to make `Cancel-Lock` headers even if you
    inhibit Gnus to add a `Message-ID` header, you could say:

    ```
    (add-hook 'nntp-prepare-post-hook 'canlock-insert-header)
    ```

    Note that not all servers support the recommended ID. This works for
    INN versions 2.3.0 and later, for instance.

`nntp-server-list-active-group`
:   If `nil`, then always use ‘`GROUP`’ instead of ‘`LIST
    ACTIVE`’. This is usually slower, but on misconfigured servers that
    don’t update their active files often, this can help.

* [Direct Functions](#Direct-Functions)
* [Indirect Functions](#Indirect-Functions)
* [Common Variables](#Common-Variables)

---

Next: [Indirect Functions](#Indirect-Functions), Up: [NNTP](#NNTP)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.2.1.1 Direct Functions [¶](#Direct-Functions-1)

These functions are called direct because they open a direct connection
between your machine and the NNTP server. The behavior of these
functions is also affected by commonly understood variables
(see [Common Variables](#Common-Variables)).

`nntp-open-network-stream` [¶](#index-nntp_002dopen_002dnetwork_002dstream)
:   This is the default, and simply connects to some port or other on the
    remote system. If both Emacs and the server supports it, the connection
    will be upgraded to an encrypted STARTTLS connection
    automatically. If you want to avoid the possibility of a malicious
    intermediary blocking the use of STARTTLS, use
    `nntp-open-tls-stream` instead.

`nntp-open-plain-stream`

`network-only`
:   The same as the above, but don’t do automatic STARTTLS
    upgrades. Only use this if you want anyone to be able to read your
    traffic.

`nntp-open-tls-stream` [¶](#index-nntp_002dopen_002dtls_002dstream)
:   Opens a connection to a server over a *secure* channel. To use
    this, your Emacs must have been compiled with GnuTLS support
    [GnuTLS](https://www.gnu.org/software/gnutls/). You can check this
    using the `gnutls-available-p` command.

    You then define a server as follows:

    ```
    ;; "nntps" is port 563 and is predefined in our /etc/services
    ;; however, ‘gnutls-cli -p’ doesn’t like named ports.
    ;;
    (nntp "snews.bar.com"
          (nntp-open-connection-function nntp-open-tls-stream)
          (nntp-port-number 563)
          (nntp-address "snews.bar.com"))
    ```

`nntp-open-ssl-stream` [¶](#index-nntp_002dopen_002dssl_002dstream)
:   This is the old name for `nntp-open-tls-stream`, and is
    completely equivalent.

`nntp-open-netcat-stream` [¶](#index-nntp_002dopen_002dnetcat_002dstream)
:   Opens a connection to an NNTP server using the `netcat`
    program. You might wonder why this function exists, since we have
    the default `nntp-open-network-stream` which would do the job. (One
    of) the reason(s) is that if you are behind a firewall but have direct
    connections to the outside world thanks to a command wrapper like
    `runsocks`, you can use it like this:

    ```
    (nntp "socksified"
          (nntp-pre-command "runsocks")
          (nntp-open-connection-function nntp-open-netcat-stream)
          (nntp-address "the.news.server"))
    ```

    With the default method, you would need to wrap your whole Emacs
    session, which is not a good idea.

`nntp-open-telnet-stream` [¶](#index-nntp_002dopen_002dtelnet_002dstream)
:   Like `nntp-open-netcat-stream`, but uses `telnet` rather than
    `netcat`. `telnet` is a bit less robust because of things
    like line-end-conversion, but sometimes netcat is simply
    not available. The previous example would turn into:

    ```
    (nntp "socksified"
          (nntp-pre-command "runsocks")
          (nntp-open-connection-function nntp-open-telnet-stream)
          (nntp-address "the.news.server")
          (nntp-end-of-line "\n"))
    ```

---

Next: [Common Variables](#Common-Variables), Previous: [Direct Functions](#Direct-Functions), Up: [NNTP](#NNTP)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.2.1.2 Indirect Functions [¶](#Indirect-Functions-1)

These functions are called indirect because they connect to an
intermediate host before actually connecting to the NNTP server.
All of these functions and related variables are also said to belong to
the “via” family of connection: they’re all prefixed with “via” to make
things cleaner. The behavior of these functions is also affected by
commonly understood variables (see [Common Variables](#Common-Variables)).

`nntp-open-via-rlogin-and-netcat` [¶](#index-nntp_002dopen_002dvia_002drlogin_002dand_002dnetcat)
:   Does an ‘`rlogin`’ on a remote system, and then uses `netcat` to connect
    to the real NNTP server from there. This is useful for instance if
    you need to connect to a firewall machine first.

    `nntp-open-via-rlogin-and-netcat`-specific variables:

    `nntp-via-rlogin-command` [¶](#index-nntp_002dvia_002drlogin_002dcommand)
    :   Command used to log in on the intermediate host. The default is
        ‘`rsh`’, but ‘`ssh`’ is a popular alternative.

    `nntp-via-rlogin-command-switches` [¶](#index-nntp_002dvia_002drlogin_002dcommand_002dswitches)
    :   List of strings to be used as the switches to
        `nntp-via-rlogin-command`. The default is `nil`. If you use
        ‘`ssh`’ for `nntp-via-rlogin-command`, you may set this to
        ‘`("-C")`’ in order to compress all data connections.

`nntp-open-via-rlogin-and-telnet` [¶](#index-nntp_002dopen_002dvia_002drlogin_002dand_002dtelnet)
:   Does essentially the same, but uses `telnet` instead of ‘`netcat`’
    to connect to the real NNTP server from the intermediate host.
    `telnet` is a bit less robust because of things like
    line-end-conversion, but sometimes `netcat` is simply not available.

    `nntp-open-via-rlogin-and-telnet`-specific variables:

    `nntp-telnet-command` [¶](#index-nntp_002dtelnet_002dcommand)
    :   Command used to connect to the real NNTP server from the
        intermediate host. The default is ‘`telnet`’.

    `nntp-telnet-switches` [¶](#index-nntp_002dtelnet_002dswitches)
    :   List of strings to be used as the switches to the
        `nntp-telnet-command` command. The default is `("-8")`.

    `nntp-via-rlogin-command` [¶](#index-nntp_002dvia_002drlogin_002dcommand-1)
    :   Command used to log in on the intermediate host. The default is
        ‘`rsh`’, but ‘`ssh`’ is a popular alternative.

    `nntp-via-rlogin-command-switches` [¶](#index-nntp_002dvia_002drlogin_002dcommand_002dswitches-1)
    :   List of strings to be used as the switches to
        `nntp-via-rlogin-command`. If you use ‘`ssh`’, you may need to set
        this to ‘`("-t" "-e" "none")`’ or ‘`("-C" "-t" "-e" "none")`’ if
        the telnet command requires a pseudo-tty allocation on an intermediate
        host. The default is `nil`.

    Note that you may want to change the value for `nntp-end-of-line`
    to ‘`\n`’ (see [Common Variables](#Common-Variables)).

`nntp-open-via-telnet-and-telnet` [¶](#index-nntp_002dopen_002dvia_002dtelnet_002dand_002dtelnet)
:   Does essentially the same, but uses ‘`telnet`’ instead of
    ‘`rlogin`’ to connect to the intermediate host.

    `nntp-open-via-telnet-and-telnet`-specific variables:

    `nntp-via-telnet-command` [¶](#index-nntp_002dvia_002dtelnet_002dcommand)
    :   Command used to `telnet` the intermediate host. The default is
        ‘`telnet`’.

    `nntp-via-telnet-switches` [¶](#index-nntp_002dvia_002dtelnet_002dswitches)
    :   List of strings to be used as the switches to the
        `nntp-via-telnet-command` command. The default is ‘`("-8")`’.

    `nntp-via-user-password` [¶](#index-nntp_002dvia_002duser_002dpassword)
    :   Password to use when logging in on the intermediate host.

    `nntp-via-envuser` [¶](#index-nntp_002dvia_002denvuser)
    :   If non-`nil`, the intermediate `telnet` session (client and
        server both) will support the `ENVIRON` option and not prompt for
        login name. This works for Solaris `telnet`, for instance.

    `nntp-via-shell-prompt` [¶](#index-nntp_002dvia_002dshell_002dprompt)
    :   Regexp matching the shell prompt on the intermediate host. The default
        is ‘`bash\\|\$ *\r?$\\|> *\r?`’.

    Note that you may want to change the value for `nntp-end-of-line`
    to ‘`\n`’ (see [Common Variables](#Common-Variables)).

Here are some additional variables that are understood by all the above
functions:

`nntp-via-user-name` [¶](#index-nntp_002dvia_002duser_002dname)
:   User name to use when connecting to the intermediate host.

`nntp-via-address` [¶](#index-nntp_002dvia_002daddress)
:   Address of the intermediate host to connect to.

---

Previous: [Indirect Functions](#Indirect-Functions), Up: [NNTP](#NNTP)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.2.1.3 Common Variables [¶](#Common-Variables-1)

The following variables affect the behavior of all, or several of the
pre-made connection functions. When not specified, all functions are
affected (the values of the following variables will be used as the
default if each virtual `nntp` server doesn’t specify those server
variables individually).

`nntp-pre-command` [¶](#index-nntp_002dpre_002dcommand)
:   A command wrapper to use when connecting through a non native
    connection function (all except `nntp-open-network-stream`,
    `nntp-open-tls-stream`, and `nntp-open-ssl-stream`). This is
    where you would put a ‘`SOCKS`’ wrapper for instance.

`nntp-address` [¶](#index-nntp_002daddress)
:   The address of the NNTP server.

`nntp-port-number` [¶](#index-nntp_002dport_002dnumber)
:   Port number to connect to the NNTP server. The default is
    ‘`nntp`’. If you use NNTP over
    TLS/SSL, you may want to use integer ports rather
    than named ports (i.e., use ‘`563`’ instead of ‘`snews`’ or
    ‘`nntps`’), because external TLS/SSL tools may
    not work with named ports.

`nntp-end-of-line` [¶](#index-nntp_002dend_002dof_002dline)
:   String to use as end-of-line marker when talking to the NNTP
    server. This is ‘`\r\n`’ by default, but should be ‘`\n`’ when
    using a non native telnet connection function.

`nntp-netcat-command` [¶](#index-nntp_002dnetcat_002dcommand)
:   Command to use when connecting to the NNTP server through
    ‘`netcat`’. This is *not* for an intermediate host. This is
    just for the real NNTP server. The default is
    ‘`nc`’.

`nntp-netcat-switches` [¶](#index-nntp_002dnetcat_002dswitches)
:   A list of switches to pass to `nntp-netcat-command`. The default
    is ‘`()`’.

---

Previous: [NNTP](#NNTP), Up: [Getting News](#Getting-News)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.2.2 News Spool [¶](#News-Spool-1)

Subscribing to a foreign group from the local spool is extremely easy,
and might be useful, for instance, to speed up reading groups that
contain very big articles—‘`alt.binaries.pictures.furniture`’, for
instance.

Anyway, you just specify `nnspool` as the method and `""` (or
anything else) as the address.

If you have access to a local spool, you should probably use that as the
native select method (see [Finding the News](#Finding-the-News)). It is normally faster
than using an `nntp` select method, but might not be. It depends.
You just have to try to find out what’s best at your site.

`nnspool-inews-program` [¶](#index-nnspool_002dinews_002dprogram)
:   Program used to post an article.

`nnspool-inews-switches` [¶](#index-nnspool_002dinews_002dswitches)
:   Parameters given to the inews program when posting an article.

`nnspool-spool-directory` [¶](#index-nnspool_002dspool_002ddirectory)
:   Where `nnspool` looks for the articles. This is normally
    `/usr/spool/news/`.

`nnspool-nov-directory` [¶](#index-nnspool_002dnov_002ddirectory)
:   Where `nnspool` will look for NOV files. This is normally  
    `/usr/spool/news/over.view/`.

`nnspool-lib-dir` [¶](#index-nnspool_002dlib_002ddir)
:   Where the news lib dir is (`/usr/lib/news/` by default).

`nnspool-active-file` [¶](#index-nnspool_002dactive_002dfile)
:   The name of the active file.

`nnspool-newsgroups-file` [¶](#index-nnspool_002dnewsgroups_002dfile)
:   The name of the group descriptions file.

`nnspool-history-file` [¶](#index-nnspool_002dhistory_002dfile)
:   The name of the news history file.

`nnspool-active-times-file` [¶](#index-nnspool_002dactive_002dtimes_002dfile)
:   The name of the active date file.

`nnspool-nov-is-evil` [¶](#index-nnspool_002dnov_002dis_002devil)
:   If non-`nil`, `nnspool` won’t try to use any NOV files
    that it finds.

`nnspool-sift-nov-with-sed` [¶](#index-nnspool_002dsift_002dnov_002dwith_002dsed)
:   If non-`nil`, which is the default, use `sed` to get the
    relevant portion from the overview file. If `nil`,
    `nnspool` will load the entire file into a buffer and process it
    there.

---

Next: [Getting Mail](#Getting-Mail), Previous: [Getting News](#Getting-News), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 7.3 Using IMAP [¶](#Using-IMAP-1)

The most popular mail backend is probably `nnimap`, which
provides access to IMAP servers. IMAP servers
store mail remotely, so the client doesn’t store anything locally.
This means that it’s a convenient choice when you’re reading your mail
from different locations, or with different user agents.

* [Connecting to an IMAP Server](#Connecting-to-an-IMAP-Server)
* [Customizing the IMAP Connection](#Customizing-the-IMAP-Connection)
* [Client-Side IMAP Splitting](#Client_002dSide-IMAP-Splitting)
* [Support for IMAP Extensions](#Support-for-IMAP-Extensions)

---

Next: [Customizing the IMAP Connection](#Customizing-the-IMAP-Connection), Up: [Using IMAP](#Using-IMAP)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.3.1 Connecting to an IMAP Server [¶](#Connecting-to-an-IMAP-Server-1)

Connecting to an IMAP can be very easy. Type `B` in the
group buffer, or (if your primary interest is reading email), say
something like:

```
(setq gnus-select-method
      '(nnimap "imap.gmail.com"))
```

You’ll be prompted for a user name and password. If you grow tired of
that, then add the following to your `~/.authinfo` file:

```
machine imap.gmail.com login <username> password <password> port imap
```

That should basically be it for most users.

---

Next: [Client-Side IMAP Splitting](#Client_002dSide-IMAP-Splitting), Previous: [Connecting to an IMAP Server](#Connecting-to-an-IMAP-Server), Up: [Using IMAP](#Using-IMAP)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.3.2 Customizing the IMAP Connection [¶](#Customizing-the-IMAP-Connection-1)

Here’s an example method that’s more complex:

```
(nnimap "imap.gmail.com"
        (nnimap-inbox "INBOX")
        (nnimap-split-methods default)
        (nnimap-expunge t)
        (nnimap-stream tls))
```

`nnimap-address` [¶](#index-nnimap_002daddress)
:   The address of the server, like ‘`imap.gmail.com`’.

`nnimap-user` [¶](#index-nnimap_002duser)
:   Username to use for authentication to the IMAP server. This
    corresponds to the value of the ‘`login`’ token in your
    `~/.authinfo` file. Set this variable if you want to access
    multiple accounts from the same IMAP server.

`nnimap-server-port` [¶](#index-nnimap_002dserver_002dport)
:   If the server uses a non-standard port, that can be specified here. A
    typical port would be `"imap"` or `"imaps"`.

`nnimap-stream` [¶](#index-nnimap_002dstream)
:   How `nnimap` should connect to the server. Possible values are:

    `undecided`
    :   This is the default, and this first tries the `tls` setting, and
        then tries the `network` setting.

    `tls`
    :   This uses standard TLS/SSL connections. `ssl`
        is an equivalent but deprecated way to specify this.

    `network`
    :   Non-encrypted and unsafe straight socket connection, but will upgrade
        to encrypted STARTTLS if both Emacs and the server
        supports it.

    `starttls`
    :   Encrypted STARTTLS over the normal IMAP port.

    `shell`
    :   If you need to tunnel via other systems to connect to the server, you
        can use this option, and customize `nnimap-shell-program` to be
        what you need.

    `plain`
    :   Non-encrypted and unsafe straight socket connection.
        STARTTLS will not be used even if it is available.

`nnimap-authenticator` [¶](#index-nnimap_002dauthenticator)
:   Some IMAP servers allow anonymous logins. In that case,
    this should be set to `anonymous`. If this variable isn’t set,
    the normal login methods will be used. If you wish to specify a
    specific login method to be used, you can set this variable to either
    `login` (the traditional IMAP login method),
    `plain`, `cram-md5` or `xoauth2`. (The latter method
    requires using the `oauth2.el` library.)

`nnimap-expunge` [¶](#index-nnimap_002dexpunge)
:   When to expunge deleted messages. If `never`, deleted articles
    are marked with the IMAP `\\Delete` flag but not
    automatically expunged. If `immediately`, deleted articles are
    immediately expunged (this requires the server to support the
    `UID EXPUNGE` command). If `on-exit`, deleted articles are
    flagged, and all flagged articles are expunged when the group is
    closed.

    For backwards compatibility, this variable may also be set to `t`
    or `nil`. If the server supports `UID EXPUNGE`, both
    `t` and `nil` are equivalent to `immediately`. If the
    server does not support `UID EXPUNGE`, `nil` is equivalent
    to `never`, while `t` will immediately expunge *all*
    articles that are currently flagged as deleted (i.e., potentially not
    only the article that was just deleted).

`nnimap-streaming` [¶](#index-nnimap_002dstreaming)
:   Virtually all IMAP server support fast streaming of data.
    If you have problems connecting to the server, try setting this to
    `nil`.

`nnimap-fetch-partial-articles` [¶](#index-nnimap_002dfetch_002dpartial_002darticles)
:   If non-`nil`, fetch partial articles from the server. If set to
    a string, then it’s interpreted as a regexp, and parts that have
    matching types will be fetched. For instance, ‘`"text/"`’ will
    fetch all textual parts, while leaving the rest on the server.

`nnimap-record-commands` [¶](#index-nnimap_002drecord_002dcommands)
:   If non-`nil`, record all IMAP commands in the
    ‘`"*imap log*"`’ buffer.

`nnimap-use-namespaces` [¶](#index-nnimap_002duse_002dnamespaces)
:   If non-`nil`, omit the IMAP namespace prefix in nnimap group
    names. If your IMAP mailboxes are called something like ‘`INBOX`’
    and ‘`INBOX.Lists.emacs`’, but you’d like the nnimap group names to
    be ‘`INBOX`’ and ‘`Lists.emacs`’, you should enable this option.

`nnimap-keepalive-intervals` [¶](#index-nnimap_002dkeepalive_002dintervals)
:   By default, nnimap will send occasional ‘`NOOP`’ (keepalive)
    commands to the server, to keep the connection alive. This option
    governs how often that happens. It is a cons of two integers,
    representing seconds: first how often to run the keepalive check, and
    the second how many seconds of user inactivity are required to
    actually send the command. The default, `(900 . 300)`, means run
    the check every fifteen minutes and, if the user has been inactive for
    five minutes, send the keepalive command. Set to `nil` to
    disable keepalive commands altogether.

---

Next: [Support for IMAP Extensions](#Support-for-IMAP-Extensions), Previous: [Customizing the IMAP Connection](#Customizing-the-IMAP-Connection), Up: [Using IMAP](#Using-IMAP)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.3.3 Client-Side IMAP Splitting [¶](#Client_002dSide-IMAP-Splitting-1)

Many people prefer to do the sorting/splitting of mail into their mail
boxes on the IMAP server. That way they don’t have to
download the mail they’re not all that interested in.

If you do want to do client-side mail splitting, then the following
variables are relevant:

`nnimap-inbox`
:   This is the IMAP mail box that will be scanned for new
    mail. This can also be a list of mail box names.

`nnimap-split-methods`
:   Uses the same syntax as `nnmail-split-methods` (see [Splitting Mail](#Splitting-Mail)), except the symbol `default`, which means that it should
    use the value of the `nnmail-split-methods` variable.

`nnimap-split-fancy`
:   Uses the same syntax as `nnmail-split-fancy`.

`nnimap-unsplittable-articles`
:   List of flag symbols to ignore when doing splitting. That is,
    articles that have these flags won’t be considered when splitting.
    The default is ‘`(%Deleted %Seen)`’.

By default, the nnimap back end only retrieves the message headers;
the option `nnimap-split-download-body` (which is a regular
customization option, not a server variable) tells it to retrieve the
message bodies as well. We don’t set this by default because it will
slow IMAP down, and that is not an appropriate decision to
make on behalf of the user.

Here’s a complete example `nnimap` backend with a client-side
“fancy” splitting method:

```
(nnimap "imap.example.com"
        (nnimap-inbox "INBOX")
        (nnimap-split-fancy
         (| ("MailScanner-SpamCheck" "spam" "spam.detected")
            (to "foo@bar.com" "foo")
            "undecided")))
```

---

Previous: [Client-Side IMAP Splitting](#Client_002dSide-IMAP-Splitting), Up: [Using IMAP](#Using-IMAP)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.3.4 Support for IMAP Extensions [¶](#Support-for-IMAP-Extensions-1)

If you’re using Google’s Gmail, you may want to see your Gmail labels
when reading your mail. Gnus can give you this information if you ask
for ‘`X-GM-LABELS`’ in the variable `gnus-extra-headers`. For
example:

```
(setq gnus-extra-headers
      '(To Newsgroups X-GM-LABELS))
```

This will result in Gnus storing your labels in message header
structures for later use. The content is always a parenthesized
(possible empty) list.

---

Next: [Browsing the Web](#Browsing-the-Web), Previous: [Using IMAP](#Using-IMAP), Up: [Select Methods](#Select-Methods)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
