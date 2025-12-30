# B-Tree

> **Category:** Data Structures > Trees > Balanced Trees
> **Implementation:** [BTree.java](../../../src/main/java/com/thealgorithms/datastructures/trees/BTree.java)

## Overview

A **B-Tree** is a self-balancing search tree data structure that maintains sorted data and allows searches, sequential access, insertions, and deletions in logarithmic time. Unlike binary search trees, B-Trees can have more than two children per node.

B-Trees are particularly well-suited for storage systems that read and write large blocks of data, making them ideal for databases and file systems.

### Key Properties

1. **Order (t):** The minimum degree defining the range for number of keys
2. **Keys per Node:** Between `t-1` and `2t-1` keys (except root)
3. **Children per Node:** Between `t` and `2t` children for internal nodes
4. **Balanced Height:** All leaves appear at the same depth
5. **Sorted Keys:** Keys within a node are stored in sorted order

## Mathematical Foundation

### B-Tree Properties

For a B-Tree of minimum degree `t`:

| Property | Minimum | Maximum |
|----------|---------|---------|
| Keys in non-root node | t - 1 | 2t - 1 |
| Children in internal node | t | 2t |
| Keys in root | 1 | 2t - 1 |

### Height Bounds

For a B-Tree with `n` keys:

**Maximum Height:**
$$h \leq \log_t \frac{n + 1}{2}$$

**Minimum Height:**
$$h \geq \log_{2t} (n + 1) - 1$$

### Node Structure

A B-Tree node contains:
- An array of keys: `keys[0...n-1]`
- An array of child pointers: `children[0...n]`
- Current number of keys: `n`
- Boolean flag: `leaf`

For internal nodes, `children[i]` points to subtree with keys between `keys[i-1]` and `keys[i]`.

## Complexity Analysis

### Time Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(log n) | O(log n) |
| Insert | O(log n) | O(log n) |
| Delete | O(log n) | O(log n) |
| Traverse | O(n) | O(n) |

### Space Complexity

| Aspect | Complexity |
|--------|------------|
| Storage | O(n) |
| Operations | O(1) |
| Auxiliary (traversal) | O(h) = O(log n) |

### Comparison with Other Trees

| Tree Type | Height | Search | Insert | Delete | Disk Access |
|-----------|--------|--------|--------|--------|-------------|
| Binary Search Tree | O(n) | O(n) | O(n) | O(n) | High |
| AVL Tree | O(log n) | O(log n) | O(log n) | O(log n) | High |
| B-Tree | O(log_t n) | O(log n) | O(log n) | O(log n) | **Low** |
| B+ Tree | O(log_t n) | O(log n) | O(log n) | O(log n) | **Lowest** |

## Algorithm Pseudocode

### Search Operation

```
SEARCH(node, key):
    i = 0
    while i < node.n and key > node.keys[i]:
        i = i + 1
    
    if i < node.n and node.keys[i] == key:
        return node  // Key found
    
    if node.leaf:
        return null  // Key not found
    
    return SEARCH(node.children[i], key)  // Recurse into child
```

### Insert Operation

```
INSERT(tree, key):
    if tree.root == null:
        tree.root = new BTreeNode(t, true)
        tree.root.keys[0] = key
        tree.root.n = 1
    else:
        if tree.root.n == 2t - 1:
            // Root is full, create new root
            s = new BTreeNode(t, false)
            s.children[0] = tree.root
            SPLIT-CHILD(s, 0, tree.root)
            i = 0
            if s.keys[0] < key:
                i = i + 1
            INSERT-NON-FULL(s.children[i], key)
            tree.root = s
        else:
            INSERT-NON-FULL(tree.root, key)
```

### Split Child Operation

```
SPLIT-CHILD(parent, i, fullChild):
    // Create new node to hold half of fullChild's keys
    newNode = new BTreeNode(fullChild.t, fullChild.leaf)
    newNode.n = t - 1
    
    // Copy right half of keys to new node
    for j = 0 to t - 2:
        newNode.keys[j] = fullChild.keys[j + t]
    
    // If not leaf, copy right half of children
    if not fullChild.leaf:
        for j = 0 to t - 1:
            newNode.children[j] = fullChild.children[j + t]
    
    fullChild.n = t - 1
    
    // Shift parent's children and keys to make room
    for j = parent.n downto i + 1:
        parent.children[j + 1] = parent.children[j]
    parent.children[i + 1] = newNode
    
    for j = parent.n - 1 downto i:
        parent.keys[j + 1] = parent.keys[j]
    
    // Move middle key up to parent
    parent.keys[i] = fullChild.keys[t - 1]
    parent.n = parent.n + 1
```

### Delete Operation

The delete operation is more complex, involving three cases:

```
DELETE(node, key):
    idx = FIND-KEY(node, key)
    
    if idx < node.n and node.keys[idx] == key:
        if node.leaf:
            REMOVE-FROM-LEAF(node, idx)
        else:
            REMOVE-FROM-NON-LEAF(node, idx)
    else:
        if node.leaf:
            return  // Key not found
        
        flag = (idx == node.n)
        if node.children[idx].n < t:
            FILL(node, idx)
        
        if flag and idx > node.n:
            DELETE(node.children[idx - 1], key)
        else:
            DELETE(node.children[idx], key)
```

## Implementation Details

### Java Implementation Highlights

```java
public class BTree {
    static class BTreeNode {
        int[] keys;
        int t; // Minimum degree
        BTreeNode[] children;
        int n; // Current number of keys
        boolean leaf;

        BTreeNode(int t, boolean leaf) {
            this.t = t;
            this.leaf = leaf;
            this.keys = new int[2 * t - 1];
            this.children = new BTreeNode[2 * t];
            this.n = 0;
        }
    }

    private BTreeNode root;
    private final int t;

    public BTree(int t) {
        this.root = null;
        this.t = t;
    }
}
```

### Key Implementation Methods

#### Search Implementation

```java
BTreeNode search(int key) {
    int i = 0;
    while (i < n && key > keys[i]) {
        i++;
    }
    if (i < n && keys[i] == key) {
        return this;
    }
    if (leaf) {
        return null;
    }
    return children[i].search(key);
}
```

#### Insert Non-Full Node

```java
void insertNonFull(int key) {
    int i = n - 1;
    if (leaf) {
        while (i >= 0 && keys[i] > key) {
            keys[i + 1] = keys[i];
            i--;
        }
        keys[i + 1] = key;
        n++;
    } else {
        while (i >= 0 && keys[i] > key) {
            i--;
        }
        if (children[i + 1].n == 2 * t - 1) {
            splitChild(i + 1, children[i + 1]);
            if (keys[i + 1] < key) {
                i++;
            }
        }
        children[i + 1].insertNonFull(key);
    }
}
```

## Real-World Applications

### 1. Database Management Systems
- **MySQL/InnoDB:** Uses B+ trees for indexing
- **PostgreSQL:** B-tree indexes for primary and secondary keys
- **Oracle:** Index-organized tables

### 2. File Systems
- **NTFS (Windows):** Master File Table uses B-trees
- **HFS+ (macOS):** Catalog and extents files
- **ext4 (Linux):** Directory indexing with H-trees (similar)
- **Btrfs:** Copy-on-write B-trees

### 3. Key-Value Stores
- **Berkeley DB:** B-tree storage backend
- **LevelDB/RocksDB:** LSM trees with B-tree indices
- **SQLite:** B-tree for both tables and indices

### 4. Operating Systems
- **Virtual Memory:** Page table management
- **File Allocation:** Block allocation tracking

## B-Tree Variants

### B+ Tree

| Feature | B-Tree | B+ Tree |
|---------|--------|---------|
| Data storage | All nodes | Leaf nodes only |
| Leaf linkage | None | Linked list |
| Range queries | Less efficient | Very efficient |
| Space usage | Lower overhead | Higher overhead |

### B* Tree

- Nodes must be at least 2/3 full (vs 1/2 for B-tree)
- Delays splits by redistributing keys to siblings
- Better space utilization

## Common Pitfalls and Edge Cases

### 1. Choosing Minimum Degree (t)

```
❌ Pitfall: Choosing t too small or too large

Considerations:
- t too small: More levels, more disk accesses
- t too large: Wasted space, slower operations
- Optimal: Match disk block size
```

### 2. Split During Insert

```
⚠️ Edge Case: Root split creates new level

When root is full (2t-1 keys):
1. Create new root node
2. Old root becomes child
3. Split old root
4. Tree height increases by 1
```

### 3. Merge During Delete

```
⚠️ Edge Case: Underflow propagates to root

When root has only one key and both children have t-1 keys:
1. Merge children with root key
2. Merged child becomes new root
3. Tree height decreases by 1
```

### 4. Empty Tree After Deletion

```
⚠️ Edge Case: Deleting last key from single-node tree

public void delete(int key) {
    if (root == null) return;
    root.remove(key);
    if (root.n == 0) {
        root = root.leaf ? null : root.children[0];
    }
}
```

### 5. Duplicate Key Handling

```
✓ This implementation prevents duplicates:

public void insert(int key) {
    if (search(key)) {
        return;  // Duplicate found, skip insertion
    }
    // ... proceed with insertion
}
```

## Testing Strategies

### Unit Test Scenarios

1. **Empty tree operations**
2. **Single node operations**
3. **Root splits during insertion**
4. **Multiple level insertions**
5. **Deletion causing underflow**
6. **Deletion with redistribution**
7. **Deletion causing merge**
8. **Tree shrinking (root with single child)**

### Example Test Cases

```java
@Test
void testInsertionAndSearch() {
    BTree tree = new BTree(3);  // t = 3
    int[] keys = {10, 20, 5, 6, 12, 30, 7, 17};
    
    for (int key : keys) {
        tree.insert(key);
    }
    
    for (int key : keys) {
        assertTrue(tree.search(key));
    }
    assertFalse(tree.search(100));
}
```

## References

### Academic
- Bayer, R. & McCreight, E. (1970). "Organization and Maintenance of Large Ordered Indices"
- Comer, D. (1979). "The Ubiquitous B-Tree"
- Knuth, D. E. "The Art of Computer Programming, Volume 3"

### Related Algorithms
- [AVL Tree](avl-tree.md) - Height-balanced binary tree
- [Red-Black Tree](red-black-tree.md) - Color-balanced binary tree
- [Segment Tree](segment-tree.md) - Range query structure
- [2-3 Tree](https://en.wikipedia.org/wiki/2-3_tree) - Special case B-tree (t=2)

---

*Last updated: Phase 2 Documentation*
