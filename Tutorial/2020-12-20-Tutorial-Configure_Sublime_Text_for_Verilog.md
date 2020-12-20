---
layout: post
title: "[Tutorial] Configure Sublime Text for Verilog"
description: "Configure Sublime Text for Verilog and SystemVerilog"
categories: [Tutorial]
tags: [Verilog, Sublime]
last_updated: 2020-12-20 16:43:00 GMT+8
excerpt: "Configure Sublime Text for Verilog and SystemVerilog"
redirect_from:
  - /2020/12/20/
---

* Kramdown table of contents
{:toc .toc}
# Configure Sublime Text for Verilog

## Download and install Sublime Text

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

sudo apt-get install apt-transport-https

echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

sudo apt-get update
sudo apt-get install sublime-text
```

## Install some plugins for Verilog

[This](https://zhuanlan.zhihu.com/p/33443736) commends three plugins:

+  `SystemVeriog`: syntax highlights.

+ `VerilogGadget`: helps to generate a simple testbench, instantiate a module, insert a  user-header, repeat codes with formatted incremental/decremental  numbers, etc.

+ `SublimeLinter` and `SublimeLinter-contrib-verilator`: provides an interface to Verilator.

To install packages:

<kbd>Crtl</kbd> + <kbd>Shift</kbd> + <kbd>p</kbd>

Then input `install`.

## Configure `SublimeLinter`

Configure it like this

```json
// SublimeLinter Settings - User
{
    "no_column_highlights_line": true,
    "linters":
    {
        "verilator": {
            "lint_mode": "load_save",
            "styles" : [
                {
                    "types": ["warning"],
                    "mark_style": "squiggly_underline",
                    "icon": "Packages/SublimeLinter/gutter-themes/Default/cog.png"
                },
                {
                    "types": ["error"],
                    "mark_style": "fill",
                    "icon": "Packages/SublimeLinter/gutter-themes/Default/cog.png"
                }
            ],
            "args": [
                "--error-limit",
                "500",
                "--default-language",
                "1800-2017", // systemverilog
                // "1364-2005", // verilog
                "--bbox-sys",
                "--bbox-unsup",
                "-Wall",
                "-Wno-WIDTH",
                "-Wno-IGNINC",
                "-Wno-IGNDEF",
                "-Wno-STMTDLY",
                "-Wno-UNDRIVEN",
                "-Wno-PINCONNECTEMPTY",
                "-Wno-INPUTPINEMPTY",
                "-Wno-OUTPUTPINEMPTY"
            ],
            "filter_errors": [
                "Unsupported:",
                "\\[IGNDEF\\]",
                // "expects 8192 bits" // not to use -Wno-WIDTH
            ],

            // to lint based on single file (ignoring external module definition)
            //   "use_multiple_source": false,
            //   "search_project_path": false,

            // to lint based on multiple files (searching external sources - the same directory or project path)
            //   "use_multiple_source": true,
            //   "search_project_path": true,
            //  example) example.sublime-project
            //       "sources": [ "D:\\project\\srcs", "D:\\project\\working" ]

            "use_multiple_source": false,
            "search_project_path": false,

            // windows subsystem for linux (wsl verilator_bin)
            "use_wsl": false,

            // additional option to filter file type
            "extension": [
                ".v",
                ".sv",
                ".svh"
            ],
            // additional option for better highlighting near
            "message_near_map": [
                ["Case values", "case"],
                ["Suggest casez", "casex"]
            ]
        }
    }
}

```

## Configure Sublime Text

```json
{
	"font_size": 18,
	"highlight_line": true,
	"ignored_packages":
	[
		"Vintage"
	],
	"tab_size": 2,
	"translate_tabs_to_spaces": true,
	"word_wrap": true
}
```

