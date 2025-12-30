# Trie (Prefix Tree)

> **Category:** Data Structures  
> **Subcategory:** Trees  
> **Implementation:** [Trie.java](../../../src/main/java/com/thealgorithms/datastructures/trees/Trie.java)

---

## 📚 Overview

A **Trie** (pronounced "try", from retrieval) is a tree-like data structure used to efficiently store and retrieve strings. Also known as a **prefix tree** or **digital tree**, it provides $O(m)$ time complexity for search, insert, and delete operations, where $m$ is the length of the string.

Unlike binary search trees that compare entire keys, tries examine one character at a time, making them particularly efficient for prefix-based operations like autocomplete and spell checking.

---

## 🔢 Mathematical Foundation

### Definition

A Trie is a rooted tree where:
- Each node represents a character (or end of string)
- The root represents the empty string
- Each path from root to a marked node represents a stored string
- Children of a node share a common prefix

### Key Properties

- **No Key Storage:** Keys are implicitly stored in paths
- **Prefix Sharing:** Common prefixes are stored only once
- **Alphabet Size:** Each node has up to $|\Sigma|$ children ($\Sigma$ = alphabet)
- **Space-Time Tradeoff:** Uses more memory for faster operations

### Mathematical Formulation

For a set of strings $S$ with total length $N$ and alphabet size $|\Sigma|$:

**Height:** 
$$h = \max_{s \in S} |s|$$

**Node Count:** 
$$O(\min(N, |\Sigma|^h))$$

**Space Complexity:**
$$O(N \cdot |\Sigma|)$$ (array-based) or $O(N)$ (hash-based)

---

## 📊 Complexity Analysis

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| **Insert** | $O(m)$ | $m$ = word length |
| **Search** | $O(m)$ | $m$ = word length |
| **Delete** | $O(m)$ | $m$ = word length |
| **Prefix Search** | $O(p + k)$ | $p$ = prefix length, $k$ = results |
| **Space** | $O(N \cdot |\Sigma|)$ | $N$ = total chars, $|\Sigma|$ = alphabet |

### Comparison with Other String Structures

| Structure | Insert | Search | Prefix Search | Space |
|-----------|--------|--------|---------------|-------|
| Trie | $O(m)$ | $O(m)$ | $O(p)$ | $O(N|\Sigma|)$ |
| Hash Set | $O(m)$ | $O(m)$ | $O(N \cdot m)$ | $O(N \cdot m)$ |
| BST of Strings | $O(m \log n)$ | $O(m \log n)$ | $O(m \log n + k)$ | $O(N \cdot m)$ |
| Sorted Array | $O(n \cdot m)$ | $O(m \log n)$ | $O(m \log n + k)$ | $O(N \cdot m)$ |

---

## 🔄 Structure Visualization

```
Trie containing: ["app", "apple", "apply", "apt", "bat"]

                (root)
               /      \
              a        b
             /          \
            p            a
           / \            \
          p   t            t*
         /|\              
        l* e* y*
           |
           *
           
* = end of word marker
```

---

## 🔄 Algorithm (Pseudocode)

### Insert

```
ALGORITHM TrieInsert(root, word)
    INPUT: Trie root, word to insert
    OUTPUT: Updated trie
    
    1. current ← root
    2. FOR each character c in word DO
    3.     IF current.children[c] = NULL THEN
    4.         current.children[c] ← new TrieNode()
    5.     END IF
    6.     current ← current.children[c]
    7. END FOR
    8. current.isEndOfWord ← TRUE
```

### Search

```
ALGORITHM TrieSearch(root, word)
    INPUT: Trie root, word to search
    OUTPUT: TRUE if word exists, FALSE otherwise
    
    1. current ← root
    2. FOR each character c in word DO
    3.     IF current.children[c] = NULL THEN
    4.         RETURN FALSE
    5.     END IF
    6.     current ← current.children[c]
    7. END FOR
    8. RETURN current.isEndOfWord
```

### Prefix Search

```
ALGORITHM StartsWith(root, prefix)
    INPUT: Trie root, prefix string
    OUTPUT: TRUE if any word starts with prefix
    
    1. current ← root
    2. FOR each character c in prefix DO
    3.     IF current.children[c] = NULL THEN
    4.         RETURN FALSE
    5.     END IF
    6.     current ← current.children[c]
    7. END FOR
    8. RETURN TRUE
```

### Delete

```
ALGORITHM TrieDelete(root, word)
    INPUT: Trie root, word to delete
    OUTPUT: TRUE if deleted, FALSE if not found
    
    1. RETURN deleteHelper(root, word, 0)
    
ALGORITHM deleteHelper(node, word, depth)
    IF node = NULL THEN
        RETURN FALSE
    END IF
    
    IF depth = length(word) THEN
        IF node.isEndOfWord THEN
            node.isEndOfWord ← FALSE
            RETURN isEmpty(node)  // Safe to delete if no children
        END IF
        RETURN FALSE
    END IF
    
    c ← word[depth]
    IF deleteHelper(node.children[c], word, depth + 1) THEN
        node.children[c] ← NULL
        RETURN isEmpty(node) AND NOT node.isEndOfWord
    END IF
    RETURN FALSE
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Uses `HashMap` for children (flexible alphabet)
- Tracks word count at each node
- Supports prefix-based operations
- Efficient delete with cleanup

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/trees/Trie.java`

```java
public class Trie {
    
    private class TrieNode {
        Map<Character, TrieNode> children;
        boolean isEndOfWord;
        int wordCount;
        
        TrieNode() {
            children = new HashMap<>();
            isEndOfWord = false;
            wordCount = 0;
        }
    }
    
    private TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    // Insert a word
    public void insert(String word) {
        TrieNode current = root;
        for (char c : word.toCharArray()) {
            current.children.putIfAbsent(c, new TrieNode());
            current = current.children.get(c);
        }
        current.isEndOfWord = true;
        current.wordCount++;
    }
    
    // Search for exact word
    public boolean search(String word) {
        TrieNode node = searchNode(word);
        return node != null && node.isEndOfWord;
    }
    
    // Check if any word starts with prefix
    public boolean startsWith(String prefix) {
        return searchNode(prefix) != null;
    }
    
    // Helper to traverse to node for given string
    private TrieNode searchNode(String str) {
        TrieNode current = root;
        for (char c : str.toCharArray()) {
            if (!current.children.containsKey(c)) {
                return null;
            }
            current = current.children.get(c);
        }
        return current;
    }
    
    // Count words with given prefix
    public int countWordsWithPrefix(String prefix) {
        TrieNode node = searchNode(prefix);
        if (node == null) return 0;
        return countWords(node);
    }
    
    private int countWords(TrieNode node) {
        int count = node.wordCount;
        for (TrieNode child : node.children.values()) {
            count += countWords(child);
        }
        return count;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Autocomplete Systems
**Use Case:** Search suggestions, IDE code completion  
**Example:** Google Search, VS Code IntelliSense

### 2. Spell Checkers
**Use Case:** Dictionary lookup and suggestions  
**Example:** Microsoft Word, Grammarly

### 3. IP Routing Tables
**Use Case:** Longest prefix matching for routing  
**Example:** Network routers using CIDR notation

### 4. T9 Predictive Text
**Use Case:** Mobile phone word prediction  
**Example:** Old phone keyboards predicting words

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Google Search | Autocomplete suggestions |
| IDE (VS Code, IntelliJ) | Code completion |
| Networking Equipment | IP routing (modified tries) |
| Browsers | URL autocomplete |
| Bioinformatics | DNA sequence matching |

---

## ⚖️ Comparison with Related Data Structures

| Aspect | Trie | Radix Tree | Suffix Tree | Hash Map |
|--------|------|------------|-------------|----------|
| Prefix Operations | $O(p)$ | $O(p)$ | $O(p)$ | $O(N \cdot m)$ |
| Space | Higher | Compressed | Very High | Lower |
| Common Use | Autocomplete | IP routing | Substring search | Exact lookup |
| Implementation | Simple | Moderate | Complex | Simple |

### Trie Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| **Compressed Trie** | Merge single-child nodes | Reduce memory |
| **Radix Tree** | Store edge labels | IP routing |
| **Ternary Search Trie** | 3 children per node | Space efficient |
| **Suffix Trie** | All suffixes stored | Pattern matching |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Memory Usage:** Can consume significant memory for large alphabets
2. **Empty String:** Decide whether to support empty string insertion
3. **Case Sensitivity:** Handle consistently (convert to lowercase?)
4. **Special Characters:** Define valid character set

### Edge Cases to Handle

- [x] Empty string insertion/search
- [x] Single character words
- [x] Words that are prefixes of other words ("app" and "apple")
- [x] Delete word that is prefix of another
- [x] Non-existent word deletion

### Memory Optimization Techniques

```java
// 1. Use array instead of HashMap for small alphabets
class TrieNode {
    TrieNode[] children = new TrieNode[26];  // lowercase only
    boolean isEndOfWord;
}

// 2. Compress single-child chains (Radix Tree)
class CompressedNode {
    String edge;  // Store entire edge string
    Map<Character, CompressedNode> children;
}

// 3. Use ternary search trie for moderate space savings
class TSTNode {
    char c;
    TSTNode left, mid, right;
    boolean isEnd;
}
```

---

## 📖 References

1. Fredkin, E. (1960). "Trie memory". *Communications of the ACM*. 3 (9): 490–499.
2. Knuth, D. E. (1998). *The Art of Computer Programming, Volume 3: Sorting and Searching* (2nd ed.). Addison-Wesley. Section 6.3.
3. Sedgewick, R., & Wayne, K. (2011). *Algorithms* (4th ed.). Addison-Wesley. Chapter 5.2.
4. [Wikipedia: Trie](https://en.wikipedia.org/wiki/Trie)

---

## 🔗 Related Algorithms

- [Radix Tree](radix-tree.md) - Compressed trie variant
- [Suffix Tree](suffix-tree.md) - For substring operations
- [Aho-Corasick](../../09-string-algorithms/pattern-matching/aho-corasick.md) - Multi-pattern matching using trie
- [Hash Map](../hashing/hashmap.md) - Alternative for exact lookups
