# Caesar Cipher

> **Category:** Cryptography / Classic Cipher
> **Implementation:** [Caesar.java](../../../src/main/java/com/thealgorithms/ciphers/Caesar.java)

## Overview

The **Caesar Cipher** is one of the simplest and most widely known encryption techniques. It is a type of substitution cipher where each letter in the plaintext is replaced by a letter a fixed number of positions down the alphabet.

Named after Julius Caesar, who reportedly used it with a shift of 3 to protect military messages, it represents a foundational concept in cryptography despite offering minimal security by modern standards.

### Key Properties

| Property | Value |
|----------|-------|
| **Type** | Monoalphabetic Substitution |
| **Key Space** | 26 possible shifts (25 non-trivial) |
| **Alphabet** | 26 letters (A-Z) |
| **Historical Use** | Roman military communications |
| **Security Level** | Trivially breakable |
| **Educational Value** | High - introduces core concepts |

---

## Mathematical Foundation

### Shift Cipher Model

The Caesar cipher operates on the integers modulo 26 (representing A=0, B=1, ..., Z=25).

**Encryption:**
$$E(x) = (x + k) \bmod 26$$

**Decryption:**
$$D(x) = (x - k) \bmod 26 = (x + (26 - k)) \bmod 26$$

Where:
- $x$ = numeric value of plaintext character
- $k$ = shift key (0-25)
- $E(x)$ = encrypted character value
- $D(x)$ = decrypted character value

### Example with Shift 3 (Classical Caesar)

```
Plaintext:  A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
                ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓
Ciphertext: D E F G H I J K L M N O P Q R S T U V W X Y Z A B C

Message:    HELLO WORLD
Encrypted:  KHOOR ZRUOG
```

### Group Theory Perspective

The Caesar cipher forms a cyclic group under composition:
- The set of all Caesar ciphers forms $\mathbb{Z}_{26}$
- Composition of two shifts: $(k_1 + k_2) \bmod 26$
- Identity element: shift of 0
- Inverse: shift of $(26 - k) \bmod 26$

---

## Complexity Analysis

### Time Complexity

| Operation | Complexity | Notes |
|-----------|------------|-------|
| Encryption | O(n) | Single pass through message |
| Decryption | O(n) | Single pass through message |
| Brute Force | O(26n) = O(n) | Try all 25 shifts |

### Space Complexity

| Operation | Space | Notes |
|-----------|-------|-------|
| Encryption | O(n) | Output string storage |
| Decryption | O(n) | Output string storage |
| In-place | O(1) | If modifying input array |

### Breaking Complexity

| Attack | Time | Success Rate |
|--------|------|--------------|
| Brute Force | O(26n) | 100% |
| Frequency Analysis | O(n) | ~95%+ |
| Known Plaintext | O(1) | 100% |

---

## Algorithm Pseudocode

### Encryption

```
function CAESAR_ENCRYPT(message, shift):
    shift ← shift mod 26  // Normalize to valid range
    result ← empty string
    
    for each character c in message:
        if c is uppercase letter:
            // Shift within A-Z (ASCII 65-90)
            shifted ← ((c - 'A' + shift) mod 26) + 'A'
            result ← result + shifted
        else if c is lowercase letter:
            // Shift within a-z (ASCII 97-122)
            shifted ← ((c - 'a' + shift) mod 26) + 'a'
            result ← result + shifted
        else:
            // Non-alphabetic: preserve as-is
            result ← result + c
    
    return result
```

### Decryption

```
function CAESAR_DECRYPT(ciphertext, shift):
    // Decryption is encryption with inverse shift
    return CAESAR_ENCRYPT(ciphertext, 26 - (shift mod 26))
```

### Brute Force Attack

```
function CAESAR_BRUTE_FORCE(ciphertext):
    results ← empty list
    
    for shift from 0 to 25:
        decrypted ← CAESAR_DECRYPT(ciphertext, shift)
        results.append((shift, decrypted))
    
    return results  // Human inspection needed to identify correct one
```

### Frequency Analysis Attack

```
function FREQUENCY_ATTACK(ciphertext):
    // English letter frequency: E, T, A, O, I, N, S, H, R...
    english_freq ← [12.7, 9.1, 8.2, 7.5, 7.0, 6.7, 6.3, 6.1, 6.0, ...]
    
    // Count letter frequencies in ciphertext
    cipher_freq ← COUNT_FREQUENCIES(ciphertext)
    
    best_shift ← 0
    best_score ← infinity
    
    for shift from 0 to 25:
        // Compute chi-squared statistic
        score ← CHI_SQUARED(cipher_freq, english_freq, shift)
        if score < best_score:
            best_score ← score
            best_shift ← shift
    
    return CAESAR_DECRYPT(ciphertext, best_shift)
```

---

## Implementation Notes

### From Caesar.java

**Encoding (Encryption):**

```java
public static String encode(String message, int shift) {
    shift = normalizeShift(shift);  // Handle negative shifts
    StringBuilder encoded = new StringBuilder();
    
    for (char c : message.toCharArray()) {
        if (isCapitalLatinLetter(c)) {
            int shiftedChar = (c - 'A' + shift) % 26 + 'A';
            encoded.append((char) shiftedChar);
        } else if (isSmallLatinLetter(c)) {
            int shiftedChar = (c - 'a' + shift) % 26 + 'a';
            encoded.append((char) shiftedChar);
        } else {
            encoded.append(c);  // Non-letter preserved
        }
    }
    return encoded.toString();
}
```

**Decoding (Decryption):**

```java
public static String decode(String encryptedMessage, int shift) {
    shift = normalizeShift(shift);
    StringBuilder decoded = new StringBuilder();
    
    for (char c : encryptedMessage.toCharArray()) {
        if (isCapitalLatinLetter(c)) {
            int shiftedChar = 'Z' - (25 - (c - 'A') + shift) % 26;
            decoded.append((char) shiftedChar);
        } else if (isSmallLatinLetter(c)) {
            int shiftedChar = 'z' - (25 - (c - 'a') + shift) % 26;
            decoded.append((char) shiftedChar);
        } else {
            decoded.append(c);
        }
    }
    return decoded.toString();
}
```

**Shift Normalization:**

```java
private static int normalizeShift(int shift) {
    // Handle negative shifts and values >= 26
    return ((shift % 26) + 26) % 26;
}
```

**Brute Force Attack:**

```java
public static String[] bruteforce(String encryptedMessage) {
    String[] results = new String[26];
    for (int i = 0; i < 26; i++) {
        results[i] = decode(encryptedMessage, i);
    }
    return results;
}
```

**Character Classification:**

```java
public static boolean isCapitalLatinLetter(char c) {
    return c >= 'A' && c <= 'Z';
}

public static boolean isSmallLatinLetter(char c) {
    return c >= 'a' && c <= 'z';
}
```

---

## Real-World Applications

### Historical Usage

| Era | Application | Shift Used |
|-----|-------------|------------|
| Roman Empire | Military messages | 3 (classical) |
| 19th Century | Newspaper puzzles | Various |
| Modern | Educational tool | Various |
| CTF/Puzzles | Challenge problems | Various (often ROT13) |

### ROT13 - Special Case

ROT13 (shift = 13) is self-inverse since $13 + 13 = 26 \equiv 0 \pmod{26}$:

```
ROT13("HELLO") = "URYYB"
ROT13("URYYB") = "HELLO"
```

**Uses:**
- Usenet spoiler hiding
- Email obfuscation
- Simple text obscuring (not encryption!)

### Modern Applications

The Caesar cipher itself is not used for security, but its concepts appear in:

1. **Cryptography Education**
   - Introduction to symmetric encryption
   - Modular arithmetic demonstration
   - Attack methodology (frequency analysis)

2. **Capture The Flag (CTF)**
   - Beginner challenges
   - Often combined with other techniques

3. **ROT13 Usage**
   - Forum spoiler tags
   - Email address obfuscation
   - URL encoding tricks

---

## Comparison with Related Ciphers

### Substitution Cipher Comparison

| Cipher | Key Space | Attack | Security |
|--------|-----------|--------|----------|
| Caesar | 26 | Brute force | None |
| Affine | 312 | Brute force | Minimal |
| Simple Substitution | 26! ≈ 4×10²⁶ | Frequency analysis | Low |
| Vigenère | 26^k | Kasiski/IC | Medium (historical) |
| One-Time Pad | 26^n | None* | Perfect* |

### Shift Cipher Variants

| Variant | Description | Key |
|---------|-------------|-----|
| Caesar (k=3) | Classical | Fixed: 3 |
| ROT13 (k=13) | Self-inverse | Fixed: 13 |
| General Shift | Variable key | 0-25 |
| Affine | ax + b mod 26 | (a, b) pair |
| Decimation | Multiplication only | a where gcd(a,26)=1 |

---

## Common Pitfalls & Vulnerabilities

### Security Issues

1. **Trivial Key Space**
   ```
   Only 25 meaningful shifts to try
   Even manual checking is feasible
   ```

2. **Frequency Analysis**
   ```
   In English text:
   - 'E' appears ~12.7%
   - 'T' appears ~9.1%
   - 'A' appears ~8.2%
   
   Most common ciphertext letter → likely 'E'
   ```

3. **Pattern Preservation**
   ```
   Same letters encrypt to same ciphertext
   Word patterns remain: "HELLO" → "KHOOR" (double letter visible)
   ```

### Implementation Pitfalls

| Issue | Problem | Solution |
|-------|---------|----------|
| Negative shift | Incorrect result | Normalize: ((shift % 26) + 26) % 26 |
| Non-ASCII | Incorrect shift | Explicit character range check |
| Case handling | Lost case info | Process upper/lower separately |
| Non-letters | May be shifted | Preserve non-alphabetic characters |

### Code Vulnerabilities

```java
// WRONG: Negative shift breaks math
int shifted = (c - 'A' + shift) % 26;  // -3 % 26 = -3 in Java!

// RIGHT: Normalize negative shifts
shift = ((shift % 26) + 26) % 26;
int shifted = (c - 'A' + shift) % 26;

// WRONG: Doesn't preserve non-letters
// WRONG: Modifies original string
// WRONG: Doesn't handle Unicode
```

---

## Breaking the Caesar Cipher

### Method 1: Brute Force

```java
String[] allDecryptions = Caesar.bruteforce(ciphertext);
// Examine all 26 possibilities manually
for (int i = 0; i < 26; i++) {
    System.out.println("Shift " + i + ": " + allDecryptions[i]);
}
```

### Method 2: Frequency Analysis

```java
public static int findShiftByFrequency(String ciphertext) {
    int[] freq = new int[26];
    for (char c : ciphertext.toLowerCase().toCharArray()) {
        if (c >= 'a' && c <= 'z') {
            freq[c - 'a']++;
        }
    }
    
    // Find most common letter (assume it's 'E')
    int maxIdx = 0;
    for (int i = 1; i < 26; i++) {
        if (freq[i] > freq[maxIdx]) {
            maxIdx = i;
        }
    }
    
    // 'E' is at position 4, so shift = maxIdx - 4
    return (maxIdx - 4 + 26) % 26;
}
```

### Method 3: Known Plaintext

```java
// If we know any plaintext-ciphertext pair
char plain = 'E';
char cipher = 'H';
int shift = (cipher - plain + 26) % 26;  // shift = 3
```

---

## Educational Value

### Cryptographic Concepts Introduced

1. **Symmetric Encryption**: Same key for encrypt/decrypt
2. **Key Space**: Total number of possible keys
3. **Modular Arithmetic**: Working in ℤ₂₆
4. **Brute Force Attacks**: Trying all possible keys
5. **Frequency Analysis**: Statistical attack method
6. **Known Plaintext Attack**: Using partial information

### Exercises

1. **Basic**: Encrypt "ATTACKATDAWN" with shift 7
2. **Decryption**: Decrypt "WKLQN" (hint: common English word)
3. **Analysis**: Use frequency analysis on longer ciphertext
4. **Extension**: Implement affine cipher (ax + b mod 26)

---

## References

### Historical Sources

1. Suetonius. *The Twelve Caesars* (Life of Julius Caesar, §56)
2. Singh, S. (1999). *The Code Book*

### Online Resources

- [Wikipedia: Caesar Cipher](https://en.wikipedia.org/wiki/Caesar_cipher)
- [Khan Academy: Caesar Cipher](https://www.khanacademy.org/computing/computer-science/cryptography/crypt/v/caesar-cipher)

### Related Algorithms

- [Vigenère Cipher](vigenere.md) - Polyalphabetic extension
- [AES](../symmetric/aes.md) - Modern symmetric encryption
- [ROT13](https://en.wikipedia.org/wiki/ROT13) - Special case (k=13)
