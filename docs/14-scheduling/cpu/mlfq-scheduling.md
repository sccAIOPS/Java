# Multi-Level Feedback Queue (MLFQ) Scheduling

> **Category:** Scheduling Algorithms  
> **Subcategory:** CPU Scheduling  
> **Implementation:** [`MLFQScheduler.java`](../../src/main/java/com/thealgorithms/scheduling/MLFQScheduler.java)

---

## 📚 Overview

Multi-Level Feedback Queue (MLFQ) is an advanced CPU scheduling algorithm that dynamically adjusts process priorities based on their behavior. It uses multiple queues with different priorities and time quanta, allowing the scheduler to learn about processes over time.

**Key Characteristics:**
- Multiple priority queues
- Processes move between queues based on behavior
- Favors interactive (I/O-bound) processes
- Addresses gaming and starvation concerns

---

## 🔢 Algorithm Foundation

### MLFQ Rules

1. **Rule 1:** If Priority(A) > Priority(B), A runs
2. **Rule 2:** If Priority(A) = Priority(B), A & B run in RR
3. **Rule 3:** New jobs start at highest priority
4. **Rule 4a:** If job uses entire time slice, priority reduced
5. **Rule 4b:** If job yields before slice ends, stays at same priority
6. **Rule 5:** Periodically boost all jobs to top queue (prevent starvation)

### Queue Structure

```
Queue 0 (Highest Priority): Quantum = 8ms    [Interactive]
Queue 1 (Medium Priority):  Quantum = 16ms   [Mixed]
Queue 2 (Low Priority):     Quantum = 32ms   [CPU-bound]
...
Queue n (Lowest Priority):  Quantum = ∞      [Background]
```

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n × queues × operations) |
| Space | O(n × queues) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM MLFQ(processes, numQueues, quanta)
─────────────────────────────────────────────────────
    INPUT:  processes - list of jobs
            numQueues - number of priority levels
            quanta[] - time slice for each queue
    OUTPUT: scheduling order with metrics
─────────────────────────────────────────────────────

    queues[] ← array of numQueues empty queues
    boostInterval ← configured period (e.g., 100ms)
    
    // All new processes enter highest priority queue
    FOR each P IN processes DO
        P.priority ← 0
        queues[0].enqueue(P)
    END FOR
    
    currentTime ← 0
    
    WHILE NOT all processes complete DO
        // Find highest non-empty queue
        activeQueue ← -1
        FOR i ← 0 TO numQueues - 1 DO
            IF queues[i] NOT empty THEN
                activeQueue ← i
                BREAK
            END IF
        END FOR
        
        IF activeQueue = -1 THEN
            currentTime ← nextArrival
            CONTINUE
        END IF
        
        P ← queues[activeQueue].dequeue()
        execTime ← min(quanta[activeQueue], P.remaining)
        currentTime ← currentTime + execTime
        P.remaining ← P.remaining - execTime
        
        IF P.remaining = 0 THEN
            P.completionTime ← currentTime
        ELSE IF P used full quantum THEN
            // Demote to lower priority
            newPriority ← min(activeQueue + 1, numQueues - 1)
            P.priority ← newPriority
            queues[newPriority].enqueue(P)
        ELSE
            // Yielded early - keep same priority
            queues[activeQueue].enqueue(P)
        END IF
        
        // Periodic priority boost
        IF currentTime MOD boostInterval = 0 THEN
            BOOST_ALL_TO_TOP_QUEUE()
        END IF
    END WHILE
```

### Execution Example

**Processes:**
- A: CPU-bound, burst = 20
- B: Interactive, burst = 3 (with I/O waits)

**3 Queues with quanta: 4, 8, 16**

```
Time 0-4:   A runs (Q0), uses full slice → demoted to Q1
Time 4-7:   B runs (Q0), completes early → stays Q0
            
Time 7-8:   B runs (Q0), I/O wait
Time 8-12:  A runs (Q1), uses full slice → demoted to Q2

Result: B gets good response, A gradually moves to background
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class MLFQScheduler {
    private List<Queue<Process>> queues;
    private int[] quanta;
    private int boostInterval;
    
    public MLFQScheduler(int numQueues, int[] quanta, int boostInterval) {
        this.queues = new ArrayList<>();
        for (int i = 0; i < numQueues; i++) {
            this.queues.add(new LinkedList<>());
        }
        this.quanta = quanta;
        this.boostInterval = boostInterval;
    }
    
    public void schedule(List<Process> processes) {
        // Initialize all at highest priority
        for (Process p : processes) {
            p.priority = 0;
            queues.get(0).offer(p);
        }
        
        int currentTime = 0;
        int lastBoost = 0;
        
        while (!allCompleted(processes)) {
            // Priority boost
            if (currentTime - lastBoost >= boostInterval) {
                boostAll();
                lastBoost = currentTime;
            }
            
            // Find highest non-empty queue
            int active = findActiveQueue();
            if (active == -1) {
                currentTime++;
                continue;
            }
            
            Process p = queues.get(active).poll();
            int quantum = quanta[active];
            int execTime = Math.min(quantum, p.remaining);
            
            currentTime += execTime;
            p.remaining -= execTime;
            
            if (p.remaining == 0) {
                p.completionTime = currentTime;
            } else if (execTime == quantum) {
                // Used full slice - demote
                int newPriority = Math.min(active + 1, queues.size() - 1);
                p.priority = newPriority;
                queues.get(newPriority).offer(p);
            } else {
                // Yielded early - same priority
                queues.get(active).offer(p);
            }
        }
    }
    
    private void boostAll() {
        for (int i = 1; i < queues.size(); i++) {
            while (!queues.get(i).isEmpty()) {
                Process p = queues.get(i).poll();
                p.priority = 0;
                queues.get(0).offer(p);
            }
        }
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/scheduling/MLFQScheduler.java`](../../src/main/java/com/thealgorithms/scheduling/MLFQScheduler.java)

---

## 🌍 Real-World Applications

### 1. Modern Operating Systems
**Use Case:** Windows, macOS, Linux scheduler variants

### 2. Server Systems
**Use Case:** Web server request prioritization

### 3. Real-Time Systems
**Use Case:** Mixed workload environments

---

## 🚨 Gaming Prevention

Without safeguards, processes can "game" the scheduler:

**Attack:** Yield just before time slice expires to stay in high-priority queue.

**Solutions:**
1. **Accounting:** Track total CPU time across intervals
2. **Rule 4 Modification:** If total time at priority ≥ quantum, demote

---

## ⚖️ Advantages vs Disadvantages

| Advantages | Disadvantages |
|------------|---------------|
| Adapts to process behavior | Complex implementation |
| Good for mixed workloads | Many parameters to tune |
| Prevents starvation (boost) | Gaming possible |
| Favors interactive | Overhead for queue management |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| No boost | Starvation | Implement Rule 5 |
| Gaming | Unfair advantage | Track CPU accounting |
| Too many queues | Overhead | 3-5 queues typical |
| Wrong quanta | Poor performance | Profile and tune |

---

## 📖 References

1. **"Operating Systems: Three Easy Pieces"** - Arpaci-Dusseau
2. **"Unix Internals"** - Bach, Design of UNIX OS

---

## 🔗 Related Algorithms

- [Round Robin](./round-robin.md)
- [Priority Scheduling](./priority-scheduling.md)
- [EDF Scheduling](./edf-scheduling.md)

---

*Last updated: December 30, 2025*
