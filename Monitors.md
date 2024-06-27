#Slides-Lecture-19
*aka intrinsic locks, monitor locks*
A monitor is not an object, it’s an **interface**
> "any object intended to be used safely by multiple threads"

To be precise, any code object that has the following **functionality** is a monitor...
## Functionality
A monitor provides **a mechanism to check a condition** with following semantics:
>**If a condition does not hold...**
- **release** the monitor lock
- **wait** for the condition **to become true**
- **signaling** mechanism to avoid busy-loops (spinning)
![[Monitor Queues.png]]

[[Producer-Consumer]]  ( #personalTODO *What is the illustration above saying?*)
## In Java: wait() - notify() - notifyAll()
- **wait()** - the current thread waits until it is signaled via notify() by another thread
- **notify()** - wakes up one arbitrary waiting thread
- **notifyAll()** - wakes up all waiting threads
[[ExecutorService (WIP)]]
[[Thread States in Java]]
## Possible semantics of notify()
### signal and wait
- signaling process exits the monitor (goes to waiting entry queue) 
- signaling process passes monitor lock to signaled
### signal and continue (in Java!)
- signaling process continues running
- signaling process moves signaled process to waiting entry queue

# Implementing a semaphore
![[Pasted image 20240615172330.png]]
> If, additionally, different threads evaluate different conditions, the notification has to be a **notifyAll**

# Condition Interface
Conditions are always associated with a lock. **So firstly, instantiate one:**
`Lock lock = new ReentrantLock();`
A lock can have multiple conditions:
![[Pasted image 20240615172745.png]]

**.await()**
- called with the lock held
- **atomically** releases the lock and waits until thread is signaled
- **guaranteed** to hold lock when returns
- thread **always** needs to check condition
**.signal{All}()**
- called with the lock held
