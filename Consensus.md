#Slides-Lecture-26
>[! A protocol for n threads:]
> 1) All provide some value
> 2) In the end, all agree on the same value (that was provided by at leas one thread)

## Requirements on consensus protocol
### wait-free
Consensus returns **in finite time** for each thread
### consistent
All threads decide **the same value**
### valid
The common decision value is **some thread's input**

```java
public interface Consensus<T> {
	T decide (T value);
}
```

$\Longrightarrow$ [[Linearizability]] of consensus must be such that first thread's decision is adopted for all threads. 



![[ConsensusExample.png]]
## Consensus number
> [!Definition]
> A class $C$ solves **$n$-thread consensus** if there exists a consensus protocol (algorithm) using any number of objects of class $C$ and any number of atomic registers.

> [!Definition]
> **Consensus number** of $C$: largest $n$ such that $C$ solves $n$-thread consensus


## Concrete examples / Important theorems
> [!Important]
> 
> ### Theorem I
> **<center>Atomic Registers have consensus number 1.</center>**
> 
> #### Corollary
> **There is no [[Progress Assumptions and Progress Conditions#Wait-free|wait-free]] implementation of $n$-thread consensus, $n > 1$, from [[Atomicity#Atomic Register|read-write]] registers.**
> 
> #### Proof #TODO
> *exam relevant*
> 


> [!Important]
> ### Theorem II
> **<center>[[TAS and CAS|Compare-And-Swap]] has infinite consensus number.</center>**
> #### Proof: by construction
> #personalTODO :
> ![[CAS consensus.png]]
> 

> [!Important]
> ## Theorem III
> **<center>There is no [[Progress Assumptions and Progress Conditions#Wait-free|wait-free]] implementation of a FIFO queue with [[Atomicity#Atomic Register|atomic registers]].</center>**
> 
> ### Proof (by contradiction)
> ![[ConsensusFIFO1.png]]
> 
> ![[ConsensusFIFO2.png]]
> 
> ![[ConsensusFIFO3.png]]
> 
> **Why does this work?**
> If one thread gets the <font color="red">red</font> ball, the other gets the <font color="black">black</font> ball.
> 
> Since the winner decides their own value and the loser can find the value in the array, they output the same value.
> 
> Note that the threads write their values to the array before dequeueing from the queue.
> 
> **Now we have solved 2-thread consensus!**
> <font color="red">BUT since we only used atomic-registers, **this contradicts Theorem I**.</font>
> 
> $\Longrightarrow$ So Theorem III **must be true**!

## Comparison of synchronization numbers

![[Consensus numbers of standard synchronization tools.png]]

> Atomic Register: unable to “synchronize” two threads because there’s no way around Data [[Races]]
 $\Rightarrow$ **It is impossible to implement [[TAS and CAS]] with Atomic Registers!** $\rightarrow$ that’s why extra hardware is needed

### Why do we care?
+ [[Progress Assumptions and Progress Conditions#Wait-free|wait-free]] FIFO queues, wait-free [[TAS and CAS|RMW]] operations and [[TAS and CAS|CAS]] can not be implemented with atomic registers!
Otherwise, this would contradict the consensus number of Atomic Registers. (See Theorem III for an example)

### TAS consensus
![[TAS consensus code.png]]
#### ... It does not scale 
TAS is inherently binary :
- It returns true false, so a third one wouldn’t know which array index to return
- Even if it returned the previous value, it can only be 0 or 1 $\rightarrow$ same issue
 $\Rightarrow$ The binary is not enough for n-thread consensus; same logic for **AtomicInteger.getAndSet(int new)** - It sets the given value and return the previous. It’s consensus number is also 2.
 
 #### A global array for TAS and one for CAS...
![[The necessity of an array.png]]