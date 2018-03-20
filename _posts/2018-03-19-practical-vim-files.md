---
layout: post
title: "Practical Vim: Files"
date: 2018-03-19 01:00:00
categories: en
tags: Vim
---


* content
{:toc}

> What's this: notes on _Practical Vim_ by D. Neil.

## Chap 6 - Manage Multiple Files

### Tip 36: Track Open Files with the Buffer List

When we are editing a file, we are actually operating on an in-memory representation of a file, named _buffer_.

`:ls`: show the buffer list.

`:bnext`: switch to the next buffer in the buffer list, in which the `#` symbol represents the alternate file.

`<C-^>`: toggle between the current and alternate files.

`:buffer N`: jump to a buffer by number.

`:buffer {bufname}`: jump to a buffer by name. The `{bufname}` need only contain enough characters from the filepath to uniquely identity the buffer.

`:bufdo`: allows to execute an Ex command in all of the buffers listed by `:ls`.
In practice, `:argdo` may be more practical.

Deleting buffers can be in one of these forms:

```
:bdelete N1 N2 N3
:N,M bdelete
```

`:bdelete` can be written as `:bd`.

### Tip 37: Group Buffers into a Collection with the Argument List

`:args`: provides the argument list. 
The argument list represents the list of files that was passed as an argument when we ran the `vim` command.

`:args {arglist}`: the `{arglist}` can include filenames, wildcards, or even the output from a shell command. 
The technique works fine if we want to add some buffers.

__Specify Files by Glob__

| Glob            | Files Matching the Expansion                                                   |
|-----------------|--------------------------------------------------------------------------------|
| `:args *.*`     | `index.html`<br>`app.js`                                                       |
| `:args **/*.js` | `app.js` <br> `lib/framework.js` <br> `app/controllers/Mailers.js` <br> ...etc |
| `:args **/*.*`  | `app.js` <br> `index.js`<br> `lib/framework.js` <br> `lib/theme.css`<br>`app/controllers/Mailers.js` <br> ...etc |

__Specify Files by Backtick Expansion__

`` :args `cat .chapters` ``: execute the text inside the backtick characters in the shell, using the output from the `cat` command as the argument for the `:args` command.
Here, we get the contents of the hidden file `.chapters` and pass them to `:args`.

__Use the Argument List__

Argument list is simpler to manage than Buffer list.

- `:args {arglist}` can clear and repopulate the list with a single command
- `:next` `:prev` can traverse the files in the list
- `:argdo` can execute the same command on each buffer in the arg list

### Tip 38: Manage Hidden Buffers

| Command    | Effect                                                                         |
|------------|--------------------------------------------------------------------------------|
| `:w[rite]` | write the contents of the buffer to disk                                       |
| `:e[dit]!` | read the file from disk back into the buffer (that is, revert unsaved changes) |
| `:qa[ll]!` | close all windows, discarding changes without warning                          |
| `:wa[ll]`  | write all modified buffers to disk                                             |

Vim does not allow going to other items in Argument list until we save the changes in the current buffer. 
If `hidden` setting is enabled, then we can use the `:next` `:bnext` `:cnext` commands without a trailing bang.
If the active buffer is modified, Vim will automatically hide it when we navigate away from it.
The `hidden` setting makes it possible to use `:argdo` and `:bufdo` to change a collection of buffers with a single command.

### Tip 39: Divide Workspace into Split Windows

| Command            | Effect                                                                              |
|--------------------|-------------------------------------------------------------------------------------|
| `<C-w>s`           | split the current window horizontally, reusing the current buffer in the new window |
| `<C-w>v`           | split the current window vertically, reusing the current buffer in the new window   |
| `:sp[lit] {file}`  | split the current window horizontally, loading `{file}` into the new window         |
| `:vsp[lit] {file}` | split the current window vertically, loading `{file}` into the new window           |


| Command  | Effect                        |
|----------|-------------------------------|
| `<C-w>w` | cycle between open windows    |
| `<C-w>h` | focus the window to the left  |
| `<C-w>j` | focus the window below        |
| `<C-w>k` | focus the window above        |
| `<C-w>l` | focus the window to the right |

`<C-w><C-w>` does the same thing as `<C-w>w`; so as the other four in the above table.

| Ex Command | Normal Command | Effect                                          |
|------------|----------------|-------------------------------------------------|
| `:cl`      | `<C-w>c`       | close the active window                         |
| `:on[ly]`  | `<C-w>o`       | keep only the active window, closing all others |


| Keystrokes  | Buffer Contents                          |
|-------------|------------------------------------------|
| `<C-w>=`    | equalize width and height of all windows |
| `<C-w>_`    | maximize height of the active window     |
| ` <C-w>\| ` | maximize width of the active window      |
| `[N]<C-w>_` | set active window height to `[N]` rows   |
| `[N]<C-w>\|`| set active window width to `[N]` columns |
