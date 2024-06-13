# #SpeedUp on P processors
## $\frac{T_1}{T_P}$
### Perfect linear speedup $\leftrightarrow$ if speedup = P
### #Parallelism is the speedup over $T_\infty$ ( #span ) i.e. maximum possible speedup

# Expected time:
## $T_P = O((\frac{T_1}{P})+T_\infty)$

# [[Amdahl's Law]]
## $T_{P} \geq W_{ser} + \frac{W_{par}}{P}$

## $S_{P} \leq \frac{W_{ser} + W_{par}}{W_{ser} + \frac{W_{par}}{P}}$

# [[Gustafson's Law]]

## $S_{P} = f + p(1-f) = p - f(p-1)$

**f : serial fraction.**