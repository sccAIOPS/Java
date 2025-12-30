# Rabin-Karp Algorithm

> **Category:** String Algorithms  
> **Subcategory:** Pattern Matching  
> **Implementation:** [`RabinKarp.java`](../../../src/main/java/com/thealgorithms/strings/RabinKarp.java)

---

## 📚 Overview

The Rabin-Karp algorithm is a string searching algorithm that uses hashing to find pattern matches in text. It calculates hash values for the pattern and for each substring of the text, comparing hashes before doing character-by-character comparison only when hashes match.

**Key Characteristics:**
- Uses rolling hash for efficient hash computation
- Average time complexity O(n + m), worst case O(nm)
- Excellent for multiple pattern matching scenarios
- Hash collisions require verification through character comparison

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given text $T[0..n-1]$ and pattern $P[0..m-1]$, compute hash values $h(T[i..i+m-1])$ for all valid $i$ and compare with $h(P)$.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Hash Function | Polynomial rolling hash | $h(s) = \sum_{i=0}^{m-1} s[i] \cdot d^{m-1-i} \mod q$ |
| Rolling Hash | Efficient update | $h_{new} = d \cdot (h_{old} - s[i] \cdot d^{m-1}) + s[i+m]$ |
| Base | Alphabet size | $d = 256$ (ASCII) |

### Mathematical Formulation

The hash function for a string $s$ of length $m$ is:

$$
h(s[0..m-1]) = \left(\sum_{i=0}^{m-1} s[i] \cdot d^{m-1-i}\right) \mod q
$$

**Rolling Hash Update:**

$$
h(s[i+1..i+m]) = \left(d \cdot \left(h(s[i..i+m-1]) - s[i] \cdot d^{m-1}\right) + s[i+m]\right) \mod q
$$

**Where:**
- $d$ = size of alphabet (typically 256 for ASCII)
- $q$ = a large prime number to reduce hash collisions
- $s[i]$ = character at position $i$

### Proof of Correctness

**Theorem:** If $h(P) \neq h(T[i..i+m-1])$, then $P \neq T[i..i+m-1]$.

**Proof:** By contrapositive. If $P = T[i..i+m-1]$, then since the hash function is deterministic, $h(P) = h(T[i..i+m-1])$.

**Note:** The converse is not true due to hash collisions, hence verification is needed when hashes match.

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **Best** | $O(n + m)$ | Few or no hash collisions |
| **Average** | $O(n + m)$ | With good hash function and prime |
| **Worst** | $O(nm)$ | Many hash collisions (e.g., all same characters) |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary Space** | $O(1)$ | Only hash values stored |
| **Total Space** | $O(n + m)$ | Including input strings |

### Additional Properties

| Property | Value |
|----------|-------|
| **In-place** | Yes (minimal extra space) |
| **Online** | Yes (streaming capable) |
| **Multiple Patterns** | Excellent (same hash comparison) |

### Detailed Analysis

**Preprocessing (Pattern Hash):** $O(m)$
- Compute hash of pattern once

**Matching Phase:** $O(n - m + 1)$ hash comparisons
- Each rolling hash update: $O(1)$
- Character verification on collision: $O(m)$
- Expected collisions: $O(n/q)$ with good prime $q$

```
Expected time = O(m) + O(n) + O(m · n/q) ≈ O(n + m)
```

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Rabin-Karp(T, P, d, q)
─────────────────────────────────────────────────────
    INPUT:  Text T[0..n-1], Pattern P[0..m-1]
            Base d (alphabet size), Prime q
    OUTPUT: All positions where P occurs in T
─────────────────────────────────────────────────────

    1. n ← length(T)
    2. m ← length(P)
    3. h ← d^(m-1) mod q        // highest power for rolling
    4. p ← 0                     // pattern hash
    5. t ← 0                     // text window hash
    
    6. // Preprocessing: compute hash of pattern and first window
    7. FOR i ← 0 TO m-1 DO
    8.     p ← (d · p + P[i]) mod q
    9.     t ← (d · t + T[i]) mod q
    10. END FOR
    
    11. // Matching: slide pattern over text
    12. FOR i ← 0 TO n-m DO
    13.     IF p = t THEN
    14.         IF P[0..m-1] = T[i..i+m-1] THEN
    15.             PRINT "Pattern found at index" i
    16.         END IF
    17.     END IF
    18.     IF i < n-m THEN
    19.         // Rolling hash: remove leading, add trailing
    20.         t ← (d · (t - T[i] · h) + T[i+m]) mod q
    21.         IF t < 0 THEN
    22.             t ← t + q    // ensure positive
    23.         END IF
    24.     END IF
    25. END FOR
```

### Step-by-Step Walkthrough

**Example Input:** Text = "ABCABCD", Pattern = "ABC", d = 256, q = 101

| Step | Window | t (hash) | p (hash) | Match? | Action |
|------|--------|----------|----------|--------|--------|
| 1 | ABC | 65 | 65 | ✓ | Verify → Found at 0 |
| 2 | BCA | 42 | 65 | ✗ | Skip |
| 3 | CAB | 33 | 65 | ✗ | Skip |
| 4 | ABC | 65 | 65 | ✓ | Verify → Found at 3 |
| 5 | BCD | 43 | 65 | ✗ | Skip |

**Visual Representation:**
```
Text:    A B C A B C D
         ─────
         ABC (match at 0)
           ─────
           BCA (no match)
             ─────
             CAB (no match)
               ─────
               ABC (match at 3)
                 ─────
                 BCD (no match)
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses `ALPHABET_SIZE = 256` for ASCII characters
- Prime `q = 101` for modulo operations
- Rolling hash computed using polynomial hashing
- Verification step after hash match to handle collisions

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/strings/RabinKarp.java`

```java
// Key code snippet - Rolling Hash Update
private static void searchPat(String text, String pattern, int q) {
    int m = pattern.length();
    int n = text.length();
    int h = (int) Math.pow(ALPHABET_SIZE, m - 1) % q;
    
    // Initial hash computation
    int p = 0, t = 0;
    for (int i = 0; i < m; i++) {
        p = (ALPHABET_SIZE * p + pattern.charAt(i)) % q;
        t = (ALPHABET_SIZE * t + text.charAt(i)) % q;
    }
    
    // Rolling hash matching
    for (int i = 0; i <= n - m; i++) {
        if (p == t) {
            // Verify character by character
            // ...
        }
        if (i < n - m) {
            t = (ALPHABET_SIZE * (t - text.charAt(i) * h) + text.charAt(i + m)) % q;
            if (t < 0) t = t + q;
        }
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Plagiarism Detection
**Use Case:** Detecting copied content across multiple documents  
**Example:** Turnitin compares document fingerprints using hash-based matching

### 2. Version Control Systems
**Use Case:** Finding similarities between file versions  
**Example:** Git uses hash-based comparison for detecting file changes

### 3. Network Security
**Use Case:** Detecting malware signatures in network traffic  
**Example:** Deep packet inspection systems use Rabin fingerprinting

### 4. Bioinformatics
**Use Case:** Finding DNA sequence motifs  
**Example:** BLAST-like algorithms use hash-based sequence comparison

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Git | Content-addressable storage |
| rsync | Rolling checksum for file synchronization |
| Turnitin | Document similarity detection |
| Snort | Network intrusion detection |
| Google BigTable | Data deduplication |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Rabin-Karp | KMP | Boyer-Moore | Naive |
|--------|------------|-----|-------------|-------|
| Time Complexity | O(n+m) avg | O(n+m) | O(n/m) avg | O(nm) |
| Space Complexity | O(1) | O(m) | O(m+σ) | O(1) |
| Multiple Patterns | Excellent | Poor | Poor | Poor |
| Best For | Multiple patterns, plagiarism | Single pattern | Large alphabets | Simple cases |
| Worst Case | O(nm) | O(n+m) | O(nm) | O(nm) |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Hash Collisions:** Always verify matches character-by-character
2. **Negative Hash Values:** Handle modulo arithmetic correctly
3. **Integer Overflow:** Use appropriate data types for large values
4. **Poor Prime Choice:** Can lead to many collisions

### Edge Cases to Handle
- [ ] Empty text or pattern
- [ ] Pattern longer than text
- [ ] All characters same (worst case for collisions)
- [ ] Very long strings (overflow concerns)
- [ ] Special characters

---

## 📖 References

1. Rabin, M.O., Karp, R.M. (1987). "Efficient randomized pattern-matching algorithms". IBM Journal of Research and Development.
2. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
3. [Wikipedia - Rabin-Karp Algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm)

---

## 🔗 Related Algorithms

- [KMP Algorithm](./kmp.md) - Deterministic linear time matching
- [Z-Algorithm](./z-algorithm.md) - Linear time pattern matching
- [Aho-Corasick Algorithm](./aho-corasick.md) - Multiple pattern matching
- [Rolling Hash](../../appendix/glossary.md) - Hash technique used in Rabin-Karp
