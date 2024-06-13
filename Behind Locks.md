Implementation of Mutual Exclusion
## Assumptions
- Reads and writes of primitive type variables are atomic
- No reordering of read and write sequences (not true in practice!)
- Threads calling lock (**entering a critical section**) will eventually call unlock (**leave the critical section**).
Otherwise we assume a multithreaded environment where processes can arbitrarily interleave.
=> Absolutely **no assumptions** about threads' behavior **outside of critical sections**
## Critical section - the proper definition
Code with following conditions...
1. **Mutual exclusion**
Statements from critical sections of two or more processes (threads) **must not be interleaved**
2. **Freedom from [[Deadlock]]**
If *some* processes are trying to enter a critical section then *one of them* must eventually succeed
3. **Freedom from [[Starvation]]**
If *any* process (thread) tries to enter its critical section, then that process must eventually succeed

#IRQ s = interrupt requests. hardware signal sent to the processor that temporarily stops a running program and allows a special program, an [interrupt handler](https://en.wikipedia.org/wiki/Interrupt_handler "Interrupt handler"), to run instead.
... can be deactivated and switched back on with the critical section in-between in order to implement critical sections on a single core system.

#TODO
Mutual exclusion for 2 processes.
LiveLock vs DeadLock : same effect
#### Weakening Atomic Registers - "the safe SWMR register"
Single Writer Muliple Reader: **only 1 concurrent write**, but multiple concurrent reads allowed
Safe Register - any read **not concurrent with a write** returns the current value of r
any read concurrent with a write can return any value of the domain of r


