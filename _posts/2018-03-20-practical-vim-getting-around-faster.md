---
layout: post
title: "Practical Vim: Getting Around Faster"
date: 2018-03-20 01:00:00
categories: en
tags: Vim
---

* content
{:toc}

> What's this: notes on _Practical Vim_ by D. Neil.

## Chap 8 - Navigate Inside Files with Motions

### Tip 46: Keep Your Fingers on the Home Row

_home row_: left hand on `a` `s` `d` `f`; right hand on `h` `j` `k` `l`.

| Command | Move cursor      |
|---------|------------------|
| `h`     | one column left  |
| `l`     | one column right |
| `j`     | one line down    |
| `k`     | one line up      |

### Tip 47: Distinguish Between Real Lines and Display Lines

When the `wrap` setting is enabled (it's on by default), each line that exceeds the window width will display as wrapped.

| Command | Move cursor                                 |
|---------|---------------------------------------------|
| `gj`    | down one display line                       |
| `gk`    | up one display line                         |
| `0`     | to first character of real line             |
| `g0`    | to first character of display line          |
| `^`     | to first nonblank character of real line    |
| `g^`    | to first nonblank character of display line |
| `$`     | to end of real line                         |
| `g$`    | to end of display line                      |

### Tip 48: Move Word-Wise

| Command | Move cursor                          |
|---------|--------------------------------------|
| `w`     | forward to start of next word        |
| `b`     | backward to previous 'start of word' |
| `e`     | forward to next 'end of word'        |
| `ge`    | backward to end of previous word     |

`ea` acts like 'append at the end of the current word'; `gea` acts like 'append at the end of the previous word'.

For each of the word-wise motions, there is a WORD-wise equivalent, including `W` `B` `E` `gE`.
A WORD is defined as consisting of a sequence of nonblank characters separated with whitespaces.

### Tip 49: Find by Character

| Command   | Effect                                                              |
|-----------|---------------------------------------------------------------------|
| `f{char}` | forward to the next occurrence of `{char}`                          |
| `F{char}` | backward to the previous occurrence of `{char}`                     |
| `t{char}` | forward to the character before the next occurrence of `{char}`     |
| `T{char}` | backward to the character after the previous occurrence of `{char}` |
| `;`       | repeat the last character-search command                            |
| `,`       | reverse the last character-search command                           |

Character search can be used like a motion, and hence can be combined with `d{motion}` `c{motion}` to finish more complicated operations.

It is better to choose target characters with a low frequency of occurrences, e.g. `x` `z` are better than `e` `f`.

### Tip 50: Search to Navigate

`/` to search for characters in the buffer in a forward direction; `?` in a backward direction.
Note that it jumps to put the cursor right at the beginning of the occurrence.

`n` to jump to the next occurrence by repeating the previous search; `N` to jump in the inverse direction.

Search can help in Visual mode to guide text selection.

Search can be combined with `d{motion}`.
