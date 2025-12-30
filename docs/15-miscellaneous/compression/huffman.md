# Huffman Coding

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Data Compression  
> **Implementation:** [`Huffman.java`](../../src/main/java/com/thealgorithms/others/Huffman.java)

---

## 📚 Overview

Huffman coding is a lossless data compression algorithm that creates variable-length prefix codes for characters based on their frequencies. More frequent characters get shorter codes, resulting in optimal prefix-free encoding.

**Key Characteristics:**
- Prefix-free codes (no code is prefix of another)
- Optimal for character-level encoding
- Greedy algorithm approach
- Creates binary tree structure

---

## 🔢 Mathematical Foundation

### Prefix Codes

A code is prefix-free if no codeword is a prefix of another:
- ✅ Valid: {0, 10, 11}
- ❌ Invalid: {0, 01, 11} (0 is prefix of 01)

### Expected Code Length

For characters with frequencies $f_1, f_2, ..., f_n$ and code lengths $l_1, l_2, ..., l_n$:

$$
L = \sum_{i=1}^{n} f_i \times l_i
$$

Huffman minimizes this expected length.

### Compression Ratio

$$
\text{Compression Ratio} = 1 - \frac{\text{Compressed Size}}{\text{Original Size}}
$$

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Build tree | O(n log n) | O(n) |
| Encode | O(m) where m = input length | O(n) |
| Decode | O(m) | O(n) |

---

## 🔄 Algorithm (Pseudocode)

### Building Huffman Tree
```
ALGORITHM BuildHuffmanTree(frequencies)
─────────────────────────────────────────────────────
    INPUT:  frequencies - character frequency map
    OUTPUT: root of Huffman tree
─────────────────────────────────────────────────────

    // Create leaf node for each character
    priorityQueue ← MinHeap()
    FOR each (char, freq) IN frequencies DO
        node ← new Node(char, freq)
        priorityQueue.insert(node)
    END FOR
    
    // Build tree by combining smallest frequencies
    WHILE priorityQueue.size() > 1 DO
        left ← priorityQueue.extractMin()
        right ← priorityQueue.extractMin()
        
        parent ← new Node(null, left.freq + right.freq)
        parent.left ← left
        parent.right ← right
        
        priorityQueue.insert(parent)
    END WHILE
    
    RETURN priorityQueue.extractMin()  // Root
```

### Generating Codes
```
ALGORITHM GenerateCodes(root, prefix, codeMap)
─────────────────────────────────────────────────────
    IF root is leaf THEN
        codeMap[root.char] ← prefix
    ELSE
        GenerateCodes(root.left, prefix + "0", codeMap)
        GenerateCodes(root.right, prefix + "1", codeMap)
    END IF
```

### Step-by-Step Example

**Input:** "AAAAABBBCCDD" (A:5, B:3, C:2, D:2)

**Step 1:** Create nodes
```
[A:5] [B:3] [C:2] [D:2]
```

**Step 2:** Combine smallest (C, D)
```
[A:5] [B:3] [CD:4]
              / \
            C:2 D:2
```

**Step 3:** Combine smallest (B, CD)
```
[A:5] [BCD:7]
        / \
      B:3 [CD:4]
            / \
          C:2 D:2
```

**Step 4:** Combine last two
```
     [Root:12]
      /      \
    A:5    [BCD:7]
            /   \
          B:3  [CD:4]
                / \
              C:2 D:2
```

**Codes:**
| Char | Code |
|------|------|
| A | 0 |
| B | 10 |
| C | 110 |
| D | 111 |

**Compression:**
- Original: 12 × 8 = 96 bits (ASCII)
- Huffman: 5×1 + 3×2 + 2×3 + 2×3 = 23 bits
- Ratio: 76% compression

---

## 💻 Implementation Notes

### Java Implementation

```java
public class Huffman {
    
    private static class Node implements Comparable<Node> {
        char ch;
        int freq;
        Node left, right;
        
        Node(char ch, int freq) {
            this.ch = ch;
            this.freq = freq;
        }
        
        boolean isLeaf() {
            return left == null && right == null;
        }
        
        @Override
        public int compareTo(Node other) {
            return this.freq - other.freq;
        }
    }
    
    public static Map<Character, String> buildCodes(String text) {
        // Count frequencies
        Map<Character, Integer> freq = new HashMap<>();
        for (char c : text.toCharArray()) {
            freq.merge(c, 1, Integer::sum);
        }
        
        // Build priority queue
        PriorityQueue<Node> pq = new PriorityQueue<>();
        for (Map.Entry<Character, Integer> e : freq.entrySet()) {
            pq.offer(new Node(e.getKey(), e.getValue()));
        }
        
        // Build tree
        while (pq.size() > 1) {
            Node left = pq.poll();
            Node right = pq.poll();
            
            Node parent = new Node('\0', left.freq + right.freq);
            parent.left = left;
            parent.right = right;
            
            pq.offer(parent);
        }
        
        // Generate codes
        Map<Character, String> codes = new HashMap<>();
        generateCodes(pq.poll(), "", codes);
        
        return codes;
    }
    
    private static void generateCodes(Node node, String code, 
                                       Map<Character, String> codes) {
        if (node.isLeaf()) {
            codes.put(node.ch, code.isEmpty() ? "0" : code);
            return;
        }
        generateCodes(node.left, code + "0", codes);
        generateCodes(node.right, code + "1", codes);
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/Huffman.java`](../../src/main/java/com/thealgorithms/others/Huffman.java)

---

## 🌍 Real-World Applications

### 1. File Compression
**Use Case:** ZIP, GZIP, DEFLATE

### 2. Media Compression
**Use Case:** JPEG (DC coefficients), MP3

### 3. Data Transmission
**Use Case:** FAX machines (modified Huffman)

### 4. Text Compression
**Use Case:** Book/document compression

### Industry Usage

| Format | Huffman Usage |
|--------|---------------|
| ZIP/GZIP | Combined with LZ77 |
| JPEG | Final stage for quantized values |
| PNG | Part of DEFLATE |
| MP3 | Audio data compression |

---

## ⚖️ Huffman vs Other Compression

| Method | Type | Compression | Speed |
|--------|------|-------------|-------|
| Huffman | Lossless | Good | Fast |
| LZW | Lossless | Better | Fast |
| Arithmetic | Lossless | Best | Slower |
| LZ77/78 | Lossless | Good | Fast |

---

## 🔄 Adaptive Huffman

Static Huffman requires two passes:
1. Calculate frequencies
2. Encode data

**Adaptive Huffman** updates tree as data is processed:
- Single pass
- No need to transmit tree
- Slightly less optimal

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Single character | Empty code | Use "0" |
| Tree transmission | Overhead | Use canonical Huffman |
| Small data | Overhead > savings | Minimum size threshold |
| Block-based | Local patterns missed | Adaptive or block reset |

---

## 📖 References

1. **"A Method for the Construction of Minimum-Redundancy Codes"** - Huffman (1952)
2. **"Introduction to Data Compression"** - Sayood
3. **RFC 1951** - DEFLATE Compressed Data Format

---

## 🔗 Related Algorithms

- [Shannon-Fano Coding](../../compression/shannon-fano.md)
- [Arithmetic Coding](../../compression/arithmetic-coding.md)
- [LZW Compression](../../compression/lzw.md)

---

*Last updated: December 30, 2025*
