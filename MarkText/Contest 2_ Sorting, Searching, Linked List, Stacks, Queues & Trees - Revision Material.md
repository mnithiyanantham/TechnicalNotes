---
title: 'Contest 2: Sorting, Searching, Linked List, Stacks, Queues & Trees - Revision Material'

---

# Revision - Searching

### Introduction to Searching - Story

* Let us say you lost something
* When you go to the police, you tell them what to search for—the target—and where to search—the search space.
* It is easy to look for a word in the dictionary as compared to searching for a word in a book or newspaper. This is because along with the target element, we also have defined search space(alphabetical order). 
* In the Phone book as well, we have names sorted in the contacts list, so it's easier to find a person's number. 
* **Search space** - The area where we know the result exists and we search there only
* **Target** - The item we are looking for
* **Condition** - Some condition to discard some part of the search space repeatedly to make it smaller and finally reach the result.
* **Binary Search** - divide the search space into two parts and repeatedly keep on neglecting one-half of the search space.

### Binary Search Question

Given a sorted array with distinct elements, search the index of an element **k**, if k is not present return -1.
arr[] = 

| 3   | 6   | 9   | 12  | 14  | 19  | 20  | 23  | 25  | 27  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

* Binary search has three elements,
  * **Search space -** array
  * **target -** we are given a target element to be found from the array
  * **condition ->** 
  * array is sorted, say I am at some random position, if I compare the current element with the target element. 
  * If `current_element > target_element`, I can discard all the element appearing before current element and the target_element as they will always be smaller than current element. 
  * Similarly, if the  `current_element < target_element`, the target element and the elements appearing after it can be discareded.
  * We have all the three elements and thus, binary search can be easily applied to it.
* Let us take the middle index.
* if `arr[mid] == k`, i.e. we found the target, we return the index of the target.
* if `arr[mid] < k` i.e. the target exists on the right side of the array so the left half is of no use and we can move to the right.
* Similarly, if `arr[mid] > k` i.e. the target exists on the left, and the right half can be discarded.

<img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/051/908/original/upload_806379972b851dba6e99f692192ada2a.png?1696270159" width=400/>

### Binary Search Pseudo Code

```javascript
int search(int arr[], int N, int k){
    lo = 0, hi = N - 1;
    while(lo <= hi){
        mid = lo + (hi + lo) / 2;
        if(arr[mid] == k) {
            return mid;
        }else if(arr[mid] < k){
            lo = mid + 1;
        }else{
            hi = mid - 1;
        }
    }
    return -1;
}
```

### Complexity

**Time Complexity:** O(log N)
**Space Complexity:** O(1)

### Question

If the element 'k' is found in a sorted array, what will be the time complexity of binary search?

### Choices

- [ ] O(1)
- [x] O(log n)
- [ ] O(n)
- [ ] O(n^2)

## Best Practices - Binary Search

* Some write the formula of mid calculation as <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/051/912/original/Screenshot_2023-10-02_235404.png?1696271055" width=80 /> but it is incorrect, as in some cases it may lead to overflow.
* e.g, Let us assume that maximum number stored by a particular data type is `100` (It's Integer.Max_Int but just to make maths easy, assuming its 100), and the number of elements in the array is `100` for which last index will be `99` for zero indexing and thus, is in the limit. 
* There is the posibility that the target element is at the position `98` and value of `low = 98` and `high = 99`. When we calculate `mid` for this scenario, the `high` and the `low` are in limits but while calculating the mid <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/051/915/original/Screenshot_2023-10-02_235615.png?1696271183" width="100"/> first the sum `197` will be calculated and stored. This will cause the overflow as the limit of the variable is `100` only(assumption).
* This may result in incorrect or negative value.
* Whereas, when we write <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/051/916/original/Screenshot_2023-10-02_235725.png?1696271253" width="100"/> we will get <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/051/917/original/Screenshot_2023-10-02_235917.png?1696271365" width="100"/> which even in the worst case be `99` and will not overflow.

<img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/051/918/original/upload_8464c14ddc9d5166e7118e05dcbfdc72.png?1696271394" width="500"/>

![](Contest%202_%20Sorting,%20Searching,%20Linked%20List,%20Stacks,%20Queues%20&%20Trees%20-%20Revision%20Material/2026-04-04-23-38-59-image.png)

## Searching 2: Binary Search Problems

### Introduction:

We will continue with our second lecture on Binary Search. In our previous session, we explored the fundamental concepts of this efficient searching algorithm. Today, we will dive even deeper into the world of binary search, uncovering advanced techniques and applications that expand its capabilities.

In this lecture, we will build upon the foundation laid in the first session. We'll delve into topics such as :

1. Binary search on Rotated Sorted Arrays 
2. Finding Square Root using binary search etc 

and addressing various edge cases and challenges that may arise during binary search implementation.

### Pseudocode

```java
function binarySearch(array, target):
    left = 0
    right = length(array) - 1

    while left <= right:
        mid = left + (right - left) / 2

        if array[mid] == target:
            return mid
        else if array[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return NOT_FOUND
```

### Use Cases:

Binary search has numerous applications, including:

* Searching in databases.
* Finding an element in a sorted array.
* Finding an insertion point for a new element in a sorted array.
* Implementing features like autocomplete in search engines.

### Finding an Element in a Rotated Array

Suppose we have the following rotated sorted array:

```javascript
Original Sorted Rotated Array: [4, 5, 6, 7, 8, 9, 1, 2, 3]
```

Let's say we want to find the element 7 within this rotated array using a brute-force approach (Linear Search).

#### Brute-Force Approach:

* Initialize a variable target with the value we want to find (e.g., 7).
* Loop through each element in the array one by one, starting from the first element.
* Compare the current element with the target:
* If the current element matches the target, we have found our element, and we can return its index.
* If the current element does not match the target, continue to the next element.
* Repeat step 3 until we either find the target or reach the end of the array without finding it.

#### Adapting Binary Search:

While the array is rotated, we can still leverage the divide-and-conquer nature of Binary Search. However, we need to make a few adjustments to handle the rotation.

**Intution:**

* In a rotated sorted array, elements were initially sorted in ascending order but have been rotated at some point.

* Let's asusme array contain distinct elements only.

* The goal is to find a specific target element within this rotated array.

* The key to binary search in a rotated array is to determine the pivot point, which is where the array rotation occurred.

* The pivot point is essentially the maximum element in the array.

* Once you've identified the pivot point, you have split the array into two subarrays, each of which is sorted.

* Then you can apply individual binary search in both the parts, and find the target element.

* **Another Idea of Doing it in one Binary Search we'll discuss below**

* **Partitioning of Rotated Sorted Array:**
  
  * A rotated sorted array can be visualized as being split into two parts: part 1 and part 2.
  
  * Crucially, both part 1 and part 2 are sorted individually, but every element in part 1 is greater than those in part 2 due to the rotation.
    
    <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/054/290/original/upload_7109177bd564db28fb52884947596f19.jpeg?1697636298" width=600 />

* **Identifying Target's Part**
  
  * To determine which part the Target belongs to (part 1 or 2), compare it with the 0th element.
  
  * If the midpoint is greater than (also equals to) the 0th element, then it belongs to part 1. Otherwise, it's in part 2.
    
    <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/054/291/original/upload_de49790f61452f4caceea8736169fe3d.jpeg?1697636323" width=500 />

* **Identifying Midpoint's Part:**
  
  * To determine which part the midpoint belongs to (part 1 or 2), compare it with the 0th element.
  * If the midpoint is greater than (also equals to) the 0th element, then it belongs to part 1. Otherwise, it's in part 2.

* **Midpoint vs Target:**
  
  * If the midpoint is the equals to the target, you've found it.
    
    * If not, then check if the target lies in the same part as the midpoint. If yes, both target and midpoint is within the same sorted part, perform a binary search in that part to move your midpoint towards the target.
      
      <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/054/292/original/upload_e222ee12696646bb515fe8b21149991b.png?1697636367" width=600 />
  
  * If no, move your search towards the other part, effectively approaching the midpoint towards target.
    
    <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/054/293/original/upload_8ab6fabad4f92c55665e78291b1befb5.png?1697636388" width=600 />

* **Iterative Process:**
  
  * Continue adjusting your boundaries based on the decisions made in the previous step until you either find the target or exhaust your search space.

* **Result:**
  
  * Return the index of the target if found, or -1 if not.

### Pseudocode:

```cpp
function searchRotatedArray(nums, target):
    left = 0
    right = length(nums) - 1

    while left <= right:
        mid = left + (right - left) / 2

        if nums[mid] == target:
            return mid

        if target < nums[0]:
            if nums[mid] >= nums[0]:
                left = mid + 1
            else:
                if nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1
        else:
            if nums[mid] < nums[0]:
                right = mid - 1
            else:
                if nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1

    return -1
```

## Searching 3: Binary Search on Answer

### Problem Statement

We have to paint all N boards of length [C0, C1, C2, C3 … CN - 1]. There are K painters available and each of them takes 1 units of time to paint 1 unit of the board.

Calculate and return the minimum time required to get the job done.

> NOTE: 
> 
> 1. Two painters cannot share a board to paint. That is to say, a board cannot be painted partially by one painter, and partially by another.
> 2. A painter will only paint contiguous boards. This means a painter paints a continous subarray of boards

### Example 1

Below are some of the possible configurations:
<img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/078/original/IMG_3AFFFE9FBC6A-1.jpeg?1699349867" width=600 />

Configuration 1: Max is 31
Configuration 2: Max is 26
Configuration 3: Max is 25

**Out of above least is 25**, hence configuration 3 is better. We want to minimize the maximum value. 

There can be more configurations, but you'll find the 3rd to be the best, hence **`25`** is the answer.

### Example 2

```cpp
A = [10,20,30,40]
K = 2
```

P1 => red
P2 => green
<img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/948/original/Screenshot_2023-10-17_123615.png?1697526385" width=200 />
Max = 70

<img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/949/original/Screenshot_2023-10-17_123726.png?1697526456" width=200 />

Max = 60

### Output

60

* Greedy Approach:- We can just divide the total time by total number of painters.
* One might think it as by dividing the boards among the painters would result in least time per painter 
* But is this idea valid ?

### Flaw:

```cpp
A = [1,2,3,4,100] K = 2
```

According to Idea 1 :- $110/2 = 55$

But we see that it is impossible, because we can't divide the boards among two painters with 55 length each.

### Explanation:

The minimum time required to Finish the job is 100.

The configuration of the boards is as follows

* painter1 = [1, 2, 3, 4] = 10 
* painter2 = [100] = 100

Thus the maximum of (10, 100) is the minimum time taken to complete the job.

Among all possible configuration **100** is the minimum time achieved.

Lets's look at example below :-

`[3, 5, 1, 7, 8, 2, 5, 3, 10, 1, 4, 7, 5, 4, 6]` ans `K = 4`

Color code for each painter:

> P1 -> Red
> P2 -> Green
> P3 -> Blue
> P4 -> Orange

### Search Space

**Best Case**
Say we have as many painters as the number of boards, in which case each painter can paint one board. The maximum value will be the answer.

**Example:**
`A[ ] = {2, 5, 3, 8}, K = 4`
`Then 8 is the answer.`

**Worst Case**
There is ony 1 painter. In this case, sum(array) is the answer.

So, our Search Space will be within range: **[max(array) to sum(array)]**

### Target

The max time to complete the task

### Condition

* Say we land at mid. How can we decide whether mid is the answer? 
* We can check if we can complete the task within mid amount of time at max.
  * If yes, then we should save it as the answer and move left to try for a lesser time.
  * Else, move right(it means we will need more time to complete the task )

`[3, 5, 1, 7, 8, 2, 5, 3, 10, 1, 4, 7, 5, 4, 6]` ans `K = 4` 

* Intially the **Lo = 10** and **Hi = 71** (By using above defined criteria for search space) therefore using the above algorithm Mid = $10 + (71 - 10)/2 = 40$ then we check if it is possible to paint all boards in atleast 40 units below is the configuration that satisfy the condition :-
  
  * Painter P1 takes = $3 + 5 + 1 + 7 + 8 + 2 + 5 + 3 = 34$
  * Painter P2 takes = $10 + 1 + 4 + 7 + 5 + 4 + 6 = 37$
  * <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/955/original/Screenshot_2023-10-17_124357.png?1697526848" width=250 />
  * Since the condition is satisfied let's go to left i.e. $Hi = mid -1 = 40 - 1 = 39$

* Now **Hi = 39** and **Lo = 10**; Mid = $10 + (39 - 10)/2 = 24$
  
  * Painter P1 takes = $3 + 5 + 1 + 7 + 8 = 24$
  * Painter P2 takes = $2 + 5 + 3 + 10 + 1 = 21$
  * Painter P3 takes = $4 + 7 + 5 + 4 = 20$
  * Painter P4 takes = 6
  * <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/956/original/Screenshot_2023-10-17_124440.png?1697526887" width=250 />
  * Since the condition is satisfied let's go to left i.e. $Hi = mid -1 = 24 - 1 = 23$

* Now Hi = 23 and Lo = 10 therefore using the above algorithm Mid = $10 + (23 - 10)/2 = 16$ then we check if it is possible to paint all boards in atleast 10 units We find that there is no configuration that satisfy the condition therefore we go to right.

* Similarly we follow the algorithm to arrive at the solution.
  
  <img src="https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/957/original/upload_2f20dca15bacdd111b2ee85772b291c9.png?1697526929" width=750 />

### Pseudocode

```cpp
// Find minimum painters required for given maximum length 
// (Length that painter can paint)
paintersNumber(arr[], N, mid, K)
{
    res = 0, numPainters = 1;

    for (i = 0; i < N; i++) {
        res += arr[i];

        if (res > mid) {
            res = arr[i];
            numPainters++;
        }
    }
    //if we have used less than or equal to given number of painters,
    //then the configurations works
    if(numPainters <= K)return true;
    else return false;
}


partition(arr[], N, K)
{
    Lo = maxEle(arr, N);
    Hi = sumArr(arr, N);

    while (Lo <= Hi) {
        mid = Lo + (Hi - Lo) / 2;

        if (paintersNumber(arr, N, mid, K))
            ans = mid
            Hi = mid - 1 ;
        else
            Lo = mid + 1;
    }
    return ans;
}
```

### Time and Space complexity

**T.C --->** O(log (sum - max) * N )
**S.C --->** O(1)

# Stacks

### Introduction to Stack

- Simple English word that refers to the stack or pile of things.
- It is a Linear Data Structure.
- Follows the Last In First Out(LIFO) or First In Last Out (FILO) principle.
- In recursion, we used call stack for function calls that execute the newest function first.
- Browser history can be built using stack. For example - Google -> Scaler -> Twitter -> Facebook. As soon as we go back the current item is removed from the stack. This removed item is then stored in another stack. Thus, there are two stacks that support forward and backward operations in the browser.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/861/original/upload_6d111b024044063fd59ba3031d696ce2.jpeg?1695740538)

### Functions of Stacks

- add elements
- remove elements
- returns the size of the stack
- access elements on the top

```javascript
Stack<Integer> st = new Stack<>();
st.push(x); // add element
st.pop(); // remove element
st.size() // returns size of the stack;
st.peek(); // access the top element
```

### Problem Statement

Given a string str, remove equal pair of adjacent characters. Return the answer without adjacent duplicates.

#### Example

- **s = abbd** -> "ad"
- **s = abccbde** -> "ade"
- **s = abbbd** -> "abd"
- **s = abba** -> ""

#### Psuedo Code

```java
Stack<Character> st = new Stack<>();
String str = "abbcbbcacx";
int n = str.length();
for(int i = 0; i < n; i++){
    char ch = str.charAt(i);
    if(ch == st.peek()) st.pop();
    else st.push(ch);
}

StringBuilder sb = new StringBuilder();
while(st.size() > 0){
    sb.insert(0,st.pop());
}
return sb.reverse();
```

## Expression Evaluation

#### Mathematical Expression

- **Bodmas** defines Operators precedency, the execution order of operators.
- If two operators have the same precedence then we solve from **left to right**.

#### Infix expressions

- Operators(+, -, * , ...) between operands
- Operand1 Operator Operand2

#### Postfix expressions

- Operands between Operators(+, -, * , ...)
- Operand1 Operator Operand2

### Infix to Postfix expression

**Example 1**

- `a/(b-`**`(c-d)`**`+e*f)`
- `a/(b-(cd-)+`**`e*f`**`)`
- `a/(`**`b-(cd-)`**`+ef*) `
- `a/(`**`(bcd--)+ef*) `**
- **`a/(bcd--ef*+))`**
- **`abcd--ef*+/`**

**Example 2**

- `A+B/C*`**`(D+E)`**`-F`
- `A+`**`B/C`**`*DE+-F`
- `A+`**`BC/DE+*`**`-F`
- **`ABC/DE+*+`**`-F`
- **`ABC/DE+*+F-`**

### Infix to Postfix Conversion

- Dry run the above examples using a stack to convert the above infix expressions to postfix expressions.

### Procedure

1. If the **character is operand**, then add it to postfix string

2. If the **character is (** (opening bracket), push it to the stack.

3. If the **character is )** (closing bracket), pop all the operators and add them to the postfix string until and unless we encounter the opening bracket. Pop the opening bracket from the stack.

4. If **character is operator**
   
   ```java
   while((st.size() > 0) && (st.peek() != '(') && (priority(st.peek()) >= priority(current element))){
    char character = st.pop();
    postfix.append(character);
   }
   if((st.size() == 0) || (st.peek() == '(') || (priority(st.peek() < priority(current element)))){
    st.push(ch)
   }
   ```

5. If **all the characters from the string are read** then pop all the operators from the stack and insert them in postfix.

### Priority Function

```java
int priority(char ch){
    if(ch == '+' || ch == '-') return 1;
    else if(ch == '*' || ch == '/') return 2;
    else return 3;
}
```

### Problem : Nearest smaller on left

Given an `arr[N]`, for every element `arr[i]` find nearest smaller element on left side.

**Note:** Nearest element `arr[i]` distance between indices.

#### Example 1

```java
arr[6]: [4, 5, 2, 10, 3, 12]
ans[6]: [-1, 4, -1, 2, 3, 3]
```

#### Example 2

```java
arr[8]: [4, 6, 10, 11, 7, 8, 3, 5]
ans[8]: [-1, 4, 6, 10, 6, 7, -1, 3]
```

#### Brute force idea

For every element iterate on the left and get nearest smaller on left.

**Pesudocode**

```java
int[] findNearestSmallerElements(int arr[N]) {
        int ans[N] = {-1};

        for (int i = 0; i < n; i++) {
            for (int j = i - 1; j >= 0; j--) {
                if (arr[j] < arr[i]) {
                    ans[i] = arr[j];
                    break;
                }
            }
        }
        return ans;
    }
```

#### Time and Space Complexity

What will be T.C and S.C for the above approach?

* **TC -** $O(N^2)$
* **SC -** O(1)

#### Optimized Solution

We can optimize the solution using the **Stack** data structure.

#### Example 1

```java
arr[12]: [5, 8, 11, 14, 7, 10, 13, 6, 9 , 10, 2, 5]
ans[12]: [-1, 5, 8, 11, 5, 7, 10, 5, 6, 9, -1, 2]
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/450/original/upload_e9bcb173f4a37017c0dd0f8e1d399fc4.png?1697174865)

#### Example 2

```java
arr[12]: [4, 6, 10, 11, 6, 8, 3, 5]
ans[12]: [-1, 4, 6, 10, 4, 6, -1, 3]
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/451/original/upload_62ff821c472b37cc30142cbb47e18c7e.png?1697174917)

### Pseudocode

```java
int[] findNearestSmallerElements(int[] arr) {
        int n = arr.length;
        int[] ans = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && st.peek() >= arr[i]) {
                st.pop();
            }
            if (!st.isEmpty()) {
                ans[i] = st.peek();
            } else {
                ans[i] = -1;
            }
            st.push(arr[i]);
        }

        return ans;
    }
```

### Time and Space Complexity

What will be T.C and S.C for the above approach?

* **TC -** O(N)
* **SC -** O(N)

## Problem Statement : Next smaller index on left

Given an `arr[N]`, for every element `arr[i]` find nearest smaller index on left side.

#### Example 1

```java
arr[8]: [4, 6, 10, 11, 7, 8, 3, 5]
ans[8]: [-1, 0, 1, 2, 1, 4, -1, 6]
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/452/original/upload_640f8251a6ef9c5db8d00a7e3a4d8c5d.png?1697174965)

**Pesudocode**

```java
int[] findNearestSmallerElements(int[] arr) {
        int n = arr.length;
        int[] ans = new int[n];
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && arr[st.peek()] >= arr[i]) {
                st.pop();
            }
            if (!st.isEmpty()) {
                ans[i] = st.peek();
            } else {
                ans[i] = -1;
            }
            st.push(i);
        }

        return ans;
    }
```

#### Time and Space Complexity

What will be T.C and S.C for the above approach?

* **TC -** O(N)
* **SC -** O(N)

## Problem Statement - Nearest Greater index on right

Given an `arr[N]`, for every element `arr[i]` find nearest greater index on right side.  

**Pesudocode**

```java
int[] findNearestGreaterElements(int[] arr) {
        int n = arr.length;
        int[] ans = new int[n];
        Stack<Integer> st = new Stack<>();

        // Iterate from right to left
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && arr[st.peek()] <= arr[i]) {
                st.pop();
            }
            if (!st.isEmpty()) {
                ans[i] = st.peek();
            } else {
                ans[i] = -1;
            }
            st.push(i);
        }

        return ans;
    }
```

#### Time and Space Complexity

What will be T.C and S.C for the above approach?

* TC - O(N)
* SC - O(N)

### Hands on exercise :

By refering above questions, Please try the other variants also : 

* Given an arr[N], for every element arr[i] find nearest smaller index on right side.
* Given an arr[N], for every element arr[i] find nearest greater element on left side.

## Problem Statement - Largest Area Histogram

Given continous block of Histogram find max rectangle area which can be present with in histograms.

**Note:** Every Histogram is of `width = 1`.
This question has been previously asked in *Adobe*, *Amazon*, *Microsoft*, *Uber*, *Bloomberg*, and *Facebook*.

#### Example 1

```java
arr[6]: [2, 4, 3, 4, 5, 1]
area = 12
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/461/original/upload_bc1cb2e48b7ee6450d08326f88463827.png?1697176000)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/461/original/upload_bc1cb2e48b7ee6450d08326f88463827.png?1697176000)

#### Example 2

```javascript
arr[18]: [3, 2, 5, 7, 4, 6, 5, 2, 3, 1, 5, 6, 4, 3, 5, 6, 4, 1]
area = 21
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/053/462/original/upload_60c5ca960d851463e4d6747ce768281f.png?1697176055)

#### Observation

Height of any rectangle, it should be one of the heights of histogram.

#### Idea

Take every histogram height as rectangle height and calculate area.

> **Step 1:** Calculate first smaller index on left = L.
> **Step 2:** Calculate first smaller index on right = R.
> **Step 3:** are = [width = R - L - 1, Height = Histogram height arr[i]]

**Pseudocode**

```java
int[] getMaxRectangleArea(int heights[n]) {
       int L = nearestSmallerIndexLeft(heights);
       int R = nearestSmallerIndexRight(heights);
       int ans = 0;
       for(int i = 0; i < n; i++) {
           int h = heights[i];
           int w = R[i] - L[i] - 1;
           if(ans < h * w) {
               ans = h * w;
           }
       }
       return ans;
    }
```

#### Time and Space Complexity

What will be T.C and S.C for the above approach?

* TC - O(N)
* SC - O(N)

# Queues

### Explanation

Queue is a data structure that works like a line of people waiting for something. Imagine you have a line at a ticket counter or a supermarket checkout. The first person to join the line is the first one to get served, and new people join at the end of the line. Similarly, the first element added to a queue is the first one to be removed, and new elements are added to the back of the queue.

A queue follows the **"First-In-First-Out" (FIFO)** principle, which means that the element that has been in the queue the longest is the first one to be processed, and the newest element added will have to wait until all the elements in front of it have been processed and removed.

Functions of queue involves:

* **Enqueue:** Adding an element to the rear (end) of the queue.
* **Dequeue:** Removing the front (first) element from the queue.
* **Front:** Accessing the element at the front (first) of the queue without removing it.
* **Rear:** Accessing the element at the rear (end) of the queue without removing it.

Each of these functions takes `O(1)` time for its operations.

Let's use the numbers 8, 14, 9, 20, and 30 to perform the queue operations:

Initial Queue: (empty queue)

**Enqueue (Adding to the queue):**

* Enqueue 8: Queue: [8]
* Enqueue 14: Queue: [8, 14]
* Enqueue 9: Queue: [8, 14, 9]
* Enqueue 20: Queue: [8, 14, 9, 20]
* Enqueue 30: Queue: [8, 14, 9, 20, 30]

**Dequeue (Removing from the queue):**

* Dequeue: The first element 8 is removed. Queue: [14, 9, 20, 30]
* Dequeue: The first element 14 is removed. Queue: [9, 20, 30]

**Front (Viewing the front element without removing it):**
The front element of the queue is 9.

**Rear (Viewing the rear element without removing it):**
The rear element of the queue is 30.

## Problem : Generate numbers using 1 & 2

Generate k-th number of the series using digits only 1 or 2. In this the input will be k

**For K=5**

* Start with an empty queue and enqueue the initial numbers 1 and 2.
* Queue: [1, 2]
* Dequeue the front number (1) and generate two new numbers by appending 1 and 2 to it.
* Enqueue: [2 (enqueue 12), 1 (enqueue 11)]
* Dequeue the front number (2) and generate two new numbers by appending 1 and 2 to it.
* Enqueue: [1 (enqueue 11), 12 (enqueue 112), 11 (enqueue 111)]
* Dequeue the front number (1) and generate two new numbers by appending 1 and 2 to it.
* Enqueue: [12 (enqueue 112), 11 (enqueue 111), 11 (enqueue 1111), 112 (enqueue 1112)]
* Dequeue the front number (12) and generate two new numbers by appending 1 and 2 to it.
* Enqueue: [11 (enqueue 111), 11 (enqueue 1111), 112 (enqueue 1112), 111 (enqueue 11111), 112 (enqueue 11112)]
* Now, the queue contains all numbers in the series up to the 5th position.
* The k-th number in the series is the last dequeued number before reaching k. In this case, the 5th number is "21".

The approach uses a queue to generate the series efficiently. Starting with the initial numbers 1 and 2 in the queue, we dequeue each number, append 1 and 2 to it, and enqueue the newly generated numbers back into the queue. This process is repeated k-1 times (since we already have the 1st and 2nd numbers). The k-th number is then the last number dequeued before reaching k iterations. This way, we efficiently generate the k-th number in the series using digits 1 or 2.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/044/067/original/upload_c66f4e890416b3befcc9e41d47aee195.png?1692827763)

**Code for k-th Number**

```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

string kthNumber(int k) {
if (k <= 0) {
return ""; // Invalid input, return an empty string
}

if (k == 1) {
return "1"; // The 1st number in the series is always "1"
}

if (k == 2) {
return "2"; // The 2nd number in the series is always "2"
}

queue<string> q;
q.push("1");
q.push("2");

string kthNumber;

for (int i = 0; i < k - 2; i++) {
string currentNumber = q.front();
q.pop();

q.push(currentNumber + "1");
q.push(currentNumber + "2");

kthNumber = currentNumber;
}

return kthNumber;
}
```

**Time Complexity (TC):**

* The initialization part of the function (handling invalid inputs and the first two numbers) takes constant time, which can be considered as `O(1)`.
* The main loop runs k-2 times, where k is the input number. Within each iteration, the queue operations (enqueue and dequeue) take constant time, as they involve adding/removing elements from the front/rear of the queue.

Therefore, the overall time complexity is **O(k)**, as the main loop iterates k-2 times, and each iteration takes constant time.

**Space Complexity (SC):**

* The space required for the queue: At any point, the queue will contain at most k-2 elements. During the loop, we enqueue and dequeue elements, but the maximum number of elements in the queue remains k-2.
* Other than the queue, we use some additional variables like kthNumber, which are constant in terms of space complexity.

Therefore, the overall space complexity is **O(k)**, as the maximum space used by the queue is k-2, and the additional variables take constant space.

# Trees

A tree is a widely used data structure in computer science. It consists of nodes connected by edges, forming a hierarchical structure.Each node can have zero or more child nodes,except for the root node which has no parent.

Some of the applications of trees are:

- Filesystems —files inside folders that are in turn inside other folders.
- Comments on social media – comments,replies to comments, replies to replies etc form a tree representation.
- Family trees — grandparents,parents,children,grandchildren etc represent the family hierarchy.

![Screenshot 2023-12-12 at 10.43.21 PM](https://hackmd.io/_uploads/H1PTVf8Lp.png)

The most commonly used tree data structure are ➖

- Binary Tree
- Binary Search Tree

Lets try to see both these tree data structures in a more detailed manner.

## Binary Tree

A binary tree is a tree data structure whose all nodes have either zero, one, or at most two children nodes. These two children are generally referred to as left and right children respectively.

**Traversal in Binary Tree :** 

- Preorder Traversal
- Inorder Traversal
- Postorder Traversal
- Level order Traversal

![Screenshot 2023-12-12 at 10.45.02 PM](https://hackmd.io/_uploads/rk0tSG8Up.png)

Output -
1
2 3
4 5
6 7

Code for levelorder traversal : 

```java
void printLevelOrder(root)
{
    Queue<Node> queue = new LinkedList<Node>();
    queue.add(root);
    while (!queue.isEmpty()) {
         Node tempNode = queue.poll();
         System.out.print(tempNode.data + " ");
         if (tempNode.left != null) {
              queue.add(tempNode.left);
         }
         if (tempNode.right != null) {
              queue.add(tempNode.right);
         }
     }
}
```

Practice Questions :- 
[Reverse Level Order](https://www.interviewbit.com/problems/reverse-level-order/)
[Right View of Binary tree](https://www.interviewbit.com/problems/right-view-of-binary-tree/)

**Search in Binary Tree**

The idea is to use tree traversals to traverse the tree and while traversing check if the current node matches with the given node. 

- Print YES if any node matches with the given node and return back.
- If the tree is completely traversed and none of the nodes matches then print NO.

```java
boolean ifNodeExists( Node root, int key)
{
    if(root == null)
        return false;
    if(root.data == key)
        return true;
    if(ifNodeExists(root.left, key) || ifNodeExists(root.right, key) )
        return true;
    return false;
}
```

## Binary Search Tree

A binary search tree (BST) is a sorted binary tree, where we can easily search for any key using the binary search algorithm. To sort the BST, it has to have the following properties: The node's left subtree contains only a key that's smaller than the node's key.

![Screenshot 2023-12-12 at 10.52.42 PM](https://hackmd.io/_uploads/rJV6dzLLa.png)

Let’s talk about the special properties of the Binary Search Tree. 

- The value of all the nodes in the left subtree of the root node is less than the value of the root node.
- The value of all the nodes in the right subtree is greater than the value of the root node.
- Properties 1 and 2 are true for all the nodes of the tree.

**Insertion in BST**
Let’s say the element we’re trying to insert is K.The algorithm doe insert operation in Binary tree is as follows :-

- if node == null, create a new node with the value of the key field equal to K. We return this newly created node directly from here.
- if K <= node.key, it means K must be inserted in the left subtree of the current node. We repeat(recur) the process from step 1 for the left subtree.
- else K > node.key, which means K must be inserted in the right subtree of the current node. We repeat(recur) the process from step 1 for the right subtree.
- Return the current node.

**Search in BST**

- if(node==null) k is not present in the tree.
- if(k<node.data), it means k must be present in the left subtree of the current node. We recur the process from step 1 for the left subtree.
- if(k > node.data), it means k must be present in the right subtree of the current node. We recur the process from step 1 for the right subtree.
- if( k == node.data ) we have found k so return true.
