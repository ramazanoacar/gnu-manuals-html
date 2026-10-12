### 26.12 Making Certain File Names “Magic” [¶](#Making-Certain-File-Names-_0060_0060Magic_0027_0027)

You can implement special handling for certain file names. This is
called making those names *magic*. The principal use for this
feature is in implementing access to remote files (see [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual).

To define a kind of magic file name, you must supply a regular
expression to define the class of names (all those that match the
regular expression), plus a handler that implements all the primitive
Emacs file operations for file names that match.

The variable `file-name-handler-alist` holds a list of handlers,
together with regular expressions that determine when to apply each
handler. Each element has this form:

```
(regexp . handler)
```

All the Emacs primitives for file access and file name transformation
check the given file name against `file-name-handler-alist`. If
the file name matches regexp, the primitives handle that file by
calling handler.

The first argument given to handler is the name of the
primitive, as a symbol; the remaining arguments are the arguments that
were passed to that primitive. (The first of these arguments is most
often the file name itself.) For example, if you do this:

```
(file-exists-p filename)
```

and filename has handler handler, then handler is
called like this:

```
(funcall handler 'file-exists-p filename)
```

When a function takes two or more arguments that must be file names,
it checks each of those names for a handler. For example, if you do
this:

```
(expand-file-name filename dirname)
```

then it checks for a handler for filename and then for a handler
for dirname. In either case, the handler is called like
this:

```
(funcall handler 'expand-file-name filename dirname)
```

The handler then needs to figure out whether to handle
filename or dirname.

If the specified file name matches more than one handler, the one
whose match starts last in the file name gets precedence. This rule
is chosen so that handlers for jobs such as uncompression are handled
first, before handlers for jobs such as remote file access.

Here are the operations that a magic file name handler gets to handle:

`abbreviate-file-name`, `access-file`,
`add-name-to-file`, `byte-compiler-base-file-name`,  
`copy-directory`, `copy-file`,
`delete-directory`, `delete-file`,
`diff-latest-backup-file`,
`directory-file-name`,
`directory-files`,
`directory-files-and-attributes`,
`dired-compress-file`, `dired-uncache`,
`exec-path`, `expand-file-name`,  
`file-accessible-directory-p`,
`file-acl`,
`file-attributes`,
`file-directory-p`,
`file-equal-p`,
`file-executable-p`, `file-exists-p`,
`file-group-gid`, `file-in-directory-p`,
`file-local-copy`, `file-locked-p`,
`file-modes`, `file-name-all-completions`,
`file-name-as-directory`,
`file-name-case-insensitive-p`,
`file-name-completion`,
`file-name-directory`,
`file-name-nondirectory`,
`file-name-sans-versions`, `file-newer-than-file-p`,
`file-notify-add-watch`, `file-notify-rm-watch`,
`file-notify-valid-p`,
`file-ownership-preserved-p`,
`file-readable-p`, `file-regular-p`,
`file-remote-p`, `file-selinux-context`,
`file-symlink-p`, `file-system-info`,
`file-truename`, `file-user-uid`,
`file-writable-p`,
`find-backup-file-name`,  
`get-file-buffer`,
`insert-directory`,
`insert-file-contents`,  
`list-system-processes`,
`load`, `lock-file`,
`make-auto-save-file-name`,
`make-directory`,
`make-lock-file-name`,
`make-nearby-temp-file`,
`make-process`,
`make-symbolic-link`,  
`memory-info`, `process-attributes`, `process-file`,
`rename-file`, `set-file-acl`, `set-file-modes`,
`set-file-selinux-context`, `set-file-times`,
`set-visited-file-modtime`, `shell-command`,
`start-file-process`,
`substitute-in-file-name`,  
`temporary-file-directory`,
`unhandled-file-name-directory`,
`unlock-file`,
`vc-registered`,
`verify-visited-file-modtime`,  
`write-region`.

Handlers for `insert-file-contents` typically need to clear the
buffer’s modified flag, with `(set-buffer-modified-p nil)`, if the
visit argument is non-`nil`. This also has the effect of
unlocking the buffer if it is locked.

The handler function must handle all of the above operations, and
possibly others to be added in the future. It need not implement all
these operations itself—when it has nothing special to do for a
certain operation, it can reinvoke the primitive, to handle the
operation in the usual way. It should always reinvoke the primitive
for an operation it does not recognize. Here’s one way to do this:

```
(defun my-file-handler (operation &rest args)
  ;; First check for the specific operations
  ;; that we have special handling for.
  (cond ((eq operation 'insert-file-contents) ...)
        ((eq operation 'write-region) ...)
        ...
        ;; Handle any operation we don’t know about.
        (t (let ((inhibit-file-name-handlers
                  (cons 'my-file-handler
                        (and (eq inhibit-file-name-operation operation)
                             inhibit-file-name-handlers)))
                 (inhibit-file-name-operation operation))
             (apply operation args)))))
```

When a handler function decides to call the ordinary Emacs primitive for
the operation at hand, it needs to prevent the primitive from calling
the same handler once again, thus leading to an infinite recursion. The
example above shows how to do this, with the variables
`inhibit-file-name-handlers` and
`inhibit-file-name-operation`. Be careful to use them exactly as
shown above; the details are crucial for proper behavior in the case of
multiple handlers, and for operations that have two file names that may
each have handlers.

Handlers that don’t really do anything special for actual access to the
file—such as the ones that implement completion of host names for
remote file names—should have a non-`nil` `safe-magic`
property. For instance, Emacs normally protects directory names
it finds in `PATH` from becoming magic, if they look like magic
file names, by prefixing them with ‘`/:`’. But if the handler that
would be used for them has a non-`nil` `safe-magic`
property, the ‘`/:`’ is not added.

A file name handler can have an `operations` property to
declare which operations it handles in a nontrivial way. If this
property has a non-`nil` value, it should be a list of
operations; then only those operations will call the handler. This
avoids inefficiency, but its main purpose is for autoloaded handler
functions, so that they won’t be loaded except when they have real
work to do.

Simply deferring all operations to the usual primitives does not
work. For instance, if the file name handler applies to
`file-exists-p`, then it must handle `load` itself, because
the usual `load` code won’t work properly in that case. However,
if the handler uses the `operations` property to say it doesn’t
handle `file-exists-p`, then it need not handle `load`
nontrivially.

Variable: **inhibit-file-name-handlers** [¶](#index-inhibit_002dfile_002dname_002dhandlers)
:   This variable holds a list of handlers whose use is presently inhibited
    for a certain operation.

Variable: **inhibit-file-name-operation** [¶](#index-inhibit_002dfile_002dname_002doperation)
:   The operation for which certain handlers are presently inhibited.

Function: **find-file-name-handler** file operation [¶](#index-find_002dfile_002dname_002dhandler)
:   This function returns the handler function for file name file,
    or `nil` if there is none. The argument operation should
    be the operation to be performed on the file—the value you will pass
    to the handler as its first argument when you call it. If
    operation equals `inhibit-file-name-operation`, or if it is
    not found in the `operations` property of the handler, this
    function returns `nil`.

Function: **file-local-copy** filename [¶](#index-file_002dlocal_002dcopy)
:   This function copies file filename to an ordinary non-magic file
    on the local machine, if it isn’t on the local machine already. Magic
    file names should handle the `file-local-copy` operation if they
    refer to files on other machines. A magic file name that is used for
    other purposes than remote file access should not handle
    `file-local-copy`; then this function will treat the file as
    local.

    If filename is local, whether magic or not, this function does
    nothing and returns `nil`. Otherwise it returns the file name
    of the local copy file.

Function: **file-remote-p** filename &optional identification connected [¶](#index-file_002dremote_002dp)
:   This function tests whether filename is a remote file. If
    filename is local (not remote), the return value is `nil`.
    If filename is indeed remote, the return value is a string that
    identifies the remote system.

    This identifier string can include a host name and a user name, as
    well as characters designating the method used to access the remote
    system. For example, the remote identifier string for the file name
    `/sudo::/some/file` is `/sudo:root@localhost:`.

    If `file-remote-p` returns the same identifier for two different
    file names, that means they are stored on the same file system and can
    be accessed locally with respect to each other. This means, for
    example, that it is possible to start a remote process accessing both
    files at the same time. Implementers of file name handlers need to
    ensure this principle is valid.

    identification specifies which part of the identifier shall be
    returned as string. identification can be the symbol
    `method`, `user`, `host` or `localname`; any other
    value is handled like `nil` and means to return the complete
    identifier string. In the example above, the remote `user`
    identifier string would be `root`.

    If the remote file does not contain a method, a user name, or a
    host name, the respective default value is returned. The string
    returned for identification `localname` can differ depending
    on whether there is an existing connection. File name handler specific
    implementations could support further identification symbols;
    See [Tramp](https://www.gnu.org/software/emacs/manual/html_mono/tramp.html#Top), for example, knows also the `hop`
    symbol.

    If connected is non-`nil`, this function returns `nil`
    even if filename is remote, if Emacs has no network connection
    to its host. This is useful when you want to avoid the delay of
    making connections when they don’t exist. If connected is
    `never`, *never* use an existing connection to return the
    identification, even if one is already present (this is otherwise like
    a value of `nil`). This lets you prevent any connection-specific
    logic, such as expanding the local part of the file name.

Function: **unhandled-file-name-directory** filename [¶](#index-unhandled_002dfile_002dname_002ddirectory)
:   This function returns the name of a directory that is not magic. For
    a non-magic filename it returns the corresponding directory name
    (see [Directory Names](#Directory-Names)). For a magic filename, it invokes the
    file name handler, which therefore decides what value to return. If
    filename is not accessible from a local process, then the file
    name handler should indicate that by returning `nil`.

    This is useful for running a subprocess; every subprocess must have a
    non-magic directory to serve as its current directory, and this function
    is a good way to come up with one.

Function: **file-local-name** filename [¶](#index-file_002dlocal_002dname)
:   This function returns the *local part* of filename. This is
    the part of the file’s name that identifies it on the remote host, and
    is typically obtained by removing from the remote file name the parts
    that specify the remote host and the method of accessing it. For
    example:

    ```
    (file-local-name "/ssh:user@host:/foo/bar")
         ⇒ "/foo/bar"
    ```

    For a remote filename, this function returns a file name which
    could be used directly as an argument of a remote process
    (see [Creating an Asynchronous Process](#Asynchronous-Processes), and see [Creating a Synchronous Process](#Synchronous-Processes)),
    and as the program to run on the remote host. If filename is
    local, this function returns it unchanged.

User Option: **remote-file-name-inhibit-cache** [¶](#index-remote_002dfile_002dname_002dinhibit_002dcache)
:   The attributes of remote files can be cached for better performance. If
    they are changed outside of Emacs’s control, the cached values become
    invalid, and must be reread.

    When this variable is set to `nil`, cached values are never
    expired. Use this setting with caution, only if you are sure nothing
    other than Emacs ever changes the remote files. If it is set to
    `t`, cached values are never used. This is the safest value, but
    could result in performance degradation.

    A compromise is to set it to a positive number. This means that
    cached values are used for that amount of seconds since they were
    cached. If a remote file is checked regularly, it might be a good
    idea to let-bind this variable to a value less than the time period
    between consecutive checks. For example:

    ```
    (defun display-time-file-nonempty-p (file)
      (let ((remote-file-name-inhibit-cache
             (- display-time-interval 5)))
        (and (file-exists-p file)
             (< 0 (file-attribute-size
                   (file-attributes
                    (file-chase-links file)))))))
    ```

Macro: **without-remote-files** body… [¶](#index-without_002dremote_002dfiles)
:   The `without-remote-files` macro evaluates the body forms
    with deactivated file name handlers for remote files. Those file
    names would be handled literally.

    The macro should be used only in forms where it is obvious, that
    remote files cannot appear or where it is intended not to handle
    remote file names. It also reduces checks with
    `file-name-handler-alist`, resulting in more performant code.

---

Previous: [Making Certain File Names “Magic”](#Magic-File-Names), Up: [Files](#Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 26.13 File Format Conversion [¶](#File-Format-Conversion)

Emacs performs several steps to convert the data in a buffer (text,
text properties, and possibly other information) to and from a
representation suitable for storing into a file. This section describes
the fundamental functions that perform this *format conversion*,
namely `insert-file-contents` for reading a file into a buffer,
and `write-region` for writing a buffer into a file.

* [Overview](#Format-Conversion-Overview)
* [Round-Trip Specification](#Format-Conversion-Round_002dTrip)
* [Piecemeal Specification](#Format-Conversion-Piecemeal)

---

Next: [Round-Trip Specification](#Format-Conversion-Round_002dTrip), Up: [File Format Conversion](#Format-Conversion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.13.1 Overview [¶](#Overview)

The function `insert-file-contents`:

* initially, inserts bytes from the file into the buffer;
* decodes bytes to characters as appropriate;
* processes formats as defined by entries in `format-alist`; and
* calls functions in `after-insert-file-functions`.

The function `write-region`:

* initially, calls functions in `write-region-annotate-functions`;
* processes formats as defined by entries in `format-alist`;
* encodes characters to bytes as appropriate; and
* modifies the file with the bytes.

This shows the symmetry of the lowest-level operations; reading and
writing handle things in opposite order. The rest of this section
describes the two facilities surrounding the three variables named
above, as well as some related functions. [Coding Systems](#Coding-Systems), for
details on character encoding and decoding.

---

Next: [Piecemeal Specification](#Format-Conversion-Piecemeal), Previous: [Overview](#Format-Conversion-Overview), Up: [File Format Conversion](#Format-Conversion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.13.2 Round-Trip Specification [¶](#Round_002dTrip-Specification)

The most general of the two facilities is controlled by the variable
`format-alist`, a list of *file format* specifications, which
describe textual representations used in files for the data in an Emacs
buffer. The descriptions for reading and writing are paired, which is
why we call this “round-trip” specification
(see [Piecemeal Specification](#Format-Conversion-Piecemeal), for non-paired specification).

Variable: **format-alist** [¶](#index-format_002dalist)
:   This list contains one format definition for each defined file format.
    Each format definition is a list of this form:

    ```
    (name doc-string regexp from-fn to-fn modify mode-fn preserve)
    ```

Here is what the elements in a format definition mean:

name
:   The name of this format.

doc-string
:   A documentation string for the format.

regexp
:   A regular expression which is used to recognize files represented in
    this format. If `nil`, the format is never applied automatically.

from-fn
:   A shell command or function to decode data in this format (to convert
    file data into the usual Emacs data representation).

    A shell command is represented as a string; Emacs runs the command as a
    filter to perform the conversion.

    If from-fn is a function, it is called with two arguments, begin
    and end, which specify the part of the buffer it should convert.
    It should convert the text by editing it in place. Since this can
    change the length of the text, from-fn should return the modified
    end position.

    One responsibility of from-fn is to make sure that the beginning
    of the file no longer matches regexp. Otherwise it is likely to
    get called again. Also, from-fn must not involve buffers or
    files other than the one being decoded, otherwise the internal buffer
    used for formatting might be overwritten.

to-fn
:   A shell command or function to encode data in this format—that is, to
    convert the usual Emacs data representation into this format.

    If to-fn is a string, it is a shell command; Emacs runs the
    command as a filter to perform the conversion.

    If to-fn is a function, it is called with three arguments:
    begin and end, which specify the part of the buffer it
    should convert, and buffer, which specifies which buffer. There
    are two ways it can do the conversion:

    * By editing the buffer in place. In this case, to-fn should
      return the end-position of the range of text, as modified.
    * By returning a list of annotations. This is a list of elements of the
      form `(position . string)`, where position is an
      integer specifying the relative position in the text to be written, and
      string is the annotation to add there. The list must be sorted in
      order of position when to-fn returns it.

      When `write-region` actually writes the text from the buffer to the
      file, it intermixes the specified annotations at the corresponding
      positions. All this takes place without modifying the buffer.

    to-fn must not involve buffers or files other than the one being
    encoded, otherwise the internal buffer used for formatting might be
    overwritten.

modify
:   A flag, `t` if the encoding function modifies the buffer, and
    `nil` if it works by returning a list of annotations.

mode-fn
:   A minor-mode function to call after visiting a file converted from this
    format. The function is called with one argument, the integer 1;
    that tells a minor-mode function to enable the mode.

preserve
:   A flag, `t` if `format-write-file` should not remove this format
    from `buffer-file-format`.

The function `insert-file-contents` automatically recognizes file
formats when it reads the specified file. It checks the text of the
beginning of the file against the regular expressions of the format
definitions, and if it finds a match, it calls the decoding function for
that format. Then it checks all the known formats over again.
It keeps checking them until none of them is applicable.

Visiting a file, with `find-file-noselect` or the commands that use
it, performs conversion likewise (because it calls
`insert-file-contents`); it also calls the mode function for each
format that it decodes. It stores a list of the format names in the
buffer-local variable `buffer-file-format`.

Variable: **buffer-file-format** [¶](#index-buffer_002dfile_002dformat)
:   This variable states the format of the visited file. More precisely,
    this is a list of the file format names that were decoded in the course
    of visiting the current buffer’s file. It is always buffer-local in all
    buffers.

When `write-region` writes data into a file, it first calls the
encoding functions for the formats listed in `buffer-file-format`,
in the order of appearance in the list.

Command: **format-write-file** file format &optional confirm [¶](#index-format_002dwrite_002dfile)
:   This command writes the current buffer contents into the file file
    in a format based on format, which is a list of format names. It
    constructs the actual format starting from format, then appending
    any elements from the value of `buffer-file-format` with a
    non-`nil` preserve flag (see above), if they are not already
    present in format. It then updates `buffer-file-format` with
    this format, making it the default for future saves. Except for the
    format argument, this command is similar to `write-file`. In
    particular, confirm has the same meaning and interactive treatment
    as the corresponding argument to `write-file`. See [Definition of write-file](#Definition-of-write_002dfile).

Command: **format-find-file** file format [¶](#index-format_002dfind_002dfile)
:   This command finds the file file, converting it according to
    format format. It also makes format the default if the
    buffer is saved later.

    The argument format is a list of format names. If format is
    `nil`, no conversion takes place. Interactively, typing just
    `RET` for format specifies `nil`.

Command: **format-insert-file** file format &optional beg end [¶](#index-format_002dinsert_002dfile)
:   This command inserts the contents of file file, converting it
    according to format format. If beg and end are
    non-`nil`, they specify which part of the file to read, as in
    `insert-file-contents` (see [Reading from Files](#Reading-from-Files)).

    The return value is like what `insert-file-contents` returns: a
    list of the absolute file name and the length of the data inserted
    (after conversion).

    The argument format is a list of format names. If format is
    `nil`, no conversion takes place. Interactively, typing just
    `RET` for format specifies `nil`.

Variable: **buffer-auto-save-file-format** [¶](#index-buffer_002dauto_002dsave_002dfile_002dformat)
:   This variable specifies the format to use for auto-saving. Its value is
    a list of format names, just like the value of
    `buffer-file-format`; however, it is used instead of
    `buffer-file-format` for writing auto-save files. If the value
    is `t`, the default, auto-saving uses the same format as a
    regular save in the same buffer. This variable is always buffer-local
    in all buffers.

---

Previous: [Round-Trip Specification](#Format-Conversion-Round_002dTrip), Up: [File Format Conversion](#Format-Conversion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 26.13.3 Piecemeal Specification [¶](#Piecemeal-Specification)

In contrast to the round-trip specification described in the previous
subsection (see [Round-Trip Specification](#Format-Conversion-Round_002dTrip)), you can use the variables
`after-insert-file-functions` and `write-region-annotate-functions`
to separately control the respective reading and writing conversions.

Conversion starts with one representation and produces another
representation. When there is only one conversion to do, there is no
conflict about what to start with. However, when there are multiple
conversions involved, conflict may arise when two conversions need to
start with the same data.

This situation is best understood in the context of converting text
properties during `write-region`. For example, the character at
position 42 in a buffer is ‘`X`’ with a text property `foo`. If
the conversion for `foo` is done by inserting into the buffer, say,
‘`FOO:`’, then that changes the character at position 42 from
‘`X`’ to ‘`F`’. The next conversion will start with the wrong
data straight away.

To avoid conflict, cooperative conversions do not modify the buffer,
but instead specify *annotations*, a list of elements of the form
`(position . string)`, sorted in order of increasing
position.

If there is more than one conversion, `write-region` merges their
annotations destructively into one sorted list. Later, when the text
from the buffer is actually written to the file, it intermixes the
specified annotations at the corresponding positions. All this takes
place without modifying the buffer.

In contrast, when reading, the annotations intermixed with the text
are handled immediately. `insert-file-contents` sets point to
the beginning of some text to be converted, then calls the conversion
functions with the length of that text. These functions should always
return with point at the beginning of the inserted text. This
approach makes sense for reading because annotations removed by the
first converter can’t be mistakenly processed by a later converter.
Each conversion function should scan for the annotations it
recognizes, remove the annotation, modify the buffer text (to set a
text property, for example), and return the updated length of the
text, as it stands after those changes. The value returned by one
function becomes the argument to the next function.

Variable: **write-region-annotate-functions** [¶](#index-write_002dregion_002dannotate_002dfunctions)
:   A list of functions for `write-region` to call. Each function in
    the list is called with two arguments: the start and end of the region
    to be written. These functions should not alter the contents of the
    buffer. Instead, they should return annotations.

    As a special case, a function may return with a different buffer
    current. Emacs takes this to mean that the current buffer contains
    altered text to be output. It therefore changes the start and
    end arguments of the `write-region` call, giving them the
    values of `point-min` and `point-max` in the new buffer,
    respectively. It also discards all previous annotations, because they
    should have been dealt with by this function.

Variable: **write-region-post-annotation-function** [¶](#index-write_002dregion_002dpost_002dannotation_002dfunction)
:   The value of this variable, if non-`nil`, should be a function.
    This function is called, with no arguments, after `write-region`
    has completed.

    If any function in `write-region-annotate-functions` returns with
    a different buffer current, Emacs calls
    `write-region-post-annotation-function` more than once. Emacs
    calls it with the last buffer that was current, and again with the
    buffer before that, and so on back to the original buffer.

    Thus, a function in `write-region-annotate-functions` can create
    a buffer, give this variable the local value of `kill-buffer` in
    that buffer, set up the buffer with altered text, and make the buffer
    current. The buffer will be killed after `write-region` is done.

Variable: **after-insert-file-functions** [¶](#index-after_002dinsert_002dfile_002dfunctions)
:   Each function in this list is called by `insert-file-contents`
    with one argument, the number of characters inserted, and with point
    at the beginning of the inserted text. Each function should leave
    point unchanged, and return the new character count describing the
    inserted text as modified by the function.

We invite users to write Lisp programs to store and retrieve text
properties in files, using these hooks, and thus to experiment with
various data formats and find good ones. Eventually we hope users
will produce good, general extensions we can install in Emacs.

We suggest not trying to handle arbitrary Lisp objects as text property
names or values—because a program that general is probably difficult
to write, and slow. Instead, choose a set of possible data types that
are reasonably flexible, and not too hard to encode.

---

Next: [Buffers](#Buffers), Previous: [Files](#Files), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 27 Backups and Auto-Saving [¶](#Backups-and-Auto_002dSaving-1)

Backup files and auto-save files are two methods by which Emacs tries
to protect the user from the consequences of crashes or of the user’s
own errors. Auto-saving preserves the text from earlier in the current
editing session; backup files preserve file contents prior to the
current session.

* [Backup Files](#Backup-Files)
* [Auto-Saving](#Auto_002dSaving)
* [Reverting](#Reverting)

---

Next: [Auto-Saving](#Auto_002dSaving), Up: [Backups and Auto-Saving](#Backups-and-Auto_002dSaving)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 27.1 Backup Files [¶](#Backup-Files-1)

A *backup file* is a copy of the old contents of a file you are
editing. Emacs makes a backup file the first time you save a buffer
into its visited file. Thus, normally, the backup file contains the
contents of the file as it was before the current editing session.
The contents of the backup file normally remain unchanged once it
exists.

Backups are usually made by renaming the visited file to a new name.
Optionally, you can specify that backup files should be made by copying
the visited file. This choice makes a difference for files with
multiple names; it also can affect whether the edited file remains owned
by the original owner or becomes owned by the user editing it.

By default, Emacs makes a single backup file for each file edited.
You can alternatively request numbered backups; then each new backup
file gets a new name. You can delete old numbered backups when you
don’t want them any more, or Emacs can delete them automatically.

For performance, the operating system may not write the backup
file’s contents to secondary storage immediately, or may alias the
backup data with the original until one or the other is later
modified. See [Files and Secondary Storage](#Files-and-Storage).

* [Making Backup Files](#Making-Backups)
* [Backup by Renaming or by Copying?](#Rename-or-Copy)
* [Making and Deleting Numbered Backup Files](#Numbered-Backups)
* [Naming Backup Files](#Backup-Names)

---

Next: [Backup by Renaming or by Copying?](#Rename-or-Copy), Up: [Backup Files](#Backup-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 27.1.1 Making Backup Files [¶](#Making-Backup-Files)

Function: **backup-buffer** [¶](#index-backup_002dbuffer)
:   This function makes a backup of the file visited by the current
    buffer, if appropriate. It is called by `save-buffer` before
    saving the buffer the first time.

    If a backup was made by renaming, the return value is a cons cell of
    the form (modes extra-alist backupname), where
    modes are the mode bits of the original file, as returned by
    `file-modes` (see [Testing Accessibility](#Testing-Accessibility)), extra-alist
    is an alist describing the original file’s extended attributes, as
    returned by `file-extended-attributes` (see [Extended File Attributes](#Extended-Attributes)), and backupname is the name of the backup.

    In all other cases (i.e., if a backup was made by copying or if no
    backup was made), this function returns `nil`.

Variable: **buffer-backed-up** [¶](#index-buffer_002dbacked_002dup)
:   This buffer-local variable says whether this buffer’s file has
    been backed up on account of this buffer. If it is non-`nil`,
    the backup file has been written. Otherwise, the file should be backed
    up when it is next saved (if backups are enabled). This is a
    permanent local; `kill-all-local-variables` does not alter it.

User Option: **make-backup-files** [¶](#index-make_002dbackup_002dfiles)
:   This variable determines whether or not to make backup files. If it
    is non-`nil`, then Emacs creates a backup of each file when it is
    saved for the first time—provided that `backup-inhibited`
    is `nil` (see below).

    The following example shows how to change the `make-backup-files`
    variable only in the Rmail buffers and not elsewhere. Setting it
    `nil` stops Emacs from making backups of these files, which may
    save disk space. (You would put this code in your init file.)

    ```
    (add-hook 'rmail-mode-hook
              (lambda () (setq-local make-backup-files nil)))
    ```

Variable: **backup-enable-predicate** [¶](#index-backup_002denable_002dpredicate)
:   This variable’s value is a function to be called on certain occasions to
    decide whether a file should have backup files. The function receives
    one argument, an absolute file name to consider. If the function returns
    `nil`, backups are disabled for that file. Otherwise, the other
    variables in this section say whether and how to make backups.

    The default value is `normal-backup-enable-predicate`, which checks
    for files in `temporary-file-directory` and
    `small-temporary-file-directory`.

Variable: **backup-inhibited** [¶](#index-backup_002dinhibited)
:   If this variable is non-`nil`, backups are inhibited. It records
    the result of testing `backup-enable-predicate` on the visited file
    name. It can also coherently be used by other mechanisms that inhibit
    backups based on which file is visited. For example, VC sets this
    variable non-`nil` to prevent making backups for files managed
    with a version control system.

    This is a permanent local, so that changing the major mode does not lose
    its value. Major modes should not set this variable—they should set
    `make-backup-files` instead.

User Option: **backup-directory-alist** [¶](#index-backup_002ddirectory_002dalist)
:   This variable’s value is an alist of filename patterns and backup
    directories. Each element looks like

    ```
    (regexp . directory)
    ```

    Backups of files with names matching regexp will be made in
    directory. directory may be relative or absolute. If it is
    absolute, so that all matching files are backed up into the same
    directory, the file names in this directory will be the full name of the
    file backed up with all directory separators changed to ‘`!`’ to
    prevent clashes. This will not work correctly if your filesystem
    truncates the resulting name.

    For the common case of all backups going into one directory, the alist
    should contain a single element pairing ‘`"."`’ with the appropriate
    directory.

    If this variable is `nil` (the default), or it fails to match a
    filename, the backup is made in the original file’s directory.

    On MS-DOS filesystems without long names this variable is always
    ignored.

User Option: **make-backup-file-name-function** [¶](#index-make_002dbackup_002dfile_002dname_002dfunction)
:   This variable’s value is a function to use for making backup file names.
    The function `make-backup-file-name` calls it.
    See [Naming Backup Files](#Backup-Names).

    This could be buffer-local to do something special for specific
    files. If you change it, you may need to change
    `backup-file-name-p` and `file-name-sans-versions` too.

---

Next: [Making and Deleting Numbered Backup Files](#Numbered-Backups), Previous: [Making Backup Files](#Making-Backups), Up: [Backup Files](#Backup-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 27.1.2 Backup by Renaming or by Copying? [¶](#Backup-by-Renaming-or-by-Copying_003f)

There are two ways that Emacs can make a backup file:

* Emacs can rename the original file so that it becomes a backup file, and
  then write the buffer being saved into a new file. After this
  procedure, any other names (i.e., hard links) of the original file now
  refer to the backup file. The new file is owned by the user doing the
  editing, and its group is the default for new files written by the user
  in that directory.
* Emacs can copy the original file into a backup file, and then overwrite
  the original file with new contents. After this procedure, any other
  names (i.e., hard links) of the original file continue to refer to the
  current (updated) version of the file. The file’s owner and group will
  be unchanged.

The first method, renaming, is the default.

The variable `backup-by-copying`, if non-`nil`, says to use
the second method, which is to copy the original file and overwrite it
with the new buffer contents. The variable `file-precious-flag`,
if non-`nil`, also has this effect (as a sideline of its main
significance). See [Saving Buffers](#Saving-Buffers).

User Option: **backup-by-copying** [¶](#index-backup_002dby_002dcopying)
:   If this variable is non-`nil`, Emacs always makes backup files by
    copying. The default is `nil`.

The following three variables, when non-`nil`, cause the second
method to be used in certain special cases. They have no effect on the
treatment of files that don’t fall into the special cases.

User Option: **backup-by-copying-when-linked** [¶](#index-backup_002dby_002dcopying_002dwhen_002dlinked)
:   If this variable is non-`nil`, Emacs makes backups by copying for
    files with multiple names (hard links). The default is `nil`.

    This variable is significant only if `backup-by-copying` is
    `nil`, since copying is always used when that variable is
    non-`nil`.

User Option: **backup-by-copying-when-mismatch** [¶](#index-backup_002dby_002dcopying_002dwhen_002dmismatch)
:   If this variable is non-`nil` (the default), Emacs makes backups
    by copying in cases where renaming would change either the owner or
    the group of the file.

    The value has no effect when renaming would not alter the owner or
    group of the file; that is, for files which are owned by the user and
    whose group matches the default for a new file created there by the
    user.

    This variable is significant only if `backup-by-copying` is
    `nil`, since copying is always used when that variable is
    non-`nil`.

User Option: **backup-by-copying-when-privileged-mismatch** [¶](#index-backup_002dby_002dcopying_002dwhen_002dprivileged_002dmismatch)
:   This variable, if non-`nil`, specifies the same behavior as
    `backup-by-copying-when-mismatch`, but only for certain user-id
    and group-id values: namely, those less than or equal to a certain number.
    You set this variable to that number.

    Thus, if you set `backup-by-copying-when-privileged-mismatch`
    to 0, backup by copying is done for the superuser and group 0 only,
    when necessary to prevent a change in the owner of the file.

    The default is 200.

---

Next: [Naming Backup Files](#Backup-Names), Previous: [Backup by Renaming or by Copying?](#Rename-or-Copy), Up: [Backup Files](#Backup-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 27.1.3 Making and Deleting Numbered Backup Files [¶](#Making-and-Deleting-Numbered-Backup-Files)

If a file’s name is `foo`, the names of its numbered backup
versions are `foo.~v~`, for various integers v, like
this: `foo.~1~`, `foo.~2~`, `foo.~3~`, …,
`foo.~259~`, and so on.

User Option: **version-control** [¶](#index-version_002dcontrol)
:   This variable controls whether to make a single non-numbered backup
    file or multiple numbered backups.

    `nil`
    :   Make numbered backups if the visited file already has numbered backups;
        otherwise, do not. This is the default.

    `never`
    :   Do not make numbered backups.

    anything else
    :   Make numbered backups.

The use of numbered backups ultimately leads to a large number of
backup versions, which must then be deleted. Emacs can do this
automatically or it can ask the user whether to delete them.

User Option: **kept-new-versions** [¶](#index-kept_002dnew_002dversions)
:   The value of this variable is the number of newest versions to keep
    when a new numbered backup is made. The newly made backup is included
    in the count. The default value is 2.

User Option: **kept-old-versions** [¶](#index-kept_002dold_002dversions)
:   The value of this variable is the number of oldest versions to keep
    when a new numbered backup is made. The default value is 2.

If there are backups numbered 1, 2, 3, 5, and 7, and both of these
variables have the value 2, then the backups numbered 1 and 2 are kept
as old versions and those numbered 5 and 7 are kept as new versions;
backup version 3 is excess. The function `find-backup-file-name`
(see [Naming Backup Files](#Backup-Names)) is responsible for determining which backup
versions to delete, but does not delete them itself.

User Option: **delete-old-versions** [¶](#index-delete_002dold_002dversions)
:   If this variable is `t`, then saving a file deletes excess
    backup versions silently. If it is `nil`, that means
    to ask for confirmation before deleting excess backups.
    Otherwise, they are not deleted at all.

User Option: **dired-kept-versions** [¶](#index-dired_002dkept_002dversions)
:   This variable specifies how many of the newest backup versions to keep
    in the Dired command `.` (`dired-clean-directory`). That’s the
    same thing `kept-new-versions` specifies when you make a new backup
    file. The default is 2.

---

Previous: [Making and Deleting Numbered Backup Files](#Numbered-Backups), Up: [Backup Files](#Backup-Files)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 27.1.4 Naming Backup Files [¶](#Naming-Backup-Files)

The functions in this section are documented mainly because you can
customize the naming conventions for backup files by redefining them.
If you change one, you probably need to change the rest.

Function: **backup-file-name-p** filename [¶](#index-backup_002dfile_002dname_002dp)
:   This function returns a non-`nil` value if filename is a
    possible name for a backup file. It just checks the name, not whether
    a file with the name filename exists.

    ```
    (backup-file-name-p "foo")
         ⇒ nil
    ```

    ```
    (backup-file-name-p "foo~")
         ⇒ 3
    ```

    The standard definition of this function is as follows:

    ```
    (defun backup-file-name-p (file)
      "Return non-nil if FILE is a backup file \
    name (numeric or not)..."
      (string-match "~\\'" file))
    ```

    Thus, the function returns a non-`nil` value if the file name ends
    with a ‘`~`’. (We use a backslash to split the documentation
    string’s first line into two lines in the text, but produce just one
    line in the string itself.)

    This simple expression is placed in a separate function to make it easy
    to redefine for customization.

Function: **make-backup-file-name** filename [¶](#index-make_002dbackup_002dfile_002dname)
:   This function returns a string that is the name to use for a
    non-numbered backup file for file filename. On Unix, this is just
    filename with a tilde appended.

    The standard definition of this function, on most operating systems, is
    as follows:

    ```
    (defun make-backup-file-name (file)
      "Create the non-numeric backup file name for FILE..."
      (concat file "~"))
    ```

    You can change the backup-file naming convention by redefining this
    function. The following example redefines `make-backup-file-name`
    to prepend a ‘`.`’ in addition to appending a tilde:

    ```
    (defun make-backup-file-name (filename)
      (expand-file-name
        (concat "." (file-name-nondirectory filename) "~")
        (file-name-directory filename)))
    ```

    ```

    ```

    ```
    (make-backup-file-name "backups.texi")
         ⇒ ".backups.texi~"
    ```

    Some parts of Emacs, including some Dired commands, assume that backup
    file names end with ‘`~`’. If you do not follow that convention, it
    will not cause serious problems, but these commands may give
    less-than-desirable results.

Function: **find-backup-file-name** filename [¶](#index-find_002dbackup_002dfile_002dname)
:   This function computes the file name for a new backup file for
    filename. It may also propose certain existing backup files for
    deletion. `find-backup-file-name` returns a list whose CAR is
    the name for the new backup file and whose CDR is a list of backup
    files whose deletion is proposed. The value can also be `nil`,
    which means not to make a backup.

    Two variables, `kept-old-versions` and `kept-new-versions`,
    determine which backup versions should be kept. This function keeps
    those versions by excluding them from the CDR of the value.
    See [Making and Deleting Numbered Backup Files](#Numbered-Backups).

    In this example, the value says that `~rms/foo.~5~` is the name
    to use for the new backup file, and `~rms/foo.~3~` is an excess
    version that the caller should consider deleting now.

    ```
    (find-backup-file-name "~rms/foo")
         ⇒ ("~rms/foo.~5~" "~rms/foo.~3~")
    ```

Function: **file-backup-file-names** filename [¶](#index-file_002dbackup_002dfile_002dnames)
:   This function returns a list of all the backup file names for
    filename, or `nil` if there are none. The files are sorted
    by modification time, descending, so that the most recent files are
    first.

Function: **file-newest-backup** filename [¶](#index-file_002dnewest_002dbackup)
:   This function returns the first element of the list returned by
    `file-backup-file-names`.

    Some file comparison commands use this function so that they can
    automatically compare a file with its most recent backup.

---

Next: [Reverting](#Reverting), Previous: [Backup Files](#Backup-Files), Up: [Backups and Auto-Saving](#Backups-and-Auto_002dSaving)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 27.2 Auto-Saving [¶](#Auto_002dSaving-1)

Emacs periodically saves all files that you are visiting; this is
called *auto-saving*. Auto-saving prevents you from losing more
than a limited amount of work if the system crashes. By default,
auto-saves happen every 300 keystrokes, or after around 30 seconds of
idle time. See [Auto-Saving: Protection Against
Disasters](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Auto-Save) in The GNU Emacs Manual, for information on auto-save
for users. Here we describe the functions used to implement auto-saving
and the variables that control them.

Variable: **buffer-auto-save-file-name** [¶](#index-buffer_002dauto_002dsave_002dfile_002dname)
:   This buffer-local variable is the name of the file used for
    auto-saving the current buffer. It is `nil` if the buffer
    should not be auto-saved.

    ```
    buffer-auto-save-file-name
         ⇒ "/xcssun/users/rms/lewis/#backups.texi#"
    ```

Command: **auto-save-mode** arg [¶](#index-auto_002dsave_002dmode)
:   This is the mode command for Auto Save mode, a buffer-local minor
    mode. When Auto Save mode is enabled, auto-saving is enabled in the
    buffer. The calling convention is the same as for other minor mode
    commands (see [Conventions for Writing Minor Modes](#Minor-Mode-Conventions)).

    Unlike most minor modes, there is no `auto-save-mode` variable.
    Auto Save mode is enabled if `buffer-auto-save-file-name` is
    non-`nil` and `buffer-saved-size` (see below) is non-zero.

Variable: **auto-save-file-name-transforms** [¶](#index-auto_002dsave_002dfile_002dname_002dtransforms)
:   This variable lists transforms to apply to buffer’s file name before
    making the auto-save file name.

    Each transform is a list of the form `(regexp replacement [uniquify])`. regexp is a regular
    expression to match against the file name; if it matches,
    `replace-match` is used to replace the matching part with
    replacement. If the optional element uniquify is non-`nil`,
    the auto-save file name is constructed by concatenating the directory
    part of the transformed file name with the buffer’s file name in which
    all directory separators were changed to ‘`!`’ to prevent clashes.
    (This will not work correctly if your filesystem truncates the
    resulting name.)

    If uniquify is one of the members of
    `secure-hash-algorithms`, Emacs constructs the nondirectory part
    of the auto-save file name by applying that `secure-hash` to the
    buffer file name. This avoids any risk of excessively long file
    names.

    All the transforms in the list are tried, in the order they are listed.
    When one transform applies, its result is final;
    no further transforms are tried.

    The default value is set up to put the auto-save files of remote files
    into the temporary directory (see [Generating Unique File Names](#Unique-File-Names)).

    On MS-DOS filesystems without long names this variable is always
    ignored.

Function: **auto-save-file-name-p** filename [¶](#index-auto_002dsave_002dfile_002dname_002dp)
:   This function returns a non-`nil` value if filename is a
    string that could be the name of an auto-save file. It assumes
    the usual naming convention for auto-save files: a name that
    begins and ends with hash marks (‘`#`’) is a possible auto-save file
    name. The argument filename should not contain a directory part.

    ```
    (make-auto-save-file-name)
         ⇒ "/xcssun/users/rms/lewis/#backups.texi#"
    ```

    ```
    (auto-save-file-name-p "#backups.texi#")
         ⇒ 0
    ```

    ```
    (auto-save-file-name-p "backups.texi")
         ⇒ nil
    ```

Function: **make-auto-save-file-name** [¶](#index-make_002dauto_002dsave_002dfile_002dname)
:   This function returns the file name to use for auto-saving the current
    buffer. This is just the file name with hash marks (‘`#`’) prepended
    and appended to it. This function does not look at the variable
    `auto-save-visited-file-name` (described below); callers of this
    function should check that variable first.

    ```
    (make-auto-save-file-name)
         ⇒ "/xcssun/users/rms/lewis/#backups.texi#"
    ```

User Option: **auto-save-visited-file-name** [¶](#index-auto_002dsave_002dvisited_002dfile_002dname)
:   If this variable is non-`nil`, Emacs auto-saves buffers in
    the files they are visiting. That is, the auto-save is done in the same
    file that you are editing. Normally, this variable is `nil`, so
    auto-save files have distinct names that are created by
    `make-auto-save-file-name`.

    When you change the value of this variable, the new value does not take
    effect in an existing buffer until the next time auto-save mode is
    reenabled in it. If auto-save mode is already enabled, auto-saves
    continue to go in the same file name until `auto-save-mode` is
    called again.

    Note that setting this variable to a non-`nil` value does not
    change the fact that auto-saving is different from saving the buffer;
    e.g., the hooks described in [Saving Buffers](#Saving-Buffers) are *not* run
    when a buffer is auto-saved.

Function: **recent-auto-save-p** [¶](#index-recent_002dauto_002dsave_002dp)
:   This function returns `t` if the current buffer has been
    auto-saved since the last time it was read in or saved.

Function: **set-buffer-auto-saved** [¶](#index-set_002dbuffer_002dauto_002dsaved)
:   This function marks the current buffer as auto-saved. The buffer will
    not be auto-saved again until the buffer text is changed again. The
    function returns `nil`.

User Option: **auto-save-interval** [¶](#index-auto_002dsave_002dinterval)
:   The value of this variable specifies how often to do auto-saving, in
    terms of number of input events. Each time this many additional input
    events are read, Emacs does auto-saving for all buffers in which that is
    enabled. Setting this to zero disables autosaving based on the
    number of characters typed.

User Option: **auto-save-timeout** [¶](#index-auto_002dsave_002dtimeout)
:   The value of this variable is the number of seconds of idle time that
    should cause auto-saving. Each time the user pauses for this long,
    Emacs does auto-saving for all buffers in which that is enabled. (If
    the current buffer is large, the specified timeout is multiplied by a
    factor that increases as the size increases; for a million-byte
    buffer, the factor is almost 4.)

    If the value is zero or `nil`, then auto-saving is not done as a
    result of idleness, only after a certain number of input events as
    specified by `auto-save-interval`.

Variable: **auto-save-hook** [¶](#index-auto_002dsave_002dhook)
:   This normal hook is run whenever an auto-save is about to happen.

User Option: **auto-save-default** [¶](#index-auto_002dsave_002ddefault)
:   If this variable is non-`nil`, buffers that are visiting files
    have auto-saving enabled by default. Otherwise, they do not.

Command: **do-auto-save** &optional no-message current-only [¶](#index-do_002dauto_002dsave)
:   This function auto-saves all buffers that need to be auto-saved. It
    saves all buffers for which auto-saving is enabled and that have been
    changed since the previous auto-save.

    If any buffers are auto-saved, `do-auto-save` normally displays a
    message saying ‘`Auto-saving...`’ in the echo area while
    auto-saving is going on. However, if no-message is
    non-`nil`, the message is inhibited.

    If current-only is non-`nil`, only the current buffer
    is auto-saved.

Function: **delete-auto-save-file-if-necessary** &optional force [¶](#index-delete_002dauto_002dsave_002dfile_002dif_002dnecessary)
:   This function deletes the current buffer’s auto-save file if
    `delete-auto-save-files` is non-`nil`. It is called every
    time a buffer is saved.

    Unless force is non-`nil`, this function only deletes the
    file if it was written by the current Emacs session since the last
    true save.

User Option: **delete-auto-save-files** [¶](#index-delete_002dauto_002dsave_002dfiles)
:   This variable is used by the function
    `delete-auto-save-file-if-necessary`. If it is non-`nil`,
    Emacs deletes auto-save files when a true save is done (in the visited
    file). This saves disk space and unclutters your directory.

Function: **rename-auto-save-file** [¶](#index-rename_002dauto_002dsave_002dfile)
:   This function adjusts the current buffer’s auto-save file name if the
    visited file name has changed. It also renames an existing auto-save
    file, if it was made in the current Emacs session. If the visited
    file name has not changed, this function does nothing.

Variable: **buffer-saved-size** [¶](#index-buffer_002dsaved_002dsize)
:   The value of this buffer-local variable is the length of the current
    buffer, when it was last read in, saved, or auto-saved. This is
    used to detect a substantial decrease in size, and turn off auto-saving
    in response.

    If it is −1, that means auto-saving is temporarily shut off in
    this buffer due to a substantial decrease in size. Explicitly saving
    the buffer stores a positive value in this variable, thus reenabling
    auto-saving. Turning auto-save mode off or on also updates this
    variable, so that the substantial decrease in size is forgotten.

    If it is −2, that means this buffer should disregard changes in
    buffer size; in particular, it should not shut off auto-saving
    temporarily due to changes in buffer size.

Variable: **auto-save-list-file-name** [¶](#index-auto_002dsave_002dlist_002dfile_002dname)
:   This variable (if non-`nil`) specifies a file for recording the
    names of all the auto-save files. Each time Emacs does auto-saving, it
    writes two lines into this file for each buffer that has auto-saving
    enabled. The first line gives the name of the visited file (it’s empty
    if the buffer has none), and the second gives the name of the auto-save
    file.

    When Emacs exits normally, it deletes this file; if Emacs crashes, you
    can look in the file to find all the auto-save files that might contain
    work that was otherwise lost. The `recover-session` command uses
    this file to find them.

    The default name for this file specifies your home directory and starts
    with ‘`.saves-`’. It also contains the Emacs process ID and the
    host name.

User Option: **auto-save-list-file-prefix** [¶](#index-auto_002dsave_002dlist_002dfile_002dprefix)
:   After Emacs reads your init file, it initializes
    `auto-save-list-file-name` (if you have not already set it
    non-`nil`) based on this prefix, adding the host name and process
    ID. If you set this to `nil` in your init file, then Emacs does
    not initialize `auto-save-list-file-name`.

---

Previous: [Auto-Saving](#Auto_002dSaving), Up: [Backups and Auto-Saving](#Backups-and-Auto_002dSaving)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 27.3 Reverting [¶](#Reverting-1)

If you have made extensive changes to a file and then change your mind
about them, you can get rid of them by reading in the previous version
of the file with the `revert-buffer` command. See [Reverting a Buffer](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Reverting) in The GNU Emacs Manual.

Command: **revert-buffer** &optional ignore-auto noconfirm preserve-modes [¶](#index-revert_002dbuffer)
:   This command replaces the buffer text with the text of the visited
    file on disk. This action undoes all changes since the file was visited
    or saved.

    By default, if the latest auto-save file is more recent than the visited
    file, and the argument ignore-auto is `nil`,
    `revert-buffer` asks the user whether to use that auto-save
    instead. When you invoke this command interactively, ignore-auto
    is `t` if there is no numeric prefix argument; thus, the
    interactive default is not to check the auto-save file.

    Normally, `revert-buffer` asks for confirmation before it changes
    the buffer; but if the argument noconfirm is non-`nil`,
    `revert-buffer` does not ask for confirmation.

    Normally, this command reinitializes the buffer’s major and minor modes
    using `normal-mode`. But if preserve-modes is
    non-`nil`, the modes remain unchanged.

    Reverting tries to preserve marker positions in the buffer by using the
    replacement feature of `insert-file-contents`. If the buffer
    contents and the file contents are identical before the revert
    operation, reverting preserves all the markers. If they are not
    identical, reverting does change the buffer; in that case, it preserves
    the markers in the unchanged text (if any) at the beginning and end of
    the buffer. Preserving any additional markers would be problematic.

    When reverting from non-file sources, markers are usually not
    preserved, but this is up to the specific `revert-buffer-function`
    implementation.

Variable: **revert-buffer-in-progress** [¶](#index-revert_002dbuffer_002din_002dprogress)
:   `revert-buffer` binds this variable to a non-`nil` value
    while it is working.

You can customize how `revert-buffer` does its work by setting
the variables described in the rest of this section.

User Option: **revert-without-query** [¶](#index-revert_002dwithout_002dquery)
:   This variable holds a list of files that should be reverted without
    query. The value is a list of regular expressions. If the visited file
    name matches one of these regular expressions, and the file has changed
    on disk but the buffer is not modified, then `revert-buffer`
    reverts the file without asking the user for confirmation.

Some major modes customize `revert-buffer` by making
buffer-local bindings for these variables:

Variable: **revert-buffer-function** [¶](#index-revert_002dbuffer_002dfunction)
:   The value of this variable is the function to use to revert this
    buffer. It should be a function with two optional
    arguments to do the work of reverting. The two optional arguments,
    ignore-auto and noconfirm, are the arguments that
    `revert-buffer` received.

    Modes such as Dired mode, in which the text being edited does not
    consist of a file’s contents but can be regenerated in some other
    fashion, can give this variable a buffer-local value that is a special
    function to regenerate the contents.

Variable: **revert-buffer-insert-file-contents-function** [¶](#index-revert_002dbuffer_002dinsert_002dfile_002dcontents_002dfunction)
:   The value of this variable specifies the function to use to
    insert the updated contents when reverting this buffer. The function
    receives two arguments: first the file name to use; second, `t` if
    the user has asked to read the auto-save file.

    The reason for a mode to change this variable instead of
    `revert-buffer-function` is to avoid duplicating or replacing the
    rest of what `revert-buffer` does: asking for confirmation,
    clearing the undo list, deciding the proper major mode, and running the
    hooks listed below.

Variable: **before-revert-hook** [¶](#index-before_002drevert_002dhook)
:   This normal hook is run by the default `revert-buffer-function`
    before inserting the modified contents. A custom `revert-buffer-function`
    may or may not run this hook.

Variable: **after-revert-hook** [¶](#index-after_002drevert_002dhook)
:   This normal hook is run by the default `revert-buffer-function`
    after inserting the modified contents. A custom `revert-buffer-function`
    may or may not run this hook.

Variable: **revert-buffer-restore-functions** [¶](#index-revert_002dbuffer_002drestore_002dfunctions)
:   The value of this variable specifies a list of functions that preserve
    the state of the buffer. Before the revert operation each function from
    this list is called without arguments, and it should return a lambda
    that preserves some particular state (for example, the read-only state).
    After the revert operation each lambda will be called one by one in the
    order of the list, and it should restore the saved state in the reverted
    buffer.

Emacs can revert buffers automatically. It does that by default for
buffers visiting files. The following describes how to add support
for auto-reverting new types of buffers.

First, such buffers must have a suitable `revert-buffer-function`
and `buffer-stale-function` defined.

Variable: **buffer-stale-function** [¶](#index-buffer_002dstale_002dfunction)
:   The value of this variable specifies a function to call to check
    whether a buffer needs reverting. The default value only handles
    buffers that are visiting files, by checking their modification time.
    Buffers that are not visiting files require a custom function of one
    optional argument noconfirm. The function should return
    non-`nil` if the buffer should be reverted. The buffer is
    current when this function is called.

    While this function is mainly intended for use in auto-reverting, it
    could be used for other purposes as well. For instance, if
    auto-reverting is not enabled, it could be used to warn the user that
    the buffer needs reverting. The idea behind the noconfirm
    argument is that it should be `t` if the buffer is going to be
    reverted without asking the user and `nil` if the function is
    just going to be used to warn the user that the buffer is out of date.
    In particular, for use in auto-reverting, noconfirm is `t`.
    If the function is only going to be used for auto-reverting, you can
    ignore the noconfirm argument.

    If you just want to automatically auto-revert every
    `auto-revert-interval` seconds (like the Buffer Menu), use:

    ```
    (setq-local buffer-stale-function
         (lambda (&optional noconfirm) 'fast))
    ```

    in the buffer’s mode function.

    The special return value ‘`fast`’ tells the caller that the need
    for reverting was not checked, but that reverting the buffer is fast.
    It also tells Auto Revert not to print any revert messages, even if
    `auto-revert-verbose` is non-`nil`. This is important, as
    getting revert messages every `auto-revert-interval` seconds can
    be very annoying. The information provided by this return value could
    also be useful if the function is consulted for purposes other than
    auto-reverting.

Once the buffer has a suitable `revert-buffer-function` and
`buffer-stale-function`, several problems usually remain.

The buffer will only auto-revert if it is marked unmodified. Hence,
you will have to make sure that various functions mark the buffer
modified if and only if either the buffer contains information that
might be lost by reverting, or there is reason to believe that the user
might be inconvenienced by auto-reverting, because he is actively
working on the buffer. The user can always override this by manually
adjusting the modified status of the buffer. To support this, calling
the `revert-buffer-function` on a buffer that is marked
unmodified should always keep the buffer marked unmodified.

It is important to assure that point does not continuously jump around
as a consequence of auto-reverting. Of course, moving point might be
inevitable if the buffer radically changes.

Variable: **auto-revert-buffer-in-progress** [¶](#index-auto_002drevert_002dbuffer_002din_002dprogress)
:   `auto-revert-buffer` binds this variable to a non-`nil` value
    while it is working. This can be used by major mode
    `revert-buffer-function` implementations to suppress messages in
    Auto Revert modes, for example.

Variable: **inhibit-auto-revert-buffers** [¶](#index-inhibit_002dauto_002drevert_002dbuffers)
:   When the current buffer is member of this variable (a list of buffers),
    auto-reverting is suppressed for that buffer. This is useful if serious
    changes are applied to that buffer which would be poisoned by an
    unexpected auto-revert. After the change is finished, the buffer shall
    be removed from `inhibit-auto-revert-buffers`.

    The check of membership in `inhibit-auto-revert-buffers` is applied
    prior to the call of `buffer-stale-function`; any heavy check in
    that function is avoided, therefore.

    If auto-reverting is triggered by file notification while
    `inhibit-auto-revert-buffers` prevents this, auto-revert will
    happen next time the buffer is polled for changes, unless
    `auto-revert-avoid-polling` is non-`nil`. see [(emacs)Auto
    Revert](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Auto-Revert).

Macro: **inhibit-auto-revert** &rest body [¶](#index-inhibit_002dauto_002drevert)
:   This macro adds the current buffer to
    `inhibit-auto-revert-buffers`, runs body, and removes the
    current buffer from `inhibit-auto-revert-buffers` afterwards.

You should make sure that the `revert-buffer-function` does not
print messages that unnecessarily duplicate Auto Revert’s own messages,
displayed if `auto-revert-verbose` is `t`, and effectively
override a `nil` value for `auto-revert-verbose`. Hence,
adapting a mode for auto-reverting often involves getting rid of such
messages. This is especially important for buffers that automatically
revert every `auto-revert-interval` seconds.

If the new auto-reverting is part of Emacs, you should mention it
in the documentation string of `global-auto-revert-non-file-buffers`.

Similarly, you should document the additions in the Emacs manual.

---

Next: [Windows](#Windows), Previous: [Backups and Auto-Saving](#Backups-and-Auto_002dSaving), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 28 Buffers [¶](#Buffers-1)

A *buffer* is a Lisp object containing text to be edited. Buffers
are used to hold the contents of files that are being visited; there may
also be buffers that are not visiting files. While several buffers may
exist at one time, only one buffer is designated the *current
buffer* at any time. Most editing commands act on the contents of the
current buffer. Each buffer, including the current buffer, may or may
not be displayed in any windows.

* [Buffer Basics](#Buffer-Basics)
* [The Current Buffer](#Current-Buffer)
* [Buffer Names](#Buffer-Names)
* [Buffer File Name](#Buffer-File-Name)
* [Buffer Modification](#Buffer-Modification)
* [Buffer Modification Time](#Modification-Time)
* [Read-Only Buffers](#Read-Only-Buffers)
* [The Buffer List](#Buffer-List)
* [Creating Buffers](#Creating-Buffers)
* [Killing Buffers](#Killing-Buffers)
* [Indirect Buffers](#Indirect-Buffers)
* [Swapping Text Between Two Buffers](#Swapping-Text)
* [The Buffer Gap](#Buffer-Gap)

---

Next: [The Current Buffer](#Current-Buffer), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.1 Buffer Basics [¶](#Buffer-Basics-1)

A *buffer* is a Lisp object containing text to be edited. Buffers
are used to hold the contents of files that are being visited; there may
also be buffers that are not visiting files. Although several buffers
normally exist, only one buffer is designated the *current
buffer* at any time. Most editing commands act on the contents of the
current buffer. Each buffer, including the current buffer, may or may
not be displayed in any windows.

Buffers in Emacs editing are objects that have distinct names and hold
text that can be edited. Buffers appear to Lisp programs as a special
data type. You can think of the contents of a buffer as a string that
you can extend; insertions and deletions may occur in any part of the
buffer. See [Text](#Text).

A Lisp buffer object contains numerous pieces of information. Some of
this information is directly accessible to the programmer through
variables, while other information is accessible only through
special-purpose functions. For example, the visited file name is
directly accessible through a variable, while the value of point is
accessible only through a primitive function.

Buffer-specific information that is directly accessible is stored in
*buffer-local* variable bindings, which are variable values that are
effective only in a particular buffer. This feature allows each buffer
to override the values of certain variables. Most major modes override
variables such as `fill-column` or `comment-column` in this
way. For more information about buffer-local variables and functions
related to them, see [Buffer-Local Variables](#Buffer_002dLocal-Variables).

For functions and variables related to visiting files in buffers, see
[Visiting Files](#Visiting-Files) and [Saving Buffers](#Saving-Buffers). For functions and
variables related to the display of buffers in windows, see
[Buffers and Windows](#Buffers-and-Windows).

Function: **bufferp** object [¶](#index-bufferp)
:   This function returns `t` if object is a buffer,
    `nil` otherwise.

---

Next: [Buffer Names](#Buffer-Names), Previous: [Buffer Basics](#Buffer-Basics), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 28.2 The Current Buffer [¶](#The-Current-Buffer)

There are, in general, many buffers in an Emacs session. At any
time, one of them is designated the *current buffer*—the buffer
in which most editing takes place. Most of the primitives for
examining or changing text operate implicitly on the current buffer
(see [Text](#Text)).

Normally, the buffer displayed in the selected window
(see [Selecting Windows](#Selecting-Windows)) is the current buffer, but this is not
always so: a Lisp program can temporarily designate any buffer as
current in order to operate on its contents, without changing what is
displayed on the screen. The most basic function for designating a
current buffer is `set-buffer`.

Function: **current-buffer** [¶](#index-current_002dbuffer)
:   This function returns the current buffer.

    ```
    (current-buffer)
         ⇒ #<buffer buffers.texi>
    ```

Function: **set-buffer** buffer-or-name [¶](#index-set_002dbuffer)
:   This function makes buffer-or-name the current buffer.
    buffer-or-name must be an existing buffer or the name of an
    existing buffer. The return value is the buffer made current.

    This function does not display the buffer in any window, so the user
    cannot necessarily see the buffer. But Lisp programs will now operate
    on it.

When an editing command returns to the editor command loop, Emacs
automatically calls `set-buffer` on the buffer shown in the
selected window (see [Selecting Windows](#Selecting-Windows)). This is to prevent
confusion: it ensures that the buffer that the cursor is in, when Emacs
reads a command, is the buffer to which that command applies
(see [Command Loop](#Command-Loop)). Thus, you should not use `set-buffer` to
switch visibly to a different buffer; for that, use the functions
described in [Switching to a Buffer in a Window](#Switching-Buffers).

When writing a Lisp function, do *not* rely on this behavior of
the command loop to restore the current buffer after an operation.
Editing commands can also be called as Lisp functions by other
programs, not just from the command loop; it is convenient for the
caller if the subroutine does not change which buffer is current
(unless, of course, that is the subroutine’s purpose).

To operate temporarily on another buffer, put the `set-buffer`
within a `save-current-buffer` form. Here, as an example, is a
simplified version of the command `append-to-buffer`:

```
(defun append-to-buffer (buffer start end)
  "Append the text of the region to BUFFER."
  (interactive "BAppend to buffer: \nr")
  (let ((oldbuf (current-buffer)))
    (save-current-buffer
      (set-buffer (get-buffer-create buffer))
      (insert-buffer-substring oldbuf start end))))
```

Here, we bind a local variable to record the current buffer, and then
`save-current-buffer` arranges to make it current again later.
Next, `set-buffer` makes the specified buffer current, and
`insert-buffer-substring` copies the string from the original
buffer to the specified (and now current) buffer.

Alternatively, we can use the `with-current-buffer` macro:

```
(defun append-to-buffer (buffer start end)
  "Append the text of the region to BUFFER."
  (interactive "BAppend to buffer: \nr")
  (let ((oldbuf (current-buffer)))
    (with-current-buffer (get-buffer-create buffer)
      (insert-buffer-substring oldbuf start end))))
```

In either case, if the buffer appended to happens to be displayed in
some window, the next redisplay will show how its text has changed.
If it is not displayed in any window, you will not see the change
immediately on the screen. The command causes the buffer to become
current temporarily, but does not cause it to be displayed.

If you make local bindings (with `let` or function arguments)
for a variable that may also have buffer-local bindings, make sure
that the same buffer is current at the beginning and at the end of the
local binding’s scope. Otherwise you might bind it in one buffer and
unbind it in another!

Do not rely on using `set-buffer` to change the current buffer
back, because that won’t do the job if a quit happens while the wrong
buffer is current. For instance, in the previous example, it would
have been wrong to do this:

```
  (let ((oldbuf (current-buffer)))
    (set-buffer (get-buffer-create buffer))
    (insert-buffer-substring oldbuf start end)
    (set-buffer oldbuf))
```

Using `save-current-buffer` or `with-current-buffer`, as we
did, correctly handles quitting, errors, and `throw`, as well as
ordinary evaluation.

Special Form: **save-current-buffer** body… [¶](#index-save_002dcurrent_002dbuffer)
:   The `save-current-buffer` special form saves the identity of the
    current buffer, evaluates the body forms, and finally restores
    that buffer as current. The return value is the value of the last
    form in body. The current buffer is restored even in case of an
    abnormal exit via `throw` or error (see [Nonlocal Exits](#Nonlocal-Exits)).

    If the buffer that used to be current has been killed by the time of
    exit from `save-current-buffer`, then it is not made current again,
    of course. Instead, whichever buffer was current just before exit
    remains current.

Macro: **with-current-buffer** buffer-or-name body… [¶](#index-with_002dcurrent_002dbuffer)
:   The `with-current-buffer` macro saves the identity of the current
    buffer, makes buffer-or-name current, evaluates the body
    forms, and finally restores the current buffer. buffer-or-name
    must specify an existing buffer or the name of an existing buffer.

    The return value is the value of the last form in body. The
    current buffer is restored even in case of an abnormal exit via
    `throw` or error (see [Nonlocal Exits](#Nonlocal-Exits)).

Macro: **with-temp-buffer** body… [¶](#index-with_002dtemp_002dbuffer)
:   The `with-temp-buffer` macro evaluates the body forms with
    a temporary buffer as the current buffer. It saves the identity of
    the current buffer, creates a temporary buffer and makes it current,
    evaluates the body forms, and finally restores the previous
    current buffer while killing the temporary buffer.

    By default, undo information (see [Undo](#Undo)) is not recorded in the
    buffer created by this macro (but body can enable that, if
    needed). The temporary buffer also does not run the hooks
    `kill-buffer-hook`, `kill-buffer-query-functions`
    (see [Killing Buffers](#Killing-Buffers)), and `buffer-list-update-hook`
    (see [The Buffer List](#Buffer-List)).

    The return value is the value of the last form in body. You can
    return the contents of the temporary buffer by using
    `(buffer-string)` as the last form.

    The current buffer is restored even in case of an abnormal exit via
    `throw` or error (see [Nonlocal Exits](#Nonlocal-Exits)).

    See also `with-temp-file` in [Writing to Files](#Definition-of-with_002dtemp_002dfile).

---

Next: [Buffer File Name](#Buffer-File-Name), Previous: [The Current Buffer](#Current-Buffer), Up: [Buffers](#Buffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
