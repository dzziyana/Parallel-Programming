#Slides-Lecture-20 
> [!In a nutshell]  
> When we use [[TAS and CAS|CAS/TAS]] to update our value, we often want to express the desire to **"set iff nothing bad happened in the meantime"**
> 
> This is however not what the instruction does. It only checks if the value itself has changed, disregarding any potential context.
> 
> **ABA** in this case means: 
> 1. We read value *A*
> 2. Some thread writes value *B* to our variable
> 3. Some other thread writes value *A* again
> 4. We perform [[TAS and CAS|CAS/TAS]], which of course succeeds, but in reality something bad happened. Assume that the variable contained a pointer and that we reuse pointers. Then the object referenced by the pointer might have contained something entirely else, while we based our computation on the old object *A* pointed to.

Or more concise from the lecture:

> [!Definition]
> "The ABA problem ... occurs when one activity fails to recognize that a single
memory location was modified temporarily by another activity and therefore
erroneously assumes that the overall state has not been changed."
![[ABA4.png]]
## Concrete Example
**We want to have a stack, but do not want to allocate for new nodes, rather have a pool that stores a set of nodes and reuses them**.
![[ABA1.png]]
![[ABA2.png]]
![[ABA3.png]]
> [!Error]
> This is obviously very bad.

## Possible Solutions
### DCAS (Double [[TAS and CAS#Hardware support for atomic operations|CAS]])
+ not available on most platforms
+ where do you draw the line when implementing in hardware? Triple CAS, Quadruple CAS?
### Garbage Collection
+ There needs to be a garbage collector for this to work...
+ Implementation of e.g. a lock-free garbage collector using a GC?
+ Too slow for e.g. the inner loop of a kernel
### Pointer Tagging
> [!Warning]
> Only delays the problem, can work in practice.

![[ABA5.png]]
### Hazard Pointers
> [!Idea]
> Let *P* be the pointer that is reused and causes problems.
> + Keep an array of length $n = \text{\# threads}$
> + Before reading a pointer *P* enter it into the array slot of the current thread.
> + After the CAS, remove *P* from slot
> + **Before trying to reuse pointer *P*, check all slots of the array for *P***

#### Use with the [[ABA Problem#Concrete Example|concrete example]]
![[ABA6.png]]
![[ABA7.png]]
![[ABA8.png]]
![[ABA9.png]]

> [! Warning]
> This obviously has a performance penalty
### [[Transactional Memory]]

*Useful web resource:*
https://www.baeldung.com/cs/aba-concurrency
