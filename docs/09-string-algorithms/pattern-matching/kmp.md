# Knuth-Morris-Pratt (KMP) Algorithm

> **Category:** String Algorithms  
> **Subcategory:** Pattern Matching  
> **Implementation:** [`KMP.java`](../../../src/main/java/com/thealgorithms/strings/KMP.java)

---

## 📚 Overview

The Knuth-Morris-Pratt (KMP) algorithm is an efficient string matching algorithm that searches for occurrences of a "pattern" string within a "text" string. Unlike naive string matching, KMP avoids unnecessary character comparisons by using information from partial matches to skip portions of the text.

**Key Characteristics:**
- Linear time complexity O(n + m) where n is text length and m is pattern length
- Preprocesses the pattern to create a failure function (prefix table)
- Never backtracks in the text string during matching
- Optimal for single pattern matching scenarios

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given a text string $T[1..n]$ and a pattern string $P[1..m]$, find all positions $i$ such that $T[i..i+m-1] = P[1..m]$.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Prefix Function | Longest proper prefix that is also a suffix | $\pi[i] = \max\{k : P[1..k] = P[i-k+1..i]\}$ |
| Invariant | After comparing $P[j]$ with $T[i]$, shift pattern optimally | $j = \pi[j-1]$ on mismatch |
| No Backtracking | Text pointer never decreases | $i$ monotonically increases |

### Mathematical Formulation

The prefix function $\pi$ for pattern $P$ is defined as:

$$
\pi[i] = \max\{k : 0 \leq k < i \text{ and } P[0..k-1] = P[i-k..i-1]\}
$$

**Where:**
- $\pi[i]$ = length of longest proper prefix of $P[0..i]$ that is also a suffix
- $P[0..k-1]$ = prefix of length $k$
- $P[i-k..i-1]$ = suffix of length $k$

### Proof of Correctness

**Loop Invariant:** At the start of each iteration, $q$ equals the length of the longest prefix of $P$ that is a suffix of $T[1..i-1]$.

1. **Initialization:** Before the first iteration, $q = 0$, which correctly represents that no prefix of $P$ matches any suffix of the empty string.

2. **Maintenance:** When $T[i] = P[q]$, we extend the match. When $T[i] \neq P[q]$, we use the prefix function to find the next longest prefix that could match.

3. **Termination:** When $q = m$, we've found a complete match of $P$ in $T$.

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **Best** | $O(n)$ | Pattern not found (no matches) |
| **Average** | $O(n + m)$ | Typical text and pattern |
| **Worst** | $O(n + m)$ | Always linear due to no backtracking |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary Space** | $O(m)$ | Prefix function array |
| **Total Space** | $O(n + m)$ | Including input strings |

### Additional Properties

| Property | Value |
|----------|-------|
| **In-place** | No (requires prefix array) |
| **Online** | Yes (can process text character by character) |
| **Single/Multiple Pattern** | Single pattern |

### Detailed Analysis

**Prefix Function Computation:** $O(m)$
- Each position is visited at most twice (once for increment, once for potential fallback)

**Pattern Matching:** $O(n)$
- Text pointer $i$ increases in every iteration
- Pattern pointer $q$ may decrease but total decrements bounded by increments

```
Total operations ≤ 2n + 2m = O(n + m)
```

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM KMP-Matcher(T, P)
─────────────────────────────────────────────────────
    INPUT:  Text T[1..n], Pattern P[1..m]
    OUTPUT: All positions where P occurs in T
─────────────────────────────────────────────────────

    1. π ← COMPUTE-PREFIX-FUNCTION(P)
    2. q ← 0                           // characters matched
    3. FOR i ← 1 TO n DO
    4.     WHILE q > 0 AND P[q+1] ≠ T[i] DO
    5.         q ← π[q]                // fall back
    6.     END WHILE
    7.     IF P[q+1] = T[i] THEN
    8.         q ← q + 1               // next char matches
    9.     END IF
    10.    IF q = m THEN
    11.        PRINT "Pattern found at" i - m + 1
    12.        q ← π[q]                // look for next match
    13.    END IF
    14. END FOR

ALGORITHM COMPUTE-PREFIX-FUNCTION(P)
─────────────────────────────────────────────────────
    1. m ← length(P)
    2. π[1] ← 0
    3. k ← 0
    4. FOR q ← 2 TO m DO
    5.     WHILE k > 0 AND P[k+1] ≠ P[q] DO
    6.         k ← π[k]
    7.     END WHILE
    8.     IF P[k+1] = P[q] THEN
    9.         k ← k + 1
    10.    END IF
    11.    π[q] ← k
    12. END FOR
    13. RETURN π
```

### Step-by-Step Walkthrough

**Example Input:** Text = "AAAAABAAABA", Pattern = "AAAA"

| Step | i | Text[i] | q | π[q] | Action | Result |
|------|---|---------|---|------|--------|--------|
| 1 | 0 | A | 0→1 | - | Match | q=1 |
| 2 | 1 | A | 1→2 | - | Match | q=2 |
| 3 | 2 | A | 2→3 | - | Match | q=3 |
| 4 | 3 | A | 3→4 | - | Match, q=m | **Found at 0** |
| 5 | 4 | A | 3→4 | π[3]=2 | Match, q=m | **Found at 1** |
| 6 | 5 | B | 3→0 | - | Mismatch, fallback | q=0 |
| ... | ... | ... | ... | ... | ... | ... |

**Prefix Function for "AAAA":**
```
Index:  0  1  2  3
Char:   A  A  A  A
π:      0  1  2  3
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses 0-indexed arrays (standard Java convention)
- `computePrefixFunction()` builds the failure function
- `kmpMatcher()` performs the actual pattern matching
- Outputs starting positions of all matches

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/strings/KMP.java`

```java
// Key code snippet - Prefix Function
private static int[] computePrefixFunction(final String p) {
    final int n = p.length();
    final int[] pi = new int[n];
    pi[0] = 0;
    int q = 0;
    for (int i = 1; i < n; i++) {
        while (q > 0 && p.charAt(q) != p.charAt(i)) {
            q = pi[q - 1];
        }
        if (p.charAt(q) == p.charAt(i)) {
            q++;
        }
        pi[i] = q;
    }
    return pi;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Text Editors and IDEs
**Use Case:** Find and replace functionality  
**Example:** VS Code, IntelliJ IDEA use KMP-like algorithms for efficient text search

### 2. Intrusion Detection Systems
**Use Case:** Network packet inspection for malware signatures  
**Example:** Snort IDS uses pattern matching for detecting attack signatures

### 3. DNA Sequence Analysis
**Use Case:** Finding gene sequences within larger DNA strings  
**Example:** BLAST algorithm variants use KMP principles for sequence alignment

### 4. Plagiarism Detection
**Use Case:** Finding copied text passages in documents  
**Example:** Turnitin uses string matching algorithms for similarity detection

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Google Search | Autocomplete and spell checking |
| Microsoft Word | Find and Replace feature |
| Snort IDS | Signature-based intrusion detection |
| NCBI BLAST | Bioinformatics sequence matching |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | KMP | Rabin-Karp | Boyer-Moore | Naive |
|--------|-----|------------|-------------|-------|
| Time Complexity | O(n+m) | O(n+m) avg | O(n/m) avg | O(nm) |
| Space Complexity | O(m) | O(1) | O(m+σ) | O(1) |
| Preprocessing | O(m) | O(m) | O(m+σ) | None |
| Best For | Single pattern | Multiple patterns | Large alphabets | Simple cases |
| Worst Case | O(n+m) | O(nm) | O(nm) | O(nm) |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Off-by-One Errors:** Ensure correct indexing (0-based vs 1-based)
2. **Empty Pattern:** Should handle gracefully, typically return empty result
3. **Pattern Longer Than Text:** Algorithm should return no matches
4. **Single Character Patterns:** Works correctly but simpler approaches may suffice

### Edge Cases to Handle
- [ ] Empty text or pattern
- [ ] Pattern longer than text
- [ ] Repeated characters (e.g., "AAAA" in "AAAAAAAA")
- [ ] No matches found
- [ ] Overlapping matches

---

## 📖 References

1. Knuth, D.E., Morris, J.H., Pratt, V.R. (1977). "Fast pattern matching in strings". SIAM Journal on Computing.
2. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
3. [Wikipedia - Knuth-Morris-Pratt Algorithm](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm)

---

## 🔗 Related Algorithms

- [Rabin-Karp Algorithm](./rabin-karp.md) - Hash-based pattern matching
- [Z-Algorithm](./z-algorithm.md) - Linear time pattern matching
- [Aho-Corasick Algorithm](./aho-corasick.md) - Multiple pattern matching
- [Boyer-Moore Algorithm](./boyer-moore.md) - Right-to-left scanning
