---
layout: post
title: "[Tutorial] Concatenate a List of `.flv` Files into a Single `.mp4` File"
description: "This tutorial introduces how to concatenate a list of `.flv` files into a single `.mp4` file using `ffmpeg`. And if the video and audio codecs are different, how to re-encode the video stream with the `libx264` codec."
categories: [Tutorial]
tags: [FFmpeg]
last_updated: 2024-08-14 12:45:00 GMT+8
excerpt: "This tutorial introduces how to concatenate a list of `.flv` files into a single `.mp4` file using `ffmpeg`. And if the video and audio codecs are different, how to re-encode the video stream with the `libx264` codec."
redirect_from:
  - /2024/08/14/
---

* Kramdown table of contents
{:toc .toc}

# Concat A List of `.flv` Files into a Single `.mp4` File

```bash
ffmpeg -f concat -safe 0 -i <(for f in ./*.flv; do echo "file '$PWD/$f'"; done) -c copy output.mp4
```

Or Create a `.txt` File to List the `.flv` Files

```bash
for f in ./*.flv; do echo "file '$PWD/$f'" >> filelist.txt; done
ffmpeg -f concat -safe 0 -i filelist.txt -c copy -threads 0 output.mp4
```

# If the Video and Audio Codecs are Different

If the Video and Audio Codecs are Different, Re-encode the Video Stream with `libx264` Codec:
```bash
ffmpeg -f concat -safe 0 -i filelist.txt -c:v libx264 -threads 0 output.mp4
```

