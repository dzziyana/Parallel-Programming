#Slides-Lecture-10-11
From the Script: 
> Produce single answer from collection via an **associative operator**

The idea is to split up the task into multiple smaller parts that can be computed in parallel recursively.

In contrast to the [[Map Operation]], the results are combined.

This is essentially **Divide and Conquer**.

## Time Complexity
$T_{p} = O(\frac{T_{1}}{P} + T_{\infty}) = O(\frac{n}{P} + logn)$

In contrast to the [[Map Operation]]

![[Pasted image 20240612140634.png]]


## Example with [[ForkJoin]] (Recursive Sum, no cutoff)
```java
protected Long compute() {
	if(high - low <= 1)
		return array[high];
	else {
		int mid = low + (high - low) / 2;
		SumForkJoin left = new SumForkJoin(array, low, mid);
		SumForkJoin right = new SumForkJoin(array, mid, high);
		left.fork();
		right.fork();
		return left.join() + right.join();
	}
}
```

![[ReduceGraphic.png]]

We went from $O(n)$ sequentially to $O(n \log n)$ in parallel (with unlimited processors, of course)
## Example with [[ForkJoin]] (Recursive Sum, cutoff)
```java
protected Long compute() {
	if(high - low <= SEQUENTIAL_THRESHOLD) {
		long sum = 0;
		for(int i=low; i < high; ++i)
			sum += array[i];
		return sum;
	} else {
		int mid = low + (high - low) / 2;
		SumForkJoin left = new SumForkJoin(array, low, mid);
		SumForkJoin right = new SumForkJoin(array, mid, high);
		left.fork();
		long rightAns = right.compute();
		long leftAns = left.join();
		return leftAns + rightAns;
	}
}
```
