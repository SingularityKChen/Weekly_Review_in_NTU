---
layout: post
title: "[Tutorial] Count the number of occurrences of a pattern in a file in Vim"
description: "Count the number of occurrences of a pattern in a file in Vim."
categories: [Tutorial]
tags: [Vim, Regex]
last_updated: 2024-04-18 9:43:00 GMT+8
excerpt: "Count the number of occurrences of a pattern in a file in Vim."
redirect_from:
  - /2024/04/18/
---

* Kramdown table of contents
{:toc .toc}

# Count the number of occurrences of a pattern in a file in Vim

```vim
:%s/pattern//gn
```

## Explanation:

+ `%` is the range, which means the whole file.
+ `s` is the substitute command.
+ `pattern` is the pattern to search.
+ `//` is the replacement, which is empty in this case.
+ `g` is the flag to replace all occurrences in a line.
+ `n` is the flag to count the number of occurrences.
