### 41.1 Functions that Create Subprocesses [¶](#Functions-that-Create-Subprocesses)

There are three primitives that create a new subprocess in which to run
a program. One of them, `make-process`, creates an asynchronous
process and returns a process object (see [Creating an Asynchronous Process](#Asynchronous-Processes)).
The other two, `call-process` and `call-process-region`,
create a synchronous process and do not return a process object
(see [Creating a Synchronous Process](#Synchronous-Processes)). There are various higher-level
functions that make use of these primitives to run particular types of
process.

Synchronous and asynchronous processes are explained in the following
sections. Since the three functions are all called in a similar
fashion, their common arguments are described here.

In all cases, the functions specify the program to be run. An error
is signaled if the file is not found or cannot be executed. If the
file name is relative, the variable `exec-path` contains a list
of directories to search. Emacs initializes `exec-path` when it
starts up, based on the value of the environment variable `PATH`.
The standard file name constructs, ‘`~`’, ‘`.`’, and ‘`..`’,
are interpreted as usual in `exec-path`, but environment variable
substitutions (‘`$HOME`’, etc.) are not recognized; use
`substitute-in-file-name` to perform them (see [Functions that Expand Filenames](#File-Name-Expansion)). `nil` in this list refers to
`default-directory`.

Executing a program can also try adding suffixes to the specified
name:

User Option: **exec-suffixes** [¶](#index-exec_002dsuffixes-1)
:   This variable is a list of suffixes (strings) to try adding to the
    specified program file name. The list should include `""` if you
    want the name to be tried exactly as specified. The default value is
    system-dependent.

Function: **exec-suffixes** [¶](#index-exec_002dsuffixes)
:   This function returns the connection-local value of variable
    `exec-suffixes`.

**Please note:** The argument program contains only the
name of the program file; it may not contain any command-line
arguments. You must use a separate argument, args, to provide
those, as described below.

Each of the subprocess-creating functions has a buffer-or-name
argument that specifies where the output from the program will go. It
should be a buffer or a buffer name; if it is a buffer name, that will
create the buffer if it does not already exist. It can also be
`nil`, which says to discard the output, unless a custom filter
function handles it. (See [Process Filter Functions](#Filter-Functions), and [Reading and Printing Lisp Objects](#Read-and-Print).) Normally, you should avoid having multiple processes send
output to the same buffer because their output would be intermixed
randomly. For synchronous processes, you can send the output to a
file instead of a buffer (and the corresponding argument is therefore
more appropriately called destination). By default, both
standard output and standard error streams go to the same destination,
but all the 3 primitives allow optionally to direct the standard error
stream to a different destination.

All three of the subprocess-creating functions allow specifying
command-line arguments for the process to run. For `call-process`
and `call-process-region`, these come in the form of a
`&rest` argument, args. For `make-process`, both the
program to run and its command-line arguments are specified as a list
of strings. The command-line arguments must all be strings, and they
are supplied to the program as separate argument strings. Wildcard
characters and other shell constructs have no special meanings in
these strings, since the strings are passed directly to the specified
program.

The subprocess inherits its environment from Emacs, but you can
specify overrides for it with `process-environment`. See [Operating System Environment](#System-Environment). The subprocess gets its current directory from the
value of `default-directory`.

Variable: **exec-directory** [¶](#index-exec_002ddirectory)
:   The value of this variable is a string, the name of a directory that
    contains programs that come with GNU Emacs and are intended for Emacs
    to invoke. The program `movemail` is an example of such a program;
    Rmail uses it to fetch new mail from an inbox.

User Option: **exec-path** [¶](#index-exec_002dpath-1)
:   The value of this variable is a list of directories to search for
    programs to run in subprocesses. Each element is either the name of a
    directory (i.e., a string), or `nil`, which stands for the default
    directory (which is the value of `default-directory`).
    See [executable-find](#Locating-Files), for the details of this search.

    The value of `exec-path` is used by `call-process` and
    `start-process` when the program argument is not an absolute
    file name.

    Generally, you should not modify `exec-path` directly. Instead,
    ensure that your `PATH` environment variable is set appropriately
    before starting Emacs. Trying to modify `exec-path`
    independently of `PATH` can lead to confusing results.

Function: **exec-path** [¶](#index-exec_002dpath)
:   This function is an extension of the variable `exec-path`. If
    `default-directory` indicates a remote directory, this function
    returns a list of directories used for searching programs on the
    respective remote host. In case of a local `default-directory`,
    the function returns just the value of the variable `exec-path`.
    If you use `exec-suffixes` together with this function, you must
    also use the function `exec-suffixes` instead of the variable.

When starting a program that is part of the Emacs distribution, you
must take into account that the program may have been renamed in order
to comply with executable naming restrictions present on the system.

Instead of starting `emacsclient`, for example, you should specify
the value of `emacsclient-program-name` instead. Likewise, instead of
starting `movemail`, you must start
`movemail-program-name`, and the same goes for `etags`,
`hexl`, `rcs2log`, and
`ebrowse`.

---

Next: [Creating a Synchronous Process](#Synchronous-Processes), Previous: [Functions that Create Subprocesses](#Subprocess-Creation), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.2 Shell Arguments [¶](#Shell-Arguments-1)

Lisp programs sometimes need to run a shell and give it a command
that contains file names that were specified by the user. These
programs ought to be able to support any valid file name. But the shell
gives special treatment to certain characters, and if these characters
occur in the file name, they will confuse the shell. To handle these
characters, use the function `shell-quote-argument`:

Function: **shell-quote-argument** argument &optional posix [¶](#index-shell_002dquote_002dargument)
:   This function returns a string that represents, in shell syntax,
    an argument whose actual contents are argument. It should
    work reliably to concatenate the return value into a shell command
    and then pass it to a shell for execution.

    Precisely what this function does depends on your operating system. The
    function is designed to work with the syntax of your system’s standard
    shell; if you use an unusual shell, you will need to redefine this
    function. See [Security Considerations](#Security-Considerations).

    ```
    ;; This example shows the behavior on GNU and Unix systems.
    (shell-quote-argument "foo > bar")
         ⇒ "foo\\ \\>\\ bar"

    ;; This example shows the behavior on MS-DOS and MS-Windows.
    (shell-quote-argument "foo > bar")
         ⇒ "\"foo > bar\""
    ```

    Here’s an example of using `shell-quote-argument` to construct
    a shell command:

    ```
    (concat "diff -u "
            (shell-quote-argument oldfile)
            " "
            (shell-quote-argument newfile))
    ```

    If the optional posix argument is non-`nil`, argument
    is quoted according to POSIX shell quoting rules, regardless of the
    system’s shell. This is useful when your shell could run on a remote
    host, which requires a POSIX shell in general.

    ```
    (shell-quote-argument "foo > bar" (file-remote-p default-directory))
    ```

The following two functions are useful for combining a list of
individual command-line argument strings into a single string, and
taking a string apart into a list of individual command-line
arguments. These functions are mainly intended for converting user
input in the minibuffer, a Lisp string, into a list of string
arguments to be passed to `make-process`, `call-process` or
`start-process`, or for converting such lists of arguments into a
single Lisp string to be presented in the minibuffer or echo area.
Note that if a shell is involved (e.g., if using
`call-process-shell-command`), arguments should still be
protected by `shell-quote-argument`;
`combine-and-quote-strings` is *not* intended to protect
special characters from shell evaluation.

Function: **split-string-shell-command** string [¶](#index-split_002dstring_002dshell_002dcommand)
:   This function splits string into substrings, respecting double
    and single quotes, as well as backslash quoting.

    ```
    (split-string-shell-command "ls /tmp/'foo bar'")
         ⇒ ("ls" "/tmp/foo bar")
    ```

Function: **split-string-and-unquote** string &optional separators [¶](#index-split_002dstring_002dand_002dunquote)
:   This function splits string into substrings at matches for the
    regular expression separators, like `split-string` does
    (see [Creating Strings](#Creating-Strings)); in addition, it removes quoting from the
    substrings. It then makes a list of the substrings and returns it.

    If separators is omitted or `nil`, it defaults to
    `"\\s-+"`, which is a regular expression that matches one or more
    characters with whitespace syntax (see [Table of Syntax Classes](#Syntax-Class-Table)).

    This function supports two types of quoting: enclosing a whole string
    in double quotes `"…"`, and quoting individual characters
    with a backslash escape ‘`\`’. The latter is also used in Lisp
    strings, so this function can handle those as well.

Function: **combine-and-quote-strings** list-of-strings &optional separator [¶](#index-combine_002dand_002dquote_002dstrings)
:   This function concatenates list-of-strings into a single string,
    quoting each string as necessary. It also sticks the separator
    string between each pair of strings; if separator is omitted or
    `nil`, it defaults to `" "`. The return value is the
    resulting string.

    The strings in list-of-strings that need quoting are those that
    include separator as their substring. Quoting a string encloses
    it in double quotes `"…"`. In the simplest case, if you
    are consing a command from the individual command-line arguments,
    every argument that includes embedded blanks will be quoted.

Systems running Emacs impose a limit on the maximum total length of a
command and its arguments submitted for execution. If a list of
arguments to be processed by an external command might be arbitrarily
long, then there is a risk of exceeding this limit, in which case
executing the command will fail. In some cases this problem can be
avoided by running the command multiple times on subsequent partitions
of the list of arguments. When you know that it will not cause problems
to run the command more than once, you can use this function to
preemptively partition a list of arguments:

Function: **multiple-command-partition-arguments** command arguments &optional shellp [¶](#index-multiple_002dcommand_002dpartition_002darguments)
:   This function returns a list of subsequent partitions of
    arguments, a list of strings. command is a string or list
    of strings specifying the command to be run on arguments (unless
    shellp is non-nil, see below), and including any arguments that
    must be passed to every invocation of the command. Then, running a
    command formed of command plus any one of the returned partitions
    will not exceed the system’s limits on the maximum total length of a
    command and its arguments.

    If optional argument shellp is non-`nil`, this function
    implicitly includes `shell-file-name` and
    `shell-command-switch` at the beginning of command
    (see [Creating an Asynchronous Process](#Asynchronous-Processes)). In addition, in that case, this
    function accounts for the extra length taken up by quoting and space
    characters in its partitioning calculations.

    On systems where the `process-environment` (see [Operating System Environment](#System-Environment)) counts against command line length limits, this function
    takes that into account too.

    In the following example we impose an artificially small limit on
    command line length and provide a fixed `process-environment` for
    demonstration purposes. The partitioning of the list of arguments into
    two sublists in the returned value implies that running `cmd foo bar baz` on this hypothetical system would exceed system limits, and
    thus fail. The calling code should run `cmd` twice, on each of the
    two partitions, so `cmd foo bar` and then `cmd baz`.

    ```
    (let ((system-type 'gnu/linux)
          (command-line-max-length 20)
          (process-environment '("BLAH=blah")))
      (multiple-command-partition-arguments "cmd" '("foo" "bar" "baz")))
         ⇒ (("foo" "bar") ("baz"))
    ```

    This function takes a conservative approach and does not try to minimize
    the number of partitions of arguments, because doing that would
    require a great deal of platform-specific information, and also
    information about encoding which is not currently made available to
    Lisp.

Variable: **command-line-max-length** [¶](#index-command_002dline_002dmax_002dlength)
:   This variable holds the maximum length of a command and its arguments on
    this system, measured in characters, as used by
    `multiple-command-partition-arguments`.

---

Next: [Creating an Asynchronous Process](#Asynchronous-Processes), Previous: [Shell Arguments](#Shell-Arguments), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.3 Creating a Synchronous Process [¶](#Creating-a-Synchronous-Process)

After a *synchronous process* is created, Emacs waits for the
process to terminate before continuing. Starting Dired on GNU or
Unix[31](#FOOT31) is an example of this: it
runs `ls` in a synchronous process, then modifies the output
slightly. Because the process is synchronous, the entire directory
listing arrives in the buffer before Emacs tries to do anything with it.

While Emacs waits for the synchronous subprocess to terminate, the
user can quit by typing `C-g`. The first `C-g` tries to kill
the subprocess with a `SIGINT` signal; but it waits until the
subprocess actually terminates before quitting. If during that time the
user types another `C-g`, that kills the subprocess instantly with
`SIGKILL` and quits immediately (except on MS-DOS, where killing
other processes doesn’t work). See [Quitting](#Quitting).

The synchronous subprocess functions return an indication of how the
process terminated.

The output from a synchronous subprocess is generally decoded using a
coding system, much like text read from a file. The input sent to a
subprocess by `call-process-region` is encoded using a coding
system, much like text written into a file. See [Coding Systems](#Coding-Systems).

Function: **call-process** program &optional infile destination display &rest args [¶](#index-call_002dprocess)
:   This function calls program and waits for it to finish.

    The current working directory of the subprocess is set to the current
    buffer’s value of `default-directory` if that is local (as
    determined by `unhandled-file-name-directory`), or "~" otherwise.
    If you want to run a process in a remote directory use
    `process-file`.

    The standard input for the new process comes from file infile if
    infile is not `nil`, and from the null device otherwise.
    The argument destination says where to put the process output.
    Here are the possibilities:

    a buffer
    :   Insert the output in that buffer, before point. This includes both the
        standard output stream and the standard error stream of the process.

    a buffer name (a string)
    :   Insert the output in a buffer with that name, before point.

    `t`
    :   Insert the output in the current buffer, before point.

    `nil`
    :   Discard the output.

    0
    :   Discard the output, and return `nil` immediately without waiting
        for the subprocess to finish.

        In this case, the process is not truly synchronous, since it can run in
        parallel with Emacs; but you can think of it as synchronous in that
        Emacs is essentially finished with the subprocess as soon as this
        function returns.

        MS-DOS doesn’t support asynchronous subprocesses, so this option doesn’t
        work there.

    `(:file file-name)`
    :   Send the output to the file name specified, overwriting it if it
        already exists.

    `(real-destination error-destination)`
    :   Keep the standard output stream separate from the standard error stream;
        deal with the ordinary output as specified by real-destination,
        and dispose of the error output according to error-destination.
        If error-destination is `nil`, that means to discard the
        error output, `t` means mix it with the ordinary output, and a
        string specifies a file name to redirect error output into.

        You can’t directly specify a buffer to put the error output in; that is
        too difficult to implement. But you can achieve this result by sending
        the error output to a temporary file and then inserting the file into a
        buffer when the subprocess finishes.

    If display is non-`nil`, then `call-process` redisplays
    the buffer as output is inserted. (However, if the coding system chosen
    for decoding output is `undecided`, meaning deduce the encoding
    from the actual data, then redisplay sometimes cannot continue once
    non-ASCII characters are encountered. There are fundamental
    reasons why it is hard to fix this; see [Receiving Output from Processes](#Output-from-Processes).)

    Otherwise the function `call-process` does no redisplay, and the
    results become visible on the screen only when Emacs redisplays that
    buffer in the normal course of events.

    The remaining arguments, args, are strings that specify command
    line arguments for the program. Each string is passed to
    program as a separate argument.

    The value returned by `call-process` (unless you told it not to
    wait) indicates the reason for process termination. A number gives the
    exit status of the subprocess; 0 means success, and any other value
    means failure. If the process terminated with a signal,
    `call-process` returns a string describing the signal. If you
    told `call-process` not to wait, it returns `nil`.

    In the examples below, the buffer ‘`foo`’ is current.

    ```
    (call-process "pwd" nil t)
         ⇒ 0

    ---------- Buffer: foo ----------
    /home/lewis/manual
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (call-process "grep" nil "bar" nil "lewis" "/etc/passwd")
         ⇒ 0

    ---------- Buffer: bar ----------
    lewis:x:1001:1001:Bil Lewis,,,,:/home/lewis:/bin/bash

    ---------- Buffer: bar ----------
    ```

    Here is an example of the use of `call-process`, as used to
    be found in the definition of the `insert-directory` function:

    ```
    (call-process insert-directory-program nil t nil switches
                  (if full-directory-p
                      (concat (file-name-as-directory file) ".")
                    file))
    ```

Function: **process-file** program &optional infile buffer display &rest args [¶](#index-process_002dfile)
:   This function processes files synchronously in a separate process. It
    is similar to `call-process`, but may invoke a file name handler
    based on the value of the variable `default-directory`, which
    specifies the current working directory of the subprocess.

    The arguments are handled in almost the same way as for
    `call-process`, with the following differences:

    Some file name handlers may not support all combinations and forms of the
    arguments infile, buffer, and display. For example,
    some file name handlers might behave as if display were `nil`,
    regardless of the value actually passed. As another example, some
    file name handlers might not support separating standard output and error
    output by way of the buffer argument.

    If a file name handler is invoked, it determines the program to run based
    on the first argument program. For instance, suppose that a
    handler for remote files is invoked. Then the path that is used for
    searching for the program might be different from `exec-path`.

    The second argument infile may invoke a file name handler. The file
    name handler could be different from the handler chosen for the
    `process-file` function itself. (For example,
    `default-directory` could be on one remote host, and
    infile on a different remote host. Or `default-directory`
    could be non-special, whereas infile is on a remote host.)

    If buffer is a list of the form `(real-destination
    error-destination)`, and error-destination names a file,
    then the same remarks as for infile apply.

    The remaining arguments (args) will be passed to the process
    verbatim. Emacs is not involved in processing file names that are
    present in args. To avoid confusion, it may be best to avoid
    absolute file names in args, but rather to specify all file
    names as relative to `default-directory`. The function
    `file-relative-name` is useful for constructing such relative
    file names. Alternatively, you can use `file-local-name`
    (see [Making Certain File Names “Magic”](#Magic-File-Names)) to obtain an absolute file name as seen
    from the remote host’s perspective.

Variable: **process-file-side-effects** [¶](#index-process_002dfile_002dside_002deffects)
:   This variable indicates whether a call of `process-file` changes
    remote files.

    By default, this variable is always set to `t`, meaning that a
    call of `process-file` could potentially change any file on a
    remote host. When set to `nil`, a file name handler could optimize
    its behavior with respect to remote file attribute caching.

    You should only ever change this variable with a let-binding; never
    with `setq`.

User Option: **process-file-return-signal-string** [¶](#index-process_002dfile_002dreturn_002dsignal_002dstring)
:   This user option indicates whether a call of `process-file`
    returns a string describing the signal interrupting a remote process.

    When a process returns an exit code greater than 128, it is
    interpreted as a signal. `process-file` requires returning a
    string describing this signal.

    Since there are processes violating this rule, returning exit codes
    greater than 128 which are not bound to a signal, `process-file`
    returns always the exit code as natural number for remote processes.
    Setting this user option to non-`nil` forces `process-file` to
    interpret such exit codes as signals, and to return a corresponding
    string.

Function: **call-process-region** start end program &optional delete destination display &rest args [¶](#index-call_002dprocess_002dregion)
:   This function sends the text from start to end as
    standard input to a process running program. It deletes the text
    sent if delete is non-`nil`; this is useful when
    destination is `t`, to insert the output in the current
    buffer in place of the input.

    The arguments destination and display control what to do
    with the output from the subprocess, and whether to update the display
    as it comes in. For details, see the description of
    `call-process`, above. If destination is the integer 0,
    `call-process-region` discards the output and returns `nil`
    immediately, without waiting for the subprocess to finish (this only
    works if asynchronous subprocesses are supported; i.e., not on MS-DOS).

    The remaining arguments, args, are strings that specify command
    line arguments for the program.

    The return value of `call-process-region` is just like that of
    `call-process`: `nil` if you told it to return without
    waiting; otherwise, a number or string which indicates how the
    subprocess terminated.

    In the following example, we use `call-process-region` to run the
    `cat` utility, with standard input being the first five characters
    in buffer ‘`foo`’ (the word ‘`input`’). `cat` copies its
    standard input into its standard output. Since the argument
    destination is `t`, this output is inserted in the current
    buffer.

    ```
    ---------- Buffer: foo ----------
    input∗
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (call-process-region 1 6 "cat" nil t)
         ⇒ 0

    ---------- Buffer: foo ----------
    inputinput∗
    ---------- Buffer: foo ----------
    ```

    For example, the `shell-command-on-region` command uses
    `call-shell-region` in a manner similar to this:

    ```
    (call-shell-region
     start end
     command              ; shell command
     nil                  ; do not delete region
     buffer)              ; send output to buffer
    ```

Function: **call-process-shell-command** command &optional infile destination display [¶](#index-call_002dprocess_002dshell_002dcommand)
:   This function executes the shell command command synchronously.
    The other arguments are handled as in `call-process`. An old
    calling convention allowed passing any number of additional arguments
    after display, which were concatenated to command; this is
    still supported, but strongly discouraged.

Function: **process-file-shell-command** command &optional infile destination display [¶](#index-process_002dfile_002dshell_002dcommand)
:   This function is like `call-process-shell-command`, but uses
    `process-file` internally. Depending on `default-directory`,
    command can be executed also on remote hosts. An old calling
    convention allowed passing any number of additional arguments after
    display, which were concatenated to command; this is still
    supported, but strongly discouraged.

Function: **call-shell-region** start end command &optional delete destination [¶](#index-call_002dshell_002dregion)
:   This function sends the text from start to end as
    standard input to an inferior shell running command. This function
    is similar than `call-process-region`, with process being a shell.
    The arguments `delete`, `destination` and the return value
    are like in `call-process-region`.
    Note that this function doesn’t accept additional arguments.

    If command names a shell (e.g., via `shell-file-name`), keep
    in mind that behavior of various shells when commands are piped to their
    standard input is shell- and system-dependent, and thus non-portable.
    The differences are especially prominent when the region includes more
    than one line, i.e. when piping to a shell commands with embedded
    newlines. Lisp programs using this technique will therefore need to
    format the text in the region differently, according to the expectations
    of the shell.

Function: **shell-command-to-string** command [¶](#index-shell_002dcommand_002dto_002dstring)
:   This function executes command (a string) as a shell command,
    then returns the command’s output as a string. If command
    actually includes more than one command, the behavior depends on the
    shell to be invoked (determined by `shell-file-name` for local
    commands). In particular, the separator between the commands cannot be
    a newline on MS-Windows; use ‘`&&`’ instead.

Function: **process-lines** program &rest args [¶](#index-process_002dlines)
:   This function runs program, waits for it to finish, and returns
    its output as a list of strings. Each string in the list holds a
    single line of text output by the program; the end-of-line characters
    are stripped from each line. The arguments beyond program,
    args, are strings that specify command-line arguments with which
    to run the program.

    If program exits with a non-zero exit status, this function
    signals an error.

    This function works by calling `call-process`, so program output
    is decoded in the same way as for `call-process`.

Function: **process-lines-ignore-status** program &rest args [¶](#index-process_002dlines_002dignore_002dstatus)
:   This function is just like `process-lines`, but does not signal
    an error if program exits with a non-zero exit status.

---

Next: [Deleting Processes](#Deleting-Processes), Previous: [Creating a Synchronous Process](#Synchronous-Processes), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.4 Creating an Asynchronous Process [¶](#Creating-an-Asynchronous-Process)

In this section, we describe how to create an *asynchronous
process*. After an asynchronous process is created, it runs in
parallel with Emacs, and Emacs can communicate with it using the
functions described in the following sections (see [Sending Input to Processes](#Input-to-Processes), and see [Receiving Output from Processes](#Output-from-Processes)). Note that process
communication is only partially asynchronous: Emacs sends and receives
data to and from a process only when those functions are called.

An asynchronous process is controlled either via a *pty*
(pseudo-terminal) or a *pipe*. The choice of pty or pipe is made
when creating the process, by default based on the value of the
variable `process-connection-type` (see below). If available,
ptys are usually preferable for processes visible to the user, as in
Shell mode, because they allow for job control (`C-c`, `C-z`,
etc.) between the process and its children, and because interactive
programs treat ptys as terminal devices, whereas pipes don’t support
these features. However, for subprocesses used by Lisp programs for
internal purposes (i.e., no user interaction with the subprocess is
required), where significant amounts of data need to be exchanged
between the subprocess and the Lisp program, it is often better to use
a pipe, because pipes are more efficient. Also, the total number of
ptys is limited on many systems, and it is good not to waste them
unnecessarily.

Function: **make-process** &rest args [¶](#index-make_002dprocess)
:   This function is the basic low-level primitive for starting
    asynchronous subprocesses. It returns a process object representing
    the subprocess. Compared to the more high-level `start-process`,
    described below, it takes keyword arguments, is more flexible, and
    enables you to specify process filters and sentinels in a single call.

    The arguments args are a list of keyword/argument pairs.
    Omitting a keyword is always equivalent to specifying it with value
    `nil`. Here are the meaningful keywords:

    `:name name`
    :   Use the string name as the process name; if a process with this
        name already exists, then name is modified (by appending
        ‘`<1>`’, etc.) to be unique.

    `:buffer buffer`
    :   Use buffer as the process buffer. If the value is `nil`,
        the subprocess is not associated with any buffer.

    `:command command`
    :   Use command as the command line of the process. The value
        should be a list starting with the program’s executable file name,
        followed by strings to give to the program as its arguments. If
        the first element of the list is `nil`, Emacs opens a new
        pseudoterminal (pty) and associates its input and output with
        buffer, without actually running any program; the rest of the
        list elements are ignored in that case.

    `:coding coding`
    :   If coding is a symbol, it specifies the coding system to be
        used for both reading and writing of data from and to the
        connection. If coding is a cons cell
        `(decoding . encoding)`, then decoding
        will be used for reading and encoding for writing. The coding
        system used for encoding the data written to the program is also used
        for encoding the command-line arguments (but not the program itself,
        whose file name is encoded as any other file name; see [file-name-coding-system](#Encoding-and-I_002fO)).

        If coding is `nil`, the default rules for finding the
        coding system will apply. See [Default Coding Systems](#Default-Coding-Systems).

    `:connection-type type`
    :   Initialize the type of device used to communicate with the subprocess.
        Possible values are `pty` to use a pty, `pipe` to use a
        pipe, or `nil` to use the default derived from the value of the
        `process-connection-type` variable. If type is a cons cell
        `(input . output)`, then input will be used
        for standard input and output for standard output (and standard
        error if `:stderr` is `nil`).

        On systems where ptys are not available (MS-Windows), this parameter
        is ignored, and pipes are used unconditionally.

    `:noquery query-flag`
    :   Initialize the process query flag to query-flag.
        See [Querying Before Exit](#Query-Before-Exit).

    `:stop stopped`
    :   If provided, stopped must be `nil`; it is an error to use
        any non-`nil` value. The `:stop` key is ignored otherwise
        and is retained for compatibility with other process types such as
        pipe processes. Asynchronous subprocesses never start in the stopped
        state.

    `:filter filter`
    :   Initialize the process filter to filter. If not specified, a
        default filter will be provided, which can be overridden later.
        See [Process Filter Functions](#Filter-Functions).

    `:sentinel sentinel`
    :   Initialize the process sentinel to sentinel. If not specified,
        a default sentinel will be used, which can be overridden later.
        See [Sentinels: Detecting Process Status Changes](#Sentinels).

    `:stderr stderr`
    :   Associate stderr with the standard error of the process. A
        non-`nil` value should be either a buffer or a pipe process
        created with `make-pipe-process`, described below. If
        stderr is `nil`, standard error is mixed with standard
        output, and both are sent to buffer or filter.

        If stderr is a buffer, Emacs will create a pipe process, the
        *standard error process*. This process will have the default
        filter (see [Process Filter Functions](#Filter-Functions)), sentinel (see [Sentinels: Detecting Process Status Changes](#Sentinels)), and
        coding systems (see [Default Coding Systems](#Default-Coding-Systems)). On the other hand,
        it will use query-flag as its query-on-exit flag (see [Querying Before Exit](#Query-Before-Exit)). It will be associated with the stderr buffer
        (see [Process Buffers](#Process-Buffers)) and send its output (which is the standard
        error of the main process) there. To get the process object for the
        standard error process, pass the stderr buffer to
        `get-buffer-process`.

        If stderr is a pipe process, Emacs will use it as standard error
        process for the new process.

    `:file-handler file-handler`
    :   If file-handler is non-`nil`, then look for a file name
        handler for the current buffer’s `default-directory`, and invoke
        that file name handler to make the process. If there is no such
        handler, proceed as if file-handler were `nil`.

    The original argument list, modified with the actual connection
    information, is available via the `process-contact` function.

    The current working directory of the subprocess is set to the current
    buffer’s value of `default-directory` if that is local (as
    determined by `unhandled-file-name-directory`), or `~`
    otherwise. If you want to run a process in a remote directory, pass
    `:file-handler t` to `make-process`. In that case, the
    current working directory is the local name component of
    `default-directory` (as determined by `file-local-name`).

    Depending on the implementation of the file name handler, it might not
    be possible to apply filter or sentinel to the resulting
    process object. The `:stderr` argument cannot be a pipe process,
    file name handlers do not support pipe processes for this. A buffer
    as `:stderr` argument is accepted, its contents is shown without
    the use of pipe processes. See [Process Filter Functions](#Filter-Functions), [Sentinels: Detecting Process Status Changes](#Sentinels),
    and [Accepting Output from Processes](#Accepting-Output).

    Some file name handlers may not support `make-process`. In such
    cases, this function does nothing and returns `nil`.

Function: **make-pipe-process** &rest args [¶](#index-make_002dpipe_002dprocess)
:   This function creates a bidirectional pipe which can be attached to a
    child process. This is useful with the `:stderr` keyword of
    `make-process`. The function returns a process object.

    The arguments args are a list of keyword/argument pairs.
    Omitting a keyword is always equivalent to specifying it with value
    `nil`.

    Here are the meaningful keywords:

    `:name name`
    :   Use the string name as the process name. As with
        `make-process`, it is modified if necessary to make it unique.

    `:buffer buffer`
    :   Use buffer as the process buffer. If the value is `nil`,
        the subprocess is not associated with any buffer.

    `:coding coding`
    :   If coding is a symbol, it specifies the coding system to be
        used for both reading and writing of data from and to the
        connection. If coding is a cons cell
        `(decoding . encoding)`, then decoding
        will be used for reading and encoding for writing.

        If coding is `nil`, the default rules for finding the
        coding system will apply. See [Default Coding Systems](#Default-Coding-Systems).

    `:noquery query-flag`
    :   Initialize the process query flag to query-flag.
        See [Querying Before Exit](#Query-Before-Exit).

    `:stop stopped`
    :   If stopped is non-`nil`, start the process in the stopped
        state. In the stopped state, a pipe process does not accept incoming
        data, but you can send outgoing data. The stopped state is set by
        `stop-process` and cleared by `continue-process`
        (see [Sending Signals to Processes](#Signals-to-Processes)).

    `:filter filter`
    :   Initialize the process filter to filter. If not specified, a
        default filter will be provided, which can be changed later.
        See [Process Filter Functions](#Filter-Functions).

    `:sentinel sentinel`
    :   Initialize the process sentinel to sentinel. If not specified,
        a default sentinel will be used, which can be changed later.
        See [Sentinels: Detecting Process Status Changes](#Sentinels).

    The original argument list, modified with the actual connection
    information, is available via the `process-contact` function.

Function: **start-process** name buffer-or-name program &rest args [¶](#index-start_002dprocess)
:   This function is a higher-level wrapper around `make-process`,
    exposing an interface that is similar to `call-process`. It
    creates a new asynchronous subprocess and starts the specified
    program running in it. It returns a process object that stands
    for the new subprocess in Lisp. The argument name specifies the
    name for the process object; as with `make-process`, it is
    modified if necessary to make it unique. The buffer
    buffer-or-name is the buffer to associate with the process.

    If program is `nil`, Emacs opens a new pseudoterminal (pty)
    and associates its input and output with buffer-or-name, without
    creating a subprocess. In that case, the remaining arguments
    args are ignored.

    The rest of args are strings that specify command line arguments
    for the subprocess.

    In the example below, the first process is started and runs (rather,
    sleeps) for 100 seconds (the output buffer ‘`foo`’ is created
    immediately). Meanwhile, the second process is started, and
    given the name ‘`my-process<1>`’ for the sake of uniqueness. It
    inserts the directory listing at the end of the buffer ‘`foo`’,
    before the first process finishes. Then it finishes, and a message to
    that effect is inserted in the buffer. Much later, the first process
    finishes, and another message is inserted in the buffer for it.

    ```
    (start-process "my-process" "foo" "sleep" "100")
         ⇒ #<process my-process>
    ```

    ```

    ```

    ```
    (start-process "my-process" "foo" "ls" "-l" "/bin")
         ⇒ #<process my-process<1>>

    ---------- Buffer: foo ----------
    total 8336
    -rwxr-xr-x 1 root root 971384 Mar 30 10:14 bash
    -rwxr-xr-x 1 root root 146920 Jul  5  2011 bsd-csh
    ...
    -rwxr-xr-x 1 root root 696880 Feb 28 15:55 zsh4

    Process my-process<1> finished

    Process my-process finished
    ---------- Buffer: foo ----------
    ```

Function: **start-file-process** name buffer-or-name program &rest args [¶](#index-start_002dfile_002dprocess)
:   Like `start-process`, this function starts a new asynchronous
    subprocess running program in it, and returns its process
    object.

    The difference from `start-process` is that this function may
    invoke a file name handler based on the value of `default-directory`.
    This handler ought to run program, perhaps on the local host,
    perhaps on a remote host that corresponds to `default-directory`.
    In the latter case, the local part of `default-directory` becomes
    the working directory of the process.

    This function does not try to invoke file name handlers for
    program or for the rest of args. For that reason, if
    program or any of args use the remote-file syntax
    (see [Making Certain File Names “Magic”](#Magic-File-Names)), they must be converted either to file
    names relative to `default-directory`, or to names that identify
    the files locally on the remote host, by running them through
    `file-local-name`.

    Depending on the implementation of the file name handler, it might not be
    possible to apply `process-filter` or `process-sentinel` to
    the resulting process object. See [Process Filter Functions](#Filter-Functions), and [Sentinels: Detecting Process Status Changes](#Sentinels).

    Some file name handlers may not support `start-file-process` (for
    example the function `ange-ftp-hook-function`). In such cases,
    this function does nothing and returns `nil`.

Function: **start-process-shell-command** name buffer-or-name command [¶](#index-start_002dprocess_002dshell_002dcommand)
:   This function is like `start-process`, except that it uses a
    shell to execute the specified command. The argument
    command is a shell command string. The variable
    `shell-file-name` specifies which shell to use.

    The point of running a program through the shell, rather than directly
    with `make-process` or `start-process`, is so that you can
    employ shell features such as wildcards in the arguments. It follows
    that if you include any arbitrary user-specified arguments in the
    command, you should quote them with `shell-quote-argument` first,
    so that any special shell characters do *not* have their special
    shell meanings. See [Shell Arguments](#Shell-Arguments). Of course, when executing
    commands based on user input you should also consider the security
    implications.

Function: **start-file-process-shell-command** name buffer-or-name command [¶](#index-start_002dfile_002dprocess_002dshell_002dcommand)
:   This function is like `start-process-shell-command`, but uses
    `start-file-process` internally. Because of this, command
    can also be executed on remote hosts, depending on `default-directory`.

Variable: **process-connection-type** [¶](#index-process_002dconnection_002dtype)
:   This variable controls the type of device used to communicate with
    asynchronous subprocesses. If it is non-`nil`, then ptys are
    used, when available. Otherwise, pipes are used.

    The value of `process-connection-type` takes effect when
    `make-process` or `start-process` is called. So you can
    specify how to communicate with one subprocess by binding the variable
    around the call to these functions.

    Note that the value of this variable is ignored when
    `make-process` is called with a non-`nil` value of the
    `:stderr` parameter; in that case, Emacs will communicate with
    the process using pipes. It is also ignored if ptys are unavailable
    (MS-Windows).

    ```
    (let ((process-connection-type nil))  ; use a pipe
      (start-process ...))
    ```

    To determine whether a given subprocess actually got a pipe or a pty,
    use the function `process-tty-name` (see [Process Information](#Process-Information)).

Variable: **process-error-pause-time** [¶](#index-process_002derror_002dpause_002dtime)
:   If a process sentinel/filter function has an error, Emacs will (by
    default) pause Emacs for `process-error-pause-time` seconds after
    displaying this error, so that users will see the error in question.
    However, this can lead to situations where Emacs becomes unresponsive
    (if there’s a lot of these errors happening), so this can be disabled
    by setting `process-error-pause-time` to 0.

---

Next: [Process Information](#Process-Information), Previous: [Creating an Asynchronous Process](#Asynchronous-Processes), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.5 Deleting Processes [¶](#Deleting-Processes-1)

*Deleting a process* disconnects Emacs immediately from the
subprocess. Processes are deleted automatically after they terminate,
but not necessarily right away. You can delete a process explicitly
at any time. If you explicitly delete a terminated process before it
is deleted automatically, no harm results. Deleting a running
process sends a signal to terminate it (and its child processes, if
any), and calls the process sentinel. See [Sentinels: Detecting Process Status Changes](#Sentinels).

When a process is deleted, the process object itself continues to
exist as long as other Lisp objects point to it. All the Lisp
primitives that work on process objects accept deleted processes, but
those that do I/O or send signals will report an error. The process
mark continues to point to the same place as before, usually into a
buffer where output from the process was being inserted.

User Option: **delete-exited-processes** [¶](#index-delete_002dexited_002dprocesses)
:   This variable controls automatic deletion of processes that have
    terminated (due to calling `exit` or to a signal). If it is
    `nil`, then they continue to exist until the user runs
    `list-processes`. Otherwise, they are deleted immediately after
    they exit.

Function: **delete-process** &optional process [¶](#index-delete_002dprocess)
:   This function deletes a process, killing it with a `SIGKILL`
    signal if the process was running a program. The argument may be a
    process, the name of a process, a buffer, or the name of a buffer. (A
    buffer or buffer-name stands for the process that
    `get-buffer-process` returns, and a missing or `nil`
    process means that the current buffer’s process should be
    killed.) Calling `delete-process` on a running process
    terminates it, updates the process status, and runs the sentinel
    immediately. If the process has already terminated, calling
    `delete-process` has no effect on its status, or on the running
    of its sentinel (which will happen sooner or later).

    If the process object represents a network, serial, or pipe
    connection, its status changes to `closed`; otherwise, it changes
    to `signal`, unless the process already exited. See [process-status](#Process-Information).

    ```
    (delete-process "*shell*")
         ⇒ nil
    ```

---

Next: [Sending Input to Processes](#Input-to-Processes), Previous: [Deleting Processes](#Deleting-Processes), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.6 Process Information [¶](#Process-Information-1)

Several functions return information about processes.

Command: **list-processes** &optional query-only buffer [¶](#index-list_002dprocesses)
:   This command displays a listing of all living processes. In addition,
    it finally deletes any process whose status was ‘`Exited`’ or
    ‘`Signaled`’. It returns `nil`.

    The processes are shown in a buffer named `*Process List*`
    (unless you specify otherwise using the optional argument buffer),
    whose major mode is Process Menu mode.

    If query-only is non-`nil`, it only lists processes
    whose query flag is non-`nil`. See [Querying Before Exit](#Query-Before-Exit).

Function: **process-list** [¶](#index-process_002dlist)
:   This function returns a list of all processes that have not been deleted.

    ```
    (process-list)
         ⇒ (#<process display-time> #<process shell>)
    ```

Function: **num-processors** &optional query [¶](#index-num_002dprocessors)
:   This function returns the number of processors, a positive integer.
    Each usable thread execution unit counts as a processor.
    By default, the count includes the number of available processors,
    which you can override by setting the
    [`OMP_NUM_THREADS` environment variable of OpenMP](https://www.openmp.org/spec-html/5.1/openmpse59.html).
    If the optional argument query is `current`,
    this function ignores `OMP_NUM_THREADS`;
    if query is `all`, this function also counts processors
    that are on the system but are not available to the current process.

Function: **get-process** name [¶](#index-get_002dprocess)
:   This function returns the process named name (a string), or
    `nil` if there is none. The argument name can also be a
    process object, in which case it is returned.

    ```
    (get-process "shell")
         ⇒ #<process shell>
    ```

Function: **process-command** process [¶](#index-process_002dcommand)
:   This function returns the command that was executed to start
    process. This is a list of strings, the first string being the
    program executed and the rest of the strings being the arguments that
    were given to the program. For a network, serial, or pipe connection,
    this is either `nil`, which means the process is running or
    `t` (process is stopped).

    ```
    (process-command (get-process "shell"))
         ⇒ ("bash" "-i")
    ```

Function: **process-contact** process &optional key no-block [¶](#index-process_002dcontact)
:   This function returns information about how a network, a serial, or a
    pipe connection was set up. When key is `nil`, it returns
    `(hostname service)` for a network connection,
    `(port speed)` for a serial connection, and `t`
    for a pipe connection. For an ordinary child process, this function
    always returns `t` when called with a `nil` key.

    If key is `t`, the value is the complete status information
    for the connection, server, serial port, or pipe; that is, the list of
    keywords and values specified in `make-network-process`,
    `make-serial-process`, or `make-pipe-process`, except that
    some of the values represent the current status instead of what you
    specified.

    For a network process, the values include (see
    `make-network-process` for a complete list):

    `:buffer`
    :   The associated value is the process buffer.

    `:filter`
    :   The associated value is the process filter function. See [Process Filter Functions](#Filter-Functions).

    `:sentinel`
    :   The associated value is the process sentinel function. See [Sentinels: Detecting Process Status Changes](#Sentinels).

    `:remote`
    :   In a connection, the address in internal format of the remote peer.

    `:local`
    :   The local address, in internal format.

    `:service`
    :   In a server, if you specified `t` for service,
        this value is the actual port number.

    `:local` and `:remote` are included even if they were not
    specified explicitly in `make-network-process`.

    For a serial connection, see `make-serial-process` and
    `serial-process-configure` for the list of keys. For a pipe
    connection, see `make-pipe-process` for the list of keys.

    If key is a keyword, the function returns the value corresponding
    to that keyword.

    If process is a non-blocking network stream that hasn’t been
    fully set up yet, then this function will block until that has
    happened. If given the optional no-block parameter, this
    function will return `nil` instead of blocking.

Function: **process-id** process [¶](#index-process_002did)
:   This function returns the PID of process. This is an
    integral number that distinguishes the process process from all
    other processes running on the same computer at the current time. The
    PID of a process is chosen by the operating system kernel
    when the process is started and remains constant as long as the
    process exists. For network, serial, and pipe connections, this
    function returns `nil`.

Function: **process-name** process [¶](#index-process_002dname)
:   This function returns the name of process, as a string.

Function: **process-status** process-name [¶](#index-process_002dstatus)
:   This function returns the status of process-name as a symbol.
    The argument process-name must be a process, a buffer, or a
    process name (a string).

    The possible values for an actual subprocess are:

    `run`
    :   for a process that is running.

    `stop`
    :   for a process that is stopped but continuable.

    `exit`
    :   for a process that has exited.

    `signal`
    :   for a process that has received a fatal signal.

    `open`
    :   for a network, serial, or pipe connection that is open.

    `closed`
    :   for a network, serial, or pipe connection that is closed. Once a
        connection is closed, you cannot reopen it, though you might be able
        to open a new connection to the same place.

    `connect`
    :   for a non-blocking connection that is waiting to complete.

    `failed`
    :   for a non-blocking connection that has failed to complete.

    `listen`
    :   for a network server that is listening.

    `nil`
    :   if process-name is not the name of an existing process.

    ```
    (process-status (get-buffer "*shell*"))
         ⇒ run
    ```

    For a network, serial, or pipe connection, `process-status`
    returns one of the symbols `open`, `stop`, or `closed`.
    The latter means that the other side closed the connection, or Emacs
    did `delete-process`. The value `stop` means that
    `stop-process` was called on the connection.

Function: **process-live-p** process [¶](#index-process_002dlive_002dp)
:   This function returns non-`nil` if process is alive. A
    process is considered alive if its status is `run`, `open`,
    `listen`, `connect` or `stop`.

Function: **process-type** process [¶](#index-process_002dtype)
:   This function returns the symbol `network` for a network
    connection or server, `serial` for a serial port connection,
    `pipe` for a pipe connection, or `real` for a subprocess
    created for running a program.

Function: **process-exit-status** process [¶](#index-process_002dexit_002dstatus)
:   This function returns the exit status of process or the signal
    number that killed it. (Use the result of `process-status` to
    determine which of those it is.) If process has not yet
    terminated, the value is 0. For network, serial, and pipe connections
    that are already closed, the value is either 0 or 256, depending on
    whether the connection was closed normally or abnormally.

Function: **process-tty-name** process &optional stream [¶](#index-process_002dtty_002dname)
:   This function returns the terminal name that process is using for
    its communication with Emacs—or `nil` if it is using pipes
    instead of a pty (see `process-connection-type` in
    [Creating an Asynchronous Process](#Asynchronous-Processes)). By default, this function returns the
    terminal name if any of process’s standard streams use a
    terminal. If stream is one of `stdin`, `stdout`, or
    `stderr`, this function returns the terminal name (or `nil`,
    as above) that process uses for that stream specifically. You
    can use this to determine whether a particular stream uses a pipe or a
    pty.

    If process represents a program running on a remote host, this
    function returns the *local* terminal name that communicates with
    process; you can get the terminal name used by that program on
    the remote host with the process property `remote-tty`. If
    process represents a network, serial, or pipe connection, this
    function always returns `nil`.

Function: **process-coding-system** process [¶](#index-process_002dcoding_002dsystem)
:   This function returns a cons cell `(decode . encode)`,
    describing the coding systems in use for decoding output from, and
    encoding input to, process (see [Coding Systems](#Coding-Systems)).

Function: **set-process-coding-system** process &optional decoding-system encoding-system [¶](#index-set_002dprocess_002dcoding_002dsystem)
:   This function specifies the coding systems to use for subsequent output
    from and input to process. It will use decoding-system to
    decode subprocess output, and encoding-system to encode subprocess
    input.

Every process also has a property list that you can use to store
miscellaneous values associated with the process.

Function: **process-get** process propname [¶](#index-process_002dget)
:   This function returns the value of the propname property
    of process.

Function: **process-put** process propname value [¶](#index-process_002dput)
:   This function sets the value of the propname property
    of process to value.

Function: **process-plist** process [¶](#index-process_002dplist)
:   This function returns the process plist of process.

Function: **set-process-plist** process plist [¶](#index-set_002dprocess_002dplist)
:   This function sets the process plist of process to plist.

---

Next: [Sending Signals to Processes](#Signals-to-Processes), Previous: [Process Information](#Process-Information), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.7 Sending Input to Processes [¶](#Sending-Input-to-Processes)

Asynchronous subprocesses receive input when it is sent to them by
Emacs, which is done with the functions in this section. You must
specify the process to send input to, and the input data to send. If
the subprocess runs a program, the data appears on the standard input
of that program; for connections, the data is sent to the connected
device or program.

Some operating systems have limited space for buffered input in a
pty. On these systems, Emacs sends an EOF periodically
amidst the other characters, to force them through. For most
programs, these EOFs do no harm.

Subprocess input is normally encoded using a coding system before the
subprocess receives it, much like text written into a file. You can use
`set-process-coding-system` to specify which coding system to use
(see [Process Information](#Process-Information)). Otherwise, the coding system comes from
`coding-system-for-write`, if that is non-`nil`; or else from
the defaulting mechanism (see [Default Coding Systems](#Default-Coding-Systems)).

Sometimes the system is unable to accept input for that process,
because the input buffer is full. When this happens, the send functions
wait a short while, accepting output from subprocesses, and then try
again. This gives the subprocess a chance to read more of its pending
input and make space in the buffer. It also allows filters (including
the one currently running), sentinels and timers to run—so take
account of that in writing your code.

In these functions, the process argument can be a process or
the name of a process, or a buffer or buffer name (which stands
for a process via `get-buffer-process`). `nil` means
the current buffer’s process.

Function: **process-send-string** process string [¶](#index-process_002dsend_002dstring)
:   This function sends process the contents of string as
    standard input. It returns `nil`. For example, to make a
    Shell buffer list files:

    ```
    (process-send-string "shell<1>" "ls\n")
         ⇒ nil
    ```

Function: **process-send-region** process start end [¶](#index-process_002dsend_002dregion)
:   This function sends the text in the region defined by start and
    end as standard input to process.

    An error is signaled unless both start and end are
    integers or markers that indicate positions in the current buffer. (It
    is unimportant which number is larger.)

Function: **process-send-eof** &optional process [¶](#index-process_002dsend_002deof)
:   This function makes process see an end-of-file in its
    input. The EOF comes after any text already sent to it.
    The function returns process.

    ```
    (process-send-eof "shell")
         ⇒ "shell"
    ```

Function: **process-running-child-p** &optional process [¶](#index-process_002drunning_002dchild_002dp)
:   This function will tell you whether a process, which must not be
    a connection but a real subprocess, has given control of its terminal
    to a child process of its own. If this is true, the function returns
    the numeric ID of the foreground process group of process; it
    returns `nil` if Emacs can be certain that this is not so. The
    value is `t` if Emacs cannot tell whether this is true. This
    function signals an error if process is a network, serial, or
    pipe connection, or if the subprocess is not active.

---

Next: [Receiving Output from Processes](#Output-from-Processes), Previous: [Sending Input to Processes](#Input-to-Processes), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.8 Sending Signals to Processes [¶](#Sending-Signals-to-Processes)

*Sending a signal* to a subprocess is a way of interrupting its
activities. There are several different signals, each with its own
meaning. The set of signals and their names is defined by the operating
system. For example, the signal `SIGINT` means that the user has
typed `C-c`, or that some analogous thing has happened.

Each signal has a standard effect on the subprocess. Most signals
kill the subprocess, but some stop (or resume) execution instead. Most
signals can optionally be handled by programs; if the program handles
the signal, then we can say nothing in general about its effects.

You can send signals explicitly by calling the functions in this
section. Emacs also sends signals automatically at certain times:
killing a buffer sends a `SIGHUP` signal to all its associated
processes; killing Emacs sends a `SIGHUP` signal to all remaining
processes. (`SIGHUP` is a signal that usually indicates that the
user “hung up the phone”, i.e., disconnected.)

Each of the signal-sending functions takes two optional arguments:
process and current-group.

The argument process must be either a process, a process
name, a buffer, a buffer name, or `nil`. A buffer or buffer name
stands for a process through `get-buffer-process`. `nil`
stands for the process associated with the current buffer. Except
with `stop-process` and `continue-process`, an error is
signaled if process does not identify an active process, or if
it represents a network, serial, or pipe connection.

The argument current-group is a flag that makes a difference
when you are running a job-control shell as an Emacs subprocess. If it
is non-`nil`, then the signal is sent to the current process-group
of the terminal that Emacs uses to communicate with the subprocess. If
the process is a job-control shell, this means the shell’s current
subjob. If current-group is `nil`, the signal is
sent to the process group of the immediate subprocess of Emacs. If
the subprocess is a job-control shell, this is the shell itself. If
current-group is `lambda`, the signal is sent to the
process-group that owns the terminal, but only if it is not the shell
itself.

The flag current-group has no effect when a pipe is used to
communicate with the subprocess, because the operating system does not
support the distinction in the case of pipes. For the same reason,
job-control shells won’t work when a pipe is used. See
`process-connection-type` in [Creating an Asynchronous Process](#Asynchronous-Processes).

Function: **interrupt-process** &optional process current-group [¶](#index-interrupt_002dprocess)
:   This function interrupts the process process by sending the
    signal `SIGINT`. Outside of Emacs, typing the interrupt
    character (normally `C-c` on some systems, and `DEL` on
    others) sends this signal. When the argument current-group is
    non-`nil`, you can think of this function as typing `C-c`
    on the terminal by which Emacs talks to the subprocess.

Command: **kill-process** &optional process current-group [¶](#index-kill_002dprocess)
:   This command kills the process process by sending the
    signal `SIGKILL`. This signal kills the subprocess immediately,
    and cannot be handled by the subprocess. Interactively, it’ll prompt
    the user for a process name, defaulting to the process (if any) in the
    current buffer.

Function: **quit-process** &optional process current-group [¶](#index-quit_002dprocess)
:   This function sends the signal `SIGQUIT` to the process
    process. This signal is the one sent by the quit
    character (usually `C-\`) when you are not inside
    Emacs.

Function: **stop-process** &optional process current-group [¶](#index-stop_002dprocess)
:   This function stops the specified process. If it is a real
    subprocess running a program, it sends the signal `SIGTSTP` to
    that subprocess. If process represents a network, serial, or
    pipe connection, this function inhibits handling of the incoming data
    from the connection; for a network server, this means not accepting
    new connections. Use `continue-process` to resume normal
    execution.

    Outside of Emacs, on systems with job control, the stop character
    (usually `C-z`) normally sends the `SIGTSTP` signal to a
    subprocess. When current-group is non-`nil`, you can think
    of this function as typing `C-z` on the terminal Emacs uses to
    communicate with the subprocess.

Function: **continue-process** &optional process current-group [¶](#index-continue_002dprocess)
:   This function resumes execution of the process process. If it
    is a real subprocess running a program, it sends the signal
    `SIGCONT` to that subprocess; this presumes that process
    was stopped previously. If process represents a network,
    serial, or pipe connection, this function resumes handling of the
    incoming data from the connection. For serial connections, data that
    arrived during the time the process was stopped might be lost.

Command: **signal-process** process signal &optional remote [¶](#index-signal_002dprocess)
:   This function sends a signal to process process. The argument
    signal specifies which signal to send; it should be an integer,
    or a symbol whose name is a signal.

    The process argument can be a system process ID (an
    integer); that allows you to send signals to processes that are not
    children of Emacs. See [Accessing Other Processes](#System-Processes).

    If process is a process object which contains the property
    `remote-pid`, or process is a number and remote is a
    remote file name, process is interpreted as process on the
    respective remote host, which will be the process to signal.

    If process is a string, it is interpreted as process object with
    the respective process name, or as a number.

Sometimes, it is necessary to send a signal to a non-local
asynchronous process. This is possible by writing an own
`interrupt-process` or `signal-process` implementation.
This function must be added then to `interrupt-process-functions`
or `signal-process-functions`, respectively.

Variable: **interrupt-process-functions** [¶](#index-interrupt_002dprocess_002dfunctions)
:   This variable is a list of functions to be called for
    `interrupt-process`. The arguments of the functions are the same
    as for `interrupt-process`. These functions are called in the
    order of the list, until one of them returns non-`nil`. The
    default function, which shall always be the last in this list, is
    `internal-default-interrupt-process`.

    This is the mechanism, how Tramp implements `interrupt-process`.

Variable: **signal-process-functions** [¶](#index-signal_002dprocess_002dfunctions)
:   This variable is a list of functions to be called for
    `signal-process`. The arguments of the functions are the same as
    for `signal-process`. These functions are called in the order of
    the list, until one of them returns non-`nil`. The default
    function, which shall always be the last in this list, is
    `internal-default-signal-process`.

    This is the mechanism, how Tramp implements `signal-process`.

---

Next: [Sentinels: Detecting Process Status Changes](#Sentinels), Previous: [Sending Signals to Processes](#Signals-to-Processes), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
