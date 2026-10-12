### 35.6 The Match Data [¶](#The-Match-Data)

Emacs keeps track of the start and end positions of the segments of
text found during a search; this is called the *match data*.
Thanks to the match data, you can search for a complex pattern, such
as a date in a mail message, and then extract parts of the match under
control of the pattern.

Because the match data normally describe the most recent search only,
you must be careful not to do another search inadvertently between the
search you wish to refer back to and the use of the match data. If you
can’t avoid another intervening search, you must save and restore the
match data around it, to prevent it from being overwritten.

Notice that all functions are allowed to overwrite the match data
unless they’re explicitly documented not to do so. A consequence is
that functions that are run implicitly in the background
(see [Timers for Delayed Execution](#Timers), and [Idle Timers](#Idle-Timers)) should likely save and restore
the match data explicitly.

* [Replacing the Text that Matched](#Replacing-Match)
* [Simple Match Data Access](#Simple-Match-Data)
* [Accessing the Entire Match Data](#Entire-Match-Data)
* [Saving and Restoring the Match Data](#Saving-Match-Data)

---

Next: [Simple Match Data Access](#Simple-Match-Data), Up: [The Match Data](#Match-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.6.1 Replacing the Text that Matched [¶](#Replacing-the-Text-that-Matched)

This function replaces all or part of the text matched by the last
search. It works by means of the match data.

Function: **replace-match** replacement &optional fixedcase literal string subexp [¶](#index-replace_002dmatch)
:   This function performs a replacement operation on a buffer or string.

    If you did the last search in a buffer, you should omit the
    string argument or specify `nil` for it, and make sure that
    the current buffer is the one in which you performed the last search.
    Then this function edits the buffer, replacing the matched text with
    replacement. It leaves point at the end of the replacement
    text.

    If you performed the last search on a string, pass the same string as
    string. Then this function returns a new string, in which the
    matched text is replaced by replacement.

    If fixedcase is non-`nil`, then `replace-match` uses
    the replacement text without case conversion; otherwise, it converts
    the replacement text depending upon the capitalization of the text to
    be replaced. If the original text is all upper case, this converts
    the replacement text to upper case. If all words of the original text
    are capitalized, this capitalizes all the words of the replacement
    text. If all the words are one-letter and they are all upper case,
    they are treated as capitalized words rather than all-upper-case
    words.

    If literal is non-`nil`, then replacement is inserted
    exactly as it is, the only alterations being case changes as needed.
    If it is `nil` (the default), then the character ‘`\`’ is treated
    specially. If a ‘`\`’ appears in replacement, then it must be
    part of one of the following sequences:

    ‘`\&`’ [¶](#index-_0026-in-replacement)
    :   This stands for the entire text being replaced.

    ‘`\n`’, where n is a digit [¶](#index-_005cn-in-replacement)
    :   This stands for the text that matched the nth subexpression in
        the original regexp. Subexpressions are those expressions grouped
        inside ‘`\(…\)`’. If the nth subexpression never
        matched, an empty string is substituted.

    ‘`\\`’ [¶](#index-_005c-in-replacement)
    :   This stands for a single ‘`\`’ in the replacement text.

    ‘`\?`’
    :   This stands for itself (for compatibility with `replace-regexp`
        and related commands; see [Regexp Replace](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexp-Replace) in The GNU
        Emacs Manual).

    Any other character following ‘`\`’ signals an error.

    The substitutions performed by ‘`\&`’ and ‘`\n`’ occur
    after case conversion, if any. Therefore, the strings they substitute
    are never case-converted.

    If subexp is non-`nil`, that says to replace just
    subexpression number subexp of the regexp that was matched, not
    the entire match. For example, after matching ‘`foo \(ba*r\)`’,
    calling `replace-match` with 1 as subexp means to replace
    just the text that matched ‘`\(ba*r\)`’.

Function: **match-substitute-replacement** replacement &optional fixedcase literal string subexp [¶](#index-match_002dsubstitute_002dreplacement)
:   This function returns the text that would be inserted into the buffer
    by `replace-match`, but without modifying the buffer. It is
    useful if you want to present the user with actual replacement result,
    with constructs like ‘`\n`’ or ‘`\&`’ substituted with
    matched groups. Arguments replacement and optional
    fixedcase, literal, string and subexp have the
    same meaning as for `replace-match`.

---

Next: [Accessing the Entire Match Data](#Entire-Match-Data), Previous: [Replacing the Text that Matched](#Replacing-Match), Up: [The Match Data](#Match-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.6.2 Simple Match Data Access [¶](#Simple-Match-Data-Access)

This section explains how to use the match data to find out what was
matched by the last search or match operation, if it succeeded.

You can ask about the entire matching text, or about a particular
parenthetical subexpression of a regular expression. The count
argument in the functions below specifies which. If count is
zero, you are asking about the entire match. If count is
positive, it specifies which subexpression you want.

Recall that the subexpressions of a regular expression are those
expressions grouped with escaped parentheses, ‘`\(…\)`’. The
countth subexpression is found by counting occurrences of
‘`\(`’ from the beginning of the whole regular expression. The first
subexpression is numbered 1, the second 2, and so on. Only regular
expressions can have subexpressions—after a simple string search, the
only information available is about the entire match.

Every successful search sets the match data. Therefore, you should
query the match data immediately after searching, before calling any
other function that might perform another search. Alternatively, you
may save and restore the match data (see [Saving and Restoring the Match Data](#Saving-Match-Data)) around
the call to functions that could perform another search. Or use the
functions that explicitly do not modify the match data;
e.g., `string-match-p`.

A search which fails may or may not alter the match data. In the
current implementation, it does not, but we may change it in the
future. Don’t try to rely on the value of the match data after a
failing search.

Function: **match-string** count &optional in-string [¶](#index-match_002dstring)
:   This function returns, as a string, the text matched in the last search
    or match operation. It returns the entire text if count is zero,
    or just the portion corresponding to the countth parenthetical
    subexpression, if count is positive.

    If the last such operation was done against a string with
    `string-match`, then you should pass the same string as the
    argument in-string. After a buffer search or match,
    you should omit in-string or pass `nil` for it; but you
    should make sure that the current buffer when you call
    `match-string` is the one in which you did the searching or
    matching. Failure to follow this advice will lead to incorrect results.

    The value is `nil` if count is out of range, or for a
    subexpression inside a ‘`\|`’ alternative that wasn’t used or a
    repetition that repeated zero times.

Function: **match-string-no-properties** count &optional in-string [¶](#index-match_002dstring_002dno_002dproperties)
:   This function is like `match-string` except that the result
    has no text properties.

Function: **match-beginning** count [¶](#index-match_002dbeginning)
:   If the last regular expression search found a match, this function
    returns the position of the start of the matching text or of a
    subexpression of it.

    If count is zero, then the value is the position of the start of
    the entire match. Otherwise, count specifies a subexpression in
    the regular expression, and the value of the function is the starting
    position of the match for that subexpression.

    The value is `nil` for a subexpression inside a ‘`\|`’
    alternative that wasn’t used or a repetition that repeated zero times.

Function: **match-end** count [¶](#index-match_002dend)
:   This function is like `match-beginning` except that it returns the
    position of the end of the match, rather than the position of the
    beginning.

Here is an example of using the match data, with a comment showing the
positions within the text:

```
(string-match "\\(qu\\)\\(ick\\)"
              "The quick fox jumped quickly.")
              ;0123456789
     ⇒ 4
```

```

```

```
(match-string 0 "The quick fox jumped quickly.")
     ⇒ "quick"
(match-string 1 "The quick fox jumped quickly.")
     ⇒ "qu"
(match-string 2 "The quick fox jumped quickly.")
     ⇒ "ick"
```

```

```

```
(match-beginning 1)       ; The beginning of the match
     ⇒ 4                 ;   with ‘qu’ is at index 4.
```

```

```

```
(match-beginning 2)       ; The beginning of the match
     ⇒ 6                 ;   with ‘ick’ is at index 6.
```

```

```

```
(match-end 1)             ; The end of the match
     ⇒ 6                 ;   with ‘qu’ is at index 6.

(match-end 2)             ; The end of the match
     ⇒ 9                 ;   with ‘ick’ is at index 9.
```

Here is another example. Point is initially located at the beginning
of the line. Searching moves point to between the space and the word
‘`in`’. The beginning of the entire match is at the 9th character of
the buffer (‘`T`’), and the beginning of the match for the first
subexpression is at the 13th character (‘`c`’).

```
(list
  (re-search-forward "The \\(cat \\)")
  (match-beginning 0)
  (match-beginning 1))
    ⇒ (17 9 13)
```

```

```

```
---------- Buffer: foo ----------
I read "The cat ∗in the hat comes back" twice.
        ^   ^
        9  13
---------- Buffer: foo ----------
```

(In this case, the index returned is a buffer position; the first
character of the buffer counts as 1.)

---

Next: [Saving and Restoring the Match Data](#Saving-Match-Data), Previous: [Simple Match Data Access](#Simple-Match-Data), Up: [The Match Data](#Match-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.6.3 Accessing the Entire Match Data [¶](#Accessing-the-Entire-Match-Data)

The functions `match-data` and `set-match-data` read or
write the entire match data, all at once.

Function: **match-data** &optional integers reuse reseat [¶](#index-match_002ddata)
:   This function returns a list of positions (markers or integers) that
    record all the information on the text that the last search matched.
    Element zero is the position of the beginning of the match for the
    whole expression; element one is the position of the end of the match
    for the expression. The next two elements are the positions of the
    beginning and end of the match for the first subexpression, and so on.
    In general, element
    number 2n
    corresponds to `(match-beginning n)`; and
    element
    number 2n + 1
    corresponds to `(match-end n)`.

    Normally all the elements are markers or `nil`, but if
    integers is non-`nil`, that means to use integers instead
    of markers. (In that case, the buffer itself is appended as an
    additional element at the end of the list, to facilitate complete
    restoration of the match data.) If the last match was done on a
    string with `string-match`, then integers are always used,
    since markers can’t point into a string.

    If reuse is non-`nil`, it should be a list. In that case,
    `match-data` stores the match data in reuse. That is,
    reuse is destructively modified. reuse does not need to
    have the right length. If it is not long enough to contain the match
    data, it is extended. If it is too long, the length of reuse
    stays the same, but the elements that were not used are set to
    `nil`. The purpose of this feature is to reduce the need for
    garbage collection.

    If reseat is non-`nil`, all markers on the reuse list
    are reseated to point to nowhere.

    As always, there must be no possibility of intervening searches between
    the call to a search function and the call to `match-data` that is
    intended to access the match data for that search.

    ```
    (match-data)
         ⇒  (#<marker at 9 in foo>
              #<marker at 17 in foo>
              #<marker at 13 in foo>
              #<marker at 17 in foo>)
    ```

Function: **set-match-data** match-list &optional reseat [¶](#index-set_002dmatch_002ddata)
:   This function sets the match data from the elements of match-list,
    which should be a list that was the value of a previous call to
    `match-data`. (More precisely, anything that has the same format
    will work.)

    If match-list refers to a buffer that doesn’t exist, you don’t get
    an error; that sets the match data in a meaningless but harmless way.

    If reseat is non-`nil`, all markers on the match-list list
    are reseated to point to nowhere.

    `store-match-data` is a semi-obsolete alias for `set-match-data`.

---

Previous: [Accessing the Entire Match Data](#Entire-Match-Data), Up: [The Match Data](#Match-Data)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.6.4 Saving and Restoring the Match Data [¶](#Saving-and-Restoring-the-Match-Data)

When you call a function that may search, you may need to save
and restore the match data around that call, if you want to preserve the
match data from an earlier search for later use. Here is an example
that shows the problem that arises if you fail to save the match data:

```
(re-search-forward "The \\(cat \\)")
     ⇒ 48
(foo)                   ; foo does more searching.
(match-end 0)
     ⇒ 61              ; Unexpected result—not 48!
```

You can save and restore the match data with `save-match-data`:

Macro: **save-match-data** body… [¶](#index-save_002dmatch_002ddata)
:   This macro executes body, saving and restoring the match
    data around it. The return value is the value of the last form in
    body.

You could use `set-match-data` together with `match-data` to
imitate the effect of the special form `save-match-data`. Here is
how:

```
(let ((data (match-data)))
  (unwind-protect
      ...   ; Ok to change the original match data.
    (set-match-data data)))
```

Emacs automatically saves and restores the match data when it runs
process filter functions (see [Process Filter Functions](#Filter-Functions)) and process
sentinels (see [Sentinels: Detecting Process Status Changes](#Sentinels)).

---

Next: [Standard Regular Expressions Used in Editing](#Standard-Regexps), Previous: [The Match Data](#Match-Data), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 35.7 Search and Replace [¶](#Search-and-Replace-1)

If you want to find all matches for a regexp in part of the buffer
and replace them, the most flexible way is to write an explicit loop
using `re-search-forward` and `replace-match`, like this:

```
(while (re-search-forward "foo[ \t]+bar" nil t)
  (replace-match "foobar"))
```

See [Replacing the Text that Matched](#Replacing-Match), for a
description of `replace-match`.

It may be more convenient to limit the replacements to a specific
region. The function `replace-regexp-in-region` does that.

Function: **replace-regexp-in-region** regexp replacement &optional start end [¶](#index-replace_002dregexp_002din_002dregion)
:   This function replaces all the occurrences of regexp with
    replacement in the region of buffer text between start and
    end; start defaults to position of point, and end
    defaults to the last accessible position of the buffer. The search
    for regexp is case-sensitive, and replacement is inserted
    without changing its letter-case. The replacement string can
    use the same special elements starting with ‘`\`’ as
    `replace-match` does. The function returns the number of
    replaced occurrences, or `nil` if regexp is not found. The
    function preserves the position of point.

    ```
    (replace-regexp-in-region "foo[ \t]+bar" "foobar")
    ```

Function: **replace-string-in-region** string replacement &optional start end [¶](#index-replace_002dstring_002din_002dregion)
:   This function works similarly to `replace-regexp-in-region`,
    but searches for, and replaces, literal strings instead of
    regular expressions.

Emacs also has special functions for replacing matches in a string.

Function: **replace-regexp-in-string** regexp rep string &optional fixedcase literal subexp start [¶](#index-replace_002dregexp_002din_002dstring)
:   This function copies string and searches it for matches for
    regexp, and replaces them with rep. It returns the
    modified copy. If start is non-`nil`, the search for
    matches starts at that index in string, and the returned value
    does not include the first start characters of string.
    To get the whole transformed string, concatenate the first
    start characters of string with the return value.

    This function uses `replace-match` to do the replacement, and it
    passes the optional arguments fixedcase, literal and
    subexp along to `replace-match`.

    Instead of a string, rep can be a function. In that case,
    `replace-regexp-in-string` calls rep for each match,
    passing the text of the match as its sole argument. It collects the
    value rep returns and passes that to `replace-match` as the
    replacement string. The match data at this point are the result
    of matching regexp against a substring of string.

Function: **string-replace** from-string to-string in-string [¶](#index-string_002dreplace)
:   This function replaces all occurrences of from-string with
    to-string in in-string and returns the result. It may
    return one of its arguments unchanged, a constant string or a new
    string. Case is significant, and text properties are ignored.

If you want to write a command along the lines of `query-replace`,
you can use `perform-replace` to do the work.

Function: **perform-replace** from-string replacements query-flag regexp-flag delimited-flag &optional repeat-count map start end backward region-noncontiguous-p [¶](#index-perform_002dreplace)
:   This function is the guts of `query-replace` and related
    commands. It searches for occurrences of from-string in the
    text between positions start and end and replaces some or
    all of them. If start is `nil` (or omitted), point is used
    instead, and the end of the buffer’s accessible portion is used for
    end. (If the optional argument backward is
    non-`nil`, the search starts at end and goes backward.)

    If query-flag is `nil`, it replaces all
    occurrences; otherwise, it asks the user what to do about each one.

    If regexp-flag is non-`nil`, then from-string is
    considered a regular expression; otherwise, it must match literally. If
    delimited-flag is non-`nil`, then only replacements
    surrounded by word boundaries are considered.

    The argument replacements specifies what to replace occurrences
    with. If it is a string, that string is used. It can also be a list of
    strings, to be used in cyclic order.

    If replacements is a cons cell, `(function . data)`, this means to call function after each match to
    get the replacement text. This function is called with two arguments:
    data, and the number of replacements already made.

    If repeat-count is non-`nil`, it should be an integer. Then
    it specifies how many times to use each of the strings in the
    replacements list before advancing cyclically to the next one.

    If from-string contains upper-case letters, then
    `perform-replace` binds `case-fold-search` to `nil`, and
    it uses the replacements without altering their case.

    Normally, the keymap `query-replace-map` defines the possible
    user responses for queries. The argument map, if
    non-`nil`, specifies a keymap to use instead of
    `query-replace-map`.

    Non-`nil` region-noncontiguous-p means that the region
    between start and end is composed of noncontiguous pieces.
    The most common example of this is a rectangular region, where the
    pieces are separated by newline characters.

    This function uses one of two functions to search for the next
    occurrence of from-string. These functions are specified by the
    values of two variables: `replace-re-search-function` and
    `replace-search-function`. The former is called when the
    argument regexp-flag is non-`nil`, the latter when it is
    `nil`.

Variable: **query-replace-map** [¶](#index-query_002dreplace_002dmap)
:   This variable holds a special keymap that defines the valid user
    responses for `perform-replace` and the commands that use it, as
    well as `y-or-n-p` and `map-y-or-n-p`. This map is unusual
    in two ways:

    * The key bindings are not commands, just symbols that are meaningful
      to the functions that use this map.
    * Prefix keys are not supported; each key binding must be for a
      single-event key sequence. This is because the functions don’t use
      `read-key-sequence` to get the input; instead, they read a single
      event and look it up “by hand”.

Here are the meaningful bindings for `query-replace-map`.
Several of them are meaningful only for `query-replace` and
friends.

`act`
:   Do take the action being considered—in other words, “yes”.

`skip`
:   Do not take action for this question—in other words, “no”.

`exit`
:   Answer this question “no”, and give up on the entire series of
    questions, assuming that the answers will be “no”.

`exit-prefix`
:   Like `exit`, but add the key that was pressed to
    `unread-command-events` (see [Miscellaneous Event Input Features](#Event-Input-Misc)).

`act-and-exit`
:   Answer this question “yes”, and give up on the entire series of
    questions, assuming that subsequent answers will be “no”.

`act-and-show`
:   Answer this question “yes”, but show the results—don’t advance yet
    to the next question.

`automatic`
:   Answer this question and all subsequent questions in the series with
    “yes”, without further user interaction.

`backup`
:   Move back to the previous place that a question was asked about.

`undo`
:   Undo last replacement and move back to the place where that
    replacement was performed.

`undo-all`
:   Undo all replacements and move back to the place where the first
    replacement was performed.

`edit`
:   Enter a recursive edit to deal with this question—instead of any
    other action that would normally be taken.

`edit-replacement`
:   Edit the replacement for this question in the minibuffer.

`delete-and-edit`
:   Delete the text being considered, then enter a recursive edit to replace
    it.

`recenter`

`scroll-up`

`scroll-down`

`scroll-other-window`

`scroll-other-window-down`
:   Perform the specified window scroll operation, then ask the same
    question again. Only `y-or-n-p` and related functions use this
    answer.

`quit`
:   Perform a quit right away. Only `y-or-n-p` and related functions
    use this answer.

`help`
:   Display some help, then ask again.

Variable: **multi-query-replace-map** [¶](#index-multi_002dquery_002dreplace_002dmap)
:   This variable holds a keymap that extends `query-replace-map` by
    providing additional key bindings that are useful in multi-buffer
    replacements. The additional bindings are:

    `automatic-all`
    :   Answer this question and all subsequent questions in the series with
        “yes”, without further user interaction, for all remaining buffers.

    `exit-current`
    :   Answer this question “no”, and give up on the entire series of
        questions for the current buffer. Continue to the next buffer in the
        sequence.

Variable: **replace-search-function** [¶](#index-replace_002dsearch_002dfunction)
:   This variable specifies a function that `perform-replace` calls
    to search for the next string to replace. Its default value is
    `search-forward`. Any other value should name a function of 3
    arguments: the first 3 arguments of `search-forward`
    (see [Searching for Strings](#String-Search)).

Variable: **replace-re-search-function** [¶](#index-replace_002dre_002dsearch_002dfunction)
:   This variable specifies a function that `perform-replace` calls
    to search for the next regexp to replace. Its default value is
    `re-search-forward`. Any other value should name a function of 3
    arguments: the first 3 arguments of `re-search-forward`
    (see [Regular Expression Searching](#Regexp-Search)).

---

Next: [Emacs versus POSIX Regular Expressions](#POSIX-Regexps), Previous: [Search and Replace](#Search-and-Replace), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 35.8 Standard Regular Expressions Used in Editing [¶](#Standard-Regular-Expressions-Used-in-Editing)

This section describes some variables that hold regular expressions
used for certain purposes in editing:

User Option: **page-delimiter** [¶](#index-page_002ddelimiter)
:   This is the regular expression describing line-beginnings that separate
    pages. The default value is `"^\014"` (i.e., `"^^L"` or
    `"^\C-l"`); this matches a line that starts with a formfeed
    character.

The following two regular expressions should *not* assume the
match always starts at the beginning of a line; they should not use
‘`^`’ to anchor the match. Most often, the paragraph commands do
check for a match only at the beginning of a line, which means that
‘`^`’ would be superfluous. When there is a nonzero left margin,
they accept matches that start after the left margin. In that case, a
‘`^`’ would be incorrect. However, a ‘`^`’ is harmless in modes
where a left margin is never used.

User Option: **paragraph-separate** [¶](#index-paragraph_002dseparate)
:   This is the regular expression for recognizing the beginning of a line
    that separates paragraphs. (If you change this, you may have to
    change `paragraph-start` also.) The default value is
    `"[ \t\f]*$"`, which matches a line that consists entirely of
    spaces, tabs, and form feeds (after its left margin).

User Option: **paragraph-start** [¶](#index-paragraph_002dstart)
:   This is the regular expression for recognizing the beginning of a line
    that starts *or* separates paragraphs. The default value is
    `"\f\\|[ \t]*$"`, which matches a line containing only
    whitespace or starting with a form feed (after its left margin).

User Option: **sentence-end** [¶](#index-sentence_002dend-1)
:   If non-`nil`, the value should be a regular expression describing
    the end of a sentence, including the whitespace following the
    sentence. (All paragraph boundaries also end sentences, regardless.)

    If the value is `nil`, as it is by default, then the function
    `sentence-end` constructs the regexp. That is why you
    should always call the function `sentence-end` to obtain the
    regexp to be used to recognize the end of a sentence.

Function: **sentence-end** [¶](#index-sentence_002dend)
:   This function returns the value of the variable `sentence-end`,
    if non-`nil`. Otherwise it returns a default value based on the
    values of the variables `sentence-end-double-space`
    (see [Definition of sentence-end-double-space](#Definition-of-sentence_002dend_002ddouble_002dspace)),
    `sentence-end-without-period`, and
    `sentence-end-without-space`.

---

Previous: [Standard Regular Expressions Used in Editing](#Standard-Regexps), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 35.9 Emacs versus POSIX Regular Expressions [¶](#Emacs-versus-POSIX-Regular-Expressions)

Regular expression syntax varies significantly among computer programs.
When writing Elisp code that generates regular expressions for use by other
programs, it is helpful to know how syntax variants differ.
To give a feel for the variation, this section discusses how
Emacs regular expressions differ from two syntax variants standarded by POSIX:
basic regular expressions (BREs) and extended regular expressions (EREs).
Plain `grep` uses BREs, and ‘`grep -E`’ uses EREs.

Emacs regular expressions have a syntax closer to EREs than to BREs,
with some extensions. Here is a summary of how POSIX BREs and EREs
differ from Emacs regular expressions.

* In POSIX BREs ‘`+`’ and ‘`?`’ are not special.
  The only backslash escape sequences are ‘`\(…\)`’,
  ‘`\{…\}`’, ‘`\1`’ through ‘`\9`’, along with the
  escaped special characters ‘`\$`’, ‘`\*`’, ‘`\.`’, ‘`\[`’,
  ‘`\\`’, and ‘`\^`’.
  Therefore ‘`\(?:`’ acts like ‘`\([?]:`’.
  POSIX does not define how other BRE escapes behave;
  for example, GNU `grep` treats ‘`\|`’ like Emacs does,
  but does not support all the Emacs escapes.
* In POSIX BREs, it is an implementation option whether ‘`^`’ is special
  after ‘`\(`’; GNU `grep` treats it like Emacs does.
  In POSIX EREs, ‘`^`’ is always special outside of bracket expressions,
  which means the ERE ‘`x^`’ never matches.
  In Emacs regular expressions, ‘`^`’ is special only at the
  beginning of the regular expression, or after ‘`\(`’, ‘`\(?:`’
  or ‘`\|`’.
* In POSIX BREs, it is an implementation option whether ‘`$`’ is
  special before ‘`\)`’; GNU `grep` treats it like Emacs
  does. In POSIX EREs, ‘`$`’ is always special outside of bracket
  expressions (see [bracket expressions](#Regexp-Special)), which means
  the ERE ‘`$x`’ never matches. In Emacs regular expressions,
  ‘`$`’ is special only at the end of the regular expression, or
  before ‘`\)`’ or ‘`\|`’.
* In POSIX EREs ‘`{`’, ‘`(`’ and ‘`|`’ are special,
  and ‘`)`’ is special when matched with a preceding ‘`(`’.
  These special characters do not use preceding backslashes;
  ‘`(?`’ produces undefined results.
  The only backslash escape sequences are the escaped special characters
  ‘`\$`’, ‘`\(`’, ‘`\)`’, ‘`\*`’, ‘`\+`’, ‘`\.`’,
  ‘`\?`’, ‘`\[`’, ‘`\\`’, ‘`\^`’, ‘`\{`’ and ‘`\|`’.
  POSIX does not define how other ERE escapes behave;
  for example, GNU ‘`grep -E`’ treats ‘`\1`’ like Emacs does,
  but does not support all the Emacs escapes.
* In POSIX BREs and EREs, undefined results are produced by repetition
  operators at the start of a regular expression or subexpression
  (possibly preceded by ‘`^`’), except that the repetition operator
  ‘`*`’ has the same behavior in BREs as in Emacs.
  In Emacs, these operators are treated as ordinary.
* In BREs and EREs, undefined results are produced by two repetition
  operators in sequence. In Emacs, these have well-defined behavior,
  e.g., ‘`a**`’ is equivalent to ‘`a*`’.
* In BREs and EREs, undefined results are produced by empty regular
  expressions or subexpressions. In Emacs these have well-defined
  behavior, e.g., ‘`\(\)*`’ matches the empty string,
* In BREs and EREs, undefined results are produced for the named
  character classes ‘`[:ascii:]`’, ‘`[:multibyte:]`’,
  ‘`[:nonascii:]`’, ‘`[:unibyte:]`’, and ‘`[:word:]`’.
* BREs and EREs can contain collating symbols and equivalence
  class expressions within bracket expressions, e.g., ‘`[[.ch.]d[=a=]]`’.
  Emacs regular expressions do not support this.
* BREs, EREs, and the strings they match cannot contain encoding errors
  or NUL bytes. In Emacs these constructs simply match themselves.
* BRE and ERE searching always finds the longest match.
  Emacs searching by default does not necessarily do so.
  See [Longest-match searching for regular expression matches](#Longest-Match).

---

Next: [Parsing Expression Grammars](#Parsing-Expression-Grammars), Previous: [Searching and Matching](#Searching-and-Matching), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 36 Syntax Tables [¶](#Syntax-Tables-1)

A *syntax table* specifies the syntactic role of each character
in a buffer. It can be used to determine where words, symbols, and
other syntactic constructs begin and end. This information is used by
many Emacs facilities, including Font Lock mode (see [Font Lock Mode](#Font-Lock-Mode)) and the various complex movement commands (see [Motion](#Motion)).

* [Syntax Table Concepts](#Syntax-Basics)
* [Syntax Descriptors](#Syntax-Descriptors)
* [Syntax Table Functions](#Syntax-Table-Functions)
* [Syntax Properties](#Syntax-Properties)
* [Motion and Syntax](#Motion-and-Syntax)
* [Parsing Expressions](#Parsing-Expressions)
* [Syntax Table Internals](#Syntax-Table-Internals)
* [Categories](#Categories)

---

Next: [Syntax Descriptors](#Syntax-Descriptors), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 36.1 Syntax Table Concepts [¶](#Syntax-Table-Concepts)

A syntax table is a data structure which can be used to look up the
*syntax class* and other syntactic properties of each character.
Syntax tables are used by Lisp programs for scanning and moving across
text.

Internally, a syntax table is a char-table (see [Char-Tables](#Char_002dTables)).
The element at index c describes the character with code
c; its value is a cons cell which specifies the syntax of the
character in question. See [Syntax Table Internals](#Syntax-Table-Internals), for details.
However, instead of using `aset` and `aref` to modify and
inspect syntax table contents, you should usually use the higher-level
functions `char-syntax` and `modify-syntax-entry`, which are
described in [Syntax Table Functions](#Syntax-Table-Functions).

Function: **syntax-table-p** object [¶](#index-syntax_002dtable_002dp)
:   This function returns `t` if object is a syntax table.

Each buffer has its own major mode, and each major mode has its own
idea of the syntax class of various characters. For example, in Lisp
mode, the character ‘`;`’ begins a comment, but in C mode, it
terminates a statement. To support these variations, the syntax table
is local to each buffer. Typically, each major mode has its own
syntax table, which it installs in all buffers that use that mode.
For example, the variable `emacs-lisp-mode-syntax-table` holds
the syntax table used by Emacs Lisp mode, and
`c-mode-syntax-table` holds the syntax table used by C mode.
Changing a major mode’s syntax table alters the syntax in all of that
mode’s buffers, as well as in any buffers subsequently put in that
mode. Occasionally, several similar modes share one syntax table.
See [Major Mode Examples](#Example-Major-Modes), for an example of how to set up a syntax
table.

A syntax table can *inherit* from another syntax table, which is
called its *parent syntax table*. A syntax table can leave the
syntax class of some characters unspecified, by giving them the
“inherit” syntax class; such a character then acquires the syntax
class specified by the parent syntax table (see [Table of Syntax Classes](#Syntax-Class-Table)). Emacs defines a *standard syntax table*, which is the
default parent syntax table, and is also the syntax table used by
Fundamental mode.

Function: **standard-syntax-table** [¶](#index-standard_002dsyntax_002dtable)
:   This function returns the standard syntax table, which is the syntax
    table used in Fundamental mode.

Syntax tables are not used by the Emacs Lisp reader, which has its
own built-in syntactic rules which cannot be changed. (Some Lisp
systems provide ways to redefine the read syntax, but we decided to
leave this feature out of Emacs Lisp for simplicity.)

---

Next: [Syntax Table Functions](#Syntax-Table-Functions), Previous: [Syntax Table Concepts](#Syntax-Basics), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 36.2 Syntax Descriptors [¶](#Syntax-Descriptors-1)

The *syntax class* of a character describes its syntactic role.
Each syntax table specifies the syntax class of each character. There
is no necessary relationship between the class of a character in one
syntax table and its class in any other table.

Each syntax class is designated by a mnemonic character, which
serves as the name of the class when you need to specify a class.
Usually, this designator character is one that is often assigned that
class; however, its meaning as a designator is unvarying and
independent of what syntax that character currently has. Thus,
‘`\`’ as a designator character always stands for escape character
syntax, regardless of whether the ‘`\`’ character actually has that
syntax in the current syntax table.
See [Table of Syntax Classes](#Syntax-Class-Table), for a list of syntax classes and their
designator characters.

A *syntax descriptor* is a Lisp string that describes the syntax
class and other syntactic properties of a character. When you want to
modify the syntax of a character, that is done by calling the function
`modify-syntax-entry` and passing a syntax descriptor as one of
its arguments (see [Syntax Table Functions](#Syntax-Table-Functions)).

The first character in a syntax descriptor must be a syntax class
designator character. The second character, if present, specifies a
matching character (e.g., in Lisp, the matching character for
‘`(`’ is ‘`)`’); a space specifies that there is no matching
character. Then come characters specifying additional syntax
properties (see [Syntax Flags](#Syntax-Flags)).

If no matching character or flags are needed, only one character
(specifying the syntax class) is sufficient.

For example, the syntax descriptor for the character ‘`*`’ in C
mode is `". 23"` (i.e., punctuation, matching character slot
unused, second character of a comment-starter, first character of a
comment-ender), and the entry for ‘`/`’ is ‘`. 14`’ (i.e.,
punctuation, matching character slot unused, first character of a
comment-starter, second character of a comment-ender).

Emacs also defines *raw syntax descriptors*, which are used to
describe syntax classes at a lower level. See [Syntax Table Internals](#Syntax-Table-Internals).

* [Table of Syntax Classes](#Syntax-Class-Table)
* [Syntax Flags](#Syntax-Flags)

---

Next: [Syntax Flags](#Syntax-Flags), Up: [Syntax Descriptors](#Syntax-Descriptors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 36.2.1 Table of Syntax Classes [¶](#Table-of-Syntax-Classes)

Here is a table of syntax classes, the characters that designate
them, their meanings, and examples of their use.

Whitespace characters: ‘’ or ‘`-`’
:   Characters that separate symbols and words from each other.
    Typically, whitespace characters have no other syntactic significance,
    and multiple whitespace characters are syntactically equivalent to a
    single one. Space, tab, and formfeed are classified as whitespace in
    almost all major modes.

    This syntax class can be designated by either ‘’ or
    ‘`-`’. Both designators are equivalent.

Word constituents: ‘`w`’
:   Parts of words in human languages. These are typically used in
    variable and command names in programs. All upper- and lower-case
    letters, and the digits, are typically word constituents.

Symbol constituents: ‘`_`’
:   Extra characters used in variable and command names along with word
    constituents. Examples include the characters ‘`$&*+-_<>`’ in Lisp
    mode, which may be part of a symbol name even though they are not part
    of English words. In standard C, the only non-word-constituent
    character that is valid in symbols is underscore (‘`_`’).

Punctuation characters: ‘`.`’
:   Characters used as punctuation in a human language, or used in a
    programming language to separate symbols from one another. Some
    programming language modes, such as Emacs Lisp mode, have no
    characters in this class since the few characters that are not symbol
    or word constituents all have other uses. Other programming language
    modes, such as C mode, use punctuation syntax for operators.

Open parenthesis characters: ‘`(`’

Close parenthesis characters: ‘`)`’
:   Characters used in dissimilar pairs to surround sentences or
    expressions. Such a grouping is begun with an open parenthesis
    character and terminated with a close. Each open parenthesis
    character matches a particular close parenthesis character, and vice
    versa. Normally, Emacs indicates momentarily the matching open
    parenthesis when you insert a close parenthesis. See [Blinking Parentheses](#Blinking).

    In human languages, and in C code, the parenthesis pairs are
    ‘`()`’, ‘`[]`’, and ‘`{}`’. In Emacs Lisp, the delimiters
    for lists and vectors (‘`()`’ and ‘`[]`’) are classified as
    parenthesis characters.

String quotes: ‘`"`’
:   Characters used to delimit string constants. The same string quote
    character appears at the beginning and the end of a string. Such
    quoted strings do not nest.

    The parsing facilities of Emacs consider a string as a single token.
    The usual syntactic meanings of the characters in the string are
    suppressed.

    The Lisp modes have two string quote characters: double-quote (‘`"`’)
    and vertical bar (‘`|`’). ‘`|`’ is not used in Emacs Lisp, but it
    is used in Common Lisp. C also has two string quote characters:
    double-quote for strings, and apostrophe (‘`'`’) for character
    constants.

    Human text has no string quote characters. We do not want quotation
    marks to turn off the usual syntactic properties of other characters
    in the quotation.

Escape-syntax characters: ‘`\`’
:   Characters that start an escape sequence, such as is used in string
    and character constants. The character ‘`\`’ belongs to this class
    in both C and Lisp. (In C, it is used thus only inside strings, but
    it turns out to cause no trouble to treat it this way throughout C
    code.)

    Characters in this class count as part of words if
    `words-include-escapes` is non-`nil`. See [Motion by Words](#Word-Motion).

Character quotes: ‘`/`’
:   Characters used to quote the following character so that it loses its
    normal syntactic meaning. This differs from an escape character in
    that only the character immediately following is ever affected.

    Characters in this class count as part of words if
    `words-include-escapes` is non-`nil`. See [Motion by Words](#Word-Motion).

    This class is used for backslash in TeX mode.

Paired delimiters: ‘`$`’
:   Similar to string quote characters, except that the syntactic
    properties of the characters between the delimiters are not
    suppressed. Only TeX mode uses a paired delimiter presently—the
    ‘`$`’ that both enters and leaves math mode.

Expression prefixes: ‘`'`’
:   Characters used for syntactic operators that are considered as part of
    an expression if they appear next to one. In Lisp modes, these
    characters include the apostrophe, ‘`'`’ (used for quoting), the
    comma, ‘`,`’ (used in macros), and ‘`#`’ (used in the read
    syntax for certain data types).

Comment starters: ‘`<`’ [¶](#index-comment-syntax)

Comment enders: ‘`>`’
:   Characters used in various languages to delimit comments. Human text
    has no comment characters. In Lisp, the semicolon (‘`;`’) starts a
    comment and a newline or formfeed ends one.

Inherit standard syntax: ‘`@`’
:   This syntax class does not specify a particular syntax. It says to
    look in the parent syntax table to find the syntax of this
    character.

Generic comment delimiters: ‘`!`’
:   (This syntax class is also known as “comment-fence”.) Characters
    that start or end a special kind of comment. *Any* generic
    comment delimiter matches *any* generic comment delimiter, but
    they cannot match a comment starter or comment ender; generic comment
    delimiters can only match each other.

    This syntax class is primarily meant for use with the
    `syntax-table` text property (see [Syntax Properties](#Syntax-Properties)). You
    can mark any range of characters as forming a comment, by giving the
    first and last characters of the range `syntax-table` properties
    identifying them as generic comment delimiters.

Generic string delimiters: ‘`|`’
:   (This syntax class is also known as “string-fence”.) Characters
    that start or end a string. This class differs from the string quote
    class in that *any* generic string delimiter can match any other
    generic string delimiter; but they do not match ordinary string quote
    characters.

    This syntax class is primarily meant for use with the
    `syntax-table` text property (see [Syntax Properties](#Syntax-Properties)). You
    can mark any range of characters as forming a string constant, by
    giving the first and last characters of the range `syntax-table`
    properties identifying them as generic string delimiters.

---

Previous: [Table of Syntax Classes](#Syntax-Class-Table), Up: [Syntax Descriptors](#Syntax-Descriptors)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 36.2.2 Syntax Flags [¶](#Syntax-Flags-1)

In addition to the classes, entries for characters in a syntax table
can specify flags. There are eight possible flags, represented by the
characters ‘`1`’, ‘`2`’, ‘`3`’, ‘`4`’, ‘`b`’, ‘`c`’,
‘`n`’, and ‘`p`’.

All the flags except ‘`p`’ are used to describe comment
delimiters. The digit flags are used for comment delimiters made up
of 2 characters. They indicate that a character can *also* be
part of a comment sequence, in addition to the syntactic properties
associated with its character class. The flags are independent of the
class and each other for the sake of characters such as ‘`*`’ in
C mode, which is a punctuation character, *and* the second
character of a start-of-comment sequence (‘`/*`’), *and* the
first character of an end-of-comment sequence (‘`*/`’). The flags
‘`b`’, ‘`c`’, and ‘`n`’ are used to qualify the corresponding
comment delimiter.

Here is a table of the possible flags for a character c,
and what they mean:

* ‘`1`’ means c is the start of a two-character comment-start
  sequence.
* ‘`2`’ means c is the second character of such a sequence.
* ‘`3`’ means c is the start of a two-character comment-end
  sequence.
* ‘`4`’ means c is the second character of such a sequence.
* ‘`b`’ means that c as a comment delimiter belongs to the
  alternative “b” comment style. For a two-character comment starter,
  this flag is only significant on the second char, and for a 2-character
  comment ender it is only significant on the first char.
* ‘`c`’ means that c as a comment delimiter belongs to the
  alternative “c” comment style. For a two-character comment
  delimiter, ‘`c`’ on either character makes it of style “c”.
* ‘`n`’ on a comment delimiter character specifies that this kind of
  comment can be nested. Inside such a comment, only comments of the
  same style will be recognized. For a two-character comment delimiter,
  ‘`n`’ on either character makes it nestable.

  Emacs supports several comment styles simultaneously in any one syntax
  table. A comment style is a set of flags ‘`b`’, ‘`c`’, and
  ‘`n`’, so there can be up to 8 different comment styles, each one
  named by the set of its flags. Each comment delimiter has a style and
  only matches comment delimiters of the same style. Thus if a comment
  starts with the comment-start sequence of style “bn”, it will extend
  until the next matching comment-end sequence of style “bn”. When
  the set of flags has neither flag ‘`b`’ nor flag ‘`c`’ set, the
  resulting style is called the “a” style.

  The appropriate comment syntax settings for C++ can be as follows:

  ‘`/`’
  :   ‘`124`’

  ‘`*`’
  :   ‘`23b`’

  newline
  :   ‘`>`’

  This defines four comment-delimiting sequences:

  ‘`/*`’
  :   This is a comment-start sequence for “b” style because the
      second character, ‘`*`’, has the ‘`b`’ flag.

  ‘`//`’
  :   This is a comment-start sequence for “a” style because the second
      character, ‘`/`’, does not have the ‘`b`’ flag.

  ‘`*/`’
  :   This is a comment-end sequence for “b” style because the first
      character, ‘`*`’, has the ‘`b`’ flag.

  newline
  :   This is a comment-end sequence for “a” style, because the newline
      character does not have the ‘`b`’ flag.
* ‘`p`’ identifies an additional prefix character for Lisp syntax.
  These characters are treated as whitespace when they appear between
  expressions. When they appear within an expression, they are handled
  according to their usual syntax classes.

  The function `backward-prefix-chars` moves back over these
  characters, as well as over characters whose primary syntax class is
  prefix (‘`'`’). See [Motion and Syntax](#Motion-and-Syntax).

---

Next: [Syntax Properties](#Syntax-Properties), Previous: [Syntax Descriptors](#Syntax-Descriptors), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 36.3 Syntax Table Functions [¶](#Syntax-Table-Functions-1)

In this section we describe functions for creating, accessing and
altering syntax tables.

Function: **make-syntax-table** &optional table [¶](#index-make_002dsyntax_002dtable)
:   This function creates a new syntax table. If table is
    non-`nil`, the parent of the new syntax table is table;
    otherwise, the parent is the standard syntax table.

    In the new syntax table, all characters are initially given the
    “inherit” (‘`@`’) syntax class, i.e., their syntax is inherited
    from the parent table (see [Table of Syntax Classes](#Syntax-Class-Table)).

Function: **copy-syntax-table** &optional table [¶](#index-copy_002dsyntax_002dtable)
:   This function constructs a copy of table and returns it. If
    table is omitted or `nil`, it returns a copy of the
    standard syntax table. Otherwise, an error is signaled if table
    is not a syntax table.

Command: **modify-syntax-entry** char syntax-descriptor &optional table [¶](#index-modify_002dsyntax_002dentry)
:   This function sets the syntax entry for char according to
    syntax-descriptor. char must be a character, or a cons
    cell of the form `(min . max)`; in the latter case,
    the function sets the syntax entries for all characters in the range
    between min and max, inclusive.

    The syntax is changed only for table, which defaults to the
    current buffer’s syntax table, and not in any other syntax table.

    The argument syntax-descriptor is a syntax descriptor, i.e., a
    string whose first character is a syntax class designator and whose
    second and subsequent characters optionally specify a matching
    character and syntax flags. See [Syntax Descriptors](#Syntax-Descriptors). An error is
    signaled if syntax-descriptor is not a valid syntax descriptor.

    This function always returns `nil`. The old syntax information in
    the table for this character is discarded.

    ```
    Examples:
    ```

    ```
    ;; Put the space character in class whitespace.
    (modify-syntax-entry ?\s " ")
         ⇒ nil
    ```

    ```

    ```

    ```
    ;; Make ‘$’ an open parenthesis character,
    ;;   with ‘^’ as its matching close.
    (modify-syntax-entry ?$ "(^")
         ⇒ nil
    ```

    ```

    ```

    ```
    ;; Make ‘^’ a close parenthesis character,
    ;;   with ‘$’ as its matching open.
    (modify-syntax-entry ?^ ")$")
         ⇒ nil
    ```

    ```

    ```

    ```
    ;; Make ‘/’ a punctuation character,
    ;;   the first character of a start-comment sequence,
    ;;   and the second character of an end-comment sequence.
    ;;   This is used in C mode.
    (modify-syntax-entry ?/ ". 14")
         ⇒ nil
    ```

Function: **char-syntax** character [¶](#index-char_002dsyntax)
:   This function returns the syntax class of character, represented
    by its designator character (see [Table of Syntax Classes](#Syntax-Class-Table)). This
    returns *only* the class, not its matching character or syntax
    flags.

    The following examples apply to C mode. (We use `string` to make
    it easier to see the character returned by `char-syntax`.)

    ```
    ;; Space characters have whitespace syntax class.
    (string (char-syntax ?\s))
         ⇒ " "
    ```

    ```

    ```

    ```
    ;; Forward slash characters have punctuation syntax.
    ;; Note that this char-syntax call does not reveal
    ;; that it is also part of comment-start and -end sequences.
    (string (char-syntax ?/))
         ⇒ "."
    ```

    ```

    ```

    ```
    ;; Open parenthesis characters have open parenthesis syntax.
    ;; Note that this char-syntax call does not reveal that
    ;; it has a matching character, ‘)’.
    (string (char-syntax ?\())
         ⇒ "("
    ```

Function: **set-syntax-table** table [¶](#index-set_002dsyntax_002dtable)
:   This function makes table the syntax table for the current buffer.
    It returns table.

Function: **syntax-table** [¶](#index-syntax_002dtable)
:   This function returns the current syntax table, which is the table for
    the current buffer.

Command: **describe-syntax** &optional buffer [¶](#index-describe_002dsyntax)
:   This command displays the contents of the syntax table of
    buffer (by default, the current buffer) in a help buffer.

Macro: **with-syntax-table** table body… [¶](#index-with_002dsyntax_002dtable)
:   This macro executes body using table as the current syntax
    table. It returns the value of the last form in body, after
    restoring the old current syntax table.

    Since each buffer has its own current syntax table, we should make that
    more precise: `with-syntax-table` temporarily alters the current
    syntax table of whichever buffer is current at the time the macro
    execution starts. Other buffers are not affected.

---

Next: [Motion and Syntax](#Motion-and-Syntax), Previous: [Syntax Table Functions](#Syntax-Table-Functions), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 36.4 Syntax Properties [¶](#Syntax-Properties-1)

When the syntax table is not flexible enough to specify the syntax of
a language, you can override the syntax table for specific character
occurrences in the buffer, by applying a `syntax-table` text
property. See [Text Properties](#Text-Properties), for how to apply text properties.

The valid values of `syntax-table` text property are:

syntax-table
:   If the property value is a syntax table, that table is used instead of
    the current buffer’s syntax table to determine the syntax for the
    underlying text character.

`(syntax-code . matching-char)`
:   A cons cell of this format is a raw syntax descriptor (see [Syntax Table Internals](#Syntax-Table-Internals)), which directly specifies a syntax class for the
    underlying text character.

`nil`
:   If the property is `nil`, the character’s syntax is determined from
    the current syntax table in the usual way.

Variable: **parse-sexp-lookup-properties** [¶](#index-parse_002dsexp_002dlookup_002dproperties)
:   If this is non-`nil`, the syntax scanning functions, like
    `forward-sexp`, pay attention to `syntax-table` text
    properties. Otherwise they use only the current syntax table.

Variable: **syntax-propertize-function** [¶](#index-syntax_002dpropertize_002dfunction)
:   This variable, if non-`nil`, should store a function for applying
    `syntax-table` properties to a specified stretch of text. It is
    intended to be used by major modes to install a function which applies
    `syntax-table` properties in some mode-appropriate way.

    The function is called by `syntax-ppss` (see [Finding the Parse State for a Position](#Position-Parse)),
    and by Font Lock mode during syntactic fontification (see [Syntactic Font Lock](#Syntactic-Font-Lock)). It is called with two arguments, start and
    end, which are the starting and ending positions of the text on
    which it should act. It is allowed to arbitrarily move point within
    the region delimited by start and end; such motions don’t
    need to use `save-excursion` (see [Excursions](#Excursions)). It is also
    allowed to call `syntax-ppss` on any position before end,
    but if a Lisp program calls `syntax-ppss` on some position and
    later modifies the buffer at some earlier position, then it is that
    program’s responsibility to call `syntax-ppss-flush-cache` to
    flush the now obsolete info from the cache.

    **Caution:** When this variable is non-`nil`, Emacs removes
    `syntax-table` text properties arbitrarily and relies on
    `syntax-propertize-function` to reapply them. Thus if this
    facility is used at all, the function must apply **all**
    `syntax-table` text properties used by the major mode. In
    particular, Modes derived from a CC Mode mode must not use this
    variable, since CC Mode uses other means to apply and remove these
    text properties.

Variable: **syntax-propertize-extend-region-functions** [¶](#index-syntax_002dpropertize_002dextend_002dregion_002dfunctions)
:   This abnormal hook is run by the syntax parsing code prior to calling
    `syntax-propertize-function`. Its role is to help locate safe
    starting and ending buffer positions for passing to
    `syntax-propertize-function`. For example, a major mode can add
    a function to this hook to identify multi-line syntactic constructs,
    and ensure that the boundaries do not fall in the middle of one.

    Each function in this hook should accept two arguments, start
    and end. It should return either a cons cell of two adjusted
    buffer positions, `(new-start . new-end)`, or
    `nil` if no adjustment is necessary. The hook functions are run
    in turn, repeatedly, until they all return `nil`.

---

Next: [Parsing Expressions](#Parsing-Expressions), Previous: [Syntax Properties](#Syntax-Properties), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 36.5 Motion and Syntax [¶](#Motion-and-Syntax-1)

This section describes functions for moving across characters that
have certain syntax classes.

Function: **skip-syntax-forward** syntaxes &optional limit [¶](#index-skip_002dsyntax_002dforward)
:   This function moves point forward across characters having syntax
    classes mentioned in syntaxes (a string of syntax class
    characters). It stops when it encounters the end of the buffer, or
    position limit (if specified), or a character it is not supposed
    to skip.

    If syntaxes starts with ‘`^`’, then the function skips
    characters whose syntax is *not* in syntaxes.

    The return value is the distance traveled, which is a nonnegative
    integer.

Function: **skip-syntax-backward** syntaxes &optional limit [¶](#index-skip_002dsyntax_002dbackward)
:   This function moves point backward across characters whose syntax
    classes are mentioned in syntaxes. It stops when it encounters
    the beginning of the buffer, or position limit (if specified), or
    a character it is not supposed to skip.

    If syntaxes starts with ‘`^`’, then the function skips
    characters whose syntax is *not* in syntaxes.

    The return value indicates the distance traveled. It is an integer that
    is zero or less.

Function: **backward-prefix-chars** [¶](#index-backward_002dprefix_002dchars)
:   This function moves point backward over any number of characters with
    expression prefix syntax. This includes both characters in the
    expression prefix syntax class, and characters with the ‘`p`’ flag.

---

Next: [Syntax Table Internals](#Syntax-Table-Internals), Previous: [Motion and Syntax](#Motion-and-Syntax), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 36.6 Parsing Expressions [¶](#Parsing-Expressions-1)

This section describes functions for parsing and scanning balanced
expressions. We will refer to such expressions as *sexps*,
following the terminology of Lisp, even though these functions can act
on languages other than Lisp. Basically, a sexp is either a balanced
parenthetical grouping, a string, or a symbol (i.e., a sequence
of characters whose syntax is either word constituent or symbol
constituent). However, characters in the expression prefix syntax
class (see [Table of Syntax Classes](#Syntax-Class-Table)) are treated as part of the sexp if
they appear next to it.

The syntax table controls the interpretation of characters, so these
functions can be used for Lisp expressions when in Lisp mode and for C
expressions when in C mode. See [Moving over Balanced Expressions](#List-Motion), for convenient
higher-level functions for moving over balanced expressions.

A character’s syntax controls how it changes the state of the
parser, rather than describing the state itself. For example, a
string delimiter character toggles the parser state between
in-string and in-code, but the syntax of characters does not
directly say whether they are inside a string. For example (note that
15 is the syntax code for generic string delimiters),

```
(put-text-property 1 9 'syntax-table '(15 . nil))
```

does not tell Emacs that the first eight chars of the current buffer
are a string, but rather that they are all string delimiters. As a
result, Emacs treats them as four consecutive empty string constants.

* [Motion Commands Based on Parsing](#Motion-via-Parsing)
* [Finding the Parse State for a Position](#Position-Parse)
* [Parser State](#Parser-State)
* [Low-Level Parsing](#Low_002dLevel-Parsing)
* [Parameters to Control Parsing](#Control-Parsing)

---

Next: [Finding the Parse State for a Position](#Position-Parse), Up: [Parsing Expressions](#Parsing-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 36.6.1 Motion Commands Based on Parsing [¶](#Motion-Commands-Based-on-Parsing)

This section describes simple point-motion functions that operate
based on parsing expressions.

Function: **scan-lists** from count depth [¶](#index-scan_002dlists)
:   This function scans forward count balanced parenthetical
    groupings from position from. It returns the position where the
    scan stops. If count is negative, the scan moves backwards.

    If depth is nonzero, treat the starting position as being
    depth parentheses deep. The scanner moves forward or backward
    through the buffer until the depth changes to zero count times.
    Hence, a positive value for depth has the effect of moving out
    depth levels of parenthesis from the starting position, while a
    negative depth has the effect of moving deeper by -depth
    levels of parenthesis.

    Scanning ignores comments if `parse-sexp-ignore-comments` is
    non-`nil`.

    If the scan reaches the beginning or end of the accessible part of the
    buffer before it has scanned over count parenthetical groupings,
    the return value is `nil` if the depth at that point is zero; if
    the depth is non-zero, a `scan-error` error is signaled.

Function: **scan-sexps** from count [¶](#index-scan_002dsexps)
:   This function scans forward count sexps from position from.
    It returns the position where the scan stops. If count is
    negative, the scan moves backwards.

    Scanning ignores comments if `parse-sexp-ignore-comments` is
    non-`nil`.

    If the scan reaches the beginning or end of (the accessible part of) the
    buffer while in the middle of a parenthetical grouping, an error is
    signaled. If it reaches the beginning or end between groupings but
    before count is used up, `nil` is returned.

Function: **forward-comment** count [¶](#index-forward_002dcomment)
:   This function moves point forward across count complete comments
    (that is, including the starting delimiter and the terminating
    delimiter if any), plus any whitespace encountered on the way. It
    moves backward if count is negative. If it encounters anything
    other than a comment or whitespace, it stops, leaving point at the
    place where it stopped. This includes (for instance) finding the end
    of a comment when moving forward and expecting the beginning of one.
    The function also stops immediately after moving over the specified
    number of complete comments. If count comments are found as
    expected, with nothing except whitespace between them, it returns
    `t`; otherwise it returns `nil`.

    This function cannot tell whether the comments it traverses are
    embedded within a string. If they look like comments, it treats them
    as comments.

    To move forward over all comments and whitespace following point, use
    `(forward-comment (buffer-size))`. `(buffer-size)` is a
    good argument to use, because the number of comments in the buffer
    cannot exceed that many.

---

Next: [Parser State](#Parser-State), Previous: [Motion Commands Based on Parsing](#Motion-via-Parsing), Up: [Parsing Expressions](#Parsing-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 36.6.2 Finding the Parse State for a Position [¶](#Finding-the-Parse-State-for-a-Position)

For syntactic analysis, such as in indentation, often the useful
thing is to compute the syntactic state corresponding to a given buffer
position. This function does that conveniently.

Function: **syntax-ppss** &optional pos [¶](#index-syntax_002dppss)
:   This function returns the parser state that the parser would reach at
    position pos starting from the beginning of the visible portion
    of the buffer.
    See [Parser State](#Parser-State),
    for a description of the parser state.

    The return value is the same as if you call the low-level parsing
    function `parse-partial-sexp` to parse from the beginning of the
    visible portion of the buffer to pos (see [Low-Level Parsing](#Low_002dLevel-Parsing)). However, `syntax-ppss` uses caches to speed up the
    computation. Due to this optimization, the second value (previous
    complete subexpression) and sixth value (minimum parenthesis depth) in
    the returned parser state are not meaningful.

    This function has a side effect: it adds a buffer-local entry to
    `before-change-functions` (see [Change Hooks](#Change-Hooks)) for
    `syntax-ppss-flush-cache` (see below). This entry keeps the
    cache consistent as the buffer is modified. However, the cache might
    not be updated if `syntax-ppss` is called while
    `before-change-functions` is temporarily let-bound, or if the
    buffer is modified without running the hook, such as when using
    `inhibit-modification-hooks`. In those cases, it is necessary to
    call `syntax-ppss-flush-cache` explicitly.

Function: **syntax-ppss-flush-cache** beg &rest ignored-args [¶](#index-syntax_002dppss_002dflush_002dcache)
:   This function flushes the cache used by `syntax-ppss`, starting
    at position beg. The remaining arguments, ignored-args,
    are ignored; this function accepts them so that it can be directly
    used on hooks such as `before-change-functions` (see [Change Hooks](#Change-Hooks)).

---

Next: [Low-Level Parsing](#Low_002dLevel-Parsing), Previous: [Finding the Parse State for a Position](#Position-Parse), Up: [Parsing Expressions](#Parsing-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 36.6.3 Parser State [¶](#Parser-State-1)

A *parser state* is a list of (currently) eleven elements
describing the state of the syntactic parser, after it parses the text
between a specified starting point and a specified end point in the
buffer using `parse-partial-sexp` (see [Low-Level Parsing](#Low_002dLevel-Parsing)).
Parsing functions such as `syntax-ppss`
(see [Finding the Parse State for a Position](#Position-Parse))
also return a parser state as the value. `parse-partial-sexp`
can accept a parser state as an argument, for resuming parsing.

Here are the meanings of the elements of the parser state:

0. The depth in parentheses, counting from 0. **Warning:** this can
   be negative if there are more close parens than open parens between
   the parser’s starting point and end point.
1. The character position of the start of the innermost parenthetical
   grouping containing the stopping point; `nil` if none.
2. The character position of the start of the last complete subexpression
   terminated; `nil` if none.
3. Non-`nil` if inside a string. More precisely, this is the
   character that will terminate the string, or `t` if a generic
   string delimiter character should terminate it.
4. `t` if inside a non-nestable comment (of any comment style;
   see [Syntax Flags](#Syntax-Flags)); or the comment nesting level if inside a
   comment that can be nested.
5. `t` if the end point is just after a quote character.
6. The minimum parenthesis depth encountered during this scan.
7. What kind of comment is active: `nil` if not in a comment or in a
   comment of style ‘`a`’; 1 for a comment of style ‘`b`’; 2 for a
   comment of style ‘`c`’; and `syntax-table` for a comment that
   should be ended by a generic comment delimiter character.
8. The string or comment start position. While inside a comment, this is
   the position where the comment began; while inside a string, this is the
   position where the string began. When outside of strings and comments,
   this element is `nil`.
9. The list of the positions of the currently open parentheses, starting
   with the outermost.
10. When the last buffer position scanned was the (potential) first
    character of a two character construct (comment delimiter or
    escaped/char-quoted character pair), the syntax-code
    (see [Syntax Table Internals](#Syntax-Table-Internals)) of that position. Otherwise
    `nil`.

Elements 1, 2, and 6 are ignored in a state which you pass as an
argument to `parse-partial-sexp` to continue parsing. Elements 9
and 10 are mainly used internally by the parser code.

Some additional useful information is available from a parser state
using these functions:

Function: **syntax-ppss-toplevel-pos** state [¶](#index-syntax_002dppss_002dtoplevel_002dpos)
:   This function extracts, from parser state state, the last
    position scanned in the parse which was at top level in grammatical
    structure. “At top level” means outside of any parentheses,
    comments, or strings.

    The value is `nil` if state represents a parse which has
    arrived at a top level position.

Function: **syntax-ppss-context** state [¶](#index-syntax_002dppss_002dcontext)
:   Return `string` if the end position of the scan returning
    state is in a string, and `comment` if it’s in a comment.
    Otherwise return `nil`.

---

Next: [Parameters to Control Parsing](#Control-Parsing), Previous: [Parser State](#Parser-State), Up: [Parsing Expressions](#Parsing-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 36.6.4 Low-Level Parsing [¶](#Low_002dLevel-Parsing-1)

The most basic way to use the expression parser is to tell it
to start at a given position with a certain state, and parse up to
a specified end position.

Function: **parse-partial-sexp** start limit &optional target-depth stop-before state stop-comment [¶](#index-parse_002dpartial_002dsexp)
:   This function parses a sexp in the current buffer starting at
    start, not scanning past limit. It stops at position
    limit or when certain criteria described below are met, and sets
    point to the location where parsing stops. It returns a parser state
    describing the status of the parse at the point where it stops.

    If the third argument target-depth is non-`nil`, parsing
    stops if the depth in parentheses becomes equal to target-depth.
    The depth starts at 0, or at whatever is given in state.

    If the fourth argument stop-before is non-`nil`, parsing
    stops when it comes to any character that starts a sexp. If
    stop-comment is non-`nil`, parsing stops after the start of
    an unnested comment. If stop-comment is the symbol
    `syntax-table`, parsing stops after the start of an unnested
    comment or a string, or after the end of an unnested comment or a
    string, whichever comes first.

    If state is `nil`, start is assumed to be at the top
    level of parenthesis structure, such as the beginning of a function
    definition. Alternatively, you might wish to resume parsing in the
    middle of the structure. To do this, you must provide a state
    argument that describes the initial status of parsing. The value
    returned by a previous call to `parse-partial-sexp` will do
    nicely.

---

Previous: [Low-Level Parsing](#Low_002dLevel-Parsing), Up: [Parsing Expressions](#Parsing-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 36.6.5 Parameters to Control Parsing [¶](#Parameters-to-Control-Parsing)

Variable: **multibyte-syntax-as-symbol** [¶](#index-multibyte_002dsyntax_002das_002dsymbol)
:   If this variable is non-`nil`, `scan-sexps` treats all
    non-ASCII characters as symbol constituents regardless of
    what the syntax table says about them. (However, `syntax-table`text properties can still override the syntax.)

User Option: **parse-sexp-ignore-comments** [¶](#index-parse_002dsexp_002dignore_002dcomments)
:   If the value is non-`nil`, then comments are treated as
    whitespace by the functions in this section and by `forward-sexp`,
    `scan-lists` and `scan-sexps`.

The behavior of `parse-partial-sexp` is also affected by
`parse-sexp-lookup-properties` (see [Syntax Properties](#Syntax-Properties)).

Variable: **comment-end-can-be-escaped** [¶](#index-comment_002dend_002dcan_002dbe_002descaped)
:   If this buffer local variable is non-`nil`, a single character
    which usually terminates a comment doesn’t do so when that character
    is escaped. This is used in C and C++ Modes, where line comments
    starting with ‘`//`’ can be continued onto the next line by
    escaping the newline with ‘`\`’.

You can use `forward-comment` to move forward or backward over
one comment or several comments.

---

Next: [Categories](#Categories), Previous: [Parsing Expressions](#Parsing-Expressions), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 36.7 Syntax Table Internals [¶](#Syntax-Table-Internals-1)

Syntax tables are implemented as char-tables (see [Char-Tables](#Char_002dTables)),
but most Lisp programs don’t work directly with their elements.
Syntax tables do not store syntax data as syntax descriptors
(see [Syntax Descriptors](#Syntax-Descriptors)); they use an internal format, which is
documented in this section. This internal format can also be assigned
as syntax properties (see [Syntax Properties](#Syntax-Properties)).

Each entry in a syntax table is a *raw syntax descriptor*: a
cons cell of the form `(syntax-code
. matching-char)`. syntax-code is an integer which
encodes the syntax class and syntax flags, according to the table
below. matching-char, if non-`nil`, specifies a matching
character (similar to the second character in a syntax descriptor).

Use `aref` (see [Functions that Operate on Arrays](#Array-Functions)) to get the raw syntax
descriptor of a character, for example `(aref (syntax-table) ch)`.

Here are the syntax codes corresponding to the various syntax
classes:

|  |  |  |  |
| --- | --- | --- | --- |
| *Code* | *Class* | *Code* | *Class* |
| 0 | whitespace | 8 | paired delimiter |
| 1 | punctuation | 9 | escape |
| 2 | word | 10 | character quote |
| 3 | symbol | 11 | comment-start |
| 4 | open parenthesis | 12 | comment-end |
| 5 | close parenthesis | 13 | inherit |
| 6 | expression prefix | 14 | generic comment |
| 7 | string quote | 15 | generic string |

For example, in the standard syntax table, the entry for ‘`(`’ is
`(4 . 41)`. 41 is the character code for ‘`)`’.

Syntax flags are encoded in higher order bits, starting 16 bits from
the least significant bit. This table gives the power of two which
corresponds to each syntax flag.

|  |  |  |  |
| --- | --- | --- | --- |
| *Prefix* | *Flag* | *Prefix* | *Flag* |
| ‘`1`’ | `(ash 1 16)` | ‘`p`’ | `(ash 1 20)` |
| ‘`2`’ | `(ash 1 17)` | ‘`b`’ | `(ash 1 21)` |
| ‘`3`’ | `(ash 1 18)` | ‘`n`’ | `(ash 1 22)` |
| ‘`4`’ | `(ash 1 19)` | ‘`c`’ | `(ash 1 23)` |

Function: **string-to-syntax** desc [¶](#index-string_002dto_002dsyntax)
:   Given a syntax descriptor desc (a string), this function returns
    the corresponding raw syntax descriptor.

Function: **syntax-class-to-char** syntax [¶](#index-syntax_002dclass_002dto_002dchar)
:   Given a raw syntax descriptor syntax (an integer), this function
    returns the corresponding syntax descriptor (a character).

Function: **syntax-after** pos [¶](#index-syntax_002dafter)
:   This function returns the raw syntax descriptor for the character in
    the buffer after position pos, taking account of syntax
    properties as well as the syntax table. If pos is outside the
    buffer’s accessible portion (see [accessible portion](#Narrowing)),
    the return value is `nil`.

Function: **syntax-class** syntax [¶](#index-syntax_002dclass)
:   This function returns the syntax code for the raw syntax descriptor
    syntax. More precisely, it takes the raw syntax descriptor’s
    syntax-code component, masks off the high 16 bits which record
    the syntax flags, and returns the resulting integer.

    If syntax is `nil`, the return value is `nil`. This
    is so that the expression

    ```
    (syntax-class (syntax-after pos))
    ```

    evaluates to `nil` if `pos` is outside the buffer’s
    accessible portion, without throwing errors or returning an incorrect
    code.

---

Previous: [Syntax Table Internals](#Syntax-Table-Internals), Up: [Syntax Tables](#Syntax-Tables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
