# SelectionSort

> **Category:** Comparison-Based Sorting  
> **Paradigm:** Selection  
> **In-Place:** Yes  
> **Stable:** No  
> **Adaptive:** No

---

## 📋 Overview

SelectionSort divides the input into a sorted and unsorted region. It repeatedly finds the minimum element from the unsorted region and places it at the end of the sorted region. Unlike InsertionSort, it makes O(n) swaps regardless of input order.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(n²) |
| **Time (Average)** | O(n²) |
| **Time (Worst)** | O(n²) |
| **Space** | O(1) |
| **Stable** | No |
| **In-Place** | Yes |

---

## 🔬 Mathematical Analysis

### Algorithm Mechanics

**Invariant:** After iteration $i$, the smallest $i$ elements are in sorted order at positions $0$ to $i-1$.

### Comparison Count

For all cases:
$$
C(n) = \sum_{i=0}^{n-2}(n-1-i) = \sum_{j=1}^{n-1}j = \frac{n(n-1)}{2} = O(n^2)
$$

**Note:** Comparisons are always O(n²), regardless of input order!

### Swap Count

$$
S(n) = n - 1 = O(n)
$$

This is the **minimum number of swaps** among simple sorting algorithms.

### Why Not Stable?

Consider: `[4a, 4b, 2]`
- Find min = 2 at index 2
- Swap with index 0: `[2, 4b, 4a]`
- `4a` and `4b` changed relative order!

---

## 📝 Pseudocode

```
SELECTION-SORT(A)
    n ← length(A)
    for i ← 0 to n-2 do
        minIndex ← FIND-MIN-INDEX(A, i, n-1)
        if minIndex ≠ i then
            SWAP(A[i], A[minIndex])
    return A

FIND-MIN-INDEX(A, start, end)
    minIndex ← start
    for j ← start + 1 to end do
        if A[j] < A[minIndex] then
            minIndex ← j
    return minIndex
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/SelectionSort.java](../../src/main/java/com/thealgorithms/sorts/SelectionSort.java)

```java
public class SelectionSort implements SortAlgorithm {
    /**
     * Generic Selection Sort algorithm.
     *
     * Time Complexity:
     * - Best case: O(n^2)
     * - Average case: O(n^2)
     * - Worst case: O(n^2)
     *
     * Space Complexity: O(1) – in-place sorting.
     *
     * @see SortAlgorithm
     */
    @Override
    public <T extends Comparable<T>> T[] sort(T[] array) {

        for (int i = 0; i < array.length - 1; i++) {
            final int minIndex = findIndexOfMin(array, i);
            SortUtils.swap(array, i, minIndex);
        }
        return array;
    }

    private static <T extends Comparable<T>> int findIndexOfMin(T[] array, final int startIndex) {
        int minIndex = startIndex;
        for (int i = startIndex + 1; i < array.length; i++) {
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
| **findIndexOfMin** | Helper method finds minimum in unsorted region |
| **Modular Design** | Separation of concerns |

---

## 🎯 Step-by-Step Example

### Input: `[64, 25, 12, 22, 11]`

**Pass 1 (i=0):**
```
Find min in [64, 25, 12, 22, 11] → min = 11 at index 4
Swap index 0 and 4:
[11, 25, 12, 22, 64]
 ↑ sorted
```

**Pass 2 (i=1):**
```
Find min in [25, 12, 22, 64] → min = 12 at index 2
Swap index 1 and 2:
[11, 12, 25, 22, 64]
 ↑──↑ sorted
```

**Pass 3 (i=2):**
```
Find min in [25, 22, 64] → min = 22 at index 3
Swap index 2 and 3:
[11, 12, 22, 25, 64]
 ↑──↑──↑ sorted
```

**Pass 4 (i=3):**
```
Find min in [25, 64] → min = 25 at index 3
No swap needed (already in position):
[11, 12, 22, 25, 64]
 ↑──↑──↑──↑ sorted
```

### Final Result: `[11, 12, 22, 25, 64]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Why |
|------|------------|-----|
| **Best** | O(n²) | Always scans unsorted region |
| **Average** | O(n²) | Same |
| **Worst** | O(n²) | Same |

**Note:** Unlike InsertionSort, SelectionSort is NOT adaptive.

### Space Complexity

| Type | Complexity |
|------|------------|
| **Auxiliary** | O(1) |

### Comparison: Selection vs Insertion

| Aspect | SelectionSort | InsertionSort |
|--------|---------------|---------------|
| **Comparisons** | Always O(n²) | O(n) to O(n²) |
| **Swaps** | O(n) | O(n²) |
| **Adaptive** | No | Yes |
| **Best for** | Minimizing writes | Nearly sorted data |

---

## ⚠️ Common Pitfalls

### 1. Unnecessary Swap

```java
// Can avoid swap when minIndex == i
final int minIndex = findIndexOfMin(array, i);
if (minIndex != i) {  // Add this check
    SortUtils.swap(array, i, minIndex);
}
```

### 2. Wrong Loop Bound

```java
// Wrong: i goes to n-1, but nothing to compare with
for (int i = 0; i < array.length; i++)

// Correct: i goes to n-2
for (int i = 0; i < array.length - 1; i++)
```

### 3. Instability Misconception

Some try to make it "stable" by shifting instead of swapping—this changes it to InsertionSort!

---

## 🔧 Optimizations

### 1. Bidirectional Selection Sort (Cocktail Selection)

Find both min and max in each pass:

```java
public <T extends Comparable<T>> T[] bidirectionalSort(T[] array) {
    int left = 0, right = array.length - 1;
    
    while (left < right) {
        int minIdx = left, maxIdx = right;
        
        for (int i = left; i <= right; i++) {
            if (less(array[i], array[minIdx])) minIdx = i;
            if (greater(array[i], array[maxIdx])) maxIdx = i;
        }
        
        swap(array, left, minIdx);
        if (maxIdx == left) maxIdx = minIdx;  // Handle edge case
        swap(array, right, maxIdx);
        
        left++;
        right--;
    }
    return array;
}
```

### 2. Heap Selection (HeapSort)

Use a heap to find minimum in O(log n) instead of O(n).

---

## 🌍 Real-World Applications

| Application | Why SelectionSort? |
|-------------|---------------------|
| **Memory writes expensive** | Minimizes swaps (flash memory, EEPROM) |
| **Small datasets** | Simple, predictable performance |
| **Checking sortedness** | Can be adapted to detect sorted regions |

### When to Prefer SelectionSort

- Write operations are expensive (flash memory)
- Need exactly O(n) swaps
- Array is very small (< 10 elements)

### When NOT to Use

- Large datasets (use QuickSort, MergeSort)
- Nearly sorted data (use InsertionSort)
- Need stable sort

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/SelectionSortTest.java](../../src/test/java/com/thealgorithms/sorts/SelectionSortTest.java)

### Test Cases

| Input | Expected | Tests |
|-------|----------|-------|
| `[]` | `[]` | Empty array |
| `[1]` | `[1]` | Single element |
| `[1,2,3]` | `[1,2,3]` | Already sorted |
| `[3,2,1]` | `[1,2,3]` | Reverse sorted |
| `[4a,4b,2]` | `[2,4b,4a]` | Instability (order changed!) |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS)
- Sedgewick, R. *"Algorithms"*, Section 2.1

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [HeapSort](./heapsort.md) | O(n log n) selection using heap |
| [InsertionSort](./insertionsort.md) | Similar simplicity, but adaptive |
| [CycleSort](./cyclesort.md) | Optimal for minimizing writes |

---

## 📊 Visualization

```
Initial: [64, 25, 12, 22, 11]
          unsorted ──────────

Pass 1:  [11,│25, 12, 22, 64]   min=11, swap with pos 0
Pass 2:  [11, 12,│25, 22, 64]   min=12, swap with pos 1
Pass 3:  [11, 12, 22,│25, 64]   min=22, swap with pos 2
Pass 4:  [11, 12, 22, 25,│64]   min=25, no swap needed

Final:   [11, 12, 22, 25, 64] ✅
          sorted ─────────────
```

---

[← Back to Sorting Algorithms](../README.md) | [Next: ShellSort →](./shellsort.md)
