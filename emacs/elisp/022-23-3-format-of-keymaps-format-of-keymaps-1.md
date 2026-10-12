### 23.3 Format of Keymaps [¶](#Format-of-Keymaps-1)

Each keymap is a list whose CAR is the symbol `keymap`. The
remaining elements of the list define the key bindings of the keymap.
A symbol whose function definition is a keymap is also a keymap. Use
the function `keymapp` (see below) to test whether an object is a
keymap.

Several kinds of elements may appear in a keymap, after the symbol
`keymap` that begins it:

`(type . binding)`
:   This specifies one binding, for events of type type. Each
    ordinary binding applies to events of a particular *event type*,
    which is always a character or a symbol. See [Classifying Events](#Classifying-Events).
    In this kind of binding, binding is a command.

`(type item-name . binding)`
:   This specifies a binding which is also a simple menu item that
    displays as item-name in the menu. See [Simple Menu Items](#Simple-Menu-Items).

`(type item-name help-string . binding)`
:   This is a simple menu item with help string help-string.

`(type menu-item . details)`
:   This specifies a binding which is also an extended menu item. This
    allows use of other features. See [Extended Menu Items](#Extended-Menu-Items).

`(t . binding)` [¶](#index-default-key-binding)
:   This specifies a *default key binding*; any event not bound by other
    elements of the keymap is given binding as its binding. Default
    bindings allow a keymap to bind all possible event types without having
    to enumerate all of them. A keymap that has a default binding
    completely masks any lower-precedence keymap, except for events
    explicitly bound to `nil` (see below).

`char-table`
:   If an element of a keymap is a char-table, it counts as holding
    bindings for all character events with no modifier bits
    (see [modifier bits](#modifier-bits)): the element whose index is c is the
    binding for the character c. This is a compact way to record
    lots of bindings. A keymap with such a char-table is called a
    *full keymap*. Other keymaps are called *sparse keymaps*.

`vector`
:   This kind of element is similar to a char-table: the element whose
    index is c is the binding for the character c. Since the
    range of characters that can be bound this way is limited by the
    vector size, and vector creation allocates space for all character
    codes from 0 up, this format should not be used except for creating
    menu keymaps (see [Menu Keymaps](#Menu-Keymaps)), where the bindings themselves
    don’t matter.

`string` [¶](#index-keymap-prompt-string)
:   Aside from elements that specify bindings for keys, a keymap can also
    have a string as an element. This is called the *overall prompt
    string* and makes it possible to use the keymap as a menu.
    See [Defining Menus](#Defining-Menus).

`(keymap …)`
:   If an element of a keymap is itself a keymap, it counts as if this inner keymap
    were inlined in the outer keymap. This is used for multiple-inheritance, such
    as in `make-composed-keymap`.

When the binding is `nil`, it doesn’t constitute a definition
but it does take precedence over a default binding or a binding in the
parent keymap. On the other hand, a binding of `nil` does
*not* override lower-precedence keymaps; thus, if the local map
gives a binding of `nil`, Emacs uses the binding from the
global map.

Keymaps do not directly record bindings for the meta characters.
Instead, meta characters are regarded for purposes of key lookup as
sequences of two characters, the first of which is `ESC` (or
whatever is currently the value of `meta-prefix-char`). Thus, the
key `M-a` is internally represented as `ESC a`, and its
global binding is found at the slot for `a` in `esc-map`
(see [Prefix Keys](#Prefix-Keys)).

This conversion applies only to characters, not to function keys or
other input events; thus, `M-end` has nothing to do with
`ESC end`.

Here as an example is the local keymap for Lisp mode, a sparse
keymap. It defines bindings for `DEL`, `C-c C-z`,
`C-M-q`, and `C-M-x` (the actual value also contains a menu
binding, which is omitted here for the sake of brevity).

```
lisp-mode-map
⇒
```

```
(keymap
 (3 keymap
    ;; C-c C-z
    (26 . run-lisp))
```

```
 (27 keymap
     ;; C-M-x, treated as ESC C-x
     (24 . lisp-send-defun))
```

```
 ;; This part is inherited from lisp-mode-shared-map.
 keymap
 ;; DEL
 (127 . backward-delete-char-untabify)
```

```
 (27 keymap
     ;; C-M-q, treated as ESC C-q
     (17 . indent-sexp)))
```

Function: **keymapp** object [¶](#index-keymapp)
:   This function returns `t` if object is a keymap, `nil`
    otherwise. More precisely, this function tests for a list whose
    CAR is `keymap`, or for a symbol whose function definition
    satisfies `keymapp`.

    ```
    (keymapp '(keymap))
        ⇒ t
    ```

    ```
    (fset 'foo '(keymap))
    (keymapp 'foo)
        ⇒ t
    ```

    ```
    (keymapp (current-global-map))
        ⇒ t
    ```

---

Next: [Inheritance and Keymaps](#Inheritance-and-Keymaps), Previous: [Format of Keymaps](#Format-of-Keymaps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.4 Creating Keymaps [¶](#Creating-Keymaps-1)

Here we describe the functions for creating keymaps.

Function: **make-sparse-keymap** &optional prompt [¶](#index-make_002dsparse_002dkeymap)
:   This function creates and returns a new sparse keymap with no entries.
    (A sparse keymap is the kind of keymap you usually want.) The new
    keymap does not contain a char-table, unlike `make-keymap`, and
    does not bind any events.

    ```
    (make-sparse-keymap)
        ⇒ (keymap)
    ```

    If you specify prompt, that becomes the overall prompt string
    for the keymap. You should specify this only for menu keymaps
    (see [Defining Menus](#Defining-Menus)). A keymap with an overall prompt string will
    always present a mouse menu or a keyboard menu if it is active for
    looking up the next input event. Don’t specify an overall prompt string
    for the main map of a major or minor mode, because that would cause
    the command loop to present a keyboard menu every time.

Function: **make-keymap** &optional prompt [¶](#index-make_002dkeymap)
:   This function creates and returns a new full keymap. That keymap
    contains a char-table (see [Char-Tables](#Char_002dTables)) with slots for all
    characters without modifiers. The new keymap initially binds all
    these characters to `nil`, and does not bind any other kind of
    event. The argument prompt specifies a
    prompt string, as in `make-sparse-keymap`.

    ```
    (make-keymap)
        ⇒ (keymap #^[nil nil keymap nil nil nil ...])
    ```

    A full keymap is more efficient than a sparse keymap when it holds
    lots of bindings; for just a few, the sparse keymap is better.

Function: **define-keymap** &key options... &rest pairs... [¶](#index-define_002dkeymap)
:   You can create a keymap with the functions described above, and then
    use `keymap-set` (see [Changing Key Bindings](#Changing-Key-Bindings)) to specify key
    bindings in that map. When writing modes, however, you frequently
    have to bind a large number of keys at once, and using
    `keymap-set` on them all can be tedious and error-prone. Instead
    you can use `define-keymap`, which creates a keymap and binds a
    number of keys. Here’s a very basic example:

    ```
    (define-keymap
      "n" #'forward-line
      "f" #'previous-line
      "C-c C-c" #'quit-window)
    ```

    This function creates a new sparse keymap, defines the keystrokes in
    pairs, and returns the new keymap. It signals an error if there
    are duplicate key bindings in pairs.

    pairs is a list of alternating key bindings and key definitions,
    as accepted by `keymap-set`. In addition, the key can be the
    special symbol `:menu`, in which case the definition should be a
    menu definition as accepted by `easy-menu-define` (see [Easy Menu](#Easy-Menu)). Here’s a brief example of this usage:

    ```
    (define-keymap :full t
      "g" #'eww-reload
      :menu '("Eww"
              ["Exit" quit-window t]
              ["Reload" eww-reload t]))
    ```

    A number of keywords can be used before the key/definition pairs to
    change features of the new keymap. If any of the feature keywords is
    missing from the `define-keymap` call, the default value for that
    feature is `nil`. Here’s a list of the available feature
    keywords:

    `:full`
    :   If non-`nil`, create a char-table keymap (as from
        `make-keymap`) instead of a sparse keymap (as from
        `make-sparse-keymap` (see [Creating Keymaps](#Creating-Keymaps)). A sparse keymap
        is the default.

    `:parent`
    :   If non-`nil`, the value should be a keymap to use as the parent
        (see [Inheritance and Keymaps](#Inheritance-and-Keymaps)).

    `:keymap`
    :   If non-`nil`, the value should be a keymap. Instead of creating
        a new keymap, the specified keymap is modified instead.

    `:suppress`
    :   If non-`nil`, the keymap will be suppressed with
        `suppress-keymap` (see [Changing Key Bindings](#Changing-Key-Bindings)). By default,
        digits and the minus sign are exempt from suppressing, but if the
        value is `nodigits`, this suppresses digits and minus-sign like
        it does with other characters.

    `:name`
    :   If non-`nil`, the value should be a string to use as the menu for
        the keymap if you use it as a menu with `x-popup-menu`
        (see [Pop-Up Menus](#Pop_002dUp-Menus)).

    `:prefix`
    :   If non-`nil`, the value should be a symbol to be used as a prefix
        command (see [Prefix Keys](#Prefix-Keys)). If this is the case, this symbol is
        returned by `define-keymap` instead of the map itself.

Macro: **defvar-keymap** name &key options... &rest pairs... [¶](#index-defvar_002dkeymap)
:   By far, the most common thing to do with a keymap is to bind it to a
    variable. This is what virtually all modes do—a mode called
    `foo` almost always has a variable called `foo-mode-map`.

    This macro defines name as a variable, passes options
    and pairs to `define-keymap`, and uses the result as the
    default value for the variable. It signals an error if there are
    duplicate key bindings in pairs.

    options is mostly like the keywords in `define-keymap`.
    There’s an additional `:doc` keyword that provides the doc string
    for the defined variable. Here’s an example:

    ```
    (defvar-keymap eww-textarea-map
      :parent text-mode-map
      :doc "Keymap for the eww text area."
      "RET" #'forward-line
      "TAB" #'shr-next-link)
    ```

    The `:prefix` keyword can take an additional value, `t`, which
    is an abbreviation for using name as the value of that option.
    I.e. instead of writing

    ```
    (defvar-keymap foo-map
      :prefix 'foo-map
      ...)
    ```

    you can write

    ```
    (defvar-keymap foo-map
      :prefix t
      ...)
    ```

    Each command in the keymap can be marked as ‘repeatable’, i.e. usable
    in `repeat-mode`, by putting a `repeat-map` property on it,
    for example:

    ```
    (put 'undo 'repeat-map 'undo-repeat-map)
    ```

    where the value of the property is the map to be used by
    `repeat-mode`.

    To avoid repetitive invocations of `put`, `defvar-keymap`
    also has a `:repeat` keyword, which can be used to specify which
    commands in the keymap are usable by `repeat-mode`. The
    following values are available:

    `t`
    :   This means all the commands in the keymap are repeatable, and is the
        most common usage.

    `(:enter (commands ...) :exit (commands ...)`

    `:continue (commands ...) :hints ((command . "hint") ...))`
    :   This specifies that the commands in the `:enter` list enter
        `repeat-mode`, and the commands in the `:exit` list exit
        repeat mode.

        If the `:enter` list is empty, then all commands in the map enter
        `repeat-mode`. Specifying one or more commands in this list is
        useful if there is a command which doesn’t exist in the map being
        defined, but which should have the `repeat-map` property.

        If the `:exit` list is empty then no commands in the map exit
        `repeat-mode`. Specifying one or more commands in this list is
        useful if the keymap being defined contains a command that should not
        have the `repeat-map` property.

        The `:continue` list specifies the commands that should not
        enter `repeat-mode`. These command should only continue the
        already activated repeating sequence.

        The `:hints` list can contain cons pairs where the CAR is
        a command and the CDR is a string that is displayed alongside of
        the repeatable key in the echo area.

    In order to make e.g. `u` repeat the `undo` command, the
    following two stanzas are equivalent:

    ```
    (defvar-keymap undo-repeat-map
      "u" #'undo)
    (put 'undo 'repeat-map 'undo-repeat-map)
    ```

    and

    ```
    (defvar-keymap undo-repeat-map
      :repeat t
      "u" #'undo)
    ```

    The latter is preferred when there are many commands in the map, all
    of which should be repeatable.

Function: **copy-keymap** keymap [¶](#index-copy_002dkeymap)
:   This function returns a copy of keymap. This is almost never
    needed. If you want a keymap that’s like another yet with a few
    changes, you should use map inheritance rather than copying.
    I.e., something like:

    ```
    (let ((map (make-sparse-keymap)))
      (set-keymap-parent map <theirmap>)
      (keymap-set map ...)
      ...)
    ```

    When performing `copy-keymap`, any keymaps that
    appear directly as bindings in keymap are also copied recursively,
    and so on to any number of levels. However, recursive copying does not
    take place when the definition of a character is a symbol whose function
    definition is a keymap; the same symbol appears in the new copy.

    ```
    (setq map (copy-keymap (current-local-map)))
    ⇒ (keymap
    ```

    ```
         ;; (This implements meta characters.)
         (27 keymap
             (83 . center-paragraph)
             (115 . center-line))
         (9 . tab-to-tab-stop))
    ```

    ```

    ```

    ```
    (eq map (current-local-map))
        ⇒ nil
    ```

    ```
    (equal map (current-local-map))
        ⇒ t
    ```

---

Next: [Prefix Keys](#Prefix-Keys), Previous: [Creating Keymaps](#Creating-Keymaps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.5 Inheritance and Keymaps [¶](#Inheritance-and-Keymaps-1)

A keymap can inherit the bindings of another keymap, which we call the
*parent keymap*. Such a keymap looks like this:

```
(keymap elements... . parent-keymap)
```

The effect is that this keymap inherits all the bindings of
parent-keymap, whatever they may be at the time a key is looked up,
but can add to them or override them with elements.

If you change the bindings in parent-keymap using
`keymap-set` or other key-binding functions, these changed
bindings are visible in the inheriting keymap, unless shadowed by the
bindings made by elements. The converse is not true: if you use
`keymap-set` to change bindings in the inheriting keymap, these
changes are recorded in elements, but have no effect on
parent-keymap.

The proper way to construct a keymap with a parent is to use
`set-keymap-parent`; if you have code that directly constructs a
keymap with a parent, please convert the program to use
`set-keymap-parent` instead.

Function: **keymap-parent** keymap [¶](#index-keymap_002dparent)
:   This returns the parent keymap of keymap. If keymap
    has no parent, `keymap-parent` returns `nil`.

Function: **set-keymap-parent** keymap parent [¶](#index-set_002dkeymap_002dparent)
:   This sets the parent keymap of keymap to parent, and returns
    parent. If parent is `nil`, this function gives
    keymap no parent at all.

    If keymap has submaps (bindings for prefix keys), they too receive
    new parent keymaps that reflect what parent specifies for those
    prefix keys.

Here is an example showing how to make a keymap that inherits
from `text-mode-map`:

```
(let ((map (make-sparse-keymap)))
  (set-keymap-parent map text-mode-map)
  map)
```

A non-sparse keymap can have a parent too, but this is not very
useful. A non-sparse keymap always specifies something as the binding
for every numeric character code without modifier bits, even if it is
`nil`, so these character’s bindings are never inherited from
the parent keymap.

Sometimes you want to make a keymap that inherits from more than one
map. You can use the function `make-composed-keymap` for this.

Function: **make-composed-keymap** maps &optional parent [¶](#index-make_002dcomposed_002dkeymap)
:   This function returns a new keymap composed of the existing keymap(s)
    maps, and optionally inheriting from a parent keymap
    parent. maps can be a single keymap or a list of more
    than one. When looking up a key in the resulting new map, Emacs
    searches in each of the maps in turn, and then in parent,
    stopping at the first match. A `nil` binding in any one of
    maps overrides any binding in parent, but it does not
    override any non-`nil` binding in any other of the maps.

For example, here is how Emacs sets the parent of
`help-mode-map`, such that it inherits from both
`button-buffer-map` and `special-mode-map`:

```
(defvar-keymap help-mode-map
  :parent (make-composed-keymap button-buffer-map
                                special-mode-map)
  ...)
```

---

Next: [Active Keymaps](#Active-Keymaps), Previous: [Inheritance and Keymaps](#Inheritance-and-Keymaps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.6 Prefix Keys [¶](#Prefix-Keys-1)

A *prefix key* is a key sequence whose binding is a keymap. The
keymap defines what to do with key sequences that extend the prefix key.
For example, `C-x` is a prefix key, and it uses a keymap that is
also stored in the variable `ctl-x-map`. This keymap defines
bindings for key sequences starting with `C-x`.

Some of the standard Emacs prefix keys use keymaps that are
also found in Lisp variables:

* `esc-map` is the global keymap for the `ESC` prefix key. Thus,
  the global definitions of all meta characters are actually found here.
  This map is also the function definition of `ESC-prefix`.
* `help-map` is the global keymap for the `C-h` prefix key.
* `mode-specific-map` is the global keymap for the prefix key
  `C-c`. This map is actually global, not mode-specific, but its name
  provides useful information about `C-c` in the output of `C-h b`
  (`display-bindings`), since the main use of this prefix key is for
  mode-specific bindings.
* `ctl-x-map` is the global keymap used for the `C-x` prefix key.
  This map is found via the function cell of the symbol
  `Control-X-prefix`.
* `mule-keymap` is the global keymap used for the `C-x RET`
  prefix key.
* `ctl-x-4-map` is the global keymap used for the `C-x 4` prefix
  key.
* `ctl-x-5-map` is the global keymap used for the `C-x 5` prefix
  key.
* `2C-mode-map` is the global keymap used for the `C-x 6` prefix
  key.
* `tab-prefix-map` is the global keymap used for the `C-x t` prefix
  key.
* `vc-prefix-map` is the global keymap used for the `C-x v` prefix
  key.
* `goto-map` is the global keymap used for the `M-g` prefix
  key.
* `search-map` is the global keymap used for the `M-s` prefix
  key.
* The other Emacs prefix keys are `C-x @`, `C-x a i`, `C-x
  ESC` and `ESC ESC`. They use keymaps that have
  no special names.

The keymap binding of a prefix key is used for looking up the event
that follows the prefix key. (It may instead be a symbol whose function
definition is a keymap. The effect is the same, but the symbol serves
as a name for the prefix key.) Thus, the binding of `C-x` is the
symbol `Control-X-prefix`, whose function cell holds the keymap
for `C-x` commands. (The same keymap is also the value of
`ctl-x-map`.)

Prefix key definitions can appear in any active keymap. The
definitions of `C-c`, `C-x`, `C-h` and `ESC` as prefix
keys appear in the global map, so these prefix keys are always
available. Major and minor modes can redefine a key as a prefix by
putting a prefix key definition for it in the local map or the minor
mode’s map. See [Active Keymaps](#Active-Keymaps).

If a key is defined as a prefix in more than one active map, then its
various definitions are in effect merged: the commands defined in the
minor mode keymaps come first, followed by those in the local map’s
prefix definition, and then by those from the global map.

In the following example, we make `C-p` a prefix key in the local
keymap, in such a way that `C-p` is identical to `C-x`. Then
the binding for `C-p C-f` is the function `find-file`, just
like `C-x C-f`. By contrast, the key sequence `C-p 9` is not
found in any active keymap.

```
(use-local-map (make-sparse-keymap))
    ⇒ nil
```

```
(keymap-local-set "C-p" ctl-x-map)
    ⇒ (keymap #^[nil nil keymap ...
```

```
(keymap-lookup nil "C-p C-f")
    ⇒ find-file
```

```
(keymap-lookup nil "C-p 9")
    ⇒ nil
```

Function: **define-prefix-command** symbol &optional mapvar prompt [¶](#index-define_002dprefix_002dcommand)
:   This function prepares symbol for use as a prefix key’s binding:
    it creates a sparse keymap and stores it as symbol’s function
    definition. Subsequently binding a key sequence to symbol will
    make that key sequence into a prefix key. The return value is `symbol`.

    This function also sets symbol as a variable, with the keymap as
    its value. But if mapvar is non-`nil`, it sets mapvar
    as a variable instead.

    If prompt is non-`nil`, that becomes the overall prompt
    string for the keymap. The prompt string should be given for menu keymaps
    (see [Defining Menus](#Defining-Menus)).

---

Next: [Searching the Active Keymaps](#Searching-Keymaps), Previous: [Prefix Keys](#Prefix-Keys), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.7 Active Keymaps [¶](#Active-Keymaps-1)

Emacs contains many keymaps, but at any time only a few keymaps are
*active*. When Emacs receives user input, it translates the input
event (see [Keymaps for Translating Sequences of Events](#Translation-Keymaps)), and looks for a key binding in
the active keymaps.

Usually, the active keymaps are: (i) the keymap specified by the
`keymap` property, (ii) the keymaps of enabled minor modes, (iii)
the current buffer’s local keymap, and (iv) the global keymap, in that
order. Emacs searches for each input key sequence in all these
keymaps.

Of these usual keymaps, the highest-precedence one is specified
by the `keymap` text or overlay property at point, if any. (For
a mouse input event, Emacs uses the event position instead of point;
see [Searching the Active Keymaps](#Searching-Keymaps).)

Next in precedence are keymaps specified by enabled minor modes.
These keymaps, if any, are specified by the variables
`emulation-mode-map-alists`,
`minor-mode-overriding-map-alist`, and
`minor-mode-map-alist`. See [Controlling the Active Keymaps](#Controlling-Active-Maps).

Next in precedence is the buffer’s *local keymap*, containing
key bindings specific to the buffer. The minibuffer also has a local
keymap (see [Introduction to Minibuffers](#Intro-to-Minibuffers)). If there is a `local-map`
text or overlay property at point, that specifies the local keymap to
use, in place of the buffer’s default local keymap.

The local keymap is normally set by the buffer’s major mode, and
every buffer with the same major mode shares the same local keymap.
Hence, if you call `keymap-local-set` (see [Commands for Binding Keys](#Key-Binding-Commands))
to change the local keymap in one buffer, that also affects the local
keymaps in other buffers with the same major mode.

Finally, the *global keymap* contains key bindings that are
defined regardless of the current buffer, such as `C-f`. It is
always active, and is bound to the variable `global-map`.

Apart from the above usual keymaps, Emacs provides special ways
for programs to make other keymaps active. Firstly, the variable
`overriding-local-map` specifies a keymap that replaces the usual
active keymaps, except for the global keymap. Secondly, the
terminal-local variable `overriding-terminal-local-map` specifies
a keymap that takes precedence over *all* other keymaps
(including `overriding-local-map`); this is normally used for
modal/transient key bindings (the function `set-transient-map`
provides a convenient interface for this). See [Controlling the Active Keymaps](#Controlling-Active-Maps), for details.

Making keymaps active is not the only way to use them. Keymaps are
also used in other ways, such as for translating events within
`read-key-sequence`. See [Keymaps for Translating Sequences of Events](#Translation-Keymaps).

See [Standard Keymaps](#Standard-Keymaps), for a list of some standard keymaps.

Function: **current-active-maps** &optional olp position [¶](#index-current_002dactive_002dmaps)
:   This returns the list of active keymaps that would be used by the
    command loop in the current circumstances to look up a key sequence.
    Normally it ignores `overriding-local-map` and
    `overriding-terminal-local-map`, but if olp is non-`nil`
    then it pays attention to them. position can optionally be either
    an event position as returned by `event-start` or a buffer
    position, and may change the keymaps as described for
    `keymap-lookup` (see [keymap-lookup](#Functions-for-Key-Lookup)).

---

Next: [Controlling the Active Keymaps](#Controlling-Active-Maps), Previous: [Active Keymaps](#Active-Keymaps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.8 Searching the Active Keymaps [¶](#Searching-the-Active-Keymaps)

Here is a pseudo-Lisp summary of how Emacs searches the active
keymaps:

```
(or (if overriding-terminal-local-map
        (find-in overriding-terminal-local-map))
    (if overriding-local-map
        (find-in overriding-local-map)
      (or (find-in (get-char-property (point) 'keymap))
          (find-in-any emulation-mode-map-alists)
          (find-in-any minor-mode-overriding-map-alist)
          (find-in-any minor-mode-map-alist)
          (if (get-char-property (point) 'local-map)
              (find-in (get-char-property (point) 'local-map))
            (find-in (current-local-map)))))
    (find-in (current-global-map)))
```

Here, find-in and find-in-any are pseudo functions that
search in one keymap and in an alist of keymaps, respectively. Note
that the `set-transient-map` function works by setting
`overriding-terminal-local-map` (see [Controlling the Active Keymaps](#Controlling-Active-Maps)).

In the above pseudo-code, if a key sequence starts with a mouse
event (see [Mouse Events](#Mouse-Events)), that event’s position is used instead of
point, and the event’s buffer is used instead of the current buffer.
In particular, this affects how the `keymap` and `local-map`
properties are looked up. If a mouse event occurs on a string
embedded with a `display`, `before-string`, or
`after-string` property (see [Properties with Special Meanings](#Special-Properties)), and the
string has a non-`nil` `keymap` or `local-map`
property, that overrides the corresponding property in the underlying
buffer text (i.e., the property specified by the underlying text is
ignored).

When a key binding is found in one of the active keymaps, and that
binding is a command, the search is over—the command is executed.
However, if the binding is a symbol with a value or a string, Emacs
replaces the input key sequences with the variable’s value or the
string, and restarts the search of the active keymaps. See [Key Lookup](#Key-Lookup).

The command which is finally found might also be remapped.
See [Remapping Commands](#Remapping-Commands).

---

Next: [Key Lookup](#Key-Lookup), Previous: [Searching the Active Keymaps](#Searching-Keymaps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.9 Controlling the Active Keymaps [¶](#Controlling-the-Active-Keymaps)

Variable: **global-map** [¶](#index-global_002dmap)
:   This variable contains the default global keymap that maps Emacs
    keyboard input to commands. The global keymap is normally this
    keymap. The default global keymap is a full keymap that binds
    `self-insert-command` to all of the printing characters.

    It is normal practice to change the bindings in the global keymap, but you
    should not assign this variable any value other than the keymap it starts
    out with.

Function: **current-global-map** [¶](#index-current_002dglobal_002dmap)
:   This function returns the current global keymap. This is the same as
    the value of `global-map` unless you change one or the other.
    The return value is a reference, not a copy; if you use
    `keymap-set` or other functions on it you will alter global
    bindings.

    ```
    (current-global-map)
    ⇒ (keymap [set-mark-command beginning-of-line ...
                delete-backward-char])
    ```

Function: **current-local-map** [¶](#index-current_002dlocal_002dmap)
:   This function returns the current buffer’s local keymap, or `nil`
    if it has none. In the following example, the keymap for the
    `*scratch*` buffer (using Lisp Interaction mode) is a sparse keymap
    in which the entry for `ESC`, ASCII code 27, is another sparse
    keymap.

    ```
    (current-local-map)
    ⇒ (keymap
        (10 . eval-print-last-sexp)
        (9 . lisp-indent-line)
        (127 . backward-delete-char-untabify)
    ```

    ```
        (27 keymap
            (24 . eval-defun)
            (17 . indent-sexp)))
    ```

`current-local-map` returns a reference to the local keymap, not
a copy of it; if you use `keymap-set` or other functions on it
you will alter local bindings.

Function: **current-minor-mode-maps** [¶](#index-current_002dminor_002dmode_002dmaps)
:   This function returns a list of the keymaps of currently enabled minor modes.

Function: **use-global-map** keymap [¶](#index-use_002dglobal_002dmap)
:   This function makes keymap the new current global keymap. It
    returns `nil`.

    It is very unusual to change the global keymap.

Function: **use-local-map** keymap [¶](#index-use_002dlocal_002dmap)
:   This function makes keymap the new local keymap of the current
    buffer. If keymap is `nil`, then the buffer has no local
    keymap. `use-local-map` returns `nil`. Most major mode
    commands use this function.

Variable: **minor-mode-map-alist** [¶](#index-minor_002dmode_002dmap_002dalist)
:   This variable is an alist describing keymaps that may or may not be
    active according to the values of certain variables. Its elements look
    like this:

    ```
    (variable . keymap)
    ```

    The keymap keymap is active whenever variable has a
    non-`nil` value. Typically variable is the variable that
    enables or disables a minor mode. See [Keymaps and Minor Modes](#Keymaps-and-Minor-Modes).

    Note that elements of `minor-mode-map-alist` do not have the same
    structure as elements of `minor-mode-alist`. The map must be the
    CDR of the element; a list with the map as the second element will
    not do. The CDR can be either a keymap (a list) or a symbol whose
    function definition is a keymap.

    When more than one minor mode keymap is active, the earlier one in
    `minor-mode-map-alist` takes priority. But you should design
    minor modes so that they don’t interfere with each other. If you do
    this properly, the order will not matter.

    See [Keymaps and Minor Modes](#Keymaps-and-Minor-Modes), for more information about minor modes.
    See also `minor-mode-key-binding` (see [Functions for Key Lookup](#Functions-for-Key-Lookup)).

Variable: **minor-mode-overriding-map-alist** [¶](#index-minor_002dmode_002doverriding_002dmap_002dalist)
:   This variable allows major modes to override the key bindings for
    particular minor modes. The elements of this alist look like the
    elements of `minor-mode-map-alist`: `(variable
    . keymap)`.

    If a variable appears as an element of
    `minor-mode-overriding-map-alist`, the map specified by that
    element totally replaces any map specified for the same variable in
    `minor-mode-map-alist`.

    `minor-mode-overriding-map-alist` is automatically buffer-local in
    all buffers.

Variable: **overriding-local-map** [¶](#index-overriding_002dlocal_002dmap)
:   If non-`nil`, this variable holds a keymap to use instead of the
    buffer’s local keymap, any text property or overlay keymaps, and any
    minor mode keymaps. This keymap, if specified, overrides all other
    maps that would have been active, except for the current global map.

Variable: **overriding-terminal-local-map** [¶](#index-overriding_002dterminal_002dlocal_002dmap)
:   If non-`nil`, this variable holds a keymap to use instead of
    `overriding-local-map`, the buffer’s local keymap, text property
    or overlay keymaps, and all the minor mode keymaps.

    This variable is always local to the current terminal and cannot be
    buffer-local. See [Multiple Terminals](#Multiple-Terminals). It is used to implement
    incremental search mode.

Variable: **overriding-local-map-menu-flag** [¶](#index-overriding_002dlocal_002dmap_002dmenu_002dflag)
:   If this variable is non-`nil`, the value of
    `overriding-local-map` or `overriding-terminal-local-map` can
    affect the display of the menu bar. The default value is `nil`, so
    those map variables have no effect on the menu bar.

    Note that these two map variables do affect the execution of key
    sequences entered using the menu bar, even if they do not affect the
    menu bar display. So if a menu bar key sequence comes in, you should
    clear the variables before looking up and executing that key sequence.
    Modes that use the variables would typically do this anyway; normally
    they respond to events that they do not handle by “unreading” them and
    exiting.

Variable: **special-event-map** [¶](#index-special_002devent_002dmap)
:   This variable holds a keymap for special events. If an event type has a
    binding in this keymap, then it is special, and the binding for the
    event is run directly by `read-event`. See [Special Events](#Special-Events).

Variable: **emulation-mode-map-alists** [¶](#index-emulation_002dmode_002dmap_002dalists)
:   This variable holds a list of keymap alists to use for emulation
    modes. It is intended for modes or packages using multiple minor-mode
    keymaps. Each element is a keymap alist which has the same format and
    meaning as `minor-mode-map-alist`, or a symbol with a variable
    binding which is such an alist. The active keymaps in each alist
    are used before `minor-mode-map-alist` and
    `minor-mode-overriding-map-alist`.

Function: **set-transient-map** keymap &optional keep-pred on-exit message timeout [¶](#index-set_002dtransient_002dmap)
:   This function adds keymap as a *transient* keymap, which
    takes precedence over other keymaps for one (or more) subsequent keys.

    Normally, keymap is used just once, to look up the very next key.
    If the optional argument keep-pred is `t`, the map stays
    active as long as the user types keys defined in keymap; when the
    user types a key that is not in keymap, the transient keymap is
    deactivated and normal key lookup continues for that key.

    The keep-pred argument can also be a function. In that case, the
    function is called with no arguments, prior to running each command,
    while keymap is active; it should return non-`nil` if
    keymap should stay active.

    The optional argument on-exit, if non-`nil`, specifies a
    function that is called, with no arguments, after keymap is
    deactivated.

    The optional argument message specifies the message to display
    after activating the transient map. If message is a string, it
    is the format string for the message, and any ‘`%k`’ specifier in
    that string is replaced with the list of keys from the transient map.
    Any other non-`nil` value of message stands for the default
    message format ‘`Repeat with %k`’.

    If the optional argument timeout is non-`nil`, it should be
    a number that specifies how many seconds of idle time to wait before
    deactivating keymap. The value of the variable
    `set-transient-map-timeout`, if non-`nil`, overrides the
    value of this argument.

    This function works by adding and removing keymap from the
    variable `overriding-terminal-local-map`, which takes precedence
    over all other active keymaps (see [Searching the Active Keymaps](#Searching-Keymaps)).

---

Next: [Functions for Key Lookup](#Functions-for-Key-Lookup), Previous: [Controlling the Active Keymaps](#Controlling-Active-Maps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.10 Key Lookup [¶](#Key-Lookup-1)

*Key lookup* is the process of finding the binding of a key
sequence from a given keymap. The execution or use of the binding is
not part of key lookup.

Key lookup uses just the event type of each event in the key sequence;
the rest of the event is ignored. In fact, a key sequence used for key
lookup may designate a mouse event with just its types (a symbol)
instead of the entire event (a list). See [Input Events](#Input-Events). Such
a key sequence is insufficient for `command-execute` to run,
but it is sufficient for looking up or rebinding a key.

When the key sequence consists of multiple events, key lookup
processes the events sequentially: the binding of the first event is
found, and must be a keymap; then the second event’s binding is found in
that keymap, and so on until all the events in the key sequence are used
up. (The binding thus found for the last event may or may not be a
keymap.) Thus, the process of key lookup is defined in terms of a
simpler process for looking up a single event in a keymap. How that is
done depends on the type of object associated with the event in that
keymap.

Let’s use the term *keymap entry* to describe the value found by
looking up an event type in a keymap. (This doesn’t include the item
string and other extra elements in a keymap element for a menu item, because
`keymap-lookup` and other key lookup functions don’t include them in
the returned value.) While any Lisp object may be stored in a keymap
as a keymap entry, not all make sense for key lookup. Here is a table
of the meaningful types of keymap entries:

`nil` [¶](#index-nil-in-keymap)
:   `nil` means that the events used so far in the lookup form an
    undefined key. When a keymap fails to mention an event type at all, and
    has no default binding, that is equivalent to a binding of `nil`
    for that event type.

command [¶](#index-command-in-keymap)
:   The events used so far in the lookup form a complete key,
    and command is its binding. See [What Is a Function?](#What-Is-a-Function).

array [¶](#index-string-in-keymap)
:   The array (either a string or a vector) is a keyboard macro. The events
    used so far in the lookup form a complete key, and the array is its
    binding. See [Keyboard Macros](#Keyboard-Macros), for more information.

keymap [¶](#index-keymap-in-keymap)
:   The events used so far in the lookup form a prefix key. The next
    event of the key sequence is looked up in keymap.

list [¶](#index-list-in-keymap)
:   The meaning of a list depends on what it contains:

    * If the CAR of list is the symbol `keymap`, then the list
      is a keymap, and is treated as a keymap (see above).
    * If the CAR of list is `lambda`, then the list is a
      lambda expression. This is presumed to be a function, and is treated
      as such (see above). In order to execute properly as a key binding,
      this function must be a command—it must have an `interactive`
      specification. See [Defining Commands](#Defining-Commands).

symbol [¶](#index-symbol-in-keymap)
:   The function definition of symbol is used in place of
    symbol. If that too is a symbol, then this process is repeated,
    any number of times. Ultimately this should lead to an object that is
    a keymap, a command, or a keyboard macro.

    Note that keymaps and keyboard macros (strings and vectors) are not
    valid functions, so a symbol with a keymap, string, or vector as its
    function definition is invalid as a function. It is, however, valid as
    a key binding. If the definition is a keyboard macro, then the symbol
    is also valid as an argument to `command-execute`
    (see [Interactive Call](#Interactive-Call)).

    The symbol `undefined` is worth special mention: it means to treat
    the key as undefined. Strictly speaking, the key is defined, and its
    binding is the command `undefined`; but that command does the same
    thing that is done automatically for an undefined key: it rings the bell
    (by calling `ding`) but does not signal an error.

    `undefined` is used in local keymaps to override a global key
    binding and make the key undefined locally. A local binding of
    `nil` would fail to do this because it would not override the
    global binding.

anything else
:   If any other type of object is found, the events used so far in the
    lookup form a complete key, and the object is its binding, but the
    binding is not executable as a command.

In short, a keymap entry may be a keymap, a command, a keyboard
macro, a symbol that leads to one of them, or `nil`.

---

Next: [Key Sequences](#Key-Sequences), Previous: [Key Lookup](#Key-Lookup), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.11 Functions for Key Lookup [¶](#Functions-for-Key-Lookup-1)

Here are the functions and variables pertaining to key lookup.

Function: **keymap-lookup** keymap key &optional accept-defaults no-remap position [¶](#index-keymap_002dlookup)
:   This function returns the definition of key in keymap. All
    the other functions described in this chapter that look up keys use
    `keymap-lookup`. Here are examples:

    ```
    (keymap-lookup (current-global-map) "C-x C-f")
        ⇒ find-file
    ```

    ```
    (keymap-lookup (current-global-map) "C-x C-f 1 2 3 4 5")
        ⇒ 2
    ```

    If the string or vector key is not a valid key sequence according
    to the prefix keys specified in keymap, it must be too long
    and have extra events at the end that do not fit into a single key
    sequence. Then the value is a number, the number of events at the front
    of key that compose a complete key.

    If accept-defaults is non-`nil`, then `keymap-lookup`
    considers default bindings as well as bindings for the specific events
    in key. Otherwise, `keymap-lookup` reports only bindings for
    the specific sequence key, ignoring default bindings except when
    you explicitly ask about them. (To do this, supply `t` as an
    element of key; see [Format of Keymaps](#Format-of-Keymaps).)

    If key contains a meta character (not a function key), that
    character is implicitly replaced by a two-character sequence: the value
    of `meta-prefix-char`, followed by the corresponding non-meta
    character. Thus, the first example below is handled by conversion into
    the second example.

    ```
    (keymap-lookup (current-global-map) "M-f")
        ⇒ forward-word
    ```

    ```
    (keymap-lookup (current-global-map) "ESC f")
        ⇒ forward-word
    ```

    The keymap argument can be `nil`, meaning to look up
    key in the current keymaps (as returned by
    `current-active-maps`, see [Active Keymaps](#Active-Keymaps)); or it can be a
    keymap or a list of keymaps, meaning to look up key only in the
    specified keymaps.

    Unlike `read-key-sequence`, this function does not modify the
    specified events in ways that discard information (see [Key Sequence Input](#Key-Sequence-Input)). In particular, it does not convert letters to lower case and
    it does not change drag events to clicks.

    Like the normal command loop, `keymap-lookup` will remap the
    command resulting from looking up key by looking up the command
    in the current keymaps. However, if the optional third argument
    no-remap is non-`nil`, `keymap-lookup` returns the
    command without remapping.

    If the optional argument position is non-`nil`, it
    specifies a mouse position as returned by `event-start` and
    `event-end`, and the lookup occurs in the keymaps associated with
    that position, instead of in keymap. position can also be
    a number or a marker, in which case it is interpreted as a buffer
    position, and the function uses the keymap properties at that position
    instead of at point.

Command: **undefined** [¶](#index-undefined)
:   Used in keymaps to undefine keys. It calls `ding`, but does
    not cause an error.

Function: **keymap-local-lookup** key &optional accept-defaults [¶](#index-keymap_002dlocal_002dlookup)
:   This function returns the binding for key in the current
    local keymap, or `nil` if it is undefined there.

    The argument accept-defaults controls checking for default bindings,
    as in `keymap-lookup` (above).

Function: **keymap-global-lookup** key &optional accept-defaults [¶](#index-keymap_002dglobal_002dlookup)
:   This function returns the binding for command key in the
    current global keymap, or `nil` if it is undefined there.

    The argument accept-defaults controls checking for default bindings,
    as in `keymap-lookup` (above).

Function: **minor-mode-key-binding** key &optional accept-defaults [¶](#index-minor_002dmode_002dkey_002dbinding)
:   This function returns a list of all the active minor mode bindings of
    key. More precisely, it returns an alist of pairs
    `(modename . binding)`, where modename is the
    variable that enables the minor mode, and binding is key’s
    binding in that mode. If key has no minor-mode bindings, the
    value is `nil`.

    If the first binding found is not a prefix definition (a keymap or a
    symbol defined as a keymap), all subsequent bindings from other minor
    modes are omitted, since they would be completely shadowed. Similarly,
    the list omits non-prefix bindings that follow prefix bindings.

    The argument accept-defaults controls checking for default
    bindings, as in `keymap-lookup` (above).

User Option: **meta-prefix-char** [¶](#index-meta_002dprefix_002dchar)
:   This variable is the meta-prefix character code. It is used for
    translating a meta character to a two-character sequence so it can be
    looked up in a keymap. For useful results, the value should be a
    prefix event (see [Prefix Keys](#Prefix-Keys)). The default value is 27, which is
    the ASCII code for `ESC`.

    As long as the value of `meta-prefix-char` remains 27, key lookup
    translates `M-b` into `ESC b`, which is normally defined
    as the `backward-word` command. However, if you were to set
    `meta-prefix-char` to 24, the code for `C-x`, then Emacs will
    translate `M-b` into `C-x b`, whose standard binding is the
    `switch-to-buffer` command. (Don’t actually do this!) Here is an
    illustration of what would happen:

    ```
    meta-prefix-char                    ; The default value.
         ⇒ 27
    ```

    ```
    (key-binding "\M-b")
         ⇒ backward-word
    ```

    ```
    ?\C-x                               ; The print representation
         ⇒ 24                          ;   of a character.
    ```

    ```
    (setq meta-prefix-char 24)
         ⇒ 24
    ```

    ```
    (key-binding "\M-b")
         ⇒ switch-to-buffer            ; Now, typing M-b is
                                        ;   like typing C-x b.

    (setq meta-prefix-char 27)          ; Avoid confusion!
         ⇒ 27                          ; Restore the default value!
    ```

    This translation of one event into two happens only for characters, not
    for other kinds of input events. Thus, `M-F1`, a function
    key, is not converted into `ESC F1`.

---

Next: [Low-Level Key Binding](#Low_002dLevel-Key-Binding), Previous: [Functions for Key Lookup](#Functions-for-Key-Lookup), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.12 Key Sequences [¶](#Key-Sequences-1)

A *key sequence*, or *key* for short, is a sequence of one
or more input events that form a unit. Input events include
characters, function keys, mouse actions, or system events external to
Emacs, such as `iconify-frame` (see [Input Events](#Input-Events)).
The Emacs Lisp representation for a key sequence is a string or
vector. Unless otherwise stated, any Emacs Lisp function that accepts
a key sequence as an argument can handle both representations.

In the string representation, alphanumeric characters ordinarily
stand for themselves; for example, `"a"` represents `a`
and `"2"` represents `2`. Control character events are
prefixed by the substring `"\C-"`, and meta characters by
`"\M-"`; for example, `"\C-x"` represents the key `C-x`.
In addition, the `TAB`, `RET`, `ESC`, and `DEL` events
are represented by `"\t"`, `"\r"`, `"\e"`, and
`"\d"` respectively. The string representation of a complete key
sequence is the concatenation of the string representations of the
constituent events; thus, `"\C-xl"` represents the key sequence
`C-x l`.

Key sequences containing function keys, mouse button events, system
events, or non-ASCII characters such as `C-=` or
`H-a` cannot be represented as strings; they have to be
represented as vectors.

In the vector representation, each element of the vector represents
an input event, in its Lisp form. See [Input Events](#Input-Events). For example,
the vector `[?\C-x ?l]` represents the key sequence `C-x l`.

For examples of key sequences written in string and vector
representations, [Init Rebinding](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-Rebinding) in The GNU Emacs Manual.

Function: **kbd** keyseq-text [¶](#index-kbd)
:   This function converts the text keyseq-text (a string constant)
    into a key sequence (a string or vector constant). The contents of
    keyseq-text should use the same syntax as in the buffer invoked
    by the `C-x C-k RET` (`kmacro-edit-macro`) command; in
    particular, you must surround function key names with
    ‘`<…>`’. See [Edit Keyboard Macro](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Edit-Keyboard-Macro) in The GNU Emacs
    Manual.

    ```
    (kbd "C-x") ⇒ "\C-x"
    (kbd "C-x C-f") ⇒ "\C-x\C-f"
    (kbd "C-x 4 C-f") ⇒ "\C-x4\C-f"
    (kbd "X") ⇒ "X"
    (kbd "RET") ⇒ "^M"
    (kbd "C-c SPC") ⇒ "\C-c "
    (kbd "<f1> SPC") ⇒ [f1 32]
    (kbd "C-M-<down>") ⇒ [C-M-down]
    ```

    The `kbd` function is very permissive, and will try to return
    something sensible even if the syntax used isn’t completely
    conforming. To check whether the syntax is actually valid, use the
    `key-valid-p` function.

---

Next: [Remapping Commands](#Remapping-Commands), Previous: [Key Sequences](#Key-Sequences), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.13 Low-Level Key Binding [¶](#Low_002dLevel-Key-Binding-1)

Historically, Emacs has supported a number of different syntaxes for
defining keys. The documented way to bind a key today is to use the
syntax supported by `key-valid-p`, which is what all the
functions like `keymap-set` and `keymap-lookup` supports.
This section documents the old-style syntax and interface functions;
they should not be used in new code.

`define-key` (and other low-level functions that are used to
rebind keys) understand a number of different syntaxes for the keys.

A vector containing lists of keys.
:   You can use a list containing modifier names plus one base event (a
    character or function key name). For example, `[(control ?a)
    (meta b)]` is equivalent to `C-a M-b` and `[(hyper control
    left)]` is equivalent to `C-H-left`.

A string of characters with modifiers
:   Internally, key sequences are often represented as strings using the
    special escape sequences for shift, control and meta modifiers
    (see [String Type](#String-Type)), but this representation can also be used by
    users when rebinding keys. A string like `"\M-x"` is read as
    containing a single `M-x`, `"\C-f"` is read as containing a
    single `C-f`, and `"\M-\C-x"` and `"\C-\M-x"` are both
    read as containing a single `C-M-x`.

A vector of characters and key symbols
:   This is the other internal representation of key sequences. It
    supports a fuller range of modifiers than the string representation,
    and also support function keys. An example is ‘`[?\C-\H-x home]`’, which represents the `C-H-x home` key sequence.
    See [Character Type](#Character-Type).

Function: **define-key** keymap key binding &optional remove [¶](#index-define_002dkey)
:   This function is like `keymap-set` (see [Changing Key Bindings](#Changing-Key-Bindings), but understands only the legacy key syntaxes.

    In addition, this function also has a remove argument. If it is
    non-`nil`, the definition will be removed. This is almost the
    same as setting the definition to `nil`, but makes a difference
    if the keymap has a parent, and key is shadowing the same
    binding in the parent. With remove, subsequent lookups will
    return the binding in the parent, whereas with a `nil` definition the
    lookups will return `nil`.

Here are other legacy key definition functions and commands, with the
equivalent modern function to use instead in new code.

Command: **global-set-key** key binding [¶](#index-global_002dset_002dkey)
:   This function sets the binding of key in the current global map
    to binding. Use `keymap-global-set` instead.

Command: **global-unset-key** key [¶](#index-global_002dunset_002dkey)
:   This function removes the binding of key from the current
    global map. Use `keymap-global-unset` instead.

Command: **local-set-key** key binding [¶](#index-local_002dset_002dkey)
:   This function sets the binding of key in the current local
    keymap to binding. Use `keymap-local-set` instead.

Command: **local-unset-key** key [¶](#index-local_002dunset_002dkey)
:   This function removes the binding of key from the current
    local map. Use `keymap-local-unset` instead.

Function: **substitute-key-definition** olddef newdef keymap &optional oldmap [¶](#index-substitute_002dkey_002ddefinition-1)
:   This function replaces olddef with newdef for any keys in
    keymap that were bound to olddef. In other words,
    olddef is replaced with newdef wherever it appears. The
    function returns `nil`. Use `keymap-substitute` instead.

Function: **define-key-after** map key binding &optional after [¶](#index-define_002dkey_002dafter)
:   Define a binding in map for key, with value binding,
    just like `define-key`, but position the binding in map after
    the binding for the event after. The argument key should be
    of length one—a vector or string with just one element. But
    after should be a single event type—a symbol or a character, not
    a sequence. The new binding goes after the binding for after. If
    after is `t` or is omitted, then the new binding goes last, at
    the end of the keymap. However, new bindings are added before any
    inherited keymap. Use `keymap-set-after` instead of this function.

Function: **keyboard-translate** from to [¶](#index-keyboard_002dtranslate)
:   This function modifies `keyboard-translate-table` to translate
    character code from into character code to. It creates
    the keyboard translate table if necessary. Use `key-translate`
    instead.

Function: **key-binding** key &optional accept-defaults no-remap position [¶](#index-key_002dbinding)
:   This function returns the binding for key according to the
    current active keymaps. The result is `nil` if key is
    undefined in the keymaps. The argument accept-defaults controls
    checking for default bindings, as in `lookup-key`
    (see [Functions for Key Lookup](#Functions-for-Key-Lookup)). If no-remap is
    non-`nil`, `key-binding` ignores command remappings
    (see [Remapping Commands](#Remapping-Commands)) and returns the binding directly
    specified for key. The optional argument position should
    be either a buffer position or an event position like the value of
    `event-start`; it tells the function to consult the maps
    determined based on that position.

    Emacs signals an error if key is not a string or a vector.

    Use `keymap-lookup` instead of this function.

Function: **lookup-key** keymap key &optional accept-defaults [¶](#index-lookup_002dkey)
:   This function returns the definition of key in keymap. If
    the string or vector key is not a valid key sequence according
    to the prefix keys specified in keymap, it must be too long and
    have extra events at the end that do not fit into a single key
    sequence. Then the value is a number, the number of events at the
    front of key that compose a complete key.

    If accept-defaults is non-`nil`, then `lookup-key`
    considers default bindings as well as bindings for the specific events
    in key. Otherwise, `lookup-key` reports only bindings for
    the specific sequence key, ignoring default bindings except when
    you explicitly ask about them.

    Use `keymap-lookup` instead of this function.

Function: **local-key-binding** key &optional accept-defaults [¶](#index-local_002dkey_002dbinding)
:   This function returns the binding for key in the current
    local keymap, or `nil` if it is undefined there.

    The argument accept-defaults controls checking for default bindings,
    as in `lookup-key` (above).

    Use `keymap-local-lookup` instead of this function.

Function: **global-key-binding** key &optional accept-defaults [¶](#index-global_002dkey_002dbinding)
:   This function returns the binding for command key in the
    current global keymap, or `nil` if it is undefined there.

    The argument accept-defaults controls checking for default bindings,
    as in `lookup-key` (above).

    Use `keymap-global-lookup` instead of this function.

Function: **event-convert-list** list [¶](#index-event_002dconvert_002dlist)
:   This function converts a list of modifier names and a basic event type
    to an event type which specifies all of them. The basic event type
    must be the last element of the list. For example,

    ```
    (event-convert-list '(control ?a))
         ⇒ 1
    (event-convert-list '(control meta ?a))
         ⇒ -134217727
    (event-convert-list '(control super f1))
         ⇒ C-s-f1
    ```

---

Next: [Keymaps for Translating Sequences of Events](#Translation-Keymaps), Previous: [Low-Level Key Binding](#Low_002dLevel-Key-Binding), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.14 Remapping Commands [¶](#Remapping-Commands-1)

A special kind of key binding can be used to *remap* one command
to another, without having to refer to the key sequence(s) bound to
the original command. To use this feature, make a key binding for a
key sequence that starts with the dummy event `remap`, followed
by the command name you want to remap; for the binding, specify the
new definition (usually a command name, but possibly any other valid
definition for a key binding).

For example, suppose My mode provides a special command
`my-kill-line`, which should be invoked instead of
`kill-line`. To establish this, its mode keymap should contain
the following remapping:

```
(keymap-set my-mode-map "<remap> <kill-line>" 'my-kill-line)
```

Then, whenever `my-mode-map` is active, if the user types
`C-k` (the default global key sequence for `kill-line`) Emacs
will instead run `my-kill-line`.

Note that remapping only takes place through active keymaps; for
example, putting a remapping in a prefix keymap like `ctl-x-map`
typically has no effect, as such keymaps are not themselves active.
In addition, remapping only works through a single level; in the
following example,

```
(keymap-set my-mode-map "<remap> <kill-line>" 'my-kill-line)
(keymap-set my-mode-map "<remap> <my-kill-line>" 'my-other-kill-line)
```

`kill-line` is *not* remapped to `my-other-kill-line`.
Instead, if an ordinary key binding specifies `kill-line`, it is
remapped to `my-kill-line`; if an ordinary binding specifies
`my-kill-line`, it is remapped to `my-other-kill-line`.

To undo the remapping of a command, remap it to `nil`; e.g.,

```
(keymap-set my-mode-map "<remap> <kill-line>" nil)
```

Function: **command-remapping** command &optional position keymaps [¶](#index-command_002dremapping)
:   This function returns the remapping for command (a symbol),
    given the current active keymaps. If command is not remapped
    (which is the usual situation), or not a symbol, the function returns
    `nil`. `position` can optionally specify a buffer position
    or an event position to determine the keymaps to use, as in
    `key-binding`.

    If the optional argument `keymaps` is non-`nil`, it
    specifies a list of keymaps to search in. This argument is ignored if
    `position` is non-`nil`.

---

Next: [Commands for Binding Keys](#Key-Binding-Commands), Previous: [Remapping Commands](#Remapping-Commands), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.15 Keymaps for Translating Sequences of Events [¶](#Keymaps-for-Translating-Sequences-of-Events)

When the `read-key-sequence` function reads a key sequence
(see [Key Sequence Input](#Key-Sequence-Input)), it uses *translation keymaps* to
translate certain event sequences into others. The translation
keymaps are `input-decode-map`, `local-function-key-map`,
and `key-translation-map` (in order of priority).

Translation keymaps have the same structure as other keymaps, but
are used differently: they specify translations to make while reading
key sequences, rather than bindings for complete key sequences. As
each key sequence is read, it is checked against each translation
keymap. If one of the translation keymaps binds k to a
vector v, then whenever k appears as a sub-sequence
*anywhere* in a key sequence, that sub-sequence is replaced with
the events in v.

For example, VT100 terminals send `ESC O P` when the
keypad key `PF1` is pressed. On such terminals, Emacs must
translate that sequence of events into a single event `pf1`.
This is done by binding `ESC O P` to `[pf1]` in
`input-decode-map`. Thus, when you type `C-c PF1` on
the terminal, the terminal emits the character sequence `C-c
ESC O P`, and `read-key-sequence` translates this back into
`C-c PF1` and returns it as the vector `[?\C-c pf1]`.

Translation keymaps take effect only after Emacs has decoded the
keyboard input (via the input coding system specified by
`keyboard-coding-system`). See [Terminal I/O Encoding](#Terminal-I_002fO-Encoding).

Variable: **input-decode-map** [¶](#index-input_002ddecode_002dmap)
:   This variable holds a keymap that describes the character sequences sent
    by function keys on an ordinary character terminal.

    The value of `input-decode-map` is usually set up automatically
    according to the terminal’s Terminfo or Termcap entry, but sometimes
    those need help from terminal-specific Lisp files. Emacs comes with
    terminal-specific files for many common terminals; their main purpose is
    to make entries in `input-decode-map` beyond those that can be
    deduced from Termcap and Terminfo. See [Terminal-Specific Initialization](#Terminal_002dSpecific).

Variable: **local-function-key-map** [¶](#index-local_002dfunction_002dkey_002dmap)
:   This variable holds a keymap similar to `input-decode-map` except
    that it describes key sequences which should be translated to
    alternative interpretations that are usually preferred. It applies
    after `input-decode-map` and before `key-translation-map`.

    Entries in `local-function-key-map` are ignored if they conflict
    with bindings made in the minor mode, local, or global keymaps. I.e.,
    the remapping only applies if the original key sequence would
    otherwise not have any binding.

    `local-function-key-map` inherits from `function-key-map`.
    The latter should only be altered if you want the binding to apply in
    all terminals, so using the former is almost always preferred.

Variable: **key-translation-map** [¶](#index-key_002dtranslation_002dmap)
:   This variable is another keymap used just like `input-decode-map`
    to translate input events into other events. It differs from
    `input-decode-map` in that it goes to work after
    `local-function-key-map` is finished rather than before; it
    receives the results of translation by `local-function-key-map`.

    Just like `input-decode-map`, but unlike
    `local-function-key-map`, this keymap is applied regardless of
    whether the input key-sequence has a normal binding. Note however
    that actual key bindings can have an effect on
    `key-translation-map`, even though they are overridden by it.
    Indeed, actual key bindings override `local-function-key-map` and
    thus may alter the key sequence that `key-translation-map`
    receives. Clearly, it is better to avoid this type of situation.

    The intent of `key-translation-map` is for users to map one
    character set to another, including ordinary characters normally bound
    to `self-insert-command`.

You can use `input-decode-map`, `local-function-key-map`,
and `key-translation-map` for more than simple aliases, by using
a function, instead of a key sequence, as the translation of a
key. Then this function is called to compute the translation of that
key.

The key translation function receives one argument, which is the prompt
that was specified in `read-key-sequence`—or `nil` if the
key sequence is being read by the editor command loop. In most cases
you can ignore the prompt value.

If the function reads input itself, it can have the effect of altering
the event that follows. For example, here’s how to define `C-c h`
to turn the character that follows into a Hyper character:

```
(defun hyperify (prompt)
  (let ((e (read-event)))
    (vector (if (numberp e)
                (logior (ash 1 24) e)
              (if (memq 'hyper (event-modifiers e))
                  e
                (add-event-modifier "H-" e))))))

(defun add-event-modifier (string e)
  (let ((symbol (if (symbolp e) e (car e))))
    (setq symbol (intern (concat string
                                 (symbol-name symbol))))
    (if (symbolp e)
        symbol
      (cons symbol (cdr e)))))

(keymap-set local-function-key-map "C-c h" 'hyperify)
```

A key translation function might want to adjust its behavior based on
parameters to events within a key sequence containing non-key events
(see [Input Events](#Input-Events).) This information is available from the
variable `current-key-remap-sequence`, which is bound to the key
sub-sequence being translated around calls to key translation
functions.

* [Interaction with normal keymaps](#Interaction-with-normal-keymaps)

#### 23.15.1 Interaction with normal keymaps [¶](#Interaction-with-normal-keymaps)

The end of a key sequence is detected when that key sequence either is bound
to a command, or when Emacs determines that no additional event can lead
to a sequence that is bound to a command.

This means that, while `input-decode-map` and `key-translation-map`
apply regardless of whether the original key sequence would have a binding, the
presence of such a binding can still prevent translation from taking place.
For example, let us return to our VT100 example above and add a binding for
`C-c ESC` to the global map; now when the user hits `C-c
PF1` Emacs will fail to decode `C-c ESC O P` into `C-c
PF1` because it will stop reading keys right after `C-c ESC`,
leaving `O P` for later. This is in case the user really hit `C-c
ESC`, in which case Emacs should not sit there waiting for the next key
to decide whether the user really pressed `ESC` or `PF1`.

For that reason, it is better to avoid binding commands to key sequences where
the end of the key sequence is a prefix of a key translation. The main such
problematic suffixes/prefixes are `ESC`, `M-O` (which is really
`ESC O`) and `M-[` (which is really `ESC [`).

---

Next: [Scanning Keymaps](#Scanning-Keymaps), Previous: [Keymaps for Translating Sequences of Events](#Translation-Keymaps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.16 Commands for Binding Keys [¶](#Commands-for-Binding-Keys)

This section describes some convenient interactive interfaces for
changing key bindings. They work by calling `keymap-set`
(see [Changing Key Bindings](#Changing-Key-Bindings)). In interactive use, these commands
prompt for the argument key and expect the user to type a valid
key sequence; they also prompt for the binding of the key
sequence, and expect the name of a command (i.e., a symbol that
satisfies `commandp`, see [Interactive Call](#Interactive-Call)). When called
from Lisp, these commands expect key to be a string that
satisfies `key-valid-p` (see [Key Sequences](#Key-Sequences)), and
binding to be any Lisp object that is meaningful in a keymap
(see [Key Lookup](#Key-Lookup)).

People often use `keymap-global-set` in their init files
(see [The Init File](#Init-File)) for simple customization. For example,

```
(keymap-global-set "C-x C-\\" 'next-line)
```

redefines `C-x C-\` to move down a line.

```
(keymap-global-set "M-<mouse-1>" 'mouse-set-point)
```

redefines the first (leftmost) mouse button, entered with the Meta key, to
set point where you click.

Be careful when using non-ASCII text characters in Lisp
specifications of keys to bind. If these are read as multibyte text, as
they usually will be in a Lisp file (see [Loading Non-ASCII Characters](#Loading-Non_002dASCII)), you
must type the keys as multibyte too. For instance, if you use this:

```
(keymap-global-set "ö" 'my-function) ; bind o-umlaut
```

and your language environment is multibyte Latin-1, these commands
actually bind the multibyte character with code 246, not the byte
code 246 (`M-v`) sent by a Latin-1 terminal. In order to use this
binding, you need to teach Emacs how to decode the keyboard by using an
appropriate input method (see [Input Methods](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Input-Methods) in The GNU
Emacs Manual).

Command: **keymap-global-set** key binding [¶](#index-keymap_002dglobal_002dset)
:   This function sets the binding of key in the current global map
    to binding.

    ```
    (keymap-global-set key binding)
    ≡
    (keymap-set (current-global-map) key binding)
    ```

Command: **keymap-global-unset** key [¶](#index-keymap_002dglobal_002dunset)
:   This function removes the binding of key from the current
    global map.

    One use of this function is in preparation for defining a longer key
    that uses key as a prefix—which would not be allowed if
    key has a non-prefix binding. For example:

    ```
    (keymap-global-unset "C-l")
        ⇒ nil
    ```

    ```
    (keymap-global-set "C-l C-l" 'redraw-display)
        ⇒ nil
    ```

Command: **keymap-local-set** key binding [¶](#index-keymap_002dlocal_002dset)
:   This function sets the binding of key in the current local
    keymap to binding.

    ```
    (keymap-local-set key binding)
    ≡
    (keymap-set (current-local-map) key binding)
    ```

Command: **keymap-local-unset** key [¶](#index-keymap_002dlocal_002dunset)
:   This function removes the binding of key from the current
    local map.

---

Next: [Menu Keymaps](#Menu-Keymaps), Previous: [Commands for Binding Keys](#Key-Binding-Commands), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 23.17 Scanning Keymaps [¶](#Scanning-Keymaps-1)

This section describes functions used to scan all the current
keymaps for the sake of printing help information. To display the
bindings in a particular keymap, you can use the
`describe-keymap` command (see [Other Help
Commands](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Misc-Help) in The GNU Emacs Manual)

Function: **accessible-keymaps** keymap &optional prefix [¶](#index-accessible_002dkeymaps)
:   This function returns a list of all the keymaps that can be reached (via
    zero or more prefix keys) from keymap. The value is an
    association list with elements of the form `(key .
    map)`, where key is a prefix key whose definition in
    keymap is map.

    The elements of the alist are ordered so that the key increases
    in length. The first element is always `([] . keymap)`,
    because the specified keymap is accessible from itself with a prefix of
    no events.

    If prefix is given, it should be a prefix key sequence; then
    `accessible-keymaps` includes only the submaps whose prefixes start
    with prefix. These elements look just as they do in the value of
    `(accessible-keymaps)`; the only difference is that some elements
    are omitted.

    In the example below, the returned alist indicates that the key
    `ESC`, which is displayed as ‘`^[`’, is a prefix key whose
    definition is the sparse keymap `(keymap (83 . center-paragraph)
    (115 . foo))`.

    ```
    (accessible-keymaps (current-local-map))
    ⇒(([] keymap
          (27 keymap   ; Note this keymap for ESC is repeated below.
              (83 . center-paragraph)
              (115 . center-line))
          (9 . tab-to-tab-stop))
    ```

    ```

    ```

    ```
       ("^[" keymap
        (83 . center-paragraph)
        (115 . foo)))
    ```

    In the following example, `C-h` is a prefix key that uses a sparse
    keymap starting with `(keymap (118 . describe-variable)…)`.
    Another prefix, `C-x 4`, uses a keymap which is also the value of
    the variable `ctl-x-4-map`. The event `mode-line` is one of
    several dummy events used as prefixes for mouse actions in special parts
    of a window.

    ```
    (accessible-keymaps (current-global-map))
    ⇒ (([] keymap [set-mark-command beginning-of-line ...
                       delete-backward-char])
    ```

    ```
        ("^H" keymap (118 . describe-variable) ...
         (8 . help-for-help))
    ```

    ```
        ("^X" keymap [x-flush-mouse-queue ...
         backward-kill-sentence])
    ```

    ```
        ("^[" keymap [mark-sexp backward-sexp ...
         backward-kill-word])
    ```

    ```
        ("^X4" keymap (15 . display-buffer) ...)
    ```

    ```
        ([mode-line] keymap
         (S-mouse-2 . mouse-split-window-horizontally) ...))
    ```

    These are not all the keymaps you would see in actuality.

Function: **map-keymap** function keymap [¶](#index-map_002dkeymap)
:   The function `map-keymap` calls function once
    for each binding in keymap. It passes two arguments,
    the event type and the value of the binding. If keymap
    has a parent, the parent’s bindings are included as well.
    This works recursively: if the parent has itself a parent, then the
    grandparent’s bindings are also included and so on.

    This function is the cleanest way to examine all the bindings
    in a keymap.

Function: **where-is-internal** command &optional keymap firstonly noindirect no-remap [¶](#index-where_002dis_002dinternal)
:   This function is a subroutine used by the `where-is` command
    (see [Help](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Help) in The GNU Emacs Manual). It returns a list
    of all key sequences (of any length) that are bound to command in a
    set of keymaps.

    The argument command can be any object; it is compared with all
    keymap entries using `eq`.

    If keymap is `nil`, then the maps used are the current active
    keymaps, disregarding `overriding-local-map` (that is, pretending
    its value is `nil`). If keymap is a keymap, then the
    maps searched are keymap and the global keymap. If keymap
    is a list of keymaps, only those keymaps are searched.

    Usually it’s best to use `overriding-local-map` as the expression
    for keymap. Then `where-is-internal` searches precisely
    the keymaps that are active. To search only the global map, pass the
    value `(keymap)` (an empty keymap) as keymap.

    If firstonly is `non-ascii`, then the value is a single
    vector representing the first key sequence found, rather than a list of
    all possible key sequences. If firstonly is `t`, then the
    value is the first key sequence, except that key sequences consisting
    entirely of ASCII characters (or meta variants of ASCII
    characters) are preferred to all other key sequences and that the
    return value can never be a menu binding.

    If noindirect is non-`nil`, `where-is-internal` doesn’t look
    inside menu-items to find their commands. This makes it possible to search for
    a menu-item itself.

    The fifth argument, no-remap, determines how this function
    treats command remappings (see [Remapping Commands](#Remapping-Commands)). There are two
    cases of interest:

    If a command other-command is remapped to command:
    :   If no-remap is `nil`, find the bindings for
        other-command and treat them as though they are also bindings
        for command. If no-remap is non-`nil`, include the
        vector `[remap other-command]` in the list of possible key
        sequences, instead of finding those bindings.

    If command is remapped to other-command:
    :   If no-remap is `nil`, return the bindings for
        other-command rather than command. If no-remap is
        non-`nil`, return the bindings for command, ignoring the
        fact that it is remapped.

    If a command maps to a key binding like `[some-event]`, and
    `some-event` has a symbol plist containing a non-`nil`
    `non-key-event` property, then that binding is ignored by
    `where-is-internal`.

Command: **describe-bindings** &optional prefix buffer-or-name [¶](#index-describe_002dbindings)
:   This function creates a listing of all current key bindings, and
    displays it in a buffer named `*Help*`. The text is grouped by
    modes—minor modes first, then the major mode, then global bindings.

    If prefix is non-`nil`, it should be a prefix key; then the
    listing includes only keys that start with prefix.

    When several characters with consecutive ASCII codes have the
    same definition, they are shown together, as
    ‘`firstchar..lastchar`’. In this instance, you need to
    know the ASCII codes to understand which characters this means.
    For example, in the default global map, the characters ‘`SPC
    .. ~`’ are described by a single line. `SPC` is ASCII 32,
    `~` is ASCII 126, and the characters between them include all
    the normal printing characters, (e.g., letters, digits, punctuation,
    etc.); all these characters are bound to `self-insert-command`.

    If buffer-or-name is non-`nil`, it should be a buffer or a
    buffer name. Then `describe-bindings` lists that buffer’s bindings,
    instead of the current buffer’s.

---

Previous: [Scanning Keymaps](#Scanning-Keymaps), Up: [Keymaps](#Keymaps)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
