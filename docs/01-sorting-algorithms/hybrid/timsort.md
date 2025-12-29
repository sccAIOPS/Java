# TimSort

> **Category:** Hybrid Sorting  
> **Paradigm:** Merge Sort + Insertion Sort  
> **In-Place:** No  
> **Stable:** Yes  
> **Adaptive:** Yes

---

## 📋 Overview

TimSort is a hybrid stable sorting algorithm derived from MergeSort and InsertionSort. It was designed by Tim Peters in 2002 for Python's sort implementation. TimSort exploits the fact that real-world data often contains ordered subsequences ("runs").

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(n) |
| **Time (Average)** | O(n log n) |
| **Time (Worst)** | O(n log n) |
| **Space** | O(n) |
| **Stable** | Yes |
| **In-Place** | No |

---

## 🔬 Mathematical Analysis

### Core Concept: Runs

A **run** is a maximal sequence of consecutive elements that are either:
- Non-decreasing: $a_i \leq a_{i+1}$
- Strictly decreasing: $a_i > a_{i+1}$ (reversed during sorting)

### Minimum Run Length

TimSort uses a minimum run length (minrun), typically 32-64, chosen such that:
$$
\text{minrun} \in [32, 64] \text{ and } \frac{n}{\text{minrun}} \text{ is a power of 2 or close to it}
$$

### Time Complexity Analysis

**Best Case (fully sorted):**
$$
T(n) = O(n)
$$
Single scan detects the array is one run.

**Average/Worst Case:**
$$
T(n) = O(n \log n)
$$

For $k$ runs of average length $l$:
$$
T(n) = O(n) + O(k \cdot l \log l) + O(n \log k)
$$

Where:
- O(n): Identify runs
- O(k·l log l): InsertionSort small runs
- O(n log k): Merge k runs

### Space Complexity

$$
S(n) = O(n)
$$

Auxiliary array for merging (can be optimized to $O(n/2)$).

---

## 📝 Pseudocode

```
TIMSORT(A)
    n ← length(A)
    minRun ← COMPUTE-MIN-RUN(n)  // Usually 32
    
    // Step 1: Sort small subarrays using InsertionSort
    for i ← 0 to n step minRun do
        INSERTION-SORT(A, i, min(i + minRun - 1, n - 1))
    
    // Step 2: Merge runs using bottom-up MergeSort
    size ← minRun
    while size < n do
        for left ← 0 to n step 2 * size do
            mid ← left + size - 1
            right ← min(left + 2 * size - 1, n - 1)
            
            if mid < right then
                MERGE(A, left, mid, right)
        
        size ← size * 2
    
    return A

COMPUTE-MIN-RUN(n)
    r ← 0
    while n ≥ 64 do
        r ← r OR (n AND 1)
        n ← n >> 1
    return n + r
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/TimSort.java](../../src/main/java/com/thealgorithms/sorts/TimSort.java)

```java
/**
 * This is simplified TimSort algorithm implementation. 
 * The original one is more complicated.
 * <p>
 * For more details @see <a href="https://en.wikipedia.org/wiki/Timsort">TimSort Algorithm</a>
 */
@SuppressWarnings({"rawtypes", "unchecked"})
class TimSort implements SortAlgorithm {
    private static final int SUB_ARRAY_SIZE = 32;
    private Comparable[] aux;

    @Override
    public <T extends Comparable<T>> T[] sort(T[] array) {
        final int n = array.length;

        // Step 1: Sort small subarrays with InsertionSort
        InsertionSort insertionSort = new InsertionSort();
        for (int i = 0; i < n; i += SUB_ARRAY_SIZE) {
            insertionSort.sort(array, i, Math.min(i + SUB_ARRAY_SIZE, n));
        }

        // Step 2: Bottom-up merge
        aux = new Comparable[n];
        for (int sz = SUB_ARRAY_SIZE; sz < n; sz = sz + sz) {
            for (int lo = 0; lo < n - sz; lo += sz + sz) {
                merge(array, lo, lo + sz - 1, Math.min(lo + sz + sz - 1, n - 1));
            }
        }

        return array;
    }

    private <T extends Comparable<T>> void merge(T[] a, final int lo, final int mid, final int hi) {
        int i = lo;
        int j = mid + 1;
        System.arraycopy(a, lo, aux, lo, hi + 1 - lo);

        for (int k = lo; k <= hi; k++) {
            if (j > hi) {
                a[k] = (T) aux[i++];
            } else if (i > mid) {
                a[k] = (T) aux[j++];
            } else if (less(aux[j], aux[i])) {
                a[k] = (T) aux[j++];
            } else {
                a[k] = (T) aux[i++];
            }
        }
    }
}
```

### Implementation Notes

| Feature | Value | Production TimSort |
|---------|-------|-------------------|
| **SUB_ARRAY_SIZE** | 32 | 32-64 (adaptive) |
| **Run Detection** | Fixed size | Detects natural runs |
| **Galloping Mode** | Not implemented | Yes |
| **Stack Invariant** | Not implemented | Yes |

---

## 🎯 Step-by-Step Example

### Input: `[5, 21, 7, 23, 19, 42, 3, 14, 8, 6]` (n=10)

**SUB_ARRAY_SIZE = 32** (but array is smaller, so we'll use 4 for illustration)

**Phase 1: InsertionSort Subarrays**

```
Subarray 1: [5, 21, 7, 23] → InsertionSort → [5, 7, 21, 23]
Subarray 2: [19, 42, 3, 14] → InsertionSort → [3, 14, 19, 42]
Subarray 3: [8, 6] → InsertionSort → [6, 8]

After Phase 1: [5, 7, 21, 23, 3, 14, 19, 42, 6, 8]
              └────sorted────┘ └────sorted────┘ └sorted┘
```

**Phase 2: Bottom-Up Merge**

```
Merge [5,7,21,23] with [3,14,19,42]:
→ [3, 5, 7, 14, 19, 21, 23, 42]

Result after first merge pass:
[3, 5, 7, 14, 19, 21, 23, 42, 6, 8]
└─────────sorted───────────┘ └─┘

Merge [3,5,7,14,19,21,23,42] with [6,8]:
→ [3, 5, 6, 7, 8, 14, 19, 21, 23, 42]
```

### Final Result: `[3, 5, 6, 7, 8, 14, 19, 21, 23, 42]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Condition |
|------|------------|-----------|
| **Best** | O(n) | Already sorted (single run) |
| **Average** | O(n log n) | Random data |
| **Worst** | O(n log n) | Guaranteed |

### Comparison with Other Sorts

| Algorithm | Best | Average | Worst | Stable | Space |
|-----------|------|---------|-------|--------|-------|
| **TimSort** | O(n) | O(n log n) | O(n log n) | Yes | O(n) |
| QuickSort | O(n log n) | O(n log n) | O(n²) | No | O(log n) |
| MergeSort | O(n log n) | O(n log n) | O(n log n) | Yes | O(n) |
| HeapSort | O(n log n) | O(n log n) | O(n log n) | No | O(1) |

### Why TimSort is Optimal for Real Data

Real-world data often has:
- Partially sorted sequences
- Repeated patterns
- Organized structure

TimSort exploits this with **natural run detection**.

---

## ⚠️ Common Pitfalls

### 1. Not Understanding Runs

```java
// This is NOT a run (must be consecutive)
[1, 3, 2, 4]  // Not a valid run

// These ARE runs
[1, 2, 3, 4]  // Ascending run
[4, 3, 2, 1]  // Descending run (will be reversed)
```

### 2. Memory Allocation

```java
// Avoid allocating inside merge
private <T extends Comparable<T>> void merge(T[] a, ...) {
    // BAD: Creates new array every merge
    Comparable[] aux = new Comparable[hi - lo + 1];
    
    // GOOD: Use pre-allocated auxiliary array
    System.arraycopy(a, lo, this.aux, lo, hi + 1 - lo);
}
```

### 3. Merge Invariant

Production TimSort maintains stack invariants for optimal merging:
```
// Invariants for runs on stack (lengths)
run[i-2] > run[i-1] + run[i]
run[i-1] > run[i]
```

---

## 🔧 Optimizations

### 1. Natural Run Detection (Not in this implementation)

```java
int identifyRun(T[] array, int start) {
    if (start >= array.length - 1) return 1;
    
    int runLength = 2;
    if (less(array[start + 1], array[start])) {
        // Descending run - find end and reverse
        while (runLength < array.length - start && 
               less(array[start + runLength], array[start + runLength - 1])) {
            runLength++;
        }
        reverseRange(array, start, start + runLength - 1);
    } else {
        // Ascending run
        while (runLength < array.length - start && 
               !less(array[start + runLength], array[start + runLength - 1])) {
            runLength++;
        }
    }
    return runLength;
}
```

### 2. Galloping Mode

When one run consistently "wins" during merge, switch to galloping (binary search):
```java
// Instead of linear comparison, binary search for position
int gallopLeft(T key, T[] array, int base, int len) {
    // Use exponential search then binary search
}
```

### 3. Minimum Run Length Calculation

```java
static int minRunLength(int n) {
    int r = 0;
    while (n >= 64) {
        r |= (n & 1);
        n >>= 1;
    }
    return n + r;
}
```

---

## 🌍 Real-World Applications

| Application | Why TimSort? |
|-------------|--------------|
| **Python's `list.sort()`** | Default since Python 2.3 |
| **Java's `Arrays.sort()` (objects)** | Default for objects since Java 7 |
| **Android SDK** | Default sort |
| **Swift** | Standard library sort |
| **V8 JavaScript Engine** | Used in `Array.prototype.sort()` |

### Industry Adoption

```
Python:     sorted(), list.sort()
Java:       Arrays.sort(Object[]), Collections.sort()
Android:    java.util.Arrays
Rust:       slice::sort() for stable sort
Swift:      Array.sorted()
```

### Why Industry Chose TimSort

1. **Stable:** Required for many applications
2. **Adaptive:** O(n) for sorted data
3. **Predictable:** O(n log n) worst case
4. **Real-world optimized:** Exploits patterns in real data

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/TimSortTest.java](../../src/test/java/com/thealgorithms/sorts/TimSortTest.java)

### Test Cases

| Input | Expected | Tests |
|-------|----------|-------|
| `[]` | `[]` | Empty array |
| `[1]` | `[1]` | Single element |
| `[1,2,3,...,1000]` | Same | Already sorted (O(n)) |
| `[1000,...,2,1]` | `[1,2,...,1000]` | Reverse sorted |
| `[3,1,3,1,3]` | `[1,1,3,3,3]` | Duplicates (stability) |
| Random 10000 | Sorted | Large random array |

---

## 📖 References

### Original Paper

- Peters, Tim. *"listsort.txt"* - Original TimSort description in Python source

### Wikipedia

- [TimSort - Wikipedia](https://en.wikipedia.org/wiki/Timsort)

### Bug Discovery

- de Gouw, S., et al. *"Proving that Android's, Java's and Python's Sort is Broken (and Showing How to Fix It)"* - 2015

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [MergeSort](./mergesort.md) | TimSort uses merge operation |
| [InsertionSort](./insertionsort.md) | TimSort uses for small runs |
| [AdaptiveMergeSort](./adaptive-merge-sort.md) | Similar adaptive approach |

---

## 📊 Visualization

```
Original:  [5, 21, 7, 23 | 19, 42, 3, 14 | 8, 6]
            run 1        run 2           run 3

Phase 1 - InsertionSort each run:
           [5, 7, 21, 23 | 3, 14, 19, 42 | 6, 8]
            ─────────────  ─────────────   ────
               sorted         sorted      sorted

Phase 2 - Bottom-up merge:
           [3, 5, 7, 14, 19, 21, 23, 42 | 6, 8]
            ───────────────────────────   ────
                    merged                 run

Final merge:
           [3, 5, 6, 7, 8, 14, 19, 21, 23, 42]
            ─────────────────────────────────
                       fully sorted ✅
```

---

## 🔍 TimSort vs Production Implementation

| Feature | This Implementation | Java/Python TimSort |
|---------|---------------------|---------------------|
| Run Detection | Fixed 32 elements | Natural runs |
| Minimum Run | 32 (constant) | Dynamic (32-64) |
| Galloping | ❌ Not implemented | ✅ Yes |
| Stack Invariant | ❌ Not implemented | ✅ Yes |
| Memory | O(n) | O(n/2) optimized |

---

[← Back to Sorting Algorithms](../README.md) | [Next: CountingSort →](../distribution-based/countingsort.md)
