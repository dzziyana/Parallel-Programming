> [!Error]
> **Note: ** I do not think a TATAS lock has anything to do with CAS 

= **Test and Test and Set**
- more or less a TAS /CAS lock with indentation
- Decreases time/thread compared to TAS
- does not generalize to other applications bc Memory Ordering ([[Java's Memory Model]]) leads to race conditions 
![[TATAS Lock - TA slides.png]]
#TODO *part of lecture where prof explains why it doesn't work (??? according to the TA slide at least)*
![[Pasted image 20240626172326.png]]
## Why is this preferrable?
The TAS operation causes bus contention. A simple read does not have this problem.

## With Backoff:
[[Backoff Lock]]

## Performance comparison (only example)
![[Pasted image 20240626175255.png]]