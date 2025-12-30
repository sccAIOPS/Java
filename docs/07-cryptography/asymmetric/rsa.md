# RSA Cryptosystem

> **Category:** Cryptography / Asymmetric Key
> **Implementation:** [RSA.java](../../../src/main/java/com/thealgorithms/ciphers/RSA.java)

## Overview

**RSA** (Rivest-Shamir-Adleman) is one of the first practical public-key cryptosystems, widely used for secure data transmission. The algorithm is based on the practical difficulty of factoring the product of two large prime numbers.

In RSA, the encryption key is public, while the decryption key is kept private. This asymmetric property enables secure communication without prior key exchange and forms the foundation for digital signatures.

### Key Properties

| Property | Value |
|----------|-------|
| **Type** | Asymmetric (Public-Key) |
| **Key Sizes** | 1024, 2048, 3072, 4096 bits |
| **Security Basis** | Integer Factorization Problem |
| **Published** | 1977 |
| **Inventors** | Ron Rivest, Adi Shamir, Leonard Adleman |
| **Use Cases** | Key exchange, Digital signatures, Encryption |

---

## Mathematical Foundation

### Number Theory Prerequisites

#### Euler's Totient Function

For $n = pq$ where $p, q$ are distinct primes:

$$\phi(n) = (p-1)(q-1)$$

#### Euler's Theorem

For any integer $a$ coprime to $n$:

$$a^{\phi(n)} \equiv 1 \pmod{n}$$

#### Modular Multiplicative Inverse

$d$ is the modular inverse of $e$ modulo $\phi(n)$ if:

$$ed \equiv 1 \pmod{\phi(n)}$$

### RSA Key Generation

1. **Choose primes**: Select two large random primes $p$ and $q$
2. **Compute modulus**: $n = p \times q$
3. **Compute totient**: $\phi(n) = (p-1)(q-1)$
4. **Choose public exponent**: Select $e$ such that $1 < e < \phi(n)$ and $\gcd(e, \phi(n)) = 1$
5. **Compute private exponent**: $d = e^{-1} \bmod \phi(n)$

**Public Key**: $(e, n)$
**Private Key**: $(d, n)$

### Encryption and Decryption

**Encryption** (using public key):
$$c = m^e \bmod n$$

**Decryption** (using private key):
$$m = c^d \bmod n$$

### Correctness Proof

By Euler's theorem, since $ed \equiv 1 \pmod{\phi(n)}$:

$$ed = 1 + k\phi(n)$$ for some integer $k$

Therefore:
$$c^d = (m^e)^d = m^{ed} = m^{1+k\phi(n)} = m \cdot (m^{\phi(n)})^k \equiv m \cdot 1^k \equiv m \pmod{n}$$

---

## Complexity Analysis

### Time Complexity

| Operation | Complexity | Notes |
|-----------|------------|-------|
| Key Generation | O(k⁴) | k = key bits, probabilistic |
| Prime Generation | O(k³ × log k) | Miller-Rabin test |
| Encryption | O(k² × log e) | Modular exponentiation |
| Decryption | O(k³) | Using CRT optimization: O(k³/4) |
| Modular Inverse | O(k²) | Extended Euclidean Algorithm |

### Space Complexity

| Component | Size |
|-----------|------|
| Public Key (n, e) | 2k bits |
| Private Key (d) | k bits |
| Plaintext Block | < k bits |
| Ciphertext Block | k bits |

### Key Size Recommendations (NIST)

| Security Level | RSA Key Size | Equivalent AES |
|----------------|--------------|----------------|
| 80 bits | 1024 bits | Not recommended |
| 112 bits | 2048 bits | AES-128 |
| 128 bits | 3072 bits | AES-128 |
| 192 bits | 7680 bits | AES-192 |
| 256 bits | 15360 bits | AES-256 |

---

## Algorithm Pseudocode

### Key Generation

```
function GENERATE_RSA_KEYS(bits):
    // Step 1: Generate two large primes
    p ← GENERATE_PRIME(bits/2)
    q ← GENERATE_PRIME(bits/2)
    
    // Step 2: Compute modulus
    n ← p × q
    
    // Step 3: Compute Euler's totient
    φ(n) ← (p - 1) × (q - 1)
    
    // Step 4: Choose public exponent
    e ← 3  // Common choices: 3, 17, 65537
    while GCD(e, φ(n)) ≠ 1:
        e ← e + 2
    
    // Step 5: Compute private exponent
    d ← MODULAR_INVERSE(e, φ(n))
    
    public_key ← (e, n)
    private_key ← (d, n)
    
    return (public_key, private_key)
```

### Encryption

```
function RSA_ENCRYPT(message, public_key):
    (e, n) ← public_key
    m ← message as integer
    
    if m ≥ n:
        ERROR "Message too large for key size"
    
    c ← MODULAR_EXPONENTIATION(m, e, n)
    return c
```

### Decryption

```
function RSA_DECRYPT(ciphertext, private_key):
    (d, n) ← private_key
    c ← ciphertext
    
    m ← MODULAR_EXPONENTIATION(c, d, n)
    return m as message
```

### Modular Exponentiation (Square-and-Multiply)

```
function MODULAR_EXPONENTIATION(base, exponent, modulus):
    result ← 1
    base ← base mod modulus
    
    while exponent > 0:
        if exponent is odd:
            result ← (result × base) mod modulus
        exponent ← exponent >> 1
        base ← (base × base) mod modulus
    
    return result
```

---

## Implementation Notes

### From RSA.java

**Key Generation:**

```java
public final synchronized void generateKeys(int bits) {
    SecureRandom random = new SecureRandom();
    BigInteger p = new BigInteger(bits / 2, 100, random);  // Prime with certainty
    BigInteger q = new BigInteger(bits / 2, 100, random);
    modulus = p.multiply(q);
    
    BigInteger phi = (p.subtract(BigInteger.ONE))
                    .multiply(q.subtract(BigInteger.ONE));
    
    // Choose e coprime to phi
    publicKey = BigInteger.valueOf(3L);
    while (phi.gcd(publicKey).intValue() > 1) {
        publicKey = publicKey.add(BigInteger.TWO);
    }
    
    // Compute modular inverse
    privateKey = publicKey.modInverse(phi);
}
```

**Encryption:**

```java
public synchronized String encrypt(String message) {
    if (message.isEmpty()) {
        throw new IllegalArgumentException("Message is empty");
    }
    return (new BigInteger(message.getBytes()))
           .modPow(publicKey, modulus).toString();
}

public synchronized BigInteger encrypt(BigInteger message) {
    return message.modPow(publicKey, modulus);
}
```

**Decryption:**

```java
public synchronized String decrypt(String encryptedMessage) {
    if (encryptedMessage.isEmpty()) {
        throw new IllegalArgumentException("Message is empty");
    }
    return new String(
        (new BigInteger(encryptedMessage))
        .modPow(privateKey, modulus).toByteArray()
    );
}

public synchronized BigInteger decrypt(BigInteger encryptedMessage) {
    return encryptedMessage.modPow(privateKey, modulus);
}
```

---

## Real-World Applications

### Common Use Cases

| Application | Usage | Key Operation |
|-------------|-------|---------------|
| **TLS/SSL** | Server authentication | Signature verification |
| **SSH** | Key-based login | Signature + key exchange |
| **PGP/GPG** | Email encryption | Hybrid encryption |
| **Digital Signatures** | Document signing | Sign with private key |
| **Code Signing** | Software integrity | Signature verification |
| **S/MIME** | Secure email | Certificate-based |

### Hybrid Encryption

RSA is typically used with symmetric encryption:

```
SENDER:
1. Generate random AES key K
2. Encrypt message: C = AES_ENCRYPT(K, message)
3. Encrypt key: K_enc = RSA_ENCRYPT(public_key, K)
4. Send: (K_enc, C)

RECEIVER:
1. Decrypt key: K = RSA_DECRYPT(private_key, K_enc)
2. Decrypt message: message = AES_DECRYPT(K, C)
```

### Digital Signatures

RSA can create unforgeable signatures:

```
SIGN (private key):
    signature = hash(message)^d mod n

VERIFY (public key):
    computed_hash = signature^e mod n
    return computed_hash == hash(message)
```

---

## Comparison with Related Algorithms

### Asymmetric Algorithms

| Feature | RSA | DSA | ECDSA | Ed25519 |
|---------|-----|-----|-------|---------|
| Key Size (128-bit security) | 3072 bits | 3072 bits | 256 bits | 256 bits |
| Encryption | Yes | No | No | No |
| Signatures | Yes | Yes | Yes | Yes |
| Key Generation | Slow | Fast | Fast | Fast |
| Sign | Slow | Fast | Fast | Fast |
| Verify | Fast | Slow | Slow | Fast |
| Security Basis | Factoring | DLP | ECDLP | ECDLP |

### Key Exchange Comparison

| Algorithm | Security Basis | Key Size | Forward Secrecy |
|-----------|----------------|----------|-----------------|
| RSA Key Transport | Factoring | 2048+ | No |
| Diffie-Hellman | DLP | 2048+ | Yes |
| ECDH | ECDLP | 256+ | Yes |
| RSA-OAEP | Factoring | 2048+ | No |

---

## Common Pitfalls & Edge Cases

### Security Vulnerabilities

1. **Textbook RSA (No Padding)**
   ```java
   // WRONG: Raw RSA is deterministic and malleable
   c = m.modPow(e, n);
   
   // RIGHT: Use OAEP padding
   Cipher cipher = Cipher.getInstance("RSA/ECB/OAEPWithSHA-256AndMGF1Padding");
   ```

2. **Small Public Exponent Attacks**
   ```java
   // WRONG: e = 3 with small message
   // If m³ < n, then c = m³ and m = ∛c (no modular math needed)
   
   // RIGHT: Use proper padding or e = 65537
   publicKey = BigInteger.valueOf(65537L);
   ```

3. **Common Modulus Attack**
   ```java
   // WRONG: Same n, different e for multiple users
   // Attackers can decrypt any message
   
   // RIGHT: Each user gets unique (n, e, d)
   ```

4. **Timing Attacks**
   ```java
   // WRONG: Variable-time modular exponentiation
   
   // RIGHT: Constant-time implementation or blinding
   // Blinding: compute (m * r^e)^d * r^(-1) instead of m^d
   ```

### Implementation Pitfalls

| Issue | Attack | Mitigation |
|-------|--------|------------|
| No padding | Deterministic | OAEP, PKCS#1 v2 |
| Small e | Coppersmith | e = 65537 |
| Small d | Wiener's attack | d > n^(1/4) |
| p ≈ q | Fermat factorization | |p - q| > 2n^(1/4) |
| Shared primes | GCD attack | Good random generator |

### Current Implementation Limitations

1. **Starting with e=3**: Vulnerable to broadcast attack
2. **No padding**: Textbook RSA is insecure
3. **No CRT optimization**: Decryption slower than necessary
4. **String conversion**: Inefficient for large messages

---

## Security Considerations

### Prime Generation Security

```java
// Ensure primes are truly random and large enough
SecureRandom random = new SecureRandom();
BigInteger p = new BigInteger(bits / 2, 100, random);  // 100 = certainty
```

**Primality Testing:**
- Miller-Rabin test with sufficient iterations
- Certainty parameter affects probability: P(composite) ≤ 2^(-certainty)

### Recommended Practices

1. **Key Size**: Minimum 2048 bits, prefer 4096 for long-term security
2. **Public Exponent**: Use 65537 (0x10001) - Fermat prime F4
3. **Padding**: Always use OAEP (Optimal Asymmetric Encryption Padding)
4. **Random Generation**: Use cryptographically secure RNG
5. **Key Storage**: Protect private keys with hardware security modules (HSM)

### Known Attacks Summary

| Attack | Target | Complexity | Countermeasure |
|--------|--------|------------|----------------|
| Brute Force | Key | O(√n) | Large key size |
| Wiener | Small d | O(n^(1/4)) | Large d |
| Coppersmith | Small e, short msg | Polynomial | Proper padding |
| Bleichenbacher | PKCS#1 v1.5 | Oracle | OAEP padding |
| Fault Attack | CRT implementation | Physical | Signature verification |

---

## References

### Academic Sources

1. Rivest, R., Shamir, A., & Adleman, L. (1978). *A Method for Obtaining Digital Signatures and Public-Key Cryptosystems*
2. Boneh, D. (1999). *Twenty Years of Attacks on the RSA Cryptosystem*
3. PKCS #1 v2.2: RSA Cryptography Standard (RFC 8017)

### Online Resources

- [Wikipedia: RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))
- [NIST Key Management Guidelines (SP 800-57)](https://csrc.nist.gov/publications/detail/sp/800-57-part-1/rev-5/final)

### Related Algorithms

- [Diffie-Hellman](diffie-hellman.md) - Key exchange protocol
- [AES](../symmetric/aes.md) - Symmetric encryption for hybrid schemes
- [DES](../symmetric/des.md) - Historical symmetric cipher
