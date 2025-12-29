# BinarySearch

> **Category:** Divide and Conquer Search  
> **Paradigm:** Interval Halving  
> **Prerequisite:** Sorted Array  
> **In-Place:** Yes

---

## 📋 Overview

Binary Search is one of the most fundamental and efficient searching algorithms. It finds the position of a target value within a **sorted array** by repeatedly dividing the search interval in half. If the target value is less than the middle element, the search continues in the lower half; otherwise, it continues in the upper half.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(1) |
| **Time (Average)** | O(log n) |
| **Time (Worst)** | O(log n) |
| **Space (Iterative)** | O(1) |
| **Space (Recursive)** | O(log n) |
| **Prerequisite** | Sorted array |

---

## 🔬 Mathematical Analysis

### Algorithm Concept

Binary Search uses the **divide and conquer** principle:

1. Compare target with middle element
2. If equal, return position
3. If target < middle, search left half
4. If target > middle, search right half
5. Repeat until found or interval is empty

### Recurrence Relation

$$
T(n) = T\left(\frac{n}{2}\right) + O(1)
$$

Using the Master Theorem (Case 2: $a = 1, b = 2, f(n) = O(1)$):
$$
T(n) = O(\log n)
$$

### Number of Comparisons

**Maximum comparisons** to find (or not find) an element:
$$
C(n) = \lfloor \log_2 n \rfloor + 1 = O(\log n)
$$

| Array Size | Max Comparisons |
|------------|-----------------|
| 10 | 4 |
| 100 | 7 |
| 1,000 | 10 |
| 1,000,000 | 20 |
| 1,000,000,000 | 30 |

### Why So Fast?

Each comparison eliminates **half** of the remaining elements:
$$
n \rightarrow \frac{n}{2} \rightarrow \frac{n}{4} \rightarrow \cdots \rightarrow 1
$$

After $k$ comparisons: $\frac{n}{2^k} = 1 \Rightarrow k = \log_2 n$

---

## 📝 Pseudocode

### Recursive Version (Implemented)

```
BINARY-SEARCH(A, key, left, right)
    if right < left then
        return -1  // Not found
    
    mid ← (left + right) / 2  // or (left + right) >>> 1
    
    if key = A[mid] then
        return mid
    else if key < A[mid] then
        return BINARY-SEARCH(A, key, left, mid - 1)
    else
        return BINARY-SEARCH(A, key, mid + 1, right)
```

### Iterative Version

```
BINARY-SEARCH-ITERATIVE(A, key)
    left ← 0
    right ← length(A) - 1
    
    while left ≤ right do
        mid ← (left + right) / 2
        
        if key = A[mid] then
            return mid
        else if key < A[mid] then
            right ← mid - 1
        else
            left ← mid + 1
    
    return -1  // Not found
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/searches/BinarySearch.java](../../src/main/java/com/thealgorithms/searches/BinarySearch.java)

```java
/**
 * Binary search is one of the most popular algorithms. The algorithm finds the
 * position of a target value within a sorted array.
 *
 * Worst-case performance O(log n) 
 * Best-case performance O(1) 
 * Average performance O(log n) 
 * Worst-case space complexity O(1)
 *
 * @author Varun Upadhyay
 * @author Podshivalov Nikita
 * @see SearchAlgorithm
 * @see IterativeBinarySearch
 */
class BinarySearch implements SearchAlgorithm {

    /**
     * @param array is an array where the element should be found
     * @param key is an element which should be found
     * @param <T> is any comparable type
     * @return index of the element
     */
    @Override
    public <T extends Comparable<T>> int find(T[] array, T key) {
        return search(array, key, 0, array.length - 1);
    }

    /**
     * This method implements the Generic Binary Search
     *
     * @param array The array to make the binary search
     * @param key The number you are looking for
     * @param left The lower bound
     * @param right The upper bound
     * @return the location of the key
     */
    private <T extends Comparable<T>> int search(T[] array, T key, int left, int right) {
        if (right < left) {
            return -1; // this means that the key not found
        }
        // find median
        int median = (left + right) >>> 1;
        int comp = key.compareTo(array[median]);

        if (comp == 0) {
            return median;
        } else if (comp < 0) {
            return search(array, key, left, median - 1);
        } else {
            return search(array, key, median + 1, right);
        }
    }
}
```

### Implementation Features

| Feature | Description |
|---------|-------------|
| **Generic** | Works with any `Comparable<T>` type |
| **Overflow-safe** | Uses `>>> 1` instead of `/ 2` |
| **Recursive** | Clean recursive implementation |
| **Interface** | Implements `SearchAlgorithm` |

---

## 🎯 Step-by-Step Example

### Input: Array `[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]`, Target: `23`

**Initial State:**
```
Array: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
        0  1  2   3   4   5   6   7   8   9
left = 0, right = 9
```

**Iteration 1:**
```
mid = (0 + 9) >>> 1 = 4
array[4] = 16
23 > 16 → search right half
left = 5, right = 9
```

**Iteration 2:**
```
mid = (5 + 9) >>> 1 = 7
array[7] = 56
23 < 56 → search left half
left = 5, right = 6
```

**Iteration 3:**
```
mid = (5 + 6) >>> 1 = 5
array[5] = 23
23 = 23 → Found! ✅
```

### Result: Index `5` (found in 3 comparisons)

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When |
|------|------------|------|
| **Best** | O(1) | Target at middle |
| **Average** | O(log n) | Random position |
| **Worst** | O(log n) | Target at end or not present |

### Space Complexity

| Implementation | Space |
|----------------|-------|
| **Iterative** | O(1) |
| **Recursive** | O(log n) stack frames |

### Comparison with Linear Search

| Metric | Binary Search | Linear Search |
|--------|---------------|---------------|
| **Time** | O(log n) | O(n) |
| **Prerequisite** | Sorted | None |
| **1M elements** | ~20 ops | ~500K ops avg |

---

## ⚠️ Common Pitfalls

### 1. Integer Overflow in Midpoint

```java
// WRONG: Can overflow for large arrays
int mid = (left + right) / 2;

// CORRECT: Overflow-safe
int mid = left + (right - left) / 2;
// OR
int mid = (left + right) >>> 1;
```

### 2. Infinite Loop

```java
// WRONG: Doesn't shrink interval
if (key < array[mid])
    right = mid;  // Should be mid - 1

// CORRECT
if (key < array[mid])
    right = mid - 1;
```

### 3. Off-by-One Error

```java
// WRONG: Missing the edge case
while (left < right)  // Should be <=

// CORRECT
while (left <= right)
```

### 4. Unsorted Array

Binary Search **requires** a sorted array. Using it on unsorted data produces incorrect results!

---

## 🔧 Variants and Optimizations

### 1. Find First/Last Occurrence

```java
// Find first occurrence (for duplicates)
public int findFirst(int[] arr, int target) {
    int result = -1;
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int mid = (left + right) >>> 1;
        if (arr[mid] == target) {
            result = mid;
            right = mid - 1;  // Keep searching left
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return result;
}
```

### 2. Lower/Upper Bound

```java
// Find smallest index where arr[i] >= target
public int lowerBound(int[] arr, int target) {
    int left = 0, right = arr.length;
    while (left < right) {
        int mid = (left + right) >>> 1;
        if (arr[mid] < target) left = mid + 1;
        else right = mid;
    }
    return left;
}
```

### 3. Iterative Version (No Stack)

```java
public <T extends Comparable<T>> int findIterative(T[] array, T key) {
    int left = 0, right = array.length - 1;
    while (left <= right) {
        int mid = (left + right) >>> 1;
        int cmp = key.compareTo(array[mid]);
        if (cmp == 0) return mid;
        if (cmp < 0) right = mid - 1;
        else left = mid + 1;
    }
    return -1;
}
```

---

## 🌍 Real-World Applications

| Application | Use Case |
|-------------|----------|
| **Database indexing** | B-tree search operations |
| **Version control** | Git bisect for bug finding |
| **Libraries** | `Arrays.binarySearch()`, `Collections.binarySearch()` |
| **Autocomplete** | Prefix matching in sorted dictionaries |
| **IP routing** | Longest prefix match |
| **Machine Learning** | Hyperparameter tuning (bisection) |

### Industry Usage

```java
// Java standard library
int index = Arrays.binarySearch(sortedArray, key);

// Collections
int index = Collections.binarySearch(sortedList, key);

// C++ STL
auto it = std::lower_bound(v.begin(), v.end(), key);
```

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/BinarySearchTest.java](../../src/test/java/com/thealgorithms/searches/BinarySearchTest.java)

### Test Cases

| Input | Key | Expected | Tests |
|-------|-----|----------|-------|
| `[]` | 5 | -1 | Empty array |
| `[5]` | 5 | 0 | Single element found |
| `[5]` | 3 | -1 | Single element not found |
| `[1,2,3,4,5]` | 1 | 0 | First element |
| `[1,2,3,4,5]` | 5 | 4 | Last element |
| `[1,2,3,4,5]` | 3 | 2 | Middle element |
| `[1,2,3,4,5]` | 6 | -1 | Not present |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 2.3
- Sedgewick, R. *"Algorithms"*, Section 3.1

### Famous Bug

Jon Bentley's Programming Pearls noted that most binary search implementations are buggy. The overflow bug (`(left + right) / 2`) wasn't fixed in Java's `Arrays.binarySearch()` until 2006!

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [IterativeBinarySearch](./iterative-binary-search.md) | Non-recursive variant |
| [InterpolationSearch](./interpolation-search.md) | Better for uniform distribution |
| [ExponentialSearch](./exponential-search.md) | Unbounded/infinite arrays |
| [TernarySearch](./ternary-search.md) | Finds maximum in unimodal function |

---

## 📊 Visualization

```
Search for 23 in [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]

Step 1: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
         L           M                    R
         0           4                    9
         array[4] = 16 < 23 → go right

Step 2: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                          L       M       R
                          5       7       9
         array[7] = 56 > 23 → go left

Step 3: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                          L   M   R
                          5   5   6
         array[5] = 23 = 23 → Found! ✅

Total comparisons: 3 (vs 6 for linear search)
```

---

[← Back to Searching Algorithms](../README.md) | [Next: LinearSearch →](./linear-search.md)
