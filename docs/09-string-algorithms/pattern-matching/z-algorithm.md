# Z-Algorithm

> **Category:** String Algorithms  
> **Subcategory:** Pattern Matching  
> **Implementation:** [`ZAlgorithm.java`](../../../src/main/java/com/thealgorithms/strings/ZAlgorithm.java)

---

## 📚 Overview

The Z-Algorithm is a linear time string matching algorithm that constructs a Z-array for a given string. The Z-array stores the length of the longest substring starting from each position that matches a prefix of the string. This array enables efficient pattern matching by concatenating the pattern and text.

**Key Characteristics:**
- Linear time complexity O(n + m)
- Constructs Z-array in single pass
- Uses concept of Z-boxes for optimization
- Simpler implementation than KMP with same complexity

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** For a string $S[0..n-1]$, the Z-array $Z[0..n-1]$ is defined such that $Z[i]$ is the length of the longest substring starting at $S[i]$ that matches a prefix of $S$.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Z-value | Longest prefix match at position i | $Z[i] = \max\{k : S[0..k-1] = S[i..i+k-1]\}$ |
| Z-box | Interval [L, R] of rightmost matched segment | $R = \max\{i + Z[i] - 1\}$ |
| Pattern Matching | Pattern found when Z[i] = pattern length | $Z[m+1+i] = m \Rightarrow$ match at $i$ |

### Mathematical Formulation

For string $S$ of length $n$:

$$
Z[i] = \max\{k \geq 0 : S[0..k-1] = S[i..i+k-1]\}
$$

**Z-box boundaries:**

$$
L = \arg\max_{j \leq i}\{j + Z[j]\}, \quad R = L + Z[L] - 1
$$

**Pattern Matching using Z-array:**

Given pattern $P$ of length $m$ and text $T$ of length $n$:
- Construct string $S = P \$ T$ ($ is a separator not in P or T)
- Compute Z-array for $S$
- Pattern found at position $i$ in $T$ if $Z[m+1+i] = m$

### Recurrence Relation

For position $i$ with Z-box $[L, R]$:

$$
Z[i] = \begin{cases}
0 & \text{if } i > R \\
\min(Z[i-L], R-i+1) & \text{if } i \leq R \text{ (initial estimate)}
\end{cases}
$$

Then extend by character comparison while $S[Z[i]] = S[i + Z[i]]$.

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **Best** | $O(n)$ | All characters same or very different |
| **Average** | $O(n)$ | Typical strings |
| **Worst** | $O(n)$ | Always linear due to Z-box optimization |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary Space** | $O(n)$ | Z-array storage |
| **Total Space** | $O(n + m)$ | For pattern matching (combined string) |

### Additional Properties

| Property | Value |
|----------|-------|
| **In-place** | No (requires Z-array) |
| **Online** | No (needs full string) |
| **Deterministic** | Yes |

### Detailed Analysis

The algorithm maintains Z-box [L, R] representing the rightmost matched interval:
- Each position is visited at most twice
- R only increases, never decreases
- Total comparisons bounded by 2n

```
Time = O(n) for Z-array construction
Pattern matching = O(n + m) for combined string
```

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Z-Function(S)
─────────────────────────────────────────────────────
    INPUT:  String S[0..n-1]
    OUTPUT: Z-array Z[0..n-1]
─────────────────────────────────────────────────────

    1. n ← length(S)
    2. Z ← array of size n, initialized to 0
    3. L ← 0, R ← 0              // Z-box boundaries
    
    4. FOR i ← 1 TO n-1 DO
    5.     IF i ≤ R THEN
    6.         Z[i] ← min(R - i + 1, Z[i - L])
    7.     END IF
    8.     
    9.     // Extend Z[i] by comparing characters
    10.    WHILE i + Z[i] < n AND S[Z[i]] = S[i + Z[i]] DO
    11.        Z[i] ← Z[i] + 1
    12.    END WHILE
    13.    
    14.    // Update Z-box if needed
    15.    IF i + Z[i] - 1 > R THEN
    16.        L ← i
    17.        R ← i + Z[i] - 1
    18.    END IF
    19. END FOR
    20. RETURN Z


ALGORITHM Z-Search(text, pattern)
─────────────────────────────────────────────────────
    INPUT:  Text T, Pattern P
    OUTPUT: First occurrence position or -1
─────────────────────────────────────────────────────

    1. combined ← P + "$" + T
    2. Z ← Z-Function(combined)
    3. m ← length(P)
    
    4. FOR i ← 0 TO length(Z) - 1 DO
    5.     IF Z[i] = m THEN
    6.         RETURN i - m - 1    // position in T
    7.     END IF
    8. END FOR
    9. RETURN -1
```

### Step-by-Step Walkthrough

**Example Input:** String S = "aabxaab"

| i | S[i] | L | R | Z[i-L] | Initial Z[i] | After Extension | New L,R |
|---|------|---|---|--------|--------------|-----------------|---------|
| 0 | a | - | - | - | undefined | undefined | - |
| 1 | a | 0 | 0 | - | 0 | 1 | 1,1 |
| 2 | b | 1 | 1 | - | 0 | 0 | 1,1 |
| 3 | x | 1 | 1 | - | 0 | 0 | 1,1 |
| 4 | a | 1 | 1 | - | 0 | 3 | 4,6 |
| 5 | a | 4 | 6 | Z[1]=1 | 1 | 1 | 4,6 |
| 6 | b | 4 | 6 | Z[2]=0 | 0 | 0 | 4,6 |

**Result:** Z = [-, 1, 0, 0, 3, 1, 0]

**Visual Representation:**
```
String:  a  a  b  x  a  a  b
Index:   0  1  2  3  4  5  6
Z-value: -  1  0  0  3  1  0

Position 4: "aab" matches prefix "aab" (length 3)
Position 5: "a" matches prefix "a" (length 1)
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses 0-indexed arrays
- Returns first match position or -1 for no match
- Separator character '$' ensures no false matches across boundary
- Clean, utility-class design pattern

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/strings/ZAlgorithm.java`

```java
// Key code snippet - Z-function
public static int[] zFunction(String s) {
    int n = s.length();
    int[] z = new int[n];
    int l = 0, r = 0;

    for (int i = 1; i < n; i++) {
        if (i <= r) {
            z[i] = Math.min(r - i + 1, z[i - l]);
        }
        while (i + z[i] < n && s.charAt(z[i]) == s.charAt(i + z[i])) {
            z[i]++;
        }
        if (i + z[i] - 1 > r) {
            l = i;
            r = i + z[i] - 1;
        }
    }
    return z;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Text Processing Tools
**Use Case:** Find occurrences of patterns in text editors  
**Example:** grep-like utilities for pattern searching

### 2. Competitive Programming
**Use Case:** Efficient string matching in programming contests  
**Example:** ICPC and Codeforces problems involving string algorithms

### 3. Data Compression
**Use Case:** Finding repeated substrings for compression  
**Example:** LZ77-style compression algorithms

### 4. Bioinformatics
**Use Case:** DNA/RNA sequence pattern matching  
**Example:** Finding motifs in genomic sequences

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| GNU grep | Pattern searching in files |
| Bioinformatics tools | Sequence alignment preprocessing |
| Code editors | Syntax highlighting and search |
| Data compression | Repeated pattern detection |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Z-Algorithm | KMP | Rabin-Karp | Suffix Array |
|--------|-------------|-----|------------|--------------|
| Time Complexity | O(n) | O(n) | O(n) avg | O(n log n) |
| Space Complexity | O(n) | O(m) | O(1) | O(n) |
| Implementation | Simple | Moderate | Simple | Complex |
| Multiple Patterns | Moderate | Poor | Excellent | Excellent |
| Best For | Single pattern, learning | Single pattern | Multiple patterns | Many queries |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Z[0] Undefined:** First element is not computed (undefined)
2. **Separator Choice:** Must use character not in pattern or text
3. **Off-by-One:** Careful with L, R boundary updates
4. **Empty Strings:** Handle gracefully

### Edge Cases to Handle
- [ ] Empty pattern or text
- [ ] Pattern equals text
- [ ] Single character strings
- [ ] Pattern longer than text
- [ ] No matches found

---

## 📖 References

1. Gusfield, D. (1997). "Algorithms on Strings, Trees, and Sequences". Cambridge University Press.
2. [CP-Algorithms - Z-function](https://cp-algorithms.com/string/z-function.html)
3. [Wikipedia - Z Algorithm](https://en.wikipedia.org/wiki/Z_algorithm)

---

## 🔗 Related Algorithms

- [KMP Algorithm](./kmp.md) - Alternative linear time matching
- [Rabin-Karp Algorithm](./rabin-karp.md) - Hash-based matching
- [Aho-Corasick Algorithm](./aho-corasick.md) - Multiple pattern matching
- [Suffix Array](./suffix-array.md) - Advanced string structure
