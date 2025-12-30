# Single Bit Operations

> **Category:** Bit Manipulation  
> **Subcategory:** Fundamental Operations  
> **Implementation:** [`SingleBitOperations.java`](../../src/main/java/com/thealgorithms/bitmanipulation/SingleBitOperations.java)

---

## 📚 Overview

Single bit operations are the building blocks of bit manipulation. These operations allow you to get, set, clear, and toggle individual bits at specific positions within an integer. Mastering these operations is essential for low-level programming, embedded systems, and optimization.

**Key Characteristics:**
- All operations are O(1) constant time
- Use bit masking with shift operations
- Foundation for all other bit manipulation techniques

---

## 🔢 Mathematical Foundation

### Bit Position Convention

Bits are numbered from right to left, starting at 0 (least significant bit):

```
Bit Position:  7  6  5  4  3  2  1  0
Binary Value: [0][0][1][0][1][0][1][0] = 42
```

### The Four Basic Operations

| Operation | Formula | Description |
|-----------|---------|-------------|
| **Get** | `(n >> i) & 1` | Extract bit i |
| **Set** | `n \| (1 << i)` | Set bit i to 1 |
| **Clear** | `n & ~(1 << i)` | Set bit i to 0 |
| **Toggle** | `n ^ (1 << i)` | Flip bit i |

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Get bit | O(1) | O(1) |
| Set bit | O(1) | O(1) |
| Clear bit | O(1) | O(1) |
| Toggle bit | O(1) | O(1) |

---

## 🔄 Algorithm (Pseudocode)

### Get Bit
```
ALGORITHM GetBit(n, position)
─────────────────────────────────────────────────────
    RETURN (n >> position) AND 1
```

### Set Bit
```
ALGORITHM SetBit(n, position)
─────────────────────────────────────────────────────
    mask ← 1 << position
    RETURN n OR mask
```

### Clear Bit
```
ALGORITHM ClearBit(n, position)
─────────────────────────────────────────────────────
    mask ← NOT (1 << position)
    RETURN n AND mask
```

### Toggle Bit
```
ALGORITHM ToggleBit(n, position)
─────────────────────────────────────────────────────
    mask ← 1 << position
    RETURN n XOR mask
```

### Step-by-Step Walkthrough

**Example: n = 42 (binary: 00101010), position = 3**

| Operation | Initial | Mask | Result |
|-----------|---------|------|--------|
| Get bit 3 | 00101010 | - | 1 |
| Set bit 3 | 00101010 | 00001000 | 00101010 (unchanged) |
| Clear bit 3 | 00101010 | 11110111 | 00100010 = 34 |
| Toggle bit 3 | 00101010 | 00001000 | 00100010 = 34 |

---

## 💻 Implementation Notes

### Java Implementation

```java
public class SingleBitOperations {
    
    /**
     * Get the bit at position (0-indexed from right)
     */
    public static int getBit(int n, int position) {
        return (n >> position) & 1;
    }
    
    /**
     * Set the bit at position to 1
     */
    public static int setBit(int n, int position) {
        return n | (1 << position);
    }
    
    /**
     * Clear the bit at position (set to 0)
     */
    public static int clearBit(int n, int position) {
        return n & ~(1 << position);
    }
    
    /**
     * Toggle (flip) the bit at position
     */
    public static int toggleBit(int n, int position) {
        return n ^ (1 << position);
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/bitmanipulation/SingleBitOperations.java`](../../src/main/java/com/thealgorithms/bitmanipulation/SingleBitOperations.java)

📁 **Test File:** [`src/test/java/com/thealgorithms/bitmanipulation/SingleBitOperationsTest.java`](../../src/test/java/com/thealgorithms/bitmanipulation/SingleBitOperationsTest.java)

---

## 🌍 Real-World Applications

### 1. Permission Flags
```java
int READ = 0, WRITE = 1, EXECUTE = 2;
int permissions = 0;
permissions = setBit(permissions, READ);    // Add read permission
permissions = setBit(permissions, WRITE);   // Add write permission
boolean canRead = getBit(permissions, READ) == 1;
```

### 2. Feature Toggles
```java
int features = 0;
int DARK_MODE = 0, NOTIFICATIONS = 1, ANALYTICS = 2;
features = toggleBit(features, DARK_MODE);  // Enable dark mode
```

### 3. Hardware Register Manipulation
```java
// Set pin 5 high on a GPIO register
registerValue = setBit(registerValue, 5);
```

### Industry Examples

| Application | Use Case |
|-------------|----------|
| Unix Permissions | rwx flags (chmod) |
| Network Protocols | TCP flags (SYN, ACK, FIN) |
| Game Development | Entity component flags |
| Embedded Systems | GPIO pin control |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Position overflow | Position ≥ 32 | Validate input |
| Negative position | Undefined behavior | Check position ≥ 0 |
| Signed shift | Sign extension | Use >>> for unsigned |

---

## 📖 References

1. **"Hacker's Delight"** - Henry S. Warren Jr.
2. **"C Programming Language"** - Kernighan & Ritchie

---

## 🔗 Related Algorithms

- [Highest Set Bit](./highest-set-bit.md)
- [Lowest Set Bit](./lowest-set-bit.md)
- [Clear Leftmost Set Bit](./clear-leftmost-set-bit.md)

---

*Last updated: December 30, 2025*
