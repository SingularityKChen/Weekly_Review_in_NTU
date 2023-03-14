---
layout: post
title: "[Tutorial] Obtain Objects in the collection in Genus Using Tcl"
description: "collection is an extension provided by EDA vendors like Synopsys to support a list of objects in their Tcl API. Usually, most database query operations in Cadence and Synopsys would return a collection object. Complex query operations with filters may be slow in large design. Pre-store the query results might reduce runtime when it will be used in multiple places."
categories: [Tutorial]
tags: [Cadence, Genus, Tcl]
last_updated: 2023-03-14 20:24:00 GMT+8
excerpt: "collection is an extension provided by EDA vendors like Synopsys to support a list of objects in their Tcl API. Usually, most database query operations in Cadence and Synopsys would return a collection object. Complex query operations with filters may be slow in large design. Pre-store the query results might reduce runtime when it will be used in multiple places."
redirect_from:
  - /2023/03/14/
---

* Kramdown table of contents
{:toc .toc}

# Obtain Objects in the collection in Genus Using Tcl

## Introduction

`collection` is an extension provided by EDA vendors like Synopsys to support a list of objects in their Tcl API. Usually, most database query operations in Cadence and Synopsys would return a collection object.

For instance, the following Cadence Genus command would return a hex value, which is assigned to the variable `$reg_collection`.

```tcl
genus> set reg_collection [all_registers -clock clk]
0x123
```

## Access `collection`

To access the objects of the collection, these commands could be used in Cadence Genus:

`get_cell` `get_cells` `get_db`

## Examples

Complex query operations with filters may be slow in large design. Pre-store the query results might reduce runtime when it will be used in multiple places.

```tcl
set reg_collection [all_registers -clock clk]
report_timing -from [all_inputs] -to [get_db $reg_collection]
report_timing -from [get_db $reg_collection] -to [get_db $reg_collection]
report_timing -from [get_db $reg_collection] -to [all_outputs]
```

