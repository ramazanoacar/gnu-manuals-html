# The Gnus Newsreader [¶](#The-Gnus-Newsreader)## Short Table of Contents

* [1 Don’t Panic](#toc-Don_0027t-Panic-1)
* [2 Starting Gnus](#toc-Starting-Gnus)
* [3 Group Buffer](#toc-Group-Buffer-1)
* [4 Summary Buffer](#toc-Summary-Buffer-1)
* [5 Article Buffer](#toc-Article-Buffer-1)
* [6 Composing Messages](#toc-Composing-Messages-1)
* [7 Select Methods](#toc-Select-Methods-1)
* [8 Scoring](#toc-Scoring-1)
* [9 Searching](#toc-Searching-1)
* [10 Various](#toc-Various-1)
* [11 The End](#toc-The-End-1)
* [12 Appendices](#toc-Appendices-1)
* [13 GNU Free Documentation License](#toc-GNU-Free-Documentation-License-1)
* [14 Index](#toc-Index-1)
* [15 Key Index](#toc-Key-Index-1)## Table of Contents

* [1 Don’t Panic](#Don_0027t-Panic)
* [2 Starting Gnus](#Starting-Up)
  + [2.1 Finding the News](#Finding-the-News)
  + [2.2 The Server is Down](#The-Server-is-Down)
  + [2.3 Child Gnusae](#Child-Gnusae)
  + [2.4 New Groups](#New-Groups)
    - [2.4.1 Checking New Groups](#Checking-New-Groups)
    - [2.4.2 Subscription Methods](#Subscription-Methods)
    - [2.4.3 Filtering New Groups](#Filtering-New-Groups)
  + [2.5 Changing Servers](#Changing-Servers)
  + [2.6 Startup Files](#Startup-Files)
  + [2.7 Auto Save](#Auto-Save)
  + [2.8 The Active File](#The-Active-File)
  + [2.9 Startup Variables](#Startup-Variables)
* [3 Group Buffer](#Group-Buffer)
  + [3.1 Group Buffer Format](#Group-Buffer-Format)
    - [3.1.1 Group Line Specification](#Group-Line-Specification)
    - [3.1.2 Group Mode Line Specification](#Group-Mode-Line-Specification)
    - [3.1.3 Group Highlighting](#Group-Highlighting)
  + [3.2 Group Maneuvering](#Group-Maneuvering)
  + [3.3 Selecting a Group](#Selecting-a-Group)
  + [3.4 Subscription Commands](#Subscription-Commands)
  + [3.5 Group Data](#Group-Data)
  + [3.6 Group Levels](#Group-Levels)
  + [3.7 Group Score](#Group-Score)
  + [3.8 Marking Groups](#Marking-Groups)
  + [3.9 Foreign Groups](#Foreign-Groups)
  + [3.10 Group Parameters](#Group-Parameters)
  + [3.11 Listing Groups](#Listing-Groups)
  + [3.12 Sorting Groups](#Sorting-Groups)
  + [3.13 Group Maintenance](#Group-Maintenance)
  + [3.14 Browse Foreign Server](#Browse-Foreign-Server)
  + [3.15 Exiting Gnus](#Exiting-Gnus)
  + [3.16 Group Topics](#Group-Topics)
    - [3.16.1 Topic Commands](#Topic-Commands)
    - [3.16.2 Topic Variables](#Topic-Variables)
    - [3.16.3 Topic Sorting](#Topic-Sorting)
    - [3.16.4 Topic Topology](#Topic-Topology)
    - [3.16.5 Topic Parameters](#Topic-Parameters)
  + [3.17 Accessing groups of non-English names](#Non_002dASCII-Group-Names)
  + [3.18 Misc Group Stuff](#Misc-Group-Stuff)
    - [3.18.1 Scanning New Messages](#Scanning-New-Messages)
    - [3.18.2 Group Information](#Group-Information)
    - [3.18.3 Group Timestamp](#Group-Timestamp)
    - [3.18.4 File Commands](#File-Commands)
    - [3.18.5 Sieve Commands](#Sieve-Commands)
* [4 Summary Buffer](#Summary-Buffer)
  + [4.1 Summary Buffer Format](#Summary-Buffer-Format)
    - [4.1.1 Summary Buffer Lines](#Summary-Buffer-Lines)
    - [4.1.2 To From Newsgroups](#To-From-Newsgroups)
    - [4.1.3 Summary Buffer Mode Line](#Summary-Buffer-Mode-Line)
    - [4.1.4 Summary Highlighting](#Summary-Highlighting)
  + [4.2 Summary Maneuvering](#Summary-Maneuvering)
  + [4.3 Choosing Articles](#Choosing-Articles)
    - [4.3.1 Choosing Commands](#Choosing-Commands)
    - [4.3.2 Choosing Variables](#Choosing-Variables)
  + [4.4 Scrolling the Article](#Paging-the-Article)
  + [4.5 Reply, Followup and Post](#Reply-Followup-and-Post)
    - [4.5.1 Summary Mail Commands](#Summary-Mail-Commands)
    - [4.5.2 Summary Post Commands](#Summary-Post-Commands)
    - [4.5.3 Summary Message Commands](#Summary-Message-Commands)
    - [4.5.4 Canceling Articles](#Canceling-and-Superseding)
  + [4.6 Delayed Articles](#Delayed-Articles)
  + [4.7 Marking Articles](#Marking-Articles)
    - [4.7.1 Unread Articles](#Unread-Articles)
    - [4.7.2 Read Articles](#Read-Articles)
    - [4.7.3 Other Marks](#Other-Marks)
    - [4.7.4 Setting Marks](#Setting-Marks)
    - [4.7.5 Generic Marking Commands](#Generic-Marking-Commands)
    - [4.7.6 Setting Process Marks](#Setting-Process-Marks)
  + [4.8 Limiting](#Limiting)
  + [4.9 Threading](#Threading)
    - [4.9.1 Customizing Threading](#Customizing-Threading)
      * [4.9.1.1 Loose Threads](#Loose-Threads)
      * [4.9.1.2 Filling In Threads](#Filling-In-Threads)
      * [4.9.1.3 More Threading](#More-Threading)
      * [4.9.1.4 Low-Level Threading](#Low_002dLevel-Threading)
    - [4.9.2 Thread Commands](#Thread-Commands)
  + [4.10 Sorting the Summary Buffer](#Sorting-the-Summary-Buffer)
  + [4.11 Asynchronous Article Fetching](#Asynchronous-Fetching)
  + [4.12 Article Caching](#Article-Caching)
  + [4.13 Persistent Articles](#Persistent-Articles)
  + [4.14 Sticky Articles](#Sticky-Articles)
  + [4.15 Article Backlog](#Article-Backlog)
  + [4.16 Saving Articles](#Saving-Articles)
  + [4.17 Decoding Articles](#Decoding-Articles)
    - [4.17.1 Uuencoded Articles](#Uuencoded-Articles)
    - [4.17.2 Shell Archives](#Shell-Archives)
    - [4.17.3 PostScript Files](#PostScript-Files)
    - [4.17.4 Other Files](#Other-Files)
    - [4.17.5 Decoding Variables](#Decoding-Variables)
      * [4.17.5.1 Rule Variables](#Rule-Variables)
      * [4.17.5.2 Other Decode Variables](#Other-Decode-Variables)
      * [4.17.5.3 Uuencoding and Posting](#Uuencoding-and-Posting)
    - [4.17.6 Viewing Files](#Viewing-Files)
  + [4.18 Article Treatment](#Article-Treatment)
    - [4.18.1 Article Highlighting](#Article-Highlighting)
    - [4.18.2 Article Fontifying](#Article-Fontifying)
    - [4.18.3 Article Hiding](#Article-Hiding)
    - [4.18.4 Article Washing](#Article-Washing)
    - [4.18.5 Article Header](#Article-Header)
    - [4.18.6 Article Buttons](#Article-Buttons)
      * [4.18.6.1 Related variables and functions](#Related-variables-and-functions)
    - [4.18.7 Article button levels](#Article-Button-Levels)
    - [4.18.8 Article Date](#Article-Date)
    - [4.18.9 Article Display](#Article-Display)
    - [4.18.10 Article Signature](#Article-Signature)
    - [4.18.11 Article Miscellanea](#Article-Miscellanea)
  + [4.19 MIME Commands](#MIME-Commands)
  + [4.20 Charsets](#Charsets)
  + [4.21 Article Commands](#Article-Commands)
  + [4.22 Summary Sorting](#Summary-Sorting)
  + [4.23 Finding the Parent](#Finding-the-Parent)
  + [4.24 Alternative Approaches](#Alternative-Approaches)
    - [4.24.1 Pick and Read](#Pick-and-Read)
    - [4.24.2 Binary Groups](#Binary-Groups)
  + [4.25 Tree Display](#Tree-Display)
  + [4.26 Mail Group Commands](#Mail-Group-Commands)
  + [4.27 Various Summary Stuff](#Various-Summary-Stuff)
    - [4.27.1 Summary Group Information](#Summary-Group-Information)
    - [4.27.2 Searching for Articles](#Searching-for-Articles)
    - [4.27.3 Summary Generation Commands](#Summary-Generation-Commands)
    - [4.27.4 Really Various Summary Commands](#Really-Various-Summary-Commands)
  + [4.28 Exiting the Summary Buffer](#Exiting-the-Summary-Buffer)
  + [4.29 Crosspost Handling](#Crosspost-Handling)
  + [4.30 Duplicate Suppression](#Duplicate-Suppression)
  + [4.31 Security](#Security)
  + [4.32 Mailing List](#Mailing-List)
* [5 Article Buffer](#Article-Buffer)
  + [5.1 Hiding Headers](#Hiding-Headers)
  + [5.2 Using MIME](#Using-MIME)
  + [5.3 HTML](#HTML)
  + [5.4 Customizing Articles](#Customizing-Articles)
  + [5.5 Article Keymap](#Article-Keymap)
  + [5.6 Misc Article](#Misc-Article)
* [6 Composing Messages](#Composing-Messages)
  + [6.1 Mail](#Mail)
  + [6.2 Posting Server](#Posting-Server)
  + [6.3 POP before SMTP](#POP-before-SMTP)
  + [6.4 Mail and Post](#Mail-and-Post)
  + [6.5 Archived Messages](#Archived-Messages)
  + [6.6 Posting Styles](#Posting-Styles)
  + [6.7 Drafts](#Drafts)
  + [6.8 Rejected Articles](#Rejected-Articles)
  + [6.9 Signing and encrypting](#Signing-and-encrypting)
* [7 Select Methods](#Select-Methods)
  + [7.1 Server Buffer](#Server-Buffer)
    - [7.1.1 Server Buffer Format](#Server-Buffer-Format)
    - [7.1.2 Server Commands](#Server-Commands)
    - [7.1.3 Example Methods](#Example-Methods)
    - [7.1.4 Creating a Virtual Server](#Creating-a-Virtual-Server)
    - [7.1.5 Server Variables](#Server-Variables)
    - [7.1.6 Servers and Methods](#Servers-and-Methods)
    - [7.1.7 Unavailable Servers](#Unavailable-Servers)
  + [7.2 Getting News](#Getting-News)
    - [7.2.1 NNTP](#NNTP)
      * [7.2.1.1 Direct Functions](#Direct-Functions)
      * [7.2.1.2 Indirect Functions](#Indirect-Functions)
      * [7.2.1.3 Common Variables](#Common-Variables)
    - [7.2.2 News Spool](#News-Spool)
  + [7.3 Using IMAP](#Using-IMAP)
    - [7.3.1 Connecting to an IMAP Server](#Connecting-to-an-IMAP-Server)
    - [7.3.2 Customizing the IMAP Connection](#Customizing-the-IMAP-Connection)
    - [7.3.3 Client-Side IMAP Splitting](#Client_002dSide-IMAP-Splitting)
    - [7.3.4 Support for IMAP Extensions](#Support-for-IMAP-Extensions)
  + [7.4 Getting Mail](#Getting-Mail)
    - [7.4.1 Mail in a Newsreader](#Mail-in-a-Newsreader)
    - [7.4.2 Getting Started Reading Mail](#Getting-Started-Reading-Mail)
    - [7.4.3 Splitting Mail](#Splitting-Mail)
    - [7.4.4 Mail Sources](#Mail-Sources)
      * [7.4.4.1 Mail Source Specifiers](#Mail-Source-Specifiers)
      * [7.4.4.2 Function Interface](#Mail-Source-Functions)
      * [7.4.4.3 Mail Source Customization](#Mail-Source-Customization)
      * [7.4.4.4 Fetching Mail](#Fetching-Mail)
    - [7.4.5 Mail Back End Variables](#Mail-Back-End-Variables)
    - [7.4.6 Fancy Mail Splitting](#Fancy-Mail-Splitting)
    - [7.4.7 Group Mail Splitting](#Group-Mail-Splitting)
    - [7.4.8 Incorporating Old Mail](#Incorporating-Old-Mail)
    - [7.4.9 Expiring Mail](#Expiring-Mail)
    - [7.4.10 Washing Mail](#Washing-Mail)
    - [7.4.11 Duplicates](#Duplicates)
    - [7.4.12 Not Reading Mail](#Not-Reading-Mail)
    - [7.4.13 Choosing a Mail Back End](#Choosing-a-Mail-Back-End)
      * [7.4.13.1 Unix Mail Box](#Unix-Mail-Box)
      * [7.4.13.2 Babyl](#Babyl)
      * [7.4.13.3 Mail Spool](#Mail-Spool)
      * [7.4.13.4 MH Spool](#MH-Spool)
      * [7.4.13.5 Maildir](#Maildir)
      * [7.4.13.6 Group parameters](#nnmaildir-Group-Parameters)
      * [7.4.13.7 Article identification](#Article-Identification)
      * [7.4.13.8 NOV data](#NOV-Data)
      * [7.4.13.9 Article marks](#Article-Marks)
      * [7.4.13.10 Mail Folders](#Mail-Folders)
      * [7.4.13.11 Comparing Mail Back Ends](#Comparing-Mail-Back-Ends)
  + [7.5 Browsing the Web](#Browsing-the-Web)
    - [7.5.1 Web Searches](#Web-Searches)
    - [7.5.2 RSS](#RSS)
    - [7.5.3 Atom](#Atom)
  + [7.6 Other Sources](#Other-Sources)
    - [7.6.1 Directory Groups](#Directory-Groups)
    - [7.6.2 Anything Groups](#Anything-Groups)
    - [7.6.3 Document Groups](#Document-Groups)
      * [7.6.3.1 Document Server Internals](#Document-Server-Internals)
    - [7.6.4 Mail-To-News Gateways](#Mail_002dTo_002dNews-Gateways)
    - [7.6.5 The Empty Backend](#The-Empty-Backend)
  + [7.7 Virtual Groups](#Virtual-Groups)
    - [7.7.1 Select Groups](#Selection-Groups)
    - [7.7.2 Combined Groups](#Combined-Groups)
  + [7.8 Email Based Diary](#Email-Based-Diary)
    - [7.8.1 The NNDiary Back End](#The-NNDiary-Back-End)
      * [7.8.1.1 Diary Messages](#Diary-Messages)
      * [7.8.1.2 Running NNDiary](#Running-NNDiary)
      * [7.8.1.3 Customizing NNDiary](#Customizing-NNDiary)
    - [7.8.2 The Gnus Diary Library](#The-Gnus-Diary-Library)
      * [7.8.2.1 Diary Summary Line Format](#Diary-Summary-Line-Format)
      * [7.8.2.2 Diary Articles Sorting](#Diary-Articles-Sorting)
      * [7.8.2.3 Diary Headers Generation](#Diary-Headers-Generation)
      * [7.8.2.4 Diary Group Parameters](#Diary-Group-Parameters)
  + [7.9 Gnus Unplugged](#Gnus-Unplugged)
    - [7.9.1 Agent Basics](#Agent-Basics)
    - [7.9.2 Agent Categories](#Agent-Categories)
      * [7.9.2.1 Category Syntax](#Category-Syntax)
      * [7.9.2.2 Category Buffer](#Category-Buffer)
      * [7.9.2.3 Category Variables](#Category-Variables)
    - [7.9.3 Agent Commands](#Agent-Commands)
      * [7.9.3.1 Group Agent Commands](#Group-Agent-Commands)
      * [7.9.3.2 Summary Agent Commands](#Summary-Agent-Commands)
      * [7.9.3.3 Server Agent Commands](#Server-Agent-Commands)
    - [7.9.4 Agent Visuals](#Agent-Visuals)
    - [7.9.5 Agent as Cache](#Agent-as-Cache)
    - [7.9.6 Agent Expiry](#Agent-Expiry)
    - [7.9.7 Agent Regeneration](#Agent-Regeneration)
    - [7.9.8 Agent and flags](#Agent-and-flags)
    - [7.9.9 Agent and IMAP](#Agent-and-IMAP)
    - [7.9.10 Outgoing Messages](#Outgoing-Messages)
    - [7.9.11 Agent Variables](#Agent-Variables)
    - [7.9.12 Example Setup](#Example-Setup)
    - [7.9.13 Batching Agents](#Batching-Agents)
    - [7.9.14 Agent Caveats](#Agent-Caveats)
* [8 Scoring](#Scoring)
  + [8.1 Summary Score Commands](#Summary-Score-Commands)
  + [8.2 Group Score Commands](#Group-Score-Commands)
  + [8.3 Score Variables](#Score-Variables)
  + [8.4 Score File Format](#Score-File-Format)
  + [8.5 Score File Editing](#Score-File-Editing)
  + [8.6 Adaptive Scoring](#Adaptive-Scoring)
  + [8.7 Home Score File](#Home-Score-File)
  + [8.8 Followups To Yourself](#Followups-To-Yourself)
  + [8.9 Scoring On Other Headers](#Scoring-On-Other-Headers)
  + [8.10 Scoring Tips](#Scoring-Tips)
  + [8.11 Reverse Scoring](#Reverse-Scoring)
  + [8.12 Global Score Files](#Global-Score-Files)
  + [8.13 Kill Files](#Kill-Files)
  + [8.14 Converting Kill Files](#Converting-Kill-Files)
  + [8.15 Advanced Scoring](#Advanced-Scoring)
    - [8.15.1 Advanced Scoring Syntax](#Advanced-Scoring-Syntax)
    - [8.15.2 Advanced Scoring Examples](#Advanced-Scoring-Examples)
    - [8.15.3 Advanced Scoring Tips](#Advanced-Scoring-Tips)
  + [8.16 Score Decays](#Score-Decays)
* [9 Searching](#Searching)
  + [9.1 Search Engines](#Search-Engines)
  + [9.2 Creating Search Groups](#Creating-Search-Groups)
  + [9.3 Search Queries](#Search-Queries)
    - [9.3.1 Date value parsing](#Date-value-parsing)
  + [9.4 nnmairix](#nnmairix)
    - [9.4.1 About mairix](#About-mairix)
    - [9.4.2 nnmairix requirements](#nnmairix-requirements)
    - [9.4.3 What nnmairix does](#What-nnmairix-does)
    - [9.4.4 Setting up mairix](#Setting-up-mairix)
    - [9.4.5 Configuring nnmairix](#Configuring-nnmairix)
    - [9.4.6 nnmairix keyboard shortcuts](#nnmairix-keyboard-shortcuts)
    - [9.4.7 Propagating marks](#Propagating-marks)
    - [9.4.8 nnmairix tips and tricks](#nnmairix-tips-and-tricks)
    - [9.4.9 nnmairix caveats](#nnmairix-caveats)
  + [9.5 Migrating from nnir](#nnir)
* [10 Various](#Various)
  + [10.1 Process/Prefix](#Process_002fPrefix)
  + [10.2 Interactive](#Interactive)
  + [10.3 Symbolic Prefixes](#Symbolic-Prefixes)
  + [10.4 Formatting Variables](#Formatting-Variables)
    - [10.4.1 Formatting Basics](#Formatting-Basics)
    - [10.4.2 Mode Line Formatting](#Mode-Line-Formatting)
    - [10.4.3 Advanced Formatting](#Advanced-Formatting)
    - [10.4.4 User-Defined Specs](#User_002dDefined-Specs)
    - [10.4.5 Formatting Fonts](#Formatting-Fonts)
    - [10.4.6 Positioning Point](#Positioning-Point)
    - [10.4.7 Tabulation](#Tabulation)
  + [10.5 Window Layout](#Window-Layout)
    - [10.5.1 Window Configuration Names](#Window-Configuration-Names)
    - [10.5.2 Example Window Configurations](#Example-Window-Configurations)
  + [10.6 Tabbed Interface](#Tabbed-Interface)
  + [10.7 Faces and Fonts](#Faces-and-Fonts)
  + [10.8 Mode Lines](#Mode-Lines)
  + [10.9 Highlighting and Menus](#Highlighting-and-Menus)
  + [10.10 Daemons](#Daemons)
  + [10.11 Notifications](#Notifications)
  + [10.12 Undo](#Undo)
  + [10.13 Predicate Specifiers](#Predicate-Specifiers)
  + [10.14 Moderation](#Moderation)
  + [10.15 Fetching a Group](#Fetching-a-Group)
  + [10.16 Image Enhancements](#Image-Enhancements)
    - [10.16.1 X-Face](#X_002dFace)
    - [10.16.2 Face](#Face)
    - [10.16.3 Smileys](#Smileys)
    - [10.16.4 Picons](#Picons)
    - [10.16.5 Gravatars](#Gravatars)
      * [10.16.5.1 Tool bar](#Tool-bar)
  + [10.17 Fuzzy Matching](#Fuzzy-Matching)
  + [10.18 Thwarting Email Spam](#Thwarting-Email-Spam)
    - [10.18.1 The problem of spam](#The-problem-of-spam)
    - [10.18.2 Anti-Spam Basics](#Anti_002dSpam-Basics)
    - [10.18.3 SpamAssassin, Vipul’s Razor, DCC, etc](#SpamAssassin)
  + [10.19 Spam Package](#Spam-Package)
    - [10.19.1 Spam Package Introduction](#Spam-Package-Introduction)
    - [10.19.2 Filtering Incoming Mail](#Filtering-Incoming-Mail)
    - [10.19.3 Detecting Spam in Groups](#Detecting-Spam-in-Groups)
    - [10.19.4 Spam and Ham Processors](#Spam-and-Ham-Processors)
    - [10.19.5 Spam Package Configuration Examples](#Spam-Package-Configuration-Examples)
    - [10.19.6 Spam Back Ends](#Spam-Back-Ends)
      * [10.19.6.1 Blacklists and Whitelists](#Blacklists-and-Whitelists)
      * [10.19.6.2 BBDB Whitelists](#BBDB-Whitelists)
      * [10.19.6.3 Gmane Spam Reporting](#Gmane-Spam-Reporting)
      * [10.19.6.4 Blackholes](#Blackholes)
      * [10.19.6.5 Regular Expressions Header Matching](#Regular-Expressions-Header-Matching)
      * [10.19.6.6 Bogofilter](#Bogofilter)
      * [10.19.6.7 SpamAssassin back end](#SpamAssassin-back-end)
      * [10.19.6.8 ifile spam filtering](#ifile-spam-filtering)
      * [10.19.6.9 Spam Statistics Filtering](#Spam-Statistics-Filtering)
      * [10.19.6.10 Using SpamOracle with Gnus](#SpamOracle)
    - [10.19.7 Extending the Spam package](#Extending-the-Spam-package)
    - [10.19.8 Spam Statistics Package](#Spam-Statistics-Package)
      * [10.19.8.1 Creating a spam-stat dictionary](#Creating-a-spam_002dstat-dictionary)
      * [10.19.8.2 Splitting mail using spam-stat](#Splitting-mail-using-spam_002dstat)
      * [10.19.8.3 Low-level interface to the spam-stat dictionary](#Low_002dlevel-interface-to-the-spam_002dstat-dictionary)
  + [10.20 The Gnus Registry](#The-Gnus-Registry)
    - [10.20.1 Gnus Registry Setup](#Gnus-Registry-Setup)
    - [10.20.2 Fetching by `Message-ID` Using the Registry](#Registry-Article-Refer-Method)
    - [10.20.3 Fancy splitting to parent](#Fancy-splitting-to-parent)
    - [10.20.4 Store custom flags and keywords](#Store-custom-flags-and-keywords)
    - [10.20.5 Store arbitrary data](#Store-arbitrary-data)
  + [10.21 The Gnus Cloud](#The-Gnus-Cloud)
    - [10.21.1 Gnus Cloud Setup](#Gnus-Cloud-Setup)
    - [10.21.2 Gnus Cloud Usage](#Gnus-Cloud-Usage)
  + [10.22 System Sleep Integration](#System-Sleep-Integration)
  + [10.23 Interaction with other modes](#Other-modes)
    - [10.23.1 Dired](#Dired)
  + [10.24 Various Various](#Various-Various)
* [11 The End](#The-End)
* [12 Appendices](#Appendices)
  + [12.1 History](#History)
    - [12.1.1 Gnus Versions](#Gnus-Versions)
    - [12.1.2 Why?](#Why_003f)
    - [12.1.3 Conformity](#Conformity)
    - [12.1.4 Contributors](#Contributors)
    - [12.1.5 New Features](#New-Features)
      * [12.1.5.1 (ding) Gnus](#ding-Gnus)
      * [12.1.5.2 September Gnus](#September-Gnus)
      * [12.1.5.3 Red Gnus](#Red-Gnus)
      * [12.1.5.4 Quassia Gnus](#Quassia-Gnus)
      * [12.1.5.5 Pterodactyl Gnus](#Pterodactyl-Gnus)
      * [12.1.5.6 Oort Gnus](#Oort-Gnus)
      * [12.1.5.7 No Gnus](#No-Gnus)
      * [12.1.5.8 Ma Gnus](#Ma-Gnus)
  + [12.2 On Writing Manuals](#On-Writing-Manuals)
  + [12.3 Terminology](#Terminology)
  + [12.4 Customization](#Customization)
    - [12.4.1 Slow/Expensive Connection](#Slow_002fExpensive-Connection)
    - [12.4.2 Slow Terminal Connection](#Slow-Terminal-Connection)
    - [12.4.3 Little Disk Space](#Little-Disk-Space)
    - [12.4.4 Slow Machine](#Slow-Machine)
  + [12.5 Troubleshooting](#Troubleshooting)
  + [12.6 Gnus Reference Guide](#Gnus-Reference-Guide)
    - [12.6.1 Gnus Utility Functions](#Gnus-Utility-Functions)
    - [12.6.2 Back End Interface](#Back-End-Interface)
      * [12.6.2.1 Required Back End Functions](#Required-Back-End-Functions)
      * [12.6.2.2 Optional Back End Functions](#Optional-Back-End-Functions)
      * [12.6.2.3 Error Messaging](#Error-Messaging)
      * [12.6.2.4 Writing New Back Ends](#Writing-New-Back-Ends)
      * [12.6.2.5 Hooking New Back Ends Into Gnus](#Hooking-New-Back-Ends-Into-Gnus)
      * [12.6.2.6 Mail-like Back Ends](#Mail_002dlike-Back-Ends)
      * [12.6.2.7 Web Feed Back Ends](#Web-Feed-Back-Ends)
    - [12.6.3 Score File Syntax](#Score-File-Syntax)
    - [12.6.4 Headers](#Headers)
    - [12.6.5 Ranges](#Ranges)
    - [12.6.6 Group Info](#Group-Info)
    - [12.6.7 Extended Interactive](#Extended-Interactive)
    - [12.6.8 Various File Formats](#Various-File-Formats)
      * [12.6.8.1 Active File Format](#Active-File-Format)
      * [12.6.8.2 Newsgroups File Format](#Newsgroups-File-Format)
  + [12.7 Emacs for Heathens](#Emacs-for-Heathens)
    - [12.7.1 Keystrokes](#Keystrokes)
    - [12.7.2 Emacs Lisp](#Emacs-Lisp)
  + [12.8 Frequently Asked Questions](#Frequently-Asked-Questions)
    - [12.8.1 Introduction](#FAQ-_002d-Introduction)
    - [12.8.2 Installation FAQ](#FAQ-1-_002d-Installation-FAQ)
    - [12.8.3 Startup / Group buffer](#FAQ-2-_002d-Startup-_002f-Group-buffer)
    - [12.8.4 Getting Messages](#FAQ-3-_002d-Getting-Messages)
    - [12.8.5 Reading messages](#FAQ-4-_002d-Reading-messages)
    - [12.8.6 Composing messages](#FAQ-5-_002d-Composing-messages)
    - [12.8.7 Old messages](#FAQ-6-_002d-Old-messages)
    - [12.8.8 Gnus in a dial-up environment](#FAQ-7-_002d-Gnus-in-a-dial_002dup-environment)
    - [12.8.9 Getting help](#FAQ-8-_002d-Getting-help)
    - [12.8.10 Tuning Gnus](#FAQ-9-_002d-Tuning-Gnus)
    - [12.8.11 Glossary](#FAQ-_002d-Glossary)
* [13 GNU Free Documentation License](#GNU-Free-Documentation-License)
* [14 Index](#Index)
* [15 Key Index](#Key-Index)

---

Next: [Starting Gnus](#Starting-Up), Previous: [The Gnus Newsreader](#Top), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 1 Don’t Panic [¶](#Don_0027t-Panic-1)

Welcome, gentle user, to the Gnus newsreader and email client! Gnus
is unlike most clients, in part because of its endless
configurability, in part because of its historical origins. Gnus is
now a fully-featured email client, but it began life as a Usenet-style
newsreader, and its genes are still newsreader genes. Thus it behaves
a little differently than most mail clients.

The typical assumptions of a newsreader are:

1. The server offers a potentially enormous number of newsgroups on a
   variety of subjects. The user may only be interested in some of those
   groups, and more interested in some than others.
2. Many groups see a high volume of articles, and the user won’t want to
   read all of them. Mechanisms are needed for foregrounding interesting
   articles, and backgrounding uninteresting articles.
3. Once a group has been scanned and dealt with by the user, it’s
   unlikely to be of further interest until new articles come in.

These assumptions lead to certain default Gnus behaviors:

1. Not all interesting groups are equally interesting, thus groups have
   varying degrees of “subscribedness”, with different behavior
   depending on “how subscribed” a group is.
2. There are many commands and tools for scoring and sorting articles,
   or otherwise sweeping them under the rug.
3. Gnus will only show you groups with unread or ticked articles;
   groups with no new articles are hidden.
4. When entering a group, only unread or ticked articles are shown,
   all other articles are hidden.

If this seems draconian, think of it as Automatic Inbox Zero. This is
the way Gnus works by default. It is possible to make it work more
like an email client (always showing read groups and read articles),
but that takes some effort on the part of the user.

The brief introduction below should be enough to get you off the
ground.

### The Basics of Servers, Groups, and Articles [¶](#The-Basics-of-Servers_002c-Groups_002c-and-Articles)

The fundamental building blocks of Gnus are *servers*,
*groups*, and *articles*. Servers can be local or remote.
Each server maintains a list of groups, and those groups contain
articles. Because Gnus presents a unified interface to a wide variety
of servers, the vocabulary doesn’t always quite line up (see [Glossary](#FAQ-_002d-Glossary), for a more complete glossary). Thus a local Maildir is
referred to as a “server” (see [Finding the News](#Finding-the-News)) the same as a
Usenet or IMAP server is; “groups” (see [Group Buffer](#Group-Buffer)) might mean
an NNTP group, IMAP folder, or local mail directory; and an
“article” (see [Summary Buffer](#Summary-Buffer)) might elsewhere be known as a
message or an email. Gnus employs unified terms for all these things.

Servers fall into two general categories: “news-like”, meaning that
the articles are part of a public archive and can’t be manipulated by
the user; and “mail-like”, meaning that the articles are owned by
the user, who can freely edit them, move them around, and delete
them.

For news-like servers, which typically offer hundreds or thousands of
groups, it’s important to be able to subscribe to a subset of those
groups. For mail-like servers, the user is generally automatically
subscribed to all groups (though IMAP, for example, also allows
selective subscription). To change group subscription, enter the
Server buffer (with `^`) and press `RET` on the server
in question. From here, Gnus provides commands to change or toggle
your group subscriptions (see [Browse Foreign Server](#Browse-Foreign-Server)).

A Gnus installation is basically just a list of one or more servers,
plus the user’s subscribed groups from those servers, plus articles in
those groups.

Servers can be added and configured in two places: in the user’s
gnus.el startup file, using the `gnus-select-method` and
`gnus-secondary-select-methods` options, or within Gnus itself
using interactive commands in the Server buffer. See [Finding the News](#Finding-the-News), for details.

### Fetching Mail [¶](#Fetching-Mail-2)

New mail has to come from somewhere. Some servers, such as NNTP or
IMAP, are themselves responsible for fetching newly-arrived articles.
Others, such as Maildir or mbox servers, only store articles and don’t
fetch them from anywhere.

In the latter case, Gnus provides for `mail sources`: places
where new mail is fetched from. A mail source might be a local spool,
or a remote POP server, or some other source of incoming articles.
Mail sources are usually configured globally, but can be specified
per-group (see [Mail Sources](#Mail-Sources) for more information).

See [Scanning New Messages](#Scanning-New-Messages), for details on fetching new mail.

### Viewing Mail [¶](#Viewing-Mail)

By default, Gnus’s Group buffer only displays groups with unread
articles. It is always possible to display all the groups temporarily
with `L`, and to configure Gnus to always display some groups
(see [Listing Groups](#Listing-Groups)).

See [Selecting a Group](#Selecting-a-Group), for how to enter a group, and see [Summary Buffer](#Summary-Buffer) for what to do once you’re there.

### Sending Mail [¶](#Sending-Mail)

New message composition can be initiated from the Group buffer
(see [Misc Group Stuff](#Misc-Group-Stuff)). If you’re in a Summary buffer, you can
compose replies and forward emails in addition to starting new
messages, see [Summary Mail Commands](#Summary-Mail-Commands), for details.

For information about what happens once you’ve started composing a
message, see [Composing Messages](#Composing-Messages). For information on setting up
SMTP servers in particular, see [Mail
Variables](https://www.gnu.org/software/emacs/manual/html_mono/message.html#Mail-Variables) in Message manual.

---

Next: [Group Buffer](#Group-Buffer), Previous: [Don’t Panic](#Don_0027t-Panic), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 2 Starting Gnus [¶](#Starting-Gnus)

If you haven’t used Emacs much before using Gnus, read [Emacs for Heathens](#Emacs-for-Heathens) first.

If your system administrator has set things up properly, starting Gnus
and reading news is extremely easy—you just type `M-x gnus` in
your Emacs. If not, you should customize the variable
`gnus-select-method` as described in [Finding the News](#Finding-the-News). For a
minimal setup for posting should also customize the variables
`user-full-name` and `user-mail-address`.

If you want to start Gnus in a different frame, you can use the command
`M-x gnus-other-frame` instead.

If things do not go smoothly at startup, you have to twiddle some
variables in your `~/.gnus.el` file. This file is similar to
`~/.emacs`, but is read when Gnus starts.

If you puzzle at any terms used in this manual, please refer to the
terminology section (see [Terminology](#Terminology)).

* [Finding the News](#Finding-the-News)
* [The Server is Down](#The-Server-is-Down)
* [Child Gnusae](#Child-Gnusae)
* [New Groups](#New-Groups)
* [Changing Servers](#Changing-Servers)
* [Startup Files](#Startup-Files)
* [Auto Save](#Auto-Save)
* [The Active File](#The-Active-File)
* [Startup Variables](#Startup-Variables)

---

Next: [The Server is Down](#The-Server-is-Down), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.1 Finding the News [¶](#Finding-the-News-1)

First of all, you should know that there is a special buffer called
`*Server*` that lists all the servers Gnus knows about. You can
press `^` from the Group buffer to see it. In the Server buffer,
you can press `RET` on a defined server to see all the groups it
serves (subscribed or not!). You can also add or delete servers, edit
a foreign server’s definition, agentize or de-agentize a server, and
do many other neat things. See [Server Buffer](#Server-Buffer).
See [Foreign Groups](#Foreign-Groups). See [Agent Basics](#Agent-Basics).

The `gnus-select-method` variable says where Gnus should look for
news. This variable should be a list where the first element says
*how* and the second element says *where*. This method is your
native method. All groups not fetched with this method are
secondary or foreign groups.

For instance, if the ‘`news.somewhere.edu`’ NNTP server is where
you want to get your daily dosage of news from, you’d say:

```
(setq gnus-select-method '(nntp "news.somewhere.edu"))
```

If you want to read directly from the local spool, say:

```
(setq gnus-select-method '(nnspool ""))
```

If you can use a local spool, you probably should, as it will almost
certainly be much faster. But do not use the local spool if your
server is running Leafnode (which is a simple, standalone private news
server); in this case, use `(nntp "localhost")`.

If this variable is not set, Gnus will take a look at the
`NNTPSERVER` environment variable. If that variable isn’t set,
Gnus will see whether `gnus-nntpserver-file`
(`/etc/nntpserver` by default) has any opinions on the matter.
If that fails as well, Gnus will try to use the machine running Emacs
as an NNTP server. That’s a long shot, though.

However, if you use one NNTP server regularly and are just
interested in a couple of groups from a different server, you would be
better served by using the `B` command in the group buffer. It will
let you have a look at what groups are available, and you can subscribe
to any of the groups you want to. This also makes `.newsrc`
maintenance much tidier. See [Foreign Groups](#Foreign-Groups).

A slightly different approach to foreign groups is to set the
`gnus-secondary-select-methods` variable. The select methods
listed in this variable are in many ways just as native as the
`gnus-select-method` server. They will also be queried for active
files during startup (if that’s required), and new newsgroups that
appear on these servers will be subscribed (or not) just as native
groups are.

For instance, if you use the `nnmbox` back end to read your mail,
you would typically set this variable to

```
(setq gnus-secondary-select-methods '((nnmbox "")))
```

---

Next: [Child Gnusae](#Child-Gnusae), Previous: [Finding the News](#Finding-the-News), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.2 The Server is Down [¶](#The-Server-is-Down-1)

If the default server is down, Gnus will understandably have some
problems starting. However, if you have some mail groups in addition to
the news groups, you may want to start Gnus anyway.

Gnus, being the trusting sort of program, will ask whether to proceed
without a native select method if that server can’t be contacted. This
will happen whether the server doesn’t actually exist (i.e., you have
given the wrong address) or the server has just momentarily taken ill
for some reason or other. If you decide to continue and have no foreign
groups, you’ll find it difficult to actually do anything in the group
buffer. But, hey, that’s your problem. Blllrph!

If you know that the server is definitely down, or you just want to read
your mail without bothering with the server at all, you can use the
`gnus-no-server` command to start Gnus. That might come in handy
if you’re in a hurry as well. This command will not attempt to contact
your primary server—instead, it will just activate all groups on level
1 and 2. (You should preferably keep no native groups on those two
levels.) Also see [Group Levels](#Group-Levels).

---

Next: [New Groups](#New-Groups), Previous: [The Server is Down](#The-Server-is-Down), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.3 Child Gnusae [¶](#Child-Gnusae-1)

You might want to run more than one Emacs with more than one Gnus at the
same time. If you are using different `.newsrc` files (e.g., if you
are using the two different Gnusae to read from two different servers),
that is no problem whatsoever. You just do it.

The problem appears when you want to run two Gnusae that use the same
`.newsrc` file.

To work around that problem some, we here at the Think-Tank at the Gnus
Towers have come up with a new concept: *Parents* and
*children*.

Anyway, you start one Gnus up the normal way with `M-x gnus` (or
however you do it). Each subsequent child Gnusae should be started with
`M-x gnus-child`. These children won’t save normal `.newsrc`
files, but instead save *child files* that contain information only
on what groups have been read in the child session. When a parent Gnus
starts, it will read (and delete) these child files, incorporating all
information from them. (The child files will be read in the sequence
they were created, so the latest changes will have precedence.)

Information from the child files has, of course, precedence over the
information in the normal (i.e., parent) `.newsrc` file.

If the `.newsrc*` files have not been saved in the parent when the
child starts, you may be prompted as to whether to read an auto-save
file. If you answer “yes”, the unsaved changes to the parent will be
incorporated into the child. If you answer “no”, the child may see some
messages as unread that have been read in the parent.

---

Next: [Changing Servers](#Changing-Servers), Previous: [Child Gnusae](#Child-Gnusae), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.4 New Groups [¶](#New-Groups-1)

If you are satisfied that you really never want to see any new groups,
you can set `gnus-check-new-newsgroups` to `nil`. This will
also save you some time at startup. Even if this variable is
`nil`, you can always subscribe to the new groups just by pressing
`U` in the group buffer (see [Group Maintenance](#Group-Maintenance)). This variable
is `ask-server` by default. If you set this variable to
`always`, then Gnus will query the back ends for new groups even
when you do the `g` command (see [Scanning New Messages](#Scanning-New-Messages)).

* [Checking New Groups](#Checking-New-Groups)
* [Subscription Methods](#Subscription-Methods)
* [Filtering New Groups](#Filtering-New-Groups)

---

Next: [Subscription Methods](#Subscription-Methods), Up: [New Groups](#New-Groups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.1 Checking New Groups [¶](#Checking-New-Groups-1)

Gnus normally determines whether a group is new or not by comparing
the list of groups from the active file(s) with the lists of
subscribed and dead groups. This isn’t a particularly fast method.
If `gnus-check-new-newsgroups` is `ask-server`, Gnus will
ask the server for new groups since the last time. This is both
faster and cheaper. This also means that you can get rid of the list
of killed groups (see [Group Levels](#Group-Levels)) altogether, so you may set
`gnus-save-killed-list` to `nil`, which will save time both
at startup, at exit, and all over. Saves disk space, too. Why isn’t
this the default, then? Unfortunately, not all servers support this
command.

I bet I know what you’re thinking now: How do I find out whether my
server supports `ask-server`? No? Good, because I don’t have a
fail-safe answer. I would suggest just setting this variable to
`ask-server` and see whether any new groups appear within the next
few days. If any do, then it works. If none do, then it doesn’t
work. I could write a function to make Gnus guess whether the server
supports `ask-server`, but it would just be a guess. So I won’t.
You could `telnet` to the server and say `HELP` and see
whether it lists ‘`NEWGROUPS`’ among the commands it understands. If
it does, then it might work. (But there are servers that lists
‘`NEWGROUPS`’ without supporting the function properly.)

This variable can also be a list of select methods. If so, Gnus will
issue an `ask-server` command to each of the select methods, and
subscribe them (or not) using the normal methods. This might be handy
if you are monitoring a few servers for new groups. A side effect is
that startup will take much longer, so you can meditate while waiting.
Use the mantra “dingnusdingnusdingnus” to achieve permanent bliss.

---

Next: [Filtering New Groups](#Filtering-New-Groups), Previous: [Checking New Groups](#Checking-New-Groups), Up: [New Groups](#New-Groups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.2 Subscription Methods [¶](#Subscription-Methods-1)

What Gnus does when it encounters a new group is determined by the
`gnus-subscribe-newsgroup-method` variable.

This variable should contain a function. This function will be called
with the name of the new group as the only parameter.

Some handy pre-fab functions are:

`gnus-subscribe-zombies` [¶](#index-gnus_002dsubscribe_002dzombies)
:   Make all new groups zombies (see [Group Levels](#Group-Levels)). This is the
    default. You can browse the zombies later (with `A z`) and either
    kill them all off properly (with `S z`), or subscribe to them
    (with `u`).

`gnus-subscribe-randomly` [¶](#index-gnus_002dsubscribe_002drandomly)
:   Subscribe all new groups in arbitrary order. This really means that all
    new groups will be added at “the top” of the group buffer.

`gnus-subscribe-alphabetically` [¶](#index-gnus_002dsubscribe_002dalphabetically)
:   Subscribe all new groups in alphabetical order.

`gnus-subscribe-hierarchically` [¶](#index-gnus_002dsubscribe_002dhierarchically)
:   Subscribe all new groups hierarchically. The difference between this
    function and `gnus-subscribe-alphabetically` is slight.
    `gnus-subscribe-alphabetically` will subscribe new groups in a strictly
    alphabetical fashion, while this function will enter groups into its
    hierarchy. So if you want to have the ‘`rec`’ hierarchy before the
    ‘`comp`’ hierarchy, this function will not mess that configuration
    up. Or something like that.

`gnus-subscribe-interactively` [¶](#index-gnus_002dsubscribe_002dinteractively)
:   Subscribe new groups interactively. This means that Gnus will ask
    you about **all** new groups. The groups you choose to subscribe
    to will be subscribed hierarchically.

`gnus-subscribe-killed` [¶](#index-gnus_002dsubscribe_002dkilled)
:   Kill all new groups.

`gnus-subscribe-topics` [¶](#index-gnus_002dsubscribe_002dtopics)
:   Put the groups into the topic that has a matching `subscribe` topic
    parameter (see [Topic Parameters](#Topic-Parameters)). For instance, a `subscribe`
    topic parameter that looks like

    ```
    "nnml"
    ```

    will mean that all groups that match that regex will be subscribed under
    that topic.

    If no topics match the groups, the groups will be subscribed in the
    top-level topic.

A closely related variable is
`gnus-subscribe-hierarchical-interactive`. (That’s quite a
mouthful.) If this variable is non-`nil`, Gnus will ask you in a
hierarchical fashion whether to subscribe to new groups or not. Gnus
will ask you for each sub-hierarchy whether you want to descend the
hierarchy or not.

One common mistake is to set the variable a few paragraphs above
(`gnus-subscribe-newsgroup-method`) to
`gnus-subscribe-hierarchical-interactive`. This is an error. This
will not work. This is ga-ga. So don’t do it.

---

Previous: [Subscription Methods](#Subscription-Methods), Up: [New Groups](#New-Groups)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 2.4.3 Filtering New Groups [¶](#Filtering-New-Groups-1)

A nice and portable way to control which new newsgroups should be
subscribed (or ignored) is to put an *options* line at the start of
the `.newsrc` file. Here’s an example:

```
options -n !alt.all !rec.all sci.all
```

This line obviously belongs to a serious-minded intellectual scientific
person (or she may just be plain old boring), because it says that all
groups that have names beginning with ‘`alt`’ and ‘`rec`’ should
be ignored, and all groups with names beginning with ‘`sci`’ should
be subscribed. Gnus will not use the normal subscription method for
subscribing these groups.
`gnus-subscribe-options-newsgroup-method` is used instead. This
variable defaults to `gnus-subscribe-alphabetically`.

The “options -n” format is very simplistic. The syntax above is all
that is supports: you can force-subscribe hierarchies, or you can
deny hierarchies, and that’s it.

If you don’t want to mess with your `.newsrc` file, you can just
set the two variables `gnus-options-subscribe` and
`gnus-options-not-subscribe`. These two variables do exactly the
same as the `.newsrc` ‘`options -n`’ trick. Both are regexps,
and if the new group matches the former, it will be unconditionally
subscribed, and if it matches the latter, it will be ignored.

Yet another variable that meddles here is
`gnus-auto-subscribed-groups`. It works exactly like
`gnus-options-subscribe`, and is therefore really superfluous,
but I thought it would be nice to have two of these. This variable is
more meant for setting some ground rules, while the other variable is
used more for user fiddling. By default this variable makes all new
groups that come from mail back ends (`nnml`, `nnbabyl`,
`nnfolder`, `nnmbox`, `nnmh`, `nnimap`, and
`nnmaildir`) subscribed. If you don’t like that, just set this
variable to `nil`.

As if that wasn’t enough, `gnus-auto-subscribed-categories` also
allows you to specify that new groups should be subscribed based on the
category their select methods belong to. The default is ‘`(mail
post-mail)`’, meaning that all new groups from mail-like backends
should be subscribed automatically.

New groups that match these variables are subscribed using
`gnus-subscribe-options-newsgroup-method`.

---

Next: [Startup Files](#Startup-Files), Previous: [New Groups](#New-Groups), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.5 Changing Servers [¶](#Changing-Servers-1)

Sometimes it is necessary to move from one NNTP server to another.
This happens very rarely, but perhaps you change jobs, or one server is
very flaky and you want to use another.

Changing the server is pretty easy, right? You just change
`gnus-select-method` to point to the new server?

*Wrong!*

Article numbers are not (in any way) kept synchronized between different
NNTP servers, and the only way Gnus keeps track of what articles
you have read is by keeping track of article numbers. So when you
change `gnus-select-method`, your `.newsrc` file becomes
worthless.

You can use the `M-x gnus-group-clear-data-on-native-groups`
command to clear out all data that you have on your native groups.
Use with caution.

Clear the data from the current group only—nix out marks and the
list of read articles (`gnus-group-clear-data`).

After changing servers, you **must** move the cache hierarchy away,
since the cached articles will have wrong article numbers, which will
affect which articles Gnus thinks are read.
`gnus-group-clear-data-on-native-groups` will ask you if you want
to have it done automatically; for `gnus-group-clear-data`, you
can use `M-x gnus-cache-move-cache` (but beware, it will move the
cache for all groups).

---

Next: [Auto Save](#Auto-Save), Previous: [Changing Servers](#Changing-Servers), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.6 Startup Files [¶](#Startup-Files-1)

Most common Unix news readers use a shared startup file called
`.newsrc`. This file contains all the information about what
groups are subscribed, and which articles in these groups have been
read.

Things got a bit more complicated with GNUS. In addition to
keeping the `.newsrc` file updated, it also used a file called
`.newsrc.el` for storing all the information that didn’t fit into
the `.newsrc` file. (Actually, it also duplicated everything in
the `.newsrc` file.) GNUS would read whichever one of these
files was the most recently saved, which enabled people to swap between
GNUS and other newsreaders.

That was kinda silly, so Gnus went one better: In addition to the
`.newsrc` and `.newsrc.el` files, Gnus also has a file called
`.newsrc.eld`. It will read whichever of these files that are most
recent, but it will never write a `.newsrc.el` file. You should
never delete the `.newsrc.eld` file—it contains much information
not stored in the `.newsrc` file.

You can turn off writing the `.newsrc` file by setting
`gnus-save-newsrc-file` to `nil`, which means you can delete
the file and save some space, as well as exiting from Gnus faster.
However, this will make it impossible to use other newsreaders than
Gnus. But hey, who would want to, right? Similarly, setting
`gnus-read-newsrc-file` to `nil` makes Gnus ignore the
`.newsrc` file and any `.newsrc-SERVER` files, which can be
convenient if you use a different news reader occasionally, and you
want to read a different subset of the available groups with that
news reader.

If `gnus-save-killed-list` (default `t`) is `nil`, Gnus
will not save the list of killed groups to the startup file. This will
save both time (when starting and quitting) and space (on disk). It
will also mean that Gnus has no record of what groups are new or old,
so the automatic new groups subscription methods become meaningless.
You should always set `gnus-check-new-newsgroups` to `nil` or
`ask-server` if you set this variable to `nil` (see [New Groups](#New-Groups)). This variable can also be a regular expression. If that’s
the case, remove all groups that do not match this regexp before
saving. This can be useful in certain obscure situations that involve
several servers where not all servers support `ask-server`.

The `gnus-startup-file` variable says where the startup files are.
The default value is `~/.newsrc`, with the Gnus (El Dingo) startup
file being whatever that one is, with a ‘`.eld`’ appended.
If you want to keep multiple numbered backups of this file, set
`gnus-backup-startup-file`. It respects the same values as the
`version-control` variable.

`gnus-save-newsrc-hook` is called before saving any of the newsrc
files, while `gnus-save-quick-newsrc-hook` is called just before
saving the `.newsrc.eld` file, and
`gnus-save-standard-newsrc-hook` is called just before saving the
`.newsrc` file. The latter two are commonly used to turn version
control on or off. Version control is on by default when saving the
startup files. If you want to turn backup creation off, say something like:

```
(defun turn-off-backup ()
  (set (make-local-variable 'backup-inhibited) t))

(add-hook 'gnus-save-quick-newsrc-hook 'turn-off-backup)
(add-hook 'gnus-save-standard-newsrc-hook 'turn-off-backup)
```

When Gnus starts, it will read the `gnus-site-init-file`
(`.../site-lisp/gnus-init` by default) and `gnus-init-file`
(`~/.gnus` by default) files. These are normal Emacs Lisp files
and can be used to avoid cluttering your `~/.emacs` and
`site-init` files with Gnus stuff. Gnus will also check for files
with the same names as these, but with `.elc` and `.el`
suffixes. In other words, if you have set `gnus-init-file` to
`~/.gnus`, it will look for `~/.gnus.elc`, `~/.gnus.el`,
and finally `~/.gnus` (in this order). If Emacs was invoked with
the `-q` or `--no-init-file` options (see [Initial Options](https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Initial-Options) in The Emacs Manual), Gnus doesn’t read
`gnus-init-file`.

---

Next: [The Active File](#The-Active-File), Previous: [Startup Files](#Startup-Files), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.7 Auto Save [¶](#Auto-Save-1)

Whenever you do something that changes the Gnus data (reading articles,
catching up, killing/subscribing groups), the change is added to a
special *dribble buffer*. This buffer is auto-saved the normal
Emacs way. If your Emacs should crash before you have saved the
`.newsrc` files, all changes you have made can be recovered from
this file.

If Gnus detects this file at startup, it will ask the user whether to
read it. The auto save file is deleted whenever the real startup file is
saved.

If `gnus-use-dribble-file` is `nil`, Gnus won’t create and
maintain a dribble buffer. The default is `t`.

Gnus will put the dribble file(s) in `gnus-dribble-directory`. If
this variable is `nil`, which it is by default, Gnus will dribble
into the directory where the `.newsrc` file is located. (This is
normally the user’s home directory.) The dribble file will get the same
file permissions as the `.newsrc` file.

If `gnus-always-read-dribble-file` is non-`nil`, Gnus will
read the dribble file on startup without querying the user.

---

Next: [Startup Variables](#Startup-Variables), Previous: [Auto Save](#Auto-Save), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.8 The Active File [¶](#The-Active-File-1)

When Gnus starts, or indeed whenever it tries to determine whether new
articles have arrived, it reads the active file. This is a very large
file that lists all the active groups and articles on the server.

Before examining the active file, Gnus deletes all lines that match the
regexp `gnus-ignored-newsgroups`. This is done primarily to reject
any groups with bogus names, but you can use this variable to make Gnus
ignore hierarchies you aren’t ever interested in. However, this is not
recommended. In fact, it’s highly discouraged. Instead, see [New Groups](#New-Groups) for an overview of other variables that can be used instead.

The active file can be rather Huge, so if you have a slow network, you
can set `gnus-read-active-file` to `nil` to prevent Gnus from
reading the active file. This variable is `some` by default.

Gnus will try to make do by getting information just on the groups that
you actually subscribe to.

Note that if you subscribe to lots and lots of groups, setting this
variable to `nil` will probably make Gnus slower, not faster. At
present, having this variable `nil` will slow Gnus down
considerably, unless you read news over a 2400 baud modem.

This variable can also have the value `some`. Gnus will then
attempt to read active info only on the subscribed groups. On some
servers this is quite fast (on sparkling, brand new INN servers that
support the `LIST ACTIVE group` command), on others this isn’t fast
at all. In any case, `some` should be faster than `nil`, and
is certainly faster than `t` over slow lines.

Some news servers (old versions of Leafnode and old versions of INN, for
instance) do not support the `LIST ACTIVE group`. For these
servers, `nil` is probably the most efficient value for this
variable.

If this variable is `nil`, Gnus will ask for group info in total
lock-step, which isn’t very fast. If it is `some` and you use an
NNTP server, Gnus will pump out commands as fast as it can, and
read all the replies in one swoop. This will normally result in better
performance, but if the server does not support the aforementioned
`LIST ACTIVE group` command, this isn’t very nice to the server.

If you think that starting up Gnus takes too long, try all the three
different values for this variable and see what works best for you.

In any case, if you use `some` or `nil`, you should definitely
kill all groups that you aren’t interested in to speed things up.

Note that this variable also affects active file retrieval from
secondary select methods.

---

Previous: [The Active File](#The-Active-File), Up: [Starting Gnus](#Starting-Up)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 2.9 Startup Variables [¶](#Startup-Variables-1)

`gnus-before-startup-hook` [¶](#index-gnus_002dbefore_002dstartup_002dhook)
:   A hook called as the first thing when Gnus is started.

`gnus-before-resume-hook` [¶](#index-gnus_002dbefore_002dresume_002dhook)
:   A hook called as the first thing when Gnus is resumed after a suspend.

`gnus-startup-hook` [¶](#index-gnus_002dstartup_002dhook)
:   A hook run as the very last thing after starting up Gnus

`gnus-started-hook` [¶](#index-gnus_002dstarted_002dhook)
:   A hook that is run as the very last thing after starting up Gnus
    successfully.

`gnus-setup-news-hook` [¶](#index-gnus_002dsetup_002dnews_002dhook)
:   A hook that is run after reading the `.newsrc` file(s), but before
    generating the group buffer.

`gnus-check-bogus-newsgroups` [¶](#index-gnus_002dcheck_002dbogus_002dnewsgroups)
:   If non-`nil`, Gnus will check for and delete all bogus groups at
    startup. A *bogus group* is a group that you have in your
    `.newsrc` file, but doesn’t exist on the news server. Checking for
    bogus groups can take quite a while, so to save time and resources it’s
    best to leave this option off, and do the checking for bogus groups once
    in a while from the group buffer instead (see [Group Maintenance](#Group-Maintenance)).

`gnus-inhibit-startup-message` [¶](#index-gnus_002dinhibit_002dstartup_002dmessage)
:   If non-`nil`, the startup message won’t be displayed. That way,
    your boss might not notice as easily that you are reading news instead
    of doing your job. Note that this variable is used before
    `~/.gnus.el` is loaded, so it should be set in `.emacs` instead.

`gnus-no-groups-message` [¶](#index-gnus_002dno_002dgroups_002dmessage)
:   Message displayed by Gnus when no groups are available.

---

Next: [Summary Buffer](#Summary-Buffer), Previous: [Starting Gnus](#Starting-Up), Up: [The Gnus Newsreader](#Top)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]## 3 Group Buffer [¶](#Group-Buffer-1)

The *group buffer* lists all (or parts) of the available groups. It
is the first buffer shown when Gnus starts, and will never be killed as
long as Gnus is active.

* [Group Buffer Format](#Group-Buffer-Format)
* [Group Maneuvering](#Group-Maneuvering)
* [Selecting a Group](#Selecting-a-Group)
* [Subscription Commands](#Subscription-Commands)
* [Group Data](#Group-Data)
* [Group Levels](#Group-Levels)
* [Group Score](#Group-Score)
* [Marking Groups](#Marking-Groups)
* [Foreign Groups](#Foreign-Groups)
* [Group Parameters](#Group-Parameters)
* [Listing Groups](#Listing-Groups)
* [Sorting Groups](#Sorting-Groups)
* [Group Maintenance](#Group-Maintenance)
* [Browse Foreign Server](#Browse-Foreign-Server)
* [Exiting Gnus](#Exiting-Gnus)
* [Group Topics](#Group-Topics)
* [Accessing groups of non-English names](#Non_002dASCII-Group-Names)
* [Misc Group Stuff](#Misc-Group-Stuff)

---

Next: [Group Maneuvering](#Group-Maneuvering), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.1 Group Buffer Format [¶](#Group-Buffer-Format-1)

You can customize the Group Mode tool bar, see `M-x
customize-apropos RET gnus-group-tool-bar`.

The tool bar icons are now (de)activated correctly depending on the
cursor position. Therefore, moving around in the Group Buffer is
slower. You can disable this via the variable
`gnus-group-update-tool-bar`. Its default value depends on your
Emacs version.

* [Group Line Specification](#Group-Line-Specification)
* [Group Mode Line Specification](#Group-Mode-Line-Specification)
* [Group Highlighting](#Group-Highlighting)

---

Next: [Group Mode Line Specification](#Group-Mode-Line-Specification), Up: [Group Buffer Format](#Group-Buffer-Format)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.1.1 Group Line Specification [¶](#Group-Line-Specification-1)

The default format of the group buffer is nice and dull, but you can
make it as exciting and ugly as you feel like.

Here’s a couple of example group lines:

```
     25: news.announce.newusers
 *    0: alt.fan.andrea-dworkin
```

Quite simple, huh?

You can see that there are 25 unread articles in
‘`news.announce.newusers`’. There are no unread articles, but some
ticked articles, in ‘`alt.fan.andrea-dworkin`’ (see that little
asterisk at the beginning of the line?).

You can change that format to whatever you want by fiddling with the
`gnus-group-line-format` variable. This variable works along the
lines of a `format` specification, which is pretty much the same as
a `printf` specification, for those of you who use (feh!) C.
See [Formatting Variables](#Formatting-Variables).

‘`%M%S%5y:%B%(%g%)\n`’ is the value that produced those lines above.

There should always be a colon on the line; the cursor always moves to
the colon after performing an operation. See [Positioning Point](#Positioning-Point). Nothing else is required—not even the group name. All
displayed text is just window dressing, and is never examined by Gnus.
Gnus stores all real information it needs using text properties.

(Note that if you make a really strange, wonderful, spreadsheet-like
layout, everybody will believe you are hard at work with the accounting
instead of wasting time reading news.)

Here’s a list of all available format characters:

‘`M`’
:   An asterisk if the group only has marked articles.

‘`S`’
:   Whether the group is subscribed.

‘`L`’
:   Level of subscribedness.

‘`N`’
:   Number of unread articles.

‘`I`’
:   Number of dormant articles.

‘`T`’
:   Number of ticked articles.

‘`R`’
:   Number of read articles.

‘`U`’
:   Number of unseen articles.

‘`t`’
:   Estimated total number of articles. (This is really max-number
    minus min-number plus 1.)

    Gnus uses this estimation because the NNTP protocol provides
    efficient access to max-number and min-number but getting
    the true unread message count is not possible efficiently. For
    hysterical raisins, even the mail back ends, where the true number of
    unread messages might be available efficiently, use the same limited
    interface. To remove this restriction from Gnus means that the back
    end interface has to be changed, which is not an easy job.

    The nnml backend (see [Mail Spool](#Mail-Spool)) has a feature called “group
    compaction” which circumvents this deficiency: the idea is to
    renumber all articles from 1, removing all gaps between numbers, hence
    getting a correct total count. Other backends may support this in the
    future. In order to keep your total article count relatively up to
    date, you might want to compact your groups (or even directly your
    server) from time to time. See [Misc Group Stuff](#Misc-Group-Stuff), See [Server Commands](#Server-Commands).

‘`y`’
:   Number of unread, unticked, non-dormant articles.

‘`i`’
:   Number of ticked and dormant articles.

‘`g`’
:   Full group name.

‘`G`’
:   Group name.

‘`C`’
:   Group comment (see [Group Parameters](#Group-Parameters)) or group name if there is no
    comment element in the group parameters.

‘`D`’
:   Newsgroup description. You need to read the group descriptions
    before these will appear, and to do that, you either have to set
    `gnus-read-active-file` or use the group buffer `M-d`
    command.

‘`o`’
:   ‘`m`’ if moderated.

‘`O`’
:   ‘`(m)`’ if moderated.

‘`s`’
:   Select method.

‘`B`’
:   If the summary buffer for the group is open or not.

‘`n`’
:   Select from where.

‘`z`’
:   A string that looks like ‘`<%s:%n>`’ if a foreign select method is
    used.

‘`P`’
:   Indentation based on the level of the topic (see [Group Topics](#Group-Topics)).

‘`c`’ [¶](#index-gnus_002dgroup_002duncollapsed_002dlevels)
:   Short (collapsed) group name. The `gnus-group-uncollapsed-levels`
    variable says how many levels to leave at the end of the group name.
    The default is 1—this will mean that group names like
    ‘`gnu.emacs.gnus`’ will be shortened to ‘`g.e.gnus`’.

‘`m`’ [¶](#index-gnus_002dnew_002dmail_002dmark)
:   ‘`%`’ (`gnus-new-mail-mark`) if there has arrived new mail to
    the group lately.

‘`p`’
:   ‘`#`’ (`gnus-process-mark`) if the group is process marked.

‘`d`’
:   A string that says when you last read the group (see [Group Timestamp](#Group-Timestamp)).

‘`F`’
:   The disk space used by the articles fetched by both the cache and
    agent. The value is automatically scaled to bytes(B), kilobytes(K),
    megabytes(M), or gigabytes(G) to minimize the column width. A format
    of %7F is sufficient for a fixed-width column.

‘`u`’
:   User defined specifier. The next character in the format string should
    be a letter. Gnus will call the function
    `gnus-user-format-function-`‘`X`’, where ‘`X`’ is the letter
    following ‘`%u`’. The function will be passed a single dummy
    parameter as argument. The function should return a string, which will
    be inserted into the buffer just like information from any other
    specifier.

All the “number-of” specs will be filled with an asterisk (‘`*`’)
if no info is available—for instance, if it is a non-activated foreign
group, or a bogus native group.

---

Next: [Group Highlighting](#Group-Highlighting), Previous: [Group Line Specification](#Group-Line-Specification), Up: [Group Buffer Format](#Group-Buffer-Format)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.1.2 Group Mode Line Specification [¶](#Group-Mode-Line-Specification-1)

The mode line can be changed by setting
`gnus-group-mode-line-format` (see [Mode Line Formatting](#Mode-Line-Formatting)). It
doesn’t understand that many format specifiers:

‘`S`’
:   The native news server.

‘`M`’
:   The native select method.

---

Previous: [Group Mode Line Specification](#Group-Mode-Line-Specification), Up: [Group Buffer Format](#Group-Buffer-Format)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

#### 3.1.3 Group Highlighting [¶](#Group-Highlighting-1)

Highlighting in the group buffer is controlled by the
`gnus-group-highlight` variable. This is an alist with elements
that look like `(form . face)`. If form evaluates to
something non-`nil`, the face will be used on the line.

Here’s an example value for this variable that might look nice if the
background is dark:

```
(cond (window-system
       (setq custom-background-mode 'light)
       (defface my-group-face-1
         '((t (:foreground "Red" :bold t))) "First group face")
       (defface my-group-face-2
         '((t (:foreground "DarkSeaGreen4" :bold t)))
         "Second group face")
       (defface my-group-face-3
         '((t (:foreground "Green4" :bold t))) "Third group face")
       (defface my-group-face-4
         '((t (:foreground "SteelBlue" :bold t))) "Fourth group face")
       (defface my-group-face-5
         '((t (:foreground "Blue" :bold t))) "Fifth group face")))

(setq gnus-group-highlight
      '(((> unread 200) . my-group-face-1)
        ((and (< level 3) (zerop unread)) . my-group-face-2)
        ((< level 3) . my-group-face-3)
        ((zerop unread) . my-group-face-4)
        (t . my-group-face-5)))
```

Also see [Faces and Fonts](#Faces-and-Fonts).

Variables that are dynamically bound when the forms are evaluated
include:

`group`
:   The group name.

`unread`
:   The number of unread articles in the group.

`method`
:   The select method.

`mailp`
:   Whether the group is a mail group.

`level`
:   The level of the group.

`score`
:   The score of the group.

`ticked`
:   The number of ticked articles in the group.

`total`
:   The total number of articles in the group. Or rather,
    max-number minus min-number plus one.

`topic`
:   When using the topic minor mode, this variable is bound to the current
    topic being inserted.

When the forms are `eval`ed, point is at the beginning of the line
of the group in question, so you can use many of the normal Gnus
functions for snarfing info on the group.

`gnus-group-update-hook` is called when a group line is changed.
It will not be called when `gnus-visual` is `nil`.

---

Next: [Selecting a Group](#Selecting-a-Group), Previous: [Group Buffer Format](#Group-Buffer-Format), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.2 Group Maneuvering [¶](#Group-Maneuvering-1)

All movement commands understand the numeric prefix and will behave as
expected, hopefully.

`n` [¶](#index-n-_0028Group_0029)
:   Go to the next group that has unread articles
    (`gnus-group-next-unread-group`).

`p` [¶](#index-DEL-_0028Group_0029)

`DEL`
:   Go to the previous group that has unread articles
    (`gnus-group-prev-unread-group`).

`N` [¶](#index-N-_0028Group_0029)
:   Go to the next group (`gnus-group-next-group`).

`P` [¶](#index-P-_0028Group_0029)
:   Go to the previous group (`gnus-group-prev-group`).

`M-n` [¶](#index-M_002dn-_0028Group_0029)
:   Go to the next unread group on the same (or lower) level
    (`gnus-group-next-unread-group-same-level`).

`M-p` [¶](#index-M_002dp-_0028Group_0029)
:   Go to the previous unread group on the same (or lower) level
    (`gnus-group-prev-unread-group-same-level`).

Three commands for jumping to groups:

`j` [¶](#index-j-_0028Group_0029)
:   Jump to a group (and make it visible if it isn’t already)
    (`gnus-group-jump-to-group`). Killed groups can be jumped to, just
    like living groups.

`,` [¶](#index-_002c-_0028Group_0029)
:   Jump to the unread group with the lowest level
    (`gnus-group-best-unread-group`).

`.` [¶](#index-_002e-_0028Group_0029)
:   Jump to the first group with unread articles
    (`gnus-group-first-unread-group`).

If `gnus-group-goto-unread` is `nil`, all the movement
commands will move to the next group, not the next unread group. Even
the commands that say they move to the next unread group. The default
is `t`.

If `gnus-summary-next-group-on-exit` is `t`, when a summary is
exited, the point in the group buffer is moved to the next unread group.
Otherwise, the point is set to the group just exited. The default is
`t`.

---

Next: [Subscription Commands](#Subscription-Commands), Previous: [Group Maneuvering](#Group-Maneuvering), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.3 Selecting a Group [¶](#Selecting-a-Group-1)

`SPC` [¶](#index-SPC-_0028Group_0029)
:   Select the current group, switch to the summary buffer and display the
    first unread article (`gnus-group-read-group`). If there are no
    unread articles in the group, or if you give a non-numerical prefix to
    this command, Gnus will offer to fetch all the old articles in this
    group from the server. If you give a numerical prefix n, n
    determines the number of articles Gnus will fetch. If n is
    positive, Gnus fetches the n newest articles, if n is
    negative, Gnus fetches the `abs(n)` oldest articles.

    Thus, `SPC` enters the group normally, `C-u SPC`
    offers old articles, `C-u 4 2 SPC` fetches the 42 newest
    articles, and `C-u - 4 2 SPC` fetches the 42 oldest ones.

    When you are in the group (in the Summary buffer), you can type
    `M-g` to fetch new articles, or `C-u M-g` to also show the old
    ones.

`RET` [¶](#index-RET-_0028Group_0029)
:   Select the current group and switch to the summary buffer
    (`gnus-group-select-group`). Takes the same arguments as
    `gnus-group-read-group`—the only difference is that this command
    does not display the first unread article automatically upon group
    entry.

`M-RET` [¶](#index-M_002dRET-_0028Group_0029)
:   This does the same as the command above, but tries to do it with the
    minimum amount of fuzz (`gnus-group-quick-select-group`). No
    scoring/killing will be performed, there will be no highlights and no
    expunging. This might be useful if you’re in a real hurry and have to
    enter some humongous group. If you give a 0 prefix to this command
    (i.e., `0 M-RET`), Gnus won’t even generate the summary buffer,
    which is useful if you want to toggle threading before generating the
    summary buffer (see [Summary Generation Commands](#Summary-Generation-Commands)).

`M-SPC` [¶](#index-M_002dSPC-_0028Group_0029)
:   This is yet one more command that does the same as the `RET`
    command, but this one does it without expunging and hiding dormants
    (`gnus-group-visible-select-group`).

`C-M-RET` [¶](#index-C_002dM_002dRET-_0028Group_0029)
:   Finally, this command selects the current group ephemerally without
    doing any processing of its contents
    (`gnus-group-select-group-ephemerally`). Even threading has been
    turned off. Everything you do in the group after selecting it in this
    manner will have no permanent effects.

The `gnus-large-newsgroup` variable says what Gnus should
consider to be a big group. If it is `nil`, no groups are
considered big. The default value is 200. If the group has more
(unread and/or ticked) articles than this, Gnus will query the user
before entering the group. The user can then specify how many
articles should be fetched from the server. If the user specifies a
negative number (−n), the n oldest articles will be
fetched. If it is positive, the n articles that have arrived
most recently will be fetched.

`gnus-large-ephemeral-newsgroup` is the same as
`gnus-large-newsgroup`, but is only used for ephemeral
newsgroups.

In groups in some news servers, there might be a big gap between a few
very old articles that will never be expired and the recent ones. In
such a case, the server will return the data like `(1 . 30000000)`
for the `LIST ACTIVE group` command, for example. Even if there
are actually only the articles 1–10 and 29999900–30000000, Gnus doesn’t
know it at first and prepares for getting 30000000 articles. However,
it will consume hundreds megabytes of memories and might make Emacs get
stuck as the case may be. If you use such news servers, set the
variable `gnus-newsgroup-maximum-articles` to a positive number.
The value means that Gnus ignores articles other than this number of the
latest ones in every group. For instance, the value 10000 makes Gnus
get only the articles 29990001–30000000 (if the latest article number is
30000000 in a group). Note that setting this variable to a number might
prevent you from reading very old articles. The default value of the
variable `gnus-newsgroup-maximum-articles` is `nil`, which
means Gnus never ignores old articles.

If `gnus-auto-select-first` is non-`nil`, select an article
automatically when entering a group with the `SPC` command.
Which article this is controlled by the
`gnus-auto-select-subject` variable. Valid values for this
variable are:

`unread`
:   Place point on the subject line of the first unread article.

`first`
:   Place point on the subject line of the first article.

`unseen`
:   Place point on the subject line of the first unseen article.

`unseen-or-unread`
:   Place point on the subject line of the first unseen article, and if
    there is no such article, place point on the subject line of the first
    unread article.

`best`
:   Place point on the subject line of the highest-scored unread article.

This variable can also be a function. In that case, that function
will be called to place point on a subject line.

If you want to prevent automatic selection in some group (say, in a
binary group with Huge articles) you can set the
`gnus-auto-select-first` variable to `nil` in
`gnus-select-group-hook`, which is called when a group is
selected.

---

Next: [Group Data](#Group-Data), Previous: [Selecting a Group](#Selecting-a-Group), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.4 Subscription Commands [¶](#Subscription-Commands-1)

The following commands allow for managing your subscriptions in the
Group buffer. If you want to subscribe to many groups, it’s probably
more convenient to go to the [Server Buffer](#Server-Buffer), and choose the
server there using `RET` or `SPC`. Then you’ll have the
commands listed in [Browse Foreign Server](#Browse-Foreign-Server) at hand.

`S t` [¶](#index-S-t-_0028Group_0029)

`u`
:   Toggle subscription to group under point
    (`gnus-group-toggle-subscription-at-point`).

`S s` [¶](#index-S-s-_0028Group_0029)

`U`
:   Prompt for group, and toggle its subscription.
    (`gnus-group-toggle-subscription`).

`S k` [¶](#index-S-k-_0028Group_0029)

`C-k`
:   Kill the current group (`gnus-group-kill-group`).

`S y` [¶](#index-S-y-_0028Group_0029)

`C-y`
:   Yank the last killed group (`gnus-group-yank-group`).

`C-x C-t` [¶](#index-C_002dx-C_002dt-_0028Group_0029)
:   Transpose two groups (`gnus-group-transpose-groups`). This isn’t
    really a subscription command, but you can use it instead of a
    kill-and-yank sequence sometimes.

`S w` [¶](#index-S-w-_0028Group_0029)

`C-w`
:   Kill all groups in the region (`gnus-group-kill-region`).

`S z` [¶](#index-S-z-_0028Group_0029)
:   Kill all zombie groups (`gnus-group-kill-all-zombies`).

`S C-k` [¶](#index-S-C_002dk-_0028Group_0029)
:   Kill all groups on a certain level (`gnus-group-kill-level`).
    These groups can’t be yanked back after killing, so this command should
    be used with some caution. The only time where this command comes in
    really handy is when you have a `.newsrc` with lots of unsubscribed
    groups that you want to get rid off. `S C-k` on level 7 will
    kill off all unsubscribed groups that do not have message numbers in the
    `.newsrc` file.

Also see [Group Levels](#Group-Levels).

---

Next: [Group Levels](#Group-Levels), Previous: [Subscription Commands](#Subscription-Commands), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]

### 3.5 Group Data [¶](#Group-Data-1)

`c` [¶](#index-c-_0028Group_0029)
:   Mark all unticked articles in this group as read
    (`gnus-group-catchup-current`).
    `gnus-group-catchup-group-hook` is called when catching up a group from
    the group buffer.

`C` [¶](#index-C-_0028Group_0029)
:   Mark all articles in this group, even the ticked ones, as read
    (`gnus-group-catchup-current-all`).

`M-c` [¶](#index-M_002dc-_0028Group_0029)
:   Clear the data from the current group—nix out marks and the list of
    read articles (`gnus-group-clear-data`).

`M-x gnus-group-clear-data-on-native-groups` [¶](#index-gnus_002dgroup_002dclear_002ddata_002don_002dnative_002dgroups-1)
:   If you have switched from one NNTP server to another, all your marks
    and read ranges have become worthless. You can use this command to
    clear out all data that you have on your native groups. Use with
    caution.

---

Next: [Group Score](#Group-Score), Previous: [Group Data](#Group-Data), Up: [Group Buffer](#Group-Buffer)   [[Contents](#SEC_Contents "Table of contents")][[Index](#Index "Index")]
