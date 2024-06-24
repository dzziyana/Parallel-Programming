is a big chunk of *Lecture 18 pt 2*
**Locks and semaphores are insufficient** to efficiently implement producer-consumer queues because a mechanism is needed to produce a **"notification"** when the queue is not empty.
# Pattern
- There are entities (threads) that **compute** X and there are threads that **consume** X:
$T_0$  $\underrightarrow{X}$  $T_1$
...**Fundamental** concurrent programming pattern. Can be used to build data-flow parallel programs, e.g. **pipelines**.

# Multi Producer-consumer
= multiple producers on one end and multiples consumers on the other end
![[Multi Producer-Consumer figure 1.png]]
### requires a special Queue...
# Circular Buffer - tailored queue implementation
![[Circular Buffer - illustration.png]]
## Basic Idea
![[Circular Buffer (Queue) - initial implementation.png]]
### Helper functions
![[Circular Buffer - helper functions.png]]

## Concurrency-safe implementation (with monitor)
![[Producer-Consumer Queue with Monitor.png]]
### ... with explicit Lock
![[Pasted image 20240616180535.png]]