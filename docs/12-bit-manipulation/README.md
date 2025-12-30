# Bit Manipulation Algorithms

> **Category:** Low-Level Algorithms  
> **Difficulty:** Beginner to Intermediate  
> **Prerequisites:** Binary number system, bitwise operators

---

## 📚 Overview

Bit manipulation algorithms operate directly on the binary representation of data, providing extremely efficient solutions for many computational problems. These techniques are fundamental to systems programming, cryptography, graphics, and performance-critical applications.

**Why Learn Bit Manipulation?**
- **Performance:** Bitwise operations are among the fastest operations a CPU can perform
- **Memory Efficiency:** Bit-level operations can pack more information into less space
- **Interview Preparation:** Common topic in technical interviews at top tech companies
- **Systems Programming:** Essential for low-level programming, device drivers, and embedded systems

---

## 🗂️ Algorithms in This Category

### Fundamental Operations

| Algorithm | Description | Time Complexity | Documentation |
|-----------|-------------|-----------------|---------------|
| **Count Set Bits** | Count the number of 1s in binary representation | O(log n) | [count-set-bits.md](./count-set-bits.md) |
| **Is Power of Two** | Check if a number is a power of 2 | O(1) | [is-power-of-two.md](./is-power-of-two.md) |
| **Is Even** | Check if a number is even using LSB | O(1) | [is-even.md](./is-even.md) |
| **Single Bit Operations** | Get, set, clear, toggle specific bits | O(1) | [single-bit-operations.md](./single-bit-operations.md) |

### Bit Finding Operations

| Algorithm | Description | Time Complexity | Documentation |
|-----------|-------------|-----------------|---------------|
| **Highest Set Bit** | Find position of most significant set bit | O(log n) | [highest-set-bit.md](./highest-set-bit.md) |
| **Lowest Set Bit** | Find position of least significant set bit | O(1) | [lowest-set-bit.md](./lowest-set-bit.md) |
| **Find Nth Bit** | Get the value of the nth bit | O(1) | [find-nth-bit.md](./find-nth-bit.md) |
| **Count Leading Zeros** | Count zeros before first set bit | O(log n) | [count-leading-zeros.md](./count-leading-zeros.md) |

### Bit Transformation

| Algorithm | Description | Time Complexity | Documentation |
|-----------|-------------|-----------------|---------------|
| **Reverse Bits** | Reverse the bit order of a number | O(log n) | [reverse-bits.md](./reverse-bits.md) |
| **Swap Adjacent Bits** | Swap every pair of adjacent bits | O(1) | [swap-adjacent-bits.md](./swap-adjacent-bits.md) |
| **Bit Rotation** | Rotate bits left or right | O(1) | [bit-rotation.md](./bit-rotation.md) |
| **Gray Code Conversion** | Convert to/from Gray code | O(1) | [gray-code.md](./gray-code.md) |

### Complement Operations

| Algorithm | Description | Time Complexity | Documentation |
|-----------|-------------|-----------------|---------------|
| **One's Complement** | Flip all bits | O(1) | [ones-complement.md](./ones-complement.md) |
| **Two's Complement** | Standard negative number representation | O(1) | [twos-complement.md](./twos-complement.md) |

### Distance & Comparison

| Algorithm | Description | Time Complexity | Documentation |
|-----------|-------------|-----------------|---------------|
| **Hamming Distance** | Count different bits between two numbers | O(log n) | [hamming-distance.md](./hamming-distance.md) |
| **Count Bits Flip** | Bits to flip to convert one number to another | O(log n) | [count-bits-flip.md](./count-bits-flip.md) |
| **First Different Bit** | Find first position where bits differ | O(log n) | [first-different-bit.md](./first-different-bit.md) |

### Special Applications

| Algorithm | Description | Time Complexity | Documentation |
|-----------|-------------|-----------------|---------------|
| **Find Non-Repeating** | Find unique element using XOR | O(n) | [non-repeating-number.md](./non-repeating-number.md) |
| **Number Appearing Odd Times** | Find element with odd occurrences | O(n) | [number-odd-times.md](./number-odd-times.md) |
| **Generate Subsets** | Generate all subsets using bit masking | O(2^n) | [generate-subsets.md](./generate-subsets.md) |
| **Modulo Power of Two** | Fast modulo for power of 2 divisors | O(1) | [modulo-power-two.md](./modulo-power-two.md) |
| **Bitwise GCD** | Calculate GCD using bit operations | O(log(min(a,b))) | [bitwise-gcd.md](./bitwise-gcd.md) |

### Encoding Systems

| Algorithm | Description | Time Complexity | Documentation |
|-----------|-------------|-----------------|---------------|
| **BCD Conversion** | Binary-Coded Decimal conversion | O(log n) | [bcd-conversion.md](./bcd-conversion.md) |
| **XS-3 Conversion** | Excess-3 code conversion | O(log n) | [xs3-conversion.md](./xs3-conversion.md) |
| **Parity Check** | Check even/odd parity of bits | O(log n) | [parity-check.md](./parity-check.md) |

### Boolean Logic

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Boolean Algebra Gates** | AND, OR, NOT, XOR, NAND, NOR, XNOR | [boolean-gates.md](./boolean-gates.md) |

---

## 🔧 Key Bitwise Operators

| Operator | Symbol | Description | Example |
|----------|--------|-------------|---------|
| AND | `&` | Both bits must be 1 | `5 & 3 = 1` |
| OR | `\|` | At least one bit must be 1 | `5 \| 3 = 7` |
| XOR | `^` | Exactly one bit must be 1 | `5 ^ 3 = 6` |
| NOT | `~` | Flip all bits | `~5 = -6` |
| Left Shift | `<<` | Shift bits left | `5 << 1 = 10` |
| Right Shift | `>>` | Shift bits right (signed) | `5 >> 1 = 2` |
| Unsigned Right Shift | `>>>` | Shift right with zero fill | `-5 >>> 1` |

---

## 🌍 Real-World Applications

### 1. Systems Programming
- Memory-mapped I/O
- Device driver development
- Interrupt handling
- CPU flag manipulation

### 2. Graphics & Gaming
- Color manipulation (RGB values)
- Sprite collision detection
- Texture compression
- Fast pixel operations

### 3. Cryptography
- Block cipher implementations
- Hash function design
- Random number generation
- Checksums and CRCs

### 4. Network Programming
- IP address manipulation
- Subnet mask operations
- Protocol header parsing
- Checksum calculation

### 5. Database Systems
- Bitmap indexes
- Bloom filters
- Set membership testing
- Permission/flag storage

### 6. Embedded Systems
- Register manipulation
- Pin configuration
- Power-efficient operations
- Sensor data processing

---

## 💡 Common Bit Manipulation Tricks

```java
// Check if n is power of 2
boolean isPowerOfTwo = (n > 0) && ((n & (n - 1)) == 0);

// Get lowest set bit
int lowestBit = n & (-n);

// Clear lowest set bit
int clearLowest = n & (n - 1);

// Check if n is even
boolean isEven = (n & 1) == 0;

// Multiply by 2^k
int multiply = n << k;

// Divide by 2^k
int divide = n >> k;

// Swap two numbers without temp
a ^= b; b ^= a; a ^= b;

// Get bit at position i
int bit = (n >> i) & 1;

// Set bit at position i
n = n | (1 << i);

// Clear bit at position i
n = n & ~(1 << i);

// Toggle bit at position i
n = n ^ (1 << i);
```

---

## 📊 Complexity Reference

| Operation | Time | Space | Notes |
|-----------|------|-------|-------|
| Bitwise AND/OR/XOR | O(1) | O(1) | Constant time |
| Left/Right Shift | O(1) | O(1) | Constant time |
| Count Set Bits | O(log n) | O(1) | Can be O(1) with lookup table |
| Reverse Bits | O(log n) | O(1) | 32 or 64 iterations max |
| Generate All Subsets | O(2^n) | O(2^n) | Exponential |

---

## 📖 Recommended Learning Path

```
1. Basic Operators → 2. Single Bit Operations → 3. Count Set Bits
        ↓                      ↓                        ↓
4. Power of Two → 5. Bit Finding → 6. XOR Applications
        ↓                ↓                   ↓
7. Subset Generation → 8. Advanced Tricks → 9. Real Applications
```

---

## 📚 References

1. **"Hacker's Delight"** by Henry S. Warren Jr. - Comprehensive bit manipulation techniques
2. **"Bit Twiddling Hacks"** by Sean Eron Anderson - Stanford Graphics collection
3. **Computer Organization and Design** - Patterson & Hennessy

---

## 🔗 Related Categories

- [Mathematical Algorithms](../08-mathematical-algorithms/README.md) - Number theory foundations
- [Cryptography](../07-cryptography/README.md) - Bit manipulation in encryption
- [Data Structures](../04-data-structures/README.md) - Bloom filters, bit arrays

---

*Last updated: December 30, 2025*
