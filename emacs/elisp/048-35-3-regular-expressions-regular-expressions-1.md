### 35.3 Regular Expressions [¶](#Regular-Expressions-1)

A *regular expression*, or *regexp* for short, is a pattern that
denotes a (possibly infinite) set of strings. Searching for matches for
a regexp is a very powerful operation. This section explains how to write
regexps; the following section says how to search for them.

For interactive development of regular expressions, you
can use the `M-x re-builder` command. It provides a convenient
interface for creating regular expressions, by giving immediate visual
feedback in a separate buffer. As you edit the regexp, all its
matches in the target buffer are highlighted. Each parenthesized
sub-expression of the regexp is shown in a distinct face, which makes
it easier to verify even very complex regexps.

Note that by default Emacs search ignores case (see [Searching and Case](#Searching-and-Case)). To enable case-sensitive regexp search and match, bind
`case-fold-search` to `nil` around the code you want to be
case-sensitive.

* [Syntax of Regular Expressions](#Syntax-of-Regexps)
* [Complex Regexp Example](#Regexp-Example)
* [The `rx` Structured Regexp Notation](#Rx-Notation)
* [Regular Expression Functions](#Regexp-Functions)
* [Problems with Regular Expressions](#Regexp-Problems)

---

Next: [Complex Regexp Example](#Regexp-Example), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.1 Syntax of Regular Expressions [¶](#Syntax-of-Regular-Expressions)

Regular expressions have a syntax in which a few characters are
special constructs and the rest are *ordinary*. An ordinary
character is a simple regular expression that matches that character
and nothing else. The special characters are ‘`.`’, ‘`*`’,
‘`+`’, ‘`?`’, ‘`[`’, ‘`^`’, ‘`$`’, and ‘`\`’; no new
special characters will be defined in the future. The character
‘`]`’ is special if it ends a bracket expression (see later).
The character ‘`-`’ is special inside a bracket expression. A
‘`[:`’ and balancing ‘`:]`’ enclose a character class inside a
bracket expression. Any other character appearing in a regular
expression is ordinary, unless a ‘`\`’ precedes it.

For example, ‘`f`’ is not a special character, so it is ordinary, and
therefore ‘`f`’ is a regular expression that matches the string
‘`f`’ and no other string. (It does *not* match the string
‘`fg`’, but it does match a *part* of that string.) Likewise,
‘`o`’ is a regular expression that matches only ‘`o`’.

Any two regular expressions a and b can be concatenated. The
result is a regular expression that matches a string if a matches
some amount of the beginning of that string and b matches the rest of
the string.

As a simple example, we can concatenate the regular expressions ‘`f`’
and ‘`o`’ to get the regular expression ‘`fo`’, which matches only
the string ‘`fo`’. Still trivial. To do something more powerful, you
need to use one of the special regular expression constructs.

* [Special Characters in Regular Expressions](#Regexp-Special)
* [Character Classes](#Char-Classes)
* [Backslash Constructs in Regular Expressions](#Regexp-Backslash)

---

Next: [Character Classes](#Char-Classes), Up: [Syntax of Regular Expressions](#Syntax-of-Regexps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.1.1 Special Characters in Regular Expressions [¶](#Special-Characters-in-Regular-Expressions)

Here is a list of the characters that are special in a regular
expression.

‘`.`’ (Period) [¶](#index-_002e-in-regexp)
:   is a special character that matches any single character except a newline.
    Using concatenation, we can make regular expressions like ‘`a.b`’, which
    matches any three-character string that begins with ‘`a`’ and ends with
    ‘`b`’.

‘`*`’ [¶](#index-_002a-in-regexp)
:   is not a construct by itself; it is a postfix operator that means to
    match the preceding regular expression repetitively as many times as
    possible. Thus, ‘`o*`’ matches any number of ‘`o`’s (including no
    ‘`o`’s).

    ‘`*`’ always applies to the *smallest* possible preceding
    expression. Thus, ‘`fo*`’ has a repeating ‘`o`’, not a repeating
    ‘`fo`’. It matches ‘`f`’, ‘`fo`’, ‘`foo`’, and so on.

    The matcher processes a ‘`*`’ construct by matching, immediately, as
    many repetitions as can be found. Then it continues with the rest of
    the pattern. If that fails, backtracking occurs, discarding some of the
    matches of the ‘`*`’-modified construct in the hope that this will
    make it possible to match the rest of the pattern. For example, in
    matching ‘`ca*ar`’ against the string ‘`caaar`’, the ‘`a*`’
    first tries to match all three ‘`a`’s; but the rest of the pattern is
    ‘`ar`’ and there is only ‘`r`’ left to match, so this try fails.
    The next alternative is for ‘`a*`’ to match only two ‘`a`’s. With
    this choice, the rest of the regexp matches successfully.

‘`+`’ [¶](#index-_002b-in-regexp)
:   is a postfix operator, similar to ‘`*`’ except that it must match
    the preceding expression at least once. So, for example, ‘`ca+r`’
    matches the strings ‘`car`’ and ‘`caaaar`’ but not the string
    ‘`cr`’, whereas ‘`ca*r`’ matches all three strings.

‘`?`’ [¶](#index-_003f-in-regexp)
:   is a postfix operator, similar to ‘`*`’ except that it must match the
    preceding expression either once or not at all. For example,
    ‘`ca?r`’ matches ‘`car`’ or ‘`cr`’; nothing else.

‘`*?`’, ‘`+?`’, ‘`??`’ [¶](#index-non_002dgreedy-repetition-characters-in-regexp)
:   are *non-greedy* variants of the operators ‘`*`’, ‘`+`’
    and ‘`?`’. Where those operators match the largest possible
    substring (consistent with matching the entire containing expression),
    the non-greedy variants match the smallest possible substring
    (consistent with matching the entire containing expression).

    For example, the regular expression ‘`c[ad]*a`’ when applied to the
    string ‘`cdaaada`’ matches the whole string; but the regular
    expression ‘`c[ad]*?a`’, applied to that same string, matches just
    ‘`cda`’. (The smallest possible match here for ‘`[ad]*?`’ that
    permits the whole expression to match is ‘`d`’.)

‘`[ … ]`’ [¶](#index-bracket-expression-_0028in-regexp_0029)
:   is a *bracket expression* (a.k.a. *character alternative*),
    which begins with ‘`[`’ and is terminated by ‘`]`’. In the
    simplest case, the characters between the two brackets are what this
    bracket expression can match.

    Thus, ‘`[ad]`’ matches either one ‘`a`’ or one ‘`d`’, and
    ‘`[ad]*`’ matches any string composed of just ‘`a`’s and ‘`d`’s
    (including the empty string). It follows that ‘`c[ad]*r`’
    matches ‘`cr`’, ‘`car`’, ‘`cdr`’, ‘`caddaar`’, etc.

    You can also include character ranges in a bracket expression, by
    writing the starting and ending characters with a ‘`-`’ between them.
    Thus, ‘`[a-z]`’ matches any lower-case ASCII letter.
    Ranges may be intermixed freely with individual characters, as in
    ‘`[a-z$%.]`’, which matches any lower case ASCII letter
    or ‘`$`’, ‘`%`’ or period. However, the ending character of one
    range should not be the starting point of another one; for example,
    ‘`[a-m-z]`’ should be avoided.

    A bracket expression can also specify named character classes
    (see [Character Classes](#Char-Classes)). For example, ‘`[[:ascii:]]`’ matches any
    ASCII character. Using a character class is equivalent to
    mentioning each of the characters in that class; but the latter is not
    feasible in practice, since some classes include thousands of
    different characters. A character class should not appear as the
    lower or upper bound of a range.

    The usual regexp special characters are not special inside a
    bracket expression. A completely different set of characters is
    special: ‘`]`’, ‘`-`’ and ‘`^`’.
    To include ‘`]`’ in a bracket expression, put it at the
    beginning. To include ‘`^`’, put it anywhere but at the beginning.
    To include ‘`-`’, put it at the end. Thus, ‘`[]^-]`’ matches
    all three of these special characters. You cannot use ‘`\`’ to
    escape these three characters, since ‘`\`’ is not special here.

    The following aspects of ranges are specific to Emacs, in that POSIX
    allows but does not require this behavior and programs other than
    Emacs may behave differently:

    1. If `case-fold-search` is non-`nil`, ‘`[a-z]`’ also
       matches upper-case letters.
    2. A range is not affected by the locale’s collation sequence: it always
       represents the set of characters with codepoints ranging between those
       of its bounds, so that ‘`[a-z]`’ matches only ASCII letters, even
       outside the C or POSIX locale.
    3. If the lower bound of a range is greater than its upper bound, the
       range is empty and represents no characters. Thus, ‘`[z-a]`’
       always fails to match, and ‘`[^z-a]`’ matches any character,
       including newline. However, a reversed range should always be from
       the letter ‘`z`’ to the letter ‘`a`’ to make it clear that it is
       not a typo; for example, ‘`[+-*/]`’ should be avoided, because it
       matches only ‘`/`’ rather than the likely-intended four characters.
    4. If the end points of a range are raw 8-bit bytes (see [Text Representations](#Text-Representations)), or if the range start is ASCII and the end is a raw
       byte (as in ‘`[a-\377]`’), the range will match only ASCII
       characters and raw 8-bit bytes, but not non-ASCII characters. This
       feature is intended for searching text in unibyte buffers and strings.
    5. To search for *raw bytes*, which are characters belonging to the
       `eight-bit` character set (see [Character Sets](#Character-Sets)), you can use
       ‘`[\200-\377]`’ or ‘`[\x3fff80-\x3fffff]`’

    Some kinds of bracket expressions are not the best style even
    though they have a well-defined meaning in Emacs. They include:

    1. Although a range’s bound can be almost any character, it is better
       style to stay within natural sequences of ASCII letters and digits
       because most people have not memorized character code tables.
       For example, ‘`[.-9]`’ is less clear than ‘`[./0-9]`’,
       and ‘`` [`-~] ``’ is less clear than ‘`` [`a-z{|}~] ``’.
       Unicode character escapes can help here; for example, for most programmers
       ‘`[ก-ฺ฿-๛]`’ is less clear than ‘`[\u0E01-\u0E3A\u0E3F-\u0E5B]`’.
    2. Although a bracket expression can include duplicates, it is better
       style to avoid them. For example, ‘`[XYa-yYb-zX]`’ is less clear
       than ‘`[XYa-z]`’.
    3. Although a range can denote just one, two, or three characters, it
       is simpler to list the characters. For example,
       ‘`[a-a0]`’ is less clear than ‘`[a0]`’, ‘`[i-j]`’ is less clear
       than ‘`[ij]`’, and ‘`[i-k]`’ is less clear than ‘`[ijk]`’.
    4. Although a ‘`-`’ can appear at the beginning of a bracket
       expression or as the upper bound of a range, it is better style to
       put ‘`-`’ by itself at the end of a bracket expression. For
       example, although ‘`[-a-z]`’ is valid, ‘`[a-z-]`’ is better
       style; and although ‘`[*--]`’ is valid, ‘`[*+,-]`’ is clearer.

‘`[^ … ]`’ [¶](#index-_005e-in-regexp)
:   ‘`[^`’ begins a *complemented bracket expression*, or
    *complemented character alternative*. This matches any character
    except the ones specified. Thus, ‘`[^a-z0-9A-Z]`’ matches all
    characters *except* ASCII letters and digits.

    ‘`^`’ is not special in a bracket expression unless it is the first
    character. The character following the ‘`^`’ is treated as if it
    were first (in other words, ‘`-`’ and ‘`]`’ are not special there).

    A complemented bracket expression can match a newline, unless newline is
    mentioned as one of the characters not to match. This is in contrast to
    the handling of regexps in programs such as `grep`.

    You can specify named character classes, just like in bracket
    expressions. For instance, ‘`[^[:ascii:]]`’ matches any
    non-ASCII character. See [Character Classes](#Char-Classes).

‘`^`’ [¶](#index-beginning-of-line-in-regexp)
:   When matching a buffer, ‘`^`’ matches the empty string, but only at the
    beginning of a line in the text being matched (or the beginning of the
    accessible portion of the buffer). Otherwise it fails to match
    anything. Thus, ‘`^foo`’ matches a ‘`foo`’ that occurs at the
    beginning of a line.

    When matching a string instead of a buffer, ‘`^`’ matches at the
    beginning of the string or after a newline character.

    For historical compatibility, ‘`^`’ is special only at the beginning
    of the regular expression, or after ‘`\(`’, ‘`\(?:`’ or ‘`\|`’.
    Although ‘`^`’ is an ordinary character in other contexts,
    it is good practice to use ‘`\^`’ even then.

‘`$`’ [¶](#index-_0024-in-regexp)
:   is similar to ‘`^`’ but matches only at the end of a line (or the
    end of the accessible portion of the buffer). Thus, ‘`x+$`’
    matches a string of one ‘`x`’ or more at the end of a line.

    When matching a string instead of a buffer, ‘`$`’ matches at the end
    of the string or before a newline character.

    For historical compatibility, ‘`$`’ is special only at the
    end of the regular expression, or before ‘`\)`’ or ‘`\|`’.
    Although ‘`$`’ is an ordinary character in other contexts,
    it is good practice to use ‘`\$`’ even then.

‘`\`’ [¶](#index-_005c-in-regexp)
:   has two functions: it quotes the special characters (including
    ‘`\`’), and it introduces additional special constructs.

    Because ‘`\`’ quotes special characters, ‘`\$`’ is a regular
    expression that matches only ‘`$`’, and ‘`\[`’ is a regular
    expression that matches only ‘`[`’, and so on.

    Note that ‘`\`’ also has special meaning in the read syntax of Lisp
    strings (see [String Type](#String-Type)), and must be quoted with ‘`\`’. For
    example, the regular expression that matches the ‘`\`’ character is
    ‘`\\`’. To write a Lisp string that contains the characters
    ‘`\\`’, Lisp syntax requires you to quote each ‘`\`’ with another
    ‘`\`’. Therefore, the read syntax for a regular expression matching
    ‘`\`’ is `"\\\\"`.

For historical compatibility, a repetition operator is treated as ordinary
if it appears at the start of a regular expression
or after ‘`^`’, ‘`` \` ``’, ‘`\(`’, ‘`\(?:`’ or ‘`\|`’.
For example, ‘`*foo`’ is treated as ‘`\*foo`’, and
‘`two\|^\{2\}`’ is treated as ‘`two\|^{2}`’.
It is poor practice to depend on this behavior; use proper backslash
escaping anyway, regardless of where the repetition operator appears.

As a ‘`\`’ is not special inside a bracket expression, it can
never remove the special meaning of ‘`-`’, ‘`^`’ or ‘`]`’.
You should not quote these characters when they have no special
meaning. This would not clarify anything, since backslashes
can legitimately precede these characters where they *have*
special meaning, as in ‘`[^\]`’ (`"[^\\]"` for Lisp string
syntax), which matches any single character except a backslash.

In practice, most ‘`]`’ that occur in regular expressions close a
bracket expression and hence are special. However, occasionally a
regular expression may try to match a complex pattern of literal
‘`[`’ and ‘`]`’. In such situations, it sometimes may be
necessary to carefully parse the regexp from the start to determine
which square brackets enclose a bracket expression. For example,
‘`[^][]]`’ consists of the complemented bracket expression
‘`[^][]`’ (which matches any single character that is not a square
bracket), followed by a literal ‘`]`’.

The exact rules are that at the beginning of a regexp, ‘`[`’ is
special and ‘`]`’ not. This lasts until the first unquoted
‘`[`’, after which we are in a bracket expression; ‘`[`’ is
no longer special (except when it starts a character class) but ‘`]`’
is special, unless it immediately follows the special ‘`[`’ or that
‘`[`’ followed by a ‘`^`’. This lasts until the next special
‘`]`’ that does not end a character class. This ends the bracket
expression and restores the ordinary syntax of regular expressions;
an unquoted ‘`[`’ is special again and a ‘`]`’ not.

---

Next: [Backslash Constructs in Regular Expressions](#Regexp-Backslash), Previous: [Special Characters in Regular Expressions](#Regexp-Special), Up: [Syntax of Regular Expressions](#Syntax-of-Regexps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.1.2 Character Classes [¶](#Character-Classes)

Below is a table of the classes you can use in a bracket expression
(see [bracket expression](#Regexp-Special)), and what they mean.
Note that the ‘`[`’ and ‘`]`’ characters that enclose the class
name are part of the name, so a regular expression using these classes
needs one more pair of brackets. For example, a regular expression
matching a sequence of one or more letters and digits would be
‘`[[:alnum:]]+`’, not ‘`[:alnum:]+`’.

‘`[:ascii:]`’
:   This matches any ASCII character (codes 0–127).

‘`[:alnum:]`’
:   This matches any letter or digit. For multibyte characters, it
    matches characters whose Unicode ‘`general-category`’ property
    (see [Character Properties](#Character-Properties)) indicates they are alphabetic or
    decimal number characters.

‘`[:alpha:]`’
:   This matches any letter. For multibyte characters, it matches
    characters whose Unicode ‘`general-category`’ property
    (see [Character Properties](#Character-Properties)) indicates they are alphabetic
    characters.

‘`[:blank:]`’
:   This matches horizontal whitespace, as defined by Annex C of the
    Unicode Technical Standard #18. In particular, it matches spaces,
    tabs, and other characters whose Unicode ‘`general-category`’
    property (see [Character Properties](#Character-Properties)) indicates they are spacing
    separators. (If you only need to look for ASCII whitespace characters,
    we suggest using an explicit set of character alternatives, such as
    ‘`[ \t]`’, instead, as it will be faster than
    `[[:blank:]]`.)

‘`[:cntrl:]`’
:   This matches any character whose code is in the range 0–31.

‘`[:digit:]`’
:   This matches ‘`0`’ through ‘`9`’. Thus, ‘`[-+[:digit:]]`’
    matches any digit, as well as ‘`+`’ and ‘`-`’.

‘`[:graph:]`’
:   This matches graphic characters—everything except spaces,
    ASCII and non-ASCII control characters,
    surrogates, and codepoints unassigned by Unicode, as indicated by the
    Unicode ‘`general-category`’ property (see [Character Properties](#Character-Properties)).

‘`[:lower:]`’
:   This matches any lower-case letter, as determined by the current case
    table (see [The Case Table](#Case-Tables)). If `case-fold-search` is
    non-`nil`, this also matches any upper-case letter. Note that a
    buffer can have its own local case table different from the default
    one.

‘`[:multibyte:]`’
:   This matches any multibyte character (see [Text Representations](#Text-Representations)).

‘`[:nonascii:]`’
:   This matches any non-ASCII character.

‘`[:print:]`’
:   This matches any printing character—either spaces or graphic
    characters matched by ‘`[:graph:]`’.

‘`[:punct:]`’
:   This matches any punctuation character. (At present, for multibyte
    characters, it matches anything that has non-word syntax, and thus its
    exact definition can vary from one major mode to another, since the
    syntax of a character depends on the major mode.)

‘`[:space:]`’
:   This matches any character that has whitespace syntax
    (see [Table of Syntax Classes](#Syntax-Class-Table)). Note that the syntax of a character,
    and thus which characters are considered “whitespace”,
    depends on the major mode.

‘`[:unibyte:]`’
:   This matches any unibyte character (see [Text Representations](#Text-Representations)).

‘`[:upper:]`’
:   This matches any upper-case letter, as determined by the current case
    table (see [The Case Table](#Case-Tables)). If `case-fold-search` is
    non-`nil`, this also matches any lower-case letter. Note that a
    buffer can have its own local case table different from the default
    one.

‘`[:word:]`’
:   This matches any character that has word syntax (see [Table of Syntax Classes](#Syntax-Class-Table)). Note that the syntax of a character, and thus which
    characters are considered “word-constituent”, depends on the major
    mode.

‘`[:xdigit:]`’
:   This matches the hexadecimal digits: ‘`0`’ through ‘`9`’, ‘`a`’
    through ‘`f`’ and ‘`A`’ through ‘`F`’.

The classes ‘`[:space:]`’, ‘`[:word:]`’ and ‘`[:punct:]`’ use
the syntax-table of the current buffer but not any overriding syntax
text properties (see [Syntax Properties](#Syntax-Properties)).

---

Previous: [Character Classes](#Char-Classes), Up: [Syntax of Regular Expressions](#Syntax-of-Regexps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.1.3 Backslash Constructs in Regular Expressions [¶](#Backslash-Constructs-in-Regular-Expressions)

For the most part, ‘`\`’ followed by any character matches only
that character. However, there are several exceptions: certain
sequences starting with ‘`\`’ that have special meanings. Here is
a table of the special ‘`\`’ constructs.

‘`\|`’ [¶](#index-_007c-in-regexp)
:   specifies an alternative.
    Two regular expressions a and b with ‘`\|`’ in
    between form an expression that matches anything that either a or
    b matches.

    Thus, ‘`foo\|bar`’ matches either ‘`foo`’ or ‘`bar`’
    but no other string.

    ‘`\|`’ applies to the largest possible surrounding expressions. Only a
    surrounding ‘`\( … \)`’ grouping can limit the grouping power of
    ‘`\|`’.

    If you need full backtracking capability to handle multiple uses of
    ‘`\|`’, use the POSIX regular expression functions (see [Emacs versus POSIX Regular Expressions](#POSIX-Regexps)).

‘`\{m\}`’
:   is a postfix operator that repeats the previous pattern exactly m
    times. Thus, ‘`x\{5\}`’ matches the string ‘`xxxxx`’
    and nothing else. ‘`c[ad]\{3\}r`’ matches string such as
    ‘`caaar`’, ‘`cdddr`’, ‘`cadar`’, and so on.

‘`\{m,n\}`’
:   is a more general postfix operator that specifies repetition with a
    minimum of m repeats and a maximum of n repeats. If m
    is omitted, the minimum is 0; if n is omitted, there is no
    maximum. For both forms, m and n, if specified, may be no
    larger than
    2\*\*16 − 1
    .

    For example, ‘`c[ad]\{1,2\}r`’ matches the strings ‘`car`’,
    ‘`cdr`’, ‘`caar`’, ‘`cadr`’, ‘`cdar`’, and ‘`cddr`’, and
    nothing else.  
    ‘`\{0,1\}`’ or ‘`\{,1\}`’ is equivalent to ‘`?`’.  
    ‘`\{0,\}`’ or ‘`\{,\}`’ is equivalent to ‘`*`’.  
    ‘`\{1,\}`’ is equivalent to ‘`+`’.

‘`\( … \)`’ [¶](#index-_0028-in-regexp)
:   is a grouping construct that serves three purposes:

    1. To enclose a set of ‘`\|`’ alternatives for other operations. Thus,
       the regular expression ‘`\(foo\|bar\)x`’ matches either ‘`foox`’
       or ‘`barx`’.
    2. To enclose a complicated expression for the postfix operators ‘`*`’,
       ‘`+`’ and ‘`?`’ to operate on. Thus, ‘`ba\(na\)*`’ matches
       ‘`ba`’, ‘`bana`’, ‘`banana`’, ‘`bananana`’, etc., with any
       number (zero or more) of ‘`na`’ strings.
    3. To record a matched substring for future reference with
       ‘`\digit`’ (see below).

    This last application is not a consequence of the idea of a
    parenthetical grouping; it is a separate feature that was assigned as a
    second meaning to the same ‘`\( … \)`’ construct because, in
    practice, there was usually no conflict between the two meanings. But
    occasionally there is a conflict, and that led to the introduction of
    shy groups.

‘`\(?: … \)`’ [¶](#index-shy-groups)
:   is the *shy group* construct. A shy group serves the first two
    purposes of an ordinary group (controlling the nesting of other
    operators), but it does not get a number, so you cannot refer back to
    its value with ‘`\digit`’. Shy groups are particularly
    useful for mechanically-constructed regular expressions, because they
    can be added automatically without altering the numbering of ordinary,
    non-shy groups.

    Shy groups are also called *non-capturing* or *unnumbered
    groups*.

‘`\(?num: … \)`’
:   is the *explicitly numbered group* construct. Normal groups get
    their number implicitly, based on their position, which can be
    inconvenient. This construct allows you to force a particular group
    number. There is no particular restriction on the numbering,
    e.g., you can have several groups with the same number in which case
    the last one to match (i.e., the rightmost match) will win.
    Implicitly numbered groups always get the smallest integer larger than
    the one of any previous group.

‘`\digit`’
:   matches the same text that matched the digitth occurrence of a
    grouping (‘`\( … \)`’) construct.

    In other words, after the end of a group, the matcher remembers the
    beginning and end of the text matched by that group. Later on in the
    regular expression you can use ‘`\`’ followed by digit to
    match that same text, whatever it may have been.

    The strings matching the first nine grouping constructs appearing in
    the entire regular expression passed to a search or matching function
    are assigned numbers 1 through 9 in the order that the open
    parentheses appear in the regular expression. So you can use
    ‘`\1`’ through ‘`\9`’ to refer to the text matched by the
    corresponding grouping constructs.

    For example, ‘`\(.*\)\1`’ matches any newline-free string that is
    composed of two identical halves. The ‘`\(.*\)`’ matches the first
    half, which may be anything, but the ‘`\1`’ that follows must match
    the same exact text.

    If a ‘`\( … \)`’ construct matches more than once (which can
    happen, for instance, if it is followed by ‘`*`’), only the last
    match is recorded.

    If a particular grouping construct in the regular expression was never
    matched—for instance, if it appears inside of an alternative that
    wasn’t used, or inside of a repetition that repeated zero times—then
    the corresponding ‘`\digit`’ construct never matches
    anything. To use an artificial example, ‘`\(foo\(b*\)\|lose\)\2`’
    cannot match ‘`lose`’: the second alternative inside the larger
    group matches it, but then ‘`\2`’ is undefined and can’t match
    anything. But it can match ‘`foobb`’, because the first
    alternative matches ‘`foob`’ and ‘`\2`’ matches ‘`b`’.

‘`\w`’ [¶](#index-_005cw-in-regexp)
:   matches any word-constituent character. The editor syntax table
    determines which characters these are. See [Syntax Tables](#Syntax-Tables).

‘`\W`’ [¶](#index-_005cW-in-regexp)
:   matches any character that is not a word constituent.

‘`\scode`’ [¶](#index-_005cs-in-regexp)
:   matches any character whose syntax is code. Here code is a
    character that represents a syntax code: thus, ‘`w`’ for word
    constituent, ‘`-`’ for whitespace, ‘`(`’ for open parenthesis,
    etc. To represent whitespace syntax, use either ‘`-`’ or a space
    character. See [Table of Syntax Classes](#Syntax-Class-Table), for a list of syntax codes and
    the characters that stand for them.

‘`\Scode`’ [¶](#index-_005cS-in-regexp)
:   matches any character whose syntax is not code.

‘`\ccode`’ [¶](#index-category_002c-regexp-search-for)
:   matches any character whose category is code. Here code
    is a character that represents a category: for example, in the standard
    category table, ‘`c`’ stands for Chinese characters and ‘`g`’
    stands for Greek characters. You can see the list of all the
    currently defined categories with `M-x describe-categories RET`. You can also define your own categories in addition to
    the standard ones using the `define-category` function
    (see [Categories](#Categories)).

‘`\Ccode`’
:   matches any character whose category is not code.

The following regular expression constructs match the empty string—that is,
they don’t consume any characters—but whether they match depends on the
context. For all, the beginning and end of the accessible portion of
the buffer are treated as if they were the actual beginning and end of
the buffer.

‘`` \` ``’ [¶](#index-_005c_0060-in-regexp)
:   matches the empty string, but only at the beginning
    of the buffer or string being matched against.

‘`\'`’ [¶](#index-_005c_0027-in-regexp)
:   matches the empty string, but only at the end of
    the buffer or string being matched against.

‘`\=`’ [¶](#index-_005c_003d-in-regexp)
:   matches the empty string, but only at point.
    (This construct is not defined when matching against a string.)

‘`\b`’ [¶](#index-_005cb-in-regexp)
:   matches the empty string, but only at the beginning or
    end of a word. Thus, ‘`\bfoo\b`’ matches any occurrence of
    ‘`foo`’ as a separate word. ‘`\bballs?\b`’ matches
    ‘`ball`’ or ‘`balls`’ as a separate word.

    ‘`\b`’ matches at the beginning or end of the buffer (or string)
    regardless of what text appears next to it.

‘`\B`’ [¶](#index-_005cB-in-regexp)
:   matches the empty string, but *not* at the beginning or
    end of a word, nor at the beginning or end of the buffer (or string).

‘`\<`’ [¶](#index-_005c_003c-in-regexp)
:   matches the empty string, but only at the beginning of a word.
    ‘`\<`’ matches at the beginning of the buffer (or string) only if a
    word-constituent character follows.

‘`\>`’ [¶](#index-_005c_003e-in-regexp)
:   matches the empty string, but only at the end of a word. ‘`\>`’
    matches at the end of the buffer (or string) only if the contents end
    with a word-constituent character.

‘`\_<`’ [¶](#index-_005c_005f_003c-in-regexp)
:   matches the empty string, but only at the beginning of a symbol. A
    symbol is a sequence of one or more word or symbol constituent
    characters. ‘`\_<`’ matches at the beginning of the buffer (or
    string) only if a symbol-constituent character follows.

‘`\_>`’ [¶](#index-_005c_005f_003e-in-regexp)
:   matches the empty string, but only at the end of a symbol. ‘`\_>`’
    matches at the end of the buffer (or string) only if the contents end
    with a symbol-constituent character.

Not every string is a valid regular expression. For example, a string
that ends inside a bracket expression without a terminating ‘`]`’
is invalid, and so is a string that ends with a single ‘`\`’. If
an invalid regular expression is passed to any of the search functions,
an `invalid-regexp` error is signaled.

---

Next: [The `rx` Structured Regexp Notation](#Rx-Notation), Previous: [Syntax of Regular Expressions](#Syntax-of-Regexps), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.2 Complex Regexp Example [¶](#Complex-Regexp-Example)

Here is a complicated regexp which was formerly used by Emacs to
recognize the end of a sentence together with any whitespace that
follows. (Nowadays Emacs uses a similar but more complex default
regexp constructed by the function `sentence-end`.
See [Standard Regular Expressions Used in Editing](#Standard-Regexps).)

Below, we show first the regexp as a string in Lisp syntax (to
distinguish spaces from tab characters), and then the result of
evaluating it. The string constant begins and ends with a
double-quote. ‘`\"`’ stands for a double-quote as part of the
string, ‘`\\`’ for a backslash as part of the string, ‘`\t`’ for a
tab and ‘`\n`’ for a newline.

```
"[.?!][]\"')}]*\\($\\| $\\|\t\\|  \\)[ \t\n]*"
     ⇒ "[.?!][]\"')}]*\\($\\| $\\|  \\|  \\)[
]*"
```

In the output, tab and newline appear as themselves.

This regular expression contains four parts in succession and can be
deciphered as follows:

`[.?!]`
:   The first part of the pattern is a bracket expression that matches
    any one of three characters: period, question mark, and exclamation
    mark. The match must begin with one of these three characters. (This
    is one point where the new default regexp used by Emacs differs from
    the old. The new value also allows some non-ASCII
    characters that end a sentence without any following whitespace.)

`[]\"')}]*`
:   The second part of the pattern matches any closing braces and quotation
    marks, zero or more of them, that may follow the period, question mark
    or exclamation mark. The `\"` is Lisp syntax for a double-quote in
    a string. The ‘`*`’ at the end indicates that the immediately
    preceding regular expression (a bracket expression, in this case) may be
    repeated zero or more times.

`\\($\\| $\\|\t\\|  \\)`
:   The third part of the pattern matches the whitespace that follows the
    end of a sentence: the end of a line (optionally with a space), or a
    tab, or two spaces. The double backslashes mark the parentheses and
    vertical bars as regular expression syntax; the parentheses delimit a
    group and the vertical bars separate alternatives. The dollar sign is
    used to match the end of a line.

`[ \t\n]*`
:   Finally, the last part of the pattern matches any additional whitespace
    beyond the minimum needed to end a sentence.

In the `rx` notation (see [The `rx` Structured Regexp Notation](#Rx-Notation)), the regexp could be written

```
(rx (any ".?!")                    ; Punctuation ending sentence.
    (zero-or-more (any "\"')]}"))  ; Closing quotes or brackets.
    (or line-end
        (seq " " line-end)
        "\t"
        "  ")                      ; Two spaces.
    (zero-or-more (any "\t\n ")))  ; Optional extra whitespace.
```

Since `rx` regexps are just S-expressions, they can be formatted
and commented as such.

---

Next: [Regular Expression Functions](#Regexp-Functions), Previous: [Complex Regexp Example](#Regexp-Example), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.3 The `rx` Structured Regexp Notation [¶](#The-rx-Structured-Regexp-Notation)

As an alternative to the string-based syntax, Emacs provides the
structured `rx` notation based on Lisp S-expressions. This
notation is usually easier to read, write and maintain than regexp
strings, and can be indented and commented freely. It requires a
conversion into string form since that is what regexp functions
expect, but that conversion typically takes place during
byte-compilation rather than when the Lisp code using the regexp is
run.

Here is an `rx` regexp[29](#FOOT29) that matches a block comment in the C
programming language:

```
(rx "/*"                            ; Initial /*
    (zero-or-more
     (or (not "*")                  ;  Either non-*,
         (seq (one-or-more "*")     ;  or some * followed by
              (not (or "*" "/"))))) ;     neither * nor /
    (one-or-more "*")               ; At least one star,
    "/")                            ; and the final /
```

or, using shorter synonyms and written more compactly,

```
(rx "/*"
    (* (| (not "*")
          (: (+ "*") (not (in "*/")))))
    (+ "*") "/")
```

In conventional string syntax, it would be written

```
"/\\*\\(?:[^*]\\|\\*+[^*/]\\)*\\*+/"
```

The `rx` notation is mainly useful in Lisp code; it cannot be
used in most interactive situations where a regexp is requested, such
as when running `query-replace-regexp` or in variable
customization.

* [Constructs in `rx` regexps](#Rx-Constructs)
* [Functions and macros using `rx` regexps](#Rx-Functions)
* [Defining new `rx` forms](#Extending-Rx)

---

Next: [Functions and macros using `rx` regexps](#Rx-Functions), Up: [The `rx` Structured Regexp Notation](#Rx-Notation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.3.1 Constructs in `rx` regexps [¶](#Constructs-in-rx-regexps)

The various forms in `rx` regexps are described below. The
shorthand rx represents any `rx` form. rx…
means zero or more `rx` forms and, unless stated otherwise,
matches these forms in sequence as if wrapped in a `(seq …)`
subform.

These are all valid arguments to the `rx` macro. All forms are
defined by their described semantics; the corresponding string regexps
are provided for ease of understanding only. A, B, …
denote (suitably bracketed) string regexp subexpressions therein.

#### Literals [¶](#Literals)

`"some-string"`
:   Match the string ‘`some-string`’ literally. There are no
    characters with special meaning, unlike in string regexps.

`?C`
:   Match the character ‘`C`’ literally.

#### Sequence and alternative [¶](#Sequence-and-alternative)

`(seq rx…)` [¶](#index-and-in-rx)

`(sequence rx…)`

`(: rx…)`

`(and rx…)`
:   Match the rxs in sequence. Without arguments, the expression
    matches the empty string.  
    Corresponding string regexp: ‘`AB…`’
    (subexpressions in sequence).

`(or rx…)` [¶](#index-_007c-in-rx)

`(| rx…)`
:   Match exactly one of the rxs.
    If all arguments are strings, characters, or `or` forms
    so constrained, the longest possible match will always be used.
    Otherwise, either the longest match or the
    first (in left-to-right order) will be used.
    Without arguments, the expression will not match anything at all.  
    Corresponding string regexp: ‘`A\|B\|…`’.

`unmatchable` [¶](#index-unmatchable-in-rx)
:   Refuse any match. Equivalent to `(or)`.
    See [regexp-unmatchable](#regexp_002dunmatchable).

#### Repetition [¶](#Repetition)

Normally, repetition forms are greedy, in that they attempt to match
as many times as possible. Some forms are non-greedy; they try to
match as few times as possible (see [Non-greedy repetition](#Non_002dgreedy-repetition)).

`(zero-or-more rx…)` [¶](#index-0_002b-in-rx)

`(0+ rx…)`
:   Match the rxs zero or more times. Greedy by default.  
    Corresponding string regexp: ‘`A*`’ (greedy),
    ‘`A*?`’ (non-greedy)

`(one-or-more rx…)` [¶](#index-1_002b-in-rx)

`(1+ rx…)`
:   Match the rxs one or more times. Greedy by default.  
    Corresponding string regexp: ‘`A+`’ (greedy),
    ‘`A+?`’ (non-greedy)

`(zero-or-one rx…)` [¶](#index-opt-in-rx)

`(optional rx…)`

`(opt rx…)`
:   Match the rxs once or an empty string. Greedy by default.  
    Corresponding string regexp: ‘`A?`’ (greedy),
    ‘`A??`’ (non-greedy).

`(* rx…)` [¶](#index-_002a-in-rx)
:   Match the rxs zero or more times. Greedy.  
    Corresponding string regexp: ‘`A*`’

`(+ rx…)` [¶](#index-_002b-in-rx)
:   Match the rxs one or more times. Greedy.  
    Corresponding string regexp: ‘`A+`’

`(? rx…)` [¶](#index-_003f-in-rx)
:   Match the rxs once or an empty string. Greedy.  
    Corresponding string regexp: ‘`A?`’

`(*? rx…)` [¶](#index-_002a_003f-in-rx)
:   Match the rxs zero or more times. Non-greedy.  
    Corresponding string regexp: ‘`A*?`’

`(+? rx…)` [¶](#index-_002b_003f-in-rx)
:   Match the rxs one or more times. Non-greedy.  
    Corresponding string regexp: ‘`A+?`’

`(?? rx…)` [¶](#index-_003f_003f-in-rx)
:   Match the rxs or an empty string. Non-greedy.  
    Corresponding string regexp: ‘`A??`’

`(= n rx…)`

`(repeat n rx)`
:   Match the rxs exactly n times.  
    Corresponding string regexp: ‘`A\{n\}`’

`(>= n rx…)` [¶](#index-_003e_003d-in-rx)
:   Match the rxs n or more times. Greedy.  
    Corresponding string regexp: ‘`A\{n,\}`’

`(** n m rx…)` [¶](#index-repeat-in-rx)

`(repeat n m rx…)`
:   Match the rxs at least n but no more than m times. Greedy.  
    Corresponding string regexp: ‘`A\{n,m\}`’

The greediness of some repetition forms can be controlled using the
following constructs. However, it is usually better to use the
explicit non-greedy forms above when such matching is required.

`(minimal-match rx)` [¶](#index-minimal_002dmatch-in-rx)
:   Match rx, with `zero-or-more`, `0+`,
    `one-or-more`, `1+`, `zero-or-one`, `opt` and
    `optional` using non-greedy matching.

`(maximal-match rx)` [¶](#index-maximal_002dmatch-in-rx)
:   Match rx, with `zero-or-more`, `0+`,
    `one-or-more`, `1+`, `zero-or-one`, `opt` and
    `optional` using greedy matching. This is the default.

#### Matching single characters [¶](#Matching-single-characters)

`(any set…)` [¶](#index-in-in-rx)

`(char set…)`

`(in set…)`
:   Match a single character from one of the sets. Each set
    is a character, a string representing the set of its characters, a
    range or a character class (see below). A range is either a
    hyphen-separated string like `"A-Z"`, or a cons of characters
    like `(?A . ?Z)`.

    Note that hyphen (`-`) is special in strings in this construct,
    since it acts as a range separator. To include a hyphen, add it as a
    separate character or single-character string.  
    Corresponding string regexp: ‘`[…]`’

`(not charspec)` [¶](#index-not-in-rx)
:   Match a character not included in charspec. charspec can
    be a character, a single-character string, an `any`, `not`,
    `or`, `intersection`, `syntax` or `category` form,
    or a character class.
    If charspec is an `or` form, its arguments have the same
    restrictions as those of `intersection`; see below.  
    Corresponding string regexp: ‘`[^…]`’, ‘`\Scode`’,
    ‘`\Ccode`’

`(intersection charset…)` [¶](#index-intersection-in-rx)
:   Match a character included in all of the charsets.
    Each charset can be a character, a single-character string, an
    `any` form without character classes, or an `intersection`,
    `or` or `not` form whose arguments are also charsets.

`not-newline`, `nonl` [¶](#index-not_002dnewline-in-rx)
:   Match any character except a newline.  
    Corresponding string regexp: ‘`.`’ (dot)

`anychar`, `anything` [¶](#index-anychar-in-rx)
:   Match any character.  
    Corresponding string regexp: ‘`.\|\n`’ (for example)

character class [¶](#index-character-class-in-rx-1)
:   Match a character from a named character class:

    `alpha`, `alphabetic`, `letter`
    :   Match alphabetic characters. More precisely, match characters whose
        Unicode ‘`general-category`’ property indicates that they are
        alphabetic.

    `alnum`, `alphanumeric`
    :   Match alphabetic characters and digits. More precisely, match
        characters whose Unicode ‘`general-category`’ property indicates
        that they are alphabetic or decimal digits.

    `digit`, `numeric`, `num`
    :   Match the digits ‘`0`’–‘`9`’.

    `xdigit`, `hex-digit`, `hex`
    :   Match the hexadecimal digits ‘`0`’–‘`9`’, ‘`A`’–‘`F`’
        and ‘`a`’–‘`f`’.

    `cntrl`, `control`
    :   Match any character whose code is in the range 0–31.

    `blank`
    :   Match horizontal whitespace. More precisely, match characters whose
        Unicode ‘`general-category`’ property indicates that they are
        spacing separators.

    `space`, `whitespace`, `white`
    :   Match any character that has whitespace syntax
        (see [Table of Syntax Classes](#Syntax-Class-Table)).

    `lower`, `lower-case`
    :   Match anything lower-case, as determined by the current case table.
        If `case-fold-search` is non-`nil`, this also matches any
        upper-case letter.

    `upper`, `upper-case`
    :   Match anything upper-case, as determined by the current case table.
        If `case-fold-search` is non-`nil`, this also matches any
        lower-case letter.

    `graph`, `graphic`
    :   Match any character except whitespace, ASCII and
        non-ASCII control characters, surrogates, and codepoints
        unassigned by Unicode, as indicated by the Unicode
        ‘`general-category`’ property.

    `print`, `printing`
    :   Match whitespace or a character matched by `graph`.

    `punct`, `punctuation`
    :   Match any punctuation character. (At present, for multibyte
        characters, anything that has non-word syntax.)

    `word`, `wordchar`
    :   Match any character that has word syntax (see [Table of Syntax Classes](#Syntax-Class-Table)).

    `ascii`
    :   Match any ASCII character (codes 0–127).

    `nonascii`
    :   Match any non-ASCII character (but not raw bytes).

    The classes `space`, `word` and `punct` use the
    syntax-table of the current buffer but not any overriding syntax text
    properties (see [Syntax Properties](#Syntax-Properties)).  
    Corresponding string regexp: ‘`[[:class:]]`’

`(syntax syntax)` [¶](#index-syntax-in-rx)
:   Match a character with syntax syntax, being one of the following
    names:

    | Syntax name | Syntax character |
    | --- | --- |
    | `whitespace` | `-` |
    | `punctuation` | `.` |
    | `word` | `w` |
    | `symbol` | `_` |
    | `open-parenthesis` | `(` |
    | `close-parenthesis` | `)` |
    | `expression-prefix` | `'` |
    | `string-quote` | `"` |
    | `paired-delimiter` | `$` |
    | `escape` | `\` |
    | `character-quote` | `/` |
    | `comment-start` | `<` |
    | `comment-end` | `>` |
    | `string-delimiter` | `|` |
    | `comment-delimiter` | `!` |

    For details, see [Table of Syntax Classes](#Syntax-Class-Table). Please note that
    `(syntax punctuation)` is *not* equivalent to the character class
    `punctuation`.  
    Corresponding string regexp: ‘`\schar`’ where char is the
    syntax character.

`(category category)` [¶](#index-category-in-rx)
:   Match a character in category category, which is either one of
    the names below or its category character.

    | Category name | Category character |
    | --- | --- |
    | `space-for-indent` | space |
    | `base` | `.` |
    | `consonant` | `0` |
    | `base-vowel` | `1` |
    | `upper-diacritical-mark` | `2` |
    | `lower-diacritical-mark` | `3` |
    | `tone-mark` | `4` |
    | `symbol` | `5` |
    | `digit` | `6` |
    | `vowel-modifying-diacritical-mark` | `7` |
    | `vowel-sign` | `8` |
    | `semivowel-lower` | `9` |
    | `not-at-end-of-line` | `<` |
    | `not-at-beginning-of-line` | `>` |
    | `alpha-numeric-two-byte` | `A` |
    | `chinese-two-byte` | `C` |
    | `greek-two-byte` | `G` |
    | `japanese-hiragana-two-byte` | `H` |
    | `indian-two-byte` | `I` |
    | `japanese-katakana-two-byte` | `K` |
    | `strong-left-to-right` | `L` |
    | `korean-hangul-two-byte` | `N` |
    | `strong-right-to-left` | `R` |
    | `cyrillic-two-byte` | `Y` |
    | `combining-diacritic` | `^` |
    | `ascii` | `a` |
    | `arabic` | `b` |
    | `chinese` | `c` |
    | `ethiopic` | `e` |
    | `greek` | `g` |
    | `korean` | `h` |
    | `indian` | `i` |
    | `japanese` | `j` |
    | `japanese-katakana` | `k` |
    | `latin` | `l` |
    | `lao` | `o` |
    | `tibetan` | `q` |
    | `japanese-roman` | `r` |
    | `thai` | `t` |
    | `vietnamese` | `v` |
    | `hebrew` | `w` |
    | `cyrillic` | `y` |
    | `can-break` | `|` |

    For more information about currently defined categories, run the
    command `M-x describe-categories RET`. For how to define
    new categories, see [Categories](#Categories).  
    Corresponding string regexp: ‘`\cchar`’ where char is the
    category character.

#### Zero-width assertions [¶](#Zero_002dwidth-assertions)

These all match the empty string, but only in specific places.

`line-start`, `bol` [¶](#index-line_002dstart-in-rx)
:   Match at the beginning of a line.  
    Corresponding string regexp: ‘`^`’

`line-end`, `eol` [¶](#index-line_002dend-in-rx)
:   Match at the end of a line.  
    Corresponding string regexp: ‘`$`’

`string-start`, `bos`, `buffer-start`, `bot` [¶](#index-string_002dstart-in-rx)
:   Match at the start of the string or buffer being matched against.  
    Corresponding string regexp: ‘`` \` ``’

`string-end`, `eos`, `buffer-end`, `eot` [¶](#index-string_002dend-in-rx)
:   Match at the end of the string or buffer being matched against.  
    Corresponding string regexp: ‘`\'`’

`point` [¶](#index-point-in-rx)
:   Match at point.  
    Corresponding string regexp: ‘`\=`’

`word-start`, `bow` [¶](#index-word_002dstart-in-rx)
:   Match at the beginning of a word.  
    Corresponding string regexp: ‘`\<`’

`word-end`, `eow` [¶](#index-word_002dend-in-rx)
:   Match at the end of a word.  
    Corresponding string regexp: ‘`\>`’

`word-boundary` [¶](#index-word_002dboundary-in-rx)
:   Match at the beginning or end of a word.  
    Corresponding string regexp: ‘`\b`’

`not-word-boundary` [¶](#index-not_002dword_002dboundary-in-rx)
:   Match anywhere but at the beginning or end of a word.  
    Corresponding string regexp: ‘`\B`’

`symbol-start` [¶](#index-symbol_002dstart-in-rx)
:   Match at the beginning of a symbol.  
    Corresponding string regexp: ‘`\_<`’

`symbol-end` [¶](#index-symbol_002dend-in-rx)
:   Match at the end of a symbol.  
    Corresponding string regexp: ‘`\_>`’

#### Capture groups [¶](#Capture-groups)

`(group rx…)` [¶](#index-submatch-in-rx)

`(submatch rx…)`
:   Match the rxs, making the matched text and position accessible
    in the match data. The first group in a regexp is numbered 1;
    subsequent groups will be numbered one above the previously
    highest-numbered group in the pattern so far.  
    Corresponding string regexp: ‘`\(…\)`’

`(group-n n rx…)` [¶](#index-submatch_002dn-in-rx)

`(submatch-n n rx…)`
:   Like `group`, but explicitly assign the group number n.
    n must be positive.  
    Corresponding string regexp: ‘`\(?n:…\)`’

`(backref n)` [¶](#index-backref-in-rx)
:   Match the text previously matched by group number n.
    n must be in the range 1–9.  
    Corresponding string regexp: ‘`\n`’

#### Dynamic inclusion [¶](#Dynamic-inclusion)

`(literal expr)` [¶](#index-literal-in-rx)
:   Match the literal string that is the result from evaluating the Lisp
    expression expr. The evaluation takes place at call time, in
    the current lexical environment.

`(regexp expr)` [¶](#index-regex-in-rx)

`(regex expr)`
:   Match the string regexp that is the result from evaluating the Lisp
    expression expr. The evaluation takes place at call time, in
    the current lexical environment.

`(eval expr)` [¶](#index-eval-in-rx)
:   Match the rx form that is the result from evaluating the Lisp
    expression expr. The evaluation takes place at macro-expansion
    time for `rx`, at call time for `rx-to-string`,
    in the current global environment.

---

Next: [Defining new `rx` forms](#Extending-Rx), Previous: [Constructs in `rx` regexps](#Rx-Constructs), Up: [The `rx` Structured Regexp Notation](#Rx-Notation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.3.2 Functions and macros using `rx` regexps [¶](#Functions-and-macros-using-rx-regexps)

Macro: **rx** rx-form… [¶](#index-rx-1)
:   Translate the rx-forms to a string regexp, as if they were the
    body of a `(seq …)` form. The `rx` macro expands to a
    string constant, or, if `literal` or `regexp` forms are
    used, a Lisp expression that evaluates to a string. Example:

    ```
    (rx (+ alpha) "=" (+ digit))
      ⇒ "[[:alpha:]]+=[[:digit:]]+"
    ```

Function: **rx-to-string** rx-expr &optional no-group [¶](#index-rx_002dto_002dstring)
:   Translate rx-expr to a string regexp which is returned.
    If no-group is absent or `nil`, bracket the result in a
    non-capturing group, ‘`\(?:…\)`’, if necessary to ensure that
    a postfix operator appended to it will apply to the whole expression.
    Example:

    ```
    (rx-to-string '(seq (+ alpha) "=" (+ digit)) t)
      ⇒ "[[:alpha:]]+=[[:digit:]]+"
    ```

    Arguments to `literal` and `regexp` forms in rx-expr
    must be string literals.

The `pcase` macro can use `rx` expressions as patterns
directly; see [rx in pcase](#rx-in-pcase).

For mechanisms to add user-defined extensions to the `rx`
notation, see [Defining new `rx` forms](#Extending-Rx).

---

Previous: [Functions and macros using `rx` regexps](#Rx-Functions), Up: [The `rx` Structured Regexp Notation](#Rx-Notation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.3.3 Defining new `rx` forms [¶](#Defining-new-rx-forms)

The `rx` notation can be extended by defining new symbols and
parameterized forms in terms of other `rx` expressions. This is
handy for sharing parts between several regexps, and for making
complex ones easier to build and understand by putting them together
from smaller pieces.

For example, you could define `name` to mean
`(one-or-more letter)`, and `(quoted x)` to mean
`(seq ?' x ?')` for any x. These forms could then be
used in `rx` expressions like any other: `(rx (quoted name))`
would match a nonempty sequence of letters inside single quotes.

The Lisp macros below provide different ways of binding names to
definitions. Common to all of them are the following rules:

* Built-in `rx` forms, like `digit` and `group`, cannot
  be redefined.
* The definitions live in a name space of their own, separate from that
  of Lisp variables. There is thus no need to attach a suffix like
  `-regexp` to names; they cannot collide with anything else.
* Definitions cannot refer to themselves recursively, directly or
  indirectly. If you find yourself needing this, you want a parser, not
  a regular expression.
* Definitions are only ever expanded in calls to `rx` or
  `rx-to-string`, not merely by their presence in definition
  macros. This means that the order of definitions doesn’t matter, even
  when they refer to each other, and that syntax errors only show up
  when they are used, not when they are defined.
* User-defined forms are allowed wherever arbitrary `rx`
  expressions are expected; for example, in the body of a
  `zero-or-one` form, but not inside `any` or `category`
  forms. They are also allowed inside `not` and
  `intersection` forms.

Macro: **rx-define** name [arglist] rx-form [¶](#index-rx_002ddefine)
:   Define name globally in all subsequent calls to `rx` and
    `rx-to-string`. If arglist is absent, then name is
    defined as a plain symbol to be replaced with rx-form. Example:

    ```
    (rx-define haskell-comment (seq "--" (zero-or-more nonl)))
    (rx haskell-comment)
         ⇒ "--.*"
    ```

    If arglist is present, it must be a list of zero or more
    argument names, and name is then defined as a parameterized form.
    When used in an `rx` expression as `(name arg…)`,
    each arg will replace the corresponding argument name inside
    rx-form.

    arglist may end in `&rest` and one final argument name,
    denoting a rest parameter. The rest parameter will expand to all
    extra actual argument values not matched by any other parameter in
    arglist, spliced into rx-form where it occurs. Example:

    ```
    (rx-define moan (x y &rest r) (seq x (one-or-more y) r "!"))
    (rx (moan "MOO" "A" "MEE" "OW"))
         ⇒ "MOOA+MEEOW!"
    ```

    Since the definition is global, it is recommended to give name a
    package prefix to avoid name clashes with definitions elsewhere, as is
    usual when naming non-local variables and functions.

    Forms defined this way only perform simple template substitution.
    For arbitrary computations, use them together with the `rx`
    forms `eval`, `regexp` or `literal`. Example:

    ```
    (defun n-tuple-rx (n element)
      `(seq "<"
            (group-n 1 ,element)
            ,@(mapcar (lambda (i) `(seq ?, (group-n ,i ,element)))
                      (number-sequence 2 n))
            ">"))
    (rx-define n-tuple (n element) (eval (n-tuple-rx n 'element)))
    (rx (n-tuple 3 (+ (in "0-9"))))
      ⇒ "<\\(?1:[0-9]+\\),\\(?2:[0-9]+\\),\\(?3:[0-9]+\\)>"
    ```

Macro: **rx-let** (bindings…) body… [¶](#index-rx_002dlet)
:   Make the `rx` definitions in bindings available locally for
    `rx` macro invocations in body, which is then evaluated.

    Each element of bindings is on the form
    `(name [arglist] rx-form)`, where the parts
    have the same meaning as in `rx-define` above. Example:

    ```
    (rx-let ((comma-separated (item) (seq item (0+ "," item)))
             (number (1+ digit))
             (numbers (comma-separated number)))
      (re-search-forward (rx "(" numbers ")")))
    ```

    The definitions are only available during the macro-expansion of
    body, and are thus not present during execution of compiled
    code.

    `rx-let` can be used not only inside a function, but also at top
    level to include global variable and function definitions that need
    to share a common set of `rx` forms. Since the names are local
    inside body, there is no need for any package prefixes.
    Example:

    ```
    (rx-let ((phone-number (seq (opt ?+) (1+ (any digit ?-)))))
      (defun find-next-phone-number ()
        (re-search-forward (rx phone-number)))
      (defun phone-number-p (string)
        (string-match-p (rx bos phone-number eos) string)))
    ```

    The scope of the `rx-let` bindings is lexical, which means that
    they are not visible outside body itself, even in functions
    called from body.

Macro: **rx-let-eval** bindings body… [¶](#index-rx_002dlet_002deval)
:   Evaluate bindings to a list of bindings as in `rx-let`,
    and evaluate body with those bindings in effect for calls
    to `rx-to-string`.

    This macro is similar to `rx-let`, except that the bindings
    argument is evaluated (and thus needs to be quoted if it is a list
    literal), and the definitions are substituted at run time, which is
    required for `rx-to-string` to work. Example:

    ```
    (rx-let-eval
        '((ponder (x) (seq "Where have all the " x " gone?")))
      (looking-at (rx-to-string
                   '(ponder (or "flowers" "young girls"
                                "left socks")))))
    ```

    Another difference from `rx-let` is that the bindings are
    dynamically scoped, and thus also available in functions called from
    body. However, they are not visible inside functions defined in
    body.

---

Next: [Problems with Regular Expressions](#Regexp-Problems), Previous: [The `rx` Structured Regexp Notation](#Rx-Notation), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.4 Regular Expression Functions [¶](#Regular-Expression-Functions)

These functions operate on regular expressions.

Function: **regexp-quote** string [¶](#index-regexp_002dquote)
:   This function returns a regular expression whose only exact match is
    string. Using this regular expression in `looking-at` will
    succeed only if the next characters in the buffer are string;
    using it in a search function will succeed if the text being searched
    contains string. See [Regular Expression Searching](#Regexp-Search).

    This allows you to request an exact string match or search when calling
    a function that wants a regular expression.

    ```
    (regexp-quote "^The cat$")
         ⇒ "\\^The cat\\$"
    ```

    One use of `regexp-quote` is to combine an exact string match with
    context described as a regular expression. For example, this searches
    for the string that is the value of string, surrounded by
    whitespace:

    ```
    (re-search-forward
     (concat "\\s-" (regexp-quote string) "\\s-"))
    ```

    The returned string may be string itself if it does not contain
    any special characters.

Function: **regexp-opt** strings &optional paren [¶](#index-regexp_002dopt)
:   This function returns an efficient regular expression that will match
    any of the strings in the list strings. This is useful when you
    need to make matching or searching as fast as possible—for example,
    for Font Lock mode[30](#FOOT30).

    If strings is the empty list, the return value is a regexp that
    never matches anything.

    The optional argument paren can be any of the following:

    a string
    :   The resulting regexp is preceded by paren and followed by
        ‘`\)`’. For example, use ‘`"\\(?1:"`’ to produce an explicitly
        numbered group.

    `words`
    :   The resulting regexp is surrounded by ‘`\<\(`’ and ‘`\)\>`’.

    `symbols`
    :   The resulting regexp is surrounded by ‘`\_<\(`’ and ‘`\)\_>`’
        (this is often appropriate when matching programming-language
        keywords and the like).

    non-`nil`
    :   The resulting regexp is surrounded by ‘`\(`’ and ‘`\)`’.

    `nil`
    :   The resulting regexp is surrounded by ‘`\(?:`’ and ‘`\)`’,
        if it is necessary to ensure that a postfix operator appended to
        it will apply to the whole expression.

    The returned regexp is ordered in such a way that it will always match
    the longest string possible.

    Up to reordering, the resulting regexp of `regexp-opt` is
    equivalent to but usually more efficient than that of a simplified
    version:

    ```
    (defun simplified-regexp-opt (strings &optional paren)
     (let ((parens
            (cond
             ((stringp paren)       (cons paren "\\)"))
             ((eq paren 'words)    '("\\<\\(" . "\\)\\>"))
             ((eq paren 'symbols) '("\\_<\\(" . "\\)\\_>"))
             ((null paren)          '("\\(?:" . "\\)"))
             (t                       '("\\(" . "\\)")))))
       (concat (car parens)
               (mapconcat 'regexp-quote strings "\\|")
               (cdr parens))))
    ```

Function: **regexp-opt-depth** regexp [¶](#index-regexp_002dopt_002ddepth)
:   This function returns the total number of grouping constructs
    (parenthesized expressions) in regexp. This does not include
    shy groups (see [Backslash Constructs in Regular Expressions](#Regexp-Backslash)).

Function: **regexp-opt-charset** chars [¶](#index-regexp_002dopt_002dcharset)
:   This function returns a regular expression matching a character in the
    list of characters chars.

    ```
    (regexp-opt-charset '(?a ?b ?c ?d ?e))
         ⇒ "[a-e]"
    ```

Variable: **regexp-unmatchable** [¶](#index-regexp_002dunmatchable)
:   This variable contains a regexp that is guaranteed not to match any
    string at all. It is particularly useful as default value for
    variables that may be set to a pattern that actually matches
    something.

---

Previous: [Regular Expression Functions](#Regexp-Functions), Up: [Regular Expressions](#Regular-Expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 35.3.5 Problems with Regular Expressions [¶](#Problems-with-Regular-Expressions)

The Emacs regexp implementation, like many of its kind, is generally
robust but occasionally causes trouble in either of two ways: matching
may run out of internal stack space and signal an error, and it can
take a long time to complete. The advice below will make these
symptoms less likely and help alleviate problems that do arise.

* Anchor regexps at the beginning of a line, string or buffer using
  zero-width assertions (‘`^`’ and `` \` ``). This takes advantage
  of fast paths in the implementation and can avoid futile matching
  attempts. Other zero-width assertions may also bring benefits by
  causing a match to fail early.
* Avoid or-patterns in favor of bracket expressions: write
  ‘`[ab]`’ instead of ‘`a\|b`’. Recall that ‘`\s-`’ and ‘`\sw`’
  are equivalent to ‘`[[:space:]]`’ and ‘`[[:word:]]`’, respectively,
  most of the time.
* Since the last branch of an or-pattern does not add a backtrack point
  on the stack, consider putting the most likely matched pattern last.
  For example, ‘`^\(?:a\|.b\)*c`’ will run out of stack if trying to
  match a very long string of ‘`a`’s, but the equivalent
  ‘`^\(?:.b\|a\)*c`’ will not.

  (It is a trade-off: successfully matched or-patterns run faster with
  the most frequently matched pattern first.)
* Try to ensure that any part of the text can only match in a single
  way. For example, ‘`a*a*`’ will match the same set of strings as
  ‘`a*`’, but the former can do so in many ways and will therefore
  cause slow backtracking if the match fails later on. Make or-pattern
  branches mutually exclusive if possible, so that matching will not go
  far into more than one branch before failing.

  Be especially careful with nested repetitions: they can easily result
  in very slow matching in the presence of ambiguities. For example,
  ‘`\(?:a*b*\)+c`’ will take a long time attempting to match even a
  moderately long string of ‘`a`’s before failing. The equivalent
  ‘`\(?:a\|b\)*c`’ is much faster, and ‘`[ab]*c`’ better still.
* Don’t use capturing groups unless they are really needed; that is, use
  ‘`\(?:…\)`’ instead of ‘`\(…\)`’ for bracketing
  purposes.
* Consider using `rx` (see [The `rx` Structured Regexp Notation](#Rx-Notation)); it can optimize some
  or-patterns automatically and will never introduce capturing groups
  unless explicitly requested.

If you run into regexp stack overflow despite following the above
advice, don’t be afraid of performing the matching in multiple
function calls, each using a simpler regexp where backtracking can
more easily be contained.

Function: **re--describe-compiled** regexp &optional raw [¶](#index-re_002d_002ddescribe_002dcompiled)
:   To help diagnose problems in your regexps or in the regexp engine
    itself, this function returns a string describing the compiled
    form of regexp. To make sense of it, it can be necessary
    to read at least the description of the `re_opcode_t` type in the
    `src/regex-emacs.c` file in Emacs’s source code.

    It is currently able to give a meaningful description only if Emacs
    was compiled with `--enable-checking`.

---

Next: [Longest-match searching for regular expression matches](#Longest-Match), Previous: [Regular Expressions](#Regular-Expressions), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 35.4 Regular Expression Searching [¶](#Regular-Expression-Searching)

In GNU Emacs, you can search for the next match for a regular
expression (see [Syntax of Regular Expressions](#Syntax-of-Regexps)) either incrementally or not.
For incremental search commands, see [Regular
Expression Search](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexp-Search) in The GNU Emacs Manual. Here we describe
only the search functions useful in programs. The principal one is
`re-search-forward`.

These search functions convert the regular expression to multibyte if
the buffer is multibyte; they convert the regular expression to unibyte
if the buffer is unibyte. See [Text Representations](#Text-Representations).

Command: **re-search-forward** regexp &optional limit noerror count [¶](#index-re_002dsearch_002dforward)
:   This function searches forward in the current buffer for a string of
    text that is matched by the regular expression regexp. The
    function skips over any amount of text that is not matched by
    regexp, and leaves point at the end of the first match found.
    It returns the new value of point.

    If limit is non-`nil`, it must be a position in the current
    buffer. It specifies the upper bound to the search. No match
    extending after that position is accepted. If limit is omitted
    or `nil`, it defaults to the end of the accessible portion of the
    buffer.

    What `re-search-forward` does when the search fails depends on
    the value of noerror:

    `nil`
    :   Signal a `search-failed` error.

    `t`
    :   Do nothing and return `nil`.

    anything else
    :   Move point to limit (or the end of the accessible portion of the
        buffer) and return `nil`.

    The argument noerror only affects valid searches which fail to
    find a match. Invalid arguments cause errors regardless of
    noerror.

    If count is a positive number n, the search is done
    n times; each successive search starts at the end of the
    previous match. If all these successive searches succeed, the
    function call succeeds, moving point and returning its new value.
    Otherwise the function call fails, with results depending on the value
    of noerror, as described above. If count is a negative
    number −n, the search is done n times in the opposite
    (backward) direction.

    In the following example, point is initially before the ‘`T`’.
    Evaluating the search call moves point to the end of that line (between
    the ‘`t`’ of ‘`hat`’ and the newline).

    ```
    ---------- Buffer: foo ----------
    I read "∗The cat in the hat
    comes back" twice.
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (re-search-forward "[a-z]+" nil t 5)
         ⇒ 27

    ---------- Buffer: foo ----------
    I read "The cat in the hat∗
    comes back" twice.
    ---------- Buffer: foo ----------
    ```

Command: **re-search-backward** regexp &optional limit noerror count [¶](#index-re_002dsearch_002dbackward)
:   This function searches backward in the current buffer for a string of
    text that is matched by the regular expression regexp, leaving
    point at the beginning of the first text found.

    This function is analogous to `re-search-forward`, but they are not
    simple mirror images. `re-search-forward` finds the match whose
    beginning is as close as possible to the starting point. If
    `re-search-backward` were a perfect mirror image, it would find the
    match whose end is as close as possible. However, in fact it finds the
    match whose beginning is as close as possible (and yet ends before the
    starting point). The reason for this is that matching a regular
    expression at a given spot always works from beginning to end, and
    starts at a specified beginning position.

    A true mirror-image of `re-search-forward` would require a special
    feature for matching regular expressions from end to beginning. It’s
    not worth the trouble of implementing that.

Function: **string-match** regexp string &optional start inhibit-modify [¶](#index-string_002dmatch)
:   This function returns the index of the start of the first match for
    the regular expression regexp in string, or `nil` if
    there is no match. If start is non-`nil`, the search starts
    at that index in string.

    For example,

    ```
    (string-match
     "quick" "The quick brown fox jumped quickly.")
         ⇒ 4
    ```

    ```
    (string-match
     "quick" "The quick brown fox jumped quickly." 8)
         ⇒ 27
    ```

    The index of the first character of the
    string is 0, the index of the second character is 1, and so on.

    By default, if this function finds a match, the index of the first
    character beyond the match is available as `(match-end 0)`.
    See [The Match Data](#Match-Data). If inhibit-modify is non-`nil`, the
    match data isn’t modified.

    ```
    (string-match
     "quick" "The quick brown fox jumped quickly." 8)
         ⇒ 27
    ```

    ```

    ```

    ```
    (match-end 0)
         ⇒ 32
    ```

Function: **string-match-p** regexp string &optional start [¶](#index-string_002dmatch_002dp)
:   This predicate function does what `string-match` does, but it
    avoids modifying the match data.

Function: **looking-at** regexp &optional inhibit-modify [¶](#index-looking_002dat)
:   This function determines whether the text in the current buffer directly
    following point matches the regular expression regexp. “Directly
    following” means precisely that: the search is “anchored” and it can
    succeed only starting with the first character following point. The
    result is `t` if so, `nil` otherwise.

    This function does not move point, but it does update the match data
    (if inhibit-modify is `nil` or missing, which is the
    default). See [The Match Data](#Match-Data). As a convenience, instead of using the
    inhibit-modify argument, you can use `looking-at-p`,
    described below.

    In this example, point is located directly before the ‘`T`’. If it
    were anywhere else, the result would be `nil`.

    ```
    ---------- Buffer: foo ----------
    I read "∗The cat in the hat
    comes back" twice.
    ---------- Buffer: foo ----------

    (looking-at "The cat in the hat$")
         ⇒ t
    ```

Function: **looking-back** regexp limit &optional greedy [¶](#index-looking_002dback)
:   This function returns `t` if regexp matches the text
    immediately before point (i.e., ending at point), and `nil` otherwise.

    Because regular expression matching works only going forward, this is
    implemented by searching backwards from point for a match that ends at
    point. That can be quite slow if it has to search a long distance.
    You can bound the time required by specifying a non-`nil` value
    for limit, which says not to search before limit. In this
    case, the match that is found must begin at or after limit.
    Here’s an example:

    ```
    ---------- Buffer: foo ----------
    I read "∗The cat in the hat
    comes back" twice.
    ---------- Buffer: foo ----------

    (looking-back "read \"" 3)
         ⇒ t
    (looking-back "read \"" 4)
         ⇒ nil
    ```

    If greedy is non-`nil`, this function extends the match
    backwards as far as possible, stopping when a single additional
    previous character cannot be part of a match for regexp. When
    the match is extended, its starting position is allowed to occur
    before limit.

    As a general recommendation, try to avoid using `looking-back`
    wherever possible, since it is slow. For this reason, there are no
    plans to add a `looking-back-p` function.

Function: **looking-at-p** regexp [¶](#index-looking_002dat_002dp)
:   This predicate function works like `looking-at`, but without
    updating the match data.

Variable: **search-spaces-regexp** [¶](#index-search_002dspaces_002dregexp)
:   If this variable is non-`nil`, it should be a regular expression
    that says how to search for whitespace. In that case, any group of
    spaces in a regular expression being searched for stands for use of
    this regular expression. However, spaces inside of constructs such as
    ‘`[…]`’ and ‘`*`’, ‘`+`’, ‘`?`’ are not affected by
    `search-spaces-regexp`.

    Since this variable affects all regular expression search and match
    constructs, you should bind it temporarily for as small as possible
    a part of the code, and only where the Lisp code affected by the
    binding performs searches whose regexp was produced from interactive
    user input. In other words, this variable should only be used to tell
    regexp search primitives how to interpret whitespace typed by the
    user.

---

Next: [The Match Data](#Match-Data), Previous: [Regular Expression Searching](#Regexp-Search), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 35.5 Longest-match searching for regular expression matches [¶](#Longest_002dmatch-searching-for-regular-expression-matches)

The usual regular expression functions do backtracking when necessary
to handle the ‘`\|`’ and repetition constructs, but they continue
this only until they find *some* match. Then they succeed and
report the first match found.

This section describes alternative search functions which perform the
full backtracking specified by the POSIX standard for regular expression
matching. They continue backtracking until they have tried all
possibilities and found all matches, so they can report the longest
match, as required by POSIX. This is much slower, so use these
functions only when you really need the longest match.

Despite their names, the POSIX search and match functions
use Emacs regular expressions, not POSIX regular expressions.
See [Emacs versus POSIX Regular Expressions](#POSIX-Regexps). Also, they do not properly support the
non-greedy repetition operators (see [non-greedy](#Regexp-Special)).
This is because POSIX backtracking conflicts with the semantics of
non-greedy repetition.

Command: **posix-search-forward** regexp &optional limit noerror count [¶](#index-posix_002dsearch_002dforward)
:   This is like `re-search-forward` except that it performs the full
    backtracking specified by the POSIX standard for regular expression
    matching.

Command: **posix-search-backward** regexp &optional limit noerror count [¶](#index-posix_002dsearch_002dbackward)
:   This is like `re-search-backward` except that it performs the full
    backtracking specified by the POSIX standard for regular expression
    matching.

Function: **posix-looking-at** regexp &optional inhibit-modify [¶](#index-posix_002dlooking_002dat)
:   This is like `looking-at` except that it performs the full
    backtracking specified by the POSIX standard for regular expression
    matching.

Function: **posix-string-match** regexp string &optional start inhibit-modify [¶](#index-posix_002dstring_002dmatch)
:   This is like `string-match` except that it performs the full
    backtracking specified by the POSIX standard for regular expression
    matching.

---

Next: [Search and Replace](#Search-and-Replace), Previous: [Longest-match searching for regular expression matches](#Longest-Match), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
