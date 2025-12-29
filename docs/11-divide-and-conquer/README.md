# ➗ Divide and Conquer

> **Category:** Algorithm Design Paradigm  
> **Difficulty:** Intermediate  
> **Prerequisites:** Recursion, Basic Complexity Analysis

---

## 📚 Overview

Divide and Conquer algorithms solve problems by:
1. **Divide:** Break problem into smaller subproblems
2. **Conquer:** Solve subproblems recursively
3. **Combine:** Merge subproblem solutions

---

## 📊 Classification

```
Divide and Conquer
├── Sorting
│   ├── Merge Sort
│   └── Quick Sort
│
├── Searching
│   └── Binary Search
│
├── Matrix Operations
│   └── Strassen's Multiplication
│
├── Geometric
│   ├── Closest Pair of Points
│   └── Convex Hull
│
└── Counting
    └── Count Inversions
```

---

## 📈 Key Algorithms

| Algorithm | Time | Space | Recurrence |
|-----------|------|-------|------------|
| [Binary Search](./binary-search.md) | O(log n) | O(1) | T(n) = T(n/2) + O(1) |
| [Merge Sort](./merge-sort.md) | O(n log n) | O(n) | T(n) = 2T(n/2) + O(n) |
| [Quick Sort](./quick-sort.md) | O(n log n)* | O(log n) | T(n) = 2T(n/2) + O(n) |
| [Strassen](./strassen.md) | O(n^2.81) | O(n²) | T(n) = 7T(n/2) + O(n²) |
| [Closest Pair](./closest-pair.md) | O(n log n) | O(n) | T(n) = 2T(n/2) + O(n) |
| [Count Inversions](./count-inversions.md) | O(n log n) | O(n) | T(n) = 2T(n/2) + O(n) |

*Average case

---

## 🔬 Mathematical Foundation

### Master Theorem

For recurrence: $T(n) = aT(n/b) + f(n)$

$$
T(n) = \begin{cases}
\Theta(n^{\log_b a}) & \text{if } f(n) = O(n^{\log_b a - \epsilon}) \\
\Theta(n^{\log_b a} \log n) & \text{if } f(n) = \Theta(n^{\log_b a}) \\
\Theta(f(n)) & \text{if } f(n) = \Omega(n^{\log_b a + \epsilon})
\end{cases}
$$

### Examples

**Merge Sort:** $T(n) = 2T(n/2) + n$
- $a=2, b=2, f(n)=n$
- $n^{\log_2 2} = n^1 = n = f(n)$ → Case 2: $\Theta(n \log n)$

**Strassen:** $T(n) = 7T(n/2) + n^2$
- $a=7, b=2, f(n)=n^2$
- $n^{\log_2 7} \approx n^{2.81} > n^2$ → Case 1: $\Theta(n^{2.81})$

---

## 📁 Algorithms in This Section

| File | Algorithm | Status |
|------|-----------|--------|
| [strassen.md](./strassen.md) | Strassen's Matrix Multiplication | 📋 Planned |
| [closest-pair.md](./closest-pair.md) | Closest Pair of Points | 📋 Planned |
| [count-inversions.md](./count-inversions.md) | Count Inversions | 📋 Planned |
| [karatsuba.md](./karatsuba.md) | Karatsuba Multiplication | 📋 Planned |

---

## 🌍 Real-World Applications

| Algorithm | Application | Example |
|-----------|-------------|---------|
| Merge Sort | External sorting | Database operations |
| Quick Sort | In-memory sorting | Standard libraries |
| Binary Search | Database queries | Index lookups |
| Strassen | Large matrix operations | Scientific computing |
| FFT | Signal processing | Audio/video codecs |

---

## 📖 References

1. Cormen, T. H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 4
2. Kleinberg, J. & Tardos, E. *"Algorithm Design"*, Chapter 5

---

[← Back to Main Index](../README.md)
