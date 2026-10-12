### 43.8 Parsing and Formatting Times [¶](#Parsing-and-Formatting-Times)

These functions convert time values to text in a string, and vice
versa. Time values are either represented as a Lisp timestamp
(see [Time of Day](#Time-of-Day)) or a decoded time structure (see [Time Conversion](#Time-Conversion)).

Function: **date-to-time** string [¶](#index-date_002dto_002dtime)
:   This function parses the time-string string and returns the
    corresponding Lisp timestamp. The argument string should represent
    a date-time, and should be in one of the forms recognized by
    `parse-time-string` (see below). This function assumes local time
    if string lacks explicit time zone information,
    and assumes earliest values if string lacks month, day, or time.
    The operating system limits the range of time and zone values.

Function: **parse-time-string** string [¶](#index-parse_002dtime_002dstring)
:   This function parses the time-string string into a decoded time
    structure (see [Time Conversion](#Time-Conversion)). The argument string should
    resemble an RFC 822 (or later) or ISO 8601 string, like “Fri, 25 Mar
    2016 16:24:56 +0100” or “1998-09-12T12:21:54-0200”, but this
    function will attempt to parse less well-formed time strings as well.
    It parses an incomplete string like “2024-08-13” to an incomplete
    structure like ‘`(nil nil nil 13 8 2024 nil -1 nil)`’, in which
    an unknown DST value is −1 and other unknown values are `nil`.

    Note that, unlike `decode-time` (see [Time Conversion](#Time-Conversion)), this
    function does not interpret the time string, and in particular the
    values of daylight-saving and timezone or UTC offset parts of the
    string argument do not affect the returned value of date and time,
    they only affect the last two members of the returned decoded time
    structure. For example, if the time-zone information is present in
    string, the decoded time structure will include it; otherwise the
    time-zone member of the returned value will be `nil`. In other
    words, this function simply parses the textual representation of date
    and time into separate numerical values, and doesn’t care whether the
    input time is local or UTC.

    If a Lisp program passes the return value of this function to some other
    time-related API, it should make sure the `nil` members of the
    decoded time structure are interpreted correctly, and in particular the
    lack of time-zone information is interpreted as UTC or local time,
    according to the needs of the calling program.

Function: **iso8601-parse** string [¶](#index-iso8601_002dparse)
:   For a more strict function (that will error out upon invalid input),
    Lisp programs can use this function instead. It can parse all variants
    of the ISO 8601 standard, so in addition to the formats mentioned above,
    it also parses things like “1998W45-3” (week number) and “1998-245”
    (ordinal day number). To parse durations, there’s
    `iso8601-parse-duration`, and to parse intervals, there’s
    `iso8601-parse-interval`. All these functions return decoded time
    structures, except the final one, which returns three of them (the
    start, the end, and the duration).
    Like `parse-time-string`, this function does not interpret the time
    string, and in particular the time-zone designator or UTC offset that is
    part of the string argument does not affect the returned value of
    date and time, it only affects the last two members of the returned
    decoded time structure. The ISO 8601 standard specifies that date/time
    strings that do not include information about UTC relation are assumed
    to be in local time, but this function does not do that, because it
    doesn’t interpret the time values. For example, if the time-zone
    information is present in string, the decoded time structure will
    include it; otherwise the time-zone member of the returned value will be
    `nil`. In other words, this function simply parses the textual
    representation of date and time into separate numerical values, and
    doesn’t care whether the input time is local or UTC.

Function: **iso8601-parse-duration** string [¶](#index-iso8601_002dparse_002dduration)
:   This function parses an ISO 8601 time duration string
    and returns a decoded time structure.

Function: **iso8601-parse-interval** string [¶](#index-iso8601_002dparse_002dinterval)
:   This function parses an ISO 8601 time interval string
    and returns three decoded time structures
    representing the start, the end, and the duration.

Function: **format-time-string** format-string &optional time zone [¶](#index-format_002dtime_002dstring)
:   This function converts time (which should be a Lisp timestamp,
    and defaults to the current time if time is omitted or
    `nil`) to a string according to format-string. The
    conversion uses the time zone rule zone, which defaults to the
    current time zone rule. See [Time Zone Rules](#Time-Zone-Rules). The argument
    format-string may contain ‘`%`’-sequences which say to
    substitute parts of the time. Here is a table of what the
    ‘`%`’-sequences mean:

    ‘`%a`’
    :   This stands for the abbreviated name of the day of week.

    ‘`%A`’
    :   This stands for the full name of the day of week.

    ‘`%b`’
    :   This stands for the abbreviated name of the month.

    ‘`%B`’
    :   This stands for the full name of the month.

    ‘`%c`’
    :   This is a synonym for ‘`%x %X`’.

    ‘`%C`’
    :   This stands for the century, that is, the year divided by 100,
        truncated toward zero.
        The default field width is 2.

    ‘`%d`’
    :   This stands for the day of month, zero-padded.

    ‘`%D`’
    :   This is a synonym for ‘`%m/%d/%y`’.

    ‘`%e`’
    :   This stands for the day of month, blank-padded.

    ‘`%F`’
    :   This stands for the ISO 8601 date format, which is like
        ‘`%+4Y-%m-%d`’ except that any flags or field width override the
        ‘`+`’ and (after subtracting 6) the ‘`4`’.

    ‘`%g`’ [¶](#index-ISO-week_002c-in-time-formatting)
    :   This stands for the year without century (00–99) corresponding to the
        current *ISO week* number. ISO weeks start on Monday and end on
        Sunday. If an ISO week begins in one year and ends in another, the
        rules regarding which year ‘`%g`’ will produce are complex and will
        not be described here; however, in general, if most of the week’s days
        are in the ending year, ‘`%g`’ will produce that year.

    ‘`%G`’
    :   This stands for the year with century corresponding to the current ISO
        week number.

    ‘`%h`’
    :   This is a synonym for ‘`%b`’.

    ‘`%H`’
    :   This stands for the hour (00–23).

    ‘`%I`’
    :   This stands for the hour (01–12).

    ‘`%j`’
    :   This stands for the day of the year (001–366).

    ‘`%k`’
    :   This stands for the hour (0–23), blank padded.

    ‘`%l`’
    :   This stands for the hour (1–12), blank padded.

    ‘`%m`’
    :   This stands for the month (01–12).

    ‘`%M`’
    :   This stands for the minute (00–59).

    ‘`%n`’
    :   This stands for a newline.

    ‘`%N`’
    :   This stands for the nanoseconds (000000000–999999999). To ask for
        fewer digits, use ‘`%3N`’ for milliseconds, ‘`%6N`’ for
        microseconds, etc. Any excess digits are discarded, without rounding.

    ‘`%p`’
    :   This stands for ‘`AM`’ or ‘`PM`’, as appropriate.

    ‘`%q`’
    :   This stands for the calendar quarter (1–4).

    ‘`%r`’
    :   This is a synonym for ‘`%I:%M:%S %p`’.

    ‘`%R`’
    :   This is a synonym for ‘`%H:%M`’.

    ‘`%s`’
    :   This stands for the integer number of seconds since the epoch.

    ‘`%S`’
    :   This stands for the second (00–59, or 00–60 on platforms
        that support leap seconds).

    ‘`%t`’
    :   This stands for a tab character.

    ‘`%T`’
    :   This is a synonym for ‘`%H:%M:%S`’.

    ‘`%u`’
    :   This stands for the numeric day of week (1–7). Monday is day 1.

    ‘`%U`’
    :   This stands for the week of the year (01–52), assuming that weeks
        start on Sunday. If January 1 is not a Sunday, the first partial week
        is week zero.

    ‘`%V`’
    :   This stands for the week of the year according to ISO 8601. Note that,
        unlike ‘`%U`’ and ‘`%W`’, the week according to ISO 8601 does
        *not* roll over to 1 on January 1, but keeps its last number.

    ‘`%w`’
    :   This stands for the numeric day of week (0–6). Sunday is day 0.

    ‘`%W`’
    :   This stands for the week of the year (01–52), assuming that weeks start
        on Monday. If January 1 is not a Monday, the first partial week is week
        zero.

    ‘`%x`’
    :   This has a locale-specific meaning. In the default locale (named
        ‘`C`’), it is equivalent to ‘`%D`’.

    ‘`%X`’
    :   This has a locale-specific meaning. In the default locale (named
        ‘`C`’), it is equivalent to ‘`%T`’.

    ‘`%y`’
    :   This stands for the year without century (00–99).

    ‘`%Y`’
    :   This stands for the year with century.

    ‘`%Z`’
    :   This stands for the time zone abbreviation (e.g., ‘`EST`’).

    ‘`%z`’
    :   This stands for the time zone numerical offset. The ‘`z`’ can be
        preceded by one, two, or three colons; if plain ‘`%z`’ stands for
        ‘`-0500`’, then ‘`%:z`’ stands for ‘`-05:00`’, ‘`%::z`’
        stands for ‘`-05:00:00`’, and ‘`%:::z`’ is like ‘`%::z`’
        except it suppresses trailing instances of ‘`:00`’ so it stands for
        ‘`-05`’ in the same example.

    ‘`%%`’
    :   This stands for a single ‘`%`’.

    One or more flag characters can appear immediately after the ‘`%`’.
    ‘`0`’ pads with zeros, ‘`+`’ pads with zeros and also puts
    ‘`+`’ before nonnegative year numbers with more than four digits,
    ‘`_`’ pads with blanks, ‘`-`’
    suppresses padding, ‘`^`’ upper-cases letters, and ‘`#`’
    reverses the case of letters.

    You can also specify the field width and type of padding for any of
    these ‘`%`’-sequences. This works as in `printf`: you write
    the field width as digits in a ‘`%`’-sequence, after any flags.
    For example, ‘`%S`’ specifies the number of seconds since the minute;
    ‘`%03S`’ means to pad this with zeros to 3 positions, ‘`%_3S`’ to
    pad with spaces to 3 positions. Plain ‘`%3S`’ pads with zeros,
    because that is how ‘`%S`’ normally pads to two positions.

    The characters ‘`E`’ and ‘`O`’ act as modifiers when used after
    any flags and field widths in a ‘`%`’-sequence. ‘`E`’ specifies
    using the current locale’s alternative version of the date and time.
    In a Japanese locale, for example, `%Ex` might yield a date format
    based on the Japanese Emperors’ reigns. ‘`E`’ is allowed in
    ‘`%Ec`’, ‘`%EC`’, ‘`%Ex`’, ‘`%EX`’, ‘`%Ey`’, and
    ‘`%EY`’.

    ‘`O`’ means to use the current locale’s alternative
    representation of numbers, instead of the ordinary decimal digits. This
    is allowed with most letters, all the ones that output numbers.

    To help debug programs, unrecognized ‘`%`’-sequences stand for
    themselves and are output as-is. Programs should not rely on this
    behavior, as future versions of Emacs may recognize new
    ‘`%`’-sequences as extensions.

    This function uses the C library function `strftime`
    (see [Formatting Calendar Time](https://www.gnu.org/software/libc/manual/html_mono/libc.html#Formatting-Calendar-Time) in The GNU C Library Reference
    Manual) to do most of the work. In order to communicate with that
    function, it first converts time and zone to internal form;
    the operating system limits the range of time and zone values.
    This function also encodes format-string using the coding system
    specified by `locale-coding-system` (see [Locales](#Locales)); after
    `strftime` returns the resulting string,
    this function decodes the string using that same coding
    system.

Function: **format-seconds** format-string seconds [¶](#index-format_002dseconds)
:   This function converts its argument seconds into a string of
    years, days, hours, etc., according to format-string. The
    argument format-string may contain ‘`%`’-sequences which
    control the conversion. Here is a table of what the
    ‘`%`’-sequences mean:

    ‘`%y`’

    ‘`%Y`’
    :   The integer number of 365-day years.

    ‘`%d`’

    ‘`%D`’
    :   The integer number of days.

    ‘`%h`’

    ‘`%H`’
    :   The integer number of hours.

    ‘`%m`’

    ‘`%M`’
    :   The integer number of minutes.

    ‘`%s`’

    ‘`%S`’
    :   The number of seconds. If the optional ‘`,`’ parameter is used,
        it’s a floating point number, and the number after the ‘`,`’
        specifies how many decimals to be used. ‘`%,2s`’ means “use two
        decimals”.

    ‘`%z`’
    :   Non-printing control flag. When it is used, other specifiers must be
        given in the order of decreasing size, i.e., years before days, hours
        before minutes, etc. Nothing will be produced in the result string to
        the left of ‘`%z`’ until the first non-zero conversion is
        encountered. For example, the default format used by
        `emacs-uptime` (see [emacs-uptime](#Processor-Run-Time))
        `"%Y, %D, %H, %M, %z%S"` means that the number of seconds
        will always be produced, but years, days, hours, and minutes will only
        be shown if they are non-zero.

    ‘`%x`’
    :   Non-printing control flag that works along the same lines as
        ‘`%z`’, but instead suppresses printing of trailing zero-value time
        elements.

    ‘`%%`’
    :   Produces a literal ‘`%`’.

    Upper-case format sequences produce the units in addition to the
    numbers, lower-case formats produce only the numbers.

    You can also specify the field width by following the ‘`%`’ with a
    number; shorter numbers will be padded with blanks. An optional
    period before the width requests zero-padding instead. For example,
    `"%.3Y"` might produce `"004 years"`.

---

Next: [Time Calculations](#Time-Calculations), Previous: [Parsing and Formatting Times](#Time-Parsing), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.9 Processor Run time [¶](#Processor-Run-time)

Emacs provides several functions and primitives that return time,
both elapsed and processor time, used by the Emacs process.

Command: **emacs-uptime** &optional format [¶](#index-emacs_002duptime)
:   This function returns a string representing the Emacs
    *uptime*—the elapsed wall-clock time this instance of Emacs is
    running. The string is formatted by `format-seconds` according
    to the optional argument format. For the available format
    descriptors, see [format-seconds](#Time-Parsing). If format
    is `nil` or omitted, it defaults to `"%Y, %D, %H, %M,
    %z%S"`.

    When called interactively, it prints the uptime in the echo area.

Function: **get-internal-run-time** [¶](#index-get_002dinternal_002drun_002dtime)
:   This function returns the processor run time used by Emacs, as a Lisp
    timestamp (see [Time of Day](#Time-of-Day)).

    Note that the time returned by this function excludes the time Emacs
    was not using the processor, and if the Emacs process has several
    threads, the returned value is the sum of the processor times used up
    by all Emacs threads.

    If the system doesn’t provide a way to determine the processor run
    time, `get-internal-run-time` returns the same time as
    `current-time`.

Command: **emacs-init-time** [¶](#index-emacs_002dinit_002dtime)
:   This function returns the duration of the Emacs initialization
    (see [Summary: Sequence of Actions at Startup](#Startup-Summary)) in seconds, as a string. When called
    interactively, it prints the duration in the echo area.

---

Next: [Timers for Delayed Execution](#Timers), Previous: [Processor Run time](#Processor-Run-Time), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.10 Time Calculations [¶](#Time-Calculations-1)

These functions perform calendrical computations using time values
(see [Time of Day](#Time-of-Day)). As with any time value, a value of
`nil` for any of their
time-value arguments stands for the current system time, and a finite
number stands for the number of seconds since the epoch.

Function: **time-less-p** t1 t2 [¶](#index-time_002dless_002dp)
:   This returns `t` if the time value t1 is less than the time value
    t2.

Function: **time-equal-p** t1 t2 [¶](#index-time_002dequal_002dp)
:   This returns `t` if the two time values t1 and t2 are
    equal. The result is `nil` if either argument is a NaN.
    For the purpose of comparison, a `nil` argument represents the
    current time with infinite resolution, so this function returns
    `nil` if one argument is `nil` and the other is not, and
    callers can therefore use `nil` to represent an unknown time
    value that does not equal any timestamp.

Function: **time-subtract** t1 t2 [¶](#index-time_002dsubtract)
:   This returns the time difference t1 − t2 between
    two time values, as a Lisp timestamp. The result is exact and its clock
    resolution is no worse than the worse of its two arguments’ resolutions.
    If you need the difference in units
    of elapsed seconds, you can convert it with `time-convert` or
    `float-time`. See [Time Conversion](#Time-Conversion).

Function: **time-add** t1 t2 [¶](#index-time_002dadd)
:   This returns the sum of two time values,
    using the same conversion rules as `time-subtract`.
    One argument should represent a time difference rather than a point in time,
    as a time value that is often just a single number of elapsed seconds.
    Here is how to add a number of seconds to a time value:

    ```
    (time-add time seconds)
    ```

Function: **time-to-days** time-value [¶](#index-time_002dto_002ddays)
:   This function returns the number of days between the beginning of year
    1 and time-value, assuming the default time zone.
    The operating system limits the range of time and zone values.

Function: **days-to-time** days [¶](#index-days_002dto_002dtime)
:   This is not quite the inverse of the `time-to-days` function, as
    it uses the Emacs epoch (instead of the year 1) for historical
    reasons. To get the inverse, subtract `(time-to-days 0)` from
    days, in which case `days-to-time` may return `nil` if
    days is negative.

Function: **time-to-day-in-year** time-value [¶](#index-time_002dto_002dday_002din_002dyear)
:   This returns the day number within the year corresponding to time-value,
    assuming the default time zone.
    The operating system limits the range of time and zone values.

Function: **date-leap-year-p** year [¶](#index-date_002dleap_002dyear_002dp)
:   This function returns `t` if year is a leap year.

Function: **date-days-in-month** year month [¶](#index-date_002ddays_002din_002dmonth)
:   Return the number of days in month in year. For instance,
    February 2020 has 29 days.

Function: **date-ordinal-to-time** year ordinal [¶](#index-date_002dordinal_002dto_002dtime)
:   Return the date of ordinal in year as a decoded time
    structure. For instance, the 120th day in 2004 is April 29th.

Function: **seconds-to-string** delay &optional readable abbrev precision [¶](#index-seconds_002dto_002dstring)
:   Return a string describing a given delay (in seconds). By
    default, this function formats the returned string as a floating-point
    number in units selected according to the value of delay. For
    example, a delay of 9861.5 seconds yields ‘`2.74h`’, since the value
    of delay is longer than 1 hour, but shorter than 1 day. The
    output formatting can be further controlled by the optional arguments,
    if optional argument readable is non-`nil`. If
    readable’s value is `expanded`, the returned string will
    describe delay using two units; for example, a delay of 9861.5
    seconds with readable set to the symbol `expanded` returns
    ‘`2 hours 44 minutes`’, but if readable is `t`, the
    function returns ‘`3 hours`’. Optional argument abbrev, if
    non-`nil`, means to abbreviate the units: use ‘`h`’ instead of
    ‘`hours`’, ‘`m`’ instead of ‘`minutes`’, etc. If
    precision is a whole integer number, the function rounds the value
    of the smallest unit it produces to that many digits after the decimal
    point; thus, 9861.5 with precision set to 3 yields ‘`2.739
    hours`’. If precision is a non-negative float smaller than 1, the
    function rounds to that value.

---

Next: [Idle Timers](#Idle-Timers), Previous: [Time Calculations](#Time-Calculations), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.11 Timers for Delayed Execution [¶](#Timers-for-Delayed-Execution)

You can set up a *timer* to call a function at a specified
future time or after a certain length of idleness. A timer is a
special object that stores the information about the next invocation
times and the function to invoke.

Function: **timerp** object [¶](#index-timerp)
:   This predicate function returns non-`nil` if `object` is a
    timer.

Emacs cannot run timers at any arbitrary point in a Lisp program; it
can run them only when Emacs could accept output from a subprocess:
namely, while waiting or inside certain primitive functions such as
`sit-for` or `read-event` which *can* wait. Therefore, a
timer’s execution may be delayed if Emacs is busy. However, the time of
execution is very precise if Emacs is idle.

Emacs binds `inhibit-quit` to `t` before calling the timer
function, because quitting out of many timer functions can leave
things in an inconsistent state. This is normally unproblematical
because most timer functions don’t do a lot of work. Indeed, for a
timer to call a function that takes substantial time to run is likely
to be annoying. If a timer function needs to allow quitting, it
should use `with-local-quit` (see [Quitting](#Quitting)). For example, if
a timer function calls `accept-process-output` to receive output
from an external process, that call should be wrapped inside
`with-local-quit`, to ensure that `C-g` works if the external
process hangs.

It is usually a bad idea for timer functions to alter buffer
contents. When they do, they usually should call `undo-boundary`
both before and after changing the buffer, to separate the timer’s
changes from user commands’ changes and prevent a single undo entry
from growing to be quite large.

Timer functions should also avoid calling functions that cause Emacs
to wait, such as `sit-for` (see [Waiting for Elapsed Time or Input](#Waiting)). This can lead to
unpredictable effects, since other timers (or even the same timer) can
run while waiting. If a timer function needs to perform an action
after a certain time has elapsed, it can do this by scheduling a new
timer.

If a timer function performs a remote file operation, it can be in
conflict with an already running remote file operation of the same
connection. Such conflicts are detected, and they result in a
`remote-file-error` error (see [Standard Errors](#Standard-Errors)). This should
be protected by wrapping the timer function body with

```
(ignore-error remote-file-error
  ...)
```

If a timer function calls functions that can change the match data,
it should save and restore the match data. See [Saving and Restoring the Match Data](#Saving-Match-Data).

Command: **run-at-time** time repeat function &rest args [¶](#index-run_002dat_002dtime)
:   This sets up a timer that calls the function function with
    arguments args at time time. If repeat is a number
    (integer or floating point), the timer is scheduled to run again every
    repeat seconds after time. If repeat is `nil`,
    the timer runs only once.

    time may specify an absolute or a relative time.

    Absolute times may be specified using a string with a limited variety
    of formats, and are taken to be times *today*, even if already in
    the past. The recognized forms are ‘`xxxx`’,
    ‘`x:xx`’, or ‘`xx:xx`’ (military time),
    and ‘`xxam`’, ‘`xxAM`’, ‘`xxpm`’,
    ‘`xxPM`’, ‘`xx:xxam`’,
    ‘`xx:xxAM`’, ‘`xx:xxpm`’, or
    ‘`xx:xxPM`’. A period can be used instead of a colon
    to separate the hour and minute parts.

    To specify a relative time as a string, use numbers followed by units.
    For example:

    ‘`1 min`’
    :   denotes 1 minute from now.

    ‘`1 min 5 sec`’
    :   denotes 65 seconds from now.

    ‘`1 min 2 sec 3 hour 4 day 5 week 6 fortnight 7 month 8 year`’
    :   denotes exactly 103 months, 123 days, and 10862 seconds from now.

    For relative time values, Emacs considers a month to be exactly thirty
    days, and a year to be exactly 365.25 days.

    Not all convenient formats are strings. If time is a number
    (integer or floating point), that specifies a relative time measured in
    seconds. The result of `encode-time` can also be used to specify
    an absolute value for time.

    In most cases, repeat has no effect on when *first* call
    takes place—time alone specifies that. There is one exception:
    if time is `t`, then the timer runs whenever the time is a
    multiple of repeat seconds after the epoch. This is useful for
    functions like `display-time`. For instance, the following will
    make function run at every “whole” minute (e.g.,
    ‘`11:03:00`’, ‘`11:04:00`’, etc):

    ```
    (run-at-time t 60 function)
    ```

    If Emacs didn’t get any CPU time when the timer would have run (for
    example if the system was busy running another process or if the
    computer was sleeping or in a suspended state), the timer will run as
    soon as Emacs resumes and is idle.

    The function `run-at-time` returns a timer value that identifies
    the particular scheduled future action. You can use this value to call
    `cancel-timer` (see below).

Command: **run-with-timer** secs repeat function &rest args [¶](#index-run_002dwith_002dtimer)
:   This is exactly the same as `run-at-time` (so see that definition
    for an explanation of the parameters; secs is passed as
    time to that function), but is meant to be used when the delay
    is specified in seconds.

A repeating timer nominally ought to run every repeat seconds,
but remember that any invocation of a timer can be late. Lateness of
one repetition has no effect on the scheduled time of the next
repetition. For instance, if Emacs is busy computing for long enough
to cover three scheduled repetitions of the timer, and then starts to
wait, it will immediately call the timer function three times in
immediate succession (presuming no other timers trigger before or
between them). If you want a timer to run again no less than n
seconds after the last invocation, don’t use the repeat argument.
Instead, the timer function should explicitly reschedule the timer.

User Option: **timer-max-repeats** [¶](#index-timer_002dmax_002drepeats)
:   This variable’s value specifies the maximum number of times to repeat
    calling a timer function in a row, when many previously scheduled
    calls were unavoidably delayed.

Macro: **with-timeout** (seconds timeout-forms…) body… [¶](#index-with_002dtimeout)
:   Execute body, but give up after seconds seconds. If
    body finishes before the time is up, `with-timeout` returns
    the value of the last form in body. If, however, the execution of
    body is cut short by the timeout, then `with-timeout`
    executes all the timeout-forms and returns the value of the last
    of them.

    This macro works by setting a timer to run after seconds seconds. If
    body finishes before that time, it cancels the timer. If the
    timer actually runs, it terminates execution of body, then
    executes timeout-forms.

    Since timers can run within a Lisp program only when the program calls a
    primitive that can wait, `with-timeout` cannot stop executing
    body while it is in the midst of a computation—only when it
    calls one of those primitives. So use `with-timeout` only with a
    body that waits for input, not one that does a long computation.
    Primitives that wait for input include `sit-for`, `sleep-for`,
    `accept-process-output`, and some others.

The function `y-or-n-p-with-timeout` provides a simple way to use
a timer to avoid waiting too long for an answer. See [Yes-or-No Queries](#Yes_002dor_002dNo-Queries).

Function: **cancel-timer** timer [¶](#index-cancel_002dtimer)
:   This cancels the requested action for timer, which should be a
    timer—usually, one previously returned by `run-at-time` or
    `run-with-idle-timer`. This cancels the effect of that call to
    one of these functions; the arrival of the specified time will not
    cause anything special to happen.

The `list-timers` command lists all the currently active timers.
The command `c` (`timer-list-cancel`) will cancel the timer
on the line under point. You can sort the list by column using the
command `S` (`tabulated-list-sort`).

---

Next: [Terminal Input](#Terminal-Input), Previous: [Timers for Delayed Execution](#Timers), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.12 Idle Timers [¶](#Idle-Timers-1)

Here is how to set up a timer that runs when Emacs is idle for a
certain length of time. Aside from how to set them up, idle timers
work just like ordinary timers.

Command: **run-with-idle-timer** secs repeat function &rest args [¶](#index-run_002dwith_002didle_002dtimer)
:   Set up a timer which runs the next time Emacs is idle for secs
    seconds. The value of secs may be a number or a value of the type
    returned by `current-idle-time`.

    If repeat is `nil`, the timer runs just once, the first time
    Emacs remains idle for a long enough time. More often repeat is
    non-`nil`, which means to run the timer *each time* Emacs
    remains idle for secs seconds.

    The function `run-with-idle-timer` returns a timer value which you
    can use in calling `cancel-timer` (see [Timers for Delayed Execution](#Timers)).

Emacs becomes *idle* when it starts waiting for user input
(unless it waits for input with a timeout, see [Reading One Event](#Reading-One-Event)), and
it remains idle until the user provides some input. If a timer is set
for five seconds of idleness, it runs approximately five seconds after
Emacs first becomes idle. Even if repeat is non-`nil`,
this timer will not run again as long as Emacs remains idle, because
the duration of idleness will continue to increase and will not go
down to five seconds again.

Emacs can do various things while idle: garbage collect, autosave or
handle data from a subprocess. But these interludes during idleness do
not interfere with idle timers, because they do not reset the clock of
idleness to zero. An idle timer set for 600 seconds will run when ten
minutes have elapsed since the last user command was finished, even if
subprocess output has been accepted thousands of times within those ten
minutes, and even if there have been garbage collections and autosaves.

When the user supplies input, Emacs becomes non-idle while executing the
input. Then it becomes idle again, and all the idle timers that are
set up to repeat will subsequently run another time, one by one.

Do not write an idle timer function containing a loop which does a
certain amount of processing each time around, and exits when
`(input-pending-p)` is non-`nil`. This approach seems very
natural but has two problems:

* It blocks out all process output (since Emacs accepts process output
  only while waiting).
* It blocks out any idle timers that ought to run during that time.

Similarly, do not write an idle timer function that sets up another
idle timer (including the same idle timer) with secs argument
less than or equal to the current idleness time. Such a timer will
run almost immediately, and continue running again and again, instead
of waiting for the next time Emacs becomes idle. The correct approach
is to reschedule with an appropriate increment of the current value of
the idleness time, as described below.

Function: **current-idle-time** [¶](#index-current_002didle_002dtime)
:   If Emacs is idle, this function returns the length of time Emacs has
    been idle, using the same format as
    `current-time` (see [Time of Day](#Time-of-Day)).

    When Emacs is not idle, `current-idle-time` returns `nil`.
    This is a convenient way to test whether Emacs is idle.

The main use of `current-idle-time` is when an idle timer
function wants to “take a break” for a while. It can set up another
idle timer to call the same function again, after a few seconds more
idleness. Here’s an example:

```
(defvar my-resume-timer nil
  "Timer for `my-timer-function' to reschedule itself, or nil.")

(defun my-timer-function ()
  ;; If the user types a command while my-resume-timer
  ;; is active, the next time this function is called from
  ;; its main idle timer, deactivate my-resume-timer.
  (when my-resume-timer
    (cancel-timer my-resume-timer))
  ...do the work for a while...
  (when taking-a-break
    (setq my-resume-timer
          (run-with-idle-timer
            ;; Compute an idle time break-length
            ;; more than the current value.
            (time-add (current-idle-time) break-length)
            nil
            'my-timer-function))))
```

---

Next: [Terminal Output](#Terminal-Output), Previous: [Idle Timers](#Idle-Timers), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.13 Terminal Input [¶](#Terminal-Input-1)

This section describes functions and variables for recording or
manipulating terminal input. See [Emacs Display](#Display), for related functions.

* [Input Modes](#Input-Modes)
* [Recording Input](#Recording-Input)

---

Next: [Recording Input](#Recording-Input), Up: [Terminal Input](#Terminal-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.13.1 Input Modes [¶](#Input-Modes-1)

Function: **set-input-mode** interrupt flow meta &optional quit-char [¶](#index-set_002dinput_002dmode)
:   This function sets the mode for reading keyboard input. If
    interrupt is non-`nil`, then Emacs uses input interrupts.
    If it is `nil`, then it uses CBREAK mode. The default
    setting is system-dependent. Some systems always use CBREAK mode
    regardless of what is specified.

    When Emacs communicates directly with X, it ignores this argument and
    uses interrupts if that is the way it knows how to communicate.

    If flow is non-`nil`, then Emacs uses XON/XOFF
    (`C-q`, `C-s`) flow control for output to the terminal. This
    has no effect except in CBREAK mode.

    The argument meta controls support for input character codes
    above 127. If meta is `t`, Emacs converts characters with
    the 8th bit set into Meta characters, before it decodes them as needed
    (see [Terminal I/O Encoding](#Terminal-I_002fO-Encoding)). If meta is `nil`,
    Emacs disregards the 8th bit; this is necessary when the terminal uses
    it as a parity bit. If meta is the symbol `encoded`, Emacs
    first decodes the characters using all the 8 bits of each byte, and
    then converts the decoded single-byte characters into Meta characters
    if they have their eighth bit set. Finally, if meta is neither
    `t` nor `nil` nor `encoded`, Emacs uses all 8 bits of
    input unchanged, both before and after decoding them. This is good
    for terminals that use 8-bit character sets and don’t encode the Meta
    modifier as the eighth bit.

    If quit-char is non-`nil`, it specifies the character to
    use for quitting. Normally this character is `C-g`.
    See [Quitting](#Quitting).

The `current-input-mode` function returns the input mode settings
Emacs is currently using.

Function: **current-input-mode** [¶](#index-current_002dinput_002dmode)
:   This function returns the current mode for reading keyboard input. It
    returns a list, corresponding to the arguments of `set-input-mode`,
    of the form `(interrupt flow meta quit)` in
    which:

    interrupt
    :   is non-`nil` when Emacs is using interrupt-driven input. If
        `nil`, Emacs is using CBREAK mode.

    flow
    :   is non-`nil` if Emacs uses XON/XOFF (`C-q`, `C-s`)
        flow control for output to the terminal. This value is meaningful only
        when interrupt is `nil`.

    meta
    :   is `t` if Emacs treats the eighth bit of input characters as
        the Meta bit before decoding input; `encoded` if Emacs treats the
        eighth bit of the decoded single-byte characters as the Meta bit;
        `nil` if Emacs clears the eighth bit of every input character;
        any other value means Emacs uses all eight bits as the basic character
        code.

    quit
    :   is the character Emacs currently uses for quitting, usually `C-g`.

---

Previous: [Input Modes](#Input-Modes), Up: [Terminal Input](#Terminal-Input)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 43.13.2 Recording Input [¶](#Recording-Input-1)

Function: **recent-keys** &optional include-cmds [¶](#index-recent_002dkeys)
:   This function returns a vector containing the last 300 input events from
    the keyboard or mouse. All input events are included, whether or not
    they were used as parts of key sequences. Thus, you always get the last
    300 input events, not counting events generated by keyboard macros.
    (These are excluded because they are less interesting for debugging; it
    should be enough to see the events that invoked the macros.)

    If include-cmds is non-`nil`, complete key sequences in the
    result vector are interleaved with pseudo-events of the form
    `(nil . COMMAND)`, where COMMAND is the binding of
    the key sequence (see [Command Loop Overview](#Command-Overview)).

    A call to `clear-this-command-keys` (see [Information from the Command Loop](#Command-Loop-Info))
    causes this function to return an empty vector immediately afterward.

Command: **open-dribble-file** filename [¶](#index-open_002ddribble_002dfile)
:   This function opens a *dribble file* named filename. When a
    dribble file is open, each input event from the keyboard or mouse (but
    not those from keyboard macros) is written in that file. A
    non-character event is expressed using its printed representation
    surrounded by ‘`<…>`’. Be aware that sensitive information
    (such as passwords) may end up recorded in the dribble file.

    You close the dribble file by calling this function with an argument
    of `nil`.

See also the `open-termscript` function (see [Terminal Output](#Terminal-Output)).

---

Next: [Sound Output](#Sound-Output), Previous: [Terminal Input](#Terminal-Input), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.14 Terminal Output [¶](#Terminal-Output-1)

The terminal output functions send output to a text terminal, or keep
track of output sent to the terminal. The variable `baud-rate`
tells you what Emacs thinks is the output speed of the terminal.

User Option: **baud-rate** [¶](#index-baud_002drate)
:   This variable’s value is the output speed of the terminal, as far as
    Emacs knows. Setting this variable does not change the speed of actual
    data transmission, but the value is used for calculations such as
    padding.

    It also affects decisions about whether to scroll part of the
    screen or repaint on text terminals. See [Forcing Redisplay](#Forcing-Redisplay),
    for the corresponding functionality on graphical terminals.

    The value is measured in baud.

If you are running across a network, and different parts of the
network work at different baud rates, the value returned by Emacs may be
different from the value used by your local terminal. Some network
protocols communicate the local terminal speed to the remote machine, so
that Emacs and other programs can get the proper value, but others do
not. If Emacs has the wrong value, it makes decisions that are less
than optimal. To fix the problem, set `baud-rate`.

Function: **send-string-to-terminal** string &optional terminal [¶](#index-send_002dstring_002dto_002dterminal)
:   This function sends string to terminal without alteration.
    Control characters in string have terminal-dependent effects.
    (If you need to display non-ASCII text on the terminal, encode it
    using one of the functions described in [Explicit Encoding and Decoding](#Explicit-Encoding).)
    This function operates only on text terminals. terminal may be
    a terminal object, a frame, or `nil` for the selected frame’s
    terminal. In batch mode, string is sent to `stdout` when
    terminal is `nil`.

    One use of this function is to define function keys on terminals that
    have downloadable function key definitions. For example, this is how (on
    certain terminals) to define function key 4 to move forward four
    characters (by transmitting the characters `C-u C-f` to the
    computer):

    ```
    (send-string-to-terminal "\eF4\^U\^F")
         ⇒ nil
    ```

Command: **open-termscript** filename [¶](#index-open_002dtermscript)
:   This function is used to open a *termscript file* that will record
    all the characters sent by Emacs to the terminal. It returns
    `nil`. Termscript files are useful for investigating problems
    where Emacs garbles the screen, problems that are due to incorrect
    Termcap entries or to undesirable settings of terminal options more
    often than to actual Emacs bugs. Once you are certain which characters
    were actually output, you can determine reliably whether they correspond
    to the Termcap specifications in use.

    ```
    (open-termscript "../junk/termscript")
         ⇒ nil
    ```

    You close the termscript file by calling this function with an
    argument of `nil`.

    See also `open-dribble-file` in [Recording Input](#Recording-Input).

---

Next: [Operating on X11 Keysyms](#X11-Keysyms), Previous: [Terminal Output](#Terminal-Output), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.15 Sound Output [¶](#Sound-Output-1)

To play sound using Emacs, use the function `play-sound`. Only
certain systems are supported; if you call `play-sound` on a
system which cannot really do the job, it gives an error.

The sound must be stored as a file in RIFF-WAVE format (‘`.wav`’)
or Sun Audio format (‘`.au`’).

Function: **play-sound** sound [¶](#index-play_002dsound)
:   This function plays a specified sound. The argument, sound, has
    the form `(sound properties...)`, where the properties
    consist of alternating keywords (particular symbols recognized
    specially) and values corresponding to them.

    Here is a table of the keywords that are currently meaningful in
    sound, and their meanings:

    `:file file`
    :   This specifies the file containing the sound to play.
        If the file name is not absolute, it is expanded against
        the directory `data-directory`.

    `:data data`
    :   This specifies the sound to play without need to refer to a file. The
        value, data, should be a string containing the same bytes as a
        sound file. We recommend using a unibyte string.

    `:volume volume`
    :   This specifies how loud to play the sound. It should be a number in the
        range of 0 to 1. The default is to use whatever volume has been
        specified before.

    `:device device`
    :   This specifies the system device on which to play the sound, as a
        string. The default device is system-dependent.

    Before actually playing the sound, `play-sound`
    calls the functions in the list `play-sound-functions`.
    Each function is called with one argument, sound.

Command: **play-sound-file** file &optional volume device [¶](#index-play_002dsound_002dfile)
:   This function is an alternative interface to playing a sound file
    specifying an optional volume and device.

Variable: **play-sound-functions** [¶](#index-play_002dsound_002dfunctions)
:   A list of functions to be called before playing a sound. Each function
    is called with one argument, a property list that describes the sound.

---

Next: [Batch Mode](#Batch-Mode), Previous: [Sound Output](#Sound-Output), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.16 Operating on X11 Keysyms [¶](#Operating-on-X11-Keysyms)

To define system-specific X11 keysyms, set the variable
`system-key-alist`.

Variable: **system-key-alist** [¶](#index-system_002dkey_002dalist)
:   This variable’s value should be an alist with one element for each
    system-specific keysym. Each element has the form `(code
    . symbol)`, where code is the numeric keysym code (not
    including the vendor-specific bit,
    −2\*\*28),
    and symbol is the name for the function key.

    For example `(168 . mute-acute)` defines a system-specific key (used
    by HP X servers) whose numeric code is
    −2\*\*28
    + 168.

    It is not crucial to exclude from the alist the keysyms of other X
    servers; those do no harm, as long as they don’t conflict with the ones
    used by the X server actually in use.

    The variable is always local to the current terminal, and cannot be
    buffer-local. See [Multiple Terminals](#Multiple-Terminals).

You can specify which keysyms Emacs should use for the Control, Meta,
Alt, Hyper, and Super modifiers by setting these variables:

Variable: **x-ctrl-keysym** [¶](#index-x_002dctrl_002dkeysym)

Variable: **x-alt-keysym** [¶](#index-x_002dalt_002dkeysym)

Variable: **x-meta-keysym** [¶](#index-x_002dmeta_002dkeysym)

Variable: **x-hyper-keysym** [¶](#index-x_002dhyper_002dkeysym)

Variable: **x-super-keysym** [¶](#index-x_002dsuper_002dkeysym)
:   The name of the keysym that should stand for the Control modifier
    (respectively, for Alt, Meta, Hyper, and Super). For example, here is
    how to swap the Meta and Alt modifiers within Emacs:

    ```
    (setq x-alt-keysym 'meta)
    (setq x-meta-keysym 'alt)
    ```

---

Next: [Session Management](#Session-Management), Previous: [Operating on X11 Keysyms](#X11-Keysyms), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.17 Batch Mode [¶](#Batch-Mode-1)

The command-line option ‘`-batch`’ causes Emacs to run
noninteractively. In this mode, Emacs does not read commands from the
terminal, it does not alter the terminal modes, and it does not expect
to be outputting to an erasable screen. The idea is that you specify
Lisp programs to run; when they are finished, Emacs should exit. The
way to specify the programs to run is with ‘`-l file`’, which
loads the library named file, or ‘`-f function`’, which
calls function with no arguments, or ‘`--eval=form`’.

Variable: **noninteractive** [¶](#index-noninteractive)
:   This variable is non-`nil` when Emacs is running in batch mode.

If the specified Lisp program signals an unhandled error in batch
mode, Emacs exits with a non-zero exit status after invoking the Lisp
debugger which shows the Lisp backtrace (see [Invoking the Debugger](#Invoking-the-Debugger)) on the standard error stream:

```
$ emacs -Q --batch --eval '(error "foo")'; echo $?
```

```
Error: error ("foo")
mapbacktrace(#f(compiled-function (evald func args flags) #<bytecode -0x4f85c5
7c45e2f81>))
debug-early-backtrace()
debug-early(error (error "foo"))
signal(error ("foo"))
error("foo")
eval((error "foo") t)
command-line-1(("--eval" "(error \"foo\")"))
command-line()
normal-top-level()
```

```
foo
255
```

Any Lisp program output that would normally go to the echo area,
either using `message`, or using `prin1`, etc., with
`t` as the stream (see [Output Streams](#Output-Streams)), goes instead to
Emacs’s standard descriptors when in batch mode: `message` writes
to the standard error descriptor, while `prin1` and other print
functions write to the standard output. Similarly, input that would
normally come from the minibuffer is read from the standard input
descriptor. Thus, Emacs behaves much like a noninteractive
application program. (The echo area output that Emacs itself normally
generates, such as command echoing, is suppressed entirely.)

Non-ASCII text written to the standard output or error descriptors is
by default encoded using `locale-coding-system` (see [Locales](#Locales))
if it is non-`nil`; this can be overridden by binding
`coding-system-for-write` to a coding system of you choice
(see [Explicit Encoding and Decoding](#Explicit-Encoding)).

In batch mode, Emacs will enlarge the value of the
`gc-cons-percentage` variable from the default of ‘`0.1`’ up to
‘`1.0`’. Batch jobs that are supposed to run for a long time
should adjust the limit back down again, because this means that less
garbage collection will be performed by default (and more memory
consumed).

---

Next: [Desktop Notifications](#Desktop-Notifications), Previous: [Batch Mode](#Batch-Mode), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.18 Session Management [¶](#Session-Management-1)

Emacs supports the X Session Management Protocol, which is used to
suspend and restart applications. In the X Window System, a program
called the *session manager* is responsible for keeping track of
the applications that are running. When the X server shuts down, the
session manager asks applications to save their state, and delays the
actual shutdown until they respond. An application can also cancel
the shutdown.

When the session manager restarts a suspended session, it directs
these applications to individually reload their saved state. It does
this by specifying a special command-line argument that says what
saved session to restore. For Emacs, this argument is ‘`--smid
session`’.

Variable: **emacs-save-session-functions** [¶](#index-emacs_002dsave_002dsession_002dfunctions)
:   Emacs supports saving state via a hook called
    `emacs-save-session-functions`. Emacs runs this hook when the
    session manager tells it that the window system is shutting down. The
    functions are called with no arguments, and with the current buffer
    set to a temporary buffer. Each function can use `insert` to add
    Lisp code to this buffer. At the end, Emacs saves the buffer in a
    file, called the *session file*.

    Subsequently, when the session manager restarts Emacs, it loads the
    session file automatically (see [Loading](#Loading)). This is performed by a
    function named `emacs-session-restore`, which is called during
    startup. See [Summary: Sequence of Actions at Startup](#Startup-Summary).

    If a function in `emacs-save-session-functions` returns
    non-`nil`, Emacs tells the session manager to cancel the
    shutdown.

Here is an example that just inserts some text into `*scratch*` when
Emacs is restarted by the session manager.

```
(add-hook 'emacs-save-session-functions 'save-yourself-test)
```

```

```

```
(defun save-yourself-test ()
  (insert
   (format "%S" '(with-current-buffer "*scratch*"
                   (insert "I am restored"))))
  nil)
```

---

Next: [Controlling System GUI Taskbar Features](#System-Taskbar), Previous: [Session Management](#Session-Management), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.19 Desktop Notifications [¶](#Desktop-Notifications-1)

Emacs is able to send *notifications* on systems that support the
freedesktop.org Desktop Notifications Specification, MS-Windows,
Haiku, and Android.

In order to use this functionality on POSIX hosts, Emacs must have
been compiled with D-Bus support, and the `notifications` library
must be loaded. See [D-Bus](https://www.gnu.org/software/emacs/manual/html_mono/dbus.html#Top) in D-Bus integration in Emacs.
The following function is supported when D-Bus support is available:

Function: **notifications-notify** &rest params [¶](#index-notifications_002dnotify)
:   This function sends a notification to the desktop via D-Bus,
    consisting of the parameters specified by the params arguments.
    These arguments should consist of alternating keyword and value pairs.
    The supported keywords and values are as follows:

    `:bus bus`
    :   The D-Bus bus. This argument is needed only if a bus other than
        `:session` shall be used.

    `:title title`
    :   The notification title.

    `:body text`
    :   The notification body text. Depending on the implementation of the
        notification server, the text could contain HTML markups, like
        ‘`"<b>bold text</b>"`’, hyperlinks, or images. Special HTML
        characters must be encoded, as ‘`"Contact
        &lt;postmaster@localhost&gt;!"`’.

    `:app-name name`
    :   The name of the application sending the notification. The default is
        `notifications-application-name`.

    `:replaces-id id`
    :   The notification id that this notification replaces. id
        must be the result of a previous `notifications-notify` call.

    `:app-icon icon-file`
    :   The file name of the notification icon. If set to `nil`, no icon
        is displayed. The default is `notifications-application-icon`.
        If the value is a string, the function interprets it as a file name
        and converts to absolute by using `expand-file-name`; if it is a
        symbol, the function will use its name (which is useful when using the
        Icon Naming Specification [36](#FOOT36)).

    `:actions (key title key title ...)`
    :   A list of actions to be applied. key and title are both
        strings. The default action (usually invoked by clicking the
        notification) should have a key named ‘`"default"`’. The title can
        be anything, though implementations are free not to display it.

    `:timeout timeout`
    :   The timeout time in milliseconds since the display of the notification
        at which the notification should automatically close. If −1, the
        notification’s expiration time is dependent on the notification
        server’s settings, and may vary for the type of notification. If 0,
        the notification never expires. Default value is −1.

    `:urgency urgency`
    :   The urgency level. It can be `low`, `normal`, or `critical`.

    `:action-items`
    :   When this keyword is given, the title string of the actions is
        interpreted as icon name.

    `:category category`
    :   The type of notification this is, a string. See the
        [Desktop Notifications Specification](https://specifications.freedesktop.org/notification-spec/notification-spec-latest.html#categories) for a list of standard
        categories.

    `:desktop-entry filename`
    :   This specifies the name of the desktop filename representing the
        calling program, like ‘`"emacs"`’.

    `:image-data (width height rowstride has-alpha bits channels data)`
    :   This is a raw data image format that describes the width, height,
        rowstride, whether there is an alpha channel, bits per sample,
        channels and image data, respectively.

    `:image-path path`
    :   This is represented either as a URI (‘`file://`’ is the only URI
        schema supported right now) or a name in a freedesktop.org-compliant
        icon theme from ‘`$XDG_DATA_DIRS/icons`’.

    `:sound-file filename`
    :   The path to a sound file to play when the notification pops up.

    `:sound-name name`
    :   A themable named sound from the freedesktop.org sound naming
        specification from ‘`$XDG_DATA_DIRS/sounds`’, to play when the
        notification pops up. Similar to the icon name, only for sounds. An
        example would be ‘`"message-new-instant"`’.

    `:suppress-sound`
    :   Causes the server to suppress playing any sounds, if it has that
        ability.

    `:resident`
    :   When set the server will not automatically remove the notification
        when an action has been invoked. The notification will remain resident
        in the server until it is explicitly removed by the user or by the
        sender. This hint is likely only useful when the server has the
        `:persistence` capability.

    `:transient`
    :   When set the server will treat the notification as transient and
        by-pass the server’s persistence capability, if it should exist.

    `:x position`

    `:y position`
    :   Specifies the X, Y location on the screen that the
        notification should point to. Both arguments must be used together.

    `:on-action function`
    :   Function to call when an action is invoked. The notification id
        and the key of the action are passed as arguments to the
        function.

    `:on-close function`
    :   Function to call when the notification has been closed by timeout or
        by the user. The function receive the notification id and the closing
        reason as arguments:

        * `expired` if the notification has expired
        * `dismissed` if the notification was dismissed by the user
        * `close-notification` if the notification was closed by a call to
          `notifications-close-notification`
        * `undefined` if the notification server hasn’t provided a reason

    Which parameters are accepted by the notification server can be
    checked via `notifications-get-capabilities`.

    This function returns a notification id, an integer, which can be used
    to manipulate the notification item with
    `notifications-close-notification` or the `:replaces-id`
    argument of another `notifications-notify` call. For example:

    ```
    (defun my-on-action-function (id key)
      (message "Message %d, key \"%s\" pressed" id key))
         ⇒ my-on-action-function
    ```

    ```

    ```

    ```
    (defun my-on-close-function (id reason)
      (message "Message %d, closed due to \"%s\"" id reason))
         ⇒ my-on-close-function
    ```

    ```

    ```

    ```
    (notifications-notify
     :title "Title"
     :body "This is <b>important</b>."
     :actions '("Confirm" "I agree" "Refuse" "I disagree")
     :on-action 'my-on-action-function
     :on-close 'my-on-close-function)
         ⇒ 22
    ```

    ```

    ```

    ```
    A message window opens on the desktop.  Press ``I agree''.
         ⇒ Message 22, key "Confirm" pressed
            Message 22, closed due to "dismissed"
    ```

Function: **notifications-close-notification** id &optional bus [¶](#index-notifications_002dclose_002dnotification)
:   This function closes a notification with identifier id.
    bus can be a string denoting a D-Bus connection, the default is
    `:session`.

Function: **notifications-get-capabilities** &optional bus [¶](#index-notifications_002dget_002dcapabilities)
:   Returns the capabilities of the notification server, a list of
    symbols. bus can be a string denoting a D-Bus connection, the
    default is `:session`. The following capabilities can be
    expected:

    `:actions`
    :   The server will provide the specified actions to the user.

    `:body`
    :   Supports body text.

    `:body-hyperlinks`
    :   The server supports hyperlinks in the notifications.

    `:body-images`
    :   The server supports images in the notifications.

    `:body-markup`
    :   Supports markup in the body text.

    `:icon-multi`
    :   The server will render an animation of all the frames in a given image
        array.

    `:icon-static`
    :   Supports display of exactly 1 frame of any given image array. This
        value is mutually exclusive with `:icon-multi`.

    `:persistence`
    :   The server supports persistence of notifications.

    `:sound`
    :   The server supports sounds on notifications.

    Further vendor-specific caps start with `:x-vendor`, like
    `:x-gnome-foo-cap`.

Function: **notifications-get-server-information** &optional bus [¶](#index-notifications_002dget_002dserver_002dinformation)
:   Return information on the notification server, a list of strings.
    bus can be a string denoting a D-Bus connection, the default is
    `:session`. The returned list is `(name vendor
    version spec-version)`.

    name
    :   The product name of the server.

    vendor
    :   The vendor name. For example, ‘`"KDE"`’, ‘`"GNOME"`’.

    version
    :   The server’s version number.

    spec-version
    :   The specification version the server is compliant with.

    If spec\_version is `nil`, the server supports a
    specification prior to ‘`"1.0"`’.

When Emacs runs on MS-Windows as a GUI session, it supports a small
subset of the D-Bus notifications functionality via a native
primitive:

Function: **w32-notification-notify** &rest params [¶](#index-w32_002dnotification_002dnotify)
:   This function displays an MS-Windows tray notification as specified by
    params. MS-Windows tray notifications are displayed in a
    balloon from an icon in the notification area of the taskbar.

    Value is the integer unique ID of the notification that can be used to
    remove the notification using `w32-notification-close`, described
    below. If the function fails, the return value is `nil`.

    The arguments params are specified as keyword/value pairs. All the
    parameters are optional, but if no parameters are specified, the
    function will do nothing and return `nil`.

    The following parameters are supported:

    `:icon icon`
    :   Display icon in the system tray. If icon is a string, it
        should specify a file name from which to load the icon; the specified
        file should be a `.ico` Windows icon file. If icon is not
        a string, or if this parameter is not specified, the standard Emacs
        icon will be used.

    `:tip tip`
    :   Use tip as the tooltip for the notification. If tip is a
        string, this is the text of a tooltip that will be shown when the
        mouse pointer hovers over the tray icon added by the notification. If
        tip is not a string, or if this parameter is not specified, the
        default tooltip text is ‘`Emacs notification`’. The tooltip text can
        be up to 127 characters long (63 on Windows versions before W2K).
        Longer strings will be truncated.

    `:level level`
    :   Notification severity level, one of `info`, `warning`, or
        `error`. If given, the value determines the icon displayed to the
        left of the notification title, but only if the `:title` parameter
        (see below) is also specified and is a string.

    `:title title`
    :   The title of the notification. If title is a string, it is
        displayed in a larger font immediately above the body text. The title
        text can be up to 63 characters long; longer text will be truncated.

    `:body body`
    :   The body of the notification. If body is a string, it specifies
        the text of the notification message. Use embedded newlines to
        control how the text is broken into lines. The body text can be up to
        255 characters long, and will be truncated if it’s longer. Unlike
        with D-Bus, the body text should be plain text, with no markup.

    Note that versions of Windows before W2K support only `:icon` and
    `:tip`. The other parameters can be passed, but they will be
    ignored on those old systems.

    There can be at most one active notification at any given time for each
    Emacs frame. An active notification must be removed by calling
    `w32-notification-close` before a new one can be shown for the same
    frame.

To remove the notification and its icon from the taskbar, use the
following function:

Function: **w32-notification-close** id [¶](#index-w32_002dnotification_002dclose)
:   This function removes the tray notification given by its unique
    id. The same Emacs frame which was the selected frame when the
    notification was created must be selected when removing the
    notification. A Lisp program which shows notifications must track which
    frame corresponds to which notification id, and select that frame
    when removing the notification. (When a frame is deleted, a
    notification it created, if any, will be removed by MS-Windows.)

When Emacs runs under Haiku as a GUI program, it is also provides a
restricted pastiche of the D-Bus desktop notifications interface
previously addressed. The principle capabilities absent from the
function detailed below are call-back functions such as
`:actions`, `:on-action` and `:on-close`.

Function: **haiku-notifications-notify** &rest params [¶](#index-haiku_002dnotifications_002dnotify)
:   This function sends a notification to the desktop notification server,
    incorporating a number of parameters that are akin to some of those
    accepted by `notifications-notify`. The parameters are:

    `:title title`

    `:body body`

    `:replaces-id replaces-id`

    `:urgency urgency`
    :   These have the same meaning as they do when used in calls to
        `notifications-notify`.

    `:app-icon app-icon`
    :   This should be the file name designating an image file to use as the
        icon for the notification displayed. If `nil`, the icon
        presented will instead be Emacs’s app icon.

    Its return value is a number identifying the notification, which can
    be exploited as the `:replaces-id` parameter to a subsequent call
    to this function.

When Emacs is built as an Android application package, displaying
notifications is facilitated by the function
`android-notifications-notify`. This function does not feature
call-backs, and has several idiosyncrasies, when compared to
`notifications-notify`.

Function: **android-notifications-notify** &rest params [¶](#index-android_002dnotifications_002dnotify)
:   This function displays a desktop notification. params is a list
    of parameters analogous to its namesake in
    `notifications-notify`. The parameters are:

    `:title title`

    `:body body`

    `:replaces-id replaces-id`

    `:on-action on-action`

    `:on-cancel on-close`

    `:actions actions`

    `:timeout timeout`

    `:resident resident`
    :   These have the same meaning as they do when used in calls to
        `notifications-notify`, except that no more than three non-default
        actions will be displayed.

    `:urgency urgency`
    :   The set of accepted values for urgency is the same as with
        `notifications-notify`, but the urgency applies to all
        notifications displayed with the defined group, except under
        Android 7.1 and earlier.

    `:group group`
    :   group is a string that designates a category to which the
        notification sent will belong. This category is reproduced within the
        system’s notification settings menus, but is ignored under Android 7.1
        and earlier.

        If group is `nil` or not present within params, it is
        replaced by the string ‘`"Desktop Notifications"`’.

        Callers should provide one stable combination of urgency and
        group for each kind of notification they send, given that the
        system may elect to disregard urgency if it does not match that
        of any notification previously delivered to group.

    `:icon icon`
    :   This parameter controls the symbolic icon the notification will be
        displayed with. Its value is a string designating an icon within the
        `android.R.drawable` system package. See
        [R.drawable
        | Android Developers](https://developer.android.com/reference/android/R.drawable) for a list of such icons.

        If it is not provided within params or icon does not
        exist, it defaults to ‘`"ic_dialog_alert"`’.

    It returns a number identifying the notification, which may be
    supplied as the `:replaces-id` parameter to a later call to this
    function.

    If Emacs is not afforded the permission to display notifications
    (see [Android Environment](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Android-Environment) in The GNU Emacs Manual) under
    Android 13 and later, any notifications sent will be silently
    disregarded.

---

Next: [Block System Sleep and Process Sleep Events](#System-Sleep), Previous: [Desktop Notifications](#Desktop-Notifications), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.20 Controlling System GUI Taskbar Features [¶](#Controlling-System-GUI-Taskbar-Features)

See [System Taskbar](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#System-Taskbar) in The GNU Emacs Manual, for an overview
and configuration.

Function: **system-taskbar-badge** &optional count [¶](#index-system_002dtaskbar_002dbadge)
:   This function displays count as an overlay on the system taskbar
    Emacs icon.

    If count is an integer, display that.

    If count is a string on back ends that support strings, display
    that. The string should be short.

    On back ends which do not support strings, convert count to an
    integer, or `nil` if that fails.

    If count is `nil` or an empty string, remove the counter or
    short string.

    Display the system taskbar icon badge set to count. If
    count is `nil`, clear the badge. count is typically an
    integer.

    If count is a string, it is converted to an integer on systems
    that do not support string badges, such as GNU/Linux D-Bus, and the
    badge will be cleared if the string is an invalid integer
    representation. On systems that support strings, such as macOS/GNUstep
    and MS-Windows, the badge is set to the string and displayed, and may be
    truncated to fit the visual space allocated by the system. In any case,
    if the string is empty, clear the badge.

Function: **system-taskbar-attention** &optional urgency timeout [¶](#index-system_002dtaskbar_002dattention)
:   This function flashes or bounces system taskbar Emacs icon and/or its
    frame to alert the user.

    urgency can be one of the symbols `informational`, or
    `critical`.

    If urgency is `nil`, clear the attention indicator.

    The attention indicator is cleared by the earliest of bringing the Emacs
    GUI into focus, or after timeout seconds. If timeout is
    `nil`, the system GUI behavior has priority.

    On some back ends, `critical` has the same effect as
    `informational`.

    On some back ends, attention will be displayed
    only if Emacs is not the currently focused application.

Function: **system-taskbar-progress** &optional progress [¶](#index-system_002dtaskbar_002dprogress)
:   This function displays a progress indicator overlay on the system
    taskbar Emacs icon.

    progress is a float in the range 0.0 to 1.0. If progress is
    `nil`, remove the progress indicator.

    It is convenient to use the built-in progress reporter functions which,
    when `system-taskbar-mode` is enabled, integrate with
    `system-taskbar-progress` by default. See [Reporting Operation Progress](#Progress).

Examples of system taskbar functions:

```
;; Enable and initialize system-taskbar-mode before calling its
;; package functions.
(system-taskbar-mode)

;; Display a badge integer on the taskbar icon.
(system-taskbar-badge emacs-major-version)

;; A string representation of an integer is converted to an
;; integer on GNU/Linux.
(system-taskbar-badge "31")

;; Short strings are displayed on macOS/GNUstep and MS-Windows.
(system-taskbar-badge "Test")

;; Clear the badge.
(system-taskbar-badge)
```

```

```

```
;; Get the user's attention and clear the request after 3 seconds.
(system-taskbar-attention 'informational 3)

;; Get the user's attention and clear when Emacs is focused.
(system-taskbar-attention 'critical)

;; Clear the attention request.
(system-taskbar-attention)
```

```

```

```
;; Make sure system taskbar is integrated with progress-reporter.
(setopt system-taskbar-use-progress-reporter t) ; t is the default
(system-taskbar-mode)

;; Report `dotimes` progress on the taskbar icon.
(dotimes-with-progress-reporter
    (i 10)
    "Counting from 1 to 10..."
  (sleep-for 1))

;; Report `dolist` progress on the taskbar icon.
(dolist-with-progress-reporter
    (i (make-list 10 t))
    "Progress from 1 to 10 elements..."
  (sleep-for 1))
```

---

Next: [Notifications on File Changes](#File-Notifications), Previous: [Controlling System GUI Taskbar Features](#System-Taskbar), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 43.21 Block System Sleep and Process Sleep Events [¶](#Block-System-Sleep-and-Process-Sleep-Events)

Function: **system-sleep-block-sleep** &optional why allow-display-sleep [¶](#index-system_002dsleep_002dblock_002dsleep)
:   This function blocks the system from entering its idle sleep state.

    It returns a token that must be passed to
    `system-sleep-unblock-sleep` to unblock this specific block (other
    sleep blocks may be simultaneously in force for other purposes).
    Otherwise, it returns `nil` if the sleep blocking fails.

    why is a string and, when non-`nil`, is used to identify the
    sleep block as it may appear on the system’s inspectable block lists.
    It defaults to ‘`Emacs`’.

    If allow-display-sleep is non-`nil`, allow the display to
    sleep. By default, the display is kept active.

    Note that when the Emacs process terminates, blocks are released on all
    platforms.

Function: **system-sleep-unblock-sleep** token [¶](#index-system_002dsleep_002dunblock_002dsleep)
:   This function unblocks the sleep block associated with token. It
    returns non-`nil` on success, otherwise it returns `nil`.

Macro: **with-system-sleep-block** (&optional why allow-display-sleep) body… [¶](#index-with_002dsystem_002dsleep_002dblock)
:   This is a convenience macro that lets you wrap the forms in body
    with a sleep block that is unblocked for you when body completes.
    This guarantees that the system will never go to sleep while body
    executes. The arguments have the same meaning as in
    `system-sleep-block-sleep`, above.

Function: **system-sleep-sleep-blocked-p** [¶](#index-system_002dsleep_002dsleep_002dblocked_002dp)
:   This predicate function returns non-`nil` if there are any
    active `system-sleep` blocks, otherwise it returns `nil`.

Function: **system-sleep-unblock-all-sleep-blocks** [¶](#index-system_002dsleep_002dunblock_002dall_002dsleep_002dblocks)
:   This function unblocks all active sleep blocks. It is unlikely that you
    will need to call this function.

User Option: **system-sleep-event-functions** [¶](#index-system_002dsleep_002devent_002dfunctions)
:   When the system is about to enter a sleep state or after it wakes from
    one, each function on this abnormal hook is called with one argument,
    event, a sleep event. Its state can be retrieved via
    ‘`(sleep-event-state event)`’. State will be one of the
    symbols `pre-sleep` or `post-wake`.

    Handling `pre-sleep` events should be done as fast as possible and
    avoid user prompting. Systems often grant a very short pre-sleep
    processing interval, typically ranging between 2 and 5 seconds. The
    system may sleep even if your processing is not complete, so be sure you
    do as little as possible. For example, your function could close active
    connections or serial ports.

    Handling `post-wake` events offers more leeway. Use this, for
    example, to reestablish connections.

    Note: Your code, or the functions it calls, should not raise any signals
    or all hooks will be halted. You can wrap your code in a
    `condition-case` block (see [Errors](#Errors)).

---

Next: [Dynamically Loaded Libraries](#Dynamic-Libraries), Previous: [Block System Sleep and Process Sleep Events](#System-Sleep), Up: [Operating System Interface](#System-Interface)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
