# LinearSearch

> **Category:** Sequential Search  
> **Paradigm:** Brute Force  
> **Prerequisite:** None  
> **In-Place:** Yes

---

## 📋 Overview

Linear Search (also known as Sequential Search) is the simplest searching algorithm. It sequentially checks each element of the list until a match is found or the entire list has been searched. Unlike Binary Search, it **does not require** a sorted array.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(1) |
| **Time (Average)** | O(n) |
| **Time (Worst)** | O(n) |
| **Space** | O(1) |
| **Prerequisite** | None |

---

## 🔬 Mathematical Analysis

### Algorithm Concept

Linear Search examines each element in sequence:
1. Start from the first element
2. Compare current element with target
3. If match, return index
4. If no match, move to next element
5. If end reached, return "not found"

### Expected Comparisons

**If element is present:**

For uniform distribution (element equally likely at any position):
$$
E[C] = \frac{1}{n}\sum_{i=1}^{n}i = \frac{1}{n} \cdot \frac{n(n+1)}{2} = \frac{n+1}{2} \approx \frac{n}{2}
$$

**If element may or may not be present:**

With probability $p$ that element exists:
$$
E[C] = p \cdot \frac{n+1}{2} + (1-p) \cdot n
$$

### Probability of Finding After k Comparisons

If target is at position $i$ with probability $\frac{1}{n}$:
$$
P(\text{found at position } k) = \frac{1}{n}
$$

---

## 📝 Pseudocode

```
LINEAR-SEARCH(A, key)
    for i ← 0 to length(A) - 1 do
        if A[i] = key then
            return i
    return -1  // Not found
```

### Sentinel Linear Search (Optimization)

```
SENTINEL-LINEAR-SEARCH(A, key)
    n ← length(A)
    last ← A[n-1]
    A[n-1] ← key      // Place sentinel
    
    i ← 0
    while A[i] ≠ key do
        i ← i + 1
    
    A[n-1] ← last     // Restore original
    
    if i < n-1 OR last = key then
        return i
    return -1
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/searches/LinearSearch.java](../../src/main/java/com/thealgorithms/searches/LinearSearch.java)

```java
/**
 * Linear search is the easiest search algorithm. It works with sorted and
 * unsorted arrays (unlike binary search which works only with sorted array). 
 * This algorithm just compares all elements of an array to find a value.
 *
 * Worst-case performance O(n) 
 * Best-case performance O(1) 
 * Average performance O(n) 
 * Worst-case space complexity O(1)
 *
 * @author Varun Upadhyay
 * @author Podshivalov Nikita
 * @see BinarySearch
 * @see SearchAlgorithm
 */
public class LinearSearch implements SearchAlgorithm {

    /**
     * Generic Linear search method
     *
     * @param array List to be searched
     * @param value Key being searched for
     * @return Location of the key
     */
    @Override
    public <T extends Comparable<T>> int find(T[] array, T value) {
        for (int i = 0; i < array.length; i++) {
            if (array[i].compareTo(value) == 0) {
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
| **Simple** | Minimal code, easy to understand |
| **Flexible** | No sorting requirement |
| **Interface** | Implements `SearchAlgorithm` |

---

## 🎯 Step-by-Step Example

### Input: Array `[4, 2, 7, 1, 9, 3]`, Target: `7`

```
Step 1: Compare array[0]=4 with 7 → 4 ≠ 7, continue
        [4, 2, 7, 1, 9, 3]
         ↑
         
Step 2: Compare array[1]=2 with 7 → 2 ≠ 7, continue
        [4, 2, 7, 1, 9, 3]
            ↑
            
Step 3: Compare array[2]=7 with 7 → 7 = 7, found! ✅
        [4, 2, 7, 1, 9, 3]
               ↑
```

### Result: Index `2` (found in 3 comparisons)

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Condition |
|------|------------|-----------|
| **Best** | O(1) | Target at first position |
| **Average** | O(n) | Target at random position |
| **Worst** | O(n) | Target at last position or not present |

### Space Complexity

| Component | Space |
|-----------|-------|
| **Auxiliary** | O(1) |

### Comparison Table

| Array Size | Linear (avg) | Binary (worst) |
|------------|--------------|----------------|
| 10 | 5 | 4 |
| 100 | 50 | 7 |
| 1,000 | 500 | 10 |
| 1,000,000 | 500,000 | 20 |

---

## ⚠️ Common Pitfalls

### 1. Not Returning -1 for Empty Array

```java
// Array bounds are handled naturally in the for loop
// Empty array: loop doesn't execute, returns -1
for (int i = 0; i < array.length; i++) { ... }
```

### 2. Using == Instead of .equals() for Objects

```java
// WRONG for objects
if (array[i] == value)

// CORRECT for Comparable objects
if (array[i].compareTo(value) == 0)

// OR for general objects
if (array[i].equals(value))
```

### 3. Index Out of Bounds

```java
// Ensure bounds checking
for (int i = 0; i < array.length; i++)  // Correct
for (int i = 0; i <= array.length; i++) // Wrong! Accesses array[length]
```

---

## 🔧 Optimizations

### 1. Sentinel Search

Eliminates bounds check in each iteration:

```java
public int sentinelSearch(int[] array, int key) {
    int n = array.length;
    if (n == 0) return -1;
    
    int last = array[n - 1];
    array[n - 1] = key;  // Place sentinel
    
    int i = 0;
    while (array[i] != key) {
        i++;
    }
    
    array[n - 1] = last;  // Restore
    
    if (i < n - 1 || last == key) {
        return i;
    }
    return -1;
}
```

**Benefit:** Removes one comparison per iteration (bounds check).

### 2. Move-to-Front Heuristic

For repeated searches, move found elements to front:

```java
public int moveToFrontSearch(T[] array, T key) {
    for (int i = 0; i < array.length; i++) {
        if (array[i].compareTo(key) == 0) {
            // Move to front for faster future access
            T temp = array[i];
            System.arraycopy(array, 0, array, 1, i);
            array[0] = temp;
            return 0;
        }
    }
    return -1;
}
```

### 3. Transposition Heuristic

Swap found element with predecessor:

```java
public int transpositionSearch(T[] array, T key) {
    for (int i = 0; i < array.length; i++) {
        if (array[i].compareTo(key) == 0) {
            if (i > 0) {
                // Swap with predecessor
                T temp = array[i];
                array[i] = array[i - 1];
                array[i - 1] = temp;
                return i - 1;
            }
            return i;
        }
    }
    return -1;
}
```

---

## 🌍 Real-World Applications

| Application | Why Linear Search? |
|-------------|---------------------|
| **Small datasets** | Overhead of sorting not worth it |
| **Unsorted data** | Can't use binary search |
| **Linked lists** | Sequential access only |
| **Single search** | One-time search doesn't justify sorting |
| **Finding all occurrences** | Need to scan entire array anyway |
| **Streaming data** | Data arrives sequentially |

### When to Use Linear Search

1. **Array is unsorted** and sorting isn't needed otherwise
2. **Array is small** (n < ~100)
3. **Searching only once** (cost of sorting: O(n log n))
4. **Data structure doesn't support random access** (linked list)

### When NOT to Use

1. **Sorted array with many searches** → Use Binary Search
2. **Large datasets** → Sort first, then Binary Search
3. **Frequent lookups** → Use HashMap O(1)

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/LinearSearchTest.java](../../src/test/java/com/thealgorithms/searches/LinearSearchTest.java)

### Test Cases

| Input | Key | Expected | Tests |
|-------|-----|----------|-------|
| `[]` | 5 | -1 | Empty array |
| `[5]` | 5 | 0 | Single element found |
| `[5]` | 3 | -1 | Single element not found |
| `[4,2,7,1,9]` | 4 | 0 | First element |
| `[4,2,7,1,9]` | 9 | 4 | Last element |
| `[4,2,7,1,9]` | 7 | 2 | Middle element |
| `[4,2,7,1,9]` | 6 | -1 | Not present |
| `[1,1,1,1]` | 1 | 0 | All same (returns first) |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS)
- Knuth, D.E. *"The Art of Computer Programming"*, Vol. 3

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [BinarySearch](./binary-search.md) | Faster O(log n) for sorted arrays |
| [SentinelLinearSearch](./sentinel-linear-search.md) | Optimized variant |
| [JumpSearch](./jump-search.md) | O(√n) for sorted arrays |
| [InterpolationSearch](./interpolation-search.md) | O(log log n) for uniform distribution |

---

## 📊 Visualization

```
Search for 7 in [4, 2, 7, 1, 9, 3]

Step 1: [4, 2, 7, 1, 9, 3]
         ↑
         4 ≠ 7

Step 2: [4, 2, 7, 1, 9, 3]
            ↑
            2 ≠ 7

Step 3: [4, 2, 7, 1, 9, 3]
               ↑
               7 = 7 ✅ Found at index 2!


Search for 6 in [4, 2, 7, 1, 9, 3]

Step 1-6: [4, 2, 7, 1, 9, 3]
           ↑  ↑  ↑  ↑  ↑  ↑
           
All elements checked, 6 not found → return -1
```

---

## 📈 When to Prefer Linear Over Binary Search

```
Break-even point: When is sorting + binary search worth it?

Single search:
  Linear: O(n)
  Sort + Binary: O(n log n) + O(log n) ≈ O(n log n)
  → Linear wins!

k searches:
  Linear: O(k × n)
  Sort + Binary: O(n log n) + O(k × log n)
  
  Break-even: k × n ≈ n log n + k log n
              k ≈ log n (approximately)
              
For k ≥ log n searches on same data → Sort once, then Binary Search
```

---

[← Back to Searching Algorithms](../README.md) | [Next: InterpolationSearch →](./interpolation-search.md)
