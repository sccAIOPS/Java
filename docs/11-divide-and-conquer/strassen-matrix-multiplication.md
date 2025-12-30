# Strassen's Matrix Multiplication

> **Category:** Divide and Conquer  
> **Subcategory:** Matrix Operations  
> **Implementation:** [`StrassenMatrixMultiplication.java`](../../src/main/java/com/thealgorithms/divideandconquer/StrassenMatrixMultiplication.java)

---

## 📚 Overview

Strassen's algorithm is a divide-and-conquer algorithm for matrix multiplication that improves upon the naive O(n³) algorithm. Discovered by Volker Strassen in 1969, it was the first algorithm to show that matrix multiplication can be done faster than O(n³). It achieves O(n^2.8074) by reducing the number of recursive multiplications from 8 to 7.

**Key Characteristics:**
- First algorithm to break O(n³) barrier for matrix multiplication
- Uses 7 multiplications instead of 8 (at the cost of more additions)
- Time complexity O(n^log₂7) ≈ O(n^2.8074)
- Foundation for even faster algorithms (Coppersmith-Winograd, etc.)

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given two n×n matrices A and B, compute their product C = A × B using divide-and-conquer with only 7 recursive multiplications.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Block Decomposition | Divide into 4 quadrants | $A = \begin{pmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{pmatrix}$ |
| Multiplications | Reduced from 8 to 7 | $M_1$ through $M_7$ |
| Additions | Increased from 4 to 18 | Required for combining |

### Mathematical Formulation

**Standard Block Multiplication (8 multiplications):**

$$
C = \begin{pmatrix} C_{11} & C_{12} \\ C_{21} & C_{22} \end{pmatrix} = \begin{pmatrix} A_{11}B_{11} + A_{12}B_{21} & A_{11}B_{12} + A_{12}B_{22} \\ A_{21}B_{11} + A_{22}B_{21} & A_{21}B_{12} + A_{22}B_{22} \end{pmatrix}
$$

**Strassen's 7 Products:**

$$
\begin{aligned}
M_1 &= (A_{11} + A_{22})(B_{11} + B_{22}) \\
M_2 &= (A_{21} + A_{22})B_{11} \\
M_3 &= A_{11}(B_{12} - B_{22}) \\
M_4 &= A_{22}(B_{21} - B_{11}) \\
M_5 &= (A_{11} + A_{12})B_{22} \\
M_6 &= (A_{21} - A_{11})(B_{11} + B_{12}) \\
M_7 &= (A_{12} - A_{22})(B_{21} + B_{22})
\end{aligned}
$$

**Combining the Products:**

$$
\begin{aligned}
C_{11} &= M_1 + M_4 - M_5 + M_7 \\
C_{12} &= M_3 + M_5 \\
C_{21} &= M_2 + M_4 \\
C_{22} &= M_1 - M_2 + M_3 + M_6
\end{aligned}
$$

### Recurrence Relation

$$
T(n) = 7T(n/2) + O(n^2)
$$

By Master Theorem: $T(n) = O(n^{\log_2 7}) = O(n^{2.8074...})$

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **All Cases** | $O(n^{2.8074})$ | Exact: $O(n^{\log_2 7})$ |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Recursive** | $O(n^2)$ | Temporary matrices at each level |
| **Stack Depth** | $O(\log n)$ | Recursion depth |

### Additional Properties

| Property | Value |
|----------|-------|
| **Cache Friendly** | Poor (many small matrices) |
| **Numerical Stability** | Less stable than naive |
| **Practical Crossover** | n ≈ 32-128 (switch to naive) |

### Detailed Analysis

**Naive Algorithm:**
- 8 multiplications: $T(n) = 8T(n/2) + O(n^2) = O(n^3)$

**Strassen's Algorithm:**
- 7 multiplications: $T(n) = 7T(n/2) + O(n^2) = O(n^{2.807})$

**Speedup Factor:**
- For n = 1024: $\frac{n^3}{n^{2.807}} = \frac{1024^3}{1024^{2.807}} \approx 7.4×$ speedup

**Practical Considerations:**
- Hidden constants are larger than naive
- Crossover point typically 32-128
- Modern variants achieve O(n^{2.373})

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Strassen(A, B)
─────────────────────────────────────────────────────
    INPUT:  Two n×n matrices A, B (n = power of 2)
    OUTPUT: Product matrix C = A × B
─────────────────────────────────────────────────────

    1. n ← size of A
    2. IF n ≤ CROSSOVER THEN
    3.     RETURN Naive-Multiply(A, B)
    4. END IF
    
    5. // Split matrices into quadrants
    6. (A11, A12, A21, A22) ← SPLIT(A)
    7. (B11, B12, B21, B22) ← SPLIT(B)
    
    8. // Compute the 7 Strassen products
    9. M1 ← Strassen(A11 + A22, B11 + B22)
    10. M2 ← Strassen(A21 + A22, B11)
    11. M3 ← Strassen(A11, B12 - B22)
    12. M4 ← Strassen(A22, B21 - B11)
    13. M5 ← Strassen(A11 + A12, B22)
    14. M6 ← Strassen(A21 - A11, B11 + B12)
    15. M7 ← Strassen(A12 - A22, B21 + B22)
    
    16. // Combine into result quadrants
    17. C11 ← M1 + M4 - M5 + M7
    18. C12 ← M3 + M5
    19. C21 ← M2 + M4
    20. C22 ← M1 - M2 + M3 + M6
    
    21. C ← JOIN(C11, C12, C21, C22)
    22. RETURN C


ALGORITHM SPLIT(M)
─────────────────────────────────────────────────────
    // Split n×n matrix into four (n/2)×(n/2) quadrants
    n ← size of M
    h ← n / 2
    M11 ← M[0:h, 0:h]
    M12 ← M[0:h, h:n]
    M21 ← M[h:n, 0:h]
    M22 ← M[h:n, h:n]
    RETURN (M11, M12, M21, M22)


ALGORITHM JOIN(C11, C12, C21, C22)
─────────────────────────────────────────────────────
    // Combine four (n/2)×(n/2) matrices into n×n
    RETURN matrix with C11, C12, C21, C22 as quadrants
```

### Step-by-Step Walkthrough

**Example Input:** 2×2 matrices (base case illustration)

$$
A = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}, \quad
B = \begin{pmatrix} 5 & 6 \\ 7 & 8 \end{pmatrix}
$$

**Strassen Products (for 2×2, elements are scalars):**

| Product | Formula | Value |
|---------|---------|-------|
| $M_1$ | $(1+4)(5+8)$ | $5 \times 13 = 65$ |
| $M_2$ | $(3+4) \times 5$ | $7 \times 5 = 35$ |
| $M_3$ | $1 \times (6-8)$ | $1 \times (-2) = -2$ |
| $M_4$ | $4 \times (7-5)$ | $4 \times 2 = 8$ |
| $M_5$ | $(1+2) \times 8$ | $3 \times 8 = 24$ |
| $M_6$ | $(3-1)(5+6)$ | $2 \times 11 = 22$ |
| $M_7$ | $(2-4)(7+8)$ | $(-2) \times 15 = -30$ |

**Combining:**

| Element | Formula | Value |
|---------|---------|-------|
| $C_{11}$ | $M_1 + M_4 - M_5 + M_7$ | $65 + 8 - 24 - 30 = 19$ |
| $C_{12}$ | $M_3 + M_5$ | $-2 + 24 = 22$ |
| $C_{21}$ | $M_2 + M_4$ | $35 + 8 = 43$ |
| $C_{22}$ | $M_1 - M_2 + M_3 + M_6$ | $65 - 35 - 2 + 22 = 50$ |

**Result:**

$$
C = \begin{pmatrix} 19 & 22 \\ 43 & 50 \end{pmatrix}
$$

**Verification:** Standard multiplication gives same result ✓

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Handles non-power-of-2 matrices by padding
- Uses helper methods: `add()`, `sub()`, `split()`, `join()`
- Recursive divide-and-conquer structure
- Base case typically uses naive O(n³) for small matrices

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/divideandconquer/StrassenMatrixMultiplication.java`

```java
// Key code snippet - Strassen multiplication
public int[][] multiply(int[][] a, int[][] b) {
    int n = a.length;
    
    if (n == 1) {
        return new int[][]{{a[0][0] * b[0][0]}};
    }
    
    int[][] a11, a12, a21, a22;
    int[][] b11, b12, b21, b22;
    
    // Split matrices
    // ... split code ...
    
    // Compute 7 products
    int[][] m1 = multiply(add(a11, a22), add(b11, b22));
    int[][] m2 = multiply(add(a21, a22), b11);
    int[][] m3 = multiply(a11, sub(b12, b22));
    int[][] m4 = multiply(a22, sub(b21, b11));
    int[][] m5 = multiply(add(a11, a12), b22);
    int[][] m6 = multiply(sub(a21, a11), add(b11, b12));
    int[][] m7 = multiply(sub(a12, a22), add(b21, b22));
    
    // Combine results
    int[][] c11 = add(sub(add(m1, m4), m5), m7);
    int[][] c12 = add(m3, m5);
    int[][] c21 = add(m2, m4);
    int[][] c22 = add(sub(add(m1, m3), m2), m6);
    
    return join(c11, c12, c21, c22);
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Computer Graphics
**Use Case:** Transformation matrices, rendering pipelines  
**Example:** 3D game engines with complex transformations

### 2. Scientific Computing
**Use Case:** Large-scale matrix operations in simulations  
**Example:** Climate modeling, physics simulations

### 3. Machine Learning
**Use Case:** Neural network training (matrix multiplications)  
**Example:** Deep learning frameworks (TensorFlow, PyTorch)

### 4. Cryptography
**Use Case:** Matrix-based cryptographic algorithms  
**Example:** Hill cipher, lattice-based cryptography

### 5. Signal Processing
**Use Case:** Convolution and filtering operations  
**Example:** Image processing, audio analysis

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| NVIDIA CUDA | GPU matrix libraries |
| Intel MKL | Optimized math libraries |
| TensorFlow | Neural network operations |
| MATLAB | Numerical computing |
| OpenBLAS | Linear algebra package |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Strassen | Naive | Coppersmith-Winograd | Practical (BLAS) |
|--------|----------|-------|---------------------|------------------|
| Time Complexity | O(n^2.807) | O(n³) | O(n^2.373) | O(n³) optimized |
| Numerical Stability | Fair | Good | Poor | Good |
| Cache Efficiency | Poor | Good | Very Poor | Excellent |
| Implementation | Moderate | Simple | Very Complex | Complex |
| Practical Speed | Medium | Medium | Theoretical | Fastest |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Non-Power-of-2:** Pad matrices to next power of 2
2. **Numerical Instability:** More additions increase rounding errors
3. **Memory Overhead:** Many temporary matrices
4. **Crossover Point:** Need to tune when to switch to naive

### Edge Cases to Handle
- [ ] Non-square matrices (pad to square)
- [ ] Matrix size not power of 2
- [ ] 1×1 matrices (base case)
- [ ] Empty matrices
- [ ] Very large matrices (memory management)

---

## 📖 References

1. Strassen, V. (1969). "Gaussian Elimination is not Optimal". Numerische Mathematik.
2. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
3. [Wikipedia - Strassen Algorithm](https://en.wikipedia.org/wiki/Strassen_algorithm)

---

## 🔗 Related Algorithms

- [Naive Matrix Multiplication](./matrix-multiplication.md) - O(n³) baseline
- [Coppersmith-Winograd](./coppersmith-winograd.md) - Theoretically faster
- [Divide and Conquer Framework](./divide-and-conquer.md) - General pattern
- [Matrix Exponentiation](../08-mathematical-algorithms/matrix-exponentiation.md) - Uses multiplication
