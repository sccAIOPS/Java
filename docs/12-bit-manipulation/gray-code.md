# Gray Code Conversion

> **Category:** Bit Manipulation  
> **Subcategory:** Bit Transformation  
> **Implementation:** [`GrayCodeConversion.java`](../../src/main/java/com/thealgorithms/bitmanipulation/GrayCodeConversion.java)

---

## 📚 Overview

Gray code (also known as reflected binary code) is a binary numeral system where two successive values differ in only one bit. This property makes Gray code invaluable in digital communications, rotary encoders, and minimizing errors in analog-to-digital converters.

**Key Characteristics:**
- Adjacent codes differ by exactly one bit (Hamming distance = 1)
- No ambiguity during transitions
- O(1) conversion in both directions

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** For a binary number $B = b_{n-1}...b_1b_0$, the Gray code $G = g_{n-1}...g_1g_0$ is defined as:
> $$g_i = b_i \oplus b_{i+1}$$
> where $b_n = 0$ (implied leading zero)

### Key Properties

| Property | Description |
|----------|-------------|
| Single-bit change | Adjacent codes differ by 1 bit |
| Cyclic | First and last codes also differ by 1 bit |
| Unique mapping | Bijective function |

### Conversion Formulas

**Binary to Gray:**
$$G = B \oplus (B \gg 1)$$

**Gray to Binary:**
$$B = G \oplus (G \gg 1) \oplus (G \gg 2) \oplus ... \oplus (G \gg (n-1))$$

Or iteratively:
$$b_i = \bigoplus_{j=i}^{n-1} g_j$$

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Binary to Gray | O(1) | O(1) |
| Gray to Binary | O(log n) | O(1) |

---

## 🔄 Algorithm (Pseudocode)

### Binary to Gray
```
ALGORITHM BinaryToGray(n)
─────────────────────────────────────────────────────
    INPUT:  n - binary number
    OUTPUT: Gray code equivalent
─────────────────────────────────────────────────────

    RETURN n XOR (n >> 1)
```

### Gray to Binary
```
ALGORITHM GrayToBinary(gray)
─────────────────────────────────────────────────────
    INPUT:  gray - Gray code number
    OUTPUT: binary equivalent
─────────────────────────────────────────────────────

    binary ← gray
    mask ← gray >> 1
    
    WHILE mask > 0 DO
        binary ← binary XOR mask
        mask ← mask >> 1
    END WHILE
    
    RETURN binary
```

### Step-by-Step Walkthrough

**Binary to Gray for n = 5 (101):**
```
n       = 101
n >> 1  = 010
─────────────
n XOR   = 111 (Gray code = 7)
```

**3-bit Gray Code Sequence:**

| Decimal | Binary | Gray | Diff from prev |
|---------|--------|------|----------------|
| 0 | 000 | 000 | - |
| 1 | 001 | 001 | bit 0 |
| 2 | 010 | 011 | bit 1 |
| 3 | 011 | 010 | bit 0 |
| 4 | 100 | 110 | bit 2 |
| 5 | 101 | 111 | bit 0 |
| 6 | 110 | 101 | bit 1 |
| 7 | 111 | 100 | bit 0 |

---

## 💻 Implementation Notes

### Java Implementation

```java
/**
 * Convert binary to Gray code
 */
public static int binaryToGray(int n) {
    return n ^ (n >> 1);
}

/**
 * Convert Gray code to binary
 */
public static int grayToBinary(int gray) {
    int binary = gray;
    while (gray > 0) {
        gray >>= 1;
        binary ^= gray;
    }
    return binary;
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/bitmanipulation/GrayCodeConversion.java`](../../src/main/java/com/thealgorithms/bitmanipulation/GrayCodeConversion.java)

---

## 🌍 Real-World Applications in Software Engineering

### 1. Rotary Encoders
**Use Case:** Position sensing without ambiguity  
**Example:** CNC machines, robotic joints

### 2. Error Minimization
**Use Case:** Analog-to-digital conversion  
**Example:** Reduces errors during signal transitions

### 3. Genetic Algorithms
**Use Case:** Chromosome encoding  
**Example:** Minimizes mutation disruption

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Industrial Sensors | Rotary encoders |
| ADC Manufacturers | Signal conversion |
| Karnaugh Maps | Logic minimization |

---

## ⚖️ Comparison: Binary vs Gray Code

| Aspect | Binary | Gray Code |
|--------|--------|-----------|
| Bit changes | Multiple | Single |
| Transition errors | Possible | Minimal |
| Arithmetic | Natural | Requires conversion |
| Hardware cost | Lower | Slightly higher |

---

## ⚠️ Common Pitfalls & Edge Cases

| Edge Case | Binary | Gray |
|-----------|--------|------|
| Zero | 0 | 0 |
| Max 32-bit | 2^31-1 | Different |

---

## 📖 References

1. **Frank Gray** - "Pulse Code Communication" (1953)
2. **"Digital Design"** - Mano & Ciletti

---

## 🔗 Related Algorithms

- [Hamming Distance](./hamming-distance.md) - Gray codes have distance 1
- [Generate Subsets](./generate-subsets.md) - Alternative ordering

---

*Last updated: December 30, 2025*
