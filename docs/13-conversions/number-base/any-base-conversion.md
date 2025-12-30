# Any Base to Any Base Conversion

> **Category:** Conversions  
> **Subcategory:** Number Base Conversions  
> **Implementation:** [`AnyBaseToAnyBase.java`](../../src/main/java/com/thealgorithms/conversions/AnyBaseToAnyBase.java)

---

## 📚 Overview

Any Base to Any Base conversion is a generalized algorithm that converts numbers between arbitrary numeral systems (bases 2-36). This algorithm typically uses decimal as an intermediate format: first converting the source base to decimal, then converting decimal to the target base.

**Key Characteristics:**
- Supports bases 2 through 36 (digits 0-9 + letters A-Z)
- Two-step process: source → decimal → target
- Fundamental for understanding positional notation

---

## 🔢 Mathematical Foundation

### Positional Notation

For a number in base $b$: $d_{n-1}d_{n-2}...d_1d_0$

$$
\text{Decimal Value} = \sum_{i=0}^{n-1} d_i \times b^i
$$

### Conversion Process

**Step 1: Source Base to Decimal**
$$
\text{decimal} = \sum_{i=0}^{n-1} \text{digit}_i \times \text{source}^i
$$

**Step 2: Decimal to Target Base**
Repeated division by target base, collecting remainders

### Supported Digits

| Base | Digits |
|------|--------|
| 2 (Binary) | 0, 1 |
| 8 (Octal) | 0-7 |
| 10 (Decimal) | 0-9 |
| 16 (Hexadecimal) | 0-9, A-F |
| 36 (Maximum) | 0-9, A-Z |

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n × log(source) + log_target(decimal)) |
| Space | O(log_target(value)) |

Where n is the number of digits in input.

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM AnyBaseToAnyBase(number, sourceBase, targetBase)
─────────────────────────────────────────────────────
    INPUT:  number - string representation
            sourceBase - base of input (2-36)
            targetBase - base of output (2-36)
    OUTPUT: string in target base
─────────────────────────────────────────────────────

    // Step 1: Convert to decimal
    decimal ← toDecimal(number, sourceBase)
    
    // Step 2: Convert to target base
    RETURN fromDecimal(decimal, targetBase)

ALGORITHM toDecimal(number, base)
─────────────────────────────────────────────────────
    result ← 0
    multiplier ← 1
    
    FOR i ← length(number) - 1 DOWNTO 0 DO
        digit ← charToValue(number[i])
        result ← result + digit × multiplier
        multiplier ← multiplier × base
    END FOR
    
    RETURN result

ALGORITHM fromDecimal(decimal, base)
─────────────────────────────────────────────────────
    IF decimal = 0 THEN
        RETURN "0"
    END IF
    
    result ← ""
    WHILE decimal > 0 DO
        remainder ← decimal MOD base
        result ← valueToChar(remainder) + result
        decimal ← decimal DIV base
    END WHILE
    
    RETURN result
```

### Step-by-Step Walkthrough

**Convert "1F" from base 16 to base 2:**

**Step 1: Hex → Decimal**
| Digit | Value | Position | Calculation |
|-------|-------|----------|-------------|
| F | 15 | 0 | 15 × 16⁰ = 15 |
| 1 | 1 | 1 | 1 × 16¹ = 16 |
| **Total** | | | **31** |

**Step 2: Decimal → Binary**
| Division | Quotient | Remainder |
|----------|----------|-----------|
| 31 ÷ 2 | 15 | 1 |
| 15 ÷ 2 | 7 | 1 |
| 7 ÷ 2 | 3 | 1 |
| 3 ÷ 2 | 1 | 1 |
| 1 ÷ 2 | 0 | 1 |

**Result: "11111"**

---

## 💻 Implementation Notes

### Java Implementation

```java
public class AnyBaseToAnyBase {
    
    private static final String DIGITS = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    
    public static String convert(String number, int sourceBase, int targetBase) {
        // Step 1: Convert to decimal
        long decimal = toDecimal(number.toUpperCase(), sourceBase);
        
        // Step 2: Convert to target base
        return fromDecimal(decimal, targetBase);
    }
    
    private static long toDecimal(String number, int base) {
        long result = 0;
        long multiplier = 1;
        
        for (int i = number.length() - 1; i >= 0; i--) {
            int digit = DIGITS.indexOf(number.charAt(i));
            if (digit < 0 || digit >= base) {
                throw new IllegalArgumentException(
                    "Invalid digit for base " + base);
            }
            result += digit * multiplier;
            multiplier *= base;
        }
        return result;
    }
    
    private static String fromDecimal(long decimal, int base) {
        if (decimal == 0) return "0";
        
        StringBuilder result = new StringBuilder();
        while (decimal > 0) {
            result.insert(0, DIGITS.charAt((int)(decimal % base)));
            decimal /= base;
        }
        return result.toString();
    }
}
```

### Code Reference

📁 **Source Files:**
- [`src/main/java/com/thealgorithms/conversions/AnyBaseToAnyBase.java`](../../src/main/java/com/thealgorithms/conversions/AnyBaseToAnyBase.java)
- [`src/main/java/com/thealgorithms/conversions/AnyBaseToDecimal.java`](../../src/main/java/com/thealgorithms/conversions/AnyBaseToDecimal.java)
- [`src/main/java/com/thealgorithms/conversions/DecimalToAnyBase.java`](../../src/main/java/com/thealgorithms/conversions/DecimalToAnyBase.java)

---

## 🌍 Real-World Applications

### 1. Data Encoding
**Use Case:** Converting between hex, base64, and binary

### 2. Color Representations
**Use Case:** RGB (decimal) ↔ Hex color codes

### 3. URL Shorteners
**Use Case:** Base62 encoding for short URLs

### 4. Cryptography
**Use Case:** Various encoding schemes

---

## ⚖️ Common Base Conversions

| From | To | Use Case |
|------|-----|----------|
| Binary | Hex | Memory addresses |
| Decimal | Hex | Color codes |
| Base10 | Base62 | URL shortening |
| Any | Base64 | Data encoding |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Overflow | Large numbers exceed long | Use BigInteger |
| Case sensitivity | 'a' vs 'A' | Normalize case |
| Invalid digits | '9' in base 8 | Validate input |
| Leading zeros | Significant or not? | Document behavior |

---

## 📖 References

1. **"Computer Systems: A Programmer's Perspective"** - Bryant & O'Hallaron
2. **Positional Numeral Systems** - Mathematical History

---

## 🔗 Related Algorithms

- [Binary to Decimal](./binary-to-decimal.md)
- [Decimal to Binary](./decimal-to-binary.md)
- [Hexadecimal Conversions](./hex-conversions.md)

---

*Last updated: December 30, 2025*
