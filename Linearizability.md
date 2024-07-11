#Slides-Lecture-22 
> [!Important]
> We talk about specific **executions** not only about the source code!


#Slides-Lecture-22 
> [!Motivation]
> Each method should take effect **instantaneously** between invocation and response events. (see [[Histories]])
> 

> [!Definition]
> A [[Histories|History]] $H$ is **linearizable** if it can be extended to a [[Histories|History]] $G$
> + appending **zero or more** responses to pending invocations that **took effect**
> + discarding **zero or more** pending invocations that **did not take effect**
>   
>  such that $G$ is equivalent to a [[Histories#sequential|legal, sequential]] History $S$ with 
>  $$\huge\rightarrow_G \subset \rightarrow_S$$

Informally: Linearizability implies that there exists at least one valid execution order.

> [!Note]
> This directly implies [[Sequential Consistency]]
> 

## Composability Theorem
> [!Theorem]
> HIstory $H$ is **linearizable** $\Longleftrightarrow$ for every object $x$ $H|x$ is **linearizable**

**Consequence**:  Modularity
+ Linearizablity of objects can be proven in isolation
+ independently implemented objects can be composed
### How to figure out if a history is linearizable?
1. Find all pending invocations.
		Whose effects are already visible in the effects of other calls? $\rightarrow$ add responses
		Effects are invisible?  $\rightarrow$ remove the invocation
2. Move methods around to form a logical sequential history.
		Cannot move them between threads
		**Must not** skip other methods **- no matter of which thread** (preserve **global** order)
>If not possible, then not linearizable.
## Linearization Point
![[LinearizationPointsEx.png]]
> [!Intution] 
> #TODO *Make sure this makes sense, as this is never explicitly mentioned in the slides and just my personal understanding (I would leave this note in to make sure this is not taken as a definition of linearization point). So take this with a grain of salt.*
> 
> A **linearization point** is a part of the code that either makes changes *visible* to other threads or depends on a global state that can be changed by other threads.
> 
> In other words, a linearization point is part of the code where the changes **take effect**.



### Reasoning about Linearizability
![[ReasoningLinearizabilityLocking.png]]
![[ReasoningLinearizabilityWaitFree.png]]
![[ReasoningLinearizabilityLockFree.png]]

### Summary & Strategy
**Identify one atomic step where the method "happens".**
+ Critical Section
+ Machine Instruction
**Does not always work**
+ Might need to define several different steps for a given method

**Linearizability summary**:
+ Powerful specification tool for shared objects
+ Allows us to capture the notion of objects being "atomic"


