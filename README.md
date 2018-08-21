<p align=center><img width=300 src=https://user-images.githubusercontent.com/2152766/44388275-9fe5b200-a51f-11e8-80b0-a9a1c2f7174d.png></p>

<p align=center>A cProfile alternative for long-running multi-threaded applications<br><i>Cloned from <a href=https://bitbucket.org/sumerc/yappi>bitbucket.org/sumerc/yappi</a>, <a href=https://bitbucket.org/sumerc/yappi/commits/bf48b53eb3ce51a21e58b5223f10b19e1c3351c9?at=default>commit</a></i></p>.

**Changes**

No functional changes, just a few to presentation and documentation.

- Moved (hidden) documentation into this README
- Moved source code into `/src`, for more visibility of README

**Table of contents**

- [Installation](#installation)
  - [Limitations](#limitations)
  - [Features](#features)
- [Usage](#usage)
  - [Clock Types](#clock-types)

<br>

### Install

Can be installed via `pip install yappi` or from the source directly.

<br>

### Features

- Ability to hook underlying threading model events/properties. (*new in 0.92*)
- Decorator to profile individual functions easily. (*new in 0.92*)
- Profiler results can be saved in [callgrind][] and [pstat][] formats. (*new in 0.82*) 
- Profiler results can be merged from different sessions on-the-fly. (*new in 0.82*)
- Profiler results can be easily converted to pstats. (*new in 0.82*) 
- Profiling of multithreaded Python applications transparently. 
- Supports profiling per-thread [CPU time][] (*new in 0.62*)
- Profiler can be started from any thread at any time.
- Ability to get statistics at any time without even stopping the profiler.
- Various flags to arrange/sort profiler results.
- Supports Python 2.6.x <= x <= Python 3.4
	
##### Limitations

- Threads must be derived from "threading" module's Thread object
- Latest version of Yappi supports Python 2.6.x <= x <= Python.3

[callgrind]: http://valgrind.org/docs/manual/cl-format.html
[pstat]: http://docs.python.org/3.4/library/profile.html#pstats.Stats
[CPU time]: http://en.wikipedia.org/wiki/CPU_time

<br>

### Usage

A typical example on profiling with `yappi`, includes at least 3 lines of code:

```python
import yappi

def a(): 
    for i in range(10000000): pass

yappi.start()
a()
yappi.get_func_stats().print_all()
yappi.get_thread_stats().print_all()
```

And the output of running above script:

```bash
Clock type: cpu
Ordered by: totaltime, desc

name                                    #n         tsub      ttot      tavg
deneme.py:35 a                          1          0.296402  0.296402  0.296402

name           tid              ttot      scnt
_MainThread    6016             0.296402  1
```

Let's inspect the results in detail. So, first line:

```bash
Clock type: cpu
```

This indicates the profiling timing stats shown are retrieved using the CPU clock. That means the actual CPU time spent in the function is shown. Yappi provides two modes of operation: CPU and Wall time profiling. You can change the setting by a call to
`yappi.set_clock_type()` API.

Second is:

```bash
Ordered by: totaltime, desc
```

It is obvious. It shows the sort order and sort key of the shown
profiling stats. You can see the valid values for this in
*YFuncStats().sort()* API.

Ok, now we actually see the statistic of the function `a()`:

```bash
name                                    #n         tsub      ttot      tavg
deneme.py:35 a                          1          0.296402  0.296402  0.296402
```

Let's explain the fields in detail:

| Title   | Description
|:--------|:--------------
| `name`  | the full unique name of the called function. 
| `n`     | how many times this function is called. 
| `tsub`  | how many time this function has spent in total, subcalls excluded. See [Clock Types](#clock-types) 
| `ttot`  | how many time this function has spent in total, subcalls included. See [Clock Types](#clock-types) 
| `tavg`  | how many time this function has spent in average, subcalls included. See [Clock Types](#clock-types)

The next lines shows the thread stats. So, let see:

```bash
name           tid              ttot      scnt
_MainThread    6016             0.296402  1
```

| Title   | Description
|:--------|:------------
| `name`  | the class name of the Thread. (This is the name of the class inherits the threading.Thread class) 
| `tid`   | the thread id.
| `ttot`  | how many time this thread has spent in total.
| `scnt`  | how many times this thread is scheduled.

<br>

#### Clock Types

Currently, `yappi` supports two basic clock types used for calculating the
timing data:

- [CPU Clock](https://en.wikipedia.org/wiki/CPU_time)
- [Wall Clock](https://en.wikipedia.org/wiki/Wall_time)

Clock types of yappi can be change via calls to `yappi.set_clock_type()`.

Let's explain the concept via an example as I believe it is the best way to explain something:

Suppose following code:

```python
import yappi
import time

def a():
    time.sleep(4.0)

yappi.start()
a()
yappi.get_func_stats().print_all()   
```

It prints put following:

```bash
Clock type: cpu
Ordered by: totaltime, desc

name                                    #n         tsub      ttot      tavg
deneme.py:35 a                          1          0.000000  0.000000  0.000000
```

So, what happened? The answer is that yappi supports CPU clock by
default for timing calculations as can be seen in the output:

```bash
Clock type: cpu
```

And as `time.sleep()` is a blocking function(which means it actually block the calling thread, thread usually sleeps in the OS queue), the CPU clock cannot accumulate any timing data for the function a. In fact, there may be some very few CPU cycles involved before actually calling the `time.sleep()`, however that level of precision is not shown at all.

Let's see what happens when change the clock\_type to Wall Clock:

```python
yappi.clear_stats()
yappi.set_clock_type("wall")
yappi.start()
a()
yappi.get_func_stats().print_all()   
```

Output for above is:

```bash
Clock type: wall
Ordered by: totaltime, desc

name                                    #n         tsub      ttot      tavg
deneme.py:4 a                           1          0.000006  3.999587  3.999587
```

So, as can be seen, now `time.sleep()` blocking call gets into account. Let's add a piece of code that actually burns CPU cycles:

```python
import yappi
import time

def a():
    for i in range(10000000): pass

yappi.set_clock_type("cpu")
yappi.start()
a()
stats = yappi.get_func_stats()
stats.print_all()
```

When you run the above script, you actually get:

```bash
Clock type: cpu
Ordered by: totaltime, desc

name                                    #n         tsub      ttot      tavg
deneme.py:4 a                           1          0.280802  0.280802  0.280802
```

Note that the values actually may differ from computer to computer as CPU clock rates may differ significantly. Yappi actually uses native OS APIs to retrieve per-thread CPU time information. You can see *timing.c* module in the repository for details.

So, briefly, it is up to you to decide with which mode of clock type you need to profile your application.

### Development

Latest development sources can be found [here](http://bitbucket.org/sumerc/yappi/).
