#Slides-Lecture-22 
*Characterizing histories...*

> [!Motivation]
> For **sequential** we can use Hoare-Logic to argue about correctness. For concurrent programs this is not possible, as we can not look at methods in isolation. 
> 
> **We need a formal notation to talk about concurrent code**

![[SequentialVsConcurrent.png]]
![[BlockingQueueBehaviour.png]]

## Definition
![[HistoryInvocationResponse.png]]
> [!Definition]
> **History $H =$** sequence of invocations and responses
![[HistoryExample.png]]
## Projections
### Thread
> [!Definition]
> $H | B$ = subhistory of $H$, with only the calls in thread $B$
### Object
> [!Definition]
> $H | q$ = subhistory of H, with only the calls on obj $q$ (e.g. a datastructure)
- all object projections are **legal** sequential programs (i.e. possible, fulfil pre- & postconds)
#### Example of projections
![[ProjectionsExample.png]]
## Characterisations
+ [[Linearizability]]
### complete
> [!Definition]
> A history is complete, when it has **no pending invocations**
> - i.e. to each invocation, there was a response

![[completeHistoryExample.png]]
### sequential
> [!Definition] 
> every method call finishes (response) before next starts (invocation)
> - but it’s okay if last invocation is still pending

![[sequentialHistoryExample.png]]
### well-formed
> [!Definition]
> A history $H$ is well-formed if **each thread's subhistory $H|P$ is sequential.**

![[well-formedHIstoryExample.png]]

### equivalent
> [!Definition]
> Two histories $H$ and $H^{*}$ are equivalent if for every thread $A$, **the subhistory $H|A = H^{*}|A$**, i.e.
> - Individual threads execute the same commands in the same intra-thread order
> - The histories are two different interleavings of the same program

### legal
> [!Defintiion]
> A sequential history $H$ is **legal** if for every object $x$ $H|x$ adheres to the sequential specification of $x$ 
> (i.e. Pre- & Postconditions)

### Precedence
![[Precedence.png]]
> [!Defintiion]
> **Given**: history $H$ and method executions $m_0$ and $m_1$ on $H$
> $$ m_0 \rightarrow_H m_1 \Longleftrightarrow m_0 \text{\textbf{ precedes }} m_1 $$

> $\rightarrow_H$ is a relation and implies partial order on $H$ 

**The order is total when $H$ is sequential!
### Examples
	
![[Characterizing a history example 1.png]]

![[Characterizing a history example 2.png]]

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

...[[Linearizability]] is stronger: H is linearizable iff H is sequentially consistent and...
![[Linearizibility intro.png]]