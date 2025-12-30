# Red-Black Tree

> **Category:** Data Structures  
> **Subcategory:** Trees (Self-Balancing BST)  
> **Implementation:** [RedBlackBST.java](../../../src/main/java/com/thealgorithms/datastructures/trees/RedBlackBST.java)

---

## 📚 Overview

A **Red-Black Tree** is a self-balancing binary search tree where each node has an extra bit for color (red or black). The tree uses coloring rules and rotations to maintain approximate balance, ensuring that the longest path is no more than twice the length of the shortest path.

Invented by Rudolf Bayer in 1972 (as "symmetric binary B-trees") and later refined by Leonidas Guibas and Robert Sedgewick in 1978, Red-Black trees are widely used in language libraries and operating systems.

---

## 🔢 Mathematical Foundation

### Definition

A Red-Black tree is a BST satisfying these properties:

1. **Node Color:** Every node is either red or black
2. **Root Property:** The root is always black
3. **Leaf Property:** All leaves (NIL nodes) are black
4. **Red Property:** Red nodes cannot have red children (no consecutive reds)
5. **Black-Height Property:** Every path from a node to its descendant NIL nodes has the same number of black nodes

### Key Properties

- **Black-Height:** $bh(n)$ = number of black nodes on path from $n$ to any leaf (not counting $n$)
- **Height Bound:** $h \leq 2 \log_2(n+1)$
- **Subtree Size:** A subtree rooted at $n$ has $\geq 2^{bh(n)} - 1$ internal nodes

### Mathematical Formulation

**Height Bound Proof:**
- Any node $x$ has at least $2^{bh(x)} - 1$ internal nodes
- At least half the nodes on any path from root to leaf are black
- Therefore $bh(root) \geq h/2$
- Thus $n \geq 2^{h/2} - 1$, giving $h \leq 2\log_2(n+1)$

---

## 📊 Complexity Analysis

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| **Search** | $O(\log n)$ | $O(\log n)$ |
| **Insert** | $O(\log n)$ | $O(\log n)$ |
| **Delete** | $O(\log n)$ | $O(\log n)$ |
| **Space** | $O(n)$ | $O(n)$ |

### Rotation Comparison

| Operation | AVL Tree | Red-Black Tree |
|-----------|----------|----------------|
| Insert Rotations | 0-2 | 0-2 |
| Delete Rotations | 0-$\log n$ | 0-3 |
| Recoloring | N/A | May be required |

---

## 🔄 Red-Black Tree Operations

### Color Representation

```
RED   = 0
BLACK = 1
```

### Insertion Cases

**Case 1: Uncle is Red**
```
        G(B)                    G(R)
       /   \                   /   \
      P(R)  U(R)    ────►    P(B)  U(B)
     /                      /
    N(R)                   N(R)

Recolor: Parent and Uncle → Black, Grandparent → Red
Continue fixing from Grandparent
```

**Case 2: Uncle is Black, Node is Inner Child**
```
      G(B)                G(B)
     /   \               /   \
    P(R)  U(B)  ────►  N(R)  U(B)
      \               /
      N(R)          P(R)

Rotate at Parent (Left), then apply Case 3
```

**Case 3: Uncle is Black, Node is Outer Child**
```
        G(B)                  P(B)
       /   \                 /   \
      P(R)  U(B)  ────►   N(R)  G(R)
     /                            \
    N(R)                          U(B)

Rotate at Grandparent (Right), Recolor
```

---

## 🔄 Algorithm (Pseudocode)

### Insertion

```
ALGORITHM RBInsert(T, key)
    INPUT: Red-Black tree T, key to insert
    OUTPUT: Tree with key inserted
    
    // Standard BST insert
    1. z ← new Node(key)
    2. z.color ← RED
    3. BST_Insert(T, z)
    
    // Fix Red-Black properties
    4. RBInsertFixup(T, z)
```

### Insert Fixup

```
ALGORITHM RBInsertFixup(T, z)
    1. WHILE z.parent.color = RED DO
    2.     IF z.parent = z.parent.parent.left THEN
    3.         y ← z.parent.parent.right  // Uncle
    4.         
    5.         IF y.color = RED THEN        // Case 1
    6.             z.parent.color ← BLACK
    7.             y.color ← BLACK
    8.             z.parent.parent.color ← RED
    9.             z ← z.parent.parent
    10.        ELSE
    11.            IF z = z.parent.right THEN  // Case 2
    12.                z ← z.parent
    13.                LEFT_ROTATE(T, z)
    14.            END IF
    15.            // Case 3
    16.            z.parent.color ← BLACK
    17.            z.parent.parent.color ← RED
    18.            RIGHT_ROTATE(T, z.parent.parent)
    19.        END IF
    20.    ELSE
    21.        // Symmetric cases (left ↔ right)
    22.    END IF
    23. END WHILE
    24. T.root.color ← BLACK
```

### Left Rotation

```
ALGORITHM LEFT_ROTATE(T, x)
    y ← x.right
    x.right ← y.left
    
    IF y.left ≠ T.nil THEN
        y.left.parent ← x
    END IF
    
    y.parent ← x.parent
    
    IF x.parent = T.nil THEN
        T.root ← y
    ELSE IF x = x.parent.left THEN
        x.parent.left ← y
    ELSE
        x.parent.right ← y
    END IF
    
    y.left ← x
    x.parent ← y
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Uses sentinel NIL node for simplicity
- Color represented as int (RED=0, BLACK=1)
- Includes full deletion with fixup
- Parent pointers for efficient traversal

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/trees/RedBlackBST.java`

```java
public class RedBlackBST {
    private final int RED = 0;
    private final int BLACK = 1;
    
    private class Node {
        int key, color;
        Node left, right, parent;
        
        Node(int key) {
            this.key = key;
            this.color = RED;  // New nodes are red
        }
    }
    
    private Node nil;  // Sentinel node
    private Node root;
    
    public RedBlackBST() {
        nil = new Node(0);
        nil.color = BLACK;
        root = nil;
    }
    
    // Left rotation
    private void rotateLeft(Node x) {
        Node y = x.right;
        x.right = y.left;
        
        if (y.left != nil) {
            y.left.parent = x;
        }
        
        y.parent = x.parent;
        
        if (x.parent == nil) {
            root = y;
        } else if (x == x.parent.left) {
            x.parent.left = y;
        } else {
            x.parent.right = y;
        }
        
        y.left = x;
        x.parent = y;
    }
    
    // Fix tree after insertion
    private void fixTree(Node z) {
        while (z.parent.color == RED) {
            if (z.parent == z.parent.parent.left) {
                Node y = z.parent.parent.right;
                
                if (y.color == RED) {
                    // Case 1: Uncle is red
                    z.parent.color = BLACK;
                    y.color = BLACK;
                    z.parent.parent.color = RED;
                    z = z.parent.parent;
                } else {
                    if (z == z.parent.right) {
                        // Case 2: Uncle black, z is right child
                        z = z.parent;
                        rotateLeft(z);
                    }
                    // Case 3: Uncle black, z is left child
                    z.parent.color = BLACK;
                    z.parent.parent.color = RED;
                    rotateRight(z.parent.parent);
                }
            } else {
                // Symmetric cases
            }
        }
        root.color = BLACK;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Java Collections Framework
**Use Case:** `TreeMap` and `TreeSet` implementation  
**Example:** Sorted map with guaranteed $O(\log n)$ operations

### 2. Linux Kernel
**Use Case:** Completely Fair Scheduler (CFS)  
**Example:** Organizing processes by virtual runtime for scheduling

### 3. C++ STL
**Use Case:** `std::map`, `std::set`, `std::multimap`, `std::multiset`  
**Example:** Ordered associative containers

### 4. Database Systems
**Use Case:** In-memory index structures  
**Example:** PostgreSQL internal data structures

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Java (OpenJDK) | TreeMap, TreeSet |
| Linux Kernel | Process scheduler, memory management |
| GNU libstdc++ | Associative containers |
| Nginx | Timer management |
| Epoll | Event management (Linux) |

---

## ⚖️ Comparison with Related Data Structures

| Aspect | Red-Black Tree | AVL Tree | B-Tree | 2-3-4 Tree |
|--------|---------------|----------|--------|------------|
| Balance | Color rules | Height diff ≤1 | Multi-way | 2-4 children |
| Height | $\leq 2\log n$ | $\leq 1.44\log n$ | $\log_B n$ | $\log_4 n$ to $\log_2 n$ |
| Insert Rotations | 0-2 | 0-2 | 0 | 0 |
| Delete Rotations | 0-3 | 0-$\log n$ | 0 | 0 |
| Best For | General use | Read-heavy | Disk I/O | Isomorphic to RB |

### When to Choose Red-Black Tree

✅ **Use Red-Black Tree when:**
- Need guaranteed $O(\log n)$ operations
- Insertions and deletions are frequent
- Need ordered data access
- Standard library implementation is acceptable

❌ **Don't use when:**
- Need disk-based storage (use B-Tree)
- Data is mostly read-only (AVL may be better)
- Need concurrent access (use concurrent structures)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **NIL Sentinel:** Must initialize and use consistently
2. **Root Color:** Always ensure root is black after operations
3. **Parent Pointers:** Must update during rotations
4. **Deletion Complexity:** Double-black cases require careful handling

### Edge Cases to Handle

- [x] Insert into empty tree
- [x] Delete root node
- [x] Consecutive red nodes (must fix)
- [x] Delete causing double-black
- [x] Rotation at root

### Common Bug: NIL Pointer Issues

```java
// BUG: Checking null instead of nil
if (node.left == null) { ... }

// CORRECT: Check against sentinel
if (node.left == nil) { ... }

// BUG: Not initializing nil properly
Node nil;  // Uninitialized!

// CORRECT: Initialize sentinel
nil = new Node(0);
nil.color = BLACK;
nil.left = nil;
nil.right = nil;
nil.parent = nil;
```

---

## 📖 References

1. Bayer, R. (1972). "Symmetric binary B-Trees: Data structure and maintenance algorithms". *Acta Informatica*. 1 (4): 290–306.
2. Guibas, L. J., & Sedgewick, R. (1978). "A dichromatic framework for balanced trees". *Proceedings of FOCS*. 8–21.
3. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 13.
4. Sedgewick, R. (2008). "Left-leaning red-black trees". *Dagstuhl Workshop on Data Structures*.
5. [Wikipedia: Red-Black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)

---

## 🔗 Related Algorithms

- [AVL Tree](avl-tree.md) - Stricter balancing
- [B-Tree](b-tree.md) - Disk-optimized multi-way tree
- [2-3-4 Tree](2-3-4-tree.md) - Isomorphic to Red-Black
- [Left-Leaning Red-Black Tree](llrb.md) - Simplified variant
