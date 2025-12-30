# Scheduling Algorithms

> **Category:** Operating Systems / Resource Management  
> **Difficulty:** Intermediate to Advanced  
> **Prerequisites:** Process management, queues, priority concepts

---

## 📚 Overview

Scheduling algorithms determine the order in which tasks, processes, or jobs are executed when multiple entities compete for shared resources. These algorithms are fundamental to operating systems, job schedulers, task managers, and any system that must allocate limited resources among competing demands.

**Why Learn Scheduling Algorithms?**
- **Operating Systems:** Core component of OS kernel design
- **Cloud Computing:** Resource allocation in distributed systems
- **Real-Time Systems:** Meeting timing constraints
- **Job Schedulers:** Batch processing and workflow management

---

## 🗂️ Algorithms in This Category

### CPU Scheduling Algorithms

#### Non-Preemptive Algorithms

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **FCFS (First Come First Serve)** | Processes in arrival order | [fcfs.md](./cpu/fcfs.md) |
| **SJF (Shortest Job First)** | Shortest burst time first | [sjf.md](./cpu/sjf.md) |
| **Non-Preemptive Priority** | Highest priority first | [priority-non-preemptive.md](./cpu/priority-non-preemptive.md) |

#### Preemptive Algorithms

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **SRTF (Shortest Remaining Time First)** | Preemptive SJF | [srtf.md](./cpu/srtf.md) |
| **Round Robin** | Time-slice based scheduling | [round-robin.md](./cpu/round-robin.md) |
| **Preemptive Priority** | Priority with preemption | [priority-preemptive.md](./cpu/priority-preemptive.md) |
| **MLFQ (Multi-Level Feedback Queue)** | Adaptive priority queues | [mlfq.md](./cpu/mlfq.md) |

#### Real-Time Scheduling

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **EDF (Earliest Deadline First)** | Minimum deadline first | [edf.md](./real-time/edf.md) |
| **Slack Time Scheduling** | Based on slack time | [slack-time.md](./real-time/slack-time.md) |

#### Advanced CPU Scheduling

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **HRRN (Highest Response Ratio Next)** | Response ratio optimization | [hrrn.md](./cpu/hrrn.md) |
| **Lottery Scheduling** | Probabilistic fair share | [lottery.md](./advanced/lottery.md) |
| **Fair Share Scheduling** | User/group fair allocation | [fair-share.md](./advanced/fair-share.md) |
| **Aging Scheduling** | Priority aging to prevent starvation | [aging.md](./advanced/aging.md) |
| **Self-Adjusting Scheduling** | Dynamic parameter adjustment | [self-adjusting.md](./advanced/self-adjusting.md) |

### Multi-Processor Scheduling

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Gang Scheduling** | Co-schedule related processes | [gang.md](./multiprocessor/gang.md) |
| **Multi-Agent Scheduling** | Distributed agent coordination | [multi-agent.md](./multiprocessor/multi-agent.md) |
| **Proportional Fair** | Proportional resource sharing | [proportional-fair.md](./multiprocessor/proportional-fair.md) |

### Job Scheduling

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **Job Scheduling with Deadline** | Maximize profit with deadlines | [job-deadline.md](./job/job-deadline.md) |
| **Random Scheduling** | Random job selection | [random.md](./job/random.md) |

### Disk Scheduling Algorithms

| Algorithm | Description | Documentation |
|-----------|-------------|---------------|
| **FCFS (Disk)** | First request first | [disk-fcfs.md](./disk/fcfs.md) |
| **SSF (Shortest Seek First)** | Minimize seek distance | [ssf.md](./disk/ssf.md) |
| **SCAN (Elevator)** | Sweep in one direction | [scan.md](./disk/scan.md) |
| **C-SCAN (Circular SCAN)** | One-way sweep with wrap | [c-scan.md](./disk/c-scan.md) |
| **LOOK** | SCAN without going to ends | [look.md](./disk/look.md) |
| **C-LOOK (Circular LOOK)** | C-SCAN without going to ends | [c-look.md](./disk/c-look.md) |

---

## 📊 Scheduling Metrics

| Metric | Definition | Formula |
|--------|------------|---------|
| **Arrival Time (AT)** | When process enters ready queue | Given |
| **Burst Time (BT)** | CPU time required | Given |
| **Completion Time (CT)** | When process finishes | Calculated |
| **Turnaround Time (TAT)** | Total time in system | CT - AT |
| **Waiting Time (WT)** | Time spent waiting | TAT - BT |
| **Response Time (RT)** | First response delay | First CPU - AT |
| **Throughput** | Processes per unit time | n / total_time |

---

## ⚖️ Algorithm Comparison

### CPU Scheduling Algorithms

| Algorithm | Preemptive | Starvation | Overhead | Best For |
|-----------|------------|------------|----------|----------|
| FCFS | No | No | Very Low | Batch systems |
| SJF | No | Yes | Low | Known burst times |
| SRTF | Yes | Yes | Medium | Interactive systems |
| Round Robin | Yes | No | Medium | Time-sharing |
| Priority | Both | Yes | Low | Differentiated service |
| MLFQ | Yes | No | High | General purpose |
| EDF | Yes | No | Medium | Real-time systems |
| Lottery | Yes | No | Low | Fair share |

### Disk Scheduling Algorithms

| Algorithm | Seek Time | Variance | Starvation | Best For |
|-----------|-----------|----------|------------|----------|
| FCFS | High | High | No | Low load |
| SSF | Low | High | Yes | Random access |
| SCAN | Medium | Low | No | Moderate load |
| C-SCAN | Medium | Very Low | No | Heavy load |
| LOOK | Medium | Low | No | Most systems |
| C-LOOK | Medium | Very Low | No | High throughput |

---

## 🔢 Complexity Analysis

### Time Complexity

| Algorithm | Per Schedule | Total (n processes) |
|-----------|--------------|---------------------|
| FCFS | O(1) | O(n) |
| SJF | O(n) | O(n²) |
| Priority | O(n) or O(log n)* | O(n²) or O(n log n)* |
| Round Robin | O(1) | O(n × quantum_count) |
| MLFQ | O(log k) | O(n log k) |

*With priority queue implementation

### Space Complexity

| Algorithm | Space |
|-----------|-------|
| Basic (FCFS, SJF) | O(n) |
| MLFQ | O(n × k queues) |
| Disk Scheduling | O(n) |

---

## 🌍 Real-World Applications

### 1. Operating Systems
- **Linux Completely Fair Scheduler (CFS)**
- **Windows Thread Scheduler**
- **Real-time OS schedulers (VxWorks, QNX)**

### 2. Cloud Computing
- **Kubernetes Pod Scheduling**
- **AWS Lambda function scheduling**
- **Virtual machine placement**

### 3. Database Systems
- **Query scheduler**
- **Transaction prioritization**
- **I/O request ordering**

### 4. Network Systems
- **Packet scheduling (WFQ, DRR)**
- **Quality of Service (QoS)**
- **Load balancing**

### 5. Real-Time Systems
- **Automotive ECU scheduling**
- **Avionics systems**
- **Industrial automation**

### Industry Examples

| System | Algorithm Used | Purpose |
|--------|----------------|---------|
| Linux CFS | Weighted Fair Queue | General-purpose fairness |
| Windows | Multi-level feedback | Desktop responsiveness |
| Kubernetes | Gang + Priority | Container orchestration |
| Hard RT Systems | EDF/Rate Monotonic | Deadline guarantees |

---

## 📈 Gantt Chart Visualization

A **Gantt chart** is the standard way to visualize scheduling:

```
Process Execution Timeline:

Time:    0   1   2   3   4   5   6   7   8   9   10  11  12
         +---+---+---+---+---+---+---+---+---+---+---+---+
P1:      |███████████|           |███████|
P2:      |           |███████████|       |███|
P3:      |           |           |       |   |███████|

Legend: ███ = Executing, blank = Waiting/Not arrived
```

---

## 🔧 Implementation Patterns

### Basic Scheduler Structure

```java
public abstract class Scheduler {
    protected Queue<Process> readyQueue;
    
    public abstract Process selectNextProcess();
    public abstract void addProcess(Process p);
    public abstract void onQuantumExpired(Process p);
}
```

### Priority Queue for Efficient Selection

```java
// O(log n) selection using heap
PriorityQueue<Process> queue = new PriorityQueue<>(
    Comparator.comparingInt(Process::getPriority)
);
```

---

## 📖 Recommended Learning Path

```
1. FCFS (Simplest) → 2. SJF/SRTF → 3. Priority Scheduling
        ↓                  ↓                   ↓
4. Round Robin → 5. MLFQ (Combines above) → 6. Real-Time (EDF)
        ↓                                        ↓
7. Disk Scheduling (SCAN, LOOK) → 8. Advanced (Lottery, Fair Share)
```

---

## ⚠️ Common Pitfalls

| Pitfall | Description | Solution |
|---------|-------------|----------|
| **Starvation** | Low-priority processes never execute | Aging, fair share |
| **Convoy Effect** | Short processes wait for long ones | SJF, SRTF |
| **Priority Inversion** | High-priority blocked by low | Priority inheritance |
| **Context Switch Overhead** | Too frequent switching | Larger quantum |
| **Deadline Miss** | Real-time task misses deadline | EDF, admission control |

---

## 📚 References

1. **"Operating System Concepts"** - Silberschatz, Galvin, Gagne
2. **"Modern Operating Systems"** - Andrew S. Tanenbaum
3. **"Real-Time Systems"** - Jane W. S. Liu
4. **Linux Kernel Documentation** - Scheduler design

---

## 🔗 Related Categories

- [Greedy Algorithms](../10-greedy-algorithms/README.md) - Job sequencing
- [Data Structures](../04-data-structures/README.md) - Priority queues
- [Graph Algorithms](../05-graph-algorithms/README.md) - Dependency scheduling

---

*Last updated: December 30, 2025*
