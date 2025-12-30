# Fractional Knapsack Problem

> **Category:** Greedy Algorithms  
> **Subcategory:** Optimization  
> **Implementation:** [`FractionalKnapsack.java`](../../src/main/java/com/thealgorithms/greedyalgorithms/FractionalKnapsack.java)

---

## 📚 Overview

The Fractional Knapsack problem is a classic optimization problem where items can be divided into smaller parts. Given items with weights and values, and a knapsack with limited capacity, the goal is to maximize the total value that can be carried. Unlike the 0/1 Knapsack, items can be taken fractionally, making a greedy solution optimal.

**Key Characteristics:**
- Greedy algorithm with provably optimal solution
- O(n log n) time complexity
- Items can be divided into fractions
- Contrast with 0/1 Knapsack (which requires DP)

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given $n$ items with values $v_i$ and weights $w_i$, and a knapsack capacity $W$, find fractions $x_i \in [0, 1]$ such that $\sum_{i=1}^n x_i \cdot w_i \leq W$ and $\sum_{i=1}^n x_i \cdot v_i$ is maximized.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Value Density | Value per unit weight | $\rho_i = \frac{v_i}{w_i}$ |
| Greedy Choice | Take highest density first | $\arg\max_i\{\rho_i\}$ |
| Feasibility | Total weight ≤ capacity | $\sum x_i w_i \leq W$ |

### Mathematical Formulation

**Objective Function:**

$$
\text{maximize } \sum_{i=1}^{n} x_i \cdot v_i
$$

**Subject to:**

$$
\sum_{i=1}^{n} x_i \cdot w_i \leq W, \quad 0 \leq x_i \leq 1
$$

**Greedy Strategy:**
Sort items by $\frac{v_i}{w_i}$ in descending order, take greedily.

### Proof of Correctness

**Theorem:** The greedy algorithm for Fractional Knapsack produces an optimal solution.

**Proof (Exchange Argument):**
1. Let $X = (x_1, ..., x_n)$ be the greedy solution.
2. Let $Y = (y_1, ..., y_n)$ be any optimal solution.
3. If $X \neq Y$, let $i$ be the first item where $x_i \neq y_i$.
4. Since greedy fills by density: $\rho_i \geq \rho_j$ for all $j > i$.
5. If $x_i > y_i$, then $Y$ has room for more of item $i$ (contradiction).
6. If $x_i < y_i$, we can exchange portion from later items to $i$, improving or maintaining value.
7. This exchange transforms $Y$ towards $X$ without decreasing value.
8. Therefore, greedy solution is optimal. ∎

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **Best** | $O(n)$ | Already sorted by density |
| **Average** | $O(n \log n)$ | Sorting dominates |
| **Worst** | $O(n \log n)$ | Always need to sort |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary Space** | $O(n)$ | Sorted array |
| **In-place Sort** | $O(1)$ | Extra space (excluding sort) |

### Additional Properties

| Property | Value |
|----------|-------|
| **Greedy Algorithm** | Yes |
| **Optimal** | Yes (proven) |
| **Online** | No |

### Detailed Analysis

**Sorting Phase:** $O(n \log n)$
- Sort items by value/weight ratio

**Selection Phase:** $O(n)$
- Single pass through sorted items
- Each item: O(1) to decide and add

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Fractional-Knapsack(items, capacity)
─────────────────────────────────────────────────────
    INPUT:  Array of items (value, weight), capacity W
    OUTPUT: Maximum value and fractions taken
─────────────────────────────────────────────────────

    1. FOR each item i DO
    2.     item[i].ratio ← item[i].value / item[i].weight
    3. END FOR
    
    4. SORT items by ratio in descending order
    
    5. totalValue ← 0
    6. remainingCapacity ← W
    7. fractions ← array of n zeros
    
    8. FOR i ← 0 TO n-1 DO
    9.     IF remainingCapacity = 0 THEN
    10.        BREAK
    11.    END IF
    12.    
    13.    IF items[i].weight ≤ remainingCapacity THEN
    14.        // Take entire item
    15.        fractions[i] ← 1.0
    16.        totalValue ← totalValue + items[i].value
    17.        remainingCapacity ← remainingCapacity - items[i].weight
    18.    ELSE
    19.        // Take fraction of item
    20.        fractions[i] ← remainingCapacity / items[i].weight
    21.        totalValue ← totalValue + fractions[i] × items[i].value
    22.        remainingCapacity ← 0
    23.    END IF
    24. END FOR
    
    25. RETURN totalValue, fractions
```

### Step-by-Step Walkthrough

**Example Input:** Capacity W = 50

| Item | Value | Weight | Ratio (v/w) |
|------|-------|--------|-------------|
| A | 60 | 10 | 6.0 |
| B | 100 | 20 | 5.0 |
| C | 120 | 30 | 4.0 |

**After Sorting by Ratio:**
| Item | Value | Weight | Ratio | Cumulative Weight |
|------|-------|--------|-------|-------------------|
| A | 60 | 10 | 6.0 | 10 |
| B | 100 | 20 | 5.0 | 30 |
| C | 120 | 30 | 4.0 | 60 |

**Selection Process:**

| Step | Item | Remaining Cap | Action | Fraction | Value Added |
|------|------|---------------|--------|----------|-------------|
| 1 | A | 50 | Take all (10 ≤ 50) | 1.0 | 60 |
| 2 | B | 40 | Take all (20 ≤ 40) | 1.0 | 100 |
| 3 | C | 20 | Take 20/30 | 0.667 | 80 |

**Result:** 
- Total Value = 60 + 100 + 80 = **240**
- Fractions: A=1.0, B=1.0, C=0.667

**Visual Representation:**
```
Knapsack Capacity: 50

│███████████│████████████████████│██████████████│░░░░░░░░░░│
│    A=10   │       B=20         │   C=20       │ C=10     │
│   $60     │      $100          │   $80        │ (left)   │
└───────────┴────────────────────┴──────────────┴──────────┘
0          10                    30             50

Total Value: $240
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Calculates value/weight ratio for each item
- Sorts by ratio in descending order
- Handles fractional item at the end of capacity
- Returns total achievable value

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/greedyalgorithms/FractionalKnapsack.java`

```java
// Key code snippet - Fractional Knapsack
public static double fractionalKnapsack(int[] values, int[] weights, int capacity) {
    int n = values.length;
    double[][] items = new double[n][3];
    
    // Calculate ratio and store with original values
    for (int i = 0; i < n; i++) {
        items[i][0] = values[i];
        items[i][1] = weights[i];
        items[i][2] = (double) values[i] / weights[i];
    }
    
    // Sort by ratio descending
    Arrays.sort(items, (a, b) -> Double.compare(b[2], a[2]));
    
    double totalValue = 0;
    int remainingCapacity = capacity;
    
    for (double[] item : items) {
        if (remainingCapacity == 0) break;
        
        if (item[1] <= remainingCapacity) {
            totalValue += item[0];
            remainingCapacity -= (int) item[1];
        } else {
            totalValue += item[2] * remainingCapacity;
            remainingCapacity = 0;
        }
    }
    
    return totalValue;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Resource Allocation
**Use Case:** Allocating bandwidth, CPU time, or memory  
**Example:** Cloud resource allocation among tenants

### 2. Portfolio Optimization
**Use Case:** Investment with partial share purchases  
**Example:** Stock portfolio construction

### 3. Loading Problems
**Use Case:** Loading cargo onto vehicles with weight limits  
**Example:** Logistics and shipping optimization

### 4. Advertisement Scheduling
**Use Case:** Allocating ad slots with varying values  
**Example:** TV/web ad placement optimization

### 5. Memory Management
**Use Case:** Caching with limited memory  
**Example:** Database buffer pool allocation

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Robinhood | Fractional share purchasing |
| AWS | Resource allocation |
| FedEx/UPS | Container loading |
| Google Ads | Ad slot allocation |
| CDNs | Cache content prioritization |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Fractional Knapsack | 0/1 Knapsack | Unbounded Knapsack |
|--------|--------------------|--------------|--------------------|
| Items Divisible | Yes | No | Unlimited copies |
| Approach | Greedy | DP | DP |
| Time Complexity | O(n log n) | O(nW) | O(nW) |
| Space Complexity | O(n) | O(nW) or O(W) | O(W) |
| Optimal | Yes | Yes | Yes |
| Best For | Continuous goods | Discrete items | Unlimited supply |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Division by Zero:** Handle items with zero weight
2. **Floating Point Precision:** Use appropriate precision for fractions
3. **Sorting Stability:** Tie-breaking when ratios are equal
4. **Integer Overflow:** Large values in ratio calculation

### Edge Cases to Handle
- [ ] Empty item set
- [ ] Capacity = 0
- [ ] All items fit (no fractioning needed)
- [ ] Single item larger than capacity
- [ ] Items with zero weight
- [ ] Items with zero value

---

## 📖 References

1. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
2. Dantzig, G.B. (1957). "Discrete-Variable Extremum Problems". Operations Research.
3. [Wikipedia - Continuous Knapsack Problem](https://en.wikipedia.org/wiki/Continuous_knapsack_problem)

---

## 🔗 Related Algorithms

- [0/1 Knapsack Problem](../03-dynamic-programming/knapsack.md) - Indivisible items
- [Activity Selection](./activity-selection.md) - Another greedy algorithm
- [Job Sequencing](./job-sequencing.md) - Deadline-based optimization
- [Huffman Coding](./huffman-coding.md) - Greedy encoding
