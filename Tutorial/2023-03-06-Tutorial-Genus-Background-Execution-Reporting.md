---
layout: post
title: "[Tutorial] Background Execution of Reporting Commands in Cadence Genus"
description: "Cadence Genus supports doing report in parallel and running them in the background. This tutorial introduces how to conditional enable this feature using Tcl syntax."
categories: [Tutorial]
tags: [Cadence, Genus, Tcl]
last_updated: 2023-03-12 12:30:00 GMT+8
excerpt: "Cadence Genus supports doing report in parallel and running them in the background. This tutorial introduces how to conditional enable this feature using Tcl syntax."
redirect_from:
  - /2023/03/06/
---

* Kramdown table of contents
{:toc .toc}

# Background Execution of Reporting Commands in Cadence Genus

Cadence Genus supports doing report in parallel and running them in the background with `run_parallel_commands`. 
Number of parallel processes depends on the
`max_cpu_per_server` setting.
Supported commands: `report*`, `write*`, `check_timing_intent`. 

This tutorial introduces how to conditional enable this feature by adding a conditional prefix to the supported commands.

## Step 1: query Genus version

Get the Genus version using `get_db program_major_version`, which will be used for condition.

```tcl
set genus_version [get_db program_major_version]
```

## Step 2: set conditional command

Set a conditional string that if supports `run_parallel_commands`, then it is the prefix of supported commands. Otherwise, the prefix is null.

```tcl
if { $genus_version >= 21 } {
  set parallel_cmd "run_parallel_commands -log_dir $LOG_PATH -queue "
} else {
  set parallel_cmd {}
}
```

## Step 3: add a prefix to the supported commands.

Using the following syntax to add the command to a queue.

```tcl
"${parallel_cmd}\"YOUR CMD\""
# e.g.
"${parallel_cmd}\"report_timing\""
```

## Step 4: execute the commands in the queue

After adding all the commands within one stage, you have to execute them parallelly using `run_parallel_commands -execute`.

```tcl
if {$genus_version >= 21 }{
  run_parallel_commands -execute
}
```

## Summary

```tcl
set genus_version [get_db program_major_version]
if { $genus_version >= 21 } {
  set parallel_cmd "run_parallel_commands -log_dir $LOG_PATH -queue "
} else {
  set parallel_cmd {}
}

"${parallel_cmd}\"YOUR CMD\""

if {$genus_version >= 21 }{
  run_parallel_commands -execute
}
```
