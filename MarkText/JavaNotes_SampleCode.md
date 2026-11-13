## Arrays

Filling of Arrays without for loop ->

```java
// Initializes each row by generating a filled 1D array on the fly
Arrays.setAll(matrix, i -> {
    int[] row = new int[cols];
    Arrays.fill(row, defaultValue);
    return row;
});

// The outer loop just iterates references; the actual filling is native
for (int[] row : matrix) {
    Arrays.fill(row, defaultValue);
}

// Array Sorting
int[] arr = {5, 4, 3, 2, 1};

Arrays.sort(arr, 1, 4);
Arrays.sort(arr, Collections.reverseOrder());
Arrays.sort(arr, (a, b) -> b - a);


// Arrays copy
// primites type array does DEEP COPY
int[] original = {1, 2, 3};
int[] cloned = original.clone(); // Duplicates values into a new object

// Object array does Shallow copy
        Person[] original = { new Person("Alice"), new Person("Bob") };
        Person[] cloned = original.clone();

        // Modifying the object inside the cloned array
        cloned[0].name = "Charlie";

        // Both point to the same object!
        System.out.println(original[0].name); // Prints "Charlie"
        System.out.println(cloned[0].name);   // Prints "Charlie"


// Multi dimensional array does Shallow copy on the inner arrays
        int[][] original = { {1, 2}, {3, 4} };
        int[][] cloned = original.clone();

        System.out.println(original == cloned);       // Prints false (Outer arrays are distinct)
        System.out.println(original[0] == cloned[0]); // Prints true (Inner arrays are SHARED)
```

## 

| Method                                                                                 | Best Used For                             | Notes                                                                                  |
| -------------------------------------------------------------------------------------- | ----------------------------------------- | -------------------------------------------------------------------------------------- |
| **`array.clone()`**                                                                    | Quick duplication of 1D primitive arrays. | No imports required; syntax is very concise.                                           |
| **[Arrays.copyOf()](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)** | Resizing arrays during duplication.       | Truncates or pads the new array with zeros/nulls if sizes differ.                      |
| **`System.arraycopy()`**                                                               | High-performance partial copying.         | Highly optimized native method; lets you specify index ranges.                         |
| **Manual `for` Loop**                                                                  | Custom mutations or **Deep Copying**.     | The only straightforward way to build a completely independent multi-dimensional copy. |

## List

```java
// Convert List to Array
List<Integer> ansList = new ArrayList<>();
int[] ansArr = ansList.stream().mapToInt(Integer::intValue).toArray();


// Sorting
List<Integer> numbers = new ArrayList<>(List.of(5, 1, 9, 3));

// Sorts in descending order
Collections.sort(numbers, Collections.reverseOrder());
System.out.println(numbers); // Output: [9, 5, 3, 1]

// Create list with Default values
List<Integer> lMax = new ArrayList<Integer>(Collections.nCopies(n,0));
```

## PriorityQueue

```java
// PriorityQueue with int[] type.
PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));
minHeap.add(new int[]{0, C});
```

## Queue

```java
// Create a queue that stores integer arrays primitive type
Queue<int[]> queue = new LinkedList<>();
 // Add int arrays to the queue
queue.add(new int[]{1, 2, 3});
queue.add(new int[]{4, 5});
```

## Stack, Treeset, Deque

```java
// Stack - Using Stack class
Stack<Integer> st = new Stack<>();
// Stack - Using ArrayDeque (recommended)
Deque<Integer> st = new ArrayDeque<>();

// Using LinkedList
Queue<Integer> q = new LinkedList<>();
// Using ArrayDeque (recommended for ordinary FIFO queues)
Queue<Integer> q = new ArrayDeque<>();


// TreeSet - Ascending order (default)
TreeSet<Integer> ts = new TreeSet<>();
// TreeSet - Descending order
TreeSet<Integer> ts =
    new TreeSet<>(Collections.reverseOrder());
// Useful methods
ts.first();
ts.last();
ts.lower(20);
ts.higher(20);
ts.floor(20);
ts.ceiling(20);
```

## All Data Structures in a glance

| Data structure | Declaration and instantiation                        | Import                    |
| -------------- | ---------------------------------------------------- | ------------------------- |
| PriorityQueue  | `PriorityQueue<Integer> pq = new PriorityQueue<>();` | `java.util.PriorityQueue` |
| ArrayDeque     | `Deque<Integer> dq = new ArrayDeque<>();`            | `java.util.*`             |
| TreeSet        | `TreeSet<Integer> ts = new TreeSet<>();`             | `java.util.TreeSet`       |
| Queue          | `Queue<Integer> q = new LinkedList<>();`             | `java.util.*`             |
| Stack          | `Stack<Integer> st = new Stack<>();`                 | `java.util.Stack`         |

Min Heap (default)

```
PriorityQueue<Integer> pq = new PriorityQueue<>();
```

Max Heap

```
PriorityQueue<Integer> pq =
    new PriorityQueue<>(Collections.reverseOrder());
```

Custom comparator

```
PriorityQueue<Integer> pq =
    new PriorityQueue<>((a, b) -> b - a);
```

### TreeSet

Ascending order (default)

```
TreeSet<Integer> ts = new TreeSet<>();
```

Descending order

```
TreeSet<Integer> ts =
    new TreeSet<>(Collections.reverseOrder());
```

## Quick reference – Which one to use?

| Requirement                       | Recommended instantiation                         |
| --------------------------------- | ------------------------------------------------- |
| Min Heap                          | `new PriorityQueue<>()`                           |
| Max Heap                          | `new PriorityQueue<>(Collections.reverseOrder())` |
| Stack (LIFO)                      | `new ArrayDeque<>()`                              |
| Queue (FIFO)                      | `new ArrayDeque<>()`                              |
| Sorted unique elements            | `new TreeSet<>()`                                 |
| Descending sorted unique elements | `new TreeSet<>(Collections.reverseOrder())`       |
