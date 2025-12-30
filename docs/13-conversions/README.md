# Conversion Algorithms

> **Category:** Utility Algorithms  
> **Difficulty:** Beginner to Intermediate  
> **Prerequisites:** Number systems, data representation

---

## 📚 Overview

Conversion algorithms transform data between different formats, number systems, encodings, and representations. These algorithms are fundamental to software systems that need to process, store, or transmit data in various formats.

**Why Learn Conversion Algorithms?**
- **Interoperability:** Convert data between different systems and formats
- **Data Processing:** Transform input data into usable formats
- **Communication:** Format data for different protocols and interfaces
- **Internationalization:** Handle different character sets and formats

---

## 🗂️ Algorithms in This Category

### Number Base Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Binary to Decimal** | Convert binary to base-10 | [binary-to-decimal.md](./number-base/binary-to-decimal.md) |
| **Decimal to Binary** | Convert base-10 to binary | [decimal-to-binary.md](./number-base/decimal-to-binary.md) |
| **Decimal to Octal** | Convert base-10 to base-8 | [decimal-to-octal.md](./number-base/decimal-to-octal.md) |
| **Octal to Decimal** | Convert base-8 to base-10 | [octal-to-decimal.md](./number-base/octal-to-decimal.md) |
| **Decimal to Hexadecimal** | Convert base-10 to base-16 | [decimal-to-hex.md](./number-base/decimal-to-hex.md) |
| **Hexadecimal to Decimal** | Convert base-16 to base-10 | [hex-to-decimal.md](./number-base/hex-to-decimal.md) |
| **Hexadecimal to Binary** | Convert base-16 to base-2 | [hex-to-binary.md](./number-base/hex-to-binary.md) |
| **Binary to Hexadecimal** | Convert base-2 to base-16 | [binary-to-hex.md](./number-base/binary-to-hex.md) |
| **Octal to Binary** | Convert base-8 to base-2 | [octal-to-binary.md](./number-base/octal-to-binary.md) |
| **Binary to Octal** | Convert base-2 to base-8 | [binary-to-octal.md](./number-base/binary-to-octal.md) |
| **Any Base to Any Base** | Generic base conversion | [any-base-conversion.md](./number-base/any-base-conversion.md) |
| **Any Base to Decimal** | Convert any base to base-10 | [any-base-to-decimal.md](./number-base/any-base-to-decimal.md) |
| **Decimal to Any Base** | Convert base-10 to any base | [decimal-to-any-base.md](./number-base/decimal-to-any-base.md) |

### Roman Numeral Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Integer to Roman** | Convert integers to Roman numerals | [integer-to-roman.md](./roman/integer-to-roman.md) |
| **Roman to Integer** | Convert Roman numerals to integers | [roman-to-integer.md](./roman/roman-to-integer.md) |

### Text/Number Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Number to Words** | Convert numbers to English words | [number-to-words.md](./text/number-to-words.md) |
| **Integer to English** | Spell out integers in English | [integer-to-english.md](./text/integer-to-english.md) |
| **Words to Number** | Parse written numbers to integers | [words-to-number.md](./text/words-to-number.md) |

### Encoding Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Base64** | Base64 encoding and decoding | [base64.md](./encoding/base64.md) |
| **Morse Code** | Convert to/from Morse code | [morse-code.md](./encoding/morse-code.md) |
| **Phonetic Alphabet** | NATO phonetic alphabet conversion | [phonetic-alphabet.md](./encoding/phonetic-alphabet.md) |

### Network Address Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **IP Converter** | IPv4 address conversions | [ip-converter.md](./network/ip-converter.md) |
| **IPv6 Converter** | IPv6 address format conversions | [ipv6-converter.md](./network/ipv6-converter.md) |

### Unit Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Temperature Converter** | Celsius, Fahrenheit, Kelvin | [temperature.md](./units/temperature.md) |
| **Time Converter** | Time zone and format conversions | [time-converter.md](./units/time-converter.md) |
| **Unit Conversions** | Generic unit conversion framework | [unit-conversions.md](./units/unit-conversions.md) |
| **Affine Converter** | Linear transformation conversions | [affine-converter.md](./units/affine-converter.md) |

### Color Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **RGB to HSV** | Color space conversion | [rgb-hsv.md](./color/rgb-hsv.md) |

### Coordinate Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Coordinate Converter** | Geographic coordinate systems | [coordinate-converter.md](./coordinate-converter.md) |

### Data Format Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Endian Converter** | Big-endian ↔ Little-endian | [endian-converter.md](./data-format/endian-converter.md) |

### Character Set Conversions

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Turkish to Latin** | Turkish character normalization | [turkish-to-latin.md](./charset/turkish-to-latin.md) |

---

## 🔢 Number Base Systems Reference

| Base | Name | Digits | Common Use |
|------|------|--------|------------|
| 2 | Binary | 0-1 | Computer hardware, digital logic |
| 8 | Octal | 0-7 | Unix file permissions, legacy systems |
| 10 | Decimal | 0-9 | Human-readable numbers |
| 16 | Hexadecimal | 0-9, A-F | Memory addresses, colors, MAC addresses |
| 64 | Base64 | A-Z, a-z, 0-9, +, / | Data encoding for transmission |

---

## 📊 Complexity Analysis

### Number Base Conversions

| Conversion Type | Time Complexity | Space Complexity |
|-----------------|-----------------|------------------|
| Any to Decimal | O(n) | O(1) |
| Decimal to Any | O(log n) | O(log n) |
| Direct (Binary↔Hex) | O(n) | O(n) |

Where `n` is the number of digits in the source representation.

### Text Conversions

| Conversion Type | Time Complexity | Space Complexity |
|-----------------|-----------------|------------------|
| Number to Words | O(log n) | O(log n) |
| Roman to Integer | O(n) | O(1) |
| Integer to Roman | O(1) | O(1) |

---

## 🌍 Real-World Applications

### 1. Web Development
- URL encoding/decoding
- Base64 image embedding
- Color code conversions (HEX, RGB, HSL)
- Character set normalization

### 2. Systems Programming
- Memory address display
- Binary data inspection
- Endianness handling
- File permission representation

### 3. Network Programming
- IP address parsing and formatting
- MAC address conversions
- Protocol data encoding

### 4. Financial Software
- Currency display
- Number to words for checks
- Unit conversions

### 5. Internationalization
- Character set conversions
- Number formatting per locale
- Date/time format conversions

### 6. Scientific Computing
- Unit conversions
- Coordinate system transformations
- Data format standardization

---

## 💡 Common Patterns

### Base Conversion Algorithm (Division Method)

```java
// Decimal to any base
public String decimalToBase(int decimal, int base) {
    StringBuilder result = new StringBuilder();
    while (decimal > 0) {
        int remainder = decimal % base;
        result.insert(0, getDigitChar(remainder));
        decimal /= base;
    }
    return result.length() > 0 ? result.toString() : "0";
}
```

### Base Conversion Algorithm (Positional Method)

```java
// Any base to decimal
public int baseToDecimal(String number, int base) {
    int result = 0;
    int power = 1;
    for (int i = number.length() - 1; i >= 0; i--) {
        result += getDigitValue(number.charAt(i)) * power;
        power *= base;
    }
    return result;
}
```

---

## 📖 Recommended Learning Path

```
1. Binary ↔ Decimal → 2. Octal/Hex Conversions → 3. Any Base Conversion
         ↓                       ↓                         ↓
4. Roman Numerals → 5. Text Conversions → 6. Encoding (Base64, Morse)
         ↓                    ↓                         ↓
7. Network Addresses → 8. Unit Conversions → 9. Coordinate Systems
```

---

## 🔧 Implementation Tips

1. **Validate Input:** Always validate input before conversion
2. **Handle Edge Cases:** Zero, negative numbers, empty strings
3. **Consider Overflow:** Large numbers may exceed integer limits
4. **Use StringBuilder:** Efficient for building converted strings
5. **Lookup Tables:** Pre-computed tables for faster character conversions

---

## 📚 References

1. **"The Art of Computer Programming"** Vol. 2 - Donald Knuth (Seminumerical Algorithms)
2. **IEEE 754** - Floating-point number representation
3. **RFC 4648** - Base64 encoding standard
4. **Unicode Standard** - Character encoding

---

## 🔗 Related Categories

- [Bit Manipulation](../12-bit-manipulation/README.md) - Binary operations
- [Mathematical Algorithms](../08-mathematical-algorithms/README.md) - Number theory
- [String Algorithms](../09-string-algorithms/README.md) - Text processing

---

*Last updated: December 30, 2025*
