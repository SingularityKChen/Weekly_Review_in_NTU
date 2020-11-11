---
layout: post
title: "[CodeStudy] Scala Excel Read: POI XSSF"
description: "In this article, I introduced how to read a workbook, a sheet, a row and a special cell. The methods to obtain the row number and column number are also given. One way to filter empty cells is introduced too."
categories: [CodeStudy]
tags: [Chisel, Scala, POI]
last_updated: 2020-11-11 21:09:00 GMT+8
excerpt: "In this article, I introduced how to read a workbook, a sheet, a row and a special cell. The methods to obtain the row number and column number are also given. One way to filter empty cells is introduced too."
redirect_from:
  - /2020/11/11/
---

* Kramdown table of contents
{:toc .toc}
# Scala Excel Read

In Chisel tester, sometime we need to read some data from excel. In these situation, we need to use `poi` APIs of Java to read the Excel files.

## Import

We need to import both Java `io` and `poi`.

```scala
import java.io.{File, FileInputStream}
import org.apache.poi.xssf.usermodel.{XSSFSheet, XSSFWorkbook}
```

## Read a workbook

First step, new a file. Then using the `poi` to read this file.

```scala
val excelFile = new File(excelFileName)
val excelWorkbook = new XSSFWorkbook(new FileInputStream(excelFile))
```

## Read a sheet

You can using either `.getSheet("sheet name")` or `.getSheetAt(sheet_idx)` to read a sheet.

```scala
val sheet: XSSFSheet = excelWorkbook.getSheet("sheetName")
```

## Read a row or a special cell

Simply using `sheet.getRow(rowIdx)` and `row.getCell(columnIdx)` to get the row and special cell.

**NOTICE: the index starts at zero**.

If you want to specify the data type of this fill, you can using some function like `cell.getRawValue` or `cell.getNumericCellValue` or even `cell.toString`.

For more usage, you can just search `cell.getValue` in the IDEA.

![search in the IDEA](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20201111205646.png)

## Get the total row and column numbers

The row number can be obtained by `sheet.getLastRowNum`.

For one row, you can get the column number by `row.getLastCellNum`.

## Filter empty cells

In some cases, there are empty or null cells and we don't want them.

We can judge them by `cell.getStringCellValue.isEmpty`.

Hence, if we need to filter them:

```scala
val row = sheet.getRow(rowIdx)
val columnNum = row.getLastCellNum
val nonemptyCells = Range(0, columnNum).map(x => row.getCell(x)).filter(x => !x.getStringCellValue.isEmpty)
```

