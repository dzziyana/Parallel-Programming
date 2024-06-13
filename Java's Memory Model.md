#### (minimale) Garantien (/definitions) wie JVM mit der Memory-Hardware interagiert
• More Safety  More Guarantees  Less Optimization Possible  Less Speed
#### - volatile fields
• 1) compiler MUST NOT reorder/delete
• 2) caches must IMMEDIATELY write to main memory
##### example:
```
class C {
	private volatile int x = 0;
	private volatile int y = 0;
	void f() {
	//Thread 1
		x = 1;
		y = 1;
	}
	void g() {
	//Thread 2
		int a = y;
		int b = x;
		assert(b >= a);
	}
}
```
Kein bad interleaving möglich für assert, aber falls $T_{1}$ und  $T_{2}$ auf unterschiedlichen Cores laufen...
- x = 1, y = 1 im Cache vom Core von $T_1$
- y = 1 im Main Memory, $T_2$ kopiert im eigenen Cache
- $T_2$ liest vom eigenen Cache -> y = 1 & x = 0 -> a = 1 & b = 0
- x = 1 im MainMemory, $T_2$ kopiert in eigenen Cache
#### in volatile fields
... accesses don't count as data races
... slower than regular fields, faster than locks
**... rather don't use them**
f.e. used in a *turn* variable context where else the different threads would have different views on the state of the variable => atomic

## JMM : Program Order
is a total order of intra-thread (not between threads) actions., does not provide an ordering guarantee for memory accesses. 
***Intra**-thread consistency*: **Per thread**, the PO's order is consistent with the thread's isolated execution.
![[Pasted image 20240612160859.png]]
## Synchronization Actions (SA) & Synchronization Order (SO)
### SAs are...
- **read/write** of a **volatile** variable
- un/lock monitor
- **first/last** action of a thread (synthetic)
- actions which **start a thread**
- actions which determine **whether a thread has terminated**

> **SAs form the SO**
### SO
- is a total order
- All thread see an SA in the same order.
- SA within a thread are in Program Order 
- SO is consistent: all reads in SO see the last writes in SO

## Synchronizes-With (SW) / Happens-Before (HB) Orders
- **SW** only **pairs** the **specific actions which "see" each other.**
- A volatile write to x synchronizes with subsequent read of x (subsequent in SO)
#### The **transitive closure** of PO and SW **forms HB**

**HB consistency**: When reading a variable, we see either the last write (in HB) or any other unordered write.
▪ This means races are allowed!
