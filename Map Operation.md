#Slides-Lecture-10-11
From the Slide:
> A **map** A map operates on each element of a collection independently to create a new collection of the same size

This is in contrast to the [[Reduction Operation]]
The Computation of an element of the result **does not** depend on another element of the result.

**No combination of results!**

### Maps
$T_{p} = O(\frac{T_{1}}{P} + T_{\infty}) = O(\frac{n}{P} + 1) = O(\frac{n}{P})$

![[Pasted image 20240612140634.png]]

## Example (Vector Addition)

```java
int[] vector_add(int[] arr1, int[] arr2){
	assert (arr1.length == arr2.length);
	result = new int[arr1.length];
	FORALL(i = 0; i < arr1.length; i++) {
		result[i] = arr1[i] + arr2[i];
	}
	return result;
}
```

## Usage with [[ForkJoin]] (Vector Addition)
```java
protected void compute(){
	if(hi-lo < SEQUENTIAL_CUTOFF) {
		for(int i=lo; i < hi; i++)
		res[i] = arr1[i] + arr2[i]; // 2 inputs, 1 output
	} else {
		int mid = (hi+lo)/2;
		VecAdd left = new VecAdd(lo,mid,res,arr1,arr2);
		VecAdd right = new VecAdd(mid,hi,res,arr1,arr2);
		left.fork();
		right.compute();
		left.join();
	}
}
```
Note that the `res` Array is passed to all instances of `VecAdd` and the different instances only write to their respective part of `res`