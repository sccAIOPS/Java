# 💰 Greedy Algorithms

> **Category:** Algorithm Design Paradigm  
> **Difficulty:** Intermediate  
> **Prerequisites:** Basic Problem-Solving, Sorting

---

## 📚 Overview

Greedy algorithms make locally optimal choices at each step, hoping to find a global optimum. They don't reconsider choices once made, making them efficient but not always optimal.

### When Greedy Works

1. **Greedy Choice Property:** Local optimal choice leads to global optimal
2. **Optimal Substructure:** Optimal solution contains optimal solutions to subproblems

---

## 📊 Classification

```
Greedy Algorithms
├── Scheduling
│   ├── Activity Selection
│   ├── Job Sequencing with Deadlines
│   └── Fractional Knapsack
│
├── Graph Problems
│   ├── Prim's MST
│   ├── Kruskal's MST
│   └── Dijkstra's Shortest Path
│
├── Data Compression
│   └── Huffman Coding
│
└── Optimization
    ├── Coin Change (special cases)
    └── Interval Scheduling
```

---

## 📈 Key Algorithms

| Algorithm | Time | Space | Optimal? |
|-----------|------|-------|----------|
| [Activity Selection](./activity-selection.md) | O(n log n) | O(1) | ✅ Yes |
| [Fractional Knapsack](./fractional-knapsack.md) | O(n log n) | O(1) | ✅ Yes |
| [Huffman Coding](./huffman-coding.md) | O(n log n) | O(n) | ✅ Yes |
| Coin Change | O(n) | O(1) | ⚠️ Sometimes |
| [Job Sequencing](./job-sequencing.md) | O(n² ) | O(n) | ✅ Yes |

---

## 🔬 Mathematical Foundation

### Activity Selection

Given activities with start/finish times, select maximum non-overlapping:

**Greedy Strategy:** Always pick activity with earliest finish time

**Proof of Optimality:**  
Let $A$ be optimal solution, $a_1$ be greedy choice (earliest finish)
- If $a_1 \in A$: greedy choice is compatible
- If $a_1 \notin A$: Replace first activity in $A$ with $a_1$ (still valid)

### Fractional Knapsack

Maximize value with weight constraint:
$$
\max \sum_{i} v_i \cdot x_i \quad \text{subject to} \quad \sum_{i} w_i \cdot x_i \leq W
$$

**Greedy Strategy:** Sort by value/weight ratio, take greedily

---

## 📁 Algorithms in This Section

| File | Algorithm | Status |
|------|-----------|--------|
| [activity-selection.md](./activity-selection.md) | Activity Selection | 📋 Planned |
| [fractional-knapsack.md](./fractional-knapsack.md) | Fractional Knapsack | 📋 Planned |
| [huffman-coding.md](./huffman-coding.md) | Huffman Coding | 📋 Planned |
| [job-sequencing.md](./job-sequencing.md) | Job Sequencing | 📋 Planned |

---

## 🌍 Real-World Applications

| Algorithm | Application | Example |
|-----------|-------------|---------|
| Activity Selection | Room scheduling | Meeting room booking |
| Huffman Coding | Compression | ZIP, JPEG |
| Fractional Knapsack | Resource allocation | Portfolio optimization |
| Dijkstra | Navigation | Google Maps |
| Prim/Kruskal | Network design | ISP routing |

---

## 📖 References

1. Cormen, T. H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 16
2. Kleinberg, J. & Tardos, E. *"Algorithm Design"*, Chapter 4

---

[← Back to Main Index](../README.md)
