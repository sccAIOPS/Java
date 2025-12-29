# 🔢 Mathematical Algorithms

> **Category:** Computational Mathematics  
> **Difficulty:** Beginner to Advanced  
> **Prerequisites:** Basic Mathematics, Number Theory basics

---

## 📚 Overview

Mathematical algorithms solve computational problems rooted in mathematics. They form the foundation for many other algorithms and are essential in cryptography, graphics, scientific computing, and optimization.

---

## 📊 Classification

```
Mathematical Algorithms
├── Number Theory
│   ├── Primality Testing
│   │   ├── Trial Division
│   │   ├── Sieve of Eratosthenes
│   │   ├── Miller-Rabin
│   │   └── AKS Primality Test
│   ├── GCD/LCM
│   │   ├── Euclidean Algorithm
│   │   └── Extended Euclidean
│   ├── Factorization
│   │   ├── Trial Division
│   │   ├── Pollard's Rho
│   │   └── Quadratic Sieve
│   └── Modular Arithmetic
│       ├── Modular Exponentiation
│       ├── Modular Inverse
│       └── Chinese Remainder Theorem
│
├── Geometry
│   ├── Convex Hull
│   ├── Line Intersection
│   ├── Point in Polygon
│   └── Closest Pair of Points
│
├── Linear Algebra
│   ├── Matrix Multiplication
│   ├── Gaussian Elimination
│   ├── Matrix Inversion
│   └── Eigenvalue Computation
│
├── Statistics
│   ├── Mean, Median, Mode
│   ├── Standard Deviation
│   ├── Correlation
│   └── Regression
│
└── Numerical Methods
    ├── Newton-Raphson
    ├── Simpson's Rule
    ├── FFT (Fast Fourier Transform)
    └── Monte Carlo Methods
```

---

## 📈 Key Algorithms

### Number Theory

| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| [Sieve of Eratosthenes](./number-theory/sieve-eratosthenes.md) | O(n log log n) | O(n) | Generate primes up to n |
| [Euclidean GCD](./number-theory/euclidean-gcd.md) | O(log min(a,b)) | O(1) | Find GCD |
| [Extended Euclidean](./number-theory/extended-euclidean.md) | O(log min(a,b)) | O(1) | Find GCD + coefficients |
| [Fast Exponentiation](./number-theory/fast-exponentiation.md) | O(log n) | O(1) | Compute a^n mod m |
| [Miller-Rabin](./number-theory/miller-rabin.md) | O(k log³ n) | O(1) | Probabilistic primality |

### Geometry

| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| [Graham Scan](./geometry/graham-scan.md) | O(n log n) | O(n) | Convex Hull |
| [Line Intersection](./geometry/line-intersection.md) | O(1) | O(1) | Check intersection |
| [Closest Pair](./geometry/closest-pair.md) | O(n log n) | O(n) | Nearest neighbors |

---

## 🔬 Mathematical Foundations

### Euclidean Algorithm

$$
\gcd(a, b) = \gcd(b, a \mod b)
$$

**Base case:** $\gcd(a, 0) = a$

### Sieve of Eratosthenes

Mark multiples of each prime starting from 2:
$$
\text{For each prime } p: \text{mark } p^2, p^2+p, p^2+2p, \ldots \leq n
$$

### Modular Exponentiation

$$
a^n \mod m = \begin{cases}
1 & \text{if } n = 0 \\
(a^{n/2})^2 \mod m & \text{if } n \text{ even} \\
a \cdot a^{n-1} \mod m & \text{if } n \text{ odd}
\end{cases}
$$

### Fast Fourier Transform

$$
X_k = \sum_{n=0}^{N-1} x_n \cdot e^{-i2\pi kn/N}
$$

**Time Complexity:** $O(n \log n)$ vs $O(n^2)$ for naive DFT

---

## 📁 Algorithms in This Section

### [Number Theory](./number-theory/)

| File | Algorithm | Status |
|------|-----------|--------|
| [sieve-eratosthenes.md](./number-theory/sieve-eratosthenes.md) | Sieve of Eratosthenes | 📋 Planned |
| [euclidean-gcd.md](./number-theory/euclidean-gcd.md) | Euclidean GCD | 📋 Planned |
| [fast-exponentiation.md](./number-theory/fast-exponentiation.md) | Fast Power | 📋 Planned |
| [miller-rabin.md](./number-theory/miller-rabin.md) | Miller-Rabin Primality | 📋 Planned |

### [Geometry](./geometry/)

| File | Algorithm | Status |
|------|-----------|--------|
| [convex-hull.md](./geometry/convex-hull.md) | Convex Hull | 📋 Planned |
| [line-intersection.md](./geometry/line-intersection.md) | Line Intersection | 📋 Planned |

### [Statistics](./statistics/)

| File | Algorithm | Status |
|------|-----------|--------|
| [basic-statistics.md](./statistics/basic-statistics.md) | Mean, Median, Mode | 📋 Planned |

---

## 🌍 Real-World Applications

| Algorithm | Application | Example |
|-----------|-------------|---------|
| Sieve | Cryptographic key generation | RSA prime selection |
| GCD | Fraction simplification | Calculator apps |
| Modular exponentiation | Encryption | RSA, Diffie-Hellman |
| Convex Hull | Computer graphics | Collision detection |
| FFT | Signal processing | Audio compression |
| Monte Carlo | Risk analysis | Financial modeling |

---

## 📖 References

1. Knuth, D. E. *"The Art of Computer Programming"*, Volume 2
2. Cormen, T. H., et al. *"Introduction to Algorithms"* (CLRS), Chapter 31
3. Press, W. H., et al. *"Numerical Recipes"*

---

[← Back to Main Index](../README.md)
