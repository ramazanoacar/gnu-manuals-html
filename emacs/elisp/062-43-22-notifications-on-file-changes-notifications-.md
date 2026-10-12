### 43.22 Notifications on File Changes [¶](#Notifications-on-File-Changes)

Several operating systems support watching of filesystems for changes
to files or their attributes. If configured properly, Emacs links a
respective library like `inotify`, `kqueue`,
`gfilenotify`, or `w32notify` statically. These libraries
enable watching of filesystems on the local machine.

It is also possible to watch filesystems on remote machines,
see [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual.
This does not depend on one of the libraries linked to Emacs.

Since all these libraries emit different events upon notified file
changes, Emacs provides a special library `filenotify` which
presents a unified interface to applications. Lisp programs that want
to receive file notifications should always use this library in
preference to the native ones. This section documents the
`filenotify` library functions and variables.

Function: **file-notify-add-watch** file flags callback [¶](#index-file_002dnotify_002dadd_002dwatch)
:   Add a watch for filesystem events pertaining to file. This
    arranges for filesystem events pertaining to file to be reported
    to Emacs.

    The returned value is a descriptor for the added watch. Its type
    depends on the underlying library, and in general cannot be assumed to
    be an integer as in the example below. It should be used for
    comparison by `equal` only.

    If the file cannot be watched for some reason, this function
    signals a `file-notify-error` error.

    Sometimes, mounted filesystems cannot be watched for file changes.
    This is not detected by this function, and so a non-`nil` return
    value does not guarantee that changes on file will be actually
    notified.

    If file is a symlink, it doesn’t follow that link. Just
    file itself will be watched.

    flags is a list of conditions to set what will be watched for.
    It can include the following symbols:

    `change`
    :   watch for changes in file’s contents

    `attribute-change`
    :   watch for changes in file attributes, like permissions or modification
        time

    If file is a directory, `change` watches for file creation
    and deletion in that directory. Some of the native file notification
    libraries also report file changes in that case. This does not work
    recursively.

    When any event happens, Emacs will call the callback function
    passing it a single argument event, which is of the form

    ```
    (descriptor action file [file1])
    ```

    descriptor is the same object as the one returned by this
    function. action is the description of the event. It could be
    any one of the following symbols:

    `created`
    :   file was created

    `deleted`
    :   file was deleted

    `changed`
    :   file’s contents has changed; with `w32notify` library,
        reports attribute changes as well

    `renamed`
    :   file has been renamed to file1

    `attribute-changed`
    :   a file attribute was changed

    `stopped`
    :   watching file has stopped

    Note that the `w32notify` library does not report
    `attribute-changed` events. When some file’s attribute, like
    permissions or modification time, has changed, this library reports a
    `changed` event. Likewise, the `kqueue` library does not
    reliably report file attribute changes when watching a directory.

    The `stopped` event means that watching the file has been
    discontinued. This could be because `file-notify-rm-watch` was
    called (see below), or because the file being watched was deleted, or
    because the filesystem of the file being watched was unmounted, or due
    to another error reported from the underlying library which makes
    further watching impossible.

    file and file1 are the name of the file(s) whose event is
    being reported. For example:

    ```
    (require 'filenotify)
         ⇒ filenotify
    ```

    ```

    ```

    ```
    (defun my-notify-callback (event)
      (message "Event %S" event))
         ⇒ my-notify-callback
    ```

    ```

    ```

    ```
    (file-notify-add-watch
      "/tmp" '(change attribute-change) 'my-notify-callback)
         ⇒ 35025468
    ```

    ```

    ```

    ```
    (write-region "foo" nil "/tmp/foo")
         ⇒ Event (35025468 created "/tmp/.#foo")
            Event (35025468 created "/tmp/foo")
            Event (35025468 changed "/tmp/foo")
            Event (35025468 deleted "/tmp/.#foo")
    ```

    ```

    ```

    ```
    (write-region "bla" nil "/tmp/foo")
         ⇒ Event (35025468 created "/tmp/.#foo")
            Event (35025468 changed "/tmp/foo")
            Event (35025468 deleted "/tmp/.#foo")
    ```

    ```

    ```

    ```
    (set-file-modes "/tmp/foo" (default-file-modes) 'nofollow)
         ⇒ Event (35025468 attribute-changed "/tmp/foo")
    ```

    Whether the action `renamed` is returned depends on the used
    watch library. Otherwise, the actions `deleted` and
    `created` could be returned in a random order.

    ```
    (rename-file "/tmp/foo" "/tmp/bla")
         ⇒ Event (35025468 renamed "/tmp/foo" "/tmp/bla")
    ```

    ```

    ```

    ```
    (delete-file "/tmp/bla")
         ⇒ Event (35025468 deleted "/tmp/bla")
    ```

Function: **file-notify-rm-watch** descriptor [¶](#index-file_002dnotify_002drm_002dwatch)
:   Removes an existing file watch specified by its descriptor.
    descriptor should be an object returned by
    `file-notify-add-watch`.

Command: **file-notify-rm-all-watches** [¶](#index-file_002dnotify_002drm_002dall_002dwatches)
:   Removes all existing file notification watches from Emacs.

    Use this command with caution, because it could have unexpected side
    effects on packages relying on file watches. It is intended mainly
    for debugging purposes, or when Emacs has been stalled.

Function: **file-notify-valid-p** descriptor [¶](#index-file_002dnotify_002dvalid_002dp)
:   Checks a watch specified by its descriptor for validity.
    descriptor should be an object returned by
    `file-notify-add-watch`.

    A watch can become invalid if the file or directory it watches is
    deleted, or if the watcher thread exits abnormally for any other
    reason. Removing the watch by calling `file-notify-rm-watch`
    also makes it invalid.

    ```
    (make-directory "/tmp/foo")
         ⇒ Event (35025468 created "/tmp/foo")
    ```

    ```

    ```

    ```
    (setq desc
          (file-notify-add-watch
            "/tmp/foo" '(change) 'my-notify-callback))
         ⇒ 11359632
    ```

    ```

    ```

    ```
    (file-notify-valid-p desc)
         ⇒ t
    ```

    ```

    ```

    ```
    (write-region "bla" nil "/tmp/foo/bla")
         ⇒ Event (11359632 created "/tmp/foo/.#bla")
            Event (11359632 created "/tmp/foo/bla")
            Event (11359632 changed "/tmp/foo/bla")
            Event (11359632 deleted "/tmp/foo/.#bla")
    ```

    ```

    ```

    ```
    ;; Deleting a file in the directory doesn't invalidate the watch.
    (delete-file "/tmp/foo/bla")
         ⇒ Event (11359632 deleted "/tmp/foo/bla")
    ```

    ```

    ```

    ```
    (write-region "bla" nil "/tmp/foo/bla")
         ⇒ Event (11359632 created "/tmp/foo/.#bla")
            Event (11359632 created "/tmp/foo/bla")
            Event (11359632 changed "/tmp/foo/bla")
            Event (11359632 deleted "/tmp/foo/.#bla")
    ```

    ```

    ```

    ```
    ;; Deleting the directory invalidates the watch.
    ;; Events arrive for different watch descriptors.
    (delete-directory "/tmp/foo" 'recursive)
         ⇒ Event (35025468 deleted "/tmp/foo")
            Event (11359632 deleted "/tmp/foo/bla")
            Event (11359632 deleted "/tmp/foo")
            Event (11359632 stopped "/tmp/foo")
    ```

    ```

    ```

    ```
    (file-notify-valid-p desc)
         ⇒ nil
    ```

---

Next: [Security Considerations](#Security-Considerations), Previous: [Notifications on File Changes](#File-Notifications), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.23 Dynamically Loaded Libraries [¶](#Dynamically-Loaded-Libraries)

A *dynamically loaded library* is a library that is loaded on
demand, when its facilities are first needed. Emacs supports such
on-demand loading of support libraries for some of its features.

Variable: **dynamic-library-alist** [¶](#index-dynamic_002dlibrary_002dalist)
:   This is an alist of dynamic libraries and external library files
    implementing them.

    Each element is a list of the form
    `(library files…)`, where the `car` is
    a symbol representing a supported external library, and the rest are
    strings giving alternate filenames for that library.

    Emacs tries to load the library from the files in the order they
    appear in the list; if none is found, the Emacs session won’t have
    access to that library, and the features it provides will be
    unavailable.

    Image support on some platforms uses this facility. Here’s an example
    of setting this variable for supporting images on MS-Windows:

    ```
    (setq dynamic-library-alist
          '((xpm "libxpm.dll" "xpm4.dll" "libXpm-nox4.dll")
            (png "libpng12d.dll" "libpng12.dll" "libpng.dll"
                 "libpng13d.dll" "libpng13.dll")
            (jpeg "jpeg62.dll" "libjpeg.dll" "jpeg-62.dll"
                  "jpeg.dll")
            (tiff "libtiff3.dll" "libtiff.dll")
            (gif "giflib4.dll" "libungif4.dll" "libungif.dll")
            (svg "librsvg-2-2.dll")
            (gdk-pixbuf "libgdk_pixbuf-2.0-0.dll")
            (glib "libglib-2.0-0.dll")
            (gobject "libgobject-2.0-0.dll")))
    ```

    Note that image types `pbm` and `xbm` do not need entries in
    this variable because they do not depend on external libraries and are
    always available in Emacs.

    Also note that this variable is not meant to be a generic facility for
    accessing external libraries; only those already known by Emacs can
    be loaded through it.

    This variable is ignored if the given library is statically
    linked into Emacs.

---

Previous: [Dynamically Loaded Libraries](#Dynamic-Libraries), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.24 Security Considerations [¶](#Security-Considerations-1)

Like any application, Emacs can be run in a secure environment, where
the operating system enforces rules about access and the like. With
some care, Emacs-based applications can also be part of a security
perimeter that checks such rules. Although the default settings for
Emacs work well for a typical software development environment, they
may require adjustment in environments containing untrusted users that
may include attackers. Here is a compendium of security issues that
may be helpful if you are developing such applications. It is by no
means complete; it is intended to give you an idea of the security
issues involved, rather than to be a security checklist.

File local variables [¶](#index-file-local-variables-1)
:   A file that Emacs visits can contain variable settings that affect
    the buffer visiting that file; See [File Local Variables](#File-Local-Variables).
    Similarly, a directory can specify local variable values common to all
    files in that directory; see [Directory Local Variables](#Directory-Local-Variables). Although
    Emacs takes some effort to protect against misuse of these variables,
    a security hole can be created merely by a package setting
    `safe-local-variable` too optimistically, a problem that is all
    too common. To disable this feature for both files and directories,
    set `enable-local-variables` to `nil`.

Access control
:   Although Emacs normally respects access permissions of the underlying
    operating system, in some cases it handles accesses specially. For
    example, file names can have handlers that treat the files specially,
    with their own access checking. See [Making Certain File Names “Magic”](#Magic-File-Names). Also, a
    buffer can be read-only even if the corresponding file is writable,
    and vice versa, which can result in messages such as ‘`File passwd
    is write-protected; try to save anyway? (yes or no)`’. See [Read-Only Buffers](#Read-Only-Buffers).

Authentication
:   Emacs has several functions that deal with passwords, e.g.,
    `read-passwd`. See [Reading a Password](#Reading-a-Password).
    Although these functions do not attempt to
    broadcast passwords to the world, their implementations are not proof
    against determined attackers with access to Emacs internals. For
    example, even if Elisp code uses `clear-string` to scrub a password from
    its memory after using it, remnants of the password may still reside
    in the garbage-collected free list. See [Modifying Strings](#Modifying-Strings).

Code injection
:   Emacs can send commands to many other applications, and applications
    should take care that strings sent as operands of these commands are
    not misinterpreted as directives. For example, when using a shell
    command to rename a file a to b, do not simply use the
    string `mv a b`, because either file name might start
    with ‘`-`’, or might contain shell metacharacters like ‘`;`’.
    Although functions like `shell-quote-argument` can help avoid
    this sort of problem, they are not panaceas; for example, on a POSIX
    platform `shell-quote-argument` quotes shell metacharacters but
    not leading ‘`-`’. On MS-Windows, quoting for ‘`%`’ assumes
    none of the environment variables have ‘`^`’ in their name.
    See [Shell Arguments](#Shell-Arguments). Typically it is safer
    to use `call-process` than a subshell. See [Creating a Synchronous Process](#Synchronous-Processes). And it is safer yet to use builtin Emacs functions; for
    example, use `(rename-file "a" "b" t)` instead of
    invoking `mv`. See [Changing File Names and Attributes](#Changing-Files).

Coding systems
:   Emacs attempts to infer the coding systems of the files and network
    connections it accesses. See [Coding Systems](#Coding-Systems).
    If Emacs infers incorrectly, or if the other
    parties to the network connection disagree with Emacs’s inferences,
    the resulting system could be unreliable. Also, even when it infers
    correctly, Emacs often can use bytes that other programs cannot. For
    example, although to Emacs the null byte is just a
    character like any other, many other applications treat it as a string
    terminator and mishandle strings or files containing null bytes.

Environment and configuration variables
:   POSIX specifies several environment variables that can affect how
    Emacs behaves. Any environment variable whose name consists entirely
    of uppercase ASCII letters, digits, and the underscore may affect the
    internal behavior of Emacs. Emacs uses several such variables, e.g.,
    `EMACSLOADPATH`. See [Library Search](#Library-Search). On some platforms some
    environment variables (e.g., `PATH`, `POSIXLY_CORRECT`,
    `SHELL`, `TMPDIR`) need to have properly-configured values in
    order to get standard behavior for any utility Emacs might invoke.
    Even seemingly-benign variables like `TZ` may have security
    implications. See [Operating System Environment](#System-Environment).

    Emacs has customization and other variables with similar
    considerations. For example, if the variable `shell-file-name`
    specifies a shell with nonstandard behavior, an Emacs-based
    application may misbehave.

Installation
:   When Emacs is installed, if the installation directory hierarchy can
    be modified by untrusted users, the application cannot be trusted.
    This applies also to the directory hierarchies of the programs that
    Emacs uses, and of the files that Emacs reads and writes.

Network access
:   Emacs often accesses the network, and you may want to configure it to
    avoid network accesses that it would normally do. For example, unless
    you set `tramp-mode` to `nil`, file names using a certain
    syntax are interpreted as being network files, and are retrieved
    across the network. See [The Tramp Manual](https://www.gnu.org/software/emacs/manual/html_mono/tramp.html#Top) in The Tramp
    Manual.

Race conditions
:   Emacs applications have the same sort of race-condition issues that
    other applications do. For example, even when
    `(file-readable-p "foo.txt")` returns `t`, it could be that
    `foo.txt` is unreadable because some other program changed the
    file’s permissions between the call to `file-readable-p` and now.
    See [Testing Accessibility](#Testing-Accessibility).

Resource limits
:   When Emacs exhausts memory or other operating system resources, its
    behavior can be less reliable, in that computations that ordinarily
    run to completion may abort back to the top level. This may cause
    Emacs to neglect operations that it normally would have done.

---

Next: [Emacs 30 Antinews](#Antinews), Previous: [Operating System Interface](#System-Interface), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 44 Preparing Lisp code for distribution [¶](#Preparing-Lisp-code-for-distribution)

Emacs provides a standard way to distribute Emacs Lisp code to
users. A *package* is a collection of one or more files,
formatted and bundled in such a way that users can easily download,
install, uninstall, and upgrade it.

The following sections describe how to create a package, and how to
put it in a *package archive* for others to download.
See [Packages](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Packages) in The GNU Emacs Manual, for a description of
user-level features of the packaging system.

These sections are mostly directed towards package archive
maintainers—much of this information is not relevant for package
authors (i.e., people who write code that will be distributed via
these archives).

* [Packaging Basics](#Packaging-Basics)
* [Simple Packages](#Simple-Packages)
* [Multi-file Packages](#Multi_002dfile-Packages)
* [Creating and Maintaining Package Archives](#Package-Archives)
* [Interfacing to an archive web server](#Archive-Web-Server)
* [Supporting older versions of Emacs](#Forwards_002dCompatibility)
* [Get built-in packages and versions](#Package-Information)

---

Next: [Simple Packages](#Simple-Packages), Up: [Preparing Lisp code for distribution](#Packaging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 44.1 Packaging Basics [¶](#Packaging-Basics-1)

A package is either a *simple package* or a *multi-file
package*. A simple package is stored in a package archive as a single
Emacs Lisp file, while a multi-file package is stored as a tar file
(containing multiple Lisp files, and possibly non-Lisp files such as a
manual).

In ordinary usage, the difference between simple packages and
multi-file packages is relatively unimportant; the Package Menu
interface makes no distinction between them. However, the procedure
for creating them differs, as explained in the following sections.

Each package (whether simple or multi-file) has certain
*attributes*:

Name
:   A short word (e.g., ‘`auctex`’). This is usually also the symbol
    prefix used in the program (see [Emacs Lisp Coding Conventions](#Coding-Conventions)).

Version
:   A version number, in a form that the function `version-to-list`
    understands (e.g., ‘`11.86`’). Each release of a package should be
    accompanied by an increase in the version number so that it will be
    recognized as an upgrade by users querying the package archive.

Brief description
:   This is shown when the package is listed in the Package Menu. It
    should occupy a single line, ideally in 36 characters or less.

Long description
:   This is shown in the buffer created by `C-h P`
    (`describe-package`), following the package’s brief description
    and installation status. It normally spans multiple lines, and should
    fully describe the package’s capabilities and how to begin using it
    once it is installed.

Dependencies
:   A list of other packages (possibly including minimal acceptable
    version numbers) on which this package depends. The list may be
    empty, meaning this package has no dependencies. Otherwise,
    installing this package also automatically installs its dependencies,
    recursively; if any dependency cannot be found, the package cannot be
    installed.

Installing a package, either via the command `package-install-file`,
or via the Package Menu, creates a subdirectory of
`package-user-dir` named `name-version`, where
name is the package’s name and version its version
(e.g., `~/.emacs.d/elpa/auctex-11.86/`). We call this the
package’s *content directory*. It is where Emacs puts the
package’s contents (the single Lisp file for a simple package, or the
files extracted from a multi-file package).

Emacs then searches every Lisp file in the content directory for
autoload magic comments (see [Autoload](#Autoload)). These autoload
definitions are saved to a file named `name-autoloads.el`
in the content directory. They are typically used to autoload the
principal user commands defined in the package, but they can also
perform other tasks, such as adding an element to
`auto-mode-alist` (see [How Emacs Chooses a Major Mode](#Auto-Major-Mode)). Note that a package
typically does *not* autoload every function and variable defined
within it—only the handful of commands typically called to begin
using the package. Emacs then byte-compiles every Lisp file in the
package.

After installation, the installed package is *loaded*: Emacs
adds the package’s content directory to `load-path`, and
evaluates the autoload definitions in `name-autoloads.el`.

Whenever Emacs starts up, it automatically calls the function
`package-activate-all` to make installed packages available to the
current session. This is done after loading the early init file, but
before loading the regular init file (see [Summary: Sequence of Actions at Startup](#Startup-Summary)).
Packages are not automatically made available if the user option
`package-enable-at-startup` is set to `nil` in the early
init file.

Function: **package-activate-all** [¶](#index-package_002dactivate_002dall)
:   This function makes the packages available to the current session.
    The user option `package-load-list` specifies which packages to
    make available; by default, all installed packages are made available.
    See [Package Installation](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Package-Installation) in The GNU Emacs Manual.

    In most cases, you should not need to call `package-activate-all`,
    as this is done automatically during startup. Simply make sure to put
    any code that should run before `package-activate-all` in the early
    init file, and any code that should run after it in the primary init
    file (see [Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-File) in The GNU Emacs Manual).

---

Next: [Multi-file Packages](#Multi_002dfile-Packages), Previous: [Packaging Basics](#Packaging-Basics), Up: [Preparing Lisp code for distribution](#Packaging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 44.2 Simple Packages [¶](#Simple-Packages-1)

A simple package consists of a single Emacs Lisp source file. The
file must conform to the Emacs Lisp library header conventions
(see [Conventional Headers for Emacs Libraries](#Library-Headers)). The package’s attributes are taken from
the various headers, as illustrated by the following example:

```
;;; superfrobnicator.el --- Frobnicate and bifurcate flanges  -*- lexical-binding:t -*-

;; Copyright (C) 2022, 2025--2026 Free Software Foundation, Inc.
```

```
;; Author: J. R. Hacker <jrh@example.com>
;; Version: 1.3
;; Package-Requires: ((flange "1.0"))
;; Keywords: multimedia, hypermedia
;; URL: https://example.com/jrhacker/superfrobnicate

...

;;; Commentary:

;; This package provides a minor mode to frobnicate and/or
;; bifurcate any flanges you desire.  To activate it, just type
...

;;;###autoload
(define-minor-mode superfrobnicator-mode
...
```

The name of the package is the same as the base name of the file, as
written on the first line. Here, it is ‘`superfrobnicator`’.

The brief description is also taken from the first line. Here, it
is ‘`Frobnicate and bifurcate flanges`’.

The version number comes from the ‘`Package-Version`’ header, if
it exists, or from the ‘`Version`’ header otherwise. One or the
other *must* be present. Here, the version number is 1.3.

If the file has a ‘`;;; Commentary:`’ section, this section is
used as the long description. (When displaying the description, Emacs
omits the ‘`;;; Commentary:`’ line, as well as the leading comment
characters in the commentary itself.)

If the file has a ‘`Package-Requires`’ header, that is used as the
package dependencies. In the above example, the package depends on the
‘`flange`’ package, version 1.0 or higher. See [Conventional Headers for Emacs Libraries](#Library-Headers),
for a description of the ‘`Package-Requires`’ header. To depend on a
specific version of Emacs, specify ‘`emacs`’ as the package name. If
the header is omitted, the package has no dependencies.

The ‘`Keywords`’ and ‘`URL`’ headers are optional, but recommended.
The command `describe-package` uses these to add links to its
output. The ‘`Keywords`’ header should contain at least one
standard keyword from the `finder-known-keywords` list.

The file ought to also contain one or more autoload magic comments,
as explained in [Packaging Basics](#Packaging-Basics). In the above example, a magic
comment autoloads `superfrobnicator-mode`.

See [Creating and Maintaining Package Archives](#Package-Archives), for an explanation of how to add a
single-file package to a package archive.

---

Next: [Creating and Maintaining Package Archives](#Package-Archives), Previous: [Simple Packages](#Simple-Packages), Up: [Preparing Lisp code for distribution](#Packaging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 44.3 Multi-file Packages [¶](#Multi_002dfile-Packages-1)

A multi-file package is less convenient to create than a single-file
package, but it offers more features: it can include multiple Emacs
Lisp files, an Info manual, and other file types (such as images).

Prior to installation, a multi-file package is stored in a package
archive as a tar file. The tar file must be named
`name-version.tar`, where name is the package
name and version is the version number. Its contents, once
extracted, must all appear in a directory named
`name-version`, the *content directory*
(see [Packaging Basics](#Packaging-Basics)). Files may also extract into
subdirectories of the content directory.

One of the files in the content directory must be named
`name-pkg.el`. It must contain a single Lisp form,
consisting of a call to the function `define-package`, described
below. This defines the package’s attributes: version, brief
description, and requirements.

For example, if we distribute version 1.3 of the superfrobnicator as
a multi-file package, the tar file would be
`superfrobnicator-1.3.tar`. Its contents would extract into the
directory `superfrobnicator-1.3`, and one of these would be the
file `superfrobnicator-pkg.el`.

Function: **define-package** name version &optional docstring requirements [¶](#index-define_002dpackage)
:   This function defines a package. name is the package name, a
    string. version is the version, as a string of a form that can
    be understood by the function `version-to-list`. docstring
    is the brief description.

    requirements is a list of required packages and their versions.
    Each element in this list should have the form `(dep-name
    dep-version)`, where dep-name is a symbol whose name is the
    dependency’s package name, and dep-version is the dependency’s
    version (a string). The special value ‘`emacs`’ means that the
    package depends on the given version of Emacs.

If the content directory contains a file named `README`, this
file is used as the long description (overriding any ‘`;;;
Commentary:`’ section).

If the content directory contains a file named `dir`, this is
assumed to be an Info directory file made with `install-info`.
See [Invoking
install-info](https://www.gnu.org/software/texinfo/manual/texinfo/texinfo.html#Invoking-install_002dinfo) in Texinfo. The relevant Info files should also
be present in the content directory. In this case, Emacs will
automatically add the content directory to `Info-directory-list`
when the package is activated.

Do not include any `.elc` files in the package. Those are
created when the package is installed. Note that there is no way to
control the order in which files are byte-compiled.

Do not include any file named `name-autoloads.el`. This
file is reserved for the package’s autoload definitions
(see [Packaging Basics](#Packaging-Basics)). It is created automatically when the
package is installed, by searching all the Lisp files in the package
for autoload magic comments.

If the multi-file package contains auxiliary data files (such as
images), the package’s Lisp code can refer to these files via the
variable `load-file-name` (see [Loading](#Loading)). Here is an example:

```
(defconst superfrobnicator-base (file-name-directory load-file-name))

(defun superfrobnicator-fetch-image (file)
  (expand-file-name file superfrobnicator-base))
```

If your package contains files that you don’t wish to distribute to
users (e.g. regression tests), you can add them to an
`.elpaignore` file. In this file, each line lists a file or a
wildcard matching files; those files should be ignored when producing
your package’s tarball on ELPA (see [Creating and Maintaining Package Archives](#Package-Archives)). (ELPA
will pass this file to the `tar` command via the `-X`
command-line option, when it prepares the package for download.)

---

Next: [Interfacing to an archive web server](#Archive-Web-Server), Previous: [Multi-file Packages](#Multi_002dfile-Packages), Up: [Preparing Lisp code for distribution](#Packaging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 44.4 Creating and Maintaining Package Archives [¶](#Creating-and-Maintaining-Package-Archives)

Via the Package Menu, users may download packages from *package
archives*. Such archives are specified by the variable
`package-archives`, whose default value lists the archives
hosted on [GNU ELPA](https://elpa.gnu.org) and
[NonGNU ELPA](https://elpa.nongnu.org). This section describes
how to set up and maintain a package archive.

A package archive is simply a directory in which the package files,
and associated files, are stored. If you want the archive to be
reachable via HTTP, this directory must be accessible to a web server;
See [Interfacing to an archive web server](#Archive-Web-Server).

After you create an archive, remember that it is not accessible in the
Package Menu interface unless it is in `package-archives`.

Maintaining a public package archive entails a degree of responsibility.
When Emacs users install packages from your archive, those packages
can cause Emacs to run arbitrary code with the permissions of the
installing user. (This is true for Emacs code in general, not just
for packages.) So you should ensure that your archive is
well-maintained and keep the hosting system secure.

One way to increase the security of your packages is to *sign*
them using a cryptographic key. If you have generated a
private/public gpg key pair, you can use gpg to sign the package like
this:

```
gpg -ba -o file.sig file
```

For a single-file package, file is the package Lisp file;
for a multi-file package, it is the package tar file.
You can also sign the archive’s contents file in the same way.
Make the `.sig` files available in the same location as the packages.
You should also make your public key available for people to download;
e.g., by uploading it to a key server such as <https://pgp.mit.edu/>.
When people install packages from your archive, they can use
your public key to verify the signatures.

A full explanation of these matters is outside the scope of this
manual. For more information on cryptographic keys and signing,
see [GnuPG](https://www.gnupg.org/documentation/manuals/gnupg/index.html#Top) in The GNU Privacy Guard Manual. Emacs comes
with an interface to GNU Privacy Guard, see [EasyPG](https://www.gnu.org/software/emacs/manual/html_mono/epa.html#Top) in Emacs EasyPG Assistant Manual.

---

Next: [Supporting older versions of Emacs](#Forwards_002dCompatibility), Previous: [Creating and Maintaining Package Archives](#Package-Archives), Up: [Preparing Lisp code for distribution](#Packaging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 44.5 Interfacing to an archive web server [¶](#Interfacing-to-an-archive-web-server)

A web server providing access to a package archive must support the
following queries:

archive-contents
:   Return a lisp form describing the archive contents. The form is a list
    of ’package-desc’ structures (see `package.el`), except the first
    element of the list is the archive version.

<package name>-readme.txt
:   Return the long description of the package.

<file name>.sig
:   Return the signature for the file.

<file name>
:   Return the file. This will be the tarball for a multi-file
    package, or the single file for a simple package.

---

Next: [Get built-in packages and versions](#Package-Information), Previous: [Interfacing to an archive web server](#Archive-Web-Server), Up: [Preparing Lisp code for distribution](#Packaging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 44.6 Supporting older versions of Emacs [¶](#Supporting-older-versions-of-Emacs)

Packages that wish to support older releases of Emacs, without giving
up on newer functionality from recent Emacs releases, one can make use
of the Compat package on GNU ELPA. By depending on the package, Emacs
can provide compatibility definitions for missing functionality.

The versioning of Compat follows that of Emacs, so next to the oldest
version that a package relies on (via the `emacs`-package), one
can also indicate what the newest version of Emacs is, that a package
wishes to use definitions from:

```
;; Package-Requires: ((emacs "27.2") (compat "29.1"))
```

Note that Compat provides replacement functions with extended
functionality for functions that are already defined (`sort`,
`assoc`, …). These functions may have changed their
calling convention (additional optional arguments) or may have changed
their behavior. These functions must be looked up explicitly with
`compat-function` or called explicitly with `compat-call`.
We call them *Extended Definitions*. In contrast, newly *Added
Definitions* can be called as usual.

Macro: **compat-call** fun &rest args [¶](#index-compat_002dcall)
:   This macro calls the compatibility function fun with args.
    Many functions provided by Compat can be called directly without this
    macro. However in the case where Compat provides an alternative
    version of an existing function, the function call has to go through
    `compat-call`.

Macro: **compat-function** fun [¶](#index-compat_002dfunction)
:   This macro returns the compatibility function symbol for fun.
    See `compat-call` for a more convenient macro to directly call
    compatibility functions.

For further details on how to make use of the package, see
[Usage](compat.html#Usage) in "Compat" Manual. In case you don’t have
the manual installed, you can also read the
[Online
Compat manual](https://elpa.gnu.org/packages/doc/compat.html#Usage).

---

Previous: [Supporting older versions of Emacs](#Forwards_002dCompatibility), Up: [Preparing Lisp code for distribution](#Packaging)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 44.7 Get built-in packages and versions [¶](#Get-built_002din-packages-and-versions)

Emacs ships built-in packages which may also be packaged externally on
ELPA. Knowing whether a built-in package has a higher version than on
ELPA or other packaging systems may be useful for external packaging
tools. Lisp programs which would like to query such information can use
the following functions.

Function: **package-versioned-builtin-packages** [¶](#index-package_002dversioned_002dbuiltin_002dpackages)
:   This function returns the list of packages that are built into Emacs
    and have a version. The returned list contains one symbol for each
    built-in package that has a version, which are likely also packaged
    externally on ELPA.

Function: **package-builtin-package-version** package [¶](#index-package_002dbuiltin_002dpackage_002dversion)
:   This function returns the version of a built-in package.
    package should be a symbol. The returned value is a list of
    integers representing the version of PACKAGE in the format returned by
    the function `version-to-list`, or `nil` if package is
    built-in but has no version, or is not a built-in package.

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [Preparing Lisp code for distribution](#Packaging), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix A Emacs 30 Antinews [¶](#Emacs-30-Antinews)

For those users who live backwards in time, here is information about
downgrading to Emacs version 30.2. We hope you will enjoy the greater
simplicity that results from the absence of many Emacs 31.1.50 features.

* Lisp strings are now mutable. You can use `aset` on unibyte
  strings to replace single bytes by multibyte characters, and `aset`
  on multibyte strings can replace any character by any other character,
  regardless if they are ASCII or multibyte non-ASCII. We feel those
  restrictions are un-Emacsy and get in the way of doing all kinds of
  exciting things with strings.
* Buffers that are current to a non-main thread cannot be killed by other
  threads. We decided to get rid of the numerous complications which were
  needed to allow killing such buffers. If your program needs to kill
  such buffers, you will need to use a disciplined approach of first
  making some other buffer current to the thread.
* We removed the early detection of faces that inherit from themselves.
  As you move back in time, Emacs Lisp programmers become more and more
  proficient, so disallowing such tricks feels wrong. Those who don’t
  know well enough what they are doing will be amply punished by having
  their Lisp programs barf at display time or even wedge Emacs cold.
* Calling `debug` in batch mode now kills Emacs. We decided that
  there’s no sense in continuing a batch session that entered the
  debugger: who and how will debug it, anyway?

  For the same reason, `C-c` in batch mode now always kills Emacs, it
  never quits.
* The functions that access and manipulate error descriptors were deleted.
  Functions like `error-type`, `error-slot-value` and others are
  useless bloat, since Lisp programs can simply access the data structure
  directly, as God intended!
* We removed `frame-initial-p`, as its job can easily be done by
  testing several obscure variables and frame parameters.
* We’ve added the `redisplay-dont-pause` variable, so we could allow
  interrupting a runaway redisplay cycle in one of the past versions of
  Emacs.
* There’s no longer a guarantee that all the digit characters will have
  the `digit` (`?6`) category. We feel that such silly
  consistency robs Lisp programmers of too much fun, whereby they should
  discover which digits have this category by trial-and-error.

  For the same reason, not all characters belonging to the `symbol`
  script will have the `symbol` (`?5`) category.
* Lisp programs can now easily delete that daemon frame by calling
  `delete-frame`, as with any other frame. We decided there’s no
  reason to make a special case for the daemon frames, especially as some
  past Emacs version will remove the daemon feature entirely.
* Lisp files found on `load-path` are no longer cached. We intend to
  make the `load-path` list shorter and shorter as we move to
  previous versions of Emacs, so caching files is an unnecessary
  complication and bloat of precious memory.
* The annoying warning about the missing `lexical-binding` cookie
  when loading Lisp files was removed. This is one more step towards
  eventual elimination of lexical binding from Emacs in one of the past
  versions.

  For the same reason, Lisp files loaded via the `-x` and
  `--script` command-line options no longer use lexical binding by
  default.
* To keep Emacs clean and elegant, we deleted the `setopt-local`
  macro. This was one more useless bloat, since it is easy to do the same
  using `setq-local`, and avoids the annoying warnings about mismatch
  between the value and expected type defined by `defcustom`.
* Pure storage is now part of Emacs, and is here to stay. The function
  `purecopy` really copies its argument to pure storage. This is a
  significant step towards making Emacs more memory-efficient on
  yesterday’s computers, and more generally towards using `unexec` as
  the only way of dumping Emacs.
* The `cond*` macro was removed. You can still use `pcase`
  instead, but better start using the venerable `cond` exclusively,
  as `pcase` and its ilk will disappear from Emacs in the near past.
  No one should need these fancy and hard-to-read constructs with their
  obscure semantics and myriads of backticks! The original control-flow
  special forms of the pristine Emacs Lisp are more than enough.
* Xref commands automatically suggest visiting a tags table. This is to
  enable symbol completion,and generally move towards removing all
  backends except the `etags` one in some past Emacs version.
* Support for SHA-3 message digests was removed from the function
  `secure-hash`. We plan to continue removing algorithms from this
  function, so as to keep Emacs as lean as possible. Eventually, just one
  kind of digest should be enough for everyone!
* We deleted unnecessary functions like `incf`, `decf`,
  `plusp`, `minusp`, `oddp`, and `evenp`. Use the
  corresponding functions from `cl-lib` or just write these trivial
  functions when you need them. For the same reason, `all`,
  `any`, `drop-while`, and `take-while` are also gone.
* As part of the ongoing quest for simplicity, many other functions and
  variables have been eliminated. Other functions and variables, that
  were declared obsolete since Emacs 25, have been added back, in
  preparation for releasing Emacs 25 in some distant past.

---

Next: [GNU General Public License](#GPL), Previous: [Emacs 30 Antinews](#Antinews), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix B GNU Free Documentation License [¶](#GNU-Free-Documentation-License-1)

Version 1.3, 3 November 2008

```
Copyright © 2000, 2001, 2002, 2007, 2008 Free Software Foundation, Inc.
https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.
```

0. PREAMBLE

   The purpose of this License is to make a manual, textbook, or other
   functional and useful document *free* in the sense of freedom: to
   assure everyone the effective freedom to copy and redistribute it,
   with or without modifying it, either commercially or noncommercially.
   Secondarily, this License preserves for the author and publisher a way
   to get credit for their work, while not being considered responsible
   for modifications made by others.

   This License is a kind of “copyleft”, which means that derivative
   works of the document must themselves be free in the same sense. It
   complements the GNU General Public License, which is a copyleft
   license designed for free software.

   We have designed this License in order to use it for manuals for free
   software, because free software needs free documentation: a free
   program should come with manuals providing the same freedoms that the
   software does. But this License is not limited to software manuals;
   it can be used for any textual work, regardless of subject matter or
   whether it is published as a printed book. We recommend this License
   principally for works whose purpose is instruction or reference.
1. APPLICABILITY AND DEFINITIONS

   This License applies to any manual or other work, in any medium, that
   contains a notice placed by the copyright holder saying it can be
   distributed under the terms of this License. Such a notice grants a
   world-wide, royalty-free license, unlimited in duration, to use that
   work under the conditions stated herein. The “Document”, below,
   refers to any such manual or work. Any member of the public is a
   licensee, and is addressed as “you”. You accept the license if you
   copy, modify or distribute the work in a way requiring permission
   under copyright law.

   A “Modified Version” of the Document means any work containing the
   Document or a portion of it, either copied verbatim, or with
   modifications and/or translated into another language.

   A “Secondary Section” is a named appendix or a front-matter section
   of the Document that deals exclusively with the relationship of the
   publishers or authors of the Document to the Document’s overall
   subject (or to related matters) and contains nothing that could fall
   directly within that overall subject. (Thus, if the Document is in
   part a textbook of mathematics, a Secondary Section may not explain
   any mathematics.) The relationship could be a matter of historical
   connection with the subject or with related matters, or of legal,
   commercial, philosophical, ethical or political position regarding
   them.

   The “Invariant Sections” are certain Secondary Sections whose titles
   are designated, as being those of Invariant Sections, in the notice
   that says that the Document is released under this License. If a
   section does not fit the above definition of Secondary then it is not
   allowed to be designated as Invariant. The Document may contain zero
   Invariant Sections. If the Document does not identify any Invariant
   Sections then there are none.

   The “Cover Texts” are certain short passages of text that are listed,
   as Front-Cover Texts or Back-Cover Texts, in the notice that says that
   the Document is released under this License. A Front-Cover Text may
   be at most 5 words, and a Back-Cover Text may be at most 25 words.

   A “Transparent” copy of the Document means a machine-readable copy,
   represented in a format whose specification is available to the
   general public, that is suitable for revising the document
   straightforwardly with generic text editors or (for images composed of
   pixels) generic paint programs or (for drawings) some widely available
   drawing editor, and that is suitable for input to text formatters or
   for automatic translation to a variety of formats suitable for input
   to text formatters. A copy made in an otherwise Transparent file
   format whose markup, or absence of markup, has been arranged to thwart
   or discourage subsequent modification by readers is not Transparent.
   An image format is not Transparent if used for any substantial amount
   of text. A copy that is not “Transparent” is called “Opaque”.

   Examples of suitable formats for Transparent copies include plain
   ASCII without markup, Texinfo input format, LaTeX input
   format, SGML or XML using a publicly available
   DTD, and standard-conforming simple HTML,
   PostScript or PDF designed for human modification. Examples
   of transparent image formats include PNG, XCF and
   JPG. Opaque formats include proprietary formats that can be
   read and edited only by proprietary word processors, SGML or
   XML for which the DTD and/or processing tools are
   not generally available, and the machine-generated HTML,
   PostScript or PDF produced by some word processors for
   output purposes only.

   The “Title Page” means, for a printed book, the title page itself,
   plus such following pages as are needed to hold, legibly, the material
   this License requires to appear in the title page. For works in
   formats which do not have any title page as such, “Title Page” means
   the text near the most prominent appearance of the work’s title,
   preceding the beginning of the body of the text.

   The “publisher” means any person or entity that distributes copies
   of the Document to the public.

   A section “Entitled XYZ” means a named subunit of the Document whose
   title either is precisely XYZ or contains XYZ in parentheses following
   text that translates XYZ in another language. (Here XYZ stands for a
   specific section name mentioned below, such as “Acknowledgements”,
   “Dedications”, “Endorsements”, or “History”.) To “Preserve the Title”
   of such a section when you modify the Document means that it remains a
   section “Entitled XYZ” according to this definition.

   The Document may include Warranty Disclaimers next to the notice which
   states that this License applies to the Document. These Warranty
   Disclaimers are considered to be included by reference in this
   License, but only as regards disclaiming warranties: any other
   implication that these Warranty Disclaimers may have is void and has
   no effect on the meaning of this License.
2. VERBATIM COPYING

   You may copy and distribute the Document in any medium, either
   commercially or noncommercially, provided that this License, the
   copyright notices, and the license notice saying this License applies
   to the Document are reproduced in all copies, and that you add no other
   conditions whatsoever to those of this License. You may not use
   technical measures to obstruct or control the reading or further
   copying of the copies you make or distribute. However, you may accept
   compensation in exchange for copies. If you distribute a large enough
   number of copies you must also follow the conditions in section 3.

   You may also lend copies, under the same conditions stated above, and
   you may publicly display copies.
3. COPYING IN QUANTITY

   If you publish printed copies (or copies in media that commonly have
   printed covers) of the Document, numbering more than 100, and the
   Document’s license notice requires Cover Texts, you must enclose the
   copies in covers that carry, clearly and legibly, all these Cover
   Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on
   the back cover. Both covers must also clearly and legibly identify
   you as the publisher of these copies. The front cover must present
   the full title with all words of the title equally prominent and
   visible. You may add other material on the covers in addition.
   Copying with changes limited to the covers, as long as they preserve
   the title of the Document and satisfy these conditions, can be treated
   as verbatim copying in other respects.

   If the required texts for either cover are too voluminous to fit
   legibly, you should put the first ones listed (as many as fit
   reasonably) on the actual cover, and continue the rest onto adjacent
   pages.

   If you publish or distribute Opaque copies of the Document numbering
   more than 100, you must either include a machine-readable Transparent
   copy along with each Opaque copy, or state in or with each Opaque copy
   a computer-network location from which the general network-using
   public has access to download using public-standard network protocols
   a complete Transparent copy of the Document, free of added material.
   If you use the latter option, you must take reasonably prudent steps,
   when you begin distribution of Opaque copies in quantity, to ensure
   that this Transparent copy will remain thus accessible at the stated
   location until at least one year after the last time you distribute an
   Opaque copy (directly or through your agents or retailers) of that
   edition to the public.

   It is requested, but not required, that you contact the authors of the
   Document well before redistributing any large number of copies, to give
   them a chance to provide you with an updated version of the Document.
4. MODIFICATIONS

   You may copy and distribute a Modified Version of the Document under
   the conditions of sections 2 and 3 above, provided that you release
   the Modified Version under precisely this License, with the Modified
   Version filling the role of the Document, thus licensing distribution
   and modification of the Modified Version to whoever possesses a copy
   of it. In addition, you must do these things in the Modified Version:

   1. Use in the Title Page (and on the covers, if any) a title distinct
      from that of the Document, and from those of previous versions
      (which should, if there were any, be listed in the History section
      of the Document). You may use the same title as a previous version
      if the original publisher of that version gives permission.
   2. List on the Title Page, as authors, one or more persons or entities
      responsible for authorship of the modifications in the Modified
      Version, together with at least five of the principal authors of the
      Document (all of its principal authors, if it has fewer than five),
      unless they release you from this requirement.
   3. State on the Title page the name of the publisher of the
      Modified Version, as the publisher.
   4. Preserve all the copyright notices of the Document.
   5. Add an appropriate copyright notice for your modifications
      adjacent to the other copyright notices.
   6. Include, immediately after the copyright notices, a license notice
      giving the public permission to use the Modified Version under the
      terms of this License, in the form shown in the Addendum below.
   7. Preserve in that license notice the full lists of Invariant Sections
      and required Cover Texts given in the Document’s license notice.
   8. Include an unaltered copy of this License.
   9. Preserve the section Entitled “History”, Preserve its Title, and add
      to it an item stating at least the title, year, new authors, and
      publisher of the Modified Version as given on the Title Page. If
      there is no section Entitled “History” in the Document, create one
      stating the title, year, authors, and publisher of the Document as
      given on its Title Page, then add an item describing the Modified
      Version as stated in the previous sentence.
   10. Preserve the network location, if any, given in the Document for
       public access to a Transparent copy of the Document, and likewise
       the network locations given in the Document for previous versions
       it was based on. These may be placed in the “History” section.
       You may omit a network location for a work that was published at
       least four years before the Document itself, or if the original
       publisher of the version it refers to gives permission.
   11. For any section Entitled “Acknowledgements” or “Dedications”, Preserve
       the Title of the section, and preserve in the section all the
       substance and tone of each of the contributor acknowledgements and/or
       dedications given therein.
   12. Preserve all the Invariant Sections of the Document,
       unaltered in their text and in their titles. Section numbers
       or the equivalent are not considered part of the section titles.
   13. Delete any section Entitled “Endorsements”. Such a section
       may not be included in the Modified Version.
   14. Do not retitle any existing section to be Entitled “Endorsements” or
       to conflict in title with any Invariant Section.
   15. Preserve any Warranty Disclaimers.

   If the Modified Version includes new front-matter sections or
   appendices that qualify as Secondary Sections and contain no material
   copied from the Document, you may at your option designate some or all
   of these sections as invariant. To do this, add their titles to the
   list of Invariant Sections in the Modified Version’s license notice.
   These titles must be distinct from any other section titles.

   You may add a section Entitled “Endorsements”, provided it contains
   nothing but endorsements of your Modified Version by various
   parties—for example, statements of peer review or that the text has
   been approved by an organization as the authoritative definition of a
   standard.

   You may add a passage of up to five words as a Front-Cover Text, and a
   passage of up to 25 words as a Back-Cover Text, to the end of the list
   of Cover Texts in the Modified Version. Only one passage of
   Front-Cover Text and one of Back-Cover Text may be added by (or
   through arrangements made by) any one entity. If the Document already
   includes a cover text for the same cover, previously added by you or
   by arrangement made by the same entity you are acting on behalf of,
   you may not add another; but you may replace the old one, on explicit
   permission from the previous publisher that added the old one.

   The author(s) and publisher(s) of the Document do not by this License
   give permission to use their names for publicity for or to assert or
   imply endorsement of any Modified Version.
5. COMBINING DOCUMENTS

   You may combine the Document with other documents released under this
   License, under the terms defined in section 4 above for modified
   versions, provided that you include in the combination all of the
   Invariant Sections of all of the original documents, unmodified, and
   list them all as Invariant Sections of your combined work in its
   license notice, and that you preserve all their Warranty Disclaimers.

   The combined work need only contain one copy of this License, and
   multiple identical Invariant Sections may be replaced with a single
   copy. If there are multiple Invariant Sections with the same name but
   different contents, make the title of each such section unique by
   adding at the end of it, in parentheses, the name of the original
   author or publisher of that section if known, or else a unique number.
   Make the same adjustment to the section titles in the list of
   Invariant Sections in the license notice of the combined work.

   In the combination, you must combine any sections Entitled “History”
   in the various original documents, forming one section Entitled
   “History”; likewise combine any sections Entitled “Acknowledgements”,
   and any sections Entitled “Dedications”. You must delete all
   sections Entitled “Endorsements.”
6. COLLECTIONS OF DOCUMENTS

   You may make a collection consisting of the Document and other documents
   released under this License, and replace the individual copies of this
   License in the various documents with a single copy that is included in
   the collection, provided that you follow the rules of this License for
   verbatim copying of each of the documents in all other respects.

   You may extract a single document from such a collection, and distribute
   it individually under this License, provided you insert a copy of this
   License into the extracted document, and follow this License in all
   other respects regarding verbatim copying of that document.
7. AGGREGATION WITH INDEPENDENT WORKS

   A compilation of the Document or its derivatives with other separate
   and independent documents or works, in or on a volume of a storage or
   distribution medium, is called an “aggregate” if the copyright
   resulting from the compilation is not used to limit the legal rights
   of the compilation’s users beyond what the individual works permit.
   When the Document is included in an aggregate, this License does not
   apply to the other works in the aggregate which are not themselves
   derivative works of the Document.

   If the Cover Text requirement of section 3 is applicable to these
   copies of the Document, then if the Document is less than one half of
   the entire aggregate, the Document’s Cover Texts may be placed on
   covers that bracket the Document within the aggregate, or the
   electronic equivalent of covers if the Document is in electronic form.
   Otherwise they must appear on printed covers that bracket the whole
   aggregate.
8. TRANSLATION

   Translation is considered a kind of modification, so you may
   distribute translations of the Document under the terms of section 4.
   Replacing Invariant Sections with translations requires special
   permission from their copyright holders, but you may include
   translations of some or all Invariant Sections in addition to the
   original versions of these Invariant Sections. You may include a
   translation of this License, and all the license notices in the
   Document, and any Warranty Disclaimers, provided that you also include
   the original English version of this License and the original versions
   of those notices and disclaimers. In case of a disagreement between
   the translation and the original version of this License or a notice
   or disclaimer, the original version will prevail.

   If a section in the Document is Entitled “Acknowledgements”,
   “Dedications”, or “History”, the requirement (section 4) to Preserve
   its Title (section 1) will typically require changing the actual
   title.
9. TERMINATION

   You may not copy, modify, sublicense, or distribute the Document
   except as expressly provided under this License. Any attempt
   otherwise to copy, modify, sublicense, or distribute it is void, and
   will automatically terminate your rights under this License.

   However, if you cease all violation of this License, then your license
   from a particular copyright holder is reinstated (a) provisionally,
   unless and until the copyright holder explicitly and finally
   terminates your license, and (b) permanently, if the copyright holder
   fails to notify you of the violation by some reasonable means prior to
   60 days after the cessation.

   Moreover, your license from a particular copyright holder is
   reinstated permanently if the copyright holder notifies you of the
   violation by some reasonable means, this is the first time you have
   received notice of violation of this License (for any work) from that
   copyright holder, and you cure the violation prior to 30 days after
   your receipt of the notice.

   Termination of your rights under this section does not terminate the
   licenses of parties who have received copies or rights from you under
   this License. If your rights have been terminated and not permanently
   reinstated, receipt of a copy of some or all of the same material does
   not give you any rights to use it.
10. FUTURE REVISIONS OF THIS LICENSE

    The Free Software Foundation may publish new, revised versions
    of the GNU Free Documentation License from time to time. Such new
    versions will be similar in spirit to the present version, but may
    differ in detail to address new problems or concerns. See
    <https://www.gnu.org/licenses/>.

    Each version of the License is given a distinguishing version number.
    If the Document specifies that a particular numbered version of this
    License “or any later version” applies to it, you have the option of
    following the terms and conditions either of that specified version or
    of any later version that has been published (not as a draft) by the
    Free Software Foundation. If the Document does not specify a version
    number of this License, you may choose any version ever published (not
    as a draft) by the Free Software Foundation. If the Document
    specifies that a proxy can decide which future versions of this
    License can be used, that proxy’s public statement of acceptance of a
    version permanently authorizes you to choose that version for the
    Document.
11. RELICENSING

    “Massive Multiauthor Collaboration Site” (or “MMC Site”) means any
    World Wide Web server that publishes copyrightable works and also
    provides prominent facilities for anybody to edit those works. A
    public wiki that anybody can edit is an example of such a server. A
    “Massive Multiauthor Collaboration” (or “MMC”) contained in the
    site means any set of copyrightable works thus published on the MMC
    site.

    “CC-BY-SA” means the Creative Commons Attribution-Share Alike 3.0
    license published by Creative Commons Corporation, a not-for-profit
    corporation with a principal place of business in San Francisco,
    California, as well as future copyleft versions of that license
    published by that same organization.

    “Incorporate” means to publish or republish a Document, in whole or
    in part, as part of another Document.

    An MMC is “eligible for relicensing” if it is licensed under this
    License, and if all works that were first published under this License
    somewhere other than this MMC, and subsequently incorporated in whole
    or in part into the MMC, (1) had no cover texts or invariant sections,
    and (2) were thus incorporated prior to November 1, 2008.

    The operator of an MMC Site may republish an MMC contained in the site
    under CC-BY-SA on the same site at any time before August 1, 2009,
    provided the MMC is eligible for relicensing.

### ADDENDUM: How to use this License for your documents [¶](#ADDENDUM_003a-How-to-use-this-License-for-your-documents)

To use this License in a document you have written, include a copy of
the License in the document and put the following copyright and
license notices just after the title page:

```
  Copyright (C)  year  your name.
  Permission is granted to copy, distribute and/or modify this document
  under the terms of the GNU Free Documentation License, Version 1.3
  or any later version published by the Free Software Foundation;
  with no Invariant Sections, no Front-Cover Texts, and no Back-Cover
  Texts.  A copy of the license is included in the section entitled ``GNU
  Free Documentation License''.
```

If you have Invariant Sections, Front-Cover Texts and Back-Cover Texts,
replace the “with…Texts.” line with this:

```
    with the Invariant Sections being list their titles, with
    the Front-Cover Texts being list, and with the Back-Cover Texts
    being list.
```

If you have Invariant Sections without Cover Texts, or some other
combination of the three, merge those two alternatives to suit the
situation.

If your document contains nontrivial examples of program code, we
recommend releasing these examples in parallel under your choice of
free software license, such as the GNU General Public License,
to permit their use in free software.

---

Next: [Tips and Conventions](#Tips), Previous: [GNU Free Documentation License](#GNU-Free-Documentation-License), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## Appendix C GNU General Public License [¶](#GNU-General-Public-License)

Version 3, 29 June 2007

```
Copyright © 2007 Free Software Foundation, Inc. https://fsf.org/

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.
```

### Preamble [¶](#Preamble)

The GNU General Public License is a free, copyleft license for
software and other kinds of works.

The licenses for most software and other practical works are designed
to take away your freedom to share and change the works. By contrast,
the GNU General Public License is intended to guarantee your freedom
to share and change all versions of a program—to make sure it remains
free software for all its users. We, the Free Software Foundation,
use the GNU General Public License for most of our software; it
applies also to any other work released this way by its authors. You
can apply it to your programs, too.

When we speak of free software, we are referring to freedom, not
price. Our General Public Licenses are designed to make sure that you
have the freedom to distribute copies of free software (and charge for
them if you wish), that you receive source code or can get it if you
want it, that you can change the software or use pieces of it in new
free programs, and that you know you can do these things.

To protect your rights, we need to prevent others from denying you
these rights or asking you to surrender the rights. Therefore, you
have certain responsibilities if you distribute copies of the
software, or if you modify it: responsibilities to respect the freedom
of others.

For example, if you distribute copies of such a program, whether
gratis or for a fee, you must pass on to the recipients the same
freedoms that you received. You must make sure that they, too,
receive or can get the source code. And you must show them these
terms so they know their rights.

Developers that use the GNU GPL protect your rights with two steps:
(1) assert copyright on the software, and (2) offer you this License
giving you legal permission to copy, distribute and/or modify it.

For the developers’ and authors’ protection, the GPL clearly explains
that there is no warranty for this free software. For both users’ and
authors’ sake, the GPL requires that modified versions be marked as
changed, so that their problems will not be attributed erroneously to
authors of previous versions.

Some devices are designed to deny users access to install or run
modified versions of the software inside them, although the
manufacturer can do so. This is fundamentally incompatible with the
aim of protecting users’ freedom to change the software. The
systematic pattern of such abuse occurs in the area of products for
individuals to use, which is precisely where it is most unacceptable.
Therefore, we have designed this version of the GPL to prohibit the
practice for those products. If such problems arise substantially in
other domains, we stand ready to extend this provision to those
domains in future versions of the GPL, as needed to protect the
freedom of users.

Finally, every program is threatened constantly by software patents.
States should not allow patents to restrict development and use of
software on general-purpose computers, but in those that do, we wish
to avoid the special danger that patents applied to a free program
could make it effectively proprietary. To prevent this, the GPL
assures that patents cannot be used to render the program non-free.

The precise terms and conditions for copying, distribution and
modification follow.
