### 24.7 Font Lock Mode [¶](#Font-Lock-Mode-1)

*Font Lock mode* is a buffer-local minor mode that automatically
attaches `face` properties to certain parts of the buffer based on
their syntactic role. How it parses the buffer depends on the major
mode; most major modes define syntactic criteria for which faces to use
in which contexts. This section explains how to customize Font Lock for
a particular major mode.

Font Lock mode finds text to highlight in three ways: through
parsing based on a full-blown parser (usually, via an external library
or program), through syntactic parsing based on the Emacs’s built-in
syntax table, or through searching (usually for regular expressions).
If enabled, parser-based fontification happens first
(see [Parser-based Font Lock](#Parser_002dbased-Font-Lock)). Syntactic fontification happens
next; it finds comments and string constants and highlights them.
Search-based fontification happens last.

* [Font Lock Basics](#Font-Lock-Basics)
* [Search-based Fontification](#Search_002dbased-Fontification)
* [Customizing Search-Based Fontification](#Customizing-Keywords)
* [Other Font Lock Variables](#Other-Font-Lock-Variables)
* [Levels of Font Lock](#Levels-of-Font-Lock)
* [Precalculated Fontification](#Precalculated-Fontification)
* [Faces for Font Lock](#Faces-for-Font-Lock)
* [Syntactic Font Lock](#Syntactic-Font-Lock)
* [Multiline Font Lock Constructs](#Multiline-Font-Lock)
* [Parser-based Font Lock](#Parser_002dbased-Font-Lock)

---

Next: [Search-based Fontification](#Search_002dbased-Fontification), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.1 Font Lock Basics [¶](#Font-Lock-Basics-1)

The Font Lock functionality is based on several basic functions.
Each of these calls the function specified by the corresponding
variable. This indirection allows major and minor modes to modify the
way fontification works in the buffers of that mode, and even use the
Font Lock mechanisms for features that have nothing to do with
fontification. (This is why the description below says “should”
when it describes what the functions do: the mode can customize the
values of the corresponding variables to do something entirely
different.) The variables mentioned below are described in [Other Font Lock Variables](#Other-Font-Lock-Variables).

`font-lock-fontify-buffer` [¶](#index-font_002dlock_002dfontify_002dbuffer)
:   This function should fontify the current buffer’s accessible portion,
    by calling the function specified by
    `font-lock-fontify-buffer-function`.

`font-lock-unfontify-buffer` [¶](#index-font_002dlock_002dunfontify_002dbuffer)
:   Used when turning Font Lock off to remove the fontification. Calls
    the function specified by `font-lock-unfontify-buffer-function`.

`font-lock-fontify-region beg end &optional loudly` [¶](#index-font_002dlock_002dfontify_002dregion-beg-end-_0026optional-loudly)
:   Should fontify the region between beg and end. If
    loudly is non-`nil`, should display status messages while
    fontifying. Calls the function specified by
    `font-lock-fontify-region-function`.

`font-lock-unfontify-region beg end` [¶](#index-font_002dlock_002dunfontify_002dregion-beg-end)
:   Should remove fontification from the region between beg and
    end. Calls the function specified by
    `font-lock-unfontify-region-function`.

`font-lock-flush &optional beg end` [¶](#index-font_002dlock_002dflush-_0026optional-beg-end)
:   This function should mark the fontification of the region between
    beg and end as outdated. If not specified or `nil`,
    beg and end default to the beginning and end of the
    buffer’s accessible portion. Calls the function specified by
    `font-lock-flush-function`.

`font-lock-ensure &optional beg end` [¶](#index-font_002dlock_002densure-_0026optional-beg-end)
:   This function should make sure the region between beg and
    end has been fontified. The optional arguments beg and
    end default to the beginning and the end of the buffer’s
    accessible portion. Calls the function specified by
    `font-lock-ensure-function`.

`font-lock-debug-fontify` [¶](#index-font_002dlock_002ddebug_002dfontify)
:   This is a convenience command meant to be used when developing font
    locking for a mode, and should not be called from Lisp code. It
    recomputes all the relevant variables and then calls
    `font-lock-fontify-region` on the entire buffer.

There are several variables that control how Font Lock mode highlights
text. But major modes should not set any of these variables directly.
Instead, they should set `font-lock-defaults` as a buffer-local
variable. The value assigned to this variable is used, if and when Font
Lock mode is enabled, to set all the other variables.

Variable: **font-lock-defaults** [¶](#index-font_002dlock_002ddefaults)
:   This variable is set by modes to specify how to fontify text in that
    mode. It automatically becomes buffer-local when set. If its value
    is `nil`, Font Lock mode does no highlighting.

    If non-`nil`, the value should look like this:

    ```
    (keywords [keywords-only [case-fold
     [syntax-alist other-vars...]]])
    ```

    The first element, keywords, indirectly specifies the value of
    `font-lock-keywords` which directs search-based fontification.
    It can be a symbol, a variable or a function whose value is the list
    to use for `font-lock-keywords`. It can also be a list of
    several such symbols, one for each possible level of fontification.
    The first symbol specifies the ‘`mode default`’ level of
    fontification, the next symbol level 1 fontification, the next level 2,
    and so on. The ‘`mode default`’ level is normally the same as level
    1. It is used when `font-lock-maximum-decoration` has a `nil`
    value. See [Levels of Font Lock](#Levels-of-Font-Lock).

    The second element, keywords-only, specifies the value of the
    variable `font-lock-keywords-only`. If this is omitted or
    `nil`, syntactic fontification (of strings and comments) is also
    performed. If this is non-`nil`, syntactic fontification is not
    performed. See [Syntactic Font Lock](#Syntactic-Font-Lock).

    The third element, case-fold, specifies the value of
    `font-lock-keywords-case-fold-search`. If it is non-`nil`,
    Font Lock mode ignores case during search-based fontification.

    If the fourth element, syntax-alist, is non-`nil`, it should
    be a list of cons cells of the form `(char-or-string
    . string)`. These are used to set up a syntax table during
    fontification; the resulting syntax table is stored in
    `font-lock-syntax-table`. If syntax-alist is omitted or
    `nil`, fontification uses the syntax table returned by
    the `syntax-table` function. See [Syntax Table Functions](#Syntax-Table-Functions).

    The most common uses of syntax-alist simply change the syntax of
    a few chars from symbol constituent to word constituent so that the
    fontification rules can use regexp operators based on word boundaries.
    When syntax-alist describes more intrusive changes that can change
    what is recognized as a string or a comment, this prevents an important
    optimization in syntactic fontification, so it’s better to either
    refrain from using such settings or to additionally set
    `syntax-ppss-table` to a non-`nil` value, which takes
    precedence during syntactic fontification.

    All the remaining elements (if any) are collectively called
    other-vars. Each of these elements should have the form
    `(variable . value)`—which means, make
    variable buffer-local and then set it to value. You can
    use these other-vars to set other variables that affect
    fontification, aside from those you can control with the first five
    elements. See [Other Font Lock Variables](#Other-Font-Lock-Variables).

If your mode fontifies text explicitly by adding
`font-lock-face` properties, it can specify `(nil t)` for
`font-lock-defaults` to turn off all automatic fontification.
However, this is not required; it is possible to fontify some things
using `font-lock-face` properties and set up automatic
fontification for other parts of the text.

---

Next: [Customizing Search-Based Fontification](#Customizing-Keywords), Previous: [Font Lock Basics](#Font-Lock-Basics), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.2 Search-based Fontification [¶](#Search_002dbased-Fontification-1)

The variable which directly controls search-based fontification is
`font-lock-keywords`, which is typically specified via the
keywords element in `font-lock-defaults`.

Variable: **font-lock-keywords** [¶](#index-font_002dlock_002dkeywords)
:   The value of this variable is a list of the keywords to highlight. Lisp
    programs should not set this variable directly. Normally, the value is
    automatically set by Font Lock mode, using the keywords element in
    `font-lock-defaults`. The value can also be altered using the
    functions `font-lock-add-keywords` and
    `font-lock-remove-keywords` (see [Customizing Search-Based Fontification](#Customizing-Keywords)).

Each element of `font-lock-keywords` specifies how to find
certain cases of text, and how to highlight those cases. Font Lock mode
processes the elements of `font-lock-keywords` one by one, and for
each element, it finds and handles all matches. Ordinarily, once
part of the text has been fontified already, this cannot be overridden
by a subsequent match in the same text; but you can specify different
behavior using the override element of a subexp-highlighter.

Each element of `font-lock-keywords` should have one of these
forms:

`regexp`
:   Highlight all matches for regexp using
    `font-lock-keyword-face`. For example,

    ```
    ;; Highlight occurrences of the word ‘foo’
    ;; using font-lock-keyword-face.
    "\\<foo\\>"
    ```

    Be careful when composing these regular expressions; a poorly written
    pattern can dramatically slow things down! The function
    `regexp-opt` (see [Regular Expression Functions](#Regexp-Functions)) is useful for calculating
    optimal regular expressions to match several keywords.

`function`
:   Find text by calling function, and highlight the matches
    it finds using `font-lock-keyword-face`.

    When function is called, it receives one argument, the limit of
    the search; it should begin searching at point, and not search beyond the
    limit. It should return non-`nil` if it succeeds, and set the
    match data to describe the match that was found. Returning `nil`
    indicates failure of the search.

    Fontification will call function repeatedly with the same limit,
    and with point where the previous invocation left it, until
    function fails. On failure, function need not reset point
    in any particular way.

    function can also take on the responsibility of performing the
    highlighting of the region between point and the limit argument it
    receives. In that case, it should return `nil`, otherwise
    font-lock will highlight the match described by the match data
    and may call the function again with the same limit.

`(matcher . subexp)`
:   In this kind of element, matcher is either a regular
    expression or a function, as described above. The CDR,
    subexp, specifies which subexpression of matcher should be
    highlighted (instead of the entire text that matcher matched).

    ```
    ;; Highlight the ‘bar’ in each occurrence of ‘fubar’,
    ;; using font-lock-keyword-face.
    ("fu\\(bar\\)" . 1)
    ```

`(matcher . facespec)`
:   In this kind of element, facespec is an expression whose value
    specifies the face to use for highlighting. In the simplest case,
    facespec is a Lisp variable (a symbol) whose value is a face
    name.

    ```
    ;; Highlight occurrences of ‘fubar’,
    ;; using the face which is the value of fubar-face.
    ("fubar" . fubar-face)
    ```

    However, facespec can also evaluate to a list of this form:

    ```
    (subexp
     (face face prop1 val1 prop2 val2...))
    ```

    to specify the face face and various additional text properties
    to put on the text that matches. If you do this, be sure to add the
    other text property names that you set in this way to the value of
    `font-lock-extra-managed-props` so that the properties will also
    be cleared out when they are no longer appropriate. Alternatively,
    you can set the variable `font-lock-unfontify-region-function` to
    a function that clears these properties. See [Other Font Lock Variables](#Other-Font-Lock-Variables).

`(matcher . subexp-highlighter)`
:   In this kind of element, subexp-highlighter is a list
    which specifies how to highlight matches found by matcher.
    It has the form:

    ```
    (subexp facespec [override [laxmatch]])
    ```

    The CAR, subexp, is an integer specifying which subexpression
    of the match to fontify (0 means the entire matching text). The second
    subelement, facespec, is an expression whose value specifies the
    face, as described above.

    The last two values in subexp-highlighter, override and
    laxmatch, are optional flags. If override is `t`,
    this element can override existing fontification made by previous
    elements of `font-lock-keywords`. If it is `keep`, then
    each character is fontified if it has not been fontified already by
    some other element. If it is `prepend`, the face specified by
    facespec is added to the beginning of the `font-lock-face`
    property. If it is `append`, the face is added to the end of the
    `font-lock-face` property.

    If laxmatch is non-`nil`, it means there should be no error
    if there is no subexpression numbered subexp in matcher.
    Obviously, fontification of the subexpression numbered subexp will
    not occur. However, fontification of other subexpressions (and other
    regexps) will continue. If laxmatch is `nil`, and the
    specified subexpression is missing, then an error is signaled which
    terminates search-based fontification.

    Here are some examples of elements of this kind, and what they do:

    ```
    ;; Highlight occurrences of either ‘foo’ or ‘bar’, using
    ;; foo-bar-face, even if they have already been highlighted.
    ;; foo-bar-face should be a variable whose value is a face.
    ("foo\\|bar" 0 foo-bar-face t)

    ;; Highlight the first subexpression within each occurrence
    ;; that the function fubar-match finds,
    ;; using the face which is the value of fubar-face.
    (fubar-match 1 fubar-face)
    ```

`(matcher . anchored-highlighter)`
:   In this kind of element, anchored-highlighter specifies how to
    highlight text that follows a match found by matcher. So a
    match found by matcher acts as the anchor for further searches
    specified by anchored-highlighter. anchored-highlighter
    is a list of the following form:

    ```
    (anchored-matcher pre-form post-form subexp-highlighters...)
    ```

    Here, anchored-matcher, like matcher, is either a regular
    expression or a function. After a match of matcher is found,
    point is at the end of the match. Now, Font Lock evaluates the form
    pre-form. Then it searches for matches of
    anchored-matcher and uses subexp-highlighters to highlight
    these. A subexp-highlighter is as described above. Finally,
    Font Lock evaluates post-form.

    The forms pre-form and post-form can be used to initialize
    before, and cleanup after, anchored-matcher is used. Typically,
    pre-form is used to move point to some position relative to the
    match of matcher, before starting with anchored-matcher.
    post-form might be used to move back, before resuming with
    matcher.

    After Font Lock evaluates pre-form, it does not search for
    anchored-matcher beyond the end of the line. However, if
    pre-form returns a buffer position that is greater than the
    position of point after pre-form is evaluated, then the position
    returned by pre-form is used as the limit of the search instead.
    It is generally a bad idea to return a position greater than the end
    of the line; in other words, the anchored-matcher search should
    not span lines.

    For example,

    ```
    ;; Highlight occurrences of the word ‘item’ following
    ;; an occurrence of the word ‘anchor’ (on the same line)
    ;; in the value of item-face.
    ("\\<anchor\\>" "\\<item\\>" nil nil (0 item-face))
    ```

    Here, pre-form and post-form are `nil`. Therefore
    searching for ‘`item`’ starts at the end of the match of
    ‘`anchor`’, and searching for subsequent instances of ‘`anchor`’
    resumes from where searching for ‘`item`’ concluded.

`(matcher highlighters…)`
:   This sort of element specifies several highlighter lists for a
    single matcher. A highlighter list can be of the type
    subexp-highlighter or anchored-highlighter as described
    above.

    For example,

    ```
    ;; Highlight occurrences of the word ‘anchor’ in the value
    ;; of anchor-face, and subsequent occurrences of the word
    ;; ‘item’ (on the same line) in the value of item-face.
    ("\\<anchor\\>" (0 anchor-face)
                    ("\\<item\\>" nil nil (0 item-face)))
    ```

`(eval . form)`
:   Here form is an expression to be evaluated the first time
    this value of `font-lock-keywords` is used in a buffer.
    Its value should have one of the forms described in this table.

**Warning:** Do not design an element of `font-lock-keywords`
to match text which spans lines; this does not work reliably.
For details, see [Multiline Font Lock Constructs](#Multiline-Font-Lock).

You can use case-fold in `font-lock-defaults` to specify
the value of `font-lock-keywords-case-fold-search` which says
whether search-based fontification should be case-insensitive.

Variable: **font-lock-keywords-case-fold-search** [¶](#index-font_002dlock_002dkeywords_002dcase_002dfold_002dsearch)
:   Non-`nil` means that regular expression matching for the sake of
    `font-lock-keywords` should be case-insensitive.

---

Next: [Other Font Lock Variables](#Other-Font-Lock-Variables), Previous: [Search-based Fontification](#Search_002dbased-Fontification), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.3 Customizing Search-Based Fontification [¶](#Customizing-Search_002dBased-Fontification)

You can use `font-lock-add-keywords` to add additional
search-based fontification rules to a major mode, and
`font-lock-remove-keywords` to remove rules. You can also
customize the `font-lock-ignore` option to selectively disable
fontification rules for keywords that match certain criteria.

Function: **font-lock-add-keywords** mode keywords &optional how [¶](#index-font_002dlock_002dadd_002dkeywords)
:   This function adds highlighting keywords, for the current buffer
    or for major mode mode. The argument keywords should be a
    list with the same format as the variable `font-lock-keywords`.

    If mode is a symbol which is a major mode command name, such as
    `c-mode`, the effect is that enabling Font Lock mode in
    mode will add keywords to `font-lock-keywords`.
    Calling with a non-`nil` value of mode is correct only in
    your `~/.emacs` file.

    If mode is `nil`, this function adds keywords to
    `font-lock-keywords` in the current buffer. This way of calling
    `font-lock-add-keywords` is usually used in mode hook functions.

    By default, keywords are added at the beginning of
    `font-lock-keywords`. If the optional argument how is
    `set`, they are used to replace the value of
    `font-lock-keywords`. If how is any other non-`nil`
    value, they are added at the end of `font-lock-keywords`.

    Some modes provide specialized support you can use in additional
    highlighting patterns. See the variables
    `c-font-lock-extra-types`, `c++-font-lock-extra-types`,
    and `java-font-lock-extra-types`, for example.

    **Warning:** Major mode commands must not call
    `font-lock-add-keywords` under any circumstances, either directly
    or indirectly, except through their mode hooks. (Doing so would lead to
    incorrect behavior for some minor modes.) They should set up their
    rules for search-based fontification by setting
    `font-lock-keywords`.

Function: **font-lock-remove-keywords** mode keywords [¶](#index-font_002dlock_002dremove_002dkeywords)
:   This function removes keywords from `font-lock-keywords`
    for the current buffer or for major mode mode. As in
    `font-lock-add-keywords`, mode should be a major mode
    command name or `nil`. All the caveats and requirements for
    `font-lock-add-keywords` apply here too. The argument
    keywords must exactly match the one used by the corresponding
    `font-lock-add-keywords`.

For example, the following code adds two fontification patterns for C
mode: one to fontify the word ‘`FIXME`’, even in comments, and
another to fontify the words ‘`and`’, ‘`or`’ and ‘`not`’ as
keywords.

```
(font-lock-add-keywords 'c-mode
 '(("\\<\\(FIXME\\):" 1 font-lock-warning-face prepend)
   ("\\<\\(and\\|or\\|not\\)\\>" . font-lock-keyword-face)))
```

This example affects only C mode proper. To add the same patterns to C
mode *and* all modes derived from it, do this instead:

```
(add-hook 'c-mode-hook
 (lambda ()
  (font-lock-add-keywords nil
   '(("\\<\\(FIXME\\):" 1 font-lock-warning-face prepend)
     ("\\<\\(and\\|or\\|not\\)\\>" .
      font-lock-keyword-face)))))
```

User Option: **font-lock-ignore** [¶](#index-font_002dlock_002dignore)
:   This option defines conditions for selectively disabling
    fontifications due to certain Font Lock keywords. If non-`nil`,
    its value is a list of elements of the following form:

    ```
    (symbol condition ...)
    ```

    Here, symbol is a symbol, usually a major or minor mode. The
    subsequent conditions of a symbol’s list element will be in
    effect if symbol is bound and its value is non-`nil`. For
    a mode’s symbol, it means that the current major mode is derived from
    that mode, or that minor mode is enabled in the buffer. When a
    condition is in effect, any fontifications caused by
    `font-lock-keywords` elements that match the condition will
    be disabled.

    Each condition can be one of the following:

    a symbol
    :   This condition matches any element of Font Lock keywords that
        references the symbol. This is usually a face, but can be any symbol
        referenced by an element of the `font-lock-keywords` list. The
        symbol can contain wildcards: `*` matches any string in the
        symbol’s name, `?` matches a single character, and
        `[char-set]`, where char-set is a string of one or
        more characters, matches a single character from the set.

    a string
    :   This condition matches any element of Font Lock keywords whose
        matcher is a regexp which matches the string. In other words,
        this condition matches a Font Lock rule which highlights the string.
        Thus, the string could be a specific program keyword whose
        highlighting you want to disable.

    `(pred function)`
    :   This condition matches any element of Font Lock keywords for which
        function, when called with the element as the argument, returns
        non-`nil`.

    `(not condition)`
    :   This matches if condition doesn’t.

    `(and condition …)`
    :   This matches if each of the conditions matches.

    `(or condition …)`
    :   This matches if at least one of the conditions matches.

    `(except condition)`
    :   This condition can only be used at top level or inside an
        `or` clause. It undoes the effect of a previously matching
        condition on the same level.

As an example, consider the following setting:

```
(setq font-lock-ignore
      '((prog-mode font-lock-*-face
                   (except help-echo))
        (emacs-lisp-mode (except ";;;###autoload)")
        (whitespace-mode whitespace-empty-at-bob-regexp)
        (makefile-mode (except *))))
```

Line by line, this does the following:

1. In all programming modes, disable fontifications due to all font-lock
   keywords that apply one of the standard font-lock faces (excluding
   strings and comments, which are covered by syntactic Font Lock).
2. However, keep any keywords that add a `help-echo` text property.
3. In Emacs Lisp mode, also keep the highlighting of autoload cookies,
   which would have been excluded by the first condition.
4. When `whitespace-mode` (a minor mode) is enabled, also don’t
   highlight an empty line at beginning of buffer.
5. Finally, in Makefile mode, don’t apply any conditions.

---

Next: [Levels of Font Lock](#Levels-of-Font-Lock), Previous: [Customizing Search-Based Fontification](#Customizing-Keywords), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.4 Other Font Lock Variables [¶](#Other-Font-Lock-Variables-1)

This section describes additional variables that a major mode can
set by means of other-vars in `font-lock-defaults`
(see [Font Lock Basics](#Font-Lock-Basics)).

Variable: **font-lock-mark-block-function** [¶](#index-font_002dlock_002dmark_002dblock_002dfunction)
:   If this variable is non-`nil`, it should be a function that is
    called with no arguments, to choose an enclosing range of text for
    refontification for the command `M-x font-lock-fontify-block`.

    The function should report its choice by placing the region around it.
    A good choice is a range of text large enough to give proper results,
    but not too large so that refontification becomes slow. Typical values
    are `mark-defun` for programming modes or `mark-paragraph` for
    textual modes.

Variable: **font-lock-extra-managed-props** [¶](#index-font_002dlock_002dextra_002dmanaged_002dprops)
:   This variable specifies additional properties (other than
    `font-lock-face`) that are being managed by Font Lock mode. It
    is used by `font-lock-default-unfontify-region`, which normally
    only manages the `font-lock-face` property. If you want Font
    Lock to manage other properties as well, you must specify them in a
    facespec in `font-lock-keywords` as well as add them to
    this list. See [Search-based Fontification](#Search_002dbased-Fontification).

Variable: **font-lock-fontify-buffer-function** [¶](#index-font_002dlock_002dfontify_002dbuffer_002dfunction)
:   Function to use for fontifying the buffer. The default value is
    `font-lock-default-fontify-buffer`.

Variable: **font-lock-unfontify-buffer-function** [¶](#index-font_002dlock_002dunfontify_002dbuffer_002dfunction)
:   Function to use for unfontifying the buffer. This is used when
    turning off Font Lock mode. The default value is
    `font-lock-default-unfontify-buffer`.

Variable: **font-lock-fontify-region-function** [¶](#index-font_002dlock_002dfontify_002dregion_002dfunction)
:   Function to use for fontifying a region. It should take two
    arguments, the beginning and end of the region, and an optional third
    argument verbose. If verbose is non-`nil`, the
    function should print status messages. The default value is
    `font-lock-default-fontify-region`.

Variable: **font-lock-unfontify-region-function** [¶](#index-font_002dlock_002dunfontify_002dregion_002dfunction)
:   Function to use for unfontifying a region. It should take two
    arguments, the beginning and end of the region. The default value is
    `font-lock-default-unfontify-region`.

Variable: **font-lock-flush-function** [¶](#index-font_002dlock_002dflush_002dfunction)
:   Function to use for declaring that a region’s fontification is out of
    date. It takes two arguments, the beginning and end of the region.
    The default value of this variable is
    `font-lock-after-change-function`.

Variable: **font-lock-ensure-function** [¶](#index-font_002dlock_002densure_002dfunction)
:   Function to use for making sure a region of the current buffer has
    been fontified. It is called with two arguments, the beginning and
    end of the region. The default value of this variable is a function
    that calls `font-lock-default-fontify-buffer` if the buffer is
    not fontified; the effect is to make sure the entire accessible
    portion of the buffer is fontified.

Function: **jit-lock-register** function &optional contextual [¶](#index-jit_002dlock_002dregister)
:   This function tells Font Lock mode to run the Lisp function
    function any time it has to fontify or refontify part of the
    current buffer. It calls function before calling the default
    fontification functions, and gives it two arguments, start and
    end, which specify the region to be fontified or refontified.
    If function performs fontifications, it can return a list of the
    form `(jit-lock-bounds beg . end)`, to indicate
    the bounds of the region it actually fontified; Just-In-Time (a.k.a.
    “JIT”) font-lock will use
    this information to optimize subsequent redisplay cycles and regions
    of buffer text it will pass to future calls to function.

    The optional argument contextual, if non-`nil`, forces Font
    Lock mode to always refontify a syntactically relevant part of the
    buffer, and not just the modified lines. This argument can usually be
    omitted.

    When Font Lock is activated in a buffer, it calls this function with a
    non-`nil` value of contextual if the value of
    `font-lock-keywords-only` (see [Syntactic Font Lock](#Syntactic-Font-Lock)) is
    `nil`.

Function: **jit-lock-unregister** function [¶](#index-jit_002dlock_002dunregister)
:   If function was previously registered as a fontification
    function using `jit-lock-register`, this function unregisters it.

Command: **jit-lock-debug-mode** &optional arg [¶](#index-jit_002dlock_002ddebug_002dmode)
:   This is a minor mode whose purpose is to help in debugging code that
    is run by JIT font-lock. When this mode is enabled, most of the code
    that JIT font-lock normally runs during redisplay cycles, where Lisp
    errors are suppressed, is instead run by a timer. Thus, this mode
    allows using debugging aids such as `debug-on-error`
    (see [Entering the Debugger on an Error](#Error-Debugging)) and Edebug (see [Edebug](#Edebug)) for finding and
    fixing problems in font-lock code and any other code run by JIT
    font-lock. Another command that could be useful when developing and
    debugging font-lock is `font-lock-debug-fontify`, see [Font Lock Basics](#Font-Lock-Basics).

---

Next: [Precalculated Fontification](#Precalculated-Fontification), Previous: [Other Font Lock Variables](#Other-Font-Lock-Variables), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.5 Levels of Font Lock [¶](#Levels-of-Font-Lock-1)

Some major modes offer three different levels of fontification. You
can define multiple levels by using a list of symbols for keywords
in `font-lock-defaults`. Each symbol specifies one level of
fontification; it is up to the user to choose one of these levels,
normally by setting `font-lock-maximum-decoration` (see [Font
Lock](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Font-Lock) in the GNU Emacs Manual). The chosen level’s symbol value
is used to initialize `font-lock-keywords`.

Here are the conventions for how to define the levels of
fontification:

* Level 1: highlight function declarations, file directives (such as include or
  import directives), strings and comments. The idea is speed, so only
  the most important and top-level components are fontified.
* Level 2: in addition to level 1, highlight all language keywords,
  including type names that act like keywords, as well as named constant
  values. The idea is that all keywords (either syntactic or semantic)
  should be fontified appropriately.
* Level 3: in addition to level 2, highlight the symbols being defined in
  function and variable declarations, and all builtin function names,
  wherever they appear.

---

Next: [Faces for Font Lock](#Faces-for-Font-Lock), Previous: [Levels of Font Lock](#Levels-of-Font-Lock), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.6 Precalculated Fontification [¶](#Precalculated-Fontification-1)

Some major modes such as `list-buffers` and `occur`
construct the buffer text programmatically. The easiest way for them
to support Font Lock mode is to specify the faces of text when they
insert the text in the buffer.

The way to do this is to specify the faces in the text with the
special text property `font-lock-face` (see [Properties with Special Meanings](#Special-Properties)). When Font Lock mode is enabled, this property controls
the display, just like the `face` property. When Font Lock mode
is disabled, `font-lock-face` has no effect on the display.

It is ok for a mode to use `font-lock-face` for some text and
also use the normal Font Lock machinery. But if the mode does not use
the normal Font Lock machinery, it should not set the variable
`font-lock-defaults`. In this case the `face` property will
not be overridden, so using the `face` property could work too.
However, using `font-lock-face` is generally preferable as it
allows the user to control the fontification by toggling
`font-lock-mode`, and lets the code work regardless of whether
the mode uses Font Lock machinery or not.

---

Next: [Syntactic Font Lock](#Syntactic-Font-Lock), Previous: [Precalculated Fontification](#Precalculated-Fontification), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.7 Faces for Font Lock [¶](#Faces-for-Font-Lock-1)

Font Lock mode can highlight using any face, but Emacs defines several
faces specifically for Font Lock to use to highlight text. These
*Font Lock faces* are listed below. They can also be used by major
modes for syntactic highlighting outside of Font Lock mode (see [Major Mode Conventions](#Major-Mode-Conventions)).

The faces are listed with descriptions of their typical usage, and in
order of greater to lesser prominence. If a mode’s syntactic
categories do not fit well with the usage descriptions, the faces can be
assigned using the ordering as a guide.

`font-lock-warning-face` [¶](#index-font_002dlock_002dwarning_002dface)
:   for a construct that is peculiar (e.g., an unescaped confusable quote
    in an Emacs Lisp symbol like ‘`‘foo`’), or that greatly changes the meaning of
    other text, like ‘`;;;###autoload`’ in Emacs Lisp and ‘`#error`’
    in C.

`font-lock-function-name-face` [¶](#index-font_002dlock_002dfunction_002dname_002dface)
:   for the name of a function being defined or declared.

`font-lock-function-call-face` [¶](#index-font_002dlock_002dfunction_002dcall_002dface)
:   for the name of a function being called. This face inherits, by
    default, from `font-lock-function-name-face`.

`font-lock-variable-name-face` [¶](#index-font_002dlock_002dvariable_002dname_002dface)
:   for the name of a variable being defined or declared.

`font-lock-variable-use-face` [¶](#index-font_002dlock_002dvariable_002duse_002dface)
:   for the name of a variable being referenced. This face inherits, by
    default, from `font-lock-variable-name-face`.

`font-lock-keyword-face` [¶](#index-font_002dlock_002dkeyword_002dface)
:   for a keyword with special syntactic significance, like ‘`for`’ and
    ‘`if`’ in C.

`font-lock-comment-face` [¶](#index-font_002dlock_002dcomment_002dface)
:   for comments.

`font-lock-comment-delimiter-face` [¶](#index-font_002dlock_002dcomment_002ddelimiter_002dface)
:   for comments delimiters, like ‘`/*`’ and ‘`*/`’ in C. On most
    terminals, this inherits from `font-lock-comment-face`.

`font-lock-type-face` [¶](#index-font_002dlock_002dtype_002dface)
:   for the names of user-defined data types.

`font-lock-constant-face` [¶](#index-font_002dlock_002dconstant_002dface)
:   for the names of constants, like ‘`NULL`’ in C.

`font-lock-builtin-face` [¶](#index-font_002dlock_002dbuiltin_002dface)
:   for the names of built-in functions.

`font-lock-preprocessor-face` [¶](#index-font_002dlock_002dpreprocessor_002dface)
:   for preprocessor commands. This inherits, by default, from
    `font-lock-builtin-face`.

`font-lock-string-face` [¶](#index-font_002dlock_002dstring_002dface)
:   for string constants.

`font-lock-doc-face` [¶](#index-font_002dlock_002ddoc_002dface)
:   for documentation embedded in program code inside specially-formed
    comments or strings. This face inherits, by default, from
    `font-lock-string-face`.

`font-lock-doc-markup-face` [¶](#index-font_002dlock_002ddoc_002dmarkup_002dface)
:   for mark-up elements in text using `font-lock-doc-face`.
    It is typically used for the mark-up constructs in documentation embedded
    in program code, following conventions such as Haddock, Javadoc or Doxygen.
    This face inherits, by default, from `font-lock-constant-face`.

`font-lock-negation-char-face` [¶](#index-font_002dlock_002dnegation_002dchar_002dface)
:   for easily-overlooked negation characters.

`font-lock-escape-face` [¶](#index-font_002dlock_002descape_002dface)
:   for escape sequences in strings.
    This face inherits, by default, from `font-lock-regexp-grouping-backslash`.

    Here is an example in Python, where the escape sequence `\n` is used:

    ```
    print('Hello world!\n')
    ```

`font-lock-number-face` [¶](#index-font_002dlock_002dnumber_002dface)
:   for numbers.

`font-lock-operator-face` [¶](#index-font_002dlock_002doperator_002dface)
:   for operators.

`font-lock-property-name-face` [¶](#index-font_002dlock_002dproperty_002dname_002dface)
:   for properties of an object, such as the declaration of fields in a
    struct. This face inherits, by default, from
    `font-lock-variable-name-face`.

`font-lock-property-use-face` [¶](#index-font_002dlock_002dproperty_002duse_002dface)
:   for properties of an object, such as use of fields in a struct. This
    face inherits, by default, from `font-lock-property-name-face`.

    For example,

    ```
    typedef struct
    {
      int prop;
    //    ^ property
    } obj;

    int main()
    {
      obj o;
      o.prop = 3;
    //  ^ property
    }
    ```

`font-lock-punctuation-face` [¶](#index-font_002dlock_002dpunctuation_002dface)
:   for punctuation such as brackets and delimiters.

`font-lock-bracket-face` [¶](#index-font_002dlock_002dbracket_002dface)
:   for brackets (e.g., `()`, `[]`, `{}`).
    This face inherits, by default, from `font-lock-punctuation-face`.

`font-lock-delimiter-face` [¶](#index-font_002dlock_002ddelimiter_002dface)
:   for delimiters (e.g., `;`, `:`, `,`).
    This face inherits, by default, from `font-lock-punctuation-face`.

`font-lock-misc-punctuation-face` [¶](#index-font_002dlock_002dmisc_002dpunctuation_002dface)
:   for punctuation that is not a bracket or delimiter.
    This face inherits, by default, from `font-lock-punctuation-face`.

---

Next: [Multiline Font Lock Constructs](#Multiline-Font-Lock), Previous: [Faces for Font Lock](#Faces-for-Font-Lock), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.8 Syntactic Font Lock [¶](#Syntactic-Font-Lock-1)

Syntactic fontification uses a syntax table (see [Syntax Tables](#Syntax-Tables)) to
find and highlight syntactically relevant text. If enabled, it runs
prior to search-based fontification. The variable
`font-lock-syntactic-face-function`, documented below, determines
which syntactic constructs to highlight. There are several variables
that affect syntactic fontification; you should set them by means of
`font-lock-defaults` (see [Font Lock Basics](#Font-Lock-Basics)).

Whenever Font Lock mode performs syntactic fontification on a stretch
of text, it first calls the function specified by
`syntax-propertize-function`. Major modes can use this to apply
`syntax-table` text properties to override the buffer’s syntax
table in special cases. See [Syntax Properties](#Syntax-Properties).

Variable: **font-lock-keywords-only** [¶](#index-font_002dlock_002dkeywords_002donly)
:   If the value of this variable is non-`nil`, Font Lock does not do
    syntactic fontification, only search-based fontification based on
    `font-lock-keywords`; this will usually have the effect of not
    fontifying comments and strings. This variable is normally set by Font
    Lock mode based on the keywords-only element in
    `font-lock-defaults`. If the value is `nil`, Font Lock will
    call `jit-lock-register` (see [Other Font Lock Variables](#Other-Font-Lock-Variables)) to set
    up for automatic refontification of buffer text following a modified
    line to reflect the new syntactic context due to the change.

    To use only syntactic fontification, both this variable and
    `font-lock-keywords` should be set to `nil` (see [Font Lock Basics](#Font-Lock-Basics)).

Variable: **font-lock-syntax-table** [¶](#index-font_002dlock_002dsyntax_002dtable)
:   This variable holds the syntax table to use for fontification of
    comments and strings. It is normally set by Font Lock mode based on the
    syntax-alist element in `font-lock-defaults`. If this value
    is `nil`, syntactic fontification uses the buffer’s syntax table
    (the value returned by the function `syntax-table`; see [Syntax Table Functions](#Syntax-Table-Functions)).

Variable: **font-lock-syntactic-face-function** [¶](#index-font_002dlock_002dsyntactic_002dface_002dfunction)
:   If this variable is non-`nil`, it should be a function to determine
    which face to use for a given syntactic element (a string or a comment).

    The function is called with one argument, the parse state at point
    returned by `parse-partial-sexp`, and should return a face. The
    default value returns `font-lock-comment-face` for comments and
    `font-lock-string-face` for strings (see [Faces for Font Lock](#Faces-for-Font-Lock)).

    This variable is normally set through the “other” elements in
    `font-lock-defaults`:

    ```
    (setq-local font-lock-defaults
                `(,python-font-lock-keywords
                  nil nil nil
                  (font-lock-syntactic-face-function
                   . python-font-lock-syntactic-face-function)))
    ```

---

Next: [Parser-based Font Lock](#Parser_002dbased-Font-Lock), Previous: [Syntactic Font Lock](#Syntactic-Font-Lock), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.9 Multiline Font Lock Constructs [¶](#Multiline-Font-Lock-Constructs)

Normally, elements of `font-lock-keywords` should not match
across multiple lines; that doesn’t work reliably, because Font Lock
usually scans just part of the buffer, and it can miss a multi-line
construct that crosses the line boundary where the scan starts. (The
scan normally starts at the beginning of a line.)

Making elements that match multiline constructs work properly has
two aspects: correct *identification* and correct
*rehighlighting*. The first means that Font Lock finds all
multiline constructs. The second means that Font Lock will correctly
rehighlight all the relevant text when a multiline construct is
changed—for example, if some of the text that was previously part of
a multiline construct ceases to be part of it. The two aspects are
closely related, and often getting one of them to work will appear to
make the other also work. However, for reliable results you must
attend explicitly to both aspects.

There are three ways to ensure correct identification of multiline
constructs:

* Add a function to `font-lock-extend-region-functions` that does
  the *identification* and extends the scan so that the scanned
  text never starts or ends in the middle of a multiline construct.
* Use the `font-lock-fontify-region-function` hook similarly to
  extend the scan so that the scanned text never starts or ends in the
  middle of a multiline construct.
* Somehow identify the multiline construct right when it gets inserted
  into the buffer (or at any point after that but before font-lock
  tries to highlight it), and mark it with a `font-lock-multiline`
  which will instruct font-lock not to start or end the scan in the
  middle of the construct.

There are several ways to do rehighlighting of multiline constructs:

* Place a `font-lock-multiline` property on the construct. This
  will rehighlight the whole construct if any part of it is changed. In
  some cases you can do this automatically by setting the
  `font-lock-multiline` variable, which see.
* Make sure `jit-lock-contextually` is set and rely on it doing its
  job. This will only rehighlight the part of the construct that
  follows the actual change, and will do it after a short delay.
  This only works if the highlighting of the various parts of your
  multiline construct never depends on text in subsequent lines.
  Since `jit-lock-contextually` is activated by default, this can
  be an attractive solution.
* Place a `jit-lock-defer-multiline` property on the construct.
  This works only if `jit-lock-contextually` is used, and with the
  same delay before rehighlighting, but like `font-lock-multiline`,
  it also handles the case where highlighting depends on
  subsequent lines.
* If parsing the *syntax* of a construct depends on it being parsed in one
  single chunk, you can add the `syntax-multiline` text property
  over the construct in question. The most common use for this is when
  the syntax property to apply to ‘`FOO`’ depend on some later text
  ‘`BAR`’: By placing this text property over the whole of
  ‘`FOO...BAR`’, you make sure that any change of ‘`BAR`’ will
  also cause the syntax property of ‘`FOO`’ to be recomputed.
  Note: For this to work, the mode needs to add
  `syntax-propertize-multiline` to
  `syntax-propertize-extend-region-functions`.

* [Font Lock Multiline](#Font-Lock-Multiline)
* [Region to Fontify after a Buffer Change](#Region-to-Refontify)

---

Next: [Region to Fontify after a Buffer Change](#Region-to-Refontify), Up: [Multiline Font Lock Constructs](#Multiline-Font-Lock)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.9.1 Font Lock Multiline [¶](#Font-Lock-Multiline-1)

One way to ensure reliable rehighlighting of multiline Font Lock
constructs is to put on them the text property `font-lock-multiline`.
It should be present and non-`nil` for text that is part of a
multiline construct.

When Font Lock is about to highlight a range of text, it first
extends the boundaries of the range as necessary so that they do not
fall within text marked with the `font-lock-multiline` property.
Then it removes any `font-lock-multiline` properties from the
range, and highlights it. The highlighting specification (mostly
`font-lock-keywords`) must reinstall this property each time,
whenever it is appropriate.

**Warning:** don’t use the `font-lock-multiline` property
on large ranges of text, because that will make rehighlighting slow.

Variable: **font-lock-multiline** [¶](#index-font_002dlock_002dmultiline)
:   If the `font-lock-multiline` variable is set to `t`, Font
    Lock will try to add the `font-lock-multiline` property
    automatically on multiline constructs. This is not a universal
    solution, however, since it slows down Font Lock somewhat. It can
    miss some multiline constructs, or make the property larger or smaller
    than necessary.

    For elements whose matcher is a function, the function should
    ensure that submatch 0 covers the whole relevant multiline construct,
    even if only a small subpart will be highlighted. It is often just as
    easy to add the `font-lock-multiline` property by hand.

The `font-lock-multiline` property is meant to ensure proper
refontification; it does not automatically identify new multiline
constructs. Identifying them requires that Font Lock mode operate on
large enough chunks at a time. This will happen by accident on many
cases, which may give the impression that multiline constructs magically
work. If you set the `font-lock-multiline` variable
non-`nil`, this impression will be even stronger, since the
highlighting of those constructs which are found will be properly
updated from then on. But that does not work reliably.

To find multiline constructs reliably, you must either manually place
the `font-lock-multiline` property on the text before Font Lock
mode looks at it, or use `font-lock-fontify-region-function`.

---

Previous: [Font Lock Multiline](#Font-Lock-Multiline), Up: [Multiline Font Lock Constructs](#Multiline-Font-Lock)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.9.2 Region to Fontify after a Buffer Change [¶](#Region-to-Fontify-after-a-Buffer-Change)

When a buffer is changed, the region that Font Lock refontifies is
by default the smallest sequence of whole lines that spans the change.
While this works well most of the time, sometimes it doesn’t—for
example, when a change alters the syntactic meaning of text on an
earlier line.

You can enlarge (or even reduce) the region to refontify by setting
the following variable:

Variable: **font-lock-extend-after-change-region-function** [¶](#index-font_002dlock_002dextend_002dafter_002dchange_002dregion_002dfunction)
:   This buffer-local variable is either `nil` or a function for Font
    Lock mode to call to determine the region to scan and fontify.

    The function is given three parameters, the standard beg,
    end, and old-len from `after-change-functions`
    (see [Change Hooks](#Change-Hooks)). It should return either a cons of the
    beginning and end buffer positions (in that order) of the region to
    fontify, or `nil` (which means choose the region in the standard
    way). This function needs to preserve point, the match-data, and the
    current restriction. The region it returns may start or end in the
    middle of a line.

    Since this function is called after every buffer change, it should be
    reasonably fast.

---

Previous: [Multiline Font Lock Constructs](#Multiline-Font-Lock), Up: [Font Lock Mode](#Font-Lock-Mode)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.7.10 Parser-based Font Lock [¶](#Parser_002dbased-Font-Lock-1)

Besides simple syntactic font lock and regexp-based font lock, Emacs
also provides complete syntactic font lock with the help of a parser.
Currently, Emacs uses the tree-sitter library (see [Parsing Program Source](#Parsing-Program-Source)) for this purpose.

Parser-based font lock and other font lock mechanisms are not mutually
exclusive. By default, if enabled, parser-based font lock runs first,
replacing syntactic font lock, followed by regexp-based font lock.

Although parser-based font lock doesn’t share the same customization
variables with regexp-based font lock, it uses similar customization
schemes. The tree-sitter counterpart of `font-lock-keywords` is
`treesit-font-lock-settings`.

In general, tree-sitter fontification works as follows:

* A Lisp program (usually, part of a major mode) provides a *query*
  consisting of *patterns*, each pattern associated with a
  *capture name*.
* The tree-sitter library finds the nodes in the parse tree
  that match these patterns, tags the nodes with the corresponding
  capture names, and returns them to the Lisp program.
* The Lisp program uses the returned nodes to highlight the portions of
  buffer text corresponding to each node as appropriate, using the
  tagged capture names of the nodes to determine the correct
  fontification. For example, a node tagged `font-lock-keyword`
  would be highlighted in `font-lock-keyword` face.

For more information about queries, patterns, and capture names, see
[Pattern Matching Tree-sitter Nodes](#Pattern-Matching).

To set up tree-sitter fontification, a major mode should first set
`treesit-font-lock-settings` with the output of
`treesit-font-lock-rules`, then call
`treesit-major-mode-setup`.

Function: **treesit-font-lock-rules** &rest query-specs [¶](#index-treesit_002dfont_002dlock_002drules)
:   This function is used to set `treesit-font-lock-settings`. It
    takes care of compiling queries and other post-processing, and outputs
    a value that `treesit-font-lock-settings` accepts. Here’s an
    example:

    ```
    (treesit-font-lock-rules
     :language 'javascript
     :feature 'constant
     :override t
     '((true) @font-lock-constant-face
       (false) @font-lock-constant-face)
     :language 'html
     :feature 'script
     "(script_element) @font-lock-builtin-face")
    ```

    This function takes a series of query-specs, where each
    query-spec is a query preceded by one or more
    keyword/value pairs. Each query is a tree-sitter
    query in either the string, s-expression, or compiled form.

    For each query, the keyword/value pairs that precede
    it add meta information to it. The `:language` keyword declares
    query’s language. The `:feature` keyword sets the feature
    name of query. Users can control which features are enabled
    with `treesit-font-lock-level` and
    `treesit-font-lock-feature-list` (described below). These two
    keywords are mandatory (with exceptions).

    Other keywords are optional:

    | Keyword | Value | Description |
    | --- | --- | --- |
    | `:override` | `nil` | If the region already has a face, discard the new face |
    |  | `t` | Always apply the new face |
    |  | `append` | Append the new face to existing ones |
    |  | `prepend` | Prepend the new face to existing ones |
    |  | `keep` | Fill-in regions without an existing face |
    | `:reversed` | `t` | Enable query when feature is not in the feature list. |
    | `:default-language` | language | Every query after this keyword will use language by default. |

    Lisp programs mark patterns in query with capture names (names
    that start with `@`), and tree-sitter will return matched nodes
    tagged with those same capture names. For the purpose of
    fontification, capture names in query should be face names like
    `font-lock-keyword-face`. The captured node will be fontified
    with that face.

    A capture name can also be a function name, in which case the function
    is called with 4 arguments: node and override, start
    and end, where node is the node itself, override is
    the `:override` property of the rule which captured this node,
    and start and end limit the region which this function
    should fontify. (If this function wants to respect the override
    argument, it can use `treesit-fontify-with-override`.)

    Beyond the 4 arguments presented, this function should accept more
    arguments as optional arguments for future extensibility.

    If a capture name is both a face and a function, the face takes
    priority. If a capture name is neither a face nor a function, it is
    ignored.

    Sometimes, to support different versions of the same grammar, it’s
    useful to conditionally include some optional query, or choose the
    first valid query from a list of queries. Functions like
    `treesit-query-with-optional` and
    `treesit-query-with-fallback` can come in handy.

Variable: **treesit-font-lock-feature-list** [¶](#index-treesit_002dfont_002dlock_002dfeature_002dlist)
:   This is a list of lists of feature symbols. Each element of the list
    is a list that represents a decoration level.
    The `treesit-font-lock-level` user option controls which levels are
    activated.

    Each element of the list is a list of the form `(feature …)`, where each feature corresponds to the
    `:feature` value of a query defined in
    `treesit-font-lock-rules`. Removing a feature symbol from this
    list disables the corresponding query during font-lock.

    Common feature names, for many programming languages, include
    `definition`, `type`, `assignment`, `builtin`,
    `constant`, `keyword`, `string-interpolation`,
    `comment`, `doc`, `string`, `operator`,
    `preprocessor`, `escape-sequence`, and `key`. Major
    modes are free to subdivide or extend these common features.

    Some of these features warrant some explanation: `definition`
    highlights whatever is being defined, e.g., the function name in a
    function definition, the struct name in a struct definition, the
    variable name in a variable definition; `assignment` highlights
    whatever is being assigned to, e.g., the variable or field in an
    assignment statement; `key` highlights keys in key-value pairs,
    e.g., keys in a JSON object or Python dictionary; `doc`
    highlights docstrings or doc-comments.

    For example, the value of this variable could be:

    ```
    ((comment string doc) ; level 1
     (function-name keyword type builtin constant) ; level 2
     (variable-name string-interpolation key)) ; level 3
    ```

    Major modes should set this variable before calling
    `treesit-major-mode-setup`.

    For this variable to take effect, a Lisp program should call
    `treesit-font-lock-recompute-features` (which resets
    `treesit-font-lock-settings` accordingly), or
    `treesit-major-mode-setup` (which calls
    `treesit-font-lock-recompute-features`).

Variable: **treesit-font-lock-settings** [¶](#index-treesit_002dfont_002dlock_002dsettings)
:   A list of settings for tree-sitter based font lock. The exact format of
    each individual setting is considered internal. One should always use
    `treesit-font-lock-rules` to set this variable.

    Even though the setting object is opaque, Emacs provides accessors for
    the setting’s query, feature, enable flag and override flag:
    `treesit-font-lock-setting-query`,
    `treesit-font-lock-setting-feature`,
    `treesit-font-lock-setting-enable`,
    `treesit-font-lock-setting-override`,
    `treesit-font-lock-setting-reversed`.

Multi-language major modes should provide range functions in
`treesit-range-functions`, and Emacs will set the ranges
accordingly before fontifing a region (see [Parsing Text in Multiple Languages](#Multiple-Languages)).

---

Next: [Desktop Save Mode](#Desktop-Save-Mode), Previous: [Font Lock Mode](#Font-Lock-Mode), Up: [Major and Minor Modes](#Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
