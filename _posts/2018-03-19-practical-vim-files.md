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
:bdelete N1 N2 N3`
:N,M bdelete`
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
