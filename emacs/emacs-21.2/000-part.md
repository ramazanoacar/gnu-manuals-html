|  |  |  |  |
| --- | --- | --- | --- |
| [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] |# GNU Emacs Manual

Emacs is the extensible, customizable, self-documenting real-time
display editor. This Info file describes how to edit with Emacs and
some of how to customize it; it corresponds to GNU Emacs version
21.2.
For information on extending Emacs, see section `Top' in The Emacs Lisp Reference Manual.

> |  |  |  |
> | --- | --- | --- |
> | [Distribution](emacs.html#SEC1) |  | How to get the latest Emacs distribution. |
> | [GNU GENERAL PUBLIC LICENSE](emacs.html#SEC2) |  | The GNU General Public License gives you permission to redistribute GNU Emacs on certain terms; it also explains that there is no warranty. |
> | [A. GNU Free Documentation License](emacs.html#SEC5) |  | The license for this documentation. |
> | [Introduction](emacs.html#SEC7) |  | An introduction to Emacs concepts. |
> | [Glossary](emacs.html#SEC573) |  | The glossary. |
> | [AF. Emacs 20 Antinews](emacs.html#SEC546) |  | Information about Emacs version 20. |
> | [AG. Emacs and the Mac OS](emacs.html#SEC547) |  | Using Emacs in the Mac. |
> | [AH. Emacs and MS-DOS](emacs.html#SEC554) |  | Using Emacs on MS-DOS (otherwise known as "MS-DOG"). |
> | [The GNU Manifesto](emacs.html#SEC564) |  | What's GNU? Gnu's Not Unix! |
> | [AI. Acknowledgments](emacs.html#SEC574) |  | Major contributors to GNU Emacs. |
>
>   
> Indexes (nodes containing large menus)
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [Key (Character) Index](emacs.html#SEC575) |  | An item for each standard Emacs key sequence. |
> | [Command and Function Index](emacs.html#SEC577) |  | An item for each command name. |
> | [Variable Index](emacs.html#SEC578) |  | An item for each documented variable. |
> | [Concept Index](emacs.html#SEC579) |  | An item for each concept. |
> | [Command-Line Options Index](emacs.html#SEC576) |  | An item for every command-line option. |
>
>   
> Important General Concepts
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [B. The Organization of the Screen](emacs.html#SEC8) |  | How to interpret what you see on the screen. |
> | [B.5 Kinds of User Input](emacs.html#SEC13) |  | Kinds of input events (characters, buttons, function keys). |
> | [B.6 Keys](emacs.html#SEC14) |  | Key sequences: what you type to request one editing action. |
> | [B.7 Keys and Commands](emacs.html#SEC15) |  | Named functions run by key sequences to do editing. |
> | [B.8 Character Set for Text](emacs.html#SEC16) |  | Character set for text (the contents of buffers and strings). |
> | [C. Entering and Exiting Emacs](emacs.html#SEC17) |  | Starting Emacs from the shell. |
> | [C.1 Exiting Emacs](emacs.html#SEC18) |  | Stopping or killing Emacs. |
> | [AE. Command Line Arguments](emacs.html#SEC528) |  | Hairy startup options. |
>
>   
> Fundamental Editing Commands
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [D. Basic Editing Commands](emacs.html#SEC19) |  | The most basic editing commands. |
> | [E. The Minibuffer](emacs.html#SEC31) |  | Entering arguments that are prompted for. |
> | [F. Running Commands by Name](emacs.html#SEC41) |  | Invoking commands by their names. |
> | [G. Help](emacs.html#SEC42) |  | Commands for asking Emacs about its commands. |
>
>   
> Important Text-Changing Commands
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [H. The Mark and the Region](emacs.html#SEC52) |  | The mark: how to delimit a "region" of text. |
> | [H.7 Deletion and Killing](emacs.html#SEC59) |  | Killing text. |
> | [H.8 Yanking](emacs.html#SEC63) |  | Recovering killed text. Moving text. |
> | [H.9 Accumulating Text](emacs.html#SEC67) |  | Other ways of copying text. |
> | [H.10 Rectangles](emacs.html#SEC68) |  | Operating on the text inside a rectangle on the screen. |
> | [I. Registers](emacs.html#SEC69) |  | Saving a text string or a location in the buffer. |
> | [J. Controlling the Display](emacs.html#SEC77) |  | Controlling what text is displayed. |
> | [K. Searching and Replacement](emacs.html#SEC91) |  | Finding or replacing occurrences of a string. |
> | [L. Commands for Fixing Typos](emacs.html#SEC105) |  | Commands especially useful for fixing typos. |
>
>   
> Major Structures of Emacs
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [M. File Handling](emacs.html#SEC110) |  | All about handling files. |
> | [N. Using Multiple Buffers](emacs.html#SEC172) |  | Multiple buffers; editing several files at once. |
> | [O. Multiple Windows](emacs.html#SEC183) |  | Viewing two pieces of text at once. |
> | [P. Frames and X Windows](emacs.html#SEC191) |  | Running the same Emacs session in multiple X windows. |
> | [Q. International Character Set Support](emacs.html#SEC213) |  | Using non-ASCII character sets (the MULE features). |
>
>   
> Advanced Features
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [R. Major Modes](emacs.html#SEC227) |  | Text mode vs. Lisp mode vs. C mode ... |
> | [S. Indentation](emacs.html#SEC229) |  | Editing the white space at the beginnings of lines. |
> | [T. Commands for Human Languages](emacs.html#SEC233) |  | Commands and modes for editing English. |
> | [U. Editing Programs](emacs.html#SEC268) |  | Commands and modes for editing programs. |
> | [V. Compiling and Testing Programs](emacs.html#SEC316) |  | Compiling, running and debugging programs. |
> | [W. Maintaining Programs](emacs.html#SEC332) |  | Features for maintaining large programs. |
> | [X. Abbrevs](emacs.html#SEC350) |  | How to define text abbreviations to reduce the number of characters you must type. |
> | [Y. Editing Pictures](emacs.html#SEC358) |  | Editing pictures made up of characters using the quarter-plane screen model. |
> | [Z. Sending Mail](emacs.html#SEC363) |  | Sending mail in Emacs. |
> | [AA. Reading Mail with Rmail](emacs.html#SEC374) |  | Reading mail in Emacs. |
> | [AB. Dired, the Directory Editor](emacs.html#SEC396) |  | You can "edit" a directory to manage files in it. |
> | [AC. The Calendar and the Diary](emacs.html#SEC412) |  | The calendar and diary facilities. |
> | [AC.14 Gnus](emacs.html#SEC438) |  | How to read netnews with Emacs. |
> | [AC.15 Running Shell Commands from Emacs](emacs.html#SEC442) |  | Executing shell commands from Emacs. |
> | [AC.16 Using Emacs as a Server](emacs.html#SEC456) |  | Using Emacs as an editing server for `mail`, etc. |
> | [AC.18 Hardcopy Output](emacs.html#SEC458) |  | Printing buffers or regions. |
> | [AC.19 PostScript Hardcopy](emacs.html#SEC459) |  | Printing buffers or regions as PostScript. |
> | [AC.20 Variables for PostScript Hardcopy](emacs.html#SEC460) |  | Customizing the PostScript printing commands. |
> | [AC.21 Sorting Text](emacs.html#SEC461) |  | Sorting lines, paragraphs or pages within Emacs. |
> | [AC.22 Narrowing](emacs.html#SEC462) |  | Restricting display and editing to a portion of the buffer. |
> | [AC.23 Two-Column Editing](emacs.html#SEC463) |  | Splitting apart columns to edit them in side-by-side windows. |
> | [AC.24 Editing Binary Files](emacs.html#SEC464) |  | Using Hexl mode to edit binary files. |
> | [AC.25 Saving Emacs Sessions](emacs.html#SEC465) |  | Saving Emacs state from one session to the next. |
> | [AC.26 Recursive Editing Levels](emacs.html#SEC466) |  | A command can allow you to do editing |
>
>   "within the command".  This is called a
>   
>   "recursive editing level".
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AC.27 Emulation](emacs.html#SEC467) |  | Emulating some other editors with Emacs. |
> | [AC.28 Hyperlinking and Navigation Features](emacs.html#SEC468) |  | Following links in buffers. |
> | [AC.29 Dissociated Press](emacs.html#SEC473) |  | Dissociating text for fun. |
> | [AC.30 Other Amusements](emacs.html#SEC474) |  | Various games and hacks. |
> | [AD. Customization](emacs.html#SEC475) |  | Modifying the behavior of Emacs. |
>
>   
> Recovery from Problems
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.8 Quitting and Aborting](emacs.html#SEC510) |  | Quitting and aborting. |
> | [AD.9 Dealing with Emacs Trouble](emacs.html#SEC511) |  | What to do if Emacs is hung or malfunctioning. |
> | [AD.10 Reporting Bugs](emacs.html#SEC521) |  | How and when to report a bug. |
> | [AD.11 Contributing to Emacs Development](emacs.html#SEC526) |  | How to contribute improvements to Emacs. |
> | [AD.12 How To Get Help with GNU Emacs](emacs.html#SEC527) |  | How to get help for your own Emacs needs. |
>
>   
> Here are some other nodes which are really inferiors of the ones
>   
> already listed, mentioned here so you can get to them in one step:
>   
>   
>  -- The Detailed Node Listing ---
>   
>   
> The Organization of the Screen
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [B.1 Point](emacs.html#SEC9) |  | The place in the text where editing commands operate. |
> | [B.2 The Echo Area](emacs.html#SEC10) |  | Short messages appear at the bottom of the screen. |
> | [B.3 The Mode Line](emacs.html#SEC11) |  | Interpreting the mode line. |
> | [B.4 The Menu Bar](emacs.html#SEC12) |  | How to use the menu bar. |
>
>   
> Basic Editing Commands
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [D.1 Inserting Text](emacs.html#SEC20) |  | Inserting text by simply typing it. |
> | [D.2 Changing the Location of Point](emacs.html#SEC21) |  | How to move the cursor to the place where you want to change something. |
> | [D.3 Erasing Text](emacs.html#SEC22) |  | Deleting and killing text. |
> | [D.4 Undoing Changes](emacs.html#SEC23) |  | Undoing recent changes in the text. |
> | [D.5 Files](emacs.html#SEC24) |  | Visiting, creating, and saving files. |
> | [D.6 Help](emacs.html#SEC25) |  | Asking what a character does. |
> | [D.7 Blank Lines](emacs.html#SEC26) |  | Commands to make or delete blank lines. |
> | [D.8 Continuation Lines](emacs.html#SEC27) |  | Lines too wide for the screen. |
> | [D.9 Cursor Position Information](emacs.html#SEC28) |  | What page, line, row, or column is point on? |
> | [D.10 Numeric Arguments](emacs.html#SEC29) |  | Numeric arguments for repeating a command. |
>
>   
> The Minibuffer
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [E.1 Minibuffers for File Names](emacs.html#SEC32) |  | Entering file names with the minibuffer. |
> | [E.2 Editing in the Minibuffer](emacs.html#SEC33) |  | How to edit in the minibuffer. |
> | [E.3 Completion](emacs.html#SEC34) |  | An abbreviation facility for minibuffer input. |
> | [E.4 Minibuffer History](emacs.html#SEC39) |  | Reusing recent minibuffer arguments. |
> | [E.5 Repeating Minibuffer Commands](emacs.html#SEC40) |  | Re-executing commands that used the minibuffer. |
>
>   
> Help
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [G.1 Help Summary](emacs.html#SEC43) |  | Brief list of all Help commands. |
> | [G.2 Documentation for a Key](emacs.html#SEC44) |  | Asking what a key does in Emacs. |
> | [G.3 Help by Command or Variable Name](emacs.html#SEC45) |  | Asking about a command, variable or function name. |
> | [G.4 Apropos](emacs.html#SEC46) |  | Asking what pertains to a given topic. |
> | [G.5 Keyword Search for Lisp Libraries](emacs.html#SEC47) |  | Finding Lisp libraries by keywords (topics). |
> | [G.6 Help for International Language Support](emacs.html#SEC48) |  | Help relating to international language support. |
> | [G.8 Other Help Commands](emacs.html#SEC50) |  | Other help commands. |
>
>   
> The Mark and the Region
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [H.1 Setting the Mark](emacs.html#SEC53) |  | Commands to set the mark. |
> | [H.2 Transient Mark Mode](emacs.html#SEC54) |  | How to make Emacs highlight the region-- when there is one. |
> | [H.3 Operating on the Region](emacs.html#SEC55) |  | Summary of ways to operate on contents of the region. |
> | [H.4 Commands to Mark Textual Objects](emacs.html#SEC56) |  | Commands to put region around textual units. |
> | [H.5 The Mark Ring](emacs.html#SEC57) |  | Previous mark positions saved so you can go back there. |
> | [H.6 The Global Mark Ring](emacs.html#SEC58) |  | Previous mark positions in various buffers. |
>
>   
> Deletion and Killing
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [H.7.1 Deletion](emacs.html#SEC60) |  | Commands for deleting small amounts of text and blank areas. |
> | [H.7.2 Killing by Lines](emacs.html#SEC61) |  | How to kill entire lines of text at one time. |
> | [H.7.3 Other Kill Commands](emacs.html#SEC62) |  | Commands to kill large regions of text and syntactic units such as words and sentences. |
>
>   
> Yanking
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [H.8.1 The Kill Ring](emacs.html#SEC64) |  | Where killed text is stored. Basic yanking. |
> | [H.8.2 Appending Kills](emacs.html#SEC65) |  | Several kills in a row all yank together. |
> | [H.8.3 Yanking Earlier Kills](emacs.html#SEC66) |  | Yanking something killed some time ago. |
>
>   
> Registers
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [I.1 Saving Positions in Registers](emacs.html#SEC70) |  | Saving positions in registers. |
> | [I.2 Saving Text in Registers](emacs.html#SEC71) |  | Saving text in registers. |
> | [I.3 Saving Rectangles in Registers](emacs.html#SEC72) |  | Saving rectangles in registers. |
> | [I.4 Saving Window Configurations in Registers](emacs.html#SEC73) |  | Saving window configurations in registers. |
> | [I.6 Keeping File Names in Registers](emacs.html#SEC75) |  | File names in registers. |
> | [I.7 Bookmarks](emacs.html#SEC76) |  | Bookmarks are like registers, but persistent. |
>
>   
> Controlling the Display
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [J.6 Scrolling](emacs.html#SEC83) |  | Moving text up and down in a window. |
> | [J.7 Horizontal Scrolling](emacs.html#SEC84) |  | Moving text left and right in a window. |
> | [J.8 Follow Mode](emacs.html#SEC85) |  | Follow mode lets two windows scroll as one. |
> | [J.9 Selective Display](emacs.html#SEC86) |  | Hiding lines with lots of indentation. |
> | [J.10 Optional Mode Line Features](emacs.html#SEC87) |  | Optional mode line display features. |
> | [J.11 How Text Is Displayed](emacs.html#SEC88) |  | How text is normally displayed. |
> | [J.12 Customization of Display](emacs.html#SEC89) |  | Information on variables for customizing display. |
>
>   
> Searching and Replacement
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [K.1 Incremental Search](emacs.html#SEC92) |  | Search happens as you type the string. |
> | [K.2 Nonincremental Search](emacs.html#SEC94) |  | Specify entire string and then search. |
> | [K.3 Word Search](emacs.html#SEC95) |  | Search for sequence of words. |
> | [K.4 Regular Expression Search](emacs.html#SEC96) |  | Search for match for a regexp. |
> | [K.5 Syntax of Regular Expressions](emacs.html#SEC97) |  | Syntax of regular expressions. |
> | [K.6 Searching and Case](emacs.html#SEC98) |  | To ignore case while searching, or not. |
> | [K.7 Replacement Commands](emacs.html#SEC99) |  | Search, and replace some or all matches. |
> | [K.8 Other Search-and-Loop Commands](emacs.html#SEC104) |  | Operating on all matches for some regexp. |
>
>   
> Replacement Commands
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [K.7.1 Unconditional Replacement](emacs.html#SEC100) |  | Replacing all matches for a string. |
> | [K.7.2 Regexp Replacement](emacs.html#SEC101) |  | Replacing all matches for a regexp. |
> | [K.7.3 Replace Commands and Case](emacs.html#SEC102) |  | How replacements preserve case of letters. |
> | [K.7.4 Query Replace](emacs.html#SEC103) |  | How to use querying. |
>
>   
> Commands for Fixing Typos
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [L.1 Killing Your Mistakes](emacs.html#SEC106) |  | Commands to kill a batch of recently entered text. |
> | [L.2 Transposing Text](emacs.html#SEC107) |  | Exchanging two characters, words, lines, lists... |
> | [L.3 Case Conversion](emacs.html#SEC108) |  | Correcting case of last word entered. |
> | [L.4 Checking and Correcting Spelling](emacs.html#SEC109) |  | Apply spelling checker to a word or a whole buffer. |
>
>   
> File Handling
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [M.1 File Names](emacs.html#SEC111) |  | How to type and edit file-name arguments. |
> | [M.2 Visiting Files](emacs.html#SEC112) |  | Visiting a file prepares Emacs to edit the file. |
> | [M.3 Saving Files](emacs.html#SEC113) |  | Saving makes your changes permanent. |
> | [M.4 Reverting a Buffer](emacs.html#SEC121) |  | Reverting cancels all the changes not saved. |
> | [M.5 Auto-Saving: Protection Against Disasters](emacs.html#SEC122) |  | Auto Save periodically protects against loss of data. |
> | [M.6 File Name Aliases](emacs.html#SEC126) |  | Handling multiple names for one file. |
> | [M.7 Version Control](emacs.html#SEC127) |  | Version control systems (RCS, CVS and SCCS). |
> | [M.8 File Directories](emacs.html#SEC163) |  | Creating, deleting, and listing file directories. |
> | [M.9 Comparing Files](emacs.html#SEC164) |  | Finding where two files differ. |
> | [M.10 Miscellaneous File Operations](emacs.html#SEC165) |  | Other things you can do on files. |
> | [M.11 Accessing Compressed Files](emacs.html#SEC166) |  | Accessing compressed files. |
> | [M.13 Remote Files](emacs.html#SEC168) |  | Accessing files on other sites. |
> | [M.14 Quoted File Names](emacs.html#SEC169) |  | Quoting special characters in file names. |
>
>   
> Saving Files
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [M.3.1 Backup Files](emacs.html#SEC114) |  | How Emacs saves the old version of your file. |
> | [M.3.2 Protection against Simultaneous Editing](emacs.html#SEC118) |  | How Emacs protects against simultaneous editing of one file by two users. |
>
>   
> Version Control
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [M.7.1 Introduction to Version Control](emacs.html#SEC128) |  | How version control works in general. |
> | [M.7.2 Version Control and the Mode Line](emacs.html#SEC131) |  | How the mode line shows version control status. |
> | [M.7.3 Basic Editing under Version Control](emacs.html#SEC132) |  | How to edit a file under version control. |
> | [M.7.4 Examining And Comparing Old Versions](emacs.html#SEC137) |  | Examining and comparing old versions. |
> | [M.7.5 The Secondary Commands of VC](emacs.html#SEC138) |  | The commands used a little less frequently. |
> | [M.7.6 Multiple Branches of a File](emacs.html#SEC144) |  | Multiple lines of development. |
> | [M.7.8 Snapshots](emacs.html#SEC152) |  | Sets of file versions treated as a unit. |
> | [M.7.9 Miscellaneous Commands and Features of VC](emacs.html#SEC155) |  | Various other commands and features of VC. |
> | [M.7.10 Customizing VC](emacs.html#SEC159) |  | Variables that change VC's behavior. |
>
>   
> Using Multiple Buffers
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [N.1 Creating and Selecting Buffers](emacs.html#SEC173) |  | Creating a new buffer or reselecting an old one. |
> | [N.2 Listing Existing Buffers](emacs.html#SEC174) |  | Getting a list of buffers that exist. |
> | [N.3 Miscellaneous Buffer Operations](emacs.html#SEC175) |  | Renaming; changing read-onliness; copying text. |
> | [N.4 Killing Buffers](emacs.html#SEC176) |  | Killing buffers you no longer need. |
> | [N.5 Operating on Several Buffers](emacs.html#SEC177) |  | How to go through the list of all buffers and operate variously on several of them. |
> | [N.6 Indirect Buffers](emacs.html#SEC178) |  | An indirect buffer shares the text of another buffer. |
>
>   
> Multiple Windows
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [O.1 Concepts of Emacs Windows](emacs.html#SEC184) |  | Introduction to Emacs windows. |
> | [O.2 Splitting Windows](emacs.html#SEC185) |  | New windows are made by splitting existing windows. |
> | [O.3 Using Other Windows](emacs.html#SEC186) |  | Moving to another window or doing something to it. |
> | [O.4 Displaying in Another Window](emacs.html#SEC187) |  | Finding a file or buffer in another window. |
> | [O.5 Forcing Display in the Same Window](emacs.html#SEC188) |  | Forcing certain buffers to appear in the selected window rather than in another window. |
> | [O.6 Deleting and Rearranging Windows](emacs.html#SEC189) |  | Deleting windows and changing their sizes. |
>
>   
> Frames and X Windows
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [P.1 Mouse Commands for Editing](emacs.html#SEC192) |  | Moving, cutting, and pasting, with the mouse. |
> | [P.2 Secondary Selection](emacs.html#SEC193) |  | Cutting without altering point and mark. |
> | [P.3 Using the Clipboard](emacs.html#SEC194) |  | Using the clipboard for selections. |
> | [P.4 Following References with the Mouse](emacs.html#SEC195) |  | Using the mouse to select an item from a list. |
> | [P.5 Mouse Clicks for Menus](emacs.html#SEC196) |  | Mouse clicks that bring up menus. |
> | [P.6 Mode Line Mouse Commands](emacs.html#SEC197) |  | Mouse clicks on the mode line. |
> | [P.7 Creating Frames](emacs.html#SEC198) |  | Creating additional Emacs frames with various contents. |
> | [P.8 Frame Commands](emacs.html#SEC199) |  | Iconifying, deleting, and switching frames. |
> | [P.9 Making and Using a Speedbar Frame](emacs.html#SEC200) |  | How to make and use a speedbar frame. |
> | [P.10 Multiple Displays](emacs.html#SEC201) |  | How one Emacs job can talk to several displays. |
> | [P.11 Special Buffer Frames](emacs.html#SEC202) |  | You can make certain buffers have their own frames. |
> | [P.12 Setting Frame Parameters](emacs.html#SEC203) |  | Changing the colors and other modes of frames. |
> | [P.13 Scroll Bars](emacs.html#SEC204) |  | How to enable and disable scroll bars; how to use them. |
> | [P.14 Scrolling With "Wheeled" Mice](emacs.html#SEC205) |  | Using mouse wheels for scrolling. |
> | [P.15 Menu Bars](emacs.html#SEC206) |  | Enabling and disabling the menu bar. |
> | [P.16 Tool Bars](emacs.html#SEC207) |  | Enabling and disabling the tool bar. |
> | [P.17 Using Dialog Boxes](emacs.html#SEC208) |  | Controlling use of dialog boxes. |
> | [J.1 Using Multiple Typefaces](emacs.html#SEC78) |  | How to change the display style using faces. |
> | [J.2 Font Lock mode](emacs.html#SEC79) |  | Minor mode for syntactic highlighting using faces. |
> | [J.3 Highlight Changes Mode](emacs.html#SEC80) |  | Using colors to show where you changed the buffer. |
> | [J.4 Interactive Highlighting by Matching](emacs.html#SEC81) |  | Tell Emacs what text to highlight. |
> | [J.5 Trailing Whitespace](emacs.html#SEC82) |  | Showing possibly-spurious trailing whitespace. |
> | [P.18 Tooltips (or "Balloon Help")](emacs.html#SEC209) |  | Showing "tooltips", AKA "ballon help" for active text. |
> | [P.19 Mouse Avoidance](emacs.html#SEC210) |  | Moving the mouse pointer out of the way. |
> | [P.20 Non-Window Terminals](emacs.html#SEC211) |  | Multiple frames on terminals that show only one. |
> | [P.21 Using a Mouse in Terminal Emulators](emacs.html#SEC212) |  | Using the mouse in an XTerm terminal emulator. |
>
>   
> International Character Set Support
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [Q.1 Introduction to International Character Sets](emacs.html#SEC214) |  | Basic concepts of multibyte characters. |
> | [Q.2 Enabling Multibyte Characters](emacs.html#SEC215) |  | Controlling whether to use multibyte characters. |
> | [Q.3 Language Environments](emacs.html#SEC216) |  | Setting things up for the language you use. |
> | [Q.4 Input Methods](emacs.html#SEC217) |  | Entering text characters not on your keyboard. |
> | [Q.5 Selecting an Input Method](emacs.html#SEC218) |  | Specifying your choice of input methods. |
> | [Q.7 Coding Systems](emacs.html#SEC220) |  | Character set conversion when you read and write files, and so on. |
> | [Q.8 Recognizing Coding Systems](emacs.html#SEC221) |  | How Emacs figures out which conversion to use. |
> | [Q.9 Specifying a Coding System](emacs.html#SEC222) |  | Various ways to choose which conversion to use. |
> | [Q.10 Fontsets](emacs.html#SEC223) |  | Fontsets are collections of fonts that cover the whole spectrum of characters. |
> | [Q.11 Defining fontsets](emacs.html#SEC224) |  | Defining a new fontset. |
> | [Q.13 Single-byte Character Set Support](emacs.html#SEC226) |  | You can pick one European character set to use without multibyte characters. |
>
>   
> Major Modes
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [R.1 How Major Modes are Chosen](emacs.html#SEC228) |  | How major modes are specified or chosen. |
>
>   
> Indentation
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [S.1 Indentation Commands and Techniques](emacs.html#SEC230) |  | Various commands and techniques for indentation. |
> | [S.2 Tab Stops](emacs.html#SEC231) |  | You can set arbitrary "tab stops" and then indent to the next tab stop when you want to. |
> | [S.3 Tabs vs. Spaces](emacs.html#SEC232) |  | You can request indentation using just spaces. |
>
>   
> Commands for Human Languages
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [T.1 Words](emacs.html#SEC234) |  | Moving over and killing words. |
> | [T.2 Sentences](emacs.html#SEC235) |  | Moving over and killing sentences. |
> | [T.3 Paragraphs](emacs.html#SEC236) |  | Moving over paragraphs. |
> | [T.4 Pages](emacs.html#SEC237) |  | Moving over pages. |
> | [T.5 Filling Text](emacs.html#SEC238) |  | Filling or justifying text. |
> | [T.6 Case Conversion Commands](emacs.html#SEC244) |  | Changing the case of text. |
> | [T.7 Text Mode](emacs.html#SEC245) |  | The major modes for editing text files. |
> | [T.8 Outline Mode](emacs.html#SEC246) |  | Editing outlines. |
> | [T.9 TeX Mode](emacs.html#SEC252) |  | Editing input to the formatter TeX. |
> | [T.10 Nroff Mode](emacs.html#SEC257) |  | Editing input to the formatter nroff. |
> | [T.11 Editing Formatted Text](emacs.html#SEC258) |  | Editing formatted text directly in WYSIWYG fashion. |
>
>   
> Filling Text
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [T.5.1 Auto Fill Mode](emacs.html#SEC239) |  | Auto Fill mode breaks long lines automatically. |
> | [T.5.3 Explicit Fill Commands](emacs.html#SEC241) |  | Commands to refill paragraphs and center lines. |
> | [T.5.4 The Fill Prefix](emacs.html#SEC242) |  | Filling paragraphs that are indented or in a comment, etc. |
> | [T.5.5 Adaptive Filling](emacs.html#SEC243) |  | How Emacs can determine the fill prefix automatically. |
>
>   
> Editing Programs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.1 Major Modes for Programming Languages](emacs.html#SEC269) |  | Major modes for editing programs. |
> | [U.2 Top-Level Definitions, or Defuns](emacs.html#SEC270) |  | Commands to operate on major top-level parts of a program. |
> | [U.3 Indentation for Programs](emacs.html#SEC275) |  | Adjusting indentation to show the nesting. |
> | [U.5 Manipulating Comments](emacs.html#SEC285) |  | Inserting, killing, and aligning comments. |
> | [U.4 Commands for Editing with Parentheses](emacs.html#SEC281) |  | Commands that operate on parentheses. |
> | [U.6 Documentation Lookup](emacs.html#SEC289) |  | Getting documentation of functions you plan to call. |
> | [U.7 Hideshow minor mode](emacs.html#SEC293) |  | Displaying blocks selectively. |
> | [U.8 Completion for Symbol Names](emacs.html#SEC294) |  | Completion on symbol names of your program or language. |
> | [U.9 Glasses minor mode](emacs.html#SEC295) |  | Making identifiersLikeThis more readable. |
> | [U.10 Other Features Useful for Editing Programs](emacs.html#SEC296) |  | Other Emacs features useful for editing programs. |
> | [U.11 C and Related Modes](emacs.html#SEC297) |  | Special commands of C, C++, Objective-C, Java, and Pike modes. |
> | [U.12 Fortran Mode](emacs.html#SEC303) |  | Fortran mode and its special features. |
> | [U.13 Asm Mode](emacs.html#SEC315) |  | Asm mode and its special features. |
>
>   
> Top-Level Definitions, or Defuns
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.2.1 Left Margin Convention](emacs.html#SEC271) |  | An open-paren or similar opening delimiter starts a defun if it is at the left margin. |
> | [U.2.2 Moving by Defuns](emacs.html#SEC272) |  | Commands to move over or mark a major definition. |
> | [U.2.3 Imenu](emacs.html#SEC273) |  | Making buffer indexes as menus. |
> | [U.2.4 Which Function Mode](emacs.html#SEC274) |  | Which Function mode shows which function you are in. |
>
>   
> Indentation for Programs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.3.1 Basic Program Indentation Commands](emacs.html#SEC276) |  | Indenting a single line. |
> | [U.3.2 Indenting Several Lines](emacs.html#SEC277) |  | Commands to reindent many lines at once. |
> | [U.3.3 Customizing Lisp Indentation](emacs.html#SEC278) |  | Specifying how each Lisp function should be indented. |
> | [U.3.4 Commands for C Indentation](emacs.html#SEC279) |  | Extra features for indenting C and related modes. |
> | [U.3.5 Customizing C Indentation](emacs.html#SEC280) |  | Controlling indentation style for C and related modes. |
>
>   
> Commands for Editing with Parentheses
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.4.1 Expressions with Balanced Parentheses](emacs.html#SEC282) |  | Expressions with balanced parentheses. |
> | [U.4.2 Moving in the Parenthesis Structure](emacs.html#SEC283) |  | Commands for moving up, down and across in the structure of parentheses. |
> | [U.4.3 Automatic Display Of Matching Parentheses](emacs.html#SEC284) |  | Insertion of a close-delimiter flashes matching open. |
>
>   
> Manipulating Comments
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.5.1 Comment Commands](emacs.html#SEC286) |  | Inserting, killing, and indenting comments. |
> | [U.5.2 Multiple Lines of Comments](emacs.html#SEC287) |  | Commands for adding and editing multi-line comments. |
> | [U.5.3 Options Controlling Comments](emacs.html#SEC288) |  | Customizing the comment features. |
>
>   
> Documentation Lookup
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.6.1 Info Documentation Lookup](emacs.html#SEC290) |  | Looking up library functions and commands in Info files. |
> | [U.6.2 Man Page Lookup](emacs.html#SEC291) |  | Looking up man pages of library functions and commands. |
> | [U.6.3 Emacs Lisp Documentation Lookup](emacs.html#SEC292) |  | Looking up Emacs Lisp functions, etc. |
>
>   
> C and Related Modes
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.11.1 C Mode Motion Commands](emacs.html#SEC298) |  | Commands to move by C statements, etc. |
> | [U.11.2 Electric C Characters](emacs.html#SEC299) |  | Colon and other chars can automatically reindent. |
> | [U.11.3 Hungry Delete Feature in C](emacs.html#SEC300) |  | A more powerful DEL command. |
> | [U.11.4 Other Commands for C Mode](emacs.html#SEC301) |  | Filling comments, viewing expansion of macros, and other neat features. |
> | [U.11.5 Comments in C Modes](emacs.html#SEC302) |  | Options for customizing comment style. |
>
>   
> Fortran Mode
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [U.12.1 Motion Commands](emacs.html#SEC304) |  | Moving point by statements or subprograms. |
> | [U.12.2 Fortran Indentation](emacs.html#SEC305) |  | Indentation commands for Fortran. |
> | [U.12.3 Fortran Comments](emacs.html#SEC311) |  | Inserting and aligning comments. |
> | [U.12.4 Fortran Auto Fill Mode](emacs.html#SEC312) |  | Auto fill minor mode for Fortran. |
> | [U.12.5 Checking Columns in Fortran](emacs.html#SEC313) |  | Measuring columns for valid Fortran. |
> | [U.12.6 Fortran Keyword Abbrevs](emacs.html#SEC314) |  | Built-in abbrevs for Fortran keywords. |
>
>   
> Compiling and Testing Programs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [V.1 Running Compilations under Emacs](emacs.html#SEC317) |  | Compiling programs in languages other than Lisp (C, Pascal, etc.). |
> | [V.3 Compilation Mode](emacs.html#SEC319) |  | The mode for visiting compiler errors. |
> | [V.4 Subshells for Compilation](emacs.html#SEC320) |  | Customizing your shell properly for use in the compilation buffer. |
> | [V.5 Running Debuggers Under Emacs](emacs.html#SEC321) |  | Running symbolic debuggers for non-Lisp programs. |
> | [V.6 Executing Lisp Expressions](emacs.html#SEC327) |  | Various modes for editing Lisp programs, with different facilities for running the Lisp programs. |
> | [V.7 Libraries of Lisp Code for Emacs](emacs.html#SEC328) |  | Creating Lisp programs to run in Emacs. |
> | [V.9 Lisp Interaction Buffers](emacs.html#SEC330) |  | Executing Lisp in an Emacs buffer. |
> | [V.8 Evaluating Emacs-Lisp Expressions](emacs.html#SEC329) |  | Executing a single Lisp expression in Emacs. |
> | [V.10 Running an External Lisp](emacs.html#SEC331) |  | Communicating through Emacs with a separate Lisp. |
>
>   
> Running Debuggers Under Emacs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [V.5.1 Starting GUD](emacs.html#SEC322) |  | How to start a debugger subprocess. |
> | [V.5.2 Debugger Operation](emacs.html#SEC323) |  | Connection between the debugger and source buffers. |
> | [V.5.3 Commands of GUD](emacs.html#SEC324) |  | Key bindings for common commands. |
> | [V.5.4 GUD Customization](emacs.html#SEC325) |  | Defining your own commands for GUD. |
>
>   
> Maintaining Programs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [W.1 Change Logs](emacs.html#SEC333) |  | Maintaining a change history for your program. |
> | [W.2 Tags Tables](emacs.html#SEC334) |  | Go direct to any function in your program in one command. Tags remembers which file it is in. |
> | [W.3 Merging Files with Emerge](emacs.html#SEC342) |  | A convenient way of merging two versions of a program. |
>
>   
> Tags Tables
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [W.2.1 Source File Tag Syntax](emacs.html#SEC335) |  | Tag syntax for various types of code and text files. |
> | [W.2.2 Creating Tags Tables](emacs.html#SEC336) |  | Creating a tags table with `etags`. |
> | [W.2.4 Selecting a Tags Table](emacs.html#SEC338) |  | How to visit a tags table. |
> | [W.2.5 Finding a Tag](emacs.html#SEC339) |  | Commands to find the definition of a specific tag. |
> | [W.2.6 Searching and Replacing with Tags Tables](emacs.html#SEC340) |  | Using a tags table for searching and replacing. |
> | [W.2.7 Tags Table Inquiries](emacs.html#SEC341) |  | Listing and finding tags defined in a file. |
>
>   
> Merging Files with Emerge
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [W.3.1 Overview of Emerge](emacs.html#SEC343) |  | How to start Emerge. Basic concepts. |
> | [W.3.2 Submodes of Emerge](emacs.html#SEC344) |  | Fast mode vs. Edit mode. Skip Prefers mode and Auto Advance mode. |
> | [W.3.3 State of a Difference](emacs.html#SEC345) |  | You do the merge by specifying state A or B for each difference. |
> | [W.3.4 Merge Commands](emacs.html#SEC346) |  | Commands for selecting a difference, changing states of differences, etc. |
> | [W.3.5 Exiting Emerge](emacs.html#SEC347) |  | What to do when you've finished the merge. |
> | [W.3.6 Combining the Two Versions](emacs.html#SEC348) |  | How to keep both alternatives for a difference. |
> | [W.3.7 Fine Points of Emerge](emacs.html#SEC349) |  | Misc. |
>
>   
> Abbrevs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [X.1 Abbrev Concepts](emacs.html#SEC351) |  | Fundamentals of defined abbrevs. |
> | [X.2 Defining Abbrevs](emacs.html#SEC352) |  | Defining an abbrev, so it will expand when typed. |
> | [X.3 Controlling Abbrev Expansion](emacs.html#SEC353) |  | Controlling expansion: prefixes, canceling expansion. |
> | [X.4 Examining and Editing Abbrevs](emacs.html#SEC354) |  | Viewing or editing the entire list of defined abbrevs. |
> | [X.5 Saving Abbrevs](emacs.html#SEC355) |  | Saving the entire list of abbrevs for another session. |
> | [X.6 Dynamic Abbrev Expansion](emacs.html#SEC356) |  | Abbreviations for words already in the buffer. |
>
>   
> Editing Pictures
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [Y.1 Basic Editing in Picture Mode](emacs.html#SEC359) |  | Basic concepts and simple commands of Picture Mode. |
> | [Y.2 Controlling Motion after Insert](emacs.html#SEC360) |  | Controlling direction of cursor motion after "self-inserting" characters. |
> | [Y.3 Picture Mode Tabs](emacs.html#SEC361) |  | Various features for tab stops and indentation. |
> | [Y.4 Picture Mode Rectangle Commands](emacs.html#SEC362) |  | Clearing and superimposing rectangles. |
>
>   
> Sending Mail
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [Z.1 The Format of the Mail Buffer](emacs.html#SEC364) |  | Format of the mail being composed. |
> | [Z.2 Mail Header Fields](emacs.html#SEC365) |  | Details of permitted mail header fields. |
> | [Z.3 Mail Aliases](emacs.html#SEC366) |  | Abbreviating and grouping mail addresses. |
> | [Z.4 Mail Mode](emacs.html#SEC367) |  | Special commands for editing mail being composed. |
> | [Z.5 Mail Amusements](emacs.html#SEC372) |  | Distract the NSA's attention; add a fortune to a msg. |
> | [Z.6 Mail-Composition Methods](emacs.html#SEC373) |  | Using alternative mail-composition methods. |
>
>   
> Reading Mail with Rmail
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AA.1 Basic Concepts of Rmail](emacs.html#SEC375) |  | Basic concepts of Rmail, and simple use. |
> | [AA.2 Scrolling Within a Message](emacs.html#SEC376) |  | Scrolling through a message. |
> | [AA.3 Moving Among Messages](emacs.html#SEC377) |  | Moving to another message. |
> | [AA.4 Deleting Messages](emacs.html#SEC378) |  | Deleting and expunging messages. |
> | [AA.5 Rmail Files and Inboxes](emacs.html#SEC379) |  | How mail gets into the Rmail file. |
> | [AA.6 Multiple Rmail Files](emacs.html#SEC380) |  | Using multiple Rmail files. |
> | [AA.7 Copying Messages Out to Files](emacs.html#SEC381) |  | Copying message out to files. |
> | [AA.8 Labels](emacs.html#SEC382) |  | Classifying messages by labeling them. |
> | [AA.9 Rmail Attributes](emacs.html#SEC383) |  | Certain standard labels, called attributes. |
> | [AA.10 Sending Replies](emacs.html#SEC384) |  | Sending replies to messages you are viewing. |
> | [AA.11 Summaries](emacs.html#SEC385) |  | Summaries show brief info on many messages. |
> | [AA.12 Sorting the Rmail File](emacs.html#SEC388) |  | Sorting messages in Rmail. |
> | [AA.13 Display of Messages](emacs.html#SEC389) |  | How Rmail displays a message; customization. |
> | [AA.15 Editing Within a Message](emacs.html#SEC391) |  | Editing message text and headers in Rmail. |
> | [AA.16 Digest Messages](emacs.html#SEC392) |  | Extracting the messages from a digest message. |
> | [AA.17 Converting an Rmail File to Inbox Format](emacs.html#SEC393) |  | Converting an Rmail file to mailbox format. |
> | [AA.18 Reading Rot13 Messages](emacs.html#SEC394) |  | Reading messages encoded in the rot13 code. |
> | [AA.19 `movemail` and POP](emacs.html#SEC395) |  | More details of fetching new mail. |
>
>   
> Dired, the Directory Editor
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AB.1 Entering Dired](emacs.html#SEC397) |  | How to invoke Dired. |
> | [AB.2 Navigation in the Dired Buffer](emacs.html#SEC398) |  | How to move in the Dired buffer. |
> | [AB.3 Deleting Files with Dired](emacs.html#SEC399) |  | Deleting files with Dired. |
> | [AB.4 Flagging Many Files at Once](emacs.html#SEC400) |  | Flagging files based on their names. |
> | [AB.5 Visiting Files in Dired](emacs.html#SEC401) |  | Other file operations through Dired. |
> | [AB.6 Dired Marks vs. Flags](emacs.html#SEC402) |  | Flagging for deletion vs marking. |
> | [AB.7 Operating on Files](emacs.html#SEC403) |  | How to copy, rename, print, compress, etc. either one file or several files. |
> | [AB.8 Shell Commands in Dired](emacs.html#SEC404) |  | Running a shell command on the marked files. |
> | [AB.9 Transforming File Names in Dired](emacs.html#SEC405) |  | Using patterns to rename multiple files. |
> | [AB.10 File Comparison with Dired](emacs.html#SEC406) |  | Running `diff' by way of Dired. |
> | [AB.11 Subdirectories in Dired](emacs.html#SEC407) |  | Adding subdirectories to the Dired buffer. |
> | [AB.12 Moving Over Subdirectories](emacs.html#SEC408) |  | Moving across subdirectories, and up and down. |
> | [AB.13 Hiding Subdirectories](emacs.html#SEC409) |  | Making subdirectories visible or invisible. |
> | [AB.14 Updating the Dired Buffer](emacs.html#SEC410) |  | Discarding lines for files of no interest. |
> | [AB.15 Dired and `find`](emacs.html#SEC411) |  | Using `find' to choose the files for Dired. |
>
>   
> The Calendar and the Diary
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AC.1 Movement in the Calendar](emacs.html#SEC413) |  | Moving through the calendar; selecting a date. |
> | [AC.2 Scrolling in the Calendar](emacs.html#SEC417) |  | Bringing earlier or later months onto the screen. |
> | [AC.3 Counting Days](emacs.html#SEC418) |  | How many days are there between two dates? |
> | [AC.4 Miscellaneous Calendar Commands](emacs.html#SEC419) |  | Exiting or recomputing the calendar. |
> | [AC.5 LaTeX Calendar](emacs.html#SEC420) |  | Print a calendar using LaTeX. |
> | [AC.6 Holidays](emacs.html#SEC421) |  | Displaying dates of holidays. |
> | [AC.7 Times of Sunrise and Sunset](emacs.html#SEC422) |  | Displaying local times of sunrise and sunset. |
> | [AC.8 Phases of the Moon](emacs.html#SEC423) |  | Displaying phases of the moon. |
> | [AC.9 Conversion To and From Other Calendars](emacs.html#SEC424) |  | Converting dates to other calendar systems. |
> | [AC.10 The Diary](emacs.html#SEC429) |  | Displaying events from your diary. |
> | [AC.11 Appointments](emacs.html#SEC435) |  | Reminders when it's time to do something. |
> | [AC.12 Daylight Savings Time](emacs.html#SEC436) |  | How to specify when daylight savings time is active. |
>
>   
> Movement in the Calendar
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AC.1.1 Motion by Standard Lengths of Time](emacs.html#SEC414) |  | Moving by days, weeks, months, and years. |
> | [AC.1.2 Beginning or End of Week, Month or Year](emacs.html#SEC415) |  | Moving to start/end of weeks, months, and years. |
> | [AC.1.3 Specified Dates](emacs.html#SEC416) |  | Moving to the current date or another specific date. |
>
>   
> Conversion To and From Other Calendars
>   
>   
>
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
>
>   
> The Diary
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AC.10.1 Commands Displaying Diary Entries](emacs.html#SEC430) |  | Viewing diary entries and associated calendar dates. |
> | [AC.10.2 The Diary File](emacs.html#SEC431) |  | Entering events in your diary. |
> | [AC.10.3 Date Formats](emacs.html#SEC432) |  | Various ways you can specify dates. |
> | [AC.10.4 Commands to Add to the Diary](emacs.html#SEC433) |  | Commands to create diary entries. |
> | [AC.10.5 Special Diary Entries](emacs.html#SEC434) |  | Anniversaries, blocks of dates, cyclic entries, etc. |
>
>   
> GNUS
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AC.14.1 Gnus Buffers](emacs.html#SEC439) |  | The group, summary, and article buffers. |
> | [AC.14.2 When Gnus Starts Up](emacs.html#SEC440) |  | What you should know about starting Gnus. |
> | [AC.14.3 Summary of Gnus Commands](emacs.html#SEC441) |  | A short description of the basic Gnus commands. |
>
>   
> Running Shell Commands from Emacs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AC.15.1 Single Shell Commands](emacs.html#SEC443) |  | How to run one shell command and return. |
> | [AC.15.2 Interactive Inferior Shell](emacs.html#SEC444) |  | Permanent shell taking input via Emacs. |
> | [AC.15.3 Shell Mode](emacs.html#SEC445) |  | Special Emacs commands used with permanent shell. |
> | [AC.15.4 Shell Command History](emacs.html#SEC446) |  | Repeating previous commands in a shell buffer. |
> | [AC.15.6 Shell Mode Options](emacs.html#SEC451) |  | Options for customizing Shell mode. |
> | [AC.15.10 Remote Host Shell](emacs.html#SEC455) |  | Connecting to another computer. |
>
>   
> Customization
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.1 Minor Modes](emacs.html#SEC476) |  | Each minor mode is one feature you can turn on independently of any others. |
> | [AD.2 Variables](emacs.html#SEC477) |  | Many Emacs commands examine Emacs variables to decide what to do; by setting variables, you can control their functioning. |
> | [AD.3 Keyboard Macros](emacs.html#SEC487) |  | A keyboard macro records a sequence of keystrokes to be replayed with a single command. |
> | [AD.4 Customizing Key Bindings](emacs.html#SEC491) |  | The keymaps say what command each key runs. By changing them, you can "redefine keys". |
> | [AD.5 Keyboard Translations](emacs.html#SEC503) |  | If your keyboard passes an undesired code for a key, you can tell Emacs to substitute another code. |
> | [AD.6 The Syntax Table](emacs.html#SEC504) |  | The syntax table controls how words and expressions are parsed. |
> | [AD.7 The Init File, `~/.emacs'](emacs.html#SEC505) |  | How to write common customizations in the |
>
> `.emacs' file. 
>   
>   
> Variables
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.2.1 Examining and Setting Variables](emacs.html#SEC478) |  | Examining or setting one variable's value. |
> | [AD.2.2 Easy Customization Interface](emacs.html#SEC479) |  | Convenient and easy customization of variables. |
> | [AD.2.3 Hooks](emacs.html#SEC484) |  | Hook variables let you specify programs for parts of Emacs to run on particular occasions. |
> | [AD.2.4 Local Variables](emacs.html#SEC485) |  | Per-buffer values of variables. |
> | [AD.2.5 Local Variables in Files](emacs.html#SEC486) |  | How files can specify variable values. |
>
>   
> Keyboard Macros
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.3.1 Basic Use](emacs.html#SEC488) |  | Defining and running keyboard macros. |
> | [AD.3.2 Naming and Saving Keyboard Macros](emacs.html#SEC489) |  | Giving keyboard macros names; saving them in files. |
> | [AD.3.3 Executing Macros with Variations](emacs.html#SEC490) |  | Making keyboard macros do different things each time. |
>
>   
> Customizing Key Bindings
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.4.1 Keymaps](emacs.html#SEC492) |  | Generalities. The global keymap. |
> | [AD.4.2 Prefix Keymaps](emacs.html#SEC493) |  | Keymaps for prefix keys. |
> | [AD.4.3 Local Keymaps](emacs.html#SEC494) |  | Major and minor modes have their own keymaps. |
> | [AD.4.4 Minibuffer Keymaps](emacs.html#SEC495) |  | The minibuffer uses its own local keymaps. |
> | [AD.4.5 Changing Key Bindings Interactively](emacs.html#SEC496) |  | How to redefine one key's meaning conveniently. |
> | [AD.4.6 Rebinding Keys in Your Init File](emacs.html#SEC497) |  | Rebinding keys with your init file, `.emacs'. |
> | [AD.4.7 Rebinding Function Keys](emacs.html#SEC498) |  | Rebinding terminal function keys. |
> | [AD.4.8 Named ASCII Control Characters](emacs.html#SEC499) |  | Distinguishing `TAB` from `C-i`, and so on. |
> | [AD.4.10 Rebinding Mouse Buttons](emacs.html#SEC501) |  | Rebinding mouse buttons in Emacs. |
> | [AD.4.11 Disabling Commands](emacs.html#SEC502) |  | Disabling a command means confirmation is required before it can be executed. This is done to protect beginners from surprises. |
>
>   
> The Init File, `~/.emacs'
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.7.1 Init File Syntax](emacs.html#SEC506) |  | Syntax of constants in Emacs Lisp. |
> | [AD.7.2 Init File Examples](emacs.html#SEC507) |  | How to do some things with an init file. |
> | [AD.7.3 Terminal-specific Initialization](emacs.html#SEC508) |  | Each terminal type can have an init file. |
> | [AD.7.4 How Emacs Finds Your Init File](emacs.html#SEC509) |  | How Emacs finds the init file. |
>
>   
> Dealing with Emacs Trouble
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.9.1 If `DEL` Fails to Delete](emacs.html#SEC512) |  | What to do if `DEL` doesn't delete. |
> | [AD.9.2 Recursive Editing Levels](emacs.html#SEC513) |  | `[...]' in mode line around the parentheses. |
> | [AD.9.3 Garbage on the Screen](emacs.html#SEC514) |  | Garbage on the screen. |
> | [AD.9.4 Garbage in the Text](emacs.html#SEC515) |  | Garbage in the text. |
> | [AD.9.5 Spontaneous Entry to Incremental Search](emacs.html#SEC516) |  | Spontaneous entry to incremental search. |
> | [AD.9.6 Running out of Memory](emacs.html#SEC517) |  | How to cope when you run out of memory. |
> | [AD.9.8 Emergency Escape](emacs.html#SEC519) |  | Emergency escape--- What to do if Emacs stops responding. |
> | [AD.9.9 Help for Total Frustration](emacs.html#SEC520) |  | When you are at your wits' end. |
>
>   
> Reporting Bugs
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AD.10.1 When Is There a Bug](emacs.html#SEC522) |  | Have you really found a bug? |
> | [AD.10.2 Understanding Bug Reporting](emacs.html#SEC523) |  | How to report a bug effectively. |
> | [AD.10.3 Checklist for Bug Reports](emacs.html#SEC524) |  | Steps to follow for a good bug report. |
> | [AD.10.4 Sending Patches for GNU Emacs](emacs.html#SEC525) |  | How to send a patch for GNU Emacs. |
>
>   
> Command Line Options and Arguments
>   
>   
>
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
>
>   
> Environment Variables
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AE.5.1 General Variables](emacs.html#SEC534) |  | Environment variables that all versions of Emacs use. |
> | [AE.5.2 Miscellaneous Variables](emacs.html#SEC535) |  | Certain system specific variables. |
>
>   
> MS-DOS and Windows 95/98/NT
>   
>   
>
> |  |  |  |
> | --- | --- | --- |
> | [AH.1 Keyboard and Mouse on MS-DOS](emacs.html#SEC555) |  | Keyboard and mouse usage on MS-DOS. |
> | [AH.2 Display on MS-DOS](emacs.html#SEC556) |  | Fonts, frames and display size on MS-DOS. |
> | [AH.3 File Names on MS-DOS](emacs.html#SEC557) |  | File-name conventions on MS-DOS. |
> | [AH.4 Text Files and Binary Files](emacs.html#SEC558) |  | Text files on MS-DOS use CRLF to separate lines. |
> | [AH.5 Printing and MS-DOS](emacs.html#SEC559) |  | How to specify the printer on MS-DOS. |
> | [AH.7 Subprocesses on MS-DOS](emacs.html#SEC561) |  | Running subprocesses on MS-DOS. |
> | [AH.8 Subprocesses on Windows 9X/ME and Windows NT/2K](emacs.html#SEC562) |  | Running subprocesses on Windows. |
> | [AH.9 Using the System Menu on Windows](emacs.html#SEC563) |  | Controlling what the ALT key does. |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC_Top) ] | [ [>](emacs.html#SEC2) ] | [ [<<](emacs.html#SEC_Top) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC2) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# Distribution

GNU Emacs is *free software*; this means that everyone is free to
use it and free to redistribute it on certain conditions. GNU Emacs is
not in the public domain; it is copyrighted and there are restrictions
on its distribution, but these restrictions are designed to permit
everything that a good cooperating citizen would want to do. What is
not allowed is to try to prevent others from further sharing any version
of GNU Emacs that they might get from you. The precise conditions are
found in the GNU General Public License that comes with Emacs and also
appears following this section.

One way to get a copy of GNU Emacs is from someone else who has it. You
need not ask for our permission to do so, or tell any one else; just
copy it. If you have access to the Internet, you can get the latest
distribution version of GNU Emacs by anonymous FTP; see the file
`etc/FTP' in the Emacs distribution for more information.

You may also receive GNU Emacs when you buy a computer. Computer
manufacturers are free to distribute copies on the same terms that apply to
everyone else. These terms require them to give you the full sources,
including whatever changes they may have made, and to permit you to
redistribute the GNU Emacs received from them under the usual terms of the
General Public License. In other words, the program must be free for you
when you get it, not just free for the manufacturer.

You can also order copies of GNU Emacs from the Free Software Foundation
on CD-ROM. This is a convenient and reliable way to get a copy; it is
also a good way to help fund our work. (The Foundation has always
received most of its funds in this way.) An order form is included in
the file `etc/ORDERS' in the Emacs distribution, and on our web
site in <http://www.gnu.org/order/order.html>. For further
information, write to

|  |  |
| --- | --- |
|  | ``` Free Software Foundation 59 Temple Place, Suite 330 Boston, MA  02111-1307 USA USA ``` |

The income from distribution fees goes to support the foundation's
purpose: the development of new free software, and improvements to our
existing programs including GNU Emacs.

If you find GNU Emacs useful, please **send a donation** to the
Free Software Foundation to support our work. Donations to the Free
Software Foundation are tax deductible in the US. If you use GNU Emacs
at your workplace, please suggest that the company make a donation. If
company policy is unsympathetic to the idea of donating to charity, you
might instead suggest ordering a CD-ROM from the Foundation
occasionally, or subscribing to periodic updates.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC1) ] | [ [>](emacs.html#SEC3) ] | [ [<<](emacs.html#SEC1) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC5) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# GNU GENERAL PUBLIC LICENSE

Version 2, June 1991

|  |  |
| --- | --- |
|  | ``` Copyright (C) 1989, 1991 Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA  Everyone is permitted to copy and distribute verbatim copies of this license document, but changing it is not allowed. ``` |

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC2) ] | [ [>](emacs.html#SEC4) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## Preamble

The licenses for most software are designed to take away your
freedom to share and change it. By contrast, the GNU General Public
License is intended to guarantee your freedom to share and change free
software--to make sure the software is free for all its users. This
General Public License applies to most of the Free Software
Foundation's software and to any other program whose authors commit to
using it. (Some other Free Software Foundation software is covered by
the GNU Library General Public License instead.) You can apply it to
your programs, too.

When we speak of free software, we are referring to freedom, not
price. Our General Public Licenses are designed to make sure that you
have the freedom to distribute copies of free software (and charge for
this service if you wish), that you receive source code or can get it
if you want it, that you can change the software or use pieces of it
in new free programs; and that you know you can do these things.

To protect your rights, we need to make restrictions that forbid
anyone to deny you these rights or to ask you to surrender the rights.
These restrictions translate to certain responsibilities for you if you
distribute copies of the software, or if you modify it.

For example, if you distribute copies of such a program, whether
gratis or for a fee, you must give the recipients all the rights that
you have. You must make sure that they, too, receive or can get the
source code. And you must show them these terms so they know their
rights.

We protect your rights with two steps: (1) copyright the software, and
(2) offer you this license which gives you legal permission to copy,
distribute and/or modify the software.

Also, for each author's protection and ours, we want to make certain
that everyone understands that there is no warranty for this free
software. If the software is modified by someone else and passed on, we
want its recipients to know that what they have is not the original, so
that any problems introduced by others will not reflect on the original
authors' reputations.

Finally, any free program is threatened constantly by software
patents. We wish to avoid the danger that redistributors of a free
program will individually obtain patent licenses, in effect making the
program proprietary. To prevent this, we have made it clear that any
patent must be licensed for everyone's free use or not licensed at all.

The precise terms and conditions for copying, distribution and
modification follow.

TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION

1. This License applies to any program or other work which contains
   a notice placed by the copyright holder saying it may be distributed
   under the terms of this General Public License. The "Program," below,
   refers to any such program or work, and a "work based on the Program"
   means either the Program or any derivative work under copyright law:
   that is to say, a work containing the Program or a portion of it,
   either verbatim or with modifications and/or translated into another
   language. (Hereinafter, translation is included without limitation in
   the term "modification.") Each licensee is addressed as "you."

   Activities other than copying, distribution and modification are not
   covered by this License; they are outside its scope. The act of
   running the Program is not restricted, and the output from the Program
   is covered only if its contents constitute a work based on the
   Program (independent of having been made by running the Program).
   Whether that is true depends on what the Program does.

   - You may copy and distribute verbatim copies of the Program's
     source code as you receive it, in any medium, provided that you
     conspicuously and appropriately publish on each copy an appropriate
     copyright notice and disclaimer of warranty; keep intact all the
     notices that refer to this License and to the absence of any warranty;
     and give any other recipients of the Program a copy of this License
     along with the Program.

     You may charge a fee for the physical act of transferring a copy, and
     you may at your option offer warranty protection in exchange for a fee.

     - You may modify your copy or copies of the Program or any portion
       of it, thus forming a work based on the Program, and copy and
       distribute such modifications or work under the terms of Section 1
       above, provided that you also meet all of these conditions:

       1. You must cause the modified files to carry prominent notices
          stating that you changed the files and the date of any change.

          - You must cause any work that you distribute or publish, that in
            whole or in part contains or is derived from the Program or any
            part thereof, to be licensed as a whole at no charge to all third
            parties under the terms of this License.

            - If the modified program normally reads commands interactively
              when run, you must cause it, when started running for such
              interactive use in the most ordinary way, to print or display an
              announcement including an appropriate copyright notice and a
              notice that there is no warranty (or else, saying that you provide
              a warranty) and that users may redistribute the program under
              these conditions, and telling the user how to view a copy of this
              License. (Exception: if the Program itself is interactive but
              does not normally print such an announcement, your work based on
              the Program is not required to print an announcement.)

       These requirements apply to the modified work as a whole. If
       identifiable sections of that work are not derived from the Program,
       and can be reasonably considered independent and separate works in
       themselves, then this License, and its terms, do not apply to those
       sections when you distribute them as separate works. But when you
       distribute the same sections as part of a whole which is a work based
       on the Program, the distribution of the whole must be on the terms of
       this License, whose permissions for other licensees extend to the
       entire whole, and thus to each and every part regardless of who wrote it.

       Thus, it is not the intent of this section to claim rights or contest
       your rights to work written entirely by you; rather, the intent is to
       exercise the right to control the distribution of derivative or
       collective works based on the Program.

       In addition, mere aggregation of another work not based on the Program
       with the Program (or with a work based on the Program) on a volume of
       a storage or distribution medium does not bring the other work under
       the scope of this License.

       - You may copy and distribute the Program (or a work based on it,
         under Section 2) in object code or executable form under the terms of
         Sections 1 and 2 above provided that you also do one of the following:

         1. Accompany it with the complete corresponding machine-readable
            source code, which must be distributed under the terms of Sections
            1 and 2 above on a medium customarily used for software interchange; or,

            - Accompany it with a written offer, valid for at least three
              years, to give any third party, for a charge no more than your
              cost of physically performing source distribution, a complete
              machine-readable copy of the corresponding source code, to be
              distributed under the terms of Sections 1 and 2 above on a medium
              customarily used for software interchange; or,

              - Accompany it with the information you received as to the offer
                to distribute corresponding source code. (This alternative is
                allowed only for noncommercial distribution and only if you
                received the program in object code or executable form with such
                an offer, in accord with Subsection b above.)

         The source code for a work means the preferred form of the work for
         making modifications to it. For an executable work, complete source
         code means all the source code for all modules it contains, plus any
         associated interface definition files, plus the scripts used to
         control compilation and installation of the executable. However, as a
         special exception, the source code distributed need not include
         anything that is normally distributed (in either source or binary
         form) with the major components (compiler, kernel, and so on) of the
         operating system on which the executable runs, unless that component
         itself accompanies the executable.

         If distribution of executable or object code is made by offering
         access to copy from a designated place, then offering equivalent
         access to copy the source code from the same place counts as
         distribution of the source code, even though third parties are not
         compelled to copy the source along with the object code.

         - You may not copy, modify, sublicense, or distribute the Program
           except as expressly provided under this License. Any attempt
           otherwise to copy, modify, sublicense or distribute the Program is
           void, and will automatically terminate your rights under this License.
           However, parties who have received copies, or rights, from you under
           this License will not have their licenses terminated so long as such
           parties remain in full compliance.

           - You are not required to accept this License, since you have not
             signed it. However, nothing else grants you permission to modify or
             distribute the Program or its derivative works. These actions are
             prohibited by law if you do not accept this License. Therefore, by
             modifying or distributing the Program (or any work based on the
             Program), you indicate your acceptance of this License to do so, and
             all its terms and conditions for copying, distributing or modifying
             the Program or works based on it.

             - Each time you redistribute the Program (or any work based on the
               Program), the recipient automatically receives a license from the
               original licensor to copy, distribute or modify the Program subject to
               these terms and conditions. You may not impose any further
               restrictions on the recipients' exercise of the rights granted herein.
               You are not responsible for enforcing compliance by third parties to
               this License.

               - If, as a consequence of a court judgment or allegation of patent
                 infringement or for any other reason (not limited to patent issues),
                 conditions are imposed on you (whether by court order, agreement or
                 otherwise) that contradict the conditions of this License, they do not
                 excuse you from the conditions of this License. If you cannot
                 distribute so as to satisfy simultaneously your obligations under this
                 License and any other pertinent obligations, then as a consequence you
                 may not distribute the Program at all. For example, if a patent
                 license would not permit royalty-free redistribution of the Program by
                 all those who receive copies directly or indirectly through you, then
                 the only way you could satisfy both it and this License would be to
                 refrain entirely from distribution of the Program.

                 If any portion of this section is held invalid or unenforceable under
                 any particular circumstance, the balance of the section is intended to
                 apply and the section as a whole is intended to apply in other
                 circumstances.

                 It is not the purpose of this section to induce you to infringe any
                 patents or other property right claims or to contest validity of any
                 such claims; this section has the sole purpose of protecting the
                 integrity of the free software distribution system, which is
                 implemented by public license practices. Many people have made
                 generous contributions to the wide range of software distributed
                 through that system in reliance on consistent application of that
                 system; it is up to the author/donor to decide if he or she is willing
                 to distribute software through any other system and a licensee cannot
                 impose that choice.

                 This section is intended to make thoroughly clear what is believed to
                 be a consequence of the rest of this License.

                 - If the distribution and/or use of the Program is restricted in
                   certain countries either by patents or by copyrighted interfaces, the
                   original copyright holder who places the Program under this License
                   may add an explicit geographical distribution limitation excluding
                   those countries, so that distribution is permitted only in or among
                   countries not thus excluded. In such case, this License incorporates
                   the limitation as if written in the body of this License.

                   - The Free Software Foundation may publish revised and/or new versions
                     of the General Public License from time to time. Such new versions will
                     be similar in spirit to the present version, but may differ in detail to
                     address new problems or concerns.

                     Each version is given a distinguishing version number. If the Program
                     specifies a version number of this License which applies to it and "any
                     later version," you have the option of following the terms and conditions
                     either of that version or of any later version published by the Free
                     Software Foundation. If the Program does not specify a version number of
                     this License, you may choose any version ever published by the Free Software
                     Foundation.

                     - If you wish to incorporate parts of the Program into other free
                       programs whose distribution conditions are different, write to the author
                       to ask for permission. For software which is copyrighted by the Free
                       Software Foundation, write to the Free Software Foundation; we sometimes
                       make exceptions for this. Our decision will be guided by the two goals
                       of preserving the free status of all derivatives of our free software and
                       of promoting the sharing and reuse of software generally.

                       NO WARRANTY

                       - BECAUSE THE PROGRAM IS LICENSED FREE OF CHARGE, THERE IS NO WARRANTY
                         FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW. EXCEPT WHEN
                         OTHERWISE STATED IN WRITING THE COPYRIGHT HOLDERS AND/OR OTHER PARTIES
                         PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED
                         OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
                         MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK AS
                         TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE
                         PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING,
                         REPAIR OR CORRECTION.

                         - IN NO EVENT UNLESS REQUIRED BY APPLICABLE LAW OR AGREED TO IN WRITING
                           WILL ANY COPYRIGHT HOLDER, OR ANY OTHER PARTY WHO MAY MODIFY AND/OR
                           REDISTRIBUTE THE PROGRAM AS PERMITTED ABOVE, BE LIABLE TO YOU FOR DAMAGES,
                           INCLUDING ANY GENERAL, SPECIAL, INCIDENTAL OR CONSEQUENTIAL DAMAGES ARISING
                           OUT OF THE USE OR INABILITY TO USE THE PROGRAM (INCLUDING BUT NOT LIMITED
                           TO LOSS OF DATA OR DATA BEING RENDERED INACCURATE OR LOSSES SUSTAINED BY
                           YOU OR THIRD PARTIES OR A FAILURE OF THE PROGRAM TO OPERATE WITH ANY OTHER
                           PROGRAMS), EVEN IF SUCH HOLDER OR OTHER PARTY HAS BEEN ADVISED OF THE
                           POSSIBILITY OF SUCH DAMAGES.

END OF TERMS AND CONDITIONS

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC3) ] | [ [>](emacs.html#SEC5) ] | [ << ] | [ [Up](emacs.html#SEC_Top) ] | [ >> ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |## How to Apply These Terms to Your New Programs

If you develop a new program, and you want it to be of the greatest
possible use to the public, the best way to achieve this is to make it
free software which everyone can redistribute and change under these terms.

To do so, attach the following notices to the program. It is safest
to attach them to the start of each source file to most effectively
convey the exclusion of warranty; and each file should have at least
the "copyright" line and a pointer to where the full notice is found.

|  |  |
| --- | --- |
|  | ``` one line to give the program's name and an idea of what it does. Copyright (C) 19yy  name of author  This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.  This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.  You should have received a copy of the GNU General Public License along with this program; if not, write to the Free Software Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA 02111-1307, USA. ``` |

Also add information on how to contact you by electronic and paper mail.

If the program is interactive, make it output a short notice like this
when it starts in an interactive mode:

|  |  |
| --- | --- |
|  | ``` Gnomovision version 69, Copyright (C) 20yy name of author Gnomovision comes with ABSOLUTELY NO WARRANTY; for details type `show w'.  This is free software, and you are welcome to redistribute it under certain conditions; type `show c'  for details. ``` |

The hypothetical commands `` `show w' `` and `` `show c' `` should show
the appropriate parts of the General Public License. Of course, the
commands you use may be called something other than `` `show w' `` and
`` `show c' ``; they could even be mouse-clicks or menu items--whatever
suits your program.

You should also get your employer (if you work as a programmer) or your
school, if any, to sign a "copyright disclaimer" for the program, if
necessary. Here is a sample; alter the names:

|  |  |
| --- | --- |
|  | ``` Yoyodyne, Inc., hereby disclaims all copyright interest in the program `Gnomovision' (which makes passes at compilers) written  by James Hacker.  signature of Ty Coon, 1 April 1989 Ty Coon, President of Vice ``` |

This General Public License does not permit incorporating your program into
proprietary programs. If your program is a subroutine library, you may
consider it more useful to permit linking proprietary applications with the
library. If this is what you want to do, use the GNU Library General
Public License instead of this License.

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC4) ] | [ [>](emacs.html#SEC6) ] | [ [<<](emacs.html#SEC2) ] | [ [Up](emacs.html#SEC_Top) ] | [ [>>](emacs.html#SEC7) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |# A. GNU Free Documentation License

Version 1.1, March 2000

|  |  |
| --- | --- |
|  | ``` Copyright (C) 2000  Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA  Everyone is permitted to copy and distribute verbatim copies of this license document, but changing it is not allowed. ``` |

1. PREAMBLE

   The purpose of this License is to make a manual, textbook, or other
   written document "free" in the sense of freedom: to assure everyone
   the effective freedom to copy and redistribute it, with or without
   modifying it, either commercially or noncommercially. Secondarily,
   this License preserves for the author and publisher a way to get
   credit for their work, while not being considered responsible for
   modifications made by others.

   This License is a kind of "copyleft", which means that derivative
   works of the document must themselves be free in the same sense. It
   complements the GNU General Public License, which is a copyleft
   license designed for free software.

   We have designed this License in order to use it for manuals for free
   software, because free software needs free documentation: a free
   program should come with manuals providing the same freedoms that the
   software does. But this License is not limited to software manuals;
   it can be used for any textual work, regardless of subject matter or
   whether it is published as a printed book. We recommend this License
   principally for works whose purpose is instruction or reference.

   - APPLICABILITY AND DEFINITIONS

     This License applies to any manual or other work that contains a
     notice placed by the copyright holder saying it can be distributed
     under the terms of this License. The "Document", below, refers to any
     such manual or work. Any member of the public is a licensee, and is
     addressed as "you."

     A "Modified Version" of the Document means any work containing the
     Document or a portion of it, either copied verbatim, or with
     modifications and/or translated into another language.
