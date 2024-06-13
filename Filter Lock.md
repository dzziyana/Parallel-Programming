- every thread knows its level in the filter level[t]
- last level is CS
- in order to enter CS, a thread has to eliminate all levels
- **for each level, Peterson's mechanism is used** to filter **at most one thread** (victim) which stays behind, if other threads are at a higher level
## Pseudocode
![[Pasted image 20240513182550.png]]

## Java Implementation
 ![[Pasted image 20240513184115.png]]
filter lock is not fair, it's first-come first-serve (? - #TODO)