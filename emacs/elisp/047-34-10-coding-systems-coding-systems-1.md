### 34.10 Coding Systems [¶](#Coding-Systems-1)

When Emacs reads or writes a file, and when Emacs sends text to a
subprocess or receives text from a subprocess, it normally performs
character code conversion and end-of-line conversion as specified
by a particular *coding system*.

How to define a coding system is an arcane matter, and is not
documented here.

* [Basic Concepts of Coding Systems](#Coding-System-Basics)
* [Encoding and I/O](#Encoding-and-I_002fO)
* [Coding Systems in Lisp](#Lisp-and-Coding-Systems)
* [User-Chosen Coding Systems](#User_002dChosen-Coding-Systems)
* [Default Coding Systems](#Default-Coding-Systems)
* [Specifying a Coding System for One Operation](#Specifying-Coding-Systems)
* [Explicit Encoding and Decoding](#Explicit-Encoding)
* [Terminal I/O Encoding](#Terminal-I_002fO-Encoding)

---

Next: [Encoding and I/O](#Encoding-and-I_002fO), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.1 Basic Concepts of Coding Systems [¶](#Basic-Concepts-of-Coding-Systems)

*Character code conversion* involves conversion between the
internal representation of characters used inside Emacs and some other
encoding. Emacs supports many different encodings, in that it can
convert to and from them. For example, it can convert text to or from
encodings such as Latin 1, Latin 2, Latin 3, Latin 4, Latin 5, and
several variants of ISO 2022. In some cases, Emacs supports several
alternative encodings for the same characters; for example, there are
three coding systems for the Cyrillic (Russian) alphabet: ISO,
Alternativnyj, and KOI8.

Every coding system specifies a particular set of character code
conversions, but the coding system `undecided` is special: it
leaves the choice unspecified, to be chosen heuristically for each
file or string, based on the file’s or string’s data, when they are
decoded or encoded. The coding system `prefer-utf-8` is like
`undecided`, but it prefers to choose `utf-8` when possible.

In general, a coding system doesn’t guarantee roundtrip identity:
decoding a byte sequence using a coding system, then encoding the
resulting text in the same coding system, can produce a different byte
sequence. But some coding systems do guarantee that the byte sequence
will be the same as what you originally decoded. Here are a few
examples:

> iso-8859-1, utf-8, big5, shift\_jis, euc-jp

Encoding buffer text and then decoding the result can also fail to
reproduce the original text. For instance, if you encode a character
with a coding system which does not support that character, the result
is unpredictable, and thus decoding it using the same coding system
may produce a different text. Currently, Emacs can’t report errors
that result from encoding unsupported characters.

*End of line conversion* handles three different conventions used
on various systems for representing end of line in files. The Unix
convention, used on GNU and Unix systems, and macOS, is to use the
linefeed character (also called newline). The DOS convention, used on
MS-Windows and MS-DOS systems, is to use a carriage return and a
linefeed. The Mac convention, used in Classic Mac OS and now rare
outside of legacy software, is to use just carriage return.

*Base coding systems* such as `latin-1` leave the end-of-line
conversion unspecified, to be chosen based on the data. *Variant
coding systems* such as `latin-1-unix`, `latin-1-dos` and
`latin-1-mac` specify the end-of-line conversion explicitly as
well. Most base coding systems have three corresponding variants whose
names are formed by adding ‘`-unix`’, ‘`-dos`’ and ‘`-mac`’.

The coding system `raw-text` is special in that it prevents
character code conversion, and causes the buffer visited with this
coding system to be a unibyte buffer. For historical reasons, you can
save both unibyte and multibyte text with this coding system. When
you use `raw-text` to encode multibyte text, it does perform one
character code conversion: it converts eight-bit characters to their
single-byte external representation. `raw-text` does not specify
the end-of-line conversion, allowing that to be determined as usual by
the data, and has the usual three variants which specify the
end-of-line conversion.

`no-conversion` (and its alias `binary`) is equivalent to
`raw-text-unix`: it specifies no conversion of either character
codes or end-of-line.

The coding system `utf-8-emacs` specifies that the data is
represented in the internal Emacs encoding (see [Text Representations](#Text-Representations)). This is like `raw-text` in that no code
conversion happens, but different in that the result is multibyte
data. The name `emacs-internal` is an alias for
`utf-8-emacs-unix` (so it forces no conversion of end-of-line,
unlike `utf-8-emacs`, which can decode all 3 kinds of
end-of-line conventions). Since this coding system can represent all
the characters supported by Emacs in its buffers and strings, we
recommend using it whenever saving text for internal purposes, such as
caching.

Function: **coding-system-get** coding-system property [¶](#index-coding_002dsystem_002dget)
:   This function returns the specified property of the coding system
    coding-system. Most coding system properties exist for internal
    purposes, but one that you might find useful is `:mime-charset`.
    That property’s value is the name used in MIME for the character coding
    which this coding system can read and write. Examples:

    ```
    (coding-system-get 'iso-latin-1 :mime-charset)
         ⇒ iso-8859-1
    (coding-system-get 'iso-2022-cn :mime-charset)
         ⇒ iso-2022-cn
    (coding-system-get 'cyrillic-koi8 :mime-charset)
         ⇒ koi8-r
    ```

    The value of the `:mime-charset` property is also defined
    as an alias for the coding system.

Function: **coding-system-aliases** coding-system [¶](#index-coding_002dsystem_002daliases)
:   This function returns the list of aliases of coding-system.

---

Next: [Coding Systems in Lisp](#Lisp-and-Coding-Systems), Previous: [Basic Concepts of Coding Systems](#Coding-System-Basics), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.2 Encoding and I/O [¶](#Encoding-and-I_002fO-1)

The principal purpose of coding systems is for use in reading and
writing files. The function `insert-file-contents` uses a coding
system to decode the file data, and `write-region` uses one to
encode the buffer contents.

You can specify the coding system to use either explicitly
(see [Specifying a Coding System for One Operation](#Specifying-Coding-Systems)), or implicitly using a default
mechanism (see [Default Coding Systems](#Default-Coding-Systems)). But these methods may not
completely specify what to do. For example, they may choose a coding
system such as `undecided` which leaves the character code
conversion to be determined from the data. In these cases, the I/O
operation finishes the job of choosing a coding system. Very often
you will want to find out afterwards which coding system was chosen.

Variable: **buffer-file-coding-system** [¶](#index-buffer_002dfile_002dcoding_002dsystem)
:   This buffer-local variable records the coding system used for saving the
    buffer and for writing part of the buffer with `write-region`. If
    the text to be written cannot be safely encoded using the coding system
    specified by this variable, these operations select an alternative
    encoding by calling the function `select-safe-coding-system`
    (see [User-Chosen Coding Systems](#User_002dChosen-Coding-Systems)). If selecting a different encoding
    requires asking the user to specify a coding system,
    `buffer-file-coding-system` is updated to the newly selected coding
    system.

    `buffer-file-coding-system` does *not* affect sending text
    to a subprocess.

Variable: **save-buffer-coding-system** [¶](#index-save_002dbuffer_002dcoding_002dsystem)
:   This variable specifies the coding system for saving the buffer (by
    overriding `buffer-file-coding-system`). Note that it is not used
    for `write-region`.

    When a command to save the buffer starts out to use
    `buffer-file-coding-system` (or `save-buffer-coding-system`),
    and that coding system cannot handle
    the actual text in the buffer, the command asks the user to choose
    another coding system (by calling `select-safe-coding-system`).
    After that happens, the command also updates
    `buffer-file-coding-system` to represent the coding system that
    the user specified.

Variable: **last-coding-system-used** [¶](#index-last_002dcoding_002dsystem_002dused)
:   I/O operations for files and subprocesses set this variable to the
    coding system name that was used. The explicit encoding and decoding
    functions (see [Explicit Encoding and Decoding](#Explicit-Encoding)) set it too.

    **Warning:** Since receiving subprocess output sets this variable,
    it can change whenever Emacs waits; therefore, you should copy the
    value shortly after the function call that stores the value you are
    interested in.

The variable `selection-coding-system` specifies how to encode
selections for the window system. See [Window System Selections](#Window-System-Selections).

Variable: **file-name-coding-system** [¶](#index-file_002dname_002dcoding_002dsystem)
:   The variable `file-name-coding-system` specifies the coding
    system to use for encoding file names. Emacs encodes file names using
    that coding system for all file operations. If
    `file-name-coding-system` is `nil`, Emacs uses a default
    coding system determined by the selected language environment. In the
    default language environment, any non-ASCII characters in
    file names are not encoded specially; they appear in the file system
    using the internal Emacs representation.

**Warning:** if you change `file-name-coding-system` (or
the language environment) in the middle of an Emacs session, problems
can result if you have already visited files whose names were encoded
using the earlier coding system and are handled differently under the
new coding system. If you try to save one of these buffers under the
visited file name, saving may use the wrong file name, or it may get
an error. If such a problem happens, use `C-x C-w` to specify a
new file name for that buffer.

On Windows 2000 and later, Emacs by default uses Unicode APIs to
pass file names to the OS, so the value of
`file-name-coding-system` is largely ignored. Lisp applications
that need to encode or decode file names on the Lisp level should use
`utf-8` coding-system when `system-type` is
`windows-nt`; the conversion of UTF-8 encoded file names to the
encoding appropriate for communicating with the OS is performed
internally by Emacs.

---

Next: [User-Chosen Coding Systems](#User_002dChosen-Coding-Systems), Previous: [Encoding and I/O](#Encoding-and-I_002fO), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.3 Coding Systems in Lisp [¶](#Coding-Systems-in-Lisp)

Here are the Lisp facilities for working with coding systems:

Function: **coding-system-list** &optional base-only [¶](#index-coding_002dsystem_002dlist)
:   This function returns a list of all coding system names (symbols). If
    base-only is non-`nil`, the value includes only the
    base coding systems. Otherwise, it includes alias and variant coding
    systems as well.

Function: **coding-system-p** object [¶](#index-coding_002dsystem_002dp)
:   This function returns `t` if object is a coding system
    name or `nil`.

Function: **check-coding-system** coding-system [¶](#index-check_002dcoding_002dsystem)
:   This function checks the validity of coding-system. If that is
    valid, it returns coding-system. If coding-system is
    `nil`, the function returns `nil`. For any other values, it
    signals an error whose `error-symbol` is `coding-system-error`
    (see [signal](#Signaling-Errors)).

Function: **coding-system-eol-type** coding-system [¶](#index-coding_002dsystem_002deol_002dtype)
:   This function returns the type of end-of-line (a.k.a. *eol*)
    conversion used by coding-system. If coding-system
    specifies a certain eol conversion, the return value is an integer 0,
    1, or 2, standing for `unix`, `dos`, and `mac`,
    respectively. If coding-system doesn’t specify eol conversion
    explicitly, the return value is a vector of coding systems, each one
    with one of the possible eol conversion types, like this:

    ```
    (coding-system-eol-type 'latin-1)
         ⇒ [latin-1-unix latin-1-dos latin-1-mac]
    ```

    If this function returns a vector, Emacs will decide, as part of the
    text encoding or decoding process, what eol conversion to use. For
    decoding, the end-of-line format of the text is auto-detected, and the
    eol conversion is set to match it (e.g., DOS-style CRLF format will
    imply `dos` eol conversion). For encoding, the eol conversion is
    taken from the appropriate default coding system (e.g.,
    default value of `buffer-file-coding-system` for
    `buffer-file-coding-system`), or from the default eol conversion
    appropriate for the underlying platform.

Function: **coding-system-change-eol-conversion** coding-system eol-type [¶](#index-coding_002dsystem_002dchange_002deol_002dconversion)
:   This function returns a coding system which is like coding-system
    except for its eol conversion, which is specified by `eol-type`.
    eol-type should be `unix`, `dos`, `mac`, or
    `nil`. If it is `nil`, the returned coding system determines
    the end-of-line conversion from the data.

    eol-type may also be 0, 1 or 2, standing for `unix`,
    `dos` and `mac`, respectively.

Function: **coding-system-change-text-conversion** eol-coding text-coding [¶](#index-coding_002dsystem_002dchange_002dtext_002dconversion)
:   This function returns a coding system which uses the end-of-line
    conversion of eol-coding, and the text conversion of
    text-coding. If text-coding is `nil`, it returns
    `undecided`, or one of its variants according to eol-coding.

Function: **find-coding-systems-region** from to [¶](#index-find_002dcoding_002dsystems_002dregion)
:   This function returns a list of coding systems that could be used to
    encode a text between from and to. All coding systems in
    the list can safely encode any multibyte characters in that portion of
    the text.

    If the text contains no multibyte characters, the function returns the
    list `(undecided)`.

Function: **find-coding-systems-string** string [¶](#index-find_002dcoding_002dsystems_002dstring)
:   This function returns a list of coding systems that could be used to
    encode the text of string. All coding systems in the list can
    safely encode any multibyte characters in string. If the text
    contains no multibyte characters, this returns the list
    `(undecided)`.

Function: **find-coding-systems-for-charsets** charsets [¶](#index-find_002dcoding_002dsystems_002dfor_002dcharsets)
:   This function returns a list of coding systems that could be used to
    encode all the character sets in the list charsets.

Function: **check-coding-systems-region** start end coding-system-list [¶](#index-check_002dcoding_002dsystems_002dregion)
:   This function checks whether coding systems in the list
    `coding-system-list` can encode all the characters in the region
    between start and end. If all of the coding systems in
    the list can encode the specified text, the function returns
    `nil`. If some coding systems cannot encode some of the
    characters, the value is an alist, each element of which has the form
    `(coding-system1 pos1 pos2 …)`, meaning
    that coding-system1 cannot encode characters at buffer positions
    pos1, pos2, ....

    start may be a string, in which case end is ignored and
    the returned value references string indices instead of buffer
    positions.

Function: **detect-coding-region** start end &optional highest [¶](#index-detect_002dcoding_002dregion)
:   This function chooses a plausible coding system for decoding the text
    from start to end. This text should be a byte sequence,
    i.e., unibyte text or multibyte text with only ASCII and
    eight-bit characters (see [Explicit Encoding and Decoding](#Explicit-Encoding)).

    Normally this function returns a list of coding systems that could
    handle decoding the text that was scanned. They are listed in order of
    decreasing priority. But if highest is non-`nil`, then the
    return value is just one coding system, the one that is highest in
    priority.

    If the region contains only ASCII characters except for such
    ISO-2022 control characters ISO-2022 as `ESC`, the value is
    `undecided` or `(undecided)`, or a variant specifying
    end-of-line conversion, if that can be deduced from the text.

    If the region contains null bytes, the value is `no-conversion`,
    even if the region contains text encoded in some coding system.

Function: **detect-coding-string** string &optional highest [¶](#index-detect_002dcoding_002dstring)
:   This function is like `detect-coding-region` except that it
    operates on the contents of string instead of bytes in the buffer.

Variable: **inhibit-null-byte-detection** [¶](#index-inhibit_002dnull_002dbyte_002ddetection)
:   If this variable has a non-`nil` value, null bytes are ignored
    when detecting the encoding of a region or a string. This allows the
    encoding of text that contains null bytes to be correctly detected,
    such as Info files with Index nodes.

Variable: **inhibit-iso-escape-detection** [¶](#index-inhibit_002diso_002descape_002ddetection)
:   If this variable has a non-`nil` value, ISO-2022 escape sequences
    are ignored when detecting the encoding of a region or a string. The
    result is that no text is ever detected as encoded in some ISO-2022
    encoding, and all escape sequences become visible in a buffer.
    **Warning:** *Use this variable with extreme caution,
    because many files in the Emacs distribution use ISO-2022 encoding.*

Function: **coding-system-charset-list** coding-system [¶](#index-coding_002dsystem_002dcharset_002dlist)
:   This function returns the list of character sets (see [Character Sets](#Character-Sets)) supported by coding-system. Some coding systems that
    support too many character sets to list them all yield special values:

    * If coding-system supports all Emacs characters, the value is
      `(emacs)`.
    * If coding-system supports all Unicode characters, the value is
      `(unicode)`.
    * If coding-system supports all ISO-2022 charsets, the value is
      `iso-2022`.
    * If coding-system supports all the characters in the internal
      coding system used by Emacs version 21 (prior to the implementation of
      internal Unicode support), the value is `emacs-mule`.

See [Process Information](#Coding-systems-for-a-subprocess), in
particular the description of the functions
`process-coding-system` and `set-process-coding-system`, for
how to examine or set the coding systems used for I/O to a subprocess.

---

Next: [Default Coding Systems](#Default-Coding-Systems), Previous: [Coding Systems in Lisp](#Lisp-and-Coding-Systems), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.4 User-Chosen Coding Systems [¶](#User_002dChosen-Coding-Systems-1)

Function: **select-safe-coding-system** from to &optional default-coding-system accept-default-p file [¶](#index-select_002dsafe_002dcoding_002dsystem)
:   This function selects a coding system for encoding specified text,
    asking the user to choose if necessary. Normally the specified text
    is the text in the current buffer between from and to. If
    from is a string, the string specifies the text to encode, and
    to is ignored.

    If the specified text includes raw bytes (see [Text Representations](#Text-Representations)), `select-safe-coding-system` suggests
    `raw-text` for its encoding.

    If default-coding-system is non-`nil`, that is the first
    coding system to try; if that can handle the text,
    `select-safe-coding-system` returns that coding system. It can
    also be a list of coding systems; then the function tries each of them
    one by one. After trying all of them, it next tries the current
    buffer’s value of `buffer-file-coding-system` (if it is not
    `undecided`), then the default value of
    `buffer-file-coding-system` and finally the user’s most
    preferred coding system, which the user can set using the command
    `prefer-coding-system` (see [Recognizing
    Coding Systems](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Recognize-Coding) in The GNU Emacs Manual).

    If one of those coding systems can safely encode all the specified
    text, `select-safe-coding-system` chooses it and returns it.
    Otherwise, it asks the user to choose from a list of coding systems
    which can encode all the text, and returns the user’s choice.

    default-coding-system can also be a list whose first element is
    `t` and whose other elements are coding systems. Then, if no coding
    system in the list can handle the text, `select-safe-coding-system`
    queries the user immediately, without trying any of the three
    alternatives described above. This is handy for checking only the
    coding systems in the list.

    The optional argument accept-default-p determines whether a
    coding system selected without user interaction is acceptable. If
    it’s omitted or `nil`, such a silent selection is always
    acceptable. If it is non-`nil`, it should be a function;
    `select-safe-coding-system` calls this function with one
    argument, the base coding system of the selected coding system. If
    the function returns `nil`, `select-safe-coding-system`
    rejects the silently selected coding system, and asks the user to
    select a coding system from a list of possible candidates.

    If the variable `select-safe-coding-system-accept-default-p` is
    non-`nil`, it should be a function taking a single argument.
    It is used in place of accept-default-p, overriding any
    value supplied for this argument.

    As a final step, before returning the chosen coding system,
    `select-safe-coding-system` checks whether that coding system is
    consistent with what would be selected if the contents of the region
    were read from a file. (If not, this could lead to data corruption in
    a file subsequently re-visited and edited.) Normally,
    `select-safe-coding-system` uses `buffer-file-name` as the
    file for this purpose, but if file is non-`nil`, it uses
    that file instead (this can be relevant for `write-region` and
    similar functions). If it detects an apparent inconsistency,
    `select-safe-coding-system` queries the user before selecting the
    coding system.

Variable: **select-safe-coding-system-function** [¶](#index-select_002dsafe_002dcoding_002dsystem_002dfunction)
:   This variable names the function to be called to request the user to
    select a proper coding system for encoding text when the default
    coding system for an output operation cannot safely encode that text.
    The default value of this variable is `select-safe-coding-system`.
    Emacs primitives that write text to files, such as
    `write-region`, or send text to other processes, such as
    `process-send-region`, normally call the value of this variable,
    unless `coding-system-for-write` is bound to a non-`nil`
    value (see [Specifying a Coding System for One Operation](#Specifying-Coding-Systems)).

Here are two functions you can use to let the user specify a coding
system, with completion. See [Completion](#Completion).

Function: **read-coding-system** prompt &optional default [¶](#index-read_002dcoding_002dsystem)
:   This function reads a coding system using the minibuffer, prompting with
    string prompt, and returns the coding system name as a symbol. If
    the user enters null input, default specifies which coding system
    to return. It should be a symbol or a string.

Function: **read-non-nil-coding-system** prompt [¶](#index-read_002dnon_002dnil_002dcoding_002dsystem)
:   This function reads a coding system using the minibuffer, prompting with
    string prompt, and returns the coding system name as a symbol. If
    the user tries to enter null input, it asks the user to try again.
    See [Coding Systems](#Coding-Systems).

---

Next: [Specifying a Coding System for One Operation](#Specifying-Coding-Systems), Previous: [User-Chosen Coding Systems](#User_002dChosen-Coding-Systems), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.5 Default Coding Systems [¶](#Default-Coding-Systems-1)

This section describes variables that specify the default coding
system for certain files or when running certain subprograms, and the
function that I/O operations use to access them.

The idea of these variables is that you set them once and for all to the
defaults you want, and then do not change them again. To specify a
particular coding system for a particular operation in a Lisp program,
don’t change these variables; instead, override them using
`coding-system-for-read` and `coding-system-for-write`
(see [Specifying a Coding System for One Operation](#Specifying-Coding-Systems)).

User Option: **auto-coding-regexp-alist** [¶](#index-auto_002dcoding_002dregexp_002dalist)
:   This variable is an alist of text patterns and corresponding coding
    systems. Each element has the form `(regexp
    . coding-system)`; a file whose first few kilobytes match
    regexp is decoded with coding-system when its contents are
    read into a buffer. The settings in this alist take priority over
    `coding:` tags in the files and the contents of
    `file-coding-system-alist` (see below). The default value is set
    so that Emacs automatically recognizes mail files in Babyl format and
    reads them with no code conversions.

User Option: **file-coding-system-alist** [¶](#index-file_002dcoding_002dsystem_002dalist)
:   This variable is an alist that specifies the coding systems to use for
    reading and writing particular files. Each element has the form
    `(pattern . coding)`, where pattern is a regular
    expression that matches certain file names. The element applies to file
    names that match pattern.

    The CDR of the element, coding, should be either a coding
    system, a cons cell containing two coding systems, or a function name (a
    symbol with a function definition). If coding is a coding system,
    that coding system is used for both reading the file and writing it. If
    coding is a cons cell containing two coding systems, its CAR
    specifies the coding system for decoding, and its CDR specifies the
    coding system for encoding.

    If coding is a function name, the function should take one
    argument, a list of all arguments passed to
    `find-operation-coding-system`. It must return a coding system
    or a cons cell containing two coding systems. This value has the same
    meaning as described above.

    If coding (or what returned by the above function) is
    `undecided`, the normal code-detection is performed.

User Option: **auto-coding-alist** [¶](#index-auto_002dcoding_002dalist)
:   This variable is an alist that specifies the coding systems to use for
    reading and writing particular files. Its form is like that of
    `file-coding-system-alist`, but, unlike the latter, this variable
    takes priority over any `coding:` tags in the file.

Variable: **process-coding-system-alist** [¶](#index-process_002dcoding_002dsystem_002dalist)
:   This variable is an alist specifying which coding systems to use for a
    subprocess, depending on which program is running in the subprocess. It
    works like `file-coding-system-alist`, except that pattern is
    matched against the program name used to start the subprocess. The coding
    system or systems specified in this alist are used to initialize the
    coding systems used for I/O to the subprocess, but you can specify
    other coding systems later using `set-process-coding-system`.

**Warning:** Coding systems such as `undecided`, which
determine the coding system from the data, do not work entirely reliably
with asynchronous subprocess output. This is because Emacs handles
asynchronous subprocess output in batches, as it arrives. If the coding
system leaves the character code conversion unspecified, or leaves the
end-of-line conversion unspecified, Emacs must try to detect the proper
conversion from one batch at a time, and this does not always work.

Therefore, with an asynchronous subprocess, if at all possible, use a
coding system which determines both the character code conversion and
the end of line conversion—that is, one like `latin-1-unix`,
rather than `undecided` or `latin-1`.

Variable: **network-coding-system-alist** [¶](#index-network_002dcoding_002dsystem_002dalist)
:   This variable is an alist that specifies the coding system to use for
    network streams. It works much like `file-coding-system-alist`,
    with the difference that the pattern in an element may be either a
    port number or a regular expression. If it is a regular expression, it
    is matched against the network service name used to open the network
    stream.

Variable: **default-process-coding-system** [¶](#index-default_002dprocess_002dcoding_002dsystem)
:   This variable specifies the coding systems to use for subprocess (and
    network stream) input and output, when nothing else specifies what to
    do.

    The value should be a cons cell of the form `(input-coding
    . output-coding)`. Here input-coding applies to input from
    the subprocess, and output-coding applies to output to it.

User Option: **auto-coding-functions** [¶](#index-auto_002dcoding_002dfunctions)
:   This variable holds a list of functions that try to determine a
    coding system for a file based on its undecoded contents.

    Each function in this list should be written to look at text in the
    current buffer, but should not modify it in any way. The buffer will
    contain the text of parts of the file. Each function should take one
    argument, size, which tells it how many characters to look at,
    starting from point. If the function succeeds in determining a coding
    system for the file, it should return that coding system. Otherwise,
    it should return `nil`.
    Each function can also find the name of the file to which
    the buffer’s content belong in the variable
    `auto-coding-file-name`.

    The functions in this list could be called either when the file is
    visited and Emacs wants to decode its contents, and/or when the file’s
    buffer is about to be saved and Emacs wants to determine how to encode
    its contents.

    If a file has a ‘`coding:`’ tag, that takes precedence, so these
    functions won’t be called.

Function: **find-auto-coding** filename size [¶](#index-find_002dauto_002dcoding)
:   This function tries to determine a suitable coding system for
    filename. It examines the buffer visiting the named file, using
    the variables documented above in sequence, until it finds a match for
    one of the rules specified by these variables. It then returns a cons
    cell of the form `(coding . source)`, where
    coding is the coding system to use and source is a symbol,
    one of `auto-coding-alist`, `auto-coding-regexp-alist`,
    `:coding`, or `auto-coding-functions`, indicating which one
    supplied the matching rule. The value `:coding` means the coding
    system was specified by the `coding:` tag in the file
    (see [coding tag](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Specify-Coding) in The GNU Emacs Manual).
    The order of looking for a matching rule is `auto-coding-alist`
    first, then `auto-coding-regexp-alist`, then the `coding:`
    tag, and lastly `auto-coding-functions`. If no matching rule was
    found, the function returns `nil`.

    The second argument size is the size of text, in characters,
    following point. The function examines text only within size
    characters after point. Normally, the buffer should be positioned at
    the beginning when this function is called, because one of the places
    for the `coding:` tag is the first one or two lines of the file;
    in that case, size should be the size of the buffer.

Function: **set-auto-coding** filename size [¶](#index-set_002dauto_002dcoding)
:   This function returns a suitable coding system for file
    filename. It uses `find-auto-coding` to find the coding
    system. If no coding system could be determined, the function returns
    `nil`. The meaning of the argument size is like in
    `find-auto-coding`.

Function: **find-operation-coding-system** operation &rest arguments [¶](#index-find_002doperation_002dcoding_002dsystem)
:   This function returns the coding system to use (by default) for
    performing operation with arguments. The value has this
    form:

    ```
    (decoding-system . encoding-system)
    ```

    The first element, decoding-system, is the coding system to use
    for decoding (in case operation does decoding), and
    encoding-system is the coding system for encoding (in case
    operation does encoding).

    The argument operation is a symbol; it should be one of
    `write-region`, `start-process`, `call-process`,
    `call-process-region`, `insert-file-contents`, or
    `open-network-stream`. These are the names of the Emacs I/O
    primitives that can do character code and eol conversion.

    The remaining arguments should be the same arguments that might be given
    to the corresponding I/O primitive. Depending on the primitive, one
    of those arguments is selected as the *target*. For example, if
    operation does file I/O, whichever argument specifies the file
    name is the target. For subprocess primitives, the process name is the
    target. For `open-network-stream`, the target is the service name
    or port number.

    Depending on operation, this function looks up the target in
    `file-coding-system-alist`, `process-coding-system-alist`,
    or `network-coding-system-alist`. If the target is found in the
    alist, `find-operation-coding-system` returns its association in
    the alist; otherwise it returns `nil`.

    If operation is `insert-file-contents`, the argument
    corresponding to the target may be a cons cell of the form
    `(filename . buffer)`. In that case, filename
    is a file name to look up in `file-coding-system-alist`, and
    buffer is a buffer that contains the file’s contents (not yet
    decoded). If `file-coding-system-alist` specifies a function to
    call for this file, and that function needs to examine the file’s
    contents (as it usually does), it should examine the contents of
    buffer instead of reading the file.

---

Next: [Explicit Encoding and Decoding](#Explicit-Encoding), Previous: [Default Coding Systems](#Default-Coding-Systems), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.6 Specifying a Coding System for One Operation [¶](#Specifying-a-Coding-System-for-One-Operation)

You can specify the coding system for a specific operation by binding
the variables `coding-system-for-read` and/or
`coding-system-for-write`.

Variable: **coding-system-for-read** [¶](#index-coding_002dsystem_002dfor_002dread)
:   If this variable is non-`nil`, it specifies the coding system to
    use for reading a file, or for input from a synchronous subprocess.

    It also applies to any asynchronous subprocess or network stream, but in
    a different way: the value of `coding-system-for-read` when you
    start the subprocess or open the network stream specifies the input
    decoding method for that subprocess or network stream. It remains in
    use for that subprocess or network stream unless and until overridden.

    The right way to use this variable is to bind it with `let` for a
    specific I/O operation. Its global value is normally `nil`, and
    you should not globally set it to any other value. Here is an example
    of the right way to use the variable:

    ```
    ;; Read the file with no character code conversion.
    (let ((coding-system-for-read 'no-conversion))
      (insert-file-contents filename))
    ```

    When its value is non-`nil`, this variable takes precedence over
    all other methods of specifying a coding system to use for input,
    including `file-coding-system-alist`,
    `process-coding-system-alist` and
    `network-coding-system-alist`.

Variable: **coding-system-for-write** [¶](#index-coding_002dsystem_002dfor_002dwrite)
:   This works much like `coding-system-for-read`, except that it
    applies to output rather than input. It affects writing to files,
    as well as sending output to subprocesses and net connections. It
    also applies to encoding command-line arguments with which Emacs
    invokes subprocesses.

    When a single operation does both input and output, as do
    `call-process-region` and `start-process`, both
    `coding-system-for-read` and `coding-system-for-write`
    affect it.

Variable: **coding-system-require-warning** [¶](#index-coding_002dsystem_002drequire_002dwarning)
:   Binding `coding-system-for-write` to a non-`nil` value
    prevents output primitives from calling the function specified by
    `select-safe-coding-system-function` (see [User-Chosen Coding Systems](#User_002dChosen-Coding-Systems)). This is because `C-x RET c`
    (`universal-coding-system-argument`) works by binding
    `coding-system-for-write`, and Emacs should obey user selection.
    If a Lisp program binds `coding-system-for-write` to a value that
    might not be safe for encoding the text to be written, it can also bind
    `coding-system-require-warning` to a non-`nil` value, which
    will force the output primitives to check the encoding by calling the
    value of `select-safe-coding-system-function` even though
    `coding-system-for-write` is non-`nil`. Alternatively, call
    `select-safe-coding-system` explicitly before using the specified
    encoding.

User Option: **inhibit-eol-conversion** [¶](#index-inhibit_002deol_002dconversion)
:   When this variable is non-`nil`, no end-of-line conversion is done,
    no matter which coding system is specified. This applies to all the
    Emacs I/O and subprocess primitives, and to the explicit encoding and
    decoding functions (see [Explicit Encoding and Decoding](#Explicit-Encoding)).

Sometimes, you need to prefer several coding systems for some
operation, rather than fix a single one. Emacs lets you specify a
priority order for using coding systems. This ordering affects the
sorting of lists of coding systems returned by functions such as
`find-coding-systems-region` (see [Coding Systems in Lisp](#Lisp-and-Coding-Systems)).

Function: **coding-system-priority-list** &optional highestp [¶](#index-coding_002dsystem_002dpriority_002dlist)
:   This function returns the list of coding systems in the order of their
    current priorities. Optional argument highestp, if
    non-`nil`, means return only the highest priority coding system.

Function: **set-coding-system-priority** &rest coding-systems [¶](#index-set_002dcoding_002dsystem_002dpriority)
:   This function puts coding-systems at the beginning of the
    priority list for coding systems, thus making their priority higher
    than all the rest.

Macro: **with-coding-priority** coding-systems &rest body [¶](#index-with_002dcoding_002dpriority)
:   This macro executes body, like `progn` does
    (see [progn](#Sequencing)), with coding-systems at the front of
    the priority list for coding systems. coding-systems should be
    a list of coding systems to prefer during execution of body.

---

Next: [Terminal I/O Encoding](#Terminal-I_002fO-Encoding), Previous: [Specifying a Coding System for One Operation](#Specifying-Coding-Systems), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.7 Explicit Encoding and Decoding [¶](#Explicit-Encoding-and-Decoding)

All the operations that transfer text in and out of Emacs have the
ability to use a coding system to encode or decode the text.
You can also explicitly encode and decode text using the functions
in this section.

The result of encoding, and the input to decoding, are not ordinary
text. They logically consist of a series of byte values; that is, a
series of ASCII and eight-bit characters. In unibyte
buffers and strings, these characters have codes in the range 0
through #xFF (255). In a multibyte buffer or string, eight-bit
characters have character codes higher than #xFF (see [Text Representations](#Text-Representations)), but Emacs transparently converts them to their
single-byte values when you encode or decode such text.

The usual way to read a file into a buffer as a sequence of bytes, so
you can decode the contents explicitly, is with
`insert-file-contents-literally` (see [Reading from Files](#Reading-from-Files));
alternatively, specify a non-`nil` rawfile argument when
visiting a file with `find-file-noselect`. These methods result in
a unibyte buffer.

The usual way to use the byte sequence that results from explicitly
encoding text is to copy it to a file or process—for example, to write
it with `write-region` (see [Writing to Files](#Writing-to-Files)), and suppress
encoding by binding `coding-system-for-write` to
`no-conversion`.

Here are the functions to perform explicit encoding or decoding. The
encoding functions produce sequences of bytes; the decoding functions
are meant to operate on sequences of bytes. All of these functions
discard text properties. They also set `last-coding-system-used`
to the precise coding system they used.

Command: **encode-coding-region** start end coding-system &optional destination [¶](#index-encode_002dcoding_002dregion)
:   This command encodes the text from start to end according
    to coding system coding-system. Normally, the encoded text
    replaces the original text in the buffer, but the optional argument
    destination can change that. If destination is a buffer,
    the encoded text is inserted in that buffer after point (point does
    not move); if it is `t`, the command returns the encoded text as
    a unibyte string without inserting it.

    If encoded text is inserted in some buffer, this command returns the
    length of the encoded text.

    The result of encoding is logically a sequence of bytes, but the
    buffer remains multibyte if it was multibyte before, and any 8-bit
    bytes are converted to their multibyte representation (see [Text Representations](#Text-Representations)).

    Do *not* use `undecided` for coding-system when
    encoding text, since that may lead to unexpected results. Instead,
    use `select-safe-coding-system` (see [select-safe-coding-system](#User_002dChosen-Coding-Systems)) to suggest a suitable encoding,
    if there’s no obvious pertinent value for coding-system.

Function: **encode-coding-string** string coding-system &optional nocopy buffer [¶](#index-encode_002dcoding_002dstring)
:   This function encodes the text in string according to coding
    system coding-system. It returns a new string containing the
    encoded text, except when nocopy is non-`nil`, in which
    case the function may return string itself if the encoding
    operation is trivial. The result of encoding is a unibyte string.

Command: **decode-coding-region** start end coding-system &optional destination [¶](#index-decode_002dcoding_002dregion)
:   This command decodes the text from start to end according
    to coding system coding-system. To make explicit decoding
    useful, the text before decoding ought to be a sequence of byte
    values, but both multibyte and unibyte buffers are acceptable (in the
    multibyte case, the raw byte values should be represented as eight-bit
    characters). Normally, the decoded text replaces the original text in
    the buffer, but the optional argument destination can change
    that. If destination is a buffer, the decoded text is inserted
    in that buffer after point (point does not move); if it is `t`,
    the command returns the decoded text as a multibyte string without
    inserting it.

    If decoded text is inserted in some buffer, this command returns the
    length of the decoded text. If that buffer is a unibyte buffer
    (see [Selecting a Representation](#Selecting-a-Representation)), the internal representation of
    the decoded text (see [Text Representations](#Text-Representations)) is inserted into the
    buffer as individual bytes.

    This command puts a `charset` text property on the decoded text.
    The value of the property states the character set used to decode the
    original text.

    This command detects the encoding of the text if necessary. If
    coding-system is `undecided`, the command detects the
    encoding of the text based on the byte sequences it finds in the text,
    and also detects the type of end-of-line convention used by the text
    (see [eol type](#Lisp-and-Coding-Systems)). If coding-system
    is `undecided-eol-type`, where eol-type is
    `unix`, `dos`, or `mac`, then the command detects only
    the encoding of the text. Any coding-system that doesn’t
    specify eol-type, as in `utf-8`, causes the command to
    detect the end-of-line convention; specify the encoding completely, as
    in `utf-8-unix`, if the EOL convention used by the text is known
    in advance, to prevent any automatic detection.

Function: **decode-coding-string** string coding-system &optional nocopy buffer [¶](#index-decode_002dcoding_002dstring)
:   This function decodes the text in string according to
    coding-system. It returns a new string containing the decoded
    text, except when nocopy is non-`nil`, in which case the
    function may return string itself if the decoding operation is
    trivial. To make explicit decoding useful, the contents of
    string ought to be a unibyte string with a sequence of byte
    values, but a multibyte string is also acceptable (assuming it
    contains 8-bit bytes in their multibyte form).

    This function detects the encoding of the string if needed, like
    `decode-coding-region` does.

    If optional argument buffer specifies a buffer, the decoded text
    is inserted in that buffer after point (point does not move). In this
    case, the return value is the length of the decoded text. If that
    buffer is a unibyte buffer, the internal representation of the decoded
    text is inserted into it as individual bytes.

    This function puts a `charset` text property on the decoded text.
    The value of the property states the character set used to decode the
    original text:

    ```
    (decode-coding-string "Gr\374ss Gott" 'latin-1)
         ⇒ #("Grüss Gott" 0 10 (charset iso-8859-1))
    ```

Function: **decode-coding-inserted-region** from to filename &optional visit beg end replace [¶](#index-decode_002dcoding_002dinserted_002dregion)
:   This function decodes the text from from to to as if
    it were being read from file filename using `insert-file-contents`
    using the rest of the arguments provided.

    The normal way to use this function is after reading text from a file
    without decoding, if you decide you would rather have decoded it.
    Instead of deleting the text and reading it again, this time with
    decoding, you can call this function.

---

Previous: [Explicit Encoding and Decoding](#Explicit-Encoding), Up: [Coding Systems](#Coding-Systems)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 34.10.8 Terminal I/O Encoding [¶](#Terminal-I_002fO-Encoding-1)

Emacs can use coding systems to decode keyboard input and encode
terminal output. This is useful for terminals that transmit or
display text using a particular encoding, such as Latin-1. Emacs does
not set `last-coding-system-used` when encoding or decoding
terminal I/O.

Function: **keyboard-coding-system** &optional terminal [¶](#index-keyboard_002dcoding_002dsystem)
:   This function returns the coding system used for decoding keyboard
    input from terminal. A value of `no-conversion` means no
    decoding is done. If terminal is omitted or `nil`, it
    means the selected frame’s terminal. See [Multiple Terminals](#Multiple-Terminals).

Command: **set-keyboard-coding-system** coding-system &optional terminal [¶](#index-set_002dkeyboard_002dcoding_002dsystem)
:   This command specifies coding-system as the coding system to use
    for decoding keyboard input from terminal. If
    coding-system is `nil`, that means not to decode keyboard
    input. If terminal is a frame, it means that frame’s terminal;
    if it is `nil`, that means the currently selected frame’s
    terminal. See [Multiple Terminals](#Multiple-Terminals). Note that on modern MS-Windows
    systems Emacs always uses Unicode input when decoding keyboard input,
    so the encoding set by this command has no effect on Windows.

Function: **terminal-coding-system** &optional terminal [¶](#index-terminal_002dcoding_002dsystem)
:   This function returns the coding system that is in use for encoding
    terminal output from terminal. A value of `no-conversion`
    means no encoding is done. If terminal is a frame, it means
    that frame’s terminal; if it is `nil`, that means the currently
    selected frame’s terminal.

Command: **set-terminal-coding-system** coding-system &optional terminal [¶](#index-set_002dterminal_002dcoding_002dsystem)
:   This command specifies coding-system as the coding system to use
    for encoding terminal output from terminal. If
    coding-system is `nil`, that means not to encode terminal
    output. If terminal is a frame, it means that frame’s terminal;
    if it is `nil`, that means the currently selected frame’s
    terminal.

---

Next: [Locales](#Locales), Previous: [Coding Systems](#Coding-Systems), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.11 Input Methods [¶](#Input-Methods-1)

*Input methods* provide convenient ways of entering non-ASCII
characters from the keyboard. Unlike coding systems, which translate
non-ASCII characters to and from encodings meant to be read by
programs, input methods provide human-friendly commands. (See [Input
Methods](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Input-Methods) in The GNU Emacs Manual, for information on how users
use input methods to enter text.) How to define input methods is not
yet documented in this manual, but here we describe how to use them.

Each input method has a name, which is currently a string;
in the future, symbols may also be usable as input method names.

Variable: **current-input-method** [¶](#index-current_002dinput_002dmethod)
:   This variable holds the name of the input method now active in the
    current buffer. (It automatically becomes local in each buffer when set
    in any fashion.) It is `nil` if no input method is active in the
    buffer now.

User Option: **default-input-method** [¶](#index-default_002dinput_002dmethod)
:   This variable holds the default input method for commands that choose an
    input method. Unlike `current-input-method`, this variable is
    normally global.

Command: **set-input-method** input-method [¶](#index-set_002dinput_002dmethod)
:   This command activates input method input-method for the current
    buffer. It also sets `default-input-method` to input-method.
    If input-method is `nil`, this command deactivates any input
    method for the current buffer.

Function: **read-input-method-name** prompt &optional default inhibit-null [¶](#index-read_002dinput_002dmethod_002dname)
:   This function reads an input method name with the minibuffer, prompting
    with prompt. If default is non-`nil`, that is returned
    by default, if the user enters empty input. However, if
    inhibit-null is non-`nil`, empty input signals an error.

    The returned value is a string.

Variable: **input-method-alist** [¶](#index-input_002dmethod_002dalist)
:   This variable defines all the supported input methods.
    Each element defines one input method, and should have the form:

    ```
    (input-method language-env activate-func
     title description args...)
    ```

    Here input-method is the input method name, a string;
    language-env is another string, the name of the language
    environment this input method is recommended for. (That serves only for
    documentation purposes.)

    activate-func is a function to call to activate this method. The
    args, if any, are passed as arguments to activate-func. All
    told, the arguments to activate-func are input-method and
    the args.

    title is a string to display in the mode line while this method is
    active. description is a string describing this method and what
    it is good for.

The fundamental interface to input methods is through the
variable `input-method-function`. See [Reading One Event](#Reading-One-Event),
and [Invoking the Input Method](#Invoking-the-Input-Method).

---

Previous: [Input Methods](#Input-Methods), Up: [Non-ASCII Characters](#Non_002dASCII-Characters)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 34.12 Locales [¶](#Locales-1)

In POSIX, locales control which language
to use in language-related features. These Emacs variables control
how Emacs interacts with these features.

Variable: **locale-coding-system** [¶](#index-locale_002dcoding_002dsystem)
:   This variable specifies the coding system to use for decoding system
    error messages and—on X Window system only—keyboard input, for
    sending batch output to the standard output and error streams, for
    encoding the format argument to `format-time-string`, and for
    decoding the return value of `format-time-string`.

Variable: **system-messages-locale** [¶](#index-system_002dmessages_002dlocale)
:   This variable specifies the locale to use for generating system error
    messages. Changing the locale can cause messages to come out in a
    different language or in a different orthography. If the variable is
    `nil`, the locale is specified by environment variables in the
    usual POSIX fashion.
    Otherwise, the variable should specify a locale compatible with the
    coding system implied by those environment variables.

Variable: **system-time-locale** [¶](#index-system_002dtime_002dlocale)
:   This variable specifies the locale to use for formatting time values.
    Changing the locale can cause messages to appear according to the
    conventions of a different language. If the variable is `nil`, the
    locale is specified by environment variables in the usual POSIX fashion.
    Otherwise, the variable should specify a locale compatible with the
    coding system implied by those environment variables.

Function: **locale-info** item [¶](#index-locale_002dinfo)
:   This function returns locale data item for the current POSIX
    locale, if available. item should be one of these symbols:

    `codeset`
    :   Return the character set as a string (locale item `CODESET`).

    `days`
    :   Return a 7-element vector of day names (locale items
        `DAY_1` through `DAY_7`);

    `months`
    :   Return a 12-element vector of month names (locale items `MON_1`
        through `MON_12`).

    `paper`
    :   Return a list `(width height)` of 2 integers, for
        the default paper size measured in millimeters (locale items
        `_NL_PAPER_WIDTH` and `_NL_PAPER_HEIGHT`).

    If the system can’t provide the requested information, or if
    item is not one of those symbols, the value is `nil`. All
    strings in the return value are decoded using
    `locale-coding-system`. See [Locales](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Locales) in The GNU Libc Manual,
    for more information about locales and locale items.

---

Next: [Syntax Tables](#Syntax-Tables), Previous: [Non-ASCII Characters](#Non_002dASCII-Characters), Up: [Emacs Lisp](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 35 Searching and Matching [¶](#Searching-and-Matching-1)

GNU Emacs provides two ways to search through a buffer for specified
text: exact string searches and regular expression searches. After a
regular expression search, you can examine the *match data* to
determine which text matched the whole regular expression or various
portions of it.

The ‘`skip-chars…`’ functions also perform a kind of searching.
See [Skipping Characters](#Skipping-Characters). To search for changes in character
properties, see [Text Property Search Functions](#Property-Search).

* [Searching for Strings](#String-Search)
* [Searching and Case](#Searching-and-Case)
* [Regular Expressions](#Regular-Expressions)
* [Regular Expression Searching](#Regexp-Search)
* [Longest-match searching for regular expression matches](#Longest-Match)
* [The Match Data](#Match-Data)
* [Search and Replace](#Search-and-Replace)
* [Standard Regular Expressions Used in Editing](#Standard-Regexps)
* [Emacs versus POSIX Regular Expressions](#POSIX-Regexps)

---

Next: [Searching and Case](#Searching-and-Case), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 35.1 Searching for Strings [¶](#Searching-for-Strings)

These are the primitive functions for searching through the text in a
buffer. They are meant for use in programs, but you may call them
interactively. If you do so, they prompt for the search string; the
arguments limit and noerror are `nil`, and repeat
is 1. For more details on interactive searching, see [Searching and Replacement](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Search) in The GNU Emacs Manual.

These search functions convert the search string to multibyte if the
buffer is multibyte; they convert the search string to unibyte if the
buffer is unibyte. See [Text Representations](#Text-Representations).

Command: **search-forward** string &optional limit noerror count [¶](#index-search_002dforward)
:   This function searches forward from point for an exact match for
    string. If successful, it sets point to the end of the occurrence
    found, and returns the new value of point. If no match is found, the
    value and side effects depend on noerror (see below).

    In the following example, point is initially at the beginning of the
    line. Then `(search-forward "fox")` moves point after the last
    letter of ‘`fox`’:

    ```
    ---------- Buffer: foo ----------
    ∗The quick brown fox jumped over the lazy dog.
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (search-forward "fox")
         ⇒ 20

    ---------- Buffer: foo ----------
    The quick brown fox∗ jumped over the lazy dog.
    ---------- Buffer: foo ----------
    ```

    The argument limit specifies the bound to the search, and should
    be a position in the current buffer. No match extending after
    that position is accepted. If limit is omitted or `nil`, it
    defaults to the end of the accessible portion of the buffer.

    What happens when the search fails depends on the value of
    noerror. If noerror is `nil`, a `search-failed`
    error is signaled. If noerror is `t`, `search-forward`
    returns `nil` and does nothing. If noerror is neither
    `nil` nor `t`, then `search-forward` moves point to the
    upper bound and returns `nil`.

    The argument noerror only affects valid searches which fail to
    find a match. Invalid arguments cause errors regardless of
    noerror.

    If count is a positive number n, the search is done
    n times; each successive search starts at the end of the
    previous match. If all these successive searches succeed, the
    function call succeeds, moving point and returning its new value.
    Otherwise the function call fails, with results depending on the value
    of noerror, as described above. If count is a negative
    number −n, the search is done n times in the opposite
    (backward) direction.

Command: **search-backward** string &optional limit noerror count [¶](#index-search_002dbackward)
:   This function searches backward from point for string. It is
    like `search-forward`, except that it searches backwards rather
    than forwards. Backward searches leave point at the beginning of the
    match.

Command: **word-search-forward** string &optional limit noerror count [¶](#index-word_002dsearch_002dforward)
:   This function searches forward from point for a word match for
    string. If it finds a match, it sets point to the end of the
    match found, and returns the new value of point.

    Word matching regards string as a sequence of words, disregarding
    punctuation that separates them. It searches the buffer for the same
    sequence of words. Each word must be distinct in the buffer (searching
    for the word ‘`ball`’ does not match the word ‘`balls`’), but the
    details of punctuation and spacing are ignored (searching for ‘`ball
    boy`’ does match ‘`ball. Boy!`’).

    In this example, point is initially at the beginning of the buffer; the
    search leaves it between the ‘`y`’ and the ‘`!`’.

    ```
    ---------- Buffer: foo ----------
    ∗He said "Please!  Find
    the ball boy!"
    ---------- Buffer: foo ----------
    ```

    ```

    ```

    ```
    (word-search-forward "Please find the ball, boy.")
         ⇒ 39

    ---------- Buffer: foo ----------
    He said "Please!  Find
    the ball boy∗!"
    ---------- Buffer: foo ----------
    ```

    If limit is non-`nil`, it must be a position in the current
    buffer; it specifies the upper bound to the search. The match found
    must not extend after that position.

    If noerror is `nil`, then `word-search-forward` signals
    an error if the search fails. If noerror is `t`, then it
    returns `nil` instead of signaling an error. If noerror is
    neither `nil` nor `t`, it moves point to limit (or the
    end of the accessible portion of the buffer) and returns `nil`.

    If count is a positive number, it specifies how many successive
    occurrences to search for. Point is positioned at the end of the last
    match. If count is a negative number, the search is backward
    and point is positioned at the beginning of the last match.

    Internally, `word-search-forward` and related functions use the
    function `word-search-regexp` to convert string to a
    regular expression that ignores punctuation.

Command: **word-search-forward-lax** string &optional limit noerror count [¶](#index-word_002dsearch_002dforward_002dlax)
:   This command is identical to `word-search-forward`, except that
    the beginning or the end of string need not match a word
    boundary, unless string begins or ends in whitespace.
    For instance, searching for ‘`ball boy`’ matches ‘`ball boyee`’,
    but does not match ‘`balls boy`’.

Command: **word-search-backward** string &optional limit noerror count [¶](#index-word_002dsearch_002dbackward)
:   This function searches backward from point for a word match to
    string. This function is just like `word-search-forward`
    except that it searches backward and normally leaves point at the
    beginning of the match.

Command: **word-search-backward-lax** string &optional limit noerror count [¶](#index-word_002dsearch_002dbackward_002dlax)
:   This command is identical to `word-search-backward`, except that
    the beginning or the end of string need not match a word
    boundary, unless string begins or ends in whitespace.

---

Next: [Regular Expressions](#Regular-Expressions), Previous: [Searching for Strings](#String-Search), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 35.2 Searching and Case [¶](#Searching-and-Case-1)

By default, searches in Emacs ignore the case of the text they are
searching through; if you specify searching for ‘`FOO`’, then
‘`Foo`’ or ‘`foo`’ is also considered a match. This applies to
regular expressions, too; thus, ‘`[aB]`’ would match ‘`a`’ or
‘`A`’ or ‘`b`’ or ‘`B`’.

If you do not want this feature, set the variable
`case-fold-search` to `nil`. Then all letters must match
exactly, including case. This is a buffer-local variable; altering the
variable affects only the current buffer. (See [Introduction to Buffer-Local Variables](#Intro-to-Buffer_002dLocal).) Alternatively, you may change the default value.
In Lisp code, you will more typically use `let` to bind
`case-fold-search` to the desired value.

Note that the user-level incremental search feature handles case
distinctions differently. When the search string contains only lower
case letters, the search ignores case, but when the search string
contains one or more upper case letters, the search becomes
case-sensitive. But this has nothing to do with the searching
functions used in Lisp code. See [Incremental Search](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Incremental-Search) in The GNU Emacs Manual.

User Option: **case-fold-search** [¶](#index-case_002dfold_002dsearch)
:   This buffer-local variable determines whether searches should ignore
    case. If the variable is `nil` they do not ignore case; otherwise
    (and by default) they do ignore case.

User Option: **case-replace** [¶](#index-case_002dreplace)
:   This variable determines whether the higher-level replacement
    functions should preserve case. If the variable is `nil`, that
    means to use the replacement text verbatim. A non-`nil` value
    means to convert the case of the replacement text according to the
    text being replaced.

    This variable is used by passing it as an argument to the function
    `replace-match`. See [Replacing the Text that Matched](#Replacing-Match).

---

Next: [Regular Expression Searching](#Regexp-Search), Previous: [Searching and Case](#Searching-and-Case), Up: [Searching and Matching](#Searching-and-Matching)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
