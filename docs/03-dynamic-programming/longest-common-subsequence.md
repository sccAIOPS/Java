# Longest Common Subsequence (LCS)

> **Category:** Dynamic Programming  
> **Subcategory:** String Algorithms  
> **Implementation:** [LongestCommonSubsequence.java](../../src/main/java/com/thealgorithms/dynamicprogramming/LongestCommonSubsequence.java)

---

## 📚 Overview

The **Longest Common Subsequence (LCS)** problem finds the longest sequence that appears in both input sequences in the same relative order (not necessarily contiguous). It is fundamental to diff utilities, bioinformatics, and version control systems.

---

## 🔢 Mathematical Foundation

### Definition

Given sequences $X = \langle x_1, x_2, ..., x_m \rangle$ and $Y = \langle y_1, y_2, ..., y_n \rangle$:

A **subsequence** of X is obtained by deleting zero or more elements from X without changing the order of remaining elements.

The **LCS** is the longest sequence Z that is a subsequence of both X and Y.

### Key Properties

- **Non-unique:** Multiple LCS of same length may exist
- **Optimal Substructure:** LCS of X and Y contains LCS of prefixes
- **Symmetry:** LCS(X,Y) = LCS(Y,X)

### Recurrence Relation

Let $L[i][j]$ = length of LCS of $X[1..i]$ and $Y[1..j]$:

$$
L[i][j] = \begin{cases}
0 & \text{if } i = 0 \text{ or } j = 0 \\
L[i-1][j-1] + 1 & \text{if } x_i = y_j \\
\max(L[i-1][j], L[i][j-1]) & \text{if } x_i \neq y_j
\end{cases}
$$

---

## 📊 Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| **Brute Force** | $O(2^{m+n})$ | $O(m+n)$ |
| **Memoization** | $O(m \cdot n)$ | $O(m \cdot n)$ |
| **Tabulation** | $O(m \cdot n)$ | $O(m \cdot n)$ |
| **Space Optimized** | $O(m \cdot n)$ | $O(\min(m, n))$ |
| **Hirschberg's** | $O(m \cdot n)$ | $O(\min(m, n))$ |

### Detailed Analysis

- **Time:** O(m×n) - fill each cell once
- **Space:** Can be reduced to O(min(m,n)) using two rows
- **Reconstruction:** Additional O(m+n) to backtrack and build actual LCS

---

## 🔄 Algorithm (Pseudocode)

### Building the LCS Table
```
ALGORITHM LCSLength(X, Y)
    INPUT: Sequences X[1..m] and Y[1..n]
    OUTPUT: Length of LCS and DP table
    
    1. m ← length(X), n ← length(Y)
    2. L[0..m][0..n] ← 0
    3. FOR i ← 1 TO m DO
    4.     FOR j ← 1 TO n DO
    5.         IF X[i] = Y[j] THEN
    6.             L[i][j] ← L[i-1][j-1] + 1
    7.         ELSE
    8.             L[i][j] ← max(L[i-1][j], L[i][j-1])
    9.         END IF
    10.    END FOR
    11. END FOR
    12. RETURN L[m][n], L
```

### Reconstructing the LCS
```
ALGORITHM PrintLCS(L, X, Y, i, j)
    INPUT: DP table L, sequences X, Y, indices i, j
    OUTPUT: The actual LCS string
    
    1. IF i = 0 OR j = 0 THEN
    2.     RETURN ""
    3. IF X[i] = Y[j] THEN
    4.     RETURN PrintLCS(L, X, Y, i-1, j-1) + X[i]
    5. ELSE IF L[i-1][j] > L[i][j-1] THEN
    6.     RETURN PrintLCS(L, X, Y, i-1, j)
    7. ELSE
    8.     RETURN PrintLCS(L, X, Y, i, j-1)
    9. END IF
```

### Step-by-Step Walkthrough

**Example:** X = "ABCBDAB", Y = "BDCAB"

```
    ""   B   D   C   A   B
""   0   0   0   0   0   0
A    0   0   0   0   1   1
B    0   1   1   1   1   2
C    0   1   1   2   2   2
B    0   1   1   2   2   3
D    0   1   2   2   2   3
A    0   1   2   2   3   3
B    0   1   2   2   3   4

LCS length = 4
LCS = "BDAB" (or "BCAB")
```

**Backtracking:**
- Start at L[7][5] = 4
- X[7]='B' = Y[5]='B' → include, go diagonal
- X[6]='A' = Y[4]='A' → include, go diagonal
- X[5]='D' = Y[2]='D' → include, go diagonal
- X[1]='B' = Y[1]='B' → include, go diagonal
- Result: "BDAB" (reversed)

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Null Safety:** Returns null for null inputs, empty string for empty inputs
- **String Splitting:** Converts strings to character arrays for indexing
- **Reconstruction:** Builds LCS by backtracking through the DP table

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/dynamicprogramming/LongestCommonSubsequence.java`

```java
public static String getLCS(String str1, String str2) {
    if (str1 == null || str2 == null) return null;
    if (str1.isEmpty() || str2.isEmpty()) return "";
    
    int[][] lcsMatrix = new int[str1.length() + 1][str2.length() + 1];
    
    // Build LCS matrix
    for (int i = 1; i <= str1.length(); i++) {
        for (int j = 1; j <= str2.length(); j++) {
            if (str1.charAt(i-1) == str2.charAt(j-1)) {
                lcsMatrix[i][j] = lcsMatrix[i-1][j-1] + 1;
            } else {
                lcsMatrix[i][j] = Math.max(lcsMatrix[i-1][j], lcsMatrix[i][j-1]);
            }
        }
    }
    return lcsString(str1, str2, lcsMatrix);
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Version Control Systems
**Use Case:** Computing file differences and merges  
**Example:** Git uses LCS-based diff algorithms (Myers diff) for comparing file versions

### 2. Bioinformatics
**Use Case:** DNA/protein sequence alignment  
**Example:** BLAST algorithm for finding similar biological sequences uses LCS variants

### 3. Spell Checkers
**Use Case:** Suggesting corrections based on similarity  
**Example:** Word processors use LCS to suggest similar words for typos

### 4. Plagiarism Detection
**Use Case:** Comparing documents for similarity  
**Example:** Turnitin and similar services use LCS-based algorithms

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **GitHub** | Pull request diffs and merge conflict detection |
| **Beyond Compare** | File and folder comparison tool |
| **NCBI BLAST** | Biological sequence database search |
| **Grammarly** | Text similarity and correction suggestions |

---

## ⚖️ Comparison with Related Algorithms

| Algorithm | Problem | Time | Output |
|-----------|---------|------|--------|
| **LCS** | Longest Common Subsequence | O(mn) | Subsequence |
| **Edit Distance** | Min edits to transform | O(mn) | Integer |
| **LCSubstring** | Longest Common Substring | O(mn) | Contiguous substring |
| **Longest Palindromic Subsequence** | LCS(X, reverse(X)) | O(n²) | Subsequence |

### Key Differences

- **LCS vs Substring:** LCS allows gaps; substring must be contiguous
- **LCS vs Edit Distance:** LCS finds similarity; edit distance measures difference
- **Relationship:** $EditDistance(X,Y) = m + n - 2 \times LCS(X,Y)$

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Off-by-One Errors:** Indexing mismatch between 0-based and 1-based
   - Solution: Initialize table with size (m+1) × (n+1)

2. **Reconstruction Errors:** Wrong backtracking direction
   - Solution: Check all three conditions (match, up, left) carefully

3. **Case Sensitivity:** "ABC" vs "abc" may need normalization
   - Solution: Normalize strings before comparison if needed

4. **Large Inputs:** Memory issues with very long strings
   - Solution: Use space-optimized version or Hirschberg's algorithm

### Edge Cases to Handle

- [x] Empty strings → returns ""
- [x] Null inputs → returns null
- [x] No common characters → returns ""
- [x] Identical strings → returns the string itself
- [x] One string contains the other → returns smaller string

---

## 📖 References

1. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 15.4
2. Hirschberg, D.S. "A linear space algorithm for computing maximal common subsequences" (1975)
3. [Wikipedia: Longest Common Subsequence](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem)

---

## 🔗 Related Algorithms

- [Edit Distance](./edit-distance.md) - Levenshtein distance
- [Longest Palindromic Subsequence](./longest-palindromic-subsequence.md) - Special case of LCS
- [Shortest Common Supersequence](./shortest-common-supersequence.md) - SCS length = m + n - LCS
- [Diff Algorithm](../../09-string-algorithms/diff.md) - Uses LCS for file comparison
