# 📝 String Algorithms

> **Category:** Text Processing  
> **Difficulty:** Intermediate to Advanced  
> **Prerequisites:** Arrays, Basic String Operations, Hash Functions

---

## 📚 Overview

String algorithms process and analyze text data. They are fundamental in text editors, search engines, bioinformatics, and data compression.

---

## 📊 Classification

```
String Algorithms
├── Pattern Matching
│   ├── Naive Search
│   ├── KMP (Knuth-Morris-Pratt)
│   ├── Boyer-Moore
│   ├── Rabin-Karp
│   └── Aho-Corasick (Multiple patterns)
│
├── String Comparison
│   ├── Edit Distance (Levenshtein)
│   ├── Longest Common Subsequence
│   ├── Longest Common Substring
│   └── Hamming Distance
│
├── String Manipulation
│   ├── Reversal
│   ├── Rotation
│   ├── Palindrome Detection
│   └── Anagram Detection
│
└── Advanced Structures
    ├── Trie
    ├── Suffix Array
    ├── Suffix Tree
    └── Z-Algorithm
```

---

## 📈 Pattern Matching Comparison

| Algorithm | Preprocessing | Search Time | Space | Best For |
|-----------|---------------|-------------|-------|----------|
| Naive | O(1) | O(nm) | O(1) | Short patterns |
| [KMP](./pattern-matching/kmp.md) | O(m) | O(n) | O(m) | Single pattern, repeated search |
| [Boyer-Moore](./pattern-matching/boyer-moore.md) | O(m + σ) | O(n/m) avg | O(σ) | Long patterns, large alphabet |
| [Rabin-Karp](./pattern-matching/rabin-karp.md) | O(m) | O(n) avg | O(1) | Multiple patterns |
| [Aho-Corasick](./pattern-matching/aho-corasick.md) | O(Σm) | O(n + z) | O(Σm) | Multiple patterns simultaneously |

*n = text length, m = pattern length, σ = alphabet size, z = matches*

---

## 🔬 Mathematical Foundation

### KMP Failure Function

For pattern P, compute failure function π where:
$$
\pi[i] = \max\{k : k < i \land P[0..k-1] = P[i-k..i-1]\}
$$

### Rabin-Karp Hash

Rolling hash for window of size m:
$$
H(s[i..i+m-1]) = \sum_{j=0}^{m-1} s[i+j] \cdot d^{m-1-j} \mod q
$$

Rolling update:
$$
H_{new} = (H_{old} - s[i] \cdot d^{m-1}) \cdot d + s[i+m]
$$

### Edit Distance Recurrence

$$
dp[i][j] = \min \begin{cases}
dp[i-1][j] + 1 & \text{(delete)} \\
dp[i][j-1] + 1 & \text{(insert)} \\
dp[i-1][j-1] + [s[i] \neq t[j]] & \text{(replace/match)}
\end{cases}
$$

---

## 📁 Algorithms in This Section

### [Pattern Matching](./pattern-matching/)

| File | Algorithm | Status |
|------|-----------|--------|
| [kmp.md](./pattern-matching/kmp.md) | KMP Algorithm | 📋 Planned |
| [boyer-moore.md](./pattern-matching/boyer-moore.md) | Boyer-Moore | 📋 Planned |
| [rabin-karp.md](./pattern-matching/rabin-karp.md) | Rabin-Karp | 📋 Planned |
| [aho-corasick.md](./pattern-matching/aho-corasick.md) | Aho-Corasick | 📋 Planned |

---

## 🌍 Real-World Applications

| Algorithm | Application | Example |
|-----------|-------------|---------|
| KMP | Text editors | Find/Replace in VS Code |
| Boyer-Moore | grep command | File searching |
| Rabin-Karp | Plagiarism detection | Turnitin |
| Edit Distance | Spell checkers | Autocorrect |
| Trie | Autocomplete | Google Search |
| Suffix Array | Bioinformatics | DNA sequence matching |

---

## 📖 References

1. Cormen, T. H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 32
2. Gusfield, D. *"Algorithms on Strings, Trees, and Sequences"*
3. Sedgewick, R. *"Algorithms"*, Part 5: Strings

---

[← Back to Main Index](../README.md)
