# Roman to Integer Conversion

> **Category:** Conversions  
> **Subcategory:** Roman Numerals  
> **Implementation:** [`RomanToInteger.java`](../../src/main/java/com/thealgorithms/conversions/RomanToInteger.java)

---

## 📚 Overview

Roman to Integer conversion transforms Roman numeral strings into their decimal equivalents. Roman numerals use seven symbols (I, V, X, L, C, D, M) with specific rules for combining them, including subtractive notation for certain combinations.

**Key Characteristics:**
- Seven basic symbols with fixed values
- Subtractive notation for 4, 9, 40, 90, 400, 900
- Left-to-right processing with lookahead

---

## 🔢 Mathematical Foundation

### Roman Numeral Values

| Symbol | Value |
|--------|-------|
| I | 1 |
| V | 5 |
| X | 10 |
| L | 50 |
| C | 100 |
| D | 500 |
| M | 1000 |

### Subtractive Notation

| Notation | Value | Meaning |
|----------|-------|---------|
| IV | 4 | 5 - 1 |
| IX | 9 | 10 - 1 |
| XL | 40 | 50 - 10 |
| XC | 90 | 100 - 10 |
| CD | 400 | 500 - 100 |
| CM | 900 | 1000 - 100 |

### Rule

If a smaller value precedes a larger value, subtract it; otherwise, add it.

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n) where n = string length |
| Space | O(1) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM RomanToInteger(s)
─────────────────────────────────────────────────────
    INPUT:  s - Roman numeral string
    OUTPUT: integer value
─────────────────────────────────────────────────────

    values ← {'I':1, 'V':5, 'X':10, 'L':50, 'C':100, 'D':500, 'M':1000}
    result ← 0
    
    FOR i ← 0 TO length(s) - 1 DO
        current ← values[s[i]]
        next ← values[s[i+1]] if i+1 < length(s) else 0
        
        IF current < next THEN
            result ← result - current    // Subtractive case
        ELSE
            result ← result + current    // Additive case
        END IF
    END FOR
    
    RETURN result
```

### Step-by-Step Walkthrough

**Convert "MCMXCIV" (1994):**

| Position | Char | Value | Next | Action | Total |
|----------|------|-------|------|--------|-------|
| 0 | M | 1000 | C(100) | Add | 1000 |
| 1 | C | 100 | M(1000) | Subtract | 900 |
| 2 | M | 1000 | X(10) | Add | 1900 |
| 3 | X | 10 | C(100) | Subtract | 1890 |
| 4 | C | 100 | I(1) | Add | 1990 |
| 5 | I | 1 | V(5) | Subtract | 1989 |
| 6 | V | 5 | - | Add | **1994** |

---

## 💻 Implementation Notes

### Java Implementation

```java
public static int romanToInteger(String s) {
    Map<Character, Integer> values = Map.of(
        'I', 1, 'V', 5, 'X', 10, 'L', 50,
        'C', 100, 'D', 500, 'M', 1000
    );
    
    int result = 0;
    for (int i = 0; i < s.length(); i++) {
        int current = values.get(s.charAt(i));
        int next = (i + 1 < s.length()) ? 
            values.get(s.charAt(i + 1)) : 0;
        
        if (current < next) {
            result -= current;
        } else {
            result += current;
        }
    }
    return result;
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/conversions/RomanToInteger.java`](../../src/main/java/com/thealgorithms/conversions/RomanToInteger.java)

---

## 🌍 Real-World Applications

### 1. Document Processing
**Use Case:** Converting chapter numbers in legal/academic documents

### 2. Clock Faces
**Use Case:** Reading time from Roman numeral clocks

### 3. Media Titles
**Use Case:** Movie sequels (Rocky IV, Star Wars Episode IX)

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Invalid input | "IIII" instead of "IV" | Validate or assume valid |
| Case sensitivity | 'i' vs 'I' | Normalize to uppercase |
| Empty string | No input | Return 0 or handle error |

---

## 📖 References

1. **LeetCode Problem 13** - Roman to Integer
2. **Roman Numeral History** - Ancient Mathematics

---

## 🔗 Related Algorithms

- [Integer to Roman](./integer-to-roman.md)
- [Number to Words](../text/number-to-words.md)

---

*Last updated: December 30, 2025*
