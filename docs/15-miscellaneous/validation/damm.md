# Damm Algorithm

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Validation  
> **Implementation:** [`Damm.java`](../../src/main/java/com/thealgorithms/others/Damm.java)

---

## 📚 Overview

The Damm algorithm is a check digit algorithm that detects all single-digit errors and all adjacent transposition errors. Developed by H. Michael Damm in 2004, it uses a quasigroup operation table to calculate a check digit.

**Key Characteristics:**
- Detects all single digit errors
- Detects all adjacent transpositions
- Simple implementation
- Based on weak totally anti-symmetric quasigroup

---

## 🔢 Mathematical Foundation

### Quasigroup

A quasigroup (Q, ∗) is a set Q with a binary operation ∗ such that for each a and b in Q, there exist unique elements x and y in Q such that:
- a ∗ x = b
- y ∗ a = b

### Totally Anti-Symmetric

A quasigroup is totally anti-symmetric if:
- a ∗ b = b ∗ a implies a = b
- a ∗ b ∗ a = b implies a = b

### Operation Table

The Damm algorithm uses a 10×10 operation table:

|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|---|---|---|---|
| 0 | 0 | 3 | 1 | 7 | 5 | 9 | 8 | 6 | 4 | 2 |
| 1 | 7 | 0 | 9 | 2 | 1 | 5 | 4 | 8 | 6 | 3 |
| 2 | 4 | 2 | 0 | 6 | 8 | 7 | 1 | 3 | 5 | 9 |
| 3 | 1 | 7 | 5 | 0 | 9 | 8 | 3 | 4 | 2 | 6 |
| 4 | 6 | 1 | 2 | 3 | 0 | 4 | 5 | 9 | 7 | 8 |
| 5 | 3 | 6 | 7 | 4 | 2 | 0 | 9 | 5 | 8 | 1 |
| 6 | 5 | 8 | 6 | 9 | 7 | 2 | 0 | 1 | 3 | 4 |
| 7 | 8 | 9 | 4 | 5 | 3 | 6 | 2 | 0 | 1 | 7 |
| 8 | 9 | 4 | 3 | 8 | 6 | 1 | 7 | 2 | 0 | 5 |
| 9 | 2 | 5 | 8 | 1 | 4 | 3 | 6 | 7 | 9 | 0 |

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
ALGORITHM DammValidate(number)
─────────────────────────────────────────────────────
    INPUT:  number - string of digits
    OUTPUT: TRUE if valid, FALSE otherwise
─────────────────────────────────────────────────────

    interim ← 0
    
    FOR each digit d IN number DO
        interim ← table[interim][d]
    END FOR
    
    RETURN interim = 0
```

### Check Digit Generation
```
ALGORITHM DammCheckDigit(partialNumber)
─────────────────────────────────────────────────────
    interim ← 0
    
    FOR each digit d IN partialNumber DO
        interim ← table[interim][d]
    END FOR
    
    RETURN interim  // The check digit
```

### Step-by-Step Example

**Number:** 572

```
Start: interim = 0

Digit 5: table[0][5] = 9 → interim = 9
Digit 7: table[9][7] = 7 → interim = 7
Digit 2: table[7][2] = 4 → interim = 4

Check digit = 4
Complete number: 5724
```

**Verification of 5724:**
```
interim = 0
table[0][5] = 9
table[9][7] = 7
table[7][2] = 4
table[4][4] = 0 ✓ (Valid!)
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class Damm {
    private static final int[][] TABLE = {
        {0, 3, 1, 7, 5, 9, 8, 6, 4, 2},
        {7, 0, 9, 2, 1, 5, 4, 8, 6, 3},
        {4, 2, 0, 6, 8, 7, 1, 3, 5, 9},
        {1, 7, 5, 0, 9, 8, 3, 4, 2, 6},
        {6, 1, 2, 3, 0, 4, 5, 9, 7, 8},
        {3, 6, 7, 4, 2, 0, 9, 5, 8, 1},
        {5, 8, 6, 9, 7, 2, 0, 1, 3, 4},
        {8, 9, 4, 5, 3, 6, 2, 0, 1, 7},
        {9, 4, 3, 8, 6, 1, 7, 2, 0, 5},
        {2, 5, 8, 1, 4, 3, 6, 7, 9, 0}
    };
    
    public static boolean validate(String number) {
        int interim = 0;
        
        for (char c : number.toCharArray()) {
            int digit = c - '0';
            interim = TABLE[interim][digit];
        }
        
        return interim == 0;
    }
    
    public static int generateCheckDigit(String partialNumber) {
        int interim = 0;
        
        for (char c : partialNumber.toCharArray()) {
            int digit = c - '0';
            interim = TABLE[interim][digit];
        }
        
        return interim;
    }
    
    public static String addCheckDigit(String partialNumber) {
        return partialNumber + generateCheckDigit(partialNumber);
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/Damm.java`](../../src/main/java/com/thealgorithms/others/Damm.java)

---

## 🌍 Real-World Applications

### 1. Bank Account Numbers
**Use Case:** Account number validation

### 2. Product Codes
**Use Case:** Serial numbers, part numbers

### 3. Medical Records
**Use Case:** Patient identification numbers

### 4. Government IDs
**Use Case:** Identification card numbers

---

## 🚨 Error Detection Capabilities

| Error Type | Detected? |
|------------|-----------|
| Single digit error | ✅ 100% |
| Adjacent transposition | ✅ 100% |
| Twin errors | ✅ Yes |
| Phonetic errors | ✅ Yes |
| Jump transposition | Partial |

---

## ⚖️ Damm vs Other Checksum Algorithms

| Feature | Damm | Luhn | Verhoeff |
|---------|------|------|----------|
| Single digit | 100% | 100% | 100% |
| Transposition | 100% | ~98% | 100% |
| Twin errors | Yes | No | Yes |
| Tables required | 1 | 0 | 3 |
| Implementation | Simple | Simplest | Complex |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Table errors | Wrong results | Use verified table |
| Non-digits | Crashes | Validate input |
| Empty string | Edge case | Return 0 or handle |
| Not cryptographic | Forgeable | Use for validation only |

---

## 📖 References

1. **"Totally Anti-Symmetric Quasigroups"** - Damm (2004)
2. **"Error Detection Methods"** - ACM Computing Surveys
3. **ISO/IEC 7064** - Check character systems

---

## 🔗 Related Algorithms

- [Luhn Algorithm](./luhn.md)
- [Verhoeff Algorithm](./verhoeff.md)
- [CRC](./error-detection/crc.md)

---

*Last updated: December 30, 2025*
