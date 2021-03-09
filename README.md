# High_Performance_Python

## Timing with cProfile

cProfile is a built-in profiling tool in the standard Python library. It introduces a greater overhead, but
it provides additional information on each aspect of your code and where the most time is being spent. 

```commandline
$ python -m cProfile -s cumulative julia1.py
```
```
Length of x: 1000
Total elements: 1000000
calculate_z_serial_purepythontook  9.52362608909607  seconds
         36221995 function calls in 10.267 seconds

   Ordered by: cumulative time

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000   10.267   10.267 {built-in method builtins.exec}
        1    0.032    0.032   10.267   10.267 julia1.py:1(<module>)
        1    0.573    0.573   10.234   10.234 julia1.py:9(calc_pure_python)
        1    7.369    7.369    9.524    9.524 julia1.py:43(calculate_z_serial_purepython)
 34219980    2.155    0.000    2.155    0.000 {built-in method builtins.abs}
  2002000    0.129    0.000    0.129    0.000 {method 'append' of 'list' objects}
        1    0.008    0.008    0.008    0.008 {built-in method builtins.sum}
        3    0.000    0.000    0.000    0.000 {built-in method builtins.print}
        4    0.000    0.000    0.000    0.000 {built-in method builtins.len}
        2    0.000    0.000    0.000    0.000 {built-in method time.time}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

Outside of the returned stats, we can generate a statistics file to be analyzed and visualized.

```commandline
$ python -m cProfile -o profile.stats julia1.py
```

This can be referenced in Python using the pstats library. 

```python
import pstats
p = pstats.Stats("profile.stats")
p.sort_stats("cumulative")
p.print_stats() # returns same message as above
```