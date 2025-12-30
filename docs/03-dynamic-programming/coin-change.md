# Coin Change Problem

> **Category:** Dynamic Programming  
> **Subcategory:** Optimization Problems  
> **Implementation:** [CoinChange.java](../../src/main/java/com/thealgorithms/dynamicprogramming/CoinChange.java)

---

## 📚 Overview

The **Coin Change Problem** has two common variants: counting the number of ways to make change for a given amount, and finding the minimum number of coins needed. Both are classic dynamic programming problems demonstrating the unbounded knapsack pattern where coins can be used multiple times.

---

## 🔢 Mathematical Foundation

### Problem Definition

**Variant 1 - Count Ways:** Given coins of denominations $d_1, d_2, ..., d_n$ and amount $A$, count the number of ways to make change.

**Variant 2 - Minimum Coins:** Find the minimum number of coins needed to make amount $A$.

### Recurrence Relations

**Count Ways:**
$$
ways[a] = \sum_{coin \in coins} ways[a - coin] \quad \text{if } a \geq coin
$$

**Minimum Coins:**
$$
minCoins[a] = \begin{cases}
0 & \text{if } a = 0 \\
\infty & \text{if } a < 0 \\
1 + \min_{coin \in coins}(minCoins[a - coin]) & \text{otherwise}
\end{cases}
$$

### Key Properties

- **Unbounded:** Each coin can be used unlimited times
- **Optimal Substructure:** Optimal solution uses optimal solutions to subproblems
- **Order Independence:** {1,2,1} and {1,1,2} are the same combination (for counting)

---

## 📊 Complexity Analysis

| Variant | Time Complexity | Space Complexity |
|---------|-----------------|------------------|
| **Count Ways** | $O(n \cdot A)$ | $O(A)$ |
| **Minimum Coins** | $O(n \cdot A)$ | $O(A)$ |

Where n = number of coin denominations, A = target amount.

### Detailed Analysis

- Each amount from 1 to A is processed
- For each amount, all n coins are checked
- Space optimized to single array since we use unlimited coins

---

## 🔄 Algorithm (Pseudocode)

### Count Ways (Number of Combinations)
```
ALGORITHM CountWays(coins[], amount)
    INPUT: Array of coin denominations, target amount
    OUTPUT: Number of ways to make change
    
    1. ways[0..amount] ← 0
    2. ways[0] ← 1                    // One way to make 0: use no coins
    3. FOR EACH coin IN coins DO
    4.     FOR i ← coin TO amount DO
    5.         ways[i] ← ways[i] + ways[i - coin]
    6.     END FOR
    7. END FOR
    8. RETURN ways[amount]
```

### Minimum Coins
```
ALGORITHM MinimumCoins(coins[], amount)
    INPUT: Array of coin denominations, target amount
    OUTPUT: Minimum number of coins needed
    
    1. minCoins[0..amount] ← ∞
    2. minCoins[0] ← 0
    3. FOR i ← 1 TO amount DO
    4.     FOR EACH coin IN coins DO
    5.         IF coin ≤ i AND minCoins[i - coin] ≠ ∞ THEN
    6.             minCoins[i] ← min(minCoins[i], minCoins[i - coin] + 1)
    7.         END IF
    8.     END FOR
    9. END FOR
    10. RETURN minCoins[amount]
```

### Step-by-Step Walkthrough

**Example:** coins = [1, 2, 5], amount = 11

**Minimum Coins:**
```
Amount:  0   1   2   3   4   5   6   7   8   9   10  11
Min:     0   1   1   2   2   1   2   2   3   3   2   3

Breakdown for amount 11:
- 11 = 5 + 5 + 1 (3 coins)
- Not 11 = 2+2+2+2+2+1 (6 coins)
```

**Count Ways for amount = 5:**
```
Coin 1: ways = [1,1,1,1,1,1]  (all 1s)
Coin 2: ways = [1,1,2,2,3,3]  (add {2}, {2,2}, etc.)
Coin 5: ways = [1,1,2,2,3,4]  (add {5})

4 ways: {1,1,1,1,1}, {1,1,1,2}, {1,2,2}, {5}
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Two Methods:** `change()` for counting, `minimumCoins()` for minimum
- **Integer.MAX_VALUE:** Used as infinity marker for impossible amounts
- **Overflow Check:** Verifies subRes != MAX_VALUE before adding

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/dynamicprogramming/CoinChange.java`

```java
// Count number of ways to make change
public static int change(int[] coins, int amount) {
    int[] combinations = new int[amount + 1];
    combinations[0] = 1;
    
    for (int coin : coins) {
        for (int i = coin; i <= amount; i++) {
            combinations[i] += combinations[i - coin];
        }
    }
    return combinations[amount];
}

// Find minimum number of coins
public static int minimumCoins(int[] coins, int amount) {
    int[] minimumCoins = new int[amount + 1];
    Arrays.fill(minimumCoins, Integer.MAX_VALUE);
    minimumCoins[0] = 0;
    
    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i && minimumCoins[i - coin] != Integer.MAX_VALUE) {
                minimumCoins[i] = Math.min(minimumCoins[i], minimumCoins[i - coin] + 1);
            }
        }
    }
    return minimumCoins[amount];
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Currency/Payment Systems
**Use Case:** ATM cash dispensing algorithms  
**Example:** ATMs determine optimal bill combinations to minimize dispensing time

### 2. Resource Allocation
**Use Case:** Cloud computing resource provisioning  
**Example:** AWS allocating minimum number of instances of different sizes

### 3. Change-Making in POS Systems
**Use Case:** Retail point-of-sale systems  
**Example:** Self-checkout machines calculating optimal change

### 4. Game Development
**Use Case:** Item purchasing systems in games  
**Example:** Calculating if player can afford items with different currency types

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **Square/Stripe** | Payment processing optimization |
| **Coinstar** | Coin counting machines |
| **Vending machines** | Change calculation |
| **Mobile games** | In-app purchase currency systems |

---

## ⚖️ Comparison with Related Problems

| Problem | Item Usage | Goal | Approach |
|---------|------------|------|----------|
| **Coin Change (Ways)** | Unlimited | Count combinations | DP |
| **Coin Change (Min)** | Unlimited | Minimize count | DP |
| **0/1 Knapsack** | Once each | Maximize value | DP |
| **Subset Sum** | Once each | Existence check | DP |
| **Partition Problem** | Once each | Equal halves | DP |

### Why Loop Order Matters

**For Combinations (not permutations):**
```java
// Outer: coins, Inner: amounts → Combinations only
for (coin : coins)
    for (i = coin; i <= amount; i++)
```

**For Permutations:**
```java
// Outer: amounts, Inner: coins → Counts permutations
for (i = 1; i <= amount; i++)
    for (coin : coins)
```

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Impossible Amount:** No way to make change with given coins
   - Solution: Return MAX_VALUE/0 or -1 based on problem variant

2. **Integer Overflow:** Adding to MAX_VALUE causes overflow
   - Solution: Check for MAX_VALUE before adding

3. **Empty Coins Array:** No coins provided
   - Solution: Return 0 ways or MAX_VALUE for minimum

4. **Permutation vs Combination:** Counting same coins in different orders
   - Solution: Iterate coins in outer loop for combinations

### Edge Cases to Handle

- [x] amount = 0 → 1 way (empty set), 0 coins minimum
- [x] coins = [] → 0 ways, impossible
- [x] Single coin larger than amount → 0 ways
- [x] coin = 1 exists → always possible

---

## 📖 References

1. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 16
2. Bellman, R. "Dynamic Programming" (1957)
3. [Wikipedia: Change-making Problem](https://en.wikipedia.org/wiki/Change-making_problem)

---

## 🔗 Related Algorithms

- [0/1 Knapsack](./knapsack.md) - Limited item usage
- [Unbounded Knapsack](./unbounded-knapsack.md) - Same pattern
- [Subset Sum](./subset-sum.md) - Boolean version
- [Rod Cutting](./rod-cutting.md) - Similar unbounded structure
