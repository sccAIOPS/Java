# 📚 TheAlgorithms/Java - Documentation

> **Comprehensive algorithm documentation with mathematical foundations, pseudocode, and real-world applications**

[![Build](https://github.com/TheAlgorithms/Java/actions/workflows/build.yml/badge.svg?branch=master)](https://github.com/TheAlgorithms/Java/actions/workflows/build.yml)
[![Documentation](https://img.shields.io/badge/docs-complete-brightgreen.svg)](./README.md)

---

## 🎯 About This Documentation

This documentation provides in-depth coverage of **400+ algorithms** implemented in the TheAlgorithms/Java repository. Each algorithm is documented with:

- 🔢 **Mathematical Foundation** - Formal definitions, theorems, and proofs
- 📊 **Complexity Analysis** - Time and space complexity with derivations
- 🔄 **Pseudocode** - Language-agnostic algorithm descriptions
- 💻 **Implementation Notes** - Java-specific considerations
- 🌍 **Real-World Applications** - How algorithms are used in industry
- ⚖️ **Comparisons** - Trade-offs between related algorithms

---

## 📖 Quick Navigation

### Core Algorithm Categories

| # | Category | Algorithms | Description |
|---|----------|------------|-------------|
| 01 | [**Sorting Algorithms**](./01-sorting-algorithms/README.md) | ~50 | Comparison, non-comparison, and hybrid sorts |
| 02 | [**Searching Algorithms**](./02-searching-algorithms/README.md) | ~33 | Linear, binary, graph traversals |
| 03 | [**Dynamic Programming**](./03-dynamic-programming/README.md) | ~54 | Optimization and counting problems |
| 04 | [**Data Structures**](./04-data-structures/README.md) | ~100+ | Linear, tree, graph, and hash structures |
| 05 | [**Graph Algorithms**](./05-graph-algorithms/README.md) | ~17 | Shortest path, MST, network flow |
| 06 | [**Backtracking**](./06-backtracking/README.md) | ~18 | Constraint satisfaction problems |
| 07 | [**Cryptography**](./07-cryptography/README.md) | ~25+ | Symmetric and asymmetric ciphers |
| 08 | [**Mathematical Algorithms**](./08-mathematical-algorithms/README.md) | ~100+ | Number theory, geometry, statistics |
| 09 | [**String Algorithms**](./09-string-algorithms/README.md) | ~30+ | Pattern matching, manipulation |
| 10 | [**Greedy Algorithms**](./10-greedy-algorithms/README.md) | ~15 | Optimization by local choices |
| 11 | [**Divide and Conquer**](./11-divide-and-conquer/README.md) | ~7 | Recursive problem decomposition |

### Appendix

| Resource | Description |
|----------|-------------|
| [**Complexity Cheatsheet**](./appendix/complexity-cheatsheet.md) | Quick reference for all algorithm complexities |
| [**Glossary**](./appendix/glossary.md) | Definitions of key terms |
| [**References**](./appendix/references.md) | Academic papers and resources |

---

## 🗂️ Algorithm Index by Category

### 01. Sorting Algorithms

<details>
<summary>Click to expand (50 algorithms)</summary>

#### Comparison Sorts
| Algorithm | Time (Avg) | Space | Stable | Link |
|-----------|------------|-------|--------|------|
| Bubble Sort | $O(n^2)$ | $O(1)$ | ✅ | [📄](./01-sorting-algorithms/comparison-sorts/bubble-sort.md) |
| Selection Sort | $O(n^2)$ | $O(1)$ | ❌ | [📄](./01-sorting-algorithms/comparison-sorts/selection-sort.md) |
| Insertion Sort | $O(n^2)$ | $O(1)$ | ✅ | [📄](./01-sorting-algorithms/comparison-sorts/insertion-sort.md) |
| Merge Sort | $O(n \log n)$ | $O(n)$ | ✅ | [📄](./01-sorting-algorithms/comparison-sorts/merge-sort.md) |
| Quick Sort | $O(n \log n)$ | $O(\log n)$ | ❌ | [📄](./01-sorting-algorithms/comparison-sorts/quick-sort.md) |
| Heap Sort | $O(n \log n)$ | $O(1)$ | ❌ | [📄](./01-sorting-algorithms/comparison-sorts/heap-sort.md) |
| Shell Sort | $O(n^{3/2})$ | $O(1)$ | ❌ | [📄](./01-sorting-algorithms/comparison-sorts/shell-sort.md) |

#### Non-Comparison Sorts
| Algorithm | Time (Avg) | Space | Stable | Link |
|-----------|------------|-------|--------|------|
| Counting Sort | $O(n + k)$ | $O(k)$ | ✅ | [📄](./01-sorting-algorithms/non-comparison-sorts/counting-sort.md) |
| Radix Sort | $O(nk)$ | $O(n + k)$ | ✅ | [📄](./01-sorting-algorithms/non-comparison-sorts/radix-sort.md) |
| Bucket Sort | $O(n + k)$ | $O(n)$ | ✅ | [📄](./01-sorting-algorithms/non-comparison-sorts/bucket-sort.md) |

#### Hybrid Sorts
| Algorithm | Time (Avg) | Space | Stable | Link |
|-----------|------------|-------|--------|------|
| Tim Sort | $O(n \log n)$ | $O(n)$ | ✅ | [📄](./01-sorting-algorithms/hybrid-sorts/tim-sort.md) |
| Intro Sort | $O(n \log n)$ | $O(\log n)$ | ❌ | [📄](./01-sorting-algorithms/hybrid-sorts/introspective-sort.md) |

</details>

### 02. Searching Algorithms

<details>
<summary>Click to expand (33 algorithms)</summary>

| Algorithm | Time (Avg) | Space | Link |
|-----------|------------|-------|------|
| Linear Search | $O(n)$ | $O(1)$ | [📄](./02-searching-algorithms/linear-search.md) |
| Binary Search | $O(\log n)$ | $O(1)$ | [📄](./02-searching-algorithms/binary-search.md) |
| Interpolation Search | $O(\log \log n)$ | $O(1)$ | [📄](./02-searching-algorithms/interpolation-search.md) |
| Jump Search | $O(\sqrt{n})$ | $O(1)$ | [📄](./02-searching-algorithms/jump-search.md) |
| Exponential Search | $O(\log n)$ | $O(1)$ | [📄](./02-searching-algorithms/exponential-search.md) |
| Fibonacci Search | $O(\log n)$ | $O(1)$ | [📄](./02-searching-algorithms/fibonacci-search.md) |
| Ternary Search | $O(\log n)$ | $O(1)$ | [📄](./02-searching-algorithms/ternary-search.md) |
| BFS | $O(V + E)$ | $O(V)$ | [📄](./02-searching-algorithms/bfs.md) |
| DFS | $O(V + E)$ | $O(V)$ | [📄](./02-searching-algorithms/dfs.md) |

</details>

### 03. Dynamic Programming

<details>
<summary>Click to expand (54 algorithms)</summary>

| Algorithm | Problem Type | Link |
|-----------|--------------|------|
| Fibonacci | Sequence | [📄](./03-dynamic-programming/fibonacci.md) |
| 0/1 Knapsack | Optimization | [📄](./03-dynamic-programming/knapsack-problem.md) |
| Longest Common Subsequence | String | [📄](./03-dynamic-programming/longest-common-subsequence.md) |
| Edit Distance | String | [📄](./03-dynamic-programming/edit-distance.md) |
| Coin Change | Counting | [📄](./03-dynamic-programming/coin-change.md) |
| Longest Increasing Subsequence | Sequence | [📄](./03-dynamic-programming/longest-increasing-subsequence.md) |
| Matrix Chain Multiplication | Optimization | [📄](./03-dynamic-programming/matrix-chain-multiplication.md) |
| Rod Cutting | Optimization | [📄](./03-dynamic-programming/rod-cutting.md) |

</details>

### 04. Data Structures

<details>
<summary>Click to expand (100+ implementations)</summary>

#### Linear Structures
| Structure | Operations | Link |
|-----------|------------|------|
| Dynamic Array | Insert, Delete, Access | [📄](./04-data-structures/linear/dynamic-array.md) |
| Singly Linked List | Insert, Delete, Traverse | [📄](./04-data-structures/linear/singly-linked-list.md) |
| Doubly Linked List | Insert, Delete, Traverse | [📄](./04-data-structures/linear/doubly-linked-list.md) |
| Stack | Push, Pop, Peek | [📄](./04-data-structures/linear/stack.md) |
| Queue | Enqueue, Dequeue, Peek | [📄](./04-data-structures/linear/queue.md) |
| Deque | Insert/Delete both ends | [📄](./04-data-structures/linear/deque.md) |

#### Tree Structures
| Structure | Operations | Link |
|-----------|------------|------|
| Binary Tree | Insert, Delete, Traverse | [📄](./04-data-structures/trees/binary-tree.md) |
| Binary Search Tree | Insert, Delete, Search | [📄](./04-data-structures/trees/bst.md) |
| AVL Tree | Self-balancing BST | [📄](./04-data-structures/trees/avl-tree.md) |
| Red-Black Tree | Self-balancing BST | [📄](./04-data-structures/trees/red-black-tree.md) |
| B-Tree | Disk-optimized search | [📄](./04-data-structures/trees/b-tree.md) |
| Trie | Prefix search | [📄](./04-data-structures/trees/trie.md) |
| Segment Tree | Range queries | [📄](./04-data-structures/trees/segment-tree.md) |
| Fenwick Tree | Range updates | [📄](./04-data-structures/trees/fenwick-tree.md) |

#### Hashing
| Structure | Operations | Link |
|-----------|------------|------|
| Hash Map | Insert, Delete, Lookup | [📄](./04-data-structures/hashing/hash-map.md) |
| Bloom Filter | Probabilistic membership | [📄](./04-data-structures/hashing/bloom-filter.md) |

</details>

### 05. Graph Algorithms

<details>
<summary>Click to expand (17 algorithms)</summary>

#### Shortest Path
| Algorithm | Time | Use Case | Link |
|-----------|------|----------|------|
| Dijkstra | $O((V+E) \log V)$ | Non-negative weights | [📄](./05-graph-algorithms/shortest-path/dijkstra.md) |
| Bellman-Ford | $O(VE)$ | Negative weights | [📄](./05-graph-algorithms/shortest-path/bellman-ford.md) |
| Floyd-Warshall | $O(V^3)$ | All-pairs | [📄](./05-graph-algorithms/shortest-path/floyd-warshall.md) |
| A* | $O(E)$ | Heuristic search | [📄](./05-graph-algorithms/shortest-path/a-star.md) |

#### Minimum Spanning Tree
| Algorithm | Time | Link |
|-----------|------|------|
| Prim's | $O(E \log V)$ | [📄](./05-graph-algorithms/spanning-tree/prim.md) |
| Kruskal's | $O(E \log E)$ | [📄](./05-graph-algorithms/spanning-tree/kruskal.md) |

#### Network Flow
| Algorithm | Time | Link |
|-----------|------|------|
| Ford-Fulkerson | $O(E \cdot f)$ | [📄](./05-graph-algorithms/flow/ford-fulkerson.md) |
| Edmonds-Karp | $O(VE^2)$ | [📄](./05-graph-algorithms/flow/edmonds-karp.md) |

</details>

---

## 🔍 Search by Problem Type

### Optimization Problems
- [0/1 Knapsack](./03-dynamic-programming/knapsack-problem.md)
- [Coin Change](./03-dynamic-programming/coin-change.md)
- [Activity Selection](./10-greedy-algorithms/activity-selection.md)
- [Traveling Salesman](./05-graph-algorithms/traveling-salesman.md)

### String Problems
- [Pattern Matching (KMP)](./09-string-algorithms/pattern-matching/kmp.md)
- [Longest Common Subsequence](./03-dynamic-programming/longest-common-subsequence.md)
- [Edit Distance](./03-dynamic-programming/edit-distance.md)

### Graph Problems
- [Shortest Path](./05-graph-algorithms/shortest-path/dijkstra.md)
- [Minimum Spanning Tree](./05-graph-algorithms/spanning-tree/prim.md)
- [Graph Coloring](./06-backtracking/m-coloring.md)

### Constraint Satisfaction
- [N-Queens](./06-backtracking/n-queens.md)
- [Sudoku Solver](./06-backtracking/sudoku-solver.md)
- [Hamiltonian Path](./06-backtracking/hamiltonian-cycle.md)

---

## 📊 Complexity Quick Reference

### Sorting Algorithms

```
┌─────────────────────────────────────────────────────────────┐
│ Algorithm        │ Best      │ Average   │ Worst     │ Space │
├─────────────────────────────────────────────────────────────┤
│ Quick Sort       │ O(n lg n) │ O(n lg n) │ O(n²)     │ O(lg n)│
│ Merge Sort       │ O(n lg n) │ O(n lg n) │ O(n lg n) │ O(n)  │
│ Heap Sort        │ O(n lg n) │ O(n lg n) │ O(n lg n) │ O(1)  │
│ Tim Sort         │ O(n)      │ O(n lg n) │ O(n lg n) │ O(n)  │
│ Insertion Sort   │ O(n)      │ O(n²)     │ O(n²)     │ O(1)  │
│ Counting Sort    │ O(n+k)    │ O(n+k)    │ O(n+k)    │ O(k)  │
└─────────────────────────────────────────────────────────────┘
```

### Data Structures

```
┌─────────────────────────────────────────────────────────────┐
│ Structure        │ Access    │ Search    │ Insert    │ Delete │
├─────────────────────────────────────────────────────────────┤
│ Array            │ O(1)      │ O(n)      │ O(n)      │ O(n)   │
│ Linked List      │ O(n)      │ O(n)      │ O(1)      │ O(1)   │
│ Hash Table       │ N/A       │ O(1)      │ O(1)      │ O(1)   │
│ BST (balanced)   │ O(lg n)   │ O(lg n)   │ O(lg n)   │ O(lg n)│
│ Heap             │ O(1)*     │ O(n)      │ O(lg n)   │ O(lg n)│
└─────────────────────────────────────────────────────────────┘
* Min/Max only
```

---

## 🤝 Contributing to Documentation

We welcome contributions! Please follow these guidelines:

### Adding New Algorithm Documentation

1. Use the [TEMPLATE.md](./TEMPLATE.md) as your starting point
2. Place files in the appropriate category folder
3. Update the category README with links
4. Update this index

### Documentation Quality Checklist

- [ ] Mathematical definition provided
- [ ] Complexity analysis with derivation
- [ ] Clear pseudocode
- [ ] Step-by-step example
- [ ] At least 3 real-world applications
- [ ] Comparison with alternatives
- [ ] Edge cases documented
- [ ] References included

See [PLAN.md](./PLAN.md) for the full documentation roadmap.

---

## 📜 License

This documentation is part of the [TheAlgorithms/Java](https://github.com/TheAlgorithms/Java) project, licensed under the MIT License.

---

## 🔗 Quick Links

- [📦 Main Repository](https://github.com/TheAlgorithms/Java)
- [📄 Contributing Guidelines](../CONTRIBUTING.md)
- [📋 Documentation Plan](./PLAN.md)
- [📝 Documentation Template](./TEMPLATE.md)

---

*Last updated: December 29, 2025*
