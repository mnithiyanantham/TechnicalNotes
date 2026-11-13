## Advanced DSA 1

#### Count Sort - TODO

                Sort based on counting frequency of the elements.

#### Find smallest number that can be formed by rearranging the digits given in the array.

**Approach 1:**

Sort the array. This takes `TC: O(N log N)`

**Approach 2:**

    **Psedocode**

1. Count frequency of all the digits

2. Use frequency array to fill the original array

```java

```

<img src="Notes-Advanced%20DSA1/2026-03-08-23-53-09-image.png" title="" alt="" width="518">

**TC:** $O(N+10+N) = O(N)$

Will count sort work if the range of A[i] is greater than $10^9$?

    $1 int => 4 bytes$

    $10^9 integers => 4B * 10^9 = 4GB$

               Count sort works if the given range is $10^6$

            $4B * 10^6 = 4MB$

**Frequency array size**

    Iterate the array and find the min and max of array

    `int freq[max-min+1] = <0>`

Given array `A = [-1 3 8 3 -2 3]`

`index = A[i] - min`

<img title="" src="Notes-Advanced%20DSA1/2026-03-09-00-03-42-image.png" alt="" width="547">

**TC:** $O(3N + Range) => O(N+Range)$

**SC:** $O(Range)=> O(max - min+1)$

#### Merge Sort- TODO

* Merge sort is one of the **most efficient sorting algorithms**.

* It works on the principle of **Divide and Conquer**.

* Merge sort repeatedly breaks down a list into several sublists until each sublist consists of a **single element** and merging those sublists in a manner that **results into a sorted list**.
- [ ] Take notes from pdf

      

#### Prefix Sum

Prefix sum = $sum(L-R) = pf[R] - pf[L-1]$

Number of subarray of N size array = $n(n+1)/2$

No of sub arrays starting at 0th element = N

No of sub arrays starting at 0th element = N-1

No of sub arrays starting at 0th element = N-2

...

No of sub arrays starting at 0th element = 1

Finally it becomes = $1+2+3+....+N = n(n+1)/2$

**No of sub arrays of size k in N size array:**

| 0   | 1   | 2   | 3   | .... | k   | .... | N-1 |
| --- | --- | --- | --- | ---- | --- | ---- | --- |

No of ending points = $[k-1, N-1]$

Total sub array = $N-1-(k-1)+1 = N-k+1$

#### Max possible sub array sum - TODO

Use Kadane's Algorithm

* Reset sum if **negative number** is encountered. Dont carry forward the negative number

* just compare the current element with sum and keep `max `value in sum

#### Amount of water trapped between bars - TODO

>  water = $min(left_(max), right_(max) ) - A[i]$

`if(water >0)`

    `ans += water;`

#### Rotate array k times

1. Reverse the entire array

2. Reverse first k elements

3. Reverse n-k elements at last

#### Common Notes

Applying XOR on same element will result in 0.

To identify unique element in an array, apply XOR on all elements

Every element occurs thrice, find the unique element.

1. Count each bits , apply %3, result will give the unique element.

Check if **i^th^** bit is set in a number -> **Apply & on the ith bit**

                                    `N & (1<<i)`
