### 10.3 Symbolic Prefixes [¶](#Symbolic-Prefixes-1)

Quite a lot of Emacs commands react to the (numeric) prefix. For
instance, `C-u 4 C-f` moves point four characters forward, and
`C-u 9 0 0 I s s p` adds a permanent `Subject` substring score
rule of 900 to the current article.

This is all nice and well, but what if you want to give a command some
additional information? Well, what most commands do is interpret the
“raw” prefix in some special way. `C-u 0 C-x C-s` means that one
doesn’t want a backup file to be created when saving the current buffer,
for instance. But what if you want to save without making a backup
file, and you want Emacs to flash lights and play a nice tune at the
same time? You can’t, and you’re probably perfectly happy that way.

I’m not, so I’ve added a second prefix—the *symbolic prefix*. The
prefix key is `M-i` (`gnus-symbolic-argument`), and the next
character typed in is the value. You can stack as many `M-i`
prefixes as you want. `M-i a C-M-u` means “feed the `C-M-u`
command the symbolic prefix `a`”. `M-i a M-i b C-M-u` means
“feed the `C-M-u` command the symbolic prefixes `a` and
`b`”. You get the drift.

Typing in symbolic prefixes to commands that don’t accept them doesn’t
hurt, but it doesn’t do any good either. Currently not many Gnus
functions make use of the symbolic prefix.

If you’re interested in how Gnus implements this, see [Extended Interactive](#Extended-Interactive).

---

Next: [Window Layout](#Window-Layout), Previous: [Symbolic Prefixes](#Symbolic-Prefixes), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.4 Formatting Variables [¶](#Formatting-Variables-1)

Throughout this manual you’ve probably noticed lots of variables called
things like `gnus-group-line-format` and
`gnus-summary-mode-line-format`. These control how Gnus is to
output lines in the various buffers. There’s quite a lot of them.
Fortunately, they all use the same syntax, so there’s not that much to
be annoyed by.

Gnus does not use the font locking machinery used by most modes in
Emacs, so switching `font-lock-mode` on in the Gnus
group/summary/article buffers usually doesn’t do anything
useful—instead it’ll just mess up the faces that Gnus has already
put in the buffer. (This is also the case for other minor modes that
use the font locking machinery, like `whitespace-mode`.)

Here’s an example format spec (from the group buffer): ‘`%M%S%5y:
%(%g%)\n`’. We see that it is indeed extremely ugly, and that there are
lots of percentages everywhere.

Currently Gnus uses the following formatting variables:
`gnus-group-line-format`, `gnus-summary-line-format`,
`gnus-server-line-format`, `gnus-topic-line-format`,
`gnus-group-mode-line-format`,
`gnus-summary-mode-line-format`,
`gnus-article-mode-line-format`,
`gnus-server-mode-line-format`, and
`gnus-summary-pick-line-format`.

All these format variables can also be arbitrary elisp forms. In that
case, they will be `eval`ed to insert the required lines.

Gnus includes a command to help you while creating your own format
specs. `M-x gnus-update-format` will `eval` the current form,
update the spec in question and pop you to a buffer where you can
examine the resulting Lisp code to be run to generate the line.

* [Formatting Basics](#Formatting-Basics)
* [Mode Line Formatting](#Mode-Line-Formatting)
* [Advanced Formatting](#Advanced-Formatting)
* [User-Defined Specs](#User_002dDefined-Specs)
* [Formatting Fonts](#Formatting-Fonts)
* [Positioning Point](#Positioning-Point)
* [Tabulation](#Tabulation)

---

Next: [Mode Line Formatting](#Mode-Line-Formatting), Up: [Formatting Variables](#Formatting-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.4.1 Formatting Basics [¶](#Formatting-Basics-1)

Each ‘`%`’ element will be replaced by some string or other when the
buffer in question is generated. ‘`%5y`’ means “insert the ‘`y`’
spec, and pad with spaces to get a 5-character field”.

As with normal C and Emacs Lisp formatting strings, the numerical
modifier between the ‘`%`’ and the formatting type character will
*pad* the output so that it is always at least that long.
‘`%5y`’ will make the field always (at least) five characters wide by
padding with spaces to the left. If you say ‘`%-5y`’, it will pad to
the right instead.

You may also wish to limit the length of the field to protect against
particularly wide values. For that you can say ‘`%4,6y`’, which
means that the field will never be more than 6 characters wide and never
less than 4 characters wide.

Also Gnus supports some extended format specifications, such as
‘`%&user-date;`’.

---

Next: [Advanced Formatting](#Advanced-Formatting), Previous: [Formatting Basics](#Formatting-Basics), Up: [Formatting Variables](#Formatting-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.4.2 Mode Line Formatting [¶](#Mode-Line-Formatting-1)

Mode line formatting variables (e.g.,
`gnus-summary-mode-line-format`) follow the same rules as other,
buffer line oriented formatting variables (see [Formatting Basics](#Formatting-Basics))
with the following two differences:

1. There must be no newline (‘`\n`’) at the end.
2. The special ‘`%%b`’ spec can be used to display the buffer name.
   Well, it’s no spec at all, really—‘`%%`’ is just a way to quote
   ‘`%`’ to allow it to pass through the formatting machinery unmangled,
   so that Emacs receives ‘`%b`’, which is something the Emacs mode line
   display interprets to mean “show the buffer name”. For a full list of
   mode line specs Emacs understands, see the documentation of the
   `mode-line-format` variable.

---

Next: [User-Defined Specs](#User_002dDefined-Specs), Previous: [Mode Line Formatting](#Mode-Line-Formatting), Up: [Formatting Variables](#Formatting-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.4.3 Advanced Formatting [¶](#Advanced-Formatting-1)

It is frequently useful to post-process the fields in some way.
Padding, limiting, cutting off parts and suppressing certain values can
be achieved by using *tilde modifiers*. A typical tilde spec might
look like ‘`%~(cut 3)~(ignore "0")y`’.

These are the valid modifiers:

`pad`

`pad-left`
:   Pad the field to the left with spaces until it reaches the required
    length.

`pad-right`
:   Pad the field to the right with spaces until it reaches the required
    length.

`max`

`max-left`
:   Cut off characters from the left until it reaches the specified length.

`max-right`
:   Cut off characters from the right until it reaches the specified
    length.

`cut`

`cut-left`
:   Cut off the specified number of characters from the left.

`cut-right`
:   Cut off the specified number of characters from the right.

`ignore`
:   Return an empty string if the field is equal to the specified value.

`form`
:   Use the specified form as the field value when the ‘`@`’ spec is
    used.

    Here’s an example:

    ```
    "~(form (current-time-string))@"
    ```

Let’s take an example. The ‘`%o`’ spec in the summary mode lines
will return a date in compact ISO8601 format—‘`19960809T230410`’.
This is quite a mouthful, so we want to shave off the century number and
the time, leaving us with a six-character date. That would be
‘`%~(cut-left 2)~(max-right 6)~(pad 6)o`’. (Cutting is done before
maxing, and we need the padding to ensure that the date is never less
than 6 characters to make it look nice in columns.)

Ignoring is done first; then cutting; then maxing; and then as the very
last operation, padding.

---

Next: [Formatting Fonts](#Formatting-Fonts), Previous: [Advanced Formatting](#Advanced-Formatting), Up: [Formatting Variables](#Formatting-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.4.4 User-Defined Specs [¶](#User_002dDefined-Specs-1)

All the specs allow for inserting user defined specifiers—‘`u`’.
The next character in the format string should be a letter. Gnus
will call the function `gnus-user-format-function-`‘`X`’, where
‘`X`’ is the letter following ‘`%u`’. The function will be passed
a single parameter—what the parameter means depends on what buffer
it’s being called from. The function should return a string, which will
be inserted into the buffer just like information from any other
specifier. This function may also be called with dummy values, so it
should protect against that.

Also Gnus supports extended user-defined specs, such as ‘`%u&foo;`’.
Gnus will call the function `gnus-user-format-function-`‘`foo`’.

You can also use tilde modifiers (see [Advanced Formatting](#Advanced-Formatting) to achieve
much the same without defining new functions. Here’s an example:
‘`%~(form (count-lines (point-min) (point)))@`’. The form
given here will be evaluated to yield the current line number, and then
inserted.

---

Next: [Positioning Point](#Positioning-Point), Previous: [User-Defined Specs](#User_002dDefined-Specs), Up: [Formatting Variables](#Formatting-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.4.5 Formatting Fonts [¶](#Formatting-Fonts-1)

There are specs for highlighting, and these are shared by all the format
variables. Text inside the ‘`%(`’ and ‘`%)`’ specifiers will get
the special `mouse-face` property set, which means that it will be
highlighted (with `gnus-mouse-face`) when you put the mouse pointer
over it.

Text inside the ‘`%{`’ and ‘`%}`’ specifiers will have their
normal faces set using `gnus-face-0`, which is `bold` by
default. If you say ‘`%1{`’, you’ll get `gnus-face-1` instead,
and so on. Create as many faces as you wish. The same goes for the
`mouse-face` specs—you can say ‘`%3(hello%)`’ to have
‘`hello`’ mouse-highlighted with `gnus-mouse-face-3`.

Text inside the ‘`%<<`’ and ‘`%>>`’ specifiers will get the
special `balloon-help` property set to
`gnus-balloon-face-0`. If you say ‘`%1<<`’, you’ll get
`gnus-balloon-face-1` and so on. The `gnus-balloon-face-*`
variables should be either strings or symbols naming functions that
return a string. When the mouse passes over text with this property
set, a balloon window will appear and display the string. Please
refer to [Tooltips](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tooltips) in The Emacs Manual for more
information on this. (For technical reasons, the guillemets have been
approximated as ‘`<<`’ and ‘`>>`’ in this paragraph.)

Here’s an alternative recipe for the group buffer:

```
;; Create three face types.
(setq gnus-face-1 'bold)
(setq gnus-face-3 'italic)

;; We want the article count to be in
;; a bold and green face.  So we create
;; a new face called my-green-bold.
(copy-face 'bold 'my-green-bold)
;; Set the color.
(set-face-foreground 'my-green-bold "ForestGreen")
(setq gnus-face-2 'my-green-bold)

;; Set the new & fancy format.
(setq gnus-group-line-format
      "%M%S%3{%5y%}%2[:%] %(%1{%g%}%)\n")
```

I’m sure you’ll be able to use this scheme to create totally unreadable
and extremely vulgar displays. Have fun!

Note that the ‘`%(`’ specs (and friends) do not make any sense on the
mode-line variables.

---

Next: [Tabulation](#Tabulation), Previous: [Formatting Fonts](#Formatting-Fonts), Up: [Formatting Variables](#Formatting-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.4.6 Positioning Point [¶](#Positioning-Point-1)

Gnus usually moves point to a pre-defined place on each line in most
buffers. By default, point move to the first colon character on the
line. You can customize this behavior in three different ways.

You can move the colon character to somewhere else on the line.

You can redefine the function that moves the point to the colon. The
function is called `gnus-goto-colon`.

But perhaps the most convenient way to deal with this, if you don’t want
to have a colon in your line, is to use the ‘`%*`’ specifier. If you
put a ‘`%*`’ somewhere in your format line definition, Gnus will
place point there.

---

Previous: [Positioning Point](#Positioning-Point), Up: [Formatting Variables](#Formatting-Variables)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.4.7 Tabulation [¶](#Tabulation-1)

You can usually line up your displays by padding and cutting your
strings. However, when combining various strings of different size, it
can often be more convenient to just output the strings, and then worry
about lining up the following text afterwards.

To do that, Gnus supplies tabulator specs—‘`%=`’. There are two
different types—*hard tabulators* and *soft tabulators*.

‘`%50=`’ will insert space characters to pad the line up to column
50. If the text is already past column 50, nothing will be inserted.
This is the soft tabulator.

‘`%-50=`’ will insert space characters to pad the line up to column
50. If the text is already past column 50, the excess text past column
50 will be removed. This is the hard tabulator.

---

Next: [Tabbed Interface](#Tabbed-Interface), Previous: [Formatting Variables](#Formatting-Variables), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.5 Window Layout [¶](#Window-Layout-1)

No, there’s nothing here about X, so be quiet.

If `gnus-use-full-window` non-`nil`, Gnus will delete all
other windows and occupy the entire Emacs screen by itself. It is
`t` by default.

Setting this variable to `nil` kinda works, but there are
glitches. Use at your own peril.

`gnus-buffer-configuration` describes how much space each Gnus
buffer should be given. Here’s an excerpt of this variable:

```
((group (vertical 1.0 (group 1.0 point)))
 (article (vertical 1.0 (summary 0.25 point)
                        (article 1.0))))
```

This is an alist. The *key* is a symbol that names some action or
other. For instance, when displaying the group buffer, the window
configuration function will use `group` as the key. A full list of
possible names is listed below.

The *value* (i.e., the *split*) says how much space each buffer
should occupy. To take the `article` split as an example:

```
(article (vertical 1.0 (summary 0.25 point)
                       (article 1.0)))
```

This *split* says that the summary buffer should occupy 25% of upper
half of the screen, and that it is placed over the article buffer. As
you may have noticed, 100% + 25% is actually 125% (yup, I saw y’all
reaching for that calculator there). However, the special number
`1.0` is used to signal that this buffer should soak up all the
rest of the space available after the rest of the buffers have taken
whatever they need. There should be only one buffer with the `1.0`
size spec per split.

Point will be put in the buffer that has the optional third element
`point`. In a `frame` split, the last subsplit having a leaf
split where the tag `frame-focus` is a member (i.e., is the third or
fourth element in the list, depending on whether the `point` tag is
present) gets focus.

Here’s a more complicated example:

```
(article (vertical 1.0 (group 4)
                       (summary 0.25 point)
                       (article 1.0)))
```

If the size spec is an integer instead of a floating point number,
then that number will be used to say how many lines a buffer should
occupy, not a percentage.

If the *split* looks like something that can be `eval`ed (to be
precise—if the `car` of the split is a function or a subr), this
split will be `eval`ed. If the result is non-`nil`, it will
be used as a split.

Not complicated enough for you? Well, try this on for size:

```
(article (horizontal 1.0
             (vertical 0.5
                 (group 1.0))
             (vertical 1.0
                 (summary 0.25 point)
                 (article 1.0))))
```

Whoops. Two buffers with the mystery 100% tag. And what’s that
`horizontal` thingie?

If the first element in one of the split is `horizontal`, Gnus will
split the window horizontally, giving you two windows side-by-side.
Inside each of these strips you may carry on all you like in the normal
fashion. The number following `horizontal` says what percentage of
the screen is to be given to this strip.

For each split, there *must* be one element that has the 100% tag.
The splitting is never accurate, and this buffer will eat any leftover
lines from the splits.

To be slightly more formal, here’s a definition of what a valid split
may look like:

```
split      = frame | horizontal | vertical | buffer | form
frame      = "(frame " size *split ")"
horizontal = "(horizontal " size *split ")"
vertical   = "(vertical " size *split ")"
buffer     = "(" buf-name " " size *[ "point" ] *[ "frame-focus"] ")"
size       = number | frame-params
buf-name   = group | article | summary ...
```

The limitations are that the `frame` split can only appear as the
top-level split. form should be an Emacs Lisp form that should
return a valid split. We see that each split is fully recursive, and
may contain any number of `vertical` and `horizontal` splits.

Finding the right sizes can be a bit complicated. No window may be less
than `gnus-window-min-height` (default 1) characters high, and all
windows must be at least `gnus-window-min-width` (default 1)
characters wide. Gnus will try to enforce this before applying the
splits. If you want to use the normal Emacs window width/height limit,
you can just set these two variables to `nil`.

If you’re not familiar with Emacs terminology, `horizontal` and
`vertical` splits may work the opposite way of what you’d expect.
Windows inside a `horizontal` split are shown side-by-side, and
windows within a `vertical` split are shown above each other.

If you want to experiment with window placement, a good tip is to call
`gnus-configure-frame` directly with a split. This is the function
that does all the real work when splitting buffers. Below is a pretty
nonsensical configuration with 5 windows; two for the group buffer and
three for the article buffer. (I said it was nonsensical.) If you
`eval` the statement below, you can get an idea of how that would
look straight away, without going through the normal Gnus channels.
Play with it until you’re satisfied, and then use
`gnus-add-configuration` to add your new creation to the buffer
configuration list.

```
(gnus-configure-frame
 '(horizontal 1.0
    (vertical 10
      (group 1.0)
      (article 0.3 point))
    (vertical 1.0
      (article 1.0)
      (horizontal 4
        (group 1.0)
        (article 10)))))
```

You might want to have several frames as well. No prob—just use the
`frame` split:

```
(gnus-configure-frame
 '(frame 1.0
         (vertical 1.0
                   (summary 0.25 point frame-focus)
                   (article 1.0))
         (vertical ((height . 5) (width . 15)
                    (user-position . t)
                    (left . -1) (top . 1))
                   (picon 1.0))))
```

This split will result in the familiar summary/article window
configuration in the first (or “main”) frame, while a small additional
frame will be created where picons will be shown. As you can see,
instead of the normal `1.0` top-level spec, each additional split
should have a frame parameter alist as the size spec.
See [Frame Parameters](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Frame-Parameters) in The GNU Emacs Lisp
Reference Manual.
The list of all possible keys for `gnus-buffer-configuration` can
be found in its default value.

Note that the `message` key is used for both
`gnus-group-mail` and `gnus-summary-mail-other-window`. If
it is desirable to distinguish between the two, something like this
might be used:

```
(message (horizontal 1.0
                     (vertical 1.0 (message 1.0 point))
                     (vertical 0.24
                               (if (buffer-live-p gnus-summary-buffer)
                                   '(summary 0.5))
                               (group 1.0))))
```

One common desire for a multiple frame split is to have a separate frame
for composing mail and news while leaving the original frame intact. To
accomplish that, something like the following can be done:

```
(message
  (frame 1.0
         (if (not (buffer-live-p gnus-summary-buffer))
             (car (cdr (assoc 'group gnus-buffer-configuration)))
           (car (cdr (assoc 'summary gnus-buffer-configuration))))
         (vertical ((user-position . t) (top . 1) (left . 1)
                    (name . "Message"))
                   (message 1.0 point))))
```

Since the `gnus-buffer-configuration` variable is so long and
complicated, there’s a function you can use to ease changing the config
of a single setting: `gnus-add-configuration`. If, for instance,
you want to change the `article` setting, you could say:

```
(gnus-add-configuration
 '(article (vertical 1.0
               (group 4)
               (summary .25 point)
               (article 1.0))))
```

You’d typically stick these `gnus-add-configuration` calls in your
`~/.gnus.el` file or in some startup hook—they should be run after
Gnus has been loaded.

If all windows mentioned in the configuration are already visible, Gnus
won’t change the window configuration. If you always want to force the
“right” window configuration, you can set
`gnus-always-force-window-configuration` to non-`nil`.

If you’re using tree displays (see [Tree Display](#Tree-Display)), and the tree
window is displayed vertically next to another window, you may also want
to fiddle with `gnus-tree-minimize-window` to avoid having the
windows resized.

Lastly, it’s possible to make Gnus window layouts “atomic”
(see [Atomic Windows](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Atomic-Windows) in The GNU Emacs Lisp
Reference Manual) by setting `gnus-use-atomic-windows` to
`t`. This will ensure that pop-up buffers (e.g., help or
completion buffers), will appear below or to the side of the entire
Gnus window layout and not, for example, squashed between the summary
and article buffers.

* [Window Configuration Names](#Window-Configuration-Names)
* [Example Window Configurations](#Example-Window-Configurations)

#### 10.5.1 Window Configuration Names [¶](#Window-Configuration-Names)

Here’s a list of most of the currently known window configurations,
and when they’re used:

`group`
:   The group buffer.

`summary`
:   Entering a group and showing only the summary.

`article`
:   Selecting an article.

`server`
:   The server buffer.

`browse`
:   Browsing groups from the server buffer.

`message`
:   Composing a (new) message.

`only-article`
:   Showing only the article buffer.

`edit-article`
:   Editing an article.

`edit-form`
:   Editing group parameters and the like.

`edit-score`
:   Editing a server definition.

`post`
:   Composing a news message.

`reply`
:   Replying or following up an article without yanking the text.

`forward`
:   Forwarding a message.

`reply-yank`
:   Replying or following up an article with yanking the text.

`mail-bound`
:   Bouncing a message.

`pipe`
:   Sending an article to an external process.

`bug`
:   Sending a bug report.

`score-trace`
:   Displaying the score trace.

`score-words`
:   Displaying the score words.

`split-trace`
:   Displaying the split trace.

`compose-bounce`
:   Composing a bounce message.

`mml-preview`
:   Previewing a MIME part.

#### 10.5.2 Example Window Configurations [¶](#Example-Window-Configurations)

* Narrow left hand side occupied by group buffer. Right hand side split
  between summary buffer (top one-sixth) and article buffer (bottom).

  ```
  (gnus-add-configuration
   '(article
     (horizontal 1.0
                 (vertical 25 (group 1.0))
                 (vertical 1.0
                           (summary 0.16 point)
                           (article 1.0)))))

  (gnus-add-configuration
   '(summary
     (horizontal 1.0
                 (vertical 25 (group 1.0))
                 (vertical 1.0 (summary 1.0 point)))))
  ```

---

Next: [Faces and Fonts](#Faces-and-Fonts), Previous: [Window Layout](#Window-Layout), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.6 Tabbed Interface [¶](#Tabbed-Interface-1)

Gnus supports display of different buffers in dedicated tabs
that you can select using the tab bar.

To open the group buffer in a new tab named ‘`Gnus`’, use:

```
(push '("\\`\\*Group\\*\\'" .
        (display-buffer-in-tab
         (tab-name . "Gnus")))
      display-buffer-alist)
```

To read every summary in a separate explicitly named tab, use:

```
(push '("\\`\\*Summary .*\\*\\'" .
        (display-buffer-in-tab
         (tab-name . (lambda (buffer _alist)
                       (setq buffer (buffer-name buffer))
                       (when (string-match "\\`\\*Summary \\(.*\\)\\*\\'" buffer)
                         (format "Group %s" (match-string 1 buffer)))))))
      display-buffer-alist)
```

---

Next: [Mode Lines](#Mode-Lines), Previous: [Tabbed Interface](#Tabbed-Interface), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.7 Faces and Fonts [¶](#Faces-and-Fonts-1)

Fiddling with fonts and faces used to be very difficult, but these days
it is very simple. You simply say `M-x customize-face`, pick out
the face you want to alter, and alter it via the standard Customize
interface.

---

Next: [Highlighting and Menus](#Highlighting-and-Menus), Previous: [Faces and Fonts](#Faces-and-Fonts), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.8 Mode Lines [¶](#Mode-Lines-1)

`gnus-updated-mode-lines` says what buffers should keep their mode
lines updated. It is a list of symbols. Supported symbols include
`group`, `article`, `summary`, `server`,
`browse`, and `tree`. If the corresponding symbol is present,
Gnus will keep that mode line updated with information that may be
pertinent. If this variable is `nil`, screen refresh may be
quicker.

By default, Gnus displays information on the current article in the mode
lines of the summary and article buffers. The information Gnus wishes
to display (e.g., the subject of the article) is often longer than the
mode lines, and therefore have to be cut off at some point. The
`gnus-mode-non-string-length` variable says how long the other
elements on the line is (i.e., the non-info part). If you put
additional elements on the mode line (e.g., a clock), you should modify
this variable:

```
(add-hook 'display-time-hook
          (lambda () (setq gnus-mode-non-string-length
                           (+ 21
                              (if line-number-mode 5 0)
                              (if column-number-mode 4 0)
                              (length display-time-string)))))
```

If this variable is `nil` (which is the default), the mode line
strings won’t be chopped off, and they won’t be padded either. Note
that the default is unlikely to be desirable, as even the percentage
complete in the buffer may be crowded off the mode line; the user should
configure this variable appropriately for her configuration.

---

Next: [Daemons](#Daemons), Previous: [Mode Lines](#Mode-Lines), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.9 Highlighting and Menus [¶](#Highlighting-and-Menus-1)

The `gnus-visual` variable controls most of the Gnus-prettifying
aspects. If `nil`, Gnus won’t attempt to create menus or use fancy
colors or fonts. This will also inhibit loading the `gnus-vis.el`
file.

This variable can be a list of visual properties that are enabled. The
following elements are valid, and are all included by default:

`group-highlight`
:   Do highlights in the group buffer.

`summary-highlight`
:   Do highlights in the summary buffer.

`article-highlight`
:   Do highlights in the article buffer.

`highlight`
:   Turn on highlighting in all buffers.

`group-menu`
:   Create menus in the group buffer.

`summary-menu`
:   Create menus in the summary buffers.

`article-menu`
:   Create menus in the article buffer.

`browse-menu`
:   Create menus in the browse buffer.

`server-menu`
:   Create menus in the server buffer.

`score-menu`
:   Create menus in the score buffers.

`menu`
:   Create menus in all buffers.

So if you only want highlighting in the article buffer and menus in all
buffers, you could say something like:

```
(setq gnus-visual '(article-highlight menu))
```

If you want highlighting only and no menus whatsoever, you’d say:

```
(setq gnus-visual '(highlight))
```

If `gnus-visual` is `t`, highlighting and menus will be used
in all Gnus buffers.

Other general variables that influence the look of all buffers include:

`gnus-mouse-face` [¶](#index-gnus_002dmouse_002dface-1)
:   This is the face (i.e., font) used for mouse highlighting in Gnus. No
    mouse highlights will be done if `gnus-visual` is `nil`.

There are hooks associated with the creation of all the different menus:

`gnus-article-menu-hook` [¶](#index-gnus_002darticle_002dmenu_002dhook)
:   Hook called after creating the article mode menu.

`gnus-group-menu-hook` [¶](#index-gnus_002dgroup_002dmenu_002dhook)
:   Hook called after creating the group mode menu.

`gnus-summary-menu-hook` [¶](#index-gnus_002dsummary_002dmenu_002dhook)
:   Hook called after creating the summary mode menu.

`gnus-server-menu-hook` [¶](#index-gnus_002dserver_002dmenu_002dhook)
:   Hook called after creating the server mode menu.

`gnus-browse-menu-hook` [¶](#index-gnus_002dbrowse_002dmenu_002dhook)
:   Hook called after creating the browse mode menu.

`gnus-score-menu-hook` [¶](#index-gnus_002dscore_002dmenu_002dhook)
:   Hook called after creating the score mode menu.

---

Next: [Notifications](#Notifications), Previous: [Highlighting and Menus](#Highlighting-and-Menus), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.10 Daemons [¶](#Daemons-1)

Gnus, being larger than any program ever written (allegedly), does lots
of strange stuff that you may wish to have done while you’re not
present. For instance, you may want it to check for new mail once in a
while. Or you may want it to close down all connections to all servers
when you leave Emacs idle. And stuff like that.

Gnus will let you do stuff like that by defining various
*handlers*. Each handler consists of three elements: A
function, a time, and an idle parameter.

Here’s an example of a handler that closes connections when Emacs has
been idle for thirty minutes:

```
(gnus-demon-close-connections nil 30)
```

Here’s a handler that scans for PGP headers every hour when
Emacs is idle:

```
(gnus-demon-scan-pgp 60 t)
```

This time parameter and that idle parameter work together
in a strange, but wonderful fashion. Basically, if idle is
`nil`, then the function will be called every time minutes.

If idle is `t`, then the function will be called after
time minutes only if Emacs is idle. So if Emacs is never idle,
the function will never be called. But once Emacs goes idle, the
function will be called every time minutes.

If idle is a number and time is a number, the function will
be called every time minutes only when Emacs has been idle for
idle minutes.

If idle is a number and time is `nil`, the function
will be called once every time Emacs has been idle for idle
minutes.

And if time is a string, it should look like ‘`07:31`’, and
the function will then be called once every day somewhere near that
time. Modified by the idle parameter, of course.

(When I say “minute” here, I really mean `gnus-demon-timestep`
seconds. This is 60 by default. If you change that variable,
all the timings in the handlers will be affected.)

So, if you want to add a handler, you could put something like this in
your `~/.gnus.el` file:

```
(gnus-demon-add-handler 'gnus-demon-close-connections 30 t)
```

Some ready-made functions to do this have been created:
`gnus-demon-add-disconnection`,
`gnus-demon-add-nntp-close-connection`,
`gnus-demon-add-scan-timestamps`, `gnus-demon-add-rescan`, and
`gnus-demon-add-scanmail`. Just put those functions in your
`~/.gnus.el` if you want those abilities.

If you add handlers to `gnus-demon-handlers` directly, you should
run `gnus-demon-init` to make the changes take hold. To cancel all
daemons, you can use the `gnus-demon-cancel` function.

Note that adding daemons can be pretty naughty if you over do it. Adding
functions that scan all news and mail from all servers every two seconds
is a sure-fire way of getting booted off any respectable system. So
behave.

---

Next: [Undo](#Undo), Previous: [Daemons](#Daemons), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.11 Notifications [¶](#Notifications-1)

Gnus provides the `gnus-notifications` package that, combined with
a `gnus-demon` handler, can be used to notify you when fresh news
or mails arrive via desktop notifications. Here’s a basic configuration
to put in your `~/.gnus.el` file:

```
(require 'gnus-notifications)
(add-hook 'gnus-after-getting-new-news-hook #'gnus-notifications)
(gnus-demon-add-handler #'gnus-demon-scan-news 15 3) ;every 15 minutes
```

The above will produce notifications for new arrivals when the
`gnus-after-getting-new-news-hook` hook runs, which is triggered
after retrieving news and mails with the `gnus-demon-scan-news`
procedure. The demon handler is configured to scan news every fifteen
minutes, once Emacs has been idle for three minutes.

> **Important:** `gnus-notifications` will only emit notifications for groups whose
> level is equal or lower than gnus-notifications-minimum-level,
> which defaults to `1`, so make sure to set the level of the groups
> you want to be notified for to `1`, or adjust the value of
> gnus-notifications-minimum-level to your needs.

---

Next: [Predicate Specifiers](#Predicate-Specifiers), Previous: [Notifications](#Notifications), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.12 Undo [¶](#Undo-1)

It is very useful to be able to undo actions one has done. In normal
Emacs buffers, it’s easy enough—you just push the `undo` button.
In Gnus buffers, however, it isn’t that simple.

The things Gnus displays in its buffer is of no value whatsoever to
Gnus—it’s all just data designed to look nice to the user.
Killing a group in the group buffer with `C-k` makes the line
disappear, but that’s just a side-effect of the real action—the
removal of the group in question from the internal Gnus structures.
Undoing something like that can’t be done by the normal Emacs
`undo` function.

Gnus tries to remedy this somewhat by keeping track of what the user
does and coming up with actions that would reverse the actions the user
takes. When the user then presses the `undo` key, Gnus will run
the code to reverse the previous action, or the previous actions.
However, not all actions are easily reversible, so Gnus currently offers
a few key functions to be undoable. These include killing groups,
yanking groups, and changing the list of read articles of groups.
That’s it, really. More functions may be added in the future, but each
added function means an increase in data to be stored, so Gnus will
never be totally undoable.

The undoability is provided by the `gnus-undo-mode` minor mode. It
is used if `gnus-use-undo` is non-`nil`, which is the
default. The `C-M-_` key performs the `gnus-undo`
command, which should feel kinda like the normal Emacs `undo`
command.

---

Next: [Moderation](#Moderation), Previous: [Undo](#Undo), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.13 Predicate Specifiers [¶](#Predicate-Specifiers-1)

Some Gnus variables are *predicate specifiers*. This is a special
form that allows flexible specification of predicates without having
to type all that much.

These specifiers are lists consisting of functions, symbols and lists.

Here’s an example:

```
(or gnus-article-unseen-p
    gnus-article-unread-p)
```

The available symbols are `or`, `and` and `not`. The
functions all take one parameter.

Internally, Gnus calls `gnus-make-predicate` on these specifiers
to create a function that can be called. This input parameter to this
function will be passed along to all the functions in the predicate
specifier.

---

Next: [Fetching a Group](#Fetching-a-Group), Previous: [Predicate Specifiers](#Predicate-Specifiers), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.14 Moderation [¶](#Moderation-1)

If you are a moderator, you can use the `gnus-mdrtn.el` package.
It is not included in the standard Gnus package. Write a mail to
‘`larsi@gnus.org`’ and state what group you moderate, and you’ll
get a copy.

The moderation package is implemented as a minor mode for summary
buffers. Put

```
(add-hook 'gnus-summary-mode-hook 'gnus-moderate)
```

in your `~/.gnus.el` file.

If you are the moderator of ‘`rec.zoofle`’, this is how it’s
supposed to work:

1. You split your incoming mail by matching on
   ‘`Newsgroups:.*rec.zoofle`’, which will put all the to-be-posted
   articles in some mail group—for instance, ‘`nnml:rec.zoofle`’.
2. You enter that group once in a while and post articles using the `e`
   (edit-and-post) or `s` (just send unedited) commands.
3. If, while reading the ‘`rec.zoofle`’ newsgroup, you happen upon some
   articles that weren’t approved by you, you can cancel them with the
   `c` command.

To use moderation mode in these two groups, say:

```
(setq gnus-moderated-list
      "^nnml:rec.zoofle$\\|^rec.zoofle$")
```

---

Next: [Image Enhancements](#Image-Enhancements), Previous: [Moderation](#Moderation), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.15 Fetching a Group [¶](#Fetching-a-Group-1)

It is sometimes convenient to be able to just say “I want to read this
group and I don’t care whether Gnus has been started or not”. This is
perhaps more useful for people who write code than for users, but the
command `gnus-fetch-group` provides this functionality in any case.
It takes the group name as a parameter.

---

Next: [Fuzzy Matching](#Fuzzy-Matching), Previous: [Fetching a Group](#Fetching-a-Group), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.16 Image Enhancements [¶](#Image-Enhancements-1)

Emacs is able to display pictures and stuff, so Gnus has taken
advantage of that.

* [X-Face](#X_002dFace)
* [Face](#Face)
* [Smileys](#Smileys)
* [Picons](#Picons)
* [Gravatars](#Gravatars)

---

Next: [Face](#Face), Up: [Image Enhancements](#Image-Enhancements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.16.1 X-Face [¶](#X_002dFace-1)

`X-Face` headers describe a 48x48 pixel black-and-white (1 bit
depth) image that’s supposed to represent the author of the message.
It seems to be supported by an ever-growing number of mail and news
readers.

Viewing an `X-Face` header either requires an Emacs that has
‘`compface`’ support, or that you
have suitable conversion or display programs installed. If your Emacs
has image support the default action is to display the face before the
`From` header. If there’s no native `X-Face` support, Gnus
will try to convert the `X-Face` header using external programs
from the `pbmplus` package and friends, see below. The
default action under Emacs without image support is to fork off the
`display` program.

On a GNU/Linux system, the `display` program is included in the
ImageMagick package. For external conversion programs look for packages
with names like `netpbm`, `libgr-progs` and `compface`.
On Windows, you may use the packages `netpbm` and `compface`
from <https://gnuwin32.sourceforge.net>. You need to add the
`bin` directory to your `PATH` environment variable.

The variable `gnus-article-x-face-command` controls which programs
are used to display the `X-Face` header. If this variable is a
string, this string will be executed in a sub-shell. If it is a
function, this function will be called with the face as the argument.
If `gnus-article-x-face-too-ugly` (which is a regexp) matches the
`From` header, the face will not be shown.

(Note: `x-face` is used in the variable/function names, not
`xface`).

Face and variable:

`gnus-x-face` [¶](#index-gnus_002dx_002dface)
:   Face to show X-Face. The colors from this face are used as the
    foreground and background colors of the displayed X-Faces. The
    default colors are black and white.

`gnus-face-properties-alist` [¶](#index-gnus_002dface_002dproperties_002dalist)
:   Alist of image types and properties applied to Face (see [Face](#Face)) and
    X-Face images. The default value is `((pbm . (:face gnus-x-face))
    (png . nil))`. Here are examples:

    ```
    ;; Specify the altitude of Face and X-Face images in the From header.
    (setq gnus-face-properties-alist
          '((pbm . (:face gnus-x-face :ascent 80))
            (png . (:ascent 80))))

    ;; Show Face and X-Face images as pressed buttons.
    (setq gnus-face-properties-alist
          '((pbm . (:face gnus-x-face :relief -2))
            (png . (:relief -2))))
    ```

    See [Image Descriptors](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Image-Descriptors) in The Emacs Lisp Reference Manual,
    for the valid properties for various image types. Currently,
    `pbm` is used for X-Face images and `png` is used for Face
    images in Emacs.

If you use posting styles, you can use an `x-face-file` entry in
`gnus-posting-styles`, See [Posting Styles](#Posting-Styles). If you don’t, Gnus
provides a few convenience functions and variables to allow easier
insertion of X-Face headers in outgoing messages. You also need the
above mentioned ImageMagick, netpbm or other image conversion packages
(depending the values of the variables below) for these functions.

`gnus-random-x-face` goes through all the ‘`pbm`’ files in
`gnus-x-face-directory` and picks one at random, and then
converts it to the X-Face format by using the
`gnus-convert-pbm-to-x-face-command` shell command. The
‘`pbm`’ files should be 48x48 pixels big. It returns the X-Face
header data as a string.

`gnus-insert-random-x-face-header` calls
`gnus-random-x-face` and inserts a ‘`X-Face`’ header with the
randomly generated data.

`gnus-x-face-from-file` takes a GIF file as the parameter, and then
converts the file to X-Face format by using the
`gnus-convert-image-to-x-face-command` shell command.

Here’s how you would typically use the first function. Put something
like the following in your `~/.gnus.el` file:

```
(setq message-required-news-headers
      (nconc message-required-news-headers
             (list '(X-Face . gnus-random-x-face))))
```

Using the last function would be something like this:

```
(setq message-required-news-headers
      (nconc message-required-news-headers
             (list '(X-Face . (lambda ()
                                (gnus-x-face-from-file
                                 "~/My-face.gif"))))))
```

---

Next: [Smileys](#Smileys), Previous: [X-Face](#X_002dFace), Up: [Image Enhancements](#Image-Enhancements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.16.2 Face [¶](#Face-1)

`Face` headers are essentially a funkier version of `X-Face`
ones. They describe a 48x48 pixel colored image that’s supposed to
represent the author of the message.

The contents of a `Face` header must be a base64 encoded PNG image.
See <https://quimby.gnus.org/circus/face/> for the precise
specifications.

The `gnus-face-properties-alist` variable affects the appearance of
displayed Face images. See [X-Face](#X_002dFace).

Viewing a `Face` header requires an Emacs that is able to display
PNG images.

Gnus provides a few convenience functions and variables to allow
easier insertion of Face headers in outgoing messages.

`gnus-convert-png-to-face` takes a 48x48 PNG image, no longer than
726 bytes long, and converts it to a face.

`gnus-face-from-file` takes a JPEG file as the parameter, and then
converts the file to Face format by using the
`gnus-convert-image-to-face-command` shell command.

Here’s how you would typically use this function. Put something like the
following in your `~/.gnus.el` file:

```
(setq message-required-news-headers
      (nconc message-required-news-headers
             (list '(Face . (lambda ()
                              (gnus-face-from-file "~/face.jpg"))))))
```

---

Next: [Picons](#Picons), Previous: [Face](#Face), Up: [Image Enhancements](#Image-Enhancements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.16.3 Smileys [¶](#Smileys-1)

*Smiley* is a package separate from Gnus, but since Gnus is
currently the only package that uses Smiley, it is documented here.

In short—to use Smiley in Gnus, put the following in your
`~/.gnus.el` file:

```
(setq gnus-treat-display-smileys t)
```

Smiley maps text smiley faces—‘`:-)`’, ‘`8-)`’, ‘`:-(`’ and
the like—to pictures and displays those instead of the text smiley
faces. The conversion is controlled by a list of regexps that matches
text and maps that to file names.

The alist used is specified by the `smiley-regexp-alist`
variable. The first item in each element is the regexp to be matched;
the second element is the regexp match group that is to be replaced by
the picture; and the third element is the name of the file to be
displayed.

The following variables customize the appearance of the smileys:

`smiley-style` [¶](#index-smiley_002dstyle)
:   Specifies the smiley style. Predefined smiley styles include
    `emoji` (use emoji characters), `low-color` (small 13x14
    pixel, three-color images), `medium` (more colorful images, 16x16
    pixel), and `grayscale` (grayscale images, 14x14 pixel). The
    default depends on the height of the default face.

`smiley-data-directory` [¶](#index-smiley_002ddata_002ddirectory)
:   Where Smiley will look for smiley faces files. You shouldn’t set this
    variable anymore. Customize `smiley-style` instead.

`gnus-smiley-file-types` [¶](#index-gnus_002dsmiley_002dfile_002dtypes)
:   List of suffixes on smiley file names to try.

---

Next: [Gravatars](#Gravatars), Previous: [Smileys](#Smileys), Up: [Image Enhancements](#Image-Enhancements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.16.4 Picons [¶](#Picons-1)

So… You want to slow down your news reader even more! This is a
good way to do so. It’s also a great way to impress people staring
over your shoulder as you read news.

What are Picons? To quote directly from the Picons Web site:

> *Picons* is short for “personal icons”. They’re small,
> constrained images used to represent users and domains on the net,
> organized into databases so that the appropriate image for a given
> e-mail address can be found. Besides users and domains, there are picon
> databases for Usenet newsgroups and weather forecasts. The picons are
> in either monochrome `XBM` format or color `XPM` and
> `GIF` formats.

For instructions on obtaining and installing the picons databases,
point your Web browser at
<https://cs.indiana.edu/ftp/faces/picons/>.

If you are using Debian GNU/Linux, saying ‘`apt-get install
picon-.*`’ will install the picons where Gnus can find them.

To enable displaying picons, simply make sure that
`gnus-picon-databases` points to the directory containing the
Picons databases.

The variable `gnus-picon-style` controls how picons are displayed.
If `inline`, the textual representation is replaced. If
`right`, picons are added right to the textual representation.

The value of the variable `gnus-picon-properties` is a list of
properties applied to picons.

The following variables offer control over where things are located.

`gnus-picon-databases` [¶](#index-gnus_002dpicon_002ddatabases-1)
:   The location of the picons database. This is a list of directories
    containing the `news`, `domains`, `users` (and so on)
    subdirectories. Defaults to `("/usr/lib/picon"
    "/usr/local/faces")`.

`gnus-picon-news-directories` [¶](#index-gnus_002dpicon_002dnews_002ddirectories)
:   List of subdirectories to search in `gnus-picon-databases` for
    newsgroups faces. `("news")` is the default.

`gnus-picon-user-directories` [¶](#index-gnus_002dpicon_002duser_002ddirectories)
:   List of subdirectories to search in `gnus-picon-databases` for user
    faces. `("users" "usenix" "local" "misc")` is the default.

`gnus-picon-domain-directories` [¶](#index-gnus_002dpicon_002ddomain_002ddirectories)
:   List of subdirectories to search in `gnus-picon-databases` for
    domain name faces. Defaults to `("domains")`. Some people may
    want to add ‘`"unknown"`’ to this list.

`gnus-picon-file-types` [¶](#index-gnus_002dpicon_002dfile_002dtypes)
:   Ordered list of suffixes on picon file names to try. Defaults to
    `("xpm" "gif" "xbm")` minus those not built-in your Emacs.

`gnus-picon-inhibit-top-level-domains` [¶](#index-gnus_002dpicon_002dinhibit_002dtop_002dlevel_002ddomains)
:   If non-`nil` (which is the default), don’t display picons for
    things like ‘`.net`’ and ‘`.de`’, which aren’t usually very
    interesting.

---

Previous: [Picons](#Picons), Up: [Image Enhancements](#Image-Enhancements)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.16.5 Gravatars [¶](#Gravatars-1)

A gravatar is an image registered to an e-mail address.

You can submit yours on-line at <https://en.gravatar.com/>.

The following variables offer control over how things are displayed.

`gnus-gravatar-size` [¶](#index-gnus_002dgravatar_002dsize)
:   The size in pixels of gravatars. Gravatars are always square, so one
    number for the size is enough. If `nil`, this defaults to the
    value of `gravatar-size`.

`gnus-gravatar-properties` [¶](#index-gnus_002dgravatar_002dproperties)
:   List of image properties applied to Gravatar images (see [Image
    Descriptors](https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Image-Descriptors) in The Emacs Lisp Reference Manual).

`gnus-gravatar-too-ugly` [¶](#index-gnus_002dgravatar_002dtoo_002dugly)
:   Regexp that matches mail addresses or names of people whose avatars
    should not be displayed, or `nil` to display all avatars. It
    defaults to the value of `gnus-article-x-face-too-ugly`
    (see [X-Face](#X_002dFace)).

If you want to see gravatars in the From field, set:

```
(setq gnus-treat-from-gravatar 'head)
```

If you want to see them in the Cc and To fields, set:

```
(setq gnus-treat-mail-gravatar 'head)
```

* [Tool bar](#Tool-bar)

#### 10.16.5.1 Tool bar [¶](#Tool-bar)

`gnus-group-tool-bar` [¶](#index-gnus_002dgroup_002dtool_002dbar)
:   Specifies the tool bar in the group buffer. It can be either a list
    or a symbol referring to a list.

`gnus-summary-tool-bar` [¶](#index-gnus_002dsummary_002dtool_002dbar)
:   Specifies the tool bar in the summary buffer. It can be either a list
    or a symbol referring to a list.

---

Next: [Thwarting Email Spam](#Thwarting-Email-Spam), Previous: [Image Enhancements](#Image-Enhancements), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.17 Fuzzy Matching [¶](#Fuzzy-Matching-1)

Gnus provides *fuzzy matching* of `Subject` lines when doing
things like scoring, thread gathering and thread comparison.

As opposed to regular expression matching, fuzzy matching is very fuzzy.
It’s so fuzzy that there’s not even a definition of what *fuzziness*
means, and the implementation has changed over time.

Basically, it tries to remove all noise from lines before comparing.
‘`Re:` ’, parenthetical remarks, white space, and so on, are filtered
out of the strings before comparing the results. This often leads to
adequate results—even when faced with strings generated by text
manglers masquerading as newsreaders.

---

Next: [Spam Package](#Spam-Package), Previous: [Fuzzy Matching](#Fuzzy-Matching), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 10.18 Thwarting Email Spam [¶](#Thwarting-Email-Spam-1)

In these last days of the Usenet, commercial vultures are hanging about
and grepping through news like crazy to find email addresses they can
foist off their scams and products to. As a reaction to this, many
people have started putting nonsense addresses into their `From`
lines. I think this is counterproductive—it makes it difficult for
people to send you legitimate mail in response to things you write, as
well as making it difficult to see who wrote what. This rewriting may
perhaps be a bigger menace than the spam itself in the end.

The biggest problem I have with email spam is that it comes in under
false pretenses. I press `g` and Gnus merrily informs me that I
have 10 new emails. I say “Golly gee! Happy is me!” and select the
mail group, only to find two pyramid schemes, seven advertisements
(“New! Miracle tonic for growing full, lustrous hair on your toes!”)
and one mail asking me to repent and find some god.

This is annoying. Here’s what you can do about it.

* [The problem of spam](#The-problem-of-spam)
* [Anti-Spam Basics](#Anti_002dSpam-Basics)
* [SpamAssassin, Vipul’s Razor, DCC, etc](#SpamAssassin)

---

Next: [Anti-Spam Basics](#Anti_002dSpam-Basics), Up: [Thwarting Email Spam](#Thwarting-Email-Spam)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.18.1 The problem of spam [¶](#The-problem-of-spam-1)

Spam comes from a wide variety of sources. It is simply impossible to
dispose of all spam without discarding useful messages.

The simplest approach to filtering spam is filtering, at the mail
server or when you sort through incoming mail. If you get 200 spam
messages per day from ‘`random-address@example.org`’, you block
‘`example.org`’. If you get 200 messages about ‘`VIAGRA`’, you
discard all messages with ‘`VIAGRA`’ in the message. If you get
lots of spam from Bulgaria, for example, you try to filter all mail
from Bulgarian IPs.

This, unfortunately, is a great way to discard legitimate e-mail. The
risks of blocking a whole country (Bulgaria, Norway, Nigeria, China,
etc.) or even a continent (Asia, Africa, Europe, etc.) from contacting
you should be obvious, so don’t do it if you have the choice.

In another instance, the very informative and useful RISKS digest has
been blocked by overzealous mail filters because it **contained**
words that were common in spam messages. Nevertheless, in isolated
cases, with great care, direct filtering of mail can be useful.

Another approach to filtering e-mail is the distributed spam
processing, for instance DCC implements such a system. In essence,
N systems around the world agree that a machine X in
Ghana, Estonia, or California is sending out spam e-mail, and these
N systems enter X or the spam e-mail from X into a
database. The criteria for spam detection vary—it may be the number
of messages sent, the content of the messages, and so on. When a user
of the distributed processing system wants to find out if a message is
spam, he consults one of those N systems.

Distributed spam processing works very well against spammers that send
a large number of messages at once, but it requires the user to set up
fairly complicated checks. There are commercial and free distributed
spam processing systems. Distributed spam processing has its risks as
well. For instance legitimate e-mail senders have been accused of
sending spam, and their web sites and mailing lists have been shut
down for some time because of the incident.

The statistical approach to spam filtering is also popular. It is
based on a statistical analysis of previous spam messages. Usually
the analysis is a simple word frequency count, with perhaps pairs of
words or 3-word combinations thrown into the mix. Statistical
analysis of spam works very well in most of the cases, but it can
classify legitimate e-mail as spam in some cases. It takes time to
run the analysis, the full message must be analyzed, and the user has
to store the database of spam analysis. Statistical analysis on the
server is gaining popularity. This has the advantage of letting the
user Just Read Mail, but has the disadvantage that it’s harder to tell
the server that it has misclassified mail.

Fighting spam is not easy, no matter what anyone says. There is no
magic switch that will distinguish Viagra ads from Mom’s e-mails.
Even people are having a hard time telling spam apart from non-spam,
because spammers are actively looking to fool us into thinking they
are Mom, essentially. Spamming is irritating, irresponsible, and
idiotic behavior from a bunch of people who think the world owes them
a favor. We hope the following sections will help you in fighting the
spam plague.

---

Next: [SpamAssassin, Vipul’s Razor, DCC, etc](#SpamAssassin), Previous: [The problem of spam](#The-problem-of-spam), Up: [Thwarting Email Spam](#Thwarting-Email-Spam)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.18.2 Anti-Spam Basics [¶](#Anti_002dSpam-Basics-1)

One way of dealing with spam is having Gnus split out all spam into a
‘`spam`’ mail group (see [Splitting Mail](#Splitting-Mail)).

First, pick one (1) valid mail address that you can be reached at, and
put it in your `From` header of all your news articles. (I’ve
chosen ‘`larsi@trym.ifi.uio.no`’, but for many addresses on the form
‘`larsi+usenet@ifi.uio.no`’ will be a better choice. Ask your
sysadmin whether your sendmail installation accepts keywords in the local
part of the mail address.)

```
(setq message-default-news-headers
      "From: Lars Magne Ingebrigtsen <larsi@trym.ifi.uio.no>\n")
```

Then put the following split rule in `nnmail-split-fancy`
(see [Fancy Mail Splitting](#Fancy-Mail-Splitting)):

```
(...
 (to "larsi@trym.ifi.uio.no"
     (| ("subject" "re:.*" "misc")
        ("references" ".*@.*" "misc")
        "spam"))
 ...)
```

This says that all mail to this address is suspect, but if it has a
`Subject` that starts with a ‘`Re:`’ or has a `References`
header, it’s probably ok. All the rest goes to the ‘`spam`’ group.
(This idea probably comes from Tim Pierce.)

In addition, many mail spammers talk directly to your SMTP server
and do not include your email address explicitly in the `To`
header. Why they do this is unknown—perhaps it’s to thwart this
thwarting scheme? In any case, this is trivial to deal with—you just
put anything not addressed to you in the ‘`spam`’ group by ending
your fancy split rule in this way:

```
(
 ...
 (to "larsi" "misc")
 "spam")
```

In my experience, this will sort virtually everything into the right
group. You still have to check the ‘`spam`’ group from time to time to
check for legitimate mail, though. If you feel like being a good net
citizen, you can even send off complaints to the proper authorities on
each spam—at your leisure.

This works for me. It allows people an easy way to contact me (they can
just press `r` in the usual way), and I’m not bothered at all with
spam. It’s a win-win situation. Forging `From` headers to point
to non-existent domains is yucky, in my opinion.

Be careful with this approach. Spammers are wise to it.

---

Previous: [Anti-Spam Basics](#Anti_002dSpam-Basics), Up: [Thwarting Email Spam](#Thwarting-Email-Spam)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 10.18.3 SpamAssassin, Vipul’s Razor, DCC, etc [¶](#SpamAssassin_002c-Vipul_0027s-Razor_002c-DCC_002c-etc)

The days where the hints in the previous section were sufficient to
avoid spam are over. There are many tools out there
that claim to reduce the amount of spam you get. This section could
easily become outdated fast, as new products replace old, but
fortunately most of these tools seem to have similar interfaces. Even
though this section will use SpamAssassin as an example, it should be
easy to adapt it to most other tools.

Note that this section does not involve the `spam.el` package,
which is discussed in the next section. If you don’t care for all
the features of `spam.el`, you can make do with these simple
recipes.

If the tool you are using is not installed on the mail server, you
need to invoke it yourself. Ideas on how to use the
`:postscript` mail source parameter (see [Mail Source Specifiers](#Mail-Source-Specifiers)) follow.

```
(setq mail-sources
      '((file :prescript "formail -bs spamassassin < /var/mail/%u")
        (pop :user "jrl"
             :server "pophost"
             :postscript
             "mv %t /tmp/foo; formail -bs spamc < /tmp/foo > %t")))
```

Once you manage to process your incoming spool somehow, thus making
the mail contain, e.g., a header indicating it is spam, you are ready to
filter it out. Using normal split methods (see [Splitting Mail](#Splitting-Mail)):

```
(setq nnmail-split-methods '(("spam"  "^X-Spam-Flag: YES")
                             ...))
```

Or using fancy split methods (see [Fancy Mail Splitting](#Fancy-Mail-Splitting)):

```
(setq nnmail-split-methods 'nnmail-split-fancy
      nnmail-split-fancy '(| ("X-Spam-Flag" "YES" "spam")
                             ...))
```

Some people might not like the idea of piping the mail through various
programs using a `:prescript` (if some program is buggy, you
might lose all mail). If you are one of them, another solution is to
call the external tools during splitting. Example fancy split method:

```
(setq nnmail-split-fancy '(| (: kevin-spamassassin)
                             ...))
(defun kevin-spamassassin ()
  (save-excursion
    (save-restriction
      (widen)
      (if (eq 1 (call-process-region (point-min) (point-max)
                                     "spamc" nil nil nil "-c"))
          "spam"))))
```

Note that with the nnimap back end, message bodies will not be
downloaded by default. You need to set
`nnimap-split-download-body` to `t` to do that
(see [Client-Side IMAP Splitting](#Client_002dSide-IMAP-Splitting)).

That is about it. As some spam is likely to get through anyway, you
might want to have a nifty function to call when you happen to read
spam. And here is the nifty function:

```
(defun my-gnus-raze-spam ()
  "Submit SPAM to Vipul's Razor, then mark it as expirable."
  (interactive)
  (gnus-summary-save-in-pipe "razor-report -f -d" t)
  (gnus-summary-mark-as-expirable 1))
```

---

Next: [The Gnus Registry](#The-Gnus-Registry), Previous: [Thwarting Email Spam](#Thwarting-Email-Spam), Up: [Various](#Various)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
