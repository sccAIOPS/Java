# InterpolationSearch

> **Category:** Interval Search  
> **Paradigm:** Probe Position Estimation  
> **Prerequisite:** Sorted Array with Uniform Distribution  
> **In-Place:** Yes

---

## 📋 Overview

Interpolation Search is an improved variant of Binary Search for **uniformly distributed sorted arrays**. Instead of always checking the middle element, it estimates the position of the target based on the values at the boundaries, similar to how humans search through a phone book.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(1) |
| **Time (Average)** | O(log log n) |
| **Time (Worst)** | O(n) |
| **Space** | O(1) |
| **Prerequisite** | Sorted, uniformly distributed |

---

## 🔬 Mathematical Analysis

### Probe Position Formula

Instead of always probing at the middle:
$$
\text{mid} = \frac{\text{left} + \text{right}}{2}
$$

Interpolation Search probes at:
$$
\text{pos} = \text{left} + \frac{(\text{key} - A[\text{left}])}{(A[\text{right}] - A[\text{left}])} \times (\text{right} - \text{left})
$$

### Intuition

If searching for `key` in `[A[left], A[right]]`:
- The probe position is **proportional** to where `key` should be
- Like finding "Smith" in a phone book—you open near the end, not the middle

### Time Complexity Derivation

**For uniformly distributed data:**

Each probe splits the array by a factor proportional to the value distribution.

After one probe, expected remaining elements:
$$
n^{\frac{1}{2}}
$$

After $k$ probes:
$$
n^{\left(\frac{1}{2}\right)^k} = 1 \Rightarrow k = \log_2(\log_2 n)
$$

Therefore: $T(n) = O(\log \log n)$

**For non-uniform data:**

Worst case degrades to O(n) when distribution is highly skewed.

---

## 📝 Pseudocode

```
INTERPOLATION-SEARCH(A, key)
    left ← 0
    right ← length(A) - 1
    
    while left ≤ right AND key ≥ A[left] AND key ≤ A[right] do
        // Avoid division by zero
        if A[right] = A[left] then
            if A[left] = key then return left
            else return -1
        
        // Estimate position using linear interpolation
        pos ← left + ((key - A[left]) / (A[right] - A[left])) × (right - left)
        
        if A[pos] = key then
            return pos
        else if A[pos] < key then
            left ← pos + 1
        else
            right ← pos - 1
    
    return -1
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/searches/InterpolationSearch.java](../../src/main/java/com/thealgorithms/searches/InterpolationSearch.java)

```java
/**
 * InterpolationSearch is an algorithm that searches for a target value within a sorted array
 * by estimating the position based on the values at the corners of the current search range.
 *
 * The performance of this algorithm can vary:
 * - Worst-case performance: O(n)
 * - Best-case performance: O(1)
 * - Average performance: O(log(log(n))) if elements are uniformly distributed; otherwise O(n)
 * - Worst-case space complexity: O(1)
 *
 * This search algorithm requires the input array to be sorted.
 *
 * @author Podshivalov Nikita
 */
class InterpolationSearch {

    /**
     * Finds the index of the specified key in a sorted array using interpolation search.
     *
     * @param array The sorted array to search.
     * @param key The value to search for.
     * @return The index of the key if found, otherwise -1.
     */
    public int find(int[] array, int key) {
        // Find indexes of two corners
        int start = 0;
        int end = (array.length - 1);

        // Since array is sorted, an element present
        // in array must be in range defined by corner
        while (start <= end && key >= array[start] && key <= array[end]) {
            // Probing the position with keeping
            // uniform distribution in mind.
            int pos = start + (((end - start) / (array[end] - array[start])) * (key - array[start]));

            // Condition of target found
            if (array[pos] == key) {
                return pos;
            }

            // If key is larger, key is in upper part
            if (array[pos] < key) {
                start = pos + 1;
            } // If key is smaller, x is in lower part
            else {
                end = pos - 1;
            }
        }
        return -1;
    }
}
```

### Implementation Notes

| Feature | Description |
|---------|-------------|
| **Integer arrays** | Works with `int[]` (not generic) |
| **Boundary check** | `key >= array[start] && key <= array[end]` |
| **Position formula** | Linear interpolation |

### ⚠️ Known Issue in Implementation

The current implementation has integer division truncation:
```java
// Current (may lose precision)
int pos = start + (((end - start) / (array[end] - array[start])) * (key - array[start]));

// Better (preserves precision)
int pos = start + (int)(((double)(key - array[start]) / (array[end] - array[start])) * (end - start));
```

---

## 🎯 Step-by-Step Example

### Input: Array `[10, 20, 30, 40, 50, 60, 70, 80, 90, 100]`, Target: `70`

**Uniform distribution:** elements are evenly spaced by 10.

**Initial State:**
```
Array: [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
Index:   0   1   2   3   4   5   6   7   8    9

start = 0, end = 9
A[start] = 10, A[end] = 100
```

**Iteration 1:**
```
pos = 0 + ((70 - 10) / (100 - 10)) × (9 - 0)
    = 0 + (60 / 90) × 9
    = 0 + 0.667 × 9
    = 0 + 6
    = 6

A[6] = 70 = key → Found! ✅
```

**Result:** Found at index `6` in just **1 comparison**!

### Compare with Binary Search

Binary Search would need:
1. mid = 4, A[4] = 50 < 70 → go right
2. mid = 7, A[7] = 80 > 70 → go left
3. mid = 5, A[5] = 60 < 70 → go right
4. mid = 6, A[6] = 70 = key → found

**4 comparisons** vs **1 comparison**!

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Condition |
|------|------------|-----------|
| **Best** | O(1) | Target at estimated position |
| **Average** | O(log log n) | Uniform distribution |
| **Worst** | O(n) | Non-uniform distribution |

### When Does Worst Case Occur?

```
Array: [1, 2, 3, 4, 5, 1000000]
Key: 5

pos = 0 + ((5 - 1) / (1000000 - 1)) × 5 ≈ 0

The algorithm repeatedly probes near the start!
```

### Space Complexity

| Component | Space |
|-----------|-------|
| **Auxiliary** | O(1) |

### Comparison with Binary Search

| Metric | Interpolation | Binary |
|--------|---------------|--------|
| **Uniform data** | O(log log n) | O(log n) |
| **Non-uniform** | O(n) | O(log n) |
| **Probes (n=10⁶)** | ~4-5 | ~20 |

---

## ⚠️ Common Pitfalls

### 1. Division by Zero

```java
// When all elements are the same
if (array[end] == array[start]) {
    if (array[start] == key) return start;
    else return -1;
}
```

### 2. Integer Overflow

```java
// WRONG: Can overflow
int pos = start + ((key - array[start]) * (end - start)) / (array[end] - array[start]);

// BETTER: Use long or reorder operations
long numerator = (long)(key - array[start]) * (end - start);
int pos = start + (int)(numerator / (array[end] - array[start]));
```

### 3. Non-Uniform Distribution

```java
// Check if distribution is suitable
// Interpolation search should NOT be used for:
// [1, 2, 3, 4, 5, 1000000]  // Skewed
// [1, 1, 1, 1, 1, 100]      // Many duplicates
```

### 4. Negative Numbers

```java
// The formula works with negatives, but be careful with ranges
// Array: [-100, -50, 0, 50, 100], Key: -50
// pos = 0 + ((-50 - (-100)) / (100 - (-100))) × 4
//     = 0 + (50 / 200) × 4
//     = 0 + 1 = 1 ✅
```

---

## 🔧 Optimizations

### 1. Hybrid Approach

Switch to Binary Search when range is small:

```java
public int hybridSearch(int[] array, int key) {
    int start = 0, end = array.length - 1;
    
    while (start <= end && key >= array[start] && key <= array[end]) {
        if (end - start < 10) {
            // Use binary search for small ranges
            return binarySearch(array, key, start, end);
        }
        
        // Use interpolation for large ranges
        int pos = interpolatePosition(array, key, start, end);
        // ... rest of algorithm
    }
    return -1;
}
```

### 2. Quadratic Interpolation

Better estimation for quadratic distributions:

```java
// Instead of linear interpolation, use quadratic
int pos = estimateQuadratic(array, key, start, end);
```

### 3. Guard Against Poor Distribution

```java
// Use a ratio to decide
double ratio = (double)(key - array[start]) / (array[end] - array[start]);
if (ratio < 0.01 || ratio > 0.99) {
    // Likely non-uniform, fall back to binary search
    return binarySearch(array, key, start, end);
}
```

---

## 🌍 Real-World Applications

| Application | Why Interpolation Search? |
|-------------|---------------------------|
| **Numerical tables** | Uniformly spaced values |
| **Time series data** | Regular intervals |
| **Database indexes** | Sequential IDs (1, 2, 3, ...) |
| **Phone books** | Names somewhat uniformly distributed |
| **Compressed indexes** | Uniform hash distributions |

### When to Use

✅ **Good candidates:**
- Sequential IDs
- Timestamps at regular intervals
- Evenly distributed sensor readings
- Price ranges with uniform buckets

❌ **Bad candidates:**
- Exponentially distributed data
- Data with many duplicates
- Highly skewed distributions
- Small arrays (binary search is simpler)

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/InterpolationSearchTest.java](../../src/test/java/com/thealgorithms/searches/InterpolationSearchTest.java)

### Test Cases

| Input | Key | Expected | Tests |
|-------|-----|----------|-------|
| `[]` | 5 | -1 | Empty array |
| `[10,20,30,40,50]` | 10 | 0 | First element |
| `[10,20,30,40,50]` | 50 | 4 | Last element |
| `[10,20,30,40,50]` | 30 | 2 | Middle (uniform) |
| `[10,20,30,40,50]` | 25 | -1 | Not present |
| `[1,1,1,1,1]` | 1 | 0 | All same |
| `[-50,-25,0,25,50]` | 0 | 2 | With negatives |

---

## 📖 References

### Papers

- Peterson, W.W. *"Addressing for Random-Access Storage"*, IBM Journal of Research and Development, 1957

### Textbooks

- Sedgewick, R. *"Algorithms"*, Section on searching
- Knuth, D.E. *"The Art of Computer Programming"*, Vol. 3

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [BinarySearch](./binary-search.md) | More robust, always O(log n) |
| [ExponentialSearch](./exponential-search.md) | Good for unbounded arrays |
| [FibonacciSearch](./fibonacci-search.md) | Division-free binary search |

---

## 📊 Visualization

```
Search for 70 in [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]

Binary Search approach:
        [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
                        ↑
                       mid=50
                       50 < 70 → right half
                       
        [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
                                    ↑
                                   mid=80
                                   80 > 70 → left
        ... continues (4 steps)


Interpolation Search approach:
        [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
        ↑                            ↑         ↑
       start                        pos       end
       
        pos = 0 + (70-10)/(100-10) × 9
            = 0 + 60/90 × 9
            = 6
        
        array[6] = 70 ✅ Found in 1 step!
```

---

## 📈 Performance Comparison Graph

```
Comparisons vs Array Size (Uniform Distribution)

Comparisons
    │
 30 ┤                               Binary O(log n)
    │                         ╭─────────────────────
 20 ┤                   ╭─────╯
    │             ╭─────╯
 10 ┤       ╭─────╯
    │ ╭─────╯         Interpolation O(log log n)
  5 ┤─╯  ──────────────────────────────────────
    │
  0 ┼─────┬─────┬─────┬─────┬─────┬─────┬─────→
        10²   10³   10⁴   10⁵   10⁶   10⁷  Array Size
```

---

[← Back to Searching Algorithms](../README.md) | [Next: JumpSearch →](./jump-search.md)
