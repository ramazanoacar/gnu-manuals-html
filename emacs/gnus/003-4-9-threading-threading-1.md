### 4.9 Threading [¶](#Threading-1)

Gnus threads articles by default. *To thread* is to put responses
to articles directly after the articles they respond to—in a
hierarchical fashion.

Threading is done by looking at the `References` headers of the
articles. In a perfect world, this would be enough to build pretty
trees, but unfortunately, the `References` header is often broken
or simply missing. Weird news propagation exacerbates the problem,
so one has to employ other heuristics to get pleasing results. A
plethora of approaches exists, as detailed in horrible detail in
[Customizing Threading](#Customizing-Threading).

First, a quick overview of the concepts:

*root*
:   The top-most article in a thread; the first article in the thread.

*thread*
:   A tree-like article structure.

*sub-thread*
:   A small(er) section of this tree-like structure.

*loose threads*
:   Threads often lose their roots due to article expiry, or due to the root
    already having been read in a previous session, and not displayed in the
    summary buffer. We then typically have many sub-threads that really
    belong to one thread, but are without connecting roots. These are
    called loose threads.

*thread gathering*
:   An attempt to gather loose threads into bigger threads.

*sparse threads*
:   A thread where the missing articles have been “guessed” at, and are
    displayed as empty lines in the summary buffer.

* [Customizing Threading](#Customizing-Threading)
* [Thread Commands](#Thread-Commands)

---

Next: [Thread Commands](#Thread-Commands), Up: [Threading](#Threading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.9.1 Customizing Threading [¶](#Customizing-Threading-1)

* [Loose Threads](#Loose-Threads)
* [Filling In Threads](#Filling-In-Threads)
* [More Threading](#More-Threading)
* [Low-Level Threading](#Low_002dLevel-Threading)

---

Next: [Filling In Threads](#Filling-In-Threads), Up: [Customizing Threading](#Customizing-Threading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.9.1.1 Loose Threads [¶](#Loose-Threads-1)

`gnus-summary-make-false-root` [¶](#index-gnus_002dsummary_002dmake_002dfalse_002droot)
:   If non-`nil`, Gnus will gather all loose subtrees into one big tree
    and create a dummy root at the top. (Wait a minute. Root at the top?
    Yup.) Loose subtrees occur when the real root has expired, or you’ve
    read or killed the root in a previous session.

    When there is no real root of a thread, Gnus will have to fudge
    something. This variable says what fudging method Gnus should use.
    There are four possible values:

    `adopt`
    :   Gnus will make the first of the orphaned articles the parent. This
        parent will adopt all the other articles. The adopted articles will be
        marked as such by pointy brackets (‘`<>`’) instead of the standard
        square brackets (‘`[]`’). This is the default method.

    `dummy` [¶](#index-gnus_002dsummary_002ddummy_002dline_002dformat)
    :   Gnus will create a dummy summary line that will pretend to be the
        parent. This dummy line does not correspond to any real article, so
        selecting it will just select the first real article after the dummy
        article. `gnus-summary-dummy-line-format` is used to specify the
        format of the dummy roots. It accepts only one format spec: ‘`S`’,
        which is the subject of the article. See [Formatting Variables](#Formatting-Variables).
        If you want all threads to have a dummy root, even the non-gathered
        ones, set `gnus-summary-make-false-root-always` to `t`.

    `empty`
    :   Gnus won’t actually make any article the parent, but simply leave the
        subject field of all orphans except the first empty. (Actually, it will
        use `gnus-summary-same-subject` as the subject (see [Summary Buffer Format](#Summary-Buffer-Format)).)

    `none`
    :   Don’t make any article parent at all. Just gather the threads and
        display them after one another.

    `nil`
    :   Don’t gather loose threads.

`gnus-summary-gather-subject-limit` [¶](#index-gnus_002dsummary_002dgather_002dsubject_002dlimit)
:   Loose threads are gathered by comparing subjects of articles. If this
    variable is `nil`, Gnus requires an exact match between the
    subjects of the loose threads before gathering them into one big
    super-thread. This might be too strict a requirement, what with the
    presence of stupid newsreaders that chop off long subject lines. If
    you think so, set this variable to, say, 20 to require that only the
    first 20 characters of the subjects have to match. If you set this
    variable to a really low number, you’ll find that Gnus will gather
    everything in sight into one thread, which isn’t very helpful.

    If you set this variable to the special value `fuzzy`, Gnus will
    use a fuzzy string comparison algorithm on the subjects (see [Fuzzy Matching](#Fuzzy-Matching)).

`gnus-simplify-subject-fuzzy-regexp` [¶](#index-gnus_002dsimplify_002dsubject_002dfuzzy_002dregexp)
:   This can either be a regular expression or list of regular expressions
    that match strings that will be removed from subjects if fuzzy subject
    simplification is used.

`gnus-simplify-ignored-prefixes` [¶](#index-gnus_002dsimplify_002dignored_002dprefixes)
:   If you set `gnus-summary-gather-subject-limit` to something as low
    as 10, you might consider setting this variable to something sensible:

    ```
    (setq gnus-simplify-ignored-prefixes
          (concat
           "\\`\\[?\\("
           (regexp-opt '("looking"
                         "wanted" "followup" "summary" "summary of"
                         "help" "query" "problem" "question"
                         "answer" "reference" "announce"
                         "How can I" "How to" "Comparison of"
                         ;; ...
                         ))
           "\\)\\s *\\("
           (regexp-opt '("for" "for reference" "with" "about"))
           "\\)?\\]?:?[ \t]*"))
    ```

    All words that match this regexp will be removed before comparing two
    subjects.

`gnus-simplify-subject-functions` [¶](#index-gnus_002dsimplify_002dsubject_002dfunctions)
:   If non-`nil`, this variable overrides
    `gnus-summary-gather-subject-limit`. This variable should be a
    list of functions to apply to the `Subject` string iteratively to
    arrive at the simplified version of the string.

    Useful functions to put in this list include:

    `gnus-simplify-subject-re` [¶](#index-gnus_002dsimplify_002dsubject_002dre)
    :   Strip the leading ‘`Re:`’.

    `gnus-simplify-subject-fuzzy` [¶](#index-gnus_002dsimplify_002dsubject_002dfuzzy)
    :   Simplify fuzzily.

    `gnus-simplify-whitespace` [¶](#index-gnus_002dsimplify_002dwhitespace)
    :   Remove excessive whitespace.

    `gnus-simplify-all-whitespace` [¶](#index-gnus_002dsimplify_002dall_002dwhitespace)
    :   Remove all whitespace.

    You may also write your own functions, of course.

`gnus-summary-gather-exclude-subject` [¶](#index-gnus_002dsummary_002dgather_002dexclude_002dsubject)
:   Since loose thread gathering is done on subjects only, that might lead
    to many false hits, especially with certain common subjects like
    ‘’ and ‘`(none)`’. To make the situation slightly better,
    you can use the regexp `gnus-summary-gather-exclude-subject` to say
    what subjects should be excluded from the gathering process.  
    The default is ‘`^ *$\\|^(none)$`’.

`gnus-summary-thread-gathering-function` [¶](#index-gnus_002dsummary_002dthread_002dgathering_002dfunction)
:   Gnus gathers threads by looking at `Subject` headers. This means
    that totally unrelated articles may end up in the same “thread”, which
    is confusing. An alternate approach is to look at all the
    `Message-ID`s in all the `References` headers to find matches.
    This will ensure that no gathered threads ever include unrelated
    articles, but it also means that people who have posted with broken
    newsreaders won’t be gathered properly. The choice is yours—plague or
    cholera:

    `gnus-gather-threads-by-subject` [¶](#index-gnus_002dgather_002dthreads_002dby_002dsubject)
    :   This function is the default gathering function and looks at
        `Subject`s exclusively.

    `gnus-gather-threads-by-references` [¶](#index-gnus_002dgather_002dthreads_002dby_002dreferences)
    :   This function looks at `References` headers exclusively.

    If you want to test gathering by `References`, you could say
    something like:

    ```
    (setq gnus-summary-thread-gathering-function
          'gnus-gather-threads-by-references)
    ```

---

Next: [More Threading](#More-Threading), Previous: [Loose Threads](#Loose-Threads), Up: [Customizing Threading](#Customizing-Threading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.9.1.2 Filling In Threads [¶](#Filling-In-Threads-1)

`gnus-fetch-old-headers` [¶](#index-gnus_002dfetch_002dold_002dheaders)
:   If non-`nil`, Gnus will attempt to build old threads by fetching
    more old headers—headers to articles marked as read. If you would
    like to display as few summary lines as possible, but still connect as
    many loose threads as possible, you should set this variable to
    `some` or a number. If you set it to a number, no more than that
    number of extra old headers will be fetched. In either case, fetching
    old headers only works if the back end you are using carries overview
    files—this would normally be `nntp`, `nnspool`,
    `nnml`, and `nnmaildir`. Also remember that if the root of
    the thread has been expired by the server, there’s not much Gnus can
    do about that.

    This variable can also be set to `invisible`. This won’t have any
    visible effects, but is useful if you use the `A T` command a lot
    (see [Finding the Parent](#Finding-the-Parent)).

    The server has to support NOV for any of this to work.

    This feature can seriously impact performance it ignores all locally
    cached header entries. Setting it to `t` for groups for a server
    that doesn’t expire articles (such as news.gmane.io), leads to very
    slow summary generation.

`gnus-fetch-old-ephemeral-headers` [¶](#index-gnus_002dfetch_002dold_002dephemeral_002dheaders)
:   Same as `gnus-fetch-old-headers`, but only used for ephemeral
    newsgroups.

`gnus-build-sparse-threads` [¶](#index-gnus_002dbuild_002dsparse_002dthreads)
:   Fetching old headers can be slow. A low-rent similar effect can be
    gotten by setting this variable to `some`. Gnus will then look at
    the complete `References` headers of all articles and try to string
    together articles that belong in the same thread. This will leave
    *gaps* in the threading display where Gnus guesses that an article
    is missing from the thread. (These gaps appear like normal summary
    lines. If you select a gap, Gnus will try to fetch the article in
    question.) If this variable is `t`, Gnus will display all these
    “gaps” without regard for whether they are useful for completing the
    thread or not. Finally, if this variable is `more`, Gnus won’t cut
    off sparse leaf nodes that don’t lead anywhere. This variable is
    `nil` by default.

`gnus-read-all-available-headers` [¶](#index-gnus_002dread_002dall_002davailable_002dheaders)
:   This is a rather obscure variable that few will find useful. It’s
    intended for those non-news newsgroups where the back end has to fetch
    quite a lot to present the summary buffer, and where it’s impossible to
    go back to parents of articles. This is mostly the case in the
    web-based groups.

    If you don’t use those, then it’s safe to leave this as the default
    `nil`. If you want to use this variable, it should be a regexp
    that matches the group name, or `t` for all groups.

---

Next: [Low-Level Threading](#Low_002dLevel-Threading), Previous: [Filling In Threads](#Filling-In-Threads), Up: [Customizing Threading](#Customizing-Threading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.9.1.3 More Threading [¶](#More-Threading-1)

`gnus-show-threads` [¶](#index-gnus_002dshow_002dthreads)
:   If this variable is `nil`, no threading will be done, and all of
    the rest of the variables here will have no effect. Turning threading
    off will speed group selection up a bit, but it is sure to make reading
    slower and more awkward.

`gnus-thread-hide-subtree` [¶](#index-gnus_002dthread_002dhide_002dsubtree)
:   If non-`nil`, all threads will be hidden when the summary buffer is
    generated.

    This can also be a predicate specifier (see [Predicate Specifiers](#Predicate-Specifiers)).
    Available predicates are `gnus-article-unread-p` and
    `gnus-article-unseen-p`.

    Here’s an example:

    ```
    (setq gnus-thread-hide-subtree
          '(or gnus-article-unread-p
               gnus-article-unseen-p))
    ```

    (It’s a pretty nonsensical example, since all unseen articles are also
    unread, but you get my drift.)

`gnus-thread-expunge-below` [¶](#index-gnus_002dthread_002dexpunge_002dbelow)
:   All threads that have a total score (as defined by
    `gnus-thread-score-function`) less than this number will be
    expunged. This variable is `nil` by default, which means that no
    threads are expunged.

`gnus-thread-hide-killed` [¶](#index-gnus_002dthread_002dhide_002dkilled)
:   if you kill a thread and this variable is non-`nil`, the subtree
    will be hidden.

`gnus-thread-ignore-subject` [¶](#index-gnus_002dthread_002dignore_002dsubject)
:   Sometimes somebody changes the subject in the middle of a thread. If
    this variable is non-`nil`, which is the default, the subject
    change is ignored. If it is `nil`, a change in the subject will
    result in a new thread.

`gnus-thread-indent-level` [¶](#index-gnus_002dthread_002dindent_002dlevel)
:   This is a number that says how much each sub-thread should be indented.
    The default is 4.

`gnus-sort-gathered-threads-function` [¶](#index-gnus_002dsort_002dgathered_002dthreads_002dfunction)
:   Sometimes, particularly with mailing lists, the order in which mails
    arrive locally is not necessarily the same as the order in which they
    arrived on the mailing list. Consequently, when sorting sub-threads
    using the default `gnus-thread-sort-by-number`, responses can end
    up appearing before the article to which they are responding to.
    Setting this variable to an alternate value
    (e.g., `gnus-thread-sort-by-date`), in a group’s parameters or in an
    appropriate hook (e.g., `gnus-summary-generate-hook`) can produce a
    more logical sub-thread ordering in such instances.

---

Previous: [More Threading](#More-Threading), Up: [Customizing Threading](#Customizing-Threading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.9.1.4 Low-Level Threading [¶](#Low_002dLevel-Threading-1)

`gnus-parse-headers-hook` [¶](#index-gnus_002dparse_002dheaders_002dhook)
:   Hook run before parsing any headers.

`gnus-alter-header-function` [¶](#index-gnus_002dalter_002dheader_002dfunction)
:   If non-`nil`, this function will be called to allow alteration of
    article header structures. The function is called with one parameter,
    the article header vector, which it may alter in any way. For instance,
    if you have a mail-to-news gateway which alters the `Message-ID`s
    in systematic ways (by adding prefixes and such), you can use this
    variable to un-scramble the `Message-ID`s so that they are more
    meaningful. Here’s one example:

    ```
    (setq gnus-alter-header-function 'my-alter-message-id)

    (defun my-alter-message-id (header)
      (let ((id (mail-header-id header)))
        (when (string-match
               "\\(<[^<>@]*\\)\\.?cygnus\\..*@\\([^<>@]*>\\)" id)
          (mail-header-set-id
           (concat (match-string 1 id) "@" (match-string 2 id))
           header))))
    ```

    And another example: the protonmail bridge adds fake message-ids to
    `References` in message headers, which can confuse threading. To
    remove these spurious ids

    ```
    (setq gnus-alter-header-function 'fix-protonmail-references)

    (defun fix-protonmail-references (header)
      (setf (mail-header-references header)
            (mapconcat
             #'(lambda (x) (if (string-search "protonmail.internalid" x) "" x))
             (gnus-split-references (mail-header-references header)) " "))
      header)
    ```

---

Previous: [Customizing Threading](#Customizing-Threading), Up: [Threading](#Threading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.9.2 Thread Commands [¶](#Thread-Commands-1)

`T k` [¶](#index-T-k-_0028Summary_0029)

`C-M-k`
:   Mark all articles in the current (sub-)thread as read
    (`gnus-summary-kill-thread`). If the prefix argument is positive,
    remove all marks instead. If the prefix argument is negative, tick
    articles instead.

`T l` [¶](#index-T-l-_0028Summary_0029)

`C-M-l`
:   Lower the score of the current (sub-)thread
    (`gnus-summary-lower-thread`).

`T i` [¶](#index-T-i-_0028Summary_0029)
:   Increase the score of the current (sub-)thread
    (`gnus-summary-raise-thread`).

`T #` [¶](#index-T-_0023-_0028Summary_0029)
:   Set the process mark on the current (sub-)thread
    (`gnus-uu-mark-thread`).

`T M-#` [¶](#index-T-M_002d_0023-_0028Summary_0029)
:   Remove the process mark from the current (sub-)thread
    (`gnus-uu-unmark-thread`).

`T T` [¶](#index-T-T-_0028Summary_0029)
:   Toggle threading (`gnus-summary-toggle-threads`).

`T s` [¶](#index-T-s-_0028Summary_0029)
:   Expose the (sub-)thread hidden under the current article, if any  
    (`gnus-summary-show-thread`).

`T h` [¶](#index-T-h-_0028Summary_0029)
:   Hide the current (sub-)thread (`gnus-summary-hide-thread`).

`T S` [¶](#index-T-S-_0028Summary_0029)
:   Expose all hidden threads (`gnus-summary-show-all-threads`).

`T H` [¶](#index-T-H-_0028Summary_0029)
:   Hide all threads (`gnus-summary-hide-all-threads`).

`T t` [¶](#index-T-t-_0028Summary_0029)
:   Re-thread the current article’s thread
    (`gnus-summary-rethread-current`). This works even when the
    summary buffer is otherwise unthreaded.

`T ^` [¶](#index-T-_005e-_0028Summary_0029)
:   Make the current article the child of the marked (or previous) article
    (`gnus-summary-reparent-thread`).

`T M-^` [¶](#index-T-M_002d_005e-_0028Summary_0029)
:   Make the current article the parent of the marked articles
    (`gnus-summary-reparent-children`).

The following commands are thread movement commands. They all
understand the numeric prefix.

`T n` [¶](#index-M_002dDOWN-_0028Summary_0029)

`C-M-f`

`M-DOWN`
:   Go to the next thread (`gnus-summary-next-thread`).

`T p` [¶](#index-M_002dUP-_0028Summary_0029)

`C-M-b`

`M-UP`
:   Go to the previous thread (`gnus-summary-prev-thread`).

`T d` [¶](#index-T-d-_0028Summary_0029)
:   Descend the thread (`gnus-summary-down-thread`).

`T u` [¶](#index-T-u-_0028Summary_0029)
:   Ascend the thread (`gnus-summary-up-thread`).

`T o` [¶](#index-T-o-_0028Summary_0029)
:   Go to the top of the thread (`gnus-summary-top-thread`).

If you ignore subject while threading, you’ll naturally end up with
threads that have several different subjects in them. If you then issue
a command like `T k` (`gnus-summary-kill-thread`) you might not
wish to kill the entire thread, but just those parts of the thread that
have the same subject as the current article. If you like this idea,
you can fiddle with `gnus-thread-operation-ignore-subject`. If it
is non-`nil` (which it is by default), subjects will be ignored
when doing thread commands. If this variable is `nil`, articles in
the same thread with different subjects will not be included in the
operation in question. If this variable is `fuzzy`, only articles
that have subjects fuzzily equal will be included (see [Fuzzy Matching](#Fuzzy-Matching)).

---

Next: [Asynchronous Article Fetching](#Asynchronous-Fetching), Previous: [Threading](#Threading), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.10 Sorting the Summary Buffer [¶](#Sorting-the-Summary-Buffer-1)

If you are using a threaded summary display, you can sort the threads by
setting `gnus-thread-sort-functions`, which can be either a single
function, a list of functions, or a list containing functions and
`(not some-function)` elements.

By default, sorting is done on article numbers. Ready-made sorting
predicate functions include `gnus-thread-sort-by-number`,
`gnus-thread-sort-by-author`, `gnus-thread-sort-by-recipient`,
`gnus-thread-sort-by-subject`,
`gnus-thread-sort-by-date`,
`gnus-thread-sort-by-score`,
`gnus-thread-sort-by-most-recent-number`,
`gnus-thread-sort-by-most-recent-date`,
`gnus-thread-sort-by-newsgroups` and
`gnus-thread-sort-by-random` and
`gnus-thread-sort-by-total-score`.

Each function takes two threads and returns non-`nil` if the first
thread should be sorted before the other. Note that sorting really is
normally done by looking only at the roots of each thread. Exceptions
to this rule are `gnus-thread-sort-by-most-recent-number` and
`gnus-thread-sort-by-most-recent-date`.

If you use more than one function, the primary sort key should be the
last function in the list. You should probably always include
`gnus-thread-sort-by-number` in the list of sorting
functions—preferably first. This will ensure that threads that are
equal with respect to the other sort criteria will be displayed in
ascending article order.

If you would like to sort by reverse score, then by subject, and finally
by number, you could do something like:

```
(setq gnus-thread-sort-functions
      '(gnus-thread-sort-by-number
        gnus-thread-sort-by-subject
        (not gnus-thread-sort-by-total-score)))
```

The threads that have highest score will be displayed first in the
summary buffer. When threads have the same score, they will be sorted
alphabetically. The threads that have the same score and the same
subject will be sorted by number, which is (normally) the sequence in
which the articles arrived.

If you want to sort by score and then reverse arrival order, you could
say something like:

```
(setq gnus-thread-sort-functions
      '((not gnus-thread-sort-by-number)
        gnus-thread-sort-by-score))
```

By default, threads including their subthreads are sorted according to
the value of `gnus-thread-sort-functions`. By customizing
`gnus-subthread-sort-functions` you can define a custom sorting
order for subthreads. This allows for example to sort threads from
high score to low score in the summary buffer, but to have subthreads
still sorted chronologically from old to new without taking their
score into account.

The function in the `gnus-thread-score-function` variable (default
`+`) is used for calculating the total score of a thread. Useful
functions might be `max`, `min`, or squared means, or whatever
tickles your fancy.

If you are using an unthreaded display for some strange reason or
other, you have to fiddle with the `gnus-article-sort-functions`
variable. It is very similar to the
`gnus-thread-sort-functions`, except that it uses slightly
different functions for article comparison. Available sorting
predicate functions are `gnus-article-sort-by-number`,
`gnus-article-sort-by-author`,
`gnus-article-sort-by-subject`, `gnus-article-sort-by-date`,
`gnus-article-sort-by-newsgroups`, `gnus-article-sort-by-random`,
and `gnus-article-sort-by-score`.

If you want to sort an unthreaded summary display by subject, you could
say something like:

```
(setq gnus-article-sort-functions
      '(gnus-article-sort-by-number
        gnus-article-sort-by-subject))
```

You can define group specific sorting via `gnus-parameters`,
See [Group Parameters](#Group-Parameters).

---

Next: [Article Caching](#Article-Caching), Previous: [Sorting the Summary Buffer](#Sorting-the-Summary-Buffer), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.11 Asynchronous Article Fetching [¶](#Asynchronous-Article-Fetching)

If you read your news from an NNTP server that’s far away, the
network latencies may make reading articles a chore. You have to wait
for a while after pressing `n` to go to the next article before the
article appears. Why can’t Gnus just go ahead and fetch the article
while you are reading the previous one? Why not, indeed.

First, some caveats. There are some pitfalls to using asynchronous
article fetching, especially the way Gnus does it.

Let’s say you are reading article 1, which is short, and article 2 is
quite long, and you are not interested in reading that. Gnus does not
know this, so it goes ahead and fetches article 2. You decide to read
article 3, but since Gnus is in the process of fetching article 2, the
connection is blocked.

To avoid these situations, Gnus will open two (count ’em two)
connections to the server. Some people may think this isn’t a very nice
thing to do, but I don’t see any real alternatives. Setting up that
extra connection takes some time, so Gnus startup will be slower.

Gnus will fetch more articles than you will read. This will mean that
the link between your machine and the NNTP server will become more
loaded than if you didn’t use article pre-fetch. The server itself will
also become more loaded—both with the extra article requests, and the
extra connection.

Ok, so now you know that you shouldn’t really use this thing… unless
you really want to.

Here’s how: Set `gnus-asynchronous` to `t`. The rest should
happen automatically.

You can control how many articles are to be pre-fetched by setting
`gnus-use-article-prefetch`. This is 30 by default, which means
that when you read an article in the group, the back end will pre-fetch
the next 30 articles. If this variable is `t`, the back end will
pre-fetch all the articles it can without bound. If it is
`nil`, no pre-fetching will be done.

There are probably some articles that you don’t want to pre-fetch—read
articles, for instance. The `gnus-async-prefetch-article-p`
variable controls whether an article is to be pre-fetched. This
function should return non-`nil` when the article in question is
to be pre-fetched. The default is `gnus-async-unread-p`, which
returns `nil` on read articles. The function is called with an
article data structure as the only parameter.

If, for instance, you wish to pre-fetch only unread articles shorter
than 100 lines, you could say something like:

```
(defun my-async-short-unread-p (data)
  "Return non-nil for short, unread articles."
  (and (gnus-data-unread-p data)
       (< (mail-header-lines (gnus-data-header data))
          100)))

(setq gnus-async-prefetch-article-p 'my-async-short-unread-p)
```

These functions will be called many, many times, so they should
preferably be short and sweet to avoid slowing down Gnus too much.
It’s probably a good idea to byte-compile things like this.

After an article has been prefetched, this
`gnus-async-post-fetch-function` will be called. The buffer will
be narrowed to the region of the article that was fetched. A useful
value would be `gnus-html-prefetch-images`, which will prefetch
and store images referenced in the article, so that you don’t have to
wait for them to be fetched when you read the article. This is useful
for HTML messages that have external images.

Articles have to be removed from the asynch buffer sooner or later. The
`gnus-prefetched-article-deletion-strategy` says when to remove
articles. This is a list that may contain the following elements:

`read`
:   Remove articles when they are read.

`exit`
:   Remove articles when exiting the group.

The default value is `(read exit)`.

---

Next: [Persistent Articles](#Persistent-Articles), Previous: [Asynchronous Article Fetching](#Asynchronous-Fetching), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.12 Article Caching [¶](#Article-Caching-1)

If you have an *extremely* slow NNTP connection, you may
consider turning article caching on. Each article will then be stored
locally under your home directory. As you may surmise, this could
potentially use *huge* amounts of disk space, as well as eat up all
your inodes so fast it will make your head swim. In vodka.

Used carefully, though, it could be just an easier way to save articles.

To turn caching on, set `gnus-use-cache` to `t`. By default,
all articles ticked or marked as dormant will then be copied
over to your local cache (`gnus-cache-directory`). Whether this
cache is flat or hierarchical is controlled by the
`gnus-use-long-file-name` variable, as usual.

When re-selecting a ticked or dormant article, it will be fetched from the
cache instead of from the server. As articles in your cache will never
expire, this might serve as a method of saving articles while still
keeping them where they belong. Just mark all articles you want to save
as dormant, and don’t worry.

When an article is marked as read, is it removed from the cache.

The entering/removal of articles from the cache is controlled by the
`gnus-cache-enter-articles` and `gnus-cache-remove-articles`
variables. Both are lists of symbols. The first is `(ticked
dormant)` by default, meaning that ticked and dormant articles will be
put in the cache. The latter is `(read)` by default, meaning that
articles marked as read are removed from the cache. Possibly
symbols in these two lists are `ticked`, `dormant`,
`unread` and `read`.

So where does the massive article-fetching and storing come into the
picture? The `gnus-jog-cache` command will go through all
subscribed newsgroups, request all unread articles, score them, and
store them in the cache. You should only ever, ever ever ever, use this
command if 1) your connection to the NNTP server is really, really,
really slow and 2) you have a really, really, really huge disk.
Seriously. One way to cut down on the number of articles downloaded is
to score unwanted articles down and have them marked as read. They will
not then be downloaded by this command.

It is likely that you do not want caching on all groups. For instance,
if your `nnml` mail is located under your home directory, it makes no
sense to cache it somewhere else under your home directory. Unless you
feel that it’s neat to use twice as much space.

To limit the caching, you could set `gnus-cacheable-groups` to a
regexp of groups to cache, ‘`^nntp`’ for instance, or set the
`gnus-uncacheable-groups` regexp to ‘`^nnml`’, for instance.
Both variables are `nil` by default. If a group matches both
variables, the group is not cached.

The cache stores information on what articles it contains in its active
file (`gnus-cache-active-file`). If this file (or any other parts
of the cache) becomes all messed up for some reason or other, Gnus
offers two functions that will try to set things right. `M-x
gnus-cache-generate-nov-databases` will (re)build all the NOV
files, and `gnus-cache-generate-active` will (re)generate the active
file.

`gnus-cache-move-cache` will move your whole
`gnus-cache-directory` to some other location. You get asked to
where, isn’t that cool?

---

Next: [Sticky Articles](#Sticky-Articles), Previous: [Article Caching](#Article-Caching), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.13 Persistent Articles [¶](#Persistent-Articles-1)

Closely related to article caching, we have *persistent articles*.
In fact, it’s just a different way of looking at caching, and much more
useful in my opinion.

Say you’re reading a newsgroup, and you happen on to some valuable gem
that you want to keep and treasure forever. You’d normally just save it
(using one of the many saving commands) in some file. The problem with
that is that it’s just, well, yucky. Ideally you’d prefer just having
the article remain in the group where you found it forever; untouched by
the expiry going on at the news server.

This is what a *persistent article* is—an article that just won’t
be deleted. It’s implemented using the normal cache functions, but
you use two explicit commands for managing persistent articles:

`*` [¶](#index-_002a-_0028Summary_0029)
:   Make the current article persistent (`gnus-cache-enter-article`).

`M-*` [¶](#index-M_002d_002a-_0028Summary_0029)
:   Remove the current article from the persistent articles
    (`gnus-cache-remove-article`). This will normally delete the
    article.

Both these commands understand the process/prefix convention.

To avoid having all ticked articles (and stuff) entered into the cache,
you should set `gnus-use-cache` to `passive` if you’re just
interested in persistent articles:

```
(setq gnus-use-cache 'passive)
```

---

Next: [Article Backlog](#Article-Backlog), Previous: [Persistent Articles](#Persistent-Articles), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.14 Sticky Articles [¶](#Sticky-Articles-1)

When you select an article the current article buffer will be reused
according to the value of the variable
`gnus-single-article-buffer`. If its value is non-`nil` (the
default) all articles reuse the same article buffer. Else each group
has its own article buffer.

This implies that it’s not possible to have more than one article buffer
in a group at a time. But sometimes you might want to display all the
latest emails from your mother, your father, your aunt, your uncle and
your 17 cousins to coordinate the next Christmas party.

That’s where sticky articles come in handy. A sticky article buffer
basically is a normal article buffer, but it won’t be reused when you
select another article. You can make an article sticky with:

`A S` [¶](#index-A-S-_0028Summary_0029)
:   Make the current article sticky. If a prefix arg is given, ask for a
    name for this sticky article buffer.

To close a sticky article buffer you can use these commands:

`q` [¶](#index-q-_0028Article_0029)
:   Puts this sticky article buffer at the end of the list of all buffers.

`k` [¶](#index-k-_0028Article_0029)
:   Kills this sticky article buffer.

To kill all sticky article buffers you can use:

Function: **gnus-kill-sticky-article-buffers** ARG [¶](#index-gnus_002dkill_002dsticky_002darticle_002dbuffers)
:   Kill all sticky article buffers.
    If a prefix ARG is given, ask for confirmation.

---

Next: [Saving Articles](#Saving-Articles), Previous: [Sticky Articles](#Sticky-Articles), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.15 Article Backlog [¶](#Article-Backlog-1)

If you have a slow connection, but the idea of using caching seems
unappealing to you (and it is, really), you can help the situation some
by switching on the *backlog*. This is where Gnus will buffer
already read articles so that it doesn’t have to re-fetch articles
you’ve already read. This only helps if you are in the habit of
re-selecting articles you’ve recently read, of course. If you never do
that, turning the backlog on will slow Gnus down a little bit, and
increase memory usage some.

If you set `gnus-keep-backlog` to a number n, Gnus will store
at most n old articles in a buffer for later re-fetching. If this
variable is non-`nil` and is not a number, Gnus will store
*all* read articles, which means that your Emacs will grow without
bound before exploding and taking your machine down with you. I put
that in there just to keep y’all on your toes.

The default value is 20.

---

Next: [Decoding Articles](#Decoding-Articles), Previous: [Article Backlog](#Article-Backlog), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.16 Saving Articles [¶](#Saving-Articles-1)

Gnus can save articles in a number of ways. Below is the documentation
for saving articles in a fairly straight-forward fashion (i.e., little
processing of the article is done before it is saved). For a different
approach (uudecoding, unsharing) you should use `gnus-uu`
(see [Decoding Articles](#Decoding-Articles)).

For the commands listed here, the target is a file.
A directory name (ending in ‘`/`’) causes the target
to be a file under that directory. If you want to
save to a group, see the `B c` (`gnus-summary-copy-article`)
command (see [Mail Group Commands](#Mail-Group-Commands)).

If `gnus-save-all-headers` is non-`nil`, Gnus will not delete
unwanted headers before saving the article.

If the preceding variable is `nil`, all headers that match the
`gnus-saved-headers` regexp will be kept, while the rest will be
deleted before saving.

`O o` [¶](#index-O-o-_0028Summary_0029)

`o`
:   Save the current article using the default article saver
    (`gnus-summary-save-article`).

`O m` [¶](#index-O-m-_0028Summary_0029)
:   Save the current article in a Unix mail box (mbox) file
    (`gnus-summary-save-article-mail`).

`O r` [¶](#index-O-r-_0028Summary_0029)
:   Save the current article in Rmail format
    (`gnus-summary-save-article-rmail`). This is mbox since Emacs 23,
    Babyl in older versions.

`O f` [¶](#index-O-f-_0028Summary_0029)
:   Save the current article in plain file format
    (`gnus-summary-save-article-file`).

`O F` [¶](#index-O-F-_0028Summary_0029)
:   Write the current article in plain file format, overwriting any previous
    file contents (`gnus-summary-write-article-file`).

`O b` [¶](#index-O-b-_0028Summary_0029)
:   Save the current article body in plain file format
    (`gnus-summary-save-article-body-file`).

`O h` [¶](#index-O-h-_0028Summary_0029)
:   Save the current article in mh folder format
    (`gnus-summary-save-article-folder`).

`O v` [¶](#index-O-v-_0028Summary_0029)
:   Save the current article in a VM folder
    (`gnus-summary-save-article-vm`).

`O p` [¶](#index-O-p-_0028Summary_0029)

`|`
:   Save the current article in a pipe. Uhm, like, what I mean is—Pipe
    the current article to a process (`gnus-summary-pipe-output`).
    If given a symbolic prefix (see [Symbolic Prefixes](#Symbolic-Prefixes)), include the
    complete headers in the piped output. The symbolic prefix `r` is
    special; it lets this command pipe a raw article including all headers.
    The `gnus-summary-pipe-output-default-command` variable can be set
    to a string containing the default command and options (default
    `nil`).

`O P` [¶](#index-O-P-_0028Summary_0029)
:   Save the current article into muttprint. That is, print it using the
    external program [Muttprint](https://muttprint.sourceforge.net/). The program name and options to use is controlled by the
    variable `gnus-summary-muttprint-program`.
    (`gnus-summary-muttprint`).

All these commands use the process/prefix convention
(see [Process/Prefix](#Process_002fPrefix)). If you save bunches of articles using these
functions, you might get tired of being prompted for files to save each
and every article in. The prompting action is controlled by
the `gnus-prompt-before-saving` variable, which is `always` by
default, giving you that excessive prompting action you know and
loathe. If you set this variable to `t` instead, you’ll be prompted
just once for each series of articles you save. If you like to really
have Gnus do all your thinking for you, you can even set this variable
to `nil`, which means that you will never be prompted for files to
save articles in. Gnus will simply save all the articles in the default
files.

You can customize the `gnus-default-article-saver` variable to make
Gnus do what you want it to. You can use any of the eight ready-made
functions below, or you can create your own.

`gnus-summary-save-in-rmail` [¶](#index-gnus_002dsummary_002dsave_002din_002drmail)
:   This is the default format, that used by the Rmail package. Since Emacs
    23, Rmail uses standard mbox format. Before this, it used the
    *Babyl* format. Accordingly, this command writes mbox format since
    Emacs 23, unless appending to an existing Babyl file. In older versions
    of Emacs, it always uses Babyl format. Uses the function in the
    `gnus-rmail-save-name` variable to get a file name to save the
    article in. The default is `gnus-plain-save-name`.

`gnus-summary-save-in-mail` [¶](#index-gnus_002dsummary_002dsave_002din_002dmail)
:   Save in a Unix mail (mbox) file. Uses the function in the
    `gnus-mail-save-name` variable to get a file name to save the
    article in. The default is `gnus-plain-save-name`.

`gnus-summary-save-in-file` [¶](#index-gnus_002dsummary_002dsave_002din_002dfile)
:   Append the article straight to an ordinary file. Uses the function in
    the `gnus-file-save-name` variable to get a file name to save the
    article in. The default is `gnus-numeric-save-name`.

`gnus-summary-write-to-file` [¶](#index-gnus_002dsummary_002dwrite_002dto_002dfile)
:   Write the article straight to an ordinary file. The file is
    overwritten if it exists. Uses the function in the
    `gnus-file-save-name` variable to get a file name to save the
    article in. The default is `gnus-numeric-save-name`.

`gnus-summary-save-body-in-file` [¶](#index-gnus_002dsummary_002dsave_002dbody_002din_002dfile)
:   Append the article body to an ordinary file. Uses the function in the
    `gnus-file-save-name` variable to get a file name to save the
    article in. The default is `gnus-numeric-save-name`.

`gnus-summary-write-body-to-file` [¶](#index-gnus_002dsummary_002dwrite_002dbody_002dto_002dfile)
:   Write the article body straight to an ordinary file. The file is
    overwritten if it exists. Uses the function in the
    `gnus-file-save-name` variable to get a file name to save the
    article in. The default is `gnus-numeric-save-name`.

`gnus-summary-save-in-folder` [¶](#index-gnus_002dsummary_002dsave_002din_002dfolder)
:   Save the article to an MH folder using `rcvstore` from the MH
    library. Uses the function in the `gnus-folder-save-name` variable
    to get a file name to save the article in. The default is
    `gnus-folder-save-name`, but you can also use
    `gnus-Folder-save-name`, which creates capitalized names.

`gnus-summary-save-in-vm` [¶](#index-gnus_002dsummary_002dsave_002din_002dvm)
:   Save the article in a VM folder. You have to have the VM mail
    reader to use this setting.

`gnus-summary-save-in-pipe` [¶](#index-gnus_002dsummary_002dsave_002din_002dpipe)
:   Pipe the article to a shell command. This function takes optional two
    arguments COMMAND and RAW. Valid values for COMMAND include:

    * a string  
      The executable command name and possibly arguments.
    * `nil`  
      You will be prompted for the command in the minibuffer.
    * the symbol `default`  
      It will be replaced with the command which the variable
      `gnus-summary-pipe-output-default-command` holds or the command
      last used for saving.

    Non-`nil` value for RAW overrides `:decode` and
    `:headers` properties (see below) and the raw article including all
    headers will be piped.

The symbol of each function may have the following properties:

`:decode`
:   The value non-`nil` means save decoded articles. This is
    meaningful only with `gnus-summary-save-in-file`,
    `gnus-summary-save-body-in-file`,
    `gnus-summary-write-to-file`,
    `gnus-summary-write-body-to-file`, and
    `gnus-summary-save-in-pipe`.

`:function`
:   The value specifies an alternative function which appends, not
    overwrites, articles to a file. This implies that when saving many
    articles at a time, `gnus-prompt-before-saving` is bound to
    `t` and all articles are saved in a single file. This is
    meaningful only with `gnus-summary-write-to-file` and
    `gnus-summary-write-body-to-file`.

`:headers`
:   The value specifies the symbol of a variable of which the value
    specifies headers to be saved. If it is omitted,
    `gnus-save-all-headers` and `gnus-saved-headers` control what
    headers should be saved.

All of these functions, except for the last one, will save the article
in the `gnus-article-save-directory`, which is initialized from the
`SAVEDIR` environment variable. This is `~/News/` by
default.

As you can see above, the functions use different functions to find a
suitable name of a file to save the article in. Below is a list of
available functions that generate names:

`gnus-Numeric-save-name` [¶](#index-gnus_002dNumeric_002dsave_002dname)
:   File names like `~/News/Alt.andrea-dworkin/45`.

`gnus-numeric-save-name` [¶](#index-gnus_002dnumeric_002dsave_002dname-1)
:   File names like `~/News/alt.andrea-dworkin/45`.

`gnus-Plain-save-name` [¶](#index-gnus_002dPlain_002dsave_002dname)
:   File names like `~/News/Alt.andrea-dworkin`.

`gnus-plain-save-name` [¶](#index-gnus_002dplain_002dsave_002dname-1)
:   File names like `~/News/alt.andrea-dworkin`.

`gnus-sender-save-name` [¶](#index-gnus_002dsender_002dsave_002dname)
:   File names like `~/News/larsi`.

You can have Gnus suggest where to save articles by plonking a regexp into
the `gnus-split-methods` alist. For instance, if you would like to
save articles related to Gnus in the file `gnus-stuff`, and articles
related to VM in `vm-stuff`, you could set this variable to something
like:

```
(("^Subject:.*gnus\\|^Newsgroups:.*gnus" "gnus-stuff")
 ("^Subject:.*vm\\|^Xref:.*vm" "vm-stuff")
 (my-choosing-function "../other-dir/my-stuff")
 ((equal gnus-newsgroup-name "mail.misc") "mail-stuff"))
```

We see that this is a list where each element is a list that has two
elements—the *match* and the *file*. The match can either be
a string (in which case it is used as a regexp to match on the article
head); it can be a symbol (which will be called as a function with the
group name as a parameter); or it can be a list (which will be
`eval`ed). If any of these actions have a non-`nil` result,
the *file* will be used as a default prompt. In addition, the
result of the operation itself will be used if the function or form
called returns a string or a list of strings.

You basically end up with a list of file names that might be used when
saving the current article. (All “matches” will be used.) You will
then be prompted for what you really want to use as a name, with file
name completion over the results from applying this variable.

This variable is `((gnus-article-archive-name))` by default, which
means that Gnus will look at the articles it saves for an
`Archive-name` line and use that as a suggestion for the file
name.

Here’s an example function to clean up file names somewhat. If you have
lots of mail groups called things like
‘`nnml:mail.whatever`’, you may want to chop off the beginning of
these group names before creating the file name to save to. The
following will do just that:

```
(defun my-save-name (group)
  (when (string-match "^nnml:mail." group)
    (substring group (match-end 0))))

(setq gnus-split-methods
      '((gnus-article-archive-name)
        (my-save-name)))
```

Finally, you have the `gnus-use-long-file-name` variable. If it is
`nil`, all the preceding functions will replace all periods
(‘`.`’) in the group names with slashes (‘`/`’)—which means that
the functions will generate hierarchies of directories instead of having
all the files in the top level directory
(`~/News/alt/andrea-dworkin` instead of
`~/News/alt.andrea-dworkin`.) This variable is `t` by default
on most systems. However, for historical reasons, this is `nil` on
Xenix and usg-unix-v machines by default.

This function also affects kill and score file names. If this variable
is a list, and the list contains the element `not-score`, long file
names will not be used for score files, if it contains the element
`not-save`, long file names will not be used for saving, and if it
contains the element `not-kill`, long file names will not be used
for kill files.

If you’d like to save articles in a hierarchy that looks something like
a spool, you could

```
(setq gnus-use-long-file-name '(not-save)) ; to get a hierarchy
(setq gnus-default-article-saver
      'gnus-summary-save-in-file)          ; no encoding
```

Then just save with `o`. You’d then read this hierarchy with
ephemeral `nneething` groups—`G D` in the group buffer, and
the top level directory as the argument (`~/News/`). Then just walk
around to the groups/directories with `nneething`.

---

Next: [Article Treatment](#Article-Treatment), Previous: [Saving Articles](#Saving-Articles), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.17 Decoding Articles [¶](#Decoding-Articles-1)

Sometime users post articles (or series of articles) that have been
encoded in some way or other. Gnus can decode them for you.

All these functions use the process/prefix convention
(see [Process/Prefix](#Process_002fPrefix)) for finding out what articles to work on, with
the extension that a “single article” means “a single series”. Gnus
can find out by itself what articles belong to a series, decode all the
articles and unpack/view/save the resulting file(s).

Gnus guesses what articles are in the series according to the following
simplish rule: The subjects must be (nearly) identical, except for the
last two numbers of the line. (Spaces are largely ignored, however.)

For example: If you choose a subject called ‘`cat.gif (2/3)`’, Gnus
will find all the articles that match the regexp ‘`^cat.gif
([0-9]+/[0-9]+).*$`’.

Subjects that are non-standard, like ‘`cat.gif (2/3) Part 6 of a
series`’, will not be properly recognized by any of the automatic viewing
commands, and you have to mark the articles manually with `#`.

* [Uuencoded Articles](#Uuencoded-Articles)
* [Shell Archives](#Shell-Archives)
* [PostScript Files](#PostScript-Files)
* [Other Files](#Other-Files)
* [Decoding Variables](#Decoding-Variables)
* [Viewing Files](#Viewing-Files)

---

Next: [Shell Archives](#Shell-Archives), Up: [Decoding Articles](#Decoding-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.1 Uuencoded Articles [¶](#Uuencoded-Articles-1)

`X u` [¶](#index-X-u-_0028Summary_0029)
:   Uudecodes the current series (`gnus-uu-decode-uu`).

`X U` [¶](#index-X-U-_0028Summary_0029)
:   Uudecodes and saves the current series
    (`gnus-uu-decode-uu-and-save`).

`X v u` [¶](#index-X-v-u-_0028Summary_0029)
:   Uudecodes and views the current series (`gnus-uu-decode-uu-view`).

`X v U` [¶](#index-X-v-U-_0028Summary_0029)
:   Uudecodes, views and saves the current series
    (`gnus-uu-decode-uu-and-save-view`).

Remember that these all react to the presence of articles marked with
the process mark. If, for instance, you’d like to decode and save an
entire newsgroup, you’d typically do `M P a`
(`gnus-uu-mark-all`) and then `X U`
(`gnus-uu-decode-uu-and-save`).

All this is very much different from how `gnus-uu` worked with
GNUS 4.1, where you had explicit keystrokes for everything under
the sun. This version of `gnus-uu` generally assumes that you mark
articles in some way (see [Setting Process Marks](#Setting-Process-Marks)) and then press
`X u`.

Note: When trying to decode articles that have names matching
`gnus-uu-notify-files`, which is hard-coded to
‘`[Cc][Ii][Nn][Dd][Yy][0-9]+.\\(gif\\|jpg\\)`’, `gnus-uu` will
automatically post an article on ‘`comp.unix.wizards`’ saying that
you have just viewed the file in question. This feature can’t be turned
off.

---

Next: [PostScript Files](#PostScript-Files), Previous: [Uuencoded Articles](#Uuencoded-Articles), Up: [Decoding Articles](#Decoding-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.2 Shell Archives [¶](#Shell-Archives-1)

Shell archives (“shar files”) used to be a popular way to distribute
sources, but it isn’t used all that much today. In any case, we have
some commands to deal with these:

`X s` [¶](#index-X-s-_0028Summary_0029)
:   Unshars the current series (`gnus-uu-decode-unshar`).

`X S` [¶](#index-X-S-_0028Summary_0029)
:   Unshars and saves the current series (`gnus-uu-decode-unshar-and-save`).

`X v s` [¶](#index-X-v-s-_0028Summary_0029)
:   Unshars and views the current series (`gnus-uu-decode-unshar-view`).

`X v S` [¶](#index-X-v-S-_0028Summary_0029)
:   Unshars, views and saves the current series
    (`gnus-uu-decode-unshar-and-save-view`).

---

Next: [Other Files](#Other-Files), Previous: [Shell Archives](#Shell-Archives), Up: [Decoding Articles](#Decoding-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.3 PostScript Files [¶](#PostScript-Files-1)

`X p` [¶](#index-X-p-_0028Summary_0029)
:   Unpack the current PostScript series (`gnus-uu-decode-postscript`).

`X P` [¶](#index-X-P-_0028Summary_0029)
:   Unpack and save the current PostScript series
    (`gnus-uu-decode-postscript-and-save`).

`X v p` [¶](#index-X-v-p-_0028Summary_0029)
:   View the current PostScript series
    (`gnus-uu-decode-postscript-view`).

`X v P` [¶](#index-X-v-P-_0028Summary_0029)
:   View and save the current PostScript series
    (`gnus-uu-decode-postscript-and-save-view`).

---

Next: [Decoding Variables](#Decoding-Variables), Previous: [PostScript Files](#PostScript-Files), Up: [Decoding Articles](#Decoding-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.4 Other Files [¶](#Other-Files-1)

`X o` [¶](#index-X-o-_0028Summary_0029)
:   Save the current series
    (`gnus-uu-decode-save`).

`X b` [¶](#index-X-b-_0028Summary_0029)
:   Unbinhex the current series (`gnus-uu-decode-binhex`). This
    doesn’t really work yet.

`X Y` [¶](#index-X-Y-_0028Summary_0029)
:   yEnc-decode the current series and save it (`gnus-uu-decode-yenc`).

---

Next: [Viewing Files](#Viewing-Files), Previous: [Other Files](#Other-Files), Up: [Decoding Articles](#Decoding-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.5 Decoding Variables [¶](#Decoding-Variables-1)

Adjective, not verb.

* [Rule Variables](#Rule-Variables)
* [Other Decode Variables](#Other-Decode-Variables)
* [Uuencoding and Posting](#Uuencoding-and-Posting)

---

Next: [Other Decode Variables](#Other-Decode-Variables), Up: [Decoding Variables](#Decoding-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.5.1 Rule Variables [¶](#Rule-Variables-1)

Gnus uses *rule variables* to decide how to view a file. All these
variables are of the form

```
      (list '(regexp1 command2)
            '(regexp2 command2)
            ...)
```

`gnus-uu-user-view-rules` [¶](#index-gnus_002duu_002duser_002dview_002drules)
:   This variable is consulted first when viewing files. If you wish to use,
    for instance, `sox` to convert an `.au` sound file, you could
    say something like:

    ```
    (setq gnus-uu-user-view-rules
          (list '("\\\\.au$" "sox %s -t .aiff > /dev/audio")))
    ```

`gnus-uu-user-view-rules-end` [¶](#index-gnus_002duu_002duser_002dview_002drules_002dend)
:   This variable is consulted if Gnus couldn’t make any matches from the
    user and default view rules.

`gnus-uu-user-archive-rules` [¶](#index-gnus_002duu_002duser_002darchive_002drules)
:   This variable can be used to say what commands should be used to unpack
    archives.

---

Next: [Uuencoding and Posting](#Uuencoding-and-Posting), Previous: [Rule Variables](#Rule-Variables), Up: [Decoding Variables](#Decoding-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.5.2 Other Decode Variables [¶](#Other-Decode-Variables-1)

`gnus-uu-grabbed-file-functions`
:   All functions in this list will be called right after each file has been
    successfully decoded—so that you can move or view files right away,
    and don’t have to wait for all files to be decoded before you can do
    anything. Ready-made functions you can put in this list are:

    `gnus-uu-grab-view` [¶](#index-gnus_002duu_002dgrab_002dview)
    :   View the file.

    `gnus-uu-grab-move` [¶](#index-gnus_002duu_002dgrab_002dmove)
    :   Move the file (if you’re using a saving function.)

`gnus-uu-be-dangerous` [¶](#index-gnus_002duu_002dbe_002ddangerous)
:   Specifies what to do if unusual situations arise during decoding. If
    `nil`, be as conservative as possible. If `t`, ignore things
    that didn’t work, and overwrite existing files. Otherwise, ask each
    time.

`gnus-uu-ignore-files-by-name` [¶](#index-gnus_002duu_002dignore_002dfiles_002dby_002dname)
:   Files with name matching this regular expression won’t be viewed.

`gnus-uu-ignore-files-by-type` [¶](#index-gnus_002duu_002dignore_002dfiles_002dby_002dtype)
:   Files with a MIME type matching this variable won’t be viewed.
    Note that Gnus tries to guess what type the file is based on the name.
    `gnus-uu` is not a MIME package (yet), so this is slightly
    kludgy.

`gnus-uu-tmp-dir` [¶](#index-gnus_002duu_002dtmp_002ddir)
:   Where `gnus-uu` does its work.

`gnus-uu-do-not-unpack-archives` [¶](#index-gnus_002duu_002ddo_002dnot_002dunpack_002darchives)
:   Non-`nil` means that `gnus-uu` won’t peek inside archives
    looking for files to display.

`gnus-uu-view-and-save` [¶](#index-gnus_002duu_002dview_002dand_002dsave)
:   Non-`nil` means that the user will always be asked to save a file
    after viewing it.

`gnus-uu-ignore-default-view-rules` [¶](#index-gnus_002duu_002dignore_002ddefault_002dview_002drules)
:   Non-`nil` means that `gnus-uu` will ignore the default viewing
    rules.

`gnus-uu-ignore-default-archive-rules` [¶](#index-gnus_002duu_002dignore_002ddefault_002darchive_002drules)
:   Non-`nil` means that `gnus-uu` will ignore the default archive
    unpacking commands.

`gnus-uu-kill-carriage-return` [¶](#index-gnus_002duu_002dkill_002dcarriage_002dreturn)
:   Non-`nil` means that `gnus-uu` will strip all carriage returns
    from articles.

`gnus-uu-unmark-articles-not-decoded` [¶](#index-gnus_002duu_002dunmark_002darticles_002dnot_002ddecoded)
:   Non-`nil` means that `gnus-uu` will mark unsuccessfully
    decoded articles as unread.

`gnus-uu-correct-stripped-uucode` [¶](#index-gnus_002duu_002dcorrect_002dstripped_002duucode)
:   Non-`nil` means that `gnus-uu` will *try* to fix
    uuencoded files that have had trailing spaces deleted.

`gnus-uu-pre-uudecode-hook` [¶](#index-gnus_002duu_002dpre_002duudecode_002dhook)
:   Hook run before sending a message to `uudecode`.

`gnus-uu-save-in-digest` [¶](#index-gnus_002duu_002dsave_002din_002ddigest)
:   Non-`nil` means that `gnus-uu`, when asked to save without
    decoding, will save in digests. If this variable is `nil`,
    `gnus-uu` will just save everything in a file without any
    embellishments. The digesting almost conforms to RFC 1153—no easy way
    to specify any meaningful volume and issue numbers were found, so I
    simply dropped them.

---

Previous: [Other Decode Variables](#Other-Decode-Variables), Up: [Decoding Variables](#Decoding-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.5.3 Uuencoding and Posting [¶](#Uuencoding-and-Posting-1)

`gnus-uu-post-include-before-composing` [¶](#index-gnus_002duu_002dpost_002dinclude_002dbefore_002dcomposing)
:   Non-`nil` means that `gnus-uu` will ask for a file to encode
    before you compose the article. If this variable is `t`, you can
    either include an encoded file with `C-c C-i` or have one included
    for you when you post the article.

`gnus-uu-post-length` [¶](#index-gnus_002duu_002dpost_002dlength)
:   Maximum length of an article. The encoded file will be split into how
    many articles it takes to post the entire file.

`gnus-uu-post-threaded` [¶](#index-gnus_002duu_002dpost_002dthreaded)
:   Non-`nil` means that `gnus-uu` will post the encoded file in a
    thread. This may not be smart, as no other decoder I have seen is able
    to follow threads when collecting uuencoded articles. (Well, I have
    seen one package that does that—`gnus-uu`, but somehow, I don’t
    think that counts…) Default is `nil`.

`gnus-uu-post-separate-description` [¶](#index-gnus_002duu_002dpost_002dseparate_002ddescription)
:   Non-`nil` means that the description will be posted in a separate
    article. The first article will typically be numbered (0/x). If this
    variable is `nil`, the description the user enters will be included
    at the beginning of the first article, which will be numbered (1/x).
    Default is `t`.

---

Previous: [Decoding Variables](#Decoding-Variables), Up: [Decoding Articles](#Decoding-Articles)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.17.6 Viewing Files [¶](#Viewing-Files-1)

After decoding, if the file is some sort of archive, Gnus will attempt
to unpack the archive and see if any of the files in the archive can be
viewed. For instance, if you have a gzipped tar file `pics.tar.gz`
containing the files `pic1.jpg` and `pic2.gif`, Gnus will
uncompress and de-tar the main file, and then view the two pictures.
This unpacking process is recursive, so if the archive contains archives
of archives, it’ll all be unpacked.

Finally, Gnus will normally insert a *pseudo-article* for each
extracted file into the summary buffer. If you go to these
“articles”, you will be prompted for a command to run (usually Gnus
will make a suggestion), and then the command will be run.

If `gnus-view-pseudo-asynchronously` is `nil`, Emacs will wait
until the viewing is done before proceeding.

If `gnus-view-pseudos` is `automatic`, Gnus will not insert
the pseudo-articles into the summary buffer, but view them
immediately. If this variable is `not-confirm`, the user won’t even
be asked for a confirmation before viewing is done.

If `gnus-view-pseudos-separately` is non-`nil`, one
pseudo-article will be created for each file to be viewed. If
`nil`, all files that use the same viewing command will be given as
a list of parameters to that command.

If `gnus-insert-pseudo-articles` is non-`nil`, insert
pseudo-articles when decoding. It is `t` by default.

So; there you are, reading your *pseudo-articles* in your
*virtual newsgroup* from the *virtual server*; and you think:
Why isn’t anything real anymore? How did we get here?

---

Next: [MIME Commands](#MIME-Commands), Previous: [Decoding Articles](#Decoding-Articles), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
