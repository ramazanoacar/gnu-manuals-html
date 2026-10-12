### AC.15.4 Shell Command History

Shell buffers support three ways of repeating earlier commands. You
can use the same keys used in the minibuffer; these work much as they do
in the minibuffer, inserting text from prior commands while point
remains always at the end of the buffer. You can move through the
buffer to previous inputs in their original place, then resubmit them or
copy them to the end. Or you can use a `` `!' ``-style history
reference.

> |  |  |  |
> | --- | --- | --- |
> | [AC.15.4.1 Shell History Ring](emacs.html#SEC447) |  | Fetching commands from the history list. |
> | [AC.15.4.2 Shell History Copying](emacs.html#SEC448) |  | Moving to a command and then copying it. |
> | [AC.15.4.3 Shell History References](emacs.html#SEC449) |  | Expanding `` `!' ``-style history references. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC446) ] | [ [>](emacs.html#SEC448) ] | [ [<<](emacs.html#SEC450) ] | [ [Up](emacs.html#SEC446) ] | [ [>>](emacs.html#SEC450) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### AC.15.4.1 Shell History Ring

`M-p`: Fetch the next earlier old shell command. `M-n`: Fetch the next later old shell command. `M-r regexp RET`: `M-s regexp RET`: Search backwards or forwards for old shell commands that match regexp. `C-c C-x (Shell mode)`: Fetch the next subsequent command from the history.

Shell buffers provide a history of previously entered shell commands. To
reuse shell commands from the history, use the editing commands `M-p`,
`M-n`, `M-r` and `M-s`. These work just like the minibuffer
history commands except that they operate on the text at the end of the
shell buffer, where you would normally insert text to send to the shell.

`M-p` fetches an earlier shell command to the end of the shell buffer.
Successive use of `M-p` fetches successively earlier shell commands,
each replacing any text that was already present as potential shell input.
`M-n` does likewise except that it finds successively more recent shell
commands from the buffer.

The history search commands `M-r` and `M-s` read a regular
expression and search through the history for a matching command. Aside
from the choice of which command to fetch, they work just like `M-p`
and `M-n`. If you enter an empty regexp, these commands reuse the
same regexp used last time.

When you find the previous input you want, you can resubmit it by
typing `RET`, or you can edit it first and then resubmit it if you
wish.

Often it is useful to reexecute several successive shell commands that
were previously executed in sequence. To do this, first find and
reexecute the first command of the sequence. Then type `C-c C-x`;
that will fetch the following command--the one that follows the command
you just repeated. Then type `RET` to reexecute this command. You
can reexecute several successive commands by typing `C-c C-x
RET` over and over.

These commands get the text of previous shell commands from a special
history list, not from the shell buffer itself. Thus, editing the shell
buffer, or even killing large parts of it, does not affect the history
that these commands access.

Some shells store their command histories in files so that you can
refer to previous commands from previous shell sessions. Emacs reads
the command history file for your chosen shell, to initialize its own
command history. The file name is `~/.bash\_history' for bash,
`~/.sh\_history' for ksh, and `~/.history' for other shells.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC447) ] | [ [>](emacs.html#SEC449) ] | [ [<<](emacs.html#SEC449) ] | [ [Up](emacs.html#SEC446) ] | [ [>>](emacs.html#SEC450) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### AC.15.4.2 Shell History Copying

`C-c C-p`: Move point to the previous prompt (`comint-previous-prompt`). `C-c C-n`: Move point to the following prompt (`comint-next-prompt`). `C-c RET`: Copy the input command which point is in, inserting the copy at the end of the buffer (`comint-copy-old-input`). This is useful if you move point back to a previous command. After you copy the command, you can submit the copy as input with `RET`. If you wish, you can edit the copy before resubmitting it.

Moving to a previous input and then copying it with `C-c
RET` produces the same results--the same buffer contents--that
you would get by using `M-p` enough times to fetch that previous
input from the history list. However, `C-c RET` copies the
text from the buffer, which can be different from what is in the history
list if you edit the input text in the buffer after it has been sent.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC448) ] | [ [>](emacs.html#SEC450) ] | [ [<<](emacs.html#SEC450) ] | [ [Up](emacs.html#SEC446) ] | [ [>>](emacs.html#SEC450) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

#### AC.15.4.3 Shell History References

Various shells including csh and bash support *history
references* that begin with `` `!' `` and `` `^' ``. Shell mode
recognizes these constructs, and can perform the history substitution
for you.

If you insert a history reference and type `TAB`, this searches
the input history for a matching command, performs substitution if
necessary, and places the result in the buffer in place of the history
reference. For example, you can fetch the most recent command
beginning with `` `mv' `` with `! m v TAB`. You can edit the
command if you wish, and then resubmit the command to the shell by
typing `RET`.

Shell mode can optionally expand history references in the buffer
when you send them to the shell. To request this, set the variable
`comint-input-autoexpand` to `input`. You can make
`SPC` perform history expansion by binding `SPC` to the
command `comint-magic-space`.

Shell mode recognizes history references when they follow a prompt.
Normally, any text output by a program at the beginning of an input
line is considered a prompt. However, if the variable
`comint-use-prompt-regexp-instead-of-fields` is non-`nil`,
then Comint mode uses a regular expression to recognize prompts. In
general, the variable `comint-prompt-regexp` specifies the
regular expression; Shell mode uses the variable
`shell-prompt-pattern` to set up `comint-prompt-regexp` in
the shell buffer.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC449) ] | [ [>](emacs.html#SEC451) ] | [ [<<](emacs.html#SEC451) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.5 Directory Tracking

Shell mode keeps track of `` `cd' ``, `` `pushd' `` and `` `popd' ``
commands given to the inferior shell, so it can keep the
`` `*shell*' `` buffer's default directory the same as the shell's
working directory. It recognizes these commands syntactically, by
examining lines of input that are sent.

If you use aliases for these commands, you can tell Emacs to
recognize them also. For example, if the value of the variable
`shell-pushd-regexp` matches the beginning of a shell command
line, that line is regarded as a `pushd` command. Change this
variable when you add aliases for `` `pushd' ``. Likewise,
`shell-popd-regexp` and `shell-cd-regexp` are used to
recognize commands with the meaning of `` `popd' `` and `` `cd' ``.
These commands are recognized only at the beginning of a shell command
line.

If Emacs gets confused about changes in the current directory of the
subshell, use the command `M-x dirs` to ask the shell what its
current directory is. This command works for shells that support the
most common command syntax; it may not work for unusual shells.

You can also use `M-x dirtrack-mode` to enable (or disable) an
alternative and more aggressive method of tracking changes in the
current directory.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC450) ] | [ [>](emacs.html#SEC452) ] | [ [<<](emacs.html#SEC452) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.6 Shell Mode Options

If the variable `comint-scroll-to-bottom-on-input` is
non-`nil`, insertion and yank commands scroll the selected window
to the bottom before inserting.

If `comint-scroll-show-maximum-output` is non-`nil`, then
scrolling due to the arrival of output tries to place the last line of
text at the bottom line of the window, so as to show as much useful
text as possible. (This mimics the scrolling behavior of many
terminals.) The default is `nil`.

By setting `comint-scroll-to-bottom-on-output`, you can opt for
having point jump to the end of the buffer whenever output arrives--no
matter where in the buffer point was before. If the value is
`this`, point jumps in the selected window. If the value is
`all`, point jumps in each window that shows the Comint buffer. If
the value is `other`, point jumps in all nonselected windows that
show the current buffer. The default value is `nil`, which means
point does not jump to the end.

The variable `comint-input-ignoredups` controls whether successive
identical inputs are stored in the input history. A non-`nil`
value means to omit an input that is the same as the previous input.
The default is `nil`, which means to store each input even if it is
equal to the previous input.

Three variables customize file name completion. The variable
`comint-completion-addsuffix` controls whether completion inserts a
space or a slash to indicate a fully completed file or directory name
(non-`nil` means do insert a space or slash).
`comint-completion-recexact`, if non-`nil`, directs `TAB`
to choose the shortest possible completion if the usual Emacs completion
algorithm cannot add even a single character.
`comint-completion-autolist`, if non-`nil`, says to list all
the possible completions whenever completion is not exact.

Command completion normally considers only executable files.
If you set `shell-completion-execonly` to `nil`,
it considers nonexecutable files as well.

You can configure the behavior of `` `pushd' ``. Variables control
whether `` `pushd' `` behaves like `` `cd' `` if no argument is given
(`shell-pushd-tohome`), pop rather than rotate with a numeric
argument (`shell-pushd-dextract`), and only add directories to the
directory stack if they are not already on it
(`shell-pushd-dunique`). The values you choose should match the
underlying shell, of course.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC451) ] | [ [>](emacs.html#SEC453) ] | [ [<<](emacs.html#SEC453) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.7 Emacs Terminal Emulator

To run a subshell in a terminal emulator, putting its typescript in
an Emacs buffer, use `M-x term`. This creates (or reuses) a
buffer named `` `*terminal*' ``, and runs a subshell with input coming
from your keyboard, and output going to that buffer.

The terminal emulator uses Term mode, which has two input modes. In
line mode, Term basically acts like Shell mode; see [AC.15.3 Shell Mode](emacs.html#SEC445).

In char mode, each character is sent directly to the inferior
subshell, as "terminal input." Any "echoing" of your input is the
responsibility of the subshell. The sole exception is the terminal
escape character, which by default is `C-c` (see section [AC.15.8 Term Mode](emacs.html#SEC453)).
Any "terminal output" from the subshell goes into the buffer,
advancing point.

Some programs (such as Emacs itself) need to control the appearance
on the terminal screen in detail. They do this by sending special
control codes. The exact control codes needed vary from terminal to
terminal, but nowadays most terminals and terminal emulators
(including `xterm`) understand the ANSI-standard (VT100-style)
escape sequences. Term mode recognizes these escape sequences, and
handles each one appropriately, changing the buffer so that the
appearance of the window matches what it would be on a real terminal.
You can actually run Emacs inside an Emacs Term window.

The file name used to load the subshell is determined the same way
as for Shell mode. To make multiple terminal emulators, rename the
buffer `` `*terminal*' `` to something different using `M-x
rename-uniquely`, just as with Shell mode.

Unlike Shell mode, Term mode does not track the current directory by
examining your input. But some shells can tell Term what the current
directory is. This is done automatically by `bash` version 1.15
and later.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC452) ] | [ [>](emacs.html#SEC454) ] | [ [<<](emacs.html#SEC454) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.8 Term Mode

The terminal emulator uses Term mode, which has two input modes. In
line mode, Term basically acts like Shell mode; see [AC.15.3 Shell Mode](emacs.html#SEC445).
In char mode, each character is sent directly to the inferior
subshell, except for the Term escape character, normally `C-c`.

To switch between line and char mode, use these commands:

`C-c C-k`: Switch to line mode. Do nothing if already in line mode. `C-c C-j`: Switch to char mode. Do nothing if already in char mode.

The following commands are only available in char mode:

`C-c C-c`: Send a literal `C-c` to the sub-shell. `C-c C-x`: A prefix command to access the global `C-x` commands conveniently. For example, `C-c C-x o` invokes the global binding of `C-x o`, which is normally `` `other-window' ``.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC453) ] | [ [>](emacs.html#SEC455) ] | [ [<<](emacs.html#SEC442) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.9 Page-At-A-Time Output

Term mode has a page-at-a-time feature. When enabled it makes
output pause at the end of each screenful.

`C-c C-q`: Toggle the page-at-a-time feature. This command works in both line and char modes. When page-at-a-time is enabled, the mode-line displays the word `` `page' ``.

With page-at-a-time enabled, whenever Term receives more than a
screenful of output since your last input, it pauses, displaying
`` `**MORE**' `` in the mode-line. Type `SPC` to display the next
screenful of output. Type `?` to see your other options. The
interface is similar to the `more` program.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC454) ] | [ [>](emacs.html#SEC456) ] | [ [<<](emacs.html#SEC444) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.10 Remote Host Shell

You can login to a remote computer, using whatever commands you
would from a regular terminal (e.g. using the `telnet` or
`rlogin` commands), from a Term window.

A program that asks you for a password will normally suppress
echoing of the password, so the password will not show up in the
buffer. This will happen just as if you were using a real terminal,
if the buffer is in char mode. If it is in line mode, the password is
temporarily visible, but will be erased when you hit return. (This
happens automatically; there is no special password processing.)

When you log in to a different machine, you need to specify the type
of terminal you're using. Terminal types `` `ansi' `` or `` `vt100' ``
will work on most systems.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC455) ] | [ [>](emacs.html#SEC457) ] | [ [<<](emacs.html#SEC442) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC458) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.16 Using Emacs as a Server

Various programs such as `mail` can invoke your choice of editor
to edit a particular piece of text, such as a message that you are
sending. By convention, most of these programs use the environment
variable `EDITOR` to specify which editor to run. If you set
`EDITOR` to `` `emacs' ``, they invoke Emacs--but in an
inconvenient fashion, by starting a new, separate Emacs process. This
is inconvenient because it takes time and because the new Emacs process
doesn't share the buffers in any existing Emacs process.

You can arrange to use your existing Emacs process as the editor for
programs like `mail` by using the Emacs client and Emacs server
programs. Here is how.

First, the preparation. Within Emacs, call the function
`server-start`. (Your `.emacs' file can do this automatically
if you add the expression `(server-start)` to it.) Then, outside
Emacs, set the `EDITOR` environment variable to `` `emacsclient' ``.
(Note that some programs use a different environment variable; for
example, to make TeX use `` `emacsclient' ``, you should set the
`TEXEDIT` environment variable to `` `emacsclient +%d %s' ``.)

Then, whenever any program invokes your specified `EDITOR`
program, the effect is to send a message to your principal Emacs telling
it to visit a file. (That's what the program `emacsclient` does.)
Emacs displays the buffer immediately and you can immediately begin
editing it.

When you've finished editing that buffer, type `C-x #`
(`server-edit`). This saves the file and sends a message back to
the `emacsclient` program telling it to exit. The programs that
use `EDITOR` wait for the "editor" (actually, `emacsclient`)
to exit. `C-x #` also checks for other pending external requests
to edit various files, and selects the next such file.

You can switch to a server buffer manually if you wish; you don't
have to arrive at it with `C-x #`. But `C-x #` is the way to
say that you are finished with one.

Finishing with a server buffer also kills the buffer, unless it
already existed in the Emacs session before the server asked to create
it. However, if you set `server-kill-new-buffers` to `nil`,
then a different criterion is used: finishing with a server buffer
kills it if the file name matches the regular expression
`server-temp-file-regexp`. This is set up to distinguish certain
"temporary" files.

If you set the variable `server-window` to a window or a frame,
`C-x #` displays the server buffer in that window or in that frame.

While `mail` or another application is waiting for
`emacsclient` to finish, `emacsclient` does not read terminal
input. So the terminal that `mail` was using is effectively
blocked for the duration. In order to edit with your principal Emacs,
you need to be able to use it without using that terminal. There are
three ways to do this:

* Using a window system, run `mail` and the principal Emacs in two
  separate windows. While `mail` is waiting for `emacsclient`,
  the window where it was running is blocked, but you can use Emacs by
  switching windows.

  * Using virtual terminals, run `mail` in one virtual terminal
    and run Emacs in another.

    * Use Shell mode or Term mode in Emacs to run the other program such as
      `mail`; then, `emacsclient` blocks only the subshell under
      Emacs, and you can still use Emacs to edit the file.

If you run `emacsclient` with the option `` `--no-wait' ``, it
returns immediately without waiting for you to "finish" the buffer
in Emacs. Note that server buffers created in this way are not killed
automatically when you finish with them.

> |  |  |  |
> | --- | --- | --- |
> | [AC.17 Invoking `emacsclient`](emacs.html#SEC457) |  |  |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC456) ] | [ [>](emacs.html#SEC458) ] | [ [<<](emacs.html#SEC456) ] | [ [Up](emacs.html#SEC456) ] | [ [>>](emacs.html#SEC458) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.17 Invoking `emacsclient`

To run the `emacsclient` program, specify file names as arguments,
and optionally line numbers as well. Do it like this:

|  |  |
| --- | --- |
|  | ``` emacsclient {[+line[column]] filename}... ``` |

This tells Emacs to visit each of the specified files; if you specify a
line number for a certain file, Emacs moves to that line in the file.
If you specify a column number as well, Emacs puts point on that column
in the line.

Ordinarily, `emacsclient` does not return until you use the
`C-x #` command on each of these buffers. When that happens,
Emacs sends a message to the `emacsclient` program telling it to
return.

But if you use the option `` `-n' `` or `` `--no-wait' `` when running
`emacsclient`, then it returns immediately. (You can take as
long as you like to edit the files in Emacs.)

The option `` `--alternate-editor=command' `` is useful when
running `emacsclient` in a script. It specifies a command to run
if `emacsclient` fails to contact Emacs. For example, the
following setting for the EDITOR environment variable will
always give an editor, even if Emacs is not running:

|  |  |
| --- | --- |
|  | ``` EDITOR="emacsclient --alternate-editor vi +%d %s" ``` |

The environment variable ALTERNATE\_EDITOR has the same effect, but
the value of the `` `--alternate-editor' `` takes precedence.

Alternatively, the file `etc/emacs.bash' defines a bash
function which will communicate with a running Emacs server, or start
one if none exists.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC457) ] | [ [>](emacs.html#SEC459) ] | [ [<<](emacs.html#SEC456) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC459) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.18 Hardcopy Output

The Emacs commands for making hardcopy let you print either an entire
buffer or just part of one, either with or without page headers.
See also the hardcopy commands of Dired (see section [M.10 Miscellaneous File Operations](emacs.html#SEC165))
and the diary (see section [AC.10.1 Commands Displaying Diary Entries](emacs.html#SEC430)).

`M-x print-buffer`: Print hardcopy of current buffer with page headings containing the file name and page number. `M-x lpr-buffer`: Print hardcopy of current buffer without page headings. `M-x print-region`: Like `print-buffer` but print only the current region. `M-x lpr-region`: Like `lpr-buffer` but print only the current region.

The hardcopy commands (aside from the Postscript commands) pass extra
switches to the `lpr` program based on the value of the variable
`lpr-switches`. Its value should be a list of strings, each string
an option starting with `` `-' ``. For example, to specify a line width
of 80 columns for all the printing you do in Emacs, set
`lpr-switches` like this:

|  |  |
| --- | --- |
|  | ``` (setq lpr-switches '("-w80")) ``` |

You can specify the printer to use by setting the variable
`printer-name`.

The variable `lpr-command` specifies the name of the printer
program to run; the default value depends on your operating system type.
On most systems, the default is `"lpr"`. The variable
`lpr-headers-switches` similarly specifies the extra switches to
use to make page headers. The variable `lpr-add-switches` controls
whether to supply `` `-T' `` and `` `-J' `` options (suitable for
`lpr`) to the printer program: `nil` means don't add them.
`lpr-add-switches` should be `nil` if your printer program is
not compatible with `lpr`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC458) ] | [ [>](emacs.html#SEC460) ] | [ [<<](emacs.html#SEC458) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC460) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.19 PostScript Hardcopy

These commands convert buffer contents to PostScript,
either printing it or leaving it in another Emacs buffer.

`M-x ps-print-buffer`: Print hardcopy of the current buffer in PostScript form. `M-x ps-print-region`: Print hardcopy of the current region in PostScript form. `M-x ps-print-buffer-with-faces`: Print hardcopy of the current buffer in PostScript form, showing the faces used in the text by means of PostScript features. `M-x ps-print-region-with-faces`: Print hardcopy of the current region in PostScript form, showing the faces used in the text. `M-x ps-spool-buffer`: Generate PostScript for the current buffer text. `M-x ps-spool-region`: Generate PostScript for the current region. `M-x ps-spool-buffer-with-faces`: Generate PostScript for the current buffer, showing the faces used. `M-x ps-spool-region-with-faces`: Generate PostScript for the current region, showing the faces used. `M-x handwrite`: Generates/prints PostScript for the current buffer as if handwritten.

The PostScript commands, `ps-print-buffer` and
`ps-print-region`, print buffer contents in PostScript form. One
command prints the entire buffer; the other, just the region. The
corresponding `` `-with-faces' `` commands,
`ps-print-buffer-with-faces` and `ps-print-region-with-faces`,
use PostScript features to show the faces (fonts and colors) in the text
properties of the text being printed.

If you are using a color display, you can print a buffer of program
code with color highlighting by turning on Font-Lock mode in that
buffer, and using `ps-print-buffer-with-faces`.

The commands whose names have `` `spool' `` instead of `` `print' ``
generate the PostScript output in an Emacs buffer instead of sending
it to the printer.

`M-x handwrite` is more frivolous. It generates a PostScript
rendition of the current buffer as a cursive handwritten document. It
can be customized in group `handwrite`. This function only
supports ISO 8859-1 characters.

The following section describes variables for customizing these commands.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC459) ] | [ [>](emacs.html#SEC461) ] | [ [<<](emacs.html#SEC459) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC461) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.20 Variables for PostScript Hardcopy

All the PostScript hardcopy commands use the variables
`ps-lpr-command` and `ps-lpr-switches` to specify how to print
the output. `ps-lpr-command` specifies the command name to run,
`ps-lpr-switches` specifies command line options to use, and
`ps-printer-name` specifies the printer. If you don't set the
first two variables yourself, they take their initial values from
`lpr-command` and `lpr-switches`. If `ps-printer-name`
is `nil`, `printer-name` is used.

The variable `ps-print-header` controls whether these commands
add header lines to each page--set it to `nil` to turn headers
off.

If your printer doesn't support colors, you should turn off color
processing by setting `ps-print-color-p` to `nil`. By
default, if the display supports colors, Emacs produces hardcopy output
with color information; on black-and-white printers, colors are emulated
with shades of gray. This might produce illegible output, even if your
screen colors only use shades of gray.

By default, PostScript printing ignores the background colors of the
faces, unless the variable `ps-use-face-background` is
non-`nil`. This is to avoid unwanted interference with the zebra
stripes and background image/text.

The variable `ps-paper-type` specifies which size of paper to
format for; legitimate values include `a4`, `a3`,
`a4small`, `b4`, `b5`, `executive`, `ledger`,
`legal`, `letter`, `letter-small`, `statement`,
`tabloid`. The default is `letter`. You can define
additional paper sizes by changing the variable
`ps-page-dimensions-database`.

The variable `ps-landscape-mode` specifies the orientation of
printing on the page. The default is `nil`, which stands for
"portrait" mode. Any non-`nil` value specifies "landscape"
mode.

The variable `ps-number-of-columns` specifies the number of
columns; it takes effect in both landscape and portrait mode. The
default is 1.

The variable `ps-font-family` specifies which font family to use
for printing ordinary text. Legitimate values include `Courier`,
`Helvetica`, `NewCenturySchlbk`, `Palatino` and
`Times`. The variable `ps-font-size` specifies the size of
the font for ordinary text. It defaults to 8.5 points.

Emacs supports more scripts and characters than a typical PostScript
printer. Thus, some of the characters in your buffer might not be
printable using the fonts built into your printer. You can augment
the fonts supplied with the printer with those from the GNU Intlfonts
package, or you can instruct Emacs to use Intlfonts exclusively. The
variable `ps-multibyte-buffer` controls this: the default value,
`nil`, is appropriate for printing ASCII and Latin-1
characters; a value of `non-latin-printer` is for printers which
have the fonts for ASCII, Latin-1, Japanese, and Korean
characters built into them. A value of `bdf-font` arranges for
the BDF fonts from the Intlfonts package to be used for *all*
characters. Finally, a value of `bdf-font-except-latin`
instructs the printer to use built-in fonts for ASCII and Latin-1
characters, and Intlfonts BDF fonts for the rest.

To be able to use the BDF fonts, Emacs needs to know where to find
them. The variable `bdf-directory-list` holds the list of
directories where Emacs should look for the fonts; the default value
includes a single directory `/usr/local/share/emacs/fonts/bdf'.

Many other customization variables for these commands are defined and
described in the Lisp files `ps-print.el' and `ps-mule.el'.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC460) ] | [ [>](emacs.html#SEC462) ] | [ [<<](emacs.html#SEC460) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC462) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.21 Sorting Text

Emacs provides several commands for sorting text in the buffer. All
operate on the contents of the region (the text between point and the
mark). They divide the text of the region into many *sort records*,
identify a *sort key* for each record, and then reorder the records
into the order determined by the sort keys. The records are ordered so
that their keys are in alphabetical order, or, for numeric sorting, in
numeric order. In alphabetic sorting, all upper-case letters `A' through
`Z' come before lower-case `a', in accord with the ASCII character
sequence.

The various sort commands differ in how they divide the text into sort
records and in which part of each record is used as the sort key. Most of
the commands make each line a separate sort record, but some commands use
paragraphs or pages as sort records. Most of the sort commands use each
entire sort record as its own sort key, but some use only a portion of the
record as the sort key.

`M-x sort-lines`: Divide the region into lines, and sort by comparing the entire text of a line. A numeric argument means sort into descending order. `M-x sort-paragraphs`: Divide the region into paragraphs, and sort by comparing the entire text of a paragraph (except for leading blank lines). A numeric argument means sort into descending order. `M-x sort-pages`: Divide the region into pages, and sort by comparing the entire text of a page (except for leading blank lines). A numeric argument means sort into descending order. `M-x sort-fields`: Divide the region into lines, and sort by comparing the contents of one field in each line. Fields are defined as separated by whitespace, so the first run of consecutive non-whitespace characters in a line constitutes field 1, the second such run constitutes field 2, etc. Specify which field to sort by with a numeric argument: 1 to sort by field 1, etc. A negative argument means count fields from the right instead of from the left; thus, minus 1 means sort by the last field. If several lines have identical contents in the field being sorted, they keep the same relative order that they had in the original buffer. `M-x sort-numeric-fields`: Like `M-x sort-fields` except the specified field is converted to an integer for each line, and the numbers are compared. `` `10' `` comes before `` `2' `` when considered as text, but after it when considered as a number. By default, numbers are interpreted according to `sort-numeric-base`, but numbers beginning with `` `0x' `` or `` `0' `` are interpreted as hexadecimal and octal, respectively. `M-x sort-columns`: Like `M-x sort-fields` except that the text within each line used for comparison comes from a fixed range of columns. See below for an explanation. `M-x reverse-region`: Reverse the order of the lines in the region. This is useful for sorting into descending order by fields or columns, since those sort commands do not have a feature for doing that.

For example, if the buffer contains this:

|  |  |
| --- | --- |
|  | ``` On systems where clash detection (locking of files being edited) is implemented, Emacs also checks the first time you modify a buffer whether the file has changed on disk since it was last visited or saved.  If it has, you are asked to confirm that you want to change the buffer. ``` |

applying `M-x sort-lines` to the entire buffer produces this:

|  |  |
| --- | --- |
|  | ``` On systems where clash detection (locking of files being edited) is implemented, Emacs also checks the first time you modify a buffer saved.  If it has, you are asked to confirm that you want to change the buffer. whether the file has changed on disk since it was last visited or ``` |

where the upper-case `` `O' `` sorts before all lower-case letters. If
you use `C-u 2 M-x sort-fields` instead, you get this:

|  |  |
| --- | --- |
|  | ``` implemented, Emacs also checks the first time you modify a buffer saved.  If it has, you are asked to confirm that you want to change the buffer. On systems where clash detection (locking of files being edited) is whether the file has changed on disk since it was last visited or ``` |

where the sort keys were `` `Emacs' ``, `` `If' ``, `` `buffer' ``,
`` `systems' `` and `` `the' ``.

`M-x sort-columns` requires more explanation. You specify the
columns by putting point at one of the columns and the mark at the other
column. Because this means you cannot put point or the mark at the
beginning of the first line of the text you want to sort, this command
uses an unusual definition of "region": all of the line point is in is
considered part of the region, and so is all of the line the mark is in,
as well as all the lines in between.

For example, to sort a table by information found in columns 10 to 15,
you could put the mark on column 10 in the first line of the table, and
point on column 15 in the last line of the table, and then run
`sort-columns`. Equivalently, you could run it with the mark on
column 15 in the first line and point on column 10 in the last line.

This can be thought of as sorting the rectangle specified by point and
the mark, except that the text on each line to the left or right of the
rectangle moves along with the text inside the rectangle.
See section [H.10 Rectangles](emacs.html#SEC68).

Many of the sort commands ignore case differences when comparing, if
`sort-fold-case` is non-`nil`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC461) ] | [ [>](emacs.html#SEC463) ] | [ [<<](emacs.html#SEC461) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC463) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.22 Narrowing

*Narrowing* means focusing in on some portion of the buffer,
making the rest temporarily inaccessible. The portion which you can
still get to is called the *accessible portion*. Canceling the
narrowing, which makes the entire buffer once again accessible, is
called *widening*. The amount of narrowing in effect in a buffer at
any time is called the buffer's *restriction*.

Narrowing can make it easier to concentrate on a single subroutine or
paragraph by eliminating clutter. It can also be used to restrict the
range of operation of a replace command or repeating keyboard macro.

`C-x n n`: Narrow down to between point and mark (`narrow-to-region`). `C-x n w`: Widen to make the entire buffer accessible again (`widen`). `C-x n p`: Narrow down to the current page (`narrow-to-page`). `C-x n d`: Narrow down to the current defun (`narrow-to-defun`).

When you have narrowed down to a part of the buffer, that part appears
to be all there is. You can't see the rest, you can't move into it
(motion commands won't go outside the accessible part), you can't change
it in any way. However, it is not gone, and if you save the file all
the inaccessible text will be saved. The word `` `Narrow' `` appears in
the mode line whenever narrowing is in effect.

The primary narrowing command is `C-x n n` (`narrow-to-region`).
It sets the current buffer's restrictions so that the text in the current
region remains accessible, but all text before the region or after the
region is inaccessible. Point and mark do not change.

Alternatively, use `C-x n p` (`narrow-to-page`) to narrow
down to the current page. See section [T.4 Pages](emacs.html#SEC237), for the definition of a page.
`C-x n d` (`narrow-to-defun`) narrows down to the defun
containing point (see section [U.2 Top-Level Definitions, or Defuns](emacs.html#SEC270)).

The way to cancel narrowing is to widen with `C-x n w`
(`widen`). This makes all text in the buffer accessible again.

You can get information on what part of the buffer you are narrowed down
to using the `C-x =` command. See section [D.9 Cursor Position Information](emacs.html#SEC28).

Because narrowing can easily confuse users who do not understand it,
`narrow-to-region` is normally a disabled command. Attempting to use
this command asks for confirmation and gives you the option of enabling it;
if you enable the command, confirmation will no longer be required for
it. See section [AD.4.11 Disabling Commands](emacs.html#SEC502).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC462) ] | [ [>](emacs.html#SEC464) ] | [ [<<](emacs.html#SEC462) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC464) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.23 Two-Column Editing

Two-column mode lets you conveniently edit two side-by-side columns of
text. It uses two side-by-side windows, each showing its own
buffer.

There are three ways to enter two-column mode:

`F2 2` or `C-x 6 2`: Enter two-column mode with the current buffer on the left, and on the right, a buffer whose name is based on the current buffer's name (`2C-two-columns`). If the right-hand buffer doesn't already exist, it starts out empty; the current buffer's contents are not changed. This command is appropriate when the current buffer is empty or contains just one column and you want to add another column. `F2 s` or `C-x 6 s`: Split the current buffer, which contains two-column text, into two buffers, and display them side by side (`2C-split`). The current buffer becomes the left-hand buffer, but the text in the right-hand column is moved into the right-hand buffer. The current column specifies the split point. Splitting starts with the current line and continues to the end of the buffer. This command is appropriate when you have a buffer that already contains two-column text, and you wish to separate the columns temporarily. `F2 b buffer RET`: `C-x 6 b buffer RET`: Enter two-column mode using the current buffer as the left-hand buffer, and using buffer buffer as the right-hand buffer (`2C-associate-buffer`).

`F2 s` or `C-x 6 s` looks for a column separator, which
is a string that appears on each line between the two columns. You can
specify the width of the separator with a numeric argument to
`F2 s`; that many characters, before point, constitute the
separator string. By default, the width is 1, so the column separator
is the character before point.

When a line has the separator at the proper place, `F2 s`
puts the text after the separator into the right-hand buffer, and
deletes the separator. Lines that don't have the column separator at
the proper place remain unsplit; they stay in the left-hand buffer, and
the right-hand buffer gets an empty line to correspond. (This is the
way to write a line that "spans both columns while in two-column
mode": write it in the left-hand buffer, and put an empty line in the
right-hand buffer.)

The command `C-x 6 RET` or `F2 RET`
(`2C-newline`) inserts a newline in each of the two buffers at
corresponding positions. This is the easiest way to add a new line to
the two-column text while editing it in split buffers.

When you have edited both buffers as you wish, merge them with
`F2 1` or `C-x 6 1` (`2C-merge`). This copies the
text from the right-hand buffer as a second column in the other buffer.
To go back to two-column editing, use `F2 s`.

Use `F2 d` or `C-x 6 d` to dissociate the two buffers,
leaving each as it stands (`2C-dissociate`). If the other buffer,
the one not current when you type `F2 d`, is empty,
`F2 d` kills it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC463) ] | [ [>](emacs.html#SEC465) ] | [ [<<](emacs.html#SEC463) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC465) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.24 Editing Binary Files

There is a special major mode for editing binary files: Hexl mode. To
use it, use `M-x hexl-find-file` instead of `C-x C-f` to visit
the file. This command converts the file's contents to hexadecimal and
lets you edit the translation. When you save the file, it is converted
automatically back to binary.

You can also use `M-x hexl-mode` to translate an existing buffer
into hex. This is useful if you visit a file normally and then discover
it is a binary file.

Ordinary text characters overwrite in Hexl mode. This is to reduce
the risk of accidentally spoiling the alignment of data in the file.
There are special commands for insertion. Here is a list of the
commands of Hexl mode:

`C-M-d`: Insert a byte with a code typed in decimal. `C-M-o`: Insert a byte with a code typed in octal. `C-M-x`: Insert a byte with a code typed in hex. `C-x [`: Move to the beginning of a 1k-byte "page." `C-x ]`: Move to the end of a 1k-byte "page." `M-g`: Move to an address specified in hex. `M-j`: Move to an address specified in decimal. `C-c C-c`: Leave Hexl mode, going back to the major mode this buffer had before you invoked `hexl-mode`.

Other Hexl commands let you insert strings (sequences) of binary
bytes, move by `short`s or `int`s, etc.; type `C-h a
hexl-RET` for details.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC464) ] | [ [>](emacs.html#SEC466) ] | [ [<<](emacs.html#SEC464) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC466) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.25 Saving Emacs Sessions

You can use the Desktop library to save the state of Emacs from one
session to another. Saving the state means that Emacs starts up with
the same set of buffers, major modes, buffer positions, and so on that
the previous Emacs session had.

To use Desktop, you should use the Customization buffer (see section [AD.2.2 Easy Customization Interface](emacs.html#SEC479)) to set `desktop-enable` to a non-`nil` value,
or add these lines at the end of your `.emacs' file:

|  |  |
| --- | --- |
|  | ``` (desktop-load-default) (desktop-read) ``` |

The first time you save the state of the Emacs session, you must do it
manually, with the command `M-x desktop-save`. Once you have done
that, exiting Emacs will save the state again--not only the present
Emacs session, but also subsequent sessions. You can also save the
state at any time, without exiting Emacs, by typing `M-x
desktop-save` again.

In order for Emacs to recover the state from a previous session, you
must start it with the same current directory as you used when you
started the previous session. This is because `desktop-read` looks
in the current directory for the file to read. This means that you can
have separate saved sessions in different directories; the directory in
which you start Emacs will control which saved session to use.

The variable `desktop-files-not-to-save` controls which files are
excluded from state saving. Its value is a regular expression that
matches the files to exclude. By default, remote (ftp-accessed) files
are excluded; this is because visiting them again in the subsequent
session would be slow. If you want to include these files in state
saving, set `desktop-files-not-to-save` to `"^$"`.
See section [M.13 Remote Files](emacs.html#SEC168).

The Saveplace library provides a simpler feature that records your
position in each file when you kill its buffer (or kill Emacs), and
jumps to the same position when you visit the file again (even in
another Emacs session). Use `M-x toggle-save-place` to turn on
place-saving in a given file. Customize the option `save-place`
to turn it on for all files in each session.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC465) ] | [ [>](emacs.html#SEC467) ] | [ [<<](emacs.html#SEC465) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC467) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.26 Recursive Editing Levels

A *recursive edit* is a situation in which you are using Emacs
commands to perform arbitrary editing while in the middle of another
Emacs command. For example, when you type `C-r` inside of a
`query-replace`, you enter a recursive edit in which you can change
the current buffer. On exiting from the recursive edit, you go back to
the `query-replace`.

*Exiting* the recursive edit means returning to the unfinished
command, which continues execution. The command to exit is `C-M-c`
(`exit-recursive-edit`).

You can also *abort* the recursive edit. This is like exiting,
but also quits the unfinished command immediately. Use the command
`C-]` (`abort-recursive-edit`) to do this. See section [AD.8 Quitting and Aborting](emacs.html#SEC510).

The mode line shows you when you are in a recursive edit by displaying
square brackets around the parentheses that always surround the major and
minor mode names. Every window's mode line shows this in the same way,
since being in a recursive edit is true of Emacs as a whole rather than
any particular window or buffer.

It is possible to be in recursive edits within recursive edits. For
example, after typing `C-r` in a `query-replace`, you may type a
command that enters the debugger. This begins a recursive editing level
for the debugger, within the recursive editing level for `C-r`.
Mode lines display a pair of square brackets for each recursive editing
level currently in progress.

Exiting the inner recursive edit (such as, with the debugger `c`
command) resumes the command running in the next level up. When that
command finishes, you can then use `C-M-c` to exit another recursive
editing level, and so on. Exiting applies to the innermost level only.
Aborting also gets out of only one level of recursive edit; it returns
immediately to the command level of the previous recursive edit. If you
wish, you can then abort the next recursive editing level.

Alternatively, the command `M-x top-level` aborts all levels of
recursive edits, returning immediately to the top-level command reader.

The text being edited inside the recursive edit need not be the same text
that you were editing at top level. It depends on what the recursive edit
is for. If the command that invokes the recursive edit selects a different
buffer first, that is the buffer you will edit recursively. In any case,
you can switch buffers within the recursive edit in the normal manner (as
long as the buffer-switching keys have not been rebound). You could
probably do all the rest of your editing inside the recursive edit,
visiting files and all. But this could have surprising effects (such as
stack overflow) from time to time. So remember to exit or abort the
recursive edit when you no longer need it.

In general, we try to minimize the use of recursive editing levels in
GNU Emacs. This is because they constrain you to "go back" in a
particular order--from the innermost level toward the top level. When
possible, we present different activities in separate buffers so that
you can switch between them as you please. Some commands switch to a
new major mode which provides a command to switch back. These
approaches give you more flexibility to go back to unfinished tasks in
the order you choose.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC466) ] | [ [>](emacs.html#SEC468) ] | [ [<<](emacs.html#SEC466) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC468) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.27 Emulation

GNU Emacs can be programmed to emulate (more or less) most other
editors. Standard facilities can emulate these:

CRiSP/Brief (PC editor): You can turn on key bindings to emulate the CRiSP/Brief editor with `M-x crisp-mode`. Note that this rebinds `M-x` to exit Emacs unless you change the user option `crisp-override-meta-x`. You can also use the command `M-x scroll-all-mode` or set the user option `crisp-load-scroll-all` to emulate CRiSP's scroll-all feature (scrolling all windows together). EDT (DEC VMS editor): Turn on EDT emulation with `M-x edt-emulation-on`. `M-x edt-emulation-off` restores normal Emacs command bindings. Most of the EDT emulation commands are keypad keys, and most standard Emacs key bindings are still available. The EDT emulation rebindings are done in the global keymap, so there is no problem switching buffers or major modes while in EDT emulation. "PC" bindings: The command `M-x pc-bindings-mode` sets up certain key bindings for "PC compatibility"---what people are often used to on PCs--as follows: `Delete` and its variants delete forward instead of backward, `C-Backspace` kills backward a word (as `C-Delete` normally would), `M-Backspace` does undo, `Home` and `End` move to beginning and end of line, `C-Home` and `C-End` move to beginning and end of buffer and `C-Escape` does `list-buffers`. PC Selection mode: The command `M-x pc-selection-mode` enables a global minor mode that emulates the mark, copy, cut and paste commands of various other systems--an interface known as CUA. It establishes the key bindings of PC mode, and also modifies the bindings of the cursor keys and the `next`, `prior`, `home` and `end` keys. It does not provide the full set of CUA key bindings--the fundamental Emacs keys `C-c`, `C-v` and `C-x` are not changed. The standard keys for moving around (`right`, `left`, `up`, `down`, `home`, `end`, `prior`, `next`, called "move-keys") deactivate the mark in PC selection mode. However, using `Shift` together with the "move keys" activates the region over which they move. The copy, cut and paste functions are available on `C-insert`, `S-delete` and `S-insert` respectively. The `s-region` package provides similar, but less complete, facilities. TPU (DEC VMS editor): `M-x tpu-edt-on` turns on emulation of the TPU editor emulating EDT. vi (Berkeley editor): Viper is the newest emulator for vi. It implements several levels of emulation; level 1 is closest to vi itself, while level 5 departs somewhat from strict emulation to take advantage of the capabilities of Emacs. To invoke Viper, type `M-x viper-mode`; it will guide you the rest of the way and ask for the emulation level. See Info file `viper', node `Top'. vi (another emulator): `M-x vi-mode` enters a major mode that replaces the previously established major mode. All of the vi commands that, in real vi, enter "input" mode are programmed instead to return to the previous major mode. Thus, ordinary Emacs serves as vi's "input" mode. Because vi emulation works through major modes, it does not work to switch buffers during emulation. Return to normal Emacs first. If you plan to use vi emulation much, you probably want to bind a key to the `vi-mode` command. vi (alternate emulator): `M-x vip-mode` invokes another vi emulator, said to resemble real vi more thoroughly than `M-x vi-mode`. "Input" mode in this emulator is changed from ordinary Emacs so you can use `ESC` to go back to emulated vi command mode. To get from emulated vi command mode back to ordinary Emacs, type `C-z`. This emulation does not work through major modes, and it is possible to switch buffers in various ways within the emulator. It is not so necessary to assign a key to the command `vip-mode` as it is with `vi-mode` because terminating insert mode does not use it. See Info file `vip', node `Top', for full information. WordStar (old wordprocessor): `M-x wordstar-mode` provides a major mode with WordStar-like key bindings.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC467) ] | [ [>](emacs.html#SEC469) ] | [ [<<](emacs.html#SEC467) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC473) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.28 Hyperlinking and Navigation Features

Various modes documented elsewhere have hypertext features so that
you can follow links, usually by clicking `Mouse-2` on the link or
typing `RET` while point is on the link. Info mode, Help mode and
the Dired-like modes are examples. The Tags facility links between
uses and definitions in source files, see [W.2 Tags Tables](emacs.html#SEC334). Imenu provides
navigation amongst items indexed in the current buffer, see
[U.2.3 Imenu](emacs.html#SEC273). Info-lookup provides mode-specific lookup of definitions
in Info indexes, see [U.6 Documentation Lookup](emacs.html#SEC289). Speedbar maintains a frame
in which links to files, and locations in files are displayed, see
[P.9 Making and Using a Speedbar Frame](emacs.html#SEC200).

Other non-mode-specific facilities described in this section enable
following links from the current buffer in a context-sensitive
fashion.

> |  |  |  |
> | --- | --- | --- |
> | [AC.28.1 Following URLs](emacs.html#SEC469) |  | Following URLs. |
> | [AC.28.2 Activating URLs](emacs.html#SEC470) |  | Activating URLs. |
> | [AC.28.3 Finding Files and URLs at Point](emacs.html#SEC471) |  | Finding files etc. at point. |
> | [AC.28.4 Finding Function and Variable Definitions](emacs.html#SEC472) |  | Finding function and variable definitions. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC468) ] | [ [>](emacs.html#SEC470) ] | [ [<<](emacs.html#SEC468) ] | [ [Up](emacs.html#SEC468) ] | [ [>>](emacs.html#SEC473) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.28.1 Following URLs

`M-x browse-url RET url RET`: Load a URL into a Web browser.

The Browse-URL package provides facilities for following URLs specifying
links on the World Wide Web. Usually this works by invoking a web
browser, but you can, for instance, arrange to invoke `compose-mail`
from `` `mailto:' `` URLs.

The general way to use this feature is to type `M-x browse-url`,
which displays a specified URL. If point is located near a plausible
URL, that URL is used as the default. Other commands are available
which you might like to bind to keys, such as
`browse-url-at-point` and `browse-url-at-mouse`.

You can customize Browse-URL's behavior via various options in the
`browse-url` Customize group, particularly
`browse-url-browser-function`. You can invoke actions dependent
on the type of URL by defining `browse-url-browser-function` as
an association list. The package's commentary available via `C-h
p` provides more information. Packages with facilities for following
URLs should always go through Browse-URL, so that the customization
options for Browse-URL will affect all browsing in Emacs.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC469) ] | [ [>](emacs.html#SEC471) ] | [ [<<](emacs.html#SEC471) ] | [ [Up](emacs.html#SEC468) ] | [ [>>](emacs.html#SEC473) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.28.2 Activating URLs

`M-x goto-address`: Activate URLs and e-mail addresses in the current buffer.

You can make URLs in the current buffer active with `M-x
goto-address`. This finds all the URLs in the buffer, and establishes
bindings for `Mouse-2` and `C-c RET` on them. After
activation, if you click on a URL with `Mouse-2`, or move to a URL
and type `C-c RET`, that will display the web page that the URL
specifies. For a `` `mailto' `` URL, it sends mail instead, using your
selected mail-composition method (see section [Z.6 Mail-Composition Methods](emacs.html#SEC373)).

It can be useful to add `goto-address` to mode hooks and the
hooks used to display an incoming message.
`rmail-show-message-hook` is the appropriate hook for Rmail, and
`mh-show-mode-hook` for MH-E. This is not needed for Gnus,
which has a similar feature of its own.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC470) ] | [ [>](emacs.html#SEC472) ] | [ [<<](emacs.html#SEC472) ] | [ [Up](emacs.html#SEC468) ] | [ [>>](emacs.html#SEC473) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.28.3 Finding Files and URLs at Point

FFAP mode replaces certain key bindings for finding files, including
`C-x C-f`, with commands that provide more sensitive defaults.
These commands behave like the ordinary ones when given a prefix
argument. Otherwise, they get the default file name or URL from the
text around point. If what is found in the buffer has the form of a
URL rather than a file name, the commands use `browse-url` to
view it.

This feature is useful for following references in mail or news
buffers, `README' files, `MANIFEST' files, and so on. The
`` `ffap' `` package's commentary available via `C-h p` and the
`ffap` Custom group provide details.

You can turn on FFAP minor mode to make the following key bindings
and to install hooks for using `ffap` in Rmail, Gnus and VM
article buffers.

`C-x C-f filename RET`: Find filename, guessing a default from text around point (`find-file-at-point`). `C-x 4 f`: `ffap-other-window`, analogous to `find-file-other-window`. `C-x 5 f`: `ffap-other-frame`, analogous to `find-file-other-frame`. `M-x ffap-next`: Search buffer for next file name or URL, then find that file or URL. `C-x d directory RET`: Start Dired on directory, defaulting to the directory name at point (`ffap-dired-at-point`). `S-Mouse-3`: `ffap-at-mouse` finds the file guessed from text around the position of a mouse click. `C-S-Mouse-3`: Display a menu of files and URLs mentioned in current buffer, then find the one you select (`ffap-menu`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC471) ] | [ [>](emacs.html#SEC473) ] | [ [<<](emacs.html#SEC468) ] | [ [Up](emacs.html#SEC468) ] | [ [>>](emacs.html#SEC473) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
