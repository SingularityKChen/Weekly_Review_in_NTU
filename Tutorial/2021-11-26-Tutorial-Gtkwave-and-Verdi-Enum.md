---
layout: post
title: "[Tutorial] GTKWave and Verdi Enum tcl Commands"
description: "In this tutorial, the tcl commands of GTKWave and Verdi for displaying enum are introduced."
categories: [Tutorial]
tags: [GTKWave, Verdi, tcl]
last_updated: 2021-12-02 23:23:00 GMT+8
excerpt: "In this tutorial, the tcl commands of GTKWave and Verdi for displaying enum are introduced."
redirect_from:
  - /2021/11/26/
---

* Kramdown table of contents
{:toc .toc}
# GTKWave and Verdi Enum tcl Commands

## GTKWave

To set the enum list to a set of signals, there are two steps[^1]:

1. set Translate Filters
2. apply Translate Filters

### Set Translate Filters

`setCurrentTranslateEnums`: sets the enum list to function as the current translate file and returns the corresponding which value to be used with `gtkwave::installFileFilter`. As a real file is not used, the results of this are not recreated when a save file is loaded or if the waveform is reloaded.

Syntax: `set which_f [ gtkwave::setCurrentTranslateEnums elist ]`

Example:

```tcl
set enums [list]
lappend enums 0000000000000000 IDLE 
lappend enums FFFFFFFFFFFFFFFF BUSY
lappend enums 3000000000000000
lappend enums 0123456789ABCDEF
lappend enums 1111111111111111
set which_f [ gtkwave::setCurrentTranslateFile $enums ]
puts "$which_f"
```

### Apply Translate Filters

To apply the filters, the function `signalShowString` can be called.

```tcl
proc signalShowString {signals fileFilter} {
    gtkwave::addSignalsFromList $signals
    gtkwave::highlightSignalsFromList $signals
    gtkwave::installFileFilter $fileFilter
    gtkwave::unhighlightSignalsFromList $signals
}
```

## Verdi


### **wvAddSignal**

**Description**

Add signals to the *nWave* window. 

**Syntax**

```tcl
wvAddSignal [-win *window*] [-delim *delim*] [-clear] {*signalName* [-color *color*] | [-shiftSignal *signal*] [-sdfSignal *localName*] [-risingDelay *delay timescale* -fallingDelay *delay timescale*]} | {-group {*groupName* {*signalName* [-color *color*]}}} [-scope *scopeName*]
wvAddSignal [-win *window*] [-delim *delim*] {-v *variableFullName*} wvAddSignal [-win *window*] [-delim delim] {-pw *fullSignalName*} wvAddSignal [-win *window*] [-delim *delim*] -scope *fullScopeName* wvAddSignal [-win *window*] [-delim *delim*] -scope *fullScopeName* [-type input|output|inout|net|register|other]
wvAddSignal [-win *window*] "-mem *2DMemoryArray*"
wvAddSignal [-marker] -group {{*groupName* {*signalName* [-color color]}}} [-scope *scopeName*]
wvAddSignal [-win *window*] [delim *delim*] {-ss *fullSignalName*} wvAddSignal [-win *window*] [delim *delim*] {-sn *fullSignalName*}
```

**Arguments**

`-clear`

  Clear the *nWave* window before adding signals. 

`-color color`

  Specify the color for the signal name.

`-delim delim `

  Delimiter of the specified signal name. The default is '/'.

`-fallingDelay delay timescale `

  Specify the delay and time scale for falling edge.

`-group`

  Add the specified list of signals to the specified group name.

`{groupName}`

  Add the signals to this specified group name.

`-marker`

When this option is specified with the **-group** option, signals and the specified group is added with reference to the marker position. When this option is specified without the **-group** option is not specified, a group name is created.

`-mem`

Show the two-dimensional (2D) memory array evaluated (not in simulator-dumped FSDB) by Verdi on *nWave*. This option is only for 2D array.

`-pw fullSignalName`

Specify the runtime power state signal to be added and evaluated.

`-risingDelay delay timescale`

Specify the delay and time scale for rising edge.

`-scope fullScopeName`

Specify the scope where signals are added from.

`-sdfSignal localName`

Specify the local name for the shifted signal.

`-shiftSignal signal` 

Specify the signal to shift.

`{signalName}`

Specify the signal to add into the current position with the `-color`, `-shiftSignal`, `-sdfSignal`, `-risingDelay`, and `-fallingDelay` attributes, if any are specified.

`-sn fullSignalName `

Specify the inserted signal as the runtime power supply net signal to be evaluated.

`–ss fullSignalName`

Specify the power supply set signal to be added and evaluated.

`-type input|output|inout|net|register|other`

Specify the type of signals to be added in the selected scope. The types are **input**, **output**, **inout**, **net**, **register**, and **other**. Multiple types can be specified together with a space as the separator.

`-v variableFullName`

Specify the full variable signal name.

`-win window`

Specify the window ID of the invoking *nWave* window.

**Value Returned**

1 if successful; otherwise, returns 0.

**Example**

```tcl
wvAddSignal -group {GA  {test/mem_c/arbiter/MEM_RD} {test/mem_c/arbiter/MEM}} {GB {/test/A} {/test/B}}
wvAddSignal {/system/I_cpu/I_ALUB/U288/A1} -shiftSignal {A1} -sdfSignal {U288.A1} -risingDelay 28.0 1p -fallingDelay 2.00 1p
wvAddSignal {/test/C} -color ID_YELLOW8  {/test/D}
wvAddSignal -v {top/v1} -v {top/v2}
wvAddSignal -scope top.i_cpu
wvAddSignal -win $_nWave2 "-mem system.i_pram.macron[2:0]"
wvAddSignal -win $_nWave3 {-pw /$power_root/@{\system.PDsystem}}
wvAddSignal -win $_nWave1 -scope /system -type inout register
wvAddSignal -marker -group {GA {test/mem_c/arbiter/MEM_RD} {test/mem_c/arbiter/MEM}} {GB {/test/A} {/test/B}}
wvAddSignal -win $_nWave3 {-ss /$power_root/$power_supplyset}
wvAddSignal -win $_nWave2 {-sn /$power_root/$power_net/\$VDD_TOP}
```

### **wvSetAliasTable**

**Description**

Set alias table to signal(s).

**Syntax**

```tcl
wvSetAliasTable [-win window] [-signal {(group index)}] -table aliasTable [-global]
```

**Arguments**

`-signal {(group index)}`

  If specified, alias table is applied to the specified signal position. Otherwise, apply to selected.

`-table aliasTable `

  Specify the alias table name.

`-global`

  If specified, the alias table is applied globally.

`-win window `

  Specify the window ID of the invoking *nWave* window.

**Value Returned**

1 if successful; otherwise, returns 0.

**Example**

```tcl
wvSetAliasTable -win $_nWave1 -signal {(G1 1) (G2 2)} -table fsm_0 -global
```

# Reference

[^1]: https://github.com/chipsalliance/firrtl/pull/2397/commits/27839dbe39035cbb1a175c8bd1c537b827082708#
