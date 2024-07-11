#Slides-Lecture-18 #Slides-Lecture-17 
## Why?
- **Locks** provide atomicity via mutual exclusion. However, they **lack the means for threads to communicate about changes.**
- Thus, they provide no order and are hard to use.
$\rightarrow$ Example: *[[Producer-Consumer]] queues*
# Semaphore
### ~ generalized lock (but not exactly!)
i.e. a lock can b e implemented by means of a semaphore and regarded as a special instance ("Spezialfall")
Locks ensure that only one thread operates between lock() and unlock(), whereas semaphores allow...
> **exactly n threads** between between acquire() and release()

> [!Warning]
> While in an old exam it was stated that `Semaphore(1) = Lock`, this was counted as wrong in one of the online quizzes (if I remember correctly). 
> #TODO fact check!
> 
> This is because a semaphore will decrease its semaphore number when calling `release` even if `acquire` has not been called beforehand.
#### formally...
~ data type with an integer value n ("semaphore number")
### pseudocode
![[Semaphore - pseudocode.png]]
# Rendezvous
*Rendezvous point:* location in code where threads P and Q wait for each to arrive.
### Idea
The thread that first reaches the rendezvous-section of the code must wait for others
- synchronized can't do that
- ... so instead it's done with 2 semaphores
### Bad pseudocode example - Deadlock
![[Rendezvous pseudocode bad.png]]
### Pseudocode example - legit
![[Rendezvous pseudocode legit.png]]
#TODO "pre-charging semaphores" optimization
### Implementing semaphores without spinning
via FIFO process list $Q_S$ :
![[Semaphores without spinning idea.png]]
# Barrier - Rendezvous for n
- all threads increment ***counter***
- ***counter*** is a shared variable **-> protected by lock**
- everyone **waits** til ***counter* = n**
- threads pass barrier 1-by-1...
...controlled, only n threads may enter, even if m > n threads are waiting
...**a second semaphore** (on ***barrier***) operates as **turnstile**
## Bad pseudocode
![[Barrier bad pseudocode.png]]
- is not reusable since count isn't 0 anymore
- This example only works if all threads pass and only then restart from top
#personalTODO

### Reusable Barrier - bad code:
![[ReusableBarrierBad.png]]
**Why this does not work / Why we need the second invariant**
As soon as the n-th Thread calls the first release, one of the other threads waiting could pass the whole barrier and then try to acquire it again, before the other threads have all passed it. This would mean that one thread passed the barrier twice, while another one of the threads waiting would not have passed it at all.

## Two-Phase Barrier - solution for proper reusability
### ... code example
![[Two-Phase Barrier code.png]]
**How this works** (read through the other (incorrect) code examples above first)
The Two-Phase-Barrier adds a second rendezvous. This ensures that if any thread is able to pass the second phase, all threads have already completed the first phase. So none of the threads that are released in the second phase can successfully complete the first phase again before the other threads have completed their first phase. So the problem we had with the bad code above does not occur.