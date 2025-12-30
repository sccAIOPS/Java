# Longest Increasing Subsequence (LIS)

> **Category:** Dynamic Programming  
> **Subcategory:** Sequence Algorithms  
> **Implementation:** [LongestIncreasingSubsequence.java](../../src/main/java/com/thealgorithms/dynamicprogramming/LongestIncreasingSubsequence.java)

---

## 📚 Overview

The **Longest Increasing Subsequence (LIS)** problem finds the longest subsequence of a given sequence where elements are in strictly increasing order. Unlike substrings, subsequences are not required to be contiguous. This problem has efficient O(n log n) solutions and applications in patience sorting and related algorithms.

---

## 🔢 Mathematical Foundation

### Definition

Given sequence $A = \langle a_1, a_2, ..., a_n \rangle$, find the longest subsequence $\langle a_{i_1}, a_{i_2}, ..., a_{i_k} \rangle$ where:
- $i_1 < i_2 < ... < i_k$ (indices are increasing)
- $a_{i_1} < a_{i_2} < ... < a_{i_k}$ (values are strictly increasing)

### Key Properties

- **Multiple Solutions:** There may be multiple LIS of the same length
- **Not Contiguous:** Elements don't need to be adjacent
- **Strictly Increasing:** Equal elements don't count (use ≤ for non-decreasing)

### Recurrence Relation (O(n²) DP)

Let $dp[i]$ = length of LIS ending at index i:

$$
dp[i] = 1 + \max_{0 \leq j < i, a_j < a_i} dp[j]
$$

Base case: $dp[i] = 1$ (element itself)

---

## 📊 Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| **Brute Force** | $O(2^n)$ | $O(n)$ |
| **O(n²) DP** | $O(n^2)$ | $O(n)$ |
| **Binary Search + DP** | $O(n \log n)$ | $O(n)$ |
| **Patience Sorting** | $O(n \log n)$ | $O(n)$ |

### Detailed Analysis (O(n log n) Approach)

The optimal approach maintains a **tail array** where `tail[i]` is the smallest ending element of an LIS of length i+1:
- For each element, binary search for its position in tail
- Update tail array accordingly
- Final answer is the length of tail array

---

## 🔄 Algorithm (Pseudocode)

### O(n²) DP Approach
```
ALGORITHM LIS_DP(A)
    INPUT: Array A[1..n]
    OUTPUT: Length of longest increasing subsequence
    
    1. n ← length(A)
    2. dp[1..n] ← 1                    // Each element is LIS of length 1
    3. FOR i ← 2 TO n DO
    4.     FOR j ← 1 TO i-1 DO
    5.         IF A[j] < A[i] THEN
    6.             dp[i] ← max(dp[i], dp[j] + 1)
    7.         END IF
    8.     END FOR
    9. END FOR
    10. RETURN max(dp[1..n])
```

### O(n log n) Binary Search Approach
```
ALGORITHM LIS_BinarySearch(A)
    INPUT: Array A[1..n]
    OUTPUT: Length of longest increasing subsequence
    
    1. n ← length(A)
    2. IF n = 0 RETURN 0
    3. tail[1] ← A[1]
    4. length ← 1
    5. FOR i ← 2 TO n DO
    6.     IF A[i] < tail[1] THEN
    7.         tail[1] ← A[i]           // New smallest
    8.     ELSE IF A[i] > tail[length] THEN
    9.         tail[++length] ← A[i]    // Extend LIS
    10.    ELSE
    11.        pos ← BinarySearchCeiling(tail, 1, length, A[i])
    12.        tail[pos] ← A[i]         // Replace to allow future extension
    13.    END IF
    14. END FOR
    15. RETURN length
```

### Step-by-Step Walkthrough

**Example:** A = [10, 9, 2, 5, 3, 7, 101, 18]

**O(n log n) trace:**
```
i=0: A[0]=10  → tail = [10]           length=1
i=1: A[1]=9   → tail = [9]            length=1  (9<10, replace)
i=2: A[2]=2   → tail = [2]            length=1  (2<9, replace)
i=3: A[3]=5   → tail = [2, 5]         length=2  (5>2, extend)
i=4: A[4]=3   → tail = [2, 3]         length=2  (3 replaces 5)
i=5: A[5]=7   → tail = [2, 3, 7]      length=3  (7>3, extend)
i=6: A[6]=101 → tail = [2, 3, 7, 101] length=4  (101>7, extend)
i=7: A[7]=18  → tail = [2, 3, 7, 18]  length=4  (18 replaces 101)

LIS length = 4
One valid LIS: [2, 3, 7, 101] or [2, 5, 7, 18]
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Binary Search:** Custom `upperBound` function finds insertion position
- **Tail Array:** Maintains smallest endings for each LIS length
- **Two Methods:** `lis()` and `findLISLen()` with slightly different implementations

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/dynamicprogramming/LongestIncreasingSubsequence.java`

```java
public static int lis(int[] array) {
    int len = array.length;
    if (len == 0) return 0;
    
    int[] tail = new int[len];
    int length = 1;
    tail[0] = array[0];
    
    for (int i = 1; i < len; i++) {
        if (array[i] < tail[0]) {
            tail[0] = array[i];  // New smallest
        } else if (array[i] > tail[length - 1]) {
            tail[length++] = array[i];  // Extend
        } else {
            // Replace element using binary search
            tail[upperBound(tail, -1, length - 1, array[i])] = array[i];
        }
    }
    return length;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Version Control & Dependencies
**Use Case:** Finding longest compatible dependency chain  
**Example:** Package managers resolving version constraints

### 2. Data Analysis
**Use Case:** Identifying trends in time series data  
**Example:** Stock price trend analysis, finding longest upward trends

### 3. Bioinformatics
**Use Case:** Gene sequence analysis  
**Example:** Finding conserved regions across species

### 4. Task Scheduling
**Use Case:** Maximizing sequential tasks with dependencies  
**Example:** Build systems determining optimal compilation order

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **npm/Maven** | Dependency version resolution |
| **Bloomberg** | Financial time series analysis |
| **NCBI** | Genomic sequence alignment preprocessing |
| **Atlassian Jira** | Sprint planning with task dependencies |

---

## ⚖️ Comparison with Related Algorithms

| Problem | Constraint | Time | Application |
|---------|------------|------|-------------|
| **LIS** | Strictly increasing | O(n log n) | Trend analysis |
| **LDS** | Strictly decreasing | O(n log n) | Reverse trend |
| **LNDS** | Non-decreasing (≤) | O(n log n) | Allowing ties |
| **LCS** | Two sequences | O(mn) | Diff/alignment |
| **Maximum Sum Increasing** | Sum, not length | O(n²) | Optimization |

### Connection to Other Problems

- **Dilworth's Theorem:** Minimum number of decreasing subsequences = LIS length
- **Patience Sorting:** LIS length = number of piles in patience sort
- **LCS Reduction:** LIS(A) = LCS(A, sorted(A)) when elements are distinct

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Non-Strict Increase:** Using ≥ instead of > changes the problem
   - Solution: Be clear about strictly vs non-strictly increasing

2. **Binary Search Off-by-One:** Wrong position calculation
   - Solution: Use `lower_bound` for non-strict, `upper_bound` for strict

3. **Empty Array:** No elements means no subsequence
   - Solution: Return 0 for empty input

4. **All Same Elements:** [5,5,5,5] has LIS length 1
   - Solution: Strictly increasing means no ties

### Edge Cases to Handle

- [x] Empty array → returns 0
- [x] Single element → returns 1
- [x] Already sorted ascending → returns n
- [x] Already sorted descending → returns 1
- [x] All same elements → returns 1

---

## 📖 References

1. Fredman, M.L. "On computing the length of longest increasing subsequences" (1975)
2. Aldous, D., Diaconis, P. "Longest increasing subsequences" (1999)
3. [Wikipedia: Longest Increasing Subsequence](https://en.wikipedia.org/wiki/Longest_increasing_subsequence)

---

## 🔗 Related Algorithms

- [Longest Common Subsequence](./longest-common-subsequence.md) - Two-sequence version
- [Patience Sort](../01-sorting-algorithms/patience-sort.md) - Related sorting algorithm
- [Maximum Sum Increasing Subsequence](./maximum-sum-increasing-subsequence.md) - Sum variant
- [Longest Bitonic Subsequence](./longest-bitonic-subsequence.md) - Increasing then decreasing
