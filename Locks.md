
**Wikipedia**
a **lock** is a [synchronization primitive](https://en.wikipedia.org/wiki/Synchronization_primitive "Synchronization primitive") that prevents state from being modified or accessed by multiple [threads of execution](https://en.wikipedia.org/wiki/Threads_(computer_science) "Threads (computer science)") at once.

## List of lock types
[[Backoff Lock]]
[[TATAS Lock]]
[[Spinlock]]
[[Behind Locks]]
[[Decker's Algorithm]] (Lock)
[[Peterson Lock]]
[[Filter Lock]]
[[Reader-Writer Lock]]

## Locking strategies
See [[Locking Strategies]]

# Anatomy of a lock
The execution of a `lock()` call is divided into **doorway section** and **waiting section**.
### **Doorway**
The part of the locking method that contains **a finite amount of instructions** that is guaranteed to finish within **a finite amount of time**
$\rightarrow$ e.g. the raise of a flag
### **Waiting Section**
The part that comes after the doorway section and that contains an unbounded number of instructions 
$\rightarrow$ e.g. a spin wait  (cf. [[Spinlock]])

# Characterizing locks
## Fairness

> [!Definition]
> a lock is **fair**, if for two processes _A_ and _B_, whenever _A_ finishes its doorway before _B_ starts its doorway, _A_ cannot be overtaken by _B_ :

![[Lock fairness formal notation.png]]
#TODO *Legend for the notation. What do the variables in the exponent (j and k) exactly signify? Insert a reference to the part of the study material where this notation is introduced.*


### A lock is fair if it **fulfills FIFO order**. 
> *The FIFO order is fulfilled...*
> When a thread that finishes the doorway section before some other threads have started their doorway sections is ***guaranteed to acquire the lock first.*** 
## [[Starvation]] free
A lock is defined as starvation free if and only if when one or more threads are contesting for it, **all of them** are guaranteed to acquire it within a finite amount of time.
## [[Deadlock]] free
A lock is defined as deadlock free if and only if when one or more threads are contesting for it, **(at least) one** of them is guaranteed to acquire it within a finite amount of time.

## Mutual exclusion

# Performance
### Uncontended Case
+ Threads do not compete for the lock
+ locks try to have minimal overhead
+ typically "just" the cost of an atomic operation
### Contended Case
+ threads do compete for the lock
+ can lead to significant performance degradation
+ starvation can occur
+ there are locks that try to optimize for this case
# Disadvantages
### General
+ **Pessimistic by design** - Mutual exclusion always enforced
+ [[#Performance]]  
+ **Ahmdal's law** - enforcing mutual exclusion makes this part of the code sequential!
+ **Delays in the CS** -> Affect all threads
+ **Death of a thread in the CS** -> No progress at all
+ **Deadlocks / Livelocks**
+ Can not be used in **interrupt handlers** without precautions
**Further Problems**: [[Transactional Memory#Problems with locking]]
### Spinlocks
+ missing FIFO behaviour
+ Computing resources wasted / performance degraded especially for long lived contention
+ no notification mechanism
### Locks with waiting / scheduling
+ Require support from runtime system (OS / scheduler)
+ Data structures for scheduled locks need to be protected against concurrent access
	+ again with spinlocks OR
	+ using lock free datastructures
+ higher wakeup latency (due to scheduler)
=> hybrid solultions


All of these are motivation for [[Lock Free Programming]]



# Guide for Condition waits
> [!Important]-  
> 
> + Always have a condition predicate
> + Always test the condition predicate:
> 	- before calling wait
> 	- after returning from wait
>  + Always call wait in a loop
>  + Ensure state is protected by lock associated with condition

