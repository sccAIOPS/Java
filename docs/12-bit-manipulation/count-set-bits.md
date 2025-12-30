# Count Set Bits (Popcount)

> **Category:** Bit Manipulation  
> **Subcategory:** Fundamental Operations  
> **Implementation:** [`CountSetBits.java`](../../src/main/java/com/thealgorithms/bitmanipulation/CountSetBits.java)

---

## 📚 Overview

The Count Set Bits algorithm, also known as **Population Count (popcount)** or **Hamming Weight**, counts the number of 1-bits in the binary representation of a number. This fundamental bit manipulation operation has numerous applications in computer science, from cryptography to error correction.

**Key Characteristics:**
- Multiple implementation approaches (naive, Brian Kernighan, lookup table, hardware)
- O(k) where k is the number of set bits (Brian Kernighan)
- Used extensively in low-level programming and optimization

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** For a non-negative integer $n$, the population count $\text{popcount}(n)$ is the number of 1-bits in its binary representation.

$$
\text{popcount}(n) = \sum_{i=0}^{\lfloor \log_2 n \rfloor} \left\lfloor \frac{n}{2^i} \right\rfloor \mod 2
$$

### Key Properties

| Property | Description | Example |
|----------|-------------|---------|
| Additivity (XOR) | popcount(a⊕b) ≤ popcount(a) + popcount(b) | popcount(5⊕3) = popcount(6) = 2 |
| Hamming Distance | popcount(a⊕b) = HammingDistance(a,b) | popcount(5⊕3) = 2 bits differ |
| Power of 2 | popcount(2^k) = 1 | popcount(8) = 1 |

### Mathematical Formulation

The count of set bits can be expressed as:

$$
\text{popcount}(n) = \sum_{i=0}^{31} \text{bit}_i(n)
$$

Where $\text{bit}_i(n) = (n \gg i) \land 1$

---

## 📊 Complexity Analysis

### Time Complexity

| Method | Best | Average | Worst |
|--------|------|---------|-------|
| **Naive (shift and count)** | $O(1)$ | $O(\log n)$ | $O(\log n)$ |
| **Brian Kernighan** | $O(1)$ | $O(k)$ | $O(\log n)$ |
| **Lookup Table** | $O(1)$ | $O(1)$ | $O(1)$ |
| **Hardware (POPCNT)** | $O(1)$ | $O(1)$ | $O(1)$ |

Where $k$ = number of set bits, $n$ = input value

### Space Complexity

| Method | Space |
|--------|-------|
| Naive | $O(1)$ |
| Brian Kernighan | $O(1)$ |
| Lookup Table | $O(2^{8}) = O(256)$ for byte-wise |

---

## 🔄 Algorithm (Pseudocode)

### Method 1: Naive Approach
```
ALGORITHM CountSetBitsNaive(n)
─────────────────────────────────────────────────────
    INPUT:  n - non-negative integer
    OUTPUT: count of 1-bits in n
─────────────────────────────────────────────────────

    count ← 0
    WHILE n > 0 DO
        count ← count + (n AND 1)
        n ← n >> 1          // Right shift by 1
    END WHILE
    RETURN count
```

### Method 2: Brian Kernighan's Algorithm
```
ALGORITHM CountSetBitsKernighan(n)
─────────────────────────────────────────────────────
    INPUT:  n - non-negative integer
    OUTPUT: count of 1-bits in n
─────────────────────────────────────────────────────

    count ← 0
    WHILE n > 0 DO
        n ← n AND (n - 1)   // Clear lowest set bit
        count ← count + 1
    END WHILE
    RETURN count
```

### Step-by-Step Walkthrough (Brian Kernighan)

**Example Input:** n = 13 (binary: 1101)

| Step | n (binary) | n-1 (binary) | n & (n-1) | count |
|------|------------|--------------|-----------|-------|
| 0 | 1101 | - | - | 0 |
| 1 | 1101 | 1100 | 1100 | 1 |
| 2 | 1100 | 1011 | 1000 | 2 |
| 3 | 1000 | 0111 | 0000 | 3 |
| End | 0000 | - | - | **3** |

**Result:** 13 has 3 set bits

---

## 💻 Implementation Notes

### Java Implementation Highlights

```java
/**
 * Brian Kernighan's Algorithm - O(k) where k = set bits
 */
public static int countSetBits(int n) {
    int count = 0;
    while (n != 0) {
        n &= (n - 1);  // Clear the lowest set bit
        count++;
    }
    return count;
}

/**
 * Using Java's built-in method (uses hardware POPCNT when available)
 */
public static int countSetBitsBuiltin(int n) {
    return Integer.bitCount(n);
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/bitmanipulation/CountSetBits.java`](../../src/main/java/com/thealgorithms/bitmanipulation/CountSetBits.java)

📁 **Test File:** [`src/test/java/com/thealgorithms/bitmanipulation/CountSetBitsTest.java`](../../src/test/java/com/thealgorithms/bitmanipulation/CountSetBitsTest.java)

---

## 🌍 Real-World Applications in Software Engineering

### 1. Cryptography
**Use Case:** Calculating Hamming distance for error detection/correction  
**Example:** Used in ECC memory, network protocols

### 2. Database Systems
**Use Case:** Bitmap index operations  
**Example:** PostgreSQL, Oracle use popcount for bitmap intersection queries

### 3. Chess Engines
**Use Case:** Counting pieces on bitboard  
**Example:** Stockfish chess engine uses popcount extensively

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Intel/AMD | Hardware POPCNT instruction |
| PostgreSQL | Bitmap index operations |
| Redis | HyperLogLog cardinality estimation |
| Stockfish | Chess bitboard operations |

---

## ⚖️ Comparison with Related Algorithms

| Method | Time | Space | Hardware Dependent |
|--------|------|-------|-------------------|
| Naive Shift | O(log n) | O(1) | No |
| Brian Kernighan | O(k) | O(1) | No |
| Lookup Table | O(1) | O(256) | No |
| POPCNT instruction | O(1) | O(1) | Yes |

---

## ⚠️ Common Pitfalls & Edge Cases

| Pitfall | Description | Solution |
|---------|-------------|----------|
| Negative numbers | Sign bit handling | Use unsigned or handle separately |
| Zero input | Edge case | Returns 0 correctly |
| Integer overflow | Not applicable | No overflow possible |

### Edge Cases

| Edge Case | Input | Expected Output |
|-----------|-------|-----------------|
| Zero | 0 | 0 |
| All ones (32-bit) | -1 | 32 |
| Power of 2 | 8 | 1 |
| Max int | 2147483647 | 31 |

---

## 📖 References

1. **"Hacker's Delight"** - Henry S. Warren Jr. (Chapter 5)
2. **Brian Kernighan** - Original algorithm description
3. **Intel Intrinsics Guide** - POPCNT instruction

---

## 🔗 Related Algorithms

- [Hamming Distance](./hamming-distance.md) - Uses popcount
- [Is Power of Two](./is-power-of-two.md) - Related bit property
- [Brian Kernighan Algorithm](../15-miscellaneous/bit-counting/brian-kernighan.md)

---

*Last updated: December 30, 2025*
