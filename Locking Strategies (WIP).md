#Slides-Lecture-19 
# General


>[!Warning]
> When aquiring **multiple locks**, define an **order** on the objects you are trying to lock so that objects are always locked in the same order!
> 
> **Erronous** example:
> ```java
> public static void doSomething(Object a, Object b) {
> 	synchronized(a) {
> 		synchronized(b) {
> 			something(a, b);
> 		}
> 	}
> }
> ```
> **Correct**:
> ```java
> public static void doSomething(Object a, Object b) {
> 	Object first = a;
> 	Object second = b;
> 	if (a.compareTo(b) > 0) {
> 		first = b;
> 		second = a;
> 	 }
> 	 synchronized(first) {
> 		 synchronized(second) {
> 			 something(a, b);
> 		 }
> 	 }
> }
> ```
> 
> 

> [!Tip]
> You can just generate an order by using unique IDs for your objects.
> ```java
> class BankAccount {
>	private static final AtomicLong counter = new AtomicLong();
>	private final long index = counter.incrementAndGet();
>	...
>	void transferTo(int amount, BankAccount to) {
>		if (to.index < this.index)
>		...
>	}
> }


# Lock granularity
**Coarse-grained**: fewer locks with more objects per lock
- simple to implement, faster to implement operations that access multiple locations
**Fine-grained**: opposite
- more simultaneous access
- often allows for more parallelism, but each lock has an overhead and requires memory.

# Pessimistic vs Optimistic locking
### Pessimistic Concurrency Control
assumes **all shared data will always be contended.** Any thread must forcefully keep others away from their currently owned resources.
- All locks
- $\rightarrow$ `synchronized`, `lock.lock()`, barriers, wait-notify etc...
### Optimistic Concurrency Control
assumes **contention is unlikely.** It will attempt to get the data without locks, and then check if any other thread has interrupted this. If not, proceed, else start over.
- CAS & TAS
- **Optimistic list** (without CAS/TAS) :
	- 1) Find all nodes involved (without locking)
	- 2) Make change iff everything is fine

[[Optimistic Locking]]

# Lazy Locking

# Example - LinkedList
![[Locking Strategies - LinkedList.png]]

## Coarse-grained locking
Trivial
![[LinkedListCourseGrained.png]]
## Fine-grained locking
> [!Important]
> Use **hand-over-hand locking** to prevent other threads passing the current thread!

![[LinkedListFineGrainedRemove.png]]
### Disadvantages
- `aquire` and `release` has to be called a lot of times before the desired elements are reached
- the hand-over-hand locking causes faster threads that want to reach an element at the end of the list to wait for a slower thread 


## Optimistic locking
See [[Optimistic Locking]]
## Lazy locking
See [[Lazy Locking]]
