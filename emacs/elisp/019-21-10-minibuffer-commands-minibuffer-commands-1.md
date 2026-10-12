### 21.10 Minibuffer Commands [¶](#Minibuffer-Commands-1)

This section describes some commands meant for use in the
minibuffer.

Command: **exit-minibuffer** [¶](#index-exit_002dminibuffer)
:   This command exits the active minibuffer. It is normally bound to
    keys in minibuffer local keymaps. The command throws an error if the
    current buffer is a minibuffer, but not the active minibuffer.

Command: **self-insert-and-exit** [¶](#index-self_002dinsert_002dand_002dexit)
:   This command exits the active minibuffer after inserting the last
    character typed on the keyboard (found in `last-command-event`;
    see [Information from the Command Loop](#Command-Loop-Info)).

Command: **previous-history-element** n [¶](#index-previous_002dhistory_002delement)
:   This command replaces the minibuffer contents with the value of the
    nth previous (older) history element.

Command: **next-history-element** n [¶](#index-next_002dhistory_002delement)
:   This command replaces the minibuffer contents with the value of the
    nth more recent history element. The position in the history
    can go beyond the current position and invoke “future history”
    (see [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer)).

Command: **previous-matching-history-element** pattern n [¶](#index-previous_002dmatching_002dhistory_002delement)
:   This command replaces the minibuffer contents with the value of the
    nth previous (older) history element that matches pattern (a
    regular expression).

Command: **next-matching-history-element** pattern n [¶](#index-next_002dmatching_002dhistory_002delement)
:   This command replaces the minibuffer contents with the value of the
    nth next (newer) history element that matches pattern (a
    regular expression).

Command: **previous-complete-history-element** n [¶](#index-previous_002dcomplete_002dhistory_002delement)
:   This command replaces the minibuffer contents with the value of the
    nth previous (older) history element that completes the current
    contents of the minibuffer before the point.

Command: **next-complete-history-element** n [¶](#index-next_002dcomplete_002dhistory_002delement)
:   This command replaces the minibuffer contents with the value of the
    nth next (newer) history element that completes the current
    contents of the minibuffer before the point.

Command: **goto-history-element** nabs [¶](#index-goto_002dhistory_002delement)
:   This function puts element of the minibuffer history in the
    minibuffer. The argument nabs specifies the absolute history
    position in descending order, where 0 means the current element and a
    positive number n means the nth previous element. NABS
    being a negative number -n means the nth entry of “future
    history”. When this function reaches the end of the default values
    provided by `read-from-minibuffer` (see [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer))
    and `completing-read` (see [Completion and the Minibuffer](#Minibuffer-Completion)), it adds
    the completion candidates to “future history”, see [minibuffer-default-add-function](#Minibuffer-History).

---

Next: [Minibuffer Contents](#Minibuffer-Contents), Previous: [Minibuffer Commands](#Minibuffer-Commands), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.11 Minibuffer Windows [¶](#Minibuffer-Windows-1)

These functions access and select minibuffer windows, test whether they
are active and control how they get resized.

Function: **minibuffer-window** &optional frame [¶](#index-minibuffer_002dwindow)
:   This function returns the minibuffer window used for frame frame.
    If frame is `nil`, that stands for the selected frame.

    Note that the minibuffer window used by a frame need not be part of that
    frame—a frame that has no minibuffer of its own necessarily uses some
    other frame’s minibuffer window. The minibuffer window of a
    minibuffer-less frame can be changed by setting that frame’s
    `minibuffer` frame parameter (see [Buffer Parameters](#Buffer-Parameters)).

Function: **set-minibuffer-window** window [¶](#index-set_002dminibuffer_002dwindow)
:   This function specifies window as the minibuffer window to use.
    This affects where the minibuffer is displayed if you put text in it
    without invoking the usual minibuffer commands. It has no effect on the
    usual minibuffer input functions because they all start by choosing the
    minibuffer window according to the selected frame.

Function: **window-minibuffer-p** &optional window [¶](#index-window_002dminibuffer_002dp)
:   This function returns `t` if window is a minibuffer window.
    window defaults to the selected window.

The following function returns the window showing the currently active
minibuffer.

Function: **active-minibuffer-window** [¶](#index-active_002dminibuffer_002dwindow)
:   This function returns the window of the currently active minibuffer, or
    `nil` if there is no active minibuffer.

It is not sufficient to determine whether a given window shows the
currently active minibuffer by comparing it with the result of
`(minibuffer-window)`, because there can be more than one
minibuffer window if there is more than one frame.

Function: **minibuffer-window-active-p** window [¶](#index-minibuffer_002dwindow_002dactive_002dp)
:   This function returns non-`nil` if window shows the currently
    active minibuffer.

The following two options control whether minibuffer windows are resized
automatically and how large they can get in the process.

User Option: **resize-mini-windows** [¶](#index-resize_002dmini_002dwindows)
:   This option specifies whether minibuffer windows are resized
    automatically. The default value is `grow-only`, which means that
    a minibuffer window by default expands automatically to accommodate the
    text it displays and shrinks back to one line as soon as the minibuffer
    gets empty. If the value is `t`, Emacs will always try to fit the
    height of a minibuffer window to the text it displays (with a minimum of
    one line). If the value is `nil`, a minibuffer window never
    changes size automatically. In that case the window resizing commands
    (see [Resizing Windows](#Resizing-Windows)) can be used to adjust its height.

User Option: **max-mini-window-height** [¶](#index-max_002dmini_002dwindow_002dheight)
:   This option provides a maximum height for resizing minibuffer windows
    automatically. A floating-point number specifies the maximum height
    as a fraction of the frame’s height; an integer specifies the maximum
    height in units of the frame’s canonical character height
    (see [Frame Font](#Frame-Font)). The default value is 0.25.

Note that the values of the above two variables take effect at display
time, so let-binding them around code which produces echo-area messages
will not work. If you want to prevent resizing of minibuffer windows
when displaying long messages, bind the `message-truncate-lines`
variable instead (see [Echo Area Customization](#Echo-Area-Customization)).

The option `resize-mini-windows` does not affect the behavior of
minibuffer-only frames (see [Frame Layout](#Frame-Layout)). The following option
enables automatically resizing such frames as well.

User Option: **resize-mini-frames** [¶](#index-resize_002dmini_002dframes)
:   If this is `nil`, minibuffer-only frames are never resized
    automatically.

    If this is a function, that function is called with the
    minibuffer-only frame to be resized as sole argument. At the time
    this function is called, the buffer of the minibuffer window of that
    frame is the buffer whose contents will be shown the next time that
    window is redisplayed. The function is expected to fit the frame to
    the buffer in some appropriate way.

    Any other non-`nil` value means to resize minibuffer-only frames by
    calling `fit-mini-frame-to-buffer`, a function that behaves like
    `fit-frame-to-buffer` (see [Resizing Windows](#Resizing-Windows)) but does not strip
    leading or trailing empty lines from the buffer text.

---

Next: [Recursive Minibuffers](#Recursive-Mini), Previous: [Minibuffer Windows](#Minibuffer-Windows), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.12 Minibuffer Contents [¶](#Minibuffer-Contents-1)

These functions access the minibuffer prompt and contents.

Function: **minibuffer-prompt** [¶](#index-minibuffer_002dprompt)
:   This function returns the prompt string of the currently active
    minibuffer. If no minibuffer is active, it returns `nil`.

Function: **minibuffer-prompt-end** [¶](#index-minibuffer_002dprompt_002dend)
:   This function returns the current
    position of the end of the minibuffer prompt, if a minibuffer is
    current. Otherwise, it returns the minimum valid buffer position.

Function: **minibuffer-prompt-width** [¶](#index-minibuffer_002dprompt_002dwidth)
:   This function returns the current display-width of the minibuffer
    prompt, if a minibuffer is current. Otherwise, it returns zero.

Function: **minibuffer-contents** [¶](#index-minibuffer_002dcontents)
:   This function returns the editable
    contents of the minibuffer (that is, everything except the prompt) as
    a string, if a minibuffer is current. Otherwise, it returns the
    entire contents of the current buffer.

Function: **minibuffer-contents-no-properties** [¶](#index-minibuffer_002dcontents_002dno_002dproperties)
:   This is like `minibuffer-contents`, except that it does not copy text
    properties, just the characters themselves. See [Text Properties](#Text-Properties).

Command: **delete-minibuffer-contents** [¶](#index-delete_002dminibuffer_002dcontents)
:   This command erases the editable contents of the minibuffer (that is,
    everything except the prompt), if a minibuffer is current. Otherwise,
    it erases the entire current buffer.

---

Next: [Inhibiting Interaction](#Inhibiting-Interaction), Previous: [Minibuffer Contents](#Minibuffer-Contents), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.13 Recursive Minibuffers [¶](#Recursive-Minibuffers)

These functions and variables deal with recursive minibuffers
(see [Recursive Editing](#Recursive-Editing)):

Function: **minibuffer-depth** [¶](#index-minibuffer_002ddepth)
:   This function returns the current depth of activations of the
    minibuffer, a nonnegative integer. If no minibuffers are active, it
    returns zero.

User Option: **enable-recursive-minibuffers** [¶](#index-enable_002drecursive_002dminibuffers)
:   If this variable is non-`nil`, you can invoke commands (such as
    `find-file`) that use minibuffers even while the minibuffer is
    active. Such invocation produces a recursive editing level for a new
    minibuffer. By default, the outer-level minibuffer is invisible while
    you are editing the inner one. If you have
    `minibuffer-follows-selected-frame` set to `nil`, you can
    have minibuffers visible on several frames at the same time.
    See [(emacs)Basic Minibuffer](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Basic-Minibuffer).

    If this variable is `nil`, you cannot invoke minibuffer commands
    when the minibuffer is active, not even if you switch to another window
    to do it.

If a command name has a property `enable-recursive-minibuffers`
that is non-`nil`, then the command can use the minibuffer to read
arguments even if it is invoked from the minibuffer. A command can
also achieve this by binding `enable-recursive-minibuffers`
to `t` in the interactive declaration (see [Using `interactive`](#Using-Interactive)).
The minibuffer command `next-matching-history-element` (normally
`M-s` in the minibuffer) does the latter.

---

Next: [Minibuffer Miscellany](#Minibuffer-Misc), Previous: [Recursive Minibuffers](#Recursive-Mini), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.14 Inhibiting Interaction [¶](#Inhibiting-Interaction-1)

It’s sometimes useful to be able to run Emacs as a headless server
process that responds to commands given over a network connection.
However, Emacs is primarily a platform for interactive usage, so many
commands prompt the user for feedback in certain anomalous situations.
This makes this use case more difficult, since the server process will
just hang waiting for user input.

Binding the `inhibit-interaction` variable to something
non-`nil` makes Emacs signal a `inhibited-interaction` error
instead of prompting, which can then be used by the server process to
handle these situations.

Here’s a typical use case:

```
(let ((inhibit-interaction t))
  (respond-to-client
   (condition-case err
       (my-client-handling-function)
     (inhibited-interaction err))))
```

If `my-client-handling-function` ends up calling something that
asks the user for something (via `y-or-n-p` or
`read-from-minibuffer` or the like), an
`inhibited-interaction` error is signaled instead. The server
code then catches that error and reports it to the client.

---

Previous: [Inhibiting Interaction](#Inhibiting-Interaction), Up: [Minibuffers](#Minibuffers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 21.15 Minibuffer Miscellany [¶](#Minibuffer-Miscellany)

Function: **minibufferp** &optional buffer-or-name live [¶](#index-minibufferp)
:   This function returns non-`nil` if buffer-or-name is a
    minibuffer. If buffer-or-name is omitted or `nil`, it
    tests the current buffer. When live is non-`nil`, the
    function returns non-`nil` only when buffer-or-name is an
    active minibuffer.

Variable: **minibuffer-setup-hook** [¶](#index-minibuffer_002dsetup_002dhook)
:   This is a normal hook that is run whenever a minibuffer is entered.
    See [Hooks](#Hooks).

Macro: **minibuffer-with-setup-hook** function &rest body [¶](#index-minibuffer_002dwith_002dsetup_002dhook)
:   This macro executes body after arranging for the specified
    function to be called via `minibuffer-setup-hook`. By
    default, function is called before the other functions in the
    `minibuffer-setup-hook` list, but if function is of the
    form `(:append func)`, func will be called
    *after* the other hook functions.

    The body forms should not use the minibuffer more than once. If
    the minibuffer is re-entered recursively, function will only be
    called once, for the outermost use of the minibuffer.

Variable: **minibuffer-exit-hook** [¶](#index-minibuffer_002dexit_002dhook)
:   This is a normal hook that is run whenever a minibuffer is exited.
    See [Hooks](#Hooks).

Variable: **minibuffer-help-form** [¶](#index-minibuffer_002dhelp_002dform)
:   The current value of this variable is used to rebind `help-form`
    locally inside the minibuffer (see [Help Functions](#Help-Functions)).

Variable: **minibuffer-scroll-window** [¶](#index-minibuffer_002dscroll_002dwindow)
:   If the value of this variable is non-`nil`, it should be a window
    object. When the function `scroll-other-window` is called in the
    minibuffer, it scrolls this window (see [Textual Scrolling](#Textual-Scrolling)).

Function: **minibuffer-selected-window** [¶](#index-minibuffer_002dselected_002dwindow)
:   This function returns the window that was selected just before the
    minibuffer window was selected. If the selected window is not a
    minibuffer window, it returns `nil`.

Function: **minibuffer-message** string &rest args [¶](#index-minibuffer_002dmessage)
:   This function is like `message` (see [Displaying Messages in the Echo Area](#Displaying-Messages)),
    but it displays the messages specially when the user types in the
    minibuffer, typically because Emacs prompted the user for some input.
    When the minibuffer is the current buffer, this function displays the
    message specified by string temporarily at the end of the
    minibuffer text, and thus avoids hiding the minibuffer text by the
    echo-area display of the message. It leaves the message on display
    for a few seconds, or until the next input event arrives, whichever
    comes first. The variable `minibuffer-message-timeout` specifies
    the number of seconds to wait in the absence of input. It defaults to
    2. If args is non-`nil`, the actual message is obtained by
    passing string and args through `format-message`.
    See [Formatting Strings](#Formatting-Strings).

    If called when the minibuffer is not the current buffer, this function
    just calls `message`, and thus string will be shown in the
    echo-area.

Command: **minibuffer-inactive-mode** [¶](#index-minibuffer_002dinactive_002dmode)
:   This is the major mode used in inactive minibuffers. It uses
    keymap `minibuffer-inactive-mode-map`. This can be useful
    if the minibuffer is in a separate frame. See [Minibuffers and Frames](#Minibuffers-and-Frames).

Command: **minibuffer-regexp-mode** [¶](#index-minibuffer_002dregexp_002dmode)
:   This minor mode makes editing regular expressions in the minibuffer
    more convenient. It highlight parens via `show-paren-mode` and
    `blink-matching-paren` in a user-friendly way, avoids reporting
    false paren mismatches, and makes sexp navigation more intuitive.

By default, only certain minibuffer prompts automatically activate the
convenience features of `minibuffer-regexp-mode` when the
minibuffer becomes active. This list of prompts can be customized via
`minibuffer-regexp-prompts`.

User Option: **minibuffer-regexp-prompts** [¶](#index-minibuffer_002dregexp_002dprompts)
:   This variable holds the list of regular expressions for activating the
    features of `minibuffer-regexp-mode` in the minibuffer. The
    mode’s features will be activated only if the minibuffer prompt
    matches one of the regular expressions in the list.

---

Next: [Keymaps](#Keymaps), Previous: [Minibuffers](#Minibuffers), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 22 Command Loop [¶](#Command-Loop-1)

When you run Emacs, it enters the *editor command loop* almost
immediately. This loop reads key sequences, executes their definitions,
and displays the results. In this chapter, we describe how these things
are done, and the subroutines that allow Lisp programs to do them.

* [Command Loop Overview](#Command-Overview)
* [Defining Commands](#Defining-Commands)
* [Interactive Call](#Interactive-Call)
* [Distinguish Interactive Calls](#Distinguish-Interactive)
* [Information from the Command Loop](#Command-Loop-Info)
* [Adjusting Point After Commands](#Adjusting-Point)
* [Input Events](#Input-Events)
* [Reading Input](#Reading-Input)
* [Special Events](#Special-Events)
* [Waiting for Elapsed Time or Input](#Waiting)
* [Quitting](#Quitting)
* [Prefix Command Arguments](#Prefix-Command-Arguments)
* [Recursive Editing](#Recursive-Editing)
* [Disabling Commands](#Disabling-Commands)
* [Command History](#Command-History)
* [Keyboard Macros](#Keyboard-Macros)

---

Next: [Defining Commands](#Defining-Commands), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.1 Command Loop Overview [¶](#Command-Loop-Overview)

The first thing the command loop must do is read a key sequence,
which is a sequence of input events that translates into a command.
It does this by calling the function `read-key-sequence`. Lisp
programs can also call this function (see [Key Sequence Input](#Key-Sequence-Input)).
They can also read input at a lower level with `read-key` or
`read-event` (see [Reading One Event](#Reading-One-Event)), or discard pending
input with `discard-input` (see [Miscellaneous Event Input Features](#Event-Input-Misc)).

The key sequence is translated into a command through the currently
active keymaps. See [Key Lookup](#Key-Lookup), for information on how this is done.
The result should be a keyboard macro or an interactively callable
function. If the key is `M-x`, then it reads the name of another
command, which it then calls. This is done by the command
`execute-extended-command` (see [Interactive Call](#Interactive-Call)).

Prior to executing the command, Emacs runs `undo-boundary` to
create an undo boundary. See [Maintaining Undo Lists](#Maintaining-Undo).

To execute a command, Emacs first reads its arguments by calling
`command-execute` (see [Interactive Call](#Interactive-Call)). For commands
written in Lisp, the `interactive` specification says how to read
the arguments. This may use the prefix argument (see [Prefix Command Arguments](#Prefix-Command-Arguments)) or may read with prompting in the minibuffer
(see [Minibuffers](#Minibuffers)). For example, the command `find-file` has
an `interactive` specification which says to read a file name
using the minibuffer. The function body of `find-file` does not
use the minibuffer, so if you call `find-file` as a function from
Lisp code, you must supply the file name string as an ordinary Lisp
function argument.

If the command is a keyboard macro (i.e., a string or vector),
Emacs executes it using `execute-kbd-macro` (see [Keyboard Macros](#Keyboard-Macros)).

Variable: **pre-command-hook** [¶](#index-pre_002dcommand_002dhook)
:   This normal hook is run by the editor command loop before it executes
    each command. At that time, `this-command` contains the command
    that is about to run, and `last-command` describes the previous
    command. See [Information from the Command Loop](#Command-Loop-Info).

Variable: **post-command-hook** [¶](#index-post_002dcommand_002dhook)
:   This normal hook is run by the editor command loop after it executes
    each command (including commands terminated prematurely by quitting or
    by errors). At that time, `this-command` refers to the command
    that just ran, and `last-command` refers to the command before
    that.

    This hook is also run when Emacs first enters the command loop (at
    which point `this-command` and `last-command` are both
    `nil`).

Quitting is suppressed while running `pre-command-hook` and
`post-command-hook`. If an error happens while executing one of
these hooks, it does not terminate execution of the hook; instead
the error is silenced and the function in which the error occurred
is removed from the hook.

A request coming into the Emacs server (see [Emacs Server](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Emacs-Server) in The GNU Emacs Manual) runs these two hooks just as a keyboard
command does.

Note that, when the buffer text includes very long lines, these two
hooks are called as if they were in a `with-restriction` form
(see [Narrowing](#Narrowing)), with a
`long-line-optimizations-in-command-hooks` label and with the
buffer narrowed to a portion around point.

---

Next: [Interactive Call](#Interactive-Call), Previous: [Command Loop Overview](#Command-Overview), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.2 Defining Commands [¶](#Defining-Commands-1)

The special form `interactive` turns a Lisp function into a
command. The `interactive` form must be located at top-level in
the function body, usually as the first form in the body; this applies
to both lambda expressions (see [Lambda Expressions](#Lambda-Expressions)) and
`defun` forms (see [Defining Functions](#Defining-Functions)). This form does
nothing during the actual execution of the function; its presence
serves as a flag, telling the Emacs command loop that the function can
be called interactively. The argument of the `interactive` form
specifies how the arguments for an interactive call should be read.

Alternatively, an `interactive` form may be specified in a
function symbol’s `interactive-form` property. A non-`nil`
value for this property takes precedence over any `interactive`
form in the function body itself. This feature is seldom used.

Sometimes, a function is only intended to be called interactively,
never directly from Lisp. In that case, give the function a
non-`nil` `interactive-only` property, either directly
or via `declare` (see [The `declare` Form](#Declare-Form)). This causes the
byte compiler to warn if the command is called from Lisp. The output
of `describe-function` will include similar information.
The value of the property can be: a string, which the byte-compiler
will use directly in its warning (it should end with a period, and not
start with a capital, e.g., `"use (system-name) instead."`); `t`; any
other symbol, which should be an alternative function to use in Lisp
code.

Generic functions (see [Generic Functions](#Generic-Functions)) cannot be turned into
commands by adding the `interactive` form to them.

* [Using `interactive`](#Using-Interactive)
* [Code Characters for `interactive`](#Interactive-Codes)
* [Examples of Using `interactive`](#Interactive-Examples)
* [Specifying Modes For Commands](#Command-Modes)
* [Select among Command Alternatives](#Generic-Commands)

---

Next: [Code Characters for `interactive`](#Interactive-Codes), Up: [Defining Commands](#Defining-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.2.1 Using `interactive` [¶](#Using-interactive)

This section describes how to write the `interactive` form that
makes a Lisp function an interactively-callable command, and how to
examine a command’s `interactive` form.

Special Form: **interactive** &optional arg-descriptor &rest modes [¶](#index-interactive)
:   This special form declares that a function is a command, and that it
    may therefore be called interactively (via `M-x` or by entering a
    key sequence bound to it). The argument arg-descriptor declares
    how to compute the arguments to the command when the command is called
    interactively.

    A command may be called from Lisp programs like any other function, but
    then the caller supplies the arguments and arg-descriptor has no
    effect.

    The `interactive` form must be located at top-level in the
    function body, or in the function symbol’s `interactive-form`
    property (see [Symbol Properties](#Symbol-Properties)). It has its effect because the
    command loop looks for it before calling the function
    (see [Interactive Call](#Interactive-Call)). Once the function is called, all its body
    forms are executed; at this time, if the `interactive` form
    occurs within the body, the form simply returns `nil` without
    even evaluating its argument.

    The modes list allows specifying which modes the command is
    meant to be used in. See [Specifying Modes For Commands](#Command-Modes), for more details about
    the effect of specifying modes, and when to use it.

    By convention, you should put the `interactive` form in the
    function body, as the first top-level form. If there is an
    `interactive` form in both the `interactive-form` symbol
    property and the function body, the former takes precedence. The
    `interactive-form` symbol property can be used to add an
    interactive form to an existing function, or change how its arguments
    are processed interactively, without redefining the function.

There are three possibilities for the argument arg-descriptor:

* It may be omitted or `nil`; then the command is called with no
  arguments. This leads quickly to an error if the command requires one
  or more arguments.
* It may be a string; its contents are a sequence of elements separated
  by newlines, one for each argument[19](#FOOT19). Each element consists of a code character
  (see [Code Characters for `interactive`](#Interactive-Codes)) optionally followed by a prompt (which
  some code characters use and some ignore). Here is an example:

  ```
  (interactive "P\nbFrobnicate buffer: ")
  ```

  The code letter ‘`P`’ sets the command’s first argument to the raw
  command prefix (see [Prefix Command Arguments](#Prefix-Command-Arguments)). ‘`bFrobnicate
  buffer:` ’ prompts the user with ‘`Frobnicate buffer:` ’ to enter
  the name of an existing buffer, which becomes the second and final
  argument.

  The prompt string can use ‘`%`’ to include previous argument values
  (starting with the first argument) in the prompt. This is done using
  `format-message` (see [Formatting Strings](#Formatting-Strings)). For example, here is how
  you could read the name of an existing buffer followed by a new name to
  give to that buffer:

  ```
  (interactive "bBuffer to rename: \nsRename buffer %s to: ")
  ```

  If ‘`*`’ appears at the beginning of the string, then an error is
  signaled if the buffer is read-only.

  If ‘`@`’ appears at the beginning of the string, and if the key
  sequence used to invoke the command includes any mouse events, then
  the window associated with the first of those events is selected
  before the command is run.

  If ‘`^`’ appears at the beginning of the string, and if the command
  was invoked through *shift-translation*, set the mark and activate
  the region temporarily, or extend an already active region, before the
  command is run. If the command was invoked without shift-translation,
  and the region is temporarily active, deactivate the region before the
  command is run. Shift-translation is controlled on the user level by
  `shift-select-mode`; see [Shift Selection](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Shift-Selection) in The GNU
  Emacs Manual.

  You can use ‘`*`’, ‘`@`’, and `^` together; the order does
  not matter. Actual reading of arguments is controlled by the rest of
  the prompt string (starting with the first character that is not
  ‘`*`’, ‘`@`’, or ‘`^`’).
* It may be a Lisp expression that is not a string; then it should be a
  form that is evaluated to get a list of arguments to pass to the
  command. Usually this form will call various functions to read input
  from the user, most often through the minibuffer (see [Minibuffers](#Minibuffers))
  or directly from the keyboard (see [Reading Input](#Reading-Input)).

  Providing point or the mark as an argument value is also common, but
  if you do this *and* read input (whether using the minibuffer or
  not), be sure to get the integer values of point or the mark after
  reading. The current buffer may be receiving subprocess output; if
  subprocess output arrives while the command is waiting for input, it
  could relocate point and the mark.

  Here’s an example of what *not* to do:

  ```
  (interactive
   (list (region-beginning) (region-end)
         (read-string "Foo: " nil 'my-history)))
  ```

  Here’s how to avoid the problem, by examining point and the mark after
  reading the keyboard input:

  ```
  (interactive
   (let ((string (read-string "Foo: " nil 'my-history)))
     (list (region-beginning) (region-end) string)))
  ```

  **Warning:** the argument values should not include any data
  types that can’t be printed and then read. Some facilities save
  `command-history` in a file to be read in the subsequent
  sessions; if a command’s arguments contain a data type that prints
  using ‘`#<…>`’ syntax, those facilities won’t work.

  There are, however, a few exceptions: it is ok to use a limited set of
  expressions such as `(point)`, `(mark)`,
  `(region-beginning)`, and `(region-end)`, because Emacs
  recognizes them specially and puts the expression (rather than its
  value) into the command history. To see whether the expression you
  wrote is one of these exceptions, run the command, then examine
  `(car command-history)`.

Function: **interactive-form** function [¶](#index-interactive_002dform)
:   This function returns the `interactive` form of function.
    If function is an interactively callable function
    (see [Interactive Call](#Interactive-Call)), the value is the command’s
    `interactive` form `(interactive spec)`, which
    specifies how to compute its arguments. Otherwise, the value is
    `nil`. If function is a symbol, its function definition is
    used.
    When called on an OClosure, the work is delegated to the generic
    function `oclosure-interactive-form`.

Function: **oclosure-interactive-form** function [¶](#index-oclosure_002dinteractive_002dform)
:   Just like `interactive-form`, this function takes a command and
    returns its interactive form. The difference is that it is a generic
    function and it is only called when function is an OClosure
    (see [Open Closures](#OClosures)). The purpose is to make it possible for some
    OClosure types to compute their interactive forms dynamically instead
    of carrying it in one of their slots.

    This is used for example for `kmacro` functions in order to
    reduce their memory size, since they all share the same interactive
    form. It is also used for `advice` functions, where the
    interactive form is computed from the interactive forms of its
    components, so as to make this computation more lazily and to
    correctly adjust the interactive form when one of its component’s
    is redefined.

---

Next: [Examples of Using `interactive`](#Interactive-Examples), Previous: [Using `interactive`](#Using-Interactive), Up: [Defining Commands](#Defining-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.2.2 Code Characters for `interactive` [¶](#Code-Characters-for-interactive)

The code character descriptions below contain a number of key words,
defined here as follows:

**Completion** [¶](#index-interactive-completion)
:   Provide completion. `TAB`, `SPC`, and `RET` perform name
    completion because the argument is read using `completing-read`
    (see [Completion](#Completion)). `?` displays a list of possible completions.

**Existing**
:   Require the name of an existing object. An invalid name is not
    accepted; the commands to exit the minibuffer do not exit if the current
    input is not valid.

**Default** [¶](#index-default-argument-string)
:   A default value of some sort is used if the user enters no text in the
    minibuffer. The default depends on the code character.

**No I/O**
:   This code letter computes an argument without reading any input.
    Therefore, it does not use a prompt string, and any prompt string you
    supply is ignored.

    Even though the code letter doesn’t use a prompt string, you must follow
    it with a newline if it is not the last code character in the string.

**Prompt**
:   A prompt immediately follows the code character. The prompt ends either
    with the end of the string or with a newline.

**Special**
:   This code character is meaningful only at the beginning of the
    interactive string, and it does not look for a prompt or a newline.
    It is a single, isolated character.

Here are the code character descriptions for use with `interactive`:

‘`*`’
:   Signal an error if the current buffer is read-only. Special. Note that
    many Emacs primitives will signal an error if a command attempts to
    modify text of a read-only buffer, even if the command’s interactive
    spec does not include ‘`*`’. So this character is for when you want
    to forcibly signal an error even if the command’s implementation doesn’t
    invoke any primitives which modify the buffer, or if you want to avoid
    processing or side effects of a command that will fail later due to
    buffer modifications. For example, a command that binds
    `inhibit-read-only` non-`nil` (because it affects more than
    just read-only buffers), or a command which, depending on the
    conditions, might end up not modifying the buffer, but you want it to
    signal this error anyway.

‘`@`’
:   Select the window mentioned in the first mouse event in the key
    sequence that invoked this command. Special.

‘`^`’
:   If the command was invoked through shift-translation, set the mark and
    activate the region temporarily, or extend an already active region,
    before the command is run. If the command was invoked without
    shift-translation, and the region is temporarily active, deactivate
    the region before the command is run. Special.

‘`a`’
:   A function name (i.e., a symbol satisfying `fboundp`). Existing,
    Completion, Prompt.

‘`b`’
:   The name of an existing buffer. By default, uses the name of the
    current buffer (see [Buffers](#Buffers)). Existing, Completion, Default,
    Prompt.

‘`B`’
:   A buffer name. The buffer need not exist. By default, uses the name of
    a recently used buffer other than the current buffer. Completion,
    Default, Prompt.

‘`c`’
:   A character. The cursor does not move into the echo area. Prompt.

‘`C`’
:   A command name (i.e., a symbol satisfying `commandp`). Existing,
    Completion, Prompt.

‘`d`’ [¶](#index-position-argument)
:   The position of point, as an integer (see [Point](#Point)). No I/O.

‘`D`’
:   A directory. The default is the current default directory of the
    current buffer, `default-directory` (see [Functions that Expand Filenames](#File-Name-Expansion)).
    Existing, Completion, Default, Prompt.

‘`e`’
:   The first or next non-keyboard event in the key sequence that invoked
    the command. More precisely, ‘`e`’ gets events that are lists, so
    you can look at the data in the lists. See [Input Events](#Input-Events). No I/O.

    You use ‘`e`’ for mouse events and for special system events
    (see [Miscellaneous System Events](#Misc-Events)). The event list that the command receives
    depends on the event. See [Input Events](#Input-Events), which describes the forms
    of the list for each event in the corresponding subsections.

    You can use ‘`e`’ more than once in a single command’s interactive
    specification. If the key sequence that invoked the command has
    n events that are lists, the nth ‘`e`’ provides the
    nth such event. Events that are not lists, such as function keys
    and ASCII characters, do not count where ‘`e`’ is concerned.

‘`f`’
:   A file name of an existing file (see [File Names](#File-Names)). See [Reading File Names](#Reading-File-Names), for details about default values. Existing, Completion,
    Default, Prompt.

‘`F`’
:   A file name. The file need not exist. Completion, Default, Prompt.

‘`G`’
:   A file name. The file need not exist. If the user enters just a
    directory name, then the value is just that directory name, with no
    file name within the directory added. Completion, Default, Prompt.

‘`i`’
:   An irrelevant argument. This code always supplies `nil` as
    the argument’s value. No I/O.

‘`k`’
:   A key sequence (see [Key Sequences](#Key-Sequences)). This keeps reading events
    until a command (or undefined command) is found in the current key
    maps. The key sequence argument is represented as a string or vector.
    The cursor does not move into the echo area. Prompt.

    If ‘`k`’ reads a key sequence that ends with a down-event, it also
    reads and discards the following up-event. You can get access to that
    up-event with the ‘`U`’ code character.

    This kind of input is used by commands such as `describe-key` and
    `keymap-global-set`.

‘`K`’
:   A key sequence on a form that can be used as input to functions like
    `keymap-set`. This works like ‘`k`’, except that it
    suppresses, for the last input event in the key sequence, the
    conversions that are normally used (when necessary) to convert an
    undefined key into a defined one (see [Key Sequence Input](#Key-Sequence-Input)), so this
    form is usually used when prompting for a new key sequence that is to
    be bound to a command.

‘`m`’ [¶](#index-marker-argument)
:   The position of the mark, as an integer. No I/O.

‘`M`’
:   Arbitrary text, read in the minibuffer using the current buffer’s input
    method, and returned as a string (see [Input Methods](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Input-Methods) in The GNU
    Emacs Manual). Prompt.

‘`n`’
:   A number, read with the minibuffer. If the input is not a number, the
    user has to try again. ‘`n`’ never uses the prefix argument.
    Prompt.

‘`N`’
:   The numeric prefix argument; but if there is no prefix argument, read
    a number as with `n`. The value is always a number. See [Prefix Command Arguments](#Prefix-Command-Arguments). Prompt.

‘`p`’ [¶](#index-numeric-prefix-argument-usage)
:   The numeric prefix argument. (Note that this ‘`p`’ is lower case.)
    No I/O.

‘`P`’ [¶](#index-raw-prefix-argument-usage)
:   The raw prefix argument. (Note that this ‘`P`’ is upper case.) No
    I/O.

‘`r`’ [¶](#index-region-argument)
:   Point and the mark, as two numeric arguments, smallest first. This and
    the following are the only code letters that specify two successive
    arguments rather than one. ‘`r`’ will signal an error if the mark is
    not set in the buffer which is current when the command is invoked. If
    Transient Mark mode is turned on (see [The Mark](#The-Mark)) — as it is by
    default — and user option `mark-even-if-inactive` is `nil`,
    Emacs will signal an error even if the mark *is* set, but is
    inactive. No I/O.

‘`R`’
:   Point and the mark, as two numeric arguments, smallest first, but only
    if the region is active and it is appropriate to act on it, in the sense
    of `use-region-p` (see [The Region](#The-Region)). Otherwise supplies
    `nil` twice, for each of the successive arguments. This code
    letter never signals an error because if the mark is not set,
    `use-region-p` just returns nil. Use this code letter instead of
    ‘`r`’ for commands which act specially on an active region. No I/O.

‘`s`’
:   Arbitrary text, read in the minibuffer and returned as a string
    (see [Reading Text Strings with the Minibuffer](#Text-from-Minibuffer)). Terminate the input with either
    `C-j` or `RET`. (`C-q` may be used to include either of
    these characters in the input.) Prompt.

‘`S`’
:   An interned symbol whose name is read in the minibuffer. Terminate
    the input with either `C-j` or `RET`. Other characters that
    normally terminate a symbol (e.g., whitespace, parentheses and
    brackets) do not do so here. Prompt.

‘`U`’
:   A key sequence or `nil`. Can be used after a ‘`k`’ or
    ‘`K`’ argument to get the up-event that was discarded (if any)
    after ‘`k`’ or ‘`K`’ read a down-event. If no up-event has been
    discarded, ‘`U`’ provides `nil` as the argument. No I/O.

‘`v`’
:   A variable declared to be a user option (i.e., satisfying the
    predicate `custom-variable-p`). This reads the variable using
    `read-variable`. See [Definition of read-variable](#Definition-of-read_002dvariable). Existing,
    Completion, Prompt.

‘`x`’
:   A Lisp object, specified with its read syntax, terminated with a
    `C-j` or `RET`. The object is not evaluated. See [Reading Lisp Objects with the Minibuffer](#Object-from-Minibuffer). Prompt.

‘`X`’ [¶](#index-evaluated-expression-argument)
:   A Lisp form’s value. ‘`X`’ reads as ‘`x`’ does, then evaluates
    the form so that its value becomes the argument for the command.
    Prompt.

‘`z`’
:   A coding system name (a symbol). If the user enters null input, the
    argument value is `nil`. See [Coding Systems](#Coding-Systems). Completion,
    Existing, Prompt.

‘`Z`’
:   A coding system name (a symbol)—but only if this command has a prefix
    argument. With no prefix argument, ‘`Z`’ provides `nil` as the
    argument value. Completion, Existing, Prompt.

---

Next: [Specifying Modes For Commands](#Command-Modes), Previous: [Code Characters for `interactive`](#Interactive-Codes), Up: [Defining Commands](#Defining-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.2.3 Examples of Using `interactive` [¶](#Examples-of-Using-interactive)

Here are some examples of `interactive`:

```
(defun foo1 ()              ; foo1 takes no arguments,
    (interactive)           ;   just moves forward two words.
    (forward-word 2))
     ⇒ foo1
```

```

```

```
(defun foo2 (n)             ; foo2 takes one argument,
    (interactive "^p")      ;   which is the numeric prefix.
                            ; under shift-select-mode,
                            ;   will activate or extend region.
    (forward-word (* 2 n)))
     ⇒ foo2
```

```

```

```
(defun foo3 (n)             ; foo3 takes one argument,
    (interactive "nCount:") ;   which is read with the Minibuffer.
    (forward-word (* 2 n)))
     ⇒ foo3
```

```

```

```
(defun three-b (b1 b2 b3)
  "Select three existing buffers.
Put them into three windows, selecting the last one."
```

```
    (interactive "bBuffer1:\nbBuffer2:\nbBuffer3:")
    (delete-other-windows)
    (split-window (selected-window) 8)
    (switch-to-buffer b1)
    (other-window 1)
    (split-window (selected-window) 8)
    (switch-to-buffer b2)
    (other-window 1)
    (switch-to-buffer b3))
     ⇒ three-b
```

```
(three-b "*scratch*" "declarations.texi" "*mail*")
     ⇒ nil
```

---

Next: [Select among Command Alternatives](#Generic-Commands), Previous: [Examples of Using `interactive`](#Interactive-Examples), Up: [Defining Commands](#Defining-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.2.4 Specifying Modes For Commands [¶](#Specifying-Modes-For-Commands)

Many commands in Emacs are general, and not tied to any specific mode.
For instance, `M-x kill-region` can be used in pretty much any
mode that has editable text, and commands that display information
(like `M-x list-buffers`) can be used in pretty much any context.

Many other commands, however, are specifically tied to a mode, and
make no sense outside of that context. For instance, `M-x
dired-diff` will just signal an error if used outside of a Dired
buffer.

Emacs therefore has a mechanism for specifying what mode (or modes) a
command “belongs” to:

```
(defun dired-diff (...)
  ...
  (interactive "p" dired-mode)
  ...)
```

This will mark the command as applicable to `dired-mode` only (or
any modes that are derived from `dired-mode`). Any number of
modes can be added to the `interactive` form.

Specifying modes affects command completion in `M-S-x`
(`execute-extended-command-for-buffer`, see [Interactive Call](#Interactive-Call)). It may also affect completion in `M-x`, depending on the
value of `read-extended-command-predicate`.

For instance, when using the
`command-completion-default-include-p` predicate as the value of
`read-extended-command-predicate`, `M-x` won’t list commands
that have been marked as being applicable to a specific mode (unless
you are in a buffer that uses that mode, of course). This goes for
both major and minor modes. (By contrast, `M-S-x` always omits
inapplicable commands from the completion candidates.)

By default, `read-extended-command-predicate` is `nil`, and
completion in `M-x` lists all the commands that match what the
user has typed, whether those commands are or aren’t marked as
applicable to the current buffer’s mode.

Marking commands to be applicable to a mode will also make `C-h m`
list these commands (if they aren’t bound to any keys).

If using this extended `interactive` form isn’t convenient
(because the code is supposed to work in older versions of Emacs that
don’t support the extended `interactive` form), the following
equivalent declaration (see [The `declare` Form](#Declare-Form)) can be used instead:

```
(declare (modes dired-mode))
```

Which commands to tag with modes is to some degree a matter of taste,
but commands that clearly do not work outside of the mode should be
tagged. This includes commands that will signal an error if called
from somewhere else, but also commands that are destructive when
called from an unexpected mode. (This usually includes most of the
commands that are written for special (i.e., non-editing) modes.)

Some commands may be harmless, and “work” when called from other
modes, but should still be tagged with a mode if they don’t actually
make much sense to use elsewhere. For instance, many special modes
have commands to exit the buffer bound to `q`, and may not do
anything but issue a message like "Goodbye from this mode" and then
call `kill-buffer`. This command will “work” from any mode,
but it is highly unlikely that anybody would actually want to use the
command outside the context of this special mode.

Many modes have a set of different commands that start the mode in
different ways (e.g., `eww-open-in-new-buffer` and
`eww-open-file`). Commands like that should never be tagged as
mode-specific, as they can be issued by the user from pretty much any
context.

---

Previous: [Specifying Modes For Commands](#Command-Modes), Up: [Defining Commands](#Defining-Commands)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 22.2.5 Select among Command Alternatives [¶](#Select-among-Command-Alternatives)

Sometimes it is useful to define a command that serves as a “generic
dispatcher” capable of invoking one of a set of commands according to
the user’s needs. For example, imagine that you want to define a
command named ‘`open`’ that can “open” and display several
different types of objects. Or you could have a command named
‘`mua`’ (which stands for Mail User Agent) that can read and send
email using one of several email backends, such as Rmail, Gnus, or
MH-E. The macro `define-alternatives` can be used to define such
*generic commands*. A generic command is an interactive function
whose implementation can be selected from several alternatives, as a
matter of user preference.

Macro: **define-alternatives** command &rest customizations [¶](#index-define_002dalternatives)
:   This macro defines the new generic command, which can have
    several alternative implementations. The argument command
    should be an unquoted symbol.

    When invoked, the macro creates an interactive Lisp closure
    (see [Closures](#Closures)). When the user runs `M-x command RET` for the first time, Emacs asks to select one of the
    alternative implementations of command, offering completion for
    the names of these alternatives. These names come from the user
    option whose name is `command-alternatives`, which the
    macro creates (if it didn’t exist before). To be useful, this
    variable’s value should be an alist whose elements have the form
    `(alt-name . alt-func)`, where alt-name is
    the name of the alternative and alt-func is the interactive
    function to be called if this alternative is selected. When the user
    selects an alternative, Emacs remembers the selection, and will
    thereafter automatically call that selected alternative without
    prompting when the user invokes `M-x command` again. To
    choose a different alternative, type `C-u M-x command RET`–then Emacs will again prompt for one of the alternatives,
    and the selection will override the previous one.

    The variable `command-alternatives` can be created before
    calling `define-alternatives`, with the appropriate values;
    otherwise the macro creates the variable with a `nil` value, and
    it should then be populated with the associations describing the
    alternatives. Packages that wish to provide their own implementation
    of an existing generic command can use `autoload` cookies
    (see [Autoload](#Autoload)) to add to the alist, for example:

    ```
    ;;;###autoload (push '("My name" . my-foo-symbol) foo-alternatives
    ```

    If the optional argument customizations is non-`nil`, it
    should consist of alternating `defcustom` keywords (typically
    `:group` and `:version`) and values to add to the definition
    of the `defcustom` `command-alternatives`.

    Here is an example of a simple generic dispatcher command named
    `open` with 3 alternative implementations:

    ```
    (define-alternatives open
      :group 'files
      :version "42.1")
    ```

    ```
    (setq open-alternatives
          '(("file" . find-file)
    	("directory" . dired)
    	("hexl" . hexl-find-file)))
    ```

---

Next: [Distinguish Interactive Calls](#Distinguish-Interactive), Previous: [Defining Commands](#Defining-Commands), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.3 Interactive Call [¶](#Interactive-Call-1)

After the command loop has translated a key sequence into a command,
it invokes that command using the function `command-execute`. If
the command is a function, `command-execute` calls
`call-interactively`, which reads the arguments and calls the
command. You can also call these functions yourself.

Note that the term “command”, in this context, refers to an
interactively callable function (or function-like object), or a
keyboard macro. It does not refer to the key sequence used to invoke
a command (see [Keymaps](#Keymaps)).

Function: **commandp** object &optional for-call-interactively [¶](#index-commandp)
:   This function returns `t` if object is a command.
    Otherwise, it returns `nil`.

    Commands include strings and vectors (which are treated as keyboard
    macros), lambda expressions that contain a top-level
    `interactive` form (see [Using `interactive`](#Using-Interactive)), byte-code
    function objects made from such lambda expressions, autoload objects
    that are declared as interactive (non-`nil` fourth argument to
    `autoload`), and some primitive functions. Also, a symbol is
    considered a command if it has a non-`nil`
    `interactive-form` property, or if its function definition
    satisfies `commandp`.

    If for-call-interactively is non-`nil`, then
    `commandp` returns `t` only for objects that
    `call-interactively` could call—thus, not for keyboard macros.

    See `documentation` in [Access to Documentation Strings](#Accessing-Documentation), for a
    realistic example of using `commandp`.

Function: **call-interactively** command &optional record-flag keys [¶](#index-call_002dinteractively)
:   This function calls the interactively callable function command,
    providing arguments according to its interactive calling specifications.
    It returns whatever command returns.

    If, for instance, you have a function with the following signature:

    ```
    (defun foo (begin end)
      (interactive "r")
      ...)
    ```

    then saying

    ```
    (call-interactively 'foo)
    ```

    will call `foo` with the region (`point` and `mark`) as
    the arguments.

    An error is signaled if command is not a function or if it
    cannot be called interactively (i.e., is not a command). Note that
    keyboard macros (strings and vectors) are not accepted, even though
    they are considered commands, because they are not functions. If
    command is a symbol, then `call-interactively` uses its
    function definition.

    If record-flag is non-`nil`, then this command and its
    arguments are unconditionally added to the list `command-history`.
    Otherwise, the command is added only if it uses the minibuffer to read
    an argument. See [Command History](#Command-History).

    The argument keys, if given, should be a vector which specifies
    the sequence of events to supply if the command inquires which events
    were used to invoke it. If keys is omitted or `nil`, the
    default is the return value of `this-command-keys-vector`.
    See [Definition of this-command-keys-vector](#Definition-of-this_002dcommand_002dkeys_002dvector).

Function: **funcall-interactively** function &rest arguments [¶](#index-funcall_002dinteractively)
:   This function works like `funcall` (see [Calling Functions](#Calling-Functions)),
    but it makes the call look like an interactive invocation: a call to
    `called-interactively-p` inside function will return
    `t`. If function is not a command, it is called without
    signaling an error.

Function: **command-execute** command &optional record-flag keys special [¶](#index-command_002dexecute)
:   This function executes command. The argument command must
    satisfy the `commandp` predicate; i.e., it must be an interactively
    callable function or a keyboard macro.

    A string or vector as command is executed with
    `execute-kbd-macro`. A function is passed to
    `call-interactively` (see above), along with the
    record-flag and keys arguments.

    If command is a symbol, its function definition is used in its
    place. A symbol with an `autoload` definition counts as a
    command if it was declared to stand for an interactively callable
    function. Such a definition is handled by loading the specified
    library and then rechecking the definition of the symbol.

    The argument special, if given, means to ignore the prefix
    argument and not clear it. This is used for executing special events
    (see [Special Events](#Special-Events)).

Command: **execute-extended-command** prefix-argument [¶](#index-execute_002dextended_002dcommand)
:   This function reads a command name from the minibuffer using
    `completing-read` (see [Completion](#Completion)). Then it uses
    `command-execute` to call the specified command. Whatever that
    command returns becomes the value of `execute-extended-command`.

    If the command asks for a prefix argument, it receives the value
    prefix-argument. If `execute-extended-command` is called
    interactively, the current raw prefix argument is used for
    prefix-argument, and thus passed on to whatever command is run.

    `execute-extended-command` is the normal definition of `M-x`,
    so it uses the string ‘`M-x`’ as a prompt. (It would be better
    to take the prompt from the events used to invoke
    `execute-extended-command`, but that is painful to implement.) A
    description of the value of the prefix argument, if any, also becomes
    part of the prompt.

    ```
    (execute-extended-command 3)
    ---------- Buffer: Minibuffer ----------
    3 M-x forward-word RET
    ---------- Buffer: Minibuffer ----------
         ⇒ t
    ```

    This command heeds the `read-extended-command-predicate`
    variable, which can filter out commands that are not applicable to the
    current major mode (or enabled minor modes). By default, the value of
    this variable is `nil`, and no commands are filtered out.
    However, customizing it to invoke the function
    `command-completion-default-include-p` will perform
    mode-dependent filtering. `read-extended-command-predicate` can
    be any predicate function; it will be called with two parameters: the
    command’s symbol and the current buffer. If should return
    non-`nil` if the command is to be included when completing in
    that buffer.

Command: **execute-extended-command-for-buffer** prefix-argument [¶](#index-execute_002dextended_002dcommand_002dfor_002dbuffer)
:   This is like `execute-extended-command`, but limits the commands
    offered for completion to those commands that are of particular
    relevance to the current major mode (and enabled minor modes). This
    includes commands that are tagged with the modes (see [Using `interactive`](#Using-Interactive)), and also commands that are bound to locally active
    keymaps. This command is the normal definition of `M-S-x`
    (that’s “meta shift x”).

Both these commands prompt for a command name, but with different
completion rules. You can toggle between these two modes by using the
`M-S-x` command while being prompted.

---

Next: [Information from the Command Loop](#Command-Loop-Info), Previous: [Interactive Call](#Interactive-Call), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.4 Distinguish Interactive Calls [¶](#Distinguish-Interactive-Calls)

Sometimes a command should display additional visual feedback (such
as an informative message in the echo area) for interactive calls
only. There are three ways to do this. The recommended way to test
whether the function was called using `call-interactively` is to
give it an optional argument `print-message` and use the
`interactive` spec to make it non-`nil` in interactive
calls. Here’s an example:

```
(defun foo (&optional print-message)
  (interactive "p")
  (when print-message
    (message "foo")))
```

We use `"p"` because the numeric prefix argument is never
`nil`. Defined in this way, the function does display the
message when called from a keyboard macro.

The above method with the additional argument is usually best,
because it allows callers to say “treat this call as interactive”.
But you can also do the job by testing `called-interactively-p`.

Function: **called-interactively-p** kind [¶](#index-called_002dinteractively_002dp)
:   This function returns `t` when the calling function was called
    using `call-interactively`.

    The argument kind should be either the symbol `interactive`
    or the symbol `any`. If it is `interactive`, then
    `called-interactively-p` returns `t` only if the call was
    made directly by the user—e.g., if the user typed a key sequence
    bound to the calling function, but *not* if the user ran a
    keyboard macro that called the function (see [Keyboard Macros](#Keyboard-Macros)). If
    kind is `any`, `called-interactively-p` returns
    `t` for any kind of interactive call, including keyboard macros.

    If in doubt, use `any`; the only known proper use of
    `interactive` is if you need to decide whether to display a
    helpful message while a function is running.

    A function is never considered to be called interactively if it was
    called via Lisp evaluation (or with `apply` or `funcall`).

Here is an example of using `called-interactively-p`:

```
(defun foo ()
  (interactive)
  (when (called-interactively-p 'any)
    (message "Interactive!")
    'foo-called-interactively))
```

```

```

```
;; Type M-x foo.
     -| Interactive!
```

```

```

```
(foo)
     ⇒ nil
```

Here is another example that contrasts direct and indirect calls to
`called-interactively-p`.

```
(defun bar ()
  (interactive)
  (message "%s" (list (foo) (called-interactively-p 'any))))
```

```

```

```
;; Type M-x bar.
     -| (nil t)
```

---

Next: [Adjusting Point After Commands](#Adjusting-Point), Previous: [Distinguish Interactive Calls](#Distinguish-Interactive), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.5 Information from the Command Loop [¶](#Information-from-the-Command-Loop)

The editor command loop sets several Lisp variables to keep status
records for itself and for commands that are run. With the exception of
`this-command` and `last-command` it’s generally a bad idea to
change any of these variables in a Lisp program.

Variable: **last-command** [¶](#index-last_002dcommand)
:   This variable records the name of the previous command executed by the
    command loop (the one before the current command). Normally the value
    is a symbol with a function definition, but this is not guaranteed.

    The value is copied from `this-command` when a command returns to
    the command loop, except when the command has specified a prefix
    argument for the following command.

    This variable is always local to the current terminal and cannot be
    buffer-local. See [Multiple Terminals](#Multiple-Terminals).

Variable: **real-last-command** [¶](#index-real_002dlast_002dcommand)
:   This variable is set up by Emacs just like `last-command`,
    but never altered by Lisp programs.

Variable: **last-repeatable-command** [¶](#index-last_002drepeatable_002dcommand)
:   This variable stores the most recently executed command that was not
    part of an input event. This is the command `repeat` will try to
    repeat, See [Repeating](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Repeating) in The GNU Emacs Manual.

Variable: **this-command** [¶](#index-this_002dcommand)
:   This variable records the name of the command now being executed by
    the editor command loop. Like `last-command`, it is normally a symbol
    with a function definition.

    The command loop sets this variable just before running a command, and
    copies its value into `last-command` when the command finishes
    (unless the command specified a prefix argument for the following
    command).

    Some commands set this variable during their execution, as a flag for
    whatever command runs next. In particular, the functions for killing text
    set `this-command` to `kill-region` so that any kill commands
    immediately following will know to append the killed text to the
    previous kill.

If you do not want a particular command to be recognized as the previous
command in the case where it got an error, you must code that command to
prevent this. One way is to set `this-command` to `t` at the
beginning of the command, and set `this-command` back to its proper
value at the end, like this:

```
(defun foo (args...)
  (interactive ...)
  (let ((old-this-command this-command))
    (setq this-command t)
    ...do the work...
    (setq this-command old-this-command)))
```

We do not bind `this-command` with `let` because that would
restore the old value in case of error—a feature of `let` which
in this case does precisely what we want to avoid.

Variable: **this-original-command** [¶](#index-this_002doriginal_002dcommand)
:   This has the same value as `this-command` except when command
    remapping occurs (see [Remapping Commands](#Remapping-Commands)). In that case,
    `this-command` gives the command actually run (the result of
    remapping), and `this-original-command` gives the command that
    was specified to run but remapped into another command.

Variable: **current-minibuffer-command** [¶](#index-current_002dminibuffer_002dcommand)
:   This has the same value as `this-command`, but is bound
    recursively when entering a minibuffer. This variable can be used
    from minibuffer hooks and the like to determine what command opened
    the current minibuffer session.

Function: **this-command-keys** [¶](#index-this_002dcommand_002dkeys)
:   This function returns a string or vector containing the key sequence
    that invoked the present command. Any events read by the command
    using `read-event` without a timeout get tacked on to the end.

    However, if the command has called `read-key-sequence`, it
    returns the last read key sequence. See [Key Sequence Input](#Key-Sequence-Input). The
    value is a string if all events in the sequence were characters that
    fit in a string. See [Input Events](#Input-Events).

    ```
    (this-command-keys)
    ;; Now use C-u C-x C-e to evaluate that.
         ⇒ "^X^E"
    ```

Function: **this-command-keys-vector** [¶](#index-this_002dcommand_002dkeys_002dvector)
:   Like `this-command-keys`, except that it always returns the events
    in a vector, so you don’t need to deal with the complexities of storing
    input events in a string (see [Putting Keyboard Events in Strings](#Strings-of-Events)).

Function: **clear-this-command-keys** &optional keep-record [¶](#index-clear_002dthis_002dcommand_002dkeys)
:   This function empties out the table of events for
    `this-command-keys` to return. Unless keep-record is
    non-`nil`, it also empties the records that the function
    `recent-keys` (see [Recording Input](#Recording-Input)) will subsequently return.
    This is useful after reading a password, to prevent the password from
    echoing inadvertently as part of the next command in certain cases.

Variable: **last-nonmenu-event** [¶](#index-last_002dnonmenu_002devent)
:   This variable holds the last input event read as part of a key sequence,
    not counting events resulting from mouse menus.

    One use of this variable is for telling `x-popup-menu` where to pop
    up a menu. It is also used internally by `y-or-n-p`
    (see [Yes-or-No Queries](#Yes_002dor_002dNo-Queries)).

Variable: **last-command-event** [¶](#index-last_002dcommand_002devent)
:   This variable is set to the last input event that was read by the
    command loop as part of a command. The principal use of this variable
    is in `self-insert-command`, which uses it to decide which
    character to insert, and in `post-self-insert-hook`
    (see [User-Level Insertion Commands](#Commands-for-Insertion)), which uses it to access the
    character that was just inserted.

    ```
    last-command-event
    ;; Now use C-u C-x C-e to evaluate that.
         ⇒ 5
    ```

    The value is 5 because that is the ASCII code for `C-e`.

Variable: **last-event-frame** [¶](#index-last_002devent_002dframe)
:   This variable records which frame the last input event was directed to.
    Usually this is the frame that was selected when the event was
    generated, but if that frame has redirected input focus to another
    frame, the value is the frame to which the event was redirected.
    See [Input Focus](#Input-Focus).

    If the last event came from a keyboard macro, the value is `macro`.

Input events must come from somewhere; sometimes, that is a keyboard
macro, a signal, or `unread-command-events`, but it is usually a
physical input device connected to a computer that is controlled by
the user. Those devices are referred to as *input devices*, and
Emacs associates each input event with the input device from which it
originated. They are identified by a name that is unique to each
input device.

The ability to determine the precise input device used depends on the
details of each system. When that information is unavailable, Emacs
reports keyboard events as originating from the ‘`"Virtual core
keyboard"`’, and other events as originating from the ‘`"Virtual
core pointer"`’. (These values are used on every platform because the
X server reports them when detailed device information is not known.)

Variable: **last-event-device** [¶](#index-last_002devent_002ddevice)
:   This variable records the name of the input device from which the last
    input event read was generated. It is `nil` if no such device
    exists, i.e., the last input event was read from
    `unread-command-events`, or it came from a keyboard macro.

    When the X Input Extension is being used on X Windows, the device name
    is a string that is unique to each physical keyboard, pointing device
    and touchscreen attached to the X server. Otherwise, it is either the
    string ‘`"Virtual core pointer"`’ or ‘`"Virtual core
    keyboard"`’, depending on whether the event was generated by a pointing
    device (such as a mouse) or a keyboard.

Function: **device-class** frame name [¶](#index-device_002dclass)
:   There are various different types of devices, which can be determined
    from their names. This function can be used to determined the correct
    type of the device name for an event originating from
    frame.

    The return value is one of the following symbols (“device classes”):

    `core-keyboard`
    :   The core keyboard; this is means the device is a keyboard-like device,
        but no other characteristics are unknown.

    `core-pointer`
    :   The core pointer; this means the device is a pointing device, but no
        other characteristics are known.

    `mouse`
    :   A computer mouse.

    `trackpoint`
    :   A trackpoint or joystick (or other similar control.)

    `eraser`
    :   The other end of a stylus on a graphics tablet, or a standalone
        eraser.

    `pen`
    :   The pointed end of a pen on a graphics tablet, a stylus, or some other
        similar device.

    `puck`
    :   A device that looks like a computer mouse, but reports absolute
        coordinates relative to some other surface.

    `power-button`
    :   A power button or volume button (or other similar control.)

    `keyboard`
    :   A computer keyboard.

    `touchscreen`
    :   A computer touchpad.

    `pad`
    :   A collection of sensitive buttons, rings, and strips commonly found
        around a drawing tablet.

    `touchpad`
    :   An indirect touch device such as a touchpad.

    `piano`
    :   A musical instrument such as an electronic keyboard.

    `test`
    :   A device used by the XTEST extension to report input.

---

Next: [Input Events](#Input-Events), Previous: [Information from the Command Loop](#Command-Loop-Info), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 22.6 Adjusting Point After Commands [¶](#Adjusting-Point-After-Commands)

When a sequence of text has the `display` or `composition`
property, or is invisible, there can be several buffer positions that
result in the cursor being displayed at same place on the screen.
Therefore, after a command finishes and returns to the command loop,
if point is in such a sequence, the command loop normally moves point
to try and make this sequence effectively intangible.

This *point adjustment* follows the following general rules: first, the
adjustment should not change the overall direction of the command;
second if the command moved point, the adjustment tries to ensure the
cursor is also moved; third, Emacs prefers the edges of an intangible
sequence and among those edges it prefers the non sticky ones, such
that newly inserted text is visible.

A command can inhibit this feature by setting the variable
`disable-point-adjustment`:

Variable: **disable-point-adjustment** [¶](#index-disable_002dpoint_002dadjustment)
:   If this variable is non-`nil` when a command returns to the
    command loop, then the command loop does not check for those text
    properties, and does not move point out of sequences that have them.

    The command loop sets this variable to `nil` before each command,
    so if a command sets it, the effect applies only to that command.

Variable: **global-disable-point-adjustment** [¶](#index-global_002ddisable_002dpoint_002dadjustment)
:   If you set this variable to a non-`nil` value, the feature of
    moving point out of these sequences is completely turned off.

---

Next: [Reading Input](#Reading-Input), Previous: [Adjusting Point After Commands](#Adjusting-Point), Up: [Command Loop](#Command-Loop)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
