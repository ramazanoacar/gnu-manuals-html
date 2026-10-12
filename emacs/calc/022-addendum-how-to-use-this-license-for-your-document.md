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

Next: [Reporting Bugs](#Reporting-Bugs), Previous: [GNU Free Documentation License](#GNU-Free-Documentation-License), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## Appendix C Customizing Calc [¶](#Customizing-Calc-1)

The usual prefix for Calc is the key sequence `C-x *`. If you wish
to use a different prefix, you can put

```
(keymap-global-set "NEWPREFIX" 'calc-dispatch)
```

in your .emacs file.
(See [Customizing Key Bindings](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Key-Bindings) in The GNU Emacs Manual, for more information on binding keys.)
A convenient way to start Calc is with `C-x * *`; to make it equally
convenient for users who use a different prefix, the prefix can be
followed by `=`, `&`, `#`, `\`, `/`, `+` or
`-` as well as `*` to start Calc, and so in many cases the last
character of the prefix can simply be typed twice.

Calc is controlled by many variables, most of which can be reset from
within Calc. Some variables are less involved with actual calculation
and can be set outside of Calc using Emacs’s customization facilities.
These variables are listed below. Typing `M-x customize-variable
RET variable-name RET` will bring up a buffer in
which the variable’s value can be redefined. Typing `M-x
customize-group RET calc RET` will bring up a buffer which
contains all of Calc’s customizable variables. (These variables can
also be reset by putting the appropriate lines in your .emacs file;
See [Init File](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-File) in The GNU Emacs Manual.)

Some of the customizable variables are regular expressions. A regular
expression is basically a pattern that Calc can search for.
See [Regular Expression Search](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Regexp-Search) in The GNU Emacs
Manual, to see how regular expressions work.

Variable: **calc-settings-file** [¶](#index-calc_002dsettings_002dfile)
:   The variable `calc-settings-file` holds the file name in
    which commands like `m m` and `Z P` store “permanent”
    definitions.
    If `calc-settings-file` is not your user init file (typically
    `~/.emacs`) and if the variable `calc-loaded-settings-file` is
    `nil`, then Calc will automatically load your settings file (if it
    exists) the first time Calc is invoked.

    The default value for this variable is `"~/.emacs.d/calc.el"`
    unless the file `~/.calc.el` exists, in which case the default
    value will be `"~/.calc.el"`.

Variable: **calc-gnuplot-name** [¶](#index-calc_002dgnuplot_002dname-1)
:   See [Graphics](#Graphics).  
    The variable `calc-gnuplot-name` should be the name of the
    GNUPLOT program (a string). If you have GNUPLOT installed on your
    system but Calc is unable to find it, you may need to set this
    variable. You may also need to set some Lisp variables to show Calc how
    to run GNUPLOT on your system, see [Graphical Devices](#Devices) .
    The default value of `calc-gnuplot-name` is `"gnuplot"`.

Variable: **calc-gnuplot-plot-command** [¶](#index-calc_002dgnuplot_002dplot_002dcommand-1)

Variable: **calc-gnuplot-print-command** [¶](#index-calc_002dgnuplot_002dprint_002dcommand-1)
:   See [Graphical Devices](#Devices).  
    The variables `calc-gnuplot-plot-command` and
    `calc-gnuplot-print-command` represent system commands to
    display and print the output of GNUPLOT, respectively. These may be
    `nil` if no command is necessary, or strings which can include
    ‘`%s`’ to signify the name of the file to be displayed or printed.
    Or, these variables may contain Lisp expressions which are evaluated
    to display or print the output.

    The default value of `calc-gnuplot-plot-command` is `nil`,
    and the default value of `calc-gnuplot-print-command` is
    `"lp %s"`.

Variable: **calc-language-alist** [¶](#index-calc_002dlanguage_002dalist)
:   See [Basic Embedded Mode](#Basic-Embedded-Mode).  
    The variable `calc-language-alist` controls the languages that
    Calc will associate with major modes. When Calc embedded mode is
    enabled, it will try to use the current major mode to
    determine what language should be used. (This can be overridden using
    Calc’s mode changing commands, See [Mode Settings in Embedded Mode](#Mode-Settings-in-Embedded-Mode).)
    The variable `calc-language-alist` consists of a list of pairs of
    the form `(MAJOR-MODE . LANGUAGE)`; for example,
    `(latex-mode . latex)` is one such pair. If Calc embedded is
    activated in a buffer whose major mode is MAJOR-MODE, it will set itself
    to use the language LANGUAGE.

    The default value of `calc-language-alist` is

    ```
       ((latex-mode . latex)
        (tex-mode   . tex)
        (plain-tex-mode . tex)
        (context-mode . tex)
        (nroff-mode . eqn)
        (pascal-mode . pascal)
        (c-mode . c)
        (c++-mode . c)
        (fortran-mode . fortran)
        (f90-mode . fortran))
    ```

Variable: **calc-embedded-announce-formula** [¶](#index-calc_002dembedded_002dannounce_002dformula-1)

Variable: **calc-embedded-announce-formula-alist** [¶](#index-calc_002dembedded_002dannounce_002dformula_002dalist)
:   See [Customizing Embedded Mode](#Customizing-Embedded-Mode).  
    The variable `calc-embedded-announce-formula` helps determine
    what formulas `C-x * a` will activate in a buffer. It is a
    regular expression, and when activating embedded formulas with
    `C-x * a`, it will tell Calc that what follows is a formula to be
    activated. (Calc also uses other patterns to find formulas, such as
    ‘`=>`’ and ‘`:=`’.)

    The default pattern is `"%Embed\n\\(% .*\n\\)*"`, which checks
    for ‘`%Embed`’ followed by any number of lines beginning with
    ‘`%`’ and a space.

    The variable `calc-embedded-announce-formula-alist` is used to
    set `calc-embedded-announce-formula` to different regular
    expressions depending on the major mode of the editing buffer.
    It consists of a list of pairs of the form `(MAJOR-MODE .
    REGEXP)`, and its default value is

    ```
       ((c++-mode     . "//Embed\n\\(// .*\n\\)*")
        (c-mode       . "/\\*Embed\\*/\n\\(/\\* .*\\*/\n\\)*")
        (f90-mode     . "!Embed\n\\(! .*\n\\)*")
        (fortran-mode . "C Embed\n\\(C .*\n\\)*")
        (html-helper-mode . "<!-- Embed -->\n\\(<!-- .* -->\n\\)*")
        (html-mode    . "<!-- Embed -->\n\\(<!-- .* -->\n\\)*")
        (nroff-mode   . "\\\\\"Embed\n\\(\\\\\" .*\n\\)*")
        (pascal-mode  . "{Embed}\n\\({.*}\n\\)*")
        (sgml-mode    . "<!-- Embed -->\n\\(<!-- .* -->\n\\)*")
        (xml-mode     . "<!-- Embed -->\n\\(<!-- .* -->\n\\)*")
        (texinfo-mode . "@c Embed\n\\(@c .*\n\\)*"))
    ```

    Any major modes added to `calc-embedded-announce-formula-alist`
    should also be added to `calc-embedded-open-close-plain-alist`
    and `calc-embedded-open-close-mode-alist`.

Variable: **calc-embedded-open-formula** [¶](#index-calc_002dembedded_002dopen_002dformula-1)

Variable: **calc-embedded-close-formula** [¶](#index-calc_002dembedded_002dclose_002dformula-1)

Variable: **calc-embedded-open-close-formula-alist** [¶](#index-calc_002dembedded_002dopen_002dclose_002dformula_002dalist)
:   See [Customizing Embedded Mode](#Customizing-Embedded-Mode).  
    The variables `calc-embedded-open-formula` and
    `calc-embedded-close-formula` control the region that Calc will
    activate as a formula when Embedded mode is entered with `C-x * e`.
    They are regular expressions;
    Calc normally scans backward and forward in the buffer for the
    nearest text matching these regular expressions to be the “formula
    delimiters”.

    The simplest delimiters are blank lines. Other delimiters that
    Embedded mode understands by default are:

    1. The TeX and LaTeX math delimiters ‘`$ $`’, ‘`$$ $$`’,
       ‘`\[ \]`’, and ‘`\( \)`’;
    2. Lines beginning with ‘`\begin`’ and ‘`\end`’ (except matrix delimiters);
    3. Lines beginning with ‘`@`’ (Texinfo delimiters).
    4. Lines beginning with ‘`.EQ`’ and ‘`.EN`’ (*eqn* delimiters);
    5. Lines containing a single ‘`%`’ or ‘`.\"`’ symbol and nothing else.

    The variable `calc-embedded-open-close-formula-alist` is used to
    set `calc-embedded-open-formula` and
    `calc-embedded-close-formula` to different regular
    expressions depending on the major mode of the editing buffer.
    It consists of a list of lists of the form
    `(MAJOR-MODE OPEN-FORMULA-REGEXP
    CLOSE-FORMULA-REGEXP)`, and its default value is
    `nil`.

Variable: **calc-embedded-word-regexp** [¶](#index-calc_002dembedded_002dword_002dregexp-1)

Variable: **calc-embedded-word-regexp-alist** [¶](#index-calc_002dembedded_002dword_002dregexp_002dalist)
:   See [Customizing Embedded Mode](#Customizing-Embedded-Mode).  
    The variable `calc-embedded-word-regexp` determines the expression
    that Calc will activate when Embedded mode is entered with `C-x *
    w`. It is a regular expressions.

    The default value of `calc-embedded-word-regexp` is
    `"[-+]?[0-9]+\\(\\.[0-9]+\\)?\\([eE][-+]?[0-9]+\\)?"`.

    The variable `calc-embedded-word-regexp-alist` is used to
    set `calc-embedded-word-regexp` to a different regular
    expression depending on the major mode of the editing buffer.
    It consists of a list of lists of the form
    `(MAJOR-MODE WORD-REGEXP)`, and its default value is
    `nil`.

Variable: **calc-embedded-open-plain** [¶](#index-calc_002dembedded_002dopen_002dplain-1)

Variable: **calc-embedded-close-plain** [¶](#index-calc_002dembedded_002dclose_002dplain-1)

Variable: **calc-embedded-open-close-plain-alist** [¶](#index-calc_002dembedded_002dopen_002dclose_002dplain_002dalist)
:   See [Customizing Embedded Mode](#Customizing-Embedded-Mode).  
    The variables `calc-embedded-open-plain` and
    `calc-embedded-open-plain` are used to delimit “plain”
    formulas. Note that these are actual strings, not regular
    expressions, because Calc must be able to write these string into a
    buffer as well as to recognize them.

    The default string for `calc-embedded-open-plain` is
    `"%%% "`, note the trailing space. The default string for
    `calc-embedded-close-plain` is `" %%%\n"`, without
    the trailing newline here, the first line of a Big mode formula
    that followed might be shifted over with respect to the other lines.

    The variable `calc-embedded-open-close-plain-alist` is used to
    set `calc-embedded-open-plain` and
    `calc-embedded-close-plain` to different strings
    depending on the major mode of the editing buffer.
    It consists of a list of lists of the form
    `(MAJOR-MODE OPEN-PLAIN-STRING
    CLOSE-PLAIN-STRING)`, and its default value is

    ```
       ((c++-mode     "// %% "   " %%\n")
        (c-mode       "/* %% "   " %% */\n")
        (f90-mode     "! %% "    " %%\n")
        (fortran-mode "C %% "    " %%\n")
        (html-helper-mode "<!-- %% " " %% -->\n")
        (html-mode "<!-- %% " " %% -->\n")
        (nroff-mode   "\\\" %% " " %%\n")
        (pascal-mode  "{%% "    " %%}\n")
        (sgml-mode     "<!-- %% " " %% -->\n")
        (xml-mode     "<!-- %% " " %% -->\n")
        (texinfo-mode "@c %% "   " %%\n"))
    ```

    Any major modes added to `calc-embedded-open-close-plain-alist`
    should also be added to `calc-embedded-announce-formula-alist`
    and `calc-embedded-open-close-mode-alist`.

Variable: **calc-embedded-open-new-formula** [¶](#index-calc_002dembedded_002dopen_002dnew_002dformula-1)

Variable: **calc-embedded-close-new-formula** [¶](#index-calc_002dembedded_002dclose_002dnew_002dformula-1)

Variable: **calc-embedded-open-close-new-formula-alist** [¶](#index-calc_002dembedded_002dopen_002dclose_002dnew_002dformula_002dalist)
:   See [Customizing Embedded Mode](#Customizing-Embedded-Mode).  
    The variables `calc-embedded-open-new-formula` and
    `calc-embedded-close-new-formula` are strings which are
    inserted before and after a new formula when you type `C-x * f`.

    The default value of `calc-embedded-open-new-formula` is
    `"\n\n"`. If this string begins with a newline character and the
    `C-x * f` is typed at the beginning of a line, `C-x * f` will skip
    this first newline to avoid introducing unnecessary blank lines in the
    file. The default value of `calc-embedded-close-new-formula` is
    also `"\n\n"`. The final newline is omitted by `C-x * f`
    if typed at the end of a line. (It follows that if `C-x * f` is
    typed on a blank line, both a leading opening newline and a trailing
    closing newline are omitted.)

    The variable `calc-embedded-open-close-new-formula-alist` is used to
    set `calc-embedded-open-new-formula` and
    `calc-embedded-close-new-formula` to different strings
    depending on the major mode of the editing buffer.
    It consists of a list of lists of the form
    `(MAJOR-MODE OPEN-NEW-FORMULA-STRING
    CLOSE-NEW-FORMULA-STRING)`, and its default value is
    `nil`.

Variable: **calc-embedded-open-mode** [¶](#index-calc_002dembedded_002dopen_002dmode-1)

Variable: **calc-embedded-close-mode** [¶](#index-calc_002dembedded_002dclose_002dmode-1)

Variable: **calc-embedded-open-close-mode-alist** [¶](#index-calc_002dembedded_002dopen_002dclose_002dmode_002dalist)
:   See [Customizing Embedded Mode](#Customizing-Embedded-Mode).  
    The variables `calc-embedded-open-mode` and
    `calc-embedded-close-mode` are strings which Calc will place before
    and after any mode annotations that it inserts. Calc never scans for
    these strings; Calc always looks for the annotation itself, so it is not
    necessary to add them to user-written annotations.

    The default value of `calc-embedded-open-mode` is `"% "`
    and the default value of `calc-embedded-close-mode` is
    `"\n"`.
    If you change the value of `calc-embedded-close-mode`, it is a good
    idea still to end with a newline so that mode annotations will appear on
    lines by themselves.

    The variable `calc-embedded-open-close-mode-alist` is used to
    set `calc-embedded-open-mode` and
    `calc-embedded-close-mode` to different strings
    expressions depending on the major mode of the editing buffer.
    It consists of a list of lists of the form
    `(MAJOR-MODE OPEN-MODE-STRING
    CLOSE-MODE-STRING)`, and its default value is

    ```
       ((c++-mode     "// "   "\n")
        (c-mode       "/* "   " */\n")
        (f90-mode     "! "    "\n")
        (fortran-mode "C "    "\n")
        (html-helper-mode "<!-- " " -->\n")
        (html-mode    "<!-- " " -->\n")
        (nroff-mode   "\\\" " "\n")
        (pascal-mode  "{ "    " }\n")
        (sgml-mode    "<!-- " " -->\n")
        (xml-mode     "<!-- " " -->\n")
        (texinfo-mode "@c "   "\n"))
    ```

    Any major modes added to `calc-embedded-open-close-mode-alist`
    should also be added to `calc-embedded-announce-formula-alist`
    and `calc-embedded-open-close-plain-alist`.

Variable: **calc-lu-power-reference** [¶](#index-calc_002dlu_002dpower_002dreference-1)

Variable: **calc-lu-field-reference** [¶](#index-calc_002dlu_002dfield_002dreference-1)
:   See [Logarithmic Units](#Logarithmic-Units).  
    The variables `calc-lu-power-reference` and
    `calc-lu-field-reference` are unit expressions (written as
    strings) which Calc will use as reference quantities for logarithmic
    units.

    The default value of `calc-lu-power-reference` is `"mW"`
    and the default value of `calc-lu-field-reference` is
    `"20 uPa"`.

Variable: **calc-note-threshold** [¶](#index-calc_002dnote_002dthreshold)
:   See [Musical Notes](#Musical-Notes).  
    The variable `calc-note-threshold` is a number (written as a
    string) which determines how close (in cents) a frequency needs to be
    to a note to be recognized as that note.

    The default value of `calc-note-threshold` is 1.

Variable: **calc-highlight-selections-with-faces** [¶](#index-calc_002dhighlight_002dselections_002dwith_002dfaces-1)

Variable: **calc-selected-face** [¶](#index-calc_002dselected_002dface-1)

Variable: **calc-nonselected-face** [¶](#index-calc_002dnonselected_002dface-1)
:   See [Displaying Selections](#Displaying-Selections).  
    The variable `calc-highlight-selections-with-faces`
    determines how selected sub-formulas are distinguished.
    If `calc-highlight-selections-with-faces` is `nil`, then
    a selected sub-formula is distinguished either by changing every
    character not part of the sub-formula with a dot or by changing every
    character in the sub-formula with a ‘`#`’ sign.
    If `calc-highlight-selections-with-faces` is `t`,
    then a selected sub-formula is distinguished either by displaying the
    non-selected portion of the formula with `calc-nonselected-face`
    or by displaying the selected sub-formula with
    `calc-nonselected-face`.

Variable: **calc-multiplication-has-precedence** [¶](#index-calc_002dmultiplication_002dhas_002dprecedence)
:   The variable `calc-multiplication-has-precedence` determines
    whether multiplication has precedence over division in algebraic
    formulas in normal language modes. If
    `calc-multiplication-has-precedence` is non-`nil`, then
    multiplication has precedence (and, for certain obscure reasons, is
    right associative), and so for example ‘`a/b*c`’ will be interpreted
    as ‘`a/(b*c)`’. If `calc-multiplication-has-precedence` is
    `nil`, then multiplication has the same precedence as division
    (and, like division, is left associative), and so for example
    ‘`a/b*c`’ will be interpreted as ‘`(a/b)*c`’. The default value
    of `calc-multiplication-has-precedence` is `t`.

Variable: **calc-context-sensitive-enter** [¶](#index-calc_002dcontext_002dsensitive_002denter)
:   The commands `calc-enter` and `calc-pop` will typically
    duplicate the top of the stack. If
    `calc-context-sensitive-enter` is non-`nil`, then the
    `calc-enter` will copy the element at the cursor to the
    top of the stack and `calc-pop` will delete the element at the
    cursor. The default value of `calc-context-sensitive-enter` is
    `nil`.

Variable: **calc-undo-length** [¶](#index-calc_002dundo_002dlength)
:   The variable `calc-undo-length` determines the number of undo
    steps that Calc will keep track of when `calc-quit` is called.
    If `calc-undo-length` is a non-negative integer, then this is the
    number of undo steps that will be preserved; if
    `calc-undo-length` has any other value, then all undo steps will
    be preserved. The default value of `calc-undo-length` is ‘`100`’.

Variable: **calc-gregorian-switch** [¶](#index-calc_002dgregorian_002dswitch)
:   See [Date Forms](#Date-Forms).  
    The variable `calc-gregorian-switch` is either a list of integers
    `(YEAR MONTH DAY)` or `nil`.
    If it is `nil`, then Calc’s date forms always represent Gregorian dates.
    Otherwise, `calc-gregorian-switch` represents the date that the
    calendar switches from Julian dates to Gregorian dates;
    `(YEAR MONTH DAY)` will be the first Gregorian
    date. The customization buffer will offer several standard dates to
    choose from, or the user can enter their own date.

    The default value of `calc-gregorian-switch` is `nil`.

Variable: **calc-string-maximum-character** [¶](#index-calc_002dstring_002dmaximum_002dcharacter)
:   See [Strings](#Strings).

    The variable `calc-string-maximum-character` is the maximum value
    of a vector’s elements for `calc-display-strings`, `string`,
    and `bstring` to display the vector as a string. This maximum
    *must* represent a character, i.e. it’s a non-negative integer less
    than or equal to `(max-char)` or `0x3FFFFF`. Any value not
    representing a character effectively disables the display of strings.
    Some natural choices (and their resulting ranges) are:

    * `0x7F` or 127 (ASCII),
    * `0xFF` or 255 (Latin-1, the default),
    * `0x10FFFF` (Unicode),
    * `0x3FFFFF` (Emacs).

    The default value of `calc-string-maximum-character` is `0xFF`
    or 255.

Variable: **calc-inhibit-startup-message** [¶](#index-calc_002dinhibit_002dstartup_002dmessage)
:   The variable `calc-inhibit-startup-message` controls display of a
    welcome message when starting Calc. If it is `nil` (the default),
    Calc will print a brief message listing key bindings to get help or to
    quit. If it is non-`nil`, Calc will start without printing
    anything.

---

Next: [Calc Summary](#Summary), Previous: [Customizing Calc](#Customizing-Calc), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## Appendix D Reporting Bugs [¶](#Reporting-Bugs-1)

If you find a bug in Calc, send e-mail to [bug-gnu-emacs@gnu.org](mailto:bug-gnu-emacs@gnu.org).
There is an automatic command `M-x report-emacs-bug` which helps
you to report bugs. This command prompts you for a brief subject
line, then leaves you in a mail editing buffer. Type `C-c C-c` to
send your mail. Make sure your subject line indicates that you are
reporting a Calc bug.

If you have suggestions for additional features for Calc, please send
them. Some have dared to suggest that Calc is already top-heavy with
features; this obviously cannot be the case, so if you have ideas, send
them right in.

At the front of the source file, `calc.el`, is a list of ideas for
future work. If any enthusiastic souls wish to take it upon themselves
to work on these, please send a message (using `M-x report-emacs-bug`)
so any efforts can be coordinated.

The latest version of Calc is available from Savannah, in the Emacs
repository. See <https://savannah.gnu.org/projects/emacs>.

---

Next: [Index of Key Sequences](#Key-Index), Previous: [Reporting Bugs](#Reporting-Bugs), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## Appendix E Calc Summary [¶](#Calc-Summary)

This section includes a complete list of Calc keystroke commands.
Each line lists the stack entries used by the command (top-of-stack
last), the keystrokes themselves, the prompts asked by the command,
and the result of the command (also with top-of-stack last).
The result is expressed using the equivalent algebraic function.
Commands which put no results on the stack show the full `M-x`
command name in that position. Numbers preceding the result or
command name refer to notes at the end.

Algebraic functions and `M-x` commands that don’t have corresponding
keystrokes are not listed in this summary.
See [Index of Calculator Commands](#Command-Index). See [Index of Algebraic Functions](#Function-Index).

```
            C-x * a                   33  calc-embedded-activate
            C-x * b                       calc-big-or-small
            C-x * c                       calc
            C-x * d                       calc-embedded-duplicate
            C-x * e                   34  calc-embedded
            C-x * f  formula              calc-embedded-new-formula
            C-x * g                   35  calc-grab-region
            C-x * i                       calc-info
            C-x * j                       calc-embedded-select
            C-x * k                       calc-keypad
            C-x * l                       calc-load-everything
            C-x * m                       read-kbd-macro
            C-x * n                    4  calc-embedded-next
            C-x * o                       calc-other-window
            C-x * p                    4  calc-embedded-previous
            C-x * q  formula              quick-calc
            C-x * r                   36  calc-grab-rectangle
            C-x * s                       calc-info-summary
            C-x * t                       calc-tutorial
            C-x * u                       calc-embedded-update-formula
            C-x * w                       calc-embedded-word
            C-x * x                       calc-quit
            C-x * y              1,28,49  calc-copy-to-buffer
            C-x * z                       calc-user-invocation
            C-x * :                   36  calc-grab-sum-down
            C-x * _                   36  calc-grab-sum-across
            C-x * ‘  editing          30  calc-embedded-edit
            C-x * 0  (zero)               calc-reset

             0-9   number               number
             .     number               0.number
             _     number               -number
             e     number               1e number
             #     number               current-radix#number
             p     (in number)          +/-
             M     (in number)          mod
             @ ’ "   (in number)        HMS form
             h m s   (in number)        HMS form

             ’     formula       37,46  formula
             $     formula       37,46  $formula
             "     string        37,46  string

    a b      +                       2  add(a,b)  a+b
    a b      -                       2  sub(a,b)  a−b
    a b      *                       2  mul(a,b)  a b, a*b
    a b      /                       2  div(a,b)  a/b
    a b      ^                       2  pow(a,b)  a^b
    a b    I ^                       2  nroot(a,b)  a^(1/b)
    a b      %                       2  mod(a,b)  a%b
    a b      \                       2  idiv(a,b)  a\b
    a b      :                       2  fdiv(a,b)
    a b      |                       2  vconcat(a,b)  a|b
    a b    I |                          vconcat(b,a)  b|a
    a b    H |                       2  append(a,b)
    a b  I H |                          append(b,a)
      a      &                       1  inv(a)  1/a
      a      !                       1  fact(a)  a!
      a      =                       1  evalv(a)
      a      M-%                        percent(a)  a%

  ... a      RET                     1  ... a a
  ... a      SPC                     1  ... a a
... a b      TAB                     3  ... b a
. a b c      M-TAB                   3  ... b c a
... a b      LFD                     1  ... a b a
  ... a      DEL                     1  ...
... a b      M-DEL                   1  ... b
             M-RET                   4  calc-last-args
      a      ‘     editing        1,30  calc-edit

  ... a      C-d                     1  ...
             C-k                    27  calc-kill
             C-w                    27  calc-kill-region
             C-y                        calc-yank
             C-_                     4  calc-undo
             M-k                    27  calc-copy-as-kill
             M-w                    27  calc-copy-region-as-kill

             [                          [...
[.. a b      ]                          [a,b]
             (                          (...
(.. a b      )                          (a,b)
             ,                          vector or rect complex
             ;                          matrix or polar complex
             ..                         interval

             ~                          calc-num-prefix
             <                       4  calc-scroll-left
             >                       4  calc-scroll-right
             {                       4  calc-scroll-down
             }                       4  calc-scroll-up
             ?                          calc-help

      a      n                       1  neg(a)  −a
             o                       4  calc-realign
             p     precision        31  calc-precision
             q                          calc-quit
             w                          calc-why
             x     command              M-x calc-command
      a      y                 1,28,49  calc-copy-to-buffer

      a      A                       1  abs(a)
    a b      B                       2  log(a,b)
    a b    I B                       2  alog(a,b)  b^a
      a      C                       1  cos(a)
      a    I C                       1  arccos(a)
      a    H C                       1  cosh(a)
      a  I H C                       1  arccosh(a)
             D                       4  calc-redo
      a      E                       1  exp(a)
      a    H E                       1  exp10(a)  10.^a
      a      F                    1,11  floor(a,d)
      a    I F                    1,11  ceil(a,d)
      a    H F                    1,11  ffloor(a,d)
      a  I H F                    1,11  fceil(a,d)
      a      G                       1  arg(a)
             H     command          32  Hyperbolic
             I     command          32  Inverse
      a      J                       1  conj(a)
             K     command          32  Keep-args
      a      L                       1  ln(a)
      a    H L                       1  log10(a)
             M                          calc-more-recursion-depth
           I M                          calc-less-recursion-depth
      a      N                       5  evalvn(a)
             O     command          32  Option
             P                          pi
           I P                          gamma
           H P                          e
         I H P                          phi
      a      Q                       1  sqrt(a)
      a    I Q                       1  sqr(a)  a^2
      a      R                    1,11  round(a,d)
      a    I R                    1,11  trunc(a,d)
      a    H R                    1,11  fround(a,d)
      a  I H R                    1,11  ftrunc(a,d)
      a      S                       1  sin(a)
      a    I S                       1  arcsin(a)
      a    H S                       1  sinh(a)
      a  I H S                       1  arcsinh(a)
      a      T                       1  tan(a)
      a    I T                       1  arctan(a)
      a    H T                       1  tanh(a)
      a  I H T                       1  arctanh(a)
             U                       4  calc-undo
             X                       4  calc-call-last-kbd-macro

    a b      a =                     2  eq(a,b)  a=b
    a b      a #                     2  neq(a,b)  a!=b
    a b      a <                     2  lt(a,b)  a<b
    a b      a >                     2  gt(a,b)  a>b
    a b      a [                     2  leq(a,b)  a<=b
    a b      a ]                     2  geq(a,b)  a>=b
    a b      a {                     2  in(a,b)
    a b      a &                  2,45  land(a,b)  a&&b
    a b      a |                  2,45  lor(a,b)  a||b
      a      a !                  1,45  lnot(a)  !a
  a b c      a :                    45  if(a,b,c)  a?b:c
      a      a .                     1  rmeq(a)
      a      a "                   7,8  calc-expand-formula

      a      a +   i, l, h        6,38  sum(a,i,l,h)
      a      a -   i, l, h        6,38  asum(a,i,l,h)
      a      a *   i, l, h        6,38  prod(a,i,l,h)
    a b      a _                     2  subscr(a,b)  a_b

    a b      a \                     2  pdiv(a,b)
    a b      a %                     2  prem(a,b)
    a b      a /                     2  pdivrem(a,b)  [q,r]
    a b    H a /                     2  pdivide(a,b)  q+r/b

      a      a a                     1  apart(a)
      a      a b   old, new         38  subst(a,old,new)
      a      a c   v                38  collect(a,v)
      a      a d   v              4,38  deriv(a,v)
      a    H a d   v              4,38  tderiv(a,v)
      a      a e                        esimplify(a)
      a      a f                     1  factor(a)
      a    H a f                     1  factors(a)
    a b      a g                     2  pgcd(a,b)
      a      a i   v                38  integ(a,v)
      a      a m   pats             38  match(a,pats)
      a    I a m   pats             38  matchnot(a,pats)
 data x      a p                    28  polint(data,x)
 data x    H a p                    28  ratint(data,x)
      a      a n                     1  nrat(a)
      a      a r   rules        4,8,38  rewrite(a,rules,n)
      a      a s                        simplify(a)
      a      a t   v, n          31,39  taylor(a,v,n)
      a      a v                   7,8  calc-alg-evaluate
      a      a x                   4,8  expand(a)

   data      a F   model, vars      48  fit(m,iv,pv,data)
   data    I a F   model, vars      48  xfit(m,iv,pv,data)
   data    H a F   model, vars      48  efit(m,iv,pv,data)
      a      a I   v, l, h          38  ninteg(a,v,l,h)
    a b      a M   op               22  mapeq(op,a,b)
    a b    I a M   op               22  mapeqr(op,a,b)
    a b    H a M   op               22  mapeqp(op,a,b)
    a g      a N   v                38  minimize(a,v,g)
    a g    H a N   v                38  wminimize(a,v,g)
      a      a P   v                38  roots(a,v)
    a g      a R   v                38  root(a,v,g)
    a g    H a R   v                38  wroot(a,v,g)
      a      a S   v                38  solve(a,v)
      a    I a S   v                38  finv(a,v)
      a    H a S   v                38  fsolve(a,v)
      a  I H a S   v                38  ffinv(a,v)
      a      a T   i, l, h        6,38  table(a,i,l,h)
    a g      a X   v                38  maximize(a,v,g)
    a g    H a X   v                38  wmaximize(a,v,g)

    a b      b a                     9  and(a,b,w)
      a      b c                     9  clip(a,w)
    a b      b d                     9  diff(a,b,w)
      a      b l                    10  lsh(a,n,w)
    a n    H b l                     9  lsh(a,n,w)
      a      b n                     9  not(a,w)
    a b      b o                     9  or(a,b,w)
      v      b p                     1  vpack(v)
      a      b r                    10  rsh(a,n,w)
    a n    H b r                     9  rsh(a,n,w)
      a      b t                    10  rot(a,n,w)
    a n    H b t                     9  rot(a,n,w)
      a      b u                     1  vunpack(a)
             b w   w              9,50  calc-word-size
    a b      b x                     9  xor(a,b,w)

c s l p      b D                        ddb(c,s,l,p)
  r n p      b F                        fv(r,n,p)
  r n p    I b F                        fvb(r,n,p)
  r n p    H b F                        fvl(r,n,p)
      v      b I                    19  irr(v)
      v    I b I                    19  irrb(v)
      a      b L                    10  ash(a,n,w)
    a n    H b L                     9  ash(a,n,w)
  r n a      b M                        pmt(r,n,a)
  r n a    I b M                        pmtb(r,n,a)
  r n a    H b M                        pmtl(r,n,a)
    r v      b N                    19  npv(r,v)
    r v    I b N                    19  npvb(r,v)
  r n p      b P                        pv(r,n,p)
  r n p    I b P                        pvb(r,n,p)
  r n p    H b P                        pvl(r,n,p)
      a      b R                    10  rash(a,n,w)
    a n    H b R                     9  rash(a,n,w)
  c s l      b S                        sln(c,s,l)
  n p a      b T                        rate(n,p,a)
  n p a    I b T                        rateb(n,p,a)
  n p a    H b T                        ratel(n,p,a)
c s l p      b Y                        syd(c,s,l,p)

  r p a      b #                        nper(r,p,a)
  r p a    I b #                        nperb(r,p,a)
  r p a    H b #                        nperl(r,p,a)
    a b      b %                        relch(a,b)

      a      c c                     5  pclean(a,p)
      a      c 0-9                      pclean(a,p)
      a    H c c                     5  clean(a,p)
      a    H c 0-9                      clean(a,p)
      a      c d                     1  deg(a)
      a      c f                     1  pfloat(a)
      a    H c f                     1  float(a)
      a      c h                     1  hms(a)
      a      c p                        polar(a)
      a    I c p                        rect(a)
      a      c r                     1  rad(a)

      a      c F                     5  pfrac(a,p)
      a    H c F                     5  frac(a,p)

      a      c %                        percent(a*100)

             d .   char             50  calc-point-char
             d ,   char             50  calc-group-char
             d <                 13,50  calc-left-justify
             d =                 13,50  calc-center-justify
             d >                 13,50  calc-right-justify
             d {   label            50  calc-left-label
             d }   label            50  calc-right-label
             d [                     4  calc-truncate-up
             d ]                     4  calc-truncate-down
             d "                 12,50  calc-display-strings
             d SPC                      calc-refresh
             d RET                   1  calc-refresh-top

             d 0                    50  calc-decimal-radix
             d 2                    50  calc-binary-radix
             d 6                    50  calc-hex-radix
             d 8                    50  calc-octal-radix

             d b              12,13,50  calc-line-breaking
             d c                    50  calc-complex-notation
             d d   format           50  calc-date-notation
             d e                  5,50  calc-eng-notation
             d f   num           31,50  calc-fix-notation
             d g              12,13,50  calc-group-digits
             d h   format           50  calc-hms-notation
             d i                    50  calc-i-notation
             d j                    50  calc-j-notation
             d l                 12,50  calc-line-numbering
             d n                  5,50  calc-normal-notation
             d o   format           50  calc-over-notation
             d p                 12,50  calc-show-plain
             d r   radix         31,50  calc-radix
             d s                  5,50  calc-sci-notation
             d t                    27  calc-truncate-stack
             d w                 12,13  calc-auto-why
             d z                 12,50  calc-leading-zeros

             d B                    50  calc-big-language
             d C                    50  calc-c-language
             d E                    50  calc-eqn-language
             d F                    50  calc-fortran-language
             d M                    50  calc-mathematica-language
             d N                    50  calc-normal-language
             d O                    50  calc-flat-language
             d P                    50  calc-pascal-language
             d T                    50  calc-tex-language
             d L                    50  calc-latex-language
             d U                    50  calc-unformatted-language
             d W                    50  calc-maple-language

      a      f [                     4  decr(a,n)
      a      f ]                     4  incr(a,n)

    a b      f b                     2  beta(a,b)
      a      f e                     1  erf(a)
      a    I f e                     1  erfc(a)
      a      f g                     1  gamma(a)
    a b      f h                     2  hypot(a,b)
      a      f i                     1  im(a)
    n a      f j                     2  besJ(n,a)
    a b      f n                     2  min(a,b)
      a      f r                     1  re(a)
      a      f s                     1  sign(a)
    a b      f x                     2  max(a,b)
    n a      f y                     2  besY(n,a)

      a      f A                     1  abssqr(a)
  x a b      f B                        betaI(x,a,b)
  x a b    H f B                        betaB(x,a,b)
      a      f E                     1  expm1(a)
    a x      f G                     2  gammaP(a,x)
    a x    I f G                     2  gammaQ(a,x)
    a x    H f G                     2  gammag(a,x)
    a x  I H f G                     2  gammaG(a,x)
    a b      f I                     2  ilog(a,b)
    a b    I f I                     2  alog(a,b)  b^a
      a      f L                     1  lnp1(a)
      a      f M                     1  mant(a)
      a      f Q                     1  isqrt(a)
      a    I f Q                     1  sqr(a)  a^2
    a n      f S                     2  scf(a,n)
    y x      f T                        arctan2(y,x)
      a      f X                     1  xpon(a)

    x y      g a                 28,40  calc-graph-add
             g b                    12  calc-graph-border
             g c                        calc-graph-clear
             g d                    41  calc-graph-delete
    x y      g f                 28,40  calc-graph-fast
             g g                    12  calc-graph-grid
             g h   title                calc-graph-header
             g j                     4  calc-graph-juggle
             g k                    12  calc-graph-key
             g l                    12  calc-graph-log-x
             g n   name                 calc-graph-name
             g p                    42  calc-graph-plot
             g q                        calc-graph-quit
             g r   range                calc-graph-range-x
             g s                 12,13  calc-graph-line-style
             g t   title                calc-graph-title-x
             g v                        calc-graph-view-commands
             g x   display              calc-graph-display
             g z                    12  calc-graph-zero-x

  x y z      g A                 28,40  calc-graph-add-3d
             g C   command              calc-graph-command
             g D   device        43,44  calc-graph-device
  x y z      g F                 28,40  calc-graph-fast-3d
             g H                    12  calc-graph-hide
             g K                        calc-graph-kill
             g L                    12  calc-graph-log-y
             g N   number        43,51  calc-graph-num-points
             g O   filename      43,44  calc-graph-output
             g P                    42  calc-graph-print
             g R   range                calc-graph-range-y
             g S                 12,13  calc-graph-point-style
             g T   title                calc-graph-title-y
             g V                        calc-graph-view-trail
             g X   format               calc-graph-geometry
             g Z                    12  calc-graph-zero-y

             g C-l                  12  calc-graph-log-z
             g C-r range                calc-graph-range-z
             g C-t title                calc-graph-title-z

             h b                        calc-describe-bindings
             h c   key                  calc-describe-key-briefly
             h f   function             calc-describe-function
             h h                        calc-full-help
             h i                        calc-info
             h k   key                  calc-describe-key
             h n                        calc-view-news
             h s                        calc-info-summary
             h t                        calc-tutorial
             h v   var                  calc-describe-variable

             j 1-9                      calc-select-part
             j RET                  27  calc-copy-selection
             j DEL                  27  calc-del-selection
             j ’   formula          27  calc-enter-selection
             j ‘   editing       27,30  calc-edit-selection
             j "                  7,27  calc-sel-expand-formula

             j +   formula          27  calc-sel-add-both-sides
             j -   formula          27  calc-sel-sub-both-sides
             j *   formula          27  calc-sel-mult-both-sides
             j /   formula          27  calc-sel-div-both-sides
             j &                    27  calc-sel-invert

             j a                    27  calc-select-additional
             j b                    12  calc-break-selections
             j c                        calc-clear-selections
             j d                 12,50  calc-show-selections
             j e                    12  calc-enable-selections
             j l                  4,27  calc-select-less
             j m                  4,27  calc-select-more
             j n                     4  calc-select-next
             j o                  4,27  calc-select-once
             j p                     4  calc-select-previous
             j r   rules        4,8,27  calc-rewrite-selection
             j s                  4,27  calc-select-here
             j u                    27  calc-unselect
             j v                  7,27  calc-sel-evaluate

             j C                    27  calc-sel-commute
             j D                  4,27  calc-sel-distribute
             j E                    27  calc-sel-jump-equals
             j I                    27  calc-sel-isolate
           H j I                    27  calc-sel-isolate (full)
             j L                  4,27  calc-commute-left
             j M                    27  calc-sel-merge
             j N                    27  calc-sel-negate
             j O                  4,27  calc-select-once-maybe
             j R                  4,27  calc-commute-right
             j S                  4,27  calc-select-here-maybe
             j U                    27  calc-sel-unpack

             k a                        calc-random-again
      n      k b                     1  bern(n)
    n x    H k b                     2  bern(n,x)
    n m      k c                     2  choose(n,m)
    n m    H k c                     2  perm(n,m)
      n      k d                     1  dfact(n)  n!!
      n      k e                     1  euler(n)
    n x    H k e                     2  euler(n,x)
      n      k f                     4  prfac(n)
    n m      k g                     2  gcd(n,m)
    m n      k h                    14  shuffle(n,m)
    n m      k l                     2  lcm(n,m)
      n      k m                     1  moebius(n)
      n      k n                     4  nextprime(n)
      n    I k n                     4  prevprime(n)
      n      k p                  4,28  calc-prime-test
      m      k r                    14  random(m)
    n m      k s                     2  stir1(n,m)
    n m    H k s                     2  stir2(n,m)
      n      k t                     1  totient(n)

  n p x      k B                        utpb(x,n,p)
  n p x    I k B                        ltpb(x,n,p)
    v x      k C                        utpc(x,v)
    v x    I k C                        ltpc(x,v)
    n m      k E                        egcd(n,m)
v1 v2 x      k F                        utpf(x,v1,v2)
v1 v2 x    I k F                        ltpf(x,v1,v2)
  m s x      k N                        utpn(x,m,s)
  m s x    I k N                        ltpn(x,m,s)
    m x      k P                        utpp(x,m)
    m x    I k P                        ltpp(x,m)
    v x      k T                        utpt(x,v)
    v x    I k T                        ltpt(x,v)

    a b      l +                        lupadd(a,b)
    a b    H l +                        lufadd(a,b)
    a b      l -                        lupsub(a,b)
    a b    H l -                        lufsub(a,b)
    a b      l *                        lupmul(a,b)
    a b    H l *                        lufmul(a,b)
    a b      l /                        lupdiv(a,b)
    a b    H l /                        lufdiv(a,b)
      a      l d                        dbpower(a)
    a b    O l d                        dbpower(a,b)
      a    H l d                        dbfield(a)
    a b  O H l d                        dbfield(a,b)
      a      l n                        nppower(a)
    a b    O l n                        nppower(a,b)
      a    H l n                        npfield(a)
    a b  O H l n                        npfield(a,b)
      a      l q                        lupquant(a)
    a b    O l q                        lupquant(a,b)
      a    H l q                        lufquant(a)
    a b  O H l q                        lufquant(a,b)
      a      l s                        spn(a)
      a      l m                        midi(a)
      a      l f                        freq(a)

             m a                 12,13  calc-algebraic-mode
             m d                        calc-degrees-mode
             m e                        calc-embedded-preserve-modes
             m f                    12  calc-frac-mode
             m g                    52  calc-get-modes
             m h                        calc-hms-mode
             m i                 12,13  calc-infinite-mode
             m m                        calc-save-modes
             m p                    12  calc-polar-mode
             m r                        calc-radians-mode
             m s                    12  calc-symbolic-mode
             m t                    12  calc-total-algebraic-mode
             m v                 12,13  calc-matrix-mode
             m w                    13  calc-working
             m x                        calc-always-load-extensions

             m A                    12  calc-alg-simplify-mode
             m B                    12  calc-bin-simplify-mode
             m C                    12  calc-auto-recompute
             m D                        calc-default-simplify-mode
             m E                    12  calc-ext-simplify-mode
             m F   filename         13  calc-settings-file-name
             m N                    12  calc-num-simplify-mode
             m O                    12  calc-no-simplify-mode
             m R                 12,13  calc-mode-record-mode
             m S                    12  calc-shift-prefix
             m U                    12  calc-units-simplify-mode

             r s   register         27  calc-copy-to-register
             r i   register             calc-insert-register

             s c   var1, var2       29  calc-copy-variable
             s d   var, decl            calc-declare-variable
             s e   var, editing  29,30  calc-edit-variable
             s i   buffer               calc-insert-variables
             s k   const, var       29  calc-copy-special-constant
    a b      s l   var              29  a  (letting var=b)
  a ...      s m   op, var       22,29  calc-store-map
             s n   var           29,47  calc-store-neg  (v/-1)
             s p   var              29  calc-permanent-variable
             s r   var              29  v  (recalled value)
             r 0-9                      calc-recall-quick
      a      s s   var           28,29  calc-store
      a      s 0-9                      calc-store-quick
      a      s t   var              29  calc-store-into
      a      t 0-9                      calc-store-into-quick
             s u   var              29  calc-unstore
      a      s x   var              29  calc-store-exchange

             s A   editing          30  calc-edit-AlgSimpRules
             s D   editing          30  calc-edit-Decls
             s E   editing          30  calc-edit-EvalRules
             s F   editing          30  calc-edit-FitRules
             s G   editing          30  calc-edit-GenCount
             s H   editing          30  calc-edit-Holidays
             s I   editing          30  calc-edit-IntegLimit
             s L   editing          30  calc-edit-LineStyles
             s P   editing          30  calc-edit-PointStyles
             s R   editing          30  calc-edit-PlotRejects
             s T   editing          30  calc-edit-TimeZone
             s U   editing          30  calc-edit-Units
             s X   editing          30  calc-edit-ExtSimpRules

      a      s +   var           29,47  calc-store-plus  (v+a)
      a      s -   var           29,47  calc-store-minus  (v-a)
      a      s *   var           29,47  calc-store-times  (v*a)
      a      s /   var           29,47  calc-store-div  (v/a)
      a      s ^   var           29,47  calc-store-power  (v^a)
      a      s |   var           29,47  calc-store-concat  (v|a)
             s &   var           29,47  calc-store-inv  (v^-1)
             s [   var           29,47  calc-store-decr  (v-1)
             s ]   var           29,47  calc-store-incr  (v-(-1))
    a b      s :                     2  assign(a,b)  a := b
      a      s =                     1  evalto(a,b)  a =>

             t [                     4  calc-trail-first
             t ]                     4  calc-trail-last
             t <                     4  calc-trail-scroll-left
             t >                     4  calc-trail-scroll-right
             t .                    12  calc-full-trail-vectors

             t b                     4  calc-trail-backward
             t d                 12,50  calc-trail-display
             t f                     4  calc-trail-forward
             t h                        calc-trail-here
             t i                        calc-trail-in
             t k                     4  calc-trail-kill
             t m   string               calc-trail-marker
             t n                     4  calc-trail-next
             t o                        calc-trail-out
             t p                     4  calc-trail-previous
             t r   string               calc-trail-isearch-backward
             t s   string               calc-trail-isearch-forward
             t y                     4  calc-trail-yank

      d      t C   oz, nz               tzconv(d,oz,nz)
d oz nz      t C   $                    tzconv(d,oz,nz)
      d      t D                    15  date(d)
      d      t I                     4  incmonth(d,n)
      d      t J                    16  julian(d,z)
      d      t M                    17  newmonth(d,n)
             t N                    16  now(z)
      d      t P   1                31  year(d)
      d      t P   2                31  month(d)
      d      t P   3                31  day(d)
      d      t P   4                31  hour(d)
      d      t P   5                31  minute(d)
      d      t P   6                31  second(d)
      d      t P   7                31  weekday(d)
      d      t P   8                31  yearday(d)
      d      t P   9                31  time(d)
      d      t U                    16  unixtime(d,z)
      d      t W                    17  newweek(d,w)
      d      t Y                    17  newyear(d,n)

    a b      t +                     2  badd(a,b)
    a b      t -                     2  bsub(a,b)

             u a                    12  calc-autorange-units
      a      u b                        calc-base-units
      a      u c   units            18  calc-convert-units
   defn      u d   unit, descr          calc-define-unit
             u e                        calc-explain-units
             u g   unit                 calc-get-unit-definition
             u n   units            18  calc-convert-exact-units
             u p                        calc-permanent-units
      a      u r                        calc-remove-units
      a      u s                        usimplify(a)
      a      u t   units            18  calc-convert-temperature
             u u   unit                 calc-undefine-unit
             u v                        calc-enter-units-table
      a      u x                        calc-extract-units
      a      u 0-9                      calc-quick-units

  v1 v2      u C                    20  vcov(v1,v2)
  v1 v2    I u C                    20  vpcov(v1,v2)
  v1 v2    H u C                    20  vcorr(v1,v2)
      v      u G                    19  vgmean(v)
    a b    H u G                     2  agmean(a,b)
      v      u M                    19  vmean(v)
      v    I u M                    19  vmeane(v)
      v    H u M                    19  vmedian(v)
      v  I H u M                    19  vhmean(v)
      v      u N                    19  vmin(v)
      v      u R                        rms(v)
      v      u S                    19  vsdev(v)
      v    I u S                    19  vpsdev(v)
      v    H u S                    19  vvar(v)
      v  I H u S                    19  vpvar(v)
             u V                        calc-view-units-table
      v      u X                    19  vmax(v)

      v      u +                    19  vsum(v)
      v      u *                    19  vprod(v)
      v      u #                    19  vcount(v)

             V (                    50  calc-vector-parens
             V {                    50  calc-vector-braces
             V [                    50  calc-vector-brackets
             V ]   ROCP             50  calc-matrix-brackets
             V ,                    50  calc-vector-commas
             V <                    50  calc-matrix-left-justify
             V =                    50  calc-matrix-center-justify
             V >                    50  calc-matrix-right-justify
             V /                 12,50  calc-break-vectors
             V .                 12,50  calc-full-vectors

    s t      V ^                     2  vint(s,t)
    s t      V -                     2  vdiff(s,t)
      s      V ~                     1  vcompl(s)
      s      V #                     1  vcard(s)
      s      V :                     1  vspan(s)
      s      V +                     1  rdup(s)

      m      V &                     1  inv(m)  1/m

      v      v a   n                    arrange(v,n)
      a      v b   n                    cvec(a,n)
      v      v c   n >0          21,31  mcol(v,n)
      v      v c   n <0             31  mrcol(v,-n)
      m      v c   0                31  getdiag(m)
      v      v d                    25  diag(v,n)
    v m      v e                     2  vexp(v,m)
  v m f    H v e                     2  vexp(v,m,f)
    v a      v f                    26  find(v,a,n)
      v      v h                     1  head(v)
      v    I v h                     1  tail(v)
      v    H v h                     1  rhead(v)
      v  I H v h                     1  rtail(v)
             v i   n                31  idn(1,n)
             v i   0                31  idn(1)
    h t      v k                     2  cons(h,t)
    h t    H v k                     2  rcons(h,t)
      v      v l                     1  vlen(v)
      v    H v l                     1  mdims(v)
    v m      v m                     2  vmask(v,m)
      v      v n                     1  rnorm(v)
  a b c      v p                    24  calc-pack
      v      v r   n >0          21,31  mrow(v,n)
      v      v r   n <0             31  mrrow(v,-n)
      m      v r   0                31  getdiag(m)
  v i j      v s                        subvec(v,i,j)
  v i j    I v s                        rsubvec(v,i,j)
      m      v t                     1  trn(m)
      v      v u                    24  calc-unpack
      v      v v                     1  rev(v)
             v x   n                31  index(n)
  n s i  C-u v x                        index(n,s,i)

      v      V A   op               22  apply(op,v)
  v1 v2      V C                     2  cross(v1,v2)
      m      V D                     1  det(m)
      s      V E                     1  venum(s)
      s      V F                     1  vfloor(s)
      v      V G                        grade(v)
      v    I V G                        rgrade(v)
      v      V H   n                31  histogram(v,n)
    v w    H V H   n                31  histogram(v,w,n)
  v1 v2      V I   mop aop          22  inner(mop,aop,v1,v2)
      m      V J                     1  ctrn(m)
  m1 m2      V K                        kron(m1,m2)
      m      V L                     1  lud(m)
      v      V M   op            22,23  map(op,v)
      v      V N                     1  cnorm(v)
  v1 v2      V O   op               22  outer(op,v1,v2)
      v      V R   op            22,23  reduce(op,v)
      v    I V R   op            22,23  rreduce(op,v)
    a n    H V R   op               22  nest(op,a,n)
      a  I H V R   op               22  fixp(op,a)
      v      V S                        sort(v)
      v    I V S                        rsort(v)
      m      V T                     1  tr(m)
      v      V U   op               22  accum(op,v)
      v    I V U   op               22  raccum(op,v)
    a n    H V U   op               22  anest(op,a,n)
      a  I H V U   op               22  afixp(op,a)
    s t      V V                     2  vunion(s,t)
    s t      V X                     2  vxor(s,t)

             Y                          user commands

             z                          user commands

      c      Z [                    45  calc-kbd-if
      c      Z |                    45  calc-kbd-else-if
             Z :                        calc-kbd-else
             Z ]                        calc-kbd-end-if

             Z {                     4  calc-kbd-loop
      c      Z /                    45  calc-kbd-break
             Z }                        calc-kbd-end-loop
      n      Z <                        calc-kbd-repeat
             Z >                        calc-kbd-end-repeat
    n m      Z (                        calc-kbd-for
      s      Z )                        calc-kbd-end-for

             Z C-g                      cancel if/loop command

             Z ‘                        calc-kbd-push
             Z ’                        calc-kbd-pop
             Z #                        calc-kbd-query

   comp      Z C   func, args       50  calc-user-define-composition
             Z D   key, command         calc-user-define
             Z E   key, editing     30  calc-user-define-edit
   defn      Z F   k, c, f, a, n    28  calc-user-define-formula
             Z G   key                  calc-get-user-defn
             Z I                        calc-user-define-invocation
             Z K   key, command         calc-user-define-kbd-macro
             Z P   key                  calc-user-define-permanent
             Z S                    30  calc-edit-user-syntax
             Z T                    12  calc-timing
             Z U   key                  calc-user-undefine
```

NOTES

1. Positive prefix arguments apply to ‘`n`’ stack entries.
   Negative prefix arguments apply to the ‘`-n`’th stack entry.
   A prefix of zero applies to the entire stack. (For `LFD` and
   `M-DEL`, the meaning of the sign is reversed.)
2. Positive prefix arguments apply to ‘`n`’ stack entries.
   Negative prefix arguments apply to the top stack entry
   and the next ‘`-n`’ stack entries.
3. Positive prefix arguments rotate top ‘`n`’ stack entries by one.
   Negative prefix arguments rotate the entire stack by ‘`-n`’.
   A prefix of zero reverses the entire stack.
4. Prefix argument specifies a repeat count or distance.
5. Positive prefix arguments specify a precision ‘`p`’.
   Negative prefix arguments reduce the current precision by ‘`-p`’.
6. A prefix argument is interpreted as an additional step-size parameter.
   A plain `C-u` prefix means to prompt for the step size.
7. A prefix argument specifies simplification level and depth.
   1=Basic simplifications, 2=Algebraic simplifications, 3=Extended simplifications
8. A negative prefix operates only on the top level of the input formula.
9. Positive prefix arguments specify a word size of ‘`w`’ bits, unsigned.
   Negative prefix arguments specify a word size of ‘`w`’ bits, signed.
10. Prefix arguments specify the shift amount ‘`n`’. The ‘`w`’ argument
    cannot be specified in the keyboard version of this command.
11. From the keyboard, ‘`d`’ is omitted and defaults to zero.
12. Mode is toggled; a positive prefix always sets the mode, and a negative
    prefix always clears the mode.
13. Some prefix argument values provide special variations of the mode.
14. A prefix argument, if any, is used for ‘`m`’ instead of taking
    ‘`m`’ from the stack. ‘`M`’ may take any of these values:

    Integer
    :   Random integer in the interval ‘`[0 .. m)`’.

    Float
    :   Random floating-point number in the interval ‘`[0 .. m)`’.

    0.0
    :   Gaussian with mean 1 and standard deviation 0.

    Error form
    :   Gaussian with specified mean and standard deviation.

    Interval
    :   Random integer or floating-point number in that interval.

    Vector
    :   Random element from the vector.
15. A prefix argument from 1 to 6 specifies number of date components
    to remove from the stack. See [Date Conversions](#Date-Conversions).
16. A prefix argument specifies a time zone; `C-u` says to take the
    time zone number or name from the top of the stack. See [Time Zones](#Time-Zones).
17. A prefix argument specifies a day number (0–6, 0–31, or 0–366).
18. If the input has no units, you will be prompted for both the old and
    the new units.
19. With a prefix argument, collect that many stack entries to form the
    input data set. Each entry may be a single value or a vector of values.
20. With a prefix argument of 1, take a single
    *Nx2*
    matrix from the stack instead of two separate data vectors.
21. The row or column number ‘`n`’ may be given as a numeric prefix
    argument instead. A plain `C-u` prefix says to take ‘`n`’
    from the top of the stack. If ‘`n`’ is a vector or interval,
    a subvector/submatrix of the input is created.
22. The ‘`op`’ prompt can be answered with the key sequence for the
    desired function, or with `x` or `z` followed by a function name,
    or with `$` to take a formula from the top of the stack, or with
    `'` and a typed formula. In the last two cases, the formula may
    be a nameless function like ‘`<#1+#2>`’ or ‘`<x, y : x+y>`’; or it
    may include `$`, `$$`, etc., where `$` will correspond to the
    last argument of the created function; or otherwise you will be
    prompted for an argument list. The number of vectors popped from the
    stack by `V M` depends on the number of arguments of the function.
23. One of the mapping direction keys `_` (horizontal, i.e., map
    by rows or reduce across), `:` (vertical, i.e., map by columns or
    reduce down), or `=` (map or reduce by rows) may be used before
    entering ‘`op`’; these modify the function name by adding the letter
    `r` for “rows,” `c` for “columns,” `a` for “across,”
    or `d` for “down.”
24. The prefix argument specifies a packing mode. A nonnegative mode
    is the number of items (for `v p`) or the number of levels
    (for `v u`). A negative mode is as described below. With no
    prefix argument, the mode is taken from the top of the stack and
    may be an integer or a vector of integers.

    -1
    :   (2) Rectangular complex number.

    -2
    :   (2) Polar complex number.

    -3
    :   (3) HMS form.

    -4
    :   (2) Error form.

    -5
    :   (2) Modulo form.

    -6
    :   (2) Closed interval.

    -7
    :   (2) Closed .. open interval.

    -8
    :   (2) Open .. closed interval.

    -9
    :   (2) Open interval.

    -10
    :   (2) Fraction.

    -11
    :   (2) Float with integer mantissa.

    -12
    :   (2) Float with mantissa in ‘`[1 .. 10)`’.

    -13
    :   (1) Date form (using date numbers).

    -14
    :   (3) Date form (using year, month, day).

    -15
    :   (6) Date form (using year, month, day, hour, minute, second).
25. A prefix argument specifies the size ‘`n`’ of the matrix. With no
    prefix argument, ‘`n`’ is omitted and the size is inferred from
    the input vector.
26. The prefix argument specifies the starting position ‘`n`’ (default 1).
27. Cursor position within stack buffer affects this command.
28. Arguments are not actually removed from the stack by this command.
29. Variable name may be a single digit or a full name.
30. Editing occurs in a separate buffer. Press `C-c C-c` (or
    `LFD`, or in some cases `RET`) to finish the edit, or kill the
    buffer with `C-x k` to cancel the edit. The `LFD` key prevents evaluation
    of the result of the edit.
31. The number prompted for can also be provided as a prefix argument.
32. Press this key a second time to cancel the prefix.
33. With a negative prefix, deactivate all formulas. With a positive
    prefix, deactivate and then reactivate from scratch.
34. Default is to scan for nearest formula delimiter symbols. With a
    prefix of zero, formula is delimited by mark and point. With a
    non-zero prefix, formula is delimited by scanning forward or
    backward by that many lines.
35. Parse the region between point and mark as a vector. A nonzero prefix
    parses n lines before or after point as a vector. A zero prefix
    parses the current line as a vector. A `C-u` prefix parses the
    region between point and mark as a single formula.
36. Parse the rectangle defined by point and mark as a matrix. A positive
    prefix n divides the rectangle into columns of width n.
    A zero or `C-u` prefix parses each line as one formula. A negative
    prefix suppresses special treatment of bracketed portions of a line.
37. A numeric prefix causes the current language mode to be ignored.
38. Responding to a prompt with a blank line answers that and all
    later prompts by popping additional stack entries.
39. Answer for ‘`v`’ may also be of the form ‘`v = v_0`’ or
    ‘`v - v_0`’.
40. With a positive prefix argument, stack contains many ‘`y`’’s and one
    common ‘`x`’. With a zero prefix, stack contains a vector of
    ‘`y`’s and a common ‘`x`’. With a negative prefix, stack
    contains many ‘`[x,y]`’ vectors. (For 3D plots, substitute
    ‘`z`’ for ‘`y`’ and ‘`x,y`’ for ‘`x`’.)
41. With any prefix argument, all curves in the graph are deleted.
42. With a positive prefix, refines an existing plot with more data points.
    With a negative prefix, forces recomputation of the plot data.
43. With any prefix argument, set the default value instead of the
    value for this graph.
44. With a negative prefix argument, set the value for the printer.
45. Condition is considered “true” if it is a nonzero real or complex
    number, or a formula whose value is known to be nonzero; it is “false”
    otherwise.
46. Several formulas separated by commas are pushed as multiple stack
    entries. Trailing `)`, `]`, `}`, `>`, and `"`
    delimiters may be omitted. The notation `$$$` refers to the value
    in stack level three, and causes the formula to replace the top three
    stack levels. The notation `$3` refers to stack level three without
    causing that value to be removed from the stack. Use `LFD` in place
    of `RET` to prevent evaluation; use `M-=` in place of `RET`
    to evaluate variables.
47. The variable is replaced by the formula shown on the right. The
    Inverse flag reverses the order of the operands, e.g., `I s - x`
    assigns
    ‘`x := a-x`’.
48. Press `?` repeatedly to see how to choose a model. Answer the
    variables prompt with ‘`iv`’ or ‘`iv;pv`’ to specify
    independent and parameter variables. A positive prefix argument
    takes *n+1* vectors from the stack; a zero prefix takes a matrix
    and a vector from the stack.
49. With a plain `C-u` prefix, replace the current region of the
    destination buffer with the yanked text instead of inserting.
50. All stack entries are reformatted; the `H` prefix inhibits this.
    The `I` prefix sets the mode temporarily, redraws the top stack
    entry, then restores the original setting of the mode.
51. A negative prefix sets the default 3D resolution instead of the
    default 2D resolution.
52. This grabs a vector of the form [prec, wsize, ssize,
    radix, flfmt, ang, frac, symb, polar,
    matrix, simp, inf]. A prefix argument from 1 to 12
    grabs the nth mode value only.

---

Next: [Index of Calculator Commands](#Command-Index), Previous: [Calc Summary](#Summary), Up: [The GNU Emacs Calculator](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Key-Index "Index")]## Index of Key Sequences [¶](#Index-of-Key-Sequences)

|  |  |
| --- | --- |
| Jump to: | [**\_**](#Key-Index_ky_symbol-1)   [**-**](#Key-Index_ky_symbol-2)   [**,**](#Key-Index_ky_symbol-3)   [**;**](#Key-Index_ky_symbol-4)   [**:**](#Key-Index_ky_symbol-5)   [**!**](#Key-Index_ky_symbol-6)   [**?**](#Key-Index_ky_symbol-7)   [**.**](#Key-Index_ky_symbol-8)   [**'**](#Key-Index_ky_symbol-9)   [**"**](#Key-Index_ky_symbol-10)   [**(**](#Key-Index_ky_symbol-11)   [**)**](#Key-Index_ky_symbol-12)   [**[**](#Key-Index_ky_symbol-13)   [**]**](#Key-Index_ky_symbol-14)   [**{**](#Key-Index_ky_symbol-15)   [**}**](#Key-Index_ky_symbol-16)   [**@**](#Key-Index_ky_symbol-17)   [**\***](#Key-Index_ky_symbol-18)   [**/**](#Key-Index_ky_symbol-19)   [**\**](#Key-Index_ky_symbol-20)   [**&**](#Key-Index_ky_symbol-21)   [**#**](#Key-Index_ky_symbol-22)   [**%**](#Key-Index_ky_symbol-23)   [**`**](#Key-Index_ky_symbol-24)   [**^**](#Key-Index_ky_symbol-25)   [**+**](#Key-Index_ky_symbol-26)   [**<**](#Key-Index_ky_symbol-27)   [**=**](#Key-Index_ky_symbol-28)   [**>**](#Key-Index_ky_symbol-29)   [**|**](#Key-Index_ky_symbol-30)   [**~**](#Key-Index_ky_symbol-31)   [**$**](#Key-Index_ky_symbol-32)   [**0**](#Key-Index_ky_symbol-33)     [**A**](#Key-Index_ky_letter-A)   [**B**](#Key-Index_ky_letter-B)   [**C**](#Key-Index_ky_letter-C)   [**D**](#Key-Index_ky_letter-D)   [**E**](#Key-Index_ky_letter-E)   [**F**](#Key-Index_ky_letter-F)   [**G**](#Key-Index_ky_letter-G)   [**H**](#Key-Index_ky_letter-H)   [**I**](#Key-Index_ky_letter-I)   [**J**](#Key-Index_ky_letter-J)   [**K**](#Key-Index_ky_letter-K)   [**L**](#Key-Index_ky_letter-L)   [**M**](#Key-Index_ky_letter-M)   [**N**](#Key-Index_ky_letter-N)   [**O**](#Key-Index_ky_letter-O)   [**P**](#Key-Index_ky_letter-P)   [**Q**](#Key-Index_ky_letter-Q)   [**R**](#Key-Index_ky_letter-R)   [**S**](#Key-Index_ky_letter-S)   [**T**](#Key-Index_ky_letter-T)   [**U**](#Key-Index_ky_letter-U)   [**V**](#Key-Index_ky_letter-V)   [**W**](#Key-Index_ky_letter-W)   [**X**](#Key-Index_ky_letter-X)   [**Y**](#Key-Index_ky_letter-Y)   [**Z**](#Key-Index_ky_letter-Z) |
