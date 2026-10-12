### 7.9 Gnus Unplugged [¶](#Gnus-Unplugged-1)

In olden times (ca. February ’88), people used to run their newsreaders
on big machines with permanent connections to the net. News transport
was dealt with by news servers, and all the newsreaders had to do was to
read news. Believe it or not.

Nowadays most people read news and mail at home, and use some sort of
modem to connect to the net. To avoid running up huge phone bills, it
would be nice to have a way to slurp down all the news and mail, hang up
the phone, read for several hours, and then upload any responses you
have to make. And then you repeat the procedure.

Of course, you can use news servers for doing this as well. I’ve used
`inn` together with `slurp`, `pop` and `sendmail`
for some years, but doing that’s a bore. Moving the news server
functionality up to the newsreader makes sense if you’re the only person
reading news on a machine.

Setting up Gnus as an “offline” newsreader is quite simple. In
fact, you don’t have to configure anything as the agent is now enabled
by default (see [gnus-agent](#Agent-Variables)).

Of course, to use it as such, you have to learn a few new commands.

* [Agent Basics](#Agent-Basics)
* [Agent Categories](#Agent-Categories)
* [Agent Commands](#Agent-Commands)
* [Agent Visuals](#Agent-Visuals)
* [Agent as Cache](#Agent-as-Cache)
* [Agent Expiry](#Agent-Expiry)
* [Agent Regeneration](#Agent-Regeneration)
* [Agent and flags](#Agent-and-flags)
* [Agent and IMAP](#Agent-and-IMAP)
* [Outgoing Messages](#Outgoing-Messages)
* [Agent Variables](#Agent-Variables)
* [Example Setup](#Example-Setup)
* [Batching Agents](#Batching-Agents)
* [Agent Caveats](#Agent-Caveats)

---

Next: [Agent Categories](#Agent-Categories), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.1 Agent Basics [¶](#Agent-Basics-1)

First, let’s get some terminology out of the way.

The Gnus Agent is said to be *unplugged* when you have severed the
connection to the net (and notified the Agent that this is the case).
When the connection to the net is up again (and Gnus knows this), the
Agent is *plugged*.

The *local* machine is the one you’re running on, and which isn’t
connected to the net continuously.

*Downloading* means fetching things from the net to your local
machine. *Uploading* is doing the opposite.

You know that Gnus gives you all the opportunity you’d ever want for
shooting yourself in the foot. Some people call it flexibility. Gnus
is also customizable to a great extent, which means that the user has a
say on how Gnus behaves. Other newsreaders might unconditionally shoot
you in your foot, but with Gnus, you have a choice!

Gnus is never really in plugged or unplugged state. Rather, it applies
that state to each server individually. This means that some servers
can be plugged while others can be unplugged. Additionally, some
servers can be ignored by the Agent altogether (which means that
they’re kinda like plugged always).

So when you unplug the Agent and then wonder why is Gnus opening a
connection to the Net, the next step to do is to look whether all
servers are agentized. If there is an unagentized server, you found
the culprit.

Another thing is the *offline* state. Sometimes, servers aren’t
reachable. When Gnus notices this, it asks you whether you want the
server to be switched to offline state. If you say yes, then the
server will behave somewhat as if it was unplugged, except that Gnus
will ask you whether you want to switch it back online again.

Let’s take a typical Gnus session using the Agent.

* You start Gnus with `gnus-unplugged`. This brings up the Gnus
  Agent in a disconnected state. You can read all the news that you have
  already fetched while in this mode.
* You then decide to see whether any new news has arrived. You connect
  your machine to the net (using PPP or whatever), and then hit `J j`
  to make Gnus become *plugged* and use `g` to check for new mail
  as usual. To check for new mail in unplugged mode (see [Mail Source Specifiers](#Mail-Source-Specifiers)).
* You can then read the new news immediately, or you can download the
  news onto your local machine. If you want to do the latter, you press
  `g` to check if there are any new news and then `J s` to fetch
  all the eligible articles in all the groups. (To let Gnus know which
  articles you want to download, see [Agent Categories](#Agent-Categories)).
* After fetching the articles, you press `J j` to make Gnus become
  unplugged again, and you shut down the PPP thing (or whatever). And
  then you read the news offline.
* And then you go to step 2.

Here are some things you should do the first time (or so) that you use
the Agent.

* Decide which servers should be covered by the Agent. If you have a mail
  back end, it would probably be nonsensical to have it covered by the
  Agent. Go to the server buffer (`^` in the group buffer) and press
  `J a` on the server (or servers) that you wish to have covered by the
  Agent (see [Server Agent Commands](#Server-Agent-Commands)), or `J r` on automatically
  added servers you do not wish to have covered by the Agent. By default,
  no servers are agentized.
* Decide on download policy. It’s fairly simple once you decide whether
  you are going to use agent categories, topic parameters, and/or group
  parameters to implement your policy. If you’re new to gnus, it
  is probably best to start with a category, See [Agent Categories](#Agent-Categories).

  Both topic parameters (see [Topic Parameters](#Topic-Parameters)) and agent categories
  (see [Agent Categories](#Agent-Categories)) provide for setting a policy that applies
  to multiple groups. Which you use is entirely up to you. Topic
  parameters do override categories so, if you mix the two, you’ll have
  to take that into account. If you have a few groups that deviate from
  your policy, you can use group parameters (see [Group Parameters](#Group-Parameters)) to
  configure them.
* Uhm… that’s it.

---

Next: [Agent Commands](#Agent-Commands), Previous: [Agent Basics](#Agent-Basics), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.2 Agent Categories [¶](#Agent-Categories-1)

One of the main reasons to integrate the news transport layer into the
newsreader is to allow greater control over what articles to download.
There’s not much point in downloading huge amounts of articles, just to
find out that you’re not interested in reading any of them. It’s better
to be somewhat more conservative in choosing what to download, and then
mark the articles for downloading manually if it should turn out that
you’re interested in the articles anyway.

One of the more effective methods for controlling what is to be
downloaded is to create a *category* and then assign some (or all)
groups to this category. Groups that do not belong in any other
category belong to the `default` category. Gnus has its own
buffer for creating and managing categories.

If you prefer, you can also use group parameters (see [Group Parameters](#Group-Parameters)) and topic parameters (see [Topic Parameters](#Topic-Parameters)) for an
alternative approach to controlling the agent. The only real
difference is that categories are specific to the agent (so there is
less to learn) while group and topic parameters include the kitchen
sink.

Since you can set agent parameters in several different places we have
a rule to decide which source to believe. This rule specifies that
the parameter sources are checked in the following order: group
parameters, topic parameters, agent category, and finally customizable
variables. So you can mix all of these sources to produce a wide range
of behavior, just don’t blame me if you don’t remember where you put
your settings.

* [Category Syntax](#Category-Syntax)
* [Category Buffer](#Category-Buffer)
* [Category Variables](#Category-Variables)

---

Next: [Category Buffer](#Category-Buffer), Up: [Agent Categories](#Agent-Categories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.2.1 Category Syntax [¶](#Category-Syntax-1)

A category consists of a name, the list of groups belonging to the
category, and a number of optional parameters that override the
customizable variables. The complete list of agent parameters are
listed below.

`agent-groups`
:   The list of groups that are in this category.

`agent-predicate`
:   A predicate which (generally) gives a rough outline of which articles
    are eligible for downloading; and

`agent-score`
:   a score rule which (generally) gives you a finer granularity when
    deciding what articles to download. (Note that this *download
    score* is not necessarily related to normal scores.)

`agent-enable-expiration`
:   a boolean indicating whether the agent should expire old articles in
    this group. Most groups should be expired to conserve disk space. In
    fact, its probably safe to say that the gnus.\* hierarchy contains the
    only groups that should not be expired.

`agent-days-until-old`
:   an integer indicating the number of days that the agent should wait
    before deciding that a read article is safe to expire.

`agent-low-score`
:   an integer that overrides the value of `gnus-agent-low-score`.

`agent-high-score`
:   an integer that overrides the value of `gnus-agent-high-score`.

`agent-short-article`
:   an integer that overrides the value of
    `gnus-agent-short-article`.

`agent-long-article`
:   an integer that overrides the value of `gnus-agent-long-article`.

`agent-enable-undownloaded-faces`
:   a symbol indicating whether the summary buffer should display
    undownloaded articles using the `gnus-summary-*-undownloaded-face`
    faces. Any symbol other than `nil` will enable the use of
    undownloaded faces.

The name of a category can not be changed once the category has been
created.

Each category maintains a list of groups that are exclusive members of
that category. The exclusivity rule is automatically enforced, add a
group to a new category and it is automatically removed from its old
category.

A predicate in its simplest form can be a single predicate such as
`true` or `false`. These two will download every available
article or nothing respectively. In the case of these two special
predicates an additional score rule is superfluous.

Predicates of `high` or `low` download articles in respect of
their scores in relationship to `gnus-agent-high-score` and
`gnus-agent-low-score` as described below.

To gain even finer control of what is to be regarded eligible for
download a predicate can consist of a number of predicates with logical
operators sprinkled in between.

Perhaps some examples are in order.

Here’s a simple predicate. (The default predicate is `false`, used
for all groups that don’t belong to any other category.)

```
short
```

Quite simple, eh? This predicate is true if and only if the article is
short (for some value of “short”).

Here’s a more complex predicate:

```
(or high
    (and
     (not low)
     (not long)))
```

This means that an article should be downloaded if it has a high score,
or if the score is not low and the article is not long. You get the
drift.

The available logical operators are `or`, `and` and
`not`. (If you prefer, you can use the more “C”-ish operators
‘`|`’, `&` and `!` instead.)

The following predicates are pre-defined, but if none of these fit what
you want to do, you can write your own.

When evaluating each of these predicates, the named constant will be
bound to the value determined by calling
`gnus-agent-find-parameter` on the appropriate parameter. For
example, gnus-agent-short-article will be bound to
`(gnus-agent-find-parameter group 'agent-short-article)`. This
means that you can specify a predicate in your category then tune that
predicate to individual groups.

`short`
:   True if the article is shorter than `gnus-agent-short-article`
    lines; default 100.

`long`
:   True if the article is longer than `gnus-agent-long-article`
    lines; default 200.

`low`
:   True if the article has a download score less than
    `gnus-agent-low-score`; default 0.

`high`
:   True if the article has a download score greater than
    `gnus-agent-high-score`; default 0.

`spam`
:   True if the Gnus Agent guesses that the article is spam. The
    heuristics may change over time, but at present it just computes a
    checksum and sees whether articles match.

`true`
:   Always true.

`false`
:   Always false.

If you want to create your own predicate function, here’s what you have
to know: The functions are called with no parameters, but the
`gnus-headers` and `gnus-score` dynamic variables are bound to
useful values.

For example, you could decide that you don’t want to download articles
that were posted more than a certain number of days ago (e.g., posted
more than `gnus-agent-expire-days` ago) you might write a function
something along the lines of the following:

```
(defun my-article-old-p ()
  "Say whether an article is old."
  (< (time-to-days (date-to-time (mail-header-date gnus-headers)))
     (- (time-to-days nil) gnus-agent-expire-days)))
```

with the predicate then defined as:

```
(not my-article-old-p)
```

or you could append your predicate to the predefined
`gnus-category-predicate-alist` in your `~/.gnus.el` or
wherever.

```
(require 'gnus-agent)
(setq  gnus-category-predicate-alist
  (append gnus-category-predicate-alist
         '((old . my-article-old-p))))
```

and simply specify your predicate as:

```
(not old)
```

If/when using something like the above, be aware that there are many
misconfigured systems/mailers out there and so an article’s date is not
always a reliable indication of when it was posted. Hell, some people
just don’t give a damn.

The above predicates apply to *all* the groups which belong to the
category. However, if you wish to have a specific predicate for an
individual group within a category, or you’re just too lazy to set up a
new category, you can enter a group’s individual predicate in its group
parameters like so:

```
(agent-predicate . short)
```

This is the group/topic parameter equivalent of the agent category default.
Note that when specifying a single word predicate like this, the
`agent-predicate` specification must be in dotted pair notation.

The equivalent of the longer example from above would be:

```
(agent-predicate or high (and (not low) (not long)))
```

The outer parenthesis required in the category specification are not
entered here as, not being in dotted pair notation, the value of the
predicate is assumed to be a list.

Now, the syntax of the download score is the same as the syntax of
normal score files, except that all elements that require actually
seeing the article itself are verboten. This means that only the
following headers can be scored on: `Subject`, `From`,
`Date`, `Message-ID`, `References`, `Chars`,
`Lines`, and `Xref`.

As with predicates, the specification of the `download score rule`
to use in respect of a group can be in either the category definition if
it’s to be applicable to all groups in therein, or a group’s parameters
if it’s to be specific to that group.

In both of these places the `download score rule` can take one of
three forms:

1. Score rule

   This has the same syntax as a normal Gnus score file except only a
   subset of scoring keywords are available as mentioned above.

   example:

   * Category specification

     ```
     (("from"
            ("Lars Ingebrigtsen" 1000000 nil s))
     ("lines"
            (500 -100 nil <)))
     ```
   * Group/Topic Parameter specification

     ```
     (agent-score ("from"
                        ("Lars Ingebrigtsen" 1000000 nil s))
                  ("lines"
                        (500 -100 nil <)))
     ```

     Again, note the omission of the outermost parenthesis here.
2. Agent score file

   These score files must *only* contain the permitted scoring
   keywords stated above.

   example:

   * Category specification

     ```
     ("~/News/agent.SCORE")
     ```

     or perhaps

     ```
     ("~/News/agent.SCORE" "~/News/agent.group.SCORE")
     ```
   * Group Parameter specification

     ```
     (agent-score "~/News/agent.SCORE")
     ```

     Additional score files can be specified as above. Need I say anything
     about parenthesis?
3. Use `normal` score files

   If you don’t want to maintain two sets of scoring rules for a group, and
   your desired `downloading` criteria for a group are the same as your
   `reading` criteria then you can tell the agent to refer to your
   `normal` score files when deciding what to download.

   These directives in either the category definition or a group’s
   parameters will cause the agent to read in all the applicable score
   files for a group, *filtering out* those sections that do not
   relate to one of the permitted subset of scoring keywords.

   * Category Specification

     ```
     file
     ```
   * Group Parameter specification

     ```
     (agent-score . file)
     ```

---

Next: [Category Variables](#Category-Variables), Previous: [Category Syntax](#Category-Syntax), Up: [Agent Categories](#Agent-Categories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.2.2 Category Buffer [¶](#Category-Buffer-1)

You’d normally do all category maintenance from the category buffer.
When you enter it for the first time (with the `J c` command from
the group buffer), you’ll only see the `default` category.

The following commands are available in this buffer:

`q` [¶](#index-q-_0028Category_0029)
:   Return to the group buffer (`gnus-category-exit`).

`e` [¶](#index-e-_0028Category_0029)
:   Use a customization buffer to set all of the selected category’s
    parameters at one time (`gnus-category-customize-category`).

`k` [¶](#index-k-_0028Category_0029)
:   Kill the current category (`gnus-category-kill`).

`c` [¶](#index-c-_0028Category_0029)
:   Copy the current category (`gnus-category-copy`).

`a` [¶](#index-a-_0028Category_0029)
:   Add a new category (`gnus-category-add`).

`p` [¶](#index-p-_0028Category_0029)
:   Edit the predicate of the current category
    (`gnus-category-edit-predicate`).

`g` [¶](#index-g-_0028Category_0029)
:   Edit the list of groups belonging to the current category
    (`gnus-category-edit-groups`).

`s` [¶](#index-s-_0028Category_0029)
:   Edit the download score rule of the current category
    (`gnus-category-edit-score`).

`l` [¶](#index-l-_0028Category_0029)
:   List all the categories (`gnus-category-list`).

---

Previous: [Category Buffer](#Category-Buffer), Up: [Agent Categories](#Agent-Categories)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.2.3 Category Variables [¶](#Category-Variables-1)

`gnus-category-mode-hook` [¶](#index-gnus_002dcategory_002dmode_002dhook)
:   Hook run in category buffers.

`gnus-category-line-format` [¶](#index-gnus_002dcategory_002dline_002dformat)
:   Format of the lines in the category buffer (see [Formatting Variables](#Formatting-Variables)). Valid elements are:

    ‘`c`’
    :   The name of the category.

    ‘`g`’
    :   The number of groups in the category.

`gnus-category-mode-line-format` [¶](#index-gnus_002dcategory_002dmode_002dline_002dformat)
:   Format of the category mode line (see [Mode Line Formatting](#Mode-Line-Formatting)).

`gnus-agent-short-article` [¶](#index-gnus_002dagent_002dshort_002darticle)
:   Articles that have fewer lines than this are short. Default 100.

`gnus-agent-long-article` [¶](#index-gnus_002dagent_002dlong_002darticle)
:   Articles that have more lines than this are long. Default 200.

`gnus-agent-low-score` [¶](#index-gnus_002dagent_002dlow_002dscore)
:   Articles that have a score lower than this have a low score. Default
    0.

`gnus-agent-high-score` [¶](#index-gnus_002dagent_002dhigh_002dscore)
:   Articles that have a score higher than this have a high score. Default
    0.

`gnus-agent-expire-days` [¶](#index-gnus_002dagent_002dexpire_002ddays)
:   The number of days that a ‘`read`’ article must stay in the agent’s
    local disk before becoming eligible for expiration (While the name is
    the same, this doesn’t mean expiring the article on the server. It
    just means deleting the local copy of the article). What is also
    important to understand is that the counter starts with the time the
    article was written to the local disk and not the time the article was
    read.
    Default 7.

`gnus-agent-enable-expiration` [¶](#index-gnus_002dagent_002denable_002dexpiration)
:   Determines whether articles in a group are, by default, expired or
    retained indefinitely. The default is `ENABLE` which means that
    you’ll have to disable expiration when desired. On the other hand,
    you could set this to `DISABLE`. In that case, you would then
    have to enable expiration in selected groups.

---

Next: [Agent Visuals](#Agent-Visuals), Previous: [Agent Categories](#Agent-Categories), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.3 Agent Commands [¶](#Agent-Commands-1)

All the Gnus Agent commands are on the `J` submap. The `J j`
(`gnus-agent-toggle-plugged`) command works in all modes, and
toggles the plugged/unplugged state of the Gnus Agent.

* [Group Agent Commands](#Group-Agent-Commands)
* [Summary Agent Commands](#Summary-Agent-Commands)
* [Server Agent Commands](#Server-Agent-Commands)

---

Next: [Summary Agent Commands](#Summary-Agent-Commands), Up: [Agent Commands](#Agent-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.3.1 Group Agent Commands [¶](#Group-Agent-Commands-1)

`J u` [¶](#index-J-u-_0028Agent-Group_0029)
:   Fetch all eligible articles in the current group
    (`gnus-agent-fetch-groups`).

`J c` [¶](#index-J-c-_0028Agent-Group_0029)
:   Enter the Agent category buffer (`gnus-enter-category-buffer`).

`J s` [¶](#index-J-s-_0028Agent-Group_0029)
:   Fetch all eligible articles in all groups
    (`gnus-agent-fetch-session`).

`J S` [¶](#index-J-S-_0028Agent-Group_0029)
:   Send all sendable messages in the queue group
    (`gnus-group-send-queue`). See [Drafts](#Drafts).

`J a` [¶](#index-J-a-_0028Agent-Group_0029)
:   Add the current group to an Agent category
    (`gnus-agent-add-group`). This command understands the
    process/prefix convention (see [Process/Prefix](#Process_002fPrefix)).

`J r` [¶](#index-J-r-_0028Agent-Group_0029)
:   Remove the current group from its category, if any
    (`gnus-agent-remove-group`). This command understands the
    process/prefix convention (see [Process/Prefix](#Process_002fPrefix)).

`J Y` [¶](#index-J-Y-_0028Agent-Group_0029)
:   Synchronize flags changed while unplugged with remote server, if any.

---

Next: [Server Agent Commands](#Server-Agent-Commands), Previous: [Group Agent Commands](#Group-Agent-Commands), Up: [Agent Commands](#Agent-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.3.2 Summary Agent Commands [¶](#Summary-Agent-Commands-1)

`J #` [¶](#index-J-_0023-_0028Agent-Summary_0029)
:   Mark the article for downloading (`gnus-agent-mark-article`).

`J M-#` [¶](#index-J-M_002d_0023-_0028Agent-Summary_0029)
:   Remove the downloading mark from the article
    (`gnus-agent-unmark-article`).

`@` [¶](#index-_0040-_0028Agent-Summary_0029)
:   Toggle whether to download the article
    (`gnus-agent-toggle-mark`). The download mark is ‘`%`’ by
    default.

`J c` [¶](#index-J-c-_0028Agent-Summary_0029)
:   Mark all articles as read (`gnus-agent-catchup`) that are neither cached, downloaded, nor downloadable.

`J S` [¶](#index-J-S-_0028Agent-Summary_0029)
:   Download all eligible (see [Agent Categories](#Agent-Categories)) articles in this group.
    (`gnus-agent-fetch-group`).

`J s` [¶](#index-J-s-_0028Agent-Summary_0029)
:   Download all processable articles in this group.
    (`gnus-agent-summary-fetch-series`).

`J u` [¶](#index-J-u-_0028Agent-Summary_0029)
:   Download all downloadable articles in the current group
    (`gnus-agent-summary-fetch-group`).

---

Previous: [Summary Agent Commands](#Summary-Agent-Commands), Up: [Agent Commands](#Agent-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.3.3 Server Agent Commands [¶](#Server-Agent-Commands-1)

`J a` [¶](#index-J-a-_0028Agent-Server_0029)
:   Add the current server to the list of servers covered by the Gnus Agent
    (`gnus-agent-add-server`).

`J r` [¶](#index-J-r-_0028Agent-Server_0029)
:   Remove the current server from the list of servers covered by the Gnus
    Agent (`gnus-agent-remove-server`).

---

Next: [Agent as Cache](#Agent-as-Cache), Previous: [Agent Commands](#Agent-Commands), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.4 Agent Visuals [¶](#Agent-Visuals-1)

If you open a summary while unplugged and, Gnus knows from the group’s
active range that there are more articles than the headers currently
stored in the Agent, you may see some articles whose subject looks
something like ‘`[Undownloaded article #####]`’. These are
placeholders for the missing headers. Aside from setting a mark,
there is not much that can be done with one of these placeholders.
When Gnus finally gets a chance to fetch the group’s headers, the
placeholders will automatically be replaced by the actual headers.
You can configure the summary buffer’s maneuvering to skip over the
placeholders if you care (See `gnus-auto-goto-ignores`).

While it may be obvious to all, the only headers and articles
available while unplugged are those headers and articles that were
fetched into the Agent while previously plugged. To put it another
way, “If you forget to fetch something while plugged, you might have a
less than satisfying unplugged session”. For this reason, the Agent
adds two visual effects to your summary buffer. These effects display
the download status of each article so that you always know which
articles will be available when unplugged.

The first visual effect is the ‘`%O`’ spec. If you customize
`gnus-summary-line-format` to include this specifier, you will add
a single character field that indicates an article’s download status.
Articles that have been fetched into either the Agent or the Cache,
will display `gnus-downloaded-mark` (defaults to ‘`+`’). All
other articles will display `gnus-undownloaded-mark` (defaults to
‘`-`’). If you open a group that has not been agentized, a space
(‘’) will be displayed.

The second visual effect are the undownloaded faces. The faces, there
are three indicating the article’s score (low, normal, high), seem to
result in a love/hate response from many Gnus users. The problem is
that the face selection is controlled by a list of condition tests and
face names (See `gnus-summary-highlight`). Each condition is
tested in the order in which it appears in the list so early
conditions have precedence over later conditions. All of this means
that, if you tick an undownloaded article, the article will continue
to be displayed in the undownloaded face rather than the ticked face.

If you use the Agent as a cache (to avoid downloading the same article
each time you visit it or to minimize your connection time), the
undownloaded face will probably seem like a good idea. The reason
being that you do all of our work (marking, reading, deleting) with
downloaded articles so the normal faces always appear. For those
users using the agent to improve online performance by caching the NOV
database (most users since 5.10.2), the undownloaded faces may appear
to be an absolutely horrible idea. The issue being that, since none
of their articles have been fetched into the Agent, all of the
normal faces will be obscured by the undownloaded faces.

If you would like to use the undownloaded faces, you must enable the
undownloaded faces by setting the `agent-enable-undownloaded-faces`
group parameter to `t`. This parameter, like all other agent
parameters, may be set on an Agent Category (see [Agent Categories](#Agent-Categories)),
a Group Topic (see [Topic Parameters](#Topic-Parameters)), or an individual group
(see [Group Parameters](#Group-Parameters)).

The one problem common to all users using the agent is how quickly it
can consume disk space. If you using the agent on many groups, it is
even more difficult to effectively recover disk space. One solution
is the ‘`%F`’ format available in `gnus-group-line-format`.
This format will display the actual disk space used by articles
fetched into both the agent and cache. By knowing which groups use
the most space, users know where to focus their efforts when “agent
expiring” articles.

---

Next: [Agent Expiry](#Agent-Expiry), Previous: [Agent Visuals](#Agent-Visuals), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.5 Agent as Cache [¶](#Agent-as-Cache-1)

When Gnus is plugged, it is not efficient to download headers or
articles from the server again, if they are already stored in the
Agent. So, Gnus normally only downloads headers once, and stores them
in the Agent. These headers are later used when generating the summary
buffer, regardless of whether you are plugged or unplugged. Articles
are not cached in the Agent by default though (that would potentially
consume lots of disk space), but if you have already downloaded an
article into the Agent, Gnus will not download the article from the
server again but use the locally stored copy instead.

If you so desire, you can configure the agent (see `gnus-agent-cache`
see [Agent Variables](#Agent-Variables)) to always download headers and articles while
plugged. Gnus will almost certainly be slower, but it will be kept
synchronized with the server. That last point probably won’t make any
sense if you are using an nntp or nnimap back end.

---

Next: [Agent Regeneration](#Agent-Regeneration), Previous: [Agent as Cache](#Agent-as-Cache), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.6 Agent Expiry [¶](#Agent-Expiry-1)

The Agent back end, `nnagent`, doesn’t handle expiry. Well, at
least it doesn’t handle it like other back ends. Instead, there are
special `gnus-agent-expire` and `gnus-agent-expire-group`
commands that will expire all read articles that are older than
`gnus-agent-expire-days` days. They can be run whenever you feel
that you’re running out of space. Neither are particularly fast or
efficient, and it’s not a particularly good idea to interrupt them (with
`C-g` or anything else) once you’ve started one of them.

Note that other functions might run `gnus-agent-expire` for you
to keep the agent synchronized with the group.

The agent parameter `agent-enable-expiration` may be used to
prevent expiration in selected groups.

If `gnus-agent-expire-all` is non-`nil`, the agent
expiration commands will expire all articles—unread, read, ticked
and dormant. If `nil` (which is the default), only read articles
are eligible for expiry, and unread, ticked and dormant articles will
be kept indefinitely.

The last (i.e., newest) article in a group will normally not be
expired (due to internal book-keeping reasons).

If you find that some articles eligible for expiry are never expired,
perhaps some Gnus Agent files are corrupted. There’s are special
commands, `gnus-agent-regenerate` and
`gnus-agent-regenerate-group`, to fix possible problems.

---

Next: [Agent and flags](#Agent-and-flags), Previous: [Agent Expiry](#Agent-Expiry), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.7 Agent Regeneration [¶](#Agent-Regeneration-1)

The local data structures used by `nnagent` may become corrupted
due to certain exceptional conditions. When this happens,
`nnagent` functionality may degrade or even fail. The solution
to this problem is to repair the local data structures by removing all
internal inconsistencies.

For example, if your connection to your server is lost while
downloaded articles into the agent, the local data structures will not
know about articles successfully downloaded prior to the connection
failure. Running `gnus-agent-regenerate` or
`gnus-agent-regenerate-group` will update the data structures
such that you don’t need to download these articles a second time.

The command `gnus-agent-regenerate` will perform
`gnus-agent-regenerate-group` on every agentized group. While
you can run `gnus-agent-regenerate` in any buffer, it is strongly
recommended that you first close all summary buffers.

The command `gnus-agent-regenerate-group` uses the local copies
of individual articles to repair the local NOV(header) database. It
then updates the internal data structures that document which articles
are stored locally. An optional argument will mark articles in the
agent as unread.

---

Next: [Agent and IMAP](#Agent-and-IMAP), Previous: [Agent Regeneration](#Agent-Regeneration), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.8 Agent and flags [¶](#Agent-and-flags-1)

The Agent works with any Gnus back end including those, such as
nnimap, that store flags (read, ticked, etc.) on the server. Sadly,
the Agent does not actually know which backends keep their flags in
the backend server rather than in `.newsrc`. This means that the
Agent, while unplugged or disconnected, will always record all changes
to the flags in its own files.

When you plug back in, Gnus will then check to see if you have any
changed any flags and ask if you wish to synchronize these with the
server. This behavior is customizable by `gnus-agent-synchronize-flags`.

If `gnus-agent-synchronize-flags` is `nil`, the Agent will
never automatically synchronize flags. If it is `ask`, which is
the default, the Agent will check if you made any changes and if so
ask if you wish to synchronize these when you re-connect. If it has
any other value, all flags will be synchronized automatically.

If you do not wish to synchronize flags automatically when you
re-connect, you can do it manually with the
`gnus-agent-synchronize-flags` command that is bound to `J Y`
in the group buffer.

Technical note: the synchronization algorithm does not work by “pushing”
all local flags to the server, but rather by incrementally updated the
server view of flags by changing only those flags that were changed by
the user. Thus, if you set one flag on an article, quit the group then
re-select the group and remove the flag; the flag will be set and
removed from the server when you “synchronize”. The queued flag
operations can be found in the per-server `flags` file in the Agent
directory. It’s emptied when you synchronize flags.

---

Next: [Outgoing Messages](#Outgoing-Messages), Previous: [Agent and flags](#Agent-and-flags), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.9 Agent and IMAP [¶](#Agent-and-IMAP-1)

The Agent works with any Gnus back end, including nnimap. However,
since there are some conceptual differences between NNTP and
IMAP, this section (should) provide you with some information to
make Gnus Agent work smoother as a IMAP Disconnected Mode client.

Some things are currently not implemented in the Agent that you’d might
expect from a disconnected IMAP client, including:

* Copying/moving articles into nnimap groups when unplugged.
* Creating/deleting nnimap groups when unplugged.

---

Next: [Agent Variables](#Agent-Variables), Previous: [Agent and IMAP](#Agent-and-IMAP), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.10 Outgoing Messages [¶](#Outgoing-Messages-1)

By default, when Gnus is unplugged, all outgoing messages (both mail
and news) are stored in the draft group “queue” (see [Drafts](#Drafts)).
You can view them there after posting, and edit them at will.

You can control the circumstances under which outgoing mail is queued
(see `gnus-agent-queue-mail`, see [Agent Variables](#Agent-Variables)). Outgoing
news is always queued when Gnus is unplugged, and never otherwise.

You can send the messages either from the draft group with the special
commands available there, or you can use the `J S` command in the
group buffer to send all the sendable messages in the draft group.
Posting news will only work when Gnus is plugged, but you can send
mail at any time.

If sending mail while unplugged does not work for you and you worry
about hitting `J S` by accident when unplugged, you can have Gnus
ask you to confirm your action (see
`gnus-agent-prompt-send-queue`, see [Agent Variables](#Agent-Variables)).

---

Next: [Example Setup](#Example-Setup), Previous: [Outgoing Messages](#Outgoing-Messages), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.11 Agent Variables [¶](#Agent-Variables-1)

`gnus-agent` [¶](#index-gnus_002dagent)
:   Is the agent enabled? The default is `t`. When first enabled,
    the agent will use `gnus-agent-auto-agentize-methods` to
    automatically mark some back ends as agentized. You may change which
    back ends are agentized using the agent commands in the server buffer.

    To enter the server buffer, use the `^`
    (`gnus-group-enter-server-mode`) command in the group buffer.

`gnus-agent-directory` [¶](#index-gnus_002dagent_002ddirectory)
:   Where the Gnus Agent will store its files. The default is
    `~/News/agent/`.

`gnus-agent-handle-level` [¶](#index-gnus_002dagent_002dhandle_002dlevel)
:   Groups on levels (see [Group Levels](#Group-Levels)) higher than this variable will
    be ignored by the Agent. The default is `gnus-level-subscribed`,
    which means that only subscribed group will be considered by the Agent
    by default.

`gnus-agent-plugged-hook` [¶](#index-gnus_002dagent_002dplugged_002dhook)
:   Hook run when connecting to the network.

`gnus-agent-unplugged-hook` [¶](#index-gnus_002dagent_002dunplugged_002dhook)
:   Hook run when disconnecting from the network.

`gnus-agent-fetched-hook` [¶](#index-gnus_002dagent_002dfetched_002dhook)
:   Hook run when finished fetching articles.

`gnus-agent-cache` [¶](#index-gnus_002dagent_002dcache)
:   Variable to control whether use the locally stored NOV and
    articles when plugged, e.g., essentially using the Agent as a cache.
    The default is non-`nil`, which means to use the Agent as a cache.

`gnus-agent-eagerly-store-articles` [¶](#index-gnus_002dagent_002deagerly_002dstore_002darticles)
:   If non-`nil` (which is the default), store all articles read in
    agentized groups in the Agent cache.

`gnus-agent-go-online` [¶](#index-gnus_002dagent_002dgo_002donline)
:   If `gnus-agent-go-online` is `nil`, the Agent will never
    automatically switch offline servers into online status. If it is
    `ask`, the default, the Agent will ask if you wish to switch
    offline servers into online status when you re-connect. If it has any
    other value, all offline servers will be automatically switched into
    online status.

`gnus-agent-mark-unread-after-downloaded` [¶](#index-gnus_002dagent_002dmark_002dunread_002dafter_002ddownloaded)
:   If `gnus-agent-mark-unread-after-downloaded` is non-`nil`,
    mark articles as unread after downloading. This is usually a safe
    thing to do as the newly downloaded article has obviously not been
    read. The default is `t`.

`gnus-agent-synchronize-flags` [¶](#index-gnus_002dagent_002dsynchronize_002dflags-2)
:   If `gnus-agent-synchronize-flags` is `nil`, the Agent will
    never automatically synchronize flags. If it is `ask`, which is
    the default, the Agent will check if you made any changes and if so
    ask if you wish to synchronize these when you re-connect. If it has
    any other value, all flags will be synchronized automatically.

`gnus-agent-consider-all-articles` [¶](#index-gnus_002dagent_002dconsider_002dall_002darticles)
:   If `gnus-agent-consider-all-articles` is non-`nil`, the
    agent will let the agent predicate decide whether articles need to be
    downloaded or not, for all articles. When `nil`, the default,
    the agent will only let the predicate decide whether unread articles
    are downloaded or not. If you enable this, you may also want to look
    into the agent expiry settings (see [Category Variables](#Category-Variables)), so that
    the agent doesn’t download articles which the agent will later expire,
    over and over again.

`gnus-agent-max-fetch-size` [¶](#index-gnus_002dagent_002dmax_002dfetch_002dsize)
:   The agent fetches articles into a temporary buffer prior to parsing
    them into individual files. To avoid exceeding the max. buffer size,
    the agent alternates between fetching and parsing until all articles
    have been fetched. `gnus-agent-max-fetch-size` provides a size
    limit to control how often the cycling occurs. A large value improves
    performance. A small value minimizes the time lost should the
    connection be lost while fetching (You may need to run
    `gnus-agent-regenerate-group` to update the group’s state.
    However, all articles parsed prior to losing the connection will be
    available while unplugged). The default is 10M so it is unusual to
    see any cycling.

`gnus-server-unopen-status` [¶](#index-gnus_002dserver_002dunopen_002dstatus)
:   Perhaps not an Agent variable, but closely related to the Agent, this
    variable says what will happen if Gnus cannot open a server. If the
    Agent is enabled, the default, `nil`, makes Gnus ask the user
    whether to deny the server or whether to unplug the agent. If the
    Agent is disabled, Gnus always simply deny the server. Other choices
    for this variable include `denied` and `offline` the latter
    is only valid if the Agent is used.

`gnus-auto-goto-ignores` [¶](#index-gnus_002dauto_002dgoto_002dignores)
:   Another variable that isn’t an Agent variable, yet so closely related
    that most will look for it here, this variable tells the summary
    buffer how to maneuver around undownloaded (only headers stored in the
    agent) and unfetched (neither article nor headers stored) articles.

    The valid values are `nil` (maneuver to any article),
    `undownloaded` (maneuvering while unplugged ignores articles that
    have not been fetched), `always-undownloaded` (maneuvering always
    ignores articles that have not been fetched), `unfetched`
    (maneuvering ignores articles whose headers have not been fetched).

`gnus-agent-queue-mail` [¶](#index-gnus_002dagent_002dqueue_002dmail)
:   When `gnus-agent-queue-mail` is `always`, Gnus will always
    queue mail rather than sending it straight away. When `t`, Gnus
    will queue mail when unplugged only. When `nil`, never queue
    mail. The default is `t`.

`gnus-agent-prompt-send-queue` [¶](#index-gnus_002dagent_002dprompt_002dsend_002dqueue)
:   When `gnus-agent-prompt-send-queue` is non-`nil` Gnus will
    prompt you to confirm that you really wish to proceed if you hit
    `J S` while unplugged. The default is `nil`.

`gnus-agent-auto-agentize-methods` [¶](#index-gnus_002dagent_002dauto_002dagentize_002dmethods)
:   If you have never used the Agent before (or more technically, if
    `~/News/agent/lib/servers` does not exist), Gnus will
    automatically agentize a few servers for you. This variable control
    which back ends should be auto-agentized. It is typically only useful
    to agentize remote back ends. The auto-agentizing has the same effect
    as running `J a` on the servers (see [Server Agent Commands](#Server-Agent-Commands)).
    If the file exist, you must manage the servers manually by adding or
    removing them, this variable is only applicable the first time you
    start Gnus. The default is ‘`nil`’.

---

Next: [Batching Agents](#Batching-Agents), Previous: [Agent Variables](#Agent-Variables), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.12 Example Setup [¶](#Example-Setup-1)

If you don’t want to read this manual, and you have a fairly standard
setup, you may be able to use something like the following as your
`~/.gnus.el` file to get started.

```
;; Define how Gnus is to fetch news.  We do this over NNTP
;; from your ISP’s server.
(setq gnus-select-method '(nntp "news.your-isp.com"))

;; Define how Gnus is to read your mail.  We read mail from
;; your ISP’s POP server.
(setq mail-sources '((pop :server "pop.your-isp.com")))

;; Say how Gnus is to store the mail.  We use nnml groups.
(setq gnus-secondary-select-methods '((nnml "")))

;; Make Gnus into an offline newsreader.
;; (gnus-agentize) ; The obsolete setting.
;; (setq gnus-agent t) ; Now the default.
```

That should be it, basically. Put that in your `~/.gnus.el` file,
edit to suit your needs, start up PPP (or whatever), and type `M-x
gnus`.

If this is the first time you’ve run Gnus, you will be subscribed
automatically to a few default newsgroups. You’ll probably want to
subscribe to more groups, and to do that, you have to query the
NNTP server for a complete list of groups with the `A A`
command. This usually takes quite a while, but you only have to do it
once.

After reading and parsing a while, you’ll be presented with a list of
groups. Subscribe to the ones you want to read with the `u`
command. `l` to make all the killed groups disappear after you’ve
subscribe to all the groups you want to read. (`A k` will bring
back all the killed groups.)

You can now read the groups at once, or you can download the articles
with the `J s` command. And then read the rest of this manual to
find out which of the other gazillion things you want to customize.

---

Next: [Agent Caveats](#Agent-Caveats), Previous: [Example Setup](#Example-Setup), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.13 Batching Agents [¶](#Batching-Agents-1)

Having the Gnus Agent fetch articles (and post whatever messages you’ve
written) is quite easy once you’ve gotten things set up properly. The
following shell script will do everything that is necessary:

You can run a complete batch command from the command line with the
following incantation:

```
#!/bin/sh
emacs -batch -l ~/.emacs -l ~/.gnus.el -f gnus-agent-batch >/dev/null 2>&1
```

---

Previous: [Batching Agents](#Batching-Agents), Up: [Gnus Unplugged](#Gnus-Unplugged)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 7.9.14 Agent Caveats [¶](#Agent-Caveats-1)

The Gnus Agent doesn’t seem to work like most other offline
newsreaders. Here are some common questions that some imaginary people
may ask:

*If I read an article while plugged, do they get entered into the Agent?*
:   Yes, because of the default value of
    `gnus-agent-eagerly-store-articles`. An alternative is to add
    `gnus-agent-fetch-selected-article` to
    `gnus-select-article-hook`.

*If I read an article while plugged, and the article already exists in*
:   the Agent, will it get downloaded once more?

    **No**, unless `gnus-agent-cache` is `nil`.

In short, when Gnus is unplugged, it only looks into the locally stored
articles; when it’s plugged, it talks to your ISP and may also use the
locally stored articles.

---

Next: [Searching](#Searching), Previous: [Select Methods](#Select-Methods), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 8 Scoring [¶](#Scoring-1)

Other people use *kill files* (see [Kill Files](#Kill-Files)), but we here at
Gnus Towers like scoring better than killing, so we’d rather switch
than fight. Scoring and score files processing are more powerful and
faster than processing of kill files. Scoring also does something
completely different as well, so sit up straight and pay attention!

All articles have a default score (`gnus-summary-default-score`),
which is 0 by default. This score may be raised or lowered either
interactively or by score files. Articles that have a score lower than
`gnus-summary-mark-below` are marked as read.

Gnus will read any *score files* that apply to the current group
before generating the summary buffer.

There are several commands in the summary buffer that insert score
entries based on the current article. You can, for instance, ask Gnus to
lower or increase the score of all articles with a certain subject.

There are two sorts of scoring entries: Permanent and temporary.
Temporary score entries are self-expiring entries. Any entries that are
temporary and have not been used for, say, a week, will be removed
silently to help keep the sizes of the score files down.

* [Summary Score Commands](#Summary-Score-Commands)
* [Group Score Commands](#Group-Score-Commands)
* [Score Variables](#Score-Variables)
* [Score File Format](#Score-File-Format)
* [Score File Editing](#Score-File-Editing)
* [Adaptive Scoring](#Adaptive-Scoring)
* [Home Score File](#Home-Score-File)
* [Followups To Yourself](#Followups-To-Yourself)
* [Scoring On Other Headers](#Scoring-On-Other-Headers)
* [Scoring Tips](#Scoring-Tips)
* [Reverse Scoring](#Reverse-Scoring)
* [Global Score Files](#Global-Score-Files)
* [Kill Files](#Kill-Files)
* [Converting Kill Files](#Converting-Kill-Files)
* [Advanced Scoring](#Advanced-Scoring)
* [Score Decays](#Score-Decays)

---

Next: [Group Score Commands](#Group-Score-Commands), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.1 Summary Score Commands [¶](#Summary-Score-Commands-1)

The score commands that alter score entries do not actually modify real
score files. That would be too inefficient. Gnus maintains a cache of
previously loaded score files, one of which is considered the
*current score file alist*. The score commands simply insert
entries into this list, and upon group exit, this list is saved.

The current score file is by default the group’s local score file, even
if no such score file actually exists. To insert score commands into
some other score file (e.g., `all.SCORE`), you must first make this
score file the current one.

General score commands that don’t actually change the score file:

`V s` [¶](#index-V-s-_0028Summary_0029)
:   Set the score of the current article (`gnus-summary-set-score`).

`V S` [¶](#index-V-S-_0028Summary_0029)
:   Display the score of the current article
    (`gnus-summary-current-score`).

`V t` [¶](#index-V-t-_0028Summary_0029)
:   Display all score rules that have been used on the current article
    (`gnus-score-find-trace`). In the `*Score Trace*` buffer, you
    may type `e` to edit score file corresponding to the score rule on
    current line and `f` to format (`gnus-score-pretty-print`) the
    score file and edit it.

`V w` [¶](#index-V-w-_0028Summary_0029)
:   List words used in scoring (`gnus-score-find-favorite-words`).

`V R` [¶](#index-V-R-_0028Summary_0029)
:   Run the current summary through the scoring process
    (`gnus-summary-rescore`). This might be useful if you’re playing
    around with your score files behind Gnus’ back and want to see the
    effect you’re having.

`V c` [¶](#index-V-c-_0028Summary_0029)
:   Make a different score file the current
    (`gnus-score-change-score-file`).

`V e` [¶](#index-V-e-_0028Summary_0029)
:   Edit the current score file (`gnus-score-edit-current-scores`).
    You will be popped into a `gnus-score-mode` buffer (see [Score File Editing](#Score-File-Editing)).

`V f` [¶](#index-V-f-_0028Summary_0029)
:   Edit a score file and make this score file the current one
    (`gnus-score-edit-file`).

`V F` [¶](#index-V-F-_0028Summary_0029)
:   Flush the score cache (`gnus-score-flush-cache`). This is useful
    after editing score files.

`V C` [¶](#index-V-C-_0028Summary_0029)
:   Customize a score file in a visually pleasing manner
    (`gnus-score-customize`).

The rest of these commands modify the local score file.

`V m` [¶](#index-V-m-_0028Summary_0029)
:   Prompt for a score, and mark all articles with a score below this as
    read (`gnus-score-set-mark-below`).

`V x` [¶](#index-V-x-_0028Summary_0029)
:   Prompt for a score, and add a score rule to the current score file to
    expunge all articles below this score
    (`gnus-score-set-expunge-below`).

The keystrokes for actually making score entries follow a very regular
pattern, so there’s no need to list all the commands. (Hundreds of
them.)

1. The first key is either `I` (upper case i) for increasing the score
   or `L` for lowering the score.
2. The second key says what header you want to score on. The following
   keys are available:

   `a`
   :   Score on the author name.

   `s`
   :   Score on the subject line.

   `x`
   :   Score on the `Xref` line—i.e., the cross-posting line.

   `r`
   :   Score on the `References` line.

   `d`
   :   Score on the date.

   `l`
   :   Score on the number of lines.

   `i`
   :   Score on the `Message-ID` header.

   `e`
   :   Score on an “extra” header, that is, one of those in gnus-extra-headers,
       if your NNTP server tracks additional header data in overviews.

   `f`
   :   Score on followups—this matches the author name, and adds scores to
       the followups to this author. (Using this key leads to the creation of
       `ADAPT` files.)

   `b`
   :   Score on the body.

   `h`
   :   Score on the head.

   `t`
   :   Score on thread. (Using this key leads to the creation of `ADAPT`
       files.)
3. The third key is the match type. Which match types are valid depends on
   what headers you are scoring on.

   `strings`
   :   `e`
       :   Exact matching.

       `s`
       :   Substring matching.

       `f`
       :   Fuzzy matching (see [Fuzzy Matching](#Fuzzy-Matching)).

       `r`
       :   Regexp matching.

   `date`
   :   `r`
       :   Regexp matching.

       `b`
       :   Before date.

       `a`
       :   After date.

       `n`
       :   This date.

       `<`
       :   Less than days.

       `>`
       :   Greater than days.

   `number`
   :   `<`
       :   Less than number.

       `=`
       :   Equal to number.

       `>`
       :   Greater than number.

   `body-strings`
   :   These match types are available on the ‘`head`’ and `body`
       “header types”.

       `z`
       :   Substring matching.

       `p`
       :   Regexp matching.
4. The fourth and usually final key says whether this is a temporary (i.e.,
   expiring) score entry, or a permanent (i.e., non-expiring) score entry,
   or whether it is to be done immediately, without adding to the score
   file.

   `t`
   :   Temporary score entry.

   `p`
   :   Permanent score entry.

   `i`
   :   Immediately scoring.
5. If you are scoring on ‘`e`’ (extra) headers, you will then be prompted for
   the header name on which you wish to score. This must be a header named
   in gnus-extra-headers, and ‘`TAB`’ completion is available.

So, let’s say you want to increase the score on the current author with
exact matching permanently: `I a e p`. If you want to lower the
score based on the subject line, using substring matching, and make a
temporary score entry: `L s s t`. Pretty easy.

To make things a bit more complicated, there are shortcuts. If you use
a capital letter on either the second or third keys, Gnus will use
defaults for the remaining one or two keystrokes. The defaults are
“substring” and “temporary”. So `I A` is the same as `I a s
t`, and `I a R` is the same as `I a r t`. (These shortcuts
are not available for the body matches.)

These functions take both the numerical prefix and the symbolic prefix
(see [Symbolic Prefixes](#Symbolic-Prefixes)). A numerical prefix says how much to lower
(or increase) the score of the article. A symbolic prefix of `a`
says to use the `all.SCORE` file for the command instead of the
current score file.

The `gnus-score-mimic-keymap` says whether these commands will
pretend they are keymaps or not.

---

Next: [Score Variables](#Score-Variables), Previous: [Summary Score Commands](#Summary-Score-Commands), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.2 Group Score Commands [¶](#Group-Score-Commands-1)

There aren’t many of these as yet, I’m afraid.

`W e` [¶](#index-W-e-_0028Group_0029)
:   Edit the apply-to-all-groups all.SCORE file. You will be popped into
    a `gnus-score-mode` buffer (see [Score File Editing](#Score-File-Editing)).

`W f` [¶](#index-W-f-_0028Group_0029)
:   Gnus maintains a cache of score alists to avoid having to reload them
    all the time. This command will flush the cache
    (`gnus-score-flush-cache`).

You can do scoring from the command line by saying something like:

```
$ emacs -batch -l ~/.emacs -l ~/.gnus.el -f gnus-batch-score
```

---

Next: [Score File Format](#Score-File-Format), Previous: [Group Score Commands](#Group-Score-Commands), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.3 Score Variables [¶](#Score-Variables-1)

`gnus-use-scoring` [¶](#index-gnus_002duse_002dscoring)
:   If `nil`, Gnus will not check for score files, and will not, in
    general, do any score-related work. This is `t` by default.

`gnus-kill-killed` [¶](#index-gnus_002dkill_002dkilled)
:   If this variable is `nil`, Gnus will never apply score files to
    articles that have already been through the kill process. While this
    may save you lots of time, it also means that if you apply a kill file
    to a group, and then change the kill file and want to run it over you
    group again to kill more articles, it won’t work. You have to set this
    variable to `t` to do that. (It is `t` by default.)

`gnus-kill-files-directory` [¶](#index-gnus_002dkill_002dfiles_002ddirectory)
:   All kill and score files will be stored in this directory, which is
    initialized from the `SAVEDIR` environment variable by default.
    This is `~/News/` by default.

`gnus-score-file-suffix` [¶](#index-gnus_002dscore_002dfile_002dsuffix)
:   Suffix to add to the group name to arrive at the score file name
    (`SCORE` by default.)

`gnus-score-uncacheable-files` [¶](#index-gnus_002dscore_002duncacheable_002dfiles)
:   All score files are normally cached to avoid excessive re-loading of
    score files. However, this might make your Emacs grow big and
    bloated, so this regexp can be used to weed out score files unlikely
    to be needed again. It would be a bad idea to deny caching of
    `all.SCORE`, while it might be a good idea to not cache
    `comp.infosystems.www.authoring.misc.ADAPT`. In fact, this
    variable is ‘`ADAPT$`’ by default, so no adaptive score files will
    be cached.

`gnus-save-score` [¶](#index-gnus_002dsave_002dscore)
:   If you have really complicated score files, and do lots of batch
    scoring, then you might set this variable to `t`. This will make
    Gnus save the scores into the `.newsrc.eld` file.

    If you do not set this to `t`, then manual scores (like those set
    with `V s` (`gnus-summary-set-score`)) will not be preserved
    across group visits.

`gnus-score-interactive-default-score` [¶](#index-gnus_002dscore_002dinteractive_002ddefault_002dscore)
:   Score used by all the interactive raise/lower commands to raise/lower
    score with. Default is 1000, which may seem excessive, but this is to
    ensure that the adaptive scoring scheme gets enough room to play with.
    We don’t want the small changes from the adaptive scoring to overwrite
    manually entered data.

`gnus-summary-default-score` [¶](#index-gnus_002dsummary_002ddefault_002dscore)
:   Default score of an article, which is 0 by default.

`gnus-summary-expunge-below` [¶](#index-gnus_002dsummary_002dexpunge_002dbelow)
:   Don’t display the summary lines of articles that have scores lower than
    this variable. This is `nil` by default, which means that no
    articles will be hidden. This variable is local to the summary buffers,
    and has to be set from `gnus-summary-mode-hook`.

`gnus-score-over-mark` [¶](#index-gnus_002dscore_002dover_002dmark)
:   Mark (in the third column) used for articles with a score over the
    default. Default is ‘`+`’.

`gnus-score-below-mark` [¶](#index-gnus_002dscore_002dbelow_002dmark)
:   Mark (in the third column) used for articles with a score below the
    default. Default is ‘`-`’.

`gnus-score-find-score-files-function` [¶](#index-gnus_002dscore_002dfind_002dscore_002dfiles_002dfunction)
:   Function used to find score files for the current group. This function
    is called with the name of the group as the argument.

    Predefined functions available are:

    `gnus-score-find-single` [¶](#index-gnus_002dscore_002dfind_002dsingle)
    :   Only apply the group’s own score file.

    `gnus-score-find-bnews` [¶](#index-gnus_002dscore_002dfind_002dbnews)
    :   Apply all score files that match, using bnews syntax. This is the
        default. If the current group is ‘`gnu.emacs.gnus`’, for instance,
        `all.emacs.all.SCORE`, `not.alt.all.SCORE` and
        `gnu.all.SCORE` would all apply. In short, the instances of
        ‘`all`’ in the score file names are translated into ‘`.*`’, and
        then a regexp match is done.

        This means that if you have some score entries that you want to apply to
        all groups, then you put those entries in the `all.SCORE` file.

        The score files are applied in a semi-random order, although Gnus will
        try to apply the more general score files before the more specific score
        files. It does this by looking at the number of elements in the score
        file names—discarding the ‘`all`’ elements.

    `gnus-score-find-hierarchical` [¶](#index-gnus_002dscore_002dfind_002dhierarchical)
    :   Apply all score files from all the parent groups. This means that you
        can’t have score files like `all.SCORE`, but you can have
        `SCORE`, `comp.SCORE` and `comp.emacs.SCORE` for each
        server.

    This variable can also be a list of functions. In that case, all
    these functions will be called with the group name as argument, and
    all the returned lists of score files will be applied. These
    functions can also return lists of lists of score alists directly. In
    that case, the functions that return these non-file score alists
    should probably be placed before the “real” score file functions, to
    ensure that the last score file returned is the local score file.
    Phu.

    For example, to do hierarchical scoring but use a non-server-specific
    overall score file, you could use the value

    ```
    (list (lambda (group) (list "all.SCORE"))
          'gnus-score-find-hierarchical)
    ```

`gnus-score-expiry-days` [¶](#index-gnus_002dscore_002dexpiry_002ddays)
:   This variable says how many days should pass before an unused score file
    entry is expired. If this variable is `nil`, no score file entries
    are expired. It’s 7 by default.

`gnus-update-score-entry-dates` [¶](#index-gnus_002dupdate_002dscore_002dentry_002ddates)
:   If this variable is non-`nil`, temporary score entries that have
    been triggered (matched) will have their dates updated. (This is how Gnus
    controls expiry—all non-matched-entries will become too old while
    matched entries will stay fresh and young.) However, if you set this
    variable to `nil`, even matched entries will grow old and will
    have to face that oh-so grim reaper.

`gnus-score-after-write-file-function` [¶](#index-gnus_002dscore_002dafter_002dwrite_002dfile_002dfunction)
:   Function called with the name of the score file just written.

`gnus-score-thread-simplify` [¶](#index-gnus_002dscore_002dthread_002dsimplify)
:   If this variable is non-`nil`, article subjects will be
    simplified for subject scoring purposes in the same manner as with
    threading—according to the current value of
    `gnus-simplify-subject-functions`. If the scoring entry uses
    `substring` or `exact` matching, the match will also be
    simplified in this manner.

---

Next: [Score File Editing](#Score-File-Editing), Previous: [Score Variables](#Score-Variables), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.4 Score File Format [¶](#Score-File-Format-1)

A score file is an `emacs-lisp` file that normally contains just a
single form. Casual users are not expected to edit these files;
everything can be changed from the summary buffer.

Anyway, if you’d like to dig into it yourself, here’s an example:

```
(("from"
  ("Lars Ingebrigtsen" -10000)
  ("Per Abrahamsen")
  ("larsi\\|lmi" -50000 nil R))
 ("subject"
  ("Ding is Badd" nil 728373))
 ("xref"
  ("alt.politics" -1000 728372 s))
 ("lines"
  (2 -100 nil <))
 (mark 0)
 (expunge -1000)
 (mark-and-expunge -10)
 (read-only nil)
 (orphan -10)
 (adapt t)
 (files "/home/larsi/News/gnu.SCORE")
 (exclude-files "all.SCORE")
 (local (gnus-newsgroup-auto-expire t)
        (gnus-summary-make-false-root empty))
 (eval (ding)))
```

This example demonstrates most score file elements. See [Advanced Scoring](#Advanced-Scoring), for a different approach.

Even though this looks much like Lisp code, nothing here is actually
`eval`ed. The Lisp reader is used to read this form, though, so it
has to be valid syntactically, if not semantically.

Six keys are supported by this alist:

`STRING`
:   If the key is a string, it is the name of the header to perform the
    match on. Scoring can only be performed on these eight headers:
    `From`, `Subject`, `References`, `Message-ID`,
    `Xref`, `Lines`, `Chars` and `Date`. In addition to
    these headers, there are three strings to tell Gnus to fetch the entire
    article and do the match on larger parts of the article: `Body`
    will perform the match on the body of the article, `Head` will
    perform the match on the head of the article, and `All` will
    perform the match on the entire article. Note that using any of these
    last three keys will slow down group entry *considerably*. The
    final “header” you can score on is `Followup`. These score
    entries will result in new score entries being added for all follow-ups
    to articles that matches these score entries.

    Following this key is an arbitrary number of score entries, where each
    score entry has one to four elements.

    1. The first element is the *match element*. On most headers this will
       be a string, but on the Lines and Chars headers, this must be an
       integer.
    2. If the second element is present, it should be a number—the *score
       element*. This number should be an integer in the neginf to posinf
       interval. This number is added to the score of the article if the match
       is successful. If this element is not present, the
       `gnus-score-interactive-default-score` number will be used
       instead. This is 1000 by default.
    3. If the third element is present, it should be a number—the *date
       element*. This date says when the last time this score entry matched,
       which provides a mechanism for expiring the score entries. It this
       element is not present, the score entry is permanent. The date is
       represented by the number of days since December 31, 1 BCE.
    4. If the fourth element is present, it should be a symbol—the *type
       element*. This element specifies what function should be used to see
       whether this score entry matches the article. What match types that can
       be used depends on what header you wish to perform the match on.

       *From, Subject, References, Xref, Message-ID*
       :   For most header types, there are the `r` and `R` (regexp), as
           well as `s` and `S` (substring) types, and `e` and
           `E` (exact match), and `w` (word match) types. If this
           element is not present, Gnus will assume that substring matching should
           be used. `R`, `S`, and `E` differ from the others in
           that the matches will be done in a case-sensitive manner. All these
           one-letter types are really just abbreviations for the `regexp`,
           `string`, `exact`, and `word` types, which you can use
           instead, if you feel like.

       *Extra*
       :   Just as for the standard string overview headers, if you are using
           gnus-extra-headers, you can score on these headers’ values. In this
           case, there is a 5th element in the score entry, being the name of the
           header to be scored. The following entry is useful in your
           `all.SCORE` file in case of spam attacks from a single origin
           host, if your NNTP server tracks ‘`NNTP-Posting-Host`’ in
           overviews:

           ```
           ("111.222.333.444" -1000 nil s
            "NNTP-Posting-Host")
           ```

       *Lines, Chars*
       :   These two headers use different match types: `<`, `>`,
           `=`, `>=` and `<=`.

           These predicates are true if

           ```
           (PREDICATE HEADER MATCH)
           ```

           evaluates to non-`nil`. For instance, the advanced match
           `("lines" 4 <)` (see [Advanced Scoring](#Advanced-Scoring)) will result in the
           following form:

           ```
           (< header-value 4)
           ```

           Or to put it another way: When using `<` on `Lines` with 4 as
           the match, we get the score added if the article has less than 4 lines.
           (It’s easy to get confused and think it’s the other way around. But
           it’s not. I think.)

           When matching on `Lines`, be careful because some back ends (like
           `nndir`) do not generate `Lines` header, so every article ends
           up being marked as having 0 lines. This can lead to strange results if
           you happen to lower score of the articles with few lines.

       *Date*
       :   For the Date header we have three kinda silly match types:
           `before`, `at` and `after`. I can’t really imagine this
           ever being useful, but, like, it would feel kinda silly not to provide
           this function. Just in case. You never know. Better safe than sorry.
           Once burnt, twice shy. Don’t judge a book by its cover. Never not have
           sex on a first date. (I have been told that at least one person, and I
           quote, “found this function indispensable”, however.)

           A more useful match type is `regexp`. With it, you can match the
           date string using a regular expression. The date is normalized to
           ISO8601 compact format first—YYYYMMDD`T`HHMMSS. If
           you want to match all articles that have been posted on April 1st in
           every year, you could use ‘`....0401.........`’ as a match string,
           for instance. (Note that the date is kept in its original time zone, so
           this will match articles that were posted when it was April 1st where
           the article was posted from. Time zones are such wholesome fun for the
           whole family, eh?)

           Finally, two actually useful match types for dates: `<` and
           `>`. These will allow scoring on the relative age (in days) of
           the articles. Here’s an example score file using the method:

           ```
           (("date"
             (7 10 nil <)
             (7 -10 nil >)
             (14 -10 nil >)))
           ```

           This results in articles less than a week old getting a 10 point
           increase, articles older than a week getting a 10 point decrease, and
           articles older than two weeks getting a cumulative 20 point decrease.

           The day can also be a floating point number: To score articles less
           than an hour old, you can say ‘`(0.04 10 nil <)`’.

       *Head, Body, All*
       :   These three match keys use the same match types as the `From` (etc.)
           header uses.

       *Followup*
       :   This match key is somewhat special, in that it will match the
           `From` header, and affect the score of not only the matching
           articles, but also all followups to the matching articles. This allows
           you to increase the score of followups to your own articles, or
           decrease the score of followups to the articles of some known
           trouble-maker. Uses the same match types as the `From` header
           uses. (Using this match key will lead to creation of `ADAPT`
           files.)

       *Thread*
       :   This match key works along the same lines as the `Followup` match
           key. If you say that you want to score on a (sub-)thread started by an
           article with a `Message-ID` x, then you add a ‘`thread`’
           match. This will add a new ‘`thread`’ match for each article that
           has x in its `References` header. (These new ‘`thread`’
           matches will use the `Message-ID`s of these matching articles.)
           This will ensure that you can raise/lower the score of an entire thread,
           even though some articles in the thread may not have complete
           `References` headers. Note that using this may lead to
           nondeterministic scores of the articles in the thread. (Using this match
           key will lead to creation of `ADAPT` files.)

`score-fn` [¶](#index-score-file-atoms)
:   The value of this entry should be one or more user-defined function
    names in parentheses. Each function will be called in order and the
    returned value is required to be an integer.

    ```
    (score-fn (custom-scoring))
    ```

    The user-defined function is called with an association list with the
    keys `number subject from date id refs chars lines xref extra`
    followed by the article’s score before the function is run.

    The following (somewhat contrived) example shows how to use a
    user-defined function that increases an article’s score by 10 if the
    year of the article’s date is also mentioned in its subject.

    ```
    (defun custom-scoring (article-alist score)
      (let ((subject (cdr (assoc 'subject article-alist)))
            (date (cdr (assoc 'date article-alist))))
        (if (string-match (number-to-string
                           (nth 5 (parse-time-string date)))
                          subject)
            10)))
    ```

    `score-fn` entries are permanent and can only be added or
    modified directly in the `SCORE` file.

`mark`
:   The value of this entry should be a number. Any articles with a score
    lower than this number will be marked as read.

`expunge`
:   The value of this entry should be a number. Any articles with a score
    lower than this number will be removed from the summary buffer.

`mark-and-expunge`
:   The value of this entry should be a number. Any articles with a score
    lower than this number will be marked as read and removed from the
    summary buffer.

`thread-mark-and-expunge`
:   The value of this entry should be a number. All articles that belong to
    a thread that has a total score below this number will be marked as read
    and removed from the summary buffer. `gnus-thread-score-function`
    says how to compute the total score for a thread.

`files`
:   The value of this entry should be any number of file names. These files
    are assumed to be score files as well, and will be loaded the same way
    this one was.

`exclude-files`
:   The clue of this entry should be any number of files. These files will
    not be loaded, even though they would normally be so, for some reason or
    other.

`eval`
:   The value of this entry will be `eval`ed. This element will be
    ignored when handling global score files.

`read-only`
:   Read-only score files will not be updated or saved. Global score files
    should feature this atom (see [Global Score Files](#Global-Score-Files)). (Note:
    *Global* here really means *global*; not your personal
    apply-to-all-groups score files.)

`orphan`
:   The value of this entry should be a number. Articles that do not have
    parents will get this number added to their scores. Imagine you follow
    some high-volume newsgroup, like ‘`comp.lang.c`’. Most likely you
    will only follow a few of the threads, also want to see any new threads.

    You can do this with the following two score file entries:

    ```
            (orphan -500)
            (mark-and-expunge -100)
    ```

    When you enter the group the first time, you will only see the new
    threads. You then raise the score of the threads that you find
    interesting (with `I T` or `I S`), and ignore (`c y`) the
    rest. Next time you enter the group, you will see new articles in the
    interesting threads, plus any new threads.

    I.e., the orphan score atom is for high-volume groups where a few
    interesting threads which can’t be found automatically by ordinary
    scoring rules exist.

`adapt`
:   This entry controls the adaptive scoring. If it is `t`, the
    default adaptive scoring rules will be used. If it is `ignore`, no
    adaptive scoring will be performed on this group. If it is a list, this
    list will be used as the adaptive scoring rules. If it isn’t present,
    or is something other than `t` or `ignore`, the default
    adaptive scoring rules will be used. If you want to use adaptive
    scoring on most groups, you’d set `gnus-use-adaptive-scoring` to
    `t`, and insert an `(adapt ignore)` in the groups where you do
    not want adaptive scoring. If you only want adaptive scoring in a few
    groups, you’d set `gnus-use-adaptive-scoring` to `nil`, and
    insert `(adapt t)` in the score files of the groups where you want
    it.

`adapt-file`
:   All adaptive score entries will go to the file named by this entry. It
    will also be applied when entering the group. This atom might be handy
    if you want to adapt on several groups at once, using the same adaptive
    file for a number of groups.

`local` [¶](#index-local-variables)
:   The value of this entry should be a list of `(var
    value)` pairs. Each var will be made buffer-local to the
    current summary buffer, and set to the value specified. This is a
    convenient, if somewhat strange, way of setting variables in some
    groups if you don’t like hooks much. Note that the value won’t
    be evaluated.

---

Next: [Adaptive Scoring](#Adaptive-Scoring), Previous: [Score File Format](#Score-File-Format), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
