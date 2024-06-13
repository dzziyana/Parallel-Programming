# Semaphore
~ generalised lock, i.e. ein Lock kann durch eine Semaphore implementiert werden bzw. als Spezialfall einer Semaphore gesehen werden
[[Locks]] ensure that only one thread operates between lock() and unlock(), whereas semaphores allow...
> **exactly n threads** between between acquire() and release()
#### formally...
~ data type with an integer value n ("semaphore number")
### pseudocode
![[Pasted image 20240613155806.png]]
# Rendezvous
#TODO why?
### Idea
The thread that first reaches the rendezvous-section of the code must wait for others
- synchronized can't do that
- ... so instead it's done with 2 semaphores
### Pseudocode example
![[Pasted image 20240613162621.png]]
# Barrier - Rendezvous for n
- all threads increment *counter*
- *counter* is a shared variable -> protected by semaphore lock
- everyone waits til *counter* = max
- threads pass barrier 1-by-1...
...controlled, only n threads may enter, even if m > n threads are waiting
...a second semaphore operates as turnstile
