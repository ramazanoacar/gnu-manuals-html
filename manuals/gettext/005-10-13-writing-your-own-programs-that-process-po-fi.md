### 10.13 Writing your own programs that process PO files [¶](#Writing-your-own-programs-that-process-PO-files)

For the tasks for which a combination of ‘`msgattrib`’, ‘`msgcat`’ etc.
is not sufficient, a set of C functions is provided in a library, to make it
possible to process PO files in your own programs. When you use this library,
you don’t need to write routines to parse the PO file; instead, you retrieve
a pointer in memory to each of messages contained in the PO file. Functions
for writing those memory structures to a file after working with them are
provided too.

The functions are declared in the header file ‘`<gettext-po.h>`’, and are
defined in a library called ‘`libgettextpo`’.

The library is multithread-safe in the following sense:
Different threads can safely use the various functions simultaneously on
unrelated data objects.
For example, if several threads have created separate `po_file_t` objects,
each of them can safely work on its respective `po_file_t` object,
without caring about the other threads.

The following example shows code how these functions can be used. Error
handling code is omitted, as its implementation is delegated to the user
provided functions.

```
struct po_xerror_handler handler =
  {
    .xerror = ...,
    .xerror2 = ...
  };
const char *filename = ...;
/* Read the file into memory.  */
po_file_t file = po_file_read (filename, &handler);

{
  const char * const *domains = po_file_domains (file);
  const char * const *domainp;

  /* Iterate the domains contained in the file.  */
  for (domainp = domains; *domainp; domainp++)
    {
      po_message_t *message;
      const char *domain = *domainp;
      po_message_iterator_t iterator = po_message_iterator (file, domain);

      /* Iterate each message inside the domain.  */
      while ((message = po_next_message (iterator)) != NULL)
        {
          /* Read data from the message ...  */
          const char *msgid = po_message_msgid (message);
          const char *msgstr = po_message_msgstr (message);

          ...

          /* Modify its contents ...  */
          if (perform_some_tests (msgid, msgstr))
            po_message_set_fuzzy (message, 1);

          ...
        }
      /* Always release returned po_message_iterator_t.  */
      po_message_iterator_free (iterator);
    }

  /* Write back the result.  */
  po_file_t result = po_file_write (file, filename, &handler);
}

/* Always release the returned po_file_t.  */
po_file_free (file);
```

* [Error Handling](#Error-Handling)
* [po\_file\_t API](#po_005ffile_005ft-API)
* [po\_message\_iterator\_t API](#po_005fmessage_005fiterator_005ft-API)
* [po\_message\_t API](#po_005fmessage_005ft-API)
* [PO Header Entry API](#PO-Header-Entry-API)
* [po\_filepos\_t API](#po_005ffilepos_005ft-API)
* [Format Type API](#Format-Type-API)
* [po\_flag\_iterator\_t API](#po_005fflag_005fiterator_005ft-API)
* [Checking API](#Checking-API)

---

Next: [po\_file\_t API](#po_005ffile_005ft-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.1 Error Handling [¶](#Error-Handling-1)

Error management is performed through callbacks provided by the user of
the library. They are provided through a parameter with the following
type:

Data Type: **struct** po\_xerror\_handler [¶](#index-struct)
:   Its pointer is defined as `po_xerror_handler_t`. Contains
    two fields, `xerror` and `xerror2`, with the following function
    signatures.

Function: `void` **xerror** `(int severity, po_message_t message, const char *filename, size_t lineno, size_t column, int multiline_p, const char *message_text)` [¶](#index-xerror)
:   This function is called to signal a problem of the given severity.
    It *must not return* if severity is
    `PO_SEVERITY_FATAL_ERROR`.

    message\_text is the problem description. When multiline\_p
    is true, it can contain multiple lines of text, each terminated with a
    newline, otherwise a single line.

    message and/or filename and lineno indicate where the
    problem occurred:

    * If filename is `NULL`, filename and lineno and
      column should be ignored.
    * If lineno is `(size_t)(-1)`, lineno and column
      should be ignored.
    * If column is `(size_t)(-1)`, it should be ignored.

Function: `void` **xerror2** `(int severity, po_message_t message1, const char *filename1, size_t lineno1, size_t column1, int multiline_p1, const char *message_text1, po_message_t message2, const char *filename2, size_t lineno2, size_t column2, int multiline_p2, const char *message_text2)` [¶](#index-xerror2)
:   This function is called to signal a problem of the given severity
    that refers to two messages. It *must not return* if
    severity is `PO_SEVERITY_FATAL_ERROR`.

    It is similar to two calls to xerror. If possible, an ellipsis can be
    appended to message\_text1 and prepended to message\_text2.

---

Next: [po\_message\_iterator\_t API](#po_005fmessage_005fiterator_005ft-API), Previous: [Error Handling](#Error-Handling), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.2 po\_file\_t API [¶](#po_005ffile_005ft-API-1)

Data Type: **po\_file\_t** [¶](#index-po_005ffile_005ft)
:   This is a pointer type that refers to the contents of a PO file, after it has
    been read into memory.

Function: `po_file_t` **po\_file\_create** `()` [¶](#index-po_005ffile_005fcreate)
:   The `po_file_create` function creates an empty PO file representation in
    memory.

Function: `po_file_t` **po\_file\_read** `(const char *filename, struct po_xerror_handler *handler)` [¶](#index-po_005ffile_005fread)
:   The `po_file_read` function reads a PO file into memory. The file name
    is given as argument. The return value is a handle to the PO file’s contents,
    valid until `po_file_free` is called on it. In case of error, the
    functions from handler are called to signal it.

    This function is exported as ‘`po_file_read_v3`’ at ABI level, but is
    defined as `po_file_read` in C code after the inclusion of
    ‘`<gettext-po.h>`’.

Function: `po_file_t` **po\_file\_write** `(po_file_t file, const char *filename, struct po_xerror_handler *handler)` [¶](#index-po_005ffile_005fwrite)
:   The `po_file_write` function writes the contents of the memory
    structure file the filename given. The return value is
    file after a successful operation. In case of error, the
    functions from handler are called to signal it.

    This function is exported as ‘`po_file_write_v2`’ at ABI level, but
    is defined as `po_file_write` in C code after the inclusion of
    ‘`<gettext-po.h>`’.

Function: `void` **po\_file\_free** `(po_file_t file)` [¶](#index-po_005ffile_005ffree)
:   The `po_file_free` function frees a PO file’s contents from memory,
    including all messages that are only implicitly accessible through iterators.

Function: `const char * const *` **po\_file\_domains** `(po_file_t file)` [¶](#index-po_005ffile_005fdomains)
:   The `po_file_domains` function returns the domains for which the given
    PO file has messages. The return value is a `NULL` terminated array
    which is valid as long as the file handle is valid. For PO files which
    contain no ‘`domain`’ directive, the return value contains only one domain,
    namely the default domain `"messages"`.

---

Next: [po\_message\_t API](#po_005fmessage_005ft-API), Previous: [po\_file\_t API](#po_005ffile_005ft-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.3 po\_message\_iterator\_t API [¶](#po_005fmessage_005fiterator_005ft-API-1)

Data Type: **po\_message\_iterator\_t** [¶](#index-po_005fmessage_005fiterator_005ft)
:   This is a pointer type that refers to an iterator that produces a sequence of
    messages.

Function: `po_message_iterator_t` **po\_message\_iterator** `(po_file_t file, const char *domain)` [¶](#index-po_005fmessage_005fiterator)
:   The `po_message_iterator` returns an iterator that will produce the
    messages of file that belong to the given domain. If domain
    is `NULL`, the default domain is used instead. To list the messages,
    use the function `po_next_message` repeatedly.

Function: `void` **po\_message\_iterator\_free** `(po_message_iterator_t iterator)` [¶](#index-po_005fmessage_005fiterator_005ffree)
:   The `po_message_iterator_free` function frees an iterator previously
    allocated through the `po_message_iterator` function.

Function: `po_message_t` **po\_next\_message** `(po_message_iterator_t iterator)` [¶](#index-po_005fnext_005fmessage)
:   The `po_next_message` function returns the next message from
    iterator and advances the iterator. It returns `NULL` when the
    iterator has reached the end of its message list.

---

Next: [PO Header Entry API](#PO-Header-Entry-API), Previous: [po\_message\_iterator\_t API](#po_005fmessage_005fiterator_005ft-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.4 po\_message\_t API [¶](#po_005fmessage_005ft-API-1)

Data Type: **po\_message\_t** [¶](#index-po_005fmessage_005ft)
:   This is a pointer type that refers to a message of a PO file, including its
    translation.

Function: `po_message_t` **po\_message\_create** `(void)` [¶](#index-po_005fmessage_005fcreate)
:   Returns a freshly constructed message. To finish initializing the
    message, you must set the `msgid` and `msgstr`. It *must* be
    inserted into a file to manage its memory, as there is no
    `po_message_free` available to the user of the library.

The following functions access details of a `po_message_t`. Recall
that the results are valid as long as the file handle is valid.

Function: `const char *` **po\_message\_msgctxt** `(po_message_t message)` [¶](#index-po_005fmessage_005fmsgctxt)
:   The `po_message_msgctxt` function returns the `msgctxt`, the
    context of message. Returns `NULL` for a message not restricted
    to a context.

Function: `void` **po\_message\_set\_msgctxt** `(po_message_t message, const char *msgctxt)` [¶](#index-po_005fmessage_005fset_005fmsgctxt)
:   The `po_message_set_msgctxt` function changes the `msgctxt`,
    the context of the message, to the value provided through
    msgctxt. The value `NULL` removes the restriction.

Function: `const char *` **po\_message\_msgid** `(po_message_t message)` [¶](#index-po_005fmessage_005fmsgid)
:   The `po_message_msgid` function returns the `msgid` (untranslated
    English string) of message. This is guaranteed to be non-`NULL`.

Function: `void` **po\_message\_set\_msgid** `(po_message_t message, const char *msgid)` [¶](#index-po_005fmessage_005fset_005fmsgid)
:   The `po_message_set_msgid` function changes the `msgid`
    (untranslated English string) of message to the value provided through
    msgid, a non-`NULL` string.

Function: `const char *` **po\_message\_msgid\_plural** `(po_message_t message)` [¶](#index-po_005fmessage_005fmsgid_005fplural)
:   The `po_message_msgid_plural` function returns the `msgid_plural`
    (untranslated English plural string) of message, a message with plurals,
    or `NULL` for a message without plural.

Function: `void` **po\_message\_set\_msgid\_plural** `(po_message_t message, const char *msgid_plural)` [¶](#index-po_005fmessage_005fset_005fmsgid_005fplural)
:   The `po_message_set_msgid_plural` function changes the
    `msgid_plural` (untranslated English plural string) of a message to
    the value provided through msgid\_plural, or removes the plurals if
    `NULL` is provided as msgid\_plural.

Function: `const char *` **po\_message\_msgstr** `(po_message_t message)` [¶](#index-po_005fmessage_005fmsgstr)
:   The `po_message_msgstr` function returns the `msgstr` (translation)
    of message. For an untranslated message, the return value is an empty
    string.

Function: `void` **po\_message\_set\_msgstr** `(po_message_t message, const char *msgstr)` [¶](#index-po_005fmessage_005fset_005fmsgstr)
:   The `po_message_set_msgstr` function changes the `msgstr`
    (translation) of message to the value provided through msgstr, a
    non-`NULL` string.

Function: `const char *` **po\_message\_msgstr\_plural** `(po_message_t message, int index)` [¶](#index-po_005fmessage_005fmsgstr_005fplural)
:   The `po_message_msgstr_plural` function returns the
    `msgstr[index]` of message, a message with plurals, or
    `NULL` when the index is out of range or for a message without
    plural.

Function: `void` **po\_message\_set\_msgstr\_plural** `(po_message_t message, int index, const char *msgstr_plural)` [¶](#index-po_005fmessage_005fset_005fmsgstr_005fplural)
:   The `po_message_set_msgstr_plural` function changes the
    `msgstr[index]` of message, a message with plurals, to
    the value provided through msgstr\_plural. message must be a
    message with plurals.
    Use `NULL` as the value of msgstr\_plural with
    index pointing to the last element to reduce the number of plural
    forms.

Function: `const char *` **po\_message\_comments** `(po_message_t message)` [¶](#index-po_005fmessage_005fcomments)
:   The `po_message_comments` function returns the comments of message,
    a multiline string, ending in a newline, or a non-`NULL` empty string.

Function: `void` **po\_message\_set\_comments** `(po_message_t message, const char *comments)` [¶](#index-po_005fmessage_005fset_005fcomments)
:   The `po_message_set_comments` function changes the comments of
    message to the value comments, a multiline string, ending in a
    newline, or a non-`NULL` empty string.

Function: `const char *` **po\_message\_extracted\_comments** `(po_message_t message)` [¶](#index-po_005fmessage_005fextracted_005fcomments)
:   The `po_message_extracted_comments` function returns the extracted
    comments of message, a multiline string, ending in a newline, or a
    non-`NULL` empty string.

Function: `void` **po\_message\_set\_extracted\_comments** `(po_message_t message, const char *extracted_comments)` [¶](#index-po_005fmessage_005fset_005fextracted_005fcomments)
:   The `po_message_set_extracted_comments` function changes the
    comments of message to the value extracted\_comments, a multiline
    string, ending in a newline, or a non-`NULL` empty string.

Function: `const char *` **po\_message\_prev\_msgctxt** `(po_message_t message)` [¶](#index-po_005fmessage_005fprev_005fmsgctxt)
:   The `po_message_prev_msgctxt` function returns the previous
    `msgctxt`, the previous context of message. Return
    `NULL` for a message that does not have a previous context.

Function: `void` **po\_message\_set\_prev\_msgctxt** `(po_message_t message, const char *prev_msgctxt)` [¶](#index-po_005fmessage_005fset_005fprev_005fmsgctxt)
:   The `po_message_set_prev_msgctxt` function changes the previous
    `msgctxt`, the context of the message, to the value provided
    through prev\_msgctxt. The value `NULL` removes the stored
    previous msgctxt.

Function: `const char *` **po\_message\_prev\_msgid** `(po_message_t message)` [¶](#index-po_005fmessage_005fprev_005fmsgid)
:   The `po_message_prev_msgid` function returns the previous
    `msgid` (untranslated English string) of message, or
    `NULL` if there is no previous `msgid` stored.

Function: `void` **po\_message\_set\_prev\_msgid** `(po_message_t message, const char *prev_msgid)` [¶](#index-po_005fmessage_005fset_005fprev_005fmsgid)
:   The `po_message_set_prev_msgid` function changes the previous
    `msgid` (untranslated English string) of message to the value
    provided through prev\_msgid, or removes the message when it is
    `NULL`.

Function: `const char *` **po\_message\_prev\_msgid\_plural** `(po_message_t message)` [¶](#index-po_005fmessage_005fprev_005fmsgid_005fplural)
:   The `po_message_prev_msgid_plural` function returns the previous
    `msgid_plural` (untranslated English plural string) of
    message, a message with plurals, or `NULL` for a message
    without plural without any stored previous `msgid_plural`.

Function: `void` **po\_message\_set\_prev\_msgid\_plural** `(po_message_t message, const char *prev_msgid_plural)` [¶](#index-po_005fmessage_005fset_005fprev_005fmsgid_005fplural)
:   The `po_message_set_prev_msgid_plural` function changes the
    previous `msgid_plural` (untranslated English plural string) of a
    message to the value provided through prev\_msgid\_plural, or
    removes the stored previous `msgid_plural` if `NULL` is
    provided as prev\_msgid\_plural.

Function: `int` **po\_message\_is\_obsolete** `(po_message_t message)` [¶](#index-po_005fmessage_005fis_005fobsolete)
:   The `po_message_is_obsolete` function returns true when message
    is marked as obsolete.

Function: `void` **po\_message\_set\_obsolete** `(po_message_t message, int obsolete)` [¶](#index-po_005fmessage_005fset_005fobsolete)
:   The `po_message_set_obsolete` function changes the obsolete mark of
    message.

Function: `int` **po\_message\_is\_fuzzy** `(po_message_t message)` [¶](#index-po_005fmessage_005fis_005ffuzzy)
:   The `po_message_is_fuzzy` function returns true when message
    is marked as fuzzy.

Function: `void` **po\_message\_set\_fuzzy** `(po_message_t message, int fuzzy)` [¶](#index-po_005fmessage_005fset_005ffuzzy)
:   The `po_message_set_fuzzy` function changes the fuzzy mark of
    message.

Function: `int` **po\_message\_has\_workflow\_flag** `(po_message_t message, const char *workflow_flag)` [¶](#index-po_005fmessage_005fhas_005fworkflow_005fflag)
:   The `po_message_has_workflow_flag` function returns true
    when message is marked with a given workflow flag.

    When the argument workflow\_flag is `"fuzzy"`,
    this function corresponds to the `po_message_is_fuzzy` function.

Function: `void` **po\_message\_set\_workflow\_flag** `(po_message_t message, const char *workflow_flag, int value)` [¶](#index-po_005fmessage_005fset_005fworkflow_005fflag)
:   The `po_message_set_workflow_flag` function changes
    a given workflow flag on message:
    If the value argument is non-zero, it sets it.
    If the value argument is zero, it unsets it.

    When the argument workflow\_flag is `"fuzzy"`,
    this function corresponds to the `po_message_set_fuzzy` function.

Function: `po_flag_iterator_t` **po\_message\_workflow\_flags\_iterator** `(po_message_t message)` [¶](#index-po_005fmessage_005fworkflow_005fflags_005fiterator)
:   The `po_message_workflow_flags_iterator` function returns an iterator
    across the workflow flags of message.
    This includes the `"fuzzy"` flag.
    This iterator returns the workflow flags set on message,
    one by one, through the function `po_flag_next`.
    When done, you should free the iterator,
    through the function `po_flag_iterator_free`.

Function: `int` **po\_message\_is\_format** `(po_message_t message, const char *format_type)` [¶](#index-po_005fmessage_005fis_005fformat)
:   The `po_message_is_format` function returns true when the message
    is marked as being a format string of format\_type.

Function: `int` **po\_message\_get\_format** `(po_message_t message, const char *format_type)` [¶](#index-po_005fmessage_005fget_005fformat)
:   The `po_message_get_format` function returns an indicator
    whether the message is marked as being a format string of format\_type
    or the opposite (`no-format_type`).
    More precisely,
    it returns 1 if the format\_type mark is set,
    0 if the `no-format_type` mark is set,
    or -1 of neither the mark nor the opposite mark is set.

Function: `void` **po\_message\_set\_format** `(po_message_t message, const char *format_type, int value)` [¶](#index-po_005fmessage_005fset_005fformat)
:   The `po_message_set_format` function changes
    the format string mark of the message for the format\_type provided.
    Pass value = 1
    to assert the format string mark (leading to e.g. ‘`c-format`’),
    value = 0
    to assert the opposite (leading to e.g. ‘`no-c-format`’),
    or value = -1
    to remove the format string mark and its opposite.

Function: `int` **po\_message\_has\_sticky\_flag** `(po_message_t message, const char *sticky_flag)` [¶](#index-po_005fmessage_005fhas_005fsticky_005fflag)
:   The `po_message_has_sticky_flag` function returns true
    when message is marked with a given sticky flag.

    This function is a generalization of the
    `po_message_is_format` and `po_message_get_format` format functions.

Function: `void` **po\_message\_set\_sticky\_flag** `(po_message_t message, const char *sticky_flag, int value)` [¶](#index-po_005fmessage_005fset_005fsticky_005fflag)
:   The `po_message_set_sticky_flag` function changes
    a given sticky flag on message:
    If the value argument is non-zero, it sets it.
    If the value argument is zero, it unsets it.

    This function is a generalization of the `po_message_set_format` function.

    Note that a message cannot have the sticky flags
    `language-format` and `no-language-format`
    at the same time.
    Therefore, setting the `language-format` flag for some language
    has the side effect of unsetting the `no-language-format` flag,
    and vice versa.

Function: `po_flag_iterator_t` **po\_message\_sticky\_flags\_iterator** `(po_message_t message)` [¶](#index-po_005fmessage_005fsticky_005fflags_005fiterator)
:   The `po_message_sticky_flags_iterator` function returns an iterator
    across the sticky flags of message.
    This includes the various
    `language-format` and `no-language-format` flags,
    as well as the `no-wrap` flag.
    It does *not* include the `"range"`, because that is not a flag.
    This iterator returns the sticky flags set on message,
    one by one, through the function `po_flag_next`.
    When done, you should free the iterator,
    through the function `po_flag_iterator_free`.

Function: `int` **po\_message\_is\_range** `(po_message_t message, int *minp, int *maxp)` [¶](#index-po_005fmessage_005fis_005frange)
:   The `po_message_is_range` function returns true when the message
    has a numeric range set, and stores the minimum and maximum value in the
    locations pointed by minp and maxp respectively.

Function: `void` **po\_message\_set\_range** `(po_message_t message, int min, int max)` [¶](#index-po_005fmessage_005fset_005frange)
:   The `po_message_set_range` function changes the numeric range of
    the message. min and max must be non-negative, with
    min < max. Use min and max with value `-1`
    to remove the numeric range of message.

---

Next: [po\_filepos\_t API](#po_005ffilepos_005ft-API), Previous: [po\_message\_t API](#po_005fmessage_005ft-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.5 PO Header Entry API [¶](#PO-Header-Entry-API-1)

The following functions provide an interface to extract and manipulate
the header entry (see [Filling in the Header Entry](#Header-Entry)) from a file loaded in memory.
The meta information must be written back into the domain message with
the empty string as `msgid`.

Function: `const char *` **po\_file\_domain\_header** `(po_file_t file, const char *domain)` [¶](#index-po_005ffile_005fdomain_005fheader)
:   Returns the header entry of a domain from file, a PO file loaded in
    memory. The value `NULL` provided as domain denotes the
    default domain. Returns `NULL` if there is no header entry.

Function: `char *` **po\_header\_field** `(const char *header, const char *field)` [¶](#index-po_005fheader_005ffield)
:   Returns the value of field in the header entry. The return
    value is either a freshly allocated string, to be freed by the caller,
    or `NULL`.

Function: `char *` **po\_header\_set\_field** `(const char *header, const char *field, const char *value)` [¶](#index-po_005fheader_005fset_005ffield)
:   Returns a freshly allocated string which contains the entry from
    header with field set to value. The field is added if
    necessary.

---

Next: [Format Type API](#Format-Type-API), Previous: [PO Header Entry API](#PO-Header-Entry-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.6 po\_filepos\_t API [¶](#po_005ffilepos_005ft-API-1)

Data Type: **po\_filepos\_t** [¶](#index-po_005ffilepos_005ft)
:   This is a pointer type that refers to a string’s position within a
    source file.

The following functions provide an interface to extract and manipulate
these references.

Function: `po_filepos_t` **po\_message\_filepos** `(po_message_t message, int index)` [¶](#index-po_005fmessage_005ffilepos)
:   Returns the file reference in position index from the message. If
    index is out of range, returns `NULL`.

Function: `void` **po\_message\_remove\_filepos** `(po_message_t message, int index)` [¶](#index-po_005fmessage_005fremove_005ffilepos)
:   Removes the file reference in position index from the message. It
    moves all references following index one position backwards.

Function: `void` **po\_message\_add\_filepos** `(po_message_t message, const char *file, size_t start_line)` [¶](#index-po_005fmessage_005fadd_005ffilepos)
:   Adds a reference to the string from file starting at
    start\_line, if it is not already present for the message. The
    value `(size_t)(-1)` for start\_line denotes that the line
    number is not available.

---

Next: [po\_flag\_iterator\_t API](#po_005fflag_005fiterator_005ft-API), Previous: [po\_filepos\_t API](#po_005ffilepos_005ft-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.7 Format Type API [¶](#Format-Type-API-1)

Function: `const char * const *` **po\_format\_list** `(void)` [¶](#index-po_005fformat_005flist)
:   Returns a `NULL` terminated array of the supported format types.

Function: `const char *` **po\_format\_pretty\_name** `(const char *format_type)` [¶](#index-po_005fformat_005fpretty_005fname)
:   Returns the pretty name associated with format\_type. For example,
    it returns “C#” when format\_type is “csharp\_format”.
    Return `NULL` if format\_type is not a supported format type.

---

Next: [Checking API](#Checking-API), Previous: [Format Type API](#Format-Type-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.8 po\_flag\_iterator\_t API [¶](#po_005fflag_005fiterator_005ft-API-1)

The following functions apply to a `po_flag_iterator_t` object,
as returned by the functions `po_message_workflow_flags_iterator`
and `po_message_sticky_flags_iterator`.

Function: `void` **po\_flag\_iterator\_free** `(po_flag_iterator_t iterator)` [¶](#index-po_005fflag_005fiterator_005ffree)
:   The `po_flag_iterator_free` function frees the given iterator.

Function: `const char *` **po\_flag\_next** `(po_flag_iterator_t iterator)` [¶](#index-po_005fflag_005fnext)
:   The `po_flag_next` function returns
    the next flag of the given kind on the given message.
    At the end of the list of flags, it returns `NULL`.

---

Previous: [po\_flag\_iterator\_t API](#po_005fflag_005fiterator_005ft-API), Up: [Writing your own programs that process PO files](#libgettextpo)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 10.13.9 Checking API [¶](#Checking-API-1)

Function: `void` **po\_file\_check\_all** `(po_file_t file, po_xerror_handler_t handler)` [¶](#index-po_005ffile_005fcheck_005fall)
:   Tests whether the entire file is valid, like `msgfmt` does it. If it
    is invalid, passes the reasons to handler.

Function: `void` **po\_message\_check\_all** `(po_message_t message, po_message_iterator_t iterator, po_xerror_handler_t handler)` [¶](#index-po_005fmessage_005fcheck_005fall)
:   Tests message, to be inserted at iterator in a PO file in memory,
    like `msgfmt` does it. If it is invalid, passes the reasons to
    handler. iterator is not modified by this call; it only
    specifies the file and the domain.

Function: `void` **po\_message\_check\_format** `(po_message_t message, po_xerror_handler_t handler)` [¶](#index-po_005fmessage_005fcheck_005fformat)
:   Tests whether the message translation from message is a valid
    format string if the message is marked as being a format string. If it
    is invalid, passes the reasons to handler.

    This function is exported as ‘`po_message_check_format_v2`’ at ABI
    level, but is defined as `po_message_check_format` in C code after
    the inclusion of ‘`<gettext-po.h>`’.

---

Next: [The Programmer’s View](#Programmers), Previous: [Manipulating PO Files](#Manipulating), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 11 Producing Binary MO Files [¶](#Producing-Binary-MO-Files)

* [Invoking the `msgfmt` Program](#msgfmt-Invocation)
* [Invoking the `msgunfmt` Program](#msgunfmt-Invocation)
* [The Format of GNU MO Files](#MO-Files)

---

Next: [Invoking the `msgunfmt` Program](#msgunfmt-Invocation), Up: [Producing Binary MO Files](#Binaries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 11.1 Invoking the `msgfmt` Program [¶](#Invoking-the-msgfmt-Program)

```
msgfmt [option] filename.po ...
```

The `msgfmt` programs generates a binary message catalog from a textual
translation description.

* [Input file location](#Input-file-location-14)
* [Operation mode](#Operation-mode-2)
* [Output file location](#Output-file-location-12)
* [Output file location in Java mode](#Output-file-location-in-Java-mode)
* [Output file location in C# mode](#Output-file-location-in-C_0023-mode)
* [Output file location in Tcl mode](#Output-file-location-in-Tcl-mode)
* [Desktop Entry mode options](#Desktop-Entry-mode-options)
* [XML mode options](#XML-mode-options)
* [Input file syntax](#Input-file-syntax-13)
* [Input file interpretation](#Input-file-interpretation-1)
* [Output details](#Output-details-12)
* [Informative output](#Informative-output-15)

#### 11.1.1 Input file location [¶](#Input-file-location-14)

‘`filename.po …`’

‘`-D directory`’ [¶](#index-_002dD_002c-msgfmt-option)

‘`--directory=directory`’
:   Add directory to the list of directories. Source files are
    searched relative to this list of directories. The resulting binary
    file will be written relative to the current directory, though.

If an input file is ‘`-`’, standard input is read.

#### 11.1.2 Operation mode [¶](#Operation-mode-2)

‘`-j`’ [¶](#index-_002dj_002c-msgfmt-option)

‘`--java`’
:   Java mode: generate a Java `ResourceBundle` class.

‘`--java2`’ [¶](#index-_002d_002djava2_002c-msgfmt-option)
:   Like –java, and assume Java2 (JDK 1.2 or higher).

‘`--csharp`’ [¶](#index-_002d_002dcsharp_002c-msgfmt-option)
:   C# mode: generate a .NET .dll file containing a subclass of
    `GettextResourceSet`.

‘`--csharp-resources`’ [¶](#index-_002d_002dcsharp_002dresources_002c-msgfmt-option)
:   C# resources mode: generate a .NET `.resources` file.

‘`--tcl`’ [¶](#index-_002d_002dtcl_002c-msgfmt-option)
:   Tcl mode: generate a tcl/msgcat `.msg` file.

‘`--qt`’ [¶](#index-_002d_002dqt_002c-msgfmt-option)
:   Qt mode: generate a Qt `.qm` file.

‘`--desktop`’ [¶](#index-_002d_002ddesktop_002c-msgfmt-option)
:   Desktop Entry mode: generate a `.desktop` file.

‘`--xml`’ [¶](#index-_002d_002dxml_002c-msgfmt-option)
:   XML mode: generate an XML file.

#### 11.1.3 Output file location [¶](#Output-file-location-12)

‘`-o file`’ [¶](#index-_002do_002c-msgfmt-option)

‘`--output-file=file`’
:   Write output to specified file.

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgfmt-option)
:   Direct the program to work strictly following the Uniforum/Sun
    implementation. Currently this only affects the naming of the output
    file. If this option is not given the name of the output file is the
    same as the domain name. If the strict Uniforum mode is enabled the
    suffix `.mo` is added to the file name if it is not already
    present.

    We find this behaviour of Sun’s implementation rather silly and so by
    default this mode is *not* selected.

If the output file is ‘`-`’, output is written to standard output.

#### 11.1.4 Output file location in Java mode [¶](#Output-file-location-in-Java-mode)

‘`-r resource`’ [¶](#index-_002dr_002c-msgfmt-option)

‘`--resource=resource`’
:   Specify the resource name.

‘`-l locale`’ [¶](#index-_002dl_002c-msgfmt-option)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

‘`-d directory`’ [¶](#index-_002dd_002c-msgfmt-option)
:   Specify the base directory of classes directory hierarchy.

‘`--source`’ [¶](#index-_002d_002dsource_002c-msgfmt-option)
:   Produce a .java source file, instead of a compiled .class file.

The class name is determined by appending the locale name to the resource name,
separated with an underscore. The ‘`-d`’ option is mandatory. The class
is written under the specified directory.

#### 11.1.5 Output file location in C# mode [¶](#Output-file-location-in-C_0023-mode)

‘`-r resource`’ [¶](#index-_002dr_002c-msgfmt-option-1)

‘`--resource=resource`’
:   Specify the resource name.

‘`-l locale`’ [¶](#index-_002dl_002c-msgfmt-option-1)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

‘`-d directory`’ [¶](#index-_002dd_002c-msgfmt-option-1)
:   Specify the base directory for locale dependent `.dll` files.

The ‘`-l`’ and ‘`-d`’ options are mandatory. The `.dll` file is
written in a subdirectory of the specified directory whose name depends on the
locale.

#### 11.1.6 Output file location in Tcl mode [¶](#Output-file-location-in-Tcl-mode)

‘`-l locale`’ [¶](#index-_002dl_002c-msgfmt-option-2)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

‘`-d directory`’ [¶](#index-_002dd_002c-msgfmt-option-2)
:   Specify the base directory of `.msg` message catalogs.

The ‘`-l`’ and ‘`-d`’ options are mandatory. The `.msg` file is
written in the specified directory.

#### 11.1.7 Desktop Entry mode options [¶](#Desktop-Entry-mode-options)

‘`--template=template`’ [¶](#index-_002d_002dtemplate_002c-msgfmt-option)
:   Specify a .desktop file used as a template.

‘`-k[keywordspec]`’ [¶](#index-_002dk_002c-msgfmt-option)

‘`--keyword[=keywordspec]`’
:   Specify keywordspec as an additional keyword to be looked for.
    Without a keywordspec, the option means to not use default keywords.

‘`-l locale`’ [¶](#index-_002dl_002c-msgfmt-option-3)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

‘`-d directory`’ [¶](#index-_002dd_002c-msgfmt-option-3)
:   Specify the directory where PO files are read. The directory must
    contain the ‘`LINGUAS`’ file.

To generate a ‘`.desktop`’ file for a single locale, you can use it
as follows.

```
msgfmt --desktop --template=template --locale=locale \
  -o file filename.po ...
```

msgfmt provides a special "bulk" operation mode to process multiple
`.po` files at a time.

```
msgfmt --desktop --template=template -d directory -o file
```

msgfmt first reads the ‘`LINGUAS`’ file under directory, and
then processes all ‘`.po`’ files listed there. You can also limit
the locales to a subset, through the ‘`LINGUAS`’ environment
variable.

For either operation modes, the ‘`-o`’ and ‘`--template`’
options are mandatory.

#### 11.1.8 XML mode options [¶](#XML-mode-options)

‘`--template=template`’ [¶](#index-_002d_002dtemplate_002c-msgfmt-option-1)
:   Specify an XML file used as a template.

‘`-L name`’ [¶](#index-_002dL_002c-msgfmt-option)

‘`--language=name`’
:   Specifies the language of the input files.

‘`-l locale`’ [¶](#index-_002dl_002c-msgfmt-option-4)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

‘`-d directory`’ [¶](#index-_002dd_002c-msgfmt-option-4)
:   Specify the base directory of `.po` message catalogs.

‘`--replace-text`’ [¶](#index-_002d_002dreplace_002dtext_002c-msgfmt-option)
:   Output XML with translated text replacing the original text,
    not augmenting the original text.
    With this option, `msgfmt` produces a mono-lingual XML file.
    Without this option, it produces a multi-lingual XML file.

To generate an XML file for a single locale, you can use it as follows.

```
msgfmt --xml --template=template --locale=locale \
  -o file filename.po ...
```

msgfmt provides a special "bulk" operation mode to process multiple
`.po` files at a time.

```
msgfmt --xml --template=template -d directory -o file
```

msgfmt first reads the ‘`LINGUAS`’ file under directory, and
then processes all ‘`.po`’ files listed there. You can also limit
the locales to a subset, through the ‘`LINGUAS`’ environment
variable.

For either operation modes, the ‘`-o`’ and ‘`--template`’
options are mandatory.

If your XML file is not of one of the types covered by the system-wide
installed \*.its files,
you need a particular \*.its file and a corresponding \*.loc file
(see [Preparing Rules for XML Internationalization](#Preparing-ITS-Rules)).
Furthermore you need to store these files
in a directory `parent_dir/its/`

and set the environment variable `GETTEXTDATADIRS` to include
`parent_dir`.
More generally, the value of `GETTEXTDATADIRS` should be
a colon-separated list of directory names.

#### 11.1.9 Input file syntax [¶](#Input-file-syntax-13)

‘`-P`’ [¶](#index-_002dP_002c-msgfmt-option)

‘`--properties-input`’
:   Assume the input files are Java ResourceBundles in Java `.properties`
    syntax, not in PO file syntax.

‘`--stringtable-input`’ [¶](#index-_002d_002dstringtable_002dinput_002c-msgfmt-option)
:   Assume the input files are NeXTstep/GNUstep localized resource files in
    `.strings` syntax, not in PO file syntax.

#### 11.1.10 Input file interpretation [¶](#Input-file-interpretation-1)

‘`-c`’ [¶](#index-_002dc_002c-msgfmt-option)

‘`--check`’
:   Perform all the checks implied by `--check-format`, `--check-header`,
    `--check-domain`.

‘`--check-format`’ [¶](#index-_002d_002dcheck_002dformat_002c-msgfmt-option)
:   Check language dependent format strings.

    If the string represents a format string used in a
    `printf`-like function both strings should have the same number of
    ‘`%`’ format specifiers, with matching types. If the flag
    `c-format` or `possible-c-format` appears in the special
    comment `#,` for this entry a check is performed. For example, the
    check will diagnose using ‘`%.*s`’ against ‘`%s`’, or ‘`%d`’
    against ‘`%s`’, or ‘`%d`’ against ‘`%x`’. It can even handle
    positional parameters.

    Normally the `xgettext` program automatically decides whether a
    string is a format string or not. This algorithm is not perfect,
    though. It might regard a string as a format string though it is not
    used in a `printf`-like function and so `msgfmt` might report
    errors where there are none.

    To solve this problem the programmer can dictate the decision to the
    `xgettext` program (see [C Format Strings](#c_002dformat)). The translator should not
    consider removing the flag from the `#,` line. This "fix" would be
    reversed again as soon as `msgmerge` is called the next time.

‘`--check-header`’ [¶](#index-_002d_002dcheck_002dheader_002c-msgfmt-option)
:   Verify presence and contents of the header entry. See [Filling in the Header Entry](#Header-Entry),
    for a description of the various fields in the header entry.

‘`--check-domain`’ [¶](#index-_002d_002dcheck_002ddomain_002c-msgfmt-option)
:   Check for conflicts between domain directives and the `--output-file`
    option

‘`-C`’ [¶](#index-_002dC_002c-msgfmt-option)

‘`--check-compatibility`’
:   Check that GNU msgfmt behaves like X/Open msgfmt. This will give an error
    when attempting to use the GNU extensions.

‘`--check-accelerators[=char]`’ [¶](#index-_002d_002dcheck_002daccelerators_002c-msgfmt-option)
:   Check presence of keyboard accelerators for menu items. This is based on
    the convention used in some GUIs that a keyboard accelerator in a menu
    item string is designated by an immediately preceding ‘`&`’ character.
    Sometimes a keyboard accelerator is also called "keyboard mnemonic".
    This check verifies that if the untranslated string has exactly one
    ‘`&`’ character, the translated string has exactly one ‘`&`’ as well.
    If this option is given with a char argument, this char should
    be a non-alphanumeric character and is used as keyboard accelerator mark
    instead of ‘`&`’.

‘`-f`’ [¶](#index-_002df_002c-msgfmt-option)

‘`--use-fuzzy`’
:   Use fuzzy entries in output. Note that using this option is usually wrong,
    because fuzzy messages are exactly those which have not been validated by
    a human translator.

#### 11.1.11 Output details [¶](#Output-details-12)

‘`--no-convert`’ [¶](#index-_002d_002dno_002dconvert_002c-msgfmt-option)
:   Don’t convert the messages to UTF-8 encoding. By default, messages are
    converted to UTF-8 encoding before being stored in a MO file; this helps
    avoiding conversions at run time, since nowadays most locales use the
    UTF-8 encoding.

‘`--no-redundancy`’ [¶](#index-_002d_002dno_002dredundancy_002c-msgfmt-option)
:   Don’t pre-expand ISO C 99 <inttypes.h> format string directive macros.
    By default, messages that are marked as `c-format` and contain
    ISO C 99 <inttypes.h> format string directive macros are pre-expanded
    for selected platforms, and these redundant expansions are stored in the
    MO file. These redundant expansions make the translations of these
    messages work with the `gettext` implementation in the `libc`
    of that platform, without requiring GNU `gettext`’s `libintl`.
    The platforms that benefit from this pre-expansion are those with the
    musl libc.

‘`-a number`’ [¶](#index-_002da_002c-msgfmt-option)

‘`--alignment=number`’
:   Align strings to number bytes (default: 1).

‘`--endianness=byteorder`’ [¶](#index-_002d_002dendianness_002c-msgfmt-option)
:   Write out 32-bit numbers in the given byte order. The possible values are
    `big` and `little`. The default is `little`.

    MO files of any endianness can be used on any platform. When a MO file has
    an endianness other than the platform’s one, the 32-bit numbers from the MO
    file are swapped at runtime. The performance impact is negligible.

    This option can be useful to produce MO files that are optimized for one
    platform.

‘`--no-hash`’ [¶](#index-_002d_002dno_002dhash_002c-msgfmt-option)
:   Don’t include a hash table in the binary file. Lookup will be more expensive
    at run time (binary search instead of hash table lookup).

#### 11.1.12 Informative output [¶](#Informative-output-15)

‘`-h`’ [¶](#index-_002dh_002c-msgfmt-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgfmt-option)

‘`--version`’
:   Output version information and exit.

‘`--statistics`’ [¶](#index-_002d_002dstatistics_002c-msgfmt-option)
:   Print statistics about translations. When the option `--verbose` is used
    in combination with `--statistics`, the input file name is printed in
    front of the statistics line.

‘`-v`’ [¶](#index-_002dv_002c-msgfmt-option)

‘`--verbose`’
:   Increase verbosity level.

---

Next: [The Format of GNU MO Files](#MO-Files), Previous: [Invoking the `msgfmt` Program](#msgfmt-Invocation), Up: [Producing Binary MO Files](#Binaries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 11.2 Invoking the `msgunfmt` Program [¶](#Invoking-the-msgunfmt-Program)

```
msgunfmt [option] [file]...
```

The `msgunfmt` program converts a binary message catalog to a
Uniforum style .po file.

* [Operation mode](#Operation-mode-3)
* [Input file location](#Input-file-location-15)
* [Input file location in Java mode](#Input-file-location-in-Java-mode)
* [Input file location in C# mode](#Input-file-location-in-C_0023-mode)
* [Input file location in Tcl mode](#Input-file-location-in-Tcl-mode)
* [Output file location](#Output-file-location-13)
* [Output details](#Output-details-13)
* [Informative output](#Informative-output-16)

#### 11.2.1 Operation mode [¶](#Operation-mode-3)

‘`-j`’ [¶](#index-_002dj_002c-msgunfmt-option)

‘`--java`’
:   Java mode: input is a Java `ResourceBundle` class.

‘`--csharp`’ [¶](#index-_002d_002dcsharp_002c-msgunfmt-option)
:   C# mode: input is a .NET .dll file containing a subclass of
    `GettextResourceSet`.

‘`--csharp-resources`’ [¶](#index-_002d_002dcsharp_002dresources_002c-msgunfmt-option)
:   C# resources mode: input is a .NET `.resources` file.

‘`--tcl`’ [¶](#index-_002d_002dtcl_002c-msgunfmt-option)
:   Tcl mode: input is a tcl/msgcat `.msg` file.

#### 11.2.2 Input file location [¶](#Input-file-location-15)

‘`file …`’
:   Input .mo files.

If no input file is given or if it is ‘`-`’, standard input is read.

#### 11.2.3 Input file location in Java mode [¶](#Input-file-location-in-Java-mode)

‘`-r resource`’ [¶](#index-_002dr_002c-msgunfmt-option)

‘`--resource=resource`’
:   Specify the resource name.

‘`-l locale`’ [¶](#index-_002dl_002c-msgunfmt-option)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

The class name is determined by appending the locale name to the resource name,
separated with an underscore. The class is located using the `CLASSPATH`.

#### 11.2.4 Input file location in C# mode [¶](#Input-file-location-in-C_0023-mode)

‘`-r resource`’ [¶](#index-_002dr_002c-msgunfmt-option-1)

‘`--resource=resource`’
:   Specify the resource name.

‘`-l locale`’ [¶](#index-_002dl_002c-msgunfmt-option-1)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

‘`-d directory`’ [¶](#index-_002dd_002c-msgunfmt-option)
:   Specify the base directory for locale dependent `.dll` files.

The ‘`-l`’ and ‘`-d`’ options are mandatory. The `.msg` file is
located in a subdirectory of the specified directory whose name depends on the
locale.

#### 11.2.5 Input file location in Tcl mode [¶](#Input-file-location-in-Tcl-mode)

‘`-l locale`’ [¶](#index-_002dl_002c-msgunfmt-option-2)

‘`--locale=locale`’
:   Specify the locale name, either a language specification of the form ll
    or a combined language and country specification of the form ll\_CC.

‘`-d directory`’ [¶](#index-_002dd_002c-msgunfmt-option-1)
:   Specify the base directory of `.msg` message catalogs.

The ‘`-l`’ and ‘`-d`’ options are mandatory. The `.msg` file is
located in the specified directory.

#### 11.2.6 Output file location [¶](#Output-file-location-13)

‘`-o file`’ [¶](#index-_002do_002c-msgunfmt-option)

‘`--output-file=file`’
:   Write output to specified file.

The results are written to standard output if no output file is specified
or if it is ‘`-`’.

#### 11.2.7 Output details [¶](#Output-details-13)

‘`--color`’ [¶](#index-_002d_002dcolor_002c-msgunfmt-option)

‘`--color=when`’
:   Specify whether or when to use colors and other text attributes.
    See [The `--color` option](#The-_002d_002dcolor-option) for details.

‘`--style=style_file`’ [¶](#index-_002d_002dstyle_002c-msgunfmt-option)
:   Specify the CSS style rule file to use for `--color`.
    See [The `--style` option](#The-_002d_002dstyle-option) for details.

‘`--force-po`’ [¶](#index-_002d_002dforce_002dpo_002c-msgunfmt-option)
:   Always write an output file even if it contains no message.

‘`-i`’ [¶](#index-_002di_002c-msgunfmt-option)

‘`--indent`’
:   Write the .po file using indented style.

‘`--strict`’ [¶](#index-_002d_002dstrict_002c-msgunfmt-option)
:   Write out a strict Uniforum conforming PO file. Note that this
    Uniforum format should be avoided because it doesn’t support the
    GNU extensions.

‘`-p`’ [¶](#index-_002dp_002c-msgunfmt-option)

‘`--properties-output`’
:   Write out a Java ResourceBundle in Java `.properties` syntax. Note
    that this file format doesn’t support plural forms and silently drops
    obsolete messages.

‘`--stringtable-output`’ [¶](#index-_002d_002dstringtable_002doutput_002c-msgunfmt-option)
:   Write out a NeXTstep/GNUstep localized resource file in `.strings` syntax.
    Note that this file format doesn’t support plural forms.

‘`-w number`’ [¶](#index-_002dw_002c-msgunfmt-option)

‘`--width=number`’
:   Set the output page width. Long strings in the output files will be
    split across multiple lines in order to ensure that each line’s width
    (= number of screen columns) is less or equal to the given number.

‘`--no-wrap`’ [¶](#index-_002d_002dno_002dwrap_002c-msgunfmt-option)
:   Do not break long message lines. Message lines whose width exceeds the
    output page width will not be split into several lines. Only file reference
    lines which are wider than the output page width will be split.

‘`-s`’ [¶](#index-_002ds_002c-msgunfmt-option)

‘`--sort-output`’
:   Generate sorted output. Note that using this option makes it much harder
    for the translator to understand each message’s context.

#### 11.2.8 Informative output [¶](#Informative-output-16)

‘`-h`’ [¶](#index-_002dh_002c-msgunfmt-option)

‘`--help`’
:   Display this help and exit.

‘`-V`’ [¶](#index-_002dV_002c-msgunfmt-option)

‘`--version`’
:   Output version information and exit.

‘`-v`’ [¶](#index-_002dv_002c-msgunfmt-option)

‘`--verbose`’
:   Increase verbosity level.

---

Previous: [Invoking the `msgunfmt` Program](#msgunfmt-Invocation), Up: [Producing Binary MO Files](#Binaries)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 11.3 The Format of GNU MO Files [¶](#The-Format-of-GNU-MO-Files)

The format of the generated MO files is best described by a picture,
which appears below.

The first two words serve the identification of the file. The magic
number will always signal GNU MO files. The number is stored in the
byte order used when the MO file was generated, so the magic number
really is two numbers: `0x950412de` and `0xde120495`.

The second word describes the current revision of the file format,
composed of a major and a minor revision number. The revision numbers
ensure that the readers of MO files can distinguish new formats from
old ones and handle their contents, as far as possible. For now the
major revision is 0 or 1, and the minor revision is also 0 or 1. More
revisions might be added in the future. A program seeing an unexpected
major revision number should stop reading the MO file entirely; whereas
an unexpected minor revision number means that the file can be read but
will not reveal its full contents, when parsed by a program that
supports only smaller minor revision numbers.

The version is kept
separate from the magic number, instead of using different magic
numbers for different formats, mainly because `/etc/magic` is
not updated often.

Follow a number of pointers to later tables in the file, allowing
for the extension of the prefix part of MO files without having to
recompile programs reading them. This might become useful for later
inserting a few flag bits, indication about the charset used, new
tables, or other things.

Then, at offset O and offset T in the picture, two tables
of string descriptors can be found. In both tables, each string
descriptor uses two 32 bits integers, one for the string length,
another for the offset of the string in the MO file, counting in bytes
from the start of the file. The first table contains descriptors
for the original strings, and is sorted so the original strings
are in increasing lexicographical order. The second table contains
descriptors for the translated strings, and is parallel to the first
table: to find the corresponding translation one has to access the
array slot in the second array with the same index.

Having the original strings sorted enables the use of simple binary
search, for when the MO file does not contain an hashing table, or
for when it is not practical to use the hashing table provided in
the MO file. This also has another advantage, as the empty string
in a PO file GNU `gettext` is usually *translated* into
some system information attached to that particular MO file, and the
empty string necessarily becomes the first in both the original and
translated tables, making the system information very easy to find.

The size S of the hash table can be zero. In this case, the
hash table itself is not contained in the MO file. Some people might
prefer this because a precomputed hashing table takes disk space, and
does not win *that* much speed. The hash table contains indices
to the sorted array of strings in the MO file. Conflict resolution is
done by double hashing. The precise hashing algorithm used is fairly
dependent on GNU `gettext` code, and is not documented here.

As for the strings themselves, they follow the hash file, and each
is terminated with a `NUL`, and this `NUL` is not counted in
the length which appears in the string descriptor. The `msgfmt`
program has an option selecting the alignment for MO file strings.
With this option, each string is separately aligned so it starts at
an offset which is a multiple of the alignment value. On some RISC
machines, a correct alignment will speed things up.

Contexts are stored by storing the concatenation of the context, a
`EOT` byte, and the original string, instead of the original string.

Plural forms are stored by letting the plural of the original string
follow the singular of the original string, separated through a
`NUL` byte. The length which appears in the string descriptor
includes both. However, only the singular of the original string
takes part in the hash table lookup. The plural variants of the
translation are all stored consecutively, separated through a
`NUL` byte. Here also, the length in the string descriptor
includes all of them.

The character encoding of the strings can be any standard ASCII-compatible
encoding, such as UTF-8, ISO-8859-1, EUC-JP, etc., as long as the
encoding’s name is stated in the header entry (see [Filling in the Header Entry](#Header-Entry)).
Starting with GNU `gettext` version 0.22, the MO files produced by
`msgfmt` have them in UTF-8 encoding, unless the `msgfmt`
option ‘`--no-convert`’ is used.

Nothing prevents a MO file from having embedded `NUL`s in strings.
However, the program interface currently used already presumes
that strings are `NUL` terminated, so embedded `NUL`s are
somewhat useless. But the MO file format is general enough so other
interfaces would be later possible, if for example, we ever want to
implement wide characters right in MO files, where `NUL` bytes may
accidentally appear. (No, we don’t want to have wide characters in MO
files. They would make the file unnecessarily large, and the
‘`wchar_t`’ type being platform dependent, MO files would be
platform dependent as well.)

This particular issue has been strongly debated in the GNU
`gettext` development forum, and it is expectable that MO file
format will evolve or change over time. It is even possible that many
formats may later be supported concurrently. But surely, we have to
start somewhere, and the MO file format described here is a good start.
Nothing is cast in concrete, and the format may later evolve fairly
easily, so we should feel comfortable with the current approach.

```
        byte
             +------------------------------------------+
          0  | magic number = 0x950412de                |
             |                                          |
          4  | file format revision = 0                 |
             |                                          |
          8  | number of strings                        |  == N
             |                                          |
         12  | offset of table with original strings    |  == O
             |                                          |
         16  | offset of table with translation strings |  == T
             |                                          |
         20  | size of hashing table                    |  == S
             |                                          |
         24  | offset of hashing table                  |  == H
             |                                          |
             .                                          .
             .    (possibly more entries later)         .
             .                                          .
             |                                          |
          O  | length & offset 0th string  ----------------.
      O + 8  | length & offset 1st string  ------------------.
              ...                                    ...   | |
O + ((N-1)*8)| length & offset (N-1)th string           |  | |
             |                                          |  | |
          T  | length & offset 0th translation  ---------------.
      T + 8  | length & offset 1st translation  -----------------.
              ...                                    ...   | | | |
T + ((N-1)*8)| length & offset (N-1)th translation      |  | | | |
             |                                          |  | | | |
          H  | start hash table                         |  | | | |
              ...                                    ...   | | | |
  H + S * 4  | end hash table                           |  | | | |
             |                                          |  | | | |
             | NUL terminated 0th string  <----------------' | | |
             |                                          |    | | |
             | NUL terminated 1st string  <------------------' | |
             |                                          |      | |
              ...                                    ...       | |
             |                                          |      | |
             | NUL terminated 0th translation  <---------------' |
             |                                          |        |
             | NUL terminated 1st translation  <-----------------'
             |                                          |
              ...                                    ...
             |                                          |
             +------------------------------------------+
```

---

Next: [The Translator’s View](#Translators), Previous: [Producing Binary MO Files](#Binaries), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## 12 The Programmer’s View [¶](#The-Programmer_0027s-View)

One aim of the current message catalog implementation provided by
GNU `gettext` was to use the system’s message catalog handling, if the
installer wishes to do so. So we perhaps should first take a look at
the solutions we know about. The people in the POSIX committee did not
manage to agree on one of the semi-official standards which we’ll
describe below. In fact they couldn’t agree on anything, so they decided
only to include an example of an interface. The major Unix vendors
are split in the usage of the two most important specifications: X/Open’s
catgets vs. Uniforum’s gettext interface. We’ll describe them both and
later explain our solution of this dilemma.

* [About `catgets`](#catgets)
* [About `gettext`](#gettext)
* [Comparing the Two Interfaces](#Comparison)
* [Using libintl.a in own programs](#Using-libintl_002ea)
* [Being a `gettext` grok](#gettext-grok)
* [Temporary Notes for the Programmers Chapter](#Temp-Programmers)

---

Next: [About `gettext`](#gettext), Up: [The Programmer’s View](#Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 12.1 About `catgets` [¶](#About-catgets)

The `catgets` implementation is defined in the X/Open Portability
Guide, Volume 3, XSI Supplementary Definitions, Chapter 5. But the
process of creating this standard seemed to be too slow for some of
the Unix vendors so they created their implementations on preliminary
versions of the standard. Of course this leads again to problems while
writing platform independent programs: even the usage of `catgets`
does not guarantee a unique interface.

Another, personal comment on this that only a bunch of committee members
could have made this interface. They never really tried to program
using this interface. It is a fast, memory-saving implementation, an
user can happily live with it. But programmers hate it (at least I and
some others do…)

But we must not forget one point: after all the trouble with transferring
the rights on Unix they at last came to X/Open, the very same who
published this specification. This leads me to making the prediction
that this interface will be in future Unix standards (e.g. Spec1170) and
therefore part of all Unix implementation (implementations, which are
*allowed* to wear this name).

* [The Interface](#Interface-to-catgets)
* [Problems with the `catgets` Interface?!](#Problems-with-catgets)

---

Next: [Problems with the `catgets` Interface?!](#Problems-with-catgets), Up: [About `catgets`](#catgets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.1.1 The Interface [¶](#The-Interface)

The interface to the `catgets` implementation consists of three
functions which correspond to those used in file access: `catopen`
to open the catalog for using, `catgets` for accessing the message
tables, and `catclose` for closing after work is done. Prototypes
for the functions and the needed definitions are in the
`<nl_types.h>` header file.

`catopen` is used like in this:

```
nl_catd catd = catopen ("catalog_name", 0);
```

The function takes as the argument the name of the catalog. This usual
refers to the name of the program or the package. The second parameter
is not further specified in the standard. I don’t even know whether it
is implemented consistently among various systems. So the common advice
is to use `0` as the value. The return value is a handle to the
message catalog, equivalent to handles to file returned by `open`.

This handle is of course used in the `catgets` function which can
be used like this:

```
char *translation = catgets (catd, set_no, msg_id, "original string");
```

The first parameter is this catalog descriptor. The second parameter
specifies the set of messages in this catalog, in which the message
described by `msg_id` is obtained. `catgets` therefore uses a
three-stage addressing:

```
catalog name ⇒ set number ⇒ message ID ⇒ translation
```

The fourth argument is not used to address the translation. It is given
as a default value in case when one of the addressing stages fail. One
important thing to remember is that although the return type of catgets
is `char *` the resulting string *must not* be changed. It
should better be `const char *`, but the standard is published in
1988, one year before ANSI C.

The last of these functions is used and behaves as expected:

```
catclose (catd);
```

After this no `catgets` call using the descriptor is legal anymore.

---

Previous: [The Interface](#Interface-to-catgets), Up: [About `catgets`](#catgets)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 12.1.2 Problems with the `catgets` Interface?! [¶](#Problems-with-the-catgets-Interface_003f_0021)

Now that this description seemed to be really easy — where are the
problems we speak of? In fact the interface could be used in a
reasonable way, but constructing the message catalogs is a pain. The
reason for this lies in the third argument of `catgets`: the unique
message ID. This has to be a numeric value for all messages in a single
set. Perhaps you could imagine the problems keeping such a list while
changing the source code. Add a new message here, remove one there. Of
course there have been developed a lot of tools helping to organize this
chaos but one as the other fails in one aspect or the other. We don’t
want to say that the other approach has no problems but they are far
more easy to manage.

---

Next: [Comparing the Two Interfaces](#Comparison), Previous: [About `catgets`](#catgets), Up: [The Programmer’s View](#Programmers)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
