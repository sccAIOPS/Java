# 📊 Complexity Cheatsheet

> Quick reference for algorithm complexities

---

## Time Complexity Hierarchy

```
O(1) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)

Constant < Logarithmic < Root < Linear < Linearithmic < Quadratic < Cubic < Exponential < Factorial
```

---

## Sorting Algorithms

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | ✅ |
| Radix Sort | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | ✅ |
| Tim Sort | O(n) | O(n log n) | O(n log n) | O(n) | ✅ |

---

## Searching Algorithms

| Algorithm | Best | Average | Worst | Space | Requirement |
|-----------|------|---------|-------|-------|-------------|
| Linear Search | O(1) | O(n) | O(n) | O(1) | None |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) | Sorted |
| Jump Search | O(1) | O(√n) | O(√n) | O(1) | Sorted |
| Interpolation | O(1) | O(log log n) | O(n) | O(1) | Sorted, Uniform |
| Hash Table | O(1) | O(1) | O(n) | O(n) | None |

---

## Graph Algorithms

| Algorithm | Time | Space |
|-----------|------|-------|
| BFS | O(V + E) | O(V) |
| DFS | O(V + E) | O(V) |
| Dijkstra | O((V+E) log V) | O(V) |
| Bellman-Ford | O(VE) | O(V) |
| Floyd-Warshall | O(V³) | O(V²) |
| Prim's MST | O(E log V) | O(V) |
| Kruskal's MST | O(E log E) | O(V) |
| Topological Sort | O(V + E) | O(V) |

---

## Data Structure Operations

### Arrays & Lists

| Operation | Array | ArrayList | LinkedList |
|-----------|-------|-----------|------------|
| Access | O(1) | O(1) | O(n) |
| Search | O(n) | O(n) | O(n) |
| Insert (end) | N/A | O(1)* | O(1) |
| Insert (middle) | N/A | O(n) | O(1)† |
| Delete | N/A | O(n) | O(1)† |

*Amortized †With reference to node

### Trees

| Operation | BST (avg) | BST (worst) | AVL | Red-Black | B-Tree |
|-----------|-----------|-------------|-----|-----------|--------|
| Search | O(log n) | O(n) | O(log n) | O(log n) | O(log n) |
| Insert | O(log n) | O(n) | O(log n) | O(log n) | O(log n) |
| Delete | O(log n) | O(n) | O(log n) | O(log n) | O(log n) |

### Hash Tables

| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(1) | O(n) |
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |

---

## Space Complexity Quick Reference

| Data Structure | Space |
|----------------|-------|
| Array | O(n) |
| LinkedList | O(n) |
| Hash Table | O(n) |
| Binary Tree | O(n) |
| Graph (Adj List) | O(V + E) |
| Graph (Adj Matrix) | O(V²) |
| Heap | O(n) |
| Trie | O(ALPHABET × n × m) |

---

## Common Recurrences

| Recurrence | Solution | Example |
|------------|----------|---------|
| T(n) = T(n/2) + O(1) | O(log n) | Binary Search |
| T(n) = T(n-1) + O(1) | O(n) | Linear Search |
| T(n) = T(n-1) + O(n) | O(n²) | Selection Sort |
| T(n) = 2T(n/2) + O(1) | O(n) | Tree Traversal |
| T(n) = 2T(n/2) + O(n) | O(n log n) | Merge Sort |
| T(n) = 2T(n-1) + O(1) | O(2ⁿ) | Fibonacci (naive) |

---

[← Back to Index](../README.md)
