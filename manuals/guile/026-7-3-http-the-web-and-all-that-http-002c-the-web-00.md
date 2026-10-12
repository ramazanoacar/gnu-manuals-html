### 7.3 HTTP, the Web, and All That [¶](#HTTP_002c-the-Web_002c-and-All-That)

It has always been possible to connect computers together and share
information between them, but the rise of the World Wide Web over the
last couple of decades has made it much easier to do so. The result is
a richly connected network of computation, in which Guile forms a part.

By “the web”, we mean the HTTP protocol[26](#FOOT26) as handled by
servers, clients, proxies, caches, and the various kinds of messages and
message components that can be sent and received by that protocol,
notably HTML.

On one level, the web is text in motion: the protocols themselves are
textual (though the payload may be binary), and it’s possible to create
a socket and speak text to the web. But such an approach is obviously
primitive. This section details the higher-level data types and
operations provided by Guile: URIs, HTTP request and response records,
and a conventional web server implementation.

The material in this section is arranged in ascending order, in which
later concepts build on previous ones. If you prefer to start with the
highest-level perspective, see [Web Examples](#Web-Examples), and work your way
back.

* [Types and the Web](#Types-and-the-Web)
* [Universal Resource Identifiers](#URIs)
* [The Hyper-Text Transfer Protocol](#HTTP)
* [HTTP Headers](#HTTP-Headers)
* [Transfer Codings](#Transfer-Codings)
* [HTTP Requests](#Requests)
* [HTTP Responses](#Responses)
* [Web Client](#Web-Client)
* [Web Server](#Web-Server)
* [Web Examples](#Web-Examples)

---

Next: [Universal Resource Identifiers](#URIs), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.1 Types and the Web [¶](#Types-and-the-Web-1)

It is a truth universally acknowledged, that a program with good use of
data types, will be free from many common bugs. Unfortunately, the
common practice in web programming seems to ignore this maxim. This
subsection makes the case for expressive data types in web programming.

By “expressive data types”, we mean that the data types *say*
something about how a program solves a problem. For example, if we
choose to represent dates using SRFI 19 date records (see [SRFI-19 - Time/Date Library](#SRFI_002d19)),
this indicates that there is a part of the program that will always have
valid dates. Error handling for a number of basic cases, like invalid
dates, occurs on the boundary in which we produce a SRFI 19 date record
from other types, like strings.

With regards to the web, data types are helpful in the two broad phases
of HTTP messages: parsing and generation.

Consider a server, which has to parse a request, and produce a response.
Guile will parse the request into an HTTP request object
(see [HTTP Requests](#Requests)), with each header parsed into an appropriate Scheme
data type. This transition from an incoming stream of characters to
typed data is a state change in a program—the strings might parse, or
they might not, and something has to happen if they do not. (Guile
throws an error in this case.) But after you have the parsed request,
“client” code (code built on top of the Guile web framework) will not
have to check for syntactic validity. The types already make this
information manifest.

This state change on the parsing boundary makes programs more robust,
as they themselves are freed from the need to do a number of common
error checks, and they can use normal Scheme procedures to handle a
request instead of ad-hoc string parsers.

The need for types on the response generation side (in a server) is more
subtle, though not less important. Consider the example of a POST
handler, which prints out the text that a user submits from a form.
Such a handler might include a procedure like this:

```
;; First, a helper procedure
(define (para . contents)
  (string-append "<p>" (string-concatenate contents) "</p>"))

;; Now the meat of our simple web application
(define (you-said text)
  (para "You said: " text))

(display (you-said "Hi!"))
⊣ <p>You said: Hi!</p>
```

This is a perfectly valid implementation, provided that the incoming
text does not contain the special HTML characters ‘`<`’, ‘`>`’, or
‘`&`’. But this provision of a restricted character set is not
reflected anywhere in the program itself: we must *assume* that the
programmer understands this, and performs the check elsewhere.

Unfortunately, the short history of the practice of programming does not
bear out this assumption. A *cross-site scripting* (XSS)
vulnerability is just such a common error in which unfiltered user input
is allowed into the output. A user could submit a crafted comment to
your web site which results in visitors running malicious Javascript,
within the security context of your domain:

```
(display (you-said "<script src=\"http://bad.com/nasty.js\" />"))
⊣ <p>You said: <script src="http://bad.com/nasty.js" /></p>
```

The fundamental problem here is that both user data and the program
template are represented using strings. This identity means that types
can’t help the programmer to make a distinction between these two, so
they get confused.

There are a number of possible solutions, but perhaps the best is to
treat HTML not as strings, but as native s-expressions: as SXML. The
basic idea is that HTML is either text, represented by a string, or an
element, represented as a tagged list. So ‘`foo`’ becomes
‘`"foo"`’, and ‘`<b>foo</b>`’ becomes ‘`(b "foo")`’.
Attributes, if present, go in a tagged list headed by ‘`@`’, like
‘`(img (@ (src "http://example.com/foo.png")))`’. See [SXML](#SXML), for
more information.

The good thing about SXML is that HTML elements cannot be confused with
text. Let’s make a new definition of `para`:

```
(define (para . contents)
  `(p ,@contents))

(use-modules (sxml simple))
(sxml->xml (you-said "Hi!"))
⊣ <p>You said: Hi!</p>

(sxml->xml (you-said "<i>Rats, foiled again!</i>"))
⊣ <p>You said: &lt;i&gt;Rats, foiled again!&lt;/i&gt;</p>
```

So we see in the second example that HTML elements cannot be unwittingly
introduced into the output. However it is now perfectly acceptable to
pass SXML to `you-said`; in fact, that is the big advantage of SXML
over everything-as-a-string.

```
(sxml->xml (you-said (you-said "<Hi!>")))
⊣ <p>You said: <p>You said: &lt;Hi!&gt;</p></p>
```

The SXML types allow procedures to *compose*. The types make
manifest which parts are HTML elements, and which are text. So you
needn’t worry about escaping user input; the type transition back to a
string handles that for you. XSS vulnerabilities are a thing
of the past.

Well. That’s all very nice and opinionated and such, but how do I use
the thing? Read on!

---

Next: [The Hyper-Text Transfer Protocol](#HTTP), Previous: [Types and the Web](#Types-and-the-Web), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.2 Universal Resource Identifiers [¶](#Universal-Resource-Identifiers)

Guile provides a standard data type for Universal Resource Identifiers
(URIs), as defined in RFC 3986.

The generic URI syntax is as follows:

```
URI-reference := [scheme ":"] ["//" [userinfo "@"] host [":" port]] path \
                 [ "?" query ] [ "#" fragment ]
```

For example, in the URI, ‘`http://www.gnu.org/help/`’, the
scheme is `http`, the host is `www.gnu.org`, the path is
`/help/`, and there is no userinfo, port, query, or fragment.

Userinfo is something of an abstraction, as some legacy URI schemes
allowed userinfo of the form `username:passwd`. But
since passwords do not belong in URIs, the RFC does not want to condone
this practice, so it calls anything before the `@` sign
*userinfo*.

```
(use-modules (web uri))
```

The following procedures can be found in the `(web uri)`
module. Load it into your Guile, using a form like the above, to have
access to them.

The most common way to build a URI from Scheme is with the
`build-uri` function.

Scheme Procedure: **build-uri** scheme [#:userinfo=`#f`] [#:host=`#f`] [#:port=`#f`] [#:path=`""`] [#:query=`#f`] [#:fragment=`#f`] [#:validate?=`#t`] [¶](#index-build_002duri)
:   Construct a URI. scheme should be a symbol, port either a
    positive, exact integer or `#f`, and the rest of the fields are
    either strings or `#f`. If validate? is true, also run some
    consistency checks to make sure that the constructed URI is valid.

Scheme Procedure: **uri?** obj [¶](#index-uri_003f)
:   Return `#t` if obj is a URI.

Guile, URIs are represented as URI records, with a number of associated
accessors.

Scheme Procedure: **uri-scheme** uri [¶](#index-uri_002dscheme)

Scheme Procedure: **uri-userinfo** uri [¶](#index-uri_002duserinfo)

Scheme Procedure: **uri-host** uri [¶](#index-uri_002dhost)

Scheme Procedure: **uri-port** uri [¶](#index-uri_002dport)

Scheme Procedure: **uri-path** uri [¶](#index-uri_002dpath)

Scheme Procedure: **uri-query** uri [¶](#index-uri_002dquery)

Scheme Procedure: **uri-fragment** uri [¶](#index-uri_002dfragment)
:   Field accessors for the URI record type. The URI scheme will be a
    symbol, or `#f` if the object is a relative-ref (see below). The
    port will be either a positive, exact integer or `#f`, and the rest
    of the fields will be either strings or `#f` if not present.

Scheme Procedure: **string->uri** string [¶](#index-string_002d_003euri)
:   Parse string into a URI object. Return `#f` if the string
    could not be parsed.

Scheme Procedure: **uri->string** uri [#:include-fragment?=`#t`] [¶](#index-uri_002d_003estring)
:   Serialize uri to a string. If the URI has a port that is the
    default port for its scheme, the port is not included in the
    serialization. If include-fragment? is given as false, the
    resulting string will omit the fragment (if any).

Scheme Procedure: **declare-default-port!** scheme port [¶](#index-declare_002ddefault_002dport_0021)
:   Declare a default port for the given URI scheme.

Scheme Procedure: **uri-decode** str [#:encoding=`"utf-8"`] [#:decode-plus-to-space? #t] [¶](#index-uri_002ddecode)
:   Percent-decode the given str, according to encoding, which
    should be the name of a character encoding.

    Note that this function should not generally be applied to a full URI
    string. For paths, use `split-and-decode-uri-path` instead. For
    query strings, split the query on `&` and `=` boundaries, and
    decode the components separately.

    Note also that percent-encoded strings encode *bytes*, not
    characters. There is no guarantee that a given byte sequence is a valid
    string encoding. Therefore this routine may signal an error if the
    decoded bytes are not valid for the given encoding. Pass `#f` for
    encoding if you want decoded bytes as a bytevector directly.
    See [`set-port-encoding!`](#Ports), for more information on
    character encodings.

    If decode-plus-to-space? is true, which is the default, also
    replace instances of the plus character ‘`+`’ with a space character.
    This is needed when parsing `application/x-www-form-urlencoded`
    data.

    Returns a string of the decoded characters, or a bytevector if
    encoding was `#f`.

Scheme Procedure: **uri-encode** str [#:encoding=`"utf-8"`] [#:unescaped-chars] [¶](#index-uri_002dencode)
:   Percent-encode any character not in the character set,
    unescaped-chars.

    The default character set includes alphanumerics from ASCII, as well as
    the special characters ‘`-`’, ‘`.`’, ‘`_`’, and ‘`~`’. Any
    other character will be percent-encoded, by writing out the character to
    a bytevector within the given encoding, then encoding each byte as
    `%HH`, where HH is the hexadecimal representation of
    the byte.

Scheme Procedure: **split-and-decode-uri-path** path [¶](#index-split_002dand_002ddecode_002duri_002dpath)
:   Split path into its components, and decode each component,
    removing empty components.

    For example, `"/foo/bar%20baz/"` decodes to the two-element list,
    `("foo" "bar baz")`.

Scheme Procedure: **encode-and-join-uri-path** parts [¶](#index-encode_002dand_002djoin_002duri_002dpath)
:   URI-encode each element of parts, which should be a list of
    strings, and join the parts together with `/` as a delimiter.

    For example, the list `("scrambled eggs" "biscuits&gravy")` encodes
    as `"scrambled%20eggs/biscuits%26gravy"`.

#### Subtypes of URI [¶](#Subtypes-of-URI)

As we noted above, not all URI objects have a scheme. You might have
noted in the “generic URI syntax” example that the left-hand side of
that grammar definition was URI-reference, not URI. A
*URI-reference* is a generalization of a URI where the scheme is
optional. If no scheme is specified, it is taken to be relative to some
other related URI. A common use of URI references is when you want to
be vague regarding the choice of HTTP or HTTPS – serving a web page
referring to `/foo.css` will use HTTPS if loaded over HTTPS, or
HTTP otherwise.

Scheme Procedure: **build-uri-reference** [#:scheme=`#f`] [#:userinfo=`#f`] [#:host=`#f`] [#:port=`#f`] [#:path=`""`] [#:query=`#f`] [#:fragment=`#f`] [#:validate?=`#t`] [¶](#index-build_002duri_002dreference)
:   Like `build-uri`, but with an optional scheme.

Scheme Procedure: **uri-reference?** obj [¶](#index-uri_002dreference_003f)
:   Return `#t` if obj is a URI-reference. This is the most
    general URI predicate, as it includes not only full URIs that have
    schemes (those that match `uri?`) but also URIs without schemes.

It’s also possible to build a *relative-ref*: a URI-reference that
explicitly lacks a scheme.

Scheme Procedure: **build-relative-ref** [#:userinfo=`#f`] [#:host=`#f`] [#:port=`#f`] [#:path=`""`] [#:query=`#f`] [#:fragment=`#f`] [#:validate?=`#t`] [¶](#index-build_002drelative_002dref)
:   Like `build-uri`, but with no scheme.

Scheme Procedure: **relative-ref?** obj [¶](#index-relative_002dref_003f)
:   Return `#t` if obj is a “relative-ref”: a URI-reference
    that has no scheme. Every URI-reference will either match `uri?`
    or `relative-ref?` (but not both).

In case it’s not clear from the above, the most general of these URI
types is the URI-reference, with `build-uri-reference` as the most
general constructor. `build-uri` and `build-relative-ref`
enforce enforce specific restrictions on the URI-reference. The most
generic URI parser is then `string->uri-reference`, and there is
also a parser for when you know that you want a relative-ref.

Note that `uri?` will only return `#t` for URI objects that
have schemes; that is, it rejects relative-refs.

Scheme Procedure: **string->uri-reference** string [¶](#index-string_002d_003euri_002dreference)
:   Parse string into a URI object, while not requiring a scheme.
    Return `#f` if the string could not be parsed.

Scheme Procedure: **string->relative-ref** string [¶](#index-string_002d_003erelative_002dref)
:   Parse string into a URI object, while asserting that no scheme is
    present. Return `#f` if the string could not be parsed.

---

Next: [HTTP Headers](#HTTP-Headers), Previous: [Universal Resource Identifiers](#URIs), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.3 The Hyper-Text Transfer Protocol [¶](#The-Hyper_002dText-Transfer-Protocol)

The initial motivation for including web functionality in Guile, rather
than rely on an external package, was to establish a standard base on
which people can share code. To that end, we continue the focus on data
types by providing a number of low-level parsers and unparsers for
elements of the HTTP protocol.

If you are want to skip the low-level details for now and move on to web
pages, see [Web Client](#Web-Client), and see [Web Server](#Web-Server). Otherwise, load the
HTTP module, and read on.

```
(use-modules (web http))
```

The focus of the `(web http)` module is to parse and unparse
standard HTTP headers, representing them to Guile as native data
structures. For example, a `Date:` header will be represented as a
SRFI-19 date record (see [SRFI-19 - Time/Date Library](#SRFI_002d19)), rather than as a string.

Guile tries to follow RFCs fairly strictly—the road to perdition being
paved with compatibility hacks—though some allowances are made for
not-too-divergent texts.

Header names are represented as lower-case symbols.

Scheme Procedure: **string->header** name [¶](#index-string_002d_003eheader)
:   Parse name to a symbolic header name.

Scheme Procedure: **header->string** sym [¶](#index-header_002d_003estring)
:   Return the string form for the header named sym.

For example:

```
(string->header "Content-Length")
⇒ content-length
(header->string 'content-length)
⇒ "Content-Length"

(string->header "FOO")
⇒ foo
(header->string 'foo)
⇒ "Foo"
```

Guile keeps a registry of known headers, their string names, and some
parsing and serialization procedures. If a header is unknown, its
string name is simply its symbol name in title-case.

Scheme Procedure: **known-header?** sym [¶](#index-known_002dheader_003f)
:   Return `#t` if sym is a known header, with associated
    parsers and serialization procedures, or `#f` otherwise.

Scheme Procedure: **header-parser** sym [¶](#index-header_002dparser)
:   Return the value parser for headers named sym. The result is a
    procedure that takes one argument, a string, and returns the parsed
    value. If the header isn’t known to Guile, a default parser is returned
    that passes through the string unchanged.

Scheme Procedure: **header-validator** sym [¶](#index-header_002dvalidator)
:   Return a predicate which returns `#t` if the given value is valid
    for headers named sym. The default validator for unknown headers
    is `string?`.

Scheme Procedure: **header-writer** sym [¶](#index-header_002dwriter)
:   Return a procedure that writes values for headers named sym to a
    port. The resulting procedure takes two arguments: a value and a port.
    The default writer is `display`.

For more on the set of headers that Guile knows about out of the box,
see [HTTP Headers](#HTTP-Headers). To add your own, use the `declare-header!`
procedure:

Scheme Procedure: **declare-header!** name parser validator writer [#:multiple?=`#f`] [¶](#index-declare_002dheader_0021)
:   Declare a parser, validator, and writer for a given header.

For example, let’s say you are running a web server behind some sort of
proxy, and your proxy adds an `X-Client-Address` header, indicating
the IPv4 address of the original client. You would like for the HTTP
request record to parse out this header to a Scheme value, instead of
leaving it as a string. You could register this header with Guile’s
HTTP stack like this:

```
(declare-header! "X-Client-Address"
  (lambda (str)
    (inet-pton AF_INET str))
  (lambda (ip)
    (and (integer? ip) (exact? ip) (<= 0 ip #xffffffff)))
  (lambda (ip port)
    (display (inet-ntop AF_INET ip) port)))
```

Scheme Procedure: **declare-opaque-header!** name [¶](#index-declare_002dopaque_002dheader_0021)
:   A specialised version of `declare-header!` for the case in which
    you want a header’s value to be returned/written “as-is”.

Scheme Procedure: **valid-header?** sym val [¶](#index-valid_002dheader_003f)
:   Return a true value if val is a valid Scheme value for the header
    with name sym, or `#f` otherwise.

Now that we have a generic interface for reading and writing headers, we
do just that.

Scheme Procedure: **read-header** port [¶](#index-read_002dheader)
:   Read one HTTP header from port. Return two values: the header
    name and the parsed Scheme value. May raise an exception if the header
    was known but the value was invalid.

    Returns the end-of-file object for both values if the end of the message
    body was reached (i.e., a blank line).

Scheme Procedure: **parse-header** name val [¶](#index-parse_002dheader)
:   Parse val, a string, with the parser for the header named
    name. Returns the parsed value.

Scheme Procedure: **write-header** name val port [¶](#index-write_002dheader)
:   Write the given header name and value to port, using the writer
    from `header-writer`.

Scheme Procedure: **read-headers** port [¶](#index-read_002dheaders)
:   Read the headers of an HTTP message from port, returning them
    as an ordered alist.

Scheme Procedure: **write-headers** headers port [¶](#index-write_002dheaders)
:   Write the given header alist to port. Doesn’t write the final
    ‘`\r\n`’, as the user might want to add another header.

The `(web http)` module also has some utility procedures to read
and write request and response lines.

Scheme Procedure: **parse-http-method** str [start] [end] [¶](#index-parse_002dhttp_002dmethod)
:   Parse an HTTP method from str. The result is an upper-case symbol,
    like `GET`.

Scheme Procedure: **parse-http-version** str [start] [end] [¶](#index-parse_002dhttp_002dversion)
:   Parse an HTTP version from str, returning it as a major–minor
    pair. For example, `HTTP/1.1` parses as the pair of integers,
    `(1 . 1)`.

Scheme Procedure: **parse-request-uri** str [start] [end] [¶](#index-parse_002drequest_002duri)
:   Parse a URI from an HTTP request line. Note that URIs in requests do not
    have to have a scheme or host name. The result is a URI object.

Scheme Procedure: **read-request-line** port [¶](#index-read_002drequest_002dline)
:   Read the first line of an HTTP request from port, returning three
    values: the method, the URI, and the version.

Scheme Procedure: **write-request-line** method uri version port [¶](#index-write_002drequest_002dline)
:   Write the first line of an HTTP request to port.

Scheme Procedure: **read-response-line** port [¶](#index-read_002dresponse_002dline)
:   Read the first line of an HTTP response from port, returning three
    values: the HTTP version, the response code, and the “reason phrase”.

Scheme Procedure: **write-response-line** version code reason-phrase port [¶](#index-write_002dresponse_002dline)
:   Write the first line of an HTTP response to port.

---

Next: [Transfer Codings](#Transfer-Codings), Previous: [The Hyper-Text Transfer Protocol](#HTTP), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.4 HTTP Headers [¶](#HTTP-Headers-1)

In addition to defining the infrastructure to parse headers, the
`(web http)` module defines specific parsers and unparsers for all
headers defined in the HTTP/1.1 standard.

For example, if you receive a header named ‘`Accept-Language`’ with a
value ‘`en, es;q=0.8`’, Guile parses it as a quality list (defined
below):

```
(parse-header 'accept-language "en, es;q=0.8")
⇒ ((1000 . "en") (800 . "es"))
```

The format of the value for ‘`Accept-Language`’ headers is defined
below, along with all other headers defined in the HTTP standard. (If
the header were unknown, the value would have been returned as a
string.)

For brevity, the header definitions below are given in the form,
Type `name`, indicating that values for the header
`name` will be of the given Type. Since Guile
internally treats header names in lower case, in this document we give
types title-cased names. A short description of the each header’s
purpose and an example follow.

For full details on the meanings of all of these headers, see the HTTP
1.1 standard, RFC 2616.

* [HTTP Header Types](#HTTP-Header-Types)
* [General Headers](#General-Headers)
* [Entity Headers](#Entity-Headers)
* [Request Headers](#Request-Headers)
* [Response Headers](#Response-Headers)

#### 7.3.4.1 HTTP Header Types [¶](#HTTP-Header-Types)

Here we define the types that are used below, when defining headers.

HTTP Header Type: **Date** [¶](#index-Date)
:   A SRFI-19 date.

HTTP Header Type: **KVList** [¶](#index-KVList)
:   A list whose elements are keys or key-value pairs. Keys are parsed to
    symbols. Values are strings by default. Non-string values are the
    exception, and are mentioned explicitly below, as appropriate.

HTTP Header Type: **SList** [¶](#index-SList)
:   A list of strings.

HTTP Header Type: **Quality** [¶](#index-Quality)
:   An exact integer between 0 and 1000. Qualities are used to express
    preference, given multiple options. An option with a quality of 870,
    for example, is preferred over an option with quality 500.

    (Qualities are written out over the wire as numbers between 0.0 and
    1.0, but since the standard only allows three digits after the decimal,
    it’s equivalent to integers between 0 and 1000, so that’s what Guile
    uses.)

HTTP Header Type: **QList** [¶](#index-QList)
:   A quality list: a list of pairs, the car of which is a quality, and the
    cdr a string. Used to express a list of options, along with their
    qualities.

HTTP Header Type: **ETag** [¶](#index-ETag)
:   An entity tag, represented as a pair. The car of the pair is an opaque
    string, and the cdr is `#t` if the entity tag is a “strong” entity
    tag, and `#f` otherwise.

#### 7.3.4.2 General Headers [¶](#General-Headers)

General HTTP headers may be present in any HTTP message.

HTTP Header: `KVList` **cache-control** [¶](#index-cache_002dcontrol)
:   A key-value list of cache-control directives. See RFC 2616, for more
    details.

    If present, parameters to `max-age`, `max-stale`,
    `min-fresh`, and `s-maxage` are all parsed as non-negative
    integers.

    If present, parameters to `private` and `no-cache` are parsed
    as lists of header names, as symbols.

    ```
    (parse-header 'cache-control "no-cache,no-store"
    ⇒ (no-cache no-store)
    (parse-header 'cache-control "no-cache=\"Authorization,Date\",no-store"
    ⇒ ((no-cache . (authorization date)) no-store)
    (parse-header 'cache-control "no-cache=\"Authorization,Date\",max-age=10"
    ⇒ ((no-cache . (authorization date)) (max-age . 10))
    ```

HTTP Header: `List` **connection** [¶](#index-connection)
:   A list of header names that apply only to this HTTP connection, as
    symbols. Additionally, the symbol ‘`close`’ may be present, to
    indicate that the server should close the connection after responding to
    the request.

    ```
    (parse-header 'connection "close")
    ⇒ (close)
    ```

HTTP Header: `Date` **date** [¶](#index-date-2)
:   The date that a given HTTP message was originated.

    ```
    (parse-header 'date "Tue, 15 Nov 1994 08:12:31 GMT")
    ⇒ #<date ...>
    ```

HTTP Header: `KVList` **pragma** [¶](#index-pragma)
:   A key-value list of implementation-specific directives.

    ```
    (parse-header 'pragma "no-cache, broccoli=tasty")
    ⇒ (no-cache (broccoli . "tasty"))
    ```

HTTP Header: `List` **trailer** [¶](#index-trailer)
:   A list of header names which will appear after the message body, instead
    of with the message headers.

    ```
    (parse-header 'trailer "ETag")
    ⇒ (etag)
    ```

HTTP Header: `List` **transfer-encoding** [¶](#index-transfer_002dencoding)
:   A list of transfer codings, expressed as key-value lists. The only
    transfer coding defined by the specification is `chunked`.

    ```
    (parse-header 'transfer-encoding "chunked")
    ⇒ ((chunked))
    ```

HTTP Header: `List` **upgrade** [¶](#index-upgrade)
:   A list of strings, indicating additional protocols that a server could use
    in response to a request.

    ```
    (parse-header 'upgrade "WebSocket")
    ⇒ ("WebSocket")
    ```

FIXME: parse out more fully?

HTTP Header: `List` **via** [¶](#index-via)
:   A list of strings, indicating the protocol versions and hosts of
    intermediate servers and proxies. There may be multiple `via`
    headers in one message.

    ```
    (parse-header 'via "1.0 venus, 1.1 mars")
    ⇒ ("1.0 venus" "1.1 mars")
    ```

HTTP Header: `List` **warning** [¶](#index-warning)
:   A list of warnings given by a server or intermediate proxy. Each
    warning is a itself a list of four elements: a code, as an exact integer
    between 0 and 1000, a host as a string, the warning text as a string,
    and either `#f` or a SRFI-19 date.

    There may be multiple `warning` headers in one message.

    ```
    (parse-header 'warning "123 foo \"core breach imminent\"")
    ⇒ ((123 "foo" "core-breach imminent" #f))
    ```

#### 7.3.4.3 Entity Headers [¶](#Entity-Headers)

Entity headers may be present in any HTTP message, and refer to the
resource referenced in the HTTP request or response.

HTTP Header: `List` **allow** [¶](#index-allow)
:   A list of allowed methods on a given resource, as symbols.

    ```
    (parse-header 'allow "GET, HEAD")
    ⇒ (GET HEAD)
    ```

HTTP Header: `List` **content-encoding** [¶](#index-content_002dencoding)
:   A list of content codings, as symbols.

    ```
    (parse-header 'content-encoding "gzip")
    ⇒ (gzip)
    ```

HTTP Header: `List` **content-language** [¶](#index-content_002dlanguage)
:   The languages that a resource is in, as strings.

    ```
    (parse-header 'content-language "en")
    ⇒ ("en")
    ```

HTTP Header: `UInt` **content-length** [¶](#index-content_002dlength)
:   The number of bytes in a resource, as an exact, non-negative integer.

    ```
    (parse-header 'content-length "300")
    ⇒ 300
    ```

HTTP Header: `URI` **content-location** [¶](#index-content_002dlocation)
:   The canonical URI for a resource, in the case that it is also accessible
    from a different URI.

    ```
    (parse-header 'content-location "http://example.com/foo")
    ⇒ #<<uri> ...>
    ```

HTTP Header: `String` **content-md5** [¶](#index-content_002dmd5)
:   The MD5 digest of a resource.

    ```
    (parse-header 'content-md5 "ffaea1a79810785575e29e2bd45e2fa5")
    ⇒ "ffaea1a79810785575e29e2bd45e2fa5"
    ```

HTTP Header: `List` **content-range** [¶](#index-content_002drange)
:   Range specification as a list of three elements: the symbol
    `bytes`, either the symbol `*` or a pair of integers
    indicating the byte range, and either `*` or an integer indicating
    the instance length. Used to indicate that a response only includes
    part of a resource.

    ```
    (parse-header 'content-range "bytes 10-20/*")
    ⇒ (bytes (10 . 20) *)
    ```

HTTP Header: `List` **content-type** [¶](#index-content_002dtype)
:   The MIME type of a resource, as a symbol, along with any parameters.

    ```
    (parse-header 'content-type "text/plain")
    ⇒ (text/plain)
    (parse-header 'content-type "text/plain;charset=utf-8")
    ⇒ (text/plain (charset . "utf-8"))
    ```

    Note that the `charset` parameter is something of a misnomer, and
    the HTTP specification admits this. It specifies the *encoding* of
    the characters, not the character set.

HTTP Header: `Date` **expires** [¶](#index-expires)
:   The date/time after which the resource given in a response is considered
    stale.

    ```
    (parse-header 'expires "Tue, 15 Nov 1994 08:12:31 GMT")
    ⇒ #<date ...>
    ```

HTTP Header: `Date` **last-modified** [¶](#index-last_002dmodified)
:   The date/time on which the resource given in a response was last
    modified.

    ```
    (parse-header 'expires "Tue, 15 Nov 1994 08:12:31 GMT")
    ⇒ #<date ...>
    ```

#### 7.3.4.4 Request Headers [¶](#Request-Headers)

Request headers may only appear in an HTTP request, not in a response.

HTTP Header: `List` **accept** [¶](#index-accept-1)
:   A list of preferred media types for a response. Each element of the
    list is itself a list, in the same format as `content-type`.

    ```
    (parse-header 'accept "text/html,text/plain;charset=utf-8")
    ⇒ ((text/html) (text/plain (charset . "utf-8")))
    ```

    Preference is expressed with quality values:

    ```
    (parse-header 'accept "text/html;q=0.8,text/plain;q=0.6")
    ⇒ ((text/html (q . 800)) (text/plain (q . 600)))
    ```

HTTP Header: `QList` **accept-charset** [¶](#index-accept_002dcharset)
:   A quality list of acceptable charsets. Note again that what HTTP calls
    a “charset” is what Guile calls a “character encoding”.

    ```
    (parse-header 'accept-charset "iso-8859-5, unicode-1-1;q=0.8")
    ⇒ ((1000 . "iso-8859-5") (800 . "unicode-1-1"))
    ```

HTTP Header: `QList` **accept-encoding** [¶](#index-accept_002dencoding)
:   A quality list of acceptable content codings.

    ```
    (parse-header 'accept-encoding "gzip,identity=0.8")
    ⇒ ((1000 . "gzip") (800 . "identity"))
    ```

HTTP Header: `QList` **accept-language** [¶](#index-accept_002dlanguage)
:   A quality list of acceptable languages.

    ```
    (parse-header 'accept-language "cn,en=0.75")
    ⇒ ((1000 . "cn") (750 . "en"))
    ```

HTTP Header: `Pair` **authorization** [¶](#index-authorization)
:   Authorization credentials. The car of the pair indicates the
    authentication scheme, like `basic`. For basic authentication, the
    cdr of the pair will be the base64-encoded ‘`user:pass`’
    string. For other authentication schemes, like `digest`, the cdr
    will be a key-value list of credentials.

    ```
    (parse-header 'authorization "Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ=="
    ⇒ (basic . "QWxhZGRpbjpvcGVuIHNlc2FtZQ==")
    ```

HTTP Header: `List` **expect** [¶](#index-expect-1)
:   A list of expectations that a client has of a server. The expectations
    are key-value lists.

    ```
    (parse-header 'expect "100-continue")
    ⇒ ((100-continue))
    ```

HTTP Header: `String` **from** [¶](#index-from)
:   The email address of a user making an HTTP request.

    ```
    (parse-header 'from "bob@example.com")
    ⇒ "bob@example.com"
    ```

HTTP Header: `Pair` **host** [¶](#index-host)
:   The host for the resource being requested, as a hostname-port pair. If
    no port is given, the port is `#f`.

    ```
    (parse-header 'host "gnu.org:80")
    ⇒ ("gnu.org" . 80)
    (parse-header 'host "gnu.org")
    ⇒ ("gnu.org" . #f)
    ```

HTTP Header: `*|List` **if-match** [¶](#index-if_002dmatch)
:   A set of etags, indicating that the request should proceed if and only
    if the etag of the resource is in that set. Either the symbol `*`,
    indicating any etag, or a list of entity tags.

    ```
    (parse-header 'if-match "*")
    ⇒ *
    (parse-header 'if-match "asdfadf")
    ⇒ (("asdfadf" . #t))
    (parse-header 'if-match W/"asdfadf")
    ⇒ (("asdfadf" . #f))
    ```

HTTP Header: `Date` **if-modified-since** [¶](#index-if_002dmodified_002dsince)
:   Indicates that a response should proceed if and only if the resource has
    been modified since the given date.

    ```
    (parse-header 'if-modified-since "Tue, 15 Nov 1994 08:12:31 GMT")
    ⇒ #<date ...>
    ```

HTTP Header: `*|List` **if-none-match** [¶](#index-if_002dnone_002dmatch)
:   A set of etags, indicating that the request should proceed if and only
    if the etag of the resource is not in the set. Either the symbol
    `*`, indicating any etag, or a list of entity tags.

    ```
    (parse-header 'if-none-match "*")
    ⇒ *
    ```

HTTP Header: `ETag|Date` **if-range** [¶](#index-if_002drange)
:   Indicates that the range request should proceed if and only if the
    resource matches a modification date or an etag. Either an entity tag,
    or a SRFI-19 date.

    ```
    (parse-header 'if-range "\"original-etag\"")
    ⇒ ("original-etag" . #t)
    ```

HTTP Header: `Date` **if-unmodified-since** [¶](#index-if_002dunmodified_002dsince)
:   Indicates that a response should proceed if and only if the resource has
    not been modified since the given date.

    ```
    (parse-header 'if-not-modified-since "Tue, 15 Nov 1994 08:12:31 GMT")
    ⇒ #<date ...>
    ```

HTTP Header: `UInt` **max-forwards** [¶](#index-max_002dforwards)
:   The maximum number of proxy or gateway hops that a request should be
    subject to.

    ```
    (parse-header 'max-forwards "10")
    ⇒ 10
    ```

HTTP Header: `Pair` **proxy-authorization** [¶](#index-proxy_002dauthorization)
:   Authorization credentials for a proxy connection. See the documentation
    for `authorization` above for more information on the format.

    ```
    (parse-header 'proxy-authorization "Digest foo=bar,baz=qux"
    ⇒ (digest (foo . "bar") (baz . "qux"))
    ```

HTTP Header: `Pair` **range** [¶](#index-range)
:   A range request, indicating that the client wants only part of a
    resource. The car of the pair is the symbol `bytes`, and the cdr
    is a list of pairs. Each element of the cdr indicates a range; the car
    is the first byte position and the cdr is the last byte position, as
    integers, or `#f` if not given.

    ```
    (parse-header 'range "bytes=10-30,50-")
    ⇒ (bytes (10 . 30) (50 . #f))
    ```

HTTP Header: `URI` **referer** [¶](#index-referer)
:   The URI of the resource that referred the user to this resource. The
    name of the header is a misspelling, but we are stuck with it.

    ```
    (parse-header 'referer "http://www.gnu.org/")
    ⇒ #<uri ...>
    ```

HTTP Header: `List` **te** [¶](#index-te)
:   A list of transfer codings, expressed as key-value lists. A common
    transfer coding is `trailers`.

    ```
    (parse-header 'te "trailers")
    ⇒ ((trailers))
    ```

HTTP Header: `String` **user-agent** [¶](#index-user_002dagent)
:   A string indicating the user agent making the request. The
    specification defines a structured format for this header, but it is
    widely disregarded, so Guile does not attempt to parse strictly.

    ```
    (parse-header 'user-agent "Mozilla/5.0")
    ⇒ "Mozilla/5.0"
    ```

#### 7.3.4.5 Response Headers [¶](#Response-Headers)

HTTP Header: `List` **accept-ranges** [¶](#index-accept_002dranges)
:   A list of range units that the server supports, as symbols.

    ```
    (parse-header 'accept-ranges "bytes")
    ⇒ (bytes)
    ```

HTTP Header: `UInt` **age** [¶](#index-age)
:   The age of a cached response, in seconds.

    ```
    (parse-header 'age "3600")
    ⇒ 3600
    ```

HTTP Header: `ETag` **etag** [¶](#index-etag)
:   The entity-tag of the resource.

    ```
    (parse-header 'etag "\"foo\"")
    ⇒ ("foo" . #t)
    ```

HTTP Header: `URI-reference` **location** [¶](#index-location-1)
:   A URI reference on which a request may be completed. Used in
    combination with a redirecting status code to perform client-side
    redirection.

    ```
    (parse-header 'location "http://example.com/other")
    ⇒ #<uri ...>
    ```

HTTP Header: `List` **proxy-authenticate** [¶](#index-proxy_002dauthenticate)
:   A list of challenges to a proxy, indicating the need for authentication.

    ```
    (parse-header 'proxy-authenticate "Basic realm=\"foo\"")
    ⇒ ((basic (realm . "foo")))
    ```

HTTP Header: `UInt|Date` **retry-after** [¶](#index-retry_002dafter)
:   Used in combination with a server-busy status code, like 503, to
    indicate that a client should retry later. Either a number of seconds,
    or a date.

    ```
    (parse-header 'retry-after "60")
    ⇒ 60
    ```

HTTP Header: `String` **server** [¶](#index-server)
:   A string identifying the server.

    ```
    (parse-header 'server "My first web server")
    ⇒ "My first web server"
    ```

HTTP Header: `*|List` **vary** [¶](#index-vary)
:   A set of request headers that were used in computing this response.
    Used to indicate that server-side content negotiation was performed, for
    example in response to the `accept-language` header. Can also be
    the symbol `*`, indicating that all headers were considered.

    ```
    (parse-header 'vary "Accept-Language, Accept")
    ⇒ (accept-language accept)
    ```

HTTP Header: `List` **www-authenticate** [¶](#index-www_002dauthenticate)
:   A list of challenges to a user, indicating the need for authentication.

    ```
    (parse-header 'www-authenticate "Basic realm=\"foo\"")
    ⇒ ((basic (realm . "foo")))
    ```

---

Next: [HTTP Requests](#Requests), Previous: [HTTP Headers](#HTTP-Headers), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.5 Transfer Codings [¶](#Transfer-Codings-1)

HTTP 1.1 allows for various transfer codings to be applied to message
bodies. These include various types of compression, and HTTP chunked
encoding. Currently, only chunked encoding is supported by guile.

Chunked coding is an optional coding that may be applied to message
bodies, to allow messages whose length is not known beforehand to be
returned. Such messages can be split into chunks, terminated by a final
zero length chunk.

In order to make dealing with encodings more simple, guile provides
procedures to create ports that “wrap” existing ports, applying
transformations transparently under the hood.

These procedures are in the `(web http)` module.

```
(use-modules (web http))
```

Scheme Procedure: **make-chunked-input-port** port [#:keep-alive?=#f] [¶](#index-make_002dchunked_002dinput_002dport)
:   Returns a new port, that transparently reads and decodes chunk-encoded
    data from port. If no more chunk-encoded data is available, it
    returns the end-of-file object. When the port is closed, port will
    also be closed, unless keep-alive? is true.

    If the chunked input ends prematurely, a
    `&chunked-input-ended-promaturely` exception will be raised.

```
(use-modules (ice-9 rdelim))

(define s "5\r\nFirst\r\nA\r\n line\n Sec\r\n8\r\nond line\r\n0\r\n")
(define p (make-chunked-input-port (open-input-string s)))
(read-line s)
⇒ "First line"
(read-line s)
⇒ "Second line"
```

Scheme Procedure: **make-chunked-output-port** port [#:keep-alive?=#f] [¶](#index-make_002dchunked_002doutput_002dport)
:   Returns a new port, which transparently encodes data as chunk-encoded
    before writing it to port. Whenever a write occurs on this port,
    it buffers it, until the port is flushed, at which point it writes a
    chunk containing all the data written so far. When the port is closed,
    the data remaining is written to port, as is the terminating zero
    chunk. It also causes port to be closed, unless keep-alive?
    is true.

    Note. Forcing a chunked output port when there is no data is buffered
    does not write a zero chunk, as this would cause the data to be
    interpreted incorrectly by the client.

```
(call-with-output-string
  (lambda (out)
    (define out* (make-chunked-output-port out #:keep-alive? #t))
    (display "first chunk" out*)
    (force-output out*)
    (force-output out*) ; note this does not write a zero chunk
    (display "second chunk" out*)
    (close-port out*)))
⇒ "b\r\nfirst chunk\r\nc\r\nsecond chunk\r\n0\r\n"
```

---

Next: [HTTP Responses](#Responses), Previous: [Transfer Codings](#Transfer-Codings), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.6 HTTP Requests [¶](#HTTP-Requests)

```
(use-modules (web request))
```

The request module contains a data type for HTTP requests.

* [An Important Note on Character Sets](#An-Important-Note-on-Character-Sets)
* [Request API](#Request-API)

#### 7.3.6.1 An Important Note on Character Sets [¶](#An-Important-Note-on-Character-Sets)

HTTP requests consist of two parts: the request proper, consisting of a
request line and a set of headers, and (optionally) a body. The body
might have a binary content-type, and even in the textual case its
length is specified in bytes, not characters.

Therefore, HTTP is a fundamentally binary protocol. However the request
line and headers are specified to be in a subset of ASCII, so they can
be treated as text, provided that the port’s encoding is set to an
ASCII-compatible one-byte-per-character encoding. ISO-8859-1 (latin-1)
is just such an encoding, and happens to be very efficient for Guile.

So what Guile does when reading requests from the wire, or writing them
out, is to set the port’s encoding to latin-1, and treating the request
headers as text.

The request body is another issue. For binary data, the data is
probably in a bytevector, so we use the R6RS binary output procedures to
write out the binary payload. Textual data usually has to be written
out to some character encoding, usually UTF-8, and then the resulting
bytevector is written out to the port.

In summary, Guile reads and writes HTTP over latin-1 sockets, without
any loss of generality.

#### 7.3.6.2 Request API [¶](#Request-API)

Scheme Procedure: **request?** obj [¶](#index-request_003f)

Scheme Procedure: **request-method** request [¶](#index-request_002dmethod)

Scheme Procedure: **request-uri** request [¶](#index-request_002duri)

Scheme Procedure: **request-version** request [¶](#index-request_002dversion)

Scheme Procedure: **request-headers** request [¶](#index-request_002dheaders)

Scheme Procedure: **request-meta** request [¶](#index-request_002dmeta)

Scheme Procedure: **request-port** request [¶](#index-request_002dport)
:   A predicate and field accessors for the request type. The fields are as
    follows:

    `method`
    :   The HTTP method, for example, `GET`.

    `uri`
    :   The URI as a URI record.

    `version`
    :   The HTTP version pair, like `(1 . 1)`.

    `headers`
    :   The request headers, as an alist of parsed values.

    `meta`
    :   An arbitrary alist of other data, for example information returned in
        the `sockaddr` from `accept` (see [Network Sockets and Communication](#Network-Sockets-and-Communication)).

    `port`
    :   The port on which to read or write a request body, if any.

Scheme Procedure: **read-request** port [meta=’()] [¶](#index-read_002drequest)
:   Read an HTTP request from port, optionally attaching the given
    metadata, meta.

    As a side effect, sets the encoding on port to ISO-8859-1
    (latin-1), so that reading one character reads one byte. See the
    discussion of character sets above, for more information.

    Note that the body is not part of the request. Once you have read a
    request, you may read the body separately, and likewise for writing
    requests.

Scheme Procedure: **build-request** uri [#:method=’GET] [#:version=’(1 . 1)] [#:headers=’()] [#:port=#f] [#:meta=’()] [#:validate-headers?=#t] [¶](#index-build_002drequest)
:   Construct an HTTP request object. If validate-headers? is true,
    the headers are each run through their respective validators.

Scheme Procedure: **write-request** r port [¶](#index-write_002drequest)
:   Write the given HTTP request to port.

    Return a new request, whose `request-port` will continue writing
    on port, perhaps using some transfer encoding.

Scheme Procedure: **read-request-body** r [¶](#index-read_002drequest_002dbody)
:   Reads the request body from r, as a bytevector. Return `#f`
    if there was no request body.

Scheme Procedure: **write-request-body** r bv [¶](#index-write_002drequest_002dbody)
:   Write bv, a bytevector, to the port corresponding to the HTTP
    request r.

The various headers that are typically associated with HTTP requests may
be accessed with these dedicated accessors. See [HTTP Headers](#HTTP-Headers), for
more information on the format of parsed headers.

Scheme Procedure: **request-accept** request [default=’()] [¶](#index-request_002daccept)

Scheme Procedure: **request-accept-charset** request [default=’()] [¶](#index-request_002daccept_002dcharset)

Scheme Procedure: **request-accept-encoding** request [default=’()] [¶](#index-request_002daccept_002dencoding)

Scheme Procedure: **request-accept-language** request [default=’()] [¶](#index-request_002daccept_002dlanguage)

Scheme Procedure: **request-allow** request [default=’()] [¶](#index-request_002dallow)

Scheme Procedure: **request-authorization** request [default=#f] [¶](#index-request_002dauthorization)

Scheme Procedure: **request-cache-control** request [default=’()] [¶](#index-request_002dcache_002dcontrol)

Scheme Procedure: **request-connection** request [default=’()] [¶](#index-request_002dconnection)

Scheme Procedure: **request-content-encoding** request [default=’()] [¶](#index-request_002dcontent_002dencoding)

Scheme Procedure: **request-content-language** request [default=’()] [¶](#index-request_002dcontent_002dlanguage)

Scheme Procedure: **request-content-length** request [default=#f] [¶](#index-request_002dcontent_002dlength)

Scheme Procedure: **request-content-location** request [default=#f] [¶](#index-request_002dcontent_002dlocation)

Scheme Procedure: **request-content-md5** request [default=#f] [¶](#index-request_002dcontent_002dmd5)

Scheme Procedure: **request-content-range** request [default=#f] [¶](#index-request_002dcontent_002drange)

Scheme Procedure: **request-content-type** request [default=#f] [¶](#index-request_002dcontent_002dtype)

Scheme Procedure: **request-date** request [default=#f] [¶](#index-request_002ddate)

Scheme Procedure: **request-expect** request [default=’()] [¶](#index-request_002dexpect)

Scheme Procedure: **request-expires** request [default=#f] [¶](#index-request_002dexpires)

Scheme Procedure: **request-from** request [default=#f] [¶](#index-request_002dfrom)

Scheme Procedure: **request-host** request [default=#f] [¶](#index-request_002dhost)

Scheme Procedure: **request-if-match** request [default=#f] [¶](#index-request_002dif_002dmatch)

Scheme Procedure: **request-if-modified-since** request [default=#f] [¶](#index-request_002dif_002dmodified_002dsince)

Scheme Procedure: **request-if-none-match** request [default=#f] [¶](#index-request_002dif_002dnone_002dmatch)

Scheme Procedure: **request-if-range** request [default=#f] [¶](#index-request_002dif_002drange)

Scheme Procedure: **request-if-unmodified-since** request [default=#f] [¶](#index-request_002dif_002dunmodified_002dsince)

Scheme Procedure: **request-last-modified** request [default=#f] [¶](#index-request_002dlast_002dmodified)

Scheme Procedure: **request-max-forwards** request [default=#f] [¶](#index-request_002dmax_002dforwards)

Scheme Procedure: **request-pragma** request [default=’()] [¶](#index-request_002dpragma)

Scheme Procedure: **request-proxy-authorization** request [default=#f] [¶](#index-request_002dproxy_002dauthorization)

Scheme Procedure: **request-range** request [default=#f] [¶](#index-request_002drange)

Scheme Procedure: **request-referer** request [default=#f] [¶](#index-request_002dreferer)

Scheme Procedure: **request-te** request [default=#f] [¶](#index-request_002dte)

Scheme Procedure: **request-trailer** request [default=’()] [¶](#index-request_002dtrailer)

Scheme Procedure: **request-transfer-encoding** request [default=’()] [¶](#index-request_002dtransfer_002dencoding)

Scheme Procedure: **request-upgrade** request [default=’()] [¶](#index-request_002dupgrade)

Scheme Procedure: **request-user-agent** request [default=#f] [¶](#index-request_002duser_002dagent)

Scheme Procedure: **request-via** request [default=’()] [¶](#index-request_002dvia)

Scheme Procedure: **request-warning** request [default=’()] [¶](#index-request_002dwarning)
:   Return the given request header, or default if none was present.

Scheme Procedure: **request-absolute-uri** r [default-host=#f] [default-port=#f] [default-scheme=#f] [¶](#index-request_002dabsolute_002duri)
:   A helper routine to determine the absolute URI of a request, using the
    `host` header and the default scheme, host and port. If there is
    no default scheme and the URI is not itself absolute, an error is
    signaled.

---

Next: [Web Client](#Web-Client), Previous: [HTTP Requests](#Requests), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.7 HTTP Responses [¶](#HTTP-Responses)

```
(use-modules (web response))
```

As with requests (see [HTTP Requests](#Requests)), Guile offers a data type for HTTP
responses. Again, the body is represented separately from the request.

Scheme Procedure: **response?** obj [¶](#index-response_003f)

Scheme Procedure: **response-version** response [¶](#index-response_002dversion)

Scheme Procedure: **response-code** response [¶](#index-response_002dcode)

Scheme Procedure: **response-reason-phrase** response [¶](#index-response_002dreason_002dphrase)

Scheme Procedure: **response-headers** response [¶](#index-response_002dheaders)

Scheme Procedure: **response-port** response [¶](#index-response_002dport)
:   A predicate and field accessors for the response type. The fields are as
    follows:

    `version`
    :   The HTTP version pair, like `(1 . 1)`.

    `code`
    :   The HTTP response code, like `200`.

    `reason-phrase`
    :   The reason phrase, or the standard reason phrase for the response’s
        code.

    `headers`
    :   The response headers, as an alist of parsed values.

    `port`
    :   The port on which to read or write a response body, if any.

Scheme Procedure: **read-response** port [¶](#index-read_002dresponse)
:   Read an HTTP response from port.

    As a side effect, sets the encoding on port to ISO-8859-1
    (latin-1), so that reading one character reads one byte. See the
    discussion of character sets in [HTTP Responses](#Responses), for more information.

Scheme Procedure: **build-response** [#:version=’(1 . 1)] [#:code=200] [#:reason-phrase=#f] [#:headers=’()] [#:port=#f] [#:validate-headers?=#t] [¶](#index-build_002dresponse)
:   Construct an HTTP response object. If validate-headers? is true,
    the headers are each run through their respective validators.

Scheme Procedure: **adapt-response-version** response version [¶](#index-adapt_002dresponse_002dversion)
:   Adapt the given response to a different HTTP version. Return a new HTTP
    response.

    The idea is that many applications might just build a response for the
    default HTTP version, and this method could handle a number of
    programmatic transformations to respond to older HTTP versions (0.9 and
    1.0). But currently this function is a bit heavy-handed, just updating
    the version field.

Scheme Procedure: **write-response** r port [¶](#index-write_002dresponse)
:   Write the given HTTP response to port.

    Return a new response, whose `response-port` will continue writing
    on port, perhaps using some transfer encoding.

Scheme Procedure: **response-must-not-include-body?** r [¶](#index-response_002dmust_002dnot_002dinclude_002dbody_003f)
:   Some responses, like those with status code 304, are specified as never
    having bodies. This predicate returns `#t` for those responses.

    Note also, though, that responses to `HEAD` requests must also not
    have a body.

Scheme Procedure: **response-body-port** r [#:decode?=#t] [#:keep-alive?=#t] [¶](#index-response_002dbody_002dport)
:   Return an input port from which the body of r can be read. The encoding
    of the returned port is set according to r’s `content-type` header,
    when it’s textual, except if decode? is `#f`. Return `#f`
    when no body is available.

    When keep-alive? is `#f`, closing the returned port also closes
    r’s response port.

Scheme Procedure: **read-response-body** r [¶](#index-read_002dresponse_002dbody)
:   Read the response body from r, as a bytevector. Returns `#f`
    if there was no response body.

Scheme Procedure: **write-response-body** r bv [¶](#index-write_002dresponse_002dbody)
:   Write bv, a bytevector, to the port corresponding to the HTTP
    response r.

As with requests, the various headers that are typically associated with
HTTP responses may be accessed with these dedicated accessors.
See [HTTP Headers](#HTTP-Headers), for more information on the format of parsed
headers.

Scheme Procedure: **response-accept-ranges** response [default=#f] [¶](#index-response_002daccept_002dranges)

Scheme Procedure: **response-age** response [default=’()] [¶](#index-response_002dage)

Scheme Procedure: **response-allow** response [default=’()] [¶](#index-response_002dallow)

Scheme Procedure: **response-cache-control** response [default=’()] [¶](#index-response_002dcache_002dcontrol)

Scheme Procedure: **response-connection** response [default=’()] [¶](#index-response_002dconnection)

Scheme Procedure: **response-content-encoding** response [default=’()] [¶](#index-response_002dcontent_002dencoding)

Scheme Procedure: **response-content-language** response [default=’()] [¶](#index-response_002dcontent_002dlanguage)

Scheme Procedure: **response-content-length** response [default=#f] [¶](#index-response_002dcontent_002dlength)

Scheme Procedure: **response-content-location** response [default=#f] [¶](#index-response_002dcontent_002dlocation)

Scheme Procedure: **response-content-md5** response [default=#f] [¶](#index-response_002dcontent_002dmd5)

Scheme Procedure: **response-content-range** response [default=#f] [¶](#index-response_002dcontent_002drange)

Scheme Procedure: **response-content-type** response [default=#f] [¶](#index-response_002dcontent_002dtype)

Scheme Procedure: **response-date** response [default=#f] [¶](#index-response_002ddate)

Scheme Procedure: **response-etag** response [default=#f] [¶](#index-response_002detag)

Scheme Procedure: **response-expires** response [default=#f] [¶](#index-response_002dexpires)

Scheme Procedure: **response-last-modified** response [default=#f] [¶](#index-response_002dlast_002dmodified)

Scheme Procedure: **response-location** response [default=#f] [¶](#index-response_002dlocation)

Scheme Procedure: **response-pragma** response [default=’()] [¶](#index-response_002dpragma)

Scheme Procedure: **response-proxy-authenticate** response [default=#f] [¶](#index-response_002dproxy_002dauthenticate)

Scheme Procedure: **response-retry-after** response [default=#f] [¶](#index-response_002dretry_002dafter)

Scheme Procedure: **response-server** response [default=#f] [¶](#index-response_002dserver)

Scheme Procedure: **response-trailer** response [default=’()] [¶](#index-response_002dtrailer)

Scheme Procedure: **response-transfer-encoding** response [default=’()] [¶](#index-response_002dtransfer_002dencoding)

Scheme Procedure: **response-upgrade** response [default=’()] [¶](#index-response_002dupgrade)

Scheme Procedure: **response-vary** response [default=’()] [¶](#index-response_002dvary)

Scheme Procedure: **response-via** response [default=’()] [¶](#index-response_002dvia)

Scheme Procedure: **response-warning** response [default=’()] [¶](#index-response_002dwarning)

Scheme Procedure: **response-www-authenticate** response [default=#f] [¶](#index-response_002dwww_002dauthenticate)
:   Return the given response header, or default if none was present.

Scheme Procedure: **text-content-type?** type [¶](#index-text_002dcontent_002dtype_003f)
:   Return `#t` if type, a symbol as returned by
    `response-content-type`, represents a textual type such as
    `text/plain`.

---

Next: [Web Server](#Web-Server), Previous: [HTTP Responses](#Responses), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.8 Web Client [¶](#Web-Client-1)

`(web client)` provides a simple, synchronous HTTP client, built on
the lower-level HTTP, request, and response modules.

```
(use-modules (web client))
```

Scheme Procedure: **open-socket-for-uri** uri [#:verify-certificate? #t] [¶](#index-open_002dsocket_002dfor_002duri)
:   Return an open input/output port for a connection to URI. Guile
    dynamically loads Guile-GnuTLS for HTTPS support.

    See the [Web site of Guile-GnuTLS](https://gitlab.com/gnutls/guile/), and
    see [how to install the GnuTLS bindings for Guile](gnutls-guile.html#Guile-Preparations) in GnuTLS-Guile, for more information.

    When verify-certificate? is true, verify the server’s X.509
    certificates against those read from `x509-certificate-directory`.
    When an error occurs—e.g., the server’s certificate has expired, or
    its host name does not match—raise a `tls-certificate-error`
    exception. The arguments to the `tls-certificate-error` exception
    are:

    1. a symbol indicating the failure cause, `host-mismatch` if the
       certificate’s host name does not match the server’s host name, and
       `invalid-certificate` for other causes;
    2. the server’s X.509 certificate (see [GnuTLS Guile
       reference](gnutls-guile.html#Guile-Reference) in GnuTLS-Guile);
    3. the server’s host name (a string);
    4. in the case of `invalid-certificate` errors, a list of GnuTLS
       certificate status values—one of the `certificate-status/`
       constants, such as `certificate-status/signer-not-found` or
       `certificate-status/revoked`.

Scheme Procedure: **http-request** uri arg… [¶](#index-http_002drequest)
:   Connect to the server corresponding to uri and make a request over
    HTTP, using method (`GET`, `HEAD`, `POST`, etc.).

    The following keyword arguments allow you to modify the requests in
    various ways, for example attaching a body to the request, or setting
    specific headers. The following table lists the keyword arguments and
    their default values.

    `#:method 'GET`

    `#:body #f`

    `#:verify-certificate? #t`

    `#:port (open-socket-for-uri uri #:verify-certificate? verify-certificate?)`

    `#:version '(1 . 1)`

    `#:keep-alive? #f`

    `#:headers '()`

    `#:decode-body? #t`

    `#:streaming? #f`

    If you already have a port open, pass it as port. Otherwise, a
    connection will be opened to the server corresponding to uri. Any
    extra headers in the alist headers will be added to the request.

    If body is not `#f`, a message body will also be sent with
    the HTTP request. If body is a string, it is encoded according to
    the content-type in headers, defaulting to UTF-8. Otherwise
    body should be a bytevector, or `#f` for no body. Although a
    message body may be sent with any request, usually only `POST` and
    `PUT` requests have bodies.

    If decode-body? is true, as is the default, the body of the
    response will be decoded to string, if it is a textual content-type.
    Otherwise it will be returned as a bytevector.

    However, if streaming? is true, instead of eagerly reading the
    response body from the server, this function only reads off the headers.
    The response body will be returned as a port on which the data may be
    read.

    Unless keep-alive? is true, the port will be closed after the full
    response body has been read.

    If port is false, uri denotes an HTTPS URL, and verify-certificate? is
    true, verify X.509 certificates against those available in
    `x509-certificate-directory`.

    Returns two values: the response read from the server, and the response
    body as a string, bytevector, #f value, or as a port (if
    streaming? is true).

Scheme Procedure: **http-get** uri arg… [¶](#index-http_002dget)

Scheme Procedure: **http-head** uri arg… [¶](#index-http_002dhead)

Scheme Procedure: **http-post** uri arg… [¶](#index-http_002dpost)

Scheme Procedure: **http-put** uri arg… [¶](#index-http_002dput)

Scheme Procedure: **http-delete** uri arg… [¶](#index-http_002ddelete)

Scheme Procedure: **http-trace** uri arg… [¶](#index-http_002dtrace)

Scheme Procedure: **http-options** uri arg… [¶](#index-http_002doptions)
:   Connect to the server corresponding to uri and make a request over
    HTTP, using the appropriate method (`GET`, `HEAD`,
    `POST`, etc.).

    These procedures are variants of `http-request` specialized with a
    specific method argument, and have the same prototype: a URI
    followed by an optional sequence of keyword arguments.
    See [http-request](#http_002drequest), for full documentation on the various keyword
    arguments.

Scheme Parameter: **x509-certificate-directory** [¶](#index-x509_002dcertificate_002ddirectory)
:   This parameter gives the name of the directory where X.509 certificates
    for HTTPS connections should be looked for.

    Its default value is one of:

    * the value of the `GUILE_TLS_CERTIFICATE_DIRECTORY` environment
      variable;
    * or the value of the `SSL_CERT_DIR` environment variable (also
      honored by the OpenSSL library);
    * or, as a last resort, `"/etc/ssl/certs"`.

    X.509 certificates are used when authenticating the identity of a remote
    site, when the `#:verify-certificate?` argument to
    `open-socket-for-uri`, to `http-request`, or to related
    procedures is true.

`http-get` is useful for making one-off requests to web sites. If
you are writing a web spider or some other client that needs to handle a
number of requests in parallel, it’s better to build an event-driven URL
fetcher, similar in structure to the web server (see [Web Server](#Web-Server)).

Another option, good but not as performant, would be to use threads,
possibly via par-map or futures.

Scheme Parameter: **current-http-proxy** [¶](#index-current_002dhttp_002dproxy)

Scheme Parameter: **current-https-proxy** [¶](#index-current_002dhttps_002dproxy)
:   Either `#f` or a non-empty string containing the URL of the HTTP
    or HTTPS proxy server to be used by the procedures in the `(web client)`
    module, including `open-socket-for-uri`. Its initial value is
    based on the `http_proxy` and `https_proxy` environment variables.

    ```
    (current-http-proxy) ⇒ "http://localhost:8123/"
    (parameterize ((current-http-proxy #f))
      (http-get "http://example.com/"))  ; temporarily bypass proxy
    (current-http-proxy) ⇒ "http://localhost:8123/"
    ```

---

Next: [Web Examples](#Web-Examples), Previous: [Web Client](#Web-Client), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.9 Web Server [¶](#Web-Server-1)

`(web server)` is a generic web server interface, along with a main
loop implementation for web servers controlled by Guile.

```
(use-modules (web server))
```

The lowest layer is the `<server-impl>` object, which defines a set
of hooks to open a server, read a request from a client, write a
response to a client, and close a server. These hooks – `open`,
`read`, `write`, and `close`, respectively – are bound
together in a `<server-impl>` object. Procedures in this module take a
`<server-impl>` object, if needed.

A `<server-impl>` may also be looked up by name. If you pass the
`http` symbol to `run-server`, Guile looks for a variable
named `http` in the `(web server http)` module, which should
be bound to a `<server-impl>` object. Such a binding is made by
instantiation of the `define-server-impl` syntax. In this way the
run-server loop can automatically load other backends if available.

The life cycle of a server goes as follows:

1. The `open` hook is called, to open the server. `open` takes
   zero or more arguments, depending on the backend, and returns an opaque
   server socket object, or signals an error.
2. The `read` hook is called, to read a request from a new client.
   The `read` hook takes one argument, the server socket. It should
   return three values: an opaque client socket, the request, and the
   request body. The request should be a `<request>` object, from
   `(web request)`. The body should be a string or a bytevector, or
   `#f` if there is no body.

   If the read failed, the `read` hook may return #f for the client
   socket, request, and body.
3. A user-provided handler procedure is called, with the request and body
   as its arguments. The handler should return two values: the response,
   as a `<response>` record from `(web response)`, and the
   response body as bytevector, or `#f` if not present.

   The respose and response body are run through `sanitize-response`,
   documented below. This allows the handler writer to take some
   convenient shortcuts: for example, instead of a `<response>`, the
   handler can simply return an alist of headers, in which case a default
   response object is constructed with those headers. Instead of a
   bytevector for the body, the handler can return a string, which will be
   serialized into an appropriate encoding; or it can return a procedure,
   which will be called on a port to write out the data. See the
   `sanitize-response` documentation, for more.
4. The `write` hook is called with three arguments: the client
   socket, the response, and the body. The `write` hook returns no
   values.
5. At this point the request handling is complete. For a loop, we
   loop back and try to read a new request.
6. If the user interrupts the loop, the `close` hook is called on
   the server socket.

A user may define a server implementation with the following form:

Scheme Syntax: **define-server-impl** name open read write close [¶](#index-define_002dserver_002dimpl)
:   Make a `<server-impl>` object with the hooks open,
    read, write, and close, and bind it to the symbol
    name in the current module.

Scheme Procedure: **lookup-server-impl** impl [¶](#index-lookup_002dserver_002dimpl)
:   Look up a server implementation. If impl is a server
    implementation already, it is returned directly. If it is a symbol, the
    binding named impl in the `(web server impl)` module is
    looked up. Otherwise an error is signaled.

    Currently a server implementation is a somewhat opaque type, useful only
    for passing to other procedures in this module, like `read-client`.

The `(web server)` module defines a number of routines that use
`<server-impl>` objects to implement parts of a web server. Given
that we don’t expose the accessors for the various fields of a
`<server-impl>`, indeed these routines are the only procedures with
any access to the impl objects.

Scheme Procedure: **open-server** impl open-params [¶](#index-open_002dserver)
:   Open a server for the given implementation. Return one value, the new
    server object. The implementation’s `open` procedure is applied to
    open-params, which should be a list.

Scheme Procedure: **read-client** impl server [¶](#index-read_002dclient)
:   Read a new client from server, by applying the implementation’s
    `read` procedure to the server. If successful, return three
    values: an object corresponding to the client, a request object, and the
    request body. If any exception occurs, return `#f` for all three
    values.

Scheme Procedure: **handle-request** handler request body state [¶](#index-handle_002drequest)
:   Handle a given request, returning the response and body.

    The response and response body are produced by calling the given
    handler with request and body as arguments.

    The elements of state are also passed to handler as
    arguments, and may be returned as additional values. The new
    state, collected from the handler’s return values, is then
    returned as a list. The idea is that a server loop receives a handler
    from the user, along with whatever state values the user is interested
    in, allowing the user’s handler to explicitly manage its state.

Scheme Procedure: **sanitize-response** request response body [¶](#index-sanitize_002dresponse)
:   “Sanitize” the given response and body, making them appropriate for
    the given request.

    As a convenience to web handler authors, response may be given as
    an alist of headers, in which case it is used to construct a default
    response. Ensures that the response version corresponds to the request
    version. If body is a string, encodes the string to a bytevector,
    in an encoding appropriate for response. Adds a
    `content-length` and `content-type` header, as necessary.

    If body is a procedure, it is called with a port as an argument,
    and the output collected as a bytevector. In the future we might try to
    instead use a compressing, chunk-encoded port, and call this procedure
    later, in the write-client procedure. Authors are advised not to rely on
    the procedure being called at any particular time.

Scheme Procedure: **write-client** impl server client response body [¶](#index-write_002dclient)
:   Write an HTTP response and body to client. If the server and
    client support persistent connections, it is the implementation’s
    responsibility to keep track of the client thereafter, presumably by
    attaching it to the server argument somehow.

Scheme Procedure: **close-server** impl server [¶](#index-close_002dserver)
:   Release resources allocated by a previous invocation of
    `open-server`.

Given the procedures above, it is a small matter to make a web server:

Scheme Procedure: **serve-one-client** handler impl server state [¶](#index-serve_002done_002dclient)
:   Read one request from server, call handler on the request
    and body, and write the response to the client. Return the new state
    produced by the handler procedure.

Scheme Procedure: **run-server** handler [impl=’http] [open-params=’()] arg … [¶](#index-run_002dserver-1)
:   Run Guile’s built-in web server.

    handler should be a procedure that takes two or more arguments,
    the HTTP request and request body, and returns two or more values, the
    response and response body.

    For examples, skip ahead to the next section, [Web Examples](#Web-Examples).

    The response and body will be run through `sanitize-response`
    before sending back to the client.

    Additional arguments to handler are taken from arg
    .... These arguments comprise a *state*. Additional return
    values are accumulated into a new state, which will be used for
    subsequent requests. In this way a handler can explicitly manage its
    state.

The default web server implementation is `http`, which binds to a
socket, listening for request on that port.

HTTP Implementation: **http** [#:host=#f] [#:family=AF\_INET] [#:addr=INADDR\_LOOPBACK] [#:port 8080] [#:socket] [¶](#index-http)
:   The default HTTP implementation. We document it as a function with
    keyword arguments, because that is precisely the way that it is – all
    of the open-params to `run-server` get passed to the
    implementation’s open function.

    ```
    ;; The defaults: localhost:8080
    (run-server handler)
    ;; Same thing
    (run-server handler 'http '())
    ;; On a different port
    (run-server handler 'http '(#:port 8081))
    ;; IPv6
    (run-server handler 'http '(#:family AF_INET6 #:port 8081))
    ;; Custom socket
    (run-server handler 'http `(#:socket ,(sudo-make-me-a-socket)))
    ```

---

Previous: [Web Server](#Web-Server), Up: [HTTP, the Web, and All That](#Web)   [[Contents](#SEC_Contents "Table of contents")][[Index](#R5RS-Index "Index")]

#### 7.3.10 Web Examples [¶](#Web-Examples-1)

Well, enough about the tedious internals. Let’s make a web application!

* [Hello, World!](#Hello_002c-World_0021)
* [Inspecting the Request](#Inspecting-the-Request)
* [Higher-Level Interfaces](#Higher_002dLevel-Interfaces)
* [Conclusion](#Conclusion)

#### 7.3.10.1 Hello, World! [¶](#Hello_002c-World_0021)

The first program we have to write, of course, is “Hello, World!”.
This means that we have to implement a web handler that does what we
want.

Now we define a handler, a function of two arguments and two return
values:

```
(define (handler request request-body)
  (values response response-body))
```

In this first example, we take advantage of a short-cut, returning an
alist of headers instead of a proper response object. The response body
is our payload:

```
(define (hello-world-handler request request-body)
  (values '((content-type . (text/plain)))
          "Hello World!"))
```

Now let’s test it, by running a server with this handler. Load up the
web server module if you haven’t yet done so, and run a server with this
handler:

```
(use-modules (web server))
(run-server hello-world-handler)
```

By default, the web server listens for requests on
`localhost:8080`. Visit that address in your web browser to
test. If you see the string, `Hello World!`, sweet!

#### 7.3.10.2 Inspecting the Request [¶](#Inspecting-the-Request)

The Hello World program above is a general greeter, responding to all
URIs. To make a more exclusive greeter, we need to inspect the request
object, and conditionally produce different results. So let’s load up
the request, response, and URI modules, and do just that.

```
(use-modules (web server)) ; you probably did this already
(use-modules (web request)
             (web response)
             (web uri))

(define (request-path-components request)
  (split-and-decode-uri-path (uri-path (request-uri request))))

(define (hello-hacker-handler request body)
  (if (equal? (request-path-components request)
              '("hacker"))
      (values '((content-type . (text/plain)))
              "Hello hacker!")
      (not-found request)))

(run-server hello-hacker-handler)
```

Here we see that we have defined a helper to return the components of
the URI path as a list of strings, and used that to check for a request
to `/hacker/`. Then the success case is just as before – visit
`http://localhost:8080/hacker/` in your browser to check.

You should always match against URI path components as decoded by
`split-and-decode-uri-path`. The above example will work for
`/hacker/`, `//hacker///`, and `/h%61ck%65r`.

But we forgot to define `not-found`! If you are pasting these
examples into a REPL, accessing any other URI in your web browser will
drop your Guile console into the debugger:

```
<unnamed port>:38:7: In procedure module-lookup:
<unnamed port>:38:7: Unbound variable: not-found

Entering a new prompt.  Type `,bt' for a backtrace or `,q' to continue.
scheme@(guile-user) [1]>
```

So let’s define the function, right there in the debugger. As you
probably know, we’ll want to return a 404 response.

```
;; Paste this in your REPL
(define (not-found request)
  (values (build-response #:code 404)
          (string-append "Resource not found: "
                         (uri->string (request-uri request)))))

;; Now paste this to let the web server keep going:
,continue
```

Now if you access `http://localhost/foo/`, you get this error
message. (Note that some popular web browsers won’t show
server-generated 404 messages, showing their own instead, unless the 404
message body is long enough.)

#### 7.3.10.3 Higher-Level Interfaces [¶](#Higher_002dLevel-Interfaces)

The web handler interface is a common baseline that all kinds of Guile
web applications can use. You will usually want to build something on
top of it, however, especially when producing HTML. Here is a simple
example that builds up HTML output using SXML (see [SXML](#SXML)).

First, load up the modules:

```
(use-modules (web server)
             (web request)
             (web response)
             (sxml simple))
```

Now we define a simple templating function that takes a list of HTML
body elements, as SXML, and puts them in our super template:

```
(define (templatize title body)
  `(html (head (title ,title))
         (body ,@body)))
```

For example, the simplest Hello HTML can be produced like this:

```
(sxml->xml (templatize "Hello!" '((b "Hi!"))))
⊣
<html><head><title>Hello!</title></head><body><b>Hi!</b></body></html>
```

Much better to work with Scheme data types than to work with HTML as
strings. Now we define a little response helper:

```
(define* (respond #:optional body #:key
                  (status 200)
                  (title "Hello hello!")
                  (doctype "<!DOCTYPE html>\n")
                  (content-type-params '((charset . "utf-8")))
                  (content-type 'text/html)
                  (extra-headers '())
                  (sxml (and body (templatize title body))))
  (values (build-response
           #:code status
           #:headers `((content-type
                        . (,content-type ,@content-type-params))
                       ,@extra-headers))
          (lambda (port)
            (if sxml
                (begin
                  (if doctype (display doctype port))
                  (sxml->xml sxml port))))))
```
