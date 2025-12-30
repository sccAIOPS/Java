# Luhn Algorithm (Mod 10)

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Validation  
> **Implementation:** [`Luhn.java`](../../src/main/java/com/thealgorithms/others/Luhn.java)

---

## 📚 Overview

The Luhn algorithm, also known as the "modulus 10" algorithm, is a checksum formula used to validate identification numbers such as credit card numbers, IMEI numbers, and national identification numbers.

**Key Characteristics:**
- Simple single-digit error detection
- Detects transposition of adjacent digits
- Fast O(n) validation
- Used in credit cards, IMEI, ISIN

---

## 🔢 Mathematical Foundation

### Algorithm Steps

1. From rightmost digit (check digit), double every second digit
2. If doubling results in > 9, subtract 9
3. Sum all digits
4. Valid if sum mod 10 = 0

### Formula

For digits $d_1, d_2, ..., d_n$:
$$
\left(\sum_{i=1}^{n} f(d_i, i)\right) \mod 10 = 0
$$

Where:
$$
f(d, i) = 
\begin{cases}
d & \text{if } i \text{ is odd (from right)} \\
d \times 2 - 9 & \text{if } i \text{ is even and } d \times 2 > 9 \\
d \times 2 & \text{if } i \text{ is even and } d \times 2 \leq 9
\end{cases}
$$

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Validate | O(n) | O(1) |
| Generate check digit | O(n) | O(1) |

---

## 🔄 Algorithm (Pseudocode)

### Validation
```
ALGORITHM LuhnValidate(number)
─────────────────────────────────────────────────────
    INPUT:  number - string of digits
    OUTPUT: TRUE if valid, FALSE otherwise
─────────────────────────────────────────────────────

    sum ← 0
    isEven ← FALSE  // Position parity (from right)
    
    FOR i ← length(number) - 1 DOWNTO 0 DO
        digit ← number[i] - '0'
        
        IF isEven THEN
            digit ← digit × 2
            IF digit > 9 THEN
                digit ← digit - 9
            END IF
        END IF
        
        sum ← sum + digit
        isEven ← NOT isEven
    END FOR
    
    RETURN sum MOD 10 = 0
```

### Check Digit Generation
```
ALGORITHM LuhnCheckDigit(partialNumber)
─────────────────────────────────────────────────────
    // Append 0 as placeholder
    checkDigit ← 0
    
    // Calculate what check digit should be
    FOR checkDigit ← 0 TO 9 DO
        IF LuhnValidate(partialNumber + checkDigit) THEN
            RETURN checkDigit
        END IF
    END FOR
```

### Step-by-Step Example

**Credit Card:** 4539 1488 0343 6467

**Step 1:** Process from right, double every second
```
Original: 4  5  3  9  1  4  8  8  0  3  4  3  6  4  6  7
Position: 16 15 14 13 12 11 10 9  8  7  6  5  4  3  2  1
Double:   8  5  6  9  2  4  16 8  0  3  8  3  12 4  12 7
After-9:  8  5  6  9  2  4  7  8  0  3  8  3  3  4  3  7
```

**Step 2:** Sum all
```
Sum = 8+5+6+9+2+4+7+8+0+3+8+3+3+4+3+7 = 80
```

**Step 3:** Check
```
80 mod 10 = 0 ✓ Valid!
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class Luhn {
    
    public static boolean isValid(String number) {
        int sum = 0;
        boolean alternate = false;
        
        // Process from right to left
        for (int i = number.length() - 1; i >= 0; i--) {
            int digit = number.charAt(i) - '0';
            
            if (alternate) {
                digit *= 2;
                if (digit > 9) {
                    digit -= 9;
                }
            }
            
            sum += digit;
            alternate = !alternate;
        }
        
        return sum % 10 == 0;
    }
    
    public static int generateCheckDigit(String partialNumber) {
        // Calculate sum without check digit
        int sum = 0;
        boolean alternate = true;  // First from right will be doubled
        
        for (int i = partialNumber.length() - 1; i >= 0; i--) {
            int digit = partialNumber.charAt(i) - '0';
            
            if (alternate) {
                digit *= 2;
                if (digit > 9) {
                    digit -= 9;
                }
            }
            
            sum += digit;
            alternate = !alternate;
        }
        
        return (10 - (sum % 10)) % 10;
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/Luhn.java`](../../src/main/java/com/thealgorithms/others/Luhn.java)

---

## 🌍 Real-World Applications

### 1. Credit Cards
**Use Case:** Visa, MasterCard, Amex validation

### 2. IMEI Numbers
**Use Case:** Mobile device identification

### 3. Canadian SIN
**Use Case:** Social Insurance Numbers

### 4. ISIN
**Use Case:** International Securities Identification

### Card Number Prefixes

| Card Type | Prefix | Length |
|-----------|--------|--------|
| Visa | 4 | 16 |
| MasterCard | 51-55 | 16 |
| Amex | 34, 37 | 15 |
| Discover | 6011 | 16 |

---

## 🚨 Error Detection Capabilities

| Error Type | Detected? |
|------------|-----------|
| Single digit error | ✅ Yes (100%) |
| Adjacent transposition | ✅ Yes (most) |
| Twin errors (90↔09) | ❌ No |
| Random multiple errors | Partial |

**Detection Rate:** ~98% of random errors

---

## ⚖️ Luhn vs Other Checksums

| Algorithm | Error Detection | Complexity | Use |
|-----------|-----------------|------------|-----|
| Luhn | Good | Simple | Finance |
| Verhoeff | Better | More complex | Higher security |
| Damm | Better | More complex | General purpose |
| ISBN-13 | Good | Simple | Books |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Non-digit characters | Invalid input | Strip non-digits |
| Leading zeros | May be dropped | Use string type |
| Direction confusion | Wrong result | Always right-to-left |
| Not cryptographic | Can forge valid numbers | Use for validation only |

---

## 📖 References

1. **"Computer Methods of Check Digit Verification"** - IBM (1954)
2. **US Patent 2,950,048** - Hans Peter Luhn
3. **ISO/IEC 7812** - Card numbering standard

---

## 🔗 Related Algorithms

- [Verhoeff Algorithm](./verhoeff.md)
- [Damm Algorithm](./damm.md)
- [CRC](./error-detection/crc.md)

---

*Last updated: December 30, 2025*
