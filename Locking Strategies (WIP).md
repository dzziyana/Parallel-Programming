# Lock granularity
**Coarse-grained**: fewer locks with more objects per lock
- simple to implement, faster to implement operations that access multiple locations
**Fine-grained**: opposite
- more simultaneous access

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

[[Optimistic Locking (WIP)]]

# Lazy Locking

# Lock-free Programming

# Comparison
## Example - LinkedList
![[Locking Strategies - LinkedList.png]]