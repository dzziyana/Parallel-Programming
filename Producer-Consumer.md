#Slides-Lecture-19
**Locks and semaphores are insufficient** to efficiently implement producer-consumer queues because a mechanism is needed to produce a **"notification"** when the queue is not empty.
# Pattern
- There are entities (threads) that **compute** X and there are threads that **consume** X:
$$T_0 \xrightarrow{\hspace{1em} X \hspace{1em}} T_1$$
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

## Thread-safe implementation (with semaphores)

```java
import java.util.concurrent.Semaphore;  
class Queue {  
	...  
	Semaphore nonEmpty, nonFull, manipulation;  
	Queue(int s) {  
		...
		nonEmpty = new Semaphore(0); // use the counting feature of semaphores!  
		nonFull = new Semaphore(size); // use the counting feature of semaphores!  
		manipulation = new Semaphore(1); // binary semaphore  
	}  
}
```
![[ProducerConsumerSemaphores.png]]
## Thread-safe implementation (with monitor of object)
![[Producer-Consumer Queue with Monitor.png]]
### ... with explicit Lock using condition variables
for the use of condition variables, see [[Monitors#Condition Interface]]
![[Pasted image 20240616180535.png]]

### Sleeping-Barber Variant
Checks before signalling or awaiting if there is even a producer / consumer that is waiting
> [!Info]
>  $m \le 0 \Longleftrightarrow$ Buffer is full and $-m$ producers are waiting
>  $n \le 0 \Longleftrightarrow$ Buffer is empty and $-n$ consumers are waiting

```java
void enqueue(long x) {
	lock.lock();
	m--; if (m<0) // only wait for signal, if some consumer is waiting
		while (isFull())
		try { notFull.await(); }
		catch(InterruptedException e){}
	doEnqueue(x); // since no one was waiting or notFull got sent
				  // we can be sure that the queue is not full
				  // as m is incremented each time an element is 
				  // dequeued
				  
	n++;          // increment n and check if there is at least one 
				  // consumer waiting
	if (n<=0) notEmpty.signal();
	lock.unlock();
}
```
\
Dequeue works the same way.

```java
long dequeue() {
	long x;
	lock.lock();
	n--; if (n<0)
		while (isEmpty())
		try { notEmpty.await(); }
		catch(InterruptedException e){}
	x = doDequeue();
	m++;
	if (m<=0) notFull.signal();
	lock.unlock();
	return x;
}
```


