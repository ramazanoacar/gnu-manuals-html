### 6.4 Excluding Some Files

To avoid operating on files whose names match a particular pattern,
use the `--exclude` or `--exclude-from` options.

`--exclude=pattern`
:   Causes `tar` to ignore files that match the pattern.

The `--exclude=pattern` option prevents any file or
member whose name matches the shell wildcard (pattern) from
being operated on.
For example, to create an archive with all the contents of the directory
`src` except for files whose names end in `.o`, use the
command ‘`tar -cf src.tar --exclude='*.o' src`’.

You may give multiple `--exclude` options.

`--exclude-from=file`

`-X file`
:   Causes `tar` to ignore files that match the patterns listed in
    file.

Use the `--exclude-from` option to read a
list of patterns, one per line, from file; `tar` will
ignore files matching those patterns. Thus if `tar` is
called as ‘`tar -c -X foo .`’ and the file `foo` contains a
single line `*.o`, no files whose names end in `.o` will be
added to the archive.

Notice, that lines from file are read verbatim. One of the
frequent errors is leaving some extra whitespace after a file name,
which is difficult to catch using text editors.

However, empty lines are OK.

When archiving directories that are under some version control system (VCS),
it is often convenient to read exclusion patterns from this VCS’
ignore files (e.g. `.cvsignore`, `.gitignore`, etc.) The
following options provide such possibility:

`--exclude-vcs-ignores`
:   Before archiving a directory, see if it contains any of the following
    files: `cvsignore`, `.gitignore`, `.bzrignore`, or
    `.hgignore`. If so, read ignore patterns from these files.

    The patterns are treated much as the corresponding VCS would treat
    them, i.e.:

    `.cvsignore`
    :   Contains shell-style globbing patterns that apply only to the
        directory where this file resides. No comments are allowed in the
        file. Empty lines are ignored.

    `.gitignore`
    :   Contains shell-style globbing patterns. Applies to the directory
        where `.gitfile` is located and all its subdirectories.

        Any line beginning with a ‘`#`’ is a comment. Backslash escapes
        the comment character.

    `.bzrignore`
    :   Contains shell globbing-patterns and regular expressions (if prefixed
        with ‘`RE:`’[17](#FOOT17). Patterns affect the directory and all its
        subdirectories.

        Any line beginning with a ‘`#`’ is a comment.

    `.hgignore`
    :   Contains POSIX regular expressions[18](#FOOT18). The line ‘`syntax:
        glob`’ switches to shell globbing patterns. The line ‘`syntax:
        regexp`’ switches back. Comments begin with a ‘`#`’. Patterns
        affect the directory and all its subdirectories.

`--exclude-ignore=file`
:   Before dumping a directory, `tar` checks if it contains
    file. If so, exclusion patterns are read from this file.
    The patterns affect only the directory itself.

`--exclude-ignore-recursive=file`
:   Same as `--exclude-ignore`, except that the patterns read
    affect both the directory where file resides and all its
    subdirectories.

`--exclude-vcs`
:   Exclude files and directories used by following version control
    systems: ‘`CVS`’, ‘`RCS`’, ‘`SCCS`’, ‘`SVN`’, ‘`Arch`’,
    ‘`Bazaar`’, ‘`Mercurial`’, and ‘`Darcs`’.

    As of version 1.35.90, the following files are excluded:

    * `CVS/`, and everything under it
    * `RCS/`, and everything under it
    * `SCCS/`, and everything under it
    * `.git/`, and everything under it
    * `.gitignore`
    * `.gitmodules`
    * `.gitattributes`
    * `.cvsignore`
    * `.svn/`, and everything under it
    * `.arch-ids/`, and everything under it
    * `{arch}/`, and everything under it
    * `=RELEASE-ID`
    * `=meta-update`
    * `=update`
    * `.bzr`
    * `.bzrignore`
    * `.bzrtags`
    * `.hg`
    * `.hgignore`
    * `.hgrags`
    * `_darcs`

`--exclude-backups`
:   Exclude backup and lock files. This option causes exclusion of files
    that match the following shell globbing patterns:

    .#\*

    \*~

    #\*#

When creating an archive, the `--exclude-caches` option family
causes `tar` to exclude all directories that contain a *cache
directory tag*. A cache directory tag is a regular file with the
well-known name `CACHEDIR.TAG` whose first 43 octets
consist of the following ASCII header string:

```
Signature: 8a477f597d28d172789f06886806bc55
```

Any remaining bytes are not relevant, in particular the signature
string does not have to be followed by an LF or CR/LF pair[19](#FOOT19).

Various applications write cache directory tags into directories they
use to hold regenerable, non-precious data, so that such data can be
more easily excluded from backups.

There are three ‘`exclude-caches`’ options, each providing a different
exclusion semantics:

`--exclude-caches`
:   Do not archive the contents of the directory, but archive the
    directory itself and the `CACHEDIR.TAG` file.

`--exclude-caches-under`
:   Do not archive the contents of the directory, nor the
    `CACHEDIR.TAG` file, archive only the directory itself.

`--exclude-caches-all`
:   Omit directories containing `CACHEDIR.TAG` file entirely.

Another option family, `--exclude-tag`, provides a generalization of
this concept. It takes a single argument, a file name to look for.
Any directory that contains this file will be excluded from the dump.
Similarly to ‘`exclude-caches`’, there are three options in this
option family:

`--exclude-tag=file`
:   Do not dump the contents of the directory, but dump the
    directory itself and the file.

`--exclude-tag-under=file`
:   Do not dump the contents of the directory, nor the
    file, archive only the directory itself.

`--exclude-tag-all=file`
:   Omit directories containing file file entirely.

Multiple `--exclude-tag*` options can be given.

For example, given this directory:

```
$ find dir
dir
dir/blues
dir/jazz
dir/folk
dir/folk/tagfile
dir/folk/sanjuan
dir/folk/trote
```

The `--exclude-tag` will produce the following:

```
$ tar -cf archive.tar --exclude-tag=tagfile -v dir
dir/
dir/blues
dir/jazz
dir/folk/
tar: dir/folk/: contains a cache directory tag tagfile;
  contents not dumped
dir/folk/tagfile
```

Both the `dir/folk` directory and its tagfile are preserved in
the archive, however the rest of files in this directory are not.

Now, using the `--exclude-tag-under` option will exclude
`tagfile` from the dump, while still preserving the directory
itself, as shown in this example:

```
$ tar -cf archive.tar --exclude-tag-under=tagfile -v dir
dir/
dir/blues
dir/jazz
dir/folk/
./tar: dir/folk/: contains a cache directory tag tagfile;
  contents not dumped
```

Finally, using `--exclude-tag-all` omits the `dir/folk`
directory entirely:

```
$ tar -cf archive.tar --exclude-tag-all=tagfile -v dir
dir/
dir/blues
dir/jazz
./tar: dir/folk/: contains a cache directory tag tagfile;
  directory not dumped
```

* [Problems with Using the `exclude` Options](#problems-with-exclude)

---

Up: [Excluding Some Files](#exclude)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Problems with Using the `exclude` Options

Some users find ‘`exclude`’ options confusing. Here are some common
pitfalls:

* The main operating mode of `tar` does not act on a file name
  explicitly listed on the command line, if one of its file name
  components is excluded. In the example above, if
  you create an archive and exclude files that end with ‘`*.o`’, but
  explicitly name the file ‘`dir.o/foo`’ after all the options have been
  listed, ‘`dir.o/foo`’ will be excluded from the archive.
* You can sometimes confuse the meanings of `--exclude` and
  `--exclude-from`. Be careful: use `--exclude` when files
  to be excluded are given as a pattern on the command line. Use
  `--exclude-from` to introduce the name of a file which contains
  a list of patterns, one per line; each of these patterns can exclude
  zero, one, or many files.
* When you use `--exclude=pattern`, be sure to quote the
  pattern parameter, so GNU `tar` sees wildcard characters
  like ‘`*`’. If you do not do this, the shell might expand the
  ‘`*`’ itself using files at hand, so `tar` might receive a
  list of files instead of one pattern, or none at all, making the
  command somewhat illegal. This might not correspond to what you want.

  For example, write:

  ```
  $ tar -c -f archive.tar --exclude '*.o' directory
  ```

  rather than:

  ```
  # Wrong!
  $ tar -c -f archive.tar --exclude *.o directory
  ```
* You must use use shell syntax, or globbing, rather than `regexp`
  syntax, when using exclude options in `tar`. If you try to use
  `regexp` syntax to describe files to be excluded, your command
  might fail.
* In earlier versions of `tar`, what is now the
  `--exclude-from` option was called `--exclude` instead.
  Now, `--exclude` applies to patterns listed on the command
  line and `--exclude-from` applies to patterns listed in a
  file.

---

Next: [Quoting Member Names](#quoting-styles), Previous: [Excluding Some Files](#exclude), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.5 Wildcards Patterns and Matching

*Globbing* is the operation by which *wildcard* characters,
‘`*`’ or ‘`?`’ for example, are replaced and expanded into all
existing files matching the given pattern. GNU `tar` can use wildcard
patterns for matching (or globbing) archive members when extracting
from or listing an archive. Wildcard patterns are also used for
verifying volume labels of `tar` archives. This section has the
purpose of explaining wildcard syntax for `tar`.

A pattern should be written according to shell syntax, using wildcard
characters to effect globbing. Most characters in the pattern stand
for themselves in the matched string, and case is significant: ‘`a`’
will match only ‘`a`’, and not ‘`A`’. The character ‘`?`’ in the
pattern matches any single character in the matched string. The character
‘`*`’ in the pattern matches zero, one, or more single characters in
the matched string. The character ‘`\`’ says to take the following
character of the pattern *literally*; it is useful when one needs to
match the ‘`?`’, ‘`*`’, ‘`[`’ or ‘`\`’ characters, themselves.

The character ‘`[`’, up to the matching ‘`]`’, introduces a character
class. A *character class* is a list of acceptable characters
for the next single character of the matched string. For example,
‘`[abcde]`’ would match any of the first five letters of the alphabet.
Note that within a character class, all of the “special characters”
listed above other than ‘`\`’ lose their special meaning; for example,
‘`[-\\[*?]]`’ would match any of the characters, ‘`-`’, ‘`\`’,
‘`[`’, ‘`*`’, ‘`?`’, or ‘`]`’. (Due to parsing constraints,
the characters ‘`-`’ and ‘`]`’ must either come *first* or
*last* in a character class.)

If the first character of the class after the opening ‘`[`’
is ‘`!`’ or ‘`^`’, then the meaning of the class is reversed.
Rather than listing character to match, it lists those characters which
are *forbidden* as the next single character of the matched string.

Other characters of the class stand for themselves. The special
construction ‘`[a-e]`’, using an hyphen between two
letters, is meant to represent all characters between a and
e, inclusive.

Periods (‘`.`’) or forward slashes (‘`/`’) are not considered
special for wildcard matches. However, if a pattern completely matches
a directory prefix of a matched string, then it matches the full matched
string: thus, excluding a directory also excludes all the files beneath it.

* [Controlling Pattern-Matching](#controlling-pattern_002dmatching)

---

Up: [Wildcards Patterns and Matching](#wildcards)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### Controlling Pattern-Matching

For the purposes of this section, we call *exclusion members* all
member names obtained while processing `--exclude` and
`--exclude-from` options, and *inclusion members* those
member names that were given in the command line or read from the file
specified with `--files-from` option.

These two pairs of member lists are used in the following operations:
`--diff`, `--extract`, `--list`,
`--update`.

There are no inclusion members in create mode (`--create` and
`--append`), since in this mode the names obtained from the
command line refer to *files*, not archive members.

By default, inclusion members are compared with archive members
literally [20](#FOOT20) and exclusion members are
treated as globbing patterns. For example:

```
$ tar tf foo.tar
a.c
b.c
a.txt
[remarks]
# Member names are used verbatim:
$ tar -xf foo.tar -v '[remarks]'
[remarks]
# Exclude member names are globbed:
$ tar -xf foo.tar -v --exclude '*.c'
a.txt
[remarks]
```

This behavior can be altered by using the following options:

`--wildcards`
:   Treat all member names as wildcards.

`--no-wildcards`
:   Treat all member names as literal strings.

Thus, to extract files whose names end in ‘`.c`’, you can use:

```
$ tar -xf foo.tar -v --wildcards '*.c'
a.c
b.c
```

Notice quoting of the pattern to prevent the shell from interpreting
it.

The effect of `--wildcards` option is canceled by
`--no-wildcards`. This can be used to pass part of
the command line arguments verbatim and other part as globbing
patterns. For example, the following invocation:

```
$ tar -xf foo.tar --wildcards '*.txt' --no-wildcards '[remarks]'
```

instructs `tar` to extract from `foo.tar` all files whose
names end in ‘`.txt`’ and the file named `[remarks]`.

Normally, a pattern matches a name if an initial subsequence of the
name’s components matches the pattern, where ‘`*`’, ‘`?`’, and
‘`[...]`’ are the usual shell wildcards, ‘`\`’ escapes wildcards,
and wildcards can match ‘`/`’.

Other than optionally stripping leading ‘`/`’ from names
(see [Absolute File Names](#absolute)), patterns and names are used as-is. For
example, trailing ‘`/`’ is not trimmed from a user-specified name
before deciding whether to exclude it.

However, this matching procedure can be altered by the options listed
below. These options accumulate. For example:

```
--ignore-case --exclude='makefile' --no-ignore-case ---exclude='readme'
```

ignores case when excluding ‘`makefile`’, but not when excluding
‘`readme`’.

`--anchored`

`--no-anchored`
:   If anchored, a pattern must match an initial subsequence
    of the name’s components. Otherwise, the pattern can match any
    subsequence. Default is `--no-anchored` for exclusion members
    and `--anchored` inclusion members.

`--ignore-case`

`--no-ignore-case`
:   When ignoring case, upper-case patterns match lower-case names and vice versa.
    When not ignoring case (the default), matching is case-sensitive.

`--wildcards-match-slash`

`--no-wildcards-match-slash`
:   When wildcards match slash (the default for exclusion members), a
    wildcard like ‘`*`’ in the pattern can match a ‘`/`’ in the
    name. Otherwise, ‘`/`’ is matched only by ‘`/`’.

The `--recursion` and `--no-recursion` options
(see [Descending into Directories](#recurse)) also affect how member patterns are interpreted. If
recursion is in effect, a pattern matches a name if it matches any of
the name’s parent directories.

The following table summarizes pattern-matching default values:

| Members | Default settings |
| --- | --- |
| Inclusion | `--no-wildcards --anchored --no-wildcards-match-slash` |
| Exclusion | `--wildcards --no-anchored --wildcards-match-slash` |

---

Next: [Modifying File and Member Names](#transform), Previous: [Wildcards Patterns and Matching](#wildcards), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.6 Quoting Member Names

When displaying member names, `tar` takes care to avoid
ambiguities caused by certain characters. This is called *name
quoting*. The characters in question are:

* Non-printable control characters:

  | Character | ASCII | Character name |
  | --- | --- | --- |
  | \a | 7 | Audible bell |
  | \b | 8 | Backspace |
  | \f | 12 | Form feed |
  | \n | 10 | New line |
  | \r | 13 | Carriage return |
  | \t | 9 | Horizontal tabulation |
  | \v | 11 | Vertical tabulation |
* Space (ASCII 32)
* Single and double quotes (‘`'`’ and ‘`"`’)
* Backslash (‘`\`’)

The exact way `tar` uses to quote these characters depends on
the *quoting style*. The default quoting style, called
*escape* (see below), uses backslash notation to represent control
characters and backslash.

GNU `tar` offers seven distinct quoting styles, which can be selected
using `--quoting-style` option:

`--quoting-style=style` [¶](#index-quoting_002dstyle)
:   Sets quoting style. Valid values for style argument are:
    literal, shell, shell-always, c, escape, locale, clocale.

These styles are described in detail below. To illustrate their
effect, we will use an imaginary tar archive `arch.tar`
containing the following members:

```
# 1. Contains horizontal tabulation character.
a       tab
# 2. Contains newline character
a
newline
# 3. Contains a space
a space
# 4. Contains double quotes
a"double"quote
# 5. Contains single quotes
a'single'quote
# 6. Contains a backslash character:
a\backslash
```

Here is how usual `ls` command would have listed them, if they
had existed in the working directory:

```
$ ls
a\ttab
a\nnewline
a\ space
a"double"quote
a'single'quote
a\\backslash
```

Quoting styles:

‘`literal`’
:   No quoting, display each character as is:

    ```
    $ tar tf arch.tar --quoting-style=literal
    ./
    ./a space
    ./a'single'quote
    ./a"double"quote
    ./a\backslash
    ./a     tab
    ./a
    newline
    ```

‘`shell`’
:   Display characters the same way Bourne shell does:
    control characters, except ‘`\t`’ and ‘`\n`’, are printed using
    backslash escapes, ‘`\t`’ and ‘`\n`’ are printed as is, and a
    single quote is printed as ‘`\'`’. If a name contains any quoted
    characters, it is enclosed in single quotes. In particular, if a name
    contains single quotes, it is printed as several single-quoted strings:

    ```
    $ tar tf arch.tar --quoting-style=shell
    ./
    './a space'
    './a'\''single'\''quote'
    './a"double"quote'
    './a\backslash'
    './a    tab'
    './a
    newline'
    ```

‘`shell-always`’
:   Same as ‘`shell`’, but the names are always enclosed in single
    quotes:

    ```
    $ tar tf arch.tar --quoting-style=shell-always
    './'
    './a space'
    './a'\''single'\''quote'
    './a"double"quote'
    './a\backslash'
    './a    tab'
    './a
    newline'
    ```

‘`c`’
:   Use the notation of the C programming language. All names are
    enclosed in double quotes. Control characters are quoted using
    backslash notations, double quotes are represented as ‘`\"`’,
    backslash characters are represented as ‘`\\`’. Single quotes and
    spaces are not quoted:

    ```
    $ tar tf arch.tar --quoting-style=c
    "./"
    "./a space"
    "./a'single'quote"
    "./a\"double\"quote"
    "./a\\backslash"
    "./a\ttab"
    "./a\nnewline"
    ```

‘`escape`’
:   Control characters are printed using backslash notation, and a
    backslash as ‘`\\`’. This is the default quoting style, unless it
    was changed when configured the package.

    ```
    $ tar tf arch.tar --quoting-style=escape
    ./
    ./a space
    ./a'single'quote
    ./a"double"quote
    ./a\\backslash
    ./a\ttab
    ./a\nnewline
    ```

‘`locale`’
:   Control characters, single quote and backslash are printed using
    backslash notation. All names are quoted using left and right
    quotation marks, appropriate to the current locale. If it does not
    define quotation marks, use ‘`'`’ as left and as right
    quotation marks. Any occurrences of the right quotation mark in a
    name are escaped with ‘`\`’, for example:

    For example:

    ```
    $ tar tf arch.tar --quoting-style=locale
    './'
    './a space'
    './a\'single\'quote'
    './a"double"quote'
    './a\\backslash'
    './a\ttab'
    './a\nnewline'
    ```

‘`clocale`’
:   Same as ‘`locale`’, but ‘`"`’ is used for both left and right
    quotation marks, if not provided by the currently selected locale:

    ```
    $ tar tf arch.tar --quoting-style=clocale
    "./"
    "./a space"
    "./a'single'quote"
    "./a\"double\"quote"
    "./a\\backslash"
    "./a\ttab"
    "./a\nnewline"
    ```

You can specify which characters should be quoted in addition to those
implied by the current quoting style:

`--quote-chars=string`
:   Always quote characters from string, even if the selected
    quoting style would not quote them.

For example, using ‘`escape`’ quoting (compare with the usual
escape listing above):

```
$ tar tf arch.tar --quoting-style=escape --quote-chars=' "'
./
./a\ space
./a'single'quote
./a\"double\"quote
./a\\backslash
./a\ttab
./a\nnewline
```

To disable quoting of such additional characters, use the following
option:

`--no-quote-chars=string`
:   Remove characters listed in string from the list of quoted
    characters set by the previous `--quote-chars` option.

This option is particularly useful if you have added
`--quote-chars` to your `TAR_OPTIONS` (see [TAR\_OPTIONS](#TAR_005fOPTIONS))
and wish to disable it for the current invocation.

Note, that `--no-quote-chars` does *not* disable those
characters that are quoted by default in the selected quoting style.

---

Next: [Operating Only on New Files](#after), Previous: [Quoting Member Names](#quoting-styles), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.7 Modifying File and Member Names

`Tar` archives contain detailed information about files stored
in them and full file names are part of that information. When
storing a file to an archive, its file name is recorded in it,
along with the actual file contents. When restoring from an archive,
a file is created on disk with exactly the same name as that stored
in the archive. In the majority of cases this is the desired behavior
of a file archiver. However, there are some cases when it is not.

First of all, it is often unsafe to extract archive members with
absolute file names or those that begin with a `../`. GNU `tar`
takes special precautions when extracting such names and provides a
special option for handling them, which is described in
[Absolute File Names](#absolute).

Secondly, you may wish to extract file names without some leading
directory components, or with otherwise modified names. In other
cases it is desirable to store files under differing names in the
archive.

GNU `tar` provides several options for these needs.

`--strip-components=number`
:   Strip given number of leading components from file names before
    extraction.

For example, suppose you have archived whole `/usr` hierarchy to
a tar archive named `usr.tar`. Among other files, this archive
contains `usr/include/stdlib.h`, which you wish to extract to
the working directory. To do so, you type:

```
$ tar -xf usr.tar --strip=2 usr/include/stdlib.h
```

The option `--strip=2` instructs `tar` to strip the
two leading components (`usr/` and `include/`) off the file
name, before extracting it. Notice, that archive members to extract are
searched before that modification, hence the file name is specified in
full.

If you add the `--verbose` (`-v`) option to the invocation
above, you will note that the verbose listing still contains the
full file name, with the two removed components still in place. This
can be inconvenient, so `tar` provides a special option for
altering this behavior:

`--show-transformed-names`
:   Display file or member names with all requested transformations
    applied.

For example:

```
$ tar -xf usr.tar -v --strip=2 usr/include/stdlib.h
usr/include/stdlib.h
$ tar -xf usr.tar -v --strip=2 --show-transformed usr/include/stdlib.h
stdlib.h
```

Notice that in both cases the file `stdlib.h` is extracted to the
working directory, `--show-transformed-names` affects
only the way its name is displayed.

This option is especially useful for verifying whether the invocation
will have the desired effect. Thus, before running

```
$ tar -x --strip=n
```

it is often advisable to run

```
$ tar -t -v --show-transformed --strip=n
```

to make sure the command will produce the intended results.

In case you need to apply more complex modifications to the file name,
GNU `tar` provides a general-purpose transformation option:

`--transform=expression`

`--xform=expression`
:   Modify file names using supplied expression.

The expression is a `sed`-like replace expression of the
form:

```
s/regexp/replace/[flags]
```

where regexp is a *regular expression*, replace is a
replacement for each file name part that matches regexp. Both
regexp and replace are described in detail in
[The ‘s’ Command](https://www.gnu.org/software/sed/manual/sed.html#The-_0022s_0022-Command) in GNU sed.

Any delimiter can be used in lieu of ‘`/`’, the only requirement being
that it be used consistently throughout the expression. For example,
the following two expressions are equivalent:

```
s/one/two/
s,one,two,
```

Changing delimiters is often useful when the regex contains
slashes. For example, it is more convenient to write `s,/,-,` than
`s/\//-/`.

As in `sed`, you can give several replace expressions,
separated by a semicolon.

Supported flags are:

‘`g`’
:   Apply the replacement to *all* matches to the regexp, not
    just the first.

‘`i`’
:   Use case-insensitive matching.

‘`x`’
:   regexp is an *extended regular expression* (see [Extended regular expressions](https://www.gnu.org/software/sed/manual/sed.html#Extended-regexps) in GNU sed).

‘`number`’
:   Only replace the numberth match of the regexp.

    Note: the POSIX standard does not specify what should happen
    when you mix the ‘`g`’ and number modifiers. GNU `tar`
    follows the GNU `sed` implementation in this regard, so
    the interaction is defined to be: ignore matches before the
    numberth, and then match and replace all matches from the
    numberth on.

In addition, several *transformation scope* flags are supported,
that control to what files transformations apply. These are:

‘`r`’
:   Apply transformation to regular archive members.

‘`R`’
:   Do not apply transformation to regular archive members.

‘`s`’
:   Apply transformation to symbolic link targets.

‘`S`’
:   Do not apply transformation to symbolic link targets.

‘`h`’
:   Apply transformation to hard link targets.

‘`H`’
:   Do not apply transformation to hard link targets.

Default is ‘`rsh`’, which means to apply transformations to both archive
members and targets of symbolic and hard links.

Default scope flags can also be changed using ‘`flags=`’ statement
in the transform expression. The flags set this way remain in force
until next ‘`flags=`’ statement or end of expression, whichever
occurs first. For example:

```
  --transform 'flags=S;s|^|/usr/local/|'
```

Here are several examples of `--transform` usage:

1. Extract `usr/` hierarchy into `usr/local/`:

   ```
   $ tar --transform='s,usr/,usr/local/,' -x -f arch.tar
   ```
2. Strip two leading directory components (equivalent to
   `--strip-components=2`):

   ```
   $ tar --transform='s,/*[^/]*/[^/]*/,,' -x -f arch.tar
   ```
3. Convert each file name to lower case:

   ```
   $ tar --transform 's/.*/\L&/' -x -f arch.tar
   ```
4. Prepend `/prefix/` to each file name:

   ```
   $ tar --transform 's,^,/prefix/,' -x -f arch.tar
   ```
5. Archive the `/lib` directory, prepending ‘`/usr/local`’
   to each archive member:

   ```
   $ tar --transform 's,^,/usr/local/,S' -c -f arch.tar /lib
   ```

Notice the use of flags in the last example. The `/lib`
directory often contains many symbolic links to files within it.
It may look, for example, like this:

```
$ ls -l
drwxr-xr-x root/root       0 2008-07-08 16:20 /lib/
-rwxr-xr-x root/root 1250840 2008-05-25 07:44 /lib/libc-2.3.2.so
lrwxrwxrwx root/root       0 2008-06-24 17:12 /lib/libc.so.6 -> libc-2.3.2.so
...
```

Using the expression ‘`s,^,/usr/local/,`’ would mean adding
‘`/usr/local`’ to both regular archive members and to link
targets. In this case, `/lib/libc.so.6` would become:

```
  /usr/local/lib/libc.so.6 -> /usr/local/libc-2.3.2.so
```

This is definitely not desired. To avoid this, the ‘`S`’ flag
is used, which excludes symbolic link targets from filename
transformations. The result is:

```
$ tar --transform 's,^,/usr/local/,S' -c -v -f arch.tar \
       --show-transformed /lib
drwxr-xr-x root/root       0 2008-07-08 16:20 /usr/local/lib/
-rwxr-xr-x root/root 1250840 2008-05-25 07:44 /usr/local/lib/libc-2.3.2.so
lrwxrwxrwx root/root       0 2008-06-24 17:12 /usr/local/lib/libc.so.6 \
 -> libc-2.3.2.so
```

Unlike `--strip-components`, `--transform` can be used
in any GNU `tar` operation mode. For example, the following command
adds files to the archive while replacing the leading `usr/`
component with `var/`:

```
$ tar -cf arch.tar --transform='s,^usr/,var/,' /
```

To test `--transform` effect we suggest using
`--show-transformed-names` option:

```
$ tar -cf arch.tar --transform='s,^usr/,var/,' \
       --verbose --show-transformed-names /
```

If both `--strip-components` and `--transform` are used
together, then `--transform` is applied first, and the required
number of components is then stripped from its result.

You can use as many `--transform` options in a single command
line as you want. The specified expressions will then be applied in
order of their appearance. For example, the following two invocations
are equivalent:

```
$ tar -cf arch.tar --transform='s,/usr/var,/var/,' \
                        --transform='s,/usr/local,/usr/,'
$ tar -cf arch.tar \
               --transform='s,/usr/var,/var/,;s,/usr/local,/usr/,'
```

Applying transformations to some file names may produce empty
strings. This may indicate errors in regular expressions, but it may
as well happen during normal operation, for instance, when using
`--strip-components` option with a value which is greater then
or equal to the number of directory components in the topmost
directory stored in the archive. Consider the following example. Let
the archive `usr.tar` contain:

```
$ tar tf usr.tar
usr/
usr/local/
...
```

Extracting from this archive with the `--strip=1` option will
transform the name of its first entry (‘`usr/`’) to an empty
string. When this happens, GNU `tar` prints a warning and skips this
member:

```
$ tar --strip=1 -xf usr.tar
tar: usr: transforms to empty name
```

If an empty name results from transformations applied when creating or
updating the archive, the warning is output and the file is not
archived.

If this is intended, you can suppress the warning using the
`--warning=no-empty-transform` option (see [Controlling Warning Messages](#warnings)).

---

Next: [Descending into Directories](#recurse), Previous: [Modifying File and Member Names](#transform), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.8 Operating Only on New Files

The `--after-date=date` (`--newer=date`,
`-N date`) option causes `tar` to only work on
files whose data modification or status change times are newer than
the date given. If date starts with ‘`/`’ or ‘`.`’,
it is taken to be a file name; the data modification time of that file
is used as the date. If you use this option when creating or appending
to an archive, the archive will only include new files. If you use
`--after-date` when extracting an archive, `tar` will
only extract files newer than the date you specify.

If you want `tar` to make the date comparison based only on
modification of the file’s data (rather than status
changes), then use the `--newer-mtime=date` option.

You may use these options with any operation. Note that these options
differ from the `--update` (`-u`) operation in that they
allow you to specify a particular date against which `tar` can
compare when deciding whether or not to archive the files.

`--after-date=date`

`--newer=date`

`-N date`
:   Only store files newer than date.

    Acts on files only if their data modification or status change times are
    later than date. Use in conjunction with any operation.

    If date starts with ‘`/`’ or ‘`.`’, it is taken to be a file
    name; the data modification time of that file is used as the date.

`--newer-mtime=date`
:   Act like `--after-date`, but look only at data modification times.

These options limit `tar` to operate only on files which have
been modified after the date specified. A file’s status is considered to have
changed if its contents have been modified, or if its owner,
permissions, and so forth, have been changed. (For more information on
how to specify a date, see [Date input formats](#Date-input-formats); remember that the
entire date argument must be quoted if it contains any spaces.)

Gurus would say that `--after-date` tests both the data
modification time (`mtime`, the time the contents of the file
were last modified) and the status change time (`ctime`, the time
the file’s status was last changed: owner, permissions, etc.)
fields, while `--newer-mtime` tests only the `mtime`
field.

To be precise, `--after-date` checks *both* `mtime` and
`ctime` and processes the file if either one is more recent than
date, while `--newer-mtime` checks only `mtime` and
disregards `ctime`. Neither option uses `atime` (the last time the
contents of the file were looked at).

Date specifiers can have embedded spaces. Because of this, you may need
to quote date arguments to keep the shell from parsing them as separate
arguments. For example, the following command will add to the archive
all the files modified less than two days ago:

```
$ tar -cf foo.tar --newer-mtime '2 days ago'
```

When any of these options is used with the option `--verbose`
(see [The `--verbose` Option](#verbose-tutorial)) GNU `tar` converts the specified
date back to a textual form and compares that with the
one given with the option. If the two forms differ, `tar`
prints both forms in a message, to help the user check that the right
date is being used. For example:

```
$ tar -c -f archive.tar --after-date='10 days ago' .
tar: Option --after-date: Treating date '10 days ago' as 2006-06-11
13:19:37.232434
```

> **Please Note:** `--after-date` and `--newer-mtime`
> should not be used for incremental backups. See [Using `tar` to Perform Incremental Dumps](#Incremental-Dumps),
> for proper way of creating incremental backups.

---

Next: [Crossing File System Boundaries](#one), Previous: [Operating Only on New Files](#after), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.9 Descending into Directories

Usually, `tar` will recursively explore all directories (either
those given on the command line or through the `--files-from`
option) for the various files they contain. However, you may not always
want `tar` to act this way.

The `--no-recursion` option inhibits `tar`’s recursive descent
into specified directories. If you specify `--no-recursion`, you can
use the `find` (see [find](find.html#Top) in GNU Find Manual)
utility for hunting through levels of directories to
construct a list of file names which you could then pass to `tar`.
`find` allows you to be more selective when choosing which files to
archive; see [Reading Names from a File](#files), for more information on using `find` with
`tar`.

`--no-recursion`
:   Prevents `tar` from recursively descending directories.

`--recursion`
:   Requires `tar` to recursively descend directories.
    This is the default.

When you use `--no-recursion`, GNU `tar` grabs
directory entries themselves, but does not descend on them
recursively. Many people use `find` for locating files they
want to back up, and since `tar` *usually* recursively
descends on directories, they have to use the ‘`-not -type d`’
test in their `find` invocation (see [Type test](find.html#Type) in Finding Files), as they usually do not want all the files in a
directory. They then use the `--files-from` option to archive
the files located via `find`.

The problem when restoring files archived in this manner is that the
directories themselves are not in the archive; so the
`--same-permissions` (`--preserve-permissions`,
`-p`) option does not affect them—while users might really
like it to. Specifying `--no-recursion` is a way to tell
`tar` to grab only the directory entries given to it, adding
no new files on its own. To summarize, if you use `find` to
create a list of files to be stored in an archive, use it as follows:

```
$ find dir tests | \
  tar -cf archive --no-recursion -T -
```

The `--no-recursion` option also applies when extracting: it
causes `tar` to extract only the matched directory entries, not
the files under those directories.

The `--no-recursion` option also affects how globbing patterns
are interpreted (see [Controlling Pattern-Matching](#controlling-pattern_002dmatching)).

The `--no-recursion` and `--recursion` options apply to
later options and operands, and can be overridden by later occurrences
of `--no-recursion` and `--recursion`. For example:

```
$ tar -cf jams.tar --no-recursion grape --recursion grape/concord
```

creates an archive with one entry for `grape`, and the recursive
contents of `grape/concord`, but no entries under `grape`
other than `grape/concord`.

---

Previous: [Descending into Directories](#recurse), Up: [Choosing Files and Names for `tar`](#Choosing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 6.10 Crossing File System Boundaries

`tar` will normally automatically cross file system boundaries in
order to archive files which are part of a directory tree. You can
change this behavior by running `tar` and specifying
`--one-file-system`. This option only affects files that are
archived because they are in a directory that is being archived;
`tar` will still archive files explicitly named on the command line
or through `--files-from`, regardless of where they reside.

`--one-file-system`
:   Prevents `tar` from crossing file system boundaries when
    archiving. Use in conjunction with any write operation.

The `--one-file-system` option causes `tar` to modify its
normal behavior in archiving the contents of directories. If a file in
a directory is not on the same file system as the directory itself, then
`tar` will not archive that file. If the file is a directory
itself, `tar` will not archive anything beneath it; in other words,
`tar` will not cross mount points.

This option is useful for making full or incremental archival backups of
a file system. If this option is used in conjunction with
`--verbose` (`-v`), files that are excluded are
mentioned by name on the standard error.

* [Changing the Working Directory](#directory)
* [Absolute File Names](#absolute)

---

Next: [Absolute File Names](#absolute), Up: [Crossing File System Boundaries](#one)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 6.10.1 Changing the Working Directory

To change the working directory in the middle of a list of file names,
either on the command line or in a file specified using
`--files-from` (`-T`), use `--directory` (`-C`).
This will change the working directory to the specified directory
after that point in the list.

`--directory=directory`

`-C directory`
:   Changes the working directory in the middle of a command line.

All filename arguments appearing after this option will be taken
relative to directory. For example,

```
$ tar -c -f jams.tar grape prune -C food cherry
```

will place the files `grape` and `prune` from the working
directory into the archive `jams.tar`, followed by the file
`cherry` from the directory `food`. This option is especially
useful when you have several widely separated files that you want to
store in the same archive.

Note that the file `cherry` is recorded in the archive under the
precise name `cherry`, *not* `food/cherry`. Thus, the
archive will contain three files that all appear to have come from the
same directory; if the archive is extracted with plain ‘`tar
--extract`’, all three files will be written in the working directory.

Contrast this with the command,

```
$ tar -c -f jams.tar grape prune -C food red/cherry
```

which records the third file in the archive under the name
`red/cherry` so that, if the archive is extracted using
‘`tar --extract`’, the third file will be written in a subdirectory
named `red`.

You can use the `--directory` option to make the archive
independent of the original name of the directory holding the files.
The following command places the files `/etc/passwd`,
`/etc/hosts`, and `/lib/libc.a` into the archive
`foo.tar`:

```
$ tar -c -f foo.tar -C /etc passwd hosts -C /lib libc.a
```

However, the names of the archive members will be exactly what they were
on the command line: `passwd`, `hosts`, and `libc.a`.
They will not appear to be related by file name to the original
directories where those files were located.

Note that `--directory` options are interpreted consecutively. If
`--directory` specifies a relative file name, it is interpreted
relative to the then working directory, which might not be the same as
the original working directory of `tar`, due to a previous
`--directory` option.

There is an important exception, though: the `-C` option does
not affect filename arguments to another options that follow it, in
particular `-f`. This means that the example above can be
written as

```
$ tar -c -C /etc -f foo.tar passwd hosts -C /lib libc.a
```

and it will not change its overall effect: the files
`passwd` and `hosts` will be read from the directory
`/etc`, but the archive `foo.tar` will be created in the
directory which was the working directory when the `tar`
command was started.

The command-line options not affected by `-C` are:

* `-g`, `--listed-incremental`
* `--exclude-ignore`
* `--exclude-ignore-recursive`
* `--exclude-tag`
* `--exclude-tag-all`
* `--exclude-tag-under`
* `-T`, `--files-from`
* `-X`, `--exclude-from`
* `--mtime`
* `--owner-map`
* `-f`, `--file`
* `-F`, `--info-script`, `--new-volume-script`
* `--volno-file`
* `-N`, `--newer`
* `--index-file`

When using `--files-from` (see [Reading Names from a File](#files)), you can put certain
`tar` options (including `-C`) in the file list as well.
For instance, the file list for the above example will be:

```
-C /etc
passwd
hosts
--directory=/lib
libc.a
```

To use it, you would invoke `tar` as follows:

```
$ tar -c -f foo.tar --files-from list
```

For a full list of options that can be used in file lists,
[Position-Sensitive Options](#Position_002dSensitive-Options).

The interpretation of options in file lists is disabled by
`--verbatim-files-from` and `--null` options.

---

Previous: [Changing the Working Directory](#directory), Up: [Crossing File System Boundaries](#one)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 6.10.2 Absolute File Names

By default, GNU `tar` drops a leading ‘`/`’ on
input or output, and complains about file names containing a ‘`..`’
component. There is an option that turns off this behavior:

`--absolute-names`

`-P`
:   Do not strip leading slashes from file names, and permit file names
    containing a ‘`..`’ file name component, or that escape
    the extraction directory.

When `tar` extracts archive members from an archive, it strips any
leading slashes (‘`/`’) from the member name. This causes absolute
member names in the archive to be treated as relative file names. This
allows you to have such members extracted wherever you want, instead of
being restricted to extracting the member in the exact directory named
in the archive. For example, if the archive member has the name
`/etc/passwd`, `tar` will extract it as if the name were
really `etc/passwd`.

File names containing ‘`..`’ can cause problems when extracting, so
`tar` normally warns you about such files when creating an
archive, and prevents attempts to extract such files if that would
affect files outside the working directory.

Other `tar` programs do not do this. As a result, if you
create an archive whose member names start with a slash, they will be
difficult for other people with a non-GNU `tar`
program to use. Therefore, GNU `tar` also strips
leading slashes from member names when putting members into the
archive. For example, if you ask `tar` to add the file
`/bin/ls` to an archive, it will do so, but the member name will
be `bin/ls`[21](#FOOT21).

If you use the `--absolute-names` (`-P`) option,
`tar` will do none of these transformations.

By default `tar` prints out a message about removing the ‘`/`’ from
file names. This message appears once per GNU `tar`
invocation. It represents something which ought to be told; ignoring
what it means can cause very serious surprises, later.
However, to suppress this message change to
the `/` directory first, and then avoid absolute notation.
For example:

```
$ tar -c -f archive.tar -C / home
```

If you use the dangerous options `--absolute-names`
(`-P`) or `--dereference` (`-h`),
symbolic links containing ‘`..`’ or leading ‘`/`’ can cause
problems when extracting, so `tar` extracts them last;
it may create empty files as placeholders during extraction.
Although these placeholders prevent problems if you are extracting
into an empty directory, they do not suffice for nonempty directories.
See [Integrity](#Integrity), for some of the security-related implications
of using these dangerous options.

---

Next: [Controlling the Archive Format](#Formats), Previous: [Choosing Files and Names for `tar`](#Choosing), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## 7 Date input formats

First, a quote:

> Our units of temporal measurement, from seconds on up to months, are so
> complicated, asymmetrical and disjunctive so as to make coherent mental
> reckoning in time all but impossible. Indeed, had some tyrannical god
> contrived to enslave our minds to time, to make it all but impossible
> for us to escape subjection to sodden routines and unpleasant surprises,
> he could hardly have done better than handing down our present system.
> It is like a set of trapezoidal building blocks, with no vertical or
> horizontal surfaces, like a language in which the simplest thought
> demands ornate constructions, useless particles and lengthy
> circumlocutions. Unlike the more successful patterns of language and
> science, which enable us to face experience boldly or at least
> level-headedly, our system of temporal calculation silently and
> persistently encourages our terror of time.
>
> … It is as though architects had to measure length in feet, width
> in meters and height in ells; as though basic instruction manuals
> demanded a knowledge of five different languages. It is no wonder then
> that we often look into our own immediate past or future, last Tuesday
> or a week from Sunday, with feelings of helpless confusion. …
>
> —Robert Grudin, Time and the Art of Living.

This section describes the textual date representations that GNU
programs accept. These are the strings you, as a user, can supply as
arguments to the various programs. The C interface (via the
`parse_datetime` function) is not described here.

* [General date syntax](#General-date-syntax)
* [Calendar date items](#Calendar-date-items)
* [Time of day items](#Time-of-day-items)
* [Time zone items](#Time-zone-items)
* [Combined date and time of day items](#Combined-date-and-time-of-day-items)
* [Day of week items](#Day-of-week-items)
* [Relative items in date strings](#Relative-items-in-date-strings)
* [Pure numbers in date strings](#Pure-numbers-in-date-strings)
* [Seconds since the Epoch](#Seconds-since-the-Epoch)
* [Specifying time zone rules](#Specifying-time-zone-rules)
* [Authors of `parse_datetime`](#Authors-of-parse_005fdatetime)

---

Next: [Calendar date items](#Calendar-date-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.1 General date syntax

A *date* is a string, possibly empty, containing many items
separated by whitespace. The whitespace may be omitted when no
ambiguity arises. The empty string means the beginning of today (i.e.,
midnight). Order of the items is immaterial. A date string may contain
many flavors of items:

* calendar date items
* time of day items
* time zone items
* combined date and time of day items
* day of the week items
* relative items
* pure numbers.

We describe each of these item types in turn, below.

A few ordinal numbers may be written out in words in some contexts. This is
most useful for specifying day of the week items or relative items (see
below). Among the most commonly used ordinal numbers, the word
‘`last`’ stands for *-1*, ‘`this`’ stands for 0, and
‘`first`’ and ‘`next`’ both stand for 1. Because the word
‘`second`’ stands for the unit of time there is no way to write the
ordinal number 2, but for convenience ‘`third`’ stands for 3,
‘`fourth`’ for 4, ‘`fifth`’ for 5,
‘`sixth`’ for 6, ‘`seventh`’ for 7, ‘`eighth`’ for 8,
‘`ninth`’ for 9, ‘`tenth`’ for 10, ‘`eleventh`’ for 11 and
‘`twelfth`’ for 12.

When a month is written this way, it is still considered to be written
numerically, instead of being “spelled in full”; this changes the
allowed strings.

In the current implementation, only English is supported for words and
abbreviations like ‘`AM`’, ‘`DST`’, ‘`EST`’, ‘`first`’,
‘`January`’, ‘`Sunday`’, ‘`tomorrow`’, and ‘`year`’.

In the current implementation, only the Gregorian calendar is supported,
regardless of the locale.

The output of the `date` command
is not always acceptable as a date string,
not only because of the language problem and of the calendar problem,
but also because there is no
standard meaning for time zone items like ‘`IST`’. When using
`date` to generate a date string intended to be parsed later,
specify a date format that is independent of language,
that uses the Gregorian calendar,
and that does not use time zone items other than ‘`UTC`’ and ‘`Z`’.
Here are some ways to do this:

```
$ LC_ALL=C TZ=UTC0 date
Tue Nov 15 02:02:42 UTC 2022
$ LC_ALL=C TZ=UTC0 date +'%Y-%m-%d %H:%M:%SZ'
2022-11-15 02:02:42Z
$ date --rfc-3339=ns  # --rfc-3339 is a GNU extension.
2022-11-14 21:02:42.000000000-05:00
$ date --rfc-email  # a GNU extension
Mon, 14 Nov 2022 21:02:42 -0500
$ LC_ALL=C date +'%Y-%m-%d %H:%M:%S %z'  # %z is a GNU extension.
2022-11-14 21:02:42 -0500
$ date +'@%s.%N'  # %s and %N are GNU extensions.
@1668477762.692722128
```

Alphabetic case is completely ignored in dates. Comments may be introduced
between round parentheses, as long as included parentheses are properly
nested. Hyphens not followed by a digit are currently ignored. Leading
zeros on numbers are ignored.

Invalid dates like ‘`2022-02-29`’ or times like ‘`24:00`’ are
rejected. In the typical case of a host that does not support leap
seconds, a time like ‘`23:59:60`’ is rejected even if it
corresponds to a valid leap second.

---

Next: [Time of day items](#Time-of-day-items), Previous: [General date syntax](#General-date-syntax), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.2 Calendar date items

A *calendar date item* specifies a day of the year. It is
specified differently, depending on whether the month is specified
numerically or literally. All these strings specify the same calendar date:

```
2022-11-14     # ISO 8601.
22-11-14       # Assume 19xx for 69 through 99,
               # 20xx for 00 through 68 (not recommended).
11/14/2022     # Common U.S. writing.
14.11.2022     # Common European writing.
14 November 2022
14 Nov 2022    # Three-letter abbreviations always allowed.
November 14, 2022
14-nov-2022
14nov2022
```

The year can also be omitted. In this case, the last specified year is
used, or the current year if none. For example:

```
11/14
14.11.
nov 14
```

Here are the rules.

For numeric months, the ISO 8601 format
‘`year-month-day`’ is allowed, where year is
any positive number, month is a number between 01 and 12, and
day is a number between 01 and 31. A leading zero must be present
if a number is less than ten. If year is 68 or smaller, then 2000
is added to it; otherwise, if year is less than 100,
then 1900 is added to it. The construct
‘`month/day/year`’, popular in the United States,
is accepted. Also ‘`month/day`’, omitting the year.
Similarly, the construct ‘`day.month.year`’, popular in
Europe, and its short form ‘`day.month.`’ are accepted.

Literal months may be spelled out in full: ‘`January`’,
‘`February`’, ‘`March`’, ‘`April`’, ‘`May`’, ‘`June`’,
‘`July`’, ‘`August`’, ‘`September`’, ‘`October`’,
‘`November`’ or ‘`December`’. Literal months may be abbreviated
to their first three letters, possibly followed by an abbreviating dot.
It is also permitted to write ‘`Sept`’ instead of ‘`September`’.

When months are written literally, the calendar date may be given as any
of the following:

```
day month year
day month
month day year
day-month-year
```

Or, omitting the year:

```
month day
```

---

Next: [Time zone items](#Time-zone-items), Previous: [Calendar date items](#Calendar-date-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.3 Time of day items

A *time of day item* in date strings specifies the time on a given
day. Here are some examples, all of which represent the same time:

```
20:02:00.000000
20:02
8:02pm
20:02-0500      # In EST (U.S. Eastern Standard Time).
```

More generally, the time of day may be given as
‘`hour:minute:second`’, where hour is
a number between 0 and 23, minute is a number between 0 and
59, and second is a number between 0 and 59 possibly followed by
‘`.`’ or ‘`,`’ and a fraction containing one or more digits.
Alternatively,
‘`:second`’ can be omitted, in which case it is taken to
be zero. On the rare hosts that support leap seconds, second
may be 60.

If the time is followed by ‘`am`’ or ‘`pm`’ (or ‘`a.m.`’
or ‘`p.m.`’), hour is restricted to run from 1 to 12, and
‘`:minute`’ may be omitted (taken to be zero). ‘`am`’
indicates the first half of the day, ‘`pm`’ indicates the second
half of the day. In this notation, 12 is the predecessor of 1:
midnight is ‘`12am`’ while noon is ‘`12pm`’.
(This is the zero-oriented interpretation of ‘`12am`’ and ‘`12pm`’,
as opposed to the old tradition derived from Latin
which uses ‘`12m`’ for noon and ‘`12pm`’ for midnight.)

The time may alternatively be followed by a time zone correction,
expressed as ‘`shhmm`’, where s is ‘`+`’
or ‘`-`’, hh is a number of zone hours and mm is a number
of zone minutes.
The zone minutes term, mm, may be omitted, in which case
the one- or two-digit correction is interpreted as a number of hours.
You can also separate hh from mm with a colon.
When a time zone correction is given this way, it
forces interpretation of the time relative to
Coordinated Universal Time (UTC), overriding any previous
specification for the time zone or the local time zone. For example,
‘`+0530`’ and ‘`+05:30`’ both stand for the time zone 5.5 hours
ahead of UTC (e.g., India).
This is the best way to
specify a time zone correction by fractional parts of an hour.
The maximum zone correction is 24 hours.

Either ‘`am`’/‘`pm`’ or a time zone correction may be specified,
but not both.

---

Next: [Combined date and time of day items](#Combined-date-and-time-of-day-items), Previous: [Time of day items](#Time-of-day-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.4 Time zone items

A *time zone item* specifies an international time zone, indicated
by a small set of letters, e.g., ‘`UTC`’ or ‘`Z`’
for Coordinated Universal
Time. Any included periods are ignored. By following a
non-daylight-saving time zone by the string ‘`DST`’ in a separate
word (that is, separated by some white space), the corresponding
daylight saving time zone may be specified.
Alternatively, a non-daylight-saving time zone can be followed by a
time zone correction, to add the two values. This is normally done
only for ‘`UTC`’; for example, ‘`UTC+05:30`’ is equivalent to
‘`+05:30`’.

Time zone items other than ‘`UTC`’ and ‘`Z`’
are obsolescent and are not recommended, because they
are ambiguous; for example, ‘`EST`’ has a different meaning in
Australia than in the United States, and ‘`A`’ has different
meaning as a military time zone than as an obsolete
RFC 822 time zone. Instead, it’s better to use
unambiguous numeric time zone corrections like ‘`-0500`’, as
described in the previous section.

If neither a time zone item nor a time zone correction is supplied,
timestamps are interpreted using the rules of the default time zone
(see [Specifying time zone rules](#Specifying-time-zone-rules)).

---

Next: [Day of week items](#Day-of-week-items), Previous: [Time zone items](#Time-zone-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.5 Combined date and time of day items

The ISO 8601 date and time of day extended format consists of an ISO
8601 date, a ‘`T`’ character separator, and an ISO 8601 time of
day. This format is also recognized if the ‘`T`’ is replaced by a
space.

In this format, the time of day should use 24-hour notation.
Fractional seconds are allowed, with either comma or period preceding
the fraction. ISO 8601 fractional minutes and hours are not
supported. Typically, hosts support nanosecond timestamp resolution;
excess precision is silently discarded.

Here are some examples:

```
2022-09-24T20:02:00.052-05:00
2022-12-31T23:59:59,999999999+11:00
1970-01-01 00:00Z
```

---

Next: [Relative items in date strings](#Relative-items-in-date-strings), Previous: [Combined date and time of day items](#Combined-date-and-time-of-day-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.6 Day of week items

The explicit mention of a day of the week will forward the date
(only if necessary) to reach that day of the week in the future.

Days of the week may be spelled out in full: ‘`Sunday`’,
‘`Monday`’, ‘`Tuesday`’, ‘`Wednesday`’, ‘`Thursday`’,
‘`Friday`’ or ‘`Saturday`’. Days may be abbreviated to their
first three letters, optionally followed by a period. The special
abbreviations ‘`Tues`’ for ‘`Tuesday`’, ‘`Wednes`’ for
‘`Wednesday`’ and ‘`Thur`’ or ‘`Thurs`’ for ‘`Thursday`’ are
also allowed.

A number may precede a day of the week item to move forward
supplementary weeks. It is best used in expression like ‘`third
monday`’. In this context, ‘`last day`’ or ‘`next
day`’ is also acceptable; they move one week before or after
the day that day by itself would represent.

A comma following a day of the week item is ignored.

---

Next: [Pure numbers in date strings](#Pure-numbers-in-date-strings), Previous: [Day of week items](#Day-of-week-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.7 Relative items in date strings

*Relative items* adjust a date (or the current date if none) forward
or backward. The effects of relative items accumulate. Here are some
examples:

```
1 year
1 year ago
3 years
2 days
```

The unit of time displacement may be selected by the string ‘`year`’
or ‘`month`’ for moving by whole years or months. These are fuzzy
units, as years and months are not all of equal duration. More precise
units are ‘`fortnight`’ which is worth 14 days, ‘`week`’ worth 7
days, ‘`day`’ worth 24 hours, ‘`hour`’ worth 60 minutes,
‘`minute`’ or ‘`min`’ worth 60 seconds, and ‘`second`’ or
‘`sec`’ worth one second. An ‘`s`’ suffix on these units is
accepted and ignored.

The unit of time may be preceded by a multiplier, given as an optionally
signed number. Unsigned numbers are taken as positively signed. No
number at all implies 1 for a multiplier. Following a relative item by
the string ‘`ago`’ is equivalent to preceding the unit by a
multiplier with value *-1*.

The string ‘`tomorrow`’ is worth one day in the future (equivalent
to ‘`day`’), the string ‘`yesterday`’ is worth
one day in the past (equivalent to ‘`day ago`’).

The strings ‘`now`’ or ‘`today`’ are relative items corresponding
to zero-valued time displacement, these strings come from the fact
a zero-valued time displacement represents the current time when not
otherwise changed by previous items. They may be used to stress other
items, like in ‘`12:00 today`’. The string ‘`this`’ also has
the meaning of a zero-valued time displacement, but is preferred in
date strings like ‘`this thursday`’.

When a relative item causes the resulting date to cross a boundary
where the clocks were adjusted, typically for daylight saving time,
the resulting date and time are adjusted accordingly.

The fuzz in units can cause problems with relative items. For
example, ‘`2022-12-31 -1 month`’ might evaluate to 2022-12-01,
because 2022-11-31 is an invalid date. To determine the previous
month more reliably, you can ask for the month before the 15th of the
current month. For example:

```
$ date -R
Thu, 31 Dec 2022 13:02:39 -0400
$ date --date='-1 month' +'Last month was %B?'
Last month was December?
$ date --date="$(date +%Y-%m-15) -1 month" +'Last month was %B!'
Last month was November!
```

Also, take care when manipulating dates around clock changes such as
daylight saving leaps. In a few cases these have added or subtracted
as much as 24 hours from the clock, so it is often wise to adopt
universal time by setting the `TZ` environment variable to
‘`UTC0`’ before embarking on calendrical calculations.

---

Next: [Seconds since the Epoch](#Seconds-since-the-Epoch), Previous: [Relative items in date strings](#Relative-items-in-date-strings), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.8 Pure numbers in date strings

The precise interpretation of a pure decimal number depends
on the context in the date string.

If the decimal number is of the form yyyymmdd and no
other calendar date item (see [Calendar date items](#Calendar-date-items)) appears before it
in the date string, then yyyy is read as the year, mm as the
month number and dd as the day of the month, for the specified
calendar date.

If the decimal number is of the form hhmm and no other time
of day item appears before it in the date string, then hh is read
as the hour of the day and mm as the minute of the hour, for the
specified time of day. mm can also be omitted.

If both a calendar date and a time of day appear to the left of a number
in the date string, but no relative item, then the number overrides the
year.

---

Next: [Specifying time zone rules](#Specifying-time-zone-rules), Previous: [Pure numbers in date strings](#Pure-numbers-in-date-strings), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.9 Seconds since the Epoch

If you precede a number with ‘`@`’, it represents an internal
timestamp as a count of seconds. The number can contain an internal
decimal point (either ‘`.`’ or ‘`,`’); any excess precision not
supported by the internal representation is truncated toward minus
infinity. Such a number cannot be combined with any other date
item, as it specifies a complete timestamp.

Internally, computer times are represented as a count of seconds since
an Epoch—a well-defined point of time. On GNU and
POSIX systems, the Epoch is 1970-01-01 00:00:00 UTC, so
‘`@0`’ represents this time, ‘`@1`’ represents 1970-01-01
00:00:01 UTC, and so forth. GNU and most other
POSIX-compliant systems support such times as an extension
to POSIX, using negative counts, so that ‘`@-1`’
represents 1969-12-31 23:59:59 UTC.

Most modern systems count seconds with 64-bit two’s-complement integers
of seconds with nanosecond subcounts, which is a range that includes
the known lifetime of the universe with nanosecond resolution.
Some obsolescent systems count seconds with 32-bit two’s-complement
integers and can represent times from 1901-12-13 20:45:52 through
2038-01-19 03:14:07 UTC. A few systems sport other time ranges.

On most hosts, these counts ignore the presence of leap seconds.
For example, on most hosts ‘`@1483228799`’ represents 2016-12-31
23:59:59 UTC, ‘`@1483228800`’ represents 2017-01-01 00:00:00
UTC, and there is no way to represent the intervening leap second
2016-12-31 23:59:60 UTC.

---

Next: [Authors of `parse_datetime`](#Authors-of-parse_005fdatetime), Previous: [Seconds since the Epoch](#Seconds-since-the-Epoch), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.10 Specifying time zone rules

Normally, dates are interpreted using the rules of the current time
zone, which in turn are specified by the `TZ` environment
variable, or by a system default if `TZ` is not set. To specify a
different set of default time zone rules that apply just to one date,
start the date with a string of the form ‘`TZ="rule"`’. The
two quote characters (‘`"`’) must be present in the date, and any
quotes or backslashes within rule must be escaped by a
backslash.

For example, with the GNU `date` command you can
answer the question “What time is it in New York when a Paris clock
shows 6:30am on October 31, 2022?” by using a date beginning with
‘`TZ="Europe/Paris"`’ as shown in the following shell transcript:

```
$ export TZ="America/New_York"
$ date --date='TZ="Europe/Paris" 2022-10-31 06:30'
Mon Oct 31 01:30:00 EDT 2022
```

In this example, the `--date` operand begins with its own
`TZ` setting, so the rest of that operand is processed according
to ‘`Europe/Paris`’ rules, treating the string ‘`2022-11-14
06:30`’ as if it were in Paris. However, since the output of the
`date` command is processed according to the overall time zone
rules, it uses New York time. (Paris was normally six hours ahead of
New York in 2022, but this example refers to a brief Halloween period
when the gap was five hours.)

A `TZ` value is a rule that typically names a location in the
[‘`tz`’ database](https://www.iana.org/time-zones).
A recent catalog of location names appears in the
[TWiki Date and Time
Gateway](https://twiki.org/cgi-bin/xtra/tzdatepick.html). A few non-GNU hosts require a colon before a
location name in a `TZ` setting, e.g.,
‘`TZ=":America/New_York"`’.

The ‘`tz`’ database includes a wide variety of locations ranging
from ‘`Africa/Abidjan`’ to ‘`Pacific/Tongatapu`’, but
if you are at sea and have your own private time zone, or if you are
using a non-GNU host that does not support the ‘`tz`’
database, you may need to use a POSIX rule instead.
The previously-mentioned POSIX rule ‘`UTC0`’ says that the time zone
abbreviation is ‘`UTC`’, the zone is zero hours away from
Greenwich, and there is no daylight saving time.
POSIX rules can also specify nonzero Greenwich offsets.
For example, the following shell transcript answers the question
“What time is it five and a half hours east of Greenwich when a clock
seven hours west of Greenwich shows 9:50pm on July 12, 2022?”

```
$ TZ="<+0530>-5:30" date --date='TZ="<-07>+7" 2022-07-12 21:50'
Wed Jul 13 10:20:00 +0530 2022
```

This example uses the somewhat-confusing POSIX convention for rules.
‘`TZ="<-07>+7"`’ says that the time zone abbreviation is ‘`-07`’
and the time zone is 7 hours west of Greenwich, and
‘`TZ="<+0530>-5:30"`’ says that the time zone abbreviation is ‘`+0530`’
and the time zone is 5 hours 30 minutes east of Greenwich.
(One should never use a setting like ‘`TZ="UTC-5"`’, since
this would incorrectly imply that local time is five hours east of
Greenwich and the time zone is called “UTC”.)
Although trickier POSIX `TZ` settings like
‘`TZ="<-05>+5<-04>,M3.2.0/2,M11.1.0/2"`’ can specify some daylight
saving regimes, location-based settings like
‘`TZ="America/New_York"`’ are typically simpler and more accurate
historically. See [Specifying the Time Zone with `TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library.

---

Previous: [Specifying time zone rules](#Specifying-time-zone-rules), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 7.11 Authors of `parse_datetime`

`parse_datetime` started life as `getdate`, as originally
implemented by Steven M. Bellovin
([smb@research.att.com](mailto:smb@research.att.com)) while at the University of North Carolina
at Chapel Hill. The code was later tweaked by a couple of people on
Usenet, then completely overhauled by Rich $alz ([rsalz@bbn.com](mailto:rsalz@bbn.com))
and Jim Berets ([jberets@bbn.com](mailto:jberets@bbn.com)) in August, 1990. Various
revisions for the GNU system were made by David MacKenzie, Jim Meyering,
Paul Eggert and others, including renaming it to `get_date` to
avoid a conflict with the alternative Posix function `getdate`,
and a later rename to `parse_datetime`. The Posix function
`getdate` can parse more locale-specific dates using
`strptime`, but relies on an environment variable and external
file, and lacks the thread safety of `parse_datetime`.

This chapter was originally produced by François Pinard
([pinard@iro.umontreal.ca](mailto:pinard@iro.umontreal.ca)) from the `parse_datetime.y` source code,
and then edited by K. Berry ([kb@cs.umb.edu](mailto:kb@cs.umb.edu)).

---

Next: [Tapes and Other Archive Media](#Media), Previous: [Date input formats](#Date-input-formats), Up: [GNU tar: an archiver tool](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]## 8 Controlling the Archive Format

Due to historical reasons, there are several formats of tar archives.
All of them are based on the same principles, but have some subtle
differences that often make them incompatible with each other.

GNU tar is able to create and handle archives in a variety of formats.
The most frequently used formats are (in alphabetical order):

gnu
:   Format used by GNU `tar` versions starting with 1.13. This format derived
    from an early POSIX standard, adding some improvements such as
    sparse file handling and incremental archives. Unfortunately these
    features were implemented in a way incompatible with other archive
    formats.

    Archives in ‘`gnu`’ format are able to hold file names of unlimited
    length.
    However, file timestamps in this format are integers, so the timestamps
    have only one-second resolution; any subsecond information is lost.
    Also, UIDs and GIDs are limited to 64 bits, and device numbers to 63 bits.

oldgnu
:   Format used by GNU `tar` of versions prior to 1.12.

v7
:   Archive format, compatible with the V7 implementation of tar. This
    format imposes a number of limitations. The most important of them
    are:

    1. File names and symbolic links can contain at most 100 bytes.
    2. File sizes must be less than 8 GiB (*2^33* bytes = 8,589,934,592 bytes).
    3. File timestamps must be nonnegative integers less than *2^33* seconds
       since the Epoch, corresponding to the range from 1970-01-01 00:00:00
       (inclusive) to 2242-03-16 12:56:32 (exclusive) UTC, ignoring leap seconds.
    4. It is impossible to store special files (block and character
       devices, fifos etc.)
    5. UIDs and GIDs must be less than *2^21* (2,097,152).
    6. V7 archives do not contain symbolic ownership information (user
       and group name of the file owner).

    This format has traditionally been used by Automake when producing
    Makefiles. This practice will change in the future, in the meantime,
    however this means that projects containing file names more than 100
    bytes long will not be able to use GNU `tar` 1.35.90 and
    Automake prior to 1.9.

ustar
:   Archive format defined by POSIX.1-1988 and later. It stores
    symbolic ownership information. It is also able to store
    special files. However, it imposes several restrictions as well:

    1. File names can contain at most 255 bytes.
    2. File names longer than 100 bytes must be split at a directory separator in
       two parts, the first being at most 155 bytes long.
       So, in most cases file names must be a bit shorter than 255 bytes.
    3. Symbolic links can contain at most 100 bytes.
    4. File sizes must be less than 8 GiB (*2^33* bytes = 8,589,934,592 bytes).
    5. File timestamps must nonnegative integers less than *2^33* seconds
       since the Epoch, corresponding to the range from 1970-01-01 00:00:00
       (inclusive) to 2242-03-16 12:56:32 (exclusive) UTC, ignoring leap seconds.
    6. UIDs, GIDs, device major numbers, and device minor numbers
       must be less than *2^21* (2,097,152).

star
:   The format used by the late Jörg Schilling’s `star`
    implementation. GNU `tar` can read ‘`star`’ archives but
    does not produce them.

posix
:   The format defined by POSIX.1-2001 and later. This is the
    most flexible and feature-rich format. It does not impose arbitrary
    restrictions on file sizes, file timestamp range or resolution,
    file name lengths, and number of bits in UIDs or device numbers.
    This format is more
    recent, so some `tar` implementations cannot handle it properly.
    However, any `tar` implementation able to read ‘`ustar`’
    archives should be able to read most ‘`posix`’ archives as well,
    except that it will extract any additional information (such as long
    file names) as extra plain text files and their parent directories.

    This archive format will be the default format for future versions
    of GNU `tar`.

The following table summarizes the limits of each of these formats.
“Size” and “Date” stands for file size and date,
“Res” for file timestamp resolution (where “1 s” means one second),
“Name” for length of file names,
“Sym” for length of symbolic links,
“Un” for the number of bits in a UID or GID, and
“Dn” for the number of bits in a device number.
A “-” means the archive format imposes no practical limits,
though the operating system may have limits of its own.

| Format | Size | Date | Res | Name | Sym | Un | Dn |
| --- | --- | --- | --- | --- | --- | --- | --- |
| v7 | < 8 GiB | 1970–2242 | 1 s | 100 | 100 | 21 | n/a |
| ustar | < 8 GiB | 1970–2242 | 1 s | 255 | 100 | 21 | 21 |
| oldgnu | - | - | 1 s | - | - | 64 | 63 |
| gnu | - | - | 1 s | - | - | 64 | 63 |
| posix | - | - | - | - | - | - | - |

The default format for GNU `tar` is defined at compilation
time. You may check it by running `tar --help`, and examining
the last lines of its output. Usually, GNU `tar` is configured
to create archives in ‘`gnu`’ format, however, a future version will
switch to ‘`posix`’.

* [Using Less Space through Compression](#Compression)
* [Handling File Attributes](#Attributes)
* [Making `tar` Archives More Portable](#Portability)
* [Making `tar` Archives More Reproducible](#Reproducibility)
* [Comparison of `tar` and `cpio`](#cpio)

---

Next: [Handling File Attributes](#Attributes), Up: [Controlling the Archive Format](#Formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

### 8.1 Using Less Space through Compression

* [Creating and Reading Compressed Archives](#gzip)
* [Archiving Sparse Files](#sparse)

---

Next: [Archiving Sparse Files](#sparse), Up: [Using Less Space through Compression](#Compression)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index-of-Command-Line-Options "Index")]

#### 8.1.1 Creating and Reading Compressed Archives

GNU `tar` is able to create and read compressed archives. It supports
a wide variety of compression programs, namely: `gzip`,
`bzip2`, `lzip`, `lzma`, `lzop`,
`zstd`, `xz` and traditional `compress`. The
latter is supported mostly for backward compatibility, and we recommend
against using it, because it is by far less effective than the other
compression programs[22](#FOOT22).

Creating a compressed archive is simple: you just specify a
*compression option* along with the usual archive creation
commands. Available compression options are summarized in the
table below:

| Long | Short | Archive format |
| --- | --- | --- |
| `--gzip` | `-z` | `gzip` |
| `--bzip2` | `-j` | `bzip2` |
| `--xz` | `-J` | `xz` |
| `--lzip` |  | `lzip` |
| `--lzma` |  | `lzma` |
| `--lzop` |  | `lzop` |
| `--zstd` |  | `zstd` |
| `--compress` | `-Z` | `compress` |

For example:

```
$ tar czf archive.tar.gz .
```

You can also let GNU `tar` select the compression program based on
the suffix of the archive file name. This is done using
`--auto-compress` (`-a`) command line option. For
example, the following invocation will use `bzip2` for
compression:

```
$ tar caf archive.tar.bz2 .
```

whereas the following one will use `lzma`:

```
$ tar caf archive.tar.lzma .
```

For a complete list of file name suffixes recognized by GNU `tar`,
see [auto-compress](#auto_002dcompress).

Reading compressed archive is even simpler: you don’t need to specify
any additional options as GNU `tar` recognizes its format
automatically. Thus, the following commands will list and extract the
archive created in previous example:

```
# List the compressed archive
$ tar tf archive.tar.gz
