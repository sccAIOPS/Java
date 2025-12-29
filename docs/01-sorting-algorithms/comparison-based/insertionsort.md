# InsertionSort

> **Category:** Comparison-Based Sorting  
> **Paradigm:** Incremental  
> **In-Place:** Yes  
> **Stable:** Yes  
> **Adaptive:** Yes

---

## 📋 Overview

InsertionSort builds the sorted array one element at a time by repeatedly picking the next element and inserting it into its correct position among the previously sorted elements. It's similar to how most people sort playing cards in their hands.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(n) |
| **Time (Average)** | O(n²) |
| **Time (Worst)** | O(n²) |
| **Space** | O(1) |
| **Stable** | Yes |
| **In-Place** | Yes |

---

## 🔬 Mathematical Analysis

### Algorithm Intuition

Maintain a sorted subarray $A[0..i-1]$ and insert $A[i]$ into its correct position.

**Loop Invariant:** At the start of iteration $i$, subarray $A[0..i-1]$ is sorted.

### Comparison Count

**Best Case (already sorted):**
$$
C(n) = n - 1 = O(n)
$$

Each element is compared once with its predecessor.

**Worst Case (reverse sorted):**
$$
C(n) = \sum_{i=1}^{n-1} i = \frac{n(n-1)}{2} = O(n^2)
$$

**Average Case:**
$$
C(n) = \frac{1}{4}n(n-1) = O(n^2)
$$

On average, each element travels half the distance.

### Shift Count

Instead of swaps, InsertionSort uses shifts (moves):

| Case | Shifts |
|------|--------|
| Best | 0 |
| Average | $\frac{n(n-1)}{4}$ |
| Worst | $\frac{n(n-1)}{2}$ |

---

## 📝 Pseudocode

```
INSERTION-SORT(A)
    for i ← 1 to length(A) - 1 do
        key ← A[i]
        j ← i - 1
        
        // Shift elements greater than key to the right
        while j ≥ 0 and A[j] > key do
            A[j + 1] ← A[j]
            j ← j - 1
        
        // Insert key at correct position
        A[j + 1] ← key
    
    return A
```

### Sentinel Sort Variant

```
SENTINEL-INSERTION-SORT(A)
    // Move minimum to position 0 (acts as sentinel)
    minIndex ← FIND-MIN-INDEX(A)
    SWAP(A[0], A[minIndex])
    
    // Now we can skip j ≥ 0 check
    for i ← 2 to length(A) - 1 do
        key ← A[i]
        j ← i - 1
        while A[j] > key do      // No j ≥ 0 check needed!
            A[j + 1] ← A[j]
            j ← j - 1
        A[j + 1] ← key
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/InsertionSort.java](../../src/main/java/com/thealgorithms/sorts/InsertionSort.java)

```java
class InsertionSort implements SortAlgorithm {

    @Override
    public <T extends Comparable<T>> T[] sort(T[] array) {
        return sort(array, 0, array.length);
    }

    public <T extends Comparable<T>> T[] sort(T[] array, final int lo, final int hi) {
        if (array == null || lo >= hi) {
            return array;
        }

        for (int i = lo + 1; i < hi; i++) {
            final T key = array[i];
            int j = i - 1;
            while (j >= lo && SortUtils.less(key, array[j])) {
                array[j + 1] = array[j];
                j--;
            }
            array[j + 1] = key;
        }

        return array;
    }

    /**
     * Sentinel sort optimization - places minimum at index 0
     * to avoid boundary checks in inner loop.
     */
    public <T extends Comparable<T>> T[] sentinelSort(T[] array) {
        if (array == null || array.length <= 1) {
            return array;
        }

        final int minElemIndex = findMinIndex(array);
        SortUtils.swap(array, 0, minElemIndex);

        for (int i = 2; i < array.length; i++) {
            final T currentValue = array[i];
            int j = i;
            while (SortUtils.less(currentValue, array[j - 1])) {
                array[j] = array[j - 1];
                j--;
            }
            array[j] = currentValue;
        }

        return array;
    }

    private <T extends Comparable<T>> int findMinIndex(final T[] array) {
        int minIndex = 0;
        for (int i = 1; i < array.length; i++) {
            if (SortUtils.less(array[i], array[minIndex])) {
                minIndex = i;
            }
        }
        return minIndex;
    }
}
```

### Implementation Features

| Feature | Description |
|---------|-------------|
| **Standard sort** | Basic insertion sort |
| **Range sort** | Sort subarray `[lo, hi)` |
| **Sentinel sort** | Optimized with minimum at front |

---

## 🎯 Step-by-Step Example

### Input: `[5, 2, 4, 6, 1, 3]`

**Initial:** Sorted portion is `[5]`

**Pass 1 (i=1, key=2):**
```
Sorted: [5] | Unsorted: [2, 4, 6, 1, 3]
                         ↑ key

[5, 5, 4, 6, 1, 3]  shift 5 right
[2, 5, 4, 6, 1, 3]  insert 2
  ↑ inserted
```

**Pass 2 (i=2, key=4):**
```
Sorted: [2, 5] | Unsorted: [4, 6, 1, 3]
                            ↑ key

[2, 5, 5, 6, 1, 3]  shift 5 right
[2, 4, 5, 6, 1, 3]  insert 4
    ↑ inserted
```

**Pass 3 (i=3, key=6):**
```
Sorted: [2, 4, 5] | Unsorted: [6, 1, 3]
                               ↑ key

[2, 4, 5, 6, 1, 3]  no shift (6 > 5), already in place
```

**Pass 4 (i=4, key=1):**
```
Sorted: [2, 4, 5, 6] | Unsorted: [1, 3]
                                  ↑ key

[2, 4, 5, 6, 6, 3]  shift 6
[2, 4, 5, 5, 6, 3]  shift 5
[2, 4, 4, 5, 6, 3]  shift 4
[2, 2, 4, 5, 6, 3]  shift 2
[1, 2, 4, 5, 6, 3]  insert 1
 ↑ inserted
```

**Pass 5 (i=5, key=3):**
```
Sorted: [1, 2, 4, 5, 6] | Unsorted: [3]
                                     ↑ key

[1, 2, 4, 5, 6, 6]  shift 6
[1, 2, 4, 5, 5, 6]  shift 5
[1, 2, 4, 4, 5, 6]  shift 4
[1, 2, 3, 4, 5, 6]  insert 3
       ↑ inserted
```

### Final Result: `[1, 2, 3, 4, 5, 6]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When |
|------|------------|------|
| **Best** | O(n) | Already sorted |
| **Average** | O(n²) | Random order |
| **Worst** | O(n²) | Reverse sorted |

### Adaptive Behavior

InsertionSort is **adaptive**: performs better on nearly sorted data.

For an array with $k$ inversions:
$$
T(n) = O(n + k)
$$

If $k = O(n)$, then $T(n) = O(n)$!

### Comparison with BubbleSort

| Metric | InsertionSort | BubbleSort |
|--------|---------------|------------|
| Best Case | O(n) | O(n) |
| Swaps (worst) | O(n²) | O(n²) |
| Moves | Shifts | Swaps |
| Practical | **Faster** | Slower |

InsertionSort moves elements instead of swapping, which is more efficient.

---

## ⚠️ Common Pitfalls

### 1. Off-by-One in Loop Bounds

```java
// Wrong: starts at 0 (nothing to compare)
for (int i = 0; i < array.length; i++)

// Correct: starts at 1
for (int i = 1; i < array.length; i++)
```

### 2. Incorrect Shift Direction

```java
// Wrong: shifts left (overwrites data)
array[j] = array[j + 1];

// Correct: shifts right
array[j + 1] = array[j];
```

### 3. Forgetting to Insert Key

```java
// Wrong: key never placed
while (j >= 0 && less(key, array[j])) {
    array[j + 1] = array[j];
    j--;
}
// Missing: array[j + 1] = key;
```

---

## 🔧 Optimizations

### 1. Sentinel Sort (Implemented)

Avoids boundary check by placing minimum at index 0:
```java
while (SortUtils.less(currentValue, array[j - 1])) {  // No j > 0 check!
    array[j] = array[j - 1];
    j--;
}
```

### 2. Binary Insertion Sort

Use binary search to find insertion position:
```java
int insertPos = binarySearch(array, 0, i, key);
// Still O(n²) due to shifts, but fewer comparisons: O(n log n)
```

### 3. Gap Insertion Sort (ShellSort)

Sort elements with a gap, gradually reducing to 1:
```java
for (int gap = n/2; gap > 0; gap /= 2) {
    // Insertion sort with gap
}
```

---

## 🌍 Real-World Applications

| Application | Why InsertionSort? |
|-------------|---------------------|
| **Small arrays** | Less overhead than complex sorts |
| **Nearly sorted data** | O(n) performance |
| **Online sorting** | Can sort as data arrives |
| **Hybrid sorts** | Used in TimSort, IntroSort for small subarrays |
| **Card games** | How humans naturally sort cards |

### Industry Usage

- **Java's Arrays.sort():** Uses InsertionSort for arrays < 47 elements
- **Python's TimSort:** Uses InsertionSort for small runs
- **V8 JavaScript Engine:** Uses InsertionSort for small arrays

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/InsertionSortTest.java](../../src/test/java/com/thealgorithms/sorts/InsertionSortTest.java)

### Test Cases

| Input | Expected | Tests |
|-------|----------|-------|
| `[]` | `[]` | Empty array |
| `[5]` | `[5]` | Single element |
| `[1,2,3,4,5]` | `[1,2,3,4,5]` | Already sorted (O(n)) |
| `[5,4,3,2,1]` | `[1,2,3,4,5]` | Reverse sorted (worst) |
| `[2,1,2,1,2]` | `[1,1,2,2,2]` | Duplicates |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Section 2.1
- Sedgewick, R. *"Algorithms"*, Section 2.1

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [ShellSort](./shellsort.md) | Gap-based InsertionSort |
| [BinaryInsertionSort](./binary-insertion-sort.md) | Binary search for position |
| [TimSort](./timsort.md) | Uses InsertionSort for small runs |

---

## 📊 Visualization

```
Initial: [5, 2, 4, 6, 1, 3]
         sorted | unsorted
         ─┬─    └──────────
          │
Pass 1:  [2, 5,│4, 6, 1, 3]   insert 2 before 5
Pass 2:  [2, 4, 5,│6, 1, 3]   insert 4 between 2,5
Pass 3:  [2, 4, 5, 6,│1, 3]   6 stays in place
Pass 4:  [1, 2, 4, 5, 6,│3]   insert 1 at front
Pass 5:  [1, 2, 3, 4, 5, 6│]  insert 3 between 2,4

Final:   [1, 2, 3, 4, 5, 6] ✅
```

---

[← Back to Sorting Algorithms](../README.md) | [Next: SelectionSort →](./selectionsort.md)
