##### Wikipedia
a **spinlock** is a [lock](https://en.wikipedia.org/wiki/Lock_(computer_science) "Lock (computer science)") that causes a [thread](https://en.wikipedia.org/wiki/Thread_(computer_science) "Thread (computer science)") trying to acquire it to simply wait in a [loop](https://en.wikipedia.org/wiki/While_loop "While loop") ("spin") while repeatedly checking whether the lock is available. Since the thread remains active but is not performing a useful task, the use of such a lock is a kind of [busy waiting](https://en.wikipedia.org/wiki/Busy_waiting "Busy waiting"). Once acquired, spinlocks will usually be held until they are explicitly released, although in some implementations they may be automatically released if the thread being waited on (the one that holds the lock) blocks or "goes to sleep".
Because they avoid overhead from OS [process rescheduling](https://en.wikipedia.org/wiki/Scheduling_(computing) "Scheduling (computing)") or [context switching](https://en.wikipedia.org/wiki/Context_switch "Context switch"), spinlocks are efficient if threads are likely to be blocked for only short periods
# Implementation using simple atomic operations
[[TAS and CAS]]

### Test and Set 
**Init  (lock)**
lock = 0;
**Acquire (lock)**
while !TAS(lock); //wait
**Release (lock)**
lock = 0;

## Compare and Swap 
**Init (lock)**
lock = 0;
**Acquire (lock)**
while (CAS(lock,0,1) != 0);
**Release (lock)**
CAS (lock, 1, 0);

# Performance measurement 
**Sequential bottleneck** : threads fight for the memory bus during call of `getAndSet()` $\Rightarrow$ #Contention 
- **Cache coherency protocol** invalidates cached copies of the lock variable on other processors 