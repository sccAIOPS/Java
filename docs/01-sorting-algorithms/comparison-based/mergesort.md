# MergeSort

> **Category:** Comparison-Based Sorting  
> **Paradigm:** Divide and Conquer  
> **In-Place:** No  
> **Stable:** Yes  
> **Adaptive:** No

---

## 📋 Overview

MergeSort is a divide-and-conquer algorithm that divides the input array into two halves, recursively sorts them, and then merges the sorted halves. It guarantees O(n log n) performance in all cases, making it predictable and reliable.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(n log n) |
| **Time (Average)** | O(n log n) |
| **Time (Worst)** | O(n log n) |
| **Space** | O(n) |
| **Stable** | Yes |
| **In-Place** | No |

---

## 🔬 Mathematical Analysis

### Algorithm Structure

MergeSort follows the divide-and-conquer paradigm:

1. **Divide:** Split array into two halves
2. **Conquer:** Recursively sort each half
3. **Combine:** Merge the two sorted halves

### Recurrence Relation

$$
T(n) = 2T\left(\frac{n}{2}\right) + O(n)
$$

Where:
- $2T(n/2)$ = time for sorting two halves
- $O(n)$ = time for merging

### Master Theorem Solution

Using the Master Theorem with $a = 2$, $b = 2$, $f(n) = n$:

$$
n^{\log_b a} = n^{\log_2 2} = n^1 = n
$$

Since $f(n) = \Theta(n^{\log_b a})$, Case 2 applies:

$$
T(n) = \Theta(n \log n)
$$

### Exact Comparison Count

The number of comparisons $C(n)$ satisfies:

$$
C(n) = C\left(\lfloor n/2 \rfloor\right) + C\left(\lceil n/2 \rceil\right) + \text{merge comparisons}
$$

For the merge step, worst-case comparisons = $n - 1$

$$
C(n) \approx n \log_2 n - n + 1 \approx n \log_2 n
$$

This is optimal for comparison-based sorting!

---

## 📝 Pseudocode

```
MERGE-SORT(A, left, right)
    if left < right then
        mid ← ⌊(left + right) / 2⌋
        
        // Divide
        MERGE-SORT(A, left, mid)
        MERGE-SORT(A, mid + 1, right)
        
        // Combine
        MERGE(A, left, mid, right)

MERGE(A, left, mid, right)
    // Copy to auxiliary array
    for i ← left to right do
        aux[i] ← A[i]
    
    i ← left      // pointer for left half
    j ← mid + 1   // pointer for right half
    
    // Merge back to A
    for k ← left to right do
        if i > mid then
            A[k] ← aux[j++]
        else if j > right then
            A[k] ← aux[i++]
        else if aux[j] < aux[i] then
            A[k] ← aux[j++]
        else
            A[k] ← aux[i++]
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/MergeSort.java](../../src/main/java/com/thealgorithms/sorts/MergeSort.java)

```java
@SuppressWarnings("rawtypes")
class MergeSort implements SortAlgorithm {

    private Comparable[] aux;

    @Override
    public <T extends Comparable<T>> T[] sort(T[] unsorted) {
        aux = new Comparable[unsorted.length];
        doSort(unsorted, 0, unsorted.length - 1);
        return unsorted;
    }

    private <T extends Comparable<T>> void doSort(T[] arr, int left, int right) {
        if (left < right) {
            int mid = (left + right) >>> 1;
            doSort(arr, left, mid);
            doSort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }

    @SuppressWarnings("unchecked")
    private <T extends Comparable<T>> void merge(T[] arr, int left, int mid, int right) {
        int i = left;
        int j = mid + 1;
        System.arraycopy(arr, left, aux, left, right + 1 - left);

        for (int k = left; k <= right; k++) {
            if (j > right) {
                arr[k] = (T) aux[i++];
            } else if (i > mid) {
                arr[k] = (T) aux[j++];
            } else if (less(aux[j], aux[i])) {
                arr[k] = (T) aux[j++];
            } else {
                arr[k] = (T) aux[i++];
            }
        }
    }
}
```

### Design Patterns Used

| Pattern | Usage |
|---------|-------|
| **Strategy** | Implements `SortAlgorithm` interface |
| **Divide & Conquer** | `doSort()` splits, `merge()` combines |
| **Generics** | `<T extends Comparable<T>>` |

---

## 🎯 Step-by-Step Example

### Input: `[38, 27, 43, 3, 9, 82, 10]`

**Divide Phase:**
```
                [38, 27, 43, 3, 9, 82, 10]
                           ↓
         [38, 27, 43, 3]          [9, 82, 10]
              ↓                        ↓
      [38, 27]    [43, 3]       [9, 82]    [10]
         ↓           ↓             ↓
     [38]  [27]  [43]  [3]     [9]  [82]   [10]
```

**Merge Phase:**
```
     [38]  [27]  [43]  [3]     [9]  [82]   [10]
         ↓           ↓             ↓
      [27, 38]    [3, 43]       [9, 82]    [10]
              ↓                        ↓
         [3, 27, 38, 43]          [9, 10, 82]
                           ↓
                [3, 9, 10, 27, 38, 43, 82]
```

### Final Result: `[3, 9, 10, 27, 38, 43, 82]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Notes |
|------|------------|-------|
| **Best** | O(n log n) | Always divides in half |
| **Average** | O(n log n) | Consistent performance |
| **Worst** | O(n log n) | No degradation |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary** | O(n) | For the merge operation |
| **Stack** | O(log n) | Recursion depth |
| **Total** | O(n) | Dominated by aux array |

### Why Stable?

During merge, when elements are equal:
```java
else if (less(aux[j], aux[i])) {
    arr[k] = (T) aux[j++];  // Only take from right if strictly less
} else {
    arr[k] = (T) aux[i++];  // Equal elements: prefer left (original order)
}
```

---

## ⚠️ Common Pitfalls

### 1. Memory Allocation in Recursion

**Problem:** Creating new arrays at each recursive call
```java
// BAD: O(n log n) space
private void merge(...) {
    T[] temp = new T[right - left + 1];  // Creates array each time
}
```

**Solution:** Allocate auxiliary array once
```java
// GOOD: O(n) space - this implementation
aux = new Comparable[unsorted.length];  // Once at start
```

### 2. Off-by-One Errors

**Problem:** Incorrect mid calculation or loop bounds
```java
// Common error: missing elements
int mid = (left + right) / 2;
doSort(arr, left, mid - 1);  // Wrong: misses mid element
doSort(arr, mid, right);

// Correct
int mid = (left + right) >>> 1;
doSort(arr, left, mid);
doSort(arr, mid + 1, right);
```

### 3. Forgetting to Copy Back

**Problem:** Merge results not written to original array
**Solution:** The implementation correctly merges back to `arr[]`

---

## 🔧 Optimizations

### 1. Skip Merge for Already Sorted Subarrays

```java
private <T extends Comparable<T>> void doSort(T[] arr, int left, int right) {
    if (left < right) {
        int mid = (left + right) >>> 1;
        doSort(arr, left, mid);
        doSort(arr, mid + 1, right);
        
        // Optimization: skip merge if already in order
        if (arr[mid].compareTo(arr[mid + 1]) <= 0) {
            return;  // Already sorted!
        }
        
        merge(arr, left, mid, right);
    }
}
```

### 2. Use Insertion Sort for Small Subarrays

```java
private static final int INSERTION_THRESHOLD = 15;

private <T extends Comparable<T>> void doSort(T[] arr, int left, int right) {
    if (right - left < INSERTION_THRESHOLD) {
        insertionSort(arr, left, right);
        return;
    }
    // ... rest of merge sort
}
```

### 3. Bottom-Up (Iterative) MergeSort

Avoids recursion overhead:
```java
public <T extends Comparable<T>> void bottomUpSort(T[] arr) {
    int n = arr.length;
    for (int size = 1; size < n; size *= 2) {
        for (int left = 0; left < n - size; left += 2 * size) {
            int mid = left + size - 1;
            int right = Math.min(left + 2 * size - 1, n - 1);
            merge(arr, left, mid, right);
        }
    }
}
```

---

## 🌍 Real-World Applications

| Application | Why MergeSort? |
|-------------|----------------|
| **External Sorting** | Perfect for sorting data on disk |
| **Linked Lists** | O(1) space when sorting linked lists |
| **Stability Required** | Database record sorting |
| **Parallel Processing** | Easily parallelizable |
| **Inversion Counting** | Modified merge counts inversions |

### Industry Usage

- **Java:** `Collections.sort()` uses modified MergeSort (TimSort)
- **Python:** `list.sort()` uses TimSort (hybrid with MergeSort)
- **Databases:** External merge sort for large datasets
- **Hadoop:** MapReduce shuffle phase

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/MergeSortTest.java](../../src/test/java/com/thealgorithms/sorts/MergeSortTest.java)

### Test Cases

| Test Case | Purpose |
|-----------|---------|
| Empty array | Edge case handling |
| Single element | Base case |
| Sorted input | Verify stability |
| Reverse sorted | Worst comparison count |
| Duplicates | Test stability |
| Large arrays | Performance |

---

## 📖 References

### Academic Papers

1. von Neumann, J. (1945). "First Draft of a Report on the EDVAC"
2. Knuth, D.E. *"The Art of Computer Programming"*, Vol. 3

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 2.3
- Sedgewick, R. *"Algorithms"*, Chapter 2.2

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [TimSort](./timsort.md) | Hybrid using MergeSort + InsertionSort |
| [QuickSort](./quicksort.md) | Another divide-and-conquer sort |
| [MergeSortNoExtraSpace](./mergesort-no-extra-space.md) | In-place variant |
| [MergeSortRecursive](./mergesort-recursive.md) | Alternative implementation |

---

[← Back to Sorting Algorithms](../README.md) | [Next: HeapSort →](./heapsort.md)
