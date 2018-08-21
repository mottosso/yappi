\#summary yappi clock types

Clock Types {#Clock Types}
===========

Currently, yappi supports two basic clock types used for calculating the
timing data:

- [CPU Clock](https://en.wikipedia.org/wiki/CPU_time)
- [Wall Clock](https://en.wikipedia.org/wiki/Wall_time)

Clock types of yappi can be change via calls to
\_yappi.set\_clock*type()* API call.

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

And as *time.sleep()* is a blocking function(which means it actually block the calling thread, thread usually sleeps in the OS queue), the CPU clock cannot accumulate any timing data for the function a. In fact, there may be some very few CPU cycles involved before actually calling the *time.sleep*, however that level of precision is not shown at all.

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

So, as can be seen, now *time.sleep* blocking call gets into account. Let's add a piece of code that actually burns CPU cycles:

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
