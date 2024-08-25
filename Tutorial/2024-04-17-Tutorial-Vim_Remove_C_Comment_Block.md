---
layout: post
title: "[Tutorial] Remove the C/C++ comment block in Vim"
description: "This tutorial introduces how to remove the C/C++ comment blocks and line comments in Vim."
categories: [Tutorial]
tags: [Vim, Regex]
last_updated: 2024-05-16 13:46:00 GMT+8
excerpt: "This tutorial introduces how to remove the C/C++ comment blocks and line comments in Vim."
redirect_from:
  - /2024/04/17/
---

* Kramdown table of contents
{:toc .toc}

# Remove the C comment block in Vim

For a multi-line comment block in C/C++, you can remove it in Vim by the following command:

```vim
:%s/\/\*\_.\{-}\*\///g
```

To remove line comments, you can use the following command:

```vim
:%s/\/\/.*$//g
```

## Explanation:

- `\/\*` — Starts the match with the beginning of a comment block `/*`.
- `\_.*\{-}` — The `\_.*` pattern matches any character including newline (`\_` extends `.` to match newlines), and `\{-}` makes it non-greedy.
- `\*\ /` — Ends the match with the end of a comment block `*/`.
- `/g` — Applies the substitution globally across each line where a match is found.
