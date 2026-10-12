### E.3.4 Completion Options

When completion is done on file names, certain file names are usually
ignored. The variable `completion-ignored-extensions` contains a
list of strings; a file whose name ends in any of those strings is
ignored as a possible completion. The standard value of this variable
has several elements including `".o"`, `".elc"`, `".dvi"`
and `"~"`. The effect is that, for example, `` `foo' `` can
complete to `` `foo.c' `` even though `` `foo.o' `` exists as well.
However, if *all* the possible completions end in "ignored"
strings, then they are not ignored. Ignored extensions do not apply to
lists of completions--those always mention all possible completions.

Normally, a completion command that cannot determine even one
additional character automatically displays a list of all possible
completions. If the variable `completion-auto-help` is set to
`nil`, this automatic display is disabled, so you must type
`?` to display the list of completions.

Partial Completion mode implements a more powerful kind of
completion that can complete multiple words in parallel. For example,
it can complete the command name abbreviation `p-b` into
`print-buffer`, because no other command starts with two words
whose initials are `` `p' `` and `` `b' ``.

Partial completion of directories in file names uses `` `*' `` to
indicate the places for completion; thus, `/u\*/b\*/f\*' might
complete to `/usr/bin/foo'.

To enable this mode, use the command `M-x
partial-completion-mode`, or customize the option
`partial-completion-mode`. This binds the partial completion
commands to `TAB`, `SPC`, `RET`, and `?`. The usual
completion commands are available on `M-TAB`,
`M-SPC`, `M-RET` and `M-?`.

Another feature of Partial Completion mode is to extend
`find-file` so that the `` `<include>' `` stands for the
file named include in some directory in the path
`PC-include-file-path`. If you set `PC-disable-includes` to
non-`nil`, this feature is disabled.

Icomplete mode presents a constantly-updated display that tells you
what completions are available for the text you've entered so far. The
command to enable or disable this minor mode is `M-x
icomplete-mode`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC38) ] | [ [>](emacs.html#SEC40) ] | [ [<<](emacs.html#SEC40) ] | [ [Up](emacs.html#SEC31) ] | [ [>>](emacs.html#SEC41) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## E.4 Minibuffer History

Every argument that you enter with the minibuffer is saved on a
*minibuffer history list* so that you can use it again later in
another argument. Special commands load the text of an earlier argument
in the minibuffer. They discard the old minibuffer contents, so you can
think of them as moving through the history of previous arguments.

`UP`: `M-p`: Move to the next earlier argument string saved in the minibuffer history (`previous-history-element`). `DOWN`: `M-n`: Move to the next later argument string saved in the minibuffer history (`next-history-element`). `M-r regexp RET`: Move to an earlier saved argument in the minibuffer history that has a match for regexp (`previous-matching-history-element`). `M-s regexp RET`: Move to a later saved argument in the minibuffer history that has a match for regexp (`next-matching-history-element`).

The simplest way to reuse the saved arguments in the history list is
to move through the history list one element at a time. While in the
minibuffer, use `M-p` or up-arrow (`previous-history-element`)
to "move to" the next earlier minibuffer input, and use `M-n` or
down-arrow (`next-history-element`) to "move to" the next later
input.

The previous input that you fetch from the history entirely replaces
the contents of the minibuffer. To use it as the argument, exit the
minibuffer as usual with `RET`. You can also edit the text before
you reuse it; this does not change the history element that you
"moved" to, but your new argument does go at the end of the history
list in its own right.

For many minibuffer arguments there is a "default" value. In some
cases, the minibuffer history commands know the default value. Then you
can insert the default value into the minibuffer as text by using
`M-n` to move "into the future" in the history. Eventually we
hope to make this feature available whenever the minibuffer has a
default value.

There are also commands to search forward or backward through the
history; they search for history elements that match a regular
expression that you specify with the minibuffer. `M-r`
(`previous-matching-history-element`) searches older elements in
the history, while `M-s` (`next-matching-history-element`)
searches newer elements. By special dispensation, these commands can
use the minibuffer to read their arguments even though you are already
in the minibuffer when you issue them. As with incremental searching,
an upper-case letter in the regular expression makes the search
case-sensitive (see section [K.6 Searching and Case](emacs.html#SEC98)).

All uses of the minibuffer record your input on a history list, but
there are separate history lists for different kinds of arguments. For
example, there is a list for file names, used by all the commands that
read file names. (As a special feature, this history list records
the absolute file name, no more and no less, even if that is not how
you entered the file name.)

There are several other very specific history lists, including one for
command names read by `M-x`, one for buffer names, one for arguments
of commands like `query-replace`, and one for compilation commands
read by `compile`. Finally, there is one "miscellaneous" history
list that most minibuffer arguments use.

The variable `history-length` specifies the maximum length of a
minibuffer history list; once a list gets that long, the oldest element
is deleted each time an element is added. If the value of
`history-length` is `t`, though, there is no maximum length
and elements are never deleted.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC39) ] | [ [>](emacs.html#SEC41) ] | [ [<<](emacs.html#SEC31) ] | [ [Up](emacs.html#SEC31) ] | [ [>>](emacs.html#SEC41) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## E.5 Repeating Minibuffer Commands

Every command that uses the minibuffer at least once is recorded on a
special history list, together with the values of its arguments, so that
you can repeat the entire command. In particular, every use of
`M-x` is recorded there, since `M-x` uses the minibuffer to read
the command name.

`C-x ESC ESC`: Re-execute a recent minibuffer command (`repeat-complex-command`). `M-x list-command-history`: Display the entire command history, showing all the commands `C-x ESC ESC` can repeat, most recent first.

`C-x ESC ESC` is used to re-execute a recent
minibuffer-using command. With no argument, it repeats the last such
command. A numeric argument specifies which command to repeat; one
means the last one, and larger numbers specify earlier ones.

`C-x ESC ESC` works by turning the previous command
into a Lisp expression and then entering a minibuffer initialized with
the text for that expression. If you type just `RET`, the command
is repeated as before. You can also change the command by editing the
Lisp expression. Whatever expression you finally submit is what will be
executed. The repeated command is added to the front of the command
history unless it is identical to the most recently executed command
already there.

Even if you don't understand Lisp syntax, it will probably be obvious
which command is displayed for repetition. If you do not change the
text, it will repeat exactly as before.

Once inside the minibuffer for `C-x ESC ESC`, you can
use the minibuffer history commands (`M-p`, `M-n`, `M-r`,
`M-s`; see section [E.4 Minibuffer History](emacs.html#SEC39)) to move through the history list
of saved entire commands. After finding the desired previous command,
you can edit its expression as usual and then resubmit it by typing
`RET` as usual.

The list of previous minibuffer-using commands is stored as a Lisp
list in the variable `command-history`. Each element is a Lisp
expression which describes one command and its arguments. Lisp programs
can re-execute a command by calling `eval` with the
`command-history` element.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC40) ] | [ [>](emacs.html#SEC42) ] | [ [<<](emacs.html#SEC31) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC42) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# F. Running Commands by Name

Every Emacs command has a name that you can use to run it. Commands
that are used often, or that must be quick to type, are also bound to
keys--short sequences of characters--for convenient use. You can
run them by name if you don't remember the keys. Other Emacs commands
that do not need to be quick are not bound to keys; the only way to
run them is by name. See section [AD.4 Customizing Key Bindings](emacs.html#SEC491), for the description of
how to bind commands to keys.

By convention, a command name consists of one or more words,
separated by hyphens; for example, `auto-fill-mode` or
`manual-entry`. The use of English words makes the command name
easier to remember than a key made up of obscure characters, even
though it is more characters to type.

The way to run a command by name is to start with `M-x`, type the
command name, and finish it with `RET`. `M-x` uses the
minibuffer to read the command name. `RET` exits the minibuffer and
runs the command. The string `` `M-x' `` appears at the beginning of the
minibuffer as a *prompt* to remind you to enter the name of a
command to be run. See section [E. The Minibuffer](emacs.html#SEC31), for full information on the
features of the minibuffer.

You can use completion to enter the command name. For example, you
can invoke the command `forward-char` by name by typing either

|  |  |
| --- | --- |
|  | ``` M-x forward-char RET ``` |

or

|  |  |
| --- | --- |
|  | ``` M-x forw TAB c RET ``` |

Note that `forward-char` is the same command that you invoke with
the key `C-f`. You can run any Emacs command by name using
`M-x`, whether or not any keys are bound to it.

If you type `C-g` while the command name is being read, you cancel
the `M-x` command and get out of the minibuffer, ending up at top level.

To pass a numeric argument to the command you are invoking with
`M-x`, specify the numeric argument before the `M-x`. `M-x`
passes the argument along to the command it runs. The argument value
appears in the prompt while the command name is being read.

If the command you type has a key binding of its own, Emacs mentions
this in the echo area, two seconds after the command finishes (if you
don't type anything else first). For example, if you type `M-x
forward-word`, the message says that you can run the same command more
easily by typing `M-f`. You can turn off these messages by setting
`suggest-key-bindings` to `nil`.

Normally, when describing in this manual a command that is run by
name, we omit the `RET` that is needed to terminate the name. Thus
we might speak of `M-x auto-fill-mode` rather than `M-x
auto-fill-mode RET`. We mention the `RET` only when there is
a need to emphasize its presence, such as when we show the command
together with following arguments.

`M-x` works by running the command
`execute-extended-command`, which is responsible for reading the
name of another command and invoking it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC41) ] | [ [>](emacs.html#SEC43) ] | [ [<<](emacs.html#SEC41) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# G. Help

Emacs provides extensive help features accessible through a single
character, `C-h`. `C-h` is a prefix key that is used for
commands that display documentation. The characters that you can type after
`C-h` are called *help options*. One help option is `C-h`;
that is how you ask for help about using `C-h`. To cancel, type
`C-g`. The function key `F1` is equivalent to `C-h`.

`C-h C-h` (`help-for-help`) displays a list of the possible
help options, each with a brief description. Before you type a help
option, you can use `SPC` or `DEL` to scroll through the list.

`C-h` or `F1` means "help" in various other contexts as
well. For example, in the middle of `query-replace`, it describes
the options available for how to operate on the current match. After a
prefix key, it displays a list of the alternatives that can follow the
prefix key. (A few prefix keys don't support `C-h`, because they
define other meanings for it, but they all support `F1`.)

Most help buffers use a special major mode, Help mode, which lets you
scroll conveniently with `SPC` and `DEL`. It also offers
hyperlinks to further help regarding cross-referenced names, Info nodes,
customization buffers and the like. See section [G.7 Help Mode Commands](emacs.html#SEC49).

If you are looking for a certain feature, but don't know where
exactly it is documented, and aren't sure of the name of a
related command or option, we recommend trying these methods. Usually
it is best to start with an apropos command, then try searching the
manual index, then finally look in the FAQ and the package keywords.

`C-h a topic RET`: This searches for commands whose names match topic, which should be a regular expression (see section [K.5 Syntax of Regular Expressions](emacs.html#SEC97)). Browse the buffer that this command displays to find what you are looking for. See section [G.4 Apropos](emacs.html#SEC46). `M-x apropos RET topic RET`: This works like `C-h a`, but it also searches for user options and other variables, in case the feature you are looking for is controlled by an option, not a command. See section [G.4 Apropos](emacs.html#SEC46). `M-x apropos-documentation RET topic RET`: This searches the *documentation strings* (the built-in short descriptions) of all variables and functions (not their names) for a match for topic, a regular expression. See section [G.4 Apropos](emacs.html#SEC46). `C-h i d m emacs RET i topic RET`: This looks up topic in the indices of the Emacs on-line manual. If there are several matches, Emacs displays the first one. You can then press `,` to move to other matches, until you find what you are looking for. `C-h i d m emacs RET s topic RET`: Similar, but searches for topic (which can be a regular expression) in the *text* of the manual rather than in its indices. `C-h F`: This brings up the Emacs FAQ, where you can use the usual search commands (see section [K. Searching and Replacement](emacs.html#SEC91)) to find the information. `C-h p`: Finally, you can try looking up a suitable package using keywords pertinent to the feature you need. See section [G.5 Keyword Search for Lisp Libraries](emacs.html#SEC47).

To find the documentation of a key sequence or a menu item, type
`C-h C-k` and then type that key sequence or select the menu
item. This looks up the description of the command invoked by the key
or the menu in the appropriate manual (not necessarily the Emacs
manual). Likewise, use `C-h C-f` for reading documentation of a
command.

> |  |  |  |
> | --- | --- | --- |
> | [G.1 Help Summary](emacs.html#SEC43) |  | Brief list of all Help commands. |
> | [G.2 Documentation for a Key](emacs.html#SEC44) |  | Asking what a key does in Emacs. |
> | [G.3 Help by Command or Variable Name](emacs.html#SEC45) |  | Asking about a command, variable or function name. |
> | [G.4 Apropos](emacs.html#SEC46) |  | Asking what pertains to a given topic. |
> | [G.5 Keyword Search for Lisp Libraries](emacs.html#SEC47) |  | Finding Lisp libraries by keywords (topics). |
> | [G.6 Help for International Language Support](emacs.html#SEC48) |  | Help relating to international language support. |
> | [G.7 Help Mode Commands](emacs.html#SEC49) |  | Special features of Help mode and Help buffers. |
> | [G.8 Other Help Commands](emacs.html#SEC50) |  | Other help commands. |
> | [G.9 Help on Active Text and Tooltips](emacs.html#SEC51) |  | Help on active text and tooltips (`balloon help') |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC42) ] | [ [>](emacs.html#SEC44) ] | [ [<<](emacs.html#SEC42) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.1 Help Summary

Here is a summary of the defined help commands.

`C-h a regexp RET`: Display a list of commands whose names match regexp (`apropos-command`). `C-h b`: Display a table of all key bindings in effect now, in this order: minor mode bindings, major mode bindings, and global bindings (`describe-bindings`). `C-h c key`: Show the name of the command that key runs (`describe-key-briefly`). Here `c` stands for "character." For more extensive information on key, use `C-h k`. `C-h f function RET`: Display documentation on the Lisp function named function (`describe-function`). Since commands are Lisp functions, a command name may be used. `C-h h`: Display the `HELLO' file, which shows examples of various character sets. `C-h i`: Run Info, the program for browsing documentation files (`info`). The complete Emacs manual is available on-line in Info. `C-h k key`: Display the name and documentation of the command that key runs (`describe-key`). `C-h l`: Display a description of the last 100 characters you typed (`view-lossage`). `C-h m`: Display documentation of the current major mode (`describe-mode`). `C-h n`: Display documentation of Emacs changes, most recent first (`view-emacs-news`). `C-h P`: Display info on known problems with Emacs and possible workarounds (`view-emacs-problems`). `C-h p`: Find packages by topic keyword (`finder-by-keyword`). `C-h s`: Display the current contents of the syntax table, plus an explanation of what they mean (`describe-syntax`). See section [AD.6 The Syntax Table](emacs.html#SEC504). `C-h t`: Enter the Emacs interactive tutorial (`help-with-tutorial`). `C-h v var RET`: Display the documentation of the Lisp variable var (`describe-variable`). `C-h w command RET`: Show which keys run the command named command (`where-is`). `C-h C coding RET`: Describe coding system coding (`describe-coding-system`). `C-h C RET`: Describe the coding systems currently in use. `C-h I method RET`: Describe an input method (`describe-input-method`). `C-h L language-env RET`: Display information on the character sets, coding systems, and input methods used for language environment language-env (`describe-language-environment`). `C-h C-c`: Display the copying conditions for GNU Emacs. `C-h C-d`: Display information about getting new versions of GNU Emacs. `C-h C-f function RET`: Enter Info and go to the node documenting the Emacs function function (`Info-goto-emacs-command-node`). `C-h C-k key`: Enter Info and go to the node where the key sequence key is documented (`Info-goto-emacs-key-command-node`). `C-h C-p`: Display information about the GNU Project. `C-h TAB symbol RET`: Display the Info documentation on symbol symbol according to the programming language you are editing (`info-lookup-symbol`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC43) ] | [ [>](emacs.html#SEC45) ] | [ [<<](emacs.html#SEC45) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.2 Documentation for a Key

The most basic `C-h` options are `C-h c`
(`describe-key-briefly`) and `C-h k` (`describe-key`).
`C-h c key` displays in the echo area the name of the command
that key is bound to. For example, `C-h c C-f` displays
`` `forward-char' ``. Since command names are chosen to describe what
the commands do, this is a good way to get a very brief description of
what key does.

`C-h k key` is similar but gives more information: it
displays the documentation string of the command as well as its name.
This is too big for the echo area, so a window is used for the display.

`C-h c` and `C-h k` work for any sort of key sequences,
including function keys and mouse events.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC44) ] | [ [>](emacs.html#SEC46) ] | [ [<<](emacs.html#SEC46) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.3 Help by Command or Variable Name

`C-h f` (`describe-function`) reads the name of a Lisp function
using the minibuffer, then displays that function's documentation string
in a window. Since commands are Lisp functions, you can use this to get
the documentation of a command that you know by name. For example,

|  |  |
| --- | --- |
|  | ``` C-h f auto-fill-mode RET ``` |

displays the documentation of `auto-fill-mode`. This is the only
way to get the documentation of a command that is not bound to any key
(one which you would normally run using `M-x`).

`C-h f` is also useful for Lisp functions that you are planning to
use in a Lisp program. For example, if you have just written the
expression `(make-vector len)` and want to check that you are using
`make-vector` properly, type `C-h f make-vector RET`.
Because `C-h f` allows all function names, not just command names,
you may find that some of your favorite abbreviations that work in
`M-x` don't work in `C-h f`. An abbreviation may be unique
among command names yet fail to be unique when other function names are
allowed.

The default function name for `C-h f` to describe, if you type
just `RET`, is the name of the function called by the innermost Lisp
expression in the buffer around point, *provided* that is a valid,
defined Lisp function name. For example, if point is located following
the text `` `(make-vector (car x)' ``, the innermost list containing
point is the one that starts with `` `(make-vector' ``, so the default is
to describe the function `make-vector`.

`C-h f` is often useful just to verify that you have the right
spelling for the function name. If `C-h f` mentions a name from the
buffer as the default, that name must be defined as a Lisp function. If
that is all you want to know, just type `C-g` to cancel the `C-h
f` command, then go on editing.

`C-h w command RET` tells you what keys are bound to
command. It displays a list of the keys in the echo area. If it
says the command is not on any key, you must use `M-x` to run it.
`C-h w` runs the command `where-is`.

`C-h v` (`describe-variable`) is like `C-h f` but describes
Lisp variables instead of Lisp functions. Its default is the Lisp symbol
around or before point, but only if that is the name of a known Lisp
variable. See section [AD.2 Variables](emacs.html#SEC477).

Help buffers describing variables or functions defined in Lisp
normally have hyperlinks to the Lisp definition, if you have the Lisp
source files installed. If you know Lisp, this provides the ultimate
documentation. If you don't know Lisp, you should learn it. If you
are treating Emacs as an object file, then you are just *using*
Emacs. For real intimacy with Emacs, you must read the source code.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC45) ] | [ [>](emacs.html#SEC47) ] | [ [<<](emacs.html#SEC47) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.4 Apropos

A more sophisticated sort of question to ask is, "What are the
commands for working with files?" To ask this question, type `C-h
a file RET`, which displays a list of all command names that
contain `` `file' ``, including `copy-file`, `find-file`, and
so on. With each command name appears a brief description of how to use
the command, and what keys you can currently invoke it with. For
example, it would say that you can invoke `find-file` by typing
`C-x C-f`. The `a` in `C-h a` stands for "Apropos";
`C-h a` runs the command `apropos-command`. This command
normally checks only commands (interactive functions); if you specify a
prefix argument, it checks noninteractive functions as well.

Because `C-h a` looks only for functions whose names contain the
string you specify, you must use ingenuity in choosing the
string. If you are looking for commands for killing backwards and
`C-h a kill-backwards RET` doesn't reveal any, don't give up.
Try just `kill`, or just `backwards`, or just `back`. Be
persistent. Also note that you can use a regular expression as the
argument, for more flexibility (see section [K.5 Syntax of Regular Expressions](emacs.html#SEC97)).

Here is a set of arguments to give to `C-h a` that covers many
classes of Emacs commands, since there are strong conventions for naming
the standard Emacs commands. By giving you a feel for the naming
conventions, this set should also serve to aid you in developing a
technique for picking `apropos` strings.

> char, line, word, sentence, paragraph, region, page, sexp, list, defun,
> rect, buffer, frame, window, face, file, dir, register, mode, beginning, end,
> forward, backward, next, previous, up, down, search, goto, kill, delete,
> mark, insert, yank, fill, indent, case, change, set, what, list, find,
> view, describe, default.

To list all user variables that match a regexp, use the command
`M-x apropos-variable`. This command shows only user variables and
customization options by default; if you specify a prefix argument, it
checks all variables.

To list all Lisp symbols that contain a match for a regexp, not just
the ones that are defined as commands, use the command `M-x apropos`
instead of `C-h a`. This command does not check key bindings by
default; specify a numeric argument if you want it to check them.

The `apropos-documentation` command is like `apropos` except
that it searches documentation strings as well as symbol names for
matches for the specified regular expression.

The `apropos-value` command is like `apropos` except that it
searches symbols' values for matches for the specified regular
expression. This command does not check function definitions or
property lists by default; specify a numeric argument if you want it to
check them.

If the variable `apropos-do-all` is non-`nil`, the commands
above all behave as if they had been given a prefix argument.

If you want more information about a function definition, variable or
symbol property listed in the Apropos buffer, you can click on it with
`Mouse-2` or move there and type `RET`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC46) ] | [ [>](emacs.html#SEC48) ] | [ [<<](emacs.html#SEC48) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.5 Keyword Search for Lisp Libraries

The `C-h p` command lets you search the standard Emacs Lisp
libraries by topic keywords. Here is a partial list of keywords you can
use:

|  |  |
| --- | --- |
|  | ``` abbrev --- abbreviation handling, typing shortcuts, macros. bib --- support for the bibliography processor bib. c --- C and C++ language support. calendar --- calendar and time management support. comm --- communications, networking, remote access to files. data --- support for editing files of data. docs --- support for Emacs documentation. emulations --- emulations of other editors. extensions --- Emacs Lisp language extensions. faces --- support for using faces (fonts and colors; see section J.1 Using Multiple Typefaces). frames --- support for Emacs frames and window systems. games --- games, jokes and amusements. hardware --- support for interfacing with exotic hardware. help --- support for on-line help systems. hypermedia --- support for links within text, or other media types. i18n --- internationalization and alternate character-set support. internal --- code for Emacs internals, build process, defaults. languages --- specialized modes for editing programming languages. lisp --- support for using Lisp (including Emacs Lisp). local --- libraries local to your site. maint --- maintenance aids for the Emacs development group. mail --- modes for electronic-mail handling. matching --- searching and matching. news --- support for netnews reading and posting. non-text --- support for editing files that are not ordinary text. oop --- support for object-oriented programming. outlines --- hierarchical outlining. processes --- process, subshell, compilation, and job control support. terminals --- support for terminal types. tex --- support for the TeX formatter. tools --- programming tools. unix --- front-ends/assistants for, or emulators of, system features. vms --- support code for VMS. wp --- word processing. ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC47) ] | [ [>](emacs.html#SEC49) ] | [ [<<](emacs.html#SEC49) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.6 Help for International Language Support

You can use the command `C-h L`
(`describe-language-environment`) to find out information about
the support for a specific language environment. See section [Q.3 Language Environments](emacs.html#SEC216). This tells you which languages this language
environment is useful for, and lists the character sets, coding
systems, and input methods that go with it. It also shows some sample
text to illustrate scripts.

The command `C-h h` (`view-hello-file`) displays the file
`etc/HELLO', which shows how to say "hello" in many languages.

The command `C-h I` (`describe-input-method`) describes
information about input methods--either a specified input method, or by
default the input method in use. See section [Q.4 Input Methods](emacs.html#SEC217).

The command `C-h C` (`describe-coding-system`) describes
information about coding systems--either a specified coding system, or
the ones currently in use. See section [Q.7 Coding Systems](emacs.html#SEC220).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC48) ] | [ [>](emacs.html#SEC50) ] | [ [<<](emacs.html#SEC50) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.7 Help Mode Commands

Help buffers provide the same commands as View mode (see section [M.10 Miscellaneous File Operations](emacs.html#SEC165)), plus a few special commands of their own.

`SPC`: Scroll forward. `DEL`: `BS`: Scroll backward. On some keyboards, this key is known as `BS` or `backspace`. `RET`: Follow a cross reference at point. `TAB`: Move point forward to the next cross reference. `S-TAB`: Move point back to the previous cross reference. `Mouse-2`: Follow a cross reference that you click on.

When a command name (see section [Running Commands by Name](emacs.html#SEC41)) or
variable name (see section [AD.2 Variables](emacs.html#SEC477)) appears in the documentation, it
normally appears inside paired single-quotes. You can click on the name
with `Mouse-2`, or move point there and type `RET`, to view the
documentation of that command or variable. Use `C-c C-b` to retrace
your steps.

There are convenient commands for moving point to cross references in
the help text. `TAB` (`help-next-ref`) moves point down to the
next cross reference. Use `S-TAB` to move point up to the
previous cross reference (`help-previous-ref`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC49) ] | [ [>](emacs.html#SEC51) ] | [ [<<](emacs.html#SEC51) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.8 Other Help Commands

`C-h i` (`info`) runs the Info program, which is used for
browsing through structured documentation files. The entire Emacs manual
is available within Info. Eventually all the documentation of the GNU
system will be available. Type `h` after entering Info to run
a tutorial on using Info.

If you specify a numeric argument, `C-h i` prompts for the name of
a documentation file. This way, you can browse a file which doesn't
have an entry in the top-level Info menu. It is also handy when you
need to get to the documentation quickly, and you know the exact name of
the file.

There are two special help commands for accessing Emacs documentation
through Info. `C-h C-f function RET` enters Info and
goes straight to the documentation of the Emacs function
function. `C-h C-k key` enters Info and goes straight
to the documentation of the key key. These two keys run the
commands `Info-goto-emacs-command-node` and
`Info-goto-emacs-key-command-node`. You can use `C-h C-k` to
find the documentation of a menu item: just select that menu item when
`C-h C-k` prompts for a key.

`C-h C-f` and `C-h C-k` know about commands and keys
described in manuals other than the Emacs manual. Thus, they make it
easier to find the documentation of commands and keys when you are not
sure which manual describes them, like when using some specialized
mode.

When editing a program, if you have an Info version of the manual for
the programming language, you can use the command `C-h C-i` to refer
to the manual documentation for a symbol (keyword, function or
variable). The details of how this command works depend on the major
mode.

If something surprising happens, and you are not sure what commands you
typed, use `C-h l` (`view-lossage`). `C-h l` displays the last
100 command characters you typed in. If you see commands that you don't
know, you can use `C-h c` to find out what they do.

Emacs has numerous major modes, each of which redefines a few keys and
makes a few other changes in how editing works. `C-h m`
(`describe-mode`) displays documentation on the current major mode,
which normally describes all the commands that are changed in this
mode.

`C-h b` (`describe-bindings`) and `C-h s`
(`describe-syntax`) present other information about the current
Emacs mode. `C-h b` displays a list of all the key bindings now in
effect, showing the local bindings defined by the current minor modes first,
then the local bindings defined by the current major mode, and finally
the global bindings (see section [AD.4 Customizing Key Bindings](emacs.html#SEC491)). `C-h s` displays the
contents of the syntax table, with explanations of each character's
syntax (see section [AD.6 The Syntax Table](emacs.html#SEC504)).

You can get a similar list for a particular prefix key by typing
`C-h` after the prefix key. (There are a few prefix keys for which
this does not work--those that provide their own bindings for
`C-h`. One of these is `ESC`, because `ESC C-h` is
actually `C-M-h`, which marks a defun.)

The other `C-h` options display various files containing useful
information. `C-h C-w` displays the full details on the complete
absence of warranty for GNU Emacs. `C-h n` (`view-emacs-news`)
displays the file `emacs/etc/NEWS', which contains documentation on
Emacs changes arranged chronologically. `C-h F`
(`view-emacs-FAQ`) displays the Emacs frequently-answered-questions
list. `C-h t` (`help-with-tutorial`) displays the
learn-by-doing Emacs tutorial. `C-h C-c` (`describe-copying`)
displays the file `emacs/etc/COPYING', which tells you the
conditions you must obey in distributing copies of Emacs. `C-h C-d`
(`describe-distribution`) displays the file
`emacs/etc/DISTRIB', which tells you how you can order a copy of
the latest version of Emacs. `C-h C-p` (`describe-project`)
displays general information about the GNU Project. `C-h P`
(`view-emacs-problems`) displays the file
`emacs/etc/PROBLEMS', which lists known problems with Emacs in
various situations with solutions or workarounds in many cases.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC50) ] | [ [>](emacs.html#SEC52) ] | [ [<<](emacs.html#SEC42) ] | [ [Up](emacs.html#SEC42) ] | [ [>>](emacs.html#SEC52) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## G.9 Help on Active Text and Tooltips

When a region of text is "active," so that you can select it with
the mouse or a key like `RET`, it often has associated help text.
Areas of the mode line are examples. This help will normally be
shown in the echo area when you move point into the active text. In
a window system you can display the help text as a "tooltip"
(sometimes known as "balloon help"). See section [P.18 Tooltips (or "Balloon Help")](emacs.html#SEC209).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC51) ] | [ [>](emacs.html#SEC53) ] | [ [<<](emacs.html#SEC42) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC59) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# H. The Mark and the Region

Many Emacs commands operate on an arbitrary contiguous part of the
current buffer. To specify the text for such a command to operate on,
you set *the mark* at one end of it, and move point to the other
end. The text between point and the mark is called *the region*.
Emacs highlights the region whenever there is one, if you enable
Transient Mark mode (see section [H.2 Transient Mark Mode](emacs.html#SEC54)).

Certain Emacs commands set the mark; other editing commands do not
affect it, so the mark remains where you set it last. Each Emacs
buffer has its own mark, and setting the mark in one buffer has no
effect on other buffers' marks. When you return to a buffer that was
current earlier, its mark is at the same place as before.

The ends of the region are always point and the mark. It doesn't
matter which of them was put in its current place first, or which one
comes earlier in the text--the region starts from point or the mark
(whichever comes first), and ends at point or the mark (whichever
comes last). Every time you move point, or set the mark in a new
place, the region changes.

Many commands that insert text, such as `C-y` (`yank`) and
`M-x insert-buffer`, position point and the mark at opposite ends
of the inserted text, so that the region consists of the text just
inserted.

Aside from delimiting the region, the mark is also useful for
remembering a spot that you may want to go back to. To make this
feature more useful, each buffer remembers 16 previous locations of the
mark in the *mark ring*.

> |  |  |  |
> | --- | --- | --- |
> | [H.1 Setting the Mark](emacs.html#SEC53) |  | Commands to set the mark. |
> | [H.2 Transient Mark Mode](emacs.html#SEC54) |  | How to make Emacs highlight the region-- when there is one. |
> | [H.3 Operating on the Region](emacs.html#SEC55) |  | Summary of ways to operate on contents of the region. |
> | [H.4 Commands to Mark Textual Objects](emacs.html#SEC56) |  | Commands to put region around textual units. |
> | [H.5 The Mark Ring](emacs.html#SEC57) |  | Previous mark positions saved so you can go back there. |
> | [H.6 The Global Mark Ring](emacs.html#SEC58) |  | Previous mark positions in various buffers. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC52) ] | [ [>](emacs.html#SEC54) ] | [ [<<](emacs.html#SEC52) ] | [ [Up](emacs.html#SEC52) ] | [ [>>](emacs.html#SEC59) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.1 Setting the Mark

Here are some commands for setting the mark:

`C-SPC`: Set the mark where point is (`set-mark-command`). `C-@`: The same. `C-x C-x`: Interchange mark and point (`exchange-point-and-mark`). `Drag-Mouse-1`: Set point and the mark around the text you drag across. `Mouse-3`: Set the mark where point is, then move point to where you click (`mouse-save-then-kill`).

For example, suppose you wish to convert part of the buffer to
upper case, using the `C-x C-u` (`upcase-region`) command,
which operates on the text in the region. You can first go to the
beginning of the text to be capitalized, type `C-SPC` to put
the mark there, move to the end, and then type `C-x C-u`. Or, you
can set the mark at the end of the text, move to the beginning, and then
type `C-x C-u`.

The most common way to set the mark is with the `C-SPC` command
(`set-mark-command`). This sets the mark where point is. Then you
can move point away, leaving the mark behind.

There are two ways to set the mark with the mouse. You can drag mouse
button one across a range of text; that puts point where you release the
mouse button, and sets the mark at the other end of that range. Or you
can click mouse button three, which sets the mark at point (like
`C-SPC`) and then moves point (like `Mouse-1`). Both of
these methods copy the region into the kill ring in addition to setting
the mark; that gives behavior consistent with other window-driven
applications, but if you don't want to modify the kill ring, you must
use keyboard commands to set the mark. See section [P.1 Mouse Commands for Editing](emacs.html#SEC192).

Ordinary terminals have only one cursor, so there is no way for Emacs
to show you where the mark is located. You have to remember. The usual
solution to this problem is to set the mark and then use it soon, before
you forget where it is. Alternatively, you can see where the mark is
with the command `C-x C-x` (`exchange-point-and-mark`) which
puts the mark where point was and point where the mark was. The extent
of the region is unchanged, but the cursor and point are now at the
previous position of the mark. In Transient Mark mode, this command
reactivates the mark.

`C-x C-x` is also useful when you are satisfied with the position
of point but want to move the other end of the region (where the mark
is); do `C-x C-x` to put point at that end of the region, and then
move it. Using `C-x C-x` a second time, if necessary, puts the mark at
the new position with point back at its original position.

For more facilities that allow you to go to previously set marks, see
[H.5 The Mark Ring](emacs.html#SEC57).

There is no such character as `C-SPC` in ASCII; when you
type `SPC` while holding down `CTRL`, what you get on most
ordinary terminals is the character `C-@`. This key is actually
bound to `set-mark-command`. But unless you are unlucky enough to
have a terminal where typing `C-SPC` does not produce
`C-@`, you might as well think of this character as
`C-SPC`. Under X, `C-SPC` is actually a distinct
character, but its binding is still `set-mark-command`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC53) ] | [ [>](emacs.html#SEC55) ] | [ [<<](emacs.html#SEC55) ] | [ [Up](emacs.html#SEC52) ] | [ [>>](emacs.html#SEC59) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.2 Transient Mark Mode

On a terminal that supports colors, Emacs can highlight the current
region. But normally it does not. Why not?

Highlighting the region whenever it exists would not be desirable in
Emacs, because once you have set a mark, there is *always* a
region (in that buffer). And highlighting the region all the time
would be a nuisance. So normally Emacs highlights the region only
immediately after you have selected one with the mouse.

You can turn on region highlighting by enabling Transient Mark mode.
This is a more rigid mode of operation in which the region "lasts"
only temporarily, so you must set up a region for each command that uses
one. In Transient Mark mode, most of the time there is no region;
therefore, highlighting the region when it exists is useful and
not annoying.

To enable Transient Mark mode, type `M-x transient-mark-mode`.
This command toggles the mode, so you can repeat the command to turn off
the mode.

Here are the details of Transient Mark mode:

* To set the mark, type `C-SPC` (`set-mark-command`).
  This makes the mark active and thus begins highlighting of the region.
  As you move point, you will see the highlighted region grow and
  shrink.

  * The mouse commands for specifying the mark also make it active. So do
    keyboard commands whose purpose is to specify a region, including
    `M-@`, `C-M-@`, `M-h`, `C-M-h`, `C-x C-p`, and
    `C-x h`.

    * When the mark is active, you can execute commands that operate on the
      region, such as killing, indenting, or writing to a file.

      * Any change to the buffer, such as inserting or deleting a character,
        deactivates the mark. This means any subsequent command that operates
        on a region will get an error and refuse to operate. You can make the
        region active again by typing `C-x C-x`.

        * Commands like `M->` and `C-s`, that "leave the mark behind" in
          addition to some other primary purpose, do not activate the new mark.
          You can activate the new region by executing `C-x C-x`
          (`exchange-point-and-mark`).

          * `C-s` when the mark is active does not alter the mark.

            * Quitting with `C-g` deactivates the mark.

              * Some commands operate on the region whenever it is active. For
                instance, `C-x u` in Transient Mark mode operates on the region
                when there is a region. Outside Transient Mark mode, you must type
                `C-u C-x u` if you want it to operate on the region.
                See section [D.4 Undoing Changes](emacs.html#SEC23). Other commands that act this way are identified
                in their own documentation.

The highlighting of the region uses the `region` face; you can
customize the appearance of the highlighted region by changing this
face. See section [AD.2.2.3 Customizing Faces](emacs.html#SEC482).

When multiple windows show the same buffer, they can have different
regions, because they can have different values of point (though they
all share one common mark position). Ordinarily, only the selected
window highlights its region (see section [O. Multiple Windows](emacs.html#SEC183)). However, if the
variable `highlight-nonselected-windows` is non-`nil`, then
each window highlights its own region (provided that Transient Mark mode
is enabled and the mark in the window's buffer is active).

When Transient Mark mode is not enabled, every command that sets the
mark also activates it, and nothing ever deactivates it.

If the variable `mark-even-if-inactive` is non-`nil` in
Transient Mark mode, then commands can use the mark and the region
even when it is inactive. Region highlighting appears and disappears
just as it normally does in Transient Mark mode, but the mark doesn't
really go away when the highlighting disappears.

Transient Mark mode is also sometimes known as "Zmacs mode"
because the Zmacs editor on the MIT Lisp Machine handled the mark in a
similar way.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC54) ] | [ [>](emacs.html#SEC56) ] | [ [<<](emacs.html#SEC56) ] | [ [Up](emacs.html#SEC52) ] | [ [>>](emacs.html#SEC59) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.3 Operating on the Region

Once you have a region and the mark is active, here are some of the
ways you can operate on the region:

* Kill it with `C-w` (see section [H.7 Deletion and Killing](emacs.html#SEC59)).* Save it in a register with `C-x r s` (see section [I. Registers](emacs.html#SEC69)).* Save it in a buffer or a file (see section [H.9 Accumulating Text](emacs.html#SEC67)).* Convert case with `C-x C-l` or `C-x C-u` (see section [T.6 Case Conversion Commands](emacs.html#SEC244)).* Indent it with `C-x TAB` or `C-M-\` (see section [S. Indentation](emacs.html#SEC229)).* Fill it as text with `M-x fill-region` (see section [T.5 Filling Text](emacs.html#SEC238)).* Print hardcopy with `M-x print-region` (see section [AC.18 Hardcopy Output](emacs.html#SEC458)).* Evaluate it as Lisp code with `M-x eval-region` (see section [V.8 Evaluating Emacs-Lisp Expressions](emacs.html#SEC329)).

Most commands that operate on the text in the region have the word
`region` in their names.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC55) ] | [ [>](emacs.html#SEC57) ] | [ [<<](emacs.html#SEC57) ] | [ [Up](emacs.html#SEC52) ] | [ [>>](emacs.html#SEC59) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.4 Commands to Mark Textual Objects

Here are the commands for placing point and the mark around a textual
object such as a word, list, paragraph or page.

`M-@`: Set mark after end of next word (`mark-word`). This command and the following one do not move point. `C-M-@`: Set mark after end of following balanced expression (`mark-sexp`). `M-h`: Put region around current paragraph (`mark-paragraph`). `C-M-h`: Put region around current defun (`mark-defun`). `C-x h`: Put region around the entire buffer (`mark-whole-buffer`). `C-x C-p`: Put region around current page (`mark-page`).

`M-@` (`mark-word`) puts the mark at the end of the next
word, while `C-M-@` (`mark-sexp`) puts it at the end of the
next balanced expression (see section [U.4.1 Expressions with Balanced Parentheses](emacs.html#SEC282)). These commands handle
arguments just like `M-f` and `C-M-f`.

Other commands set both point and mark, to delimit an object in the
buffer. For example, `M-h` (`mark-paragraph`) moves point to
the beginning of the paragraph that surrounds or follows point, and puts
the mark at the end of that paragraph (see section [T.3 Paragraphs](emacs.html#SEC236)). It prepares
the region so you can indent, case-convert, or kill a whole paragraph.

`C-M-h` (`mark-defun`) similarly puts point before, and the
mark after, the current (or following) major top-level definition, or
defun (see section [U.2.2 Moving by Defuns](emacs.html#SEC272)). `C-x C-p` (`mark-page`)
puts point before the current page, and mark at the end
(see section [T.4 Pages](emacs.html#SEC237)). The mark goes after the terminating page delimiter
(to include it in the region), while point goes after the preceding
page delimiter (to exclude it). A numeric argument specifies a later
page (if positive) or an earlier page (if negative) instead of the
current page.

Finally, `C-x h` (`mark-whole-buffer`) sets up the entire
buffer as the region, by putting point at the beginning and the mark at
the end.

In Transient Mark mode, all of these commands activate the mark.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC56) ] | [ [>](emacs.html#SEC58) ] | [ [<<](emacs.html#SEC58) ] | [ [Up](emacs.html#SEC52) ] | [ [>>](emacs.html#SEC59) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.5 The Mark Ring

Aside from delimiting the region, the mark is also useful for
remembering a spot that you may want to go back to. To make this
feature more useful, each buffer remembers 16 previous locations of the
mark, in the *mark ring*. Commands that set the mark also push the
old mark onto this ring. To return to a marked location, use `C-u
C-SPC` (or `C-u C-@`); this is the command
`set-mark-command` given a numeric argument. It moves point to
where the mark was, and restores the mark from the ring of former
marks. Thus, repeated use of this command moves point to all of the old
marks on the ring, one by one. The mark positions you move through in
this way are not lost; they go to the end of the ring.

Each buffer has its own mark ring. All editing commands use the current
buffer's mark ring. In particular, `C-u C-SPC` always stays in
the same buffer.

Many commands that can move long distances, such as `M-<`
(`beginning-of-buffer`), start by setting the mark and saving the
old mark on the mark ring. This is to make it easier for you to move
back later. Searches set the mark if they move point. You can tell
when a command sets the mark because it displays `` `Mark set' `` in the
echo area.

If you want to move back to the same place over and over, the mark
ring may not be convenient enough. If so, you can record the position
in a register for later retrieval (see section [Saving Positions in Registers](emacs.html#SEC70)).

The variable `mark-ring-max` specifies the maximum number of
entries to keep in the mark ring. If that many entries exist and
another one is pushed, the earliest one in the list is discarded. Repeating
`C-u C-SPC` cycles through the positions currently in the
ring.

The variable `mark-ring` holds the mark ring itself, as a list of
marker objects, with the most recent first. This variable is local in
every buffer.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC57) ] | [ [>](emacs.html#SEC59) ] | [ [<<](emacs.html#SEC59) ] | [ [Up](emacs.html#SEC52) ] | [ [>>](emacs.html#SEC59) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.6 The Global Mark Ring

In addition to the ordinary mark ring that belongs to each buffer,
Emacs has a single *global mark ring*. It records a sequence of
buffers in which you have recently set the mark, so you can go back
to those buffers.

Setting the mark always makes an entry on the current buffer's mark
ring. If you have switched buffers since the previous mark setting, the
new mark position makes an entry on the global mark ring also. The
result is that the global mark ring records a sequence of buffers that
you have been in, and, for each buffer, a place where you set the mark.

The command `C-x C-SPC` (`pop-global-mark`) jumps to
the buffer and position of the latest entry in the global ring. It also
rotates the ring, so that successive uses of `C-x C-SPC` take
you to earlier and earlier buffers.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC58) ] | [ [>](emacs.html#SEC60) ] | [ [<<](emacs.html#SEC52) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC63) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.7 Deletion and Killing

Most commands which erase text from the buffer save it in the kill
ring so that you can move or copy it to other parts of the buffer.
These commands are known as *kill* commands. The rest of the
commands that erase text do not save it in the kill ring; they are known
as *delete* commands. (This distinction is made only for erasure of
text in the buffer.) If you do a kill or delete command by mistake, you
can use the `C-x u` (`undo`) command to undo it
(see section [D.4 Undoing Changes](emacs.html#SEC23)).

You cannot kill read-only text, since such text does not allow any
kind of modification. But some users like to use the kill commands to
copy read-only text into the kill ring, without actually changing it.
If you set the variable `kill-read-only-ok` to a non-`nil`
value, the kill commands work specially in a read-only buffer: they
move over text, and copy it to the kill ring, without actually
deleting it from the buffer. When this happens, a message in the echo
area tells you what is happening.

The delete commands include `C-d` (`delete-char`) and
`DEL` (`delete-backward-char`), which delete only one
character at a time, and those commands that delete only spaces or
newlines. Commands that can destroy significant amounts of nontrivial
data generally do a kill operation instead. The commands' names and
individual descriptions use the words `` `kill' `` and `` `delete' `` to
say which kind of operation they perform.

Many window systems follow the convention that insertion while text
is selected deletes the selected text. You can make Emacs behave this
way by enabling Delete Selection mode, with `M-x
delete-selection-mode`, or using Custom. Another effect of this mode
is that `DEL`, `C-d` and some other keys, when a selection
exists, will kill the whole selection. It also enables Transient Mark
mode (see section [H.2 Transient Mark Mode](emacs.html#SEC54)).

> |  |  |  |
> | --- | --- | --- |
> | [H.7.1 Deletion](emacs.html#SEC60) |  | Commands for deleting small amounts of text and blank areas. |
> | [H.7.2 Killing by Lines](emacs.html#SEC61) |  | How to kill entire lines of text at one time. |
> | [H.7.3 Other Kill Commands](emacs.html#SEC62) |  | Commands to kill large regions of text and syntactic units such as words and sentences. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC59) ] | [ [>](emacs.html#SEC61) ] | [ [<<](emacs.html#SEC59) ] | [ [Up](emacs.html#SEC59) ] | [ [>>](emacs.html#SEC63) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### H.7.1 Deletion

Deletion means erasing text and not saving it in the kill ring. For
the most part, the Emacs commands that delete text are those that
erase just one character or only whitespace.

`C-d`: `Delete`: Delete next character (`delete-char`). If your keyboard has a `Delete` function key (usually located in the edit keypad), Emacs binds it to `delete-char` as well. `DEL`: `BS`: Delete previous character (`delete-backward-char`). Some keyboards refer to this key as a "backspace key" and label it with a left arrow. `M-\`: Delete spaces and tabs around point (`delete-horizontal-space`). `M-SPC`: Delete spaces and tabs around point, leaving one space (`just-one-space`). `C-x C-o`: Delete blank lines around the current line (`delete-blank-lines`). `M-^`: Join two lines by deleting the intervening newline, along with any indentation following it (`delete-indentation`).

The most basic delete commands are `C-d` (`delete-char`) and
`DEL` (`delete-backward-char`). `C-d` deletes the
character after point, the one the cursor is "on top of." This
doesn't move point. `DEL` deletes the character before the cursor,
and moves point back. You can delete newlines like any other characters
in the buffer; deleting a newline joins two lines. Actually, `C-d`
and `DEL` aren't always delete commands; when given arguments, they
kill instead, since they can erase more than one character this way.

Every keyboard has a large key, labeled `DEL`, `BACKSPACE`,
`BS` or `DELETE`, which is a short distance above the
`RET` or `ENTER` key and is normally used for erasing what you
have typed. Regardless of the actual name on the key, in Emacs it is
equivalent to `DEL`---or it should be.

Many keyboards (including standard PC keyboards) have a
`BACKSPACE` key a short ways above `RET` or `ENTER`, and a
`DELETE` key elsewhere. In that case, the `BACKSPACE` key is
`DEL`, and the `DELETE` key is equivalent to `C-d`---or it
should be.

Why do we say "or it should be"? When Emacs starts up using a
window system, it determines automatically which key or keys should be
equivalent to `DEL`. As a result, `BACKSPACE` and/or `DELETE`
keys normally do the right things. But in some unusual cases Emacs
gets the wrong information from the system. If these keys don't do
what they ought to do, you need to tell Emacs which key to use for
`DEL`. See section [AD.9.1 If `DEL` Fails to Delete](emacs.html#SEC512), for how to do this.

On most text-only terminals, Emacs cannot tell which keys the
keyboard really has, so it follows a uniform plan which may or may not
fit your keyboard. The uniform plan is that the ASCII `DEL`
character deletes, and the ASCII `BS` (backspace) character asks
for help (it is the same as `C-h`). If this is not right for your
keyboard, such as if you find that the key which ought to delete backwards
enters Help instead, see [AD.9.1 If `DEL` Fails to Delete](emacs.html#SEC512).

The other delete commands are those which delete only whitespace
characters: spaces, tabs and newlines. `M-\`
(`delete-horizontal-space`) deletes all the spaces and tab
characters before and after point. `M-SPC`
(`just-one-space`) does likewise but leaves a single space after
point, regardless of the number of spaces that existed previously (even
if there were none before).

`C-x C-o` (`delete-blank-lines`) deletes all blank lines
after the current line. If the current line is blank, it deletes all
blank lines preceding the current line as well (leaving one blank line,
the current line).

`M-^` (`delete-indentation`) joins the current line and the
previous line, by deleting a newline and all surrounding spaces, usually
leaving a single space. See section [M-^](emacs.html#SEC229).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC60) ] | [ [>](emacs.html#SEC62) ] | [ [<<](emacs.html#SEC62) ] | [ [Up](emacs.html#SEC59) ] | [ [>>](emacs.html#SEC63) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### H.7.2 Killing by Lines

`C-k`: Kill rest of line or one or more lines (`kill-line`).

The simplest kill command is `C-k`. If given at the beginning of
a line, it kills all the text on the line, leaving it blank. When used
on a blank line, it kills the whole line including its newline. To kill
an entire non-blank line, go to the beginning and type `C-k` twice.

More generally, `C-k` kills from point up to the end of the line,
unless it is at the end of a line. In that case it kills the newline
following point, thus merging the next line into the current one.
Spaces and tabs that you can't see at the end of the line are ignored
when deciding which case applies, so if point appears to be at the end
of the line, you can be sure `C-k` will kill the newline.

When `C-k` is given a positive argument, it kills that many lines
and the newlines that follow them (however, text on the current line
before point is not killed). With a negative argument -n, it
kills n lines preceding the current line (together with the text
on the current line before point). Thus, `C-u - 2 C-k` at the front
of a line kills the two previous lines.

`C-k` with an argument of zero kills the text before point on the
current line.

If the variable `kill-whole-line` is non-`nil`, `C-k` at
the very beginning of a line kills the entire line including the
following newline. This variable is normally `nil`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC61) ] | [ [>](emacs.html#SEC63) ] | [ [<<](emacs.html#SEC59) ] | [ [Up](emacs.html#SEC59) ] | [ [>>](emacs.html#SEC63) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### H.7.3 Other Kill Commands

`C-w`: Kill region (from point to the mark) (`kill-region`). `M-d`: Kill word (`kill-word`). See section [T.1 Words](emacs.html#SEC234). `M-DEL`: Kill word backwards (`backward-kill-word`). `C-x DEL`: Kill back to beginning of sentence (`backward-kill-sentence`). See section [T.2 Sentences](emacs.html#SEC235). `M-k`: Kill to end of sentence (`kill-sentence`). `C-M-k`: Kill the following balanced expression (`kill-sexp`). See section [U.4.1 Expressions with Balanced Parentheses](emacs.html#SEC282). `M-z char`: Kill through the next occurrence of char (`zap-to-char`).

A kill command which is very general is `C-w`
(`kill-region`), which kills everything between point and the
mark. With this command, you can kill any contiguous sequence of
characters, if you first set the region around them.

A convenient way of killing is combined with searching: `M-z`
(`zap-to-char`) reads a character and kills from point up to (and
including) the next occurrence of that character in the buffer. A
numeric argument acts as a repeat count. A negative argument means to
search backward and kill text before point.

Other syntactic units can be killed: words, with `M-DEL`
and `M-d` (see section [T.1 Words](emacs.html#SEC234)); balanced expressions, with `C-M-k`
(see section [U.4.1 Expressions with Balanced Parentheses](emacs.html#SEC282)); and sentences, with `C-x DEL` and
`M-k` (see section [T.2 Sentences](emacs.html#SEC235)).

You can use kill commands in read-only buffers. They don't actually
change the buffer, and they beep to warn you of that, but they do copy
the text you tried to kill into the kill ring, so you can yank it into
other buffers. Most of the kill commands move point across the text
they copy in this way, so that successive kill commands build up a
single kill ring entry as usual.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC62) ] | [ [>](emacs.html#SEC64) ] | [ [<<](emacs.html#SEC59) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC67) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## H.8 Yanking

*Yanking* means reinserting text previously killed. This is what
some systems call "pasting." The usual way to move or copy text is to
kill it and then yank it elsewhere one or more times.

`C-y`: Yank last killed text (`yank`). `M-y`: Replace text just yanked with an earlier batch of killed text (`yank-pop`). `M-w`: Save region as last killed text without actually killing it (`kill-ring-save`). `C-M-w`: Append next kill to last batch of killed text (`append-next-kill`).

> |  |  |  |
> | --- | --- | --- |
> | [H.8.1 The Kill Ring](emacs.html#SEC64) |  | Where killed text is stored. Basic yanking. |
> | [H.8.2 Appending Kills](emacs.html#SEC65) |  | Several kills in a row all yank together. |
> | [H.8.3 Yanking Earlier Kills](emacs.html#SEC66) |  | Yanking something killed some time ago. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC63) ] | [ [>](emacs.html#SEC65) ] | [ [<<](emacs.html#SEC63) ] | [ [Up](emacs.html#SEC63) ] | [ [>>](emacs.html#SEC67) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### H.8.1 The Kill Ring

All killed text is recorded in the *kill ring*, a list of blocks of
text that have been killed. There is only one kill ring, shared by all
buffers, so you can kill text in one buffer and yank it in another buffer.
This is the usual way to move text from one file to another.
(See section [H.9 Accumulating Text](emacs.html#SEC67), for some other ways.)

The command `C-y` (`yank`) reinserts the text of the most recent
kill. It leaves the cursor at the end of the text. It sets the mark at
the beginning of the text. See section [H. The Mark and the Region](emacs.html#SEC52).

`C-u C-y` leaves the cursor in front of the text, and sets the
mark after it. This happens only if the argument is specified with just
a `C-u`, precisely. Any other sort of argument, including `C-u`
and digits, specifies an earlier kill to yank (see section [H.8.3 Yanking Earlier Kills](emacs.html#SEC66)).

To copy a block of text, you can use `M-w`
(`kill-ring-save`), which copies the region into the kill ring
without removing it from the buffer. This is approximately equivalent
to `C-w` followed by `C-x u`, except that `M-w` does not
alter the undo history and does not temporarily change the screen.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC64) ] | [ [>](emacs.html#SEC66) ] | [ [<<](emacs.html#SEC66) ] | [ [Up](emacs.html#SEC63) ] | [ [>>](emacs.html#SEC67) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### H.8.2 Appending Kills

Normally, each kill command pushes a new entry onto the kill ring.
However, two or more kill commands in a row combine their text into a
single entry, so that a single `C-y` yanks all the text as a unit,
just as it was before it was killed.

Thus, if you want to yank text as a unit, you need not kill all of it
with one command; you can keep killing line after line, or word after
word, until you have killed it all, and you can still get it all back at
once.

Commands that kill forward from point add onto the end of the previous
killed text. Commands that kill backward from point add text onto the
beginning. This way, any sequence of mixed forward and backward kill
commands puts all the killed text into one entry without rearrangement.
Numeric arguments do not break the sequence of appending kills. For
example, suppose the buffer contains this text:

|  |  |
| --- | --- |
|  | ``` This is a line -!-of sample text. ``` |

with point shown by -!-. If you type `M-d M-DEL M-d
M-DEL`, killing alternately forward and backward, you end up with
`` `a line of sample' `` as one entry in the kill ring, and `` `This
is text.' `` in the buffer. (Note the double space between `` `is' ``
and `` `text' ``, which you can clean up with `M-SPC` or
`M-q`.)

Another way to kill the same text is to move back two words with
`M-b M-b`, then kill all four words forward with `C-u M-d`.
This produces exactly the same results in the buffer and in the kill
ring. `M-f M-f C-u M-DEL` kills the same text, all going
backward; once again, the result is the same. The text in the kill ring
entry always has the same order that it had in the buffer before you
killed it.

If a kill command is separated from the last kill command by other
commands (not just numeric arguments), it starts a new entry on the kill
ring. But you can force it to append by first typing the command
`C-M-w` (`append-next-kill`) right before it. The `C-M-w`
tells the following command, if it is a kill command, to append the text
it kills to the last killed text, instead of starting a new entry. With
`C-M-w`, you can kill several separated pieces of text and
accumulate them to be yanked back in one place.

A kill command following `M-w` does not append to the text that
`M-w` copied into the kill ring.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC65) ] | [ [>](emacs.html#SEC67) ] | [ [<<](emacs.html#SEC63) ] | [ [Up](emacs.html#SEC63) ] | [ [>>](emacs.html#SEC67) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
