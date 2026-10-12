### 4.1 String and Character Basics [¶](#String-and-Character-Basics)

A character is a Lisp object which represents a single character of
text. In Emacs Lisp, characters are simply integers; whether an
integer is a character or not is determined only by how it is used.
See [Character Codes](#Character-Codes), for details about character representation in
Emacs.

A string is a fixed sequence of characters. It is a type of
sequence called an *array*, meaning that its length is fixed and
cannot be altered once it is created (see [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors)). Unlike in C, Emacs Lisp strings are *not* terminated
by a distinguished character code.

Since strings are arrays, and therefore sequences as well, you can
operate on them with the general array and sequence functions documented
in [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors). For example, you can access
individual characters in a string using the function `aref`
(see [Functions that Operate on Arrays](#Array-Functions)).

There are two text representations for non-ASCII
characters in Emacs strings (and in buffers): unibyte and multibyte.
For most Lisp programming, you don’t need to be concerned with these
two representations. See [Text Representations](#Text-Representations), for details.

Sometimes key sequences are represented as unibyte strings. When a
unibyte string is a key sequence, string elements in the range 128 to
255 represent meta characters (which are large integers) rather than
character codes in the range 128 to 255. Strings cannot hold
characters that have the hyper, super or alt modifiers; they can hold
ASCII control characters, but no other control characters.
They do not distinguish case in ASCII control characters.
If you want to store such characters in a sequence, such as a key
sequence, you must use a vector instead of a string. See [Character Type](#Character-Type), for more information about keyboard input characters.

Strings are useful for holding regular expressions. You can also
match regular expressions against strings with `string-match`
(see [Regular Expression Searching](#Regexp-Search)). The functions `match-string`
(see [Simple Match Data Access](#Simple-Match-Data)) and `replace-match` (see [Replacing the Text that Matched](#Replacing-Match)) are useful for decomposing and modifying strings after
matching regular expressions against them.

Like a buffer, a string can contain text properties for the characters
in it, as well as the characters themselves. See [Text Properties](#Text-Properties).
All the Lisp primitives that copy text from strings to buffers or other
strings also copy the properties of the characters being copied.

See [Text](#Text), for information about functions that display strings or
copy them into buffers. See [Character Type](#Character-Type), and [String Type](#String-Type),
for information about the syntax of characters and strings.
See [Non-ASCII Characters](#Non_002dASCII-Characters), for functions to convert between text
representations and to encode and decode character codes.
Also, note that `length` should *not* be used for computing
the width of a string on display; use `string-width` (see [Size of Displayed Text](#Size-of-Displayed-Text)) instead.

---

Next: [Creating Strings](#Creating-Strings), Previous: [String and Character Basics](#String-Basics), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.2 Predicates for Strings [¶](#Predicates-for-Strings-1)

For more information about general sequence and array predicates,
see [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors), and [Arrays](#Arrays).

Function: **stringp** object [¶](#index-stringp)
:   This function returns `t` if object is a string, `nil`
    otherwise.

Function: **string-or-null-p** object [¶](#index-string_002dor_002dnull_002dp)
:   This function returns `t` if object is a string or
    `nil`. It returns `nil` otherwise.

Function: **char-or-string-p** object [¶](#index-char_002dor_002dstring_002dp)
:   This function returns `t` if object is a string or a
    character (i.e., an integer), `nil` otherwise.

---

Next: [Modifying Strings](#Modifying-Strings), Previous: [Predicates for Strings](#Predicates-for-Strings), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.3 Creating Strings [¶](#Creating-Strings-1)

The following functions create strings, either from scratch, or by
putting strings together, or by taking them apart. (For functions
that create strings based on the modified contents of other strings,
like `string-replace` and `replace-regexp-in-string`, see
[Search and Replace](#Search-and-Replace).)

Function: **make-string** count character &optional multibyte [¶](#index-make_002dstring)
:   This function returns a string made up of count repetitions of
    character. If count is negative, an error is signaled.

    ```
    (make-string 5 ?x)
         ⇒ "xxxxx"
    (make-string 0 ?x)
         ⇒ ""
    ```

    Normally, if character is an ASCII character, the
    result is a unibyte string. But if the optional argument
    multibyte is non-`nil`, the function will produce a
    multibyte string instead. This is useful when you later need to
    concatenate the result with non-ASCII strings or replace
    some of its characters with non-ASCII characters.

    Other functions to compare with this one include `make-vector`
    (see [Vectors](#Vectors)) and `make-list` (see [Building Cons Cells and Lists](#Building-Lists)).

Function: **string** &rest characters [¶](#index-string)
:   This returns a string containing the characters characters.

    ```
    (string ?a ?b ?c)
         ⇒ "abc"
    ```

Function: **substring** string &optional start end [¶](#index-substring)
:   This function returns a new string which consists of those characters
    from string in the range from (and including) the character at the
    index start up to (but excluding) the character at the index
    end. The first character is at index zero. With one argument,
    this function just copies string.

    ```
    (substring "abcdefg" 0 3)
         ⇒ "abc"
    ```

    In the above example, the index for ‘`a`’ is 0, the index for
    ‘`b`’ is 1, and the index for ‘`c`’ is 2. The index 3—which
    is the fourth character in the string—marks the character position
    up to which the substring is copied. Thus, ‘`abc`’ is copied from
    the string `"abcdefg"`.

    A negative number counts from the end of the string, so that −1
    signifies the index of the last character of the string. For example:

    ```
    (substring "abcdefg" -3 -1)
         ⇒ "ef"
    ```

    In this example, the index for ‘`e`’ is −3, the index for
    ‘`f`’ is −2, and the index for ‘`g`’ is −1.
    Therefore, ‘`e`’ and ‘`f`’ are included, and ‘`g`’ is excluded.

    When `nil` is used for end, it stands for the length of the
    string. Thus,

    ```
    (substring "abcdefg" -3 nil)
         ⇒ "efg"
    ```

    Omitting the argument end is equivalent to specifying `nil`.
    It follows that `(substring string 0)` returns a copy of all
    of string.

    ```
    (substring "abcdefg" 0)
         ⇒ "abcdefg"
    ```

    But we recommend `copy-sequence` for this purpose (see [Sequences](#Sequence-Functions)).

    If the characters copied from string have text properties, the
    properties are copied into the new string also. See [Text Properties](#Text-Properties).

    `substring` also accepts a vector for the first argument.
    For example:

    ```
    (substring [a b (c) "d"] 1 3)
         ⇒ [b (c)]
    ```

    A `wrong-type-argument` error is signaled if start is not
    an integer or if end is neither an integer nor `nil`. An
    `args-out-of-range` error is signaled if start indicates a
    character following end, or if either integer is out of range
    for string.

    Contrast this function with `buffer-substring` (see [Examining Buffer Contents](#Buffer-Contents)), which returns a string containing a portion of the text in
    the current buffer. The beginning of a string is at index 0, but the
    beginning of a buffer is at index 1.

Function: **substring-no-properties** string &optional start end [¶](#index-substring_002dno_002dproperties)
:   This works like `substring` but discards all text properties from
    the value. Also, start may be omitted or `nil`, which is
    equivalent to 0. Thus, `(substring-no-properties string)` returns a copy of string, with all text
    properties removed.

Function: **concat** &rest sequences [¶](#index-concat)
:   This function returns a string consisting of the characters in the
    arguments passed to it (along with their text properties, if any). The
    arguments may be strings, lists of numbers, or vectors of numbers; they
    are not themselves changed. If `concat` receives no arguments, it
    returns an empty string.

    ```
    (concat "abc" "-def")
         ⇒ "abc-def"
    (concat "abc" (list 120 121) [122])
         ⇒ "abcxyz"
    ;; nil is an empty sequence.
    (concat "abc" nil "-def")
         ⇒ "abc-def"
    (concat "The " "quick brown " "fox.")
         ⇒ "The quick brown fox."
    (concat)
         ⇒ ""
    ```

    This function does not always allocate a new string. Callers are
    advised not rely on the result being a new string nor on it being
    `eq` to an existing string.

    In particular, mutating the returned value may inadvertently change
    another string, alter a constant string in the program, or even raise
    an error. To obtain a string that you can safely mutate, use
    `copy-sequence` on the result.

    If you need to create a string made from n copies of a given
    source string source, you can use `concat` as follows:

    ```
      (apply #'concat (make-list n source))
    ```

    This uses the fact that `concat` can take any kind of sequence as
    its arguments.

    For information about other concatenation functions, see the
    description of `mapconcat` in [Mapping Functions](#Mapping-Functions),
    `vconcat` in [Functions for Vectors](#Vector-Functions), and `append` in [Building Cons Cells and Lists](#Building-Lists). For concatenating individual command-line arguments into a
    string to be used as a shell command, see [combine-and-quote-strings](#Shell-Arguments).

Function: **split-string** string &optional separators omit-empty trim [¶](#index-split_002dstring)
:   This function splits string into substrings based on the regular
    expression separators (see [Regular Expressions](#Regular-Expressions)). Each match
    for separators defines a splitting point; the substrings between
    splitting points are made into a list, which is returned.

    If separators is `nil` (or omitted), the default is the
    value of `split-string-default-separators` and the function
    behaves as if omit-empty were `t`.

    If omit-empty is `nil` (or omitted), the result contains
    empty strings whenever there are two consecutive matches for
    separators, or a match is adjacent to the beginning or end of
    string. If omit-empty is `t`, these empty strings are
    omitted from the result.

    If the optional argument trim is non-`nil`, it should be a
    regular expression to match text to trim from the beginning and end of
    each substring. Trimming may make the substring empty and omitted from
    the result if omit-empty is `t` as above.

    If you need to split a string into a list of individual command-line
    arguments suitable for `call-process` or `start-process`,
    see [split-string-and-unquote](#Shell-Arguments).

    Do not use a value for separators that matches the empty string,
    or the results will be unpredictable. To split a string into individual
    characters, use `string-to-list` or `string-to-vector`.

    Examples:

    ```
    (split-string "  one  two   ")            ⇒ ("one" "two")
    (split-string "one::two:" ":")            ⇒ ("one" "" "two" "")
    (split-string "one::two:" ":+")           ⇒ ("one" "two" "")
    (split-string "one::two:" ":" t)          ⇒ ("one" "two")
    (split-string "one: : two : " ":" t " +") ⇒ ("one" "two")
    ```

Variable: **split-string-default-separators** [¶](#index-split_002dstring_002ddefault_002dseparators)
:   The default value of separators for `split-string`. Its
    usual value is `"[ \f\t\n\r\v]+"`.

Function: **string-clean-whitespace** string [¶](#index-string_002dclean_002dwhitespace)
:   Clean up the whitespace in string by collapsing stretches of
    whitespace to a single space character, as well as removing all
    whitespace from the start and the end of string.

Function: **string-trim-left** string &optional regexp [¶](#index-string_002dtrim_002dleft)
:   Remove the leading text that matches regexp from string.
    regexp defaults to ‘`[ \t\n\r]+`’.

Function: **string-trim-right** string &optional regexp [¶](#index-string_002dtrim_002dright)
:   Remove the trailing text that matches regexp from string.
    regexp defaults to ‘`[ \t\n\r]+`’.

Function: **string-trim** string &optional trim-left trim-right [¶](#index-string_002dtrim)
:   Remove the leading text that matches trim-left and trailing text
    that matches trim-right from string. Both regexps
    default to ‘`[ \t\n\r]+`’.

Function: **string-fill** string width [¶](#index-string_002dfill)
:   Attempt to Word-wrap string so that it displays with lines no
    wider than width. Filling is done on whitespace boundaries only.
    If there are individual words that are longer than width, these
    will not be shortened, and therefore string might be shown with
    lines wider than width in that case.

Function: **string-limit** string length &optional end coding-system [¶](#index-string_002dlimit)
:   If string is shorter than length characters, string
    is returned as is. Otherwise, return a substring of string
    consisting of the first length characters. If the optional
    end parameter is given, return a string of the length last
    characters instead.

    If coding-system is non-`nil`, string will be encoded
    before limiting, and the result will be a unibyte string that’s
    shorter than `length` bytes. If string contains characters
    that are encoded into several bytes (for instance, when using
    `utf-8`), the resulting unibyte string is never truncated in the
    middle of a character representation.

    This function measures the string length in characters or bytes, and
    thus is generally inappropriate if you need to shorten strings for
    display purposes; use `truncate-string-to-width` or
    `window-text-pixel-size` or `string-glyph-split` instead
    (see [Size of Displayed Text](#Size-of-Displayed-Text)).

Function: **string-lines** string &optional omit-empty keep-newlines [¶](#index-string_002dlines)
:   Split string into a list of strings on newline boundaries. If
    the optional argument omit-empty is non-`nil`, remove empty
    lines from the results. If the optional argument keep-newlines
    is non-`nil`, don’t remove the trailing newlines from the result
    strings.

Function: **string-pad** string length &optional padding start [¶](#index-string_002dpad)
:   Pad string to be of the given length using padding
    as the padding character. padding defaults to the space
    character. If string is longer than length, no padding is
    done. If start is `nil` or omitted, the padding is
    appended to the characters of string, and if it’s
    non-`nil`, the padding is prepended to string’s characters.

Function: **string-chop-newline** string [¶](#index-string_002dchop_002dnewline)
:   Remove the final newline, if any, from string.

---

Next: [Comparison of Characters and Strings](#Text-Comparison), Previous: [Creating Strings](#Creating-Strings), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.4 Modifying Strings [¶](#Modifying-Strings-1)

You can alter the contents of a mutable string via operations
described in this section. See [Mutability](#Mutability).

The most basic way to alter the contents of an existing string is with
`aset` (see [Functions that Operate on Arrays](#Array-Functions)). `(aset string idx char)` stores char into string at character
index idx. When string is a unibyte string (see [Text Representations](#Text-Representations)), char must be a single byte (0–255); when
string is multibyte, both char and the previous character at
idx must be ASCII (0–127).

To clear out a string that contained a password, use
`clear-string`:

Function: **clear-string** string [¶](#index-clear_002dstring)
:   This makes string a unibyte string, clears its contents to null
    characters, and removes all text properties. It may also change
    string’s length.

To modify the text properties of a string, use the functions described
in [Changing Text Properties](#Changing-Properties).

---

Next: [Conversion of Characters and Strings](#String-Conversion), Previous: [Modifying Strings](#Modifying-Strings), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.5 Comparison of Characters and Strings [¶](#Comparison-of-Characters-and-Strings)

Function: **char-equal** character1 character2 [¶](#index-char_002dequal)
:   This function returns `t` if the arguments represent the same
    character, `nil` otherwise. This function ignores differences
    in case if `case-fold-search` is non-`nil`.

    ```
    (char-equal ?x ?x)
         ⇒ t
    (let ((case-fold-search nil))
      (char-equal ?x ?X))
         ⇒ nil
    ```

Function: **string-equal** string1 string2 [¶](#index-string_002dequal)
:   This function returns `t` if the characters of the two strings
    match exactly. Symbols are also allowed as arguments, in which case
    the symbol names are used. Case is always significant, regardless of
    `case-fold-search`.

    This function is equivalent to `equal` for comparing two strings
    (see [Equality Predicates](#Equality-Predicates)). In particular, the text properties of
    the two strings are ignored; use `equal-including-properties` if
    you need to distinguish between strings that differ only in their text
    properties. However, unlike `equal`, if either argument is not a
    string or symbol, `string-equal` signals an error.

    ```
    (string-equal "abc" "abc")
         ⇒ t
    (string-equal "abc" "ABC")
         ⇒ nil
    (string-equal "ab" "ABC")
         ⇒ nil
    ```

    A unibyte and a multibyte string are equal in the sense of
    `string-equal` if and only if they contain the same sequence of
    character codes all being in the range 0–127 (ASCII).
    See [Text Representations](#Text-Representations).

Function: **string=** string1 string2 [¶](#index-string_003d)
:   `string=` is another name for `string-equal`.

Function: **string-equal-ignore-case** string1 string2 [¶](#index-string_002dequal_002dignore_002dcase)
:   `string-equal-ignore-case` compares strings ignoring case
    differences, like `char-equal` when `case-fold-search` is
    `t`.

Function: **string-collate-equalp** string1 string2 &optional locale ignore-case [¶](#index-string_002dcollate_002dequalp)
:   This function returns `t` if string1 and string2 are
    equal with respect to the collation rules of the specified
    locale, which defaults to your current system locale. A
    collation rule is not only
    determined by the lexicographic order of the characters contained in
    string1 and string2, but also by further rules about
    relations between these characters. Usually, it is defined by the
    locale environment with which Emacs is running and by the Standard C
    library against which Emacs was linked[4](#FOOT4).

    For example, characters with different code points but the same
    meaning, like different grave accent Unicode characters, might, in
    some locales, be considered as equal:

    ```
    (string-collate-equalp (string ?\uFF40) (string ?\u1FEF))
         ⇒ t
    ```

    The optional argument locale, a string, overrides the setting of
    your current locale identifier for collation. The value is system
    dependent; a locale `"en_US.UTF-8"` is applicable on POSIX
    systems, while it would be, e.g., `"enu_USA.1252"` on MS-Windows
    systems.

    If ignore-case is non-`nil`, characters are compared
    case-insensitively, by converting them to lower-case. However, if the
    underlying system library doesn’t provide locale-specific collation
    rules, this function falls back to `string-equal`, in which case
    the ignore-case argument is ignored, and the comparison will
    always be case-sensitive.

    To emulate Unicode-compliant collation on MS-Windows systems,
    bind `w32-collate-ignore-punctuation` to a non-`nil` value, since
    the codeset part of the locale cannot be `"UTF-8"` on MS-Windows.

    If your system does not support a locale environment, this function
    behaves like `string-equal`.

    Do *not* use this function to compare file names for equality, as
    filesystems generally don’t honor linguistic equivalence of strings
    that collation implements.

Function: **string-lessp** string1 string2 [¶](#index-string_002dlessp)
:   This function compares two strings a character at a time. It
    scans both the strings at the same time to find the first pair of corresponding
    characters that do not match. If the lesser character of these two is
    the character from string1, then string1 is less, and this
    function returns `t`. If the lesser character is the one from
    string2, then string1 is greater, and this function returns
    `nil`. If the two strings match entirely, the value is `nil`.

    Pairs of characters are compared according to their character codes.
    Keep in mind that lower case letters have higher numeric values in the
    ASCII character set than their upper case counterparts; digits and
    many punctuation characters have a lower numeric value than upper case
    letters. An ASCII character is less than any non-ASCII
    character. However, as unibyte and multibyte strings only have the
    ASCII character set in common, it is usually not meaningful to
    compare other characters between those two types of strings
    (see [Text Representations](#Text-Representations)).

    ```
    (string-lessp "abc" "abd")
         ⇒ t
    (string-lessp "abd" "abc")
         ⇒ nil
    (string-lessp "123" "abc")
         ⇒ t
    ```

    When the strings have different lengths, and they match up to the
    length of string1, then the result is `t`. If they match up
    to the length of string2, the result is `nil`. A string of
    no characters is less than any other string.

    ```
    (string-lessp "" "abc")
         ⇒ t
    (string-lessp "ab" "abc")
         ⇒ t
    (string-lessp "abc" "")
         ⇒ nil
    (string-lessp "abc" "ab")
         ⇒ nil
    (string-lessp "" "")
         ⇒ nil
    ```

    Symbols are also allowed as arguments, in which case their print names
    are compared.

Function: **string<** string1 string2 [¶](#index-string_003c)
:   `string<` is another name for `string-lessp`.

Function: **string-greaterp** string1 string2 [¶](#index-string_002dgreaterp)
:   This function returns the result of comparing string1 and
    string2 in the opposite order, i.e., it is equivalent to calling
    `(string-lessp string2 string1)`.

Function: **string>** string1 string2 [¶](#index-string_003e)
:   `string>` is another name for `string-greaterp`.

Function: **string-collate-lessp** string1 string2 &optional locale ignore-case [¶](#index-string_002dcollate_002dlessp)
:   This function returns `t` if string1 is less than
    string2 in collation order of the specified locale, which
    defaults to your current system locale. A collation order is not only
    determined by the lexicographic order of the characters contained in
    string1 and string2, but also by further rules about
    relations between these characters. Usually, it is defined by the
    locale environment with which Emacs is running, and by the Standard C
    library against which Emacs was linked.

    For example, punctuation and whitespace characters might be ignored
    for sorting (see [Sequences](#Sequence-Functions)):

    ```
    (sort '("11" "12" "1 1" "1 2" "1.1" "1.2")
           :lessp #'string-collate-lessp)
         ⇒ ("11" "1 1" "1.1" "12" "1 2" "1.2")
    ```

    This behavior is system-dependent; e.g., punctuation and whitespace
    are never ignored on Cygwin, regardless of locale.

    The optional argument locale, a string, overrides the setting of
    your current locale identifier for collation. The value is system
    dependent; a locale `"en_US.UTF-8"` is applicable on POSIX
    systems, while it would be, e.g., `"enu_USA.1252"` on MS-Windows
    systems. The locale value of `"POSIX"` or `"C"` lets
    `string-collate-lessp` behave like `string-lessp`:

    ```
    (sort '("11" "12" "1 1" "1 2" "1.1" "1.2")
          :lessp (lambda (s1 s2) (string-collate-lessp s1 s2 "POSIX")))
         ⇒ ("1 1" "1 2" "1.1" "1.2" "11" "12")
    ```

    If ignore-case is non-`nil`, characters are compared
    case-insensitively, by converting them to lower-case. However, if the
    underlying system library doesn’t provide locale-specific collation
    rules, this function falls back to `string-lessp`, in which case
    the ignore-case argument is ignored, and the comparison will
    always be case-sensitive.

    To emulate Unicode-compliant collation on MS-Windows systems,
    bind `w32-collate-ignore-punctuation` to a non-`nil` value, since
    the codeset part of the locale cannot be `"UTF-8"` on MS-Windows.

    If your system does not support a locale environment, this function
    behaves like `string-lessp`.

Function: **string-version-lessp** string1 string2 [¶](#index-string_002dversion_002dlessp)
:   This function compares strings lexicographically, except it treats
    sequences of numerical characters as if they comprised a base-ten
    number, and then compares the numbers. So ‘`foo2.png`’ is
    “smaller” than ‘`foo12.png`’ according to this predicate, even if
    ‘`12`’ is lexicographically “smaller” than ‘`2`’.

Function: **string-prefix-p** string1 string2 &optional ignore-case [¶](#index-string_002dprefix_002dp)
:   This function returns non-`nil` if string1 is a prefix of
    string2; i.e., if string2 starts with string1. If
    the optional argument ignore-case is non-`nil`, the
    comparison ignores case differences.

Function: **string-suffix-p** suffix string &optional ignore-case [¶](#index-string_002dsuffix_002dp)
:   This function returns non-`nil` if suffix is a suffix of
    string; i.e., if string ends with suffix. If the
    optional argument ignore-case is non-`nil`, the comparison
    ignores case differences.

Function: **string-search** needle haystack &optional start-pos [¶](#index-string_002dsearch)
:   Return the position of the first instance of needle in
    haystack, both of which are strings. If start-pos is
    non-`nil`, start searching from that position in haystack.
    Return `nil` if no match was found.
    This function only considers the characters in the strings when doing
    the comparison; text properties are ignored. Matching is always
    case-sensitive.

Function: **compare-strings** string1 start1 end1 string2 start2 end2 &optional ignore-case [¶](#index-compare_002dstrings)
:   This function compares a specified part of string1 with a
    specified part of string2. The specified part of string1
    runs from index start1 (inclusive) up to index end1
    (exclusive); `nil` for start1 means the start of the
    string, while `nil` for end1 means the length of the
    string. Likewise, the specified part of string2 runs from index
    start2 up to index end2.

    The strings are compared by the numeric values of their characters.
    For instance, str1 is considered less than str2 if
    its first differing character has a smaller numeric value. If
    ignore-case is non-`nil`, characters are converted to
    upper-case, using the current buffer’s case-table (see [The Case Table](#Case-Tables)), before comparing them. Unibyte strings are converted to
    multibyte for comparison (see [Text Representations](#Text-Representations)), so that a
    unibyte string and its conversion to multibyte are always regarded as
    equal.

    If the specified portions of the two strings match, the value is
    `t`. Otherwise, the value is an integer which indicates how many
    leading characters agree, and which string is less. Its absolute
    value is one plus the number of characters that agree at the beginning
    of the two strings. The sign is negative if string1 (or its
    specified portion) is less.

Function: **string-distance** string1 string2 &optional bytecompare [¶](#index-string_002ddistance)
:   This function returns the *Levenshtein distance* between the
    source string string1 and the target string string2. The
    Levenshtein distance is the number of single-character
    changes—deletions, insertions, or replacements—required to
    transform the source string into the target string; it is one possible
    definition of the *edit distance* between strings.

    Letter-case of the strings is significant for the computed distance,
    but their text properties are ignored. If the optional argument
    bytecompare is non-`nil`, the function calculates the
    distance in terms of bytes instead of characters. The byte-wise
    comparison uses the internal Emacs representation of characters, so it
    will produce inaccurate results for multibyte strings that include raw
    bytes (see [Text Representations](#Text-Representations)); make the strings unibyte by
    encoding them (see [Explicit Encoding and Decoding](#Explicit-Encoding)) if you need accurate results
    with raw bytes.

Function: **assoc-string** key alist &optional case-fold [¶](#index-assoc_002dstring)
:   This function works like `assoc`, except that key must be a
    string or symbol, and comparison is done using `compare-strings`.
    Symbols are converted to strings before testing.
    If case-fold is non-`nil`, key and the elements of
    alist are converted to upper-case before comparison.
    Unlike `assoc`, this function can also match elements of the alist
    that are strings or symbols rather than conses. In particular, alist can
    be a list of strings or symbols rather than an actual alist.
    See [Association Lists](#Association-Lists).

See also the function `compare-buffer-substrings` in
[Comparing Text](#Comparing-Text), for a way to compare text in buffers. The
function `string-match`, which matches a regular expression
against a string, can be used for a kind of string comparison; see
[Regular Expression Searching](#Regexp-Search).

---

Next: [Formatting Strings](#Formatting-Strings), Previous: [Comparison of Characters and Strings](#Text-Comparison), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.6 Conversion of Characters and Strings [¶](#Conversion-of-Characters-and-Strings)

This section describes functions for converting between characters,
strings and integers. `format` (see [Formatting Strings](#Formatting-Strings)) and
`prin1-to-string` (see [Output Functions](#Output-Functions)) can also convert
Lisp objects into strings. `read-from-string` (see [Input Functions](#Input-Functions)) can convert a string representation of a Lisp object
into an object. The functions `string-to-multibyte` and
`string-to-unibyte` convert the text representation of a string
(see [Converting Text Representations](#Converting-Representations)).

See [Documentation](#Documentation), for functions that produce textual descriptions
of text characters and general input events
(`single-key-description` and `text-char-description`). These
are used primarily for making help messages.

Function: **number-to-string** number [¶](#index-number_002dto_002dstring)
:   This function returns a string consisting of the printed base-ten
    representation of number. The returned value starts with a
    minus sign if the argument is negative.

    ```
    (number-to-string 256)
         ⇒ "256"
    ```

    ```
    (number-to-string -23)
         ⇒ "-23"
    ```

    ```
    (number-to-string -23.5)
         ⇒ "-23.5"
    ```

    `int-to-string` is a semi-obsolete alias for this function.

    See also the function `format` in [Formatting Strings](#Formatting-Strings).

Function: **string-to-number** string &optional base [¶](#index-string_002dto_002dnumber)
:   This function returns the numeric value of the characters in
    string. If base is non-`nil`, it must be an integer
    between 2 and 16 (inclusive), and integers are converted in that base.
    If base is `nil`, then base ten is used. Floating-point
    conversion only works in base ten; we have not implemented other
    radices for floating-point numbers, because that would be much more
    work and does not seem useful.

    The parsing skips spaces and tabs at the beginning of string,
    then reads as much of string as it can interpret as a number in
    the given base. (On some systems it ignores other whitespace at the
    beginning, not just spaces and tabs.) If string cannot be
    interpreted as a number, this function returns 0.

    ```
    (string-to-number "256")
         ⇒ 256
    (string-to-number "25 is a perfect square.")
         ⇒ 25
    (string-to-number "X256")
         ⇒ 0
    (string-to-number "-4.5")
         ⇒ -4.5
    (string-to-number "1e5")
         ⇒ 100000.0
    ```

Function: **char-to-string** character [¶](#index-char_002dto_002dstring)
:   This function returns a new string containing one character,
    character. This function is semi-obsolete because the function
    `string` is more general. See [Creating Strings](#Creating-Strings).

Function: **string-to-char** string [¶](#index-string_002dto_002dchar)
:   This function returns the first character in string. This is
    mostly identical to `(aref string 0)`, except that it returns 0
    if the string is empty. (The value is also 0 when the first character
    of string is the null character, ASCII code 0.) This
    function may be eliminated in the future if it does not seem useful
    enough to retain.

Here are some other functions that can convert to or from a string:

`concat`
:   This function converts a vector or a list into a string.
    See [Creating Strings](#Creating-Strings).

`vconcat`
:   This function converts a string into a vector. See [Functions for Vectors](#Vector-Functions).

`append`
:   This function converts a string into a list. See [Building Cons Cells and Lists](#Building-Lists).

`byte-to-string`
:   This function converts a byte of character data into a unibyte string.
    See [Converting Text Representations](#Converting-Representations).

---

Next: [Custom Format Strings](#Custom-Format-Strings), Previous: [Conversion of Characters and Strings](#String-Conversion), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.7 Formatting Strings [¶](#Formatting-Strings-1)

*Formatting* means constructing a string by substituting
computed values at various places in a constant string. This constant
string controls how the other values are printed, as well as where
they appear; it is called a *format string*.

Formatting is often useful for computing messages to be displayed. In
fact, the functions `message` and `error` provide the same
formatting feature described here; they differ from `format-message` only
in how they use the result of formatting.

Function: **format** string &rest objects [¶](#index-format)
:   This function returns a string equal to string, replacing any format
    specifications with encodings of the corresponding objects. The
    arguments objects are the computed values to be formatted.

    The characters in string, other than the format specifications,
    are copied directly into the output, including their text properties,
    if any. Any text properties of the format specifications are copied
    to the produced string representations of the argument objects.

    The output string need not be newly-allocated. For example, if
    `x` is the string `"foo"`, the expressions `(eq x
    (format x))` and `(eq x (format "%s" x))` might both yield
    `t`.

Function: **format-message** string &rest objects [¶](#index-format_002dmessage)
:   This function acts like `format`, except it also converts any
    grave accents (`` ` ``) and apostrophes (`'`) in string as per the
    value of `text-quoting-style`.

    Typically grave accent and apostrophe in the format translate to
    matching curved quotes, e.g., `` "Missing `%s'" `` might result in
    `"Missing ‘foo’"`. See [Text Quoting Style](#Text-Quoting-Style), for how to influence
    or inhibit this translation.

A format specification is a sequence of characters beginning with a
‘`%`’. Thus, if there is a ‘`%d`’ in string, the
`format` function replaces it with the printed representation of
one of the values to be formatted (one of the arguments objects).
For example:

```
(format "The value of fill-column is %d." fill-column)
     ⇒ "The value of fill-column is 72."
```

Since `format` interprets ‘`%`’ characters as format
specifications, you should *never* pass an arbitrary string as
the first argument. This is particularly true when the string is
generated by some Lisp code. Unless the string is *known* to
never include any ‘`%`’ characters, pass `"%s"`, described
below, as the first argument, and the string as the second, like this:

```
  (format "%s" arbitrary-string)
```

Certain format specifications require values of particular types. If
you supply a value that doesn’t fit the requirements, an error is
signaled.

Here is a table of valid format specifications:

‘`%s`’
:   Replace the specification with the printed representation of the object,
    made without quoting (that is, using `princ`, not
    `prin1`—see [Output Functions](#Output-Functions)). Thus, strings are represented
    by their contents alone, with no ‘`"`’ characters, and symbols appear
    without ‘`\`’ characters.

    If the object is a string, its text properties are
    copied into the output. The text properties of the ‘`%s`’ itself
    are also copied, but those of the object take priority.

‘`%S`’
:   Replace the specification with the printed representation of the object,
    made with quoting (that is, using `prin1`—see [Output Functions](#Output-Functions)). Thus, strings are enclosed in ‘`"`’ characters, and
    ‘`\`’ characters appear where necessary before special characters.

‘`%b`’

‘`%B`’ [¶](#index-integer-to-binary)
:   Replace the specification with the base-two or binary representation of
    an integer. The representation has leading ‘`-`’ if negative;
    for an unsigned two’s complement representation of a possibly-negative
    integer n, format `(logand n mask)`
    where mask corresponds to the desired word size.
    Although plain ‘`%b`’ and ‘`%B`’ are identical,
    ‘`%#b`’ prefixes with lower case ‘`0b`’ and ‘`%#B`’ with upper case.
    The object can also be a floating-point number that is
    formatted as an integer, dropping any fraction.

‘`%o`’ [¶](#index-integer-to-octal)
:   Replace the specification with the base-eight representation of an
    integer. Like ‘`%b`’, the representation has leading ‘`-`’ if negative;
    The object can also be a floating-point number that is formatted
    as an integer, dropping any fraction.

‘`%d`’

‘`%i`’
:   Replace the specification with the base-ten representation of a signed
    integer. The object can also be a floating-point number that is
    formatted as an integer, dropping any fraction.

‘`%x`’ [¶](#index-integer-to-hexadecimal)

‘`%X`’
:   Replace the specification with the base-sixteen representation of an
    integer. Like ‘`%b`’, the representation has leading ‘`-`’ if negative.
    ‘`%x`’ uses lower case and ‘`%X`’ uses upper case.
    The object can also be a floating-point number that is
    formatted as an integer, dropping any fraction.

‘`%c`’
:   Replace the specification with the character which is the value given.

‘`%e`’
:   Replace the specification with the exponential notation for a
    floating-point number.

‘`%f`’
:   Replace the specification with the decimal-point notation for a
    floating-point number.

‘`%g`’
:   Replace the specification with notation for a floating-point number,
    using either exponential notation or decimal-point notation. The
    exponential notation is used if the exponent would be less than −4 or
    greater than or equal to the precision (default: 6). By default,
    trailing zeros are removed from the fractional portion of the result
    and a decimal-point character appears only if it is followed by a
    digit.

‘`%%`’
:   Replace the specification with a single ‘`%`’. This format
    specification is unusual in that its only form is plain
    ‘`%%`’ and that it does not use a value. For example,
    `(format "%% %d" 30)` returns `"% 30"`.

Any other format character results in an ‘`Invalid format
operation`’ error.

Here are several examples, which assume the typical
`text-quoting-style` settings:

```
(format "The octal value of %d is %o,
         and the hex value is %x." 18 18 18)
     ⇒ "The octal value of 18 is 22,
         and the hex value is 12."

(format-message
 "The name of this buffer is ‘%s’." (buffer-name))
     ⇒ "The name of this buffer is ‘strings.texi’."

(format-message
 "The buffer object prints as `%s'." (current-buffer))
     ⇒ "The buffer object prints as ‘strings.texi’."
```

By default, format specifications correspond to successive values from
objects. Thus, the first format specification in string
uses the first such value, the second format specification uses the
second such value, and so on. Any extra format specifications (those
for which there are no corresponding values) cause an error. Any
extra values to be formatted are ignored.

A format specification can have a *field number*, which is a
decimal number immediately after the initial ‘`%`’, followed by a
literal dollar sign ‘`$`’. It causes the format specification to
convert the argument with the given number instead of the next
argument. Field numbers start at 1. A format can contain either
numbered or unnumbered format specifications but not both, except that
‘`%%`’ can be mixed with numbered specifications.

```
(format "%2$s, %3$s, %%, %1$s" "x" "y" "z")
     ⇒ "y, z, %, x"
```

After the ‘`%`’ and any field number, you can put certain
*flag characters*.

The flag ‘`+`’ inserts a plus sign before a nonnegative number, so
that it always has a sign. A space character as flag inserts a space
before a nonnegative number. (Otherwise, nonnegative numbers start with the
first digit.) These flags are useful for ensuring that nonnegative
and negative numbers use the same number of columns. They are
ignored except for ‘`%d`’, ‘`%e`’, ‘`%f`’, ‘`%g`’, and if
both flags are used, ‘`+`’ takes precedence.

The flag ‘`#`’ specifies an alternate form which depends on
the format in use. For ‘`%o`’, it ensures that the result begins
with ‘`0`’. For ‘`%x`’, ‘`%X`’, ‘`%b`’ and ‘`%B`’,
it prefixes nonzero results with ‘`0x`’, ‘`0X`’, ‘`0b`’
or ‘`0B`’, respectively. For ‘`%e`’ and ‘`%f`’, the
‘`#`’ flag means include a decimal point even if the precision is
zero. For ‘`%g`’, it always includes a decimal point, and also
forces any trailing zeros after the decimal point to be left in place
where they would otherwise be removed.

The flag ‘`0`’ ensures that the padding consists of ‘`0`’
characters instead of spaces. This flag is ignored for non-numerical
specification characters like ‘`%s`’, ‘`%S`’ and ‘`%c`’.
These specification characters accept the ‘`0`’ flag, but still pad
with *spaces*.

The flag ‘`-`’ causes any padding inserted by the width,
if specified, to be inserted on the right rather than the left.
If both ‘`-`’ and ‘`0`’ are present, the ‘`0`’ flag is
ignored.

```
(format "%06d is padded on the left with zeros" 123)
     ⇒ "000123 is padded on the left with zeros"

(format "'%-6d' is padded on the right" 123)
     ⇒ "'123   ' is padded on the right"

(format "The word '%-7s' actually has %d letters in it."
        "foo" (length "foo"))
     ⇒ "The word 'foo    ' actually has 3 letters in it."
```

A specification can have a *width*, which is a decimal number
that appears after any field number and flags. If the printed
representation of the object contains fewer characters than this
width, `format` extends it with padding. Any padding introduced by
the width normally consists of spaces inserted on the left:

```
(format "%5d is padded on the left with spaces" 123)
     ⇒ "  123 is padded on the left with spaces"
```

If the width is too small, `format` does not truncate the
object’s printed representation. Thus, you can use a width to specify
a minimum spacing between columns with no risk of losing information.
In the following two examples, ‘`%7s`’ specifies a minimum width
of 7. In the first case, the string inserted in place of ‘`%7s`’
has only 3 letters, and needs 4 blank spaces as padding. In the
second case, the string `"specification"` is 13 letters wide but
is not truncated.

```
(format "The word '%7s' has %d letters in it."
        "foo" (length "foo"))
     ⇒ "The word '    foo' has 3 letters in it."
(format "The word '%7s' has %d letters in it."
        "specification" (length "specification"))
     ⇒ "The word 'specification' has 13 letters in it."
```

All the specification characters allow an optional *precision*
after the field number, flags and width, if present. The precision is
a decimal-point ‘`.`’ followed by a digit-string. For the
floating-point specifications (‘`%e`’ and ‘`%f`’), the
precision specifies how many digits following the decimal point to
show; if zero, the decimal-point itself is also omitted. For
‘`%g`’, the precision specifies how many significant digits to show
(significant digits are the first digit before the decimal point and
all the digits after it). If the precision of %g is zero or
unspecified, it is treated as 1. For ‘`%s`’ and ‘`%S`’, the
precision truncates the string to the given width, so ‘`%.3s`’
shows only the first three characters of the representation for
object. For other specification characters, the effect of
precision is what the local library functions of the `printf`
family produce.

If you plan to use `read` later on the formatted string to
retrieve a copy of the formatted value, use a specification that lets
`read` reconstruct the value. To format numbers in this
reversible way you can use ‘`%s`’ and ‘`%S`’, to format just
integers you can also use ‘`%d`’, ‘`#x%x`’, ‘`#o%o`’ and ‘`#b%b`’.
Other formats do not guarantee preservation of all values; for example,
‘`%g`’ can lose precision and type and does not preserve NaN or
infinity. See [Input Functions](#Input-Functions).

The functions described in this section accept a fixed set of
specification characters. The next section describes a function
`format-spec` which can accept custom specification characters,
such as ‘`%a`’ or ‘`%z`’.

---

Next: [Case Conversion in Lisp](#Case-Conversion), Previous: [Formatting Strings](#Formatting-Strings), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.8 Custom Format Strings [¶](#Custom-Format-Strings-1)

Sometimes it is useful to allow users and Lisp programs alike to
control how certain text is generated via custom format control
strings. For example, a format string could control how to display
someone’s forename, surname, and email address. Using the function
`format` described in the previous section, the format string
could be something like `"%s %s <%s>"`. This approach
quickly becomes impractical, however, as it can be unclear which
specification character corresponds to which piece of information.

A more convenient format string for such cases would be something like
`"%f %l <%e>"`, where each specification character carries
more semantic information and can easily be rearranged relative to
other specification characters, making such format strings more easily
customizable by the user.

The function `format-spec` described in this section performs a
similar function to `format`, except it operates on format
control strings that use arbitrary specification characters.

Function: **format-spec** template spec-alist &optional ignore-missing split [¶](#index-format_002dspec)
:   This function returns a string produced from the format string
    template according to conversions specified in spec-alist,
    which is an alist (see [Association Lists](#Association-Lists)) of the form
    `(letter . replacement)`. Each specification
    `%letter` in template will be replaced by
    replacement when formatting the resulting string.

    The characters in template, other than the format
    specifications, are copied directly into the output, including their
    text properties, if any. Any text properties of the format
    specifications are copied to their replacements.

    Using an alist to specify conversions gives rise to some useful
    properties:

    * If spec-alist contains more unique letter keys than there
      are unique specification characters in template, the unused keys
      are simply ignored.
    * If spec-alist contains more than one association with the same
      letter, the closest one to the start of the list is used.
    * If template contains the same specification character more than
      once, then the same replacement found in spec-alist is
      used as a basis for all of that character’s substitutions.
    * The order of specifications in template need not correspond to
      the order of associations in spec-alist.

    REPLACEMENT can also be a function taking no arguments, and returning
    a string to be used for the replacement. It will only be called when
    the corresponding LETTER is used in the TEMPLATE. This is useful, for
    example, to avoid prompting for input unless it is needed.

    The optional argument ignore-missing indicates how to handle
    specification characters in template that are not found in
    spec-alist. If it is `nil` or omitted, the function
    signals an error; if it is `ignore`, those format specifications
    are left verbatim in the output, including their text properties, if
    any; if it is `delete`, those format specifications are removed
    from the output; any other non-`nil` value is handled like
    `ignore`, but any occurrences of ‘`%%`’ are also left verbatim
    in the output.

    If the optional argument split is non-`nil`, instead of
    returning a single string, `format-spec` will split the result
    into a list of strings, based on where the substitutions were
    performed. For instance:

    ```
    (format-spec "foo %b bar" '((?b . "zot")) nil t)
         ⇒ ("foo " "zot" " bar")
    ```

The syntax of format specifications accepted by `format-spec` is
similar, but not identical, to that accepted by `format`. In
both cases, a format specification is a sequence of characters
beginning with ‘`%`’ and ending with an alphabetic letter such as
‘`s`’.

Unlike `format`, which assigns specific meanings to a fixed set
of specification characters, `format-spec` accepts arbitrary
specification characters and treats them all equally. For example:

```
(setq my-site-info
      (list (cons ?s system-name)
            (cons ?t (symbol-name system-type))
            (cons ?c system-configuration)
            (cons ?v emacs-version)
            (cons ?e invocation-name)
            (cons ?p (number-to-string (emacs-pid)))
            (cons ?a user-mail-address)
            (cons ?n user-full-name)))

(format-spec "%e %v (%c)" my-site-info)
     ⇒ "emacs 27.1 (x86_64-pc-linux-gnu)"

(format-spec "%n <%a>" my-site-info)
     ⇒ "Emacs Developers <emacs-devel@gnu.org>"
```

A format specification can include any number of the following flag
characters immediately after the ‘`%`’ to modify aspects of the
substitution.

‘`0`’
:   This flag causes any padding specified by the width to consist of
    ‘`0`’ characters instead of spaces.

‘`-`’
:   This flag causes any padding specified by the width to be inserted on
    the right rather than the left.

‘`<`’
:   This flag causes the substitution to be truncated on the left to the
    given width and precision, if specified.

‘`>`’
:   This flag causes the substitution to be truncated on the right to the
    given width and precision, if specified.

‘`^`’
:   This flag converts the substituted text to upper case (see [Case Conversion in Lisp](#Case-Conversion)).

‘`_ (underscore)`’
:   This flag converts the substituted text to lower case (see [Case Conversion in Lisp](#Case-Conversion)).

The result of using contradictory flags (for instance, both upper and
lower case) is undefined.

As is the case with `format`, a format specification can include
a width, which is a decimal number that appears after any flags, and a
precision, which is a decimal-point ‘`.`’ followed by a decimal
number that appears after any flags and width.

If a substitution contains fewer characters than its specified width,
it is padded on the left:

```
(format-spec "%8a is padded on the left with spaces"
             '((?a . "alpha")))
     ⇒ "   alpha is padded on the left with spaces"
```

If a substitution contains more characters than its specified
precision, it is truncated on the right:

```
(format-spec "%.2a is truncated on the right"
             '((?a . "alpha")))
     ⇒ "al is truncated on the right"
```

Here is a more complicated example that combines several
aforementioned features:

```
(setq my-battery-info
      (list (cons ?p "73")      ; Percentage
            (cons ?L "Battery") ; Status
            (cons ?t "2:23")    ; Remaining time
            (cons ?c "24330")   ; Capacity
            (cons ?r "10.6")))  ; Rate of discharge

(format-spec "%>^-3L : %3p%% (%05t left)" my-battery-info)
     ⇒ "BAT :  73% (02:23 left)"

(format-spec "%>^-3L : %3p%% (%05t left)"
             (cons (cons ?L "AC")
                   my-battery-info))
     ⇒ "AC  :  73% (02:23 left)"
```

As the examples in this section illustrate, `format-spec` is
often used for selectively formatting an assortment of different
pieces of information. This is useful in programs that provide
user-customizable format strings, as the user can choose to format
with a regular syntax and in any desired order only a subset of the
information that the program makes available.

---

Next: [The Case Table](#Case-Tables), Previous: [Custom Format Strings](#Custom-Format-Strings), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.9 Case Conversion in Lisp [¶](#Case-Conversion-in-Lisp)

The character case functions change the case of single characters or
of the contents of strings. The functions normally convert only
alphabetic characters (the letters ‘`A`’ through ‘`Z`’ and
‘`a`’ through ‘`z`’, as well as non-ASCII letters); other
characters are not altered. You can specify a different case
conversion mapping by specifying a case table (see [The Case Table](#Case-Tables)).

These functions do not modify the strings that are passed to them as
arguments.

The examples below use the characters ‘`X`’ and ‘`x`’ which have
ASCII codes 88 and 120 respectively.

Function: **downcase** string-or-char [¶](#index-downcase)
:   This function converts string-or-char, which should be either a
    character or a string, to lower case.

    When string-or-char is a string, this function returns a new
    string in which each letter in the argument that is upper case is
    converted to lower case. When string-or-char is a character,
    this function returns the corresponding lower case character (an
    integer); if the original character is lower case, or is not a letter,
    the return value is equal to the original character.

    ```
    (downcase "The cat in the hat")
         ⇒ "the cat in the hat"

    (downcase ?X)
         ⇒ 120
    ```

Function: **upcase** string-or-char [¶](#index-upcase)
:   This function converts string-or-char, which should be either a
    character or a string, to upper case.

    When string-or-char is a string, this function returns a new
    string in which each letter in the argument that is lower case is
    converted to upper case. When string-or-char is a character,
    this function returns the corresponding upper case character (an
    integer); if the original character is upper case, or is not a letter,
    the return value is equal to the original character.

    ```
    (upcase "The cat in the hat")
         ⇒ "THE CAT IN THE HAT"

    (upcase ?x)
         ⇒ 88
    ```

Function: **capitalize** string-or-char [¶](#index-capitalize)
:   This function capitalizes strings or characters. If
    string-or-char is a string, the function returns a new string
    whose contents are a copy of string-or-char in which each word
    has been capitalized. This means that the first character of each
    word is converted to upper case, and the rest are converted to lower
    case.

    The definition of a word is any sequence of consecutive characters that
    are assigned to the word constituent syntax class in the current syntax
    table (see [Table of Syntax Classes](#Syntax-Class-Table)); if `case-symbols-as-words`
    is non-`nil`, characters assigned to the symbol constituent syntax
    class are also considered as word constituent.

    When string-or-char is a character, this function does the same
    thing as `upcase`.

    ```
    (capitalize "The cat in the hat")
         ⇒ "The Cat In The Hat"
    ```

    ```

    ```

    ```
    (capitalize "THE 77TH-HATTED CAT")
         ⇒ "The 77th-Hatted Cat"
    ```

    ```

    ```

    ```
    (capitalize ?x)
         ⇒ 88
    ```

Function: **upcase-initials** string-or-char [¶](#index-upcase_002dinitials)
:   If string-or-char is a string, this function capitalizes the
    initials of the words in string-or-char, without altering any
    letters other than the initials. It returns a new string whose
    contents are a copy of string-or-char, in which each word has
    had its initial letter converted to upper case.

    The definition of a word for this function is the same as described
    for `capitalize` above, and `case-symbols-as-words` has the
    same effect on word constituent characters.

    When the argument to `upcase-initials` is a character,
    `upcase-initials` has the same result as `upcase`.

    ```
    (upcase-initials "The CAT in the hAt")
         ⇒ "The CAT In The HAt"
    ```

Note that case conversion is not a one-to-one mapping of codepoints
and length of the result may differ from length of the argument.
Furthermore, because passing a character forces return type to be
a character, functions are unable to perform proper substitution and
result may differ compared to treating a one-character string. For
example:

```
(upcase "ﬁ")  ; note: single character, ligature "fi"
     ⇒ "FI"
```

```
(upcase ?ﬁ)
     ⇒ 64257  ; i.e. ?ﬁ
```

To avoid this, a character must first be converted into a string,
using `string` function, before being passed to one of the casing
functions. Of course, no assumptions on the length of the result may
be made.

Other characters can also have special case-conversion rules. They
all have non-`nil` character properties `special-uppercase`,
`special-lowercase`, or `special-titlecase` (see [Character Properties](#Character-Properties)) defined by the Unicode Standard. These properties define
special case-conversion rules which override the current case table
(see [The Case Table](#Case-Tables)).

See [Comparison of Characters and Strings](#Text-Comparison), for functions that compare strings; some of
them ignore case differences, or can optionally ignore case differences.

---

Previous: [Case Conversion in Lisp](#Case-Conversion), Up: [Strings and Characters](#Strings-and-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 4.10 The Case Table [¶](#The-Case-Table)

You can customize case conversion by installing a special *case
table*. A case table specifies the mapping between upper case and lower
case letters. It affects both the case conversion functions for Lisp
objects (see the previous section) and those that apply to text in the
buffer (see [Case Changes](#Case-Changes)). Each buffer has a case table; there is
also a standard case table which is used to initialize the case table
of new buffers.

A case table is a char-table (see [Char-Tables](#Char_002dTables)) whose subtype is
`case-table`. This char-table maps each character into the
corresponding lower case character. It has three extra slots, which
hold related tables:

upcase
:   The upcase table maps each character into the corresponding upper
    case character.

canonicalize
:   The canonicalize table maps all of a set of case-related characters
    into a particular member of that set.

equivalences
:   The equivalences table maps each one of a set of case-related characters
    into the next character in that set.

In simple cases, all you need to specify is the mapping to lower-case;
the three related tables will be calculated automatically from that one.

For some languages, upper and lower case letters are not in one-to-one
correspondence. There may be two different lower case letters with the
same upper case equivalent. In these cases, you need to specify the
maps for both lower case and upper case.

Some characters have special case-conversion rules defined for them,
which by default override the current case table. These characters have
non-`nil` character properties `special-uppercase`,
`special-lowercase`, or `special-titlecase` (see [Character Properties](#Character-Properties)) defined by the Unicode Standard. An example is U+00DF
LATIN SMALL LETTER SHARP S, ß, which by default up-cases to the
string `"SS"`, not to U+1E9E LATIN CAPITAL LETTER SHARP S. To
force these characters to follow the case-table conversions, set the
corresponding Unicode property to `nil`:

```
 (upcase "ß")
  => "SS"
 (put-char-code-property ?ß 'special-uppercase nil)
 (upcase "ß")
  => "ẞ"
```

The extra slot canonicalize of a case table maps each character to a canonical
equivalent; any two characters that are related by case-conversion have
the same canonical equivalent character. For example, since ‘`a`’
and ‘`A`’ are related by case-conversion, they should have the same
canonical equivalent character (which should be either ‘`a`’ for both
of them, or ‘`A`’ for both of them).

The extra slot equivalences is a map that cyclically permutes
each equivalence class (of characters with the same canonical
equivalent). (For ordinary ASCII, this would map ‘`a`’ into
‘`A`’ and ‘`A`’ into ‘`a`’, and likewise for each set of
equivalent characters.)

When constructing a case table, you can provide `nil` for
canonicalize; then Emacs fills in this slot from the lower case
and upper case mappings. You can also provide `nil` for
equivalences; then Emacs fills in this slot from
canonicalize. In a case table that is actually in use, those
components are non-`nil`. Do not try to specify
equivalences without also specifying canonicalize.

Here are the functions for working with case tables:

Function: **case-table-p** object [¶](#index-case_002dtable_002dp)
:   This predicate returns non-`nil` if object is a valid case
    table.

Function: **set-standard-case-table** table [¶](#index-set_002dstandard_002dcase_002dtable)
:   This function makes table the standard case table, so that it will
    be used in any buffers created subsequently.

Function: **standard-case-table** [¶](#index-standard_002dcase_002dtable)
:   This returns the standard case table.

Function: **current-case-table** [¶](#index-current_002dcase_002dtable)
:   This function returns the current buffer’s case table.

Function: **set-case-table** table [¶](#index-set_002dcase_002dtable)
:   This sets the current buffer’s case table to table.

Macro: **with-case-table** table body… [¶](#index-with_002dcase_002dtable)
:   The `with-case-table` macro saves the current case table, makes
    table the current case table, evaluates the body forms,
    and finally restores the case table. The return value is the value of
    the last form in body. The case table is restored even in case
    of an abnormal exit via `throw` or error (see [Nonlocal Exits](#Nonlocal-Exits)).

Some language environments modify the case conversions of
ASCII characters; for example, in the Turkish language
environment, the ASCII capital I is downcased into
a Turkish dotless i (‘`ı`’). This can interfere with code that requires
ordinary ASCII case conversion, such as implementations of
ASCII-based network protocols. In that case, use the
`with-case-table` macro with the variable ascii-case-table,
which stores the unmodified case table for the ASCII
character set.

Variable: **ascii-case-table** [¶](#index-ascii_002dcase_002dtable)
:   The case table for the ASCII character set. This should not be
    modified by any language environment settings.

The following three functions are convenient subroutines for packages
that define non-ASCII character sets. They modify the specified
case table case-table; they also modify the standard syntax table.
See [Syntax Tables](#Syntax-Tables). Normally you would use these functions to change
the standard case table.

Function: **set-case-syntax-pair** uc lc case-table [¶](#index-set_002dcase_002dsyntax_002dpair)
:   This function specifies a pair of corresponding letters, one upper case
    and one lower case.

Function: **set-case-syntax-delims** l r case-table [¶](#index-set_002dcase_002dsyntax_002ddelims)
:   This function makes characters l and r a matching pair of
    case-invariant delimiters.

Function: **set-case-syntax** char syntax case-table [¶](#index-set_002dcase_002dsyntax)
:   This function makes char case-invariant, with syntax
    syntax.

Command: **describe-buffer-case-table** [¶](#index-describe_002dbuffer_002dcase_002dtable)
:   This command displays a description of the contents of the current
    buffer’s case table.

---

Next: [Sequences, Arrays, and Vectors](#Sequences-Arrays-Vectors), Previous: [Strings and Characters](#Strings-and-Characters), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 5 Lists [¶](#Lists-1)

A *list* represents a sequence of zero or more elements (which may
be any Lisp objects). The important difference between lists and
vectors is that two or more lists can share part of their structure; in
addition, you can insert or delete elements in a list without copying
the whole list.

* [Lists and Cons Cells](#Cons-Cells)
* [Predicates on Lists](#List_002drelated-Predicates)
* [Accessing Elements of Lists](#List-Elements)
* [Building Cons Cells and Lists](#Building-Lists)
* [Modifying List Variables](#List-Variables)
* [Modifying Existing List Structure](#Modifying-Lists)
* [Using Lists as Sets](#Sets-And-Lists)
* [Association Lists](#Association-Lists)
* [Property Lists](#Property-Lists)

---

Next: [Predicates on Lists](#List_002drelated-Predicates), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.1 Lists and Cons Cells [¶](#Lists-and-Cons-Cells)

Lists in Lisp are not a primitive data type; they are built up from
*cons cells* (see [Cons Cell and List Types](#Cons-Cell-Type)). A cons cell is a data
object that represents an ordered pair. That is, it has two slots,
and each slot *holds*, or *refers to*, some Lisp object. One
slot is known as the CAR, and the other is known as the CDR.
(These names are traditional; see [Cons Cell and List Types](#Cons-Cell-Type).) CDR is
pronounced “could-er”.

We say that “the CAR of this cons cell is” whatever object
its CAR slot currently holds, and likewise for the CDR.

A list is a series of cons cells chained together, so that each
cell refers to the next one. There is one cons cell for each element
of the list. By convention, the CARs of the cons cells hold the
elements of the list, and the CDRs are used to chain the list
(this asymmetry between CAR and CDR is entirely a matter of
convention; at the level of cons cells, the CAR and CDR
slots have similar properties). Hence, the CDR slot of each cons
cell in a list refers to the following cons cell.

Also by convention, the CDR of the last cons cell in a list is
`nil`. We call such a `nil`-terminated structure a
*proper list*[5](#FOOT5). In Emacs Lisp, the symbol `nil` is both a symbol and a
list with no elements. For convenience, the symbol `nil` is
considered to have `nil` as its CDR (and also as its
CAR).

Hence, the CDR of a proper list is always a proper list. The
CDR of a nonempty proper list is a proper list containing all the
elements except the first.

If the CDR of a list’s last cons cell is some value other than
`nil`, we call the structure a *dotted list*, since its
printed representation would use dotted pair notation (see [Dotted Pair Notation](#Dotted-Pair-Notation)). There is one other possibility: some cons cell’s
CDR could point to one of the previous cons cells in the list.
We call that structure a *circular list*.

For some purposes, it does not matter whether a list is proper,
circular or dotted. If a program doesn’t look far enough down the
list to see the CDR of the final cons cell, it won’t care.
However, some functions that operate on lists demand proper lists and
signal errors if given a dotted list. Most functions that try to find
the end of a list enter infinite loops if given a circular list. You
can use the function `proper-list-p`, described in the next
section (see [proper-list-p](#List_002drelated-Predicates)), to determine
whether a list is a proper one.

Because most cons cells are used as part of lists, we refer to any
structure made out of cons cells as a *list structure*.

---

Next: [Accessing Elements of Lists](#List-Elements), Previous: [Lists and Cons Cells](#Cons-Cells), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 5.2 Predicates on Lists [¶](#Predicates-on-Lists)

The following predicates test whether a Lisp object is an atom,
whether it is a cons cell or is a list, or whether it is the
distinguished object `nil`. (Many of these predicates can be
defined in terms of the others, but they are used so often that it is
worth having them.)

Function: **consp** object [¶](#index-consp)
:   This function returns `t` if object is a cons cell, `nil`
    otherwise. `nil` is not a cons cell, although it *is* a list.

Function: **atom** object [¶](#index-atom)
:   This function returns `t` if object is an atom, `nil`
    otherwise. All objects except cons cells are atoms. The symbol
    `nil` is an atom and is also a list; it is the only Lisp object
    that is both.

    ```
    (atom object) ≡ (not (consp object))
    ```

Function: **listp** object [¶](#index-listp)
:   This function returns `t` if object is a cons cell or
    `nil`. Otherwise, it returns `nil`.

    ```
    (listp '(1))
         ⇒ t
    ```

    ```
    (listp '())
         ⇒ t
    ```

Function: **nlistp** object [¶](#index-nlistp)
:   This function is the opposite of `listp`: it returns `t` if
    object is not a list. Otherwise, it returns `nil`.

    ```
    (listp object) ≡ (not (nlistp object))
    ```

Function: **null** object [¶](#index-null)
:   This function returns `t` if object is `nil`, and
    returns `nil` otherwise. This function is identical to `not`,
    but as a matter of clarity we use `not` when object is
    considered a truth value (see `not` in [Constructs for Combining Conditions](#Combining-Conditions)) and `null` otherwise.

    ```
    (null '(1))
         ⇒ nil
    ```

    ```
    (null '())
         ⇒ t
    ```

Function: **proper-list-p** object [¶](#index-proper_002dlist_002dp)
:   This function returns the length of object if it is a proper
    list, `nil` otherwise (see [Lists and Cons Cells](#Cons-Cells)). In addition to
    satisfying `listp`, a proper list is neither circular nor dotted.

    ```
    (proper-list-p '(a b c))
        ⇒ 3
    ```

    ```
    (proper-list-p '(a b . c))
        ⇒ nil
    ```

---

Next: [Building Cons Cells and Lists](#Building-Lists), Previous: [Predicates on Lists](#List_002drelated-Predicates), Up: [Lists](#Lists)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
