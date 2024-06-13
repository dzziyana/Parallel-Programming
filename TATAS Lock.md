= **Test and Test and Set**
Decreases time/thread compared to TAS
- does not generalize bc Memory Ordering ([[Java's Memory Model]]) leads to race conditions 
![[Pasted image 20240613132246.png]]
#TODO *part of lecture where prof explains why it doesn't work (??? according to the TA slide at least)*
# With Backoff:
[[Backoff Lock]]
