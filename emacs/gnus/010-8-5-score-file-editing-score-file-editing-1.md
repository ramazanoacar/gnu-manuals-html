### 8.5 Score File Editing [¶](#Score-File-Editing-1)

You normally enter all scoring commands from the summary buffer, but you
might feel the urge to edit them by hand as well, so we’ve supplied you
with a mode for that.

It’s simply a slightly customized `emacs-lisp` mode, with these
additional commands:

`C-c C-c` [¶](#index-C_002dc-C_002dc-_0028Score_0029)
:   Save the changes you have made and return to the summary buffer
    (`gnus-score-edit-exit`).

`C-c C-d` [¶](#index-C_002dc-C_002dd-_0028Score_0029)
:   Insert the current date in numerical format
    (`gnus-score-edit-insert-date`). This is really the day number, if
    you were wondering.

`C-c C-p` [¶](#index-C_002dc-C_002dp-_0028Score_0029)
:   The adaptive score files are saved in an unformatted fashion. If you
    intend to read one of these files, you want to *pretty print* it
    first. This command (`gnus-score-pretty-print`) does that for
    you.

Type `M-x gnus-score-mode` to use this mode.

`gnus-score-menu-hook` is run in score mode buffers.

In the summary buffer you can use commands like `V f`, `V e` and
`V t` to begin editing score files.

---

Next: [Home Score File](#Home-Score-File), Previous: [Score File Editing](#Score-File-Editing), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.6 Adaptive Scoring [¶](#Adaptive-Scoring-1)

If all this scoring is getting you down, Gnus has a way of making it all
happen automatically—as if by magic. Or rather, as if by artificial
stupidity, to be precise.

When you read an article, or mark an article as read, or kill an
article, you leave marks behind. On exit from the group, Gnus can sniff
these marks and add score elements depending on what marks it finds.
You turn on this ability by setting `gnus-use-adaptive-scoring` to
`t` or `(line)`. If you want score adaptively on separate
words appearing in the subjects, you should set this variable to
`(word)`. If you want to use both adaptive methods, set this
variable to `(word line)`.

To give you complete control over the scoring process, you can customize
the `gnus-default-adaptive-score-alist` variable. For instance, it
might look something like this:

```
(setq gnus-default-adaptive-score-alist
  '((gnus-unread-mark)
    (gnus-ticked-mark (from 4))
    (gnus-dormant-mark (from 5))
    (gnus-del-mark (from -4) (subject -1))
    (gnus-read-mark (from 4) (subject 2))
    (gnus-expirable-mark (from -1) (subject -1))
    (gnus-killed-mark (from -1) (subject -3))
    (gnus-kill-file-mark)
    (gnus-ancient-mark)
    (gnus-low-score-mark)
    (gnus-catchup-mark (from -1) (subject -1))))
```

As you see, each element in this alist has a mark as a key (either a
variable name or a “real” mark—a character). Following this key is
a arbitrary number of header/score pairs. If there are no header/score
pairs following the key, no adaptive scoring will be done on articles
that have that key as the article mark. For instance, articles with
`gnus-unread-mark` in the example above will not get adaptive score
entries.

Each article can have only one mark, so just a single of these rules
will be applied to each article.

To take `gnus-del-mark` as an example—this alist says that all
articles that have that mark (i.e., are marked with ‘`e`’) will have a
score entry added to lower based on the `From` header by −4, and
lowered by `Subject` by −1. Change this to fit your prejudices.

If you have marked 10 articles with the same subject with
`gnus-del-mark`, the rule for that mark will be applied ten times.
That means that that subject will get a score of ten times −1, which
should be, unless I’m much mistaken, −10.

If you have auto-expirable (mail) groups (see [Expiring Mail](#Expiring-Mail)), all
the read articles will be marked with the ‘`E`’ mark. This’ll
probably make adaptive scoring slightly impossible, so auto-expiring and
adaptive scoring doesn’t really mix very well.

The headers you can score on are `from`, `subject`,
`message-id`, `references`, `xref`, `lines`,
`chars` and `date`. In addition, you can score on
`followup`, which will create an adaptive score entry that matches
on the `References` header using the `Message-ID` of the
current article, thereby matching the following thread.

If you use this scheme, you should set the score file atom `mark`
to something small—like −300, perhaps, to avoid having small random
changes result in articles getting marked as read.

After using adaptive scoring for a week or so, Gnus should start to
become properly trained and enhance the authors you like best, and kill
the authors you like least, without you having to say so explicitly.

You can control what groups the adaptive scoring is to be performed on
by using the score files (see [Score File Format](#Score-File-Format)). This will also
let you use different rules in different groups.

The adaptive score entries will be put into a file where the name is the
group name with `gnus-adaptive-file-suffix` appended. The default
is `ADAPT`.

Adaptive score files can get huge and are not meant to be edited by
human hands. If `gnus-adaptive-pretty-print` is `nil` (the
default) those files will not be written in a human readable way.

When doing adaptive scoring, substring or fuzzy matching would probably
give you the best results in most cases. However, if the header one
matches is short, the possibility for false positives is great, so if
the length of the match is less than
`gnus-score-exact-adapt-limit`, exact matching will be used. If
this variable is `nil`, exact matching will always be used to avoid
this problem.

As mentioned above, you can adapt either on individual words or entire
headers. If you adapt on words, the
`gnus-default-adaptive-word-score-alist` variable says what score
each instance of a word should add given a mark.

```
(setq gnus-default-adaptive-word-score-alist
      `((,gnus-read-mark . 30)
        (,gnus-catchup-mark . -10)
        (,gnus-killed-mark . -20)
        (,gnus-del-mark . -15)))
```

This is the default value. If you have adaption on words enabled, every
word that appears in subjects of articles marked with
`gnus-read-mark` will result in a score rule that increase the
score with 30 points.

Words that appear in the `gnus-default-ignored-adaptive-words` list
will be ignored. If you wish to add more words to be ignored, use the
`gnus-ignored-adaptive-words` list instead.

Some may feel that short words shouldn’t count when doing adaptive
scoring. If so, you may set `gnus-adaptive-word-length-limit` to
an integer. Words shorter than this number will be ignored. This
variable defaults to `nil`.

When the scoring is done, `gnus-adaptive-word-syntax-table` is the
syntax table in effect. It is similar to the standard syntax table, but
it considers numbers to be non-word-constituent characters.

If `gnus-adaptive-word-minimum` is set to a number, the adaptive
word scoring process will never bring down the score of an article to
below this number. The default is `nil`.

If `gnus-adaptive-word-no-group-words` is set to `t`, gnus
won’t adaptively word score any of the words in the group name. Useful
for groups like ‘`comp.editors.emacs`’, where most of the subject
lines contain the word ‘`emacs`’.

After using this scheme for a while, it might be nice to write a
`gnus-psychoanalyze-user` command to go through the rules and see
what words you like and what words you don’t like. Or perhaps not.

Note that the adaptive word scoring thing is highly experimental and is
likely to change in the future. Initial impressions seem to indicate
that it’s totally useless as it stands. Some more work (involving more
rigorous statistical methods) will have to be done to make this useful.

---

Next: [Followups To Yourself](#Followups-To-Yourself), Previous: [Adaptive Scoring](#Adaptive-Scoring), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.7 Home Score File [¶](#Home-Score-File-1)

The score file where new score file entries will go is called the
*home score file*. This is normally (and by default) the score file
for the group itself. For instance, the home score file for
‘`gnu.emacs.gnus`’ is `gnu.emacs.gnus.SCORE`.

However, this may not be what you want. It is often convenient to share
a common home score file among many groups—all ‘`emacs`’ groups
could perhaps use the same home score file.

The variable that controls this is `gnus-home-score-file`. It can
be:

1. A string. Then this file will be used as the home score file for all
   groups.
2. A function. The result of this function will be used as the home score
   file. The function will be called with the name of the group as the
   parameter.
3. A list. The elements in this list can be:
   1. `(regexp file-name)`. If the regexp matches the
      group name, the file-name will be used as the home score file.
   2. A function. If the function returns non-`nil`, the result will
      be used as the home score file. The function will be called with the
      name of the group as the parameter.
   3. A string. Use the string as the home score file.

   The list will be traversed from the beginning towards the end looking
   for matches.

So, if you want to use just a single score file, you could say:

```
(setq gnus-home-score-file
      "my-total-score-file.SCORE")
```

If you want to use `gnu.SCORE` for all ‘`gnu`’ groups and
`rec.SCORE` for all ‘`rec`’ groups (and so on), you can say:

```
(setq gnus-home-score-file
      'gnus-hierarchial-home-score-file)
```

This is a ready-made function provided for your convenience.
Other functions include

`gnus-current-home-score-file` [¶](#index-gnus_002dcurrent_002dhome_002dscore_002dfile)
:   Return the “current” regular score file. This will make scoring
    commands add entry to the “innermost” matching score file.

If you want to have one score file for the ‘`emacs`’ groups and
another for the ‘`comp`’ groups, while letting all other groups use
their own home score files:

```
(setq gnus-home-score-file
      ;; All groups that match the regexp "\\.emacs"
      '(("\\.emacs" "emacs.SCORE")
        ;; All the comp groups in one score file
        ("^comp" "comp.SCORE")))
```

`gnus-home-adapt-file` works exactly the same way as
`gnus-home-score-file`, but says what the home adaptive score file
is instead. All new adaptive file entries will go into the file
specified by this variable, and the same syntax is allowed.

In addition to using `gnus-home-score-file` and
`gnus-home-adapt-file`, you can also use group parameters
(see [Group Parameters](#Group-Parameters)) and topic parameters (see [Topic Parameters](#Topic-Parameters)) to achieve much the same. Group and topic parameters take
precedence over this variable.

---

Next: [Scoring On Other Headers](#Scoring-On-Other-Headers), Previous: [Home Score File](#Home-Score-File), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.8 Followups To Yourself [¶](#Followups-To-Yourself-1)

Gnus offers two commands for picking out the `Message-ID` header in
the current buffer. Gnus will then add a score rule that scores using
this `Message-ID` on the `References` header of other
articles. This will, in effect, increase the score of all articles that
respond to the article in the current buffer. Quite useful if you want
to easily note when people answer what you’ve said.

`gnus-score-followup-article` [¶](#index-gnus_002dscore_002dfollowup_002darticle)
:   This will add a score to articles that directly follow up your own
    article.

`gnus-score-followup-thread` [¶](#index-gnus_002dscore_002dfollowup_002dthread)
:   This will add a score to all articles that appear in a thread “below”
    your own article.

These two functions are both primarily meant to be used in hooks like
`message-sent-hook`, like this:

```
(add-hook 'message-sent-hook 'gnus-score-followup-thread)
```

If you look closely at your own `Message-ID`, you’ll notice that
the first two or three characters are always the same. Here’s two of
mine:

```
<x6u3u47icf.fsf@eyesore.no>
<x6sp9o7ibw.fsf@eyesore.no>
```

So “my” ident on this machine is ‘`x6`’. This can be
exploited—the following rule will raise the score on all followups to
myself:

```
("references"
 ("<x6[0-9a-z]+\\.fsf\\(_-_\\)?@.*eyesore\\.no>"
  1000 nil r))
```

Whether it’s the first two or first three characters that are “yours”
is system-dependent.

---

Next: [Scoring Tips](#Scoring-Tips), Previous: [Followups To Yourself](#Followups-To-Yourself), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.9 Scoring On Other Headers [¶](#Scoring-On-Other-Headers-1)

Gnus is quite fast when scoring the “traditional”
headers—‘`From`’, ‘`Subject`’ and so on. However, scoring
other headers requires writing a `head` scoring rule, which means
that Gnus has to request every single article from the back end to find
matches. This takes a long time in big groups.

You can inhibit this slow scoring on headers or body by setting the
variable `gnus-inhibit-slow-scoring`. If
`gnus-inhibit-slow-scoring` is regexp, slow scoring is inhibited if
the group matches the regexp. If it is `t`, slow scoring on it is
inhibited for all groups.

Now, there’s not much you can do about the slowness for news groups, but for
mail groups, you have greater control. In [To From Newsgroups](#To-From-Newsgroups),
it’s explained in greater detail what this mechanism does, but here’s
a cookbook example for `nnml` on how to allow scoring on the
‘`To`’ and ‘`Cc`’ headers.

Put the following in your `~/.gnus.el` file.

```
(setq gnus-extra-headers '(To Cc Newsgroups Keywords)
      nnmail-extra-headers gnus-extra-headers)
```

Restart Gnus and rebuild your `nnml` overview files with the
`M-x nnml-generate-nov-databases` command. This will take a long
time if you have much mail.

Now you can score on ‘`To`’ and ‘`Cc`’ as “extra headers” like
so: `I e s p To RET <your name> RET`.

See? Simple.

---

Next: [Reverse Scoring](#Reverse-Scoring), Previous: [Scoring On Other Headers](#Scoring-On-Other-Headers), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.10 Scoring Tips [¶](#Scoring-Tips-1)

*Crossposts* [¶](#index-crossposts)
:   If you want to lower the score of crossposts, the line to match on is
    the `Xref` header.

    ```
    ("xref" (" talk.politics.misc:" -1000))
    ```

*Multiple crossposts*
:   If you want to lower the score of articles that have been crossposted to
    more than, say, 3 groups:

    ```
    ("xref"
      ("[^:\n]+:[0-9]+ +[^:\n]+:[0-9]+ +[^:\n]+:[0-9]+"
       -1000 nil r))
    ```

*Matching on the body*
:   This is generally not a very good idea—it takes a very long time.
    Gnus actually has to fetch each individual article from the server. But
    you might want to anyway, I guess. Even though there are three match
    keys (`Head`, `Body` and `All`), you should choose one
    and stick with it in each score file. If you use any two, each article
    will be fetched *twice*. If you want to match a bit on the
    `Head` and a bit on the `Body`, just use `All` for all
    the matches.

*Marking as read*
:   You will probably want to mark articles that have scores below a certain
    number as read. This is most easily achieved by putting the following
    in your `all.SCORE` file:

    ```
    ((mark -100))
    ```

    You may also consider doing something similar with `expunge`.

*Negated character classes*
:   If you say stuff like `[^abcd]*`, you may get unexpected results.
    That will match newlines, which might lead to, well, The Unknown. Say
    `[^abcd\n]*` instead.

*Continuation lines when scoring on other headers*
:   When scoring on other headers using the `Head` or `All` match
    keys and regexp matching, your regular expression must take into account
    header continuation lines. For example, this naive attempt to match
    messages including a particular address in the `To` field:

    ```
    ("head" "^To: .*\\bspwhitton@spwhitton\\.name\\b" r)
    ```

    will fail to match a message with a `To` header like this:

    ```
    To: A long description of the Emacs devel list <emacs-devel@gnu.org>,
     spwhitton@spwhitton.name, 12345@debbugs.gnu.org
    ```

    You can handle this issue with a regexp of this form:

    ```
    ("head" "^To: .*\\(?:\n[\s\t].*\\)*\\bspwhitton@spwhitton\\.name\\b" r)
    ```

---

Next: [Global Score Files](#Global-Score-Files), Previous: [Scoring Tips](#Scoring-Tips), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.11 Reverse Scoring [¶](#Reverse-Scoring-1)

If you want to keep just articles that have ‘`Sex with Emacs`’ in the
subject header, and expunge all other articles, you could put something
like this in your score file:

```
(("subject"
  ("Sex with Emacs" 2))
 (mark 1)
 (expunge 1))
```

So, you raise all articles that match ‘`Sex with Emacs`’ and mark the
rest as read, and expunge them to boot.

---

Next: [Kill Files](#Kill-Files), Previous: [Reverse Scoring](#Reverse-Scoring), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.12 Global Score Files [¶](#Global-Score-Files-1)

Sure, other newsreaders have “global kill files”. These are usually
nothing more than a single kill file that applies to all groups, stored
in the user’s home directory. Bah! Puny, weak newsreaders!

What I’m talking about here are Global Score Files. Score files from
all over the world, from users everywhere, uniting all nations in one
big, happy score file union! Ange-score! New and untested!

All you have to do to use other people’s score files is to set the
`gnus-global-score-files` variable. One entry for each score file,
or each score file directory. Gnus will decide by itself what score
files are applicable to which group.

To use the score file
`/ftp@ftp.gnus.org:/pub/larsi/ding/score/soc.motss.SCORE` and
all score files in the `/ftp@ftp.some-where:/pub/score` directory,
say this:

```
(setq gnus-global-score-files
      '("/ftp@ftp.gnus.org:/pub/larsi/ding/score/soc.motss.SCORE"
        "/ftp@ftp.some-where:/pub/score/"))
```

Simple, eh? Directory names must end with a ‘`/`’. These
directories are typically scanned only once during each Gnus session.
If you feel the need to manually re-scan the remote directories, you can
use the `gnus-score-search-global-directories` command.

Note that, at present, using this option will slow down group entry
somewhat. (That is—a lot.)

If you want to start maintaining score files for other people to use,
just put your score file up for anonymous ftp and announce it to the
world. Become a retro-moderator! Participate in the retro-moderator
wars sure to ensue, where retro-moderators battle it out for the
sympathy of the people, luring them to use their score files on false
premises! Yay! The net is saved!

Here are some tips for the would-be retro-moderator, off the top of my
head:

* Articles heavily crossposted are probably junk.
* To lower a single inappropriate article, lower by `Message-ID`.
* Particularly brilliant authors can be raised on a permanent basis.
* Authors that repeatedly post off-charter for the group can safely be
  lowered out of existence.
* Set the `mark` and `expunge` atoms to obliterate the nastiest
  articles completely.
* Use expiring score entries to keep the size of the file down. You
  should probably have a long expiry period, though, as some sites keep
  old articles for a long time.

… I wonder whether other newsreaders will support global score files
in the future. *Snicker*. Yup, any day now, newsreaders like Blue
Wave, xrn and 1stReader are bound to implement scoring. Should we start
holding our breath yet?

---

Next: [Converting Kill Files](#Converting-Kill-Files), Previous: [Global Score Files](#Global-Score-Files), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.13 Kill Files [¶](#Kill-Files-1)

Gnus still supports those pesky old kill files. In fact, the kill file
entries can now be expiring, which is something I wrote before Daniel
Quinlan thought of doing score files, so I’ve left the code in there.

In short, kill processing is a lot slower (and I do mean *a lot*)
than score processing, so it might be a good idea to rewrite your kill
files into score files.

Anyway, a kill file is a normal `emacs-lisp` file. You can put any
forms into this file, which means that you can use kill files as some
sort of primitive hook function to be run on group entry, even though
that isn’t a very good idea.

Normal kill files look like this:

```
(gnus-kill "From" "Lars Ingebrigtsen")
(gnus-kill "Subject" "ding")
(gnus-expunge "X")
```

This will mark every article written by me as read, and remove the
marked articles from the summary buffer. Very useful, you’ll agree.

Other programs use a totally different kill file syntax. If Gnus
encounters what looks like a `rn` kill file, it will take a stab at
interpreting it.

Two summary functions for editing a GNUS kill file:

`M-k` [¶](#index-M_002dk-_0028Summary_0029)
:   Edit this group’s kill file (`gnus-summary-edit-local-kill`).

`M-K` [¶](#index-M_002dK-_0028Summary_0029)
:   Edit the general kill file (`gnus-summary-edit-global-kill`).

Two group mode functions for editing the kill files:

`M-k` [¶](#index-M_002dk-_0028Group_0029)
:   Edit this group’s kill file (`gnus-group-edit-local-kill`).

`M-K` [¶](#index-M_002dK-_0028Group_0029)
:   Edit the general kill file (`gnus-group-edit-global-kill`).

Kill file variables:

`gnus-kill-file-name` [¶](#index-gnus_002dkill_002dfile_002dname)
:   A kill file for the group ‘`soc.motss`’ is normally called
    `soc.motss.KILL`. The suffix appended to the group name to get
    this file name is detailed by the `gnus-kill-file-name` variable.
    The “global” kill file (not in the score file sense of “global”, of
    course) is just called `KILL`.

`gnus-kill-save-kill-file` [¶](#index-gnus_002dkill_002dsave_002dkill_002dfile)
:   If this variable is non-`nil`, Gnus will save the
    kill file after processing, which is necessary if you use expiring
    kills.

`gnus-apply-kill-hook` [¶](#index-gnus_002dapply_002dkill_002dhook)
:   A hook called to apply kill files to a group. It is
    `(gnus-apply-kill-file)` by default. If you want to ignore the
    kill file if you have a score file for the same group, you can set this
    hook to `(gnus-apply-kill-file-unless-scored)`. If you don’t want
    kill files to be processed, you should set this variable to `nil`.

`gnus-kill-file-mode-hook` [¶](#index-gnus_002dkill_002dfile_002dmode_002dhook)
:   A hook called in kill-file mode buffers.

---

Next: [Advanced Scoring](#Advanced-Scoring), Previous: [Kill Files](#Kill-Files), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.14 Converting Kill Files [¶](#Converting-Kill-Files-1)

If you have loads of old kill files, you may want to convert them into
score files. If they are “regular”, you can use
the `gnus-kill-to-score.el` package; if not, you’ll have to do it
by hand.

The kill to score conversion package isn’t included in Emacs by default.
You can fetch it from the contrib directory of the Gnus distribution or
from
<http://heim.ifi.uio.no/~larsi/ding-various/gnus-kill-to-score.el>.

If your old kill files are very complex—if they contain more
non-`gnus-kill` forms than not, you’ll have to convert them by
hand. Or just let them be as they are. Gnus will still use them as
before.

---

Next: [Score Decays](#Score-Decays), Previous: [Converting Kill Files](#Converting-Kill-Files), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.15 Advanced Scoring [¶](#Advanced-Scoring-1)

Scoring on Subjects and From headers is nice enough, but what if you’re
really interested in what a person has to say only when she’s talking
about a particular subject? Or what if you really don’t want to
read what person A has to say when she’s following up to person B, but
want to read what she says when she’s following up to person C?

By using advanced scoring rules you may create arbitrarily complex
scoring patterns.

* [Advanced Scoring Syntax](#Advanced-Scoring-Syntax)
* [Advanced Scoring Examples](#Advanced-Scoring-Examples)
* [Advanced Scoring Tips](#Advanced-Scoring-Tips)

---

Next: [Advanced Scoring Examples](#Advanced-Scoring-Examples), Up: [Advanced Scoring](#Advanced-Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.15.1 Advanced Scoring Syntax [¶](#Advanced-Scoring-Syntax-1)

Ordinary scoring rules have a string as the first element in the rule.
Advanced scoring rules have a list as the first element. The second
element is the score to be applied if the first element evaluated to a
non-`nil` value.

These lists may consist of three logical operators, one redirection
operator, and various match operators.

Logical operators:

`&`

`and`
:   This logical operator will evaluate each of its arguments until it finds
    one that evaluates to `false`, and then it’ll stop. If all arguments
    evaluate to `true` values, then this operator will return
    `true`.

`|`

`or`
:   This logical operator will evaluate each of its arguments until it finds
    one that evaluates to `true`. If no arguments are `true`,
    then this operator will return `false`.

`!`

`not`

`¬`
:   This logical operator only takes a single argument. It returns the
    logical negation of the value of its argument.

There is an *indirection operator* that will make its arguments
apply to the ancestors of the current article being scored. For
instance, `1-` will make score rules apply to the parent of the
current article. `2-` will make score rules apply to the
grandparent of the current article. Alternatively, you can write
`^^`, where the number of `^`s (carets) says how far back into
the ancestry you want to go.

Finally, we have the match operators. These are the ones that do the
real work. Match operators are header name strings followed by a match
and a match type. A typical match operator looks like ‘`("from"
"Lars Ingebrigtsen" s)`’. The header names are the same as when using
simple scoring, and the match types are also the same.

---

Next: [Advanced Scoring Tips](#Advanced-Scoring-Tips), Previous: [Advanced Scoring Syntax](#Advanced-Scoring-Syntax), Up: [Advanced Scoring](#Advanced-Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.15.2 Advanced Scoring Examples [¶](#Advanced-Scoring-Examples-1)

Please note that the following examples are score file rules. To
make a complete score file from them, surround them with another pair
of parentheses.

Let’s say you want to increase the score of articles written by Lars
when he’s talking about Gnus:

```
((&
  ("from" "Lars Ingebrigtsen")
  ("subject" "Gnus"))
 1000)
```

Quite simple, huh?

When he writes long articles, he sometimes has something nice to say:

```
((&
  ("from" "Lars Ingebrigtsen")
  (|
   ("subject" "Gnus")
   ("lines" 100 >)))
 1000)
```

However, when he responds to things written by Reig Eigil Logge, you
really don’t want to read what he’s written:

```
((&
  ("from" "Lars Ingebrigtsen")
  (1- ("from" "Reig Eigil Logge")))
 -100000)
```

Everybody that follows up Redmondo when he writes about disappearing
socks should have their scores raised, but only when they talk about
white socks. However, when Lars talks about socks, it’s usually not
very interesting:

```
((&
  (1-
   (&
    ("from" "redmondo@.*no" r)
    ("body" "disappearing.*socks" t)))
  (! ("from" "Lars Ingebrigtsen"))
  ("body" "white.*socks"))
 1000)
```

Suppose you’re reading a high volume group and you’re only interested
in replies. The plan is to score down all articles that don’t have
subject that begin with "Re:", "Fw:" or "Fwd:" and then score up all
parents of articles that have subjects that begin with reply marks.

```
((! ("subject" "re:\\|fwd?:" r))
  -200)
((1- ("subject" "re:\\|fwd?:" r))
  200)
```

The possibilities are endless.

---

Previous: [Advanced Scoring Examples](#Advanced-Scoring-Examples), Up: [Advanced Scoring](#Advanced-Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 8.15.3 Advanced Scoring Tips [¶](#Advanced-Scoring-Tips-1)

The `&` and `|` logical operators do short-circuit logic.
That is, they stop processing their arguments when it’s clear what the
result of the operation will be. For instance, if one of the arguments
of an `&` evaluates to `false`, there’s no point in evaluating
the rest of the arguments. This means that you should put slow matches
(‘`body`’, ‘`header`’) last and quick matches (‘`from`’,
‘`subject`’) first.

The indirection arguments (`1-` and so on) will make their
arguments work on previous generations of the thread. If you say
something like:

```
...
(1-
 (1-
  ("from" "lars")))
...
```

Then that means “score on the from header of the grandparent of the
current article”. An indirection is quite fast, but it’s better to say:

```
(1-
 (&
  ("from" "Lars")
  ("subject" "Gnus")))
```

than it is to say:

```
(&
 (1- ("from" "Lars"))
 (1- ("subject" "Gnus")))
```

---

Previous: [Advanced Scoring](#Advanced-Scoring), Up: [Scoring](#Scoring)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 8.16 Score Decays [¶](#Score-Decays-1)

You may find that your scores have a tendency to grow without
bounds, especially if you’re using adaptive scoring. If scores get too
big, they lose all meaning—they simply max out and it’s difficult to
use them in any sensible way.

Gnus provides a mechanism for decaying scores to help with this problem.
When score files are loaded and `gnus-decay-scores` is
non-`nil`, Gnus will run the score files through the decaying
mechanism thereby lowering the scores of all non-permanent score rules.
If `gnus-decay-scores` is a regexp, only score files matching this
regexp are treated. E.g., you may set it to ‘`\\.ADAPT\\'`’ if only
*adaptive* score files should be decayed. The decay itself if
performed by the `gnus-decay-score-function` function, which is
`gnus-decay-score` by default. Here’s the definition of that
function:

```
(defun gnus-decay-score (score)
  "Decay SCORE according to `gnus-score-decay-constant'
and `gnus-score-decay-scale'."
  (let ((n (- score
              (* (if (< score 0) -1 1)
                 (min (abs score)
                      (max gnus-score-decay-constant
                           (* (abs score)
                              gnus-score-decay-scale)))))))
    (floor n)))
```

`gnus-score-decay-constant` is 3 by default and
`gnus-score-decay-scale` is 0.05. This should cause the following:

1. Scores between −3 and 3 will be set to 0 when this function is called.
2. Scores with magnitudes between 3 and 60 will be shrunk by 3.
3. Scores with magnitudes greater than 60 will be shrunk by 5% of the
   score.

If you don’t like this decay function, write your own. It is called
with the score to be decayed as its only parameter, and it should return
the new score, which should be an integer.

Gnus will try to decay scores once a day. If you haven’t run Gnus for
four days, Gnus will decay the scores four times, for instance.

---

Next: [Various](#Various), Previous: [Scoring](#Scoring), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 9 Searching [¶](#Searching-1)

Gnus has various ways of finding articles that match certain criteria
(from a particular author, on a certain subject, etc.). The simplest
method is to enter a group and then either "limit" the summary buffer
to the desired articles using the limiting commands (see [Limiting](#Limiting)),
or searching through messages in the summary buffer (see [Searching for Articles](#Searching-for-Articles)).

Limiting commands and summary buffer searching work on articles
already fetched from the servers, and these commands won’t query the
server for additional articles. While simple, these methods are
therefore inadequate if the desired articles span multiple groups, or
if the group is so large that fetching all articles is impractical.

It’s possible to search a backend more thoroughly using an associated
search engine. Some backends come with their own search engine: IMAP
servers, for instance, do their own searching. Other backends, for
example a local `nnmaildir` installation, might require the user
to manually set up some sort of search indexing. Default associations
between backends and engines can be defined in
`gnus-search-default-engines`, and engines can also be defined on
a per-backend basis (see [Search Engines](#Search-Engines)).

Once the search engines are set up, you can search for messages in
groups from one or more backends, and show the results in a group.
The groups that hold search results are created on the nnselect
backend, and can be either ephemeral or persistent (see [Creating Search Groups](#Creating-Search-Groups)).

Search queries can be specified one of two ways: either using the
syntax of the engine responsible for the group you’re searching, or
using Gnus’ generalized search syntax. Set the option
`gnus-search-use-parsed-queries` to a non-`nil` value to use the
generalized syntax. The advantage of this syntax is that, if you have
multiple backends indexed by different engines, you don’t need to
remember which one you’re searching—it’s also possible to issue the
same query against multiple groups, indexed by different engines, at
the same time. It also provides a few other conveniences including
relative date parsing and tie-ins into other Emacs packages. For
details on Gnus’ query language, see [Search Queries](#Search-Queries).

* [Search Engines](#Search-Engines)
* [Creating Search Groups](#Creating-Search-Groups)
* [Search Queries](#Search-Queries)
* [nnmairix](#nnmairix)
* [Migrating from nnir](#nnir)

---

Next: [Creating Search Groups](#Creating-Search-Groups), Up: [Searching](#Searching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.1 Search Engines [¶](#Search-Engines-1)

In order to search for messages from any given server, that server
must have a search engine associated with it. IMAP servers do their
own searching, and searching IMAP groups will work with no additional
configuration, but in all other cases the user will have to manually
specify an engine to use. This can be done at two different levels:
by server type, or on a per-server basis.

The option `gnus-search-default-engines` assigns search engines
by server type. Its value is an alist mapping symbols indicating a
server type (e.g. `nnmaildir` or `nnml`) to symbols
indicating a search engine class. The built-in search engine symbols
are:

* `gnus-search-imap`
* `gnus-search-find-grep`
* `gnus-search-notmuch`
* `gnus-search-swish-e`
* `gnus-search-swish++`
* `gnus-search-mairix`
* `gnus-search-namazu`
* `gnus-search-mu`

If you need more granularity, you can specify a search engine in the
server definition, using the `gnus-search-engine` key, whether
that be in your `.gnus.el` config file, or through Gnus’ server
buffer. That might look like:

```
'(nnmaildir "My Mail"
   (directory "/home/user/.mail")
   (gnus-search-engine gnus-search-notmuch
     (config-file "/home/user/.mail/.notmuch_config")))
```

Search engines like notmuch, namazu, mairix and mu are similar in
behavior: they use a local executable to create an index of a message
store, and run command line search queries against those messages,
and return a list of absolute file names of matching messages.

These engines have a handful of configuration parameters in common.
These common parameters are:

`program`
:   The name of the executable. Defaults to the plain
    program name such as `notmuch` or `namazu`.

`config-file`
:   The absolute filename of the configuration file for this search
    engine.

`remove-prefix`
:   The directory part to be removed from the filenames returned by the
    search query. This absolute path should include everything up to the
    top level of the message store. Note that this is the path to the
    location of the actual messages, not to the search engine’s indexes
    (nor, in the case of Mairix, to its symlink directories).

`switches`
:   Additional command-line switches to be fed to the search program. The
    value of this parameter must be a list of strings, one string per
    switch.

The options above can be set in one of two ways: using a customization
option that is set for all engines of that type, or on a per-engine
basis in your server configuration files.

The customization options are formed on the pattern
`gnus-search-engine-parameter`. For instance, to use a
non-standard notmuch program, you might set
`gnus-search-notmuch-program` to `/usr/local/bin/notmuch`.
This would apply to all notmuch engines. The engines that use these
options are: “notmuch”, “namazu”, “mairix”, “mu”, “swish-e”
and “swish++”.

Alternately, the options can be set directly on your Gnus server
definitions, for instance, in the `nnmaildir` example above.
Note that the server options are part of the `gnus-search-engine`
sexp, and the option symbol and value form a two-element list, not a
cons cell.

The namazu and swish-e engines each have one additional option,
specifying where to store their index files. For namazu it is
`index-directory`, and should be a single directory path. For
swish-e it is `index-files`, and should be a list of strings.

All indexed search engines come with their own method of updating
their search indexes to include newly-arrived messages. Gnus
currently provides no convenient interface for this, and you’ll have
to manage updates yourself, though this will likely change in the
future.

Lastly, all search engines accept a `raw-queries-p` option. This
indicates that engines of this type (or this particular engine) should
always use raw queries, never parsed (see [Search Queries](#Search-Queries)).

---

Next: [Search Queries](#Search-Queries), Previous: [Search Engines](#Search-Engines), Up: [Searching](#Searching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.2 Creating Search Groups [¶](#Creating-Search-Groups-1)

In the group buffer typing `G G` will search the group on the
current line by calling `gnus-group-read-ephemeral-search-group`.
This prompts for a query string, creates an ephemeral `nnselect`
group containing the articles that match this query, and takes you to
a summary buffer showing these articles. Articles may then be read,
moved and deleted using the usual commands.

The `nnselect` group made in this way is `ephemeral`: it
will disappear upon exit from the group. However changes made in the
group are permanently reflected in the real groups from which the
articles are drawn. If you want to create a *persistent* group
that sticks around after exit from the summary buffer, you can call
`gnus-group-make-search-group` (bound to `G g`).

Unlike persistent groups, ephemeral groups by default do not run
articles through the expiry process on exiting. If you want expiry to
happen in ephemeral search groups you can customize the variable
`nnselect-allow-ephemeral-expiry`. In all cases the expiry
process uses the underlying group’s expiry parameters.

So you just performed a search whose results are so fabulous you
wished you had done a persistent search rather than an ephemeral one?
No problem; you can create such a group by calling
`gnus-summary-make-group-from-search` (bound to `C-c C-p`)
from the ephemeral summary buffer.

It is occasionally convenient to view articles found through searching
in their original group. You can *warp* (i.e., jump) to the
original group for the article on the current line with `A W`, aka
`gnus-warp-to-article`.

You say you want to search more than just the group on the current
line? No problem: just process-mark the groups you want to search.
You want even more? Initiating a search with the cursor on a topic
heading will search all the groups under that topic.

Still not enough? OK, in the server buffer
`gnus-group-read-ephemeral-search-group` (here bound to `G`)
will search all groups from the server on the current line. Too much?
Want to ignore certain groups when searching, like spam groups? Just
customize `gnus-search-ignored-newsgroups`: groups matching this
regexp will not be searched.

---

Next: [nnmairix](#nnmairix), Previous: [Creating Search Groups](#Creating-Search-Groups), Up: [Searching](#Searching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.3 Search Queries [¶](#Search-Queries-1)

Gnus provides an optional unified search syntax that can be used
across all supported search engines. This can be convenient in that
you don’t have to remember different search syntaxes; it’s also
possible to mark multiple groups indexed by different engines and
issue a single search against them.

Set the option `gnus-search-use-parsed-queries` to non-`nil`
to enable this—it is `nil` by default. Even if it is
non-`nil`, it’s still possible to turn off parsing for a class of
engines or a single engine (see [Search Engines](#Search-Engines)), or a single
search by giving a prefix argument to any of the search commands.

The search syntax is fairly simple: keys and values are separated by a
colon, multi-word values must be quoted, “and” is implicit, “or”
is explicit, “not” will negate the following expression (or keys can
be prefixed with a “-”),and parentheses can be used to group logical
sub-clauses. For example:

```
(from:john or from:peter) subject:"lunch tomorrow" since:3d
```

The syntax is made to be accepted by a wide range of engines, and thus
will happily accept most input, valid or not. Some terms will only be
meaningful to some engines; other engines will drop them silently.

Key completion is offered on `TAB`, but it’s also possible to
enter the query with abbreviated keys, which will be expanded during
parsing. If a key is abbreviated to the point of ambiguity (for
instance, “s:” could be “subject:” or “since:”), an error will
be raised.

Supported keys include all the usual mail headers: “from”,
“subject”, “cc”, etc. Other keys are:

‘`body`’
:   The body of the message.

‘`recipient`’
:   Equivalent to ‘`to or cc or bcc`’.

‘`address`’
:   Equivalent to ‘`from or recipient`’.

‘`id`’
:   The keys ‘`message-id`’ and ‘`id`’ are equivalent.

‘`mark`’
:   Accepts ‘`flag`’, ‘`seen`’, ‘`read`’ or ‘`replied`’, or
    any of Gnus’ single-letter representations of those marks, e.g.
    ‘`mark:R`’ for ‘`read`’.

‘`tag`’
:   This is interpreted as ‘`keyword`’ for IMAP and ‘`tag`’ for
    notmuch.

‘`attachment`’
:   Matches the attachment file name.

‘`before`’
:   Date is exclusive; see below for date parsing.

‘`after`’
:   Date is inclusive; can also use ‘`since`’.

‘`thread`’
:   Return entire message threads, not just individual messages.

‘`raw`’
:   Do not parse this particular search.

‘`limit`’
:   Limit the results to this many messages. When searching multiple
    groups this may give undesired results, as the limiting happens before
    sorting.

‘`grep`’
:   Only applicable to “local index” engines such as mairix or notmuch.
    On systems with a grep command, additionally filter the results by
    using the value of this term as a grep regexp.

Elisp-based contact management packages (e.g. BBDB or EBDB) can push
completion tables onto the variable `gnus-search-contact-tables`,
allowing auto-completion of contact names and addresses for keys like
‘`from`’ or ‘`to`’.

* [Date value parsing](#Date-value-parsing)

#### 9.3.1 Date value parsing [¶](#Date-value-parsing)

Date-type keys (see `gnus-search-date-keys`) will accept a wide
variety of values. First, anything that `parse-time-string` can
parse is acceptable. Dates with missing values will be interpreted as
the most recent occurrence thereof: for instance “march 03” is the
most recent March 3rd. Lastly, it’s possible to use relative
specifications, such as “3d” (three days ago). This format also accepts
w, m and y.

When creating persistent search groups, the search is saved unparsed,
and re-parsed every time the group is updated. So a permanent search
group with a query like:

```
from:"my boss" mark:flag since:1w
```

would always contain only messages from the past seven days.

---

Next: [Migrating from nnir](#nnir), Previous: [Search Queries](#Search-Queries), Up: [Searching](#Searching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.4 nnmairix [¶](#nnmairix-1)

This section is now mostly obsolete, as mairix can be used as a regular
search engine, including persistent search groups, with
`nnselect`.

This paragraph describes how to set up mairix and the back end
`nnmairix` for indexing and searching your mail from within
Gnus. Additionally, you can create permanent “smart” groups which are
bound to mairix searches and are automatically updated.

* [About mairix](#About-mairix)
* [nnmairix requirements](#nnmairix-requirements)
* [What nnmairix does](#What-nnmairix-does)
* [Setting up mairix](#Setting-up-mairix)
* [Configuring nnmairix](#Configuring-nnmairix)
* [nnmairix keyboard shortcuts](#nnmairix-keyboard-shortcuts)
* [Propagating marks](#Propagating-marks)
* [nnmairix tips and tricks](#nnmairix-tips-and-tricks)
* [nnmairix caveats](#nnmairix-caveats)

---

Next: [nnmairix requirements](#nnmairix-requirements), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.1 About mairix [¶](#About-mairix-1)

Mairix is a tool for indexing and searching words in locally stored
mail. It was written by Richard Curnow and is licensed under the
GPL. Mairix comes with most popular GNU/Linux distributions, but it also
runs under Windows (with cygwin), macOS and Solaris. The website can
be found at
<http://www.rpcurnow.force9.co.uk/mairix/index.html>

Though mairix might not be as flexible as other search tools like
swish++ or namazu, it has the prime advantage of being incredibly
fast. On current systems, it can easily search through headers and
message bodies of thousands and thousands of mails in well under a
second. Building the database necessary for searching might take a
minute or two, but only has to be done once fully. Afterwards, the
updates are done incrementally and therefore are really fast, too.
Additionally, mairix is very easy to set up.

For maximum speed though, mairix should be used with mails stored in
`Maildir` or `MH` format (this includes the `nnml` back
end), although it also works with mbox. Mairix presents the search
results by populating a *virtual* maildir/MH folder with symlinks
which point to the “real” message files (if mbox is used, copies are
made). Since mairix already presents search results in such a virtual
mail folder, it is very well suited for using it as an external program
for creating *smart* mail folders, which represent certain mail
searches.

---

Next: [What nnmairix does](#What-nnmairix-does), Previous: [About mairix](#About-mairix), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.2 nnmairix requirements [¶](#nnmairix-requirements-1)

Mairix searches local mail—that means, mairix absolutely must have
direct access to your mail folders. If your mail resides on another
server (e.g., an IMAP server) and you happen to have shell
access, `nnmairix` supports running mairix remotely, e.g., via ssh.

Additionally, `nnmairix` only supports the following Gnus back
ends: `nnml`, `nnmaildir`, and `nnimap`. You must use
one of these back ends for using `nnmairix`. Other back ends, like
`nnmbox`, `nnfolder` or `nnmh`, won’t work.

If you absolutely must use mbox and still want to use `nnmairix`,
you can set up a local IMAP server, which you then access via
`nnimap`. This is a rather massive setup for accessing some mbox
files, so just change to MH or Maildir already... However, if you’re
really, really passionate about using mbox, you might want to look into
the package `mairix.el`, which comes with Emacs.

---

Next: [Setting up mairix](#Setting-up-mairix), Previous: [nnmairix requirements](#nnmairix-requirements), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.3 What nnmairix does [¶](#What-nnmairix-does-1)

The back end `nnmairix` enables you to call mairix from within Gnus,
either to query mairix with a search term or to update the
database. While visiting a message in the summary buffer, you can use
several pre-defined shortcuts for calling mairix, e.g., to quickly
search for all mails from the sender of the current message or to
display the whole thread associated with the message, even if the
mails are in different folders.

Additionally, you can create permanent `nnmairix` groups which are bound
to certain mairix searches. This way, you can easily create a group
containing mails from a certain sender, with a certain subject line or
even for one specific thread based on the Message-ID. If you check for
new mail in these folders (e.g., by pressing `g` or `M-g`), they
automatically update themselves by calling mairix.

You might ask why you need `nnmairix` at all, since mairix already
creates the group, populates it with links to the mails so that you can
then access it with Gnus, right? Well, this *might* work, but often
does not—at least not without problems. Most probably you will get
strange article counts, and sometimes you might see mails which Gnus
claims have already been canceled and are inaccessible. This is due to
the fact that Gnus isn’t really amused when things are happening behind
its back. Another problem can be the mail back end itself, e.g., if you
use mairix with an IMAP server (I had Dovecot complaining
about corrupt index files when mairix changed the contents of the search
group). Using `nnmairix` should circumvent these problems.

`nnmairix` is not really a mail back end—it’s actually more like
a wrapper, sitting between a “real” mail back end where mairix stores
the searches and the Gnus front end. You can choose between three
different mail back ends for the mairix folders: `nnml`,
`nnmaildir` or `nnimap`. `nnmairix` will call the mairix
binary so that the search results are stored in folders named
`zz_mairix-<NAME>-<NUMBER>` on this mail back end, but it will
present these folders in the Gnus front end only with `<NAME>`.
You can use an existing mail back end where you already store your mail,
but if you’re uncomfortable with `nnmairix` creating new mail
groups alongside your other mail, you can also create, e.g., a new
`nnmaildir` or `nnml` server exclusively for mairix, but then
make sure those servers do not accidentally receive your new mail
(see [nnmairix caveats](#nnmairix-caveats)). A special case exists if you want to use
mairix remotely on an IMAP server with `nnimap`—here the mairix
folders and your other mail must be on the same `nnimap` back end.

---

Next: [Configuring nnmairix](#Configuring-nnmairix), Previous: [What nnmairix does](#What-nnmairix-does), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.4 Setting up mairix [¶](#Setting-up-mairix-1)

First: create a backup of your mail folders (see [nnmairix caveats](#nnmairix-caveats)).

Setting up mairix is easy: simply create a `.mairixrc` file with
(at least) the following entries:

```
# Your Maildir/MH base folder
base=~/Maildir
```

This is the base folder for your mails. All the following directories
are relative to this base folder. If you want to use `nnmairix`
with `nnimap`, this base directory has to point to the mail
directory where the IMAP server stores the mail folders!

```
maildir= ... your maildir folders which should be indexed ...
mh= ... your nnml/mh folders which should be indexed ...
mbox = ... your mbox files which should be indexed ...
```

This specifies all your mail folders and mbox files (relative to the
base directory!) you want to index with mairix. Note that the
`nnml` back end saves mails in MH format, so you have to put those
directories in the `mh` line. See the example at the end of this
section and mairixrc’s man-page for further details.

```
omit=zz_mairix-*
```

This should make sure that you don’t accidentally index the mairix
search results. You can change the prefix of these folders with the
variable `nnmairix-group-prefix`.

```
mformat= ... 'maildir' or 'mh' ...
database= ... location of database file ...
```

The `format` setting specifies the output format for the mairix
search folder. Set this to `mh` if you want to access search results
with `nnml`. Otherwise choose `maildir`.

To summarize, here is my shortened `.mairixrc` file as an example:

```
base=~/Maildir
maildir=.personal:.work:.logcheck:.sent
mh=../Mail/nnml/*...
mbox=../mboxmail/mailarchive_year*
mformat=maildir
omit=zz_mairix-*
database=~/.mairixdatabase
```

In this case, the base directory is `~/Maildir`, where all my Maildir
folders are stored. As you can see, the folders are separated by
colons. If you wonder why every folder begins with a dot: this is
because I use Dovecot as IMAP server, which again uses
`Maildir++` folders. For testing nnmairix, I also have some
`nnml` mail, which is saved in `~/Mail/nnml`. Since this has
to be specified relative to the `base` directory, the `../Mail`
notation is needed. Note that the line ends in `*...`, which means
to recursively scan all files under this directory. Without the three
dots, the wildcard `*` will not work recursively. I also have some
old mbox files with archived mail lying around in `~/mboxmail`.
The other lines should be obvious.

See the man page for `mairixrc` for details and further options,
especially regarding wildcard usage, which may be a little different
than you are used to.

Now simply call `mairix` to create the index for the first time.
Note that this may take a few minutes, but every following index will do
the updates incrementally and hence is very fast.

---

Next: [nnmairix keyboard shortcuts](#nnmairix-keyboard-shortcuts), Previous: [Setting up mairix](#Setting-up-mairix), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.5 Configuring nnmairix [¶](#Configuring-nnmairix-1)

In group mode, type `G b c`
(`nnmairix-create-server-and-default-group`). This will ask you for all
necessary information and create a `nnmairix` server as a foreign
server. You will have to specify the following:

* The **name** of the `nnmairix` server—choose whatever you
  want.
* The name of the **back end server** where mairix should store its
  searches. This must be a full server name, like `nnml:mymail`.
  Just hit `TAB` to see the available servers. Currently, servers
  which are accessed through `nnmaildir`, `nnimap` and
  `nnml` are supported. As explained above, for locally stored
  mails, this can be an existing server where you store your mails.
  However, you can also create, e.g., a new `nnmaildir` or `nnml`
  server exclusively for `nnmairix` in your secondary select methods
  (see [Finding the News](#Finding-the-News)). If you use a secondary `nnml` server
  just for mairix, make sure that you explicitly set the server variable
  `nnml-get-new-mail` to `nil`, or you might lose mail
  (see [nnmairix caveats](#nnmairix-caveats)). If you want to use mairix remotely on an
  IMAP server, you have to choose the corresponding
  `nnimap` server here.
* The **command** to call the mairix binary. This will usually just
  be `mairix`, but you can also choose something like `ssh
  SERVER mairix` if you want to call mairix remotely, e.g., on your
  IMAP server. If you want to add some default options to
  mairix, you could do this here, but better use the variable
  `nnmairix-mairix-search-options` instead.
* The name of the **default search group**. This will be the group
  where all temporary mairix searches are stored, i.e., all searches which
  are not bound to permanent `nnmairix` groups. Choose whatever you
  like.
* If the mail back end is `nnimap` or `nnmaildir`, you will be
  asked if you work with **Maildir++**, i.e., with hidden maildir
  folders (=beginning with a dot). For example, you have to answer
  ‘`yes`’ here if you work with the Dovecot IMAP
  server. Otherwise, you should answer ‘`no`’ here.

---

Next: [Propagating marks](#Propagating-marks), Previous: [Configuring nnmairix](#Configuring-nnmairix), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.6 nnmairix keyboard shortcuts [¶](#nnmairix-keyboard-shortcuts-1)

In group mode:

`G b c` [¶](#index-G-b-c-_0028Group_0029)
:   Creates `nnmairix` server and default search group for this server
    (`nnmairix-create-server-and-default-group`). You should have done
    this by now (see [Configuring nnmairix](#Configuring-nnmairix)).

`G b s` [¶](#index-G-b-s-_0028Group_0029)
:   Prompts for query which is then sent to the mairix binary. Search
    results are put into the default search group which is automatically
    displayed (`nnmairix-search`).

`G b m` [¶](#index-G-b-m-_0028Group_0029)
:   Allows you to create a mairix search or a permanent group more
    comfortably using graphical widgets, similar to a customization
    group. Just try it to see how it works (`nnmairix-widget-search`).

`G b i` [¶](#index-G-b-i-_0028Group_0029)
:   Another command for creating a mairix query more comfortably, but uses
    only the minibuffer (`nnmairix-search-interactive`).

`G b g` [¶](#index-G-b-g-_0028Group_0029)
:   Creates a permanent group which is associated with a search query
    (`nnmairix-create-search-group`). The `nnmairix` back end
    automatically calls mairix when you update this group with `g` or
    `M-g`.

`G b q` [¶](#index-G-b-q-_0028Group_0029)
:   Changes the search query for the `nnmairix` group under cursor
    (`nnmairix-group-change-query-this-group`).

`G b t` [¶](#index-G-b-t-_0028Group_0029)
:   Toggles the ’threads’ parameter for the `nnmairix` group under cursor,
    i.e., if you want see the whole threads of the found messages
    (`nnmairix-group-toggle-threads-this-group`).

`G b u` [¶](#index-G-b-u-_0028Group_0029)
:   Calls mairix binary for updating the database
    (`nnmairix-update-database`). The default parameters are `-F`
    and `-Q` for making this as fast as possible (see variable
    `nnmairix-mairix-update-options` for defining these default
    options).

`G b r` [¶](#index-G-b-r-_0028Group_0029)
:   Keep articles in this `nnmairix` group always read or unread, or leave the
    marks unchanged (`nnmairix-group-toggle-readmarks-this-group`).

`G b d` [¶](#index-G-b-d-_0028Group_0029)
:   Recreate `nnmairix` group on the “real” mail back end
    (`nnmairix-group-delete-recreate-this-group`). You can do this if
    you always get wrong article counts with a `nnmairix` group.

`G b a` [¶](#index-G-b-a-_0028Group_0029)
:   Toggles the `allow-fast` parameters for group under cursor
    (`nnmairix-group-toggle-allowfast-this-group`). The default
    behavior of `nnmairix` is to do a mairix search every time you
    update or enter the group. With the `allow-fast` parameter set,
    mairix will only be called when you explicitly update the group, but not
    upon entering. This makes entering the group faster, but it may also
    lead to dangling symlinks if something changed between updating and
    entering the group which is not yet in the mairix database.

`G b p` [¶](#index-G-b-p-_0028Group_0029)
:   Toggle marks propagation for this group
    (`nnmairix-group-toggle-propmarks-this-group`). (see [Propagating marks](#Propagating-marks)).

`G b o` [¶](#index-G-b-o-_0028Group_0029)
:   Manually propagate marks (`nnmairix-propagate-marks`); needed only when
    `nnmairix-propagate-marks-upon-close` is set to `nil`.

In summary mode:

`G G m` [¶](#index-G-G-m-_0028Summary_0029)
:   Allows you to create a mairix query or group based on the current
    message using graphical widgets (same as `nnmairix-widget-search`)
    (`nnmairix-widget-search-from-this-article`).

`G G g` [¶](#index-G-G-g-_0028Summary_0029)
:   Interactively creates a new search group with query based on the current
    message, but uses the minibuffer instead of graphical widgets
    (`nnmairix-create-search-group-from-message`).

`G G t` [¶](#index-G-G-t-_0028Summary_0029)
:   Searches thread for the current article
    (`nnmairix-search-thread-this-article`). This is effectively a
    shortcut for calling `nnmairix-search` with ‘`m:msgid`’ of the
    current article and enabled threads.

`G G f` [¶](#index-G-G-f-_0028Summary_0029)
:   Searches all messages from sender of the current article
    (`nnmairix-search-from-this-article`). This is a shortcut for
    calling `nnmairix-search` with ‘`f:From`’.

`G G o` [¶](#index-G-G-o-_0028Summary_0029)
:   (Only in `nnmairix` groups!) Tries to determine the group this article
    originally came from and displays the article in this group, so that,
    e.g., replying to this article the correct posting styles/group
    parameters are applied (`nnmairix-goto-original-article`). This
    function will use the registry if available, but can also parse the
    article file name as a fallback method.

`G G u` [¶](#index-G-G-u-_0028Summary_0029)
:   Remove possibly existing tick mark from original article
    (`nnmairix-remove-tick-mark-original-article`). (see [nnmairix tips and tricks](#nnmairix-tips-and-tricks)).

---

Next: [nnmairix tips and tricks](#nnmairix-tips-and-tricks), Previous: [nnmairix keyboard shortcuts](#nnmairix-keyboard-shortcuts), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.7 Propagating marks [¶](#Propagating-marks-1)

First of: you really need a patched mairix binary for using the marks
propagation feature efficiently. Otherwise, you would have to update
the mairix database all the time. You can get the patch at

<http://www.randomsample.de/mairix-maildir-patch.tar>

You need the mairix v0.21 source code for this patch; everything else
is explained in the accompanied readme file. If you don’t want to use
marks propagation, you don’t have to apply these patches, but they also
fix some annoyances regarding changing maildir flags, so it might still
be useful to you.

With the patched mairix binary, you can use `nnmairix` as an
alternative to mail splitting (see [Fancy Mail Splitting](#Fancy-Mail-Splitting)). For
example, instead of splitting all mails from ‘`david@foobar.com`’
into a group, you can simply create a search group with the query
‘`f:david@foobar.com`’. This is actually what “smart folders” are
all about: simply put everything in one mail folder and dynamically
create searches instead of splitting. This is more flexible, since you
can dynamically change your folders any time you want to. This also
implies that you will usually read your mails in the `nnmairix`
groups instead of your “real” mail groups.

There is one problem, though: say you got a new mail from
‘`david@foobar.com`’; it will now show up in two groups, the
“real” group (your INBOX, for example) and in the `nnmairix`
search group (provided you have updated the mairix database). Now you
enter the `nnmairix` group and read the mail. The mail will be
marked as read, but only in the `nnmairix` group—in the “real”
mail group it will be still shown as unread.

You could now catch up the mail group (see [Group Data](#Group-Data)), but this is
tedious and error prone, since you may overlook mails you don’t have
created `nnmairix` groups for. Of course, you could first use
`nnmairix-goto-original-article` (see [nnmairix keyboard shortcuts](#nnmairix-keyboard-shortcuts)) and then read the mail in the original group, but that’s
even more cumbersome.

Clearly, the easiest way would be if marks could somehow be
automatically set for the original article. This is exactly what
*marks propagation* is about.

Marks propagation is inactive by default. You can activate it for a
certain `nnmairix` group with
`nnmairix-group-toggle-propmarks-this-group` (bound to `G b
p`). This function will warn you if you try to use it with your default
search group; the reason is that the default search group is used for
temporary searches, and it’s easy to accidentally propagate marks from
this group. However, you can ignore this warning if you really want to.

With marks propagation enabled, all the marks you set in a `nnmairix`
group should now be propagated to the original article. For example,
you can now tick an article (by default with `!`) and this mark should
magically be set for the original article, too.

A few more remarks which you may or may not want to know:

Marks will not be set immediately, but only upon closing a group. This
not only makes marks propagation faster, it also avoids problems with
dangling symlinks when dealing with maildir files (since changing flags
will change the file name). You can also control when to propagate marks
via `nnmairix-propagate-marks-upon-close` (see the doc-string for
details).

Obviously, `nnmairix` will have to look up the original group for every
article you want to set marks for. If available, `nnmairix` will first
use the registry for determining the original group. The registry is very
fast, hence you should really, really enable the registry when using
marks propagation. If you don’t have to worry about RAM and disc space,
set `gnus-registry-max-entries` to a large enough value; to be on
the safe side, choose roughly the amount of mails you index with mairix.

If you don’t want to use the registry or the registry hasn’t seen the
original article yet, `nnmairix` will use an additional mairix
search for determining the file name of the article. This, of course, is
way slower than the registry—if you set hundreds or even thousands of
marks this way, it might take some time. You can avoid this situation by
setting `nnmairix-only-use-registry` to `t`.

Maybe you also want to propagate marks the other way round, i.e., if you
tick an article in a "real" mail group, you’d like to have the same
article in a `nnmairix` group ticked, too. For several good
reasons, this can only be done efficiently if you use maildir. To
immediately contradict myself, let me mention that it WON’T work with
`nnmaildir`, since `nnmaildir` stores the marks externally and
not in the file name. Therefore, propagating marks to `nnmairix`
groups will usually only work if you use an IMAP server which uses
maildir as its file format.

If you work with this setup, just set
`nnmairix-propagate-marks-to-nnmairix-groups` to `t` and see what
happens. If you don’t like what you see, just set it to `nil` again.
One problem might be that you get a wrong number of unread articles; this
usually happens when you delete or expire articles in the original
groups. When this happens, you can recreate the `nnmairix` group on
the back end using `G b d`.

---

Next: [nnmairix caveats](#nnmairix-caveats), Previous: [Propagating marks](#Propagating-marks), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.8 nnmairix tips and tricks [¶](#nnmairix-tips-and-tricks-1)

* Checking Mail

  I put all my important mail groups at group level 1. The mairix groups
  have group level 5, so they do not get checked at start up (see [Group Levels](#Group-Levels)).

  I use the following to check for mails:

  ```
  (defun my-check-mail-mairix-update (level)
    (interactive "P")
    ;; if no prefix given, set level=1
    (gnus-group-get-new-news (or level 1))
    (nnmairix-update-groups "mairixsearch" t t)
    (gnus-group-list-groups))

  (keymap-set gnus-group-mode-map "g" 'my-check-mail-mairix-update)
  ```

  Instead of ‘`"mairixsearch"`’ use the name of your `nnmairix`
  server. See the doc string for `nnmairix-update-groups` for
  details.
* Example: search group for ticked articles

  For example, you can create a group for all ticked articles, where the
  articles always stay unread:

  Hit `G b g`, enter group name (e.g., ‘`important`’), use
  ‘`F:f`’ as query and do not include threads.

  Now activate marks propagation for this group by using `G b p`. Then
  activate the always-unread feature by using `G b r` twice.

  So far so good—but how do you remove the tick marks in the `nnmairix`
  group? There are two options: You may simply use
  `nnmairix-remove-tick-mark-original-article` (bound to `G G u`) to remove
  tick marks from the original article. The other possibility is to set
  `nnmairix-propagate-marks-to-nnmairix-groups` to `t`, but see the above
  comments about this option. If it works for you, the tick marks should
  also exist in the `nnmairix` group and you can remove them as usual,
  e.g., by marking an article as read.

  When you have removed a tick mark from the original article, this
  article should vanish from the `nnmairix` group after you have updated the
  mairix database and updated the group. Fortunately, there is a function
  for doing exactly that: `nnmairix-update-groups`. See the previous code
  snippet and the doc string for details.
* Dealing with auto-subscription of mail groups

  As described before, all `nnmairix` groups are in fact stored on
  the mail back end in the form ‘`zz_mairix-<NAME>-<NUMBER>`’. You can
  see them when you enter the back end server in the server buffer. You
  should not subscribe these groups! Unfortunately, these groups will
  usually get *auto-subscribed* when you use `nnmaildir` or
  `nnml`, i.e., you will suddenly see groups of the form
  ‘`zz_mairix*`’ pop up in your group buffer. If this happens to you,
  simply kill these groups with C-k. For avoiding this, turn off
  auto-subscription completely by setting the variable
  `gnus-auto-subscribed-groups` to `nil` (see [Filtering New Groups](#Filtering-New-Groups)), or if you like to keep this feature use the following kludge
  for turning it off for all groups beginning with ‘`zz_`’:

  ```
  (setq gnus-auto-subscribed-groups
        "^\\(nnml\\|nnfolder\\|nnmbox\\|nnmh\\|nnbabyl\\|nnmaildir\\).*:\\([^z]\\|z$\\|\\z[^z]\\|zz$\\|zz[^_]\\|zz_$\\).*")
  ```

---

Previous: [nnmairix tips and tricks](#nnmairix-tips-and-tricks), Up: [nnmairix](#nnmairix)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 9.4.9 nnmairix caveats [¶](#nnmairix-caveats-1)

* You can create a secondary `nnml` server just for nnmairix, but then
  you have to explicitly set the corresponding server variable
  `nnml-get-new-mail` to `nil`. Otherwise, new mail might get
  put into this secondary server (and would never show up again). Here’s
  an example server definition:

  ```
  (nnml "mairix" (nnml-directory "mairix") (nnml-get-new-mail nil))
  ```

  (The `nnmaildir` back end also has a server variable
  `get-new-mail`, but its default value is `nil`, so you don’t
  have to explicitly set it if you use a `nnmaildir` server just for
  mairix.)
* If you use the Gnus registry: don’t use the registry with
  `nnmairix` groups (put them in
  `gnus-registry-unfollowed-groups`; this is the default). Be
  *extra careful* if you use
  `gnus-registry-split-fancy-with-parent`; mails which are split
  into `nnmairix` groups are usually gone for good as soon as you
  check the group for new mail (yes, it has happened to me...).
* Therefore: *Never ever* put “real” mails into `nnmairix`
  groups (you shouldn’t be able to, anyway).
* If you use the Gnus agent (see [Gnus Unplugged](#Gnus-Unplugged)): don’t agentize
  `nnmairix` groups (though I have no idea what happens if you do).
* mairix does only support us-ascii characters.
* `nnmairix` uses a rather brute force method to force Gnus to
  completely reread the group on the mail back end after mairix was
  called—it simply deletes and re-creates the group on the mail
  back end. So far, this has worked for me without any problems, and I
  don’t see how `nnmairix` could delete other mail groups than its
  own, but anyway: you really should have a backup of your mail
  folders.
* All necessary information is stored in the group parameters
  (see [Group Parameters](#Group-Parameters)). This has the advantage that no active file
  is needed, but also implies that when you kill a `nnmairix` group,
  it is gone for good.
* If you create and kill a lot of `nnmairix` groups, the
  “zz\_mairix-\*” groups will accumulate on the mail back end server. To
  delete old groups which are no longer needed, call
  `nnmairix-purge-old-groups`. Note that this assumes that you don’t
  save any “real” mail in folders of the form
  `zz_mairix-<NAME>-<NUMBER>`. You can change the prefix of
  `nnmairix` groups by changing the variable
  `nnmairix-group-prefix`.
* The following only applies if you *don’t* use the mentioned patch
  for mairix (see [Propagating marks](#Propagating-marks)):

  A problem can occur when using `nnmairix` with maildir folders and
  comes with the fact that maildir stores mail flags like ‘`Seen`’ or
  ‘`Replied`’ by appending chars ‘`S`’ and ‘`R`’ to the message
  file name, respectively. This implies that currently you would have to
  update the mairix database not only when new mail arrives, but also when
  mail flags are changing. The same applies to new mails which are indexed
  while they are still in the ‘`new`’ folder but then get moved to
  ‘`cur`’ when Gnus has seen the mail. If you don’t update the database
  after this has happened, a mairix query can lead to symlinks pointing to
  non-existing files. In Gnus, these messages will usually appear with
  “(none)” entries in the header and can’t be accessed. If this happens
  to you, using `G b u` and updating the group will usually fix this.

---

Previous: [nnmairix](#nnmairix), Up: [Searching](#Searching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 9.5 Migrating from nnir [¶](#Migrating-from-nnir)

Gnus’ previous search engine was called nnir, and is obsolete as of
Emacs version 28. If you’ve upgraded Emacs and are now getting
obsolete-variable warnings about `nnir-*` variables, migration is
fairly straightforward. In addition to the variables raised by the
warnings, all previous engine-specific variables can be updated by
simply replacing the `nnir-` prefix with `gnus-search-`.
For instance, `nnir-notmuch-program` is now
`gnus-search-notmuch-program`.

---

Next: [The End](#The-End), Previous: [Searching](#Searching), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 10 Various [¶](#Various-1)

* [Process/Prefix](#Process_002fPrefix)
* [Interactive](#Interactive)
* [Symbolic Prefixes](#Symbolic-Prefixes)
* [Formatting Variables](#Formatting-Variables)
* [Window Layout](#Window-Layout)
* [Tabbed Interface](#Tabbed-Interface)
* [Faces and Fonts](#Faces-and-Fonts)
* [Mode Lines](#Mode-Lines)
* [Highlighting and Menus](#Highlighting-and-Menus)
* [Daemons](#Daemons)
* [Notifications](#Notifications)
* [Undo](#Undo)
* [Predicate Specifiers](#Predicate-Specifiers)
* [Moderation](#Moderation)
* [Fetching a Group](#Fetching-a-Group)
* [Image Enhancements](#Image-Enhancements)
* [Fuzzy Matching](#Fuzzy-Matching)
* [Thwarting Email Spam](#Thwarting-Email-Spam)
* [Spam Package](#Spam-Package)
* [The Gnus Registry](#The-Gnus-Registry)
* [The Gnus Cloud](#The-Gnus-Cloud)
* [System Sleep Integration](#System-Sleep-Integration)
* [Interaction with other modes](#Other-modes)
* [Various Various](#Various-Various)

---

Next: [Interactive](#Interactive), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.1 Process/Prefix [¶](#Process_002fPrefix-1)

Many functions, among them functions for moving, decoding and saving
articles, use what is known as the *Process/Prefix convention*.

This is a method for figuring out what articles the user wants the
command to be performed on.

It goes like this:

If the numeric prefix is N, perform the operation on the next N
articles, starting with the current one. If the numeric prefix is
negative, perform the operation on the previous N articles, starting
with the current one.

If `transient-mark-mode` in non-`nil` and the region is
active, all articles in the region will be worked upon.

If there is no numeric prefix, but some articles are marked with the
process mark, perform the operation on the articles marked with
the process mark.

If there is neither a numeric prefix nor any articles marked with the
process mark, just perform the operation on the current article.

Quite simple, really, but it needs to be made clear so that surprises
are avoided.

Commands that react to the process mark will push the current list of
process marked articles onto a stack and will then clear all process
marked articles. You can restore the previous configuration with the
`M P y` command (see [Setting Process Marks](#Setting-Process-Marks)).

One thing that seems to shock & horrify lots of people is that, for
instance, `3 d` does exactly the same as `d` `d` `d`.
Since each `d` (which marks the current article as read) by default
goes to the next unread article after marking, this means that `3 d`
will mark the next three unread articles as read, no matter what the
summary buffer looks like. Set `gnus-summary-goto-unread` to
`nil` for a more straightforward action.

Many commands do not use the process/prefix convention. All commands
that do explicitly say so in this manual. To apply the process/prefix
convention to commands that do not use it, you can use the `M-&`
command. For instance, to mark all the articles in the group as
expirable, you could say `M P b M-& E`.

---

Next: [Symbolic Prefixes](#Symbolic-Prefixes), Previous: [Process/Prefix](#Process_002fPrefix), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.2 Interactive [¶](#Interactive-1)

`gnus-novice-user` [¶](#index-gnus_002dnovice_002duser)
:   If this variable is non-`nil`, you are either a newcomer to the
    World of Usenet, or you are very cautious, which is a nice thing to be,
    really. You will be given questions of the type “Are you sure you want
    to do this?” before doing anything dangerous. This is `t` by
    default.

`gnus-expert-user` [¶](#index-gnus_002dexpert_002duser)
:   If this variable is non-`nil`, you will seldom be asked any
    questions by Gnus. It will simply assume you know what you’re doing,
    no matter how strange. For example, quitting Gnus, exiting a group
    without an update, catching up with a group, deleting expired
    articles, and replying by mail to a news message will not require
    confirmation.

`gnus-interactive-catchup` [¶](#index-gnus_002dinteractive_002dcatchup)
:   Require confirmation before catching up a group if non-`nil`. It
    is `t` by default.

`gnus-interactive-exit` [¶](#index-gnus_002dinteractive_002dexit)
:   If non-`nil`, require a confirmation when exiting Gnus. If
    `quiet`, update any active summary buffers automatically without
    querying. The default value is `t`.

---

Next: [Formatting Variables](#Formatting-Variables), Previous: [Interactive](#Interactive), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
