# CRC (Cyclic Redundancy Check)

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Error Detection  
> **Implementations:** [`CRC16.java`](../../src/main/java/com/thealgorithms/others/CRC16.java), [`CRC32.java`](../../src/main/java/com/thealgorithms/others/CRC32.java), [`CRCAlgorithm.java`](../../src/main/java/com/thealgorithms/others/CRCAlgorithm.java)

---

## 📚 Overview

Cyclic Redundancy Check (CRC) is an error-detecting code commonly used in digital networks and storage devices to detect accidental changes to raw data. CRC is based on polynomial division over finite fields.

**Key Characteristics:**
- Non-cryptographic hash function
- Fixed-size checksum (16, 32, or 64 bits)
- Detects burst errors effectively
- Used in Ethernet, ZIP, PNG, etc.

---

## 🔢 Mathematical Foundation

### Polynomial Representation

Data bits are treated as coefficients of a polynomial:
- Data: 1011001 → $x^6 + x^4 + x^3 + 1$

### CRC Calculation

$$
\text{CRC} = \text{Data} \times x^n \mod \text{Generator Polynomial}
$$

Where n = degree of generator polynomial.

### Common Generator Polynomials

| CRC | Polynomial | Hex | Detection |
|-----|------------|-----|-----------|
| CRC-16-CCITT | $x^{16}+x^{12}+x^5+1$ | 0x1021 | 99.998% |
| CRC-32 | $x^{32}+x^{26}+...+1$ | 0x04C11DB7 | 99.9999998% |

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| CRC calculation | O(n) | O(1) |
| Table lookup method | O(n) | O(256) per byte table |

---

## 🔄 Algorithm (Pseudocode)

### Basic Bit-by-Bit Method
```
ALGORITHM CRC(data, polynomial)
─────────────────────────────────────────────────────
    INPUT:  data - input bytes
            polynomial - generator polynomial
    OUTPUT: CRC checksum
─────────────────────────────────────────────────────

    // Append zeros equal to polynomial degree
    data ← data + zeros(degree(polynomial))
    
    // Initialize remainder
    remainder ← 0
    
    FOR each bit b IN data DO
        // Shift remainder left, add new bit
        remainder ← (remainder << 1) | b
        
        // If MSB is 1, XOR with polynomial
        IF MSB(remainder) = 1 THEN
            remainder ← remainder XOR polynomial
        END IF
    END FOR
    
    RETURN remainder
```

### Table-Driven Method (CRC-32)
```
ALGORITHM CRC32_Table(data)
─────────────────────────────────────────────────────
    // Pre-computed table for each byte value 0-255
    table ← GenerateCRCTable()
    
    crc ← 0xFFFFFFFF  // Initialize
    
    FOR each byte b IN data DO
        index ← (crc XOR b) AND 0xFF
        crc ← (crc >> 8) XOR table[index]
    END FOR
    
    RETURN crc XOR 0xFFFFFFFF  // Final XOR
```

---

## 💻 Implementation Notes

### Java Implementation (CRC-16)

```java
public class CRC16 {
    private static final int POLYNOMIAL = 0x8005;
    private static final int INITIAL = 0x0000;
    
    public static int calculate(byte[] data) {
        int crc = INITIAL;
        
        for (byte b : data) {
            crc ^= (b & 0xFF) << 8;
            
            for (int i = 0; i < 8; i++) {
                if ((crc & 0x8000) != 0) {
                    crc = (crc << 1) ^ POLYNOMIAL;
                } else {
                    crc <<= 1;
                }
            }
        }
        
        return crc & 0xFFFF;
    }
}
```

### Java Implementation (CRC-32 with Table)

```java
public class CRC32 {
    private static final int[] TABLE = generateTable();
    
    private static int[] generateTable() {
        int[] table = new int[256];
        int polynomial = 0xEDB88320;  // Reversed polynomial
        
        for (int i = 0; i < 256; i++) {
            int crc = i;
            for (int j = 0; j < 8; j++) {
                if ((crc & 1) != 0) {
                    crc = (crc >>> 1) ^ polynomial;
                } else {
                    crc >>>= 1;
                }
            }
            table[i] = crc;
        }
        return table;
    }
    
    public static int calculate(byte[] data) {
        int crc = 0xFFFFFFFF;
        
        for (byte b : data) {
            int index = (crc ^ b) & 0xFF;
            crc = (crc >>> 8) ^ TABLE[index];
        }
        
        return crc ^ 0xFFFFFFFF;
    }
}
```

### Code References

📁 **Source Files:**
- [`src/main/java/com/thealgorithms/others/CRC16.java`](../../src/main/java/com/thealgorithms/others/CRC16.java)
- [`src/main/java/com/thealgorithms/others/CRC32.java`](../../src/main/java/com/thealgorithms/others/CRC32.java)
- [`src/main/java/com/thealgorithms/others/CRCAlgorithm.java`](../../src/main/java/com/thealgorithms/others/CRCAlgorithm.java)

---

## 🌍 Real-World Applications

### 1. Network Protocols
**Use Case:** Ethernet frame check sequence (FCS)

### 2. Storage Systems
**Use Case:** Hard disk, SSD data integrity

### 3. File Formats
**Use Case:** ZIP, PNG, GZIP verification

### 4. Communication
**Use Case:** Bluetooth, USB, HDMI

### Industry Examples

| Application | CRC Type | Purpose |
|-------------|----------|---------|
| Ethernet | CRC-32 | Frame integrity |
| ZIP files | CRC-32 | File corruption detection |
| HDLC | CRC-16 | Frame check |
| PNG images | CRC-32 | Chunk verification |

---

## ⚖️ CRC vs Other Methods

| Method | Error Detection | Speed | Security |
|--------|-----------------|-------|----------|
| CRC | Excellent | Fast | None |
| Checksum | Good | Fastest | None |
| MD5/SHA | Excellent | Slower | Cryptographic |
| Parity | Basic | Fastest | None |

---

## 🚨 Error Detection Capabilities

CRC can detect:
- ✅ All single-bit errors
- ✅ All double-bit errors (with proper polynomial)
- ✅ Any odd number of errors
- ✅ Burst errors shorter than polynomial degree
- ❌ Cannot correct errors (only detect)

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Polynomial choice | Poor detection | Use standard polynomials |
| Initial value | Different results | Use standard initialization |
| Bit order | LSB vs MSB first | Document convention |
| Not cryptographic | Security weakness | Use for integrity only |

---

## 📖 References

1. **RFC 3309** - SCTP Checksum (CRC-32c)
2. **IEEE 802.3** - Ethernet CRC-32 Standard
3. **"A Painless Guide to CRC"** - Ross Williams

---

## 🔗 Related Algorithms

- [Luhn Algorithm](./luhn-algorithm.md)
- [Checksum](./checksum.md)
- [Hash Functions](../07-cryptography/hashing.md)

---

*Last updated: December 30, 2025*
