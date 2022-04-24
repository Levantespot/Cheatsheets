# Vim tutorial

[TOC]

## Get help

+ `:help` read help document
+ `:help COMMAND` get help for COMMAND
+ `^W^W` switch from one window to another
+ `:q` quit help
+ `^D` or `Tab` see possible completions

## Mode

- **Normal** mode - used for editor commands. This is also the default mode, unless the `insertmode` option is specified.
- **Visual** mode - similar to normal mode, but used to highlight areas of text. Normal commands are run on the highlighted area, which for an instance can be used to move or edit a selection.
- **Select** mode - works similarly to visual mode. However, if a printable character, carriage return, or newline (or line feed) is entered, Vim inserts the character, and starts insert mode.
- **Insert** mode - similar to editing in most modern editors. In insert mode, buffers can be modified with the text inserted.
- **Command-line** or **Cmdline** mode - supports a single line input at the bottom of the Vim window. Normal commands (beginning with `:`), and some other specific letters corresponding to different actions (including pattern search and the filter command) activate this mode.
- **Ex** mode - similarly to Cmdline mode, it takes a single line input at the bottom of the window. However, in Cmdline mode, entering a command exits the mode when the command is executed. Entering a command in Ex mode doesn't cause the mode to change.

## Normal mode

### Navigation

+ `h` or `←` *cursor* left.
+ `j` or `↓` *cursor* down.
+ `k` or `↑` *cursor* up.
+ `l` or `→` *cursor* right.
+ `w` move until the start of the next *word*.
+ `e` move to the end of the current *word*.
+ `b` move to the start of the last *word*. 
+ `0` (zero) move to the start of the *line*.
+ `$` move to the end of the *line*.
+ `^F` or `Page Down` move to the next *page*.
+ `^B` or `Page Up` move to the last *page*.
+ `G` move to the bottom of the *file*.
+ `gg` move to the start of the *file*.
+ `<N>G` move to the N-th *line* of the file
+ `<N>Enter` move down N *lines*. 
+ `^G` show your location in the file and the file status.
+ `%` jump between paired *parentheses* () or {} or []. 

### Grammar:

+ `[N]`+`MOTION` do n times MOTION, like `3w` go forward 3 words , `10j` go down 10 lines.
+ `OPERATOR`+`[N]`+`MOTION`
+ `[N]` + `OPERATOR`

### Editing action

+ `d + [N] + MOTION` delete something.
+ `dd` delete a whole line. **Remember** to use Vim grammar, this is equal to `dj`, `dk`, `d1Enter`, `d$` and so on.
+ `x` delete a character.
+ `u` lowercase u, undo previous action.
+ `U` capital U, undo all the changes on a line.
+ `^R` undo the undo's.
+ `p` put back text that has just been deleted below the current cursor.
+ `r + <character>` replace the character at the cursor with `<character>`.
+ `c + [N] + MOTION` change something. Equal to delete something and enter Insert mode.
+ `y + MOTION` yank (copy) something.
+ `yy` yank the line where cursor is on, equal to `y$`.
+ `p` paste below the cursor.
+ `P` paste above the cursor.
+ `.` repeat the last action you have just done.

### Search and Substitute

+ `/ or ? + PHRASE` search for the `PHRASE`, `/` will searches FORWARD for the phrase, while `?` BACKWARD for the phrase.
  + `n` search the next one below.
  + `N` search the next one above.
+ `[#1,#2]|[%]:s/old/new[/param]` substitute (replace multiple characters, words and lines) new for the old
  + `[#1,#2]` stands for substitute phrases between two line #'s
  + `%` stands for substitute all in this file.
  + `/g` substitute all in a line
  + `/c` ask for confirmation each time

### Advance action

+ `:!+COMMAND` execute an external COMMAND
+ 

### Save and Quit

+ `:w` save
+ `:q` quit (no changes have been made to the file)
+ `:q!` or `ZQ` quit without saving (discard changes)
+ `:wq` or `ZZ` save and quit
+ `:[#1,#2]w FILENAME` writes the current contents to disk with name FILENAME.
+ `:r FILENAME` retrieves disk file FILENAME and puts it below the cursor position.
+ `v motion` saves the Visually selected lines in file FILENAME.

## Insert mode 

- `i` - insert text before cursor
- `I` - insert text before first *non-blank* character in same line
- `a` - insert text after cursor
- `A` - insert text at the end of line
- `o` - Start a new line below cursor
- `O` - start new line above cursor

## Visual mode

- `v` individual Visual mode
- `C-v` block-wise Visual mode
- `V` line-wise Visual mode