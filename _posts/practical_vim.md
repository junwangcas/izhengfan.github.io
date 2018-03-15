## Chap One - The Vim Way

### Tip 1: Repeat with Dot Command

`.`: Repeat last _change_

`>G`: indent lines until the end of file

### Tip 2: Append line and repeat

`A`: append to the end of current line

`C` = `c$`: delete until the end of line, into Insert mode

`s` = `cl`: delete the character at right, into Insert mode

### Tip 3: Forward scan and repeat

`f+`: forward scan line for character `+`

`;`: forward scan line for latest forward-scanned character

`;.`: forward scan line for latest forward-scanned character, repeat lately executed change

### Tip 4: Repeat and Reverse

|Intent                           | Act                   | Repeat | Reverse |
|---------------------------------|-----------------------|--------|---------|
|make a change                    | {edit}                | `.`    | `u`     |
|scan line for next char          | `f{char}`/`t{char}`   | `;`    | `,`     |
|scan line for previous char      | `F{char}`/`T{char}`   | `;`    | `,`     |
|scan document for next match     | /pattern`<CR>`        | `n`    | `N`     |
|scan document for previous match | ?pattern`<CR>`        | `n`    | `N`     |
|Perform substitution             | :s/target/replacement | `&`    | `u`     |
|execute a sequence of changes    | `qx{changes}q`        | `@x`   | `u`     |


### Tip 5: Find and replace by hand

`*`: find the next string matched to the word below cursor

`n`: find the next matched string searched lately, which can follow the `/` or `*` command

`cw`: delete to the end of the word, into Insert mode.

`n.`: go to next matched string searched lately, repeat the lately executed change

### Tip 6: Revisit the Dot Formula

The Ideal: One Keystroke to Move, One Keystroke to Execute


## Chap Two -  Normal Mode

### Tip 8: Chunk Your Undos

`i{insert some text}<Esc>` constitutes a change that can be undone by a `u` stroke. Therefore, in Insert Mode, when openning a new line, try pressing `<Esc>o` instead of `<CR>` to keep the granularity in the change flow.

__Special case:__ If we use the `<Up>`, `<Down>`, `<Left>`, or `<Right>` cursor keys while in Insert mode, a new undo chunk is created. It’s just as though we had switched back to Normal mode to move around with the `h`, `j`, `k`, or `l` commands, except that we don’t have to leave Insert mode. This also has implications on the operation of the dot command.

### Tip 9: Compose Repeatable changes

`aw`: a word

`daw`: delete a whole word (this `daw` can be repeated as one step using `.`)

### Tip 10: Use Counts to Do Simple Arithmetic

The `<C-a>` and `<C-x>` commands perform addition and subtraction on numbers (at or after the cursor). `18<C-a>` will add 18 to the number. When run without a count, they increment by one.

### Tip 11: Count vs Repeat

__Repeat over Count__: `dw.` may be better than `2dw` or `d2w` in: 

1. counting is tedious 

2. it can be undone by `u` with one word in one step; 3) `u` and `.` commands have more granularity

__Count over Repeat__: 

1. if you want to change 3 words to another series of characters, `c3w` followed by those characters would be better than `dw..` and `i` and those characters

2. a clean and coherent undo history

### Tip 12: Combine and Conquer

__Operator + Motion = Action__:

The `d{motion}` commands can be like 

1. `dl` delete a character

2. `daw` delete a whole word

3. `dap` delete an entire paragraph

The same goes for `c{motion}`, `y{motion}`, etc. Find a complete list of operators by `:h operator`. Some of them are:

| Trigger | Effect                                           |
|---------|--------------------------------------------------|
| `c`     | change                                           |
| `d`     | delete                                           |
| `y`     | yank into register (copy)                        |
| `g~`    | swap case                                        |
| `gu`    | make lowercase                                   |
| `gU`    | make uppercase                                   |
| `>`     | shift right                                      |
| `<`     | shift left                                       |
| `=`     | autoindent                                       |
| `!`     | filter {motion} lines though an external program |

One more rule: invoking an operator in duplicate would act upon the current line, like

1. `dd` deletes the current line

2. `>>` indents the current line

3. `gUgU` or `gUU` make the current line all uppercase

__Custom Operators__

Check by `:h :map-operator`

## Chap Three - Insert Mode

### Tip 13: Corrections in Insert Mode

| Keystrokes | Effect                                |
|------------|---------------------------------------|
| `<C-h>`    | delete back one character (backspace) |
| `<C-w>`    | delete back one word                  |
| `<C-u>`    | delete back to start of line          |

These commands can also be used in Vim's command line as well as in the bash shell.

### Tip 14: Back to Normal Mode

| Keystrokes | Effect                |
|------------|-----------------------|
| `<Esc>`    | to Normal mode        |
| `<C-[>`    | to Normal mode        |
| `<C-o>`    | to Insert Normal mode |

Insert Normal mode is s special version of Normal mode. It allows one command to execute, after which it will return to Insert mode immediately.

For example, the `zz` command redraws the screeen with the current line in the middle of the window. Thus, in Insert mode, `<C-o>zz` helps to move our input flow into the middle of the window.

### Tip 15: Paste without Leaving Insert Mode

`<C-r>{register}` paste those in {register} in Insert mode.

`<C-r><C-p>{register}` is smarter, which inserts text literally and fixes any unintended indentation. But it's a bit of handful. When pasting a register containing  multiple lines of text, consider switching to Normal mode.

### Tip 16: Do Calculations in Place

The _expression register_ allows us to perform calculations directly. It is addressed by the `=` symbol. 

In Insert mode, `<C-r>=`6*35`<CR>` will insert 210 directly in the text.


### Tip 17: Insert Unusual Characters by Character Code 

In Insert mode, `<C-v>{code}` can input the character whose addresss is {code}. 

`<C-v>u{00bf}` will insert the character whose unicode address is 00bf. It should be a four-digit hexadecimal code.

`ga` outputs a message showing the address of the character under cursor.

| Keystrokes            | Effect                                                   |
|-----------------------|----------------------------------------------------------|
| `<C-v>{123}`          | insert character by decimal code                         |
| `<C-v>u{1234}`        | insert character by hexadecimal code                     |
| `<C-v>{nondigit}`     | insert nondigit literally                                |
| `<C-k>{char1}{char2}` | insert character represented by `{char1}{char2}` digraph |

### Tip 18: Insert Unusual Characters by Digraph

`<C-k>?I`: the ¿ character.

`<C-k>12`: the ½ character.

`<C-k>>>`: the » character.

`<C-k>sa`: the さ character.

Get help by `:h digraphs-default` or `:h digraph-table`.


### Tip 19: Overwrite Existing Text with Replace Mode

In Normal mode, `R` triger Replace mode.

In Insert mode, the `<Insert>` key in common keyboards can toggle between Insert and Replace mode. 

`gR` triggers _Virtual Replace mode_, which treats the tab character as though it consisted of spaces.

`r` `gr` provide the single-shot versions of Replace and Virtual Replace mode.

## Chap Four - Visual Mode

### Tip 20: Grok Visual Mode

Commands that work the same as Normal mode in Visual:

- `h` `j` `k` `l` to move cursor
- `f{char}` to jump to a character 
- `;` `,` to repeat or reverse the jump by `f{char}`
- search commmands (together with `n`/`N`) to jump to pattern matches
- `c` to change text and go into Insert mode (after selection)

### Tip 21: Define a Visual Selection

__Enable Visual mode from Normal mode__

| Command | Effect                             |
|---------|------------------------------------|
| `v`     | enable character-wise Visual mode  |
| `V`     | enable line-wise Visual mode       |
| `<C-v>` | enable block-wise Visual mode      |
| `gv`    | reselect the last visual selection |

__Switching between Visual modes__

| Command         | Effect                                                                                           |
|-----------------|--------------------------------------------------------------------------------------------------|
| `<Esc>`/`<C-[>` | switch to Normal mode                                                                            |
| `v`/`V`/`<C-v>` | switch to Normal mode (when used from character-, line-, or block-wise Visual mode, respectively |
| `v`             | switch to character-wise Visual mode                                                             |
| `V`             | switch to line-wise Visual mode                                                                  |
| `<C-v>`         | switch to block-wise Visual mode                                                                 |
| `o`             | go to the other end of highlighted text                                                          |

### Tip 22: Repeat Line-Wise Visual Commands




