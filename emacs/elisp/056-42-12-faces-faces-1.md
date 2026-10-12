### 42.12 Faces [¶](#Faces-1)

A *face* is a collection of graphical attributes for displaying
text: font, foreground color, background color, optional underlining,
etc. Faces control how Emacs displays text in buffers, as well as
other parts of the frame such as the mode line.

One way to represent a face is as a property list of attributes,
like `(:foreground "red" :weight bold)`. Such a list is called
an *anonymous face*. For example, you can assign an anonymous
face as the value of the `face` text property, and Emacs will
display the underlying text with the specified attributes.
See [Properties with Special Meanings](#Special-Properties).

More commonly, a face is referred to via a *face name*: a Lisp
symbol associated with a set of face attributes[32](#FOOT32). Named faces are
defined using the `defface` macro (see [Defining Faces](#Defining-Faces)).
Emacs comes with several standard named faces (see [Basic Faces](#Basic-Faces)).

Some parts of Emacs require named faces (e.g., the functions
documented in [Face Attribute Functions](#Attribute-Functions)). Unless otherwise stated, we
will use the term *face* to refer only to named faces.

Function: **facep** object [¶](#index-facep)
:   This function returns a non-`nil` value if object is a
    named face: a Lisp symbol or string which serves as a face name.
    Otherwise, it returns `nil`.

* [Face Attributes](#Face-Attributes)
* [Defining Faces](#Defining-Faces)
* [Face Attribute Functions](#Attribute-Functions)
* [Displaying Faces](#Displaying-Faces)
* [Face Remapping](#Face-Remapping)
* [Functions for Working with Faces](#Face-Functions)
* [Automatic Face Assignment](#Auto-Faces)
* [Basic Faces](#Basic-Faces)
* [Font Selection](#Font-Selection)
* [Looking Up Fonts](#Font-Lookup)
* [Fontsets](#Fontsets)
* [Low-Level Font Representation](#Low_002dLevel-Font)

---

Next: [Defining Faces](#Defining-Faces), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.1 Face Attributes [¶](#Face-Attributes-1)

*Face attributes* determine the visual appearance of a face.
The following table lists all the face attributes, their possible
values, and their effects.

Apart from the values given below, each face attribute can have the
value `unspecified`. This special value means that the face
doesn’t specify that attribute directly. An `unspecified`
attribute tells Emacs to refer instead to a parent face (see the
description `:inherit` attribute below); or, failing that, to an
underlying face (see [Displaying Faces](#Displaying-Faces)). (However,
`unspecified` is not a valid value in `defface`.)

A face attribute can also have the value `reset`. This special
value stands for the value of the corresponding attribute of the
`default` face.

The `default` face must explicitly specify all attributes, and
cannot use the special value `reset`.

Some of these attributes are meaningful only on certain kinds of
displays. If your display cannot handle a certain attribute, the
attribute is ignored.

`:family`
:   Font family name (a string). See [Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fonts) in The GNU
    Emacs Manual, for more information about font families. The function
    `font-family-list` (see below) returns a list of available family
    names.

`:foundry`
:   The name of the *font foundry* for the font family specified by
    the `:family` attribute (a string). See [Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fonts) in The
    GNU Emacs Manual.

`:width`
:   Relative character width. This should be one of the symbols
    `ultra-condensed`, `extra-condensed`, `condensed`,
    `semi-condensed`, `normal`, `regular`, `medium`,
    `semi-expanded`, `expanded`, `extra-expanded`, or
    `ultra-expanded`.

`:height`
:   The height of the font. In the simplest case, this is an integer in
    units of 1/10 point.

    The value can also be floating point or a function, which
    specifies the height relative to an *underlying face*
    (see [Displaying Faces](#Displaying-Faces)). A floating-point value
    specifies the amount by which to scale the height of the
    underlying face. A function value is called
    with one argument, the height of the underlying face, and returns the
    height of the new face. If the function is passed an integer
    argument, it must return an integer.

    The height of the default face must be specified using an integer;
    floating point and function values are not allowed.

`:weight`
:   Font weight—one of the symbols (from densest to faintest)
    `ultra-bold`, `extra-bold`, `bold`, `semi-bold`,
    `normal`, `semi-light`, `light`, `extra-light`, or
    `ultra-light`. On text terminals which support
    variable-brightness text, any weight greater than normal is displayed
    as extra bright, and any weight less than normal is displayed as
    half-bright.

`:slant` [¶](#index-italic-text)
:   Font slant—one of the symbols `italic`, `oblique`,
    `normal`, `reverse-italic`, or `reverse-oblique`. On
    text terminals that support variable-brightness text, slanted text is
    displayed as half-bright.

`:foreground`
:   Foreground color, a string. The value can be a system-defined color
    name, or a hexadecimal color specification. See [Color Names](#Color-Names). On
    black-and-white displays, certain shades of gray are implemented by
    stipple patterns.

`:distant-foreground`
:   Alternative foreground color, a string. This is like `:foreground`
    but the color is only used as a foreground when the background color is
    near to the foreground that would have been used. This is useful for
    example when marking text (i.e., the region face). If the text has a foreground
    that is visible with the region face, that foreground is used.
    If the foreground is near the region face background,
    `:distant-foreground` is used instead so the text is readable.

`:background`
:   Background color, a string. The value can be a system-defined color
    name, or a hexadecimal color specification. See [Color Names](#Color-Names).

`:underline` [¶](#index-underlined-text)
:   Whether or not characters should be underlined, and in what
    way. The possible values of the `:underline` attribute are:

    `nil`
    :   Don’t underline.

    `t`
    :   Underline with the foreground color of the face.

    color
    :   Underline in color color, a string specifying a color.

    `(:color color :style style :position position)`
    :   color is either a string, or the symbol `foreground-color`,
        meaning the foreground color of the face. Omitting the attribute
        `:color` means to use the foreground color of the face.
        style is a symbol which sets the line-style to of the underline.
        It should be one of `line`, `double-line`, `wave`,
        `dots`, or `dashes`. GUI frames under most window systems
        support all the aforementioned underline styles, while on text terminals
        `double-line`, `wave` and `dots` are contingent on the
        availability of the `Smulx` or `Su` terminfo capabilities.
        Omitting the attribute `:style` means to use a straight line.
        position, if non-`nil`, means to display the underline at the
        descent of the text, instead of at the baseline level. If it is a
        number, then it specifies the amount of pixels above the descent to
        display the underline.

`:overline` [¶](#index-overlined-text)
:   Whether or not characters should be overlined, and in what color.
    If the value is `t`, overlining uses the foreground color of the
    face. If the value is a string, overlining uses that color. The
    value `nil` means do not overline.

`:strike-through` [¶](#index-strike_002dthrough-text)
:   Whether or not characters should be strike-through, and in what
    color. The value is used like that of `:overline`.

`:box` [¶](#index-2D-box)
:   Whether or not a box should be drawn around characters, its color, the
    width of the box lines, and 3D appearance. Here are the possible
    values of the `:box` attribute, and what they mean:

    `nil`
    :   Don’t draw a box.

    `t`
    :   Draw a box with lines of width 1, in the foreground color.

    color
    :   Draw a box with lines of width 1, in color color.

    `(:line-width (vwidth . hwidth) :color color :style style)`
    :   You can explicitly specify all aspects of the box with a plist of this
        form. Any element in this plist can be omitted.

        The values of vwidth and hwidth specify respectively the
        width of the vertical and horizontal lines to draw; they default to (1
        . 1). A negative horizontal or vertical width −n means
        to draw a line of width n that occupies the space of the
        underlying text, thus avoiding any increase in the character height or
        width. For simplification the width could be specified with only a
        single number n instead of a list, such case is equivalent to
        `((abs n) . n)`.

        The value of color specifies the color to draw with. The default
        is the background color of the face for 3D boxes and
        `flat-button`, and the foreground color of the face for other
        boxes.

        The value of style specifies whether to draw a 3D box. If it is
        `released-button`, the box looks like a 3D button that is not
        being pressed. If it is `pressed-button`, the box looks like a
        3D button that is being pressed. If it is `nil`,
        `flat-button` or omitted, a plain 2D box is used.

    If you use the `:box` face attribute on strings displayed instead
    of buffer text via the `display` text property, special
    considerations might apply if the surrounding buffer text also has the
    `:box` face attribute. See [Display Specs That Replace The Text](#Replacing-Specs). Also note that the
    vertical lines of the box are only drawn when `:box` attribute
    changes from `nil` to non-`nil` or vice versa; two consecutive
    face properties with a non-`nil` `:box` attribute will be
    displayed without the vertical line between them.

`:inverse-video`
:   Whether or not characters should be displayed in inverse video. The
    value should be `t` (yes) or `nil` (no).

`:stipple`
:   The background stipple, a bitmap.

    The value can be a string; that should be the name of a file containing
    external-format X bitmap data. The file is found in the directories
    listed in the variable `x-bitmap-file-path`.

    Alternatively, the value can specify the bitmap directly, with a list
    of the form `(width height data)`. Here,
    width and height specify the size in pixels, and
    data is a string containing the raw bits of the bitmap, row by
    row. Each row occupies *(width + 7) / 8* consecutive bytes
    in the string (which should be a unibyte string for best results).
    This means that each row always occupies at least one whole byte.

    If the value is `nil`, that means use no stipple pattern.

    Normally you do not need to set the stipple attribute, because it is
    used automatically to handle certain shades of gray.

`:font`
:   The font used to display the face. Its value should be a font object
    or a fontset. If it is a font object, it specifies the font to be
    used by the face for displaying ASCII characters. See [Low-Level Font Representation](#Low_002dLevel-Font), for information about font objects, font specs, and font
    entities. See [Fontsets](#Fontsets), for information about fontsets.

    When specifying this attribute using `set-face-attribute` or
    `set-face-font` (see [Face Attribute Functions](#Attribute-Functions)), you may also
    supply a font spec, a font entity, or a string. Emacs converts such
    values to an appropriate font object, and stores that font object as
    the actual attribute value. If you specify a string, the contents of
    the string should be a font name (see [Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fonts) in The GNU Emacs
    Manual); if the font name is an XLFD containing wildcards, Emacs
    chooses the first font matching those wildcards. Specifying this
    attribute also changes the values of the `:family`,
    `:foundry`, `:width`, `:height`, `:weight`, and
    `:slant` attributes.

`:inherit` [¶](#index-inheritance_002c-for-faces)
:   The name of a face from which to inherit attributes, or a list of face
    names. Attributes from inherited faces are merged into the face like
    an underlying face would be, with higher priority than underlying
    faces (see [Displaying Faces](#Displaying-Faces)). If the face to inherit from is
    `unspecified`, it is treated the same as `nil`, since Emacs
    never merges `:inherit` attributes. If a list of faces is used,
    attributes from faces earlier in the list override those from later
    faces.

`:extend`
:   Whether or not this face will be extended beyond end of line and will
    affect the display of the empty space between the end of line and the
    edge of the window. The value should be `t` to display the empty
    space between end of line and edge of the window using this face, or
    `nil` to not use this face for the space between the end of the
    line and the edge of the window. When Emacs merges several faces for
    displaying the empty space beyond end of line, only those faces with
    `:extend` non-`nil` will be merged. By default, only a
    small number of faces, notably, `region`, have this attribute
    set. This attribute is different from the others in that when a theme
    doesn’t specify an explicit value for a face, the value from the
    original face definition by `defface` is inherited
    (see [Defining Faces](#Defining-Faces)).

    Some modes, like `hl-line-mode`, use a face with an
    `:extend` property to mark the entire current line. Note,
    however, that Emacs will always allow you to move point after the
    final character in a buffer, and if the buffer ends with a newline
    character, point can be placed on what is seemingly a line at the end
    of the buffer—but Emacs can’t highlight that “line”, because it
    doesn’t really exist.

Function: **font-family-list** &optional frame [¶](#index-font_002dfamily_002dlist)
:   This function returns a list of available font family names. The
    optional argument frame specifies the frame on which the text is
    to be displayed; if it is `nil`, the selected frame is used.

User Option: **underline-minimum-offset** [¶](#index-underline_002dminimum_002doffset)
:   This variable specifies the minimum distance between the baseline and
    the underline, in pixels, when displaying underlined text.

User Option: **x-bitmap-file-path** [¶](#index-x_002dbitmap_002dfile_002dpath)
:   This variable specifies a list of directories for searching
    for bitmap files, for the `:stipple` attribute.

Function: **bitmap-spec-p** object [¶](#index-bitmap_002dspec_002dp)
:   This returns `t` if object is a valid bitmap specification,
    suitable for use with `:stipple` (see above). It returns
    `nil` otherwise.

---

Next: [Face Attribute Functions](#Attribute-Functions), Previous: [Face Attributes](#Face-Attributes), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.2 Defining Faces [¶](#Defining-Faces-1)

The usual way to define a face is through the `defface` macro.
This macro associates a face name (a symbol) with a default *face
spec*. A face spec is a construct which specifies what attributes a
face should have on any given terminal; for example, a face spec might
specify one foreground color on high-color terminals, and a different
foreground color on low-color terminals.

People are sometimes tempted to create a variable whose value is a
face name. In the vast majority of cases, this is not necessary; the
usual procedure is to define a face with `defface`, and then use
its name directly.

Note that once you have defined a face (usually with `defface`),
you cannot later undefine this face safely, except by restarting
Emacs.

Macro: **defface** face spec doc [keyword value]… [¶](#index-defface)
:   This macro declares face as a named face whose default face spec
    is given by spec. You should not quote the symbol face,
    and it should not end in ‘`-face`’ (that would be redundant). The
    argument doc is a documentation string for the face. The
    additional keyword arguments have the same meanings as in
    `defgroup` and `defcustom` (see [Common Item Keywords](#Common-Keywords)).

    If face already has a default face spec, this macro does
    nothing.

    The default face spec determines face’s appearance when no
    customizations are in effect (see [Customization Settings](#Customization)). If face
    has already been customized (via Custom themes or via customizations
    read from the init file), its appearance is determined by the custom
    face spec(s), which override the default face spec spec.
    However, if the customizations are subsequently removed, the
    appearance of face will again be determined by its default face
    spec.

    As an exception, if you evaluate a `defface` form with
    `C-M-x` (`eval-defun`) or with `C-x C-e`
    (`eval-last-sexp`) in Emacs Lisp mode, a special feature of these
    commands overrides any custom face specs on the face, causing the face
    to reflect exactly what the `defface` says.

    The spec argument is a *face spec*, which states how the
    face should appear on different kinds of terminals. It should be an
    alist whose elements each have the form

    ```
    (display . plist)
    ```

    display specifies a class of terminals (see below). plist
    is a property list of face attributes and their values, specifying how
    the face appears on such terminals. For backward compatibility, you
    can also write an element as `(display plist)`.

    The display part of an element of spec determines which
    terminals the element matches. If more than one element of spec
    matches a given terminal, the first element that matches is the one
    used for that terminal. There are three possibilities for
    display:

    `default`
    :   This element of spec doesn’t match any terminal; instead, it
        specifies defaults that apply to all terminals. This element, if
        used, must be the first element of spec. Each of the following
        elements can override any or all of these defaults.

    `t`
    :   This element of spec matches all terminals. Therefore, any
        subsequent elements of spec are never used. Normally `t`
        is used in the last (or only) element of spec.

    a list
    :   If display is a list, each element should have the form
        `(characteristic value…)`. Here
        characteristic specifies a way of classifying terminals, and the
        values are possible classifications which display should
        apply to. Here are the possible values of characteristic:

        `type`
        :   The kind of window system the terminal uses—either `graphic`
            (any graphics-capable display), `x`, `pc` (for the MS-DOS
            console), `w32` (for MS Windows 9X/NT/2K/XP), `haiku` (for
            Haiku), `pgtk` (for pure GTK), `android` (for Android), or
            `tty` (a non-graphics-capable display). See [window-system](#Window-Systems).

        `class`
        :   What kinds of colors the terminal supports—either `color`,
            `grayscale`, or `mono`.

        `background`
        :   The kind of background—either `light` or `dark`.

        `min-colors`
        :   An integer that represents the minimum number of colors the terminal
            should support. This matches a terminal if its
            `display-color-cells` value is at least the specified integer.

        `supports`
        :   Whether or not the terminal can display the face attributes given in
            value… (see [Face Attributes](#Face-Attributes)). See [Display Face Attribute Testing](#Display-Face-Attribute-Testing), for more information on exactly how this testing
            is done.

        If an element of display specifies more than one value for
        a given characteristic, any of those values is acceptable. If
        display has more than one element, each element should specify a
        different characteristic; then *each* characteristic of the
        terminal must match one of the values specified for it in
        display.

For example, here’s the definition of the standard face
`highlight`:

```
(defface highlight
  '((((class color) (min-colors 88) (background light))
     :background "darkseagreen2")
    (((class color) (min-colors 88) (background dark))
     :background "darkolivegreen")
    (((class color) (min-colors 16) (background light))
     :background "darkseagreen2")
    (((class color) (min-colors 16) (background dark))
     :background "darkolivegreen")
    (((class color) (min-colors 8))
     :background "green" :foreground "black")
    (t :inverse-video t))
  "Basic face for highlighting."
  :group 'basic-faces)
```

Internally, Emacs stores each face’s default spec in its
`face-defface-spec` symbol property (see [Symbol Properties](#Symbol-Properties)).
The `saved-face` property stores any face spec saved by the user
using the customization buffer; the `customized-face` property
stores the face spec customized for the current session, but not
saved; and the `theme-face` property stores an alist associating
the active customization settings and Custom themes with the face
specs for that face. The face’s documentation string is stored in the
`face-documentation` property.

Normally, a face is declared just once, using `defface`, and
any further changes to its appearance are applied using the Customize
framework (e.g., via the Customize user interface or via the
`custom-set-faces` function; see [Applying Customizations](#Applying-Customizations)), or
by face remapping (see [Face Remapping](#Face-Remapping)). In the rare event that
you need to change a face spec directly from Lisp, you can use the
`face-spec-set` function.

Function: **face-spec-set** face spec &optional spec-type [¶](#index-face_002dspec_002dset)
:   This function applies spec as a face spec for `face`.
    spec should be a face spec, as described in the above
    documentation for `defface`.

    This function also defines face as a valid face name if it is
    not already one, and (re)calculates its attributes on existing frames.

    The optional argument spec-type determines which spec to set.
    If it is omitted or `nil` or `face-override-spec`, this
    function sets the *override spec*, which overrides face specs on
    face of all the other types mentioned below. This is useful
    when calling this function outside of Custom code. If spec-type
    is `customized-face` or `saved-face`, this function sets the
    customized spec or the saved custom spec, respectively. If it is
    `face-defface-spec`, this function sets the default face spec
    (the same one set by `defface`). If it is `reset`, this
    function clears out all customization specs and override specs from
    face (in this case, the value of spec is ignored). The
    effect of any other value of spec-type on the face specs is
    reserved for internal use, but the function will still define
    face itself and recalculate its attributes, as described above.

---

Next: [Displaying Faces](#Displaying-Faces), Previous: [Defining Faces](#Defining-Faces), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.3 Face Attribute Functions [¶](#Face-Attribute-Functions)

This section describes functions for directly accessing and
modifying the attributes of a named face.

Function: **face-attribute** face attribute &optional frame inherit [¶](#index-face_002dattribute)
:   This function returns the value of the attribute attribute for
    face on frame. See [Face Attributes](#Face-Attributes), for the supported
    attributes.

    If frame is omitted or `nil`, that means the selected frame
    (see [Input Focus](#Input-Focus)). If frame is `t`, this function
    returns the value of the specified attribute for newly-created frames,
    i.e. the value of the attribute before applying the face spec in the
    face’s `defface` definition (see [Defining Faces](#Defining-Faces)) or the spec
    set by `face-spec-set`. This default value of attribute is
    normally `unspecified`, unless you have specified some other
    value using `set-face-attribute`; see below.

    If inherit is `nil`, only attributes directly defined by
    face are considered, so the return value may be
    `unspecified`, or a relative value. If inherit is
    non-`nil`, face’s definition of attribute is merged
    with the faces specified by its `:inherit` attribute; however the
    return value may still be `unspecified` or relative. If
    inherit is a face or a list of faces, then the result is further
    merged with that face (or faces), until it becomes specified and
    absolute.

    To ensure that the return value is always specified and absolute, use
    a value of `default` for inherit; this will resolve any
    unspecified or relative values by merging with the `default` face
    (which is always completely specified).

    For example,

    ```
    (face-attribute 'bold :weight)
         ⇒ bold
    ```

Function: **face-attribute-relative-p** attribute value [¶](#index-face_002dattribute_002drelative_002dp)
:   This function returns non-`nil` if value, when used as the
    value of the face attribute attribute, is relative. This means
    it would modify, rather than completely override, any value that comes
    from a subsequent face in the face list or that is inherited from
    another face.

    `unspecified` is a relative value for all attributes. For
    `:height`, floating point and function values are also relative.

    For example:

    ```
    (face-attribute-relative-p :height 2.0)
         ⇒ t
    ```

Function: **face-all-attributes** face &optional frame inherit [¶](#index-face_002dall_002dattributes)
:   This function returns an alist of attributes of face. The
    elements of the result are name-value pairs of the form
    `(attr-name . attr-value)`. Optional argument
    frame specifies the frame whose definition of face to
    return; if omitted or `nil`, the returned value describes the
    default attributes of face for newly created frames, i.e. the
    values these attributes have before applying the face spec in the face’s
    `defface` definition or the spec set by `face-spec-set`.
    These default values of the attributes are normally `unspecified`,
    unless you have specified some other value using
    `set-face-attribute`; see below. The optional argument
    inherit has the same meaning as the same argument to
    `face-attribute`, which see. This is useful when you want the face
    attributes to be absolute and not `unspecified`.

Function: **merge-face-attribute** attribute value1 value2 [¶](#index-merge_002dface_002dattribute)
:   If value1 is a relative value for the face attribute
    attribute, returns it merged with the underlying value
    value2; otherwise, if value1 is an absolute value for the
    face attribute attribute, returns value1 unchanged.

Normally, Emacs uses the face specs of each face to automatically
calculate its attributes on each frame (see [Defining Faces](#Defining-Faces)). The
function `set-face-attribute` can override this calculation by
directly assigning attributes to a face, either on a specific frame or
for all frames. This function is mostly intended for internal usage.

Function: **set-face-attribute** face frame &rest arguments [¶](#index-set_002dface_002dattribute)
:   This function sets one or more attributes of face for
    frame. The attributes specified in this way override the face
    spec(s) belonging to face. See [Face Attributes](#Face-Attributes), for the
    supported attributes.

    The extra arguments arguments specify the attributes to set, and
    the values for them. They should consist of alternating attribute
    names (such as `:family` or `:underline`) and values. Thus,

    ```
    (set-face-attribute 'foo nil :weight 'bold :slant 'italic)
    ```

    sets the attribute `:weight` to `bold` and the attribute
    `:slant` to `italic`.

    If frame is `t`, this function sets the default attributes
    for newly created frames; they will effectively override the attribute
    values specified by `defface`. If frame is `nil`,
    this function sets the attributes for all existing frames, as well as
    for newly created frames.

    To *reset* the value of an attribute, that is, to indicate that
    the face doesn’t by itself specify a value for the attribute, use the
    special value `unspecified` (*not* `nil`!) for the
    attribute, and set the frame argument to `t`, in addition
    to the call with frame set to `nil`. This is because the
    default attributes for newly created frames are merged with the face’s
    spec in `defface` when a new frame is created, and so having
    `unspecified` in the default attributes for new frames will be
    unable to override `defface`; the special call to this function
    as described above will arrange for `defface` to be overridden.

    Note that the attribute-value pairs are evaluated in the order they
    are specified, with the exception of the `:family` and
    `:foundry` attributes, which are evaluated first. This means
    that if a certain attribute is specified more than once, only the last
    value will be used. It also means that in some cases a different
    order of attributes will produce different results. For example, when
    `:weight` is placed before `:font`, the weight value is
    applied to the current font of the face, and might be rounded to the
    closest available weight of that font, whereas when `:font` is
    placed before `:weight` the weight value is applied to the
    specified font.

The following commands and functions mostly provide compatibility
with old versions of Emacs. They work by calling
`set-face-attribute`. Values of `t` and `nil` (or
omitted) for their frame argument are handled just like
`set-face-attribute` and `face-attribute`. The commands
read their arguments using the minibuffer, if called interactively.

Command: **set-face-foreground** face color &optional frame [¶](#index-set_002dface_002dforeground)

Command: **set-face-background** face color &optional frame [¶](#index-set_002dface_002dbackground)
:   These set the `:foreground` attribute (or `:background`
    attribute, respectively) of face to color.

Command: **set-face-stipple** face pattern &optional frame [¶](#index-set_002dface_002dstipple)
:   This sets the `:stipple` attribute of face to
    pattern.

Command: **set-face-font** face font &optional frame [¶](#index-set_002dface_002dfont)
:   Change the font-related attributes of face to those of
    font (a string or a font object). See [face-font-attribute](#face_002dfont_002dattribute),
    for the supported formats of the font argument. This function
    sets the attribute `:font` of the face, and indirectly also the
    `:family`, `:foundry`, `:width`, `:height`,
    `:weight`, and `:slant` attributes, as defined by the font.
    If frame is non-`nil`, only change the attributes on the
    specified frame.

Function: **set-face-bold** face bold-p &optional frame [¶](#index-set_002dface_002dbold)
:   This sets the `:weight` attribute of face to normal
    if bold-p is `nil`, and to bold otherwise.

Function: **set-face-italic** face italic-p &optional frame [¶](#index-set_002dface_002ditalic)
:   This sets the `:slant` attribute of face to normal if
    italic-p is `nil`, and to italic otherwise.

Command: **set-face-underline** face underline &optional frame [¶](#index-set_002dface_002dunderline)
:   This sets the `:underline` attribute of face to
    underline.

Command: **set-face-inverse-video** face inverse-video-p &optional frame [¶](#index-set_002dface_002dinverse_002dvideo)
:   This sets the `:inverse-video` attribute of face to
    inverse-video-p.

Command: **invert-face** face &optional frame [¶](#index-invert_002dface)
:   This swaps the foreground and background colors of face face.

Command: **set-face-extend** face extend &optional frame [¶](#index-set_002dface_002dextend)
:   This sets the `:extend` attribute of face to
    extend.

The following functions examine the attributes of a face. They
mostly provide compatibility with old versions of Emacs. If you don’t
specify frame, they refer to the selected frame; `t` refers
to the default data for new frames. They return `unspecified` if
the face doesn’t define any value for that attribute. If
inherit is `nil`, only an attribute directly defined by the
face is returned. If inherit is non-`nil`, any faces
specified by its `:inherit` attribute are considered as well, and
if inherit is a face or a list of faces, then they are also
considered, until a specified attribute is found. To ensure that the
return value is always specified, use a value of `default` for
inherit.

Function: **face-font** face &optional frame character [¶](#index-face_002dfont)
:   This function returns the name of the font used by the specified
    face.

    If the optional argument frame is specified, it returns the name
    of the font of face for that frame; frame defaults to the
    selected frame if it is `nil` or omitted. If frame is
    `t`, the function reports on the font defaults for face to
    be used for new frames.

    By default, the returned font is for displaying ASCII characters, but
    if frame is anything but `t`, and the optional third
    argument character is supplied, the function returns the font
    name used by face for that character.

Function: **face-foreground** face &optional frame inherit [¶](#index-face_002dforeground)

Function: **face-background** face &optional frame inherit [¶](#index-face_002dbackground)
:   These functions return the foreground color (or background color,
    respectively) of face face, as a string. If the color is
    unspecified, they return `nil`.

Function: **face-stipple** face &optional frame inherit [¶](#index-face_002dstipple)
:   This function returns the name of the background stipple pattern of face
    face, or `nil` if it doesn’t have one.

Function: **face-bold-p** face &optional frame inherit [¶](#index-face_002dbold_002dp)
:   This function returns a non-`nil` value if the `:weight`
    attribute of face is bolder than normal (i.e., one of
    `semi-bold`, `bold`, `extra-bold`, or
    `ultra-bold`). Otherwise, it returns `nil`.

Function: **face-italic-p** face &optional frame inherit [¶](#index-face_002ditalic_002dp)
:   This function returns a non-`nil` value if the `:slant`
    attribute of face is `italic` or `oblique`, and
    `nil` otherwise.

Function: **face-underline-p** face &optional frame inherit [¶](#index-face_002dunderline_002dp)
:   This function returns non-`nil` if face face specifies
    a non-`nil` `:underline` attribute.

Function: **face-inverse-video-p** face &optional frame inherit [¶](#index-face_002dinverse_002dvideo_002dp)
:   This function returns non-`nil` if face face specifies
    a non-`nil` `:inverse-video` attribute.

Function: **face-extend-p** face &optional frame inherit [¶](#index-face_002dextend_002dp)
:   This function returns non-`nil` if face face specifies
    a non-`nil` `:extend` attribute. The inherit argument
    is passed to `face-attribute`.

---

Next: [Face Remapping](#Face-Remapping), Previous: [Face Attribute Functions](#Attribute-Functions), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.4 Displaying Faces [¶](#Displaying-Faces-1)

When Emacs displays a given piece of text, the visual appearance of
the text may be determined by faces drawn from different sources. If
these various sources together specify more than one face for a
particular character, Emacs merges the attributes of the various
faces. Here is the order in which Emacs merges the faces, from
highest to lowest priority:

* If the text consists of a special glyph, the glyph can specify a
  particular face. See [Glyphs](#Glyphs).
* If the text lies within an active region, Emacs highlights it using
  the `region` face. See [Standard Faces](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Standard-Faces) in The GNU Emacs
  Manual.
* If the text lies within an overlay with a non-`nil` `face`
  property, Emacs applies the face(s) specified by that property. If
  the overlay has a `mouse-face` property and the mouse is near
  enough to the overlay, Emacs applies the face or face attributes
  specified by the `mouse-face` property instead. See [Overlay Properties](#Overlay-Properties).

  When multiple overlays cover the same character, an overlay with
  higher priority overrides those with lower priority. See [Overlays](#Overlays).
* If the text contains a `face` or `mouse-face` property,
  Emacs applies the specified faces and face attributes. See [Properties with Special Meanings](#Special-Properties). (This is how Font Lock mode faces are applied.
  See [Font Lock Mode](#Font-Lock-Mode).)
* If the text lies within the mode line of the selected window, Emacs
  applies the `mode-line` face. For the mode line of a
  non-selected window, Emacs applies the `mode-line-inactive` face.
  For a header line, Emacs applies the `header-line` face.
  For a tab line, Emacs applies the `tab-line` face.
* If the text comes from an overlay string via `before-string` or
  `after-string` properties (see [Overlay Properties](#Overlay-Properties)), or from a
  display string (see [Other Display Specifications](#Other-Display-Specs)), and the string doesn’t
  contain a `face` or `mouse-face` property, or these
  properties leave some face attributes undefined, but the buffer text
  affected by the overlay/display property does define a face or those
  attributes, Emacs applies the face attributes of the “underlying”
  buffer text. However, this does not apply to strings displayed in the
  display margins, which use the `margin` face as the base instead
  (see [Displaying in the Margins](#Display-Margins)).
* If the text is to be shown in the display margins, Emacs uses the
  `margin` face as the base, so any attribute not specified by the
  string’s own face is taken from the `margin` face.
* If any given attribute has not been specified during the preceding
  steps, Emacs applies the attribute of the `default` face.

At each stage, if a face has a valid `:inherit` attribute,
Emacs treats any attribute with an `unspecified` value as having
the corresponding value drawn from the parent face(s). see [Face Attributes](#Face-Attributes). Note that the parent face(s) may also leave the
attribute unspecified; in that case, the attribute remains unspecified
at the next level of face merging.

---

Next: [Functions for Working with Faces](#Face-Functions), Previous: [Displaying Faces](#Displaying-Faces), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.5 Face Remapping [¶](#Face-Remapping-1)

The variable `face-remapping-alist` is used for buffer-local or
global changes in the appearance of a face. For instance, it is used
to implement the `text-scale-adjust` command (see [Text
Scale](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Text-Scale) in The GNU Emacs Manual).

Variable: **face-remapping-alist** [¶](#index-face_002dremapping_002dalist)
:   The value of this variable is an alist whose elements have the form
    `(face . remapping)`. This causes Emacs to display
    any text having the face face with remapping, rather than
    the ordinary definition of face.

    remapping may be any face spec suitable for a `face` text
    property: either a face (i.e., a face name or a property list of
    attribute/value pairs), or a list of faces. For details, see the
    description of the `face` text property in [Properties with Special Meanings](#Special-Properties). remapping serves as the complete specification for
    the remapped face—it replaces the normal definition of face,
    instead of modifying it.

    If `face-remapping-alist` is buffer-local, its local value takes
    effect only within that buffer. If `face-remapping-alist`
    includes faces applicable only to certain windows, by using the
    `(:filtered (:window param val) spec)`,
    that face takes effect only in windows that match the filter
    conditions (see [Properties with Special Meanings](#Special-Properties)). To turn off face filtering
    temporarily, bind `face-filters-always-match` to a non-`nil`
    value, then all face filters will match any window.

    Note: face remapping is non-recursive. If remapping references
    the same face name face, either directly or via the
    `:inherit` attribute of some other face in remapping, that
    reference uses the normal definition of face. For instance, if
    the `mode-line` face is remapped using this entry in
    `face-remapping-alist`:

    ```
    (mode-line italic mode-line)
    ```

    then the new definition of the `mode-line` face inherits from the
    `italic` face, and the *normal* (non-remapped) definition of
    `mode-line` face.

The following functions implement a higher-level interface to
`face-remapping-alist`. Most Lisp code should use these
functions instead of setting `face-remapping-alist` directly, to
avoid trampling on remappings applied elsewhere. These functions are
intended for buffer-local remappings, so they all make
`face-remapping-alist` buffer-local as a side-effect. They manage
`face-remapping-alist` entries of the form

```
  (face relative-spec-1 relative-spec-2 ... base-spec)
```

where, as explained above, each of the relative-spec-N and
base-spec is either a face name, or a property list of
attribute/value pairs. Each of the *relative remapping* entries,
relative-spec-N, is managed by the
`face-remap-add-relative` and `face-remap-remove-relative`
functions; these are intended for simple modifications like changing
the text size. The *base remapping* entry, base-spec, has
the lowest priority and is managed by the `face-remap-set-base`
and `face-remap-reset-base` functions; it is intended for major
modes to remap faces in the buffers they control.

Function: **face-remap-add-relative** face &rest specs [¶](#index-face_002dremap_002dadd_002drelative)
:   This function adds specs as relative remappings for face
    face in the current buffer. specs should be a list where
    each element is either a face name, or a property list of
    attribute/value pairs.

    The return value is a Lisp object that serves as a cookie; you can
    pass this object as an argument to `face-remap-remove-relative`
    if you need to remove the remapping later.

    ```
    ;; Remap the 'escape-glyph' face into a combination
    ;; of the 'highlight' and 'italic' faces:
    (face-remap-add-relative 'escape-glyph 'highlight 'italic)

    ;; Increase the size of the 'default' face by 50%:
    (face-remap-add-relative 'default :height 1.5)
    ```

    Note that buffer-local face remapping does not work reliably for
    parent faces of basic faces (see [Basic Faces](#Basic-Faces)). (These are the
    faces that are used in mode lines, header lines, and other basic
    decorations of windows and frames.) For instance,
    `mode-line-inactive` inherits from `mode-line`, but
    remapping `mode-line` won’t normally have the desired effect on
    `mode-line-inactive`, especially if done locally for some
    buffers. Instead you have to remap `mode-line-inactive`
    directly.

Function: **face-remap-remove-relative** cookie [¶](#index-face_002dremap_002dremove_002drelative)
:   This function removes a relative remapping previously added by
    `face-remap-add-relative`. cookie should be the Lisp
    object returned by `face-remap-add-relative` when the remapping
    was added.

Function: **face-remap-set-base** face &rest specs [¶](#index-face_002dremap_002dset_002dbase)
:   This function sets the base remapping of face in the current
    buffer to specs. If specs is empty, the default base
    remapping is restored, similar to calling `face-remap-reset-base`
    (see below); note that this is different from specs containing a
    single value `nil`, which has the opposite result (the global
    definition of face is ignored).

    This overwrites the default base-spec, which inherits the global
    face definition, so it is up to the caller to add such inheritance if
    so desired.

Function: **face-remap-reset-base** face [¶](#index-face_002dremap_002dreset_002dbase)
:   This function sets the base remapping of face to its default
    value, which inherits from face’s global definition.

---

Next: [Automatic Face Assignment](#Auto-Faces), Previous: [Face Remapping](#Face-Remapping), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.6 Functions for Working with Faces [¶](#Functions-for-Working-with-Faces)

Here are additional functions for creating and working with faces.

Function: **face-list** [¶](#index-face_002dlist)
:   This function returns a list of all defined face names.

Function: **face-id** face [¶](#index-face_002did)
:   This function returns the *face number* of face face. This
    is a number that uniquely identifies a face at low levels within
    Emacs. It is seldom necessary to refer to a face by its face number.
    However, functions that manipulate glyphs, such as
    `make-glyph-code` and `glyph-face` (see [Glyphs](#Glyphs)) access
    the face numbers internally. Note that the face number is stored as
    the value of the `face` property of the face symbol, so we
    recommend not to set that property of a face to any value of your own.

Function: **face-documentation** face [¶](#index-face_002ddocumentation-1)
:   This function returns the documentation string of face face, or
    `nil` if none was specified for it.

Function: **face-equal** face1 face2 &optional frame [¶](#index-face_002dequal)
:   This returns `t` if the faces face1 and face2 have the
    same attributes for display.

Function: **face-differs-from-default-p** face &optional frame [¶](#index-face_002ddiffers_002dfrom_002ddefault_002dp)
:   This returns non-`nil` if the face face displays
    differently from the default face.

A *face alias* provides an equivalent name for a face. You can
define a face alias by giving the alias symbol the `face-alias`
property, with a value of the target face name. The following example
makes `modeline` an alias for the `mode-line` face.

```
(put 'modeline 'face-alias 'mode-line)
```

Macro: **define-obsolete-face-alias** obsolete-face current-face when [¶](#index-define_002dobsolete_002dface_002dalias)
:   This macro defines obsolete-face as an alias for
    current-face, and also marks it as obsolete, indicating that it
    may be removed in the future. when should be a string indicating
    when obsolete-face was made obsolete (usually a version number
    string).

---

Next: [Basic Faces](#Basic-Faces), Previous: [Functions for Working with Faces](#Face-Functions), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.7 Automatic Face Assignment [¶](#Automatic-Face-Assignment)

This hook is used for automatically assigning faces to text in the
buffer. It is part of the implementation of Jit-Lock mode, used by
Font-Lock.

Variable: **fontification-functions** [¶](#index-fontification_002dfunctions)
:   This variable holds a list of functions that are called by Emacs
    redisplay as needed, just before doing redisplay. They are called even
    when Font Lock Mode isn’t enabled. When Font Lock Mode is enabled, this
    variable usually holds just one function, `jit-lock-function`.

    The functions are called in the order listed, with one argument, a
    buffer position pos. Collectively they should attempt to assign
    faces to the text in the current buffer starting at pos.

    The functions should record the faces they assign by setting the
    `face` property. They should also add a non-`nil`
    `fontified` property to all the text they have assigned faces to.
    That property tells redisplay that faces have been assigned to that text
    already.

    It is probably a good idea for the functions to do nothing if the
    character after pos already has a non-`nil` `fontified`
    property, but this is not required. If one function overrides the
    assignments made by a previous one, the properties after the last
    function finishes are the ones that really matter.

    For efficiency, we recommend writing these functions so that they
    usually assign faces to around 400 to 600 characters at each call.

    Note that, when the buffer text includes very long lines, these
    functions are called as if they were in a `with-restriction` form
    (see [Narrowing](#Narrowing)), with a
    `long-line-optimizations-in-fontification-functions` label and
    with the buffer narrowed to a portion around pos.

---

Next: [Font Selection](#Font-Selection), Previous: [Automatic Face Assignment](#Auto-Faces), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.8 Basic Faces [¶](#Basic-Faces-1)

If your Emacs Lisp program needs to assign some faces to text, it is
often a good idea to use certain existing faces or inherit from them,
rather than defining entirely new faces. This way, if other users
have customized those existing faces to give Emacs a certain look,
your program will fit in without additional customization.

Some of the *basic faces* defined in Emacs are listed below. In
addition to these, you might want to make use of the Font Lock faces
for syntactic highlighting, if highlighting is not already handled by
Font Lock mode, or if some Font Lock faces are not in use.
See [Faces for Font Lock](#Faces-for-Font-Lock).

`default`
:   The default face, whose attributes are all specified. All other faces
    implicitly inherit from it: any unspecified attribute defaults to the
    attribute on this face (see [Face Attributes](#Face-Attributes)).

`mode-line-active` [¶](#index-mode_002dline-faces)

`mode-line-inactive`

`header-line`

`tab-line`
:   Basic faces used for the mode line, header line, and tab line.

`tool-bar` [¶](#index-tool_002dbar-face)

`tab-bar`

`fringe`

`scroll-bar`

`window-divider`

`border`

`child-frame-border`
:   Basic faces used for the corresponding decorations of GUI frames.

`cursor` [¶](#index-cursor-face)
:   The basic face used for the text cursor.

`margin` [¶](#index-margin-face)
:   The basic face used for window margins, both on the left and on the
    right. It is commonly used to customize the background of the empty
    areas of the margins, and it also provides the base attributes for
    strings displayed in the margins (see [Displaying in the Margins](#Display-Margins)). It
    inherits from the `default` face.

`mouse` [¶](#index-mouse-face)
:   The basic face used for displaying mouse-sensitive text when the mouse
    pointer is on that text.

`bold`

`italic`

`bold-italic`

`underline`

`fixed-pitch`

`fixed-pitch-serif`

`variable-pitch`
:   These have the attributes indicated by their names (e.g., `bold`
    has a bold `:weight` attribute), with all other attributes
    unspecified (and so given by `default`).

`shadow` [¶](#index-shadow-face)
:   For dimmed-out text. For example, it is used for the ignored
    part of a filename in the minibuffer (see [Minibuffers for File Names](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Minibuffer-File) in The GNU Emacs Manual).

`link`

`link-visited`
:   For clickable text buttons that send the user to a different
    buffer or location.

`highlight`
:   For stretches of text that should temporarily stand out. For example,
    it is commonly assigned to the `mouse-face` property for cursor
    highlighting (see [Properties with Special Meanings](#Special-Properties)).

`match` [¶](#index-match-face)

`isearch`

`lazy-highlight`
:   For text matching (respectively) permanent search matches, interactive
    search matches, and lazy highlighting other matches than the current
    interactive one.

`error` [¶](#index-error-face)

`warning`

`success`
:   For text concerning errors, warnings, or successes. For example,
    these are used for messages in `*Compilation*` buffers.

---

Next: [Looking Up Fonts](#Font-Lookup), Previous: [Basic Faces](#Basic-Faces), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.9 Font Selection [¶](#Font-Selection-1)

Before Emacs can draw a character on a graphical display, it must
select a *font* for that character[33](#FOOT33). See [Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fonts) in The GNU Emacs Manual. Normally,
Emacs automatically chooses a font based on the faces assigned to that
character—specifically, the face attributes `:family`,
`:weight`, `:slant`, and `:width` (see [Face Attributes](#Face-Attributes)). The choice of font also depends on the character to be
displayed; some fonts can only display a limited set of characters.
If no available font exactly fits the requirements, Emacs looks for
the *closest matching font*. The variables in this section
control how Emacs makes this selection.

User Option: **face-font-family-alternatives** [¶](#index-face_002dfont_002dfamily_002dalternatives)
:   If a given family is specified but does not exist, this variable
    specifies alternative font families to try. Each element should have
    this form:

    ```
    (family alternate-families...)
    ```

    If family is specified but not available, Emacs will try the other
    families given in alternate-families, one by one, until it finds a
    family that does exist.

User Option: **face-font-selection-order** [¶](#index-face_002dfont_002dselection_002dorder)
:   If there is no font that exactly matches all desired face attributes
    (`:width`, `:height`, `:weight`, and `:slant`),
    this variable specifies the order in which these attributes should be
    considered when selecting the closest matching font. The value should
    be a list containing those four attribute symbols, in order of
    decreasing importance. The default is `(:width :height :weight
    :slant)`.

    Font selection first finds the best available matches for the first
    attribute in the list; then, among the fonts which are best in that
    way, it searches for the best matches in the second attribute, and so
    on.

    The attributes `:weight` and `:width` have symbolic values in
    a range centered around `normal`. Matches that are more extreme
    (farther from `normal`) are somewhat preferred to matches that are
    less extreme (closer to `normal`); this is designed to ensure that
    non-normal faces contrast with normal ones, whenever possible.

    One example of a case where this variable makes a difference is when the
    default font has no italic equivalent. With the default ordering, the
    `italic` face will use a non-italic font that is similar to the
    default one. But if you put `:slant` before `:height`, the
    `italic` face will use an italic font, even if its height is not
    quite right.

User Option: **face-font-registry-alternatives** [¶](#index-face_002dfont_002dregistry_002dalternatives)
:   This variable lets you specify alternative font registries to try, if a
    given registry is specified and doesn’t exist. Each element should have
    this form:

    ```
    (registry alternate-registries...)
    ```

    If registry is specified but not available, Emacs will try the
    other registries given in alternate-registries, one by one,
    until it finds a registry that does exist.

Emacs can make use of scalable fonts, but by default it does not use
them.

User Option: **scalable-fonts-allowed** [¶](#index-scalable_002dfonts_002dallowed)
:   This variable controls which scalable fonts to use. A value of
    `nil`, the default, means do not use scalable fonts. `t`
    means to use any scalable font that seems appropriate for the text.

    Otherwise, the value must be a list of regular expressions. Then a
    scalable font is enabled for use if its name matches any regular
    expression in the list. For example,

    ```
    (setq scalable-fonts-allowed '("iso10646-1$"))
    ```

    allows the use of scalable fonts with registry `iso10646-1`.

Variable: **face-font-rescale-alist** [¶](#index-face_002dfont_002drescale_002dalist)
:   This variable specifies scaling for certain faces. Its value should
    be a list of elements of the form

    ```
    (fontname-regexp . scale-factor)
    ```

    If fontname-regexp matches the font name that is about to be
    used, this says to choose a larger similar font according to the
    factor scale-factor. You would use this feature to normalize
    the font size if certain fonts are bigger or smaller than their
    nominal heights and widths would suggest.

---

Next: [Fontsets](#Fontsets), Previous: [Font Selection](#Font-Selection), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.10 Looking Up Fonts [¶](#Looking-Up-Fonts)

Function: **x-list-fonts** name &optional reference-face frame maximum width [¶](#index-x_002dlist_002dfonts)
:   This function returns a list of available font names that match
    name. name should be a string containing a font name in
    either the Fontconfig, GTK+, or XLFD format (see [Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fonts) in The
    GNU Emacs Manual). Within an XLFD string, wildcard characters may be
    used: the ‘`*`’ character matches any substring, and the ‘`?`’
    character matches any single character. Case is ignored when matching
    font names.

    If the optional arguments reference-face and frame are
    specified, the returned list includes only fonts that are the same
    size as reference-face (a face name) currently is on the frame
    frame.

    The optional argument maximum sets a limit on how many fonts to
    return. If it is non-`nil`, then the return value is truncated
    after the first maximum matching fonts. Specifying a small
    value for maximum can make this function much faster, in cases
    where many fonts match the pattern.

    The optional argument width specifies a desired font width. If
    it is non-`nil`, the function only returns those fonts whose
    characters are (on average) width times as wide as
    reference-face.

Function: **x-family-fonts** &optional family frame [¶](#index-x_002dfamily_002dfonts)
:   This function returns a list describing the available fonts for family
    family on frame. If family is omitted or `nil`,
    this list applies to all families, and therefore, it contains all
    available fonts. Otherwise, family must be a string; it may
    contain the wildcards ‘`?`’ and ‘`*`’.

    The list describes the display that frame is on; if frame is
    omitted or `nil`, it applies to the selected frame’s display
    (see [Input Focus](#Input-Focus)).

    Each element in the list is a vector of the following form:

    ```
    [family width point-size weight slant
     fixed-p full registry-and-encoding]
    ```

    The first five elements correspond to face attributes; if you
    specify these attributes for a face, it will use this font.

    The last three elements give additional information about the font.
    fixed-p is non-`nil` if the font is fixed-pitch.
    full is the full name of the font, and
    registry-and-encoding is a string giving the registry and
    encoding of the font.

---

Next: [Low-Level Font Representation](#Low_002dLevel-Font), Previous: [Looking Up Fonts](#Font-Lookup), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.11 Fontsets [¶](#Fontsets-1)

A *fontset* is a list of fonts, each assigned to a range of
character codes. An individual font cannot display the whole range of
characters that Emacs supports, but a fontset can. Fontsets have names,
just as fonts do, and you can use a fontset name in place of a font name
when you specify the font for a frame or a face. Here is
information about defining a fontset under Lisp program control.

Function: **create-fontset-from-fontset-spec** fontset-spec &optional style-variant-p noerror [¶](#index-create_002dfontset_002dfrom_002dfontset_002dspec)
:   This function defines a new fontset according to the specification
    string fontset-spec. The string should have this format:

    ```
    fontpattern, [charset:font]...
    ```

    Whitespace characters before and after the commas are ignored.

    The first part of the string, fontpattern, should have the form of
    a standard X font name, except that the last two fields should be
    ‘`fontset-alias`’.

    The new fontset has two names, one long and one short. The long name is
    fontpattern in its entirety. The short name is
    ‘`fontset-alias`’. You can refer to the fontset by either
    name. If a fontset with the same name already exists, an error is
    signaled, unless noerror is non-`nil`, in which case this
    function does nothing.

    If optional argument style-variant-p is non-`nil`, that says
    to create bold, italic and bold-italic variants of the fontset as well.
    These variant fontsets do not have a short name, only a long one, which
    is made by altering fontpattern to indicate the bold and/or italic
    status.

    The specification string also says which fonts to use in the fontset.
    See below for the details.

The construct ‘`charset:font`’ specifies which font to
use (in this fontset) for one particular character set. Here,
charset is the name of a character set, and font is the font
to use for that character set. You can use this construct any number of
times in the specification string.

For the remaining character sets, those that you don’t specify
explicitly, Emacs chooses a font based on fontpattern: it replaces
‘`fontset-alias`’ with a value that names one character set.
For the ASCII character set, ‘`fontset-alias`’ is replaced
with ‘`ISO8859-1`’.

In addition, when several consecutive fields are wildcards, Emacs
collapses them into a single wildcard. This is to prevent use of
auto-scaled fonts. Fonts made by scaling larger fonts are not usable
for editing, and scaling a smaller font is not useful because it is
better to use the smaller font in its own size, which Emacs does.

Thus if fontpattern is this,

```
-*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24
```

the font specification for ASCII characters would be this:

```
-*-fixed-medium-r-normal-*-24-*-ISO8859-1
```

and the font specification for Chinese GB2312 characters would be this:

```
-*-fixed-medium-r-normal-*-24-*-gb2312*-*
```

You may not have any Chinese font matching the above font
specification. Most X distributions include only Chinese fonts that
have ‘`song ti`’ or ‘`fangsong ti`’ in the family field. In
such a case, ‘`Fontset-n`’ can be specified as below:

```
Emacs.Fontset-0: -*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24,\
        chinese-gb2312:-*-*-medium-r-normal-*-24-*-gb2312*-*
```

Then, the font specifications for all but Chinese GB2312 characters have
‘`fixed`’ in the family field, and the font specification for
Chinese GB2312 characters has a wild card ‘`*`’ in the family
field.

Function: **set-fontset-font** fontset characters font-spec &optional frame add [¶](#index-set_002dfontset_002dfont)
:   This function modifies the existing fontset to use the font
    specified by font-spec for displaying the specified
    characters.

    If fontset is `nil`, this function modifies the fontset of
    the selected frame or that of frame if frame is not
    `nil`.

    If fontset is `t`, this function modifies the default
    fontset, whose short name as a string is ‘`fontset-default`’.

    The characters argument can be a single character which should
    be displayed using font-spec. It can also be a cons cell
    `(from . to)`, where from and to are
    characters. In that case, use font-spec for all the characters
    in the range from and to (inclusive).

    characters may be a charset symbol (see [Character Sets](#Character-Sets)). In
    that case, use font-spec for all the characters in the charset.

    characters may be a script symbol (see [char-script-table](#Character-Properties)). In that case, use font-spec for all the
    characters belonging to the script. See also
    `use-default-font-for-symbols`, which affects font selection
    when characters specify or belong to the `symbol` script
    (which includes symbol and punctuation characters).

    characters may be `nil`, which means to use font-spec
    for any character in fontset for which no font-spec is
    specified.

    font-spec may be a font-spec object created by the function
    `font-spec` (see [Low-Level Font Representation](#Low_002dLevel-Font)).

    font-spec may be a cons cell `(family . registry)`, where family is a family name of a font
    (possibly including a foundry name at the head), and registry is
    a registry name of a font (possibly including an encoding name at the
    tail).

    font-spec may be a font name, a string.

    font-spec may be `nil`, which explicitly specifies that
    there’s no font for the specified characters. This is useful,
    for example, to avoid expensive system-wide search for fonts for
    characters that have no glyphs, like those from the Unicode Private
    Use Area (PUA).

    The optional argument add, if non-`nil`, specifies how to
    add font-spec to the font specifications previously set for
    characters. If it is `prepend`, font-spec is
    prepended to the existing specs. If it is `append`,
    font-spec is appended. By default, font-spec overwrites
    the previously set font specs.

    For instance, this changes the default fontset to use a font whose
    family name is ‘`Kochi Gothic`’ for all characters belonging to
    the charset `japanese-jisx0208`:

    ```
    (set-fontset-font t 'japanese-jisx0208
                      (font-spec :family "Kochi Gothic"))
    ```

    Note that this function should generally be called from the user’s
    init files, and more generally before any of characters were
    displayed in the current Emacs session. That’s because for some
    scripts, Emacs caches the way they are displayed, and the cached
    information includes the font used for them – once these characters
    are displayed once, the cached font will continue to be used
    regardless of changes in the fontsets.

Function: **char-displayable-p** char [¶](#index-char_002ddisplayable_002dp)
:   This function returns non-`nil` if Emacs ought to be able to
    display char. Or more precisely, if the selected frame’s fontset
    has a font to display the character set that char belongs to.

    Fontsets can specify a font on a per-character basis; when the fontset
    does that, this function’s value may not be accurate.

    This function may return non-`nil` even when there is no font
    available, since it also checks whether the coding system for the text
    terminal can encode the character (see [Terminal I/O Encoding](#Terminal-I_002fO-Encoding)).

Function: **char-displayable-on-frame-p** char frame [¶](#index-char_002ddisplayable_002don_002dframe_002dp)
:   This function behaves like `char-displayable-p` does (relative to
    frame), but in the graphical case, it does not perform the final
    check of whether the underlying text terminal can encode the character.
    It thus provides a displayability check for char more specific to
    frame.

---

Previous: [Fontsets](#Fontsets), Up: [Faces](#Faces)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.12.12 Low-Level Font Representation [¶](#Low_002dLevel-Font-Representation)

Normally, it is not necessary to manipulate fonts directly. In case
you need to do so, this section explains how.

In Emacs Lisp, fonts are represented using three different Lisp
object types: *font objects*, *font specs*, and *font
entities*.

Function: **fontp** object &optional type [¶](#index-fontp)
:   Return `t` if object is a font object, font spec, or font
    entity. Otherwise, return `nil`.

    The optional argument type, if non-`nil`, determines the
    exact type of Lisp object to check for. In that case, type
    should be one of `font-object`, `font-spec`, or
    `font-entity`.

A font object is a Lisp object that represents a font that Emacs has
*opened*. Font objects cannot be modified in Lisp, but they can
be inspected.

Function: **font-at** position &optional window string [¶](#index-font_002dat)
:   Return the font object that is being used to display the character at
    position position in the window window. If window
    is `nil`, it defaults to the selected window. If string is
    `nil`, position specifies a position in the current buffer;
    otherwise, string should be a string, and position
    specifies a position in that string.

A font spec is a Lisp object that contains a set of specifications
that can be used to find a font. More than one font may match the
specifications in a font spec.

Function: **font-spec** &rest arguments [¶](#index-font_002dspec)
:   Return a new font spec using the specifications in arguments,
    which should come in `property`-`value` pairs. The possible
    specifications are as follows:

    `:name`
    :   The font name (a string), in either XLFD, Fontconfig, or GTK+ format.
        See [Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fonts) in The GNU Emacs Manual.

    `:family`

    `:foundry`

    `:weight`

    `:slant`

    `:width`
    :   These have the same meanings as the face attributes of the same name.
        See [Face Attributes](#Face-Attributes). `:family` and `:foundry` are
        strings or symbols, while the other three are either symbols or numbers,
        the numerical values of the valid style symbols. As example values,
        `:slant` may be `italic`, `:weight` may be `bold`
        and `:width` may be `normal`.

    `:size`
    :   The font size—either a non-negative integer that specifies the pixel
        size, or a floating-point number that specifies the point size.

    `:adstyle`
    :   Additional typographic style information for the font, such as
        ‘`sans`’. The value should be a string or a symbol.

    `:registry` [¶](#index-font-registry)
    :   The charset registry and encoding of the font, such as
        ‘`iso8859-1`’. The value should be a string or a symbol.

    `:dpi`
    :   The resolution in dots per inch for which the font is designed. The
        value must be a non-negative number.

    `:spacing`
    :   The spacing of the font: proportional, dual, mono, or charcell. The
        value should be either an integer (0 for proportional, 90 for dual,
        100 for mono, 110 for charcell) or a one-letter symbol (one of
        `P`, `D`, `M`, or `C`).

    `:avgwidth`
    :   The average width of the font in 1/10 pixel units. The value should
        be a non-negative number.

    `:script`
    :   The script that the font must support (a symbol).

    `:lang`
    :   The language that the font should support. The value should be a
        symbol whose name is a two-letter ISO-639 language name. On X, the
        value is matched against the “Additional Style” field of the XLFD
        name of a font, if it is non-empty. On MS-Windows, fonts matching the
        spec are required to support codepages needed for the language.
        Currently, only a small set of CJK languages is supported with this
        property: ‘`ja`’, ‘`ko`’, and ‘`zh`’.

    `:otf` [¶](#index-OpenType-font)
    :   The font must be an OpenType font that supports these OpenType
        features, provided Emacs is compiled with a library, such as
        ‘`libotf`’ on GNU/Linux, that supports complex text layout for
        scripts which need that. The value must be a list of the form

        ```
        (script-tag langsys-tag gsub gpos)
        ```

        where script-tag is the OpenType script tag symbol;
        langsys-tag is the OpenType language system tag symbol, or
        `nil` to use the default language system; `gsub` is a list
        of OpenType GSUB feature tag symbols, or `nil` if none is
        required; and `gpos` is a list of OpenType GPOS feature tag
        symbols, or `nil` if none is required. If `gsub` or
        `gpos` is a list, a `nil` element in that list means that
        the font must not match any of the remaining tag symbols. The
        `gpos` element may be omitted. For the list of OpenType script,
        language, and feature tags, see
        [the list of registered OTF tags](https://docs.microsoft.com/en-us/typography/opentype/spec/ttoreg).

    `:type` [¶](#index-font-backend)
    :   The symbol that specifies the *font backend* used to draw the
        characters. The possible values depend on the platform and on how
        Emacs was configured at build time. Typical values include
        `ftcrhb` and `xfthb` on X, `harfbuzz` on MS-Windows,
        `ns` on GNUstep, etc. It can also be `nil` if left
        unspecified, typically in a font-spec.

Function: **font-put** font-spec property value [¶](#index-font_002dput)
:   Set the font property property in the font-spec font-spec
    to value. The property can any of the ones described
    above.

A font entity is a reference to a font that need not be open. Its
properties are intermediate between a font object and a font spec:
like a font object, and unlike a font spec, it refers to a single,
specific font. Unlike a font object, creating a font entity does not
load the contents of that font into computer memory. Emacs may open
multiple font objects of different sizes from a single font entity
referring to a scalable font.

Function: **find-font** font-spec &optional frame [¶](#index-find_002dfont)
:   This function returns a font entity that best matches the font spec
    font-spec on frame frame. If frame is `nil`,
    it defaults to the selected frame.

Function: **list-fonts** font-spec &optional frame num prefer [¶](#index-list_002dfonts)
:   This function returns a list of all font entities that match the font
    spec font-spec.

    The optional argument frame, if non-`nil`, specifies the
    frame on which the fonts are to be displayed. The optional argument
    num, if non-`nil`, should be an integer that specifies the
    maximum length of the returned list. The optional argument
    prefer, if non-`nil`, should be another font spec, which is
    used to control the order of the returned list; the returned font
    entities are sorted in order of decreasing closeness to that font
    spec.

If you call `set-face-attribute` and pass a font spec, font
entity, or font name string as the value of the `:font`
attribute, Emacs opens the best matching font that is available
for display. It then stores the corresponding font object as the
actual value of the `:font` attribute for that face.

The following functions can be used to obtain information about a
font. For these functions, the font argument can be a font
object, a font entity, or a font spec.

Function: **font-get** font property [¶](#index-font_002dget)
:   This function returns the value of the font property property
    for font. The property can any of the ones that
    `font-spec` supports.

    If font is a font spec and the font spec does not specify
    property, the return value is `nil`. If font is a
    font object or font entity, the value for the :script property
    may be a list of scripts supported by the font, and the value of the
    `:otf` property is a cons of the form `(gsub . gpos)`, where gsub and gpos are lists
    representing OpenType features supported by the font, of the form

    ```
    ((script-tag (langsys-tag feature...) ...) ...)
    ```

    where script-tag, langsys-tag, and feature
    are symbols representing OpenType layout tags.

    If font is a font object, the special property
    `:combining-capability` is non-`nil` if the font backend of
    font supports rendering of combining characters for non-OpenType
    fonts.

Function: **font-face-attributes** font &optional frame [¶](#index-font_002dface_002dattributes)
:   This function returns a list of face attributes corresponding to
    font. The optional argument frame specifies the frame on
    which the font is to be displayed. If it is `nil`, the selected
    frame is used. The return value has the form

    ```
    (:family family :height height :weight weight
       :slant slant :width width)
    ```

    where the values of family, height, weight,
    slant, and width are face attribute values. Some of these
    key-attribute pairs may be omitted from the list if they are not
    specified by font.

Function: **font-xlfd-name** font &optional fold-wildcards long-xlfds [¶](#index-font_002dxlfd_002dname)
:   This function returns the XLFD (X Logical Font Descriptor), a string,
    matching font. See [Fonts](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fonts) in The GNU Emacs Manual, for
    information about XLFDs.

    If the optional argument fold-wildcards is non-`nil`,
    consecutive wildcards in the XLFD are folded into one.

    If the optional argument long-xlfds is omitted or `nil`,
    then the function returns `nil` if the XLFD would exceed 255
    characters in length; this is for compatibility with the X protocol,
    which mandates that XLFDs are restricted to that length. If
    long-xlfds is non-`nil`, this restriction is lifted, and
    the function can return XLFDs of any length.

The following two functions return important information about a font.

Function: **font-info** name &optional frame [¶](#index-font_002dinfo)
:   This function returns information about a font specified by its
    name, a string, as it is used on frame. If frame is
    omitted or `nil`, it defaults to the selected frame.

    The value returned by the function is a vector of the form
    `[opened-name full-name size height
    baseline-offset relative-compose default-ascent
    max-width ascent descent space-width
    average-width filename capability]`. Here’s the
    description of each components of this vector:

    opened-name
    :   The name used to open the font, a string.

    full-name
    :   The full name of the font, a string.

    size
    :   The pixel size of the font.

    height
    :   The height of the font in pixels.

    baseline-offset
    :   The offset in pixels from the ASCII baseline, positive
        upward.

    relative-compose

    default-ascent
    :   Numbers controlling how to compose characters.

    max-width
    :   The maximum advance width of the font.

    ascent

    descent
    :   The ascent and descent of this font. The sum of these two numbers
        should be equal to the value of height above.

    space-width
    :   The width, in pixels, of the font’s space character.

    average-width
    :   The average width of the font characters. Emacs uses this for
        calculating text layout on display; if the value of average-width
        is zero, Emacs uses the value of space-width instead for those
        purposes.
