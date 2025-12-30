# Memory Management Algorithms

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Operating Systems  
> **Implementation:** [`MemoryManagementAlgorithms.java`](../../src/main/java/com/thealgorithms/others/MemoryManagementAlgorithms.java)

---

## 📚 Overview

Memory management algorithms determine how memory is allocated to processes. These algorithms are crucial for operating systems to efficiently manage limited memory resources among competing processes.

**Key Algorithms:**
- **First Fit:** Allocate first available block
- **Best Fit:** Allocate smallest sufficient block
- **Worst Fit:** Allocate largest available block
- **Next Fit:** Continue from last allocation point

---

## 🔢 Memory Allocation Problem

### Problem Definition

Given:
- Memory blocks: $[b_1, b_2, ..., b_n]$ with sizes $[s_1, s_2, ..., s_n]$
- Processes: $[p_1, p_2, ..., p_m]$ with requirements $[r_1, r_2, ..., r_m]$

Find an allocation that minimizes fragmentation.

### Fragmentation Types

1. **Internal Fragmentation:** Unused space within allocated block
2. **External Fragmentation:** Unused space between blocks

---

## 📊 Complexity Analysis

| Algorithm | Time | Space |
|-----------|------|-------|
| First Fit | O(n) | O(1) |
| Best Fit | O(n) | O(1) |
| Worst Fit | O(n) | O(1) |
| Next Fit | O(n) amortized | O(1) |

---

## 🔄 Algorithms (Pseudocode)

### First Fit
```
ALGORITHM FirstFit(blocks, processSize)
─────────────────────────────────────────────────────
    FOR i ← 0 TO blocks.length - 1 DO
        IF blocks[i] >= processSize THEN
            ALLOCATE processSize FROM blocks[i]
            RETURN i
        END IF
    END FOR
    RETURN -1  // No suitable block
```

### Best Fit
```
ALGORITHM BestFit(blocks, processSize)
─────────────────────────────────────────────────────
    bestIdx ← -1
    minWaste ← ∞
    
    FOR i ← 0 TO blocks.length - 1 DO
        IF blocks[i] >= processSize THEN
            waste ← blocks[i] - processSize
            IF waste < minWaste THEN
                minWaste ← waste
                bestIdx ← i
            END IF
        END IF
    END FOR
    
    IF bestIdx ≠ -1 THEN
        ALLOCATE processSize FROM blocks[bestIdx]
    END IF
    RETURN bestIdx
```

### Worst Fit
```
ALGORITHM WorstFit(blocks, processSize)
─────────────────────────────────────────────────────
    worstIdx ← -1
    maxSize ← -1
    
    FOR i ← 0 TO blocks.length - 1 DO
        IF blocks[i] >= processSize AND blocks[i] > maxSize THEN
            maxSize ← blocks[i]
            worstIdx ← i
        END IF
    END FOR
    
    IF worstIdx ≠ -1 THEN
        ALLOCATE processSize FROM blocks[worstIdx]
    END IF
    RETURN worstIdx
```

### Next Fit
```
ALGORITHM NextFit(blocks, processSize, lastIndex)
─────────────────────────────────────────────────────
    n ← blocks.length
    
    // Start from last allocation point
    FOR i ← 0 TO n - 1 DO
        idx ← (lastIndex + i) MOD n
        IF blocks[idx] >= processSize THEN
            ALLOCATE processSize FROM blocks[idx]
            RETURN idx
        END IF
    END FOR
    
    RETURN -1
```

### Step-by-Step Example

**Memory Blocks:** [100, 500, 200, 300, 600]
**Processes:** [212, 417, 112, 426]

| Process | First Fit | Best Fit | Worst Fit |
|---------|-----------|----------|-----------|
| 212 | Block 2 (500) | Block 4 (300) | Block 5 (600) |
| 417 | Block 5 (600) | Block 2 (500) | Block 2 (500) |
| 112 | Block 2 (288) | Block 3 (200) | Block 4 (300) |
| 426 | Not allocated | Block 5 (600) | Not allocated |

---

## 💻 Implementation Notes

### Java Implementation

```java
public class MemoryManagement {
    
    // First Fit
    public static int firstFit(int[] blocks, int processSize) {
        for (int i = 0; i < blocks.length; i++) {
            if (blocks[i] >= processSize) {
                blocks[i] -= processSize;
                return i;
            }
        }
        return -1;
    }
    
    // Best Fit
    public static int bestFit(int[] blocks, int processSize) {
        int bestIdx = -1;
        int minWaste = Integer.MAX_VALUE;
        
        for (int i = 0; i < blocks.length; i++) {
            if (blocks[i] >= processSize) {
                int waste = blocks[i] - processSize;
                if (waste < minWaste) {
                    minWaste = waste;
                    bestIdx = i;
                }
            }
        }
        
        if (bestIdx != -1) {
            blocks[bestIdx] -= processSize;
        }
        return bestIdx;
    }
    
    // Worst Fit
    public static int worstFit(int[] blocks, int processSize) {
        int worstIdx = -1;
        int maxSize = -1;
        
        for (int i = 0; i < blocks.length; i++) {
            if (blocks[i] >= processSize && blocks[i] > maxSize) {
                maxSize = blocks[i];
                worstIdx = i;
            }
        }
        
        if (worstIdx != -1) {
            blocks[worstIdx] -= processSize;
        }
        return worstIdx;
    }
    
    // Next Fit
    private static int lastAllocated = 0;
    
    public static int nextFit(int[] blocks, int processSize) {
        int n = blocks.length;
        
        for (int i = 0; i < n; i++) {
            int idx = (lastAllocated + i) % n;
            if (blocks[idx] >= processSize) {
                blocks[idx] -= processSize;
                lastAllocated = idx;
                return idx;
            }
        }
        return -1;
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/MemoryManagementAlgorithms.java`](../../src/main/java/com/thealgorithms/others/MemoryManagementAlgorithms.java)

---

## ⚖️ Algorithm Comparison

| Algorithm | Pros | Cons |
|-----------|------|------|
| First Fit | Fast, simple | External fragmentation |
| Best Fit | Minimal waste per allocation | Slow, tiny unusable holes |
| Worst Fit | Leaves largest holes | Fragments large blocks |
| Next Fit | Good locality | May miss better earlier blocks |

---

## 🌍 Real-World Applications

### 1. Operating Systems
**Use Case:** Process memory allocation

### 2. Embedded Systems
**Use Case:** Limited memory resource management

### 3. Database Systems
**Use Case:** Buffer pool management

### 4. Memory Allocators
**Use Case:** malloc/free implementations

---

## 🔄 Compaction

When fragmentation becomes severe:
```
ALGORITHM Compaction(memory)
─────────────────────────────────────────────────────
    // Move all allocated blocks together
    // Create single large free block
    offset ← 0
    FOR each allocated block DO
        MOVE block to offset
        offset ← offset + block.size
    END FOR
    freeBlock ← remaining memory
```

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| External fragmentation | Unusable small holes | Compaction |
| Internal fragmentation | Wasted allocated space | Better fit algorithm |
| Memory leak | Unreleased blocks | Garbage collection |
| Slow allocation | O(n) search | Use free lists/buddy system |

---

## 📖 References

1. **"Operating System Concepts"** - Silberschatz, Galvin
2. **"Modern Operating Systems"** - Tanenbaum
3. **Buddy System Paper** - Knowlton (1965)

---

## 🔗 Related Algorithms

- [Buddy System](./buddy-system.md)
- [Garbage Collection](./garbage-collection.md)
- [Page Replacement](../../14-scheduling/README.md)

---

*Last updated: December 30, 2025*
