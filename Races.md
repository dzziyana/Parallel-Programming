`Approaches to avoiding clashes in memory:
## - **Immutability**
## - **Isolated mutability**
Data can change but only one thread can access them
## - **Mutable/shared data**
Data can change: all tasks/threads can potentially access them
...concurrent accesses may lead to inconsistencies...
Solution: allow only one task/thread to access **at a time** (thereby protect state)
### Protection of state
- exclusive access
- intermediate inconsistent states should not be observed
#### **Methods**:
##### #Locks 
Mechanism to ensure exclusive access / atomicity
##### #TransactionalMemory
Programmer describes a set of actions that need to be #atomic ⚛

### The problem of #Interleaving :
Second call starts before first ends
- happens even with one processor since a thread can be pre-empted at any point
- **Common wrong fix**: rearranging or repeating operations
```
void withdraw(int amount) {
  if(amount > getBalance())
//maybe balance changed
throw new WithdrawTooLargeException(); 
// maybe balance changed
setBalance(**getBalance()** – amount);
}
```

## [[Locks]]
#CriticalSection 
It is reasonable to have one lock being used by multiple methods belonging to one class 
But what about external classes? Eternal blocks may occur if same lock used for a public and class-internal method because the method will block trying to acquire a lock it already has.
One approach is to separate critical code into a public and a private method
### #Reentrant Locks
aka #RecursiveLocks
...means that the locks owner can obtain a lock multiple times **but then must release it equally often for lock to be freed**
- remembers the **thread that currently holds it**
- and a **count** that gets set to 0 whenever the lock goes not-held -> held
- TRICKY: use [[try-catch-finally]] because exception doesn't guarantee release of lock
If code running in the current holder calls **acquire()**/**lock**():
- count++, no blocking occurs
On **release()/unlock**
- if count > 0 -> count--
- if 0, the lock becomes not-held

#### **synchronized(expression){ }** blocks in Java are an implementation of re-entrant locks.
1. _expression_ gets evaluated
2. blocks if necessary, acquires lock when it gets past the "{"
3. releases the lock at the matching "}" (even if control flow "interrupts" due to throw return etc.)
In Java, all objects have an internal lock, aka #intrinsicLock or monitor lock -> used to implement synchronized
**java.util.concurrent.locks.ReentrantLock** offers
```
 try{...}
 finally{...}
```
to avoid forgetting to release lock if there's an exception

## Race Conditions
Correctness of the system depends on the specific interleaving or ordering of operations executed by multiple processes.
data races $\neq$ bad interleavings, but both are instances of race condition bugs
### - #DataRace aka [Low Level Race Condition]
Erroneous program behavior caused by insufficiently synchronized accesses of a shared resource by multiple threads, e.g. **Simultaneous read/write or write/write** of the same memory location 
=> **always an error**, due to compiler & Hardware
### - #BadInterleaving aka [High Level Race Condition] , high semantic level
Erroneous program behavior caused by **an unfavorable execution order** of a multithreaded algorithm that makes use of **otherwise well synchronized resources.** 
-> "bad" depends on specification, problems occur in the **compiler**
>  Typically, problem is some intermediate state that “messes up” a concurrent thread that “sees” that state

If one were to implement a #Stack in a concurrent context, 
```java
E peek() {
	E ans = pop(); 
	push(ans);
	return ans;
}
```
would be wrong, even if push() and pop() are synchronized, due to the inconsistent intermediate state it creates
Invariant to start with: if there has been a push() and no pop(), then isEmpty() returns false...
![[races figure 1.png]]
Moreover, we want to maintain the LIFO order when returning values.
![[races figure 2.png]]
![[races figure 3.png]]

#### Number of interleavings
#Slides-Lecture-15 
**Assuming 2 threads and $k$ statements each:
+ The merged list has length $k + k = 2k$
+ Once we know which of the $2k$ positions in the merged list are from thread 1, the interleaving is determined. 
$\Longrightarrow$ sampling without replacement (draw $k$ positions of $2k$ total)
$\Longrightarrow$ $\left ( \matrix{2k \\ k} \right) = \mathcal{O} \left( \frac{4^k}{\sqrt{2k}} \right)$

E ans = pop(); 
push(ans);
return ans;### Memory-location principles
#### 1. Thread-local
Whenever possible, do not share resources
Achievable through:
- Copies (but only when threads do not need to communicate through the resource)
- #CallStacks are thread-local => never need to synchronize on local variables
#### 2. Immutable 
Just do not write to the location. Make new objects instead of updating.
- Utilized by functional programming a lot.<- Helpful to avoid **side-effects**
- Simultaneous _reads_ are never races!
#### 3. Synchronized 
Control access through synchronization

Good practice: A lock "guards" each critical location
> Have a lock that is always held when reading or writing the location

The same lock can (and often should) #guard multiple locations 
=> Clearly document the guard for each location. In Java, often the guard is the object containing the location - ***this*** inside the object’s methods. But also often guard a larger structure with one lock to ensure mutual exclusion on the structure.

- Consistent locking
- [[Locking Strategies]]
