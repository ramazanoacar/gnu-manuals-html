|  |  |  |  |
    | --- | --- | --- | --- |
    | Lu | Uppercase letter | Pf | Final quote punctuation |
    | Ll | Lowercase letter | Po | Other punctuation |
    | Lt | Titlecase letter | Sm | Math symbol |
    | Lm | Modifier letter | Sc | Currency symbol |
    | Lo | Other letter | Sk | Modifier symbol |
    | Mn | Non-spacing mark | So | Other symbol |
    | Mc | Combining spacing mark | Zs | Space separator |
    | Me | Enclosing mark | Zl | Line separator |
    | Nd | Decimal digit number | Zp | Paragraph separator |
    | Nl | Letter number | Cc | Control |
    | No | Other number | Cf | Format |
    | Pc | Connector punctuation | Cs | Surrogate |
    | Pd | Dash punctuation | Co | Private use |
    | Ps | Open punctuation | Cn | Unassigned |
    | Pe | Close punctuation |  |  |
    | Pi | Initial quote punctuation |  |  |

Scheme Procedure: **char->integer** chr [¶](#index-char_002d_003einteger)

C Function: **scm\_char\_to\_integer** (chr) [¶](#index-scm_005fchar_005fto_005finteger)
:   Return the code point of chr.

Scheme Procedure: **integer->char** n [¶](#index-integer_002d_003echar)

C Function: **scm\_integer\_to\_char** (n) [¶](#index-scm_005finteger_005fto_005fchar)
:   Return the character that has code point n. The integer n
    must be a valid code point. Valid code points are in the ranges 0 to
    `#xD7FF` inclusive or `#xE000` to `#x10FFFF` inclusive.

Scheme Procedure: **char-upcase** chr [¶](#index-char_002dupcase)

C Function: **scm\_char\_upcase** (chr) [¶](#index-scm_005fchar_005fupcase)
:   Return the uppercase character version of chr.

Scheme Procedure: **char-downcase** chr [¶](#index-char_002ddowncase)

C Function: **scm\_char\_downcase** (chr) [¶](#index-scm_005fchar_005fdowncase)
:   Return the lowercase character version of chr.

Scheme Procedure: **char-titlecase** chr [¶](#index-char_002dtitlecase)

C Function: **scm\_char\_titlecase** (chr) [¶](#index-scm_005fchar_005ftitlecase)
:   Return the titlecase character version of chr if one exists;
    otherwise return the uppercase version.

    For most characters these will be the same, but the Unicode Standard
    includes certain digraph compatibility characters, such as `U+01F3`
    “dz”, for which the uppercase and titlecase characters are different
    (`U+01F1` “DZ” and `U+01F2` “Dz” in this case,
    respectively).

C Function: `scm_t_wchar` **scm\_c\_upcase** `(scm_t_wchar c)` [¶](#index-scm_005fc_005fupcase)

C Function: `scm_t_wchar` **scm\_c\_downcase** `(scm_t_wchar c)` [¶](#index-scm_005fc_005fdowncase)

C Function: `scm_t_wchar` **scm\_c\_titlecase** `(scm_t_wchar c)` [¶](#index-scm_005fc_005ftitlecase)
:   These C functions take an integer representation of a Unicode
    codepoint and return the codepoint corresponding to its uppercase,
    lowercase, and titlecase forms respectively. The type
    `scm_t_wchar` is a signed, 32-bit integer.

Characters also have “formal names”, which are defined by Unicode.
These names can be accessed in Guile from the `(ice-9 unicode)`
module:

```
(use-modules (ice-9 unicode))
```

Scheme Procedure: **char->formal-name** chr [¶](#index-char_002d_003eformal_002dname)
:   Return the formal all-upper-case Unicode name of ch,
    as a string, or `#f` if the character has no name.

Scheme Procedure: **formal-name->char** name [¶](#index-formal_002dname_002d_003echar)
:   Return the character whose formal all-upper-case Unicode name is
    name, or `#f` if no such character is known.

---

Next: [Strings](#Strings), Previous: [Characters](#Characters), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.4 Character Sets [¶](#Character-Sets-1)

The features described in this section correspond directly to SRFI-14.

The data type *charset* implements sets of characters
(see [Characters](#Characters)). Because the internal representation of
character sets is not visible to the user, a lot of procedures for
handling them are provided.

Character sets can be created, extended, tested for the membership of a
characters and be compared to other character sets.

* [Character Set Predicates/Comparison](#Character-Set-Predicates_002fComparison)
* [Iterating Over Character Sets](#Iterating-Over-Character-Sets)
* [Creating Character Sets](#Creating-Character-Sets)
* [Querying Character Sets](#Querying-Character-Sets)
* [Character-Set Algebra](#Character_002dSet-Algebra)
* [Standard Character Sets](#Standard-Character-Sets)

---

Next: [Iterating Over Character Sets](#Iterating-Over-Character-Sets), Up: [Character Sets](#Character-Sets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.4.1 Character Set Predicates/Comparison [¶](#Character-Set-Predicates_002fComparison-1)

Use these procedures for testing whether an object is a character set,
or whether several character sets are equal or subsets of each other.
`char-set-hash` can be used for calculating a hash value, maybe for
usage in fast lookup procedures.

Scheme Procedure: **char-set?** obj [¶](#index-char_002dset_003f)

C Function: **scm\_char\_set\_p** (obj) [¶](#index-scm_005fchar_005fset_005fp)
:   Return `#t` if obj is a character set, `#f`
    otherwise.

Scheme Procedure: **char-set=** char\_set … [¶](#index-char_002dset_003d)

C Function: **scm\_char\_set\_eq** (char\_sets) [¶](#index-scm_005fchar_005fset_005feq)
:   Return `#t` if all given character sets are equal.

Scheme Procedure: **char-set<=** char\_set … [¶](#index-char_002dset_003c_003d)

C Function: **scm\_char\_set\_leq** (char\_sets) [¶](#index-scm_005fchar_005fset_005fleq)
:   Return `#t` if every character set char\_seti is a subset
    of character set char\_seti+1.

Scheme Procedure: **char-set-hash** cs [bound] [¶](#index-char_002dset_002dhash)

C Function: **scm\_char\_set\_hash** (cs, bound) [¶](#index-scm_005fchar_005fset_005fhash)
:   Compute a hash value for the character set cs. If
    bound is given and non-zero, it restricts the
    returned value to the range 0 … bound - 1.

---

Next: [Creating Character Sets](#Creating-Character-Sets), Previous: [Character Set Predicates/Comparison](#Character-Set-Predicates_002fComparison), Up: [Character Sets](#Character-Sets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.4.2 Iterating Over Character Sets [¶](#Iterating-Over-Character-Sets-1)

Character set cursors are a means for iterating over the members of a
character sets. After creating a character set cursor with
`char-set-cursor`, a cursor can be dereferenced with
`char-set-ref`, advanced to the next member with
`char-set-cursor-next`. Whether a cursor has passed past the last
element of the set can be checked with `end-of-char-set?`.

Additionally, mapping and (un-)folding procedures for character sets are
provided.

Scheme Procedure: **char-set-cursor** cs [¶](#index-char_002dset_002dcursor)

C Function: **scm\_char\_set\_cursor** (cs) [¶](#index-scm_005fchar_005fset_005fcursor)
:   Return a cursor into the character set cs.

Scheme Procedure: **char-set-ref** cs cursor [¶](#index-char_002dset_002dref)

C Function: **scm\_char\_set\_ref** (cs, cursor) [¶](#index-scm_005fchar_005fset_005fref)
:   Return the character at the current cursor position
    cursor in the character set cs. It is an error to
    pass a cursor for which `end-of-char-set?` returns true.

Scheme Procedure: **char-set-cursor-next** cs cursor [¶](#index-char_002dset_002dcursor_002dnext)

C Function: **scm\_char\_set\_cursor\_next** (cs, cursor) [¶](#index-scm_005fchar_005fset_005fcursor_005fnext)
:   Advance the character set cursor cursor to the next
    character in the character set cs. It is an error if the
    cursor given satisfies `end-of-char-set?`.

Scheme Procedure: **end-of-char-set?** cursor [¶](#index-end_002dof_002dchar_002dset_003f)

C Function: **scm\_end\_of\_char\_set\_p** (cursor) [¶](#index-scm_005fend_005fof_005fchar_005fset_005fp)
:   Return `#t` if cursor has reached the end of a
    character set, `#f` otherwise.

Scheme Procedure: **char-set-fold** kons knil cs [¶](#index-char_002dset_002dfold)

C Function: **scm\_char\_set\_fold** (kons, knil, cs) [¶](#index-scm_005fchar_005fset_005ffold)
:   Fold the procedure kons over the character set cs,
    initializing it with knil.

Scheme Procedure: **char-set-unfold** p f g seed [base\_cs] [¶](#index-char_002dset_002dunfold)

C Function: **scm\_char\_set\_unfold** (p, f, g, seed, base\_cs) [¶](#index-scm_005fchar_005fset_005funfold)
:   This is a fundamental constructor for character sets.

    * g is used to generate a series of “seed” values
      from the initial seed: seed, (g seed),
      (g^2 seed), (g^3 seed), …
    * p tells us when to stop – when it returns true
      when applied to one of the seed values.
    * f maps each seed value to a character. These
      characters are added to the base character set base\_cs to
      form the result; base\_cs defaults to the empty set.

Scheme Procedure: **char-set-unfold!** p f g seed base\_cs [¶](#index-char_002dset_002dunfold_0021)

C Function: **scm\_char\_set\_unfold\_x** (p, f, g, seed, base\_cs) [¶](#index-scm_005fchar_005fset_005funfold_005fx)
:   This is a fundamental constructor for character sets.

    * g is used to generate a series of “seed” values
      from the initial seed: seed, (g seed),
      (g^2 seed), (g^3 seed), …
    * p tells us when to stop – when it returns true
      when applied to one of the seed values.
    * f maps each seed value to a character. These
      characters are added to the base character set base\_cs to
      form the result; base\_cs defaults to the empty set.

Scheme Procedure: **char-set-for-each** proc cs [¶](#index-char_002dset_002dfor_002deach)

C Function: **scm\_char\_set\_for\_each** (proc, cs) [¶](#index-scm_005fchar_005fset_005ffor_005feach)
:   Apply proc to every character in the character set
    cs. The return value is not specified.

Scheme Procedure: **char-set-map** proc cs [¶](#index-char_002dset_002dmap)

C Function: **scm\_char\_set\_map** (proc, cs) [¶](#index-scm_005fchar_005fset_005fmap)
:   Map the procedure proc over every character in cs.
    proc must be a character -> character procedure.

---

Next: [Querying Character Sets](#Querying-Character-Sets), Previous: [Iterating Over Character Sets](#Iterating-Over-Character-Sets), Up: [Character Sets](#Character-Sets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.4.3 Creating Character Sets [¶](#Creating-Character-Sets-1)

New character sets are produced with these procedures.

Scheme Procedure: **char-set-copy** cs [¶](#index-char_002dset_002dcopy)

C Function: **scm\_char\_set\_copy** (cs) [¶](#index-scm_005fchar_005fset_005fcopy)
:   Return a newly allocated character set containing all
    characters in cs.

Scheme Procedure: **char-set** chr … [¶](#index-char_002dset)

C Function: **scm\_char\_set** (chrs) [¶](#index-scm_005fchar_005fset)
:   Return a character set containing all given characters.

Scheme Procedure: **list->char-set** list [base\_cs] [¶](#index-list_002d_003echar_002dset)

C Function: **scm\_list\_to\_char\_set** (list, base\_cs) [¶](#index-scm_005flist_005fto_005fchar_005fset)
:   Convert the character list list to a character set. If
    the character set base\_cs is given, the character in this
    set are also included in the result.

Scheme Procedure: **list->char-set!** list base\_cs [¶](#index-list_002d_003echar_002dset_0021)

C Function: **scm\_list\_to\_char\_set\_x** (list, base\_cs) [¶](#index-scm_005flist_005fto_005fchar_005fset_005fx)
:   Convert the character list list to a character set. The
    characters are added to base\_cs and base\_cs is
    returned.

Scheme Procedure: **string->char-set** str [base\_cs] [¶](#index-string_002d_003echar_002dset)

C Function: **scm\_string\_to\_char\_set** (str, base\_cs) [¶](#index-scm_005fstring_005fto_005fchar_005fset)
:   Convert the string str to a character set. If the
    character set base\_cs is given, the characters in this
    set are also included in the result.

Scheme Procedure: **string->char-set!** str base\_cs [¶](#index-string_002d_003echar_002dset_0021)

C Function: **scm\_string\_to\_char\_set\_x** (str, base\_cs) [¶](#index-scm_005fstring_005fto_005fchar_005fset_005fx)
:   Convert the string str to a character set. The
    characters from the string are added to base\_cs, and
    base\_cs is returned.

Scheme Procedure: **char-set-filter** pred cs [base\_cs] [¶](#index-char_002dset_002dfilter)

C Function: **scm\_char\_set\_filter** (pred, cs, base\_cs) [¶](#index-scm_005fchar_005fset_005ffilter)
:   Return a character set containing every character from cs
    so that it satisfies pred. If provided, the characters
    from base\_cs are added to the result.

Scheme Procedure: **char-set-filter!** pred cs base\_cs [¶](#index-char_002dset_002dfilter_0021)

C Function: **scm\_char\_set\_filter\_x** (pred, cs, base\_cs) [¶](#index-scm_005fchar_005fset_005ffilter_005fx)
:   Return a character set containing every character from cs
    so that it satisfies pred. The characters are added to
    base\_cs and base\_cs is returned.

Scheme Procedure: **ucs-range->char-set** lower upper [error [base\_cs]] [¶](#index-ucs_002drange_002d_003echar_002dset)

C Function: **scm\_ucs\_range\_to\_char\_set** (lower, upper, error, base\_cs) [¶](#index-scm_005fucs_005frange_005fto_005fchar_005fset)
:   Return a character set containing all characters whose
    character codes lie in the half-open range
    [lower,upper).

    If error is a true value, an error is signaled if the
    specified range contains characters which are not contained in
    the implemented character range. If error is `#f`,
    these characters are silently left out of the resulting
    character set.

    The characters in base\_cs are added to the result, if
    given.

Scheme Procedure: **ucs-range->char-set!** lower upper error base\_cs [¶](#index-ucs_002drange_002d_003echar_002dset_0021)

C Function: **scm\_ucs\_range\_to\_char\_set\_x** (lower, upper, error, base\_cs) [¶](#index-scm_005fucs_005frange_005fto_005fchar_005fset_005fx)
:   Return a character set containing all characters whose
    character codes lie in the half-open range
    [lower,upper).

    If error is a true value, an error is signaled if the
    specified range contains characters which are not contained in
    the implemented character range. If error is `#f`,
    these characters are silently left out of the resulting
    character set.

    The characters are added to base\_cs and base\_cs is
    returned.

Scheme Procedure: **->char-set** x [¶](#index-_002d_003echar_002dset)

C Function: **scm\_to\_char\_set** (x) [¶](#index-scm_005fto_005fchar_005fset)
:   Coerces x into a char-set. x may be a string, character or
    char-set. A string is converted to the set of its constituent
    characters; a character is converted to a singleton set; a char-set is
    returned as-is.

---

Next: [Character-Set Algebra](#Character_002dSet-Algebra), Previous: [Creating Character Sets](#Creating-Character-Sets), Up: [Character Sets](#Character-Sets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.4.4 Querying Character Sets [¶](#Querying-Character-Sets-1)

Access the elements and other information of a character set with these
procedures.

Scheme Procedure: **%char-set-dump** cs [¶](#index-_0025char_002dset_002ddump)
:   Returns an association list containing debugging information
    for cs. The association list has the following entries.

    `char-set`
    :   The char-set itself

    `len`
    :   The number of groups of contiguous code points the char-set
        contains

    `ranges`
    :   A list of lists where each sublist is a range of code points
        and their associated characters

    The return value of this function cannot be relied upon to be
    consistent between versions of Guile and should not be used in code.

Scheme Procedure: **char-set-size** cs [¶](#index-char_002dset_002dsize)

C Function: **scm\_char\_set\_size** (cs) [¶](#index-scm_005fchar_005fset_005fsize)
:   Return the number of elements in character set cs.

Scheme Procedure: **char-set-count** pred cs [¶](#index-char_002dset_002dcount)

C Function: **scm\_char\_set\_count** (pred, cs) [¶](#index-scm_005fchar_005fset_005fcount)
:   Return the number of the elements int the character set
    cs which satisfy the predicate pred.

Scheme Procedure: **char-set->list** cs [¶](#index-char_002dset_002d_003elist)

C Function: **scm\_char\_set\_to\_list** (cs) [¶](#index-scm_005fchar_005fset_005fto_005flist)
:   Return a list containing the elements of the character set
    cs.

Scheme Procedure: **char-set->string** cs [¶](#index-char_002dset_002d_003estring)

C Function: **scm\_char\_set\_to\_string** (cs) [¶](#index-scm_005fchar_005fset_005fto_005fstring)
:   Return a string containing the elements of the character set
    cs. The order in which the characters are placed in the
    string is not defined.

Scheme Procedure: **char-set-contains?** cs ch [¶](#index-char_002dset_002dcontains_003f)

C Function: **scm\_char\_set\_contains\_p** (cs, ch) [¶](#index-scm_005fchar_005fset_005fcontains_005fp)
:   Return `#t` if the character ch is contained in the
    character set cs, or `#f` otherwise.

Scheme Procedure: **char-set-every** pred cs [¶](#index-char_002dset_002devery)

C Function: **scm\_char\_set\_every** (pred, cs) [¶](#index-scm_005fchar_005fset_005fevery)
:   Return a true value if every character in the character set
    cs satisfies the predicate pred.

Scheme Procedure: **char-set-any** pred cs [¶](#index-char_002dset_002dany)

C Function: **scm\_char\_set\_any** (pred, cs) [¶](#index-scm_005fchar_005fset_005fany)
:   Return a true value if any character in the character set
    cs satisfies the predicate pred.

---

Next: [Standard Character Sets](#Standard-Character-Sets), Previous: [Querying Character Sets](#Querying-Character-Sets), Up: [Character Sets](#Character-Sets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.4.5 Character-Set Algebra [¶](#Character_002dSet-Algebra-1)

Character sets can be manipulated with the common set algebra operation,
such as union, complement, intersection etc. All of these procedures
provide side-effecting variants, which modify their character set
argument(s).

Scheme Procedure: **char-set-adjoin** cs chr … [¶](#index-char_002dset_002dadjoin)

C Function: **scm\_char\_set\_adjoin** (cs, chrs) [¶](#index-scm_005fchar_005fset_005fadjoin)
:   Add all character arguments to the first argument, which must
    be a character set.

Scheme Procedure: **char-set-delete** cs chr … [¶](#index-char_002dset_002ddelete)

C Function: **scm\_char\_set\_delete** (cs, chrs) [¶](#index-scm_005fchar_005fset_005fdelete)
:   Delete all character arguments from the first argument, which
    must be a character set.

Scheme Procedure: **char-set-adjoin!** cs chr … [¶](#index-char_002dset_002dadjoin_0021)

C Function: **scm\_char\_set\_adjoin\_x** (cs, chrs) [¶](#index-scm_005fchar_005fset_005fadjoin_005fx)
:   Add all character arguments to the first argument, which must
    be a character set.

Scheme Procedure: **char-set-delete!** cs chr … [¶](#index-char_002dset_002ddelete_0021)

C Function: **scm\_char\_set\_delete\_x** (cs, chrs) [¶](#index-scm_005fchar_005fset_005fdelete_005fx)
:   Delete all character arguments from the first argument, which
    must be a character set.

Scheme Procedure: **char-set-complement** cs [¶](#index-char_002dset_002dcomplement)

C Function: **scm\_char\_set\_complement** (cs) [¶](#index-scm_005fchar_005fset_005fcomplement)
:   Return the complement of the character set cs.

Note that the complement of a character set is likely to contain many
reserved code points (code points that are not associated with
characters). It may be helpful to modify the output of
`char-set-complement` by computing its intersection with the set
of designated code points, `char-set:designated`.

Scheme Procedure: **char-set-union** cs … [¶](#index-char_002dset_002dunion)

C Function: **scm\_char\_set\_union** (char\_sets) [¶](#index-scm_005fchar_005fset_005funion)
:   Return the union of all argument character sets.

Scheme Procedure: **char-set-intersection** cs … [¶](#index-char_002dset_002dintersection)

C Function: **scm\_char\_set\_intersection** (char\_sets) [¶](#index-scm_005fchar_005fset_005fintersection)
:   Return the intersection of all argument character sets.

Scheme Procedure: **char-set-difference** cs1 cs … [¶](#index-char_002dset_002ddifference)

C Function: **scm\_char\_set\_difference** (cs1, char\_sets) [¶](#index-scm_005fchar_005fset_005fdifference)
:   Return the difference of all argument character sets.

Scheme Procedure: **char-set-xor** cs … [¶](#index-char_002dset_002dxor)

C Function: **scm\_char\_set\_xor** (char\_sets) [¶](#index-scm_005fchar_005fset_005fxor)
:   Return the exclusive-or of all argument character sets.

Scheme Procedure: **char-set-diff+intersection** cs1 cs … [¶](#index-char_002dset_002ddiff_002bintersection)

C Function: **scm\_char\_set\_diff\_plus\_intersection** (cs1, char\_sets) [¶](#index-scm_005fchar_005fset_005fdiff_005fplus_005fintersection)
:   Return the difference and the intersection of all argument
    character sets.

Scheme Procedure: **char-set-complement!** cs [¶](#index-char_002dset_002dcomplement_0021)

C Function: **scm\_char\_set\_complement\_x** (cs) [¶](#index-scm_005fchar_005fset_005fcomplement_005fx)
:   Return the complement of the character set cs.

Scheme Procedure: **char-set-union!** cs1 cs … [¶](#index-char_002dset_002dunion_0021)

C Function: **scm\_char\_set\_union\_x** (cs1, char\_sets) [¶](#index-scm_005fchar_005fset_005funion_005fx)
:   Return the union of all argument character sets.

Scheme Procedure: **char-set-intersection!** cs1 cs … [¶](#index-char_002dset_002dintersection_0021)

C Function: **scm\_char\_set\_intersection\_x** (cs1, char\_sets) [¶](#index-scm_005fchar_005fset_005fintersection_005fx)
:   Return the intersection of all argument character sets.

Scheme Procedure: **char-set-difference!** cs1 cs … [¶](#index-char_002dset_002ddifference_0021)

C Function: **scm\_char\_set\_difference\_x** (cs1, char\_sets) [¶](#index-scm_005fchar_005fset_005fdifference_005fx)
:   Return the difference of all argument character sets.

Scheme Procedure: **char-set-xor!** cs1 cs … [¶](#index-char_002dset_002dxor_0021)

C Function: **scm\_char\_set\_xor\_x** (cs1, char\_sets) [¶](#index-scm_005fchar_005fset_005fxor_005fx)
:   Return the exclusive-or of all argument character sets.

Scheme Procedure: **char-set-diff+intersection!** cs1 cs2 cs … [¶](#index-char_002dset_002ddiff_002bintersection_0021)

C Function: **scm\_char\_set\_diff\_plus\_intersection\_x** (cs1, cs2, char\_sets) [¶](#index-scm_005fchar_005fset_005fdiff_005fplus_005fintersection_005fx)
:   Return the difference and the intersection of all argument
    character sets.

---

Previous: [Character-Set Algebra](#Character_002dSet-Algebra), Up: [Character Sets](#Character-Sets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.4.6 Standard Character Sets [¶](#Standard-Character-Sets-1)

In order to make the use of the character set data type and procedures
useful, several predefined character set variables exist.

These character sets are locale independent and are not recomputed
upon a `setlocale` call. They contain characters from the whole
range of Unicode code points. For instance, `char-set:letter`
contains about 100,000 characters.

Scheme Variable: **char-set:lower-case** [¶](#index-char_002dset_003alower_002dcase)

C Variable: **scm\_char\_set\_lower\_case** [¶](#index-scm_005fchar_005fset_005flower_005fcase)
:   All lower-case characters.

Scheme Variable: **char-set:upper-case** [¶](#index-char_002dset_003aupper_002dcase)

C Variable: **scm\_char\_set\_upper\_case** [¶](#index-scm_005fchar_005fset_005fupper_005fcase)
:   All upper-case characters.

Scheme Variable: **char-set:title-case** [¶](#index-char_002dset_003atitle_002dcase)

C Variable: **scm\_char\_set\_title\_case** [¶](#index-scm_005fchar_005fset_005ftitle_005fcase)
:   All single characters that function as if they were an upper-case
    letter followed by a lower-case letter.

Scheme Variable: **char-set:letter** [¶](#index-char_002dset_003aletter)

C Variable: **scm\_char\_set\_letter** [¶](#index-scm_005fchar_005fset_005fletter)
:   All letters. This includes `char-set:lower-case`,
    `char-set:upper-case`, `char-set:title-case`, and many
    letters that have no case at all. For example, Chinese and Japanese
    characters typically have no concept of case.

Scheme Variable: **char-set:digit** [¶](#index-char_002dset_003adigit)

C Variable: **scm\_char\_set\_digit** [¶](#index-scm_005fchar_005fset_005fdigit)
:   All digits.

Scheme Variable: **char-set:letter+digit** [¶](#index-char_002dset_003aletter_002bdigit)

C Variable: **scm\_char\_set\_letter\_and\_digit** [¶](#index-scm_005fchar_005fset_005fletter_005fand_005fdigit)
:   The union of `char-set:letter` and `char-set:digit`.

Scheme Variable: **char-set:graphic** [¶](#index-char_002dset_003agraphic)

C Variable: **scm\_char\_set\_graphic** [¶](#index-scm_005fchar_005fset_005fgraphic)
:   All characters which would put ink on the paper.

Scheme Variable: **char-set:printing** [¶](#index-char_002dset_003aprinting)

C Variable: **scm\_char\_set\_printing** [¶](#index-scm_005fchar_005fset_005fprinting)
:   The union of `char-set:graphic` and `char-set:whitespace`.

Scheme Variable: **char-set:whitespace** [¶](#index-char_002dset_003awhitespace)

C Variable: **scm\_char\_set\_whitespace** [¶](#index-scm_005fchar_005fset_005fwhitespace)
:   All whitespace characters.

Scheme Variable: **char-set:blank** [¶](#index-char_002dset_003ablank)

C Variable: **scm\_char\_set\_blank** [¶](#index-scm_005fchar_005fset_005fblank)
:   All horizontal whitespace characters, which notably includes
    `#\space` and `#\tab`.

Scheme Variable: **char-set:iso-control** [¶](#index-char_002dset_003aiso_002dcontrol)

C Variable: **scm\_char\_set\_iso\_control** [¶](#index-scm_005fchar_005fset_005fiso_005fcontrol)
:   The ISO control characters are the C0 control characters (U+0000 to
    U+001F), delete (U+007F), and the C1 control characters (U+0080 to
    U+009F).

Scheme Variable: **char-set:punctuation** [¶](#index-char_002dset_003apunctuation)

C Variable: **scm\_char\_set\_punctuation** [¶](#index-scm_005fchar_005fset_005fpunctuation)
:   All punctuation characters, such as the characters
    `!"#%&'()*,-./:;?@[\\]_{}`

Scheme Variable: **char-set:symbol** [¶](#index-char_002dset_003asymbol)

C Variable: **scm\_char\_set\_symbol** [¶](#index-scm_005fchar_005fset_005fsymbol)
:   All symbol characters, such as the characters `` $+<=>^`|~ ``.

Scheme Variable: **char-set:hex-digit** [¶](#index-char_002dset_003ahex_002ddigit)

C Variable: **scm\_char\_set\_hex\_digit** [¶](#index-scm_005fchar_005fset_005fhex_005fdigit)
:   The hexadecimal digits `0123456789abcdefABCDEF`.

Scheme Variable: **char-set:ascii** [¶](#index-char_002dset_003aascii)

C Variable: **scm\_char\_set\_ascii** [¶](#index-scm_005fchar_005fset_005fascii)
:   All ASCII characters.

Scheme Variable: **char-set:empty** [¶](#index-char_002dset_003aempty)

C Variable: **scm\_char\_set\_empty** [¶](#index-scm_005fchar_005fset_005fempty)
:   The empty character set.

Scheme Variable: **char-set:designated** [¶](#index-char_002dset_003adesignated)

C Variable: **scm\_char\_set\_designated** [¶](#index-scm_005fchar_005fset_005fdesignated)
:   This character set contains all designated code points. This includes
    all the code points to which Unicode has assigned a character or other
    meaning.

Scheme Variable: **char-set:full** [¶](#index-char_002dset_003afull)

C Variable: **scm\_char\_set\_full** [¶](#index-scm_005fchar_005fset_005ffull)
:   This character set contains all possible code points. This includes
    both designated and reserved code points.

---

Next: [Symbols](#Symbols), Previous: [Character Sets](#Character-Sets), Up: [Data Types](#Data-Types)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5 Strings [¶](#Strings-1)

Strings are fixed-length sequences of characters. They can be created
by calling constructor procedures, but they can also literally get
entered at the REPL or in Scheme source files.

Strings always carry the information about how many characters they are
composed of with them, so there is no special end-of-string character,
like in C. That means that Scheme strings can contain any character,
even the ‘`#\nul`’ character ‘`\0`’.

To use strings efficiently, you need to know a bit about how Guile
implements them. In Guile, a string consists of two parts, a head and
the actual memory where the characters are stored. When a string (or
a substring of it) is copied, only a new head gets created, the memory
is usually not copied. The two heads start out pointing to the same
memory.

When one of these two strings is modified, as with `string-set!`,
their common memory does get copied so that each string has its own
memory and modifying one does not accidentally modify the other as well.
Thus, Guile’s strings are ‘copy on write’; the actual copying of their
memory is delayed until one string is written to.

This implementation makes functions like `substring` very
efficient in the common case that no modifications are done to the
involved strings.

If you do know that your strings are getting modified right away, you
can use `substring/copy` instead of `substring`. This
function performs the copy immediately at the time of creation. This
is more efficient, especially in a multi-threaded program. Also,
`substring/copy` can avoid the problem that a short substring
holds on to the memory of a very large original string that could
otherwise be recycled.

If you want to avoid the copy altogether, so that modifications of one
string show up in the other, you can use `substring/shared`. The
strings created by this procedure are called *mutation sharing
substrings* since the substring and the original string share
modifications to each other.

If you want to prevent modifications, use `substring/read-only`.

Guile provides all procedures of SRFI-13 and a few more.

* [String Read Syntax](#String-Syntax)
* [String Predicates](#String-Predicates)
* [String Constructors](#String-Constructors)
* [List/String conversion](#List_002fString-Conversion)
* [String Selection](#String-Selection)
* [String Modification](#String-Modification)
* [String Comparison](#String-Comparison)
* [String Searching](#String-Searching)
* [Alphabetic Case Mapping](#Alphabetic-Case-Mapping)
* [Reversing and Appending Strings](#Reversing-and-Appending-Strings)
* [Mapping, Folding, and Unfolding](#Mapping-Folding-and-Unfolding)
* [Miscellaneous String Operations](#Miscellaneous-String-Operations)
* [Representing Strings as Bytes](#Representing-Strings-as-Bytes)
* [Conversion to/from C](#Conversion-to_002ffrom-C)
* [String Internals](#String-Internals)

---

Next: [String Predicates](#String-Predicates), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.1 String Read Syntax [¶](#String-Read-Syntax)

The read syntax for strings is an arbitrarily long sequence of
characters enclosed in double quotes (`"`).

Backslash is an escape character and can be used to insert the following
special characters. `\"` and `\\` are R5RS standard,
`\|` is R7RS standard, the next seven are R6RS standard —
notice they follow C syntax — and the remaining four are Guile
extensions.

`\\`
:   Backslash character.

`\"`
:   Double quote character (an unescaped `"` is otherwise the end
    of the string).

`\|`
:   Vertical bar character.

`\a`
:   Bell character (ASCII 7).

`\f`
:   Formfeed character (ASCII 12).

`\n`
:   Newline character (ASCII 10).

`\r`
:   Carriage return character (ASCII 13).

`\t`
:   Tab character (ASCII 9).

`\v`
:   Vertical tab character (ASCII 11).

`\b`
:   Backspace character (ASCII 8).

`\0`
:   NUL character (ASCII 0).

`\(`
:   Open parenthesis. This is intended for use at the beginning of lines in
    multiline strings to avoid confusing Emacs lisp modes.

`\` followed by newline (ASCII 10)
:   Nothing. This way if `\` is the last character in a line, the
    string will continue with the first character from the next line,
    without a line break.

    If the `hungry-eol-escapes` reader option is enabled, which is not
    the case by default, leading whitespace on the next line is discarded.

    ```
    "foo\
      bar"
    ⇒ "foo  bar"
    (read-enable 'hungry-eol-escapes)
    "foo\
      bar"
    ⇒ "foobar"
    ```

`\xHH`
:   Character code given by two hexadecimal digits. For example
    `\x7f` for an ASCII DEL (127).

`\uHHHH`
:   Character code given by four hexadecimal digits. For example
    `\u0100` for a capital A with macron (U+0100).

`\UHHHHHH`
:   Character code given by six hexadecimal digits. For example
    `\U010402`.

The following are examples of string literals:

```
"foo"
"bar plonk"
"Hello World"
"\"Hi\", he said."
```

The three escape sequences `\xHH`, `\uHHHH` and `\UHHHHHH` were
chosen to not break compatibility with code written for previous versions of
Guile. The R6RS specification suggests a different, incompatible syntax for hex
escapes: `\xHHHH;` – a character code followed by one to eight hexadecimal
digits terminated with a semicolon. If this escape format is desired instead,
it can be enabled with the reader option `r6rs-hex-escapes`.

```
(read-enable 'r6rs-hex-escapes)
```

For more on reader options, See [Reading Scheme Code](#Scheme-Read).

---

Next: [String Constructors](#String-Constructors), Previous: [String Read Syntax](#String-Syntax), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.2 String Predicates [¶](#String-Predicates-1)

The following procedures can be used to check whether a given string
fulfills some specified property.

Scheme Procedure: **string?** obj [¶](#index-string_003f)

C Function: **scm\_string\_p** (obj) [¶](#index-scm_005fstring_005fp)
:   Return `#t` if obj is a string, else `#f`.

C Function: `int` **scm\_is\_string** `(SCM obj)` [¶](#index-scm_005fis_005fstring)
:   Returns `1` if obj is a string, `0` otherwise.

Scheme Procedure: **string-null?** str [¶](#index-string_002dnull_003f)

C Function: **scm\_string\_null\_p** (str) [¶](#index-scm_005fstring_005fnull_005fp)
:   Return `#t` if str’s length is zero, and
    `#f` otherwise.

    ```
    (string-null? "")  ⇒ #t
    y                    ⇒ "foo"
    (string-null? y)     ⇒ #f
    ```

Scheme Procedure: **string-any** char\_pred s [start [end]] [¶](#index-string_002dany)

C Function: **scm\_string\_any** (char\_pred, s, start, end) [¶](#index-scm_005fstring_005fany)
:   Check if char\_pred is true for any character in string s.

    char\_pred can be a character to check for any equal to that, or
    a character set (see [Character Sets](#Character-Sets)) to check for any in that set,
    or a predicate procedure to call.

    For a procedure, calls `(char_pred c)` are made
    successively on the characters from start to end. If
    char\_pred returns true (ie. non-`#f`), `string-any`
    stops and that return value is the return from `string-any`. The
    call on the last character (ie. at *end-1*), if that
    point is reached, is a tail call.

    If there are no characters in s (ie. start equals
    end) then the return is `#f`.

Scheme Procedure: **string-every** char\_pred s [start [end]] [¶](#index-string_002devery)

C Function: **scm\_string\_every** (char\_pred, s, start, end) [¶](#index-scm_005fstring_005fevery)
:   Check if char\_pred is true for every character in string
    s.

    char\_pred can be a character to check for every character equal
    to that, or a character set (see [Character Sets](#Character-Sets)) to check for
    every character being in that set, or a predicate procedure to call.

    For a procedure, calls `(char_pred c)` are made
    successively on the characters from start to end. If
    char\_pred returns `#f`, `string-every` stops and
    returns `#f`. The call on the last character (ie. at
    *end-1*), if that point is reached, is a tail call and the
    return from that call is the return from `string-every`.

    If there are no characters in s (ie. start equals
    end) then the return is `#t`.

---

Next: [List/String conversion](#List_002fString-Conversion), Previous: [String Predicates](#String-Predicates), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.3 String Constructors [¶](#String-Constructors-1)

The string constructor procedures create new string objects, possibly
initializing them with some specified character data. See also
See [String Selection](#String-Selection), for ways to create strings from existing
strings.

Scheme Procedure: **string** char… [¶](#index-string)
:   Return a newly allocated string made from the given character
    arguments.

    ```
    (string #\x #\y #\z) ⇒ "xyz"
    (string)             ⇒ ""
    ```

Scheme Procedure: **list->string** lst [¶](#index-list_002d_003estring)

C Function: **scm\_string** (lst) [¶](#index-scm_005fstring)
:   Return a newly allocated string made from a list of characters.

    ```
    (list->string '(#\a #\b #\c)) ⇒ "abc"
    ```

Scheme Procedure: **reverse-list->string** lst [¶](#index-reverse_002dlist_002d_003estring)

C Function: **scm\_reverse\_list\_to\_string** (lst) [¶](#index-scm_005freverse_005flist_005fto_005fstring)
:   Return a newly allocated string made from a list of characters, in
    reverse order.

    ```
    (reverse-list->string '(#\a #\B #\c)) ⇒ "cBa"
    ```

Scheme Procedure: **make-string** k [chr] [¶](#index-make_002dstring)

C Function: **scm\_make\_string** (k, chr) [¶](#index-scm_005fmake_005fstring)
:   Return a newly allocated string of
    length k. If chr is given, then all elements of
    the string are initialized to chr, otherwise the contents
    of the string are unspecified.

C Function: `SCM` **scm\_c\_make\_string** `(size_t len, SCM chr)` [¶](#index-scm_005fc_005fmake_005fstring)
:   Like `scm_make_string`, but expects the length as a
    `size_t`.

Scheme Procedure: **string-tabulate** proc len [¶](#index-string_002dtabulate)

C Function: **scm\_string\_tabulate** (proc, len) [¶](#index-scm_005fstring_005ftabulate)
:   proc is an integer->char procedure. Construct a string
    of size len by applying proc to each index to
    produce the corresponding string element. The order in which
    proc is applied to the indices is not specified.

Scheme Procedure: **string-join** ls [delimiter [grammar]] [¶](#index-string_002djoin)

C Function: **scm\_string\_join** (ls, delimiter, grammar) [¶](#index-scm_005fstring_005fjoin)
:   Append the string in the string list ls, using the string
    delimiter as a delimiter between the elements of ls.
    delimiter defaults to ‘’, that is, strings in ls
    are appended with the space character in between them. grammar is
    a symbol which specifies how the delimiter is placed between the
    strings, and defaults to the symbol `infix`.

    `infix`
    :   Insert the separator between list elements. An empty string
        will produce an empty list.

    `strict-infix`
    :   Like `infix`, but will raise an error if given the empty
        list.

    `suffix`
    :   Insert the separator after every list element.

    `prefix`
    :   Insert the separator before each list element.

---

Next: [String Selection](#String-Selection), Previous: [String Constructors](#String-Constructors), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.4 List/String conversion [¶](#List_002fString-conversion)

When processing strings, it is often convenient to first convert them
into a list representation by using the procedure `string->list`,
work with the resulting list, and then convert it back into a string.
These procedures are useful for similar tasks.

Scheme Procedure: **string->list** str [start [end]] [¶](#index-string_002d_003elist)

C Function: **scm\_substring\_to\_list** (str, start, end) [¶](#index-scm_005fsubstring_005fto_005flist)

C Function: **scm\_string\_to\_list** (str) [¶](#index-scm_005fstring_005fto_005flist)
:   Convert the string str into a list of characters.

Scheme Procedure: **string-split** str char\_pred [¶](#index-string_002dsplit)

C Function: **scm\_string\_split** (str, char\_pred) [¶](#index-scm_005fstring_005fsplit)
:   Split the string str into a list of substrings delimited
    by appearances of characters that

    * equal char\_pred, if it is a character,
    * satisfy the predicate char\_pred, if it is a procedure,
    * are in the set char\_pred, if it is a character set.

    Note that an empty substring between separator characters will result in
    an empty string in the result list.

    ```
    (string-split "root:x:0:0:root:/root:/bin/bash" #\:)
    ⇒
    ("root" "x" "0" "0" "root" "/root" "/bin/bash")

    (string-split "::" #\:)
    ⇒
    ("" "" "")

    (string-split "" #\:)
    ⇒
    ("")
    ```

---

Next: [String Modification](#String-Modification), Previous: [List/String conversion](#List_002fString-Conversion), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.5 String Selection [¶](#String-Selection-1)

Portions of strings can be extracted by these procedures.
`string-ref` delivers individual characters whereas
`substring` can be used to extract substrings from longer strings.

Scheme Procedure: **string-length** string [¶](#index-string_002dlength)

C Function: **scm\_string\_length** (string) [¶](#index-scm_005fstring_005flength)
:   Return the number of characters in string.

C Function: `size_t` **scm\_c\_string\_length** `(SCM str)` [¶](#index-scm_005fc_005fstring_005flength)
:   Return the number of characters in str as a `size_t`.

Scheme Procedure: **string-ref** str k [¶](#index-string_002dref)

C Function: **scm\_string\_ref** (str, k) [¶](#index-scm_005fstring_005fref)
:   Return character k of str using zero-origin
    indexing. k must be a valid index of str.

C Function: `SCM` **scm\_c\_string\_ref** `(SCM str, size_t k)` [¶](#index-scm_005fc_005fstring_005fref)
:   Return character k of str using zero-origin
    indexing. k must be a valid index of str.

Scheme Procedure: **string-copy** str [start [end]] [¶](#index-string_002dcopy)

C Function: **scm\_substring\_copy** (str, start, end) [¶](#index-scm_005fsubstring_005fcopy)

C Function: **scm\_string\_copy** (str) [¶](#index-scm_005fstring_005fcopy)
:   Return a copy of the given string str.

    The returned string shares storage with str initially, but it is
    copied as soon as one of the two strings is modified.

Scheme Procedure: **substring** str start [end] [¶](#index-substring)

C Function: **scm\_substring** (str, start, end) [¶](#index-scm_005fsubstring)
:   Return a new string formed from the characters
    of str beginning with index start (inclusive) and
    ending with index end (exclusive).
    str must be a string, start and end must be
    exact integers satisfying:

    0 <= start <= end <= `(string-length str)`.

    The returned string shares storage with str initially, but it is
    copied as soon as one of the two strings is modified.

Scheme Procedure: **substring/shared** str start [end] [¶](#index-substring_002fshared)

C Function: **scm\_substring\_shared** (str, start, end) [¶](#index-scm_005fsubstring_005fshared)
:   Like `substring`, but the strings continue to share their storage
    even if they are modified. Thus, modifications to str show up
    in the new string, and vice versa.

Scheme Procedure: **substring/copy** str start [end] [¶](#index-substring_002fcopy)

C Function: **scm\_substring\_copy** (str, start, end) [¶](#index-scm_005fsubstring_005fcopy-1)
:   Like `substring`, but the storage for the new string is copied
    immediately.

Scheme Procedure: **substring/read-only** str start [end] [¶](#index-substring_002fread_002donly)

C Function: **scm\_substring\_read\_only** (str, start, end) [¶](#index-scm_005fsubstring_005fread_005fonly)
:   Like `substring`, but the resulting string can not be modified.

C Function: `SCM` **scm\_c\_substring** `(SCM str, size_t start, size_t end)` [¶](#index-scm_005fc_005fsubstring)

C Function: `SCM` **scm\_c\_substring\_shared** `(SCM str, size_t start, size_t end)` [¶](#index-scm_005fc_005fsubstring_005fshared)

C Function: `SCM` **scm\_c\_substring\_copy** `(SCM str, size_t start, size_t end)` [¶](#index-scm_005fc_005fsubstring_005fcopy)

C Function: `SCM` **scm\_c\_substring\_read\_only** `(SCM str, size_t start, size_t end)` [¶](#index-scm_005fc_005fsubstring_005fread_005fonly)
:   Like `scm_substring`, etc. but the bounds are given as a `size_t`.

Scheme Procedure: **string-take** s n [¶](#index-string_002dtake)

C Function: **scm\_string\_take** (s, n) [¶](#index-scm_005fstring_005ftake)
:   Return the n first characters of s.

Scheme Procedure: **string-drop** s n [¶](#index-string_002ddrop)

C Function: **scm\_string\_drop** (s, n) [¶](#index-scm_005fstring_005fdrop)
:   Return all but the first n characters of s.

Scheme Procedure: **string-take-right** s n [¶](#index-string_002dtake_002dright)

C Function: **scm\_string\_take\_right** (s, n) [¶](#index-scm_005fstring_005ftake_005fright)
:   Return the n last characters of s.

Scheme Procedure: **string-drop-right** s n [¶](#index-string_002ddrop_002dright)

C Function: **scm\_string\_drop\_right** (s, n) [¶](#index-scm_005fstring_005fdrop_005fright)
:   Return all but the last n characters of s.

Scheme Procedure: **string-pad** s len [chr [start [end]]] [¶](#index-string_002dpad)

Scheme Procedure: **string-pad-right** s len [chr [start [end]]] [¶](#index-string_002dpad_002dright)

C Function: **scm\_string\_pad** (s, len, chr, start, end) [¶](#index-scm_005fstring_005fpad)

C Function: **scm\_string\_pad\_right** (s, len, chr, start, end) [¶](#index-scm_005fstring_005fpad_005fright)
:   Take characters start to end from the string s and
    either pad with chr or truncate them to give len
    characters.

    `string-pad` pads or truncates on the left, so for example

    ```
    (string-pad "x" 3)     ⇒ "  x"
    (string-pad "abcde" 3) ⇒ "cde"
    ```

    `string-pad-right` pads or truncates on the right, so for example

    ```
    (string-pad-right "x" 3)     ⇒ "x  "
    (string-pad-right "abcde" 3) ⇒ "abc"
    ```

Scheme Procedure: **string-trim** s [char\_pred [start [end]]] [¶](#index-string_002dtrim)

Scheme Procedure: **string-trim-right** s [char\_pred [start [end]]] [¶](#index-string_002dtrim_002dright)

Scheme Procedure: **string-trim-both** s [char\_pred [start [end]]] [¶](#index-string_002dtrim_002dboth)

C Function: **scm\_string\_trim** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005ftrim)

C Function: **scm\_string\_trim\_right** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005ftrim_005fright)

C Function: **scm\_string\_trim\_both** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005ftrim_005fboth)
:   Trim occurrences of char\_pred from the ends of s.

    `string-trim` trims char\_pred characters from the left
    (start) of the string, `string-trim-right` trims them from the
    right (end) of the string, `string-trim-both` trims from both
    ends.

    char\_pred can be a character, a character set, or a predicate
    procedure to call on each character. If char\_pred is not given
    the default is whitespace as per `char-set:whitespace`
    (see [Standard Character Sets](#Standard-Character-Sets)).

    ```
    (string-trim " x ")              ⇒ "x "
    (string-trim-right "banana" #\a) ⇒ "banan"
    (string-trim-both ".,xy:;" char-set:punctuation)
                      ⇒ "xy"
    (string-trim-both "xyzzy" (lambda (c)
                                 (or (eqv? c #\x)
                                     (eqv? c #\y))))
                      ⇒ "zz"
    ```

---

Next: [String Comparison](#String-Comparison), Previous: [String Selection](#String-Selection), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.6 String Modification [¶](#String-Modification-1)

These procedures are for modifying strings in-place. This means that the
result of the operation is not a new string; instead, the original string’s
memory representation is modified.

Scheme Procedure: **string-set!** str k chr [¶](#index-string_002dset_0021)

C Function: **scm\_string\_set\_x** (str, k, chr) [¶](#index-scm_005fstring_005fset_005fx)
:   Store chr in element k of str and return
    an unspecified value. k must be a valid index of
    str.

C Function: `void` **scm\_c\_string\_set\_x** `(SCM str, size_t k, SCM chr)` [¶](#index-scm_005fc_005fstring_005fset_005fx)
:   Like `scm_string_set_x`, but the index is given as a `size_t`.

Scheme Procedure: **string-fill!** str chr [start [end]] [¶](#index-string_002dfill_0021)

C Function: **scm\_substring\_fill\_x** (str, chr, start, end) [¶](#index-scm_005fsubstring_005ffill_005fx)

C Function: **scm\_string\_fill\_x** (str, chr) [¶](#index-scm_005fstring_005ffill_005fx)
:   Stores chr in every element of the given str and
    returns an unspecified value.

Scheme Procedure: **substring-fill!** str start end fill [¶](#index-substring_002dfill_0021)

C Function: **scm\_substring\_fill\_x** (str, start, end, fill) [¶](#index-scm_005fsubstring_005ffill_005fx-1)
:   Change every character in str between start and
    end to fill.

    ```
    (define y (string-copy "abcdefg"))
    (substring-fill! y 1 3 #\r)
    y
    ⇒ "arrdefg"
    ```

Scheme Procedure: **substring-move!** str1 start1 end1 str2 start2 [¶](#index-substring_002dmove_0021)

C Function: **scm\_substring\_move\_x** (str1, start1, end1, str2, start2) [¶](#index-scm_005fsubstring_005fmove_005fx)
:   Copy the substring of str1 bounded by start1 and end1
    into str2 beginning at position start2.
    str1 and str2 can be the same string.

Scheme Procedure: **string-copy!** target tstart s [start [end]] [¶](#index-string_002dcopy_0021)

C Function: **scm\_string\_copy\_x** (target, tstart, s, start, end) [¶](#index-scm_005fstring_005fcopy_005fx)
:   Copy the sequence of characters from index range [start,
    end) in string s to string target, beginning
    at index tstart. The characters are copied left-to-right
    or right-to-left as needed – the copy is guaranteed to work,
    even if target and s are the same string. It is an
    error if the copy operation runs off the end of the target
    string.

---

Next: [String Searching](#String-Searching), Previous: [String Modification](#String-Modification), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.7 String Comparison [¶](#String-Comparison-1)

The procedures in this section are similar to the character ordering
predicates (see [Characters](#Characters)), but are defined on character sequences.

The first set is specified in R5RS and has names that end in `?`.
The second set is specified in SRFI-13 and the names have not ending
`?`.

The predicates ending in `-ci` ignore the character case
when comparing strings. For now, case-insensitive comparison is done
using the R5RS rules, where every lower-case character that has a
single character upper-case form is converted to uppercase before
comparison. See See [the `(ice-9
i18n)` module](#Text-Collation), for locale-dependent string comparison.

Scheme Procedure: **string=?** s1 s2 s3 … [¶](#index-string_003d_003f)
:   Lexicographic equality predicate; return `#t` if all strings are
    the same length and contain the same characters in the same positions,
    otherwise return `#f`.

    The procedure `string-ci=?` treats upper and lower case
    letters as though they were the same character, but
    `string=?` treats upper and lower case as distinct
    characters.

Scheme Procedure: **string<?** s1 s2 s3 … [¶](#index-string_003c_003f)
:   Lexicographic ordering predicate; return `#t` if, for every pair of
    consecutive string arguments str\_i and str\_i+1, str\_i is
    lexicographically less than str\_i+1.

Scheme Procedure: **string<=?** s1 s2 s3 … [¶](#index-string_003c_003d_003f)
:   Lexicographic ordering predicate; return `#t` if, for every pair of
    consecutive string arguments str\_i and str\_i+1, str\_i is
    lexicographically less than or equal to str\_i+1.

Scheme Procedure: **string>?** s1 s2 s3 … [¶](#index-string_003e_003f)
:   Lexicographic ordering predicate; return `#t` if, for every pair of
    consecutive string arguments str\_i and str\_i+1, str\_i is
    lexicographically greater than str\_i+1.

Scheme Procedure: **string>=?** s1 s2 s3 … [¶](#index-string_003e_003d_003f)
:   Lexicographic ordering predicate; return `#t` if, for every pair of
    consecutive string arguments str\_i and str\_i+1, str\_i is
    lexicographically greater than or equal to str\_i+1.

Scheme Procedure: **string-ci=?** s1 s2 s3 … [¶](#index-string_002dci_003d_003f)
:   Case-insensitive string equality predicate; return `#t` if
    all strings are the same length and their component
    characters match (ignoring case) at each position; otherwise
    return `#f`.

Scheme Procedure: **string-ci<?** s1 s2 s3 … [¶](#index-string_002dci_003c_003f)
:   Case insensitive lexicographic ordering predicate; return `#t` if,
    for every pair of consecutive string arguments str\_i and
    str\_i+1, str\_i is lexicographically less than str\_i+1
    regardless of case.

Scheme Procedure: **string-ci<=?** s1 s2 s3 … [¶](#index-string_002dci_003c_003d_003f)
:   Case insensitive lexicographic ordering predicate; return `#t` if,
    for every pair of consecutive string arguments str\_i and
    str\_i+1, str\_i is lexicographically less than or equal to
    str\_i+1 regardless of case.

Scheme Procedure: **string-ci>?** s1 s2 s3 … [¶](#index-string_002dci_003e_003f)
:   Case insensitive lexicographic ordering predicate; return `#t` if,
    for every pair of consecutive string arguments str\_i and
    str\_i+1, str\_i is lexicographically greater than
    str\_i+1 regardless of case.

Scheme Procedure: **string-ci>=?** s1 s2 s3 … [¶](#index-string_002dci_003e_003d_003f)
:   Case insensitive lexicographic ordering predicate; return `#t` if,
    for every pair of consecutive string arguments str\_i and
    str\_i+1, str\_i is lexicographically greater than or equal to
    str\_i+1 regardless of case.

Scheme Procedure: **string-compare** s1 s2 proc\_lt proc\_eq proc\_gt [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dcompare)

C Function: **scm\_string\_compare** (s1, s2, proc\_lt, proc\_eq, proc\_gt, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fcompare)
:   Apply proc\_lt, proc\_eq, proc\_gt to the
    mismatch index, depending upon whether s1 is less than,
    equal to, or greater than s2. The mismatch index is the
    largest index i such that for every 0 <= j <
    i, s1[j] = s2[j] – that is,
    i is the first position that does not match.

Scheme Procedure: **string-compare-ci** s1 s2 proc\_lt proc\_eq proc\_gt [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dcompare_002dci)

C Function: **scm\_string\_compare\_ci** (s1, s2, proc\_lt, proc\_eq, proc\_gt, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fcompare_005fci)
:   Apply proc\_lt, proc\_eq, proc\_gt to the
    mismatch index, depending upon whether s1 is less than,
    equal to, or greater than s2. The mismatch index is the
    largest index i such that for every 0 <= j <
    i, s1[j] = s2[j] – that is,
    i is the first position where the lowercased letters
    do not match.

Scheme Procedure: **string=** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_003d)

C Function: **scm\_string\_eq** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005feq)
:   Return `#f` if s1 and s2 are not equal, a true
    value otherwise.

Scheme Procedure: **string<>** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_003c_003e)

C Function: **scm\_string\_neq** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fneq)
:   Return `#f` if s1 and s2 are equal, a true
    value otherwise.

Scheme Procedure: **string<** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_003c)

C Function: **scm\_string\_lt** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005flt)
:   Return `#f` if s1 is greater or equal to s2, a
    true value otherwise.

Scheme Procedure: **string>** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_003e)

C Function: **scm\_string\_gt** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fgt)
:   Return `#f` if s1 is less or equal to s2, a
    true value otherwise.

Scheme Procedure: **string<=** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_003c_003d)

C Function: **scm\_string\_le** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fle)
:   Return `#f` if s1 is greater to s2, a true
    value otherwise.

Scheme Procedure: **string>=** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_003e_003d)

C Function: **scm\_string\_ge** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fge)
:   Return `#f` if s1 is less to s2, a true value
    otherwise.

Scheme Procedure: **string-ci=** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dci_003d)

C Function: **scm\_string\_ci\_eq** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fci_005feq)
:   Return `#f` if s1 and s2 are not equal, a true
    value otherwise. The character comparison is done
    case-insensitively.

Scheme Procedure: **string-ci<>** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dci_003c_003e)

C Function: **scm\_string\_ci\_neq** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fci_005fneq)
:   Return `#f` if s1 and s2 are equal, a true
    value otherwise. The character comparison is done
    case-insensitively.

Scheme Procedure: **string-ci<** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dci_003c)

C Function: **scm\_string\_ci\_lt** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fci_005flt)
:   Return `#f` if s1 is greater or equal to s2, a
    true value otherwise. The character comparison is done
    case-insensitively.

Scheme Procedure: **string-ci>** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dci_003e)

C Function: **scm\_string\_ci\_gt** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fci_005fgt)
:   Return `#f` if s1 is less or equal to s2, a
    true value otherwise. The character comparison is done
    case-insensitively.

Scheme Procedure: **string-ci<=** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dci_003c_003d)

C Function: **scm\_string\_ci\_le** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fci_005fle)
:   Return `#f` if s1 is greater to s2, a true
    value otherwise. The character comparison is done
    case-insensitively.

Scheme Procedure: **string-ci>=** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dci_003e_003d)

C Function: **scm\_string\_ci\_ge** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fci_005fge)
:   Return `#f` if s1 is less to s2, a true value
    otherwise. The character comparison is done
    case-insensitively.

Scheme Procedure: **string-hash** s [bound [start [end]]] [¶](#index-string_002dhash)

C Function: **scm\_substring\_hash** (s, bound, start, end) [¶](#index-scm_005fsubstring_005fhash)
:   Compute a hash value for s. The optional argument bound is
    a non-negative exact integer specifying the range of the hash function.
    A positive value restricts the return value to the range [0,bound).

Scheme Procedure: **string-hash-ci** s [bound [start [end]]] [¶](#index-string_002dhash_002dci)

C Function: **scm\_substring\_hash\_ci** (s, bound, start, end) [¶](#index-scm_005fsubstring_005fhash_005fci)
:   Compute a hash value for s. The optional argument bound is
    a non-negative exact integer specifying the range of the hash function.
    A positive value restricts the return value to the range [0,bound).

Because the same visual appearance of an abstract Unicode character can
be obtained via multiple sequences of Unicode characters, even the
case-insensitive string comparison functions described above may return
`#f` when presented with strings containing different
representations of the same character. For example, the Unicode
character “LATIN SMALL LETTER S WITH DOT BELOW AND DOT ABOVE” can be
represented with a single character (U+1E69) or by the character “LATIN
SMALL LETTER S” (U+0073) followed by the combining marks “COMBINING
DOT BELOW” (U+0323) and “COMBINING DOT ABOVE” (U+0307).

For this reason, it is often desirable to ensure that the strings
to be compared are using a mutually consistent representation for every
character. The Unicode standard defines two methods of normalizing the
contents of strings: Decomposition, which breaks composite characters
into a set of constituent characters with an ordering defined by the
Unicode Standard; and composition, which performs the converse.

There are two decomposition operations. “Canonical decomposition”
produces character sequences that share the same visual appearance as
the original characters, while “compatibility decomposition” produces
ones whose visual appearances may differ from the originals but which
represent the same abstract character.

These operations are encapsulated in the following set of normalization
forms:

*NFD*
:   Characters are decomposed to their canonical forms.

*NFKD*
:   Characters are decomposed to their compatibility forms.

*NFC*
:   Characters are decomposed to their canonical forms, then composed.

*NFKC*
:   Characters are decomposed to their compatibility forms, then composed.

The functions below put their arguments into one of the forms described
above.

Scheme Procedure: **string-normalize-nfd** s [¶](#index-string_002dnormalize_002dnfd)

C Function: **scm\_string\_normalize\_nfd** (s) [¶](#index-scm_005fstring_005fnormalize_005fnfd)
:   Return the `NFD` normalized form of s.

Scheme Procedure: **string-normalize-nfkd** s [¶](#index-string_002dnormalize_002dnfkd)

C Function: **scm\_string\_normalize\_nfkd** (s) [¶](#index-scm_005fstring_005fnormalize_005fnfkd)
:   Return the `NFKD` normalized form of s.

Scheme Procedure: **string-normalize-nfc** s [¶](#index-string_002dnormalize_002dnfc)

C Function: **scm\_string\_normalize\_nfc** (s) [¶](#index-scm_005fstring_005fnormalize_005fnfc)
:   Return the `NFC` normalized form of s.

Scheme Procedure: **string-normalize-nfkc** s [¶](#index-string_002dnormalize_002dnfkc)

C Function: **scm\_string\_normalize\_nfkc** (s) [¶](#index-scm_005fstring_005fnormalize_005fnfkc)
:   Return the `NFKC` normalized form of s.

---

Next: [Alphabetic Case Mapping](#Alphabetic-Case-Mapping), Previous: [String Comparison](#String-Comparison), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.8 String Searching [¶](#String-Searching-1)

Scheme Procedure: **string-index** s char\_pred [start [end]] [¶](#index-string_002dindex)

C Function: **scm\_string\_index** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005findex)
:   Search through the string s from left to right, returning
    the index of the first occurrence of a character which

    * equals char\_pred, if it is character,
    * satisfies the predicate char\_pred, if it is a procedure,
    * is in the set char\_pred, if it is a character set.

    Return `#f` if no match is found.

Scheme Procedure: **string-rindex** s char\_pred [start [end]] [¶](#index-string_002drindex)

C Function: **scm\_string\_rindex** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005frindex)
:   Search through the string s from right to left, returning
    the index of the last occurrence of a character which

    * equals char\_pred, if it is character,
    * satisfies the predicate char\_pred, if it is a procedure,
    * is in the set if char\_pred is a character set.

    Return `#f` if no match is found.

Scheme Procedure: **string-prefix-length** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dprefix_002dlength)

C Function: **scm\_string\_prefix\_length** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fprefix_005flength)
:   Return the length of the longest common prefix of the two
    strings.

Scheme Procedure: **string-prefix-length-ci** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dprefix_002dlength_002dci)

C Function: **scm\_string\_prefix\_length\_ci** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fprefix_005flength_005fci)
:   Return the length of the longest common prefix of the two
    strings, ignoring character case.

Scheme Procedure: **string-suffix-length** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dsuffix_002dlength)

C Function: **scm\_string\_suffix\_length** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fsuffix_005flength)
:   Return the length of the longest common suffix of the two
    strings.

Scheme Procedure: **string-suffix-length-ci** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dsuffix_002dlength_002dci)

C Function: **scm\_string\_suffix\_length\_ci** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fsuffix_005flength_005fci)
:   Return the length of the longest common suffix of the two
    strings, ignoring character case.

Scheme Procedure: **string-prefix?** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dprefix_003f)

C Function: **scm\_string\_prefix\_p** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fprefix_005fp)
:   Is s1 a prefix of s2?

Scheme Procedure: **string-prefix-ci?** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dprefix_002dci_003f)

C Function: **scm\_string\_prefix\_ci\_p** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fprefix_005fci_005fp)
:   Is s1 a prefix of s2, ignoring character case?

Scheme Procedure: **string-suffix?** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dsuffix_003f)

C Function: **scm\_string\_suffix\_p** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fsuffix_005fp)
:   Is s1 a suffix of s2?

Scheme Procedure: **string-suffix-ci?** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dsuffix_002dci_003f)

C Function: **scm\_string\_suffix\_ci\_p** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fsuffix_005fci_005fp)
:   Is s1 a suffix of s2, ignoring character case?

Scheme Procedure: **string-index-right** s char\_pred [start [end]] [¶](#index-string_002dindex_002dright)

C Function: **scm\_string\_index\_right** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005findex_005fright)
:   Search through the string s from right to left, returning
    the index of the last occurrence of a character which

    * equals char\_pred, if it is character,
    * satisfies the predicate char\_pred, if it is a procedure,
    * is in the set if char\_pred is a character set.

    Return `#f` if no match is found.

Scheme Procedure: **string-skip** s char\_pred [start [end]] [¶](#index-string_002dskip)

C Function: **scm\_string\_skip** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005fskip)
:   Search through the string s from left to right, returning
    the index of the first occurrence of a character which

    * does not equal char\_pred, if it is character,
    * does not satisfy the predicate char\_pred, if it is a
      procedure,
    * is not in the set if char\_pred is a character set.

Scheme Procedure: **string-skip-right** s char\_pred [start [end]] [¶](#index-string_002dskip_002dright)

C Function: **scm\_string\_skip\_right** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005fskip_005fright)
:   Search through the string s from right to left, returning
    the index of the last occurrence of a character which

    * does not equal char\_pred, if it is character,
    * does not satisfy the predicate char\_pred, if it is a
      procedure,
    * is not in the set if char\_pred is a character set.

Scheme Procedure: **string-count** s char\_pred [start [end]] [¶](#index-string_002dcount)

C Function: **scm\_string\_count** (s, char\_pred, start, end) [¶](#index-scm_005fstring_005fcount)
:   Return the count of the number of characters in the string
    s which

    * equals char\_pred, if it is character,
    * satisfies the predicate char\_pred, if it is a procedure.
    * is in the set char\_pred, if it is a character set.

Scheme Procedure: **string-contains** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dcontains)

C Function: **scm\_string\_contains** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fcontains)
:   Does string s1 contain string s2? Return the index
    in s1 where s2 occurs as a substring, or false.
    The optional start/end indices restrict the operation to the
    indicated substrings.

Scheme Procedure: **string-contains-ci** s1 s2 [start1 [end1 [start2 [end2]]]] [¶](#index-string_002dcontains_002dci)

C Function: **scm\_string\_contains\_ci** (s1, s2, start1, end1, start2, end2) [¶](#index-scm_005fstring_005fcontains_005fci)
:   Does string s1 contain string s2? Return the index
    in s1 where s2 occurs as a substring, or false.
    The optional start/end indices restrict the operation to the
    indicated substrings. Character comparison is done
    case-insensitively.

---

Next: [Reversing and Appending Strings](#Reversing-and-Appending-Strings), Previous: [String Searching](#String-Searching), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.9 Alphabetic Case Mapping [¶](#Alphabetic-Case-Mapping-1)

These are procedures for mapping strings to their upper- or lower-case
equivalents, respectively, or for capitalizing strings.

They use the basic case mapping rules for Unicode characters. No
special language or context rules are considered. The resulting strings
are guaranteed to be the same length as the input strings.

See [the `(ice-9
i18n)` module](#Character-Case-Mapping), for locale-dependent case conversions.

Scheme Procedure: **string-upcase** str [start [end]] [¶](#index-string_002dupcase)

C Function: **scm\_substring\_upcase** (str, start, end) [¶](#index-scm_005fsubstring_005fupcase)

C Function: **scm\_string\_upcase** (str) [¶](#index-scm_005fstring_005fupcase)
:   Upcase every character in `str`.

Scheme Procedure: **string-upcase!** str [start [end]] [¶](#index-string_002dupcase_0021)

C Function: **scm\_substring\_upcase\_x** (str, start, end) [¶](#index-scm_005fsubstring_005fupcase_005fx)

C Function: **scm\_string\_upcase\_x** (str) [¶](#index-scm_005fstring_005fupcase_005fx)
:   Destructively upcase every character in `str`.

    ```
    (string-upcase! y)
    ⇒ "ARRDEFG"
    y
    ⇒ "ARRDEFG"
    ```

Scheme Procedure: **string-downcase** str [start [end]] [¶](#index-string_002ddowncase)

C Function: **scm\_substring\_downcase** (str, start, end) [¶](#index-scm_005fsubstring_005fdowncase)

C Function: **scm\_string\_downcase** (str) [¶](#index-scm_005fstring_005fdowncase)
:   Downcase every character in str.

Scheme Procedure: **string-downcase!** str [start [end]] [¶](#index-string_002ddowncase_0021)

C Function: **scm\_substring\_downcase\_x** (str, start, end) [¶](#index-scm_005fsubstring_005fdowncase_005fx)

C Function: **scm\_string\_downcase\_x** (str) [¶](#index-scm_005fstring_005fdowncase_005fx)
:   Destructively downcase every character in str.

    ```
    y
    ⇒ "ARRDEFG"
    (string-downcase! y)
    ⇒ "arrdefg"
    y
    ⇒ "arrdefg"
    ```

Scheme Procedure: **string-capitalize** str [¶](#index-string_002dcapitalize)

C Function: **scm\_string\_capitalize** (str) [¶](#index-scm_005fstring_005fcapitalize)
:   Return a freshly allocated string with the characters in
    str, where the first character of every word is
    capitalized.

Scheme Procedure: **string-capitalize!** str [¶](#index-string_002dcapitalize_0021)

C Function: **scm\_string\_capitalize\_x** (str) [¶](#index-scm_005fstring_005fcapitalize_005fx)
:   Upcase the first character of every word in str
    destructively and return str.

    ```
    y                      ⇒ "hello world"
    (string-capitalize! y) ⇒ "Hello World"
    y                      ⇒ "Hello World"
    ```

Scheme Procedure: **string-titlecase** str [start [end]] [¶](#index-string_002dtitlecase)

C Function: **scm\_string\_titlecase** (str, start, end) [¶](#index-scm_005fstring_005ftitlecase)
:   Titlecase every first character in a word in str.

Scheme Procedure: **string-titlecase!** str [start [end]] [¶](#index-string_002dtitlecase_0021)

C Function: **scm\_string\_titlecase\_x** (str, start, end) [¶](#index-scm_005fstring_005ftitlecase_005fx)
:   Destructively titlecase every first character in a word in
    str.

---

Next: [Mapping, Folding, and Unfolding](#Mapping-Folding-and-Unfolding), Previous: [Alphabetic Case Mapping](#Alphabetic-Case-Mapping), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.10 Reversing and Appending Strings [¶](#Reversing-and-Appending-Strings-1)

Scheme Procedure: **string-reverse** str [start [end]] [¶](#index-string_002dreverse)

C Function: **scm\_string\_reverse** (str, start, end) [¶](#index-scm_005fstring_005freverse)
:   Reverse the string str. The optional arguments
    start and end delimit the region of str to
    operate on.

Scheme Procedure: **string-reverse!** str [start [end]] [¶](#index-string_002dreverse_0021)

C Function: **scm\_string\_reverse\_x** (str, start, end) [¶](#index-scm_005fstring_005freverse_005fx)
:   Reverse the string str in-place. The optional arguments
    start and end delimit the region of str to
    operate on. The return value is unspecified.

Scheme Procedure: **string-append** arg … [¶](#index-string_002dappend)

C Function: **scm\_string\_append** (args) [¶](#index-scm_005fstring_005fappend)
:   Return a newly allocated string whose characters form the
    concatenation of the given strings, arg ....

    ```
    (let ((h "hello "))
      (string-append h "world"))
    ⇒ "hello world"
    ```

Scheme Procedure: **string-append/shared** arg … [¶](#index-string_002dappend_002fshared)

C Function: **scm\_string\_append\_shared** (args) [¶](#index-scm_005fstring_005fappend_005fshared)
:   Like `string-append`, but the result may share memory
    with the argument strings.

Scheme Procedure: **string-concatenate** ls [¶](#index-string_002dconcatenate)

C Function: **scm\_string\_concatenate** (ls) [¶](#index-scm_005fstring_005fconcatenate)
:   Append the elements (which must be strings) of ls together into a
    single string. Guaranteed to return a freshly allocated string.

Scheme Procedure: **string-concatenate-reverse** ls [final\_string [end]] [¶](#index-string_002dconcatenate_002dreverse)

C Function: **scm\_string\_concatenate\_reverse** (ls, final\_string, end) [¶](#index-scm_005fstring_005fconcatenate_005freverse)
:   Without optional arguments, this procedure is equivalent to

    ```
    (string-concatenate (reverse ls))
    ```

    If the optional argument final\_string is specified, it is
    consed onto the beginning to ls before performing the
    list-reverse and string-concatenate operations. If end
    is given, only the characters of final\_string up to index
    end are used.

    Guaranteed to return a freshly allocated string.

Scheme Procedure: **string-concatenate/shared** ls [¶](#index-string_002dconcatenate_002fshared)

C Function: **scm\_string\_concatenate\_shared** (ls) [¶](#index-scm_005fstring_005fconcatenate_005fshared)
:   Like `string-concatenate`, but the result may share memory
    with the strings in the list ls.

Scheme Procedure: **string-concatenate-reverse/shared** ls [final\_string [end]] [¶](#index-string_002dconcatenate_002dreverse_002fshared)

C Function: **scm\_string\_concatenate\_reverse\_shared** (ls, final\_string, end) [¶](#index-scm_005fstring_005fconcatenate_005freverse_005fshared)
:   Like `string-concatenate-reverse`, but the result may
    share memory with the strings in the ls arguments.

---

Next: [Miscellaneous String Operations](#Miscellaneous-String-Operations), Previous: [Reversing and Appending Strings](#Reversing-and-Appending-Strings), Up: [Strings](#Strings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.6.5.11 Mapping, Folding, and Unfolding [¶](#Mapping_002c-Folding_002c-and-Unfolding)

Scheme Procedure: **string-map** proc s [start [end]] [¶](#index-string_002dmap)

C Function: **scm\_string\_map** (proc, s, start, end) [¶](#index-scm_005fstring_005fmap)
:   proc is a char->char procedure, it is mapped over
    s. The order in which the procedure is applied to the
    string elements is not specified.

Scheme Procedure: **string-map!** proc s [start [end]] [¶](#index-string_002dmap_0021)

C Function: **scm\_string\_map\_x** (proc, s, start, end) [¶](#index-scm_005fstring_005fmap_005fx)
:   proc is a char->char procedure, it is mapped over
    s. The order in which the procedure is applied to the
    string elements is not specified. The string s is
    modified in-place, the return value is not specified.

Scheme Procedure: **string-for-each** proc s [start [end]] [¶](#index-string_002dfor_002deach)

C Function: **scm\_string\_for\_each** (proc, s, start, end) [¶](#index-scm_005fstring_005ffor_005feach)
:   proc is mapped over s in left-to-right order. The
    return value is not specified.

Scheme Procedure: **string-for-each-index** proc s [start [end]] [¶](#index-string_002dfor_002deach_002dindex)

C Function: **scm\_string\_for\_each\_index** (proc, s, start, end) [¶](#index-scm_005fstring_005ffor_005feach_005findex)
:   Call `(proc i)` for each index i in s, from left to
    right.

    For example, to change characters to alternately upper and lower case,

    ```
    (define str (string-copy "studly"))
    (string-for-each-index
        (lambda (i)
          (string-set! str i
            ((if (even? i) char-upcase char-downcase)
             (string-ref str i))))
        str)
    str ⇒ "StUdLy"
    ```

Scheme Procedure: **string-fold** kons knil s [start [end]] [¶](#index-string_002dfold)

C Function: **scm\_string\_fold** (kons, knil, s, start, end) [¶](#index-scm_005fstring_005ffold)
:   Fold kons over the characters of s, with knil
    as the terminating element, from left to right. kons
    must expect two arguments: The actual character and the last
    result of kons’ application.

Scheme Procedure: **string-fold-right** kons knil s [start [end]] [¶](#index-string_002dfold_002dright)

C Function: **scm\_string\_fold\_right** (kons, knil, s, start, end) [¶](#index-scm_005fstring_005ffold_005fright)
:   Fold kons over the characters of s, with knil
    as the terminating element, from right to left. kons
    must expect two arguments: The actual character and the last
    result of kons’ application.

Scheme Procedure: **string-unfold** p f g seed [base [make\_final]] [¶](#index-string_002dunfold)
