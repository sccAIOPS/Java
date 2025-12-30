# Segment Tree

> **Category:** Data Structures  
> **Subcategory:** Trees  
> **Implementation:** [SegmentTree.java](../../../src/main/java/com/thealgorithms/datastructures/trees/SegmentTree.java)

---

## 📚 Overview

A **Segment Tree** is a tree data structure used for storing information about intervals (segments). It allows efficient querying of cumulative data (sum, minimum, maximum, GCD, etc.) over a range of elements and supports efficient point or range updates.

Segment trees are particularly useful when you have an array and need to answer multiple range queries while also supporting modifications to the array elements.

---

## 🔢 Mathematical Foundation

### Definition

For an array $A[0..n-1]$, a segment tree is a binary tree where:
- Leaf nodes store individual array elements
- Internal nodes store aggregate information (sum, min, max, etc.) of their children
- The root stores aggregate of entire array

### Key Properties

- **Complete Binary Tree:** Almost complete, stored in array
- **Height:** $O(\log n)$
- **Node Count:** $\leq 4n$ (often $2n - 1$ or $4n$ for safety)
- **Interval Division:** Each node covers interval $[l, r]$; children cover $[l, mid]$ and $[mid+1, r]$

### Mathematical Formulation

For node at index $i$ covering range $[l, r]$:
- **Left child:** index $2i + 1$, range $[l, \lfloor(l+r)/2\rfloor]$
- **Right child:** index $2i + 2$, range $[\lfloor(l+r)/2\rfloor + 1, r]$
- **Value:** $tree[i] = \text{combine}(tree[2i+1], tree[2i+2])$

---

## 📊 Complexity Analysis

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| **Build** | $O(n)$ | One-time construction |
| **Point Update** | $O(\log n)$ | Update single element |
| **Range Query** | $O(\log n)$ | Query any range |
| **Range Update** | $O(\log n)$ | With lazy propagation |
| **Space** | $O(n)$ | Typically $4n$ array |

### Why Segment Trees?

| Approach | Build | Point Update | Range Query |
|----------|-------|--------------|-------------|
| Naive Array | $O(n)$ | $O(1)$ | $O(n)$ |
| Prefix Sum | $O(n)$ | $O(n)$ | $O(1)$ |
| Segment Tree | $O(n)$ | $O(\log n)$ | $O(\log n)$ |
| Fenwick Tree | $O(n)$ | $O(\log n)$ | $O(\log n)$ |

---

## 🔄 Structure Visualization

```
Array: [1, 3, 5, 7, 9, 11]

Segment Tree (Sum):
                    [36]                    // sum of [0,5]
                   /    \
            [9]            [27]             // [0,2] and [3,5]
           /   \          /    \
        [4]    [5]     [16]    [11]         // [0,1], [2,2], [3,4], [5,5]
       /   \          /    \
     [1]   [3]      [7]    [9]              // leaves: individual elements
```

Array representation: `[36, 9, 27, 4, 5, 16, 11, 1, 3, _, _, 7, 9, _, _]`

---

## 🔄 Algorithm (Pseudocode)

### Build

```
ALGORITHM BuildTree(arr, tree, node, start, end)
    INPUT: Original array, tree array, current node index, range [start, end]
    OUTPUT: Segment tree built in tree array
    
    1. IF start = end THEN
    2.     tree[node] ← arr[start]
    3.     RETURN
    4. END IF
    
    5. mid ← (start + end) / 2
    6. BuildTree(arr, tree, 2*node+1, start, mid)      // Left child
    7. BuildTree(arr, tree, 2*node+2, mid+1, end)      // Right child
    8. tree[node] ← tree[2*node+1] + tree[2*node+2]    // Combine
```

### Point Update

```
ALGORITHM Update(tree, node, start, end, idx, val)
    INPUT: Tree array, current node, range, index to update, new value
    OUTPUT: Updated tree
    
    1. IF start = end THEN
    2.     tree[node] ← val
    3.     RETURN
    4. END IF
    
    5. mid ← (start + end) / 2
    6. IF idx ≤ mid THEN
    7.     Update(tree, 2*node+1, start, mid, idx, val)
    8. ELSE
    9.     Update(tree, 2*node+2, mid+1, end, idx, val)
    10. END IF
    
    11. tree[node] ← tree[2*node+1] + tree[2*node+2]  // Recombine
```

### Range Query

```
ALGORITHM Query(tree, node, start, end, l, r)
    INPUT: Tree array, current node, node range, query range [l, r]
    OUTPUT: Aggregate value for range [l, r]
    
    // No overlap
    1. IF r < start OR end < l THEN
    2.     RETURN 0  // Identity element for sum
    3. END IF
    
    // Complete overlap
    4. IF l ≤ start AND end ≤ r THEN
    5.     RETURN tree[node]
    6. END IF
    
    // Partial overlap
    7. mid ← (start + end) / 2
    8. leftSum ← Query(tree, 2*node+1, start, mid, l, r)
    9. rightSum ← Query(tree, 2*node+2, mid+1, end, l, r)
    10. RETURN leftSum + rightSum
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Array-based storage (index 0 is root)
- Recursive construction and updates
- Supports range sum queries

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/trees/SegmentTree.java`

```java
public class SegmentTree {
    private int[] tree;
    private int n;
    
    public SegmentTree(int[] arr) {
        n = arr.length;
        tree = new int[4 * n];  // Safe size
        constructTree(arr, 0, 0, n - 1);
    }
    
    // Build segment tree
    private void constructTree(int[] arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
            return;
        }
        
        int mid = (start + end) / 2;
        constructTree(arr, 2 * node + 1, start, mid);
        constructTree(arr, 2 * node + 2, mid + 1, end);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }
    
    // Point update
    public void update(int idx, int val) {
        updateTree(0, 0, n - 1, idx, val);
    }
    
    private void updateTree(int node, int start, int end, int idx, int val) {
        if (start == end) {
            tree[node] = val;
            return;
        }
        
        int mid = (start + end) / 2;
        if (idx <= mid) {
            updateTree(2 * node + 1, start, mid, idx, val);
        } else {
            updateTree(2 * node + 2, mid + 1, end, idx, val);
        }
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }
    
    // Range sum query
    public int getSum(int l, int r) {
        return getSumTree(0, 0, n - 1, l, r);
    }
    
    private int getSumTree(int node, int start, int end, int l, int r) {
        if (r < start || end < l) {
            return 0;  // No overlap
        }
        if (l <= start && end <= r) {
            return tree[node];  // Complete overlap
        }
        
        int mid = (start + end) / 2;
        int leftSum = getSumTree(2 * node + 1, start, mid, l, r);
        int rightSum = getSumTree(2 * node + 2, mid + 1, end, l, r);
        return leftSum + rightSum;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Range Minimum/Maximum Queries (RMQ)
**Use Case:** Finding min/max in a range with updates  
**Example:** Stock price analysis over time periods

### 2. Computational Geometry
**Use Case:** Counting points in rectangles  
**Example:** Geographic information systems (GIS)

### 3. Competitive Programming
**Use Case:** Range queries with updates  
**Example:** Solving problems on Codeforces, LeetCode

### 4. Database Systems
**Use Case:** Range aggregation queries  
**Example:** Analytics dashboards with time-range filters

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Financial Systems | Range statistics on time series |
| Gaming | Leaderboard range queries |
| GIS Systems | Spatial range queries |
| Analytics Platforms | Time-series aggregations |
| Ad Tech | Impression/click counting in intervals |

---

## ⚖️ Comparison with Related Data Structures

| Aspect | Segment Tree | Fenwick Tree | Sparse Table | Sqrt Decomposition |
|--------|-------------|--------------|--------------|-------------------|
| Build Time | $O(n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ |
| Query Time | $O(\log n)$ | $O(\log n)$ | $O(1)$ | $O(\sqrt{n})$ |
| Update Time | $O(\log n)$ | $O(\log n)$ | N/A | $O(\sqrt{n})$ |
| Space | $O(n)$ | $O(n)$ | $O(n \log n)$ | $O(n)$ |
| Range Update | ✅ (lazy) | Limited | ❌ | ✅ |
| Complexity | Moderate | Simple | Simple | Simple |

### When to Choose Segment Tree

✅ **Use Segment Tree when:**
- Need both range queries AND point/range updates
- Query operation is associative (sum, min, max, GCD, etc.)
- Have sufficient memory for tree structure

❌ **Don't use when:**
- Only need prefix queries (use Fenwick Tree)
- No updates needed (use Sparse Table for $O(1)$ queries)
- Memory is very constrained

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Array Size:** Use $4n$ size to be safe (not $2n$)
2. **Index Bounds:** Off-by-one errors in range boundaries
3. **Identity Element:** Choose correct identity (0 for sum, $\infty$ for min)
4. **Integer Overflow:** Sum of large ranges may overflow

### Edge Cases to Handle

- [x] Single element array
- [x] Query entire array
- [x] Query single element
- [x] Update to same value
- [x] Empty range query

### Common Bug: Wrong Tree Size

```java
// BUG: Tree may not be large enough
int[] tree = new int[2 * n];

// CORRECT: Use 4n for safety
int[] tree = new int[4 * n];

// WHY: For n = 3, tree needs indices 0-6 (7 nodes)
// But 2*3 = 6 (only indices 0-5)
```

---

## 🔄 Advanced: Lazy Propagation

For efficient range updates:

```java
// Lazy propagation arrays
int[] tree, lazy;

void pushDown(int node) {
    if (lazy[node] != 0) {
        tree[2*node+1] += lazy[node];
        tree[2*node+2] += lazy[node];
        lazy[2*node+1] += lazy[node];
        lazy[2*node+2] += lazy[node];
        lazy[node] = 0;
    }
}

void rangeUpdate(int node, int start, int end, int l, int r, int val) {
    if (r < start || end < l) return;
    if (l <= start && end <= r) {
        tree[node] += val;
        lazy[node] += val;
        return;
    }
    pushDown(node);
    int mid = (start + end) / 2;
    rangeUpdate(2*node+1, start, mid, l, r, val);
    rangeUpdate(2*node+2, mid+1, end, l, r, val);
    // Note: For sum, need to track count of elements
}
```

---

## 📖 References

1. Bentley, J. L. (1977). "Solutions to Klee's rectangle problems". Unpublished manuscript.
2. de Berg, M., et al. (2008). *Computational Geometry: Algorithms and Applications* (3rd ed.). Springer.
3. [CP-Algorithms: Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
4. [Wikipedia: Segment tree](https://en.wikipedia.org/wiki/Segment_tree)

---

## 🔗 Related Algorithms

- [Fenwick Tree](fenwick-tree.md) - Simpler, same complexity
- [Lazy Segment Tree](../../../src/main/java/com/thealgorithms/datastructures/trees/LazySegmentTree.java) - Range updates
- [Sparse Table](sparse-table.md) - $O(1)$ queries, no updates
- [Sqrt Decomposition](sqrt-decomposition.md) - Simpler alternative
