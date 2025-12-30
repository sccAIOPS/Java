# First-Come, First-Served (FCFS) Scheduling

> **Category:** Scheduling Algorithms  
> **Subcategory:** CPU Scheduling  
> **Implementation:** [`FCFSScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/FCFSScheduling.java)

---

## 📚 Overview

First-Come, First-Served (FCFS) is the simplest CPU scheduling algorithm. Processes are executed in the order they arrive in the ready queue, using a FIFO (First-In, First-Out) data structure.

**Key Characteristics:**
- Non-preemptive scheduling
- Simple FIFO queue implementation
- No starvation possible
- Convoy effect possible

---

## 🔢 Algorithm Foundation

### Process Attributes

| Attribute | Description |
|-----------|-------------|
| Arrival Time | When process enters ready queue |
| Burst Time | CPU time required for completion |
| Completion Time | When process finishes execution |
| Turnaround Time | Completion - Arrival |
| Waiting Time | Turnaround - Burst |

### Performance Metrics

$$
\text{Turnaround Time} = \text{Completion Time} - \text{Arrival Time}
$$

$$
\text{Waiting Time} = \text{Turnaround Time} - \text{Burst Time}
$$

$$
\text{Average Waiting Time} = \frac{\sum \text{Waiting Times}}{n}
$$

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n) |
| Space | O(n) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM FCFS(processes)
─────────────────────────────────────────────────────
    INPUT:  processes - list of (id, arrivalTime, burstTime)
    OUTPUT: scheduling order with completion times
─────────────────────────────────────────────────────

    // Sort by arrival time
    SORT processes BY arrivalTime
    
    currentTime ← 0
    
    FOR each process P IN processes DO
        IF currentTime < P.arrivalTime THEN
            currentTime ← P.arrivalTime   // CPU idle
        END IF
        
        P.startTime ← currentTime
        P.completionTime ← currentTime + P.burstTime
        currentTime ← P.completionTime
        
        P.turnaroundTime ← P.completionTime - P.arrivalTime
        P.waitingTime ← P.turnaroundTime - P.burstTime
    END FOR
    
    RETURN processes
```

### Gantt Chart Example

**Processes:**
| Process | Arrival | Burst |
|---------|---------|-------|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 8 |

**Gantt Chart:**
```
|   P1   |  P2 |    P3     |
0        5     8          16
```

**Results:**
| Process | Completion | Turnaround | Waiting |
|---------|------------|------------|---------|
| P1 | 5 | 5 | 0 |
| P2 | 8 | 7 | 4 |
| P3 | 16 | 14 | 6 |

**Average Waiting Time:** (0 + 4 + 6) / 3 = **3.33**

---

## 💻 Implementation Notes

### Java Implementation

```java
public class FCFSScheduling {
    
    public static void schedule(List<Process> processes) {
        // Sort by arrival time
        processes.sort(Comparator.comparingInt(p -> p.arrivalTime));
        
        int currentTime = 0;
        
        for (Process p : processes) {
            // Handle idle time
            if (currentTime < p.arrivalTime) {
                currentTime = p.arrivalTime;
            }
            
            p.startTime = currentTime;
            p.completionTime = currentTime + p.burstTime;
            currentTime = p.completionTime;
            
            p.turnaroundTime = p.completionTime - p.arrivalTime;
            p.waitingTime = p.turnaroundTime - p.burstTime;
        }
    }
}

class Process {
    int id;
    int arrivalTime;
    int burstTime;
    int startTime;
    int completionTime;
    int turnaroundTime;
    int waitingTime;
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/scheduling/FCFSScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/FCFSScheduling.java)

---

## 🌍 Real-World Applications

### 1. Print Queues
**Use Case:** Print jobs processed in submission order

### 2. Batch Processing
**Use Case:** Sequential job processing

### 3. Simple Embedded Systems
**Use Case:** Resource-constrained environments

---

## ⚖️ Advantages vs Disadvantages

| Advantages | Disadvantages |
|------------|---------------|
| Simple to implement | Convoy effect |
| No starvation | High average waiting time |
| Fair (order-based) | Not optimal |
| Low overhead | No priority support |

---

## 🚨 The Convoy Effect

When a long process holds the CPU, shorter processes must wait:

```
Without Convoy:          With Convoy:
P1(2) P2(1) P3(1)       P3(10) P1(2) P2(1)
|P1|P2|P3|              |    P3    |P1|P2|
0  2  3  4              0         10 12 13

Avg Wait: 1.0           Avg Wait: 5.67
```

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Convoy effect | Long wait times | Consider SJF/SRTF |
| Idle CPU | Gaps between arrivals | Track and report |
| Unsorted input | Incorrect results | Always sort first |

---

## 📖 References

1. **"Operating System Concepts"** - Silberschatz, Galvin, Gagne
2. **"Modern Operating Systems"** - Tanenbaum

---

## 🔗 Related Algorithms

- [Shortest Job First (SJF)](./sjf-scheduling.md)
- [Round Robin](./round-robin.md)
- [Priority Scheduling](./priority-scheduling.md)

---

*Last updated: December 30, 2025*
