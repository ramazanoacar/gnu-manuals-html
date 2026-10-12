### 11.1 Special Characters: Inserting @ {} , \ # & [¶](#Special-Characters_003a-Inserting-_0040-_007b_007d-_002c-_005c-_0023-_0026)

‘`@`’ and curly braces are the basic special characters in
Texinfo. To insert these characters so they appear in text, you must
put an ‘`@`’ in front of these characters to prevent Texinfo from
misinterpreting them. Alphabetic commands are also provided.

The other characters (comma, backslash, hash, ampersand) are special
only in restricted contexts, as explained in the respective sections.

* [Inserting ‘@’ with `@@` and `@atchar{}`](#Inserting-an-Atsign)
* [Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}`](#Inserting-Braces)
* [Inserting ‘,’ with `@comma{}`](#Inserting-a-Comma)
* [Inserting ‘\’ with `@backslashchar{}`](#Inserting-a-Backslash)
* [Inserting ‘#’ with `@hashchar{}`](#Inserting-a-Hashsign)
* [Inserting ‘&’ with `@&` and `@ampchar{}`](#Inserting-an-Ampersand)

---

Next: [Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}`](#Inserting-Braces), Up: [Special Characters: Inserting @ {} , \ # &](#Special-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.1.1 Inserting ‘@’ with `@@` and `@atchar{}` [¶](#Inserting-_0060_0040_0027-with-_0040_0040-and-_0040atchar_007b_007d)

`@@` produces a single ‘`@`’ character in the output. Do
not put braces after an `@@` command.

`@atchar{}` also produces a single ‘`@`’ character in the
output. It does need following braces, as usual for alphabetic
commands. In inline conditionals (see [Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals)), it
can be necessary to avoid using the literal ‘`@`’ character in the
source (and may be clearer in other contexts).

---

Next: [Inserting ‘,’ with `@comma{}`](#Inserting-a-Comma), Previous: [Inserting ‘@’ with `@@` and `@atchar{}`](#Inserting-an-Atsign), Up: [Special Characters: Inserting @ {} , \ # &](#Special-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.1.2 Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}` [¶](#Inserting-_0060_007b-_0060_007d_0027-with-_0040_007b-_0040_007d-and-_0040l-rbracechar_007b_007d)

`@{` produces a single ‘`{`’ in the output, and `@}`
produces a single ‘`}`’. Do not put braces after either an
`@{` or an `@}` command.

`@lbracechar{}` and `@rbracechar{}` also produce
single ‘`{`’ and ‘`}`’ characters in the output. They do need
following braces, as usual for alphabetic commands. In inline
conditionals (see [Inline Conditionals: `@inline`, `@inlineifelse`, `@inlineraw`](#Inline-Conditionals)), it can be
necessary to avoid using literal brace characters in the source (and
may be clearer in other contexts).

---

Next: [Inserting ‘\’ with `@backslashchar{}`](#Inserting-a-Backslash), Previous: [Inserting ‘{ ‘}’ with `@{ @}` and `@l rbracechar{}`](#Inserting-Braces), Up: [Special Characters: Inserting @ {} , \ # &](#Special-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.1.3 Inserting ‘,’ with `@comma{}` [¶](#Inserting-_0060_002c_0027-with-_0040comma_007b_007d)

Ordinarily, a comma ‘,’ is a normal character that can be simply typed
in your input where you need it.

However, Texinfo uses the comma as a special character only in one
context: to separate arguments to those Texinfo commands, such as
`@node` (see [`@node` Line Requirements](#Node-Line-Requirements)),
`@acronym` (see [`@acronym`{acronym[, meaning]}](#g_t_0040acronym)) and `@xref`
(see [Cross-references](#Cross-References)), as well as user-defined macros
(see [Defining Macros](#Defining-Macros)), which take more than one argument.

Since a comma character would confuse Texinfo’s parsing for these
commands, you must use the command ‘`@comma{}`’ instead if you want
to pass an actual comma. Here are some examples:

```
@acronym{ABC, A Bizarre @comma{}}
@xref{Comma,, The @comma{} symbol}
@mymac{One argument@comma{} containing a comma}
```

Although ‘`@comma{}`’ can be used nearly anywhere, there is no
need for it anywhere except in this unusual case.

(Incidentally, the name ‘`@comma`’ lacks the ‘`char`’ suffix used
in its companion commands only for historical reasons. It didn’t seem
important enough to define a synonym.)

---

Next: [Inserting ‘#’ with `@hashchar{}`](#Inserting-a-Hashsign), Previous: [Inserting ‘,’ with `@comma{}`](#Inserting-a-Comma), Up: [Special Characters: Inserting @ {} , \ # &](#Special-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.1.4 Inserting ‘\’ with `@backslashchar{}` [¶](#Inserting-_0060_005c_0027-with-_0040backslashchar_007b_007d)

Ordinarily, a backslash ‘\’ is a normal character in Texinfo that can
be simply typed in your input where you need it. The result is to
typeset the backslash from the typewriter font.

However, Texinfo uses the backslash as a special character in one
restricted context: to delimit formal arguments in the bodies of
user-defined macros (see [Defining Macros](#Defining-Macros)).

Due to the vagaries of macro argument parsing, it is more reliable to
pass an alphabetic command that produces a backslash instead of using
a literal \. Hence `@backslashchar{}`. Here is an example
macro call:

```
@mymac{One argument@backslashchar{} with a backslash}
```

Texinfo documents may also use \ as a command character inside
`@math` (see [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math)). In this case, `@\` or
`\backslash` produce a “math” backslash (from the math symbol
font), while `@backslashchar{}` produces a typewriter
backslash as usual.

Although ‘`@backslashchar{}`’ can be used nearly anywhere, there
is no need for it except in these unusual cases.

---

Next: [Inserting ‘&’ with `@&` and `@ampchar{}`](#Inserting-an-Ampersand), Previous: [Inserting ‘\’ with `@backslashchar{}`](#Inserting-a-Backslash), Up: [Special Characters: Inserting @ {} , \ # &](#Special-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.1.5 Inserting ‘#’ with `@hashchar{}` [¶](#Inserting-_0060_0023_0027-with-_0040hashchar_007b_007d)

Ordinarily, a hash ‘#’ is a normal character in Texinfo that can be
simply typed in your input where you need it. The result is to
typeset the hash character from the current font.

This character has many other names, varying by locale, such as
“number sign”, “pound”, and “octothorp”. It is also sometimes
called “sharp” or “sharp sign” since it vaguely resembles the
musical symbol by that name. In situations where Texinfo is used,
“hash” is the most common in our experience.

However, Texinfo uses the hash character as a special character in one
restricted context: to introduce the so-called `#line` directive
and variants (see [External Macro Processors: Line Directives](#External-Macro-Processors)).

So, in order to typeset an actual hash character in such a place (for
example, in a program that needs documentation about `#line`),
it’s necessary to use `@hashchar{}` or some other construct.
Here’s an example:

```
@hashchar{} 10 "example.c"
```

Although ‘`@hashchar{}`’ can be used nearly anywhere, there
is no need for it anywhere except this unusual case.

---

Previous: [Inserting ‘#’ with `@hashchar{}`](#Inserting-a-Hashsign), Up: [Special Characters: Inserting @ {} , \ # &](#Special-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.1.6 Inserting ‘&’ with `@&` and `@ampchar{}` [¶](#Inserting-_0060_0026_0027-with-_0040_0026-and-_0040ampchar_007b_007d)

Ordinarily, an ampersand ‘&’ is a normal character in Texinfo that can be
simply typed in your input where you need it. The result is to
typeset the ampersand character.

However, the ampersand character is treated specially in just
one restricted context. In the argument to a definition command
(see [Definition Commands](#Definition-Commands)), an ampersand followed by a series
of letters may be typeset specially when processing with TeX
[7](#FOOT7) (see [Conventions for Writing Definitions](#Def-Cmd-Conventions)). For example:

```
@defun foo integer1 &optional integer2 &rest integers
@code{foo} described here.
@end defun
```

may have ‘`&optional`’ and ‘`&rest`’ formatted in bold,
even without any @-command involved.

So, in order to typeset an ampersand in such a context,
it’s necessary to use `@&` or some other construct.
Here’s an example of a C++ function taking a reference as a parameter:

```
@deftypefn Function int foo (const std::vector<int>@& @var{bar})
Documentation of @code{foo}.
@end deftypefn
```

This gives the output

Function: `int` **foo** `(const std::vector<int>& bar)` [¶](#index-foo)
:   Documentation of `foo`.

Although ‘`@&`’ and ‘`@ampchar{}`’ can be used nearly
anywhere, there is no need for them anywhere except this unusual case.

---

Next: [Inserting Space](#Inserting-Space), Previous: [Special Characters: Inserting @ {} , \ # &](#Special-Characters), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.2 Inserting Quote Characters [¶](#Inserting-Quote-Characters-1)

As explained in the early section on general Texinfo input conventions
(see [General Syntactic Conventions](#Conventions)), Texinfo source files use the ASCII character
`` ` `` (96 decimal) to produce a left quote (‘), and ASCII `'`
(39 decimal) to produce a right quote (’). Doubling these input
characters (``` `` ``` and `''`) produces double quotes (“ and
”). These are the conventions used by TeX.

In examples of computer code, however, `` ` `` and `'` produce
typical renderings for these ASCII characters: the backtick character
(standalone grave accent) and undirected single quote respectively.
In the past, directed glyphs
were used by default in TeX output. Texinfo provides these commands
to choose between these alternate renderings:

`@codequoteundirected on-off` [¶](#index-codequoteundirected)
:   Set to ‘`off`’ to output the `'` character in code environments
    as the right curly single quote.

`@codequotebacktick on-off`
:   Set to ‘`off`’ to output the `` ` `` character in code environments
    as the left curly single quote.

If you want these settings for only part of the document,
`@codequote... on` will restore the normal behavior, as in
`@codequoteundirected on`.

These settings affect `@code`, `@example`, `@kbd`,
`@samp`, `@verb`, and `@verbatim`. See [Highlighting Commands are Useful](#Useful-Highlighting).

Unfortunately, some document viewers will mangle the directed
quote characters when copying and pasting. (The free PDF reader
`xpdf` works fine, but other PDF readers, both free and
nonfree, have problems.)

This feature can also be controlled by using `@set`
and `@clear` on the corresponding variables
`txicodequoteundirected` and `txicodequotebacktick`.

---

Next: [Inserting Accents](#Inserting-Accents), Previous: [Inserting Quote Characters](#Inserting-Quote-Characters), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.3 Inserting Space [¶](#Inserting-Space-1)

The following sections describe commands that control spacing of various
kinds within and after sentences.

* [Multiple Spaces](#Multiple-Spaces)
* [Not Ending a Sentence](#Not-Ending-a-Sentence)
* [Ending a Sentence](#Ending-a-Sentence)
* [`@frenchspacing` val: Control Sentence Spacing](#g_t_0040frenchspacing)
* [`@dmn`{dimension}: Format a Dimension](#g_t_0040dmn)

---

Next: [Not Ending a Sentence](#Not-Ending-a-Sentence), Up: [Inserting Space](#Inserting-Space)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.3.1 Multiple Spaces [¶](#Multiple-Spaces-1)

Ordinarily, multiple whitespace characters (space, tab, and newline)
are collapsed into a single space.

Occasionally, you may want to produce several consecutive spaces,
either for purposes of example (e.g., what your program does with
multiple spaces as input), or merely for purposes of appearance in
headings or lists. Texinfo supports three commands:
`@SPACE`, `@TAB`, and `@NL`, all
of which insert a single space into the output. (Here,
`@SPACE` represents an ‘`@`’ character followed by a
space, i.e., ‘`@` ’, `TAB` represents an actual tab character,
and `@NL` represents an ‘`@`’ character and end-of-line,
i.e., when ‘`@`’ is the last character on a line.)

For example,

```
Spacey@ @ @ @
example.
```

produces

```
Spacey    example.
```

Do not follow any of these commands with braces.

To produce a non-breakable space, see [`@tie{}`: Inserting an Unbreakable Space](#g_t_0040tie).

---

Next: [Ending a Sentence](#Ending-a-Sentence), Previous: [Multiple Spaces](#Multiple-Spaces), Up: [Inserting Space](#Inserting-Space)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.3.2 Not Ending a Sentence [¶](#Not-Ending-a-Sentence-1)

When a period, exclamation point or question mark is
at the end of a sentence, slightly more space is
inserted after it in a typeset manual.

Usually, Texinfo can determine automatically when a period ends a
sentence. However, special commands are needed in some circumstances.
Use the `@:` command after a period, question mark, exclamation
mark or colon that should not be followed by extra space. This is
necessary in the following situations:

1. After a period that ends a lowercase abbreviation which is not at
   the end of a sentence.
2. When a parenthetical remark in the middle of a sentence (like
   this one!) ends with a period, exclamation point or question mark,
   `@:` should be used after the right parenthesis. Similarly for
   right brackets and right quotes (both single and double).

For example:

```
‘foo vs.@: bar (or?)@: baz’,
```

The first line below shows the output, and for comparison, the second
line shows the spacing when the ‘`@:`’ commands were not used.

> foo vs. bar (or?) baz  
> foo vs. bar (or?) baz

It may help you to remember what `@:` does by imagining that it
stands for an invisible lower-case character that stops a word ending in
a period.

A few Texinfo commands force normal interword spacing, so that you
don’t have to insert `@:` where you otherwise would. These are
the code-like highlighting commands, `@var`, `@abbr`, and
`@acronym` (see [Highlighting Commands are Useful](#Useful-Highlighting)). For example, in
‘`@code{foo. bar}`’ the period is not considered to be the end of a
sentence, and no extra space is inserted.

`@:` has no effect on the HTML or DocBook output.

---

Next: [`@frenchspacing` val: Control Sentence Spacing](#g_t_0040frenchspacing), Previous: [Not Ending a Sentence](#Not-Ending-a-Sentence), Up: [Inserting Space](#Inserting-Space)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.3.3 Ending a Sentence [¶](#Ending-a-Sentence-1)

As mentioned above, Texinfo normally inserts additional space after
the end of a sentence. It uses the same heuristic for this as TeX:
a sentence ends with a period, exclamation point, or question mark,
either preceded or followed by optional closing punctuation, and then
whitespace, and *not* preceded by a capital letter.

Use `@.` instead of a period, `@!` instead of an
exclamation point, and `@?` instead of a question mark at the
end of a sentence that does end with a capital letter. Do not put
braces after any of these commands. For example:

```
Give it to M.I.B. and to M.E.W@.  Also, give it to R.J.C@.
Give it to M.I.B. and to M.E.W.  Also, give it to R.J.C.
```

The output follows. In printed output and Info, you can see the
desired extra whitespace after the ‘`W`’ in the first line.

> Give it to M.I.B. and to M.E.W. Also, give it to R.J.C.  
> Give it to M.I.B. and to M.E.W. Also, give it to R.J.C.

In other output formats, `@.` is equivalent to a simple ‘`.`’;
likewise for `@!` and `@?`.

The “closing punctuation” mentioned above is defined as a right
parenthesis (‘`)`’, right bracket (‘`]`’), or right quote,
either single or double (‘`'`’ and ‘`''`’; the many possible
additional Unicode right quotes are not included). These characters
can be thought of as invisible with respect to whether a given period
ends a sentence. (This is the same rule as TeX.) For instance,
the periods in ‘`foo.) Bar`’ and ‘`foo.'' Bar`’ do end
sentences.

The meanings of `@:` and `@.`, etc. in Texinfo are
designed to work well with the Emacs sentence motion commands
(see [Sentences](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Sentences) in The GNU Emacs Manual). It may help to
imagine that the ‘`@`’ in ‘`@.`’, etc., is an invisible
lower-case letter ‘a’ which makes an upper-case letter before it
immaterial for the purposes of deciding whether the period ends the
sentence.

A few Texinfo commands are not considered as being an abbreviation,
even though they may end with a capital letter when expanded, so that
you don’t have to insert `@.` and companions. Notably, this is
the case for code-like highlighting commands, `@var` arguments
ending with a capital letter, `@LaTeX`, and `@TeX`. For
example, that sentence ended with ‘`... @code{@@TeX}.`’;
`@.` was not needed. Similarly, in
`... @var{VARNAME}. Text` the period after VARNAME ends
the sentence; there is no need to use `@.`.

---

Next: [`@dmn`{dimension}: Format a Dimension](#g_t_0040dmn), Previous: [Ending a Sentence](#Ending-a-Sentence), Up: [Inserting Space](#Inserting-Space)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.3.4 `@frenchspacing` val: Control Sentence Spacing [¶](#g_t_0040frenchspacing-val_003a-Control-Sentence-Spacing)

In American typography, it is traditional and correct to put extra
space at the end of a sentence. This is the default in Texinfo
(implemented in Info and printed output, not in other output formats). In
French typography (and others), this extra space is wrong; all spaces are
uniform.

Therefore Texinfo provides the `@frenchspacing` command to
control the spacing after punctuation. It reads the rest of the line
as its argument, which must be the single word ‘`on`’ or ‘`off`’
(always these words, regardless of the language of the document).
Here is an example:

```
@frenchspacing on
This is text. Two sentences. Three sentences. French spacing.

@frenchspacing off
This is text. Two sentences. Three sentences. Non-French spacing.
```

produces:

This is text. Two sentences. Three sentences. French spacing.

This is text. Two sentences. Three sentences. Non-French spacing.

`@frenchspacing` also affects the output after `@.`,
`@!`, and `@?` (see [Ending a Sentence](#Ending-a-Sentence)).

`@frenchspacing` has no effect on the HTML or DocBook output.

---

Previous: [`@frenchspacing` val: Control Sentence Spacing](#g_t_0040frenchspacing), Up: [Inserting Space](#Inserting-Space)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.3.5 `@dmn`{dimension}: Format a Dimension [¶](#g_t_0040dmn_007bdimension_007d_003a-Format-a-Dimension)

You can use the `@dmn` command to format a dimension with just enough
space for proper typesetting inserted in printed output. In other output
formats, the formatting commands insert no space at all.

To use the `@dmn` command, write the number and then follow it
immediately, with no intervening space, by `@dmn`, and then by
the dimension within braces. For example,

```
A4 paper is 8.27@dmn{in} wide.
```

produces

> A4 paper is 8.27in wide.

Not everyone uses this style. Some people prefer ‘8.27 in.’ or
‘8.27 inches’. In these cases, however, you need to use
`@tie` (see [`@tie{}`: Inserting an Unbreakable Space](#g_t_0040tie)) or `@w` (see [`@w`{text}: Prevent Line Breaks](#g_t_0040w))
so that no line break can occur between the number and the dimension.
Also, if you write a period after an abbreviation within a sentence
(as with the ‘in.’ above), you should write ‘`@:`’ after the
period to avoid inserting extra whitespace in printed output, as shown
here. See [Not Ending a Sentence](#Not-Ending-a-Sentence).

---

Next: [Inserting Quotation Marks](#Inserting-Quotation-Marks), Previous: [Inserting Space](#Inserting-Space), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.4 Inserting Accents [¶](#Inserting-Accents-1)

Here is a table with the commands Texinfo provides for inserting
floating accents. They all need an argument, the character to accent,
which can either be given in braces as usual (`@'{e}`), or, as
a special case, the braces can be omitted, in which case the argument
is the next character (`@'e`). This is to make the source as
convenient as possible to type and read, since accented characters are
very common in some languages.

If the command is alphabetic, such as `@dotaccent`, then there
must be a space between the command name and argument if braces are
not used. If the command is non-alphabetic, such as `@'`, then
there must *not* be a space; the argument is the very next
character.

Exception: the argument to `@tieaccent` must be enclosed in
braces (since it is two characters instead of one).

In Info and plain text output, accent constructs are output as the true
accented characters if the document encoding supports the required characters,
unless the option `--disable-encoding` is given to `texi2any`
(see [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding)). ASCII transliterations are used if the
encoded characters are not output.

| Command | Output | What |
| --- | --- | --- |
| `@"o` | ö | umlaut accent |
| `@'o` | ó | acute accent |
| `@,{c}` | ç | cedilla accent |
| `@=o` | ō | macron/overbar accent |
| `@^o` | ô | circumflex accent |
| `` @`o `` | ò | grave accent |
| `@~o` | õ | tilde accent |
| `@dotaccent{o}` | ȯ | overdot accent |
| `@H{o}` | ő | long Hungarian umlaut |
| `@ogonek{a}` | ą | ogonek |
| `@ringaccent{o}` | o̊ | ring accent |
| `@tieaccent{oo}` | o͡o | tie-after accent |
| `@u{o}` | ŏ | breve accent |
| `@ubaraccent{o}` | o̲ | underbar accent |
| `@udotaccent{o}` | ọ | underdot accent |
| `@v{o}` | ǒ | caron/hacek/check accent |

This table lists the Texinfo commands for inserting other characters
commonly used in languages other than English.

|  |  |  |
| --- | --- | --- |
| `@exclamdown{}` | ¡ | upside-down ! |
| `@questiondown{}` | ¿ | upside-down ? |
| `@aa{} @AA{}` | å Å | a,A with circle |
| `@ae{} @AE{}` | æ Æ | ae,AE ligatures |
| `@dh{} @DH{}` | ð Ð | Icelandic eth |
| `@dotless{i}` | ı | dotless i |
| `@dotless{j}` | ȷ | dotless j |
| `@l{} @L{}` | ł Ł | suppressed-L,l |
| `@o{} @O{}` | ø Ø | O,o with slash |
| `@oe{} @OE{}` | œ Œ | oe,OE ligatures |
| `@ordf{} @ordm{}` | ª º | Spanish ordinals |
| `@ss{}` | ß | es-zet or sharp S |
| `@th{} @TH{}` | þ Þ | Icelandic thorn |

---

Next: [`@sub` and `@sup`: Inserting Subscripts and Superscripts](#Inserting-Subscripts-and-Superscripts), Previous: [Inserting Accents](#Inserting-Accents), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.5 Inserting Quotation Marks [¶](#Inserting-Quotation-Marks-1)

Use doubled single-quote characters to begin and end quotations:
``` ``…'' ```. Two single quotes
are converted to left- and right-hand doubled quotation marks,
“like this”.

You may occasionally need to produce two consecutive single quotes;
for example, in documenting a computer language such as Maxima where
`''` is a valid command. You can do this with the input
`'@w{}'`; the empty `@w` command stops the combination into
the double-quote characters.

The left quote character (`` ` ``, ASCII code 96) used in Texinfo is a
grave accent in ANSI and ISO character set standards. We use it as a
quote character because that is how TeX is set up, by default.

Texinfo supports several other quotation marks used in languages other
than English. Below is a table with the commands Texinfo provides for
inserting quotation marks.

| Command | Glyph | Unicode name (point) |
| --- | --- | --- |
| ``` @quotedblleft{} `` ``` | “ | Left double quotation mark (U+201C) |
| `@quotedblright{} ''` | ” | Right double quotation mark (U+201D) |
| `` @quoteleft{} ` `` | ‘ | Left single quotation mark (U+2018) |
| `@quoteright{} '` | ’ | Right single quotation mark (U+2019) |
| `@quotedblbase{}` | „ | Double low-9 quotation mark (U+201E) |
| `@quotesinglbase{}` | ‚ | Single low-9 quotation mark (U+201A) |
| `@guillemetleft{}` | « | Left-pointing double angle quotation mark (U+00AB) |
| `@guillemetright{}` | » | Right-pointing double angle quotation mark (U+00BB) |
| `@guilsinglleft{}` | ‹ | Single left-pointing angle quotation mark (U+2039) |
| `@guilsinglright{}` | › | Single right-pointing angle quotation mark (U+203A) |

For the double angle quotation marks, Adobe and LaTeX glyph names
are also supported: `@guillemotleft` and
`@guillemotright`. These names are incorrect; a
“guillemot” is a bird species (a type of auk).

The standard TeX fonts support the usual quotation marks used in
English (the ones produced with single and doubled ASCII
single quotes). For the other quotation marks, TeX uses European
Computer Modern (EC) fonts (`ecrm1000` and other variants).
These fonts are freely available, of course; you can download them
from <http://ctan.org/pkg/ec>, among other places.

The free EC fonts are bitmap fonts created with Metafont. Especially
for on-line viewing, Type 1 (vector) versions of the fonts are
preferable; these are available in the CM-Super font package
(<http://ctan.org/pkg/cm-super>).

Both distributions include installation instructions.

Traditions for quotation mark usage vary to a great extent between
languages (<https://en.wikipedia.org/wiki/Quotation_mark>).
Texinfo does not provide commands or configurations for typesetting
quotation marks according to the numerous traditions. Therefore, you
have to choose the commands appropriate for the language of your
manual. Sometimes aliases (see [‘`@alias new=existing`’](#g_t_0040alias)) can simplify the
usage and make the source code more readable. For example, in German,
`@quotedblbase` is used for the left double quote, and the right
double quote is the glyph produced by `@quotedblleft`, which is
counterintuitive. Thus, in this case the following aliases would be
convenient:

```
@alias lgqq = quotedblbase
@alias rgqq = quotedblleft
```

---

Next: [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math), Previous: [Inserting Quotation Marks](#Inserting-Quotation-Marks), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.6 `@sub` and `@sup`: Inserting Subscripts and Superscripts [¶](#g_t_0040sub-and-_0040sup_003a-Inserting-Subscripts-and-Superscripts)

You can insert subscripts and superscripts with the `@sub`
and `@sup` commands. For example:

```
here@sub{below}@sup{above}
```

produces:

```
herebelowabove
```

In Info and plain text, `@sub{text}` is currently output
as ‘`_{text}`’ and `@sup{text}` as
‘`^{text}`’, including the literal braces (to mark the
beginning and end of the “script” text to the reader).

When the output format (and display program) permit (printed output,
HTML), the superscript is set above the subscript when both commands are
given consecutively.

For subscripts and superscripts in mathematical expressions, it is
better to use TeX’s ‘`_`’ and ‘`^`’ characters. See the
next section.

---

Next: [Glyphs for Text](#Glyphs-for-Text), Previous: [`@sub` and `@sup`: Inserting Subscripts and Superscripts](#Inserting-Subscripts-and-Superscripts), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.7 `@math` and `@displaymath`: Formatting Mathematics [¶](#g_t_0040math-and-_0040displaymath_003a-Formatting-Mathematics)

You can write a mathematical expression, equation or formula using
the `@math` command. Write the TeX math notation between
braces, like this:

```
@math{\partial_\alpha \partial^\alpha A^\beta = \mu_0 J^\beta}
```

`@math` is formatted inline when used inside a paragraph,
like *\(\partial\_\alpha \partial^\alpha A^\beta = \mu\_0 J^\beta\)* in
this example.
The `@math` command has no special effect on the Info
output, merely outputting the contents verbatim.

For printed output, `@math` switches into TeX “math mode”.
In that context, ‘`\`’ must be used instead of ‘`@`’
for plain TeX math control sequences for symbols, functions,
and so on.

By default, the HTML output is only emphasized.
`texi2any` provides three options for displaying properly
formatted mathematics for HTML. You can select these with the
`HTML_MATH` variable (see [HTML Customization Variables](#HTML-Customization-Variables)).
With `HTML_MATH` set to ‘`l2h`’, `texi2any` attempts
to use the `latex2html` program to produce image files for
mathematical material. With the ‘`t4h`’ setting, `texi2any`
attempts to use the `tex4ht` program. With the ‘`mathjax`’
setting, `texi2any` inserts references in the output files
to MathJax scripts to format the material. The MathJax option
requires JavaScript to be enabled in the browser to work. See also
[MathJax Customization Variables](#MathJax-Customization-Variables), [`latex2html` Customization Variables](#latex2html-Customization-Variables) and [`tex4ht` Customization Variables](#tex4ht-Customization-Variables).

For displayed equations, you can use the `@displaymath`
command. Example:

```
@displaymath
f(x) = {1\over\sigma\sqrt{2\pi}}
e^{-{1\over2}\left({x-\mu\over\sigma}\right)^2}
@end displaymath
```

which produces:

*\[f(x) = {1\over\sigma\sqrt{2\pi}}
e^{-{1\over2}\left({x-\mu\over\sigma}\right)^2}
\]*

In general, the contents of `@math` or `@displaymath`
should be plain TeX only, with no interspersed Texinfo commands.
If you do use any Texinfo commands, then you should mark these with
‘`@`’ as usual, rather than ‘`\`’ (e.g. ‘`@var`’ rather than
‘`\var`’), but we do not guarantee which Texinfo commands will work.

Although `@sub` and `@sup` may work inside math mode in
some contexts, it is better to use TeX’s ‘`_`’ and ‘`^`’
characters to denote subscripts and superscripts within mathematical
expressions.

LaTeX-specific code will only work when the output format is LaTeX,
and with the `HTML_MATH` options (although `tex4ht` needs
`T4H_MATH_CONVERSION` to be set to ‘`latex`’;
see [`tex4ht` Customization Variables](#tex4ht-Customization-Variables)).

Due to the conflict with Texinfo’s `@sup` command, you can access
the plain TeX command `\sup` as `\mathopsup` instead,
in the unlikely occurrence that you want to do this (but only when
processing with TeX, not when outputting LaTeX nor with any of the
`HTML_MATH` options).

---

Next: [Glyphs for Programming](#Glyphs-for-Programming), Previous: [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.8 Glyphs for Text [¶](#Glyphs-for-Text-1)

Texinfo has support for a few additional glyphs that are commonly used
in printed text but not available in ASCII. Of course, there are
many thousands more. It is possible to use Unicode characters as-is
as far as `texi2any` is concerned, but TeX is not so lucky.

* [`@TeX`{} (TeX) and `@LaTeX`{} (LaTeX)](#g_t_0040TeX-_0040LaTeX)
* [`@copyright{}` (©)](#g_t_0040copyright)
* [`@registeredsymbol{}` (®)](#g_t_0040registeredsymbol)
* [`@dots` (…) and `@enddots` (...)](#g_t_0040dots)
* [`@bullet` (•)](#g_t_0040bullet)
* [`@euro` (€): Euro Currency Symbol](#g_t_0040euro)
* [`@pounds` (£): Pounds Sterling](#g_t_0040pounds)
* [`@textdegree` (°): Degrees Symbol](#g_t_0040textdegree)
* [`@minus` (−): Inserting a Minus Sign](#g_t_0040minus)
* [`@geq` (≥) and `@leq` (≤): Inserting Relations](#g_t_0040geq-_0040leq)

---

Next: [`@copyright{}` (©)](#g_t_0040copyright), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.1 `@TeX`{} (TeX) and `@LaTeX`{} (LaTeX) [¶](#g_t_0040TeX_007b_007d-_0028TeX_0029-and-_0040LaTeX_007b_007d-_0028LaTeX_0029)

Use the `@TeX{}` command to generate ‘TeX’. In a printed
manual, this is a special logo that is different from three ordinary
letters. In other output formats, it just looks like ‘`TeX`’.

Similarly, use the `@LaTeX{}` command to generate ‘LaTeX’,
which is even more special in printed manuals (and different from the
incorrect `La@TeX{}`. In other output formats, the result is just
‘`LaTeX`’.

The spelling of these commands is unusual for Texinfo, in that they
use both uppercase and lowercase letters.

---

Next: [`@registeredsymbol{}` (®)](#g_t_0040registeredsymbol), Previous: [`@TeX`{} (TeX) and `@LaTeX`{} (LaTeX)](#g_t_0040TeX-_0040LaTeX), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.2 `@copyright{}` (©) [¶](#g_t_0040copyright_007b_007d-_0028_00a9_0029)

Use the `@copyright{}` command to generate the copyright
symbol, ‘©’. Where possible, this is a ‘`c`’ inside a
circle; otherwise this is ‘`(C)`’.

Legally, it’s not necessary to use the copyright symbol; the English
word ‘Copyright’ suffices, according to international treaty.

---

Next: [`@dots` (…) and `@enddots` (...)](#g_t_0040dots), Previous: [`@copyright{}` (©)](#g_t_0040copyright), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.3 `@registeredsymbol{}` (®) [¶](#g_t_0040registeredsymbol_007b_007d-_0028_00ae_0029)

Use the `@registeredsymbol{}` command to generate the
registered symbol, ‘®’. Where possible, this is an
‘`R`’ inside a circle; otherwise this is ‘`(R)`’.

---

Next: [`@bullet` (•)](#g_t_0040bullet), Previous: [`@registeredsymbol{}` (®)](#g_t_0040registeredsymbol), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.4 `@dots` (…) and `@enddots` (...) [¶](#g_t_0040dots-_0028_2026_0029-and-_0040enddots-_0028_002e_002e_002e_0029)

An *ellipsis* (a sequence of dots) would be spaced wrong when
typeset as a string of periods, so a special command is used in
Texinfo: use the `@dots{}` command to generate a normal
ellipsis, which is three dots in a row, appropriately spaced …
like so. To emphasize: do not simply write three periods in the input
file; that could work in some output formats, but would produce the
wrong amount of space between the periods in printed manuals.

The `@enddots{}` command generates an end-of-sentence
ellipsis, which also has three dots, but with different spacing
afterwards, ... Look closely to see the difference.

Here is an ellipsis: …
Here are three periods in a row: ...

In printed (and usually HTML) output, the three periods in a row are
much closer together than the dots in the ellipsis.

---

Next: [`@euro` (€): Euro Currency Symbol](#g_t_0040euro), Previous: [`@dots` (…) and `@enddots` (...)](#g_t_0040dots), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.5 `@bullet` (•) [¶](#g_t_0040bullet-_0028_2022_0029)

Use the `@bullet{}` command to generate a large round dot, or
the closest possible thing to one. An asterisk can also be used.
Here is a bullet: •

When you use `@bullet` in `@itemize`, you do not need to
type the braces, because `@itemize` supplies them.
(see [`@itemize`: Making an Itemized List](#g_t_0040itemize)).

---

Next: [`@pounds` (£): Pounds Sterling](#g_t_0040pounds), Previous: [`@bullet` (•)](#g_t_0040bullet), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.6 `@euro` (€): Euro Currency Symbol [¶](#g_t_0040euro-_0028_20ac_0029_003a-Euro-Currency-Symbol)

Use the `@euro{}` command to generate ‘€’. Where
possible, this is the symbol for the Euro currency. Otherwise, the
word ‘`Euro`’ is used.

The Euro symbol does not exist in the standard TeX fonts (which
were designed before the Euro was legislated into existence).
Therefore, TeX uses an additional font, named `feymr10` (along
with other variables). It is freely available, of course; you can
download it from <http://ctan.org/pkg/eurosym>, among other
places. The distribution includes installation instructions.

---

Next: [`@textdegree` (°): Degrees Symbol](#g_t_0040textdegree), Previous: [`@euro` (€): Euro Currency Symbol](#g_t_0040euro), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.7 `@pounds` (£): Pounds Sterling [¶](#g_t_0040pounds-_0028_00a3_0029_003a-Pounds-Sterling)

Use the `@pounds{}` command to generate ‘£’. Where
possible, this is the symbol for the pounds sterling British currency.
Otherwise, it is ‘`#`’.

---

Next: [`@minus` (−): Inserting a Minus Sign](#g_t_0040minus), Previous: [`@pounds` (£): Pounds Sterling](#g_t_0040pounds), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.8 `@textdegree` (°): Degrees Symbol [¶](#g_t_0040textdegree-_0028_00b0_0029_003a-Degrees-Symbol)

Use the `@textdegree{}` command to generate ‘°’.
Where possible, this is the normal symbol for degrees. Otherwise,
it is an ‘`o`’.

---

Next: [`@geq` (≥) and `@leq` (≤): Inserting Relations](#g_t_0040geq-_0040leq), Previous: [`@textdegree` (°): Degrees Symbol](#g_t_0040textdegree), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.9 `@minus` (−): Inserting a Minus Sign [¶](#g_t_0040minus-_0028_2212_0029_003a-Inserting-a-Minus-Sign)

Use the `@minus{}` command to generate a minus sign.
Where the character encoding and font used in the output allow it,
the symbol is the customary length for a minus sign—a little longer
than a hyphen, shorter than an em-dash:

```
‘−’ is a minus sign generated with ‘@minus{}’,

‘-’ is a hyphen generated with the character ‘-’,

‘—’ is an em-dash for text.
```

If you actually want to typeset some math that does a subtraction, it
is better to use `@math`, as in `@math{a-b}` (see [`@math` and `@displaymath`: Formatting Mathematics](#Inserting-Math)).

---

Previous: [`@minus` (−): Inserting a Minus Sign](#g_t_0040minus), Up: [Glyphs for Text](#Glyphs-for-Text)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.8.10 `@geq` (≥) and `@leq` (≤): Inserting Relations [¶](#g_t_0040geq-_0028_2265_0029-and-_0040leq-_0028_2264_0029_003a-Inserting-Relations)

Use the `@geq{}` and `@leq{}` commands to generate
greater-than-or-equal and less-than-equal-signs, ‘≥’ and
‘≤’. When those symbols are not available, the ASCII sequences
‘`>=`’ and ‘`<=`’ are output.

---

Next: [Inserting Unicode: `@U`](#Inserting-Unicode), Previous: [Glyphs for Text](#Glyphs-for-Text), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.9 Glyphs for Programming [¶](#Glyphs-for-Programming-1)

In Texinfo, code is often illustrated in examples that are delimited
by `@example` and `@end example`. In such examples,
you can indicate the results of evaluation or an expansion using
‘`⇒`’ or ‘`→`’. Likewise, there are commands
to insert glyphs to indicate printed output, error messages, equivalence
of expressions, the location of point in an editor, and GUI operation
sequences.

The glyph-insertion commands do not need to be used within an example,
but most often they are. All glyph-insertion commands are followed by
empty braces.

* [Glyphs Summary](#Glyphs-Summary)
* [`@result{}` (⇒): Result of an Expression](#g_t_0040result)
* [`@expansion{}` (→): Indicating an Expansion](#g_t_0040expansion)
* [`@print{}` (-|): Indicating Generated Output](#g_t_0040print)
* [`@error{}` (error→): Indicating an Error Message](#g_t_0040error)
* [`@equiv{}` (≡): Indicating Equivalence](#g_t_0040equiv)
* [`@point{}` (∗): Indicating Point in a Buffer](#g_t_0040point)
* [Click Sequences](#Click-Sequences)

---

Next: [`@result{}` (⇒): Result of an Expression](#g_t_0040result), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.1 Glyphs Summary [¶](#Glyphs-Summary-1)

Here is a summary of the glyph commands:

⇒
:   `@result{}` indicates the result of an expression.

→
:   `@expansion{}` indicates the results of a macro expansion.

-|
:   `@print{}` indicates printed output.

error→
:   `@error{}` indicates the following text is an error message.

≡
:   `@equiv{}` indicates the exact equivalence of two forms.

∗
:   `@point{}` shows the location of point.

A → B
:   `@clicksequence{A @click{} B}` indicates a GUI operation
    sequence: first A, then clicking B, or choosing B from a menu, or
    otherwise selecting it.

---

Next: [`@expansion{}` (→): Indicating an Expansion](#g_t_0040expansion), Previous: [Glyphs Summary](#Glyphs-Summary), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.2 `@result{}` (⇒): Result of an Expression [¶](#g_t_0040result_007b_007d-_0028_21d2_0029_003a-Result-of-an-Expression)

Use the `@result{}` command to indicate the result of
evaluating an expression.

The `@result{}` command is displayed as ‘`⇒`’,
either a double stemmed arrow or (when that is not available) the
ASCII sequence ‘`=>`’.

Thus, the following,

```
(cdr '(1 2 3))
    ⇒ (2 3)
```

may be read as “`(cdr '(1 2 3))` evaluates to `(2 3)`”.

---

Next: [`@print{}` (-|): Indicating Generated Output](#g_t_0040print), Previous: [`@result{}` (⇒): Result of an Expression](#g_t_0040result), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.3 `@expansion{}` (→): Indicating an Expansion [¶](#g_t_0040expansion_007b_007d-_0028_21a6_0029_003a-Indicating-an-Expansion)

When an expression is a macro call, it expands into a new expression.
You can indicate the result of the expansion with the
`@expansion{}` command.

The `@expansion{}` command is displayed as
‘`→`’, either a long arrow with a flat base or (when
that is not available) the ASCII sequence ‘`==>`’.

For example, the following

```
@example lisp
(third '(a b c))
    @expansion{} (car (cdr (cdr '(a b c))))
    @result{} c
@end example
```

produces

```
(third '(a b c))
    → (car (cdr (cdr '(a b c))))
    ⇒ c
```

which may be read as:

> `(third '(a b c))` expands to `(car (cdr (cdr '(a b c))))`;
> the result of evaluating the expression is `c`.

Often, as in this case, an example looks better if the
`@expansion{}` and `@result{}` commands are indented.

---

Next: [`@error{}` (error→): Indicating an Error Message](#g_t_0040error), Previous: [`@expansion{}` (→): Indicating an Expansion](#g_t_0040expansion), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.4 `@print{}` (-|): Indicating Generated Output [¶](#g_t_0040print_007b_007d-_0028_22a3_0029_003a-Indicating-Generated-Output)

Sometimes an expression will generate output during its execution.
You can indicate such displayed output with the `@print{}`
command.

The `@print{}` command is displayed as ‘`-|`’, either
a horizontal dash butting against a vertical bar or (when that is not
available) the ASCII sequence ‘`-|`’.

In the following example, the printed text is indicated with
‘`-|`’, and the value of the expression follows on the
last line.

```
(progn (print 'foo) (print 'bar))
    -| foo
    -| bar
    ⇒ bar
```

In a Texinfo source file, this example is written as follows:

```
@example lisp
(progn (print 'foo) (print 'bar))
    @print{} foo
    @print{} bar
    @result{} bar
@end example
```

---

Next: [`@equiv{}` (≡): Indicating Equivalence](#g_t_0040equiv), Previous: [`@print{}` (-|): Indicating Generated Output](#g_t_0040print), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.5 `@error{}` (error→): Indicating an Error Message [¶](#g_t_0040error_007b_007d-_0028error_002d_002d_003e_0029_003a-Indicating-an-Error-Message)

A piece of code may cause an error when you evaluate it. You can
designate the error message with the `@error{}` command.

The `@error{}` command is displayed as ‘`error→`’, either
the word ‘error’ in a box in the printed output, the word error
followed by an arrow in other formats or (when no arrow is available)
‘`error-->`’.

Thus,

```
@example lisp
(+ 23 'x)
@error{} Wrong type argument: integer-or-marker-p, x
@end example
```

produces

```
(+ 23 'x)
error→ Wrong type argument: integer-or-marker-p, x
```

This indicates that the following error message is printed
when you evaluate the expression:

```
Wrong type argument: integer-or-marker-p, x
```

The word ‘`error→`’ itself is not part of the error message.

---

Next: [`@point{}` (∗): Indicating Point in a Buffer](#g_t_0040point), Previous: [`@error{}` (error→): Indicating an Error Message](#g_t_0040error), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.6 `@equiv{}` (≡): Indicating Equivalence [¶](#g_t_0040equiv_007b_007d-_0028_2261_0029_003a-Indicating-Equivalence)

Sometimes two expressions produce identical results. You can indicate
the exact equivalence of two forms with the `@equiv{}`
command. The `@equiv{}` command is displayed as
‘`≡`’, either a standard mathematical equivalence sign
(three parallel horizontal lines) or (when that is not available) as
the ASCII sequence ‘`==`’.

Thus,

```
@example lisp
(make-sparse-keymap) @equiv{} (list 'keymap)
@end example
```

produces

```
(make-sparse-keymap) ≡ (list 'keymap)
```

This indicates that evaluating `(make-sparse-keymap)` produces
identical results to evaluating `(list 'keymap)`.

---

Next: [Click Sequences](#Click-Sequences), Previous: [`@equiv{}` (≡): Indicating Equivalence](#g_t_0040equiv), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.7 `@point{}` (∗): Indicating Point in a Buffer [¶](#g_t_0040point_007b_007d-_0028_22c6_0029_003a-Indicating-Point-in-a-Buffer)

Sometimes you need to show an example of text in an Emacs buffer. In
such examples, the convention is to include the entire contents of the
buffer in question between two lines of dashes containing the buffer
name.

You can use the ‘`@point{}`’ command to show the location of
point in the text in the buffer. (The symbol for point, of course, is
not part of the text in the buffer; it indicates the place
*between* two characters where point is located.)

The `@point{}` command is displayed as ‘`∗`’, either
a pointed star or (when that is not available) the ASCII sequence
‘`-!-`’.

The following example shows the contents of buffer `foo` before
and after evaluating a Lisp command to insert the word `changed`.

```
---------- Buffer: foo ----------
This is the ∗contents of foo.
---------- Buffer: foo ----------
```

```
(insert "changed ")
    ⇒ nil
---------- Buffer: foo ----------
This is the changed ∗contents of foo.
---------- Buffer: foo ----------
```

In a Texinfo source file, the example is written like this:

```
@example
---------- Buffer: foo ----------
This is the @point{}contents of foo.
---------- Buffer: foo ----------

(insert "changed ")
    @result{} nil
---------- Buffer: foo ----------
This is the changed @point{}contents of foo.
---------- Buffer: foo ----------
@end example
```

---

Previous: [`@point{}` (∗): Indicating Point in a Buffer](#g_t_0040point), Up: [Glyphs for Programming](#Glyphs-for-Programming)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

#### 11.9.8 Click Sequences [¶](#Click-Sequences-1)

When documenting graphical interfaces, it is necessary to describe
sequences such as ‘Click on ‘`File`’, then choose ‘`Open`’, then
…’. Texinfo offers commands `@clicksequence` and
`click` to represent this, typically used like this:

```
... @clicksequence{File @click{} Open} ...
```

which produces:

```
... File → Open ...
```

The `@click` command produces a right arrow by default; this
glyph is also available independently via the command
`@arrow{}`.

You can change the glyph produced by `@click` with the command
`@clickstyle`, which takes a command name as its single argument
on the rest of the line, much like `@itemize` and friends
(see [`@itemize`: Making an Itemized List](#g_t_0040itemize)). The command should produce a glyph, and
the usual empty braces ‘`{}`’ are omitted. Here’s an example:

```
@clickstyle @result
... @clicksequence{File @click{} Open} ...
```

now produces:

```
... File ⇒ Open ...
```

---

Previous: [Glyphs for Programming](#Glyphs-for-Programming), Up: [Special Insertions](#Insertions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 11.10 Inserting Unicode: `@U` [¶](#Inserting-Unicode_003a-_0040U)

The command `@U{hex}` inserts a representation of the
Unicode character U+hex. For example, `@U{0132}`
inserts the Dutch ‘IJ’ ligature (‘Ĳ’).

The hex value should be at least four hex digits; leading zeros
are *not* added. In general, hex must specify a valid
normal Unicode character; e.g., U+10FFFF (the very last code point) is
invalid by definition, and thus cannot be inserted this way.

`@U` is useful for inserting occasional glyphs for which Texinfo
has no dedicated command, while allowing the Texinfo source to remain
purely 7-bit ASCII for maximum portability.

This command has many limitations—the same limitations as inserting
Unicode characters in UTF-8 or another binary form. First and most
importantly, TeX knows nothing about most of Unicode. Supporting
specific additional glyphs upon request is possible, but it’s not
viable for `texinfo.tex` to support whole additional scripts
(Japanese, Urdu, …). The `@U` command does nothing to
change this. If the specified character is not supported in TeX,
an error is given. LaTeX output has more possibilities regarding
UTF-8, but could require extra code to load fonts and declare
how UTF-8 characters are output. (See [`@documentencoding enc`: Set Input Encoding](#g_t_0040documentencoding).)

In HTML and DocBook, the output from `@U` is always an
entity reference of the form ‘`&#xhex;`’, as in
‘`&#x0132;`’ for the example above. This should work even when an
HTML document uses some other encoding (say, Latin 1) and the
given character is not supported in that encoding.

In Info and plain text, if the output encoding is not UTF-8, the output
is the ASCII sequence ‘`U+hex`’, as in the six ASCII characters
‘`U+0132`’ for the example above.

---

Next: [Definition Commands](#Definition-Commands), Previous: [Special Insertions](#Insertions), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 12 Forcing and Preventing Breaks [¶](#Forcing-and-Preventing-Breaks)

Line and page breaks can sometimes occur in the ‘wrong’ place in one
or another form of output. It’s up to you to ensure that text looks
right in all the output formats.

For example, in a printed manual, page breaks may occur awkwardly in
the middle of an example; to prevent this, you can hold text together
using a grouping command that keeps the text from being split across
two pages. Conversely, you may want to force a page break where none
would normally occur.

You can use the break, break prevention, or pagination commands to fix
problematic line and page breaks.

* [Break Commands](#Break-Commands)
* [`@*` and `@/`: Generate and Allow Line Breaks](#Line-Breaks)
* [`@-` and `@hyphenation`: Hyphenation in Printed Output](#g_t_0040_002d-_0040hyphenation)
* [`@allowcodebreaks`: Control Line Breaks in `@code`](#g_t_0040allowcodebreaks)
* [`@w`{text}: Prevent Line Breaks](#g_t_0040w)
* [`@tie{}`: Inserting an Unbreakable Space](#g_t_0040tie)
* [`@sp` n: Insert Blank Lines](#g_t_0040sp)
* [`@page`: Start a New Page](#g_t_0040page)
* [`@group`: Prevent Page Breaks](#g_t_0040group)
* [`@need mils`: Prevent Page Breaks](#g_t_0040need)

---

Next: [`@*` and `@/`: Generate and Allow Line Breaks](#Line-Breaks), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.1 Break Commands [¶](#Break-Commands-1)

The break commands create or allow line and paragraph breaks:

`@*`
:   Force a line break.

`@sp n`
:   Skip n blank lines.

`@-`
:   Insert a discretionary hyphen.

`@hyphenation{hy-phen-a-ted words}`
:   Define hyphen points in hy-phen-a-ted words.

These commands hold text together on a single line:

`@w{text}`
:   Prevent text from being split and hyphenated across two lines.

`@tie{}`
:   Insert a normal interword space at which a line break may not occur.

The pagination commands apply only to printed output, since other
output formats do not have pages.

`@page`
:   Start a new page.

`@group`
:   Hold text together that must appear on one page.

`@need mils`
:   Start a new page if not enough space on this one.

---

Next: [`@-` and `@hyphenation`: Hyphenation in Printed Output](#g_t_0040_002d-_0040hyphenation), Previous: [Break Commands](#Break-Commands), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.2 `@*` and `@/`: Generate and Allow Line Breaks [¶](#g_t_0040_002a-and-_0040_002f_003a-Generate-and-Allow-Line-Breaks)

The `@*` command forces a line break in all output formats.
The `@/` command allows a line break (printed manual only).

Here is an example with `@*`:

```
This sentence is broken @*into two lines.
```

produces

```
This sentence is broken
into two lines.
```

The `@/` command can be useful within long URLs or other
identifiers where TeX can’t find a good place to break. TeX
will automatically break URLs at the natural places (see [URL Line Breaking](#URL-Line-Breaking)), so only use `@/` if you need it. `@/` has no
effect on the other output formats.

---

Next: [`@allowcodebreaks`: Control Line Breaks in `@code`](#g_t_0040allowcodebreaks), Previous: [`@*` and `@/`: Generate and Allow Line Breaks](#Line-Breaks), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.3 `@-` and `@hyphenation`: Hyphenation in Printed Output [¶](#g_t_0040_002d-and-_0040hyphenation_003a-Hyphenation-in-Printed-Output)

Although TeX’s hyphenation algorithm is generally pretty good, it
does miss useful hyphenation points from time to time. (Or, far more
rarely, insert an incorrect hyphenation.) So, for documents with an
unusual vocabulary or when fine-tuning for a printed edition, you may
wish to specify hyphenation points explicitly. Texinfo supports two
commands for this:

`@-`
:   Insert a discretionary hyphen, i.e., a place where a word can be
    broken across lines with a hyphen. This is especially useful when you
    notice that an overfull hbox is due to TeX missing a hyphenation
    (see [Overfull “hboxes”](#Overfull-hboxes)). TeX will not insert any hyphenation
    points itself into a word containing `@-`.

`@hyphenation{hy-phen-a-ted words}`
:   Give hyphenation points for certain words. For example:

    ```
    @hyphenation{man-u-script man-u-scripts}
    ```

    As shown, you put a ‘`-`’ at each hyphenation point.
    TeX only uses the specified hyphenation points when the words match
    exactly, so give all necessary variants, such as plurals.

Non-printed output is not hyphenated, so none of
these commands have any effect in other output formats.

---

Next: [`@w`{text}: Prevent Line Breaks](#g_t_0040w), Previous: [`@-` and `@hyphenation`: Hyphenation in Printed Output](#g_t_0040_002d-_0040hyphenation), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.4 `@allowcodebreaks`: Control Line Breaks in `@code` [¶](#g_t_0040allowcodebreaks_003a-Control-Line-Breaks-in-_0040code)

Ordinarily, TeX considers breaking lines at ‘`-`’ and ‘`_`’
characters within `@code` and related commands
(see [`@code`{sample-code}](#g_t_0040code)), more or less as if they were “empty”
hyphenation points.

This is necessary since many manuals, especially for Lisp-family
languages, must document very long identifiers. On the other hand,
some manuals don’t have this problem, and you may not wish to allow a
line break at the underscore in, for example, `SIZE_MAX`, or even
worse, after any of the four underscores in `__typeof__`.

So Texinfo provides this command:

```
@allowcodebreaks false
```

to prevent from breaking at ‘`-`’ or ‘`_`’ within
`@code`. You can go back to allowing such breaks with
`@allowcodebreaks true`. Write these commands on lines by
themselves.

These commands can be given anywhere in the document. For example,
you may have just one problematic paragraph where you need to turn off
the breaks, but want them in general, or vice versa.

This command has no effect except in TeX output.

---

Next: [`@tie{}`: Inserting an Unbreakable Space](#g_t_0040tie), Previous: [`@allowcodebreaks`: Control Line Breaks in `@code`](#g_t_0040allowcodebreaks), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.5 `@w`{text}: Prevent Line Breaks [¶](#g_t_0040w_007btext_007d_003a-Prevent-Line-Breaks)

`@w{text}` outputs text, while prohibiting line
breaks within text.

Thus, you can use `@w` to produce a non-breakable space, fixed at
the width of a normal interword space:

```
@w{ } @w{ } @w{ } indentation.
```

produces:

```
      indentation.
```

The space from `@w{ }`, as well as being non-breakable,
also will not stretch or shrink. Sometimes that is what you want, for
instance if you’re doing manual indenting. However, usually you want
a normal interword space that does stretch and shrink (in the printed
output); for that, see the `@tie` command in the next section.

In printed output, you can also use the `@w` command to prevent
a long name or phrase being automatically hyphenated, for example if
it happens to fall near the end of a line.

You can also use `@w` to avoid unwanted keyword expansion in
source control systems. For example, to literally write `$Id$`
in your document, use `@w{$}Id$`. This trick isn’t effective
for output files in some output formats, though.

---

Next: [`@sp` n: Insert Blank Lines](#g_t_0040sp), Previous: [`@w`{text}: Prevent Line Breaks](#g_t_0040w), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.6 `@tie{}`: Inserting an Unbreakable Space [¶](#g_t_0040tie_007b_007d_003a-Inserting-an-Unbreakable-Space)

The `@tie{}` command produces a normal interword space at which
a line break may not occur. Always write it with following (empty)
braces, as usual for commands used within a paragraph. Here’s an
example:

```
@TeX{} was written by Donald E.@tie{}Knuth.
```

produces:

```
TeX was written by Donald E. Knuth.
```

There are two important differences between `@tie{}` and
`@w{ }`:

* The space produced by `@tie{}` will stretch and shrink slightly
  along with the normal interword spaces in the paragraph; the space
  produced by `@w{ }` will not vary.
* `@tie{}` allows hyphenation of the surrounding words, while
  `@w{ }` inhibits hyphenation of those words (for TeXnical
  reasons, namely that it produces an ‘`\hbox`’).

---

Next: [`@page`: Start a New Page](#g_t_0040page), Previous: [`@tie{}`: Inserting an Unbreakable Space](#g_t_0040tie), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.7 `@sp` n: Insert Blank Lines [¶](#g_t_0040sp-n_003a-Insert-Blank-Lines)

A line beginning with and containing only `@sp n`
generates n blank lines of space. `@sp` also
forces a paragraph break. For example,

```
@sp 2
```

generates two blank lines.

The `@sp` command is most often used in the title page.

---

Next: [`@group`: Prevent Page Breaks](#g_t_0040group), Previous: [`@sp` n: Insert Blank Lines](#g_t_0040sp), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.8 `@page`: Start a New Page [¶](#g_t_0040page_003a-Start-a-New-Page)

A line containing only `@page` starts a new page in a printed
manual. In other formats, without the concept of pages, it starts a
new paragraph. A `@page` command is often used in the
`@titlepage` section of a Texinfo file to start the copyright
page.

---

Next: [`@need mils`: Prevent Page Breaks](#g_t_0040need), Previous: [`@page`: Start a New Page](#g_t_0040page), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.9 `@group`: Prevent Page Breaks [¶](#g_t_0040group_003a-Prevent-Page-Breaks)

The `@group` command is used to hold an example
together on one page. Use it inside an
`@example` or similar construct to begin an unsplittable vertical
group, which will appear entirely on one page in the printed output.
Terminate the group by a line containing only `@end group`.
This command has an effect in TeX output only.

The `@group` and `@end group` commands need to be inside the
`@example` and `@end example` commands, thus:

```
@example
@group
...
@end group
@end example
```

Although `@group` would make sense conceptually in a wide
variety of contexts, its current implementation works only
within `@example` and variants, and within `@display`,
`@format`, `@flushleft` and `@flushright`.
See [Quotations and Examples](#Quotations-and-Examples). (What all these commands have in
common is that each line of input produces a line of output.)

---

Previous: [`@group`: Prevent Page Breaks](#g_t_0040group), Up: [Forcing and Preventing Breaks](#Breaks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 12.10 `@need mils`: Prevent Page Breaks [¶](#g_t_0040need-mils_003a-Prevent-Page-Breaks)

A line containing only `@need n` starts a new page in a
printed manual if fewer than n mils (thousandths of an inch)
remain on the current page. Do not use braces around the argument
n. The `@need` command has no effect on other output
formats since they are not paginated.

This paragraph is preceded by a `@need` command that starts a
new page in printed output if fewer than 800 mils (eight-tenths inch)
remain on the page. It looks like this:

```
@need 800
This paragraph is preceded by ...
```

The `@need` command is useful for preventing orphans: single
lines at the bottoms of printed pages.

---

Next: [Internationalization](#Internationalization), Previous: [Forcing and Preventing Breaks](#Breaks), Up: [Texinfo](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]## 13 Definition Commands [¶](#Definition-Commands-1)

The `@deffn` command and the other *definition commands*
enable you to describe functions, variables, macros, commands, user
options, special forms and other entities in a uniform format.

In the output, the name of the entity is shown with any arguments, along
with the entity category —‘Function’, ‘Variable’, or whatever.
Underneath, the body of the definition is indented.
The name of the entity is also entered into the appropriate index.

All the definition commands follow a similar format. This chapter
starts by describing `@deffn` before detailing all the other
definition commands.

* [The Template for a Definition](#Def-Cmd-Template)
* [Definition Command Continuation Lines](#Def-Cmd-Continuation-Lines)
* [Optional and Repeated Arguments](#Optional-Arguments)
* [Omitting the Space After a Definition Name](#No-Space-After-Definition-Name)
* [`@deffnx`, et al.: Two or More ‘First’ Lines](#g_t_0040deffnx)
* [The Definition Commands](#Def-Cmds-in-Detail)
* [Generic Definition Commands](#Generic-Definition-Commands)
* [Conventions for Writing Definitions](#Def-Cmd-Conventions)
* [A Sample Function Definition](#Sample-Function-Definition)

---

Next: [Definition Command Continuation Lines](#Def-Cmd-Continuation-Lines), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.1 The Template for a Definition [¶](#The-Template-for-a-Definition)

```
@deffn category name arguments...
body-of-definition
@end deffn
```

The `@deffn` command is used for definitions of entities that
resemble functions—entities that may take arguments.
Write the `@deffn` command at the beginning of a line
and follow it on the same line by the category of the entity, the name
of the entity itself, and its arguments (if any). Then write the body
of the definition on succeeding lines.
Finally, end the definition with an `@end deffn` command
written on a line of its own.

For example,

```
@deffn Command forward-word count
This command moves point forward @var{count} words
(or backward if @var{count} is negative). ...
@end deffn
```

produces

> Command: **forward-word** count [¶](#index-forward_002dword)
> :   This command moves point forward count words
>     (or backward if count is negative). …

Capitalize the category name like a title. If the name of the
category contains spaces, as in the phrase ‘Interactive Command’,
enclose it in braces. For example:

```
@deffn {Interactive Command} isearch-forward
...
@end deffn
```

Otherwise, the second word will be mistaken for the name of the
entity. As a general rule, when any of the arguments in the heading
line *except* the last one are more than one word, you need to
enclose them in braces. This may also be necessary if the text
contains commands, for example, ‘`{declaraci@'on}`’ if you are
writing in Spanish.

The category is output in a different location for different output
formats. For example, in the Info file, the category appears at the
beginning of the first line of the definition. With TeX output,
the category is printed next to the right margin.

`@deffn` enters names into the index of functions.

Three predefined, specialized variations of `@deffn`
(`@defun`, `@defmac`, and `@defspec`) specify the
category for you: “Function”, “Macro”, and “Special Form”
respectively. (In Lisp, a special form is an entity much like a
function.) Similarly, the general `@defvr` command is
accompanied by several specialized variations for describing
particular kinds of variables.

---

Next: [Optional and Repeated Arguments](#Optional-Arguments), Previous: [The Template for a Definition](#Def-Cmd-Template), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.2 Definition Command Continuation Lines [¶](#Definition-Command-Continuation-Lines)

The heading line of a definition command can get very long.
Therefore, Texinfo has a special syntax allowing them to be continued
over multiple lines of the source file: a lone ‘`@`’ at the end of
each line to be continued. Here’s an example:

```
@defun fn-name @
  arg1 arg2 arg3
This is the basic continued defun.
@end defun
```

produces:

Function: **fn-name** arg1 arg2 arg3 [¶](#index-fn_002dname)
:   This is the basic continued defun.

As you can see, the continued lines are combined, as if they had been
typed on one source line.

Although this example only shows a one-line continuation,
continuations may extend over any number of lines, in the same way;
put an `@` at the end of each line to be continued.

In general, any number of spaces or tabs before the `@`
continuation character are collapsed into a single space. There is one
exception: the Texinfo processors will not fully collapse whitespace
around a continuation inside braces. For example:

```
@deffn {Category @
  Name} ...
```

The output (not shown) has excess space between ‘Category’
and ‘Name’. To avoid this, elide the unwanted whitespace in your
input, or put the continuation `@` outside braces.

`@` does not function as a continuation character in *any*
other context. Ordinarily, ‘`@`’ followed by a whitespace
character (space, tab, newline) produces a normal interword space
(see [Multiple Spaces](#Multiple-Spaces)).

---

Next: [Omitting the Space After a Definition Name](#No-Space-After-Definition-Name), Previous: [Definition Command Continuation Lines](#Def-Cmd-Continuation-Lines), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.3 Optional and Repeated Arguments [¶](#Optional-and-Repeated-Arguments)

Some entities take optional or repeated arguments. One convention for
indicating these uses square brackets and ellipses: an argument enclosed
within square brackets is optional, and an argument followed by an
ellipsis is optional and may be repeated more than once.

Thus, [optional-arg] means that optional-arg is optional
and repeated-args`…` stands for zero or more
arguments. Parentheses are used when several arguments are grouped
into additional levels of list structure in Lisp.

Here is the definition line of an example of an imaginary
(complicated) special form:

> Special Form: **foobar** (var [from to [inc]]) body… [¶](#index-foobar)

In this example, the arguments from and to are optional,
but must both be present or both absent. If they are present,
inc may optionally be specified as well. These arguments are
grouped with the argument var into a list, to distinguish them
from body, which includes all remaining elements of the
form.

In a Texinfo source file, this `@defspec` line is written like
this:

```
@defspec foobar (var [from to [inc]]) body@dots{}
```

---

Next: [`@deffnx`, et al.: Two or More ‘First’ Lines](#g_t_0040deffnx), Previous: [Optional and Repeated Arguments](#Optional-Arguments), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.4 Omitting the Space After a Definition Name [¶](#Omitting-the-Space-After-a-Definition-Name)

As a matter of style or as demanded by the syntax of a programming language,
you may wish to omit any space after a name in a definition, occurring
before an opening bracket. To do this, set the ‘`txidefnamenospace`’
flag (see [`@set` and `@value`](#g_t_0040set-_0040value)). For example, this input

```
@set txidefnamenospace
@deffn Builtin index (string, substring)
@dots{}
@end deffn
```

produces the following:

Builtin: **index**(string, substring) [¶](#index-index)
:   …

---

Next: [The Definition Commands](#Def-Cmds-in-Detail), Previous: [Omitting the Space After a Definition Name](#No-Space-After-Definition-Name), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]

### 13.5 `@deffnx`, et al.: Two or More ‘First’ Lines [¶](#g_t_0040deffnx_002c-et-al_002e_003a-Two-or-More-_0060First_0027-Lines)

To create two or more ‘first’ or header lines for a definition, follow
the first `@deffn` line by a line beginning with
`@deffnx`.

For example,

```
@deffn {Interactive Command} isearch-forward
@deffnx {Interactive Command} isearch-backward
These two search commands are similar except ...
@end deffn
```

produces

Interactive Command: **isearch-forward** [¶](#index-isearch_002dforward)

Interactive Command: **isearch-backward** [¶](#index-isearch_002dbackward)
:   These two search commands are similar except …

Each definition command has an ‘x’ form: `@defunx`,
`@defvrx`, `@deftypefunx`, etc.

The ‘x’ forms work similarly to `@itemx`
(see [`@itemx`: Second and Subsequent Items](#g_t_0040itemx)).

---

Next: [Generic Definition Commands](#Generic-Definition-Commands), Previous: [`@deffnx`, et al.: Two or More ‘First’ Lines](#g_t_0040deffnx), Up: [Definition Commands](#Definition-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Command-and-Variable-Index "Index")]
