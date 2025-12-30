# JumpSearch

> **Category:** Block-Based Search  
> **Prerequisite:** Sorted Array  
> **Paradigm:** Jump + Linear  
> **Approach:** Iterative

---

## 📋 Overview

Jump Search (also called Block Search) is a searching algorithm for sorted arrays. It works by jumping ahead by fixed steps and then performing a linear search within the identified block. It bridges the gap between Linear Search O(n) and Binary Search O(log n), achieving **O(√n) time complexity**.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(1) |
| **Time (Average)** | O(√n) |
| **Time (Worst)** | O(√n) |
| **Space** | O(1) |
| **Prerequisite** | Sorted array |

---

## 🔬 Mathematical Analysis

### Algorithm Concept

1. **Jump Phase:** Jump ahead by block size `m` until we find a block where `A[km] ≤ key < A[(k+1)m]`
2. **Linear Phase:** Linear search within that block

### Optimal Block Size

Let block size = $m$. Total operations:
$$
\text{jumps} + \text{linear search} = \frac{n}{m} + m
$$

To minimize, take derivative and set to zero:
$$
\frac{d}{dm}\left(\frac{n}{m} + m\right) = -\frac{n}{m^2} + 1 = 0
$$

Solving: $m = \sqrt{n}$

### Time Complexity Derivation

With optimal block size $m = \sqrt{n}$:
$$
T(n) = \frac{n}{\sqrt{n}} + \sqrt{n} = \sqrt{n} + \sqrt{n} = 2\sqrt{n} = O(\sqrt{n})
$$

### Comparison with Other Searches

| Algorithm | Time | Operations for n=1,000,000 |
|-----------|------|----------------------------|
| Linear | O(n) | 1,000,000 |
| **Jump** | **O(√n)** | **1,000** |
| Binary | O(log n) | 20 |

---

## 📝 Pseudocode

```
JUMP-SEARCH(A, key)
    n ← length(A)
    m ← floor(√n)  // Block size
    
    // Jump phase: find the block
    limit ← m
    while limit < n AND A[limit] < key do
        limit ← min(limit + m, n - 1)
    
    // Linear phase: search within block
    start ← limit - m
    for i ← start to limit do
        if i < n AND A[i] = key then
            return i
    
    return -1
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/searches/JumpSearch.java](../../src/main/java/com/thealgorithms/searches/JumpSearch.java)

```java
/**
 * An implementation of the Jump Search algorithm.
 *
 * <p>
 * Jump Search is an algorithm for searching sorted arrays. It works by dividing the array
 * into blocks of a fixed size (the block size is typically the square root of the array length)
 * and jumping ahead by this block size to find a range where the target element may be located.
 * Once the range is found, a linear search is performed within that block.
 *
 * <p>
 * The Jump Search algorithm is particularly effective for large sorted arrays where the cost of
 * performing a linear search on the entire array would be prohibitive.
 *
 * <p>
 * Worst-case performance: O(√N)<br>
 * Best-case performance: O(1)<br>
 * Average performance: O(√N)<br>
 * Worst-case space complexity: O(1)
 *
 * <p>
 * This class implements the {@link SearchAlgorithm} interface, providing a generic search method
 * for any comparable type.
 */
public class JumpSearch implements SearchAlgorithm {

    /**
     * Jump Search algorithm implementation.
     *
     * @param array the sorted array containing elements
     * @param key   the element to be searched
     * @return the index of {@code key} if found, otherwise -1
     */
    @Override
    public <T extends Comparable<T>> int find(T[] array, T key) {
        int length = array.length;
        int blockSize = (int) Math.sqrt(length);

        int limit = blockSize;
        // Jumping ahead to find the block where the key may be located
        while (limit < length && key.compareTo(array[limit]) > 0) {
            limit = Math.min(limit + blockSize, length - 1);
        }

        // Perform linear search within the identified block
        for (int i = limit - blockSize; i <= limit && i < length; i++) {
            if (array[i].equals(key)) {
                return i;
            }
        }
        return -1;
    }
}
```

### Implementation Features

| Feature | Description |
|---------|-------------|
| **Generic** | Works with any `Comparable<T>` type |
| **Optimal block size** | Uses `√n` |
| **Bounds checking** | Prevents array overflow |
| **SearchAlgorithm** | Implements standard interface |

---

## 🎯 Step-by-Step Example

### Input: Array `[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377]`, Key = `55`

**Array length:** n = 15  
**Block size:** m = √15 ≈ 3

**Jump Phase:**

```
Block 0: [0, 1, 1]     limit=3, A[3]=2 < 55 → jump
         ↑
Block 1: [2, 3, 5]     limit=6, A[6]=8 < 55 → jump
            ↑
Block 2: [8, 13, 21]   limit=9, A[9]=34 < 55 → jump
               ↑
Block 3: [34, 55, 89]  limit=12, A[12]=144 > 55 → STOP!
                  ↑

Key is in block starting at index 9 (limit - blockSize = 12 - 3 = 9)
```

**Linear Phase:**

```
Search [34, 55, 89] from index 9 to 12:

i=9:  A[9]=34 ≠ 55
i=10: A[10]=55 == 55 → FOUND at index 10 ✅
```

### Result: Index `10` (4 jumps + 2 comparisons = 6 operations)

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When |
|------|------------|------|
| **Best** | O(1) | Target at first position |
| **Average** | O(√n) | Target in middle blocks |
| **Worst** | O(√n) | Target at end |

### Operations Breakdown

| Phase | Maximum Operations |
|-------|-------------------|
| Jump phase | $\frac{n}{\sqrt{n}} = \sqrt{n}$ |
| Linear phase | $\sqrt{n}$ |
| **Total** | $2\sqrt{n} = O(\sqrt{n})$ |

### Comparison Table

| Algorithm | Time | n=10,000 | n=1,000,000 |
|-----------|------|----------|-------------|
| Linear | O(n) | 10,000 | 1,000,000 |
| **Jump** | **O(√n)** | **100** | **1,000** |
| Binary | O(log n) | 14 | 20 |

---

## ⚠️ Common Pitfalls

### 1. Incorrect Block Size

```java
// Wrong: fixed block size
int blockSize = 10;  // Not optimal!

// Correct: use square root
int blockSize = (int) Math.sqrt(length);
```

### 2. Array Bounds Overflow

```java
// Wrong: can overflow
while (limit < length && array[limit] < key) {
    limit += blockSize;  // Can exceed array bounds!
}

// Correct: cap at array end
while (limit < length && key.compareTo(array[limit]) > 0) {
    limit = Math.min(limit + blockSize, length - 1);
}
```

### 3. Missing Edge Case for Last Block

```java
// Wrong: linear search may go out of bounds
for (int i = limit - blockSize; i <= limit; i++) {
    if (array[i] == key) return i;  // Can overflow!
}

// Correct: check bounds
for (int i = limit - blockSize; i <= limit && i < length; i++) {
    if (array[i].equals(key)) return i;
}
```

---

## 🔧 Optimizations

### 1. Backward Jump Search

When target is more likely near the end:

```java
public int backwardJumpSearch(int[] array, int key) {
    int n = array.length;
    int blockSize = (int) Math.sqrt(n);
    
    int limit = n - 1;
    // Jump backward from end
    while (limit > 0 && array[limit] > key) {
        limit = Math.max(limit - blockSize, 0);
    }
    
    // Linear search forward in block
    for (int i = limit; i < Math.min(limit + blockSize, n); i++) {
        if (array[i] == key) return i;
    }
    return -1;
}
```

### 2. Binary Search in Block

Replace linear search with binary search for even better performance:

```java
public int jumpBinarySearch(int[] array, int key) {
    int n = array.length;
    int blockSize = (int) Math.sqrt(n);
    
    int limit = blockSize;
    while (limit < n && array[limit] < key) {
        limit = Math.min(limit + blockSize, n - 1);
    }
    
    // Binary search within block instead of linear
    int left = limit - blockSize;
    int right = Math.min(limit, n - 1);
    return Arrays.binarySearch(array, left, right + 1, key);
}
```

This gives O(√n + log √n) = O(√n) but with a smaller constant.

### 3. Variable Block Size

Adapt block size based on distribution:

```java
// For exponentially distributed data
int blockSize = (int) Math.sqrt(n) * 2;  // Larger blocks
```

---

## 🌍 Real-World Applications

| Application | Why Jump Search? |
|-------------|------------------|
| **Systems with slow random access** | Sequential jumps are cache-friendly |
| **Sorted linked lists** | Forward traversal only |
| **Tape/disk storage** | Sequential access is faster |
| **Large sorted files** | Block-based file I/O |

### Jump Search vs Binary Search

| Criterion | Jump Search | Binary Search |
|-----------|-------------|---------------|
| Random access cost | Low (better) | High |
| Sequential access | Exploits | Doesn't exploit |
| Cache locality | Good | Poor |
| Implementation | Simple | Simple |
| Time complexity | O(√n) | O(log n) |

**Use Jump Search when:**
- Random access is expensive
- Sequential access is cheap
- Data is on sequential storage (tapes, linked lists)

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/JumpSearchTest.java](../../src/test/java/com/thealgorithms/searches/JumpSearchTest.java)

### Test Cases

| Input Array | Key | Expected | Tests |
|-------------|-----|----------|-------|
| `[1,3,5,7,9,11]` | 7 | 3 | Normal case |
| `[1,3,5,7,9,11]` | 1 | 0 | First element |
| `[1,3,5,7,9,11]` | 11 | 5 | Last element |
| `[1,3,5,7,9,11]` | 6 | -1 | Not found |
| `[5]` | 5 | 0 | Single element |
| Large array (n=10000) | Last | 9999 | Performance test |

---

## 📖 References

### Academic Sources

- Shneiderman, B. *"Jump Searching: A Fast Sequential Search Technique"*, Communications of the ACM, 1978

### Textbooks

- Knuth, D.E. *"The Art of Computer Programming"*, Vol. 3

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [LinearSearch](./linearsearch.md) | O(n), used in second phase |
| [BinarySearch](./binarysearch.md) | O(log n), more efficient |
| [ExponentialSearch](./exponential-search.md) | Similar jump concept |
| [FibonacciSearch](./fibonacci-search.md) | Uses Fibonacci steps |

---

## 📊 Visualization

```
Array: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377]
       0  1  2  3  4  5  6   7   8   9  10  11   12   13   14

Block size m = √15 ≈ 3

Searching for 55:

Jump Phase:
┌─────────┬─────────┬─────────┬─────────┬─────────┐
│ Block 0 │ Block 1 │ Block 2 │ Block 3 │ Block 4 │
│ [0,1,1] │ [2,3,5] │[8,13,21]│[34,55,89]│[144...] │
└────↓────┴────↓────┴────↓────┴────↑────┴─────────┘
     │        │        │        │
  2<55     5<55    21<55    89>55 STOP!
   jump    jump    jump

Linear Phase:
┌───────────────────────────────────────────────────┐
│                           [34, 55, 89]            │
│                             ↑                     │
│                         34 ≠ 55                   │
│                                 ↑                 │
│                             55 = 55 ✅ FOUND!     │
└───────────────────────────────────────────────────┘

Result: index 10
Jumps: 3 | Linear comparisons: 2 | Total: 5 operations
```

---

## 💡 When to Choose Jump Search

```
Decision Flow:

Is data sorted?
├── No → Use Linear Search or sort first
└── Yes
    └── What's the access pattern cost?
        ├── Random access cheap → Use Binary Search
        └── Sequential access cheap → Use Jump Search
            └── Examples:
                • Linked lists
                • Tape storage
                • Network streams
                • Large files on HDD
```

---

[← Back to Searching Algorithms](../README.md) | [Next: ExponentialSearch →](./exponentialsearch.md)
