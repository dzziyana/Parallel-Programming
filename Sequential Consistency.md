#Slides-Lecture-22 
> [!Definition]
> A [[HIstories|History]] $H$ is **linearizable** if it can be extended to a [[HIstories|History]] $G$
> + appending **zero or more** responses to pending invocations that **took effect**
> + discarding **zero or more** pending invocations that **did not take effect**
>   
>  such that $G$ is equivalent to a [[HIstories#sequential|legal, sequential]] History $S$
>   

> [!Important]
> $\huge\rightarrow_G \subset \rightarrow_S$ **is not required!**
> 
> $\Longrightarrow$ It is possible to **change inter-thread order**, but **not intra-thread order**

Very similar to [[Linearizability]], but weaker. (**Is implied by [[Linearizability]]**)

### General:
+ Operations by **one thread** respect program order
+ No need to preserve real-time order
	+ Cannot reorder operations done by **the same thread**
	+ **BUT**: Can reorder non-overlapping operations done by **different threads**
Is often used to describe multiprocessor memory architectures

Consequence for the [[Peterson Lock#]]


> [!Warning]
> ## Theorem
> 
> $$\large\textbf{Sequential consistency is not a local property!}$$
> 
> $\Longrightarrow$ **No composability!** (See [[Linearizability#Composability Theorem]])

#### Counterexample
![[SequentialConsistencyQueue1.png]]
![[SequentialConsistencyQueue2.png]]
![[SequentialConsistencyQueue3.png]]

#### Counterexample (**even simpler**)
![[SequentialConsistencyFlagsExample.png]]

## Example
![[SequentialConsistencyEx1.png]]![[SequentialConsistencyEx2.png]]

## The importance of sequential consistency
The pattern "**Write mine, read yours**" is exactly the **flag principle**
+ Peterson
+ Bakery
+ $\dots$
It seems non-negotiable!

**But**
+ Too expensive to implement on hardware
+ many hardware architects think it is **too** strong (/ restrictive)
$\Longrightarrow$ **Assume:** violated by default, honored by **explicit request** (e.g. volatile)

See [[Java's Memory Model#volatile fields]] 
