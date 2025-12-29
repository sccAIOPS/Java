# BubbleSort

> **Category:** Comparison-Based Sorting  
> **Paradigm:** Exchange Sort  
> **In-Place:** Yes  
> **Stable:** Yes  
> **Adaptive:** Yes (with optimization)

---

## 📋 Overview

BubbleSort is the simplest sorting algorithm. It repeatedly steps through the list, compares adjacent elements, and swaps them if they're in the wrong order. The algorithm gets its name because smaller elements "bubble" to the top of the list.

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

### Algorithm Mechanics

After pass $i$, the $i$ largest elements are in their final positions.

**Invariant:** After pass $k$, the last $k$ elements are sorted.

### Comparison Count

**Worst/Average Case:**
$$
C(n) = \sum_{i=1}^{n-1}(n-i) = \sum_{i=1}^{n-1}i = \frac{n(n-1)}{2} = O(n^2)
$$

**Best Case (already sorted, with early termination):**
$$
C(n) = n - 1 = O(n)
$$

### Swap Count

**Worst Case (reverse sorted):**
$$
S(n) = \frac{n(n-1)}{2} = O(n^2)
$$

**Best Case (already sorted):**
$$
S(n) = 0
$$

### Inversion Analysis

BubbleSort removes one **inversion** per swap. An inversion is a pair $(i, j)$ where $i < j$ but $A[i] > A[j]$.

Maximum inversions (reverse sorted): $\frac{n(n-1)}{2}$

---

## 📝 Pseudocode

```
BUBBLE-SORT(A)
    n ← length(A)
    for i ← 1 to n-1 do
        swapped ← false
        for j ← 0 to n-i-1 do
            if A[j] > A[j+1] then
                SWAP(A[j], A[j+1])
                swapped ← true
        
        // Optimization: early termination if no swaps
        if not swapped then
            break
    return A
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/BubbleSort.java](../../src/main/java/com/thealgorithms/sorts/BubbleSort.java)

```java
class BubbleSort implements SortAlgorithm {

    @Override
    public <T extends Comparable<T>> T[] sort(T[] array) {
        for (int i = 1, size = array.length; i < size; ++i) {
            boolean swapped = false;
            for (int j = 0; j < size - i; ++j) {
                if (SortUtils.greater(array[j], array[j + 1])) {
                    SortUtils.swap(array, j, j + 1);
                    swapped = true;
                }
            }
            if (!swapped) {
                break;  // Early termination - array is sorted
            }
        }
        return array;
    }
}
```

### Key Features

- **Early Termination:** If no swaps occur in a pass, array is sorted
- **Adaptive:** O(n) for nearly sorted arrays
- **Stable:** Equal elements maintain relative order

---

## 🎯 Step-by-Step Example

### Input: `[5, 3, 8, 4, 2]`

**Pass 1:**
```
[5, 3, 8, 4, 2] → [3, 5, 8, 4, 2]  (swap 5, 3)
[3, 5, 8, 4, 2] → [3, 5, 8, 4, 2]  (5 < 8, no swap)
[3, 5, 8, 4, 2] → [3, 5, 4, 8, 2]  (swap 8, 4)
[3, 5, 4, 8, 2] → [3, 5, 4, 2, 8]  (swap 8, 2)
                          ↑ 8 is in place
```

**Pass 2:**
```
[3, 5, 4, 2, 8] → [3, 5, 4, 2, 8]  (3 < 5, no swap)
[3, 5, 4, 2, 8] → [3, 4, 5, 2, 8]  (swap 5, 4)
[3, 4, 5, 2, 8] → [3, 4, 2, 5, 8]  (swap 5, 2)
                       ↑ 5 is in place
```

**Pass 3:**
```
[3, 4, 2, 5, 8] → [3, 4, 2, 5, 8]  (3 < 4, no swap)
[3, 4, 2, 5, 8] → [3, 2, 4, 5, 8]  (swap 4, 2)
                    ↑ 4 is in place
```

**Pass 4:**
```
[3, 2, 4, 5, 8] → [2, 3, 4, 5, 8]  (swap 3, 2)
                 ↑ 3 is in place
```

### Final Result: `[2, 3, 4, 5, 8]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Condition |
|------|------------|-----------|
| **Best** | O(n) | Already sorted (with early termination) |
| **Average** | O(n²) | Random order |
| **Worst** | O(n²) | Reverse sorted |

### Space Complexity

| Type | Complexity |
|------|------------|
| **Auxiliary** | O(1) |

### Why Stable?

When `array[j] == array[j+1]`, no swap occurs:
```java
if (SortUtils.greater(array[j], array[j + 1])) {  // Only swap if strictly greater
    SortUtils.swap(array, j, j + 1);
}
```

---

## ⚠️ Common Pitfalls

### 1. Missing Early Termination

**Problem:** O(n²) even for sorted arrays
```java
// BAD: Always O(n²)
for (int i = 0; i < n - 1; i++) {
    for (int j = 0; j < n - i - 1; j++) {
        if (arr[j] > arr[j + 1]) swap(arr, j, j + 1);
    }
}

// GOOD: O(n) for sorted arrays
boolean swapped;
do {
    swapped = false;
    // ...
} while (swapped);
```

### 2. Wrong Loop Bounds

**Problem:** Index out of bounds
```java
// Wrong: j goes to n-1, then j+1 is out of bounds
for (int j = 0; j < size; ++j)

// Correct: j goes to size-i-1
for (int j = 0; j < size - i; ++j)
```

---

## 🔧 Optimizations

### 1. Early Termination (Implemented)

Already present in this implementation with `swapped` flag.

### 2. Remember Last Swap Position

```java
public <T extends Comparable<T>> T[] optimizedSort(T[] array) {
    int n = array.length;
    int lastSwapIndex;
    
    do {
        lastSwapIndex = 0;
        for (int j = 0; j < n - 1; j++) {
            if (SortUtils.greater(array[j], array[j + 1])) {
                SortUtils.swap(array, j, j + 1);
                lastSwapIndex = j + 1;
            }
        }
        n = lastSwapIndex;  // Next pass only needs to go this far
    } while (lastSwapIndex > 0);
    
    return array;
}
```

### 3. Cocktail Shaker Sort

Bidirectional variant that sorts both ends simultaneously.

---

## 🌍 Real-World Applications

| Application | Why BubbleSort? |
|-------------|-----------------|
| **Education** | Simple to understand and implement |
| **Small datasets** | Overhead is minimal |
| **Nearly sorted data** | O(n) with early termination |
| **Detecting sorted arrays** | Single pass detection |

### When NOT to Use

- Large datasets (use QuickSort, MergeSort)
- Performance-critical applications
- Datasets with random order

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/BubbleSortTest.java](../../src/test/java/com/thealgorithms/sorts/BubbleSortTest.java)

### Test Cases

| Input | Expected | Tests |
|-------|----------|-------|
| `[]` | `[]` | Empty array |
| `[1]` | `[1]` | Single element |
| `[1,2,3]` | `[1,2,3]` | Already sorted (O(n)) |
| `[3,2,1]` | `[1,2,3]` | Reverse (worst case) |
| `[3,1,3,2]` | `[1,2,3,3]` | Duplicates (stability) |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS)
- Knuth, D.E. *"The Art of Computer Programming"*, Vol. 3

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [CocktailShakerSort](./cocktail-shaker-sort.md) | Bidirectional BubbleSort |
| [InsertionSort](./insertionsort.md) | Similar O(n²), often faster |
| [SelectionSort](./selectionsort.md) | Another simple O(n²) sort |

---

## 📊 Visualization

```
Pass 1: [5, 3, 8, 4, 2] → [3, 5, 4, 2, | 8]
        Largest bubbles to end ───────────┘

Pass 2: [3, 5, 4, 2, | 8] → [3, 4, 2, | 5, 8]
        Second largest ───────────────┘

Pass 3: [3, 4, 2, | 5, 8] → [3, 2, | 4, 5, 8]

Pass 4: [3, 2, | 4, 5, 8] → [2, | 3, 4, 5, 8]

Final:  [2, 3, 4, 5, 8] ✅
```

---

[← Back to Sorting Algorithms](../README.md) | [Next: InsertionSort →](./insertionsort.md)
