---
layout: post
title: "[CodeStudy] Python Matplotlib"
description: "Using Python Matplotlib to draw graph and plot it in a function."
categories: [CodeStudy]
tags: [Python, Matplotlib]
last_updated: 2021-01-25 14:45:00 GMT+8
excerpt: "Using Python Matplotlib to draw graph and plot it in a function."
redirect_from:
  - /2021/01/22/
---

* Kramdown table of contents
{:toc .toc}
# Python Matplotlib

## Basic Operations

```python
# import the lib
import matplotlib.pyplot as plt

# create your figure
my_fig = plt.figure()

# add your sub plot and draw graph here
# let's assume we need a 2 x 2 graph
my_sub_plot_1 = my_fig.add_subplot(2, 2, 1)
my_sub_plot_2 = my_fig.add_subplot(2, 2, 2)
my_sub_plot_3 = my_fig.add_subplot(2, 2, 3)
my_sub_plot_4 = my_fig.add_subplot(2, 2, 4)

# now plot the dots and lines
a = ...
b = ...
my_sub_plot_1.plot(a, b)
...

# then show the graph
plt.show()
```

You can change the number of sub plots, just modify the parameters in `add_subplot()`.

And you can also combine the subplot operation by:

```python
my_fig, (my_sub_plot_1, my_sub_plot_2, my_sub_plot_3, my_sub_plot_4 = plt.subplots(nrows=2, ncols=2)
```

Which I think is much better.

## Transfer Subplots into a function and plot it

If you want to send the graph to a function and return it to the main function after adding some details in the graph, you can not send the Figure object otherwise you'll meet this error:

```
AttributeError: 'Figure' object has no attribute 'plot'
```

In this case, you can not send the variable `my_fig` into the function, but you can send the sub plot into the function and return it.

```python
def my_func:(f_plot):
    a = ...
		b = ...
		f_plot.plot(a, b)
    return f_plot
```

However, I suggest just append the list out and plot them at one time, which is much faster in my case.

## Useful format to custom your lines

The syntax of `plot` is:

```python
plot([x], [y], [format], [x2], y2, [format2], ..., **kwargs)
```

Where the `format` including color, markers, line style. You can even add more parameters like

```python
plot(x,y2,color='green', marker='o', linestyle='dashed', linewidth=1, markersize=6)
plot(x,y3,color='#900302',marker='+',linestyle='-')
```

Here is a reference of characters[^1] that you can use like

```python
plot(x, y, 'bo-')
```



```python
**Markers**

=============    ===============================
character        description
=============    ===============================
``'.'``          point marker
``','``          pixel marker
``'o'``          circle marker
``'v'``          triangle_down marker
``'^'``          triangle_up marker
``'<'``          triangle_left marker
``'>'``          triangle_right marker
``'1'``          tri_down marker
``'2'``          tri_up marker
``'3'``          tri_left marker
``'4'``          tri_right marker
``'s'``          square marker
``'p'``          pentagon marker
``'*'``          star marker
``'h'``          hexagon1 marker
``'H'``          hexagon2 marker
``'+'``          plus marker
``'x'``          x marker
``'D'``          diamond marker
``'d'``          thin_diamond marker
``'|'``          vline marker
``'_'``          hline marker
=============    ===============================

**Line Styles**

=============    ===============================
character        description
=============    ===============================
``'-'``          solid line style
``'--'``         dashed line style
``'-.'``         dash-dot line style
``':'``          dotted line style
=============    ===============================

Example format strings::    

		'b'    # blue markers with default shape
    'or'   # red circles
    '-g'   # green solid line
    '--'   # dashed line with default color
    '^k:'  # black triangle_up markers connected by a dotted line

**Colors**

The supported color abbreviations are the single letter codes

=============    ===============================
character        color
=============    ===============================
``'b'``          blue
``'g'``          green
``'r'``          red
``'c'``          cyan
``'m'``          magenta
``'y'``          yellow
``'k'``          black
``'w'``          white
=============    ===============================
```

## Plot shapes and plot from Data Frames

For this part, you can see this post[^1].

[^1]: Introduction to Data Visualization With Matplotlib in Python, https://medium.com/swlh/introduction-to-data-visualization-with-matplotlib-in-python-e9fb25276829