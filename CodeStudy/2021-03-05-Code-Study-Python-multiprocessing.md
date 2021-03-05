---
layout: post
title: "[CodeStudy] Python Multiprocessing with Return Values Using Pool"
description: "This post introduces multiprocessing in Python with return values from the child processing using Pool class."
categories: [CodeStudy]
tags: [Python, Multiprocessing]
last_updated: 2021-03-05 14:57:00 GMT+8
excerpt: "This post introduces multiprocessing in Python with return values from the child processing using Pool class."
redirect_from:
  - /2021/03/05/
---

* Kramdown table of contents
{:toc .toc}
# Python Multiprocessing with Return Values Using Pool

In my course assignment, there is a function to process several independent text chucks and return the terms with the document id. Firstly I just processed these chucks sequentially, then I thought I could processing them parallelly.

## Multiprocessing using pool

In Python, you can use `Process` class to get child process, but seems you need to manage them manually.

In my case, there is a class called `Pool` which represents a pool of worker processes and I can simply manage the child process and receive the return values.

```python
from multiprocessing import Pool
import time
def test(p):
       print(p)
       time.sleep(3)
if __name__=="__main__":
    pool = Pool(processes=2)
    for i in range(500):
        pool.apply_async(func=test, args=(i,))   
    print('test')
    pool.close()
    pool.join()
```

The method `apply_async` will help you to generate many child processings until the pool is full. And once any process in the pool finished, the new process will start until the for loop ends.

The father processing will continue until it meets `pool.join()`. Before you call `pool.join()`, you're supposed to call `pool.close()` to indicate that there will be no new processing.

## Sub-processings with return values

The `pool.apply_async` will return the sub-processing's value if any. But you need to get the value after the processing finish using `.get()` method.

```python
# make a single worker sleep for 10 secs
res = pool.apply_async(time.sleep, (10,))
try:
  print(res.get(timeout=1))
except TimeoutError:
  print("We lacked patience and got a multiprocessing.TimeoutError")
```

Also pay attention that if you call the `pool.apply_async` in a for loop, you're supposed to call `.get()` the final results outside the for loop otherwise the process will wait for return values thus slow the father process.

## Speedup

In my case, the sequential one takes 100252ms while the 8-processing one takes 16060ms, over 6 times speed up, as there are also some external sequential function after this parallel function.