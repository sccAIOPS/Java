# 0/1 Knapsack Problem

> **Category:** Dynamic Programming  
> **Subcategory:** Optimization Problems  
> **Implementation:** [Knapsack.java](../../src/main/java/com/thealgorithms/dynamicprogramming/Knapsack.java)

---

## 📚 Overview

The **0/1 Knapsack Problem** is a classic combinatorial optimization problem where you must select items with given weights and values to maximize total value while staying within a weight capacity. Each item can either be included (1) or excluded (0), hence "0/1".

---

## 🔢 Mathematical Foundation

### Definition

Given:
- n items with weights $w_1, w_2, ..., w_n$ and values $v_1, v_2, ..., v_n$
- Knapsack capacity $W$

Maximize:
$$
\sum_{i=1}^{n} v_i \cdot x_i
$$

Subject to:
$$
\sum_{i=1}^{n} w_i \cdot x_i \leq W, \quad x_i \in \{0, 1\}
$$

### Key Properties

- **Optimal Substructure:** Optimal solution contains optimal solutions to subproblems
- **Overlapping Subproblems:** Same subproblems are solved multiple times
- **NP-Complete:** No known polynomial-time algorithm for arbitrary weights

### Recurrence Relation

Let $dp[i][w]$ = maximum value achievable with first i items and capacity w:

$$
dp[i][w] = \begin{cases}
0 & \text{if } i = 0 \text{ or } w = 0 \\
dp[i-1][w] & \text{if } w_i > w \\
\max(dp[i-1][w], dp[i-1][w-w_i] + v_i) & \text{otherwise}
\end{cases}
$$

---

## 📊 Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| **Brute Force** | $O(2^n)$ | $O(n)$ |
| **Memoization** | $O(n \cdot W)$ | $O(n \cdot W)$ |
| **Tabulation (2D)** | $O(n \cdot W)$ | $O(n \cdot W)$ |
| **Space Optimized (1D)** | $O(n \cdot W)$ | $O(W)$ |

### Detailed Analysis

The DP solution is **pseudo-polynomial**:
- Polynomial in n and W
- Exponential in the number of bits needed to represent W
- When W is small relative to input size, this is efficient

---

## 🔄 Algorithm (Pseudocode)

### 2D Tabulation
```
ALGORITHM Knapsack2D(weights[], values[], W)
    INPUT: weights array, values array, capacity W
    OUTPUT: Maximum value achievable
    
    1. n ← length(weights)
    2. dp[0..n][0..W] ← 0
    3. FOR i ← 1 TO n DO
    4.     FOR w ← 0 TO W DO
    5.         IF weights[i-1] > w THEN
    6.             dp[i][w] ← dp[i-1][w]
    7.         ELSE
    8.             include ← dp[i-1][w - weights[i-1]] + values[i-1]
    9.             exclude ← dp[i-1][w]
    10.            dp[i][w] ← max(include, exclude)
    11.        END IF
    12.    END FOR
    13. END FOR
    14. RETURN dp[n][W]
```

### Space Optimized (1D Array)
```
ALGORITHM KnapsackOptimized(weights[], values[], W)
    INPUT: weights array, values array, capacity W
    OUTPUT: Maximum value achievable
    
    1. dp[0..W] ← 0
    2. FOR i ← 0 TO n-1 DO
    3.     FOR w ← W DOWN TO weights[i] DO
    4.         dp[w] ← max(dp[w], dp[w - weights[i]] + values[i])
    5.     END FOR
    6. END FOR
    7. RETURN dp[W]
```

### Step-by-Step Walkthrough

**Example:** W = 5, items = [(weight=2, value=3), (weight=3, value=4), (weight=4, value=5)]

```
Capacity:  0    1    2    3    4    5
Item 0:    0    0    0    0    0    0    (no items)
Item 1:    0    0    3    3    3    3    (w=2, v=3)
Item 2:    0    0    3    4    4    7    (w=3, v=4)
Item 3:    0    0    3    4    5    7    (w=4, v=5)

Answer: dp[3][5] = 7 (items 1 and 2: value 3+4=7, weight 2+3=5)
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Input Validation:** Checks for null arrays, mismatched lengths, non-positive weights
- **Space Optimized:** Uses single 1D array iterating backward to avoid overwriting
- **Clean Code:** Immutable parameters with `final` keyword

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/dynamicprogramming/Knapsack.java`

```java
public static int knapSack(final int weightCapacity, final int[] weights, final int[] values) {
    int[] dp = new int[weightCapacity + 1];
    
    for (int i = 0; i < values.length; i++) {
        // Iterate backwards to prevent using updated values
        for (int w = weightCapacity; w > 0; w--) {
            if (weights[i] <= w) {
                dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
            }
        }
    }
    return dp[weightCapacity];
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Resource Allocation
**Use Case:** Cloud computing resource scheduling with budget constraints  
**Example:** AWS Auto Scaling deciding which instance types to provision within budget

### 2. Portfolio Optimization
**Use Case:** Selecting investments to maximize returns within risk budget  
**Example:** Quantitative trading firms use variants for asset selection

### 3. Cargo Loading
**Use Case:** Optimizing container loading for shipping  
**Example:** FedEx, UPS use knapsack variants for truck/plane loading optimization

### 4. Compiler Optimization
**Use Case:** Register allocation with limited registers  
**Example:** GCC compiler's register allocator for minimizing spills

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **Amazon** | Warehouse bin packing optimization |
| **Netflix** | Video encoding quality vs bandwidth allocation |
| **Google** | Ad selection for limited ad slots |
| **Spotify** | Playlist duration fitting |

---

## ⚖️ Comparison with Related Problems

| Problem | Items | Capacity | Objective |
|---------|-------|----------|-----------|
| **0/1 Knapsack** | Each once | Weight | Maximize value |
| **Unbounded Knapsack** | Unlimited | Weight | Maximize value |
| **Fractional Knapsack** | Divisible | Weight | Maximize value |
| **Subset Sum** | Each once | Target sum | Existence |
| **Coin Change** | Unlimited | Target amount | Minimize coins |

### When to Use Each

- **0/1 Knapsack:** Indivisible items, each available once
- **Unbounded:** Multiple copies of items available
- **Fractional:** Items can be divided (Greedy is optimal)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Integer Overflow:** Large values/weights can cause overflow
   - Solution: Use `long` for accumulation or check bounds

2. **Zero Capacity:** W = 0 should return 0
   - Solution: Initialize dp[0] = 0

3. **Forgetting to Iterate Backwards:** Forward iteration in 1D array includes item multiple times
   - Solution: Always iterate `w` from W down to weight[i]

4. **Empty Items Array:** No items to select
   - Solution: Return 0 when n = 0

### Edge Cases to Handle

- [x] Empty items array → returns 0
- [x] W = 0 → returns 0
- [x] All items heavier than W → returns 0
- [x] Negative weights → throws IllegalArgumentException

---

## 📖 References

1. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 16.2
2. Kellerer, H., Pferschy, U., Pisinger, D. "Knapsack Problems" (2004)
3. [Wikipedia: Knapsack Problem](https://en.wikipedia.org/wiki/Knapsack_problem)

---

## 🔗 Related Algorithms

- [Subset Sum](./subset-sum.md) - Decision version
- [Coin Change](./coin-change.md) - Unbounded variant
- [Partition Problem](./partition-problem.md) - Equal subset sum
- [Fractional Knapsack](../10-greedy-algorithms/fractional-knapsack.md) - Greedy solution
