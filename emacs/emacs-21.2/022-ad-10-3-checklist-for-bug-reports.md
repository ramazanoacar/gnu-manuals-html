### AD.10.3 Checklist for Bug Reports

The best way to send a bug report is to mail it electronically to the
Emacs maintainers at [bug-gnu-emacs@gnu.org](mailto:bug-gnu-emacs@gnu.org), or to
[emacs-pretest-bug@gnu.org](mailto:emacs-pretest-bug@gnu.org) if you are pretesting an Emacs beta
release. (If you want to suggest a change as an improvement, use the
same address.)

If you'd like to read the bug reports, you can find them on the
newsgroup `` `gnu.emacs.bug' ``; keep in mind, however, that as a
spectator you should not criticize anything about what you see there.
The purpose of bug reports is to give information to the Emacs
maintainers. Spectators are welcome only as long as they do not
interfere with this. In particular, some bug reports contain fairly
large amounts of data; spectators should not complain about this.

Please do not post bug reports using netnews; mail is more reliable
than netnews about reporting your correct address, which we may need
in order to ask you for more information. If your data is more than
500,000 bytes, please don't include it directly in the bug report;
instead, offer to send it on request, or make it available by ftp and
say where.

If you can't send electronic mail, then mail the bug report on paper
or machine-readable media to this address:

|  |  |
| --- | --- |
|  | ``` GNU Emacs Bugs Free Software Foundation 59 Temple Place, Suite 330 Boston, MA 02111-1307 USA ``` |

We do not promise to fix the bug; but if the bug is serious,
or ugly, or easy to fix, chances are we will want to.

A convenient way to send a bug report for Emacs is to use the command
`M-x report-emacs-bug`. This sets up a mail buffer (see section [Z. Sending Mail](emacs.html#SEC363)) and automatically inserts *some* of the essential
information. However, it cannot supply all the necessary information;
you should still read and follow the guidelines below, so you can enter
the other crucial information by hand before you send the message.

To enable maintainers to investigate a bug, your report
should include all these things:

* The version number of Emacs. Without this, we won't know whether there
  is any point in looking for the bug in the current version of GNU
  Emacs.

  You can get the version number by typing `M-x emacs-version
  RET`. If that command does not work, you probably have something
  other than GNU Emacs, so you will have to report the bug somewhere
  else.

  * The type of machine you are using, and the operating system name and
    version number. `M-x emacs-version RET` provides this
    information too. Copy its output from the `` `*Messages*' `` buffer, so
    that you get it all and get it accurately.

    * The operands given to the `configure` command when Emacs was
      installed.

      * A complete list of any modifications you have made to the Emacs source.
        (We may not have time to investigate the bug unless it happens in an
        unmodified Emacs. But if you've made modifications and you don't tell
        us, you are sending us on a wild goose chase.)

        Be precise about these changes. A description in English is not
        enough--send a context diff for them.

        Adding files of your own, or porting to another machine, is a
        modification of the source.

        * Details of any other deviations from the standard procedure for installing
          GNU Emacs.

          * The complete text of any files needed to reproduce the bug.

            If you can tell us a way to cause the problem without visiting any files,
            please do so. This makes it much easier to debug. If you do need files,
            make sure you arrange for us to see their exact contents. For example, it
            can often matter whether there are spaces at the ends of lines, or a
            newline after the last line in the buffer (nothing ought to care whether
            the last line is terminated, but try telling the bugs that).

            * The precise commands we need to type to reproduce the bug.

              The easy way to record the input to Emacs precisely is to write a
              dribble file. To start the file, execute the Lisp expression

              |  |  |
              | --- | --- |
              |  | ``` (open-dribble-file "~/dribble") ``` |

              using `M-:` or from the `` `*scratch*' `` buffer just after
              starting Emacs. From then on, Emacs copies all your input to the
              specified dribble file until the Emacs process is killed.

              * For possible display bugs, the terminal type (the value of environment
                variable `TERM`), the complete termcap entry for the terminal from
                `/etc/termcap' (since that file is not identical on all machines),
                and the output that Emacs actually sent to the terminal.

                The way to collect the terminal output is to execute the Lisp expression

                |  |  |
                | --- | --- |
                |  | ``` (open-termscript "~/termscript") ``` |

                using `M-:` or from the `` `*scratch*' `` buffer just after
                starting Emacs. From then on, Emacs copies all terminal output to the
                specified termscript file as well, until the Emacs process is killed.
                If the problem happens when Emacs starts up, put this expression into
                your `.emacs' file so that the termscript file will be open when
                Emacs displays the screen for the first time.

                Be warned: it is often difficult, and sometimes impossible, to fix a
                terminal-dependent bug without access to a terminal of the type that
                stimulates the bug.

                * If non-ASCII text or internationalization is relevant, the locale that
                  was current when you started Emacs. On GNU/Linux and Unix systems, or
                  if you use a Posix-style shell such as Bash, you can use this shell
                  command to view the relevant values:

                  |  |  |
                  | --- | --- |
                  |  | ``` echo LC_ALL=$LC_ALL LC_COLLATE=$LC_COLLATE LC_TYPE=$LC_TYPE \   LC_MESSAGES=$LC_MESSAGES LC_TIME=$LC_TIME LANG=$LANG ``` |

                  Alternatively, use the `locale` command, if your system has it,
                  to display your locale settings.

                  You can use the `M-!` command to execute these commands from
                  Emacs, and then copy the output from the `` `*Messages*' `` buffer into
                  the bug report. Alternatively, `M-x getenv RET LC_ALL
                  RET` will display the value of `LC_ALL` in the echo area, and
                  you can copy its output from the `` `*Messages*' `` buffer.

                  * A description of what behavior you observe that you believe is
                    incorrect. For example, "The Emacs process gets a fatal signal," or,
                    "The resulting text is as follows, which I think is wrong."

                    Of course, if the bug is that Emacs gets a fatal signal, then one can't
                    miss it. But if the bug is incorrect text, the maintainer might fail to
                    notice what is wrong. Why leave it to chance?

                    Even if the problem you experience is a fatal signal, you should still
                    say so explicitly. Suppose something strange is going on, such as, your
                    copy of the source is out of sync, or you have encountered a bug in the
                    C library on your system. (This has happened!) Your copy might crash
                    and the copy here might not. If you *said* to expect a crash, then
                    when Emacs here fails to crash, we would know that the bug was not
                    happening. If you don't say to expect a crash, then we would not know
                    whether the bug was happening--we would not be able to draw any
                    conclusion from our observations.

                    * If the bug is that the Emacs Manual or the Emacs Lisp Reference Manual
                      fails to describe the actual behavior of Emacs, or that the text is
                      confusing, copy in the text from the online manual which you think is
                      at fault. If the section is small, just the section name is enough.

                      * If the manifestation of the bug is an Emacs error message, it is
                        important to report the precise text of the error message, and a
                        backtrace showing how the Lisp program in Emacs arrived at the error.

                        To get the error message text accurately, copy it from the
                        `` `*Messages*' `` buffer into the bug report. Copy all of it, not just
                        part.

                        To make a backtrace for the error, use `M-x toggle-debug-on-error`
                        before the error happens (that is to say, you must give that command
                        and then make the bug happen). This causes the error to run the Lisp
                        debugger, which shows you a backtrace. Copy the text of the
                        debugger's backtrace into the bug report. See section `The Lisp Debugger' in the Emacs Lisp Reference Manual, for information on
                        debugging Emacs Lisp programs with the Edebug package.

                        This use of the debugger is possible only if you know how to make the
                        bug happen again. If you can't make it happen again, at least copy
                        the whole error message.

                        * Check whether any programs you have loaded into the Lisp world,
                          including your `.emacs' file, set any variables that may affect the
                          functioning of Emacs. Also, see whether the problem happens in a
                          freshly started Emacs without loading your `.emacs' file (start
                          Emacs with the `-q` switch to prevent loading the init file). If
                          the problem does *not* occur then, you must report the precise
                          contents of any programs that you must load into the Lisp world in order
                          to cause the problem to occur.

                          * If the problem does depend on an init file or other Lisp programs that
                            are not part of the standard Emacs system, then you should make sure it
                            is not a bug in those programs by complaining to their maintainers
                            first. After they verify that they are using Emacs in a way that is
                            supposed to work, they should report the bug.

                            * If you wish to mention something in the GNU Emacs source, show the line
                              of code with a few lines of context. Don't just give a line number.

                              The line numbers in the development sources don't match those in your
                              sources. It would take extra work for the maintainers to determine what
                              code is in your version at a given line number, and we could not be
                              certain.

                              * Additional information from a C debugger such as GDB might enable
                                someone to find a problem on a machine which he does not have available.
                                If you don't know how to use GDB, please read the GDB manual--it is not
                                very long, and using GDB is easy. You can find the GDB distribution,
                                including the GDB manual in online form, in most of the same places you
                                can find the Emacs distribution. To run Emacs under GDB, you should
                                switch to the `src' subdirectory in which Emacs was compiled, then
                                do `` `gdb emacs' ``. It is important for the directory `src' to be
                                current so that GDB will read the `.gdbinit' file in this
                                directory.

                                However, you need to think when you collect the additional information
                                if you want it to show what causes the bug.

                                For example, many people send just a backtrace, but that is not very
                                useful by itself. A simple backtrace with arguments often conveys
                                little about what is happening inside GNU Emacs, because most of the
                                arguments listed in the backtrace are pointers to Lisp objects. The
                                numeric values of these pointers have no significance whatever; all that
                                matters is the contents of the objects they point to (and most of the
                                contents are themselves pointers).

                                To provide useful information, you need to show the values of Lisp
                                objects in Lisp notation. Do this for each variable which is a Lisp
                                object, in several stack frames near the bottom of the stack. Look at
                                the source to see which variables are Lisp objects, because the debugger
                                thinks of them as integers.

                                To show a variable's value in Lisp syntax, first print its value, then
                                use the user-defined GDB command `pr` to print the Lisp object in
                                Lisp syntax. (If you must use another debugger, call the function
                                `debug_print` with the object as an argument.) The `pr`
                                command is defined by the file `.gdbinit', and it works only if you
                                are debugging a running process (not with a core dump).

                                To make Lisp errors stop Emacs and return to GDB, put a breakpoint at
                                `Fsignal`.

                                For a short listing of Lisp functions running, type the GDB
                                command `xbacktrace`.

                                The file `.gdbinit' defines several other commands that are useful
                                for examining the data types and contents of Lisp objects. Their names
                                begin with `` `x' ``. These commands work at a lower level than
                                `pr`, and are less convenient, but they may work even when
                                `pr` does not, such as when debugging a core dump or when Emacs has
                                had a fatal signal.

                                More detailed advice and other useful techniques for debugging Emacs
                                are available in the file `etc/DEBUG' in the Emacs distribution.
                                That file also includes instructions for investigating problems
                                whereby Emacs stops responding (many people assume that Emacs is
                                "hung," whereas in fact it might be in an infinite loop).

                                To find the file `etc/DEBUG' in your Emacs installation, use the
                                directory name stored in the variable `data-directory`.

Here are some things that are not necessary in a bug report:

* A description of the envelope of the bug--this is not necessary for a
  reproducible bug.

  Often people who encounter a bug spend a lot of time investigating
  which changes to the input file will make the bug go away and which
  changes will not affect it.

  This is often time-consuming and not very useful, because the way we
  will find the bug is by running a single example under the debugger
  with breakpoints, not by pure deduction from a series of examples.
  You might as well save time by not searching for additional examples.
  It is better to send the bug report right away, go back to editing,
  and find another bug to report.

  Of course, if you can find a simpler example to report *instead* of
  the original one, that is a convenience. Errors in the output will be
  easier to spot, running under the debugger will take less time, etc.

  However, simplification is not vital; if you can't do this or don't have
  time to try, please report the bug with your original test case.

  * A core dump file.

    Debugging the core dump might be useful, but it can only be done on
    your machine, with your Emacs executable. Therefore, sending the core
    dump file to the Emacs maintainers won't be useful. Above all, don't
    include the core file in an email bug report! Such a large message
    can be extremely inconvenient.

    * A system-call trace of Emacs execution.

      System-call traces are very useful for certain special kinds of
      debugging, but in most cases they give little useful information. It is
      therefore strange that many people seem to think that *the* way to
      report information about a crash is to send a system-call trace. Perhaps
      this is a habit formed from experience debugging programs that don't
      have source code or debugging symbols.

      In most programs, a backtrace is normally far, far more informative than
      a system-call trace. Even in Emacs, a simple backtrace is generally
      more informative, though to give full information you should supplement
      the backtrace by displaying variable values and printing them as Lisp
      objects with `pr` (see above).

      * A patch for the bug.

        A patch for the bug is useful if it is a good one. But don't omit the
        other information that a bug report needs, such as the test case, on the
        assumption that a patch is sufficient. We might see problems with your
        patch and decide to fix the problem another way, or we might not
        understand it at all. And if we can't understand what bug you are
        trying to fix, or why your patch should be an improvement, we mustn't
        install it.

        See section [AD.10.4 Sending Patches for GNU Emacs](emacs.html#SEC525), for guidelines on how to make it easy for us to
        understand and install your patches.

        * A guess about what the bug is or what it depends on.

          Such guesses are usually wrong. Even experts can't guess right about
          such things without first using the debugger to find the facts.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC524) ] | [ [>](emacs.html#SEC526) ] | [ [<<](emacs.html#SEC521) ] | [ [Up](emacs.html#SEC521) ] | [ [>>](emacs.html#SEC526) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AD.10.4 Sending Patches for GNU Emacs

If you would like to write bug fixes or improvements for GNU Emacs,
that is very helpful. When you send your changes, please follow these
guidelines to make it easy for the maintainers to use them. If you
don't follow these guidelines, your information might still be useful,
but using it will take extra work. Maintaining GNU Emacs is a lot of
work in the best of circumstances, and we can't keep up unless you do
your best to help.

* Send an explanation with your changes of what problem they fix or what
  improvement they bring about. For a bug fix, just include a copy of the
  bug report, and explain why the change fixes the bug.

  (Referring to a bug report is not as good as including it, because then
  we will have to look it up, and we have probably already deleted it if
  we've already fixed the bug.)

  * Always include a proper bug report for the problem you think you have
    fixed. We need to convince ourselves that the change is right before
    installing it. Even if it is correct, we might have trouble
    understanding it if we don't have a way to reproduce the problem.

    * Include all the comments that are appropriate to help people reading the
      source in the future understand why this change was needed.

      * Don't mix together changes made for different reasons.
        Send them *individually*.

        If you make two changes for separate reasons, then we might not want to
        install them both. We might want to install just one. If you send them
        all jumbled together in a single set of diffs, we have to do extra work
        to disentangle them--to figure out which parts of the change serve
        which purpose. If we don't have time for this, we might have to ignore
        your changes entirely.

        If you send each change as soon as you have written it, with its own
        explanation, then two changes never get tangled up, and we can consider
        each one properly without any extra work to disentangle them.

        * Send each change as soon as that change is finished. Sometimes people
          think they are helping us by accumulating many changes to send them all
          together. As explained above, this is absolutely the worst thing you
          could do.

          Since you should send each change separately, you might as well send it
          right away. That gives us the option of installing it immediately if it
          is important.

          * Use `` `diff -c' `` to make your diffs. Diffs without context are hard
            to install reliably. More than that, they are hard to study; we must
            always study a patch to decide whether we want to install it. Unidiff
            format is better than contextless diffs, but not as easy to read as
            `` `-c' `` format.

            If you have GNU diff, use `` `diff -c -F'^[_a-zA-Z0-9$]+ *('' `` when
            making diffs of C code. This shows the name of the function that each
            change occurs in.

            * Avoid any ambiguity as to which is the old version and which is the new.
              Please make the old version the first argument to diff, and the new
              version the second argument. And please give one version or the other a
              name that indicates whether it is the old version or your new changed
              one.

              * Write the change log entries for your changes. This is both to save us
                the extra work of writing them, and to help explain your changes so we
                can understand them.

                The purpose of the change log is to show people where to find what was
                changed. So you need to be specific about what functions you changed;
                in large functions, it's often helpful to indicate where within the
                function the change was.

                On the other hand, once you have shown people where to find the change,
                you need not explain its purpose in the change log. Thus, if you add a
                new function, all you need to say about it is that it is new. If you
                feel that the purpose needs explaining, it probably does--but put the
                explanation in comments in the code. It will be more useful there.

                Please read the `ChangeLog' files in the `src' and `lisp'
                directories to see what sorts of information to put in, and to learn the
                style that we use. If you would like your name to appear in the header
                line, showing who made the change, send us the header line.
                See section [W.1 Change Logs](emacs.html#SEC333).

                * When you write the fix, keep in mind that we can't install a change that
                  would break other systems. Please think about what effect your change
                  will have if compiled on another type of system.

                  Sometimes people send fixes that *might* be an improvement in
                  general--but it is hard to be sure of this. It's hard to install
                  such changes because we have to study them very carefully. Of course,
                  a good explanation of the reasoning by which you concluded the change
                  was correct can help convince us.

                  The safest changes are changes to the configuration files for a
                  particular machine. These are safe because they can't create new bugs
                  on other machines.

                  Please help us keep up with the workload by designing the patch in a
                  form that is clearly safe to install.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC525) ] | [ [>](emacs.html#SEC527) ] | [ [<<](emacs.html#SEC521) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC527) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.11 Contributing to Emacs Development

If you would like to help pretest Emacs releases to assure they work
well, or if you would like to work on improving Emacs, please contact
the maintainers at [bug-gnu-emacs@gnu.org](mailto:bug-gnu-emacs@gnu.org). A pretester
should be prepared to investigate bugs as well as report them. If you'd
like to work on improving Emacs, please ask for suggested projects or
suggest your own ideas.

If you have already written an improvement, please tell us about it. If
you have not yet started work, it is useful to contact
[bug-gnu-emacs@gnu.org](mailto:bug-gnu-emacs@gnu.org) before you start; it might be
possible to suggest ways to make your extension fit in better with the
rest of Emacs.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC526) ] | [ [>](emacs.html#SEC528) ] | [ [<<](emacs.html#SEC526) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC528) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AD.12 How To Get Help with GNU Emacs

If you need help installing, using or changing GNU Emacs, there are two
ways to find it:

* Send a message to the mailing list
  [help-gnu-emacs@gnu.org](mailto:help-gnu-emacs@gnu.org), or post your request on
  newsgroup `gnu.emacs.help`. (This mailing list and newsgroup
  interconnect, so it does not matter which one you use.)

  * Look in the service directory for someone who might help you for a fee.
    The service directory is found in the file named `etc/SERVICE' in the
    Emacs distribution.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC527) ] | [ [>](emacs.html#SEC529) ] | [ [<<](emacs.html#SEC527) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# AE. Command Line Arguments

GNU Emacs supports command line arguments to request various actions
when invoking Emacs. These are for compatibility with other editors and
for sophisticated activities. We don't recommend using them for
ordinary editing.

Arguments starting with `` `-' `` are *options*. Other arguments
specify files to visit. Emacs visits the specified files while it
starts up. The last file name on your command line becomes the
current buffer; the other files are also visited in other buffers. If
there are two files, they are both displayed; otherwise the last file
is displayed along with a buffer list that shows what other buffers
there are. As with most programs, the special argument `` `--' `` says
that all subsequent arguments are file names, not options, even if
they start with `` `-' ``.

Emacs command options can specify many things, such as the size and
position of the X window Emacs uses, its colors, and so on. A few
options support advanced usage, such as running Lisp functions on files
in batch mode. The sections of this chapter describe the available
options, arranged according to their purpose.

There are two ways of writing options: the short forms that start with
a single `` `-' ``, and the long forms that start with `` `--' ``. For
example, `` `-d' `` is a short form and `` `--display' `` is the
corresponding long form.

The long forms with `` `--' `` are easier to remember, but longer to
type. However, you don't have to spell out the whole option name; any
unambiguous abbreviation is enough. When a long option takes an
argument, you can use either a space or an equal sign to separate the
option name and the argument. Thus, you can write either
`` `--display sugar-bombs:0.0' `` or `` `--display=sugar-bombs:0.0' ``.
We recommend an equal sign because it makes the relationship clearer,
and the tables below always show an equal sign.

Most options specify how to initialize Emacs, or set parameters for
the Emacs session. We call them *initial options*. A few options
specify things to do: for example, load libraries, call functions, or
terminate Emacs. These are called *action options*. These and file
names together are called *action arguments*. Emacs processes all
the action arguments in the order they are written.

> |  |  |  |
> | --- | --- | --- |
> | [AE.1 Action Arguments](emacs.html#SEC529) |  | Arguments to visit files, load libraries, and call functions. |
> | [AE.2 Initial Options](emacs.html#SEC530) |  | Arguments that take effect while starting Emacs. |
> | [AE.3 Command Argument Example](emacs.html#SEC531) |  | Examples of using command line arguments. |
> | [AE.4 Resuming Emacs with Arguments](emacs.html#SEC532) |  | Specifying arguments when you resume a running Emacs. |
> | [AE.5 Environment Variables](emacs.html#SEC533) |  | Environment variables that Emacs uses. |
>
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AE.6 Specifying the Display Name](emacs.html#SEC536) |  | Changing the default display and using remote login. |
> | [AE.7 Font Specification Options](emacs.html#SEC537) |  | Choosing a font for text, under X. |
> | [AE.8 Window Color Options](emacs.html#SEC538) |  | Choosing colors, under X. |
> | [AE.9 Options for Window Geometry](emacs.html#SEC539) |  | Start-up window size, under X. |
> | [AE.10 Internal and External Borders](emacs.html#SEC540) |  | Internal and external borders, under X. |
> | [AE.11 Frame Titles](emacs.html#SEC541) |  | Specifying the initial frame's title. |
> | [AE.12 Icons](emacs.html#SEC542) |  | Choosing what sort of icon to use, under X. |
> | [AE.13 X Resources](emacs.html#SEC543) |  | Advanced use of classes and resources, under X. |
> | [AE.14 Lucid Menu X Resources](emacs.html#SEC544) |  | X resources for Lucid menus. |
> | [AE.15 LessTif Menu X Resources](emacs.html#SEC545) |  | X resources for LessTif and Motif menus. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC528) ] | [ [>](emacs.html#SEC530) ] | [ [<<](emacs.html#SEC528) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.1 Action Arguments

Here is a table of the action arguments and options:

`` `file' ``: `` `--visit=file' ``: `` `--file=file' ``: Visit file using `find-file`. See section [M.2 Visiting Files](emacs.html#SEC112). `` `+linenum file' ``: Visit file using `find-file`, then go to line number linenum in it. `` `+linenum:columnnum file' ``: Visit file using `find-file`, then go to line number linenum and put point at column number columnnum. `` `-l file' ``: `` `--load=file' ``: Load a Lisp library named file with the function `load`. See section [V.7 Libraries of Lisp Code for Emacs](emacs.html#SEC328). The library can be found either in the current directory, or in the Emacs library search path as specified with `EMACSLOADPATH` (see section [AE.5.1 General Variables](emacs.html#SEC534)). `` `-f function' ``: `` `--funcall=function' ``: Call Lisp function function with no arguments. `` `--eval=expression' ``: `` `--execute=expression' ``: Evaluate Lisp expression expression. `` `--insert=file' ``: Insert the contents of file into the current buffer. This is like what `M-x insert-file` does. See section [M.10 Miscellaneous File Operations](emacs.html#SEC165). `` `--kill' ``: Exit from Emacs without asking for confirmation.

The init file can access the values of the action arguments as the
elements of a list in the variable `command-line-args`. The init
file can override the normal processing of the action arguments, or
define new ones, by reading and setting this variable.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC529) ] | [ [>](emacs.html#SEC531) ] | [ [<<](emacs.html#SEC531) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.2 Initial Options

The initial options specify parameters for the Emacs session. This
section describes the more general initial options; some other options
specifically related to the X Window System appear in the following
sections.

Some initial options affect the loading of init files. The normal
actions of Emacs are to first load `site-start.el' if it exists,
then your own init file `~/.emacs' if it exists, and finally
`default.el' if it exists; certain options prevent loading of some
of these files or substitute other files for them.

`` `-t device' ``: `` `--terminal=device' ``: Use device as the device for terminal input and output. `` `-d display' ``: `` `--display=display' ``: Use the X Window System and use the display named display to open the initial Emacs frame. See section [AE.6 Specifying the Display Name](emacs.html#SEC536), for more details. `` `-nw' ``: `` `--no-windows' ``: Don't communicate directly with the window system, disregarding the `DISPLAY` environment variable even if it is set. This forces Emacs to run as if the display were a text-only terminal. `` `-batch' ``: `` `--batch' ``: Run Emacs in *batch mode*, which means that the text being edited is not displayed and the standard terminal interrupt characters such as `C-z` and `C-c` continue to have their normal effect. Emacs in batch mode outputs to `stderr` only what would normally be displayed in the echo area under program control, and functions which would normally read from the minibuffer take their input from `stdin`. Batch mode is used for running programs written in Emacs Lisp from shell scripts, makefiles, and so on. Normally the `` `-l' `` option or `` `-f' `` option will be used as well, to invoke a Lisp program to do the batch processing. `` `-batch' `` implies `` `-q' `` (do not load an init file). It also causes Emacs to exit after processing all the command options. In addition, it disables auto-saving except in buffers for which it has been explicitly requested. `` `-q' ``: `` `--no-init-file' ``: Do not load your Emacs init file `~/.emacs', or `default.el' either. When invoked like this, Emacs does not allow saving options changed with the `M-x customize` command and its variants. See section [AD.2.2 Easy Customization Interface](emacs.html#SEC479). `` `--no-site-file' ``: Do not load `site-start.el'. The options `` `-q' ``, `` `-u' `` and `` `-batch' `` have no effect on the loading of this file--this is the only option that blocks it. `` `-u user' ``: `` `--user=user' ``: Load user's Emacs init file `~user/.emacs' instead of your own. `` `--debug-init' ``: Enable the Emacs Lisp debugger for errors in the init file. `` `--unibyte' ``: Do almost everything with single-byte buffers and strings. All buffers and strings are unibyte unless you (or a Lisp program) explicitly ask for a multibyte buffer or string. (Note that Emacs always loads Lisp files in multibyte mode, even if `` `--unibyte' `` is specified; see [Q.2 Enabling Multibyte Characters](emacs.html#SEC215).) Setting the environment variable `EMACS_UNIBYTE` has the same effect. `` `--multibyte' ``: Inhibit the effect of `EMACS_UNIBYTE`, so that Emacs uses multibyte characters by default, as usual.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC530) ] | [ [>](emacs.html#SEC532) ] | [ [<<](emacs.html#SEC532) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.3 Command Argument Example

Here is an example of using Emacs with arguments and options. It
assumes you have a Lisp program file called `hack-c.el' which, when
loaded, performs some useful operation on the current buffer, expected
to be a C program.

|  |  |
| --- | --- |
|  | ``` emacs -batch foo.c -l hack-c -f save-buffer >& log ``` |

This says to visit `foo.c', load `hack-c.el' (which makes
changes in the visited file), save `foo.c' (note that
`save-buffer` is the function that `C-x C-s` is bound to), and
then exit back to the shell (because of `` `-batch' ``). `` `-batch' ``
also guarantees there will be no problem redirecting output to
`log', because Emacs will not assume that it has a display terminal
to work with.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC531) ] | [ [>](emacs.html#SEC533) ] | [ [<<](emacs.html#SEC533) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC546) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.4 Resuming Emacs with Arguments

You can specify action arguments for Emacs when you resume it after
a suspension. To prepare for this, put the following code in your
`.emacs' file (see section [AD.2.3 Hooks](emacs.html#SEC484)):

|  |  |
| --- | --- |
|  | ``` (add-hook 'suspend-hook 'resume-suspend-hook) (add-hook 'suspend-resume-hook 'resume-process-args) ``` |

As further preparation, you must execute the shell script
`emacs.csh' (if you use csh as your shell) or `emacs.bash'
(if you use bash as your shell). These scripts define an alias named
`edit`, which will resume Emacs giving it new command line
arguments such as files to visit. The scripts are found in the
`etc' subdirectory of the Emacs distribution.

Only action arguments work properly when you resume Emacs. Initial
arguments are not recognized--it's too late to execute them anyway.

Note that resuming Emacs (with or without arguments) must be done from
within the shell that is the parent of the Emacs job. This is why
`edit` is an alias rather than a program or a shell script. It is
not possible to implement a resumption command that could be run from
other subjobs of the shell; there is no way to define a command that could
be made the value of `EDITOR`, for example. Therefore, this feature
does not take the place of the Emacs Server feature (see section [AC.16 Using Emacs as a Server](emacs.html#SEC456)).

The aliases use the Emacs Server feature if you appear to have a
server Emacs running. However, they cannot determine this with complete
accuracy. They may think that a server is still running when in
actuality you have killed that Emacs, because the file
`/tmp/esrv...' still exists. If this happens, find that
file and delete it.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC532) ] | [ [>](emacs.html#SEC534) ] | [ [<<](emacs.html#SEC536) ] | [ [Up](emacs.html#SEC528) ] | [ [>>](emacs.html#SEC536) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## AE.5 Environment Variables

The *environment* is a feature of the operating system; it
consists of a collection of variables with names and values. Each
variable is called an *environment variable*; environment variable
names are case-sensitive, and it is conventional to use upper case
letters only. The values are all text strings.

What makes the environment useful is that subprocesses inherit the
environment automatically from their parent process. This means you
can set up an environment variable in your login shell, and all the
programs you run (including Emacs) will automatically see it.
Subprocesses of Emacs (such as shells, compilers, and version-control
software) inherit the environment from Emacs, too.

Inside Emacs, the command `M-x getenv` gets the value of an
environment variable. `M-x setenv` sets a variable in the Emacs
environment. The way to set environment variables outside of Emacs
depends on the operating system, and especially the shell that you are
using. For example, here's how to set the environment variable
`ORGANIZATION` to `` `not very much' `` using Bash:

|  |  |
| --- | --- |
|  | ``` export ORGANIZATION="not very much" ``` |

and here's how to do it in csh or tcsh:

|  |  |
| --- | --- |
|  | ``` setenv ORGANIZATION "not very much" ``` |

When Emacs is uses the X Window System, it inherits the use
of a large number of environment variables from the X libraries. See
the X documentation for more information.

> |  |  |  |
> | --- | --- | --- |
> | [AE.5.1 General Variables](emacs.html#SEC534) |  | Environment variables that all versions of Emacs use. |
> | [AE.5.2 Miscellaneous Variables](emacs.html#SEC535) |  | Certain system-specific variables. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC533) ] | [ [>](emacs.html#SEC535) ] | [ [<<](emacs.html#SEC536) ] | [ [Up](emacs.html#SEC533) ] | [ [>>](emacs.html#SEC536) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |

### AE.5.1 General Variables

Here is an alphabetical list of specific environment variables that
have special meanings in Emacs, giving the name of each variable and
its meaning. Most of these variables are also used by some other
programs. Emacs does not require any of these environment variables
to be set, but it uses their values if they are set.

`CDPATH`: Used by the `cd` command to search for the directory you specify, when you specify a relative directory name. `EMACS_UNIBYTE`: Defining this environment variable with a nonempty value directs Emacs to do almost everything with single-byte buffers and strings. It is equivalent to using the `` `--unibyte' `` command-line option on each invocation. See section [AE.2 Initial Options](emacs.html#SEC530). `EMACSDATA`: Directory for the architecture-independent files that come with Emacs. This is used to initialize the Lisp variable `data-directory`. `EMACSDOC`: Directory for the documentation string file, `DOC-emacsversion'. This is used to initialize the Lisp variable `doc-directory`. `EMACSLOADPATH`: A colon-separated list of directories[(17)](emacs.html#FOOT17) to search for Emacs Lisp files--used to initialize `load-path`. `EMACSPATH`: A colon-separated list of directories to search for executable files--used to initialize `exec-path`. `ESHELL`: Used for shell-mode to override the `SHELL` environment variable. `HISTFILE`: The name of the file that shell commands are saved in between logins. This variable defaults to `~/.bash\_history' if you use Bash, to `~/.sh\_history' if you use ksh, and to `~/.history' otherwise. `HOME`: The location of the user's files in the directory tree; used for expansion of file names starting with a tilde (`~'). On MS-DOS, it defaults to the directory from which Emacs was started, with `` `/bin' `` removed from the end if it was present. On Windows, the default value of `HOME` is `C:/', the root directory of drive `C:'. `HOSTNAME`: The name of the machine that Emacs is running on. `INCPATH`: A colon-separated list of directories. Used by the `complete` package to search for files. `INFOPATH`: A colon-separated list of directories in which to search for Info files. `LC_ALL`: `LC_COLLATE`: `LC_CTYPE`: `LC_MESSAGES`: `LC_MONETARY`: `LC_NUMERIC`: `LC_TIME`: `LANG`: The user's preferred locale. The locale has six categories, specified by the environment variables `LC_COLLATE` for sorting, `LC_CTYPE` for character encoding, `LC_MESSAGES` for system messages, `LC_MONETARY` for monetary formats, `LC_NUMERIC` for numbers, and `LC_TIME` for dates and times. If one of these variables is not set, the category defaults to the value of the `LANG` environment variable, or to the default `` `C' `` locale if `LANG` is not set. But if `LC_ALL` is specified, it overrides the settings of all the other locale environment variables. The value of the LC\_CTYPE category is matched against entries in `locale-language-names`, `locale-charset-language-names`, and `locale-preferred-coding-systems`, to select a default language environment and coding system. See section [Q.3 Language Environments](emacs.html#SEC216). `LOGNAME`: The user's login name. See also `USER`. `MAIL`: The name of the user's system mail inbox. `MAILRC`: Name of file containing mail aliases. (The default is `~/.mailrc'.) `MH`: Name of setup file for the mh system. (The default is `~/.mh\_profile'.) `NAME`: The real-world name of the user. `NNTPSERVER`: The name of the news server. Used by the mh and Gnus packages. `ORGANIZATION`: The name of the organization to which you belong. Used for setting the `Organization:' header in your posts from the Gnus package. `PATH`: A colon-separated list of directories in which executables reside. This is used to initialize the Emacs Lisp variable `exec-path`. `PWD`: If set, this should be the default directory when Emacs was started. `REPLYTO`: If set, this specifies an initial value for the variable `mail-default-reply-to`. See section [Z.2 Mail Header Fields](emacs.html#SEC365). `SAVEDIR`: The name of a directory in which news articles are saved by default. Used by the Gnus package. `SHELL`: The name of an interpreter used to parse and execute programs run from inside Emacs. `TERM`: The type of the terminal that Emacs is using. This variable must be set unless Emacs is run in batch mode. On MS-DOS, it defaults to `` `internal' ``, which specifies a built-in terminal emulation that handles the machine's own display. If the value of `TERM` indicates that Emacs runs in non-windowed mode from `xterm` or a similar terminal emulator, the background mode defaults to `` `light' ``, and Emacs will choose colors that are appropriate for a light background. `TERMCAP`: The name of the termcap library file describing how to program the terminal specified by the `TERM` variable. This defaults to `/etc/termcap'. `TMPDIR`: Used by the Emerge package as a prefix for temporary files. `TZ`: This specifies the current time zone and possibly also daylight saving time information. On MS-DOS, if `TZ` is not set in the environment when Emacs starts, Emacs defines a default value as appropriate for the country code returned by DOS. On MS-Windows, Emacs does not use `TZ` at all. `USER`: The user's login name. See also `LOGNAME`. On MS-DOS, this defaults to `` `root' ``. `VERSION_CONTROL`: Used to initialize the `version-control` variable (see section [M.3.1.1 Single or Numbered Backups](emacs.html#SEC115)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC534) ] | [ [>](emacs.html#SEC536) ] | [ [<<](emacs.html#SEC536) ] | [ [Up](emacs.html#SEC533) ] | [ [>>](emacs.html#SEC536) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
