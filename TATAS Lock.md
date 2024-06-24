= **Test and Test and Set**
- more or less a TAS/CAS lock with indentation
- Decreases time/thread compared to TAS
- does not generalize bc Memory Ordering ([[Java's Memory Model]]) leads to race conditions 
![[TATAS Lock - TA slides.png]]
#TODO *part of lecture where prof explains why it doesn't work (??? according to the TA slide at least)*
## With Backoff:
[[Backoff Lock]]
