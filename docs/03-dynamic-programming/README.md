# 🧮 Dynamic Programming

> **Category:** Algorithm Design Paradigm  
> **Difficulty:** Intermediate to Advanced  
> **Prerequisites:** Recursion, Arrays, Basic Complexity Analysis

---

## 📚 Overview

Dynamic Programming (DP) is an algorithmic technique for solving complex problems by breaking them down into simpler subproblems. It stores the results of subproblems to avoid redundant computations, trading space for time.

### Key Principles

1. **Optimal Substructure:** Optimal solution contains optimal solutions to subproblems
2. **Overlapping Subproblems:** Same subproblems are solved multiple times
3. **Memoization/Tabulation:** Store subproblem results for reuse

### When to Use DP

- Problem has optimal substructure
- Recursive solution has overlapping subproblems
- Need to count/find optimal among many possibilities
- Problem asks for minimum/maximum/count

---

## 📊 Classification

```
Dynamic Programming Problems
├── Linear DP
│   ├── Fibonacci Sequence
│   ├── Climbing Stairs
│   ├── House Robber
│   └── Maximum Subarray (Kadane's)
│
├── 2D DP (Grid/String)
│   ├── Longest Common Subsequence
│   ├── Edit Distance
│   ├── Unique Paths
│   └── Minimum Path Sum
│
├── Interval DP
│   ├── Matrix Chain Multiplication
│   ├── Palindromic Partitioning
│   └── Burst Balloons
│
├── Knapsack Variants
│   ├── 0/1 Knapsack
│   ├── Unbounded Knapsack
│   ├── Subset Sum
│   └── Coin Change
│
├── Tree DP
│   ├── Tree Diameter
│   ├── Binary Tree Maximum Path Sum
│   └── Tree Matching
│
└── State Compression DP
    ├── Traveling Salesman (TSP)
    └── Assignment Problem
```

---

## 🔄 Approaches: Top-Down vs Bottom-Up

### Top-Down (Memoization)

```python
def fib_memo(n, memo={}):
    if n in memo: return memo[n]
    if n <= 1: return n
    memo[n] = fib_memo(n-1) + fib_memo(n-2)
    return memo[n]
```

**Pros:** Natural recursive thinking, only computes needed subproblems  
**Cons:** Function call overhead, stack depth limits

### Bottom-Up (Tabulation)

```python
def fib_tab(n):
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

**Pros:** No recursion overhead, often more space-efficient  
**Cons:** Must solve all subproblems, order matters

---

## 📈 Common DP Problems

### Complexity Comparison

| Problem | Time | Space | Space Optimized |
|---------|------|-------|-----------------|
| [Fibonacci](./fibonacci.md) | O(n) | O(n) | O(1) |
| [Climbing Stairs](./climbing-stairs.md) | O(n) | O(n) | O(1) |
| [Longest Common Subsequence](./longest-common-subsequence.md) | O(mn) | O(mn) | O(min(m,n)) |
| [Edit Distance](./edit-distance.md) | O(mn) | O(mn) | O(min(m,n)) |
| [0/1 Knapsack](./knapsack-problem.md) | O(nW) | O(nW) | O(W) |
| [Coin Change](./coin-change.md) | O(nS) | O(S) | O(S) |
| [Longest Increasing Subsequence](./longest-increasing-subsequence.md) | O(n²) | O(n) | O(n log n)* |
| [Matrix Chain Multiplication](./matrix-chain-multiplication.md) | O(n³) | O(n²) | - |

*With binary search optimization

---

## 🎯 Problem-Solving Framework

### Step-by-Step Approach

1. **Identify DP Applicability**
   - Can problem be divided into subproblems?
   - Are subproblems overlapping?
   - Does optimal substructure exist?

2. **Define State**
   - What parameters define a subproblem?
   - `dp[i]`, `dp[i][j]`, `dp[i][j][k]`...

3. **Write Recurrence Relation**
   - How does current state relate to previous states?
   - `dp[i] = f(dp[i-1], dp[i-2], ...)`

4. **Identify Base Cases**
   - What are the smallest subproblems?
   - Initialize DP array

5. **Determine Computation Order**
   - Bottom-up: smaller to larger subproblems
   - Ensure dependencies are computed first

6. **Optimize Space (if needed)**
   - Often only need previous row/few values
   - Rolling array technique

---

## 🔬 Mathematical Foundation

### Optimal Substructure

For a problem with optimal solution OPT:
$$
OPT(problem) = \text{combine}(OPT(subproblem_1), OPT(subproblem_2), ...)
$$

### Overlapping Subproblems Example (Fibonacci)

Without memoization:
$$
T(n) = T(n-1) + T(n-2) + O(1) \approx O(2^n)
$$

With memoization:
$$
T(n) = O(n) \text{ (each subproblem solved once)}
$$

### Bellman Equation (General DP Form)

$$
V(s) = \max_a \left[ R(s,a) + \gamma \sum_{s'} P(s'|s,a) V(s') \right]
$$

---

## 📁 Algorithms in This Section

### Sequence DP

| File | Problem | Status |
|------|---------|--------|
| [fibonacci.md](./fibonacci.md) | Fibonacci Sequence | 📋 Planned |
| [climbing-stairs.md](./climbing-stairs.md) | Climbing Stairs | 📋 Planned |
| [longest-increasing-subsequence.md](./longest-increasing-subsequence.md) | LIS | 📋 Planned |
| [maximum-subarray.md](./maximum-subarray.md) | Kadane's Algorithm | 📋 Planned |

### String DP

| File | Problem | Status |
|------|---------|--------|
| [longest-common-subsequence.md](./longest-common-subsequence.md) | LCS | 📋 Planned |
| [edit-distance.md](./edit-distance.md) | Levenshtein Distance | 📋 Planned |
| [longest-palindromic-substring.md](./longest-palindromic-substring.md) | Longest Palindrome | 📋 Planned |

### Optimization DP

| File | Problem | Status |
|------|---------|--------|
| [knapsack-problem.md](./knapsack-problem.md) | 0/1 Knapsack | 📋 Planned |
| [coin-change.md](./coin-change.md) | Coin Change | 📋 Planned |
| [rod-cutting.md](./rod-cutting.md) | Rod Cutting | 📋 Planned |
| [matrix-chain-multiplication.md](./matrix-chain-multiplication.md) | MCM | 📋 Planned |

---

## 🌍 Real-World Applications

| Application | DP Problem | Company/Product |
|-------------|------------|-----------------|
| Spell checkers | Edit Distance | Google, Microsoft |
| DNA sequence alignment | LCS / Smith-Waterman | NCBI BLAST |
| Resource allocation | Knapsack | AWS, Kubernetes |
| Text justification | Word Wrap | LaTeX, Word |
| Shortest paths | Bellman-Ford, Floyd-Warshall | Google Maps |
| Speech recognition | Viterbi Algorithm | Siri, Alexa |
| Financial optimization | Portfolio optimization | Hedge funds |

---

## 📖 References

1. Cormen, T. H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 15
2. Bellman, R. *"Dynamic Programming"* (1957)
3. Kleinberg, J. & Tardos, E. *"Algorithm Design"*, Chapter 6

---

[← Back to Main Index](../README.md)
