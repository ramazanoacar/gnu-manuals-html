### 4.1 Summary Buffer Format [¶](#Summary-Buffer-Format-1)

Gnus will use the value of the `gnus-extract-address-components`
variable as a function for getting the name and address parts of a
`From` header. Two pre-defined functions exist:
`gnus-extract-address-components`, which is the default, quite
fast, and too simplistic solution; and
`mail-extract-address-components`, which works very nicely, but is
slower. The default function will return the wrong answer in 5% of the
cases. If this is unacceptable to you, use the other function instead:

```
(setq gnus-extract-address-components
      'mail-extract-address-components)
```

`gnus-summary-same-subject` is a string indicating that the current
article has the same subject as the previous. This string will be used
with those specs that require it. The default is `""`.

* [Summary Buffer Lines](#Summary-Buffer-Lines)
* [To From Newsgroups](#To-From-Newsgroups)
* [Summary Buffer Mode Line](#Summary-Buffer-Mode-Line)
* [Summary Highlighting](#Summary-Highlighting)

---

Next: [To From Newsgroups](#To-From-Newsgroups), Up: [Summary Buffer Format](#Summary-Buffer-Format)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.1.1 Summary Buffer Lines [¶](#Summary-Buffer-Lines-1)

You can change the format of the lines in the summary buffer by changing
the `gnus-summary-line-format` variable. It works along the same
lines as a normal `format` string, with some extensions
(see [Formatting Variables](#Formatting-Variables)).

There should always be a colon or a point position marker on the line;
the cursor always moves to the point position marker or the colon after
performing an operation. (Of course, Gnus wouldn’t be Gnus if it wasn’t
possible to change this. Just write a new function
`gnus-goto-colon` which does whatever you like with the cursor.)
See [Positioning Point](#Positioning-Point).

The default string is ‘`%U%R%z%I%(%[%4L: %-23,23f%]%) %s\n`’.

The following format specification characters and extended format
specification(s) are understood:

‘`N`’
:   Article number.

‘`S`’
:   Subject string. List identifiers stripped,
    `gnus-list-identifiers`. See [Article Hiding](#Article-Hiding).

‘`s`’
:   Subject if the article is the root of the thread or the previous article
    had a different subject, `gnus-summary-same-subject` otherwise.
    (`gnus-summary-same-subject` defaults to `""`.)

‘`F`’
:   Full `From` header.

‘`n`’
:   The name (from the `From` header).

‘`f`’
:   The name, `To` header or the `Newsgroups` header (see [To From Newsgroups](#To-From-Newsgroups)).

‘`a`’
:   The name (from the `From` header). This differs from the `n`
    spec in that it uses the function designated by the
    `gnus-extract-address-components` variable, which is slower, but
    may be more thorough.

‘`A`’
:   The address (from the `From` header). This works the same way as
    the `a` spec.

‘`L`’
:   Number of lines in the article.

‘`Z`’
:   Retrieval Score Value (RSV) of the article; `nil` if not in an
    nnselect group.

‘`G`’
:   Originating group name of the article; `nil` if not in an
    nnselect group.

‘`g`’
:   Short form of the originating group name of the article; `nil` if
    not in an nnselect group.

‘`c`’
:   Number of characters in the article. This specifier is not supported
    in some methods (like nnfolder).

‘`k`’
:   Pretty-printed version of the number of characters in the article;
    for example, ‘`1.2k`’ or ‘`0.4M`’.

‘`I`’
:   Indentation based on thread level (see [Customizing Threading](#Customizing-Threading)).

‘`B`’
:   A complex trn-style thread tree, showing response-connecting trace
    lines. A thread could be drawn like this:

    ```
    >
    +->
    | +->
    | | \->
    | |   \->
    | \->
    +->
    \->
    ```

    You can customize the appearance with the following options. Note
    that it is possible to make the thread display look really neat by
    replacing the default ASCII characters with graphic
    line-drawing glyphs.

    `gnus-sum-thread-tree-root` [¶](#index-gnus_002dsum_002dthread_002dtree_002droot)
    :   Used for the root of a thread. If `nil`, use subject
        instead. The default is ‘`>` ’.

    `gnus-sum-thread-tree-false-root` [¶](#index-gnus_002dsum_002dthread_002dtree_002dfalse_002droot)
    :   Used for the false root of a thread (see [Loose Threads](#Loose-Threads)). If
        `nil`, use subject instead. The default is ‘`>` ’.

    `gnus-sum-thread-tree-single-indent` [¶](#index-gnus_002dsum_002dthread_002dtree_002dsingle_002dindent)
    :   Used for a thread with just one message. If `nil`, use subject
        instead. The default is ‘’.

    `gnus-sum-thread-tree-vertical` [¶](#index-gnus_002dsum_002dthread_002dtree_002dvertical)
    :   Used for drawing a vertical line. The default is ‘`|` ’.

    `gnus-sum-thread-tree-indent` [¶](#index-gnus_002dsum_002dthread_002dtree_002dindent)
    :   Used for indenting. The default is ‘’.

    `gnus-sum-thread-tree-leaf-with-other` [¶](#index-gnus_002dsum_002dthread_002dtree_002dleaf_002dwith_002dother)
    :   Used for a leaf with brothers. The default is ‘`+->` ’.

    `gnus-sum-thread-tree-single-leaf` [¶](#index-gnus_002dsum_002dthread_002dtree_002dsingle_002dleaf)
    :   Used for a leaf without brothers. The default is ‘`\->` ’

‘`T`’
:   Nothing if the article is a root and lots of spaces if it isn’t (it
    pushes everything after it off the screen).

‘`[`’
:   Opening bracket, which is normally ‘`[`’, but can also be ‘`<`’
    for adopted articles (see [Customizing Threading](#Customizing-Threading)). This can be
    customized using following settings:

    `gnus-sum-opening-bracket` [¶](#index-gnus_002dsum_002dopening_002dbracket)
    :   Opening bracket for normal (non-adopted) articles. The default is
        ‘`[`’.

    `gnus-sum-opening-bracket-adopted` [¶](#index-gnus_002dsum_002dopening_002dbracket_002dadopted)
    :   Opening bracket for adopted articles. The default is ‘`<`’.

‘`]`’
:   Closing bracket, which is normally ‘`]`’, but can also be ‘`>`’
    for adopted articles. This can be customized using following settings:

    `gnus-sum-closing-bracket` [¶](#index-gnus_002dsum_002dclosing_002dbracket)
    :   Closing bracket for normal (non-adopted) articles. The default is
        ‘`]`’.

    `gnus-sum-closing-bracket-adopted` [¶](#index-gnus_002dsum_002dopening_002dbracket_002dadopted-1)
    :   Closing bracket for adopted articles. The default is ‘`>`’.

‘`>`’
:   One space for each thread level.

‘`<`’
:   Twenty minus thread level spaces.

‘`U`’
:   Unread. See [Read Articles](#Read-Articles).

‘`R`’
:   This misleadingly named specifier is the *secondary mark*. This
    mark will say whether the article has been replied to, has been cached,
    or has been saved. See [Other Marks](#Other-Marks).

‘`i`’
:   Score as a number (see [Scoring](#Scoring)).

‘`z`’ [¶](#index-gnus_002dsummary_002dzcore_002dfuzz)
:   Zcore, ‘`+`’ if above the default level and ‘`-`’ if below the
    default level. If the difference between
    `gnus-summary-default-score` and the score is less than
    `gnus-summary-zcore-fuzz`, this spec will not be used.

‘`V`’
:   Total thread score.

‘`x`’
:   `Xref`.

‘`D`’
:   `Date`.

‘`d`’
:   The `Date` in `DD-MMM` format.

‘`o`’
:   The `Date` in YYYYMMDD`T`HHMMSS format.

‘`M`’
:   `Message-ID`.

‘`r`’
:   `References`.

‘`t`’
:   Number of articles in the current sub-thread. Using this spec will slow
    down summary buffer generation somewhat.

‘`e`’
:   An ‘`=`’ (`gnus-not-empty-thread-mark`) will be displayed if the
    article has any children.

‘`P`’
:   The line number.

‘`O`’
:   Download mark.

‘`*`’
:   Desired cursor position (instead of after first colon).

‘`&user-date;`’
:   Age sensitive date format. Various date format is defined in
    `gnus-user-date-format-alist`.

‘`u`’
:   User defined specifier. The next character in the format string should
    be a letter. Gnus will call the function
    `gnus-user-format-function-x`, where x is the letter
    following ‘`%u`’. The function will be passed the current header as
    argument. The function should return a string, which will be inserted
    into the summary just like information from any other summary specifier.

Text between ‘`%(`’ and ‘`%)`’ will be highlighted with
`gnus-mouse-face` when the mouse point is placed inside the area.
There can only be one such area.

The ‘`%U`’ (status), ‘`%R`’ (replied) and ‘`%z`’ (zcore) specs
have to be handled with care. For reasons of efficiency, Gnus will
compute what column these characters will end up in, and “hard-code”
that. This means that it is invalid to have these specs after a
variable-length spec. Well, you might not be arrested, but your summary
buffer will look strange, which is bad enough.

The smart choice is to have these specs as far to the left as possible.
(Isn’t that the case with everything, though? But I digress.)

This restriction may disappear in later versions of Gnus.

---

Next: [Summary Buffer Mode Line](#Summary-Buffer-Mode-Line), Previous: [Summary Buffer Lines](#Summary-Buffer-Lines), Up: [Summary Buffer Format](#Summary-Buffer-Format)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.1.2 To From Newsgroups [¶](#To-From-Newsgroups-1)

In some groups (particularly in archive groups), the `From` header
isn’t very interesting, since all the articles there are written by
you. To display the information in the `To` or `Newsgroups`
headers instead, you need to decide three things: What information to
gather; where to display it; and when to display it.

1. The reading of extra header information is controlled by the
   `gnus-extra-headers`. This is a list of header symbols. For
   instance:

   ```
   (setq gnus-extra-headers
         '(To Newsgroups X-Newsreader))
   ```

   This will result in Gnus trying to obtain these three headers, and
   storing it in header structures for later easy retrieval.
2. The value of these extra headers can be accessed via the
   `gnus-extra-header` function. Here’s a format line spec that will
   access the `X-Newsreader` header:

   ```
   "%~(form (gnus-extra-header 'X-Newsreader))@"
   ```
3. The `gnus-ignored-from-addresses` variable says when the
   ‘`%f`’ summary line spec returns the `To`, `Newsreader`
   or `From` header. The variable may be a regexp or a predicate
   function. If this matches the contents of the `From`
   header, the value of the `To` or `Newsreader` headers are
   used instead.

   To distinguish regular articles from those where the `From` field
   has been swapped, a string is prefixed to the `To` or
   `Newsgroups` header in the summary line. By default the string is
   ‘`->` ’ for `To` and ‘`=>` ’ for `Newsgroups`, you can
   customize these strings with `gnus-summary-to-prefix` and
   `gnus-summary-newsgroup-prefix`.

A related variable is `nnmail-extra-headers`, which controls when
to include extra headers when generating overview (NOV) files.
If you have old overview files, you should regenerate them after
changing this variable, by entering the server buffer using `^`,
and then `g` on the appropriate mail server (e.g., nnml) to cause
regeneration.

You also have to instruct Gnus to display the data by changing the
`%n` spec to the `%f` spec in the
`gnus-summary-line-format` variable.

In summary, you’d typically put something like the following in
`~/.gnus.el`:

```
(setq gnus-extra-headers
      '(To Newsgroups))
(setq nnmail-extra-headers gnus-extra-headers)
(setq gnus-summary-line-format
      "%U%R%z%I%(%[%4L: %-23,23f%]%) %s\n")
(setq gnus-ignored-from-addresses
      "Your Name Here")
```

(The values listed above are the default values in Gnus. Alter them
to fit your needs.)

A note for news server administrators, or for users who wish to try to
convince their news server administrator to provide some additional
support:

The above is mostly useful for mail groups, where you have control over
the NOV files that are created. However, if you can persuade your
nntp admin to add (in the usual implementation, notably INN):

```
Newsgroups:full
```

to the end of her `overview.fmt` file, then you can use that just
as you would the extra headers from the mail groups. Otherwise, you
have to disable fetching headers with ‘`XOVER`’:

```
(setq nntp-nov-is-evil t
      gnus-nov-is-evil t)
```

Be aware, though, that this will make entering an NNTP group
much, much slower, so this is not recommended.

One particular scenario in which it can be desirable to not use
‘`XOVER`’ is for `nnvirtual` groups in order to support
limiting by extra headers (e.g., by the newsgroup of its component
groups). Because group parameters are not inherited, a separate
select method for the component groups with the appropriate
`nov-is-evil` set as a method variable is required.

---

Next: [Summary Highlighting](#Summary-Highlighting), Previous: [To From Newsgroups](#To-From-Newsgroups), Up: [Summary Buffer Format](#Summary-Buffer-Format)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.1.3 Summary Buffer Mode Line [¶](#Summary-Buffer-Mode-Line-1)

You can also change the format of the summary mode bar (see [Mode Line Formatting](#Mode-Line-Formatting)). Set `gnus-summary-mode-line-format` to whatever you
like. The default is ‘`Gnus: %%b [%A] %Z`’.

Here are the elements you can play with:

‘`G`’
:   Group name.

‘`p`’
:   Unprefixed group name.

‘`A`’
:   Current article number.

‘`z`’
:   Current article score.

‘`V`’
:   Gnus version.

‘`U`’
:   Number of unread articles in this group.

‘`e`’
:   Number of unread articles in this group that aren’t displayed in the
    summary buffer.

‘`Z`’
:   A string with the number of unread and unselected articles represented
    either as ‘`<%U(+%e) more>`’ if there are both unread and unselected
    articles, and just as ‘`<%U more>`’ if there are just unread articles
    and no unselected ones.

‘`g`’
:   Shortish group name. For instance, ‘`rec.arts.anime`’ will be
    shortened to ‘`r.a.anime`’.

‘`S`’
:   Subject of the current article.

‘`u`’
:   User-defined spec (see [User-Defined Specs](#User_002dDefined-Specs)).

‘`s`’
:   Name of the current score file (see [Scoring](#Scoring)).

‘`d`’
:   Number of dormant articles (see [Unread Articles](#Unread-Articles)).

‘`t`’
:   Number of ticked articles (see [Unread Articles](#Unread-Articles)).

‘`r`’
:   Number of articles that have been marked as read in this session.

‘`E`’
:   Number of articles expunged by the score files.

---

Previous: [Summary Buffer Mode Line](#Summary-Buffer-Mode-Line), Up: [Summary Buffer Format](#Summary-Buffer-Format)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.1.4 Summary Highlighting [¶](#Summary-Highlighting-1)

`gnus-visual-mark-article-hook` [¶](#index-gnus_002dvisual_002dmark_002darticle_002dhook)
:   This hook is run after selecting an article. It is meant to be used for
    highlighting the article in some way. It is not run if
    `gnus-visual` is `nil`.

`gnus-summary-update-hook` [¶](#index-gnus_002dsummary_002dupdate_002dhook)
:   This hook is called when a summary line is changed. It is not run if
    `gnus-visual` is `nil`.

`gnus-summary-selected-face` [¶](#index-gnus_002dsummary_002dselected_002dface)
:   This is the face (or *font* as some people call it) used to
    highlight the current article in the summary buffer.

`gnus-summary-highlight` [¶](#index-gnus_002dsummary_002dhighlight)
:   Summary lines are highlighted according to this variable, which is a
    list where the elements are of the format `(form
    . face)`. If you would, for instance, like ticked articles to be
    italic and high-scored articles to be bold, you could set this variable
    to something like

    ```
    (((eq mark gnus-ticked-mark) . italic)
     ((> score default) . bold))
    ```

    As you may have guessed, if form returns a non-`nil` value,
    face will be applied to the line.

---

Next: [Choosing Articles](#Choosing-Articles), Previous: [Summary Buffer Format](#Summary-Buffer-Format), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.2 Summary Maneuvering [¶](#Summary-Maneuvering-1)

All the straight movement commands understand the numeric prefix and
behave pretty much as you’d expect.

None of these commands select articles.

`G M-n` [¶](#index-M_002dn-_0028Summary_0029)

`M-n`
:   Go to the next summary line of an unread article
    (`gnus-summary-next-unread-subject`).

`G M-p` [¶](#index-M_002dp-_0028Summary_0029)

`M-p`
:   Go to the previous summary line of an unread article
    (`gnus-summary-prev-unread-subject`).

`G g` [¶](#index-G-g-_0028Summary_0029)
:   Ask for an article number and then go to the summary line of that article
    without displaying the article (`gnus-summary-goto-subject`).

If Gnus asks you to press a key to confirm going to the next group, you
can use the `C-n` and `C-p` keys to move around the group
buffer, searching for the next group to read without actually returning
to the group buffer.

Variables related to summary movement:

`gnus-auto-select-next` [¶](#index-gnus_002dauto_002dselect_002dnext)
:   If you issue one of the movement commands (like `n`) and there are
    no more unread articles after the current one, Gnus will offer to go to
    the next group. If this variable is `t` and the next group is
    empty, Gnus will exit summary mode and return to the group buffer. If
    this variable is neither `t` nor `nil`, Gnus will select the
    next group with unread articles. As a special case, if this variable
    is `quietly`, Gnus will select the next group without asking for
    confirmation. If this variable is `almost-quietly`, the same
    will happen only if you are located on the last article in the group.
    Finally, if this variable is `slightly-quietly`, the `Z n`
    command will go to the next group without confirmation. Also
    see [Group Levels](#Group-Levels).

`gnus-auto-select-same` [¶](#index-gnus_002dauto_002dselect_002dsame)
:   If non-`nil`, all the movement commands will try to go to the next
    article with the same subject as the current. (*Same* here might
    mean *roughly equal*. See `gnus-summary-gather-subject-limit`
    for details (see [Customizing Threading](#Customizing-Threading)).) If there are no more
    articles with the same subject, go to the first unread article.

    This variable is not particularly useful if you use a threaded display.

`gnus-paging-select-next` [¶](#index-gnus_002dpaging_002dselect_002dnext)
:   Control whether to select the next/previous article when paging (with
    commands like `SPC` or `DEL`). If non-`nil`, select the
    next article when reaching the end of the article (or the previous
    article when paging backwards).

    If `nil`, don’t do anything at the end/start of the articles.

`gnus-summary-check-current` [¶](#index-gnus_002dsummary_002dcheck_002dcurrent)
:   If non-`nil`, all the “unread” movement commands will not proceed
    to the next (or previous) article if the current article is unread.
    Instead, they will choose the current article.

`gnus-auto-center-summary` [¶](#index-gnus_002dauto_002dcenter_002dsummary)
:   If non-`nil`, Gnus will keep the point in the summary buffer
    centered at all times. This makes things quite tidy, but if you have a
    slow network connection, or simply do not like this un-Emacsism, you can
    set this variable to `nil` to get the normal Emacs scrolling
    action. This will also inhibit horizontal re-centering of the summary
    buffer, which might make it more inconvenient to read extremely long
    threads.

    This variable can also be a number. In that case, center the window at
    the given number of lines from the top.

`gnus-summary-stop-at-end-of-message` [¶](#index-gnus_002dsummary_002dstop_002dat_002dend_002dof_002dmessage)
:   If non-`nil`, don’t go to the next article when hitting
    `SPC`, and you’re at the end of the article.

---

Next: [Scrolling the Article](#Paging-the-Article), Previous: [Summary Maneuvering](#Summary-Maneuvering), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.3 Choosing Articles [¶](#Choosing-Articles-1)

* [Choosing Commands](#Choosing-Commands)
* [Choosing Variables](#Choosing-Variables)

---

Next: [Choosing Variables](#Choosing-Variables), Up: [Choosing Articles](#Choosing-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.3.1 Choosing Commands [¶](#Choosing-Commands-1)

None of the following movement commands understand the numeric prefix,
and they all select and display an article.

If you want to fetch new articles or redisplay the group, see
[Exiting the Summary Buffer](#Exiting-the-Summary-Buffer).

`SPC` [¶](#index-SPC-_0028Summary_0029)
:   Select the current article, or, if that one’s read already, the next
    unread article (`gnus-summary-next-page`).

    If you have an article window open already and you press `SPC`
    again, the article will be scrolled. This lets you conveniently
    `SPC` through an entire newsgroup. See [Scrolling the Article](#Paging-the-Article).

`G n` [¶](#index-n-_0028Summary_0029)

`n`
:   Go to next unread article (`gnus-summary-next-unread-article`).

`G p` [¶](#index-p-_0028Summary_0029)

`p`
:   Go to previous unread article (`gnus-summary-prev-unread-article`).

`G N` [¶](#index-N-_0028Summary_0029)

`N`
:   Go to the next article (`gnus-summary-next-article`).

`G P` [¶](#index-P-_0028Summary_0029)

`P`
:   Go to the previous article (`gnus-summary-prev-article`).

`G u` [¶](#index-_005d-_0028Summary_0029)

`]`
:   Go to the next unseen article (`gnus-summary-next-unseen-article`).

`G U` [¶](#index-_005b-_0028Summary_0029)

`[`
:   Go to the previous unseen article (`gnus-summary-prev-unseen-article`).

`G C-n` [¶](#index-G-C_002dn-_0028Summary_0029)
:   Go to the next article with the same subject
    (`gnus-summary-next-same-subject`).

`G C-p` [¶](#index-G-C_002dp-_0028Summary_0029)
:   Go to the previous article with the same subject
    (`gnus-summary-prev-same-subject`).

`G f` [¶](#index-G-f-_0028Summary_0029)

`.`
:   Go to the first unread article
    (`gnus-summary-first-unread-article`).

`G b` [¶](#index-G-b-_0028Summary_0029)

`,`
:   Go to the unread article with the highest score
    (`gnus-summary-best-unread-article`). If given a prefix argument,
    go to the first unread article that has a score over the default score.

`G l` [¶](#index-l-_0028Summary_0029)

`l`
:   Go to the previous article read (`gnus-summary-goto-last-article`).

`G o` [¶](#index-G-o-_0028Summary_0029)
:   Pop an article off the summary history and go to this article
    (`gnus-summary-pop-article`). This command differs from the
    command above in that you can pop as many previous articles off the
    history as you like, while `l` toggles the two last read articles.
    For a somewhat related issue (if you use these commands a lot),
    see [Article Backlog](#Article-Backlog).

`G j` [¶](#index-j-_0028Summary_0029)

`j`
:   Ask for an article number or `Message-ID`, and then go to that
    article (`gnus-summary-goto-article`).

---

Previous: [Choosing Commands](#Choosing-Commands), Up: [Choosing Articles](#Choosing-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.3.2 Choosing Variables [¶](#Choosing-Variables-1)

Some variables relevant for moving and selecting articles:

`gnus-auto-extend-newsgroup` [¶](#index-gnus_002dauto_002dextend_002dnewsgroup)
:   All the movement commands will try to go to the previous (or next)
    article, even if that article isn’t displayed in the Summary buffer if
    this variable is non-`nil`. Gnus will then fetch the article from
    the server and display it in the article buffer.

`gnus-select-article-hook` [¶](#index-gnus_002dselect_002darticle_002dhook)
:   This hook is called whenever an article is selected. The default is
    `nil`. If you would like each article to be saved in the Agent as
    you read it, putting `gnus-agent-fetch-selected-article` on this
    hook will do so.

`gnus-mark-article-hook` [¶](#index-gnus_002dmark_002darticle_002dhook)
:   This hook is called whenever an article is selected. It is intended to
    be used for marking articles as read. The default value is
    `gnus-summary-mark-read-and-unread-as-read`, and will change the
    mark of almost any article you read to `gnus-read-mark`. The only
    articles not affected by this function are ticked, dormant, and
    expirable articles. If you’d instead like to just have unread articles
    marked as read, you can use `gnus-summary-mark-unread-as-read`
    instead. It will leave marks like `gnus-low-score-mark`,
    `gnus-del-mark` (and so on) alone.

---

Next: [Reply, Followup and Post](#Reply-Followup-and-Post), Previous: [Choosing Articles](#Choosing-Articles), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.4 Scrolling the Article [¶](#Scrolling-the-Article)

`SPC` [¶](#index-SPC-_0028Summary_0029-1)
:   Pressing `SPC` will scroll the current article forward one page,
    or, if you have come to the end of the current article, will choose the
    next article (`gnus-summary-next-page`).

    If `gnus-article-skip-boring` is non-`nil` and the rest of
    the article consists only of citations and signature, then it will be
    skipped; the next article will be shown instead. You can customize
    what is considered uninteresting with
    `gnus-article-boring-faces`. You can manually view the article’s
    pages, no matter how boring, using `C-M-v`.

`DEL` [¶](#index-DEL-_0028Summary_0029)
:   Scroll the current article back one page (`gnus-summary-prev-page`).

`RET` [¶](#index-RET-_0028Summary_0029)
:   Scroll the current article one line forward
    (`gnus-summary-scroll-up`).

`M-RET` [¶](#index-M_002dRET-_0028Summary_0029)
:   Scroll the current article one line backward
    (`gnus-summary-scroll-down`).

`A g` [¶](#index-A-g-_0028Summary_0029)

`g`
:   (Re)fetch the current article (`gnus-summary-show-article`). If
    given a prefix, show a completely “raw” article, just the way it
    came from the server. If given a prefix twice (i.e., `C-u C-u
    g'`), fetch the current article, but don’t run any of the article
    treatment functions.

    If given a numerical prefix, you can do semi-manual charset stuff.
    `C-u 0 g cn-gb-2312 RET` will decode the message as if it were
    encoded in the `cn-gb-2312` charset. If you have

    ```
    (setq gnus-summary-show-article-charset-alist
          '((1 . cn-gb-2312)
            (2 . big5)))
    ```

    then you can say `C-u 1 g` to get the same effect.

`A <` [¶](#index-_003c-_0028Summary_0029)

`<`
:   Scroll to the beginning of the article
    (`gnus-summary-beginning-of-article`).

`A >` [¶](#index-_003e-_0028Summary_0029)

`>`
:   Scroll to the end of the article (`gnus-summary-end-of-article`).

`A s` [¶](#index-A-s-_0028Summary_0029)

`s`
:   Perform an isearch in the article buffer
    (`gnus-summary-isearch-article`).

`h` [¶](#index-h-_0028Summary_0029)
:   Select the article buffer (`gnus-summary-select-article-buffer`).

---

Next: [Delayed Articles](#Delayed-Articles), Previous: [Scrolling the Article](#Paging-the-Article), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.5 Reply, Followup and Post [¶](#Reply_002c-Followup-and-Post)

* [Summary Mail Commands](#Summary-Mail-Commands)
* [Summary Post Commands](#Summary-Post-Commands)
* [Summary Message Commands](#Summary-Message-Commands)
* [Canceling Articles](#Canceling-and-Superseding)

---

Next: [Summary Post Commands](#Summary-Post-Commands), Up: [Reply, Followup and Post](#Reply-Followup-and-Post)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.1 Summary Mail Commands [¶](#Summary-Mail-Commands-1)

Commands for composing a mail message:

`S r` [¶](#index-S-r-_0028Summary_0029)

`r`
:   Mail a reply to the author of the current article
    (`gnus-summary-reply`).

`S R` [¶](#index-R-_0028Summary_0029)

`R`
:   Mail a reply to the author of the current article and include the
    original message (`gnus-summary-reply-with-original`). This
    command uses the process/prefix convention.

`S w` [¶](#index-S-w-_0028Summary_0029)
:   Mail a wide reply to the author of the current article
    (`gnus-summary-wide-reply`). A *wide reply* is a reply that
    goes out to all people listed in the `To`, `From` (or
    `Reply-To`) and `Cc` headers. If `Mail-Followup-To` is
    present, that’s used instead.

`S W` [¶](#index-S-W-_0028Summary_0029)
:   Mail a wide reply to the current article and include the original
    message (`gnus-summary-wide-reply-with-original`). This command uses
    the process/prefix convention, but only uses the headers from the
    first article to determine the recipients.

`S L` [¶](#index-S-L-_0028Summary_0029)
:   When replying to a message from a mailing list, send a reply to that
    message to the mailing list, and include the original message
    (`gnus-summary-reply-to-list-with-original`).

`S v` [¶](#index-S-v-_0028Summary_0029)
:   Mail a very wide reply to the author of the current article
    (`gnus-summary-very-wide-reply`). A *very wide reply* is a
    reply that goes out to all people listed in the `To`, `From`
    (or `Reply-To`) and `Cc` headers in all the process/prefixed
    articles. This command uses the process/prefix convention. If given a
    prefix argument, the body of the current article will also be yanked.

`S V` [¶](#index-S-V-_0028Summary_0029)
:   Mail a very wide reply to the author of the current article and include the
    original message (`gnus-summary-very-wide-reply-with-original`). This
    command uses the process/prefix convention.

`S B r` [¶](#index-S-B-r-_0028Summary_0029)
:   Mail a reply to the author of the current article but ignore the
    `Reply-To` field (`gnus-summary-reply-broken-reply-to`).
    If you need this because a mailing list incorrectly sets a
    `Reply-To` header pointing to the list, you probably want to set
    the `broken-reply-to` group parameter instead, so things will work
    correctly. See [Group Parameters](#Group-Parameters).

`S B R` [¶](#index-S-B-R-_0028Summary_0029)
:   Mail a reply to the author of the current article and include the
    original message but ignore the `Reply-To` field
    (`gnus-summary-reply-broken-reply-to-with-original`).

`S o m` [¶](#index-S-o-m-_0028Summary_0029)

`C-c C-f`
:   Forward the current article to some other person
    (`gnus-summary-mail-forward`). If no prefix is given, the
    message is forwarded according to the value of
    (`message-forward-as-mime`) and
    (`message-forward-show-mml`); if the prefix is 1, decode the
    message and forward directly inline; if the prefix is 2, forward
    message as an rfc822 MIME section; if the prefix is 3,
    decode message and forward as an rfc822 MIME section; if the
    prefix is 4, forward message directly inline; otherwise, the message
    is forwarded as no prefix given but use the negated value of
    (`message-forward-as-mime`). By default, the forwarded message
    is inlined into the mail.

    Which headers from the original message are included in the forwarded
    message is determined by options specific to `message-mode`,
    see [(message)Forwarding](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Forwarding). In addition, this command can be given
    the symbolic prefix ‘`a`’, using `M-i a`, to include most original
    headers.

`S m` [¶](#index-m-_0028Summary_0029)

`m`
:   Prepare a mail (`gnus-summary-mail-other-window`). By default, use
    the posting style of the current group. If given a prefix, disable that.
    If the prefix is 1, prompt for a group name to find the posting style.

`S i` [¶](#index-S-i-_0028Summary_0029)
:   Prepare a news (`gnus-summary-news-other-window`). By default,
    post to the current group. If given a prefix, disable that. If the
    prefix is 1, prompt for a group to post to.

    This function actually prepares a news even when using mail groups.
    This is useful for “posting” messages to mail groups without actually
    sending them over the network: they’re just saved directly to the group
    in question. The corresponding back end must have a request-post method
    for this to work though.

`S D b` [¶](#index-S-D-b-_0028Summary_0029)
:   If you have sent a mail, but the mail was bounced back to you for some
    reason (wrong address, transient failure), you can use this command to
    resend that bounced mail (`gnus-summary-resend-bounced-mail`). You
    will be popped into a mail buffer where you can edit the headers before
    sending the mail off again. If you give a prefix to this command, and
    the bounced mail is a reply to some other mail, Gnus will try to fetch
    that mail and display it for easy perusal of its headers. This might
    very well fail, though.

`S D r` [¶](#index-S-D-r-_0028Summary_0029)
:   Not to be confused with the previous command,
    `gnus-summary-resend-message` will prompt you for an address to
    send the current message off to, and then send it to that place. The
    headers of the message won’t be altered—but lots of headers that say
    `Resent-To`, `Resent-From` and so on will be added. This
    means that you actually send a mail to someone that has a `To`
    header that (probably) points to yourself. This will confuse people.
    So, natcherly you’ll only do that if you’re really eVIl.

    This command is mainly used if you have several accounts and want to
    ship a mail to a different account of yours. (If you’re both
    `root` and `postmaster` and get a mail for `postmaster`
    to the `root` account, you may want to resend it to
    `postmaster`. Ordnung muss sein!

    This command understands the process/prefix convention
    (see [Process/Prefix](#Process_002fPrefix)).

`S D e` [¶](#index-S-D-e-_0028Summary_0029)
:   Like the previous command, but will allow you to edit the message as
    if it were a new message before resending.

`S O m` [¶](#index-S-O-m-_0028Summary_0029)
:   Digest the current series (see [Decoding Articles](#Decoding-Articles)) and forward the
    result using mail (`gnus-uu-digest-mail-forward`). This command
    uses the process/prefix convention (see [Process/Prefix](#Process_002fPrefix)).

`S M-c` [¶](#index-S-M_002dc-_0028Summary_0029)
:   Send a complaint about excessive crossposting to the author of the
    current article (`gnus-summary-mail-crosspost-complaint`).

    This command is provided as a way to fight back against the current
    crossposting pandemic that’s sweeping Usenet. It will compose a reply
    using the `gnus-crosspost-complaint` variable as a preamble. This
    command understands the process/prefix convention
    (see [Process/Prefix](#Process_002fPrefix)) and will prompt you before sending each mail.

Also See [Header Commands](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Header-Commands) in The Message
Manual, for more information.

---

Next: [Summary Message Commands](#Summary-Message-Commands), Previous: [Summary Mail Commands](#Summary-Mail-Commands), Up: [Reply, Followup and Post](#Reply-Followup-and-Post)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.2 Summary Post Commands [¶](#Summary-Post-Commands-1)

Commands for posting a news article:

`S p` [¶](#index-a-_0028Summary_0029)

`a`
:   Prepare for posting an article (`gnus-summary-post-news`). By
    default, post to the current group. If given a prefix, disable that.
    If the prefix is 1, prompt for another group instead.

`S f` [¶](#index-f-_0028Summary_0029)

`f`
:   Post a followup to the current article (`gnus-summary-followup`).

`S F` [¶](#index-S-F-_0028Summary_0029)

`F`
:   Post a followup to the current article and include the original message
    (`gnus-summary-followup-with-original`). This command uses the
    process/prefix convention.

`S n` [¶](#index-S-n-_0028Summary_0029)
:   Post a followup to the current article via news, even if you got the
    message through mail (`gnus-summary-followup-to-mail`).

`S N` [¶](#index-S-N-_0028Summary_0029)
:   Post a followup to the current article via news, even if you got the
    message through mail and include the original message
    (`gnus-summary-followup-to-mail-with-original`). This command uses
    the process/prefix convention.

`S o p` [¶](#index-S-o-p-_0028Summary_0029)
:   Forward the current article to a newsgroup
    (`gnus-summary-post-forward`).
    If no prefix is given, the message is forwarded according to the value
    of (`message-forward-as-mime`) and
    (`message-forward-show-mml`); if the prefix is 1, decode the
    message and forward directly inline; if the prefix is 2, forward message
    as an rfc822 MIME section; if the prefix is 3, decode message and
    forward as an rfc822 MIME section; if the prefix is 4, forward message
    directly inline; otherwise, the message is forwarded as no prefix given
    but use the flipped value of (`message-forward-as-mime`). By
    default, the message is decoded and forwarded as an rfc822 MIME section.

`S O p` [¶](#index-S-O-p-_0028Summary_0029)
:   Digest the current series and forward the result to a newsgroup
    (`gnus-uu-digest-post-forward`). This command uses the
    process/prefix convention.

`S u` [¶](#index-S-u-_0028Summary_0029)
:   Uuencode a file, split it into parts, and post it as a series
    (`gnus-uu-post-news`). (see [Uuencoding and Posting](#Uuencoding-and-Posting)).

Also See [Header Commands](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Header-Commands) in The Message
Manual, for more information.

---

Next: [Canceling Articles](#Canceling-and-Superseding), Previous: [Summary Post Commands](#Summary-Post-Commands), Up: [Reply, Followup and Post](#Reply-Followup-and-Post)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.3 Summary Message Commands [¶](#Summary-Message-Commands-1)

`S y` [¶](#index-S-y-_0028Summary_0029)
:   Yank the current article into an already existing Message composition
    buffer (`gnus-summary-yank-message`). This command prompts for
    what message buffer you want to yank into, and understands the
    process/prefix convention (see [Process/Prefix](#Process_002fPrefix)).

`S A` [¶](#index-S-A-_0028Summary_0029)
:   Attach the current article into an already existing Message
    composition buffer (`gnus-summary-attach-message`). If no such
    buffer exists, a new one is created. This command prompts for what
    message buffer you want to yank into, and understands the
    process/prefix convention (see [Process/Prefix](#Process_002fPrefix)).

---

Previous: [Summary Message Commands](#Summary-Message-Commands), Up: [Reply, Followup and Post](#Reply-Followup-and-Post)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.5.4 Canceling Articles [¶](#Canceling-Articles)

Have you ever written something, and then decided that you really,
really, really wish you hadn’t posted that?

Well, you can’t cancel mail, but you can cancel posts.

Find the article you wish to cancel (you can only cancel your own
articles, so don’t try any funny stuff). Then press `C` or `S
c` (`gnus-summary-cancel-article`). Your article will be
canceled—machines all over the world will be deleting your article.
This command uses the process/prefix convention (see [Process/Prefix](#Process_002fPrefix)).

Be aware, however, that not all sites honor cancels, so your article may
live on here and there, while most sites will delete the article in
question.

Gnus will use the “current” select method when canceling. If you
want to use the standard posting method, use the ‘`a`’ symbolic
prefix (see [Symbolic Prefixes](#Symbolic-Prefixes)).

Gnus ensures that only you can cancel your own messages using a
`Cancel-Lock` header (see [Canceling News](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Canceling-News) in Message Manual).

If you discover that you have made some mistakes and want to do some
corrections, you can post a *superseding* article that will replace
your original article.

Go to the original article and press `S s`
(`gnus-summary-supersede-article`). You will be put in a buffer
where you can edit the article all you want before sending it off the
usual way.

The same goes for superseding as for canceling, only more so: Some
sites do not honor superseding. On those sites, it will appear that you
have posted almost the same article twice.

If you have just posted the article, and change your mind right away,
there is a trick you can use to cancel/supersede the article without
waiting for the article to appear on your site first. You simply return
to the post buffer (which is called `*sent ...*`). There you will
find the article you just posted, with all the headers intact. Change
the `Message-ID` header to a `Cancel` or `Supersedes`
header by substituting one of those words for the word
`Message-ID`. Then just press `C-c C-c` to send the article as
you would do normally. The previous article will be
canceled/superseded.

Just remember, kids: There is no ’c’ in ’supersede’.

---

Next: [Marking Articles](#Marking-Articles), Previous: [Reply, Followup and Post](#Reply-Followup-and-Post), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.6 Delayed Articles [¶](#Delayed-Articles-1)

Sometimes, you might wish to delay the sending of a message. For
example, you might wish to arrange for a message to turn up just in time
to remind your about the birthday of your Significant Other. For this,
there is the `gnus-delay` package. Setup is simple:

```
(gnus-delay-initialize)
```

Normally, to send a message you use the `C-c C-c` command from
Message mode. To delay a message, use `C-c C-j`
(`gnus-delay-article`) instead. This will ask you for how long the
message should be delayed. Possible answers are:

* A time span. Consists of an integer and a letter. For example,
  `42d` means to delay for 42 days. Available letters are `m`
  (minutes), `h` (hours), `d` (days), `w` (weeks), `M`
  (months) and `Y` (years).
* A specific date. Looks like `YYYY-MM-DD`. The message will be
  delayed until that day, at a specific time (eight o’clock by default).
  See also `gnus-delay-default-hour`.
* A specific time of day. Given in `hh:mm` format, 24h, no am/pm
  stuff. The deadline will be at that time today, except if that time has
  already passed, then it’s at the given time tomorrow. So if it’s ten
  o’clock in the morning and you specify `11:15`, then the deadline
  is one hour and fifteen minutes hence. But if you specify `9:20`,
  that means a time tomorrow.

The action of the `gnus-delay-article` command is influenced by a
couple of variables:

`gnus-delay-default-hour` [¶](#index-gnus_002ddelay_002ddefault_002dhour)
:   When you specify a specific date, the message will be due on that hour
    on the given date. Possible values are integers 0 through 23.

`gnus-delay-default-delay` [¶](#index-gnus_002ddelay_002ddefault_002ddelay)
:   This is a string and gives the default delay. It can be of any of the
    formats described above.

`gnus-delay-group` [¶](#index-gnus_002ddelay_002dgroup)
:   Delayed articles will be kept in this group on the drafts server until
    they are due. You probably don’t need to change this. The default
    value is `"delayed"`.

`gnus-delay-header` [¶](#index-gnus_002ddelay_002dheader)
:   The deadline for each article will be stored in a header. This variable
    is a string and gives the header name. You probably don’t need to
    change this. The default value is `"X-Gnus-Delayed"`.

The way delaying works is like this: when you use the
`gnus-delay-article` command, you give a certain delay. Gnus
calculates the deadline of the message and stores it in the
`X-Gnus-Delayed` header and puts the message in the
`nndraft:delayed` group.

And whenever you get new news, Gnus looks through the group for articles
which are due and sends them. It uses the `gnus-delay-send-queue`
function for this. By default, this function is added to the hook
`gnus-get-new-news-hook`. But of course, you can change this.
Maybe you want to use the demon to send drafts? Just tell the demon to
execute the `gnus-delay-send-queue` function.

`gnus-delay-initialize` [¶](#index-gnus_002ddelay_002dinitialize)
:   By default, this function installs `gnus-delay-send-queue` in
    `gnus-get-new-news-hook`. But it accepts the optional second
    argument `no-check`. If it is non-`nil`,
    `gnus-get-new-news-hook` is not changed. The optional first
    argument is ignored.

    For example, `(gnus-delay-initialize nil t)` means to do nothing.
    Presumably, you want to use the demon for sending due delayed articles.
    Just don’t forget to set that up :-)

---

Next: [Limiting](#Limiting), Previous: [Delayed Articles](#Delayed-Articles), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.7 Marking Articles [¶](#Marking-Articles-1)

There are several marks you can set on an article.

You have marks that decide the *readedness* (whoo, neato-keano
neologism ohoy!) of the article. Alphabetic marks generally mean
*read*, while non-alphabetic characters generally mean *unread*.

In addition, you also have marks that do not affect readedness.

* [Unread Articles](#Unread-Articles)
* [Read Articles](#Read-Articles)
* [Other Marks](#Other-Marks)
* [Setting Marks](#Setting-Marks)
* [Generic Marking Commands](#Generic-Marking-Commands)
* [Setting Process Marks](#Setting-Process-Marks)

---

Next: [Read Articles](#Read-Articles), Up: [Marking Articles](#Marking-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.7.1 Unread Articles [¶](#Unread-Articles-1)

The following marks mark articles as (kinda) unread, in one form or
other.

‘`!`’ [¶](#index-gnus_002dticked_002dmark)
:   Marked as ticked (`gnus-ticked-mark`).

    *Ticked articles* are articles that will remain visible always. If
    you see an article that you find interesting, or you want to put off
    reading it, or replying to it, until sometime later, you’d typically
    tick it. However, articles can be expired (from news servers by the
    news server software, Gnus itself never expires ticked messages), so if
    you want to keep an article forever, you’ll have to make it persistent
    (see [Persistent Articles](#Persistent-Articles)).

‘`?`’ [¶](#index-gnus_002ddormant_002dmark)
:   Marked as dormant (`gnus-dormant-mark`).

    *Dormant articles* will only appear in the summary buffer if there
    are followups to it. If you want to see them even if they don’t have
    followups, you can use the `/ D` command (see [Limiting](#Limiting)).
    Otherwise (except for the visibility issue), they are just like ticked
    messages.

‘`SPC`’ [¶](#index-gnus_002dunread_002dmark-1)
:   Marked as unread (`gnus-unread-mark`).

    *Unread articles* are articles that haven’t been read at all yet.

---

Next: [Other Marks](#Other-Marks), Previous: [Unread Articles](#Unread-Articles), Up: [Marking Articles](#Marking-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.7.2 Read Articles [¶](#Read-Articles-1)

All the following marks mark articles as read.

‘`r`’ [¶](#index-gnus_002ddel_002dmark)
:   These are articles that the user has marked as read with the `d`
    command manually, more or less (`gnus-del-mark`).

‘`R`’ [¶](#index-gnus_002dread_002dmark)
:   Articles that have actually been read (`gnus-read-mark`).

‘`O`’ [¶](#index-gnus_002dancient_002dmark)
:   Articles that were marked as read in previous sessions and are now
    *old* (`gnus-ancient-mark`).

‘`K`’ [¶](#index-gnus_002dkilled_002dmark)
:   Marked as killed (`gnus-killed-mark`).

‘`X`’ [¶](#index-gnus_002dkill_002dfile_002dmark)
:   Marked as killed by kill files (`gnus-kill-file-mark`).

‘`Y`’ [¶](#index-gnus_002dlow_002dscore_002dmark)
:   Marked as read by having too low a score (`gnus-low-score-mark`).

‘`C`’ [¶](#index-gnus_002dcatchup_002dmark)
:   Marked as read by a catchup (`gnus-catchup-mark`).

‘`G`’ [¶](#index-gnus_002dcanceled_002dmark)
:   Canceled article (`gnus-canceled-mark`)

‘`Q`’ [¶](#index-gnus_002dsparse_002dmark)
:   Sparsely reffed article (`gnus-sparse-mark`). See [Customizing Threading](#Customizing-Threading).

‘`M`’ [¶](#index-gnus_002dduplicate_002dmark)
:   Article marked as read by duplicate suppression
    (`gnus-duplicate-mark`). See [Duplicate Suppression](#Duplicate-Suppression).

All these marks just mean that the article is marked as read, really.
They are interpreted differently when doing adaptive scoring, though.

One more special mark, though:

‘`E`’ [¶](#index-gnus_002dexpirable_002dmark)
:   Marked as expirable (`gnus-expirable-mark`).

    Marking articles as *expirable* (or have them marked as such
    automatically) doesn’t make much sense in normal groups—a user doesn’t
    control expiring of news articles, but in mail groups, for instance,
    articles marked as *expirable* can be deleted by Gnus at
    any time.

---

Next: [Setting Marks](#Setting-Marks), Previous: [Read Articles](#Read-Articles), Up: [Marking Articles](#Marking-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.7.3 Other Marks [¶](#Other-Marks-1)

There are some marks that have nothing to do with whether the article is
read or not.

* You can set a bookmark in the current article. Say you are reading a
  long thesis on cats’ urinary tracts, and have to go home for dinner
  before you’ve finished reading the thesis. You can then set a bookmark
  in the article, and Gnus will jump to this bookmark the next time it
  encounters the article. See [Setting Marks](#Setting-Marks).
* All articles that you have replied to or made a followup to (i.e., have
  answered) will be marked with an ‘`A`’ in the second column
  (`gnus-replied-mark`).
* All articles that you have forwarded will be marked with an ‘`F`’ in
  the second column (`gnus-forwarded-mark`).
* Articles stored in the article cache will be marked with an ‘`*`’ in
  the second column (`gnus-cached-mark`). See [Article Caching](#Article-Caching).
* Articles “saved” (in some manner or other; not necessarily
  religiously) are marked with an ‘`S`’ in the second column
  (`gnus-saved-mark`).
* Articles that haven’t been seen before in Gnus by the user are marked
  with a ‘`.`’ in the second column (`gnus-unseen-mark`).
* When using the Gnus agent (see [Agent Basics](#Agent-Basics)), articles may be
  downloaded for unplugged (offline) viewing. If you are using the
  ‘`%O`’ spec, these articles get the ‘`+`’ mark in that spec.
  (The variable `gnus-downloaded-mark` controls which character to
  use.)
* When using the Gnus agent (see [Agent Basics](#Agent-Basics)), some articles might
  not have been downloaded. Such articles cannot be viewed while you
  are unplugged (offline). If you are using the ‘`%O`’ spec, these
  articles get the ‘`-`’ mark in that spec. (The variable
  `gnus-undownloaded-mark` controls which character to use.)
* The Gnus agent (see [Agent Basics](#Agent-Basics)) downloads some articles
  automatically, but it is also possible to explicitly mark articles for
  download, even if they would not be downloaded automatically. Such
  explicitly-marked articles get the ‘`%`’ mark in the first column.
  (The variable `gnus-downloadable-mark` controls which character to
  use.)
* If the ‘`%e`’ spec is used, the presence of threads or not will be
  marked with `gnus-not-empty-thread-mark` and
  `gnus-empty-thread-mark` in the third column, respectively.
* Finally we have the *process mark* (`gnus-process-mark`). A
  variety of commands react to the presence of the process mark. For
  instance, `X u` (`gnus-uu-decode-uu`) will uudecode and view
  all articles that have been marked with the process mark. Articles
  marked with the process mark have a ‘`#`’ in the second column.

You might have noticed that most of these “non-readedness” marks
appear in the second column by default. So if you have a cached, saved,
replied article that you have process-marked, what will that look like?

Nothing much. The precedence rules go as follows: process -> cache ->
replied -> saved. So if the article is in the cache and is replied,
you’ll only see the cache mark and not the replied mark.

---

Next: [Generic Marking Commands](#Generic-Marking-Commands), Previous: [Other Marks](#Other-Marks), Up: [Marking Articles](#Marking-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.7.4 Setting Marks [¶](#Setting-Marks-1)

All the marking commands understand the numeric prefix.

`M c` [¶](#index-M-c-_0028Summary_0029)

`M-u`
:   Clear all readedness-marks from the current article
    (`gnus-summary-clear-mark-forward`). In other words, mark the
    article as unread.

`M t` [¶](#index-_0021-_0028Summary_0029)

`!`
:   Tick the current article (`gnus-summary-tick-article-forward`).
    See [Article Caching](#Article-Caching).

`M ?` [¶](#index-_003f-_0028Summary_0029)

`?`
:   Mark the current article as dormant
    (`gnus-summary-mark-as-dormant`). See [Article Caching](#Article-Caching).

`M d` [¶](#index-M-d-_0028Summary_0029)

`d`
:   Mark the current article as read
    (`gnus-summary-mark-as-read-forward`).

`D` [¶](#index-D-_0028Summary_0029)
:   Mark the current article as read and move point to the previous line
    (`gnus-summary-mark-as-read-backward`).

`M k` [¶](#index-k-_0028Summary_0029)

`k`
:   Mark all articles that have the same subject as the current one as read,
    and then select the next unread article
    (`gnus-summary-kill-same-subject-and-select`).

`M K` [¶](#index-M-K-_0028Summary_0029)

`C-k`
:   Mark all articles that have the same subject as the current one as read
    (`gnus-summary-kill-same-subject`).

`M C` [¶](#index-M-C-_0028Summary_0029)
:   Mark all unread articles as read (`gnus-summary-catchup`).

`M C-c` [¶](#index-M-C_002dc-_0028Summary_0029)
:   Mark all articles in the group as read—even the ticked and dormant
    articles (`gnus-summary-catchup-all`).

`M H` [¶](#index-M-H-_0028Summary_0029)
:   Catchup the current group to point (before the point)
    (`gnus-summary-catchup-to-here`).

`M h` [¶](#index-M-h-_0028Summary_0029)
:   Catchup the current group from point (after the point)
    (`gnus-summary-catchup-from-here`).

`C-w` [¶](#index-C_002dw-_0028Summary_0029)
:   Mark all articles between point and mark as read
    (`gnus-summary-mark-region-as-read`).

`M V k` [¶](#index-M-V-k-_0028Summary_0029)
:   Kill all articles with scores below the default score (or below the
    numeric prefix) (`gnus-summary-kill-below`).

`M e` [¶](#index-M-e-_0028Summary_0029)

`E`
:   Mark the current article as expirable
    (`gnus-summary-mark-as-expirable`).

`M b` [¶](#index-M-b-_0028Summary_0029)
:   Set a bookmark in the current article
    (`gnus-summary-set-bookmark`).

`M B` [¶](#index-M-B-_0028Summary_0029)
:   Remove the bookmark from the current article
    (`gnus-summary-remove-bookmark`).

`M V c` [¶](#index-M-V-c-_0028Summary_0029)
:   Clear all marks from articles with scores over the default score (or
    over the numeric prefix) (`gnus-summary-clear-above`).

`M V u` [¶](#index-M-V-u-_0028Summary_0029)
:   Tick all articles with scores over the default score (or over the
    numeric prefix) (`gnus-summary-tick-above`).

`M V m` [¶](#index-M-V-m-_0028Summary_0029)
:   Prompt for a mark, and mark all articles with scores over the default
    score (or over the numeric prefix) with this mark
    (`gnus-summary-clear-above`).

The `gnus-summary-goto-unread` variable controls what action should
be taken after setting a mark. If non-`nil`, point will move to
the next/previous unread article. If `nil`, point will just move
one line up or down. As a special case, if this variable is
`never`, all the marking commands as well as other commands (like
`SPC`) will move to the next article, whether it is unread or not.
The default is `t`.

---

Next: [Setting Process Marks](#Setting-Process-Marks), Previous: [Setting Marks](#Setting-Marks), Up: [Marking Articles](#Marking-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.7.5 Generic Marking Commands [¶](#Generic-Marking-Commands-1)

Some people would like the command that ticks an article (`!`) to
go to the next article. Others would like it to go to the next unread
article. Yet others would like it to stay on the current article.
And even though I haven’t heard of anybody wanting it to go to the
previous (unread) article, I’m sure there are people that want that as
well.

Multiply these five behaviors with five different marking commands, and
you get a potentially complex set of variable to control what each
command should do.

To sidestep that mess, Gnus provides commands that do all these
different things. They can be found on the `M M` map in the summary
buffer. Type `M M C-h` to see them all—there are too many of them
to list in this manual.

While you can use these commands directly, most users would prefer
altering the summary mode keymap. For instance, if you would like the
`!` command to go to the next article instead of the next unread
article, you could say something like:

```
(add-hook 'gnus-summary-mode-hook 'my-alter-summary-map)
(defun my-alter-summary-map ()
  (keymap-local-set "!" 'gnus-summary-put-mark-as-ticked-next))
```

or

```
(defun my-alter-summary-map ()
  (keymap-local-set "!" "MM!n"))
```

---

Previous: [Generic Marking Commands](#Generic-Marking-Commands), Up: [Marking Articles](#Marking-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.7.6 Setting Process Marks [¶](#Setting-Process-Marks-1)

Process marks are displayed as `#` in the summary buffer, and are
used for marking articles in such a way that other commands will
process these articles. For instance, if you process mark four
articles and then use the `*` command, Gnus will enter these four
articles into the cache. For more information,
see [Process/Prefix](#Process_002fPrefix).

`M P p` [¶](#index-_0023-_0028Summary_0029)

`#`
:   Toggle the process mark for the current article
    (`gnus-summary-mark-as-processable`).  
    If `gnus-process-mark-toggle` is `nil`, set the process mark
    for the current article.

`M P u` [¶](#index-M-P-u-_0028Summary_0029)

`M-#`
:   Remove the process mark, if any, from the current article
    (`gnus-summary-unmark-as-processable`).

`M P U` [¶](#index-M-P-U-_0028Summary_0029)
:   Remove the process mark from all articles
    (`gnus-summary-unmark-all-processable`).

`M P i` [¶](#index-M-P-i-_0028Summary_0029)
:   Invert the list of process marked articles
    (`gnus-uu-invert-processable`).

`M P R` [¶](#index-M-P-R-_0028Summary_0029)
:   Mark articles that have a `Subject` header that matches a regular
    expression (`gnus-uu-mark-by-regexp`).

`M P G` [¶](#index-M-P-G-_0028Summary_0029)
:   Unmark articles that have a `Subject` header that matches a regular
    expression (`gnus-uu-unmark-by-regexp`).

`M P r` [¶](#index-M-P-r-_0028Summary_0029)
:   Mark articles in region (`gnus-uu-mark-region`).

`M P g` [¶](#index-M-P-g-_0028Summary_0029)
:   Unmark articles in region (`gnus-uu-unmark-region`).

`M P t` [¶](#index-M-P-t-_0028Summary_0029)
:   Mark all articles in the current (sub)thread
    (`gnus-uu-mark-thread`).

`M P T` [¶](#index-M-P-T-_0028Summary_0029)
:   Unmark all articles in the current (sub)thread
    (`gnus-uu-unmark-thread`).

`M P v` [¶](#index-M-P-v-_0028Summary_0029)
:   Mark all articles that have a score above the prefix argument
    (`gnus-uu-mark-over`).

`M P s` [¶](#index-M-P-s-_0028Summary_0029)
:   Mark all articles in the current series (`gnus-uu-mark-series`).

`M P S` [¶](#index-M-P-S-_0028Summary_0029)
:   Mark all series that have already had some articles marked
    (`gnus-uu-mark-sparse`).

`M P a` [¶](#index-M-P-a-_0028Summary_0029)
:   Mark all articles in series order (`gnus-uu-mark-all`).

`M P b` [¶](#index-M-P-b-_0028Summary_0029)
:   Mark all articles in the buffer in the order they appear
    (`gnus-uu-mark-buffer`).

`M P k` [¶](#index-M-P-k-_0028Summary_0029)
:   Push the current process mark set onto the stack and unmark all articles
    (`gnus-summary-kill-process-mark`).

`M P y` [¶](#index-M-P-y-_0028Summary_0029)
:   Pop the previous process mark set from the stack and restore it
    (`gnus-summary-yank-process-mark`).

`M P w` [¶](#index-M-P-w-_0028Summary_0029)
:   Push the current process mark set onto the stack
    (`gnus-summary-save-process-mark`).

Also see the `&` command in [Searching for Articles](#Searching-for-Articles), for how to
set process marks based on article body contents.

---

Next: [Threading](#Threading), Previous: [Marking Articles](#Marking-Articles), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.8 Limiting [¶](#Limiting-1)

It can be convenient to limit the summary buffer to just show some
subset of the articles currently in the group. The effect most limit
commands have is to remove a few (or many) articles from the summary
buffer.

Limiting commands work on subsets of the articles already fetched from
the servers. These commands don’t query the server for additional
articles.

`/ /` [¶](#index-_002f-_002f-_0028Summary_0029)

`/ s`
:   Limit the summary buffer to articles that match some subject
    (`gnus-summary-limit-to-subject`). If given a prefix, exclude
    matching articles.

`/ a` [¶](#index-_002f-a-_0028Summary_0029)
:   Limit the summary buffer to articles that match some author
    (`gnus-summary-limit-to-author`). If given a prefix, exclude
    matching articles.

`/ R` [¶](#index-_002f-R-_0028Summary_0029)
:   Limit the summary buffer to articles that match some recipient
    (`gnus-summary-limit-to-recipient`). If given a prefix, exclude
    matching articles.

`/ A` [¶](#index-_002f-A-_0028Summary_0029)
:   Limit the summary buffer to articles in which contents of From, To or Cc
    header match a given address (`gnus-summary-limit-to-address`). If
    given a prefix, exclude matching articles.

`/ S` [¶](#index-_002f-S-_0028Summary_0029)
:   Limit the summary buffer to articles that aren’t part of any displayed
    threads (`gnus-summary-limit-to-singletons`). If given a prefix,
    limit to articles that are part of displayed threads.

`/ x` [¶](#index-_002f-x-_0028Summary_0029)
:   Limit the summary buffer to articles that match one of the “extra”
    headers (see [To From Newsgroups](#To-From-Newsgroups))
    (`gnus-summary-limit-to-extra`). If given a prefix, exclude
    matching articles.

`/ u` [¶](#index-_002f-u-_0028Summary_0029)

`x`
:   Limit the summary buffer to articles not marked as read
    (`gnus-summary-limit-to-unread`). If given a prefix, limit the
    buffer to articles strictly unread. This means that ticked and
    dormant articles will also be excluded.

`/ m` [¶](#index-_002f-m-_0028Summary_0029)
:   Ask for a mark and then limit to all articles that have been marked
    with that mark (`gnus-summary-limit-to-marks`).

`/ t` [¶](#index-_002f-t-_0028Summary_0029)
:   Ask for a number and then limit the summary buffer to articles older than (or equal to) that number of days
    (`gnus-summary-limit-to-age`). If given a prefix, limit to
    articles younger than that number of days.

`/ n` [¶](#index-_002f-n-_0028Summary_0029)
:   With prefix ‘`n`’, limit the summary buffer to the next ‘`n`’
    articles. If not given a prefix, use the process marked articles
    instead. (`gnus-summary-limit-to-articles`).

`/ w` [¶](#index-_002f-w-_0028Summary_0029)
:   Pop the previous limit off the stack and restore it
    (`gnus-summary-pop-limit`). If given a prefix, pop all limits off
    the stack.

`/ .` [¶](#index-_002f-_002e-_0028Summary_0029)
:   Limit the summary buffer to the unseen articles
    (`gnus-summary-limit-to-unseen`).

`/ v` [¶](#index-_002f-v-_0028Summary_0029)
:   Limit the summary buffer to articles that have a score at or above some
    score (`gnus-summary-limit-to-score`). If given a prefix, below
    some score.

`/ p` [¶](#index-_002f-p-_0028Summary_0029)
:   Limit the summary buffer to articles that satisfy the `display`
    group parameter predicate
    (`gnus-summary-limit-to-display-predicate`). See [Group Parameters](#Group-Parameters), for more on this predicate.

`/ r` [¶](#index-_002f-r-_0028Summary_0029)
:   Limit the summary buffer to replied articles
    (`gnus-summary-limit-to-replied`). If given a prefix, exclude
    replied articles.

`/ E` [¶](#index-M-S-_0028Summary_0029)

`M S`
:   Include all expunged articles in the limit
    (`gnus-summary-limit-include-expunged`).

`/ D` [¶](#index-_002f-D-_0028Summary_0029)
:   Include all dormant articles in the limit
    (`gnus-summary-limit-include-dormant`).

`/ *` [¶](#index-_002f-_002a-_0028Summary_0029)
:   Include all cached articles in the limit
    (`gnus-summary-limit-include-cached`).

`/ d` [¶](#index-_002f-d-_0028Summary_0029)
:   Exclude all dormant articles from the limit
    (`gnus-summary-limit-exclude-dormant`).

`/ M` [¶](#index-_002f-M-_0028Summary_0029)
:   Exclude all marked articles (`gnus-summary-limit-exclude-marks`).

`/ T` [¶](#index-_002f-T-_0028Summary_0029)
:   Include all the articles in the current thread in the limit.

`/ c` [¶](#index-_002f-c-_0028Summary_0029)
:   Exclude all dormant articles that have no children from the limit  
    (`gnus-summary-limit-exclude-childless-dormant`).

`/ C` [¶](#index-_002f-C-_0028Summary_0029)
:   Mark all excluded unread articles as read
    (`gnus-summary-limit-mark-excluded-as-read`). If given a prefix,
    also mark excluded ticked and dormant articles as read.

`/ b` [¶](#index-_002f-b-_0028Summary_0029)
:   Limit the summary buffer to articles that have bodies that match a
    certain regexp (`gnus-summary-limit-to-bodies`). If given a
    prefix, reverse the limit. This command is quite slow since it
    requires selecting each article to find the matches.

`/ h` [¶](#index-_002f-h-_0028Summary_0029)
:   Like the previous command, only limit to headers instead
    (`gnus-summary-limit-to-headers`).

The following commands aren’t limiting commands, but use the `/`
prefix as well.

`/ N` [¶](#index-_002f-N-_0028Summary_0029)
:   Insert all new articles in the summary buffer. It scans for new emails
    if back-end`-get-new-mail` is non-`nil`.

`/ o` [¶](#index-_002f-o-_0028Summary_0029)
:   Insert all old articles in the summary buffer. If given a numbered
    prefix, fetch this number of articles.

---

Next: [Sorting the Summary Buffer](#Sorting-the-Summary-Buffer), Previous: [Limiting](#Limiting), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
