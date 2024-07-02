See [[Progress Assumptions and Progress Conditions#Lock-free]] for further information on what lock free means.

## Implementations
#Slides-Lecture-19 
### Lock-free counter with CAS

![[LockFreeCounterCAS.png]]

> [!Important]
> While this almost looks like a [[Spinlock]], which is obviously blocking, it is in fact non blocking, because the [[TAS and CAS#Hardware support for atomic operations|CAS]] operation is used to potentially make progress (it is not part of the [[Locks#**Waiting Section**| waiting section]] of a lock). If the CAS operation fails, it means that another thread made progress and incremented the counter!

### Lock-free stack
![[LockFreeStack1.png]]
![[LockFreeStack3.png]]
![[LockFreeStack2.png]]
#### Performance - lock-free not automatically faster!
![[LockFreeStackPerf.png]]

### Lock-free list set
> [!Idea]
> Use markers like in a [[Lazy Locking#Lazy List|lazy list]], but without locking.
> 
> **Problem**: We want to change both a reference and the marker atomically at the same time.
> 
> **Solution**: Store the marker bit in our 64-bit address, since we don't need all bits of the pointer.

##### Attempt (**wrong!**):
![[LazyListSetWrong2.png]]
![[LazyListSetWrong1.png]]
##### Solution:
![[LockFreeListSet4.png]]

![[LockFreeListSet3.png]]
![[LockFreeListSet2.png]]
![[LockFreeListSet1.png]]

### Lock-free unbounded queue
#Slides-Lecture-20
![[MotivationLockFreeQueue.png]]
#### First attempt (wrong!)
![[LazyQueue1.png]]
![[LazyQueue2.png]]
> [!Error]
> We need to update two pointers at the same time!
> 
> **Otherwise:** Inconsistency! e.g. in `enqueue` `tail` points to old `tail`, but `next` pointer of `tail` is not empty
> 
> Waiting for inconsistency to be established would be locking camouflaged!

#### Using `AtomicReference` (still wrong!)
![[LazyQueue3.png]]
> [!Error]
> This attempt still has multiple problems:
> ![[LazyQueue4.png]]
> ![[LazyQueue5.png]]
> ![[LazyQueue6.png]]


#### Protocol (final, correct version)
> [!Idea]
> Allow `tail` to not always point to the last element. If either `enqueue` or `dequeue` try to access `tail`, potentially advance it until it is truly the last element.

![[LazyQueue7.png]]
![[LazyQueue9.png]]
- head and tail as `AtomicReference` objects due to both having potential concurrency issues while being modified as intended
- `first` and `last` variables for copies of `head and tail, CAS to try and set last.next
- dequeuer is supposed to retry til failure, enqueuer does not attempt any retries

#### With hypothetical DCAS (Double [[TAS and CAS|CAS]])
![[UnboundedLockFreeQueueDCAS.png]]
