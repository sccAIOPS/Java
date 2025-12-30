# Hash Map

> **Category:** Data Structures  
> **Subcategory:** Hashing  
> **Implementation:** [HashMap.java](../../src/main/java/com/thealgorithms/datastructures/hashmap/hashing/HashMap.java)

---

## 📚 Overview

A **Hash Map** (also called Hash Table or Dictionary) is a data structure that implements an associative array, mapping keys to values. It uses a hash function to compute an index into an array of buckets, from which the desired value can be found. Hash maps provide average-case O(1) time complexity for lookups, insertions, and deletions.

---

## 🔢 Mathematical Foundation

### Hash Function

A hash function $h: K \rightarrow \{0, 1, ..., m-1\}$ maps keys to array indices:
- **Deterministic:** Same key always produces same hash
- **Uniform Distribution:** Keys should be evenly distributed
- **Efficient:** O(1) computation time

### Common Hash Functions

**Division Method:**
$$h(k) = k \mod m$$

**Multiplication Method:**
$$h(k) = \lfloor m \cdot (k \cdot A \mod 1) \rfloor$$

where $A$ is a constant (often $\frac{\sqrt{5} - 1}{2} \approx 0.618$)

### Load Factor

$$\alpha = \frac{n}{m}$$

where n = number of elements, m = table size

- $\alpha < 0.75$ typically maintains good performance
- Resize when load factor exceeds threshold

---

## 📊 Complexity Analysis

| Operation | Average Case | Worst Case | Notes |
|-----------|--------------|------------|-------|
| **get(key)** | $O(1)$ | $O(n)$ | Worst case with all collisions |
| **put(key, value)** | $O(1)$ | $O(n)$ | Amortized with resizing |
| **remove(key)** | $O(1)$ | $O(n)$ | Depends on collision handling |
| **containsKey** | $O(1)$ | $O(n)$ | Same as get |

### Space Complexity

- **Total:** $O(n)$ for n key-value pairs
- **Per Entry:** Key + Value + overhead (next pointer for chaining)
- **Load Factor Impact:** Lower α = more space, fewer collisions

---

## 🔄 Collision Resolution Strategies

### 1. Separate Chaining
```
Each bucket contains a linked list of entries
 
Index 0: → [K1:V1] → [K5:V5] → null
Index 1: → [K2:V2] → null
Index 2: → null
Index 3: → [K3:V3] → [K6:V6] → [K9:V9] → null
...
```

### 2. Open Addressing (Linear Probing)
```
If slot h(k) is occupied, try h(k)+1, h(k)+2, ...

ALGORITHM LinearProbe(key, table)
    index ← hash(key)
    WHILE table[index] is occupied AND table[index].key ≠ key DO
        index ← (index + 1) MOD table.length
    RETURN index
```

### 3. Quadratic Probing
```
Try slots: h(k), h(k)+1², h(k)+2², h(k)+3², ...
```

### 4. Double Hashing
```
h(k, i) = (h1(k) + i * h2(k)) MOD m
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

The repository implementation uses **Separate Chaining**:
- **Bucket Array:** Array of linked lists
- **Node Class:** Contains key, value, next pointer
- **Hashing:** Uses `key.hashCode() % capacity`
- **Resize:** Doubles capacity when load factor exceeds threshold

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/hashmap/hashing/HashMap.java`

```java
public class HashMap<K, V> {
    private static final int INITIAL_CAPACITY = 16;
    private static final float LOAD_FACTOR = 0.75f;
    
    private LinkedList<Entry<K, V>>[] buckets;
    private int size;
    
    private static class Entry<K, V> {
        K key;
        V value;
        
        Entry(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }
    
    @SuppressWarnings("unchecked")
    public HashMap() {
        buckets = new LinkedList[INITIAL_CAPACITY];
        for (int i = 0; i < INITIAL_CAPACITY; i++) {
            buckets[i] = new LinkedList<>();
        }
    }
    
    private int getBucketIndex(K key) {
        return Math.abs(key.hashCode() % buckets.length);
    }
    
    public void put(K key, V value) {
        int index = getBucketIndex(key);
        LinkedList<Entry<K, V>> bucket = buckets[index];
        
        for (Entry<K, V> entry : bucket) {
            if (entry.key.equals(key)) {
                entry.value = value;  // Update existing
                return;
            }
        }
        
        bucket.add(new Entry<>(key, value));  // Add new
        size++;
        
        if ((float) size / buckets.length > LOAD_FACTOR) {
            resize();
        }
    }
    
    public V get(K key) {
        int index = getBucketIndex(key);
        LinkedList<Entry<K, V>> bucket = buckets[index];
        
        for (Entry<K, V> entry : bucket) {
            if (entry.key.equals(key)) {
                return entry.value;
            }
        }
        return null;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Caching Systems
**Use Case:** Fast data retrieval from cache  
**Example:** Redis, Memcached use hash-based storage

### 2. Database Indexing
**Use Case:** Quick record lookup by key  
**Example:** In-memory database indexes, hash indexes

### 3. Symbol Tables
**Use Case:** Compiler/interpreter variable storage  
**Example:** Python dictionaries, JavaScript objects

### 4. Counting/Frequency
**Use Case:** Word frequency, vote counting  
**Example:** Log analysis, analytics systems

### 5. Deduplication
**Use Case:** Removing duplicate entries  
**Example:** Email deduplication, file comparison

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **Redis** | Key-value store engine |
| **Python** | Dict implementation |
| **Chrome** | URL cache |
| **DNS** | Domain name resolution caching |
| **Compilers** | Symbol table management |

---

## ⚖️ Comparison with Related Data Structures

| Feature | HashMap | TreeMap | LinkedHashMap |
|---------|---------|---------|---------------|
| **Order** | None | Sorted | Insertion |
| **get/put** | O(1) avg | O(log n) | O(1) avg |
| **Implementation** | Array + List | Red-Black Tree | HashMap + List |
| **Null keys** | 1 allowed | No | 1 allowed |

### Hash Map vs Alternatives

| Use Case | Best Choice | Reason |
|----------|-------------|--------|
| **Fast lookup** | HashMap | O(1) average |
| **Sorted iteration** | TreeMap | Maintains order |
| **LRU cache** | LinkedHashMap | Access order tracking |
| **Thread-safe** | ConcurrentHashMap | Fine-grained locking |

### Java Standard Library

```java
// Standard HashMap
Map<String, Integer> map = new HashMap<>();

// Concurrent version
Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();

// Sorted by keys
Map<String, Integer> treeMap = new TreeMap<>();

// Maintains insertion order
Map<String, Integer> linkedMap = new LinkedHashMap<>();
```

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Mutable Keys:** Changing key after insertion corrupts map
   - Solution: Use immutable keys (String, Integer)

2. **Poor hashCode():** Bad distribution causes many collisions
   - Solution: Implement proper hashCode() with good distribution

3. **equals() / hashCode() Contract:** If `a.equals(b)` then `a.hashCode() == b.hashCode()`
   - Solution: Always override both methods together

4. **Null Handling:** Some implementations don't allow null keys
   - Solution: Check documentation, handle nulls explicitly

5. **Iteration During Modification:** ConcurrentModificationException
   - Solution: Use Iterator.remove() or ConcurrentHashMap

### Edge Cases to Handle

- [x] Null key insertion (one allowed in Java HashMap)
- [x] Duplicate key (update existing value)
- [x] Key not found (return null)
- [x] High collision rate (automatic resize)
- [x] Empty map operations

---

## 📖 References

1. Knuth, D.E. "The Art of Computer Programming, Vol. 3" (1998)
2. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 11
3. [Wikipedia: Hash Table](https://en.wikipedia.org/wiki/Hash_table)
4. [Java HashMap Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)

---

## 🔗 Related Data Structures & Algorithms

- [HashSet](./hashset.md) - Set implementation using HashMap
- [TreeMap](../trees/treemap.md) - Sorted map using Red-Black tree
- [LinkedHashMap](./linked-hashmap.md) - Ordered hash map
- [Bloom Filter](./bloom-filter.md) - Probabilistic set membership
- [Consistent Hashing](./consistent-hashing.md) - Distributed systems
