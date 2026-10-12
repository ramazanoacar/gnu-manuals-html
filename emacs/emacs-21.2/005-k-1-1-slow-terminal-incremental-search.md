### K.1.1 Slow Terminal Incremental Search

Incremental search on a slow terminal uses a modified style of display
that is designed to take less time. Instead of redisplaying the buffer at
each place the search gets to, it creates a new single-line window and uses
that to display the line that the search has found. The single-line window
comes into play as soon as point moves outside of the text that is already
on the screen.

When you terminate the search, the single-line window is removed.
Emacs then redisplays the window in which the search was done, to show
its new position of point.

The slow terminal style of display is used when the terminal baud rate is
less than or equal to the value of the variable `search-slow-speed`,
initially 1200. See `baud-rate` in [J.12 Customization of Display](emacs.html#SEC89).

The number of lines to use in slow terminal search display is controlled
by the variable `search-slow-window-lines`. Its normal value is 1.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC93) ] | [ [>](emacs.html#SEC95) ] | [ [<<](emacs.html#SEC92) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.2 Nonincremental Search

Emacs also has conventional nonincremental search commands, which require
you to type the entire search string before searching begins.

`C-s RET string RET`: Search for string. `C-r RET string RET`: Search backward for string.

To do a nonincremental search, first type `C-s RET`. This
enters the minibuffer to read the search string; terminate the string
with `RET`, and then the search takes place. If the string is not
found, the search command signals an error.

When you type `C-s RET`, the `C-s` invokes incremental
search as usual. That command is specially programmed to invoke
nonincremental search, `search-forward`, if the string you
specify is empty. (Such an empty argument would otherwise be
useless.) But it does not call `search-forward` right away. First
it checks the next input character to see if is `C-w`,
which specifies a word search.
See section [K.3 Word Search](emacs.html#SEC95).
`C-r RET` does likewise, for a reverse incremental search.

Forward and backward nonincremental searches are implemented by the
commands `search-forward` and `search-backward`. These
commands may be bound to keys in the usual manner. The feature that you
can get to them via the incremental search commands exists for
historical reasons, and to avoid the need to find key sequences
for them.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC94) ] | [ [>](emacs.html#SEC96) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.3 Word Search

Word search searches for a sequence of words without regard to how the
words are separated. More precisely, you type a string of many words,
using single spaces to separate them, and the string can be found even
if there are multiple spaces, newlines, or other punctuation characters
between these words.

Word search is useful for editing a printed document made with a text
formatter. If you edit while looking at the printed, formatted version,
you can't tell where the line breaks are in the source file. With word
search, you can search without having to know them.

`C-s RET C-w words RET`: Search for words, ignoring details of punctuation. `C-r RET C-w words RET`: Search backward for words, ignoring details of punctuation.

Word search is a special case of nonincremental search and is invoked
with `C-s RET C-w`. This is followed by the search string,
which must always be terminated with `RET`. Being nonincremental,
this search does not start until the argument is terminated. It works
by constructing a regular expression and searching for that; see
[K.4 Regular Expression Search](emacs.html#SEC96).

Use `C-r RET C-w` to do backward word search.

Forward and backward word searches are implemented by the commands
`word-search-forward` and `word-search-backward`. These
commands may be bound to keys in the usual manner. They are available
via the incremental search commands both for historical reasons and
to avoid the need to find suitable key sequences for them.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC95) ] | [ [>](emacs.html#SEC97) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.4 Regular Expression Search

A *regular expression* (*regexp*, for short) is a pattern
that denotes a class of alternative strings to match, possibly
infinitely many. GNU Emacs provides both incremental and
nonincremental ways to search for a match for a regexp.

Incremental search for a regexp is done by typing `C-M-s`
(`isearch-forward-regexp`), or by invoking `C-s` with a
prefix argument (whose value does not matter). This command reads a
search string incrementally just like `C-s`, but it treats the
search string as a regexp rather than looking for an exact match
against the text in the buffer. Each time you add text to the search
string, you make the regexp longer, and the new regexp is searched
for. To search backward for a regexp, use `C-M-r`
(`isearch-backward-regexp`), or `C-r` with a prefix argument.

All of the control characters that do special things within an
ordinary incremental search have the same function in incremental regexp
search. Typing `C-s` or `C-r` immediately after starting the
search retrieves the last incremental search regexp used; that is to
say, incremental regexp and non-regexp searches have independent
defaults. They also have separate search rings that you can access with
`M-p` and `M-n`.

If you type `SPC` in incremental regexp search, it matches any
sequence of whitespace characters, including newlines. If you want
to match just a space, type `C-q SPC`.

Note that adding characters to the regexp in an incremental regexp
search can make the cursor move back and start again. For example, if
you have searched for `` `foo' `` and you add `` `\|bar' ``, the cursor
backs up in case the first `` `bar' `` precedes the first `` `foo' ``.

Nonincremental search for a regexp is done by the functions
`re-search-forward` and `re-search-backward`. You can invoke
these with `M-x`, or bind them to keys, or invoke them by way of
incremental regexp search with `C-M-s RET` and `C-M-r
RET`.

If you use the incremental regexp search commands with a prefix
argument, they perform ordinary string search, like
`isearch-forward` and `isearch-backward`. See section [K.1 Incremental Search](emacs.html#SEC92).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC96) ] | [ [>](emacs.html#SEC98) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.5 Syntax of Regular Expressions

Regular expressions have a syntax in which a few characters are
special constructs and the rest are *ordinary*. An ordinary
character is a simple regular expression which matches that same
character and nothing else. The special characters are `` `$' ``,
`` `^' ``, `` `.' ``, `` `*' ``, `` `+' ``, `` `?' ``, `` `[' ``, `` `]' `` and
`` `\' ``. Any other character appearing in a regular expression is
ordinary, unless a `` `\' `` precedes it. (When you use regular
expressions in a Lisp program, each `` `\' `` must be doubled, see the
example near the end of this section.)

For example, `` `f' `` is not a special character, so it is ordinary, and
therefore `` `f' `` is a regular expression that matches the string
`` `f' `` and no other string. (It does *not* match the string
`` `ff' ``.) Likewise, `` `o' `` is a regular expression that matches
only `` `o' ``. (When case distinctions are being ignored, these regexps
also match `` `F' `` and `` `O' ``, but we consider this a generalization
of "the same string," rather than an exception.)

Any two regular expressions a and b can be concatenated. The
result is a regular expression which matches a string if a matches
some amount of the beginning of that string and b matches the rest of
the string.

As a simple example, we can concatenate the regular expressions `` `f' ``
and `` `o' `` to get the regular expression `` `fo' ``, which matches only
the string `` `fo' ``. Still trivial. To do something nontrivial, you
need to use one of the special characters. Here is a list of them.

`. (Period)`: is a special character that matches any single character except a newline. Using concatenation, we can make regular expressions like `` `a.b' ``, which matches any three-character string that begins with `` `a' `` and ends with `` `b' ``. `*`: is not a construct by itself; it is a postfix operator that means to match the preceding regular expression repetitively as many times as possible. Thus, `` `o*' `` matches any number of `` `o' ``s (including no `` `o' ``s). `` `*' `` always applies to the *smallest* possible preceding expression. Thus, `` `fo*' `` has a repeating `` `o' ``, not a repeating `` `fo' ``. It matches `` `f' ``, `` `fo' ``, `` `foo' ``, and so on. The matcher processes a `` `*' `` construct by matching, immediately, as many repetitions as can be found. Then it continues with the rest of the pattern. If that fails, backtracking occurs, discarding some of the matches of the `` `*' ``-modified construct in case that makes it possible to match the rest of the pattern. For example, in matching `` `ca*ar' `` against the string `` `caaar' ``, the `` `a*' `` first tries to match all three `` `a' ``s; but the rest of the pattern is `` `ar' `` and there is only `` `r' `` left to match, so this try fails. The next alternative is for `` `a*' `` to match only two `` `a' ``s. With this choice, the rest of the regexp matches successfully. `+`: is a postfix operator, similar to `` `*' `` except that it must match the preceding expression at least once. So, for example, `` `ca+r' `` matches the strings `` `car' `` and `` `caaaar' `` but not the string `` `cr' ``, whereas `` `ca*r' `` matches all three strings. `?`: is a postfix operator, similar to `` `*' `` except that it can match the preceding expression either once or not at all. For example, `` `ca?r' `` matches `` `car' `` or `` `cr' ``; nothing else. `*?, +?, ??`: are non-greedy variants of the operators above. The normal operators `` `*' ``, `` `+' ``, `` `?' `` are *greedy* in that they match as much as they can, as long as the overall regexp can still match. With a following `` `?' ``, they are non-greedy: they will match as little as possible. Thus, both `` `ab*' `` and `` `ab*?' `` can match the string `` `a' `` and the string `` `abbbb' ``; but if you try to match them both against the text `` `abbb' ``, `` `ab*' `` will match it all (the longest valid match), while `` `ab*?' `` will match just `` `a' `` (the shortest valid match). `\{n\}`: is a postfix operator that specifies repetition n times--that is, the preceding regular expression must match exactly n times in a row. For example, `` `x\{4\}' `` matches the string `` `xxxx' `` and nothing else. `\{n,m\}`: is a postfix operator that specifies repetition between n and m times--that is, the preceding regular expression must match at least n times, but no more than m times. If m is omitted, then there is no upper limit, but the preceding regular expression must match at least n times. `` `\{0,1\}' `` is equivalent to `` `?' ``. `` `\{0,\}' `` is equivalent to `` `*' ``. `` `\{1,\}' `` is equivalent to `` `+' ``. `[ ... ]`: is a *character set*, which begins with `` `[' `` and is terminated by `` `]' ``. In the simplest case, the characters between the two brackets are what this set can match. Thus, `` `[ad]' `` matches either one `` `a' `` or one `` `d' ``, and `` `[ad]*' `` matches any string composed of just `` `a' ``s and `` `d' ``s (including the empty string), from which it follows that `` `c[ad]*r' `` matches `` `cr' ``, `` `car' ``, `` `cdr' ``, `` `caddaar' ``, etc. You can also include character ranges in a character set, by writing the starting and ending characters with a `` `-' `` between them. Thus, `` `[a-z]' `` matches any lower-case ASCII letter. Ranges may be intermixed freely with individual characters, as in `` `[a-z$%.]' ``, which matches any lower-case ASCII letter or `` `$' ``, `` `%' `` or period. Note that the usual regexp special characters are not special inside a character set. A completely different set of special characters exists inside character sets: `` `]' ``, `` `-' `` and `` `^' ``. To include a `` `]' `` in a character set, you must make it the first character. For example, `` `[]a]' `` matches `` `]' `` or `` `a' ``. To include a `` `-' ``, write `` `-' `` as the first or last character of the set, or put it after a range. Thus, `` `[]-]' `` matches both `` `]' `` and `` `-' ``. To include `` `^' `` in a set, put it anywhere but at the beginning of the set. (At the beginning, it complements the set--see below.) When you use a range in case-insensitive search, you should write both ends of the range in upper case, or both in lower case, or both should be non-letters. The behavior of a mixed-case range such as `` `A-z' `` is somewhat ill-defined, and it may change in future Emacs versions. `[^ ... ]`: `` `[^' `` begins a *complemented character set*, which matches any character except the ones specified. Thus, `` `[^a-z0-9A-Z]' `` matches all characters *except* ASCII letters and digits. `` `^' `` is not special in a character set unless it is the first character. The character following the `` `^' `` is treated as if it were first (in other words, `` `-' `` and `` `]' `` are not special there). A complemented character set can match a newline, unless newline is mentioned as one of the characters not to match. This is in contrast to the handling of regexps in programs such as `grep`. `^`: is a special character that matches the empty string, but only at the beginning of a line in the text being matched. Otherwise it fails to match anything. Thus, `` `^foo' `` matches a `` `foo' `` that occurs at the beginning of a line. `$`: is similar to `` `^' `` but matches only at the end of a line. Thus, `` `x+$' `` matches a string of one `` `x' `` or more at the end of a line. `\`: has two functions: it quotes the special characters (including `` `\' ``), and it introduces additional special constructs. Because `` `\' `` quotes special characters, `` `\$' `` is a regular expression that matches only `` `$' ``, and `` `\[' `` is a regular expression that matches only `` `[' ``, and so on.

Note: for historical compatibility, special characters are treated as
ordinary ones if they are in contexts where their special meanings make no
sense. For example, `` `*foo' `` treats `` `*' `` as ordinary since there is
no preceding expression on which the `` `*' `` can act. It is poor practice
to depend on this behavior; it is better to quote the special character anyway,
regardless of where it appears.

For the most part, `` `\' `` followed by any character matches only that
character. However, there are several exceptions: two-character
sequences starting with `` `\' `` that have special meanings. The second
character in the sequence is always an ordinary character when used on
its own. Here is a table of `` `\' `` constructs.

`\|`: specifies an alternative. Two regular expressions a and b with `` `\|' `` in between form an expression that matches some text if either a matches it or b matches it. It works by trying to match a, and if that fails, by trying to match b. Thus, `` `foo\|bar' `` matches either `` `foo' `` or `` `bar' `` but no other string. `` `\|' `` applies to the largest possible surrounding expressions. Only a surrounding `` `\( ... \)' `` grouping can limit the grouping power of `` `\|' ``. Full backtracking capability exists to handle multiple uses of `` `\|' ``. `\( ... \)`: is a grouping construct that serves three purposes: 1. To enclose a set of `` `\|' `` alternatives for other operations. Thus, `` `\(foo\|bar\)x' `` matches either `` `foox' `` or `` `barx' ``. - To enclose a complicated expression for the postfix operators `` `*' ``, `` `+' `` and `` `?' `` to operate on. Thus, `` `ba\(na\)*' `` matches `` `bananana' ``, etc., with any (zero or more) number of `` `na' `` strings. - To record a matched substring for future reference. This last application is not a consequence of the idea of a parenthetical grouping; it is a separate feature that is assigned as a second meaning to the same `` `\( ... \)' `` construct. In practice there is usually no conflict between the two meanings; when there is a conflict, you can use a "shy" group. `\(?: ... \)`: specifies a "shy" group that does not record the matched substring; you can't refer back to it with `` `\d' ``. This is useful in mechanically combining regular expressions, so that you can add groups for syntactic purposes without interfering with the numbering of the groups that were written by the user. `\d`: matches the same text that matched the dth occurrence of a `` `\( ... \)' `` construct. After the end of a `` `\( ... \)' `` construct, the matcher remembers the beginning and end of the text matched by that construct. Then, later on in the regular expression, you can use `` `\' `` followed by the digit d to mean "match the same text matched the dth time by the `` `\( ... \)' `` construct." The strings matching the first nine `` `\( ... \)' `` constructs appearing in a regular expression are assigned numbers 1 through 9 in the order that the open-parentheses appear in the regular expression. So you can use `` `\1' `` through `` `\9' `` to refer to the text matched by the corresponding `` `\( ... \)' `` constructs. For example, `` `\(.*\)\1' `` matches any newline-free string that is composed of two identical halves. The `` `\(.*\)' `` matches the first half, which may be anything, but the `` `\1' `` that follows must match the same exact text. If a particular `` `\( ... \)' `` construct matches more than once (which can easily happen if it is followed by `` `*' ``), only the last match is recorded. `` \` ``: matches the empty string, but only at the beginning of the buffer or string being matched against. `\'`: matches the empty string, but only at the end of the buffer or string being matched against. `\=`: matches the empty string, but only at point. `\b`: matches the empty string, but only at the beginning or end of a word. Thus, `` `\bfoo\b' `` matches any occurrence of `` `foo' `` as a separate word. `` `\bballs?\b' `` matches `` `ball' `` or `` `balls' `` as a separate word. `` `\b' `` matches at the beginning or end of the buffer regardless of what text appears next to it. `\B`: matches the empty string, but *not* at the beginning or end of a word. `\<`: matches the empty string, but only at the beginning of a word. `` `\<' `` matches at the beginning of the buffer only if a word-constituent character follows. `\>`: matches the empty string, but only at the end of a word. `` `\>' `` matches at the end of the buffer only if the contents end with a word-constituent character. `\w`: matches any word-constituent character. The syntax table determines which characters these are. See section [AD.6 The Syntax Table](emacs.html#SEC504). `\W`: matches any character that is not a word-constituent. `\sc`: matches any character whose syntax is c. Here c is a character that designates a particular syntax class: thus, `` `w' `` for word constituent, `` `-' `` or `` ` ' `` for whitespace, `` `.' `` for ordinary punctuation, etc. See section [AD.6 The Syntax Table](emacs.html#SEC504). `\Sc`: matches any character whose syntax is not c. `\cc`: matches any character that belongs to the category c. For example, `` `\cc' `` matches Chinese characters, `` `\cg' `` matches Greek characters, etc. For the description of the known categories, type `M-x describe-categories RET`. `\Cc`: matches any character that does *not* belong to category c.

The constructs that pertain to words and syntax are controlled by the
setting of the syntax table (see section [AD.6 The Syntax Table](emacs.html#SEC504)).

Here is a complicated regexp, stored in `sentence-end` and used
by Emacs to recognize the end of a sentence together with any
whitespace that follows. We show its Lisp syntax to distinguish the
spaces from the tab characters. In Lisp syntax, the string constant
begins and ends with a double-quote. `` `\"' `` stands for a
double-quote as part of the regexp, `` `\\' `` for a backslash as part
of the regexp, `` `\t' `` for a tab, and `` `\n' `` for a newline.

|  |  |
| --- | --- |
|  | ``` "[.?!][]\"')]*\\($\\| $\\|\t\\|  \\)[ \t\n]*" ``` |

This contains four parts in succession: a character set matching
period, `` `?' ``, or `` `!' ``; a character set matching
close-brackets, quotes, or parentheses, repeated zero or more times; a
set of alternatives within backslash-parentheses that matches either
end-of-line, a space at the end of a line, a tab, or two spaces; and a
character set matching whitespace characters, repeated any number of
times.

To enter the same regexp interactively, you would type `TAB` to
enter a tab, and `C-j` to enter a newline. (When typed
interactively, `C-j` should be preceded by a `C-q`, to prevent
Emacs from running the command bound to a newline.) You would also type
single backslashes as themselves, instead of doubling them for Lisp
syntax.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC97) ] | [ [>](emacs.html#SEC99) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.6 Searching and Case

Incremental searches in Emacs normally ignore the case of the text
they are searching through, if you specify the text in lower case.
Thus, if you specify searching for `` `foo' ``, then `` `Foo' `` and
`` `foo' `` are also considered a match. Regexps, and in particular
character sets, are included: `` `[ab]' `` would match `` `a' `` or
`` `A' `` or `` `b' `` or `` `B' ``.

An upper-case letter anywhere in the incremental search string makes
the search case-sensitive. Thus, searching for `` `Foo' `` does not find
`` `foo' `` or `` `FOO' ``. This applies to regular expression search as
well as to string search. The effect ceases if you delete the
upper-case letter from the search string.

Typing `M-c` within an incremental search toggles the case
sensitivity of that search. The effect does not extend beyond the
current incremental search to the next one, but it does override the
effect of including an upper-case letter in the current search.

If you set the variable `case-fold-search` to `nil`, then
all letters must match exactly, including case. This is a per-buffer
variable; altering the variable affects only the current buffer, but
there is a default value which you can change as well. See section [AD.2.4 Local Variables](emacs.html#SEC485).
This variable applies to nonincremental searches also, including those
performed by the replace commands (see section [K.7 Replacement Commands](emacs.html#SEC99)) and the minibuffer
history matching commands (see section [E.4 Minibuffer History](emacs.html#SEC39)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC98) ] | [ [>](emacs.html#SEC100) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC104) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.7 Replacement Commands

Global search-and-replace operations are not needed often in Emacs,
but they are available. In addition to the simple `M-x
replace-string` command which is like that found in most editors,
there is a `M-x query-replace` command which finds each occurrence
of the pattern and asks you whether to replace it.

The replace commands normally operate on the text from point to the
end of the buffer; however, in Transient Mark mode, when the mark is
active, they operate on the region. The replace commands all replace
one string (or regexp) with one replacement string. It is possible to
perform several replacements in parallel using the command
`expand-region-abbrevs` (see section [X.3 Controlling Abbrev Expansion](emacs.html#SEC353)).

> |  |  |  |
> | --- | --- | --- |
> | [K.7.1 Unconditional Replacement](emacs.html#SEC100) |  | Replacing all matches for a string. |
> | [K.7.2 Regexp Replacement](emacs.html#SEC101) |  | Replacing all matches for a regexp. |
> | [K.7.3 Replace Commands and Case](emacs.html#SEC102) |  | How replacements preserve case of letters. |
> | [K.7.4 Query Replace](emacs.html#SEC103) |  | How to use querying. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC99) ] | [ [>](emacs.html#SEC101) ] | [ [<<](emacs.html#SEC98) ] | [ [Up](emacs.html#SEC99) ] | [ [>>](emacs.html#SEC104) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### K.7.1 Unconditional Replacement

`M-x replace-string RET string RET newstring RET`: Replace every occurrence of string with newstring. `M-x replace-regexp RET regexp RET newstring RET`: Replace every match for regexp with newstring.

To replace every instance of `` `foo' `` after point with `` `bar' ``,
use the command `M-x replace-string` with the two arguments
`` `foo' `` and `` `bar' ``. Replacement happens only in the text after
point, so if you want to cover the whole buffer you must go to the
beginning first. All occurrences up to the end of the buffer are
replaced; to limit replacement to part of the buffer, narrow to that
part of the buffer before doing the replacement (see section [AC.22 Narrowing](emacs.html#SEC462)).
In Transient Mark mode, when the region is active, replacement is
limited to the region (see section [H.2 Transient Mark Mode](emacs.html#SEC54)).

When `replace-string` exits, it leaves point at the last
occurrence replaced. It sets the mark to the prior position of point
(where the `replace-string` command was issued); use `C-u
C-SPC` to move back there.

A numeric argument restricts replacement to matches that are surrounded
by word boundaries. The argument's value doesn't matter.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC100) ] | [ [>](emacs.html#SEC102) ] | [ [<<](emacs.html#SEC98) ] | [ [Up](emacs.html#SEC99) ] | [ [>>](emacs.html#SEC104) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### K.7.2 Regexp Replacement

The `M-x replace-string` command replaces exact matches for a
single string. The similar command `M-x replace-regexp` replaces
any match for a specified pattern.

In `replace-regexp`, the newstring need not be constant: it
can refer to all or part of what is matched by the regexp.
`` `\&' `` in newstring stands for the entire match being replaced.
`` `\d' `` in newstring, where d is a digit, stands for
whatever matched the dth parenthesized grouping in regexp.
To include a `` `\' `` in the text to replace with, you must enter
`` `\\' ``. For example,

|  |  |
| --- | --- |
|  | ``` M-x replace-regexp RET c[ad]+r RET \&-safe RET ``` |

replaces (for example) `` `cadr' `` with `` `cadr-safe' `` and `` `cddr' ``
with `` `cddr-safe' ``.

|  |  |
| --- | --- |
|  | ``` M-x replace-regexp RET \(c[ad]+r\)-safe RET \1 RET ``` |

performs the inverse transformation.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC101) ] | [ [>](emacs.html#SEC103) ] | [ [<<](emacs.html#SEC98) ] | [ [Up](emacs.html#SEC99) ] | [ [>>](emacs.html#SEC104) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### K.7.3 Replace Commands and Case

If the first argument of a replace command is all lower case, the
command ignores case while searching for occurrences to
replace--provided `case-fold-search` is non-`nil`. If
`case-fold-search` is set to `nil`, case is always significant
in all searches.

In addition, when the newstring argument is all or partly lower
case, replacement commands try to preserve the case pattern of each
occurrence. Thus, the command

|  |  |
| --- | --- |
|  | ``` M-x replace-string RET foo RET bar RET ``` |

replaces a lower case `` `foo' `` with a lower case `` `bar' ``, an
all-caps `` `FOO' `` with `` `BAR' ``, and a capitalized `` `Foo' `` with
`` `Bar' ``. (These three alternatives--lower case, all caps, and
capitalized, are the only ones that `replace-string` can
distinguish.)

If upper-case letters are used in the replacement string, they remain
upper case every time that text is inserted. If upper-case letters are
used in the first argument, the second argument is always substituted
exactly as given, with no case conversion. Likewise, if either
`case-replace` or `case-fold-search` is set to `nil`,
replacement is done without case conversion.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC102) ] | [ [>](emacs.html#SEC104) ] | [ [<<](emacs.html#SEC98) ] | [ [Up](emacs.html#SEC99) ] | [ [>>](emacs.html#SEC104) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### K.7.4 Query Replace

`M-% string RET newstring RET`: `M-x query-replace RET string RET newstring RET`: Replace some occurrences of string with newstring. `C-M-% regexp RET newstring RET`: `M-x query-replace-regexp RET regexp RET newstring RET`: Replace some matches for regexp with newstring.

If you want to change only some of the occurrences of `` `foo' `` to
`` `bar' ``, not all of them, then you cannot use an ordinary
`replace-string`. Instead, use `M-%` (`query-replace`).
This command finds occurrences of `` `foo' `` one by one, displays each
occurrence and asks you whether to replace it. Aside from querying,
`query-replace` works just like `replace-string`. It
preserves case, like `replace-string`, provided
`case-replace` is non-`nil`, as it normally is. A numeric
argument means consider only occurrences that are bounded by
word-delimiter characters.

`C-M-%` performs regexp search and replace (`query-replace-regexp`).

The characters you can type when you are shown a match for the string
or regexp are:

`SPC`: to replace the occurrence with newstring. `DEL`: to skip to the next occurrence without replacing this one. `, (Comma)`: to replace this occurrence and display the result. You are then asked for another input character to say what to do next. Since the replacement has already been made, `DEL` and `SPC` are equivalent in this situation; both move to the next occurrence. You can type `C-r` at this point (see below) to alter the replaced text. You can also type `C-x u` to undo the replacement; this exits the `query-replace`, so if you want to do further replacement you must use `C-x ESC ESC RET` to restart (see section [E.5 Repeating Minibuffer Commands](emacs.html#SEC40)). `RET`: to exit without doing any more replacements. `. (Period)`: to replace this occurrence and then exit without searching for more occurrences. `!`: to replace all remaining occurrences without asking again. `^`: to go back to the position of the previous occurrence (or what used to be an occurrence), in case you changed it by mistake. This works by popping the mark ring. Only one `^` in a row is meaningful, because only one previous replacement position is kept during `query-replace`. `C-r`: to enter a recursive editing level, in case the occurrence needs to be edited rather than just replaced with newstring. When you are done, exit the recursive editing level with `C-M-c` to proceed to the next occurrence. See section [AC.26 Recursive Editing Levels](emacs.html#SEC466). `C-w`: to delete the occurrence, and then enter a recursive editing level as in `C-r`. Use the recursive edit to insert text to replace the deleted occurrence of string. When done, exit the recursive editing level with `C-M-c` to proceed to the next occurrence. `e`: to edit the replacement string in the minibuffer. When you exit the minibuffer by typing `RET`, the minibuffer contents replace the current occurrence of the pattern. They also become the new replacement string for any further occurrences. `C-l`: to redisplay the screen. Then you must type another character to specify what to do with this occurrence. `C-h`: to display a message summarizing these options. Then you must type another character to specify what to do with this occurrence.

Some other characters are aliases for the ones listed above: `y`,
`n` and `q` are equivalent to `SPC`, `DEL` and
`RET`.

Aside from this, any other character exits the `query-replace`,
and is then reread as part of a key sequence. Thus, if you type
`C-k`, it exits the `query-replace` and then kills to end of
line.

To restart a `query-replace` once it is exited, use `C-x
ESC ESC`, which repeats the `query-replace` because it
used the minibuffer to read its arguments. See section [C-x ESC ESC](emacs.html#SEC40).

See also [AB.9 Transforming File Names in Dired](emacs.html#SEC405), for Dired commands to rename,
copy, or link files by replacing regexp matches in file names.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC103) ] | [ [>](emacs.html#SEC105) ] | [ [<<](emacs.html#SEC99) ] | [ [Up](emacs.html#SEC91) ] | [ [>>](emacs.html#SEC105) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## K.8 Other Search-and-Loop Commands

Here are some other commands that find matches for a regular
expression. They all ignore case in matching, if the pattern contains
no upper-case letters and `case-fold-search` is non-`nil`.
Aside from `occur`, all operate on the text from point to the end
of the buffer, or on the active region in Transient Mark mode.

`M-x occur RET regexp RET`: Display a list showing each line in the buffer that contains a match for regexp. To limit the search to part of the buffer, narrow to that part (see section [AC.22 Narrowing](emacs.html#SEC462)). A numeric argument n specifies that n lines of context are to be displayed before and after each matching line. The buffer `` `*Occur*' `` containing the output serves as a menu for finding the occurrences in their original context. Click `Mouse-2` on an occurrence listed in `` `*Occur*' ``, or position point there and type `RET`; this switches to the buffer that was searched and moves point to the original of the chosen occurrence. `M-x list-matching-lines`: Synonym for `M-x occur`. `M-x how-many RET regexp RET`: Print the number of matches for regexp that exist in the buffer after point. In Transient Mark mode, if the region is active, the command operates on the region instead. `M-x flush-lines RET regexp RET`: Delete each line that contains a match for regexp, operating on the text after point. In Transient Mark mode, if the region is active, the command operates on the region instead. `M-x keep-lines RET regexp RET`: Delete each line that *does not* contain a match for regexp, operating on the text after point. In Transient Mark mode, if the region is active, the command operates on the region instead.

You can also search multiple files under control of a tags table
(see section [W.2.6 Searching and Replacing with Tags Tables](emacs.html#SEC340)) or through Dired `A` command
(see section [AB.7 Operating on Files](emacs.html#SEC403)), or ask the `grep` program to do it
(see section [V.2 Searching with Grep under Emacs](emacs.html#SEC318)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC104) ] | [ [>](emacs.html#SEC106) ] | [ [<<](emacs.html#SEC91) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC110) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# L. Commands for Fixing Typos

In this chapter we describe the commands that are especially useful for
the times when you catch a mistake in your text just after you have made
it, or change your mind while composing text on the fly.

The most fundamental command for correcting erroneous editing is the
undo command, `C-x u` or `C-_`. This command undoes a single
command (usually), a part of a command (in the case of
`query-replace`), or several consecutive self-inserting characters.
Consecutive repetitions of `C-_` or `C-x u` undo earlier and
earlier changes, back to the limit of the undo information available.
See section [D.4 Undoing Changes](emacs.html#SEC23), for more information.

> |  |  |  |
> | --- | --- | --- |
> | [L.1 Killing Your Mistakes](emacs.html#SEC106) |  | Commands to kill a batch of recently entered text. |
> | [L.2 Transposing Text](emacs.html#SEC107) |  | Exchanging two characters, words, lines, lists... |
> | [L.3 Case Conversion](emacs.html#SEC108) |  | Correcting case of last word entered. |
> | [L.4 Checking and Correcting Spelling](emacs.html#SEC109) |  | Apply spelling checker to a word, or a whole file. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC105) ] | [ [>](emacs.html#SEC107) ] | [ [<<](emacs.html#SEC105) ] | [ [Up](emacs.html#SEC105) ] | [ [>>](emacs.html#SEC110) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## L.1 Killing Your Mistakes

`DEL`: Delete last character (`delete-backward-char`). `M-DEL`: Kill last word (`backward-kill-word`). `C-x DEL`: Kill to beginning of sentence (`backward-kill-sentence`).

The `DEL` character (`delete-backward-char`) is the most
important correction command. It deletes the character before point.
When `DEL` follows a self-inserting character command, you can think
of it as canceling that command. However, avoid the mistake of thinking
of `DEL` as a general way to cancel a command!

When your mistake is longer than a couple of characters, it might be
more convenient to use `M-DEL` or `C-x DEL`.
`M-DEL` kills back to the start of the last word, and `C-x
DEL` kills back to the start of the last sentence. `C-x
DEL` is particularly useful when you change your mind about the
phrasing of the text you are writing. `M-DEL` and `C-x
DEL` save the killed text for `C-y` and `M-y` to
retrieve. See section [H.8 Yanking](emacs.html#SEC63).

`M-DEL` is often useful even when you have typed only a few
characters wrong, if you know you are confused in your typing and aren't
sure exactly what you typed. At such a time, you cannot correct with
`DEL` except by looking at the screen to see what you did. Often it
requires less thought to kill the whole word and start again.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC106) ] | [ [>](emacs.html#SEC108) ] | [ [<<](emacs.html#SEC108) ] | [ [Up](emacs.html#SEC105) ] | [ [>>](emacs.html#SEC110) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## L.2 Transposing Text

`C-t`: Transpose two characters (`transpose-chars`). `M-t`: Transpose two words (`transpose-words`). `C-M-t`: Transpose two balanced expressions (`transpose-sexps`). `C-x C-t`: Transpose two lines (`transpose-lines`).

The common error of transposing two characters can be fixed, when they
are adjacent, with the `C-t` command (`transpose-chars`). Normally,
`C-t` transposes the two characters on either side of point. When
given at the end of a line, rather than transposing the last character of
the line with the newline, which would be useless, `C-t` transposes the
last two characters on the line. So, if you catch your transposition error
right away, you can fix it with just a `C-t`. If you don't catch it so
fast, you must move the cursor back between the two transposed
characters before you type `C-t`. If you transposed a space with
the last character of the word before it, the word motion commands are
a good way of getting there. Otherwise, a reverse search (`C-r`)
is often the best way. See section [K. Searching and Replacement](emacs.html#SEC91).

`M-t` transposes the word before point with the word after point
(`transpose-words`). It moves point forward over a word,
dragging the word preceding or containing point forward as well. The
punctuation characters between the words do not move. For example,
`` `FOO, BAR' `` transposes into `` `BAR, FOO' `` rather than
`` `BAR FOO,' ``.

`C-M-t` (`transpose-sexps`) is a similar command for
transposing two expressions (see section [U.4.1 Expressions with Balanced Parentheses](emacs.html#SEC282)), and `C-x C-t`
(`transpose-lines`) exchanges lines. They work like `M-t`
except as regards what units of text they transpose.

A numeric argument to a transpose command serves as a repeat count: it
tells the transpose command to move the character (word, expression, line)
before or containing point across several other characters (words,
expressions, lines). For example, `C-u 3 C-t` moves the character before
point forward across three other characters. It would change
`` `f-!-oobar' `` into `` `oobf-!-ar' ``. This is equivalent to
repeating `C-t` three times. `C-u - 4 M-t` moves the word
before point backward across four words. `C-u - C-M-t` would cancel
the effect of plain `C-M-t`.

A numeric argument of zero is assigned a special meaning (because
otherwise a command with a repeat count of zero would do nothing): to
transpose the character (word, expression, line) ending after point
with the one ending after the mark.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC107) ] | [ [>](emacs.html#SEC109) ] | [ [<<](emacs.html#SEC109) ] | [ [Up](emacs.html#SEC105) ] | [ [>>](emacs.html#SEC110) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## L.3 Case Conversion

`M-- M-l`: Convert last word to lower case. Note `Meta--` is Meta-minus. `M-- M-u`: Convert last word to all upper case. `M-- M-c`: Convert last word to lower case with capital initial.

A very common error is to type words in the wrong case. Because of this,
the word case-conversion commands `M-l`, `M-u` and `M-c` have a
special feature when used with a negative argument: they do not move the
cursor. As soon as you see you have mistyped the last word, you can simply
case-convert it and go on typing. See section [T.6 Case Conversion Commands](emacs.html#SEC244).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC108) ] | [ [>](emacs.html#SEC110) ] | [ [<<](emacs.html#SEC105) ] | [ [Up](emacs.html#SEC105) ] | [ [>>](emacs.html#SEC110) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## L.4 Checking and Correcting Spelling

This section describes the commands to check the spelling of a single
word or of a portion of a buffer. These commands work with the spelling
checker program Ispell, which is not part of Emacs.
See section `Overview ispell' in The Ispell Manual.

`M-x flyspell-mode`: Enable Flyspell mode, which highlights all misspelled words. `M-$`: Check and correct spelling of the word at point (`ispell-word`). `M-TAB`: Complete the word before point based on the spelling dictionary (`ispell-complete-word`). `M-x ispell`: Spell-check the active region or the current buffer. `M-x ispell-buffer`: Check and correct spelling of each word in the buffer. `M-x ispell-region`: Check and correct spelling of each word in the region. `M-x ispell-message`: Check and correct spelling of each word in a draft mail message, excluding cited material. `M-x ispell-change-dictionary RET dict RET`: Restart the Ispell process, using dict as the dictionary. `M-x ispell-kill-ispell`: Kill the Ispell subprocess.

Flyspell mode is a fully-automatic way to check spelling as you edit
in Emacs. It operates by checking words as you change or insert them.
When it finds a word that it does not recognize, it highlights that
word. This does not interfere with your editing, but when you see the
highlighted word, you can move to it and fix it. Type `M-x
flyspell-mode` to enable or disable this mode in the current buffer.

When Flyspell mode highlights a word as misspelled, you can click on
it with `Mouse-2` to display a menu of possible corrections and
actions. You can also correct the word by editing it manually in any
way you like.

The other Emacs spell-checking features check or look up words when
you give an explicit command to do so. Checking all or part of the
buffer is useful when you have text that was written outside of this
Emacs session and might contain any number of misspellings.

To check the spelling of the word around or next to point, and
optionally correct it as well, use the command `M-$`
(`ispell-word`). If the word is not correct, the command offers
you various alternatives for what to do about it.

To check the entire current buffer, use `M-x ispell-buffer`. Use
`M-x ispell-region` to check just the current region. To check
spelling in an email message you are writing, use `M-x
ispell-message`; that command checks the whole buffer, except for
material that is indented or appears to be cited from other messages.

The `M-x ispell` command spell-checks the active region if the
Transient Mark mode is on (see section [H.2 Transient Mark Mode](emacs.html#SEC54)), otherwise it
spell-checks the current buffer.

Each time these commands encounter an incorrect word, they ask you
what to do. They display a list of alternatives, usually including
several "near-misses"---words that are close to the word being
checked. Then you must type a single-character response. Here are
the valid responses:

`SPC`: Skip this word--continue to consider it incorrect, but don't change it here. `r new RET`: Replace the word (just this time) with new. `R new RET`: Replace the word with new, and do a `query-replace` so you can replace it elsewhere in the buffer if you wish. `digit`: Replace the word (just this time) with one of the displayed near-misses. Each near-miss is listed with a digit; type that digit to select it. `a`: Accept the incorrect word--treat it as correct, but only in this editing session. `A`: Accept the incorrect word--treat it as correct, but only in this editing session and for this buffer. `i`: Insert this word in your private dictionary file so that Ispell will consider it correct from now on, even in future sessions. `u`: Insert the lower-case version of this word in your private dictionary file. `m`: Like `i`, but you can also specify dictionary completion information. `l word RET`: Look in the dictionary for words that match word. These words become the new list of "near-misses"; you can select one of them as the replacement by typing a digit. You can use `` `*' `` in word as a wildcard. `C-g`: Quit interactive spell checking. You can restart it again afterward with `C-u M-$`. `X`: Same as `C-g`. `x`: Quit interactive spell checking and move point back to where it was when you started spell checking. `q`: Quit interactive spell checking and kill the Ispell subprocess. `C-l`: Refresh the screen. `C-z`: This key has its normal command meaning (suspend Emacs or iconify this frame).

The command `ispell-complete-word`, which is bound to the key
`M-TAB` in Text mode and related modes, shows a list of
completions based on spelling correction. Insert the beginning of a
word, and then type `M-TAB`; the command displays a completion
list window. To choose one of the completions listed, click
`Mouse-2` on it, or move the cursor there in the completions window
and type `RET`. See section [T.7 Text Mode](emacs.html#SEC245).

Once started, the Ispell subprocess continues to run (waiting for
something to do), so that subsequent spell checking commands complete
more quickly. If you want to get rid of the Ispell process, use
`M-x ispell-kill-ispell`. This is not usually necessary, since the
process uses no time except when you do spelling correction.

Ispell uses two dictionaries: the standard dictionary and your private
dictionary. The variable `ispell-dictionary` specifies the file
name of the standard dictionary to use. A value of `nil` says to
use the default dictionary. The command `M-x
ispell-change-dictionary` sets this variable and then restarts the
Ispell subprocess, so that it will use a different dictionary.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC109) ] | [ [>](emacs.html#SEC111) ] | [ [<<](emacs.html#SEC105) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# M. File Handling

The operating system stores data permanently in named *files*, so
most of the text you edit with Emacs comes from a file and is ultimately
stored in a file.

To edit a file, you must tell Emacs to read the file and prepare a
buffer containing a copy of the file's text. This is called
*visiting* the file. Editing commands apply directly to text in the
buffer; that is, to the copy inside Emacs. Your changes appear in the
file itself only when you *save* the buffer back into the file.

In addition to visiting and saving files, Emacs can delete, copy,
rename, and append to files, keep multiple versions of them, and operate
on file directories.

> |  |  |  |
> | --- | --- | --- |
> | [M.1 File Names](emacs.html#SEC111) |  | How to type and edit file-name arguments. |
> | [M.2 Visiting Files](emacs.html#SEC112) |  | Visiting a file prepares Emacs to edit the file. |
> | [M.3 Saving Files](emacs.html#SEC113) |  | Saving makes your changes permanent. |
> | [M.4 Reverting a Buffer](emacs.html#SEC121) |  | Reverting cancels all the changes not saved. |
> | [M.5 Auto-Saving: Protection Against Disasters](emacs.html#SEC122) |  | Auto Save periodically protects against loss of data. |
> | [M.6 File Name Aliases](emacs.html#SEC126) |  | Handling multiple names for one file. |
> | [M.7 Version Control](emacs.html#SEC127) |  | Version control systems (RCS, CVS and SCCS). |
> | [M.8 File Directories](emacs.html#SEC163) |  | Creating, deleting, and listing file directories. |
> | [M.9 Comparing Files](emacs.html#SEC164) |  | Finding where two files differ. |
> | [M.10 Miscellaneous File Operations](emacs.html#SEC165) |  | Other things you can do on files. |
> | [M.11 Accessing Compressed Files](emacs.html#SEC166) |  | Accessing compressed files. |
> | [M.12 File Archives](emacs.html#SEC167) |  | Operating on tar, zip, jar etc. archive files. |
> | [M.13 Remote Files](emacs.html#SEC168) |  | Accessing files on other sites. |
> | [M.14 Quoted File Names](emacs.html#SEC169) |  | Quoting special characters in file names. |
> | [M.15 File Name Cache](emacs.html#SEC170) |  | Completion against a list of files you often use. |
> | [M.16 Convenience Features for Finding Files](emacs.html#SEC171) |  |  |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC110) ] | [ [>](emacs.html#SEC112) ] | [ [<<](emacs.html#SEC110) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.1 File Names

Most Emacs commands that operate on a file require you to specify the
file name. (Saving and reverting are exceptions; the buffer knows which
file name to use for them.) You enter the file name using the
minibuffer (see section [E. The Minibuffer](emacs.html#SEC31)). *Completion* is available
(see section [E.3 Completion](emacs.html#SEC34)) to make it easier to specify long file names. When
completing file names, Emacs ignores those whose file-name extensions
appear in the variable `completion-ignored-extensions`; see
[E.3.4 Completion Options](emacs.html#SEC38).

For most operations, there is a *default file name* which is used
if you type just `RET` to enter an empty argument. Normally the
default file name is the name of the file visited in the current buffer;
this makes it easy to operate on that file with any of the Emacs file
commands.

Each buffer has a default directory which is normally the same as the
directory of the file visited in that buffer. When you enter a file
name without a directory, the default directory is used. If you specify
a directory in a relative fashion, with a name that does not start with
a slash, it is interpreted with respect to the default directory. The
default directory is kept in the variable `default-directory`,
which has a separate value in every buffer.

For example, if the default file name is `/u/rms/gnu/gnu.tasks' then
the default directory is `/u/rms/gnu/'. If you type just `` `foo' ``,
which does not specify a directory, it is short for `/u/rms/gnu/foo'.
`` `../.login' `` would stand for `/u/rms/.login'. `` `new/foo' ``
would stand for the file name `/u/rms/gnu/new/foo'.

The command `M-x pwd` displays the current buffer's default
directory, and the command `M-x cd` sets it (to a value read using
the minibuffer). A buffer's default directory changes only when the
`cd` command is used. A file-visiting buffer's default directory
is initialized to the directory of the file that is visited in that buffer. If
you create a buffer with `C-x b`, its default directory is copied
from that of the buffer that was current at the time.

The default directory actually appears in the minibuffer when the
minibuffer becomes active to read a file name. This serves two
purposes: it *shows* you what the default is, so that you can type
a relative file name and know with certainty what it will mean, and it
allows you to *edit* the default to specify a different directory.
This insertion of the default directory is inhibited if the variable
`insert-default-directory` is set to `nil`.

Note that it is legitimate to type an absolute file name after you
enter the minibuffer, ignoring the presence of the default directory
name as part of the text. The final minibuffer contents may look
invalid, but that is not so. For example, if the minibuffer starts out
with `` `/usr/tmp/' `` and you add `` `/x1/rms/foo' ``, you get
`` `/usr/tmp//x1/rms/foo' ``; but Emacs ignores everything through the
first slash in the double slash; the result is `` `/x1/rms/foo' ``.
See section [E.1 Minibuffers for File Names](emacs.html#SEC32).

`` `$' `` in a file name is used to substitute environment variables.
For example, if you have used the shell command `export
FOO=rms/hacks` to set up an environment variable named `FOO`, then
you can use `/u/$FOO/test.c' or `/u/${FOO}/test.c' as an
abbreviation for `/u/rms/hacks/test.c'. The environment variable
name consists of all the alphanumeric characters after the `` `$' ``;
alternatively, it may be enclosed in braces after the `` `$' ``. Note
that shell commands to set environment variables affect Emacs only if
done before Emacs is started.

You can use the `~/' in a file name to mean your home directory,
or `~user-id/' to mean the home directory of a user whose
login name is `user-id`. (On DOS and Windows systems, where a user
doesn't have a home directory, Emacs substitutes `~/' with the
value of the environment variable `HOME`; see [AE.5.1 General Variables](emacs.html#SEC534).)

To access a file with `` `$' `` in its name, type `` `$$' ``. This pair
is converted to a single `` `$' `` at the same time as variable
substitution is performed for a single `` `$' ``. Alternatively, quote the
whole file name with `` `/:' `` (see section [M.14 Quoted File Names](emacs.html#SEC169)). File names
which begin with a literal `` `~' `` should also be quoted with `` `/:' ``.

The Lisp function that performs the substitution is called
`substitute-in-file-name`. The substitution is performed only on
file names read as such using the minibuffer.

You can include non-ASCII characters in file names if you set the
variable `file-name-coding-system` to a non-`nil` value.
See section [Q.9 Specifying a Coding System](emacs.html#SEC222).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC111) ] | [ [>](emacs.html#SEC113) ] | [ [<<](emacs.html#SEC113) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC172) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.2 Visiting Files

`C-x C-f`: Visit a file (`find-file`). `C-x C-r`: Visit a file for viewing, without allowing changes to it (`find-file-read-only`). `C-x C-v`: Visit a different file instead of the one visited last (`find-alternate-file`). `C-x 4 f`: Visit a file, in another window (`find-file-other-window`). Don't alter what is displayed in the selected window. `C-x 5 f`: Visit a file, in a new frame (`find-file-other-frame`). Don't alter what is displayed in the selected frame. `M-x find-file-literally`: Visit a file with no conversion of the contents.

*Visiting* a file means copying its contents into an Emacs
buffer so you can edit them. Emacs makes a new buffer for each file
that you visit. We often say that this buffer "is visiting" that
file, or that the buffer's "visited file" is that file. Emacs
constructs the buffer name from the file name by throwing away the
directory, keeping just the name proper. For example, a file named
`/usr/rms/emacs.tex' would get a buffer named `` `emacs.tex' ``.
If there is already a buffer with that name, Emacs constructs a unique
name--the normal method is to append `` `<2>' ``, `` `<3>' ``, and so
on, but you can select other methods (see section [N.7.1 Making Buffer Names Unique](emacs.html#SEC180)).

Each window's mode line shows the name of the buffer that is being displayed
in that window, so you can always tell what buffer you are editing.

The changes you make with editing commands are made in the Emacs
buffer. They do not take effect in the file that you visited, or any
place permanent, until you *save* the buffer. Saving the buffer
means that Emacs writes the current contents of the buffer into its
visited file. See section [M.3 Saving Files](emacs.html#SEC113).

If a buffer contains changes that have not been saved, we say the
buffer is *modified*. This is important because it implies that
some changes will be lost if the buffer is not saved. The mode line
displays two stars near the left margin to indicate that the buffer is
modified.

To visit a file, use the command `C-x C-f` (`find-file`). Follow
the command with the name of the file you wish to visit, terminated by a
`RET`.

The file name is read using the minibuffer (see section [E. The Minibuffer](emacs.html#SEC31)), with
defaulting and completion in the standard manner (see section [M.1 File Names](emacs.html#SEC111)).
While in the minibuffer, you can abort `C-x C-f` by typing
`C-g`. File-name completion ignores certain filenames; for more
about this, see [E.3.4 Completion Options](emacs.html#SEC38).

When Emacs is built with a suitable GUI toolkit, it pops up the
standard File Selection dialog of that toolkit instead of prompting for
the file name in the minibuffer. On Unix and GNU/Linux platforms, Emacs
does that when built with LessTif and Motif toolkits; on MS-Windows, the
GUI version does that by default.

Your confirmation that `C-x C-f` has completed successfully is the
appearance of new text on the screen and a new buffer name in the mode
line. If the specified file does not exist and could not be created, or
cannot be read, then you get an error, with an error message displayed
in the echo area.

If you visit a file that is already in Emacs, `C-x C-f` does not make
another copy. It selects the existing buffer containing that file.
However, before doing so, it checks that the file itself has not changed
since you visited or saved it last. If the file has changed, a warning
message is shown. See section [Simultaneous Editing](emacs.html#SEC118).

Since Emacs reads the visited file in its entirety, files whose size
is larger than the maximum Emacs buffer size (see section [N. Using Multiple Buffers](emacs.html#SEC172)) cannot be
visited; if you try, Emacs will display an error message saying that the
maximum buffer size has been exceeded.

What if you want to create a new file? Just visit it. Emacs displays
`` `(New file)' `` in the echo area, but in other respects behaves as if
you had visited an existing empty file. If you make any changes and
save them, the file is created.

Emacs recognizes from the contents of a file which convention it uses
to separate lines--newline (used on GNU/Linux and on Unix),
carriage-return linefeed (used on Microsoft systems), or just
carriage-return (used on the Macintosh)---and automatically converts the
contents to the normal Emacs convention, which is that the newline
character separates lines. This is a part of the general feature of
coding system conversion (see section [Q.7 Coding Systems](emacs.html#SEC220)), and makes it possible
to edit files imported from different operating systems with
equal convenience. If you change the text and save the file, Emacs
performs the inverse conversion, changing newlines back into
carriage-return linefeed or just carriage-return if appropriate.

If the file you specify is actually a directory, `C-x C-f` invokes
Dired, the Emacs directory browser, so that you can "edit" the contents
of the directory (see section [AB. Dired, the Directory Editor](emacs.html#SEC396)). Dired is a convenient way to delete,
look at, or operate on the files in the directory. However, if the
variable `find-file-run-dired` is `nil`, then it is an error
to try to visit a directory.

Files which are actually collections of other files, or *file
archives*, are visited in special modes which invoke a Dired-like
environment to allow operations on archive members. See section [M.12 File Archives](emacs.html#SEC167), for more about these features.

If the file name you specify contains shell-style wildcard characters,
Emacs visits all the files that match it. Wildcards include `` `?' ``,
`` `*' ``, and `` `[...]' `` sequences. See section [M.14 Quoted File Names](emacs.html#SEC169), for
information on how to visit a file whose name actually contains wildcard
characters. You can disable the wildcard feature by customizing
`find-file-wildcards`.

If you visit a file that the operating system won't let you modify,
Emacs makes the buffer read-only, so that you won't go ahead and make
changes that you'll have trouble saving afterward. You can make the
buffer writable with `C-x C-q` (`vc-toggle-read-only`).
See section [N.3 Miscellaneous Buffer Operations](emacs.html#SEC175).

Occasionally you might want to visit a file as read-only in order to
protect yourself from entering changes accidentally; do so by visiting
the file with the command `C-x C-r` (`find-file-read-only`).

If you visit a nonexistent file unintentionally (because you typed the
wrong file name), use the `C-x C-v` command
(`find-alternate-file`) to visit the file you really wanted.
`C-x C-v` is similar to `C-x C-f`, but it kills the current
buffer (after first offering to save it if it is modified). When
`C-x C-v` reads the file name to visit, it inserts the entire
default file name in the buffer, with point just after the directory
part; this is convenient if you made a slight error in typing the name.

If you find a file which exists but cannot be read, `C-x C-f`
signals an error.

`C-x 4 f` (`find-file-other-window`) is like `C-x C-f`
except that the buffer containing the specified file is selected in another
window. The window that was selected before `C-x 4 f` continues to
show the same buffer it was already showing. If this command is used when
only one window is being displayed, that window is split in two, with one
window showing the same buffer as before, and the other one showing the
newly requested file. See section [O. Multiple Windows](emacs.html#SEC183).

`C-x 5 f` (`find-file-other-frame`) is similar, but opens a
new frame, or makes visible any existing frame showing the file you
seek. This feature is available only when you are using a window
system. See section [P. Frames and X Windows](emacs.html#SEC191).

If you wish to edit a file as a sequence of ASCII characters with no special
encoding or conversion, use the `M-x find-file-literally` command.
It visits a file, like `C-x C-f`, but does not do format conversion
(see section [T.11 Editing Formatted Text](emacs.html#SEC258)), character code conversion (see section [Q.7 Coding Systems](emacs.html#SEC220)), or automatic uncompression (see section [M.11 Accessing Compressed Files](emacs.html#SEC166)), and
does not add a final newline because of `require-final-newline`.
If you already have visited the same file in the usual (non-literal)
manner, this command asks you whether to visit it literally instead.

Two special hook variables allow extensions to modify the operation of
visiting files. Visiting a file that does not exist runs the functions
in the list `find-file-not-found-hooks`; this variable holds a list
of functions, and the functions are called one by one (with no
arguments) until one of them returns non-`nil`. This is not a
normal hook, and the name ends in `` `-hooks' `` rather than `` `-hook' ``
to indicate that fact.

Successful visiting of any file, whether existing or not, calls the
functions in the list `find-file-hooks`, with no arguments.
This variable is really a normal hook, but it has an abnormal name for
historical compatibility. In the case of a nonexistent file, the
`find-file-not-found-hooks` are run first. See section [AD.2.3 Hooks](emacs.html#SEC484).

There are several ways to specify automatically the major mode for
editing the file (see section [R.1 How Major Modes are Chosen](emacs.html#SEC228)), and to specify local
variables defined for that file (see section [AD.2.5 Local Variables in Files](emacs.html#SEC486)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC112) ] | [ [>](emacs.html#SEC114) ] | [ [<<](emacs.html#SEC121) ] | [ [Up](emacs.html#SEC110) ] | [ [>>](emacs.html#SEC121) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## M.3 Saving Files

*Saving* a buffer in Emacs means writing its contents back into the file
that was visited in the buffer.

`C-x C-s`: Save the current buffer in its visited file on disk (`save-buffer`). `C-x s`: Save any or all buffers in their visited files (`save-some-buffers`). `M-~`: Forget that the current buffer has been changed (`not-modified`). With prefix argument (`C-u`), mark the current buffer as changed. `C-x C-w`: Save the current buffer as a specified file name (`write-file`). `M-x set-visited-file-name`: Change the file name under which the current buffer will be saved.

When you wish to save the file and make your changes permanent, type
`C-x C-s` (`save-buffer`). After saving is finished, `C-x C-s`
displays a message like this:

|  |  |
| --- | --- |
|  | ``` Wrote /u/rms/gnu/gnu.tasks ``` |

If the selected buffer is not modified (no changes have been made in it
since the buffer was created or last saved), saving is not really done,
because it would have no effect. Instead, `C-x C-s` displays a message
like this in the echo area:

|  |  |
| --- | --- |
|  | ``` (No changes need to be saved) ``` |

The command `C-x s` (`save-some-buffers`) offers to save any
or all modified buffers. It asks you what to do with each buffer. The
possible responses are analogous to those of `query-replace`:

`y`: Save this buffer and ask about the rest of the buffers. `n`: Don't save this buffer, but ask about the rest of the buffers. `!`: Save this buffer and all the rest with no more questions. `RET`: Terminate `save-some-buffers` without any more saving. `.`: Save this buffer, then exit `save-some-buffers` without even asking about other buffers. `C-r`: View the buffer that you are currently being asked about. When you exit View mode, you get back to `save-some-buffers`, which asks the question again. `C-h`: Display a help message about these options.

`C-x C-c`, the key sequence to exit Emacs, invokes
`save-some-buffers` and therefore asks the same questions.

If you have changed a buffer but you do not want to save the changes,
you should take some action to prevent it. Otherwise, each time you use
`C-x s` or `C-x C-c`, you are liable to save this buffer by
mistake. One thing you can do is type `M-~` (`not-modified`),
which clears out the indication that the buffer is modified. If you do
this, none of the save commands will believe that the buffer needs to be
saved. (`` `~' `` is often used as a mathematical symbol for `not'; thus
`M-~` is `not', metafied.) You could also use
`set-visited-file-name` (see below) to mark the buffer as visiting
a different file name, one which is not in use for anything important.
Alternatively, you can cancel all the changes made since the file was
visited or saved, by reading the text from the file again. This is
called *reverting*. See section [M.4 Reverting a Buffer](emacs.html#SEC121). You could also undo all the
changes by repeating the undo command `C-x u` until you have undone
all the changes; but reverting is easier.

`M-x set-visited-file-name` alters the name of the file that the
current buffer is visiting. It reads the new file name using the
minibuffer. Then it marks the buffer as visiting that file name, and
changes the buffer name correspondingly. `set-visited-file-name`
does not save the buffer in the newly visited file; it just alters the
records inside Emacs in case you do save later. It also marks the
buffer as "modified" so that `C-x C-s` in that buffer
*will* save.

If you wish to mark the buffer as visiting a different file and save it
right away, use `C-x C-w` (`write-file`). It is precisely
equivalent to `set-visited-file-name` followed by `C-x C-s`.
`C-x C-s` used on a buffer that is not visiting a file has the
same effect as `C-x C-w`; that is, it reads a file name, marks the
buffer as visiting that file, and saves it there. The default file name in
a buffer that is not visiting a file is made by combining the buffer name
with the buffer's default directory (see section [M.1 File Names](emacs.html#SEC111)).

If the new file name implies a major mode, then `C-x C-w` switches
to that major mode, in most cases. The command
`set-visited-file-name` also does this. See section [R.1 How Major Modes are Chosen](emacs.html#SEC228).

If Emacs is about to save a file and sees that the date of the latest
version on disk does not match what Emacs last read or wrote, Emacs
notifies you of this fact, because it probably indicates a problem caused
by simultaneous editing and requires your immediate attention.
See section [Simultaneous Editing](emacs.html#SEC118).

If the value of the variable `require-final-newline` is `t`,
Emacs silently puts a newline at the end of any file that doesn't
already end in one, every time a file is saved or written. If the value
is `nil`, Emacs leaves the end of the file unchanged; if it's
neither `nil` nor `t`, Emacs asks you whether to add a
newline. The default is `nil`.

> |  |  |  |
> | --- | --- | --- |
> | [M.3.1 Backup Files](emacs.html#SEC114) |  | How Emacs saves the old version of your file. |
> | [M.3.2 Protection against Simultaneous Editing](emacs.html#SEC118) |  | How Emacs protects against simultaneous editing of one file by two users. |
> | [M.3.3 Shadowing Files](emacs.html#SEC119) |  | Copying files to "shadows" automatically. |
> | [M.3.4 Updating Time Stamps Automatically](emacs.html#SEC120) |  | Emacs can update time stamps on saved files. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC113) ] | [ [>](emacs.html#SEC115) ] | [ [<<](emacs.html#SEC121) ] | [ [Up](emacs.html#SEC113) ] | [ [>>](emacs.html#SEC118) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
