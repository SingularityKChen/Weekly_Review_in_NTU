---
layout: post
title: "[Glean] A better way to apply subfloat"
description: "Simply wrapper the includegraphics with makebox to adjust the width of the caption and image separately."
categories: [Glean]
tags: [Latex, Subfloat]
last_updated: 2022-05-15 16:52:00 GMT+8
excerpt: "Simply wrapper the includegraphics with makebox to adjust the width of the caption and image separately."
redirect_from:
  - /2022/05/15/
---

* Kramdown table of contents
{:toc .toc}
# `subfloat` Overview

By using `subfloat`, you're able to get an image with x-row-y-column sub-images.

# A better way to apply `subfloat`

There are some occasions that the sub-figures should be smaller but the caption should won a wider line width, i.e., adjust the width of the caption and image separately.

![](https://latex.org/forum/download/file.php?id=9913)

In this case, we could simply wrapper the `\includegraphics` with `\makebox`.

```tex
\begin{figure}[!t]
    \centering
    \captionsetup[subfigure]{width=0.45\columnwidth}
    \subfloat[Long sub captions here. Long sub captions here. ]{
        \makebox[0.48\columnwidth][c] {
            \includegraphics[width=0.3\columnwidth]{1.png}
            \label{fig:1}
        }
    }
    \subfloat[Long sub captions here. Long sub captions here. ]{
        \makebox[0.48\columnwidth][c] {
            \includegraphics[width=0.35\columnwidth]{2.png}
            \label{fig:2}
        }
    }
    \caption{Long Subcaptions with smaller image.}
    \label{fig:1-2}
\end{figure}
```

Then you can set the width of your caption in `\makebox[your_caption_width]` and the width of your image in `\includegraphics[width=your_img_width]`.

