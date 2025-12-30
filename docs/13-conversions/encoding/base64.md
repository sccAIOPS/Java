# Base64 Encoding/Decoding

> **Category:** Conversions  
> **Subcategory:** Encoding  
> **Implementation:** [`Base64.java`](../../src/main/java/com/thealgorithms/conversions/Base64.java)

---

## 📚 Overview

Base64 is a binary-to-text encoding scheme that represents binary data in an ASCII string format. It converts every 3 bytes (24 bits) of binary data into 4 printable ASCII characters, using a 64-character alphabet.

**Key Characteristics:**
- Encodes binary data as printable ASCII
- 33% size increase (4 output chars per 3 input bytes)
- URL-safe and email-safe data transmission
- Widely used in web applications, APIs, and email

---

## 🔢 Mathematical Foundation

### The Base64 Alphabet

```
Index: 0-25   → A-Z
Index: 26-51  → a-z
Index: 52-61  → 0-9
Index: 62     → +
Index: 63     → /
Padding       → =
```

### Encoding Process

1. Take 3 bytes (24 bits) of input
2. Split into 4 groups of 6 bits each
3. Map each 6-bit value to Base64 alphabet
4. Pad with '=' if input length not divisible by 3

### Size Formula

$$
\text{Encoded Length} = 4 \times \left\lceil \frac{\text{Input Length}}{3} \right\rceil
$$

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Encode | O(n) | O(n) |
| Decode | O(n) | O(n) |

---

## 🔄 Algorithm (Pseudocode)

### Encoding
```
ALGORITHM Base64Encode(bytes)
─────────────────────────────────────────────────────
    INPUT:  bytes - array of bytes
    OUTPUT: base64 encoded string
─────────────────────────────────────────────────────

    result ← ""
    FOR i ← 0 TO length(bytes) - 1 STEP 3 DO
        // Combine 3 bytes into 24-bit number
        chunk ← (bytes[i] << 16) | (bytes[i+1] << 8) | bytes[i+2]
        
        // Extract 4 groups of 6 bits
        FOR j ← 0 TO 3 DO
            index ← (chunk >> (18 - j*6)) AND 0x3F
            result ← result + alphabet[index]
        END FOR
    END FOR
    
    // Add padding if needed
    ADD '=' for each missing byte
    
    RETURN result
```

### Step-by-Step Walkthrough

**Encode "Man":**

| Step | Data | Binary |
|------|------|--------|
| Input | M a n | 01001101 01100001 01101110 |
| 6-bit groups | | 010011 010110 000101 101110 |
| Decimal | | 19, 22, 5, 46 |
| Base64 | | T, W, F, u |

**Result: "TWFu"**

---

## 💻 Implementation Notes

### Java Implementation

```java
private static final String ALPHABET = 
    "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";

public static String encode(byte[] data) {
    StringBuilder result = new StringBuilder();
    
    for (int i = 0; i < data.length; i += 3) {
        int b1 = data[i] & 0xFF;
        int b2 = (i + 1 < data.length) ? data[i + 1] & 0xFF : 0;
        int b3 = (i + 2 < data.length) ? data[i + 2] & 0xFF : 0;
        
        int combined = (b1 << 16) | (b2 << 8) | b3;
        
        result.append(ALPHABET.charAt((combined >> 18) & 0x3F));
        result.append(ALPHABET.charAt((combined >> 12) & 0x3F));
        result.append(i + 1 < data.length ? 
            ALPHABET.charAt((combined >> 6) & 0x3F) : '=');
        result.append(i + 2 < data.length ? 
            ALPHABET.charAt(combined & 0x3F) : '=');
    }
    
    return result.toString();
}

// Using Java built-in
public static String encodeBuiltin(byte[] data) {
    return java.util.Base64.getEncoder().encodeToString(data);
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/conversions/Base64.java`](../../src/main/java/com/thealgorithms/conversions/Base64.java)

---

## 🌍 Real-World Applications

### 1. Web APIs
**Use Case:** Transmitting binary data (images, files) in JSON

### 2. Email (MIME)
**Use Case:** Encoding attachments for transmission

### 3. Data URLs
**Use Case:** Embedding images directly in HTML/CSS

### 4. Authentication
**Use Case:** Basic HTTP authentication headers

### Industry Examples

| Application | Use Case |
|-------------|----------|
| JWT Tokens | Token payload encoding |
| REST APIs | Binary data in JSON |
| Email Attachments | MIME encoding |
| HTML Images | Data URIs |

---

## ⚖️ Base64 Variants

| Variant | Char 62 | Char 63 | Padding | Use Case |
|---------|---------|---------|---------|----------|
| Standard | + | / | = | General |
| URL-safe | - | _ | Optional | URLs, filenames |
| MIME | + | / | = | Email |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Size increase | 33% larger | Consider compression first |
| Not encryption | Just encoding | Use encryption for security |
| Line breaks | MIME adds breaks | Strip for compact format |

---

## 📖 References

1. **RFC 4648** - The Base16, Base32, and Base64 Data Encodings
2. **RFC 2045** - MIME (Multipurpose Internet Mail Extensions)

---

## 🔗 Related Algorithms

- [URL Encoding](./url-encoding.md)
- [Hexadecimal Encoding](../number-base/hex-encoding.md)

---

*Last updated: December 30, 2025*
