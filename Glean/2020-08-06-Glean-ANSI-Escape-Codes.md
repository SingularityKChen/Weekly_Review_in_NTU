---
layout: post
title: "[Glean] ANSI Escape Codes"
description: "ANSI escape sequences are a standard for in-band signaling to control the cursor location, color, and other options on video text terminals and terminal emulators."
categories: [Glean]
tags: [CPP, ANSI]
last_updated: 2020-08-06 15:48:00 GMT+8
excerpt: "ANSI escape sequences are a standard for in-band signaling to control the cursor location, color, and other options on video text terminals and terminal emulators."
redirect_from:
  - /2020/08/06/
---

* Kramdown table of contents
{:toc .toc}
# ANSI Escape Codes

We can use `printf("\033[XXX ")` to beautify our `c++` log info.

e.g., 

```c++
printf("\033[1;33m Hello World. \033[0m \n");
//    \033    [    (1;)         33         m      xxxx       \033[0m
//      |           |           |          |        |          |
//     ESC       bg color   font color    end   your char    reset
```

`\033` represents <kbd>Esc</kbd> in [ANSI Escape Code](https://en.wikipedia.org/wiki/ANSI_escape_code).

## ANSI Control Code

ANSI escape sequences are a standard for in-band signaling to control the cursor location, color, and other options on video text terminals and terminal emulators.[^1]

| Code    | Effect                                                       | Note                                                         |
| ------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 0       | Reset / Normal                                               | All attributes off                                           |
| 1       | Bold or increased intensity                                  | As with faint, the color change is a PC (SCO / [CGA](https://en.wikipedia.org/wiki/Color_Graphics_Adapter)) invention.[[33\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-SCO-34)[*[better source needed](https://en.wikipedia.org/wiki/Wikipedia:NOTRS)*] |
| 2       | Faint or decreased intensity                                 | aka Dim (with a saturated color). May be implemented as a light [font weight](https://en.wikipedia.org/wiki/Font_weight) like bold.[[34\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-35) |
| 3       | Italic                                                       | Not widely supported. Sometimes treated as inverse or blink.[[33\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-SCO-34) |
| 4       | Underline                                                    | Style extensions exist for Kitty, VTE, mintty and iTerm2.[[35\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-color-u-36)[[36\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-color-u-kitty-spec-37) |
| 5       | Slow Blink                                                   | less than 150 per minute                                     |
| 6       | Rapid Blink                                                  | MS-DOS ANSI.SYS, 150+ per minute; not widely supported       |
| 7       | [Reverse video](https://en.wikipedia.org/wiki/Reverse_video) | swap foreground and background colors, aka invert; inconsistent emulation[[37\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-console-termio-realize-38) |
| 8       | Conceal                                                      | aka Hide, not widely supported.                              |
| 9       | [Crossed-out](https://en.wikipedia.org/wiki/Strikethrough)   | aka Strike, characters legible but marked as if for deletion. |
| 10      | Primary (default) font                                       |                                                              |
| 11–19   | Alternative font                                             | Select alternative font *n* − 10                             |
| 20      | [Fraktur](https://en.wikipedia.org/wiki/Fraktur_(script))    | Rarely supported                                             |
| 21      | Doubly underline **or** Bold off                             | Double-underline per ECMA-48.[[5\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-ECMA-48-5):8.3.117 *See [discussion](https://en.wikipedia.org/wiki/Talk:ANSI_escape_code#SGR_21—`Bold_off`_not_widely_supported)* |
| 22      | Normal color or intensity                                    | Neither bold nor faint                                       |
| 23      | Not italic, not Fraktur                                      |                                                              |
| 24      | Underline off                                                | Not singly or doubly underlined                              |
| 25      | Blink off                                                    |                                                              |
| 26      | Proportional spacing                                         | [ITU T.61](https://en.wikipedia.org/wiki/ITU_T.61) and T.416, not known to be used on terminals |
| 27      | Reverse/invert off                                           |                                                              |
| 28      | Reveal                                                       | conceal off                                                  |
| 29      | Not crossed out                                              |                                                              |
| 30–37   | Set foreground color                                         | See color table below                                        |
| 38      | Set foreground color                                         | Next arguments are `5;n` or `2;r;g;b`, see below             |
| 39      | Default foreground color                                     | implementation defined (according to standard)               |
| 40–47   | Set background color                                         | See color table below                                        |
| 48      | Set background color                                         | Next arguments are `5;n` or `2;r;g;b`, see below             |
| 49      | Default background color                                     | implementation defined (according to standard)               |
| 50      | Disable proportional spacing                                 | T.61 and T.416                                               |
| 51      | Framed                                                       |                                                              |
| 52      | Encircled                                                    | Implemented as "[emoji variation selector](https://en.wikipedia.org/wiki/Variation_Selectors_(Unicode_block))" in mintty.[[38\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-mintty-39) |
| 53      | Overlined                                                    |                                                              |
| 54      | Not framed or encircled                                      |                                                              |
| 55      | Not overlined                                                |                                                              |
| 58      | Set underline color                                          | Kitty, VTE, mintty, and iTerm2. (not in standard)[[35\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-color-u-36)[[36\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-color-u-kitty-spec-37) Next arguments are `5;n` or `2;r;g;b`, see below |
| 59      | Default underline color                                      | Kitty, VTE, mintty, and iTerm2. (not in standard)[[35\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-color-u-36)[[36\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-color-u-kitty-spec-37) |
| 60      | ideogram underline or right side line                        | Rarely supported                                             |
| 61      | ideogram double underline or double line on the right side   |                                                              |
| 62      | ideogram overline or left side line                          |                                                              |
| 63      | ideogram double overline or double line on the left side     |                                                              |
| 64      | ideogram stress marking                                      |                                                              |
| 65      | ideogram attributes off                                      | reset the effects of all of `60`–`64`                        |
| 73      | superscript                                                  | mintty (not in standard)[[38\]](https://en.wikipedia.org/wiki/ANSI_escape_code#cite_note-mintty-39) |
| 74      | subscript                                                    |                                                              |
| 90–97   | Set bright foreground color                                  | aixterm (not in standard)                                    |
| 100–107 | Set bright background color                                  |                                                              |

## Colours: 30-49, 90-109.

![Colour Codes in ANSI](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200806153945.png)

[^1]: ANSI escape code. https://en.wikipedia.org/wiki/ANSI_escape_code

