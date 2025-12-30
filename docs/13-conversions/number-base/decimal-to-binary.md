# Decimal to Binary Conversion

> **Category:** Conversions  
> **Subcategory:** Number Base Conversions  
> **Implementation:** [`DecimalToBinary.java`](../../src/main/java/com/thealgorithms/conversions/DecimalToBinary.java)

---

## 📚 Overview

Decimal to Binary conversion transforms a number from base-10 (decimal) to base-2 (binary) representation. This conversion uses the repeated division method, dividing by 2 and collecting remainders.

**Key Characteristics:**
- Repeated division by 2
- Remainders form the binary digits (right to left)
- Fundamental to understanding computer storage

---

## 🔢 Mathematical Foundation

### Division Method

For decimal number $n$:
1. Divide $n$ by 2, record remainder
2. Repeat with quotient until quotient = 0
3. Binary = remainders read bottom to top

### Example

Decimal 13:

| Division | Quotient | Remainder |
|----------|----------|-----------|
| 13 ÷ 2 | 6 | 1 (LSB) |
| 6 ÷ 2 | 3 | 0 |
| 3 ÷ 2 | 1 | 1 |
| 1 ÷ 2 | 0 | 1 (MSB) |

Binary = **1101**

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(log n) |
| Space | O(log n) for result |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM DecimalToBinary(decimal)
─────────────────────────────────────────────────────
    INPUT:  decimal - non-negative integer
    OUTPUT: binary string
─────────────────────────────────────────────────────

    IF decimal = 0 THEN
        RETURN "0"
    END IF
    
    binary ← ""
    WHILE decimal > 0 DO
        remainder ← decimal MOD 2
        binary ← remainder + binary    // Prepend
        decimal ← decimal DIV 2
    END WHILE
    
    RETURN binary
```

---

## 💻 Implementation Notes

### Java Implementation

```java
/**
 * Convert decimal to binary string
 */
public static String decimalToBinary(int decimal) {
    if (decimal == 0) return "0";
    
    StringBuilder binary = new StringBuilder();
    while (decimal > 0) {
        binary.insert(0, decimal % 2);
        decimal /= 2;
    }
    return binary.toString();
}

// Using bit operations (more efficient)
public static String decimalToBinaryBitwise(int decimal) {
    if (decimal == 0) return "0";
    
    StringBuilder binary = new StringBuilder();
    while (decimal > 0) {
        binary.insert(0, decimal & 1);
        decimal >>= 1;
    }
    return binary.toString();
}

// Built-in method
public static String decimalToBinaryBuiltin(int decimal) {
    return Integer.toBinaryString(decimal);
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/conversions/DecimalToBinary.java`](../../src/main/java/com/thealgorithms/conversions/DecimalToBinary.java)

---

## 🌍 Real-World Applications

### 1. Debugging
**Use Case:** Examining binary flags and masks

### 2. Network Programming
**Use Case:** Subnet calculations

### 3. Hardware Interfaces
**Use Case:** Register value inspection

---

## ⚖️ Comparison of Methods

| Method | Time | Readability | Use Case |
|--------|------|-------------|----------|
| Division | O(log n) | High | Educational |
| Bitwise | O(log n) | Medium | Performance |
| Built-in | O(log n) | High | Production |

---

## ⚠️ Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Negative numbers | Handle sign separately |
| Zero | Special case |
| Leading zeros | Trim or handle specifically |

---

## 📖 References

1. **"The Art of Computer Programming"** - Knuth, Vol. 2

---

## 🔗 Related Algorithms

- [Binary to Decimal](./binary-to-decimal.md)
- [Decimal to Hexadecimal](./decimal-to-hex.md)
- [Decimal to Any Base](./decimal-to-any-base.md)

---

*Last updated: December 30, 2025*
