### Progress Assumptions
So far, following assumption
> If a thread enters CS, it will leave it again in finite time

...unrealistic because threads can run into exceptions or be killed by OS while in critical section or even in a lock.
#### Non-blocking
= without using progress assumptions or locks

> [!Definition]
> Failure or suspension of one thread must **never** cause failure/suspension of another thread.

**never = also not in CS** $\rightarrow$ progress assumptions must be dropped $\rightarrow$ locks are unusable
##### $\Rightarrow$ Non-blocking (lock- and wait-free) algorithms *never* use locks
##### Lock-free
>[!Definition] 
>In all schedulings, at least one thread is making progress $\Rightarrow$ **Guaranteed system-wide progress**

##### Wait-free
- In all schedulings, every thread is eventually making progress $\Rightarrow$ **Guaranteed per-thread progress** (no thread's progress depends on the state of another thread)
- Does not mean these threads may never wait - as long as it's for a finite time
#### Blocking
##### [[Deadlock]] free
- under the progress assumptions, all schedulings allow **at least one thread** to eventually enter the CS 
	$\Rightarrow$ no global deadlock
##### [[Starvation]] free
- under the progress assumptions, all schedulings **all threads** to eventually enter the CS
	$\Rightarrow$ no starvation, no local dead- or livelocks
#### Non-blocking
#Slides-Lecture-19 
> [!Important]
> Failure / Suspension of one thread cannot cause failure / suspension of other thread!
### Progress Conditions
![[Progress Conditions.png]]

