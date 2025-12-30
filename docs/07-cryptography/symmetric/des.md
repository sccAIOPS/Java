# Data Encryption Standard (DES)

> **Category:** Cryptography / Symmetric Key
> **Implementation:** [DES.java](../../../src/main/java/com/thealgorithms/ciphers/DES.java)

## Overview

The **Data Encryption Standard (DES)** is a symmetric-key block cipher that was adopted as a federal standard in the United States in 1977. While now considered insecure due to its small key size, DES remains an important algorithm for educational purposes and forms the basis for understanding modern block ciphers.

DES uses a **Feistel network structure** with 16 rounds of encryption. The algorithm takes a 64-bit plaintext block and a 64-bit key (56 effective bits + 8 parity bits) to produce a 64-bit ciphertext block.

### Key Properties

| Property | Value |
|----------|-------|
| **Type** | Symmetric Block Cipher |
| **Block Size** | 64 bits (8 bytes) |
| **Key Size** | 56 bits (64 bits with parity) |
| **Rounds** | 16 |
| **Structure** | Feistel Network |
| **Published** | 1977 (FIPS 46) |
| **Status** | Deprecated (use AES or 3DES) |

---

## Mathematical Foundation

### Feistel Network Structure

DES uses a balanced Feistel cipher where each round applies:

$$L_i = R_{i-1}$$
$$R_i = L_{i-1} \oplus F(R_{i-1}, K_i)$$

Where:
- $L_i$, $R_i$ are left and right halves after round $i$
- $K_i$ is the subkey for round $i$
- $F$ is the Feistel function
- $\oplus$ denotes XOR operation

### Key Schedule

The 56-bit key generates 16 subkeys of 48 bits each:

1. **PC-1 Permutation**: 64 → 56 bits (remove parity)
2. **Split**: Divide into two 28-bit halves (C₀, D₀)
3. **Rotation**: Left circular shift by 1 or 2 positions
4. **PC-2 Permutation**: Select 48 bits from 56

$$K_i = PC2(LS_i(C_{i-1}) \| LS_i(D_{i-1}))$$

### The Feistel Function F

The function $F(R, K)$ operates as:

1. **Expansion (E)**: 32 bits → 48 bits
   - Each 4-bit block becomes 6 bits with overlapping
2. **Key Mixing**: XOR with round subkey
   $$B = E(R) \oplus K$$
3. **S-Box Substitution**: 48 bits → 32 bits
   - Eight S-boxes, each 6 bits → 4 bits
   - Row from bits 1 and 6, column from bits 2-5
4. **Permutation (P)**: 32 bits → 32 bits

$$F(R, K) = P(S(E(R) \oplus K))$$

---

## Complexity Analysis

### Time Complexity

| Operation | Per Round | Total (16 rounds) |
|-----------|-----------|-------------------|
| Initial Permutation | O(64) | O(64) |
| Expansion | O(32) | O(512) |
| XOR with Subkey | O(48) | O(768) |
| S-Box Lookup | O(8) | O(128) |
| P-Box Permutation | O(32) | O(512) |
| Final Permutation | O(64) | O(64) |
| **Total** | | **O(1)** per block |

**Key Schedule:** O(16 × 56) = O(896)

### Space Complexity

| Component | Size |
|-----------|------|
| Initial/Final Permutation Tables | 64 bytes each |
| S-Boxes | 8 × 64 = 512 bytes |
| P-Box | 32 bytes |
| Expansion Table | 48 bytes |
| PC-1, PC-2 Tables | 56 + 48 = 104 bytes |
| Subkeys | 16 × 48 bits = 96 bytes |
| **Total** | **~920 bytes** |

---

## Algorithm Pseudocode

### DES Encryption

```
function DES_ENCRYPT(plaintext, key):
    subkeys ← KEY_SCHEDULE(key)
    
    // Initial Permutation
    block ← IP(plaintext)
    L₀ ← left 32 bits of block
    R₀ ← right 32 bits of block
    
    // 16 Feistel rounds
    for i ← 1 to 16:
        Lᵢ ← Rᵢ₋₁
        Rᵢ ← Lᵢ₋₁ XOR F(Rᵢ₋₁, subkeys[i])
    
    // Swap and Final Permutation
    preOutput ← R₁₆ || L₁₆
    ciphertext ← IP⁻¹(preOutput)
    
    return ciphertext
```

### Feistel Function

```
function F(R, K):
    expanded ← E(R)           // 32 → 48 bits
    mixed ← expanded XOR K    // Key mixing
    
    substituted ← empty
    for i ← 0 to 7:
        block ← mixed[i*6 : i*6+6]
        row ← block[0] || block[5]    // 2 bits
        col ← block[1:5]              // 4 bits
        substituted += S[i][row][col]
    
    output ← P(substituted)   // Final permutation
    return output
```

### Key Schedule

```
function KEY_SCHEDULE(key):
    // Initial permutation (remove parity bits)
    permuted ← PC1(key)
    C₀ ← permuted[0:28]
    D₀ ← permuted[28:56]
    
    subkeys ← []
    shifts ← [1,1,2,2,2,2,2,2,1,2,2,2,2,2,2,1]
    
    for i ← 1 to 16:
        Cᵢ ← LEFT_ROTATE(Cᵢ₋₁, shifts[i])
        Dᵢ ← LEFT_ROTATE(Dᵢ₋₁, shifts[i])
        subkeys[i] ← PC2(Cᵢ || Dᵢ)
    
    return subkeys
```

---

## Implementation Notes

### From DES.java

**Permutation Tables:**

```java
// Permutation table to convert initial 64-bit key to 56 bit key
private static final int[] PC1 = {57, 49, 41, 33, 25, 17, 9, 1, 
    58, 50, 42, 34, 26, 18, 10, 2, ...};

// Table to convert the 56 bit subkeys to 48 bit subkeys
private static final int[] PC2 = {14, 17, 11, 24, 1, 5, 3, 28, 
    15, 6, 21, 10, 23, 19, 12, 4, ...};

// Lookup table for key rotation shifts
private static final int[] KEY_SHIFTS = {1, 1, 2, 2, 2, 2, 2, 2, 
    1, 2, 2, 2, 2, 2, 2, 1};
```

**S-Box Definition:**

```java
private static final int[][] S1 = {
    {14, 4, 13, 1, 2, 15, 11, 8, 3, 10, 6, 12, 5, 9, 0, 7},
    {0, 15, 7, 4, 14, 2, 13, 1, 10, 6, 12, 11, 9, 5, 3, 8},
    {4, 1, 14, 8, 13, 6, 2, 11, 15, 12, 9, 7, 3, 10, 5, 0},
    {15, 12, 8, 2, 4, 9, 1, 7, 5, 11, 3, 14, 10, 0, 6, 13}
};
```

**Feistel Function Implementation:**

```java
private String feistel(String messageBlock, String key) {
    StringBuilder expandedKey = new StringBuilder();
    for (int i = 0; i < 48; i++) {
        expandedKey.append(messageBlock.charAt(EXPANSION[i] - 1));
    }
    String mixedKey = xOR(expandedKey.toString(), key);
    
    // S-Box substitution
    StringBuilder substitutedString = new StringBuilder();
    for (int i = 0; i < 48; i += 6) {
        String block = mixedKey.substring(i, i + 6);
        int row = (block.charAt(0) - 48) * 2 + (block.charAt(5) - 48);
        int col = (block.charAt(1) - 48) * 8 + (block.charAt(2) - 48) * 4 
                + (block.charAt(3) - 48) * 2 + (block.charAt(4) - 48);
        String substitutedBlock = pad(Integer.toBinaryString(S[i/6][row][col]), 4);
        substitutedString.append(substitutedBlock);
    }
    
    // Apply permutation
    StringBuilder permutedString = new StringBuilder();
    for (int i = 0; i < 32; i++) {
        permutedString.append(substitutedString.charAt(PERMUTATION[i] - 1));
    }
    return permutedString.toString();
}
```

**Encryption Block:**

```java
private String encryptBlock(String message, String[] keys) {
    StringBuilder permutedMessage = new StringBuilder();
    for (int i = 0; i < 64; i++) {
        permutedMessage.append(message.charAt(IP[i] - 1));
    }
    String e0 = permutedMessage.substring(0, 32);
    String f0 = permutedMessage.substring(32);
    
    // 16 Feistel rounds
    for (int i = 0; i < 16; i++) {
        String eN = f0;
        String fN = xOR(e0, feistel(f0, keys[i]));
        e0 = eN;
        f0 = fN;
    }
    
    // Reverse and apply inverse permutation
    String combinedBlock = f0 + e0;
    // Apply IP_INVERSE...
    return permutedMessage.toString();
}
```

---

## Real-World Applications

### Historical Usage

| Era | Application | Notes |
|-----|-------------|-------|
| 1977-2000 | Federal government encryption | FIPS standard |
| Banking | ATM PIN encryption | Still in some legacy systems |
| 3DES | Extended lifespan | Triple application of DES |
| Educational | Learning block ciphers | Foundation for understanding |

### Legacy Systems

- **ATM networks**: Some still use DES for PIN blocks
- **Financial messaging**: SWIFT uses 3DES for authentication
- **Smart cards**: Older EMV cards may use 3DES

### Why DES is Deprecated

| Year | Event |
|------|-------|
| 1998 | EFF "Deep Crack" - brute force in 56 hours |
| 1999 | Distributed.net broke DES in 22 hours |
| 2005 | NIST withdrew DES as standard |
| 2024 | Modern GPU can break DES in hours |

---

## Comparison with Related Algorithms

| Feature | DES | 3DES | AES | Blowfish |
|---------|-----|------|-----|----------|
| Block Size | 64 bits | 64 bits | 128 bits | 64 bits |
| Key Size | 56 bits | 112/168 bits | 128/192/256 | 32-448 bits |
| Rounds | 16 | 48 | 10/12/14 | 16 |
| Security | Broken | Adequate | Strong | Strong |
| Speed | Moderate | Slow | Fast | Fast |
| Hardware | DES chips | Limited | AES-NI | None |

### Triple DES (3DES)

3DES applies DES three times with different keys:

$$C = E_{K_3}(D_{K_2}(E_{K_1}(P)))$$

- **2-key 3DES**: K₁ = K₃, 112-bit effective
- **3-key 3DES**: 168-bit effective

---

## Common Pitfalls & Edge Cases

### Security Issues

1. **Key Size is Fatal**
   ```java
   // DES key is only 56 bits - brute force is trivial
   // Modern attack: ~2^56 operations = hours on GPU cluster
   
   // SOLUTION: Use AES-256 instead
   Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding");
   ```

2. **Weak Keys**
   ```java
   // DES has 4 weak keys that produce identical subkeys
   // Weak keys: 0x0000000000000000, 0xFFFFFFFFFFFFFFFF, etc.
   
   // SOLUTION: Always use random key generation
   KeyGenerator keyGen = KeyGenerator.getInstance("DES");
   keyGen.init(56);
   SecretKey key = keyGen.generateKey();
   ```

3. **Semi-Weak Key Pairs**
   ```java
   // 6 pairs where E_K1(P) = E_K2(P) for all P
   // Must avoid in implementations
   ```

### Implementation Pitfalls

| Issue | Impact | Mitigation |
|-------|--------|------------|
| ECB Mode | Pattern leakage | Use CBC or CTR |
| Parity Bits | Compatibility issues | Validate key format |
| Padding Oracle | Key recovery | Use authenticated encryption |
| 64-bit Block | Birthday attack at 2³² blocks | Limit data per key |

### Edge Cases in Implementation

```java
// Handle message length not multiple of 8 bytes
if (l % 8 != 0) {
    int desiredLength = (l / 8 + 1) * 8;
    l = desiredLength;
    message = padLast(message, desiredLength);
}
```

```java
// Validate key length
private void sanitize(String key) {
    int length = key.length();
    if (length != 64) {
        throw new IllegalArgumentException(
            "DES key must be supplied as a 64 character binary string");
    }
}
```

---

## Security Analysis

### Known Attacks

| Attack | Complexity | Notes |
|--------|------------|-------|
| Brute Force | 2^56 | Practical in hours |
| Differential Cryptanalysis | 2^47 | Theoretical |
| Linear Cryptanalysis | 2^43 | Theoretical |
| Davies Attack | 2^52 | Specific conditions |

### Why the Feistel Structure is Important

1. **Reversibility**: Decryption uses same structure, reversed keys
2. **Round Function**: F doesn't need to be invertible
3. **Design**: Enables independent analysis of components

### Design Criteria (DES S-Boxes)

1. Each S-box has 6 inputs and 4 outputs
2. No output bit is too close to a linear function
3. Changing one input bit changes at least two output bits
4. S(x) and S(x ⊕ 001100) differ in at least two bits
5. For any non-zero e, S(x) ≠ S(x ⊕ e) for at most 8 values of x

---

## References

### Academic Sources

1. NIST FIPS 46-3 (1999). *Data Encryption Standard (DES)*
2. Coppersmith, D. (1994). *The Data Encryption Standard (DES) and its strength against attacks*
3. Biham, E., & Shamir, A. (1991). *Differential Cryptanalysis of DES-like Cryptosystems*

### Online Resources

- [Wikipedia: DES](https://en.wikipedia.org/wiki/Data_Encryption_Standard)
- [NIST Cryptographic Standards](https://csrc.nist.gov/projects/cryptographic-standards-and-guidelines)

### Related Algorithms

- [AES](aes.md) - Modern replacement for DES
- [RSA](../asymmetric/rsa.md) - Asymmetric encryption
- [Caesar Cipher](../classic/caesar.md) - Classical substitution cipher
