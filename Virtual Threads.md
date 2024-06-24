- decouple OS threads from threads visible to the programmer
- **native threads** still used as **carrier threads**
- **JVM** decides which instructions are assigned to carrier threads
##### $\Rightarrow$ more efficient arrangements possible & reduced number of expensive context switches

> A #VirtualThread is a **Java entity**, not a wrapper around the OS

#### Benefits
- cheap to create
- cheap to block
- almost no language changes required
#### Drawbacks
not very useful for long-running CPU-intensive operations

#Continuation #Scheduler
![[Continuation and Scheduler.png]]
#comparison #platformThreads
![[Platform threads vs Virtual threads.png]]