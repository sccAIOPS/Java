# Disk Scheduling Algorithms

> **Category:** Scheduling Algorithms  
> **Subcategory:** Disk Scheduling  
> **Implementations:** [`diskscheduling/`](../../src/main/java/com/thealgorithms/scheduling/diskscheduling/)

---

## 📚 Overview

Disk scheduling algorithms determine the order in which disk I/O requests are serviced. The goal is to minimize seek time - the time for the disk arm to move to the correct cylinder. These algorithms are crucial for optimizing disk performance.

**Key Concepts:**
- **Seek Time:** Time to position the head over correct track
- **Rotational Latency:** Time for sector to rotate under head
- **Transfer Time:** Time to read/write data

$$
\text{Total Access Time} = \text{Seek Time} + \text{Rotational Latency} + \text{Transfer Time}
$$

---

## 📋 Algorithm Overview

| Algorithm | Description | Starvation | Overhead |
|-----------|-------------|------------|----------|
| FCFS | First-Come, First-Served | No | Lowest |
| SSTF | Shortest Seek Time First | Yes | Low |
| SCAN | Elevator algorithm | No | Medium |
| C-SCAN | Circular SCAN | No | Medium |
| LOOK | SCAN without going to edge | No | Medium |
| C-LOOK | Circular LOOK | No | Medium |

---

## 🔄 Algorithm Details

### 1. FCFS (First-Come, First-Served)

Services requests in arrival order.

```
Head: 50, Requests: [82, 170, 43, 140, 24, 16, 190]

Movement: 50→82→170→43→140→24→16→190
Total: 642 cylinders
```

### 2. SSTF (Shortest Seek Time First)

Services nearest request first.

```
Head: 50, Requests: [82, 170, 43, 140, 24, 16, 190]

Movement: 50→43→24→16→82→140→170→190
Total: 208 cylinders
```

### 3. SCAN (Elevator)

Head moves in one direction servicing requests, then reverses.

```
Head: 50 (moving up), Disk: 0-199
Requests: [82, 170, 43, 140, 24, 16, 190]

Movement: 50→82→140→170→190→199→43→24→16
Total: 331 cylinders
```

### 4. C-SCAN (Circular SCAN)

Like SCAN but jumps to beginning after reaching end.

```
Head: 50 (moving up), Disk: 0-199
Requests: [82, 170, 43, 140, 24, 16, 190]

Movement: 50→82→140→170→190→199→0→16→24→43
Total: 390 cylinders (but more uniform wait)
```

### 5. LOOK

Like SCAN but reverses at last request, not edge.

```
Head: 50 (moving up)
Requests: [82, 170, 43, 140, 24, 16, 190]

Movement: 50→82→140→170→190→43→24→16
Total: 299 cylinders
```

### 6. C-LOOK (Circular LOOK)

Like C-SCAN but jumps at last request.

```
Head: 50 (moving up)
Requests: [82, 170, 43, 140, 24, 16, 190]

Movement: 50→82→140→170→190→16→24→43
Total: 320 cylinders
```

---

## 📊 Complexity Analysis

| Algorithm | Time | Space |
|-----------|------|-------|
| FCFS | O(n) | O(1) |
| SSTF | O(n²) | O(n) |
| SCAN | O(n log n) | O(n) |
| C-SCAN | O(n log n) | O(n) |
| LOOK | O(n log n) | O(n) |
| C-LOOK | O(n log n) | O(n) |

---

## 💻 Implementation Notes

### Java Implementation (LOOK Algorithm)

```java
public class LookScheduling {
    
    public static int[] schedule(int head, int[] requests, boolean movingUp, int diskSize) {
        List<Integer> left = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        List<Integer> sequence = new ArrayList<>();
        int totalSeek = 0;
        int current = head;
        
        // Separate requests by position relative to head
        for (int req : requests) {
            if (req < head) {
                left.add(req);
            } else {
                right.add(req);
            }
        }
        
        // Sort both lists
        Collections.sort(left);
        Collections.sort(right);
        
        if (movingUp) {
            // Service right side first, then left (reversed)
            for (int req : right) {
                sequence.add(req);
                totalSeek += Math.abs(req - current);
                current = req;
            }
            // Reverse direction at last request (not edge)
            Collections.reverse(left);
            for (int req : left) {
                sequence.add(req);
                totalSeek += Math.abs(req - current);
                current = req;
            }
        } else {
            // Service left side first (reversed), then right
            Collections.reverse(left);
            for (int req : left) {
                sequence.add(req);
                totalSeek += Math.abs(req - current);
                current = req;
            }
            for (int req : right) {
                sequence.add(req);
                totalSeek += Math.abs(req - current);
                current = req;
            }
        }
        
        return sequence.stream().mapToInt(i -> i).toArray();
    }
}
```

### Code References

📁 **Source Files:**
- [`src/main/java/com/thealgorithms/scheduling/diskscheduling/SSFScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/diskscheduling/SSFScheduling.java)
- [`src/main/java/com/thealgorithms/scheduling/diskscheduling/ScanScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/diskscheduling/ScanScheduling.java)
- [`src/main/java/com/thealgorithms/scheduling/diskscheduling/LookScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/diskscheduling/LookScheduling.java)
- [`src/main/java/com/thealgorithms/scheduling/diskscheduling/CircularScanScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/diskscheduling/CircularScanScheduling.java)
- [`src/main/java/com/thealgorithms/scheduling/diskscheduling/CircularLookScheduling.java`](../../src/main/java/com/thealgorithms/scheduling/diskscheduling/CircularLookScheduling.java)

---

## 🌍 Real-World Applications

### 1. Hard Disk Drives
**Use Case:** Traditional HDD seek optimization

### 2. Database Systems
**Use Case:** Optimizing disk I/O for queries

### 3. File Systems
**Use Case:** Block allocation and access

### Note on SSDs
SSDs don't have mechanical seek time, so disk scheduling is less critical. Random access is nearly as fast as sequential.

---

## 📈 Performance Comparison

**Test: Head at 50, Requests: [82, 170, 43, 140, 24, 16, 190]**

| Algorithm | Total Seek (cylinders) |
|-----------|------------------------|
| FCFS | 642 |
| SSTF | 208 |
| SCAN | 331 |
| C-SCAN | 390 |
| LOOK | 299 |
| C-LOOK | 320 |

---

## ⚖️ When to Use Each

| Algorithm | Best For |
|-----------|----------|
| FCFS | Low load, fairness required |
| SSTF | Throughput critical, starvation acceptable |
| SCAN | General purpose, moderate load |
| C-SCAN | Heavy load, uniform wait times |
| LOOK | SCAN with less overhead |
| C-LOOK | C-SCAN with less overhead |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| SSTF starvation | Edge requests wait forever | Use LOOK/SCAN |
| SCAN edge penalty | Unnecessary edge travel | Use LOOK |
| C-SCAN jump cost | Long jump after reversal | Consider C-LOOK |

---

## 📖 References

1. **"Operating System Concepts"** - Silberschatz, Galvin, Gagne
2. **"Modern Operating Systems"** - Tanenbaum

---

## 🔗 Related Algorithms

- [FCFS CPU Scheduling](./cpu/fcfs-scheduling.md)
- [Elevator Algorithm](../../05-graph-algorithms/README.md)

---

*Last updated: December 30, 2025*
