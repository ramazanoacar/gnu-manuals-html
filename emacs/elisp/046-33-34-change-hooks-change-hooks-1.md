### 33.34 Change Hooks [¶](#Change-Hooks-1)

These hook variables let you arrange to take notice of changes in
buffers (or in a particular buffer, if you make them buffer-local).
See also [Properties with Special Meanings](#Special-Properties), for how to detect changes to
specific parts of the text.

The functions you use in these hooks should save and restore the match
data if they do anything that uses regular expressions; otherwise, they
will interfere in bizarre ways with the editing operations that call
them. In addition, the functions in these hooks should avoid changing
buffer text, faces, properties, overlays, and other aspects of the
buffer-specific state except those that the hook functions themselves
create and manage, because other parts of Emacs might become confused by
such changes behind their back.

Variable: **before-change-functions** [¶](#index-before_002dchange_002dfunctions)
:   This variable holds a list of functions to call when Emacs is about to
    modify a buffer. Each function gets two arguments, the beginning and
    end of the region that is about to change, represented as integers.
    The buffer that is about to change is always the current buffer when
    the function is called.

Variable: **after-change-functions** [¶](#index-after_002dchange_002dfunctions)
:   This variable holds a list of functions to call after Emacs modifies a
    buffer. Each function receives three arguments: the beginning and end
    of the region just changed, and the length of the text that existed
    before the change. All three arguments are integers. The buffer that
    has been changed is always the current buffer when the function is
    called.

    The length of the old text is the difference between the buffer
    positions before and after that text as it was before the change. As
    for the changed text, its length is simply the difference between the
    first two arguments.

Output of messages into the `*Messages*` buffer does not call
these functions, and neither do certain internal buffer changes, such
as changes in buffers created by Emacs internally for certain jobs,
that should not be visible to Lisp programs.

The vast majority of buffer changing primitives will call
`before-change-functions` and `after-change-functions` in
balanced pairs, once for each change, where the arguments to these
hooks exactly delimit the change being made. Yet, hook functions
should not rely on this always being the case, because some complex
primitives call `before-change-functions` once before making
changes, and then call `after-change-functions` zero or more
times, depending on how many individual changes the primitive is
making. When that happens, the arguments to
`before-change-functions` will enclose a region in which the
individual changes are made, but won’t necessarily be the minimal such
region, and the arguments to each successive call of
`after-change-functions` will then delimit the part of text being
changed exactly. In general, we advise using either the before- or
the after-change hook, but not both.

Macro: **combine-after-change-calls** body… [¶](#index-combine_002dafter_002dchange_002dcalls)
:   The macro executes body normally, but arranges to call the
    after-change functions just once for a series of several changes—if
    that seems safe.

    If a program makes several text changes in the same area of the buffer,
    using the macro `combine-after-change-calls` around that part of
    the program can make it run considerably faster when after-change hooks
    are in use. When the after-change hooks are ultimately called, the
    arguments specify a portion of the buffer including all of the changes
    made within the `combine-after-change-calls` body.

    **Warning:** You must not alter the values of
    `after-change-functions` within
    the body of a `combine-after-change-calls` form.

    **Warning:** If the changes you combine occur in widely scattered
    parts of the buffer, this will still work, but it is not advisable,
    because it may lead to inefficient behavior for some change hook
    functions.

Macro: **combine-change-calls** beg end body… [¶](#index-combine_002dchange_002dcalls)
:   This executes body normally, except any buffer changes it makes
    do not trigger the calls to `before-change-functions` and
    `after-change-functions`. Instead there is a single call of each
    of these hooks for the region enclosed by beg and end, the
    parameters supplied to `after-change-functions` reflecting the
    changes made to the size of the region by body.

    The result of this macro is the result returned by body.

    This macro is useful when a function makes a possibly large number of
    repetitive changes to the buffer, and the change hooks would otherwise
    take a long time to run, were they to be run for each individual
    buffer modification. Emacs itself uses this macro, for example, in
    the commands `comment-region` and `uncomment-region`.

    **Warning:** You must not alter the values of
    `before-change-functions` or `after-change-function` within
    body.

    **Warning:** You must not make any buffer changes outside of the
    region specified by beg and end.

Variable: **first-change-hook** [¶](#index-first_002dchange_002dhook)
:   This variable is a normal hook that is run whenever a buffer is changed
    that was previously in the unmodified state.

Variable: **inhibit-modification-hooks** [¶](#index-inhibit_002dmodification_002dhooks)
:   If this variable is non-`nil`, all of the change hooks are
    disabled; none of them run. This affects all the hook variables
    described above in this section, as well as the hooks attached to
    certain special text properties (see [Properties with Special Meanings](#Special-Properties)) and overlay
    properties (see [Overlay Properties](#Overlay-Properties)).

    Also, this variable is bound to non-`nil` while running those
    same hook variables, so that by default modifying the buffer from
    a modification hook does not cause other modification hooks to be run.
    If you do want modification hooks to be run in a particular piece of
    code that is itself run from a modification hook, then rebind locally
    `inhibit-modification-hooks` to `nil`. However, doing this
    may cause recursive calls to the modification hooks, so be sure to
    prepare for that (for example, by binding some variable which tells
    your hook to do nothing).

    We recommend that you only bind this variable for modifications that
    do not result in lasting changes to buffer text contents (for example
    face changes or temporary modifications). If you need to delay change
    hooks during a series of changes (typically for performance reasons),
    use `combine-change-calls` or `combine-after-change-calls`
    instead.

* [Keeping track of buffer modifications](#Tracking-changes)

---

Up: [Change Hooks](#Change-Hooks)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 33.34.1 Keeping track of buffer modifications [¶](#Keeping-track-of-buffer-modifications)

Using `before-change-functions` and `after-change-functions`
can be difficult in practice because of a number of pitfalls, such as
the fact that the two calls are not always properly paired, or some
calls may be missing, either because some Emacs primitives failed to
properly pair them or because of incorrect use of
`inhibit-modification-hooks`. Furthermore,
many restrictions apply to those hook functions, such as the fact that
they basically should never modify the current buffer, nor use an
operation that may block, and they proceed quickly because
some commands may call these hooks a large number of times.

The Track-Changes library fundamentally provides an alternative API,
built on top of those hooks. Compared to `after-change-functions`,
the first important difference is that, instead of providing the bounds
of the change and the previous length, it provides the bounds of the
change and the actual previous content of that region. The need to
extract information from the original contents of the buffer is one of
the main reasons why some packages need to use both
`before-change-functions` and `after-change-functions` and
then try to match them up.

The second difference is that it decouples the notification of a change
from the act of processing it, and it automatically combines into
a single change operation all the changes that occur between the first
change and the actual processing. This makes it natural and easy to
process the changes at a larger granularity, such as once per command,
and eliminates most of the restrictions that apply to the usual change
hook functions, making it possible to use blocking operations or to
modify the buffer.

To start tracking changes, you have to call
`track-changes-register`, passing it a signal function as
argument. This returns a tracker id which is used to identify
your change tracker to the other functions of the library.
When the buffer is modified, the library calls the signal
function to inform you of that change and immediately starts
accumulating subsequent changes into a single combined change.
The signal function serves only to warn that a modification
occurred but does not receive a description of the change. Also the
library will not call it again until after you retrieved the change.

To retrieve changes, you need to call `track-changes-fetch`, which
provides you with the bounds of the changes accumulated since the
last call, as well as the previous content of that region. It also
“re-arms” the signal function so that the library will call it
again after the next buffer modification.

Function: **track-changes-register** signal &key nobefore disjoint immediate [¶](#index-track_002dchanges_002dregister)
:   This function creates a new *change tracker*. Change trackers are kept
    abstract, so we refer to them as mere identities, and the function thus
    returns the tracker’s id.

    signal is a function that the library will call to notify of
    a change. It will sometimes call it with a single argument and
    sometimes with two. Upon the first change to the buffer since this
    tracker last called `track-changes-fetch`, the library calls this
    signal function with a single argument holding the id of
    the tracker.

    By default, the call to the signal function does not happen
    immediately, but is instead postponed with a 0 seconds timer
    (see [Timers for Delayed Execution](#Timers)). This is usually desired to make sure the signal
    function is not called too frequently and runs in a permissive context,
    freeing the client from performance concerns or worries about which
    operations might be problematic. If a client wants to have more
    control, they can provide a non-`nil` value as the immediate
    argument in which case the library calls the signal function
    directly from `after-change-functions`. Beware that it means that
    the signal function has to be careful not to modify the buffer or
    use operations that may block.

    If you’re not interested in the actual previous content of the buffer,
    but are using this library only for its ability to combine many small
    changes into a larger one and to delay the processing to a more
    convenient time, you can specify a non-`nil` value for the
    nobefore argument. In that case, `track-change-fetch`
    provides you only with the length of the previous content, just like
    `after-change-functions`. It also allows the library to save
    some work.

    While you may like to accumulate many small changes into larger ones,
    you may not want to do that if the changes are too far apart. If you
    specify a non-`nil` value for the disjoint argument, the library
    will let you know when a change is about to occur “far” from the
    currently pending ones by calling the signal function right away,
    passing it two arguments this time: the id of the tracker, and the
    number of characters that separates the upcoming change from the
    already pending changes. This in itself does not prevent combining this
    new change with the previous ones, so if you think the upcoming change
    is indeed too far, you need to call `track-change-fetch`
    right away.
    Beware that when the signal function is called because of
    a disjoint change, this happens directly from
    `before-change-functions`, so the usual restrictions apply about
    modifying the buffer or using operations that may block.

Function: **track-changes-fetch** id func [¶](#index-track_002dchanges_002dfetch)
:   This is the function that lets you find out what has changed in the
    buffer. By providing the tracker id you let the library figure
    out which changes have already been seen by your tracker. Instead of
    returning a description of the changes, `track-changes-fetch` calls
    the func function with that description in the form of
    3 arguments: beg, end, and before, where
    `beg..end` delimit the region that was modified and
    before describes the previous content of that region.
    Usually before is a string containing the previous text of the
    modified region, but if you specified a non-`nil` nobefore argument
    to `track-changes-register`, then it is replaced by the number of
    characters of that previous text.

    In case no changes occurred since the last call,
    `track-changes-fetch` simply does not call func and returns
    `nil`. If changes did occur, it calls func and returns the value
    returned by func. But note that func is called just once
    regardless of how many changes occurred: those are summarized into
    a single beg/end/before triplet.

    In some cases, the library is not properly notified of all changes,
    for example because of a bug in the low-level C code or because of an
    improper use of `inhibit-modification-hooks`. When it detects such
    a problem, func receives a `beg..end` region
    which covers the whole buffer and the before argument is the
    symbol `error` to indicate that the library was unable to determine
    what was changed.

    Once func finishes, `track-changes-fetch` re-enables the
    signal function so that it will be called the next time a change
    occurs. This is the reason why it calls func instead of returning
    a description: it lets you process the change without worrying about the
    risk that the signal function gets triggered in the middle of it,
    because the signal is re-enabled only after func finishes.

Variable: **track-changes-undo-only** [¶](#index-track_002dchanges_002dundo_002donly)
:   If your code uses `track-changes` to perform further modifications
    to the buffer (for example, to mark the parts of the buffer that have
    been edited), then you may want to refrain from making those
    modifications when the changes are the result of an undo (which
    presumably also undoes the modifications you had applied back when the
    corresponding edit was made).
    To that end `track-changes-fetch` binds
    `track-changes-undo-only` to non-`nil` during calls to
    func if the changes were the result of undo.

Function: **track-changes-unregister** id [¶](#index-track_002dchanges_002dunregister)
:   This function tells the library that the tracker id does not need
    to know about buffer changes any more. Most clients will never want to
    stop tracking changes, but for clients such as minor modes, it is
    important to call this function when the minor mode is disabled,
    otherwise the tracker will keep accumulating changes and consume more
    and more resources.

---

Next: [Searching and Matching](#Searching-and-Matching), Previous: [Text](#Text), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 34 Non-ASCII Characters [¶](#Non_002dASCII-Characters-1)

This chapter covers the special issues relating to characters and
how they are stored in strings and buffers.

* [Text Representations](#Text-Representations)
* [Disabling Multibyte Characters](#Disabling-Multibyte)
* [Converting Text Representations](#Converting-Representations)
* [Selecting a Representation](#Selecting-a-Representation)
* [Character Codes](#Character-Codes)
* [Character Properties](#Character-Properties)
* [Character Sets](#Character-Sets)
* [Scanning for Character Sets](#Scanning-Charsets)
* [Translation of Characters](#Translation-of-Characters)
* [Coding Systems](#Coding-Systems)
* [Input Methods](#Input-Methods)
* [Locales](#Locales)

---

Next: [Disabling Multibyte Characters](#Disabling-Multibyte), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.1 Text Representations [¶](#Text-Representations-1)

Emacs buffers and strings support a large repertoire of characters
from many different scripts, allowing users to type and display text
in almost any known written language.

To support this multitude of characters and scripts, Emacs closely
follows the *Unicode Standard*. The Unicode Standard assigns a
unique number, called a *codepoint*, to each and every character.
The range of codepoints defined by Unicode, or the Unicode
*codespace*, is `0..#x10FFFF` (in hexadecimal notation),
inclusive. Emacs extends this range with codepoints in the range
`#x110000..#x3FFFFF`, which it uses for representing characters
that are not unified with Unicode and *raw 8-bit bytes* that
cannot be interpreted as characters. Thus, a character codepoint in
Emacs is a 22-bit integer.

To conserve memory, Emacs does not hold fixed-length 22-bit numbers
that are codepoints of text characters within buffers and strings.
Rather, Emacs uses a variable-length internal representation of
characters, that stores each character as a sequence of 1 to 5 8-bit
bytes, depending on the magnitude of its codepoint[27](#FOOT27). For example, any ASCII character takes up only 1
byte, a Latin-1 character takes up 2 bytes, etc. We call this
representation of text *multibyte*.

Outside Emacs, characters can be represented in many different
encodings, such as ISO-8859-1, GB-2312, Big-5, etc. Emacs converts
between these external encodings and its internal representation, as
appropriate, when it reads text into a buffer or a string, or when it
writes text to a disk file or passes it to some other process.

Occasionally, Emacs needs to hold and manipulate encoded text or
binary non-text data in its buffers or strings. For example, when
Emacs visits a file, it first reads the file’s text verbatim into a
buffer, and only then converts it to the internal representation.
Before the conversion, the buffer holds encoded text.

Encoded text is not really text, as far as Emacs is concerned, but
rather a sequence of raw 8-bit bytes. We call buffers and strings
that hold encoded text *unibyte* buffers and strings, because
Emacs treats them as a sequence of individual bytes. Usually, Emacs
displays unibyte buffers and strings as octal codes such as
`\237`. We recommend that you never use unibyte buffers and
strings except for manipulating encoded text or binary non-text data.

In a buffer, the buffer-local value of the variable
`enable-multibyte-characters` specifies the representation used.
The representation for a string is determined and recorded in the string
when the string is constructed.

Variable: **enable-multibyte-characters** [¶](#index-enable_002dmultibyte_002dcharacters)
:   This variable specifies the current buffer’s text representation.
    If it is non-`nil`, the buffer contains multibyte text; otherwise,
    it contains unibyte encoded text or binary non-text data.

    You cannot set this variable directly; instead, use the function
    `set-buffer-multibyte` to change a buffer’s representation.

Function: **position-bytes** position [¶](#index-position_002dbytes)
:   Buffer positions are measured in character units. This function
    returns the byte-position corresponding to buffer position
    position in the current buffer. This is 1 at the start of the
    buffer, and counts upward in bytes. If position is out of
    range, the value is `nil`.

Function: **byte-to-position** byte-position [¶](#index-byte_002dto_002dposition)
:   Return the buffer position, in character units, corresponding to given
    byte-position in the current buffer. If byte-position is
    out of range, the value is `nil`. In a multibyte buffer, an
    arbitrary value of byte-position can be not at character
    boundary, but inside a multibyte sequence representing a single
    character; in this case, this function returns the buffer position of
    the character whose multibyte sequence includes byte-position.
    In other words, the value does not change for all byte positions that
    belong to the same character.

The following two functions are useful when a Lisp program needs to
map buffer positions to byte offsets in a file visited by the buffer.

Function: **bufferpos-to-filepos** position &optional quality coding-system [¶](#index-bufferpos_002dto_002dfilepos)
:   This function is similar to `position-bytes`, but instead of byte
    position in the current buffer it returns the offset from the
    beginning of the current buffer’s file of the byte that corresponds to
    the given character position in the buffer. The conversion
    requires knowing how the text is encoded in the buffer’s file; this is
    what the coding-system argument is for, defaulting to the value
    of `buffer-file-coding-system`. The optional argument
    quality specifies how accurate the result should be; it should
    be one of the following:

    `exact`
    :   The result must be accurate. The function may need to encode and
        decode a large part of the buffer, which is expensive and can be slow.

    `approximate`
    :   The value can be an approximation. The function may avoid expensive
        processing and return an inexact result.

    `nil`
    :   If the exact result needs expensive processing, the function will
        return `nil` rather than an approximation. This is the default
        if the argument is omitted.

Function: **filepos-to-bufferpos** byte &optional quality coding-system [¶](#index-filepos_002dto_002dbufferpos)
:   This function returns the buffer position corresponding to a file
    position specified by byte, a zero-base byte offset from the
    file’s beginning. The function performs the conversion opposite to
    what `bufferpos-to-filepos` does. Optional arguments
    quality and coding-system have the same meaning and values
    as for `bufferpos-to-filepos`.

Function: **multibyte-string-p** string [¶](#index-multibyte_002dstring_002dp)
:   Return `t` if string is a multibyte string, `nil`
    otherwise. This function also returns `nil` if string is
    some object other than a string.

Function: **string-bytes** string [¶](#index-string_002dbytes)
:   This function returns the number of bytes in string.
    If string is a multibyte string, this can be greater than
    `(length string)`.

Function: **unibyte-string** &rest bytes [¶](#index-unibyte_002dstring)
:   This function concatenates all its argument bytes and makes the
    result a unibyte string.

---

Next: [Converting Text Representations](#Converting-Representations), Previous: [Text Representations](#Text-Representations), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.2 Disabling Multibyte Characters [¶](#Disabling-Multibyte-Characters)

By default, Emacs starts in multibyte mode: it stores the contents
of buffers and strings using an internal encoding that represents
non-ASCII characters using multi-byte sequences. Multibyte
mode allows you to use all the supported languages and scripts without
limitations.

Under very special circumstances, you may want to disable multibyte
character support, for a specific buffer.
When multibyte characters are disabled in a buffer, we call
that *unibyte mode*. In unibyte mode, each character in the
buffer has a character code ranging from 0 through 255 (0377 octal); 0
through 127 (0177 octal) represent ASCII characters, and 128
(0200 octal) through 255 (0377 octal) represent non-ASCII
characters.

To edit a particular file in unibyte representation, visit it using
`find-file-literally`. See [Functions for Visiting Files](#Visiting-Functions). You can
convert a multibyte buffer to unibyte by saving it to a file, killing
the buffer, and visiting the file again with
`find-file-literally`. Alternatively, you can use `C-x
RET c` (`universal-coding-system-argument`) and specify
‘`raw-text`’ as the coding system with which to visit or save a
file. See [Specifying a Coding System for File Text](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Text-Coding) in GNU Emacs Manual. Unlike `find-file-literally`, finding
a file as ‘`raw-text`’ doesn’t disable format conversion,
uncompression, or auto mode selection.

The buffer-local variable `enable-multibyte-characters` is
non-`nil` in multibyte buffers, and `nil` in unibyte ones.
The mode line also indicates whether a buffer is multibyte or not.
With a graphical display, in a multibyte buffer, the portion of the
mode line that indicates the character set has a tooltip that (amongst
other things) says that the buffer is multibyte. In a unibyte buffer,
the character set indicator is absent. Thus, in a unibyte buffer
(when using a graphical display) there is normally nothing before the
indication of the visited file’s end-of-line convention (colon,
backslash, etc.), unless you are using an input method.

You can turn off multibyte support in a specific buffer by invoking the
command `toggle-enable-multibyte-characters` in that buffer.

---

Next: [Selecting a Representation](#Selecting-a-Representation), Previous: [Disabling Multibyte Characters](#Disabling-Multibyte), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.3 Converting Text Representations [¶](#Converting-Text-Representations)

Emacs can convert unibyte text to multibyte; it can also convert
multibyte text to unibyte, provided that the multibyte text contains
only ASCII and 8-bit raw bytes. In general, these
conversions happen when inserting text into a buffer, or when putting
text from several strings together in one string. You can also
explicitly convert a string’s contents to either representation.

Emacs chooses the representation for a string based on the text from
which it is constructed. The general rule is to convert unibyte text
to multibyte text when combining it with other multibyte text, because
the multibyte representation is more general and can hold whatever
characters the unibyte text has.

When inserting text into a buffer, Emacs converts the text to the
buffer’s representation, as specified by
`enable-multibyte-characters` in that buffer. In particular, when
you insert multibyte text into a unibyte buffer, Emacs converts the text
to unibyte, even though this conversion cannot in general preserve all
the characters that might be in the multibyte text. The other natural
alternative, to convert the buffer contents to multibyte, is not
acceptable because the buffer’s representation is a choice made by the
user that cannot be overridden automatically.

Converting unibyte text to multibyte text leaves ASCII
characters unchanged, and converts bytes with codes 128 through 255 to
the multibyte representation of raw eight-bit bytes.

Converting multibyte text to unibyte converts all ASCII
and eight-bit characters to their single-byte form, but loses
information for non-ASCII characters by discarding all but
the low 8 bits of each character’s codepoint. Converting unibyte text
to multibyte and back to unibyte reproduces the original unibyte text.

The next two functions either return the argument string, or a
newly created string with no text properties.

Function: **string-to-multibyte** string [¶](#index-string_002dto_002dmultibyte)
:   This function returns a multibyte string containing the same sequence
    of characters as string. If string is a multibyte string,
    it is returned unchanged. The function assumes that string
    includes only ASCII characters and raw 8-bit bytes; the
    latter are converted to their multibyte representation corresponding
    to the codepoints `#x3FFF80` through `#x3FFFFF`, inclusive
    (see [codepoints](#Text-Representations)).

Function: **string-to-unibyte** string [¶](#index-string_002dto_002dunibyte)
:   This function returns a unibyte string containing the same sequence of
    characters as string. If string is a unibyte string, it
    is returned unchanged. Otherwise, ASCII characters and
    characters in the `eight-bit` charset are converted to their
    corresponding byte values. Use this function for string
    arguments that contain only ASCII and eight-bit characters;
    the function signals an error if any other characters are encountered.

Function: **byte-to-string** byte [¶](#index-byte_002dto_002dstring)
:   This function returns a unibyte string containing a single byte of
    character data, byte. It signals an error if byte is not
    an integer between 0 and 255.

Function: **multibyte-char-to-unibyte** char [¶](#index-multibyte_002dchar_002dto_002dunibyte)
:   This converts the multibyte character char to a unibyte
    character, and returns that character. If char is neither
    ASCII nor eight-bit, the function returns −1.

Function: **unibyte-char-to-multibyte** char [¶](#index-unibyte_002dchar_002dto_002dmultibyte)
:   This converts the unibyte character char to a multibyte
    character, assuming char is either ASCII or raw 8-bit
    byte.

---

Next: [Character Codes](#Character-Codes), Previous: [Converting Text Representations](#Converting-Representations), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.4 Selecting a Representation [¶](#Selecting-a-Representation-1)

Sometimes it is useful to examine an existing buffer or string as
multibyte when it was unibyte, or vice versa.

Function: **set-buffer-multibyte** multibyte [¶](#index-set_002dbuffer_002dmultibyte)
:   Set the representation type of the current buffer. If multibyte
    is non-`nil`, the buffer becomes multibyte. If multibyte
    is `nil`, the buffer becomes unibyte.

    This function leaves the buffer contents unchanged when viewed as a
    sequence of bytes. As a consequence, it can change the contents
    viewed as characters; for instance, a sequence of three bytes which is
    treated as one character in multibyte representation will count as
    three characters in unibyte representation. Eight-bit characters
    representing raw bytes are an exception. They are represented by one
    byte in a unibyte buffer, but when the buffer is set to multibyte,
    they are converted to two-byte sequences, and vice versa.

    This function sets `enable-multibyte-characters` to record which
    representation is in use. It also adjusts various data in the buffer
    (including overlays, text properties and markers) so that they cover the
    same text as they did before.

    This function signals an error if the buffer is narrowed, since the
    narrowing might have occurred in the middle of multibyte character
    sequences.

    This function also signals an error if the buffer is an indirect
    buffer. An indirect buffer always inherits the representation of its
    base buffer.

The following two functions are obsolete and will be removed in a future
version of Emacs; use `encode-coding-string` instead.

Function: **string-as-unibyte** string [¶](#index-string_002das_002dunibyte)
:   If string is already a unibyte string, this function returns
    string itself. Otherwise, it returns a new string with the same
    bytes as string, but treating each byte as a separate character
    (so that the value may have more characters than string); as an
    exception, each eight-bit character representing a raw byte is
    converted into a single byte. The newly-created string contains no
    text properties.

Function: **string-as-multibyte** string [¶](#index-string_002das_002dmultibyte)
:   If string is a multibyte string, this function returns
    string itself. Otherwise, it returns a new string with the same
    bytes as string, but treating each multibyte sequence as one
    character. This means that the value may have fewer characters than
    string has. If a byte sequence in string is invalid as a
    multibyte representation of a single character, each byte in the
    sequence is treated as a raw 8-bit byte. The newly-created string
    contains no text properties.

---

Next: [Character Properties](#Character-Properties), Previous: [Selecting a Representation](#Selecting-a-Representation), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.5 Character Codes [¶](#Character-Codes-1)

The unibyte and multibyte text representations use different
character codes. The valid character codes for unibyte representation
range from 0 to `#xFF` (255)—the values that can fit in one
byte. The valid character codes for multibyte representation range
from 0 to `#x3FFFFF`. In this code space, values 0 through
`#x7F` (127) are for ASCII characters, and values
`#x80` (128) through `#x3FFF7F` (4194175) are for
non-ASCII characters.

Emacs character codes are a superset of the Unicode standard.
Values 0 through `#x10FFFF` (1114111) correspond to Unicode
characters of the same codepoint; values `#x110000` (1114112)
through `#x3FFF7F` (4194175) represent characters that are not
unified with Unicode; and values `#x3FFF80` (4194176) through
`#x3FFFFF` (4194303) represent eight-bit raw bytes.

Function: **characterp** charcode [¶](#index-characterp)
:   This returns `t` if charcode is a valid character, and
    `nil` otherwise.

    ```
    (characterp 65)
         ⇒ t
    ```

    ```
    (characterp 4194303)
         ⇒ t
    ```

    ```
    (characterp 4194304)
         ⇒ nil
    ```

Function: **max-char** &optional unicode [¶](#index-max_002dchar)
:   This function returns the largest value that a valid character
    codepoint can have in Emacs. If the optional argument unicode
    is non-`nil`, it returns the largest character codepoint defined
    by the Unicode Standard (which is smaller than the maximum codepoint
    supported by Emacs).

    ```
    (characterp (max-char))
         ⇒ t
    ```

    ```
    (characterp (1+ (max-char)))
         ⇒ nil
    ```

Function: **char-from-name** string &optional ignore-case [¶](#index-char_002dfrom_002dname)
:   This function returns the character whose Unicode name is string.
    If ignore-case is non-`nil`, case is ignored in string.
    This function returns `nil` if string does not name a character.

    ```
    ;; U+03A3
    (= (char-from-name "GREEK CAPITAL LETTER SIGMA") #x03A3)
         ⇒ t
    ```

Function: **char-to-name** char [¶](#index-char_002dto_002dname)
:   This function returns the Unicode name of char. It returns
    `nil` if char is not a character or has no Unicode name.

Function: **get-byte** &optional pos string [¶](#index-get_002dbyte)
:   This function returns the byte at character position pos in the
    current buffer. If the current buffer is unibyte, this is literally
    the byte at that position. If the buffer is multibyte, byte values of
    ASCII characters are the same as character codepoints,
    whereas eight-bit raw bytes are converted to their 8-bit codes. The
    function signals an error if the character at pos is
    non-ASCII.

    The optional argument string means to get a byte value from that
    string instead of the current buffer.

---

Next: [Character Sets](#Character-Sets), Previous: [Character Codes](#Character-Codes), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.6 Character Properties [¶](#Character-Properties-1)

A *character property* is a named attribute of a character that
specifies how the character behaves and how it should be handled
during text processing and display. Thus, character properties are an
important part of specifying the character’s semantics.

On the whole, Emacs follows the Unicode Standard in its implementation
of character properties. In particular, Emacs supports the
[Unicode Character Property
Model](https://www.unicode.org/reports/tr23/), and the Emacs character property database is derived from the
Unicode Character Database (UCD). See the
[Character
Properties chapter of the Unicode Standard](https://www.unicode.org/versions/Unicode15.0.0/ch04.pdf), for a detailed
description of Unicode character properties and their meaning. This
section assumes you are already familiar with that chapter of the
Unicode Standard, and want to apply that knowledge to Emacs Lisp
programs.

In Emacs, each property has a name, which is a symbol, and a set of
possible values, whose types depend on the property; if a character
does not have a certain property, the value is `nil`. As a
general rule, the names of character properties in Emacs are produced
from the corresponding Unicode properties by downcasing them and
replacing each ‘`_`’ character with a dash ‘`-`’. For example,
`Canonical_Combining_Class` becomes
`canonical-combining-class`. However, sometimes we shorten the
names to make their use easier.

Some codepoints are left *unassigned* by the
UCD—they don’t correspond to any character. The Unicode
Standard defines default values of properties for such codepoints;
they are mentioned below for each property.

Here is the full list of value types for all the character
properties that Emacs knows about:

`name`
:   Corresponds to the `Name` Unicode property. The value is a
    string consisting of upper-case Latin letters A to Z, digits, spaces,
    and hyphen ‘`-`’ characters. For unassigned codepoints, the value
    is `nil`.

`general-category` [¶](#index-unicode-general-category)
:   Corresponds to the `General_Category` Unicode property. The
    value is a symbol whose name is a 2-letter abbreviation of the
    character’s classification. For unassigned codepoints, the value
    is `Cn`.

`canonical-combining-class`
:   Corresponds to the `Canonical_Combining_Class` Unicode property.
    The value is an integer. For unassigned codepoints, the value
    is zero.

`bidi-class` [¶](#index-bidirectional-class-of-characters)
:   Corresponds to the Unicode `Bidi_Class` property. The value is a
    symbol whose name is the Unicode *directional type* of the
    character. Emacs uses this property when it reorders bidirectional
    text for display (see [Bidirectional Display](#Bidirectional-Display)). For unassigned
    codepoints, the value depends on the code blocks to which the
    codepoint belongs: most unassigned codepoints get the value of
    `L` (strong L), but some get values of `AL` (Arabic letter)
    or `R` (strong R).

`decomposition`
:   Corresponds to the Unicode properties `Decomposition_Type` and
    `Decomposition_Value`. The value is a list, whose first element
    may be a symbol representing a compatibility formatting tag, such as
    `small`[28](#FOOT28); the other elements are characters that give the
    compatibility decomposition sequence of this character. For
    characters that don’t have decomposition sequences, and for unassigned
    codepoints, the value is a list with a single member, the character
    itself.

`decimal-digit-value`
:   Corresponds to the Unicode `Numeric_Value` property for
    characters whose `Numeric_Type` is ‘`Decimal`’. The value is
    an integer, or `nil` if the character has no decimal digit value.
    For unassigned codepoints, the value is `nil`, which means
    NaN, or “not a number”.

`digit-value`
:   Corresponds to the Unicode `Numeric_Value` property for
    characters whose `Numeric_Type` is ‘`Digit`’. The value is an
    integer. Examples of such characters include compatibility subscript
    and superscript digits, for which the value is the corresponding
    number. For characters that don’t have any numeric value, and for
    unassigned codepoints, the value is `nil`, which means
    NaN.

`numeric-value`
:   Corresponds to the Unicode `Numeric_Value` property for
    characters whose `Numeric_Type` is ‘`Numeric`’. The value of
    this property is a number. Examples of characters that have this
    property include fractions, subscripts, superscripts, Roman numerals,
    currency numerators, and encircled numbers. For example, the value of
    this property for the character U+2155 VULGAR FRACTION ONE
    FIFTH is `0.2`. For characters that don’t have any numeric
    value, and for unassigned codepoints, the value is `nil`, which
    means NaN.

`mirrored` [¶](#index-mirroring-of-characters)
:   Corresponds to the Unicode `Bidi_Mirrored` property. The value
    of this property is a symbol, either `Y` or `N`. For
    unassigned codepoints, the value is `N`.

`mirroring`
:   Corresponds to the Unicode `Bidi_Mirroring_Glyph` property. The
    value of this property is a character whose glyph represents the
    mirror image of the character’s glyph, or `nil` if there’s no
    defined mirroring glyph. All the characters whose `mirrored`
    property is `N` have `nil` as their `mirroring`
    property; however, some characters whose `mirrored` property is
    `Y` also have `nil` for `mirroring`, because no
    appropriate characters exist with mirrored glyphs. Emacs uses this
    property to display mirror images of characters when appropriate
    (see [Bidirectional Display](#Bidirectional-Display)). For unassigned codepoints, the value
    is `nil`.

`paired-bracket`
:   Corresponds to the Unicode `Bidi_Paired_Bracket` property. The
    value of this property is the codepoint of a character’s *paired
    bracket*, or `nil` if the character is not a bracket character.
    This establishes a mapping between characters that are treated as
    bracket pairs by the Unicode Bidirectional Algorithm; Emacs uses this
    property when it decides how to reorder for display parentheses,
    braces, and other similar characters (see [Bidirectional Display](#Bidirectional-Display)).

`bracket-type`
:   Corresponds to the Unicode `Bidi_Paired_Bracket_Type` property.
    For characters whose `paired-bracket` property is non-`nil`,
    the value of this property is a symbol, either `o` (for opening
    bracket characters) or `c` (for closing bracket characters). For
    characters whose `paired-bracket` property is `nil`, the
    value is the symbol `n` (None). Like `paired-bracket`, this
    property is used for bidirectional display.

`old-name`
:   Corresponds to the Unicode `Unicode_1_Name` property. The value
    is a string. For unassigned codepoints, and characters that have no
    value for this property, the value is `nil`.

`iso-10646-comment`
:   Corresponds to the Unicode `ISO_Comment` property. The value is
    either a string or `nil`. For unassigned codepoints, the value
    is `nil`.

`uppercase`
:   Corresponds to the Unicode `Simple_Uppercase_Mapping` property.
    The value of this property is a single character. For unassigned
    codepoints, the value is `nil`, which means the character itself.

`lowercase`
:   Corresponds to the Unicode `Simple_Lowercase_Mapping` property.
    The value of this property is a single character. For unassigned
    codepoints, the value is `nil`, which means the character itself.

`titlecase`
:   Corresponds to the Unicode `Simple_Titlecase_Mapping` property.
    *Title case* is a special form of a character used when the first
    character of a word needs to be capitalized. The value of this
    property is a single character. For unassigned codepoints, the value
    is `nil`, which means the character itself.

`special-uppercase`
:   Corresponds to Unicode language- and context-independent special upper-casing
    rules. The value of this property is a string (which may be empty). For
    example for U+00DF LATIN SMALL LETTER SHARP S the value is
    `"SS"`. This mapping overrides the `uppercase` property, and
    thus the current case table. For characters with no special mapping,
    the value is `nil`, which means the `uppercase` property needs to
    be consulted instead.

`special-lowercase`
:   Corresponds to Unicode language- and context-independent special
    lower-casing rules. The value of this property is a string (which may
    be empty). For example for U+0130 LATIN CAPITAL LETTER I
    WITH DOT ABOVE the value is `"i\u0307"` (i.e., a 2-character string
    consisting of LATIN SMALL LETTER I followed by U+0307
    COMBINING DOT ABOVE). This mapping overrides the `lowercase`
    property, and thus the current case table. For characters with no
    special mapping, the value is `nil`, which means the `lowercase`
    property needs to be consulted instead.

`special-titlecase`
:   Corresponds to Unicode unconditional special title-casing rules. The value of
    this property is a string (which may be empty). For example for
    U+FB01 LATIN SMALL LIGATURE FI the value is `"Fi"`. This
    mapping overrides the `titlecase` property, and thus the current
    case table. For characters with no special mapping, the value is
    `nil`, which means the `titlecase` property needs to be consulted
    instead.

Function: **get-char-code-property** char propname [¶](#index-get_002dchar_002dcode_002dproperty)
:   This function returns the value of char’s propname property.

    ```
    (get-char-code-property ?\s 'general-category)
         ⇒ Zs
    ```

    ```
    (get-char-code-property ?1 'general-category)
         ⇒ Nd
    ```

    ```
    ;; U+2084
    (get-char-code-property ?\N{SUBSCRIPT FOUR}
                            'digit-value)
         ⇒ 4
    ```

    ```
    ;; U+2155
    (get-char-code-property ?\N{VULGAR FRACTION ONE FIFTH}
                            'numeric-value)
         ⇒ 0.2
    ```

    ```
    ;; U+2163
    (get-char-code-property ?\N{ROMAN NUMERAL FOUR}
                            'numeric-value)
         ⇒ 4
    ```

    ```
    (get-char-code-property ?\( 'paired-bracket)
         ⇒ 41  ; closing parenthesis
    ```

    ```
    (get-char-code-property ?\) 'bracket-type)
         ⇒ c
    ```

Function: **char-code-property-description** prop value [¶](#index-char_002dcode_002dproperty_002ddescription)
:   This function returns the description string of property prop’s
    value, or `nil` if value has no description.

    ```
    (char-code-property-description 'general-category 'Zs)
         ⇒ "Separator, Space"
    ```

    ```
    (char-code-property-description 'general-category 'Nd)
         ⇒ "Number, Decimal Digit"
    ```

    ```
    (char-code-property-description 'numeric-value '1/5)
         ⇒ nil
    ```

Function: **put-char-code-property** char propname value [¶](#index-put_002dchar_002dcode_002dproperty)
:   This function stores value as the value of the property
    propname for the character char.

Variable: **unicode-category-table** [¶](#index-unicode_002dcategory_002dtable)
:   The value of this variable is a char-table (see [Char-Tables](#Char_002dTables)) that
    specifies, for each character, its Unicode `General_Category`
    property as a symbol.

Variable: **char-script-table** [¶](#index-char_002dscript_002dtable)
:   The value of this variable is a char-table that specifies, for each
    character, a symbol whose name is the script to which the character
    belongs, according to the Unicode Standard classification of the
    Unicode code space into script-specific blocks. This char-table has a
    single extra slot whose value is the list of all script symbols, so you
    can evaluate

    ```
    (char-table-extra-slot char-script-table 0)
    ```

    to obtain the list of all the scripts known to Emacs. Note that Emacs’s
    classification of characters into scripts is not a 1-for-1 reflection of
    the Unicode standard, for example there is no ‘`symbol`’ script in
    Unicode.

Variable: **char-width-table** [¶](#index-char_002dwidth_002dtable)
:   The value of this variable is a char-table that specifies the width of
    each character in columns that it will occupy on the screen.

Variable: **printable-chars** [¶](#index-printable_002dchars)
:   The value of this variable is a char-table that specifies, for each
    character, whether it is printable or not. That is, if evaluating
    `(aref printable-chars char)` results in `t`, the character
    is printable, and if it results in `nil`, it is not. The
    definition of what is a printable character is the same as for the
    ‘`[:print:]`’ character class in regular expressions, see [Character Classes](#Char-Classes).

---

Next: [Scanning for Character Sets](#Scanning-Charsets), Previous: [Character Properties](#Character-Properties), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.7 Character Sets [¶](#Character-Sets-1)

An Emacs *character set*, or *charset*, is a set of characters
in which each character is assigned a numeric code point. (The
Unicode Standard calls this a *coded character set*.) Each Emacs
charset has a name which is a symbol. A single character can belong
to any number of different character sets, but it will generally have
a different code point in each charset. Examples of character sets
include `ascii`, `iso-8859-1`, `greek-iso8859-7`, and
`windows-1255`. The code point assigned to a character in a
charset is usually different from its code point used in Emacs buffers
and strings.

Emacs defines several special character sets. The character set
`unicode` includes all the characters whose Emacs code points are
in the range `0..#x10FFFF`. The character set `emacs`
includes all ASCII and non-ASCII characters.
Finally, the `eight-bit` charset includes the 8-bit raw bytes;
Emacs uses it to represent raw bytes encountered in text.

Function: **charsetp** object [¶](#index-charsetp)
:   Returns `t` if object is a symbol that names a character set,
    `nil` otherwise.

Variable: **charset-list** [¶](#index-charset_002dlist)
:   The value is a list of all defined character set names.

Function: **charset-priority-list** &optional highestp [¶](#index-charset_002dpriority_002dlist)
:   This function returns a list of all defined character sets ordered by
    their priority. If highestp is non-`nil`, the function
    returns a single character set of the highest priority.

Function: **set-charset-priority** &rest charsets [¶](#index-set_002dcharset_002dpriority)
:   This function makes charsets the highest priority character sets.

Function: **char-charset** character &optional restriction [¶](#index-char_002dcharset)
:   This function returns the name of the character set of highest
    priority that character belongs to. ASCII characters
    are an exception: for them, this function always returns `ascii`.

    If restriction is non-`nil`, it should be a list of
    charsets to search. Alternatively, it can be a coding system, in
    which case the returned charset must be supported by that coding
    system (see [Coding Systems](#Coding-Systems)).

Function: **charset-plist** charset [¶](#index-charset_002dplist)
:   This function returns the property list of the character set
    charset. Although charset is a symbol, this is not the
    same as the property list of that symbol. Charset properties include
    important information about the charset, such as its documentation
    string, short name, etc.

Function: **put-charset-property** charset propname value [¶](#index-put_002dcharset_002dproperty)
:   This function sets the propname property of charset to the
    given value.

Function: **get-charset-property** charset propname [¶](#index-get_002dcharset_002dproperty)
:   This function returns the value of charsets property
    propname.

Command: **list-charset-chars** charset [¶](#index-list_002dcharset_002dchars)
:   This command displays a list of characters in the character set
    charset.

Emacs can convert between its internal representation of a character
and the character’s codepoint in a specific charset. The following
two functions support these conversions.

Function: **decode-char** charset code-point [¶](#index-decode_002dchar)
:   This function decodes a character that is assigned a code-point
    in charset, to the corresponding Emacs character, and returns
    it. If charset doesn’t contain a character of that code point,
    the value is `nil`.

    For backward compatibility, if code-point doesn’t fit in a Lisp
    fixnum (see [most-positive-fixnum](#Integer-Basics)), it can be
    specified as a cons cell `(high . low)`, where
    low are the lower 16 bits of the value and high are the
    high 16 bits. This usage is obsolescent.

Function: **encode-char** char charset [¶](#index-encode_002dchar)
:   This function returns the code point assigned to the character
    char in charset. If
    charset doesn’t have a codepoint for char, the value is
    `nil`.

The following function comes in handy for applying a certain
function to all or part of the characters in a charset:

Function: **map-charset-chars** function charset &optional arg from-code to-code [¶](#index-map_002dcharset_002dchars)
:   Call function for characters in charset. function
    is called with two arguments. The first one is a cons cell
    `(from . to)`, where from and to
    indicate a range of characters contained in charset. The second
    argument passed to function is arg, or `nil` if
    arg is omitted.

    By default, the range of codepoints passed to function includes
    all the characters in charset, but optional arguments
    from-code and to-code limit that to the range of
    characters between these two codepoints of charset. If either
    of them is `nil`, it defaults to the first or last codepoint of
    charset, respectively. Note that from-code and
    to-code are charset’s codepoints, not the Emacs codes of
    characters; by contrast, the values from and to in the
    cons cell passed to function *are* Emacs character codes.
    Those Emacs character codes are either Unicode code points, or Emacs
    internal code points that extend Unicode and are beyond the Unicode
    range of characters `0..#x10FFFF` (see [Text Representations](#Text-Representations)).
    The latter happens rarely, with legacy CJK charsets for codepoints of
    charset which specify characters not yet unified with Unicode.

---

Next: [Translation of Characters](#Translation-of-Characters), Previous: [Character Sets](#Character-Sets), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.8 Scanning for Character Sets [¶](#Scanning-for-Character-Sets)

Sometimes it is useful to find out which character set a particular
character belongs to. One use for this is in determining which coding
systems (see [Coding Systems](#Coding-Systems)) are capable of representing all of
the text in question; another is to determine the font(s) for
displaying that text.

Function: **charset-after** &optional pos [¶](#index-charset_002dafter)
:   This function returns the charset of highest priority containing the
    character at position pos in the current buffer. If pos
    is omitted or `nil`, it defaults to the current value of point.
    If pos is out of range, the value is `nil`.

Function: **find-charset-region** beg end &optional translation [¶](#index-find_002dcharset_002dregion)
:   This function returns a list of the character sets of highest priority
    that contain characters in the current buffer between positions
    beg and end.

    The optional argument translation specifies a translation table
    to use for scanning the text (see [Translation of Characters](#Translation-of-Characters)). If
    it is non-`nil`, then each character in the region is translated
    through this table, and the value returned describes the translated
    characters instead of the characters actually in the buffer.

Function: **find-charset-string** string &optional translation [¶](#index-find_002dcharset_002dstring)
:   This function returns a list of character sets of highest priority
    that contain characters in string. It is just like
    `find-charset-region`, except that it applies to the contents of
    string instead of part of the current buffer.

---

Next: [Coding Systems](#Coding-Systems), Previous: [Scanning for Character Sets](#Scanning-Charsets), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.9 Translation of Characters [¶](#Translation-of-Characters-1)

A *translation table* is a char-table (see [Char-Tables](#Char_002dTables)) that
specifies a mapping of characters into characters. These tables are
used in encoding and decoding, and for other purposes. Some coding
systems specify their own particular translation tables; there are
also default translation tables which apply to all other coding
systems.

A translation table has two extra slots. The first is either
`nil` or a translation table that performs the reverse
translation; the second is the maximum number of characters to look up
for translating sequences of characters (see the description of
`make-translation-table-from-alist` below).

Function: **make-translation-table** &rest translations [¶](#index-make_002dtranslation_002dtable)
:   This function returns a translation table based on the argument
    translations. Each element of translations should be a
    list of elements of the form `(from . to)`; this says
    to translate the character from into to.

    The arguments and the forms in each argument are processed in order,
    and if a previous form already translates to to some other
    character, say to-alt, from is also translated to
    to-alt.

During decoding, the translation table’s translations are applied to
the characters that result from ordinary decoding. If a coding system
has the property `:decode-translation-table`, that specifies the
translation table to use, or a list of translation tables to apply in
sequence. (This is a property of the coding system, as returned by
`coding-system-get`, not a property of the symbol that is the
coding system’s name. See [Basic Concepts of
Coding Systems](#Coding-System-Basics).) Finally, if
`standard-translation-table-for-decode` is non-`nil`, the
resulting characters are translated by that table.

During encoding, the translation table’s translations are applied to
the characters in the buffer, and the result of translation is
actually encoded. If a coding system has property
`:encode-translation-table`, that specifies the translation table
to use, or a list of translation tables to apply in sequence. In
addition, if the variable `standard-translation-table-for-encode`
is non-`nil`, it specifies the translation table to use for
translating the result.

Variable: **standard-translation-table-for-decode** [¶](#index-standard_002dtranslation_002dtable_002dfor_002ddecode)
:   This is the default translation table for decoding. If a coding
    system specifies its own translation tables, the table that is the
    value of this variable, if non-`nil`, is applied after them.

Variable: **standard-translation-table-for-encode** [¶](#index-standard_002dtranslation_002dtable_002dfor_002dencode)
:   This is the default translation table for encoding. If a coding
    system specifies its own translation tables, the table that is the
    value of this variable, if non-`nil`, is applied after them.

Variable: **translation-table-for-input** [¶](#index-translation_002dtable_002dfor_002dinput)
:   Self-inserting characters are translated through this translation
    table before they are inserted. Search commands also translate their
    input through this table, so they can compare more reliably with
    what’s in the buffer.

    This variable automatically becomes buffer-local when set.

Function: **make-translation-table-from-vector** vec [¶](#index-make_002dtranslation_002dtable_002dfrom_002dvector)
:   This function returns a translation table made from vec that is
    an array of 256 elements to map bytes (values 0 through #xFF) to
    characters. Elements may be `nil` for untranslated bytes. The
    returned table has a translation table for reverse mapping in the
    first extra slot, and the value `1` in the second extra slot.

    This function provides an easy way to make a private coding system
    that maps each byte to a specific character. You can specify the
    returned table and the reverse translation table using the properties
    `:decode-translation-table` and `:encode-translation-table`
    respectively in the props argument to
    `define-coding-system`.

Function: **make-translation-table-from-alist** alist [¶](#index-make_002dtranslation_002dtable_002dfrom_002dalist)
:   This function is similar to `make-translation-table` but returns
    a complex translation table rather than a simple one-to-one mapping.
    Each element of alist is of the form `(from
    . to)`, where from and to are either characters or
    vectors specifying a sequence of characters. If from is a
    character, that character is translated to to (i.e., to a
    character or a character sequence). If from is a vector of
    characters, that sequence is translated to to. The returned
    table has a translation table for reverse mapping in the first extra
    slot, and the maximum length of all the from character sequences
    in the second extra slot.

---

Next: [Input Methods](#Input-Methods), Previous: [Translation of Characters](#Translation-of-Characters), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
