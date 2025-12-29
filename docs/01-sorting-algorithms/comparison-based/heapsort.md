# HeapSort

> **Category:** Comparison-Based Sorting  
> **Paradigm:** Selection Sort (using Heap)  
> **In-Place:** Yes  
> **Stable:** No  
> **Adaptive:** No

---

## 📋 Overview

HeapSort uses a binary heap data structure to sort elements. It first builds a max-heap from the input, then repeatedly extracts the maximum element and places it at the end of the array. It guarantees O(n log n) in all cases with O(1) auxiliary space.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(n log n) |
| **Time (Average)** | O(n log n) |
| **Time (Worst)** | O(n log n) |
| **Space** | O(1) |
| **Stable** | No |
| **In-Place** | Yes |

---

## 🔬 Mathematical Analysis

### Binary Heap Properties

A **max-heap** is a complete binary tree where:
$$
\forall i > 1: A[\text{parent}(i)] \geq A[i]
$$

For 0-indexed array:
- **Parent:** $\lfloor (i-1)/2 \rfloor$
- **Left child:** $2i + 1$
- **Right child:** $2i + 2$

For 1-indexed (as in implementation):
- **Parent:** $\lfloor i/2 \rfloor$
- **Left child:** $2i$
- **Right child:** $2i + 1$

### Time Complexity Analysis

**Build Heap:** O(n)

Intuition: Most nodes are near the bottom (less sifting)
$$
T_{\text{build}} = \sum_{h=0}^{\lfloor\log n\rfloor} \left\lceil \frac{n}{2^{h+1}} \right\rceil \cdot O(h) = O(n)
$$

**Extract Max (n times):** O(n log n)

Each extraction requires O(log n) to restore heap property:
$$
T_{\text{extract}} = n \cdot O(\log n) = O(n \log n)
$$

**Total:**
$$
T(n) = O(n) + O(n \log n) = O(n \log n)
$$

### Heap Height

For a heap of $n$ elements:
$$
h = \lfloor \log_2 n \rfloor
$$

---

## 📝 Pseudocode

```
HEAP-SORT(A)
    n ← length(A)
    
    // Phase 1: Build max-heap
    BUILD-MAX-HEAP(A, n)
    
    // Phase 2: Extract elements
    for i ← n-1 downto 1 do
        SWAP(A[0], A[i])      // Move max to end
        SIFT-DOWN(A, 0, i)    // Restore heap property

BUILD-MAX-HEAP(A, n)
    // Start from last non-leaf node
    for i ← ⌊n/2⌋ downto 0 do
        SIFT-DOWN(A, i, n)

SIFT-DOWN(A, i, n)
    while LEFT(i) < n do
        largest ← i
        left ← 2*i + 1
        right ← 2*i + 2
        
        if left < n and A[left] > A[largest] then
            largest ← left
        if right < n and A[right] > A[largest] then
            largest ← right
            
        if largest = i then
            break
        
        SWAP(A[i], A[largest])
        i ← largest
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/HeapSort.java](../../src/main/java/com/thealgorithms/sorts/HeapSort.java)

```java
public class HeapSort implements SortAlgorithm {

    /**
     * Uses 1-based indexing for simpler child/parent calculations.
     * Adjusts indices when accessing array elements.
     */
    @Override
    public <T extends Comparable<T>> T[] sort(T[] array) {
        int n = array.length;
        heapify(array, n);
        while (n > 1) {
            SortUtils.swap(array, 0, n - 1);
            n--;
            siftDown(array, 1, n);
        }
        return array;
    }

    private <T extends Comparable<T>> void heapify(final T[] array, final int n) {
        for (int k = n / 2; k >= 1; k--) {
            siftDown(array, k, n);
        }
    }

    private <T extends Comparable<T>> void siftDown(final T[] array, int k, final int n) {
        while (2 * k <= n) {
            int j = 2 * k;
            if (j < n && SortUtils.less(array[j - 1], array[j])) {
                j++;
            }
            if (!SortUtils.less(array[k - 1], array[j - 1])) {
                break;
            }
            SortUtils.swap(array, k - 1, j - 1);
            k = j;
        }
    }
}
```

### Implementation Notes

The implementation uses **1-based indexing** internally:
- Simplifies parent/child calculations
- Array access adjusts with `-1`: `array[k - 1]`

---

## 🎯 Step-by-Step Example

### Input: `[4, 10, 3, 5, 1]`

**Phase 1: Build Max-Heap**

```
Initial array as tree:
        4
       / \
      10   3
     / \
    5   1

After heapify:
        10
       /  \
      5    3
     / \
    4   1

Array: [10, 5, 3, 4, 1]
```

**Phase 2: Extract Max**

```
Step 1: Swap 10 with 1, sift down
        1              5
       / \    →       / \
      5   3          4   3
     /              /
    4              1
    
Sorted: [..., 10]
Array: [5, 4, 3, 1, 10]

Step 2: Swap 5 with 1, sift down
        1              4
       / \    →       / \
      4   3          1   3

Sorted: [..., 5, 10]
Array: [4, 1, 3, 5, 10]

Step 3: Swap 4 with 3, sift down
        3              3
       /      →       /
      1              1

Sorted: [..., 4, 5, 10]
Array: [3, 1, 4, 5, 10]

Step 4: Swap 3 with 1
Sorted: [1, 3, 4, 5, 10]
```

### Final Result: `[1, 3, 4, 5, 10]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Phase | Complexity | Operations |
|-------|------------|------------|
| Build Heap | O(n) | Sift down n/2 elements |
| Extract Max | O(n log n) | n extractions × log n sift |
| **Total** | **O(n log n)** | Guaranteed |

### Why Build-Heap is O(n)?

| Level | Nodes | Sift Distance | Work |
|-------|-------|---------------|------|
| 0 (root) | 1 | log n | log n |
| 1 | 2 | log n - 1 | 2(log n - 1) |
| ... | ... | ... | ... |
| h-1 | n/2 | 1 | n/2 |

Sum: $\sum_{i=0}^{h} 2^i \cdot (h-i) = O(n)$

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary** | O(1) | In-place sorting |
| **Stack** | O(1) | Iterative implementation |

---

## ⚠️ Common Pitfalls

### 1. Off-by-One Errors with Indexing

**Problem:** Mixing 0-based and 1-based indexing
```java
// This implementation uses 1-based indexing
// Array access: array[k - 1]
// Child of k: 2*k and 2*k + 1
```

### 2. Not Restoring Heap After Extraction

**Problem:** Forgetting to sift down after swapping
```java
// Correct sequence:
SortUtils.swap(array, 0, n - 1);  // Move max to end
n--;                               // Reduce heap size
siftDown(array, 1, n);            // Restore heap property
```

### 3. Not Stable

**Problem:** Equal elements may change order
```
[5a, 5b, 3] → heap operations → [3, 5b, 5a]
// 5a and 5b swapped relative position
```

---

## 🔧 Optimizations

### 1. Bottom-Up Heap Construction

Already implemented - O(n) vs O(n log n) for top-down.

### 2. Floyd's Heap Construction

Alternative sift-down approach (what this implementation uses):
```java
for (int k = n / 2; k >= 1; k--) {
    siftDown(array, k, n);
}
```

### 3. Ternary Heap

Use 3-way heap for better cache performance:
```java
// Children of k: 3k-1, 3k, 3k+1
// Parent of k: (k+1)/3
```

---

## 🌍 Real-World Applications

| Application | Why HeapSort? |
|-------------|---------------|
| **Priority Queues** | Heap is the foundation |
| **Embedded Systems** | O(1) space, no recursion |
| **Selection Algorithms** | Find k-th largest efficiently |
| **Graph Algorithms** | Dijkstra, Prim use heaps |
| **Real-time Systems** | Predictable O(n log n) |

### Comparison with Other Sorts

| Criterion | HeapSort | QuickSort | MergeSort |
|-----------|----------|-----------|-----------|
| Worst case | O(n log n) | O(n²) | O(n log n) |
| Space | O(1) | O(log n) | O(n) |
| Cache | Poor | Good | Good |
| Stable | No | No | Yes |

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/HeapSortTest.java](../../src/test/java/com/thealgorithms/sorts/HeapSortTest.java)

### Test Cases

| Test Case | Input | Purpose |
|-----------|-------|---------|
| Empty | `[]` | Edge case |
| Single | `[5]` | Base case |
| Sorted | `[1,2,3,4,5]` | Already ordered |
| Reverse | `[5,4,3,2,1]` | Worst initial heap |
| Duplicates | `[3,1,3,2,3]` | Multiple equals |

---

## 📖 References

### Academic Papers

1. Williams, J.W.J. (1964). "Algorithm 232 - Heapsort". *Communications of the ACM*.
2. Floyd, R.W. (1964). "Algorithm 245 - Treesort 3". *Communications of the ACM*.

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 6
- Sedgewick, R. *"Algorithms"*, Section 2.4

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [Priority Queue](../../04-data-structures/heaps/priority-queue.md) | Uses same heap structure |
| [QuickSort](./quicksort.md) | Comparison: different tradeoffs |
| [IntroSort](./introsort.md) | Uses HeapSort as fallback |

---

## 📊 Visualization

```
Build Max-Heap:
[4, 10, 3, 5, 1]
      4              10
     / \    →       /  \
   10   3          5    3
   / \            / \
  5   1          4   1

Extract Phase:
     10                5                4
    /  \   swap      /  \   swap      /  \
   5    3   →       4    3   →       1    3
  / \              /
 4   1            1
 
[5, 4, 3, 1, 10]  [4, 1, 3, 5, 10]  [3, 1, 4, 5, 10]

Final: [1, 3, 4, 5, 10]
```

---

[← Back to Sorting Algorithms](../README.md) | [Next: BubbleSort →](./bubblesort.md)
