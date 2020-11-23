---
layout: post
title: "[Tutorial] Install PDK"
description: "How to install the PDK and obtain the db files"
categories: [Tutorial]
tags: [db, PDK]
last_updated: 2020-11-23 23:19:00 GMT+8
excerpt: "How to install the PDK and obtain the db files"
redirect_from:
  - /2020/11/23/
---

* Kramdown table of contents
{:toc .toc}
# Install PDK

## Types of files

```text
GDS LAYER USAGE DESCRIPTION FILE  
APR TECHNOLOGY FILE 
DUMMY OD/PO GENERATION UTILITY
LVS  COMMAND FILE
LAYOUT EDITOR TECHNOLOGY FILE 
RC TECH FILE
DRC COMMAND FILE 
```

## `.db` Files for Design Compiler

```
--doc
--GP
--info
--LP
--util
```

`GP` means general propose, and `LP` means low power.

Under this two folders, you will find these folders:

```
--doc
--IO2.5V
--IO3.3V
--pdk
--stclib
	|--7-track
	|--9-track
	|--10-track
	|--12-track
--util
```

The tracks are related to the row. The bigger, the higher performance and power consumptions.

Inside each track, there are three different kinds of voltages

```
--xxx_AA_FE
--xxxhvt_AA_FE
--xxxlvt_AA_FE
```

They're high, normal and low voltages.
