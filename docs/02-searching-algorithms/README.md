# 🔍 Searching Algorithms

> **Category:** Core Algorithms  
> **Difficulty:** Beginner to Intermediate  
> **Prerequisites:** Arrays, Basic Data Structures, Recursion

---

## 📚 Overview

Searching algorithms are designed to retrieve information stored within a data structure or determine if an element exists. They are fundamental operations in computer science, used billions of times daily across all computing systems.

### Why Searching Matters

- **Data Retrieval:** Finding specific records in databases
- **Validation:** Checking existence before operations
- **Navigation:** Finding paths in graphs and networks
- **Optimization:** Finding optimal solutions in search spaces

---

## 📊 Classification

```
Searching Algorithms
├── Linear Structures
│   ├── Linear Search (Sequential)
│   ├── Binary Search (Requires sorted data)
│   ├── Interpolation Search
│   ├── Fibonacci Search
│   ├── Exponential Search
│   └── Jump Search
│
├── Graph/Tree Traversals
│   ├── Breadth-First Search (BFS)
│   ├── Depth-First Search (DFS)
│   └── A* Search
│
├── String Searching
│   ├── Naive Pattern Matching
│   ├── KMP Algorithm
│   ├── Boyer-Moore
│   └── Rabin-Karp
│
└── Specialized
    ├── Binary Search Tree Search
    ├── Hash Table Lookup
    └── Ternary Search
```

---

## 📈 Complexity Comparison

### Array/List Searches

| Algorithm | Time (Best) | Time (Avg) | Time (Worst) | Space | Prerequisite |
|-----------|-------------|------------|--------------|-------|--------------|
| [Linear Search](./linear-search.md) | O(1) | O(n) | O(n) | O(1) | None |
| [Binary Search](./binary-search.md) | O(1) | O(log n) | O(log n) | O(1) | Sorted |
| [Interpolation Search](./interpolation-search.md) | O(1) | O(log log n) | O(n) | O(1) | Sorted, Uniform |
| [Jump Search](./jump-search.md) | O(1) | O(√n) | O(√n) | O(1) | Sorted |
| [Exponential Search](./exponential-search.md) | O(1) | O(log n) | O(log n) | O(1) | Sorted |
| [Fibonacci Search](./fibonacci-search.md) | O(1) | O(log n) | O(log n) | O(1) | Sorted |

### Graph Traversals

| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| [BFS](./bfs.md) | O(V + E) | O(V) | Shortest path (unweighted), level-order |
| [DFS](./dfs.md) | O(V + E) | O(V) | Path finding, cycle detection, topological sort |

---

## 🎯 Algorithm Selection Guide

```mermaid
flowchart TD
    A[Need to Search] --> B{Data Structure?}
    B -->|Array/List| C{Is it Sorted?}
    B -->|Graph/Tree| D{Weighted Edges?}
    B -->|Hash Table| E[O(1) Lookup]
    
    C -->|No| F[Linear Search O(n)]
    C -->|Yes| G{Distribution Known?}
    
    G -->|Uniform| H[Interpolation Search]
    G -->|Unknown| I{Size?}
    
    I -->|Small| J[Binary Search]
    I -->|Large/Unknown| K[Exponential Search]
    
    D -->|No| L{Need Shortest Path?}
    D -->|Yes| M[Dijkstra/A*]
    
    L -->|Yes| N[BFS]
    L -->|No| O[DFS]
    
    style E fill:#90EE90
    style H fill:#90EE90
    style J fill:#90EE90
    style N fill:#90EE90
```

### Quick Selection Table

| Scenario | Recommended | Why |
|----------|-------------|-----|
| Unsorted small array | Linear Search | Simple, no preprocessing |
| Sorted array | Binary Search | O(log n) guaranteed |
| Sorted with uniform distribution | Interpolation Search | O(log log n) average |
| Unbounded/infinite sorted array | Exponential Search | Finds range then binary search |
| Shortest path (unweighted graph) | BFS | Guarantees shortest path |
| Cycle detection | DFS | Natural stack-based approach |
| Memory-constrained graph | DFS | O(depth) vs O(width) space |
| Level-by-level exploration | BFS | Processes by distance |

---

## 🔬 Mathematical Foundation

### Binary Search Correctness

**Loop Invariant:** If target exists, it is in `arr[low..high]`

**Recurrence Relation:**
$$
T(n) = T(n/2) + O(1) = O(\log n)
$$

### Interpolation Search Position Formula

$$
pos = low + \left\lfloor \frac{(key - arr[low]) \times (high - low)}{arr[high] - arr[low]} \right\rfloor
$$

**Expected Time (Uniform Distribution):**
$$
T(n) = O(\log \log n)
$$

### BFS/DFS Complexity

For graph G = (V, E):
- **Time:** O(|V| + |E|) - each vertex and edge visited once
- **Space:** O(|V|) - queue/stack can hold all vertices

---

## 📁 Algorithms in This Section

### Linear Structure Searches

| File | Algorithm | Status |
|------|-----------|--------|
| [linearsearch.md](./linear-search/linearsearch.md) | Linear Search | ✅ Complete |
| [binarysearch.md](./binary-search/binarysearch.md) | Binary Search | ✅ Complete |
| [interpolationsearch.md](./interpolation-search/interpolationsearch.md) | Interpolation Search | ✅ Complete |
| [jumpsearch.md](./jump-search/jumpsearch.md) | Jump Search | ✅ Complete |
| [exponential-search.md](./exponential-search.md) | Exponential Search | 📋 Planned |
| [fibonacci-search.md](./fibonacci-search.md) | Fibonacci Search | 📋 Planned |
| [ternary-search.md](./ternary-search.md) | Ternary Search | 📋 Planned |

### Graph Traversals

| File | Algorithm | Status |
|------|-----------|--------|
| [bfs.md](./bfs.md) | Breadth-First Search | 📋 Planned |
| [dfs.md](./dfs.md) | Depth-First Search | 📋 Planned |

---

## 🌍 Real-World Applications

| Application | Algorithm | Why |
|-------------|-----------|-----|
| Dictionary lookup | Binary Search / Hash | O(log n) or O(1) lookup |
| Social network connections | BFS | Find degrees of separation |
| Web crawlers | BFS/DFS | Systematic page discovery |
| GPS navigation | A* Search | Optimal pathfinding |
| Database queries | B-tree search | Disk-optimized searching |
| Spell checkers | Trie search | Prefix matching |
| Git history | DFS | Traversing commit graph |

---

## 📖 References

1. Cormen, T. H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 22
2. Sedgewick, R. *"Algorithms"*, Part 4: Searching
3. Knuth, D. E. *"The Art of Computer Programming"*, Volume 3

---

[← Back to Main Index](../README.md)
