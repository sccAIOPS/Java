# Shortest Job First (SJF) Scheduling

> **Category:** Scheduling Algorithms  
> **Subcategory:** CPU Scheduling  
> **Implementation:** [`SJFScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/SJFScheduling.java)

---

## 📚 Overview

Shortest Job First (SJF) scheduling selects the process with the smallest burst time from the ready queue. It is proven to give the minimum average waiting time among all non-preemptive scheduling algorithms.

**Key Characteristics:**
- Non-preemptive (basic version)
- Optimal for average waiting time
- Requires burst time prediction
- Can cause starvation of long processes

---

## 🔢 Algorithm Foundation

### Optimality Proof

SJF minimizes average waiting time because shorter jobs complete first, reducing the cumulative wait for all subsequent jobs.

For processes with burst times $b_1 \leq b_2 \leq ... \leq b_n$, the waiting time is:

$$
\text{Total Wait} = (n-1)b_1 + (n-2)b_2 + ... + 0 \cdot b_n
$$

Any other order results in higher total wait.

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n²) naive, O(n log n) with heap |
| Space | O(n) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM SJF(processes)
─────────────────────────────────────────────────────
    INPUT:  processes - list of (id, arrivalTime, burstTime)
    OUTPUT: scheduling order with metrics
─────────────────────────────────────────────────────

    currentTime ← 0
    completed ← 0
    n ← length(processes)
    
    WHILE completed < n DO
        // Find shortest job among arrived processes
        minBurst ← ∞
        shortest ← -1
        
        FOR i ← 0 TO n - 1 DO
            IF processes[i].arrivalTime ≤ currentTime 
               AND NOT processes[i].completed
               AND processes[i].burstTime < minBurst THEN
                minBurst ← processes[i].burstTime
                shortest ← i
            END IF
        END FOR
        
        IF shortest = -1 THEN
            currentTime ← currentTime + 1   // CPU idle
        ELSE
            P ← processes[shortest]
            P.startTime ← currentTime
            P.completionTime ← currentTime + P.burstTime
            P.turnaroundTime ← P.completionTime - P.arrivalTime
            P.waitingTime ← P.turnaroundTime - P.burstTime
            currentTime ← P.completionTime
            P.completed ← TRUE
            completed ← completed + 1
        END IF
    END WHILE
    
    RETURN processes
```

### Gantt Chart Example

**Processes:**
| Process | Arrival | Burst |
|---------|---------|-------|
| P1 | 0 | 7 |
| P2 | 2 | 4 |
| P3 | 4 | 1 |
| P4 | 5 | 4 |

**FCFS Order:**
```
|    P1     |  P2  |P3|  P4  |
0          7     11  12    16
Avg Wait: 4.0
```

**SJF Order:**
```
|    P1     |P3|  P2  |  P4  |
0          7  8     12     16
```

**Results:**
| Process | Completion | Turnaround | Waiting |
|---------|------------|------------|---------|
| P1 | 7 | 7 | 0 |
| P3 | 8 | 4 | 3 |
| P2 | 12 | 10 | 6 |
| P4 | 16 | 11 | 7 |

**Average Waiting Time:** (0 + 3 + 6 + 7) / 4 = **4.0**

---

## 💻 Implementation Notes

### Java Implementation

```java
public class SJFScheduling {
    
    public static void schedule(List<Process> processes) {
        int n = processes.size();
        int completed = 0;
        int currentTime = 0;
        boolean[] isCompleted = new boolean[n];
        
        while (completed < n) {
            // Find shortest job among arrived processes
            int shortest = -1;
            int minBurst = Integer.MAX_VALUE;
            
            for (int i = 0; i < n; i++) {
                Process p = processes.get(i);
                if (p.arrivalTime <= currentTime && 
                    !isCompleted[i] && 
                    p.burstTime < minBurst) {
                    minBurst = p.burstTime;
                    shortest = i;
                }
            }
            
            if (shortest == -1) {
                currentTime++;  // CPU idle
            } else {
                Process p = processes.get(shortest);
                p.startTime = currentTime;
                p.completionTime = currentTime + p.burstTime;
                p.turnaroundTime = p.completionTime - p.arrivalTime;
                p.waitingTime = p.turnaroundTime - p.burstTime;
                currentTime = p.completionTime;
                isCompleted[shortest] = true;
                completed++;
            }
        }
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/scheduling/SJFScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/SJFScheduling.java)

---

## 🌍 Real-World Applications

### 1. Batch Systems
**Use Case:** Optimizing throughput in offline processing

### 2. Embedded Systems
**Use Case:** Where job durations are predictable

### 3. Task Scheduling
**Use Case:** Quick tasks prioritized

---

## 🚨 Starvation Problem

Long processes may never execute if shorter processes keep arriving:

```
Time 0: P1(100) arrives
Time 1: P2(2) arrives
Time 3: P3(2) arrives
Time 5: P4(2) arrives
...

P1 may wait indefinitely!
```

**Solution:** Aging - gradually increase priority of waiting processes.

---

## ⚖️ Advantages vs Disadvantages

| Advantages | Disadvantages |
|------------|---------------|
| Optimal avg wait time | Starvation possible |
| Reduces queue length | Requires burst prediction |
| Good throughput | Not always practical |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Burst prediction | Unknown execution time | Use exponential averaging |
| Starvation | Long jobs wait forever | Implement aging |
| Tie-breaking | Equal burst times | Use FCFS as secondary |

---

## 📖 References

1. **"Operating System Concepts"** - Silberschatz, Galvin, Gagne
2. **"Process Scheduling"** - Journal of ACM

---

## 🔗 Related Algorithms

- [SRTF Scheduling](./srtf-scheduling.md)
- [FCFS Scheduling](./fcfs-scheduling.md)
- [Priority Scheduling](./priority-scheduling.md)

---

*Last updated: December 30, 2025*
