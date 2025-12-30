# Edit Distance (Levenshtein Distance)

> **Category:** Dynamic Programming  
> **Subcategory:** String Algorithms  
> **Implementation:** [EditDistance.java](../../src/main/java/com/thealgorithms/dynamicprogramming/EditDistance.java)

---

## 📚 Overview

**Edit Distance** (also known as **Levenshtein Distance**) measures the minimum number of single-character operations required to transform one string into another. The allowed operations are insertion, deletion, and substitution. It is fundamental to spell checking, DNA analysis, and natural language processing.

---

## 🔢 Mathematical Foundation

### Definition

Given two strings X and Y, the edit distance $d(X, Y)$ is the minimum number of operations to transform X into Y, where operations are:
- **Insert** a character
- **Delete** a character  
- **Replace** a character with another

### Key Properties

- **Metric Properties:**
  - $d(X, Y) \geq 0$ (non-negativity)
  - $d(X, Y) = 0 \iff X = Y$ (identity)
  - $d(X, Y) = d(Y, X)$ (symmetry)
  - $d(X, Z) \leq d(X, Y) + d(Y, Z)$ (triangle inequality)

- **Bounds:** $|len(X) - len(Y)| \leq d(X,Y) \leq \max(len(X), len(Y))$

### Recurrence Relation

Let $D[i][j]$ = edit distance between $X[1..i]$ and $Y[1..j]$:

$$
D[i][j] = \begin{cases}
j & \text{if } i = 0 \\
i & \text{if } j = 0 \\
D[i-1][j-1] & \text{if } x_i = y_j \\
1 + \min(D[i-1][j], D[i][j-1], D[i-1][j-1]) & \text{otherwise}
\end{cases}
$$

---

## 📊 Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| **Brute Force (Recursion)** | $O(3^{m+n})$ | $O(m+n)$ |
| **Memoization** | $O(m \cdot n)$ | $O(m \cdot n)$ |
| **Tabulation** | $O(m \cdot n)$ | $O(m \cdot n)$ |
| **Space Optimized** | $O(m \cdot n)$ | $O(\min(m, n))$ |

### Detailed Analysis

The DP approach ensures each subproblem $(i, j)$ is solved exactly once:
- Total subproblems: $(m+1) \times (n+1)$
- Work per subproblem: $O(1)$
- Space can be reduced to O(min(m,n)) using two rows

---

## 🔄 Algorithm (Pseudocode)

### Tabulation Approach
```
ALGORITHM EditDistance(X, Y)
    INPUT: Strings X[1..m] and Y[1..n]
    OUTPUT: Minimum edit distance
    
    1. m ← length(X), n ← length(Y)
    2. D[0..m][0..n] ← 0
    3. FOR i ← 0 TO m DO D[i][0] ← i    // deletions
    4. FOR j ← 0 TO n DO D[0][j] ← j    // insertions
    5. FOR i ← 1 TO m DO
    6.     FOR j ← 1 TO n DO
    7.         IF X[i] = Y[j] THEN
    8.             D[i][j] ← D[i-1][j-1]
    9.         ELSE
    10.            insert ← D[i][j-1] + 1
    11.            delete ← D[i-1][j] + 1
    12.            replace ← D[i-1][j-1] + 1
    13.            D[i][j] ← min(insert, delete, replace)
    14.        END IF
    15.    END FOR
    16. END FOR
    17. RETURN D[m][n]
```

### Step-by-Step Walkthrough

**Example:** X = "kitten", Y = "sitting"

```
      ""   s   i   t   t   i   n   g
""     0   1   2   3   4   5   6   7
k      1   1   2   3   4   5   6   7
i      2   2   1   2   3   4   5   6
t      3   3   2   1   2   3   4   5
t      4   4   3   2   1   2   3   4
e      5   5   4   3   2   2   3   4
n      6   6   5   4   3   3   2   3

Edit Distance = 3
```

**Operations to transform "kitten" → "sitting":**
1. **Substitute** k → s: "sitten"
2. **Substitute** e → i: "sittin"  
3. **Insert** g: "sitting"

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Two Implementations:** 
  - `minDistance()` - Iterative tabulation
  - `editDistance()` - Recursive with memoization
- **Base Cases:** Empty string transformations handled via initialization
- **In-place Updates:** Characters compared directly

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/dynamicprogramming/EditDistance.java`

```java
public static int minDistance(String word1, String word2) {
    int len1 = word1.length();
    int len2 = word2.length();
    int[][] dp = new int[len1 + 1][len2 + 1];
    
    // Base cases
    for (int i = 0; i <= len1; i++) dp[i][0] = i;
    for (int j = 0; j <= len2; j++) dp[0][j] = j;
    
    // Fill DP table
    for (int i = 0; i < len1; i++) {
        for (int j = 0; j < len2; j++) {
            if (word1.charAt(i) == word2.charAt(j)) {
                dp[i + 1][j + 1] = dp[i][j];
            } else {
                int replace = dp[i][j] + 1;
                int insert = dp[i][j + 1] + 1;
                int delete = dp[i + 1][j] + 1;
                dp[i + 1][j + 1] = Math.min(replace, Math.min(insert, delete));
            }
        }
    }
    return dp[len1][len2];
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Spell Checking & Autocorrect
**Use Case:** Suggesting corrections for misspelled words  
**Example:** Microsoft Word, Google Search suggestions use edit distance for "Did you mean..."

### 2. DNA Sequence Alignment
**Use Case:** Comparing genetic sequences for similarity  
**Example:** NCBI BLAST uses edit distance variants for sequence alignment

### 3. Plagiarism Detection
**Use Case:** Measuring similarity between documents  
**Example:** Academic tools compare submissions using edit distance metrics

### 4. Fuzzy String Matching
**Use Case:** Database search with typo tolerance  
**Example:** Elasticsearch's fuzzy query uses Levenshtein distance

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **Google Search** | Query autocorrection and suggestions |
| **Apple iOS** | Keyboard autocorrect and suggestions |
| **Elasticsearch** | Fuzzy search queries |
| **Git** | Diff algorithms for merge suggestions |

---

## ⚖️ Comparison with Related Algorithms

| Metric | Operations | Use Case |
|--------|------------|----------|
| **Levenshtein** | Insert, Delete, Replace | General spell check |
| **Damerau-Levenshtein** | + Transposition | Typo detection |
| **Hamming** | Replace only (same length) | Error detection in codes |
| **Jaro-Winkler** | Weighted similarity | Record linkage |
| **LCS-based** | Insert, Delete only | Diff utilities |

### Relationship with LCS

$$
EditDistance(X, Y) = m + n - 2 \times LCS(X, Y)
$$

When only insertions and deletions are allowed (no substitutions).

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Empty String Handling:** Distance from empty to string S is len(S)
   - Solution: Initialize base cases correctly

2. **Case Sensitivity:** "Hello" vs "hello" may need normalization
   - Solution: Convert to same case before comparison

3. **Unicode Characters:** Multi-byte characters need proper handling
   - Solution: Use `String.codePointAt()` for Unicode safety

4. **Large Strings:** Memory issues with very long strings
   - Solution: Use space-optimized O(min(m,n)) version

### Edge Cases to Handle

- [x] Empty strings → distance = length of other string
- [x] Identical strings → distance = 0
- [x] One character difference → distance = 1
- [x] Completely different strings → distance = max length

---

## 📖 References

1. Levenshtein, V.I. "Binary codes capable of correcting deletions, insertions, and reversals" (1966)
2. Wagner, R.A., Fischer, M.J. "The String-to-String Correction Problem" (1974)
3. [Wikipedia: Edit Distance](https://en.wikipedia.org/wiki/Edit_distance)

---

## 🔗 Related Algorithms

- [Longest Common Subsequence](./longest-common-subsequence.md) - Related string metric
- [Damerau-Levenshtein Distance](./damerau-levenshtein-distance.md) - With transpositions
- [Needleman-Wunsch](./needleman-wunsch.md) - Bioinformatics alignment
- [Hamming Distance](../bitmanipulation/hamming-distance.md) - Same-length strings
