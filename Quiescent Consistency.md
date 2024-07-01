#Slides-Lecture-22 
> [!Info]
> This was marked as **side remark** in the slides.
> 
> Our professor mentioned that he has not seen a single use case of this property in practice and that is nonetheless often mentioned in literature.

> [!Idea]
> Programs should respect real-time order of algorithm separated by periods of **quiescence**.
> ![[QuiescentConsistencyIdea.png]]
> 
> $\Longleftrightarrow$ Non-overlapping methods take effect in their **real-time order**!


> 
## Comparison to [[Sequential Consistency]]
> [!Warning]
> The two are **incompatible**!

#### Sequentially consistent but not quiescently consistent
![[SequentiallyNotQuiescently.png]]
#### Quiescently consistent but not sequentially consistent
![[QuiescentNotSequential.png]]