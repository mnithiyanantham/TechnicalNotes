## Maths: Combinatorics Basics & Prime Numbers

**Permutation**

* Arrangement of Objects.

* Order matters => $(i,j) != (j,i)$

Given N distinct elements, how many ways we can arrange them - 

                        $N * N-1 * N-2 * N-3 * ..... * 3 * 2 * 1 = N!$

Number of ways to arrange R elements out of N elements -

                         $N * N-1 * N-2 * N-3 * ..... * N-(r-1) = N!$

                        **$NPR=N! / (N-r)!$**

<img title="" src="Notes-Advanced%20DSA3/2026-03-28-22-51-36-image.png" alt="" width="772">

| Combination           | Permutation         |
| --------------------- | ------------------- |
| Select                | Select and Arrange. |
| Order does not matter | Order matters       |
| $(i,j) == (j,i)$      | $(i,j) != (j,i)$    |

**Combinataion**

* Number of ways to select elements

* Order does not matter

<img title="" src="Notes-Advanced%20DSA3/2026-03-28-23-04-13-image.png" alt="" width="590">

<img title="" src="Notes-Advanced%20DSA3/2026-03-28-23-03-50-image.png" alt="" width="571">

**Properties of Combination**

<img src="Notes-Advanced%20DSA3/2026-03-28-23-05-09-image.png" title="" alt="" width="581">

![](Notes-Advanced%20DSA3/2026-03-28-23-16-43-image.png)

### Sieve of Eratosthenes

* This technique is used to find the Prime numbers

* Use` boolean array` to mark the numers prime or not

* Initiate the array with default value `false`

* Iterate from `2`, mark the **multiplications of the element** as not prime. Iterate till `sqrt(N)`

* At the end, print numbers which are marked as Prime

<img src="Notes-Advanced%20DSA3/2026-03-28-23-42-46-image.png" title="" alt="" width="667">

<img title="" src="Notes-Advanced%20DSA3/2026-03-28-23-43-34-image.png" alt="" width="420">

<img src="Notes-Advanced%20DSA3/2026-03-28-23-43-13-image.png" title="" alt="" width="393">

**Problems**

### Find All Primes

Find all Primes lesser than integer A.

* Use **Sieve of Eratosthenes** method
  
  * Initialize a array with **boolean**
  
  * Iterate through the lements till **sqrt(N)**
  
  * Mark the**multiplies of each element as `false`**. Because these can not be Prime numbers

```java
    public ArrayList<Integer> solve(int A) {
        ArrayList<Integer> ans = new ArrayList<Integer>();
        // default value for boolean array is false. Initializing with true value requires a for loop. Avoided by using isNotPrime
        boolean[] isNotPrime = new boolean[A+1];

        for(int i=2; i <= (A/i); i++) {
            if(isNotPrime[i] == false) {
                for(int j = i*i; j <= A; j+=i) {
                    isNotPrime[j] = true;
                }
            }
        }
        for(int i=2; i<=A; i++) {
            if(!isNotPrime[i]) {
                ans.add(i);                
            }
        }
        return ans;
    }
```

### Prime Sum

Given an even number **A** ( greater than 2 ), return two prime numbers whose sum will be equal to the given number.

If there is more than one solution possible, return the lexicographically smaller solution.

If [a, b] is one solution with a <= b, and [c,d] is another solution with c <= d, then 
[a, b] < [c, d], If a < c OR a==c AND b < d. 

**NOTE:** A solution will always exist. Read **Goldbach's conjecture**.

**Psedocode:**

* Follow `Sieve of Eratosthenes` algorithm - find Prime numbers, add them in a `LinkedHashMap`

* `LinkedHashMap` will help in maitaining the insertion order. So to get the least element, as per problem.

* From `LinkedHashMap` , pick up element such that `(i, A-i)` are Prime. Return these immediately

```java
    public ArrayList<Integer> primesum(int A) {
        ArrayList<Integer> ans = new ArrayList<Integer>();
        HashMap<Integer, Boolean> hm = getPrimeNumbers(A);

        for(int a:hm.keySet()) {
            if(hm.containsKey(A-a)) {
                ans.add(a);
                ans.add(A-a);
                break;
            }
        }
        return ans;
    }

    private HashMap<Integer, Boolean> getPrimeNumbers(int A) {
        HashMap<Integer, Boolean> hm = new LinkedHashMap<>();
        // default value for boolean array is false. Initializing with true value requires a for loop.
        // Avoided by using isNotPrime
        boolean[] isNotPrime = new boolean[A+1];

        for(int i=2; i <= (A/i); i++) {
            if(isNotPrime[i] == false) {
                for(int j = i*i; j <= A; j+=i) { // Start from i*i. Increment step by i
                    isNotPrime[j] = true;
                }
            }
        }
        for(int i=2; i<=A; i++) {
            if(!isNotPrime[i]) {
                hm.put(i, true);                
            }
        }
        return hm;
    }
```

### Compute nCr % m

**Problem Description**  

Given three integers **A**, **B,** and **C**, where **A** represents **n**, **B** represents **r,** and **C** represents **m**, find and return the value of **nCr % m** where **nCr % m = (n!/((n-r)!*r!))% m**.

**x!** means factorial of x i.e. **x! = 1 * 2 * 3... * x.**

**Problem Constraints**  

1 <= A * B <= 106

1 <= B <= A

1 <= C <= 106

**Psedocode**

* Use 2D array to maintain the `nCr `value of `n-1Cr, n-1Cr-1` elements

```java
    public int solve(int A, int B, int C) {
        int ans[][] = new int[A+1][B+1];

        for(int i=0; i<= A; i++) {

            // Iterate till smallest i or r
            int min= i < B ? i : B;

            for(int j=0; j<=min; j++) {
                if(j==0 || i==j) {
                    ans[i][j] = 1;
                } else {
                    ans[i][j] = (ans[i-1][j-1]%C + ans[i-1][j]%C)%C;
                }
            }
        }
        return ans[A][B] % C;
    }  
```

<img src="Notes-Advanced%20DSA3/2026-03-29-16-05-02-image.png" title="" alt="" width="751">

## Lab session  Prime Numbers & 2 Pointers

### Pascal's Triangle

Write a program to print the pascal triangle up to **A** rows.

<img src="Notes-Advanced%20DSA3/2026-04-03-00-01-12-image.png" title="" alt="" width="734">

**Psedocode:**

* If `j==0 || i==j` then `nCr=1` 

* `nCr[i][j] = nCr[i-1][j-1] + nCr[i-1][j]`

```java
    public int[][] solve(int A) {
        int[][] nCr = new int[A][A];
        for(int i=0; i<A; i++) {
            for(int j=0; j<=i; j++) {
                if(j==0 || i==j)
                    nCr[i][j] = 1;
                else
                    nCr[i][j] = nCr[i-1][j-1] + nCr[i-1][j];                
            }
            for(int j=i+1; j<A; j++) {
                nCr[i][j] = 0;                
            }
        }
        return nCr;
    }
```

TC: $O(N^2)$

SC: $O(N)$

### Least Prime Factor

For a given number N, find the least Prime factor for all number less than N.

* Create arr[N], keep the value as the index itself.

* Iterate (i) through the multiple of prime numbers starting from 2.

* In all the indexes which are multiple of i, assign i itself -> this will assign the minimum prime factor.

<img title="" src="Notes-Advanced%20DSA3/2026-04-03-00-18-35-image.png" alt="" width="679">

<img src="Notes-Advanced%20DSA3/2026-04-03-00-19-38-image.png" title="" alt="" width="734">

## Lab session on Maths and 2 Pointers

### Excel Column Title - TODO

Given a positive integer **A**, return its corresponding column title as it appears in an Excel sheet.

For example:

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB

```java

```

### Container With Most Water - TODO

Given **N** non-negative integers A[0], A[1], ..., A[N-1] , where each represents a point at coordinate (i, A[i]).

**N** vertical lines are drawn such that the two endpoints of line i is at (i, A[i]) and (i, 0).

Find two lines, which together with x-axis forms a container, such that the container contains the most water. You need to return this maximum area.

**Note:** You may not slant the container. It is guaranteed that the answer will fit in integer limits.

<img src="Notes-Advanced%20DSA3/2026-09-24-20-52-26-image.png" title="" alt="" width="839">

<img src="Notes-Advanced%20DSA3/2026-09-24-20-52-09-image.png" title="" alt="" width="670">

<img src="Notes-Advanced%20DSA3/2026-09-24-20-51-16-image.png" title="" alt="" width="630">

## Sorted Permutation Rank - TODO

Given a string **A**. Find the rank of the string amongst its permutations **sorted lexicographically**.  
Assume that no characters are repeated.

**Note:** The answer might not fit in an integer, so return `your answer % 1000003`

- [ ] check approach if char repeats

![](Notes-Advanced%20DSA3/2026-09-24-20-35-34-image.png)

<img src="Notes-Advanced%20DSA3/2026-09-24-20-36-14-image.png" title="" alt="" width="688">

## Pairs with Given Difference - TODO

Given an one-dimensional integer array **A** of size **N** and an integer **B**.

**Count all distinct pairs with difference equal to B**.

Here a pair is defined as an integer pair (x, y), where x and y are both numbers in the array and their absolute difference is **B**.

<img src="Notes-Advanced%20DSA3/2026-09-24-20-44-52-image.png" title="" alt="" width="613">

## Backtracking

**Backtracking**- Technique to try multiple solutions recursively  by trying to build a solution until its correct and neglecting any invliad solutions.

<img src="Notes-Advanced%20DSA3/2026-04-07-23-48-38-image.png" title="" alt="" width="566">

### Generate all Parentheses II

```java
    public String[] generateParenthesis(int A) {
        List<String> strList = new ArrayList<>();
        generate(A, "", 0, 0, strList);
        // Convert List to array
        return strList.toArray(new String[0]);
    }
    private void generate(int A, String str, int o, int c, List<String> strList) {
        // Base case - return if str size is 2N
        if(str.length() == 2*A) {
            strList.add(str);
        }
        // Add opening bracket, if opening is less than N
        if(o < A) {
            generate(A, str + "(", o+1, c, strList);
        }
        // add closing bracker if closing < opening
        if(c < o) {
            generate(A, str + ")", o, c+1, strList);
        }
    }
```

> :bulb: `return strList.toArray(new String[0])` -> **Converts List to Array**

<img src="Notes-Advanced%20DSA3/2026-09-24-12-16-14-image.png" title="" alt="" width="575">

### Subsets

**Subsets** - Can be in any order, but numbers can not repeat

**subsequence**- Same sequence as in the original array

<img src="Notes-Advanced%20DSA3/2026-09-24-12-36-04-image.png" title="" alt="" width="602">

<img src="Notes-Advanced%20DSA3/2026-09-24-12-36-28-image.png" title="" alt="" width="601">

### Generate Subsets

**Problem Description**

Given a set of distinct integers **A**, return all possible subsets that can be formed from the elements of array **A**.

<img title="" src="Notes-Advanced%20DSA3/2026-04-08-09-47-46-image.png" alt="" width="761">

```java
class Solution {

    ArrayList<ArrayList<Integer>> ansList = new ArrayList<ArrayList<Integer>>();

    public ArrayList<ArrayList<Integer>> subsets(ArrayList<Integer> A) {
        createSubset(A, 0, new ArrayList<>());
        return ansList;
    }
    private void createSubset(ArrayList<Integer> A, int index, ArrayList<Integer> ans) {
        // Base case - Add to anslist if index become full
        if(index == A.size()) {
            ansList.add(new ArrayList<>(ans));            
            return;
        }
        // Add current element to ans
        ans.add(A.get(index));
        createSubset(A, index+1, ans);
        // Remove current element from ans and move onto next element
        ans.remove(ans.size()-1);
        createSubset(A, index+1, ans);
    }
}
```

> TC: $O(2powN)$
> 
> SC: $O(N+N)$

<img src="Notes-Advanced%20DSA3/2026-09-24-12-52-26-image.png" title="" alt="" width="543">

### Generate possible Permutations - COMPLEX

<img src="Notes-Advanced%20DSA3/2026-04-08-09-54-43-image.png" title="" alt="" width="825">

**Notes:**

* For each index, iterate it through all elements. Index decides the place where we are adding current char.

* Maintain boolean array of N size to indicate the particular char is used or not.

* After using the element, mark it as unused in this array

```java
public class Solution {       
    public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> A) {
         ArrayList<ArrayList<Integer>> ansList = new ArrayList<ArrayList<Integer>>();         
         constructPermutation(A, 0, A.size(), new boolean[A.size()], new int[A.size()], ansList);
         return ansList;
    }
    private void constructPermutation(ArrayList<Integer> A, int index, int N, boolean[] isUsed, int[] temp, ArrayList<ArrayList<Integer>> ansList) {
        // Add to ans list once index becomes N
        if(index == N) {
            ArrayList<Integer> tempList = new ArrayList<>();
            for(int n:temp) {
                tempList.add(n);            
            }
            ansList.add(tempList);
        }
        for(int i=0; i<N; i++) {
            // use it only if it is unused
            if(isUsed[i] == false) {
                temp[index]= A.get(i);
                // Mark as used
                isUsed[i] = true;
                constructPermutation(A, index+1, N, isUsed, temp, ansList);
                // Mark as unused for the next iteration
                isUsed[i]=false;
            }            
        }
    }
}
```

![](Notes-Advanced%20DSA3/2026-09-24-17-36-52-image.png)

> TC: $O(N! * N)$ -> at the end of tree, $N!$ functions calls are there. Each time, we iterate for $N$ times
> 
> SC: $O(N+N)$ -> $N$ -> Array size, $N$ -> Stack size. Each path in the tree has N number calls through it

### Climbing Staircase

You are climbing a staircase and it takes **A** steps to reach the top.

Each time you can either climb **1** or **2** steps. In how many **distinct ways** can you climb to the top?  

You need to return all the **distinct ways** to climb to the top in **lexicographical order.**

**Notes:**

* Use back tracking using recursion

**Using  List**

```java
public class Solution {
    ArrayList<ArrayList<Integer>> ansList = new ArrayList<ArrayList<Integer>>();

    public ArrayList<ArrayList<Integer>> WaysToClimb(int A) {
        return findWays(A, new ArrayList<>());
    }

    private ArrayList<ArrayList<Integer>> findWays(int A, ArrayList<Integer> ans) {
        // Invalid path
        if( A < 0) {
            return null;
        }
        // Valid path traversal, add to answer
        if(A == 0) {
            ansList.add(new ArrayList<Integer>(ans));
        }
        // Take 1 step
        ans.add(1);
        findWays(A-1, ans);
        ans.remove(ans.size()-1);
        // Take 2 steps
        ans.add(2);
        findWays(A-2, ans);
        ans.remove(ans.size()-1);
        return ansList;
    }
}
```

> TC: $O(2powN)$
> 
> SC: $O(N+N)$

**Using Strings**

* Each method call will use it own String.

* So removing last step is not required.

* It will result in more space

```java

```

### Find unique paths in matrix/Print All Maze Paths

You are given the dimensions of a rectangular board of size **A** **x B**. You need to print all the possible paths from **top-left** corner to **bottom-right** corner of the board.  

You can only move down (denoted by **'D')** or right (denoted by **'R')** at any point in time.  

**NOTE :** You need to return all the paths in **Lexicographically sorted order.** A lexicographic order is an arrangement of characters in alphabetical order.

```java
public class Solution {
    ArrayList<String> ans = new ArrayList<>();

    public ArrayList<String> PrintAllPaths(int A, int B) {
        findPaths(0, 0, A, B, new StringBuilder());
        return ans;
    }

    private void findPaths(int row, int column, int A, int B, StringBuilder temp) {
        // Base case - if bottom right cell is reached
        if(row == A-1 && column == B-1) {
            ans.add(temp.toString());
        }
        // First traverse through the rows down the box, until end of row reached
        if(row < A) {
            temp.append("D");
            findPaths(row+1, column, A, B, temp);
            // Delete the last traversal
            temp.deleteCharAt(temp.length()-1);            
        }
        // Secondly traverse through the columns right side, until end of column reached
        if(column < B) {
            temp.append("R");
            findPaths(row, column+1, A, B, temp);
            // Delete the last traversal
            temp.deleteCharAt(temp.length()-1);      
        }  
    }
}
```

> TC:
> 
> SC:
> 
> - [ ] Analyse this

### Shortest path in a Binary Maze with Hurdles

https://www.scaler.com/academy/mentee-dashboard/class/460307/assignment/problems/125143/?navref=cl_pb_nv_tb

Given an **MxN** matrix where each element can either be **0** or **1**. We need to find the length of shortest path between a given **source cell** to a **destination cell**.  

A cell with value **0** denotes that it's a hurdle. The path can only be created out of the cells with values **1**.  
If **NO** path exists then print **-1.**  

- The matrix **A** is given as input of size **M x N.**
- The coordinates of **Source cell** are given by **B, C.**
- The coordinates of **Destination** **cell** are given by **D, E**.

**Notes:**

* This is a pure Recursion, with bruteforce method. thre is no any efficiency improved

```java
public class Solution {
    int ans = Integer.MAX_VALUE;

    public int FindShortestPath(int[][] A, int B, int C, int D, int E) {

        int N = A.length;
        int M = A[0].length;
        // Default value will be boolean only
        boolean[][] visited = new boolean[N][M];

        findPath(A, B, C, D, E, N, M, visited, 0);
        return ans != Integer.MAX_VALUE ? ans : -1;
    }

    private void findPath(int[][] A, int B, int C, int D, int E, int N, int M, boolean[][] visited, int steps) {
        // Base case - Must not exceed the boundaries
        if(B < 0 || B >= N || C <0 || C >= M) {
            return ;
        }
        // If cell is is blocking cell
        if(A[B][C] == 0) {
            return ;
        }
        // Skip is cell is already visited
        if(visited[B][C]) {
            return ;
        }
        // If dest is reached, compare steps with ans
        if(B == D && C==E) {
            ans = Math.min(ans, steps);
            return;
        }

        visited[B][C] = true;
        // go up
        findPath(A, B-1, C, D, E, N, M, visited, steps+1);
        // go down
        findPath(A, B+1, C, D, E, N, M, visited, steps+1);
        // go left
        findPath(A, B, C-1, D, E, N, M, visited, steps+1);
        // go right
        findPath(A, B, C+1, D, E, N, M, visited, steps+1); 

        visited[B][C] = false;                       
    }
}
```

> TC: 
> 
> SC: 
> 
> - [ ] analyse this

## Linked List: Sorting and Problems

### Find middle element of LL

<img src="Notes-Advanced%20DSA3/2026-04-11-23-05-58-image.png" title="" alt="" width="756">

**Problem:**

Given a linked list of integers, find and return the middle element of the linked list.

**NOTE:** If there are **N** nodes in the linked list and N is even then return the (N/2 + 1)th element.

**Psedocode:**

* Use `slow and fast pointer` method

* `f.next != null && f.next.next == null)` -> if `true`, it is `even `sized list, if `false`, it is `odd` sized

* Odd -> f reaches last node, s reaches middle node

* even -> f reaches second last node, s reaches middle node

```java
public class Solution {
    public int solve(ListNode A) {
        int size = 0;
        ListNode temp = A;
        ListNode s=A;
        ListNode f = A;
        while(f.next != null && f.next.next != null) {
            s = s.next;
            f = f.next.next;
        }
        if(f.next != null && f.next.next == null) { // even sized list
            return s.next.val;
        } else { // odd sized list
            return s.val;
        }
    }
}
```

### Palindrome list

**Problem Description**  

Given a singly linked list **A**, determine if it's a palindrome. Return **1** or **0,** denoting if it's a palindrome or not, respectively.

Psedocode:

* Find the middle element.

* Break the LL into two based of middle element.

* Reverse the second LL.

* Until any one LL becomes NULL, the char must match in both LL. If it does not match, it is not Palindrome

```java
public class Solution {
    public int lPalin(ListNode A) {

        if(A.next == null) return 1;    
        else if(A.next.next == null && A.val == A.next.val) return 1;
        else if(A.next.next == null && A.val != A.next.val) return 0;
        ListNode H1 = A;
        ListNode mid = findMiddleElement(A);
        ListNode H2 = mid.next; // start of the second LL
        mid.next = null; // break the first half            
        H2 = reverseLL(H2);

        return compareLL(H1, H2);
    }
    ListNode findMiddleElement(ListNode A) {
        ListNode s = A;
        ListNode f = A;
        while(f.next != null && f.next.next != null) {
            s = s.next;
            f = f.next.next;
        }
        return s;// in case of even sized LL, return the first middle element only
    }
    ListNode reverseLL(ListNode list) {
        ListNode next;
        ListNode tail = null;
        ListNode current = list;
        while(current != null) {
            next = current.next;
            current.next = tail;
            tail = current;
            current = next;
        }
        return tail;
    }
    int compareLL(ListNode H1, ListNode H2) {
        while(H1 != null && H2 != null) { // if any becomes null, it is palindrome
            if(H1.val != H2.val) return 0;            
            H1 = H1.next;
            H2 = H2.next;
        }
        return 1;
    }
}
```

### Merge Sort LL

**Problem Description**  

Sort a linked list, **A** in **O(n log n)** time.

<img src="Notes-Advanced%20DSA3/2026-09-22-15-07-46-image.png" title="" alt="" width="636"><img title="" src="Notes-Advanced%20DSA3/2026-04-12-17-58-51-image.png" alt="" width="497">

```java
public class Solution {
    public ListNode sortList(ListNode A) {

        if(A == null || A.next == null) return A;

        ListNode mid = findMiddleElement(A);
        ListNode h2 = mid.next;
        mid.next = null;
        ListNode l = sortList(A);
        ListNode r = sortList(h2);
        ListNode mh = mergeLL(l, r);
        return mh;        
    }
    private ListNode mergeLL(ListNode h1, ListNode h2) {
        ListNode temp = new ListNode(-1);
        ListNode ans = temp;
        while( h1 != null && h2 != null) {
            if(h1.val < h2.val) {
                temp.next = h1;
                h1 = h1.next;
            } else {
                temp.next = h2;
                h2 = h2.next;
            }
            temp = temp.next;
        }
        if(h1 != null) temp.next = h1;
        else temp.next = h2;
        return ans.next;
    }
    private ListNode findMiddleElement(ListNode head) {
        ListNode s = head;
        ListNode f = head;
        while(f.next != null && f.next.next != null) {
            s = s.next;
            f = f.next.next;
        }
        return s;
    }
}
```

## Doubly LL and Deteing Loops

#### LRU implementation

<img src="Notes-Advanced%20DSA3/2026-04-12-18-15-32-image.png" title="" alt="" width="573">

* Use Doubly LL

* Keep Least recently used in **head**, Most recently used in **tail**. This helps to remove LRU quickly

* Once the hit is made, move the item to tail

* Use Hashmap to check the key already present or not

<img src="Notes-Advanced%20DSA3/2026-09-22-23-46-32-image.png" title="" alt="" width="582">

```java
public class Solution {
    // Create ListNode for storing elements
    private class ListNode {
        int key;
        int val;
        ListNode prev;
        ListNode next;

        ListNode(int val, int key) {
            this.key = key;
            this.val = val;
            this.prev = null;
            this.next = null;
        }
    }

    HashMap<Integer, ListNode> hm = null;
    int capacity = 0;
    ListNode head = null;
    ListNode tail = null;

    public Solution(int capacity) {
        // Initiate hashmap with capacity
        this.hm = new HashMap<>(capacity);
        // Initiate head, tail with dummy values and keys.
        head = new ListNode(-1, -1);
        tail = new ListNode(-1, -1);
        head.next = tail;
        tail.prev = head;
        this.capacity = capacity;
    }

    public int get(int key) {
        if(hm.containsKey(key)) {
            ListNode node = hm.get(key);
            moveBeforeTail(node);
            return node.val;
        } else return -1;
    }

    public void set(int key, int value) {
        // if element is not present, add it newly
        if(hm.containsKey(key)) {
            ListNode node = hm.get(key);
            node.val = value;
            moveBeforeTail(node);
        } else {
            // if capacity is full, remove first element and add
            if(hm.size() == capacity) deleteFirstNode();       
            ListNode newNode = new ListNode(value, key);
            insertBeforeTail(newNode);
            hm.put(key, newNode);            
        }
    }
    // Move any element at the end, but before tail
    private void moveBeforeTail(ListNode node) {
        node.prev.next = node.next;            
        node.next.prev = node.prev;
        node.prev = tail.prev;
        node.prev.next = node;
        node.next = tail;
        tail.prev = node;
    }

    // tail will be dummy node always
    private void insertBeforeTail(ListNode node) {
        tail.prev.next = node;
        node.prev = tail.prev;
        node.next = tail;
        tail.prev = node;
    }
    // Delete node in between
     private void deleteNode(ListNode node, int key) {
        node.prev.next = node.next;            
        node.next.prev = node.prev;
        node.prev = null;
        node.next = null;
        hm.remove(key);
     }
     // To delete first node
     private void deleteFirstNode() {
        ListNode first = head.next;
        ListNode second = first.next;
        head.next = second;
        second.prev = head;
        first.next = null;
        first.prev = null;
        // Remove from Hashmap
        hm.remove(first.key); 
     }
}
```

#### Cycle detection in LL - Floyd's Tortoise and Hare Alogrithm

* Use two pointers. Move one pointer one at a time, another two at a time. 

* In a point, they will meet each other. That is the cycle start point

##### Detect Intersection of two LL - Using Hashset

Write a program to find the node at which the intersection of two singly linked lists, **A** and **B**, begins. For example, the following two linked lists:

A: a1 → a2
 ↘
 c1 → c2 → c3
 ↗
B: b1 → b2 → b3

**NOTE:**

- If the two linked lists have no intersection at all, return null.
- The linked lists must retain their original structure after the function returns.
- You may assume there are no cycles anywhere in the entire linked structure.
- Your code should preferably run in O(n) time and use only O(1) memory.
- The custom input to be given is different than the one explained in the examples. Please be careful.

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode A, ListNode B) {
        Set<ListNode> set = new HashSet<>();
        while(A != null) {
            set.add(A);
            A = A.next;
        }
        while(B != null) {
            if(set.contains(B)) {
                return B;
            }
            B = B.next;
        }
        return B;
    }
}
```

##### Identify if there is a loop

![](Notes-Advanced%20DSA3/2026-09-22-23-57-50-image.png)

![](Notes-Advanced%20DSA3/2026-09-22-23-53-15-image.png)

![](Notes-Advanced%20DSA3/2026-09-22-23-59-30-image.png)

##### Remove loop from LL

<img src="Notes-Advanced%20DSA3/2026-09-23-00-05-46-image.png" title="" alt="" width="697">

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     public int val;
 *     public ListNode next;
 *     ListNode(int x) { val = x; next = null; }
 * }
 */
public class Solution {
    public ListNode solve(ListNode A) {
        ListNode t1 = A;
        ListNode t2 = null;
        ListNode s = A;
        ListNode f = A;
        // Find the cycle meet point
        while(f != null && f.next != null) {
            s = s.next;
            f = f.next.next;
            if(s == f) {
                t2 = s;
                break;
            }
        }
        ListNode prev = A;

        if(t2 != null) {
            // Iterate until the loop start point comes
            while(t1 != t2) {
                prev = t2;
                t1 = t1.next;
                t2 = t2.next;
            }
            // break the cycle
            prev.next = null;
        }
        return A; 
    }
}
```

> TC: $O(N)$
> 
> SC: $O(1)$

## Trees4: Morris Inorder Traversal + LCA

### Binary Search Tree

![](Notes-Advanced%20DSA3/2026-09-22-00-44-42-image.png)

### Find k^th^ smallest element in a Binary Search Tree

* Do **in order traversal**

* Keep a counter. Once the counter reaches k, return the node value

* Keep a global variable to exit the recursion eagerly

```java
public class Solution {
    int ans = -1;
    int count = 0;
    public int kthsmallest(TreeNode A, int B) {
        findElement(A, B);
        return ans;
    }
    private void findElement(TreeNode A, int B) {
        if(A == null) return ;
        findElement(A.left, B);
        count++;
        if(count == B) {
            ans = A.val;
            return ;
        } 
        // Break further recursion if ans is found
        if(ans != -1) return ;
        findElement(A.right, B);             
    }
}
```

TC: $O(N)$

SC: $O(log N)$ or $N$

### Morris Inorder traversal

This details to do **Inorder traversal without using Recursion and Stack**.

<img src="Notes-Advanced%20DSA3/2026-09-22-00-46-06-image.png" title="" alt="" width="656">

**Problem Description**  

Given a binary tree, return the inorder traversal of its nodes' values.

**NOTE:** <u>Using recursion and stack are not allowed.</u>

**Psedocode:**

* Link each right most node to the previous parent node.

* This linking is required to go one level up after printing the left sub tree of each node.

```java
    public ArrayList<Integer> solve(TreeNode A) {
        ArrayList<Integer> ans = new ArrayList<Integer>();
        TreeNode current = A;
        while(current != null) {
            if(current.left == null) { // if no left element, add the node and move right
                ans.add(current.val);
                current = current.right;
            } else {
                TreeNode temp = current.left;
                while(temp.right != null && temp.right != current) // Go until right node is null or is current element
                    temp = temp.right;
                if(temp.right == null) { // First time traversal. Link to one level up parent.
                    temp.right = current;
                    current = current.left;
                } else { // Second time right side path is visited. Delink it.
                    temp.right = null;
                    ans.add(current.val); // Add current element and go right
                    current = current.right;
                }
            }
        }
        return ans;
    }
```

**TC:** Each node is visited 3 times at max. so $TC = O(N)$

**SC:** O(1)

### Lowest Common Ancestor (LCA) in BST

* LCA is the place where the path to the given nodes starts diverging.

* This is the place where either A or B will be either big or small.

**Problem Description**  

Given a Binary Search Tree **A**. Also given are two nodes **B** and **C**. Find the lowest common ancestor of the two nodes.

Note 1 :- It is guaranteed that the nodes B and C exist.

Note 2 :- The LCA of B and C in A is the **shared ancestor of B and C** that is located farthest from the root.

<img src="Notes-Advanced%20DSA3/2026-09-22-00-52-30-image.png" title="" alt="" width="669">

```java
    public int solve(TreeNode A, int B, int C) {
        TreeNode current = A;
        while(current != null) {
            if(B < current.val && C < current.val) {
                current = current.left;
            } else if(B > current.val && C > current.val) {
                current = current.right;
            } else {
                return current.val;
            }
        }
        return 0;
    }
```

TC: $O(N)$ or $O(log N)$

SC: $O(1)$

### LCA in Binary Tree

**Problem Description**  

Find the lowest common ancestor in an unordered binary tree **A,** given two values, **B** and **C,** in the tree.

Lowest common ancestor: the lowest common ancestor (LCA) of two nodes and w in a tree or directed acyclic graph (DAG) is the lowest (i.e., deepest) node that has both v and w as descendants.

<img src="Notes-Advanced%20DSA3/2026-04-15-18-47-04-image.png" title="" alt="" width="289">

**Approach 1:**

```java
/**
 * Definition for binary tree
 * class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) {
 *      val = x;
 *      left=null;
 *      right=null;
 *     }
 * }
 */
public class Solution {
    public int lca(TreeNode A, int B, int C) {
        TreeNode node = findLca(A, B, C);
        return node != null ? node.val : 0;
    }

    private TreeNode findLca(TreeNode A, int B, int C) {
        if( A == null) return null;

        // return the node if either value matches
        if(A.val == B || A.val == C) {
            return A;
        }
        // Navigate the left paths 
        TreeNode left = findLca(A.left, B, C);
        TreeNode right = findLca(A.right, B, C);

        if(left != null && right != null) {
            return A;
        } else if(left != null) { // left node is the common node
            return left;
        } else if(right != null) { // right node is the common node
            return right;
        } else {
            return null;
        }
    }
}
```

**Approach 2:**

**Psedocode:**

```java
public class Solution {
    boolean isBfound = false;
    boolean isCfound = false;

    public int lca(TreeNode A, int B, int C) {
        TreeNode ans = findLca(A, B, C);
        return  isBfound && isCfound ? ans.val : -1; // Return answer only of both B and C are found.
    }

    private TreeNode findLca(TreeNode root, int B, int C) {
        if(root == null) return null;

        TreeNode temp = null;
        if(root.val == B) {
            isBfound = true; // Seprate flag for B and C
            temp = root;
        }
        if(root.val == C){
            isCfound = true; // Seprate flag for B and C
            temp = root;
        } 
        TreeNode l = findLca(root.left, B, C);
        TreeNode r = findLca(root.right, B, C);

        if(temp != null) return root; // Iterate through remaining nodes at the bottom. Covers if either of the nodes is LCA.

        if( l != null && r != null) {
            return root;            
        }
        if(l != null) {
            return l;
        }
        if(r != null) {
            return r;
        }
        return null;
    }
}
```

![](Notes-Advanced%20DSA3/2026-04-15-18-47-04-image.png)

Given (4, 7) -> 4 will be answer. This is covered by keeping `if(temp != null) return root;` after navigating through all elements at the bottom.

### Path sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

**Without any global variables (Simple):**

```java
public class Solution {

    public int hasPathSum(TreeNode A, int B) {
        return calcPathSum(A, B, 0) ? 1 : 0;
    }

    private boolean calcPathSum(TreeNode node, int B, int sum) {
        if(node == null) return false;

        sum += node.val;

        if(node.left == null && node.right == null) {
            if(sum == B) return true;
            else return false ;
        } 
        // Below travels through the left path first and then right path
        return calcPathSum(node.left, B, sum) || calcPathSum(node.right, B, sum);
    }
}
```

**Using global boolean:**

```java
public class Solution {
    boolean pathFound = false;

    public int hasPathSum(TreeNode A, int B) {
        calcPathSum(A, B, 0);
        return pathFound ? 1 : 0;
    }

    private void calcPathSum(TreeNode node, int B, int sum) {
        if(node == null) return;

        sum += node.val;

        if(node.left == null && node.right == null) {
            if(sum == B) { 
                pathFound = true;
                return ;
            } else return ;
        }

        calcPathSum(node.left, B, sum);

        if(pathFound) return ;

        calcPathSum(node.right, B, sum);

        if(pathFound) return ;

        sum -= node.val;

        return ;
    }
}
```

## Binary Trees 2 - Lab session

### Next Pointer Binary Tree - Using Level Order Traversal

Given a binary tree, Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Assume perfect binary tree.

* Use level order traversal

* This uses Queue

```java

```

> As using Queue SC: $O(N)$

### Next Pointer Binary Tree - Without Level Order Traversal

**Approach 1 - Shortest:**

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {

        TreeLinkNode current = root;

        while(current != null) {
            TreeLinkNode nextRow = new TreeLinkNode(-1);
            TreeLinkNode dummy = nextRow;

            // Iterate till the last element of the row
            while(current != null) {
                if(current.left != null) {
                    nextRow.next = current.left;
                    nextRow = nextRow.next;
                }
                if(current.right != null) {
                    nextRow.next = current.right;
                    nextRow = nextRow.next;           
                }        
                // Go to next node in the same row
                current = current.next;
            }
            // Go to the next row first element
            current = dummy.next;                
        }     
    }
}
```

> SC: $O(1)$

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {

        TreeLinkNode levelStart = new TreeLinkNode(-1);
        TreeLinkNode current = root;
        current.next = null;
        TreeLinkNode nextCurrent = null;

        while(current != null) {

            if(current.left != null) {
                if(levelStart.next == null) {
                    levelStart.next = current.left;
                    nextCurrent = current.left;
                } 
                else { 
                    nextCurrent.next = current.left;
                    nextCurrent = nextCurrent.next;           
                }
            }
            if(current.right != null) {
                if(levelStart.next == null) {
                    levelStart.next = current.right;
                    nextCurrent = current.right;
                }
                else {
                    nextCurrent.next = current.right;
                    nextCurrent = nextCurrent.next;           
                }
            }
            current = current.next;

            if(current == null) { 
                current = levelStart.next;
                levelStart.next = null;                 
            } 
        }        
    }
}
```

### Diameter of binary tree

Given a Binary Tree **A** consisting of **N** integer nodes, you need to find the **diameter** of the tree.

The **diameter** of a tree is the number of edges on the longest path between two nodes in the tree.

- Find height of each node

- Find longestPath at each node

- Max of longestPath will be the diameter

![](Notes-Advanced%20DSA3/2026-09-23-22-13-21-image.png)

![](Notes-Advanced%20DSA3/2026-09-23-22-07-30-image.png)

```java
/**
 * Definition for binary tree
 * class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) {
 *      val = x;
 *      left=null;
 *      right=null;
 *     }
 * }
 */
public class Solution {
    int diameter = 0;
    public int solve(TreeNode A) {
        findDiameter(A);
        return diameter;
    }

    private int findDiameter(TreeNode A) {
        // if node is null, return -1
        if(A == null) return -1;
        int lh = findDiameter(A.left);
        int rh = findDiameter(A.right);
        // longestPath accross each node
        int longestPath = lh + rh + 2;
        // Calculate diameter at each node
        diameter = Math.max(diameter, longestPath);
        // return height of the node
        return Math.max(lh, rh) + 1;
    }
}
```

> TC: $O(N)$
> 
> SC: $O(H)$ -> Stack space due to recursion

### Vertical order traversal

https://www.scaler.com/academy/mentee-dashboard/class/523617/assignment/problems/378/?navref=cl_pb_nv_tb

Given a binary tree, return a 2-D array with vertical order traversal of it. Go through the example and image for more details.

![](https://s3.ap-south-1.amazonaws.com/scaler-production-domestic/public_assets/assets/000/000/071/original/378.jpeg?1614507246)

**NOTE:** If 2 Tree Nodes shares the same vertical level then the one with lesser depth will come first.

**Notes:**

* Use level order traversal

* Keep node value, level in Queue to do level order traversal

* Store level and node values in **hashmap**

* Identify min, max level at each left child node, right child node

```java
/**
 * Definition for binary tree
 * class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) {
 *      val = x;
 *      left=null;
 *      right=null;
 *     }
 * }
 */
public class Solution {

    public ArrayList<ArrayList<Integer>> verticalOrderTraversal(TreeNode A) {

        Queue<NodeData> nodeQueue = new LinkedList<>();
        HashMap<Integer, ArrayList<Integer>> hm = new HashMap<>();
        int min = 0;
        int max = 0;

        nodeQueue.add(new NodeData(A, 0));

        // Do level order traversal
        // Iterate through out the queue
        while(!nodeQueue.isEmpty()) {

            NodeData tempNodeData = nodeQueue.poll();
            TreeNode tempNode = tempNodeData.getTreeNode();
            int level = tempNodeData.getLevel();

            // Store node, level detail in Hashmap
            if(hm.containsKey(level)) {
                hm.get(level).add(tempNode.val);
            } else {
                ArrayList<Integer> tempList = new ArrayList<>();
                tempList.add(tempNode.val);
                hm.put(level, tempList);
            }

            // Keep adding the childen of current node to Queue
            // Add level of childen into the Queue
            if(tempNode.left != null) {
                nodeQueue.add(new NodeData(tempNode.left, level-1));
                // Find min level so far found
                min = Math.min(min, level-1);
            }
            if(tempNode.right != null) {
                nodeQueue.add(new NodeData(tempNode.right, level+1));
                // Find max level so far found
                max = Math.max(max, level+1);
            }               
        } 

        ArrayList<ArrayList<Integer>> ans = new ArrayList<ArrayList<Integer>>();
        // Construct answer from Hashmap
        for(int i=min; i<=max; i++) {
            ans.add(hm.get(i));
        }
        return ans;
    }

    class NodeData {
        private TreeNode treeNode;
        private int level;

        public NodeData(TreeNode treeNode, int level) {
            this.treeNode = treeNode;
            this.level = level;
        }

        public TreeNode getTreeNode() {
            return treeNode;
        }

        public int getLevel() {
            return level;
        }
    }
}
```

> TC: $O(N)$
> 
> SC: $O(N)$

### Top View of Binary tree

Given a binary tree of integers denoted by root **A**. Return an array of integers representing the top view of the Binary tree.

The top view of a Binary Tree is a set of nodes visible when the tree is visited from the top.

Return the nodes in any order.

**Notes:**

* Add the first element of each item in Hashmap - Top view

* Add the last element of each item in Hashmap - Bottom view

```java
/**
 * Definition for binary tree
 * class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) {
 *      val = x;
 *      left=null;
 *      right=null;
 *     }
 * }
 */
public class Solution {
    public ArrayList<Integer> solve(TreeNode A) {

        Queue<NodeData> nodeQueue = new LinkedList<>();
        HashMap<Integer, ArrayList<Integer>> hm = new HashMap<>();
        int min = 0;
        int max = 0;

        nodeQueue.add(new NodeData(A, 0));

        // Do level order traversal
        // Iterate through out the queue
        while(!nodeQueue.isEmpty()) {

            NodeData tempNodeData = nodeQueue.poll();
            TreeNode tempNode = tempNodeData.getTreeNode();
            int level = tempNodeData.getLevel();

            // Store node, level detail in Hashmap
            if(hm.containsKey(level)) {
                hm.get(level).add(tempNode.val);
            } else {
                ArrayList<Integer> tempList = new ArrayList<>();
                tempList.add(tempNode.val);
                hm.put(level, tempList);
            }

            // Keep adding the childen of current node to Queue
            // Add level of childen into the Queue
            if(tempNode.left != null) {
                nodeQueue.add(new NodeData(tempNode.left, level-1));
                // Find min level so far found
                min = Math.min(min, level-1);
            }
            if(tempNode.right != null) {
                nodeQueue.add(new NodeData(tempNode.right, level+1));
                // Find max level so far found
                max = Math.max(max, level+1);
            }               
        } 

        ArrayList<Integer> ans = new ArrayList<Integer>();
        // Construct answer from Hashmap
        // Add the first element of each item in Hashmap
        for(int i=min; i<=max; i++) {
            ans.add(hm.get(i).get(0));
        }
        return ans;
    }

    class NodeData {
        private TreeNode treeNode;
        private int level;

        public NodeData(TreeNode treeNode, int level) {
            this.treeNode = treeNode;
            this.level = level;
        }

        public TreeNode getTreeNode() {
            return treeNode;
        }

        public int getLevel() {
            return level;
        }
    }    
}
```

## Hashing 3 - Internal Implementation

### Group Anagrams

Given an array of strings **A**, group the **anagrams** together. You can return the answer in **any order**.  

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

![](Notes-Advanced%20DSA3/2026-09-24-00-55-03-image.png)

```java
public class Solution {
    public ArrayList<ArrayList<String>> groupAnagrams(ArrayList<String> A) {
        int n = A.size();

        ArrayList<String> sortedStrings = new ArrayList<String>();
        // Create list of strings after sorting
        for(int i=0; i<n; i++) {
            sortedStrings.add(sortString(A.get(i)));
        }

        HashMap<String, ArrayList<String>> hm = new HashMap<>();
        // hm key -> sorted String, value - list of raw strings
        for(int i=0; i<n; i++) {
            if(hm.containsKey(sortedStrings.get(i))) {
                hm.get(sortedStrings.get(i)).add(A.get(i));
            } else {

                hm.put(sortedStrings.get(i), new ArrayList<>(Arrays.asList(A.get(i))));
            }
        }
        ArrayList<ArrayList<String>> ans = new ArrayList<ArrayList<String>>();
        // add each list to ans list
        hm.forEach((key, value) -> ans.add(value));

        return ans;
    }

    // Sort String
    private String sortString(String str) {
        // Convert to char[] for sorting
        char[] charArray = str.toCharArray();
        Arrays.sort(charArray);
        // Create new String from char[]
        return new String(charArray);
    }
}
```

> TC:
> 
> SC:
> 
> - [ ] Analyse 

## Contest 3

### Flatten Binary Tree to Linked List

Given a binary tree **A**, flatten it to a linked list in-place using preorder traversal.

The left child of all nodes should be NULL.

1 <= size of tree <= 100000

**Example Input**  

Input 1:

     1
    / \

   2   3

Input 2:

         1
        / \
       2   5
      / \   \
     3   4   6

**Example Output**  

Output 1:

1
 \
  2
   \
    3

Output 2:

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6

```java
public class Solution {
    TreeNode ans = new TreeNode(-1);
    TreeNode current = ans;
    public TreeNode flatten(TreeNode a) {
        constructTree(a);
        return ans.right;
    }

    private void constructTree(TreeNode node) {
        if(node ==null) return ;
        TreeNode temp = new TreeNode(node.val);
        //System.out.println("val: " + node.val);
        current.right = temp;
        current.left = null;
        current = current.right;
        constructTree(node.left);
        constructTree(node.right);
    }
}
```

### Special Prime Numbers

**Problem Description**  

You're a software developer working on a new feature for a financial application. The application provides users with insights into interesting numerical patterns. One of the features involves identifying and analyzing prime numbers with special properties.  

Your team lead has assigned you the task of finding prime numbers up to a given number A where the sum of the digits of each prime number is also a prime number. This feature will help users discover unique primes and their properties.  

Given an integer A, Your task is to find the number of prime numbers less than or equal to A where the sum of the digits of the prime number is also a prime number.

```java
public class Solution {
    public int countSpecialPrimes(int A) {
        int[] spf = new int[A+1];
        spf[0] = 0;
        if(A ==1) spf[1] = 1;
        if(A == 2) spf[2] = 2;
        for(int i=0; i< spf.length; i++) {
            spf[i] = i;
        }
        for(int i=2; i*i<=A; i++) {
            for(int j = i*i; j <= A; j += i) { // Find multiplication numbers
                if(spf[j] ==  j) { // If number is not updated already
                    spf[j] = i; // assign factor
                }
            }
        }
        int count =0;
        for(int i=2; i<spf.length; i++) {
            int sum =0;
            if(spf[i] == i) { // if this is true, it did not had any factor, so a prime number
                int temp = spf[i];
                while(temp > 0) {
                    sum += temp%10;
                    temp /= 10;
                }
                if(spf[sum] == sum) { // Check sum is also a prime number
                    count++;
                }
            }
        }
        return count;
    }
}
```

### Child amusement park

In an amusement park, there are rides being constructed for **N** children. Each ride has a maximum capacity limit of **B** for the total weight of the children on board.  

You are given an array of weights of children, where **A[i]** is the weight of the **i-th** child, and an infinite number of rides. Each ride can carry **at most two** children at the same time, provided the sum of their weights is at most the ride's capacity limit.  

Find the minimum number of rides to accommodate all the children.

**Problem Constraints**  

1 <= N <= 105

0<= B <= 109

0 <= A[i] <= B

**Algorithm:**

1. Sort the array in ascending order.

2. Maintain two pointers:
   
   - `left = 0` → lightest child.
   
   - `right = N - 1` → heaviest child.

3. Check whether both children can fit in one ride.
   
   - If `A[left] + A[right] <= B`, accommodate both and move both pointers.
   
   - Otherwise, accommodate only the heaviest child and move `right`.

4. Increment the ride count in either case.

5. Continue until all children are accommodated.
   
   ```java
   import java.util.*;
   
   public class Solution {
       public int solve(int[] A, int B) {
   
           Arrays.sort(A);
   
           int left = 0;
           int right = A.length - 1;
   
           int rides = 0;
   
           while (left <= right) {
   
               if (A[left] + A[right] <= B) {
                   left++;
               }
   
               right--;
               rides++;
           }
   
           return rides;
       }
   }
   ```

### Permutations

**Problem Description**  

Given an integer array **A** of size **N** denoting collection of numbers , return all possible permutations.

**NOTE:**

- No two entries in the permutation sequence should be the same.
- For the purpose of this problem, assume that all the numbers in the collection are unique.
- Return the answer in any order

### Number Line Confusion

**Problem Description**  

Rick and Morty are planning a new adventure using a number line.  

Morty selects two points, x and y,  

- forming Morty's interval: [min⁡(∣x∣, ∣y∣), max⁡(∣x∣, ∣y∣)]

Rick then uses Morty's selected values(x, y) to calculate the points ∣x − y∣ and ∣x + y∣,  

- forming Rick's interval: [min⁡(∣x − y∣, ∣x + y∣), max⁡(∣x − y∣, ∣x + y∣)]

Given an array **A** of N distinct integers, find the number of unordered pairs (x,y) such that Morty's interval is **fully contained** within Rick's interval.

 Note:

- ∣x∣ denotes the absolute value of x.
- An unordered pair means that the pair (x, y) is considered the same as (y, x).

**Problem Constraints**  

1 <= N <= 105  
-109 <= Ai <= 109  
All values in the array A are pairwise distinct.

**Input Format**  

The first argument is the integer array A.

**Output Format**  

Return a single integer denoting the number of unordered pairs.

**Example Input**  

Input 1:

A = [-3, 5, -7]

Input 2:

A = [3, 6]

**Example Output**  

Output 1:

```
2
```

Output 2:

```
1
```

**Example Explanation**  

Explanation 1:

A = [-3, 5, -7]
For x = -3 and y = 5:
   Morty's interval: [min(|-3|, |5|), max(|-3|, |5|)] = [3, 5]
   Rick's interval: [min(|-3 - 5|, |-3 + 5|), max(|-3 - 5|, |-3 + 5|)] = [2, 8]
   Morty's interval [3, 5] is fully contained within Rick's interval [2, 8], so this pair is valid.

For x = -3 and y = -7:
   Morty's interval: [3, 7]
   Rick's interval: [min(|-3 - (-7)|, |-3 + (-7)|), max(|-3 - (-7)|, |-3 + (-7)|)] = [4, 10]
   Morty's interval [3, 7] is not fully contained within Rick's interval [4, 10], so this pair is not valid.

For x = 5 and y = -7:
   Morty's interval: [5, 7]
   Rick's interval: [min(|5 - (-7)|, |5 + (-7)|), max(|5 - (-7)|, |5 + (-7)|)] = [2, 12]
   Morty's interval [5, 7] is fully contained within Rick's interval [2, 12], so this pair is valid.

In total, there are 2 valid pairs: (-3, 5) and (5, -7).

Explanation 2:

Morty's interval [3, 6] is fully contained within Rick's interval [3, 9], so this pair is valid.

**Bruteforce method:**

This throws TLE when the count and |A| is high.

```java
 public class Solution {
    public long findPairs(int[] A) {
        int n = A.length;
        long ans = 0l;
        int m1;
        int m2;
        int r1;
        int r2;
        for(int i=0; i<n;i++) {
            for(int j=i+1; j<n; j++) {
                int x = A[i];
                int y = A[j];
                int diff = Math.abs(x-y);
                int sum = Math.abs(x+y);
                m1=Math.min(Math.abs(x), Math.abs(y));
                m2 = Math.max(Math.abs(x), Math.abs(y));
                r1 = Math.min(diff, sum);
                r2 = Math.max(diff, sum);
                if(r1 <= m1 && r2 >= m2) ans++;
            }
        }
        return ans;
    }
}
```

### Print paths in Staircase

**Problem Description**  

You are climbing a staircase and it takes **A** steps to reach the top.

Each time you can either climb **1** or **2** steps. In how many **distinct ways** can you climb to the top?

You need to return all the **distinct ways** to climb to the top in **lexicographical order.**

```java
 public class Solution {
    ArrayList<ArrayList<Integer>> ans = new ArrayList<ArrayList<Integer>>();

    public ArrayList<ArrayList<Integer>> WaysToClimb(int A) {
        findWays(A, A, new ArrayList<>(), 0);
        return ans;
    }

    private void findWays(int A, int N, ArrayList<Integer> ansList, int steps) {
        if(A == 0) {
            ans.add(new ArrayList<>(ansList));
        }
        if(A < 0) return;

        ansList.add(1);
        findWays(A-1, N, ansList, steps+1);
        ansList.remove(ansList.size()-1);
        ansList.add(2);
        findWays(A-2, N, ansList, steps+2);
        ansList.remove(ansList.size()-1);
    }
}
```

### Remove Nth Node from List End

**Problem Description**  

Given a linked list A, remove the B-th node from the end of the list and return its head.  
For example, given linked list: 1->2->3->4->5, and B = 2.  
After removing the second node from the end, the linked list becomes 1->2->3->5.  

NOTE: If B is greater than the size of the list, remove the first node of the list.

*Try doing it using constant additional space.*

```java
 /**
 * Definition for singly-linked list.
 * class ListNode {
 *     public int val;
 *     public ListNode next;
 *     ListNode(int x) { val = x; next = null; }
 * }
 */
public class Solution {
    public ListNode removeNthFromEnd(ListNode A, int B) {
        ListNode temp = A;
        int count =0;
        while(temp != null) {
            temp = temp.next;
            count++;
        }
        if(count == 1 && B == 1) {
            return null;
        }
        if(B >= count) {
            temp = A.next;
            A.next = null;
            return temp;
        }
        temp = A;
        ListNode prev = A;
        for(int i=1; i<=count-B; i++) {
            prev = temp;
            temp = temp.next;
        }
        prev.next = (temp != null) ? temp.next : null;
        temp.next = null;
        return A;
    }
}
```
