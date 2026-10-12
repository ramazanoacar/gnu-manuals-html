### 30.23 Accessing Selections [¶](#Accessing-Selections-1)

The data types and selections that `gui-get-selection` and
`gui-set-selection` understand are not precisely specified and
differ subject to the window system on which Emacs is running.

At the same time, `gui-set-selection` abstracts over plenty of
complexity: its data argument is given verbatim to
system-specific code to be rendered suitable for transfer to the
window system or requesting clients.

The most comprehensive implementation of selections exists under the
X Window System. This is both an artifact of history (X was the first
window system supported by Emacs) and one occasioned by technical
considerations: X selections are not merely an expedient for the
transfer of text and multimedia content between clients, but a general
inter-client communication system, a design that has yielded a
proliferation of selection and data types.

Compounding this confusion, there is another inter-client
communication mechanism under X: the Inter-Client Exchange. ICE is
only used by Emacs to communicate with session managers, and is a
separate topic.

* [X Selections](#X-Selections)
* [Other Selections](#Other-Selections)

---

Next: [Other Selections](#Other-Selections), Up: [Accessing Selections](#Accessing-Selections)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.23.1 X Selections [¶](#X-Selections-1)

X refrains from defining fixed data types for selection data or a
fixed number of selections. Selections are identified by X “atoms”,
which are unique 29-bit identifiers issued by the X server for string
names. This complexity is hidden by Emacs: when Lisp provides a
symbol whose name is that of the atom, Emacs will request these
identifiers without further intervention.

When a program “sets” a selection under X, it actually makes
itself the “owner” of the selection—the X server will then deliver
selection requests to the program, which is obliged to respond to the
requesting client with the selection data.

Similarly, a program does not “get” selection data from the X
server. Instead, its selection requests are sent to the client with
the window which last asserted ownership over the selection, which is
expected to respond with the requested data.

Each selection request incorporates three parameters:

* The window which requested the selection, which identifies the
  requesting program, otherwise known as the *requestor*.
* An atom identifying the *target* to which the owner should convert
  the selection. It is easiest to think of the conversion target as the
  kind of data that the requestor wants: in selection requests made by
  Emacs, the target is determined by the type argument to
  `gui-get-selection`.
* A 32-bit timestamp representing the X server time at which the
  requestor last received input; this parameter is not relevant to Lisp
  code, for it’s only meant to abet synchronization between the X
  server, owner and requestor.

The selection owner responds by transferring to the requestor a
series of bytes, 16 bit words, or 32 bit words, along with another
atom identifying the type of those words. After requesting a
selection, Emacs then applies its own interpretation of the data
format and data type to convert the data transferred by the selection
owner to a Lisp representation, which `gui-get-selection`
returns.

Emacs converts selection data consisting of any series of bytes to a
unibyte string holding those bytes, that consisting of a single 16-bit
or 32-bit word as an unsigned number, and that consisting of multiple
such words as a vector of unsigned numbers. The exceptions to this
general pattern are that Emacs applies special treatment for data from
the following conversion targets:

`INTEGER`
:   16-bit or 32-bit words of this type are treated as signed rather than
    unsigned integers. If there are multiple words in the selection data,
    a vector is returned; otherwise, the integer is returned by itself.

`ATOM`
:   32-bit words of this type are treated as X atoms, and returned (either
    alone or as vectors) as Lisp symbols by the names they identify.
    Invalid atoms are replaced by `nil`.

`COMPOUND_TEXT`

`UTF8_STRING`

`STRING`
:   A single `foreign-selection` text property set to the type of the
    selection data will be placed in unibyte strings derived from a
    request for these data types.

Each selection owner must return at least two selection targets:
`TARGETS`, which returns a number of atoms describing the
selection targets that the owner supports, and `MULTIPLE`, used
for internal purposes by X clients. A selection owner may support any
number of other targets, some of which may be standardized by the X
Consortium’s
[Inter-Client Communication Conventions Manual](https://x.org/releases/X11R7.6/doc/xorg-docs/specs/ICCCM/icccm.html), while others, such as
`UTF8_STRING`, were meant to be standardized by the XFree86
Project, but their standardization was never completed.

Requests for a given selection target may, by convention, return
data in a specific type, or it may return data in one of several
types, whichever is most convenient for the selection owner; the
latter type of selection target is dubbed a *polymorphic target*.
In response to a request, a selection target may also return no data
at all, whereafter the selection owner executes some action as a side
effect. Targets that are thus replied to are termed *side-effect
targets*.

Here are some selection targets whose behavior is generally
consistent with a standard when requested from the `CLIPBOARD`,
`PRIMARY`, or `SECONDARY` selections.

`ADOBE_PORTABLE_DOCUMENT_FORMAT`
:   This target returns data in Adobe System’s “Portable Document
    Format” format, as a string.

`APPLE_PICT`
:   This target returns data in the “PICT” image format used on
    Macintosh computers, as a string.

`BACKGROUND`

`BITMAP`

`COLORMAP`

`FOREGROUND`
:   Together, these four targets return integer data necessary to make use
    of a bitmap image stored on the X server: the pixel value of the
    bitmap’s background color, the X identifier of the bitmap, the
    colormap inside which the background and foreground are allocated, and
    the pixel value of the bitmap’s foreground color.

`CHARACTER_POSITION`
:   This target returns two unsigned 32-bit integers of type `SPAN`
    describing the start and end positions of the selection data in the
    text field containing it, in bytes.

`COMPOUND_TEXT`
:   This target returns a string of type `COMPOUND_TEXT` in the X
    Consortium’s multi-byte text encoding system.

`DELETE`
:   This target returns nothing, but as a side-effect deletes the
    selection contents from any text field containing them.

`DRAWABLE`

`PIXMAP`
:   This target returns a list of unsigned 32-bit integers, each of which
    corresponds to an X server drawable or pixmap.

`ENCAPSULATED_POSTSCRIPT`

`_ADOBE_EPS`
:   This target returns a string containing encapsulated Postscript code.

`FILE_NAME`
:   This target returns a string containing one or more file names,
    separated by NULL characters.

`HOST_NAME`
:   This target returns a string containing the fully-qualified domain
    name of the machine on which the selection owner is running.

`USER`
:   This target returns a string containing the user name of the machine
    on which the selection owner is running.

`LENGTH`
:   This target returns an unsigned 32-bit or 16-bit integer containing
    the length of the selection data.

`LINE_NUMBER`
:   This target returns two unsigned 32-bit integers of type `SPAN`
    describing the line numbers corresponding to the start and end
    positions of the selection data in the text field containing it.

`MODULE`
:   This target returns the name of any function containing the selection
    data. It is principally requested by text editors.

`STRING`
:   This target returns the selection data as a string of type
    `STRING`, encoded in ISO Latin-1 format, with Unix newline
    characters.

`C_STRING`
:   This target returns the selection data as a “C string”. This has
    been interpreted as meaning the raw selection data in whatever
    encoding used by the owner, either terminated with a NULL byte or not
    at all, or an ASCII string which may or may not be terminated.

`UTF8_STRING`
:   This returns the selection data as a string of type
    `UTF8_STRING`, encoded in UTF-8, with unspecified EOL format.

`TIMESTAMP`
:   This target returns the X server time at which the selection owner
    took ownership over the selection as a 16-bit or 32-bit word of type
    `CARDINAL`.

`TEXT`
:   This polymorphic target returns selection data as a string, either
    `COMPOUND_TEXT`, `STRING`, `C_STRING`, or
    `UTF8_STRING`, whichever data type is convenient for the
    selection owner.

When a request for the targets `STRING`, `COMPOUND_TEXT`,
or `UTF8_STRING` is made using the function
`gui-get-selection`, and neither `selection-coding-system`
nor `next-selection-coding-system` is set, the resultant strings
are decoded by the proper coding systems for those targets:
`iso-8859-1`, `compound-text-with-extensions` and
`utf-8` respectively.

In addition to the targets specified above (and the many targets
used by various programs for their own purposes), several popular
programs and toolkits have defined selection data types of their own,
without consulting the appropriate X standards bodies. These targets
are generally named after such MIME types as `text/html` or
`image/jpeg`; they have been witnessed returning the following
forms of data:

* Unterminated, newline terminated, or NULL character terminated file
  names of an image or text file.
* Image or text data in the appropriate format.
* `file://` URIs (or conceivably newline or NUL terminated lists of
  URIs) identifying files in the appropriate format.

These selection targets were first used by Netscape, but are now
proffered by all kinds of programs, especially those based on recent
versions of the GTK+ or Qt toolkits.

Emacs is also capable of serving as a selection owner. When
`gui-set-selection` is called, the selection data provided is
recorded internally and Emacs obtains ownership of the selection being
set.

Variable: **selection-converter-alist** [¶](#index-selection_002dconverter_002dalist)
:   Alist of selection targets to “selection converter” functions. When
    a selection request is received, Emacs looks up the selection
    converter pertaining to the requested selection target.

    Selection converters are called with three arguments: the symbol
    corresponding to the atom identifying the selection being requested,
    the selection target that is being requested, and the value set with
    `gui-set-selection`. The values which they must return are
    either conses of symbols designating the data type and numbers,
    symbols, vectors of numbers or symbols, or the cdrs of such conses by
    themselves.

    If a selection converter’s value is the special symbol `NULL`,
    the data type returned to its requestor is set to `NULL`, and no
    data is sent in response.

    If such a value is a string, it must be a unibyte string; should no
    data type be explicitly specified, the data is transferred to its
    requestor with the type `STRING`.

    If it is a symbol, its “atom” is retrieved, and it is transferred to
    its requestor as a 32-bit value—if no data type is specified, its
    type is `ATOM`.

    If it is a number between `-32769` and `32768`, it is
    transferred to its requestor as a 16 bit value—if no data type is
    specified, its type is `INTEGER`.

    If it is any other number, it is accounted a 32 bit value. Even if
    the number returned is unsigned, its requestor will treat words of
    type `INTEGER` as signed. To return an unsigned value, specify
    the type `CARDINAL` in its place.

    If it is a vector of symbols or numbers, the response to its requestor
    will be a list of multiple atoms or numbers. The data type returned
    when not expressly set is that of the list’s first element.

By default, Emacs is configured with selection converters for the
following selection targets:

`TEXT`
:   This selection converter returns selection data as:

    * A string of type `C_STRING`, if the selection contents contain no
      multibyte characters, or contain “raw 8-bit bytes” (see [Text Representations](#Text-Representations)).
    * A string of type `STRING`, if the selection contents can be
      represented as ISO-Latin-1 text.
    * A string of type `COMPOUND_TEXT`, if the selection contents can
      be encoded in the X Consortium’s Compound Text Encoding, and
      `selection-coding-system` or `next-selection-coding-system`
      is set to a coding system whose `:mime-charset` property is
      `x-ctext`.
    * A string of type `UTF8_STRING` otherwise.

`COMPOUND_TEXT`
:   This selection converter returns selection data as a string of type
    `COMPOUND_TEXT`.

`STRING`
:   This selection converter returns selection data as a string of type
    `STRING`, encoded in ISO-Latin-1 format.

`UTF8_STRING`
:   This selection converter returns selection data in UTF-8 format.

`text/plain`

`text/plain;charset=utf-8`

`text/uri-list`

`text/x-xdnd-username`

`XmTRANSFER_SUCCESS`

`XmTRANSFER_FAILURE`

`FILE`

`_DT_NETFILE`
:   These selection converters are used for internal purposes during
    drag-and-drop operations and are not available for selections other
    than `XdndSelection`.

`TARGETS`
:   This selection converter returns a list of atoms, one for each
    selection target understood by Emacs.

`MULTIPLE`
:   This selection converter is implemented in C code and is used to
    implement efficient transfer of selection requests which specify
    multiple selection targets at the same time.

`LENGTH`
:   This selection converter returns the length of the selection data, in
    bytes.

`DELETE`
:   This selection converter is used for internal purposes during
    drag-and-drop operations.

`FILE_NAME`
:   This selection converter returns the file name of the buffer
    containing the selection data.

`CHARACTER_POSITION`
:   This selection converter returns the character positions of each end
    of the selection in the buffer containing the selection data.

`LINE_NUMBER`

`COLUMN_NUMBER`
:   This selection converter returns the line and column numbers of each
    end of the selection in the buffer containing the selection data.

`OWNER_OS`
:   This selection converter returns the name of the operating system on
    which Emacs is running.

`HOST_NAME`
:   This selection converter returns the fully-qualified domain name of
    the machine on which Emacs is running.

`USER`
:   This selection converter returns the username of the user account
    under which Emacs is running.

`CLASS`

`NAME`
:   These selection converters return the resource class and name used by
    Emacs.

`INTEGER`
:   This selection converter returns an integer value verbatim.

`SAVE_TARGETS`

`_EMACS_INTERNAL`
:   These selection converters are used for internal purposes.

With the exception of `INTEGER`, all selection converters
expect the data provided to `gui-set-selection` to be one of the
following:

* A string.
* A list of the form `(beg end buf)`, where
  beg and end are two markers or overlays describing the
  bounds of the selection data in the buffer buf.

---

Previous: [X Selections](#X-Selections), Up: [Accessing Selections](#Accessing-Selections)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 30.23.2 Other Selections [¶](#Other-Selections-1)

Selections under such window systems as MS-Windows, Nextstep, Haiku
and Android are not aligned with those under X. Each of these window
system improvises its own selection mechanism without employing the
“selection converter” mechanism illustrated in the preceding node.
Only the `PRIMARY`, `CLIPBOARD`, and `SECONDARY`
selections are generally supported, with the `XdndSelection`
selection that records drag-and-drop data also available under
Nextstep and Haiku.

GTK seeks to emulate the X selection system, but its emulations are
not altogether dependable, with the overall quality of each subject to
the GDK backend being used. Therefore, Emacs built with PGTK will
supply the same selection interface as Emacs built with X, but many
selection targets will not be useful.

Although a clipboard exists, there is no concept of primary or
secondary selections within the MS-Windows operating system. On this
system, Emacs simulates the presence of a primary and secondary
selection, while saving to and retrieving from the clipboard when so
requested.

The simulation of the primary and secondary selections is conducted
by saving values supplied to `gui-set-selection` within the
`x-selections` property of the symbol designating the pertinent
selection, namely the type argument to `gui-get-selection`.
Each subsequent call to `gui-get-selection` in turn returns its
value, which is not subject to further examination (such as type
checks and the like). Under such circumstances, data-type
argument is generally disregarded. (But see below for the
qualification regarding `TARGETS`.)

Where the clipboard selection is concerned (whenever type is
`CLIPBOARD`), `gui-set-selection` verifies that the value
provided is a string and saves it within the system clipboard once it
is encoded by the coding system configured in
`selection-coding-system`. Callers of `gui-get-selection`
are required to set data-type to either `STRING` or
`TARGETS`.

When data-type is set to `TARGETS` in a call to
`gui-get-selection`, a vector of symbols is returned when
selection data exists, much as it is under X. It is impossible to
request clipboard data in any format besides `STRING`, for the
prerequisite data conversion routines are absent. Just as strings
saved into the clipboard are encoded by the
`selection-coding-system`, so those read from the clipboard are
decoded by that same coding system; this variable and its cousin
`next-selection-coding-system` merit particular scrutiny when
difficulties are encountered with saving selection text into the
clipboard.

All three selections standard in X exist in Nextstep as well, but
Emacs is only capable of saving strings to such selections.
Restrictions imposed upon calls to `gui-set-selection` there are
much the same as those on MS-Windows, though text is uniformly encoded
as `utf-8-unix` without regard to the value of
`selection-coding-system`. `gui-get-selection` is more
charitable, and accepts requests for the following selection targets:

* text/plain
* image/png
* text/html
* application/pdf
* application/rtf
* application/rtfd
* STRING
* text/plain
* image/tiff

The `XdndSelection` selection is also present under Nextstep,
in the form of a repository that records values supplied to
`gui-set-selection`. Its sole purpose is to save such values for
the fundamental drag-and-drop function `x-begin-drag`
(see [Drag and Drop](#Drag-and-Drop)); no guarantees exist concerning its value when
read by anything else.

Selections on Haiku systems comprise all three selections customary
under X and the `XdndSelection` that records drag-and-drop data.

When `gui-set-selection` is called for the former three
selections, the data supplied is converted into a window server
“message” by a list of *selection encoder* functions, which is
sent to the window server.

Variable: **haiku-normal-selection-encoders** [¶](#index-haiku_002dnormal_002dselection_002dencoders)
:   List of selection encoder functions. When `gui-set-selection` is
    called, each function in this list is successively called with its
    selection and value arguments. If such a function returns
    non-`nil`, its return value must be a list of the form
    `(key type value)`. In this list,
    key must be the name of the data being transferred, generally
    that of a MIME type, for example ‘`"text/plain"`’, and type
    is a symbol or a number designating the type of the data; thus also
    governing the interpretation of value; following is a list of
    valid data types and how each of them will cause value to be
    interpreted.

    `string`
    :   A unibyte string. The string is NULL-terminated after being placed in
        the message.

    `ref`
    :   A file name. The file is located and the inode identifying the file
        is placed in the message.

    `short`
    :   A 16-bit integer value.

    `long`
    :   A 32-bit integer value.

    `llong`
    :   A 64-bit integer value.

    `byte`

    `char`
    :   An unsigned byte between 0 and 255.

    `size_t`
    :   A number between 0 and 1 minus two to the power of the word size of
        the computer Emacs is running on.

    `ssize_t`
    :   A number which fits in the C type `ssize_t`.

    `point`
    :   A cons of two floats, specifying a coordinate on-screen.

    `float`

    `double`
    :   A single or double-precision floating point number in an unspecified
        format.

    `(haiku-numeric-enum MIME)`
    :   A unibyte string containing data in a certain MIME type.

A call to `gui-get-selection` generally returns the data
named data-type within the selection message, albeit with
data-type replaced by an alternative name should it be one of
the following X selection targets:

`STRING`
:   This represents Latin-1 text under X: “text/plain;charset=iso-8859-1”

`UTF8_STRING`
:   This represents UTF-8 text: “text/plain”

If data-type is a text type such as `STRING` or a MIME
type matching the pattern ‘`` `text/* ``’, the string data is decoded
with the coding system apposite for it before being returned.

Furthermore, the two data types TIMESTAMP and `TARGETS`
are afforded special treatment; the value returned for the first is
the number of times the selection has been modified since system
startup (*not* a timestamp), and that for the other is a vector
of available selection data types, as elsewhere.

Much like MS-Windows, Android provides a clipboard but no primary or
secondary selection; `gui-set-selection` simulates the primary
and secondary selections by saving the value supplied into a variable
subsequent calls to `gui-get-selection` return.

From the clipboard, `gui-get-selection` is capable of returning
UTF-8 string data of the type `STRING`, the `TARGETS` data
type, or image and application data of any MIME type.
`gui-set-selection` sets only string data, much as under
MS-Windows, although this data is not affected by the value of
`selection-coding-system`. By contrast, only string data can be
saved to and from the primary and secondary selections; but since this
data is not communicated to programs besides Emacs, it is not subject
to encoding or decoding by any coding system.

---

Next: [Drag and Drop](#Drag-and-Drop), Previous: [Accessing Selections](#Accessing-Selections), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.24 Yanking Media [¶](#Yanking-Media-1)

Data saved within window system selections and the MS-Windows
clipboard is not restricted to plain text. It is possible for selection
data to encompass images or other binary data of the like, as well as
rich text content instanced by HTML, and also PostScript. Since the
selection data types incident to this data are at variance with those
for plain text, the insertion of such data is facilitated by a set of
functions dubbed *yank-media handlers*, which are registered by each
major mode undertaking its insertion and called where warranted upon the
execution of the `yank-media` command.

Function: **yank-media-handler** types handler [¶](#index-yank_002dmedia_002dhandler)
:   Register a yank-media handler which applies to the current buffer.

    types can be a symbol designating a selection data type
    (see [Accessing Selections](#Accessing-Selections)), a regexp against which such types are
    matched, or a list of these symbols and regexps. For instance:

    ```
    (yank-media-handler 'text/html #'my-html-handler)
    (yank-media-handler "image/.*" #'my-image-handler)
    ```

    When a selection offers a data type matching types, the function
    handler is called to insert its data, with the symbol
    designating the matching selection data type, and the data returned by
    `gui-get-selection`.

The `yank-media` command auto selects the preferred MIME type
by default. The rules used for the selection can be controlled through
the variables `yank-media-autoselect-function` and
`yank-media-preferred-types`.

Variable: **yank-media-autoselect-function** [¶](#index-yank_002dmedia_002dautoselect_002dfunction)
:   This variable should specify a function that will be called with the
    list of MIME types available for the current major mode, and should
    return a list of preferred MIME types to use. The first MIME
    type in the list will always be used by the `yank-media` command
    when auto selection is requested.

Variable: **yank-media-preferred-types** [¶](#index-yank_002dmedia_002dpreferred_002dtypes)
:   This variable changes the default selection process of
    `yank-media-autoselect-function`. It is a list that should contain
    the sole MIME type to choose in the order of their preference. It
    can also contain a function in which case it is called with the list of
    available MIME types and must return a list of preferred MIME
    types in order of their preference. This list is passed onto the
    `yank-media` command so the first element of the returned list is
    chosen when auto selection is requested.

The `yank-media-types` command presents a list of selection data
types that are currently available, which is useful when implementing
yank-media handlers; for programs generally offer an eclectic and
seldom consistent medley of data types.

---

Next: [Color Names](#Color-Names), Previous: [Yanking Media](#Yanking-Media), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.25 Drag and Drop [¶](#Drag-and-Drop-1)

Data transferred by drag and drop is generally either plain text or
a list of URLs designating files or other resources. When text is
dropped, it is inserted at the location of the drop, with recourse to
saving it into the kill ring if that is not possible.

URLs dropped are supplied to pertinent *DND handler functions*
in the variable `dnd-protocol-alist`, or alternatively “URL
handlers” as set forth by the variables `browse-url-handlers`
and `browse-url-default-handlers`; absent matching handlers of
either type, they are treated as plain text and inserted in the
buffer.

Variable: **dnd-protocol-alist** [¶](#index-dnd_002dprotocol_002dalist)
:   This variable is an alist between regexps against which URLs are
    matched and DND handler functions called on the dropping of matching
    URLs.

    If a handler function is a symbol whose `dnd-multiple-handler`
    property (see [Symbol Properties](#Symbol-Properties)) is set, then upon a drop it is
    given a list of every URL that matches its regexp; absent this
    property, it is called once for each of those URLs. Following this
    first argument is one of the symbols `copy`, `move`,
    `link`, `private` or `ask` identifying the action to be
    taken.

    If action is `private`, the program that initiated the drop
    does not insist on any particular behavior on the part of its
    recipient; a reasonable action to take in that case is to open the URL
    or copy its contents into the current buffer. The other values of
    action imply much the same as in the action argument to
    `dnd-begin-file-drag`.

    Once its work completes, a handler function must return a symbol
    designating the action it took: either the action it was provided, or
    the symbol `private`, which communicates to the source of the
    drop that the action it prescribed has not been executed.

    When multiple handlers match an overlapping subset of items within a
    drop, the handler matched against by the greatest number of items is
    called to open that subset. The items it is supplied are subsequently
    withheld from other handlers, even those they also match.

Emacs does not take measures to accept data besides text and URLs,
for the window system interfaces which enable this are too far removed
from each other to abstract over consistently. Nor are DND handlers
accorded influence over the actions they are meant to take, as
particular drag-and-drop protocols deny recipients such control. The
X11 drag-and-drop implementation rests on several underlying protocols
that make use of selection transfer and share much in common, to which
low level access is provided through the following functions and
variables:

Variable: **x-dnd-test-function** [¶](#index-x_002ddnd_002dtest_002dfunction)
:   This function is called to ascertain whether Emacs should accept a
    drop. It is called with three arguments:

    * The window under the item being dragged, which is to say the window
      whose buffer is to receive the drop. If the item is situated over a
      non-window component of a frame (such as scroll bars, tool bars and
      things to that effect), the frame itself is provided in its place.
    * One of the symbols `move`, `copy`, `link` or
      `ask`, representing an action to take on the item data suggested
      by the drop source. These symbols carry the same implications as in
      `x-begin-drag`.
    * A vector of selection data types (see [X Selections](#X-Selections)) the item
      provides.

    This function must return `nil` to reject the drop or a cons of
    the action that will be taken (such as through transfer to a DND
    handler function) and the selection data type to be requested. The
    action returned in that cons may also be the symbol `private`,
    which intimates that the action taken is as yet indeterminate.

Variable: **x-dnd-known-types** [¶](#index-x_002ddnd_002dknown_002dtypes)
:   Modifying `x-dnd-test-function` is generally unwarranted, for its
    default set of criteria for accepting a drop can be adjusted by
    changing this list of selection data types. Each element is a string,
    which if found as the symbol name of an element within the list of
    data types by the default “test function”, will induce that function
    to accept the drop.

    Introducing a new entry into this list is not useful unless a
    counterpart handler function is appended to `x-dnd-types-alist`.

Variable: **x-dnd-types-alist** [¶](#index-x_002ddnd_002dtypes_002dalist)
:   This variable is an alist between strings designating selection data
    types and functions which are called when things of such types are
    dropped.

    Each such function is supplied three arguments; the first is the
    window or frame below the location of the drop, as in
    `x-dnd-test-function`; the second is the action to be taken,
    which may be any of the actions returned by test functions, and third
    is the selection data itself (see [Accessing Selections](#Accessing-Selections)).

Selection data types as provided by X11 drag-and-drop protocols are
sometimes distinct from those provided by the ICCCM and conforming
clipboard or primary selection owners. Frequently, the name of a MIME
type, such as `"text/plain;charset=utf-8"` (with discrepant
capitalization of the “utf-8”), is substituted for a standard X
selection name such as `UTF8_STRING`.

The X Direct Save (XDS) protocol enables programs to
devolve responsibility for naming a dropped file upon the recipient.
When such a drop transpires, DND handlers and the foregoing X-specific
interface are largely circumvented, tasking a different function with
responding to the drop.

Variable: **x-dnd-direct-save-function** [¶](#index-x_002ddnd_002ddirect_002dsave_002dfunction-1)
:   This variable should be set to a function that registers and names
    files dropped using the XDS protocol in a two-step
    procedure. It is provided two arguments, need-name and
    filename.

    1. The application from which the file is dragged asks Emacs to provide
       the full file name under which to save the file. For this purpose,
       the direct-save function is called with its first argument
       need-name non-`nil`, and the second argument filename
       set to the basename of the file to be saved. It should return the
       fully-expanded absolute file name under which to save the file. For
       example, if a file is dragged to a Dired window, the natural directory
       for the file is the directory of the file shown at location of the
       drop. If saving the file is not possible for some reason, the
       function should return `nil`, which will cancel the drag-and-drop
       operation.
    2. The application from which the file is dragged saves the file under
       the name returned by the first call to the direct-save function. If
       it succeeds in saving the file, the direct-save function is called
       again, this time with the first argument need-name set to
       `nil` and the second argument filename set to the full
       absolute name of the saved file. The function is then expected to do
       whatever is needed given the fact that file was saved. For example,
       Dired should update the directory on display by showing the new file
       there.

    Its default `x-dnd-direct-save-function` is
    `x-dnd-save-direct`.

Function: **x-dnd-save-direct** need-name filename [¶](#index-x_002ddnd_002dsave_002ddirect)
:   When called with the need-name argument non-`nil`, this
    function prompts the user for the absolute file name under which it
    should be saved. If the specified file already exists, it
    additionally asks the user whether to overwrite it, and returns the
    absolute file name only if the user confirms the overwriting.

    When called with the need-name argument `nil`, it reverts
    the Dired listing if the current buffer is in Dired mode or one of its
    descendants, and otherwise visits the file by calling `find-file`
    (see [Functions for Visiting Files](#Visiting-Functions)).

Function: **x-dnd-save-direct-immediately** need-name filename [¶](#index-x_002ddnd_002dsave_002ddirect_002dimmediately)
:   This function works like `x-dnd-save-direct`, but when called
    with its need-name argument non-`nil`, it doesn’t prompt
    the user for the full name of the file to be saved; instead, it
    returns its argument filename expanded against the current
    buffer’s default directory (see [Functions that Expand Filenames](#File-Name-Expansion)). (It still
    asks for confirmation if a file by that name already exists in the
    default directory.)

It is also possible to drag content from Emacs to other programs
when this is supported by the current window-system. The functions
which provide for this are as follows:

Function: **dnd-begin-text-drag** text &optional frame action allow-same-frame [¶](#index-dnd_002dbegin_002dtext_002ddrag)
:   This function starts a drag-and-drop operation from frame to
    another program (dubbed the *drop target*), and returns when
    text is dropped or the operation is canceled.

    action must be one of the symbols `copy` or `move`,
    where `copy` means that text should be inserted by the drop
    target, and `move` means the same as `copy`, but the caller
    must also delete text from its source as explained in the list
    below.

    frame is the frame where the mouse is currently held down, or
    `nil`, which means to use the selected frame. Since this
    function might return promptly if no mouse buttons are held down, it
    should be only called in response to a `down-mouse-1` or
    analogous event (see [Mouse Events](#Mouse-Events)), with frame set to the
    frame where that event was generated (see [Click Events](#Click-Events)).

    If allow-same-frame is `nil`, drops on top of frame
    will be ignored.

    The return value reflects the action that the drop target actually
    performed, and thus also what action, if any, the caller should in
    turn take. It is one of the following symbols:

    `copy`
    :   The drop target inserted the dropped text.

    `move`
    :   The drop target inserted the dropped text, and the caller should
        delete text from the buffer where it was extracted from, if
        applicable.

    `private`
    :   The drop target took some other unspecified action.

    `nil`
    :   The drag-and-drop operation was canceled.

Function: **dnd-begin-file-drag** file &optional frame action allow-same-frame [¶](#index-dnd_002dbegin_002dfile_002ddrag)
:   This function starts a drag-and-drop operation from frame to
    another program (dubbed the *drop target*), and returns when
    file is dropped or the operation is canceled.

    If file is a remote file, then a temporary local copy will be
    made.

    action must be one of the symbols `copy`, `move` or
    `link`, where `copy` means that file should be opened
    or copied by the drop target, `move` means the drop target should
    move the file to another location, and `link` means the drop
    target should create a symbolic link to file. It is an error to
    specify `link` as the action if file is a remote file.

    frame and allow-same-frame mean the same as they do in
    calls to `dnd-begin-text-drag`.

    The return value is the action that the drop target actually
    performed, which is one of the following symbols:

    `copy`
    :   The drop target opened or copied file to a different location.

    `move`
    :   The drop target moved file to a different location.

    `link`
    :   The drop target (usually a file manager) created a symbolic link to
        file.

    `private`
    :   The drop target performed some other unspecified action.

    `nil`
    :   The drag-and-drop operation was canceled.

Function: **dnd-begin-drag-files** files &optional frame action allow-same-frame [¶](#index-dnd_002dbegin_002ddrag_002dfiles)
:   This function is like `dnd-begin-file-drag`, except that
    files is a list of files. If the drop target doesn’t support
    dropping multiple files, then the first file will be used instead.

Function: **dnd-direct-save** file name &optional frame allow-same-frame [¶](#index-dnd_002ddirect_002dsave)
:   The behavior of this function is akin to that of
    `dnd-begin-file-drag` (when the default action `copy` is
    used), except that it accepts a name under which the copy is meant to
    be filed.

The high-level interfaces described above are implemented on top of
a lower-level primitive. The low-level interface `x-begin-drag`
is also available for dragging content besides text and files. It
demands detailed knowledge of the data types and actions understood by
programs on each platform its callers wish to support.

Function: **x-begin-drag** targets &optional action frame return-frame allow-current-frame follow-tooltip [¶](#index-x_002dbegin_002ddrag)
:   This function begins a drag from frame, and returns when the
    drag-and-drop operation ends, either because the drop was successful,
    or because the drop was rejected. The drop occurs when all mouse
    buttons are released on top of an X window other than frame (the
    *drop target*), or any X window if allow-current-frame is
    non-`nil`. If no mouse buttons are held down when the
    drag-and-drop operation begins, this function may immediately return
    `nil`.

    targets is a list of strings representing selection targets,
    much like the data-type argument to `gui-get-selection`,
    that the drop target can request from Emacs (see [Window System Selections](#Window-System-Selections)).

    action is a symbol designating the action recommended to the
    target. It can either be `XdndActionCopy` or
    `XdndActionMove`; both imply copying the contents of the
    selection `XdndSelection` to the drop target, but the latter
    moreover conveys a promise to delete the contents of the selection
    after the copying.

    action may also be an alist which associates between symbols
    representing available actions, and strings that the drop target
    presents to the user for him to select between those actions.

    If return-frame is non-`nil` and the mouse moves over an
    Emacs frame after first moving out of frame, then the frame to
    which the mouse moves will be returned immediately. If
    return-frame is the symbol `now`, then any frame beneath
    the mouse pointer will be returned without waiting for the mouse to
    first move out of frame. return-frame is useful when you
    want to treat dragging content from one frame to another specially,
    while also dragging content to other programs, but it is not
    guaranteed to function on all systems and with all window managers.

    If follow-tooltip is non-`nil`, the position of any tooltip
    (such as one displayed by `tooltip-show`) will follow the
    location of the mouse pointer as it moves during the drag-and-drop
    operation. The tooltip will be hidden once all mouse buttons are
    released.

    If the drop was rejected or no drop target was found, this function
    returns `nil`. Otherwise, it returns a symbol representing the
    action the target opted to take, which can differ from action if
    that isn’t supported by the drop target. `XdndActionPrivate` is
    also a valid return value in addition to `XdndActionCopy` and
    `XdndActionMove`; it suggests that the drop target opted for an
    indeterminate action, and no further action is required of the caller.

    The caller must cooperate with the target to complete the action
    selected by the target. For example, callers should delete any buffer
    text that was dragged if this function returns `XdndActionMove`,
    and likewise for other drag data where comparable criteria apply.

The function `x-begin-drag` leverages several drag-and-drop
protocols “behind the scenes”. When dragging content that is known
to not be supported by a specific drag-and-drop protocol, that
protocol can be disabled by changing the values of the following
variables:

Variable: **x-dnd-disable-motif-protocol** [¶](#index-x_002ddnd_002ddisable_002dmotif_002dprotocol)
:   When this is non-`nil`, the Motif drag and drop protocols are
    disabled, and dropping onto programs that only understand them will
    not work.

Variable: **x-dnd-use-offix-drop** [¶](#index-x_002ddnd_002duse_002doffix_002ddrop)
:   When this is `nil`, the OffiX (old KDE) drag and drop protocol is
    disabled. When this is the symbol `files`, the OffiX protocol
    will only be used if `"FILE_NAME"` is one of the targets given to
    `x-begin-drag`. Any other value means to use the OffiX protocol
    to drop all supported content.

Variable: **x-dnd-use-unsupported-drop** [¶](#index-x_002ddnd_002duse_002dunsupported_002ddrop)
:   When one of the `"STRING"`, `"UTF8_STRING"`,
    `"COMPOUND_TEXT"` or `"TEXT"` targets is present in the list
    given to `x-begin-drag`, Emacs will try to use synthesized mouse
    events and the primary selection to insert the text if the drop target
    doesn’t support any drag-and-drop protocol at all.

    A side effect is that Emacs will become the owner of the primary
    selection upon such a drop. Such emulation can be disabled by setting
    this variable to `nil`.

---

Next: [Text Terminal Colors](#Text-Terminal-Colors), Previous: [Drag and Drop](#Drag-and-Drop), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.26 Color Names [¶](#Color-Names-1)

A color name is text (usually in a string) that specifies a color.
Symbolic names such as ‘`black`’, ‘`white`’, ‘`red`’, etc.,
are allowed; use `M-x list-colors-display` to see a list of
defined names. You can also specify colors numerically in forms such
as ‘`#rgb`’ and ‘`RGB:r/g/b`’, where
r specifies the red level, g specifies the green level,
and b specifies the blue level. You can use either one, two,
three, or four hex digits for r; then you must use the same
number of hex digits for all g and b as well, making
either 3, 6, 9 or 12 hex digits in all. (See the documentation of the
X Window System for more details about numerical RGB specification of
colors.)

These functions provide a way to determine which color names are
valid, and what they look like. In some cases, the value depends on the
*selected frame*, as described below; see [Input Focus](#Input-Focus), for the
meaning of the term “selected frame”.

To read user input of color names with completion, use
`read-color` (see [read-color](#High_002dLevel-Completion)).

Function: **color-defined-p** color &optional frame [¶](#index-color_002ddefined_002dp)
:   This function reports whether a color name is meaningful. It returns
    `t` if so; otherwise, `nil`. The argument frame says
    which frame’s display to ask about; if frame is omitted or
    `nil`, the selected frame is used.

    Note that this does not tell you whether the display you are using
    really supports that color. When using X, you can ask for any defined
    color on any kind of display, and you will get some result—typically,
    the closest it can do. To determine whether a frame can really display
    a certain color, use `color-supported-p` (see below).

Function: **defined-colors** &optional frame [¶](#index-defined_002dcolors)
:   This function returns a list of the color names that are defined
    and supported on frame frame (default, the selected frame).
    If frame does not support colors, the value is `nil`.

Function: **color-supported-p** color &optional frame background-p [¶](#index-color_002dsupported_002dp)
:   This returns `t` if frame can really display the color
    color (or at least something close to it). If frame is
    omitted or `nil`, the question applies to the selected frame.

    Some terminals support a different set of colors for foreground and
    background. If background-p is non-`nil`, that means you are
    asking whether color can be used as a background; otherwise you
    are asking whether it can be used as a foreground.

    The argument color must be a valid color name.

Function: **color-gray-p** color &optional frame [¶](#index-color_002dgray_002dp)
:   This returns `t` if color is a shade of gray, as defined on
    frame’s display. If frame is omitted or `nil`, the
    question applies to the selected frame. If color is not a valid
    color name, this function returns `nil`.

Function: **color-values** color &optional frame [¶](#index-color_002dvalues)
:   This function returns a value that describes what color should
    ideally look like on frame. If color is defined, the
    value is a list of three integers, which give the amount of red, the
    amount of green, and the amount of blue. Each integer ranges in
    principle from 0 to 65535, but some displays may not use the full
    range. This three-element list is called the *rgb values* of the
    color.

    If color is not defined, the value is `nil`.

    ```
    (color-values "black")
         ⇒ (0 0 0)
    (color-values "white")
         ⇒ (65535 65535 65535)
    (color-values "red")
         ⇒ (65535 0 0)
    (color-values "pink")
         ⇒ (65535 49344 52171)
    (color-values "hungry")
         ⇒ nil
    ```

    The color values are returned for frame’s display. If
    frame is omitted or `nil`, the information is returned for
    the selected frame’s display. If the frame cannot display colors, the
    value is `nil`.

Function: **color-name-to-rgb** color &optional frame [¶](#index-color_002dname_002dto_002drgb)
:   This function does the same as `color-values`, but it returns
    color values as floating-point numbers between 0.0 and 1.0 inclusive.

Function: **color-dark-p** rgb [¶](#index-color_002ddark_002dp)
:   This function returns non-`nil` if the color described by its RGB
    triplet rgb is more readable against white background than
    against dark background. The argument rgb should be a list of
    the form `(r g b)`, with each component a
    floating-point number in the range 0.0 to 1.0 inclusive. You can use
    `color-name-to-rgb` to convert a color’s name to such a list.

---

Next: [X Resources](#Resources), Previous: [Color Names](#Color-Names), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.27 Text Terminal Colors [¶](#Text-Terminal-Colors-1)

Text terminals usually support only a small number of colors, and
the computer uses small integers to select colors on the terminal.
This means that the computer cannot reliably tell what the selected
color looks like; instead, you have to inform your application which
small integers correspond to which colors. However, Emacs does know
the standard set of colors and will try to use them automatically.

The functions described in this section control how terminal colors
are used by Emacs.

Several of these functions use or return *rgb values*, described
in [Color Names](#Color-Names).

These functions accept a display (either a frame or the name of a
terminal) as an optional argument. We hope in the future to make
Emacs support different colors on different text terminals; then this
argument will specify which terminal to operate on (the default being
the selected frame’s terminal; see [Input Focus](#Input-Focus)). At present,
though, the frame argument has no effect.

Function: **tty-color-define** name number &optional rgb frame [¶](#index-tty_002dcolor_002ddefine)
:   This function associates the color name name with
    color number number on the terminal.

    The optional argument rgb, if specified, is an rgb value, a list
    of three numbers that specify what the color actually looks like.
    If you do not specify rgb, then this color cannot be used by
    `tty-color-approximate` to approximate other colors, because
    Emacs will not know what it looks like.

Function: **tty-color-clear** &optional frame [¶](#index-tty_002dcolor_002dclear)
:   This function clears the table of defined colors for a text terminal.

Function: **tty-color-alist** &optional frame [¶](#index-tty_002dcolor_002dalist)
:   This function returns an alist recording the known colors supported by
    a text terminal.

    Each element has the form `(name number . rgb)`
    or `(name number)`. Here, name is the color
    name, number is the number used to specify it to the terminal.
    If present, rgb is a list of three color values (for red, green,
    and blue) that says what the color actually looks like.

Function: **tty-color-approximate** rgb &optional frame [¶](#index-tty_002dcolor_002dapproximate)
:   This function finds the closest color, among the known colors
    supported for display, to that described by the rgb value
    rgb (a list of color values). The return value is an element of
    `tty-color-alist`.

Function: **tty-color-translate** color &optional frame [¶](#index-tty_002dcolor_002dtranslate)
:   This function finds the closest color to color among the known
    colors supported for display and returns its index (an integer).
    If the name color is not defined, the value is `nil`.

---

Next: [Display Feature Testing](#Display-Feature-Testing), Previous: [Text Terminal Colors](#Text-Terminal-Colors), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.28 X Resources [¶](#X-Resources)

This section describes some of the functions and variables for
querying and using X resources, or their equivalent on your operating
system. See [X Resources](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#X-Resources) in The GNU Emacs
Manual, for more information about X resources.

Function: **x-get-resource** attribute class &optional component subclass [¶](#index-x_002dget_002dresource)
:   The function `x-get-resource` retrieves a resource value from the X
    Window defaults database.

    Resources are indexed by a combination of a *key* and a *class*.
    This function searches using a key of the form
    ‘`instance.attribute`’ (where instance is the name
    under which Emacs was invoked), and using ‘`Emacs.class`’ as
    the class.

    The optional arguments component and subclass add to the key
    and the class, respectively. You must specify both of them or neither.
    If you specify them, the key is
    ‘`instance.component.attribute`’, and the class is
    ‘`Emacs.class.subclass`’.

Variable: **x-resource-class** [¶](#index-x_002dresource_002dclass)
:   This variable specifies the application name that `x-get-resource`
    should look up. The default value is `"Emacs"`. You can examine X
    resources for other application names by binding this
    variable to some other string, around a call to `x-get-resource`.

Variable: **x-resource-name** [¶](#index-x_002dresource_002dname)
:   This variable specifies the instance name that `x-get-resource`
    should look up. The default value is the name Emacs was invoked with,
    or the value specified with the ‘`-name`’ or ‘`-rn`’ switches.

To illustrate some of the above, suppose that you have the line:

```
xterm.vt100.background: yellow
```

in your X resources file (whose name is usually `~/.Xdefaults`
or `~/.Xresources`). Then:

```
(let ((x-resource-class "XTerm") (x-resource-name "xterm"))
  (x-get-resource "vt100.background" "VT100.Background"))
     ⇒ "yellow"
```

```
(let ((x-resource-class "XTerm") (x-resource-name "xterm"))
  (x-get-resource "background" "VT100" "vt100" "Background"))
     ⇒ "yellow"
```

Variable: **inhibit-x-resources** [¶](#index-inhibit_002dx_002dresources)
:   If this variable is non-`nil`, Emacs does not look up X
    resources, and X resources do not have any effect when creating new
    frames.

---

Previous: [X Resources](#Resources), Up: [Frames](#Frames)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 30.29 Display Feature Testing [¶](#Display-Feature-Testing-1)

The functions in this section describe the basic capabilities of a
particular display. Lisp programs can use them to adapt their behavior
to what the display can do. For example, a program that ordinarily uses
a popup menu could use the minibuffer if popup menus are not supported.

The optional argument display in these functions specifies which
display to ask the question about. It can be a display name, a frame
(which designates the display that frame is on), or `nil` (which
refers to the selected frame’s display, see [Input Focus](#Input-Focus)).

See [Color Names](#Color-Names), [Text Terminal Colors](#Text-Terminal-Colors), for other functions to
obtain information about displays.

Function: **display-popup-menus-p** &optional display [¶](#index-display_002dpopup_002dmenus_002dp)
:   This function returns `t` if popup menus are supported on
    display, `nil` if not. Support for popup menus requires
    that the mouse be available, since the menu is popped up by clicking
    the mouse on some portion of the Emacs display.

Function: **display-graphic-p** &optional display [¶](#index-display_002dgraphic_002dp)
:   This function returns `t` if display is a graphic display
    capable of displaying several frames and several different fonts at
    once. This is true for displays that use a window system such as X,
    and false for text terminals.

Function: **display-mouse-p** &optional display [¶](#index-display_002dmouse_002dp)
:   This function returns `t` if display has a mouse available,
    `nil` if not.

Function: **display-color-p** &optional display [¶](#index-display_002dcolor_002dp)
:   This function returns `t` if the screen is a color screen.

Function: **display-grayscale-p** &optional display [¶](#index-display_002dgrayscale_002dp)
:   This function returns `t` if the screen can display shades of gray.
    (All color displays can do this.)

Function: **display-supports-face-attributes-p** attributes &optional display [¶](#index-display_002dsupports_002dface_002dattributes_002dp)
:   This function returns non-`nil` if all the face attributes in
    attributes are supported (see [Face Attributes](#Face-Attributes)).

    The definition of “supported” is somewhat heuristic, but basically
    means that a face containing all the attributes in attributes,
    when merged with the default face for display, can be represented in a
    way that’s

    1. different in appearance than the default face, and
    2. close in spirit to what the attributes specify, if not exact.

    Point (2) implies that a `:weight black` attribute will be
    satisfied by any display that can display bold, as will
    `:foreground "yellow"` as long as some yellowish color can be
    displayed, but `:slant italic` will *not* be satisfied by
    the tty display code’s automatic substitution of a dim face for
    italic.

Function: **display-selections-p** &optional display [¶](#index-display_002dselections_002dp)
:   This function returns `t` if display supports selections.
    Windowed displays normally support selections, but they may also be
    supported in some other cases.

Function: **display-images-p** &optional display [¶](#index-display_002dimages_002dp)
:   This function returns `t` if display can display images.
    Windowed displays ought in principle to handle images, but some
    systems lack the support for that. On a display that does not support
    images, Emacs cannot display a tool bar.

Function: **display-screens** &optional display [¶](#index-display_002dscreens)
:   This function returns the number of screens associated with the display.

Function: **display-pixel-height** &optional display [¶](#index-display_002dpixel_002dheight)
:   This function returns the height of the screen in pixels.
    On a character terminal, it gives the height in characters.

    For graphical terminals, note that on multi-monitor setups this
    refers to the pixel height for all physical monitors associated with
    display. See [Multiple Terminals](#Multiple-Terminals).

Function: **display-pixel-width** &optional display [¶](#index-display_002dpixel_002dwidth)
:   This function returns the width of the screen in pixels.
    On a character terminal, it gives the width in characters.

    For graphical terminals, note that on multi-monitor setups this
    refers to the pixel width for all physical monitors associated with
    display. See [Multiple Terminals](#Multiple-Terminals).

Function: **display-mm-height** &optional display [¶](#index-display_002dmm_002dheight)
:   This function returns the height of the screen in millimeters,
    or `nil` if Emacs cannot get that information.

    For graphical terminals, note that on multi-monitor setups this
    refers to the height for all physical monitors associated with
    display. See [Multiple Terminals](#Multiple-Terminals).

Function: **display-mm-width** &optional display [¶](#index-display_002dmm_002dwidth)
:   This function returns the width of the screen in millimeters,
    or `nil` if Emacs cannot get that information.

    For graphical terminals, note that on multi-monitor setups this
    refers to the width for all physical monitors associated with
    display. See [Multiple Terminals](#Multiple-Terminals).

User Option: **display-mm-dimensions-alist** [¶](#index-display_002dmm_002ddimensions_002dalist)
:   This variable allows the user to specify the dimensions of graphical
    displays returned by `display-mm-height` and
    `display-mm-width` in case the system provides incorrect values.

Function: **display-backing-store** &optional display [¶](#index-display_002dbacking_002dstore)
:   This function returns the backing store capability of the display.
    Backing store means recording the pixels of windows (and parts of
    windows) that are not exposed, so that when exposed they can be
    displayed very quickly.

    Values can be the symbols `always`, `when-mapped`, or
    `not-useful`. The function can also return `nil`
    when the question is inapplicable to a certain kind of display.

Function: **display-save-under** &optional display [¶](#index-display_002dsave_002dunder)
:   This function returns non-`nil` if the display supports the
    SaveUnder feature. That feature is used by pop-up windows
    to save the pixels they obscure, so that they can pop down
    quickly.

Function: **display-planes** &optional display [¶](#index-display_002dplanes)
:   This function returns the number of planes the display supports.
    This is typically the number of bits per pixel.
    For a tty display, it is log to base two of the number of colors supported.

Function: **display-visual-class** &optional display [¶](#index-display_002dvisual_002dclass)
:   This function returns the visual class for the screen. The value is
    one of the symbols `static-gray` (a limited, unchangeable number
    of grays), `gray-scale` (a full range of grays),
    `static-color` (a limited, unchangeable number of colors),
    `pseudo-color` (a limited number of colors), `true-color` (a
    full range of colors), and `direct-color` (a full range of
    colors).

Function: **display-color-cells** &optional display [¶](#index-display_002dcolor_002dcells)
:   This function returns the number of color cells the screen supports.

These functions obtain additional information about the window
system in use where Emacs shows the specified display. (Their
names begin with `x-` for historical reasons.)

Function: **x-server-version** &optional display [¶](#index-x_002dserver_002dversion)
:   This function returns the list of version numbers of the GUI window
    system running on display, such as the X server on GNU and Unix
    systems. The value is a list of three integers: the major and minor
    version numbers of the protocol, and the distributor-specific release
    number of the window system software itself. On GNU and Unix systems,
    these are normally the version of the X protocol and the
    distributor-specific release number of the X server software. On
    MS-Windows, this is the version of the Windows OS.

Function: **x-server-vendor** &optional display [¶](#index-x_002dserver_002dvendor)
:   This function returns the vendor that provided the window system
    software (as a string). On GNU and Unix systems this really means
    whoever distributes the X server. On MS-Windows this is the vendor ID
    string of the Windows OS (Microsoft).

    When the developers of X labeled software distributors as
    “vendors”, they showed their false assumption that no system could
    ever be developed and distributed noncommercially.

---

Next: [Markers](#Markers), Previous: [Frames](#Frames), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 31 Positions [¶](#Positions-1)

A *position* is the index of a character in the text of a buffer.
More precisely, a position identifies the place between two characters
(or before the first character, or after the last character), so we can
speak of the character before or after a given position. However, we
often speak of the character “at” a position, meaning the character
after that position.

Positions are usually represented as integers starting from 1, but
can also be represented as *markers*—special objects that
relocate automatically when text is inserted or deleted so they stay
with the surrounding characters. Functions that expect an argument to
be a position (an integer), but accept a marker as a substitute,
normally ignore which buffer the marker points into; they convert the
marker to an integer, and use that integer, exactly as if you had
passed the integer as the argument, even if the marker points to the
wrong buffer. A marker that points nowhere cannot convert to an
integer; using it instead of an integer causes an error.
See [Markers](#Markers).

See also the field feature (see [Defining and Using Fields](#Fields)), which provides
functions that are used by many cursor-motion commands.

* [Point](#Point)
* [Motion](#Motion)
* [Excursions](#Excursions)
* [Narrowing](#Narrowing)

---

Next: [Motion](#Motion), Up: [Positions](#Positions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 31.1 Point [¶](#Point-1)

*Point* is a special buffer position used by many editing
commands, including the self-inserting typed characters and text
insertion functions. Other commands move point through the text
to allow editing and insertion at different places.

Like other positions, point designates a place between two characters
(or before the first character, or after the last character), rather
than a particular character. Usually terminals display the cursor over
the character that immediately follows point; point is actually before
the character on which the cursor sits.

The value of point is a number no less than 1, and no greater than the
buffer size plus 1. If narrowing is in effect (see [Narrowing](#Narrowing)), then
point is constrained to fall within the accessible portion of the buffer
(possibly at one end of it).

Each buffer has its own value of point, which is independent of the
value of point in other buffers. Each window also has a value of point,
which is independent of the value of point in other windows showing the
same buffer. This is why the cursor may appear at different positions
in various windows that display the same buffer. Wherever necessary, we
use the terms *buffer point* for the unique position of point of a
specific buffer and the term *window point* for the position of
point in a specific window showing that buffer. When a buffer appears
in only one window, its buffer’s point and that window’s point normally
have the same value, so the distinction is rarely important.
See [Windows and Point](#Window-Point), for more details.

Function: **point** [¶](#index-point-1)
:   This function returns the value of point in the current buffer,
    as an integer.

    ```
    (point)
         ⇒ 175
    ```

Function: **point-min** [¶](#index-point_002dmin)
:   This function returns the minimum accessible value of point in the
    current buffer. This is normally 1, but if narrowing is in effect, it
    is the position of the start of the region that you narrowed to.
    (See [Narrowing](#Narrowing).)

Function: **point-max** [¶](#index-point_002dmax)
:   This function returns the maximum accessible value of point in the
    current buffer. This is `(1+ (buffer-size))`, unless narrowing is
    in effect, in which case it is the position of the end of the region
    that you narrowed to. (See [Narrowing](#Narrowing).)

Function: **buffer-end** flag [¶](#index-buffer_002dend)
:   This function returns `(point-max)` if flag is greater than
    0, `(point-min)` otherwise. The argument flag must be a
    number.

Function: **buffer-size** &optional buffer [¶](#index-buffer_002dsize)
:   This function returns the total number of characters in the current
    buffer. In the absence of any narrowing (see [Narrowing](#Narrowing)),
    `point-max` returns a value one larger than this.

    If you specify a buffer, buffer, then the value is the
    size of buffer.

    ```
    (buffer-size)
         ⇒ 35
    ```

    ```
    (point-max)
         ⇒ 36
    ```

---

Next: [Excursions](#Excursions), Previous: [Point](#Point), Up: [Positions](#Positions)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
