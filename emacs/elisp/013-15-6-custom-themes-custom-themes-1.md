### 15.6 Custom Themes [¶](#Custom-Themes-1)

*Custom themes* are collections of settings that can be enabled
or disabled as a unit. See [Custom Themes](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Custom-Themes) in The GNU Emacs
Manual. Each Custom theme is defined by an Emacs Lisp source file,
which should follow the conventions described in this section.
(Instead of writing a Custom theme by hand, you can also create one
using a Customize-like interface; see [Creating Custom Themes](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Creating-Custom-Themes) in The GNU Emacs Manual.)

A Custom theme file should be named `foo-theme.el`, where
foo is the theme name. The first Lisp form in the file should
be a call to `deftheme`, and the last form should be a call to
`provide-theme`.

Macro: **deftheme** theme &optional doc &rest properties [¶](#index-deftheme)
:   This macro declares theme (a symbol) as the name of a Custom
    theme. The optional argument doc should be a string describing
    the theme; this is the description shown when the user invokes the
    `describe-theme` command or types `?` in the ‘`*Custom
    Themes*`’ buffer. The remaining arguments properties are used
    pass a property list with theme attributes.

    The following attributes are supported:

    `:family`
    :   A symbol designating what “family” a theme belongs to. A
        *family* of themes is a set of similar themes that differ by minor
        aspects, such as face colors that are meant for the light vs dark
        background of the frame.

    `:kind`
    :   A symbol. If a theme is enabled and this property has the value
        `color-scheme`, then the `theme-choose-variant` command will
        look for other available themes that belong to the same family in
        order to switch the themes. Other values are currently unspecified
        and should not be used.

    `:background-mode`
    :   A symbol, either `light` or `dark`. This attribute is
        currently unused, but should still be specified.

    Two special theme names are disallowed (using them causes an error):
    `user` is a dummy theme that stores the user’s direct
    customization settings, and `changed` is a dummy theme that
    stores changes made outside of the Customize system.

Macro: **provide-theme** theme [¶](#index-provide_002dtheme)
:   This macro declares that the theme named theme has been fully
    specified.

In between `deftheme` and `provide-theme` are Lisp forms
specifying the theme settings: usually a call to
`custom-theme-set-variables` and/or a call to
`custom-theme-set-faces`.

Function: **custom-theme-set-variables** theme &rest args [¶](#index-custom_002dtheme_002dset_002dvariables)
:   This function specifies the Custom theme theme’s variable
    settings. theme should be a symbol. Each argument in
    args should be a list of the form

    ```
    (var expression [now [request [comment]]])
    ```

    where the list entries have the same meanings as in
    `custom-set-variables`. See [Applying Customizations](#Applying-Customizations).

Function: **custom-theme-set-faces** theme &rest args [¶](#index-custom_002dtheme_002dset_002dfaces)
:   This function specifies the Custom theme theme’s face settings.
    theme should be a symbol. Each argument in args should be
    a list of the form

    ```
    (face spec [now [comment]])
    ```

    where the list entries have the same meanings as in
    `custom-set-faces`. See [Applying Customizations](#Applying-Customizations).

In theory, a theme file can also contain other Lisp forms, which
would be evaluated when loading the theme, but that is bad form.
To protect against loading themes containing malicious code, Emacs
displays the source file and asks for confirmation from the user
before loading any non-built-in theme for the first time. As
such, themes are not ordinarily byte-compiled, and source files
usually take precedence when Emacs is looking for a theme to load.

The following functions are useful for programmatically enabling and
disabling themes:

Function: **custom-theme-p** theme [¶](#index-custom_002dtheme_002dp)
:   This function return a non-`nil` value if theme (a symbol)
    is the name of a Custom theme (i.e., a Custom theme which has been
    loaded into Emacs, whether or not the theme is enabled). Otherwise,
    it returns `nil`.

Variable: **custom-known-themes** [¶](#index-custom_002dknown_002dthemes)
:   The value of this variable is a list of themes loaded into Emacs.
    Each theme is represented by a Lisp symbol (the theme name). The
    default value of this variable is a list containing two dummy
    themes: `(user changed)`. The `changed` theme stores
    settings made before any Custom themes are applied (e.g., variables
    set outside of Customize). The `user` theme stores settings the
    user has customized and saved. Any additional themes declared with
    the `deftheme` macro are added to the front of this list.

Command: **load-theme** theme &optional no-confirm no-enable [¶](#index-load_002dtheme)
:   This function loads the Custom theme named theme from its source
    file, looking for the source file in the directories specified by the
    variable `custom-theme-load-path`. See [Custom Themes](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Custom-Themes) in The GNU Emacs Manual. It also *enables* the theme (unless the
    optional argument no-enable is non-`nil`), causing its
    variable and face settings to take effect. It prompts the user for
    confirmation before loading the theme, unless the optional argument
    no-confirm is non-`nil`.

Function: **require-theme** feature &optional noerror [¶](#index-require_002dtheme)
:   This function searches `custom-theme-load-path` for a file that
    provides feature and then loads it. This is like the function
    `require` (see [Features](#Named-Features)), except it searches
    `custom-theme-load-path` instead of `load-path`
    (see [Library Search](#Library-Search)). This can be useful in Custom themes that
    need to load supporting Lisp files when `require` is unsuitable
    for that.

    If feature, which should be a symbol, is not already present in
    the current Emacs session according to `featurep`, then
    `require-theme` searches for a file named feature with an
    added ‘`.elc`’ or ‘`.el`’ suffix, in that order, in the
    directories specified by `custom-theme-load-path`.

    If a file providing feature is successfully found and loaded,
    then `require-theme` returns feature. The optional
    argument noerror determines what happens if the search or
    loading fails. If it is `nil`, the function signals an error;
    otherwise, it returns `nil`. If the file loads successfully but
    does not provide feature, then `require-theme` signals an
    error; this cannot be suppressed.

Command: **enable-theme** theme [¶](#index-enable_002dtheme)
:   This function enables the Custom theme named theme. It signals
    an error if no such theme has been loaded.

Command: **disable-theme** theme [¶](#index-disable_002dtheme)
:   This function disables the Custom theme named theme. The theme
    remains loaded, so that a subsequent call to `enable-theme` will
    re-enable it.

---

Next: [Byte Compilation](#Byte-Compilation), Previous: [Customization Settings](#Customization), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 16 Loading [¶](#Loading-1)

Loading a file of Lisp code means bringing its contents into the
Lisp environment in the form of Lisp objects. Emacs finds and opens
the file, reads the text, evaluates each form, and then closes the
file. Such a file is also called a *Lisp library*.

The load functions evaluate all the expressions in a file just
as the `eval-buffer` function evaluates all the
expressions in a buffer. The difference is that the load functions
read and evaluate the text in the file as found on disk, not the text
in an Emacs buffer.

The loaded file must contain Lisp expressions, either as source code
or as byte-compiled code. Each form in the file is called a
*top-level form*. There is no special format for the forms in a
loadable file; any form in a file may equally well be typed directly
into a buffer and evaluated there. (Indeed, most code is tested this
way.) Most often, the forms are function definitions and variable
definitions.

Emacs can also load compiled dynamic modules: shared libraries that
provide additional functionality for use in Emacs Lisp programs, just
like a package written in Emacs Lisp would. When a dynamic module is
loaded, Emacs calls a specially-named initialization function which
the module needs to implement, and which exposes the additional
functions and variables to Emacs Lisp programs.

For on-demand loading of external libraries which are known in advance
to be required by certain Emacs primitives, see [Dynamically Loaded Libraries](#Dynamic-Libraries).

* [How Programs Do Loading](#How-Programs-Do-Loading)
* [Load Suffixes](#Load-Suffixes)
* [Library Search](#Library-Search)
* [Loading Non-ASCII Characters](#Loading-Non_002dASCII)
* [Autoload](#Autoload)
* [Repeated Loading](#Repeated-Loading)
* [Features](#Named-Features)
* [Which File Defined a Certain Symbol](#Where-Defined)
* [Unloading](#Unloading)
* [Hooks for Loading](#Hooks-for-Loading)
* [Emacs Dynamic Modules](#Dynamic-Modules)

---

Next: [Load Suffixes](#Load-Suffixes), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.1 How Programs Do Loading [¶](#How-Programs-Do-Loading-1)

Emacs Lisp has several interfaces for loading. For example,
`autoload` creates a placeholder object for a function defined in a
file; trying to call the autoloading function loads the file to get the
function’s real definition (see [Autoload](#Autoload)). `require` loads a
file if it isn’t already loaded (see [Features](#Named-Features)). Ultimately,
all these facilities call the `load` function to do the work.

Function: **load** filename &optional missing-ok nomessage nosuffix must-suffix [¶](#index-load)
:   This function finds and opens a file of Lisp code, evaluates all the
    forms in it, and closes the file.

    To find the file, `load` first looks for a file named
    `filename.elc`, that is, for a file whose name is
    filename with the extension ‘`.elc`’ appended. If such a
    file exists, and Emacs was compiled with native-compilation support
    (see [Compilation of Lisp to Native Code](#Native-Compilation)), `load` attempts to find a
    corresponding ‘`.eln`’ file, and if found, loads it instead of
    `filename.elc`. Otherwise, it loads
    `filename.elc` (and starts a background native compilation
    to produce the missing ‘`.eln`’ file, followed by loading that
    file). If there is no `filename.elc`, then `load`
    looks for a file named `filename.el`. If that file exists,
    it is loaded. If Emacs was compiled with support for dynamic modules
    (see [Emacs Dynamic Modules](#Dynamic-Modules)), `load` next looks for a file named
    `filename.ext`, where ext is a system-dependent
    file-name extension of shared libraries (‘`.so`’ on GNU and Unix
    systems). Finally, if neither of those names is found, `load`
    looks for a file named filename with nothing appended, and loads
    it if it exists. (The `load` function is not clever about
    looking at filename. In the perverse case of a file named
    `foo.el.el`, evaluation of `(load "foo.el")` will indeed
    find it.)

    If Auto Compression mode is enabled, as it is by default, then if
    `load` can not find a file, it searches for a compressed version
    of the file before trying other file names. It decompresses and loads
    it if it exists. It looks for compressed versions by appending each
    of the suffixes in `jka-compr-load-suffixes` to the file name.
    The value of this variable must be a list of strings. Its standard
    value is `(".gz")`.

    If the optional argument nosuffix is non-`nil`, then
    `load` does not try the suffixes ‘`.elc`’ and ‘`.el`’. In
    this case, you must specify the precise file name you want, except
    that, if Auto Compression mode is enabled, `load` will still use
    `jka-compr-load-suffixes` to find compressed versions. By
    specifying the precise file name and using `t` for
    nosuffix, you can prevent file names like `foo.el.el` from
    being tried.

    If the optional argument must-suffix is non-`nil`, then
    `load` insists that the file name used must end in either
    ‘`.el`’ or ‘`.elc`’ (possibly extended with a compression
    suffix) or the shared-library extension, unless it contains an
    explicit directory name.

    If the option `load-prefer-newer` is non-`nil`, then when
    searching suffixes, `load` selects whichever version of a file
    (‘`.elc`’, ‘`.el`’, etc.) has been modified most recently.
    In this case, `load` doesn’t load the ‘`.eln`’
    native-compiled file even if it exists.

    If filename is a relative file name, such as `foo` or
    `baz/foo.bar`, `load` searches for the file using the variable
    `load-path`. It appends filename to each of the directories
    listed in `load-path`, and loads the first file it finds whose name
    matches. The current default directory is tried only if it is specified
    in `load-path`, where `nil` stands for the default directory.
    `load` tries all three possible suffixes in the first directory in
    `load-path`, then all three suffixes in the second directory, and
    so on. See [Library Search](#Library-Search).

    Whatever the name under which the file is eventually found, and the
    directory where Emacs found it, Emacs sets the value of the variable
    `load-file-name` to that file’s name.

    If you get a warning that `foo.elc` is older than `foo.el`, it
    means you should consider recompiling `foo.el`. See [Byte Compilation](#Byte-Compilation).

    When loading a source file (not compiled), `load` performs
    character set translation just as Emacs would do when visiting the file.
    See [Coding Systems](#Coding-Systems).

    When loading an uncompiled file, Emacs tries to expand any macros
    that the file contains (see [Macros](#Macros)). We refer to this as
    *eager macro expansion*. Doing this (rather than deferring
    the expansion until the relevant code runs) can significantly speed
    up the execution of uncompiled code. Sometimes, this macro expansion
    cannot be done, owing to a cyclic dependency. In the simplest
    example of this, the file you are loading refers to a macro defined
    in another file, and that file in turn requires the file you are
    loading. Emacs will issue an error about
    (‘`Eager macro-expansion skipped due to cycle…`’)
    giving details of the problem. You have to restructure
    your code so that this does not happen. Loading a compiled file does
    not cause macroexpansion, because this should already have happened
    during compilation. See [Macros and Byte Compilation](#Compiling-Macros).

    Messages like ‘`Loading foo...`’ and ‘`Loading foo...done`’ appear
    in the echo area during loading unless nomessage is
    non-`nil`. If a native-compiled ‘`.eln`’ file is loaded,
    the message says so.

    Any unhandled errors while loading a file terminate loading. If the
    load was done for the sake of `autoload`, any function definitions
    made during the loading are undone.

    If `load` can’t find the file to load, then normally it signals a
    `file-error` (with ‘`Cannot open load file
    filename`’). But if missing-ok is non-`nil`, then
    `load` just returns `nil`.

    You can use the variable `load-read-function` to specify a function
    for `load` to use instead of `read` for reading expressions.
    See below.

    `load` returns `t` if the file loads successfully.

Command: **load-file** filename [¶](#index-load_002dfile)
:   This command loads the file filename. If filename is a
    relative file name, then the current default directory is assumed.
    This command does not use `load-path`, and does not append
    suffixes. However, it does look for compressed versions (if Auto
    Compression Mode is enabled). Use this command if you wish to specify
    precisely the file name to load.

Command: **load-library** library [¶](#index-load_002dlibrary)
:   This command loads the library named library. It is equivalent to
    `load`, except for the way it reads its argument interactively.
    See [Lisp Libraries](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Lisp-Libraries) in The GNU Emacs Manual.

Variable: **load-in-progress** [¶](#index-load_002din_002dprogress)
:   This variable is non-`nil` if Emacs is in the process of loading a
    file, and it is `nil` otherwise.

Variable: **load-file-name** [¶](#index-load_002dfile_002dname)
:   When Emacs is in the process of loading a file, this variable’s value
    is the name of that file, as Emacs found it during the search
    described earlier in this section.

Variable: **load-read-function** [¶](#index-load_002dread_002dfunction)
:   This variable specifies an alternate expression-reading function for
    `load` and `eval-region` to use instead of `read`.
    The function should accept one argument, just as `read` does.

    By default, this variable’s value is `read`. See [Input Functions](#Input-Functions).

    Instead of using this variable, it is cleaner to use another, newer
    feature: to pass the function as the read-function argument to
    `eval-region`. See [Eval](#Definition-of-eval_002dregion).

Function: **load-path-filter-cache-directory-files** path filename suffixes [¶](#index-load_002dpath_002dfilter_002dcache_002ddirectory_002dfiles)
:   This function filters path to remove any directories that could
    not hold filename with any of suffixes, and returns the
    filtered list of directories. The function caches the directories it
    scans and the files inside them, and uses the cache in subsequent calls,
    which speeds up repeated lookups of files in path.

Variable: **load-path-filter-function** [¶](#index-load_002dpath_002dfilter_002dfunction)
:   If this variable names a function, `load` will call that function
    when it scans `load-path` to find files. The function will be
    called with 3 arguments: the value of `load-path`, filename,
    the name of a file being looked up as passed to `load`, and a list
    of suffixes to append to filename. It should return a shorter
    list of directories where filename can reside, thus making the
    lookup faster. The function
    `load-path-filter-cache-directory-files` is a good candidate to be
    such a function.

For information about how `load` is used in building Emacs, see
[Building Emacs](#Building-Emacs).

---

Next: [Library Search](#Library-Search), Previous: [How Programs Do Loading](#How-Programs-Do-Loading), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.2 Load Suffixes [¶](#Load-Suffixes-1)

We now describe some technical details about the exact suffixes that
`load` tries.

Variable: **load-suffixes** [¶](#index-load_002dsuffixes)
:   This is a list of suffixes indicating (compiled or source) Emacs Lisp
    files. It should not include the empty string. `load` uses
    these suffixes in order when it appends Lisp suffixes to the specified
    file name. The standard value is `(".elc" ".el")` which produces
    the behavior described in the previous section.

Variable: **load-file-rep-suffixes** [¶](#index-load_002dfile_002drep_002dsuffixes)
:   This is a list of suffixes that indicate representations of the same
    file. This list should normally start with the empty string.
    When `load` searches for a file it appends the suffixes in this
    list, in order, to the file name, before searching for another file.

    Enabling Auto Compression mode appends the suffixes in
    `jka-compr-load-suffixes` to this list and disabling Auto
    Compression mode removes them again. The standard value of
    `load-file-rep-suffixes` if Auto Compression mode is disabled is
    `("")`. Given that the standard value of
    `jka-compr-load-suffixes` is `(".gz")`, the standard value
    of `load-file-rep-suffixes` if Auto Compression mode is enabled
    is `("" ".gz")`.

Function: **get-load-suffixes** [¶](#index-get_002dload_002dsuffixes)
:   This function returns the list of all suffixes that `load` should
    try, in order, when its must-suffix argument is non-`nil`.
    This takes both `load-suffixes` and `load-file-rep-suffixes`
    into account. If `load-suffixes`, `jka-compr-load-suffixes`
    and `load-file-rep-suffixes` all have their standard values, this
    function returns `(".elc" ".elc.gz" ".el" ".el.gz")` if Auto
    Compression mode is enabled and `(".elc" ".el")` if Auto
    Compression mode is disabled.

To summarize, `load` normally first tries the suffixes in the
value of `(get-load-suffixes)` and then those in
`load-file-rep-suffixes`. If nosuffix is non-`nil`,
it skips the former group, and if must-suffix is non-`nil`,
it skips the latter group.

User Option: **load-prefer-newer** [¶](#index-load_002dprefer_002dnewer)
:   If this option is non-`nil`, then rather than stopping at the
    first suffix that exists, `load` tests them all, and uses
    whichever file is the newest.

---

Next: [Loading Non-ASCII Characters](#Loading-Non_002dASCII), Previous: [Load Suffixes](#Load-Suffixes), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.3 Library Search [¶](#Library-Search-1)

When Emacs loads a Lisp library, it searches for the library
in a list of directories specified by the variable `load-path`.

Variable: **load-path** [¶](#index-load_002dpath)
:   The value of this variable is a list of directories to search when
    loading files with `load`. Each element is a string (which must be
    a directory) or `nil` (which stands for the current working
    directory).

When Emacs starts up, it sets up the value of `load-path` in
several steps. First, it looks for the directory containing its own
Lisp files, using default locations set when Emacs was compiled. It
saves this directory in `lisp-directory`. Normally, this is a
directory where the `*.elc` files are installed, something like

```
"/usr/local/share/emacs/version/lisp"
```

where version is the Emacs version. (In this and the following
examples, replace `/usr/local` with the prefix appropriate for
your Emacs installation.) This directory and its subdirectories
contain the standard Lisp files that come with Emacs. If Emacs cannot
find its own Lisp files, it will not start correctly.

If you run Emacs from the directory where it was built—that is, an
executable that has not been installed yet—Emacs instead initializes
`lisp-directory` using the `lisp` subdirectory of the
directory containing the sources from which it was built.

Emacs then initializes `load-path` with this `lisp-directory`.
If you built Emacs in a separate directory from the
sources, it also adds the `lisp` subdirectory of the build directory.

All of these directories are stored in the above two variables as
absolute file names.

Unless you start Emacs with the `--no-site-lisp` option,
it then adds two more `site-lisp` directories to the front of
`load-path`. These are intended for locally installed Lisp files,
and are normally of the form:

```
"/usr/local/share/emacs/version/site-lisp"
```

and

```
"/usr/local/share/emacs/site-lisp"
```

The first one is for locally installed files for the current Emacs
version; the second is for locally installed files meant for use
with any installed Emacs version. (If Emacs is running uninstalled,
it also adds `site-lisp` subdirectories from the source and build
directories, if they exist. However, normally the source and build
directories do not contain `site-lisp` subdirectories.)

If the environment variable `EMACSLOADPATH` is set, it modifies
the above initialization procedure. Emacs initializes
`load-path` based on the value of the environment variable.

The syntax of `EMACSLOADPATH` is the same as used for `PATH`;
directories are separated by ‘`:`’ (or ‘`;`’, on some
operating systems).
Here is an example of how to set `EMACSLOADPATH` variable (from a
`sh`-style shell):

```
export EMACSLOADPATH=/home/foo/.emacs.d/lisp:
```

An empty element in the value of the environment variable, whether
trailing (as in the above example, note the trailing ‘`:`’),
leading, or embedded, is replaced by the default value of
`load-path` as determined by the standard initialization
procedure. If there are no such empty elements, then
`EMACSLOADPATH` specifies the entire `load-path`. You must
include either an empty element, or the explicit path to the directory
containing the standard Lisp files, else Emacs will not function.
(Another way to modify `load-path` is to use the `-L`
command-line option when starting Emacs; see below.)

For each directory in `load-path`, Emacs then checks to see if
it contains a file `subdirs.el`, and if so, loads it. The
`subdirs.el` file is created when Emacs is built/installed,
and contains code that causes Emacs to add any subdirectories of those
directories to `load-path`. Both immediate subdirectories and
subdirectories multiple levels down are added. But it excludes
subdirectories whose names do not start with a letter or digit, and
subdirectories named `RCS` or `CVS`, and subdirectories
containing a file named `.nosearch`.

Next, Emacs adds any extra load directories that you specify using the
`-L` command-line option (see [Action Arguments](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Action-Arguments) in The
GNU Emacs Manual). It also adds the directories where optional
packages are installed, if any (see [Packaging Basics](#Packaging-Basics)).

It is common to add code to one’s init file (see [The Init File](#Init-File)) to
add one or more directories to `load-path`. For example:

```
(push "~/.emacs.d/lisp" load-path)
```

See [push](#List-Variables), for the description of `push`.

Dumping Emacs uses a special value of `load-path`. If you use
a `site-load.el` or `site-init.el` file to customize the
dumped Emacs (see [Building Emacs](#Building-Emacs)), any changes to `load-path`
that these files make will be lost after dumping.

Variable: **lisp-directory** [¶](#index-lisp_002ddirectory)
:   This variable holds a string naming the directory which holds
    Emacs’s own `*.el` and `*.elc` files. This is usually the
    place where those files are located in the Emacs installation tree,
    unless Emacs is run from its build directory in which case it points
    to the `lisp` subdirectory in the source directory from which
    Emacs was built.

Command: **locate-library** library &optional nosuffix path interactive-call [¶](#index-locate_002dlibrary)
:   This command finds the precise file name for library library. It
    searches for the library in the same way `load` does, and the
    argument nosuffix has the same meaning as in `load`: don’t
    add suffixes ‘`.elc`’ or ‘`.el`’ to the specified name
    library.

    If the path is non-`nil`, that list of directories is used
    instead of `load-path`.

    When `locate-library` is called from a program, it returns the file
    name as a string. When the user runs `locate-library`
    interactively, the argument interactive-call is `t`, and this
    tells `locate-library` to display the file name in the echo area.

Command: **list-load-path-shadows** &optional stringp [¶](#index-list_002dload_002dpath_002dshadows)
:   This command shows a list of *shadowed* Emacs Lisp files. A
    shadowed file is one that will not normally be loaded, despite being
    in a directory on `load-path`, due to the existence of another
    similarly-named file in a directory earlier on `load-path`.

    For instance, suppose `load-path` is set to

    ```
      ("/opt/emacs/site-lisp" "/usr/share/emacs/29.1/lisp")
    ```

    and that both these directories contain a file named `foo.el`.
    Then `(require 'foo)` never loads the file in the second
    directory. Such a situation might indicate a problem in the way Emacs
    was installed.

    When called from Lisp, this function prints a message listing the
    shadowed files, instead of displaying them in a buffer. If the
    optional argument `stringp` is non-`nil`, it instead returns
    the shadowed files as a string.

If Emacs was compiled with support for native compilation
(see [Compilation of Lisp to Native Code](#Native-Compilation)), then when a ‘`.elc`’ byte-compiled
file is found by searching `load-path`, Emacs will try to look
for a corresponding ‘`.eln`’ file holding the corresponding
native-compiled code. The native-compiled files are looked up in
the directories listed by the `native-comp-eln-load-path`.

Variable: **native-comp-eln-load-path** [¶](#index-native_002dcomp_002deln_002dload_002dpath)
:   This variable holds a list of directories where Emacs looks for
    native-compiled ‘`.eln`’ files. File names in the list that are
    not absolute are interpreted as relative to `invocation-directory`
    (see [Operating System Environment](#System-Environment)). The last directory in the list is the
    system directory, i.e. the directory with ‘`.eln`’ files
    installed by the Emacs build and installation procedure. In each of
    the directories in the list, Emacs looks for ‘`.eln`’ files in a
    subdirectory whose name is constructed from the Emacs version and an
    8-character hash that depends on the current native-compilation
    ABI; the name of this subdirectory is stored in the variable
    `comp-native-version-dir`.

---

Next: [Autoload](#Autoload), Previous: [Library Search](#Library-Search), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.4 Loading Non-ASCII Characters [¶](#Loading-Non_002dASCII-Characters)

When Emacs Lisp programs contain string constants with non-ASCII
characters, these can be represented within Emacs either as unibyte
strings or as multibyte strings (see [Text Representations](#Text-Representations)). Which
representation is used depends on how the file is read into Emacs. If
it is read with decoding into multibyte representation, the text of the
Lisp program will be multibyte text, and its string constants will be
multibyte strings. If a file containing Latin-1 characters (for
example) is read without decoding, the text of the program will be
unibyte text, and its string constants will be unibyte strings.
See [Coding Systems](#Coding-Systems).

In most Emacs Lisp programs, the fact that non-ASCII
strings are multibyte strings should not be noticeable, since
inserting them in unibyte buffers converts them to unibyte
automatically. However, if this does make a difference, you can force
a particular Lisp file to be interpreted as unibyte by writing
‘`coding: raw-text`’ in a local variables section. With
that designator, the file will unconditionally be interpreted as
unibyte. This can matter when making key bindings to
non-ASCII characters written as `?vliteral`.

---

Next: [Repeated Loading](#Repeated-Loading), Previous: [Loading Non-ASCII Characters](#Loading-Non_002dASCII), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.5 Autoload [¶](#Autoload-1)

The *autoload* facility lets you register the existence of a
function or macro, but put off loading the file that defines it. The
first call to the function automatically loads the proper library, in
order to install the real definition and other associated code, then
runs the real definition as if it had been loaded all along.
Autoloading can also be triggered by looking up the documentation of
the function or macro (see [Documentation Basics](#Documentation-Basics)), and completion
of variable and function names (see [Autoload by Prefix](#Autoload-by-Prefix) below).

There are two ways to set up an autoloaded function: by calling
`autoload`, and by writing a “magic” comment in the
source before the real definition. `autoload` is the low-level
primitive for autoloading; any Lisp program can call `autoload` at
any time. Magic comments are the most convenient way to make a function
autoload, for packages installed along with Emacs. These comments do
nothing on their own, but they serve as a guide for the command
`loaddefs-generate`, which constructs calls to `autoload`
and arranges to execute them when Emacs is built.

Function: **autoload** function filename &optional docstring interactive type [¶](#index-autoload-1)
:   This function defines the function (or macro) named function so as
    to load automatically from filename. The string filename
    specifies the file to load to get the real definition of function.

    If filename does not contain either a directory name, or the
    suffix `.el` or `.elc`, this function insists on adding one
    of these suffixes, and it will not load from a file whose name is just
    filename with no added suffix. (The variable
    `load-suffixes` specifies the exact required suffixes.)

    The argument docstring is the documentation string for the
    function. Specifying the documentation string in the call to
    `autoload` makes it possible to look at the documentation without
    loading the function’s real definition. Normally, this should be
    identical to the documentation string in the function definition
    itself. If it isn’t, the function definition’s documentation string
    takes effect when it is loaded.

    If interactive is non-`nil`, that says function can be
    called interactively. This lets completion in `M-x` work without
    loading function’s real definition. The complete interactive
    specification is not given here; it’s not needed unless the user
    actually calls function, and when that happens, it’s time to load
    the real definition.

    If interactive is a list, it is interpreted as a list of modes
    this command is applicable for.

    You can autoload macros and keymaps as well as ordinary functions.
    Specify type as `macro` if function is really a macro.
    Specify type as `keymap` if function is really a
    keymap. Various parts of Emacs need to know this information without
    loading the real definition.

    An autoloaded keymap loads automatically during key lookup when a prefix
    key’s binding is the symbol function. Autoloading does not occur
    for other kinds of access to the keymap. In particular, it does not
    happen when a Lisp program gets the keymap from the value of a variable
    and calls `keymap-set`; not even if the variable name is the same
    symbol function.

    If function already has a non-void function definition that is not
    an autoload object, this function does nothing and returns `nil`.
    Otherwise, it constructs an autoload object (see [Autoload Type](#Autoload-Type)),
    and stores it as the function definition for function. The
    autoload object has this form:

    ```
    (autoload filename docstring interactive type)
    ```

    For example,

    ```
    (symbol-function 'run-prolog)
         ⇒ (autoload "prolog" 169681 t nil)
    ```

    In this case, `"prolog"` is the name of the file to load, 169681
    refers to the documentation string in the
    `emacs/etc/DOC` file (see [Documentation Basics](#Documentation-Basics)),
    `t` means the function is interactive, and `nil` that it is
    not a macro or a keymap.

Function: **autoloadp** object [¶](#index-autoloadp)
:   This function returns non-`nil` if object is an autoload
    object. For example, to check if `run-prolog` is defined as an
    autoloaded function, evaluate

    ```
    (autoloadp (symbol-function 'run-prolog))
    ```

The autoloaded file usually contains other definitions and may require
or provide one or more features. If the file is not completely loaded
(due to an error in the evaluation of its contents), any function
definitions or `provide` calls that occurred during the load are
undone. This is to ensure that the next attempt to call any function
autoloading from this file will try again to load the file. If not for
this, then some of the functions in the file might be defined by the
aborted load, but fail to work properly for the lack of certain
subroutines not loaded successfully because they come later in the file.

If the autoloaded file fails to define the desired Lisp function or
macro, then an error is signaled with data `"Autoloading failed to
define function function-name"`.

A magic autoload comment (often called an *autoload cookie*)
consists of ‘`;;;###autoload`’, on a line by itself,
just before the real definition of the function in its
autoloadable source file. The function `loaddefs-generate`
writes a corresponding `autoload` call into `loaddefs.el`.
(The string that serves as the autoload cookie and the name of the
file generated by `loaddefs-generate` can be changed from the
above defaults, see below.)
Building Emacs loads `loaddefs.el` and thus calls `autoload`.

The same magic comment can copy any kind of form into
`loaddefs.el`. The form following the magic comment is copied
verbatim, unless it is a form which the autoload facility handles
specially, by conversion directly into an `autoload` call, or by
recursively expanding the macro. Any macro can request expansion of
autoloads which call it during generation; See [autoload-macro expand](#autoload_002dmacro-expand).

The following forms are handled specially:

Definitions for function or function-like objects:
:   `defun` and `defmacro`; also `cl-defun` and
    `cl-defmacro` (see [Argument Lists](https://www.gnu.org/software/emacs/manual/html_mono/cl.html#Argument-Lists) in Common Lisp Extensions),
    and `define-overloadable-function` (see the commentary in
    `mode-local.el`).

Definitions for major or minor modes:
:   `define-minor-mode`, `define-globalized-minor-mode`,
    `define-generic-mode`, `define-derived-mode`, and
    `define-compilation-mode`.

Other definition types:
:   `defcustom`, `defgroup`, `deftheme`, `defclass`
    (see [EIEIO](https://www.gnu.org/software/emacs/manual/html_mono/eieio.html#Top) in EIEIO), `define-skeleton`
    (see [Autotyping](https://www.gnu.org/software/emacs/manual/html_mono/autotype.html#Top) in Autotyping),
    `transient-define-prefix`, `transient-define-suffix`,
    `transient-define-infix`, `transient-define-argument`, and
    `transient-define-group` (see [Transient](transient.html#Top) in Transient
    User and Developer Manual).

You can also use a magic comment to execute a form at build time
*without* executing it when the file itself is loaded. To do this,
write the form *on the same line* as the magic comment. Since it
is in a comment, it does nothing when you load the source file; but
`loaddefs-generate` copies it to `loaddefs.el`, where
it is executed while building Emacs.

The following example shows how `doctor` is prepared for
autoloading with a magic comment:

```
;;;###autoload
(defun doctor ()
  "Switch to *doctor* buffer and start giving psychotherapy."
  (interactive)
  (switch-to-buffer "*doctor*")
  (doctor-mode))
```

Here’s what that produces in `loaddefs.el`:

```
(autoload 'doctor "doctor" "\
Switch to *doctor* buffer and start giving psychotherapy.

\(fn)" t nil)
```

While the `loaddefs.el` isn’t for editing, we try to keep it
somewhat readable for people. For instance, control characters in
`defvar` values are escaped, and we insert a backslash and
newline immediately following the double-quote of the doc string to
keep the line length down. ‘`(fn)`’ in the usage part of the
documentation string is replaced with the function’s name when the
various help functions (see [Help Functions](#Help-Functions)) display it.

If you write a function definition with an unusual macro that is not
one of the known and recognized function definition methods, using an
ordinary magic autoload comment with a call to the macro would require
autoloading the macro definition itself to work. Doing so copies the
whole macro definition into the autoload file. If that is not desired,
you can use the special declare form `(autoload-macro expand)` in
your macro definition (see [The `declare` Form](#Declare-Form)), instead of autoloading it.
This indicates to the autoload system that calls to your macro following
‘`;;;###autoload`’ should be expanded, similar to how the special
functions listed above are handled. For example, a macro which wraps
`define-minor-mode` can request expansion, so that proper
`autoload` calls for the function it defines are generated.

The keyword symbol `:autoload-end` can be used in the expansion of
a macro to prevent including unwanted forms in the autoload output. Its
presence causes any further elements within the form where it appears to
be silently skipped. For example, if during autoload generation, a
macro’s expansion includes:

```
(progn
  (put my-mode 'visible-prop t)
  :autoload-end
  (put my-mode 'hidden-prop nil))
```

the final form (`(put my-mode 'hidden-prop nil)`) will not be
copied into the autoload file.

Note that, if a symbol in the `car` of an autoloaded form is found
to be undefined during autoload generation, the file in which the
associated ‘`;;;###autoload`’ appears will itself be loaded, to give
the macro a chance to be defined. Packages which use special loading
mechanisms, including loading their own `package-loaddefs.el`
file, should therefore gracefully handle load errors, since these files
may not yet exist during autoload generation. This can be done, e.g.,
by setting the no-error argument of `require` non-`nil`
(see [Features](#Named-Features))).

Alternatively, instead of expansion, you can put the desired
`autoload` call into `loaddefs.el` by writing this:

```
;;;###autoload (autoload 'foo "myfile")
(mydefunmacro foo
  ...)
```

You can use a non-default string as the autoload cookie and have the
corresponding autoload calls written into a file whose name is
different from the default `loaddefs.el`. Emacs provides two
variables to control this:

Variable: **lisp-mode-autoload-regexp** [¶](#index-lisp_002dmode_002dautoload_002dregexp)
:   The value of this constant is a regexp that matches autoload cookies.
    `loaddefs-generate` copies the Lisp form that follows the
    cookie into the autoload file it generates. This will match comments
    like ‘`;;;###autoload`’ and ‘`;;;###calc-autoload`’.

Variable: **generated-autoload-file** [¶](#index-generated_002dautoload_002dfile)
:   The value of this variable names an Emacs Lisp file where the autoload
    calls should go. The default value is `loaddefs.el`, but you can
    override that, e.g., in the local variables section of a
    `.el` file (see [File Local Variables](#File-Local-Variables)). The autoload file is
    assumed to contain a trailer starting with a formfeed character.

The following function may be used to explicitly load the library
specified by an autoload object:

Function: **autoload-do-load** autoload &optional name macro-only [¶](#index-autoload_002ddo_002dload)
:   This function performs the loading specified by autoload, which
    should be an autoload object. The optional argument name, if
    non-`nil`, should be a symbol whose function value is
    autoload; in that case, the return value of this function is the
    symbol’s new function value. If the value of the optional argument
    macro-only is `macro`, this function avoids loading a
    function, only a macro.

* [Autoload by Prefix](#Autoload-by-Prefix)
* [When to Use Autoload](#When-to-Autoload)

---

Next: [When to Use Autoload](#When-to-Autoload), Up: [Autoload](#Autoload)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 16.5.1 Autoload by Prefix [¶](#Autoload-by-Prefix-1)

During completion for the commands `describe-variable` and
`describe-function`, Emacs will try to load files which may
contain definitions matching the prefix being completed. The variable
`definition-prefixes` holds a hashtable which maps a prefix to
the corresponding list of files to load for it. Entries to this
mapping are added by calls to `register-definition-prefixes`
which are generated by `loaddefs-generate`
(see [Autoload](#Autoload)). Files which don’t contain any definitions worth
loading (test files, for example), should set
`autoload-compute-prefixes` to `nil` as a file-local
variable.

---

Previous: [Autoload by Prefix](#Autoload-by-Prefix), Up: [Autoload](#Autoload)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 16.5.2 When to Use Autoload [¶](#When-to-Use-Autoload)

Do not add an autoload comment unless it is really necessary.
Autoloading code means it is always globally visible. Once an item is
autoloaded, there is no compatible way to transition back to it not
being autoloaded (after people become accustomed to being able to use it
without an explicit load).

* The most common items to autoload are the interactive entry points to a
  library. For example, if `python.el` is a library defining a
  major-mode for editing Python code, autoload the definition of the
  `python-mode` function, so that people can simply use `M-x
  python-mode` to load the library.
* Variables usually don’t need to be autoloaded. An exception is if the
  variable on its own is generally useful without the whole defining
  library being loaded. (An example of this might be something like
  `find-exec-terminator`.)
* Don’t autoload a user option just so that a user can set it.
* Never add an autoload *comment* to silence a compiler warning in
  another file. In the file that produces the warning, use
  `(defvar foo)` to silence an undefined variable warning, and
  `declare-function` (see [Telling the Compiler that a Function is Defined](#Declaring-Functions)) to silence an
  undefined function warning; or require the relevant library; or use an
  explicit autoload *statement*.

---

Next: [Features](#Named-Features), Previous: [Autoload](#Autoload), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.6 Repeated Loading [¶](#Repeated-Loading-1)

You can load a given file more than once in an Emacs session. For
example, after you have rewritten and reinstalled a function definition
by editing it in a buffer, you may wish to return to the original
version; you can do this by reloading the file it came from.

When you load or reload files, bear in mind that the `load` and
`load-library` functions automatically load a byte-compiled file
rather than a non-compiled file of similar name. If you rewrite a file
that you intend to save and reinstall, you need to byte-compile the new
version; otherwise Emacs will load the older, byte-compiled file instead
of your newer, non-compiled file! If that happens, the message
displayed when loading the file includes, ‘`(compiled; note, source is
newer)`’, to remind you to recompile it.

When writing the forms in a Lisp library file, keep in mind that the
file might be loaded more than once. For example, think about whether
each variable should be reinitialized when you reload the library;
`defvar` does not change the value if the variable is already
initialized. (See [Defining Global Variables](#Defining-Variables).)

The simplest way to add an element to an alist is like this:

```
(push '(leif-mode " Leif") minor-mode-alist)
```

But this would add multiple elements if the library is reloaded. To
avoid the problem, use `add-to-list` (see [Modifying List Variables](#List-Variables)):

```
(add-to-list 'minor-mode-alist '(leif-mode " Leif"))
```

Occasionally you will want to test explicitly whether a library has
already been loaded. If the library uses `provide` to provide a
named feature, you can use `featurep` earlier in the file to test
whether the `provide` call has been executed before (see [Features](#Named-Features)). Alternatively, you could use something like this:

```
(defvar foo-was-loaded nil)

(unless foo-was-loaded
  execute-first-time-only
  (setq foo-was-loaded t))
```

---

Next: [Which File Defined a Certain Symbol](#Where-Defined), Previous: [Repeated Loading](#Repeated-Loading), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.7 Features [¶](#Features)

`provide` and `require` are an alternative to
`autoload` for loading files automatically. They work in terms of
named *features*. Autoloading is triggered by calling a specific
function, but a feature is loaded the first time another program asks
for it by name.

A feature name is a symbol that stands for a collection of functions,
variables, etc. The file that defines them should *provide* the
feature. Another program that uses them may ensure they are defined by
*requiring* the feature. This loads the file of definitions if it
hasn’t been loaded already.

To require the presence of a feature, call `require` with the
feature name as argument. `require` looks in the global variable
`features` to see whether the desired feature has been provided
already. If not, it loads the feature from the appropriate file. This
file should call `provide` at the top level to add the feature to
`features`; if it fails to do so, `require` signals an error.

For example, in `idlwave.el`, the definition for
`idlwave-complete-filename` includes the following code:

```
(defun idlwave-complete-filename ()
  "Use the comint stuff to complete a file name."
   (require 'comint)
   (let* ((comint-file-name-chars "~/A-Za-z0-9+@:_.$#%={}\\-")
          (comint-completion-addsuffix nil)
          ...)
       (comint-dynamic-complete-filename)))
```

The expression `(require 'comint)` loads the file `comint.el`
if it has not yet been loaded, ensuring that
`comint-dynamic-complete-filename` is defined. Features are
normally named after the files that provide them, so that
`require` need not be given the file name. (Note that it is
important that the `require` statement be outside the body of the
`let`. Loading a library while its variables are let-bound can
have unintended consequences, namely the variables becoming unbound
after the let exits.)

The `comint.el` file contains the following top-level expression:

```
(provide 'comint)
```

This adds `comint` to the global `features` list, so that
`(require 'comint)` will henceforth know that nothing needs to be
done.

When `require` is used at top level in a file, it takes effect
when you byte-compile that file (see [Byte Compilation](#Byte-Compilation)) as well as
when you load it. This is in case the required package contains macros
that the byte compiler must know about. It also avoids byte compiler
warnings for functions and variables defined in the file loaded with
`require`.

Although top-level calls to `require` are evaluated during
byte compilation, `provide` calls are not. Therefore, you can
ensure that a file of definitions is loaded before it is byte-compiled
by including a `provide` followed by a `require` for the same
feature, as in the following example.

```
(provide 'my-feature)  ; Ignored by byte compiler,
                       ;   evaluated by load.
(require 'my-feature)  ; Evaluated by byte compiler.
```

The compiler ignores the `provide`, then processes the
`require` by loading the file in question. Loading the file does
execute the `provide` call, so the subsequent `require` call
does nothing when the file is loaded.

Function: **provide** feature &optional subfeatures [¶](#index-provide)
:   This function announces that feature is now loaded, or being
    loaded, into the current Emacs session. This means that the facilities
    associated with feature are or will be available for other Lisp
    programs.

    The direct effect of calling `provide` is to add feature to
    the front of `features` if it is not already in that list and
    call any `eval-after-load` code waiting for it (see [Hooks for Loading](#Hooks-for-Loading)). The argument feature must be a symbol.
    `provide` returns feature.

    If provided, subfeatures should be a list of symbols indicating
    a set of specific subfeatures provided by this version of
    feature. You can test the presence of a subfeature using
    `featurep`. The idea of subfeatures is that you use them when a
    package (which is one feature) is complex enough to make it
    useful to give names to various parts or functionalities of the
    package, which might or might not be loaded, or might or might not be
    present in a given version. See [Testing Availability of Network Features](#Network-Feature-Testing), for
    an example.

    ```
    features
         ⇒ (bar bish)

    (provide 'foo)
         ⇒ foo
    features
         ⇒ (foo bar bish)
    ```

    When a file is loaded to satisfy an autoload, and it stops due to an
    error in the evaluation of its contents, any function definitions or
    `provide` calls that occurred during the load are undone.
    See [Autoload](#Autoload).

Function: **require** feature &optional filename noerror [¶](#index-require)
:   This function checks whether feature is present in the current
    Emacs session (using `(featurep feature)`; see below). The
    argument feature must be a symbol.

    If the feature is not present, then `require` loads filename
    with `load`. If filename is not supplied, then the name of
    the symbol feature is used as the base file name to load.
    However, in this case, `require` insists on finding feature
    with an added ‘`.el`’ or ‘`.elc`’ suffix (possibly extended with
    a compression suffix); a file whose name is just feature won’t
    be used. (The variable `load-suffixes` specifies the exact
    required Lisp suffixes.)

    If noerror is non-`nil`, that suppresses errors from actual
    loading of the file. In that case, `require` returns `nil`
    if loading the file fails. Normally, `require` returns
    feature.

    If loading the file succeeds but does not provide feature,
    `require` signals an error about the missing feature.

Function: **require-with-check** feature &optional filename noerror [¶](#index-require_002dwith_002dcheck)
:   This function works like `require`, except if feature is
    already loaded (i.e. is already a member of the list in
    `features`, see below). If feature is already loaded, this
    function checks if feature was provided by a file different from
    filename, and if so, it by default signals an error. If the
    value of the optional argument noerror is `reload`, the
    function doesn’t signal an error, but instead forcibly reloads
    filename; if noerror is some other non-`nil` value,
    the function emits a warning about feature being already
    provided by another file.

Function: **featurep** feature &optional subfeature [¶](#index-featurep)
:   This function returns `t` if feature has been provided in
    the current Emacs session (i.e., if feature is a member of
    `features`.) If subfeature is non-`nil`, then the
    function returns `t` only if that subfeature is provided as well
    (i.e., if subfeature is a member of the `subfeature`
    property of the feature symbol.)

Variable: **features** [¶](#index-features-1)
:   The value of this variable is a list of symbols that are the features
    loaded in the current Emacs session. Each symbol was put in this list
    with a call to `provide`. The order of the elements in the
    `features` list is not significant.

The `use-package` macro provides a convenient way of loading a
feature and configuring it for use. It provides a means to combine
requiring a feature, like `require` does, with code to be run
when the feature is actually loaded, similar to load-time hooks
(see [Hooks for Loading](#Hooks-for-Loading)). The declarative syntax of
`use-package` makes it exceptionally easy to use in user init
files.

Macro: **use-package** feature &rest args [¶](#index-use_002dpackage)
:   This macro specifies how to load the named feature and how to
    configure and customize it for use. The arguments args are
    keyword-value pairs. Some of the important keywords and their values
    are:

    `:init forms`
    :   Specifies forms to execute before feature is loaded.

    `:config forms`
    :   Specifies forms to execute after loading feature.

    `:defer condition`
    :   If condition is non-`nil`, it specifies to defer loading
        feature until any of the autoloaded commands or variables of
        feature are first used. If condition is a number n,
        it specifies that feature should be loaded after n
        seconds of idle time.

    `:commands commands…`
    :   Specifies commands of feature to be autoloaded.

    `:bind keybindings…`
    :   Specifies the keybindings for features commands. Each
        binding has the form

        ```
        (key-sequence . command)
        ```

        or

        ```
        (:map keymap (key-sequence . command))
        ```

        where key-sequence is in the form accepted by the `kbd`
        macro (see [Key Sequences](#Key-Sequences)).

    For more details about `use-package`, see [use-package User Manual](https://www.gnu.org/software/emacs/manual/html_mono/use-package.html#Top).

---

Next: [Unloading](#Unloading), Previous: [Features](#Named-Features), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.8 Which File Defined a Certain Symbol [¶](#Which-File-Defined-a-Certain-Symbol)

Function: **symbol-file** symbol &optional type native-p [¶](#index-symbol_002dfile)
:   This function returns the name of the file that defined symbol.
    If type is `nil`, then any kind of definition is acceptable.
    If type is `defun`, `defvar`, or `defface`, that
    specifies function definition, variable definition, or face definition
    only.

    The value is normally an absolute file name. It can also be `nil`,
    if the definition is not associated with any file. If symbol
    specifies an autoloaded function, the value can be a relative file name
    without extension.

    If the optional third argument native-p is non-`nil`, and
    Emacs was built with native compilation support (see [Compilation of Lisp to Native Code](#Native-Compilation)), this function will try to find the `.eln` file
    that defined symbol, instead of the `.elc` or `.el`
    file. If such a `.eln` file is found and is not outdated, the
    function will return its absolute file name; otherwise it will report
    the name of either the source or the byte-compiled file.

The basis for `symbol-file` is the data in the variable
`load-history`.

Variable: **load-history** [¶](#index-load_002dhistory)
:   The value of this variable is an alist that associates the names of
    loaded library files with the names of the functions and variables
    they defined, as well as the features they provided or required.

    Each element in this alist describes one loaded library (including
    libraries that are preloaded at startup). It is a list whose CAR
    is the absolute file name of the library (a string). The rest of the
    list elements have these forms:

    `var`
    :   The symbol var was defined as a variable.

    `(defun . fun)`
    :   The function fun was defined.
        `(defun . fun)`, which represents defining fun as a
        function.

    `(defface . face)`
    :   The face face was defined.

    `(require . feature)`
    :   The feature feature was required.

    `(provide . feature)`
    :   The feature feature was provided.

    `(cl-defmethod method specializers)`
    :   The named method was defined by using `cl-defmethod`, with
        specializers as its specializers.

    `(define-type . type)`
    :   The type type was defined.

    The value of `load-history` may have one element whose CAR is
    `nil`. This element describes definitions made with
    `eval-buffer` on a buffer that is not visiting a file.

The command `eval-region` updates `load-history`, but does so
by adding the symbols defined to the element for the file being visited,
rather than replacing that element. See [Eval](#Eval).

In addition to `load-history`, every function keeps track of its
own history in the symbol property `function-history`.
The reason why functions are treated specially in this respect is that
it is common for functions to be defined in two steps in two different
files (typically, one of them is an autoload), so in order to be
able to properly *unload* a file, we need to know more precisely
what that file did to the function definition.

The symbol property `function-history` holds a list of the form
`(file1 def2 file2 def3 ...)`, where
file1 is the last file that changed the definition and
def2 was the definition before file1, set by file2,
etc. Logically this list should end with the name of the first file
that defined this function, but to save space this last element
is usually omitted.

---

Next: [Hooks for Loading](#Hooks-for-Loading), Previous: [Which File Defined a Certain Symbol](#Where-Defined), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.9 Unloading [¶](#Unloading-1)

You can discard the functions and variables loaded by a library to
reclaim memory for other Lisp objects. To do this, use the function
`unload-feature`:

Command: **unload-feature** feature &optional force [¶](#index-unload_002dfeature)
:   This command unloads the library that provided feature feature.
    It undefines all functions, macros, and variables defined in that
    library with `defun`, `defalias`, `defsubst`,
    `defmacro`, `defconst`, `defvar`, and `defcustom`.
    It then restores any autoloads formerly associated with those symbols.
    (Loading saves these in the `function-history` property of the symbol.)

    Before restoring the previous definitions, `unload-feature` runs
    `remove-hook` to remove functions defined by the library from certain
    hooks. These hooks include variables whose names end in ‘`-hook`’
    (or the deprecated suffix ‘`-hooks`’), plus those listed in
    `unload-feature-special-hooks`, as well as
    `auto-mode-alist`. This is to prevent Emacs from ceasing to
    function because important hooks refer to functions that are no longer
    defined.

    Standard unloading activities also undo ELP profiling of functions
    in that library, unprovides any features provided by the library, and
    cancels timers held in variables defined by the library.

    If these measures are not sufficient to prevent malfunction, a library
    can define an explicit unloader named `feature-unload-function`.
    If that symbol is defined as a function, `unload-feature` calls
    it with no arguments before doing anything else. It can do whatever
    is appropriate to unload the library. If it returns `nil`,
    `unload-feature` proceeds to take the normal unload actions.
    Otherwise it considers the job to be done.

    Ordinarily, `unload-feature` refuses to unload a library on which
    other loaded libraries depend. (A library a depends on library
    b if a contains a `require` for b.) If the
    optional argument force is non-`nil`, dependencies are
    ignored and you can unload any library.

The `unload-feature` function is written in Lisp; its actions are
based on the variable `load-history`.

Variable: **unload-feature-special-hooks** [¶](#index-unload_002dfeature_002dspecial_002dhooks)
:   This variable holds a list of hooks to be scanned before unloading a
    library, to remove functions defined in the library.

---

Next: [Emacs Dynamic Modules](#Dynamic-Modules), Previous: [Unloading](#Unloading), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.10 Hooks for Loading [¶](#Hooks-for-Loading-1)

You can ask for code to be executed each time Emacs loads a library,
by using the variable `after-load-functions`:

Variable: **after-load-functions** [¶](#index-after_002dload_002dfunctions)
:   This abnormal hook is run after loading a file. Each function in the
    hook is called with a single argument, the absolute filename of the
    file that was just loaded.

If you want code to be executed when a *particular* library is
loaded, use the macro `with-eval-after-load`:

Macro: **with-eval-after-load** library body… [¶](#index-with_002deval_002dafter_002dload)
:   This macro arranges to evaluate body at the end of loading
    the file library, each time library is loaded. If
    library is already loaded, it evaluates body right away.

    Usually library is a feature (i.e., a symbol), in which case
    body is evaluated at the end of any file where
    `(provide library)` is called.

    library can be also a file name (i.e., a string).
    You don’t need to give a directory or extension in the file name
    library. Normally, you just give a bare file name, like this:

    ```
    (with-eval-after-load "js" (keymap-set js-mode-map "C-c C-c" 'js-eval))
    ```

    To restrict which files can trigger the evaluation, include a
    directory or an extension or both in library. Only a file whose
    absolute name matches all the given name components will match.
    In the following
    example, `my_inst.elc` or `my_inst.elc.gz` in some directory
    `..../foo/bar` will trigger the evaluation, but not
    `my_inst.el`:

    ```
    (with-eval-after-load "foo/bar/my_inst.elc" ...)
    ```

    An error in body does not undo the load, but does prevent
    execution of the rest of body.

Normally, well-designed Lisp programs should not use
`with-eval-after-load`. If you need to examine and set the
variables defined in another library (those meant for outside use),
you can do it immediately—there is no need to wait until the library
is loaded. If you need to call functions defined by that library, you
should load the library, preferably with `require` (see [Features](#Named-Features)).

---

Previous: [Hooks for Loading](#Hooks-for-Loading), Up: [Loading](#Loading)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 16.11 Emacs Dynamic Modules [¶](#Emacs-Dynamic-Modules)

A *dynamic Emacs module* is a shared library that provides
additional functionality for use in Emacs Lisp programs, just like a
package written in Emacs Lisp would.

Functions that load Emacs Lisp packages can also load dynamic
modules. They recognize dynamic modules by looking at their file-name
extension, a.k.a. “suffix”. This suffix is platform-dependent.

Variable: **module-file-suffix** [¶](#index-module_002dfile_002dsuffix)
:   This variable holds the system-dependent value of the file-name
    extension of the module files. Its value is `.so` on POSIX
    hosts, `.dylib` on macOS, and `.dll` on MS-Windows.

On macOS, dynamic modules can also have the suffix `.so` in
addition to `.dylib`.

Every dynamic module should export a C-callable function named
`emacs_module_init`, which Emacs will call as part of the call to
`load` or `require` which loads the module. It should also
export a symbol named `plugin_is_GPL_compatible` to indicate that
its code is released under the GPL or compatible license; Emacs will
signal an error if your program tries to load modules that don’t
export such a symbol.

If a module needs to call Emacs functions, it should do so through the
API (Application Programming Interface) defined and
documented in the header file `emacs-module.h` that is part of
the Emacs distribution. See [Writing Dynamically-Loaded Modules](#Writing-Dynamic-Modules), for details
of using that API when writing your own modules.

Modules can create `user-ptr` Lisp objects that embed pointers to
C struct’s defined by the module. This is useful for keeping around
complex data structures created by a module, to be passed back to the
module’s functions. User-ptr objects can also have associated
*finalizers* – functions to be run when the object is GC’ed; this
is useful for freeing any resources allocated for the underlying data
structure, such as memory, open file descriptors, etc. See [Conversion Between Lisp and Module Values](#Module-Values).

Function: **user-ptrp** object [¶](#index-user_002dptrp)
:   This function returns `t` if its argument is a `user-ptr`
    object.

Function: **module-load** file [¶](#index-module_002dload)
:   Emacs calls this low-level primitive to load a module from the
    specified file and perform the necessary initialization of the
    module. This is the primitive which makes sure the module exports the
    `plugin_is_GPL_compatible` symbol, calls the module’s
    `emacs_module_init` function, and signals an error if that
    function returns an error indication, or if the user typed `C-g`
    during the initialization. If the initialization succeeds,
    `module-load` returns `t`. Note that file must
    already have the proper file-name extension, as this function doesn’t
    try looking for files with known extensions, unlike `load`.

    Unlike `load`, `module-load` doesn’t record the module in
    `load-history`, doesn’t print any messages, and doesn’t protect
    against recursive loads. Most users should therefore use `load`,
    `load-file`, `load-library`, or `require` instead of
    `module-load`.

Loadable modules in Emacs are enabled by using the
`--with-modules` option at configure time.

---

Next: [Compilation of Lisp to Native Code](#Native-Compilation), Previous: [Loading](#Loading), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 17 Byte Compilation [¶](#Byte-Compilation-1)

Emacs Lisp has a *compiler* that translates functions written
in Lisp into a special representation called *byte-code* that can be
executed more efficiently. The compiler replaces Lisp function
definitions with byte-code. When a byte-code function is called, its
definition is evaluated by the *byte-code interpreter*.

Because the byte-compiled code is evaluated by the byte-code
interpreter, instead of being executed directly by the machine’s
hardware (as true compiled code is), byte-code is completely
transportable from machine to machine without recompilation. It is not,
however, as fast as true compiled code.

In general, any version of Emacs can run byte-compiled code produced
by recent earlier versions of Emacs, but the reverse is not true.

If you do not want a Lisp file to be compiled, ever, put a file-local
variable binding for `no-byte-compile` into it, like this:

```
;; -*-no-byte-compile: t; -*-
```

* [Performance of Byte-Compiled Code](#Speed-of-Byte_002dCode)
* [Byte-Compilation Functions](#Compilation-Functions)
* [Documentation Strings and Compilation](#Docs-and-Compilation)
* [Evaluation During Compilation](#Eval-During-Compile)
* [Compiler Errors](#Compiler-Errors)
* [Closure Function Objects](#Closure-Objects)
* [Disassembled Byte-Code](#Disassembly)

---

Next: [Byte-Compilation Functions](#Compilation-Functions), Up: [Byte Compilation](#Byte-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.1 Performance of Byte-Compiled Code [¶](#Performance-of-Byte_002dCompiled-Code)

A byte-compiled function is not as efficient as a primitive function
written in C, but runs much faster than the version written in Lisp.
Here is an example:

```
(defun silly-loop (n)
  "Return the time, in seconds, to run N iterations of a loop."
  (let ((t1 (float-time)))
    (while (> (setq n (1- n)) 0))
    (- (float-time) t1)))
⇒ silly-loop
```

```

```

```
(silly-loop 50000000)
⇒ 5.200886011123657
```

```

```

```
(byte-compile 'silly-loop)
⇒ [Compiled code not shown]
```

```

```

```
(silly-loop 50000000)
⇒ 0.6239290237426758
```

In this example, the interpreted code required more than 5 seconds to run,
whereas the byte-compiled code required less than 1 second. These
results are representative, but actual results may vary.

---

Next: [Documentation Strings and Compilation](#Docs-and-Compilation), Previous: [Performance of Byte-Compiled Code](#Speed-of-Byte_002dCode), Up: [Byte Compilation](#Byte-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 17.2 Byte-Compilation Functions [¶](#Byte_002dCompilation-Functions)

You can byte-compile an individual function or macro definition with
the `byte-compile` function. You can compile a whole file with
`byte-compile-file`, or several files with
`byte-recompile-directory` or `batch-byte-compile`.

Sometimes, the byte compiler produces warning and/or error messages
(see [Compiler Errors](#Compiler-Errors), for details). These messages are normally
recorded in a buffer called `*Compile-Log*`, which uses
Compilation mode. See [Compilation Mode](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Compilation-Mode) in The GNU Emacs
Manual. However, if the variable `byte-compile-debug` is
non-`nil`, error messages will be signaled as Lisp errors instead
(see [Errors](#Errors)).

Be careful when writing macro calls in files that you intend to
byte-compile. Since macro calls are expanded when they are compiled,
the macros need to be loaded into Emacs or the byte compiler will not
do the right thing. The usual way to handle this is with
`require` forms which specify the files containing the needed
macro definitions (see [Features](#Named-Features)). Normally, the
byte compiler does not evaluate the code that it is compiling, but it
handles `require` forms specially, by loading the specified
libraries. To avoid loading the macro definition files when someone
*runs* the compiled program, write `eval-when-compile`
around the `require` calls (see [Evaluation During Compilation](#Eval-During-Compile)). For
more details, See [Macros and Byte Compilation](#Compiling-Macros).

Inline (`defsubst`) functions are less troublesome; if you
compile a call to such a function before its definition is known, the
call will still work right, it will just run slower.

Function: **byte-compile** symbol [¶](#index-byte_002dcompile)
:   This function byte-compiles the function definition of symbol,
    replacing the previous definition with the compiled one. The function
    definition of symbol must be the actual code for the function;
    `byte-compile` does not handle function indirection. The return
    value is the byte-code function object which is the compiled
    definition of symbol (see [Closure Function Objects](#Closure-Objects)).

    ```
    (defun factorial (integer)
      "Compute factorial of INTEGER."
      (if (= 1 integer) 1
        (* integer (factorial (1- integer)))))
    ⇒ factorial
    ```

    ```

    ```

    ```
    (byte-compile 'factorial)
    ⇒
    #[257
      "\211\300U\203^H^@\300\207\211\301^BS!_\207"
      [1 factorial] 4
      "Compute factorial of INTEGER.\n\n(fn INTEGER)"]
    ```

    If symbol’s definition is a byte-code function object,
    `byte-compile` does nothing and returns `nil`. It does not
    compile the symbol’s definition again, since the original
    (non-compiled) code has already been replaced in the symbol’s function
    cell by the byte-compiled code.

    The argument to `byte-compile` can also be a `lambda`
    expression. In that case, the function returns the corresponding
    compiled code but does not store it anywhere.

Command: **compile-defun** &optional arg [¶](#index-compile_002ddefun)
:   This command reads the defun containing point, compiles it, and
    evaluates the result. If you use this on a defun that is actually a
    function definition, the effect is to install a compiled version of that
    function.

    `compile-defun` normally displays the result of evaluation in the
    echo area, but if arg is non-`nil`, it inserts the result
    in the current buffer after the form it has compiled.

Command: **byte-compile-file** filename [¶](#index-byte_002dcompile_002dfile)
:   This function compiles a file of Lisp code named filename into a
    file of byte-code. The output file’s name is made by changing the
    ‘`.el`’ suffix into ‘`.elc`’; if filename does not end in
    ‘`.el`’, it adds ‘`.elc`’ to the end of filename.

    Compilation works by reading the input file one form at a time. If it
    is a definition of a function or macro, the compiled function or macro
    definition is written out. Other forms are batched together, then each
    batch is compiled, and written so that its compiled code will be
    executed when the file is read. All comments are discarded when the
    input file is read.

    This command returns `t` if there were no errors and `nil`
    otherwise. When called interactively, it prompts for the file name.

    ```
    $ ls -l push*
    -rw-r--r-- 1 lewis lewis 791 Oct  5 20:31 push.el
    ```

    ```

    ```

    ```
    (byte-compile-file "~/emacs/push.el")
         ⇒ t
    ```

    ```

    ```

    ```
    $ ls -l push*
    -rw-r--r-- 1 lewis lewis 791 Oct  5 20:31 push.el
    -rw-rw-rw- 1 lewis lewis 638 Oct  8 20:25 push.elc
    ```

Command: **byte-recompile-directory** directory &optional flag force follow-symlinks [¶](#index-byte_002drecompile_002ddirectory)
:   This command recompiles every ‘`.el`’ file in directory (or
    its subdirectories) that needs recompilation. A file needs
    recompilation if a ‘`.elc`’ file exists but is older than the
    ‘`.el`’ file.

    When a ‘`.el`’ file has no corresponding ‘`.elc`’ file,
    flag says what to do. If it is `nil`, this command ignores
    these files. If flag is 0, it compiles them. If it is neither
    `nil` nor 0, it asks the user whether to compile each such file,
    and asks about each subdirectory as well.

    Interactively, `byte-recompile-directory` prompts for
    directory and flag is the prefix argument.

    If force is non-`nil`, this command recompiles every
    ‘`.el`’ file that has a ‘`.elc`’ file.

    This command will normally not compile ‘`.el`’ files that are
    symlinked. If the optional follow-symlink parameter is
    non-`nil`, symlinked ‘`.el`’ will also be compiled.

    The returned value is unpredictable.

Function: **batch-byte-compile** &optional noforce [¶](#index-batch_002dbyte_002dcompile)
:   This function runs `byte-compile-file` on files specified on the
    command line. This function must be used only in a batch execution of
    Emacs, as it kills Emacs on completion. An error in one file does not
    prevent processing of subsequent files, but no output file will be
    generated for it, and the Emacs process will terminate with a nonzero
    status code.

    If noforce is non-`nil`, this function does not recompile
    files that have an up-to-date ‘`.elc`’ file.

    ```
    $ emacs -batch -f batch-byte-compile *.el
    ```

---

Next: [Evaluation During Compilation](#Eval-During-Compile), Previous: [Byte-Compilation Functions](#Compilation-Functions), Up: [Byte Compilation](#Byte-Compilation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
