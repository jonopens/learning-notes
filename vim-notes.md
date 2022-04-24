# Vim Notes

h - left
j - down
k - up
l - right

:q! - quit and discard changes
:wq - quit and save

i - enter INSERT mode (add text in the location of the cursor)
a - enter INSERT mode (add text in the location after the cursor highlight)
ESC - exit INSERT mode/return to NORMAL mode

VERB NUMBER MOTION

x - remove the character hovered over
u - UNDO last command
U - UNDO all changes in a line
p - PUT previously deleted text
r - REPLACE the highlighted character with following character
c - CHANGE the word (delete according to motion char and enter insert mode)
  ce - delete to end of current word and insert
  cc - delete to end of line and insert
CTRL-R - redo undone changes
d - delete
  dw - delete single word to start of next word
  de - delete single word to end of current word
  d$ - delete to end of line
  dd - delete entire line
  d2w - delete two words
  d3d - delete three lines
0 - go to start of current line

CTRL-G - get file status and location
G - move to bottom of file
  499G - go to line 499 of current file
gg - move to start of file
/ - search for a word
  n - go to next instance of search word
  N - go to previous instance of search word
CTRL-O - go back
CTRL-I - go forward
% - go to matching pair of (), [] or {}
:s/old/new - SUBSTITUTE new for old in first occurrence on this line
:#,#s/old/new/g - SUBSTITUTE where # are start and end line numbers
:%s/old/new/g - SUBSTITUTE for the whole file
:%s/old/new/gc - SUBSTITUTE for the whole file and prompt to confirm
:! - run external command like `ls` or that sort of thing
:w - save changes
v - enter VISUAL selection and then operate on it by entering `:` and then a verb command
:r FILENAME - insert contents of FILENAME
:r !ls - insert the output of a command (`ls` in this case)
y - copies text (v to select specific text, y to copy, p to paste)