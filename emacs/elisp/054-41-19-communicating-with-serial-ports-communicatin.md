### 41.19 Communicating with Serial Ports [¶](#Communicating-with-Serial-Ports)

Emacs can communicate with serial ports. For interactive use,
`M-x serial-term` opens a terminal window. In a Lisp program,
`make-serial-process` creates a process object.

The serial port can be configured at run-time, without having to
close and re-open it. The function `serial-process-configure`
lets you change the speed, bytesize, and other parameters. In a
terminal window created by `serial-term`, you can click on the
mode line for configuration.

A serial connection is represented by a process object, which can be
used in a similar way to a subprocess or network process. You can send and
receive data, and configure the serial port. A serial process object
has no process ID, however, and you can’t send signals to it, and the
status codes are different from other types of processes.
`delete-process` on the process object or `kill-buffer` on
the process buffer close the connection, but this does not affect the
device connected to the serial port.

The function `process-type` returns the symbol `serial`
for a process object representing a serial port connection.

Serial ports are available on GNU/Linux, Unix, and MS Windows systems.

Command: **serial-term** port speed &optional line-mode [¶](#index-serial_002dterm)
:   Start a terminal-emulator for a serial port in a new buffer.
    port is the name of the serial port to connect to. For
    example, this could be `/dev/ttyS0` on Unix. On MS Windows, this
    could be `COM1`, or `\\.\COM10` (double the backslashes in
    Lisp strings).

    speed is the speed of the serial port in bits per second. 9600
    is a common value. The buffer is in Term mode; see [Term Mode](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Term-Mode) in The GNU Emacs Manual, for the commands to use in that buffer.
    You can change the speed and the configuration in the mode line menu.
    If line-mode is non-`nil`, `term-line-mode` is used;
    otherwise `term-raw-mode` is used.

Function: **make-serial-process** &rest args [¶](#index-make_002dserial_002dprocess)
:   This function creates a process and a buffer. Arguments are specified
    as keyword/argument pairs. Here’s the list of the meaningful
    keywords, with the first two (port and speed) being mandatory:

    `:port port`
    :   This is the name of the serial port. On Unix and GNU systems, this is
        a file name such as `/dev/ttyS0`. On Windows, this could be
        `COM1`, or `\\.\COM10` for ports higher than `COM9`
        (double the backslashes in Lisp strings).

    `:speed speed`
    :   The speed of the serial port in bits per second. This function calls
        `serial-process-configure` to handle the speed; see the
        following documentation of that function for more details.

    `:name name`
    :   The name of the process. If name is not given, port will
        serve as the process name as well.

    `:buffer buffer`
    :   The buffer to associate with the process. The value can be either a
        buffer or a string that names a buffer. Process output goes at the
        end of that buffer, unless you specify an output stream or filter
        function to handle the output. If buffer is not given, the
        process buffer’s name is taken from the value of the `:name`
        keyword.

    `:coding coding`
    :   If coding is a symbol, it specifies the coding system used for
        both reading and writing for this process. If coding is a cons
        `(decoding . encoding)`, decoding is used for
        reading, and encoding is used for writing. If not specified,
        the default is to determine the coding systems from the data itself.

    `:noquery query-flag`
    :   Initialize the process query flag to query-flag. See [Querying Before Exit](#Query-Before-Exit). The flags defaults to `nil` if unspecified.

    `:stop bool`
    :   Start process in the stopped state if bool is
        non-`nil`. In the stopped state, a serial process does not
        accept incoming data, but you can send outgoing data. The stopped
        state is cleared by `continue-process` and set by
        `stop-process`.

    `:filter filter`
    :   Install filter as the process filter.

    `:sentinel sentinel`
    :   Install sentinel as the process sentinel.

    `:plist plist`
    :   Install plist as the initial plist of the process.

    `:bytesize`

    `:parity`

    `:stopbits`

    `:flowcontrol`
    :   These are handled by `serial-process-configure`, which is called
        by `make-serial-process`.

    The original argument list, possibly modified by later configuration,
    is available via the function `process-contact`.

    Here is an example:

    ```
    (make-serial-process :port "/dev/ttyS0" :speed 9600)
    ```

Function: **serial-process-configure** &rest args [¶](#index-serial_002dprocess_002dconfigure)
:   This function configures a serial port connection. Arguments are
    specified as keyword/argument pairs. Attributes that are not given
    are re-initialized from the process’s current configuration (available
    via the function `process-contact`), or set to reasonable default
    values. The following arguments are defined:

    `:process process`

    `:name name`

    `:buffer buffer`

    `:port port`
    :   Any of these arguments can be given to identify the process that is to
        be configured. If none of these arguments is given, the current
        buffer’s process is used.

    `:speed speed`
    :   The speed of the serial port in bits per second, a.k.a. *baud
        rate*. The value can be any number, but most serial ports work only
        at a few defined values between 1200 and 115200, with 9600 being the
        most common value. If speed is `nil`, the function ignores
        all other arguments and does not configure the port. This may be
        useful for special serial ports such as Bluetooth-to-serial converters,
        which can only be configured through ‘`AT`’ commands sent through the
        connection. The value of `nil` for speed is valid only for
        connections that were already opened by a previous call to
        `make-serial-process` or `serial-term`.

    `:bytesize bytesize`
    :   The number of bits per byte, which can be 7 or 8. If bytesize
        is not given or `nil`, it defaults to 8.

    `:parity parity`
    :   The value can be `nil` (don’t use parity), the symbol
        `odd` (use odd parity), or the symbol `even` (use even
        parity). If parity is not given, it defaults to no parity.

    `:stopbits stopbits`
    :   The number of stopbits used to terminate a transmission
        of each byte. stopbits can be 1 or 2. If stopbits is not
        given or `nil`, it defaults to 1.

    `:flowcontrol flowcontrol`
    :   The type of flow control to use for this connection, which is either
        `nil` (don’t use flow control), the symbol `hw` (use RTS/CTS
        hardware flow control), or the symbol `sw` (use XON/XOFF software
        flow control). If flowcontrol is not given, it defaults to no
        flow control.

    Internally, `make-serial-process` calls
    `serial-process-configure` for the initial configuration of the
    serial port.

---

Previous: [Communicating with Serial Ports](#Serial-Ports), Up: [Processes](#Processes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 41.20 Packing and Unpacking Byte Arrays [¶](#Packing-and-Unpacking-Byte-Arrays)

This section describes how to pack and unpack arrays of bytes,
usually for binary network protocols. These functions convert byte arrays
to alists, and vice versa. The byte array can be represented as a
unibyte string or as a vector of integers, while the alist associates
symbols either with fixed-size objects or with recursive sub-alists.
To use the functions referred to in this section, load the
`bindat` library.

Conversion from byte arrays to nested alists is also known as
*deserializing* or *unpacking*, while going in the opposite
direction is also known as *serializing* or *packing*.

* [Describing Data Layout](#Bindat-Types)
* [Functions to Unpack and Pack Bytes](#Bindat-Functions)
* [Advanced data layout specifications](#Bindat-Computed-Types)

---

Next: [Functions to Unpack and Pack Bytes](#Bindat-Functions), Up: [Packing and Unpacking Byte Arrays](#Byte-Packing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.20.1 Describing Data Layout [¶](#Describing-Data-Layout)

To control unpacking and packing, you write a *data layout
specification*, also called a *Bindat type expression*. This can
be a *base type* or a *composite type* made of several fields,
where the specification controls the length of each field to be
processed, and how to pack or unpack it. We normally keep bindat type
values in variables whose names end in `-bindat-spec`; that kind
of name is automatically recognized as risky (see [File Local Variables](#File-Local-Variables)).

Macro: **bindat-type** &rest type [¶](#index-bindat_002dtype)
:   Creates a Bindat type *value* object according to the Bindat type
    *expression* type.

A field’s *type* describes the size (in bytes) of the object
that the field represents and, in the case of multibyte fields, how
the bytes are ordered within the field. The two possible orderings
are *big endian* (also known as “network byte ordering”) and
*little endian*. For instance, the number `#x23cd` (decimal
9165) in big endian would be the two bytes `#x23` `#xcd`;
and in little endian, `#xcd` `#x23`. Here are the possible
type values:

`u8`

`byte`
:   Unsigned byte, with length 1.

`uint bitlen &optional le`
:   Unsigned integer in network byte order (big-endian), with bitlen bits.
    bitlen has to be a multiple of 8.
    If le is non-`nil`, then use little-endian byte order.

`sint bitlen le`
:   Signed integer in network byte order (big-endian), with bitlen bits.
    bitlen has to be a multiple of 8.
    If le is non-`nil`, then use little-endian byte order.

`str len`
:   Unibyte string (see [Text Representations](#Text-Representations)) of length len bytes.
    When packing, the first len bytes of the input string are copied
    to the packed output. If the input string is shorter than len,
    the remaining bytes will be null (zero) unless a pre-allocated string
    was provided to `bindat-pack`, in which case the remaining bytes
    are left unmodified. If the input string is multibyte with only ASCII
    and `eight-bit` characters, it is converted to unibyte before it
    is packed; other multibyte strings signal an error. When unpacking,
    any null bytes in the packed input string will appear in the unpacked
    output.

`strz &optional len`
:   If len is not provided, this is a variable-length
    null-terminated unibyte string (see [Text Representations](#Text-Representations)). When
    packing into `strz`, the entire input string is copied to the
    packed output followed by a null (zero) byte. (If pre-allocated
    string is provided for packing into `strz`, that pre-allocated
    string should have enough space for the additional null byte appended
    to the output string contents, see [Functions to Unpack and Pack Bytes](#Bindat-Functions)). The length
    of the packed output is the length of the input string plus one (for
    the null terminator). The input string must not contain any null
    bytes. If the input string is multibyte with only ASCII and
    `eight-bit` characters, it is converted to unibyte before it is
    packed; other multibyte strings signal an error. When unpacking a
    `strz`, the resulting output string will contain all bytes up to
    (but excluding) the null byte that terminated the input string.

    If len is provided, `strz` behaves the same as `str`,
    but with a couple of differences:

    * When packing, a null terminator is written after the packed input
      string if the number of characters in the input string is less than
      len.
    * When unpacking, the first null byte encountered in the packed string
      is interpreted as the terminating byte, and it and all subsequent
      bytes are excluded from the result of the unpacking.

    > **Caution:** The packed output will not be null-terminated unless the input string
    > is shorter than len bytes or it contains a null byte within the
    > first len bytes.

`vec len [type]`
:   Vector of len elements. The type of the elements is given by
    type, defaulting to bytes. The type can be any Bindat
    type expression.

`repeat len [type]`
:   Like `vec`, but it unpacks to and packs from lists, whereas
    `vec` unpacks to vectors.

`bits len`
:   List of bits that are set to 1 in len bytes. The bytes are
    taken in big-endian order, and the bits are numbered starting with
    `8 * len − 1` and ending with zero. For example:
    `bits 2` unpacks `#x28` `#x1c` to `(2 3 4 11 13)`
    and `#x1c` `#x28` to `(3 5 10 11 12)`.

`fill len`
:   len bytes used as a mere filler. In packing, these bytes are
    left unchanged, which normally means they remain zero.
    When unpacking, this just returns `nil`.

`align len`
:   Same as `fill` except the number of bytes is that needed to skip
    to the next multiple of len bytes.

`type exp`
:   This lets you refer to a type indirectly: exp is a Lisp
    expression which should return a Bindat type *value*.

`unit exp`
:   This is a trivial type which uses up 0 bits of space. exp
    describes the value returned when we try to “unpack” such a field.

`struct fields...`
:   Composite type made of several fields. Every field is of the form
    `(name type)` where type can be any Bindat
    type expression. name can be `_` when the field’s value
    does not deserve to be named, as is often the case for `align`
    and `fill` fields.
    When the context makes it clear that this is a Bindat type expression,
    the symbol `struct` can be omitted.

In the types above, len and bitlen are given as an integer
specifying the number of bytes (or bits) in the field. When the
length of a field is not fixed, it typically depends on the value of
preceding fields. For this reason, the length len does not have
to be a constant but can be any Lisp expression and it can refer to
the value of previous fields via their name.

For example, the specification of a data layout where a leading byte gives
the size of a subsequent vector of 16 bit integers could be:

```
(bindat-type
  (len      u8)
  (payload  vec (1+ len) uint 16))
```

---

Next: [Advanced data layout specifications](#Bindat-Computed-Types), Previous: [Describing Data Layout](#Bindat-Types), Up: [Packing and Unpacking Byte Arrays](#Byte-Packing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.20.2 Functions to Unpack and Pack Bytes [¶](#Functions-to-Unpack-and-Pack-Bytes)

In the following documentation, type refers to a Bindat type
value as returned from `bindat-type`, raw to a byte
array, and struct to an alist representing unpacked field data.

Function: **bindat-unpack** type raw &optional idx [¶](#index-bindat_002dunpack)
:   This function unpacks data from the unibyte string or byte
    array raw
    according to type. Normally, this starts unpacking at the
    beginning of the byte array, but if idx is non-`nil`, it
    specifies a zero-based starting position to use instead.

    The value is an alist or nested alist in which each element describes
    one unpacked field.

Function: **bindat-get-field** struct &rest name [¶](#index-bindat_002dget_002dfield)
:   This function selects a field’s data from the nested alist
    struct. Usually struct was returned by
    `bindat-unpack`. If name corresponds to just one argument,
    that means to extract a top-level field value. Multiple name
    arguments specify repeated lookup of sub-structures. An integer
    name acts as an array index.

    For example, `(bindat-get-field struct a b 2 c)` means
    to find field `c` in the third element of subfield `b` of
    field `a`. (This corresponds to `struct.a.b[2].c` in
    the C programming language syntax.)

Although packing and unpacking operations change the organization of
data (in memory), they preserve the data’s *total length*, which is
the sum of all the fields’ lengths, in bytes. This value is not
generally inherent in either the specification or alist alone; instead,
both pieces of information contribute to its calculation. Likewise, the
length of a string or array being unpacked may be longer than the data’s
total length as described by the specification.

Function: **bindat-length** type struct [¶](#index-bindat_002dlength)
:   This function returns the total length of the data in struct,
    according to type.

Function: **bindat-pack** type struct &optional raw idx [¶](#index-bindat_002dpack)
:   This function returns a byte array packed according to type from
    the data in the alist struct. It normally creates and fills a
    new byte array starting at the beginning. However, if raw
    is non-`nil`, it specifies a pre-allocated unibyte string or vector to
    pack into. If idx is non-`nil`, it specifies the starting
    offset for packing into raw.

    When pre-allocating, you should make sure `(length raw)`
    meets or exceeds the total length to avoid an out-of-range error.

Function: **bindat-ip-to-string** ip [¶](#index-bindat_002dip_002dto_002dstring)
:   Convert the Internet address vector ip to a string in the usual
    dotted notation.

    ```
    (bindat-ip-to-string [127 0 0 1])
         ⇒ "127.0.0.1"
    ```

---

Previous: [Functions to Unpack and Pack Bytes](#Bindat-Functions), Up: [Packing and Unpacking Byte Arrays](#Byte-Packing)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 41.20.3 Advanced data layout specifications [¶](#Advanced-data-layout-specifications)

Bindat type expressions are not limited to the types described
earlier. They can also be arbitrary Lisp forms returning Bindat
type expressions. For example, the type below describes data which
can either contain a 24-bit error code or a vector of bytes:

```
(bindat-type
  (len      u8)
  (payload  . (if (zerop len) (uint 24) (vec (1- len)))))
```

Furthermore, while composite types are normally unpacked to (and
packed from) association lists, this can be changed via the use of
the following special keyword arguments:

`:unpack-val exp`
:   When the list of fields ends with this keyword argument, then the value
    returned when unpacking is the value of exp instead of the
    standard alist. exp can refer to all the previous fields by
    their name.

`:pack-val exp`
:   If a field’s type is followed by this keyword argument, then the value
    packed into this field is returned by exp instead of being
    extracted from the alist.

`:pack-var name`
:   If the list of fields is preceded by this keyword argument, then all
    the subsequent `:pack-val` arguments can refer to the overall
    value to pack into this composite type via the variable named
    name.

For example, one could describe a 16-bit signed integer as follows:

```
(defconst sint16-bindat-spec
  (let* ((max (ash 1 15))
         (wrap (+ max max)))
    (bindat-type :pack-var v
                 (n uint 16 :pack-val (if (< v 0) (+ v wrap) v))
                 :unpack-val (if (>= n max) (- n wrap) n))))
```

Which would then behave as follows:

```
(bindat-pack sint16-bindat-spec -8)
     ⇒ "\377\370"

(bindat-unpack sint16-bindat-spec "\300\100")
     ⇒ -16320
```

Finally, you can define new Bindat type forms to use in Bindat type
expressions with `bindat-defmacro`:

Macro: **bindat-defmacro** name args &rest body [¶](#index-bindat_002ddefmacro)
:   Define a new Bindat type expression named name and taking
    arguments args. Its behavior follows that of `defmacro`,
    which the important difference that the new forms can only be used
    within Bindat type expressions.

---

Next: [Operating System Interface](#System-Interface), Previous: [Processes](#Processes), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 42 Emacs Display [¶](#Emacs-Display)

This chapter describes a number of features related to the display
that Emacs presents to the user.

* [Refreshing the Screen](#Refresh-Screen)
* [Forcing Redisplay](#Forcing-Redisplay)
* [Truncation](#Truncation)
* [The Echo Area](#The-Echo-Area)
* [Reporting Warnings](#Warnings)
* [Invisible Text](#Invisible-Text)
* [Selective Display](#Selective-Display)
* [Temporary Displays](#Temporary-Displays)
* [Overlays](#Overlays)
* [Size of Displayed Text](#Size-of-Displayed-Text)
* [Line Height](#Line-Height)
* [Faces](#Faces)
* [Fringes](#Fringes)
* [Scroll Bars](#Scroll-Bars)
* [Window Dividers](#Window-Dividers)
* [The `display` Property](#Display-Property)
* [Images](#Images)
* [Icons](#Icons)
* [Embedded Native Widgets](#Xwidgets)
* [Buttons](#Buttons)
* [Abstract Display](#Abstract-Display)
* [Blinking Parentheses](#Blinking)
* [Character Display](#Character-Display)
* [Beeping](#Beeping)
* [Window Systems](#Window-Systems)
* [Tooltips](#Tooltips)
* [Bidirectional Display](#Bidirectional-Display)

---

Next: [Forcing Redisplay](#Forcing-Redisplay), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.1 Refreshing the Screen [¶](#Refreshing-the-Screen)

The function `redraw-frame` clears and redisplays the entire
contents of a given frame (see [Frames](#Frames)). This is useful if the
screen is corrupted.

Function: **redraw-frame** &optional frame [¶](#index-redraw_002dframe)
:   This function clears and redisplays frame frame. If frame
    is omitted or `nil`, it redraws the selected frame.

Even more powerful is `redraw-display`:

Command: **redraw-display** [¶](#index-redraw_002ddisplay)
:   This function clears and redisplays all visible frames.

In Emacs, processing user input takes priority over redisplay. If
you call these functions when input is available, they don’t redisplay
immediately, but the requested redisplay does happen
eventually—after all the input has been processed.

On text terminals, suspending and resuming Emacs normally also
refreshes the screen. Some terminal emulators record separate
contents for display-oriented programs such as Emacs and for ordinary
sequential display. If you are using such a terminal, you might want
to inhibit the redisplay on resumption.

User Option: **no-redraw-on-reenter** [¶](#index-no_002dredraw_002don_002dreenter)
:   This variable controls whether Emacs redraws the entire screen after it
    has been suspended and resumed. Non-`nil` means there is no need
    to redraw, `nil` means redrawing is needed. The default is `nil`.

---

Next: [Truncation](#Truncation), Previous: [Refreshing the Screen](#Refresh-Screen), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.2 Forcing Redisplay [¶](#Forcing-Redisplay-1)

Emacs normally tries to redisplay the screen whenever it waits for
input. With the following function, you can request an immediate
attempt to redisplay, in the middle of Lisp code, without actually
waiting for input.

Function: **redisplay** &optional force [¶](#index-redisplay)
:   This function tries immediately to redisplay. The optional argument
    force, if non-`nil`, forces the redisplay to be performed,
    instead of being preempted if input is pending.

    The function returns `t` if it actually tried to redisplay, and
    `nil` otherwise. A value of `t` does not mean that
    redisplay proceeded to completion; it could have been preempted by
    newly arriving input.

Although `redisplay` tries immediately to redisplay, it does
not change how Emacs decides which parts of its frame(s) to redisplay.
By contrast, the following function adds certain windows to the
pending redisplay work (as if their contents had completely changed),
but does not immediately try to perform redisplay.

Function: **force-window-update** &optional object [¶](#index-force_002dwindow_002dupdate)
:   This function forces some or all windows to be updated the next time
    Emacs does a redisplay. If object is a window, that window is
    to be updated. If object is a buffer or buffer name, all
    windows displaying that buffer are to be updated. If object is
    `nil` (or omitted), all windows are to be updated.

    This function does not do a redisplay immediately; Emacs does that as
    it waits for input, or when the function `redisplay` is called.

Variable: **pre-redisplay-function** [¶](#index-pre_002dredisplay_002dfunction)
:   A function run just before redisplay. It is called with one argument,
    the set of windows to be redisplayed. The set can be `nil`,
    meaning only the selected window, or `t`, meaning all the
    windows.

Variable: **pre-redisplay-functions** [¶](#index-pre_002dredisplay_002dfunctions)
:   This hook is run just before redisplay. It is called once in each
    window that is about to be redisplayed, with `current-buffer` set
    to the buffer displayed in that window.

---

Next: [The Echo Area](#The-Echo-Area), Previous: [Forcing Redisplay](#Forcing-Redisplay), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.3 Truncation [¶](#Truncation-1)

When a line of text extends beyond the right edge of a window, Emacs
can *continue* the line (make it wrap to the next screen
line), or *truncate* the line (limit it to one screen line). The
additional screen lines used to display a long text line are called
*continuation* lines. Continuation is not the same as filling;
continuation happens on the screen only, not in the buffer contents,
and it breaks a line precisely at the right margin, not at a word
boundary. See [Filling](#Filling).

On a graphical display, tiny arrow images in the window fringes
indicate truncated and continued lines (see [Fringes](#Fringes)). On a text
terminal, and on a graphical display when `fringe-mode` was
turned off, a ‘`$`’ in the rightmost column of the window indicates
truncation; a ‘`\`’ on the rightmost column indicates a line that
wraps. (The display table can specify alternate characters to use
for this; see [Display Tables](#Display-Tables)).

Since wrapping and truncation of text contradict each other, Emacs
turns off line truncation when wrapping is requested, and vice versa.

User Option: **truncate-lines** [¶](#index-truncate_002dlines)
:   If this buffer-local variable is non-`nil`, lines that extend
    beyond the right edge of the window are truncated; otherwise, they are
    continued. As a special exception, the variable
    `truncate-partial-width-windows` takes precedence in
    *partial-width* windows (i.e., windows that do not occupy the
    entire frame width).

User Option: **truncate-partial-width-windows** [¶](#index-truncate_002dpartial_002dwidth_002dwindows)
:   This variable controls line truncation in *partial-width* windows.
    A partial-width window is one that does not occupy the entire frame
    width (see [Splitting Windows](#Splitting-Windows)). If the value is `nil`, line
    truncation is determined by the variable `truncate-lines` (see
    above). If the value is an integer n, lines are truncated if
    the partial-width window has fewer than n columns, regardless of
    the value of `truncate-lines`; if the partial-width window has
    n or more columns, line truncation is determined by
    `truncate-lines`. For any other non-`nil` value, lines are
    truncated in every partial-width window, regardless of the value of
    `truncate-lines`.

When horizontal scrolling (see [Horizontal Scrolling](#Horizontal-Scrolling)) is in use in
a window, that forces truncation.

Variable: **wrap-prefix** [¶](#index-wrap_002dprefix)
:   If this buffer-local variable is non-`nil`, it defines a
    *wrap prefix* which Emacs displays at the start of every
    continuation line. (If lines are truncated, `wrap-prefix` is
    never used.) Its value may be a string or an image (see [Other Display Specifications](#Other-Display-Specs)), or a stretch of whitespace such as specified by the
    `:width` or `:align-to` display properties (see [Specified Spaces](#Specified-Space)). The value is interpreted in the same way as a `display`
    text property, with one important difference: the horizontal position
    specified by `:align-to` is measured from the visual beginning of
    the screen line. See [The `display` Property](#Display-Property).

    A wrap prefix may also be specified for regions of text, using the
    `wrap-prefix` text or overlay property. This takes precedence
    over the `wrap-prefix` variable. See [Properties with Special Meanings](#Special-Properties).

Variable: **line-prefix** [¶](#index-line_002dprefix)
:   If this buffer-local variable is non-`nil`, it defines a
    *line prefix* which Emacs displays at the start of every
    non-continuation line. Its value may be a string or an image
    (see [Other Display Specifications](#Other-Display-Specs)), or a stretch of whitespace such as
    specified by the `:width` or `:align-to` display properties
    (see [Specified Spaces](#Specified-Space)). The value is interpreted in the same way
    as a `display` text property. See [The `display` Property](#Display-Property).

    A line prefix may also be specified for regions of text using the
    `line-prefix` text or overlay property. This takes precedence
    over the `line-prefix` variable. See [Properties with Special Meanings](#Special-Properties).

---

Next: [Reporting Warnings](#Warnings), Previous: [Truncation](#Truncation), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.4 The Echo Area [¶](#The-Echo-Area-1)

The *echo area* is used for displaying error messages
(see [Errors](#Errors)), for messages made with the `message` primitive,
and for echoing keystrokes. It is not the same as the minibuffer,
despite the fact that the minibuffer appears (when active) in the same
place on the screen as the echo area. See [The
Minibuffer](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Minibuffer) in The GNU Emacs Manual.

Apart from the functions documented in this section, you can print
Lisp objects to the echo area by specifying `t` as the output
stream. See [Output Streams](#Output-Streams).

* [Displaying Messages in the Echo Area](#Displaying-Messages)
* [Reporting Operation Progress](#Progress)
* [Logging Messages in `*Messages*`](#Logging-Messages)
* [Echo Area Customization](#Echo-Area-Customization)

---

Next: [Reporting Operation Progress](#Progress), Up: [The Echo Area](#The-Echo-Area)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.4.1 Displaying Messages in the Echo Area [¶](#Displaying-Messages-in-the-Echo-Area)

This section describes the standard functions for displaying
messages in the echo area.

Function: **message** format-string &rest arguments [¶](#index-message)
:   This function displays a message in the echo area.
    format-string is a format string, and arguments are the
    objects for its format specifications, like in the `format-message`
    function (see [Formatting Strings](#Formatting-Strings)). The resulting formatted string
    is displayed in the echo area; if it contains `face` text
    properties, it is displayed with the specified faces (see [Faces](#Faces)).
    The string is also added to the `*Messages*` buffer, but without
    text properties (see [Logging Messages in `*Messages*`](#Logging-Messages)).

    Typically grave accent and apostrophe in the format translate to
    matching curved quotes, e.g., `` "Missing `%s'" `` might result in
    `"Missing ‘foo’"`. See [Text Quoting Style](#Text-Quoting-Style), for how to influence
    or inhibit this translation.

    In batch mode, the message is printed to the standard error stream,
    followed by a newline.

    When `inhibit-message` is non-`nil`, no message will be displayed
    in the echo area, it will only be logged to ‘`*Messages*`’.

    If format-string is `nil` or the empty string,
    `message` clears the echo area; if the echo area has been
    expanded automatically, this brings it back to its normal size. If
    the minibuffer is active, this brings the minibuffer contents back
    onto the screen immediately.

    ```
    (message "Reverting `%s'..." (buffer-name))
     -| Reverting ‘subr.el’...
    ⇒ "Reverting ‘subr.el’..."
    ```

    ```

    ```

    ```
    ---------- Echo Area ----------
    Reverting ‘subr.el’...
    ---------- Echo Area ----------
    ```

    To automatically display a message in the echo area or in a pop-buffer,
    depending on its size, use `display-message-or-buffer` (see below).

    **Warning:** If you want to use your own string as a message
    verbatim, don’t just write `(message string)`. If
    string contains ‘`%`’, ‘`` ` ``’, or ‘`'`’ it may be
    reformatted, with undesirable results. Instead, use `(message
    "%s" string)`.

The following facilities allow users and Lisp programs to control how
echo-area messages are displayed.

Variable: **set-message-function** [¶](#index-set_002dmessage_002dfunction)
:   If this variable is non-`nil`, it should be a function of one
    argument, the text of a message to display in the echo area. That
    function will be called by `message` and related functions. If
    the function returns `nil`, the message is displayed in the echo
    area as usual. If the function returns a string, that string is
    displayed in the echo area *instead* of the original message. If
    the function returns any other non-`nil` value, that means the
    message was already handled, so `message` will not display
    anything in the echo area.

    The default value calls `set-minibuffer-message`, described
    below.

Variable: **clear-message-function** [¶](#index-clear_002dmessage_002dfunction)
:   If this variable is non-`nil`, it should be a function of no
    arguments; `message` and related functions call it when their
    argument message is `nil` or the empty string, to clear the echo
    area.

    Usually this function is called when the next input event arrives
    after displaying an echo-area message. The function is expected to
    clear the message displayed by its counterpart function specified by
    `set-message-function`, but doesn’t have to. If the function
    wants the echo area to remain uncleared, it should return the symbol
    `dont-clear-message`; any other value will result in the echo
    area being cleared.

    The default value is the function that clears the message displayed in
    an active minibuffer.

User Option: **set-message-functions** [¶](#index-set_002dmessage_002dfunctions)
:   The value of this user option is a list of functions to be called for
    handling display of echo-area messages. Each function is called with
    one argument, the text of the message to display. If the function
    returns a string, that string replaces the original message, and the
    next function in the list is called with the new message text. If the
    function returns `nil`, the next function in the list is called
    with the same text; if the last function in the list returns
    `nil`, the message text is displayed in the echo area. If the
    function returns a non-`nil` value that is not a string, the
    message is considered to be handled, and no further functions in the
    list are called.

    The three useful functions to be put in the list that is the value of
    this option are described below.

Function: **set-minibuffer-message** message [¶](#index-set_002dminibuffer_002dmessage)
:   This function displays message in the echo-area when the
    minibuffer is not active, and at the end of the minibuffer when the
    minibuffer is active. However, if the text shown in the active
    minibuffer has the `minibuffer-message` text property
    (see [Properties with Special Meanings](#Special-Properties)) on some character, the message will be
    displayed before the first character having that property.

    This function is by default the only member of the list in
    `set-message-functions`.

Function: **inhibit-message** message [¶](#index-inhibit_002dmessage)
:   If an echo-area message matches any regexp in the list that is
    the value of the user option `inhibit-message-regexps`, this
    function suppresses the display of that message and returns a
    non-`nil` value that is not a string. Thus, if this function is
    in the list `set-message-functions`, the rest of the functions in
    the list will not be called when message matches the regexps in
    `inhibit-message-regexps`. To ensure a matching message
    will never be displayed, make this function be the first element of
    the list in `set-message-functions`.

Function: **set-multi-message** message [¶](#index-set_002dmulti_002dmessage)
:   This function accumulates several echo-area messages emitted one after
    another, and returns them as a single string in which individual
    messages are separated by newlines. Up to `multi-message-max`
    recent messages can be accumulated. The accumulated messages are
    discarded when more than `multi-message-timeout` seconds have
    elapsed since the time the first message was emitted.

Variable: **inhibit-message** [¶](#index-inhibit_002dmessage-1)
:   When this variable is non-`nil`, `message` and related functions
    will not display any messages in the Echo Area, and will also not clear
    previous echo-area messages when `message` is called with a
    `nil` or an empty argument. Echo-area messages are still logged in
    the `*Messages*` buffer, though.

Macro: **with-temp-message** message &rest body [¶](#index-with_002dtemp_002dmessage)
:   This construct displays a message in the echo area temporarily, during
    the execution of body. It displays message, executes
    body, then returns the value of the last body form while restoring
    the previous echo area contents.

Function: **message-or-box** format-string &rest arguments [¶](#index-message_002dor_002dbox)
:   This function displays a message like `message`, but may display it
    in a dialog box instead of the echo area. If this function is called in
    a command that was invoked using the mouse—more precisely, if
    `last-nonmenu-event` (see [Information from the Command Loop](#Command-Loop-Info)) is either
    `nil` or a list—then it uses a dialog box or pop-up menu to
    display the message. Otherwise, it uses the echo area. (This is the
    same criterion that `y-or-n-p` uses to make a similar decision; see
    [Yes-or-No Queries](#Yes_002dor_002dNo-Queries).)

    You can force use of the mouse or of the echo area by binding
    `last-nonmenu-event` to a suitable value around the call.

Function: **message-box** format-string &rest arguments [¶](#index-message_002dbox)
:   This function displays a message like `message`, but uses a dialog
    box (or a pop-up menu) whenever that is possible. If it is impossible
    to use a dialog box or pop-up menu, because the terminal does not
    support them, then `message-box` uses the echo area, like
    `message`.

Function: **display-message-or-buffer** message &optional buffer-name action frame [¶](#index-display_002dmessage_002dor_002dbuffer)
:   This function displays the message message, which may be either a
    string or a buffer. If it is shorter than the maximum height of the
    echo area, as defined by `max-mini-window-height`, it is displayed
    in the echo area, using `message`. Otherwise,
    `display-buffer` is used to show it in a pop-up buffer.

    Returns either the string shown in the echo area, or when a pop-up
    buffer is used, the window used to display it.

    If message is a string, then the optional argument
    buffer-name is the name of the buffer used to display it when a
    pop-up buffer is used, defaulting to `*Message*`. In the case
    where message is a string and displayed in the echo area, it is
    not specified whether the contents are inserted into the buffer anyway.

    The optional arguments action and frame are as for
    `display-buffer`, and only used if a buffer is displayed.

Function: **current-message** [¶](#index-current_002dmessage)
:   This function returns the message currently being displayed in the
    echo area, or `nil` if there is none.

---

Next: [Logging Messages in `*Messages*`](#Logging-Messages), Previous: [Displaying Messages in the Echo Area](#Displaying-Messages), Up: [The Echo Area](#The-Echo-Area)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.4.2 Reporting Operation Progress [¶](#Reporting-Operation-Progress)

When an operation can take a while to finish, you should inform the
user about the progress it makes. This way the user can estimate
remaining time and clearly see that Emacs is busy working, not hung.
A convenient way to do this is to use a *progress reporter*.

Here is a working example that does nothing useful:

```
(let ((progress-reporter
       (make-progress-reporter "Collecting mana for Emacs..."
                               0  500)))
  (dotimes (k 500)
    (sit-for 0.01)
    (progress-reporter-update progress-reporter k))
  (progress-reporter-done progress-reporter))
```

Function: **make-progress-reporter** message &optional min-value max-value current-value min-change min-time context [¶](#index-make_002dprogress_002dreporter)
:   This function creates and returns a progress reporter object, which
    you will use as an argument for the other functions listed below. The
    idea is to precompute as much data as possible to make progress
    reporting very fast.

    When this progress reporter is subsequently used, it will display
    message in the echo area, followed by progress percentage.
    message is treated as a simple string. If you need it to depend
    on a filename, for instance, use `format-message` before calling this
    function.

    The arguments min-value and max-value should be numbers
    standing for the starting and final states of the operation. For
    instance, an operation that scans a buffer should set these to the
    results of `point-min` and `point-max` correspondingly.
    max-value should be greater than min-value.

    Alternatively, you can set min-value and max-value to
    `nil`. In that case, the progress reporter does not report
    process percentages; it instead displays a “spinner” that rotates a
    notch each time you update the progress reporter.

    If min-value and max-value are numbers, you can give the
    argument current-value a numerical value specifying the initial
    progress; if omitted, this defaults to min-value.

    The arguments min-change and min-time control the rate of
    echo area updates. The progress reporter will wait for at least
    min-change more percents of the operation to be completed before
    printing next message; the default is one percent. min-time
    specifies the minimum time in seconds to pass between successive prints;
    the default is 0.2 seconds. (On some operating systems, the progress
    reporter may handle fractions of seconds with varying precision).

    If context is the symbol `async`, it announces that the updates
    will occur asynchronously. Backends can use that info to prevent the
    progress updates from interfering with other data. For example, the
    backend that displays the progress in the echo area will not display
    those async updates when the echo area is in use.

    This function calls `progress-reporter-update`, so the first
    message is printed immediately.

Function: **progress-reporter-update** reporter &optional value update-text [¶](#index-progress_002dreporter_002dupdate)
:   This function does the main work of reporting progress of your
    operation. It displays the message of reporter, followed by
    progress percentage determined by value. If percentage is zero,
    or close enough according to the min-change and min-time
    arguments, then it is omitted from the output.

    reporter must be the result of a call to
    `make-progress-reporter`. value specifies the current
    state of your operation and must be between min-value and
    max-value (inclusive) as passed to
    `make-progress-reporter`. For instance, if you scan a buffer,
    then value should be the result of a call to `point`.

    Optional argument update-text is a string to be displayed after
    reporter’s main message and progress text. One typical use is as
    the “step” of a long-running process so the user knows where it is.
    If reporter is a non-numerical reporter, then value should
    be `nil`, or a string to use instead of update-text.

    This function respects min-change and min-time as passed
    to `make-progress-reporter` and so does not output new messages
    on every invocation. It is thus very fast and normally you should not
    try to reduce the number of calls to it: resulting overhead will most
    likely negate your effort.

Function: **progress-reporter-force-update** reporter &optional value new-message update-text [¶](#index-progress_002dreporter_002dforce_002dupdate)
:   This function is similar to `progress-reporter-update` except
    that it prints a message in the echo area unconditionally.

    reporter, value, and update-text have the same meaning as for
    `progress-reporter-update`. Optional new-message allows
    you to change the message of the reporter. Since this function
    always updates the echo area, such a change will be immediately
    presented to the user.

Function: **progress-reporter-done** reporter [¶](#index-progress_002dreporter_002ddone)
:   This function should be called when the operation is finished. It
    prints the message of reporter followed by word ‘`done`’ in the
    echo area.

    You should always call this function and not hope for
    `progress-reporter-update` to print ‘`100%`’. Firstly, it may
    never print it, there are many good reasons for this not to happen.
    Secondly, ‘`done`’ is more explicit.

Macro: **dotimes-with-progress-reporter** (var count [result]) reporter-or-message body… [¶](#index-dotimes_002dwith_002dprogress_002dreporter)
:   This is a convenience macro that works the same way as `dotimes`
    does, but also reports loop progress using the functions described
    above. It allows you to save some typing. The argument
    reporter-or-message can be either a string or a progress
    reporter object.

    You can rewrite the example in the beginning of this subsection using
    this macro as follows:

    ```
    (dotimes-with-progress-reporter
        (k 500)
        "Collecting some mana for Emacs..."
      (sit-for 0.01))
    ```

    Using a reporter object as the reporter-or-message argument is
    useful if you want to specify the optional arguments in
    make-progress-reporter. For instance, you can write the
    previous example as follows:

    ```
    (dotimes-with-progress-reporter
        (k 500)
        (make-progress-reporter "Collecting some mana for Emacs..." 0 500 0 1 1.5)
      (sit-for 0.01))
    ```

Macro: **dolist-with-progress-reporter** (var list [result]) reporter-or-message body… [¶](#index-dolist_002dwith_002dprogress_002dreporter)
:   This is another convenience macro that works the same way as `dolist`
    does, but also reports loop progress using the functions described
    above. As in `dotimes-with-progress-reporter`,
    `reporter-or-message` can be a progress reporter or a string.
    You can rewrite the previous example with this macro as follows:

    ```
    (dolist-with-progress-reporter
        (k (number-sequence 0 500))
        "Collecting some mana for Emacs..."
      (sit-for 0.01))
    ```

Macro: **with-delayed-message** (timeout message) body… [¶](#index-with_002ddelayed_002dmessage)
:   Sometimes it’s unclear whether an operation will take a long time to
    execute or not, or it can be inconvenient to implement a progress
    reporter. This macro can be used in those situations.

    ```
    (with-delayed-message (2 (format "Gathering data for %s" entry))
      (setq data (gather-data entry)))
    ```

    In this example, if the body takes more than two seconds to execute,
    the message will be displayed. If it takes a shorter time than that,
    the message won’t be displayed. In either case, the body is evaluated
    as normally, and the return value of the final element in the body is
    the return value of the macro.

    The message element is evaluated before body, and is
    always evaluated, whether the message is displayed or not.

---

Next: [Echo Area Customization](#Echo-Area-Customization), Previous: [Reporting Operation Progress](#Progress), Up: [The Echo Area](#The-Echo-Area)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.4.3 Logging Messages in `*Messages*` [¶](#Logging-Messages-in-_002aMessages_002a)

Almost all the messages displayed in the echo area are also recorded
in the `*Messages*` buffer so that the user can refer back to
them. This includes all the messages that are output with
`message`. By default, this buffer is read-only and uses the major
mode `messages-buffer-mode`. Nothing prevents the user from
killing the `*Messages*` buffer, but the next display of a message
recreates it. Any Lisp code that needs to access the
`*Messages*` buffer directly and wants to ensure that it exists
should use the function `messages-buffer`.

Function: **messages-buffer** [¶](#index-messages_002dbuffer)
:   This function returns the `*Messages*` buffer. If it does not
    exist, it creates it, and switches it to `messages-buffer-mode`.

User Option: **message-log-max** [¶](#index-message_002dlog_002dmax)
:   This variable specifies how many lines to keep in the `*Messages*`
    buffer. The value `t` means there is no limit on how many lines to
    keep. The value `nil` disables message logging entirely. Here’s
    how to display a message and prevent it from being logged:

    ```
    (let (message-log-max)
      (message ...))
    ```

Variable: **messages-buffer-name** [¶](#index-messages_002dbuffer_002dname)
:   This variable has the name of the buffer where messages should be
    logged to, and defaults to `*Messages*`. Some packages may find
    it useful to temporarily redirect the output to a different buffer
    (perhaps to write the buffer out to a log file later), and they can
    bind this variable to a different buffer name. (Note that this buffer
    (if it doesn’t exist already), will be created and put into
    `messages-buffer-mode`.)

To make `*Messages*` more convenient for the user, the logging
facility combines successive identical messages. It also combines
successive related messages for the sake of two cases: question
followed by answer, and a series of progress messages.

A question followed by an answer has two messages like the
ones produced by `y-or-n-p`: the first is ‘`question`’,
and the second is ‘`question...answer`’. The first
message conveys no additional information beyond what’s in the second,
so logging the second message discards the first from the log.

A series of progress messages has successive messages like
those produced by `make-progress-reporter`. They have the form
‘`base...how-far`’, where base is the same each
time, while how-far varies. Logging each message in the series
discards the previous one, provided they are consecutive.

The functions `make-progress-reporter` and `y-or-n-p`
don’t have to do anything special to activate the message log
combination feature. It operates whenever two consecutive messages
are logged that share a common prefix ending in ‘`...`’.

---

Previous: [Logging Messages in `*Messages*`](#Logging-Messages), Up: [The Echo Area](#The-Echo-Area)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.4.4 Echo Area Customization [¶](#Echo-Area-Customization-1)

These variables control details of how the echo area works.

Variable: **cursor-in-echo-area** [¶](#index-cursor_002din_002decho_002darea)
:   This variable controls where the cursor appears when a message is
    displayed in the echo area. If it is non-`nil`, then the cursor
    appears at the end of the message. Otherwise, the cursor appears at
    point—not in the echo area at all.

    The value is normally `nil`; Lisp programs bind it to `t`
    for brief periods of time.

Variable: **echo-area-clear-hook** [¶](#index-echo_002darea_002dclear_002dhook)
:   This normal hook is run whenever the echo area is cleared—either by
    `(message nil)` or for any other reason.

User Option: **echo-keystrokes** [¶](#index-echo_002dkeystrokes)
:   This variable determines how much time should elapse before command
    characters echo. Its value must be a number, and specifies the
    number of seconds to wait before echoing. If the user types a prefix
    key (such as `C-x`) and then delays this many seconds before
    continuing, the prefix key is echoed in the echo area. (Once echoing
    begins in a key sequence, all subsequent characters in the same key
    sequence are echoed immediately.)

    If the value is zero, then command input is not echoed.

Variable: **message-truncate-lines** [¶](#index-message_002dtruncate_002dlines)
:   Normally, displaying a long message resizes the echo area to display
    the entire message, wrapping long line as needed. But if the variable
    `message-truncate-lines` is non-`nil`, long lines of
    echo-area message are instead truncated to fit the mini-window width.

The variable `max-mini-window-height`, which specifies the
maximum height for resizing minibuffer windows, also applies to the
echo area (which is really a special use of the minibuffer window;
see [Minibuffer Windows](#Minibuffer-Windows)).

---

Next: [Invisible Text](#Invisible-Text), Previous: [The Echo Area](#The-Echo-Area), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.5 Reporting Warnings [¶](#Reporting-Warnings)

*Warnings* are a facility for a program to inform the user of a
possible problem, but continue running (as opposed to signaling an
error, see [Errors](#Errors)).

* [Warning Basics](#Warning-Basics)
* [Warning Variables](#Warning-Variables)
* [Warning Options](#Warning-Options)
* [Delayed Warnings](#Delayed-Warnings)

---

Next: [Warning Variables](#Warning-Variables), Up: [Reporting Warnings](#Warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.5.1 Warning Basics [¶](#Warning-Basics-1)

Every warning is a textual message, which explains the problem for
the user, with the associated *severity level* which is a symbol.
Here are the supported severity levels, in order of decreasing
severity, and their meanings:

`:emergency`
:   A problem that will seriously impair Emacs operation soon
    if the user does not attend to it promptly.

`:error`
:   A report about data or circumstances that are inherently wrong.

`:warning`
:   A report about data or circumstances that are not inherently wrong,
    but raise suspicion of a possible problem.

`:debug`
:   A report of information that may be useful if the user is currently
    debugging the Lisp program which issues the warning.

When your program encounters invalid input data, it can either
signal a Lisp error by calling `error` or `signal`
(see [How to Signal an Error](#Signaling-Errors)) or report a warning with severity
`:error`. Signaling a Lisp error is the easiest thing to do, but
it means the signaling program cannot continue execution. If you want
to take the trouble of implementing a way to continue processing
despite the invalid data, then reporting a warning of severity
`:error` is the right way of informing the user of the problem.
For instance, the Emacs Lisp byte compiler can report an error that
way and continue compiling other functions. (If the program signals a
Lisp error and then handles it with `condition-case`, the user
won’t see the error message; reporting that as a warning instead
avoids that problem.)

In addition to severity level, each warning has a *warning type*
to classify it. The warning type is either a symbol or a list of
symbols. If it is a symbol, it should be the custom group that you
use for the program’s user options; if it is a list, the first element
of the list should be that custom group. For example, byte compiler
warnings use the warning type `(bytecomp)`. If the warning type
is a list, the elements of the list after the first one, which should
be arbitrary symbols, represent subcategories of the warning: they
will be displayed to the user to better explain the nature of the
warning.

Function: **display-warning** type message &optional level buffer-name [¶](#index-display_002dwarning)
:   This function reports a warning, using the string message as the
    warning text and type as the warning type. level should
    be the severity level, and defaults to `:warning` if omitted or
    `nil`.

    buffer-name, if non-`nil`, specifies the name of the buffer
    for logging the warning message. By default, it is `*Warnings*`.

Function: **lwarn** type level message &rest args [¶](#index-lwarn)
:   This function reports a warning using the value returned by
    `(format-message message args…)` as the
    message text in the `*Warnings*` buffer. In other respects it is
    equivalent to `display-warning`.

Function: **warn** message &rest args [¶](#index-warn)
:   This function reports a warning using the value returned by
    `(format-message message args…)` as the
    message text, `emacs` as the warning type, and `:warning` as
    the severity level. It exists for compatibility only; we recommend
    not using it, because you should specify a specific warning type.

---

Next: [Warning Options](#Warning-Options), Previous: [Warning Basics](#Warning-Basics), Up: [Reporting Warnings](#Warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.5.2 Warning Variables [¶](#Warning-Variables-1)

Programs can customize how their warnings appear by binding
the variables described in this section.

Variable: **warning-levels** [¶](#index-warning_002dlevels)
:   This list defines the meaning and severity order of the warning
    severity levels. Each element defines one severity level,
    and they are arranged in order of decreasing severity.

    Each element has the form `(level string [function])`, where level is the severity level it
    defines. string specifies the textual description of this
    level. string should use ‘`%s`’ to specify where to put the
    warning type information, or it can omit the ‘`%s`’ so as not to
    include that information.

    The optional function, if non-`nil`, is a function to call
    with no arguments, to get the user’s attention. A notable example is
    `ding` (see [Beeping](#Beeping)).

    Normally you should not change the value of this variable.

Variable: **warning-prefix-function** [¶](#index-warning_002dprefix_002dfunction)
:   If non-`nil`, the value is a function to generate prefix text for
    warnings. Programs can bind the variable to a suitable function.
    `display-warning` calls this function with the warnings buffer
    the current buffer, and the function can insert text into it. That
    text becomes the beginning of the warning message.

    The function is called with two arguments, the severity level and its
    entry in `warning-levels`. It should return a list to use
    *instead* of that entry (the value need not be an actual member
    of `warning-levels`, but it must have the same structure). By
    constructing this value, the function can change the severity of the
    warning, or specify different handling for a given severity level.

    If the variable’s value is `nil`, there’s no prefix text, before
    the warning is displayed, starting with the string part of the
    entry in `warning-levels` corresponding to the warning’s level.

Variable: **warning-series** [¶](#index-warning_002dseries)
:   Programs can bind this variable to `t` to say that the next
    warning should begin a series. When several warnings form a series,
    that means to leave point on the first warning of the series, rather
    than keep moving it for each warning so that it appears on the last one.
    The series ends when the local binding of this variable is unbound and
    `warning-series` becomes `nil` again.

    The value can also be a symbol with a function definition. That is
    equivalent to `t`, except that the next warning will also call
    the function with no arguments with the warnings buffer the current
    buffer. The function can, for example, insert text which will serve
    as a header for the series of warnings.

    Once a series has begun, the value of this variable is a marker which
    points to the buffer position in the warnings buffer of the start of
    the series.

    The variable’s normal value is `nil`, which means to handle
    each warning separately.

Variable: **warning-fill-prefix** [¶](#index-warning_002dfill_002dprefix)
:   When this variable is non-`nil`, it specifies a fill prefix to
    use for filling the text of each warning.

Variable: **warning-fill-column** [¶](#index-warning_002dfill_002dcolumn)
:   The column at which to fill warnings.

Variable: **warning-type-format** [¶](#index-warning_002dtype_002dformat)
:   This variable specifies the format for displaying the warning type
    in the warning text. The result of formatting the type this way
    gets included in the message under the control of the string in the
    entry in `warning-levels`. The default value is `" (%s)"`.
    If you bind it to the empty string `""` then the warning type
    won’t appear at all.

---

Next: [Delayed Warnings](#Delayed-Warnings), Previous: [Warning Variables](#Warning-Variables), Up: [Reporting Warnings](#Warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.5.3 Warning Options [¶](#Warning-Options-1)

These variables are used by users to control what happens
when a Lisp program reports a warning.

User Option: **warning-display-at-bottom** [¶](#index-warning_002ddisplay_002dat_002dbottom)
:   This user option controls the window in which the warnings buffer is
    shown. By default, the value is `t`, and Emacs displays the
    warnings buffer in a window at the bottom of the selected frame,
    creating a new window there if needed. If customized to `nil`, the
    warnings buffer will be shown using the default rules of
    `display-buffer` (see [Choosing a Window for Displaying a Buffer](#Choosing-Window)); in that case the
    `warning` category can be used in `display-buffer-alist` to
    customize how `display-buffer` will display these buffers
    (see [Action Alists for Buffer Display](#Buffer-Display-Action-Alists)).

User Option: **warning-minimum-level** [¶](#index-warning_002dminimum_002dlevel)
:   This user option specifies the minimum severity level that should be
    shown immediately to the user, by popping the warnings buffer in some
    window. The default is `:warning`, which means to show the
    warning buffer for any warning severity except `:debug`. The
    warnings of lower severity levels will still be written into the
    warnings buffer, but the buffer will not be forced onto display.

User Option: **warning-minimum-log-level** [¶](#index-warning_002dminimum_002dlog_002dlevel)
:   This user option specifies the minimum severity level that should be
    logged in the warnings buffer. Warnings of lower severity will be
    completely ignored: not written to the warnings buffer and not
    displayed. The default is `:warning`, which means to log
    warnings of any severity except `:debug`.

User Option: **warning-suppress-types** [¶](#index-warning_002dsuppress_002dtypes)
:   This list specifies which warning types should not be displayed
    immediately when they occur. Each element of the list should be a
    list of symbols. If an element of this list has the same elements as
    the first elements in a warning type, then the warning of that type
    will not be shown on display by popping the warnings buffer in some
    window (the warning will still be logged in the warnings buffer).

    For example, if the value of this variable is a list like this:

    ```
    ((foo) (bar subtype))
    ```

    then warnings whose types are `foo` or `(foo)` or
    `(foo something)` or `(bar subtype other)` will not
    be shown to the user.

User Option: **warning-suppress-log-types** [¶](#index-warning_002dsuppress_002dlog_002dtypes)
:   This list specifies which warning types should be ignored: not logged
    in the warnings buffer and not shown to the user. The structure and
    the matching of warning types are the same as for
    `warning-suppress-types` above.

During startup, Emacs delays showing any warnings until after it
loads and processes the site-wide and user’s init files
(see [Summary: Sequence of Actions at Startup](#Startup-Summary)). Let-binding (see [Local Variables](#Local-Variables)) the
values of these options around some code in your init files which
might emit a warning will therefore not work, because it will not be
in effect by the time the warning is actually processed. Thus, if you
want to suppress some warnings during startup, change the values of
the above options in your init file early enough, or put those
let-binding forms in your `after-init-hook` or
`emacs-startup-hook` functions. See [The Init File](#Init-File).

---

Previous: [Warning Options](#Warning-Options), Up: [Reporting Warnings](#Warnings)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 42.5.4 Delayed Warnings [¶](#Delayed-Warnings-1)

Sometimes, you may wish to avoid showing a warning while a command is
running, and only show it only after the end of the command. You can
use the function `delay-warning` for this. Emacs automatically
delays any warnings emitted during the early stages of startup, and
shows them only after the init files are processed.

Function: **delay-warning** type message &optional level buffer-name [¶](#index-delay_002dwarning)
:   This function is the delayed counterpart to `display-warning`
    (see [Warning Basics](#Warning-Basics)), and it is called with the same arguments.
    The warning message is queued into `delayed-warnings-list`.

Variable: **delayed-warnings-list** [¶](#index-delayed_002dwarnings_002dlist)
:   The value of this variable is a list of warnings to be displayed after
    the current command has finished. Each element must be a list

    ```
    (type message [level [buffer-name]])
    ```

    with the same form, and the same meanings, as the argument list of
    `display-warning`. Immediately after running
    `post-command-hook` (see [Command Loop Overview](#Command-Overview)), the Emacs
    command loop displays all the warnings specified by this variable,
    then resets the variable to `nil`.

Programs which need to further customize the delayed warnings
mechanism can change the variable `delayed-warnings-hook`:

Variable: **delayed-warnings-hook** [¶](#index-delayed_002dwarnings_002dhook)
:   This is a normal hook which is run by the Emacs command loop, after
    `post-command-hook`, in order to process and display delayed
    warnings. Emacs also runs this hook during startup, after loading the
    site-start and user init files (see [Summary: Sequence of Actions at Startup](#Startup-Summary)), because
    warnings emitted before that are automatically delayed.

    Its default value is a list of two functions:

    ```
    (collapse-delayed-warnings display-delayed-warnings)
    ```

    The function `collapse-delayed-warnings` removes repeated entries
    from `delayed-warnings-list`. The function
    `display-delayed-warnings` calls `display-warning` on each
    of the entries in `delayed-warnings-list`, in turn, and then sets
    `delayed-warnings-list` to `nil`.

---

Next: [Selective Display](#Selective-Display), Previous: [Reporting Warnings](#Warnings), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 42.6 Invisible Text [¶](#Invisible-Text-1)

You can make characters *invisible*, so that they do not appear on
the screen, with the `invisible` property. This can be either a
text property (see [Text Properties](#Text-Properties)) or an overlay property
(see [Overlays](#Overlays)). Cursor motion also partly ignores these
characters; if the command loop finds that point is inside a range of
invisible text after a command, it relocates point to the other side
of the text.

In the simplest case, any non-`nil` `invisible` property makes
a character invisible. This is the default case—if you don’t alter
the default value of `buffer-invisibility-spec`, this is how the
`invisible` property works. You should normally use `t`
as the value of the `invisible` property if you don’t plan
to set `buffer-invisibility-spec` yourself.

More generally, you can use the variable `buffer-invisibility-spec`
to control which values of the `invisible` property make text
invisible. This permits you to classify the text into different subsets
in advance, by giving them different `invisible` values, and
subsequently make various subsets visible or invisible by changing the
value of `buffer-invisibility-spec`.

Controlling visibility with `buffer-invisibility-spec` is
especially useful in a program to display the list of entries in a
database. It permits the implementation of convenient filtering
commands to view just a part of the entries in the database. Setting
this variable is very fast, much faster than scanning all the text in
the buffer looking for properties to change.

Variable: **buffer-invisibility-spec** [¶](#index-buffer_002dinvisibility_002dspec)
:   This variable specifies which kinds of `invisible` properties
    actually make a character invisible. Setting this variable makes it
    buffer-local.

    `t`
    :   A character is invisible if its `invisible` property is
        non-`nil`. This is the default.

    a list
    :   Each element of the list specifies a criterion for invisibility; if a
        character’s `invisible` property fits any one of these criteria,
        the character is invisible. The list can have two kinds of elements:

        `atom`
        :   A character is invisible if its `invisible` property value is
            atom or if it is a list with atom as a member; comparison
            is done with `eq`.

        `(atom . t)`
        :   A character is invisible if its `invisible` property value is
            atom or if it is a list with atom as a member; comparison
            is done with `eq`. Moreover, a sequence of such characters
            displays as an ellipsis.

Two functions are specifically provided for adding elements to
`buffer-invisibility-spec` and removing elements from it.

Function: **add-to-invisibility-spec** element [¶](#index-add_002dto_002dinvisibility_002dspec)
:   This function adds the element element to
    `buffer-invisibility-spec`. If `buffer-invisibility-spec`
    was `t`, it changes to a list, `(t)`, so that text whose
    `invisible` property is `t` remains invisible.

Function: **remove-from-invisibility-spec** element [¶](#index-remove_002dfrom_002dinvisibility_002dspec)
:   This removes the element element from
    `buffer-invisibility-spec`. This does nothing if element
    is not in the list.

A convention for use of `buffer-invisibility-spec` is that a
major mode should use the mode’s own name as an element of
`buffer-invisibility-spec` and as the value of the
`invisible` property:

```
;; If you want to display an ellipsis:
(add-to-invisibility-spec '(my-symbol . t))
;; If you don’t want ellipsis:
(add-to-invisibility-spec 'my-symbol)

(overlay-put (make-overlay beginning end)
             'invisible 'my-symbol)

;; When done with the invisibility:
(remove-from-invisibility-spec '(my-symbol . t))
;; Or respectively:
(remove-from-invisibility-spec 'my-symbol)
```

You can check for invisibility using the following function:

Function: **invisible-p** pos-or-prop [¶](#index-invisible_002dp)
:   If pos-or-prop is a marker or number, this function returns a
    non-`nil` value if the text at that position is currently
    invisible.

    If pos-or-prop is any other kind of Lisp object, that is taken
    to mean a possible value of the `invisible` text or overlay
    property. In that case, this function returns a non-`nil` value
    if that value would cause text to become invisible, based on the
    current value of `buffer-invisibility-spec`.

    The return value of this function is `t` if the text would be
    completely hidden on display, or a non-`nil`, non-`t` value
    if the text would be replaced by an ellipsis.

Ordinarily, functions that operate on text or move point do not care
whether the text is invisible, they process invisible characters and
visible characters alike. The user-level line motion commands,
such as `next-line`, `previous-line`, ignore invisible
newlines if `line-move-ignore-invisible` is non-`nil` (the
default), i.e., behave like these invisible newlines didn’t exist in
the buffer, but only because they are explicitly programmed to do so.

If a command ends with point inside or at the boundary of
invisible text, the main editing loop relocates point to one of the
two ends of the invisible text. Emacs chooses the direction of
relocation so that it is the same as the overall movement direction of
the command; if in doubt, it prefers a position where an inserted char
would not inherit the `invisible` property. Additionally, if the
text is not replaced by an ellipsis and the command only moved within
the invisible text, then point is moved one extra character so as to
try and reflect the command’s movement by a visible movement of the
cursor.

Thus, if the command moved point back to an invisible range (with the usual
stickiness), Emacs moves point back to the beginning of that range. If the
command moved point forward into an invisible range, Emacs moves point forward
to the first visible character that follows the invisible text and then forward
one more character.

These *adjustments* of point that ended up in the middle of
invisible text can be disabled by setting `disable-point-adjustment`
to a non-`nil` value. See [Adjusting Point After Commands](#Adjusting-Point).

Incremental search can make invisible overlays visible temporarily
and/or permanently when a match includes invisible text. To enable
this, the overlay should have a non-`nil`
`isearch-open-invisible` property. The property value should be a
function to be called with the overlay as an argument. This function
should make the overlay visible permanently; it is used when the match
overlaps the overlay on exit from the search.

During the search, such overlays are made temporarily visible by
temporarily modifying their invisible and intangible properties. If you
want this to be done differently for a certain overlay, give it an
`isearch-open-invisible-temporary` property which is a function.
The function is called with two arguments: the first is the overlay, and
the second is `nil` to make the overlay visible, or `t` to
make it invisible again.

The `invisible` property is ignored in text that is covered by a
*replacing* `display` property, because such `display`
properties skip the text without processing its properties.
See [Display Specs That Replace The Text](#Replacing-Specs).

---

Next: [Temporary Displays](#Temporary-Displays), Previous: [Invisible Text](#Invisible-Text), Up: [Emacs Display](#Display)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
