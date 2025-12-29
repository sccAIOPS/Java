# JumpSearch

> **Category:** Block Search  
> **Paradigm:** Jump and Linear Scan  
> **Prerequisite:** Sorted Array  
> **In-Place:** Yes

---

## 📋 Overview

Jump Search (also known as Block Search) is a searching algorithm for sorted arrays. It works by jumping ahead by fixed steps (blocks) and then performing a linear search within the identified block. The optimal block size is $\sqrt{n}$, giving a time complexity of $O(\sqrt{n})$.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(1) |
| **Time (Average)** | O(√n) |
| **Time (Worst)** | O(√n) |
| **Space** | O(1) |
| **Prerequisite** | Sorted array |

---

## 🔬 Mathematical Analysis

### Algorithm Concept

1. **Jump Phase:** Skip ahead by block size until `array[block] >= key`
2. **Linear Phase:** Search backwards or forwards within the block

### Optimal Block Size

Let $m$ = block size, $n$ = array size

**Jumps needed:** $\frac{n}{m}$
**Linear comparisons (worst):** $m - 1$

**Total comparisons:**
$$
C(m) = \frac{n}{m} + m - 1
$$

**Minimize by taking derivative:**
$$
\frac{dC}{dm} = -\frac{n}{m^2} + 1 = 0
$$

$$
m^2 = n \Rightarrow m = \sqrt{n}
$$

### Time Complexity with Optimal Block Size

$$
T(n) = \frac{n}{\sqrt{n}} + \sqrt{n} - 1 = 2\sqrt{n} - 1 = O(\sqrt{n})
$$

### Comparison Count Analysis

| Phase | Comparisons |
|-------|-------------|
| Jump | $\frac{n}{\sqrt{n}} = \sqrt{n}$ |
| Linear | $\sqrt{n} - 1$ |
| **Total** | $2\sqrt{n} - 1$ |

---

## 📝 Pseudocode

```
JUMP-SEARCH(A, key)
    n ← length(A)
    blockSize ← floor(√n)
    
    // Jump Phase: Find the block containing key
    limit ← blockSize
    while limit < n AND key > A[limit] do
        limit ← min(limit + blockSize, n - 1)
    
    // Linear Phase: Search within the block
    start ← limit - blockSize
    for i ← start to limit do
        if A[i] = key then
            return i
    
    return -1
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/searches/JumpSearch.java](../../src/main/java/com/thealgorithms/searches/JumpSearch.java)

```java
/**
 * An implementation of the Jump Search algorithm.
 *
 * Jump Search is an algorithm for searching sorted arrays. It works by dividing the array
 * into blocks of a fixed size (the block size is typically the square root of the array length)
 * and jumping ahead by this block size to find a range where the target element may be located.
 * Once the range is found, a linear search is performed within that block.
 *
 * The Jump Search algorithm is particularly effective for large sorted arrays where the cost of
 * performing a linear search on the entire array would be prohibitive.
 *
 * Worst-case performance: O(√N)
 * Best-case performance: O(1)
 * Average performance: O(√N)
 * Worst-case space complexity: O(1)
 *
 * This class implements the SearchAlgorithm interface, providing a generic search method
 * for any comparable type.
 */
public class JumpSearch implements SearchAlgorithm {

    /**
     * Jump Search algorithm implementation.
     *
     * @param array the sorted array containing elements
     * @param key   the element to be searched
     * @return the index of key if found, otherwise -1
     */
    @Override
    public <T extends Comparable<T>> int find(T[] array, T key) {
        int length = array.length;
        int blockSize = (int) Math.sqrt(length);

        int limit = blockSize;
        // Jumping ahead to find the block where the key may be located
        while (limit < length && key.compareTo(array[limit]) > 0) {
            limit = Math.min(limit + blockSize, length - 1);
        }

        // Perform linear search within the identified block
        for (int i = limit - blockSize; i <= limit && i < length; i++) {
            if (array[i].equals(key)) {
                return i;
            }
        }
        return -1;
    }
}
```

### Implementation Features

| Feature | Description |
|---------|-------------|
| **Generic** | Works with any `Comparable<T>` type |
| **Optimal block** | Uses `√n` block size |
| **Boundary safe** | Uses `Math.min` for limits |
| **Interface** | Implements `SearchAlgorithm` |

---

## 🎯 Step-by-Step Example

### Input: Array `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]`, Target: `11`

**Setup:**
```
n = 16
blockSize = √16 = 4
```

**Jump Phase:**
```
Block boundaries: [0, 4, 8, 12, 15]

Step 1: limit = 4
        array[4] = 4 < 11 → jump

Step 2: limit = 8
        array[8] = 8 < 11 → jump

Step 3: limit = 12
        array[12] = 12 > 11 → stop jumping
```

**Linear Phase:**
```
Search in block [8, 12]:
        index 8:  array[8] = 8 ≠ 11
        index 9:  array[9] = 9 ≠ 11
        index 10: array[10] = 10 ≠ 11
        index 11: array[11] = 11 = 11 ✅ Found!
```

### Result: Index `11`

**Total comparisons:** 3 jumps + 4 linear = 7

**Comparison with other methods:**
- Linear Search: 12 comparisons
- Binary Search: 4 comparisons
- Jump Search: 7 comparisons

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Condition |
|------|------------|-----------|
| **Best** | O(1) | Target at first block boundary |
| **Average** | O(√n) | Random position |
| **Worst** | O(√n) | Target at end of a block |

### Space Complexity

| Component | Space |
|-----------|-------|
| **Auxiliary** | O(1) |

### Comparison with Other Searches

| Algorithm | Time | Space | Notes |
|-----------|------|-------|-------|
| Linear | O(n) | O(1) | No sorting required |
| **Jump** | O(√n) | O(1) | Good for large sorted arrays |
| Binary | O(log n) | O(1)/O(log n) | Best for random access |
| Interpolation | O(log log n) | O(1) | Best for uniform data |

### Performance Comparison

| Array Size | Linear | Jump | Binary |
|------------|--------|------|--------|
| 100 | 50 | 20 | 7 |
| 10,000 | 5,000 | 200 | 14 |
| 1,000,000 | 500,000 | 2,000 | 20 |

---

## ⚠️ Common Pitfalls

### 1. Wrong Block Size

```java
// Too small: too many jumps
int blockSize = 2;  // O(n) jumps!

// Too large: too much linear search
int blockSize = n / 2;  // O(n) linear!

// Optimal
int blockSize = (int) Math.sqrt(n);
```

### 2. Array Index Out of Bounds

```java
// WRONG: doesn't handle array bounds
while (limit < length && key > array[limit]) {
    limit += blockSize;  // Can exceed length!
}

// CORRECT: clamp to valid range
limit = Math.min(limit + blockSize, length - 1);
```

### 3. Starting Linear Search at Wrong Position

```java
// WRONG: starts at 0 instead of previous block start
for (int i = 0; i <= limit; i++)

// CORRECT: start at previous block boundary
for (int i = limit - blockSize; i <= limit && i < length; i++)
```

### 4. Empty Array Handling

```java
// Add check for empty array
if (array.length == 0) return -1;
int blockSize = (int) Math.sqrt(array.length);
```

---

## 🔧 Optimizations

### 1. Exponential Block Growth

For very large arrays, grow block size exponentially:

```java
public int exponentialJumpSearch(int[] array, int key) {
    int n = array.length;
    if (n == 0) return -1;
    
    int bound = 1;
    while (bound < n && array[bound] < key) {
        bound *= 2;  // Exponential growth
    }
    
    // Binary search in range [bound/2, min(bound, n-1)]
    return binarySearch(array, key, bound / 2, Math.min(bound, n - 1));
}
```

### 2. Adaptive Block Size

Adjust block size based on initial jumps:

```java
public int adaptiveJumpSearch(int[] array, int key, int blockSize) {
    // If first jump overshoots significantly, reduce block size
    // If taking too many jumps, increase block size
}
```

### 3. Backward Linear Search

Search backward from the limit instead of forward from the previous limit:

```java
// Forward (current implementation)
for (int i = limit - blockSize; i <= limit; i++)

// Backward (potentially faster if key is near limit)
for (int i = limit; i >= limit - blockSize; i--)
```

---

## 🌍 Real-World Applications

| Application | Why Jump Search? |
|-------------|------------------|
| **Tape/Sequential storage** | Minimizes expensive seeks |
| **Large sorted files** | Balance between jumps and reads |
| **Linked lists** | No random access, but can skip nodes |
| **CD/DVD search** | Physical seek + sequential read |

### When to Use Jump Search

✅ **Good for:**
- Sequential access storage (tapes, disks)
- When jumps are expensive but linear scan is cheap
- Systems with high latency random access
- Large sorted arrays on slow storage

❌ **Not ideal for:**
- Small arrays (linear search is simpler)
- RAM-based arrays (binary search is faster)
- Unsorted data

### Comparison: Jump vs Binary for Storage Types

| Storage Type | Jump Search | Binary Search |
|--------------|-------------|---------------|
| RAM | ❌ Binary is faster | ✅ |
| SSD | Both good | ✅ |
| HDD | ✅ Fewer seeks | Many seeks |
| Tape | ✅ Sequential reads | ❌ Random access slow |

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/JumpSearchTest.java](../../src/test/java/com/thealgorithms/searches/JumpSearchTest.java)

### Test Cases

| Input | Key | Expected | Tests |
|-------|-----|----------|-------|
| `[]` | 5 | -1 | Empty array |
| `[5]` | 5 | 0 | Single element found |
| `[0,1,2,...,15]` | 0 | 0 | First element |
| `[0,1,2,...,15]` | 15 | 15 | Last element |
| `[0,1,2,...,15]` | 11 | 11 | Middle of block |
| `[0,1,2,...,15]` | 4 | 4 | Block boundary |
| `[0,1,2,...,15]` | 20 | -1 | Not present |

---

## 📖 References

### Papers

- Various algorithms papers on block search

### Textbooks

- Skiena, S.S. *"The Algorithm Design Manual"*

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [LinearSearch](./linear-search.md) | Used in linear phase |
| [BinarySearch](./binary-search.md) | Faster for RAM, O(log n) |
| [ExponentialSearch](./exponential-search.md) | Unbounded arrays |
| [FibonacciSearch](./fibonacci-search.md) | Uses Fibonacci numbers for blocks |

---

## 📊 Visualization

```
Search for 11 in [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

Block size = √16 = 4

Jump Phase:
┌───────────────────────────────────────────────────────────────────┐
│  0   1   2   3 │  4   5   6   7 │  8   9  10  11 │ 12  13  14  15 │
│      Block 0   │     Block 1    │     Block 2    │     Block 3    │
└───────────────────────────────────────────────────────────────────┘
                 ↑                ↑                 ↑
              Jump 1           Jump 2            Jump 3
              arr[4]=4<11      arr[8]=8<11       arr[12]=12>11
                                                 STOP!

Linear Phase:
┌───────────────────────────────────────────────────────────────────┐
│  0   1   2   3 │  4   5   6   7 │  8   9  10  11 │ 12  13  14  15 │
└───────────────────────────────────────────────────────────────────┘
                                   ↑   ↑   ↑   ↑
                                  [8] [9] [10][11]
                                  ≠   ≠   ≠   = ✅

Found at index 11!
```

---

## 📈 Block Size vs Performance

```
Comparisons for array of size 1000

Total Comparisons
    │
500 ┤ ●                                              block=2
    │
400 ┤
    │
300 ┤
    │
200 ┤
    │         
100 ┤                    ●                           block=500
    │              ●  ●  ●  ●
 63 ┤           ● optimal (√1000 ≈ 32)
    │
  0 ┼─────┬─────┬─────┬─────┬─────┬─────→
         10    32   100   200   500   Block Size
```

The minimum is at $\sqrt{n} \approx 32$ for $n = 1000$.

---

[← Back to Searching Algorithms](../README.md) | [Next: ExponentialSearch →](./exponential-search.md)
