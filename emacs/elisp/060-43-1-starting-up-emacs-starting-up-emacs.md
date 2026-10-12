### 43.1 Starting Up Emacs [¶](#Starting-Up-Emacs)

This section describes what Emacs does when it is started, and how you
can customize these actions.

* [Summary: Sequence of Actions at Startup](#Startup-Summary)
* [The Init File](#Init-File)
* [Terminal-Specific Initialization](#Terminal_002dSpecific)
* [Command-Line Arguments](#Command_002dLine-Arguments)

---

Next: [The Init File](#Init-File), Up: [Starting Up Emacs](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.1.1 Summary: Sequence of Actions at Startup [¶](#Summary_003a-Sequence-of-Actions-at-Startup)

When Emacs is started up, it performs the following operations
(see `normal-top-level` in `startup.el`):

1. It adds subdirectories to `load-path`, by running the file named
   `subdirs.el` in each directory in the list. Normally, this file
   adds the directory’s subdirectories to the list, and those are scanned
   in their turn. The files `subdirs.el` are normally generated
   automatically when Emacs is installed.
2. It loads any `leim-list.el` that it finds in the `load-path`
   directories. This file is intended for registering input methods.
   The search is only for any personal `leim-list.el` files that you
   may have created; it skips the directories containing the standard Emacs
   libraries (these should contain only a single `leim-list.el` file,
   which is compiled into the Emacs executable).
3. It sets the variable `before-init-time` to the value of
   `current-time` (see [Time of Day](#Time-of-Day)). It also sets
   `after-init-time` to `nil`, which signals to Lisp programs
   that Emacs is being initialized.
4. It sets the language environment and the terminal coding system,
   if requested by environment variables such as `LANG`.
5. It does some basic parsing of the command-line arguments.
6. It loads the library `site-start`, if it exists. This is not
   done if the options ‘`-Q`’ or ‘`--no-site-file`’ were specified.
7. It loads your early init file (see [Early Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Early-Init-File) in The
   GNU Emacs Manual). This is not done if the options ‘`-q`’,
   ‘`-Q`’, or ‘`--batch`’ were specified. If the ‘`-u`’ option
   was specified, Emacs looks for the init file in that user’s home
   directory instead.
8. It calls the function `package-activate-all` to activate any
   optional Emacs Lisp package that has been installed. See [Packaging Basics](#Packaging-Basics). However, Emacs doesn’t activate the packages when
   `package-enable-at-startup` is `nil` or when it’s started
   with one of the options ‘`-q`’, ‘`-Q`’, or ‘`--batch`’. To
   activate the packages in the latter case, `package-activate-all`
   should be called explicitly (e.g., via the ‘`--funcall`’ option).
9. If not running in batch mode, it initializes the window system that
   the variable `initial-window-system` specifies (see [initial-window-system](#Window-Systems)). The initialization function,
   `window-system-initialization`, is a *generic function*
   (see [Generic Functions](#Generic-Functions)) whose actual implementation is different
   for each supported window system. If the value of
   `initial-window-system` is windowsystem, then the
   appropriate implementation of the initialization function is defined
   in the file `term/windowsystem-win.el`. This file should
   have been compiled into the Emacs executable when it was built.
10. It runs the normal hook `before-init-hook`.
11. If appropriate, it creates a graphical frame. As part of creating the
    graphical frame, it initializes the window system specified by
    `initial-frame-alist` and `default-frame-alist`
    (see [Initial Frame Parameters](#Initial-Parameters)) for the graphical frame, by calling the
    `window-system-initialization` function for that window system.
    This is not done in batch (noninteractive) or daemon mode.
12. It initializes the initial frame’s faces, and sets up the menu bar
    and tool bar if needed. If graphical frames are supported, it sets up
    the tool bar even if the current frame is not a graphical one, since a
    graphical frame may be created later on.
13. It uses `custom-reevaluate-setting` to re-initialize the members
    of the list `custom-delayed-init-variables`. These are any
    pre-loaded user options whose default value depends on the run-time,
    rather than build-time, context.
    See [custom-initialize-delay](#Building-Emacs).
14. It loads your init file (see [The Init File](#Init-File)). This is not done if the
    options ‘`-q`’, ‘`-Q`’, or ‘`--batch`’ were specified. If
    the ‘`-u`’ option was specified, Emacs looks for the init file in
    that user’s home directory instead.
15. It loads the library `default`, if it exists. This is not done
    if `inhibit-default-init` is non-`nil`, nor if the options
    ‘`-q`’, ‘`-Q`’, or ‘`--batch`’ were specified.
16. It loads your abbrevs from the file specified by
    `abbrev-file-name`, if that file exists and can be read
    (see [abbrev-file-name](#Abbrev-Files)). This is not done if the
    option ‘`--batch`’ was specified.
17. It sets the variable `after-init-time` to the value of
    `current-time`. This variable was set to `nil` earlier;
    setting it to the current time signals that the initialization phase
    is over, and, together with `before-init-time`, provides the
    measurement of how long it took.
18. It runs the normal hooks `after-init-hook` and
    `delayed-warnings-hook`. The latter shows any warnings emitted
    during previous stages of startup, which are automatically delayed.
19. If the buffer `*scratch*` exists and is still in Fundamental mode
    (as it should be by default), it sets its major mode according to
    `initial-major-mode`.
20. If started on a text terminal, it loads the terminal-specific
    Lisp library (see [Terminal-Specific Initialization](#Terminal_002dSpecific)), and runs the hook
    `tty-setup-hook`. This is not done
    in `--batch` mode, nor if `term-file-prefix` is `nil`.
21. It displays the initial echo area message, unless you have suppressed
    that with `inhibit-startup-echo-area-message`.
22. It processes any command-line options that were not handled earlier.
23. It now exits if the option `--batch` was specified.
24. If the `*scratch*` buffer exists and is empty, it inserts
    `(substitute-command-keys initial-scratch-message)` into that buffer.
25. If `initial-buffer-choice` is a string, it visits the file (or
    directory) with that name. If it is a function, it calls the function
    with no arguments and selects the buffer that it returns. If one file
    is given as a command line argument, that file is visited and its
    buffer displayed alongside `initial-buffer-choice`. If more than
    one file is given, all of the files are visited and the `*Buffer
    List*` buffer is displayed alongside `initial-buffer-choice`.
26. It runs `emacs-startup-hook`.
27. It calls `frame-notice-user-settings`, which modifies the
    parameters of the selected frame according to whatever the init files
    specify.
28. It runs `window-setup-hook`. The only difference between this
    hook and `emacs-startup-hook` is that this one runs after the
    previously mentioned modifications to the frame parameters.
29. It displays the *startup screen*, which is a special buffer that
    contains information about copyleft and basic Emacs usage. This is
    not done if `inhibit-startup-screen` or `initial-buffer-choice`
    are non-`nil`, or if the ‘`--no-splash`’ or ‘`-Q`’ command-line
    options were specified.
30. If a daemon was requested, it calls `server-start`.
    (On POSIX systems, if a background daemon was requested, it then
    detaches from the controlling terminal.) See [Emacs
    Server](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Emacs-Server) in The GNU Emacs Manual.
31. If started by the X session manager, it calls
    `emacs-session-restore` passing it as argument the ID of the
    previous session. See [Session Management](#Session-Management).

The following options affect some aspects of the startup sequence.

User Option: **inhibit-startup-screen** [¶](#index-inhibit_002dstartup_002dscreen)
:   This variable, if non-`nil`, inhibits the startup screen. In
    that case, Emacs typically displays the `*scratch*` buffer; but
    see `initial-buffer-choice`, below.

    Do not set this variable in the init file of a new user, or in a way
    that affects more than one user, as that would prevent new users from
    receiving information about copyleft and basic Emacs usage.

    `inhibit-startup-message` and `inhibit-splash-screen` are
    aliases for this variable.

User Option: **initial-buffer-choice** [¶](#index-initial_002dbuffer_002dchoice)
:   If non-`nil`, this variable is a string that specifies a file or
    directory for Emacs to display after starting up, instead of the
    startup screen.
    If its value is a function, Emacs calls that function which must
    return a buffer which is then displayed.
    If its value is `t`, Emacs displays the `*scratch*` buffer.

User Option: **inhibit-startup-echo-area-message** [¶](#index-inhibit_002dstartup_002decho_002darea_002dmessage)
:   This variable controls the display of the startup echo area message.
    You can suppress the startup echo area message by adding text with this
    form to your init file:

    ```
    (setq inhibit-startup-echo-area-message
          "your-login-name")
    ```

    Emacs explicitly checks for an expression as shown above in your init
    file; your login name must appear in the expression as a Lisp string
    constant. You can also use the Customize interface. Other methods of
    setting `inhibit-startup-echo-area-message` to the same value do
    not inhibit the startup message. This way, you can easily inhibit the
    message for yourself if you wish, but thoughtless copying of your init
    file will not inhibit the message for someone else.

User Option: **initial-scratch-message** [¶](#index-initial_002dscratch_002dmessage)
:   This variable, if non-`nil`, should be a string, which is treated
    as documentation to be inserted into the `*scratch*` buffer when
    Emacs starts up or when that buffer is recreated. If it is
    `nil`, the `*scratch*` buffer is empty.

The following command-line options affect some aspects of the startup
sequence. See [Initial Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Initial-Options) in The GNU Emacs Manual.

`--no-splash`
:   Do not display a splash screen.

`--batch`
:   Run without an interactive terminal. See [Batch Mode](#Batch-Mode).

`--daemon`

`--bg-daemon`

`--fg-daemon`
:   Do not initialize any display; just start a server.
    (A “background” daemon automatically runs in the background.)

`--no-init-file`

`-q`
:   Do not load either the init file, or the `default` library.

`--no-site-file`
:   Do not load the `site-start` library.

`--quick`

`-Q`
:   Equivalent to ‘`-q --no-site-file --no-splash`’.

`--init-directory`
:   Specify the directory to use when finding the Emacs init files.

---

Next: [Terminal-Specific Initialization](#Terminal_002dSpecific), Previous: [Summary: Sequence of Actions at Startup](#Startup-Summary), Up: [Starting Up Emacs](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.1.2 The Init File [¶](#The-Init-File)

When you start Emacs, it normally attempts to load your *init
file*. This is either a file named `.emacs` or `.emacs.el`
in your home directory, or a file named `init.el` in a
subdirectory named `.emacs.d` in your home directory.

The command-line switches ‘`-q`’, ‘`-Q`’, and ‘`-u`’
control whether and where to find the init file; ‘`-q`’ (and the
stronger ‘`-Q`’) says not to load an init file, while ‘`-u
user`’ says to load user’s init file instead of yours.
See [Entering Emacs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Entering-Emacs) in The GNU Emacs Manual. If neither
option is specified, Emacs uses the `LOGNAME` environment
variable, or the `USER` (most systems) or `USERNAME` (MS
systems) variable, to find your home directory and thus your init
file; this way, even if you have su’d, Emacs still loads your own init
file. If those environment variables are absent, though, Emacs uses
your user-id to find your home directory.

Emacs also attempts to load a second init file, called the
*early init file*, if it exists. This is a file named
`early-init.el` in your `~/.emacs.d` directory. The
difference between the early init file and the regular init file is
that the early init file is loaded much earlier during the startup
process, so you can use it to customize some things that are
initialized before loading the regular init file. For example, you
can customize the process of initializing the package system, by
setting variables such as `package-load-list` or
`package-enable-at-startup`. See [Package Installation](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Package-Installation) in The GNU Emacs Manual.

An Emacs installation may have a *default init file*, which is a
Lisp library named `default.el`. Emacs finds this file through
the standard search path for libraries (see [How Programs Do Loading](#How-Programs-Do-Loading)). The Emacs distribution does not come with this file; it is
intended for local customizations. If the default init file exists,
it is loaded whenever you start Emacs. But your own personal init
file, if any, is loaded first; if it sets `inhibit-default-init`
to a non-`nil` value, then Emacs does not subsequently load the
`default.el` file. In batch mode, or if you specify ‘`-q`’
(or ‘`-Q`’), Emacs loads neither your personal init file nor
the default init file.

Another file for site-customization is `site-start.el`. Emacs
loads this *before* the user’s init file. You can inhibit the
loading of this file with the option ‘`--no-site-file`’.

User Option: **site-run-file** [¶](#index-site_002drun_002dfile)
:   This variable specifies the site-customization file to load before the
    user’s early init file and regular init file. Its normal value is
    `"site-start"`. The only way you can change it with real effect is
    to do so before dumping Emacs.

See [Init File Examples](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-Examples) in The GNU Emacs Manual, for
examples of how to make various commonly desired customizations in your
`.emacs` file.

User Option: **inhibit-default-init** [¶](#index-inhibit_002ddefault_002dinit)
:   If this variable is non-`nil`, it prevents Emacs from loading the
    default initialization library file. The default value is `nil`.

Variable: **before-init-hook** [¶](#index-before_002dinit_002dhook)
:   This normal hook is run, once, just before loading all the init files
    (`site-start.el`, your init file, and `default.el`).
    (The only way to change it with real effect is before dumping Emacs.)

Variable: **after-init-hook** [¶](#index-after_002dinit_002dhook)
:   This normal hook is run, once, just after loading all the init files
    (`site-start.el`, your init file, and `default.el`),
    before loading the terminal-specific library (if started on a text
    terminal) and processing the command-line action arguments.

Variable: **emacs-startup-hook** [¶](#index-emacs_002dstartup_002dhook)
:   This normal hook is run, once, just after handling the command line
    arguments. In batch mode, Emacs does not run this hook.

Variable: **window-setup-hook** [¶](#index-window_002dsetup_002dhook)
:   This normal hook is very similar to `emacs-startup-hook`.
    The only difference is that it runs slightly later, after setting
    of the frame parameters. See [window-setup-hook](#Startup-Summary).

Variable: **user-init-file** [¶](#index-user_002dinit_002dfile)
:   This variable holds the absolute file name of the user’s init file. If the
    actual init file loaded is a compiled file, such as `.emacs.elc`,
    the value refers to the corresponding source file.

Variable: **user-emacs-directory** [¶](#index-user_002demacs_002ddirectory)
:   This variable holds the name of the Emacs default directory.
    It defaults to `${XDG_CONFIG_HOME-'~/.config'}/emacs/`
    if that directory exists and `~/.emacs.d/` and `~/.emacs` do
    not exist,
    otherwise to `~/.emacs.d/` on all platforms but MS-DOS.
    Here, `${XDG_CONFIG_HOME-'~/.config'}`
    stands for the value of the environment variable `XDG_CONFIG_HOME`
    if that variable is set, and for `~/.config` otherwise.
    See [How Emacs Finds Your Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Find-Init) in The GNU Emacs Manual.

---

Next: [Command-Line Arguments](#Command_002dLine-Arguments), Previous: [The Init File](#Init-File), Up: [Starting Up Emacs](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.1.3 Terminal-Specific Initialization [¶](#Terminal_002dSpecific-Initialization)

Each terminal type can have its own Lisp library that Emacs loads when
run on that type of terminal. The library’s name is constructed by
concatenating the value of the variable `term-file-prefix` and the
terminal type (specified by the environment variable `TERM`).
Normally, `term-file-prefix` has the value `"term/"`;
changing this is not recommended. If there is an entry matching
`TERM` in the `term-file-aliases` association list,
Emacs uses the associated value in place of `TERM`.
Emacs finds the file in the normal manner, by searching the
`load-path` directories, and trying the ‘`.elc`’ and
‘`.el`’ suffixes.

The usual role of a terminal-specific library is to enable special
keys to send sequences that Emacs can recognize. It may also need to
set or add to `input-decode-map` if the Termcap or Terminfo entry
does not specify all the terminal’s function keys. See [Terminal Input](#Terminal-Input).

When the name of the terminal type contains a hyphen or underscore,
and no library is found whose name is identical to the terminal’s
name, Emacs strips from the terminal’s name the last hyphen or
underscore and everything that follows
it, and tries again. This process is repeated until Emacs finds a
matching library, or until there are no more hyphens or underscores in the name
(i.e., there is no terminal-specific library). For example, if the
terminal name is ‘`xterm-256color`’ and there is no
`term/xterm-256color.el` library, Emacs tries to load
`term/xterm.el`. If necessary, the terminal library can evaluate
`(getenv "TERM")` to find the full name of the terminal type.

Your init file can prevent the loading of the terminal-specific
library by setting the variable `term-file-prefix` to `nil`.

You can also arrange to override some of the actions of the
terminal-specific library by using `tty-setup-hook`. This is
a normal hook that Emacs runs after initializing a new text terminal.
You could use this hook to define initializations for terminals that do not
have their own libraries. See [Hooks](#Hooks).

User Option: **term-file-prefix** [¶](#index-term_002dfile_002dprefix)
:   If the value of this variable is non-`nil`, Emacs loads a
    terminal-specific initialization file as follows:

    ```
    (load (concat term-file-prefix (getenv "TERM")))
    ```

    You may set the `term-file-prefix` variable to `nil` in your
    init file if you do not wish to load the
    terminal-initialization file.

    On MS-DOS, Emacs sets the `TERM` environment variable to ‘`internal`’.

User Option: **term-file-aliases** [¶](#index-term_002dfile_002daliases)
:   This variable is an association list mapping terminal types to
    their aliases. For example, an element of the form `("vt102"
    . "vt100")` means to treat a terminal of type ‘`vt102`’ like one of
    type ‘`vt100`’.

Variable: **tty-setup-hook** [¶](#index-tty_002dsetup_002dhook)
:   This variable is a normal hook that Emacs runs after initializing a
    new text terminal. (This applies when Emacs starts up in non-windowed
    mode, and when making a tty `emacsclient` connection.) The
    hook runs after loading your init file (if applicable) and the
    terminal-specific Lisp file, so you can use it to adjust the
    definitions made by that file.

    For a related feature, see [window-setup-hook](#Init-File).

---

Previous: [Terminal-Specific Initialization](#Terminal_002dSpecific), Up: [Starting Up Emacs](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.1.4 Command-Line Arguments [¶](#Command_002dLine-Arguments-1)

You can use command-line arguments to request various actions when
you start Emacs. Note that the recommended way of using Emacs is to
start it just once, after logging in, and then do all editing in the same
Emacs session (see [Entering Emacs](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Entering-Emacs) in The GNU Emacs Manual).
For this reason, you might not use command-line arguments very often;
nonetheless, they can be useful when invoking Emacs from session
scripts or debugging Emacs. This section describes how Emacs
processes command-line arguments.

Function: **command-line** [¶](#index-command_002dline)
:   This function parses the command line that Emacs was called with,
    processes it, and (amongst other things) loads the user’s init file and
    displays the startup messages.

Variable: **command-line-processed** [¶](#index-command_002dline_002dprocessed)
:   The value of this variable is `t` once the command line has been
    processed.

    If you redump Emacs by calling `dump-emacs-portable`
    (see [Building Emacs](#Building-Emacs)), you may wish to set this variable to
    `nil` first in order to cause the new dumped Emacs to process its
    new command-line arguments.

Variable: **command-switch-alist** [¶](#index-command_002dswitch_002dalist)
:   This variable is an alist of user-defined command-line options and
    associated handler functions. By default it is empty, but you can
    add elements if you wish.

    A *command-line option* is an argument on the command line, which
    has the form:

    ```
    -option
    ```

    The elements of the `command-switch-alist` look like this:

    ```
    (option . handler-function)
    ```

    The CAR, option, is a string, the name of a command-line
    option (including the initial hyphen). The handler-function
    is called to handle option, and receives the option name as its
    sole argument.

    In some cases, the option is followed in the command line by an
    argument. In these cases, the handler-function can find all the
    remaining command-line arguments in the variable
    `command-line-args-left` (see below). (The entire list of
    command-line arguments is in `command-line-args`.)

    Note that the handling of `command-switch-alist` doesn’t treat
    equals signs in option specially. That is, if there’s an option
    like `--name=value` on the command line, then only a
    `command-switch-alist` member whose `car` is literally
    `--name=value` will match this option. If you want to parse such
    options, you need to use `command-line-functions` instead (see
    below).

    The command-line arguments are parsed by the `command-line-1`
    function in the `startup.el` file. See also [Command Line Arguments for Emacs Invocation](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Emacs-Invocation) in The
    GNU Emacs Manual.

Variable: **command-line-args** [¶](#index-command_002dline_002dargs)
:   The value of this variable is the list of command-line arguments passed
    to Emacs.

Variable: **command-line-args-left** [¶](#index-command_002dline_002dargs_002dleft)
:   The value of this variable is the list of command-line arguments that
    have not yet been processed.

Variable: **command-line-functions** [¶](#index-command_002dline_002dfunctions)
:   This variable’s value is a list of functions for handling an
    unrecognized command-line argument. Each time the next argument to be
    processed has no special meaning, the functions in this list are called,
    in order of appearance, until one of them returns a non-`nil`
    value.

    These functions are called with no arguments. They can access the
    command-line argument under consideration through the variable
    `argi`, which is bound temporarily at this point. The remaining
    arguments (not including the current one) are in the variable
    `command-line-args-left`.

    When a function recognizes and processes the argument in `argi`, it
    should return a non-`nil` value to say it has dealt with that
    argument. If it has also dealt with some of the following arguments, it
    can indicate that by deleting them from `command-line-args-left`.

    If all of these functions return `nil`, then the argument is treated
    as a file name to visit.

---

Next: [Operating System Environment](#System-Environment), Previous: [Starting Up Emacs](#Starting-Up), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.2 Getting Out of Emacs [¶](#Getting-Out-of-Emacs)

There are two ways to get out of Emacs: you can kill the Emacs job,
which exits permanently, or you can suspend it, which permits you to
reenter the Emacs process later. (In a graphical environment, you can
of course simply switch to another application without doing anything
special to Emacs, then switch back to Emacs when you want.)

* [Killing Emacs](#Killing-Emacs)
* [Suspending Emacs](#Suspending-Emacs)

---

Next: [Suspending Emacs](#Suspending-Emacs), Up: [Getting Out of Emacs](#Getting-Out)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.2.1 Killing Emacs [¶](#Killing-Emacs-1)

Killing Emacs means ending the execution of the Emacs process.
If you started Emacs from a terminal, the parent process normally
resumes control. The low-level primitive for killing Emacs is
`kill-emacs`.

Command: **kill-emacs** &optional exit-data restart [¶](#index-kill_002demacs)
:   This command calls the hook `kill-emacs-hook`, then exits the
    Emacs process and kills it.

    If exit-data is an integer, that is used as the exit status of
    the Emacs process. (This is useful primarily in batch operation; see
    [Batch Mode](#Batch-Mode).)

    If exit-data is a string, its contents (followed by a newline) are
    stuffed into the terminal input buffer so that the shell (or whatever
    program next reads input) can read them. However, stuffing is silently
    skipped if Emacs is not running interactively on a terminal, or if
    stuffing requires special privileges or is not supported on this platform.

    If exit-data is neither an integer nor a string, or is omitted,
    that means to use the (system-specific) exit status which indicates
    successful program termination.

    If restart is non-`nil`, instead of just exiting at the
    end, start a new Emacs process, using the same command line arguments
    as the currently running Emacs process.

The `kill-emacs` function is normally called via the
higher-level command `C-x C-c`
(`save-buffers-kill-terminal`). See [Exiting](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Exiting) in The GNU
Emacs Manual. It is also called automatically if Emacs receives a
`SIGTERM` or `SIGHUP` operating system signal (e.g., when the
controlling terminal is disconnected), or if it receives a
`SIGINT` signal while running in batch mode (see [Batch Mode](#Batch-Mode)).

Variable: **kill-emacs-hook** [¶](#index-kill_002demacs_002dhook)
:   This normal hook is run by `kill-emacs`, before it kills Emacs.

    Because `kill-emacs` can be called in situations where user
    interaction is impossible (e.g., when the terminal is disconnected),
    functions on this hook should not attempt to interact with the user.
    If you want to interact with the user when Emacs is shutting down, use
    `kill-emacs-query-functions`, described below.

When Emacs is killed, all the information in the Emacs process,
aside from files that have been saved, is lost. Because killing Emacs
inadvertently can lose a lot of work, the
`save-buffers-kill-terminal` command queries for confirmation if
you have buffers that need saving or subprocesses that are running.
It also runs the abnormal hook `kill-emacs-query-functions`:

User Option: **kill-emacs-query-functions** [¶](#index-kill_002demacs_002dquery_002dfunctions)
:   When `save-buffers-kill-terminal` is killing Emacs, it calls the
    functions in this hook, after asking the standard questions and before
    calling `kill-emacs`. The functions are called in order of
    appearance, with no arguments. Each function can ask for additional
    confirmation from the user. If any of them returns `nil`,
    `save-buffers-kill-emacs` does not kill Emacs, and does not run
    the remaining functions in this hook. Calling `kill-emacs`
    directly does not run this hook.

Command: **restart-emacs** [¶](#index-restart_002demacs)
:   This command does the same as `save-buffers-kill-emacs`, but
    instead of just killing the current Emacs process at the end, it’ll
    restart a new Emacs process, using the same command line arguments as
    the currently running Emacs process.

---

Previous: [Killing Emacs](#Killing-Emacs), Up: [Getting Out of Emacs](#Getting-Out)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.2.2 Suspending Emacs [¶](#Suspending-Emacs-1)

On text terminals, it is possible to *suspend Emacs*, which
means stopping Emacs temporarily and returning control to its superior
process, which is usually the shell. This allows you to resume
editing later in the same Emacs process, with the same buffers, the
same kill ring, the same undo history, and so on. To resume Emacs,
use the appropriate command in the parent shell—most likely
`fg`.

Suspending works only on a terminal device from which the Emacs
session was started. We call that device the *controlling
terminal* of the session. Suspending is not allowed if the
controlling terminal is a graphical terminal. Suspending is usually
not relevant in graphical environments, since you can simply switch to
another application without doing anything special to Emacs.

Some operating systems (those without `SIGTSTP`, or MS-DOS) do
not support suspension of jobs; on these systems, suspension
actually creates a new shell temporarily as a subprocess of Emacs.
Then you would exit the shell to return to Emacs.

Command: **suspend-emacs** &optional string [¶](#index-suspend_002demacs)
:   This function stops Emacs and returns control to the superior process.
    If and when the superior process resumes Emacs, `suspend-emacs`
    returns `nil` to its caller in Lisp.

    This function works only on the controlling terminal of the Emacs
    session; to relinquish control of other tty devices, use
    `suspend-tty` (see below). If the Emacs session uses more than
    one terminal, you must delete the frames on all the other terminals
    before suspending Emacs, or this function signals an error.
    See [Multiple Terminals](#Multiple-Terminals).

    If string is non-`nil`, its characters are sent to Emacs’s
    superior shell, to be read as terminal input.
    However, this action is skipped if Emacs is not running interactively on
    a terminal, or if stuffing characters into the terminal input requires
    special privileges or is not supported on this platform.

    Before suspending, `suspend-emacs` runs the normal hook
    `suspend-hook`. After the user resumes Emacs,
    `suspend-emacs` runs the normal hook `suspend-resume-hook`.
    See [Hooks](#Hooks).

    The next redisplay after resumption will redraw the entire screen,
    unless the variable `no-redraw-on-reenter` is non-`nil`.
    See [Refreshing the Screen](#Refresh-Screen).

    Here is an example of how you could use these hooks:

    ```
    (add-hook 'suspend-hook
              (lambda () (or (y-or-n-p "Really suspend?")
                             (error "Suspend canceled"))))
    ```

    ```
    (add-hook 'suspend-resume-hook (lambda () (message "Resumed!")
                                     (sit-for 2)))
    ```

    Here is what you would see upon evaluating `(suspend-emacs)`:

    ```
    ---------- Buffer: Minibuffer ----------
    Really suspend? y
    ---------- Buffer: Minibuffer ----------
    ```

    ```

    ```

    ```
    ---------- Parent Shell ----------
    bash$ fg
    ```

    ```

    ```

    ```
    ---------- Echo Area ----------
    Resumed!
    ```

    Note that on some operating systems, sending string to the Emacs
    parent shell might require special privileges, in which case it might
    silently fail to send string to the shell for execution. On other
    systems this is not supported, and Emacs will signal an error if you
    try. Also, string might not be echoed, even if it is executed by
    the shell. So we don’t recommend relying on this feature in portable
    Lisp programs.

Variable: **suspend-hook** [¶](#index-suspend_002dhook)
:   This variable is a normal hook that Emacs runs before suspending.

Variable: **suspend-resume-hook** [¶](#index-suspend_002dresume_002dhook)
:   This variable is a normal hook that Emacs runs on resuming
    after a suspension.

Function: **suspend-tty** &optional tty [¶](#index-suspend_002dtty)
:   If tty specifies a terminal device used by Emacs, this function
    relinquishes the device and restores it to its prior state. Frames
    that used the device continue to exist, but are not updated and Emacs
    doesn’t read input from them. tty can be a terminal object, a
    frame (meaning the terminal for that frame), or `nil` (meaning
    the terminal for the selected frame). See [Multiple Terminals](#Multiple-Terminals).

    If tty is already suspended, this function does nothing.

    This function runs the hook `suspend-tty-functions`, passing the
    terminal object as an argument to each function.

Function: **resume-tty** &optional tty [¶](#index-resume_002dtty)
:   This function resumes the previously suspended terminal device
    tty; where tty has the same possible values as it does
    for `suspend-tty`.

    This function reopens the terminal device, re-initializes it, and
    redraws it with that terminal’s selected frame. It then runs the
    hook `resume-tty-functions`, passing the terminal object as an
    argument to each function.

    If the same device is already used by another Emacs terminal, this
    function signals an error. If tty is not suspended, this
    function does nothing.

Function: **controlling-tty-p** &optional tty [¶](#index-controlling_002dtty_002dp)
:   This function returns non-`nil` if tty is the
    controlling terminal of the Emacs session; tty can be a
    terminal object, a frame (meaning the terminal for that frame), or
    `nil` (meaning the terminal for the selected frame).

Command: **suspend-frame** [¶](#index-suspend_002dframe)
:   This command *suspends* a frame. For GUI frames, it calls
    `iconify-frame` (see [Visibility of Frames](#Visibility-of-Frames)); for frames on
    text terminals, it calls either `suspend-emacs` or
    `suspend-tty`, depending on whether the frame is displayed on the
    controlling terminal device or not.

---

Next: [User Identification](#User-Identification), Previous: [Getting Out of Emacs](#Getting-Out), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.3 Operating System Environment [¶](#Operating-System-Environment)

Emacs provides access to variables in the operating system environment
through various functions. These variables include the name of the
system, the user’s UID, and so on.

Variable: **system-configuration** [¶](#index-system_002dconfiguration)
:   This variable holds the standard GNU configuration name for the
    hardware/software configuration of your system, as a string. For
    example, a typical value for a 64-bit GNU/Linux system is
    ‘`"x86_64-unknown-linux-gnu"`’.

Variable: **system-type** [¶](#index-system_002dtype)
:   The value of this variable is a symbol indicating the type of operating
    system Emacs is running on. The possible values are:

    `aix`
    :   IBM’s AIX.

    `berkeley-unix`
    :   Berkeley BSD and its variants.

    `cygwin`
    :   Cygwin, a POSIX layer on top of MS-Windows.

    `darwin`
    :   Darwin (macOS).

    `gnu`
    :   The GNU system (using the GNU kernel, which consists of the HURD and Mach).

    `gnu/linux`
    :   A GNU/Linux system—that is, a variant GNU system, using the Linux
        kernel. (These systems are the ones people often call “Linux”, but
        actually Linux is just the kernel, not the whole system.)

    `gnu/kfreebsd`
    :   A GNU (glibc-based) system with a FreeBSD kernel.

    `haiku`
    :   The Haiku operating system, a derivative of the Be Operating System.

    `hpux`
    :   Hewlett-Packard HPUX operating system.

    `nacl`
    :   Google Native Client (NaCl) sandboxing system.

    `android`
    :   The Open Handset Alliance’s Android operating system.

    `ms-dos`
    :   Microsoft’s DOS. Emacs compiled with DJGPP for MS-DOS binds
        `system-type` to `ms-dos` even when you run it on MS-Windows.

    `usg-unix-v`
    :   AT&T Unix System V.

    `windows-nt`
    :   Microsoft Windows NT, 9X and later. The value of `system-type`
        is always `windows-nt`, e.g., even on Windows 10.

    We do not wish to add new symbols to make finer distinctions unless it
    is absolutely necessary! In fact, we hope to eliminate some of these
    alternatives in the future. If you need to make a finer distinction
    than `system-type` allows for, you can test
    `system-configuration`, e.g., against a regexp.

Function: **system-name** [¶](#index-system_002dname)
:   This function returns the name of the machine you are running on, as a
    string.

User Option: **mail-host-address** [¶](#index-mail_002dhost_002daddress)
:   If this variable is non-`nil`, it is used instead of
    `system-name` for purposes of generating email addresses. For
    example, it is used when constructing the default value of
    `user-mail-address`. See [User Identification](#User-Identification).

Command: **getenv** var &optional frame [¶](#index-getenv)
:   This function returns the value of the environment variable var,
    as a string. var should be a string. If var is undefined
    in the environment, `getenv` returns `nil`. It returns
    ‘`""`’ if var is set but null. Within Emacs, a list of environment
    variables and their values is kept in the variable `process-environment`.

    Optional argument frame, if non-`nil`, specifies a frame, in
    which case the function searches frame’s parameter
    `environment` (see [Basic Parameters](#Basic-Parameters)) for var instead of
    looking for it in `process-environment`.

    The ‘`DISPLAY`’ environment variable is treated specially when
    frame is omitted or `nil`: if that variable is not found in
    `process-environment`, the function looks at the selected-frame’s
    `display` parameter, and if that is `nil`, it also looks in
    `initial-environment`.

    ```
    (getenv "USER")
         ⇒ "lewis"
    ```

    The shell command `printenv` prints all or part of the environment:

    ```
    bash$ printenv
    PATH=/usr/local/bin:/usr/bin:/bin
    USER=lewis
    ```

    ```
    TERM=xterm
    SHELL=/bin/bash
    HOME=/home/lewis
    ```

    ```
    ...
    ```

Command: **setenv** variable &optional value substitute [¶](#index-setenv)
:   This command sets the value of the environment variable named
    variable to value. variable should be a string.
    Internally, Emacs Lisp can handle any string. However, normally
    variable should be a valid shell identifier, that is, a sequence
    of letters, digits and underscores, starting with a letter or
    underscore. Otherwise, errors may occur if subprocesses of Emacs try
    to access the value of variable. If value is omitted or
    `nil` (or, interactively, with a prefix argument), `setenv`
    removes variable from the environment. Otherwise, value
    should be a string.

    If the optional argument substitute is non-`nil`, Emacs
    calls the function `substitute-env-vars` to expand any
    environment variables in value.

    `setenv` works by modifying `process-environment`; binding
    that variable with `let` is also reasonable practice.

    `setenv` returns the new value of variable, or `nil`
    if it removed variable from the environment.

Macro: **with-environment-variables** variables body… [¶](#index-with_002denvironment_002dvariables)
:   This macro sets the environment variables according to variables
    temporarily when executing body. The previous values are
    restored when the form finishes. The argument variables should
    be a list of pairs of strings of the form
    `(var value)`, where var is the name of the
    environment variable and value is that variable’s value.

    ```
    (with-environment-variables (("LANG" "C")
                                 ("LANGUAGE" "en_US:en"))
      (call-process "ls" nil t))
    ```

Variable: **process-environment** [¶](#index-process_002denvironment)
:   This variable is a list of strings, each describing one environment
    variable. The functions `getenv` and `setenv` work by means
    of this variable.

    ```
    process-environment
    ⇒ ("PATH=/usr/local/bin:/usr/bin:/bin"
        "USER=lewis"
    ```

    ```
        "TERM=xterm"
        "SHELL=/bin/bash"
        "HOME=/home/lewis"
        ...)
    ```

    If `process-environment` contains multiple elements that
    specify the same environment variable, the first of these elements
    specifies the variable, and the others are ignored.

Variable: **initial-environment** [¶](#index-initial_002denvironment)
:   This variable holds the list of environment variables Emacs inherited
    from its parent process when Emacs started.

Variable: **path-separator** [¶](#index-path_002dseparator-1)
:   This variable holds a string that says which character separates
    directories in a search path (as found in an environment variable). Its
    value is `":"` for Unix and GNU systems, and `";"` for MS systems.

Function: **path-separator** [¶](#index-path_002dseparator)
:   This function returns the connection-local value of variable
    `path-separator`. That is `";"` for MS systems and a local
    `default-directory`, and `":"` for Unix and GNU systems, or
    a remote `default-directory`.

Function: **parse-colon-path** path [¶](#index-parse_002dcolon_002dpath)
:   This function takes a search path string such as the value of
    the `PATH` environment variable, and splits it at the separators,
    returning a list of directories. `nil` in this list means
    the current directory. Although the function’s name says
    “colon”, it actually uses the value of variable `path-separator`.

    ```
    (parse-colon-path ":/foo:/bar")
         ⇒ (nil "/foo/" "/bar/")
    ```

Variable: **invocation-name** [¶](#index-invocation_002dname)
:   This variable holds the program name under which Emacs was invoked. The
    value is a string, and does not include a directory name.

Variable: **invocation-directory** [¶](#index-invocation_002ddirectory)
:   This variable holds the directory in which the Emacs executable was
    located when it was run, or `nil` if that directory cannot be
    determined.

Variable: **installation-directory** [¶](#index-installation_002ddirectory)
:   If non-`nil`, this is a directory within which to look for the
    `lib-src` and `etc` subdirectories. In an installed Emacs,
    it is normally `nil`. It is non-`nil`
    when Emacs can’t find those directories in their standard installed
    locations, but can find them in a directory related somehow to the one
    containing the Emacs executable (i.e., `invocation-directory`).

Function: **load-average** &optional use-float [¶](#index-load_002daverage)
:   This function returns the current 1-minute, 5-minute, and 15-minute
    system load averages, in a list. The load average indicates the
    number of processes trying to run on the system.

    By default, the values are integers that are 100 times the system load
    averages, but if use-float is non-`nil`, then they are
    returned as floating-point numbers without multiplying by 100.

    If it is impossible to obtain the load average, this function signals
    an error. On some platforms, access to load averages requires
    installing Emacs as setuid or setgid so that it can read kernel
    information, and that usually isn’t advisable.

    If the 1-minute load average is available, but the 5- or 15-minute
    averages are not, this function returns a shortened list containing
    the available averages.

    ```
    (load-average)
         ⇒ (169 48 36)
    ```

    ```
    (load-average t)
         ⇒ (1.69 0.48 0.36)
    ```

    The shell command `uptime` returns similar information.

Function: **emacs-pid** [¶](#index-emacs_002dpid)
:   This function returns the process ID of the Emacs process,
    as an integer.

Variable: **tty-erase-char** [¶](#index-tty_002derase_002dchar)
:   This variable holds the erase character that was selected
    in the system’s terminal driver, before Emacs was started.

Variable: **null-device** [¶](#index-null_002ddevice-1)
:   This variable holds the system null device. Its value is
    `"/dev/null"` for Unix and GNU systems, and `"NUL"` for MS
    systems.

Function: **null-device** [¶](#index-null_002ddevice)
:   This function returns the connection-local value of variable
    `null-device`. That is `"NUL"` for MS systems and a local
    `default-directory`, and `"/dev/null"` for Unix and GNU
    systems, or a remote `default-directory`.

---

Next: [Time of Day](#Time-of-Day), Previous: [Operating System Environment](#System-Environment), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.4 User Identification [¶](#User-Identification-1)

Variable: **init-file-user** [¶](#index-init_002dfile_002duser)
:   This variable says which user’s init files should be used by
    Emacs—or `nil` if none. `""` stands for the user who
    originally logged in. The value reflects command-line options such as
    ‘`-q`’ or ‘`-u user`’.

    Lisp packages that load files of customizations, or any other sort of
    user profile, should obey this variable in deciding where to find it.
    They should load the profile of the user name found in this variable.
    If `init-file-user` is `nil`, meaning that the ‘`-q`’,
    ‘`-Q`’, or ‘`-batch`’ option was used, then Lisp packages should
    not load any customization files or user profile.

User Option: **user-mail-address** [¶](#index-user_002dmail_002daddress)
:   This holds the email address of the user who is using Emacs.

Function: **user-login-name** &optional uid [¶](#index-user_002dlogin_002dname)
:   This function returns the name under which the user is logged in.
    It uses the environment variables `LOGNAME` or `USER` if
    either is set. Otherwise, the value is based on the effective
    UID, not the real UID.

    If you specify uid (a number), the result is the user name that
    corresponds to uid, or `nil` if there is no such user.

Function: **user-real-login-name** [¶](#index-user_002dreal_002dlogin_002dname)
:   This function returns the user name corresponding to Emacs’s real
    UID. This ignores the effective UID, and the
    environment variables `LOGNAME` and `USER`.

Function: **user-full-name** &optional uid [¶](#index-user_002dfull_002dname)
:   This function returns the full name of the logged-in user—or the value
    of the environment variable `NAME`, if that is set.

    If the Emacs process’s user-id does not correspond to any known user (and
    provided `NAME` is not set), the result is `"unknown"`.

    If uid is non-`nil`, then it should be a number (a user-id)
    or a string (a login name). Then `user-full-name` returns the full
    name corresponding to that user-id or login name. If you specify a
    user-id or login name that isn’t defined, it returns `nil`.

The symbols `user-login-name`, `user-real-login-name` and
`user-full-name` are variables as well as functions. The functions
return the same values that the variables hold. These variables allow
you to fake out Emacs by telling the functions what to return. The
variables are also useful for constructing frame titles (see [Frame Titles](#Frame-Titles)).

Function: **user-real-uid** [¶](#index-user_002dreal_002duid)
:   This function returns the real UID of the user.

Function: **user-uid** [¶](#index-user_002duid)
:   This function returns the effective UID of the user.

Function: **file-user-uid** [¶](#index-file_002duser_002duid)
:   This function returns the connection-local value for the user’s
    effective UID. If `default-directory` is local, this
    is equivalent to `user-uid`, but for remote files (see [Remote
    Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual), it will return the
    UID for the user associated with that remote connection; if
    the remote connection has no associated user, it will instead return
    -1.

Function: **group-real-gid** [¶](#index-group_002dreal_002dgid)
:   This function returns the real GID of the Emacs process.

Function: **group-gid** [¶](#index-group_002dgid)
:   This function returns the effective GID of the Emacs process.

Function: **file-group-gid** [¶](#index-file_002dgroup_002dgid)
:   This function returns the connection-local value for the user’s
    effective GID. Similar to `file-user-uid`, if
    `default-directory` is local, this is equivalent to
    `group-gid`, but for remote files (see [Remote Files](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Remote-Files) in The GNU Emacs Manual), it will return the GID for
    the user associated with that remote connection; if the remote
    connection has no associated user, it will instead return -1.

Function: **system-users** [¶](#index-system_002dusers)
:   This function returns a list of strings, listing the user names on the
    system. If Emacs cannot retrieve this information, the return value
    is a list containing just the value of `user-real-login-name`.

Function: **system-groups** [¶](#index-system_002dgroups)
:   This function returns a list of strings, listing the names of user
    groups on the system. If Emacs cannot retrieve this information, the
    return value is `nil`.

Function: **group-name** gid [¶](#index-group_002dname)
:   This function returns the group name that corresponds to the numeric
    group ID gid, or `nil` if there is no such group.

---

Next: [Time Zone Rules](#Time-Zone-Rules), Previous: [User Identification](#User-Identification), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.5 Time of Day [¶](#Time-of-Day-1)

This section explains how to determine the current time and time
zone.

Many functions like `current-time` and `file-attributes`
return *Lisp timestamp* values that count seconds, and that can
represent absolute time by counting seconds since the *epoch* of
1970-01-01 00:00:00 UTC (Coordinated Universal Time). Typically these
counts ignore leap seconds; however, GNU and some other operating
systems can be configured to count leap seconds.

Although traditionally Lisp timestamps were integer pairs, their
form has evolved and programs ordinarily should not depend on the
current default form. If your program needs a particular timestamp
form, you can use the `time-convert` function to convert it to the
needed form. See [Time Conversion](#Time-Conversion).

There are currently three forms of Lisp timestamps, each of
which represents a number of seconds:

* An integer. Although this is the simplest form, it cannot represent
  subsecond timestamps.
* A pair of integers `(ticks . hz)`, where hz is
  positive. This represents ticks/hz seconds, which is the
  same time as plain ticks if hz is 1. A common value for
  hz is 1000000000, for a nanosecond-resolution
  clock.
* A list of four integers `(high low micro
  pico)`, where 0≤low<65536,
  0≤micro<1000000, and 0≤pico<1000000.
  This represents the number of seconds using the formula:
  high \* 2\*\*16 + low + micro \* 10\*\*−6 +
  pico \* 10\*\*−12.
  If `current-time-list` is `t`,
  some functions may default to returning two- or
  three-element lists, with omitted micro and pico
  components defaulting to zero.
  On all current machines pico is a multiple of 1000, but this
  may change as higher-resolution clocks become available.

Function arguments, e.g., the time argument to
`format-time-string`, accept a more-general *time value*
format, which can be a Lisp timestamp, `nil` for the current
time, a finite floating-point number for seconds, or a list
`(high low micro)` or `(high
low)` that is a truncated list timestamp with missing elements
taken to be zero.

Time values can be converted to and from calendrical and other forms.
Some of these conversions rely on operating system functions that
limit the range of possible time values, and signal an error such as
‘`"Specified time is not representable"`’ if the
limits are exceeded. For instance, a system might not support
timestamps before the epoch, or years far in the future.
You can convert a time value into
a human-readable string using `format-time-string`, into a Lisp
timestamp using `time-convert`, and into other forms using
`decode-time` and `float-time`. These functions are
described in the following sections.

Function: **current-time-string** &optional time zone [¶](#index-current_002dtime_002dstring)
:   This function returns the current time and date as a human-readable
    string. The format does not vary for the initial part of the string,
    which contains the day of week, month, day of month, and time of day
    in that order: the number of characters used for these fields is
    always the same, although (unless you require English weekday or
    month abbreviations regardless of locale) it is typically more
    convenient to use `format-time-string` than to extract
    fields from the output of `current-time-string`,
    as the year might not have exactly four digits, and additional
    information may some day be added at the end.

    The argument time, if given, specifies a time to format,
    instead of the current time. The optional argument zone
    defaults to the current time zone rule. See [Time Zone Rules](#Time-Zone-Rules).
    The operating system limits the range of time and zone values.

    ```
    (current-time-string)
         ⇒ "Fri Nov  1 15:59:49 2019"
    ```

Variable: **current-time-list** [¶](#index-current_002dtime_002dlist)
:   This boolean variable is a transition aid. If `t`,
    `current-time` and related functions return timestamps in list
    form, typically `(high low micro pico)`;
    otherwise, they use `(ticks . hz)` form. Currently
    this variable defaults to `t`, for behavior compatible with
    previous Emacs versions. Developers are encouraged to test
    timestamp-related code with this variable set to `nil`, as it
    will default to `nil` in a future Emacs version, and will be
    removed in some version after that.

Function: **current-time** [¶](#index-current_002dtime)
:   This function returns the current time as a Lisp timestamp.
    If `current-time-list` is `nil`,
    the timestamp has the form `(ticks . hz)` where
    ticks counts clock ticks and hz is the clock ticks per second.
    Otherwise, the timestamp has the list form
    `(high low usec psec)`.
    You can use `(time-convert nil t)` or `(time-convert nil 'list)`
    to obtain a particular form regardless of the value of
    `current-time-list`. See [Time Conversion](#Time-Conversion).

Function: **float-time** &optional time [¶](#index-float_002dtime)
:   This function returns the current time as a floating-point number of
    seconds since the epoch. The optional argument time, if
    given, specifies a time to convert instead of the current time.

    *Warning*: Since the result is floating point, it may not be
    exact. Do not use this function if precise time stamps are required.
    For example, on typical systems `(float-time '(1 . 10))` displays
    as ‘`0.1`’ but is slightly greater than 1/10.

    `time-to-seconds` is an alias for this function.

Function: **current-cpu-time** [¶](#index-current_002dcpu_002dtime)
:   Return the current CPU time along with its resolution. The
    return value is a pair `(CPU-TICKS . TICKS-PER-SEC)`. The
    CPU-TICKS counter can wrap around, so values cannot be
    meaningfully compared if too much time has passed between them.

---

Next: [Time Conversion](#Time-Conversion), Previous: [Time of Day](#Time-of-Day), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.6 Time Zone Rules [¶](#Time-Zone-Rules-1)

The default time zone is determined by the `TZ` environment
variable. See [Operating System Environment](#System-Environment). For example, you can tell Emacs
to default to Universal Time with `(setenv "TZ" "UTC0")`. If
`TZ` is not in the environment, Emacs uses system wall clock time,
which is a platform-dependent default time zone.

The set of supported `TZ` strings is system-dependent. GNU and
many other systems support TZDB timezones, e.g.,
‘`"America/New_York"`’ specifies the time zone and daylight saving
time history for locations near New York City. GNU and most other
systems support POSIX-style `TZ` strings, e.g.,
‘`"EST5EDT,M4.1.0,M10.5.0"`’ specifies the rules used in New
York from 1987 through 2006. All systems support the string
‘`"UTC0"`’ meaning Universal Time.

Functions that convert to and from local time accept an optional
*time zone rule* argument, which specifies the conversion’s time
zone and daylight saving time history. If the time zone rule is
omitted or `nil`, the conversion uses Emacs’s default time zone.
If it is `t`, the conversion uses Universal Time. If it is
`wall`, the conversion uses the system wall clock time. If it is
a string, the conversion uses the time zone rule equivalent to setting
`TZ` to that string. If it is a list (offset abbr), where
offset is an integer number of seconds east of Universal Time
and abbr is a string, the conversion uses a fixed time zone with
the given offset and abbreviation. An integer offset is treated
as if it were (offset abbr), where abbr is a numeric
abbreviation on POSIX-compatible platforms and is unspecified on
MS-Windows.

Function: **current-time-zone** &optional time zone [¶](#index-current_002dtime_002dzone)
:   This function returns a list describing the time zone that the user is
    in.

    The value has the form `(offset abbr)`. Here
    offset is an integer giving the number of seconds ahead of Universal Time
    (east of Greenwich). A negative value means west of Greenwich. The
    second element, abbr, is a string giving an abbreviation for the
    time zone, e.g., ‘`"CST"`’ for China Standard Time or for
    U.S. Central Standard Time. Both elements can change when daylight
    saving time begins or ends; if the user has specified a time zone that
    does not use a seasonal time adjustment, then the value is constant
    through time.

    If the operating system doesn’t supply all the information necessary to
    compute the value, the unknown elements of the list are `nil`.

    The argument time, if given, specifies a time value to
    analyze instead of the current time. The optional argument zone
    defaults to the current time zone rule.
    The operating system limits the range of time and zone values.

---

Next: [Parsing and Formatting Times](#Time-Parsing), Previous: [Time Zone Rules](#Time-Zone-Rules), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.7 Time Conversion [¶](#Time-Conversion-1)

These functions convert time values (see [Time of Day](#Time-of-Day)) to Lisp
timestamps, or into calendrical information and vice versa.

Many operating systems use 64-bit signed integers to count seconds,
and can represent times far in the past or future. However, some are
more limited. For example, old-fashioned operating systems that use
32-bit signed integers typically handle only times from 1901-12-13
20:45:52 through 2038-01-19 03:14:07 Universal Time.

Calendrical conversion functions use the Gregorian calendar even for
dates before the Gregorian calendar was introduced, and for dates in
the far distant past or future for which the Gregorian calendar
is wildly inaccurate and disagrees with common practice in scientific fields
like astronomy and paleontology, which use Julian-calendar year lengths.
Year numbers count since the year 1 BCE, and do not skip zero
as traditional Gregorian years do; for example, the year number
−37 represents the Gregorian year 38 BCE.

Function: **time-convert** time form [¶](#index-time_002dconvert)
:   This function converts a time value into a Lisp timestamp.

    The form argument specifies the timestamp form to be returned.
    If form is the symbol `integer`, this function returns an
    integer count of seconds. If form is a positive integer, it
    specifies a clock frequency and this function returns an integer-pair
    timestamp `(ticks . form)`. If form is
    `t`, this function treats it as a positive integer suitable for
    representing the timestamp; for example, it is treated as 1000000000
    if time is `nil` and the platform timestamp has nanosecond
    resolution. If form is `list`, this function returns an
    integer list `(high low micro pico)`.
    Although a `nil` form currently acts like
    `list`, this is planned to change in a future Emacs version, so
    callers requiring list timestamps should pass `list` explicitly.

    If time is not a time value, this function signals an error.
    Otherwise, if time cannot be represented exactly, conversion
    truncates it toward minus infinity. When form is `t`,
    conversion is always exact so no truncation occurs, and the returned
    clock resolution is no less than that of time. By way of
    contrast, although `float-time` can also convert any time value
    without signaling an error, the result might not be exact.
    See [Time of Day](#Time-of-Day).

    For efficiency this function might return a value that is `eq` to
    time, or that otherwise shares structure with time.

    Although `(time-convert nil nil)` is equivalent to
    `(current-time)`, the latter may be a bit faster.

    ```
    (setq a (time-convert nil t))
    ⇒ (1564826753904873156 . 1000000000)
    ```

    ```
    (time-convert a 100000)
    ⇒ (156482675390487 . 100000)
    ```

    ```
    (time-convert a 'integer)
    ⇒ 1564826753
    ```

    ```
    (time-convert a 'list)
    ⇒ (23877 23681 904873 156000)
    ```

Function: **decode-time** &optional time zone form [¶](#index-decode_002dtime)
:   This function converts a time value into calendrical information. If
    you don’t specify time, it decodes the current time, and similarly
    zone defaults to the current time zone rule. See [Time Zone Rules](#Time-Zone-Rules).
    The operating system limits the range of time and zone values.

    The form argument controls the form of the returned
    seconds element, as described below.
    The return value is a list of nine elements, as follows:

    ```
    (seconds minutes hour day month year dow dst utcoff)
    ```

    Here is what the elements mean:

    seconds
    :   The number of seconds past the minute, with form described below.

    minutes
    :   The number of minutes past the hour, as an integer between 0 and 59.

    hour
    :   The hour of the day, as an integer between 0 and 23.

    day
    :   The day of the month, as an integer between 1 and 31.

    month
    :   The month of the year, as an integer between 1 and 12.

    year
    :   The year, an integer typically greater than 1900.

    dow
    :   The day of week, as an integer between 0 and 6, where 0 stands for
        Sunday.

    dst
    :   `t` if daylight saving time is effect, `nil` if it is not
        in effect, and −1 if this information is not available.

    utcoff
    :   An integer indicating the Universal Time offset in seconds, i.e., the number of
        seconds east of Greenwich.

    The seconds element is a Lisp timestamp that is nonnegative and
    less than 61; it is less than 60 except during positive leap seconds
    (assuming the operating system supports leap seconds). If the
    optional form argument is `t`, seconds uses the same
    precision as time; if form is `integer`,
    seconds is truncated to an integer. For example, if time
    is the timestamp `(1566009571321 . 1000)`, which represents
    2019-08-17 02:39:31.321 UTC on typical systems that lack leap seconds,
    then `(decode-time time t t)` returns `((31321 . 1000)
    39 2 17 8 2019 6 nil 0)`, whereas `(decode-time time t
    'integer)` returns `(31 39 2 17 8 2019 6 nil 0)`. If form
    is omitted or `nil`, it currently defaults to `integer` but
    this default may change in future Emacs releases, so callers requiring
    a particular form should specify form.

    **Common Lisp Note:** Common Lisp has different meanings for
    dow, `dst` and utcoff, and its second is an
    integer between 0 and 59 inclusive.

    To access (or alter) the elements in the calendrical information, the
    `decoded-time-second`, `decoded-time-minute`,
    `decoded-time-hour`, `decoded-time-day`,
    `decoded-time-month`, `decoded-time-year`,
    `decoded-time-weekday`, `decoded-time-dst` and
    `decoded-time-zone` accessors can be used.

Function: **encode-time** time &rest obsolescent-arguments [¶](#index-encode_002dtime)
:   This function converts time to a Lisp timestamp.
    It can act as the inverse of `decode-time`.

    Ordinarily the first argument is a list
    `(second minute hour day month
    year ignored dst zone)` that specifies a
    decoded time in the style of `decode-time`. For the meanings of
    these list elements, see the table under `decode-time`.
    In particular, dst says how to interpret timestamps during a
    daylight saving fallback when timestamps are repeated.
    If dst is −1, the DST value is guessed; if it
    is `t` or `nil` the timestamp with that DST value
    is returned, with an error signaled if no such timestamp exists.
    Unfortunately a dst value of `t` or `nil` does not
    disambiguate timestamps duplicated when a TZDB-based timezone moves
    further west of Greenwich, such as disambiguating the two
    standard-time timestamps 2020-12-27 01:30 when zone is
    ‘`"Europe/Volgograd"`’, which at 02:00 that day changed
    standard time from 4 to 3 hours east of Greenwich; if you need to
    handle situations like this you can use a numeric zone to
    disambiguate instead.

    The first argument can also be a list `(second minute
    hour day month year)`, which is treated like
    the list `(second minute hour day
    month year nil -1 nil)`.

    As an obsolescent calling convention, this function can be given six
    or more arguments. The first six arguments second,
    minute, hour, day, month, and year
    specify most of the components of a decoded time. If there are more
    than six arguments the *last* argument is used as zone and
    any other extra arguments are ignored, so that `(apply
    #'encode-time (decode-time ...))` works. In this obsolescent
    convention, dst is −1 and zone defaults to the
    current time zone rule (see [Time Zone Rules](#Time-Zone-Rules)).
    When modernizing an obsolescent caller, ensure that the more-modern
    list equivalent contains 9 elements with a `dst` element that
    is −1, not `nil`.

    Year numbers less than 100 are not treated specially. If you want them
    to stand for years above 1900, or years above 2000, you must alter them
    yourself before you call `encode-time`.
    The operating system limits the range of time and zone values.
    However, timestamps ranging from the epoch to the near future are
    always supported.

    The `encode-time` function acts as a rough inverse to
    `decode-time`. For example, you can pass the output of
    the latter to the former as follows:

    ```
    (encode-time (decode-time ...))
    ```

    You can perform simple date arithmetic by using out-of-range values for
    seconds, minutes, hour, day, and month;
    for example, day 0 means the day preceding the given month.
    Take care when doing so, as it is common for this to fail in some cases.
    For example:

    ```
    ;; Try to compute the time one month from now.
    ;; Watch out; this might not work as expected.
    (let ((time (decode-time)))
      (setf (decoded-time-month time)
            (+ (decoded-time-month time) 1))
      time)
    ```

    Unfortunately, this code might not work as expected if the resulting
    time is invalid due to month length differences,
    daylight saving transitions, time zone changes,
    or missing leap days or leap seconds. For example, if executed on
    January 30 this code yields a nonexistent date February 30,
    which `encode-time` would adjust to early March.
    Similarly, adding four years to February 29, 2096 would yield the
    nonexistent date February 29, 2100; and adding one hour to 01:30 on
    March 13, 2022 in New York would yield a timestamp 02:30 that does not
    exist because clocks sprang forward from 02:00 to 03:00 that day.
    To avoid some (though not all) of the problem, you
    can base calculations on the middle of the affected unit, e.g., start
    at the 15th of the month when adding months. Alternatively, you can use the
    `calendar` and `time-date` libraries.

---

Next: [Processor Run time](#Processor-Run-Time), Previous: [Time Conversion](#Time-Conversion), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
