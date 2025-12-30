# Verhoeff Algorithm

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Validation  
> **Implementation:** [`Verhoeff.java`](../../src/main/java/com/thealgorithms/others/Verhoeff.java)

---

## 📚 Overview

The Verhoeff algorithm is a checksum formula for error detection developed by Dutch mathematician Jacobus Verhoeff in 1969. Unlike the Luhn algorithm, it can detect all single digit errors and all transposition errors involving two adjacent digits.

**Key Characteristics:**
- Detects all single-digit errors
- Detects all adjacent transposition errors
- Based on dihedral group D₅
- More complex than Luhn but more reliable

---

## 🔢 Mathematical Foundation

### Dihedral Group D₅

The algorithm uses the symmetry group of a regular pentagon (10 elements).

### Tables Required

**Multiplication Table (d):**
| × | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|---|---|---|---|
| 0 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| 1 | 1 | 2 | 3 | 4 | 0 | 6 | 7 | 8 | 9 | 5 |
| 2 | 2 | 3 | 4 | 0 | 1 | 7 | 8 | 9 | 5 | 6 |
| ... | ... |

**Permutation Table (p):**
| Position | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|----------|---|---|---|---|---|---|---|---|---|---|
| 0 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| 1 | 1 | 5 | 7 | 6 | 2 | 8 | 3 | 0 | 9 | 4 |
| ... | ... |

**Inverse Table (inv):**
| x | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|---|---|---|---|
| inv(x) | 0 | 4 | 3 | 2 | 1 | 5 | 6 | 7 | 8 | 9 |

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
ALGORITHM VerhoeffValidate(number)
─────────────────────────────────────────────────────
    INPUT:  number - string of digits
    OUTPUT: TRUE if valid, FALSE otherwise
─────────────────────────────────────────────────────

    c ← 0
    
    FOR i ← length(number) - 1 DOWNTO 0 DO
        position ← length(number) - 1 - i
        digit ← number[i] - '0'
        
        // Apply permutation based on position
        p_value ← p[position MOD 8][digit]
        
        // Apply multiplication
        c ← d[c][p_value]
    END FOR
    
    RETURN c = 0
```

### Check Digit Generation
```
ALGORITHM VerhoeffCheckDigit(partialNumber)
─────────────────────────────────────────────────────
    c ← 0
    
    FOR i ← length(partialNumber) - 1 DOWNTO 0 DO
        position ← length(partialNumber) - i
        digit ← partialNumber[i] - '0'
        
        p_value ← p[position MOD 8][digit]
        c ← d[c][p_value]
    END FOR
    
    RETURN inv[c]
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class Verhoeff {
    // Multiplication table
    private static final int[][] d = {
        {0, 1, 2, 3, 4, 5, 6, 7, 8, 9},
        {1, 2, 3, 4, 0, 6, 7, 8, 9, 5},
        {2, 3, 4, 0, 1, 7, 8, 9, 5, 6},
        {3, 4, 0, 1, 2, 8, 9, 5, 6, 7},
        {4, 0, 1, 2, 3, 9, 5, 6, 7, 8},
        {5, 9, 8, 7, 6, 0, 4, 3, 2, 1},
        {6, 5, 9, 8, 7, 1, 0, 4, 3, 2},
        {7, 6, 5, 9, 8, 2, 1, 0, 4, 3},
        {8, 7, 6, 5, 9, 3, 2, 1, 0, 4},
        {9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
    };
    
    // Permutation table
    private static final int[][] p = {
        {0, 1, 2, 3, 4, 5, 6, 7, 8, 9},
        {1, 5, 7, 6, 2, 8, 3, 0, 9, 4},
        {5, 8, 0, 3, 7, 9, 6, 1, 4, 2},
        {8, 9, 1, 6, 0, 4, 3, 5, 2, 7},
        {9, 4, 5, 3, 1, 2, 6, 8, 7, 0},
        {4, 2, 8, 6, 5, 7, 3, 9, 0, 1},
        {2, 7, 9, 3, 8, 0, 6, 4, 1, 5},
        {7, 0, 4, 6, 9, 1, 3, 2, 5, 8}
    };
    
    // Inverse table
    private static final int[] inv = {0, 4, 3, 2, 1, 5, 6, 7, 8, 9};
    
    public static boolean validate(String number) {
        int c = 0;
        int len = number.length();
        
        for (int i = 0; i < len; i++) {
            int digit = number.charAt(len - 1 - i) - '0';
            c = d[c][p[i % 8][digit]];
        }
        
        return c == 0;
    }
    
    public static int generateCheckDigit(String partialNumber) {
        int c = 0;
        int len = partialNumber.length();
        
        for (int i = 0; i < len; i++) {
            int digit = partialNumber.charAt(len - 1 - i) - '0';
            c = d[c][p[(i + 1) % 8][digit]];
        }
        
        return inv[c];
    }
    
    public static String addCheckDigit(String partialNumber) {
        return partialNumber + generateCheckDigit(partialNumber);
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/Verhoeff.java`](../../src/main/java/com/thealgorithms/others/Verhoeff.java)

---

## 🌍 Real-World Applications

### 1. Indian Aadhaar Numbers
**Use Case:** 12-digit unique identification

### 2. ISIL Identifiers
**Use Case:** International Standard Identifier for Libraries

### 3. Medical Record Numbers
**Use Case:** Hospital identification systems

---

## 🚨 Error Detection Capabilities

| Error Type | Detected? |
|------------|-----------|
| Single digit substitution | ✅ 100% |
| Adjacent transposition | ✅ 100% |
| Twin errors (00↔99, etc.) | ✅ Yes |
| Jump transposition | ✅ Most |
| Phonetic errors (0↔0) | ✅ Yes |

---

## ⚖️ Verhoeff vs Luhn

| Feature | Verhoeff | Luhn |
|---------|----------|------|
| Single digit errors | 100% | 100% |
| Adjacent transposition | 100% | ~98% |
| Twin errors | 100% | No |
| Complexity | Higher | Lower |
| Speed | Slightly slower | Faster |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Table errors | Wrong results | Use verified tables |
| Position calculation | Off-by-one | Test with known values |
| MOD 8 for permutation | Cycle errors | Check array bounds |
| Not cryptographic | Forgeable | Use for validation only |

---

## 📖 References

1. **"Error Detecting Decimal Codes"** - Verhoeff (1969)
2. **"The Mathematics of the Verhoeff Check Digit"** - Schulz
3. **Aadhaar Technical Specifications** - UIDAI

---

## 🔗 Related Algorithms

- [Luhn Algorithm](./luhn.md)
- [Damm Algorithm](./damm.md)
- [ISBN Check Digits](./isbn-checksum.md)

---

*Last updated: December 30, 2025*
