# Aho-Corasick Algorithm

> **Category:** String Algorithms  
> **Subcategory:** Pattern Matching  
> **Implementation:** [`AhoCorasick.java`](../../../src/main/java/com/thealgorithms/strings/AhoCorasick.java)

---

## 📚 Overview

The Aho-Corasick algorithm is a string searching algorithm that efficiently finds all occurrences of multiple patterns in a text simultaneously. It constructs a finite state machine (automaton) from the patterns and processes the text in a single pass, making it optimal for searching many patterns at once.

**Key Characteristics:**
- Searches for multiple patterns in linear time O(n + m + z)
- Constructs a trie with failure (suffix) links
- Output links identify all patterns ending at each state
- Foundational algorithm for intrusion detection and bioinformatics

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given a set of patterns $P = \{P_1, P_2, ..., P_k\}$ with total length $m$ and a text $T$ of length $n$, find all occurrences of all patterns in $T$.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Trie States | Number of states in automaton | $\leq m + 1$ |
| Suffix Link | Points to longest proper suffix in trie | $f(s) = $ longest suffix of path to $s$ |
| Output Link | Points to pattern ending at suffix | $out(s) = $ pattern at $f(s)$ or $out(f(s))$ |

### Mathematical Formulation

**Trie Construction:**
For each pattern $P_i$, create a path in the trie from root to a terminal state.

**Suffix (Failure) Link:**

$$
f(s) = \text{state representing longest proper suffix of } path(s) \text{ that exists in trie}
$$

**Output Function:**

$$
output(s) = \{P_i : P_i \text{ ends at state } s\} \cup output(f(s))
$$

**Transition Function (goto + failure):**

$$
\delta(s, c) = \begin{cases}
goto(s, c) & \text{if } goto(s, c) \text{ exists} \\
\delta(f(s), c) & \text{otherwise, if } s \neq root \\
root & \text{if } s = root \text{ and } goto(root, c) \text{ doesn't exist}
\end{cases}
$$

### Proof of Correctness

**Theorem:** Aho-Corasick finds all occurrences of all patterns.

**Proof Sketch:**
1. The trie contains all patterns as paths from root
2. Suffix links ensure we consider all suffixes of the current match
3. Output links collect all patterns that are suffixes of the current state
4. Processing each text character visits all relevant pattern endings

---

## 📊 Complexity Analysis

### Time Complexity

| Phase | Complexity | Description |
|-------|------------|-------------|
| **Trie Construction** | $O(m)$ | Insert all patterns |
| **Failure Links** | $O(m)$ | BFS to compute suffix links |
| **Searching** | $O(n + z)$ | Process text, z = total matches |
| **Total** | $O(n + m + z)$ | Where z = number of pattern occurrences |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Trie Storage** | $O(m \cdot \sigma)$ | σ = alphabet size |
| **Failure Links** | $O(m)$ | One per state |
| **Output Links** | $O(m)$ | One per state |
| **Total** | $O(m \cdot \sigma)$ | Can be optimized with hashmaps |

### Additional Properties

| Property | Value |
|----------|-------|
| **Online** | Yes (text processed left to right) |
| **Multiple Patterns** | Yes (main advantage) |
| **Overlapping Matches** | Yes (all reported) |

### Detailed Analysis

**Construction Phase:**
- Each pattern character processed once: O(m)
- BFS for suffix links: O(m) states visited

**Search Phase:**
- Each text character: O(1) amortized transitions
- Each match reported: O(1)
- Total: O(n + z)

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Aho-Corasick-Build(Patterns)
─────────────────────────────────────────────────────
    INPUT:  Set of patterns P = {P1, P2, ..., Pk}
    OUTPUT: Automaton with goto, fail, and output
─────────────────────────────────────────────────────

    // Phase 1: Build Trie
    1. root ← new State()
    2. FOR each pattern P[i] in Patterns DO
    3.     curr ← root
    4.     FOR each character c in P[i] DO
    5.         IF curr.child[c] is null THEN
    6.             curr.child[c] ← new State()
    7.         END IF
    8.         curr ← curr.child[c]
    9.     END FOR
    10.    curr.patternIndex ← i    // mark pattern end
    11. END FOR

    // Phase 2: Build Failure Links (BFS)
    12. root.fail ← root
    13. queue ← empty queue
    14. FOR each child c of root DO
    15.    child.fail ← root
    16.    queue.enqueue(child)
    17. END FOR
    
    18. WHILE queue is not empty DO
    19.    curr ← queue.dequeue()
    20.    FOR each character c where curr.child[c] exists DO
    21.        child ← curr.child[c]
    22.        fail ← curr.fail
    23.        WHILE fail ≠ root AND fail.child[c] is null DO
    24.            fail ← fail.fail
    25.        END WHILE
    26.        child.fail ← fail.child[c] OR root
    27.        
    28.        // Build output link
    29.        IF child.fail.patternIndex ≥ 0 THEN
    30.            child.output ← child.fail
    31.        ELSE
    32.            child.output ← child.fail.output
    33.        END IF
    34.        queue.enqueue(child)
    35.    END FOR
    36. END WHILE


ALGORITHM Aho-Corasick-Search(text, automaton)
─────────────────────────────────────────────────────
    INPUT:  Text T, Aho-Corasick automaton
    OUTPUT: All (pattern, position) pairs
─────────────────────────────────────────────────────

    1. curr ← root
    2. results ← empty list
    
    3. FOR i ← 0 TO length(T) - 1 DO
    4.     c ← T[i]
    5.     WHILE curr ≠ root AND curr.child[c] is null DO
    6.         curr ← curr.fail
    7.     END WHILE
    8.     IF curr.child[c] exists THEN
    9.         curr ← curr.child[c]
    10.    END IF
    11.    
    12.    // Report all matches at this position
    13.    temp ← curr
    14.    WHILE temp ≠ null DO
    15.        IF temp.patternIndex ≥ 0 THEN
    16.            results.add(temp.patternIndex, i)
    17.        END IF
    18.        temp ← temp.output
    19.    END WHILE
    20. END FOR
    21. RETURN results
```

### Step-by-Step Walkthrough

**Example Input:** Patterns = {"he", "she", "his", "hers"}, Text = "ushers"

**Trie Structure:**
```
        root
       / | \
      h  s  (others)
     /|   \
    e i    h
    |      |
    r      e
    |
    s
```

**Searching "ushers":**

| i | char | state | matches found |
|---|------|-------|---------------|
| 0 | u | root | - |
| 1 | s | s | - |
| 2 | h | sh | - |
| 3 | e | she | "she", "he" |
| 4 | r | her | - |
| 5 | s | hers | "hers" |

**Result:** Found "she" at 1, "he" at 2, "hers" at 2

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses `HashMap<Character, Node>` for trie children (space efficient)
- Inner `Node` class with suffixLink, outputLink, and patternIndex
- Inner `Trie` class encapsulates automaton construction
- `PatternPositionRecorder` handles match collection

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/strings/AhoCorasick.java`

```java
// Key code snippet - Building suffix links
private void buildSuffixAndOutputLinks() {
    root.setSuffixLink(root);
    Queue<Node> q = new LinkedList<>();
    
    // Initialize root's children
    for (char rc : root.getChild().keySet()) {
        Node childNode = root.getChild().get(rc);
        q.add(childNode);
        childNode.setSuffixLink(root);
    }
    
    while (!q.isEmpty()) {
        Node currentState = q.poll();
        for (char cc : currentState.getChild().keySet()) {
            Node currentChild = currentState.getChild().get(cc);
            Node parentSuffix = currentState.getSuffixLink();
            
            while (!parentSuffix.getChild().containsKey(cc) 
                   && parentSuffix != root) {
                parentSuffix = parentSuffix.getSuffixLink();
            }
            
            if (parentSuffix.getChild().containsKey(cc)) {
                currentChild.setSuffixLink(parentSuffix.getChild().get(cc));
            } else {
                currentChild.setSuffixLink(root);
            }
            // ... output link setup
            q.add(currentChild);
        }
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Intrusion Detection Systems (IDS)
**Use Case:** Detecting malware signatures in network traffic  
**Example:** Snort IDS uses Aho-Corasick for multi-pattern matching of attack signatures

### 2. Antivirus Software
**Use Case:** Scanning files for virus signatures  
**Example:** ClamAV uses Aho-Corasick for efficient virus detection

### 3. Content Filtering
**Use Case:** Filtering prohibited words/phrases  
**Example:** Social media platforms filter inappropriate content

### 4. Bioinformatics
**Use Case:** Finding multiple DNA/protein motifs simultaneously  
**Example:** Genome annotation tools search for known gene markers

### 5. Spam Filtering
**Use Case:** Detecting spam keywords and phrases  
**Example:** Email filters match against known spam patterns

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Snort | Network intrusion detection |
| ClamAV | Antivirus scanning |
| Cloudflare | Web application firewall |
| SpamAssassin | Email spam detection |
| Hyperscan | High-performance regex matching |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Aho-Corasick | KMP | Rabin-Karp | Commentz-Walter |
|--------|--------------|-----|------------|-----------------|
| Multiple Patterns | Excellent | Poor | Good | Excellent |
| Time Complexity | O(n+m+z) | O(n·k) | O(n+m) avg | O(n+m+z) |
| Space Complexity | O(m·σ) | O(m) | O(m) | O(m·σ) |
| Construction | O(m) | O(m·k) | O(m) | O(m) |
| Best For | Many small patterns | Single pattern | Hash-based | Large alphabets |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Memory Usage:** Large pattern sets can consume significant memory
2. **Overlapping Patterns:** Ensure all overlapping matches are reported
3. **Output Link Traversal:** Don't forget to follow output links for all matches
4. **Empty Patterns:** Handle gracefully (typically skip)

### Edge Cases to Handle
- [ ] Empty pattern set
- [ ] Patterns that are prefixes of other patterns
- [ ] Single-character patterns
- [ ] Very long patterns
- [ ] Patterns with common prefixes
- [ ] No matches in text

---

## 📖 References

1. Aho, A.V., Corasick, M.J. (1975). "Efficient string matching: An aid to bibliographic search". Communications of the ACM.
2. Gusfield, D. (1997). "Algorithms on Strings, Trees, and Sequences". Cambridge University Press.
3. [Wikipedia - Aho-Corasick Algorithm](https://en.wikipedia.org/wiki/Aho%E2%80%93Corasick_algorithm)

---

## 🔗 Related Algorithms

- [KMP Algorithm](./kmp.md) - Single pattern matching
- [Rabin-Karp Algorithm](./rabin-karp.md) - Hash-based multiple pattern matching
- [Trie Data Structure](../../04-data-structures/trees/trie.md) - Foundation for Aho-Corasick
- [Commentz-Walter Algorithm](./commentz-walter.md) - Boyer-Moore for multiple patterns
