# Diffie-Hellman Key Exchange

> **Category:** Cryptography / Key Exchange
> **Implementation:** [DiffieHellman.java](../../../src/main/java/com/thealgorithms/ciphers/DiffieHellman.java)

## Overview

The **Diffie-Hellman Key Exchange** is a method of securely exchanging cryptographic keys over a public channel. Published in 1976 by Whitfield Diffie and Martin Hellman, it was one of the first practical implementations of public-key cryptography.

The protocol allows two parties who have no prior knowledge of each other to jointly establish a shared secret key over an insecure channel. This key can then be used to encrypt subsequent communications using a symmetric-key cipher.

### Key Properties

| Property | Value |
|----------|-------|
| **Type** | Key Exchange Protocol |
| **Security Basis** | Discrete Logarithm Problem (DLP) |
| **Published** | 1976 |
| **Inventors** | Whitfield Diffie, Martin Hellman |
| **Key Sizes** | 2048+ bits (finite field), 256+ bits (ECDH) |
| **Use Cases** | TLS handshake, VPN tunnels, SSH |

---

## Mathematical Foundation

### The Discrete Logarithm Problem (DLP)

Given a prime $p$, a generator $g$, and a value $A = g^a \bmod p$, finding the exponent $a$ is computationally infeasible for large $p$.

### Protocol Parameters

- **Prime $p$**: A large prime number (group modulus)
- **Generator $g$**: A primitive root modulo $p$ (base)
- **Private keys**: Secret integers $a$ (Alice) and $b$ (Bob)
- **Public values**: $A = g^a \bmod p$ and $B = g^b \bmod p$

### Key Exchange Process

```
1. Alice and Bob agree on public parameters (p, g)

2. Alice:
   - Chooses secret a
   - Computes A = g^a mod p
   - Sends A to Bob

3. Bob:
   - Chooses secret b  
   - Computes B = g^b mod p
   - Sends B to Alice

4. Shared Secret:
   - Alice computes: s = B^a mod p = (g^b)^a mod p = g^(ab) mod p
   - Bob computes:   s = A^b mod p = (g^a)^b mod p = g^(ab) mod p
```

### Mathematical Proof of Correctness

Both parties compute the same value:

$$s_{Alice} = B^a \bmod p = (g^b)^a \bmod p = g^{ab} \bmod p$$

$$s_{Bob} = A^b \bmod p = (g^a)^b \bmod p = g^{ab} \bmod p$$

Since $g^{ab} = g^{ba}$ (commutativity of exponents), both parties derive the identical shared secret.

### Security Analysis

An eavesdropper knows:
- Public parameters: $p$, $g$
- Exchanged values: $A = g^a \bmod p$, $B = g^b \bmod p$

To find the shared secret $g^{ab} \bmod p$, they must solve either:
- **DLP**: Find $a$ from $g^a \bmod p$ (infeasible)
- **CDH (Computational Diffie-Hellman)**: Compute $g^{ab}$ from $g^a$ and $g^b$ (no known efficient algorithm)

---

## Complexity Analysis

### Time Complexity

| Operation | Complexity | Notes |
|-----------|------------|-------|
| Parameter Generation | O(k⁴) | Finding safe prime |
| Public Value (g^x mod p) | O(k³) | Modular exponentiation |
| Shared Secret (A^b mod p) | O(k³) | Modular exponentiation |
| **Total Key Exchange** | O(k³) | k = bit length |

### Space Complexity

| Component | Size |
|-----------|------|
| Prime p | k bits |
| Generator g | k bits |
| Private key | k bits |
| Public value | k bits |
| Shared secret | k bits |

### Security Level Comparison

| DH Key Size | ECDH Key Size | Security Bits |
|-------------|---------------|---------------|
| 1024 bits | 160 bits | ~80 bits |
| 2048 bits | 224 bits | ~112 bits |
| 3072 bits | 256 bits | ~128 bits |
| 7680 bits | 384 bits | ~192 bits |
| 15360 bits | 521 bits | ~256 bits |

---

## Algorithm Pseudocode

### Parameter Generation

```
function GENERATE_DH_PARAMETERS(bits):
    // Generate safe prime: p = 2q + 1 where q is prime
    repeat:
        q ← RANDOM_PRIME(bits - 1)
        p ← 2 * q + 1
    until IS_PRIME(p)
    
    // Find generator of order q
    repeat:
        h ← RANDOM(2, p-2)
        g ← h^2 mod p
    until g ≠ 1
    
    return (p, g)
```

### Key Generation

```
function GENERATE_DH_KEYPAIR(p, g):
    // Private key: random integer in [2, p-2]
    private_key ← RANDOM(2, p - 2)
    
    // Public value: g^private_key mod p
    public_value ← MODULAR_EXPONENTIATION(g, private_key, p)
    
    return (private_key, public_value)
```

### Shared Secret Computation

```
function COMPUTE_SHARED_SECRET(other_public, my_private, p):
    // s = other_public^my_private mod p
    shared_secret ← MODULAR_EXPONENTIATION(other_public, my_private, p)
    
    return shared_secret
```

### Complete Protocol

```
function DIFFIE_HELLMAN_EXCHANGE():
    // Setup (public parameters)
    (p, g) ← GENERATE_DH_PARAMETERS(2048)
    
    // Alice generates her keypair
    (a, A) ← GENERATE_DH_KEYPAIR(p, g)
    
    // Bob generates his keypair
    (b, B) ← GENERATE_DH_KEYPAIR(p, g)
    
    // Exchange public values over insecure channel
    SEND(Alice → Bob: A)
    SEND(Bob → Alice: B)
    
    // Compute shared secrets
    s_alice ← COMPUTE_SHARED_SECRET(B, a, p)
    s_bob ← COMPUTE_SHARED_SECRET(A, b, p)
    
    assert s_alice == s_bob
    return s_alice  // Shared secret key
```

---

## Implementation Notes

### From DiffieHellman.java

**Class Structure:**

```java
public final class DiffieHellman {
    private final BigInteger base;    // Generator g
    private final BigInteger secret;  // Private key
    private final BigInteger prime;   // Prime modulus p
    
    public DiffieHellman(BigInteger base, BigInteger secret, BigInteger prime) {
        if (base == null || secret == null || prime == null 
            || base.signum() <= 0 || secret.signum() <= 0 || prime.signum() <= 0) {
            throw new IllegalArgumentException(
                "Base, secret, and prime must be non-null and positive values.");
        }
        this.base = base;
        this.secret = secret;
        this.prime = prime;
    }
}
```

**Public Value Calculation:**

```java
// Calculate public value: g^x mod p
public BigInteger calculatePublicValue() {
    return base.modPow(secret, prime);
}
```

**Shared Secret Computation:**

```java
// Calculate shared secret: otherPublic^secret mod p
public BigInteger calculateSharedSecret(BigInteger otherPublicValue) {
    if (otherPublicValue == null || otherPublicValue.signum() <= 0) {
        throw new IllegalArgumentException(
            "Other public value must be non-null and positive.");
    }
    return otherPublicValue.modPow(secret, prime);
}
```

### Usage Example

```java
// Shared parameters (in practice, use standard groups)
BigInteger prime = new BigInteger("23");  // Small for demo
BigInteger base = new BigInteger("5");

// Alice's setup
BigInteger aliceSecret = new BigInteger("6");
DiffieHellman alice = new DiffieHellman(base, aliceSecret, prime);
BigInteger alicePublic = alice.calculatePublicValue();  // 5^6 mod 23 = 8

// Bob's setup
BigInteger bobSecret = new BigInteger("15");
DiffieHellman bob = new DiffieHellman(base, bobSecret, prime);
BigInteger bobPublic = bob.calculatePublicValue();  // 5^15 mod 23 = 19

// Exchange public values and compute shared secret
BigInteger aliceShared = alice.calculateSharedSecret(bobPublic);  // 19^6 mod 23 = 2
BigInteger bobShared = bob.calculateSharedSecret(alicePublic);    // 8^15 mod 23 = 2

// Both compute the same shared secret: 2
```

---

## Real-World Applications

### Protocol Usage

| Protocol | DH Variant | Purpose |
|----------|------------|---------|
| **TLS 1.3** | ECDHE | Key exchange |
| **TLS 1.2** | DHE, ECDHE | Forward-secret key exchange |
| **SSH** | DH, ECDH | Session key establishment |
| **IPSec/IKE** | DH, ECDH | VPN key exchange |
| **Signal Protocol** | X3DH (ECDH) | End-to-end encryption |
| **WireGuard** | Curve25519 | VPN tunnel keys |

### Standard Groups

**Finite Field DH (RFC 7919):**
- ffdhe2048: 2048-bit MODP group
- ffdhe3072: 3072-bit MODP group
- ffdhe4096: 4096-bit MODP group

**Elliptic Curve DH:**
- P-256 (secp256r1): NIST curve
- P-384 (secp384r1): NIST curve
- Curve25519: Modern, fast curve

### Forward Secrecy

DH enables **Perfect Forward Secrecy (PFS)**:
- New ephemeral keys for each session
- Compromise of long-term key doesn't expose past sessions
- Essential for modern TLS configurations

```
Without PFS (RSA Key Transport):
- Long-term RSA key compromised → All past sessions decrypted

With PFS (DHE/ECDHE):
- Long-term key compromised → Only future sessions at risk
- Past session keys were deleted after use
```

---

## Comparison with Related Algorithms

### Key Exchange Methods

| Feature | DH | ECDH | RSA Key Transport |
|---------|-----|------|-------------------|
| Forward Secrecy | Yes (ephemeral) | Yes (ephemeral) | No |
| Key Size (128-bit) | 3072 bits | 256 bits | 3072 bits |
| Speed | Slower | Faster | Varies |
| Security Basis | DLP | ECDLP | Factoring |
| Bandwidth | Higher | Lower | Moderate |

### Elliptic Curve DH (ECDH)

ECDH provides equivalent security with smaller keys:

$$A = aG \quad \text{(Alice's public point)}$$
$$B = bG \quad \text{(Bob's public point)}$$
$$S = aB = bA = abG \quad \text{(Shared secret point)}$$

| Comparison | DH-2048 | ECDH-256 |
|------------|---------|----------|
| Security | ~112 bits | ~128 bits |
| Key Size | 2048 bits | 256 bits |
| Computation | ~2ms | ~0.5ms |
| Bandwidth | ~512 bytes | ~64 bytes |

---

## Common Pitfalls & Edge Cases

### Security Vulnerabilities

1. **Small Subgroup Attack**
   ```java
   // WRONG: Not validating received public value
   BigInteger shared = otherPublic.modPow(secret, prime);
   
   // RIGHT: Validate public value is in proper subgroup
   if (otherPublic.compareTo(BigInteger.TWO) < 0 
       || otherPublic.compareTo(prime.subtract(BigInteger.TWO)) > 0) {
       throw new SecurityException("Invalid public value");
   }
   // Also check: otherPublic^q mod p == 1 for safe primes
   ```

2. **Man-in-the-Middle (MITM)**
   ```
   Without authentication:
   Alice ←→ Mallory ←→ Bob
   
   Mallory establishes separate keys with each party
   and relays/modifies messages
   
   SOLUTION: Authenticate public values with signatures or certificates
   ```

3. **Weak Parameters**
   ```java
   // WRONG: Small or weak prime
   BigInteger prime = new BigInteger("23");
   
   // RIGHT: Use standard groups or safe primes
   // Safe prime: p = 2q + 1 where q is also prime
   // Or use NIST/IETF standard groups
   ```

4. **Static Keys (No Forward Secrecy)**
   ```java
   // WRONG: Reusing same private key across sessions
   private static final BigInteger SECRET = ...;
   
   // RIGHT: Generate ephemeral keys per session
   BigInteger ephemeralSecret = new BigInteger(bitLength, secureRandom);
   ```

### Implementation Pitfalls

| Issue | Attack | Mitigation |
|-------|--------|------------|
| No authentication | MITM | Sign public values |
| Weak prime | Discrete log attacks | Standard groups |
| Small subgroup | Key leakage | Validate public values |
| Static keys | No forward secrecy | Ephemeral keys |
| Timing side-channel | Key extraction | Constant-time implementation |

### Logjam Attack (2015)

Export-grade DH (512-bit) could be precomputed:
- One-time computation: ~1 week
- Per-connection attack: ~1 minute
- Affected 8% of HTTPS sites

**Mitigation**: Use 2048-bit DH minimum, prefer ECDH

---

## Security Considerations

### Parameter Selection

1. **Prime Generation**
   - Use safe primes: p = 2q + 1
   - Minimum 2048 bits for finite field DH
   - Prefer standardized groups

2. **Generator Selection**
   - Must be primitive root modulo p
   - For safe primes, any quadratic residue works
   - Standard value: g = 2 for many groups

3. **Private Key Generation**
   - Random: Use CSPRNG
   - Size: Same bit length as prime
   - Never reuse across sessions

### Best Practices

```java
// Good DH implementation checklist:
// 1. Use standard groups (RFC 7919 or NIST curves)
// 2. Validate received public values
// 3. Use ephemeral keys for forward secrecy
// 4. Authenticate key exchange (signatures)
// 5. Derive symmetric key with KDF (HKDF)
// 6. Clear private keys after use
```

### Key Derivation

The raw shared secret should be processed with a KDF:

```java
// WRONG: Using raw DH output as encryption key
byte[] encryptionKey = sharedSecret.toByteArray();

// RIGHT: Derive key with HKDF
HKDF hkdf = HKDF.fromHmacSha256();
byte[] derivedKey = hkdf.expand(
    hkdf.extract(salt, sharedSecret.toByteArray()),
    info,
    32  // 256-bit key
);
```

---

## Variants and Extensions

### Authenticated DH (Station-to-Station)

```
1. Alice → Bob: g^a, Sign_A(g^a)
2. Bob → Alice: g^b, Sign_B(g^a, g^b)
3. Alice verifies Bob's signature
4. Alice → Bob: Sign_A(g^a, g^b)
5. Bob verifies Alice's signature
```

### Triple DH (X3DH)

Used in Signal Protocol for asynchronous key exchange:
- Identity keys (long-term)
- Signed prekeys (medium-term)
- One-time prekeys (ephemeral)

### Password-Authenticated DH (PAKE)

- SRP (Secure Remote Password)
- OPAQUE
- Prevents offline dictionary attacks

---

## References

### Academic Sources

1. Diffie, W., & Hellman, M. (1976). *New Directions in Cryptography*
2. RFC 2631: *Diffie-Hellman Key Agreement Method*
3. RFC 7919: *Negotiated Finite Field Diffie-Hellman Ephemeral Parameters for TLS*

### Online Resources

- [Wikipedia: Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)
- [Computerphile: Diffie-Hellman](https://www.youtube.com/watch?v=NmM9HA2MQGI)

### Related Algorithms

- [RSA](rsa.md) - Public-key encryption and signatures
- [AES](../symmetric/aes.md) - Symmetric encryption using derived key
- [Caesar Cipher](../classic/caesar.md) - Historical substitution cipher
