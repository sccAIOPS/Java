# QuickSort

> **Category:** Comparison-Based Sorting  
> **Paradigm:** Divide and Conquer  
> **In-Place:** Yes  
> **Stable:** No  
> **Adaptive:** No (randomized version mitigates worst-case)

---

## 📋 Overview

QuickSort is one of the most efficient and widely-used sorting algorithms. It works by selecting a "pivot" element and partitioning the array around it, placing smaller elements to the left and larger elements to the right. The process is then recursively applied to the subarrays.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(n log n) |
| **Time (Average)** | O(n log n) |
| **Time (Worst)** | O(n²) |
| **Space** | O(log n) |
| **Stable** | No |
| **In-Place** | Yes |

---

## 🔬 Mathematical Analysis

### Algorithm Intuition

QuickSort divides the problem into smaller subproblems through partitioning:

1. Choose a **pivot** element
2. **Partition** the array so that:
   - All elements ≤ pivot are on the left
   - All elements ≥ pivot are on the right
3. **Recursively** sort the left and right subarrays

### Recurrence Relation

**Best/Average Case:**

When the pivot divides the array roughly in half:

$$
T(n) = 2T\left(\frac{n}{2}\right) + O(n)
$$

By the Master Theorem (Case 2):
$$
T(n) = O(n \log n)
$$

**Worst Case:**

When the pivot is always the smallest or largest element:

$$
T(n) = T(n-1) + O(n) = O(n^2)
$$

### Expected Comparisons

The expected number of comparisons for randomized QuickSort:

$$
C(n) = 2n \ln n \approx 1.39 \cdot n \log_2 n
$$

This is only about 39% more than the theoretical minimum of $n \log_2 n$.

### Partition Analysis

For a partition with pivot at position $k$:

$$
\text{Comparisons} = n - 1
$$

The probability that element $i$ and $j$ are compared equals:
$$
P(i, j \text{ compared}) = \frac{2}{|j - i| + 1}
$$

---

## 📝 Pseudocode

```
QUICKSORT(A, low, high)
    if low < high then
        // Partition the array and get pivot position
        pivotIndex ← PARTITION(A, low, high)
        
        // Recursively sort left subarray
        QUICKSORT(A, low, pivotIndex - 1)
        
        // Recursively sort right subarray
        QUICKSORT(A, pivotIndex + 1, high)

PARTITION(A, low, high)
    pivot ← A[high]                    // Choose last element as pivot
    i ← low - 1                        // Index of smaller element
    
    for j ← low to high - 1 do
        if A[j] ≤ pivot then
            i ← i + 1
            SWAP(A[i], A[j])
    
    SWAP(A[i + 1], A[high])            // Place pivot in correct position
    return i + 1                        // Return pivot index
```

### Randomized Partition

```
RANDOMIZED-PARTITION(A, low, high)
    // Choose random pivot to avoid worst-case on sorted input
    randomIndex ← RANDOM(low, high)
    SWAP(A[randomIndex], A[high])
    return PARTITION(A, low, high)
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/QuickSort.java](../../src/main/java/com/thealgorithms/sorts/QuickSort.java)

```java
class QuickSort implements SortAlgorithm {

    @Override
    public <T extends Comparable<T>> T[] sort(T[] array) {
        doSort(array, 0, array.length - 1);
        return array;
    }

    private static <T extends Comparable<T>> void doSort(T[] array, final int left, final int right) {
        if (left < right) {
            final int pivot = randomPartition(array, left, right);
            doSort(array, left, pivot - 1);
            doSort(array, pivot, right);
        }
    }

    private static <T extends Comparable<T>> int randomPartition(T[] array, final int left, final int right) {
        final int randomIndex = left + (int) (Math.random() * (right - left + 1));
        SortUtils.swap(array, randomIndex, right);
        return partition(array, left, right);
    }

    private static <T extends Comparable<T>> int partition(T[] array, int left, int right) {
        final int mid = (left + right) >>> 1;
        final T pivot = array[mid];
        
        while (left <= right) {
            while (SortUtils.less(array[left], pivot)) {
                ++left;
            }
            while (SortUtils.less(pivot, array[right])) {
                --right;
            }
            if (left <= right) {
                SortUtils.swap(array, left, right);
                ++left;
                --right;
            }
        }
        return left;
    }
}
```

### Design Patterns Used

| Pattern | Usage |
|---------|-------|
| **Strategy** | Implements `SortAlgorithm` interface |
| **Template Method** | `sort()` → `doSort()` → `partition()` |
| **Generics** | `<T extends Comparable<T>>` for type safety |

---

## 🎯 Step-by-Step Example

### Input: `[8, 3, 7, 1, 5, 9, 2]`

**Step 1: Initial partition (pivot = 5)**
```
[8, 3, 7, 1, 5, 9, 2]
         ↑ pivot

After partition:
[2, 3, 1] [5] [7, 9, 8]
  left     ↑    right
```

**Step 2: Sort left subarray [2, 3, 1] (pivot = 3)**
```
[2, 3, 1]
    ↑ pivot

After partition:
[2, 1] [3]
```

**Step 3: Sort [2, 1] (pivot = 1)**
```
[2, 1]
    ↑ pivot

After partition:
[1] [2]
```

**Step 4: Sort right subarray [7, 9, 8] (pivot = 9)**
```
[7, 9, 8]
    ↑ pivot

After partition:
[7, 8] [9]
```

**Step 5: Sort [7, 8] (pivot = 7)**
```
[7, 8]
 ↑ pivot

After partition:
[7] [8]
```

### Final Result: `[1, 2, 3, 5, 7, 8, 9]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it Occurs |
|------|------------|----------------|
| **Best** | O(n log n) | Pivot always divides array in half |
| **Average** | O(n log n) | Random pivot selection |
| **Worst** | O(n²) | Already sorted + poor pivot (first/last) |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary** | O(log n) | Recursion stack depth |
| **Worst Stack** | O(n) | Unbalanced partitions |

### Comparison with Other Sorts

| Algorithm | Average | Worst | Space | Stable |
|-----------|---------|-------|-------|--------|
| **QuickSort** | O(n log n) | O(n²) | O(log n) | No |
| MergeSort | O(n log n) | O(n log n) | O(n) | Yes |
| HeapSort | O(n log n) | O(n log n) | O(1) | No |
| TimSort | O(n log n) | O(n log n) | O(n) | Yes |

---

## ⚠️ Common Pitfalls

### 1. Stack Overflow on Large Arrays

**Problem:** Deep recursion on unbalanced partitions
```java
// Worst case: sorted array with first element as pivot
// Recursion depth: O(n)
```

**Solution:** Use randomized pivot or tail-call optimization

### 2. Worst-Case on Already Sorted Data

**Problem:** O(n²) performance when array is sorted
**Solution:** This implementation uses `randomPartition()` to mitigate

### 3. Not Stable

**Problem:** Equal elements may change relative order
```java
// [5a, 5b, 3] might become [3, 5b, 5a]
```

**Solution:** Use MergeSort if stability is required

### 4. Integer Overflow in Mid Calculation

**Problem:** `(left + right) / 2` can overflow
```java
// Correct approach (used in implementation):
final int mid = (left + right) >>> 1;  // Unsigned right shift
```

---

## 🔧 Optimizations

### 1. Median-of-Three Pivot Selection

```java
private static <T extends Comparable<T>> int medianOfThree(T[] arr, int lo, int hi) {
    int mid = (lo + hi) >>> 1;
    if (SortUtils.less(arr[mid], arr[lo])) SortUtils.swap(arr, lo, mid);
    if (SortUtils.less(arr[hi], arr[lo])) SortUtils.swap(arr, lo, hi);
    if (SortUtils.less(arr[hi], arr[mid])) SortUtils.swap(arr, mid, hi);
    return mid;  // median is now at mid
}
```

### 2. Insertion Sort for Small Subarrays

```java
private static final int INSERTION_THRESHOLD = 10;

private static <T extends Comparable<T>> void doSort(T[] array, int left, int right) {
    if (right - left < INSERTION_THRESHOLD) {
        insertionSort(array, left, right);
        return;
    }
    // ... rest of quicksort
}
```

### 3. Three-Way Partitioning (Dutch National Flag)

Handles many duplicate elements efficiently:

```java
// Partition into: [< pivot | == pivot | > pivot]
// O(n) for arrays with many duplicates
```

### 4. Tail Call Optimization

```java
private static <T extends Comparable<T>> void doSortOptimized(T[] array, int left, int right) {
    while (left < right) {
        int pivot = partition(array, left, right);
        // Recurse on smaller partition, iterate on larger
        if (pivot - left < right - pivot) {
            doSortOptimized(array, left, pivot - 1);
            left = pivot + 1;  // Tail call elimination
        } else {
            doSortOptimized(array, pivot + 1, right);
            right = pivot - 1;
        }
    }
}
```

---

## 🌍 Real-World Applications

| Application | Why QuickSort? |
|-------------|----------------|
| **Standard Libraries** | Java's `Arrays.sort()` uses dual-pivot QuickSort |
| **Database Systems** | Fast in-memory sorting for query results |
| **File Systems** | Sorting directory entries |
| **Networking** | Sorting packets by priority |
| **Gaming** | Sorting objects by z-order for rendering |

### Industry Usage

- **Java:** `Arrays.sort()` for primitives uses dual-pivot QuickSort
- **C++:** `std::sort()` uses Introsort (QuickSort + HeapSort hybrid)
- **Python:** `list.sort()` uses TimSort (but QuickSort for certain cases)
- **Unix:** `qsort()` in standard C library

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/QuickSortTest.java](../../src/test/java/com/thealgorithms/sorts/QuickSortTest.java)

```java
class QuickSortTest extends SortingAlgorithmTest {
    @Override
    SortAlgorithm getSortAlgorithm() {
        return new QuickSort();
    }
}
```

### Test Cases to Consider

| Test Case | Input | Expected |
|-----------|-------|----------|
| Empty array | `[]` | `[]` |
| Single element | `[5]` | `[5]` |
| Already sorted | `[1,2,3,4,5]` | `[1,2,3,4,5]` |
| Reverse sorted | `[5,4,3,2,1]` | `[1,2,3,4,5]` |
| All duplicates | `[3,3,3,3]` | `[3,3,3,3]` |
| Random | `[3,1,4,1,5,9]` | `[1,1,3,4,5,9]` |
| Negative numbers | `[-3,1,-4]` | `[-4,-3,1]` |

---

## 📖 References

### Academic Papers

1. Hoare, C.A.R. (1961). "Algorithm 64: Quicksort". *Communications of the ACM*.
2. Sedgewick, R. (1978). "Implementing Quicksort Programs". *Communications of the ACM*.
3. Bentley, J.L., McIlroy, M.D. (1993). "Engineering a Sort Function". *Software—Practice and Experience*.

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 7
- Sedgewick, R. *"Algorithms"*, Chapter 2.3

### Online Resources

- [Visualgo - QuickSort Visualization](https://visualgo.net/en/sorting)
- [Wikipedia - Quicksort](https://en.wikipedia.org/wiki/Quicksort)

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [DualPivotQuickSort](./dual-pivot-quicksort.md) | Uses two pivots for better performance |
| [MergeSort](./mergesort.md) | Another O(n log n) divide-and-conquer sort |
| [HeapSort](./heapsort.md) | O(n log n) guaranteed, in-place |
| [IntroSort](./introsort.md) | QuickSort + HeapSort hybrid |

---

## 📊 Visualization

```
Initial:  [8, 3, 7, 1, 5, 9, 2]
                     ↑ pivot=5

Pass 1:   [2, 3, 1] [5] [7, 9, 8]
               ↑         ↑
          smaller     larger

Pass 2:   [1] [2, 3] [5] [7, 8] [9]

Pass 3:   [1] [2] [3] [5] [7] [8] [9]

Final:    [1, 2, 3, 5, 7, 8, 9] ✅
```

---

[← Back to Sorting Algorithms](../README.md) | [Next: MergeSort →](./mergesort.md)
