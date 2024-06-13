- decouple OS threads from threads visible to the programmer
- **native threads** still used as **carrier threads**
- **JVM** decides which instructions are assigned to carrier threads
=> more efficient arrangements possible & reduced number of expensive context switches
A #VirtualThread is a Java entity, not a wrapper around the OS
- cheap to create
- cheap to block
- almost no language changes required
not very useful for long-running CPU-intensive operations though
#Continuation #Scheduler
![[Pasted image 20240505213817.png]]
#comparison #platformThreads
![[Pasted image 20240505221343.png]]