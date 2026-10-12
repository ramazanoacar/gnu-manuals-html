### 6.25 Support for Internationalization [¶](#Support-for-Internationalization)

Guile provides internationalization[23](#FOOT23)
support for Scheme programs in two ways. First, procedures to
manipulate text and data in a way that conforms to particular cultural
conventions (i.e., in a “locale-dependent” way) are provided in the
`(ice-9 i18n)`. Second, Guile allows the use of GNU
`gettext` to translate program message strings.

* [Internationalization with Guile](#i18n-Introduction)
* [Text Collation](#Text-Collation)
* [Character Case Mapping](#Character-Case-Mapping)
* [Number Input and Output](#Number-Input-and-Output)
* [Accessing Locale Information](#Accessing-Locale-Information)
* [Gettext Support](#Gettext-Support)

---

Next: [Text Collation](#Text-Collation), Previous: [Support for Internationalization](#Internationalization), Up: [Support for Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.25.1 Internationalization with Guile [¶](#Internationalization-with-Guile)

In order to make use of the functions described thereafter, the
`(ice-9 i18n)` module must be imported in the usual way:

```
(use-modules (ice-9 i18n))
```

The `(ice-9 i18n)` module provides procedures to manipulate text
and other data in a way that conforms to the cultural conventions
chosen by the user. Each region of the world or language has its own
customs to, for instance, represent real numbers, classify characters,
collate text, etc. All these aspects comprise the so-called
“cultural conventions” of that region or language.

Computer systems typically refer to a set of cultural conventions as a
*locale*. For each particular aspect that comprise those cultural
conventions, a *locale category* is defined. For instance, the
way characters are classified is defined by the `LC_CTYPE`
category, while the language in which program messages are issued to
the user is defined by the `LC_MESSAGES` category
(see [General Locale Information](#Locales) for details).

The procedures provided by this module allow the development of
programs that adapt automatically to any locale setting. As we will
see later, many of these procedures can optionally take a *locale
object* argument. This additional argument defines the locale
settings that must be followed by the invoked procedure. When it is
omitted, then the current locale settings of the process are followed
(see [`setlocale`](#Locales)).

The following procedures allow the manipulation of such locale
objects.

Scheme Procedure: **make-locale** category-list locale-name [base-locale] [¶](#index-make_002dlocale)

C Function: **scm\_make\_locale** (category\_list, locale\_name, base\_locale) [¶](#index-scm_005fmake_005flocale)
:   Return a reference to a data structure representing a set of locale
    datasets. locale-name should be a string denoting a particular
    locale (e.g., `"aa_DJ"`) and category-list should be either
    a list of locale categories or a single category as used with
    `setlocale` (see [`setlocale`](#Locales)). Optionally, if
    `base-locale` is passed, it should be a locale object denoting
    settings for categories not listed in category-list.

    The following invocation creates a locale object that combines the use
    of Swedish for messages and character classification with the
    default settings for the other categories (i.e., the settings of the
    default `C` locale which usually represents conventions in use in
    the USA):

    ```
    (make-locale (list LC_MESSAGES LC_CTYPE) "sv_SE")
    ```

    The following example combines the use of Esperanto messages and
    conventions with monetary conventions from Croatia:

    ```
    (make-locale LC_MONETARY "hr_HR"
                 (make-locale LC_ALL "eo_EO"))
    ```

    A `system-error` exception (see [How to Handle Errors](#Handling-Errors)) is raised by
    `make-locale` when locale-name does not match any of the
    locales compiled on the system. Note that on non-GNU systems, this
    error may be raised later, when the locale object is actually used.

Scheme Procedure: **locale?** obj [¶](#index-locale_003f)

C Function: **scm\_locale\_p** (obj) [¶](#index-scm_005flocale_005fp)
:   Return true if obj is a locale object.

Scheme Variable: **%global-locale** [¶](#index-_0025global_002dlocale)

C Variable: **scm\_global\_locale** [¶](#index-scm_005fglobal_005flocale)
:   This variable is bound to a locale object denoting the current process
    locale as installed using `setlocale ()` (see [Locales](#Locales)). It
    may be used like any other locale object, including as a third
    argument to `make-locale`, for instance.

---

Next: [Character Case Mapping](#Character-Case-Mapping), Previous: [Internationalization with Guile](#i18n-Introduction), Up: [Support for Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.25.2 Text Collation [¶](#Text-Collation-1)

The following procedures provide support for text collation, i.e.,
locale-dependent string and character sorting.

Scheme Procedure: **string-locale<?** s1 s2 [locale] [¶](#index-string_002dlocale_003c_003f)

C Function: **scm\_string\_locale\_lt** (s1, s2, locale) [¶](#index-scm_005fstring_005flocale_005flt)

Scheme Procedure: **string-locale>?** s1 s2 [locale] [¶](#index-string_002dlocale_003e_003f)

C Function: **scm\_string\_locale\_gt** (s1, s2, locale) [¶](#index-scm_005fstring_005flocale_005fgt)

Scheme Procedure: **string-locale-ci<?** s1 s2 [locale] [¶](#index-string_002dlocale_002dci_003c_003f)

C Function: **scm\_string\_locale\_ci\_lt** (s1, s2, locale) [¶](#index-scm_005fstring_005flocale_005fci_005flt)

Scheme Procedure: **string-locale-ci>?** s1 s2 [locale] [¶](#index-string_002dlocale_002dci_003e_003f)

C Function: **scm\_string\_locale\_ci\_gt** (s1, s2, locale) [¶](#index-scm_005fstring_005flocale_005fci_005fgt)
:   Compare strings s1 and s2 in a locale-dependent way. If
    locale is provided, it should be locale object (as returned by
    `make-locale`) and will be used to perform the comparison;
    otherwise, the current system locale is used. For the `-ci`
    variants, the comparison is made in a case-insensitive way.

Scheme Procedure: **string-locale-ci=?** s1 s2 [locale] [¶](#index-string_002dlocale_002dci_003d_003f)

C Function: **scm\_string\_locale\_ci\_eq** (s1, s2, locale) [¶](#index-scm_005fstring_005flocale_005fci_005feq)
:   Compare strings s1 and s2 in a case-insensitive, and
    locale-dependent way. If locale is provided, it should be
    a locale object (as returned by `make-locale`) and will be used to
    perform the comparison; otherwise, the current system locale is used.

Scheme Procedure: **char-locale<?** c1 c2 [locale] [¶](#index-char_002dlocale_003c_003f)

C Function: **scm\_char\_locale\_lt** (c1, c2, locale) [¶](#index-scm_005fchar_005flocale_005flt)

Scheme Procedure: **char-locale>?** c1 c2 [locale] [¶](#index-char_002dlocale_003e_003f)

C Function: **scm\_char\_locale\_gt** (c1, c2, locale) [¶](#index-scm_005fchar_005flocale_005fgt)

Scheme Procedure: **char-locale-ci<?** c1 c2 [locale] [¶](#index-char_002dlocale_002dci_003c_003f)

C Function: **scm\_char\_locale\_ci\_lt** (c1, c2, locale) [¶](#index-scm_005fchar_005flocale_005fci_005flt)

Scheme Procedure: **char-locale-ci>?** c1 c2 [locale] [¶](#index-char_002dlocale_002dci_003e_003f)

C Function: **scm\_char\_locale\_ci\_gt** (c1, c2, locale) [¶](#index-scm_005fchar_005flocale_005fci_005fgt)
:   Compare characters c1 and c2 according to either
    locale (a locale object as returned by `make-locale`) or
    the current locale. For the `-ci` variants, the comparison is
    made in a case-insensitive way.

Scheme Procedure: **char-locale-ci=?** c1 c2 [locale] [¶](#index-char_002dlocale_002dci_003d_003f)

C Function: **scm\_char\_locale\_ci\_eq** (c1, c2, locale) [¶](#index-scm_005fchar_005flocale_005fci_005feq)
:   Return true if character c1 is equal to c2, in a case
    insensitive way according to locale or to the current locale.

---

Next: [Number Input and Output](#Number-Input-and-Output), Previous: [Text Collation](#Text-Collation), Up: [Support for Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.25.3 Character Case Mapping [¶](#Character-Case-Mapping-1)

The procedures below provide support for “character case mapping”,
i.e., to convert characters or strings to their upper-case or
lower-case equivalent. Note that SRFI-13 provides procedures that
look similar (see [Alphabetic Case Mapping](#Alphabetic-Case-Mapping)). However, the SRFI-13
procedures are locale-independent. Therefore, they do not take into
account specificities of the customs in use in a particular language
or region of the world. For instance, while most languages using the
Latin alphabet map lower-case letter “i” to upper-case letter “I”,
Turkish maps lower-case “i” to “Latin capital letter I with dot
above”. The following procedures allow programmers to provide
idiomatic character mapping.

Scheme Procedure: **char-locale-downcase** chr [locale] [¶](#index-char_002dlocale_002ddowncase)

C Function: **scm\_char\_locale\_upcase** (chr, locale) [¶](#index-scm_005fchar_005flocale_005fupcase)
:   Return the lowercase character that corresponds to chr according
    to either locale or the current locale.

Scheme Procedure: **char-locale-upcase** chr [locale] [¶](#index-char_002dlocale_002dupcase)

C Function: **scm\_char\_locale\_downcase** (chr, locale) [¶](#index-scm_005fchar_005flocale_005fdowncase)
:   Return the uppercase character that corresponds to chr according
    to either locale or the current locale.

Scheme Procedure: **char-locale-titlecase** chr [locale] [¶](#index-char_002dlocale_002dtitlecase)

C Function: **scm\_char\_locale\_titlecase** (chr, locale) [¶](#index-scm_005fchar_005flocale_005ftitlecase)
:   Return the titlecase character that corresponds to chr according
    to either locale or the current locale.

Scheme Procedure: **string-locale-upcase** str [locale] [¶](#index-string_002dlocale_002dupcase)

C Function: **scm\_string\_locale\_upcase** (str, locale) [¶](#index-scm_005fstring_005flocale_005fupcase)
:   Return a new string that is the uppercase version of str
    according to either locale or the current locale.

Scheme Procedure: **string-locale-downcase** str [locale] [¶](#index-string_002dlocale_002ddowncase)

C Function: **scm\_string\_locale\_downcase** (str, locale) [¶](#index-scm_005fstring_005flocale_005fdowncase)
:   Return a new string that is the down-case version of str
    according to either locale or the current locale.

Scheme Procedure: **string-locale-titlecase** str [locale] [¶](#index-string_002dlocale_002dtitlecase)

C Function: **scm\_string\_locale\_titlecase** (str, locale) [¶](#index-scm_005fstring_005flocale_005ftitlecase)
:   Return a new string that is the titlecase version of str
    according to either locale or the current locale.

---

Next: [Accessing Locale Information](#Accessing-Locale-Information), Previous: [Character Case Mapping](#Character-Case-Mapping), Up: [Support for Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.25.4 Number Input and Output [¶](#Number-Input-and-Output-1)

The following procedures allow programs to read and write numbers
written according to a particular locale. As an example, in English,
“ten thousand and a half” is usually written `10,000.5` while
in French it is written `10 000,5`. These procedures allow such
differences to be taken into account.

Scheme Procedure: **locale-string->integer** str [base [locale]] [¶](#index-locale_002dstring_002d_003einteger)

C Function: **scm\_locale\_string\_to\_integer** (str, base, locale) [¶](#index-scm_005flocale_005fstring_005fto_005finteger)
:   Convert string str into an integer according to either
    locale (a locale object as returned by `make-locale`) or
    the current process locale. If base is specified, then it
    determines the base of the integer being read (e.g., `16` for an
    hexadecimal number, `10` for a decimal number); by default,
    decimal numbers are read. Return two values (see [Returning and Accepting Multiple Values](#Multiple-Values)): an integer (on success) or `#f`, and the number of
    characters read from str (`0` on failure).

    This function is based on the C library’s `strtol` function
    (see [`strtol`](/libc/latest/en/libc.html#Parsing-of-Integers) in The GNU C Library
    Reference Manual).

Scheme Procedure: **locale-string->inexact** str [locale] [¶](#index-locale_002dstring_002d_003einexact)

C Function: **scm\_locale\_string\_to\_inexact** (str, locale) [¶](#index-scm_005flocale_005fstring_005fto_005finexact)
:   Convert string str into an inexact number according to either
    locale (a locale object as returned by `make-locale`) or
    the current process locale. Return two values (see [Returning and Accepting Multiple Values](#Multiple-Values)): an inexact number (on success) or `#f`, and the number
    of characters read from str (`0` on failure).

    This function is based on the C library’s `strtod` function
    (see [`strtod`](/libc/latest/en/libc.html#Parsing-of-Floats) in The GNU C Library
    Reference Manual).

Scheme Procedure: **number->locale-string** number [fraction-digits [locale]] [¶](#index-number_002d_003elocale_002dstring)
:   Convert number (an inexact) into a string according to the
    cultural conventions of either locale (a locale object) or the
    current locale. By default, print as many fractional digits as
    necessary, up to an upper bound. Optionally, fraction-digits may
    be bound to an integer specifying the number of fractional digits to be
    displayed.

Scheme Procedure: **monetary-amount->locale-string** amount intl? [locale] [¶](#index-monetary_002damount_002d_003elocale_002dstring)
:   Convert amount (an inexact denoting a monetary amount) into a
    string according to the cultural conventions of either locale (a
    locale object) or the current locale. If intl? is true, then
    the international monetary format for the given locale is used
    (see [international and locale monetary formats](/libc/latest/en/libc.html#Currency-Symbol) in The GNU C Library Reference Manual).

---

Next: [Gettext Support](#Gettext-Support), Previous: [Number Input and Output](#Number-Input-and-Output), Up: [Support for Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.25.5 Accessing Locale Information [¶](#Accessing-Locale-Information-1)

It is sometimes useful to obtain very specific information about a
locale such as the word it uses for days or months, its format for
representing floating-point figures, etc. The `(ice-9 i18n)`
module provides support for this in a way that is similar to the libc
functions `nl_langinfo ()` and `localeconv ()`
(see [accessing locale information from C](/libc/latest/en/libc.html#Locale-Information) in The GNU C Library Reference Manual). The available functions
are listed below.

Scheme Procedure: **locale-encoding** [locale] [¶](#index-locale_002dencoding)
:   Return the name of the encoding (a string whose interpretation is
    system-dependent) of either locale or the current locale.

The following functions deal with dates and times.

Scheme Procedure: **locale-day** day [locale] [¶](#index-locale_002dday)

Scheme Procedure: **locale-day-short** day [locale] [¶](#index-locale_002dday_002dshort)

Scheme Procedure: **locale-month** month [locale] [¶](#index-locale_002dmonth)

Scheme Procedure: **locale-month-short** month [locale] [¶](#index-locale_002dmonth_002dshort)
:   Return the word (a string) used in either locale or the current
    locale to name the day (or month) denoted by day (or
    month), an integer between 1 and 7 (or 1 and 12). The
    `-short` variants provide an abbreviation instead of a full name.

Scheme Procedure: **locale-am-string** [locale] [¶](#index-locale_002dam_002dstring)

Scheme Procedure: **locale-pm-string** [locale] [¶](#index-locale_002dpm_002dstring)
:   Return a (potentially empty) string that is used to denote *ante
    meridiem* (or *post meridiem*) hours in 12-hour format.

Scheme Procedure: **locale-date+time-format** [locale] [¶](#index-locale_002ddate_002btime_002dformat)

Scheme Procedure: **locale-date-format** [locale] [¶](#index-locale_002ddate_002dformat)

Scheme Procedure: **locale-time-format** [locale] [¶](#index-locale_002dtime_002dformat)

Scheme Procedure: **locale-time+am/pm-format** [locale] [¶](#index-locale_002dtime_002bam_002fpm_002dformat)

Scheme Procedure: **locale-era-date-format** [locale] [¶](#index-locale_002dera_002ddate_002dformat)

Scheme Procedure: **locale-era-date+time-format** [locale] [¶](#index-locale_002dera_002ddate_002btime_002dformat)

Scheme Procedure: **locale-era-time-format** [locale] [¶](#index-locale_002dera_002dtime_002dformat)
:   These procedures return format strings suitable to `strftime`
    (see [Time](#Time)) that may be used to display (part of) a date/time
    according to certain constraints and to the conventions of either
    locale or the current locale (see [the `nl_langinfo ()` items](/libc/latest/en/libc.html#The-Elegant-and-Fast-Way) in The GNU C Library Reference
    Manual).

Scheme Procedure: **locale-era** [locale] [¶](#index-locale_002dera)

Scheme Procedure: **locale-era-year** [locale] [¶](#index-locale_002dera_002dyear)
:   These functions return, respectively, the era and the year of the
    relevant era used in locale or the current locale. Most locales
    do not define this value. In this case, the empty string is returned.
    An example of a locale that does define this value is the Japanese
    one.

The following procedures give information about number representation.

Scheme Procedure: **locale-decimal-point** [locale] [¶](#index-locale_002ddecimal_002dpoint)

Scheme Procedure: **locale-thousands-separator** [locale] [¶](#index-locale_002dthousands_002dseparator)
:   These functions return a string denoting the representation of the
    decimal point or that of the thousand separator (respectively) for
    either locale or the current locale.

Scheme Procedure: **locale-digit-grouping** [locale] [¶](#index-locale_002ddigit_002dgrouping)
:   Return a (potentially circular) list of integers denoting how digits
    of the integer part of a number are to be grouped, starting at the
    decimal point and going to the left. The list contains integers
    indicating the size of the successive groups, from right to left. If
    the list is non-circular, then no grouping occurs for digits beyond
    the last group.

    For instance, if the returned list is a circular list that contains
    only `3` and the thousand separator is `","` (as is the case
    with English locales), then the number `12345678` should be
    printed `12,345,678`.

The following procedures deal with the representation of monetary
amounts. Some of them take an additional intl? argument (a
boolean) that tells whether the international or local monetary
conventions for the given locale are to be used.

Scheme Procedure: **locale-monetary-decimal-point** [locale] [¶](#index-locale_002dmonetary_002ddecimal_002dpoint)

Scheme Procedure: **locale-monetary-thousands-separator** [locale] [¶](#index-locale_002dmonetary_002dthousands_002dseparator)

Scheme Procedure: **locale-monetary-grouping** [locale] [¶](#index-locale_002dmonetary_002dgrouping)
:   These are the monetary counterparts of the above procedures. These
    procedures apply to monetary amounts.

Scheme Procedure: **locale-currency-symbol** intl? [locale] [¶](#index-locale_002dcurrency_002dsymbol)
:   Return the currency symbol (a string) of either locale or the
    current locale.

    The following example illustrates the difference between the local and
    international monetary formats:

    ```
    (define us (make-locale LC_MONETARY "en_US"))
    (locale-currency-symbol #f us)
    ⇒ "-$"
    (locale-currency-symbol #t us)
    ⇒ "USD "
    ```

Scheme Procedure: **locale-monetary-fractional-digits** intl? [locale] [¶](#index-locale_002dmonetary_002dfractional_002ddigits)
:   Return the number of fractional digits to be used when printing
    monetary amounts according to either locale or the current
    locale. If the locale does not specify it, then `#f` is
    returned.

Scheme Procedure: **locale-currency-symbol-precedes-positive?** intl? [locale] [¶](#index-locale_002dcurrency_002dsymbol_002dprecedes_002dpositive_003f)

Scheme Procedure: **locale-currency-symbol-precedes-negative?** intl? [locale] [¶](#index-locale_002dcurrency_002dsymbol_002dprecedes_002dnegative_003f)

Scheme Procedure: **locale-positive-separated-by-space?** intl? [locale] [¶](#index-locale_002dpositive_002dseparated_002dby_002dspace_003f)

Scheme Procedure: **locale-negative-separated-by-space?** intl? [locale] [¶](#index-locale_002dnegative_002dseparated_002dby_002dspace_003f)
:   These procedures return a boolean indicating whether the currency
    symbol should precede a positive/negative number, and whether a
    whitespace should be inserted between the currency symbol and a
    positive/negative amount.

Scheme Procedure: **locale-monetary-positive-sign** [locale] [¶](#index-locale_002dmonetary_002dpositive_002dsign)

Scheme Procedure: **locale-monetary-negative-sign** [locale] [¶](#index-locale_002dmonetary_002dnegative_002dsign)
:   Return a string denoting the positive (respectively negative) sign
    that should be used when printing a monetary amount.

Scheme Procedure: **locale-positive-sign-position** [¶](#index-locale_002dpositive_002dsign_002dposition)

Scheme Procedure: **locale-negative-sign-position** [¶](#index-locale_002dnegative_002dsign_002dposition)
:   These functions return a symbol telling where a sign of a
    positive/negative monetary amount is to appear when printing it. The
    possible values are:

    `parenthesize`
    :   The currency symbol and quantity should be surrounded by parentheses.

    `sign-before`
    :   Print the sign string before the quantity and currency symbol.

    `sign-after`
    :   Print the sign string after the quantity and currency symbol.

    `sign-before-currency-symbol`
    :   Print the sign string right before the currency symbol.

    `sign-after-currency-symbol`
    :   Print the sign string right after the currency symbol.

    `unspecified`
    :   Unspecified. We recommend you print the sign after the currency
        symbol.

Finally, the two following procedures may be helpful when programming
user interfaces:

Scheme Procedure: **locale-yes-regexp** [locale] [¶](#index-locale_002dyes_002dregexp)

Scheme Procedure: **locale-no-regexp** [locale] [¶](#index-locale_002dno_002dregexp)
:   Return a string that can be used as a regular expression to recognize
    a positive (respectively, negative) response to a yes/no question.
    For the C locale, the default values are typically `"^[yY]"` and
    `"^[nN]"`, respectively.

    Here is an example:

    ```
    (use-modules (ice-9 rdelim))
    (format #t "Does Guile rock?~%")
    (let lp ((answer (read-line)))
      (cond ((string-match (locale-yes-regexp) answer)
             (format #t "High fives!~%"))
            ((string-match (locale-no-regexp) answer)
             (format #t "How about now? Does it rock yet?~%")
             (lp (read-line)))
            (else
             (format #t "What do you mean?~%")
             (lp (read-line)))))
    ```

    For an internationalized yes/no string output, `gettext` should
    be used (see [Gettext Support](#Gettext-Support)).

Example uses of some of these functions are the implementation of the
`number->locale-string` and `monetary-amount->locale-string`
procedures (see [Number Input and Output](#Number-Input-and-Output)), as well as that the
SRFI-19 date and time conversion to/from strings (see [SRFI-19 - Time/Date Library](#SRFI_002d19)).

---

Previous: [Accessing Locale Information](#Accessing-Locale-Information), Up: [Support for Internationalization](#Internationalization)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 6.25.6 Gettext Support [¶](#Gettext-Support-1)

Guile provides an interface to GNU `gettext` for translating
message strings (see [Introduction](https://www.gnu.org/software/gettext/manual/gettext.html#Introduction) in GNU `gettext`
utilities).

Messages are collected in domains, so different libraries and programs
maintain different message catalogs. The domain parameter in
the functions below is a string (it becomes part of the message
catalog filename).

When `gettext` is not available, or if Guile was configured
‘`--without-nls`’, dummy functions doing no translation are
provided. When `gettext` support is available in Guile, the
`i18n` feature is provided (see [Feature Tracking](#Feature-Tracking)).

Scheme Procedure: **gettext** msg [domain [category]] [¶](#index-gettext)

C Function: **scm\_gettext** (msg, domain, category) [¶](#index-scm_005fgettext)
:   Return the translation of msg in domain. domain is
    optional and defaults to the domain set through `textdomain`
    below. category is optional and defaults to `LC_MESSAGES`
    (see [Locales](#Locales)).

    Normal usage is for msg to be a literal string.
    `xgettext` can extract those from the source to form a message
    catalog ready for translators (see [Invoking
    the `xgettext` Program](https://www.gnu.org/software/gettext/manual/gettext.html#xgettext-Invocation) in GNU `gettext`
    utilities).

    ```
    (display (gettext "You are in a maze of twisty passages."))
    ```

    It is conventional to use `G_` as a shorthand for
    `gettext`.[24](#FOOT24) Libraries can define `G_` in such
    a way to look up translations using its specific domain, allowing
    different parts of a program to have different translation sources.

    ```
    (define (G_ msg) (gettext msg "mylibrary"))
    (display (G_ "File not found."))
    ```

    `G_` is also a good place to perhaps strip disambiguating extra
    text from the message string, as for instance in [How to use `gettext` in GUI programs](https://www.gnu.org/software/gettext/manual/gettext.html#GUI-program-problems) in GNU
    `gettext` utilities.

Scheme Procedure: **ngettext** msg msgplural n [domain [category]] [¶](#index-ngettext)

C Function: **scm\_ngettext** (msg, msgplural, n, domain, category) [¶](#index-scm_005fngettext)
:   Return the translation of msg/msgplural in domain,
    with a plural form chosen appropriately for the number n.
    domain is optional and defaults to the domain set through
    `textdomain` below. category is optional and defaults to
    `LC_MESSAGES` (see [Locales](#Locales)).

    msg is the singular form, and msgplural the plural. When
    no translation is available, msg is used if *n = 1*,
    or msgplural otherwise. When translated, the message catalog
    can have a different rule, and can have more than two possible forms.

    As per `gettext` above, normal usage is for msg and
    msgplural to be literal strings, since `xgettext` can
    extract them from the source to build a message catalog. For
    example,

    ```
    (define (done n)
      (format #t (ngettext "~a file processed\n"
                           "~a files processed\n" n)
                 n))

    (done 1) ⊣ 1 file processed
    (done 3) ⊣ 3 files processed
    ```

    It’s important to use `ngettext` rather than plain `gettext`
    for plurals, since the rules for singular and plural forms in English
    are not the same in other languages. Only `ngettext` will allow
    translators to give correct forms (see [Additional
    functions for plural forms](https://www.gnu.org/software/gettext/manual/gettext.html#Plural-forms) in GNU `gettext` utilities).

Scheme Procedure: **textdomain** [domain] [¶](#index-textdomain)

C Function: **scm\_textdomain** (domain) [¶](#index-scm_005ftextdomain)
:   Get or set the default gettext domain. When called with no parameter
    the current domain is returned. When called with a parameter,
    domain is set as the current domain, and that new value
    returned. For example,

    ```
    (textdomain "myprog")
    ⇒ "myprog"
    ```

Scheme Procedure: **bindtextdomain** domain [directory] [¶](#index-bindtextdomain)

C Function: **scm\_bindtextdomain** (domain, directory) [¶](#index-scm_005fbindtextdomain)
:   Get or set the directory under which to find message files for
    domain. When called without a directory the current
    setting is returned. When called with a directory,
    directory is set for domain and that new setting returned.
    For example,

    ```
    (bindtextdomain "myprog" "/my/tree/share/locale")
    ⇒ "/my/tree/share/locale"
    ```

    When using Autoconf/Automake, an application should arrange for the
    configured `localedir` to get into the program (by substituting,
    or by generating a config file) and set that for its domain. This
    ensures the catalog can be found even when installed in a
    non-standard location.

Scheme Procedure: **bind-textdomain-codeset** domain [encoding] [¶](#index-bind_002dtextdomain_002dcodeset)

C Function: **scm\_bind\_textdomain\_codeset** (domain, encoding) [¶](#index-scm_005fbind_005ftextdomain_005fcodeset)
:   Get or set the text encoding to be used by `gettext` for messages
    from domain. encoding is a string, the name of a coding
    system, for instance `"8859_1"`. (On a Unix/POSIX system the
    `iconv` program can list all available encodings.)

    When called without an encoding the current setting is returned,
    or `#f` if none yet set. When called with an encoding, it
    is set for domain and that new setting returned. For example,

    ```
    (bind-textdomain-codeset "myprog")
    ⇒ #f
    (bind-textdomain-codeset "myprog" "latin-9")
    ⇒ "latin-9"
    ```

    The encoding requested can be different from the translated data file,
    messages will be recoded as necessary. But note that when there is no
    translation, `gettext` returns its msg unchanged, ie.
    without any recoding. For that reason source message strings are best
    as plain ASCII.

    Currently Guile has no understanding of multi-byte characters, and
    string functions won’t recognize character boundaries in multi-byte
    strings. An application will at least be able to pass such strings
    through to some output though. Perhaps this will change in the
    future.

---

Next: [Code Coverage Reports](#Code-Coverage), Previous: [Support for Internationalization](#Internationalization), Up: [API Reference](#API-Reference)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]
