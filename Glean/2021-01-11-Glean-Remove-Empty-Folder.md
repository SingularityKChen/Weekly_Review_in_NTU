---
layout: post
title: "[Glean] Remove Empty File Folder"
description: "The find and xargs commands in Linux."
categories: [Glean]
tags: [Linux, Find, Xargs]
last_updated: 2021-01-11 10:38:00 GMT+8
excerpt: "Introduces two Linux command find and xargs. By combining this two command, you can easily remove empty directories and finish more jobs."
redirect_from:
  - /2021/01/11/
---

* Kramdown table of contents
{:toc .toc}
# Remove Empty File Folder

We can find the empty directories by `find` command.

```bash
find -type d -empty
```

And the results can be send to the command `xargs` or the build-in function `-exec` in `find`:

```bash
find -type d -empty -exec rm -f {}
```

Or

```bash
find -type d -empty | xargs rm -rf
```

## Introduction to `find`[^1]

### `-empty`

File is empty and is either a regular file or a directory.

### `-name pattern`

Base of file name (the path with the leading directories removed) matches shell pattern pattern.

### `-ctime n`

File's status was last changed less than, more than or exactly n*24 hours ago.  See the comments for `-atime` to understand how rounding affects the interpretation of file status change times.

### `-size n[cwbkMG]`

File uses less than, more than or exactly n units of space, rounding up.  The following suffixes can be used:

- `b`    for 512-byte blocks (this is the default if no  suffix is used)
- `c`    for bytes
- `w`    for two-byte words
- `k`    for kibibytes
- `M`    for mebibytes
- `G`    for gibibytes

### `-exec command {}`

This variant of the -exec action runs the specified command on the selected files, but the command line is built by appending each selected file name at the end; the total number of invocations of the command will be much less than the number of matched files.  The command line is built in much the same way that `xargs` builds its command lines.

## Introduction to `xargs`[^2]

`xargs` is short for eXtended ARGuments.

### `-a inputfile`

Read names from the file `inputfile` instead of standard input. If you use this option, the standard input stream remains unchanged when commands are run. Otherwise, stdin is redirected from `/dev/null`.

### `-d delim`

Input file names are terminated by the specified character `delim` instead of by whitespace, and any quotes and backslash characters are not considered special (every character is taken literally).  Disables the logical end of file marker string, which is treated like any other argument.

```bash
# echo "nameXnameXnameXname" | xargs -d X

name name name name
```

### `-n max-args`

Use at most `max-args` arguments per command line.  Fewer than `max-args` arguments will be used if the size (see the`-s` option) is exceeded, unless the `-x` option is given, in which case `xargs` will exit.

### `-I replace-str`

Replace occurrences of `replace-str` in the initial arguments with names read from standard input.  Also, unquoted blanks do not terminate arguments; instead, the input is split at newlines only.  If `replace-str` is omitted (omitting it is allowed only for `-i`), it defaults to `{}` (like for `find -exec`). Implies `-x` and `-l 1`.  The `-i` option is deprecated in favor of the ‘-I’ option.

```bash
# cat arg.txt | xargs -I {} ./sk.sh -p {} -l

-p aaa -l
-p bbb -l
-p ccc -l

ls *.jpg | xargs -n1 -I {} cp {} /data/images
```

### `-t`

Print the command line on the standard error output before executing it.



[^1]: https://man7.org/linux/man-pages/man1/find.1.html
[^2]: https://www.gnu.org/software/findutils/manual/html_node/find_html/xargs-options.html