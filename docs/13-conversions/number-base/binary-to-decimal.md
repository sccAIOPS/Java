# Binary to Decimal Conversion

> **Category:** Conversions  
> **Subcategory:** Number Base Conversions  
> **Implementation:** [`BinaryToDecimal.java`](../../src/main/java/com/thealgorithms/conversions/BinaryToDecimal.java)

---

## 📚 Overview

Binary to Decimal conversion transforms a number from base-2 (binary) representation to base-10 (decimal) representation. This is a fundamental operation in computer science, as computers internally store all data in binary format.

**Key Characteristics:**
- Positional notation conversion
- Each bit position represents a power of 2
- Foundation for understanding computer number representation

---

## 🔢 Mathematical Foundation

### Definition

For a binary number $B = b_{n-1}b_{n-2}...b_1b_0$ where each $b_i \in \{0, 1\}$:

$$
\text{Decimal} = \sum_{i=0}^{n-1} b_i \times 2^i
$$

### Example

Binary: 1101

$$
1101_2 = 1 \times 2^3 + 1 \times 2^2 + 0 \times 2^1 + 1 \times 2^0 = 8 + 4 + 0 + 1 = 13_{10}
$$

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n) where n = number of binary digits |
| Space | O(1) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM BinaryToDecimal(binary)
─────────────────────────────────────────────────────
    INPUT:  binary - string of 0s and 1s
    OUTPUT: decimal equivalent
─────────────────────────────────────────────────────

    decimal ← 0
    power ← 0
    
    FOR i ← length(binary) - 1 DOWNTO 0 DO
        IF binary[i] = '1' THEN
            decimal ← decimal + 2^power
        END IF
        power ← power + 1
    END FOR
    
    RETURN decimal
```

### Step-by-Step Walkthrough

**Convert 1101 to decimal:**

| Position | Bit | Power | Value | Running Sum |
|----------|-----|-------|-------|-------------|
| 0 (right) | 1 | 2⁰ | 1 | 1 |
| 1 | 0 | 2¹ | 0 | 1 |
| 2 | 1 | 2² | 4 | 5 |
| 3 (left) | 1 | 2³ | 8 | **13** |

---

## 💻 Implementation Notes

### Java Implementation

```java
/**
 * Convert binary string to decimal
 */
public static int binaryToDecimal(String binary) {
    int decimal = 0;
    int power = 0;
    
    for (int i = binary.length() - 1; i >= 0; i--) {
        if (binary.charAt(i) == '1') {
            decimal += (1 << power);  // 2^power using bit shift
        }
        power++;
    }
    return decimal;
}

// Alternative using built-in
public static int binaryToDecimalBuiltin(String binary) {
    return Integer.parseInt(binary, 2);
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/conversions/BinaryToDecimal.java`](../../src/main/java/com/thealgorithms/conversions/BinaryToDecimal.java)

---

## 🌍 Real-World Applications

### 1. Network Configuration
**Use Case:** IP address manipulation (subnets, masks)

### 2. Low-Level Programming
**Use Case:** Reading hardware registers

### 3. Data Compression
**Use Case:** Decoding binary encoded data

---

## ⚠️ Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Integer overflow | Use Long or BigInteger |
| Invalid characters | Validate input |
| Empty string | Handle edge case |

---

## 📖 References

1. **"Computer Organization and Design"** - Patterson & Hennessy

---

## 🔗 Related Algorithms

- [Decimal to Binary](./decimal-to-binary.md)
- [Binary to Hexadecimal](./binary-to-hex.md)
- [Any Base Conversion](./any-base-conversion.md)

---

*Last updated: December 30, 2025*
