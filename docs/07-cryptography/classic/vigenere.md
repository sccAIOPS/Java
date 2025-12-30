# Vigenère Cipher

> **Category:** Cryptography / Classic Cipher
> **Implementation:** [Vigenere.java](../../../src/main/java/com/thealgorithms/ciphers/Vigenere.java)

## Overview

The **Vigenère Cipher** is a polyalphabetic substitution cipher that uses a keyword to vary the shift applied to each character. Unlike the Caesar cipher which uses a single shift value, the Vigenère cipher applies different shifts based on the position in the keyword, making simple frequency analysis ineffective.

Invented in the 16th century and attributed to Blaise de Vigenère, it was considered unbreakable for nearly 300 years and was dubbed "le chiffre indéchiffrable" (the indecipherable cipher).

### Key Properties

| Property | Value |
|----------|-------|
| **Type** | Polyalphabetic Substitution |
| **Key Space** | 26^k (k = keyword length) |
| **Period** | Length of keyword |
| **Security** | Broken by Kasiski examination (1863) |
| **Historical Significance** | "Unbreakable" for ~300 years |
| **Superseded By** | One-Time Pad, Modern ciphers |

---

## Mathematical Foundation

### Polyalphabetic Substitution

The Vigenère cipher applies a different Caesar shift to each character based on the corresponding keyword letter.

**Encryption:**
$$C_i = (P_i + K_{i \bmod m}) \bmod 26$$

**Decryption:**
$$P_i = (C_i - K_{i \bmod m} + 26) \bmod 26$$

Where:
- $P_i$ = plaintext character at position i (A=0, B=1, ..., Z=25)
- $K_j$ = keyword character at position j
- $C_i$ = ciphertext character at position i
- $m$ = keyword length

### Vigenère Square (Tabula Recta)

```
    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
   +---------------------------------------------------
A | A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
B | B C D E F G H I J K L M N O P Q R S T U V W X Y Z A
C | C D E F G H I J K L M N O P Q R S T U V W X Y Z A B
D | D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
E | E F G H I J K L M N O P Q R S T U V W X Y Z A B C D
... (continues for all 26 rows)
```

To encrypt: Find plaintext column, keyword row → intersection is ciphertext

### Example

```
Keyword:    LEMON (repeated)
Plaintext:  ATTACKATDAWN
Key Stream: LEMONLEMONLE

Encryption:
A + L = L    (0 + 11 = 11)
T + E = X    (19 + 4 = 23)
T + M = F    (19 + 12 = 31 mod 26 = 5)
A + O = O    (0 + 14 = 14)
C + N = P    (2 + 13 = 15)
K + L = V    (10 + 11 = 21)
A + E = E    (0 + 4 = 4)
T + M = F    (19 + 12 = 5)
D + O = R    (3 + 14 = 17)
A + N = N    (0 + 13 = 13)
W + L = H    (22 + 11 = 33 mod 26 = 7)
N + E = R    (13 + 4 = 17)

Ciphertext: LXFOPVEFRNHR
```

---

## Complexity Analysis

### Time Complexity

| Operation | Complexity | Notes |
|-----------|------------|-------|
| Encryption | O(n) | Process each character once |
| Decryption | O(n) | Process each character once |
| Kasiski Examination | O(n²) | Find repeated sequences |
| Index of Coincidence | O(n) | Statistical analysis |

### Space Complexity

| Operation | Space | Notes |
|-----------|-------|-------|
| Encryption | O(n) | Output string |
| Decryption | O(n) | Output string |
| Keyword Storage | O(m) | m = keyword length |

### Key Space Analysis

| Keyword Length | Key Space | Brute Force Time |
|----------------|-----------|------------------|
| 1 | 26 | Instant |
| 2 | 676 | Instant |
| 3 | 17,576 | Seconds |
| 4 | 456,976 | Seconds |
| 5 | 11,881,376 | Minutes |
| 10 | 1.4 × 10¹⁴ | Years |
| n (One-Time Pad) | 26^n | Unbreakable |

---

## Algorithm Pseudocode

### Encryption

```
function VIGENERE_ENCRYPT(plaintext, keyword):
    result ← empty string
    keyword ← keyword.toUpperCase()
    keyLength ← length(keyword)
    keyIndex ← 0
    
    for each character c in plaintext:
        if c is alphabetic:
            // Get shift from current keyword character
            shift ← keyword[keyIndex mod keyLength] - 'A'
            
            if c is uppercase:
                encrypted ← ((c - 'A' + shift) mod 26) + 'A'
            else:  // lowercase
                encrypted ← ((c - 'a' + shift) mod 26) + 'a'
            
            result ← result + encrypted
            keyIndex ← keyIndex + 1  // Only advance for letters
        else:
            result ← result + c  // Preserve non-letters
    
    return result
```

### Decryption

```
function VIGENERE_DECRYPT(ciphertext, keyword):
    result ← empty string
    keyword ← keyword.toUpperCase()
    keyLength ← length(keyword)
    keyIndex ← 0
    
    for each character c in ciphertext:
        if c is alphabetic:
            shift ← keyword[keyIndex mod keyLength] - 'A'
            
            if c is uppercase:
                decrypted ← ((c - 'A' - shift + 26) mod 26) + 'A'
            else:
                decrypted ← ((c - 'a' - shift + 26) mod 26) + 'a'
            
            result ← result + decrypted
            keyIndex ← keyIndex + 1
        else:
            result ← result + c
    
    return result
```

### Kasiski Examination (Breaking the Cipher)

```
function KASISKI_EXAMINATION(ciphertext):
    // Step 1: Find repeated sequences (3+ characters)
    sequences ← find all repeated sequences in ciphertext
    
    // Step 2: Calculate distances between repetitions
    distances ← []
    for each sequence in sequences:
        positions ← all occurrences of sequence
        for i from 0 to length(positions) - 2:
            distances.append(positions[i+1] - positions[i])
    
    // Step 3: Find GCD of distances (likely key length)
    keyLength ← GCD of all distances
    
    // Step 4: Split ciphertext into keyLength streams
    streams ← split ciphertext by position mod keyLength
    
    // Step 5: Apply frequency analysis to each stream
    keyword ← ""
    for each stream in streams:
        shift ← frequency_analysis(stream)  // Like Caesar attack
        keyword ← keyword + chr(shift + 'A')
    
    return keyword
```

---

## Implementation Notes

### From Vigenere.java

**Encryption:**

```java
public static String encrypt(final String message, final String key) {
    StringBuilder result = new StringBuilder();
    int j = 0;  // Key index
    
    for (int i = 0; i < message.length(); i++) {
        char c = message.charAt(i);
        
        if (Character.isLetter(c)) {
            if (Character.isUpperCase(c)) {
                result.append((char) ((c + key.toUpperCase().charAt(j) - 2 * 'A') % 26 + 'A'));
            } else {
                result.append((char) ((c + key.toLowerCase().charAt(j) - 2 * 'a') % 26 + 'a'));
            }
            j = ++j % key.length();  // Advance key index cyclically
        } else {
            result.append(c);  // Non-letters preserved
        }
    }
    return result.toString();
}
```

**Decryption:**

```java
public static String decrypt(final String message, final String key) {
    StringBuilder result = new StringBuilder();
    int j = 0;
    
    for (int i = 0; i < message.length(); i++) {
        char c = message.charAt(i);
        
        if (Character.isLetter(c)) {
            if (Character.isUpperCase(c)) {
                result.append((char) ('Z' - (25 - (c - key.toUpperCase().charAt(j))) % 26));
            } else {
                result.append((char) ('z' - (25 - (c - key.toLowerCase().charAt(j))) % 26));
            }
            j = ++j % key.length();
        } else {
            result.append(c);
        }
    }
    return result.toString();
}
```

### Key Observations

1. **Key Index Advancement**: Only advances for alphabetic characters
2. **Case Preservation**: Uppercase and lowercase handled separately
3. **Non-Alphabetic Preservation**: Spaces, punctuation unchanged
4. **Cyclic Key Application**: Key wraps around using modulo

---

## Real-World Applications

### Historical Usage

| Period | Users | Purpose |
|--------|-------|---------|
| 16th-19th Century | Diplomats | Secret correspondence |
| American Civil War | Confederacy | Military communications |
| WWI | Various | Field encryption |

### Modern Usage

| Application | Context |
|-------------|---------|
| CTF Challenges | Cryptographic puzzles |
| Education | Teaching polyalphabetic concepts |
| Puzzles | Crossword-style encryption games |

### Relationship to Modern Cryptography

The Vigenère cipher's weaknesses led to important developments:

1. **Key Length Discovery** → Periodic key vulnerability
2. **Frequency Analysis Adaptation** → Need for diffusion
3. **One-Time Pad** → Perfect secrecy with random, non-repeating key
4. **Stream Ciphers** → Modern approach to key stream generation

---

## Comparison with Related Ciphers

### Polyalphabetic Cipher Family

| Cipher | Key Type | Period | Security |
|--------|----------|--------|----------|
| Vigenère | Keyword | Key length | Broken |
| Beaufort | Keyword | Key length | Broken |
| Autokey | Message-based | Varies | Slightly stronger |
| Running Key | Text passage | Long | Broken |
| One-Time Pad | Random, non-repeating | ∞ | Perfect |

### Security Comparison

| Cipher | Key Space | Vulnerability |
|--------|-----------|---------------|
| Caesar | 26 | Brute force |
| Affine | 312 | Brute force |
| Simple Substitution | 26! | Frequency analysis |
| Vigenère | 26^k | Kasiski, Index of Coincidence |
| One-Time Pad | 26^n | None (theoretically) |

### Evolution Path

```
Caesar → Vigenère → Autokey → One-Time Pad → Stream Ciphers (RC4, ChaCha)
  ↓         ↓           ↓          ↓              ↓
 Simple  Periodic    Message    Random        CSPRNG-based
 shift    key       feedback    key stream    key stream
```

---

## Breaking the Vigenère Cipher

### Method 1: Kasiski Examination

Find repeated sequences to determine key length:

```
Ciphertext: VVHQWVVRHMUSGJGTHKIHTSSEJCHLSFCBGVWCRLRYQTFSVGAHWK
CUHWAUGLQHNSLRLJSHBLTSPISPRDXLJSVEEGHLQWKASSTEHPLCTSTK

Repeated sequence: "VVH" at positions 0 and 18
Distance: 18 = 2 × 3 × 3
Likely key length: 2, 3, 6, 9, or 18
```

### Method 2: Index of Coincidence

The Index of Coincidence (IC) measures the probability that two randomly chosen letters from a text are the same:

$$IC = \frac{\sum_{i=0}^{25} n_i(n_i - 1)}{N(N-1)}$$

| Language | Expected IC |
|----------|-------------|
| English | 0.0667 |
| Random | 0.0385 |

**Finding Key Length:**
1. Split ciphertext into k groups (testing different k values)
2. Calculate IC for each group
3. Correct k gives IC ≈ 0.0667 for each group

### Method 3: Frequency Analysis per Position

Once key length k is known:
1. Group characters by position mod k
2. Each group is a simple Caesar cipher
3. Apply frequency analysis to each group
4. Combine results to get keyword

```java
public static String breakVigenere(String ciphertext, int keyLength) {
    StringBuilder key = new StringBuilder();
    
    for (int i = 0; i < keyLength; i++) {
        // Extract every keyLength-th character starting at i
        StringBuilder stream = new StringBuilder();
        for (int j = i; j < ciphertext.length(); j += keyLength) {
            if (Character.isLetter(ciphertext.charAt(j))) {
                stream.append(ciphertext.charAt(j));
            }
        }
        
        // Frequency analysis on this stream (like breaking Caesar)
        int shift = findMostLikelyShift(stream.toString());
        key.append((char) ('A' + shift));
    }
    
    return key.toString();
}
```

---

## Common Pitfalls & Edge Cases

### Implementation Issues

1. **Key Advancement for Non-Letters**
   ```java
   // WRONG: Advancing key for spaces
   j = ++j % key.length();  // Always advances
   
   // RIGHT: Only advance for letters
   if (Character.isLetter(c)) {
       // encrypt...
       j = ++j % key.length();
   }
   ```

2. **Empty Key Handling**
   ```java
   // WRONG: No validation
   public static String encrypt(String msg, String key) {
       // Division by zero if key.length() == 0
   }
   
   // RIGHT: Validate input
   if (key == null || key.isEmpty()) {
       throw new IllegalArgumentException("Key cannot be empty");
   }
   ```

3. **Case Sensitivity**
   ```java
   // Ensure consistent key case handling
   key = key.toUpperCase();  // or toLowerCase()
   ```

### Security Pitfalls

| Issue | Consequence | Mitigation |
|-------|-------------|------------|
| Short key | Easy to break | Use long, random keys |
| Dictionary word key | Vulnerable to attack | Random character sequence |
| Key reuse | Pattern analysis | Unique keys per message |
| Predictable key | Dictionary attack | High-entropy generation |

### Edge Cases

```java
// Empty message
encrypt("", "KEY") → ""

// Message shorter than key
encrypt("HI", "LONGKEY") → uses only "LO"

// Non-alphabetic message
encrypt("123!@#", "KEY") → "123!@#" (unchanged)

// Key with non-letters (implementation dependent)
encrypt("HELLO", "KEY 123") → may fail or skip non-letters
```

---

## One-Time Pad: Perfect Vigenère

When the Vigenère key is:
1. **Truly random** (not based on words or patterns)
2. **At least as long as the message**
3. **Never reused**

The cipher becomes a **One-Time Pad** with perfect secrecy:

$$P(\text{plaintext} | \text{ciphertext}) = P(\text{plaintext})$$

The ciphertext reveals no information about the plaintext.

**Why it's impractical:**
- Key distribution problem
- Key must be as long as all messages combined
- Key synchronization issues
- No key reuse ever

---

## Educational Significance

### Concepts Demonstrated

1. **Polyalphabetic Substitution**: Breaking monoalphabetic weakness
2. **Key Period**: Regular pattern in encryption
3. **Kasiski Examination**: Pattern-based cryptanalysis
4. **Index of Coincidence**: Statistical cryptanalysis
5. **Perfect Secrecy**: One-Time Pad as limit case

### Progression to Modern Cryptography

```
Vigenère (1553)
    ↓
"Unbreakable" for 300 years
    ↓
Kasiski (1863) / Babbage (~1854)
    ↓
Need for: Non-repeating keys, Complex transformations
    ↓
One-Time Pad (1917) → Perfect but impractical
    ↓
Enigma (1920s) → Rotor machines
    ↓
DES (1977) → Block ciphers
    ↓
AES (2001) → Modern standard
```

---

## References

### Historical Sources

1. Kasiski, F. W. (1863). *Die Geheimschriften und die Dechiffrir-kunst*
2. Singh, S. (1999). *The Code Book*
3. Friedman, W. (1920). *The Index of Coincidence*

### Online Resources

- [Wikipedia: Vigenère Cipher](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher)
- [Crypto Corner: Vigenère](https://crypto.interactive-maths.com/vigenegravere-cipher.html)

### Related Algorithms

- [Caesar Cipher](caesar.md) - Single-shift predecessor
- [AES](../symmetric/aes.md) - Modern symmetric encryption
- [DES](../symmetric/des.md) - Historical block cipher
- [RSA](../asymmetric/rsa.md) - Asymmetric encryption
