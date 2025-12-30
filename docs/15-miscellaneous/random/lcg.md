# Linear Congruential Generator (LCG)

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Random Number Generation  
> **Implementation:** [`LinearCongruentialGenerator.java`](../../src/main/java/com/thealgorithms/others/LinearCongruentialGenerator.java)

---

## 📚 Overview

The Linear Congruential Generator (LCG) is one of the oldest and best-known pseudorandom number generator algorithms. It produces a sequence of pseudorandom numbers using a piecewise linear equation.

**Key Characteristics:**
- Simple and fast
- Predictable (not cryptographically secure)
- Good statistical properties with proper parameters
- Used in many standard libraries

---

## 🔢 Mathematical Foundation

### Formula

$$
X_{n+1} = (a \times X_n + c) \mod m
$$

Where:
- $X$ = sequence of pseudorandom values
- $m$ = modulus (m > 0)
- $a$ = multiplier (0 < a < m)
- $c$ = increment (0 ≤ c < m)
- $X_0$ = seed (0 ≤ X₀ < m)

### Period

Maximum period = m (achieved when Hull-Dobell theorem conditions are met):
1. c and m are coprime (gcd(c, m) = 1)
2. a - 1 is divisible by all prime factors of m
3. If m is divisible by 4, then a - 1 is also divisible by 4

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Generate next | O(1) | O(1) |
| Initialize | O(1) | O(1) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM LCG(seed, a, c, m)
─────────────────────────────────────────────────────
    INPUT:  seed - initial value
            a - multiplier
            c - increment
            m - modulus
    OUTPUT: Sequence of pseudorandom numbers
─────────────────────────────────────────────────────

    current ← seed
    
    FUNCTION next():
        current ← (a × current + c) MOD m
        RETURN current
    
    FUNCTION nextDouble():
        RETURN next() / m
```

### Example Calculation

**Parameters:** a=5, c=3, m=16, seed=7

```
X₀ = 7
X₁ = (5 × 7 + 3) mod 16 = 38 mod 16 = 6
X₂ = (5 × 6 + 3) mod 16 = 33 mod 16 = 1
X₃ = (5 × 1 + 3) mod 16 = 8 mod 16 = 8
X₄ = (5 × 8 + 3) mod 16 = 43 mod 16 = 11
...
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class LinearCongruentialGenerator {
    private long current;
    private final long a;  // multiplier
    private final long c;  // increment
    private final long m;  // modulus
    
    // Common parameters (MINSTD)
    public static final long DEFAULT_A = 48271L;
    public static final long DEFAULT_C = 0L;
    public static final long DEFAULT_M = 2147483647L;  // 2^31 - 1
    
    public LinearCongruentialGenerator(long seed) {
        this(seed, DEFAULT_A, DEFAULT_C, DEFAULT_M);
    }
    
    public LinearCongruentialGenerator(long seed, long a, long c, long m) {
        this.current = seed;
        this.a = a;
        this.c = c;
        this.m = m;
    }
    
    public long next() {
        current = (a * current + c) % m;
        return current;
    }
    
    public double nextDouble() {
        return (double) next() / m;
    }
    
    public int nextInt(int bound) {
        return (int) (next() % bound);
    }
    
    // Generate array of random numbers
    public long[] generate(int count) {
        long[] result = new long[count];
        for (int i = 0; i < count; i++) {
            result[i] = next();
        }
        return result;
    }
}
```

### Well-Known Parameters

| Name | a | c | m |
|------|---|---|---|
| MINSTD | 48271 | 0 | 2³¹-1 |
| Numerical Recipes | 1664525 | 1013904223 | 2³² |
| Borland C/C++ | 22695477 | 1 | 2³² |
| glibc | 1103515245 | 12345 | 2³¹ |

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/LinearCongruentialGenerator.java`](../../src/main/java/com/thealgorithms/others/LinearCongruentialGenerator.java)

---

## 🌍 Real-World Applications

### 1. Simulation
**Use Case:** Monte Carlo simulations

### 2. Gaming
**Use Case:** Random events, procedural generation

### 3. Statistical Sampling
**Use Case:** Random sample selection

### 4. Testing
**Use Case:** Reproducible random test cases

---

## 📈 Quality Measures

### Spectral Test
Measures lattice structure in higher dimensions:
- Good LCG: Points fill space uniformly
- Bad LCG: Points fall on planes (hyperplane problem)

### Chi-Square Test
Tests uniformity of distribution.

### Serial Correlation
Measures correlation between consecutive values.

---

## ⚠️ Limitations

### NOT Cryptographically Secure

```
// DON'T use for:
String password = generatePassword(lcg);  // Predictable!
byte[] key = generateKey(lcg);            // Insecure!

// DO use for:
int randomPosition = lcg.nextInt(100);    // Games OK
double sample = lcg.nextDouble();          // Simulations OK
```

### Hyperplane Problem

Low-quality parameters cause sequences to fall on hyperplanes when viewed in multiple dimensions.

---

## ⚖️ LCG vs Other PRNGs

| PRNG | Speed | Quality | Period | Cryptographic |
|------|-------|---------|--------|---------------|
| LCG | Fastest | Moderate | m | No |
| Mersenne Twister | Fast | High | 2^19937-1 | No |
| Xorshift | Very Fast | Good | 2^64-1 | No |
| ChaCha20 | Moderate | Excellent | Huge | Yes |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Bad parameters | Short period | Use proven parameters |
| Same seed | Same sequence | Use time-based seed |
| Low bits | Poor randomness | Use high bits |
| Security use | Predictable | Use SecureRandom |
| Overflow | Wrong results | Use long arithmetic |

---

## 📖 References

1. **"The Art of Computer Programming, Vol. 2"** - Knuth
2. **"Random Number Generation and Monte Carlo Methods"** - Gentle
3. **"Tables of Linear Congruential Generators"** - L'Ecuyer (1999)

---

## 🔗 Related Algorithms

- [Mersenne Twister](./mersenne-twister.md)
- [Xorshift](./xorshift.md)
- [Monte Carlo Methods](../../08-mathematical-algorithms/README.md)

---

*Last updated: December 30, 2025*
