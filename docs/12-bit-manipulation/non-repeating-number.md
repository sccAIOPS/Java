# XOR Applications: Find Non-Repeating Number

> **Category:** Bit Manipulation  
> **Subcategory:** Special Applications  
> **Implementation:** [`NonRepeatingNumberFinder.java`](../../src/main/java/com/thealgorithms/bitmanipulation/NonRepeatingNumberFinder.java)

---

## 📚 Overview

Finding the non-repeating (unique) element in an array where every other element appears exactly twice is a classic bit manipulation problem. The XOR operation's properties make this solvable in O(n) time with O(1) space—an elegant solution that demonstrates the power of bit manipulation.

**Key Characteristics:**
- Exploits XOR's self-cancellation property: a ⊕ a = 0
- Linear time complexity
- Constant space complexity
- No extra data structures needed

---

## 🔢 Mathematical Foundation

### XOR Properties

| Property | Formula | Example |
|----------|---------|---------|
| Self-inverse | a ⊕ a = 0 | 5 ⊕ 5 = 0 |
| Identity | a ⊕ 0 = a | 5 ⊕ 0 = 5 |
| Commutative | a ⊕ b = b ⊕ a | 5 ⊕ 3 = 3 ⊕ 5 |
| Associative | (a ⊕ b) ⊕ c = a ⊕ (b ⊕ c) | Reorder freely |

### Mathematical Formulation

For array $A$ with one unique element $u$ and pairs of all other elements:

$$
\bigoplus_{i=0}^{n-1} A[i] = u
$$

**Proof:** All paired elements cancel out (a ⊕ a = 0), leaving only the unique element.

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n) |
| Space | O(1) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM FindNonRepeating(array)
─────────────────────────────────────────────────────
    INPUT:  array with one unique element, others appear twice
    OUTPUT: the unique element
─────────────────────────────────────────────────────

    result ← 0
    FOR each element in array DO
        result ← result XOR element
    END FOR
    RETURN result
```

### Step-by-Step Walkthrough

**Example:** array = [4, 1, 2, 1, 2]

| Step | Element | Result (binary) | Result (decimal) |
|------|---------|-----------------|------------------|
| Init | - | 0000 | 0 |
| 1 | 4 | 0100 | 4 |
| 2 | 1 | 0101 | 5 |
| 3 | 2 | 0111 | 7 |
| 4 | 1 | 0110 | 6 |
| 5 | 2 | **0100** | **4** |

**Result:** 4 is the unique element

---

## 💻 Implementation Notes

### Java Implementation

```java
/**
 * Find the element that appears exactly once
 * All other elements appear exactly twice
 * 
 * @param nums array of integers
 * @return the unique element
 */
public static int findNonRepeating(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

### Variations

**Find Two Non-Repeating Numbers:**
```java
// When two numbers appear once, others twice
public static int[] findTwoNonRepeating(int[] nums) {
    int xor = 0;
    for (int num : nums) xor ^= num;
    
    // Find rightmost set bit (differs between the two numbers)
    int rightmostBit = xor & (-xor);
    
    int[] result = new int[2];
    for (int num : nums) {
        if ((num & rightmostBit) != 0) {
            result[0] ^= num;
        } else {
            result[1] ^= num;
        }
    }
    return result;
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/bitmanipulation/NonRepeatingNumberFinder.java`](../../src/main/java/com/thealgorithms/bitmanipulation/NonRepeatingNumberFinder.java)

---

## 🌍 Real-World Applications

### 1. Data Integrity
**Use Case:** Detecting single bit errors in data streams

### 2. Database Operations
**Use Case:** Finding missing/extra record in database synchronization

### 3. Networking
**Use Case:** Checksum calculations for error detection

### Industry Examples

| Application | Use Case |
|-------------|----------|
| RAID Systems | Parity calculation |
| Data Sync | Finding differences |
| Interview Prep | Classic coding problem |

---

## ⚖️ Comparison with Other Approaches

| Approach | Time | Space | Constraints |
|----------|------|-------|-------------|
| XOR | O(n) | O(1) | Each duplicate appears exactly twice |
| HashMap | O(n) | O(n) | Any number of duplicates |
| Sorting | O(n log n) | O(1) | Modifies array |
| Set | O(n) | O(n) | Any number of duplicates |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Note |
|---------|---------|------|
| Wrong count | If duplicates appear odd times | Only works for exactly 2 copies |
| Empty array | Undefined | Return 0 or throw |
| Negative numbers | XOR still works | Handles correctly |

---

## 📖 References

1. **LeetCode Problem 136** - Single Number
2. **"Elements of Programming Interviews"** - Bit Manipulation chapter

---

## 🔗 Related Algorithms

- [Number Appearing Odd Times](./number-odd-times.md)
- [Count Set Bits](./count-set-bits.md)
- [Hamming Distance](./hamming-distance.md)

---

*Last updated: December 30, 2025*
