#### Atomic Action = change in state of hardware
- **cannot interrupt**: happens either completely or not at all
- **cannot interleave** with any other single action
- **effects cannot be seen until the action is complete**, *e.g. no half-changed variables!*
- -> abstraction: it happens in a single moment 
- but several actions can still happen in different orders & form bad interleavings

> [!Error]
> The following is **wrong** according to the lecture slides:
>
#### Atomic Register = hardware component
- register = individual memory cell, stores 0/1 (or X/Z)
- atomic registers have hardware support for atomic actions

> [!Definition]
> An **Atomic Register** is a basic memory object $r$ ($\ne$ CPU register) with two operations `r.read()` and `r.write()` that satisfy the following:
> + An invocation $J$ of the operations takes effect at a **single point in time** $\tau(J)$
> + $\tau(J)$ always lies **between** the **start** and the **end** of $J$
> + Two operations $J, K$ on the same register always have a **different effect time** $\tau(J) \ne \tau(K)$
> + An invocation $J$ of `r.read()` returns the value $v$ written by the invocation $K$ with **closest preceding effect time** $\tau(K)$ 

##### Example of an Atomic Register
![[Pasted image 20240626165712.png]]
#### Atomic Operation
**= code construct that, if executed, triggers an atomic action in an atomic register**