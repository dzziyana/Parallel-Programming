## Why?
- **Locks** provide atomicity via mutual exclusion. However, they **lack the means for threads to communicate about changes.**
- Thus, they provide no order and are hard to use.
$\rightarrow$ Example: *[[Producer-Consumer (WIP)]] queues*
# Semaphore
### ~ generalized lock
i.e. a lock can b e implemented by means of a semaphore and regarded as a special instance ("Spezialfall")
Locks ensure that only one thread operates between lock() and unlock(), whereas semaphores allow...
> **exactly n threads** between between acquire() and release()
#### formally...
~ data type with an integer value n ("semaphore number")
### pseudocode
![[Pasted image 20240613155806.png]]
# Rendezvous
*Rendezvous point:* location in code where threads P and Q wait for each to arrive.
### Idea
The thread that first reaches the rendezvous-section of the code must wait for others
- synchronized can't do that
- ... so instead it's done with 2 semaphores
### Bad pseudocode example - Deadlock
![[Pasted image 20240613162621.png]]
### Pseudocode example - legit
![[Pasted image 20240614174936.png]]
#TODO "pre-charging semaphores" optimization*
### Implementing semaphores without spinning
via FIFO process list $Q_S$ :
![[Pasted image 20240614175130.png]]
# Barrier - Rendezvous for n
- all threads increment ***counter***
- ***counter*** is a shared variable **-> protected by semaphore lock**
- everyone **waits** til ***counter* = max**
- threads pass barrier 1-by-1...
...controlled, only n threads may enter, even if m > n threads are waiting
...a second semaphore (on ***barrier***) operates as **turnstile**
## Two-Phase Barrier - solution for proper reusability
### ... code example
![[Pasted image 20240614175345.png]]

