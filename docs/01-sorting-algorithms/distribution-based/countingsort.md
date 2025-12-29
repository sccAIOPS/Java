# CountingSort

> **Category:** Non-Comparison Sorting  
> **Paradigm:** Distribution / Counting  
> **In-Place:** No  
> **Stable:** Yes  
> **Input Restriction:** Integers with known range

---

## 📋 Overview

CountingSort is a non-comparison sorting algorithm that works by counting the occurrences of each distinct element. It's extremely efficient when the range of input values (k) is not significantly larger than the number of elements (n). Unlike comparison-based sorts, it achieves **linear time complexity**.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(n + k) |
| **Time (Average)** | O(n + k) |
| **Time (Worst)** | O(n + k) |
| **Space** | O(k) |
| **Stable** | Yes |
| **In-Place** | No |

Where $k$ = range of input values (max - min + 1)

---

## 🔬 Mathematical Analysis

### Algorithm Concept

CountingSort works in three phases:

1. **Histogram:** Count occurrences of each value
2. **Cumulative Sum:** Convert counts to positions
3. **Reconstruct:** Build sorted output using positions

### Time Complexity Derivation

Let $n$ = number of elements, $k$ = range of values

| Step | Operations | Complexity |
|------|------------|------------|
| Find min/max | Scan array | O(n) |
| Compute histogram | Scan array | O(n) |
| Cumulative sum | Scan count array | O(k) |
| Build output | Scan array | O(n) |

**Total:**
$$
T(n, k) = O(n) + O(n) + O(k) + O(n) = O(n + k)
$$

### When is CountingSort Efficient?

Efficient when $k = O(n)$:
$$
T(n) = O(n + n) = O(n)
$$

Inefficient when $k >> n$:
- Example: Sorting 100 numbers in range [1, 1,000,000]
- $T(100) = O(100 + 1,000,000) = O(1,000,000)$

### Space Complexity

$$
S(n, k) = O(k) + O(n) = O(n + k)
$$

- O(k): Count array
- O(n): Output array

---

## 📝 Pseudocode

```
COUNTING-SORT(A)
    n ← length(A)
    min ← MIN(A)
    max ← MAX(A)
    k ← max - min + 1
    
    // Step 1: Create histogram
    count ← array of size k, initialized to 0
    for i ← 0 to n-1 do
        count[A[i] - min] ← count[A[i] - min] + 1
    
    // Step 2: Convert to cumulative counts
    for i ← 1 to k-1 do
        count[i] ← count[i] + count[i-1]
    
    // Step 3: Build sorted output (traverse backwards for stability)
    output ← array of size n
    for i ← n-1 downto 0 do
        output[count[A[i] - min] - 1] ← A[i]
        count[A[i] - min] ← count[A[i] - min] - 1
    
    return output
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/sorts/CountingSort.java](../../src/main/java/com/thealgorithms/sorts/CountingSort.java)

```java
/**
 * A standard implementation of the Counting Sort algorithm for integer arrays.
 * This implementation has a time complexity of O(n + k), where n is the number
 * of elements in the input array and k is the range of the input.
 * It works only with integer arrays.
 *
 * The space complexity is O(k), where k is the range of the input integers.
 *
 * Note: This implementation handles negative integers as it
 * calculates the range based on the minimum and maximum values of the array.
 */
public final class CountingSort {
    private CountingSort() {
    }

    /**
     * Sorts an array of integers using the Counting Sort algorithm.
     *
     * @param array the array to be sorted
     * @return the sorted array
     */
    public static int[] sort(int[] array) {
        if (array.length == 0) {
            return array;
        }
        final var stats = Arrays.stream(array).summaryStatistics();
        final int min = stats.getMin();
        int[] count = computeHistogram(array, min, stats.getMax() - min + 1);
        toCumulative(count);
        return reconstructSorted(count, min, array);
    }

    private static int[] computeHistogram(final int[] array, final int shift, final int spread) {
        int[] res = new int[spread];
        for (final var value : array) {
            res[value - shift]++;
        }
        return res;
    }

    private static void toCumulative(int[] count) {
        for (int i = 1; i < count.length; i++) {
            count[i] += count[i - 1];
        }
    }

    private static int[] reconstructSorted(final int[] cumulativeCount, final int shift, final int[] array) {
        int[] res = new int[array.length];
        for (int i = array.length - 1; i >= 0; i--) {
            res[cumulativeCount[array[i] - shift] - 1] = array[i];
            cumulativeCount[array[i] - shift]--;
        }
        return res;
    }
}
```

### Implementation Features

| Feature | Description |
|---------|-------------|
| **Handles negatives** | Uses min as shift factor |
| **Utility class** | Private constructor |
| **Java Streams** | Uses `summaryStatistics()` for min/max |
| **Stable** | Backwards traversal in reconstruction |

---

## 🎯 Step-by-Step Example

### Input: `[4, 2, 2, 8, 3, 3, 1]`

**Step 1: Find Range**
```
min = 1, max = 8
range k = 8 - 1 + 1 = 8
shift = 1 (subtract from each value for indexing)
```

**Step 2: Compute Histogram**
```
Values:  [4, 2, 2, 8, 3, 3, 1]
Shifted: [3, 1, 1, 7, 2, 2, 0]  (subtract min=1)

count[0] = 1  (value 1 appears once)
count[1] = 2  (value 2 appears twice)
count[2] = 2  (value 3 appears twice)
count[3] = 1  (value 4 appears once)
count[4] = 0
count[5] = 0
count[6] = 0
count[7] = 1  (value 8 appears once)

Histogram: [1, 2, 2, 1, 0, 0, 0, 1]
            1  2  3  4  5  6  7  8  ← values
```

**Step 3: Cumulative Sum**
```
Histogram:  [1, 2, 2, 1, 0, 0, 0, 1]
Cumulative: [1, 3, 5, 6, 6, 6, 6, 7]
             ↑  ↑  ↑  ↑           ↑
             │  │  │  │           └─ positions for 8
             │  │  │  └─ positions for 4
             │  │  └─ positions for 3
             │  └─ positions for 2
             └─ positions for 1
```

**Step 4: Reconstruct (Backwards for Stability)**
```
Input (reversed): [1, 3, 3, 8, 2, 2, 4]
                   ←─────────────────

Process 1: count[0]=1 → output[0]=1, count[0]--
Process 3: count[2]=5 → output[4]=3, count[2]--
Process 3: count[2]=4 → output[3]=3, count[2]--
Process 8: count[7]=7 → output[6]=8, count[7]--
Process 2: count[1]=3 → output[2]=2, count[1]--
Process 2: count[1]=2 → output[1]=2, count[1]--
Process 4: count[3]=6 → output[5]=4, count[3]--

Output: [1, 2, 2, 3, 3, 4, 8]
```

### Final Result: `[1, 2, 2, 3, 3, 4, 8]` ✅

---

## 📊 Complexity Analysis

### Time Complexity

| Operation | Complexity |
|-----------|------------|
| Find min/max | O(n) |
| Compute histogram | O(n) |
| Cumulative sum | O(k) |
| Reconstruct output | O(n) |
| **Total** | **O(n + k)** |

### Space Complexity

| Component | Size |
|-----------|------|
| Count array | O(k) |
| Output array | O(n) |
| **Total** | **O(n + k)** |

### Comparison with Other Sorts

| Algorithm | Time | Space | Stable |
|-----------|------|-------|--------|
| **CountingSort** | O(n + k) | O(n + k) | Yes |
| RadixSort | O(d(n + k)) | O(n + k) | Yes |
| BucketSort | O(n + k) avg | O(n + k) | Yes |
| QuickSort | O(n log n) avg | O(log n) | No |
| MergeSort | O(n log n) | O(n) | Yes |

---

## ⚠️ Common Pitfalls

### 1. Not Handling Negative Numbers

```java
// Wrong: assumes all positive
count[array[i]]++;

// Correct: use offset for negative support
int shift = min;
count[array[i] - shift]++;
```

### 2. Incorrect Range Calculation

```java
// Wrong: off by one
int k = max - min;

// Correct: inclusive range
int k = max - min + 1;
```

### 3. Forward Traversal (Destroys Stability)

```java
// Wrong: forward traversal - NOT stable
for (int i = 0; i < array.length; i++) {
    output[count[array[i]]++] = array[i];
}

// Correct: backward traversal - stable
for (int i = array.length - 1; i >= 0; i--) {
    output[--count[array[i]]] = array[i];
}
```

---

## 🔧 Optimizations

### 1. In-Place Variant (Unstable)

For special cases where stability isn't needed:
```java
public static void inPlaceCountingSort(int[] array) {
    // Only works for specific patterns, generally loses stability
}
```

### 2. Multi-pass for Unknown Range

```java
// First pass: find min/max
int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
for (int val : array) {
    min = Math.min(min, val);
    max = Math.max(max, val);
}
// Then proceed with normal counting sort
```

### 3. Parallel Histogram Computation

```java
// Using parallel streams for large arrays
IntSummaryStatistics stats = Arrays.stream(array)
    .parallel()
    .summaryStatistics();
```

---

## 🌍 Real-World Applications

| Application | Why CountingSort? |
|-------------|-------------------|
| **Grade distribution** | Grades 0-100 (small k) |
| **Age sorting** | Ages 0-150 (small k) |
| **Character frequency** | ASCII/Unicode subset |
| **Color histogram** | Image processing (0-255) |
| **RadixSort subroutine** | Stable sort for digits |

### Industry Usage

- **Database systems:** Sorting by categorical columns
- **Image processing:** Histogram equalization
- **Network routing:** Packet priority sorting
- **Graphics:** Z-buffer sorting

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/sorts/CountingSortTest.java](../../src/test/java/com/thealgorithms/sorts/CountingSortTest.java)

### Test Cases

| Input | Expected | Tests |
|-------|----------|-------|
| `[]` | `[]` | Empty array |
| `[5]` | `[5]` | Single element |
| `[1,1,1]` | `[1,1,1]` | All same |
| `[3,1,4,1,5]` | `[1,1,3,4,5]` | Normal case |
| `[-5,-2,3,1]` | `[-5,-2,1,3]` | Negative numbers |
| `[1000000,1]` | `[1,1000000]` | Large range (O(k) memory!) |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 8.2
- Sedgewick, R. *"Algorithms"*, Section 5.1

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [RadixSort](./radixsort.md) | Uses CountingSort as subroutine |
| [BucketSort](./bucketsort.md) | Similar distribution-based approach |
| [PigeonholeSort](./pigeonholesort.md) | Simpler variant |

---

## 📊 Visualization

```
Input: [4, 2, 2, 8, 3, 3, 1]

Step 1 - Histogram:
         1  2  3  4  5  6  7  8
        [1][2][2][1][ ][ ][ ][1]
         ↑  ↑  ↑  ↑           ↑
        one two two one     one

Step 2 - Cumulative:
        [1][3][5][6][6][6][6][7]
         │  │  │  │           │
         │  │  │  │           └─ 8 goes at position 7
         │  │  │  └─ 4 goes at position 6
         │  │  └─ 3s go at positions 4-5
         │  └─ 2s go at positions 2-3
         └─ 1 goes at position 1

Step 3 - Output:
        [1][2][2][3][3][4][ ][8]
         0  1  2  3  4  5  6  7

Final: [1, 2, 2, 3, 3, 4, 8] ✅
```

---

[← Back to Sorting Algorithms](../README.md) | [Next: RadixSort →](./radixsort.md)
