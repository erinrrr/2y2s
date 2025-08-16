- `i`: to start edit mode
	- + shift: input mode at the beginning of the sentence
- `a`: to start edit mode from the next char
- `A`: start edit from the end of the line
- `esc`: to go back to view mode
- `o`: to start edit in a newline under your current
	- + shift (O): newline before the current

- `dd`: delete current line
- `cc`: delete current line and goes into edit mode
- `s`: replace selected and goes into edit mode

- `r`: replace current char
- `v`: to select current char
	- combo with arrow keys
	- + 0: select from current to beginning of the line

- `gU`: change to uppercase
- `gu`: change to lowercase

- `y`: copy (yank) text
	- `2yy`: to copy 2 lines, (it works with any number)
- `p`: paste
- `>>`: indent
- `<<`: de-indent

- `/text`: search text in the note
	- `n`: next
	- `N`: previous
	- `\`: for special characters
- `:%s/text/newtext`: replace text with newtext (all the occurence)
	- +`/gc`(global, confirm): ask for every occurence if we want to replace
		- `y` yes
		- `n` no
		- `a` all
		- `q` quit 

#### Navigation
- `h`: left
	- + shift: all the way up
- `j`: down
- `k`: up
- `l`: right
	- + shift: all the way down
- shift + m: middle screen
<br>
- `w`: jump forward to the first char of the next word
- `e`: jump forward to the end of the word
- `b`: jump backward to first char
- `shift + 4`: jump to the end of line
- `0`: jump to the beginning of the line
<br>
- `gg`: first line of the document
- `G`: last line of the document
- `{`: jump previous paragraph
- `}`: jump to next paragraph
<br>
- `u`: undo
- `ctrl + r`: redo

### Edit mode

- `ctrl + a`: increase a number
- `ctrl + x`: decrease a number

> credits:
>    - [cheat sheet](https://vim.rtorr.com/)
>    - [cheat sheet 2](https://www.studocu.com/en-us/document/hudson-city-school-district-online-school/computer-science/vim-cheat-sheet/115142164)
>    - [yt video explaining some vim bindings](https://www.youtube.com/watch?v=yX_Qdr9-7kg)