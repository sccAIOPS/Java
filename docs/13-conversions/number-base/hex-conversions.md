# Hexadecimal Conversions

> **Category:** Conversions  
> **Subcategory:** Number Base Conversions  
> **Implementations:** [`HexaDecimalToBinary.java`](../../src/main/java/com/thealgorithms/conversions/HexaDecimalToBinary.java), [`HexaDecimalToDecimal.java`](../../src/main/java/com/thealgorithms/conversions/HexaDecimalToDecimal.java)

---

## 📚 Overview

Hexadecimal (base-16) is widely used in computing because it provides a compact representation of binary data. Each hex digit represents exactly 4 binary bits, making conversions between hex and binary straightforward.

**Key Characteristics:**
- Base-16 numeral system
- Digits: 0-9 and A-F
- Each hex digit = 4 binary bits
- Common in memory addresses, color codes, MAC addresses

---

## 🔢 Mathematical Foundation

### Hexadecimal Digits

| Hex | Decimal | Binary |
|-----|---------|--------|
| 0 | 0 | 0000 |
| 1 | 1 | 0001 |
| 2 | 2 | 0010 |
| 3 | 3 | 0011 |
| 4 | 4 | 0100 |
| 5 | 5 | 0101 |
| 6 | 6 | 0110 |
| 7 | 7 | 0111 |
| 8 | 8 | 1000 |
| 9 | 9 | 1001 |
| A | 10 | 1010 |
| B | 11 | 1011 |
| C | 12 | 1100 |
| D | 13 | 1101 |
| E | 14 | 1110 |
| F | 15 | 1111 |

### Conversion Formulas

**Hex to Decimal:**
$$
\text{Decimal} = \sum_{i=0}^{n-1} h_i \times 16^i
$$

**Hex to Binary:**
Replace each hex digit with its 4-bit binary equivalent.

---

## 📊 Complexity Analysis

| Conversion | Time | Space |
|-----------|------|-------|
| Hex → Binary | O(n) | O(4n) |
| Hex → Decimal | O(n) | O(1) |
| Binary → Hex | O(n) | O(n/4) |
| Decimal → Hex | O(log₁₆ n) | O(log₁₆ n) |

---

## 🔄 Algorithms (Pseudocode)

### Hex to Binary
```
ALGORITHM HexToBinary(hex)
─────────────────────────────────────────────────────
    result ← ""
    FOR each digit d IN hex DO
        result ← result + HEX_TO_BIN[d]
    END FOR
    RETURN result (with leading zeros trimmed)
```

### Hex to Decimal
```
ALGORITHM HexToDecimal(hex)
─────────────────────────────────────────────────────
    result ← 0
    FOR i ← 0 TO length(hex) - 1 DO
        digit ← hexCharToValue(hex[i])
        result ← result × 16 + digit
    END FOR
    RETURN result
```

### Binary to Hex
```
ALGORITHM BinaryToHex(binary)
─────────────────────────────────────────────────────
    // Pad to multiple of 4
    binary ← padLeft(binary, 4)
    
    result ← ""
    FOR i ← 0 TO length(binary) - 1 STEP 4 DO
        fourBits ← binary[i..i+3]
        result ← result + BIN_TO_HEX[fourBits]
    END FOR
    RETURN result
```

---

## 💻 Implementation Notes

### Java Implementation

```java
// Hex to Decimal
public static int hexToDecimal(String hex) {
    int result = 0;
    hex = hex.toUpperCase();
    
    for (int i = 0; i < hex.length(); i++) {
        char c = hex.charAt(i);
        int digit = (c >= '0' && c <= '9') ? c - '0' : c - 'A' + 10;
        result = result * 16 + digit;
    }
    return result;
}

// Hex to Binary
public static String hexToBinary(String hex) {
    StringBuilder binary = new StringBuilder();
    String[] lookup = {
        "0000", "0001", "0010", "0011", "0100", "0101", "0110", "0111",
        "1000", "1001", "1010", "1011", "1100", "1101", "1110", "1111"
    };
    
    for (char c : hex.toUpperCase().toCharArray()) {
        int value = (c >= '0' && c <= '9') ? c - '0' : c - 'A' + 10;
        binary.append(lookup[value]);
    }
    
    // Remove leading zeros
    String result = binary.toString().replaceFirst("^0+", "");
    return result.isEmpty() ? "0" : result;
}

// Decimal to Hex
public static String decimalToHex(int decimal) {
    if (decimal == 0) return "0";
    
    StringBuilder hex = new StringBuilder();
    char[] digits = "0123456789ABCDEF".toCharArray();
    
    while (decimal > 0) {
        hex.insert(0, digits[decimal % 16]);
        decimal /= 16;
    }
    return hex.toString();
}
```

### Code References

📁 **Source Files:**
- [`src/main/java/com/thealgorithms/conversions/HexaDecimalToBinary.java`](../../src/main/java/com/thealgorithms/conversions/HexaDecimalToBinary.java)
- [`src/main/java/com/thealgorithms/conversions/HexaDecimalToDecimal.java`](../../src/main/java/com/thealgorithms/conversions/HexaDecimalToDecimal.java)
- [`src/main/java/com/thealgorithms/conversions/DecimalToHexadecimal.java`](../../src/main/java/com/thealgorithms/conversions/DecimalToHexadecimal.java)
- [`src/main/java/com/thealgorithms/conversions/BinaryToHexadecimal.java`](../../src/main/java/com/thealgorithms/conversions/BinaryToHexadecimal.java)

---

## 🌍 Real-World Applications

### 1. Memory Addresses
**Use Case:** Debuggers display addresses in hex (0x7FFF5FBFF8DC)

### 2. Color Codes
**Use Case:** Web colors (#FF5733, #00FF00)

### 3. MAC Addresses
**Use Case:** Network hardware identification (00:1A:2B:3C:4D:5E)

### 4. File Signatures
**Use Case:** Magic numbers identify file types (PDF: 25 50 44 46)

---

## 🔄 Conversion Examples

### Example 1: Hex to Binary
```
Input:  1F
Output: 11111

1 → 0001
F → 1111
Combined: 00011111 → 11111 (leading zeros removed)
```

### Example 2: Hex to Decimal
```
Input:  2A
Output: 42

2 × 16¹ = 32
A × 16⁰ = 10
Total:    42
```

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Case sensitivity | 'a' vs 'A' | Normalize to uppercase |
| Prefix confusion | "0x" prefix | Strip before conversion |
| Overflow | Large hex values | Use long or BigInteger |
| Invalid characters | 'G', 'H' in hex | Validate input |

---

## 📖 References

1. **IEEE 754** - Floating Point Representation
2. **W3C CSS Colors** - Hexadecimal Color Notation

---

## 🔗 Related Algorithms

- [Binary to Decimal](./binary-to-decimal.md)
- [Decimal to Binary](./decimal-to-binary.md)
- [Any Base Conversion](./any-base-conversion.md)

---

*Last updated: December 30, 2025*
