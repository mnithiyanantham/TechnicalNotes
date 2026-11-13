### Heaps

PriorityQueue is the Data structure which helps achive Heaps in Java.

```java
// Min Heap
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// Max Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

#### Insert in Heap

* Add element at last

* Upheapify - starting from $(n-1)/2$, till $i>0$

* TC: $O(log N)$

<img src="Notes-Advanced%20DSA4/2026-08-31-14-21-13-image.png" title="" alt="" width="660">

#### Delete from Min Heap

* Swap first and last elements. remove last element.

* Down heapify  it - do heapify from **0th** element

* SC: $O(log N)$

<img src="Notes-Advanced%20DSA4/2026-08-31-14-23-34-image.png" title="" alt="" width="686">

#### Heapify

* All non-leaf node must be checked for heap order property

* Heapify from **non-lead node till 0th Node**

<img title="" src="Notes-Advanced%20DSA4/2026-08-31-13-17-44-image.png" alt="" width="660">

```java
class Solution {
    public int[] buildHeap(int[] A) {
        int n = A.length;

        for(int i=(n/2)-1; i>=0; i--) {
            heapify(A, i); 
        }
        return A;
    }

    private int[] heapify(int[] A, int i) {

        while((2*i) + 1 < A.length) {
            int lc = (2*i) + 1;
            int rc = (2*i) + 2;

            int min = Math.min(A[lc], A[i]);

            if(rc < A.length) {
                min = Math.min(A[rc], min);
            }
            if(min == A[i]) {
                break;
            } else if(A[lc] == min){
                swap(A, i, lc);
                i = lc;
            } else {
                swap(A, i, rc);
                i = rc;
            }
        }
        return A;
    }

    private void swap(int[] A, int a, int b) {
        int temp = A[a];
        A[a] = A[b];
        A[b] = temp;
    }
}
```

> TC: $O(n)$

#### Build Heap

Given an array **A** of **N** integers, convert that array into a min heap and return the array.

NOTE: A min heap is a binary tree where every node has a value less than or equal to its children.

**Approach 1:**

Sort using Arrays.sort()

**Approach 2:**

For each element, call minHeap.add() method. TC: $O(NlogN)$

**Approach 3:**

Build heap in linear time complexity

**Notes:**

* Heapify from last non-leaf node till 0th node.

* That means -> **Start from the parent of last node** => $(n/2)-1$. Navigate down till last node. LC must not go beyond array size.

* Find minimum among parent, LC and RC. Navigate it's tree down till last node.

```java
class Solution {
    public int[] buildHeap(int[] A) {
        int n = A.length;

        // Start from parent of last node => (n/2)-1
        for(int i=(n/2)-1; i>=0; i--) {
            heapify(A, i); 
        }
        return A;
    }

    private int[] heapify(int[] A, int i) {
        // LC must not go beyond the size of array
        while((2*i) + 1 < A.length) {
            int lc = (2*i) + 1;
            int rc = (2*i) + 2;

            int min = Math.min(A[lc], A[i]);

            if(rc < A.length) {
                min = Math.min(A[rc], min);
            }
            // Find min element among parent, lc and rc. Navigate down till last node
            if(min == A[i]) {
                break;
            } else if(A[lc] == min){
                swap(A, i, lc);
                i = lc;
            } else {
                swap(A, i, rc);
                i = rc;
            }
        }
        return A;
    }

    private void swap(int[] A, int a, int b) {
        int temp = A[a];
        A[a] = A[b];
        A[b] = temp;
    }
}
```

> TC: $O(N)$
> 
> SC: $O(1)$
> 
> ![](Notes-Advanced%20DSA4/2026-09-08-23-24-37-image.png)
> 
> ![](Notes-Advanced%20DSA4/2026-09-08-23-24-54-image.png)

#### Heap Sort

![](Notes-Advanced%20DSA4/2026-09-09-12-00-53-image.png)

![](Notes-Advanced%20DSA4/2026-09-09-12-01-15-image.png)

### Greedy Algorithm

A Greedy Algorithm is an algorithmic approach in which we make the **best possible choice at each step**, hoping to obtain the overall optimal solution.

In simple terms, choose what looks best right now, without reconsidering previous choices.

Greedy algorithms are commonly used in DSA problems involving optimization, such as **finding minimum cost, maximum profit, or the shortest possible route under certain conditions.**

**<u>PriorityQueue</u>** is the Data structure which helps achive Heaps in Java.

```java
// Min Heap
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// Max Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

#### Common Greedy Algorithm problems

| Problem                           | Greedy choice                                                      |
| --------------------------------- | ------------------------------------------------------------------ |
| Connect Ropes                     | Combine the two smallest ropes                                     |
| Activity Selection                | Choose the activity that finishes earliest                         |
| Fractional Knapsack               | Choose the highest value-to-weight ratio                           |
| Minimum Spanning Tree — Prim's    | Choose the minimum-weight edge connecting the tree to a new vertex |
| Minimum Spanning Tree — Kruskal's | Choose the smallest valid edge that doesn't create a cycle         |
| Job Scheduling                    | Choose jobs according to profit and deadline constraints           |

A heap is often used alongside greedy algorithms when we repeatedly need the smallest or largest available element.

#### Max jobs - Greedy Algorithm

There are **N** jobs to be done, but you can do only one job at a time.

Given an array **A** denoting the start time of the jobs and an array **B** denoting the finish time of the jobs.

Your aim is to select jobs in such a way so that you can finish the **maximum** number of jobs.  

Return the **maximum** number of jobs you can finish.

**Notes:**

* Follow Greedy approach
* Sort the given arrays based on job finish time
* Compare the finish and start times for max jobs

```java
public class Solution {
    public int solve(int[] A, int[] B) {

        Jobs[] jobslist = new Jobs[A.length];

        for(int i=0; i< A.length; i++) {
            jobslist[i] = new Jobs(A[i], B[i]);
        }
        // Sort the jobs list based on the end time
        Arrays.sort(jobslist, Comparator.comparingInt(Jobs::getSecond));
        int onGoingJob=-1;
        int count = 0;
        for(int i=0; i<A.length; i++) {
            // if ongoing job end time <= new job start time, pick it up
            if(onGoingJob == -1 || jobslist[i].getFirst() >= onGoingJob) {
                count++;
                onGoingJob = jobslist[i].getSecond();
            }
        }
        return count;
    }
    public class Jobs {
        private int first;
        private int second;
        public Jobs(int first, int second) {
            this.first = first;
            this.second = second;
        }
        public int getFirst() {
            return this.first;
        }
        public int getSecond() {
            return this.second;
        }
    }
}
```

#### Median of Stream of Integers -> HARD

Given an infinite stream of Integers, Find the median after every new element is added.

**Notes:**

![](Notes-Advanced%20DSA4/2026-09-09-00-41-10-image.png)

![](Notes-Advanced%20DSA4/2026-09-09-00-42-46-image.png)

* Instead of using ArrayList, we can use PriorityQueue, which will make this impl easy

```java
public class Solution {
    public ArrayList<Integer> solve(ArrayList<Integer> A) {
        int n = A.size();
        // Keep first half of the list
        ArrayList<Integer> maxHeap = new ArrayList<Integer>();
        // Keep second half of the list
        ArrayList<Integer> minHeap = new ArrayList<Integer>();
        ArrayList<Integer> C = new ArrayList<Integer>();

        for(int i=0; i<n; i++) {
            int x = A.get(i);

            // Insert current value in min or max heap
            if(maxHeap.size()==0 || x <= maxHeap.get(0)) {
                maxHeap.add(x);
                bubbleUpMaxHeap(maxHeap);
            } else {
                minHeap.add(x);
                bubbleUpMinHeap(minHeap);
            }

            // maxHeap is having 2 elements more than minHeap
            if(maxHeap.size() - minHeap.size() == 2) {
                minHeap.add(maxHeap.get(0));
                bubbleUpMinHeap(minHeap);

                // Remove maxElement from maxHeap
                swap(maxHeap, 0, maxHeap.size()-1);
                maxHeap.remove(maxHeap.size()-1);
                heapifyMaxHeap(maxHeap);
            } else if(minHeap.size() > maxHeap.size()) { // Swap if minHeap has more elements than maxHeap
                maxHeap.add(minHeap.get(0));
                bubbleUpMaxHeap(maxHeap);

                swap(minHeap, 0, minHeap.size()-1);
                minHeap.remove(minHeap.size()-1);
                heapifyMinHeap(minHeap);
            }
            C.add(maxHeap.get(0));                              
        }
        return C;
    }

    private void heapifyMaxHeap(ArrayList<Integer> maxHeap) {
        int n = maxHeap.size();
        for(int i=0; (2*i + 1) <n;) {
            int lc = 2*i + 1;
            int rc = 2*i + 2;
            int max = Math.max(maxHeap.get(i), maxHeap.get(lc));

            if(rc < n) {
                max = Math.max(max, maxHeap.get(rc));
            }

            if(max == maxHeap.get(i)) {
                break;
            } else if(max == maxHeap.get(lc)) {
                swap(maxHeap, i, lc);
                i = lc;
            } else {
                swap(maxHeap, i, rc);
                i = rc;
            }
        }
    }
    private void heapifyMinHeap(ArrayList<Integer> minHeap) {
        int n = minHeap.size();
        for(int i=0; 2*i + 1<n;) {
            int lc = 2*i + 1;
            int rc = 2*i + 2;
            int min = Math.min(minHeap.get(i), minHeap.get(lc));

            if(rc <n) {
                min = Math.min(min, minHeap.get(rc));
            }
            if(min == minHeap.get(i)) {
                break;
            } else if(min == minHeap.get(lc)) {
                swap(minHeap, i, lc);
                i = lc;
            } else {
                swap(minHeap, i, rc);
                i = rc;
            }
        }
    }

    private void bubbleUpMaxHeap(ArrayList<Integer> maxHeap) {
        int i = maxHeap.size()-1;
        while(i > 0 ) {
            int pi = (i-1)/2;
            if(maxHeap.get(pi) < maxHeap.get(i)) {
                swap(maxHeap, i, pi);
                i = pi;
            } else {
                break;
            }
        }
    }
    private void bubbleUpMinHeap(ArrayList<Integer> minHeap) {
         int i = minHeap.size()-1;
        while(i > 0 ) {
            int pi = (i-1)/2;
            if(minHeap.get(pi) > minHeap.get(i)) {
                swap(minHeap, i, pi);
                i = pi;
            } else {
                break;
            }
        }
    }
    private void swap(ArrayList<Integer> list, int i, int j) {
        int temp = list.get(i);
        list.set(i, list.get(j));
        list.set(j, temp);
    }   
}
```

> TC: $O(NlogN)$
> 
> SC: $O(N)$

### Dynamic Programming

Fibonacci series - $O(2^n)$

This calculates same problem again and again.

This does the same thing again and again.

<img title="" src="Notes-Advanced%20DSA4/2026-08-12-23-50-36-image.png" alt="" width="723">

* Store info of each sub problem some where and reuse it.

**Properties of Dynamic Programming:**

1. `Overlapping sub problems` - Solving a problem multiple times

2. `Optimal Structure` - Solving a problem using smaller sub problem

> **Break a problem into smaller subproblems, solve each subproblem once, and reuse its result.**

This is especially important for problems such as:

- Knapsack
- Coin Change
- Longest Common Subsequence
- Longest Increasing Subsequence
- Matrix Chain Multiplication
- Minimum Path Sum

**Types of Dynamic Programming**

#### Top down approach (Memoisation)

* Recursive solution

* We start with bigger problem and keep on breaking it till we reach base case.

* Then we start building ans from the base case.

    <img title="" src="Notes-Advanced%20DSA4/2026-08-28-23-03-10-image.png" alt="" width="601">

TC: $O(n)$

SC: $O(n)$

#### Bottom up approach (Tabulation)

* Iterative solution 

* We start with the base case and start building up ans for the bigger problems

<img src="Notes-Advanced%20DSA4/2026-08-28-23-03-46-image.png" title="" alt="" width="636">

TC: $O(n)$

SC: $O(n)$

#### Climbing Stairs

You are climbing a staircase and it takes **A** steps to reach the top.

Each time you can either climb **1** or **2** steps. In how many **distinct ways** can you climb to the top?

Return the number of distinct ways modulo 1000000007

**Notes:**

* This is same as Fibanacci series

<img src="Notes-Advanced%20DSA4/2026-09-10-00-01-14-image.png" title="" alt="" width="569">

```java
public class Solution {
    public int climbStairs(int A) {

        int ans = 0;
        int a = 0;
        int b = 1;
        if(A == 0 || A == 1) {
            return A;
        }
        for(int i=1; i<=A; i++) {
            ans = a + b;
            a = b;
            b = ans%1000000007;
        }
        return ans%1000000007;
    }
}
```

#### Find minimum Squares

Given an integer **A**. Return **minimum** count of numbers, sum of whose **squares** is equal to **A**.

<img src="Notes-Advanced%20DSA4/2026-09-10-00-12-31-image.png" title="" alt="" width="601">

**Notes:**

* Use Memoization

```java
public class Solution {
    public int countMinSquares(int A) {
        int dp[] = new int[A+1];
        for(int i=0; i<=A; i++) {
            dp[i] = -1;
        }
        return minSquares(A, dp);
    }
    private int minSquares(int A, int[] dp) {

        if(A == 0) return 0;
        if(dp[A] != -1) return dp[A];
        int min = Integer.MAX_VALUE;
        // For i -> 1 to i*i<=A, iterate and call the function in Recusive way
        for(int i=1; i*i<=A; i++) {
            min = Math.min(min, minSquares(A-i*i, dp));
        }
        dp[A] = min + 1;
        return min+1;
    }
}
```

> TC: $O(n*sqrt(n))$ -> for loop will run for $sqrt(n)$ times. When i is 1 it will be executed for n times.
> 
> SC: $O(n)$

**Using Tabulation approach:**

```java
import java.util.Arrays;

public class Solution {
    public int countMinSquares(int A) {
        int[] dp = new int[A + 1];

        Arrays.fill(dp, Integer.MAX_VALUE);

        dp[0] = 0;

        for (int i = 1; i <= A; i++) {
            for (int j = 1; j * j <= i; j++) {
                dp[i] = Math.min(dp[i], 1 + dp[i - j * j]);
            }
        }

        return dp[A];
    }
}
```

#### House robber - TODO

#### Max Sum Without Adjacent Elements

Given a **2 x N** grid of integers, **A**, your task is to choose numbers from the grid such that **sum** of these numbers is **maximized**.   
However, you cannot choose two numbers that are <mark>adjacent horizontally, vertically, or diagonally.</mark>   

Return the maximum possible sum.  

**Note:** You are allowed to choose more than 2 numbers from the grid.

**Notes:**

This is based on **bottom up approach** - using iteration. Hence it is possible to achieve $O(1)$ time complexity.

```java
public class Solution {
    public int adjacent(ArrayList<ArrayList<Integer>> A) {

        int m = A.get(0).size();
        // Max of 1st column
        int temp1 = Math.max(A.get(0).get(0), A.get(1).get(0));
        // return temp1, if only one column is there
        if (m == 1) {
            return temp1;
        }
        // Max of 2nd column
        int temp2 = Math.max(A.get(0).get(1), A.get(1).get(1));
        temp2 = Math.max(temp1, temp2);

        int curMax = 0;
        int temp;
        // Start with 2
        for(int i=2; i<m; i++) {
            curMax = Math.max(A.get(0).get(i), A.get(1).get(i));
            temp = temp2;
            temp2 = Math.max(temp1 + curMax, temp2);
            temp1 = temp;
        }
        return temp2;       
    }
}
```

 A = [   
        [1, 2, 3, 4]
        [2, 3, 4, 5]    
     ]

Ans: 8

#### Number of ways to reach/Unique paths in a Grid - TODO at end

Given a grid of size **n * m**, lets assume you are starting at **(1,1)** and your goal is to reach **(n, m)**.   
At any instance, if you are on **(x, y)**, you can either go to **(x, y + 1)** or **(x + 1, y)**.  

Now consider if some obstacles are added to the grids.   
Return the total number unique paths from (1, 1) to (n, m).  

**Note:**   

1. An obstacle is marked as **1** and empty space is marked **0** respectively in the grid.  
2. Given Source Point and Destination points are 1-based index.

$ways(i,j) = ways(i-1, j) + ways(i, j-1)$

![](Notes-Advanced%20DSA4/2026-08-30-09-08-49-image.png)

**Bottom up approach:**

:bulb: This does not require checking $dp[i][j]  != -1$, as each element will be visited only once.    

```java
public class Solution {
    public int uniquePathsWithObstacles(ArrayList<ArrayList<Integer>> A) {

        ArrayList<ArrayList<Integer>> dp = new ArrayList<ArrayList<Integer>>();

        int n = A.size();
        int m = A.get(0).size();

        ArrayList<Integer> row;

        for(int i=0; i<n; i++) {
            row = new ArrayList<Integer>();

            for(int j=0; j<m; j++) {   
                // for first row             
                if(i==0) {
                    if(j == 0) {                        
                        row.add(A.get(0).get(0) != 1 ? 1: 0); // i & j =0, consider A(0,0) only
                    } else {
                        // First row, take the previous column based on current cell value
                        row.add(A.get(0).get(j) != 1 ? row.get(j-1): 0); 
                    }
                    continue;
                }
                // for first column
                if(j==0) {
                    if(i != 0) {
                        // First column, just take the previous row first column based on current cell value
                        row.add(A.get(i).get(j) != 1 ? dp.get(i-1).get(0): 0);
                    }                   
                    continue;
                }
                // for all other cells, sum of (i-1, j) & (i, j-1)
                row.add(A.get(i).get(j) != 1 ? dp.get(i-1).get(j) + row.get(j-1) : 0);            
            }
            dp.add(row);                    
        }      
        return dp.get(n-1).get(m-1);
    }
}
```

> TC: $O(n*m)$
> 
> SC: $O(n*m)$

- [ ] How to make it in $O(m)$ time complexity

#### N digit numbers

Find out the number of **A** digit positive numbers, whose digits on being added equals to a given number **B**.

Note that a valid number starts from digits 1-9 except the number 0 itself. i.e. leading zeroes are not allowed.

Since the answer can be large, output answer modulo 1000000007

<img src="Notes-Advanced%20DSA4/2026-08-30-16-00-38-image.png" title="" alt="" width="605">

![](Notes-Advanced%20DSA4/2026-08-30-16-02-50-image.png)

```java
public class Solution {
    int[][] dp;

    public int solve(int A, int B) {
        fillArray(A, B);
        return findSum(A, B);
    }

    private int findSum(int A, int B) {
        if(B<0) {
            return 0;
        }
        // If given number of one digit number
        if(A == 1) {
            return B>=1 && B<=9 ? 1 : 0;
        }
        if(dp[A][B] != -1) {
            return dp[A][B];
        }
        int sum = 0;
        // Iterate for number from 0 to 9
        for(int i=0; i<=9; i++) {
            sum = (sum + findSum(A-1, B-i))%1000000007;             
        } 
        dp[A][B] = sum;

        return dp[A][B];
    }
    // Create dp array with -1 value
    private void fillArray(int A, int B) {
        dp = new int[A+1][B+1];
        for(int i=0; i<=A; i++) {
            for(int j=0; j<=B; j++) {
                dp[i][j] = -1;
            }
        }
    }
}
```

> TC: $O(A*B)$
> 
> SC: $O(A*B)$ -> includes the space taken for dp and Stack

#### Unique BSTs/Catalon numbers

Given an integer **A**, how many structurally unique BST's (binary search trees) exist that can store values 1...A?

<img src="Notes-Advanced%20DSA4/2026-08-30-17-03-13-image.png" title="" alt="" width="559">

<img src="Notes-Advanced%20DSA4/2026-09-13-15-58-45-image.png" title="" alt="" width="622">

Notes:

* createa a catalian arr. `catalianNum[i] = catalian sum upto i.`

```java
public class Solution {
    public int numTrees(int A) {
        int[] catalianNum = new int[A+1];
        catalianNum[0] = 1;
        catalianNum[1] = 1;
        for(int i=2; i<=A; i++) {
            int sum = 0;
            for(int j=0; j<=i-1; j++) {
                sum += catalianNum[j] * catalianNum[i-1-j];
            }
             catalianNum[i] = sum;
        }
        return catalianNum[A];
    }
}
```

#### Knapsack - Subset Problems - REVISE

N array has $2powN$ sub sets.

<img src="Notes-Advanced%20DSA4/2026-09-15-00-42-36-image.png" title="" alt="" width="591">

##### 0-1 Knapsack

Given two integer arrays **A** and **B** of size **N** each which represent **values** and **weights** associated with **N** items respectively.

Also given an integer **C** which represents knapsack capacity.

Find out the maximum value subset of **A** such that sum of the weights of this subset is smaller than or equal to **C**.

**NOTE:**

- You cannot break an item, either pick the complete item, or don’t pick it (0-1 property).

###### Using Recursion/Memoization

<img title="" src="Notes-Advanced%20DSA4/2026-09-14-19-27-52-image.png" alt="" width="699">

```java
public class Solution {

    public int solve(ArrayList<Integer> A, ArrayList<Integer> B, int C) {
        int[][] dp = new int[A.size()+1][C+1];

        // Initiate dp array to -1
        for(int i=1; i<=A.size(); i++) {
            for(int j=1; j<=C; j++) {
                dp[i][j] = -1;                 
            }
        } 
        // Call function with upper values
        return findMax(A, B, dp, A.size(), C);
    }

    private int findMax(ArrayList<Integer> A, ArrayList<Integer> B, int[][] dp, int N, int C) {
        if(C == 0 ) return 0;
        // Invalidates invalid path when capacity goes < 0
        if(C < 0) return Integer.MIN_VALUE;
        // If N is 0, there is no element, so happiness is 0
        if(N == 0 ) return 0;

        if(dp[N][C] != -1) {
            return dp[N][C];
        }

        int incl =  A.get(N-1) + findMax(A, B, dp, N-1, C-B.get(N-1));
        int excl = findMax(A, B, dp, N-1, C);
        int ans = Math.max(incl, excl);
        dp[N][C] = ans;
        return ans;
    }
}
```

> TC: $O(NB)$
> 
> SC: $O(NB + N+B) => O(NB)$ 
> 
> Note: N+B is the stack space

###### Using Iteration/Tabulation

<img title="" src="Notes-Advanced%20DSA4/2026-09-14-19-27-10-image.png" alt="" width="611">

```java
public class Solution {
    public int solve(int[] A, int[] B, int C) {
        int n = A.length;
        // Initiate to n+1, C+1
        int[][] dp = new int[n+1][C+1];

        // if n==0, happiness is 0 as there is no element
        for(int i=0; i<=n; i++) {
            dp[i][0] = 0;
        }
        // if C==0, happiness is 0 as there is no capacity
        for(int i=0; i<=C; i++) {
            dp[0][i] = 0;
        }               
        int incl = 0;
        int excl = 0;
        for(int i=1; i<=n; i++) {
            for(int j=1; j<=C; j++) {
                // Check remaining capacity is >= current weight
                if(B[i-1] <= j) {
                    incl = A[i-1] + dp[i-1][j-B[i-1]];
                    excl = dp[i-1][j];
                    dp[i][j] = Math.max(incl, excl);
                } else {
                    dp[i][j] = dp[i-1][j]; 
                }
            }
        }
        return dp[n][C];
    }
}
```

> TC: $O(NB)$
> 
> SC: $O(NB)$

##### Unbounded Knapsack

https://www.scaler.com/academy/mentee-dashboard/class/523628/assignment/problems/9340/?navref=cl_pb_nv_tb

Given a knapsack weight **A** and a set of items with certain value **B**[i] and weight **C**[i], we need to calculate maximum amount that could fit in this quantity.

This is different from classical Knapsack problem, here we are allowed to use unlimited number of instances of an item.

<img src="Notes-Advanced%20DSA4/2026-09-14-23-54-31-image.png" title="" alt="" width="671">

**Using Recursion:**

```java
public class Solution {
    public int solve(int A, ArrayList<Integer> B, ArrayList<Integer> C) {
        int[] dp = new int[A+1];
        for(int i=0; i<A+1; i++) {
            dp[i] = -1;
        }
        return findMax(A, B, C, dp, B.size());
    }

    private int findMax(int A, ArrayList<Integer> B, ArrayList<Integer> C, int[] dp, int N) {
        // Return 0 if either N or A becomes 0
        if(N == 0) return 0;
        if(A == 0) return 0;
        // Invalid case
        if(A < 0) return Integer.MIN_VALUE;

        if(dp[A] != -1) return dp[A];

        // IMPORTANT - Don't reduce N here, for repicking the same item
        int incl = B.get(N-1) + findMax(A - C.get(N-1), B, C, dp, N);
        int excl = findMax(A, B, C, dp, N-1);
        dp[A] = Math.max(incl, excl);
        return dp[A];
    }
}
```

>     TC: $O(AN)$
> 
> SC: $O(N + AN) => O(AN)$

**Using Iteration:**

Notes:

* <u>Use one dimensional array</u> to store happiness index, as same element can be picked up any number of times. So **<u>we dont need to track the element index here.</u>**

* For each happiness index, go through all weights

```java
public class Solution {
    public int solve(int A, int[] B, int[] C) {
        int n = B.length;
        // Create one dimensional array only
        int[] dp = new int[A+1];

        int incl;
        int excl;
        for(int i=1; i<=n; i++) {
            for(int j=1; j<=A; j++) {
                if(C[i-1] <= j) {
                    // Capacity is reduced
                    incl = B[i-1] + dp[j-C[i-1]];
                    excl = dp[j];
                    dp[j] = Math.max(incl, excl);                    
                }               
            }
        }
        return dp[A];
    }
}
```

>  TC: $O(AN)$
> 
> SC: $O(A)$

##### Fractional Knapsack

https://www.scaler.com/academy/mentee-dashboard/class/523628/assignment/problems/35883/?navref=cl_pb_nv_tb

Given two integer arrays **A** and **B** of size **N** each which represent **values** and **weights** associated with **N** items respectively.

Also given an integer **C** which represents knapsack capacity.

Find out the maximum total value that we can fit in the knapsack. If the maximum total value is **ans**, then return **⌊ans × 100⌋** , i.e., floor of (ans × 100).

NOTE:

- You can break an item for maximizing the total value of the knapsack

Notes:

* Sort the array based on the per unit value of the given values

* <u>Use indicesArray, assign index as value, sort it based on the per unit value</u>

```java
public class Solution {
    public int solve(int[] A, int[] B, int C) {
        int n = A.length;

        Integer[] indicesArray = new Integer[n];
        for(int i=0; i<n; i++) {
            indicesArray[i] = i;
        }
        // Sort with out creating another Java Object
        Arrays.sort(indicesArray, (i,j) -> Double.compare((double) A[j]/B[j], (double) A[i]/B[i]));
        // Maintain remaining capacity
        int remainingCapacity = C;
        double ans = 0.0;
        int index;
        for(int i=0; i<n; i++) {
            index = indicesArray[i];

            // check remainingCapacity >= current weight
            if(remainingCapacity >= B[index]) {
                ans += A[index] * 100;
                remainingCapacity -= B[index];
            } else { // if not, use the fraction
                ans += (double) remainingCapacity * (double) A[index] * 100 / (double) B[index];
                remainingCapacity = 0;
                break;
            }
        }
        return (int) Math.floor(ans);
    }
}
```

> TC: $O(NlogN)$
> 
> SC: $O(N)$

##### 0-1 Knapsack Extended - REVISE

>  This is similar to Knapsack 0-1, but the input array size is too high such that it cant be accomodated in array index.

**Problem Statement:**

Given two integer arrays **A** and **B** of size **N** each which represent **values** and **weights** associated with **N** items respectively.

Also given an integer **C** which represents knapsack capacity.

Find out the **maximum value** subset of **A** such that sum of the weights of this subset is smaller than or equal to **C**.

**NOTE:** You cannot break an item, either pick the complete item, or don’t pick it (0-1 property).

1 <= N <= 500

1 <= C, B[i] <= 106

1 <= A[i] <= 50

###### Using Iteration

<img title="" src="Notes-Advanced%20DSA4/2026-09-20-00-12-42-image.png" alt="" width="691">

<img title="" src="Notes-Advanced%20DSA4/2026-09-20-00-11-33-image.png" alt="" width="646">

<img title="" src="Notes-Advanced%20DSA4/2026-09-20-00-11-49-image.png" alt="" width="467">

```java
public class Solution {
    public int solve(int[] A, int[] B, int C) {
        int n = A.length;
        int maxValue = 0;

        // Calculate max value possible with given values
        for(int i=0; i<n; i++) {
            maxValue += A[i];
        }
        // 2D dp array, Column = maxValue
        int[][] dp = new int[n+1][maxValue+1];

        // For each maxValue, identify the minWeight
        for(int i=0; i<=n; i++) {
            for(int j=0; j<= maxValue; j++) {
                if(j==0) dp[i][j] = 0;
                else if(i==0) dp[i][j] = Integer.MAX_VALUE;
                else {
                    int pick = Integer.MAX_VALUE;
                    int notPick = dp[i-1][j];
                    // Check value is less than current value
                    // dp[i-1][j-A[i-1]] must be possible, otherwise assgning INFINITY value for current profit
                    if(A[i-1] <= j && dp[i-1][j-A[i-1]] != Integer.MAX_VALUE) {
                        pick = B[i-1] + dp[i-1][j-A[i-1]];
                    }
                    dp[i][j] = Math.min(pick, notPick);                    
                }
            }
        }
        int ans=0;
        for(int i=maxValue; i>=0; i--) {
            if(dp[n][i] <= C) {
                ans = i;
                break;
            }
        }
        return ans;
    }
}
```

> TC: $O(N*C)$
> 
> SC: $O(N*C)$

###### Using Recursion - 2D dp array

- [ ] Is this completely a recursive way???

```java
public class Solution {
    public int solve(ArrayList<Integer> A, ArrayList<Integer> B, int C) {
        int n = A.size();
        int maxVaue = 0;

        // Find the maxValue
        for(int i=0; i<n; i++) {
            maxVaue += A.get(i);            
        }
        int[][] dp = new int[n+1][maxVaue+1];

        // Initialize the first row and first column
        // If value is 0, weight is 0
        for(int i=0; i<=n; i++) {
            dp[i][0] = 0;
        }
        // If element is 0, it is not possible to reach value        
        for(int j=1; j<=maxVaue; j++) {
            dp[0][j] = Integer.MAX_VALUE;
        }  

        for(int i=1; i<=n; i++) {
            for(int j=1; j<= maxVaue; j++) {
                dp[i][j] = -1;
            }
        } 
        // Call method in iteration     
        for(int j=maxVaue; j>0; j--) {
            findMaxValue(A, B, C, dp, n, j);
        }

        // Find the minimum weight
        int ans = 0;
        for(int j = maxVaue; j>=0; j--) {
            if(dp[n][j] <= C) {
                ans = j;
                break;
            }
        }
        return ans;
    }

    private int findMaxValue(ArrayList<Integer> A, ArrayList<Integer> B, int C, int[][] dp, int i, int j) {
        if(i==0) return dp[i][j];
        if(j ==0) return 0;
        if( dp[i][j] != -1) return dp[i][j];

        int notPick = findMaxValue(A, B, C, dp, i-1, j);
        int pick = Integer.MAX_VALUE;

        if(A.get(i-1) <= j) {
            int rValue = findMaxValue(A, B, C, dp, i-1, j - A.get(i-1));
            if(rValue != Integer.MAX_VALUE) {
                pick = B.get(i-1) + rValue;
            }
        }
        dp[i][j] = Math.min(pick, notPick);
        return dp[i][j];
    }
}
```

###### Using Iteration - 1D dp array

```java
public class Solution {
    public int solve(ArrayList<Integer> A, ArrayList<Integer> B, int C) {
        int maxValue = 0;

        // Find the maxValue
        for(int i=0; i<A.size(); i++) {
            maxValue += A.get(i);            
        }
        int[] dp = new int[maxValue+1];

        // If value is 0, weight is 0
        dp[0] = 0;                
        for(int i=1; i <= maxValue; i++) {
            dp[i] = Integer.MAX_VALUE;            
        } 
        // For each element, identify the min possible weight    
        for(int i=0; i<A.size(); i++) {
            for(int j=maxValue; j >= 1; j--) {
                if(A.get(i) <= j && dp[j - A.get(i)] != Integer.MAX_VALUE) {                    
                    dp[j] = Math.min(dp[j], B.get(i) + dp[j - A.get(i)]);                    
                }
            }            
        }
        // Find the minimum weight
        int ans = 0;
        for(int j = maxValue; j>=0; j--) {
            if(dp[j] <= C) {
                ans = j;
                break;
            }
        }
        return ans;
    }
}
```

### Graphs - Introduction, DFS & Cycles

Weighted Graphs

Graph Traversals

DFS, BFS - Depth First Search, Breadth First Search

| Djiktstra                                       | Prim                                                                                                       |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Shortest distance from src to  every other node | MST -> A tree which connects all nodes (N-1) with min over all cost.                                       |
|                                                 | Prim's algorithm is used to find a Minimum Spanning Tree (MST) of a connected, weighted, undirected graph. |

| Algorithm     | Use cases                                      |
|:-------------:|:----------------------------------------------:|
| **BFS**       | Shortest path from src in **unweighted** graph |
| **Dijkstra**  | Shortest path from src in **weighted** graph   |
| **MST, Prim** | Connect all nodes with min cost                |

| Algorithm    | Data structure        | Purpose                                             |
| ------------ | --------------------- | --------------------------------------------------- |
| **DFS**      | Stack / Recursion     | Graph traversal                                     |
| **BFS**      | Queue                 | Graph traversal / shortest path in unweighted graph |
| **Prim's**   | **Min PriorityQueue** | **Minimum Spanning Tree**                           |
| **Dijkstra** | **Min PriorityQueue** | Shortest path                                       |

A good way to remember -

**BFS → Queue → level-by-level traversal**  
**DFS → Stack/Recursion → go deep**  
**Prim → Min Heap → minimum edge → MST**

#### Depth First Search

##### Cycle in Directed Graph

**Problem Description**  

Given an directed graph having **A** nodes. A matrix **B** of size `M x 2` is given which represents the **M** edges such that there is a edge directed from node **B[i][0]** to node **B[i][1]**.

Find whether the graph contains a cycle or not, return **1** if cycle is present else return **0**.

**NOTE:**

- The cycle must contain atleast two nodes.
- There are no self-loops in the graph.
- There are no multiple edges between two nodes.
- The graph may or may not be connected.
- Nodes are numbered from 1 to A

```java
public class Solution {
    public int solve(int A, int[][] B) {

        // Create an array of List<Integer>
        List<Integer>[] graph = new ArrayList[A+1];
        boolean[] visited = new boolean[A+1];
        boolean[] path = new boolean[A+1];
        int ans = 0;
        // Create the graph from the given 2D array
        generateGraph(A, B, graph);
        // Call dfs function on all nodes
        for(int i=1; i<=A; i++) {
            ans = dfs(i, graph, visited, path);
            if(ans == 1) return ans;
        }    
        return ans;   
    }

    private int dfs(int currentNode, List<Integer>[] graph, boolean[] visited, boolean[] path) {

        // Mark visited, path arrays as true for currentNode
        visited[currentNode] = true;
        path[currentNode] = true;

        // Check currentNode has neighbours
        if(graph[currentNode] != null) {
            for(int j=0; j<graph[currentNode].size(); j++) {                
                int nbr = graph[currentNode].get(j);
                if(visited[nbr] == true) {
                    // If it is already in path array, it is a cycle
                    if(path[nbr] == true) {
                        return 1;
                    }
                } else {
                    if(dfs(nbr, graph, visited, path) == 1) {
                        return 1;
                    }
                }
            }            
        }
        // Mark path of currentNode is false, as we are coming in reverse and out of currentNode
        path[currentNode] = false;
        // return 0 as cycle is not found
        return 0;
    }

    private void generateGraph(int A, int[][] B, List<Integer>[] graph) {
        for(int i=0; i<B.length; i++) {
            int v = B[i][0];
            int u = B[i][1];
            if(graph[v] != null) {
                graph[v].add(u);
            } else {
                graph[v] = new ArrayList<>(Arrays.asList(u));
            }
        }
    }
}
```

> TC: 
> 
> SC: 

##### Find Path exists in Directed Graph

https://www.scaler.com/academy/mentee-dashboard/class/523632/assignment/problems/9359/?navref=cl_pb_nv_tb

**Problem Description**  

Given an directed graph having **A** nodes labelled from **1** to **A** containing **M** edges given by matrix **B** of size `M x 2`such that there is a edge directed from node

**B[i][0]** to node **B[i][1]**.

Find whether a path exists from node **1** to node **A**.

Return **1** if path exists else return **0**.

**NOTE:**

- There are no self-loops in the graph.
- There are no multiple edges between two n

```java
public class Solution {
    public int solve(int A, int[][] B) {
        // Create an array of List<Integer>
        List<Integer>[] graph = new ArrayList[A+1];
        boolean[] visited = new boolean[A+1];
        // Create the graph from the given 2D array
        generateGraph(A, B, graph);
        return dfs(A, 1, graph, visited) ? 1 : 0;        
    }

    private boolean dfs(int A, int currentNode, List<Integer>[] graph, boolean[] visited) {

        // Mark visited for currentNode
        visited[currentNode] = true;
        // Return true if the currentNode is A
        if(currentNode == A) {
            return true;
        }
        // Check currentNode has neighbours
        if(graph[currentNode] != null) {            
            for(int j=0; j<graph[currentNode].size(); j++) {                
                int nbr = graph[currentNode].get(j);               
                if(visited[nbr] == false && dfs(A, nbr, graph, visited))
                    return true;                                    
            }            
        }
        // return false as node A is not found
        return false;
    }

    private void generateGraph(int A, int[][] B, List<Integer>[] graph) {
        for(int i=0; i<B.length; i++) {
            int v = B[i][0];
            int u = B[i][1];
            if(graph[v] != null) {
                graph[v].add(u);
            } else {
                graph[v] = new ArrayList<>(Arrays.asList(u));
            }
        }
    }
}
```

> TC: 
> 
> SC: $O(N)$

##### Connected Components in an Undirected Graph

https://www.scaler.com/academy/mentee-dashboard/class/481561/assignment/problems/272002?navref=cl_tt_lst_nm

**Problem Description**  

Given an undirected graph with **A** vertices numbered from **0 to A-1** and **E** edges, represented as a 2D array B[][], where each entry **B[i] = [u, v]** denotes an edge between vertices u and v.  

Your task is to return a list of all connected components. Each connected component should be represented as a list of its vertices, with all components returned in a collection where each component is listed separately.  

**Note:**  
Before returning the result, **sort the vertices** inside each connected component, and also **sort the list of components** as a whole (based on their first elements). This ensures a fully ordered output.

**Problem Constraints**  

1 ≤ A, E ≤ 105  
0 ≤ B[i][0], B[i][1] < A

**Output Format**  

Return a list of all connected components in sorted order

**Example Input**  

Input 1:

A: 5, 
B: [
     [0, 1], 
     [2, 1], 
     [3, 4]
   ]

Input 2:

A: 7, 
B: [
     [0, 1], 
     [6, 0], 
     [2, 4], 
     [2, 3], 
     [3, 4]
   ]

**Example Output**  

Output 1:

[[0, 1, 2], [3, 4]]

Output 2:

[[0, 1, 6], [2, 3, 4], [5]]

```java
public class Solution {
    public ArrayList<ArrayList<Integer>> getComponents(int A, ArrayList<ArrayList<Integer>> B) {

        List<Integer>[] graph = generateGraph(A, B); // Generate Graph

        ArrayList<ArrayList<Integer>> ansList = new ArrayList<ArrayList<Integer>>();
        boolean[] visited = new boolean[A];

        // Nodes are from 0 t A-1
        for(int i=0; i<A; i++) {
            ArrayList<Integer> nbrList = new ArrayList<>();
            // if the node is not visited, do dfs
            if(visited[i] == false) {
                dfs(i, graph, nbrList, visited);
                Collections.sort(nbrList);
                ansList.add(nbrList);
            }                        
        }
        // sort as a whole, based on the first element of inner list
        Collections.sort(ansList, (a,b) -> a.get(0) - b.get(0));
        return ansList;
    }

    private void dfs(int curNode, List<Integer>[] graph, List<Integer> nbrList, boolean[] visited) {
        // mark curNode as visited
        visited[curNode] = true;
        // add curNode to nbrList
        nbrList.add(curNode);

        // do dfs on all children of curNode
        if(graph[curNode] != null) {
            for(int i=0; i<graph[curNode].size(); i++) {
                int childNode = graph[curNode].get(i);
                // do dfs on child Node if its not visited already
                if(visited[childNode] == false) {
                    dfs(childNode, graph, nbrList, visited);
                }
            }
        }    
    }

    private List<Integer>[] generateGraph(int A, ArrayList<ArrayList<Integer>> B) {
        ArrayList<Integer>[] graph = new ArrayList[A+1];

        for(int i=0; i<B.size(); i++) {
            int v = B.get(i).get(0);
            int u = B.get(i).get(1);

            // as it is undirected graph, add nodes in both direction
            graph[v] = graph[v] == null ? new ArrayList<>() : graph[v];           
            graph[v].add(u);

            graph[u] = graph[u] == null ? new ArrayList<>() : graph[u];            
            graph[u].add(v);            
        }
        return graph;
    }
}
```

#### Breadth First Search

1. Level Order Traversal

2. At each step, we will explore neighbours. They will provide info about their neighbours

**Multi Source BFS**

* We will do BFS from all sources simultaneously

##### Rotten Oranges

**Problem Description**  

Given a matrix of integers **A** of size N x M consisting of 0, 1 or 2.

Each cell can have three values:

The value 0 representing an empty cell.

The value 1 representing a fresh orange.

The value 2 representing a rotten orange.

Every minute, any fresh orange that is adjacent (Left, Right, Top, or Bottom) to a rotten orange becomes rotten. Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1 instead.

**Note**: Your solution will run on multiple test cases. If you are using global variables, make sure to clear them.

**Notes:**

* Keep co-ordinates and duration of each rotten Orange in Queue. This helps for BFS traverse.

* Navigate through all four directions of all Rotten Oranges

```java
public class Solution {
    public int solve(int[][] A) {
        Queue<ArrayList<Integer>> queue = new LinkedList<>();
        int freshOrangesCount = constructQueue(A, queue);
        return traverseThroughMatrix(A, queue, freshOrangesCount);
    }

    private int constructQueue(int[][] A, Queue<ArrayList<Integer>> queue) {
        int n = A.length;
        int m = A[0].length;
        int freshOrangesCount = 0;

        for(int i=0; i<n; i++) {
            for(int j=0; j<m; j++) {
                if(A[i][j] == 2) {
                    // Collect data of all Rotten Oranges
                    queue.add(new ArrayList<>(Arrays.asList(i, j, 0)));
                } else if(A[i][j] == 1) {
                    // Increment freshOrangesCount
                    freshOrangesCount++;
                }
            }
        }
        return freshOrangesCount;
    }

    private int traverseThroughMatrix(int[][] A, Queue<ArrayList<Integer>> queue, int freshOrangesCount) {
        // To navigate from Rotten Oranges
        int[] arrX = {-1,0,1,0};
        int[] arrY = {0,-1,0,1};
        int n = A.length;
        int m = A[0].length;
        int ans = 0;

        // Iterate through all Rotten Oranges
        while(!queue.isEmpty()) {
            List<Integer> data = queue.poll();
            int x = data.get(0);
            int y = data.get(1);
            int duration = data.get(2);

            // navigate in all four directions of each Rotten Orange
            for(int i=0; i<4; i++) {
                int curX = x + arrX[i];
                int curY = y + arrY[i];
                // Pick if the Orange is fresh
                if(curX >= 0 && curX < n && curY >= 0 && curY < m && A[curX][curY] == 1) {
                    queue.add(new ArrayList<>(Arrays.asList(curX, curY, duration+1)));
                    // Increment time
                    ans = duration + 1;
                    // Make it Rotten Orange
                    A[curX][curY] = 2;
                    // Reduce number of freshOrangesCount
                    freshOrangesCount--;
                }
            }
        }
        // Return the time if all fresh Oranges are rotten
        return freshOrangesCount == 0 ? ans : -1;
    }
}
```

> TC: $O(V+E) => O(n*m + 4*n*m) => O(n*m)$
> 
> SC: $O(V) => O(n*m)$

#### Another BFS - Equal Edge Cost

**Problem Description**

Given a weighted undirected graph having A nodes, a source node C and destination node D.

Find the shortest distance from C to D and if it is impossible to reach node D from C then return -1.

You are expected to do it in Time Complexity of O(A + M).

**Note**:

There are no self-loops in the graph.

No multiple edges between two pair of vertices.

The graph may or may not be connected.

Nodes are Numbered from 0 to A-1.

Your solution will run on multiple testcases. If you are using global variables make sure to clear them.

**Notes:**

- Make all edges same cost by splitting edge and adding a dummy node in between

```java
public class Solution {
    public int solve(int A, int[][] B, int C, int D) {

        int count = A;
        // Count of edges having cost 2
        for(int i=0; i<B.length; i++) {
            if(B[i][2] == 2) count++;
        }
        // Create adjacencyMatrix with size including additional dummy nodes
        ArrayList<ArrayList<Integer>>[]  adjacencyMatrix = new ArrayList[count+1]; 
        for(int i=0; i<=count; i++) {
            adjacencyMatrix[i] = new ArrayList<ArrayList<Integer>>();
        }

        createAdjacencyMatrix(A, B, adjacencyMatrix);
        boolean[] visited = new boolean[adjacencyMatrix.length];

        return bfs(C, D, adjacencyMatrix, visited);
    }

    private int bfs(int C, int D, ArrayList<ArrayList<Integer>>[] adjacencyMatrix, boolean[] visited) {

        Queue<ArrayList<Integer>> queue = new LinkedList<ArrayList<Integer>>();
        // Add source node to the Queue
        queue.add(new ArrayList<>(Arrays.asList(0, C)));
        visited[C] = true;       

        // Return 0 is the source and dest nodes are same
        if(C == D) return 0;   

        while(!queue.isEmpty()) {
            ArrayList<Integer> curNodeData = queue.poll();
            int distance = curNodeData.get(0);
            int curNode = curNodeData.get(1);

            for(int i=0; i<adjacencyMatrix[curNode].size(); i++) {                
                ArrayList<Integer> list = adjacencyMatrix[curNode].get(i);
                int node = list.get(1);
                // Check childNode is already visited or not
                if(visited[node] == false) {   
                    // If this child node is the current node, return distance+1                 
                    if(node == D) {
                        return distance+1;
                    }
                    // Add the child node with distance+1 as cost 
                    queue.add(new ArrayList<>(Arrays.asList(distance+1, node)));
                    // Mark child node is visited
                    visited[node] = true;
                }             
            }
        }  
        // Return -1 as the dest node is not reachable
        return -1;      
    }

    private ArrayList<ArrayList<Integer>>[] createAdjacencyMatrix(int A, int[][] B, ArrayList<ArrayList<Integer>>[] adjacencyMatrix) {

        int dummy = A+1;

        for(int i=0; i<B.length; i++) {
            int fromNode = B[i][0];
            int toNode = B[i][1];

            ArrayList<Integer> tempList = new ArrayList<>();

            // Split the edge, if the distance is 2 to make it uniform
            if(B[i][2] == 2) {
                adjacencyMatrix[fromNode].add(new ArrayList<>(Arrays.asList(1, dummy)));
                adjacencyMatrix[dummy].add(new ArrayList<>(Arrays.asList(1, fromNode)));

                adjacencyMatrix[toNode].add(new ArrayList<>(Arrays.asList(1, dummy)));
                adjacencyMatrix[dummy].add(new ArrayList<>(Arrays.asList(1, toNode)));
                dummy++;
            } else {
                adjacencyMatrix[fromNode].add(new ArrayList<>(Arrays.asList(1, toNode)));
                adjacencyMatrix[toNode].add(new ArrayList<>(Arrays.asList(1, fromNode)));
            }            
        }
        return adjacencyMatrix;
    }
}
```

> TC: $$

#### Prims Algorithm/MST with different edge cost

**Spanning Tree** - The tree of N-1 edges which spans across all vertices and connects all of them is called Spanning Tree

**Minimum Spanning Tree** - The Spanning Tree with **minimum cost** is called **Minimum Spanning Tree (MST)**

There are two approaches to create MST -

1. Prims algorithm

2. Kruskal Algorithm
* **Prim's algorithm** is used to find a **Minimum Spanning Tree (MST)** of a **connected, weighted, undirected graph**.

* Start from any vertex and repeatedly choose the **minimum-weight edge that connects a visited vertex to an unvisited vertex**.

**Notes:**

* When all the costs are unique, then we get a single unique MST

* If there is  repetition in costs, then there can be **multiple MSTs with minimum cost**.

**Prim's flow:**

                *Start with any node
                               ↓
                Find minimum-weight edge
                               ↓
                Add the new node to MST
                               ↓
                Add its edges to PriorityQueue
                               ↓
                Pick minimum edge again
                               ↓
                    Repeat*

##### Construction Cost

**Problem Description**  

Flipkart has ‘**A**’ local distribution centers located across a large metropolitan city. Each distribution center needs to be interconnected through roads to facilitate efficient movement of goods. The cost of constructing a road between any two distribution centers is represented by the weight of the edge connecting them.  

Given a graph with ‘**A**’ nodes representing the distribution centers and **C** weighted edges representing the possible roads between them, your task is to **find the minimum total cost of constructing roads such that every distribution center can be reached from the first distribution center.**  

Cost Calculation:  
The cost of constructing the roads is the sum of the weights of the edges selected for the construction.  

**NOTE**: Return the answer modulo **10^9+7** as the answer can be large.

**Notes:**

* Create Adjacency Matrix

* Use minHeap to maintain the nodes based on the cost in ascending order

* PriorityQueue of ArrayList of Integer. Cost and toNode

<img src="Notes-Advanced%20DSA4/2026-10-24-10-55-30-image.png" title="" alt="" width="607">

```java
public class Solution {
    public int solve(int A, int[][] B) {
         ArrayList<ArrayList<Integer>>[] adjacencyArr = new ArrayList[A+1];
         boolean[] visited = new boolean[A+1];
         constructAdjacencyMatrix(A, B, adjacencyArr);
         return findMinDistance(A, B, adjacencyArr, visited);
    }

    private int findMinDistance(int A, int[][] B, ArrayList<ArrayList<Integer>>[] adjacencyArr, boolean[] visited) {
        int ans = 0;
        // Sorting Logic for minHeap
        PriorityQueue<ArrayList<Integer>> queue = new PriorityQueue<ArrayList<Integer>>((x,y) -> x.get(0)-y.get(0));
        if( adjacencyArr[1] != null) {
            for(int i=0; i < adjacencyArr[1].size(); i++) {                
                queue.add(adjacencyArr[1].get(i));             
            } 
            visited[1] = true;
        }
        int mod = 1000000007;

        while(!queue.isEmpty()) {

            // Remove minElement from minHeap
            ArrayList<Integer> curNode = queue.poll();

            // if node is already visited, skip it
            if(visited[curNode.get(1)] == true) {
               continue; 
            }            

            // Keep adding the total distance required
            ans = (ans%mod + curNode.get(0)%mod)%mod;

            int toNode = curNode.get(1);            
            visited[toNode] = true; // Mark as visited

            if(adjacencyArr[toNode] != null) {
                    adjacencyArr[toNode].forEach(node -> {
                        // Add node only if node is not visited
                        if(visited[node.get(1)] == false) {
                            queue.add(node);
                        }
                    });
            }
        }
        return ans;
    }

    private void constructAdjacencyMatrix(int A, int[][] B, ArrayList<ArrayList<Integer>>[] adjacencyArr) {
        for(int i=0; i<B.length; i++) {
            int fromNode = B[i][0];
            int toNode = B[i][1];
            int weight = B[i][2];
            if(adjacencyArr[fromNode] != null) {
                adjacencyArr[fromNode].add(new ArrayList<>(Arrays.asList(weight, toNode)));
            } else {
                ArrayList<Integer> newList = new ArrayList<>(Arrays.asList(weight, toNode));
                //adjacencyArr[fromNode] = new ArrayList<>(newList);
                ArrayList<ArrayList<Integer>> newList1 = new ArrayList<ArrayList<Integer>>();
                newList1.add(newList);
                adjacencyArr[fromNode] = newList1;
            }    
            if(adjacencyArr[toNode] != null) {
                adjacencyArr[toNode].add(new ArrayList<>(Arrays.asList(weight, fromNode)));
            } else {
                ArrayList<Integer> newList = new ArrayList<>(Arrays.asList(weight, fromNode));
                //adjacencyArr[fromNode] = new ArrayList<>(newList);
                ArrayList<ArrayList<Integer>> newList1 = new ArrayList<ArrayList<Integer>>();
                newList1.add(newList);
                adjacencyArr[toNode] = newList1;
            }                       
        }    
    }
}
```

> TC: $O(ElogE + 2E) => O(ElogE)$
> 
> SC: $O(V + E)$ V -> visited[], E -> Stack size

#### Djikstra Algorithm/MST with different edge cost

![](Notes-Advanced%20DSA4/2026-10-24-13-36-23-image.png)

**Problem Description**  

Given a **weighted undirected graph** having **A nodes** and **M weighted edges**, and a **source node C**.

You have to **find an integer array D of size A** such that:

- D[i]: Shortest distance from the **C node** to **node i**.
- If **node i** is not reachable from C then **-1**.

**Note:**

- There are no self-loops in the graph.
- There are no multiple edges between two pairs of vertices.
- The graph may or may not be connected.
- Nodes are numbered from 0 to A-1.
- Your solution will run on multiple test cases. If you are using global variables, make sure to clear them.

**Problem Constraints**  

1 <= A <= 1e5

0 <= B[i][0],B[i][1] < A

0 <= B[i][2] <= 1e3

0 <= C < A

```java
public class Solution {
    public int[] solve(int A, int[][] B, int C) {
        // Create adjacencyMatrix with sizeincluding additional dummy nodes
        ArrayList<ArrayList<Integer>>[]  adjacencyMatrix = new ArrayList[A]; 
        for(int i=0; i<A; i++) {
            adjacencyMatrix[i] = new ArrayList<ArrayList<Integer>>();
        }
        createAdjacencyMatrix(A, B, adjacencyMatrix);
        // Maintain shortest distance in array
        int[] shortDist = new int[A];

        bfs(adjacencyMatrix, C, shortDist);

        // This is required if the node is not reachable
        for(int i=0; i<shortDist.length; i++) {
            if(shortDist[i] == Integer.MAX_VALUE) {
                shortDist[i] = -1;
            }
        }
        return shortDist;
    }

    private void bfs(ArrayList<ArrayList<Integer>>[] adjacencyMatrix, int C, int[]  shortDist) {
        // minHeap will be sorted based on weight
        PriorityQueue<ArrayList<Integer>> minHeap = new PriorityQueue<>((a,b) -> Integer.compare(a.get(0), b.get(0)));

        // Initialize the shortDist array                                
        Arrays.fill(shortDist, Integer.MAX_VALUE);
        // srcNode distance for srcNode itself is 0
        shortDist[C] = 0; 

        // Add the distance 0 and src node
        minHeap.add(new ArrayList<>(Arrays.asList(0, C))); 

        while(!minHeap.isEmpty()) {
            ArrayList<Integer> list = minHeap.poll();
            int distance = list.get(0);
            int curNode = list.get(1);

            // Skip if the current distance > already stored shortDist
            if(shortDist[curNode] < distance) continue;

            // Store the distance as shortDist. this can be overridden later if another shortDist is found
            shortDist[curNode] = distance;

            for(int i=0; i<adjacencyMatrix[curNode].size(); i++) {
                ArrayList<Integer> childNodeData = adjacencyMatrix[curNode].get(i);
                int childNodeDistance = childNodeData.get(0);
                int childNode = childNodeData.get(1);
                // If already stored shortDist > current new Distance
                if(shortDist[childNode] > distance + childNodeDistance) {
                    shortDist[childNode] = distance + childNodeDistance;
                    minHeap.add(new ArrayList<>(Arrays.asList(distance+childNodeDistance, childNode)));
                }
            }
        }
    }

    private void createAdjacencyMatrix(int A, int[][] B, ArrayList<ArrayList<Integer>>[] adjacencyMatrix) {

        for(int i=0; i<B.length; i++) {
            int fromNode = B[i][0];
            int toNode = B[i][1];
            int cost = B[i][2];
            adjacencyMatrix[fromNode].add(new ArrayList<>(Arrays.asList(cost, toNode)));
            adjacencyMatrix[toNode].add(new ArrayList<>(Arrays.asList(cost, fromNode)));
            }           
        }

}
```

> **TC, SC:**
> 
> <img src="Notes-Advanced%20DSA4/2026-10-24-12-58-21-image.png" title="" alt="" width="731"> 

#### Topological Sort

* Linear ordering of nodes in such a way if there is edge from node i -> node j, i will be on the left of j.

* Topological sort is possible on  **Directed Acyclic Graph (DAG).** Topological sort is not possible in Cyclic Graph.

* Multiple Topological sort can exist

**Problem Description**  

Given an directed acyclic graph having **A** nodes. A matrix **B** of size `M x 2` is given which represents the **M** edges such that there is a edge directed from node **B[i][0]** to node **B[i][1]**.

Topological sorting for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for every directed edge uv, vertex u comes before v in the ordering. Topological Sorting for a graph is not possible if the graph is not a DAG.

Return the **topological ordering** of the graph and if it doesn't exist then return an empty array.

If there is a solution return the correct ordering. If there are multiple solutions print the **<u>lexographically smallest one</u>**.

Ordering (a, b, c) is said to be lexographically smaller than ordering (e, f, g) if a < e or if(a==e) then b < f and so on.

**NOTE:**

- There are no self-loops in the graph.
- The graph may or may not be connected.
- Nodes are numbered from 1 to A.
- Your solution will run on multiple test cases. If you are using global variables make sure to clear them.
- <img src="Notes-Advanced%20DSA4/2026-10-24-13-48-09-image.png" title="" alt="" width="606">

```java
public class Solution {
    public int[] solve(int A, int[][] B) {

        // Create adjacencyMatrix
        ArrayList<Integer>[] adjacencyMatrix = new ArrayList[A+1];
        for(int i=1; i<=A; i++) {
            adjacencyMatrix[i] = new ArrayList<>();
        }
        for(int i=0; i<B.length; i++) {
            int fromNode = B[i][0];
            int toNode = B[i][1];
            adjacencyMatrix[fromNode].add(toNode);
        }

        // Create inDegArray and maintain In-degree of all nodes in it
        int[] inDegArray = new int[A+1];

        for(int i=1; i<adjacencyMatrix.length; i++) {
            for(int j=0; j<adjacencyMatrix[i].size(); j++) {
                inDegArray[adjacencyMatrix[i].get(j)]++;
            }
        }
        // Use PriorityQueue to keep the nodes. Problem asks for lexographically smallest in the beginning 
        PriorityQueue<Integer> queue = new PriorityQueue<>();

        // Add all nodes having indegree 0
        for(int i=1; i<inDegArray.length; i++) {
            if(inDegArray[i] == 0) {
                queue.add(i);
            }
        }

        List<Integer> ans = new ArrayList<>();

        while(!queue.isEmpty()) {
            int curNode = queue.poll();

            // Add current element to ans
            ans.add(curNode);

            // Reduce the indegree of all child nodes of curNode
            for(int i=0; i<adjacencyMatrix[curNode].size(); i++) {
                int childNode = adjacencyMatrix[curNode].get(i);
                inDegArray[childNode]--;
                // If child node indegree became 0, add it to Queue
                if(inDegArray[childNode] == 0) {
                    queue.add(childNode);
                }
            }                                    
        }
        // return empty array, as all nodes are not covered. It must be a split graph
        if(ans.size() < A) return new int[0];
        else return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

> TC: $O(2V+2E) -> O(V+E)$
> 
> SC: $O(2V)$ -> indegarr + queue
