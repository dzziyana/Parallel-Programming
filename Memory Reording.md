#Slides-Lecture-15 
> [!Rule of thumb]
> Compiler and hardware allowed to make changes **that do not affect the semantics** of a <font color="red">sequentially</font> executed program
> 
![[MemoryReorderingExample.png]]

**Why?**
Huge potential for optimizations
+ Dead code elimination
+ Register hoisting 
+ Locality optimizations
+ $\dots$

**Memory Hierachy**
### Example: Fail with self-made rendevouz (C  / GCC)
![[FailSelfMadeRendevouz.png]]

For more information: [[Java's Memory Model]]