### 42.20 Buttons [¶](#Buttons-1)

The Button package defines functions for inserting and manipulating
*buttons* that can be activated with the mouse or via keyboard
commands. These buttons are typically used for various kinds of
hyperlinks.

A button is essentially a set of text or overlay properties,
attached to a stretch of text in a buffer. These properties are
called *button properties*. One of these properties, the
*action property*, specifies a function which is called when the
user invokes the button using the keyboard or the mouse. The action
function may examine the button and use its other properties as
desired.

In some ways, the Button package duplicates the functionality in the
Widget package. See [Introduction](https://www.gnu.org/software/emacs/manual/html_mono/widget.html#Top) in The Emacs Widget
Library. The advantage of the Button package is that it is faster,
smaller, and simpler to program. From the point of view of the user,
the interfaces produced by the two packages are very similar.

* [Button Properties](#Button-Properties)
* [Button Types](#Button-Types)
* [Making Buttons](#Making-Buttons)
* [Manipulating Buttons](#Manipulating-Buttons)
* [Button Buffer Commands](#Button-Buffer-Commands)

---

Next: [Button Types](#Button-Types), Up: [Buttons](#Buttons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.20.1 Button Properties [¶](#Button-Properties-1)

Each button has an associated list of properties defining its
appearance and behavior, and other arbitrary properties may be used
for application specific purposes. The following properties have
special meaning to the Button package:

`action` [¶](#index-action-_0028button-property_0029)
:   The function to call when the user invokes the button, which is passed
    the single argument button. By default this is `ignore`,
    which does nothing.

`mouse-action` [¶](#index-mouse_002daction-_0028button-property_0029)
:   This is similar to `action`, and when present, will be used
    instead of `action` for button invocations resulting from
    mouse-clicks (instead of the user hitting `RET`). If not
    present, mouse-clicks use `action` instead.

`face` [¶](#index-face-_0028button-property_0029)
:   This is an Emacs face controlling how buttons of this type are
    displayed; by default this is the `button` face.

`mouse-face` [¶](#index-mouse_002dface-_0028button-property_0029)
:   This is an additional face which controls appearance during
    mouse-overs (merged with the usual button face); by default this is
    the usual Emacs `highlight` face.

`keymap` [¶](#index-keymap-_0028button-property_0029)
:   The button’s keymap, defining bindings active within the button
    region. By default this is the usual button region keymap, stored
    in the variable `button-map`, which defines `RET` and
    `mouse-2` to invoke the button.

`type` [¶](#index-type-_0028button-property_0029)
:   The button type. See [Button Types](#Button-Types).

`help-echo` [¶](#index-help_002decho-_0028button-property_0029)
:   A string displayed by the Emacs tooltip help system; by default,
    `"mouse-2, RET: Push this button"`. Alternatively, a function
    that returns, or a form that evaluates to, a string to be displayed or
    `nil`. For details see [Text help-echo](#Text-help_002decho).

    The function is called with three arguments, window,
    object, and pos. The second argument, object, is
    either the overlay that had the property (for overlay buttons), or the
    buffer containing the button (for text property buttons). The other
    arguments have the same meaning as for the special text property
    `help-echo`.

`follow-link` [¶](#index-follow_002dlink-_0028button-property_0029)
:   The `follow-link` property, defining how a `mouse-1` click
    behaves on this button, See [Defining Clickable Text](#Clickable-Text).

`button` [¶](#index-button-_0028button-property_0029)
:   All buttons have a non-`nil` `button` property, which may be useful
    in finding regions of text that comprise buttons (which is what the
    standard button functions do).

There are other properties defined for the regions of text in a
button, but these are not generally interesting for typical uses.

---

Next: [Making Buttons](#Making-Buttons), Previous: [Button Properties](#Button-Properties), Up: [Buttons](#Buttons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.20.2 Button Types [¶](#Button-Types-1)

Every button has a *button type*, which defines default values
for the button’s properties. Button types are arranged in a
hierarchy, with specialized types inheriting from more general types,
so that it’s easy to define special-purpose types of buttons for
specific tasks.

Function: **define-button-type** name &rest properties [¶](#index-define_002dbutton_002dtype)
:   Define a button type called name (a symbol).
    The remaining arguments
    form a sequence of property value pairs, specifying default
    property values for buttons with this type (a button’s type may be set
    by giving it a `type` property when creating the button, using
    the `:type` keyword argument).

    In addition, the keyword argument `:supertype` may be used to
    specify a button-type from which name inherits its default
    property values. Note that this inheritance happens only when
    name is defined; subsequent changes to a supertype are not
    reflected in its subtypes.

Using `define-button-type` to define default properties for
buttons is not necessary—buttons without any specified type use the
built-in button-type `button`—but it is encouraged, since
doing so usually makes the resulting code clearer and more efficient.

---

Next: [Manipulating Buttons](#Manipulating-Buttons), Previous: [Button Types](#Button-Types), Up: [Buttons](#Buttons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.20.3 Making Buttons [¶](#Making-Buttons-1)

Buttons are associated with a region of text, using an overlay or
text properties to hold button-specific information, all of which are
initialized from the button’s type (which defaults to the built-in
button type `button`). Like all Emacs text, the appearance of
the button is governed by the `face` property; by default (via
the `face` property inherited from the `button` button-type)
this is a simple underline, like a typical web-page link.

For convenience, there are two sorts of button-creation functions,
those that add button properties to an existing region of a buffer,
called `make-...button`, and those that also insert the button
text, called `insert-...button`.

The button-creation functions all take the `&rest` argument
properties, which should be a sequence of property value
pairs, specifying properties to add to the button; see [Button Properties](#Button-Properties). In addition, the keyword argument `:type` may be
used to specify a button-type from which to inherit other properties;
see [Button Types](#Button-Types). Any properties not explicitly specified
during creation will be inherited from the button’s type (if the type
defines such a property).

The following functions add a button using an overlay
(see [Overlays](#Overlays)) to hold the button properties:

Function: **make-button** beg end &rest properties [¶](#index-make_002dbutton)
:   This makes a button from beg to end in the
    current buffer, and returns it.

Function: **insert-button** label &rest properties [¶](#index-insert_002dbutton)
:   This inserts a button with the label label at point,
    and returns it.

The following functions are similar, but using text properties
(see [Text Properties](#Text-Properties)) to hold the button properties. Such buttons
do not add markers to the buffer, so editing in the buffer does not
slow down if there is an extremely large numbers of buttons. However,
if there is an existing face text property on the text (e.g., a face
assigned by Font Lock mode), the button face may not be visible. Both
of these functions return the starting position of the new button.

Function: **make-text-button** beg end &rest properties [¶](#index-make_002dtext_002dbutton)
:   This makes a button from beg to end in the current buffer,
    using text properties.

Function: **insert-text-button** label &rest properties [¶](#index-insert_002dtext_002dbutton)
:   This inserts a button with the label label at point, using text
    properties.

Function: **buttonize** string callback &optional data [¶](#index-buttonize)
:   Sometimes it’s more convenient to make a string into a button without
    inserting it into a buffer immediately, for instance when creating
    data structures that may then, later, be inserted into a buffer. This
    function makes string into such a string, and callback
    will be called when the user clicks on the button. The optional
    data parameter will be used as the parameter when callback
    is called. If `nil`, the button is used as the parameter instead.

Function: **buttonize-region** start end callback &optional data help-echo [¶](#index-buttonize_002dregion)
:   Sometimes it’s more convenient to convert existing text in a buffer to a
    button instead of inserting new text. This function makes the region
    between start and end into a button. Arguments
    callback and data have the same meanings as for
    `buttonize`. Optional argument help-echo is used as the
    `help-echo` property of the button.

Function: **unbuttonize-region** start end [¶](#index-unbuttonize_002dregion)
:   This function removes all buttons between start and end in
    the current buffer (both overlay and text-property based ones).

---

Next: [Button Buffer Commands](#Button-Buffer-Commands), Previous: [Making Buttons](#Making-Buttons), Up: [Buttons](#Buttons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.20.4 Manipulating Buttons [¶](#Manipulating-Buttons-1)

These are functions for getting and setting properties of buttons.
Often these are used by a button’s invocation function to determine
what to do.

Where a button parameter is specified, it means an object
referring to a specific button, either an overlay (for overlay
buttons), or a buffer-position or marker (for text property buttons).
Such an object is passed as the first argument to a button’s
invocation function when it is invoked.

Function: **button-start** button [¶](#index-button_002dstart)
:   Return the position at which button starts.

Function: **button-end** button [¶](#index-button_002dend)
:   Return the position at which button ends.

Function: **button-get** button prop [¶](#index-button_002dget)
:   Get the property of button button named prop.

Function: **button-put** button prop val [¶](#index-button_002dput)
:   Set button’s prop property to val.

Function: **button-activate** button &optional use-mouse-action [¶](#index-button_002dactivate)
:   Call button’s `action` property (i.e., invoke the function
    that is the value of that property, passing it the single argument
    button). If use-mouse-action is non-`nil`, try to
    invoke the button’s `mouse-action` property instead of
    `action`; if the button has no `mouse-action` property, use
    `action` as normal. If the `button-data` property is
    present in button, use that as the argument for the
    `action` function instead of button.

Function: **button-label** button [¶](#index-button_002dlabel)
:   Return button’s text label.

Function: **button-type** button [¶](#index-button_002dtype)
:   Return button’s button-type.

Function: **button-has-type-p** button type [¶](#index-button_002dhas_002dtype_002dp)
:   Return `t` if button has button-type type, or one of
    type’s subtypes.

Function: **button-at** pos [¶](#index-button_002dat)
:   Return the button at position pos in the current buffer, or
    `nil`. If the button at pos is a text property button, the
    return value is a marker pointing to pos.

Function: **button-type-put** type prop val [¶](#index-button_002dtype_002dput)
:   Set the button-type type’s prop property to val.

Function: **button-type-get** type prop [¶](#index-button_002dtype_002dget)
:   Get the property of button-type type named prop.

Function: **button-type-subtype-p** type supertype [¶](#index-button_002dtype_002dsubtype_002dp)
:   Return `t` if button-type type is a subtype of supertype.

---

Previous: [Manipulating Buttons](#Manipulating-Buttons), Up: [Buttons](#Buttons)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.20.5 Button Buffer Commands [¶](#Button-Buffer-Commands-1)

These are commands and functions for locating and operating on
buttons in an Emacs buffer.

`push-button` is the command that a user uses to actually push
a button, and is bound by default in the button itself to `RET`
and to `mouse-2` using a local keymap in the button’s overlay or
text properties. Commands that are useful outside the buttons itself,
such as `forward-button` and `backward-button` are
additionally available in the keymap stored in
`button-buffer-map`; a mode which uses buttons may want to use
`button-buffer-map` as a parent keymap for its keymap.
Alternatively, the `button-mode` can be switched on for much the
same effect: It’s a minor mode that does nothing else than install
`button-buffer-map` as a minor mode keymap (note that disabling
`button-mode` will remove all the buttons in the current buffer).

If the button has a non-`nil` `follow-link` property, and
`mouse-1-click-follows-link` is set, a quick `mouse-1` click
will also activate the `push-button` command.
See [Defining Clickable Text](#Clickable-Text).

Command: **push-button** &optional pos use-mouse-action [¶](#index-push_002dbutton)
:   Perform the action specified by a button at location pos.
    pos may be either a buffer position or a mouse-event. If
    use-mouse-action is non-`nil`, or pos is a
    mouse-event (see [Mouse Events](#Mouse-Events)), try to invoke the button’s
    `mouse-action` property instead of `action`; if the button
    has no `mouse-action` property, use `action` as normal.
    pos defaults to point, except when `push-button` is invoked
    interactively as the result of a mouse-event, in which case, the mouse
    event’s position is used. If there’s no button at pos, do
    nothing and return `nil`, otherwise return `t`.

Command: **forward-button** n &optional wrap display-message no-error [¶](#index-forward_002dbutton)
:   Move to the nth next button, or nth previous button if
    n is negative. If n is zero, move to the start of any
    button at point. If wrap is non-`nil`, moving past either
    end of the buffer continues from the other end. If
    display-message is non-`nil`, the button’s help-echo string
    is displayed. Any button with a non-`nil` `skip` property
    is skipped over. Returns the button found, and signals an error if no
    buttons can be found. If no-error is non-`nil`, return `nil`
    instead of signaling the error.

Command: **backward-button** n &optional wrap display-message no-error [¶](#index-backward_002dbutton)
:   Move to the nth previous button, or nth next button if
    n is negative. If n is zero, move to the start of any
    button at point. If wrap is non-`nil`, moving past either
    end of the buffer continues from the other end. If
    display-message is non-`nil`, the button’s help-echo string
    is displayed. Any button with a non-`nil` `skip` property
    is skipped over. Returns the button found, and signals an error if no
    buttons can be found. If no-error is non-`nil`, return `nil`
    instead of signaling the error.

Function: **next-button** pos &optional count-current [¶](#index-next_002dbutton)

Function: **previous-button** pos &optional count-current [¶](#index-previous_002dbutton)
:   Return the next button after (for `next-button`) or before (for
    `previous-button`) position pos in the current buffer. If
    count-current is non-`nil`, count any button at pos
    in the search, instead of starting at the next button.

---

Next: [Blinking Parentheses](#Blinking), Previous: [Buttons](#Buttons), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.21 Abstract Display [¶](#Abstract-Display-1)

The Ewoc package constructs buffer text that represents a structure
of Lisp objects, and updates the text to follow changes in that
structure. This is like the “view” component in the
“model–view–controller” design paradigm. Ewoc means “Emacs’s
Widget for Object Collections”.

An *ewoc* is a structure that organizes information required to
construct buffer text that represents certain Lisp data. The buffer
text of the ewoc has three parts, in order: first, fixed *header*
text; next, textual descriptions of a series of data elements (Lisp
objects that you specify); and last, fixed *footer* text.
Specifically, an ewoc contains information on:

* The buffer which its text is generated in.
* The text’s start position in the buffer.
* The header and footer strings.
* A doubly-linked chain of *nodes*, each of which contains:
  + A *data element*, a single Lisp object.
  + Links to the preceding and following nodes in the chain.
* A *pretty-printer* function which is responsible for
  inserting the textual representation of a data
  element value into the current buffer.

Typically, you define an ewoc with `ewoc-create`, and then pass
the resulting ewoc structure to other functions in the Ewoc package to
build nodes within it, and display it in the buffer. Once it is
displayed in the buffer, other functions determine the correspondence
between buffer positions and nodes, move point from one node’s textual
representation to another, and so forth. See [Abstract Display Functions](#Abstract-Display-Functions).

A node *encapsulates* a data element much the way a variable
holds a value. Normally, encapsulation occurs as a part of adding a
node to the ewoc. You can retrieve the data element value and place a
new value in its place, like so:

```
(ewoc-data node)
⇒ value

(ewoc-set-data node new-value)
⇒ new-value
```

You can also use, as the data element value, a Lisp object (list or
vector) that is a container for the real value, or an index into
some other structure. The example (see [Abstract Display Example](#Abstract-Display-Example))
uses the latter approach.

When the data changes, you will want to update the text in the
buffer. You can update all nodes by calling `ewoc-refresh`, or
just specific nodes using `ewoc-invalidate`, or all nodes
satisfying a predicate using `ewoc-map`. Alternatively, you can
delete invalid nodes using `ewoc-delete` or `ewoc-filter`,
and add new nodes in their place. Deleting a node from an ewoc deletes
its associated textual description from buffer, as well.

* [Abstract Display Functions](#Abstract-Display-Functions)
* [Abstract Display Example](#Abstract-Display-Example)

---

Next: [Abstract Display Example](#Abstract-Display-Example), Up: [Abstract Display](#Abstract-Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.21.1 Abstract Display Functions [¶](#Abstract-Display-Functions-1)

In this subsection, ewoc and node stand for the
structures described above (see [Abstract Display](#Abstract-Display)), while
data stands for an arbitrary Lisp object used as a data element.

Function: **ewoc-create** pretty-printer &optional header footer nosep [¶](#index-ewoc_002dcreate)
:   This constructs and returns a new ewoc, with no nodes (and thus no data
    elements). pretty-printer should be a function that takes one
    argument, a data element of the sort you plan to use in this ewoc, and
    inserts its textual description at point using `insert` (and never
    `insert-before-markers`, because that would interfere with the
    Ewoc package’s internal mechanisms).

    Normally, a newline is automatically inserted after the header,
    the footer and every node’s textual description. If nosep
    is non-`nil`, no newline is inserted. This may be useful for
    displaying an entire ewoc on a single line, for example, or for
    making nodes invisible by arranging for pretty-printer
    to do nothing for those nodes.

    An ewoc maintains its text in the buffer that is current when
    you create it, so switch to the intended buffer before calling
    `ewoc-create`.

Function: **ewoc-buffer** ewoc [¶](#index-ewoc_002dbuffer)
:   This returns the buffer where ewoc maintains its text.

Function: **ewoc-get-hf** ewoc [¶](#index-ewoc_002dget_002dhf)
:   This returns a cons cell `(header . footer)`
    made from ewoc’s header and footer.

Function: **ewoc-set-hf** ewoc header footer [¶](#index-ewoc_002dset_002dhf)
:   This sets the header and footer of ewoc to the strings
    header and footer, respectively.

Function: **ewoc-enter-first** ewoc data [¶](#index-ewoc_002denter_002dfirst)

Function: **ewoc-enter-last** ewoc data [¶](#index-ewoc_002denter_002dlast)
:   These add a new node encapsulating data, putting it, respectively,
    at the beginning or end of ewoc’s chain of nodes.

Function: **ewoc-enter-before** ewoc node data [¶](#index-ewoc_002denter_002dbefore)

Function: **ewoc-enter-after** ewoc node data [¶](#index-ewoc_002denter_002dafter)
:   These add a new node encapsulating data, adding it to
    ewoc before or after node, respectively.

Function: **ewoc-prev** ewoc node [¶](#index-ewoc_002dprev)

Function: **ewoc-next** ewoc node [¶](#index-ewoc_002dnext)
:   These return, respectively, the previous node and the next node of node
    in ewoc.

Function: **ewoc-nth** ewoc n [¶](#index-ewoc_002dnth)
:   This returns the node in ewoc found at zero-based index n.
    A negative n means count from the end. `ewoc-nth` returns
    `nil` if n is out of range.

Function: **ewoc-data** node [¶](#index-ewoc_002ddata)
:   This extracts the data encapsulated by node and returns it.

Function: **ewoc-set-data** node data [¶](#index-ewoc_002dset_002ddata)
:   This sets the data encapsulated by node to data.

Function: **ewoc-locate** ewoc &optional pos guess [¶](#index-ewoc_002dlocate)
:   This determines the node in ewoc which contains point (or
    pos if specified), and returns that node. If ewoc has no
    nodes, it returns `nil`. If pos is before the first node,
    it returns the first node; if pos is after the last node, it returns
    the last node. The optional third arg guess
    should be a node that is likely to be near pos; this doesn’t
    alter the result, but makes the function run faster.

Function: **ewoc-location** node [¶](#index-ewoc_002dlocation)
:   This returns the start position of node.

Function: **ewoc-goto-prev** ewoc arg [¶](#index-ewoc_002dgoto_002dprev)

Function: **ewoc-goto-next** ewoc arg [¶](#index-ewoc_002dgoto_002dnext)
:   These move point to the previous or next, respectively, argth node
    in ewoc. `ewoc-goto-prev` does not move if it is already at
    the first node or if ewoc is empty, whereas `ewoc-goto-next`
    moves past the last node, returning `nil`. Excepting this special
    case, these functions return the node moved to.

Function: **ewoc-goto-node** ewoc node [¶](#index-ewoc_002dgoto_002dnode)
:   This moves point to the start of node in ewoc.

Function: **ewoc-refresh** ewoc [¶](#index-ewoc_002drefresh)
:   This function regenerates the text of ewoc. It works by
    deleting the text between the header and the footer, i.e., all the
    data elements’ representations, and then calling the pretty-printer
    function for each node, one by one, in order.

Function: **ewoc-invalidate** ewoc &rest nodes [¶](#index-ewoc_002dinvalidate)
:   This is similar to `ewoc-refresh`, except that only nodes in
    ewoc are updated instead of the entire set.

Function: **ewoc-delete** ewoc &rest nodes [¶](#index-ewoc_002ddelete)
:   This deletes each node in nodes from ewoc.

Function: **ewoc-filter** ewoc predicate &rest args [¶](#index-ewoc_002dfilter)
:   This calls predicate for each data element in ewoc and
    deletes those nodes for which predicate returns `nil`.
    Any args are passed to predicate.

Function: **ewoc-collect** ewoc predicate &rest args [¶](#index-ewoc_002dcollect)
:   This calls predicate for each data element in ewoc
    and returns a list of those elements for which predicate
    returns non-`nil`. The elements in the list are ordered
    as in the buffer. Any args are passed to predicate.

Function: **ewoc-map** map-function ewoc &rest args [¶](#index-ewoc_002dmap)
:   This calls map-function for each data element in ewoc and
    updates those nodes for which map-function returns non-`nil`.
    Any args are passed to map-function.

---

Previous: [Abstract Display Functions](#Abstract-Display-Functions), Up: [Abstract Display](#Abstract-Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.21.2 Abstract Display Example [¶](#Abstract-Display-Example-1)

Here is a simple example using functions of the ewoc package to
implement a *color components* display, an area in a buffer that
represents a vector of three integers (itself representing a 24-bit RGB
value) in various ways.

```
(setq colorcomp-ewoc nil
      colorcomp-data nil
      colorcomp-mode-map nil
      colorcomp-labels ["Red" "Green" "Blue"])

(defun colorcomp-pp (data)
  (if data
      (let ((comp (aref colorcomp-data data)))
        (insert (aref colorcomp-labels data) "\t: #x"
                (format "%02X" comp) " "
                (make-string (ash comp -2) ?#) "\n"))
    (let ((cstr (format "#%02X%02X%02X"
                        (aref colorcomp-data 0)
                        (aref colorcomp-data 1)
                        (aref colorcomp-data 2)))
          (samp " (sample text) "))
      (insert "Color\t: "
              (propertize samp 'face
                          `(foreground-color . ,cstr))
              (propertize samp 'face
                          `(background-color . ,cstr))
              "\n"))))

(defun colorcomp (color)
  "Allow fiddling with COLOR in a new buffer.
The buffer is in Color Components mode."
  (interactive "sColor (name or #RGB or #RRGGBB): ")
  (when (string= "" color)
    (setq color "green"))
  (unless (color-values color)
    (error "No such color: %S" color))
  (switch-to-buffer
   (generate-new-buffer (format "originally: %s" color)))
  (kill-all-local-variables)
  (setq major-mode 'colorcomp-mode
        mode-name "Color Components")
  (use-local-map colorcomp-mode-map)
  (erase-buffer)
  (buffer-disable-undo)
  (let ((data (apply 'vector (mapcar (lambda (n) (ash n -8))
                                     (color-values color))))
        (ewoc (ewoc-create 'colorcomp-pp
                           "\nColor Components\n\n"
                           (substitute-command-keys
                            "\n\\{colorcomp-mode-map}"))))
    (set (make-local-variable 'colorcomp-data) data)
    (set (make-local-variable 'colorcomp-ewoc) ewoc)
    (ewoc-enter-last ewoc 0)
    (ewoc-enter-last ewoc 1)
    (ewoc-enter-last ewoc 2)
    (ewoc-enter-last ewoc nil)))
```

This example can be extended to be a color selection widget (in
other words, the “controller” part of the “model–view–controller”
design paradigm) by defining commands to modify `colorcomp-data`
and to finish the selection process, and a keymap to tie it all
together conveniently.

```
(defun colorcomp-mod (index limit delta)
  (let ((cur (aref colorcomp-data index)))
    (unless (= limit cur)
      (aset colorcomp-data index (+ cur delta)))
    (ewoc-invalidate
     colorcomp-ewoc
     (ewoc-nth colorcomp-ewoc index)
     (ewoc-nth colorcomp-ewoc -1))))

(defun colorcomp-R-more () (interactive) (colorcomp-mod 0 255 1))
(defun colorcomp-G-more () (interactive) (colorcomp-mod 1 255 1))
(defun colorcomp-B-more () (interactive) (colorcomp-mod 2 255 1))
(defun colorcomp-R-less () (interactive) (colorcomp-mod 0 0 -1))
(defun colorcomp-G-less () (interactive) (colorcomp-mod 1 0 -1))
(defun colorcomp-B-less () (interactive) (colorcomp-mod 2 0 -1))

(defun colorcomp-copy-as-kill-and-exit ()
  "Copy the color components into the kill ring and kill the buffer.
The string is formatted #RRGGBB (hash followed by six hex digits)."
  (interactive)
  (kill-new (format "#%02X%02X%02X"
                    (aref colorcomp-data 0)
                    (aref colorcomp-data 1)
                    (aref colorcomp-data 2)))
  (kill-buffer nil))

(setq colorcomp-mode-map
      (define-keymap :suppress t
        "i" 'colorcomp-R-less
        "o" 'colorcomp-R-more
        "k" 'colorcomp-G-less
        "l" 'colorcomp-G-more
        "," 'colorcomp-B-less
        "." 'colorcomp-B-more
        "SPC" 'colorcomp-copy-as-kill-and-exit))
```

Note that we never modify the data in each node, which is fixed when the
ewoc is created to be either `nil` or an index into the vector
`colorcomp-data`, the actual color components.

---

Next: [Character Display](#Character-Display), Previous: [Abstract Display](#Abstract-Display), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.22 Blinking Parentheses [¶](#Blinking-Parentheses)

This section describes the mechanism by which Emacs shows a matching
open parenthesis when the user inserts a close parenthesis.

Variable: **blink-paren-function** [¶](#index-blink_002dparen_002dfunction)
:   The value of this variable should be a function (of no arguments) to
    be called whenever a character with close parenthesis syntax is inserted.
    The value of `blink-paren-function` may be `nil`, in which
    case nothing is done.

User Option: **blink-matching-paren** [¶](#index-blink_002dmatching_002dparen)
:   If this variable is `nil`, then `blink-matching-open` does
    nothing.

User Option: **blink-matching-paren-distance** [¶](#index-blink_002dmatching_002dparen_002ddistance)
:   This variable specifies the maximum distance to scan for a matching
    parenthesis before giving up.

User Option: **blink-matching-delay** [¶](#index-blink_002dmatching_002ddelay)
:   This variable specifies the number of seconds to keep indicating the
    matching parenthesis. A fraction of a second often gives good
    results, but the default is 1, which works on all systems.

Command: **blink-matching-open** [¶](#index-blink_002dmatching_002dopen)
:   This function is the default value of `blink-paren-function`. It
    assumes that point follows a character with close parenthesis syntax
    and applies the appropriate effect momentarily to the matching opening
    character. If that character is not already on the screen, it
    displays the character’s context in the echo area. To avoid long
    delays, this function does not search farther than
    `blink-matching-paren-distance` characters.

    Here is an example of calling this function explicitly.

    ```
    (defun interactive-blink-matching-open ()
      "Indicate momentarily the start of parenthesized sexp before point."
      (interactive)
    ```

    ```
      (let ((blink-matching-paren-distance
             (buffer-size))
            (blink-matching-paren t))
        (blink-matching-open)))
    ```

---

Next: [Beeping](#Beeping), Previous: [Blinking Parentheses](#Blinking), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.23 Character Display [¶](#Character-Display-1)

This section describes how characters are actually displayed by
Emacs. Typically, a character is displayed as a *glyph* (a
graphical symbol which occupies one character position on the screen),
whose appearance corresponds to the character itself. For example,
the character ‘`a`’ (character code 97) is displayed as ‘`a`’.
Some characters, however, are displayed specially. For example, the
formfeed character (character code 12) is usually displayed as a
sequence of two glyphs, ‘`^L`’, while the newline character
(character code 10) starts a new screen line.

You can modify how each character is displayed by defining a
*display table*, which maps each character code into a sequence of
glyphs. See [Display Tables](#Display-Tables).

* [Usual Display Conventions](#Usual-Display)
* [Display Tables](#Display-Tables)
* [Active Display Table](#Active-Display-Table)
* [Glyphs](#Glyphs)
* [Glyphless Character Display](#Glyphless-Chars)

---

Next: [Display Tables](#Display-Tables), Up: [Character Display](#Character-Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.23.1 Usual Display Conventions [¶](#Usual-Display-Conventions)

Here are the conventions for displaying each character code (in the
absence of a display table, which can override these
conventions; see [Display Tables](#Display-Tables)).

* The *printable ASCII characters*, character codes 32
  through 126 (consisting of numerals, English letters, and symbols like
  ‘`#`’) are displayed literally.
* The tab character (character code 9) displays as whitespace stretching
  up to the next tab stop column. See [Text Display](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Text-Display) in The GNU
  Emacs Manual. The variable `tab-width` controls the number of
  spaces per tab stop (see below).
* The newline character (character code 10) has a special effect: it
  ends the preceding line and starts a new line.
* The non-printable *ASCII control characters*—character
  codes 0 through 31, as well as the `DEL` character (character code
  127)—display in one of two ways according to the variable
  `ctl-arrow`. If this variable is non-`nil` (the default),
  these characters are displayed as sequences of two glyphs, where the
  first glyph is ‘`^`’ (a display table can specify a glyph to use
  instead of ‘`^`’); e.g., the `DEL` character is displayed as
  ‘`^?`’.

  If `ctl-arrow` is `nil`, these characters are displayed as
  octal escapes (see below).

  This rule also applies to carriage return (character code 13), if that
  character appears in the buffer. But carriage returns usually do not
  appear in buffer text; they are eliminated as part of end-of-line
  conversion (see [Basic Concepts of Coding Systems](#Coding-System-Basics)).
* *Raw bytes* are non-ASCII characters with codes 128
  through 255 (see [Text Representations](#Text-Representations)). These characters display
  as *octal escapes*: sequences of four glyphs, where the first
  glyph is the ASCII code for ‘`\`’, and the others are
  digit characters representing the character code in octal. (A display
  table can specify a glyph to use instead of ‘`\`’.)
* Each non-ASCII character with code above 255 is displayed
  literally, if the terminal supports it. If the terminal does not
  support it, the character is said to be *glyphless*, and it is
  usually displayed using a placeholder glyph. For example, if a
  graphical terminal has no font for a character, Emacs usually displays
  a box containing the character code in hexadecimal. See [Glyphless Character Display](#Glyphless-Chars).

The above display conventions apply even when there is a display
table, for any character whose entry in the active display table is
`nil`. Thus, when you set up a display table, you need only
specify the characters for which you want special display behavior.

The following variables affect how certain characters are displayed
on the screen. Since they change the number of columns the characters
occupy, they also affect the indentation functions. They also affect
how the mode line is displayed; if you want to force redisplay of the
mode line using the new values, call the function
`force-mode-line-update` (see [Mode Line Format](#Mode-Line-Format)).

User Option: **ctl-arrow** [¶](#index-ctl_002darrow)
:   This buffer-local variable controls how control characters are
    displayed. If it is non-`nil`, they are displayed as a caret
    followed by the character: ‘`^A`’. If it is `nil`, they are
    displayed as octal escapes: a backslash followed by three octal
    digits, as in ‘`\001`’.

User Option: **tab-width** [¶](#index-tab_002dwidth)
:   The value of this buffer-local variable is the spacing between tab
    stops used for displaying tab characters in Emacs buffers. The value
    is in units of columns, and the default is 8. Note that this feature
    is completely independent of the user-settable tab stops used by the
    command `tab-to-tab-stop`. See [Adjustable Tab Stops](#Indent-Tabs).

---

Next: [Active Display Table](#Active-Display-Table), Previous: [Usual Display Conventions](#Usual-Display), Up: [Character Display](#Character-Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.23.2 Display Tables [¶](#Display-Tables-1)

A display table is a special-purpose char-table
(see [Char-Tables](#Char_002dTables)), with `display-table` as its subtype, which
is used to override the usual character display conventions. This
section describes how to make, inspect, and assign elements to a
display table object. The next section (see [Active Display Table](#Active-Display-Table))
describes the various standard display tables and their precedence.

Function: **make-display-table** [¶](#index-make_002ddisplay_002dtable)
:   This creates and returns a display table. The table initially has
    `nil` in all elements.

The ordinary elements of the display table are indexed by character
codes; the element at index c says how to display the character
code c. The value should be `nil` (which means to display
the character c according to the usual display conventions;
see [Usual Display Conventions](#Usual-Display)), or a vector of glyph codes (which means to
display the character c as those glyphs; see [Glyphs](#Glyphs)).

**Warning:** if you use the display table to change the display
of newline characters, the whole buffer will be displayed as one long
line.

The display table also has six *extra slots* which serve special
purposes. Here is a table of their meanings; `nil` in any slot
means to use the default for that slot, as stated below.

0
:   The glyph for the end of a truncated screen line (the default for this
    is ‘`$`’). See [Glyphs](#Glyphs). On graphical terminals, Emacs by
    default uses arrows in the fringes to indicate truncation, so the
    display table has no effect, unless you disable the fringes
    (see [Window Fringes](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fringes) in the GNU Emacs Manual).

1
:   The glyph for the end of a continued line (the default is ‘`\`’).
    On graphical terminals, Emacs by default uses curved arrows in the
    fringes to indicate continuation, so the display table has no effect,
    unless you disable the fringes.

2
:   The glyph for indicating a character displayed as an octal character
    code (the default is ‘`\`’).

3
:   The glyph for indicating a control character (the default is ‘`^`’).

4
:   A vector of glyphs for indicating the presence of invisible lines (the
    default is ‘`...`’). See [Selective Display](#Selective-Display).

5
:   The glyph used to draw the border between side-by-side windows (the
    default is ‘`|`’). See [Splitting Windows](#Splitting-Windows). This currently has
    effect only on text terminals; on graphical terminals, if vertical
    scroll bars are supported and in use, a scroll bar separates the two
    windows, and if there are no vertical scroll bars and no dividers
    (see [Window Dividers](#Window-Dividers)), Emacs uses a thin line to indicate the
    border.

6 to 11
:   The glyphs for a single-line border around child frames on a terminal,
    in the order of vertical, horizontal, down-right edge, down-left edge,
    up-right, and up-left edge glyphs. The horizontal glyph is also used for
    the single-line tty menu separator.

12 to 17
:   The glyphs for a double-line border, in the order of vertical,
    horizontal, down-right edge, down-left edge, up-right, and up-left edge
    glyphs. The horizontal glyph is also used for the single-line TTY menu
    separator, the other glyphs are not yet used.

For example, here is how to construct a display table that mimics
the effect of setting `ctl-arrow` to a non-`nil` value
(see [Glyphs](#Glyphs), for the function `make-glyph-code`):

```
(setq disptab (make-display-table))
(dotimes (i 32)
  (or (= i ?\t)
      (= i ?\n)
      (aset disptab i
            (vector (make-glyph-code ?^ 'escape-glyph)
                    (make-glyph-code (+ i 64) 'escape-glyph)))))
(aset disptab 127
      (vector (make-glyph-code ?^ 'escape-glyph)
              (make-glyph-code ?? 'escape-glyph)))
```

Function: **display-table-slot** display-table slot [¶](#index-display_002dtable_002dslot)
:   This function returns the value of the extra slot slot of
    display-table. The argument slot may be a number from 0 to
    17 inclusive, or a slot name, a symbol.

Function: **set-display-table-slot** display-table slot value [¶](#index-set_002ddisplay_002dtable_002dslot)
:   This function stores value in the extra slot slot of
    display-table. The argument slot may be a number from 0 to
    17 inclusive, or a slot name, a symbol.

Valid slot name symbols are `truncation`, `wrap`,
`escape`, `control`, `selective-display`,
`vertical-border`, `box-vertical`, `box-horizontal`,
`box-down-right`, `box-down-left`, `box-up-right`,
`box-up-left`, `box-double-vertical`,
`box-double-horizontal`, `box-double-down-right`,
`box-double-down-left`, `box-double-up-right`,
`box-double-up-left`.

Function: **describe-display-table** display-table [¶](#index-describe_002ddisplay_002dtable)
:   This function displays a description of the display table
    display-table in a help buffer.

Function: **standard-display-unicode-special-glyphs** [¶](#index-standard_002ddisplay_002dunicode_002dspecial_002dglyphs)
:   This function sets the extra slots of standard-display-table with
    suitable Unicode characters.

Command: **describe-current-display-table** [¶](#index-describe_002dcurrent_002ddisplay_002dtable)
:   This command displays a description of the current display table in a
    help buffer.

---

Next: [Glyphs](#Glyphs), Previous: [Display Tables](#Display-Tables), Up: [Character Display](#Character-Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.23.3 Active Display Table [¶](#Active-Display-Table-1)

Each window can specify a display table, and so can each buffer.
The window’s display table, if there is one, takes precedence over the
buffer’s display table. If neither exists, Emacs tries to use the
standard display table; if that is `nil`, Emacs uses the usual
character display conventions (see [Usual Display Conventions](#Usual-Display)). (Emacs does
not “merge” display tables: For instance, if the window has a
display table, the buffer’s display table and the standard display
table are completely ignored.)

Note that display tables affect how the mode line is displayed, so
if you want to force redisplay of the mode line using a new display
table, call `force-mode-line-update` (see [Mode Line Format](#Mode-Line-Format)).

Function: **window-display-table** &optional window [¶](#index-window_002ddisplay_002dtable)
:   This function returns window’s display table, or `nil` if
    there is none. The default for window is the selected window.

Function: **set-window-display-table** window table [¶](#index-set_002dwindow_002ddisplay_002dtable)
:   This function sets the display table of window to table.
    The argument table should be either a display table or
    `nil`.

Variable: **buffer-display-table** [¶](#index-buffer_002ddisplay_002dtable)
:   This variable is automatically buffer-local in all buffers; its value
    specifies the buffer’s display table. If it is `nil`, there is
    no buffer display table.

Variable: **standard-display-table** [¶](#index-standard_002ddisplay_002dtable)
:   The value of this variable is the standard display table, which is
    used when Emacs is displaying a buffer in a window with neither a
    window display table nor a buffer display table defined, or when Emacs
    is outputting text to the standard output or error streams. Although its
    default is typically `nil`, in an interactive session if the
    terminal cannot display curved quotes, its default maps curved quotes
    to ASCII approximations. See [Text Quoting Style](#Text-Quoting-Style).

The `disp-table` library defines several functions for changing
the standard display table.

---

Next: [Glyphless Character Display](#Glyphless-Chars), Previous: [Active Display Table](#Active-Display-Table), Up: [Character Display](#Character-Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.23.4 Glyphs [¶](#Glyphs-1)

A *glyph* is a graphical symbol which occupies a single
character position on the screen. Each glyph is represented in Lisp
as a *glyph code*, which specifies a character and optionally a
face to display it in (see [Faces](#Faces)). The main use of glyph codes is
as the entries of display tables (see [Display Tables](#Display-Tables)). The
following functions are used to manipulate glyph codes:

Function: **make-glyph-code** char &optional face [¶](#index-make_002dglyph_002dcode)
:   This function returns a glyph code representing char char with
    face face. If face is omitted or `nil`, the glyph
    uses the default face; in that case, the glyph code is an integer. If
    face is non-`nil`, the glyph code is not necessarily an
    integer object.

Function: **glyph-char** glyph [¶](#index-glyph_002dchar)
:   This function returns the character of glyph code glyph.

Function: **glyph-face** glyph [¶](#index-glyph_002dface)
:   This function returns face of glyph code glyph, or `nil` if
    glyph uses the default face.

You can set up a *glyph table* to change how glyph codes are
actually displayed on text terminals. This feature is semi-obsolete;
use `glyphless-char-display` instead (see [Glyphless Character Display](#Glyphless-Chars)).

Variable: **glyph-table** [¶](#index-glyph_002dtable)
:   The value of this variable, if non-`nil`, is the current glyph
    table. It takes effect only on character terminals; on graphical
    displays, all glyphs are displayed literally. The glyph table should
    be a vector whose gth element specifies how to display glyph
    code g, where g is the glyph code for a glyph whose face
    is unspecified. Each element should be one of the following:

    `nil`
    :   Display this glyph literally.

    a string
    :   Display this glyph by sending the specified string to the terminal.

    a glyph code
    :   Display the specified glyph code instead.

    Any integer glyph code greater than or equal to the length of the
    glyph table is displayed literally.

---

Previous: [Glyphs](#Glyphs), Up: [Character Display](#Character-Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.23.5 Glyphless Character Display [¶](#Glyphless-Character-Display)

*Glyphless characters* are characters which are displayed in a
special way, e.g., as a box containing a hexadecimal code, instead of
being displayed literally. These include characters which are
explicitly defined to be glyphless, as well as characters for which
there is no available font (on a graphical display), and characters
which cannot be encoded by the terminal’s coding system (on a text
terminal).

The `glyphless-display-mode` minor mode can be used to toggle
displaying glyphless characters in a convenient manner in the current
buffer. If this mode is enabled, all the glyphless characters are
displayed as boxes that display acronyms of their character names.

Variable: **glyphless-char-display** [¶](#index-glyphless_002dchar_002ddisplay)
:   For more fine-grained (and global) control, this variable can be used.
    The value of this variable is a char-table which defines glyphless
    characters and how they are displayed. Each entry must be one of the
    following display methods:

    `nil`
    :   Display the character in the usual way.

    `zero-width`
    :   Don’t display the character.

    `thin-space`
    :   Display a thin space, 1-pixel wide on graphical displays, or
        1-character wide on text terminals.

    `empty-box`
    :   Display an empty box.

    `hex-code`
    :   Display a box containing the Unicode codepoint of the character, in
        hexadecimal notation.

    an ASCII string
    :   Display a box containing that string. The string should contain at
        most 6 ASCII characters. As an exception, if the string
        includes just one character, on text-mode terminals that character
        will be displayed without a box; this enables treating such
        “acronyms” as replacement characters for characters that cannot be
        displayed by the terminal.

    a cons cell `(graphical . text)`
    :   Display with graphical on graphical displays, and with
        text on text terminals. Both graphical and text
        must be one of the display methods described above.

    The `thin-space`, `empty-box`, `hex-code`, and
    ASCII string display methods are drawn with the
    `glyphless-char` face. On text terminals, a box is emulated by
    square brackets, ‘`[]`’.

    The char-table has one extra slot, which determines how to display any
    character that cannot be displayed with any available font, or cannot
    be encoded by the terminal’s coding system. Its value should be one
    of the above display methods, except `zero-width`.

    If a character has a non-`nil` entry in an active display table,
    the display table takes effect; in this case, Emacs does not consult
    `glyphless-char-display` at all.

User Option: **glyphless-char-display-control** [¶](#index-glyphless_002dchar_002ddisplay_002dcontrol)
:   This user option provides a convenient way to set
    `glyphless-char-display` for groups of similar characters. Do
    not set its value directly from Lisp code; the value takes effect only
    via a custom `:set` function (see [Defining Customization Variables](#Variable-Definitions)),
    which updates `glyphless-char-display`.

    Its value should be an alist of elements `(group
    . method)`, where group is a symbol specifying a group of
    characters, and method is a symbol specifying how to display
    them.

    group should be one of the following:

    `c0-control`
    :   ASCII control characters `U+0000` to `U+001F`,
        excluding the newline and tab characters (normally displayed as escape
        sequences like ‘`^A`’; see [How Text Is Displayed](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Text-Display) in The GNU Emacs Manual).

    `c1-control`
    :   Non-ASCII, non-printing characters `U+0080` to
        `U+009F` (normally displayed as octal escape sequences like
        ‘`\230`’).

    `format-control`
    :   Characters of Unicode General Category [Cf], such as U+200E
        LEFT-TO-RIGHT MARK, but excluding characters that have graphic
        images, such as U+00AD SOFT HYPHEN.

    `bidi-control`
    :   This is a subset of `format-control`, but only includes
        characters that are related to bidirectional formatting control, like
        U+2069 POP DIRECTIONAL ISOLATE and U+202A LEFT-TO-RIGHT
        EMBEDDING. See [Bidirectional Display](#Bidirectional-Display).

        Characters of Unicode General Category [Cf], such as U+200E
        LEFT-TO-RIGHT MARK, but excluding characters that have graphic
        images, such as U+00AD SOFT HYPHEN.

    `variation-selectors`
    :   Unicode VS-1 through VS-256 (U+FE00 through U+FE0F and U+E0100 through
        U+E01EF), which are used to select between different glyphs for the same
        codepoints (typically emojis).

    `no-font`
    :   Characters for which there is no suitable font, or which cannot be
        encoded by the terminal’s coding system, or those for which the
        text-mode terminal has no glyphs.

    The method symbol should be one of `zero-width`,
    `thin-space`, `empty-box`, or `hex-code`. These have
    the same meanings as in `glyphless-char-display`, above.

---

Next: [Window Systems](#Window-Systems), Previous: [Character Display](#Character-Display), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.24 Beeping [¶](#Beeping-1)

This section describes how to make Emacs ring the bell (or blink the
screen) to attract the user’s attention. Be conservative about how
often you do this; frequent bells can become irritating. Also be
careful not to use just beeping when signaling an error is more
appropriate (see [Errors](#Errors)).

Function: **ding** &optional do-not-terminate [¶](#index-ding)
:   This function beeps, or flashes the screen (see `visible-bell` below).
    It also terminates any keyboard macro currently executing unless
    do-not-terminate is non-`nil`.

Function: **beep** &optional do-not-terminate [¶](#index-beep)
:   This is a synonym for `ding`.

User Option: **visible-bell** [¶](#index-visible_002dbell)
:   This variable determines whether Emacs should flash the screen to
    represent a bell. Non-`nil` means yes, `nil` means no.
    This is effective on graphical displays, and on text terminals
    provided the terminal’s Termcap entry defines the visible bell
    capability (‘`vb`’).

User Option: **ring-bell-function** [¶](#index-ring_002dbell_002dfunction)
:   If this is non-`nil`, it specifies how Emacs should ring the
    bell. Its value should be a function of no arguments. If this is
    non-`nil`, it takes precedence over the `visible-bell`
    variable.

---

Next: [Tooltips](#Tooltips), Previous: [Beeping](#Beeping), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.25 Window Systems [¶](#Window-Systems-1)

Emacs works with several window systems, most notably the X Window
System. Both Emacs and X use the term “window”, but use it
differently. An Emacs frame is a single window as far as X is
concerned; the individual Emacs windows are not known to X at all.

Variable: **window-system** [¶](#index-window_002dsystem-1)
:   This terminal-local variable tells Lisp programs what window system
    Emacs is using for displaying the frame. The possible values are

    `x` [¶](#index-X-Window-System)
    :   Emacs is displaying the frame using X.

    `w32`
    :   Emacs is displaying the frame using native MS-Windows GUI.

    `ns`
    :   Emacs is displaying the frame using the Nextstep interface (used on
        GNUstep and macOS).

    `pc`
    :   Emacs is displaying the frame using MS-DOS direct screen writes.

    `haiku`
    :   Emacs is displaying the frame using the Application Kit on Haiku.

    `pgtk`
    :   Emacs is displaying the frame using pure GTK facilities.

    `android`
    :   Emacs is displaying the frame on Android.

    `nil`
    :   Emacs is displaying the frame on a character-based terminal.

Variable: **initial-window-system** [¶](#index-initial_002dwindow_002dsystem)
:   This variable holds the value of `window-system` used for the
    first frame created by Emacs during startup. (When Emacs is invoked
    as a daemon, it does not create any initial
    frames, so `initial-window-system` is `nil`, except on
    MS-Windows, where it is still `w32`. See [daemon](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Initial-Options) in The GNU Emacs Manual.)

Function: **window-system** &optional frame [¶](#index-window_002dsystem)
:   This function returns a symbol whose name tells what window system is
    used for displaying frame (which defaults to the currently
    selected frame). The list of possible symbols it returns is the same
    one documented for the variable `window-system` above.

Do *not* use `window-system` and
`initial-window-system` as predicates or boolean flag variables,
if you want to write code that works differently on text terminals and
graphic displays. That is because `window-system` is not a good
indicator of Emacs capabilities on a given display type. Instead, use
`display-graphic-p` or any of the other `display-*-p`
predicates described in [Display Feature Testing](#Display-Feature-Testing).

---

Next: [Bidirectional Display](#Bidirectional-Display), Previous: [Window Systems](#Window-Systems), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.26 Tooltips [¶](#Tooltips-1)

*Tooltips* are special frames (see [Frames](#Frames)) that are used to
display helpful hints (a.k.a. “tips”) related to the current
position of the mouse pointer. Emacs uses tooltips to display help
strings about active portions of text (see [Properties with Special Meanings](#Special-Properties)) and
about various UI elements, such as menu items (see [Extended Menu Items](#Extended-Menu-Items)) and tool-bar buttons (see [Tool bars](#Tool-Bar)).

Function: **tooltip-mode** [¶](#index-tooltip_002dmode)
:   Tooltip Mode is a minor mode that enables display of tooltips.
    Turning off this mode causes the tooltips be displayed in the echo
    area. On text-mode (a.k.a. “TTY”) frames, tooltips are always
    displayed in the echo area.

When Emacs is built with the GTK+ toolkit or Haiku windowing support,
it by default displays tooltips using toolkit functions, and the
appearance of the tooltips is then controlled by the toolkit’s
settings. Toolkit-provided tooltips can be disabled by changing the
value of the variable `use-system-tooltips` to `nil`. The
rest of this subsection describes how to control non-toolkit tooltips,
which are presented by Emacs itself.

Tooltips are displayed in special frames called tooltip frames, which
have their own frame parameters (see [Frame Parameters](#Frame-Parameters)). Unlike
other frames, the default parameters for tooltip frames are stored in a
special variable.

User Option: **tooltip-frame-parameters** [¶](#index-tooltip_002dframe_002dparameters)
:   This customizable option holds the default frame parameters used for
    displaying tooltips. Any font and color parameters are ignored, and the
    corresponding attributes of the `tooltip` face are used instead.
    If `left` or `top` parameters are included, they are used as
    absolute frame-relative coordinates where the tooltip should be shown.
    (Mouse-relative position of the tooltip can be customized using the
    variables described in [Tooltips](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tooltips) in The GNU Emacs Manual.)
    Note that the `left` and `top` parameters, if present,
    override the values of mouse-relative offsets.

The `tooltip` face determines the appearance of text shown in
tooltips. It should generally use a variable-pitch font of size that
is preferably smaller than the default frame font.

Variable: **tooltip-functions** [¶](#index-tooltip_002dfunctions)
:   This abnormal hook is a list of functions to call when Emacs needs to
    display a tooltip. Each function is called with a single argument
    event which is a copy of the last mouse movement event. If a
    function on this list actually displays the tooltip, it should return
    non-`nil`, and then the rest of the functions will not be
    called. The default value of this variable is a single function
    `tooltip-help-tips`.

If you write your own function to be put on the
`tooltip-functions` list, you may need to know the buffer of the
mouse event that triggered the tooltip display. The following
function provides that information.

Function: **tooltip-event-buffer** event [¶](#index-tooltip_002devent_002dbuffer)
:   This function returns the buffer over which event occurred.
    Call it with the argument of the function from
    `tooltip-functions` to obtain the buffer whose text triggered the
    tooltip. Note that the event might occur not over a buffer (e.g.,
    over the tool bar), in which case this function will return
    `nil`.

Other aspects of tooltip display are controlled by several
customizable settings; see [Tooltips](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Tooltips) in The GNU Emacs
Manual.

---

Previous: [Tooltips](#Tooltips), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.27 Bidirectional Display [¶](#Bidirectional-Display-1)

Emacs can display text written in scripts, such as Arabic, Farsi,
and Hebrew, whose natural ordering for horizontal text display runs
from right to left. Furthermore, segments of Latin script and digits
embedded in right-to-left text are displayed left-to-right, while
segments of right-to-left script embedded in left-to-right text
(e.g., Arabic or Hebrew text in comments or strings in a program
source file) are appropriately displayed right-to-left. We call such
mixtures of left-to-right and right-to-left text *bidirectional
text*. This section describes the facilities and options for editing
and displaying bidirectional text.

Text is stored in Emacs buffers and strings in *logical* (or
*reading*) order, i.e., the order in which a human would read
each character. In right-to-left and bidirectional text, the order in
which characters are displayed on the screen (called *visual
order*) is not the same as logical order; the characters’ screen
positions do not increase monotonically with string or buffer
position. In performing this *bidirectional reordering*, Emacs
follows the Unicode Bidirectional Algorithm (a.k.a. UBA),
which is described in Annex #9 of the Unicode standard
(<https://www.unicode.org/reports/tr9/>). Emacs provides a “Full
Bidirectionality” class implementation of the UBA,
consistent with the requirements of the Unicode Standard v9.0. Note,
however, that the way Emacs displays continuation lines when text
direction is opposite to the base paragraph direction deviates from
the UBA, which requires performing line wrapping before reordering
text for display.

Variable: **bidi-display-reordering** [¶](#index-bidi_002ddisplay_002dreordering)
:   If the value of this buffer-local variable is non-`nil` (the
    default), Emacs performs bidirectional reordering for display. The
    reordering affects buffer text, as well as display strings and overlay
    strings from text and overlay properties in the buffer (see [Overlay Properties](#Overlay-Properties), and see [The `display` Property](#Display-Property)). If the value is
    `nil`, Emacs does not perform bidirectional reordering in the
    buffer.

    The default value of `bidi-display-reordering` controls the
    reordering of strings which are not directly supplied by a buffer,
    including the text displayed in mode lines (see [Mode Line Format](#Mode-Line-Format))
    and header lines (see [Window Header Lines](#Header-Lines)).

Emacs never reorders the text of a unibyte buffer, even if
`bidi-display-reordering` is non-`nil` in the buffer. This
is because unibyte buffers contain raw bytes, not characters, and thus
lack the directionality properties required for reordering.
Therefore, to test whether text in a buffer will be reordered for
display, it is not enough to test the value of
`bidi-display-reordering` alone. The correct test is this:

```
 (if (and enable-multibyte-characters
          bidi-display-reordering)
     ;; Buffer is being reordered for display
   )
```

However, unibyte display and overlay strings *are* reordered if
their parent buffer is reordered. This is because plain-ASCII
strings are stored by Emacs as unibyte strings. If a unibyte display
or overlay string includes non-ASCII characters, these characters
are assumed to have left-to-right direction.

Text covered by `display` text properties, by overlays with
`display` properties whose value is a string, and by any other
properties that replace buffer text, is treated as a single unit when
it is reordered for display. That is, the entire chunk of text
covered by these properties is reordered together. Moreover, the
bidirectional properties of the characters in such a chunk of text are
ignored, and Emacs reorders them as if they were replaced with a
single character `U+FFFC`, known as the *Object Replacement
Character*. This means that placing a display property over a portion
of text may change the way that the surrounding text is reordered for
display. To prevent this unexpected effect, always place such
properties on text whose directionality is identical with text that
surrounds it.

Each paragraph of bidirectional text has a *base direction*,
either right-to-left or left-to-right. Left-to-right paragraphs are
displayed beginning at the left margin of the window, and are
truncated or continued when the text reaches the right margin.
Right-to-left paragraphs are displayed beginning at the right margin,
and are continued or truncated at the left margin.

Where exactly paragraphs start and end, for the purpose of the Emacs
UBA implementation, is determined by the following two
buffer-local variables (note that `paragraph-start` and
`paragraph-separate` have no influence on this). By default both
of these variables are `nil`, and paragraphs are bounded by empty
lines, i.e., lines that consist entirely of zero or more whitespace
characters followed by a newline.

Variable: **bidi-paragraph-start-re** [¶](#index-bidi_002dparagraph_002dstart_002dre)
:   If non-`nil`, this variable’s value should be a regular
    expression matching a line that starts or separates two paragraphs.
    The regular expression is always matched after a newline, so it is
    best to anchor it, i.e., begin it with a `"^"`.

Variable: **bidi-paragraph-separate-re** [¶](#index-bidi_002dparagraph_002dseparate_002dre)
:   If non-`nil`, this variable’s value should be a regular
    expression matching a line separates two paragraphs. The regular
    expression is always matched after a newline, so it is best to anchor
    it, i.e., begin it with a `"^"`.

If you modify any of these two variables, you should normally modify
both, to make sure they describe paragraphs consistently. For
example, to have each new line start a new paragraph for
bidi-reordering purposes, set both variables to `"^"`.

By default, Emacs determines the base direction of each paragraph by
looking at the text at its beginning. The precise method of
determining the base direction is specified by the UBA; in a
nutshell, the first character in a paragraph that has an explicit
directionality determines the base direction of the paragraph.
However, sometimes a buffer may need to force a certain base direction
for its paragraphs. For example, buffers containing program source
code should force all paragraphs to be displayed left-to-right. You
can use following variable to do this:

User Option: **bidi-paragraph-direction** [¶](#index-bidi_002dparagraph_002ddirection)
:   If the value of this buffer-local variable is the symbol
    `right-to-left` or `left-to-right`, all paragraphs in the
    buffer are assumed to have that specified direction. Any other value
    is equivalent to `nil` (the default), which means to determine
    the base direction of each paragraph from its contents.

    Modes for program source code should set this to `left-to-right`.
    Prog mode does this by default, so modes derived from Prog mode do not
    need to set this explicitly (see [Basic Major Modes](#Basic-Major-Modes)).

Function: **current-bidi-paragraph-direction** &optional buffer [¶](#index-current_002dbidi_002dparagraph_002ddirection)
:   This function returns the paragraph direction at point in the named
    buffer. The returned value is a symbol, either
    `left-to-right` or `right-to-left`. If buffer is
    omitted or `nil`, it defaults to the current buffer. If the
    buffer-local value of the variable `bidi-paragraph-direction` is
    non-`nil`, the returned value will be identical to that value;
    otherwise, the returned value reflects the paragraph direction
    determined dynamically by Emacs. For buffers whose value of
    `bidi-display-reordering` is `nil` as well as unibyte
    buffers, this function always returns `left-to-right`.

Sometimes there’s a need to move point in strict visual order,
either to the left or to the right of its current screen position.
Emacs provides a primitive to do that.

Function: **move-point-visually** direction [¶](#index-move_002dpoint_002dvisually)
:   This function moves point of the currently selected window to the
    buffer position that appears immediately to the right or to the left
    of point on the screen. If direction is positive, point will
    move one screen position to the right, otherwise it will move one
    screen position to the left. Note that, depending on the surrounding
    bidirectional context, this could potentially move point many buffer
    positions away. If invoked at the end of a screen line, the function
    moves point to the rightmost or leftmost screen position of the next
    or previous screen line, as appropriate for the value of
    direction.

    The function returns the new buffer position as its value.

Bidirectional reordering can have surprising and unpleasant effects
when two strings with bidirectional content are juxtaposed in a
buffer, or otherwise programmatically concatenated into a string of
text. A typical problematic case is when a buffer consists of
sequences of text fields separated by whitespace or punctuation
characters, like Buffer Menu mode or Rmail Summary Mode. Because the
punctuation characters used as separators have *weak
directionality*, they take on the directionality of surrounding text.
As result, a numeric field that follows a field with bidirectional
content can be displayed *to the left* of the preceding field,
messing up the expected layout. There are several ways to avoid this
problem:

* Append the special character U+200E LEFT-TO-RIGHT MARK, or
  LRM, to the end of each field that may have bidirectional
  content, or prepend it to the beginning of the following field. The
  function `bidi-string-mark-left-to-right`, described below, comes
  in handy for this purpose. (In a right-to-left paragraph, use
  U+200F RIGHT-TO-LEFT MARK, or RLM, instead.) This
  is one of the solutions recommended by the UBA.
* Include the tab character in the field separator. The tab character
  plays the role of *segment separator* in bidirectional reordering,
  causing the text on either side to be reordered separately.
* Separate fields with a `display` property or overlay with a
  property value of the form `(space . PROPS)` (see [Specified Spaces](#Specified-Space)). Emacs treats this display specification as a *paragraph
  separator*, and reorders the text on either side separately.

Function: **bidi-string-mark-left-to-right** string [¶](#index-bidi_002dstring_002dmark_002dleft_002dto_002dright)
:   This function returns its argument string, possibly modified,
    such that the result can be safely concatenated with another string,
    or juxtaposed with another string in a buffer, without disrupting the
    relative layout of this string and the next one on display. If the
    string returned by this function is displayed as part of a
    left-to-right paragraph, it will always appear on display to the left
    of the text that follows it. The function works by examining the
    characters of its argument, and if any of those characters could cause
    reordering on display, the function appends the LRM
    character to the string. The appended LRM character is made
    invisible by giving it an `invisible` text property of `t`
    (see [Invisible Text](#Invisible-Text)).

The reordering algorithm uses the bidirectional properties of the
characters stored as their `bidi-class` property
(see [Character Properties](#Character-Properties)). Lisp programs can change these
properties by calling the `put-char-code-property` function.
However, doing this requires a thorough understanding of the
UBA, and is therefore not recommended. Any changes to the
bidirectional properties of a character have global effect: they
affect all Emacs frames and windows.

Similarly, the `mirroring` property is used to display the
appropriate mirrored character in the reordered text. Lisp programs
can affect the mirrored display by changing this property. Again, any
such changes affect all of Emacs display.

The bidirectional properties of characters can be overridden by
inserting into the text special directional control characters,
LEFT-TO-RIGHT OVERRIDE (LRO) and RIGHT-TO-LEFT OVERRIDE
(RLO). Any characters between a RLO and the
following newline or POP DIRECTIONAL FORMATTING (PDF)
control character, whichever comes first, will be displayed as if they
were strong right-to-left characters, i.e. they will be reversed on
display. Similarly, any characters between LRO and
PDF or newline will display as if they were strong
left-to-right, and will *not* be reversed even if they are strong
right-to-left characters.

These overrides are useful when you want to make some text
unaffected by the reordering algorithm, and instead directly control
the display order. But they can also be used for malicious purposes,
known as *phishing*. Specifically, a URL on a Web page or a link
in an email message can be manipulated to make its visual appearance
unrecognizable, or similar to some popular benign location, while the
real location, interpreted by a browser in the logical order, is very
different.

Emacs provides a primitive that applications can use to detect
instances of text whose bidirectional properties were overridden so as
to make a left-to-right character display as if it were a
right-to-left character, or vice versa.

Function: **bidi-find-overridden-directionality** from to &optional object [¶](#index-bidi_002dfind_002doverridden_002ddirectionality)
:   This function looks at the text of the specified object between
    positions from (inclusive) and to (exclusive), and returns
    the first position where it finds a strong left-to-right character
    whose directional properties were forced to display the character as
    right-to-left, or for a strong right-to-left character that was forced
    to display as left-to-right. If it finds no such characters in the
    specified region of text, it returns `nil`.

    The optional argument object specifies which text to search, and
    defaults to the current buffer. If object is non-`nil`, it
    can be some other buffer, or it can be a string or a window. If it is
    a string, the function searches that string. If it is a window, the
    function searches the buffer displayed in that window. If a buffer
    whose text you want to examine is displayed in some window, we
    recommend to specify it by that window, rather than pass the buffer to
    the function. This is because telling the function about the window
    allows it to correctly account for window-specific overlays, which
    might change the result of the function if some text in the buffer is
    covered by overlays.

When text that includes mixed right-to-left and left-to-right
characters and bidirectional controls is copied into a different
location, it can change its visual appearance, and also can affect the
visual appearance of the surrounding text at destination. This is
because reordering of bidirectional text specified by the
UBA has non-trivial context-dependent effects both on the
copied text and on the text at copy destination that will surround it.

Sometimes, a Lisp program may need to preserve the exact visual
appearance of the copied text at destination, and of the text that
surrounds the copy. Lisp programs can use the following function to
achieve that effect.

Function: **buffer-substring-with-bidi-context** start end &optional no-properties [¶](#index-buffer_002dsubstring_002dwith_002dbidi_002dcontext)
:   This function works similar to `buffer-substring` (see [Examining Buffer Contents](#Buffer-Contents)), but it prepends and appends to the copied text bidi
    directional control characters necessary to preserve the visual
    appearance of the text when it is inserted at another place. Optional
    argument no-properties, if non-`nil`, means remove the text
    properties from the copy of the text.

---

Next: [Preparing Lisp code for distribution](#Packaging), Previous: [Emacs Display](#Display), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 43 Operating System Interface [¶](#Operating-System-Interface)

This chapter is about starting and getting out of Emacs, access to
values in the operating system environment, and terminal input, output.

See [Building Emacs](#Building-Emacs), for related information. See [Emacs Display](#Display), for
additional operating system status information pertaining to the
terminal and the screen.

* [Starting Up Emacs](#Starting-Up)
* [Getting Out of Emacs](#Getting-Out)
* [Operating System Environment](#System-Environment)
* [User Identification](#User-Identification)
* [Time of Day](#Time-of-Day)
* [Time Zone Rules](#Time-Zone-Rules)
* [Time Conversion](#Time-Conversion)
* [Parsing and Formatting Times](#Time-Parsing)
* [Processor Run time](#Processor-Run-Time)
* [Time Calculations](#Time-Calculations)
* [Timers for Delayed Execution](#Timers)
* [Idle Timers](#Idle-Timers)
* [Terminal Input](#Terminal-Input)
* [Terminal Output](#Terminal-Output)
* [Sound Output](#Sound-Output)
* [Operating on X11 Keysyms](#X11-Keysyms)
* [Batch Mode](#Batch-Mode)
* [Session Management](#Session-Management)
* [Desktop Notifications](#Desktop-Notifications)
* [Controlling System GUI Taskbar Features](#System-Taskbar)
* [Block System Sleep and Process Sleep Events](#System-Sleep)
* [Notifications on File Changes](#File-Notifications)
* [Dynamically Loaded Libraries](#Dynamic-Libraries)
* [Security Considerations](#Security-Considerations)

---

Next: [Getting Out of Emacs](#Getting-Out), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
