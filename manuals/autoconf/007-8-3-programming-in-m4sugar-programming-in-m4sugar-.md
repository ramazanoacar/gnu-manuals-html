### 8.3 Programming in M4sugar [¶](#Programming-in-M4sugar-1)

M4 by itself provides only a small, but sufficient, set of all-purpose
macros. M4sugar introduces additional generic macros. Its name was
coined by Lars J. Aas: “Readability And Greater Understanding Stands 4
M4sugar”.

M4sugar reserves the macro namespace ‘`^_m4_`’ for internal use, and
the macro namespace ‘`^m4_`’ for M4sugar macros. You should not
define your own macros into these namespaces.

* [Redefined M4 Macros](#Redefined-M4-Macros)
* [Diagnostic messages from M4sugar](#Diagnostic-Macros)
* [Diversion support](#Diversion-support)
* [Conditional constructs](#Conditional-constructs)
* [Looping constructs](#Looping-constructs)
* [Evaluation Macros](#Evaluation-Macros)
* [String manipulation in M4](#Text-processing-Macros)
* [Arithmetic computation in M4](#Number-processing-Macros)
* [Set manipulation in M4](#Set-manipulation-Macros)
* [Forbidden Patterns](#Forbidden-Patterns)

---

Next: [Diagnostic messages from M4sugar](#Diagnostic-Macros), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.1 Redefined M4 Macros [¶](#Redefined-M4-Macros-1)

With a few exceptions, all the M4 native macros are moved in the
‘`m4_`’ pseudo-namespace, e.g., M4sugar renames `define` as
`m4_define` etc.

The list of macros unchanged from M4, except for their name, is:

* m4\_builtin
* m4\_changecom
* m4\_changequote
* m4\_debugfile
* m4\_debugmode
* m4\_decr
* m4\_define
* m4\_divnum
* m4\_errprint
* m4\_esyscmd
* m4\_eval
* m4\_format
* m4\_ifdef
* m4\_incr
* m4\_index
* m4\_indir
* m4\_len
* m4\_pushdef
* m4\_shift
* m4\_substr
* m4\_syscmd
* m4\_sysval
* m4\_traceoff
* m4\_traceon
* m4\_translit

Some M4 macros are redefined, and are slightly incompatible with their
native equivalent.

Macro: **\_\_file\_\_** [¶](#index-_005f_005ffile_005f_005f-1)

Macro: **\_\_line\_\_** [¶](#index-_005f_005fline_005f_005f-1)
:   All M4 macros starting with ‘`__`’ retain their original name: for
    example, no `m4__file__` is defined.

Macro: **\_\_oline\_\_** [¶](#index-_005f_005foline_005f_005f-1)
:   This is not technically a macro, but a feature of Autom4te. The
    sequence `__oline__` can be used similarly to the other m4sugar
    location macros, but rather than expanding to the location of the input
    file, it is translated to the line number where it appears in the output
    file after all other M4 expansions.

Macro: **dnl** [¶](#index-dnl-3)
:   This macro kept its original name: no `m4_dnl` is defined.

Macro: **m4\_bpatsubst**(string, regexp, [replacement]) [¶](#index-m4_005fbpatsubst-1)
:   This is the M4sugar name for the M4 native macro `patsubst`.

    It uses the same regular expression syntax as `m4_bregexp`; see
    [m4\_bregexp](#m4_005fbregexp). Also see [Substituting text by regular
    expression](https://www.gnu.org/software/m4/manual/m4.html#Patsubst) in GNU m4 macro processor, for details of how the
    replacement is processed.

    The name `m4_patsubst` is reserved for use by a future version of
    M4sugar, when there is a version of GNU M4 that supports POSIX extended
    regular expression syntax.

Macro: **m4\_bregexp**(string, regexp, [replacement]) [¶](#index-m4_005fbregexp-1)
:   This is the M4sugar name for the M4 native macro `regexp`.

    The only regular expression syntax understood by GNU M4 is similar to,
    but not the same as, POSIX basic regular expression syntax.
    See [Searching for regular expressions](https://www.gnu.org/software/m4/manual/m4.html#Regexp) in GNU m4 macro
    processor, for the precise syntax.

    The name `m4_regexp` is reserved for use by a future version of
    M4sugar, when there is a version of GNU M4 that supports POSIX extended
    regular expression syntax.

Macro: **m4\_copy**(source, dest) [¶](#index-m4_005fcopy-1)

Macro: **m4\_copy\_force**(source, dest) [¶](#index-m4_005fcopy_005fforce-1)

Macro: **m4\_rename**(source, dest) [¶](#index-m4_005frename-1)

Macro: **m4\_rename\_force**(source, dest) [¶](#index-m4_005frename_005fforce-1)
:   These macros aren’t directly builtins, but are closely related to
    `m4_pushdef` and `m4_defn`. `m4_copy` and
    `m4_rename` ensure that dest is undefined, while
    `m4_copy_force` and `m4_rename_force` overwrite any existing
    definition. All four macros then proceed to copy the entire pushdef
    stack of definitions of source over to dest. `m4_copy`
    and `m4_copy_force` preserve the source (including in the special
    case where source is undefined), while `m4_rename` and
    `m4_rename_force` undefine the original macro name (making it an
    error to rename an undefined source).

    Note that attempting to invoke a renamed macro might not work, since the
    macro may have a dependence on helper macros accessed via composition of
    ‘`$0`’ but that were not also renamed; likewise, other macros may
    have a hard-coded dependence on source and could break if
    source has been deleted. On the other hand, it is always safe to
    rename a macro to temporarily move it out of the way, then rename it
    back later to restore original semantics.

Macro: **m4\_defn**(macro…) [¶](#index-m4_005fdefn-1)
:   This macro fails if macro is not defined, even when using older
    versions of M4 that did not warn. See `m4_undefine`.
    Unfortunately, in order to support these older versions of M4, there are
    some situations involving unbalanced quotes where concatenating multiple
    macros together will work in newer M4 but not in m4sugar; use
    quadrigraphs to work around this.

Macro: **m4\_divert**(diversion) [¶](#index-m4_005fdivert-1)
:   M4sugar relies heavily on diversions, so rather than behaving as a
    primitive, `m4_divert` behaves like:

    ```
    m4_divert_pop()m4_divert_push([diversion])
    ```

    See [Diversion support](#Diversion-support), for more details about the use of the
    diversion stack. In particular, this implies that diversion
    should be a named diversion rather than a raw number. But be aware that
    it is seldom necessary to explicitly change the diversion stack, and
    that when done incorrectly, it can lead to syntactically invalid
    scripts.

Macro: **m4\_dumpdef**(name…) [¶](#index-m4_005fdumpdef-1)

Macro: **m4\_dumpdefs**(name…) [¶](#index-m4_005fdumpdefs-1)
:   `m4_dumpdef` is like the M4 builtin, except that this version
    requires at least one argument, output always goes to standard error
    rather than the current debug file, no sorting is done on multiple
    arguments, and an error is issued if any
    name is undefined. `m4_dumpdefs` is a convenience macro that
    calls `m4_dumpdef` for all of the
    `m4_pushdef` stack of definitions, starting with the current, and
    silently does nothing if name is undefined.

    Unfortunately, due to a limitation in M4 1.4.x, any macro defined as a
    builtin is output as the empty string. This behavior is rectified by
    using M4 1.6 or newer. However, this behavior difference means that
    `m4_dumpdef` should only be used while developing m4sugar macros,
    and never in the final published form of a macro.

Macro: **m4\_esyscmd\_s**(command) [¶](#index-m4_005fesyscmd_005fs-1)
:   Like `m4_esyscmd`, this macro expands to the result of running
    command in a shell. The difference is that any trailing newlines
    are removed, so that the output behaves more like shell command
    substitution.

Macro: **m4\_exit**(exit-status) [¶](#index-m4_005fexit-1)
:   This macro corresponds to `m4exit`.

Macro: **m4\_if**(comment) [¶](#index-m4_005fif-1)

Macro: **m4\_if**(string-1, string-2, equal, [not-equal]) [¶](#index-m4_005fif-2)

Macro: **m4\_if**(string-1, string-2, equal-1, string-3, string-4, equal-2, …, [not-equal]) [¶](#index-m4_005fif-3)
:   This macro corresponds to `ifelse`. string-1 and
    string-2 are compared literally, so usually one of the two
    arguments is passed unquoted. See [Conditional constructs](#Conditional-constructs), for more
    conditional idioms.

Macro: **m4\_include**(file) [¶](#index-m4_005finclude-1)

Macro: **m4\_sinclude**(file) [¶](#index-m4_005fsinclude-1)
:   Like the M4 builtins, but warn against multiple inclusions of file.

Macro: **m4\_mkstemp**(template) [¶](#index-m4_005fmkstemp-1)

Macro: **m4\_maketemp**(template) [¶](#index-m4_005fmaketemp-1)
:   Older versions of POSIX specified a macro `maketemp`,
    which replaced the trailing ‘`X`’
    characters in template with the process id, without regards to the
    existence of a file by that name.
    This was a security hole, so current POSIX instead specifies a macro
    `mkstemp` that always creates a uniquely named file. In M4sugar,
    `m4_maketemp` and `m4_mkstemp` are synonyms for each other,
    and both have the secure semantics regardless of which macro the
    underlying M4 provides.

Macro: **m4\_popdef**(macro…) [¶](#index-m4_005fpopdef-1)
:   This macro fails if macro is not defined, even when using older
    versions of M4 that did not warn. See `m4_undefine`.

Macro: **m4\_undefine**(macro…) [¶](#index-m4_005fundefine-1)
:   This macro fails if macro is not defined, even when using older
    versions of M4 that did not warn. Use

    ```
    m4_ifdef([macro], [m4_undefine([macro])])
    ```

    if you are not sure whether macro is defined.

Macro: **m4\_undivert**(diversion…) [¶](#index-m4_005fundivert-1)
:   Unlike the M4 builtin, at least one diversion must be specified.
    Also, since the M4sugar diversion stack prefers named
    diversions, the use of `m4_undivert` to include files is risky.
    See [Diversion support](#Diversion-support), for more details about the use of the
    diversion stack. But be aware that it is seldom necessary to explicitly
    change the diversion stack, and that when done incorrectly, it can lead
    to syntactically invalid scripts.

Macro: **m4\_wrap**(text) [¶](#index-m4_005fwrap-1)

Macro: **m4\_wrap\_lifo**(text) [¶](#index-m4_005fwrap_005flifo-1)
:   These macros correspond to `m4wrap`. POSIX requires arguments of
    multiple wrap calls to be reprocessed at EOF in the same order
    as the original calls (first-in, first-out). GNU M4 versions
    through 1.4.10, however, reprocess them in reverse order (last-in,
    first-out). Both orders are useful, therefore, you can rely on
    `m4_wrap` to provide FIFO semantics and `m4_wrap_lifo` for
    LIFO semantics, regardless of the underlying GNU M4 version.

    Unlike the GNU M4 builtin, these macros only recognize one
    argument, and avoid token pasting between consecutive invocations. On
    the other hand, nested calls to `m4_wrap` from within wrapped text
    work just as in the builtin.

---

Next: [Diversion support](#Diversion-support), Previous: [Redefined M4 Macros](#Redefined-M4-Macros), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.2 Diagnostic messages from M4sugar [¶](#Diagnostic-messages-from-M4sugar)

When macros statically diagnose abnormal situations, benign or fatal,
they should report them using these macros. For issuing dynamic issues,
i.e., when `configure` is run, see [Printing Messages](#Printing-Messages).

Macro: **m4\_assert**(expression, [exit-status = ‘`1`’]) [¶](#index-m4_005fassert-1)
:   Assert that the arithmetic expression evaluates to non-zero.
    Otherwise, issue a fatal error, and exit `autom4te` with
    exit-status.

Macro: **m4\_errprintn**(message) [¶](#index-m4_005ferrprintn-1)
:   Similar to the builtin `m4_errprint`, except that a newline is
    guaranteed after message.

Macro: **m4\_fatal**(message) [¶](#index-m4_005ffatal-1)
:   Report a severe error message prefixed with the current location,
    and have `autom4te` die.

Macro: **m4\_location** [¶](#index-m4_005flocation-1)
:   Useful as a prefix in a message line. Short for:

    ```
    __file__:__line__
    ```

Macro: **m4\_warn**(category, message) [¶](#index-m4_005fwarn-1)
:   Report message as a warning (or as an error if requested by the
    user) if warnings of the category are turned on. If the message
    is emitted, it is prefixed with the current location, and followed by a
    call trace of all macros defined via `AC_DEFUN` used to get to the
    current expansion.

    The category must be one of:

    ‘`cross`’
    :   Warnings about constructs that may interfere with cross-compilation,
        such as using `AC_RUN_IFELSE` without a default.

    ‘`gnu`’
    :   Warnings related to the GNU Coding Standards
        (see [The GNU Coding Standards](https://www.gnu.org/prep/standards/standards.html#Top)).
        On by default.

    ‘`obsolete`’
    :   Warnings about obsolete features. On by default.

    ‘`override`’
    :   Warnings about redefinitions of Autoconf internals.

    ‘`portability`’
    :   Warnings about non-portable constructs.

    ‘`portability-recursive`’
    :   Warnings about recursive Make variable expansions (`$(foo$(x))`).

    ‘`extra-portability`’
    :   Extra warnings about non-portable constructs, covering rarely-used
        tools.

    ‘`syntax`’
    :   Warnings about questionable syntactic constructs, incorrectly ordered
        macro calls, typos, etc. On by default.

    ‘`unsupported`’
    :   Warnings about unsupported features. On by default.

    **Hacking Note:** The set of categories is defined by code in
    `autom4te`, not by M4sugar itself. Additions should be
    coordinated with Automake, so that both sets of tools accept the same
    options.

---

Next: [Conditional constructs](#Conditional-constructs), Previous: [Diagnostic messages from M4sugar](#Diagnostic-Macros), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.3 Diversion support [¶](#Diversion-support-1)

M4sugar makes heavy use of diversions under the hood, because it is
often the case that
text that must appear early in the output is not discovered until late
in the input. Additionally, some of the topological sorting algorithms
used in resolving macro dependencies use diversions. However, most
macros should not need to change diversions directly, but rather rely on
higher-level M4sugar macros to manage diversions transparently. If you
change diversions improperly, you risk generating a syntactically
invalid script, because an incorrect diversion will violate assumptions
made by many macros about whether prerequisite text has been previously
output. In short, if you manually change the diversion, you should not
expect any macros provided by the Autoconf package to work until you
have restored the diversion stack back to its original state.

In the rare case that it is necessary to write a macro that explicitly
outputs text to a different diversion, it is important to be aware of an
M4 limitation regarding diversions: text only goes to a diversion if it
is not part of argument collection. Therefore, any macro that changes
the current diversion cannot be used as an unquoted argument to another
macro, but must be expanded at the top level. The macro
`m4_expand` will diagnose any attempt to change diversions, since
it is generally useful only as an argument to another macro. The
following example shows what happens when diversion manipulation is
attempted within macro arguments:

```
m4_do([normal text]
m4_divert_push([KILL])unwanted[]m4_divert_pop([KILL])
[m4_divert_push([KILL])discarded[]m4_divert_pop([KILL])])dnl
⇒normal text
⇒unwanted
```

Notice that the unquoted text `unwanted` is output, even though it
was processed while the current diversion was `KILL`, because it
was collected as part of the argument to `m4_do`. However, the
text `discarded` disappeared as desired, because the diversion
changes were single-quoted, and were not expanded until the top-level
rescan of the output of `m4_do`.

To make diversion management easier, M4sugar uses the concept of named
diversions. Rather than using diversion numbers directly, it is nicer
to associate a name with each diversion. The diversion number associated
with a particular diversion name is an implementation detail, and a
syntax warning is issued if a diversion number is used instead of a
name. In general, you should not output text
to a named diversion until after calling the appropriate initialization
routine for your language (`m4_init`, `AS_INIT`,
`AT_INIT`, …), although there are some exceptions documented
below.

M4sugar defines two named diversions.

`KILL`
:   Text written to this diversion is discarded. This is the default
    diversion once M4sugar is initialized.

`GROW`
:   This diversion is used behind the scenes by topological sorting macros,
    such as `AC_REQUIRE`.

M4sh adds several more named diversions.

`BINSH`
:   This diversion is reserved for the ‘`#!`’ interpreter line.

`HEADER-REVISION`
:   This diversion holds text from `AC_REVISION`.

`HEADER-COMMENT`
:   This diversion holds comments about the purpose of a file.

`HEADER-COPYRIGHT`
:   This diversion is managed by `AC_COPYRIGHT`.

`M4SH-SANITIZE`
:   This diversion contains M4sh sanitization code, used to ensure M4sh is
    executing in a reasonable shell environment.

`M4SH-INIT`
:   This diversion contains M4sh initialization code, initializing variables
    that are required by other M4sh macros.

`BODY`
:   This diversion contains the body of the shell code, and is the default
    diversion once M4sh is initialized.

Autotest inherits diversions from M4sh, and changes the default
diversion from `BODY` back to `KILL`. It also adds several
more named diversions, with the following subset designed for developer
use.

`PREPARE_TESTS`
:   This diversion contains initialization sequences which are executed
    after `atconfig` and `atlocal`, and after all command line
    arguments have been parsed, but prior to running any tests. It can be
    used to set up state that is required across all tests. This diversion
    will work even before `AT_INIT`.

Autoconf inherits diversions from M4sh, and adds the following named
diversions which developers can utilize.

`DEFAULTS`
:   This diversion contains shell variable assignments to set defaults that
    must be in place before arguments are parsed. This diversion is placed
    early enough in `configure` that it is unsafe to expand any
    autoconf macros into this diversion.

`HELP_ENABLE`
:   If `AC_PRESERVE_HELP_ORDER` was used, then text placed in this
    diversion will be included as part of a quoted here-doc providing all of
    the `--help` output of `configure` related to options
    created by `AC_ARG_WITH` and `AC_ARG_ENABLE`.

`INIT_PREPARE`
:   This diversion occurs after all command line options have been parsed,
    but prior to the main body of the `configure` script. This
    diversion is the last chance to insert shell code such as variable
    assignments or shell function declarations that will used by the
    expansion of other macros.

For now, the remaining named diversions of Autoconf, Autoheader, and
Autotest are not documented. In other words,
intentionally outputting text into an undocumented diversion is subject
to breakage in a future release of Autoconf.

Macro: **m4\_cleardivert**(diversion…) [¶](#index-m4_005fcleardivert-1)
:   Permanently discard any text that has been diverted into
    diversion.

Macro: **m4\_divert\_once**(diversion, [content]) [¶](#index-m4_005fdivert_005fonce-1)
:   Similar to `m4_divert_text`, except that content is only
    output to diversion if this is the first time that
    `m4_divert_once` has been called with its particular arguments.

Macro: **m4\_divert\_pop**([diversion]) [¶](#index-m4_005fdivert_005fpop-1)
:   If provided, check that the current diversion is indeed diversion.
    Then change to the diversion located earlier on the stack, giving an
    error if an attempt is made to pop beyond the initial m4sugar diversion
    of `KILL`.

Macro: **m4\_divert\_push**(diversion) [¶](#index-m4_005fdivert_005fpush-1)
:   Remember the former diversion on the diversion stack, and output
    subsequent text into diversion. M4sugar maintains a diversion
    stack, and issues an error if there is not a matching pop for every
    push.

Macro: **m4\_divert\_text**(diversion, [content]) [¶](#index-m4_005fdivert_005ftext-1)
:   Output content and a newline into diversion, without
    affecting the current diversion. Shorthand for:

    ```
    m4_divert_push([diversion])content
    m4_divert_pop([diversion])dnl
    ```

    One use of `m4_divert_text` is to develop two related macros, where
    macro ‘`MY_A`’ does the work, but adjusts what work is performed
    based on whether the optional macro ‘`MY_B`’ has also been expanded.
    Of course, it is possible to use `AC_BEFORE` within `MY_A` to
    require that ‘`MY_B`’ occurs first, if it occurs at all. But this
    imposes an ordering restriction on the user; it would be nicer if macros
    ‘`MY_A`’ and ‘`MY_B`’ can be invoked in either order. The trick
    is to let ‘`MY_B`’ leave a breadcrumb in an early diversion, which
    ‘`MY_A`’ can then use to determine whether ‘`MY_B`’ has been
    expanded.

    ```
    AC_DEFUN([MY_A],
    [# various actions
    if test -n "$b_was_used"; then
      # extra action
    fi])
    AC_DEFUN([MY_B],
    [AC_REQUIRE([MY_A])dnl
    m4_divert_text([INIT_PREPARE], [b_was_used=true])])
    ```

Macro: **m4\_init** [¶](#index-m4_005finit-1)
:   Initialize the M4sugar environment, setting up the default named
    diversion to be `KILL`.

---

Next: [Looping constructs](#Looping-constructs), Previous: [Diversion support](#Diversion-support), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.4 Conditional constructs [¶](#Conditional-constructs-1)

The following macros provide additional conditional constructs as
convenience wrappers around `m4_if`.

Macro: **m4\_bmatch**(string, regex-1, value-1, [regex-2], [value-2], …, [default]) [¶](#index-m4_005fbmatch-1)
:   The string string is repeatedly compared against a series of
    regex arguments; if a match is found, the expansion is the
    corresponding value, otherwise, the macro moves on to the next
    regex. If no regex match, then the result is the optional
    default, or nothing.

    The regular expression syntax understood by `m4_bmatch` is the same as
    that understood by `m4_bregexp`; see [m4\_bregexp](#m4_005fbregexp). The name
    `m4_match` is reserved for use by a future version of M4sugar, when
    there is a version of GNU M4 that supports POSIX extended
    regular expression syntax.

Macro: **m4\_bpatsubsts**(string, regex-1, subst-1, [regex-2], [subst-2], …) [¶](#index-m4_005fbpatsubsts-1)
:   The string string is altered by regex-1 and subst-1,
    as if by:

    ```
    m4_bpatsubst([[string]], [regex], [subst])
    ```

    The result of the substitution is then passed through the next set of
    regex and subst, and so forth. An empty subst implies
    deletion of any matched portions in the current string. Note that this
    macro over-quotes string; this behavior is intentional, so that
    the result of each step of the recursion remains as a quoted string.
    However, it means that anchors (‘`^`’ and ‘`$`’ in the regex
    will line up with the extra quotations, and not the characters of the
    original string. The overquoting is removed after the final
    substitution.

    The regular expression and replacement syntax understood by
    `m4_bpatsubsts` is the same as that understood by `m4_bpatsubst`;
    see [m4\_bpatsubst](#m4_005fbpatsubst). The name `m4_patsubsts` is reserved for use by
    future version of M4sugar, when there is a version of GNU M4 that
    supports POSIX extended regular expression syntax.

Macro: **m4\_case**(string, value-1, if-value-1, [value-2], [if-value-2], …, [default]) [¶](#index-m4_005fcase-1)
:   Test string against multiple value possibilities, resulting
    in the first if-value for a match, or in the optional
    default. This is shorthand for:

    ```
    m4_if([string], [value-1], [if-value-1],
          [string], [value-2], [if-value-2], ...,
          [default])
    ```

Macro: **m4\_cond**(test-1, value-1, if-value-1, [test-2], [value-2], [if-value-2], …, [default]) [¶](#index-m4_005fcond-1)
:   This macro was introduced in Autoconf 2.62. Similar to `m4_if`,
    except that each test is expanded only when it is encountered.
    This is useful for short-circuiting expensive tests; while `m4_if`
    requires all its strings to be expanded up front before doing
    comparisons, `m4_cond` only expands a test when all earlier
    tests have failed.

    For an example, these two sequences give the same result, but in the
    case where ‘`$1`’ does not contain a backslash, the `m4_cond`
    version only expands `m4_index` once, instead of five times, for
    faster computation if this is a common case for ‘`$1`’. Notice that
    every third argument is unquoted for `m4_if`, and quoted for
    `m4_cond`:

    ```
    m4_if(m4_index([$1], [\]), [-1], [$2],
          m4_eval(m4_index([$1], [\\]) >= 0), [1], [$2],
          m4_eval(m4_index([$1], [\$]) >= 0), [1], [$2],
          m4_eval(m4_index([$1], [\`]) >= 0), [1], [$3],
          m4_eval(m4_index([$1], [\"]) >= 0), [1], [$3],
          [$2])
    m4_cond([m4_index([$1], [\])], [-1], [$2],
            [m4_eval(m4_index([$1], [\\]) >= 0)], [1], [$2],
            [m4_eval(m4_index([$1], [\$]) >= 0)], [1], [$2],
            [m4_eval(m4_index([$1], [\`]) >= 0)], [1], [$3],
            [m4_eval(m4_index([$1], [\"]) >= 0)], [1], [$3],
            [$2])
    ```

Macro: **m4\_default**(expr-1, expr-2) [¶](#index-m4_005fdefault-1)

Macro: **m4\_default\_quoted**(expr-1, expr-2) [¶](#index-m4_005fdefault_005fquoted-1)

Macro: **m4\_default\_nblank**(expr-1, [expr-2]) [¶](#index-m4_005fdefault_005fnblank-1)

Macro: **m4\_default\_nblank\_quoted**(expr-1, [expr-2]) [¶](#index-m4_005fdefault_005fnblank_005fquoted-1)
:   If expr-1 contains text, use it. Otherwise, select expr-2.
    `m4_default` expands the result, while `m4_default_quoted`
    does not. Useful for providing a fixed default if the expression that
    results in expr-1 would otherwise be empty. The difference
    between `m4_default` and `m4_default_nblank` is whether an
    argument consisting of just blanks (space, tab, newline) is
    significant. When using the expanding versions, note that an argument
    may contain text but still expand to an empty string.

    ```
    m4_define([active], [ACTIVE])dnl
    m4_define([empty], [])dnl
    m4_define([demo1], [m4_default([$1], [$2])])dnl
    m4_define([demo2], [m4_default_quoted([$1], [$2])])dnl
    m4_define([demo3], [m4_default_nblank([$1], [$2])])dnl
    m4_define([demo4], [m4_default_nblank_quoted([$1], [$2])])dnl
    demo1([active], [default])
    ⇒ACTIVE
    demo1([], [active])
    ⇒ACTIVE
    demo1([empty], [text])
    ⇒
    -demo1([ ], [active])-
    ⇒- -
    demo2([active], [default])
    ⇒active
    demo2([], [active])
    ⇒active
    demo2([empty], [text])
    ⇒empty
    -demo2([ ], [active])-
    ⇒- -
    demo3([active], [default])
    ⇒ACTIVE
    demo3([], [active])
    ⇒ACTIVE
    demo3([empty], [text])
    ⇒
    -demo3([ ], [active])-
    ⇒-ACTIVE-
    demo4([active], [default])
    ⇒active
    demo4([], [active])
    ⇒active
    demo4([empty], [text])
    ⇒empty
    -demo4([ ], [active])-
    ⇒-active-
    ```

Macro: **m4\_define\_default**(macro, [default-definition]) [¶](#index-m4_005fdefine_005fdefault-1)
:   If macro does not already have a definition, then define it to
    default-definition.

Macro: **m4\_ifblank**(cond, [if-blank], [if-text]) [¶](#index-m4_005fifblank-1)

Macro: **m4\_ifnblank**(cond, [if-text], [if-blank]) [¶](#index-m4_005fifnblank-1)
:   If cond is empty or consists only of blanks (space, tab, newline),
    then expand if-blank; otherwise, expand if-text. Two
    variants exist, in order to make it easier to select the correct logical
    sense when using only two parameters. Note that this is more efficient
    than the equivalent behavior of:

    ```
    m4_ifval(m4_normalize([cond]), if-text, if-blank)
    ```

Macro: **m4\_ifndef**(macro, if-not-defined, [if-defined]) [¶](#index-m4_005fifndef-1)
:   This is shorthand for:

    ```
    m4_ifdef([macro], [if-defined], [if-not-defined])
    ```

Macro: **m4\_ifset**(macro, [if-true], [if-false]) [¶](#index-m4_005fifset-1)
:   If macro is undefined, or is defined as the empty string, expand
    to if-false. Otherwise, expands to if-true. Similar to:

    ```
    m4_ifval(m4_defn([macro]), [if-true], [if-false])
    ```

    except that it is not an error if macro is undefined.

Macro: **m4\_ifval**(cond, [if-true], [if-false]) [¶](#index-m4_005fifval-1)
:   Expands to if-true if cond is not empty, otherwise to
    if-false. This is shorthand for:

    ```
    m4_if([cond], [], [if-false], [if-true])
    ```

Macro: **m4\_ifvaln**(cond, [if-true], [if-false]) [¶](#index-m4_005fifvaln-1)
:   Similar to `m4_ifval`, except guarantee that a newline is present
    after any non-empty expansion. Often followed by `dnl`.

Macro: **m4\_n**(text) [¶](#index-m4_005fn-1)
:   Expand to text, and add a newline if text is not empty.
    Often followed by `dnl`.

---

Next: [Evaluation Macros](#Evaluation-Macros), Previous: [Conditional constructs](#Conditional-constructs), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.5 Looping constructs [¶](#Looping-constructs-1)

The following macros are useful in implementing recursive algorithms in
M4, including loop operations. An M4 list is formed by quoting a list
of quoted elements; generally the lists are comma-separated, although
`m4_foreach_w` is whitespace-separated. For example, the list
‘`[[a], [b,c]]`’ contains two elements: ‘`[a]`’ and ‘`[b,c]`’.
It is common to see lists with unquoted elements when those elements are
not likely to be macro names, as in ‘`[fputc_unlocked,
fgetc_unlocked]`’.

Although not generally recommended, it is possible for quoted lists to
have side effects; all side effects are expanded only once, and prior to
visiting any list element. On the other hand, the fact that unquoted
macros are expanded exactly once means that macros without side effects
can be used to generate lists. For example,

```
m4_foreach([i], [[1], [2], [3]m4_errprintn([hi])], [i])
error→hi
⇒123
m4_define([list], [[1], [2], [3]])
⇒
m4_foreach([i], [list], [i])
⇒123
```

Macro: **m4\_argn**(n, [arg]…) [¶](#index-m4_005fargn-1)
:   Extracts argument n (larger than 0) from the remaining arguments.
    If there are too few arguments, the empty string is used. For any
    n besides 1, this is more efficient than the similar
    ‘`m4_car(m4_shiftn([n], [], [arg…]))`’.

Macro: **m4\_car**(arg…) [¶](#index-m4_005fcar-1)
:   Expands to the quoted first arg. Can be used with `m4_cdr`
    to recursively iterate
    through a list. Generally, when using quoted lists of quoted elements,
    `m4_car` should be called without any extra quotes.

Macro: **m4\_cdr**(arg…) [¶](#index-m4_005fcdr-1)
:   Expands to a quoted list of all but the first arg, or the empty
    string if there was only one argument. Generally, when using quoted
    lists of quoted elements, `m4_cdr` should be called without any
    extra quotes.

    For example, this is a simple implementation of `m4_map`; note how
    each iteration checks for the end of recursion, then merely applies the
    first argument to the first element of the list, then repeats with the
    rest of the list. (The actual implementation in M4sugar is a bit more
    involved, to gain some speed and share code with `m4_map_sep`, and
    also to avoid expanding side effects in ‘`$2`’ twice).

    ```
    m4_define([m4_map], [m4_ifval([$2],
      [m4_apply([$1], m4_car($2))[]$0([$1], m4_cdr($2))])])dnl
    m4_map([ m4_eval], [[[1]], [[1+1]], [[10],[16]]])
    ⇒ 1 2 a
    ```

Macro: **m4\_for**(var, first, last, [step], expression) [¶](#index-m4_005ffor-1)
:   Loop over the numeric values between first and last
    including bounds by increments of step. For each iteration,
    expand expression with the numeric value assigned to var.
    If step is omitted, it defaults to ‘`1`’ or ‘`-1`’ depending
    on the order of the limits. If given, step has to match this
    order. The number of iterations is determined independently from
    definition of var; iteration cannot be short-circuited or
    lengthened by modifying var from within expression.

Macro: **m4\_foreach**(var, list, expression) [¶](#index-m4_005fforeach-1)
:   Loop over the comma-separated M4 list list, assigning each value
    to var, and expand expression. The following example
    outputs two lines:

    ```
    m4_foreach([myvar], [[foo], [bar, baz]],
               [echo myvar
    ])dnl
    ⇒echo foo
    ⇒echo bar, baz
    ```

    Note that for some forms of expression, it may be faster to use
    `m4_map_args`.

Macro: **m4\_foreach\_w**(var, list, expression) [¶](#index-m4_005fforeach_005fw-1)
:   Loop over the white-space-separated list list, assigning each value
    to var, and expand expression. If var is only
    referenced once in expression, it is more efficient to use
    `m4_map_args_w`.

    The deprecated macro `AC_FOREACH` is an alias of
    `m4_foreach_w`.

Macro: **m4\_map**(macro, list) [¶](#index-m4_005fmap-1)

Macro: **m4\_mapall**(macro, list) [¶](#index-m4_005fmapall-1)

Macro: **m4\_map\_sep**(macro, separator, list) [¶](#index-m4_005fmap_005fsep-1)

Macro: **m4\_mapall\_sep**(macro, separator, list) [¶](#index-m4_005fmapall_005fsep-1)
:   Loop over the comma separated quoted list of argument descriptions in
    list, and invoke macro with the arguments. An argument
    description is in turn a comma-separated quoted list of quoted elements,
    suitable for `m4_apply`. The macros `m4_map` and
    `m4_map_sep` ignore empty argument descriptions, while
    `m4_mapall` and `m4_mapall_sep` invoke macro with no
    arguments. The macros `m4_map_sep` and `m4_mapall_sep`
    additionally expand separator between invocations of macro.

    Note that separator is expanded, unlike in `m4_join`. When
    separating output with commas, this means that the map result can be
    used as a series of arguments, by using a single-quoted comma as
    separator, or as a single string, by using a double-quoted comma.

    ```
    m4_map([m4_count], [])
    ⇒
    m4_map([ m4_count], [[],
                         [[1]],
                         [[1], [2]]])
    ⇒ 1 2
    m4_mapall([ m4_count], [[],
                            [[1]],
                            [[1], [2]]])
    ⇒ 0 1 2
    m4_map_sep([m4_eval], [,], [[[1+2]],
                                [[10], [16]]])
    ⇒3,a
    m4_map_sep([m4_echo], [,], [[[a]], [[b]]])
    ⇒a,b
    m4_count(m4_map_sep([m4_echo], [,], [[[a]], [[b]]]))
    ⇒2
    m4_map_sep([m4_echo], [[,]], [[[a]], [[b]]])
    ⇒a,b
    m4_count(m4_map_sep([m4_echo], [[,]], [[[a]], [[b]]]))
    ⇒1
    ```

Macro: **m4\_map\_args**(macro, arg…) [¶](#index-m4_005fmap_005fargs-1)
:   Repeatedly invoke macro with each successive arg as its only
    argument. In the following example, three solutions are presented with
    the same expansion; the solution using `m4_map_args` is the most
    efficient.

    ```
    m4_define([active], [ACTIVE])dnl
    m4_foreach([var], [[plain], [active]], [ m4_echo(m4_defn([var]))])
    ⇒ plain active
    m4_map([ m4_echo], [[[plain]], [[active]]])
    ⇒ plain active
    m4_map_args([ m4_echo], [plain], [active])
    ⇒ plain active
    ```

    In cases where it is useful to operate on additional parameters besides
    the list elements, the macro `m4_curry` can be used in macro
    to supply the argument currying necessary to generate the desired
    argument list. In the following example, `list_add_n` is more
    efficient than `list_add_x`. On the other hand, using
    `m4_map_args_sep` can be even more efficient.

    ```
    m4_define([list], [[1], [2], [3]])dnl
    m4_define([add], [m4_eval(([$1]) + ([$2]))])dnl
    dnl list_add_n(N, ARG...)
    dnl Output a list consisting of each ARG added to N
    m4_define([list_add_n],
    [m4_shift(m4_map_args([,m4_curry([add], [$1])], m4_shift($@)))])dnl
    list_add_n([1], list)
    ⇒2,3,4
    list_add_n([2], list)
    ⇒3,4,5
    m4_define([list_add_x],
    [m4_shift(m4_foreach([var], m4_dquote(m4_shift($@)),
      [,add([$1],m4_defn([var]))]))])dnl
    list_add_x([1], list)
    ⇒2,3,4
    ```

Macro: **m4\_map\_args\_pair**(macro, [macro-end = macro], arg…) [¶](#index-m4_005fmap_005fargs_005fpair-1)
:   For every pair of arguments arg, invoke macro with two
    arguments. If there is an odd number of arguments, invoke
    macro-end, which defaults to macro, with the remaining
    argument.

    ```
    m4_map_args_pair([, m4_reverse], [], [1], [2], [3])
    ⇒, 2, 1, 3
    m4_map_args_pair([, m4_reverse], [, m4_dquote], [1], [2], [3])
    ⇒, 2, 1, [3]
    m4_map_args_pair([, m4_reverse], [, m4_dquote], [1], [2], [3], [4])
    ⇒, 2, 1, 4, 3
    ```

Macro: **m4\_map\_args\_sep**([pre], [post], [sep], arg…) [¶](#index-m4_005fmap_005fargs_005fsep-1)
:   Expand the sequence `pre[arg]post` for each
    argument, additionally expanding sep between arguments. One
    common use of this macro is constructing a macro call, where the opening
    and closing parentheses are split between pre and post; in
    particular, `m4_map_args([macro], [arg])` is equivalent
    to `m4_map_args_sep([macro(], [)], [], [arg])`. This
    macro provides the most efficient means for iterating over an arbitrary
    list of arguments, particularly when repeatedly constructing a macro
    call with more arguments than arg.

Macro: **m4\_map\_args\_w**(string, [pre], [post], [sep]) [¶](#index-m4_005fmap_005fargs_005fw-1)
:   Expand the sequence `pre[word]post` for each word in
    the whitespace-separated string, additionally expanding sep
    between words. This macro provides the most efficient means for
    iterating over a whitespace-separated string. In particular,
    `m4_map_args_w([string], [action(], [)])` is more
    efficient than `m4_foreach_w([var], [string],
    [action(m4_defn([var]))])`.

Macro: **m4\_shiftn**(count, …) [¶](#index-m4_005fshiftn-1)

Macro: **m4\_shift2**(…) [¶](#index-m4_005fshift2-1)

Macro: **m4\_shift3**(…) [¶](#index-m4_005fshift3-1)
:   `m4_shiftn` performs count iterations of `m4_shift`,
    along with validation that enough arguments were passed in to match the
    shift count, and that the count is positive. `m4_shift2` and
    `m4_shift3` are specializations
    of `m4_shiftn`, introduced in Autoconf 2.62, and are more efficient
    for two and three shifts, respectively.

Macro: **m4\_stack\_foreach**(macro, action) [¶](#index-m4_005fstack_005fforeach-1)

Macro: **m4\_stack\_foreach\_lifo**(macro, action) [¶](#index-m4_005fstack_005fforeach_005flifo-1)
:   For each of the `m4_pushdef` definitions of macro, expand
    action with the single argument of a definition of macro.
    `m4_stack_foreach` starts with the oldest definition, while
    `m4_stack_foreach_lifo` starts with the current definition.
    action should not push or pop definitions of macro, nor is
    there any guarantee that the current definition of macro matches
    the argument that was passed to action. The macro `m4_curry`
    can be used if action needs more than one argument, although in
    that case it is more efficient to use m4\_stack\_foreach\_sep.

    Due to technical limitations, there are a few low-level m4sugar
    functions, such as `m4_pushdef`, that cannot be used as the
    macro argument.

    ```
    m4_pushdef([a], [1])m4_pushdef([a], [2])dnl
    m4_stack_foreach([a], [ m4_incr])
    ⇒ 2 3
    m4_stack_foreach_lifo([a], [ m4_curry([m4_substr], [abcd])])
    ⇒ cd bcd
    ```

Macro: **m4\_stack\_foreach\_sep**(macro, [pre], [post], [sep]) [¶](#index-m4_005fstack_005fforeach_005fsep-1)

Macro: **m4\_stack\_foreach\_sep\_lifo**(macro, [pre], [post], [sep]) [¶](#index-m4_005fstack_005fforeach_005fsep_005flifo-1)
:   Expand the sequence `pre[definition]post` for each
    `m4_pushdef` definition of macro, additionally expanding
    sep between definitions. `m4_stack_foreach_sep` visits the
    oldest definition first, while `m4_stack_foreach_sep_lifo` visits
    the current definition first. This macro provides the most efficient
    means for iterating over a pushdef stack. In particular,
    `m4_stack_foreach([macro], [action])` is short for
    `m4_stack_foreach_sep([macro], [action(], [)])`.

---

Next: [String manipulation in M4](#Text-processing-Macros), Previous: [Looping constructs](#Looping-constructs), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.6 Evaluation Macros [¶](#Evaluation-Macros-1)

The following macros give some control over the order of the evaluation
by adding or removing levels of quotes.

Macro: **m4\_apply**(macro, list) [¶](#index-m4_005fapply-1)
:   Apply the elements of the quoted, comma-separated list as the
    arguments to macro. If list is empty, invoke macro
    without arguments. Note the difference between `m4_indir`, which
    expects its first argument to be a macro name but can use names that are
    otherwise invalid, and `m4_apply`, where macro can contain
    other text, but must end in a valid macro name.

    ```
    m4_apply([m4_count], [])
    ⇒0
    m4_apply([m4_count], [[]])
    ⇒1
    m4_apply([m4_count], [[1], [2]])
    ⇒2
    m4_apply([m4_join], [[|], [1], [2]])
    ⇒1|2
    ```

Macro: **m4\_count**(arg, …) [¶](#index-m4_005fcount-1)
:   This macro returns the number of arguments it was passed.

Macro: **m4\_curry**(macro, arg…) [¶](#index-m4_005fcurry-1)
:   This macro performs argument currying. The expansion of this macro is
    another macro name that expects exactly one argument; that argument is
    then appended to the arg list, and then macro is expanded
    with the resulting argument list.

    ```
    m4_curry([m4_curry], [m4_reverse], [1])([2])([3])
    ⇒3, 2, 1
    ```

    Unfortunately, due to a limitation in M4 1.4.x, it is not possible to
    pass the definition of a builtin macro as the argument to the output of
    `m4_curry`; the empty string is used instead of the builtin token.
    This behavior is rectified by using M4 1.6 or newer.

Macro: **m4\_do**(arg, …) [¶](#index-m4_005fdo-1)
:   This macro loops over its arguments and expands each arg in
    sequence. Its main use is for readability; it allows the use of
    indentation and fewer `dnl` to result in the same expansion. This
    macro guarantees that no expansion will be concatenated with subsequent
    text; to achieve full concatenation, use `m4_unquote(m4_join([],
    arg…))`.

    ```
    m4_define([ab],[1])m4_define([bc],[2])m4_define([abc],[3])dnl
    m4_do([a],[b])c
    ⇒abc
    m4_unquote(m4_join([],[a],[b]))c
    ⇒3
    m4_define([a],[A])m4_define([b],[B])m4_define([c],[C])dnl
    m4_define([AB],[4])m4_define([BC],[5])m4_define([ABC],[6])dnl
    m4_do([a],[b])c
    ⇒ABC
    m4_unquote(m4_join([],[a],[b]))c
    ⇒3
    ```

Macro: **m4\_dquote**(arg, …) [¶](#index-m4_005fdquote-1)
:   Return the arguments as a quoted list of quoted arguments.
    Conveniently, if there is just one arg, this effectively adds a
    level of quoting.

Macro: **m4\_dquote\_elt**(arg, …) [¶](#index-m4_005fdquote_005felt-1)
:   Return the arguments as a series of double-quoted arguments. Whereas
    `m4_dquote` returns a single argument, `m4_dquote_elt` returns
    as many arguments as it was passed.

Macro: **m4\_echo**(arg, …) [¶](#index-m4_005fecho-1)
:   Return the arguments, with the same level of quoting. Other than
    discarding whitespace after unquoted commas, this macro is a no-op.

Macro: **m4\_expand**(arg) [¶](#index-m4_005fexpand-1)
:   Return the expansion of arg as a quoted string. Whereas
    `m4_quote` is designed to collect expanded text into a single
    argument, `m4_expand` is designed to perform one level of expansion
    on quoted text. One distinction is in the treatment of whitespace
    following a comma in the original arg. Any time multiple
    arguments are collected into one with `m4_quote`, the M4 argument
    collection rules discard the whitespace. However, with `m4_expand`,
    whitespace is preserved, even after the expansion of macros contained in
    arg. Additionally, `m4_expand` is able to expand text that
    would involve an unterminated comment, whereas expanding that same text
    as the argument to `m4_quote` runs into difficulty in finding the
    end of the argument. Since manipulating diversions during argument
    collection is inherently unsafe, `m4_expand` issues an error if
    arg attempts to change the current diversion (see [Diversion support](#Diversion-support)).

    ```
    m4_define([active], [ACT, IVE])dnl
    m4_define([active2], [[ACT, IVE]])dnl
    m4_quote(active, active)
    ⇒ACT,IVE,ACT,IVE
    m4_expand([active, active])
    ⇒ACT, IVE, ACT, IVE
    m4_quote(active2, active2)
    ⇒ACT, IVE,ACT, IVE
    m4_expand([active2, active2])
    ⇒ACT, IVE, ACT, IVE
    m4_expand([# m4_echo])
    ⇒# m4_echo
    m4_quote(# m4_echo)
    )
    ⇒# m4_echo)
    ⇒
    ```

    Note that `m4_expand` cannot handle an arg that expands to
    literal unbalanced quotes, but that quadrigraphs can be used when
    unbalanced output is necessary. Likewise, unbalanced parentheses should
    be supplied with double quoting or a quadrigraph.

    ```
    m4_define([pattern], [[!@<:@]])dnl
    m4_define([bar], [BAR])dnl
    m4_expand([case $foo in
      m4_defn([pattern])@:}@ bar ;;
      *[)] blah ;;
    esac])
    ⇒case $foo in
    ⇒  [![]) BAR ;;
    ⇒  *) blah ;;
    ⇒esac
    ```

Macro: **m4\_ignore**(…) [¶](#index-m4_005fignore-1)
:   This macro was introduced in Autoconf 2.62. Expands to nothing,
    ignoring all of its arguments. By itself, this isn’t very useful.
    However, it can be used to conditionally ignore an arbitrary number of
    arguments, by deciding which macro name to apply to a list of arguments.

    ```
    dnl foo outputs a message only if [debug] is defined.
    m4_define([foo],
    [m4_ifdef([debug],[AC_MSG_NOTICE],[m4_ignore])([debug message])])
    ```

    Note that for earlier versions of Autoconf, the macro `__gnu__` can
    serve the same purpose, although it is less readable.

Macro: **m4\_make\_list**(arg, …) [¶](#index-m4_005fmake_005flist-1)
:   This macro exists to aid debugging of M4sugar algorithms. Its net
    effect is similar to `m4_dquote`—it produces a quoted list of
    quoted arguments, for each arg. The difference is that this
    version uses a comma-newline separator instead of just comma, to improve
    readability of the list; with the result that it is less efficient than
    `m4_dquote`.

    ```
    m4_define([zero],[0])m4_define([one],[1])m4_define([two],[2])dnl
    m4_dquote(zero, [one], [[two]])
    ⇒[0],[one],[[two]]
    m4_make_list(zero, [one], [[two]])
    ⇒[0],
    ⇒[one],
    ⇒[[two]]
    m4_foreach([number], m4_dquote(zero, [one], [[two]]), [ number])
    ⇒ 0 1 two
    m4_foreach([number], m4_make_list(zero, [one], [[two]]), [ number])
    ⇒ 0 1 two
    ```

Macro: **m4\_quote**(arg, …) [¶](#index-m4_005fquote-1)
:   Return the arguments as a single entity, i.e., wrap them into a pair of
    quotes. This effectively collapses multiple arguments into one,
    although it loses whitespace after unquoted commas in the process.

Macro: **m4\_reverse**(arg, …) [¶](#index-m4_005freverse-1)
:   Outputs each argument with the same level of quoting, but in reverse
    order, and with space following each comma for readability.

    ```
    m4_define([active], [ACT,IVE])
    ⇒
    m4_reverse(active, [active])
    ⇒active, IVE, ACT
    ```

Macro: **m4\_unquote**(arg, …) [¶](#index-m4_005funquote-1)
:   This macro was introduced in Autoconf 2.62. Expand each argument,
    separated by commas. For a single arg, this effectively removes a
    layer of quoting, and `m4_unquote([arg])` is more efficient
    than the equivalent `m4_do([arg])`. For multiple arguments,
    this results in an unquoted list of expansions. This is commonly used
    with `m4_split`, in order to convert a single quoted list into a
    series of quoted elements.

The following example aims at emphasizing the difference between several
scenarios: not using these macros, using `m4_defn`, using
`m4_quote`, using `m4_dquote`, and using `m4_expand`.

```
$ cat example.m4
dnl Overquote, so that quotes are visible.
m4_define([show], [$[]1 = [$1], $[]@ = [$@]])
m4_define([a], [A])
m4_define([mkargs], [1, 2[,] 3])
m4_define([arg1], [[$1]])
m4_divert([0])dnl
show(a, b)
show([a, b])
show(m4_quote(a, b))
show(m4_dquote(a, b))
show(m4_expand([a, b]))

arg1(mkargs)
arg1([mkargs])
arg1(m4_defn([mkargs]))
arg1(m4_quote(mkargs))
arg1(m4_dquote(mkargs))
arg1(m4_expand([mkargs]))
$ autom4te -l m4sugar example.m4
$1 = A, $@ = [A],[b]
$1 = a, b, $@ = [a, b]
$1 = A,b, $@ = [A,b]
$1 = [A],[b], $@ = [[A],[b]]
$1 = A, b, $@ = [A, b]

1
mkargs
1, 2[,] 3
1,2, 3
[1],[2, 3]
1, 2, 3
```

---

Next: [Arithmetic computation in M4](#Number-processing-Macros), Previous: [Evaluation Macros](#Evaluation-Macros), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.7 String manipulation in M4 [¶](#String-manipulation-in-M4)

The following macros may be used to manipulate strings in M4. Many of
the macros in this section intentionally result in quoted strings as
output, rather than subjecting the arguments to further expansions. As
a result, if you are manipulating text that contains active M4
characters, the arguments are passed with single quoting rather than
double.

Macro: **m4\_append**(macro-name, string, [separator]) [¶](#index-m4_005fappend-1)

Macro: **m4\_append\_uniq**(macro-name, string, [separator] [if-uniq], [if-duplicate]) [¶](#index-m4_005fappend_005funiq-1)
:   Redefine macro-name to its former contents with separator
    and string added at the end. If macro-name was undefined
    before (but not if it was defined but empty), then no separator is
    added. As of Autoconf 2.62, neither string nor separator
    are expanded during this macro; instead, they are expanded when
    macro-name is invoked.

    `m4_append` can be used to grow strings, and `m4_append_uniq`
    to grow strings without duplicating substrings. Additionally,
    `m4_append_uniq` takes two optional parameters as of Autoconf 2.62;
    if-uniq is expanded if string was appended, and
    if-duplicate is expanded if string was already present.
    Also, `m4_append_uniq` warns if separator is not empty, but
    occurs within string, since that can lead to duplicates.

    Note that `m4_append` can scale linearly in the length of the final
    string, depending on the quality of the underlying M4 implementation,
    while `m4_append_uniq` has an inherent quadratic scaling factor.
    If an algorithm can tolerate duplicates in the final string, use the
    former for speed. If duplicates must be avoided, consider using
    `m4_set_add` instead (see [Set manipulation in M4](#Set-manipulation-Macros)).

    ```
    m4_define([active], [ACTIVE])dnl
    m4_append([sentence], [This is an])dnl
    m4_append([sentence], [ active ])dnl
    m4_append([sentence], [symbol.])dnl
    sentence
    ⇒This is an ACTIVE symbol.
    m4_undefine([active])dnl
    ⇒This is an active symbol.
    m4_append_uniq([list], [one], [, ], [new], [existing])
    ⇒new
    m4_append_uniq([list], [one], [, ], [new], [existing])
    ⇒existing
    m4_append_uniq([list], [two], [, ], [new], [existing])
    ⇒new
    m4_append_uniq([list], [three], [, ], [new], [existing])
    ⇒new
    m4_append_uniq([list], [two], [, ], [new], [existing])
    ⇒existing
    list
    ⇒one, two, three
    m4_dquote(list)
    ⇒[one],[two],[three]
    m4_append([list2], [one], [[, ]])dnl
    m4_append_uniq([list2], [two], [[, ]])dnl
    m4_append([list2], [three], [[, ]])dnl
    list2
    ⇒one, two, three
    m4_dquote(list2)
    ⇒[one, two, three]
    ```

Macro: **m4\_append\_uniq\_w**(macro-name, strings) [¶](#index-m4_005fappend_005funiq_005fw-1)
:   This macro was introduced in Autoconf 2.62. It is similar to
    `m4_append_uniq`, but treats strings as a whitespace
    separated list of words to append, and only appends unique words.
    macro-name is updated with a single space between new words.

    ```
    m4_append_uniq_w([numbers], [1 1 2])dnl
    m4_append_uniq_w([numbers], [ 2 3 ])dnl
    numbers
    ⇒1 2 3
    ```

Macro: **m4\_chomp**(string) [¶](#index-m4_005fchomp-1)

Macro: **m4\_chomp\_all**(string) [¶](#index-m4_005fchomp_005fall-1)
:   Output string in quotes, but without a trailing newline. The
    macro `m4_chomp` is slightly faster, and removes at most one
    newline; the macro `m4_chomp_all` removes all consecutive trailing
    newlines. Unlike `m4_flatten`, embedded newlines are left intact,
    and backslash does not influence the result.

Macro: **m4\_combine**([separator], prefix-list, [infix], suffix-1, [suffix-2], …) [¶](#index-m4_005fcombine-1)
:   This macro produces a quoted string containing the pairwise combination
    of every element of the quoted, comma-separated prefix-list, and
    every element from the suffix arguments. Each pairwise
    combination is joined with infix in the middle, and successive
    pairs are joined by separator. No expansion occurs on any of the
    arguments. No output occurs if either the prefix or suffix
    list is empty, but the lists can contain empty elements.

    ```
    m4_define([a], [oops])dnl
    m4_combine([, ], [[a], [b], [c]], [-], [1], [2], [3])
    ⇒a-1, a-2, a-3, b-1, b-2, b-3, c-1, c-2, c-3
    m4_combine([, ], [[a], [b]], [-])
    ⇒
    m4_combine([, ], [[a], [b]], [-], [])
    ⇒a-, b-
    m4_combine([, ], [], [-], [1], [2])
    ⇒
    m4_combine([, ], [[]], [-], [1], [2])
    ⇒-1, -2
    ```

Macro: **m4\_escape**(string) [¶](#index-m4_005fescape-1)
:   Convert all instances of ‘`[`’, ‘`]`’, ‘`#`’, and ‘`$`’
    within string into their respective quadrigraphs. The result is
    still a quoted string.

Macro: **m4\_flatten**(string) [¶](#index-m4_005fflatten-1)
:   Flatten string into a single line. Delete all backslash-newline
    pairs, and replace all remaining newlines with a space. The result is
    still a quoted string.

Macro: **m4\_join**([separator], args…) [¶](#index-m4_005fjoin-1)

Macro: **m4\_joinall**([separator], args…) [¶](#index-m4_005fjoinall-1)
:   Concatenate each arg, separated by separator.
    `joinall` uses every argument, while `join` omits empty
    arguments so that there are no back-to-back separators in the output.
    The result is a quoted string.

    ```
    m4_define([active], [ACTIVE])dnl
    m4_join([|], [one], [], [active], [two])
    ⇒one|active|two
    m4_joinall([|], [one], [], [active], [two])
    ⇒one||active|two
    ```

    Note that if all you intend to do is join args with commas between
    them, to form a quoted list suitable for `m4_foreach`, it is more
    efficient to use `m4_dquote`.

Macro: **m4\_newline**([text]) [¶](#index-m4_005fnewline-1)
:   This macro was introduced in Autoconf 2.62, and expands to a newline,
    followed by any text.
    It is primarily useful for maintaining macro formatting, and ensuring
    that M4 does not discard leading whitespace during argument collection.

Macro: **m4\_normalize**(string) [¶](#index-m4_005fnormalize-1)
:   Remove leading and trailing spaces and tabs, sequences of
    backslash-then-newline, and replace multiple spaces, tabs, and newlines
    with a single space. This is a combination of `m4_flatten` and
    `m4_strip`. To determine if string consists only of bytes
    that would be removed by `m4_normalize`, you can use
    `m4_ifblank`.

Macro: **m4\_re\_escape**(string) [¶](#index-m4_005fre_005fescape-1)
:   Backslash-escape all characters in string that are active in
    regexps.

Macro: **m4\_split**(string, [regexp = ‘`[\t ]+`’]) [¶](#index-m4_005fsplit-1)
:   Split string into an M4 list of elements quoted by ‘`[`’ and
    ‘`]`’, while keeping white space at the beginning and at the end.
    If regexp is given, use it instead of ‘`[\t ]+`’ for splitting.
    If string is empty, the result is an empty list.

Macro: **m4\_strip**(string) [¶](#index-m4_005fstrip-1)
:   Strip whitespace from string. Sequences of spaces and tabs are
    reduced to a single space, then leading and trailing spaces are removed.
    The result is still a quoted string. Note that this does not interfere
    with newlines; if you want newlines stripped as well, consider
    `m4_flatten`, or do it all at once with `m4_normalize`. To
    quickly test if string has only whitespace, use `m4_ifblank`.

Macro: **m4\_text\_box**(message, [frame = ‘`-`’]) [¶](#index-m4_005ftext_005fbox-1)
:   Add a text box around message, using frame as the border
    character above and below the message. The frame argument must be
    a single byte, and does not support quadrigraphs.
    The frame correctly accounts for
    the subsequent expansion of message. For example:

    ```
    m4_define([macro], [abc])dnl
    m4_text_box([macro])
    ⇒## --- ##
    ⇒## abc ##
    ⇒## --- ##
    ```

    The message must contain balanced quotes and parentheses, although
    quadrigraphs can be used to work around this.

Macro: **m4\_text\_wrap**(string, [prefix], [prefix1 = prefix], [width = ‘`79`’]) [¶](#index-m4_005ftext_005fwrap-1)
:   Break string into a series of whitespace-separated words, then
    output those words separated by spaces, and wrapping lines any time the
    output would exceed width columns. If given, prefix1 begins
    the first line, and prefix begins all wrapped lines. If
    prefix1 is longer than prefix, then the first line consists
    of just prefix1. If prefix is longer than prefix1,
    padding is inserted so that the first word of string begins at the
    same indentation as all wrapped lines. Note that using literal tab
    characters in any of the arguments will interfere with the calculation
    of width. No expansions occur on prefix, prefix1, or the
    words of string, although quadrigraphs are recognized.

    For some examples:

    ```
    m4_text_wrap([Short string */], [   ], [/* ], [20])
    ⇒/* Short string */
    m4_text_wrap([Much longer string */], [   ], [/* ], [20])
    ⇒/* Much longer
    ⇒   string */
    m4_text_wrap([Short doc.], [          ], [  --short ], [30])
    ⇒  --short Short doc.
    m4_text_wrap([Short doc.], [          ], [  --too-wide ], [30])
    ⇒  --too-wide
    ⇒          Short doc.
    m4_text_wrap([Super long documentation.], [     ],
                 [  --too-wide ], 30)
    ⇒  --too-wide
    ⇒     Super long
    ⇒     documentation.
    ```

Macro: **m4\_tolower**(string) [¶](#index-m4_005ftolower-1)

Macro: **m4\_toupper**(string) [¶](#index-m4_005ftoupper-1)
:   Return string with letters converted to upper or lower case,
    respectively.

---

Next: [Set manipulation in M4](#Set-manipulation-Macros), Previous: [String manipulation in M4](#Text-processing-Macros), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.8 Arithmetic computation in M4 [¶](#Arithmetic-computation-in-M4)

The following macros facilitate integer arithmetic operations.

Where a parameter is documented as taking an arithmetic expression, you
can use anything that can be parsed by `m4_eval`.
Any other numeric parameter should consist of an optional sign followed
by one or more decimal digits; it is treated as a decimal integer.

Macros that expand to a number do so as either ‘`0`’, or an optional
‘`-`’ followed by a nonzero decimal digit followed by zero or more
decimal digits.

Due to `m4` limitations, arithmetic expressions and numeric
parameters should use only numbers that fit into a 32-bit signed
integer.

Macro: **m4\_cmp**(expr-1, expr-2) [¶](#index-m4_005fcmp-1)
:   Compare the arithmetic expressions expr-1 and expr-2, and
    expand to ‘`-1`’ if expr-1 is smaller, ‘`0`’ if they are
    equal, and ‘`1`’ if expr-1 is larger.

Macro: **m4\_list\_cmp**(list-1, list-2) [¶](#index-m4_005flist_005fcmp-1)
:   Compare the two M4 lists consisting of comma-separated arithmetic
    expressions, left to right. Expand to ‘`-1`’ for the first element
    pairing where the value from list-1 is smaller, ‘`1`’ where the
    value from list-2 is smaller, or ‘`0`’ if both lists have the
    same values. If one list is shorter than the other, the remaining
    elements of the longer list are compared against zero.

    ```
    m4_list_cmp([1, 0],       [1])
    ⇒0
    m4_list_cmp([1, [1 * 0]], [1, 0])
    ⇒0
    m4_list_cmp([1, 2],       [1, 0])
    ⇒1
    m4_list_cmp([1, [1+1], 3],[1, 2])
    ⇒1
    m4_list_cmp([1, 2, -3],   [1, 2])
    ⇒-1
    m4_list_cmp([1, 0],       [1, 2])
    ⇒-1
    m4_list_cmp([1],          [1, 2])
    ⇒-1
    ```

Macro: **m4\_max**(arg, …) [¶](#index-m4_005fmax-1)
:   This macro was introduced in Autoconf 2.62. Expand to the value
    of the maximum arithmetic expression among all the arguments.

Macro: **m4\_min**(arg, …) [¶](#index-m4_005fmin-1)
:   This macro was introduced in Autoconf 2.62. Expand to the value
    of the minimum arithmetic expression among all the arguments.

Macro: **m4\_sign**(expr) [¶](#index-m4_005fsign-1)
:   Expand to ‘`-1`’ if the arithmetic expression expr is negative,
    ‘`1`’ if it is positive, and ‘`0`’ if it is zero.

Macro: **m4\_version\_compare**(version-1, version-2) [¶](#index-m4_005fversion_005fcompare-1)
:   This macro was introduced in Autoconf 2.53, but had a number of
    usability limitations that were not lifted until Autoconf 2.62. Compare
    the version strings version-1 and version-2, and expand to
    ‘`-1`’ if version-1 is smaller, ‘`0`’ if they are the same,
    or ‘`1`’ version-2 is smaller. Version strings must be a list
    of elements separated by ‘`.`’, ‘`,`’ or ‘`-`’, where each
    element is a number along with optional case-insensitive letters
    designating beta releases. The comparison stops at the leftmost element
    that contains a difference, although a 0 element compares equal to a
    missing element.

    It is permissible to include commit identifiers in version, such
    as an abbreviated SHA1 of the commit, provided there is still a
    monotonically increasing prefix to allow for accurate version-based
    comparisons. For example, this paragraph was written when the
    development snapshot of autoconf claimed to be at version
    ‘`2.61a-248-dc51`’, or 248 commits after the 2.61a release, with an
    abbreviated commit identification of ‘`dc51`’.

    ```
    m4_version_compare([1.1], [2.0])
    ⇒-1
    m4_version_compare([2.0b], [2.0a])
    ⇒1
    m4_version_compare([1.1.1], [1.1.1a])
    ⇒-1
    m4_version_compare([1.2], [1.1.1a])
    ⇒1
    m4_version_compare([1.0], [1])
    ⇒0
    m4_version_compare([1.1pre], [1.1PRE])
    ⇒0
    m4_version_compare([1.1a], [1,10])
    ⇒-1
    m4_version_compare([2.61a], [2.61a-248-dc51])
    ⇒-1
    m4_version_compare([2.61b], [2.61a-248-dc51])
    ⇒1
    ```

Macro: **m4\_version\_prereq**(version, [if-new-enough], [if-old = ‘`m4_fatal`’]) [¶](#index-m4_005fversion_005fprereq-1)
:   Compares version against the version of Autoconf currently
    running. If the running version is at version or newer, expand
    if-new-enough, but if version is larger than the version
    currently executing, expand if-old, which defaults to printing an
    error message and exiting m4sugar with status 63. When given only one
    argument, this behaves like `AC_PREREQ` (see [Dealing with Autoconf versions](#Versioning)).
    Remember that the autoconf philosophy favors feature checks over version
    checks.

---

Next: [Forbidden Patterns](#Forbidden-Patterns), Previous: [Arithmetic computation in M4](#Number-processing-Macros), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.9 Set manipulation in M4 [¶](#Set-manipulation-in-M4)

Sometimes, it is necessary to track a set of data, where the order does
not matter and where there are no duplicates in the set. The following
macros facilitate set manipulations. Each set is an opaque object,
which can only be accessed via these basic operations. The underlying
implementation guarantees linear scaling for set creation, which is more
efficient than using the quadratic `m4_append_uniq`. Both set
names and values can be arbitrary strings, except for unbalanced quotes.
This implementation ties up memory for removed elements until the next
operation that must traverse all the elements of a set; and although
that may slow down some operations until the memory for removed elements
is pruned, it still guarantees linear performance.

Macro: **m4\_set\_add**(set, value, [if-uniq], [if-dup]) [¶](#index-m4_005fset_005fadd-1)
:   Adds the string value as a member of set set. Expand
    if-uniq if the element was added, or if-dup if it was
    previously in the set. Operates in amortized constant time, so that set
    creation scales linearly.

Macro: **m4\_set\_add\_all**(set, value…) [¶](#index-m4_005fset_005fadd_005fall-1)
:   Adds each value to the set set. This is slightly more
    efficient than repeatedly invoking `m4_set_add`.

Macro: **m4\_set\_contains**(set, value, [if-present], [if-absent]) [¶](#index-m4_005fset_005fcontains-1)
:   Expands if-present if the string value is a member of
    set, otherwise if-absent.

    ```
    m4_set_contains([a], [1], [yes], [no])
    ⇒no
    m4_set_add([a], [1], [added], [dup])
    ⇒added
    m4_set_add([a], [1], [added], [dup])
    ⇒dup
    m4_set_contains([a], [1], [yes], [no])
    ⇒yes
    m4_set_remove([a], [1], [removed], [missing])
    ⇒removed
    m4_set_contains([a], [1], [yes], [no])
    ⇒no
    m4_set_remove([a], [1], [removed], [missing])
    ⇒missing
    ```

Macro: **m4\_set\_contents**(set, [sep]) [¶](#index-m4_005fset_005fcontents-1)

Macro: **m4\_set\_dump**(set, [sep]) [¶](#index-m4_005fset_005fdump-1)
:   Expands to a single string consisting of all the members of the set
    set, each separated by sep, which is not expanded.
    `m4_set_contents` leaves the elements in set but reclaims any
    memory occupied by removed elements, while `m4_set_dump` is a
    faster one-shot action that also deletes the set. No provision is made
    for disambiguating members that contain a non-empty sep as a
    substring; use `m4_set_empty` to distinguish between an empty set
    and the set containing only the empty string. The order of the output
    is unspecified; in the current implementation, part of the speed of
    `m4_set_dump` results from using a different output order than
    `m4_set_contents`. These macros scale linearly in the size of the
    set before memory pruning, and `m4_set_contents([set],
    [sep])` is faster than
    `m4_joinall([sep]m4_set_listc([set]))`.

    ```
    m4_set_add_all([a], [1], [2], [3])
    ⇒
    m4_set_contents([a], [-])
    ⇒1-2-3
    m4_joinall([-]m4_set_listc([a]))
    ⇒1-2-3
    m4_set_dump([a], [-])
    ⇒3-2-1
    m4_set_contents([a])
    ⇒
    m4_set_add([a], [])
    ⇒
    m4_set_contents([a], [-])
    ⇒
    ```

Macro: **m4\_set\_delete**(set) [¶](#index-m4_005fset_005fdelete-1)
:   Delete all elements and memory associated with set. This is
    linear in the set size, and faster than removing one element at a time.

Macro: **m4\_set\_difference**(seta, setb) [¶](#index-m4_005fset_005fdifference-1)

Macro: **m4\_set\_intersection**(seta, setb) [¶](#index-m4_005fset_005fintersection-1)

Macro: **m4\_set\_union**(seta, setb) [¶](#index-m4_005fset_005funion-1)
:   Compute the relation between seta and setb, and output the
    result as a list of quoted arguments without duplicates and with a
    leading comma. Set difference selects the elements in seta but
    not setb, intersection selects only elements in both sets, and
    union selects elements in either set. These actions are linear in the
    sum of the set sizes. The leading comma is necessary to distinguish
    between no elements and the empty string as the only element.

    ```
    m4_set_add_all([a], [1], [2], [3])
    ⇒
    m4_set_add_all([b], [3], [], [4])
    ⇒
    m4_set_difference([a], [b])
    ⇒,1,2
    m4_set_difference([b], [a])
    ⇒,,4
    m4_set_intersection([a], [b])
    ⇒,3
    m4_set_union([a], [b])
    ⇒,1,2,3,,4
    ```

Macro: **m4\_set\_empty**(set, [if-empty], [if-elements]) [¶](#index-m4_005fset_005fempty-1)
:   Expand if-empty if the set set has no elements, otherwise
    expand if-elements. This macro operates in constant time. Using
    this macro can help disambiguate output from `m4_set_contents` or
    `m4_set_list`.

Macro: **m4\_set\_foreach**(set, variable, action) [¶](#index-m4_005fset_005fforeach-1)
:   For each element in the set set, expand action with the
    macro variable defined as the set element. Behavior is
    unspecified if action recursively lists the contents of set
    (although listing other sets is acceptable), or if it modifies the set
    in any way other than removing the element currently contained in
    variable. This macro is faster than the corresponding
    `m4_foreach([variable],
    m4_indir([m4_dquote]m4_set_listc([set])), [action])`,
    although `m4_set_map` might be faster still.

    ```
    m4_set_add_all([a]m4_for([i], [1], [5], [], [,i]))
    ⇒
    m4_set_contents([a])
    ⇒12345
    m4_set_foreach([a], [i],
      [m4_if(m4_eval(i&1), [0], [m4_set_remove([a], i, [i])])])
    ⇒24
    m4_set_contents([a])
    ⇒135
    ```

Macro: **m4\_set\_list**(set) [¶](#index-m4_005fset_005flist-1)

Macro: **m4\_set\_listc**(set) [¶](#index-m4_005fset_005flistc-1)
:   Produce a list of arguments, where each argument is a quoted element
    from the set set. The variant `m4_set_listc` is unambiguous,
    by adding a leading comma if there are any set elements, whereas the
    variant `m4_set_list` cannot distinguish between an empty set and a
    set containing only the empty string. These can be directly used in
    macros that take multiple arguments, such as `m4_join` or
    `m4_set_add_all`, or wrapped by `m4_dquote` for macros that
    take a quoted list, such as `m4_map` or `m4_foreach`. Any
    memory occupied by removed elements is reclaimed during these macros.

    ```
    m4_set_add_all([a], [1], [2], [3])
    ⇒
    m4_set_list([a])
    ⇒1,2,3
    m4_set_list([b])
    ⇒
    m4_set_listc([b])
    ⇒
    m4_count(m4_set_list([b]))
    ⇒1
    m4_set_empty([b], [0], [m4_count(m4_set_list([b]))])
    ⇒0
    m4_set_add([b], [])
    ⇒
    m4_set_list([b])
    ⇒
    m4_set_listc([b])
    ⇒,
    m4_count(m4_set_list([b]))
    ⇒1
    m4_set_empty([b], [0], [m4_count(m4_set_list([b]))])
    ⇒1
    ```

Macro: **m4\_set\_map**(set, action) [¶](#index-m4_005fset_005fmap-1)
:   For each element in the set set, expand action with a single
    argument of the set element. Behavior is unspecified if action
    recursively lists the contents of set (although listing other sets
    is acceptable), or if it modifies the set in any way other than removing
    the element passed as an argument. This macro is faster than either
    corresponding counterpart of
    `m4_map_args([action]m4_set_listc([set]))` or
    `m4_set_foreach([set], [var],
    [action(m4_defn([var]))])`. It is possible to use `m4_curry`
    if more than one argument is needed for action, although it is
    more efficient to use `m4_set_map_sep` in that case.

Macro: **m4\_set\_map\_sep**(set, [pre], [post], [sep]) [¶](#index-m4_005fset_005fmap_005fsep-1)
:   For each element in the set set, expand
    `pre[element]post`, additionally expanding sep
    between elements. Behavior is unspecified if the expansion recursively
    lists the contents of set (although listing other sets
    is acceptable), or if it modifies the set in any way other than removing
    the element visited by the expansion. This macro provides the most
    efficient means for non-destructively visiting the elements of a set; in
    particular, `m4_set_map([set], [action])` is equivalent
    to `m4_set_map_sep([set], [action(], [)])`.

Macro: **m4\_set\_remove**(set, value, [if-present], [if-absent]) [¶](#index-m4_005fset_005fremove-1)
:   If value is an element in the set set, then remove it and
    expand if-present. Otherwise expand if-absent. This macro
    operates in constant time so that multiple removals will scale linearly
    rather than quadratically; but when used outside of
    `m4_set_foreach` or `m4_set_map`, it leaves memory occupied
    until the set is later
    compacted by `m4_set_contents` or `m4_set_list`. Several
    other set operations are then less efficient between the time of element
    removal and subsequent memory compaction, but still maintain their
    guaranteed scaling performance.

Macro: **m4\_set\_size**(set) [¶](#index-m4_005fset_005fsize-1)
:   Expand to the size of the set set. This implementation operates
    in constant time, and is thus more efficient than
    `m4_eval(m4_count(m4_set_listc([set])) - 1)`.

---

Previous: [Set manipulation in M4](#Set-manipulation-Macros), Up: [Programming in M4sugar](#Programming-in-M4sugar)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]

#### 8.3.10 Forbidden Patterns [¶](#Forbidden-Patterns-1)

M4sugar provides a means to define suspicious patterns, patterns
describing tokens which should not be found in the output. For
instance, if an Autoconf `configure` script includes tokens such as
‘`AC_DEFINE`’, or ‘`dnl`’, then most probably something went
wrong (typically a macro was not evaluated because of overquotation).

M4sugar forbids all the tokens matching ‘`^_?m4_`’ and ‘`^dnl$`’.
Additional layers, such as M4sh and Autoconf, add additional forbidden
patterns to the list.

Macro: **m4\_pattern\_forbid**(pattern) [¶](#index-m4_005fpattern_005fforbid-1)
:   Declare that no token matching pattern must be found in the
    output. The output file is (temporarily) split into one word per line
    as part of the `autom4te` post-processing, with each line (and
    therefore word) then being checked against the Perl regular expression
    pattern. If the regular expression matches, and
    `m4_pattern_allow` does not also match, then an error is raised.

    Comments are not checked; this can be a problem if, for instance, you
    have some macro left unexpanded after an ‘`#include`’. No consensus
    is currently found in the Autoconf community, as some people consider it
    should be valid to name macros in comments (which doesn’t make sense to
    the authors of this documentation: input, such as macros, should be
    documented by ‘`dnl`’ comments; reserving ‘`#`’-comments to
    document the output).

    As an example, if you define your own macros that begin with ‘`M_`’
    and are composed from capital letters and underscores, the specification
    of `m4_pattern_forbid([^M_[A-Z_]+])` will ensure all your macros
    are expanded when not used in comments.

    As an example of a common use of this macro, consider what happens in
    packages that want to use the `pkg-config` script via the
    third-party `PKG_CHECK_MODULES` macro. By default, if a developer
    checks out the development tree but has not yet installed the pkg-config
    macros locally, they can manage to successfully run `autoconf`
    on the package, but the resulting `configure` file will likely
    result in a confusing shell message about a syntax error on the line
    mentioning the unexpanded `PKG_CHECK_MODULES` macro. On the other hand,
    if `configure.ac` includes `m4_pattern_forbid([^PKG_])`, the
    missing pkg-config macros will be detected immediately without allowing
    `autoconf` to succeed.

Of course, you might encounter exceptions to these generic rules, for
instance you might have to refer to ‘`$m4_flags`’.

Macro: **m4\_pattern\_allow**(pattern) [¶](#index-m4_005fpattern_005fallow-1)
:   Any token matching pattern is allowed, including if it matches an
    `m4_pattern_forbid` pattern.

    For example, Gnulib uses `m4_pattern_forbid([^gl_])` to reserve the
    `gl_` namespace for itself, but also uses
    `m4_pattern_allow([^gl_ES$])` to avoid a false negative on the
    valid locale name.

---

Previous: [Programming in M4sugar](#Programming-in-M4sugar), Up: [Programming in M4](#Programming-in-M4)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Indices "Index")]
