- implies sequential consistency
- Plus, ... ( #TODO )
### How to figure out if a history is linearizable?
1. Find all pending invocations.
		Whose effects are already visible in the effects of other calls? $\rightarrow$ add responses
		Effects are invisible?  $\rightarrow$ remove the invocation
2. Move methods around to form a logical sequential history.
		Cannot move them between threads
		**Must not** skip other methods **- no matter of which thread** (preserve **global** order)
>If not possible, then not linearizable.

### Linearisierungspunkt
