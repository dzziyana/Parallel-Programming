>[! A protocol for n threads:]
> 1) All provide some input
> 2) In the end, all agree on the same value

## Requirements on consensus protocol
### wait-free
Consensus returns **in finite time** for each thread
### consistent
All threads decide **the same value**
### valid
The common decision value is **some thread's input**

```
public interface Consesus <T> {
	T decide (T value);
}
```

## n-thread consensus
Consensus where all threads participate
## Consensus number
of an object C = highest $n$ for which C can be used to solve $n$-thread consensus
- To do this, you can use **any number of objects** of class C **and** any number of **atomic registers.**
### Concrete examples
![[Consensus numbers of standard synchronization tools.png]]

> Atomic Register: unable to “synchronize” two threads because there’s no way around Data [[Races]]
 $\Rightarrow$ **It is impossible to implement [[TAS and CAS]] with Atomic Registers!** $\rightarrow$ that’s why extra hardware is needed
 
### CAS consensus
#personalTODO :
![[CAS consensus.png]]
### TAS consensus
![[TAS consensus code.png]]
#### ... It does not scale 
TAS is inherently binary :
- It returns true false, so a third one wouldn’t know which array index to return
- Even if it returned the previous value, it can only be 0 or 1 $\rightarrow$ same issue
 $\Rightarrow$ The binary is not enough for n-thread consensus; same logic for **AtomicInteger.getAndSet(int new)** - It sets the given value and return the previous. It’s consensus number is also 2.
 
 #### A global array for TAS and one for CAS...
![[The necessity of an array.png]]