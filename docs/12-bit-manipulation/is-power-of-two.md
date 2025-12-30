# Is Power of Two

> **Category:** Bit Manipulation  
> **Subcategory:** Fundamental Operations  
> **Implementation:** [`IsPowerTwo.java`](../../src/main/java/com/thealgorithms/bitmanipulation/IsPowerTwo.java)

---

## 📚 Overview

The "Is Power of Two" algorithm determines whether a given positive integer is a power of 2. This is one of the most elegant applications of bit manipulation, utilizing the property that powers of 2 have exactly one bit set in their binary representation.

**Key Characteristics:**
- Constant time O(1) solution using bit manipulation
- No loops or recursion required
- Fundamental technique used in many optimization scenarios

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** A positive integer $n$ is a power of 2 if and only if $\exists k \in \mathbb{Z}^+ : n = 2^k$

### Key Properties

| Property | Description | Example |
|----------|-------------|---------|
| Single bit | Powers of 2 have exactly one 1-bit | 8 = 1000₂ |
| n & (n-1) = 0 | Clearing lowest bit gives 0 | 8 & 7 = 0 |
| Subtraction pattern | n-1 flips all bits from rightmost 1 | 8-1 = 0111₂ |

### Mathematical Formulation

$$
\text{isPowerOfTwo}(n) = (n > 0) \land ((n \land (n-1)) = 0)
$$

**Proof:**
- If $n = 2^k$, then $n$ has binary form $1\underbrace{00...0}_{k \text{ zeros}}$
- $n - 1$ has form $0\underbrace{11...1}_{k \text{ ones}}$
- $n \land (n-1) = 0$ (no common bits)

**Visual Proof for n = 8:**
```
n     = 1000  (8 in binary)
n-1   = 0111  (7 in binary)
─────────────
n&(n-1)= 0000  (0 - confirms power of 2)
```

**Counterexample for n = 6:**
```
n     = 0110  (6 in binary)
n-1   = 0101  (5 in binary)
─────────────
n&(n-1)= 0100  (4 ≠ 0 - not a power of 2)
```

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | Description |
|------|------------|-------------|
| **All cases** | $O(1)$ | Single bitwise operation |

### Space Complexity

| Type | Complexity |
|------|------------|
| **Auxiliary Space** | $O(1)$ |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM IsPowerOfTwo(n)
─────────────────────────────────────────────────────
    INPUT:  n - integer to check
    OUTPUT: true if n is power of 2, false otherwise
─────────────────────────────────────────────────────

    IF n <= 0 THEN
        RETURN false
    END IF
    
    RETURN (n AND (n - 1)) = 0
```

### Step-by-Step Walkthrough

**Test Cases:**

| n | n (binary) | n-1 (binary) | n & (n-1) | Result |
|---|------------|--------------|-----------|--------|
| 1 | 0001 | 0000 | 0000 | ✅ true |
| 2 | 0010 | 0001 | 0000 | ✅ true |
| 4 | 0100 | 0011 | 0000 | ✅ true |
| 8 | 1000 | 0111 | 0000 | ✅ true |
| 3 | 0011 | 0010 | 0010 | ❌ false |
| 6 | 0110 | 0101 | 0100 | ❌ false |
| 0 | 0000 | - | - | ❌ false |

---

## 💻 Implementation Notes

### Java Implementation

```java
/**
 * Check if n is a power of two using bit manipulation
 * 
 * @param n the number to check
 * @return true if n is a power of 2
 */
public static boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}

// Alternative using Integer.bitCount
public static boolean isPowerOfTwoAlt(int n) {
    return n > 0 && Integer.bitCount(n) == 1;
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/bitmanipulation/IsPowerTwo.java`](../../src/main/java/com/thealgorithms/bitmanipulation/IsPowerTwo.java)

📁 **Test File:** [`src/test/java/com/thealgorithms/bitmanipulation/IsPowerTwoTest.java`](../../src/test/java/com/thealgorithms/bitmanipulation/IsPowerTwoTest.java)

---

## 🌍 Real-World Applications in Software Engineering

### 1. Memory Allocation
**Use Case:** Buffer sizes often required to be powers of 2  
**Example:** Allocating aligned memory in operating systems

### 2. Hash Tables
**Use Case:** Hash table sizes as powers of 2 for fast modulo  
**Example:** `index = hash & (size - 1)` instead of `hash % size`

### 3. Graphics Programming
**Use Case:** Texture dimensions must be powers of 2 (older GPUs)  
**Example:** OpenGL texture requirements

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Linux Kernel | Buddy system memory allocator |
| JDK HashMap | Internal capacity management |
| Game Engines | Texture size validation |
| Network Buffers | MTU size optimization |

---

## ⚖️ Comparison with Related Algorithms

| Method | Time | Space | Readability |
|--------|------|-------|-------------|
| n & (n-1) trick | O(1) | O(1) | Medium |
| Bit count = 1 | O(1)* | O(1) | High |
| Loop divide by 2 | O(log n) | O(1) | High |
| Math.log approach | O(1) | O(1) | High |

*Using hardware instruction

---

## ⚠️ Common Pitfalls & Edge Cases

| Pitfall | Description | Solution |
|---------|-------------|----------|
| Zero input | 0 is not a power of 2 | Check n > 0 |
| Negative numbers | Not valid powers of 2 | Check n > 0 |
| Integer overflow | n-1 on MIN_VALUE | Already handled by n > 0 |

### Edge Cases

| Edge Case | Input | Expected Output |
|-----------|-------|-----------------|
| Zero | 0 | false |
| One | 1 | true (2⁰) |
| Negative | -4 | false |
| Large power | 2³⁰ | true |
| Not power | 2³⁰ + 1 | false |

---

## 📖 References

1. **"Hacker's Delight"** - Henry S. Warren Jr. (Chapter 2)
2. **Bit Twiddling Hacks** - Stanford Graphics

---

## 🔗 Related Algorithms

- [Count Set Bits](./count-set-bits.md) - Alternative check using bitCount
- [Highest Set Bit](./highest-set-bit.md) - Finding the set bit position
- [Next Power of Two](./higher-lower-power-two.md) - Finding nearest power of 2

---

*Last updated: December 30, 2025*
