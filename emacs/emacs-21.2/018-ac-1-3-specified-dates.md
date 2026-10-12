### AC.1.3 Specified Dates

Calendar mode provides commands for moving to a particular date
specified in various ways.

`g d`: Move point to specified date (`calendar-goto-date`). `o`: Center calendar around specified month (`calendar-other-month`). `.`: Move point to today's date (`calendar-goto-today`).

`g d` (`calendar-goto-date`) prompts for a year, a month, and a day
of the month, and then moves to that date. Because the calendar includes all
dates from the beginning of the current era, you must type the year in its
entirety; that is, type `` `1990' ``, not `` `90' ``.

`o` (`calendar-other-month`) prompts for a month and year,
then centers the three-month calendar around that month.

You can return to today's date with `.`
(`calendar-goto-today`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC416) ] | [ [>](emacs.html#SEC418) ] | [ [<<](emacs.html#SEC418) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.2 Scrolling in the Calendar

The calendar display scrolls automatically through time when you
move out of the visible portion. You can also scroll it manually.
Imagine that the calendar window contains a long strip of paper with
the months on it. Scrolling the calendar means moving the strip
horizontally, so that new months become visible in the window.

`C-x <`: Scroll calendar one month forward (`scroll-calendar-left`). `C-x >`: Scroll calendar one month backward (`scroll-calendar-right`). `C-v`: `NEXT`: Scroll calendar three months forward (`scroll-calendar-left-three-months`). `M-v`: `PRIOR`: Scroll calendar three months backward (`scroll-calendar-right-three-months`).

The most basic calendar scroll commands scroll by one month at a
time. This means that there are two months of overlap between the
display before the command and the display after. `C-x <` scrolls
the calendar contents one month to the left; that is, it moves the
display forward in time. `C-x >` scrolls the contents to the
right, which moves backwards in time.

The commands `C-v` and `M-v` scroll the calendar by an entire
"screenful"---three months--in analogy with the usual meaning of
these commands. `C-v` makes later dates visible and `M-v` makes
earlier dates visible. These commands take a numeric argument as a
repeat count; in particular, since `C-u` multiplies the next command
by four, typing `C-u C-v` scrolls the calendar forward by a year and
typing `C-u M-v` scrolls the calendar backward by a year.

The function keys `NEXT` and `PRIOR` are equivalent to
`C-v` and `M-v`, just as they are in other modes.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC417) ] | [ [>](emacs.html#SEC419) ] | [ [<<](emacs.html#SEC419) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.3 Counting Days

`M-=`: Display the number of days in the current region (`calendar-count-days-region`).

To determine the number of days in the region, type `M-=`
(`calendar-count-days-region`). The numbers of days shown is
*inclusive*; that is, it includes the days specified by mark and
point.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC418) ] | [ [>](emacs.html#SEC420) ] | [ [<<](emacs.html#SEC420) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.4 Miscellaneous Calendar Commands

`p d`: Display day-in-year (`calendar-print-day-of-year`). `C-c C-l`: Regenerate the calendar window (`redraw-calendar`). `SPC`: Scroll the next window (`scroll-other-window`). `q`: Exit from calendar (`exit-calendar`).

To display the number of days elapsed since the start of the year, or
the number of days remaining in the year, type the `p d` command
(`calendar-print-day-of-year`). This displays both of those
numbers in the echo area. The number of days elapsed includes the
selected date. The number of days remaining does not include that
date.

If the calendar window text gets corrupted, type `C-c C-l`
(`redraw-calendar`) to redraw it. (This can only happen if you use
non-Calendar-mode editing commands.)

In Calendar mode, you can use `SPC` (`scroll-other-window`)
to scroll the other window. This is handy when you display a list of
holidays or diary entries in another window.

To exit from the calendar, type `q` (`exit-calendar`). This
buries all buffers related to the calendar, selecting other buffers.
(If a frame contains a dedicated calendar window, exiting from the
calendar iconifies that frame.)

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC419) ] | [ [>](emacs.html#SEC421) ] | [ [<<](emacs.html#SEC421) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.5 LaTeX Calendar

The Calendar LaTeX commands produce a buffer of LaTeX code that
prints as a calendar. Depending on the command you use, the printed
calendar covers the day, week, month or year that point is in.

`t m`: Generate a one-month calendar (`cal-tex-cursor-month`). `t M`: Generate a sideways-printing one-month calendar (`cal-tex-cursor-month-landscape`). `t d`: Generate a one-day calendar (`cal-tex-cursor-day`). `t w 1`: Generate a one-page calendar for one week (`cal-tex-cursor-week`). `t w 2`: Generate a two-page calendar for one week (`cal-tex-cursor-week2`). `t w 3`: Generate an ISO-style calendar for one week (`cal-tex-cursor-week-iso`). `t w 4`: Generate a calendar for one Monday-starting week (`cal-tex-cursor-week-monday`). `t f w`: Generate a Filofax-style two-weeks-at-a-glance calendar (`cal-tex-cursor-filofax-2week`). `t f W`: Generate a Filofax-style one-week-at-a-glance calendar (`cal-tex-cursor-filofax-week`). `t y`: Generate a calendar for one year (`cal-tex-cursor-year`). `t Y`: Generate a sideways-printing calendar for one year (`cal-tex-cursor-year-landscape`). `t f y`: Generate a Filofax-style calendar for one year (`cal-tex-cursor-filofax-year`).

Some of these commands print the calendar sideways (in "landscape
mode"), so it can be wider than it is long. Some of them use Filofax
paper size (3.75in x 6.75in). All of these commands accept a prefix
argument which specifies how many days, weeks, months or years to print
(starting always with the selected one).

If the variable `cal-tex-holidays` is non-`nil` (the default),
then the printed calendars show the holidays in `calendar-holidays`.
If the variable `cal-tex-diary` is non-`nil` (the default is
`nil`), diary entries are included also (in weekly and monthly
calendars only). If the variable `cal-tex-rules` is non-`nil`
(the default is `nil`), the calendar displays ruled pages
in styles that have sufficient room.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC420) ] | [ [>](emacs.html#SEC422) ] | [ [<<](emacs.html#SEC422) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.6 Holidays

The Emacs calendar knows about all major and many minor holidays,
and can display them.

`h`: Display holidays for the selected date (`calendar-cursor-holidays`). `Mouse-2 Holidays`: Display any holidays for the date you click on. `x`: Mark holidays in the calendar window (`mark-calendar-holidays`). `u`: Unmark calendar window (`calendar-unmark`). `a`: List all holidays for the displayed three months in another window (`list-calendar-holidays`). `M-x holidays`: List all holidays for three months around today's date in another window. `M-x list-holidays`: List holidays in another window for a specified range of years.

To see if any holidays fall on a given date, position point on that
date in the calendar window and use the `h` command. Alternatively,
click on that date with `Mouse-2` and then choose `Holidays`
from the menu that appears. Either way, this displays the holidays for
that date, in the echo area if they fit there, otherwise in a separate
window.

To view the distribution of holidays for all the dates shown in the
calendar, use the `x` command. This displays the dates that are
holidays in a different face (or places a `` `*' `` after these dates, if
display with multiple faces is not available). The command applies both
to the currently visible months and to other months that subsequently
become visible by scrolling. To turn marking off and erase the current
marks, type `u`, which also erases any diary marks (see section [AC.10 The Diary](emacs.html#SEC429)).

To get even more detailed information, use the `a` command, which
displays a separate buffer containing a list of all holidays in the
current three-month range. You can use `SPC` in the calendar window
to scroll that list.

The command `M-x holidays` displays the list of holidays for the
current month and the preceding and succeeding months; this works even
if you don't have a calendar window. If you want the list of holidays
centered around a different month, use `C-u M-x holidays`, which
prompts for the month and year.

The holidays known to Emacs include United States holidays and the
major Christian, Jewish, and Islamic holidays; also the solstices and
equinoxes.

The command `M-x list-holidays` displays the list of holidays for
a range of years. This function asks you for the starting and stopping
years, and allows you to choose all the holidays or one of several
categories of holidays. You can use this command even if you don't have
a calendar window.

The dates used by Emacs for holidays are based on *current
practice*, not historical fact. Historically, for instance, the start
of daylight savings time and even its existence have varied from year to
year, but present United States law mandates that daylight savings time
begins on the first Sunday in April. When the daylight savings rules
are set up for the United States, Emacs always uses the present
definition, even though it is wrong for some prior years.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC421) ] | [ [>](emacs.html#SEC423) ] | [ [<<](emacs.html#SEC423) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.7 Times of Sunrise and Sunset

Special calendar commands can tell you, to within a minute or two, the
times of sunrise and sunset for any date.

`S`: Display times of sunrise and sunset for the selected date (`calendar-sunrise-sunset`). `Mouse-2 Sunrise/sunset`: Display times of sunrise and sunset for the date you click on. `M-x sunrise-sunset`: Display times of sunrise and sunset for today's date. `C-u M-x sunrise-sunset`: Display times of sunrise and sunset for a specified date.

Within the calendar, to display the *local times* of sunrise and
sunset in the echo area, move point to the date you want, and type
`S`. Alternatively, click `Mouse-2` on the date, then choose
`` `Sunrise/sunset' `` from the menu that appears. The command `M-x
sunrise-sunset` is available outside the calendar to display this
information for today's date or a specified date. To specify a date
other than today, use `C-u M-x sunrise-sunset`, which prompts for
the year, month, and day.

You can display the times of sunrise and sunset for any location and
any date with `C-u C-u M-x sunrise-sunset`. This asks you for a
longitude, latitude, number of minutes difference from Coordinated
Universal Time, and date, and then tells you the times of sunrise and
sunset for that location on that date.

Because the times of sunrise and sunset depend on the location on
earth, you need to tell Emacs your latitude, longitude, and location
name before using these commands. Here is an example of what to set:

|  |  |
| --- | --- |
|  | ``` (setq calendar-latitude 40.1) (setq calendar-longitude -88.2) (setq calendar-location-name "Urbana, IL") ``` |

Use one decimal place in the values of `calendar-latitude` and
`calendar-longitude`.

Your time zone also affects the local time of sunrise and sunset.
Emacs usually gets time zone information from the operating system, but
if these values are not what you want (or if the operating system does
not supply them), you must set them yourself. Here is an example:

|  |  |
| --- | --- |
|  | ``` (setq calendar-time-zone -360) (setq calendar-standard-time-zone-name "CST") (setq calendar-daylight-time-zone-name "CDT") ``` |

The value of `calendar-time-zone` is the number of minutes
difference between your local standard time and Coordinated Universal
Time (Greenwich time). The values of
`calendar-standard-time-zone-name` and
`calendar-daylight-time-zone-name` are the abbreviations used in
your time zone. Emacs displays the times of sunrise and sunset
*corrected for daylight savings time*. See section [AC.12 Daylight Savings Time](emacs.html#SEC436),
for how daylight savings time is determined.

As a user, you might find it convenient to set the calendar location
variables for your usual physical location in your `.emacs' file.
And when you install Emacs on a machine, you can create a
`default.el' file which sets them properly for the typical location
of most users of that machine. See section [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC422) ] | [ [>](emacs.html#SEC424) ] | [ [<<](emacs.html#SEC424) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.8 Phases of the Moon

These calendar commands display the dates and times of the phases of
the moon (new moon, first quarter, full moon, last quarter). This
feature is useful for debugging problems that "depend on the phase of
the moon."

`M`: Display the dates and times for all the quarters of the moon for the three-month period shown (`calendar-phases-of-moon`). `M-x phases-of-moon`: Display dates and times of the quarters of the moon for three months around today's date.

Within the calendar, use the `M` command to display a separate
buffer of the phases of the moon for the current three-month range. The
dates and times listed are accurate to within a few minutes.

Outside the calendar, use the command `M-x phases-of-moon` to
display the list of the phases of the moon for the current month and the
preceding and succeeding months. For information about a different
month, use `C-u M-x phases-of-moon`, which prompts for the month and
year.

The dates and times given for the phases of the moon are given in
local time (corrected for daylight savings, when appropriate); but if
the variable `calendar-time-zone` is void, Coordinated Universal
Time (the Greenwich time zone) is used. See section [AC.12 Daylight Savings Time](emacs.html#SEC436).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC423) ] | [ [>](emacs.html#SEC425) ] | [ [<<](emacs.html#SEC412) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC429) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.9 Conversion To and From Other Calendars

The Emacs calendar displayed is *always* the Gregorian calendar,
sometimes called the "new style" calendar, which is used in most of
the world today. However, this calendar did not exist before the
sixteenth century and was not widely used before the eighteenth century;
it did not fully displace the Julian calendar and gain universal
acceptance until the early twentieth century. The Emacs calendar can
display any month since January, year 1 of the current era, but the
calendar displayed is the Gregorian, even for a date at which the
Gregorian calendar did not exist.

While Emacs cannot display other calendars, it can convert dates to
and from several other calendars.

> |  |  |  |
> | --- | --- | --- |
> | [AC.9.1 Supported Calendar Systems](emacs.html#SEC425) |  | The calendars Emacs understands |
>
>      (aside from Gregorian).
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AC.9.2 Converting To Other Calendars](emacs.html#SEC426) |  | Converting the selected date to various calendars. |
> | [AC.9.3 Converting From Other Calendars](emacs.html#SEC427) |  | Moving to a date specified in another calendar. |
> | [AC.9.4 Converting from the Mayan Calendar](emacs.html#SEC428) |  | Moving to a date specified in a Mayan calendar. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC424) ] | [ [>](emacs.html#SEC426) ] | [ [<<](emacs.html#SEC412) ] | [ [Up](emacs.html#SEC424) ] | [ [>>](emacs.html#SEC429) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.9.1 Supported Calendar Systems

The ISO commercial calendar is used largely in Europe.

The Julian calendar, named after Julius Caesar, was the one used in Europe
throughout medieval times, and in many countries up until the nineteenth
century.

Astronomers use a simple counting of days elapsed since noon, Monday,
January 1, 4713 B.C. on the Julian calendar. The number of days elapsed
is called the *Julian day number* or the *Astronomical day number*.

The Hebrew calendar is used by tradition in the Jewish religion. The
Emacs calendar program uses the Hebrew calendar to determine the dates
of Jewish holidays. Hebrew calendar dates begin and end at sunset.

The Islamic calendar is used in many predominantly Islamic countries.
Emacs uses it to determine the dates of Islamic holidays. There is no
universal agreement in the Islamic world about the calendar; Emacs uses
a widely accepted version, but the precise dates of Islamic holidays
often depend on proclamation by religious authorities, not on
calculations. As a consequence, the actual dates of observance can vary
slightly from the dates computed by Emacs. Islamic calendar dates begin
and end at sunset.

The French Revolutionary calendar was created by the Jacobins after the 1789
revolution, to represent a more secular and nature-based view of the annual
cycle, and to install a 10-day week in a rationalization measure similar to
the metric system. The French government officially abandoned this
calendar at the end of 1805.

The Maya of Central America used three separate, overlapping calendar
systems, the *long count*, the *tzolkin*, and the *haab*.
Emacs knows about all three of these calendars. Experts dispute the
exact correlation between the Mayan calendar and our calendar; Emacs uses the
Goodman-Martinez-Thompson correlation in its calculations.

The Copts use a calendar based on the ancient Egyptian solar calendar.
Their calendar consists of twelve 30-day months followed by an extra
five-day period. Once every fourth year they add a leap day to this
extra period to make it six days. The Ethiopic calendar is identical in
structure, but has different year numbers and month names.

The Persians use a solar calendar based on a design of Omar Khayyam.
Their calendar consists of twelve months of which the first six have 31
days, the next five have 30 days, and the last has 29 in ordinary years
and 30 in leap years. Leap years occur in a complicated pattern every
four or five years.

The Chinese calendar is a complicated system of lunar months arranged
into solar years. The years go in cycles of sixty, each year containing
either twelve months in an ordinary year or thirteen months in a leap
year; each month has either 29 or 30 days. Years, ordinary months, and
days are named by combining one of ten "celestial stems" with one of
twelve "terrestrial branches" for a total of sixty names that are
repeated in a cycle of sixty.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC425) ] | [ [>](emacs.html#SEC427) ] | [ [<<](emacs.html#SEC427) ] | [ [Up](emacs.html#SEC424) ] | [ [>>](emacs.html#SEC429) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.9.2 Converting To Other Calendars

The following commands describe the selected date (the date at point)
in various other calendar systems:

`Mouse-2 Other calendars`: Display the date that you click on, expressed in various other calendars. `p c`: Display ISO commercial calendar equivalent for selected day (`calendar-print-iso-date`). `p j`: Display Julian date for selected day (`calendar-print-julian-date`). `p a`: Display astronomical (Julian) day number for selected day (`calendar-print-astro-day-number`). `p h`: Display Hebrew date for selected day (`calendar-print-hebrew-date`). `p i`: Display Islamic date for selected day (`calendar-print-islamic-date`). `p f`: Display French Revolutionary date for selected day (`calendar-print-french-date`). `p C`: Display Chinese date for selected day (`calendar-print-chinese-date`). `p k`: Display Coptic date for selected day (`calendar-print-coptic-date`). `p e`: Display Ethiopic date for selected day (`calendar-print-ethiopic-date`). `p p`: Display Persian date for selected day (`calendar-print-persian-date`). `p m`: Display Mayan date for selected day (`calendar-print-mayan-date`).

If you are using X, the easiest way to translate a date into other
calendars is to click on it with `Mouse-2`, then choose `Other
calendars` from the menu that appears. This displays the equivalent
forms of the date in all the calendars Emacs understands, in the form of
a menu. (Choosing an alternative from this menu doesn't actually do
anything--the menu is used only for display.)

Otherwise, move point to the date you want to convert, then type the
appropriate command starting with `p` from the table above. The
prefix `p` is a mnemonic for "print," since Emacs "prints" the
equivalent date in the echo area.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC426) ] | [ [>](emacs.html#SEC428) ] | [ [<<](emacs.html#SEC428) ] | [ [Up](emacs.html#SEC424) ] | [ [>>](emacs.html#SEC429) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.9.3 Converting From Other Calendars

You can use the other supported calendars to specify a date to move
to. This section describes the commands for doing this using calendars
other than Mayan; for the Mayan calendar, see the following section.

`g c`: Move to a date specified in the ISO commercial calendar (`calendar-goto-iso-date`). `g j`: Move to a date specified in the Julian calendar (`calendar-goto-julian-date`). `g a`: Move to a date specified with an astronomical (Julian) day number (`calendar-goto-astro-day-number`). `g h`: Move to a date specified in the Hebrew calendar (`calendar-goto-hebrew-date`). `g i`: Move to a date specified in the Islamic calendar (`calendar-goto-islamic-date`). `g f`: Move to a date specified in the French Revolutionary calendar (`calendar-goto-french-date`). `g C`: Move to a date specified in the Chinese calendar (`calendar-goto-chinese-date`). `g p`: Move to a date specified in the Persian calendar (`calendar-goto-persian-date`). `g k`: Move to a date specified in the Coptic calendar (`calendar-goto-coptic-date`). `g e`: Move to a date specified in the Ethiopic calendar (`calendar-goto-ethiopic-date`).

These commands ask you for a date on the other calendar, move point to
the Gregorian calendar date equivalent to that date, and display the
other calendar's date in the echo area. Emacs uses strict completion
(see section [E.3 Completion](emacs.html#SEC34)) whenever it asks you to type a month name, so you
don't have to worry about the spelling of Hebrew, Islamic, or French names.

One common question concerning the Hebrew calendar is the computation
of the anniversary of a date of death, called a "yahrzeit." The Emacs
calendar includes a facility for such calculations. If you are in the
calendar, the command `M-x list-yahrzeit-dates` asks you for a
range of years and then displays a list of the yahrzeit dates for those
years for the date given by point. If you are not in the calendar,
this command first asks you for the date of death and the range of
years, and then displays the list of yahrzeit dates.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC427) ] | [ [>](emacs.html#SEC429) ] | [ [<<](emacs.html#SEC412) ] | [ [Up](emacs.html#SEC424) ] | [ [>>](emacs.html#SEC429) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.9.4 Converting from the Mayan Calendar

Here are the commands to select dates based on the Mayan calendar:

`g m l`: Move to a date specified by the long count calendar (`calendar-goto-mayan-long-count-date`). `g m n t`: Move to the next occurrence of a place in the tzolkin calendar (`calendar-next-tzolkin-date`). `g m p t`: Move to the previous occurrence of a place in the tzolkin calendar (`calendar-previous-tzolkin-date`). `g m n h`: Move to the next occurrence of a place in the haab calendar (`calendar-next-haab-date`). `g m p h`: Move to the previous occurrence of a place in the haab calendar (`calendar-previous-haab-date`). `g m n c`: Move to the next occurrence of a place in the calendar round (`calendar-next-calendar-round-date`). `g m p c`: Move to the previous occurrence of a place in the calendar round (`calendar-previous-calendar-round-date`).

To understand these commands, you need to understand the Mayan calendars.
The *long count* is a counting of days with these units:

|  |  |
| --- | --- |
|  | ``` 1 kin = 1 day   1 uinal = 20 kin   1 tun = 18 uinal 1 katun = 20 tun   1 baktun = 20 katun ``` |

Thus, the long count date 12.16.11.16.6 means 12 baktun, 16 katun, 11
tun, 16 uinal, and 6 kin. The Emacs calendar can handle Mayan long
count dates as early as 7.17.18.13.1, but no earlier. When you use the
`g m l` command, type the Mayan long count date with the baktun,
katun, tun, uinal, and kin separated by periods.

The Mayan tzolkin calendar is a cycle of 260 days formed by a pair of
independent cycles of 13 and 20 days. Since this cycle repeats
endlessly, Emacs provides commands to move backward and forward to the
previous or next point in the cycle. Type `g m p t` to go to the
previous tzolkin date; Emacs asks you for a tzolkin date and moves point
to the previous occurrence of that date. Similarly, type `g m n t`
to go to the next occurrence of a tzolkin date.

The Mayan haab calendar is a cycle of 365 days arranged as 18 months
of 20 days each, followed a 5-day monthless period. Like the tzolkin
cycle, this cycle repeats endlessly, and there are commands to move
backward and forward to the previous or next point in the cycle. Type
`g m p h` to go to the previous haab date; Emacs asks you for a haab
date and moves point to the previous occurrence of that date.
Similarly, type `g m n h` to go to the next occurrence of a haab
date.

The Maya also used the combination of the tzolkin date and the haab
date. This combination is a cycle of about 52 years called a
*calendar round*. If you type `g m p c`, Emacs asks you for
both a haab and a tzolkin date and then moves point to the previous
occurrence of that combination. Use `g m n c` to move point to the
next occurrence of a combination. These commands signal an error if the
haab/tzolkin date combination you have typed is impossible.

Emacs uses strict completion (see section [E.3.3 Strict Completion](emacs.html#SEC37)) whenever it
asks you to type a Mayan name, so you don't have to worry about
spelling.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC428) ] | [ [>](emacs.html#SEC430) ] | [ [<<](emacs.html#SEC417) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC435) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.10 The Diary

The Emacs diary keeps track of appointments or other events on a daily
basis, in conjunction with the calendar. To use the diary feature, you
must first create a *diary file* containing a list of events and
their dates. Then Emacs can automatically pick out and display the
events for today, for the immediate future, or for any specified
date.

By default, Emacs uses `~/diary' as the diary file. This is the
same file that the `calendar` utility uses. A sample
`~/diary' file is:

|  |  |
| --- | --- |
|  | ``` 12/22/1988  Twentieth wedding anniversary!! &1/1.       Happy New Year! 10/22       Ruth's birthday. * 21, *:    Payday Tuesday--weekly meeting with grad students at 10am          Supowit, Shen, Bitner, and Kapoor to attend. 1/13/89     Friday the thirteenth!! &thu 4pm    squash game with Lloyd. mar 16      Dad's birthday April 15, 1989 Income tax due. &* 15       time cards due. ``` |

This example uses extra spaces to align the event descriptions of most
of the entries. Such formatting is purely a matter of taste.

Although you probably will start by creating a diary manually, Emacs
provides a number of commands to let you view, add, and change diary
entries.

> |  |  |  |
> | --- | --- | --- |
> | [AC.10.1 Commands Displaying Diary Entries](emacs.html#SEC430) |  | Viewing diary entries and associated calendar dates. |
> | [AC.10.2 The Diary File](emacs.html#SEC431) |  | Entering events in your diary. |
> | [AC.10.3 Date Formats](emacs.html#SEC432) |  | Various ways you can specify dates. |
> | [AC.10.4 Commands to Add to the Diary](emacs.html#SEC433) |  | Commands to create diary entries. |
> | [AC.10.5 Special Diary Entries](emacs.html#SEC434) |  | Anniversaries, blocks of dates, cyclic entries, etc. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC429) ] | [ [>](emacs.html#SEC431) ] | [ [<<](emacs.html#SEC417) ] | [ [Up](emacs.html#SEC429) ] | [ [>>](emacs.html#SEC435) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.10.1 Commands Displaying Diary Entries

Once you have created a `~/diary' file, you can use the calendar
to view it. You can also view today's events outside of Calendar mode.

`d`: Display all diary entries for the selected date (`view-diary-entries`). `Mouse-2 Diary`: Display all diary entries for the date you click on. `s`: Display the entire diary file (`show-all-diary-entries`). `m`: Mark all visible dates that have diary entries (`mark-diary-entries`). `u`: Unmark the calendar window (`calendar-unmark`). `M-x print-diary-entries`: Print hard copy of the diary display as it appears. `M-x diary`: Display all diary entries for today's date. `M-x diary-mail-entries`: Mail yourself email reminders about upcoming diary entries.

Displaying the diary entries with `d` shows in a separate window
the diary entries for the selected date in the calendar. The mode line
of the new window shows the date of the diary entries and any holidays
that fall on that date. If you specify a numeric argument with `d`,
it shows all the diary entries for that many successive days. Thus,
`2 d` displays all the entries for the selected date and for the
following day.

Another way to display the diary entries for a date is to click
`Mouse-2` on the date, and then choose `Diary entries` from
the menu that appears.

To get a broader view of which days are mentioned in the diary, use
the `m` command. This displays the dates that have diary entries
in a different face (or places a `` `+' `` after these dates, if
display with multiple faces is not available). The command applies both
to the currently visible months and to other months that subsequently
become visible by scrolling. To turn marking off and erase the current
marks, type `u`, which also turns off holiday marks
(see section [AC.6 Holidays](emacs.html#SEC421)).

To see the full diary file, rather than just some of the entries, use
the `s` command.

Display of selected diary entries uses the selective display feature
to hide entries that don't apply.

The diary buffer as you see it is an illusion, so simply printing the
buffer does not print what you see on your screen. There is a special
command to print hard copy of the diary buffer *as it appears*;
this command is `M-x print-diary-entries`. It sends the data
directly to the printer. You can customize it like `lpr-region`
(see section [AC.18 Hardcopy Output](emacs.html#SEC458)).

The command `M-x diary` displays the diary entries for the current
date, independently of the calendar display, and optionally for the next
few days as well; the variable `number-of-diary-entries` specifies
how many days to include. See section `Calendar' in The Emacs Lisp Reference Manual.

If you put `(diary)` in your `.emacs' file, this
automatically displays a window with the day's diary entries, when you
enter Emacs. The mode line of the displayed window shows the date and
any holidays that fall on that date.

Many users like to receive notice of events in their diary as email.
To send such mail to yourself, use the command `M-x
diary-mail-entries`. A prefix argument specifies how many days
(starting with today) to check; otherwise, the variable
`diary-mail-days` says how many days.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC430) ] | [ [>](emacs.html#SEC432) ] | [ [<<](emacs.html#SEC432) ] | [ [Up](emacs.html#SEC429) ] | [ [>>](emacs.html#SEC435) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.10.2 The Diary File

Your *diary file* is a file that records events associated with
particular dates. The name of the diary file is specified by the
variable `diary-file`; `~/diary' is the default. The
`calendar` utility program supports a subset of the format allowed
by the Emacs diary facilities, so you can use that utility to view the
diary file, with reasonable results aside from the entries it cannot
understand.

Each entry in the diary file describes one event and consists of one
or more lines. An entry always begins with a date specification at the
left margin. The rest of the entry is simply text to describe the
event. If the entry has more than one line, then the lines after the
first must begin with whitespace to indicate they continue a previous
entry. Lines that do not begin with valid dates and do not continue a
preceding entry are ignored.

You can inhibit the marking of certain diary entries in the calendar
window; to do this, insert an ampersand (`` `&' ``) at the beginning of
the entry, before the date. This has no effect on display of the entry
in the diary window; it affects only marks on dates in the calendar
window. Nonmarking entries are especially useful for generic entries
that would otherwise mark many different dates.

If the first line of a diary entry consists only of the date or day
name with no following blanks or punctuation, then the diary window
display doesn't include that line; only the continuation lines appear.
For example, this entry:

|  |  |
| --- | --- |
|  | ``` 02/11/1989       Bill B. visits Princeton today       2pm Cognitive Studies Committee meeting       2:30-5:30 Liz at Lawrenceville       4:00pm Dentist appt       7:30pm Dinner at George's       8:00-10:00pm concert ``` |

appears in the diary window without the date line at the beginning.
This style of entry looks neater when you display just a single day's
entries, but can cause confusion if you ask for more than one day's
entries.

You can edit the diary entries as they appear in the window, but it is
important to remember that the buffer displayed contains the *entire*
diary file, with portions of it concealed from view. This means, for
instance, that the `C-f` (`forward-char`) command can put point
at what appears to be the end of the line, but what is in reality the
middle of some concealed line.

*Be careful when editing the diary entries!* Inserting
additional lines or adding/deleting characters in the middle of a
visible line cannot cause problems, but editing at the end of a line may
not do what you expect. Deleting a line may delete other invisible
entries that follow it. Before editing the diary, it is best to display
the entire file with `s` (`show-all-diary-entries`).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC431) ] | [ [>](emacs.html#SEC433) ] | [ [<<](emacs.html#SEC433) ] | [ [Up](emacs.html#SEC429) ] | [ [>>](emacs.html#SEC435) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.10.3 Date Formats

Here are some sample diary entries, illustrating different ways of
formatting a date. The examples all show dates in American order
(month, day, year), but Calendar mode supports European order (day,
month, year) as an option.

|  |  |
| --- | --- |
|  | ``` 4/20/93  Switch-over to new tabulation system apr. 25  Start tabulating annual results 4/30  Results for April are due */25  Monthly cycle finishes Friday  Don't leave without backing up files ``` |

The first entry appears only once, on April 20, 1993. The second and
third appear every year on the specified dates, and the fourth uses a
wildcard (asterisk) for the month, so it appears on the 25th of every
month. The final entry appears every week on Friday.

You can use just numbers to express a date, as in
`` `month/day' `` or `` `month/day/year' ``.
This must be followed by a nondigit. In the date itself, month
and day are numbers of one or two digits. The optional year
is also a number, and may be abbreviated to the last two digits; that
is, you can use `` `11/12/1989' `` or `` `11/12/89' ``.

Dates can also have the form `` `monthname day' `` or
`` `monthname day, year' ``, where the month's name can
be spelled in full or abbreviated to three characters (with or without a
period). Case is not significant.

A date may be *generic*; that is, partially unspecified. Then the
entry applies to all dates that match the specification. If the date
does not contain a year, it is generic and applies to any year.
Alternatively, month, day, or year can be a `` `*' ``;
this matches any month, day, or year, respectively. Thus, a diary entry
`` `3/*/*' `` matches any day in March of any year; so does `` `march
*' ``.

If you prefer the European style of writing dates--in which the day
comes before the month--type `M-x european-calendar` while in the
calendar, or set the variable `european-calendar-style` to `t`
*before* using any calendar or diary command. This mode interprets
all dates in the diary in the European manner, and also uses European
style for displaying diary dates. (Note that there is no comma after
the monthname in the European style.) To go back to the (default)
American style of writing dates, type `M-x american-calendar`.

You can use the name of a day of the week as a generic date which
applies to any date falling on that day of the week. You can abbreviate
the day of the week to three letters (with or without a period) or spell
it in full; case is not significant.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC432) ] | [ [>](emacs.html#SEC434) ] | [ [<<](emacs.html#SEC434) ] | [ [Up](emacs.html#SEC429) ] | [ [>>](emacs.html#SEC435) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.10.4 Commands to Add to the Diary

While in the calendar, there are several commands to create diary
entries:

`i d`: Add a diary entry for the selected date (`insert-diary-entry`). `i w`: Add a diary entry for the selected day of the week (`insert-weekly-diary-entry`). `i m`: Add a diary entry for the selected day of the month (`insert-monthly-diary-entry`). `i y`: Add a diary entry for the selected day of the year (`insert-yearly-diary-entry`).

You can make a diary entry for a specific date by selecting that date
in the calendar window and typing the `i d` command. This command
displays the end of your diary file in another window and inserts the
date; you can then type the rest of the diary entry.

If you want to make a diary entry that applies to a specific day of
the week, select that day of the week (any occurrence will do) and type
`i w`. This inserts the day-of-week as a generic date; you can then
type the rest of the diary entry. You can make a monthly diary entry in
the same fashion: select the day of the month, use the `i m`
command, and type the rest of the entry. Similarly, you can insert a
yearly diary entry with the `i y` command.

All of the above commands make marking diary entries by default. To
make a nonmarking diary entry, give a numeric argument to the command.
For example, `C-u i w` makes a nonmarking weekly diary entry.

When you modify the diary file, be sure to save the file before
exiting Emacs.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC433) ] | [ [>](emacs.html#SEC435) ] | [ [<<](emacs.html#SEC417) ] | [ [Up](emacs.html#SEC429) ] | [ [>>](emacs.html#SEC435) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.10.5 Special Diary Entries

In addition to entries based on calendar dates, the diary file can
contain *sexp entries* for regular events such as anniversaries.
These entries are based on Lisp expressions (sexps) that Emacs evaluates
as it scans the diary file. Instead of a date, a sexp entry contains
`` `%%' `` followed by a Lisp expression which must begin and end with
parentheses. The Lisp expression determines which dates the entry
applies to.

Calendar mode provides commands to insert certain commonly used
sexp entries:

`i a`: Add an anniversary diary entry for the selected date (`insert-anniversary-diary-entry`). `i b`: Add a block diary entry for the current region (`insert-block-diary-entry`). `i c`: Add a cyclic diary entry starting at the date (`insert-cyclic-diary-entry`).

If you want to make a diary entry that applies to the anniversary of a
specific date, move point to that date and use the `i a` command.
This displays the end of your diary file in another window and inserts
the anniversary description; you can then type the rest of the diary
entry. The entry looks like this:

|  |  |
| --- | --- |
|  | ``` %%(diary-anniversary 10 31 1948) Arthur's birthday ``` |

This entry applies to October 31 in any year after 1948; `` `10 31
1948' `` specifies the date. (If you are using the European calendar
style, the month and day are interchanged.) The reason this expression
requires a beginning year is that advanced diary functions can use it to
calculate the number of elapsed years.

A *block* diary entry applies to a specified range of consecutive
dates. Here is a block diary entry that applies to all dates from June
24, 1990 through July 10, 1990:

|  |  |
| --- | --- |
|  | ``` %%(diary-block 6 24 1990 7 10 1990) Vacation ``` |

The `` `6 24 1990' `` indicates the starting date and the `` `7 10 1990' ``
indicates the stopping date. (Again, if you are using the European calendar
style, the month and day are interchanged.)

To insert a block entry, place point and the mark on the two
dates that begin and end the range, and type `i b`. This command
displays the end of your diary file in another window and inserts the
block description; you can then type the diary entry.

*Cyclic* diary entries repeat after a fixed interval of days. To
create one, select the starting date and use the `i c` command. The
command prompts for the length of interval, then inserts the entry,
which looks like this:

|  |  |
| --- | --- |
|  | ``` %%(diary-cyclic 50 3 1 1990) Renew medication ``` |

This entry applies to March 1, 1990 and every 50th day following;
`` `3 1 1990' `` specifies the starting date. (If you are using the
European calendar style, the month and day are interchanged.)

All three of these commands make marking diary entries. To insert a
nonmarking entry, give a numeric argument to the command. For example,
`C-u i a` makes a nonmarking anniversary diary entry.

Marking sexp diary entries in the calendar is *extremely*
time-consuming, since every date visible in the calendar window must be
individually checked. So it's a good idea to make sexp diary entries
nonmarking (with `` `&' ``) when possible.

Another sophisticated kind of sexp entry, a *floating* diary entry,
specifies a regularly occurring event by offsets specified in days,
weeks, and months. It is comparable to a crontab entry interpreted by
the `cron` utility. Here is a nonmarking, floating diary entry
that applies to the last Thursday in November:

|  |  |
| --- | --- |
|  | ``` &%%(diary-float 11 4 -1) American Thanksgiving ``` |

The 11 specifies November (the eleventh month), the 4 specifies Thursday
(the fourth day of the week, where Sunday is numbered zero), and the
-1 specifies "last" (1 would mean "first," 2 would mean
"second," -2 would mean "second-to-last," and so on). The
month can be a single month or a list of months. Thus you could change
the 11 above to `` `'(1 2 3)' `` and have the entry apply to the last
Thursday of January, February, and March. If the month is `t`, the
entry applies to all months of the year.

Most generally, sexp diary entries can perform arbitrary
computations to determine when they apply. See section `Sexp Diary Entries' in The Emacs Lisp Reference Manual.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC434) ] | [ [>](emacs.html#SEC436) ] | [ [<<](emacs.html#SEC417) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.11 Appointments

If you have a diary entry for an appointment, and that diary entry
begins with a recognizable time of day, Emacs can warn you several
minutes beforehand that that appointment is pending. Emacs alerts you
to the appointment by displaying a message in the mode line.

To enable appointment notification, you must enable the time display
feature of Emacs, `M-x display-time` (see section [B.3 The Mode Line](emacs.html#SEC11)). You must
also add the function `appt-make-list` to the
`diary-hook`, like this:

|  |  |
| --- | --- |
|  | ``` (add-hook 'diary-hook 'appt-make-list) ``` |

Adding this text to your `.emacs' file does the whole job:

|  |  |
| --- | --- |
|  | ``` (display-time) (add-hook 'diary-hook 'appt-make-list) (diary 0) ``` |

With these preparations done, when you display the diary (either with
the `d` command in the calendar window or with the `M-x diary`
command), it sets up an appointment list of all the diary entries found
with recognizable times of day, and reminds you just before each of
them.

For example, suppose the diary file contains these lines:

|  |  |
| --- | --- |
|  | ``` Monday   9:30am Coffee break  12:00pm Lunch ``` |

Then on Mondays, after you have displayed the diary, you will be
reminded at 9:20am about your coffee break and at 11:50am about lunch.

You can write times in am/pm style (with `` `12:00am' `` standing
for midnight and `` `12:00pm' `` standing for noon), or 24-hour
European/military style. You need not be consistent; your diary file
can have a mixture of the two styles.

Emacs updates the appointments list automatically just after
midnight. This also displays the next day's diary entries in the diary
buffer, unless you set `appt-display-diary` to `nil`.

You can also use the appointment notification facility like an alarm
clock. The command `M-x appt-add` adds entries to the appointment
list without affecting your diary file. You delete entries from the
appointment list with `M-x appt-delete`.

You can turn off the appointment notification feature at any time by
setting `appt-issue-message` to `nil`.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC435) ] | [ [>](emacs.html#SEC437) ] | [ [<<](emacs.html#SEC417) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.12 Daylight Savings Time

Emacs understands the difference between standard time and daylight
savings time--the times given for sunrise, sunset, solstices,
equinoxes, and the phases of the moon take that into account. The rules
for daylight savings time vary from place to place and have also varied
historically from year to year. To do the job properly, Emacs needs to
know which rules to use.

Some operating systems keep track of the rules that apply to the place
where you are; on these systems, Emacs gets the information it needs
from the system automatically. If some or all of this information is
missing, Emacs fills in the gaps with the rules currently used in
Cambridge, Massachusetts. If the resulting rules are not what you want,
you can tell Emacs the rules to use by setting certain variables:
`calendar-daylight-savings-starts` and
`calendar-daylight-savings-ends`.

These values should be Lisp expressions that refer to the variable
`year`, and evaluate to the Gregorian date on which daylight
savings time starts or (respectively) ends, in the form of a list
`(month day year)`. The values should be
`nil` if your area does not use daylight savings time.

Emacs uses these expressions to determine the starting date of
daylight savings time for the holiday list and for correcting times of
day in the solar and lunar calculations.

The values for Cambridge, Massachusetts are as follows:

|  |  |
| --- | --- |
|  | ``` (calendar-nth-named-day 1 0 4 year) (calendar-nth-named-day -1 0 10 year) ``` |

That is, the first 0th day (Sunday) of the fourth month (April) in
the year specified by `year`, and the last Sunday of the tenth month
(October) of that year. If daylight savings time were
changed to start on October 1, you would set
`calendar-daylight-savings-starts` to this:

|  |  |
| --- | --- |
|  | ``` (list 10 1 year) ``` |

If there is no daylight savings time at your location, or if you want
all times in standard time, set `calendar-daylight-savings-starts`
and `calendar-daylight-savings-ends` to `nil`.

The variable `calendar-daylight-time-offset` specifies the
difference between daylight savings time and standard time, measured in
minutes. The value for Cambridge, Massachusetts is 60.

The two variables `calendar-daylight-savings-starts-time` and
`calendar-daylight-savings-ends-time` specify the number of minutes
after midnight local time when the transition to and from daylight
savings time should occur. For Cambridge, Massachusetts both variables'
values are 120.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC436) ] | [ [>](emacs.html#SEC438) ] | [ [<<](emacs.html#SEC417) ] | [ [Up](emacs.html#SEC412) ] | [ [>>](emacs.html#SEC438) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.13 Summing Time Intervals

The timeclock feature adds up time intervals, so you can (for
instance) keep track of how much time you spend working.

Use the `M-x timeclock-in` command when you start working on a
project, and `M-x timeclock-out` command when you're done. Each
time you do this, it adds one time interval to the record of the project.

Once you've collected data from a number of time intervals, you can use
`M-x timeclock-workday-remaining` to see how much time is left to
work today (assuming a typical average of 8 hours a day), and `M-x
timeclock-when-to-leave` which will calculate when you're "done."

If you want Emacs to display the amount of time "left" of your
workday in the mode line, either customize the
`timeclock-modeline-display` variable and set its value to
`t`, or invoke the `M-x timeclock-modeline-display` command.

Terminating the current Emacs session might or might not mean that
you have stopped working on the project. If you'd like Emacs to ask
you about this, set the value of the variable
`timeclock-ask-before-exiting` to `t` (via `M-x
customize`). By default, only an explicit `M-x timeclock-out`
tells Emacs that the current interval is over.

The timeclock functions work by accumulating the data in a file
called `.timelog' in your home directory. (On MS-DOS, this file
is called `\_timelog', since an initial period is not allowed in
file names on MS-DOS.) You can specify a different name for this file
by customizing the variable `timeclock-file`. If you edit the
timeclock file manually, or if you change the value of any of
timeclock's customizable variables, you should run the command
`M-x timeclock-reread-log` to update the data in Emacs from the
file.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC437) ] | [ [>](emacs.html#SEC439) ] | [ [<<](emacs.html#SEC412) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC442) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.14 Gnus

Gnus is an Emacs package primarily designed for reading and posting
Usenet news. It can also be used to read and respond to messages from a
number of other sources--mail, remote directories, digests, and so on.

Here we introduce Gnus and describe several basic features.
For full details, see section `Top' in The Gnus Manual.

To start Gnus, type `M-x gnus RET`.

> |  |  |  |
> | --- | --- | --- |
> | [AC.14.1 Gnus Buffers](emacs.html#SEC439) |  | The group, summary, and article buffers. |
> | [AC.14.2 When Gnus Starts Up](emacs.html#SEC440) |  | What you should know about starting Gnus. |
> | [AC.14.3 Summary of Gnus Commands](emacs.html#SEC441) |  | A short description of the basic Gnus commands. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC438) ] | [ [>](emacs.html#SEC440) ] | [ [<<](emacs.html#SEC438) ] | [ [Up](emacs.html#SEC438) ] | [ [>>](emacs.html#SEC442) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.14.1 Gnus Buffers

As opposed to most normal Emacs packages, Gnus uses a number of
different buffers to display information and to receive commands. The
three buffers users spend most of their time in are the *group
buffer*, the *summary buffer* and the *article buffer*.

The *group buffer* contains a list of groups. This is the first
buffer Gnus displays when it starts up. It normally displays only the
groups to which you subscribe and that contain unread articles. Use
this buffer to select a specific group.

The *summary buffer* lists one line for each article in a single
group. By default, the author, the subject and the line number are
displayed for each article, but this is customizable, like most aspects
of Gnus display. The summary buffer is created when you select a group
in the group buffer, and is killed when you exit the group. Use this
buffer to select an article.

The *article buffer* displays the article. In normal Gnus usage,
you don't select this buffer--all useful article-oriented commands work
in the summary buffer. But you can select the article buffer, and
execute all Gnus commands from that buffer, if you want to.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC439) ] | [ [>](emacs.html#SEC441) ] | [ [<<](emacs.html#SEC441) ] | [ [Up](emacs.html#SEC438) ] | [ [>>](emacs.html#SEC442) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.14.2 When Gnus Starts Up

At startup, Gnus reads your `.newsrc' news initialization file
and attempts to communicate with the local news server, which is a
repository of news articles. The news server need not be the same
computer you are logged in on.

If you start Gnus and connect to the server, but do not see any
newsgroups listed in the group buffer, type `L` or `A k` to get
a listing of all the groups. Then type `u` to toggle
subscription to groups.

The first time you start Gnus, Gnus subscribes you to a few selected
groups. All other groups start out as *killed groups* for you; you
can list them with `A k`. All new groups that subsequently come to
exist at the news server become *zombie groups* for you; type `A
z` to list them. You can subscribe to a group shown in these lists
using the `u` command.

When you quit Gnus with `q`, it automatically records in your
`.newsrc' and `.newsrc.eld' initialization files the
subscribed or unsubscribed status of all groups. You should normally
not edit these files manually, but you may if you know how.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC440) ] | [ [>](emacs.html#SEC442) ] | [ [<<](emacs.html#SEC438) ] | [ [Up](emacs.html#SEC438) ] | [ [>>](emacs.html#SEC442) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.14.3 Summary of Gnus Commands

Reading news is a two-step process:

1. Choose a group in the group buffer.

   - Select articles from the summary buffer. Each article selected is
     displayed in the article buffer in a large window, below the summary
     buffer in its small window.

Each Gnus buffer has its own special commands; however, the meanings
of any given key in the various Gnus buffers are usually analogous, even
if not identical. Here are commands for the group and summary buffers:

`q`: In the group buffer, update your `.newsrc' initialization file and quit Gnus. In the summary buffer, exit the current group and return to the group buffer. Thus, typing `q` twice quits Gnus. `L`: In the group buffer, list all the groups available on your news server (except those you have killed). This may be a long list! `l`: In the group buffer, list only the groups to which you subscribe and which contain unread articles. `u`: In the group buffer, unsubscribe from (or subscribe to) the group listed in the line that point is on. When you quit Gnus by typing `q`, Gnus lists in your `.newsrc' file which groups you have subscribed to. The next time you start Gnus, you won't see this group, because Gnus normally displays only subscribed-to groups. `C-k`: In the group buffer, "kill" the current line's group--don't even list it in `.newsrc' from now on. This affects future Gnus sessions as well as the present session. When you quit Gnus by typing `q`, Gnus writes information in the file `.newsrc' describing all newsgroups except those you have "killed." `SPC`: In the group buffer, select the group on the line under the cursor and display the first unread article in that group. In the summary buffer, * Select the article on the line under the cursor if none is selected. * Scroll the text of the selected article (if there is one). * Select the next unread article if at the end of the current article. Thus, you can move through all the articles by repeatedly typing `SPC`. `DEL`: In the group buffer, move point to the previous group containing unread articles. In the summary buffer, scroll the text of the article backwards. `n`: Move point to the next unread group, or select the next unread article. `p`: Move point to the previous unread group, or select the previous unread article. `C-n`: `C-p`: Move point to the next or previous item, even if it is marked as read. This does not select the article or group on that line. `s`: In the summary buffer, do an incremental search of the current text in the article buffer, just as if you switched to the article buffer and typed `C-s`. `M-s regexp RET`: In the summary buffer, search forward for articles containing a match for regexp.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC441) ] | [ [>](emacs.html#SEC443) ] | [ [<<](emacs.html#SEC438) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AC.15 Running Shell Commands from Emacs

Emacs has commands for passing single command lines to inferior shell
processes; it can also run a shell interactively with input and output
to an Emacs buffer named `` `*shell*' `` or run a shell inside a terminal
emulator window.

There is a shell implemented entirely in Emacs, documented in a separate
manual. See section `Eshell' in Eshell: The Emacs Shell.

`M-! cmd RET`: Run the shell command line cmd and display the output (`shell-command`). `M-| cmd RET`: Run the shell command line cmd with region contents as input; optionally replace the region with the output (`shell-command-on-region`). `M-x shell`: Run a subshell with input and output through an Emacs buffer. You can then give commands interactively. `M-x term`: Run a subshell with input and output through an Emacs buffer. You can then give commands interactively. Full terminal emulation is available. `M-x eshell`: Start the Emacs shell.

> |  |  |  |
> | --- | --- | --- |
> | [AC.15.1 Single Shell Commands](emacs.html#SEC443) |  | How to run one shell command and return. |
> | [AC.15.2 Interactive Inferior Shell](emacs.html#SEC444) |  | Permanent shell taking input via Emacs. |
> | [AC.15.3 Shell Mode](emacs.html#SEC445) |  | Special Emacs commands used with permanent shell. |
> | [AC.15.4 Shell Command History](emacs.html#SEC446) |  | Repeating previous commands in a shell buffer. |
> | [AC.15.5 Directory Tracking](emacs.html#SEC450) |  | Keeping track when the subshell changes directory. |
> | [AC.15.6 Shell Mode Options](emacs.html#SEC451) |  | Options for customizing Shell mode. |
> | [AC.15.7 Emacs Terminal Emulator](emacs.html#SEC452) |  | An Emacs window as a terminal emulator. |
> | [AC.15.8 Term Mode](emacs.html#SEC453) |  | Special Emacs commands used in Term mode. |
> | [AC.15.9 Page-At-A-Time Output](emacs.html#SEC454) |  | Paging in the terminal emulator. |
> | [AC.15.10 Remote Host Shell](emacs.html#SEC455) |  | Connecting to another computer. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC442) ] | [ [>](emacs.html#SEC444) ] | [ [<<](emacs.html#SEC442) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.1 Single Shell Commands

`M-!` (`shell-command`) reads a line of text using the
minibuffer and executes it as a shell command in a subshell made just
for that command. Standard input for the command comes from the null
device. If the shell command produces any output, the output appears
either in the echo area (if it is short), or in an Emacs buffer named
`` `*Shell Command Output*' ``, which is displayed in another window
but not selected (if the output is long).

For instance, one way to decompress a file `foo.gz' from Emacs
is to type `M-! gunzip foo.gz RET`. That shell command
normally creates the file `foo' and produces no terminal output.

A numeric argument, as in `M-1 M-!`, says to insert terminal
output into the current buffer instead of a separate buffer. It puts
point before the output, and sets the mark after the output. For
instance, `M-1 M-! gunzip < foo.gz RET` would insert the
uncompressed equivalent of `foo.gz' into the current buffer.

If the shell command line ends in `` `&' ``, it runs asynchronously.
For a synchronous shell command, `shell-command` returns the
command's exit status (0 means success), when it is called from a Lisp
program. You do not get any status information for an asynchronous
command, since it hasn't finished yet.

`M-|` (`shell-command-on-region`) is like `M-!` but
passes the contents of the region as the standard input to the shell
command, instead of no input. If a numeric argument is used, meaning
insert the output in the current buffer, then the old region is deleted
first and the output replaces it as the contents of the region. It
returns the command's exit status when it is called from a Lisp program.

One use for `M-|` is to run `uudecode`. For instance, if
the buffer contains uuencoded text, type `C-x h M-| uudecode
RET` to feed the entire buffer contents to the `uudecode`
program. That program will ignore everything except the encoded text,
and will store the decoded output into the file whose name is
specified in the encoded text.

Both `M-!` and `M-|` use `shell-file-name` to specify the
shell to use. This variable is initialized based on your `SHELL`
environment variable when Emacs is started. If the file name does not
specify a directory, the directories in the list `exec-path` are
searched; this list is initialized based on the environment variable
`PATH` when Emacs is started. Your `.emacs' file can override
either or both of these default initializations.

Both `M-!` and `M-|` wait for the shell command to complete.
To stop waiting, type `C-g` to quit; that terminates the shell
command with the signal `SIGINT`---the same signal that `C-c`
normally generates in the shell. Emacs waits until the command actually
terminates. If the shell command doesn't stop (because it ignores the
`SIGINT` signal), type `C-g` again; this sends the command a
`SIGKILL` signal which is impossible to ignore.

To specify a coding system for `M-!` or `M-|`, use the command
`C-x RET c` immediately beforehand. See section [Q.9 Specifying a Coding System](emacs.html#SEC222).

Error output from the command is normally intermixed with the regular
output. If you set the variable
`shell-command-default-error-buffer` to a string, which is a buffer
name, error output is inserted before point in the buffer of that name.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC443) ] | [ [>](emacs.html#SEC445) ] | [ [<<](emacs.html#SEC445) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.2 Interactive Inferior Shell

To run a subshell interactively, putting its typescript in an Emacs
buffer, use `M-x shell`. This creates (or reuses) a buffer named
`` `*shell*' `` and runs a subshell with input coming from and output going
to that buffer. That is to say, any "terminal output" from the subshell
goes into the buffer, advancing point, and any "terminal input" for
the subshell comes from text in the buffer. To give input to the subshell,
go to the end of the buffer and type the input, terminated by `RET`.

Emacs does not wait for the subshell to do anything. You can switch
windows or buffers and edit them while the shell is waiting, or while it is
running a command. Output from the subshell waits until Emacs has time to
process it; this happens whenever Emacs is waiting for keyboard input or
for time to elapse.

Input lines, once you submit them, are displayed using the face
`comint-highlight-input`, and prompts are displayed using the
face `comint-highlight-prompt`. This makes it easier to see
previous input lines in the buffer. See section [J.1 Using Multiple Typefaces](emacs.html#SEC78).

To make multiple subshells, you can invoke `M-x shell` with a
prefix argument (e.g. `C-u M-x shell`), which will read a buffer
name and create (or reuse) a subshell in that buffer. You can also
rename the `` `*shell*' `` buffer using `M-x rename-uniquely`, then
create a new `` `*shell*' `` buffer using plain `M-x shell`. All the
subshells in different buffers run independently and in parallel.

The file name used to load the subshell is the value of the variable
`explicit-shell-file-name`, if that is non-`nil`. Otherwise,
the environment variable `ESHELL` is used, or the environment
variable `SHELL` if there is no `ESHELL`. If the file name
specified is relative, the directories in the list `exec-path` are
searched; this list is initialized based on the environment variable
`PATH` when Emacs is started. Your `.emacs' file can override
either or both of these default initializations.

Emacs sends the new shell the contents of the file
`~/.emacs\_shellname' as input, if it exists, where
shellname is the name of the file that the shell was loaded
from. For example, if you use bash, the file sent to it is
`~/.emacs\_bash'.

To specify a coding system for the shell, you can use the command
`C-x RET c` immediately before `M-x shell`. You can also
specify a coding system after starting the shell by using `C-x
RET p` in the shell buffer. See section [Q.9 Specifying a Coding System](emacs.html#SEC222).

Unless the environment variable `EMACS` is already defined,
Emacs defines it in the subshell, with value `t`. A shell script
can check this variable to determine whether it has been run from an
Emacs subshell.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC444) ] | [ [>](emacs.html#SEC446) ] | [ [<<](emacs.html#SEC446) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC456) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AC.15.3 Shell Mode

Shell buffers use Shell mode, which defines several special keys
attached to the `C-c` prefix. They are chosen to resemble the usual
editing and job control characters present in shells that are not under
Emacs, except that you must type `C-c` first. Here is a complete list
of the special key bindings of Shell mode:

`RET`: At end of buffer send line as input; otherwise, copy current line to end of buffer and send it (`comint-send-input`). When a line is copied, any prompt at the beginning of the line (text output by programs preceding your input) is omitted. (See also the variable `comint-use-prompt-regexp-instead-of-fields`.) `TAB`: Complete the command name or file name before point in the shell buffer (`comint-dynamic-complete`). `TAB` also completes history references (see section [AC.15.4.3 Shell History References](emacs.html#SEC449)) and environment variable names. The variable `shell-completion-fignore` specifies a list of file name extensions to ignore in Shell mode completion. The default setting is `nil`, but some users prefer `("~" "#" "%")` to ignore file names ending in `` `~' ``, `` `#' `` or `` `%' ``. Other related Comint modes use the variable `comint-completion-fignore` instead. `M-?`: Display temporarily a list of the possible completions of the file name before point in the shell buffer (`comint-dynamic-list-filename-completions`). `C-d`: Either delete a character or send EOF (`comint-delchar-or-maybe-eof`). Typed at the end of the shell buffer, `C-d` sends EOF to the subshell. Typed at any other position in the buffer, `C-d` deletes a character as usual. `C-c C-a`: Move to the beginning of the line, but after the prompt if any (`comint-bol-or-process-mark`). If you repeat this command twice in a row, the second time it moves back to the process mark, which is the beginning of the input that you have not yet sent to the subshell. (Normally that is the same place--the end of the prompt on this line--but after `C-c SPC` the process mark may be in a previous line.) `C-c SPC`: Accumulate multiple lines of input, then send them together. This command inserts a newline before point, but does not send the preceding text as input to the subshell--at least, not yet. Both lines, the one before this newline and the one after, will be sent together (along with the newline that separates them), when you type `RET`. `C-c C-u`: Kill all text pending at end of buffer to be sent as input (`comint-kill-input`). `C-c C-w`: Kill a word before point (`backward-kill-word`). `C-c C-c`: Interrupt the shell or its current subjob if any (`comint-interrupt-subjob`). This command also kills any shell input pending in the shell buffer and not yet sent. `C-c C-z`: Stop the shell or its current subjob if any (`comint-stop-subjob`). This command also kills any shell input pending in the shell buffer and not yet sent. `C-c C-\`: Send quit signal to the shell or its current subjob if any (`comint-quit-subjob`). This command also kills any shell input pending in the shell buffer and not yet sent. `C-c C-o`: Delete the last batch of output from a shell command (`comint-delete-output`). This is useful if a shell command spews out lots of output that just gets in the way. This command used to be called `comint-kill-output`. `C-c C-s`: Write the last batch of output from a shell command to a file (`comint-write-output`). With a prefix argument, the file is appended to instead. Any prompt at the end of the output is not written. `C-c C-r`: `C-M-l`: Scroll to display the beginning of the last batch of output at the top of the window; also move the cursor there (`comint-show-output`). `C-c C-e`: Scroll to put the end of the buffer at the bottom of the window (`comint-show-maximum-output`). `C-c C-f`: Move forward across one shell command, but not beyond the current line (`shell-forward-command`). The variable `shell-command-regexp` specifies how to recognize the end of a command. `C-c C-b`: Move backward across one shell command, but not beyond the current line (`shell-backward-command`). `C-c C-l`: Display the buffer's history of shell commands in another window (`comint-dynamic-list-input-ring`). `M-x dirs`: Ask the shell what its current directory is, so that Emacs can agree with the shell. `M-x send-invisible RET text RET`: Send text as input to the shell, after reading it without echoing. This is useful when a shell command runs a program that asks for a password. Alternatively, you can arrange for Emacs to notice password prompts and turn off echoing for them, as follows: | | | | --- | --- | | | ``` (add-hook 'comint-output-filter-functions 'comint-watch-for-password-prompt) ``` | `M-x comint-continue-subjob`: Continue the shell process. This is useful if you accidentally suspend the shell process.[(14)](emacs.html#FOOT14) `M-x comint-strip-ctrl-m`: Discard all control-M characters from the current group of shell output. The most convenient way to use this command is to make it run automatically when you get output from the subshell. To do that, evaluate this Lisp expression: | | | | --- | --- | | | ``` (add-hook 'comint-output-filter-functions 'comint-strip-ctrl-m) ``` | `M-x comint-truncate-buffer`: This command truncates the shell buffer to a certain maximum number of lines, specified by the variable `comint-buffer-maximum-size`. Here's how to do this automatically each time you get output from the subshell: | | | | --- | --- | | | ``` (add-hook 'comint-output-filter-functions 'comint-truncate-buffer) ``` |

Shell mode also customizes the paragraph commands so that only shell
prompts start new paragraphs. Thus, a paragraph consists of an input
command plus the output that follows it in the buffer.

Shell mode is a derivative of Comint mode, a general-purpose mode for
communicating with interactive subprocesses. Most of the features of
Shell mode actually come from Comint mode, as you can see from the
command names listed above. The special features of Shell mode include
the directory tracking feature, and a few user commands.

Other Emacs features that use variants of Comint mode include GUD
(see section [V.5 Running Debuggers Under Emacs](emacs.html#SEC321)) and `M-x run-lisp` (see section [V.10 Running an External Lisp](emacs.html#SEC331)).

You can use `M-x comint-run` to execute any program of your choice
in a subprocess using unmodified Comint mode--without the
specializations of Shell mode.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC445) ] | [ [>](emacs.html#SEC447) ] | [ [<<](emacs.html#SEC450) ] | [ [Up](emacs.html#SEC442) ] | [ [>>](emacs.html#SEC450) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
