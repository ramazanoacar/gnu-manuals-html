### 18.2 Notes on the Free Translation Project [¶](#Notes-on-the-Free-Translation-Project)

This section contains the text that was, for a long time, distributed
as a file named `ABOUT-NLS`.

**NOTE:**  This documentation section is outdated. It it included
here for historical purposes only.

Free software is going international! The Free Translation Project is
a way to get maintainers of free software, translators, and users all
together, so that free software will gradually become able to speak many
languages. A few packages already provide translations for their messages.

If you found this `ABOUT-NLS` file inside a distribution, you
may assume that the distributed package does use GNU `gettext`
internally, itself available at your nearest GNU archive site. But you
do *not* need to install GNU `gettext` prior to configuring,
installing or using this package with messages translated.

Installers will find here some useful hints. These notes also explain
how users should proceed for getting the programs to use the available
translations. They tell how people wanting to contribute and work
on translations can contact the appropriate team.

* [INSTALL Matters](#INSTALL-Matters)
* [Using This Package](#Using-This-Package)
* [Translating Teams](#Translating-Teams)
* [Available Packages](#Available-Packages)
* [Using `gettext` in new packages](#Using-gettext-in-own-code)

---

Next: [Using This Package](#Using-This-Package), Up: [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 18.2.1 INSTALL Matters [¶](#INSTALL-Matters-1)

Some packages are *localizable* when properly installed; the
programs they contain can be made to speak your own native language.
Most such packages use GNU `gettext`. Other packages have their
own ways to internationalization, predating GNU `gettext`.

By default, this package will be installed to allow translation of
messages. It will automatically detect whether the system already
provides the GNU `gettext` functions. Installers may use special
options at configuration time for changing the default behaviour. The
command:

```
./configure --disable-nls
```

will *totally* disable translation of messages.

When you already have GNU `gettext` installed on your system and
run configure without an option for your new package, `configure`
will probably detect the previously built and installed `libintl`
library and will decide to use it. If not, you may have to to use the
‘`--with-libintl-prefix`’ option to tell `configure` where to
look for it.

Internationalized packages usually have many `po/ll.po`
or `po/ll_CC.po`
files, where

* ll gives an ISO 639 two-letter code identifying the language.
  For some languages,
  a two-letter code does not exist, and a three-letter code is used instead.
* The optional CC is an ISO 3166 two-letter code of a country or
  territory.

Unless translations have been forbidden
at `configure` time by using the ‘`--disable-nls`’ switch,
all available translations are installed together with the package.
However, the environment variable `LINGUAS` may be set, prior
to configuration, to limit the installed set.
`LINGUAS` should then contain a space separated list of locale names
(of the form `ll` or `ll_CC`,
stating which languages or language variants are allowed.

---

Next: [Translating Teams](#Translating-Teams), Previous: [INSTALL Matters](#INSTALL-Matters), Up: [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 18.2.2 Using This Package [¶](#Using-This-Package-1)

As a user, if your language has been installed for this package, you
only have to set the `LANG` environment variable to the appropriate
‘`ll_CC`’ combination. If you happen to have the `LC_ALL`
or some other `LC_xxx` environment variables set, you should unset them
before setting `LANG`, otherwise the setting of `LANG` will not
have the desired effect. Here

* ‘`ll`’ is an ISO 639 two-letter language code.
  For some languages,
  a two-letter code does not exist, and a three-letter code is used instead.
* ‘`CC`’ is an ISO 3166 two-letter code of a country or territory.

For example, let’s suppose that you
speak German and live in Germany. At the shell prompt, merely execute
‘`setenv LANG de_DE`’ (in `csh`),
‘`export LANG; LANG=de_DE`’ (in `sh`) or
‘`export LANG=de_DE`’ (in `bash`). This can be done from your
`.login` or `.profile` file, once and for all.

You might think that the country code specification is redundant. But in
fact, some languages have dialects in different countries. For example,
‘`de_AT`’ is used for Austria, and ‘`pt_BR`’ for Brazil. The country
code serves to distinguish the dialects.

The locale naming convention of ‘`ll_CC`’, with
‘`ll`’ denoting the language and ‘`CC`’ denoting the
country, is the one use on systems based on GNU libc. On other systems,
some variations of this scheme are used, such as ‘`ll`’ or
‘`ll_CC.encoding`’. You can get the list of
locales supported by your system for your language by running the command
‘`locale -a | grep '^ll'`’.

Not all programs have translations for all languages. By default, an
English message is shown in place of a nonexistent translation. If you
understand other languages, you can set up a priority list of languages.
This is done through a different environment variable, called
`LANGUAGE`. GNU `gettext` gives preference to `LANGUAGE`
over `LANG` for the purpose of message handling, but you still
need to have `LANG` set to the primary language; this is required
by other parts of the system libraries.
For example, some Swedish users who would rather read translations in
German than English for when Swedish is not available, set `LANGUAGE`
to ‘`sv:de`’ while leaving `LANG` to ‘`sv_SE`’.

Special advice for Norwegian users: The language code for Norwegian
bokmål changed from ‘`no`’ to ‘`nb`’ recently (in 2003).
During the transition period, while some message catalogs for this language
are installed under ‘`nb`’ and some older ones under ‘`no`’, it’s
recommended for Norwegian users to set `LANGUAGE` to ‘`nb:no`’ so that
both newer and older translations are used.

In the `LANGUAGE` environment variable, but not in the `LANG`
environment variable, ‘`ll_CC`’ combinations can be
abbreviated as ‘`ll`’ to denote the language’s main dialect.
For example, ‘`de`’ is equivalent to ‘`de_DE`’ (German as spoken in
Germany), and ‘`pt`’ to ‘`pt_PT`’ (Portuguese as spoken in Portugal)
in this context.

---

Next: [Available Packages](#Available-Packages), Previous: [Using This Package](#Using-This-Package), Up: [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 18.2.3 Translating Teams [¶](#Translating-Teams-1)

For the Free Translation Project to be a success, we need interested
people who like their own language and write it well, and who are also
able to synergize with other translators speaking the same language.
Each translation team has its own mailing list. The up-to-date list
of teams can be found at the Free Translation Project’s homepage,
`https://translationproject.org/`, in the "Teams" area.

If you’d like to volunteer to *work* at translating messages, you
should become a member of the translating team for your own language.
The subscribing address is *not* the same as the list itself, it
has ‘`-request`’ appended. For example, speakers of Swedish can send
a message to `sv-request@li.org`, having this message body:

```
subscribe
```

Keep in mind that team members are expected to participate
*actively* in translations, or at solving translational
difficulties, rather than merely lurking around. If your team does not
exist yet and you want to start one, or if you are unsure about what to
do or how to get started, please write to
`coordinator@translationproject.org` to reach the
coordinator for all translator teams.

The English team is special. It works at improving and uniformizing
the terminology in use. Proven linguistic skills are praised
more than programming skills, here.

---

Next: [Using `gettext` in new packages](#Using-gettext-in-own-code), Previous: [Translating Teams](#Translating-Teams), Up: [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 18.2.4 Available Packages [¶](#Available-Packages-1)

Languages are not equally supported in all packages. The following
matrix shows the current state of internationalization, as of
January 2026. The matrix shows, in regard of each package, for which
languages PO files have been submitted to translation coordination,
with a translation percentage of at least 50%.

```
Ready PO files       ab af an ar ast be bg bn bn_IN ca ckb crh cs da
                   +-------------------------------------------------+
a2ps               |                                           []    |
anubis             |                                              [] |
aspell             |             []  []             []         [] [] |
bash               |                    []          []         []    |
beebase            |                                                 |
bfd                |                                                 |
binutils           |          []                    []               |
bison              |                    []          []               |
bison-runtime      |             []     []          []            [] |
buzztrax           |                                           [] [] |
ccd2cue            |                                              [] |
ccide              |                                              [] |
cflow              |                                                 |
chambercourt       |                                                 |
clisp              |                                              [] |
coreutils          |          []        []          []         [] [] |
cpio               |                    []                        [] |
cppi               |                                              [] |
cpplib             |                                                 |
cryptsetup         |                                           [] [] |
datamash           |                                              [] |
denemo             |                                []         [] [] |
dfarc              |                                []            [] |
dialog             |                    []                           |
dico               |                                              [] |
diffutils          |                    []                     [] [] |
dink               |                                []            [] |
direvent           |                                              [] |
doodle             |                                           [] [] |
dos2unix           |                                []            [] |
dos2unix-man       |                                                 |
e2fsprogs          |                                []         [] [] |
enscript           |                                []            [] |
exif               |             []                            [] [] |
fetchmail          |                                []         [] [] |
findutils          |          []     [] []                     [] [] |
flex               |                    []          []            [] |
freedink           |                                []         [] [] |
gas                |                                                 |
gawk               |                    []          []               |
gcal               |                                []            [] |
gcc                |                                                 |
gdbm               |                                                 |
gettext-examples   |             []     []          []         [] [] |
gettext-runtime    |                    []                     []    |
gettext-tools      |                    []          []               |
gnubik             |          []                                  [] |
gnuchess           |                                              [] |
gnucobol           |                                                 |
gnulib             |                    []                           |
gnunet             |                                                 |
gnunet-gtk         |                                                 |
gnutls             |                                           []    |
gphoto2            |          []                               [] [] |
gprof              |                    []                        [] |
grep               |                 [] []          []         [] [] |
grip               |                 [] []          []            [] |
grub               |             []                 []            [] |
gsasl              |                                              [] |
gss                |                                              [] |
gst-plugins-bad    |          [] []     []                     [] [] |
gst-plugins-base   |          []        []          []         [] [] |
gst-plugins-good   |          []        []          []         [] [] |
gst-plugins-ugly   |          [] []     []          []         [] [] |
gstreamer          |          [] []     []          []         [] [] |
gtick              |                                           [] [] |
gtkam              |                 []                        [] [] |
gtkspell           |    []    [] []  []             []         [] [] |
gutenprint         |                                []            [] |
hello              |                                           []    |
help2man           |                                              [] |
help2man-texi      |                                                 |
i18nparse          |                                                 |
idutils            |                                              [] |
kbd                |                                           []    |
klavaro            |          []        [] []       []         [] [] |
ld                 |                                                 |
libc               |          []     [] []          []         [] [] |
libexif            |          []                                  [] |
libextractor       |          []                                  [] |
libgphoto2         |          []                               [] [] |
libgphoto2_port    |          []                               [] [] |
libiconv           |                 [] []                     [] [] |
libidn             |                                           [] [] |
libidn2            |                                           [] [] |
lilypond           |                                []         [] [] |
lordsawar          |                                []            [] |
lynx               |                                []         [] [] |
m4                 |                    []                     []    |
mailfromd          |                                              [] |
mailutils          |                                                 |
make               |                    []                     [] [] |
man-db             |             []                 []         [] [] |
man-db-manpages    |                                                 |
meritous           |                                              [] |
midi-instruments   |          []                    []         [] [] |
minicom            |                                           [] [] |
mpop               |                                                 |
msmtp              |                                                 |
nano               |                                []         [] [] |
opcodes            |                                                 |
parted             |                                           [] [] |
pies               |                                                 |
procps-ng          |                                                 |
procps-ng-man      |                                                 |
psmisc             |                 []                        [] [] |
psmisc-man         |                                                 |
pspp               |          []                    []               |
pyspread           |                                              [] |
radius             |                                []               |
recode             |                 []             []         [] [] |
recutils           |                                                 |
rush               |                                              [] |
sarg               |                                              [] |
savane             |                                                 |
sed                |             []  [] []          []         [] [] |
sharutils          |                                           []    |
shepherd           |                                                 |
shishi             |                                                 |
skribilo           |                                              [] |
sudo               |             []                 []         [] [] |
sudoers            |                                []         []    |
sysstat            |                 []                              |
tar                |                    []          []         [] [] |
texinfo            |                                []         [] [] |
texinfo_document   |                                []         []    |
tigervnc           |          []        []                     []    |
tin                |                                              [] |
tin-man            |                                                 |
trader             |                                              [] |
util-linux         |                                           [] [] |
util-linux-man     |                                                 |
vmm                |                                                 |
vorbis-tools       |                                           [] [] |
wastesedge         |                                              [] |
wcd                |                                              [] |
wcd-man            |                                                 |
wdiff              |                                []         [] [] |
wget               |          []     []                        [] [] |
wget2              |          []                                     |
wincoll            |                                                 |
wyslij-po          |                                              [] |
xboard             |                                              [] |
xdg-user-dirs      | [] [] [] [] []  [] []     []   [] []  []  [] [] |
xkeyboard-config   |          []        []          []         [] [] |
xz                 |          []                    []               |
xz-man             |          []                                     |
                   +-------------------------------------------------+
                     ab af an ar ast be bg bn bn_IN ca ckb crh cs da
                      1  2  1 26 13  14 32  1   1   49  1   1  61 91
```

```

```

```
                     de  el en en_GB eo es  et eu fa fi fr  fur ga gd
                   +--------------------------------------------------+
a2ps               | []  []             []  []          []            |
anubis             | []                 []           [] []            |
aspell             | []         []   [] []           [] []  []  []    |
bash               | []              [] []              []            |
beebase            | []                 []              []            |
bfd                |                    []              []            |
binutils           |                                    []            |
bison              | []  []          [] []  []       [] []            |
bison-runtime      | []  []          [] []  []       [] []      []    |
buzztrax           | []                 []           [] []            |
ccd2cue            | []              [] []              []            |
ccide              | []              [] []           [] []  []        |
cflow              | []              [] []              []            |
chambercourt       |     []                             []            |
clisp              | []     []          []              []            |
coreutils          | []                     []          []            |
cpio               | []                 []           [] []      []    |
cppi               | []              [] []           [] []            |
cpplib             | []                 []              []            |
cryptsetup         | []                 []              []            |
datamash           | []              [] []              []            |
denemo             |                                                  |
dfarc              | []              [] []           [] []  []        |
dialog             | []              []           []                  |
dico               | []                 []           [] []            |
diffutils          | []  []          [] []           [] []            |
dink               | []              [] []           [] []            |
direvent           | []              [] []              []            |
doodle             | []              [] []           [] []      []    |
dos2unix           | []              [] []              []  []        |
dos2unix-man       | []                 []              []            |
e2fsprogs          | []                 []              []            |
enscript           | []         []   [] []           [] []      []    |
exif               | []              [] []           [] []  []        |
fetchmail          | ()         []   [] []              []            |
findutils          | []  []          [] []  []       [] []            |
flex               | []              [] []           [] []      []    |
freedink           | []  []          [] []     []    [] []  []        |
gas                |                    []           [] []            |
gawk               | []                 []           [] []            |
gcal               | []                 []              []            |
gcc                | []                                 []            |
gdbm               | []              [] []              []            |
gettext-examples   | []              [] []           [] []      []    |
gettext-runtime    | []                 []                            |
gettext-tools      | []                 []           [] []            |
gnubik             | []              [] []           [] []  []        |
gnuchess           | []              [] []              []            |
gnucobol           | []                                 []            |
gnulib             | []              [] []           [] []            |
gnunet             |                    []                            |
gnunet-gtk         | []                 []                            |
gnutls             | []              [] []           [] []            |
gphoto2            | ()              []                 []            |
gprof              | []              [] []           [] []      []    |
grep               | []              [] []  []       [] []      []    |
grip               | []                 []           [] []  []        |
grub               | []              [] []           [] []            |
gsasl              | []              [] []           [] []      []    |
gss                | []              [] []           [] []      []    |
gst-plugins-bad    | []              [] []              []  []        |
gst-plugins-base   | []  []          [] []           [] []  []        |
gst-plugins-good   | []  []          [] []           [] []  []        |
gst-plugins-ugly   | []  []          [] []     []    [] []  []        |
gstreamer          | []  []          [] []     []    [] []  []        |
gtick              | ()              [] []           [] []      []    |
gtkam              | ()              [] []           [] []            |
gtkspell           | []  []          [] []     []    [] []  []  []    |
gutenprint         | []                 []              []            |
hello              | []                     []    [] [] []            |
help2man           | []  []          [] []           [] []            |
help2man-texi      | []                 []              []            |
i18nparse          | []              [] []              ()            |
idutils            | []              [] []           [] []      []    |
kbd                | []  []          [] []              []            |
klavaro            | []  []          [] []     []       []            |
ld                 |                    []              []            |
libc               | []                 []           [] []            |
libexif            | []                 []              []            |
libextractor       | []                 []              []            |
libgphoto2         | ()                                 []            |
libgphoto2_port    | ()                 []     []    [] []  []        |
libiconv           | []              [] []  []       [] []      []    |
libidn             | []              [] []           [] []            |
libidn2            | []              [] []              []  []        |
lilypond           | []              [] []              []            |
lordsawar          | []                                 []            |
lynx               | []              []     []       [] []            |
m4                 | []              [] []              []            |
mailfromd          | []                                 []            |
mailutils          | []                 []           [] []            |
make               | []                 []           [] []      []    |
man-db             | []              [] []              []            |
man-db-manpages    | []                 []              []            |
meritous           |                 []                               |
midi-instruments   | []  []          [] []  []    [] [] []            |
minicom            | []                 []           [] []            |
mpop               | []              [] []              []            |
msmtp              | []              [] []              []            |
nano               | []              [] []           [] []      []    |
opcodes            | []                 []              []            |
parted             | []                 []              []  []        |
pies               | []                 []              []            |
procps-ng          | []                 []              []            |
procps-ng-man      | []                                               |
psmisc             | []  []          [] []           [] []            |
psmisc-man         | []                                 []            |
pspp               | []  []             []              []            |
pyspread           | []                                 []            |
radius             |                    []              []            |
recode             | []              [] []           [] []      []    |
recutils           | []                              [] []            |
rush               | []                 []              []            |
sarg               | []                 []              []            |
savane             | []                 []              []            |
sed                | []  []          [] []  []       [] []      []    |
sharutils          | []              []     []          []            |
shepherd           | []                                 []            |
shishi             | []                              [] []            |
skribilo           |                 [] []              []  []        |
sudo               | []              [] []        [] [] []  []        |
sudoers            | []              []              [] []            |
sysstat            | []              []              [] []  []        |
tar                | []              [] []  []       [] []      []    |
texinfo            | []              [] []              []            |
texinfo_document   |                    []              []            |
tigervnc           | []                 []           [] []  []        |
tin                | []                     []          []            |
tin-man            | []                                               |
trader             | []         []   []              [] []            |
util-linux         | []                 []              []            |
util-linux-man     | []                                 []            |
vmm                |                                                  |
vorbis-tools       | []              [] []              []            |
wastesedge         |                                    []            |
wcd                | []              [] []           [] []  []        |
wcd-man            | []                                 []            |
wdiff              | []              [] []  []       [] []      []    |
wget               | []              [] []  []       [] []      []    |
wget2              | []                 []              []            |
wincoll            | []  []             []              []            |
wyslij-po          | []              [] []           [] []            |
xboard             | []                 []              []            |
xdg-user-dirs      | []  []          [] []  [] [] [] [] []  []  [] [] |
xkeyboard-config   | []              [] []           [] []  []        |
xz                 | []              [] []           []               |
xz-man             | []                                 []            |
                   +--------------------------------------------------+
                     de  el en en_GB eo es  et eu fa fi fr  fur ga gd
                     129 21  1   4   78 117 17  7  5 69 137 24  23  1
```

```

```

```
                     gl gu he hi hr hu hy id is it ja ka kab kk kn ko
                   +--------------------------------------------------+
a2ps               |             [] []             []              [] |
anubis             |             [] []    []    []                    |
aspell             |             [] []    []    [] []                 |
bash               |             [] []          []                 [] |
beebase            |                            ()                    |
bfd                |                                                  |
binutils           |                                                  |
bison              |                               []                 |
bison-runtime      | []          [] []    []    [] [] []              |
buzztrax           |                                                  |
ccd2cue            |                []                                |
ccide              |             [] []                []              |
cflow              |                                                  |
chambercourt       |                                                  |
clisp              |                                                  |
coreutils          |             [] []             []              [] |
cpio               |             [] []    []    [] [] []           [] |
cppi               | []          [] []          [] [] []              |
cpplib             |                                                  |
cryptsetup         |                               []                 |
datamash           |                                                  |
denemo             |                                                  |
dfarc              |             [] []          []                    |
dialog             |                      []       [] []              |
dico               |                                                  |
diffutils          |             [] []    []    [] [] []           [] |
dink               |                []                                |
direvent           |                []                                |
doodle             |                            []                    |
dos2unix           |                []             [] []           [] |
dos2unix-man       |                                               [] |
e2fsprogs          |                []                                |
enscript           |             []       []                          |
exif               | []          [] []    [] [] [] [] []           [] |
fetchmail          |                      []    [] []                 |
findutils          |             [] []    []    []    []           [] |
flex               |                                                  |
freedink           |             [] []    []    []                    |
gas                |                      []                          |
gawk               |                      []    ()                 [] |
gcal               |                                                  |
gcc                |                                                  |
gdbm               |                                  []              |
gettext-examples   | []          [] []          [] [] []              |
gettext-runtime    |             []             [] []              [] |
gettext-tools      |             []             [] []              [] |
gnubik             | []    []    []             []                    |
gnuchess           | []                                               |
gnucobol           |                                                  |
gnulib             |                            []    []              |
gnunet             |                                                  |
gnunet-gtk         |                                                  |
gnutls             |                            []    []              |
gphoto2            |                []    []    [] [] []           [] |
gprof              |                []    []    []    []              |
grep               | []          [] []    []    [] [] [] []        [] |
grip               |                []          [] []              [] |
grub               | []    []    [] []    []    []    []           [] |
gsasl              |                []    []    []    []              |
gss                |             [] []    []    []    []              |
gst-plugins-bad    |             [] []    []    []    []              |
gst-plugins-base   | []          [] []    []    []    []              |
gst-plugins-good   | []          [] []    []    [] [] []              |
gst-plugins-ugly   | []          [] []    []    [] [] []              |
gstreamer          | []          [] []    []    []                    |
gtick              | []             []    []    []                    |
gtkam              |       []       []    [] [] [] []                 |
gtkspell           | []    []    [] [] [] [] [] [] [] []              |
gutenprint         | []          [] []          []    []              |
hello              |             []                   []              |
help2man           |             []             [] []              [] |
help2man-texi      |                                                  |
i18nparse          |                                  []              |
idutils            |                []    []          []              |
kbd                |                      []                          |
klavaro            | []          [] []    []    []           []       |
ld                 |                                                  |
libc               | []          []       []    [] []              [] |
libexif            |                            []                    |
libextractor       |                                                  |
libgphoto2         |                                                  |
libgphoto2_port    |                            [] [] []              |
libiconv           | []          [] []    []    [] [] []           [] |
libidn             |             [] []    []    []                 [] |
libidn2            |                []    []          []           [] |
lilypond           |                            [] []                 |
lordsawar          |                               []                 |
lynx               |                []    []    [] []                 |
m4                 |                                               [] |
mailfromd          |                                                  |
mailutils          |                                                  |
make               |             []       []    [] []              [] |
man-db             |                      []    [] []              [] |
man-db-manpages    |                      []                       [] |
meritous           |                                                  |
midi-instruments   | []    []    [] [] [] [] [] [] [] []           [] |
minicom            |                []    []       [] []           [] |
mpop               |                                                  |
msmtp              |                                                  |
nano               | []          [] []    [] [] [] []              [] |
opcodes            |                                                  |
parted             |                []    []    [] [] []           [] |
pies               |                                                  |
procps-ng          |                                               [] |
procps-ng-man      |                                                  |
psmisc             |             [] []    []    []    []           [] |
psmisc-man         |             []                                [] |
pspp               |                []             []                 |
pyspread           |                                                  |
radius             |                      []                          |
recode             | []    []    [] []    []    []                    |
recutils           |                                                  |
rush               |                                                  |
sarg               |                                  []              |
savane             |       []                                         |
sed                | []          [] []    []    [] [] []           [] |
sharutils          |                                                  |
shepherd           |                                                  |
shishi             |                                                  |
skribilo           |                []                                |
sudo               | []          [] []    []    [] [] []           [] |
sudoers            |             []       []    [] []              [] |
sysstat            |             []       []    []    []           [] |
tar                |             [] []    []    [] []              [] |
texinfo            |             []             []                    |
texinfo_document   |             []                   []              |
tigervnc           |       []             []          []           [] |
tin                |                                                  |
tin-man            |                                                  |
trader             |             [] []                                |
util-linux         |             []                []              [] |
util-linux-man     |                                                  |
vmm                |                                                  |
vorbis-tools       |             []       []          []              |
wastesedge         |                            []                    |
wcd                |                                                  |
wcd-man            |                                                  |
wdiff              | []             []          []                    |
wget               |             [] []    []    [] []              [] |
wget2              |             []       []    [] []              [] |
wincoll            |                                                  |
wyslij-po          | []          [] []    []                          |
xboard             |                                                  |
xdg-user-dirs      | [] [] [] [] [] []    [] [] [] [] [] []  [] [] [] |
xkeyboard-config   | []    []    [] []    []    []    []           [] |
xz                 |             [] []          []    []           [] |
xz-man             |                            []                 [] |
                   +--------------------------------------------------+
                     gl gu he hi hr hu hy id is it ja ka kab kk kn ko
                     27  1 10  1 57 60  2 57  6 65 45 45  2   2  1 45
```

```

```

```
                     ku ky lg lt lv mk ml mn mr ms mt nb ne nl nn or
                   +-------------------------------------------------+
a2ps               |                            []    []    []       |
anubis             |                            []    []    []       |
aspell             |                      []                []       |
bash               |                                  []    []       |
beebase            |                                        []       |
bfd                |                                                 |
binutils           |                                                 |
bison              |                            []                   |
bison-runtime      |    []    [] []             []    []    []       |
buzztrax           |                                                 |
ccd2cue            |                                                 |
ccide              |             []                         []       |
cflow              |                                                 |
chambercourt       |                                                 |
clisp              |                                        []       |
coreutils          |                                  []    []       |
cpio               |                                        []       |
cppi               |                                                 |
cpplib             |                                                 |
cryptsetup         |                                                 |
datamash           |                                  []    []       |
denemo             |                                        []       |
dfarc              |                []                      []       |
dialog             |                                                 |
dico               |                                                 |
diffutils          |             []             []    []    []       |
dink               |                                        []       |
direvent           |                                        []       |
doodle             |                                        []       |
dos2unix           |                                  []    []       |
dos2unix-man       |                                        []       |
e2fsprogs          |                                        []       |
enscript           |                                        []       |
exif               |             []             []          []       |
fetchmail          |                                        []       |
findutils          |                                  []    []       |
flex               |                                        []       |
freedink           |             []                   []    []       |
gas                |                                                 |
gawk               |                                        []       |
gcal               |                                                 |
gcc                |                                                 |
gdbm               |                                                 |
gettext-examples   |    []       []             []    []    [] []    |
gettext-runtime    |                                                 |
gettext-tools      |                                                 |
gnubik             |                                  []    []       |
gnuchess           |                                  []    []       |
gnucobol           |                                                 |
gnulib             |                                        []       |
gnunet             |                                                 |
gnunet-gtk         |                                                 |
gnutls             |                            []          []       |
gphoto2            |                                        []       |
gprof              |                            []          []       |
grep               |                                  []    []       |
grip               |                                  []    []       |
grub               |          []                      []    []       |
gsasl              |                                        []       |
gss                |                                                 |
gst-plugins-bad    |             []                   []    []       |
gst-plugins-base   |             []                   []    []       |
gst-plugins-good   |             []                   []    []       |
gst-plugins-ugly   |             []             [] [] []    []       |
gstreamer          |          [] []                   []    []       |
gtick              |                                  []    []       |
gtkam              |                                  []    []       |
gtkspell           |    []    [] []       []    []    []    []       |
gutenprint         |                                        []       |
hello              |             []                   []    []       |
help2man           |                                  []             |
help2man-texi      |                                                 |
i18nparse          |                                        []       |
idutils            |                                  []    []       |
kbd                |                                                 |
klavaro            |                                  []    []       |
ld                 |                                                 |
libc               |                                        []       |
libexif            |                                        []       |
libextractor       |                                        []       |
libgphoto2         |                                        []       |
libgphoto2_port    |                                        []       |
libiconv           |          []                            []       |
libidn             |                                        []       |
libidn2            |             []                         []       |
lilypond           |                                        []       |
lordsawar          |                                                 |
lynx               |                                        []       |
m4                 |                                        []       |
mailfromd          |                                                 |
mailutils          |                                                 |
make               |                                        []       |
man-db             |                                        []       |
man-db-manpages    |                                                 |
meritous           |                                                 |
midi-instruments   | []       []          []    []       [] []       |
minicom            |                                  []             |
mpop               |                                                 |
msmtp              |                                                 |
nano               |                            []    []    []       |
opcodes            |                                                 |
parted             |                                        []       |
pies               |                                                 |
procps-ng          |                                                 |
procps-ng-man      |                                                 |
psmisc             |                                        []       |
psmisc-man         |                                                 |
pspp               |          []                            []       |
pyspread           |                                           []    |
radius             |                                        []       |
recode             |                                  []    []       |
recutils           |                                        []       |
rush               |                                                 |
sarg               |                                                 |
savane             |                                                 |
sed                |                                  []    []       |
sharutils          |                                        []       |
shepherd           |                                                 |
shishi             |                                                 |
skribilo           |                                                 |
sudo               |                                  []    []       |
sudoers            |                                  []             |
sysstat            |             []                   []    []       |
tar                |    []                            []    []       |
texinfo            |                                        []       |
texinfo_document   |                                                 |
tigervnc           |                                                 |
tin                |                                                 |
tin-man            |                                                 |
trader             |                                  []             |
util-linux         |                                        []       |
util-linux-man     |                                                 |
vmm                |                                                 |
vorbis-tools       |                                        []       |
wastesedge         |                                        []       |
wcd                |                                        []       |
wcd-man            |                                        []       |
wdiff              |                            []    []    []       |
wget               |                                  []    []       |
wget2              |                                                 |
wincoll            |                                                 |
wyslij-po          |                                        [] []    |
xboard             |                                        []       |
xdg-user-dirs      | [] [] [] [] [] [] []    []       []    [] [] [] |
xkeyboard-config   |          []                            []       |
xz                 |                                        []       |
xz-man             |                                                 |
                   +-------------------------------------------------+
                     ku ky lg lt lv mk ml mn mr ms mt nb ne nl nn or
                      2  5  1  9 16  2  1  3  1 14  1 40  1 90  4  1
```

```

```

```
                     pa pl pt pt_BR ro  ru rw scn sk sl sq sr  sv  sw
                   +--------------------------------------------------+
a2ps               |    []          []  []                 []  []     |
anubis             |    [] []       []  []                 []  []     |
aspell             |    [] []  []   []  []        [] [] [] []  []     |
bash               |    [] []  []   []                     []  []     |
beebase            |                []                     []  []     |
bfd                |       []       []  []                 []  []     |
binutils           |       []       []  []                 []  []     |
bison              |    [] []  []                          []  []     |
bison-runtime      |    [] []  []   []  []           [] [] []  []     |
buzztrax           |           []   []                     []  []     |
ccd2cue            |           []   []                     []  []     |
ccide              |    []          []  []                 []  []     |
cflow              |    []     []   []                     []  []     |
chambercourt       |           []   []                     []  []     |
clisp              |                    []                 []  []     |
coreutils          |    [] []  []   []  []           []    []  []     |
cpio               |    [] []  []   []  []                 []  []     |
cppi               |    []     []   []                     []  []     |
cpplib             |                []                     []  []     |
cryptsetup         |    []          []  []        []       []  []     |
datamash           |           []   []                     []  []     |
denemo             |                                                  |
dfarc              |    []     []   []                     []  []     |
dialog             |    [] []       []  []              [] []  []     |
dico               |    []     []   []                     []  []     |
diffutils          |    [] []  []   []  []                 []  []     |
dink               |                                           []     |
direvent           |    []     []   []                     []  []     |
doodle             |           []   []                  [] []  []     |
dos2unix           |    []     []   []  []                 []  []     |
dos2unix-man       |    []     []   []                     []  []     |
e2fsprogs          |    [] []       []                     []  []     |
enscript           |    []     []   []  []           []    []  []     |
exif               |    [] []  []   []  []        []       []  []     |
fetchmail          |    []          []  []              [] []  []     |
findutils          |    [] []  []   []  []           []    []  []     |
flex               |    [] []  []   []  []                 []  []     |
freedink           |    []     []   []  []           []    []  []     |
gas                |                []  []                     []     |
gawk               |    [] []  []   []                     []  []     |
gcal               |                []                     []  []     |
gcc                |                    []                     []     |
gdbm               |    []     []   []  []                 []  []     |
gettext-examples   |    [] []  []   []  []        [] [] [] []  []     |
gettext-runtime    |    [] []  []   []  []        []       []  []     |
gettext-tools      |    [] []  []   []  []        [] []    []  []     |
gnubik             |    [] []  []   []               []    []  []     |
gnuchess           |           []   []                     []  []     |
gnucobol           |                                       []         |
gnulib             |    [] []  []   []  []                 []  []     |
gnunet             |                                       []         |
gnunet-gtk         |                                       []         |
gnutls             |    []     []   []                     []  []     |
gphoto2            |    []     []   []  []                 []  []     |
gprof              |           []   []  []                 []  []     |
grep               |    [] []  []   []  []        [] []    []  []     |
grip               |    []     []   []  []                 []  []     |
grub               |    [] []       []  []           []    []  []     |
gsasl              |    []     []   []            []       []  []     |
gss                |    []     []   []            []       []  []     |
gst-plugins-bad    |    []     []   []  []        [] []    []  []     |
gst-plugins-base   |    []     []   []  []        [] []    []  []     |
gst-plugins-good   |    []     []   []  []        [] []    []  []     |
gst-plugins-ugly   |    []     []   []  []        [] [] [] []  []     |
gstreamer          |    []     []   []  []        [] []    []  []     |
gtick              |    []     []   []  []        []       []  []     |
gtkam              | [] [] []  []   []  []        []       []  []     |
gtkspell           |    [] []  []   []  []        [] [] [] []  []     |
gutenprint         |                []  []        [] []        []     |
hello              |    [] []  []   []  []    []  []       []  []     |
help2man           |    []     []   []  []                 []  []     |
help2man-texi      |    []     []   []                     []  []     |
i18nparse          |           []   []               []    []  []     |
idutils            |    []     []   []  []                 []  []     |
kbd                |    [] []  []   []  []                 []  []     |
klavaro            | [] [] []  []   []  []        [] []    []  []     |
ld                 |           []   []  []                 []  []     |
libc               |    [] []  []   []  []        []       []  []     |
libexif            |    []          []            []       []  []     |
libextractor       |    []     []   []                     []  []     |
libgphoto2         |    []          []                         []     |
libgphoto2_port    |    []     []   []  []        []       []  []     |
libiconv           |    []     []   []  []        [] []    []  []     |
libidn             |    []     []   []                     []  []     |
libidn2            |    []     []   []  []                 []  []     |
lilypond           |                []                         []     |
lordsawar          |                    []                     []     |
lynx               |           []   []  []                     []     |
m4                 |    []     []   []                     []  []     |
mailfromd          |    []     []   []                     []         |
mailutils          |    []     []   []                     []  []     |
make               |    [] []  []   []  []                 []  []     |
man-db             |    [] []  []   []  []                 []  []     |
man-db-manpages    |    [] []  []   []  []                 []  []     |
meritous           |           []   []                     []  []     |
midi-instruments   |    [] []  []   []  []        [] []    []  []     |
minicom            |    []     []   []  []                 []  []     |
mpop               |           []   []  []                 []  []     |
msmtp              |           []   []  []                 []  []     |
nano               |    [] []  []   []  []        []    [] []  []     |
opcodes            |           []   []                     []  []     |
parted             |    [] []  []   []  []        [] []    []  []     |
pies               |    []     []   []                     []         |
procps-ng          |    []     []   []                         []     |
procps-ng-man      |                []                         []     |
psmisc             |    [] []  []   []  []                 []  []     |
psmisc-man         |           []   []  []                 []  []     |
pspp               |                    []                     []     |
pyspread           |           []                          []  []     |
radius             |    []          []  []                 []  []     |
recode             |    [] []  []   []  []        [] []    []  []     |
recutils           |           []   []                     []  []     |
rush               |    []     []   []                     []  []     |
sarg               |           []   []  []                 []  []     |
savane             |           []   []  ()                            |
sed                |    [] []  []   []  []        [] []    []  []     |
sharutils          |    []     []   []                     []  []     |
shepherd           |                []            []       []  []     |
shishi             |    []          []                     []  []     |
skribilo           |           []   []                     []  []     |
sudo               |    [] []  []   []  []                 []  []     |
sudoers            |    [] []  []   []  []                 []  []     |
sysstat            |       []  []   []  []                 []  []     |
tar                |    [] []  []   []  []           []    []  []     |
texinfo            |    [] []  []   []  []        []       []  []     |
texinfo_document   |    [] []  []   []            []       []  []     |
tigervnc           |                []  []        []       []  []     |
tin                |                []                                |
tin-man            |                []                         []     |
trader             |           []   []  []                 []  []     |
util-linux         |    [] []  []   []                     []  []     |
util-linux-man     |    []          []                     []  []     |
vmm                |                []                     []         |
vorbis-tools       |    []     []   []               []    []  []     |
wastesedge         |           []                              []     |
wcd                |           []   []                     []  []     |
wcd-man            |           []   []                     []  []     |
wdiff              |    []     []   []  []           []    []  []     |
wget               |    [] []  []   []  []        []       []  []     |
wget2              |    []          []  []                 []  []     |
wincoll            |           []   []            []       []  []     |
wyslij-po          |    []     []   []                     []  []     |
xboard             |    []          []  []                 []  []     |
xdg-user-dirs      | [] [] []  []   []  []        [] [] [] []  []     |
xkeyboard-config   |    [] []  []   []  []           [] [] []  []     |
xz                 |    [] []  []   []                     []  []     |
xz-man             |           []   []                     []  []     |
                   +--------------------------------------------------+
                     pa pl pt pt_BR ro  ru rw scn sk sl sq sr  sv  sw
                      3 97 49  109  135 83  0  1  36 30 11 131 138  0
```

```

```

```
                     ta te tg th tr uk  ur vi wa wo zh_CN zh_HK zh_TW
                   +--------------------------------------------------+
a2ps               |          []    []                                | 20
anubis             |             [] []     []                         | 21
aspell             |                []     [] []     []               | 34
bash               |                []     []        []          []   | 23
beebase            |                []                                |  8
bfd                |                []                                |  8
binutils           |                []                                |  9
bison              |                []                           []   | 18
bison-runtime      | []       [] [] []     []        []          []   | 41
buzztrax           |                       []        []               | 12
ccd2cue            |          []    []     []        []               | 14
ccide              |                []     []        []               | 20
cflow              |                []               []               | 11
chambercourt       |                []                                |  7
clisp              |                                                  | 13
coreutils          |             [] []     []        []          []   | 27
cpio               |             [] []     []        []               | 26
cppi               |                []     []        []               | 20
cpplib             |                []               []               |  8
cryptsetup         |                []               []               | 14
datamash           |                []                                | 12
denemo             |                                 []    []    []   |  7
dfarc              |                       []                         | 19
dialog             |                []                                | 15
dico               |                []                                | 11
diffutils          |             [] []     []        []          []   | 32
dink               |             []                                   | 11
direvent           |                []     []                         | 14
doodle             |                []     []        []               | 18
dos2unix           |                []     []        []          []   | 23
dos2unix-man       |                []               []               | 12
e2fsprogs          |                []     []        []               | 16
enscript           |             [] []     []                         | 22
exif               |             [] []     []        []               | 33
fetchmail          |                       []        []               | 19
findutils          |             [] []     []        []          []   | 33
flex               |             [] []     []        []          []   | 22
freedink           |          []           []                         | 27
gas                |                []                                |  8
gawk               |             [] []     []        []               | 19
gcal               |             []                  []               | 10
gcc                |                []                                |  5
gdbm               |                []     []                         | 13
gettext-examples   | []          [] []     []        []          []   | 39
gettext-runtime    |                []               []               | 18
gettext-tools      |                []     []        []          []   | 23
gnubik             |                []     []        []               | 24
gnuchess           |                []     []        []               | 15
gnucobol           |                                                  |  3
gnulib             |                []               []          []   | 19
gnunet             |                                                  |  2
gnunet-gtk         |                                                  |  3
gnutls             |                []     []        []               | 18
gphoto2            |                []     []        []          []   | 22
gprof              |             [] []     []                         | 22
grep               | []       [] [] []     []        []          []   | 39
grip               |                []     []        []    []    []   | 26
grub               |             [] []     []        []               | 30
gsasl              |                []     []        []               | 21
gss                |                       []        []               | 20
gst-plugins-bad    |             [] []     []        []          []   | 31
gst-plugins-base   |             [] []     []        []               | 33
gst-plugins-good   |             [] []     []        []          []   | 35
gst-plugins-ugly   | []          [] []     []        []          []   | 41
gstreamer          |             [] []     []        []          []   | 36
gtick              |                []     []        []               | 23
gtkam              |                []     []        []               | 27
gtkspell           | []       [] [] []     [] []     []    []    []   | 52
gutenprint         |             [] []               []               | 19
hello              | []       []    []               []               | 24
help2man           |                []     []        []               | 21
help2man-texi      |                []                                |  9
i18nparse          |                []                                | 11
idutils            |                []     []        []               | 21
kbd                |                []                                | 15
klavaro            |             [] []  [] []    []  []               | 36
ld                 |                []                                |  8
libc               |             [] []     []        []          []   | 30
libexif            |                []     []        []               | 15
libextractor       |                []     []                         | 13
libgphoto2         |                []     []        []               | 11
libgphoto2_port    |                []     []        []          []   | 23
libiconv           |                []     [] []     []          []   | 34
libidn             |                []     []        []               | 21
libidn2            |                []               []               | 21
lilypond           |                                 []               | 13
lordsawar          |             []                                   |  8
lynx               |             [] []     []                         | 20
m4                 |                []               []               | 15
mailfromd          |                []     []                         |  9
mailutils          |                []     []                         | 11
make               |                []     []        []          []   | 25
man-db             |             [] []     []        []          []   | 25
man-db-manpages    |             [] []               []               | 15
meritous           |                                                  |  6
midi-instruments   |          [] [] []     []        []          []   | 44
minicom            |                       []                    []   | 20
mpop               |                []                                | 10
msmtp              |                []               []               | 11
nano               |             [] []     []        []          []   | 34
opcodes            |                []                                |  8
parted             |             [] []     []        []          []   | 27
pies               |                []     []                         |  9
procps-ng          |                []     []        []               | 11
procps-ng-man      |                []               []               |  5
psmisc             |                []     []        []          []   | 27
psmisc-man         |                []                                | 10
pspp               | []       []    []               []               | 16
pyspread           |                []                                |  8
radius             |                []     []                         | 12
recode             |             [] []     []        []               | 31
recutils           |                []     []                         | 10
rush               |                []               []               | 11
sarg               |                                                  | 10
savane             |                []                                |  7
sed                |             [] []     []        []          []   | 38
sharutils          |                []     []        []               | 14
shepherd           |                []               []               |  8
shishi             |                       []        []               |  9
skribilo           |                []                                | 11
sudo               |             [] []     []        []          []   | 33
sudoers            |                []               []          []   | 22
sysstat            |             [] []     []        []               | 24
tar                |             [] []     []        []          []   | 33
texinfo            |             [] []     []                         | 21
texinfo_document   |                []                                | 14
tigervnc           |             [] []               []          []   | 21
tin                |             []                  []          []   |  8
tin-man            |                                                  |  3
trader             |                                                  | 14
util-linux         |             [] []               []               | 18
util-linux-man     |                []                                |  7
vmm                |                []                                |  3
vorbis-tools       |                       []                         | 17
wastesedge         |             []                                   |  7
wcd                |                []     []        []               | 15
wcd-man            |                []                                |  8
wdiff              |                []     []        []          []   | 27
wget               |             [] []     []        []          []   | 32
wget2              |                []               []               | 16
wincoll            |                []                                | 10
wyslij-po          |                []     []        []               | 20
xboard             |             [] []               []               | 13
xdg-user-dirs      | [] []    [] [] []     [] []     []    []    []   | 73
xkeyboard-config   |             [] []               []               | 33
xz                 |             [] []     []        []          []   | 23
xz-man             |                []                                | 10
                   +--------------------------------------------------+
  87 teams           ta te tg th tr uk  ur vi wa wo zh_CN zh_HK zh_TW
 147 domains          8  1  0 10 47 124  1 83  4  1  92     4    40    2782
```

Some counters in the preceding matrix are higher than the number of visible
blocks let us expect. This is because a few extra PO files are used for
implementing regional variants of languages, or language dialects.

For a PO file in the matrix above to be effective, the package to which
it applies should also have been internationalized and distributed as
such by its maintainer. There might be an observable lag between the
mere existence a PO file and its wide availability in a distribution.

If January 2026 seems to be old, you may fetch a more recent copy
of this `ABOUT-NLS` file on most GNU archive sites. The most
up-to-date matrix with full percentage details can be found at
`https://translationproject.org/extra/matrix.html`.

---

Previous: [Available Packages](#Available-Packages), Up: [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

#### 18.2.5 Using `gettext` in new packages [¶](#Using-gettext-in-new-packages)

If you are writing a freely available program and want to internationalize
it you are welcome to use GNU `gettext` in your package. Of course
you have to respect the GNU Lesser General Public License which covers
the use of the GNU `gettext` library. This means in particular that
even non-free programs can use `libintl` as a shared library, whereas
only free software can use `libintl` as a static library or use
modified versions of `libintl`.

Once the sources are changed appropriately and the setup can handle the
use of `gettext` the only thing missing are the translations. The
Free Translation Project is also available for packages which are not
developed inside the GNU project. Therefore the information given above
applies also for every other Free Software Project. Contact
`coordinator@translationproject.org` to make the `.pot` files
available to the translation teams.

---

Previous: [Notes on the Free Translation Project](#The-original-ABOUT_002dNLS), Up: [Concluding Remarks](#Conclusion)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]

### 18.3 Related Readings [¶](#Related-Readings)

**NOTE:**  This documentation section is outdated and needs to be
revised.

Eugene H. Dorr (`dorre@well.com`) maintains an interesting
bibliography on internationalization matters, called
Internationalization Reference List, which is available as:

```
ftp://ftp.ora.com/pub/examples/nutshell/ujip/doc/i18n-books.txt
```

Michael Gschwind (`mike@vlsivie.tuwien.ac.at`) maintains a
Frequently Asked Questions (FAQ) list, entitled Programming for
Internationalisation. This FAQ discusses writing programs which
can handle different language conventions, character sets, etc.;
and is applicable to all character set encodings, with particular
emphasis on ISO 8859-1. It is regularly published in Usenet
groups `comp.unix.questions`, `comp.std.internat`,
`comp.software.international`, `comp.lang.c`,
`comp.windows.x`, `comp.std.c`, `comp.answers`
and `news.answers`. The home location of this document is:

```
ftp://ftp.vlsivie.tuwien.ac.at/pub/8bit/ISO-programming
```

Patrick D’Cruze (`pdcruze@li.org`) wrote a tutorial about NLS
matters, and Jochen Hein (`Hein@student.tu-clausthal.de`) took
over the responsibility of maintaining it. It may be found as:

```
ftp://sunsite.unc.edu/pub/Linux/utils/nls/catalogs/Incoming/...
     ...locale-tutorial-0.8.txt.gz
```

This site is mirrored in:

```
ftp://ftp.ibp.fr/pub/linux/sunsite/
```

A French version of the same tutorial should be findable at:

```
ftp://ftp.ibp.fr/pub/linux/french/docs/
```

together with French translations of many Linux-related documents.

---

Next: [Country Codes](#Country-Codes), Previous: [Concluding Remarks](#Conclusion), Up: [GNU `gettext` utilities](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]## Appendix A Language Codes [¶](#Language-Codes-1)

The ISO 639 standard defines two-letter codes for many languages, and
three-letter codes for more rarely used languages.
All abbreviations for languages used in the Translation Project should
come from this standard.

* [Usual Language Codes](#Usual-Language-Codes)
* [Rare Language Codes](#Rare-Language-Codes)

---

Next: [Rare Language Codes](#Rare-Language-Codes), Up: [Language Codes](#Language-Codes)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Program-Index "Index")]
