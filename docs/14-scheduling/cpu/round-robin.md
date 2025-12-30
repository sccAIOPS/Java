# Round Robin (RR) Scheduling

> **Category:** Scheduling Algorithms  
> **Subcategory:** CPU Scheduling  
> **Implementation:** [`RRScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/RRScheduling.java)

---

## 📚 Overview

Round Robin (RR) is a preemptive CPU scheduling algorithm that assigns fixed time slices (quantum) to each process in circular order. It's designed for time-sharing systems where each process should get fair CPU time.

**Key Characteristics:**
- Preemptive scheduling
- Fixed time quantum
- Fair CPU allocation
- Good response time for interactive systems

---

## 🔢 Algorithm Foundation

### Time Quantum

The time quantum (or time slice) is the maximum time a process can use the CPU before being preempted:

- **Too small quantum:** High context switch overhead
- **Too large quantum:** Approaches FCFS behavior
- **Optimal:** Usually 10-100 milliseconds

### Key Relationship

$$
\text{Response Time} \leq (n-1) \times \text{Quantum}
$$

where n is the number of processes.

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n × total_burst / quantum) |
| Space | O(n) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM RoundRobin(processes, quantum)
─────────────────────────────────────────────────────
    INPUT:  processes - list of (id, arrivalTime, burstTime)
            quantum - time slice
    OUTPUT: scheduling with metrics
─────────────────────────────────────────────────────

    queue ← empty queue
    currentTime ← 0
    remaining[] ← copy of burstTimes
    
    // Add initially arrived processes
    FOR each P with arrivalTime = 0 DO
        queue.enqueue(P)
    END FOR
    
    WHILE queue NOT empty OR unfinished processes exist DO
        IF queue is empty THEN
            currentTime ← next arrival time
            ADD newly arrived processes to queue
        ELSE
            P ← queue.dequeue()
            
            // Execute for quantum or remaining time
            execTime ← min(quantum, remaining[P])
            currentTime ← currentTime + execTime
            remaining[P] ← remaining[P] - execTime
            
            // Add newly arrived processes
            ADD processes arriving during execution to queue
            
            IF remaining[P] > 0 THEN
                queue.enqueue(P)    // Not finished, re-queue
            ELSE
                P.completionTime ← currentTime
            END IF
        END IF
    END WHILE
    
    CALCULATE turnaroundTime and waitingTime
    RETURN processes
```

### Gantt Chart Example

**Processes:**
| Process | Arrival | Burst |
|---------|---------|-------|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 1 |

**Quantum = 2**

**Execution:**
```
|P1|P1|P2|P2|P3|P1|P1|P1|P2|
0  1  2  3  4  5  6  7  8  9
       ^P2    ^P3    ^P1    ^P2
```

Actually, let me correct with proper quantum:
```
|P1 |P2 |P3|P1 |P2|P1|
0   2   4  5   7  8  9
```

**Results:**
| Process | Completion | Turnaround | Waiting |
|---------|------------|------------|---------|
| P1 | 9 | 9 | 4 |
| P2 | 8 | 7 | 4 |
| P3 | 5 | 3 | 2 |

---

## 💻 Implementation Notes

### Java Implementation

```java
public class RRScheduling {
    
    public static void schedule(List<Process> processes, int quantum) {
        Queue<Process> readyQueue = new LinkedList<>();
        int currentTime = 0;
        int completed = 0;
        int n = processes.size();
        
        // Sort by arrival time
        processes.sort(Comparator.comparingInt(p -> p.arrivalTime));
        
        // Initialize remaining time
        int[] remaining = new int[n];
        for (int i = 0; i < n; i++) {
            remaining[i] = processes.get(i).burstTime;
        }
        
        int index = 0;
        // Add processes arriving at time 0
        while (index < n && processes.get(index).arrivalTime <= 0) {
            readyQueue.offer(processes.get(index++));
        }
        
        while (completed < n) {
            if (readyQueue.isEmpty()) {
                currentTime = processes.get(index).arrivalTime;
                readyQueue.offer(processes.get(index++));
            }
            
            Process current = readyQueue.poll();
            int idx = processes.indexOf(current);
            
            int execTime = Math.min(quantum, remaining[idx]);
            currentTime += execTime;
            remaining[idx] -= execTime;
            
            // Add newly arrived processes
            while (index < n && processes.get(index).arrivalTime <= currentTime) {
                readyQueue.offer(processes.get(index++));
            }
            
            if (remaining[idx] > 0) {
                readyQueue.offer(current);  // Re-queue
            } else {
                current.completionTime = currentTime;
                current.turnaroundTime = current.completionTime - current.arrivalTime;
                current.waitingTime = current.turnaroundTime - current.burstTime;
                completed++;
            }
        }
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/scheduling/RRScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/RRScheduling.java)

---

## 🌍 Real-World Applications

### 1. Time-Sharing Systems
**Use Case:** Desktop operating systems (Windows, Linux)

### 2. Interactive Systems
**Use Case:** Ensuring responsive user interfaces

### 3. Web Servers
**Use Case:** Fair request handling

---

## 📈 Quantum Selection

| Quantum Size | Context Switches | Response Time | Throughput |
|--------------|------------------|---------------|------------|
| Very Small | Very High | Very Good | Poor |
| Small | High | Good | Fair |
| Medium | Moderate | Moderate | Good |
| Large | Low | Poor | Good |
| Very Large | Very Low | Very Poor | Like FCFS |

---

## ⚖️ Advantages vs Disadvantages

| Advantages | Disadvantages |
|------------|---------------|
| Fair CPU distribution | Context switch overhead |
| Good response time | Poor for I/O-bound |
| No starvation | Average waiting can be high |
| Simple to implement | Quantum selection critical |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Wrong quantum | Performance issues | Profile and tune |
| Queue order | Affects fairness | Consider arrival carefully |
| Context switch | Overhead ignored | Include in analysis |

---

## 📖 References

1. **"Operating System Concepts"** - Silberschatz, Galvin, Gagne
2. **"Modern Operating Systems"** - Tanenbaum

---

## 🔗 Related Algorithms

- [FCFS Scheduling](./fcfs-scheduling.md)
- [MLFQ Scheduling](./mlfq-scheduling.md)
- [Priority Scheduling](./priority-scheduling.md)

---

*Last updated: December 30, 2025*
