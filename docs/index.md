### Features (v0.92)

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

### Installation

Can be installed via `pip install yappi` or from the source directly.

### Documentation

Please see [API reference manual][]. For how to use the profiler and how
to interpret statistics output, see [usage reference manual]. And
finally, see [requirements and techinal] section for why we have
implemented yet another python profiler and some technical info about
the internals.

### Download

Latest version can be downloaded from [1][] or from PyPI [2][] directly.

### Development

Latest development sources can be found [3][]

[callgrind]: http://valgrind.org/docs/manual/cl-format.html
[pstat]: http://docs.python.org/3.4/library/profile.html#pstats.Stats
[CPU time]: http://en.wikipedia.org/wiki/CPU_time
[API reference manual]: http://code.google.com/p/yappi/wiki/apiyappi_v092
[usage reference manual]: http://code.google.com/p/yappi/wiki/usageyappi_v092
[requirements and techinal]: http://code.google.com/p/yappi/wiki/whyyappi
[1]: http://bitbucket.org/sumerc/yappi/downloads
[2]: http://pypi.python.org
[3]: http://bitbucket.org/sumerc/yappi/