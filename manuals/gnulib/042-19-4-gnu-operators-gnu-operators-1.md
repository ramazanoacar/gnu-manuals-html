### 19.4 GNU Operators [¶](#GNU-Operators-1)

The following are operators that GNU defines (and POSIX doesn’t) that
you can use unless the syntax bit `RE_NO_GNU_OPS` is set.

* [Word Operators](#Word-Operators)
* [Space Operators](#Space-Operators)
* [Whole-string Operators](#Whole_002dstring-Operators)

---

Next: [Space Operators](#Space-Operators), Up: [GNU Operators](#GNU-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.1 Word Operators [¶](#Word-Operators-1)

The operators in this section require Regex to recognize parts of words.
Characters that are part of words, which are called
*word-constituent*, are letters, digits, and the underscore
(‘`_`’); more precisely, any character in the POSIX class
`alnum` in the current locale, or underscore.

* [The Match-word-boundary Operator (`\b`)](#Match_002dword_002dboundary-Operator)
* [The Match-within-word Operator (`\B`)](#Match_002dwithin_002dword-Operator)
* [The Match-beginning-of-word Operator (`\<`)](#Match_002dbeginning_002dof_002dword-Operator)
* [The Match-end-of-word Operator (`\>`)](#Match_002dend_002dof_002dword-Operator)
* [The Match-word-constituent Operator (`\w`)](#Match_002dword_002dconstituent-Operator)
* [The Match-non-word-constituent Operator (`\W`)](#Match_002dnon_002dword_002dconstituent-Operator)

---

Next: [The Match-within-word Operator (`\B`)](#Match_002dwithin_002dword-Operator), Up: [Word Operators](#Word-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.1.1 The Match-word-boundary Operator (`\b`) [¶](#The-Match_002dword_002dboundary-Operator-_0028_005cb_0029)

This operator (represented by ‘`\b`’) matches the empty string at
either the beginning or the end of a word. For example, ‘`\brat\b`’
matches the separate word ‘`rat`’.

---

Next: [The Match-beginning-of-word Operator (`\<`)](#Match_002dbeginning_002dof_002dword-Operator), Previous: [The Match-word-boundary Operator (`\b`)](#Match_002dword_002dboundary-Operator), Up: [Word Operators](#Word-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.1.2 The Match-within-word Operator (`\B`) [¶](#The-Match_002dwithin_002dword-Operator-_0028_005cB_0029)

This operator (represented by ‘`\B`’) matches the empty string within
a word. For example, ‘`c\Brat\Be`’ matches ‘`crate`’, but
‘`dirty \Brat`’ doesn’t match ‘`dirty rat`’.

---

Next: [The Match-end-of-word Operator (`\>`)](#Match_002dend_002dof_002dword-Operator), Previous: [The Match-within-word Operator (`\B`)](#Match_002dwithin_002dword-Operator), Up: [Word Operators](#Word-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.1.3 The Match-beginning-of-word Operator (`\<`) [¶](#The-Match_002dbeginning_002dof_002dword-Operator-_0028_005c_003c_0029)

This operator (represented by ‘`\<`’) matches the empty string at the
beginning of a word.

---

Next: [The Match-word-constituent Operator (`\w`)](#Match_002dword_002dconstituent-Operator), Previous: [The Match-beginning-of-word Operator (`\<`)](#Match_002dbeginning_002dof_002dword-Operator), Up: [Word Operators](#Word-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.1.4 The Match-end-of-word Operator (`\>`) [¶](#The-Match_002dend_002dof_002dword-Operator-_0028_005c_003e_0029)

This operator (represented by ‘`\>`’) matches the empty string at the
end of a word.

---

Next: [The Match-non-word-constituent Operator (`\W`)](#Match_002dnon_002dword_002dconstituent-Operator), Previous: [The Match-end-of-word Operator (`\>`)](#Match_002dend_002dof_002dword-Operator), Up: [Word Operators](#Word-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.1.5 The Match-word-constituent Operator (`\w`) [¶](#The-Match_002dword_002dconstituent-Operator-_0028_005cw_0029)

This operator (represented by ‘`\w`’) matches any word-constituent
character.

---

Previous: [The Match-word-constituent Operator (`\w`)](#Match_002dword_002dconstituent-Operator), Up: [Word Operators](#Word-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.1.6 The Match-non-word-constituent Operator (`\W`) [¶](#The-Match_002dnon_002dword_002dconstituent-Operator-_0028_005cW_0029)

This operator (represented by ‘`\W`’) matches any character that is
not word-constituent.

---

Next: [Whole-string Operators](#Whole_002dstring-Operators), Previous: [Word Operators](#Word-Operators), Up: [GNU Operators](#GNU-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.2 Space Operators [¶](#Space-Operators-1)

* [The Match-space Operator (`\s`)](#Match_002dspace-Operator)
* [The Match-non-space Operator (`\S`)](#Match_002dnon_002dspace-Operator)

---

Next: [The Match-non-space Operator (`\S`)](#Match_002dnon_002dspace-Operator), Up: [Space Operators](#Space-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.2.1 The Match-space Operator (`\s`) [¶](#The-Match_002dspace-Operator-_0028_005cs_0029)

This operator (represented by ‘`\s`’) matches any space
character (that is, in the POSIX class `[:space:]`).

---

Previous: [The Match-space Operator (`\s`)](#Match_002dspace-Operator), Up: [Space Operators](#Space-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.2.2 The Match-non-space Operator (`\S`) [¶](#The-Match_002dnon_002dspace-Operator-_0028_005cS_0029)

This operator (represented by ‘`\S`’) matches any character
that is not a space (that is, in the POSIX class `[:space:]`).

---

Previous: [Space Operators](#Space-Operators), Up: [GNU Operators](#GNU-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.3 Whole-string Operators [¶](#Whole_002dstring-Operators-1)

Following are operators which work on the whole string.

* [The Match-beginning-of-string Operator (`` \` ``)](#Match_002dbeginning_002dof_002dstring-Operator)
* [The Match-end-of-string Operator (`\'`)](#Match_002dend_002dof_002dstring-Operator)

---

Next: [The Match-end-of-string Operator (`\'`)](#Match_002dend_002dof_002dstring-Operator), Up: [Whole-string Operators](#Whole_002dstring-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.3.1 The Match-beginning-of-string Operator (`` \` ``) [¶](#The-Match_002dbeginning_002dof_002dstring-Operator-_0028_005c_0060_0029)

This operator (represented by ‘`` \` ``’) matches the empty string at the
beginning of the string.

---

Previous: [The Match-beginning-of-string Operator (`` \` ``)](#Match_002dbeginning_002dof_002dstring-Operator), Up: [Whole-string Operators](#Whole_002dstring-Operators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.4.3.2 The Match-end-of-string Operator (`\'`) [¶](#The-Match_002dend_002dof_002dstring-Operator-_0028_005c_0027_0029)

This operator (represented by ‘`\'`’) matches the empty string at the
end of the string.

---

Next: [Programming with Regex](#Programming-with-Regex), Previous: [GNU Operators](#GNU-Operators), Up: [Regular expressions](#Regular-expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 19.5 What Gets Matched? [¶](#What-Gets-Matched_003f-1)

Regex usually matches strings according to the “leftmost longest”
rule; that is, it chooses the longest of the leftmost matches. This
does not mean that for a regular expression containing subexpressions
that it simply chooses the longest match for each subexpression, left to
right; the overall match must also be the longest possible one.

For example, ‘`(ac*)(c*d[ac]*)\1`’ matches ‘`acdacaaa`’, not
‘`acdac`’, as it would if it were to choose the longest match for the
first subexpression.

---

Next: [Regular expression syntaxes](#Regular-expression-syntaxes), Previous: [What Gets Matched?](#What-Gets-Matched_003f), Up: [Regular expressions](#Regular-expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 19.6 Programming with Regex [¶](#Programming-with-Regex-1)

Here we describe how you use the Regex data structures and functions in
C programs. Regex has three interfaces: one designed for GNU, one
compatible with POSIX (as specified by POSIX, draft
1003.2/D11.2), and one compatible with Berkeley Unix. The
POSIX interface is not documented here; see the documentation of
GNU libc, or the POSIX man pages. The Berkeley Unix interface is
documented here for convenience, since its documentation is not
otherwise readily available on GNU systems.

* [GNU Regex Functions](#GNU-Regex-Functions)
* [BSD Regex Functions](#BSD-Regex-Functions)

---

Next: [BSD Regex Functions](#BSD-Regex-Functions), Up: [Programming with Regex](#Programming-with-Regex)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1 GNU Regex Functions [¶](#GNU-Regex-Functions-1)

If you’re writing code that doesn’t need to be compatible with either
POSIX or Berkeley Unix, you can use these functions. They
provide more options than the other interfaces.

* [GNU Pattern Buffers](#GNU-Pattern-Buffers)
* [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling)
* [GNU Matching](#GNU-Matching)
* [GNU Searching](#GNU-Searching)
* [Matching and Searching with Split Data](#Matching_002fSearching-with-Split-Data)
* [Searching with Fastmaps](#Searching-with-Fastmaps)
* [GNU Translate Tables](#GNU-Translate-Tables)
* [Using Registers](#Using-Registers)
* [Freeing GNU Pattern Buffers](#Freeing-GNU-Pattern-Buffers)

---

Next: [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.1 GNU Pattern Buffers [¶](#GNU-Pattern-Buffers-1)

To compile, match, or search for a given regular expression, you must
supply a pattern buffer. A *pattern buffer* holds one compiled
regular expression.[6](#FOOT6)

You can have several different pattern buffers simultaneously, each
holding a compiled pattern for a different regular expression.

`regex.h` defines the pattern buffer `struct` with the
following public fields:

```
  unsigned char *buffer;
  unsigned long allocated;
  char *fastmap;
  char *translate;
  size_t re_nsub;
  unsigned no_sub : 1;
  unsigned not_bol : 1;
  unsigned not_eol : 1;
```

---

Next: [GNU Matching](#GNU-Matching), Previous: [GNU Pattern Buffers](#GNU-Pattern-Buffers), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.2 GNU Regular Expression Compiling [¶](#GNU-Regular-Expression-Compiling-1)

In GNU, you can both match and search for a given regular
expression. To do either, you must first compile it in a pattern buffer
(see [GNU Pattern Buffers](#GNU-Pattern-Buffers)).

Regular expressions match according to the syntax with which they were
compiled; with GNU, you indicate what syntax you want by setting
the variable `re_syntax_options` (declared in `regex.h`)
before calling the compiling function, `re_compile_pattern` (see
below). See [Syntax Bits](#Syntax-Bits), and [Predefined Syntaxes](#Predefined-Syntaxes).

You can change the value of `re_syntax_options` at any time.
Usually, however, you set its value once and then never change it.

`re_compile_pattern` takes a pattern buffer as an argument. You
must initialize the following fields:

`translate initialization`

`translate` [¶](#index-translate-initialization)
:   Initialize this to point to a translate table if you want one, or to
    zero if you don’t. We explain translate tables in [GNU Translate Tables](#GNU-Translate-Tables).

`fastmap` [¶](#index-fastmap-initialization)
:   Initialize this to nonzero if you want a fastmap, or to zero if you
    don’t.

`buffer` [¶](#index-buffer-initialization)

`allocated`
:   If you want `re_compile_pattern` to allocate memory for the
    compiled pattern, set both of these to zero. If you have an existing
    block of memory (allocated with `malloc`) you want Regex to use,
    set `buffer` to its address and `allocated` to its size (in
    bytes).

    `re_compile_pattern` uses `realloc` to extend the space for
    the compiled pattern as necessary.

To compile a pattern buffer, use:

```
char *
re_compile_pattern (const char *regex, const int regex_size,
                    struct re_pattern_buffer *pattern_buffer)
```

regex is the regular expression’s address, regex\_size is its
length, and pattern\_buffer is the pattern buffer’s address.

If `re_compile_pattern` successfully compiles the regular
expression, it returns zero and sets `*pattern_buffer` to the
compiled pattern. It sets the pattern buffer’s fields as follows:

`buffer` [¶](#index-buffer-field_002c-set-by-re_005fcompile_005fpattern)
:   to the compiled pattern.

`syntax` [¶](#index-syntax-field_002c-set-by-re_005fcompile_005fpattern)
:   to the current value of `re_syntax_options`.

`re_nsub` [¶](#index-re_005fnsub-field_002c-set-by-re_005fcompile_005fpattern)
:   to the number of subexpressions in regex.

If `re_compile_pattern` can’t compile regex, it returns an
error string corresponding to a POSIX error code.

---

Next: [GNU Searching](#GNU-Searching), Previous: [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.3 GNU Matching [¶](#GNU-Matching-1)

Matching the GNU way means trying to match as much of a string as
possible starting at a position within it you specify. Once you’ve compiled
a pattern into a pattern buffer (see [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling)), you can ask the matcher to match that pattern against a
string using:

```
int
re_match (struct re_pattern_buffer *pattern_buffer,
          const char *string, const int size,
          const int start, struct re_registers *regs)
```

pattern\_buffer is the address of a pattern buffer containing a
compiled pattern. string is the string you want to match; it can
contain newline and null characters. size is the length of that
string. start is the string index at which you want to
begin matching; the first character of string is at index zero.
See [Using Registers](#Using-Registers), for an explanation of regs; you can safely
pass zero.

`re_match` matches the regular expression in pattern\_buffer
against the string string according to the syntax of
pattern\_buffer. (See [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling), for how
to set it.) The function returns *-1* if the compiled pattern does
not match any part of string and *-2* if an internal error
happens; otherwise, it returns how many (possibly zero) characters of
string the pattern matched.

An example: suppose pattern\_buffer points to a pattern buffer
containing the compiled pattern for ‘`a*`’, and string points
to ‘`aaaaab`’ (whereupon size should be 6). Then if start
is 2, `re_match` returns 3, i.e., ‘`a*`’ would have matched the
last three ‘`a`’s in string. If start is 0,
`re_match` returns 5, i.e., ‘`a*`’ would have matched all the
‘`a`’s in string. If start is either 5 or 6, it returns
zero.

If start is not between zero and size, then
`re_match` returns *-1*.

---

Next: [Matching and Searching with Split Data](#Matching_002fSearching-with-Split-Data), Previous: [GNU Matching](#GNU-Matching), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.4 GNU Searching [¶](#GNU-Searching-1)

*Searching* means trying to match starting at successive positions
within a string. The function `re_search` does this.

Before calling `re_search`, you must compile your regular
expression. See [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling).

Here is the function declaration:

```
int
re_search (struct re_pattern_buffer *pattern_buffer,
           const char *string, const int size,
           const int start, const int range,
           struct re_registers *regs)
```

whose arguments are the same as those to `re_match` (see [GNU Matching](#GNU-Matching)) except that the two arguments start and range
replace `re_match`’s argument start.

If range is positive, then `re_search` attempts a match
starting first at index start, then at *start + 1* if
that fails, and so on, up to *start + range*; if
range is negative, then it attempts a match starting first at
index start, then at *start -1* if that fails, and so
on.

If start is not between zero and size, then `re_search`
returns *-1*. When range is positive, `re_search`
adjusts range so that *start + range - 1* is
between zero and size, if necessary; that way it won’t search
outside of string. Similarly, when range is negative,
`re_search` adjusts range so that *start +
range + 1* is between zero and size, if necessary.

If the `fastmap` field of pattern\_buffer is zero,
`re_search` matches starting at consecutive positions; otherwise,
it uses `fastmap` to make the search more efficient.
See [Searching with Fastmaps](#Searching-with-Fastmaps).

If no match is found, `re_search` returns *-1*. If
a match is found, it returns the index where the match began. If an
internal error happens, it returns *-2*.

---

Next: [Searching with Fastmaps](#Searching-with-Fastmaps), Previous: [GNU Searching](#GNU-Searching), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.5 Matching and Searching with Split Data [¶](#Matching-and-Searching-with-Split-Data)

Using the functions `re_match_2` and `re_search_2`, you can
match or search in data that is divided into two strings.

The function:

```
int
re_match_2 (struct re_pattern_buffer *buffer,
            const char *string1, const int size1,
            const char *string2, const int size2,
            const int start,
            struct re_registers *regs,
            const int stop)
```

is similar to `re_match` (see [GNU Matching](#GNU-Matching)) except that you
pass *two* data strings and sizes, and an index stop beyond
which you don’t want the matcher to try matching. As with
`re_match`, if it succeeds, `re_match_2` returns how many
characters of string it matched. Regard string1 and
string2 as concatenated when you set the arguments start and
stop and use the contents of regs; `re_match_2` never
returns a value larger than *size1 + size2*.

The function:

```
int
re_search_2 (struct re_pattern_buffer *buffer,
             const char *string1, const int size1,
             const char *string2, const int size2,
             const int start, const int range,
             struct re_registers *regs,
             const int stop)
```

is similarly related to `re_search`.

---

Next: [GNU Translate Tables](#GNU-Translate-Tables), Previous: [Matching and Searching with Split Data](#Matching_002fSearching-with-Split-Data), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.6 Searching with Fastmaps [¶](#Searching-with-Fastmaps-1)

If you’re searching through a long string, you should use a fastmap.
Without one, the searcher tries to match at consecutive positions in the
string. Generally, most of the characters in the string could not start
a match. It takes much longer to try matching at a given position in the
string than it does to check in a table whether or not the character at
that position could start a match. A *fastmap* is such a table.

More specifically, a fastmap is an array indexed by the characters in
your character set. Under the ASCII encoding, therefore, a fastmap
has 256 elements. If you want the searcher to use a fastmap with a
given pattern buffer, you must allocate the array and assign the array’s
address to the pattern buffer’s `fastmap` field. You either can
compile the fastmap yourself or have `re_search` do it for you;
when `fastmap` is nonzero, it automatically compiles a fastmap the
first time you search using a particular compiled pattern.

By setting the buffer’s `fastmap` field before calling
`re_compile_pattern`, you can reuse a buffer data structure across
multiple searches with different patterns, and allocate the fastmap only
once. Nonetheless, the fastmap must be recompiled each time the buffer
has a new pattern compiled into it.

To compile a fastmap yourself, use:

```
int
re_compile_fastmap (struct re_pattern_buffer *pattern_buffer)
```

pattern\_buffer is the address of a pattern buffer. If the
character c could start a match for the pattern,
`re_compile_fastmap` makes
`pattern_buffer->fastmap[c]` nonzero. It returns
*0* if it can compile a fastmap and *-2* if there is an
internal error. For example, if ‘`|`’ is the alternation operator
and pattern\_buffer holds the compiled pattern for ‘`a|b`’, then
`re_compile_fastmap` sets `fastmap['a']` and
`fastmap['b']` (and no others).

`re_search` uses a fastmap as it moves along in the string: it
checks the string’s characters until it finds one that’s in the fastmap.
Then it tries matching at that character. If the match fails, it
repeats the process. So, by using a fastmap, `re_search` doesn’t
waste time trying to match at positions in the string that couldn’t
start a match.

If you don’t want `re_search` to use a fastmap,
store zero in the `fastmap` field of the pattern buffer before
calling `re_search`.

Once you’ve initialized a pattern buffer’s `fastmap` field, you
need never do so again—even if you compile a new pattern in
it—provided the way the field is set still reflects whether or not you
want a fastmap. `re_search` will still either do nothing if
`fastmap` is null or, if it isn’t, compile a new fastmap for the
new pattern.

---

Next: [Using Registers](#Using-Registers), Previous: [Searching with Fastmaps](#Searching-with-Fastmaps), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.7 GNU Translate Tables [¶](#GNU-Translate-Tables-1)

If you set the `translate` field of a pattern buffer to a translate
table, then the GNU Regex functions to which you’ve passed that
pattern buffer use it to apply a simple transformation
to all the regular expression and string characters at which they look.

A *translate table* is an array indexed by the characters in your
character set. Under the ASCII encoding, therefore, a translate
table has 256 elements. The array’s elements are also characters in
your character set. When the Regex functions see a character c,
they use `translate[c]` in its place, with one exception: the
character after a ‘`\`’ is not translated. (This ensures that, the
operators, e.g., ‘`\B`’ and ‘`\b`’, are always distinguishable.)

For example, a table that maps all lowercase letters to the
corresponding uppercase ones would cause the matcher to ignore
differences in case.[7](#FOOT7) Such a table would map all characters except lowercase letters
to themselves, and lowercase letters to the corresponding uppercase
ones. Under the ASCII encoding, here’s how you could initialize
such a table (we’ll call it `case_fold`):

```
for (i = 0; i < 256; i++)
  case_fold[i] = i;
for (i = 'a'; i <= 'z'; i++)
  case_fold[i] = i - ('a' - 'A');
```

You tell Regex to use a translate table on a given pattern buffer by
assigning that table’s address to the `translate` field of that
buffer. If you don’t want Regex to do any translation, put zero into
this field. You’ll get weird results if you change the table’s contents
anytime between compiling the pattern buffer, compiling its fastmap, and
matching or searching with the pattern buffer.

---

Next: [Freeing GNU Pattern Buffers](#Freeing-GNU-Pattern-Buffers), Previous: [GNU Translate Tables](#GNU-Translate-Tables), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.8 Using Registers [¶](#Using-Registers-1)

A group in a regular expression can match a (possibly empty) substring
of the string that regular expression as a whole matched. The matcher
remembers the beginning and end of the substring matched by
each group.

To find out what they matched, pass a nonzero regs argument to a
GNU matching or searching function (see [GNU Matching](#GNU-Matching) and
[GNU Searching](#GNU-Searching)), i.e., the address of a structure of this type, as
defined in `regex.h`:

```
struct re_registers
{
  unsigned num_regs;
  regoff_t *start;
  regoff_t *end;
};
```

Except for (possibly) the num\_regs’th element (see below), the
ith element of the `start` and `end` arrays records
information about the ith group in the pattern. (They’re declared
as C pointers, but this is only because not all C compilers accept
zero-length arrays; conceptually, it is simplest to think of them as
arrays.)

The `start` and `end` arrays are allocated in one of two ways.
The simplest and perhaps most useful is to let the matcher (re)allocate
enough space to record information for all the groups in the regular
expression. If `re_set_registers` is not called before searching
or matching, then the matcher allocates two arrays each of *1 +
re\_nsub* elements (re\_nsub is another field in the pattern
buffer; see [GNU Pattern Buffers](#GNU-Pattern-Buffers)). The extra element is set to
*-1*. Then on subsequent calls with the same pattern buffer and
regs arguments, the matcher reallocates more space if necessary.

The function:

```
void
re_set_registers (struct re_pattern_buffer *buffer,
                  struct re_registers *regs,
                  size_t num_regs,
                  regoff_t *starts, regoff_t *ends)
```

sets regs to hold num\_regs registers, storing
them in starts and ends. Subsequent matches using
buffer and regs will use this memory for recording
register information. starts and ends must be allocated
with malloc, and must each be at least *num\_regs \*
`sizeof (regoff_t)`* bytes long.

If num\_regs is zero, then subsequent matches should allocate
their own register data.

Unless this function is called, the first search or match using
buffer will allocate its own register data, without freeing the
old data.

The following examples illustrate the information recorded in the
`re_registers` structure. (In all of them, ‘`(`’ represents the
open-group and ‘`)`’ the close-group operator. The first character
in the string string is at index 0.)

* If the regular expression has an i-th
  group that matches a
  substring of string, then the function sets
  `regs->start[i]` to the index in string where
  the substring matched by the i-th group begins, and
  `regs->end[i]` to the index just beyond that
  substring’s end. The function sets `regs->start[0]` and
  `regs->end[0]` to analogous information about the entire
  pattern.

  For example, when you match ‘`((a)(b))`’ against ‘`ab`’, you get:

  + 0 in `regs->start[0]` and 2 in `regs->end[0]`
  + 0 in `regs->start[1]` and 2 in `regs->end[1]`
  + 0 in `regs->start[2]` and 1 in `regs->end[2]`
  + 1 in `regs->start[3]` and 2 in `regs->end[3]`
* If a group matches more than once (as it might if followed by,
  e.g., a repetition operator), then the function reports the information
  about what the group *last* matched.

  For example, when you match the pattern ‘`(a)*`’ against the string
  ‘`aa`’, you get:

  + 0 in `regs->start[0]` and 2 in `regs->end[0]`
  + 1 in `regs->start[1]` and 2 in `regs->end[1]`
* If the i-th group does not participate in a
  successful match, e.g., it is an alternative not taken or a
  repetition operator allows zero repetitions of it, then the function
  sets `regs->start[i]` and
  `regs->end[i]` to *-1*.

  For example, when you match the pattern ‘`(a)*b`’ against
  the string ‘`b`’, you get:

  + 0 in `regs->start[0]` and 1 in `regs->end[0]`
  + *-1* in `regs->start[1]` and *-1* in `regs->end[1]`
* If the i-th group matches a zero-length string, then the
  function sets `regs->start[i]` and
  `regs->end[i]` to the index just beyond that
  zero-length string.

  For example, when you match the pattern ‘`(a*)b`’ against the string
  ‘`b`’, you get:

  + 0 in `regs->start[0]` and 1 in `regs->end[0]`
  + 0 in `regs->start[1]` and 0 in `regs->end[1]`
* If an i-th group contains a j-th group
  in turn not contained within any other group within group i and
  the function reports a match of the i-th group, then it
  records in `regs->start[j]` and
  `regs->end[j]` the last match (if it matched) of
  the j-th group.

  For example, when you match the pattern ‘`((a*)b)*`’ against the
  string ‘`abb`’, group 2 last matches the empty string, so you
  get what it previously matched:

  + 0 in `regs->start[0]` and 3 in `regs->end[0]`
  + 2 in `regs->start[1]` and 3 in `regs->end[1]`
  + 2 in `regs->start[2]` and 2 in `regs->end[2]`

  When you match the pattern ‘`((a)*b)*`’ against the string
  ‘`abb`’, group 2 doesn’t participate in the last match, so you
  get:

  + 0 in `regs->start[0]` and 3 in `regs->end[0]`
  + 2 in `regs->start[1]` and 3 in `regs->end[1]`
  + 0 in `regs->start[2]` and 1 in `regs->end[2]`
* If an i-th group contains a j-th group
  in turn not contained within any other group within group i
  and the function sets
  `regs->start[i]` and
  `regs->end[i]` to *-1*, then it also sets
  `regs->start[j]` and
  `regs->end[j]` to *-1*.

  For example, when you match the pattern ‘`((a)*b)*c`’ against the
  string ‘`c`’, you get:

  + 0 in `regs->start[0]` and 1 in `regs->end[0]`
  + *-1* in `regs->start[1]` and *-1* in `regs->end[1]`
  + *-1* in `regs->start[2]` and *-1* in `regs->end[2]`

---

Previous: [Using Registers](#Using-Registers), Up: [GNU Regex Functions](#GNU-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.1.9 Freeing GNU Pattern Buffers [¶](#Freeing-GNU-Pattern-Buffers-1)

To free any allocated fields of a pattern buffer, use the POSIX
function `regfree`:

```
void
regfree (regex_t *preg)
```

preg is the pattern buffer whose allocated fields you want freed;
this works because since the type `regex_t`—the type for
POSIX pattern buffers—is equivalent to the type
`re_pattern_buffer`.

`regfree` also sets preg’s `allocated` field to zero.
After a buffer has been freed, it must have a regular expression
compiled in it before passing it to a matching or searching function.

---

Previous: [GNU Regex Functions](#GNU-Regex-Functions), Up: [Programming with Regex](#Programming-with-Regex)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.2 BSD Regex Functions [¶](#BSD-Regex-Functions-1)

If you’re writing code that has to be Berkeley Unix compatible,
you’ll need to use these functions whose interfaces are the same as those
in Berkeley Unix.

* [BSD Regular Expression Compiling](#BSD-Regular-Expression-Compiling)
* [BSD Searching](#BSD-Searching)

---

Next: [BSD Searching](#BSD-Searching), Up: [BSD Regex Functions](#BSD-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.2.1 BSD Regular Expression Compiling [¶](#BSD-Regular-Expression-Compiling-1)

With Berkeley Unix, you can only search for a given regular
expression; you can’t match one. To search for it, you must first
compile it. Before you compile it, you must indicate the regular
expression syntax you want it compiled according to by setting the
variable `re_syntax_options` (declared in `regex.h`) to some
syntax (see [Regular Expression Syntax](#Regular-Expression-Syntax)).

To compile a regular expression use:

```
char *
re_comp (char *regex)
```

regex is the address of a null-terminated regular expression.
`re_comp` uses an internal pattern buffer, so you can use only the
most recently compiled pattern buffer. This means that if you want to
use a given regular expression that you’ve already compiled—but it
isn’t the latest one you’ve compiled—you’ll have to recompile it. If
you call `re_comp` with the null string (*not* the empty
string) as the argument, it doesn’t change the contents of the pattern
buffer.

If `re_comp` successfully compiles the regular expression, it
returns zero. If it can’t compile the regular expression, it returns
an error string. `re_comp`’s error messages are identical to those
of `re_compile_pattern` (see [GNU Regular Expression Compiling](#GNU-Regular-Expression-Compiling)).

---

Previous: [BSD Regular Expression Compiling](#BSD-Regular-Expression-Compiling), Up: [BSD Regex Functions](#BSD-Regex-Functions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.6.2.2 BSD Searching [¶](#BSD-Searching-1)

Searching the Berkeley Unix way means searching in a string
starting at its first character and trying successive positions within
it to find a match. Once you’ve compiled a pattern using `re_comp`
(see [BSD Regular Expression Compiling](#BSD-Regular-Expression-Compiling)), you can ask Regex
to search for that pattern in a string using:

```
int
re_exec (char *string)
```

string is the address of the null-terminated string in which you
want to search.

`re_exec` returns either 1 for success or 0 for failure. It
automatically uses a GNU fastmap (see [Searching with Fastmaps](#Searching-with-Fastmaps)).

---

Previous: [Programming with Regex](#Programming-with-Regex), Up: [Regular expressions](#Regular-expressions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 19.7 Regular expression syntaxes [¶](#Regular-expression-syntaxes-1)

Gnulib supports many different types of regular expressions; although
the underlying features are the same or identical, the syntax used
varies. The descriptions given here for the different types are
generated automatically.

* [‘`awk`’ regular expression syntax](#awk-regular-expression-syntax)
* [‘`ed`’ regular expression syntax](#ed-regular-expression-syntax)
* [‘`egrep`’ regular expression syntax](#egrep-regular-expression-syntax)
* [‘`emacs`’ regular expression syntax](#emacs-regular-expression-syntax)
* [‘`gnu-awk`’ regular expression syntax](#gnu_002dawk-regular-expression-syntax)
* [‘`grep`’ regular expression syntax](#grep-regular-expression-syntax)
* [‘`posix-awk`’ regular expression syntax](#posix_002dawk-regular-expression-syntax)
* [‘`posix-basic`’ regular expression syntax](#posix_002dbasic-regular-expression-syntax)
* [‘`posix-egrep`’ regular expression syntax](#posix_002degrep-regular-expression-syntax)
* [‘`posix-extended`’ regular expression syntax](#posix_002dextended-regular-expression-syntax)
* [‘`posix-minimal-basic`’ regular expression syntax](#posix_002dminimal_002dbasic-regular-expression-syntax)
* [‘`sed`’ regular expression syntax](#sed-regular-expression-syntax)

---

Next: [‘`ed`’ regular expression syntax](#ed-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.1 ‘`awk`’ regular expression syntax [¶](#awk-regular-expression-syntax-1)

The character ‘`.`’ matches any single character except the null character.

‘`+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`\+`’
:   matches a ‘`+`’

‘`\?`’
:   matches a ‘`?`’.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ can be used to quote the following character. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are not supported and so ‘`\w`’, ‘`\W`’, ‘`\<`’, ‘`\>`’, ‘`\b`’, ‘`\B`’, ‘`` \` ``’, and ‘`\'`’ match ‘`w`’, ‘`W`’, ‘`<`’, ‘`>`’, ‘`b`’, ‘`B`’, ‘`` ` ``’, and ‘`'`’ respectively.

Grouping is performed with parentheses ‘`()`’. An unmatched ‘`)`’ matches just itself. A backslash followed by a digit matches that digit.

The alternation operator is ‘`|`’.

The characters ‘`^`’ and ‘`$`’ always represent the beginning and end of a string respectively, except within square brackets. Within brackets, ‘`^`’ can be used to invert the membership of the character class being specified.

‘`*`’, ‘`+`’ and ‘`?`’ are special at any point in a regular expression except:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`(`’
3. After the alternation operator ‘`|`’

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`egrep`’ regular expression syntax](#egrep-regular-expression-syntax), Previous: [‘`awk`’ regular expression syntax](#awk-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.2 ‘`ed`’ regular expression syntax [¶](#ed-regular-expression-syntax-1)

The character ‘`.`’ matches any single character except the null character.

‘`\+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`\?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`+ and ?`’
:   match themselves.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ is taken literally. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are supported:

1. ‘`\w`’ matches a character within a word
2. ‘`\W`’ matches a character which is not within a word
3. ‘`\<`’ matches the beginning of a word
4. ‘`\>`’ matches the end of a word
5. ‘`\b`’ matches a word boundary
6. ‘`\B`’ matches characters which are not a word boundary
7. ‘`` \` ``’ matches the beginning of the whole input
8. ‘`\'`’ matches the end of the whole input

Grouping is performed with backslashes followed by parentheses ‘`\(`’, ‘`\)`’. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`\(`’.

The alternation operator is ‘`\|`’.

The character ‘`^`’ only represents the beginning of a string when it appears:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`\(`’
3. After the alternation operator ‘`\|`’

The character ‘`$`’ only represents the end of a string when it appears:

1. At the end of a regular expression
2. Before a close-group, signified by ‘`\)`’
3. Before the alternation operator ‘`\|`’

‘`\*`’, ‘`\+`’ and ‘`\?`’ are special at any point in a regular expression except:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`\(`’
3. After the alternation operator ‘`\|`’

Intervals are specified by ‘`\{`’ and ‘`\}`’.
Invalid intervals such as ‘`a\{1z`’ are not accepted.

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`emacs`’ regular expression syntax](#emacs-regular-expression-syntax), Previous: [‘`ed`’ regular expression syntax](#ed-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.3 ‘`egrep`’ regular expression syntax [¶](#egrep-regular-expression-syntax-1)

The character ‘`.`’ matches any single character.

‘`+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`\+`’
:   matches a ‘`+`’

‘`\?`’
:   matches a ‘`?`’.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ is taken literally. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are supported:

1. ‘`\w`’ matches a character within a word
2. ‘`\W`’ matches a character which is not within a word
3. ‘`\<`’ matches the beginning of a word
4. ‘`\>`’ matches the end of a word
5. ‘`\b`’ matches a word boundary
6. ‘`\B`’ matches characters which are not a word boundary
7. ‘`` \` ``’ matches the beginning of the whole input
8. ‘`\'`’ matches the end of the whole input

Grouping is performed with parentheses ‘`()`’. An unmatched ‘`)`’ matches just itself. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`(`’.

The alternation operator is ‘`|`’.

The characters ‘`^`’ and ‘`$`’ always represent the beginning and end of a string respectively, except within square brackets. Within brackets, ‘`^`’ can be used to invert the membership of the character class being specified.

The characters ‘`*`’, ‘`+`’ and ‘`?`’ are special anywhere in a regular expression.

Intervals are specified by ‘`{`’ and ‘`}`’.
Invalid intervals are treated as literals, for example ‘`a{1`’ is treated as ‘`a\{1`’

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`gnu-awk`’ regular expression syntax](#gnu_002dawk-regular-expression-syntax), Previous: [‘`egrep`’ regular expression syntax](#egrep-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.4 ‘`emacs`’ regular expression syntax [¶](#emacs-regular-expression-syntax-1)

The character ‘`.`’ matches any single character except newline.

‘`+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`\+`’
:   matches a ‘`+`’

‘`\?`’
:   matches a ‘`?`’.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are ignored. Within square brackets, ‘`\`’ is taken literally. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are supported:

1. ‘`\w`’ matches a character within a word
2. ‘`\W`’ matches a character which is not within a word
3. ‘`\<`’ matches the beginning of a word
4. ‘`\>`’ matches the end of a word
5. ‘`\b`’ matches a word boundary
6. ‘`\B`’ matches characters which are not a word boundary
7. ‘`` \` ``’ matches the beginning of the whole input
8. ‘`\'`’ matches the end of the whole input

Grouping is performed with backslashes followed by parentheses ‘`\(`’, ‘`\)`’. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`\(`’.

The alternation operator is ‘`\|`’.

The character ‘`^`’ only represents the beginning of a string when it appears:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`\(`’
3. After the alternation operator ‘`\|`’

The character ‘`$`’ only represents the end of a string when it appears:

1. At the end of a regular expression
2. Before a close-group, signified by ‘`\)`’
3. Before the alternation operator ‘`\|`’

‘`*`’, ‘`+`’ and ‘`?`’ are special at any point in a regular expression except:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`\(`’
3. After the alternation operator ‘`\|`’

Intervals are specified by ‘`\{`’ and ‘`\}`’.
Invalid intervals such as ‘`a\{1z`’ are not accepted.

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`grep`’ regular expression syntax](#grep-regular-expression-syntax), Previous: [‘`emacs`’ regular expression syntax](#emacs-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.5 ‘`gnu-awk`’ regular expression syntax [¶](#gnu_002dawk-regular-expression-syntax-1)

The character ‘`.`’ matches any single character.

‘`+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`\+`’
:   matches a ‘`+`’

‘`\?`’
:   matches a ‘`?`’.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ can be used to quote the following character. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are supported:

1. ‘`\w`’ matches a character within a word
2. ‘`\W`’ matches a character which is not within a word
3. ‘`\<`’ matches the beginning of a word
4. ‘`\>`’ matches the end of a word
5. ‘`\b`’ matches a word boundary
6. ‘`\B`’ matches characters which are not a word boundary
7. ‘`` \` ``’ matches the beginning of the whole input
8. ‘`\'`’ matches the end of the whole input

Grouping is performed with parentheses ‘`()`’. An unmatched ‘`)`’ matches just itself. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`(`’.

The alternation operator is ‘`|`’.

The characters ‘`^`’ and ‘`$`’ always represent the beginning and end of a string respectively, except within square brackets. Within brackets, ‘`^`’ can be used to invert the membership of the character class being specified.

‘`*`’, ‘`+`’ and ‘`?`’ are special at any point in a regular expression except:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`(`’
3. After the alternation operator ‘`|`’

Intervals are specified by ‘`{`’ and ‘`}`’.
Invalid intervals are treated as literals, for example ‘`a{1`’ is treated as ‘`a\{1`’

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`posix-awk`’ regular expression syntax](#posix_002dawk-regular-expression-syntax), Previous: [‘`gnu-awk`’ regular expression syntax](#gnu_002dawk-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.6 ‘`grep`’ regular expression syntax [¶](#grep-regular-expression-syntax-1)

The character ‘`.`’ matches any single character.

‘`\+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`\?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`+ and ?`’
:   match themselves.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ is taken literally. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are supported:

1. ‘`\w`’ matches a character within a word
2. ‘`\W`’ matches a character which is not within a word
3. ‘`\<`’ matches the beginning of a word
4. ‘`\>`’ matches the end of a word
5. ‘`\b`’ matches a word boundary
6. ‘`\B`’ matches characters which are not a word boundary
7. ‘`` \` ``’ matches the beginning of the whole input
8. ‘`\'`’ matches the end of the whole input

Grouping is performed with backslashes followed by parentheses ‘`\(`’, ‘`\)`’. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`\(`’.

The alternation operator is ‘`\|`’.

The character ‘`^`’ only represents the beginning of a string when it appears:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`\(`’
3. After a newline
4. After the alternation operator ‘`\|`’

The character ‘`$`’ only represents the end of a string when it appears:

1. At the end of a regular expression
2. Before a close-group, signified by ‘`\)`’
3. Before a newline
4. Before the alternation operator ‘`\|`’

‘`\*`’, ‘`\+`’ and ‘`\?`’ are special at any point in a regular expression except:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`\(`’
3. After a newline
4. After the alternation operator ‘`\|`’

Intervals are specified by ‘`\{`’ and ‘`\}`’.
Invalid intervals such as ‘`a\{1z`’ are not accepted.

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`posix-basic`’ regular expression syntax](#posix_002dbasic-regular-expression-syntax), Previous: [‘`grep`’ regular expression syntax](#grep-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.7 ‘`posix-awk`’ regular expression syntax [¶](#posix_002dawk-regular-expression-syntax-1)

The character ‘`.`’ matches any single character except the null character.

‘`+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`\+`’
:   matches a ‘`+`’

‘`\?`’
:   matches a ‘`?`’.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ can be used to quote the following character. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are not supported and so ‘`\w`’, ‘`\W`’, ‘`\<`’, ‘`\>`’, ‘`\b`’, ‘`\B`’, ‘`` \` ``’, and ‘`\'`’ match ‘`w`’, ‘`W`’, ‘`<`’, ‘`>`’, ‘`b`’, ‘`B`’, ‘`` ` ``’, and ‘`'`’ respectively.

Grouping is performed with parentheses ‘`()`’. An unmatched ‘`)`’ matches just itself. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`(`’.

The alternation operator is ‘`|`’.

The characters ‘`^`’ and ‘`$`’ always represent the beginning and end of a string respectively, except within square brackets. Within brackets, ‘`^`’ can be used to invert the membership of the character class being specified.

‘`*`’, ‘`+`’ and ‘`?`’ are special at any point in a regular expression except the following places, where they are not allowed:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`(`’
3. After the alternation operator ‘`|`’

Intervals are specified by ‘`{`’ and ‘`}`’.
Invalid intervals are treated as literals, for example ‘`a{1`’ is treated as ‘`a\{1`’

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`posix-egrep`’ regular expression syntax](#posix_002degrep-regular-expression-syntax), Previous: [‘`posix-awk`’ regular expression syntax](#posix_002dawk-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.8 ‘`posix-basic`’ regular expression syntax [¶](#posix_002dbasic-regular-expression-syntax-1)

This is a synonym for ed.

---

Next: [‘`posix-extended`’ regular expression syntax](#posix_002dextended-regular-expression-syntax), Previous: [‘`posix-basic`’ regular expression syntax](#posix_002dbasic-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.9 ‘`posix-egrep`’ regular expression syntax [¶](#posix_002degrep-regular-expression-syntax-1)

This is a synonym for egrep.

---

Next: [‘`posix-minimal-basic`’ regular expression syntax](#posix_002dminimal_002dbasic-regular-expression-syntax), Previous: [‘`posix-egrep`’ regular expression syntax](#posix_002degrep-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.10 ‘`posix-extended`’ regular expression syntax [¶](#posix_002dextended-regular-expression-syntax-1)

The character ‘`.`’ matches any single character except the null character.

‘`+`’
:   indicates that the regular expression should match one or more occurrences of the previous atom or regexp.

‘`?`’
:   indicates that the regular expression should match zero or one occurrence of the previous atom or regexp.

‘`\+`’
:   matches a ‘`+`’

‘`\?`’
:   matches a ‘`?`’.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ is taken literally. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are supported:

1. ‘`\w`’ matches a character within a word
2. ‘`\W`’ matches a character which is not within a word
3. ‘`\<`’ matches the beginning of a word
4. ‘`\>`’ matches the end of a word
5. ‘`\b`’ matches a word boundary
6. ‘`\B`’ matches characters which are not a word boundary
7. ‘`` \` ``’ matches the beginning of the whole input
8. ‘`\'`’ matches the end of the whole input

Grouping is performed with parentheses ‘`()`’. An unmatched ‘`)`’ matches just itself. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`(`’.

The alternation operator is ‘`|`’.

The characters ‘`^`’ and ‘`$`’ always represent the beginning and end of a string respectively, except within square brackets. Within brackets, ‘`^`’ can be used to invert the membership of the character class being specified.

‘`*`’, ‘`+`’ and ‘`?`’ are special at any point in a regular expression except the following places, where they are not allowed:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`(`’
3. After the alternation operator ‘`|`’

Intervals are specified by ‘`{`’ and ‘`}`’.
Invalid intervals such as ‘`a{1z`’ are not accepted.

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Next: [‘`sed`’ regular expression syntax](#sed-regular-expression-syntax), Previous: [‘`posix-extended`’ regular expression syntax](#posix_002dextended-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.11 ‘`posix-minimal-basic`’ regular expression syntax [¶](#posix_002dminimal_002dbasic-regular-expression-syntax-1)

The character ‘`.`’ matches any single character except the null character.

Bracket expressions are used to match ranges of characters. Bracket expressions where the range is backward, for example ‘`[z-a]`’, are invalid. Within square brackets, ‘`\`’ is taken literally. Character classes are supported; for example ‘`[[:digit:]]`’ will match a single decimal digit.

GNU extensions are supported:

1. ‘`\w`’ matches a character within a word
2. ‘`\W`’ matches a character which is not within a word
3. ‘`\<`’ matches the beginning of a word
4. ‘`\>`’ matches the end of a word
5. ‘`\b`’ matches a word boundary
6. ‘`\B`’ matches characters which are not a word boundary
7. ‘`` \` ``’ matches the beginning of the whole input
8. ‘`\'`’ matches the end of the whole input

Grouping is performed with backslashes followed by parentheses ‘`\(`’, ‘`\)`’. A backslash followed by a digit acts as a back-reference and matches the same thing as the previous grouped expression indicated by that number. For example ‘`\2`’ matches the second group expression. The order of group expressions is determined by the position of their opening parenthesis ‘`\(`’.

The character ‘`^`’ only represents the beginning of a string when it appears:

1. At the beginning of a regular expression
2. After an open-group, signified by ‘`\(`’

The character ‘`$`’ only represents the end of a string when it appears:

1. At the end of a regular expression
2. Before a close-group, signified by ‘`\)`’

Intervals are specified by ‘`\{`’ and ‘`\}`’.
Invalid intervals such as ‘`a\{1z`’ are not accepted.

The longest possible match is returned; this applies to the regular expression as a whole and (subject to this constraint) to subexpressions within groups.

---

Previous: [‘`posix-minimal-basic`’ regular expression syntax](#posix_002dminimal_002dbasic-regular-expression-syntax), Up: [Regular expression syntaxes](#Regular-expression-syntaxes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

#### 19.7.12 ‘`sed`’ regular expression syntax [¶](#sed-regular-expression-syntax-1)

This is a synonym for ed.

---

Next: [Build Infrastructure Files](#Build-Infrastructure-Files), Previous: [Regular expressions](#Regular-expressions), Up: [GNU Gnulib](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]## 20 Build Infrastructure Modules [¶](#Build-Infrastructure-Modules-1)

Gnulib has a couple of modules that don’t provide code, but rather
extend the GNU Build System. That is, they are convenience facilities
for use with GNU Automake (in particular).

* [Searching for Libraries](#Searching-for-Libraries)
* [Controlling the Exported Symbols of Shared Libraries](#Exported-Symbols-of-Shared-Libraries)
* [LD Version Scripts](#LD-Version-Scripts)
* [configmake](#configmake)
* [warnings](#warnings)
* [manywarnings](#manywarnings)
* [Running self-tests under valgrind](#Running-self_002dtests-under-valgrind)
* [Package version management](#Package-version-management)
* [VCS To ChangeLog](#VCS-To-ChangeLog)
* [gitlog-to-changelog](#gitlog_002dto_002dchangelog)
* [maintainer-makefile](#maintainer_002dmakefile)

---

Next: [Controlling the Exported Symbols of Shared Libraries](#Exported-Symbols-of-Shared-Libraries), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.1 Searching for Libraries [¶](#Searching-for-Libraries-1)

The following macros check for the presence or location of certain C, C++, or
Fortran library archive files.

* [Simple Library Tests](#Simple-Library-Tests)
* [Locating Libraries](#Locating-Libraries)

#### Simple Library Tests [¶](#Simple-Library-Tests)

The macros `AC_CHECK_LIB`, `AC_SEARCH_LIBS` from GNU Autoconf check
for the presence of certain C, C++, or Fortran library archive files.
The libraries are looked up in the default linker path—a system dependent
list of directories, that usually contains the `/usr/lib` directory—and
those directories given by `-L` options in the `LDFLAGS`
variable.

#### Locating Libraries [¶](#Locating-Libraries)

The following macros, defined in the Gnulib module `havelib`, search for
the location of certain C, C++, or Fortran library archive files and make the
found location available to the compilation process and to further Autoconf
tests.

Macro: **`AC_LIB_LINKFLAGS(name, [dependencies])`** [¶](#index-AC_005fLIB_005fLINKFLAGS_0028name_002c-_005bdependencies_005d_0029)
:   Searches for `lib<name>` and the libraries corresponding to
    explicit and implicit dependencies. Sets and AC\_SUBSTs the
    `LIB<NAME>` and `LTLIB<NAME>` variables (with
    `<NAME>` in upper case) and augments the `CPPFLAGS` variable
    by `-I` options.

    This macro should be used when `lib<name>` is expected to be found.

Macro: **`AC_LIB_HAVE_LINKFLAGS(name, [dependencies], [includes], [testcode], [missing-message])`** [¶](#index-AC_005fLIB_005fHAVE_005fLINKFLAGS_0028name_002c-_005bdependencies_005d_002c-_005bincludes_005d_002c-_005btestcode_005d_002c-_005bmissing_002dmessage_005d_0029)
:   Searches for `lib<name>` and the libraries corresponding to
    explicit and implicit dependencies, together with the specified include files
    and the ability to compile and link the specified testcode. The
    missing-message defaults to `no` and may contain additional hints
    for the user. If found, it sets and AC\_SUBSTs `HAVE_LIB<NAME>=yes`
    and the `LIB<NAME>` and `LTLIB<NAME>` variables (with
    `<NAME>` in upper case) and augments the `CPPFLAGS` variable
    by `-I` options, and #defines `HAVE_LIB<NAME>` to 1.
    Otherwise, it sets and AC\_SUBSTs `HAVE_LIB<NAME>=no` and
    `LIB<NAME>` and `LTLIB<NAME>` to empty.

These macros assume that when a library is installed in
`some_directory/lib`, its include files are installed in
`some_directory/include`.

The complexities that `AC_LIB_LINKFLAGS` and `AC_LIB_HAVE_LINKFLAGS`
deal with are the following:

* The library is not necessarily already in the search path (`CPPFLAGS` for
  the include file search path, `LDFLAGS` for the library search path).
  The macro provides a ‘`--with-lib<name>`’ option. The user of the
  ‘`configure`’ script can use this option to indicate the location of the
  library and its include files. If not provided, the `--prefix` directory
  is searched as well.
* The library is not necessarily already in the run time library search path.
  To avoid the need for setting an environment variable like
  `LD_LIBRARY_PATH`, the macro adds the appropriate run time search path
  options to the `LIB<NAME>` variable. This works on most systems.
  It can also be inhibited: The user of ‘`configure`’ can use the
  `--disable-rpath` option to force an installation that doesn’t contain
  hardcoded library search paths but instead may require the use of an
  environment variable like `LD_LIBRARY_PATH`.

The macros also set a variable `LTLIB<NAME>`, that should be used
when linking with libtool. Both `LTLIB<NAME>` and
`LIB<NAME>` contain essentially the same option, but where
`LIB<NAME>` contains platform dependent flags like
‘`-Wl,-rpath`’, `LTLIB<NAME>` contains platform independent
flags like ‘`-R`’.

If you, by mistake, use `LIB<NAME>` instead of
`LTLIB<NAME>` when linking with libtool, you will observe that the
binaries created in the build dir will prefer the shared libraries in the
installation directories over the shared libraries in the build dir; this can
lead to all sorts of build failures, test failures, or crashes!

If you, on the other hand, by mistake, use `LTLIB<NAME>` instead of
`LIB<NAME>` when linking *without* libtool, you will observe
build failures, because the ‘`-R`’ options contained in
`LTLIB<NAME>` are not valid options to compilers such as GCC.

* [Example of using `AC_LIB_LINKFLAGS`](#Example-of-using-AC_005fLIB_005fLINKFLAGS)
* [Dependencies](#Dependencies)
* [Static vs. shared](#Static-vs_002e-shared)
* [`CPPFLAGS` vs. `LDFLAGS`](#CPPFLAGS-vs_002e-LDFLAGS)
* [Bi-arch systems](#Bi_002darch-systems)

#### Example of using `AC_LIB_LINKFLAGS` [¶](#Example-of-using-AC_005fLIB_005fLINKFLAGS)

Suppose you want to use `libz`, the compression library.

1. In configure.ac you add the line

   ```
     AC_CONFIG_AUX_DIR([build-aux])
     AC_LIB_LINKFLAGS([z])
   ```

   Note that since the `AC_LIB_LINKFLAGS` invocation modifies the CPPFLAGS,
   it should precede all tests that check for header files, declarations,
   structures or types.
2. To the package’s `build-aux` directory you add the file
   `config.rpath`, also part of the Gnulib `havelib` module.
   (`gnulib-tool` will usually do this for you automatically.)
3. In `Makefile.in` you add `@LIBZ@` to the link command line of
   your program. Or, if you are using Automake, you add `$(LIBZ)` to the
   `LDADD` variable that corresponds to your program.

#### Dependencies [¶](#Dependencies)

The dependencies list is a space separated list of library names that
`libname` is known to depend upon. Example: If `libfooy`
depends on `libfoox`, and `libfooz` depends on `libfoox` and
`libfooy`, you can write:

```
AC_LIB_LINKFLAGS([foox])
AC_LIB_LINKFLAGS([fooy], [foox])
AC_LIB_LINKFLAGS([fooz], [foox fooy])
```

Explicit dependencies are necessary if you cannot assume that a `.la`
file, created by libtool, is installed. If you can assume that
`libfooy.la` is installed by libtool (and has not been omitted by the
package distributor!), you can omit the explicit dependency and just write

```
AC_LIB_LINKFLAGS([fooy])
```

This way, you don’t need to know in advance which libraries the needed
library depends upon.

#### Static vs. shared [¶](#Static-vs_002e-shared)

The macros find the libraries regardless whether they are installed as
shared or static libraries.

#### `CPPFLAGS` vs. `LDFLAGS` [¶](#CPPFLAGS-vs_002e-LDFLAGS)

The macros determine the directories that should be added to the compiler
preprocessor’s search path and to the linker’s search path. For the
compiler preprocessor, `-I` options with the necessary directories are
added to the `CPPFLAGS` variable, for use by the whole package. For
the linker, appropriate options are added to the `LIB<NAME>` and
`LTLIB<NAME>` variables, for use during linking by those programs
and libraries that need the dependency on `lib<name>`. You need
to use the value of `LIB<NAME>` or `LTLIB<NAME>` in the
Makefiles. `LTLIB<NAME>` is for use with libtool, whereas
`LIB<NAME>` is for when libtool is not involved in linking.

The macros do not check whether the include files and the library found
match. If you want to verify this at configure time, one technique is
to have a version number in the include files and a version number in the
library, like this:

```
  #define LIBNAME_VERSION 10203
  extern int libname_version; /* initialized to LIBNAME_VERSION */
```

and use a test like

```
  AC_TRY_RUN([int main () { return libname_version != LIBNAME_VERSION; }])
```

#### Bi-arch systems [¶](#Bi_002darch-systems)

A bi-arch system is one where

* the processor has a 32-bit execution mode and a 64-bit execution mode
  (for example, x86\_64, ia64, sparc64, powerpc64), and
* 32-bit mode libraries and executables and 64-bit mode libraries are both
  installed, and
* 32-bit mode libraries and object files cannot be mixed with 64-bit mode
  ones.

On several types of such systems, for historical reasons, the 32-bit libraries
are installed in `prefix/lib`, whereas the 64-bit libraries are
installed in

* `prefix/lib64` on many glibc systems,
* `prefix/lib/64` on Solaris systems.

On such systems, in 64-bit mode, `configure` will search for the
libraries in `prefix/lib64` or `prefix/lib/64`,
respectively, not in `prefix/lib`. A user can adhere to these
system-wide conventions by using the ‘`--libdir`’ option when installing
packages. When a user has already installed packages in 64-bit mode using
the GNU default ‘`--libdir=prefix/lib`’, he can make this directory
adhere to the system-wide convention by placing a symbolic link:

On glibc systems:
:   `ln -s lib prefix/lib64`

On Solaris systems:
:   `ln -s . prefix/lib/64`

---

Next: [LD Version Scripts](#LD-Version-Scripts), Previous: [Searching for Libraries](#Searching-for-Libraries), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.2 Controlling the Exported Symbols of Shared Libraries [¶](#Controlling-the-Exported-Symbols-of-Shared-Libraries)

The `lib-symbol-visibility` module allows precise control of the
symbols exported by a shared library. This is useful because

* It prevents abuse of undocumented APIs of your library. Symbols that
  are not exported from the library cannot be used. This eliminates the
  problem that when the maintainer of the library changes internals of the
  library, maintainers of other projects cry “breakage”. Instead, these
  maintainers are forced to negotiate the desired API from the maintainer
  of the library.
* It reduces the risk of symbol collision between your library and other
  libraries. For example, the symbol ‘`readline`’ is defined in several
  libraries, most of which don’t have the same semantics and the same calling
  convention as the GNU readline library.
* It reduces the startup time of programs linked to the library. This is
  because the dynamic loader has less symbols to process.
* It allows the compiler to generate better code. Within a shared library,
  a call to a function that is a global symbol costs a “call” instruction
  to a code location in the so-called PLT (procedure linkage table) which
  contains a “jump” instruction to the actual function’s code. (This is
  needed so that the function can be overridden, for example by a function
  with the same name in the executable or in a shared library interposed
  with `LD_PRELOAD`.) Whereas a call to a function for which the compiler
  can assume that it is in the same shared library is just a direct “call”
  instructions. Similarly for variables: A reference to a global variable
  fetches a pointer in the so-called GOT (global offset table); this is a
  pointer to the variable’s memory. So the code to access it is two memory
  load instructions. Whereas for a variable which is known to reside in the
  same shared library, it is just a direct memory access: one memory load
  instruction.

There are traditionally three ways to specify the exported symbols of a
shared library.

* The programmer specifies the list of symbols to be exported when the
  shared library is created. Usually a command-line option is passed
  to the linker, with the name of a file containing the symbols.

  The upside of this approach is flexibility: it allows the same code to
  be used in different libraries with different export lists. The downsides
  are: 1. it’s a lot of maintenance overhead when the symbol list is platform
  dependent, 2. it doesn’t work well with C++, due to name mangling.
* The programmer specifies a “hidden” attribute for every variable and
  function that shall not be exported.

  The drawbacks of this approach are: Symbols are still exported from
  the library by default. It’s a lot of maintenance work to mark every non-
  exported variable and function. But usually the exported API is quite small,
  compared to the internal API of the library. And it’s the wrong paradigm:
  It doesn’t force thinking when introducing new exported API.
* The programmer specifies a “hidden” attribute for all files that make up
  the shared library, and an “exported” attribute for those symbols in these
  files that shall be exported.

  This is perfect: It burdens the maintainer only for exported API, not
  for library-internal API. And it keeps the annotations in the source code.

GNU libtool’s `-export-symbols` option implements the first approach.
The script `declared.sh` from Gnulib can help to produce the list of
symbols.

This gnulib module implements the third approach. For this it relies on
GNU GCC 4.0 or newer, namely on its ‘`-fvisibility=hidden`’ command-line
option and the “visibility” attribute. (The “visibility” attribute
was already supported in GCC 3.4, but without the command line option,
introduced in GCC 4.0, the third approach could not be used.)

More explanations on this subject can be found in
<https://gcc.gnu.org/wiki/Visibility>, which contains more details
on the GCC features and additional advice for C++ libraries, and in
Ulrich Drepper’s paper <https://www.akkadia.org/drepper/dsohowto.pdf>,
which also explains other tricks for reducing the startup time impact
of shared libraries.

The gnulib autoconf macro `gl_VISIBILITY` tests for GCC 4.0 or newer.
It defines a Makefile variable `@CFLAG_VISIBILITY@` containing
‘`-fvisibility=hidden`’ or nothing. It also defines as a C macro and
as a substituted variable: @HAVE\_VISIBILITY@. Its value is 1 when symbol
visibility control is supported, and 0 otherwise.

As of 2022, symbol visibility control is supported on

* ELF platforms (glibc, Linux, \*BSD, Solaris) with GCC or clang,
* macOS,
* AIX with gcc or xlclang.

It is not supported on

* Other compilers on ELF platforms or AIX,
* Windows.

To use this module in a library, say libfoo, you will do these steps:

1. Add `@CFLAG_VISIBILITY@` or (in a Makefile.am)
   `$(CFLAG_VISIBILITY)` to the CFLAGS for the compilation of the sources
   that make up the library.
2. Add a C macro definition, say ‘`-DBUILDING_LIBFOO`’, to the CPPFLAGS
   for the compilation of the sources that make up the library.
3. Define a macro specific to your library like this.

   ```
   #if HAVE_VISIBILITY && BUILDING_LIBFOO
   # define LIBFOO_SHLIB_EXPORTED __attribute__((__visibility__("default")))
   #else
   # define LIBFOO_SHLIB_EXPORTED
   #endif
   ```

   This macro should be enabled in all public header files of your library.
4. Annotate all variable, function and class declarations in all public header
   files of your library with ‘`LIBFOO_SHLIB_EXPORTED`’. This annotation
   can occur at different locations: between the ‘`extern`’ and the
   type or return type, or just before the entity being declared, or after
   the entire declarator. My preference is to put it right after ‘`extern`’,
   so that the declarations in the header files remain halfway readable.

Note that the precise control of the exported symbols will not work with
other compilers than GCC >= 4.0, and will not work on systems where the
assembler or linker lack the support of “hidden” visibility. Therefore,
it’s good if, in order to reduce the risk of collisions with symbols in
other libraries, you continue to use a prefix specific to your library
for all non-static variables and functions and for all C++ classes in
your library.

Note about other compilers: MSVC support can be added easily, by extending
the definition of the macro mentioned above, to something like this:

```
#if HAVE_VISIBILITY && BUILDING_LIBFOO
# define LIBFOO_SHLIB_EXPORTED __attribute__((__visibility__("default")))
#elif (defined _WIN32 && !defined __CYGWIN__) && @BUILDING_SHARED@ && BUILDING_LIBFOO
# if defined DLL_EXPORT
#  define LIBFOO_SHLIB_EXPORTED __declspec(dllexport)
# else
#  define LIBFOO_SHLIB_EXPORTED
# endif
#elif (defined _WIN32 && !defined __CYGWIN__) && @BUILDING_SHARED@
# define LIBFOO_SHLIB_EXPORTED __declspec(dllimport)
#else
# define LIBFOO_SHLIB_EXPORTED
#endif
```

Here `BUILDING_SHARED` is an Autoconf variable that you have to define.
It ought to evaluate to 1 in a build configured with ‘`--enable-shared`’,
or to 0 in a build configured with ‘`--disable-shared`’.
You may use the following ‘`configure.ac`’ snippet:

```
  if test "$enable_shared" = yes; then
    BUILDING_SHARED=1
  else
    BUILDING_SHARED=0
  fi
  AC_SUBST([BUILDING_SHARED])
```

And `DLL_EXPORT` is defined by Libtool, on Windows platforms, when
compiling for a shared library (called DLL under Windows).
It is not defined when Libtool compiles an object file meant to be linked
statically into some executable.

---

Next: [configmake](#configmake), Previous: [Controlling the Exported Symbols of Shared Libraries](#Exported-Symbols-of-Shared-Libraries), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]

### 20.3 LD Version Scripts [¶](#LD-Version-Scripts-1)

The `lib-symbol-versions` module can be used to add shared
library versioning support. Currently, only GNU LD and the Solaris
linker supports this.

Version scripts provides information that can be used by GNU/Linux
distribution packaging tools. For example, Debian has a tool
`dpkg-shlibdeps` that can determine the minimal required version
of each dependency (by looking at the symbol list) and stuff the
information into the Debian specific packaging files.

For more information and other uses of version scripts, see Ulrich
Drepper’s paper <https://www.akkadia.org/drepper/dsohowto.pdf>

You use the module by importing it to your library, and then add the
following lines to the `Makefile.am` that builds the library:

```
if HAVE_LD_VERSION_SCRIPT
libfoo_la_LDFLAGS += -Wl,--version-script=$(srcdir)/libfoo.map
endif
```

The version script file format is documented in the GNU LD manual, but
a small example would be:

```
LIBFOO_1.0 {
  global:
    libfoo_init; libfoo_doit; libfoo_done;

  local:
    *;
};
```

If you target platforms that do not support linker scripts (i.e., all
platforms that doesn’t use GNU LD) you may want to consider a more
portable but less powerful alternative: libtool
`-export-symbols`. It will hide internal symbols from your
library, but will not add ELF versioning symbols. Your usage would
then be something like:

```
if HAVE_LD_VERSION_SCRIPT
libfoo_la_LDFLAGS += -Wl,--version-script=$(srcdir)/libfoo.map
else
libfoo_la_LDFLAGS += -export-symbols $(srcdir)/libfoo.sym
endif
```

See the Libtool manual for the file syntax, but a small example would
be:

```
libfoo_init
libfoo_doit
libfoo_done
```

To avoid the need for a `*.sym` file if your symbols are easily
expressed using a regular expression, you may use
`-export-symbols-regex`:

```
if HAVE_LD_VERSION_SCRIPT
libfoo_la_LDFLAGS += -Wl,--version-script=$(srcdir)/libfoo.map
else
libfoo_la_LDFLAGS += -export-symbols-regex '^libfoo_.*'
endif
```

For more discussions about symbol visibility, rather than shared
library versioning, see the `lib-symbol-visibility` module
(see [Controlling the Exported Symbols of Shared Libraries](#Exported-Symbols-of-Shared-Libraries)).

---

Next: [warnings](#warnings), Previous: [LD Version Scripts](#LD-Version-Scripts), Up: [Build Infrastructure Modules](#Build-Infrastructure-Modules)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Module-Index "Index")]
