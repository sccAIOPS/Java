# Fibonacci Sequence

> **Category:** Dynamic Programming  
> **Subcategory:** Classic Problems  
> **Implementation:** [Fibonacci.java](../../src/main/java/com/thealgorithms/dynamicprogramming/Fibonacci.java)

---

## 📚 Overview

The **Fibonacci Sequence** is a series of numbers where each number is the sum of the two preceding ones, usually starting with 0 and 1. It is one of the most fundamental examples used to demonstrate dynamic programming techniques, including memoization and tabulation.

---

## 🔢 Mathematical Foundation

### Definition

The Fibonacci sequence is defined by the recurrence relation:

$$
F(n) = \begin{cases}
0 & \text{if } n = 0 \\
1 & \text{if } n = 1 \\
F(n-1) + F(n-2) & \text{if } n > 1
\end{cases}
$$

### Key Properties

- **Golden Ratio (φ):** As n approaches infinity, the ratio F(n+1)/F(n) approaches φ ≈ 1.618034
- **Closed Form (Binet's Formula):** $F(n) = \frac{\phi^n - \psi^n}{\sqrt{5}}$ where $\phi = \frac{1+\sqrt{5}}{2}$ and $\psi = \frac{1-\sqrt{5}}{2}$
- **Identity:** $F(n)^2 + F(n+1)^2 = F(2n+1)$

### Recurrence Relation

$$
T(n) = T(n-1) + T(n-2) + O(1)
$$

Without memoization, this results in $O(2^n)$ time complexity (exponential).

---

## 📊 Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| **Naive Recursion** | $O(2^n)$ | $O(n)$ stack |
| **Memoization (Top-Down)** | $O(n)$ | $O(n)$ |
| **Tabulation (Bottom-Up)** | $O(n)$ | $O(n)$ |
| **Space Optimized** | $O(n)$ | $O(1)$ |
| **Binet's Formula** | $O(1)$* | $O(1)$ |
| **Matrix Exponentiation** | $O(\log n)$ | $O(1)$ |

*Note: Binet's formula may have precision issues for large n due to floating-point arithmetic.

### Detailed Analysis

The memoization and tabulation approaches reduce the exponential complexity by storing computed results:
- Each subproblem is solved exactly once
- Total of n+1 subproblems
- Each takes O(1) time after memoization

---

## 🔄 Algorithm (Pseudocode)

### Memoization Approach
```
ALGORITHM FibMemo(n, memo)
    INPUT: Integer n, HashMap memo
    OUTPUT: F(n) - the nth Fibonacci number
    
    1. IF memo contains n THEN
    2.     RETURN memo[n]
    3. END IF
    4. IF n ≤ 1 THEN
    5.     f ← n
    6. ELSE
    7.     f ← FibMemo(n-1, memo) + FibMemo(n-2, memo)
    8.     memo[n] ← f
    9. END IF
    10. RETURN f
```

### Space Optimized Approach
```
ALGORITHM FibOptimized(n)
    INPUT: Integer n ≥ 0
    OUTPUT: F(n) - the nth Fibonacci number
    
    1. IF n = 0 THEN RETURN 0
    2. prev ← 0
    3. curr ← 1
    4. FOR i ← 2 TO n DO
    5.     next ← prev + curr
    6.     prev ← curr
    7.     curr ← next
    8. END FOR
    9. RETURN curr
```

### Step-by-Step Walkthrough

Computing F(5) using tabulation:

```
n:    0   1   2   3   4   5
F(n): 0   1   1   2   3   5
           ↑   ↑
           └───┴── F(2) = F(0) + F(1) = 0 + 1 = 1
               ↑   ↑
               └───┴── F(3) = F(1) + F(2) = 1 + 1 = 2
                   ↑   ↑
                   └───┴── F(4) = F(2) + F(3) = 1 + 2 = 3
                       ↑   ↑
                       └───┴── F(5) = F(3) + F(4) = 2 + 3 = 5
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Memoization:** Uses `HashMap<Integer, Integer>` for caching
- **Negative Input Handling:** Throws `IllegalArgumentException` for n < 0
- **Multiple Approaches:** Provides 4 different implementations

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/dynamicprogramming/Fibonacci.java`

```java
// Memoization approach
public static int fibMemo(int n) {
    if (n < 0) throw new IllegalArgumentException("Input must be non-negative");
    if (CACHE.containsKey(n)) return CACHE.get(n);
    int f = (n <= 1) ? n : fibMemo(n - 1) + fibMemo(n - 2);
    CACHE.put(n, f);
    return f;
}

// Space optimized approach - O(1) space
public static int fibOptimized(int n) {
    if (n == 0) return 0;
    int prev = 0, res = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + res;
        prev = res;
        res = next;
    }
    return res;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Algorithm Analysis & Complexity Studies
**Use Case:** Teaching divide-and-conquer and dynamic programming paradigms  
**Example:** Fibonacci is the canonical example in algorithm courses worldwide to demonstrate memoization

### 2. Financial Modeling
**Use Case:** Technical analysis in stock markets uses Fibonacci retracement levels  
**Example:** Trading platforms like TradingView use Fibonacci ratios (23.6%, 38.2%, 50%, 61.8%) for support/resistance

### 3. Data Structure Design
**Use Case:** Fibonacci heaps provide excellent amortized time for priority queue operations  
**Example:** Dijkstra's algorithm with Fibonacci heaps achieves O(E + V log V) complexity

### 4. Computer Graphics
**Use Case:** Generating natural-looking spirals and patterns  
**Example:** Phyllotaxis patterns in plant simulation, used in games like Spore

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **Bloomberg Terminal** | Fibonacci retracement tools for financial analysis |
| **Apache Cassandra** | Uses Fibonacci-based exponential backoff for retries |
| **Chrome V8 Engine** | Fibonacci heap in garbage collection optimization |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Fibonacci | Lucas Numbers | Tribonacci |
|--------|-----------|---------------|------------|
| Base Cases | F(0)=0, F(1)=1 | L(0)=2, L(1)=1 | T(0)=0, T(1)=0, T(2)=1 |
| Recurrence | F(n-1)+F(n-2) | L(n-1)+L(n-2) | T(n-1)+T(n-2)+T(n-3) |
| Growth Rate | φⁿ | φⁿ | ≈1.839ⁿ |
| Relation | - | L(n)=F(n-1)+F(n+1) | Generalization |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Integer Overflow:** Fibonacci numbers grow exponentially; F(46) exceeds Integer.MAX_VALUE in Java
   - Solution: Use `long` or `BigInteger` for larger values

2. **Stack Overflow (Naive Recursion):** Deep recursion without memoization causes stack overflow
   - Solution: Use iterative approach or increase stack size

3. **Floating Point Precision (Binet's Formula):** Double precision fails for large n
   - Solution: Use integer arithmetic for exact results

### Edge Cases to Handle

- [x] n = 0 → returns 0
- [x] n = 1 → returns 1
- [x] n < 0 → throws IllegalArgumentException
- [x] Large n → consider overflow protection

---

## 📖 References

1. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 15: Dynamic Programming
2. Knuth, D.E. "The Art of Computer Programming", Vol. 1, Section 1.2.8
3. [Wikipedia: Fibonacci Number](https://en.wikipedia.org/wiki/Fibonacci_number)

---

## 🔗 Related Algorithms

- [Tribonacci](./tribonacci.md) - Three-term generalization
- [Climbing Stairs](./climbing-stairs.md) - Classic DP problem
- [Matrix Exponentiation](../../11-divide-and-conquer/matrix-exponentiation.md) - O(log n) Fibonacci
