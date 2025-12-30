# Tower of Hanoi

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Puzzles and Games  
> **Implementation:** [`TowerOfHanoi.java`](../../src/main/java/com/thealgorithms/puzzlesandgames/TowerOfHanoi.java)

---

## 📚 Overview

The Tower of Hanoi is a classic mathematical puzzle consisting of three rods and a number of disks of different sizes. The objective is to move all disks from the source rod to the destination rod, following specific rules.

**Rules:**
1. Only one disk can be moved at a time
2. Each move takes the upper disk from one rod and places it on another
3. No larger disk may be placed on top of a smaller disk

---

## 🔢 Mathematical Foundation

### Number of Moves

For n disks, the minimum number of moves is:
$$
T(n) = 2^n - 1
$$

**Proof by Recurrence:**
$$
T(n) = 2 \times T(n-1) + 1
$$
- Move n-1 disks to auxiliary: T(n-1)
- Move largest disk to destination: 1
- Move n-1 disks to destination: T(n-1)

### Solution:
$$
T(n) = 2^n - 1
$$

| Disks | Moves |
|-------|-------|
| 1 | 1 |
| 2 | 3 |
| 3 | 7 |
| 10 | 1,023 |
| 64 | 18,446,744,073,709,551,615 |

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(2^n) |
| Space (recursive) | O(n) stack depth |
| Space (iterative) | O(n) state storage |

---

## 🔄 Algorithm (Pseudocode)

### Recursive Solution
```
ALGORITHM TowerOfHanoi(n, source, auxiliary, destination)
─────────────────────────────────────────────────────
    INPUT:  n - number of disks
            source - starting rod
            auxiliary - helper rod
            destination - target rod
    OUTPUT: Sequence of moves
─────────────────────────────────────────────────────

    IF n = 0 THEN
        RETURN
    END IF
    
    // Move n-1 disks from source to auxiliary
    TowerOfHanoi(n-1, source, destination, auxiliary)
    
    // Move nth (largest) disk from source to destination
    PRINT "Move disk " + n + " from " + source + " to " + destination
    
    // Move n-1 disks from auxiliary to destination
    TowerOfHanoi(n-1, auxiliary, source, destination)
```

### Step-by-Step Example (3 Disks)

**Initial State:**
```
     |          |          |
    [1]         |          |
   [2 2]        |          |
  [3 3 3]       |          |
─────────────────────────────────
    A           B          C
```

**Moves:**
1. Move disk 1: A → C
2. Move disk 2: A → B
3. Move disk 1: C → B
4. Move disk 3: A → C
5. Move disk 1: B → A
6. Move disk 2: B → C
7. Move disk 1: A → C

**Final State:**
```
     |          |          |
     |          |         [1]
     |          |        [2 2]
     |          |       [3 3 3]
─────────────────────────────────
    A           B          C
```

---

## 💻 Implementation Notes

### Java Implementation (Recursive)

```java
public class TowerOfHanoi {
    
    public static void solve(int n, char source, char auxiliary, char destination) {
        if (n == 0) {
            return;
        }
        
        // Move n-1 disks from source to auxiliary
        solve(n - 1, source, destination, auxiliary);
        
        // Move nth disk from source to destination
        System.out.println("Move disk " + n + " from " + source + " to " + destination);
        
        // Move n-1 disks from auxiliary to destination
        solve(n - 1, auxiliary, source, destination);
    }
    
    public static void main(String[] args) {
        int n = 3;
        solve(n, 'A', 'B', 'C');
    }
}
```

### Java Implementation (Iterative)

```java
public class TowerOfHanoiIterative {
    
    public static void solve(int n, char source, char auxiliary, char destination) {
        Stack<int[]> stack = new Stack<>();
        // State: [n, source, aux, dest, stage]
        stack.push(new int[]{n, 0, 1, 2, 0});
        
        char[] rods = {source, auxiliary, destination};
        
        while (!stack.isEmpty()) {
            int[] state = stack.pop();
            int disks = state[0];
            int src = state[1];
            int aux = state[2];
            int dst = state[3];
            int stage = state[4];
            
            if (disks == 0) continue;
            
            if (stage == 0) {
                // Push next stages in reverse order
                stack.push(new int[]{disks - 1, aux, src, dst, 0});
                stack.push(new int[]{disks, src, aux, dst, 1});
                stack.push(new int[]{disks - 1, src, dst, aux, 0});
            } else {
                System.out.println("Move disk " + disks + " from " + rods[src] + " to " + rods[dst]);
            }
        }
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/puzzlesandgames/TowerOfHanoi.java`](../../src/main/java/com/thealgorithms/puzzlesandgames/TowerOfHanoi.java)

---

## 🌍 Real-World Applications

### 1. Algorithm Education
**Use Case:** Teaching recursion concepts

### 2. Backup Systems
**Use Case:** Multi-level backup strategies

### 3. Memory Management
**Use Case:** Stack-based operations

### 4. Computer Science
**Use Case:** Recursion and divide-and-conquer paradigm

---

## 📚 Variations

### Frame-Stewart Algorithm (4+ pegs)
For k pegs and n disks:
$$
T(n, k) = 2 \times T(n-r, k) + T(r, k-1)
$$

### Cyclic Hanoi
Disks can only move to adjacent pegs (A→B→C→A).

### Colored Hanoi
Disks of same color cannot be stacked.

---

## 🧮 Mathematical Properties

### Binary Representation
Each state can be encoded as n-digit ternary number.

### Gray Code Connection
The moves form a binary reflected Gray code sequence.

### Sierpinski Triangle
Tower of Hanoi solution graph forms a Sierpinski triangle.

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Stack overflow | Large n | Use iterative version |
| Wrong rod order | Incorrect moves | Consistent parameter naming |
| Off-by-one | Missing moves | Test with small n |
| Infinite recursion | Missing base case | Check n = 0 or n = 1 |

---

## 📖 References

1. **"Concrete Mathematics"** - Graham, Knuth, Patashnik
2. **"The Tower of Hanoi – Myths and Maths"** - Hinz et al.
3. **Original Puzzle** - Édouard Lucas (1883)

---

## 🔗 Related Algorithms

- [Recursion Fundamentals](../../recursion/README.md)
- [Divide and Conquer](../../11-divide-and-conquer/README.md)
- [Dynamic Programming](../../03-dynamic-programming/README.md)

---

*Last updated: December 30, 2025*
