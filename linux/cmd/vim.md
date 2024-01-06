# Vim

# vim

[vim](/perl/ide/vim)

[tips](/linux/command/vim/tips)

[note](/linux/command/vim/note)

[vim_color_hurts_my_eyes](/linux/command/vim/vim_color_hurts_my_eyes)

<http://www.computerhope.com/unix/uvi.htm>

<http://linux.vbird.org/linux_basic/0310vi.php>

<https://sikotec.wordpress.com/2008/12/08/linuxvivim-%E5%B0%8B%E6%89%BE%E6%9C%89%E7%84%A1%E5%B0%8D%E6%87%89%E7%9A%84-%E6%88%96/>

Ubuntu 9.10 has installed vim.tiny as default text editor when type "vi"
in a console. It does not response to my keyboard of up/down key. Thus,
I prefer to install vim.

    $sudo atp-get install vim

After installed, "vi" is referred to "vim" automatically by the
installed packages.

# Dos format text

When modify DOS format of text, ^M shows on the screen and is needed to
be cleared.

    :%s/(Ctrl-v)(Ctrl-m)//

# set cursor to previous position

OS: Ubuntu 11.04 64bit desktop

When open a file by vi, it is useful to set cursor to previous position
when exit.

Alter vimrc configuration file

    #vi /etc/vim/vimrc

Uncommand below lines from

    " Uncomment the following to have Vim jump to the last position when
    " reopening a file
    "if has("autocmd")
    "  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
    "endif

to

    " Uncomment the following to have Vim jump to the last position when
    " reopening a file
    if has("autocmd")
      au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe normal! g'\"" | endif
    endif

# \~/.vimrc

vi \~/.vimrc

    "set background=dark
    "set cursorline
    "set enc=utf8
    "set hls
    "set number
    "set ic
    "set wrap
    "set nowrap 
    "set expandtab
    "set tabstop=2

    set foldenable                                                                                                                                     
    "set foldmethod=indent
    "set foldcolumn=1
    "set foldlevel=5

    set shiftwidth=2
    set scrolloff=2
    set cursorline
    "set cursorcolumn

# Remove blank lines

Power of g

<http://vim.wikia.com/wiki/Power_of_g>

    :g/^$/d

:g will execute a command on lines which match a regex. The regex is
'blank line' and the command is :d (delete)

Remove a line contents only space in it. ( There is a blank, space in
between ^ and \* )

    :g/^ *$/d

<http://vim.wikia.com/wiki/Remove_unwanted_empty_lines>

If you want to delete all lines that are empty or that contain only
whitespace characters (spaces, tabs), use either of:

    :g/^\s*$/d
    :v/\S/d

# Remove trailing or leading whitespace

<http://vim.wikia.com/wiki/Remove_unwanted_spaces>

In a search, \\s finds whitespace (a space or a tab), and \\+ finds one
or more occurrences.

The following command deletes any trailing whitespace at the end of each
line. If no trailing whitespace is found no change occurs, and the e
flag means no error is displayed.

    :%s/\s\+$//e

To delete all trailing whitespace (at the end of each line), you can use
the command:

    :%s/ \+$//

To include tabs, use \\s instead of space.

The following deletes any leading whitespace at the beginning of each
line.

    :%s/^\s\+//e
    " Same thing (:le = :left = left-align given range; % = all lines):
    :%le

# Merge lines, replace/add character at the end of line

<http://stackoverflow.com/questions/1912905/how-do-i-join-two-lines-in-vi>

Append characters at the end of all lines

    %s/$/characters/

    %s/$/characters/g

Insert characters at the begin of all lines

    %s/^/characters/

    %s/^/characters/g

Merge two lines as one:

    "Shift j" will merge two lines

Merge all lines as one:

Just replace the "\\n" with the character I want.

For example a comma:

    :%s/\n/,/g

To confirm every replacment

    :%s/\n/,/gc

Join all lines as on line with space.

Using shift + v + arrow keys to mark lines then shift + j to join lines.

<https://www.cyberciti.biz/faq/vi-vim-editor-search-and-replace-howto/>

Use find and replace on line ranges (match by line numbers)

    :5,20s/foo/bar/

Following command will replace first occurrence of foo with bar starting
at the current line for the next 100 lines:

    :.,+100s/foo/bar/

Match by words

match by words i.e. replace first occurrence of foo with bar starting at
at the next line containing a word “test”:

    :/test/s/foo/bar/g

As usual you can specify ranges:

    :/test/,/guest/s/foo/bar/g

Moving lines up or down

<https://vim.fandom.com/wiki/Moving_lines_up_or_down>

## replace a character by a newline in Vim

<https://stackoverflow.com/questions/71323/how-to-replace-a-character-by-a-newline-in-vim/71334>

**\\n matches an end of line (newline), whereas \\r matches a carriage
return.**

Use \\r

## Multiple commands at once

<https://vim.fandom.com/wiki/Multiple_commands_at_once>

You can execute more than one command by placing a \| between two
commands.

For example:

    %s/htm/html/c | %s/JPEG/jpg/c | %s/GIF/gif/c

This example substitutes for htm, then moves on to JPEG, then GIF.

The second command (and subsequent commands) are only executed **if the
prior command succeeds**.

This works for most commands, but some commands like :argdo or :autocmd
see the '\|' as one of their arguments. This allows commands such as
:argdo, which execute a different Vim command, to execute a series of
commands. See :help :\\bar for the full list of such commands.

Multiple commands on same line

https://stackoverflow.com/questions/3249275/multiple-commands-on-same-line#:\~:text=The%20command%20seperator%20in%20vim%20is%20%7C%20.&text=I've%20always%20used%20%5EJ,%2B%20v%20%2C%20Ctrl%20%2B%20j%20.&text=You%20can%20use%20the%20e,the%20string%20is%20not%20found.

Use the e flag to ignore the error when the string is not found.

    % s/word/newword/ge | % s/word2/newword2/ge

Vim as the poor man's sed

<https://www.brianstorti.com/vim-as-the-poor-mans-sed/>

The ex mode

The ex mode is very similar to the command mode: It allows you to enter
ex commands. The main difference is that you won’t be back to normal
mode after the command is executed. You can enter the ex mode with Q,
and go back to normal mode with :visual.

It’s a mode designed for batch processing, and we can start vim with -e,
if that’s all we need.

### Macro

Vi and Vim Macro Tutorial: How To Record and Play

<https://www.thegeekstuff.com/2009/01/vi-and-vim-macro-tutorial-how-to-record-and-play/>

Start the Recording and store it in register a.

Type: Esc q followed by a

-   q indicates to start the recording
-   a indicates to store the recordings in register a
-   When you do q a, it will display “recording” at the bottom of the
    vi.

Stop the recording

Type: q

Press q to stop the recording. You’ll notice that recording message at
the bottom of the vim is now gone.

Repeat the recording

Type: @a

Repeat 10 times.

Type: 10@a

Recording Macro

<https://learnbyexample.gitbooks.io/vim-reference/content/Recording_Macro.html>

## Display Hidden Characters

How to Display Hidden Characters in vim?

<https://askubuntu.com/questions/74485/how-to-display-hidden-characters-in-vim>

    set list

    set nolist

## join every second line

How to join every second line in Vim?

<https://superuser.com/questions/168942/how-to-join-every-second-line-in-vim>

i would do this:

-   start recording a macro 'q': qqJjq
-   replay the macro 'q' 500 times: 500@q

(actually it is not a macro called 'q', it is a named register called
'q'. instead of interactively fill that register as in 1., you could
also do :let @q = "Jj" and then do 2.)

    :%normal J

    :%norm J

To do this on just a portion of the file, select the lines with V or get
a range some other way:

    :'<,'>global/^/normal J

or, shorter:

    :'<,'>g/^/norm J

# Shell script error

Vim: Warning: Output is not to a terminal

The reason you're getting the error is that when you start a shell in
your environment, it's starting in a subshell that has STDIN and STDOUT
not connected to a TTY — probably because this is in something like a
pipeline. When you redirect, you're opening a new connection directly to
the device. So, for example, your command line turns.

    vi my_file < `tty` > `tty`

# less multiple files

<http://superuser.com/questions/347760/less-command-with-multiple-files-how-to-navigate-to-next-previous>

Next

:n

Previous : this is different from vi (:N)

:p

:e \[file\] Examine a new file while less is still running

# VI / VIM: Open File And Go To Specific Function or Line Number

<http://www.cyberciti.biz/faq/linux-unix-command-open-file-linenumber-function/>

# clear last search highlighting

<http://stackoverflow.com/questions/657447/vim-clear-last-search-highlighting>

    :noh

# Change case

<http://vim.wikia.com/wiki/Switching_case_of_characters>

gUU : change all characters to upper case to the end of line. (Same as
VU)

guu : change all characters to lower case to the end of line. (Same as
Vu)

<http://stackoverflow.com/questions/1102859/how-to-convert-all-text-to-lowercase-in-vim>

ggVGu : change all characters to lower case

ggVGU : change all characters to upper case

Explanation:

to lowercase

-   gg - goes to first line of text
-   V - turns on Visual selection, in line mode
-   G - goes to end of file (at the moment you have whole text selected)
-   u - lowercase selected area

to uppercase

-   gg - goes to first line of text
-   V - turns on Visual selection, in line mode
-   G - goes to end of file (at the moment you have whole text selected)
-   U - Uppercase selected area

# Sort lines

<http://vim.wikia.com/wiki/Sort_lines>

    :{range}sort

You could create a range in advance, such as 'a,. (from mark 'a' to the
current line) or you could create one on-the-fly using visual selection
by pressing ':' in visual mode, after selecting the text you wish to
sort, to get a range of '\<,'> on the command line.

Sort all lines

    :%sort

Sort in reverse

    :%sort!

Sort, removing duplicate linesEdit

    :%sort u

Sort using the external Unix sort utility, respecting month-name
orderEdit

    :%!sort -M

("respecting month-name order" means January \< February \< ... \<
December)

Numeric sort

    :sort n

sort lines 100 to 300, inclusive

    :100,300sort

# Avoiding the Esc key

<http://vim.wikia.com/wiki/Avoid_the_escape_key>

|             |                                                                  |
|-------------|------------------------------------------------------------------|
| **Ctrl-\[** | (control plus left square bracket) is equivalent to pressing Esc |

|                             |                                             |
|-----------------------------|---------------------------------------------|
| **alt+h alt+j alt+k alt+l** | all take it from insert mode to normal mode |

# Copy to and from OS clipboard

Version : **GNOME Terminal 3.28.2**

How to copy text from vim to an external program?

<https://unix.stackexchange.com/questions/12535/how-to-copy-text-from-vim-to-an-external-program>

## Copy text from vim to gnome-text-editor

Make a copy of text first.

    ggVG y

Copy it to the OS clipboard

    "+y

or

    "*y

Past the copied text to a test editor, gnome-text-editor by "middle
mouse click"

Note that "\* and "+ work both ways

**For the reverse action :**

Copied text from a test editor, gnome-text-editor by "control-c"

Copy it to the vim

    "+p

or

    "*p

Alternative easy way : just hit "ctl-shift-v"

## GNOME Terminal - an Easy way

Version : GNOME Terminal 3.28.2

### Copy text from vim to gnome-text-editor

Make a copy of text first.

    ggVG y

Past the copied text to gnome-text-editor by **"middle mouse click"**

    middle mouse click

**For the reverse action :**

Copied text from gnome-text-editor by "control-c"

Copy it to the vim

    ctrl-shift-v

# Visual Mode

<https://opensource.com/article/19/2/getting-started-vim-visual-mode>

-   Character mode: v (lower-case)
-   Line mode: V (upper-case) (Shift + v )
-   Block mode: Ctrl+v

<https://learnbyexample.gitbooks.io/vim-reference/content/Visual_mode.html>

## Selection

|        |                                                                        |                                       |
|--------|------------------------------------------------------------------------|---------------------------------------|
| v      | start visual selection, use any movement command to complete selection | ve, vip, v) etc                       |
| V      | visually select current line                                           | Vgg, ggVG etc                         |
| Ctrl+v | visually select column(s)                                              | Ctrl+v3j2l to select a 4x3 block, etc |

Useful command

|      |                                              |
|------|----------------------------------------------|
| gv   | Reuse a visual mode selection                |
| VG   | Select all lines down to the end from cursor |
| ggVG | Select all lines                             |

## Editing

|     |                                                                                                                                      |
|-----|--------------------------------------------------------------------------------------------------------------------------------------|
| d   | delete the selected text                                                                                                             |
| c   | clear the selected text and change to Insert mode                                                                                    |
|     | for visually selected column, after c type any text and press Esc key. The typed text will be repeated across all lines              |
| I   | after selecting with Ctrl+v, press I, type text and press Esc key to replicate text across all lines to left of the selected column  |
| A   | after selecting with Ctrl+v, press A, type text and press Esc key to replicate text across all lines to right of the selected column |
| ra  | replace every character of the selected text with a                                                                                  |
| :   | perform Command Line mode editing commands like g,s,!,normal on selected text                                                        |
| J   | join selected lines with space character in between                                                                                  |
| gJ  | join selected lines without any character in between                                                                                 |
| :h  | visual-operators for more info                                                                                                       |

## Indenting

set shiftwidth=2

|     |                                                        |
|-----|--------------------------------------------------------|
| \>  | indent the visually selected lines                     |
| 3>  | indent the visually selected lines three times         |
| \<  | unindent the visually selected lines                   |
| =   | indent code, taking care of nesting too. Example below |

|      |                    |
|------|--------------------|
| vip= | to indent the code |

    for(;;)
    {
    for(;;)
    {
    statements
    }
    statements
    }

        vip= to indent the code

    for(;;)
    {
        for(;;)
        {
            statements
        }
        statements
    }

## Changing Case

|     |                                                                                           |
|-----|-------------------------------------------------------------------------------------------|
| \~  | invert the case of visually selected text, i.e lowercase becomes uppercase and vice versa |
| U   | change visually selected text to UPPERCASE                                                |
| u   | change visually selected text to lowercase                                                |

## Visual select all

Select All

ggVG => type "gg" to go at top of the test Then type VG.

Copy All

<http://stackoverflow.com/questions/15610222/how-to-select-all-and-copy-in-vim>

:%y+

Form VIM 7.0

:%y

## Column selection

<http://sethmason.com/2007/09/27/vim-tip-select-column.html>

**Column selection.**

-   ctl-v
-   use arrow keys to move around

**Replacing the selected columns with new characters.**

-   ctl-v
-   c
-   type new characters ( it will show only on the fist line )
-   \<ESC>\<ESC>

Use I (shift-i), A (shift-a) for other situation : referring to below
link

[vim#editing](/ubuntu_cookbook/system/vim#editing)

## Comments issue

<https://www.vim.org/scripts/script.php?script_id=1528>

-   ctrl-c to comment a single line
-   ctrl-x to un-comment a single line
-   shift-v and select multiple lines, then ctrl-c to comment the
    selected multiple lines
-   shift-v and select multiple lines, then ctrl-x to un-comment the
    selected multiple lines

supports: c, c++, java, php\[2345\], proc, css, html, htm, xml, xhtml,
vim, vimrc, sql, sh, ksh, csh, perl, tex, fortran, ml, caml, ocaml,
vhdl, haskel and normal files

    mkdir -p ~/.vim/plugin

    mv comments.vim  ~/.vim/plugin

Or git it directly from the github

    mkdir -p ~/.vim/plugin
    cd ~/.vim/plugin
    wget https://github.com/sudar/comments.vim/blob/master/plugin/comments.vim

<https://github.com/sudar/comments.vim>

You can map your own keys if needed

        let g:comments_map_keys = 0

        " key-mappings for comment line in normal mode
        noremap  <silent> <C-L> :call CommentLine()<CR>
        " key-mappings for range comment lines in visual <Shift-V> mode
        vnoremap <silent> <C-L> :call RangeCommentLine()<CR>

        " key-mappings for un-comment line in normal mode
        noremap  <silent> <C-K> :call UnCommentLine()<CR>
        " key-mappings for range un-comment lines in visual <Shift-V> mode
        vnoremap <silent> <C-K> :call RangeUnCommentLine()<CR>

<https://vimawesome.com/plugin/comments-vim-back-to-december>

Using Vundle

        Bundle "sudar/comments.vim"

        And :BundleInstall

# Marks

Using marks

<https://vim.fandom.com/wiki/Using_marks>

A **mark** allows you to record your current position so you can return
to it later. There is no visible indication of where marks are set.

-   Each file has a set of marks identified by lowercase letters (a-z).
-   Tthere is a global set of marks identified by uppercase letters
    (A-Z)

## Setting marks

To set a mark, **type m followed by a letter**. For example, ma sets
mark a at the current position (line and column). If you set mark a, any
mark in the current file that was previously identified as a is removed.
If you set mark A, any previous mark A (in any file) is removed.

## Using marks

jump to a mark enter an apostrophe (') or backtick (\`) followed by a
letter.

-   Using an apostrophe jumps to the beginning of the line holding the
    mark,
-   Using a backtick jumps to the line and column of the mark.

<!-- -->

-   Each file can have mark a – use a lowercase mark to jump within a
    file.
-   There is only one file mark A – use an uppercase mark to jump
    between files.

| Command   | Description                                                   |
|-----------|---------------------------------------------------------------|
| ma        | set mark a at current cursor location                         |
| 'a        | jump to line of mark a (first non-blank character in line)    |
| \`a       | jump to position (line and column) of mark a                  |
| d'a       | delete from current line to line of mark a                    |
| d\`a      | delete from current cursor position to position of mark a     |
| c'a       | change text from current line to line of mark a               |
| y\`a      | yank text to unnamed buffer from cursor to position of mark a |
| :marks    | list all the current marks                                    |
| :marks aB | list marks a, B                                               |

-   Commands like d'a operate "linewise" and include the start and end
    lines.
-   Commands like d\`a operate "characterwise" and include the start but
    not the end character.

It is possible to navigate between lowercase marks:

| Command | Description                                 |
|---------|---------------------------------------------|
| \]'     | jump to next line with a lowercase mark     |
| \['     | jump to previous line with a lowercase mark |
| \]\`    | jump to next lowercase mark                 |
| \[\`    | jump to previous lowercase mark             |

The above commands take a count. For example, 5\]\` jumps to the fifth
mark after the cursor.

## Special marks

|              |                                                               |
|--------------|---------------------------------------------------------------|
| Command      | Description                                                   |
| \`.          | jump to position where last change occurred in current buffer |
| \`"          | jump to position where last exited current buffer             |
| \`0          | jump to position in last file edited (when exited Vim)        |
| \`1          | like \`0 but the previous file (also \`2 etc)                 |
| ''           | jump back (to line in current buffer where jumped from)       |
| \`\`         | jump back (to position in current buffer where jumped from)   |
| \`\[ or \`\] | jump to beginning/end of previously changed or yanked text    |
| \`\< or \`\> | jump to beginning/end of last visual selection                |

## Deleting marks

If you delete a line containing a mark, the mark is also deleted.

If you wipeout a buffer (command :bw), all marks for the buffer are
deleted.

The :delmarks command (abbreviated as :delm) may be used to delete
specified marks.

| Command        | Description             |
|----------------|-------------------------|
| :delmarks a    | delete mark a           |
| :delmarks a-d  | delete marks a, b, c, d |
| :delmarks abxy | delete marks a, b, x, y |
| :delmarks aA   |                         |

delete marks a, A\|

|            |                                                         |
|------------|---------------------------------------------------------|
| :delmarks! | delete all lowercase marks for the current buffer (a-z) |

# Split Screen

How To Use VIM Split Screen

<https://linuxhint.com/how-to-use-vim-split-screen/>

To open a new VIM window next to the existing one, press \<Ctrl>+\<w>
then press \<v>.

open a new VIM window on the bottom of the currently selected window,
press \<Ctrl>+\<w> then press \<s>

move to the right window from the left by pressing \<Ctrl>+\<w> and then
pressing \<l>

move to the left window again by pressing \<Crtl>+\<w> and then pressing
\<h>

go to the window below the selected window by pressing \<Ctrl>+\<w> and
then pressing \<j>

go to the window above the selected window by pressing \<Ctrl>+\<w> and
then pressing \<k>

Copy and Paste Texts Between VIM Split Screens

To do that, from the “Command Mode”, first go to the location from where
you want to start your selection and press \<v> to go to the “Visual
Mode” of VIM and select the substring and press \<y>. The text should be
copied.

Now go to another window by pressing \<Ctrl>+\<w> and then any of \<h>
\<j> \<k> or \<l> depending on your choice. Now go to “Insert Mode” by
pressing ‘i’ and navigate to the place where you want to paste the text.
Then go back to “Command Mode” by pressing \<ESC> and press \<p> to
paste the copied text.

Change the Split Screen Window Size of VIM There are several shortcuts
to change the split screen window size of VIM. You can increase the
width of your window by pressing \<Ctrl>+\<w> and then ‘>’ and decrease
the width by pressing \<Ctrl>+\<w> and then ‘\<’.

You can also increase or decrease the height of your VIM window. To
increase the height of your window, press \<Ctrl>+\<

# How to keep inode

why inode value changes when we edit in “vi” editor?

<https://unix.stackexchange.com/questions/36467/why-inode-value-changes-when-we-edit-in-vi-editor>

By default, Vim renames the old file, then writes a new file with the
original name, if it thinks it can re-create the original file's
attributes. If you want to reuse the existing inode (and so risk losing
data, or waste more time making a backup copy), **add set backupcopy yes
to your .vimrc**.

    set backupcopy=yes

inode does not changed example

    $ ll -in

    1458711 -rw-r--r-- 1 1001 1001       11 Nov 26 15:08 abc

    $ vi abc

    # set backupcopy=yes  (in the vim )

    $ ll -in

    1458711 -rw-r--r-- 1 1001 1001       75 Nov 26 15:09 abc

w> and then press ‘+’ and to decrease the width press \<Ctrl>+\<w> and
then press ‘-’.

# Keyboard Shortcuts

<https://gist.github.com/awidegreen/3854277>

|     |                           |
|-----|---------------------------|
| x   | Delete char UNDER cursor  |
| X   | Delete char BEFORE cursor |

Basic vi Commands

<https://www.cs.colostate.edu/helpdocs/vi.html>

|     |                                                                             |
|-----|-----------------------------------------------------------------------------|
| r   | replace single character under cursor (no \<Esc> needed)                    |
| R   | replace characters, starting with current cursor position, until \<Esc> hit |
| C   | change (replace) the characters in the current line, until \<Esc> hit       |
| cc  | change (replace) the entire current line, stopping when \<Esc> is hit       |

## Delete shortcuts

<https://stackoverflow.com/questions/1373841/vim-deleting-backward-tricks>

# Vi usage guide

<http://www.radford.edu/~mhtay/CPSC120/VIM_Editor_Commands.htm>

<http://staff.washington.edu/rells/R110/help_vi.html>

      ---  Moving the Cursor  ---

      h            left one space
      j            down one line
      k            up one line
      l            right one space

      ---  Deleting Lines  ---

      To delete a whole line, type

        dd

      The cursor does not have to be at the beginning of the line.
      Typing dd deletes the entire line containing the cursor and
      places the cursor at the start of the next line.  To delete
      two lines, type

        2dd

      To delete from the cursor position to the end of the line,
      type

        D (uppercase)

      ---  Opening a Blank Line  ---

      To insert a blank line below the current line, type

        o (lowercase)

        shift a + enter (was what I used the most)

      To insert a blank line above the current line, type

        O (uppercase)


      ---  Joining Lines  ---

      To join two lines together:

      1. Put the cursor on the first line to be joined.
      2. Type J

      To join three lines together:

      1. Put the cursor on the first line to be joined.
      2. Type 3J

-   r - to replace the letter e.g press re to replace the letter with e
-   ce - to change until the end of a word
-   x - to delete the unwanted character
-   u - to undo the last the command and U to undo the whole line
-   CTRL-R to redo
-   2w - to move the cursor two words forward.
-   0 -(zero) to move to the start of the line.
-   G - to move you to the bottom of the file.
-   gg - to move you to the start of the file.
-   Type the number of the line you were on and then G
-   % to find a matching ),\], or }
-   e - command moves to the end of a word.
-   R - enters Replace mode until \<ESC> is pressed.

## Editing

-   i - insert mode at the cursor.
-   shift + i - insert mode at the begin of line.
-   shift + a - insert mode at the end of line.
-   o - a new line below current cursor line.
-   ctrl + f - page down
-   ctrl + b - page up
-   shift + j - join below line

# Scroll

<http://vimdoc.sourceforge.net/htmldoc/scroll.html>

Some commands are not included in previous section so they are records
here.

-   Crontl-E :Scroll up one line on the screen
-   Crontl-Y :Scroll down one line on the screen
-   Crontl-U :Up half screenful
-   Crontl-D :Down half screenful

# Matching Parenthesises

<http://www.computerhope.com/unix/vim.htm>

%

# makrs

<http://vim.wikia.com/wiki/Using_marks>

Below 'a' refers to '\[a-z\]' in lower case.

| Command Description |                                                               |
|---------------------|---------------------------------------------------------------|
| ma                  | set mark a at current cursor location                         |
| 'a                  | jump to line of mark a (first non-blank character in line)    |
| \`a                 | jump to position (line and column) of mark a                  |
| d'a                 | delete from current line to line of mark a                    |
| d\`a                | delete from current cursor position to position of mark a     |
| c'a                 | change text from current line to line of mark a               |
| y\`a                | yank text to unnamed buffer from cursor to position of mark a |
| :marks              | list all the current marks                                    |
| :marks aB           | list marks a, B                                               |

# How to stop line breaking in vim

<http://stackoverflow.com/questions/2280030/how-to-stop-line-breaking-in-vim>

Enables "visual" wrapping

:set wrap

turns off physical line wrapping (ie: automatic insertion of newlines)

:set textwidth=0 wrapmargin=0

# vimdiff

<http://vimdoc.sourceforge.net/htmldoc/diff.html>

      vimdiff file1 file2 [file3 [file4]]

This is equivalent to:

      vim -d file1 file2 [file3 [file4]]

# Replace

<http://stackoverflow.com/questions/6961705/vim-delete-the-first-2-spaces-for-multiple-lines>

Remove first two characters no matter what:

    %s/^.\{2}//

Remove first two white space characters

    %s/^\s\{2}//

# color

Turn On or Off Color Syntax Highlighting In vi or vim

<https://www.cyberciti.biz/faq/turn-on-or-off-color-syntax-highlighting-in-vi-or-vim/>

    :syntax on

    :syntax off

## root account can't see color

OS: RHEL 5.x

root account is using /bin/vi instead of vim.

Using vim will be able to see color.

Or set up an alias to it.

    alias vi='vim'

# How to view variables in Vim

<https://codeyarns.com/2010/11/26/how-to-view-variables-in-vim/>

    :let

# Indent

<http://vim.wikia.com/wiki/Indenting_source_code>

<http://vim.wikia.com/wiki/Shifting_blocks_visually>

<http://stackoverflow.com/questions/235839/indent-multiple-lines-quickly-in-vi>

|        |        |                                                                                                                                                                                         |
|--------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \>\>   | \<\<   | to indent a line                                                                                                                                                                        |
| n>\>   | n\<\<  | to indent n lines. 5>\>                                                                                                                                                                 |
| .      |        | Repeat last command. 5>\>..                                                                                                                                                             |
| Ctrl-T | Ctrl-D | In insert mode, Ctrl-T indents the current line, and Ctrl-D unindents.                                                                                                                  |
| \>%    | \<%    | to indent breaks when the cursor is on one of the break. (){}\[\]                                                                                                                       |
| V      |        | Visual mark lines with (j,K,up,down or the mouse). Vjj>.                                                                                                                                |
| gv     |        | type gv to reselect the above lines which were marked by V                                                                                                                              |
| \]p    |        | If you’re copying blocks of text around and need to align the indent of a block in its new location, use \]p instead of just p. This aligns the pasted block with the surrounding text. |
| :le 12 |        | to set the indent to 12 spaces (abbreviation for :left 12).                                                                                                                             |
| :le    |        | to remove all indents in a selected region                                                                                                                                              |
| =G     |        | To indent the all the lines below the current line                                                                                                                                      |
| n==    |        | To indent n lines below the current line                                                                                                                                                |

## Spaces

|                   |                                                                  |
|-------------------|------------------------------------------------------------------|
| set shiftwidth=2  | Indent by 2 spaces when using \>\>, \<\<, == etc.                |
| set softtabstop=2 | Indent by 2 spaces when pressing \<TAB>                          |
| set expandtab     | Use softtabstop spaces instead of tab characters for indentation |
| set autoindent    | Keep indentation from previous line                              |
| set smartindent   | Automatically inserts indentation in some cases                  |
| set cindent       | Like smartindent, but stricter and more customisable             |

# Folding

<http://vim.wikia.com/wiki/Folding>

<http://220-133-241-31.hinet-ip.hinet.net/vim/node12.html>

zc and zo to folding

zfap : fold current block

zf7G : fold from cursor to line 7

:5,23fold : fold from line 5 to 23

5zF: folder 5 lines

Marked by Shift+v first then using zf to folder it.

# Cut/copy and paste using visual selection

<http://vim.wikia.com/wiki/Cut/copy_and_paste_using_visual_selection>

To cut-and-paste or copy-and-paste:

-   Position the cursor at the beginning of the text you want to
    cut/copy.
-   Press v to begin character-based visual selection, or V to select
    whole lines, or Ctrl-v or Ctrl-q to select a block.
-   Move the cursor to the end of the text to be cut/copied. While
    selecting text, you can perform searches and other advanced
    movement.
-   Press d (delete) to cut, or y (yank) to copy.
-   Move the cursor to the desired paste location.
-   Press p to paste after the cursor, or P to paste before.

# Save to and Read from

|                            |                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------|
| :w\<Return>                | write current contents to file named in original vi call                             |
| :w newfile\<Return>        | write current contents to a new file named newfile                                   |
| :12,35w smallfile\<Return> | write the contents of the lines numbered 12 through 35 to a new file named smallfile |
| :w! prevfile\<Return>      | write current contents over a pre-existing file named prevfile                       |
| :r filename\<Return>       | read file named filename and insert after current line                               |
| :pwd                       | check current directory                                                              |
| :cd                        | change current directory                                                             |
| :sh                        | launch a shell                                                                       |
| :E                         | opens the current directory in Vim                                                   |
| ^o                         | to fallback previous opened file,                                                    |

Using :E to check the files in current directory then type ctl-o to
fallback previous opened file

# Searching

<http://vim.wikia.com/wiki/Searching>

<http://vim.wikia.com/wiki/Copy_or_change_search_hit>

# Searching and Replacing

<http://vim.wikia.com/wiki/Search_and_replace>

## Search range:

-   :s/foo/bar/g Change each 'foo' to 'bar' in the current line.
-   :%s/foo/bar/g Change each 'foo' to 'bar' in all the lines.
-   :5,12s/foo/bar/g Change each 'foo' to 'bar' for all lines from line
    5 to line 12 (inclusive).
-   :'a,'bs/foo/bar/g Change each 'foo' to 'bar' for all lines from mark
    a to mark b inclusive (see Note below).
-   :'\<,'>s/foo/bar/g When compiled with +visual, change each 'foo' to
    'bar' for all lines within a visual selection. Vim automatically
    appends the visual selection range ('\<,'>) for any ex command when
    you select an area and enter :. Also, see Note below.
-   :.,$s/foo/bar/g Change each 'foo' to 'bar' for all lines from the
    current line (.) to the last line ($) inclusive.
-   :.,+2s/foo/bar/g Change each 'foo' to 'bar' for the current line (.)
    and the two next lines (+2).
-   :g/^baz/s/foo/bar/g Change each 'foo' to 'bar' in each line starting
    with 'baz'.

Note: As of Vim 7.3, substitutions applied to a range defined by marks
or a visual selection (which uses a special type of marks '\< and '>)
are not bounded by the column position of the marks by default. Instead,
Vim applies the substitution to the entire line on which each mark
appears unless the \\%V atom is used in the pattern like:
:'\<,'>s/\\%Vfoo/bar/g.

## When searching:

-   ., \*, \\, \[, ^, and $ are metacharacters.
-   +, ?, \|, &, {, (, and ) must be escaped to use their special
    function.
-   \\/ is / (use backslash + forward slash to search for forward slash)
-   \\t is tab, \\s is whitespace (space or tab)
-   \\n is newline, \\r is CR (carriage return = Ctrl-M = ^M)
-   After an opening \[, everything until the next closing \] specifies
    a /collection. Character ranges can be represented with a -; for
    example a letter a, b, c, or the number 1 can be matched with
    \[1a-c\]. Negate the collection with \[^ instead of \[; for example
    \[^1a-c\] matches any character except a, b, c, or 1.
-   \\{#\\} is used for repetition. /foo.\\{2\\} will match foo and the
    two following characters. The \\ is not required on the closing } so
    /foo.\\{2} will do the same thing.
-   \\(foo\\) makes a backreference to foo. Parenthesis without escapes
    are literally matched. Here the \\ is required for the closing \\).

## When replacing:

-   \\r is newline, \\n is a null byte (0x00).
-   \\& is ampersand (& is the text that matches the search pattern).
-   \\0 inserts the text matched by the entire pattern
-   \\1 inserts the text of the first backreference. \\2 inserts the
    second backreference, and so on.
-   You can use other delimiters with substitute:

:s#http://www.example.com/index.html#http://example.com/#

Save typing by using \\zs and \\ze to set the start and end of a
pattern. For example, instead of:

:s/Copyright 2007 All Rights Reserved/Copyright 2008 All Rights
Reserved/ Use:

:s/Copyright \\zs2007\\ze All Rights Reserved/2008/

### \\0 represent the entire pattern

\\0 inserts the text matched by the entire pattern

    %s/^.*abc/\0\0\0/

## Additional examples

|                                  |                                                                                                                                                                                                                         |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| :%s/foo/bar/                     | On each line, replace the first occurrence of "foo" with "bar".                                                                                                                                                         |
| :%s/.\*\\zsfoo/bar/              | On each line, replace the last occurrence of "foo" with "bar".                                                                                                                                                          |
| :%s/\\\<foo\\\>//g               | On each line, delete all occurrences of the whole word "foo".                                                                                                                                                           |
| :%s/\\\<foo\\\>.\*//             | On each line, delete the whole word "foo" and all following text (to end of line).                                                                                                                                      |
| :%s/\\\<foo\\\>.\\{5}//          | On each line, delete the first occurrence of the whole word "foo" and the following five characters.                                                                                                                    |
| :%s/\\\<foo\\\>\\zs.\*//         | On each line, delete all text following the whole word "foo" (to end of line).                                                                                                                                          |
| :%s/.\*\\\<foo\\\>//             | On each line, delete the whole word "foo" and all preceding text (from beginning of line).                                                                                                                              |
| :%s/.\*\\ze\\\<foo\\\>//         | On each line, delete all the text preceding the whole word "foo" (from beginning of line).                                                                                                                              |
| :%s/.\*\\(\\\<foo\\\>\\).\*/\\1/ | On each line, delete all the text preceding and following the whole word "foo".                                                                                                                                         |
| :%s/\\\<foo\\(bar\\)\\@!/toto/g  | On each line, replace each occurrence of "foo" (which starts a word and is not followed by "bar") by "toto".                                                                                                            |
| :s/^\\(\\w\\)/\\u\\1/            | If the first character at the beginning of the current line only is lowercase, switch it to uppercase using \\u (see switching case of characters).                                                                     |
| :%s/\\(.\*\\n\\)\\{5\\}/&\\r/    | Insert a blank line every 5 lines. The pattern searches for \\(.\*\\n\\) (any line including its line ending) repeated five times (\\{5\\}). The replacement is & (the text that was found), followed by \\r (newline). |

|                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| :%s/\\\<foo\\(\\a\*\\)\\\>/\\=len(add(list, submatch(1)))?submatch(0):submatch(0)/g                                                                                                                                               |
| Get a list of search results. (the list must exist) Sets the modified flag, because of the replacement, but the content is unchanged. Note: With a recent enough Vim (version 7.3.627 or higher), you can simplify this to below. |
| :%s/\\\<foo\\(\\a\*\\)\\\>/\\=add(list, submatch(1))/gn                                                                                                                                                                           |
| This has the advantage, that the buffer won't be marked modified and no extra undo state is created. The expression in the replacement part is executed in the sandbox and not allowed to modify the buffer.                      |

# Help

    :help \1

# Plugin

How to install Vim plugins

<https://opensource.com/article/20/2/how-install-vim-plugins>

# Vundle

<http://manhnt.github.io/vim/2016/06/25/vundle-beginner.html>

<https://blog.gtwang.org/linux/vundle-vim-bundle-plugin-manager/>

Vundle is a Vim plugin that, well, manages other plugins (and even
itself).

Normally, plugins and additional configuration files are conventionally
stored at **\~/.vim** directory. Inside, most plugins are splitted into
subdirectories based on the functionality that they provide.

What Vundle does is it creating a top directory, named bundle, and
inside that, it creates a separate directory tree for each plugin.

Vundle adds an interface in Vim for user to manage the plugins easily
with some simple commands.

<http://manhnt.github.io/vim/2016/06/25/vundle-beginner.html>

    sudo apt install git

    git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

Configure Vundle

    vi ~/.vimrc

Adding below code on the top.

    set nocompatible              " be iMproved, required
    filetype off                  " required

    " set the runtime path to include Vundle and initialize
    set rtp+=~/.vim/bundle/Vundle.vim
    call vundle#begin()
    " alternatively, pass a path where Vundle should install plugins
    "call vundle#begin('~/some/path/here')

    " let Vundle manage Vundle, required
    Plugin 'VundleVim/Vundle.vim'

    Plugin 'sudar/comments.vim'

    " The following are examples of different formats supported.
    " Keep Plugin commands between vundle#begin/end.
    " plugin on GitHub repo
    "" Plugin 'tpope/vim-fugitive'
    " plugin from http://vim-scripts.org/vim/scripts.html
    " Plugin 'L9'
    " Git plugin not hosted on GitHub
    "" Plugin 'git://git.wincent.com/command-t.git'
    " git repos on your local machine (i.e. when working on your own plugin)
    "" Plugin 'file:///home/gmarik/path/to/plugin'
    " The sparkup vim script is in a subdirectory of this repo called vim.
    " Pass the path to set the runtimepath properly.
    "" Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
    " Install L9 and avoid a Naming conflict if you've already installed a
    " different version somewhere else.
    " Plugin 'ascenator/L9', {'name': 'newL9'}

    " All of your Plugins must be added before the following line
    call vundle#end()            " required
    filetype plugin indent on    " required
    " To ignore plugin indent changes, instead use:
    "filetype plugin on
    "
    " Brief help
    " :PluginList       - lists configured plugins
    " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
    " :PluginSearch foo - searches for foo; append `!` to refresh local cache
    " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
    "
    " see :h vundle for more details or wiki for FAQ
    " Put your non-Plugin stuff after this line

## Vundle tutorial

<https://linuxhint.com/vim-vundle-tutorial/>

## Install plugins with Vundle

:PluginInstall

Open vim and run command :PluginInstall. Vundle will start processing
and installing the plugins one by one.

To install from command line: vim +PluginInstall +qall

    vim +PluginInstall +qall

## Update plugins

If you wish to update the plugins to their latest version, just run this
command in vim:

:PluginUpdate

or below command will reinstall all of the plugins.

:PluginInstall!

## List installed plugins

:PluginList

## Python Plugins

<https://blog.gtwang.org/linux/vundle-vim-bundle-plugin-manager/>

# VIM-PENCIL

<https://vimawesome.com/plugin/vim-pencil>

# VIM Markdown

<https://github.com/plasticboy/vim-markdown>

<http://vim.wikia.com/wiki/Quick_Markdown_Preview>

<https://5xruby.tw/ja/posts/markdown-extension-on-vim>

Using Vundle : Adding below code to \~/.vimrc

    vi ~/.vimrc

    Plugin 'godlygeek/tabular'

    Plugin 'plasticboy/vim-markdown'

Install above plugins.

Launch vim them submit below command then type q to leave the installing
result.

:PluginInstall

Checking the installed plugins.

:PluginList

Completed. It works on the file name extension of .md well.

When the file name extension is not .md, use below commnad to highlight
makrdown syntax.

    :set syntax=markdown

Following command let vim to treat current editing file is a markdown
file.

    :set filetype=markdown

## Change default file extension

If you would like to use a file extension other than .md you may do so
using the vim_markdown_auto_extension_ext variable:

    let g:vim_markdown_auto_extension_ext = 'txt'

## Shortcuts

-   ge: open the link under the cursor in Vim for editing. Useful for
    relative markdown links. \<Plug>Markdown_EditUrlUnderCursor The
    rules for the cursor position are the same as the gx command.
-   \]\]: go to next header. \<Plug>Markdown_MoveToNextHeader
-   \[\[: go to previous header. Contrast with \]c.
    \<Plug>Markdown_MoveToPreviousHeader
-   \]\[: go to next sibling header if any.
    \<Plug>Markdown_MoveToNextSiblingHeader
-   \[\]: go to previous sibling header if any.
    \<Plug>Markdown_MoveToPreviousSiblingHeader
-   \]c: go to Current header. \<Plug>Markdown_MoveToCurHeader
-   \]u: go to parent header (Up). \<Plug>Markdown_MoveToParentHeader

## Commands

-   :Toc: create a quickfix vertical window navigable table of contents
    with the headers. Hit \<Enter> on a line to jump to the
    corresponding line of the markdown file.
-   :Toch: Same as :Toc but in an horizontal window.
-   :Toct: Same as :Toc but in a new tab.
-   :Tocv: Same as :Toc for symmetry with :Toch and :Tocv.

### switch between panes in split mode in Vim

<https://www.quora.com/How-do-I-switch-between-panes-in-split-mode-in-Vim>

In command mode, hit Ctrl-W and then a direction, or just Ctrl-W again
to switch between panes.

-   Ctrl-W and then a direction
-   Ctrl-W Ctrl-W

## Set header folding level

Folding level is a number between 1 and 6. By default, if not specified,
it is set to 1.

    let g:vim_markdown_folding_level = 6

Tip: it can be changed on the fly with:

    :let g:vim_markdown_folding_level = 1
    :edit

    :let g:vim_markdown_folding_level = 6
    :edit

<https://vi.stackexchange.com/questions/9543/how-to-fold-markdown-using-the-built-in-markdown-mode>

Folding and un-folding with below command:

    zc

    zo or space key

For more info see :h zo and :h foldcolumn

Show some visual context even when it's not folded try the foldcolumn
option!

    set foldcolumn=2

## Notes

Better Writing Markdown Experience on Vim

<https://www.lequochung.me/2016/11/11/better-markdown-writing-experience-on-vim.html>

## Vim-instant-markdown

<https://5xruby.tw/ja/posts/markdown-extension-on-vim>

<https://linjc.github.io/2017/02/22/vim-markdown-plugin/>

Installing above "vim-markdown" plugin first.

Install nodejs

<https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-16-04>

    sudo apt-get update
    sudo apt-get install nodejs
    sudo apt-get install npm

Installing instant-markdown-d

    sudo npm -g install instant-markdown-d

vi .vimrc

Adding below line at the end

    Plugin 'suan/vim-instant-markdown'

Install above plugin from current user.

    vim

    :PluginInstall

Once it is completed, it will lunch a browser when modifying a markdown
file.

## How to make Vim understand that files contain Markdown code

<https://stackoverflow.com/questions/23279292/how-to-make-vim-understand-that-md-files-contain-markdown-code-and-not-modula>

:verbose set filetype?

It is be set in below configuration file.

/usr/share/vim/vim74/filetype.vim

## Quickly adding and deleting empty lines

<http://vim.wikia.com/wiki/Quickly_adding_and_deleting_empty_lines>

Starting in normal mode, you can press O to insert a blank line before
the current line, or o to insert one after. O and o ("open") also switch
to insert mode so you can start typing.

To add 10 empty lines below the cursor in normal mode, type 10o\<Esc> or
to add them above the cursor type 10O\<Esc>.

Here is an alternative method that allows you to easily insert or delete
blank lines above or below the current line. Your cursor position is not
changed, and you stay in normal mode.

Put the following mappings in your vimrc:

    " Ctrl-j/k deletes blank line below/above, and Alt-j/k inserts.
    nnoremap <silent><C-j> m`:silent +g/\m^\s*$/d<CR>``:noh<CR>
    nnoremap <silent><C-k> m`:silent -g/\m^\s*$/d<CR>``:noh<CR>
    nnoremap <silent><A-j> :set paste<CR>m`o<Esc>``:set nopaste<CR>
    nnoremap <silent><A-k> :set paste<CR>m`O<Esc>``:set nopaste<CR>

In normal mode, the mappings perform these operations without moving the
cursor:

|        |                                                          |
|--------|----------------------------------------------------------|
| Ctrl-j | deletes the line below the current line, if it is blank. |
| Ctrl-k | deletes the line above the current line, if it is blank. |
| Alt-j  | inserts a blank line below the current line.             |
| Alt-k  | inserts a blank line above the current line.             |

# Tips

## Copy all to OS clipboard

How can I copy text to the system clipboard from Vim?

<http://vim.wikia.com/wiki/Cut/copy_and_paste_using_visual_selection>

<https://vi.stackexchange.com/questions/84/how-can-i-copy-text-to-the-system-clipboard-from-vim>

|                   |      |
|-------------------|------|
| Copy to clipboard | "\*y |

The double-qoute and start trick work well with visual mode as well. ex:
visual select text to copy to clippboard and then type "\*y

Copy all text steps are as below.

-   gg
-   shift-v
-   shift-g
-   "\*y

### No +clipboard?

Vim requires the +clipboard feature flag for any of this to work; you
can check if your Vim has this by using :echo has('clipboard') from
within Vim (if the output is 0, it not present, if it's 1, it is), or
checking the output of vim --version.

    :echo has('clipboard')

Solution:

For Ubuntu 18.04.1 64 bit desktop, install vim-gnome will do.

    sudo apt install vim-gnome

### SSH

You can also **use a clipboard on remote machines** if you enable X11
forwarding over SSH. This is especially useful with the above tip since
you can then use xclip to access your desktop's clipboard. The Vim on
the machine you're ssh-ing to will still need the +clipboard feature.

This requires the ForwardX11Trusted setting, and should only be done
with trusted servers, as this gives the server almost complete control
over your X11 session:

    ssh -XY myhost

To make these settings persistent (so you don't need to add -XY every
time), you could do something like this in your \~/.ssh/config:

    vi ~/.ssh/config


    # Do **NOT** set this globally; it gives the server complete control over
    # your X11 session.
    Host myhost
        ForwardX11 yes
        ForwardX11Trusted yes

## VIM error

symbol lookup error: /usr/lib/x86_64-linux-gnu/libpython3.6m.so.1.0:
undefined symbol: XML_SetHashSalt

    $ vim
    vim: symbol lookup error: /usr/lib/x86_64-linux-gnu/libpython3.6m.so.1.0: undefined symbol: XML_SetHashSalt

<https://github.com/sqlmapproject/sqlmap/issues/2194>

Remove the value of LD_LIBRARY_PATH

It works.

    $ env | grep LD
    LD_LIBRARY_PATH=/opt/oracle/product/12.2.0/lib:/lib:/usr/lib
    $ LD_LIBRARY_PATH=
    $ vim

## Open file under cursor

<https://vim.fandom.com/wiki/Open_file_under_cursor>

Go to fileEdit

The following commands open the file name under the cursor:

-   gf open in the same window ("goto file")
-   \<c-w>f open in a new window (Ctrl-w f)
-   \<c-w>gf open in a new tab (Ctrl-w gf)

Names containing spaces

Adjusting isfnameEdit To have a space (ASCII 32) considered as a valid
character for a file name, add the following to your vimrc:

    :set isfname+=32

## Read Only mode

<https://www.cyberciti.biz/faq/howto-open-file-tab-in-vim-in-readonly-on-linuxunix/>

    vim -R filename

    ls -1 | vim - -R

## Command output to vim

How do I redirect command output to vim in bash?

<https://askubuntu.com/questions/510890/how-do-i-redirect-command-output-to-vim-in-bash>

use vim's function to read from STDIN

    ls -1 | vim -

    vim <(ls -la)

In VIM

    :r !ls -la

## Viewports

Vim Tips: Using Viewports

<https://www.linux.com/tutorials/vim-tips-using-viewports/>

-   Ctrl-w Ctrl-w moves between Vim viewports.

# Command line Mode

<https://learnbyexample.gitbooks.io/vim-reference/content/Command_Line_mode.html>
