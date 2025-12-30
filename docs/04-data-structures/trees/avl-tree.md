# AVL Tree

> **Category:** Data Structures  
> **Subcategory:** Trees (Self-Balancing BST)  
> **Implementation:** [AVLTree.java](../../../src/main/java/com/thealgorithms/datastructures/trees/AVLTree.java)

---

## 📚 Overview

An **AVL tree** is a self-balancing binary search tree where the heights of the left and right subtrees of any node differ by at most one. Named after its inventors Georgy Adelson-Velsky and Evgenii Landis (1962), it was the first self-balancing BST data structure invented.

The AVL tree maintains its balance through rotations after insertions and deletions, ensuring $O(\log n)$ time complexity for all basic operations.

---

## 🔢 Mathematical Foundation

### Definition

An AVL tree is a binary search tree where for every node $n$:
$$
|\text{height}(n.\text{left}) - \text{height}(n.\text{right})| \leq 1
$$

### Key Properties

- **Balance Factor:** $\text{BF}(n) = \text{height}(n.\text{left}) - \text{height}(n.\text{right})$
- **Valid Range:** $\text{BF}(n) \in \{-1, 0, 1\}$
- **Height Bound:** $h < 1.44 \log_2(n+2) - 0.328$
- **Minimum Nodes:** $N_h = F_{h+3} - 1$ where $F_i$ is Fibonacci number

### Height-Node Relationship

For an AVL tree of height $h$, the minimum number of nodes is:
$$
N_h = N_{h-1} + N_{h-2} + 1
$$

This Fibonacci-like recurrence proves the $O(\log n)$ height guarantee.

---

## 📊 Complexity Analysis

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| **Search** | $O(\log n)$ | $O(\log n)$ |
| **Insert** | $O(\log n)$ | $O(\log n)$ |
| **Delete** | $O(\log n)$ | $O(\log n)$ |
| **Space** | $O(n)$ | $O(n)$ |

### Comparison with Other BSTs

| Tree Type | Search | Insert | Delete | Worst Height |
|-----------|--------|--------|--------|--------------|
| AVL | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $1.44 \log n$ |
| Red-Black | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $2 \log n$ |
| Unbalanced BST | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(n)$ |

---

## 🔄 Rotation Operations

### Single Rotations

**Right Rotation (LL Case):**
```
        y                   x
       / \                 / \
      x   C   ──────►     A   y
     / \                     / \
    A   B                   B   C
```

**Left Rotation (RR Case):**
```
      x                       y
     / \                     / \
    A   y     ──────►       x   C
       / \                 / \
      B   C               A   B
```

### Double Rotations

**Left-Right Rotation (LR Case):**
```
      z               z               y
     / \             / \             / \
    x   D  ────►    y   D  ────►   x   z
   / \             / \             / \ / \
  A   y           x   C           A  B C  D
     / \         / \
    B   C       A   B
```

**Right-Left Rotation (RL Case):**
```
    z                 z                  y
   / \               / \                / \
  A   x    ────►    A   y    ────►    z   x
     / \               / \           / \ / \
    y   D             B   x         A  B C  D
   / \                   / \
  B   C                 C   D
```

---

## 🔄 Algorithm (Pseudocode)

### Insertion

```
ALGORITHM AVLInsert(root, key)
    INPUT: Root node, key to insert
    OUTPUT: New root after insertion and rebalancing
    
    // Standard BST insert
    1. IF root = NULL THEN
    2.     RETURN new Node(key)
    3. END IF
    
    4. IF key < root.key THEN
    5.     root.left ← AVLInsert(root.left, key)
    6. ELSE IF key > root.key THEN
    7.     root.right ← AVLInsert(root.right, key)
    8. ELSE
    9.     RETURN root  // Duplicate keys not allowed
    10. END IF
    
    // Update height
    11. root.height ← 1 + max(height(root.left), height(root.right))
    
    // Get balance factor
    12. balance ← getBalance(root)
    
    // Rebalance if needed
    13. IF balance > 1 AND key < root.left.key THEN
    14.     RETURN rightRotate(root)        // LL Case
    15. END IF
    
    16. IF balance < -1 AND key > root.right.key THEN
    17.     RETURN leftRotate(root)         // RR Case
    18. END IF
    
    19. IF balance > 1 AND key > root.left.key THEN
    20.     root.left ← leftRotate(root.left)
    21.     RETURN rightRotate(root)        // LR Case
    22. END IF
    
    23. IF balance < -1 AND key < root.right.key THEN
    24.     root.right ← rightRotate(root.right)
    25.     RETURN leftRotate(root)         // RL Case
    26. END IF
    
    27. RETURN root
```

### Right Rotation

```
ALGORITHM rightRotate(y)
    x ← y.left
    T2 ← x.right
    
    // Perform rotation
    x.right ← y
    y.left ← T2
    
    // Update heights
    y.height ← max(height(y.left), height(y.right)) + 1
    x.height ← max(height(x.left), height(x.right)) + 1
    
    RETURN x  // New root
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Generic type support with `Comparable`
- Recursive insertion and deletion
- Four rotation methods for rebalancing
- Height tracking in each node

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/trees/AVLTree.java`

```java
public class AVLTree {
    private Node root;
    
    private class Node {
        int key, height;
        Node left, right;
        
        Node(int key) {
            this.key = key;
            this.height = 1;
        }
    }
    
    // Get balance factor
    private int getBalance(Node n) {
        if (n == null) return 0;
        return height(n.left) - height(n.right);
    }
    
    // Right rotation
    private Node rotateRight(Node y) {
        Node x = y.left;
        Node T2 = x.right;
        
        x.right = y;
        y.left = T2;
        
        y.height = Math.max(height(y.left), height(y.right)) + 1;
        x.height = Math.max(height(x.left), height(x.right)) + 1;
        
        return x;
    }
    
    // Rebalance after insert
    private Node rebalance(Node node) {
        int balance = getBalance(node);
        
        // Left Left Case
        if (balance > 1 && getBalance(node.left) >= 0)
            return rotateRight(node);
        
        // Left Right Case
        if (balance > 1 && getBalance(node.left) < 0) {
            node.left = rotateLeft(node.left);
            return rotateRight(node);
        }
        
        // Right Right Case
        if (balance < -1 && getBalance(node.right) <= 0)
            return rotateLeft(node);
        
        // Right Left Case
        if (balance < -1 && getBalance(node.right) > 0) {
            node.right = rotateRight(node.right);
            return rotateLeft(node);
        }
        
        return node;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Database Indexing
**Use Case:** In-memory database indexes  
**Example:** Maintaining sorted indexes with guaranteed $O(\log n)$ operations

### 2. File System Organization
**Use Case:** Directory structure management  
**Example:** Efficient file lookup in large directory trees

### 3. Memory Allocators
**Use Case:** Tracking free memory blocks  
**Example:** Best-fit memory allocation using size-ordered AVL tree

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| PostgreSQL | Internal index structures |
| Linux Kernel | Virtual memory areas (VMAs) |
| File Systems | Directory indexing |
| Compilers | Symbol tables |
| Spell Checkers | Dictionary storage |

---

## ⚖️ Comparison with Related Data Structures

| Aspect | AVL Tree | Red-Black Tree | B-Tree | Skip List |
|--------|----------|---------------|--------|-----------|
| Balance | Strict (±1) | Relaxed (2x) | Multi-way | Probabilistic |
| Height | $1.44 \log n$ | $2 \log n$ | $\log_B n$ | $\log n$ expected |
| Rotations (Insert) | 0-2 | 0-2 | 0 | 0 |
| Rotations (Delete) | 0-$\log n$ | 0-3 | 0 | 0 |
| Best For | Read-heavy | Write-heavy | Disk I/O | Concurrent |

### When to Choose AVL Tree

✅ **Use AVL when:**
- Operations are read-heavy
- Need strict balance guarantee
- Memory is not severely constrained
- Predictable performance required

❌ **Don't use when:**
- Operations are write-heavy (use Red-Black)
- Working with disk-based storage (use B-Tree)
- Need concurrent access (use Skip List)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Height Update:** Must update heights bottom-up after rotations
2. **Balance Factor Calculation:** Left height minus right (not vice versa)
3. **Deletion Complexity:** May require multiple rotations up the tree
4. **Null Height:** Define height of null as -1 or 0 consistently

### Edge Cases to Handle

- [x] Empty tree operations
- [x] Single node tree
- [x] Duplicate key insertion (reject or update)
- [x] Deleting root node
- [x] Cascading rotations during deletion

### Common Bug: Forgetting Height Update

```java
// BUG: Height not updated after rotation
private Node rotateRight(Node y) {
    Node x = y.left;
    y.left = x.right;
    x.right = y;
    return x;  // Missing height updates!
}

// CORRECT: Update heights
private Node rotateRight(Node y) {
    Node x = y.left;
    y.left = x.right;
    x.right = y;
    
    y.height = Math.max(height(y.left), height(y.right)) + 1;
    x.height = Math.max(height(x.left), height(x.right)) + 1;
    
    return x;
}
```

---

## 📖 References

1. Adelson-Velsky, G. M., & Landis, E. M. (1962). "An algorithm for the organization of information". *Doklady Akademii Nauk SSSR*. 146: 263–266.
2. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 13.
3. Knuth, D. E. (1998). *The Art of Computer Programming, Volume 3: Sorting and Searching* (2nd ed.). Addison-Wesley.
4. [Wikipedia: AVL tree](https://en.wikipedia.org/wiki/AVL_tree)

---

## 🔗 Related Algorithms

- [Red-Black Tree](red-black-tree.md) - Less strict balancing
- [Binary Search Tree](bst.md) - Unbalanced base structure
- [B-Tree](b-tree.md) - Multi-way balanced tree
- [Splay Tree](splay-tree.md) - Self-adjusting BST
