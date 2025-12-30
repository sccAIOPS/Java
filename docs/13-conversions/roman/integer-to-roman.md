# Integer to Roman Conversion

> **Category:** Conversions  
> **Subcategory:** Roman Numerals  
> **Implementation:** [`IntegerToRoman.java`](../../src/main/java/com/thealgorithms/conversions/IntegerToRoman.java)

---

## 📚 Overview

Integer to Roman conversion transforms decimal numbers (1-3999) into Roman numeral strings. The algorithm uses a greedy approach, repeatedly selecting the largest Roman value that fits into the remaining number.

**Key Characteristics:**
- Greedy algorithm approach
- Valid range: 1 to 3999
- Uses both additive and subtractive notation

---

## 🔢 Mathematical Foundation

### Roman Numeral Values (Including Subtractive)

| Value | Symbol | Value | Symbol |
|-------|--------|-------|--------|
| 1000 | M | 4 | IV |
| 900 | CM | 5 | V |
| 500 | D | 9 | IX |
| 400 | CD | 10 | X |
| 100 | C | 40 | XL |
| 90 | XC | 50 | L |
| 50 | L | 90 | XC |

### Greedy Approach

Process values from largest to smallest:
1. Find largest value ≤ remaining number
2. Append corresponding symbol
3. Subtract value from number
4. Repeat until number = 0

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(1) - bounded input range |
| Space | O(1) |

Note: Since max input is 3999, output is at most 15 characters (MMMDCCCLXXXVIII = 3888).

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM IntegerToRoman(num)
─────────────────────────────────────────────────────
    INPUT:  num - integer 1-3999
    OUTPUT: Roman numeral string
─────────────────────────────────────────────────────

    values  ← [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
    symbols ← ["M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"]
    
    result ← ""
    
    FOR i ← 0 TO length(values) - 1 DO
        WHILE num >= values[i] DO
            result ← result + symbols[i]
            num ← num - values[i]
        END WHILE
    END FOR
    
    RETURN result
```

### Step-by-Step Walkthrough

**Convert 1994 to Roman:**

| Remaining | Value | Symbol | Result |
|-----------|-------|--------|--------|
| 1994 | 1000 | M | M |
| 994 | 900 | CM | MCM |
| 94 | 90 | XC | MCMXC |
| 4 | 4 | IV | **MCMXCIV** |

---

## 💻 Implementation Notes

### Java Implementation

```java
public class IntegerToRoman {
    
    private static final int[] VALUES = {
        1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1
    };
    
    private static final String[] SYMBOLS = {
        "M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"
    };
    
    public static String intToRoman(int num) {
        if (num < 1 || num > 3999) {
            throw new IllegalArgumentException(
                "Number must be between 1 and 3999");
        }
        
        StringBuilder result = new StringBuilder();
        
        for (int i = 0; i < VALUES.length; i++) {
            while (num >= VALUES[i]) {
                result.append(SYMBOLS[i]);
                num -= VALUES[i];
            }
        }
        
        return result.toString();
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/conversions/IntegerToRoman.java`](../../src/main/java/com/thealgorithms/conversions/IntegerToRoman.java)

---

## 🌍 Real-World Applications

### 1. Document Formatting
**Use Case:** Generating outline numbers (Chapter I, II, III)

### 2. Copyright Years
**Use Case:** Movie/book copyright notices

### 3. Clock Displays
**Use Case:** Traditional clock face generation

---

## ⚖️ Alternative Implementations

| Approach | Description | Pros | Cons |
|----------|-------------|------|------|
| Greedy (Above) | Subtract largest value | Simple | Requires sorted arrays |
| Lookup Table | Pre-computed thousands, hundreds, etc. | Fast | More memory |
| Recursive | Divide and conquer | Elegant | Stack overhead |

### Lookup Table Approach

```java
private static final String[] THOUSANDS = {"", "M", "MM", "MMM"};
private static final String[] HUNDREDS = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
private static final String[] TENS = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
private static final String[] ONES = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};

public static String intToRomanLookup(int num) {
    return THOUSANDS[num / 1000] +
           HUNDREDS[(num % 1000) / 100] +
           TENS[(num % 100) / 10] +
           ONES[num % 10];
}
```

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Out of range | num > 3999 | Validate input |
| Zero | No Roman zero | Handle as error |
| Negative numbers | Not supported | Validate input |

---

## 📖 References

1. **LeetCode Problem 12** - Integer to Roman
2. **Roman Numeral Standards** - ISO/IEC

---

## 🔗 Related Algorithms

- [Roman to Integer](./roman-to-integer.md)
- [Number to Words](../text/number-to-words.md)

---

*Last updated: December 30, 2025*
