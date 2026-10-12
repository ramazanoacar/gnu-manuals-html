# and set-group-ID bits of the directory D.
chmod a-s D
chmod -6000 D
chmod =755 D
chmod 00755 D
```

This behavior is a GNU extension. Portable scripts should
not rely on requests to set or clear these bits on directories, as
POSIX allows implementations to ignore these requests.
The GNU behavior with numeric modes of four or fewer digits
is intended for scripts portable to systems that preserve these bits;
the behavior with numeric modes of five or more digits is for scripts
portable to systems that do not preserve the bits.

---

Next: [Date input formats](#Date-input-formats), Previous: [File permissions](#File-permissions), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 28 File timestamps [¶](#File-timestamps-1)

Standard POSIX files have three timestamps: the access timestamp
(atime) of the last read, the modification timestamp (mtime) of the
last write, and the status change timestamp (ctime) of the last change
to the file’s meta-information. Some file systems support a
fourth time: the birth timestamp (birthtime) of when the file was
created; by definition, birthtime never changes.

One common example of a ctime change is when the permissions of a file
change. Changing the permissions doesn’t access the file, so atime
doesn’t change, nor does it modify the file, so the mtime doesn’t
change. Yet, something about the file itself has changed, and this
must be noted somewhere. This is the job of the ctime field. This is
necessary, so that, for example, a backup program can make a fresh
copy of the file, including the new permissions value. Another
operation that modifies a file’s ctime without affecting the others is
renaming.

Naively, a file’s atime, mtime, and ctime are set to the current time
whenever you read, write, or change the attributes of the file
respectively, and searching a directory counts as reading it. A
file’s atime and mtime can also be set directly, via the
`touch` command (see [`touch`: Change file timestamps](#touch-invocation)). In practice,
though, timestamps are not updated quite that way.

For efficiency reasons, many systems are lazy about updating atimes:
when a program accesses a file, they may delay updating the file’s
atime, or may not update the file’s atime if the file has been
accessed recently, or may not update the atime at all. Similar
laziness, though typically not quite so extreme, applies to mtimes and
ctimes.

Some systems emulate timestamps instead of supporting them directly,
and these emulations may disagree with the naive interpretation. For
example, a system may fake an atime or ctime by using the mtime.

The determination of what time is “current” depends on the
platform. Platforms with network file systems often use different
clocks for the operating system and for file systems; because
updates typically uses file systems’ clocks by default, clock
skew can cause the resulting file timestamps to appear to be in a
program’s “future” or “past”.

When the system updates a file timestamp to a desired time t
(which is either the current time, or a time specified via the
`touch` command), there are several reasons the file’s
timestamp may be set to a value that differs from t. First,
t may have a higher resolution than supported. Second, a file
system may use different resolutions for different types of times.
Third, file timestamps may use a different resolution than operating
system timestamps. Fourth, the operating system primitives used to
update timestamps may employ yet a different resolution. For example,
in theory a file system might use 10-microsecond resolution for access
timestamp and 100-nanosecond resolution for modification timestamp, and the
operating system might use nanosecond resolution for the current time
and microsecond resolution for the primitive that `touch` uses
to set a file’s timestamp to an arbitrary value.

---

Next: [Version sort ordering](#Version-sort-ordering), Previous: [File timestamps](#File-timestamps), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 29 Date input formats [¶](#Date-input-formats-1)

First, a quote:

> Our units of temporal measurement, from seconds on up to months, are so
> complicated, asymmetrical and disjunctive so as to make coherent mental
> reckoning in time all but impossible. Indeed, had some tyrannical god
> contrived to enslave our minds to time, to make it all but impossible
> for us to escape subjection to sodden routines and unpleasant surprises,
> he could hardly have done better than handing down our present system.
> It is like a set of trapezoidal building blocks, with no vertical or
> horizontal surfaces, like a language in which the simplest thought
> demands ornate constructions, useless particles and lengthy
> circumlocutions. Unlike the more successful patterns of language and
> science, which enable us to face experience boldly or at least
> level-headedly, our system of temporal calculation silently and
> persistently encourages our terror of time.
>
> … It is as though architects had to measure length in feet, width
> in meters and height in ells; as though basic instruction manuals
> demanded a knowledge of five different languages. It is no wonder then
> that we often look into our own immediate past or future, last Tuesday
> or a week from Sunday, with feelings of helpless confusion. …
>
> —Robert Grudin, Time and the Art of Living.

This section describes the textual date representations that GNU
programs accept. These are the strings you, as a user, can supply as
arguments to the various programs. The C interface (via the
`parse_datetime` function) is not described here.

* [General date syntax](#General-date-syntax)
* [Calendar date items](#Calendar-date-items)
* [Time of day items](#Time-of-day-items)
* [Time zone items](#Time-zone-items)
* [Combined date and time of day items](#Combined-date-and-time-of-day-items)
* [Day of week items](#Day-of-week-items)
* [Relative items in date strings](#Relative-items-in-date-strings)
* [Pure numbers in date strings](#Pure-numbers-in-date-strings)
* [Seconds since the Epoch](#Seconds-since-the-Epoch)
* [Specifying time zone rules](#Specifying-time-zone-rules)
* [Authors of `parse_datetime`](#Authors-of-parse_005fdatetime)

---

Next: [Calendar date items](#Calendar-date-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.1 General date syntax [¶](#General-date-syntax-1)

A *date* is a string, possibly empty, containing many items
separated by whitespace. The whitespace may be omitted when no
ambiguity arises. The empty string means the beginning of today (i.e.,
midnight). Order of the items is immaterial. A date string may contain
many flavors of items:

* calendar date items
* time of day items
* time zone items
* combined date and time of day items
* day of the week items
* relative items
* pure numbers.

We describe each of these item types in turn, below.

A few ordinal numbers may be written out in words in some contexts. This is
most useful for specifying day of the week items or relative items (see
below). Among the most commonly used ordinal numbers, the word
‘`last`’ stands for *-1*, ‘`this`’ stands for 0, and
‘`first`’ and ‘`next`’ both stand for 1. Because the word
‘`second`’ stands for the unit of time there is no way to write the
ordinal number 2, but for convenience ‘`third`’ stands for 3,
‘`fourth`’ for 4, ‘`fifth`’ for 5,
‘`sixth`’ for 6, ‘`seventh`’ for 7, ‘`eighth`’ for 8,
‘`ninth`’ for 9, ‘`tenth`’ for 10, ‘`eleventh`’ for 11 and
‘`twelfth`’ for 12.

When a month is written this way, it is still considered to be written
numerically, instead of being “spelled in full”; this changes the
allowed strings.

In the current implementation, only English is supported for words and
abbreviations like ‘`AM`’, ‘`DST`’, ‘`EST`’, ‘`first`’,
‘`January`’, ‘`Sunday`’, ‘`tomorrow`’, and ‘`year`’.

The output of the `date` command
is not always acceptable as a date string,
not only because of the language problem, but also because there is no
standard meaning for time zone items like ‘`IST`’. When using
`date` to generate a date string intended to be parsed later,
specify a date format that is independent of language and that does not
use time zone items other than ‘`UTC`’ and ‘`Z`’. Here are some
ways to do this:

```
$ LC_ALL=C TZ=UTC0 date
Tue Nov 15 02:02:42 UTC 2022
$ TZ=UTC0 date +'%Y-%m-%d %H:%M:%SZ'
2022-11-15 02:02:42Z
$ date --rfc-3339=ns  # --rfc-3339 is a GNU extension.
2022-11-14 21:02:42.000000000-05:00
$ date --rfc-email  # a GNU extension
Mon, 14 Nov 2022 21:02:42 -0500
$ date +'%Y-%m-%d %H:%M:%S %z'  # %z is a GNU extension.
2022-11-14 21:02:42 -0500
$ date +'@%s.%N'  # %s and %N are GNU extensions.
@1668477762.692722128
```

Alphabetic case is completely ignored in dates. Comments may be introduced
between round parentheses, as long as included parentheses are properly
nested. Hyphens not followed by a digit are currently ignored. Leading
zeros on numbers are ignored.

Invalid dates like ‘`2022-02-29`’ or times like ‘`24:00`’ are
rejected. In the typical case of a host that does not support leap
seconds, a time like ‘`23:59:60`’ is rejected even if it
corresponds to a valid leap second.

---

Next: [Time of day items](#Time-of-day-items), Previous: [General date syntax](#General-date-syntax), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.2 Calendar date items [¶](#Calendar-date-items-1)

A *calendar date item* specifies a day of the year. It is
specified differently, depending on whether the month is specified
numerically or literally. All these strings specify the same calendar date:

```
2022-11-14     # ISO 8601.
22-11-14       # Assume 19xx for 69 through 99,
               # 20xx for 00 through 68 (not recommended).
11/14/2022     # Common U.S. writing.
14 November 2022
14 Nov 2022    # Three-letter abbreviations always allowed.
November 14, 2022
14-nov-2022
14nov2022
```

The year can also be omitted. In this case, the last specified year is
used, or the current year if none. For example:

```
11/14
nov 14
```

Here are the rules.

For numeric months, the ISO 8601 format
‘`year-month-day`’ is allowed, where year is
any positive number, month is a number between 01 and 12, and
day is a number between 01 and 31. A leading zero must be present
if a number is less than ten. If year is 68 or smaller, then 2000
is added to it; otherwise, if year is less than 100,
then 1900 is added to it. The construct
‘`month/day/year`’, popular in the United States,
is accepted. Also ‘`month/day`’, omitting the year.

Literal months may be spelled out in full: ‘`January`’,
‘`February`’, ‘`March`’, ‘`April`’, ‘`May`’, ‘`June`’,
‘`July`’, ‘`August`’, ‘`September`’, ‘`October`’,
‘`November`’ or ‘`December`’. Literal months may be abbreviated
to their first three letters, possibly followed by an abbreviating dot.
It is also permitted to write ‘`Sept`’ instead of ‘`September`’.

When months are written literally, the calendar date may be given as any
of the following:

```
day month year
day month
month day year
day-month-year
```

Or, omitting the year:

```
month day
```

---

Next: [Time zone items](#Time-zone-items), Previous: [Calendar date items](#Calendar-date-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.3 Time of day items [¶](#Time-of-day-items-1)

A *time of day item* in date strings specifies the time on a given
day. Here are some examples, all of which represent the same time:

```
20:02:00.000000
20:02
8:02pm
20:02-0500      # In EST (U.S. Eastern Standard Time).
```

More generally, the time of day may be given as
‘`hour:minute:second`’, where hour is
a number between 0 and 23, minute is a number between 0 and
59, and second is a number between 0 and 59 possibly followed by
‘`.`’ or ‘`,`’ and a fraction containing one or more digits.
Alternatively,
‘`:second`’ can be omitted, in which case it is taken to
be zero. On the rare hosts that support leap seconds, second
may be 60.

If the time is followed by ‘`am`’ or ‘`pm`’ (or ‘`a.m.`’
or ‘`p.m.`’), hour is restricted to run from 1 to 12, and
‘`:minute`’ may be omitted (taken to be zero). ‘`am`’
indicates the first half of the day, ‘`pm`’ indicates the second
half of the day. In this notation, 12 is the predecessor of 1:
midnight is ‘`12am`’ while noon is ‘`12pm`’.
(This is the zero-oriented interpretation of ‘`12am`’ and ‘`12pm`’,
as opposed to the old tradition derived from Latin
which uses ‘`12m`’ for noon and ‘`12pm`’ for midnight.)

The time may alternatively be followed by a time zone correction,
expressed as ‘`shhmm`’, where s is ‘`+`’
or ‘`-`’, hh is a number of zone hours and mm is a number
of zone minutes.
The zone minutes term, mm, may be omitted, in which case
the one- or two-digit correction is interpreted as a number of hours.
You can also separate hh from mm with a colon.
When a time zone correction is given this way, it
forces interpretation of the time relative to
Coordinated Universal Time (UTC), overriding any previous
specification for the time zone or the local time zone. For example,
‘`+0530`’ and ‘`+05:30`’ both stand for the time zone 5.5 hours
ahead of UTC (e.g., India).
This is the best way to
specify a time zone correction by fractional parts of an hour.
The maximum zone correction is 24 hours.

Either ‘`am`’/‘`pm`’ or a time zone correction may be specified,
but not both.

---

Next: [Combined date and time of day items](#Combined-date-and-time-of-day-items), Previous: [Time of day items](#Time-of-day-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.4 Time zone items [¶](#Time-zone-items-1)

A *time zone item* specifies an international time zone, indicated
by a small set of letters, e.g., ‘`UTC`’ or ‘`Z`’
for Coordinated Universal
Time. Any included periods are ignored. By following a
non-daylight-saving time zone by the string ‘`DST`’ in a separate
word (that is, separated by some white space), the corresponding
daylight saving time zone may be specified.
Alternatively, a non-daylight-saving time zone can be followed by a
time zone correction, to add the two values. This is normally done
only for ‘`UTC`’; for example, ‘`UTC+05:30`’ is equivalent to
‘`+05:30`’.

Time zone items other than ‘`UTC`’ and ‘`Z`’
are obsolescent and are not recommended, because they
are ambiguous; for example, ‘`EST`’ has a different meaning in
Australia than in the United States, and ‘`A`’ has different
meaning as a military time zone than as an obsolete
RFC 822 time zone. Instead, it’s better to use
unambiguous numeric time zone corrections like ‘`-0500`’, as
described in the previous section.

If neither a time zone item nor a time zone correction is supplied,
timestamps are interpreted using the rules of the default time zone
(see [Specifying time zone rules](#Specifying-time-zone-rules)).

---

Next: [Day of week items](#Day-of-week-items), Previous: [Time zone items](#Time-zone-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.5 Combined date and time of day items [¶](#Combined-date-and-time-of-day-items-1)

The ISO 8601 date and time of day extended format consists of an ISO
8601 date, a ‘`T`’ character separator, and an ISO 8601 time of
day. This format is also recognized if the ‘`T`’ is replaced by a
space.

In this format, the time of day should use 24-hour notation.
Fractional seconds are allowed, with either comma or period preceding
the fraction. ISO 8601 fractional minutes and hours are not
supported. Typically, hosts support nanosecond timestamp resolution;
excess precision is silently discarded.

Here are some examples:

```
2022-09-24T20:02:00.052-05:00
2022-12-31T23:59:59,999999999+11:00
1970-01-01 00:00Z
```

---

Next: [Relative items in date strings](#Relative-items-in-date-strings), Previous: [Combined date and time of day items](#Combined-date-and-time-of-day-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.6 Day of week items [¶](#Day-of-week-items-1)

The explicit mention of a day of the week will forward the date
(only if necessary) to reach that day of the week in the future.

Days of the week may be spelled out in full: ‘`Sunday`’,
‘`Monday`’, ‘`Tuesday`’, ‘`Wednesday`’, ‘`Thursday`’,
‘`Friday`’ or ‘`Saturday`’. Days may be abbreviated to their
first three letters, optionally followed by a period. The special
abbreviations ‘`Tues`’ for ‘`Tuesday`’, ‘`Wednes`’ for
‘`Wednesday`’ and ‘`Thur`’ or ‘`Thurs`’ for ‘`Thursday`’ are
also allowed.

A number may precede a day of the week item to move forward
supplementary weeks. It is best used in expression like ‘`third
monday`’. In this context, ‘`last day`’ or ‘`next
day`’ is also acceptable; they move one week before or after
the day that day by itself would represent.

A comma following a day of the week item is ignored.

---

Next: [Pure numbers in date strings](#Pure-numbers-in-date-strings), Previous: [Day of week items](#Day-of-week-items), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.7 Relative items in date strings [¶](#Relative-items-in-date-strings-1)

*Relative items* adjust a date (or the current date if none) forward
or backward. The effects of relative items accumulate. Here are some
examples:

```
1 year
1 year ago
3 years
2 days
```

The unit of time displacement may be selected by the string ‘`year`’
or ‘`month`’ for moving by whole years or months. These are fuzzy
units, as years and months are not all of equal duration. More precise
units are ‘`fortnight`’ which is worth 14 days, ‘`week`’ worth 7
days, ‘`day`’ worth 24 hours, ‘`hour`’ worth 60 minutes,
‘`minute`’ or ‘`min`’ worth 60 seconds, and ‘`second`’ or
‘`sec`’ worth one second. An ‘`s`’ suffix on these units is
accepted and ignored.

The unit of time may be preceded by a multiplier, given as an optionally
signed number. Unsigned numbers are taken as positively signed. No
number at all implies 1 for a multiplier. Following a relative item by
the string ‘`ago`’ is equivalent to preceding the unit by a
multiplier with value *-1*.

The string ‘`tomorrow`’ is worth one day in the future (equivalent
to ‘`day`’), the string ‘`yesterday`’ is worth
one day in the past (equivalent to ‘`day ago`’).

The strings ‘`now`’ or ‘`today`’ are relative items corresponding
to zero-valued time displacement, these strings come from the fact
a zero-valued time displacement represents the current time when not
otherwise changed by previous items. They may be used to stress other
items, like in ‘`12:00 today`’. The string ‘`this`’ also has
the meaning of a zero-valued time displacement, but is preferred in
date strings like ‘`this thursday`’.

When a relative item causes the resulting date to cross a boundary
where the clocks were adjusted, typically for daylight saving time,
the resulting date and time are adjusted accordingly.

The fuzz in units can cause problems with relative items. For
example, ‘`2022-12-31 -1 month`’ might evaluate to 2022-12-01,
because 2022-11-31 is an invalid date. To determine the previous
month more reliably, you can ask for the month before the 15th of the
current month. For example:

```
$ date -R
Thu, 31 Dec 2022 13:02:39 -0400
$ date --date='-1 month' +'Last month was %B?'
Last month was December?
$ date --date="$(date +%Y-%m-15) -1 month" +'Last month was %B!'
Last month was November!
```

Also, take care when manipulating dates around clock changes such as
daylight saving leaps. In a few cases these have added or subtracted
as much as 24 hours from the clock, so it is often wise to adopt
universal time by setting the `TZ` environment variable to
‘`UTC0`’ before embarking on calendrical calculations.

---

Next: [Seconds since the Epoch](#Seconds-since-the-Epoch), Previous: [Relative items in date strings](#Relative-items-in-date-strings), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.8 Pure numbers in date strings [¶](#Pure-numbers-in-date-strings-1)

The precise interpretation of a pure decimal number depends
on the context in the date string.

If the decimal number is of the form yyyymmdd and no
other calendar date item (see [Calendar date items](#Calendar-date-items)) appears before it
in the date string, then yyyy is read as the year, mm as the
month number and dd as the day of the month, for the specified
calendar date.

If the decimal number is of the form hhmm and no other time
of day item appears before it in the date string, then hh is read
as the hour of the day and mm as the minute of the hour, for the
specified time of day. mm can also be omitted.

If both a calendar date and a time of day appear to the left of a number
in the date string, but no relative item, then the number overrides the
year.

---

Next: [Specifying time zone rules](#Specifying-time-zone-rules), Previous: [Pure numbers in date strings](#Pure-numbers-in-date-strings), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.9 Seconds since the Epoch [¶](#Seconds-since-the-Epoch-1)

If you precede a number with ‘`@`’, it represents an internal
timestamp as a count of seconds. The number can contain an internal
decimal point (either ‘`.`’ or ‘`,`’); any excess precision not
supported by the internal representation is truncated toward minus
infinity. Such a number cannot be combined with any other date
item, as it specifies a complete timestamp.

Internally, computer times are represented as a count of seconds since
an Epoch—a well-defined point of time. On GNU and
POSIX systems, the Epoch is 1970-01-01 00:00:00 UTC, so
‘`@0`’ represents this time, ‘`@1`’ represents 1970-01-01
00:00:01 UTC, and so forth. GNU and most other
POSIX-compliant systems support such times as an extension
to POSIX, using negative counts, so that ‘`@-1`’
represents 1969-12-31 23:59:59 UTC.

Most modern systems count seconds with 64-bit two’s-complement integers
of seconds with nanosecond subcounts, which is a range that includes
the known lifetime of the universe with nanosecond resolution.
Some obsolescent systems count seconds with 32-bit two’s-complement
integers and can represent times from 1901-12-13 20:45:52 through
2038-01-19 03:14:07 UTC. A few systems sport other time ranges.

On most hosts, these counts ignore the presence of leap seconds.
For example, on most hosts ‘`@1483228799`’ represents 2016-12-31
23:59:59 UTC, ‘`@1483228800`’ represents 2017-01-01 00:00:00
UTC, and there is no way to represent the intervening leap second
2016-12-31 23:59:60 UTC.

---

Next: [Authors of `parse_datetime`](#Authors-of-parse_005fdatetime), Previous: [Seconds since the Epoch](#Seconds-since-the-Epoch), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.10 Specifying time zone rules [¶](#Specifying-time-zone-rules-1)

Normally, dates are interpreted using the rules of the current time
zone, which in turn are specified by the `TZ` environment
variable, or by a system default if `TZ` is not set. To specify a
different set of default time zone rules that apply just to one date,
start the date with a string of the form ‘`TZ="rule"`’. The
two quote characters (‘`"`’) must be present in the date, and any
quotes or backslashes within rule must be escaped by a
backslash.

For example, with the GNU `date` command you can
answer the question “What time is it in New York when a Paris clock
shows 6:30am on October 31, 2022?” by using a date beginning with
‘`TZ="Europe/Paris"`’ as shown in the following shell transcript:

```
$ export TZ="America/New_York"
$ date --date='TZ="Europe/Paris" 2022-10-31 06:30'
Mon Oct 31 01:30:00 EDT 2022
```

In this example, the `--date` operand begins with its own
`TZ` setting, so the rest of that operand is processed according
to ‘`Europe/Paris`’ rules, treating the string ‘`2022-11-14
06:30`’ as if it were in Paris. However, since the output of the
`date` command is processed according to the overall time zone
rules, it uses New York time. (Paris was normally six hours ahead of
New York in 2022, but this example refers to a brief Halloween period
when the gap was five hours.)

A `TZ` value is a rule that typically names a location in the
[‘`tz`’ database](https://www.iana.org/time-zones).
A recent catalog of location names appears in the
[TWiki Date and Time
Gateway](https://twiki.org/cgi-bin/xtra/tzdatepick.html). A few non-GNU hosts require a colon before a
location name in a `TZ` setting, e.g.,
‘`TZ=":America/New_York"`’.

The ‘`tz`’ database includes a wide variety of locations ranging
from ‘`Africa/Abidjan`’ to ‘`Pacific/Tongatapu`’, but
if you are at sea and have your own private time zone, or if you are
using a non-GNU host that does not support the ‘`tz`’
database, you may need to use a POSIX rule instead.
The previously-mentioned POSIX rule ‘`UTC0`’ says that the time zone
abbreviation is ‘`UTC`’, the zone is zero hours away from
Greenwich, and there is no daylight saving time.
POSIX rules can also specify nonzero Greenwich offsets.
For example, the following shell transcript answers the question
“What time is it five and a half hours east of Greenwich when a clock
seven hours west of Greenwich shows 9:50pm on July 12, 2022?”

```
$ TZ="<+0530>-5:30" date --date='TZ="<-07>+7" 2022-07-12 21:50'
Wed Jul 13 10:20:00 +0530 2022
```

This example uses the somewhat-confusing POSIX convention for rules.
‘`TZ="<-07>+7"`’ says that the time zone abbreviation is ‘`-07`’
and the time zone is 7 hours west of Greenwich, and
‘`TZ="<+0530>-5:30"`’ says that the time zone abbreviation is ‘`+0530`’
and the time zone is 5 hours 30 minutes east of Greenwich.
(One should never use a setting like ‘`TZ="UTC-5"`’, since
this would incorrectly imply that local time is five hours east of
Greenwich and the time zone is called “UTC”.)
Although trickier POSIX `TZ` settings like
‘`TZ="<-05>+5<-04>,M3.2.0/2,M11.1.0/2"`’ can specify some daylight
saving regimes, location-based settings like
‘`TZ="America/New_York"`’ are typically simpler and more accurate
historically. See [Specifying the Time Zone with `TZ`](https://www.gnu.org/software/libc/manual/html_mono/libc.html#TZ-Variable) in The GNU C Library.

---

Previous: [Specifying time zone rules](#Specifying-time-zone-rules), Up: [Date input formats](#Date-input-formats)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 29.11 Authors of `parse_datetime` [¶](#Authors-of-parse_005fdatetime-1)

`parse_datetime` started life as `getdate`, as originally
implemented by Steven M. Bellovin
([smb@research.att.com](mailto:smb@research.att.com)) while at the University of North Carolina
at Chapel Hill. The code was later tweaked by a couple of people on
Usenet, then completely overhauled by Rich $alz ([rsalz@bbn.com](mailto:rsalz@bbn.com))
and Jim Berets ([jberets@bbn.com](mailto:jberets@bbn.com)) in August, 1990. Various
revisions for the GNU system were made by David MacKenzie, Jim Meyering,
Paul Eggert and others, including renaming it to `get_date` to
avoid a conflict with the alternative Posix function `getdate`,
and a later rename to `parse_datetime`. The Posix function
`getdate` can parse more locale-specific dates using
`strptime`, but relies on an environment variable and external
file, and lacks the thread-safety of `parse_datetime`.

This chapter was originally produced by François Pinard
([pinard@iro.umontreal.ca](mailto:pinard@iro.umontreal.ca)) from the `parse_datetime.y` source code,
and then edited by K. Berry ([kb@cs.umb.edu](mailto:kb@cs.umb.edu)).

---

Next: [Opening the Software Toolbox](#Opening-the-software-toolbox), Previous: [Date input formats](#Date-input-formats), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 30 Version sort ordering [¶](#Version-sort-ordering-1)

* [Version sort overview](#Version-sort-overview)
* [Version sort implementation](#Version-sort-implementation)
* [Differences from Debian version sort](#Differences-from-Debian-version-sort)
* [Advanced Topics](#Advanced-version-sort-topics)

---

Next: [Version sort implementation](#Version-sort-implementation), Up: [Version sort ordering](#Version-sort-ordering)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 30.1 Version sort overview [¶](#Version-sort-overview-1)

*Version sort* puts items such as file names and lines of
text in an order that feels natural to people, when the text
contains a mixture of letters and digits.

Lexicographic sorting usually does not produce the order that one expects
because comparisons are made on a character-by-character basis.

Compare the sorting of the following items:

```
Lexicographic sort:          Version Sort:

a1                           a1
a120                         a2
a13                          a13
a2                           a120
```

Version sort functionality in GNU Coreutils is available in the ‘`ls -v`’,
‘`ls --sort=version`’, ‘`sort -V`’, and
‘`sort --version-sort`’ commands.

* [Using version sort in GNU Coreutils](#Using-version-sort-in-GNU-Coreutils)
* [Version sort and natural sort](#Version-sort-and-natural-sort)
* [Variations in version sort order](#Variations-in-version-sort-order)

---

Next: [Version sort and natural sort](#Version-sort-and-natural-sort), Up: [Version sort overview](#Version-sort-overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.1.1 Using version sort in GNU Coreutils [¶](#Using-version-sort-in-GNU-Coreutils-1)

Two GNU Coreutils programs use version sort: `ls` and `sort`.

To list files in version sort order, use `ls`
with the `-v` or `--sort=version` option:

```
default sort:              version sort:

$ ls -1                    $ ls -1 -v
a1                         a1
a100                       a1.4
a1.13                      a1.13
a1.4                       a1.40
a1.40                      a2
a2                         a100
```

To sort text files in version sort order, use `sort` with
the `-V` or `--version-sort` option:

```
$ cat input
b3
b11
b1
b20

lexicographic order:       version sort order:

$ sort input               $ sort -V input
b1                         b1
b11                        b3
b20                        b11
b3                         b20
```

To sort a specific field in a file, use `-k/--key` with
‘`V`’ type sorting, which is often combined with ‘`b`’ to
ignore leading blanks in the field:

```
$ cat input2
100   b3   apples
2000  b11  oranges
3000  b1   potatoes
4000  b20  bananas
$ sort -k 2bV,2 input2
3000  b1   potatoes
100   b3   apples
2000  b11  oranges
4000  b20  bananas
```

---

Next: [Variations in version sort order](#Variations-in-version-sort-order), Previous: [Using version sort in GNU Coreutils](#Using-version-sort-in-GNU-Coreutils), Up: [Version sort overview](#Version-sort-overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.1.2 Version sort and natural sort [¶](#Version-sort-and-natural-sort-1)

In GNU Coreutils, the name *version sort* was chosen because it is based
on Debian GNU/Linux’s algorithm of sorting packages’ versions.

Its goal is to answer questions like
“Which package is newer, `firefox-60.7.2` or `firefox-60.12.3`?”

In Coreutils this algorithm was slightly modified to work on more
general input such as textual strings and file names
(see [Differences from Debian version sort](#Differences-from-Debian-version-sort)).

In other contexts, such as other programs and other programming
languages, a similar sorting functionality is called
[natural sort](https://en.wikipedia.org/wiki/Natural_sort_order).

---

Previous: [Version sort and natural sort](#Version-sort-and-natural-sort), Up: [Version sort overview](#Version-sort-overview)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.1.3 Variations in version sort order [¶](#Variations-in-version-sort-order-1)

Currently there is no standard for version sort.

That is: there is no one correct way or universally agreed-upon way to
order items. Each program and each programming language can decide its
own ordering algorithm and call it “version sort”, “natural sort”,
or other names.

See [Other version/natural sort implementations](#Other-version_002fnatural-sort-implementations) for many examples of
differing sorting possibilities, each with its own rules and variations.

If you find a bug in the Coreutils implementation of version-sort, please
report it. See [Reporting version sort bugs](#Reporting-version-sort-bugs).

---

Next: [Differences from Debian version sort](#Differences-from-Debian-version-sort), Previous: [Version sort overview](#Version-sort-overview), Up: [Version sort ordering](#Version-sort-ordering)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 30.2 Version sort implementation [¶](#Version-sort-implementation-1)

GNU Coreutils version sort is based on the “upstream version”
part of
[Debian’s versioning scheme](https://www.debian.org/doc/debian-policy/ch-controlfields.html#version).

This section describes the GNU Coreutils sort ordering rules.

The next section ([Differences from Debian version sort](#Differences-from-Debian-version-sort)) describes some differences between GNU Coreutils
and Debian version sort.

* [Version-sort ordering rules](#Version_002dsort-ordering-rules)
* [Version sort is not the same as numeric sort](#Version-sort-is-not-the-same-as-numeric-sort)
* [Version sort punctuation](#Version-sort-punctuation)
* [Punctuation vs letters](#Punctuation-vs-letters)
* [The tilde ‘`~`’](#The-tilde-_007e)
* [Version sort ignores locale](#Version-sort-ignores-locale)

---

Next: [Version sort is not the same as numeric sort](#Version-sort-is-not-the-same-as-numeric-sort), Up: [Version sort implementation](#Version-sort-implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.2.1 Version-sort ordering rules [¶](#Version_002dsort-ordering-rules-1)

The version sort ordering rules are:

1. The strings are compared from left to right.
2. First the initial part of each string consisting entirely of non-digit
   bytes is determined.
   1. These two parts (either of which may be empty) are compared lexically.
      If a difference is found it is returned.
   2. The lexical comparison is a lexicographic comparison of byte strings,
      except that:
      1. ASCII letters sort before other bytes.
      2. A tilde sorts before anything, even an empty string.
3. Then the initial part of the remainder of each string that contains
   all the leading digits is determined. The numerical values represented by
   these two parts are compared, and any difference found is returned as
   the result of the comparison.
   1. For these purposes an empty string (which can only occur at the end of
      one or both version strings being compared) counts as zero.
   2. Because the numerical value is used, non-identical strings can compare
      equal. For example, ‘`123`’ compares equal to ‘`00123`’, and
      the empty string compares equal to ‘`0`’.
4. These two steps (comparing and removing initial non-digit strings and
   initial digit strings) are repeated until a difference is found or
   both strings are exhausted.

Consider the version-sort comparison of two file names:
`foo07.7z` and `foo7a.7z`. The two strings will be broken
down to the following parts, and the parts compared respectively from
each string:

```
foo  vs  foo   (rule 2, non-digits)
07   vs  7     (rule 3, digits)
.    vs  a.    (rule 2)
7    vs  7     (rule 3)
z    vs  z     (rule 2)
```

Comparison flow based on above algorithm:

1. The first parts (‘`foo`’) are identical.
2. The second parts (‘`07`’ and ‘`7`’) are compared numerically,
   and compare equal.
3. The third parts (‘`.`’ vs ‘`a.`’) are compared
   lexically by ASCII value (rule 2.B).
4. The first byte of the first string (‘`.`’) is compared
   to the first byte of the second string (‘`a`’).
5. Rule 2.B.a says letters sorts before non-letters.
   Hence, ‘`a`’ comes before ‘`.`’.
6. The returned result is that `foo7a.7z` comes before `foo07.7z`.

Result when using sort:

```
$ cat input3
foo07.7z
foo7a.7z
$ sort -V input3
foo7a.7z
foo07.7z
```

See [Differences from Debian version sort](#Differences-from-Debian-version-sort) for
additional rules that extend the Debian algorithm in Coreutils.

---

Next: [Version sort punctuation](#Version-sort-punctuation), Previous: [Version-sort ordering rules](#Version_002dsort-ordering-rules), Up: [Version sort implementation](#Version-sort-implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.2.2 Version sort is not the same as numeric sort [¶](#Version-sort-is-not-the-same-as-numeric-sort-1)

Consider the following text file:

```
$ cat input4
8.10
8.5
8.1
8.01
8.010
8.100
8.49

Numerical Sort:                   Version Sort:

$ sort -n input4                  $ sort -V input4
8.01                              8.01
8.010                             8.1
8.1                               8.5
8.10                              8.010
8.100                             8.10
8.49                              8.49
8.5                               8.100
```

Numeric sort (‘`sort -n`’) treats the entire string as a single numeric
value, and compares it to other values. For example, ‘`8.1`’, ‘`8.10`’ and
‘`8.100`’ are numerically equivalent, and are ordered together. Similarly,
‘`8.49`’ is numerically less than ‘`8.5`’, and appears before first.

Version sort (‘`sort -V`’) first breaks down the string into digit and
non-digit parts, and only then compares each part (see annotated
example in [Version-sort ordering rules](#Version_002dsort-ordering-rules)).

Comparing the string ‘`8.1`’ to ‘`8.01`’, first the
‘`8`’s are compared (and are identical), then the
dots (‘`.`’) are compared and are identical, and lastly the
remaining digits are compared numerically (‘`1`’ and ‘`01`’) –
which are numerically equal. Hence, ‘`8.01`’ and ‘`8.1`’
are grouped together.

Similarly, comparing ‘`8.5`’ to ‘`8.49`’ – the ‘`8`’
and ‘`.`’ parts are identical, then the numeric values ‘`5`’ and
‘`49`’ are compared. The resulting ‘`5`’ appears before ‘`49`’.

This sorting order (where ‘`8.5`’ comes before ‘`8.49`’) is common when
assigning versions to computer programs (while perhaps not intuitive
or “natural” for people).

---

Next: [Punctuation vs letters](#Punctuation-vs-letters), Previous: [Version sort is not the same as numeric sort](#Version-sort-is-not-the-same-as-numeric-sort), Up: [Version sort implementation](#Version-sort-implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.2.3 Version sort punctuation [¶](#Version-sort-punctuation-1)

Punctuation is sorted by ASCII order (rule 2.B).

```
$ touch 1.0.5_src.tar.gz 1.0_src.tar.gz
$ ls -v -1
1.0.5_src.tar.gz
1.0_src.tar.gz
```

Why is `1.0.5_src.tar.gz` listed before `1.0_src.tar.gz`?

Based on the version-sort ordering rules, the strings are broken down
into the following parts:

```
          1   vs  1               (rule 3, all digits)
          .   vs  .               (rule 2, all non-digits)
          0   vs  0               (rule 3)
          .   vs  _src.tar.gz     (rule 2)
          5   vs  empty string    (no more bytes in the file name)
_src.tar.gz   vs  empty string
```

The fourth parts (‘`.`’ and ‘`_src.tar.gz`’) are compared
lexically by ASCII order. The ‘`.`’ (ASCII value 46) is
less than ‘`_`’ (ASCII value 95) – and should be listed before it.

Hence, `1.0.5_src.tar.gz` is listed first.

If a different byte appears instead of the underscore (for
example, percent sign ‘`%`’ ASCII value 37, which is less
than dot’s ASCII value of 46), that file will be listed first:

```
$ touch   1.0.5_src.tar.gz     1.0%zzzzz.gz
1.0%zzzzz.gz
1.0.5_src.tar.gz
```

The same reasoning applies to the following example, as ‘`.`’ with
ASCII value 46 is less than ‘`/`’ with ASCII value 47:

```
$ cat input5
3.0/
3.0.5
$ sort -V input5
3.0.5
3.0/
```

---

Next: [The tilde ‘`~`’](#The-tilde-_007e), Previous: [Version sort punctuation](#Version-sort-punctuation), Up: [Version sort implementation](#Version-sort-implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.2.4 Punctuation vs letters [¶](#Punctuation-vs-letters-1)

Rule 2.B.a says letters sort before non-letters
(after breaking down a string to digit and non-digit parts).

```
$ cat input6
a%
az
$ sort -V input6
az
a%
```

The input strings consist entirely of non-digits, and based on the
above algorithm have only one part, all non-digits
(‘`a%`’ vs ‘`az`’).

Each part is then compared lexically,
byte-by-byte; ‘`a`’ compares identically in both
strings.

Rule 2.B.a says a letter like ‘`z`’ sorts before
a non-letter like ‘`%`’ – hence ‘`az`’ appears first (despite
‘`z`’ having ASCII value of 122, much larger than ‘`%`’
with ASCII value 37).

---

Next: [Version sort ignores locale](#Version-sort-ignores-locale), Previous: [Punctuation vs letters](#Punctuation-vs-letters), Up: [Version sort implementation](#Version-sort-implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.2.5 The tilde ‘`~`’ [¶](#The-tilde-_007e-1)

Rule 2.B.b says the tilde ‘`~`’ (ASCII 126) sorts
before other bytes, and before an empty string.

```
$ cat input7
1
1%
1.2
1~
~
$ sort -V input7
~
1~
1
1%
1.2
```

The sorting algorithm starts by breaking down the string into
non-digit (rule 2) and digit parts (rule 3).

In the above input file, only the last line in the input file starts
with a non-digit (‘`~`’). This is the first part. All other lines
in the input file start with a digit – their first non-digit part is
empty.

Based on rule 2.B.b, tilde ‘`~`’ sorts before other bytes
and before the empty string – hence it comes before all other strings,
and is listed first in the sorted output.

The remaining lines (‘`1`’, ‘`1%`’, ‘`1.2`’, ‘`1~`’)
follow similar logic: The digit part is extracted (1 for all strings)
and compares equal. The following extracted parts for the remaining
input lines are: empty part, ‘`%`’, ‘`.`’, ‘`~`’.

Tilde sorts before all others, hence the line ‘`1~`’ appears next.

The remaining lines (‘`1`’, ‘`1%`’, ‘`1.2`’) are sorted based
on previously explained rules.

---

Previous: [The tilde ‘`~`’](#The-tilde-_007e), Up: [Version sort implementation](#Version-sort-implementation)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.2.6 Version sort ignores locale [¶](#Version-sort-ignores-locale-1)

In version sort, Unicode characters are compared byte-by-byte according
to their binary representation, ignoring their Unicode value or the
current locale.

Most commonly, Unicode characters are encoded as UTF-8 bytes; for
example, GREEK SMALL LETTER ALPHA (U+03B1, ‘`α`’) is encoded as the
UTF-8 sequence ‘`0xCE 0xB1`’). The encoding is compared
byte-by-byte, e.g., first ‘`0xCE`’ (decimal value 206) then
‘`0xB1`’ (decimal value 177).

```
$ touch aa az "a%" "aα"
$ ls -1 -v
aa
az
a%
aα
```

Ignoring the first letter (‘`a`’) which is identical in all
strings, the compared values are:

‘`a`’ and ‘`z`’ are letters, and sort before
all other non-digits.

Then, percent sign ‘`%`’ (ASCII value 37) is compared to the
first byte of the UTF-8 sequence of ‘`α`’, which is 0xCE or 206). The
value 37 is smaller, hence ‘`a%`’ is listed before ‘`aα`’.

---

Next: [Advanced Topics](#Advanced-version-sort-topics), Previous: [Version sort implementation](#Version-sort-implementation), Up: [Version sort ordering](#Version-sort-ordering)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 30.3 Differences from Debian version sort [¶](#Differences-from-Debian-version-sort-1)

GNU Coreutils version sort differs slightly from the
official Debian algorithm, in order to accommodate more general usage
and file name listing.

* [Hyphen-minus ‘`-`’ and colon ‘`:`’](#Hyphen_002dminus-and-colon)
* [Special priority in GNU Coreutils version sort](#Special-priority-in-GNU-Coreutils-version-sort)
* [Special handling of file extensions](#Special-handling-of-file-extensions)
* [Comparing two strings using Debian’s algorithm](#Comparing-two-strings-using-Debian_0027s-algorithm)

---

Next: [Special priority in GNU Coreutils version sort](#Special-priority-in-GNU-Coreutils-version-sort), Up: [Differences from Debian version sort](#Differences-from-Debian-version-sort)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.3.1 Hyphen-minus ‘`-`’ and colon ‘`:`’ [¶](#Hyphen_002dminus-_002d-and-colon-_003a)

In Debian’s version string syntax the version consists of three parts:

```
[epoch:]upstream_version[-debian_revision]
```

The ‘`epoch`’ and ‘`debian_revision`’ parts are optional.

Example of such version strings:

```
60.7.2esr-1~deb9u1
52.9.0esr-1~deb9u1
1:2.3.4-1+b2
327-2
1:1.0.13-3
2:1.19.2-1+deb9u5
```

If the ‘`debian_revision part`’ is not present,
hyphens ‘`-`’ are not allowed.
If epoch is not present, colons ‘`:`’ are not allowed.

If these parts are present, hyphen and/or colons can appear only once
in valid Debian version strings.

In GNU Coreutils, such restrictions are not reasonable (a file name can
have many hyphens, a line of text can have many colons).

As a result, in GNU Coreutils hyphens and colons are treated exactly
like all other punctuation, i.e., they are sorted after
letters. See [Version sort punctuation](#Version-sort-punctuation).

In Debian, these characters are treated differently than in Coreutils:
a version string with hyphen will sort before similar strings without
hyphens.

Compare:

```
$ touch 1ab-cd 1abb
$ ls -v -1
1abb
1ab-cd
$ if dpkg --compare-versions 1abb lt 1ab-cd
> then echo sorted
> else echo out of order
> fi
out of order
```

For further details, see [Comparing two strings using Debian’s algorithm](#Comparing-two-strings-using-Debian_0027s-algorithm) and [GNU Bug 35939](https://bugs.gnu.org/35939).

---

Next: [Special handling of file extensions](#Special-handling-of-file-extensions), Previous: [Hyphen-minus ‘`-`’ and colon ‘`:`’](#Hyphen_002dminus-and-colon), Up: [Differences from Debian version sort](#Differences-from-Debian-version-sort)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.3.2 Special priority in GNU Coreutils version sort [¶](#Special-priority-in-GNU-Coreutils-version-sort-1)

In GNU Coreutils version sort, the following items have
special priority and sort before all other strings (listed in order):

1. The empty string
2. The string ‘`.`’ (a single dot, ASCII 46)
3. The string ‘`..`’ (two dots)
4. Strings starting with dot (‘`.`’) sort before
   strings starting with any other byte.

Example:

```
$ printf '%s\n' a "" b "." c  ".."  ".d20" ".d3"  | sort -V
.
..
.d3
.d20
a
b
c
```

These priorities make perfect sense for ‘`ls -v`’: The special
files dot ‘`.`’ and dot-dot ‘`..`’ will be listed
first, followed by any hidden files (files starting with a dot),
followed by non-hidden files.

For ‘`sort -V`’ these priorities might seem arbitrary. However,
because the sorting code is shared between the `ls` and `sort`
program, the ordering rules are the same.

---

Next: [Comparing two strings using Debian’s algorithm](#Comparing-two-strings-using-Debian_0027s-algorithm), Previous: [Special priority in GNU Coreutils version sort](#Special-priority-in-GNU-Coreutils-version-sort), Up: [Differences from Debian version sort](#Differences-from-Debian-version-sort)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.3.3 Special handling of file extensions [¶](#Special-handling-of-file-extensions-1)

GNU Coreutils version sort implements specialized handling
of strings that look like file names with extensions.
This enables slightly more natural ordering of file
names.

The following additional rules apply when comparing two strings where
both begin with non-‘`.`’. They also apply when comparing two
strings where both begin with ‘`.`’ but neither is ‘`.`’ or ‘`..`’.

1. A suffix (i.e., a file extension) is defined as: a dot, followed by an
   ASCII letter or tilde, followed by zero or more ASCII letters, digits,
   or tildes; all repeated zero or more times, and ending at string end.
   This is equivalent to matching the extended regular expression
   `(\.[A-Za-z~][A-Za-z0-9~]*)*$` in the C locale.
   The longest such match is used, except that a suffix is not
   allowed to match an entire nonempty string.
2. The suffixes are temporarily removed, and the strings are compared
   without them, using version sort (see [Version-sort ordering rules](#Version_002dsort-ordering-rules)) without special priority (see [Special priority in GNU Coreutils version sort](#Special-priority-in-GNU-Coreutils-version-sort)).
3. If the suffix-less strings do not compare equal, this comparison
   result is used and the suffixes are effectively ignored.
4. If the suffix-less strings compare equal, the suffixes are restored
   and the entire strings are compared using version sort.

Examples for rule 1:

* ‘`hello-8.txt`’: the suffix is ‘`.txt`’
* ‘`hello-8.2.txt`’: the suffix is ‘`.txt`’
  (‘`.2`’ is not included because the dot is not followed by a letter)
* ‘`hello-8.0.12.tar.gz`’: the suffix is ‘`.tar.gz`’ (‘`.0.12`’
  is not included)
* ‘`hello-8.2`’: no suffix (suffix is an empty string)
* ‘`hello.foobar65`’: the suffix is ‘`.foobar65`’
* ‘`gcc-c++-10.8.12-0.7rc2.fc9.tar.bz2`’: the suffix is
  ‘`.fc9.tar.bz2`’ (‘`.7rc2`’ is not included as it begins with a digit)
* ‘`.autom4te.cfg`’: the suffix is the entire string.

Examples for rule 2:

* Comparing ‘`hello-8.txt`’ to ‘`hello-8.2.12.txt`’, the
  ‘`.txt`’ suffix is temporarily removed from both strings.
* Comparing ‘`foo-10.3.tar.gz`’ to ‘`foo-10.tar.xz`’, the suffixes
  ‘`.tar.gz`’ and ‘`.tar.xz`’ are temporarily removed from the
  strings.

Example for rule 3:

* Comparing ‘`hello.foobar65`’ to ‘`hello.foobar4`’, the suffixes
  (‘`.foobar65`’ and ‘`.foobar4`’) are temporarily removed. The
  remaining strings are identical (‘`hello`’). The suffixes are then
  restored, and the entire strings are compared (‘`hello.foobar4`’ comes
  first).

Examples for rule 4:

* When comparing the strings ‘`hello-8.2.txt`’ and ‘`hello-8.10.txt`’, the
  suffixes (‘`.txt`’) are temporarily removed. The remaining strings
  (‘`hello-8.2`’ and ‘`hello-8.10`’) are compared as previously described
  (‘`hello-8.2`’ comes first).
  *(In this case the suffix removal algorithm
  does not have a noticeable effect on the resulting order.)*

**How does the suffix-removal algorithm effect ordering results?**

Consider the comparison of hello-8.txt and hello-8.2.txt.

Without the suffix-removal algorithm, the strings will be broken down
to the following parts:

```
hello-  vs  hello-  (rule 2, all non-digits)
8       vs  8       (rule 3, all digits)
.txt    vs  .       (rule 2)
empty   vs  2
empty   vs  .txt
```

The comparison of the third parts (‘`.`’ vs
‘`.txt`’) will determine that the shorter string comes first –
resulting in `hello-8.2.txt` appearing first.

Indeed this is the order in which Debian’s `dpkg` compares the strings.

A more natural result is that `hello-8.txt` should come before
`hello-8.2.txt`, and this is where the suffix-removal comes into play:

The suffixes (‘`.txt`’) are removed, and the remaining strings are
broken down into the following parts:

```
hello-  vs  hello-  (rule 2, all non-digits)
8       vs  8       (rule 3, all digits)
empty   vs  .       (rule 2)
empty   vs  2
```

As empty strings sort before non-empty strings, the result is ‘`hello-8`’
being first.

A real-world example would be listing files such as:
`gcc_10.fc9.tar.gz`
and `gcc_10.8.12.7rc2.fc9.tar.bz2`: Debian’s algorithm would list
`gcc_10.8.12.7rc2.fc9.tar.bz2` first, while ‘`ls -v`’ will list
`gcc_10.fc9.tar.gz` first.

These priorities make sense for ‘`ls -v`’:
Versioned files will be listed in a more natural order.

For ‘`sort -V`’ these priorities might seem arbitrary. However,
because the sorting code is shared between the `ls` and `sort`
program, the ordering rules are the same.

---

Previous: [Special handling of file extensions](#Special-handling-of-file-extensions), Up: [Differences from Debian version sort](#Differences-from-Debian-version-sort)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.3.4 Comparing two strings using Debian’s algorithm [¶](#Comparing-two-strings-using-Debian_0027s-algorithm-1)

The Debian program `dpkg` (available on all Debian and Ubuntu
installations) can compare two strings using the `--compare-versions`
option.

To use it, create a helper shell function (simply copy & paste the
following snippet to your shell command-prompt):

```
compver() {
  if dpkg --compare-versions "$1" lt "$2"
  then printf '%s\n' "$1" "$2"
  else printf '%s\n' "$2" "$1"
  fi
}
```

Then compare two strings by calling `compver`:

```
$ compver 8.49 8.5
8.5
8.49
```

Note that `dpkg` will warn if the strings have invalid syntax:

```
$ compver "foo07.7z" "foo7a.7z"
dpkg: warning: version 'foo07.7z' has bad syntax:
               version number does not start with digit
dpkg: warning: version 'foo7a.7z' has bad syntax:
               version number does not start with digit
foo7a.7z
foo07.7z
$ compver "3.0/" "3.0.5"
dpkg: warning: version '3.0/' has bad syntax:
               invalid character in version number
3.0.5
3.0/
```

To illustrate the different handling of hyphens between Debian and
Coreutils algorithms (see
[Hyphen-minus ‘`-`’ and colon ‘`:`’](#Hyphen_002dminus-and-colon)):

```
$ compver abb ab-cd 2>/dev/null     $ printf 'abb\nab-cd\n' | sort -V
ab-cd                               abb
abb                                 ab-cd
```

To illustrate the different handling of file extension: (see [Special handling of file extensions](#Special-handling-of-file-extensions)):

```
$ compver hello-8.txt hello-8.2.txt 2>/dev/null
hello-8.2.txt
hello-8.txt
$ printf '%s\n' hello-8.txt hello-8.2.txt | sort -V
hello-8.txt
hello-8.2.txt
```

---

Previous: [Differences from Debian version sort](#Differences-from-Debian-version-sort), Up: [Version sort ordering](#Version-sort-ordering)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### 30.4 Advanced Topics [¶](#Advanced-Topics)

* [Reporting version sort bugs](#Reporting-version-sort-bugs)
* [Other version/natural sort implementations](#Other-version_002fnatural-sort-implementations)
* [Related source code](#Related-source-code)

---

Next: [Other version/natural sort implementations](#Other-version_002fnatural-sort-implementations), Up: [Advanced Topics](#Advanced-version-sort-topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.4.1 Reporting version sort bugs [¶](#Reporting-version-sort-bugs-1)

If you suspect a bug in GNU Coreutils version sort (i.e., in the
output of ‘`ls -v`’ or ‘`sort -V`’), please first check the following:

1. Is the result consistent with Debian’s own ordering (using `dpkg`, see
   [Comparing two strings using Debian’s algorithm](#Comparing-two-strings-using-Debian_0027s-algorithm))? If it is, then this
   is not a bug – please do not report it.
2. If the result differs from Debian’s, is it explained by one of the
   sections in [Differences from Debian version sort](#Differences-from-Debian-version-sort)? If it is,
   then this is not a bug – please do not report it.
3. If you have a question about specific ordering which is not explained
   here, please write to [coreutils@gnu.org](mailto:coreutils@gnu.org), and provide a
   concise example that will help us diagnose the issue.
4. If you still suspect a bug which is not explained by the above, please
   write to [bug-coreutils@gnu.org](mailto:bug-coreutils@gnu.org) with a concrete example of the
   suspected incorrect output, with details on why you think it is
   incorrect.

---

Next: [Related source code](#Related-source-code), Previous: [Reporting version sort bugs](#Reporting-version-sort-bugs), Up: [Advanced Topics](#Advanced-version-sort-topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.4.2 Other version/natural sort implementations [¶](#Other-version_002fnatural-sort-implementations-1)

As previously mentioned, there are multiple variations on
version/natural sort, each with its own rules. Some examples are:

* Natural Sorting variants in
  [Rosetta Code](https://rosettacode.org/wiki/Natural_sorting).
* Python’s [natsort package](https://pypi.org/project/natsort/)
  (includes detailed description of their sorting rules:
  [natsort – how it works](https://natsort.readthedocs.io/en/master/howitworks.html)).
* Ruby’s [version\_sorter](https://github.com/github/version_sorter).
* Perl has multiple packages for natural and version sorts
  (each likely with its own rules and nuances):
  [Sort::Naturally](https://metacpan.org/pod/Sort::Naturally),
  [Sort::Versions](https://metacpan.org/pod/Sort::Versions),
  [CPAN::Version](https://metacpan.org/pod/CPAN::Version).
* PHP has a built-in function
  [natsort](https://www.php.net/manual/en/function.natsort.php).
* NodeJS’s [natural-sort package](https://www.npmjs.com/package/natural-sort).
* In zsh, the
  [glob modifier](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Glob-Qualifiers) ‘`*(n)`’ will expand to files in natural sort order.
* When writing C programs, the GNU libc library (‘`glibc`’)
  provides the
  [strverscmp(3)](https://man7.org/linux/man-pages/man3/strverscmp.3.html) function to compare two strings, and
  [versionsort(3)](https://man7.org/linux/man-pages/man3/versionsort.3.html)
  function to compare two directory entries (despite the names, they are
  not identical to GNU Coreutils version sort ordering).
* Using Debian’s sorting algorithm in:
  + python: [Stack Overflow Example #4957741](https://stackoverflow.com/a/4957741).
  + NodeJS: [deb-version-compare](https://www.npmjs.com/package/deb-version-compare).

---

Previous: [Other version/natural sort implementations](#Other-version_002fnatural-sort-implementations), Up: [Advanced Topics](#Advanced-version-sort-topics)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

#### 30.4.3 Related source code [¶](#Related-source-code-1)

* Debian’s code which splits a version string into
  `epoch/upstream_version/debian_revision` parts:
  [parsehelp.c:parseversion()](https://git.dpkg.org/cgit/dpkg/dpkg.git/tree/lib/dpkg/parsehelp.c#n191).
* Debian’s code which performs the `upstream_version` comparison:
  [version.c](https://git.dpkg.org/cgit/dpkg/dpkg.git/tree/lib/dpkg/version.c#n140).
* Gnulib code (used by GNU Coreutils) which performs the version comparison:
  [filevercmp.c](https://git.savannah.gnu.org/cgit/gnulib.git/tree/lib/filevercmp.c).

---

Next: [GNU Free Documentation License](#GNU-Free-Documentation-License), Previous: [Version sort ordering](#Version-sort-ordering), Up: [GNU Coreutils](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]## 31 Opening the Software Toolbox [¶](#Opening-the-Software-Toolbox)

An earlier version of this chapter appeared in
[the
What’s GNU? column of the June 1994 Linux Journal](https://www.linuxjournal.com/article.php?sid=2762).
It was written by Arnold Robbins.

* [Toolbox Introduction](#Toolbox-introduction)
* [I/O Redirection](#I_002fO-redirection)
* [The `who` Command](#The-who-command)
* [The `cut` Command](#The-cut-command)
* [The `sort` Command](#The-sort-command)
* [The `uniq` Command](#The-uniq-command)
* [Putting the Tools Together](#Putting-the-tools-together)

---

Next: [I/O Redirection](#I_002fO-redirection), Up: [Opening the Software Toolbox](#Opening-the-software-toolbox)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### Toolbox Introduction [¶](#Toolbox-Introduction)

This month’s column is only peripherally related to the GNU Project, in
that it describes a number of the GNU tools on your GNU/Linux system
and how they
might be used. What it’s really about is the “Software Tools” philosophy
of program development and usage.

The software tools philosophy was an important and integral concept
in the initial design and development of Unix (of which GNU/Linux and GNU are
essentially clones). Unfortunately, in the modern day press of
Internetworking and flashy GUIs, it seems to have fallen by the
wayside. This is a shame, since it provides a powerful mental model
for solving many kinds of problems.

Many people carry a Swiss Army knife around in their pants pockets (or
purse). A Swiss Army knife is a handy tool to have: it has several knife
blades, a screwdriver, tweezers, toothpick, nail file, corkscrew, and perhaps
a number of other things on it. For the everyday, small miscellaneous jobs
where you need a simple, general purpose tool, it’s just the thing.

On the other hand, an experienced carpenter doesn’t build a house using
a Swiss Army knife. Instead, he has a toolbox chock full of specialized
tools – a saw, a hammer, a screwdriver, a plane, and so on. And he knows
exactly when and where to use each tool; you won’t catch him hammering nails
with the handle of his screwdriver.

The Unix developers at Bell Labs were all professional programmers and trained
computer scientists. They had found that while a one-size-fits-all program
might appeal to a user because there’s only one program to use, in practice
such programs are

1. difficult to write,
2. difficult to maintain and
   debug, and
3. difficult to extend to meet new situations.

Instead, they felt that programs should be specialized tools. In short, each
program “should do one thing well.” No more and no less. Such programs are
simpler to design, write, and get right – they only do one thing.

Furthermore, they found that with the right machinery for hooking programs
together, that the whole was greater than the sum of the parts. By combining
several special purpose programs, you could accomplish a specific task
that none of the programs was designed for, and accomplish it much more
quickly and easily than if you had to write a special purpose program.
We will see some (classic) examples of this further on in the column.
(An important additional point was that, if necessary, take a detour
and build any software tools you may need first, if you don’t already
have something appropriate in the toolbox.)

---

Next: [The `who` Command](#The-who-command), Previous: [Toolbox Introduction](#Toolbox-introduction), Up: [Opening the Software Toolbox](#Opening-the-software-toolbox)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### I/O Redirection [¶](#I_002fO-Redirection)

Hopefully, you are familiar with the basics of I/O redirection in the
shell, in particular the concepts of “standard input,” “standard output,”
and “standard error”. Briefly, “standard input” is a data source, where
data comes from. A program should not need to either know or care if the
data source is a regular file, a keyboard, a magnetic tape, or even a punched
card reader. Similarly, “standard output” is a data sink, where data goes
to. The program should neither know nor care where this might be.
Programs that only read their standard input, do something to the data,
and then send it on, are called *filters*, by analogy to filters in a
water pipeline.

With the Unix shell, it’s very easy to set up data pipelines:

```
program_to_create_data | filter1 | ... | filterN > final.pretty.data
```

We start out by creating the raw data; each filter applies some successive
transformation to the data, until by the time it comes out of the pipeline,
it is in the desired form.

This is fine and good for standard input and standard output. Where does the
standard error come in to play? Well, think about `filter1` in
the pipeline above. What happens if it encounters an error in the data it
sees? If it writes an error message to standard output, it will just
disappear down the pipeline into `filter2`’s input, and the
user will probably never see it. So programs need a place where they can send
error messages so that the user will notice them. This is standard error,
and it is usually connected to your console or window, even if you have
redirected standard output of your program away from your screen.

For filter programs to work together, the format of the data has to be
agreed upon. The most straightforward and easiest format to use is simply
lines of text. Unix data files are generally just streams of bytes, with
lines delimited by the ASCII LF (Line Feed) character,
conventionally called a “newline” in the Unix literature. (This is
`'\n'` if you’re a C programmer.) This is the format used by all
the traditional filtering programs. (Many earlier operating systems
had elaborate facilities and special purpose programs for managing
binary data. Unix has always shied away from such things, under the
philosophy that it’s easiest to simply be able to view and edit your
data with a text editor.)

OK, enough introduction. Let’s take a look at some of the tools, and then
we’ll see how to hook them together in interesting ways. In the following
discussion, we will only present those command line options that interest
us. As you should always do, double check your system documentation
for the full story.

---

Next: [The `cut` Command](#The-cut-command), Previous: [I/O Redirection](#I_002fO-redirection), Up: [Opening the Software Toolbox](#Opening-the-software-toolbox)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### The `who` Command [¶](#The-who-Command)

The first program is the `who` command. By itself, it generates a
list of the users who are currently logged in. Although I’m writing
this on a single-user system, we’ll pretend that several people are
logged in:

```
$ who
-| arnold   console Jan 22 19:57
-| miriam   ttyp0   Jan 23 14:19(:0.0)
-| bill     ttyp1   Jan 21 09:32(:0.0)
-| arnold   ttyp2   Jan 23 20:48(:0.0)
```

Here, the ‘`$`’ is the usual shell prompt, at which I typed ‘`who`’.
There are three people logged in, and I am logged in twice. On traditional
Unix systems, user names are never more than eight characters long. This
little bit of trivia will be useful later. The output of `who` is nice,
but the data is not all that exciting.

---

Next: [The `sort` Command](#The-sort-command), Previous: [The `who` Command](#The-who-command), Up: [Opening the Software Toolbox](#Opening-the-software-toolbox)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### The `cut` Command [¶](#The-cut-Command)

The next program we’ll look at is the `cut` command. This program
cuts out columns or fields of input data. For example, we can tell it
to print just the login name and full name from the `/etc/passwd`
file. The `/etc/passwd` file has seven fields, separated by
colons:

```
arnold:xyzzy:2076:10:Arnold D. Robbins:/home/arnold:/bin/bash
```

To get the first and fifth fields, we would use `cut` like this:

```
$ cut -d: -f1,5 /etc/passwd
-| root:Operator
...
-| arnold:Arnold D. Robbins
-| miriam:Miriam A. Robbins
...
```

With the `-c` option, `cut` will cut out specific characters
(i.e., columns) in the input lines. This is useful for input data
that has fixed width fields, and does not have a field separator. For
example, list the Monday dates for the current month:

```
$ cal | cut -c 3-5
-|Mo
-|
-|  6
-| 13
-| 20
-| 27
```

---

Next: [The `uniq` Command](#The-uniq-command), Previous: [The `cut` Command](#The-cut-command), Up: [Opening the Software Toolbox](#Opening-the-software-toolbox)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### The `sort` Command [¶](#The-sort-Command)

Next we’ll look at the `sort` command. This is one of the most
powerful commands on a Unix-style system; one that you will often find
yourself using when setting up fancy data plumbing.

The `sort`
command reads and sorts each file named on the command line. It then
merges the sorted data and writes it to standard output. It will read
standard input if no files are given on the command line (thus
making it into a filter). The sort is based on the character collating
sequence or based on user-supplied ordering criteria.

---

Next: [Putting the Tools Together](#Putting-the-tools-together), Previous: [The `sort` Command](#The-sort-command), Up: [Opening the Software Toolbox](#Opening-the-software-toolbox)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### The `uniq` Command [¶](#The-uniq-Command)

Finally (at least for now), we’ll look at the `uniq` program. When
sorting data, you will often end up with duplicate lines, lines that
are identical. Usually, all you need is one instance of each line.
This is where `uniq` comes in. The `uniq` program reads its
standard input. It prints only one
copy of each repeated line. It does have several options. Later on,
we’ll use the `-c` option, which prints each unique line, preceded
by a count of the number of times that line occurred in the input.

---

Previous: [The `uniq` Command](#The-uniq-command), Up: [Opening the Software Toolbox](#Opening-the-software-toolbox)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Concept-index "Index")]

### Putting the Tools Together [¶](#Putting-the-Tools-Together)

Now, let’s suppose this is a large ISP server system with dozens of users
logged in. The management wants the system administrator to write a
program that will
generate a sorted list of logged in users. Furthermore, even if a user
is logged in multiple times, his or her name should only show up in the
output once.

The administrator could sit down with the system documentation and write a C
program that did this. It would take perhaps a couple of hundred lines
of code and about two hours to write it, test it, and debug it.
However, knowing the software toolbox, the administrator can instead start out
by generating just a list of logged on users:

```
$ who | cut -c1-8
-| arnold
-| miriam
-| bill
-| arnold
```

Next, sort the list:

```
$ who | cut -c1-8 | sort
-| arnold
-| arnold
-| bill
-| miriam
```

Finally, run the sorted list through `uniq`, to weed out duplicates:

```
$ who | cut -c1-8 | sort | uniq
-| arnold
-| bill
-| miriam
```

The `sort` command actually has a `-u` option that does what
`uniq` does. However, `uniq` has other uses for which one
cannot substitute ‘`sort -u`’.

The administrator puts this pipeline into a shell script, and makes it
available for
all the users on the system (‘`#`’ is the system administrator,
or `root`, prompt):

```
# cat > /usr/local/bin/listusers
who | cut -c1-8 | sort | uniq
^D
