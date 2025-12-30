# Fenwick Tree (Binary Indexed Tree)

> **Category:** Data Structures  
> **Subcategory:** Trees  
> **Implementation:** [FenwickTree.java](../../../src/main/java/com/thealgorithms/datastructures/trees/FenwickTree.java)

---

## 📚 Overview

A **Fenwick Tree** (also known as **Binary Indexed Tree** or **BIT**) is a data structure that efficiently supports point updates and prefix sum queries. Invented by Peter Fenwick in 1994, it provides $O(\log n)$ time complexity for both operations while using only $O(n)$ space.

The key insight of Fenwick Trees is using binary representation of indices to determine which elements to combine, creating an elegant and memory-efficient structure.

---

## 🔢 Mathematical Foundation

### Definition

For an array $A[1..n]$, a Fenwick Tree maintains an array $BIT[1..n]$ where:
$$BIT[i] = \sum_{j=i-LSB(i)+1}^{i} A[j]$$

Where $LSB(i)$ is the **Least Significant Bit** of $i$ (the rightmost 1-bit).

### Key Properties

- **LSB Extraction:** $LSB(i) = i \land (-i)$ (bitwise AND with two's complement)
- **Parent Finding:** Parent of $i$ is $i - LSB(i)$ for queries
- **Next Update:** Next index to update is $i + LSB(i)$
- **1-Indexed:** Typically uses 1-based indexing

### Mathematical Formulation

**Prefix Sum:**
$$\text{sum}(i) = \sum_{j=1}^{i} A[j] = \sum_{\text{ancestors}} BIT[k]$$

**Range Sum:**
$$\text{sum}(l, r) = \text{sum}(r) - \text{sum}(l-1)$$

**Update at index $i$:**
$$BIT[j] \mathrel{+}= \text{delta} \quad \forall j \in \{i, i+LSB(i), ...\}$$

---

## 📊 Complexity Analysis

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| **Build** | $O(n)$ | Using efficient construction |
| **Point Update** | $O(\log n)$ | Update single element |
| **Prefix Sum** | $O(\log n)$ | Sum from index 1 to i |
| **Range Sum** | $O(\log n)$ | Two prefix sum queries |
| **Space** | $O(n)$ | Same as input array |

### Comparison with Segment Tree

| Aspect | Fenwick Tree | Segment Tree |
|--------|--------------|--------------|
| Space | $n$ | $2n$ to $4n$ |
| Constants | Smaller | Larger |
| Implementation | Simpler | More complex |
| Range Updates | Limited | ✅ (with lazy propagation) |
| Range Queries | Prefix-based | Any range |

---

## 🔄 Structure Visualization

```
Array A:   [_, 1, 3, 2, 5, 1, 4, 2, 8]  (1-indexed)
BIT:       [_, 1, 4, 2, 10, 1, 5, 2, 28]

Index (binary):   001  010  011  100  101  110  111  1000
Responsibility:   [1]  [1-2] [3] [1-4] [5] [5-6] [7] [1-8]

Tree Structure:
                     BIT[8]=28
                    /
              BIT[4]=10
             /         \
       BIT[2]=4      BIT[6]=5
       /    \           \
    BIT[1]=1 BIT[3]=2  BIT[5]=1  BIT[7]=2
```

**Query sum(7):** BIT[7] + BIT[6] + BIT[4] = 2 + 5 + 10 = 17 ✓  
**Path:** 7 (111) → 6 (110) → 4 (100) → 0 (done)

---

## 🔄 Algorithm (Pseudocode)

### Update

```
ALGORITHM Update(BIT, n, i, delta)
    INPUT: BIT array, size n, index i, value to add
    OUTPUT: Updated BIT
    
    1. WHILE i ≤ n DO
    2.     BIT[i] ← BIT[i] + delta
    3.     i ← i + (i AND -i)    // i + LSB(i)
    4. END WHILE
```

### Prefix Sum Query

```
ALGORITHM Query(BIT, i)
    INPUT: BIT array, index i
    OUTPUT: Sum of elements from 1 to i
    
    1. sum ← 0
    2. WHILE i > 0 DO
    3.     sum ← sum + BIT[i]
    4.     i ← i - (i AND -i)    // i - LSB(i)
    5. END WHILE
    6. RETURN sum
```

### Range Sum Query

```
ALGORITHM RangeQuery(BIT, l, r)
    INPUT: BIT array, range [l, r]
    OUTPUT: Sum of elements from l to r
    
    1. RETURN Query(BIT, r) - Query(BIT, l - 1)
```

### Build (Efficient O(n))

```
ALGORITHM Build(A, n)
    INPUT: Array A, size n
    OUTPUT: BIT array
    
    1. BIT ← copy of A
    2. FOR i ← 1 TO n DO
    3.     j ← i + (i AND -i)
    4.     IF j ≤ n THEN
    5.         BIT[j] ← BIT[j] + BIT[i]
    6.     END IF
    7. END FOR
    8. RETURN BIT
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- 1-based indexing (index 0 unused)
- Compact implementation with bit manipulation
- Simple update and query methods

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/trees/FenwickTree.java`

```java
public class FenwickTree {
    private int[] tree;
    private int n;
    
    public FenwickTree(int n) {
        this.n = n;
        tree = new int[n + 1];  // 1-indexed
    }
    
    // Update: add delta to index i
    public void update(int i, int delta) {
        while (i <= n) {
            tree[i] += delta;
            i += i & (-i);  // i + LSB(i)
        }
    }
    
    // Query: sum from 1 to i
    public int query(int i) {
        int sum = 0;
        while (i > 0) {
            sum += tree[i];
            i -= i & (-i);  // i - LSB(i)
        }
        return sum;
    }
    
    // Range query: sum from l to r
    public int rangeQuery(int l, int r) {
        return query(r) - query(l - 1);
    }
}
```

### Building from Existing Array

```java
// Method 1: O(n log n) - Simple but slower
public FenwickTree(int[] arr) {
    n = arr.length;
    tree = new int[n + 1];
    for (int i = 0; i < n; i++) {
        update(i + 1, arr[i]);
    }
}

// Method 2: O(n) - Efficient
public FenwickTree(int[] arr) {
    n = arr.length;
    tree = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        tree[i] = arr[i - 1];
    }
    for (int i = 1; i <= n; i++) {
        int j = i + (i & -i);
        if (j <= n) {
            tree[j] += tree[i];
        }
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Cumulative Frequency Tables
**Use Case:** Dynamic frequency counting  
**Example:** Real-time histogram updates

### 2. Inversion Count
**Use Case:** Counting inversions in array  
**Example:** Measuring array "sortedness"

### 3. Range Sum with Updates
**Use Case:** Financial transaction sums  
**Example:** Running balance calculations

### 4. Competitive Programming
**Use Case:** Efficient range queries  
**Example:** Online judges (LeetCode, Codeforces)

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Financial Systems | Running totals, balances |
| Gaming | Leaderboard updates |
| Analytics | Cumulative metrics |
| Databases | Approximate counts |
| Text Editors | Character counting |

---

## ⚖️ Comparison with Related Data Structures

| Aspect | Fenwick Tree | Segment Tree | Prefix Sum Array |
|--------|--------------|--------------|------------------|
| Build | $O(n)$ | $O(n)$ | $O(n)$ |
| Point Update | $O(\log n)$ | $O(\log n)$ | $O(n)$ |
| Prefix Query | $O(\log n)$ | $O(\log n)$ | $O(1)$ |
| Range Query | $O(\log n)$ | $O(\log n)$ | $O(1)$ |
| Space | $O(n)$ | $O(n)$ to $O(4n)$ | $O(n)$ |
| Range Update | Complex | ✅ | $O(1)$ end points |

### When to Choose Fenwick Tree

✅ **Use Fenwick Tree when:**
- Need prefix/range sum queries with point updates
- Memory efficiency is important
- Implementation simplicity is valued
- Updates and queries are roughly balanced

❌ **Don't use when:**
- Need range updates (use Segment Tree with lazy propagation)
- Need non-prefix range queries on non-invertible operations (min, max)
- Array is static (use prefix sum array)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **0-Based vs 1-Based:** Fenwick Tree is naturally 1-indexed
2. **LSB Calculation:** Ensure correct bit manipulation
3. **Negative Numbers:** Works correctly with negative values
4. **Integer Overflow:** Sum may overflow for large arrays

### Edge Cases to Handle

- [x] Single element array
- [x] Query at index 0 (should return 0)
- [x] Update at boundary indices
- [x] Query entire array
- [x] Negative delta values

### Common Bug: 0-Based Indexing

```java
// BUG: Using 0-based indexing
public void update(int i, int delta) {
    while (i < n) {  // Wrong: 0-indexed
        tree[i] += delta;
        i += i & (-i);  // LSB(0) = 0, infinite loop!
    }
}

// CORRECT: Use 1-based indexing
public void update(int i, int delta) {
    i++;  // Convert to 1-based
    while (i <= n) {
        tree[i] += delta;
        i += i & (-i);
    }
}
```

---

## 🔄 Extensions

### 2D Fenwick Tree

```java
class FenwickTree2D {
    int[][] tree;
    int n, m;
    
    void update(int x, int y, int delta) {
        for (int i = x; i <= n; i += i & (-i))
            for (int j = y; j <= m; j += j & (-j))
                tree[i][j] += delta;
    }
    
    int query(int x, int y) {
        int sum = 0;
        for (int i = x; i > 0; i -= i & (-i))
            for (int j = y; j > 0; j -= j & (-j))
                sum += tree[i][j];
        return sum;
    }
}
```

### Range Update, Point Query

```java
// Use difference array concept
void rangeUpdate(int l, int r, int delta) {
    update(l, delta);
    update(r + 1, -delta);
}

int pointQuery(int i) {
    return query(i);  // Returns actual value at i
}
```

---

## 📖 References

1. Fenwick, P. M. (1994). "A new data structure for cumulative frequency tables". *Software: Practice and Experience*. 24 (3): 327–336.
2. [TopCoder: Binary Indexed Trees](https://www.topcoder.com/thrive/articles/Binary%20Indexed%20Trees)
3. [CP-Algorithms: Fenwick Tree](https://cp-algorithms.com/data_structures/fenwick.html)
4. [Wikipedia: Fenwick tree](https://en.wikipedia.org/wiki/Fenwick_tree)

---

## 🔗 Related Algorithms

- [Segment Tree](segment-tree.md) - More flexible, more memory
- [Prefix Sum Array](../../03-dynamic-programming/prefix-sum.md) - Static version
- [Sparse Table](sparse-table.md) - For idempotent operations
- [2D Fenwick Tree](fenwick-tree-2d.md) - Extension to 2D
