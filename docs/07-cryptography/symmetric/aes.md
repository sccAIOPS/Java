# Advanced Encryption Standard (AES)

> **Category:** Cryptography / Symmetric Key
> **Implementation:** [AES.java](../../../src/main/java/com/thealgorithms/ciphers/AES.java)

## Overview

The **Advanced Encryption Standard (AES)**, also known as **Rijndael**, is a symmetric block cipher adopted by the U.S. National Institute of Standards and Technology (NIST) in 2001. It has become the worldwide standard for encryption, replacing the older DES standard.

AES operates on fixed block sizes of 128 bits and supports key sizes of 128, 192, or 256 bits. The algorithm consists of multiple rounds of substitution, permutation, and mixing operations applied to the data block.

### Key Properties

| Property | Value |
|----------|-------|
| **Type** | Symmetric Block Cipher |
| **Block Size** | 128 bits (16 bytes) |
| **Key Sizes** | 128, 192, or 256 bits |
| **Rounds** | 10 (128-bit), 12 (192-bit), 14 (256-bit) |
| **Structure** | Substitution-Permutation Network (SPN) |
| **Published** | 2001 (NIST FIPS 197) |

---

## Mathematical Foundation

### Galois Field Arithmetic (GF(2⁸))

AES performs arithmetic in the Galois Field $GF(2^8)$, where:
- Elements are polynomials of degree ≤ 7 with coefficients in {0, 1}
- Addition is bitwise XOR
- Multiplication uses the irreducible polynomial:

$$m(x) = x^8 + x^4 + x^3 + x + 1$$

### The State Matrix

A 128-bit input block is arranged as a 4×4 matrix of bytes (the "state"):

$$
\text{State} = \begin{bmatrix}
s_{0,0} & s_{0,1} & s_{0,2} & s_{0,3} \\
s_{1,0} & s_{1,1} & s_{1,2} & s_{1,3} \\
s_{2,0} & s_{2,1} & s_{2,2} & s_{2,3} \\
s_{3,0} & s_{3,1} & s_{3,2} & s_{3,3}
\end{bmatrix}
$$

### Round Transformations

#### 1. SubBytes (S-Box Substitution)

Each byte $b$ is replaced using the S-Box:
1. Compute multiplicative inverse: $b^{-1}$ in $GF(2^8)$
2. Apply affine transformation:

$$s'_i = s_i \oplus s_{(i+4) \mod 8} \oplus s_{(i+5) \mod 8} \oplus s_{(i+6) \mod 8} \oplus s_{(i+7) \mod 8} \oplus c_i$$

#### 2. ShiftRows

Row $i$ is cyclically shifted left by $i$ positions:

$$
\begin{bmatrix}
s'_{0,j} \\ s'_{1,j} \\ s'_{2,j} \\ s'_{3,j}
\end{bmatrix} = \begin{bmatrix}
s_{0,j} \\ s_{1,(j+1) \mod 4} \\ s_{2,(j+2) \mod 4} \\ s_{3,(j+3) \mod 4}
\end{bmatrix}
$$

#### 3. MixColumns

Each column is multiplied by a fixed matrix in $GF(2^8)$:

$$
\begin{bmatrix}
s'_{0,j} \\ s'_{1,j} \\ s'_{2,j} \\ s'_{3,j}
\end{bmatrix} = \begin{bmatrix}
02 & 03 & 01 & 01 \\
01 & 02 & 03 & 01 \\
01 & 01 & 02 & 03 \\
03 & 01 & 01 & 02
\end{bmatrix} \begin{bmatrix}
s_{0,j} \\ s_{1,j} \\ s_{2,j} \\ s_{3,j}
\end{bmatrix}
$$

#### 4. AddRoundKey

XOR the state with the round key:

$$S' = S \oplus K_i$$

---

## Complexity Analysis

### Time Complexity

| Operation | Per Round | Total (10 rounds) |
|-----------|-----------|-------------------|
| SubBytes | O(16) | O(160) |
| ShiftRows | O(16) | O(160) |
| MixColumns | O(16) | O(144) (skipped last round) |
| AddRoundKey | O(16) | O(176) (including initial) |
| **Total** | | **O(n)** per block |

**Key Expansion:** O(44) word operations for AES-128

### Space Complexity

| Component | Size |
|-----------|------|
| State | 16 bytes |
| S-Box | 256 bytes |
| Inverse S-Box | 256 bytes |
| RCON | 256 bytes |
| Round Keys | 176 bytes (AES-128) |
| **Total** | **~1 KB** |

---

## Algorithm Pseudocode

### AES Encryption

```
function AES_ENCRYPT(plaintext, key):
    state ← plaintext as 4×4 matrix
    roundKeys ← KEY_EXPANSION(key)
    
    // Initial round
    state ← ADD_ROUND_KEY(state, roundKeys[0])
    
    // Main rounds (1 to Nr-1)
    for round ← 1 to Nr-1:
        state ← SUB_BYTES(state)
        state ← SHIFT_ROWS(state)
        state ← MIX_COLUMNS(state)
        state ← ADD_ROUND_KEY(state, roundKeys[round])
    
    // Final round (no MixColumns)
    state ← SUB_BYTES(state)
    state ← SHIFT_ROWS(state)
    state ← ADD_ROUND_KEY(state, roundKeys[Nr])
    
    return state as ciphertext
```

### Key Expansion (Rijndael Key Schedule)

```
function KEY_EXPANSION(key):
    w[0..3] ← key as 4 words
    
    for i ← 4 to 43:
        temp ← w[i-1]
        if i mod 4 = 0:
            temp ← SUB_WORD(ROT_WORD(temp)) XOR RCON[i/4]
        w[i] ← w[i-4] XOR temp
    
    return w as roundKeys
```

---

## Implementation Notes

### From AES.java

The implementation uses precalculated lookup tables for efficiency:

```java
// S-Box for SubBytes transformation
private static final int[] SBOX = {
    0x63, 0x7C, 0x77, 0x7B, 0xF2, 0x6B, 0x6F, 0xC5, ...
};

// Lookup tables for MixColumns multiplication
private static final int[] MULT2 = { ... };  // Multiply by 2
private static final int[] MULT3 = { ... };  // Multiply by 3
```

**Key Expansion with Schedule Core:**

```java
public static BigInteger scheduleCore(BigInteger t, int rconCounter) {
    // Rotate the first 16 bits to the back
    String rotatingBytes = rBytes.substring(0, 2);
    String fixedBytes = rBytes.substring(2);
    rBytes = new StringBuilder(fixedBytes + rotatingBytes);
    
    // Apply S-Box to all 8-bit substrings
    for (int i = 0; i < 4; i++) {
        currentByte = SBOX[currentByte];
        if (i == 0) {
            currentByte = currentByte ^ RCON[rconCounter];
        }
    }
    return new BigInteger(rBytes.toString(), 16);
}
```

**Encryption Process:**

```java
public static BigInteger encrypt(BigInteger plainText, BigInteger key) {
    BigInteger[] roundKeys = keyExpansion(key);
    
    // Initial round
    plainText = addRoundKey(plainText, roundKeys[0]);
    
    // Main rounds
    for (int i = 1; i < 10; i++) {
        plainText = subBytes(plainText);
        plainText = shiftRows(plainText);
        plainText = mixColumns(plainText);
        plainText = addRoundKey(plainText, roundKeys[i]);
    }
    
    // Final round
    plainText = subBytes(plainText);
    plainText = shiftRows(plainText);
    plainText = addRoundKey(plainText, roundKeys[10]);
    
    return plainText;
}
```

---

## Real-World Applications

### Industry Usage

| Domain | Application | Mode Used |
|--------|-------------|-----------|
| **Web Security** | TLS/SSL encryption | GCM, CBC |
| **Storage** | Full disk encryption (BitLocker, FileVault) | XTS |
| **Wi-Fi** | WPA2/WPA3 | CCM |
| **Banking** | Payment card data protection | CBC, GCM |
| **Government** | Classified communications | Various |
| **VPN** | IPSec tunnels | GCM, CBC |

### Security Standards

- **NIST FIPS 197**: AES specification
- **PCI-DSS**: Requires AES-128 minimum for card data
- **HIPAA**: Recommended for healthcare data
- **TOP SECRET**: AES-256 approved for classified data

---

## Comparison with Related Algorithms

| Feature | AES | DES | 3DES | Blowfish |
|---------|-----|-----|------|----------|
| Block Size | 128 bits | 64 bits | 64 bits | 64 bits |
| Key Size | 128/192/256 | 56 bits | 168 bits | 32-448 bits |
| Rounds | 10/12/14 | 16 | 48 | 16 |
| Structure | SPN | Feistel | Feistel | Feistel |
| Speed (SW) | Fast | Slow | Very Slow | Fast |
| Security | Strong | Broken | Adequate | Strong |
| Hardware Support | AES-NI | No | No | No |

### Mode of Operation Comparison

| Mode | Parallelizable | Random Access | Error Propagation |
|------|----------------|---------------|-------------------|
| ECB | Yes | Yes | None |
| CBC | Encrypt: No, Decrypt: Yes | Yes | 1-2 blocks |
| CTR | Yes | Yes | Same bit |
| GCM | Yes | Yes | Authentication fail |
| XTS | Yes | Yes | Same block |

---

## Common Pitfalls & Edge Cases

### Security Pitfalls

1. **ECB Mode Usage**
   ```java
   // WRONG: ECB mode reveals patterns
   Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
   
   // RIGHT: Use authenticated encryption
   Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding");
   ```

2. **Weak Key Generation**
   ```java
   // WRONG: Predictable key
   byte[] key = "mypassword123456".getBytes();
   
   // RIGHT: Derive key with KDF
   SecretKeyFactory factory = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA256");
   KeySpec spec = new PBEKeySpec(password, salt, 310000, 256);
   ```

3. **IV/Nonce Reuse**
   ```java
   // WRONG: Static IV
   byte[] iv = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
   
   // RIGHT: Random IV for each encryption
   byte[] iv = new byte[12];
   secureRandom.nextBytes(iv);
   ```

### Implementation Notes

| Issue | Impact | Solution |
|-------|--------|----------|
| Padding Oracle | Key recovery | Use authenticated encryption |
| Timing Attack | Key extraction | Constant-time implementation |
| Cache Attack | Key extraction | AES-NI hardware |
| Related-Key Attack | Key recovery | Key derivation functions |

### Current Implementation Limitations

1. **128-bit only**: The implementation only supports AES-128
2. **No modes**: Only raw block cipher, no CBC/GCM/CTR
3. **BigInteger overhead**: String-based operations are slower than byte arrays
4. **No padding**: Must handle block alignment externally

---

## Security Considerations

### Recommended Practices

1. **Key Size**: Use AES-256 for long-term security
2. **Mode**: Use GCM for authenticated encryption
3. **IV Management**: Never reuse IV with same key
4. **Key Derivation**: Use PBKDF2, Argon2, or scrypt for password-based keys
5. **Random Generation**: Use `SecureRandom` for IVs and keys

### Known Vulnerabilities

| Attack | Applicability | Mitigation |
|--------|---------------|------------|
| Biclique | Theoretical (AES-128: 2^126.1) | Use AES-256 |
| Related-Key | Weak key schedules | Key derivation |
| Side-Channel | Physical access | Hardware AES |

---

## References

### Academic Sources

1. Daemen, J., & Rijmen, V. (2002). *The Design of Rijndael: AES - The Advanced Encryption Standard*
2. NIST FIPS 197 (2001). *Advanced Encryption Standard (AES)*
3. Bogdanov, A., et al. (2011). *Biclique Cryptanalysis of the Full AES*

### Online Resources

- [NIST AES Specification](https://csrc.nist.gov/publications/detail/fips/197/final)
- [Wikipedia: AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [Computerphile: AES Explained](https://www.youtube.com/watch?v=O4xNJsjtN6E)

### Related Algorithms

- [DES](des.md) - Predecessor symmetric cipher
- [RSA](../asymmetric/rsa.md) - Asymmetric encryption
- [Diffie-Hellman](../asymmetric/diffie-hellman.md) - Key exchange
