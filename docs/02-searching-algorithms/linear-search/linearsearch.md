# LinearSearch

> **Category:** Sequential Search  
> **Prerequisite:** None  
> **Paradigm:** Brute Force  
> **Array Order:** Works on both sorted and unsorted arrays

---

## 📋 Overview

Linear Search (also called Sequential Search) is the simplest searching algorithm. It sequentially checks each element of the list until a match is found or all elements have been checked. While not efficient for large datasets, it's often the best choice for small arrays or unsorted data.

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

Linear Search simply iterates through the array from the first element to the last, comparing each element with the target value.

### Expected Number of Comparisons

**If element exists (probability $p$):**

For an element at position $i$ (1-indexed), probability of being at each position is $\frac{1}{n}$:

$$
E[\text{comparisons}] = \sum_{i=1}^{n} i \cdot \frac{1}{n} = \frac{1}{n} \cdot \frac{n(n+1)}{2} = \frac{n+1}{2}
$$

**If element might not exist:**

$$
E[\text{comparisons}] = p \cdot \frac{n+1}{2} + (1-p) \cdot n
$$

### Probability of Finding Element

If target appears with frequency $f$ in array of size $n$:

$$
P(\text{found}) = \frac{f}{n}
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

### With Early Exit for Sorted Arrays

```
SORTED-LINEAR-SEARCH(A, key)
    for i ← 0 to length(A) - 1 do
        if A[i] = key then
            return i
        if A[i] > key then
            return -1  // Can't be further right
    return -1
```

---

## 💻 Implementation

### Source File

**Location:** [src/main/java/com/thealgorithms/searches/LinearSearch.java](../../src/main/java/com/thealgorithms/searches/LinearSearch.java)

```java
/**
 * Linear search is the easiest search algorithm. It works with sorted and
 * unsorted arrays (a binary search works only with sorted array). This
 * algorithm just compares all elements of an array to find a value.
 *
 * <p>
 * Worst-case performance O(n) Best-case performance O(1) Average performance
 * O(n) Worst-case space complexity O(1)
 *
 * @author Varun Upadhyay (https://github.com/varunu28)
 * @author Podshivalov Nikita (https://github.com/nikitap492)
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
| **Universal** | Works on sorted and unsorted arrays |
| **SearchAlgorithm** | Implements standard interface |

---

## 🎯 Step-by-Step Example

### Input: Array `[5, 2, 9, 1, 7, 6, 3]`, Key = `7`

```
Step 1: Compare with index 0
[5, 2, 9, 1, 7, 6, 3]
 ↑
 5 ≠ 7 → continue

Step 2: Compare with index 1
[5, 2, 9, 1, 7, 6, 3]
    ↑
    2 ≠ 7 → continue

Step 3: Compare with index 2
[5, 2, 9, 1, 7, 6, 3]
       ↑
       9 ≠ 7 → continue

Step 4: Compare with index 3
[5, 2, 9, 1, 7, 6, 3]
          ↑
          1 ≠ 7 → continue

Step 5: Compare with index 4
[5, 2, 9, 1, 7, 6, 3]
             ↑
             7 == 7 → FOUND at index 4 ✅
```

### Result: Index `4` (5 comparisons)

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When |
|------|------------|------|
| **Best** | O(1) | Target at first position |
| **Average** | O(n) | Target at middle (n/2 comparisons) |
| **Worst** | O(n) | Target at end or not found |

### Space Complexity

| Type | Space |
|------|-------|
| **Auxiliary** | O(1) |

### Comparison Table

| Scenario | Linear Search | Binary Search |
|----------|---------------|---------------|
| Unsorted array | ✅ Works | ❌ Doesn't work |
| Sorted array | Works (inefficient) | ✅ Optimal |
| Small array (n < 10) | ✅ Preferred | Overkill |
| Single search | O(n) | O(log n) + O(n log n) sort |
| Multiple searches | ✅ If few searches | ✅ Amortized after sort |

---

## ⚠️ Common Pitfalls

### 1. Using Linear Search When Binary Search is Better

```java
// Wrong: O(n) for each search on sorted array
for (int key : queries) {
    linearSearch(sortedArray, key);  // Total: O(q × n)
}

// Correct: O(log n) for each search
for (int key : queries) {
    binarySearch(sortedArray, key);  // Total: O(q × log n)
}
```

### 2. Not Returning Early

```java
// Inefficient: continues even after finding
int found = -1;
for (int i = 0; i < array.length; i++) {
    if (array[i] == key) found = i;  // Doesn't return!
}
return found;

// Correct: returns immediately
for (int i = 0; i < array.length; i++) {
    if (array[i] == key) return i;  // Early exit
}
return -1;
```

### 3. Comparing Objects Incorrectly

```java
// Wrong: reference comparison
if (array[i] == key)

// Correct: value comparison
if (array[i].compareTo(key) == 0)
// or
if (array[i].equals(key))
```

---

## 🔧 Variations

### 1. Sentinel Linear Search

Place the key at the end to eliminate bounds checking:

```java
public int sentinelSearch(int[] array, int key) {
    int n = array.length;
    int last = array[n - 1];
    array[n - 1] = key;  // Place sentinel
    
    int i = 0;
    while (array[i] != key) {
        i++;
    }
    
    array[n - 1] = last;  // Restore
    
    if (i < n - 1 || array[n - 1] == key) {
        return i;
    }
    return -1;
}
```

### 2. Find All Occurrences

```java
public List<Integer> findAll(int[] array, int key) {
    List<Integer> indices = new ArrayList<>();
    for (int i = 0; i < array.length; i++) {
        if (array[i] == key) {
            indices.add(i);
        }
    }
    return indices;
}
```

### 3. Bidirectional Linear Search

Search from both ends simultaneously:

```java
public int bidirectionalSearch(int[] array, int key) {
    int left = 0, right = array.length - 1;
    while (left <= right) {
        if (array[left] == key) return left;
        if (array[right] == key) return right;
        left++;
        right--;
    }
    return -1;
}
```

---

## 🌍 Real-World Applications

| Application | Why Linear Search? |
|-------------|-------------------|
| **Small arrays** | Overhead of binary search not worth it |
| **Unsorted data** | No preprocessing required |
| **Single search** | Sorting takes O(n log n) |
| **Linked lists** | No random access anyway |
| **Finding any match** | May not need to search entire list |

### When Linear Search is Preferred

```
Decision Tree:

Is array sorted?
├── No → Use Linear Search
└── Yes
    └── How many searches?
        ├── One → Linear may be simpler
        └── Many → Use Binary Search
            └── Is array small (n < ~10)?
                ├── Yes → Linear is fine
                └── No → Binary Search
```

---

## 🧪 Testing

### Test File

**Location:** [src/test/java/com/thealgorithms/searches/LinearSearchTest.java](../../src/test/java/com/thealgorithms/searches/LinearSearchTest.java)

### Test Cases

| Input Array | Key | Expected | Tests |
|-------------|-----|----------|-------|
| `[]` | 5 | -1 | Empty array |
| `[5]` | 5 | 0 | Single element found |
| `[5]` | 3 | -1 | Single element not found |
| `[1,2,3,4,5]` | 1 | 0 | First element |
| `[1,2,3,4,5]` | 5 | 4 | Last element |
| `[1,2,3,4,5]` | 3 | 2 | Middle element |
| `[5,2,9,1,7]` | 7 | 4 | Unsorted array |

---

## 📖 References

### Textbooks

- Cormen, T.H., et al. *"Introduction to Algorithms"* (CLRS)
- Knuth, D.E. *"The Art of Computer Programming"*, Vol. 3

---

## 🔗 Related Algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [BinarySearch](./binarysearch.md) | O(log n) for sorted arrays |
| [SentinelLinearSearch](./sentinel-linear-search.md) | Optimized variant |
| [JumpSearch](./jumpsearch.md) | O(√n) compromise |
| [HashTable Lookup](../data-structures/hashtable.md) | O(1) average |

---

## 📊 Visualization

```
Searching for 7 in [5, 2, 9, 1, 7, 6, 3]

Step 1: [5, 2, 9, 1, 7, 6, 3]
         ↑
         5 ≠ 7

Step 2: [5, 2, 9, 1, 7, 6, 3]
            ↑
            2 ≠ 7

Step 3: [5, 2, 9, 1, 7, 6, 3]
               ↑
               9 ≠ 7

Step 4: [5, 2, 9, 1, 7, 6, 3]
                  ↑
                  1 ≠ 7

Step 5: [5, 2, 9, 1, 7, 6, 3]
                     ↑
                     7 = 7 ✅ FOUND at index 4

Comparisons: 5
```

---

## 💡 Optimization Tips

### For Frequently Searched Elements

If some elements are searched more often, use **Move-to-Front** heuristic:

```java
public int searchWithMTF(List<Integer> list, int key) {
    for (int i = 0; i < list.size(); i++) {
        if (list.get(i) == key) {
            // Move to front for faster future access
            list.remove(i);
            list.add(0, key);
            return 0;  // Now at index 0
        }
    }
    return -1;
}
```

### For Sorted Linked Lists

Use **Skip Lists** instead—they provide O(log n) average search time!

---

[← Back to Searching Algorithms](../README.md) | [Next: JumpSearch →](./jumpsearch.md)
