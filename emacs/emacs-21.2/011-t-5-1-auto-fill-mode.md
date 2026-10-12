> |  |  |  |
> | --- | --- | --- |
> | [T.5.1 Auto Fill Mode](emacs.html#SEC239) |  | Auto Fill mode breaks long lines automatically. |
> | [T.5.2 Refill Mode](emacs.html#SEC240) |  | Keeping paragraphs filled. |
> | [T.5.3 Explicit Fill Commands](emacs.html#SEC241) |  | Commands to refill paragraphs and center lines. |
> | [T.5.4 The Fill Prefix](emacs.html#SEC242) |  | Filling paragraphs that are indented or in a comment, etc. |
> | [T.5.5 Adaptive Filling](emacs.html#SEC243) |  | How Emacs can determine the fill prefix automatically. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC238) ] | [ [>](emacs.html#SEC240) ] | [ [<<](emacs.html#SEC244) ] | [ [Up](emacs.html#SEC238) ] | [ [>>](emacs.html#SEC244) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.5.1 Auto Fill Mode

*Auto Fill* mode is a minor mode in which lines are broken
automatically when they become too wide. Breaking happens only when
you type a `SPC` or `RET`.

`M-x auto-fill-mode`: Enable or disable Auto Fill mode. `SPC`: `RET`: In Auto Fill mode, break lines when appropriate.

`M-x auto-fill-mode` turns Auto Fill mode on if it was off, or off
if it was on. With a positive numeric argument it always turns Auto
Fill mode on, and with a negative argument always turns it off. You can
see when Auto Fill mode is in effect by the presence of the word
`` `Fill' `` in the mode line, inside the parentheses. Auto Fill mode is
a minor mode which is enabled or disabled for each buffer individually.
See section [AD.1 Minor Modes](emacs.html#SEC476).

In Auto Fill mode, lines are broken automatically at spaces when they
get longer than the desired width. Line breaking and rearrangement
takes place only when you type `SPC` or `RET`. If you wish to
insert a space or newline without permitting line-breaking, type
`C-q SPC` or `C-q C-j` (recall that a newline is really a
control-J). Also, `C-o` inserts a newline without line breaking.

Auto Fill mode works well with programming-language modes, because it
indents new lines with `TAB`. If a line ending in a comment gets
too long, the text of the comment is split into two comment lines.
Optionally, new comment delimiters are inserted at the end of the first
line and the beginning of the second so that each line is a separate
comment; the variable `comment-multi-line` controls the choice
(see section [U.5 Manipulating Comments](emacs.html#SEC285)).

Adaptive filling (see section [T.5.5 Adaptive Filling](emacs.html#SEC243)) works for Auto Filling as
well as for explicit fill commands. It takes a fill prefix
automatically from the second or first line of a paragraph.

Auto Fill mode does not refill entire paragraphs; it can break lines but
cannot merge lines. So editing in the middle of a paragraph can result in
a paragraph that is not correctly filled. The easiest way to make the
paragraph properly filled again is usually with the explicit fill commands.
See section [T.5.3 Explicit Fill Commands](emacs.html#SEC241).

Many users like Auto Fill mode and want to use it in all text files.
The section on init files says how to arrange this permanently for yourself.
See section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC239) ] | [ [>](emacs.html#SEC241) ] | [ [<<](emacs.html#SEC241) ] | [ [Up](emacs.html#SEC238) ] | [ [>>](emacs.html#SEC244) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.5.2 Refill Mode

Refill minor mode provides support for keeping paragraphs filled as
you type or modify them in other ways. It provides an effect similar
to typical word processor behavior. This works by running a
paragraph-filling command at suitable times.

When you are typing text, only characters which normally trigger
auto filling, like the space character, will trigger refilling. This
is to avoid making it too slow. Apart from self-inserting characters,
other commands which modify the text cause refilling.

The current implementation is preliminary and probably not robust.
We expect to improve on it.

To toggle the use of Refill mode in the current buffer, type
`M-x refill-mode`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC240) ] | [ [>](emacs.html#SEC242) ] | [ [<<](emacs.html#SEC242) ] | [ [Up](emacs.html#SEC238) ] | [ [>>](emacs.html#SEC244) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.5.3 Explicit Fill Commands

`M-q`: Fill current paragraph (`fill-paragraph`). `C-x f`: Set the fill column (`set-fill-column`). `M-x fill-region`: Fill each paragraph in the region (`fill-region`). `M-x fill-region-as-paragraph`: Fill the region, considering it as one paragraph. `M-s`: Center a line.

To refill a paragraph, use the command `M-q`
(`fill-paragraph`). This operates on the paragraph that point is
inside, or the one after point if point is between paragraphs.
Refilling works by removing all the line-breaks, then inserting new ones
where necessary.

To refill many paragraphs, use `M-x fill-region`, which
divides the region into paragraphs and fills each of them.

`M-q` and `fill-region` use the same criteria as `M-h`
for finding paragraph boundaries (see section [T.3 Paragraphs](emacs.html#SEC236)). For more
control, you can use `M-x fill-region-as-paragraph`, which refills
everything between point and mark. This command deletes any blank lines
within the region, so separate blocks of text end up combined into one
block.

A numeric argument to `M-q` causes it to *justify* the text as
well as filling it. This means that extra spaces are inserted to make
the right margin line up exactly at the fill column. To remove the
extra spaces, use `M-q` with no argument. (Likewise for
`fill-region`.) Another way to control justification, and choose
other styles of filling, is with the `justification` text property;
see [T.11.7 Justification in Formatted Text](emacs.html#SEC265).

The command `M-s` (`center-line`) centers the current line
within the current fill column. With an argument n, it centers
n lines individually and moves past them.

The maximum line width for filling is in the variable
`fill-column`. Altering the value of `fill-column` makes it
local to the current buffer; until that time, the default value is in
effect. The default is initially 70. See section [AD.2.4 Local Variables](emacs.html#SEC485). The easiest way
to set `fill-column` is to use the command `C-x f`
(`set-fill-column`). With a numeric argument, it uses that as the
new fill column. With just `C-u` as argument, it sets
`fill-column` to the current horizontal position of point.

Emacs commands normally consider a period followed by two spaces or by
a newline as the end of a sentence; a period followed by just one space
indicates an abbreviation and not the end of a sentence. To preserve
the distinction between these two ways of using a period, the fill
commands do not break a line after a period followed by just one space.

If the variable `sentence-end-double-space` is `nil`, the
fill commands expect and leave just one space at the end of a sentence.
Ordinarily this variable is `t`, so the fill commands insist on
two spaces for the end of a sentence, as explained above. See section [T.2 Sentences](emacs.html#SEC235).

If the variable `colon-double-space` is non-`nil`, the
fill commands put two spaces after a colon.

Some languages do not use period to indicate end of sentence. For
example, a sentence in Thai text ends with double space but without a
period. Set the variable `sentence-end-without-period` to
`t` to tell the sentence commands that a period is not necessary.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC241) ] | [ [>](emacs.html#SEC243) ] | [ [<<](emacs.html#SEC243) ] | [ [Up](emacs.html#SEC238) ] | [ [>>](emacs.html#SEC244) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.5.4 The Fill Prefix

To fill a paragraph in which each line starts with a special marker
(which might be a few spaces, giving an indented paragraph), you can use
the *fill prefix* feature. The fill prefix is a string that Emacs
expects every line to start with, and which is not included in filling.
You can specify a fill prefix explicitly; Emacs can also deduce the
fill prefix automatically (see section [T.5.5 Adaptive Filling](emacs.html#SEC243)).

`C-x .`: Set the fill prefix (`set-fill-prefix`). `M-q`: Fill a paragraph using current fill prefix (`fill-paragraph`). `M-x fill-individual-paragraphs`: Fill the region, considering each change of indentation as starting a new paragraph. `M-x fill-nonuniform-paragraphs`: Fill the region, considering only paragraph-separator lines as starting a new paragraph.

To specify a fill prefix, move to a line that starts with the desired
prefix, put point at the end of the prefix, and give the command
`C-x .` (`set-fill-prefix`). That's a period after the
`C-x`. To turn off the fill prefix, specify an empty prefix: type
`C-x .` with point at the beginning of a line.

When a fill prefix is in effect, the fill commands remove the fill
prefix from each line before filling and insert it on each line after
filling. Auto Fill mode also inserts the fill prefix automatically when
it makes a new line. The `C-o` command inserts the fill prefix on
new lines it creates, when you use it at the beginning of a line
(see section [D.7 Blank Lines](emacs.html#SEC26)). Conversely, the command `M-^` deletes the
prefix (if it occurs) after the newline that it deletes
(see section [S. Indentation](emacs.html#SEC229)).

For example, if `fill-column` is 40 and you set the fill prefix
to `` `;; ' ``, then `M-q` in the following text

|  |  |
| --- | --- |
|  | ``` ;; This is an ;; example of a paragraph ;; inside a Lisp-style comment. ``` |

produces this:

|  |  |
| --- | --- |
|  | ``` ;; This is an example of a paragraph ;; inside a Lisp-style comment. ``` |

Lines that do not start with the fill prefix are considered to start
paragraphs, both in `M-q` and the paragraph commands; this gives
good results for paragraphs with hanging indentation (every line
indented except the first one). Lines which are blank or indented once
the prefix is removed also separate or start paragraphs; this is what
you want if you are writing multi-paragraph comments with a comment
delimiter on each line.

You can use `M-x fill-individual-paragraphs` to set the fill
prefix for each paragraph automatically. This command divides the
region into paragraphs, treating every change in the amount of
indentation as the start of a new paragraph, and fills each of these
paragraphs. Thus, all the lines in one "paragraph" have the same
amount of indentation. That indentation serves as the fill prefix for
that paragraph.

`M-x fill-nonuniform-paragraphs` is a similar command that divides
the region into paragraphs in a different way. It considers only
paragraph-separating lines (as defined by `paragraph-separate`) as
starting a new paragraph. Since this means that the lines of one
paragraph may have different amounts of indentation, the fill prefix
used is the smallest amount of indentation of any of the lines of the
paragraph. This gives good results with styles that indent a paragraph's
first line more or less that the rest of the paragraph.

The fill prefix is stored in the variable `fill-prefix`. Its value
is a string, or `nil` when there is no fill prefix. This is a
per-buffer variable; altering the variable affects only the current buffer,
but there is a default value which you can change as well. See section [AD.2.4 Local Variables](emacs.html#SEC485).

The `indentation` text property provides another way to control
the amount of indentation paragraphs receive. See section [T.11.6 Indentation in Formatted Text](emacs.html#SEC264).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC242) ] | [ [>](emacs.html#SEC244) ] | [ [<<](emacs.html#SEC244) ] | [ [Up](emacs.html#SEC238) ] | [ [>>](emacs.html#SEC244) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.5.5 Adaptive Filling

The fill commands can deduce the proper fill prefix for a paragraph
automatically in certain cases: either whitespace or certain punctuation
characters at the beginning of a line are propagated to all lines of the
paragraph.

If the paragraph has two or more lines, the fill prefix is taken from
the paragraph's second line, but only if it appears on the first line as
well.

If a paragraph has just one line, fill commands *may* take a
prefix from that line. The decision is complicated because there are
three reasonable things to do in such a case:

* Use the first line's prefix on all the lines of the paragraph.

  * Indent subsequent lines with whitespace, so that they line up under the
    text that follows the prefix on the first line, but don't actually copy
    the prefix from the first line.

    * Don't do anything special with the second and following lines.

All three of these styles of formatting are commonly used. So the
fill commands try to determine what you would like, based on the prefix
that appears and on the major mode. Here is how.

If the prefix found on the first line matches
`adaptive-fill-first-line-regexp`, or if it appears to be a
comment-starting sequence (this depends on the major mode), then the
prefix found is used for filling the paragraph, provided it would not
act as a paragraph starter on subsequent lines.

Otherwise, the prefix found is converted to an equivalent number of
spaces, and those spaces are used as the fill prefix for the rest of the
lines, provided they would not act as a paragraph starter on subsequent
lines.

In Text mode, and other modes where only blank lines and page
delimiters separate paragraphs, the prefix chosen by adaptive filling
never acts as a paragraph starter, so it can always be used for filling.

The variable `adaptive-fill-regexp` determines what kinds of line
beginnings can serve as a fill prefix: any characters at the start of
the line that match this regular expression are used. If you set the
variable `adaptive-fill-mode` to `nil`, the fill prefix is
never chosen automatically.

You can specify more complex ways of choosing a fill prefix
automatically by setting the variable `adaptive-fill-function` to a
function. This function is called with point after the left margin of a
line, and it should return the appropriate fill prefix based on that
line. If it returns `nil`, that means it sees no fill prefix in
that line.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC243) ] | [ [>](emacs.html#SEC245) ] | [ [<<](emacs.html#SEC245) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.6 Case Conversion Commands

Emacs has commands for converting either a single word or any arbitrary
range of text to upper case or to lower case.

`M-l`: Convert following word to lower case (`downcase-word`). `M-u`: Convert following word to upper case (`upcase-word`). `M-c`: Capitalize the following word (`capitalize-word`). `C-x C-l`: Convert region to lower case (`downcase-region`). `C-x C-u`: Convert region to upper case (`upcase-region`).

The word conversion commands are the most useful. `M-l`
(`downcase-word`) converts the word after point to lower case, moving
past it. Thus, repeating `M-l` converts successive words.
`M-u` (`upcase-word`) converts to all capitals instead, while
`M-c` (`capitalize-word`) puts the first letter of the word
into upper case and the rest into lower case. All these commands convert
several words at once if given an argument. They are especially convenient
for converting a large amount of text from all upper case to mixed case,
because you can move through the text using `M-l`, `M-u` or
`M-c` on each word as appropriate, occasionally using `M-f` instead
to skip a word.

When given a negative argument, the word case conversion commands apply
to the appropriate number of words before point, but do not move point.
This is convenient when you have just typed a word in the wrong case: you
can give the case conversion command and continue typing.

If a word case conversion command is given in the middle of a word, it
applies only to the part of the word which follows point. This is just
like what `M-d` (`kill-word`) does. With a negative argument,
case conversion applies only to the part of the word before point.

The other case conversion commands are `C-x C-u`
(`upcase-region`) and `C-x C-l` (`downcase-region`), which
convert everything between point and mark to the specified case. Point and
mark do not move.

The region case conversion commands `upcase-region` and
`downcase-region` are normally disabled. This means that they ask
for confirmation if you try to use them. When you confirm, you may
enable the command, which means it will not ask for confirmation again.
See section [AD.4.11 Disabling Commands](emacs.html#SEC502).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC244) ] | [ [>](emacs.html#SEC246) ] | [ [<<](emacs.html#SEC246) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.7 Text Mode

When you edit files of text in a human language, it's more convenient
to use Text mode rather than Fundamental mode. To enter Text mode, type
`M-x text-mode`.

In Text mode, only blank lines and page delimiters separate
paragraphs. As a result, paragraphs can be indented, and adaptive
filling determines what indentation to use when filling a paragraph.
See section [T.5.5 Adaptive Filling](emacs.html#SEC243).

Text mode defines `TAB` to run `indent-relative`
(see section [S. Indentation](emacs.html#SEC229)), so that you can conveniently indent a line like
the previous line. When the previous line is not indented,
`indent-relative` runs `tab-to-tab-stop`, which uses Emacs tab
stops that you can set (see section [S.2 Tab Stops](emacs.html#SEC231)).

Text mode turns off the features concerned with comments except when
you explicitly invoke them. It changes the syntax table so that periods
are not considered part of a word, while apostrophes, backspaces and
underlines are considered part of words.

If you indent the first lines of paragraphs, then you should use
Paragraph-Indent Text mode rather than Text mode. In this mode, you do
not need to have blank lines between paragraphs, because the first-line
indentation is sufficient to start a paragraph; however paragraphs in
which every line is indented are not supported. Use `M-x
paragraph-indent-text-mode` to enter this mode. Use `M-x
paragraph-indent-minor-mode` to enter an equivalent minor mode, for
instance during mail composition.

Text mode, and all the modes based on it, define `M-TAB` as
the command `ispell-complete-word`, which performs completion of
the partial word in the buffer before point, using the spelling
dictionary as the space of possible words. See section [L.4 Checking and Correcting Spelling](emacs.html#SEC109).

Entering Text mode runs the hook `text-mode-hook`. Other major
modes related to Text mode also run this hook, followed by hooks of
their own; this includes Paragraph-Indent Text mode, Nroff mode, TeX
mode, Outline mode, and Mail mode. Hook functions on
`text-mode-hook` can look at the value of `major-mode` to see
which of these modes is actually being entered. See section [AD.2.3 Hooks](emacs.html#SEC484).

Emacs provides two other modes for editing text that is to be passed
through a text formatter to produce fancy formatted printed output.
See section [T.10 Nroff Mode](emacs.html#SEC257), for editing input to the formatter nroff.
See section [T.9 TeX Mode](emacs.html#SEC252), for editing input to the formatter TeX.

Another mode is used for editing outlines. It allows you to view the
text at various levels of detail. You can view either the outline
headings alone or both headings and text; you can also hide some of the
headings at lower levels from view to make the high level structure more
visible. See section [T.8 Outline Mode](emacs.html#SEC246).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC245) ] | [ [>](emacs.html#SEC247) ] | [ [<<](emacs.html#SEC252) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC252) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.8 Outline Mode

Outline mode is a major mode much like Text mode but intended for
editing outlines. It allows you to make parts of the text temporarily
invisible so that you can see the outline structure. Type `M-x
outline-mode` to switch to Outline mode as the major mode of the current
buffer.

When Outline mode makes a line invisible, the line does not appear on
the screen. The screen appears exactly as if the invisible line were
deleted, except that an ellipsis (three periods in a row) appears at the
end of the previous visible line (only one ellipsis no matter how many
invisible lines follow).

Editing commands that operate on lines, such as `C-n` and
`C-p`, treat the text of the invisible line as part of the previous
visible line. Killing an entire visible line, including its terminating
newline, really kills all the following invisible lines along with it.

Outline minor mode provides the same commands as the major mode,
Outline mode, but you can use it in conjunction with other major modes.
Type `M-x outline-minor-mode` to enable the Outline minor mode in
the current buffer. You can also specify this in the text of a file,
with a file local variable of the form `` `mode: outline-minor' ``
(see section [AD.2.5 Local Variables in Files](emacs.html#SEC486)).

The major mode, Outline mode, provides special key bindings on the
`C-c` prefix. Outline minor mode provides similar bindings with
`C-c @` as the prefix; this is to reduce the conflicts with the
major mode's special commands. (The variable
`outline-minor-mode-prefix` controls the prefix used.)

Entering Outline mode runs the hook `text-mode-hook` followed by
the hook `outline-mode-hook` (see section [AD.2.3 Hooks](emacs.html#SEC484)).

> |  |  |  |
> | --- | --- | --- |
> | [T.8.1 Format of Outlines](emacs.html#SEC247) |  | What the text of an outline looks like. |
> | [T.8.2 Outline Motion Commands](emacs.html#SEC248) |  | Special commands for moving through outlines. |
> | [T.8.3 Outline Visibility Commands](emacs.html#SEC249) |  | Commands to control what is visible. |
> | [T.8.4 Viewing One Outline in Multiple Views](emacs.html#SEC250) |  | Outlines and multiple views. |
> | [T.8.5 Folding Editing](emacs.html#SEC251) |  | Folding editing. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC246) ] | [ [>](emacs.html#SEC248) ] | [ [<<](emacs.html#SEC252) ] | [ [Up](emacs.html#SEC246) ] | [ [>>](emacs.html#SEC252) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.8.1 Format of Outlines

Outline mode assumes that the lines in the buffer are of two types:
*heading lines* and *body lines*. A heading line represents a
topic in the outline. Heading lines start with one or more stars; the
number of stars determines the depth of the heading in the outline
structure. Thus, a heading line with one star is a major topic; all the
heading lines with two stars between it and the next one-star heading
are its subtopics; and so on. Any line that is not a heading line is a
body line. Body lines belong with the preceding heading line. Here is
an example:

|  |  |
| --- | --- |
|  | ``` * Food This is the body, which says something about the topic of food.  ** Delicious Food This is the body of the second-level header.  ** Distasteful Food This could have a body too, with several lines.  *** Dormitory Food  * Shelter Another first-level topic with its header line. ``` |

A heading line together with all following body lines is called
collectively an *entry*. A heading line together with all following
deeper heading lines and their body lines is called a *subtree*.

You can customize the criterion for distinguishing heading lines
by setting the variable `outline-regexp`. Any line whose
beginning has a match for this regexp is considered a heading line.
Matches that start within a line (not at the left margin) do not count.
The length of the matching text determines the level of the heading;
longer matches make a more deeply nested level. Thus, for example,
if a text formatter has commands `` `@chapter' ``, `` `@section' ``
and `` `@subsection' `` to divide the document into chapters and
sections, you could make those lines count as heading lines by
setting `outline-regexp` to `` `"@chap\\|@\\(sub\\)*section"' ``.
Note the trick: the two words `` `chapter' `` and `` `section' `` are equally
long, but by defining the regexp to match only `` `chap' `` we ensure
that the length of the text matched on a chapter heading is shorter,
so that Outline mode will know that sections are contained in chapters.
This works as long as no other command starts with `` `@chap' ``.

You can change the rule for calculating the level of a heading line
by setting the variable `outline-level`. The value of
`outline-level` should be a function that takes no arguments and
returns the level of the current heading. Some major modes such as C,
Nroff, and Emacs Lisp mode set this variable and `outline-regexp`
in order to work with Outline minor mode.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC247) ] | [ [>](emacs.html#SEC249) ] | [ [<<](emacs.html#SEC249) ] | [ [Up](emacs.html#SEC246) ] | [ [>>](emacs.html#SEC252) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.8.2 Outline Motion Commands

Outline mode provides special motion commands that move backward and
forward to heading lines.

`C-c C-n`: Move point to the next visible heading line (`outline-next-visible-heading`). `C-c C-p`: Move point to the previous visible heading line (`outline-previous-visible-heading`). `C-c C-f`: Move point to the next visible heading line at the same level as the one point is on (`outline-forward-same-level`). `C-c C-b`: Move point to the previous visible heading line at the same level (`outline-backward-same-level`). `C-c C-u`: Move point up to a lower-level (more inclusive) visible heading line (`outline-up-heading`).

`C-c C-n` (`outline-next-visible-heading`) moves down to the next
heading line. `C-c C-p` (`outline-previous-visible-heading`) moves
similarly backward. Both accept numeric arguments as repeat counts. The
names emphasize that invisible headings are skipped, but this is not really
a special feature. All editing commands that look for lines ignore the
invisible lines automatically.

More powerful motion commands understand the level structure of headings.
`C-c C-f` (`outline-forward-same-level`) and
`C-c C-b` (`outline-backward-same-level`) move from one
heading line to another visible heading at the same depth in
the outline. `C-c C-u` (`outline-up-heading`) moves
backward to another heading that is less deeply nested.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC248) ] | [ [>](emacs.html#SEC250) ] | [ [<<](emacs.html#SEC250) ] | [ [Up](emacs.html#SEC246) ] | [ [>>](emacs.html#SEC252) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.8.3 Outline Visibility Commands

The other special commands of outline mode are used to make lines visible
or invisible. Their names all start with `hide` or `show`.
Most of them fall into pairs of opposites. They are not undoable; instead,
you can undo right past them. Making lines visible or invisible is simply
not recorded by the undo mechanism.

`C-c C-t`: Make all body lines in the buffer invisible (`hide-body`). `C-c C-a`: Make all lines in the buffer visible (`show-all`). `C-c C-d`: Make everything under this heading invisible, not including this heading itself (`hide-subtree`). `C-c C-s`: Make everything under this heading visible, including body, subheadings, and their bodies (`show-subtree`). `C-c C-l`: Make the body of this heading line, and of all its subheadings, invisible (`hide-leaves`). `C-c C-k`: Make all subheadings of this heading line, at all levels, visible (`show-branches`). `C-c C-i`: Make immediate subheadings (one level down) of this heading line visible (`show-children`). `C-c C-c`: Make this heading line's body invisible (`hide-entry`). `C-c C-e`: Make this heading line's body visible (`show-entry`). `C-c C-q`: Hide everything except the top n levels of heading lines (`hide-sublevels`). `C-c C-o`: Hide everything except for the heading or body that point is in, plus the headings leading up from there to the top level of the outline (`hide-other`).

Two commands that are exact opposites are `C-c C-c`
(`hide-entry`) and `C-c C-e` (`show-entry`). They are
used with point on a heading line, and apply only to the body lines of
that heading. Subheadings and their bodies are not affected.

Two more powerful opposites are `C-c C-d` (`hide-subtree`) and
`C-c C-s` (`show-subtree`). Both expect to be used when point is
on a heading line, and both apply to all the lines of that heading's
*subtree*: its body, all its subheadings, both direct and indirect, and
all of their bodies. In other words, the subtree contains everything
following this heading line, up to and not including the next heading of
the same or higher rank.

Intermediate between a visible subtree and an invisible one is having
all the subheadings visible but none of the body. There are two
commands for doing this, depending on whether you want to hide the
bodies or make the subheadings visible. They are `C-c C-l`
(`hide-leaves`) and `C-c C-k` (`show-branches`).

A little weaker than `show-branches` is `C-c C-i`
(`show-children`). It makes just the direct subheadings
visible--those one level down. Deeper subheadings remain invisible, if
they were invisible.

Two commands have a blanket effect on the whole file. `C-c C-t`
(`hide-body`) makes all body lines invisible, so that you see just
the outline structure. `C-c C-a` (`show-all`) makes all lines
visible. These commands can be thought of as a pair of opposites even
though `C-c C-a` applies to more than just body lines.

The command `C-c C-q` (`hide-sublevels`) hides all but the
top level headings. With a numeric argument n, it hides everything
except the top n levels of heading lines.

The command `C-c C-o` (`hide-other`) hides everything except
the heading or body text that point is in, plus its parents (the headers
leading up from there to top level in the outline).

You can turn off the use of ellipses at the ends of visible lines by
setting `selective-display-ellipses` to `nil`. Then there is
no visible indication of the presence of invisible lines.

When incremental search finds text that is hidden by Outline mode,
it makes that part of the buffer visible. If you exit the search
at that position, the text remains visible.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC249) ] | [ [>](emacs.html#SEC251) ] | [ [<<](emacs.html#SEC251) ] | [ [Up](emacs.html#SEC246) ] | [ [>>](emacs.html#SEC252) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.8.4 Viewing One Outline in Multiple Views

You can display two views of a single outline at the same time, in
different windows. To do this, you must create an indirect buffer using
`M-x make-indirect-buffer`. The first argument of this command is
the existing outline buffer name, and its second argument is the name to
use for the new indirect buffer. See section [N.6 Indirect Buffers](emacs.html#SEC178).

Once the indirect buffer exists, you can display it in a window in the
normal fashion, with `C-x 4 b` or other Emacs commands. The Outline
mode commands to show and hide parts of the text operate on each buffer
independently; as a result, each buffer can have its own view. If you
want more than two views on the same outline, create additional indirect
buffers.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC250) ] | [ [>](emacs.html#SEC252) ] | [ [<<](emacs.html#SEC252) ] | [ [Up](emacs.html#SEC246) ] | [ [>>](emacs.html#SEC252) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.8.5 Folding Editing

The Foldout package extends Outline mode and Outline minor mode with
"folding" commands. The idea of folding is that you zoom in on a
nested portion of the outline, while hiding its relatives at higher
levels.

Consider an Outline mode buffer all the text and subheadings under
level-1 headings hidden. To look at what is hidden under one of these
headings, you could use `C-c C-e` (`M-x show-entry`) to expose
the body, or `C-c C-i` to expose the child (level-2) headings.

With Foldout, you use `C-c C-z` (`M-x foldout-zoom-subtree`).
This exposes the body and child subheadings, and narrows the buffer so
that only the level-1 heading, the body and the level-2 headings are
visible. Now to look under one of the level-2 headings, position the
cursor on it and use `C-c C-z` again. This exposes the level-2 body
and its level-3 child subheadings and narrows the buffer again. Zooming
in on successive subheadings can be done as much as you like. A string
in the mode line shows how deep you've gone.

When zooming in on a heading, to see only the child subheadings specify
a numeric argument: `C-u C-c C-z`. The number of levels of children
can be specified too (compare `M-x show-children`), e.g. `M-2
C-c C-z` exposes two levels of child subheadings. Alternatively, the
body can be specified with a negative argument: `M-- C-c C-z`. The
whole subtree can be expanded, similarly to `C-c C-s` (`M-x
show-subtree`), by specifying a zero argument: `M-0 C-c C-z`.

While you're zoomed in, you can still use Outline mode's exposure and
hiding functions without disturbing Foldout. Also, since the buffer is
narrowed, "global" editing actions will only affect text under the
zoomed-in heading. This is useful for restricting changes to a
particular chapter or section of your document.

To unzoom (exit) a fold, use `C-c C-x` (`M-x foldout-exit-fold`).
This hides all the text and subheadings under the top-level heading and
returns you to the previous view of the buffer. Specifying a numeric
argument exits that many levels of folds. Specifying a zero argument exits all
folds.

To cancel the narrowing of a fold without hiding the text and
subheadings, specify a negative argument. For example, `M--2 C-c
C-x` exits two folds and leaves the text and subheadings exposed.

Foldout mode also provides mouse commands for entering and exiting
folds, and for showing and hiding text:

`M-C-Mouse-1` zooms in on the heading clicked on: * single click: expose body.* double click: expose subheadings.* triple click: expose body and subheadings.* quad click: expose entire subtree. `M-C-Mouse-2` exposes text under the heading clicked on: * single click: expose body.* double click: expose subheadings.* triple click: expose body and subheadings.* quad click: expose entire subtree. `M-C-Mouse-3` hides text under the heading clicked on or exits fold: * single click: hide subtree.* double click: exit fold and hide text.* triple click: exit fold without hiding text.* quad click: exit all folds and hide text.

You can specify different modifier keys (instead of
`Control-Meta-`) by setting `foldout-mouse-modifiers`; but if
you have already loaded the `foldout.el' library, you must reload
it in order for this to take effect.

To use the Foldout package, you can type `M-x load-library
RET foldout RET`; or you can arrange for to do that
automatically by putting this in your `.emacs' file:

|  |  |
| --- | --- |
|  | ``` (eval-after-load "outline" '(require 'foldout)) ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC251) ] | [ [>](emacs.html#SEC253) ] | [ [<<](emacs.html#SEC246) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC257) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.9 TeX Mode

TeX is a powerful text formatter written by Donald Knuth; it is also
free, like GNU Emacs. LaTeX is a simplified input format for TeX,
implemented by TeX macros; it comes with TeX. SliTeX is a special
form of LaTeX.[(8)](emacs.html#FOOT8)

Emacs has a special TeX mode for editing TeX input files.
It provides facilities for checking the balance of delimiters and for
invoking TeX on all or part of the file.

TeX mode has three variants, Plain TeX mode, LaTeX mode, and
SliTeX mode (these three distinct major modes differ only slightly).
They are designed for editing the three different formats. The command
`M-x tex-mode` looks at the contents of the buffer to determine
whether the contents appear to be either LaTeX input or SliTeX
input; if so, it selects the appropriate mode. If the file contents do
not appear to be LaTeX or SliTeX, it selects Plain TeX mode.
If the contents are insufficient to determine this, the variable
`tex-default-mode` controls which mode is used.

When `M-x tex-mode` does not guess right, you can use the commands
`M-x plain-tex-mode`, `M-x latex-mode`, and `M-x
slitex-mode` to select explicitly the particular variants of TeX
mode.

> |  |  |  |
> | --- | --- | --- |
> | [T.9.1 TeX Editing Commands](emacs.html#SEC253) |  | Special commands for editing in TeX mode. |
> | [T.9.2 LaTeX Editing Commands](emacs.html#SEC254) |  | Additional commands for LaTeX input files. |
> | [T.9.3 TeX Printing Commands](emacs.html#SEC255) |  | Commands for printing part of a file with TeX. |
> | [T.9.4 TeX Mode Miscellany](emacs.html#SEC256) |  | Customization of TeX mode, and related features. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC252) ] | [ [>](emacs.html#SEC254) ] | [ [<<](emacs.html#SEC246) ] | [ [Up](emacs.html#SEC252) ] | [ [>>](emacs.html#SEC257) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.9.1 TeX Editing Commands

Here are the special commands provided in TeX mode for editing the
text of the file.

`"`: Insert, according to context, either `` `"' `` or `` `"' `` or `` `"' `` (`tex-insert-quote`). `C-j`: Insert a paragraph break (two newlines) and check the previous paragraph for unbalanced braces or dollar signs (`tex-terminate-paragraph`). `M-x tex-validate-region`: Check each paragraph in the region for unbalanced braces or dollar signs. `C-c {`: Insert `` `{}' `` and position point between them (`tex-insert-braces`). `C-c }`: Move forward past the next unmatched close brace (`up-list`).

In TeX, the character `` `"' `` is not normally used; we use
`` `"' `` to start a quotation and `` `"' `` to end one. To make
editing easier under this formatting convention, TeX mode overrides
the normal meaning of the key `"` with a command that inserts a pair
of single-quotes or backquotes (`tex-insert-quote`). To be
precise, this command inserts `` `"' `` after whitespace or an open
brace, `` `"' `` after a backslash, and `` `"' `` after any other
character.

If you need the character `` `"' `` itself in unusual contexts, use
`C-q` to insert it. Also, `"` with a numeric argument always
inserts that number of `` `"' `` characters. You can turn off the
feature of `"` expansion by eliminating that binding in the local
map (see section [AD.4 Customizing Key Bindings](emacs.html#SEC491)).

In TeX mode, `` `$' `` has a special syntax code which attempts to
understand the way TeX math mode delimiters match. When you insert a
`` `$' `` that is meant to exit math mode, the position of the matching
`` `$' `` that entered math mode is displayed for a second. This is the
same feature that displays the open brace that matches a close brace that
is inserted. However, there is no way to tell whether a `` `$' `` enters
math mode or leaves it; so when you insert a `` `$' `` that enters math
mode, the previous `` `$' `` position is shown as if it were a match, even
though they are actually unrelated.

TeX uses braces as delimiters that must match. Some users prefer
to keep braces balanced at all times, rather than inserting them
singly. Use `C-c {` (`tex-insert-braces`) to insert a pair of
braces. It leaves point between the two braces so you can insert the
text that belongs inside. Afterward, use the command `C-c }`
(`up-list`) to move forward past the close brace.

There are two commands for checking the matching of braces. `C-j`
(`tex-terminate-paragraph`) checks the paragraph before point, and
inserts two newlines to start a new paragraph. It outputs a message in
the echo area if any mismatch is found. `M-x tex-validate-region`
checks a region, paragraph by paragraph. The errors are listed in the
`` `*Occur*' `` buffer, and you can use `C-c C-c` or `Mouse-2` in
that buffer to go to a particular mismatch.

Note that Emacs commands count square brackets and parentheses in
TeX mode, not just braces. This is not strictly correct for the
purpose of checking TeX syntax. However, parentheses and square
brackets are likely to be used in text as matching delimiters and it is
useful for the various motion commands and automatic match display to
work with them.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC253) ] | [ [>](emacs.html#SEC255) ] | [ [<<](emacs.html#SEC255) ] | [ [Up](emacs.html#SEC252) ] | [ [>>](emacs.html#SEC257) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.9.2 LaTeX Editing Commands

LaTeX mode, and its variant, SliTeX mode, provide a few extra
features not applicable to plain TeX.

`C-c C-o`: Insert `` `\begin' `` and `` `\end' `` for LaTeX block and position point on a line between them (`tex-latex-block`). `C-c C-e`: Close the innermost LaTeX block not yet closed (`tex-close-latex-block`).

In LaTeX input, `` `\begin' `` and `` `\end' `` commands are used to
group blocks of text. To insert a `` `\begin' `` and a matching
`` `\end' `` (on a new line following the `` `\begin' ``), use `C-c
C-o` (`tex-latex-block`). A blank line is inserted between the
two, and point is left there. You can use completion when you enter the
block type; to specify additional block type names beyond the standard
list, set the variable `latex-block-names`. For example, here's
how to add `` `theorem' ``, `` `corollary' ``, and `` `proof' ``:

|  |  |
| --- | --- |
|  | ``` (setq latex-block-names '("theorem" "corollary" "proof")) ``` |

In LaTeX input, `` `\begin' `` and `` `\end' `` commands must
balance. You can use `C-c C-e` (`tex-close-latex-block`) to
insert automatically a matching `` `\end' `` to match the last unmatched
`` `\begin' ``. It indents the `` `\end' `` to match the corresponding
`` `\begin' ``. It inserts a newline after `` `\end' `` if point is at
the beginning of a line.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC254) ] | [ [>](emacs.html#SEC256) ] | [ [<<](emacs.html#SEC256) ] | [ [Up](emacs.html#SEC252) ] | [ [>>](emacs.html#SEC257) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.9.3 TeX Printing Commands

You can invoke TeX as an inferior of Emacs on either the entire
contents of the buffer or just a region at a time. Running TeX in
this way on just one chapter is a good way to see what your changes
look like without taking the time to format the entire file.

`C-c C-r`: Invoke TeX on the current region, together with the buffer's header (`tex-region`). `C-c C-b`: Invoke TeX on the entire current buffer (`tex-buffer`). `C-c TAB`: Invoke BibTeX on the current file (`tex-bibtex-file`). `C-c C-f`: Invoke TeX on the current file (`tex-file`). `C-c C-l`: Recenter the window showing output from the inferior TeX so that the last line can be seen (`tex-recenter-output-buffer`). `C-c C-k`: Kill the TeX subprocess (`tex-kill-job`). `C-c C-p`: Print the output from the last `C-c C-r`, `C-c C-b`, or `C-c C-f` command (`tex-print`). `C-c C-v`: Preview the output from the last `C-c C-r`, `C-c C-b`, or `C-c C-f` command (`tex-view`). `C-c C-q`: Show the printer queue (`tex-show-print-queue`).

You can pass the current buffer through an inferior TeX by means of
`C-c C-b` (`tex-buffer`). The formatted output appears in a
temporary file; to print it, type `C-c C-p` (`tex-print`).
Afterward, you can use `C-c C-q` (`tex-show-print-queue`) to
view the progress of your output towards being printed. If your terminal
has the ability to display TeX output files, you can preview the
output on the terminal with `C-c C-v` (`tex-view`).

You can specify the directory to use for running TeX by setting the
variable `tex-directory`. `"."` is the default value. If
your environment variable `TEXINPUTS` contains relative directory
names, or if your files contains `` `\input' `` commands with relative
file names, then `tex-directory` *must* be `"."` or you
will get the wrong results. Otherwise, it is safe to specify some other
directory, such as `"/tmp"`.

If you want to specify which shell commands are used in the inferior TeX,
you can do so by setting the values of the variables `tex-run-command`,
`latex-run-command`, `slitex-run-command`,
`tex-dvi-print-command`, `tex-dvi-view-command`, and
`tex-show-queue-command`. You *must* set the value of
`tex-dvi-view-command` for your particular terminal; this variable
has no default value. The other variables have default values that may
(or may not) be appropriate for your system.

Normally, the file name given to these commands comes at the end of
the command string; for example, `` `latex filename' ``. In some
cases, however, the file name needs to be embedded in the command; an
example is when you need to provide the file name as an argument to one
command whose output is piped to another. You can specify where to put
the file name with `` `*' `` in the command string. For example,

|  |  |
| --- | --- |
|  | ``` (setq tex-dvi-print-command "dvips -f * | lpr") ``` |

The terminal output from TeX, including any error messages, appears
in a buffer called `` `*tex-shell*' ``. If TeX gets an error, you can
switch to this buffer and feed it input (this works as in Shell mode;
see section [AC.15.2 Interactive Inferior Shell](emacs.html#SEC444)). Without switching to this buffer you can
scroll it so that its last line is visible by typing `C-c
C-l`.

Type `C-c C-k` (`tex-kill-job`) to kill the TeX process if
you see that its output is no longer useful. Using `C-c C-b` or
`C-c C-r` also kills any TeX process still running.

You can also pass an arbitrary region through an inferior TeX by typing
`C-c C-r` (`tex-region`). This is tricky, however, because most files
of TeX input contain commands at the beginning to set parameters and
define macros, without which no later part of the file will format
correctly. To solve this problem, `C-c C-r` allows you to designate a
part of the file as containing essential commands; it is included before
the specified region as part of the input to TeX. The designated part
of the file is called the *header*.

To indicate the bounds of the header in Plain TeX mode, you insert two
special strings in the file. Insert `` `%**start of header' `` before the
header, and `` `%**end of header' `` after it. Each string must appear
entirely on one line, but there may be other text on the line before or
after. The lines containing the two strings are included in the header.
If `` `%**start of header' `` does not appear within the first 100 lines of
the buffer, `C-c C-r` assumes that there is no header.

In LaTeX mode, the header begins with `` `\documentclass' `` or
`` `\documentstyle' `` and ends with `` `\begin{document}' ``. These
are commands that LaTeX requires you to use in any case, so nothing
special needs to be done to identify the header.

The commands (`tex-buffer`) and (`tex-region`) do all of their
work in a temporary directory, and do not have available any of the auxiliary
files needed by TeX for cross-references; these commands are generally
not suitable for running the final copy in which all of the cross-references
need to be correct.

When you want the auxiliary files for cross references, use `C-c
C-f` (`tex-file`) which runs TeX on the current buffer's file,
in that file's directory. Before running TeX, it offers to save any
modified buffers. Generally, you need to use (`tex-file`) twice to
get the cross-references right.

The value of the variable `tex-start-options-string` specifies
options for the TeX run. The default value causes TeX to run in
nonstop mode. To run TeX interactively, set the variable to `""`.

Large TeX documents are often split into several files--one main
file, plus subfiles. Running TeX on a subfile typically does not
work; you have to run it on the main file. In order to make
`tex-file` useful when you are editing a subfile, you can set the
variable `tex-main-file` to the name of the main file. Then
`tex-file` runs TeX on that file.

The most convenient way to use `tex-main-file` is to specify it
in a local variable list in each of the subfiles. See section [AD.2.5 Local Variables in Files](emacs.html#SEC486).

For LaTeX files, you can use BibTeX to process the auxiliary
file for the current buffer's file. BibTeX looks up bibliographic
citations in a data base and prepares the cited references for the
bibliography section. The command `C-c TAB`
(`tex-bibtex-file`) runs the shell command
(`tex-bibtex-command`) to produce a `` `.bbl' `` file for the
current buffer's file. Generally, you need to do `C-c C-f`
(`tex-file`) once to generate the `` `.aux' `` file, then do
`C-c TAB` (`tex-bibtex-file`), and then repeat `C-c C-f`
(`tex-file`) twice more to get the cross-references correct.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC255) ] | [ [>](emacs.html#SEC257) ] | [ [<<](emacs.html#SEC246) ] | [ [Up](emacs.html#SEC252) ] | [ [>>](emacs.html#SEC257) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.9.4 TeX Mode Miscellany

Entering any variant of TeX mode runs the hooks
`text-mode-hook` and `tex-mode-hook`. Then it runs either
`plain-tex-mode-hook`, `latex-mode-hook`, or
`slitex-mode-hook`, whichever is appropriate. Starting the
TeX shell runs the hook `tex-shell-hook`. See section [AD.2.3 Hooks](emacs.html#SEC484).

TeX encoding
The commands `M-x iso-iso2tex`, `M-x iso-tex2iso`, `M-x
iso-iso2gtex` and `M-x iso-gtex2iso` can be used to convert
between Latin-1 encoded files and TeX-encoded equivalents.

For managing all kinds of references for LaTeX, you can use
RefTeX. @xref{Top, , RefTeX, reftex}.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC256) ] | [ [>](emacs.html#SEC258) ] | [ [<<](emacs.html#SEC235) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.10 Nroff Mode

Nroff mode is a mode like Text mode but modified to handle nroff commands
present in the text. Invoke `M-x nroff-mode` to enter this mode. It
differs from Text mode in only a few ways. All nroff command lines are
considered paragraph separators, so that filling will never garble the
nroff commands. Pages are separated by `` `.bp' `` commands. Comments
start with backslash-doublequote. Also, three special commands are
provided that are not in Text mode:

`M-n`: Move to the beginning of the next line that isn't an nroff command (`forward-text-line`). An argument is a repeat count. `M-p`: Like `M-n` but move up (`backward-text-line`). `M-?`: Displays in the echo area the number of text lines (lines that are not nroff commands) in the region (`count-text-lines`).

The other feature of Nroff mode is that you can turn on Electric Nroff
mode. This is a minor mode that you can turn on or off with `M-x
electric-nroff-mode` (see section [AD.1 Minor Modes](emacs.html#SEC476)). When the mode is on, each
time you use `RET` to end a line that contains an nroff command that
opens a kind of grouping, the matching nroff command to close that
grouping is automatically inserted on the following line. For example,
if you are at the beginning of a line and type `. ( b RET`,
this inserts the matching command `` `.)b' `` on a new line following
point.

If you use Outline minor mode with Nroff mode (see section [T.8 Outline Mode](emacs.html#SEC246)),
heading lines are lines of the form `` `.H' `` followed by a number (the
header level).

Entering Nroff mode runs the hook `text-mode-hook`, followed by
the hook `nroff-mode-hook` (see section [AD.2.3 Hooks](emacs.html#SEC484)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC257) ] | [ [>](emacs.html#SEC259) ] | [ [<<](emacs.html#SEC235) ] | [ [Up](emacs.html#SEC233) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## T.11 Editing Formatted Text

*Enriched mode* is a minor mode for editing files that contain
formatted text in WYSIWYG fashion, as in a word processor. Currently,
formatted text in Enriched mode can specify fonts, colors, underlining,
margins, and types of filling and justification. In the future, we plan
to implement other formatting features as well.

Enriched mode is a minor mode (see section [AD.1 Minor Modes](emacs.html#SEC476)). It is
typically used in conjunction with Text mode (see section [T.7 Text Mode](emacs.html#SEC245)), but
you can also use it with other major modes such as Outline mode and
Paragraph-Indent Text mode.

Potentially, Emacs can store formatted text files in various file
formats. Currently, only one format is implemented: *text/enriched*
format, which is defined by the MIME protocol. See section `Format Conversion' in the Emacs Lisp Reference Manual,
for details of how Emacs recognizes and converts file formats.

The Emacs distribution contains a formatted text file that can serve as
an example. Its name is `etc/enriched.doc'. It contains samples
illustrating all the features described in this section. It also
contains a list of ideas for future enhancements.

> |  |  |  |
> | --- | --- | --- |
> | [T.11.1 Requesting to Edit Formatted Text](emacs.html#SEC259) |  | Entering and exiting Enriched mode. |
> | [T.11.2 Hard and Soft Newlines](emacs.html#SEC260) |  | There are two different kinds of newlines. |
> | [T.11.3 Editing Format Information](emacs.html#SEC261) |  | How to edit text properties. |
> | [T.11.4 Faces in Formatted Text](emacs.html#SEC262) |  | Bold, italic, underline, etc. |
> | [T.11.5 Colors in Formatted Text](emacs.html#SEC263) |  | Changing the color of text. |
> | [T.11.6 Indentation in Formatted Text](emacs.html#SEC264) |  | Changing the left and right margins. |
> | [T.11.7 Justification in Formatted Text](emacs.html#SEC265) |  | Centering, setting text flush with the left or right margin, etc. |
> | [T.11.8 Setting Other Text Properties](emacs.html#SEC266) |  | The "special" text properties submenu. |
> | [T.11.9 Forcing Enriched Mode](emacs.html#SEC267) |  | How to force use of Enriched mode. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC258) ] | [ [>](emacs.html#SEC260) ] | [ [<<](emacs.html#SEC235) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.1 Requesting to Edit Formatted Text

Whenever you visit a file that Emacs saved in the text/enriched format,
Emacs automatically converts the formatting information in the file into
Emacs's own internal format (text properties), and turns on Enriched
mode.

To create a new file of formatted text, first visit the nonexistent
file, then type `M-x enriched-mode` before you start inserting text.
This command turns on Enriched mode. Do this before you begin inserting
text, to ensure that the text you insert is handled properly.

More generally, the command `enriched-mode` turns Enriched mode
on if it was off, and off if it was on. With a prefix argument, this
command turns Enriched mode on if the argument is positive, and turns
the mode off otherwise.

When you save a buffer while Enriched mode is enabled in it, Emacs
automatically converts the text to text/enriched format while writing it
into the file. When you visit the file again, Emacs will automatically
recognize the format, reconvert the text, and turn on Enriched mode
again.

Normally, after visiting a file in text/enriched format, Emacs refills
each paragraph to fit the specified right margin. You can turn off this
refilling, to save time, by setting the variable
`enriched-fill-after-visiting` to `nil` or to `ask`.

However, when visiting a file that was saved from Enriched mode, there
is no need for refilling, because Emacs saves the right margin settings
along with the text.

You can add annotations for saving additional text properties, which
Emacs normally does not save, by adding to `enriched-translations`.
Note that the text/enriched standard requires any non-standard
annotations to have names starting with `` `x-' ``, as in
`` `x-read-only' ``. This ensures that they will not conflict with
standard annotations that may be added later.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC259) ] | [ [>](emacs.html#SEC261) ] | [ [<<](emacs.html#SEC261) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.2 Hard and Soft Newlines

In formatted text, Emacs distinguishes between two different kinds of
newlines, *hard* newlines and *soft* newlines.

Hard newlines are used to separate paragraphs, or items in a list, or
anywhere that there should always be a line break regardless of the
margins. The `RET` command (`newline`) and `C-o`
(`open-line`) insert hard newlines.

Soft newlines are used to make text fit between the margins. All the
fill commands, including Auto Fill, insert soft newlines--and they
delete only soft newlines.

Although hard and soft newlines look the same, it is important to bear
the difference in mind. Do not use `RET` to break lines in the
middle of filled paragraphs, or else you will get hard newlines that are
barriers to further filling. Instead, let Auto Fill mode break lines,
so that if the text or the margins change, Emacs can refill the lines
properly. See section [T.5.1 Auto Fill Mode](emacs.html#SEC239).

On the other hand, in tables and lists, where the lines should always
remain as you type them, you can use `RET` to end lines. For these
lines, you may also want to set the justification style to
`unfilled`. See section [T.11.7 Justification in Formatted Text](emacs.html#SEC265).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC260) ] | [ [>](emacs.html#SEC262) ] | [ [<<](emacs.html#SEC262) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.3 Editing Format Information

There are two ways to alter the formatting information for a formatted
text file: with keyboard commands, and with the mouse.

The easiest way to add properties to your document is with the Text
Properties menu. You can get to this menu in two ways: from the Edit
menu in the menu bar (use `F10 e t` if you have no mouse),
or with `C-Mouse-2` (hold the `CTRL` key and press the middle
mouse button). There are also keyboard commands described in the
following section.

Most of the items in the Text Properties menu lead to other submenus.
These are described in the sections that follow. Some items run
commands directly:

`Remove Face Properties`: Delete from the region all the text properties that the Text Properties menu works with (`facemenu-remove-face-props`). `Remove All`: Delete *all* text properties from the region (`facemenu-remove-all`). `List Properties`: List all the text properties of the character following point (`list-text-properties-at`). `Display Faces`: Display a list of all the defined faces (`list-faces-display`). `Display Colors`: Display a list of all the defined colors (`list-colors-display`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC261) ] | [ [>](emacs.html#SEC263) ] | [ [<<](emacs.html#SEC263) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.4 Faces in Formatted Text

The Faces submenu lists various Emacs faces including `bold`,
`italic`, and `underline`. Selecting one of these adds the
chosen face to the region. See section [J.1 Using Multiple Typefaces](emacs.html#SEC78). You can also specify a face
with these keyboard commands:

`M-g d`: Set the region, or the next inserted character, to the `default` face (`facemenu-set-default`). `M-g b`: Set the region, or the next inserted character, to the `bold` face (`facemenu-set-bold`). `M-g i`: Set the region, or the next inserted character, to the `italic` face (`facemenu-set-italic`). `M-g l`: Set the region, or the next inserted character, to the `bold-italic` face (`facemenu-set-bold-italic`). `M-g u`: Set the region, or the next inserted character, to the `underline` face (`facemenu-set-underline`). `M-g o face RET`: Set the region, or the next inserted character, to the face face (`facemenu-set-face`).

If you use these commands with a prefix argument--or, in Transient Mark
mode, if the region is not active--then these commands specify a face
to use for your next self-inserting input. See section [H.2 Transient Mark Mode](emacs.html#SEC54). This
applies to both the keyboard commands and the menu commands.

Enriched mode defines two additional faces: `excerpt` and
`fixed`. These correspond to codes used in the text/enriched file
format.

The `excerpt` face is intended for quotations. This face is the
same as `italic` unless you customize it (see section [AD.2.2.3 Customizing Faces](emacs.html#SEC482)).

The `fixed` face means, "Use a fixed-width font for this part
of the text." This makes a visible difference only if you have
specified a variable-width font in the default face; however, even if
the default font is fixed-width, applying the `fixed` face to a
part of the text will cause that part of the text to appear in a
fixed-width font, if the file is ever displayed with a variable-width
default font. This applies to Emacs and to other systems that display
text/enriched format. So if you specifically want a certain part of
the text to use a fixed-width font, you should specify the
`fixed` face for that part.

The `fixed` face is normally set up to use a different font
from the default, even if the default face is also fixed-width.
Different systems have different fonts installed, so you may need to
customize this. See section [AD.2.2.3 Customizing Faces](emacs.html#SEC482).

If your terminal cannot display different faces, you will not be
able to see them, but you can still edit documents containing faces,
and even add faces and colors to documents. The faces you specify
will be visible when the file is viewed on a terminal that can display
them.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC262) ] | [ [>](emacs.html#SEC264) ] | [ [<<](emacs.html#SEC264) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.5 Colors in Formatted Text

You can specify foreground and background colors for portions of the
text. There is a menu for specifying the foreground color and a menu
for specifying the background color. Each color menu lists all the
colors that you have used in Enriched mode in the current Emacs session.

If you specify a color with a prefix argument--or, in Transient Mark
mode, if the region is not active--then it applies to your next
self-inserting input. See section [H.2 Transient Mark Mode](emacs.html#SEC54). Otherwise, the command
applies to the region.

Each color menu contains one additional item: `` `Other' ``. You can use
this item to specify a color that is not listed in the menu; it reads
the color name with the minibuffer. To display list of available colors
and their names, use the `` `Display Colors' `` menu item in the Text
Properties menu (see section [T.11.3 Editing Format Information](emacs.html#SEC261)).

Any color that you specify in this way, or that is mentioned in a
formatted text file that you read in, is added to both color menus for
the duration of the Emacs session.

There are no key bindings for specifying colors, but you can do so
with the extended commands `M-x facemenu-set-foreground` and
`M-x facemenu-set-background`. Both of these commands read the name
of the color with the minibuffer.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC263) ] | [ [>](emacs.html#SEC265) ] | [ [<<](emacs.html#SEC265) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.6 Indentation in Formatted Text

When editing formatted text, you can specify different amounts of
indentation for the right or left margin of an entire paragraph or a
part of a paragraph. The margins you specify automatically affect the
Emacs fill commands (see section [T.5 Filling Text](emacs.html#SEC238)) and line-breaking commands.

The Indentation submenu provides a convenient interface for specifying
these properties. The submenu contains four items:

`Indent More`: Indent the region by 4 columns (`increase-left-margin`). In Enriched mode, this command is also available on `C-x TAB`; if you supply a numeric argument, that says how many columns to add to the margin (a negative argument reduces the number of columns). `Indent Less`: Remove 4 columns of indentation from the region. `Indent Right More`: Make the text narrower by indenting 4 columns at the right margin. `Indent Right Less`: Remove 4 columns of indentation from the right margin.

You can use these commands repeatedly to increase or decrease the
indentation.

The most common way to use these commands is to change the indentation
of an entire paragraph. However, that is not the only use. You can
change the margins at any point; the new values take effect at the end
of the line (for right margins) or the beginning of the next line (for
left margins).

This makes it possible to format paragraphs with *hanging indents*,
which means that the first line is indented less than subsequent lines.
To set up a hanging indent, increase the indentation of the region
starting after the first word of the paragraph and running until the end
of the paragraph.

Indenting the first line of a paragraph is easier. Set the margin for
the whole paragraph where you want it to be for the body of the
paragraph, then indent the first line by inserting extra spaces or tabs.

Sometimes, as a result of editing, the filling of a paragraph becomes
messed up--parts of the paragraph may extend past the left or right
margins. When this happens, use `M-q` (`fill-paragraph`) to
refill the paragraph.

The variable `standard-indent` specifies how many columns these
commands should add to or subtract from the indentation. The default
value is 4. The overall default right margin for Enriched mode is
controlled by the variable `fill-column`, as usual.

The fill prefix, if any, works in addition to the specified paragraph
indentation: `C-x .` does not include the specified indentation's
whitespace in the new value for the fill prefix, and the fill commands
look for the fill prefix after the indentation on each line. See section [T.5.4 The Fill Prefix](emacs.html#SEC242).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC264) ] | [ [>](emacs.html#SEC266) ] | [ [<<](emacs.html#SEC266) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.7 Justification in Formatted Text

When editing formatted text, you can specify various styles of
justification for a paragraph. The style you specify automatically
affects the Emacs fill commands.

The Justification submenu provides a convenient interface for specifying
the style. The submenu contains five items:

`Flush Left`: This is the most common style of justification (at least for English). Lines are aligned at the left margin but left uneven at the right. `Flush Right`: This aligns each line with the right margin. Spaces and tabs are added on the left, if necessary, to make lines line up on the right. `Full`: This justifies the text, aligning both edges of each line. Justified text looks very nice in a printed book, where the spaces can all be adjusted equally, but it does not look as nice with a fixed-width font on the screen. Perhaps a future version of Emacs will be able to adjust the width of spaces in a line to achieve elegant justification. `Center`: This centers every line between the current margins. `None`: This turns off filling entirely. Each line will remain as you wrote it; the fill and auto-fill functions will have no effect on text which has this setting. You can, however, still indent the left margin. In unfilled regions, all newlines are treated as hard newlines (see section [T.11.2 Hard and Soft Newlines](emacs.html#SEC260)) .

In Enriched mode, you can also specify justification from the keyboard
using the `M-j` prefix character:

`M-j l`: Make the region left-filled (`set-justification-left`). `M-j r`: Make the region right-filled (`set-justification-right`). `M-j f`: Make the region fully-justified (`set-justification-full`). `M-j c`: `M-S`: Make the region centered (`set-justification-center`). `M-j u`: Make the region unfilled (`set-justification-none`).

Justification styles apply to entire paragraphs. All the
justification-changing commands operate on the paragraph containing
point, or, if the region is active, on all paragraphs which overlap the
region.

The default justification style is specified by the variable
`default-justification`. Its value should be one of the symbols
`left`, `right`, `full`, `center`, or `none`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC265) ] | [ [>](emacs.html#SEC267) ] | [ [<<](emacs.html#SEC267) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### T.11.8 Setting Other Text Properties

The Other Properties menu lets you add or remove three other useful text
properties: `read-only`, `invisible` and `intangible`.
The `intangible` property disallows moving point within the text,
the `invisible` text property hides text from display, and the
`read-only` property disallows alteration of the text.

Each of these special properties has a menu item to add it to the
region. The last menu item, `` `Remove Special' ``, removes all of these
special properties from the text in the region.

Currently, the `invisible` and `intangible` properties are
*not* saved in the text/enriched format. The `read-only`
property is saved, but it is not a standard part of the text/enriched
format, so other editors may not respect it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC266) ] | [ [>](emacs.html#SEC268) ] | [ [<<](emacs.html#SEC235) ] | [ [Up](emacs.html#SEC258) ] | [ [>>](emacs.html#SEC268) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
