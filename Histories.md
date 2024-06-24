*Characterizing histories...*
### complete
A history is complete, when it has **no pending responses**
- i.e. to each invocation, there was a response
### sequential
every method call finishes (response) before next starts (invocation)
- but it’s okay if last invocation is still pending
### well-formed
A history $H$ is well-formed if **each thread's subhistory $H|P$ is sequential.**
### equivalent
Two histories $H$ and $H^{*}$ are equivalent if for every thread $A$, **the subhistory $H|A = H^{*}|A$**, i.e.
- Individual threads execute the same commands in the same intra-thread order
- The histories are two different interleavings of the same program
### Examples
![[Characterizing a history example 1.png]]
![[Characterizing a history example 2.png]]
## Projections
### Thread
= subhistory of H, with only the calls in thread
### Object
= subhistory of H, with only the calls on obj (e.g. a datastructure)
- all object projections are **legal** sequential programs (i.e. possible, fulfil pre- & postconds)
### Example
![[Projections example.png]]
## Sequential Consistency
- We want guarantees from our soft- and hardware which interleavings they allow and which not.
>[!Definition]
> History H is sequentially consistent if it can be extended to a history G...
> - appending $\geq$ 0 responses to pending invocations that took effect.
> - discarding $\geq$ 0 pending invocations that did not take effect.
...So that G is equivalent to a legal sequential history S.

### Task strategy - how to figure out if a history is seq. cons.?
1. Find all pending invocations.
		Whose effects are already visible in the effects of other calls? $\rightarrow$ add responses
		Effects are invisible?  $\rightarrow$ remove the invocation
2. Move methods around to form a logical sequential history.
		Cannot move them between threads
		**Must not** skip other methods within the same thread (preserve intra-thread orders)
>If not possible, then not sequentially consistent.

#### Taking Effect
![[Taking effect example.png]]

...[[Linearizeability (WIP)]] is stronger: H is linearizable iff H is sequentially consistent and...
![[Linearizibility intro.png]]