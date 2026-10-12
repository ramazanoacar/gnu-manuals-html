### 12.2 About `gettext` [¶](#About-gettext)

The definition of the `gettext` interface comes from a Uniforum
proposal. It was submitted there by Sun, who had implemented the
`gettext` function in SunOS 4, around 1990. Nowadays, the
`gettext` interface is specified by the OpenI18N standard.

The main point about this solution is that it does not follow the
method of normal file handling (open-use-close) and that it does not
burden the programmer with so many tasks, especially the unique key handling.
Of course here also a unique key is needed, but this key is the message
itself (how long or short it is). See [Comparing the Two Interfaces](#Comparison) for a more
detailed comparison of the two methods.

The following section contains a rather detailed description of the
interface. We make it that detailed because this is the interface
we chose for the GNU `gettext` Library. Programmers interested
in using this library will be interested in this description.

* [The Interface](#Interface-to-gettext)
* [Solving Ambiguities](#Ambiguities)
* [Locating Message Catalog Files](#Locating-Catalogs)
* [How to specify the output character set `gettext` uses](#Charset-conversion)
* [Using contexts for solving ambiguities](#Contexts)
* [Additional functions for plural forms](#Plural-forms)
* [Optimization of the \*gettext functions](#Optimized-gettext)

---

Next: [Solving Ambiguities](#Ambiguities), Up: [About `gettext`](#gettext)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.2.1 The Interface [¶](#The-Interface-1)

The minimal functionality an interface must have is a) to select a
domain the strings are coming from (a single domain for all programs is
not reasonable because its construction and maintenance is difficult,
perhaps impossible) and b) to access a string in a selected domain.

This is principally the description of the `gettext` interface. It
has a global domain which unqualified usages reference. Of course this
domain is selectable by the user.

```
char *textdomain (const char *domain_name);
```

This provides the possibility to change or query the current status of
the current global domain of the `LC_MESSAGE` category. The
argument is a null-terminated string, whose characters must be legal in
the use in filenames. If the domain\_name argument is `NULL`,
the function returns the current value. If no value has been set
before, the name of the default domain is returned: *messages*.
Please note that although the return value of `textdomain` is of
type `char *` no changing is allowed. It is also important to know
that no checks of the availability are made. If the name is not
available you will see this by the fact that no translations are provided.

To use a domain set by `textdomain` the function

```
char *gettext (const char *msgid);
```

is to be used. This is the simplest reasonable form one can imagine.
The translation of the string msgid is returned if it is available
in the current domain. If it is not available, the argument itself is
returned. If the argument is `NULL` the result is undefined.

One thing which should come into mind is that no explicit dependency to
the used domain is given. The current value of the domain is used.
If this changes between two
executions of the same `gettext` call in the program, both calls
reference a different message catalog.

For the easiest case, which is normally used in internationalized
packages, once at the beginning of execution a call to `textdomain`
is issued, setting the domain to a unique name, normally the package
name. In the following code all strings which have to be translated are
filtered through the gettext function. That’s all, the package speaks
your language.

---

Next: [Locating Message Catalog Files](#Locating-Catalogs), Previous: [The Interface](#Interface-to-gettext), Up: [About `gettext`](#gettext)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.2.2 Solving Ambiguities [¶](#Solving-Ambiguities)

While this single name domain works well for most applications there
might be the need to get translations from more than one domain. Of
course one could switch between different domains with calls to
`textdomain`, but this is really not convenient nor is it fast. A
possible situation could be one case subject to discussion during this
writing: all
error messages of functions in the set of common used functions should
go into a separate domain `error`. By this mean we would only need
to translate them once.
Another case are messages from a library, as these *have* to be
independent of the current domain set by the application.

For this reasons there are two more functions to retrieve strings:

```
char *dgettext (const char *domain_name, const char *msgid);
char *dcgettext (const char *domain_name, const char *msgid,
                 int category);
```

Both take an additional argument at the first place, which corresponds
to the argument of `textdomain`. The third argument of
`dcgettext` allows to use another locale category but `LC_MESSAGES`.
But I really don’t know where this can be useful. If the
domain\_name is `NULL` or category has an value beside
the known ones, the result is undefined. It should also be noted that
this function is not part of the second known implementation of this
function family, the one found in Solaris.

A second ambiguity can arise by the fact, that perhaps more than one
domain has the same name. This can be solved by specifying where the
needed message catalog files can be found.

```
char *bindtextdomain (const char *domain_name,
                      const char *dir_name);
```

Calling this function binds the given domain to a file in the specified
directory (how this file is determined follows below). Especially a
file in the systems default place is not favored against the specified
file anymore (as it would be by solely using `textdomain`). A
`NULL` pointer for the dir\_name parameter returns the binding
associated with domain\_name. If domain\_name itself is
`NULL` nothing happens and a `NULL` pointer is returned. Here
again as for all the other functions is true that none of the return
value must be changed!

It is important to remember that relative path names for the
dir\_name parameter can be trouble. Since the path is always
computed relative to the current directory different results will be
achieved when the program executes a `chdir` command. Relative
paths should always be avoided to avoid dependencies and
unreliabilities.

```
wchar_t *wbindtextdomain (const char *domain_name,
                          const wchar_t *dir_name);
```

This function is provided only on native Windows platforms. It is like
`bindtextdomain`, except that the dir\_name parameter is a
wide string (in UTF-16 encoding, as usual on Windows).

---

Next: [How to specify the output character set `gettext` uses](#Charset-conversion), Previous: [Solving Ambiguities](#Ambiguities), Up: [About `gettext`](#gettext)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.2.3 Locating Message Catalog Files [¶](#Locating-Message-Catalog-Files)

Because many different languages for many different packages have to be
stored we need some way to add these information to file message catalog
files. The way usually used in Unix environments is have this encoding
in the file name. This is also done here. The directory name given in
`bindtextdomain`s second argument (or the default directory),
followed by the name of the locale, the locale category, and the domain name
are concatenated:

```
dir_name/locale/LC_category/domain_name.mo
```

The default value for dir\_name is system specific. For the GNU
library, and for packages adhering to its conventions, it’s:

```
/usr/local/share/locale
```

locale is the name of the locale category which is designated by
`LC_category`. For `gettext` and `dgettext` this
`LC_category` is always `LC_MESSAGES`.[3](#FOOT3)
The name of the locale category is determined through
`setlocale (LC_category, NULL)`.
[4](#FOOT4)
When using the function `dcgettext`, you can specify the locale category
through the third argument.

---

Next: [Using contexts for solving ambiguities](#Contexts), Previous: [Locating Message Catalog Files](#Locating-Catalogs), Up: [About `gettext`](#gettext)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.2.4 How to specify the output character set `gettext` uses [¶](#How-to-specify-the-output-character-set-gettext-uses)

`gettext` not only looks up a translation in a message catalog. It
also converts the translation on the fly to the desired output character
set. This is useful if the user is working in a different character set
than the translator who created the message catalog, because it avoids
distributing variants of message catalogs which differ only in the
character set.

The output character set is, by default, the value of `nl_langinfo
(CODESET)`, which depends on the `LC_CTYPE` part of the current
locale. But programs which store strings in a locale independent way
(e.g. UTF-8) can request that `gettext` and related functions
return the translations in that encoding, by use of the
`bind_textdomain_codeset` function.

Note that the msgid argument to `gettext` is not subject to
character set conversion. Also, when `gettext` does not find a
translation for msgid, it returns msgid unchanged –
independently of the current output character set. It is therefore
recommended that all msgids be US-ASCII strings.

Function: `char *` **bind\_textdomain\_codeset** `(const char *domainname, const char *codeset)` [¶](#index-bind_005ftextdomain_005fcodeset)
:   The `bind_textdomain_codeset` function can be used to specify the
    output character set for message catalogs for domain domainname.
    The codeset argument must be a valid codeset name which can be used
    for the `iconv_open` function, or a null pointer.

    If the codeset parameter is the null pointer,
    `bind_textdomain_codeset` returns the currently selected codeset
    for the domain with the name domainname. It returns `NULL` if
    no codeset has yet been selected.

    The `bind_textdomain_codeset` function can be used several times.
    If used multiple times with the same domainname argument, the
    later call overrides the settings made by the earlier one.

    The `bind_textdomain_codeset` function returns a pointer to a
    string containing the name of the selected codeset. The string is
    allocated internally in the function and must not be changed by the
    user. If the system went out of core during the execution of
    `bind_textdomain_codeset`, the return value is `NULL` and the
    global variable errno is set accordingly.

---

Next: [Additional functions for plural forms](#Plural-forms), Previous: [How to specify the output character set `gettext` uses](#Charset-conversion), Up: [About `gettext`](#gettext)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.2.5 Using contexts for solving ambiguities [¶](#Using-contexts-for-solving-ambiguities)

One place where the `gettext` functions, if used normally, have big
problems is within programs with graphical user interfaces (GUIs). The
problem is that many of the strings which have to be translated are very
short. They have to appear in pull-down menus which restricts the
length. But strings which are not containing entire sentences or at
least large fragments of a sentence may appear in more than one
situation in the program but might have different translations. This is
especially true for the one-word strings which are frequently used in
GUI programs.

As a consequence many people say that the `gettext` approach is
wrong and instead `catgets` should be used which indeed does not
have this problem. But there is a very simple and powerful method to
handle this kind of problems with the `gettext` functions.

Contexts can be added to strings to be translated. A context dependent
translation lookup is when a translation for a given string is searched,
that is limited to a given context. The translation for the same string
in a different context can be different. The different translations of
the same string in different contexts can be stored in the in the same
MO file, and can be edited by the translator in the same PO file.

The `gettext.h` include file contains the lookup macros for strings
with contexts. They are implemented as thin macros and inline functions
over the functions from `<libintl.h>`.

```
const char *pgettext (const char *msgctxt, const char *msgid);
```

In a call of this macro, msgctxt and msgid must be string
literals. The macro returns the translation of msgid, restricted
to the context given by msgctxt.

The msgctxt string is visible in the PO file to the translator.
You should try to make it somehow canonical and never changing. Because
every time you change an msgctxt, the translator will have to review
the translation of msgid.

Finding a canonical msgctxt string that doesn’t change over time can
be hard. But you shouldn’t use the file name or class name containing the
`pgettext` call – because it is a common development task to rename
a file or a class, and it shouldn’t cause translator work. Also you shouldn’t
use a comment in the form of a complete English sentence as msgctxt –
because orthography or grammar changes are often applied to such sentences,
and again, it shouldn’t force the translator to do a review.

The ‘`p`’ in ‘`pgettext`’ stands for “particular”: `pgettext`
fetches a particular translation of the msgid.

```
const char *dpgettext (const char *domain_name,
                       const char *msgctxt, const char *msgid);
const char *dcpgettext (const char *domain_name,
                        const char *msgctxt, const char *msgid,
                        int category);
```

These are generalizations of `pgettext`. They behave similarly to
`dgettext` and `dcgettext`, respectively. The domain\_name
argument defines the translation domain. The category argument
allows to use another locale category than `LC_MESSAGES`.

As as example consider the following fictional situation. A GUI program
has a menu bar with the following entries:

```
+------------+------------+--------------------------------------+
| File       | Printer    |                                      |
+------------+------------+--------------------------------------+
| Open     | | Select   |
| New      | | Open     |
+----------+ | Connect  |
             +----------+
```

To have the strings `File`, `Printer`, `Open`,
`New`, `Select`, and `Connect` translated there has to be
at some point in the code a call to a function of the `gettext`
family. But in two places the string passed into the function would be
`Open`. The translations might not be the same and therefore we
are in the dilemma described above.

What distinguishes the two places is the menu path from the menu root to
the particular menu entries:

```
Menu|File
Menu|Printer
Menu|File|Open
Menu|File|New
Menu|Printer|Select
Menu|Printer|Open
Menu|Printer|Connect
```

The context is thus the menu path without its last part. So, the calls
look like this:

```
pgettext ("Menu|", "File")
pgettext ("Menu|", "Printer")
pgettext ("Menu|File|", "Open")
pgettext ("Menu|File|", "New")
pgettext ("Menu|Printer|", "Select")
pgettext ("Menu|Printer|", "Open")
pgettext ("Menu|Printer|", "Connect")
```

Whether or not to use the ‘`|`’ character at the end of the context is a
matter of style.

For more complex cases, where the msgctxt or msgid are not
string literals, more general macros are available:

```
const char *pgettext_expr (const char *msgctxt, const char *msgid);
const char *dpgettext_expr (const char *domain_name,
                            const char *msgctxt, const char *msgid);
const char *dcpgettext_expr (const char *domain_name,
                             const char *msgctxt, const char *msgid,
                             int category);
```

Here msgctxt and msgid can be arbitrary string-valued expressions.
These macros are more general. But in the case that both argument expressions
are string literals, the macros without the ‘`_expr`’ suffix are more
efficient.

---

Next: [Optimization of the \*gettext functions](#Optimized-gettext), Previous: [Using contexts for solving ambiguities](#Contexts), Up: [About `gettext`](#gettext)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.2.6 Additional functions for plural forms [¶](#Additional-functions-for-plural-forms)

The functions of the `gettext` family described so far (and all the
`catgets` functions as well) have one problem in the real world
which have been neglected completely in all existing approaches. What
is meant here is the handling of plural forms.

Looking through Unix source code before the time anybody thought about
internationalization (and, sadly, even afterwards) one can often find
code similar to the following:

```
   printf ("%d file%s deleted", n, n == 1 ? "" : "s");
```

After the first complaints from people internationalizing the code people
either completely avoided formulations like this or used strings like
`"file(s)"`. Both look unnatural and should be avoided. First
tries to solve the problem correctly looked like this:

```
   if (n == 1)
     printf ("%d file deleted", n);
   else
     printf ("%d files deleted", n);
```

But this does not solve the problem. It helps languages where the
plural form of a noun is not simply constructed by adding an
‘s’
but that is all. Once again people fell into the trap of believing the
rules their language is using are universal. But the handling of plural
forms differs widely between the language families. For example,
Rafal Maszkowski `<rzm@mat.uni.torun.pl>` reports:

> In Polish we use e.g. plik (file) this way:
>
> ```
> 1 plik
> 2,3,4 pliki
> 5-21 pliko'w
> 22-24 pliki
> 25-31 pliko'w
> ```
>
> and so on (o’ means 8859-2 oacute which should be rather okreska,
> similar to aogonek).

There are two things which can differ between languages (and even inside
language families);

* The form how plural forms are built differs. This is a problem with
  languages which have many irregularities. German, for instance, is a
  drastic case. Though English and German are part of the same language
  family (Germanic), the almost regular forming of plural noun forms
  (appending an
  ‘s’)
  is hardly found in German.
* The number of plural forms differ. This is somewhat surprising for
  those who only have experiences with Romanic and Germanic languages
  since here the number is the same (there are two).

  But other language families have only one form or many forms. More
  information on this in an extra section.

The consequence of this is that application writers should not try to
solve the problem in their code. This would be localization since it is
only usable for certain, hardcoded language environments. Instead the
extended `gettext` interface should be used.

These extra functions are taking instead of the one key string two
strings and a numerical argument. The idea behind this is that using
the numerical argument and the first string as a key, the implementation
can select using rules specified by the translator the right plural
form. The two string arguments then will be used to provide a return
value in case no message catalog is found (similar to the normal
`gettext` behavior). In this case the rules for Germanic language
is used and it is assumed that the first string argument is the singular
form, the second the plural form.

This has the consequence that programs without language catalogs can
display the correct strings only if the program itself is written using
a Germanic language. This is a limitation but since the GNU C library
(as well as the GNU `gettext` package) are written as part of the
GNU package and the coding standards for the GNU project require program
being written in English, this solution nevertheless fulfills its
purpose.

Function: `char *` **ngettext** `(const char *msgid1, const char *msgid2, unsigned long int n)` [¶](#index-ngettext)
:   The `ngettext` function is similar to the `gettext` function
    as it finds the message catalogs in the same way. But it takes two
    extra arguments. The msgid1 parameter must contain the singular
    form of the string to be converted. It is also used as the key for the
    search in the catalog. The msgid2 parameter is the plural form.
    The parameter n is used to determine the plural form. If no
    message catalog is found msgid1 is returned if `n == 1`,
    otherwise `msgid2`.

    An example for the use of this function is:

    ```
    printf (ngettext ("%d file removed", "%d files removed", n), n);
    ```

    Please note that the numeric value n has to be passed to the
    `printf` function as well. It is not sufficient to pass it only to
    `ngettext`.

    In the English singular case, the number – always 1 – can be replaced with
    "one":

    ```
    printf (ngettext ("One file removed", "%d files removed", n), n);
    ```

    This works because the ‘`printf`’ function discards excess arguments that
    are not consumed by the format string.

    If this function is meant to yield a format string that takes two or more
    arguments, you can not use it like this:

    ```
    printf (ngettext ("%d file removed from directory %s",
                      "%d files removed from directory %s",
                      n),
            n, dir);
    ```

    because in many languages the translators want to replace the ‘`%d`’
    with an explicit word in the singular case, just like “one” in English,
    and C format strings cannot consume the second argument but skip the first
    argument. Instead, you have to reorder the arguments so that ‘`n`’
    comes last:

    ```
    printf (ngettext ("%2$d file removed from directory %1$s",
                      "%2$d files removed from directory %1$s",
                      n),
            dir, n);
    ```

    See [C Format Strings](#c_002dformat) for details about this argument reordering syntax.

    When you know that the value of `n` is within a given range, you can
    specify it as a comment directed to the `xgettext` tool. This
    information may help translators to use more adequate translations. Like
    this:

    ```
    if (days > 7 && days < 14)
      /* xgettext: range: 1..6 */
      printf (ngettext ("one week and one day", "one week and %d days",
                        days - 7),
              days - 7);
    ```

    There is one case where using `ngettext` is **not** appropriate,
    however:
    namely, when neither of the two strings contains a cardinal number.
    Consider the following example:

    ```
    puts (ngettext ("Delete the selected file?",
                    "Delete the selected files?",
                    n));
    ```

    The Russian language translator would need to provide separate
    translations for the following count forms:

    * 1, 21, 31, 41, 51, 61, 71, 81, 91…
    * 2–4, 22–24, 32–34, 42–44…
    * 5–20, 25–30, 35–40…

    As you can see,
    the case `n == 1` cannot be expressed with the Russian plural forms.
    Instead, in this case, you need to use separate calls to `gettext`:

    ```
    puts (n == 1 ? gettext ("Delete the selected file?")
                 : gettext ("Delete the selected files?"));
    ```

    The translator will then use the right grammar constructs
    for singular and plural *without* a number.

Function: `char *` **dngettext** `(const char *domain, const char *msgid1, const char *msgid2, unsigned long int n)` [¶](#index-dngettext)
:   The `dngettext` is similar to the `dgettext` function in the
    way the message catalog is selected. The difference is that it takes
    two extra parameter to provide the correct plural form. These two
    parameters are handled in the same way `ngettext` handles them.

Function: `char *` **dcngettext** `(const char *domain, const char *msgid1, const char *msgid2, unsigned long int n, int category)` [¶](#index-dcngettext)
:   The `dcngettext` is similar to the `dcgettext` function in the
    way the message catalog is selected. The difference is that it takes
    two extra parameter to provide the correct plural form. These two
    parameters are handled in the same way `ngettext` handles them.

Now, how do these functions solve the problem of the plural forms?
Without the input of linguists (which was not available) it was not
possible to determine whether there are only a few different forms in
which plural forms are formed or whether the number can increase with
every new supported language.

Therefore the solution implemented is to allow the translator to specify
the rules of how to select the plural form. Since the formula varies
with every language this is the only viable solution except for
hardcoding the information in the code (which still would require the
possibility of extensions to not prevent the use of new languages).

The information about the plural form selection has to be stored in the
header entry of the PO file (the one with the empty `msgid` string).
The plural form information looks like this:

```
Plural-Forms: nplurals=2; plural=n == 1 ? 0 : 1;
```

The `nplurals` value must be a decimal number which specifies how
many different plural forms exist for this language. The string
following `plural` is an expression which is using the C language
syntax. Exceptions are that no negative numbers are allowed, numbers
must be decimal, and the only variable allowed is `n`. Spaces are
allowed in the expression, but backslash-newlines are not; in the
examples below the backslash-newlines are present for formatting purposes
only. This expression will be evaluated whenever one of the functions
`ngettext`, `dngettext`, or `dcngettext` is called. The
numeric value passed to these functions is then substituted for all uses
of the variable `n` in the expression. The resulting value then
must be greater or equal to zero and smaller than the value given as the
value of `nplurals`.

The following rules are known at this point. The language with families
are listed. But this does not necessarily mean the information can be
generalized for the whole family (as can be easily seen in the table
below).[5](#FOOT5)

Only one form:
:   Some languages only require one single form. There is no distinction
    between the singular and plural form. An appropriate header entry
    would look like this:

    ```
    Plural-Forms: nplurals=1; plural=0;
    ```

    Languages with this property include:

    Asian family
    :   Japanese, Vietnamese, Korean

    Tai-Kadai family
    :   Thai

Two forms, singular used for one only
:   This is the form used in most existing programs since it is what English
    is using. A header entry would look like this:

    ```
    Plural-Forms: nplurals=2; plural=n != 1;
    ```

    (Note: this uses the feature of C expressions that boolean expressions
    have to value zero or one.)

    Languages with this property include:

    Germanic family
    :   English, German, Dutch, Swedish, Danish, Norwegian, Faroese

    Romanic family
    :   Spanish, Portuguese, Italian

    Latin/Greek family
    :   Greek

    Slavic family
    :   Bulgarian

    Finno-Ugric family
    :   Finnish, Estonian

    Semitic family
    :   Hebrew

    Austronesian family
    :   Bahasa Indonesian

    Artificial
    :   Esperanto

    Other languages using the same header entry are:

    Finno-Ugric family
    :   Hungarian

    Turkic/Altaic family
    :   Turkish

    Hungarian does not appear to have a plural if you look at sentences involving
    cardinal numbers. For example, “1 apple” is “1 alma”, and “123 apples” is
    “123 alma”. But when the number is not explicit, the distinction between
    singular and plural exists: “the apple” is “az alma”, and “the apples” is
    “az almák”. Since `ngettext` has to support both types of sentences,
    it is classified here, under “two forms”.

    The same holds for Turkish: “1 apple” is “1 elma”, and “123 apples” is
    “123 elma”. But when the number is omitted, the distinction between singular
    and plural exists: “the apple” is “elma”, and “the apples” is
    “elmalar”.

Two forms, singular used for zero and one
:   Exceptional case in the language family. The header entry would be:

    ```
    Plural-Forms: nplurals=2; plural=n>1;
    ```

    Languages with this property include:

    Romanic family
    :   Brazilian Portuguese, French

Three forms, special case for zero
:   The header entry would be:

    ```
    Plural-Forms: nplurals=3; plural=n%10==1 && n%100!=11 ? 0 : n != 0 ? 1 : 2;
    ```

    Languages with this property include:

    Baltic family
    :   Latvian

Three forms, special cases for one and two
:   The header entry would be:

    ```
    Plural-Forms: nplurals=3; plural=n==1 ? 0 : n==2 ? 1 : 2;
    ```

    Languages with this property include:

    Celtic
    :   Gaeilge (Irish)

Three forms, special case for numbers ending in 00 or [2-9][0-9]
:   The header entry would be:

    ```
    Plural-Forms: nplurals=3; \
        plural=n==1 ? 0 : (n==0 || (n%100 > 0 && n%100 < 20)) ? 1 : 2;
    ```

    Languages with this property include:

    Romanic family
    :   Romanian

Three forms, special case for numbers ending in 1[2-9]
:   The header entry would look like this:

    ```
    Plural-Forms: nplurals=3; \
        plural=n%10==1 && n%100!=11 ? 0 : \
               n%10>=2 && (n%100<10 || n%100>=20) ? 1 : 2;
    ```

    Languages with this property include:

    Baltic family
    :   Lithuanian

Three forms, special cases for numbers ending in 1 and 2, 3, 4, except those ending in 1[1-4]
:   The header entry would look like this:

    ```
    Plural-Forms: nplurals=3; \
        plural=n%10==1 && n%100!=11 ? 0 : \
               n%10>=2 && n%10<=4 && (n%100<10 || n%100>=20) ? 1 : 2;
    ```

    Languages with this property include:

    Slavic family
    :   Russian, Ukrainian, Belarusian, Serbian, Croatian

Three forms, special cases for 1 and 2, 3, 4
:   The header entry would look like this:

    ```
    Plural-Forms: nplurals=3; \
        plural=(n==1) ? 0 : (n>=2 && n<=4) ? 1 : 2;
    ```

    Languages with this property include:

    Slavic family
    :   Czech, Slovak

Three forms, special case for one and some numbers ending in 2, 3, or 4
:   The header entry would look like this:

    ```
    Plural-Forms: nplurals=3; \
        plural=n==1 ? 0 : \
               n%10>=2 && n%10<=4 && (n%100<10 || n%100>=20) ? 1 : 2;
    ```

    Languages with this property include:

    Slavic family
    :   Polish

Four forms, special case for one and all numbers ending in 02, 03, or 04
:   The header entry would look like this:

    ```
    Plural-Forms: nplurals=4; \
        plural=n%100==1 ? 0 : n%100==2 ? 1 : n%100==3 || n%100==4 ? 2 : 3;
    ```

    Languages with this property include:

    Slavic family
    :   Slovenian

Six forms, special cases for one, two, all numbers ending in 02, 03, … 10, all numbers ending in 11 … 99, and others
:   The header entry would look like this:

    ```
    Plural-Forms: nplurals=6; \
        plural=n==0 ? 0 : n==1 ? 1 : n==2 ? 2 : n%100>=3 && n%100<=10 ? 3 \
        : n%100>=11 ? 4 : 5;
    ```

    Languages with this property include:

    Afroasiatic family
    :   Arabic

You might now ask, `ngettext` handles only numbers n of type
‘`unsigned long`’. What about larger integer types? What about negative
numbers? What about floating-point numbers?

About larger integer types, such as ‘`uintmax_t`’ or
‘`unsigned long long`’: they can be handled by reducing the value to a
range that fits in an ‘`unsigned long`’. Simply casting the value to
‘`unsigned long`’ would not do the right thing, since it would treat
`ULONG_MAX + 1` like zero, `ULONG_MAX + 2` like singular, and
the like. Here you can exploit the fact that all mentioned plural form
formulas eventually become periodic, with a period that is a divisor of 100
(or 1000 or 1000000). So, when you reduce a large value to another one in
the range [1000000, 1999999] that ends in the same 6 decimal digits, you
can assume that it will lead to the same plural form selection. This code
does this:

```
#include <inttypes.h>
uintmax_t nbytes = ...;
printf (ngettext ("The file has %"PRIuMAX" byte.",
                  "The file has %"PRIuMAX" bytes.",
                  (nbytes > ULONG_MAX
                   ? (nbytes % 1000000) + 1000000
                   : nbytes)),
        nbytes);
```

Negative and floating-point values usually represent physical entities for
which singular and plural don’t clearly apply. In such cases, there is no
need to use `ngettext`; a simple `gettext` call with a form suitable
for all values will do. For example:

```
printf (gettext ("Time elapsed: %.3f seconds"),
        num_milliseconds * 0.001);
```

Even if num\_milliseconds happens to be a multiple of 1000, the output

```
Time elapsed: 1.000 seconds
```

is acceptable in English, and similarly for other languages.

The translators’ perspective regarding plural forms is explained in
[Translating plural forms](#Translating-plural-forms).

---

Previous: [Additional functions for plural forms](#Plural-forms), Up: [About `gettext`](#gettext)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.2.7 Optimization of the \*gettext functions [¶](#Optimization-of-the-_002agettext-functions)

At this point of the discussion we should talk about an advantage of the
GNU `gettext` implementation. Some readers might have pointed out
that an internationalized program might have a poor performance if some
string has to be translated in an inner loop. While this is unavoidable
when the string varies from one run of the loop to the other it is
simply a waste of time when the string is always the same. Take the
following example:

```
{
  while (...)
    {
      puts (gettext ("Hello world"));
    }
}
```

When the locale selection does not change between two runs the resulting
string is always the same. One way to use this is:

```
{
  str = gettext ("Hello world");
  while (...)
    {
      puts (str);
    }
}
```

But this solution is not usable in all situation (e.g. when the locale
selection changes) nor does it lead to legible code.

For this reason, GNU `gettext` caches previous translation results.
When the same translation is requested twice, with no new message
catalogs being loaded in between, `gettext` will, the second time,
find the result through a single cache lookup.

---

Next: [Using libintl.a in own programs](#Using-libintl_002ea), Previous: [About `gettext`](#gettext), Up: [The Programmer’s View](#Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 12.3 Comparing the Two Interfaces [¶](#Comparing-the-Two-Interfaces)

The following discussion is perhaps a little bit colored. As said
above we implemented GNU `gettext` following the Uniforum
proposal and this surely has its reasons. But it should show how we
came to this decision.

First we take a look at the developing process. When we write an
application using NLS provided by `gettext` we proceed as always.
Only when we come to a string which might be seen by the users and thus
has to be translated we use `gettext("…")` instead of
`"…"`. At the beginning of each source file (or in a central
header file) we define

```
#define gettext(String) (String)
```

Even this definition can be avoided when the system supports the
`gettext` function in its C library. When we compile this code the
result is the same as if no NLS code is used. When you take a look at
the GNU `gettext` code you will see that we use `_("…")`
instead of `gettext("…")`. This reduces the number of
additional characters per translatable string to *3* (in words:
three).

When now a production version of the program is needed we simply replace
the definition

```
#define _(String) (String)
```

by

```
#include <libintl.h>
#define _(String) gettext (String)
```

Additionally we run the program `xgettext` on all source code file
which contain translatable strings and that’s it: we have a running
program which does not depend on translations to be available, but which
can use any that becomes available.

The same procedure can be done for the `gettext_noop` invocations
(see [Special Cases of Translatable Strings](#Special-cases)). One usually defines `gettext_noop` as a
no-op macro. So you should consider the following code for your project:

```
#define gettext_noop(String) String
#define N_(String) gettext_noop (String)
```

`N_` is a short form similar to `_`. The `Makefile` in
the `po/` directory of GNU `gettext` knows by default both of the
mentioned short forms so you are invited to follow this proposal for
your own ease.

Now to `catgets`. The main problem is the work for the
programmer. Every time he comes to a translatable string he has to
define a number (or a symbolic constant) which has also be defined in
the message catalog file. He also has to take care for duplicate
entries, duplicate message IDs etc. If he wants to have the same
quality in the message catalog as the GNU `gettext` program
provides he also has to put the descriptive comments for the strings and
the location in all source code files in the message catalog. This is
nearly a Mission: Impossible.

But there are also some points people might call advantages speaking for
`catgets`. If you have a single word in a string and this string
is used in different contexts it is likely that in one or the other
language the word has different translations. Example:

```
printf ("%s: %d", gettext ("number"), number_of_errors)

printf ("you should see %d %s", number_count,
        number_count == 1 ? gettext ("number") : gettext ("numbers"))
```

Here we have to translate two times the string `"number"`. Even
if you do not speak a language beside English it might be possible to
recognize that the two words have a different meaning. In German the
first appearance has to be translated to `"Anzahl"` and the second
to `"Zahl"`.

Now you can say that this example is really esoteric. And you are
right! This is exactly how we felt about this problem and decide that
it does not weight that much. The solution for the above problem could
be very easy:

```
printf ("%s %d", gettext ("number:"), number_of_errors)

printf (number_count == 1 ? gettext ("you should see %d number")
                          : gettext ("you should see %d numbers"),
        number_count)
```

We believe that we can solve all conflicts with this method. If it is
difficult one can also consider changing one of the conflicting string a
little bit. But it is not impossible to overcome.

`catgets` allows same original entry to have different translations,
but `gettext` has another, scalable approach for solving ambiguities
of this kind: See [Solving Ambiguities](#Ambiguities).

---

Next: [Being a `gettext` grok](#gettext-grok), Previous: [Comparing the Two Interfaces](#Comparison), Up: [The Programmer’s View](#Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 12.4 Using libintl.a in own programs [¶](#Using-libintl_002ea-in-own-programs)

Starting with version 0.9.4 the library `libintl.h` should be
self-contained. I.e., you can use it in your own programs without
providing additional functions. The `Makefile` will put the header
and the library in directories selected using the `$(prefix)`.

---

Next: [Temporary Notes for the Programmers Chapter](#Temp-Programmers), Previous: [Using libintl.a in own programs](#Using-libintl_002ea), Up: [The Programmer’s View](#Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 12.5 Being a `gettext` grok [¶](#Being-a-gettext-grok)

**NOTE:**  This documentation section is outdated and needs to be
revised.

To fully exploit the functionality of the GNU `gettext` library it
is surely helpful to read the source code. But for those who don’t want
to spend that much time in reading the (sometimes complicated) code here
is a list comments:

* Changing the language at runtime

  For interactive programs it might be useful to offer a selection of the
  used language at runtime. To understand how to do this one need to know
  how the used language is determined while executing the `gettext`
  function. The method which is presented here only works correctly
  with the GNU implementation of the `gettext` functions.

  In the function `dcgettext` at every call the current setting of
  the highest priority environment variable is determined and used.
  Highest priority means here the following list with decreasing
  priority:

  1. `LANGUAGE`
  2. `LC_ALL`
  3. `LC_xxx`, according to selected locale category
  4. `LANG`

  Afterwards the path is constructed using the found value and the
  translation file is loaded if available.

  What happens now when the value for, say, `LANGUAGE` changes? According
  to the process explained above the new value of this variable is found
  as soon as the `dcgettext` function is called. But this also means
  the (perhaps) different message catalog file is loaded. In other
  words: the used language is changed.

  But there is one little hook. The code for gcc-2.7.0 and up provides
  some optimization. This optimization normally prevents the calling of
  the `dcgettext` function as long as no new catalog is loaded. But
  if `dcgettext` is not called the program also cannot find the
  `LANGUAGE` variable be changed (see [Optimization of the \*gettext functions](#Optimized-gettext)). A
  solution for this is very easy. Include the following code in the
  language switching function.

  ```
    /* Change language.  */
    setenv ("LANGUAGE", "fr", 1);

    /* Make change known.  */
    {
      extern int  _nl_msg_cat_cntr;
      ++_nl_msg_cat_cntr;
    }
  ```

  The variable `_nl_msg_cat_cntr` is defined in `loadmsgcat.c`.
  You don’t need to know what this is for. But it can be used to detect
  whether a `gettext` implementation is GNU gettext and not non-GNU
  system’s native gettext implementation.

---

Previous: [Being a `gettext` grok](#gettext-grok), Up: [The Programmer’s View](#Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 12.6 Temporary Notes for the Programmers Chapter [¶](#Temporary-Notes-for-the-Programmers-Chapter)

**NOTE:**  This documentation section is outdated and needs to be
revised.

* [Temporary - Two Possible Implementations](#Temp-Implementations)
* [Temporary - About `catgets`](#Temp-catgets)
* [Temporary - Why a single implementation](#Temp-WSI)
* [Temporary - Notes](#Temp-Notes)

---

Next: [Temporary - About `catgets`](#Temp-catgets), Up: [Temporary Notes for the Programmers Chapter](#Temp-Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.6.1 Temporary - Two Possible Implementations [¶](#Temporary-_002d-Two-Possible-Implementations)

There are two competing methods for language independent messages:
the X/Open `catgets` method, and the Uniforum `gettext`
method. The `catgets` method indexes messages by integers; the
`gettext` method indexes them by their English translations.
The `catgets` method has been around longer and is supported
by more vendors. The `gettext` method is supported by Sun,
and it has been heard that the COSE multi-vendor initiative is
supporting it. Neither method is a POSIX standard; the POSIX.1
committee had a lot of disagreement in this area.

Neither one is in the POSIX standard. There was much disagreement
in the POSIX.1 committee about using the `gettext` routines
vs. `catgets` (XPG). In the end the committee couldn’t
agree on anything, so no messaging system was included as part
of the standard. I believe the informative annex of the standard
includes the XPG3 messaging interfaces, “…as an example of
a messaging system that has been implemented…”

They were very careful not to say anywhere that you should use one
set of interfaces over the other. For more on this topic please
see the Programming for Internationalization FAQ.

---

Next: [Temporary - Why a single implementation](#Temp-WSI), Previous: [Temporary - Two Possible Implementations](#Temp-Implementations), Up: [Temporary Notes for the Programmers Chapter](#Temp-Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.6.2 Temporary - About `catgets` [¶](#Temporary-_002d-About-catgets)

There have been a few discussions of late on the use of
`catgets` as a base. I think it important to present both
sides of the argument and hence am opting to play devil’s advocate
for a little bit.

I’ll not deny the fact that `catgets` could have been designed
a lot better. It currently has quite a number of limitations and
these have already been pointed out.

However there is a great deal to be said for consistency and
standardization. A common recurring problem when writing Unix
software is the myriad portability problems across Unix platforms.
It seems as if every Unix vendor had a look at the operating system
and found parts they could improve upon. Undoubtedly, these
modifications are probably innovative and solve real problems.
However, software developers have a hard time keeping up with all
these changes across so many platforms.

And this has prompted the Unix vendors to begin to standardize their
systems. Hence the impetus for Spec1170. Every major Unix vendor
has committed to supporting this standard and every Unix software
developer waits with glee the day they can write software to this
standard and simply recompile (without having to use autoconf)
across different platforms.

As I understand it, Spec1170 is roughly based upon version 4 of the
X/Open Portability Guidelines (XPG4). Because `catgets` and
friends are defined in XPG4, I’m led to believe that `catgets`
is a part of Spec1170 and hence will become a standardized component
of all Unix systems.

---

Next: [Temporary - Notes](#Temp-Notes), Previous: [Temporary - About `catgets`](#Temp-catgets), Up: [Temporary Notes for the Programmers Chapter](#Temp-Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.6.3 Temporary - Why a single implementation [¶](#Temporary-_002d-Why-a-single-implementation)

Now it seems kind of wasteful to me to have two different systems
installed for accessing message catalogs. If we do want to remedy
`catgets` deficiencies why don’t we try to expand `catgets`
(in a compatible manner) rather than implement an entirely new system.
Otherwise, we’ll end up with two message catalog access systems installed
with an operating system - one set of routines for packages using GNU
`gettext` for their internationalization, and another set of routines
(catgets) for all other software. Bloated?

Supposing another catalog access system is implemented. Which do
we recommend? At least for Linux, we need to attract as many
software developers as possible. Hence we need to make it as easy
for them to port their software as possible. Which means supporting
`catgets`. We will be implementing the `libintl` code
within our `libc`, but does this mean we also have to incorporate
another message catalog access scheme within our `libc` as well?
And what about people who are going to be using the `libintl`
+ non-`catgets` routines. When they port their software to
other platforms, they’re now going to have to include the front-end
(`libintl`) code plus the back-end code (the non-`catgets`
access routines) with their software instead of just including the
`libintl` code with their software.

Message catalog support is however only the tip of the iceberg.
What about the data for the other locale categories? They also have
a number of deficiencies. Are we going to abandon them as well and
develop another duplicate set of routines (should `libintl`
expand beyond message catalog support)?

Like many parts of Unix that can be improved upon, we’re stuck with balancing
compatibility with the past with useful improvements and innovations for
the future.

---

Previous: [Temporary - Why a single implementation](#Temp-WSI), Up: [Temporary Notes for the Programmers Chapter](#Temp-Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.6.4 Temporary - Notes [¶](#Temporary-_002d-Notes)

X/Open agreed very late on the standard form so that many
implementations differ from the final form. Both of my system (old
Linux catgets and Ultrix-4) have a strange variation.

OK. After incorporating the last changes I have to spend some time on
making the GNU/Linux `libc` `gettext` functions. So in future
Solaris is not the only system having `gettext`.

---

Next: [The Maintainer’s View](#Maintainers), Previous: [The Programmer’s View](#Programmers), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 13 The Translator’s View [¶](#The-Translator_0027s-View)

* [Organization](#Organization)
* [Responsibilities in the Translation Project](#Responsibilities)
* [Language dialects](#Dialects)
* [Translating plural forms](#Translating-plural-forms)
* [Prioritizing messages: How to determine which messages to translate first](#Prioritizing-messages)

---

Next: [Responsibilities in the Translation Project](#Responsibilities), Up: [The Translator’s View](#Translators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 13.1 Organization [¶](#Organization-1)

For some software packages, each translator works on her own
and communicates directly with the developers of the package.
For some other software packages, on the other hand,
translators are organized
into *translation projects* and *translation teams*.

A *translation project* applies to a group of software packages
and shares procedures and methodologies regarding the translation.

There are currently three major translation projects:

* The “Translation Project”, which is used
  by all kinds of Free Software packages, but in particular by GNU packages.
  It has its home at <https://translationproject.org/>.
* The “KDE Localization Project”, which is used by KDE packages.
  It is at <https://l10n.kde.org/>.
* The “GNOME Localization Project”, which is used by GNOME packages.
  It is at <https://l10n.gnome.org/>.

A *translation team* is a group of translators for a single language,
in the scope of a translation project.

---

Next: [Language dialects](#Dialects), Previous: [Organization](#Organization), Up: [The Translator’s View](#Translators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 13.2 Responsibilities in the Translation Project [¶](#Responsibilities-in-the-Translation-Project)

The following rules and habits apply to the Translation Project.

The translator’s responsibilities are:

* She submits a translations disclaimer to the Free Software Foundation
  (once only, and
  only when she wants to translate a package that requires a disclaimer).

  A disclaimer is a legal document
  that allows the software package to distribute her translation work.
  It is not as strong as a copyright assignment.
  Merely, it says that the signer
  will never make use of the copyright on her translations:
  will never forbid copying them,
  and will never ask for some kind of compensation.
  This guarantees that the FSF (and everyone else)
  will always be allowed to freely distribute these translations.
  The FSF wishes to have this guarantee in a legally binding manner,
  to be on the safe side.

  There are two ways to submit the disclaimer:
  Either online through
  [this form](https://crm.fsf.org/civicrm/profile/create?gid=91&reset=1)
  on the FSF’s web site,
  or by printing, signing, and submitting
  the file `disclaim-translations.txt` found in the GNU gettext distribution.
* Agree with the other translators of the same team
  who is “in charge” for the translations of a particular package.

The Translation Project has a coordinator.
He can be reached at ‘`coordinator@translationproject.org`’.
His responsibilities are:

* He maintains the web site <https://translationproject.org/>.
* When he receives a release or prerelease announcement
  from one of the software package maintainers,
  he extracts the POT file(s)
  and sends notifications to all translation teams about it.

The responsibilities of the package maintainers are:

* To incorporate the translations in the software package before a release.
* To forward bug reports about the translations
  to the respective translation team.
  A developer or maintainer should never apply a translation fix himself,
  because that would cause conflicts with the translation team.

---

Next: [Translating plural forms](#Translating-plural-forms), Previous: [Responsibilities in the Translation Project](#Responsibilities), Up: [The Translator’s View](#Translators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 13.3 Language dialects [¶](#Language-dialects)

For many languages, a translation into the main dialect is intelligible
by all speakers of the language.
Speakers of another dialect can have a separate translation if they wish so.
In fact, since the fallback mechanism implemented in GNU libc and GNU libintl
applies on a per-message basis,
the message catalog for the dialect needs only to contain
the translations that differ from those in the main language.

For example,
French speakers in Canada (that is, users in the locale `fr_CA`)
can use and do accept translations
produced by French speakers in France (typical file name: `fr.po`).
Nevertheless, the translation system with PO files
enables them to produce special message catalogs (file name: `fr_CA.po`)
that will take priority over `fr.po` for users in that locale.
Similarly for users in Austria,
where message catalogs `de_AT.po` take priority
over the catalogs named `de.po` that reflect German as spoken in Germany.

The situation is different for Chinese, though:
Since users in the People’s Republic of China and in Singapore
want translations with Simplified Chinese characters,
whereas Chinese users in other territories
(such as Taiwan, Hong Kong, and Macao)
want translations with Traditional Chinese characters,
no translator should ever submit a file named `zh.po`.
Instead, there will typically be two separate translation teams:
a team that produces translations with Simplified Chinese characters
(file name `zh_CN.po`)
and a team that produces translations with Traditional Chinese characters
(file name `zh_TW.po`).

---

Next: [Prioritizing messages: How to determine which messages to translate first](#Prioritizing-messages), Previous: [Language dialects](#Dialects), Up: [The Translator’s View](#Translators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 13.4 Translating plural forms [¶](#Translating-plural-forms-1)

Suppose you are translating a PO file, and it contains an entry like this:

```
#, c-format
msgid "One file removed"
msgid_plural "%d files removed"
msgstr[0] ""
msgstr[1] ""
```

What does this mean? How do you fill it in?

Such an entry denotes a message with plural forms, that is, a message where
the text depends on a cardinal number. The general form of the message,
in English, is the `msgid_plural` line. The `msgid` line is the
English singular form, that is, the form for when the number is equal to 1.
More details about plural forms are explained in [Additional functions for plural forms](#Plural-forms).

The first thing you need to look at is the `Plural-Forms` line in the
header entry of the PO file. It contains the number of plural forms and a
formula. If the PO file does not yet have such a line, you have to add it.
It only depends on the language into which you are translating. You can
get this info by using the `msginit` command (see [Creating or preparing for translating a PO file](#Creating)) –
it contains a database of known plural formulas – or by asking other
members of your translation team.

Suppose the line looks as follows:

```
"Plural-Forms: nplurals=3; plural=n%10==1 && n%100!=11 ? 0 : n%10>=2 && n"
"%10<=4 && (n%100<10 || n%100>=20) ? 1 : 2;\n"
```

It’s logically one line; recall that the PO file formatting is allowed to
break long lines so that each physical line fits in 80 monospaced columns.

The value of `nplurals` here tells you that there are three plural
forms. The first thing you need to do is to ensure that the entry contains
an `msgstr` line for each of the forms:

```
#, c-format
msgid "One file removed"
msgid_plural "%d files removed"
msgstr[0] ""
msgstr[1] ""
msgstr[2] ""
```

Then translate the `msgid_plural` line and fill it in into each
`msgstr` line:

```
#, c-format
msgid "One file removed"
msgid_plural "%d files removed"
msgstr[0] "%d slika uklonjenih"
msgstr[1] "%d slika uklonjenih"
msgstr[2] "%d slika uklonjenih"
```

Now you can refine the translation so that it matches the plural form.
According to the formula above, `msgstr[0]` is used when the number
ends in 1 but does not end in 11; `msgstr[1]` is used when the number
ends in 2, 3, 4, but not in 12, 13, 14; and `msgstr[2]` is used in
all other cases. With this knowledge, you can refine the translations:

```
#, c-format
msgid "One file removed"
msgid_plural "%d files removed"
msgstr[0] "%d slika je uklonjena"
msgstr[1] "%d datoteke uklonjenih"
msgstr[2] "%d slika uklonjenih"
```

You noticed that in the English singular form (`msgid`) the number
placeholder could be omitted and replaced by the numeral word “one”.
Can you do this in your translation as well?

```
msgstr[0] "jednom datotekom je uklonjen"
```

Well, it depends on whether `msgstr[0]` applies only to the number 1,
or to other numbers as well. If, according to the plural formula,
`msgstr[0]` applies only to `n == 1`, then you can use the
specialized translation without the number placeholder. In our case,
however, `msgstr[0]` also applies to the numbers 21, 31, 41, etc.,
and therefore you cannot omit the placeholder.

---

Previous: [Translating plural forms](#Translating-plural-forms), Up: [The Translator’s View](#Translators)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 13.5 Prioritizing messages: How to determine which messages to translate first [¶](#Prioritizing-messages_003a-How-to-determine-which-messages-to-translate-first)

A translator sometimes has only a limited amount of time per week to
spend on a package, and some packages have quite large message catalogs
(over 1000 messages). Therefore she wishes to translate the messages
first that are the most visible to the user, or that occur most frequently.
This section describes how to determine these "most urgent" messages.
It also applies to determine the "next most urgent" messages after the
message catalog has already been partially translated.

In a first step, she uses the programs like a user would do. While she
does this, the GNU `gettext` library logs into a file the not yet
translated messages for which a translation was requested from the program.

In a second step, she uses the PO mode to translate precisely this set
of messages.

Here are more details. The GNU `libintl` library (but not the
corresponding functions in GNU `libc`) supports an environment variable
`GETTEXT_LOG_UNTRANSLATED`. The GNU `libintl` library will
log into this file the messages for which `gettext()` and related
functions couldn’t find the translation. If the file doesn’t exist, it
will be created as needed. On systems with GNU `libc` a shared library
‘`preloadable_libintl.so`’ is provided that can be used with the ELF
‘`LD_PRELOAD`’ mechanism.

So, in the first step, the translator uses these commands on systems with
GNU `libc`:

```
$ LD_PRELOAD=/usr/local/lib/preloadable_libintl.so
$ export LD_PRELOAD
$ GETTEXT_LOG_UNTRANSLATED=$HOME/gettextlogused
$ export GETTEXT_LOG_UNTRANSLATED
```

and these commands on other systems:

```
$ GETTEXT_LOG_UNTRANSLATED=$HOME/gettextlogused
$ export GETTEXT_LOG_UNTRANSLATED
```

Then she uses and peruses the programs. (It is a good and recommended
practice to use the programs for which you provide translations: it
gives you the needed context.) When done, she removes the environment
variables:

```
$ unset LD_PRELOAD
$ unset GETTEXT_LOG_UNTRANSLATED
```

The second step starts with removing duplicates:

```
$ msguniq $HOME/gettextlogused > missing.po
```

The result is a PO file, but needs some preprocessing before a PO file editor
can be used with it. First, it is a multi-domain PO file, containing
messages from many translation domains. Second, it lacks all translator
comments and source references. Here is how to get a list of the affected
translation domains:

```
$ sed -n -e 's,^domain "\(.*\)"$,\1,p' < missing.po | sort | uniq
```

Then the translator can handle the domains one by one. For simplicity,
let’s use environment variables to denote the language, domain and source
package.

```
$ lang=nl             # your language
$ domain=coreutils    # the name of the domain to be handled
$ package=/usr/src/gnu/coreutils-4.5.4   # the package where it comes from
```

She takes the latest copy of `$lang.po` from the Translation Project,
or from the package (in most cases, `$package/po/$lang.po`), or
creates a fresh one if she’s the first translator (see [Creating or preparing for translating a PO file](#Creating)).
She then uses the following commands to mark the not urgent messages as
"obsolete". (This doesn’t mean that these messages - translated and
untranslated ones - will go away. It simply means that the PO file editor
will ignore them in the following editing session.)

```
$ msggrep --domain=$domain missing.po | grep -v '^domain' \
  > $domain-missing.po
$ msgattrib --set-obsolete --ignore-file $domain-missing.po $domain.$lang.po \
  > $domain.$lang-urgent.po
```

Then she translates `$domain.$lang-urgent.po` by use of a PO file editor
(see [Editing PO Files](#Editing)).
(FIXME: I don’t know whether `Lokalize` and `gtranslator` also
preserve obsolete messages, as they should.)
Finally she restores the not urgent messages (with their earlier
translations, for those which were already translated) through this command:

```
$ msgmerge --no-fuzzy-matching $domain.$lang-urgent.po $package/po/$domain.pot \
  > $domain.$lang.po
```

Then she can submit `$domain.$lang.po` and proceed to the next domain.

---

Next: [The Installer’s and Distributor’s View](#Installers), Previous: [The Translator’s View](#Translators), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 14 The Maintainer’s View [¶](#The-Maintainer_0027s-View)

The maintainer of a package has many responsibilities. One of them
is ensuring that the package will install easily on many platforms,
and that the magic we described earlier (see [The User’s View](#Users)) will work
for installers and end users.

Of course, there are many possible ways by which GNU `gettext`
might be integrated in a distribution, and this chapter does not cover
them in all generality. Instead, it details one possible approach which
is especially adequate for many free software distributions following GNU
standards, or even better, Gnits standards, because GNU `gettext`
is purposely for helping the internationalization of the whole GNU
project, and as many other good free packages as possible. So, the
maintainer’s view presented here presumes that the package already has
a `configure.ac` file and uses GNU Autoconf.

Nevertheless, GNU `gettext` may surely be useful for free packages
not following GNU standards and conventions, but the maintainers of such
packages might have to show imagination and initiative in organizing
their distributions so `gettext` work for them in all situations.
There are surely many, out there.

Even if `gettext` methods are now stabilizing, slight adjustments
might be needed between successive `gettext` versions, so you
should ideally revise this chapter in subsequent releases, looking
for changes.

* [Flat or Non-Flat Directory Structures](#Flat-and-Non_002dFlat)
* [Prerequisite Works](#Prerequisites)
* [Invoking the `gettextize` Program](#gettextize-Invocation)
* [Files You Must Create or Alter](#Adjusting-Files)
* [Autoconf macros for use in `configure.ac`](#autoconf-macros)
* [Interfacing with the Translators](#Interfacing-with-the-Translators)
* [Integrating with Version Control Systems](#Version-Control-Issues)
* [Creating a Distribution Tarball](#Release-Management)

---

Next: [Prerequisite Works](#Prerequisites), Up: [The Maintainer’s View](#Maintainers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 14.1 Flat or Non-Flat Directory Structures [¶](#Flat-or-Non_002dFlat-Directory-Structures)

Some free software packages are distributed as `tar` files which unpack
in a single directory, these are said to be *flat* distributions.
Other free software packages have a one level hierarchy of subdirectories, using
for example a subdirectory named `doc/` for the Texinfo manual and
man pages, another called `lib/` for holding functions meant to
replace or complement C libraries, and a subdirectory `src/` for
holding the proper sources for the package. These other distributions
are said to be *non-flat*.

We cannot say much about flat distributions. A flat
directory structure has the disadvantage of increasing the difficulty
of updating to a new version of GNU `gettext`. Also, if you have
many PO files, this could somewhat pollute your single directory.
Also, GNU `gettext`’s libintl sources consist of C sources, shell
scripts, `sed` scripts and complicated Makefile rules, which don’t
fit well into an existing flat structure. For these reasons, we
recommend to use non-flat approach in this case as well.

Maybe because GNU `gettext` itself has a non-flat structure,
we have more experience with this approach, and this is what will be
described in the remaining of this chapter. Some maintainers might
use this as an opportunity to unflatten their package structure.

---

Next: [Invoking the `gettextize` Program](#gettextize-Invocation), Previous: [Flat or Non-Flat Directory Structures](#Flat-and-Non_002dFlat), Up: [The Maintainer’s View](#Maintainers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 14.2 Prerequisite Works [¶](#Prerequisite-Works)

There are some works which are required for using GNU `gettext`
in one of your package. These works have some kind of generality
that escape the point by point descriptions used in the remainder
of this chapter. So, we describe them here.

* Before attempting to use `gettextize` you should install some
  other packages first.
  Ensure that recent versions of GNU `m4`, GNU Autoconf and GNU
  `gettext` are already installed at your site, and if not, proceed
  to do this first. If you get to install these things, beware that
  GNU `m4` must be fully installed before GNU Autoconf is even
  *configured*.

  To further ease the task of a package maintainer the `automake`
  package was designed and implemented. GNU `gettext` now uses this
  tool and the `Makefile` in the `po/` directory therefore
  knows about all the goals necessary for using `automake`.

  Those four packages are only needed by you, as a maintainer; the
  installers of your own package and end users do not really need any of
  GNU `m4`, GNU Autoconf, GNU `gettext`, or GNU `automake`
  for successfully installing and running your package, with messages
  properly translated. But this is not completely true if you provide
  internationalized shell scripts within your own package: GNU
  `gettext` shall then be installed at the user site if the end users
  want to see the translation of shell script messages.
* Your package should use Autoconf and have a `configure.ac` or
  `configure.in` file.
  If it does not, you have to learn how. The Autoconf documentation
  is quite well written, it is a good idea that you print it and get
  familiar with it.
* Your C sources should have already been modified according to
  instructions given earlier in this manual. See [Preparing Program Sources](#Sources).
* Your `po/` directory should receive all PO files submitted to you
  by the translator teams, each having `ll.po` as a name.
  This is not usually easy to get translation
  work done before your package gets internationalized and available!
  Since the cycle has to start somewhere, the easiest for the maintainer
  is to start with absolutely no PO files, and wait until various
  translator teams get interested in your package, and submit PO files.

It is worth adding here a few words about how the maintainer should
ideally behave with PO files submissions. As a maintainer, your role is
to authenticate the origin of the submission as being the representative
of the appropriate translating teams of the Translation Project (forward
the submission to `coordinator@translationproject.org` in case of doubt),
to ensure that the PO file format is not severely broken and does not
prevent successful installation, and for the rest, to merely put these
PO files in `po/` for distribution.

As a maintainer, you do not have to take on your shoulders the
responsibility of checking if the translations are adequate or
complete, and should avoid diving into linguistic matters. Translation
teams drive themselves and are fully responsible of their linguistic
choices for the Translation Project. Keep in mind that translator teams are *not*
driven by maintainers. You can help by carefully redirecting all
communications and reports from users about linguistic matters to the
appropriate translation team, or explain users how to reach or join
their team.

Maintainers should *never ever* apply PO file bug reports
themselves, short-cutting translation teams. If some translator has
difficulty to get some of her points through her team, it should not be
an option for her to directly negotiate translations with maintainers.
Teams ought to settle their problems themselves, if any. If you, as
a maintainer, ever think there is a real problem with a team, please
never try to *solve* a team’s problem on your own.

---

Next: [Files You Must Create or Alter](#Adjusting-Files), Previous: [Prerequisite Works](#Prerequisites), Up: [The Maintainer’s View](#Maintainers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 14.3 Invoking the `gettextize` Program [¶](#Invoking-the-gettextize-Program)

The `gettextize` program is an interactive tool that helps the
maintainer of a package internationalized through GNU `gettext`.
It is used for two purposes:

* As a wizard, when a package is modified to use GNU `gettext` for
  the first time.
* As a migration tool, for upgrading the GNU `gettext` support in
  a package from a previous to a newer version of GNU `gettext`.

This program performs the following tasks:

* It copies into the package some files that are consistently and
  identically needed in every package internationalized through
  GNU `gettext`.
* It performs as many of the tasks mentioned in the next section
  [Files You Must Create or Alter](#Adjusting-Files) as can be performed automatically.
* It removes obsolete files and idioms used for previous GNU
  `gettext` versions to the form recommended for the current GNU
  `gettext` version.
* It prints a summary of the tasks that ought to be done manually
  and could not be done automatically by `gettextize`.

It can be invoked as follows:

```
gettextize [ option... ] [ directory ]
```

and accepts the following options:

‘`-f`’ [¶](#index-_002df_002c-gettextize-option)

‘`--force`’
:   Force replacement of files which already exist.

‘`--po-dir=dir`’ [¶](#index-_002d_002dpo_002ddir_002c-gettextize-option)
:   Specify a directory containing PO files. Such a directory contains the
    translations into various languages of a particular POT file. This
    option can be specified multiple times, once for each translation domain.
    If it is not specified, the directory named `po/` is updated.

‘`--no-changelog`’ [¶](#index-_002d_002dno_002dchangelog_002c-gettextize-option)
:   Don’t update or create ChangeLog files. By default, `gettextize`
    logs all changes (file additions, modifications and removals) in a
    file called ‘`ChangeLog`’ in each affected directory.

‘`--symlink`’ [¶](#index-_002d_002dsymlink_002c-gettextize-option)
:   Make symbolic links instead of copying the needed files. This can be
    useful to save a few kilobytes of disk space, but it requires extra
    effort to create self-contained tarballs, it may disturb some mechanism
    the maintainer applies to the sources, and it is likely to introduce
    bugs when a newer version of `gettext` is installed on the system.

‘`-n`’ [¶](#index-_002dd_002c-gettextize-option)

‘`--dry-run`’
:   Print modifications but don’t perform them. All actions that
    `gettextize` would normally execute are inhibited and instead only
    listed on standard output.

‘`--help`’ [¶](#index-_002d_002dhelp_002c-gettextize-option)
:   Display this help and exit.

‘`--version`’ [¶](#index-_002d_002dversion_002c-gettextize-option)
:   Output version information and exit.

If directory is given, this is the top level directory of a
package to prepare for using GNU `gettext`. If not given, it
is assumed that the current directory is the top level directory of
such a package.

The program `gettextize` provides the following files. However,
no existing file will be replaced unless the option `--force`
(`-f`) is specified.

1. The `ABOUT-NLS` file is copied in the main directory of your package,
   the one being at the top level. This file contains a reference to the
   GNU gettext documentation. It also avoids an error from Automake in
   packages that use the Automake option ‘`gnits`’:
   “error: required file ’./ABOUT-NLS’ not found”.
2. A `po/` directory is created for eventually holding
   all translation files, but initially only containing the file
   `po/Makefile.in.in` from the GNU `gettext` distribution
   (beware the double ‘`.in`’ in the file name) and a few auxiliary
   files. If the `po/` directory already exists, it will be preserved
   along with the files it contains, and only `Makefile.in.in` and
   the auxiliary files will be overwritten.

   If ‘`--po-dir`’ has been specified, this holds for every directory
   specified through ‘`--po-dir`’, instead of `po/`.
3. The file `config.rpath` is copied into the directory containing
   configuration support files. It is needed by the `AM_GNU_GETTEXT`
   autoconf macro.
4. Only if the project is using GNU `automake`:
   A set of `autoconf` macro files is copied into the package’s
   `autoconf` macro repository, usually in a directory called `m4/`.

If your site support symbolic links, `gettextize` will not
actually copy the files into your package, but establish symbolic
links instead. This avoids duplicating the disk space needed in
all packages. Merely using the ‘`-h`’ option while creating the
`tar` archive of your distribution will resolve each link by an
actual copy in the distribution archive. So, to insist, you really
should use ‘`-h`’ option with `tar` within your `dist`
goal of your main `Makefile.in`.

Furthermore, `gettextize` will update all `Makefile.am` files
in each affected directory, as well as the top level `configure.ac`
or `configure.in` file.

It is interesting to understand that most new files for supporting
GNU `gettext` facilities in one package go in `po/` and
`m4/` subdirectories. Still, these directories will mostly
contain package dependent files.

The `gettextize` program makes backup files for all files it
replaces or changes, and also write ChangeLog entries about these
changes. This way, the careful maintainer can check after running
`gettextize` whether its changes are acceptable to him, and
possibly adjust them. An exception to this rule is the `intl/`
directory, which is removed as a whole if it still existed.

It is important to understand that `gettextize` can not do the
entire job of adapting a package for using GNU `gettext`. The
amount of remaining work depends on whether the package uses GNU
`automake` or not. But in any case, the maintainer should still
read the section [Files You Must Create or Alter](#Adjusting-Files) after invoking `gettextize`.

In particular, if after using ‘`gettexize`’, you get an error
‘`AC_COMPILE_IFELSE was called before AC_GNU_SOURCE`’ or
‘`AC_RUN_IFELSE was called before AC_GNU_SOURCE`’, you can fix it
by modifying `configure.ac`, as described in [`configure.ac` at top level](#configure_002eac).

It is also important to understand that `gettextize` is not part
of the GNU build system, in the sense that it should not be invoked
automatically, and not be invoked by someone who doesn’t assume the
responsibilities of a package maintainer. For the latter purpose, a
separate tool is provided, see [Invoking the `autopoint` Program](#autopoint-Invocation).

---

Next: [Autoconf macros for use in `configure.ac`](#autoconf-macros), Previous: [Invoking the `gettextize` Program](#gettextize-Invocation), Up: [The Maintainer’s View](#Maintainers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 14.4 Files You Must Create or Alter [¶](#Files-You-Must-Create-or-Alter)

Besides files which are automatically added through `gettextize`,
there are many files needing revision for properly interacting with
GNU `gettext`. If you are closely following GNU standards for
Makefile engineering and auto-configuration, the adaptations should
be easier to achieve. Here is a point by point description of the
changes needed in each.

So, here comes a list of files, each one followed by a description of
all alterations it needs. Many examples are taken out from the GNU
`gettext` 1.0 distribution itself, or from the GNU
`hello` distribution (<https://www.gnu.org/software/hello>).
You may indeed refer to the source code of the GNU `gettext` and
GNU `hello` packages, as they are intended to be good examples for
using GNU gettext functionality.

* [`POTFILES.in` in `po/`](#po_002fPOTFILES_002ein)
* [`fetch-po` in `po/`](#po_002ffetch_002dpo)
* [`LINGUAS` in `po/`](#po_002fLINGUAS)
* [`Makevars` in `po/`](#po_002fMakevars)
* [Extending `Makefile` in `po/`](#po_002fRules_002d_002a)
* [`configure.ac` at top level](#configure_002eac)
* [`config.guess`, `config.sub` at top level](#config_002eguess)
* [`mkinstalldirs` at top level](#mkinstalldirs)
* [`aclocal.m4` at top level](#aclocal)
* [`config.h.in` at top level](#config_002eh_002ein)
* [`Makefile.in` at top level](#Makefile)
* [`Makefile.in` in `src/`](#src_002fMakefile)
* [`gettext.h` in `lib/`](#lib_002fgettext_002eh)

---

Next: [`fetch-po` in `po/`](#po_002ffetch_002dpo), Up: [Files You Must Create or Alter](#Adjusting-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 14.4.1 `POTFILES.in` in `po/` [¶](#POTFILES_002ein-in-po_002f)

The `po/` directory should receive a file named
`POTFILES.in`. This file tells which files, among all program
sources, have marked strings needing translation. Here is an example
of such a file:

```
# List of source files containing translatable strings.
# Copyright (C) 1995 Free Software Foundation, Inc.

# Common library files
lib/error.c
lib/getopt.c
lib/xmalloc.c
