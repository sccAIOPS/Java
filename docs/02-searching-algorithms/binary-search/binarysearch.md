# BinarySearch

> **Category:** Divide and Conquer Search  
> **Prerequisite:** Sorted Array  
> **Paradigm:** Divide and Conquer  
> **Approach:** Recursive (this implementation)

---

## 📋 Overview

Binary Search is one of the most fundamental and efficient searching algorithms. It works by repeatedly dividing the search space in half, eliminating half of the remaining elements with each comparison. This "divide and conquer" approach achieves **logarithmic time complexity**.

### Key Characteristics

| Property | Value |
|----------|-------|
| **Time (Best)** | O(1) |
| **Time (Average)** | O(log n) |
| **Time (Worst)** | O(log n) |
| **Space** | O(log n) recursive / O(1) iterative |
| **Prerequisite** | Sorted array |

---

## 🔬 Mathematical Analysis

### Algorithm Concept

Given a sorted array and a target value, Binary Search:
1. Compares the target with the middle element
2. If equal, return the index
3. If target is smaller, search the left half
4. If target is larger, search the right half
5. Repeat until found or search space is empty

### Recurrence Relation

$$
T(n) = T\left(\frac{n}{2}\right) + O(1)
$$

Using the Master Theorem (Case 2):
$$
T(n) = O(\log n)
$$

### Number of Comparisons

**Worst case:** $\lceil \log_2(n+1) \rceil$ comparisons

For an array of size $n$:
- 1 element: 1 comparison
- 2-3 elements: 2 comparisons
- 4-7 elements: 3 comparisons
- 1,000,000 elements: ~20 comparisons

### Search Space Reduction

After $k$ iterations, the search space is:
$$
\frac{n}{2^k}
$$

The algorithm terminates when $\frac{n}{2^k} \leq 1$, i.e., $k \geq \log_2 n$

---

## 📝 Pseudocode

```
BINARY-SEARCH(A, key)
    return SEARCH(A, key, 0, length(A) - 1)

SEARCH(A, key, left, right)
    if right < left then
        return -1  // Not found
    
    median ← (left + right) / 2  // Use >>> 1 to avoid overflow
    
    if key = A[median] then
        return median
    else if key < A[median] then
        return SEARCH(A, key, left, median - 1)
    else
        return SEARCH(A, key, median + 1, right)
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
 * <p>
 * Worst-case performance O(log n) Best-case performance O(1) Average
 * performance O(log n) Worst-case space complexity O(1)
 *
 * @author Varun Upadhyay (https://github.com/varunu28)
 * @author Podshivalov Nikita (https://github.com/nikitap492)
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
| **Recursive** | Elegant divide-and-conquer implementation |
| **Overflow-safe** | Uses `>>> 1` instead of `/ 2` |
| **SearchAlgorithm** | Implements standard interface |

---

## 🎯 Step-by-Step Example

### Input: Array `[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]`, Key = `23`

**Initial:** left=0, right=9

```
[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
 0  1  2   3   4   5   6   7   8   9
 ↑                              ↑
left                          right
```

**Step 1:** median = (0 + 9) >>> 1 = 4
```
[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                 ↑
              median=4
              array[4]=16

23 > 16 → search right half
```

**Step 2:** left=5, right=9, median = (5 + 9) >>> 1 = 7
```
[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                      ↑       ↑
                    left    median=7
                            array[7]=56

23 < 56 → search left half
```

**Step 3:** left=5, right=6, median = (5 + 6) >>> 1 = 5
```
[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                  ↑   ↑
                left  right
                median=5
                array[5]=23

23 == 23 → FOUND at index 5 ✅
```

### Result: Index `5` (3 comparisons)

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When |
|------|------------|------|
| **Best** | O(1) | Target at middle |
| **Average** | O(log n) | Typical case |
| **Worst** | O(log n) | Target at boundary or not found |

### Space Complexity

| Implementation | Space |
|----------------|-------|
| **Recursive** | O(log n) - call stack |
| **Iterative** | O(1) |

### Comparison: Binary vs Linear Search

| Array Size | Linear Search | Binary Search |
|------------|---------------|---------------|
| 10 | 10 ops | 4 ops |
| 1,000 | 1,000 ops | 10 ops |
| 1,000,000 | 1,000,000 ops | 20 ops |
| 1,000,000,000 | 1,000,000,000 ops | 30 ops |

---

## ⚠️ Common Pitfalls

### 1. Integer Overflow in Midpoint Calculation

```java
// Wrong: can overflow for large left and right
int mid = (left + right) / 2;

// Correct: unsigned right shift prevents overflow
int mid = (left + right) >>> 1;

// Also correct: subtract first
int mid = left + (right - left) / 2;
```

### 2. Forgetting Array Must Be Sorted

```java
// Wrong: Binary search on unsorted array gives incorrect results
int[] unsorted = {5, 2, 8, 1, 9};
binarySearch(unsorted, 8);  // May return -1 even though 8 exists!

// Correct: Sort first or verify sorted
Arrays.sort(array);
binarySearch(array, key);
```

### 3. Off-by-One Errors

```java
// Wrong: may skip elements
search(array, key, left, median);  // Should be median - 1

// Wrong: may infinite loop
search(array, key, median, right);  // Should be median + 1
```

---

## 🔧 Variations

### 1. Iterative Binary Search

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

### 2. Find First Occurrence

```java
public int findFirst(int[] array, int key) {
    int result = -1, left = 0, right = array.length - 1;
    while (left <= right) {
        int mid = (left + right) >>> 1;
        if (array[mid] == key) {
            result = mid;
            right = mid - 1;  // Continue searching left
        } else if (array[mid] < key) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return result;
}
```

### 3. Lower Bound (First ≥ key)

```java
public int lowerBound(int[] array, int key) {
    int left = 0, right = array.length;
    while (left < right) {
        int mid = (left + right) >>> 1;
        if (array[mid] < key) left = mid + 1;
        else right = mid;
    }
    return left;
}
```

---

## 🌍 Real-World Applications

| Application | Why Binary Search? |
|-------------|-------------------|
| **Database indexing** | B-trees use binary search at each node |
| **Dictionary lookup** | O(log n) word lookup |
| **Git bisect** | Find bug-introducing commit |
| **IP routing** | Longest prefix match |
| **Version control** | Finding specific revisions |

### Industry Usage

| System | Usage |
|--------|-------|
| **Java** | `Arrays.binarySearch()`, `Collections.binarySearch()` |
| **C++** | `std::binary_search()`, `std::lower_bound()` |
| **Python** | `bisect` module |
| **Databases** | B-tree index lookups |

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/BinarySearchTest.java](../../src/test/java/com/thealgorithms/searches/BinarySearchTest.java)

### Test Cases

| Input Array | Key | Expected | Tests |
|-------------|-----|----------|-------|
| `[]` | 5 | -1 | Empty array |
| `[5]` | 5 | 0 | Single element found |
| `[5]` | 3 | -1 | Single element not found |
| `[1,3,5,7,9]` | 1 | 0 | First element |
| `[1,3,5,7,9]` | 9 | 4 | Last element |
| `[1,3,5,7,9]` | 5 | 2 | Middle element |
| `[1,3,5,7,9]` | 4 | -1 | Not found |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 2.3
- Sedgewick, R. *"Algorithms"*, Section 3.1

### Historical Note

Binary Search was first published by John Mauchly in 1946, but the first bug-free implementation wasn't published until 1962. Even Jon Bentley's "Programming Pearls" (1986) had a bug!

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [IterativeBinarySearch](./iterative-binary-search.md) | Iterative variant |
| [InterpolationSearch](./interpolationsearch.md) | Estimates position |
| [ExponentialSearch](./exponential-search.md) | For unbounded arrays |
| [TernarySearch](./ternary-search.md) | Divides into thirds |

---

## 📊 Visualization

```
Searching for 23 in [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]

Step 1:
┌───┬───┬───┬────┬────┬────┬────┬────┬────┬────┐
│ 2 │ 5 │ 8 │ 12 │ 16 │ 23 │ 38 │ 56 │ 72 │ 91 │
└───┴───┴───┴────┴────┴────┴────┴────┴────┴────┘
  L                   M                       R
                      ↑
               16 < 23, go right

Step 2:
┌───┬───┬───┬────┬────┬────┬────┬────┬────┬────┐
│   │   │   │    │    │ 23 │ 38 │ 56 │ 72 │ 91 │
└───┴───┴───┴────┴────┴────┴────┴────┴────┴────┘
                       L         M            R
                                 ↑
                          56 > 23, go left

Step 3:
┌───┬───┬───┬────┬────┬────┬────┬────┬────┬────┐
│   │   │   │    │    │ 23 │ 38 │    │    │    │
└───┴───┴───┴────┴────┴────┴────┴────┴────┴────┘
                       L=M   R
                        ↑
                   23 == 23, FOUND! ✅
```

---

[← Back to Searching Algorithms](../README.md) | [Next: LinearSearch →](./linearsearch.md)
