# Greatest Common Divisor (GCD) - Euclidean Algorithm

> **Category:** Mathematical Algorithms  
> **Subcategory:** Number Theory  
> **Implementation:** [`GCD.java`](../../../src/main/java/com/thealgorithms/maths/GCD.java)

---

## 📚 Overview

The Greatest Common Divisor (GCD) algorithm, also known as the Euclidean algorithm, finds the largest positive integer that divides two or more integers without leaving a remainder. This ancient algorithm, described by Euclid around 300 BC, remains one of the most efficient methods for computing GCD.

**Key Characteristics:**
- One of the oldest known algorithms (over 2300 years old)
- Logarithmic time complexity O(log(min(a, b)))
- Foundation for many number theory algorithms
- Basis for extended Euclidean algorithm and modular arithmetic

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** The Greatest Common Divisor of two integers $a$ and $b$, denoted $\gcd(a, b)$, is the largest positive integer $d$ such that $d \mid a$ and $d \mid b$.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Commutativity | Order doesn't matter | $\gcd(a, b) = \gcd(b, a)$ |
| Associativity | Grouping doesn't matter | $\gcd(a, \gcd(b, c)) = \gcd(\gcd(a, b), c)$ |
| Distributivity | Over LCM | $\gcd(a, \text{lcm}(b, c)) = \text{lcm}(\gcd(a, b), \gcd(a, c))$ |
| Reduction | Euclidean property | $\gcd(a, b) = \gcd(b, a \mod b)$ |
| Identity | GCD with zero | $\gcd(a, 0) = |a|$ |

### Mathematical Formulation

**Euclidean Algorithm Recurrence:**

$$
\gcd(a, b) = \begin{cases}
a & \text{if } b = 0 \\
\gcd(b, a \mod b) & \text{otherwise}
\end{cases}
$$

**Bézout's Identity:**

For any integers $a, b$ not both zero, there exist integers $x, y$ such that:

$$
\gcd(a, b) = ax + by
$$

**Relationship with LCM:**

$$
\gcd(a, b) \times \text{lcm}(a, b) = |a \times b|
$$

### Proof of Correctness

**Theorem:** $\gcd(a, b) = \gcd(b, a \mod b)$

**Proof:**
1. Let $d = \gcd(a, b)$. Then $d \mid a$ and $d \mid b$.
2. Since $a = bq + r$ where $r = a \mod b$, we have $r = a - bq$.
3. Since $d \mid a$ and $d \mid b$, it follows that $d \mid (a - bq) = r$.
4. Thus $d \mid \gcd(b, r)$.
5. Conversely, if $d' = \gcd(b, r)$, then $d' \mid b$ and $d' \mid r$.
6. So $d' \mid (bq + r) = a$, meaning $d' \mid \gcd(a, b)$.
7. Therefore $\gcd(a, b) = \gcd(b, r) = \gcd(b, a \mod b)$. ∎

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **Best** | $O(1)$ | One number divides the other |
| **Average** | $O(\log(\min(a, b)))$ | Typical inputs |
| **Worst** | $O(\log(\min(a, b)))$ | Consecutive Fibonacci numbers |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Iterative** | $O(1)$ | Constant extra space |
| **Recursive** | $O(\log(\min(a, b)))$ | Call stack depth |

### Additional Properties

| Property | Value |
|----------|-------|
| **In-place** | Yes (iterative version) |
| **Optimal** | Yes (proven lower bound) |
| **Number of Iterations** | $\leq 5 \times \text{digits}(\min(a, b))$ |

### Detailed Analysis

**Lamé's Theorem:** The number of steps in the Euclidean algorithm never exceeds 5 times the number of digits (base 10) in the smaller number.

**Fibonacci Worst Case:** The worst case occurs when inputs are consecutive Fibonacci numbers $F_n$ and $F_{n+1}$:

$$
\text{Steps} = n = O(\log_\phi(\min(a, b))) = O(\log(\min(a, b)))
$$

where $\phi = \frac{1 + \sqrt{5}}{2} \approx 1.618$ is the golden ratio.

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM GCD-Recursive(a, b)
─────────────────────────────────────────────────────
    INPUT:  Two non-negative integers a, b
    OUTPUT: Greatest common divisor of a and b
─────────────────────────────────────────────────────

    1. IF b = 0 THEN
    2.     RETURN a
    3. ELSE
    4.     RETURN GCD-Recursive(b, a MOD b)
    5. END IF


ALGORITHM GCD-Iterative(a, b)
─────────────────────────────────────────────────────
    INPUT:  Two non-negative integers a, b
    OUTPUT: Greatest common divisor of a and b
─────────────────────────────────────────────────────

    1. WHILE b ≠ 0 DO
    2.     temp ← b
    3.     b ← a MOD b
    4.     a ← temp
    5. END WHILE
    6. RETURN a


ALGORITHM GCD-Multiple(a₁, a₂, ..., aₙ)
─────────────────────────────────────────────────────
    INPUT:  Array of integers [a₁, a₂, ..., aₙ]
    OUTPUT: GCD of all elements
─────────────────────────────────────────────────────

    1. result ← a₁
    2. FOR i ← 2 TO n DO
    3.     result ← GCD(result, aᵢ)
    4.     IF result = 1 THEN
    5.         RETURN 1          // Early termination
    6.     END IF
    7. END FOR
    8. RETURN result
```

### Step-by-Step Walkthrough

**Example Input:** a = 48, b = 18

| Step | a | b | a mod b | Action |
|------|---|---|---------|--------|
| 1 | 48 | 18 | 12 | Continue |
| 2 | 18 | 12 | 6 | Continue |
| 3 | 12 | 6 | 0 | b = 0, return a |

**Result:** $\gcd(48, 18) = 6$

**Visual Representation:**
```
gcd(48, 18)
    │
    └── gcd(18, 12)    ← 48 mod 18 = 12
            │
            └── gcd(12, 6)     ← 18 mod 12 = 6
                    │
                    └── gcd(6, 0)      ← 12 mod 6 = 0
                            │
                            └── return 6
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Provides both recursive and iterative implementations
- Supports varargs for computing GCD of multiple numbers
- Handles edge cases (zero, negative numbers)
- Uses modulo operator for efficient remainder computation

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/maths/GCD.java`

```java
// Key code snippet - Iterative GCD
public static int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// GCD for multiple numbers
public static int gcd(int... numbers) {
    int result = numbers[0];
    for (int i = 1; i < numbers.length; i++) {
        result = gcd(result, numbers[i]);
        if (result == 1) {
            return 1; // Early termination optimization
        }
    }
    return result;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Cryptography
**Use Case:** RSA key generation and modular inverse calculation  
**Example:** Computing $e$ and $d$ in RSA using Extended Euclidean Algorithm

### 2. Fraction Simplification
**Use Case:** Reducing fractions to lowest terms  
**Example:** Displaying $\frac{24}{36}$ as $\frac{2}{3}$

### 3. Aspect Ratio Calculation
**Use Case:** Finding display aspect ratios  
**Example:** 1920×1080 → GCD=120 → 16:9

### 4. Clock and Scheduling
**Use Case:** Finding synchronization periods  
**Example:** LCM of task periods for cyclic scheduling

### 5. Music Theory
**Use Case:** Finding beat synchronization  
**Example:** Polyrhythm calculations (e.g., 3:4 patterns)

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| OpenSSL | RSA cryptographic operations |
| MATLAB/NumPy | Rational number arithmetic |
| Video editors | Aspect ratio calculations |
| Real-time systems | Task scheduling (LCM via GCD) |
| Music software | Beat and tempo synchronization |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Euclidean | Binary GCD | Extended Euclidean |
|--------|-----------|------------|-------------------|
| Time Complexity | O(log n) | O(log n) | O(log n) |
| Operations | Division | Bit shifts | Division |
| Extra Output | None | None | Bézout coefficients |
| Hardware | Division unit | Shift unit | Division unit |
| Best For | General use | Embedded systems | Modular inverse |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Zero Inputs:** $\gcd(a, 0) = |a|$, but $\gcd(0, 0)$ is undefined
2. **Negative Numbers:** Take absolute values first
3. **Integer Overflow:** Intermediate values stay bounded by inputs
4. **Order of Arguments:** Doesn't matter due to commutativity

### Edge Cases to Handle
- [ ] Both inputs are zero (undefined or error)
- [ ] One input is zero (return the other)
- [ ] Negative inputs (use absolute values)
- [ ] Equal inputs ($\gcd(a, a) = a$)
- [ ] One divides the other ($\gcd(a, ka) = a$)

---

## 📖 References

1. Knuth, D.E. (1997). "The Art of Computer Programming, Vol. 2: Seminumerical Algorithms". Addison-Wesley.
2. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
3. [Wikipedia - Euclidean Algorithm](https://en.wikipedia.org/wiki/Euclidean_algorithm)

---

## 🔗 Related Algorithms

- [Extended Euclidean Algorithm](./extended-gcd.md) - Computes Bézout coefficients
- [Binary GCD (Stein's Algorithm)](./binary-gcd.md) - Division-free variant
- [LCM (Least Common Multiple)](./lcm.md) - Related computation
- [Modular Inverse](./modular-inverse.md) - Uses Extended GCD
