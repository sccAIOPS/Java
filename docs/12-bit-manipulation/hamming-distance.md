# Hamming Distance

> **Category:** Bit Manipulation  
> **Subcategory:** Distance & Comparison  
> **Implementation:** [`HammingDistance.java`](../../src/main/java/com/thealgorithms/bitmanipulation/HammingDistance.java)

---

## 📚 Overview

The Hamming distance between two integers is the number of positions at which the corresponding bits are different. Named after Richard Hamming, this metric is fundamental to information theory, coding theory, and has wide applications in error detection and correction.

**Key Characteristics:**
- Measures bit-level difference between two numbers
- Uses XOR operation to find differing bits
- Foundation for error-correcting codes

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** For two integers $a$ and $b$, the Hamming distance $H(a, b)$ is:
> $$H(a, b) = |\{i : a_i \neq b_i\}|$$
> where $a_i$ and $b_i$ are the i-th bits of $a$ and $b$ respectively.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Non-negativity | Distance is always ≥ 0 | $H(a,b) \geq 0$ |
| Identity | Distance to self is 0 | $H(a,a) = 0$ |
| Symmetry | Order doesn't matter | $H(a,b) = H(b,a)$ |
| Triangle Inequality | Standard metric property | $H(a,c) \leq H(a,b) + H(b,c)$ |

### Mathematical Formulation

$$
H(a, b) = \text{popcount}(a \oplus b)
$$

Where $\oplus$ is the XOR operation and popcount counts set bits.

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Description |
|------|------------|-------------|
| **Best** | $O(1)$ | If XOR result is 0 |
| **Average** | $O(k)$ | k = number of different bits |
| **Worst** | $O(\log n)$ | All bits differ |

### Space Complexity

| Type | Complexity |
|------|------------|
| **Auxiliary Space** | $O(1)$ |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM HammingDistance(a, b)
─────────────────────────────────────────────────────
    INPUT:  a, b - two integers
    OUTPUT: number of bit positions where a and b differ
─────────────────────────────────────────────────────

    xor ← a XOR b           // Bits that differ become 1
    count ← 0
    
    WHILE xor > 0 DO
        xor ← xor AND (xor - 1)   // Clear lowest set bit
        count ← count + 1
    END WHILE
    
    RETURN count
```

### Step-by-Step Walkthrough

**Example:** H(5, 3)

| Step | Description | Binary |
|------|-------------|--------|
| a | Input 5 | 0101 |
| b | Input 3 | 0011 |
| a XOR b | Differing bits | 0110 |
| Count bits | popcount(6) | **2** |

```
  5 = 0101
  3 = 0011
  ────────
XOR = 0110  (positions 1 and 2 differ)

Hamming Distance = 2
```

---

## 💻 Implementation Notes

### Java Implementation

```java
/**
 * Calculate Hamming distance between two integers
 * 
 * @param a first integer
 * @param b second integer
 * @return number of differing bit positions
 */
public static int hammingDistance(int a, int b) {
    int xor = a ^ b;
    int count = 0;
    while (xor != 0) {
        xor &= (xor - 1);  // Brian Kernighan's algorithm
        count++;
    }
    return count;
}

// Using built-in bitCount
public static int hammingDistanceBuiltin(int a, int b) {
    return Integer.bitCount(a ^ b);
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/bitmanipulation/HammingDistance.java`](../../src/main/java/com/thealgorithms/bitmanipulation/HammingDistance.java)

📁 **Test File:** [`src/test/java/com/thealgorithms/bitmanipulation/HammingDistanceTest.java`](../../src/test/java/com/thealgorithms/bitmanipulation/HammingDistanceTest.java)

---

## 🌍 Real-World Applications in Software Engineering

### 1. Error Detection & Correction
**Use Case:** ECC memory, Hamming codes  
**Example:** RAM error correction uses Hamming distance to detect and fix bit flips

### 2. DNA Sequence Analysis
**Use Case:** Comparing genetic sequences  
**Example:** Finding mutations between DNA strands

### 3. Image Processing
**Use Case:** Perceptual hashing, image similarity  
**Example:** pHash uses Hamming distance to find similar images

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Google | Image deduplication |
| Shazam | Audio fingerprint matching |
| Intel | ECC memory controllers |
| Bioinformatics | Sequence alignment |

---

## ⚖️ Comparison with Related Algorithms

| Distance Metric | Domain | Description |
|-----------------|--------|-------------|
| Hamming | Bits/Strings (same length) | Bit positions that differ |
| Levenshtein | Strings | Edit operations needed |
| Euclidean | Continuous | Straight-line distance |
| Jaccard | Sets | Set similarity |

---

## ⚠️ Common Pitfalls & Edge Cases

| Pitfall | Description | Solution |
|---------|-------------|----------|
| Negative numbers | Sign bit included | Use unsigned or mask |
| Different lengths | N/A for integers | Fixed 32/64 bit comparison |

### Edge Cases

| Edge Case | Inputs | Expected Output |
|-----------|--------|-----------------|
| Same numbers | (5, 5) | 0 |
| All bits differ | (0, -1) | 32 (for int) |
| One is zero | (0, 7) | 3 |
| Powers of 2 | (4, 8) | 2 |

---

## 📖 References

1. **Richard Hamming** - "Error Detecting and Error Correcting Codes" (1950)
2. **"Introduction to Coding Theory"** - Roth
3. **"Hacker's Delight"** - Warren (popcount)

---

## 🔗 Related Algorithms

- [Count Set Bits](./count-set-bits.md) - Core operation
- [Count Bits Flip](./count-bits-flip.md) - Same operation, different name
- [Gray Code](./gray-code.md) - Hamming distance of 1 between adjacent codes

---

*Last updated: December 30, 2025*
