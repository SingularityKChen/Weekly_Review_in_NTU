---
layout: post
title: "[Glean] Package hyperref: Token not allowed"
description: "Package hyperref Warning: Token not allowed in a PDF string (PDFDocEncoding):(hyperref). Using texorpdfstring to solve this."
categories: [Glean]
tags: [Latex]
last_updated: 2022-05-15 16:31:00 GMT+8
excerpt: "Package hyperref Warning: Token not allowed in a PDF string (PDFDocEncoding):(hyperref). Using texorpdfstring to solve this."
redirect_from:
  - /2022/05/15/
---

* Kramdown table of contents
{:toc .toc}
# PDFDocEncoding Warning

When we're using `hyperref` to generate toc for the PDF file, we may witness this error if there are unsupported tokens in our titles:

```
Package hyperref Warning: Token not allowed in a PDF string (PDFDocEncoding):(hyperref)
```

The easiest solution for this is to use `\texorpdfstring{token in Tex}{string that shown in PDF toc}`.

For example:

```tex
\subsection{\texorpdfstring{$7 \times 7$}{7 × 7}, Stride 2}
```
