# 📚 TheAlgorithms/Java - Documentation

> **Comprehensive Algorithm Documentation**  
> Mathematical foundations, pseudocode, complexity analysis, and real-world applications

---

## 🎯 About This Documentation

This documentation provides in-depth analysis of every algorithm implemented in the TheAlgorithms/Java repository. Each algorithm documentation includes:

- **Mathematical Foundation** - Formal definitions, formulas, and proofs
- **Pseudocode** - Language-agnostic algorithm representation
- **Complexity Analysis** - Time and space complexity with derivations
- **Real-World Applications** - Practical use cases in software engineering
- **Implementation Notes** - Java-specific details and optimizations

---

## 📖 Documentation Index

### Core Algorithm Categories

| # | Category | Description | Algorithms |
|---|----------|-------------|------------|
| 01 | [**Sorting Algorithms**](./01-sorting-algorithms/README.md) | Comparison, distribution, and hybrid sorts | ~50 |
| 02 | [**Searching Algorithms**](./02-searching-algorithms/README.md) | Linear, binary, graph traversals | ~33 |
| 03 | [**Dynamic Programming**](./03-dynamic-programming/README.md) | Optimization and counting problems | ~54 |
| 04 | [**Data Structures**](./04-data-structures/README.md) | Linear, trees, graphs, hashing | ~100+ |
| 05 | [**Graph Algorithms**](./05-graph-algorithms/README.md) | Shortest path, MST, flow networks | ~40 |
| 06 | [**Backtracking**](./06-backtracking/README.md) | Constraint satisfaction, puzzles | ~18 |
| 07 | [**Cryptography**](./07-cryptography/README.md) | Symmetric and asymmetric ciphers | ~25 |
| 08 | [**Mathematical Algorithms**](./08-mathematical-algorithms/README.md) | Number theory, geometry, statistics | ~100+ |
| 09 | [**String Algorithms**](./09-string-algorithms/README.md) | Pattern matching, manipulation | ~30 |
| 10 | [**Greedy Algorithms**](./10-greedy-algorithms/README.md) | Optimization by local choices | ~15 |
| 11 | [**Divide and Conquer**](./11-divide-and-conquer/README.md) | Recursive problem decomposition | ~7 |

### Additional Resources

| Resource | Description |
|----------|-------------|
| [**Complexity Cheatsheet**](./appendix/complexity-cheatsheet.md) | Quick reference for all algorithm complexities |
| [**Glossary**](./appendix/glossary.md) | Definitions of key terms |
| [**References**](./appendix/references.md) | Academic papers and resources |
| [**Contributing Guide**](./TEMPLATE.md) | Template for adding new documentation |

---

## 🗺️ Quick Navigation by Algorithm Type

### Sorting Algorithms

<details>
<summary><b>Comparison-Based Sorts</b></summary>

| Algorithm | Time (Best) | Time (Avg) | Time (Worst) | Space | Stable |
|-----------|-------------|------------|--------------|-------|--------|
| [Bubble Sort](./01-sorting-algorithms/comparison-sorts/bubble-sort.md) | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| [Selection Sort](./01-sorting-algorithms/comparison-sorts/selection-sort.md) | O(n²) | O(n²) | O(n²) | O(1) | ❌ |
| [Insertion Sort](./01-sorting-algorithms/comparison-sorts/insertion-sort.md) | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| [Merge Sort](./01-sorting-algorithms/comparison-sorts/merge-sort.md) | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ |
| [Quick Sort](./01-sorting-algorithms/comparison-sorts/quick-sort.md) | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ |
| [Heap Sort](./01-sorting-algorithms/comparison-sorts/heap-sort.md) | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ |

</details>

<details>
<summary><b>Non-Comparison Sorts</b></summary>

| Algorithm | Time | Space | Best For |
|-----------|------|-------|----------|
| [Counting Sort](./01-sorting-algorithms/non-comparison-sorts/counting-sort.md) | O(n + k) | O(k) | Small range integers |
| [Radix Sort](./01-sorting-algorithms/non-comparison-sorts/radix-sort.md) | O(d(n + k)) | O(n + k) | Fixed-length integers |
| [Bucket Sort](./01-sorting-algorithms/non-comparison-sorts/bucket-sort.md) | O(n + k) | O(n) | Uniformly distributed |

</details>

<details>
<summary><b>Hybrid Sorts</b></summary>

| Algorithm | Description |
|-----------|-------------|
| [Tim Sort](./01-sorting-algorithms/hybrid-sorts/tim-sort.md) | Merge + Insertion (Python/Java default) |
| [Introspective Sort](./01-sorting-algorithms/hybrid-sorts/introspective-sort.md) | Quick + Heap + Insertion |

</details>

### Searching Algorithms

<details>
<summary><b>Array/List Searches</b></summary>

| Algorithm | Time | Space | Prerequisite |
|-----------|------|-------|--------------|
| [Linear Search](./02-searching-algorithms/linear-search.md) | O(n) | O(1) | None |
| [Binary Search](./02-searching-algorithms/binary-search.md) | O(log n) | O(1) | Sorted array |
| [Interpolation Search](./02-searching-algorithms/interpolation-search.md) | O(log log n)* | O(1) | Sorted, uniform |
| [Fibonacci Search](./02-searching-algorithms/fibonacci-search.md) | O(log n) | O(1) | Sorted array |

</details>

<details>
<summary><b>Graph Traversals</b></summary>

| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| [BFS](./02-searching-algorithms/bfs.md) | O(V + E) | O(V) | Shortest path (unweighted) |
| [DFS](./02-searching-algorithms/dfs.md) | O(V + E) | O(V) | Path finding, connectivity |

</details>

### Graph Algorithms

<details>
<summary><b>Shortest Path</b></summary>

| Algorithm | Time | Space | Handles Negative |
|-----------|------|-------|------------------|
| [Dijkstra](./05-graph-algorithms/shortest-path/dijkstra.md) | O((V+E) log V) | O(V) | ❌ |
| [Bellman-Ford](./05-graph-algorithms/shortest-path/bellman-ford.md) | O(VE) | O(V) | ✅ |
| [Floyd-Warshall](./05-graph-algorithms/shortest-path/floyd-warshall.md) | O(V³) | O(V²) | ✅ |
| [A*](./05-graph-algorithms/shortest-path/a-star.md) | O(E) | O(V) | ❌ |

</details>

<details>
<summary><b>Minimum Spanning Tree</b></summary>

| Algorithm | Time | Space | Best For |
|-----------|------|-------|----------|
| [Prim](./05-graph-algorithms/spanning-tree/prim.md) | O(E log V) | O(V) | Dense graphs |
| [Kruskal](./05-graph-algorithms/spanning-tree/kruskal.md) | O(E log E) | O(V) | Sparse graphs |

</details>

---

## 📊 Complexity Quick Reference

### Time Complexity Hierarchy

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

### Visual Scale (n = 1,000,000)

| Complexity | Operations | Time (1 GHz) |
|------------|------------|--------------|
| O(1) | 1 | 1 ns |
| O(log n) | 20 | 20 ns |
| O(n) | 1,000,000 | 1 ms |
| O(n log n) | 20,000,000 | 20 ms |
| O(n²) | 10¹² | 16.7 min |
| O(2ⁿ) | ∞ | Heat death of universe |

---

## 🎓 Learning Paths

### Beginner Path
1. [Linear Search](./02-searching-algorithms/linear-search.md)
2. [Binary Search](./02-searching-algorithms/binary-search.md)
3. [Bubble Sort](./01-sorting-algorithms/comparison-sorts/bubble-sort.md)
4. [Selection Sort](./01-sorting-algorithms/comparison-sorts/selection-sort.md)
5. [Insertion Sort](./01-sorting-algorithms/comparison-sorts/insertion-sort.md)

### Intermediate Path
1. [Merge Sort](./01-sorting-algorithms/comparison-sorts/merge-sort.md)
2. [Quick Sort](./01-sorting-algorithms/comparison-sorts/quick-sort.md)
3. [BFS & DFS](./02-searching-algorithms/bfs.md)
4. [Dynamic Programming Intro](./03-dynamic-programming/README.md)
5. [Basic Data Structures](./04-data-structures/README.md)

### Advanced Path
1. [Graph Algorithms](./05-graph-algorithms/README.md)
2. [Advanced DP](./03-dynamic-programming/README.md)
3. [String Algorithms](./09-string-algorithms/README.md)
4. [Cryptography](./07-cryptography/README.md)

---

## 🔧 How to Use This Documentation

### For Learning
1. Start with the category README for overview
2. Read the mathematical foundation section
3. Study the pseudocode before implementation
4. Work through step-by-step examples
5. Review real-world applications

### For Reference
1. Use the complexity cheatsheet for quick lookups
2. Check comparison tables to choose algorithms
3. Review edge cases before implementation

### For Contributing
1. Use [TEMPLATE.md](./TEMPLATE.md) as your starting point
2. Follow the quality checklist in [PLAN.md](./PLAN.md)
3. Include all required sections
4. Add real-world examples

---

## 📈 Documentation Progress

| Category | Status | Progress |
|----------|--------|----------|
| Sorting Algorithms | 🚧 In Progress | ░░░░░░░░░░ 0% |
| Searching Algorithms | 🚧 In Progress | ░░░░░░░░░░ 0% |
| Dynamic Programming | 📋 Planned | ░░░░░░░░░░ 0% |
| Data Structures | 📋 Planned | ░░░░░░░░░░ 0% |
| Graph Algorithms | 📋 Planned | ░░░░░░░░░░ 0% |
| Backtracking | 📋 Planned | ░░░░░░░░░░ 0% |
| Cryptography | 📋 Planned | ░░░░░░░░░░ 0% |
| Mathematical | 📋 Planned | ░░░░░░░░░░ 0% |
| String Algorithms | 📋 Planned | ░░░░░░░░░░ 0% |
| Greedy Algorithms | 📋 Planned | ░░░░░░░░░░ 0% |
| Divide & Conquer | 📋 Planned | ░░░░░░░░░░ 0% |

---

## 🤝 Contributing

We welcome contributions! To add or improve documentation:

1. Check [PLAN.md](./PLAN.md) for the documentation plan
2. Use [TEMPLATE.md](./TEMPLATE.md) for new algorithm docs
3. Follow the quality checklist
4. Submit a pull request

See [CONTRIBUTING.md](../CONTRIBUTING.md) for general contribution guidelines.

---

## 📜 License

This documentation is part of TheAlgorithms/Java project and is licensed under the MIT License.

---

*Last updated: December 29, 2025*
