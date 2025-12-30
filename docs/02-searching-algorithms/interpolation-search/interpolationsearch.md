# InterpolationSearch

> **Category:** Probe-Based Search  
> **Prerequisite:** Sorted Array with Uniform Distribution  
> **Paradigm:** Divide and Conquer (Adaptive)  
> **Approach:** Iterative

---

## 📋 Overview

Interpolation Search is an improved variant of Binary Search that works on uniformly distributed sorted arrays. Instead of always going to the middle, it estimates the position of the target based on its value relative to the range of values—similar to how humans search for a word in a dictionary.

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

### Algorithm Concept

Instead of probing at the middle position, Interpolation Search estimates the position using **linear interpolation**:

$$
\text{pos} = \text{lo} + \frac{(\text{key} - A[\text{lo}]) \times (\text{hi} - \text{lo})}{A[\text{hi}] - A[\text{lo}]}
$$

This formula assumes a linear relationship between index and value.

### Derivation of Position Formula

If values are uniformly distributed:
$$
\frac{\text{key} - A[\text{lo}]}{A[\text{hi}] - A[\text{lo}]} \approx \frac{\text{pos} - \text{lo}}{\text{hi} - \text{lo}}
$$

Solving for `pos`:
$$
\text{pos} = \text{lo} + \left\lfloor \frac{(\text{key} - A[\text{lo}])(\text{hi} - \text{lo})}{A[\text{hi}] - A[\text{lo}]} \right\rfloor
$$

### Time Complexity Analysis

**Uniform Distribution:**
$$
T(n) = O(\log \log n)
$$

This is because each probe reduces the search space by a **square root factor** on average.

**Non-Uniform Distribution (Worst Case):**
$$
T(n) = O(n)
$$

Occurs when values are clustered (e.g., exponential distribution).

### Why O(log log n)?

With uniform distribution, after one probe, the expected remaining search space is $O(\sqrt{n})$:
$$
T(n) = T(\sqrt{n}) + O(1)
$$

Let $n = 2^{2^k}$. Then $\sqrt{n} = 2^{2^{k-1}}$:
$$
T(2^{2^k}) = T(2^{2^{k-1}}) + O(1) = O(k) = O(\log \log n)
$$

---

## 📝 Pseudocode

```
INTERPOLATION-SEARCH(A, key)
    lo ← 0
    hi ← length(A) - 1
    
    while lo ≤ hi AND key ≥ A[lo] AND key ≤ A[hi] do
        // Guard against division by zero
        if A[hi] = A[lo] then
            if A[lo] = key then
                return lo
            else
                return -1
        
        // Calculate probe position
        pos ← lo + ((key - A[lo]) × (hi - lo)) / (A[hi] - A[lo])
        
        if A[pos] = key then
            return pos
        else if A[pos] < key then
            lo ← pos + 1
        else
            hi ← pos - 1
    
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
 * <p>
 * The performance of this algorithm can vary:
 * - Worst-case performance: O(n)
 * - Best-case performance: O(1)
 * - Average performance: O(log(log(n))) if the elements are uniformly distributed; otherwise O(n)
 * - Worst-case space complexity: O(1)
 * </p>
 *
 * <p>
 * This search algorithm requires the input array to be sorted.
 * </p>
 *
 * @author Podshivalov Nikita (https://github.com/nikitap492)
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
            } // If key is smaller, key is in lower part
            else {
                end = pos - 1;
            }
        }
        return -1;
    }
}
```

### Implementation Features

| Feature | Description |
|---------|-------------|
| **Integer-only** | Works with `int[]` |
| **Iterative** | O(1) space complexity |
| **Bounds check** | Verifies key is in range |

---

## 🎯 Step-by-Step Example

### Input: Array `[10, 20, 30, 40, 50, 60, 70, 80, 90, 100]`, Key = `70`

**Values are uniformly distributed (step = 10)**

**Initial:** start=0, end=9, A[0]=10, A[9]=100

**Step 1:** Calculate position
$$
\text{pos} = 0 + \frac{(70 - 10) \times (9 - 0)}{100 - 10} = 0 + \frac{60 \times 9}{90} = 0 + 6 = 6
$$

```
[10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
  0   1   2   3   4   5   6   7   8    9
                         ↑
                       pos=6
                     array[6]=70

70 == 70 → FOUND at index 6 ✅
```

### Result: Index `6` (1 comparison!)

### Compare with Binary Search

Binary Search would take:
1. mid=4, A[4]=50, 70 > 50, go right
2. mid=7, A[7]=80, 70 < 80, go left
3. mid=5, A[5]=60, 70 > 60, go right
4. mid=6, A[6]=70, found!

**4 comparisons vs 1 comparison!**

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When |
|------|------------|------|
| **Best** | O(1) | Direct hit on first probe |
| **Average** | O(log log n) | Uniform distribution |
| **Worst** | O(n) | Non-uniform distribution |

### When O(n) Occurs

```
Non-uniform array: [1, 2, 3, 4, 5, 1000000]
Searching for 5:

pos = 0 + ((5-1) × (5-0)) / (1000000-1) ≈ 0

The formula always estimates near the beginning!
```

### Comparison Table

| Distribution | Interpolation | Binary |
|--------------|---------------|--------|
| Uniform | **O(log log n)** | O(log n) |
| Non-uniform | O(n) | **O(log n)** |

---

## ⚠️ Common Pitfalls

### 1. Division by Zero

```java
// Wrong: can divide by zero if all elements are equal
int pos = start + ((key - array[start]) * (end - start)) / (array[end] - array[start]);

// Correct: guard against division by zero
if (array[end] == array[start]) {
    return (array[start] == key) ? start : -1;
}
```

### 2. Integer Overflow

```java
// Wrong: can overflow
int pos = start + ((key - array[start]) * (end - start)) / (array[end] - array[start]);

// Safer: use long arithmetic
long numerator = (long)(key - array[start]) * (end - start);
int pos = start + (int)(numerator / (array[end] - array[start]));
```

### 3. Using with Non-Numeric Data

```java
// Interpolation Search is designed for numeric values
// For strings or objects, use Binary Search instead
```

---

## 🔧 Optimizations

### 1. Combine with Binary Search

Switch to Binary Search when distribution seems non-uniform:

```java
public int hybridSearch(int[] array, int key) {
    int start = 0, end = array.length - 1;
    int iterations = 0;
    int maxInterpolation = (int) Math.log(Math.log(array.length + 1)) * 2;
    
    while (start <= end && key >= array[start] && key <= array[end]) {
        iterations++;
        
        int pos;
        if (iterations <= maxInterpolation) {
            // Try interpolation
            pos = interpolate(array, key, start, end);
        } else {
            // Fall back to binary search
            pos = (start + end) / 2;
        }
        
        if (array[pos] == key) return pos;
        if (array[pos] < key) start = pos + 1;
        else end = pos - 1;
    }
    return -1;
}
```

### 2. Quadratic Interpolation

Use quadratic formula for slightly better estimates:

```java
// For even better estimates with smooth distributions
double ratio = (double)(key - array[start]) / (array[end] - array[start]);
int pos = start + (int)(Math.sqrt(ratio) * (end - start));
```

---

## 🌍 Real-World Applications

| Application | Why Interpolation Search? |
|-------------|---------------------------|
| **Phone directories** | Names roughly uniform |
| **Dictionary lookup** | Words somewhat evenly distributed |
| **Database queries** | Sequential IDs |
| **Time-series data** | Evenly-spaced timestamps |
| **Sensor data** | Linear sensor readings |

### When to Use vs Binary Search

```
Use Interpolation Search when:
✓ Data is uniformly distributed
✓ Values are numeric
✓ Array is large (n > 1000)
✓ Probing cost is expensive

Use Binary Search when:
✓ Distribution is unknown/non-uniform
✓ Data is non-numeric
✓ Array is small
✓ Simplicity is preferred
```

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/InterpolationSearchTest.java](../../src/test/java/com/thealgorithms/searches/InterpolationSearchTest.java)

### Test Cases

| Input Array | Key | Expected | Tests |
|-------------|-----|----------|-------|
| `[10,20,30,40,50]` | 30 | 2 | Uniform, middle |
| `[10,20,30,40,50]` | 10 | 0 | First element |
| `[10,20,30,40,50]` | 50 | 4 | Last element |
| `[10,20,30,40,50]` | 25 | -1 | Not found |
| `[1,1,1,1,1]` | 1 | 0 | All same (edge case) |
| `[1,2,3,4,1000000]` | 3 | 2 | Non-uniform |

---

## 📖 References

### Textbooks

- Knuth, D.E. *"The Art of Computer Programming"*, Vol. 3, Section 6.2.1
- Sedgewick, R. *"Algorithms"*

### Papers

- Perl, Y., Itai, A., Avni, H. *"Interpolation Search—A Log Log N Search"*, Communications of the ACM, 1978

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [BinarySearch](./binarysearch.md) | More robust, O(log n) |
| [ExponentialSearch](./exponential-search.md) | For unbounded arrays |
| [FibonacciSearch](./fibonacci-search.md) | Uses Fibonacci numbers |

---

## 📊 Visualization

```
Searching for 70 in [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]

Binary Search Path:           Interpolation Search Path:
┌────────────────────────┐    ┌────────────────────────┐
│ 10 20 30 40 50 60 70 80│    │ 10 20 30 40 50 60 70 80│
│                ↓       │    │                   ↓    │
│ Step 1: mid=50, go R   │    │ Step 1: pos=70 FOUND!  │
├────────────────────────┤    └────────────────────────┘
│       60 70 80 90 100  │    
│          ↓             │    Formula: pos = 0 + (70-10)×9/(100-10)
│ Step 2: mid=80, go L   │              = 0 + 60×9/90
├────────────────────────┤              = 0 + 6 = 6
│       60 70            │    
│          ↓             │    Direct hit! ✅
│ Step 3: mid=60, go R   │    
├────────────────────────┤    
│          70            │    
│          ↓             │    
│ Step 4: FOUND!         │    
└────────────────────────┘    
   4 comparisons              1 comparison
```

---

[← Back to Searching Algorithms](../README.md) | [Next: JumpSearch →](./jumpsearch.md)
