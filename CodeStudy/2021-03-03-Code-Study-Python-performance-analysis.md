---
layout: post
title: "[CodeStudy] Python Performance Analysis"
description: ""
categories: [CodeStudy]
tags: [Python, Memory Profiler, cProfile]
last_updated: 2021-03-03 11:13:00 GMT+8
excerpt: "This blog introduces Python memory and execution time analysis tools Memory Profiler and cProfile."
redirect_from:
  - /2021/03/03/
---

* Kramdown table of contents
{:toc .toc}
# Python Performance Analysis

## Memory Analysis: Memory Profiler[^1]

This is a python module for monitoring memory consumption of a process as well as line-by-line analysis of memory consumption for python programs.

### line-by-line memory usage

```python
from memory_profiler import profile

@profile
def my_func():
    a = [1] * (10 ** 6)
    b = [2] * (2 * 10 ** 7)
    del b
    return a
```

Or you can also comment out the import and leave your functions decorated.

```bash
python -m memory_profiler example.py
```

### Time-based memory usage

Sometimes it is useful to have full memory usage reports as a function of time (not line-by-line) of external processes.

```bash
mprof run <executable>
mprof plot
```

In this case, you need to leave your functions decorated.

You can customize the plot settings:

Title:

```bash
mprof plot -t 'Recorded memory usage'
```

The available commands for mprof are:

> + `mprof run`: running an executable, recording memory usage
> + `mprof plot`: plotting one the recorded memory usage (by default, the last one)
> + `mprof list`: listing all recorded memory usage files in a user-friendly way.
> + `mprof clean`: removing all recorded memory usage files.
> + `mprof rm`: removing specific recorded memory usage files

### Reporting

```python
fp=open('memory_profiler.log','w+')
@profile(stream=fp)
def my_func():
    a = [1] * (10 ** 6)
    b = [2] * (2 * 10 ** 7)
    del b
    return a
```

Or use the logger module:

```python
from memory_profiler import LogFile
import sys
sys.stdout = LogFile('memory_profile_log')
```

## Time Analysis

Actually, I think the build-in PyCharm Profile function is more convenient.

### cProfile[^2][^3]


#### run()

The most basic starting point in the profile module is `run()`.

```python
if __name__ == "__main__":
    import cProfile
    # output to std out
    cProfile.run("test()")
    # output to the file
    cProfile.run("test()", filename="result.out")
    # output to the file with order
    cProfile.run("test()", filename="result.out", sort="cumulative")
```

#### runctx()

Sometimes, instead of constructing a complex expression for `run()`, it is easier to build a simple expression and pass it parameters through a context, using `runctx()`.

```python
import profile
from profile_fibonacci_memoized import fib, fib_seq

if __name__ == '__main__':
    profile.runctx('print fib_seq(n); print', globals(), {'n':20})
```

#### pstats: Saving and Working With Statistics

For example, to run several iterations of the same test and combine the results, you could do something like this:

```python
import profile
import pstats
from profile_fibonacci_memoized import fib, fib_seq

# Create 5 set of stats
filenames = []
for i in range(5):
    filename = 'profile_stats_%d.stats' % i
    profile.run('print %d, fib_seq(20)' % i, filename)

# Read all 5 stats files into a single object
stats = pstats.Stats('profile_stats_0.stats')
for i in range(1, 5):
    stats.add('profile_stats_%d.stats' % i)

# Clean up filenames for the report
stats.strip_dirs()

# Sort the statistics by the cumulative time spent in the function
stats.sort_stats('cumulative')

stats.print_stats()
```

The output report is sorted in descending order of cumulative time spent in the function and the directory names are removed from the printed filenames to conserve horizontal space.

What is shown here is a list of all of the function calls made by the program, along with statistics about each[^4]:

+ At the top, we see the total number of calls  made in the profiled program and the total execution time. You may also  see “primitive calls,” meaning *non-recursive* calls, or calls made directly to a function that don’t in turn call themselves further down in the call stack.
+ *`ncalls`*: Number of calls made. If you see two numbers  separated by a slash, the second number is the number of primitive calls for that function.
+ *`tottime`*: Total time spent in the function, *not* including calls to other functions.
+ *`percall`*: Average time per call for *`tottime`*, derived by taking *`tottime`* and dividing it by *`ncalls`*.
+ *`cumtime`*: Total time spent in the function, including calls to other functions.
+ *`percall`* (#2): Average time per call for *`cumtime`* (*`cumtime`* divided by *`ncalls`*).
+ *`filename:lineno`*: The file name, line number, and function name for the call in question.

#### Caller / Callee Graphs

**Stats** also includes methods for printing the callers and callees of functions.

```python
import profile
import pstats
from profile_fibonacci_memoized import fib, fib_seq

# Read all 5 stats files into a single object
stats = pstats.Stats('profile_stats_0.stats')
for i in range(1, 5):
    stats.add('profile_stats_%d.stats' % i)
stats.strip_dirs()
stats.sort_stats('cumulative')

print 'INCOMING CALLERS:'
stats.print_callers('\(fib')

print 'OUTGOING CALLEES:'
stats.print_callees('\(fib')
```

The arguments to `print_callers()` and `print_callees()` work the same as the restriction arguments to `print_stats()`.  The output shows the caller, callee, and cumulative time.

To illustrate the results[^2],

```bash
python gprof2dot.py -f pstats result.out | dot -Tpng -o result.png
```

### My case

```python
profiler.enable()
dic = some_class.some_func
profiler.disable()
stats = pstats.Stats(profiler)
stats.sort_stats("cumulative")
stats.print_stats()
stats.print_title()
stats.print_callers()
stats.print_callees()
stats.dump_stats(filename)
```


[^1]: https://github.com/pythonprofilers/memory_profiler
[^2]: https://blog.csdn.net/asukasmallriver/article/details/74356771
[^3]: https://pymotw.com/2/profile/
[^4]: https://www.infoworld.com/article/3329750/how-to-profile-python-code.html