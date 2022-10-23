---
layout: post
title: "[Glean] Latex includegraphics decodearray"
description: "The use of the decodearray option to includegraphics allows the rendering colors to be changed."
categories: [Glean]
tags: [Latex]
last_updated: 2022-10-23 19:31:00 GMT+8
excerpt: "The use of the decodearray option to includegraphics allows the rendering colors to be changed."
redirect_from:
  - /2022/10/23/
---

* Kramdown table of contents
{:toc .toc}

# Latex includegraphics decodearray

The use of the `decodearray` option to `\includegraphics` allows the rendering colors to be changed.

The `decodearray` takes 6 parameters which is brightness, whiteness, and four 
colors in CMYK color model, i.e., cyan, magenta, yellow, and key (black). 

For instance, if you want a gray graph:

```latex
\includegraphics[decodearray={0 1 .5 .5 .5 .5}]{your_graph.jpg}
```

where `0` means no changes to the brightness, `1` means keeping the whiteness of all the colors, the four `0.5` means gradations of four colors from full to half saturation. Hence the four `0.5` make the graph to be gray.
