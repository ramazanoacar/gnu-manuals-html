### 4.18 Article Treatment [¶](#Article-Treatment-1)

Reading through this huge manual, you may have quite forgotten that the
object of newsreaders is to actually, like, read what people have
written. Reading articles. Unfortunately, people are quite bad at
writing, so there are tons of functions and variables to make reading
these articles easier.

* [Article Highlighting](#Article-Highlighting)
* [Article Fontifying](#Article-Fontifying)
* [Article Hiding](#Article-Hiding)
* [Article Washing](#Article-Washing)
* [Article Header](#Article-Header)
* [Article Buttons](#Article-Buttons)
* [Article button levels](#Article-Button-Levels)
* [Article Date](#Article-Date)
* [Article Display](#Article-Display)
* [Article Signature](#Article-Signature)
* [Article Miscellanea](#Article-Miscellanea)

---

Next: [Article Fontifying](#Article-Fontifying), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.1 Article Highlighting [¶](#Article-Highlighting-1)

Not only do you want your article buffer to look like fruit salad, but
you want it to look like technicolor fruit salad.

`W H a` [¶](#index-W-H-a-_0028Summary_0029)
:   Do much highlighting of the current article
    (`gnus-article-highlight`). This function highlights header, cited
    text, the signature, and adds buttons to the body and the head.

`W H h` [¶](#index-W-H-h-_0028Summary_0029)
:   Highlight the headers (`gnus-article-highlight-headers`). The
    highlighting will be done according to the `gnus-header-face-alist`
    variable, which is a list where each element has the form
    `(regexp name content)`.
    regexp is a regular expression for matching the
    header, name is the face used for highlighting the header name
    (see [Faces and Fonts](#Faces-and-Fonts)) and content is the face for highlighting
    the header value. The first match made will be used. Note that
    regexp shouldn’t have ‘`^`’ prepended—Gnus will add one.

`W H c` [¶](#index-W-H-c-_0028Summary_0029)
:   Highlight cited text (`gnus-article-highlight-citation`).

    Some variables to customize the citation highlights:

    `gnus-cite-parse-max-size`
    :   If the article size in bytes is bigger than this variable (which is
        25000 by default), no citation highlighting will be performed.

    `gnus-cite-max-prefix` [¶](#index-gnus_002dcite_002dmax_002dprefix)
    :   Maximum possible length for a citation prefix (default 20).

    `gnus-cite-face-list` [¶](#index-gnus_002dcite_002dface_002dlist)
    :   List of faces used for highlighting citations (see [Faces and Fonts](#Faces-and-Fonts)).
        When there are citations from multiple articles in the same message,
        Gnus will try to give each citation from each article its own face.
        This should make it easier to see who wrote what.

    `gnus-supercite-regexp` [¶](#index-gnus_002dsupercite_002dregexp)
    :   Regexp matching normal Supercite attribution lines.

    `gnus-supercite-secondary-regexp` [¶](#index-gnus_002dsupercite_002dsecondary_002dregexp)
    :   Regexp matching mangled Supercite attribution lines.

    `gnus-cite-minimum-match-count` [¶](#index-gnus_002dcite_002dminimum_002dmatch_002dcount)
    :   Minimum number of identical prefixes we have to see before we believe
        that it’s a citation.

    `gnus-cite-attribution-prefix` [¶](#index-gnus_002dcite_002dattribution_002dprefix)
    :   Regexp matching the beginning of an attribution line.

    `gnus-cite-attribution-suffix` [¶](#index-gnus_002dcite_002dattribution_002dsuffix)
    :   Regexp matching the end of an attribution line.

    `gnus-cite-attribution-face` [¶](#index-gnus_002dcite_002dattribution_002dface)
    :   Face used for attribution lines. It is merged with the face for the
        cited text belonging to the attribution.

    `gnus-cite-ignore-quoted-from` [¶](#index-gnus_002dcite_002dignore_002dquoted_002dfrom)
    :   If non-`nil`, no citation highlighting will be performed on lines
        beginning with ‘`>From` ’. Those lines may have been quoted by MTAs
        in order not to mix up with the envelope From line. The default value
        is `t`.

`W H s` [¶](#index-W-H-s-_0028Summary_0029)
:   Highlight the signature (`gnus-article-highlight-signature`).
    Everything after `gnus-signature-separator` (see [Article Signature](#Article-Signature)) in an article will be considered a signature and will be
    highlighted with `gnus-signature-face`, which is `italic` by
    default.

See [Customizing Articles](#Customizing-Articles), for how to highlight articles automatically.

---

Next: [Article Hiding](#Article-Hiding), Previous: [Article Highlighting](#Article-Highlighting), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.2 Article Fontifying [¶](#Article-Fontifying-1)

People commonly add emphasis to words in news articles by writing things
like ‘`_this_`’ or ‘`*this*`’ or ‘`/this/`’. Gnus can make
this look nicer by running the article through the `W e`
(`gnus-article-emphasize`) command.

How the emphasis is computed is controlled by the
`gnus-emphasis-alist` variable. This is an alist where the first
element is a regular expression to be matched. The second is a number
that says what regular expression grouping is used to find the entire
emphasized word. The third is a number that says what regexp grouping
should be displayed and highlighted. (The text between these two
groupings will be hidden.) The fourth is the face used for
highlighting.

```
(setq gnus-emphasis-alist
      '(("_\\(\\w+\\)_" 0 1 gnus-emphasis-underline)
        ("\\*\\(\\w+\\)\\*" 0 1 gnus-emphasis-bold)))
```

By default, there are seven rules, and they use the following faces:
`gnus-emphasis-bold`, `gnus-emphasis-italic`,
`gnus-emphasis-underline`, `gnus-emphasis-bold-italic`,
`gnus-emphasis-underline-italic`,
`gnus-emphasis-underline-bold`, and
`gnus-emphasis-underline-bold-italic`.

If you want to change these faces, you can either use `M-x
customize`, or you can use `copy-face`. For instance, if you want
to make `gnus-emphasis-italic` use a red face instead, you could
say something like:

```
(copy-face 'red 'gnus-emphasis-italic)
```

If you want to highlight arbitrary words, you can use the
`gnus-group-highlight-words-alist` variable, which uses the same
syntax as `gnus-emphasis-alist`. The `highlight-words` group
parameter (see [Group Parameters](#Group-Parameters)) can also be used.

See [Customizing Articles](#Customizing-Articles), for how to fontize articles automatically.

---

Next: [Article Washing](#Article-Washing), Previous: [Article Fontifying](#Article-Fontifying), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.3 Article Hiding [¶](#Article-Hiding-1)

Or rather, hiding certain things in each article. There usually is much
too much cruft in most articles.

`W W a` [¶](#index-W-W-a-_0028Summary_0029)
:   Do quite a lot of hiding on the article buffer
    (`gnus-article-hide`). In particular, this function will hide
    headers, PGP, cited text and the signature.

`W W h` [¶](#index-W-W-h-_0028Summary_0029)
:   Hide headers (`gnus-article-hide-headers`). See [Hiding Headers](#Hiding-Headers).

`W W b` [¶](#index-W-W-b-_0028Summary_0029)
:   Hide headers that aren’t particularly interesting
    (`gnus-article-hide-boring-headers`). See [Hiding Headers](#Hiding-Headers).

`W W s` [¶](#index-W-W-s-_0028Summary_0029)
:   Hide signature (`gnus-article-hide-signature`). See [Article Signature](#Article-Signature).

`W W l` [¶](#index-W-W-l-_0028Summary_0029)
:   Strip list identifiers specified in `gnus-list-identifiers`. These
    are strings some mailing list servers add to the beginning of all
    `Subject` headers—for example, ‘`[zebra 4711]`’. Any leading
    ‘`Re:` ’ is skipped before stripping. `gnus-list-identifiers`
    may not contain `\\(..\\)`.

    `gnus-list-identifiers` [¶](#index-gnus_002dlist_002didentifiers-2)
    :   A regular expression that matches list identifiers to be removed from
        subject. This can also be a list of regular expressions.

`W W P` [¶](#index-W-W-P-_0028Summary_0029)
:   Hide PEM (privacy enhanced messages) cruft
    (`gnus-article-hide-pem`).

`W W B` [¶](#index-W-W-B-_0028Summary_0029)
:   Strip the banner specified by the `banner` group parameter
    (`gnus-article-strip-banner`). This is mainly used to hide those
    annoying banners and/or signatures that some mailing lists and moderated
    groups adds to all the messages. The way to use this function is to add
    the `banner` group parameter (see [Group Parameters](#Group-Parameters)) to the
    group you want banners stripped from. The parameter either be a string,
    which will be interpreted as a regular expression matching text to be
    removed, or the symbol `signature`, meaning that the (last)
    signature should be removed, or other symbol, meaning that the
    corresponding regular expression in `gnus-article-banner-alist` is
    used.

    For instance:

    ```
    (setq gnus-article-banner-alist
          ((googleGroups .
           "^\n*--~--~---------\\(.+\n\\)+")))
    ```

    Regardless of a group, you can hide things like advertisements only when
    the sender of an article has a certain mail address specified in
    `gnus-article-address-banner-alist`.

    `gnus-article-address-banner-alist` [¶](#index-gnus_002darticle_002daddress_002dbanner_002dalist-1)
    :   Alist of mail addresses and banners. Each element has the form
        `(address . banner)`, where address is a regexp
        matching a mail address in the From header, banner is one of a
        symbol `signature`, an item in `gnus-article-banner-alist`,
        a regexp and `nil`. If address matches author’s mail
        address, it will remove things like advertisements. For example, if a
        sender has the mail address ‘`hail@yoo-hoo.co.jp`’ and there is a
        banner something like ‘`Do You Yoo-hoo!?`’ in all articles he
        sends, you can use the following element to remove them:

        ```
        ("@yoo-hoo\\.co\\.jp\\'" .
         "\n_+\nDo You Yoo-hoo!\\?\n.*\n.*\n")
        ```

`W W c` [¶](#index-W-W-c-_0028Summary_0029)
:   Hide citation (`gnus-article-hide-citation`). Some variables for
    customizing the hiding:

    `gnus-cited-opened-text-button-line-format` [¶](#index-gnus_002dcited_002dclosed_002dtext_002dbutton_002dline_002dformat)

    `gnus-cited-closed-text-button-line-format`
    :   Gnus adds buttons to show where the cited text has been hidden, and to
        allow toggle hiding the text. The format of the variable is specified
        by these format-like variable (see [Formatting Variables](#Formatting-Variables)). These
        specs are valid:

        ‘`b`’
        :   Starting point of the hidden text.

        ‘`e`’
        :   Ending point of the hidden text.

        ‘`l`’
        :   Number of characters in the hidden region.

        ‘`n`’
        :   Number of lines of hidden text.

    `gnus-cited-lines-visible` [¶](#index-gnus_002dcited_002dlines_002dvisible)
    :   The number of lines at the beginning of the cited text to leave
        shown. This can also be a cons cell with the number of lines at the top
        and bottom of the text, respectively, to remain visible.

`W W C-c` [¶](#index-W-W-C_002dc-_0028Summary_0029)
:   Hide citation (`gnus-article-hide-citation-maybe`) depending on the
    following two variables:

    `gnus-cite-hide-percentage` [¶](#index-gnus_002dcite_002dhide_002dpercentage)
    :   If the cited text is of a bigger percentage than this variable (default
        50), hide the cited text.

    `gnus-cite-hide-absolute` [¶](#index-gnus_002dcite_002dhide_002dabsolute)
    :   The cited text must have at least this length (default 10) before it
        is hidden.

`W W C` [¶](#index-W-W-C-_0028Summary_0029)
:   Hide cited text in articles that aren’t roots
    (`gnus-article-hide-citation-in-followups`). This isn’t very
    useful as an interactive command, but might be a handy function to stick
    have happen automatically (see [Customizing Articles](#Customizing-Articles)).

All these “hiding” commands are toggles, but if you give a negative
prefix to these commands, they will show what they have previously
hidden. If you give a positive prefix, they will always hide.

Also see [Article Highlighting](#Article-Highlighting) for further variables for
citation customization.

See [Customizing Articles](#Customizing-Articles), for how to hide article elements
automatically.

---

Next: [Article Header](#Article-Header), Previous: [Article Hiding](#Article-Hiding), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.4 Article Washing [¶](#Article-Washing-1)

We call this “article washing” for a really good reason. Namely, the
`A` key was taken, so we had to use the `W` key instead.

*Washing* is defined by us as “changing something from something to
something else”, but normally results in something looking better.
Cleaner, perhaps.

See [Customizing Articles](#Customizing-Articles), if you want to change how Gnus displays
articles by default.

`C-u g`
:   This is not really washing, it’s sort of the opposite of washing. If
    you type this, you see the article exactly as it exists on disk or on
    the server.

`g`
:   Force redisplaying of the current article
    (`gnus-summary-show-article`). This is also not really washing.
    If you type this, you see the article without any previously applied
    interactive Washing functions but with all default treatments
    (see [Customizing Articles](#Customizing-Articles)).

`W l` [¶](#index-W-l-_0028Summary_0029)
:   Remove page breaks from the current article
    (`gnus-summary-stop-page-breaking`). See [Misc Article](#Misc-Article), for page
    delimiters.

`W r` [¶](#index-W-r-_0028Summary_0029)
:   Do a Caesar rotate (rot13) on the article buffer
    (`gnus-summary-caesar-message`).
    Unreadable articles that tell you to read them with Caesar rotate or rot13.
    (Typically offensive jokes and such.)

    It’s commonly called “rot13” because each letter is rotated 13
    positions in the alphabet, e.g., ‘`B`’ (letter #2) -> ‘`O`’ (letter
    #15). It is sometimes referred to as “Caesar rotate” because Caesar
    is rumored to have employed this form of, uh, somewhat weak encryption.

`W m` [¶](#index-W-m-_0028Summary_0029)
:   Morse decode the article buffer (`gnus-summary-morse-message`).

`W i` [¶](#index-W-i-_0028Summary_0029)
:   Decode IDNA encoded domain names in the current articles. IDNA
    encoded domain names looks like ‘`xn--bar`’. If a string remain
    unencoded after running invoking this, it is likely an invalid IDNA
    string (‘`xn--bar`’ is invalid). You must have GNU Libidn
    (<https://www.gnu.org/software/libidn/>) installed for this command
    to work.

`W t`

`t` [¶](#index-W-t-_0028Summary_0029)
:   Toggle whether to display all headers in the article buffer
    (`gnus-summary-toggle-header`).

`W v` [¶](#index-W-v-_0028Summary_0029)
:   Toggle whether to display all headers in the article buffer permanently
    (`gnus-summary-verbose-headers`).

`W o` [¶](#index-W-o-_0028Summary_0029)
:   Treat overstrike (`gnus-article-treat-overstrike`).

`W d` [¶](#index-W-d-_0028Summary_0029)
:   Treat “Microsoft smartquotes” according to
    `gnus-article-smartquotes-map`
    (`gnus-article-treat-smartquotes`). Note that this function guesses
    whether a character is a smartquote or not, so it should only be used
    interactively.

    Smartquotes are Microsoft’s unilateral extension to the character map in
    an attempt to provide more quoting characters. If you see something
    like `\222` or `\264` where you’re expecting some kind of
    apostrophe or quotation mark, then try this wash.

`W U` [¶](#index-W-U-_0028Summary_0029)
:   Translate many non-ASCII characters into their
    ASCII equivalents (`gnus-article-treat-non-ascii`).
    This is mostly useful if you’re on a terminal that has a limited font
    and doesn’t show accented characters, “advanced” punctuation, and the
    like. For instance, ‘`»`’ is translated into ‘`>>`’, and so on.

`W Y f` [¶](#index-W-Y-f-_0028Summary_0029)
:   Full deuglify of broken Outlook (Express) articles: Treat
    \"smartquotes\", unwrap lines, repair attribution and rearrange
    citation (`gnus-article-outlook-deuglify-article`).

`W Y u` [¶](#index-W-Y-u-_0028Summary_0029)
:   Unwrap lines that appear to be wrapped citation lines. You can control
    what lines will be unwrapped by frobbing
    `gnus-outlook-deuglify-unwrap-min` and
    `gnus-outlook-deuglify-unwrap-max`, indicating the minimum and
    maximum length of an unwrapped citation line.
    (`gnus-article-outlook-unwrap-lines`).

`W Y a` [¶](#index-W-Y-a-_0028Summary_0029)
:   Repair a broken attribution line.  
    (`gnus-article-outlook-repair-attribution`).

`W Y c` [¶](#index-W-Y-c-_0028Summary_0029)
:   Repair broken citations by rearranging the text.
    (`gnus-article-outlook-rearrange-citation`).

`W w` [¶](#index-W-w-_0028Summary_0029)
:   Do word wrap (`gnus-article-fill-cited-article`).

    You can give the command a numerical prefix to specify the width to use
    when filling.

`W Q` [¶](#index-W-Q-_0028Summary_0029)
:   Fill long lines (`gnus-article-fill-long-lines`).

    You can give the command a numerical prefix to specify the width to use
    when filling.

`W C` [¶](#index-W-C-_0028Summary_0029)
:   Capitalize the first word in each sentence
    (`gnus-article-capitalize-sentences`).

`W c` [¶](#index-W-c-_0028Summary_0029)
:   Translate CRLF pairs (i.e., ‘`^M`’s on the end of the lines) into LF
    (this takes care of DOS line endings), and then translate any remaining
    CRs into LF (this takes care of Mac line endings)
    (`gnus-article-remove-cr`).

`W q` [¶](#index-W-q-_0028Summary_0029)
:   Treat quoted-printable (`gnus-article-de-quoted-unreadable`).
    Quoted-Printable is one common MIME encoding employed when
    sending non-ASCII (i.e., 8-bit) articles. It typically
    makes strings like ‘`déjà vu`’ look like ‘`d=E9j=E0 vu`’,
    which doesn’t look very readable to me. Note that this is usually
    done automatically by Gnus if the message in question has a
    `Content-Transfer-Encoding` header that says that this encoding
    has been done. If a prefix is given, a charset will be asked for.

`W 6` [¶](#index-W-6-_0028Summary_0029)
:   Treat base64 (`gnus-article-de-base64-unreadable`). Base64 is
    one common MIME encoding employed when sending
    non-ASCII (i.e., 8-bit) articles. Note that this is
    usually done automatically by Gnus if the message in question has a
    `Content-Transfer-Encoding` header that says that this encoding
    has been done. If a prefix is given, a charset will be asked for.

`W Z` [¶](#index-W-Z-_0028Summary_0029)
:   Treat HZ or HZP (`gnus-article-decode-HZ`). HZ (or HZP) is one
    common encoding employed when sending Chinese articles. It typically
    makes strings look like ‘`~{<:Ky2;S{#,NpJ)l6HK!#~}`’.

`W A` [¶](#index-W-A-_0028Summary_0029)
:   Translate ANSI SGR control sequences into overlays or
    extents (`gnus-article-treat-ansi-sequences`). ANSI
    sequences are used in some Chinese hierarchies for highlighting.

`W u` [¶](#index-W-u-_0028Summary_0029)
:   Remove newlines from within URLs. Some mailers insert newlines into
    outgoing email messages to keep lines short. This reformatting can
    split long URLs onto multiple lines. Repair those URLs by removing
    the newlines (`gnus-article-unsplit-urls`).

`W h` [¶](#index-W-h-_0028Summary_0029)
:   Treat HTML (`gnus-article-wash-html`). Note that this is
    usually done automatically by Gnus if the message in question has a
    `Content-Type` header that says that the message is HTML.

    If a prefix is given, a charset will be asked for. If it is a number,
    the charset defined in `gnus-summary-show-article-charset-alist`
    (see [Scrolling the Article](#Paging-the-Article)) will be used.

    The default is to use the function specified by
    `mm-text-html-renderer` (see [Display
    Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Display-Customization) in The Emacs MIME Manual) to convert the
    HTML. Pre-defined functions you can use include:

    `shr`
    :   Use Gnus simple html renderer.

    `gnus-w3m`
    :   Use Gnus rendered based on w3m.

    `w3m`
    :   Use [emacs-w3m](http://emacs-w3m.namazu.org/).

    `w3m-standalone`
    :   Use [w3m](https://w3m.sourceforge.net/).

    `links`
    :   Use [Links](http://links.twibright.com/).

    `lynx`
    :   Use [Lynx](https://lynx.browser.org/).

`W D F` [¶](#index-W-D-F-_0028Summary_0029)
:   Toggle proportional fonts for HTML articles. This temporarily
    changes the `shr-use-fonts` variable in the current article buffer.

`W b` [¶](#index-W-b-_0028Summary_0029)
:   Add clickable buttons to the article (`gnus-article-add-buttons`).
    See [Article Buttons](#Article-Buttons).

`W B` [¶](#index-W-B-_0028Summary_0029)
:   Add clickable buttons to the article headers
    (`gnus-article-add-buttons-to-head`).

`W p` [¶](#index-W-p-_0028Summary_0029)
:   Verify a signed control message
    (`gnus-article-verify-x-pgp-sig`). Control messages such as
    `newgroup` and `checkgroups` are usually signed by the
    hierarchy maintainer. You need to add the PGP public key of
    the maintainer to your keyring to verify the
    message.[1](#FOOT1)

`W s` [¶](#index-W-s-_0028Summary_0029)
:   Verify a signed (PGP, PGP/MIME or
    S/MIME) message
    (`gnus-summary-force-verify-and-decrypt`). See [Security](#Security).

`W a` [¶](#index-W-a-_0028Summary_0029)
:   Strip headers like the `X-No-Archive` header from the beginning of
    article bodies (`gnus-article-strip-headers-in-body`).

`W E l` [¶](#index-W-E-l-_0028Summary_0029)
:   Remove all blank lines from the beginning of the article
    (`gnus-article-strip-leading-blank-lines`).

`W E m` [¶](#index-W-E-m-_0028Summary_0029)
:   Replace all blank lines with empty lines and then all multiple empty
    lines with a single empty line.
    (`gnus-article-strip-multiple-blank-lines`).

`W E t` [¶](#index-W-E-t-_0028Summary_0029)
:   Remove all blank lines at the end of the article
    (`gnus-article-remove-trailing-blank-lines`).

`W E a` [¶](#index-W-E-a-_0028Summary_0029)
:   Do all the three commands above
    (`gnus-article-strip-blank-lines`).

`W E A` [¶](#index-W-E-A-_0028Summary_0029)
:   Remove all blank lines
    (`gnus-article-strip-all-blank-lines`).

`W E s` [¶](#index-W-E-s-_0028Summary_0029)
:   Remove all white space from the beginning of all lines of the article
    body (`gnus-article-strip-leading-space`).

`W E e` [¶](#index-W-E-e-_0028Summary_0029)
:   Remove all white space from the end of all lines of the article
    body (`gnus-article-strip-trailing-space`).

See [Customizing Articles](#Customizing-Articles), for how to wash articles automatically.

---

Next: [Article Buttons](#Article-Buttons), Previous: [Article Washing](#Article-Washing), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.5 Article Header [¶](#Article-Header-1)

These commands perform various transformations of article header.

`W G u` [¶](#index-W-G-u-_0028Summary_0029)
:   Unfold folded header lines (`gnus-article-treat-unfold-headers`).

`W G n` [¶](#index-W-G-n-_0028Summary_0029)
:   Fold the `Newsgroups` and `Followup-To` headers
    (`gnus-article-treat-fold-newsgroups`).

`W G f` [¶](#index-W-G-f-_0028Summary_0029)
:   Fold all the message headers
    (`gnus-article-treat-fold-headers`).

`W E w` [¶](#index-W-E-w-_0028Summary_0029)
:   Remove excessive whitespace from all headers
    (`gnus-article-remove-leading-whitespace`).

---

Next: [Article button levels](#Article-Button-Levels), Previous: [Article Header](#Article-Header), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.6 Article Buttons [¶](#Article-Buttons-1)

People often include references to other stuff in articles, and it would
be nice if Gnus could just fetch whatever it is that people talk about
with the minimum of fuzz when you hit `RET` or use the middle mouse
button on these references.

Gnus adds *buttons* to certain standard references by default:
Well-formed URLs, mail addresses, Message-IDs, Info links, man pages and
Emacs or Gnus related references. This is controlled by two variables,
one that handles article bodies and one that handles article heads:

`gnus-button-alist` [¶](#index-gnus_002dbutton_002dalist)
:   This is an alist where each entry has this form:

    ```
    (regexp button-par use-p function data-par)
    ```

    regexp
    :   All text that match this regular expression (case insensitive) will be
        considered an external reference. Here’s a typical regexp that matches
        embedded URLs: ‘`<URL:\\([^\n\r>]*\\)>`’. This can also be a
        variable containing a regexp, useful variables to use include
        `gnus-button-url-regexp` and `gnus-button-mid-or-mail-regexp`.

    button-par
    :   Gnus has to know which parts of the matches is to be highlighted. This
        is a number that says what sub-expression of the regexp is to be
        highlighted. If you want it all highlighted, you use 0 here.

    use-p
    :   This form will be `eval`ed, and if the result is non-`nil`,
        this is considered a match. This is useful if you want extra sifting to
        avoid false matches. Often variables named
        `gnus-button-*-level` are used here, See [Article button levels](#Article-Button-Levels), but any other form may be used too.

    function
    :   This function will be called when you click on this button.

    data-par
    :   As with button-par, this is a sub-expression number, but this one
        says which part of the match is to be sent as data to function.

    So the full entry for buttonizing URLs is then

    ```
    ("<URL:\\([^\n\r>]*\\)>" 0 t gnus-button-url 1)
    ```

`gnus-header-button-alist` [¶](#index-gnus_002dheader_002dbutton_002dalist)
:   This is just like the other alist, except that it is applied to the
    article head only, and that each entry has an additional element that is
    used to say what headers to apply the buttonize coding to:

    ```
    (header regexp button-par use-p function data-par)
    ```

    header is a regular expression.

* [Related variables and functions](#Related-variables-and-functions)

#### 4.18.6.1 Related variables and functions [¶](#Related-variables-and-functions)

`gnus-button-*-level`
:   See [Article button levels](#Article-Button-Levels).

`gnus-button-url-regexp` [¶](#index-gnus_002dbutton_002durl_002dregexp)
:   A regular expression that matches embedded URLs. It is used in the
    default values of the variables above.

`gnus-button-man-handler` [¶](#index-gnus_002dbutton_002dman_002dhandler-1)
:   The function to use for displaying man pages. It must take at least one
    argument with a string naming the man page.

`gnus-button-mid-or-mail-regexp` [¶](#index-gnus_002dbutton_002dmid_002dor_002dmail_002dregexp)
:   Regular expression that matches a message ID or a mail address.

`gnus-button-prefer-mid-or-mail` [¶](#index-gnus_002dbutton_002dprefer_002dmid_002dor_002dmail)
:   This variable determines what to do when the button on a string as
    ‘`foo123@bar.invalid`’ is pushed. Strings like this can be either a
    message ID or a mail address. If it is one of the symbols `mid` or
    `mail`, Gnus will always assume that the string is a message ID or
    a mail address, respectively. If this variable is set to the symbol
    `ask`, always query the user what to do. If it is a function, this
    function will be called with the string as its only argument. The
    function must return `mid`, `mail`, `invalid` or
    `ask`. The default value is the function
    `gnus-button-mid-or-mail-heuristic`.

`gnus-button-mid-or-mail-heuristic` [¶](#index-gnus_002dbutton_002dmid_002dor_002dmail_002dheuristic)
:   Function that guesses whether its argument is a message ID or a mail
    address. Returns `mid` if it’s a message ID, `mail` if
    it’s a mail address, `ask` if unsure and `invalid` if the
    string is invalid.

`gnus-button-mid-or-mail-heuristic-alist` [¶](#index-gnus_002dbutton_002dmid_002dor_002dmail_002dheuristic_002dalist)
:   An alist of `(RATE . REGEXP)` pairs used by the function
    `gnus-button-mid-or-mail-heuristic`.

`gnus-article-button-face` [¶](#index-gnus_002darticle_002dbutton_002dface)
:   Face used on buttons.

`gnus-article-mouse-face` [¶](#index-gnus_002darticle_002dmouse_002dface)
:   Face used when the mouse cursor is over a button.

See [Customizing Articles](#Customizing-Articles), for how to buttonize articles automatically.

---

Next: [Article Date](#Article-Date), Previous: [Article Buttons](#Article-Buttons), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.7 Article button levels [¶](#Article-button-levels)

The higher the value of the variables `gnus-button-*-level`,
the more buttons will appear. If the level is zero, no corresponding
buttons are displayed. With the default value (which is 5) you should
already see quite a lot of buttons. With higher levels, you will see
more buttons, but you may also get more false positives. To avoid them,
you can set the variables `gnus-button-*-level` local to
specific groups (see [Group Parameters](#Group-Parameters)). Here’s an example for the
variable `gnus-parameters`:

```
;; increase gnus-button-*-level in some groups:
(setq gnus-parameters
      '(("\\<\\(emacs\\|gnus\\)\\>" (gnus-button-emacs-level 10))
        ("\\<unix\\>"               (gnus-button-man-level 10))
        ("\\<tex\\>"                (gnus-button-tex-level 10))))
```

`gnus-button-browse-level` [¶](#index-gnus_002dbutton_002dbrowse_002dlevel)
:   Controls the display of references to message IDs, mail addresses and
    news URLs. Related variables and functions include
    `gnus-button-url-regexp`, `browse-url`, and
    `browse-url-browser-function`.

`gnus-button-emacs-level` [¶](#index-gnus_002dbutton_002demacs_002dlevel)
:   Controls the display of Emacs or Gnus references. Related functions are
    `gnus-button-handle-custom`,
    `gnus-button-handle-describe-function`,
    `gnus-button-handle-describe-variable`,
    `gnus-button-handle-symbol`,
    `gnus-button-handle-describe-key`,
    `gnus-button-handle-apropos`,
    `gnus-button-handle-apropos-command`,
    `gnus-button-handle-apropos-variable`,
    `gnus-button-handle-apropos-documentation`, and
    `gnus-button-handle-library`.

`gnus-button-man-level` [¶](#index-gnus_002dbutton_002dman_002dlevel)
:   Controls the display of references to (Unix) man pages.
    See `gnus-button-man-handler`.

`gnus-button-message-level` [¶](#index-gnus_002dbutton_002dmessage_002dlevel)
:   Controls the display of message IDs, mail addresses and news URLs.
    Related variables and functions include
    `gnus-button-mid-or-mail-regexp`,
    `gnus-button-prefer-mid-or-mail`,
    `gnus-button-mid-or-mail-heuristic`, and
    `gnus-button-mid-or-mail-heuristic-alist`.

---

Next: [Article Display](#Article-Display), Previous: [Article button levels](#Article-Button-Levels), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.8 Article Date [¶](#Article-Date-1)

The date is most likely generated in some obscure timezone you’ve never
heard of, so it’s quite nice to be able to find out what the time was
when the article was sent.

`W T u` [¶](#index-W-T-u-_0028Summary_0029)
:   Display the date in UT (aka. GMT, aka ZULU)
    (`gnus-article-date-ut`).

`W T i` [¶](#index-W-T-i-_0028Summary_0029)
:   Display the date in international format, aka. ISO 8601
    (`gnus-article-date-iso8601`).

`W T l` [¶](#index-W-T-l-_0028Summary_0029)
:   Display the date in the local timezone (`gnus-article-date-local`).

`W T p` [¶](#index-W-T-p-_0028Summary_0029)
:   Display the date in a format that’s easily pronounceable in English
    (`gnus-article-date-english`).

`W T s` [¶](#index-W-T-s-_0028Summary_0029)
:   Display the date using a user-defined format
    (`gnus-article-date-user`). The format is specified by the
    `gnus-article-time-format` variable, and is a string that’s passed
    to `format-time-string`. See the documentation of that variable
    for a list of possible format specs.

`W T e` [¶](#index-W-T-e-_0028Summary_0029)
:   Say how much time has elapsed between the article was posted and now
    (`gnus-article-date-lapsed`). It looks something like:

    ```
    Date: 6 weeks, 4 days, 1 hour, 3 minutes, 8 seconds ago
    ```

    To make this line updated continually, set the
    `gnus-article-update-date-headers` variable to the frequency in
    seconds (the default is `nil`).

`W T o` [¶](#index-W-T-o-_0028Summary_0029)
:   Display the original date (`gnus-article-date-original`). This can
    be useful if you normally use some other conversion function and are
    worried that it might be doing something totally wrong. Say, claiming
    that the article was posted in 1854. Although something like that is
    *totally* impossible. Don’t you trust me? \*titter\*

See [Customizing Articles](#Customizing-Articles), for how to display the date in your
preferred format automatically.

---

Next: [Article Signature](#Article-Signature), Previous: [Article Date](#Article-Date), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.9 Article Display [¶](#Article-Display-1)

These commands add various frivolous display gimmicks to the article
buffer in Emacs versions that support them.

`X-Face` headers are small black-and-white images supplied by the
message headers (see [X-Face](#X_002dFace)).

`Face` headers are small colored images supplied by the message
headers (see [Face](#Face)).

Smileys are those little ‘`:-)`’ symbols that people like to litter
their messages with (see [Smileys](#Smileys)).

Picons, on the other hand, reside on your own system, and Gnus will
try to match the headers to what you have (see [Picons](#Picons)).

Gravatars reside on-line and are fetched from
<https://en.gravatar.com/> (see [Gravatars](#Gravatars)).

All these functions are toggles—if the elements already exist,
they’ll be removed.

`W D x` [¶](#index-W-D-x-_0028Summary_0029)
:   Display an `X-Face` in the `From` header.
    (`gnus-article-display-x-face`).

`W D d` [¶](#index-W-D-d-_0028Summary_0029)
:   Display a `Face` in the `From` header.
    (`gnus-article-display-face`).

`W D s` [¶](#index-W-D-s-_0028Summary_0029)
:   Display smileys (`gnus-treat-smiley`).

`W D f` [¶](#index-W-D-f-_0028Summary_0029)
:   Piconify the `From` header (`gnus-treat-from-picon`).

`W D m` [¶](#index-W-D-m-_0028Summary_0029)
:   Piconify all mail headers (i.e., `Cc`, `To`)
    (`gnus-treat-mail-picon`).

`W D n` [¶](#index-W-D-n-_0028Summary_0029)
:   Piconify all news headers (i.e., `Newsgroups` and
    `Followup-To`) (`gnus-treat-newsgroups-picon`).

`W D g` [¶](#index-W-D-g-_0028Summary_0029)
:   Gravatarify the `From` header (`gnus-treat-from-gravatar`).

`W D h` [¶](#index-W-D-h-_0028Summary_0029)
:   Gravatarify all mail headers (i.e., `Cc`, `To`)
    (`gnus-treat-from-gravatar`).

`W D e` [¶](#index-W-D-e-_0028Summary_0029)
:   Some symbols have both a non-emoji presentation and an emoji
    presentation. This command will make Gnus choose the emoji presentation
    (`gnus-article-emojize-symbols`).

`W D D` [¶](#index-W-D-D-_0028Summary_0029)
:   Remove all images from the article buffer
    (`gnus-article-remove-images`).

`W D W` [¶](#index-W-D-W-_0028Summary_0029)
:   If you’re reading an HTML article rendered with
    `gnus-article-html`, then you can insert any blocked images in
    the buffer with this command.
    (`gnus-html-show-images`).

---

Next: [Article Miscellanea](#Article-Miscellanea), Previous: [Article Display](#Article-Display), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.10 Article Signature [¶](#Article-Signature-1)

Each article is divided into two parts—the head and the body. The
body can be divided into a signature part and a text part. The variable
that says what is to be considered a signature is
`gnus-signature-separator`. This is normally the standard
‘`^-- $`’ as mandated by RFC 5536. However, many people use
non-standard signature separators, so this variable can also be a list
of regular expressions to be tested, one by one. (Searches are done
from the end of the body towards the beginning.) One likely value is:

```
(setq gnus-signature-separator
      '("^-- $"         ; The standard
        "^-- *$"        ; A common mangling
        "^-------*$"    ; Many people just use a looong
                        ; line of dashes.  Shame!
        "^ *--------*$" ; Double-shame!
        "^________*$"   ; Underscores are also popular
        "^========*$")) ; Pervert!
```

The more permissive you are, the more likely it is that you’ll get false
positives.

`gnus-signature-limit` provides a limit to what is considered a
signature when displaying articles.

1. If it is an integer, no signature may be longer (in characters) than
   that integer.
2. If it is a floating point number, no signature may be longer (in lines)
   than that number.
3. If it is a function, the function will be called without any parameters,
   and if it returns `nil`, there is no signature in the buffer.
4. If it is a string, it will be used as a regexp. If it matches, the text
   in question is not a signature.

This variable can also be a list where the elements may be of the types
listed above. Here’s an example:

```
(setq gnus-signature-limit
      '(200.0 "^---*Forwarded article"))
```

This means that if there are more than 200 lines after the signature
separator, or the text after the signature separator is matched by
the regular expression ‘`^---*Forwarded article`’, then it isn’t a
signature after all.

---

Previous: [Article Signature](#Article-Signature), Up: [Article Treatment](#Article-Treatment)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.18.11 Article Miscellanea [¶](#Article-Miscellanea-1)

`A t` [¶](#index-A-t-_0028Summary_0029)
:   Translate the article from one language to another
    (`gnus-article-babel`).

---

Next: [Charsets](#Charsets), Previous: [Article Treatment](#Article-Treatment), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.19 MIME Commands [¶](#MIME-Commands-1)

The following commands all understand the numerical prefix. For
instance, `3 K v` means “view the third MIME part”.

`b` [¶](#index-b-_0028Summary_0029)

`K v`
:   View the MIME part.

`K o` [¶](#index-K-o-_0028Summary_0029)
:   Save the MIME part.

`K O` [¶](#index-K-O-_0028Summary_0029)
:   Prompt for a file name, then save the MIME part and strip it
    from the article. The stripped MIME object will be referred
    via the message/external-body MIME type.

`K r` [¶](#index-K-r-_0028Summary_0029)
:   Replace the MIME part with an external body.

`K d` [¶](#index-K-d-_0028Summary_0029)
:   Delete the MIME part and add some information about the
    removed part.

`K c` [¶](#index-K-c-_0028Summary_0029)
:   Copy the MIME part.

`K e` [¶](#index-K-e-_0028Summary_0029)
:   View the MIME part externally.

`K i` [¶](#index-K-i-_0028Summary_0029)
:   View the MIME part internally.

`K |` [¶](#index-K-_007c-_0028Summary_0029)
:   Pipe the MIME part to an external command.

The rest of these MIME commands do not use the numerical prefix in
the same manner:

`K H` [¶](#index-K-H-_0028Summary_0029)
:   View ‘`text/html`’ parts of the current article with a WWW browser.
    Inline images embedded in a message using the `cid` scheme, as they
    are generally considered to be safe, will be processed properly. The
    message header is added to the beginning of every HTML part
    unless the prefix argument is given.

    Warning: Spammers use links to images (using the `http` scheme) in
    HTML articles to verify whether you have read the message. As
    this command passes the HTML content to the browser without
    eliminating these “web bugs” you should only use it for mails from
    trusted senders.

    This command creates temporary files to pass HTML contents
    including images if any to the browser, and deletes them when exiting
    the group (if you want).

`K b` [¶](#index-K-b-_0028Summary_0029)
:   Make all the MIME parts have buttons in front of them. This is
    mostly useful if you wish to save (or perform other actions) on inlined
    parts.

`W M h` [¶](#index-W-M-h-_0028Summary_0029)
:   Display MIME part buttons in the end of the header of an
    article (`gnus-mime-buttonize-attachments-in-header`). This
    command toggles the display. Note that buttons to be added to the
    header are only the ones that aren’t inlined in the body. If you want
    those buttons always to be displayed, set
    `gnus-mime-display-attachment-buttons-in-header` to non-`nil`.
    The default is `t`. To change the appearance of buttons, customize
    `gnus-header-face-alist`.

`K m` [¶](#index-K-m-_0028Summary_0029)
:   Some multipart messages are transmitted with missing or faulty headers.
    This command will attempt to “repair” these messages so that they can
    be viewed in a more pleasant manner
    (`gnus-summary-repair-multipart`).

`X m` [¶](#index-X-m-_0028Summary_0029)
:   Save all parts matching a MIME type to a directory
    (`gnus-summary-save-parts`). Understands the process/prefix
    convention (see [Process/Prefix](#Process_002fPrefix)).

`M-t` [¶](#index-M_002dt-_0028Summary_0029)
:   Toggle the buttonized display of the article buffer
    (`gnus-summary-toggle-display-buttonized`).

`W M w` [¶](#index-W-M-w-_0028Summary_0029)
:   Decode RFC 2047-encoded words in the article headers
    (`gnus-article-decode-mime-words`).

`W M c` [¶](#index-W-M-c-_0028Summary_0029)
:   Decode encoded article bodies as well as charsets
    (`gnus-article-decode-charset`).

    This command looks in the `Content-Type` header to determine the
    charset. If there is no such header in the article, you can give it a
    prefix, which will prompt for the charset to decode as. In regional
    groups where people post using some common encoding (but do not
    include MIME headers), you can set the `charset` group/topic
    parameter to the required charset (see [Group Parameters](#Group-Parameters)).

`W M v` [¶](#index-W-M-v-_0028Summary_0029)
:   View all the MIME parts in the current article
    (`gnus-mime-view-all-parts`).

Relevant variables:

`gnus-ignored-mime-types` [¶](#index-gnus_002dignored_002dmime_002dtypes)
:   This is a list of regexps. MIME types that match a regexp from
    this list will be completely ignored by Gnus. The default value is
    `nil`.

    To have all Vcards be ignored, you’d say something like this:

    ```
    (setq gnus-ignored-mime-types
          '("text/x-vcard"))
    ```

`gnus-article-loose-mime` [¶](#index-gnus_002darticle_002dloose_002dmime)
:   If non-`nil`, Gnus won’t require the ‘`MIME-Version`’ header
    before interpreting the message as a MIME message. This helps
    when reading messages from certain broken mail user agents. The
    default is `t`.

`gnus-article-emulate-mime` [¶](#index-gnus_002darticle_002demulate_002dmime)
:   There are other, non-MIME encoding methods used. The most common
    is ‘`uuencode`’, but yEncode is also getting to be popular. If
    this variable is non-`nil`, Gnus will look in message bodies to
    see if it finds these encodings, and if so, it’ll run them through the
    Gnus MIME machinery. The default is `t`. Only
    single-part yEnc encoded attachments can be decoded. There’s no support
    for encoding in Gnus.

`gnus-unbuttonized-mime-types` [¶](#index-gnus_002dunbuttonized_002dmime_002dtypes)
:   This is a list of regexps. MIME types that match a regexp from
    this list won’t have MIME buttons inserted unless they aren’t
    displayed or this variable is overridden by
    `gnus-buttonized-mime-types`. The default value is
    `(".*/.*")`. This variable is only used when
    `gnus-inhibit-mime-unbuttonizing` is `nil`.

`gnus-buttonized-mime-types` [¶](#index-gnus_002dbuttonized_002dmime_002dtypes)
:   This is a list of regexps. MIME types that match a regexp from
    this list will have MIME buttons inserted unless they aren’t
    displayed. This variable overrides
    `gnus-unbuttonized-mime-types`. The default value is `nil`.
    This variable is only used when `gnus-inhibit-mime-unbuttonizing`
    is `nil`.

    E.g., to see security buttons but no other buttons, you could set this
    variable to `("multipart/signed")` and leave
    `gnus-unbuttonized-mime-types` at the default value.

    You could also add `"multipart/alternative"` to this list to
    display radio buttons that allow you to choose one of two media types
    those mails include. See also `mm-discouraged-alternatives`
    (see [Display Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Display-Customization) in The
    Emacs MIME Manual).

`gnus-inhibit-mime-unbuttonizing` [¶](#index-gnus_002dinhibit_002dmime_002dunbuttonizing)
:   If this is non-`nil`, then all MIME parts get buttons. The
    default value is `nil`.

`gnus-article-mime-part-function` [¶](#index-gnus_002darticle_002dmime_002dpart_002dfunction)
:   For each MIME part, this function will be called with the MIME
    handle as the parameter. The function is meant to be used to allow
    users to gather information from the article (e.g., add Vcard info to
    the bbdb database) or to do actions based on parts (e.g., automatically
    save all jpegs into some directory).

    Here’s an example function the does the latter:

    ```
    (defun my-save-all-jpeg-parts (handle)
      (when (equal (car (mm-handle-type handle)) "image/jpeg")
        (with-temp-buffer
          (insert (mm-get-part handle))
          (write-region (point-min) (point-max)
                        (read-file-name "Save jpeg to: ")))))
    (setq gnus-article-mime-part-function
          'my-save-all-jpeg-parts)
    ```

`gnus-mime-multipart-functions` [¶](#index-gnus_002dmime_002dmultipart_002dfunctions)
:   Alist of MIME multipart types and functions to handle them.

`gnus-mime-display-multipart-alternative-as-mixed` [¶](#index-gnus_002dmime_002ddisplay_002dmultipart_002dalternative_002das_002dmixed)
:   Display "multipart/alternative" parts as "multipart/mixed".

`gnus-mime-display-multipart-related-as-mixed` [¶](#index-gnus_002dmime_002ddisplay_002dmultipart_002drelated_002das_002dmixed)
:   Display "multipart/related" parts as "multipart/mixed".

    If displaying ‘`text/html`’ is discouraged, see
    `mm-discouraged-alternatives`, images or other material inside a
    "multipart/related" part might be overlooked when this variable is
    `nil`. [Display Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Display-Customization) in Emacs-Mime Manual.

`gnus-mime-display-multipart-as-mixed` [¶](#index-gnus_002dmime_002ddisplay_002dmultipart_002das_002dmixed)
:   Display "multipart" parts as "multipart/mixed". If `t`, it
    overrides `nil` values of
    `gnus-mime-display-multipart-alternative-as-mixed` and
    `gnus-mime-display-multipart-related-as-mixed`.

`mm-file-name-rewrite-functions` [¶](#index-mm_002dfile_002dname_002drewrite_002dfunctions)
:   List of functions used for rewriting file names of MIME parts.
    Each function takes a file name as input and returns a file name.

    Ready-made functions include  
    `mm-file-name-delete-whitespace`,
    `mm-file-name-trim-whitespace`,
    `mm-file-name-collapse-whitespace`, and
    `mm-file-name-replace-whitespace`. The later uses the value of
    the variable `mm-file-name-replace-whitespace` to replace each
    whitespace character in a file name with that string; default value
    is `"_"` (a single underscore).

    The standard functions `capitalize`, `downcase`,
    `upcase`, and `upcase-initials` may be useful, too.

    Everybody knows that whitespace characters in file names are evil,
    except those who don’t know. If you receive lots of attachments from
    such unenlightened users, you can make live easier by adding

    ```
    (setq mm-file-name-rewrite-functions
          '(mm-file-name-trim-whitespace
            mm-file-name-collapse-whitespace
            mm-file-name-replace-whitespace))
    ```

    to your `~/.gnus.el` file.

---

Next: [Article Commands](#Article-Commands), Previous: [MIME Commands](#MIME-Commands), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.20 Charsets [¶](#Charsets-1)

People use different charsets, and we have MIME to let us know what
charsets they use. Or rather, we wish we had. Many people use
newsreaders and mailers that do not understand or use MIME, and
just send out messages without saying what character sets they use. To
help a bit with this, some local news hierarchies have policies that say
what character set is the default. For instance, the ‘`fj`’
hierarchy uses `iso-2022-jp`.

This knowledge is encoded in the `gnus-group-charset-alist`
variable, which is an alist of regexps (use the first item to match full
group names) and default charsets to be used when reading these groups.

In addition, some people do use soi-disant MIME-aware agents that
aren’t. These blithely mark messages as being in `iso-8859-1`
even if they really are in `koi-8`. To help here, the
`gnus-newsgroup-ignored-charsets` variable can be used. The
charsets that are listed here will be ignored. The variable can be
set on a group-by-group basis using the group parameters (see [Group Parameters](#Group-Parameters)). The default value is `(unknown-8bit x-unknown)`,
which includes values some agents insist on having in there.

When posting, `gnus-group-posting-charset-alist` is used to
determine which charsets should not be encoded using the MIME
encodings. For instance, some hierarchies discourage using
quoted-printable header encoding.

This variable is an alist of regexps and permitted unencoded charsets
for posting. Each element of the alist has the form `(`test
header body-list`)`, where:

test
:   is either a regular expression matching the newsgroup header or a
    variable to query,

header
:   is the charset which may be left unencoded in the header (`nil`
    means encode all charsets),

body-list
:   is a list of charsets which may be encoded using 8bit content-transfer
    encoding in the body, or one of the special values `nil` (always
    encode using quoted-printable) or `t` (always use 8bit).

See [Encoding Customization](https://www.gnu.org/software/emacs/manual/html_mono/emacs-mime.html#Encoding-Customization) in The Emacs MIME Manual, for additional variables that control which
MIME charsets are used when sending messages.

Other charset tricks that may be useful, although not Gnus-specific:

If there are several MIME charsets that encode the same Emacs
charset, you can choose what charset to use by saying the following:

```
(put-charset-property 'cyrillic-iso8859-5
                      'preferred-coding-system 'koi8-r)
```

This means that Russian will be encoded using `koi8-r` instead of
the default `iso-8859-5` MIME charset.

If you want to read messages in `koi8-u`, you can cheat and say

```
(define-coding-system-alias 'koi8-u 'koi8-r)
```

This will almost do the right thing.

And finally, to read charsets like `windows-1251`, you can say
something like

```
(codepage-setup 1251)
(define-coding-system-alias 'windows-1251 'cp1251)
```

---

Next: [Summary Sorting](#Summary-Sorting), Previous: [Charsets](#Charsets), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.21 Article Commands [¶](#Article-Commands-1)

`A P` [¶](#index-PostScript-1)
:   Generate and print a PostScript image of the article buffer
    (`gnus-summary-print-article`). `gnus-ps-print-hook` will
    be run just before printing the buffer. An alternative way to print
    article is to use Muttprint (see [Saving Articles](#Saving-Articles)).

`A C` [¶](#index-gnus_002dfetch_002dpartial_002darticles)
:   If `<backend>-fetch-partial-articles` is non-`nil`, Gnus will
    fetch partial articles, if the backend it fetches them from supports
    it. Currently only `nnimap` does. If you’re looking at a
    partial article, and want to see the complete article instead, then
    the `A C` command (`gnus-summary-show-complete-article`) will
    do so.

`w` [¶](#index-w-_0028Summary_0029)

`A w`
:   Scan the article buffer for links, and offer them to the user for
    browsing with `browse-url`. With a prefix argument, browse with
    `browse-url-secondary-browser-function` instead.

---

Next: [Finding the Parent](#Finding-the-Parent), Previous: [Article Commands](#Article-Commands), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.22 Summary Sorting [¶](#Summary-Sorting-1)

You can have the summary buffer sorted in various ways, even though I
can’t really see why you’d want that.

`C-c C-s C-n` [¶](#index-C_002dc-C_002ds-C_002dn-_0028Summary_0029)
:   Sort by article number (`gnus-summary-sort-by-number`).

`C-c C-s C-m C-n` [¶](#index-C_002dc-C_002ds-C_002dn-_0028Summary_0029-1)
:   Sort by most recent article number
    (`gnus-summary-sort-by-most-recent-number`).

`C-c C-s C-a` [¶](#index-C_002dc-C_002ds-C_002da-_0028Summary_0029)
:   Sort by author (`gnus-summary-sort-by-author`).

`C-c C-s C-t` [¶](#index-C_002dc-C_002ds-C_002dt-_0028Summary_0029)
:   Sort by recipient (`gnus-summary-sort-by-recipient`).

`C-c C-s C-s` [¶](#index-C_002dc-C_002ds-C_002ds-_0028Summary_0029)
:   Sort by subject (`gnus-summary-sort-by-subject`).

`C-c C-s C-d` [¶](#index-C_002dc-C_002ds-C_002dd-_0028Summary_0029)
:   Sort by date (`gnus-summary-sort-by-date`).

`C-c C-s C-m C-d` [¶](#index-C_002dc-C_002ds-C_002dm-C_002dd-_0028Summary_0029)
:   Sort by most recent date (`gnus-summary-sort-by-most-recent-date`).

`C-c C-s C-l` [¶](#index-C_002dc-C_002ds-C_002dl-_0028Summary_0029)
:   Sort by lines (`gnus-summary-sort-by-lines`).

`C-c C-s C-c` [¶](#index-C_002dc-C_002ds-C_002dc-_0028Summary_0029)
:   Sort by article length (`gnus-summary-sort-by-chars`).

`C-c C-s C-m C-m` [¶](#index-C_002dc-C_002ds-C_002dm-C_002dm-_0028Summary_0029)
:   Sort by article “readedness” marks (`gnus-summary-sort-by-marks`).

`C-c C-s C-i` [¶](#index-C_002dc-C_002ds-C_002di-_0028Summary_0029)
:   Sort by score (`gnus-summary-sort-by-score`).

`C-c C-s C-u` [¶](#index-C_002dc-C_002ds-C_002du-_0028Summary_0029)
:   Sort by newsgroups (`gnus-summary-sort-by-newsgroups`).

`C-c C-s C-x` [¶](#index-C_002dc-C_002ds-C_002dx-_0028Summary_0029)
:   Prompts for extra header to sort by (`gnus-summary-sort-by-extra`).
    An error will be raised if no sort functions for the header are defined.

`C-c C-s C-r` [¶](#index-C_002dc-C_002ds-C_002dr-_0028Summary_0029)
:   Randomize (`gnus-summary-sort-by-random`).

`C-c C-s C-o` [¶](#index-C_002dc-C_002ds-C_002do-_0028Summary_0029)
:   Sort using the default sorting method
    (`gnus-summary-sort-by-original`).

These functions will work both when you use threading and when you don’t
use threading. In the latter case, all summary lines will be sorted,
line by line. In the former case, sorting will be done on a
root-by-root basis, which might not be what you were looking for. To
toggle whether to use threading, type `T T` (see [Thread Commands](#Thread-Commands)).

If a prefix argument if given, the sort order is reversed.

---

Next: [Alternative Approaches](#Alternative-Approaches), Previous: [Summary Sorting](#Summary-Sorting), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.23 Finding the Parent [¶](#Finding-the-Parent-1)

`^` [¶](#index-_005e-_0028Summary_0029)
:   If you’d like to read the parent of the current article, and it is not
    displayed in the summary buffer, you might still be able to. That is,
    if the current group is fetched by NNTP, the parent hasn’t expired
    and the `References` in the current article are not mangled, you
    can just press `^` or `A r`
    (`gnus-summary-refer-parent-article`). If everything goes well,
    you’ll get the parent. If the parent is already displayed in the
    summary buffer, point will just move to this article.

    If given a positive numerical prefix, fetch that many articles back into
    the ancestry. If given a negative numerical prefix, fetch just that
    ancestor. So if you say `3 ^`, Gnus will fetch the parent, the
    grandparent and the great-grandparent of the current article. If you say
    `-3 ^`, Gnus will only fetch the great-grandparent of the current
    article.

`A R (Summary)` [¶](#index-gnus_002dsummary_002drefer_002dreferences)
:   Fetch all articles mentioned in the `References` header of the
    article (`gnus-summary-refer-references`).

`A T (Summary)` [¶](#index-gnus_002dsummary_002drefer_002dthread)
:   Display the full thread where the current article appears
    (`gnus-summary-refer-thread`). By default this command looks for
    articles only in the current group. If the group belongs to a backend
    that has an associated search engine, articles are found by searching.
    In other cases each header in the current group must be fetched and
    examined, so it usually takes a while. If you do it often, you may
    consider setting `gnus-fetch-old-headers` to `invisible`
    (see [Filling In Threads](#Filling-In-Threads)). This won’t have any visible effects
    normally, but it’ll make this command work a whole lot faster. Of
    course, it’ll make group entry somewhat slow.

    If `gnus-refer-thread-use-search` is `nil` (the default)
    then thread-referral only looks for articles in the current group. If
    this variable is `t` the server to which the current group
    belongs is searched (provided that searching is available for the
    server’s backend). If this variable is a list of servers, each server
    in the list is searched.

    The `gnus-refer-thread-limit` variable says how many old (i.e.,
    articles before the first displayed in the current group) headers to
    fetch when referring a thread. The default is 500. If `t`, all
    the available headers will be fetched. This variable can be
    overridden by giving the `A T` command a numerical prefix.

    `gnus-summary-refer-thread` tries to add any articles it finds to
    the current summary buffer. (When `gnus-refer-thread-use-search`
    is true and the initial referral starts from a summary buffer for a
    non-virtual group this may not be possible. In this case a new
    summary buffer is created holding a virtual group with the result of
    the thread search.) If `gnus-refer-thread-limit-to-thread` is
    non-`nil` then the summary buffer will be limited to articles in the
    thread.

`M-^ (Summary)` [¶](#index-gnus_002dsummary_002drefer_002darticle)
:   You can also ask Gnus for an arbitrary article, no matter what group it
    belongs to. `M-^` (`gnus-summary-refer-article`) will ask you
    for a `Message-ID`, which is one of those long, hard-to-read
    thingies that look something like ‘`<38o6up$6f2@hymir.ifi.uio.no>`’.
    You have to get it all exactly right. No fuzzy searches, I’m afraid.

    Gnus looks for the `Message-ID` in the headers that have already
    been fetched, but also tries all the select methods specified by
    `gnus-refer-article-method` if it is not found.

If the group you are reading is located on a back end that does not
support fetching by `Message-ID` very well (like `nnspool`),
you can set `gnus-refer-article-method` to an NNTP method. It
would, perhaps, be best if the NNTP server you consult is the one
updating the spool you are reading from, but that’s not really
necessary.

It can also be a list of select methods, as well as the special symbol
`current`, which means to use the current select method. If it
is a list, Gnus will try all the methods in the list until it finds a
match.

Here’s an example setting that will first try the current method, and
then ask Google if that fails:

```
(setq gnus-refer-article-method
      '(current
        (nnweb "google" (nnweb-type google))))
```

Most of the mail back ends support fetching by `Message-ID`, but
do not do a particularly excellent job at it. That is, `nnmbox`,
`nnbabyl`, `nnmaildir`, `nnml`, are able to locate
articles from any groups, while `nnfolder`, and `nnimap` are
only able to locate articles that have been posted to the current
group. `nnmh` does not support this at all.

Fortunately, the special `nnregistry` back end is able to locate
articles in any groups, regardless of their back end (see [fetching by `Message-ID` using the
registry](#Registry-Article-Refer-Method)).

---

Next: [Tree Display](#Tree-Display), Previous: [Finding the Parent](#Finding-the-Parent), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.24 Alternative Approaches [¶](#Alternative-Approaches-1)

Different people like to read news using different methods. This being
Gnus, we offer a small selection of minor modes for the summary buffers.

* [Pick and Read](#Pick-and-Read)
* [Binary Groups](#Binary-Groups)

---

Next: [Binary Groups](#Binary-Groups), Up: [Alternative Approaches](#Alternative-Approaches)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.24.1 Pick and Read [¶](#Pick-and-Read-1)

Some newsreaders (like `nn` and, uhm, `Netnews` on VM/CMS) use
a two-phased reading interface. The user first marks in a summary
buffer the articles she wants to read. Then she starts reading the
articles with just an article buffer displayed.

Gnus provides a summary buffer minor mode that allows
this—`gnus-pick-mode`. This basically means that a few process
mark commands become one-keystroke commands to allow easy marking, and
it provides one additional command for switching to the summary buffer.

Here are the available keystrokes when using pick mode:

`.` [¶](#index-_002e-_0028Pick_0029)
:   Pick the article or thread on the current line or unpick it if is
    already picked (`gnus-pick-article-or-thread`). If the variable
    `gnus-thread-hide-subtree` is true, then this key selects the
    entire thread when used at the first article of the thread. Otherwise,
    it selects just the article. If given a numerical prefix, go to that
    thread or article and pick it. (The line number is normally displayed
    at the beginning of the summary pick lines.) If
    `gnus-process-mark-toggle` is `nil`, this key will pick an
    article or thread.

`SPC` [¶](#index-SPC-_0028Pick_0029)
:   Scroll the summary buffer up one page (`gnus-pick-next-page`). If
    at the end of the buffer, start reading the picked articles.

`u` [¶](#index-u-_0028Pick_0029)
:   Unpick the thread or article
    (`gnus-pick-unmark-article-or-thread`). If the variable
    `gnus-thread-hide-subtree` is true, then this key unpicks the
    thread if used at the first article of the thread. Otherwise it unpicks
    just the article. You can give this key a numerical prefix to unpick
    the thread or article at that line.

`RET` [¶](#index-RET-_0028Pick_0029)
:   Start reading the picked articles (`gnus-pick-start-reading`). If
    given a prefix, mark all unpicked articles as read first. If
    `gnus-pick-display-summary` is non-`nil`, the summary buffer
    will still be visible when you are reading.

All the normal summary mode commands are still available in the
pick-mode, with the exception of `u`. However `!` is available
which is mapped to the same function
`gnus-summary-tick-article-forward`.

If this sounds like a good idea to you, you could say:

```
(add-hook 'gnus-summary-mode-hook 'gnus-pick-mode)
```

`gnus-pick-mode-hook` is run in pick minor mode buffers.

If `gnus-mark-unpicked-articles-as-read` is non-`nil`, mark
all unpicked articles as read. The default is `nil`.

The summary line format in pick mode is slightly different from the
standard format. At the beginning of each line the line number is
displayed. The pick mode line format is controlled by the
`gnus-summary-pick-line-format` variable (see [Formatting Variables](#Formatting-Variables)). It accepts the same format specs that
`gnus-summary-line-format` does (see [Summary Buffer Lines](#Summary-Buffer-Lines)).

---

Previous: [Pick and Read](#Pick-and-Read), Up: [Alternative Approaches](#Alternative-Approaches)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 4.24.2 Binary Groups [¶](#Binary-Groups-1)

If you spend much time in binary groups, you may grow tired of hitting
`X u`, `n`, `RET` all the time. `M-x gnus-binary-mode`
is a minor mode for summary buffers that makes all ordinary Gnus article
selection functions uudecode series of articles and display the result
instead of just displaying the articles the normal way.

The only way, in fact, to see the actual articles is the `g`
command, when you have turned on this mode
(`gnus-binary-show-article`).

`gnus-binary-mode-hook` is called in binary minor mode buffers.

---

Next: [Mail Group Commands](#Mail-Group-Commands), Previous: [Alternative Approaches](#Alternative-Approaches), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.25 Tree Display [¶](#Tree-Display-1)

If you don’t like the normal Gnus summary display, you might try setting
`gnus-use-trees` to `t`. This will create (by default) an
additional *tree buffer*. You can execute all summary mode commands
in the tree buffer.

There are a few variables to customize the tree display, of course:

`gnus-tree-mode-hook` [¶](#index-gnus_002dtree_002dmode_002dhook)
:   A hook called in all tree mode buffers.

`gnus-tree-mode-line-format` [¶](#index-gnus_002dtree_002dmode_002dline_002dformat)
:   A format string for the mode bar in the tree mode buffers (see [Mode Line Formatting](#Mode-Line-Formatting)). The default is ‘`Gnus: %%b %S %Z`’. For a list
    of valid specs, see [Summary Buffer Mode Line](#Summary-Buffer-Mode-Line).

`gnus-selected-tree-face` [¶](#index-gnus_002dselected_002dtree_002dface)
:   Face used for highlighting the selected article in the tree buffer. The
    default is `modeline`.

`gnus-tree-line-format` [¶](#index-gnus_002dtree_002dline_002dformat)
:   A format string for the tree nodes. The name is a bit of a misnomer,
    though—it doesn’t define a line, but just the node. The default value
    is ‘`%(%[%3,3n%]%)`’, which displays the first three characters of
    the name of the poster. It is vital that all nodes are of the same
    length, so you *must* use ‘`%4,4n`’-like specifiers.

    Valid specs are:

    ‘`n`’
    :   The name of the poster.

    ‘`f`’
    :   The `From` header.

    ‘`N`’
    :   The number of the article.

    ‘`[`’
    :   The opening bracket.

    ‘`]`’
    :   The closing bracket.

    ‘`s`’
    :   The subject.

    See [Formatting Variables](#Formatting-Variables).

    Variables related to the display are:

    `gnus-tree-brackets` [¶](#index-gnus_002dtree_002dbrackets)
    :   This is used for differentiating between “real” articles and
        “sparse” articles. The format is

        ```
        ((real-open . real-close)
         (sparse-open . sparse-close)
         (dummy-open . dummy-close))
        ```

        and the default is `((?[ . ?]) (?( . ?)) (?{ . ?}) (?< . ?>))`.

    `gnus-tree-parent-child-edges` [¶](#index-gnus_002dtree_002dparent_002dchild_002dedges)
    :   This is a list that contains the characters used for connecting parent
        nodes to their children. The default is `(?- ?\\ ?|)`.

`gnus-tree-minimize-window` [¶](#index-gnus_002dtree_002dminimize_002dwindow)
:   If this variable is non-`nil`, Gnus will try to keep the tree
    buffer as small as possible to allow more room for the other Gnus
    windows. If this variable is a number, the tree buffer will never be
    higher than that number. The default is `t`. Note that if you
    have several windows displayed side-by-side in a frame and the tree
    buffer is one of these, minimizing the tree window will also resize all
    other windows displayed next to it.

    You may also wish to add the following hook to keep the window minimized
    at all times:

    ```
    (add-hook 'gnus-configure-windows-hook
              'gnus-tree-perhaps-minimize)
    ```

`gnus-generate-tree-function` [¶](#index-gnus_002dgenerate_002dtree_002dfunction)
:   The function that actually generates the thread tree. Two predefined
    functions are available: `gnus-generate-horizontal-tree` and
    `gnus-generate-vertical-tree` (which is the default).

Here’s an example from a horizontal tree buffer:

```
{***}-(***)-[odd]-[Gun]
     |      \[Jan]
     |      \[odd]-[Eri]
     |      \(***)-[Eri]
     |            \[odd]-[Paa]
     \[Bjo]
     \[Gun]
     \[Gun]-[Jor]
```

Here’s the same thread displayed in a vertical tree buffer:

```
{***}
  |--------------------------\-----\-----\
(***)                         [Bjo] [Gun] [Gun]
  |--\-----\-----\                          |
[odd] [Jan] [odd] (***)                   [Jor]
  |           |     |--\
[Gun]       [Eri] [Eri] [odd]
                          |
                        [Paa]
```

If you’re using horizontal trees, it might be nice to display the trees
side-by-side with the summary buffer. You could add something like the
following to your `~/.gnus.el` file:

```
(setq gnus-use-trees t
      gnus-generate-tree-function 'gnus-generate-horizontal-tree
      gnus-tree-minimize-window nil)
(gnus-add-configuration
 '(article
   (vertical 1.0
             (horizontal 0.25
                         (summary 0.75 point)
                         (tree 1.0))
             (article 1.0))))
```

See [Window Layout](#Window-Layout).

---

Next: [Various Summary Stuff](#Various-Summary-Stuff), Previous: [Tree Display](#Tree-Display), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.26 Mail Group Commands [¶](#Mail-Group-Commands-1)

Some commands only make sense in mail groups. If these commands are
invalid in the current group, they will raise a hell and let you know.

All these commands (except the expiry and edit commands) use the
process/prefix convention (see [Process/Prefix](#Process_002fPrefix)).

`B e` [¶](#index-B-e-_0028Summary_0029)
:   Run all expirable articles in the current group through the expiry
    process (`gnus-summary-expire-articles`). That is, delete all
    expirable articles in the group that have been around for a while.
    (see [Expiring Mail](#Expiring-Mail)).

`B C-M-e` [¶](#index-B-C_002dM_002de-_0028Summary_0029)
:   Delete all the expirable articles in the group
    (`gnus-summary-expire-articles-now`). This means that **all**
    articles eligible for expiry in the current group will
    disappear forever into that big `/dev/null` in the sky.

`B DEL` [¶](#index-B-DEL-_0028Summary_0029)
:   Delete the mail article. This is “delete” as in “delete it from your
    disk forever and ever, never to return again.” Use with caution.
    (`gnus-summary-delete-article`).

`B m` [¶](#index-B-m-_0028Summary_0029)
:   Move the article from one mail group to another
    (`gnus-summary-move-article`). Marks will be preserved if
    `gnus-preserve-marks` is non-`nil` (which is the default).

`B c` [¶](#index-B-c-_0028Summary_0029)
:   Copy the article from one group (mail group or not) to a mail group
    (`gnus-summary-copy-article`). Marks will be preserved if
    `gnus-preserve-marks` is non-`nil` (which is the default).

`B B` [¶](#index-B-B-_0028Summary_0029)
:   Crosspost the current article to some other group
    (`gnus-summary-crosspost-article`). This will create a new copy of
    the article in the other group, and the Xref headers of the article will
    be properly updated.

`B i` [¶](#index-B-i-_0028Summary_0029)
:   Import an arbitrary file into the current mail newsgroup
    (`gnus-summary-import-article`). You will be prompted for a file
    name, a `From` header and a `Subject` header.

`B I` [¶](#index-B-I-_0028Summary_0029)
:   Create an empty article in the current mail newsgroups
    (`gnus-summary-create-article`). You will be prompted for a
    `From` header and a `Subject` header.

`B r` [¶](#index-B-r-_0028Summary_0029)
:   Respool the mail article (`gnus-summary-respool-article`).
    `gnus-summary-respool-default-method` will be used as the default
    select method when respooling. This variable is `nil` by default,
    which means that the current group select method will be used instead.
    Marks will be preserved if `gnus-preserve-marks` is non-`nil`
    (which is the default).

`B w` [¶](#index-B-w-_0028Summary_0029)

`e`
:   Edit the current article (`gnus-summary-edit-article`). To finish
    editing and make the changes permanent, type `C-c C-c`
    (`gnus-summary-edit-article-done`). If you give a prefix to the
    `C-c C-c` command, Gnus won’t re-highlight the article.

`B q` [¶](#index-B-q-_0028Summary_0029)
:   If you want to re-spool an article, you might be curious as to what group
    the article will end up in before you do the re-spooling. This command
    will tell you (`gnus-summary-respool-query`).

`B t` [¶](#index-B-t-_0028Summary_0029)
:   Similarly, this command will display all fancy splitting patterns used
    when respooling, if any (`gnus-summary-respool-trace`).

`B p` [¶](#index-B-p-_0028Summary_0029)
:   Some people have a tendency to send you “courtesy” copies when they
    follow up to articles you have posted. These usually have a
    `Newsgroups` header in them, but not always. This command
    (`gnus-summary-article-posted-p`) will try to fetch the current
    article from your news server (or rather, from
    `gnus-refer-article-method` or `gnus-select-method`) and will
    report back whether it found the article or not. Even if it says that
    it didn’t find the article, it may have been posted anyway—mail
    propagation is much faster than news propagation, and the news copy may
    just not have arrived yet.

`K E` [¶](#index-K-E-_0028Summary_0029)
:   Encrypt the body of an article (`gnus-article-encrypt-body`).
    The body is encrypted with the encryption protocol specified by the
    variable `gnus-article-encrypt-protocol`.

If you move (or copy) articles regularly, you might wish to have Gnus
suggest where to put the articles. `gnus-move-split-methods` is a
variable that uses the same syntax as `gnus-split-methods`
(see [Saving Articles](#Saving-Articles)). You may customize that variable to create
suggestions you find reasonable. (Note that
`gnus-move-split-methods` uses group names where
`gnus-split-methods` uses file names.)

```
(setq gnus-move-split-methods
      '(("^From:.*Lars Magne" "nnml:junk")
        ("^Subject:.*gnus" "nnfolder:important")
        (".*" "nnml:misc")))
```

---

Next: [Exiting the Summary Buffer](#Exiting-the-Summary-Buffer), Previous: [Mail Group Commands](#Mail-Group-Commands), Up: [Summary Buffer](#Summary-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
