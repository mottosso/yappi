### yappi Reference Manual (v0.92) =

```python
yappi.start(builtins=False, profile_threads=True)
```

Starts profiling all threads in the current interpreter instance. This function can be called from any thread at any time. Resumes profiling if stop() is called previously.

Current parameters:

| Param    | Description
|:---------|:---------
| builtins | Profile builtin functions used by standart Python modules. It is _False_ by _default_.
| profile_threads |  Profile all of the threads if 'true', else, profile only the calling thread. |

```python
yappi.stop()
```

Stops the currently running yappi instance. Same profiling session might be resumed later by calling start().


```python
yappi.clear_stats()
```

Clears the profiler results. The results stays in memory unless application(all threads including the main thread) exists or clear_stats() is called.


```python
yappi.get_func_stats()
```

Returns the function stats as [https://code.google.com/p/yappi/wiki/YFuncStats_v092 YFuncStats]  object.


```python
yappi.get_thread_stats()
```

Returns the thread stats as [https://code.google.com/p/yappi/wiki/YThreadStats_v092 YThreadStats]  object.


```python
yappi.is_running()
```

Returns a boolean indicating whether profiler is running or not.


```python
yappi.get_clock_type()
```

Returns information about the underlying clock type Yappi uses to measure timing.
Return values can either be <i>CPU</i> or <i>Wall</i>.

```python
yappi.set_clock_type(type)
```

Sets the underlying clock type. _type_ can be following: 

|| *Clock Type* || *Description* ||
|| Wall || [http://en.wikipedia.org/wiki/Wall_time Details] ||
|| CPU || [http://en.wikipedia.org/wiki/CPU_time Details] ||

```python
yappi.get_clock_time()
```

Returns the current clock time with regard to the underlying clock type.

```python
yappi.get_clock_info()
```

Returns a dict containing the OS API used for timing, the precision of the underlying clock.

```python
'''
Example on Windows:
'''
>>> import yappi
>>> yappi.get_clock_info()
{'resolution': '100ns', 'api': 'getthreadtimes'}
>>>
```

```python
yappi.get_mem_usage()
```

Returns the internal memory usage of the profiler itself.

```python
yappi.shift_context_time(context_id, amount)
```

Adjust a context's start time, and the time of all functions currently on
the context's stack. 'amount' is in the same units as get_clock_type(). A
negative 'amount' increases the 'ttot' statistic for this context and all
functions on the stack, and a positive 'amount' decreases 'ttot'.

```python
yappi.set_context_id_callback(callback)
```

Use a num