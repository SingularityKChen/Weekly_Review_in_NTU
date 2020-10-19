---
layout: post
title: "[Tutorial] Layout Two Graphs at One Main Graph"
description: "How to layout two graphs at one main graph in Latex"
categories: [Tutorial]
tags: [Latex]
last_updated: 2020-10-19 14:02:00 GMT+8
excerpt: "Using column width to fit the image width"
redirect_from:
  - /2020/10/17/
---

* Kramdown table of contents
{:toc .toc}
# Layout Two Graphs at One Main Graph

Here is one way to layout two graphs at one line.

```latex
\usepackage{graphicx}
\usepackage{float} 
\usepackage{subfigure}

\begin{figure}[ht]
    \centering
    \subfigure[A]{
      \label{fig:A}
      \begin{minipage}[b]{0.45\columnwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{img/a.png}
      \end{minipage}%
    }
    \subfigure[B]{
      \label{fig:BoxBrute}
      \begin{minipage}[b]{0.45\columnwidth}
        \centering
        \includegraphics[width=0.9\textwidth]{img/b.png}
      \end{minipage}%
    }
    \caption{Main Name}
    \label{fig:Main}
\end{figure}
```

I tried this in my two-column layout. I tried `0.48\textwidth` firstly but failed, then the `\columnwidth` helps. If you want three or more graphs in one row, the just simply change the factor of `\columnwidth`.