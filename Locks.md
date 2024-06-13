**Wikipedia**
a **lock** is a [synchronization primitive](https://en.wikipedia.org/wiki/Synchronization_primitive "Synchronization primitive") that prevents state from being modified or accessed by multiple [threads of execution](https://en.wikipedia.org/wiki/Threads_(computer_science) "Threads (computer science)") at once.

List:
[[Backoff Lock]]
[[TATAS Lock]]
[[Spinlock]]
[[Behind Locks]]
[[Decker's Algorithm]] (Lock)
[[Peterson Lock]]

Anatomy of a lock:
- **Doorway**
The part of the locking method that contains a finite amount of instructions that is guaranteed to finish within a finite amount of time (such as the raise of a flag)
- **Waiting Section**
e part that comes after the doorway section and that contains an unbounded number of instructions (e.g. a spin wait -> [[Spinlock]])
# Fair
a lock is **fair**, if for two processes _A_ and _B_, whenever _A_ finishes its doorway before _B_ starts its doorway, _A_ cannot be overtaken by _B_:
![[Pasted image 20240611192134.png]]
A lock is fair if it **fulfills FIFO order**. The FIFO order is fulfilled when a thread that finishes the doorway section before some other threads have started their doorway sections is guaranteed to acquire the lock first. Recall that the execution of a "lock()" call is divided into **doorway section** and **waiting section**, where the former is defined as the part of the locking method that contains a finite amount of instructions that is guaranteed to finish within a finite amount of time (such as the raise of a flag), and the latter is defined as the part that comes after the doorway section and that contains an unbounded number of instructions (e.g. a spin wait).
# [[Starvation]] free
A lock is defined as starvation free if and only if when one or more threads are contesting for it, **all of them** are guaranteed to acquire it within a finite amount of time.
# [[Deadlock]] free
A lock is defined as deadlock free if and only if when one or more threads are contesting for it, **(at least) one** of them is guaranteed to acquire it within a finite amount of time.

- Software locks: read and write are atomic
- "writes" visible to all threads (no memory reordering)


