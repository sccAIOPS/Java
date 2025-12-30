# Banker's Algorithm

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Operating Systems  
> **Implementation:** [`BankersAlgorithm.java`](../../src/main/java/com/thealgorithms/others/BankersAlgorithm.java)

---

## 📚 Overview

The Banker's Algorithm is a resource allocation and deadlock avoidance algorithm developed by Edsger Dijkstra. It simulates the allocation of predetermined maximum possible amounts of all resources, then makes an "safe state" check to test for possible deadlock conditions before proceeding with allocations.

**Key Characteristics:**
- Deadlock avoidance algorithm
- Requires advance knowledge of maximum resource needs
- Ensures system remains in safe state
- Used in operating systems

---

## 🔢 Definitions

### Key Terms

- **Available:** Vector of available instances of each resource
- **Max:** Matrix defining maximum demand of each process
- **Allocation:** Matrix defining currently allocated resources
- **Need:** Matrix defining remaining resource needs (Max - Allocation)

### Safe State

A state is **safe** if the system can allocate resources to each process in some order and still avoid deadlock.

**Safe Sequence:** An ordering of processes P1, P2, ..., Pn where each Pi's resource needs can be satisfied by currently available resources plus resources held by all Pj where j < i.

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Safety check | O(n² × m) | O(n × m) |
| Resource request | O(n × m) | O(m) |

Where n = processes, m = resource types.

---

## 🔄 Algorithm (Pseudocode)

### Safety Algorithm
```
ALGORITHM SafetyCheck(Available, Allocation, Need)
─────────────────────────────────────────────────────
    INPUT:  Available[m] - available resources
            Allocation[n][m] - current allocations
            Need[n][m] - remaining needs
    OUTPUT: TRUE if safe, FALSE otherwise
─────────────────────────────────────────────────────

    Work[m] ← Available
    Finish[n] ← FALSE
    safeSequence ← []
    
    REPEAT
        found ← FALSE
        FOR i ← 0 TO n-1 DO
            IF NOT Finish[i] AND Need[i] ≤ Work THEN
                // Process i can finish
                Work ← Work + Allocation[i]
                Finish[i] ← TRUE
                safeSequence.append(i)
                found ← TRUE
            END IF
        END FOR
    UNTIL NOT found
    
    IF all Finish[i] = TRUE THEN
        RETURN TRUE, safeSequence
    ELSE
        RETURN FALSE, null
    END IF
```

### Resource Request Algorithm
```
ALGORITHM ResourceRequest(processId, request)
─────────────────────────────────────────────────────
    INPUT:  processId - requesting process
            request[m] - requested resources
    OUTPUT: TRUE if granted, FALSE if must wait
─────────────────────────────────────────────────────

    // Check if request exceeds need
    IF request > Need[processId] THEN
        ERROR "Request exceeds maximum claim"
    END IF
    
    // Check if request exceeds available
    IF request > Available THEN
        RETURN FALSE  // Must wait
    END IF
    
    // Pretend to allocate
    Available ← Available - request
    Allocation[processId] ← Allocation[processId] + request
    Need[processId] ← Need[processId] - request
    
    // Check if still safe
    IF SafetyCheck(Available, Allocation, Need) THEN
        RETURN TRUE  // Grant request
    ELSE
        // Rollback
        Available ← Available + request
        Allocation[processId] ← Allocation[processId] - request
        Need[processId] ← Need[processId] + request
        RETURN FALSE  // Must wait
    END IF
```

### Step-by-Step Example

**Initial State:**
- Resources: A=3, B=3, C=2

| Process | Allocation | Max | Need |
|---------|------------|-----|------|
| P0 | 0,1,0 | 7,5,3 | 7,4,3 |
| P1 | 2,0,0 | 3,2,2 | 1,2,2 |
| P2 | 3,0,2 | 9,0,2 | 6,0,0 |
| P3 | 2,1,1 | 2,2,2 | 0,1,1 |
| P4 | 0,0,2 | 4,3,3 | 4,3,1 |

Available: [3,3,2]

**Safety Check:**

1. **P1:** Need[1,2,2] ≤ Available[3,3,2] ✓
   - Work becomes [5,3,2]
   
2. **P3:** Need[0,1,1] ≤ Available[5,3,2] ✓
   - Work becomes [7,4,3]
   
3. **P4:** Need[4,3,1] ≤ Available[7,4,3] ✓
   - Work becomes [7,4,5]
   
4. **P0:** Need[7,4,3] ≤ Available[7,4,5] ✓
   - Work becomes [7,5,5]
   
5. **P2:** Need[6,0,0] ≤ Available[7,5,5] ✓
   - Work becomes [10,5,7]

**Safe Sequence:** <P1, P3, P4, P0, P2>

---

## 💻 Implementation Notes

### Java Implementation

```java
public class BankersAlgorithm {
    private int processes;
    private int resources;
    private int[] available;
    private int[][] max;
    private int[][] allocation;
    private int[][] need;
    
    public BankersAlgorithm(int processes, int resources) {
        this.processes = processes;
        this.resources = resources;
        available = new int[resources];
        max = new int[processes][resources];
        allocation = new int[processes][resources];
        need = new int[processes][resources];
    }
    
    public boolean isSafe() {
        int[] work = available.clone();
        boolean[] finish = new boolean[processes];
        List<Integer> safeSequence = new ArrayList<>();
        
        int count = 0;
        while (count < processes) {
            boolean found = false;
            
            for (int i = 0; i < processes; i++) {
                if (!finish[i] && canFinish(i, work)) {
                    // Process i can finish
                    for (int j = 0; j < resources; j++) {
                        work[j] += allocation[i][j];
                    }
                    finish[i] = true;
                    safeSequence.add(i);
                    found = true;
                    count++;
                }
            }
            
            if (!found) {
                return false;  // Deadlock possible
            }
        }
        
        System.out.println("Safe sequence: " + safeSequence);
        return true;
    }
    
    private boolean canFinish(int process, int[] work) {
        for (int j = 0; j < resources; j++) {
            if (need[process][j] > work[j]) {
                return false;
            }
        }
        return true;
    }
    
    public boolean requestResources(int process, int[] request) {
        // Check if request is valid
        for (int j = 0; j < resources; j++) {
            if (request[j] > need[process][j]) {
                throw new IllegalArgumentException("Request exceeds max claim");
            }
            if (request[j] > available[j]) {
                return false;  // Must wait
            }
        }
        
        // Try allocation
        for (int j = 0; j < resources; j++) {
            available[j] -= request[j];
            allocation[process][j] += request[j];
            need[process][j] -= request[j];
        }
        
        // Check safety
        if (isSafe()) {
            return true;
        }
        
        // Rollback
        for (int j = 0; j < resources; j++) {
            available[j] += request[j];
            allocation[process][j] -= request[j];
            need[process][j] += request[j];
        }
        
        return false;
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/BankersAlgorithm.java`](../../src/main/java/com/thealgorithms/others/BankersAlgorithm.java)

---

## 🌍 Real-World Applications

### 1. Operating Systems
**Use Case:** Resource allocation in multi-process environments

### 2. Database Systems
**Use Case:** Locking and concurrency control

### 3. Cloud Computing
**Use Case:** Resource provisioning

### 4. Embedded Systems
**Use Case:** Memory and I/O allocation

---

## ⚖️ Pros and Cons

### Advantages
- Prevents deadlock
- Allows maximum resource utilization
- Guarantees system remains safe

### Disadvantages
- Requires knowing maximum resource needs in advance
- Processes must wait if request leads to unsafe state
- High computational overhead
- Conservative approach may under-utilize resources

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Unknown max needs | Algorithm fails | Estimate conservatively |
| Frequent requests | Performance | Batch requests |
| Dynamic processes | Changing state | Recalculate on process arrival |
| Resource preemption | Not supported | Use different algorithm |

---

## 📖 References

1. **"Operating System Concepts"** - Silberschatz, Galvin
2. **"The Structure of the THE Multiprogramming System"** - Dijkstra (1968)
3. **"Deadlock Prevention, Avoidance, and Detection"** - Coffman et al.

---

## 🔗 Related Algorithms

- [Resource Allocation Graph](./resource-allocation-graph.md)
- [Deadlock Detection](./deadlock-detection.md)
- [Memory Management](./memory-management.md)

---

*Last updated: December 30, 2025*
