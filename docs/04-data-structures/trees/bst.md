# Binary Search Tree (BST)

> **Category:** Data Structures  
> **Subcategory:** Trees  
> **Implementation:** [BSTIterative.java](../../src/main/java/com/thealgorithms/datastructures/trees/BSTIterative.java)

---

## 📚 Overview

A **Binary Search Tree (BST)** is a binary tree where each node follows the ordering property: all values in the left subtree are less than the node's value, and all values in the right subtree are greater. This property enables efficient searching, insertion, and deletion operations with O(log n) average-case performance.

---

## 🔢 Mathematical Foundation

### BST Property

For every node N in the tree:
- All values in left subtree of N < N.value
- All values in right subtree of N > N.value

Formally: $\forall L \in LeftSubtree(N), \forall R \in RightSubtree(N): L.value < N.value < R.value$

### Key Properties

- **In-order Traversal:** Produces sorted sequence
- **Height:** $h = O(\log n)$ for balanced tree, $O(n)$ worst case
- **Unique Values:** Typically no duplicates (or handled consistently)

### Height Analysis

| Tree Type | Height | Operations |
|-----------|--------|------------|
| **Balanced** | $O(\log n)$ | $O(\log n)$ |
| **Degenerate (skewed)** | $O(n)$ | $O(n)$ |
| **Average random** | $O(\log n)$ | $O(\log n)$ |

---

## 📊 Complexity Analysis

| Operation | Average Case | Worst Case | Notes |
|-----------|--------------|------------|-------|
| **search(key)** | $O(\log n)$ | $O(n)$ | Skewed tree worst case |
| **insert(key)** | $O(\log n)$ | $O(n)$ | Degenerates if sorted input |
| **delete(key)** | $O(\log n)$ | $O(n)$ | Requires restructuring |
| **min/max** | $O(\log n)$ | $O(n)$ | Traverse to leaf |
| **predecessor/successor** | $O(\log n)$ | $O(n)$ | May traverse up |

### Space Complexity

- **Per Node:** Data + left pointer + right pointer
- **Total:** $O(n)$ for n nodes
- **Recursion Stack:** $O(h)$ where h is height

---

## 🔄 Operations (Pseudocode)

### Node Structure
```
CLASS BSTNode<T>
    data: T
    left: BSTNode<T>
    right: BSTNode<T>
    
    CONSTRUCTOR(data)
        this.data ← data
        this.left ← null
        this.right ← null
```

### Search (Iterative)
```
ALGORITHM Search(root, key)
    current ← root
    WHILE current ≠ null DO
        IF key = current.data THEN
            RETURN current
        ELSE IF key < current.data THEN
            current ← current.left
        ELSE
            current ← current.right
    RETURN null  // Not found
```

### Insert (Iterative)
```
ALGORITHM Insert(root, key)
    newNode ← new BSTNode(key)
    IF root = null THEN
        RETURN newNode
    
    current ← root
    parent ← null
    WHILE current ≠ null DO
        parent ← current
        IF key < current.data THEN
            current ← current.left
        ELSE IF key > current.data THEN
            current ← current.right
        ELSE
            RETURN root  // Duplicate, no insert
    
    IF key < parent.data THEN
        parent.left ← newNode
    ELSE
        parent.right ← newNode
    RETURN root
```

### Delete
```
ALGORITHM Delete(root, key)
    1. Find node to delete
    2. Handle three cases:
       a. Leaf node: Remove directly
       b. One child: Replace with child
       c. Two children: Replace with in-order successor/predecessor
```

### In-Order Traversal
```
ALGORITHM InOrder(node)
    IF node ≠ null THEN
        InOrder(node.left)
        VISIT(node.data)
        InOrder(node.right)
```

### Step-by-Step Insert Example

**Inserting sequence: [50, 30, 70, 20, 40, 60, 80]**

```
Step 1: Insert 50         Step 2: Insert 30
        50                        50
                                 /
                                30

Step 3: Insert 70         Step 4: Insert 20
        50                        50
       /  \                      /  \
      30   70                   30   70
                               /
                              20

Final tree after all inserts:
            50
          /    \
         30     70
        /  \   /  \
       20  40 60  80

In-order: 20, 30, 40, 50, 60, 70, 80 (sorted!)
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

The repository provides multiple BST implementations:
- **BSTIterative:** Non-recursive operations
- **BSTRecursive:** Classic recursive implementation
- **BSTRecursiveGeneric:** Generic type support

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/trees/BSTIterative.java`

```java
public class BSTIterative {
    private Node root;
    
    private static class Node {
        int data;
        Node left, right;
        
        Node(int data) {
            this.data = data;
        }
    }
    
    public void add(int data) {
        Node newNode = new Node(data);
        if (root == null) {
            root = newNode;
            return;
        }
        
        Node current = root;
        Node parent = null;
        
        while (current != null) {
            parent = current;
            if (data < current.data) {
                current = current.left;
            } else if (data > current.data) {
                current = current.right;
            } else {
                return; // Duplicate
            }
        }
        
        if (data < parent.data) {
            parent.left = newNode;
        } else {
            parent.right = newNode;
        }
    }
    
    public Node find(int data) {
        Node current = root;
        while (current != null) {
            if (data == current.data) {
                return current;
            } else if (data < current.data) {
                current = current.left;
            } else {
                current = current.right;
            }
        }
        return null;
    }
    
    public void remove(int data) {
        root = removeRecursive(root, data);
    }
    
    private Node removeRecursive(Node node, int data) {
        if (node == null) return null;
        
        if (data < node.data) {
            node.left = removeRecursive(node.left, data);
        } else if (data > node.data) {
            node.right = removeRecursive(node.right, data);
        } else {
            // Node found - handle 3 cases
            if (node.left == null) return node.right;
            if (node.right == null) return node.left;
            
            // Two children: find in-order successor
            node.data = findMin(node.right).data;
            node.right = removeRecursive(node.right, node.data);
        }
        return node;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Database Indexing
**Use Case:** Quick record lookup by key  
**Example:** Many database engines use BST variants (B-trees) for indexes

### 2. File Systems
**Use Case:** Directory organization  
**Example:** File system directory structures often use tree organization

### 3. Autocomplete Systems
**Use Case:** Prefix-based searching  
**Example:** Search suggestions in IDEs, browsers

### 4. Priority Scheduling
**Use Case:** Task management by priority  
**Example:** Operating system process scheduling

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **MySQL** | Index structures (B+ trees based on BST) |
| **Linux** | Virtual memory management (Red-Black tree) |
| **Java Collections** | TreeMap, TreeSet implementations |
| **Git** | Object storage (Merkle tree) |

---

## ⚖️ Comparison with Related Data Structures

| Feature | BST | AVL | Red-Black | B-Tree |
|---------|-----|-----|-----------|--------|
| **Balance** | None | Strict | Relaxed | Block-level |
| **Height** | O(n) worst | O(log n) | O(log n) | O(log n) |
| **Insert** | O(log n)* | O(log n) | O(log n) | O(log n) |
| **Rotations** | None | Up to 2 | Up to 3 | None |
| **Use Case** | Simple cases | Lookup-heavy | Balanced | Disk storage |

*Average case; worst case O(n) for skewed BST

### When to Use BST

✅ **Use BST when:**
- Data is randomly distributed
- Simple implementation needed
- Dynamic ordered data

❌ **Avoid BST when:**
- Input is sorted (causes degeneration)
- Guaranteed O(log n) required (use balanced variants)
- Large datasets on disk (use B-trees)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Sorted Input Degeneration:** Inserting sorted sequence creates linked list
   - Solution: Use self-balancing trees (AVL, Red-Black)

2. **Delete with Two Children:** Complex restructuring
   - Solution: Replace with in-order successor/predecessor

3. **Memory Leaks:** Not properly nullifying deleted nodes
   - Solution: Set parent references to null

4. **Duplicate Handling:** Unclear behavior
   - Solution: Define consistent policy (reject, allow in left/right)

### Edge Cases to Handle

- [x] Empty tree operations
- [x] Single node tree
- [x] Deleting root node
- [x] Duplicate insertions
- [x] Searching non-existent key

---

## 📖 References

1. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 12
2. Knuth, D.E. "The Art of Computer Programming, Vol. 3" (1998)
3. [Wikipedia: Binary Search Tree](https://en.wikipedia.org/wiki/Binary_search_tree)

---

## 🔗 Related Data Structures

- [AVL Tree](./avl-tree.md) - Self-balancing BST
- [Red-Black Tree](./red-black-tree.md) - Balanced with color property
- [B-Tree](./b-tree.md) - Multi-way search tree
- [Trie](./trie.md) - Prefix tree for strings
- [Heap](./heap.md) - Complete binary tree with heap property
