### N.7.2 Switching Between Buffers using Substrings

Iswitchb global minor mode provides convenient switching between
buffers using substrings of their names. It replaces the normal
definitions of `C-x b`, `C-x 4 b`, `C-x 5 b`, and `C-x
4 C-o` with alternative commands that are somewhat "smarter."

When one of these commands prompts you for a buffer name, you can
type in just a substring of the name you want to choose. As you enter
the substring, Iswitchb mode continuously displays a list of buffers
that match the substring you have typed.

At any time, you can type `RET` to select the first buffer in
the list. So the way to select a particular buffer is to make it the
first in the list. There are two ways to do this. You can type more
of the buffer name and thus narrow down the list, excluding unwanted
buffers above the desired one. Alternatively, you can use `C-s`
and `C-r` to rotate the list until the desired buffer is first.

`TAB` while entering the buffer name performs completion on the
string you have entered, based on the displayed list of buffers.

To enable Iswitchb mode, type `M-x iswitchb-mode`, or customize
the variable `iswitchb-mode` to `t` (see section [AD.2.2 Easy Customization Interface](emacs.html#SEC479)).

---

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [ [<](emacs.html#SEC181) ] | [ [>](emacs.html#SEC183) ] | [ [<<](emacs.html#SEC172) ] | [ [Up](emacs.html#SEC179) ] | [ [>>](emacs.html#SEC183) ] | [[Top](emacs.html#SEC_Top)] | [[Contents](emacs.html#SEC_Contents)] | [Index] | [ [?](emacs.html#SEC_About) ] | | | | | |
