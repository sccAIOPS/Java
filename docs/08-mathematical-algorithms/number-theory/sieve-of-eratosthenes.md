# Sieve of Eratosthenes

> **Category:** Mathematical Algorithms  
> **Subcategory:** Number Theory / Prime Numbers  
> **Implementation:** [`SieveOfEratosthenes.java`](../../../src/main/java/com/thealgorithms/maths/SieveOfEratosthenes.java)

---

## 📚 Overview

The Sieve of Eratosthenes is an ancient and efficient algorithm for finding all prime numbers up to a given limit. Named after the Greek mathematician Eratosthenes of Cyrene (276-194 BC), it works by iteratively marking the multiples of each prime number starting from 2.

**Key Characteristics:**
- Finds all primes up to n in O(n log log n) time
- Simple yet highly efficient for bulk prime generation
- Memory efficient with bit array optimization
- Foundation for more advanced sieves (Segmented, Linear)

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** A prime number is a natural number greater than 1 that has no positive divisors other than 1 and itself.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Prime Density | Proportion of primes up to n | $\pi(n) \sim \frac{n}{\ln n}$ |
| Sieve Bound | Only check up to square root | Need to mark multiples up to $\sqrt{n}$ |
| Starting Point | Start marking from p² | Smaller multiples already marked |
| Skip Evens | After 2, only odd numbers can be prime | Optimization: skip even indices |

### Mathematical Formulation

**Prime Counting Function:**

$$
\pi(n) = |\{p \leq n : p \text{ is prime}\}| \sim \frac{n}{\ln n}
$$

**Sieve Principle:**
For each prime $p \leq \sqrt{n}$, mark all multiples $p^2, p^2 + p, p^2 + 2p, \ldots$ as composite.

**Number of Operations:**

$$
\sum_{p \leq n, p \text{ prime}} \frac{n}{p} = n \sum_{p \leq n} \frac{1}{p} \approx n \ln \ln n
$$

### Proof of Correctness

**Theorem:** After the sieve completes, an unmarked number $m \leq n$ is prime.

**Proof:**
1. Suppose $m \leq n$ is composite and unmarked.
2. Then $m = ab$ for some $a, b > 1$.
3. Let $p$ be the smallest prime factor of $m$. Then $p \leq \sqrt{m}$.
4. Since $p \leq \sqrt{m} \leq \sqrt{n}$, we would have marked $m$ when processing $p$.
5. Contradiction. Therefore, all unmarked numbers are prime. ∎

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Description |
|------|------------|-------------|
| **All Cases** | $O(n \log \log n)$ | Nearly linear |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Boolean Array** | $O(n)$ | n bits minimum |
| **Bit Array** | $O(n/8)$ | Pack 8 flags per byte |
| **Odds Only** | $O(n/16)$ | Store only odd numbers |

### Additional Properties

| Property | Value |
|----------|-------|
| **In-place** | Sieve array modified in place |
| **Parallelizable** | Yes (segmented sieve) |
| **Cache-Friendly** | No (random access pattern) |

### Detailed Analysis

**Operations Count:**

$$
T(n) = \frac{n}{2} + \frac{n}{3} + \frac{n}{5} + \frac{n}{7} + \ldots = n \sum_{p \leq n} \frac{1}{p}
$$

Using Mertens' theorem: $\sum_{p \leq n} \frac{1}{p} \approx \ln \ln n + M$ where $M \approx 0.2615$

Therefore: $T(n) = O(n \log \log n)$

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Sieve-of-Eratosthenes(n)
─────────────────────────────────────────────────────
    INPUT:  Upper bound n
    OUTPUT: Array of all primes ≤ n
─────────────────────────────────────────────────────

    1. isPrime ← boolean array of size n+1, all TRUE
    2. isPrime[0] ← FALSE
    3. isPrime[1] ← FALSE
    
    4. FOR p ← 2 TO √n DO
    5.     IF isPrime[p] = TRUE THEN
    6.         // Mark all multiples of p as composite
    7.         FOR i ← p² TO n STEP p DO
    8.             isPrime[i] ← FALSE
    9.         END FOR
    10.    END IF
    11. END FOR
    
    12. primes ← empty list
    13. FOR i ← 2 TO n DO
    14.     IF isPrime[i] = TRUE THEN
    15.         primes.append(i)
    16.     END IF
    17. END FOR
    18. RETURN primes


ALGORITHM Count-Primes(n)
─────────────────────────────────────────────────────
    INPUT:  Upper bound n
    OUTPUT: Count of primes ≤ n
─────────────────────────────────────────────────────

    1. isPrime ← Sieve-of-Eratosthenes(n)
    2. count ← 0
    3. FOR i ← 2 TO n DO
    4.     IF isPrime[i] THEN count ← count + 1
    5. END FOR
    6. RETURN count
```

### Step-by-Step Walkthrough

**Example Input:** n = 30

| Step | p | Action | Marked Composite |
|------|---|--------|------------------|
| 1 | 2 | Mark multiples of 2 | 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30 |
| 2 | 3 | Mark multiples of 3 | 9, 15, 21, 27 (others already marked) |
| 3 | 5 | Mark multiples of 5 | 25 (others already marked) |
| Done | - | 5² = 25 > √30 ≈ 5.48 | Stop |

**Visual Sieve Progress:**
```
Initial:  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
After 2:  2  3  ×  5  ×  7  ×  9  × 11  × 13  × 15  × 17  × 19  × 21  × 23  × 25  × 27  × 29  ×
After 3:  2  3  ×  5  ×  7  ×  ×  × 11  × 13  ×  ×  × 17  × 19  ×  ×  × 23  × 25  ×  ×  × 29  ×
After 5:  2  3  ×  5  ×  7  ×  ×  × 11  × 13  ×  ×  × 17  × 19  ×  ×  × 23  ×  ×  ×  ×  × 29  ×

Primes: 2, 3, 5, 7, 11, 13, 17, 19, 23, 29
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses `boolean[]` array for prime flags
- Provides both `findPrimes()` and `countPrimes()` methods
- Outer loop runs only to √n for efficiency
- Inner loop starts from p² to avoid redundant marking

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/maths/SieveOfEratosthenes.java`

```java
// Key code snippet - Sieve Implementation
public static boolean[] findPrimes(int n) {
    boolean[] primes = new boolean[n + 1];
    Arrays.fill(primes, true);
    primes[0] = false;
    primes[1] = false;
    
    for (int i = 2; i * i <= n; i++) {
        if (primes[i]) {
            for (int j = i * i; j <= n; j += i) {
                primes[j] = false;
            }
        }
    }
    return primes;
}

public static int countPrimes(int n) {
    boolean[] primes = findPrimes(n);
    int count = 0;
    for (boolean prime : primes) {
        if (prime) count++;
    }
    return count;
}
```

### Optimizations Available
1. **Skip Even Numbers:** Only store odd indices
2. **Bit Packing:** Use bits instead of bytes
3. **Wheel Factorization:** Skip multiples of 2, 3, 5
4. **Segmented Sieve:** Process in cache-sized chunks

---

## 🌍 Real-World Applications in Software Engineering

### 1. Cryptography
**Use Case:** Generating large prime numbers for RSA  
**Example:** Finding primes in a range for key generation

### 2. Hash Functions
**Use Case:** Choosing prime table sizes for hash maps  
**Example:** Java's HashMap uses powers of 2, but prime sizes reduce collisions

### 3. Number Theory Libraries
**Use Case:** Precomputing primes for fast factorization  
**Example:** SymPy, Mathematica prime number tools

### 4. Competitive Programming
**Use Case:** Solving prime-related problems efficiently  
**Example:** Project Euler problems involving primes

### 5. Random Number Generators
**Use Case:** Prime moduli for linear congruential generators  
**Example:** LCG parameters require prime or power-of-2 moduli

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| OpenSSL | Prime generation for cryptography |
| Wolfram Mathematica | Prime number computations |
| GIMPS | Search for Mersenne primes |
| Java BigInteger | Prime probability testing |
| Database systems | Hash table sizing |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Sieve of Eratosthenes | Sieve of Atkin | Segmented Sieve | Trial Division |
|--------|----------------------|----------------|-----------------|----------------|
| Time Complexity | O(n log log n) | O(n) | O(n log log n) | O(n√n) |
| Space Complexity | O(n) | O(n) | O(√n) | O(1) |
| Implementation | Simple | Complex | Moderate | Trivial |
| Cache Friendly | No | No | Yes | N/A |
| Best For | Medium n | Large n | Very large n | Single test |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Array Size:** Remember n+1 elements for indices 0 to n
2. **Loop Bounds:** Outer loop needs only go to √n
3. **Starting Point:** Start marking from p², not 2p
4. **Integer Overflow:** i*i can overflow for large i

### Edge Cases to Handle
- [ ] n < 2 (no primes)
- [ ] n = 2 (single prime)
- [ ] Very large n (memory constraints)
- [ ] n close to Integer.MAX_VALUE

---

## 📖 References

1. Knuth, D.E. (1997). "The Art of Computer Programming, Vol. 2". Addison-Wesley.
2. Atkin, A.O.L., Bernstein, D.J. (2004). "Prime sieves using binary quadratic forms". Mathematics of Computation.
3. [Wikipedia - Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)

---

## 🔗 Related Algorithms

- [Sieve of Atkin](./sieve-of-atkin.md) - Asymptotically faster sieve
- [Miller-Rabin Primality Test](./miller-rabin.md) - Probabilistic primality testing
- [Prime Factorization](./prime-factorization.md) - Uses sieve for small factors
- [Segmented Sieve](./segmented-sieve.md) - Memory-efficient variant
