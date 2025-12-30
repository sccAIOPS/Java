# Fast Fourier Transform (FFT)

> **Category:** Mathematical Algorithms  
> **Subcategory:** Signal Processing / Polynomial Multiplication  
> **Implementation:** [`FFT.java`](../../../src/main/java/com/thealgorithms/maths/FFT.java)

---

## 📚 Overview

The Fast Fourier Transform (FFT) is an algorithm that computes the Discrete Fourier Transform (DFT) and its inverse efficiently. The most common variant, the Cooley-Tukey algorithm, reduces the complexity from O(n²) to O(n log n), making it one of the most important algorithms in computer science and signal processing.

**Key Characteristics:**
- Transforms signals between time and frequency domains
- Enables O(n log n) polynomial multiplication
- Fundamental for audio/image processing, data compression
- Uses complex number arithmetic and divide-and-conquer

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** The Discrete Fourier Transform of a sequence $(x_0, x_1, ..., x_{n-1})$ is defined as:
> $$X_k = \sum_{j=0}^{n-1} x_j \cdot e^{-2\pi i \cdot jk/n}$$

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Linearity | DFT is linear | $\mathcal{F}(ax + by) = a\mathcal{F}(x) + b\mathcal{F}(y)$ |
| Periodicity | Periodic with period n | $X_{k+n} = X_k$ |
| Conjugate Symmetry | For real input | $X_{n-k} = X_k^*$ |
| Parseval's Theorem | Energy preservation | $\sum|x_j|^2 = \frac{1}{n}\sum|X_k|^2$ |

### Mathematical Formulation

**Principal nth Root of Unity:**

$$
\omega_n = e^{2\pi i/n} = \cos(2\pi/n) + i\sin(2\pi/n)
$$

**DFT Matrix Form:**

$$
\begin{bmatrix}
X_0 \\ X_1 \\ \vdots \\ X_{n-1}
\end{bmatrix}
=
\begin{bmatrix}
1 & 1 & 1 & \cdots & 1 \\
1 & \omega & \omega^2 & \cdots & \omega^{n-1} \\
1 & \omega^2 & \omega^4 & \cdots & \omega^{2(n-1)} \\
\vdots & & & \ddots & \\
1 & \omega^{n-1} & \omega^{2(n-1)} & \cdots & \omega^{(n-1)^2}
\end{bmatrix}
\begin{bmatrix}
x_0 \\ x_1 \\ \vdots \\ x_{n-1}
\end{bmatrix}
$$

**Cooley-Tukey Decomposition:**

$$
X_k = E_k + \omega_n^k \cdot O_k
$$
$$
X_{k+n/2} = E_k - \omega_n^k \cdot O_k
$$

Where $E_k$ is DFT of even-indexed elements and $O_k$ is DFT of odd-indexed elements.

### Recurrence Relation

$$
T(n) = 2T(n/2) + O(n) \Rightarrow T(n) = O(n \log n)
$$

### Proof of Correctness

**Key Insight:** The nth roots of unity satisfy:
- $(\omega_n)^n = 1$
- $\omega_n^{n/2} = -1$ (for even n)
- $\omega_{2n}^{2k} = \omega_n^k$ (halving lemma)

These properties enable the divide-and-conquer approach.

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **All Cases** | $O(n \log n)$ | For n = power of 2 |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Recursive** | $O(n \log n)$ | Call stack + temporary arrays |
| **Iterative** | $O(n)$ | In-place with bit reversal |

### Additional Properties

| Property | Value |
|----------|-------|
| **In-place** | Yes (iterative version) |
| **Numerically Stable** | Generally yes |
| **Input Size** | Must be power of 2 (or pad) |

### Detailed Analysis

**Naive DFT:** $O(n^2)$ - n outputs, each requiring n multiplications

**FFT (Cooley-Tukey):**
- Recursive depth: $\log_2 n$
- Work per level: $O(n)$
- Total: $O(n \log n)$

**Comparison for n = 1,000,000:**
- DFT: ~10¹² operations
- FFT: ~20 × 10⁶ operations
- Speedup: ~50,000×

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM FFT-Recursive(x)
─────────────────────────────────────────────────────
    INPUT:  Array x of n complex numbers (n = power of 2)
    OUTPUT: DFT of x
─────────────────────────────────────────────────────

    1. n ← length(x)
    2. IF n = 1 THEN
    3.     RETURN x
    4. END IF
    
    5. ω_n ← e^(2πi/n)
    6. ω ← 1
    
    7. x_even ← (x[0], x[2], x[4], ..., x[n-2])
    8. x_odd  ← (x[1], x[3], x[5], ..., x[n-1])
    
    9. y_even ← FFT-Recursive(x_even)
    10. y_odd  ← FFT-Recursive(x_odd)
    
    11. y ← array of size n
    12. FOR k ← 0 TO n/2 - 1 DO
    13.     y[k]       ← y_even[k] + ω · y_odd[k]
    14.     y[k + n/2] ← y_even[k] - ω · y_odd[k]
    15.     ω ← ω · ω_n
    16. END FOR
    17. RETURN y


ALGORITHM FFT-Iterative(x)
─────────────────────────────────────────────────────
    INPUT:  Array x of n complex numbers (n = power of 2)
    OUTPUT: DFT of x (in-place)
─────────────────────────────────────────────────────

    1. n ← length(x)
    2. // Bit-reversal permutation
    3. FOR i ← 0 TO n-1 DO
    4.     j ← bit-reverse(i, log₂(n))
    5.     IF i < j THEN
    6.         SWAP(x[i], x[j])
    7.     END IF
    8. END FOR
    
    9. // Cooley-Tukey iterations
    10. FOR s ← 1 TO log₂(n) DO
    11.     m ← 2^s
    12.     ω_m ← e^(2πi/m)
    13.     FOR k ← 0 TO n-1 STEP m DO
    14.         ω ← 1
    15.         FOR j ← 0 TO m/2 - 1 DO
    16.             t ← ω · x[k + j + m/2]
    17.             u ← x[k + j]
    18.             x[k + j] ← u + t
    19.             x[k + j + m/2] ← u - t
    20.             ω ← ω · ω_m
    21.         END FOR
    22.     END FOR
    23. END FOR
    24. RETURN x
```

### Step-by-Step Walkthrough

**Example Input:** x = [1, 2, 3, 4] (n = 4)

**Recursive Call Tree:**
```
FFT([1, 2, 3, 4])
├── FFT([1, 3])       ← even indices
│   ├── FFT([1])      → [1]
│   └── FFT([3])      → [3]
│   Result: [4, -2]
└── FFT([2, 4])       ← odd indices
    ├── FFT([2])      → [2]
    └── FFT([4])      → [4]
    Result: [6, -2]
```

**Butterfly Combination (level 2):**
- ω₄ = e^(2πi/4) = i
- Y[0] = E[0] + ω⁰·O[0] = 4 + 1·6 = 10
- Y[1] = E[1] + ω¹·O[1] = -2 + i·(-2) = -2 - 2i
- Y[2] = E[0] - ω⁰·O[0] = 4 - 6 = -2
- Y[3] = E[1] - ω¹·O[1] = -2 - i·(-2) = -2 + 2i

**Result:** X = [10, -2-2i, -2, -2+2i]

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses inner `Complex` class for complex arithmetic
- Implements Cooley-Tukey radix-2 DIT algorithm
- Bit-reversal permutation for in-place operation
- Padding to power of 2 for arbitrary input sizes

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/maths/FFT.java`

```java
// Key code snippet - Complex class
public static class Complex {
    private double real, img;
    
    public Complex multiply(Complex z) {
        double newReal = real * z.real - img * z.img;
        double newImg = real * z.img + img * z.real;
        return new Complex(newReal, newImg);
    }
    
    public Complex add(Complex z) {
        return new Complex(real + z.real, img + z.img);
    }
}

// Key code snippet - FFT core
public static void fft(ArrayList<Complex> x, boolean inverse) {
    int n = x.size();
    // Bit reversal permutation
    fftBitReversal(x);
    
    // Cooley-Tukey iterations
    for (int len = 2; len <= n; len *= 2) {
        double angle = 2 * Math.PI / len * (inverse ? -1 : 1);
        Complex wLen = new Complex(Math.cos(angle), Math.sin(angle));
        
        for (int i = 0; i < n; i += len) {
            Complex w = new Complex(1, 0);
            for (int j = 0; j < len / 2; j++) {
                Complex u = x.get(i + j);
                Complex v = x.get(i + j + len / 2).multiply(w);
                x.set(i + j, u.add(v));
                x.set(i + j + len / 2, u.subtract(v));
                w = w.multiply(wLen);
            }
        }
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Audio Processing
**Use Case:** Spectral analysis, noise reduction, equalization  
**Example:** Shazam uses FFT for audio fingerprinting

### 2. Image Processing
**Use Case:** Filtering, compression, feature detection  
**Example:** JPEG uses DCT (closely related to FFT)

### 3. Signal Processing
**Use Case:** Radar, telecommunications, medical imaging  
**Example:** MRI reconstruction uses FFT

### 4. Polynomial Multiplication
**Use Case:** Large integer multiplication (Schönhage-Strassen)  
**Example:** GMP library uses FFT for big number multiplication

### 5. Data Compression
**Use Case:** Transform coding in audio/video codecs  
**Example:** MP3 uses Modified DCT (MDCT)

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Shazam | Audio fingerprinting |
| Adobe Photoshop | Image filtering |
| MATLAB | Signal analysis |
| GMP/OpenSSL | Large integer arithmetic |
| ffmpeg | Audio/video processing |
| Medical imaging | MRI/CT reconstruction |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | FFT (Cooley-Tukey) | DFT (Naive) | NTT | DCT |
|--------|-------------------|-------------|-----|-----|
| Time Complexity | O(n log n) | O(n²) | O(n log n) | O(n log n) |
| Number System | Complex | Complex | Integer mod p | Real |
| Precision | Floating point | Floating point | Exact | Floating point |
| Best For | General signals | Small n | Exact arithmetic | Image compression |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Input Size:** Must be power of 2 (pad if necessary)
2. **Floating Point Errors:** Accumulate in long transforms
3. **Normalization:** Inverse FFT needs 1/n factor
4. **Aliasing:** Sample rate must be ≥ 2× max frequency (Nyquist)

### Edge Cases to Handle
- [ ] n = 1 (trivial transform)
- [ ] Non-power-of-2 input (needs padding)
- [ ] Very large n (numerical stability)
- [ ] All-zero input

---

## 📖 References

1. Cooley, J.W., Tukey, J.W. (1965). "An Algorithm for the Machine Calculation of Complex Fourier Series". Mathematics of Computation.
2. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
3. [Wikipedia - Fast Fourier Transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform)

---

## 🔗 Related Algorithms

- [Number Theoretic Transform](./ntt.md) - Integer-only FFT variant
- [Discrete Cosine Transform](./dct.md) - Real-valued transform
- [Convolution](./convolution.md) - Uses FFT for O(n log n) convolution
- [Polynomial Multiplication](./polynomial-multiplication.md) - Key FFT application
