# Counting Inversions

> **Category:** Divide and Conquer  
> **Subcategory:** Counting / Sorting  
> **Implementation:** [`CountingInversions.java`](../../src/main/java/com/thealgorithms/divideandconquer/CountingInversions.java)

---

## 📚 Overview

Counting Inversions is a classic problem that counts the number of pairs (i, j) where i < j but arr[i] > arr[j]. This count measures how "unsorted" an array is. While a naive approach takes O(n²), a modified merge sort solves it in O(n log n) by counting inversions during the merge step.

**Key Characteristics:**
- Measures "distance" from sorted order
- Modified merge sort achieves O(n log n)
- Applications in ranking similarity, recommendation systems
- Equivalent to counting swaps in bubble sort

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** For an array $A[1..n]$, an inversion is a pair $(i, j)$ such that $i < j$ and $A[i] > A[j]$. The inversion count is the total number of such pairs.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Minimum Inversions | Sorted array | 0 |
| Maximum Inversions | Reverse sorted | $\frac{n(n-1)}{2}$ |
| Split Inversions | Across merge boundary | Counted during merge |
| Kendall Tau Distance | Normalized inversion count | $\tau = 1 - \frac{4 \cdot \text{inv}}{n(n-1)}$ |

### Mathematical Formulation

**Inversion Count:**

$$
\text{inv}(A) = |\{(i, j) : 1 \leq i < j \leq n \text{ and } A[i] > A[j]\}|
$$

**Bounds:**

$$
0 \leq \text{inv}(A) \leq \binom{n}{2} = \frac{n(n-1)}{2}
$$

**Divide and Conquer Decomposition:**

$$
\text{inv}(A) = \text{inv}(A_L) + \text{inv}(A_R) + \text{split\_inv}(A_L, A_R)
$$

Where:
- $A_L$ = left half of array
- $A_R$ = right half of array
- split_inv = inversions with one element in each half

### Proof of Correctness

**Lemma:** During merge, when we copy an element from the right half, it forms inversions with all remaining elements in the left half.

**Proof:**
1. Let $L[i..m]$ be remaining left elements, $R[j]$ be the element being copied from right.
2. Since both halves are sorted and $R[j] < L[i]$, we have $R[j] < L[i], L[i+1], ..., L[m]$.
3. Each of these is an inversion since all left elements came before $R[j]$ originally.
4. Number of inversions = $m - i + 1$ (remaining left elements). ∎

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **All Cases** | $O(n \log n)$ | Same as merge sort |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary** | $O(n)$ | Temporary merge array |
| **Stack Depth** | $O(\log n)$ | Recursion |

### Additional Properties

| Property | Value |
|----------|-------|
| **Stable** | Yes (merge sort property) |
| **In-place** | No (needs temp array) |
| **Comparison-based** | Yes |

### Detailed Analysis

**Recurrence Relation:**

$$
T(n) = 2T(n/2) + O(n)
$$

By Master Theorem: $T(n) = O(n \log n)$

**Work Distribution:**
- Recursive calls: O(n log n)
- Each merge: O(n)
- Inversion counting during merge: O(1) per comparison

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Count-Inversions(A)
─────────────────────────────────────────────────────
    INPUT:  Array A[0..n-1]
    OUTPUT: Number of inversions and sorted array
─────────────────────────────────────────────────────

    1. RETURN Merge-Sort-Count(A, 0, n - 1)


ALGORITHM Merge-Sort-Count(A, left, right)
─────────────────────────────────────────────────────

    1. IF left ≥ right THEN
    2.     RETURN 0
    3. END IF
    
    4. mid ← (left + right) / 2
    
    5. // Recursively count inversions in both halves
    6. leftInv ← Merge-Sort-Count(A, left, mid)
    7. rightInv ← Merge-Sort-Count(A, mid + 1, right)
    
    8. // Count split inversions during merge
    9. splitInv ← Merge-Count(A, left, mid, right)
    
    10. RETURN leftInv + rightInv + splitInv


ALGORITHM Merge-Count(A, left, mid, right)
─────────────────────────────────────────────────────

    1. n1 ← mid - left + 1
    2. n2 ← right - mid
    3. L ← A[left..mid]        // copy left half
    4. R ← A[mid+1..right]     // copy right half
    
    5. i ← 0, j ← 0, k ← left
    6. inversions ← 0
    
    7. WHILE i < n1 AND j < n2 DO
    8.     IF L[i] ≤ R[j] THEN
    9.         A[k] ← L[i]
    10.        i ← i + 1
    11.    ELSE
    12.        A[k] ← R[j]
    13.        inversions ← inversions + (n1 - i)  // KEY STEP
    14.        j ← j + 1
    15.    END IF
    16.    k ← k + 1
    17. END WHILE
    
    18. // Copy remaining elements
    19. WHILE i < n1 DO
    20.    A[k] ← L[i]
    21.    i ← i + 1
    22.    k ← k + 1
    23. END WHILE
    
    24. WHILE j < n2 DO
    25.    A[k] ← R[j]
    26.    j ← j + 1
    27.    k ← k + 1
    28. END WHILE
    
    29. RETURN inversions
```

### Step-by-Step Walkthrough

**Example Input:** A = [8, 4, 2, 1]

**Recursive Structure:**
```
Count([8, 4, 2, 1])
├── Count([8, 4])           → 1 inversion (8 > 4)
│   ├── Count([8])          → 0
│   └── Count([4])          → 0
│   └── Merge([4, 8])       → split_inv = 1
├── Count([2, 1])           → 1 inversion (2 > 1)
│   ├── Count([2])          → 0
│   └── Count([1])          → 0
│   └── Merge([1, 2])       → split_inv = 1
└── Merge([1, 2], [4, 8])   → split_inv = 4
```

**Merge Step Details ([1, 2] with [4, 8]):**

| Step | L[i] | R[j] | Action | Inversions Added |
|------|------|------|--------|------------------|
| 1 | 4 | 1 | Copy 1 from R | 2 (both 4, 8 > 1) |
| 2 | 4 | 2 | Copy 2 from R | 2 (both 4, 8 > 2) |
| 3 | 4 | - | Copy 4 from L | 0 |
| 4 | 8 | - | Copy 8 from L | 0 |

**Total Inversions:** 1 + 1 + 4 = **6**

**Verification by Enumeration:**
| Pair (i,j) | A[i] > A[j]? | Inversion? |
|------------|--------------|------------|
| (0,1): 8,4 | Yes | ✓ |
| (0,2): 8,2 | Yes | ✓ |
| (0,3): 8,1 | Yes | ✓ |
| (1,2): 4,2 | Yes | ✓ |
| (1,3): 4,1 | Yes | ✓ |
| (2,3): 2,1 | Yes | ✓ |

**Total: 6 inversions** ✓

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Piggybacks on merge sort algorithm
- Returns count while sorting the array
- Uses temporary array for merging
- Key insight: counting happens during right-half element copy

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/divideandconquer/CountingInversions.java`

```java
// Key code snippet - Counting during merge
public static int countInversions(int[] arr) {
    int[] temp = new int[arr.length];
    return mergeSortAndCount(arr, temp, 0, arr.length - 1);
}

private static int mergeSortAndCount(int[] arr, int[] temp, int left, int right) {
    int count = 0;
    if (left < right) {
        int mid = (left + right) / 2;
        count += mergeSortAndCount(arr, temp, left, mid);
        count += mergeSortAndCount(arr, temp, mid + 1, right);
        count += mergeAndCount(arr, temp, left, mid, right);
    }
    return count;
}

private static int mergeAndCount(int[] arr, int[] temp, int left, int mid, int right) {
    int i = left, j = mid + 1, k = left;
    int inversions = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
            inversions += (mid - i + 1);  // KEY: count inversions
        }
    }
    // ... copy remaining elements ...
    return inversions;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Recommendation Systems
**Use Case:** Measuring similarity between user rankings  
**Example:** Netflix comparing user movie preferences

### 2. Voting Systems
**Use Case:** Comparing voting results (Kendall tau distance)  
**Example:** Analyzing ballot preferences

### 3. Sorting Algorithm Analysis
**Use Case:** Measuring initial disorder of data  
**Example:** Choosing optimal sorting algorithm based on disorder

### 4. Collaborative Filtering
**Use Case:** Finding users with similar tastes  
**Example:** Amazon product recommendations

### 5. Gene Sequencing
**Use Case:** Comparing gene orderings across species  
**Example:** Evolutionary biology analysis

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Netflix | User preference similarity |
| Spotify | Playlist similarity |
| Amazon | Collaborative filtering |
| Google Search | Ranking quality measurement |
| Election systems | Ballot analysis |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Merge Sort Counting | Naive O(n²) | BIT/Fenwick | AVL Tree |
|--------|---------------------|-------------|-------------|----------|
| Time Complexity | O(n log n) | O(n²) | O(n log n) | O(n log n) |
| Space Complexity | O(n) | O(1) | O(n) | O(n) |
| Modifies Array | Yes (sorts) | No | No | No |
| Online | No | Yes | Yes | Yes |
| Implementation | Simple | Trivial | Moderate | Complex |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Off-by-One Errors:** Careful with index calculations in merge
2. **Integer Overflow:** Large arrays can have up to n(n-1)/2 inversions
3. **Stable Counting:** Use ≤ not < for equal elements
4. **Array Modification:** Original array gets sorted

### Edge Cases to Handle
- [ ] Empty array (0 inversions)
- [ ] Single element (0 inversions)
- [ ] Already sorted (0 inversions)
- [ ] Reverse sorted (maximum inversions)
- [ ] Duplicate elements
- [ ] Large arrays (overflow check)

---

## 📖 References

1. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
2. Kleinberg, J., Tardos, E. (2005). "Algorithm Design". Addison-Wesley.
3. [Wikipedia - Inversion (discrete mathematics)](https://en.wikipedia.org/wiki/Inversion_(discrete_mathematics))

---

## 🔗 Related Algorithms

- [Merge Sort](../01-sorting-algorithms/comparison-based/mergesort.md) - Base algorithm
- [Kendall Tau Distance](./kendall-tau.md) - Normalized version
- [Binary Indexed Tree](../04-data-structures/trees/fenwick-tree.md) - Alternative approach
- [Bubble Sort](../01-sorting-algorithms/comparison-based/bubblesort.md) - Inversions = swap count
