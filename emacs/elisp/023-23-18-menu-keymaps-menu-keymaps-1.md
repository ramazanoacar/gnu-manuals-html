### 23.18 Menu Keymaps [¶](#Menu-Keymaps-1)

A keymap can operate as a menu as well as defining bindings for
keyboard keys and mouse buttons. Menus are usually actuated with the
mouse, but they can function with the keyboard also. If a menu keymap
is active for the next input event, that activates the keyboard menu
feature.

* [Defining Menus](#Defining-Menus)
* [Menus and the Mouse](#Mouse-Menus)
* [Menus and the Keyboard](#Keyboard-Menus)
* [Menu Example](#Menu-Example)
* [The Menu Bar](#Menu-Bar)
* [Tool bars](#Tool-Bar)
* [Modifying Menus](#Modifying-Menus)
* [Easy Menu](#Easy-Menu)

---

Next: [Menus and the Mouse](#Mouse-Menus), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.1 Defining Menus [¶](#Defining-Menus-1)

A keymap acts as a menu if it has an *overall prompt string*,
which is a string that appears as an element of the keymap.
(See [Format of Keymaps](#Format-of-Keymaps).) The string should describe the purpose of
the menu’s commands. Emacs displays the overall prompt string as the
menu title in some cases, depending on the toolkit (if any) used for
displaying menus.[21](#FOOT21) Keyboard menus also display the
overall prompt string.

The easiest way to construct a keymap with a prompt string is to
specify the string as an argument when you call `make-keymap`,
`make-sparse-keymap` (see [Creating Keymaps](#Creating-Keymaps)), or
`define-prefix-command` (see [Definition of define-prefix-command](#Definition-of-define_002dprefix_002dcommand)). If you do not want the keymap to operate as
a menu, don’t specify a prompt string for it.

Function: **keymap-prompt** keymap [¶](#index-keymap_002dprompt)
:   This function returns the overall prompt string of keymap,
    or `nil` if it has none.

The menu’s items are the bindings in the keymap. Each binding
associates an event type to a definition, but the event types have no
significance for the menu appearance. (Usually we use pseudo-events,
symbols that the keyboard cannot generate, as the event types for menu
item bindings.) The menu is generated entirely from the bindings that
correspond in the keymap to these events.

The order of items in the menu is the same as the order of bindings in
the keymap. Since `define-key` puts new bindings at the front, you
should define the menu items starting at the bottom of the menu and
moving to the top, if you care about the order. When you add an item to
an existing menu, you can specify its position in the menu using
`keymap-set-after` (see [Modifying Menus](#Modifying-Menus)).

* [Simple Menu Items](#Simple-Menu-Items)
* [Extended Menu Items](#Extended-Menu-Items)
* [Menu Separators](#Menu-Separators)
* [Alias Menu Items](#Alias-Menu-Items)

---

Next: [Extended Menu Items](#Extended-Menu-Items), Up: [Defining Menus](#Defining-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.1.1 Simple Menu Items [¶](#Simple-Menu-Items-1)

The simpler (and original) way to define a menu item is to bind some
event type (it doesn’t matter what event type) to a binding like this:

```
(item-string . real-binding)
```

The CAR, item-string, is the string to be displayed in the
menu. It should be short—preferably one to three words. It should
describe the action of the command it corresponds to. Note that not
all graphical toolkits can display non-ASCII text in menus
(it will work for keyboard menus and will work to a large extent with
the GTK+ toolkit).

You can also supply a second string, called the help string, as follows:

```
(item-string help . real-binding)
```

help specifies a help-echo string to display while the mouse
is on that item in the same way as `help-echo` text properties
(see [Help display](#Help-display)).

As far as `define-key` is concerned, item-string and
help-string are part of the event’s binding. However,
`lookup-key` returns just real-binding, and only
real-binding is used for executing the key.

If real-binding is `nil`, then item-string appears in
the menu but cannot be selected.

If real-binding is a symbol and has a non-`nil`
`menu-enable` property, that property is an expression that
controls whether the menu item is enabled. Every time the keymap is
used to display a menu, Emacs evaluates the expression, and it enables
the menu item only if the expression’s value is non-`nil`. When a
menu item is disabled, it is displayed in a fuzzy fashion, and
cannot be selected.

The menu bar does not recalculate which items are enabled every time you
look at a menu. This is because the X toolkit requires the whole tree
of menus in advance. To force recalculation of the menu bar, call
`force-mode-line-update` (see [Mode Line Format](#Mode-Line-Format)).

---

Next: [Menu Separators](#Menu-Separators), Previous: [Simple Menu Items](#Simple-Menu-Items), Up: [Defining Menus](#Defining-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.1.2 Extended Menu Items [¶](#Extended-Menu-Items-1)

An extended-format menu item is a more flexible and also cleaner
alternative to the simple format. You define an event type with a
binding that’s a list starting with the symbol `menu-item`.
For a non-selectable string, the binding looks like this:

```
(menu-item item-name)
```

A string starting with two or more dashes specifies a separator line;
see [Menu Separators](#Menu-Separators).

To define a real menu item which can be selected, the extended format
binding looks like this:

```
(menu-item item-name real-binding
    . item-property-list)
```

Here, item-name is an expression which evaluates to the menu item
string. Thus, the string need not be a constant.

The third element, real-binding, can be the command to execute
(in which case you get a normal menu item). It can also be a keymap,
which will result in a submenu, and item-name is used as the
submenu name. Finally, it can be `nil`, in which case you will
get a non-selectable menu item. This is mostly useful when creating
separator lines and the like.

The tail of the list, item-property-list, has the form of a
property list (see [Property Lists](#Property-Lists)) which contains other
information.

Here is a table of the properties that are supported:

`:enable form`
:   The result of evaluating form determines whether the item is
    enabled (non-`nil` means yes). If the item is not enabled,
    you can’t really click on it.

`:visible form`
:   The result of evaluating form determines whether the item should
    actually appear in the menu (non-`nil` means yes). If the item
    does not appear, then the menu is displayed as if this item were
    not defined at all.

`:help help`
:   The value of this property, help, specifies a help-echo string
    to display while the mouse is on that item. This is displayed in the
    same way as `help-echo` text properties (see [Help display](#Help-display)).
    Note that this must be a constant string, unlike the `help-echo`
    property for text and overlays.

`:button (type . selected)`
:   This property provides a way to define radio buttons and toggle buttons.
    The CAR, type, says which: it should be `:toggle` or
    `:radio`. The CDR, selected, should be a form; the
    result of evaluating it says whether this button is currently selected.

    A *toggle* is a menu item which is labeled as either on or off
    according to the value of selected. The command itself should
    toggle selected, setting it to `t` if it is `nil`,
    and to `nil` if it is `t`. Here is how the menu item
    to toggle the `debug-on-error` flag is defined:

    ```
    (menu-item "Debug on Error" toggle-debug-on-error
               :button (:toggle
                        . (and (boundp 'debug-on-error)
                               debug-on-error)))
    ```

    This works because `toggle-debug-on-error` is defined as a command
    which toggles the variable `debug-on-error`.

    *Radio buttons* are a group of menu items, in which at any time one
    and only one is selected. There should be a variable whose value
    says which one is selected at any time. The selected form for
    each radio button in the group should check whether the variable has the
    right value for selecting that button. Clicking on the button should
    set the variable so that the button you clicked on becomes selected.

`:key-sequence key-sequence`
:   This property specifies which key sequence to display as keyboard equivalent.
    Before Emacs displays key-sequence in the menu, it verifies that
    key-sequence is really equivalent to this menu item, so it only
    has an effect if you specify a correct key sequence.
    Specifying `nil` for key-sequence is equivalent to the
    `:key-sequence` attribute being absent.

`:keys string`
:   This property specifies that string is the string to display
    as the keyboard equivalent for this menu item. You can use
    the ‘`\\[...]`’ documentation construct in string.

    This property can also be a function (which will be called with no
    arguments). This function should return a string. This function will
    be called every time the menu is computed, so using a function that
    takes a lot of time to compute is not a good idea, and it should
    expect to be called from any context.

`:filter filter-fn`
:   This property provides a way to compute the menu item dynamically.
    The property value filter-fn should be a function of one argument;
    when it is called, its argument will be real-binding. The
    function should return the binding to use instead.

    Emacs can call this function at any time that it does redisplay or
    operates on menu data structures, so you should write it so it can
    safely be called at any time.

`:wrap wrap-p`
:   If wrap-p is non-`nil` inside a tool bar, the menu item is
    not displayed, but instead causes subsequent items to be displayed on
    a new line. This is not supported when Emacs uses the GTK+ or
    Nextstep toolkits.

---

Next: [Alias Menu Items](#Alias-Menu-Items), Previous: [Extended Menu Items](#Extended-Menu-Items), Up: [Defining Menus](#Defining-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.1.3 Menu Separators [¶](#Menu-Separators-1)

A menu separator is a kind of menu item that doesn’t display any
text—instead, it divides the menu into subparts with a horizontal line.
A separator looks like this in the menu keymap:

```
(menu-item separator-type)
```

where separator-type is a string starting with two or more dashes.

In the simplest case, separator-type consists of only dashes.
That specifies the default kind of separator. (For compatibility,
`""` and `-` also count as separators.)

Certain other values of separator-type specify a different
style of separator. Here is a table of them:

`"--no-line"`

`"--space"`
:   An extra vertical space, with no actual line.

`"--single-line"`
:   A single line in the menu’s foreground color.

`"--double-line"`
:   A double line in the menu’s foreground color.

`"--single-dashed-line"`
:   A single dashed line in the menu’s foreground color.

`"--double-dashed-line"`
:   A double dashed line in the menu’s foreground color.

`"--shadow-etched-in"`
:   A single line with a 3D sunken appearance. This is the default,
    used separators consisting of dashes only.

`"--shadow-etched-out"`
:   A single line with a 3D raised appearance.

`"--shadow-etched-in-dash"`
:   A single dashed line with a 3D sunken appearance.

`"--shadow-etched-out-dash"`
:   A single dashed line with a 3D raised appearance.

`"--shadow-double-etched-in"`
:   Two lines with a 3D sunken appearance.

`"--shadow-double-etched-out"`
:   Two lines with a 3D raised appearance.

`"--shadow-double-etched-in-dash"`
:   Two dashed lines with a 3D sunken appearance.

`"--shadow-double-etched-out-dash"`
:   Two dashed lines with a 3D raised appearance.

You can also give these names in another style, adding a colon after
the double-dash and replacing each single dash with capitalization of
the following word. Thus, `"--:singleLine"`, is equivalent to
`"--single-line"`.

You can use a longer form to specify keywords such as `:enable`
and `:visible` for a menu separator:

`(menu-item separator-type nil . item-property-list)`

For example:

```
(menu-item "--" nil :visible (boundp 'foo))
```

Some systems and display toolkits don’t really handle all of these
separator types. If you use a type that isn’t supported, the menu
displays a similar kind of separator that is supported.

---

Previous: [Menu Separators](#Menu-Separators), Up: [Defining Menus](#Defining-Menus)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.1.4 Alias Menu Items [¶](#Alias-Menu-Items-1)

Sometimes it is useful to make menu items that use the same
command but with different enable conditions. The best way to do this
in Emacs now is with extended menu items; before that feature existed,
it could be done by defining alias commands and using them in menu
items. Here’s an example that makes two aliases for
`read-only-mode` and gives them different enable conditions:

```
(defalias 'make-read-only 'read-only-mode)
(put 'make-read-only 'menu-enable '(not buffer-read-only))
(defalias 'make-writable 'read-only-mode)
(put 'make-writable 'menu-enable 'buffer-read-only)
```

When using aliases in menus, often it is useful to display the
equivalent key bindings for the real command name, not the aliases
(which typically don’t have any key bindings except for the menu
itself). To request this, give the alias symbol a non-`nil`
`menu-alias` property. Thus,

```
(put 'make-read-only 'menu-alias t)
(put 'make-writable 'menu-alias t)
```

causes menu items for `make-read-only` and `make-writable` to
show the keyboard bindings for `read-only-mode`.

---

Next: [Menus and the Keyboard](#Keyboard-Menus), Previous: [Defining Menus](#Defining-Menus), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.2 Menus and the Mouse [¶](#Menus-and-the-Mouse)

The usual way to make a menu keymap produce a menu is to make it the
definition of a prefix key. (A Lisp program can explicitly pop up a
menu and receive the user’s choice—see [Pop-Up Menus](#Pop_002dUp-Menus).)

If the prefix key ends with a mouse event, Emacs handles the menu keymap
by popping up a visible menu, so that the user can select a choice with
the mouse. When the user clicks on a menu item, the event generated is
whatever character or symbol has the binding that brought about that
menu item. (A menu item may generate a series of events if the menu has
multiple levels or comes from the menu bar.)

It’s often best to use a button-down event to trigger the menu. Then
the user can select a menu item by releasing the button.

If the menu keymap contains a binding to a nested keymap, the nested
keymap specifies a *submenu*. There will be a menu item, labeled
by the nested keymap’s item string, and clicking on this item
automatically pops up the specified submenu. As a special exception,
if the menu keymap contains a single nested keymap and no other menu
items, the menu shows the contents of the nested keymap directly, not
as a submenu.

However, if Emacs is compiled without X toolkit support, or on text
terminals, submenus are not supported. Each nested keymap is shown as
a menu item, but clicking on it does not automatically pop up the
submenu. If you wish to imitate the effect of submenus, you can do
that by giving a nested keymap an item string which starts with
‘`@`’. This causes Emacs to display the nested keymap using a
separate *menu pane*; the rest of the item string after the
‘`@`’ is the pane label. If Emacs is compiled without X toolkit
support, or if a menu is displayed on a text terminal, menu panes are
not used; in that case, a ‘`@`’ at the beginning of an item string
is omitted when the menu label is displayed, and has no other effect.

---

Next: [Menu Example](#Menu-Example), Previous: [Menus and the Mouse](#Mouse-Menus), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.3 Menus and the Keyboard [¶](#Menus-and-the-Keyboard)

When a prefix key ending with a keyboard event (a character or
function key) has a definition that is a menu keymap, the keymap
operates as a keyboard menu; the user specifies the next event by
choosing a menu item with the keyboard.

Emacs displays the keyboard menu with the map’s overall prompt
string, followed by the alternatives (the item strings of the map’s
bindings), in the echo area. If the bindings don’t all fit at once,
the user can type `SPC` to see the next line of alternatives.
Successive uses of `SPC` eventually get to the end of the menu and
then cycle around to the beginning. (The variable
`menu-prompt-more-char` specifies which character is used for
this; `SPC` is the default.)

When the user has found the desired alternative from the menu, he or
she should type the corresponding character—the one whose binding is
that alternative.

Variable: **menu-prompt-more-char** [¶](#index-menu_002dprompt_002dmore_002dchar)
:   This variable specifies the character to use to ask to see
    the next line of a menu. Its initial value is 32, the code
    for `SPC`.

---

Next: [The Menu Bar](#Menu-Bar), Previous: [Menus and the Keyboard](#Keyboard-Menus), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.4 Menu Example [¶](#Menu-Example-1)

Here is a complete example of defining a menu keymap. It is the
definition of the ‘`Replace`’ submenu in the ‘`Edit`’ menu in
the menu bar, and it uses the extended menu item format
(see [Extended Menu Items](#Extended-Menu-Items)). First we create the keymap, and give
it a name:

```
(defvar menu-bar-replace-menu (make-sparse-keymap "Replace"))
```

Next we define the menu items:

```
(define-key menu-bar-replace-menu [tags-repl-continue]
  '(menu-item "Continue Replace" multifile-continue
              :help "Continue last tags replace operation"))
(define-key menu-bar-replace-menu [tags-repl]
  '(menu-item "Replace in tagged files" tags-query-replace
              :help "Interactively replace a regexp in all tagged files"))
(define-key menu-bar-replace-menu [separator-replace-tags]
  '(menu-item "--"))
;; ...
```

Note the symbols which the bindings are made for; these appear
inside square brackets, in the key sequence being defined. In some
cases, this symbol is the same as the command name; sometimes it is
different. These symbols are treated as function keys, but they are
not real function keys on the keyboard. They do not affect the
functioning of the menu itself, but they are echoed in the echo area
when the user selects from the menu, and they appear in the output of
`where-is` and `apropos`.

The menu in this example is intended for use with the mouse. If a
menu is intended for use with the keyboard, that is, if it is bound to
a key sequence ending with a keyboard event, then the menu items
should be bound to characters or real function keys, that can be
typed with the keyboard.

The binding whose definition is `("--")` is a separator line.
Like a real menu item, the separator has a key symbol, in this case
`separator-replace-tags`. If one menu has two separators, they
must have two different key symbols.

Here is how we make this menu appear as an item in the parent menu:

```
(define-key menu-bar-edit-menu [replace]
  (list 'menu-item "Replace" menu-bar-replace-menu))
```

Note that this incorporates the submenu keymap, which is the value of
the variable `menu-bar-replace-menu`, rather than the symbol
`menu-bar-replace-menu` itself. Using that symbol in the parent
menu item would be meaningless because `menu-bar-replace-menu` is
not a command.

If you wanted to attach the same replace menu to a mouse click, you
can do it this way:

```
(define-key global-map [C-S-down-mouse-1]
   menu-bar-replace-menu)
```

---

Next: [Tool bars](#Tool-Bar), Previous: [Menu Example](#Menu-Example), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.5 The Menu Bar [¶](#The-Menu-Bar)

Emacs usually shows a *menu bar* at the top of each frame.
See [Menu Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Menu-Bars) in The GNU Emacs Manual. Menu bar items are
subcommands of the fake function key `MENU-BAR`, as defined
in the active keymaps.

To add an item to the menu bar, invent a fake function key of your
own (let’s call it key), and make a binding for the key sequence
`[menu-bar key]`. Most often, the binding is a menu keymap,
so that pressing a button on the menu bar item leads to another menu.

When more than one active keymap defines the same function key
for the menu bar, the item appears just once. If the user clicks on
that menu bar item, it brings up a single, combined menu containing
all the subcommands of that item—the global subcommands, the local
subcommands, and the minor mode subcommands.

The variable `overriding-local-map` is normally ignored when
determining the menu bar contents. That is, the menu bar is computed
from the keymaps that would be active if `overriding-local-map`
were `nil`. See [Active Keymaps](#Active-Keymaps).

Here’s an example of setting up a menu bar item:

```
;; Make a menu keymap (with a prompt string)
;; and make it the menu bar item’s definition.
(define-key global-map [menu-bar words]
  (cons "Words" (make-sparse-keymap "Words")))
```

```

```

```
;; Define specific subcommands in this menu.
(define-key global-map
  [menu-bar words forward]
  '("Forward word" . forward-word))
```

```
(define-key global-map
  [menu-bar words backward]
  '("Backward word" . backward-word))
```

A local keymap can cancel a menu bar item made by the global keymap by
rebinding the same fake function key with `undefined` as the
binding. For example, this is how Dired suppresses the ‘`Edit`’ menu
bar item:

```
(define-key dired-mode-map [menu-bar edit] 'undefined)
```

Here, `edit` is the symbol produced by a fake function key, it is
used by the global map for the ‘`Edit`’ menu bar item. The main
reason to suppress a global menu bar item is to regain space for
mode-specific items.

Variable: **menu-bar-final-items** [¶](#index-menu_002dbar_002dfinal_002ditems)
:   Normally the menu bar shows global items followed by items defined by the
    local maps.

    This variable holds a list of fake function keys for items to display at
    the end of the menu bar rather than in normal sequence. The default
    value is `(help-menu)`; thus, the ‘`Help`’ menu item normally appears
    at the end of the menu bar, following local menu items.

Variable: **menu-bar-update-hook** [¶](#index-menu_002dbar_002dupdate_002dhook)
:   This normal hook is run by redisplay to update the menu bar contents,
    before redisplaying the menu bar. You can use it to update menus
    whose contents should vary. Since this hook is run frequently, we
    advise you to ensure that the functions it calls do not take much time
    in the usual case.

Next to every menu bar item, Emacs displays a key binding that runs
the same command (if such a key binding exists). This serves as a
convenient hint for users who do not know the key binding. If a
command has multiple bindings, Emacs normally displays the first one
it finds. You can specify one particular key binding by assigning an
`:advertised-binding` symbol property to the command. See [Substituting Key Bindings in Documentation](#Keys-in-Documentation).

---

Next: [Modifying Menus](#Modifying-Menus), Previous: [The Menu Bar](#Menu-Bar), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.6 Tool bars [¶](#Tool-bars)

A *tool bar* is a row of clickable icons at the top of a frame,
just below the menu bar. See [Tool Bars](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tool-Bars) in The GNU Emacs
Manual. Emacs normally shows a tool bar on graphical displays.

On each frame, the frame parameter `tool-bar-lines` controls
how many lines’ worth of height to reserve for the tool bar. A zero
value suppresses the tool bar. If the value is nonzero, and
`auto-resize-tool-bars` is non-`nil`, the tool bar expands
and contracts automatically as needed to hold the specified contents.
If the value is `grow-only`, the tool bar expands automatically,
but does not contract automatically.

The tool bar contents are controlled by a menu keymap attached to a
fake function key called `TOOL-BAR` (much like the way the menu
bar is controlled). So you define a tool bar item using
`define-key`, like this:

```
(define-key global-map [tool-bar key] item)
```

where key is a fake function key to distinguish this item from
other items, and item is a menu item key binding (see [Extended Menu Items](#Extended-Menu-Items)), which says how to display this item and how it behaves.

The usual menu keymap item properties, `:visible`,
`:enable`, `:button`, and `:filter`, are useful in
tool bar bindings and have their normal meanings. The real-binding
in the item must be a command, not a keymap; in other words, it does not
work to define a tool bar icon as a prefix key.

The `:help` property specifies a help-echo string to display
while the mouse is on that item. This is displayed in the same way as
`help-echo` text properties (see [Help display](#Help-display)).

In addition, you should use the `:image` property;
this is how you specify the image to display in the tool bar:

`:image image`
:   image is either a single image specification (see [Images](#Images)) or
    a vector of four image specifications. If you use a vector of four,
    one of them is used, depending on circumstances:

    item 0
    :   Used when the item is enabled and selected.

    item 1
    :   Used when the item is enabled and deselected.

    item 2
    :   Used when the item is disabled and selected.

    item 3
    :   Used when the item is disabled and deselected.

The GTK+ and NS versions of Emacs ignores items 1 to 3, because disabled and/or
deselected images are autocomputed from item 0.

If image is a single image specification, Emacs draws the tool bar
button in disabled state by applying an edge-detection algorithm to the
image.

The `:rtl` property specifies an alternative image to use for
right-to-left languages. Only the GTK+ version of Emacs supports this
at present.

Some toolkits display both an image and a text in the toolbar. If you
want to force using only the image, use a `:vert-only`
non-`nil` property.

Like the menu bar, the tool bar can display separators (see [Menu Separators](#Menu-Separators)). Tool bar separators are vertical rather than
horizontal, though, and only a single style is supported. They are
represented in the tool bar keymap by `(menu-item "--")` entries;
properties like `:visible` are not supported for tool bar
separators. Separators are rendered natively in GTK+ and Nextstep
tool bars; in the other cases, they are rendered using an image of a
vertical line.

The default tool bar is defined so that items specific to editing do not
appear for major modes whose command symbol has a `mode-class`
property of `special` (see [Major Mode Conventions](#Major-Mode-Conventions)). Major
modes may add items to the global bar by binding `[tool-bar
foo]` in their local map. It makes sense for some major modes to
replace the default tool bar items completely, since not many can be
accommodated conveniently, and the default bindings make this easy by
using an indirection through `tool-bar-map`.

Variable: **tool-bar-map** [¶](#index-tool_002dbar_002dmap)
:   By default, the global map binds `[tool-bar]` as follows:

    ```
    (keymap-global-set "<tool-bar>"
                       '(menu-item "tool bar" ignore
                                   :filter tool-bar-make-keymap))
    ```

    The function `tool-bar-make-keymap`, in turn, derives the actual
    tool bar map dynamically from the value of the variable
    `tool-bar-map`. Hence, you should normally adjust the default
    (global) tool bar by changing that map. Some major modes, such as
    Info mode, completely replace the global tool bar by making
    `tool-bar-map` buffer-local and setting it to a different keymap.

There are two convenience functions for defining tool bar items, as
follows.

Function: **tool-bar-add-item** icon def key &rest props [¶](#index-tool_002dbar_002dadd_002ditem)
:   This function adds an item to the tool bar by modifying
    `tool-bar-map`. The image to use is defined by icon, which
    is the base name of an XPM, XBM or PBM image file to be located by
    `find-image`. Given a value ‘`"exit"`’, say, `exit.xpm`,
    `exit.pbm` and `exit.xbm` would be searched for in that order
    on a color display. On a monochrome display, the search order is
    ‘`.pbm`’, ‘`.xbm`’ and ‘`.xpm`’. The binding to use is the
    command def, and key is the fake function key symbol in the
    prefix keymap. The remaining arguments props are additional
    property list elements to add to the menu item specification.

    To define items in some local map, bind `tool-bar-map` with
    `let` around calls of this function:

    ```
    (defvar foo-tool-bar-map
      (let ((tool-bar-map (make-sparse-keymap)))
        (tool-bar-add-item ...)
        ...
        tool-bar-map))
    ```

Function: **tool-bar-add-item-from-menu** command icon &optional map &rest props [¶](#index-tool_002dbar_002dadd_002ditem_002dfrom_002dmenu)
:   This function is a convenience for defining tool bar items which are
    consistent with existing menu bar bindings. The binding of
    command is looked up in the menu bar in map (default
    `global-map`) and modified to add an image specification for
    icon, which is found in the same way as by
    `tool-bar-add-item`. The resulting binding is then placed in
    `tool-bar-map`, so use this function only for global tool bar
    items.

    map must contain an appropriate keymap bound to
    `[menu-bar]`. The remaining arguments props are additional
    property list elements to add to the menu item specification.

Function: **tool-bar-local-item-from-menu** command icon in-map &optional from-map &rest props [¶](#index-tool_002dbar_002dlocal_002ditem_002dfrom_002dmenu)
:   This function is used for making non-global tool bar items. Use it
    like `tool-bar-add-item-from-menu` except that in-map
    specifies the local map to make the definition in. The argument
    from-map is like the map argument of
    `tool-bar-add-item-from-menu`.

In addition to the tool bar items defined in `tool-bar-map`,
Emacs also supports displaying an additional row of “secondary” tool
bar items specified in the keymap `secondary-tool-bar-map`.
These items are normally displayed below those defined within
`tool-bar-map` if the tool bar is positioned at the top of its
frame, but are displayed above them if the tool bar is positioned at
the bottom (see [Layout Parameters](#Layout-Parameters).) They are not displayed if the
tool bar is positioned at the left or right of a frame.

Variable: **auto-resize-tool-bars** [¶](#index-auto_002dresize_002dtool_002dbars)
:   If this variable is non-`nil`, the tool bar automatically resizes to
    show all defined tool bar items—but not larger than a quarter of the
    frame’s height.

    If the value is `grow-only`, the tool bar expands automatically,
    but does not contract automatically. To contract the tool bar, the
    user has to redraw the frame by entering `C-l`.

    If Emacs is built with GTK+ or Nextstep, the tool bar can only show one
    line, so this variable has no effect.

Variable: **auto-raise-tool-bar-buttons** [¶](#index-auto_002draise_002dtool_002dbar_002dbuttons)
:   If this variable is non-`nil`, tool bar items display
    in raised form when the mouse moves over them.

Variable: **tool-bar-button-margin** [¶](#index-tool_002dbar_002dbutton_002dmargin)
:   This variable specifies an extra margin to add around tool bar items.
    The value is an integer, a number of pixels. The default is 4.

Variable: **tool-bar-button-relief** [¶](#index-tool_002dbar_002dbutton_002drelief)
:   This variable specifies the shadow width for tool bar items.
    The value is an integer, a number of pixels. The default is 1.

Variable: **tool-bar-border** [¶](#index-tool_002dbar_002dborder)
:   This variable specifies the height of the border drawn below the tool
    bar area. An integer specifies height as a number of pixels.
    If the value is one of `internal-border-width` (the default) or
    `border-width`, the tool bar border height corresponds to the
    corresponding frame parameter.

You can define a special meaning for clicking on a tool bar item with
the shift, control, meta, etc., modifiers. You do this by setting up
additional items that relate to the original item through the fake
function keys. Specifically, the additional items should use the
modified versions of the same fake function key used to name the
original item.

Thus, if the original item was defined this way,

```
(define-key global-map [tool-bar shell]
  '(menu-item "Shell" shell
              :image (image :type xpm :file "shell.xpm")))
```

then here is how you can define clicking on the same tool bar image with
the shift modifier:

```
(define-key global-map [tool-bar S-shell] 'some-command)
```

See [Function Keys](#Function-Keys), for more information about how to add modifiers to
function keys.

If you have functions that change whether a tool bar item is enabled
or not, this status is not necessarily updated visually immediately.
To force recalculation of the tool bar, call
`force-mode-line-update` (see [Mode Line Format](#Mode-Line-Format)).

---

Next: [Easy Menu](#Easy-Menu), Previous: [Tool bars](#Tool-Bar), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.7 Modifying Menus [¶](#Modifying-Menus-1)

When you insert a new item in an existing menu, you probably want to
put it in a particular place among the menu’s existing items. If you
use `define-key` to add the item, it normally goes at the front of
the menu. To put it elsewhere in the menu, use `keymap-set-after`:

Function: **keymap-set-after** map key binding &optional after [¶](#index-keymap_002dset_002dafter)
:   Define a binding in map for key, with value binding,
    just like `keymap-set` (see [Changing Key Bindings](#Changing-Key-Bindings)), but
    position the binding in map after the binding for the event
    after. The argument key should represent a single menu
    item or key, and should satisfy `key-valid-p` (see [Key Sequences](#Key-Sequences)). after should be a single event type—a symbol or
    a character, not a sequence. The new binding goes after the binding
    for after. If after is `t` or is omitted, then the
    new binding goes last, at the end of the keymap. However, new
    bindings are added before any inherited keymap.

    Here is an example:

    ```
    (keymap-set-after my-menu "<drink>"
      '("Drink" . drink-command) 'eat)
    ```

    makes a binding for the fake function key `DRINK` and puts it
    right after the binding for `EAT`.

    Here is how to insert an item called ‘`Work`’ in the ‘`Signals`’
    menu of Shell mode, after the item `break`:

    ```
    (keymap-set-after shell-mode-map "<menu-bar> <signals> <work>"
      '("Work" . work-command) 'break)
    ```

---

Previous: [Modifying Menus](#Modifying-Menus), Up: [Menu Keymaps](#Menu-Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 23.18.8 Easy Menu [¶](#Easy-Menu-1)

The following macro provides a convenient way to define pop-up menus
and/or menu bar menus.

Macro: **easy-menu-define** symbol maps doc menu [¶](#index-easy_002dmenu_002ddefine)
:   This macro defines a pop-up menu and/or menu bar submenu, whose
    contents are given by menu.

    If symbol is non-`nil`, it should be a symbol; then this
    macro defines symbol as a function for popping up the menu
    (see [Pop-Up Menus](#Pop_002dUp-Menus)), with doc as its documentation string. It
    also defines symbol as a variable whose value is the menu.
    symbol should not be quoted.

    Regardless of the value of symbol, if maps is a keymap,
    the menu is added to that keymap, as a top-level menu for the menu bar
    (see [The Menu Bar](#Menu-Bar)). It can also be a list of keymaps, in which case
    the menu is added separately to each of those keymaps.

    The first element of menu must be a string, which serves as the
    menu label. It may be followed by any number of the following
    keyword-argument pairs:

    `:filter function`
    :   function must be a function which, if called with one
        argument—the list of the other menu items—returns the actual items
        to be displayed in the menu.

    `:visible include`
    :   include is an expression; if it evaluates to `nil`, the
        menu is made invisible. `:included` is an alias for
        `:visible`.

    `:active enable`
    :   enable is an expression; if it evaluates to `nil`, the menu
        is not selectable. `:enable` is an alias for `:active`.

    The remaining elements in menu are menu items.

    A menu item can be a vector of three elements, `[name
    callback enable]`. name is the menu item name (a
    string). callback is a command to run, or an expression to
    evaluate, when the item is chosen. enable is an expression; if
    it evaluates to `nil`, the item is disabled for selection.

    Alternatively, a menu item may have the form:

    ```
       [ name callback [ keyword arg ]... ]
    ```

    where name and callback have the same meanings as above,
    and each optional keyword and arg pair should be one of
    the following:

    `:keys keys`
    :   keys is a string to display as keyboard equivalent to the menu item.
        This is normally not needed, as keyboard equivalents are computed
        automatically. keys is expanded with
        `substitute-command-keys` before it is displayed (see [Substituting Key Bindings in Documentation](#Keys-in-Documentation)).

    `:key-sequence keys`
    :   keys is a hint indicating which key sequence to display as
        keyboard equivalent, in case the command is bound to several key sequences.
        It has no effect if keys is not bound to same command as this
        menu item.

    `:active enable`
    :   enable is an expression; if it evaluates to `nil`, the item
        is made unselectable. `:enable` is an alias for `:active`.

    `:visible include`
    :   include is an expression; if it evaluates to `nil`, the
        item is made invisible. `:included` is an alias for
        `:visible`.

    `:label form`
    :   form is an expression that is evaluated to obtain a value which
        serves as the menu item’s label (the default is name).

    `:suffix form`
    :   form is an expression that is dynamically evaluated and whose
        value is concatenated with the menu entry’s label.

    `:style style`
    :   style is a symbol describing the type of menu item; it should be
        `toggle` (a checkbox), or `radio` (a radio button), or
        anything else (meaning an ordinary menu item).

    `:selected selected`
    :   selected is an expression; the checkbox or radio button is
        selected whenever the expression’s value is non-`nil`.

    `:help help`
    :   help is a string describing the menu item.

    Alternatively, a menu item can be a string. Then that string appears
    in the menu as unselectable text. A string consisting of dashes is
    displayed as a separator (see [Menu Separators](#Menu-Separators)).

    Alternatively, a menu item can be a list with the same format as
    menu. This is a submenu.

Here is an example of using `easy-menu-define` to define a menu
similar to the one defined in the example in [The Menu Bar](#Menu-Bar):

```
(easy-menu-define words-menu global-map
  "Menu for word navigation commands."
  '("Words"
     ["Forward word" forward-word]
     ["Backward word" backward-word]))
```

---

Next: [Documentation](#Documentation), Previous: [Keymaps](#Keymaps), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 24 Major and Minor Modes [¶](#Major-and-Minor-Modes)

A *mode* is a set of definitions that customize Emacs behavior
in useful ways. There are two varieties of modes: *minor modes*,
which provide features that users can turn on and off while editing;
and *major modes*, which are used for editing or interacting with
a particular kind of text. Each buffer has exactly one *major
mode* at a time.

This chapter describes how to write both major and minor modes, how to
indicate them in the mode line, and how they run hooks supplied by the
user. For related topics such as keymaps and syntax tables, see
[Keymaps](#Keymaps), and [Syntax Tables](#Syntax-Tables).

* [Hooks](#Hooks)
* [Major Modes](#Major-Modes)
* [Minor Modes](#Minor-Modes)
* [Mode Line Format](#Mode-Line-Format)
* [Imenu](#Imenu)
* [Outline Minor Mode](#Outline-Minor-Mode)
* [Font Lock Mode](#Font-Lock-Mode)
* [Automatic Indentation of code](#Auto_002dIndentation)
* [Desktop Save Mode](#Desktop-Save-Mode)

---

Next: [Major Modes](#Major-Modes), Up: [Major and Minor Modes](#Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 24.1 Hooks [¶](#Hooks-1)

A *hook* is a variable where you can store a function or
functions (see [What Is a Function?](#What-Is-a-Function)) to be called on a particular
occasion by an existing program. Emacs provides hooks for the sake of
customization. Most often, hooks are set up in the init file
(see [The Init File](#Init-File)), but Lisp programs can set them also.
See [Standard Hooks](#Standard-Hooks), for a list of some standard hook variables.

Most of the hooks in Emacs are *normal hooks*. These variables
contain lists of functions to be called with no arguments. By
convention, whenever the hook name ends in ‘`-hook`’, that tells
you it is normal. We try to make all hooks normal, as much as
possible, so that you can use them in a uniform way.

Every major mode command is supposed to run a normal hook called the
*mode hook* as one of the last steps of initialization. This makes
it easy for a user to customize the behavior of the mode, by overriding
the buffer-local variable assignments already made by the mode. Most
minor mode functions also run a mode hook at the end. But hooks are
used in other contexts too. For example, the hook `suspend-hook`
runs just before Emacs suspends itself (see [Suspending Emacs](#Suspending-Emacs)).

If the hook variable’s name does not end with ‘`-hook`’, that
indicates it is probably an *abnormal hook*. These differ from
normal hooks in two ways: they can be called with one or more
arguments, and their return values can be used in some way. The
hook’s documentation says how the functions are called and how their
return values are used. Any functions added to an abnormal hook must
follow the hook’s calling convention. By convention, abnormal hook
names end in ‘`-functions`’.

If the name of the variable ends in ‘`-predicate`’ or
‘`-function`’ (singular) then its value must be a function, not a
list of functions. As with abnormal hooks, the expected arguments and
meaning of the return value vary across such *single function
hooks*. The details are explained in each variable’s docstring.

Since hooks (both multi and single function) are variables, their
values can be modified with `setq` or temporarily with
`let`. However, it is often useful to add or remove a particular
function from a hook while preserving any other functions it might
have. For multi function hooks, the recommended way of doing this is
with `add-hook` and `remove-hook` (see [Setting Hooks](#Setting-Hooks)).
Most normal hook variables are initially void; `add-hook` knows
how to deal with this. You can add hooks either globally or
buffer-locally with `add-hook`. For hooks which hold only a
single function, `add-hook` is not appropriate, but you can use
`add-function` (see [Advising Emacs Lisp Functions](#Advising-Functions)) to combine new
functions with the hook. Note that some single function hooks may be
`nil` which `add-function` cannot deal with, so you must
check for that before calling `add-function`.

* [Running Hooks](#Running-Hooks)
* [Setting Hooks](#Setting-Hooks)

---

Next: [Setting Hooks](#Setting-Hooks), Up: [Hooks](#Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.1.1 Running Hooks [¶](#Running-Hooks-1)

In this section, we document the `run-hooks` function, which is
used to run a normal hook. We also document the functions for running
various kinds of abnormal hooks.

Function: **run-hooks** &rest hookvars [¶](#index-run_002dhooks)
:   This function takes one or more normal hook variable names as
    arguments, and runs each hook in turn. Each argument should be a
    symbol that is a normal hook variable. These arguments are processed
    in the order specified.

    If a hook variable has a non-`nil` value, that value should be a
    list of functions. `run-hooks` calls all the functions, one by
    one, with no arguments.

    The hook variable’s value can also be a single function—either a
    lambda expression or a symbol with a function definition—which
    `run-hooks` calls. But this usage is obsolete.

    If the hook variable is buffer-local, the buffer-local variable will
    be used instead of the global variable. However, if the buffer-local
    variable contains the element `t`, the global hook variable will
    be run as well.

Function: **run-hook-with-args** hook &rest args [¶](#index-run_002dhook_002dwith_002dargs)
:   This function runs an abnormal hook by calling all the hook functions in
    hook, passing each one the arguments args.

Function: **run-hook-with-args-until-failure** hook &rest args [¶](#index-run_002dhook_002dwith_002dargs_002duntil_002dfailure)
:   This function runs an abnormal hook by calling each hook function in
    turn, stopping if one of them fails by returning `nil`. Each
    hook function is passed the arguments args. If this function
    stops because one of the hook functions fails, it returns `nil`;
    otherwise it returns a non-`nil` value.

Function: **run-hook-with-args-until-success** hook &rest args [¶](#index-run_002dhook_002dwith_002dargs_002duntil_002dsuccess)
:   This function runs an abnormal hook by calling each hook function,
    stopping if one of them succeeds by returning a non-`nil`
    value. Each hook function is passed the arguments args. If this
    function stops because one of the hook functions returns a
    non-`nil` value, it returns that value; otherwise it returns
    `nil`.

---

Previous: [Running Hooks](#Running-Hooks), Up: [Hooks](#Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 24.1.2 Setting Hooks [¶](#Setting-Hooks-1)

Here’s an example that adds a function to a mode hook to turn
on Auto Fill mode when in Lisp Interaction mode:

```
(add-hook 'lisp-interaction-mode-hook 'auto-fill-mode)
```

The value of a hook variable should be a list of functions. You can
manipulate that list using the normal Lisp facilities, but the modular
way is to use the functions `add-hook` and `remove-hook`,
defined below. They take care to handle some unusual situations and
avoid problems.

It works to put a `lambda`-expression function on a hook, but
we recommend avoiding this because it can lead to confusion. If you
add the same `lambda`-expression a second time but write it
slightly differently, you will get two equivalent but distinct
functions on the hook. If you then remove one of them, the other will
still be on it.

Function: **add-hook** hook function &optional depth local [¶](#index-add_002dhook)
:   This function is the handy way to add function function to hook
    variable hook. You can use it for abnormal hooks as well as for
    normal hooks. function can be any Lisp function that can accept
    the proper number of arguments for hook. For example,

    ```
    (add-hook 'text-mode-hook 'my-text-hook-function)
    ```

    adds `my-text-hook-function` to the hook called `text-mode-hook`.

    If function is already present in hook (comparing using
    `equal`), then `add-hook` does not add it a second time.

    If function has a non-`nil` property
    `permanent-local-hook`, then `kill-all-local-variables` (or
    changing major modes) won’t delete it from the hook variable’s local
    value.

    For a normal hook, hook functions should be designed so that the order
    in which they are executed does not matter. Any dependence on the order
    is asking for trouble. However, the order is predictable: normally,
    function goes at the front of the hook list, so it is executed
    first (barring another `add-hook` call).

    In some cases, it is important to control the relative ordering of functions
    on the hook. The optional argument depth lets you indicate where the
    function should be inserted in the list: it should then be a number
    between -100 and 100 where the higher the value, the closer to the end of the
    list the function should go. The depth defaults to 0 and for backward
    compatibility when depth is a non-`nil` symbol it is interpreted as a depth
    of 90. Furthermore, when depth is strictly greater than 0 the function
    is added *after* rather than before functions of the same depth.
    One should never use a depth of 100 (or -100), because one can never be
    sure that no other function will ever need to come before (or after) us.

    `add-hook` can handle the cases where hook is void or its
    value is a single function; it sets or changes the value to a list of
    functions.

    If local is non-`nil`, that says to add function to the
    buffer-local hook list instead of to the global hook list. This makes
    the hook buffer-local and adds `t` to the buffer-local value. The
    latter acts as a flag to run the hook functions in the default value as
    well as in the local value.

Function: **remove-hook** hook function &optional local [¶](#index-remove_002dhook)
:   This function removes function from the hook variable
    hook. It compares function with elements of hook
    using `equal`, so it works for both symbols and lambda
    expressions.

    If local is non-`nil`, that says to remove function
    from the buffer-local hook list instead of from the global hook list.

---

Next: [Minor Modes](#Minor-Modes), Previous: [Hooks](#Hooks), Up: [Major and Minor Modes](#Modes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
