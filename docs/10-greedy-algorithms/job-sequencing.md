# Job Sequencing with Deadlines

> **Category:** Greedy Algorithms  
> **Subcategory:** Scheduling Optimization  
> **Implementation:** [`JobSequencing.java`](../../src/main/java/com/thealgorithms/greedyalgorithms/JobSequencing.java)

---

## 📚 Overview

Job Sequencing with Deadlines is a classic greedy optimization problem where you have a set of jobs, each with a deadline and profit. Each job takes one unit of time, and a job can only be completed if it's finished before or on its deadline. The goal is to maximize total profit by selecting and scheduling jobs optimally.

**Key Characteristics:**
- Greedy algorithm that prioritizes higher profit jobs
- Uses slot-based scheduling to respect deadlines
- O(n²) time complexity with simple implementation
- Can be optimized to O(n log n) using Disjoint Set Union

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given $n$ jobs where job $i$ has deadline $d_i$ and profit $p_i$, each job takes 1 unit of time. Schedule jobs to maximize total profit, where job $i$ must complete by time $d_i$.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Feasibility | Job can be scheduled | $\exists$ slot $t \leq d_i$ |
| Profit Ordering | Higher profit first | Sort by $p_i$ descending |
| Slot Assignment | Latest available slot | Prefer slot closest to deadline |

### Mathematical Formulation

**Objective Function:**

$$
\text{maximize } \sum_{i \in S} p_i
$$

**Subject to:**
- Each selected job assigned to distinct time slot
- Job $i$ assigned to slot $t$ implies $t \leq d_i$
- $|S| \leq \max_i\{d_i\}$ (at most max deadline jobs)

**Greedy Strategy:**
1. Sort jobs by profit (descending)
2. For each job, find latest available slot ≤ deadline
3. If slot exists, schedule job

### Proof of Optimality

**Lemma (Exchange Property):** If we have a feasible schedule and swap a lower-profit job with a higher-profit unscheduled job (where the higher-profit job's deadline permits), the new schedule is at least as good.

**Theorem:** Greedy job sequencing produces maximum profit.

**Proof Sketch:**
1. Greedy always selects the highest-profit feasible job
2. If optimal doesn't include this job, by exchange argument, we can swap it in
3. Continue inductively - greedy matches optimal profit

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **Simple** | $O(n^2)$ | Linear slot search |
| **Optimized** | $O(n \log n)$ | Using Disjoint Set Union |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Slot Array** | $O(d_{max})$ | Track used slots |
| **With DSU** | $O(d_{max})$ | Parent pointers |

### Additional Properties

| Property | Value |
|----------|-------|
| **Greedy Algorithm** | Yes |
| **Optimal** | Yes (proven) |
| **Online** | No |

### Detailed Analysis

**Simple Algorithm:**
- Sorting: $O(n \log n)$
- For each job: $O(d_{max})$ to find slot
- Total: $O(n \cdot d_{max})$ ≈ $O(n^2)$

**DSU Optimization:**
- Sorting: $O(n \log n)$
- For each job: $O(\alpha(n))$ ≈ $O(1)$ with DSU
- Total: $O(n \log n)$

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Job-Sequencing(jobs)
─────────────────────────────────────────────────────
    INPUT:  Array of jobs with (id, deadline, profit)
    OUTPUT: Maximum profit and scheduled job sequence
─────────────────────────────────────────────────────

    1. n ← length(jobs)
    2. maxDeadline ← max(job.deadline for all jobs)
    
    3. // Sort jobs by profit in descending order
    4. SORT jobs by profit descending
    
    5. // Initialize slots (0 = empty)
    6. slots ← array of size maxDeadline, all set to -1
    7. scheduledJobs ← empty list
    8. totalProfit ← 0
    
    9. FOR each job in sorted jobs DO
    10.    // Find latest available slot ≤ deadline
    11.    FOR slot ← job.deadline - 1 DOWNTO 0 DO
    12.        IF slots[slot] = -1 THEN
    13.            slots[slot] ← job.id
    14.            scheduledJobs.add(job)
    15.            totalProfit ← totalProfit + job.profit
    16.            BREAK
    17.        END IF
    18.    END FOR
    19. END FOR
    
    20. RETURN totalProfit, scheduledJobs


ALGORITHM Job-Sequencing-DSU(jobs)
─────────────────────────────────────────────────────
    // Optimized version using Disjoint Set Union
─────────────────────────────────────────────────────

    1. SORT jobs by profit descending
    2. maxDeadline ← max(job.deadline)
    3. parent ← [0, 1, 2, ..., maxDeadline]  // DSU parent
    
    4. FUNCTION find(x):
    5.     IF parent[x] = x THEN RETURN x
    6.     parent[x] ← find(parent[x])  // Path compression
    7.     RETURN parent[x]
    
    8. totalProfit ← 0
    9. FOR each job in sorted jobs DO
    10.    slot ← find(min(job.deadline, maxDeadline))
    11.    IF slot > 0 THEN
    12.        parent[slot] ← slot - 1  // Mark slot used
    13.        totalProfit ← totalProfit + job.profit
    14.    END IF
    15. END FOR
    
    16. RETURN totalProfit
```

### Step-by-Step Walkthrough

**Example Input:**

| Job | Deadline | Profit |
|-----|----------|--------|
| J1 | 4 | 20 |
| J2 | 1 | 10 |
| J3 | 1 | 40 |
| J4 | 1 | 30 |

**After Sorting by Profit:**

| Job | Deadline | Profit |
|-----|----------|--------|
| J3 | 1 | 40 |
| J4 | 1 | 30 |
| J1 | 4 | 20 |
| J2 | 1 | 10 |

**Slot Assignment Process:**

| Step | Job | Deadline | Try Slots | Assigned Slot | Profit |
|------|-----|----------|-----------|---------------|--------|
| 1 | J3 | 1 | [0] | 0 | 40 |
| 2 | J4 | 1 | [0] | ✗ (full) | - |
| 3 | J1 | 4 | [3,2,1,0] | 3 | 20 |
| 4 | J2 | 1 | [0] | ✗ (full) | - |

**Slot State:**
```
Time:    │ 0  │ 1  │ 2  │ 3  │
         ├────┼────┼────┼────┤
Job:     │ J3 │ -- │ -- │ J1 │
Profit:  │ 40 │    │    │ 20 │
```

**Result:**
- Selected Jobs: J3, J1
- Total Profit: 40 + 20 = **60**

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses inner `Job` class implementing `Comparable` for sorting
- Sorts by profit in descending order
- Linear search for available slots (simple version)
- Returns list of scheduled jobs with total profit

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/greedyalgorithms/JobSequencing.java`

```java
// Key code snippet - Job class and scheduling
class Job implements Comparable<Job> {
    int id, deadline, profit;
    
    @Override
    public int compareTo(Job other) {
        return other.profit - this.profit; // Descending order
    }
}

public static int[] findJobSequence(Job[] jobs) {
    int n = jobs.length;
    Arrays.sort(jobs);
    
    int maxDeadline = 0;
    for (Job job : jobs) {
        maxDeadline = Math.max(maxDeadline, job.deadline);
    }
    
    int[] slot = new int[maxDeadline];
    Arrays.fill(slot, -1);
    
    int totalProfit = 0;
    int jobCount = 0;
    
    for (Job job : jobs) {
        // Find available slot (latest to earliest)
        for (int j = job.deadline - 1; j >= 0; j--) {
            if (slot[j] == -1) {
                slot[j] = job.id;
                totalProfit += job.profit;
                jobCount++;
                break;
            }
        }
    }
    
    return new int[]{jobCount, totalProfit};
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Task Scheduling in Operating Systems
**Use Case:** Scheduling tasks with deadlines and priorities  
**Example:** Real-time operating system task scheduling

### 2. Project Management
**Use Case:** Prioritizing tasks with deadlines and values  
**Example:** Sprint planning in Agile development

### 3. Advertising Slot Allocation
**Use Case:** Assigning ads to time slots with different values  
**Example:** TV commercial scheduling

### 4. Order Fulfillment
**Use Case:** Processing orders before their delivery deadlines  
**Example:** E-commerce order prioritization

### 5. Manufacturing Scheduling
**Use Case:** Machine job scheduling with due dates  
**Example:** Factory floor production scheduling

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| AWS Batch | Job scheduling with priorities |
| Kubernetes | Pod scheduling with constraints |
| Airlines | Gate/runway scheduling |
| Hospitals | Surgical procedure scheduling |
| Data centers | Backup job scheduling |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Job Sequencing | Activity Selection | Weighted Job Scheduling |
|--------|----------------|-------------------|------------------------|
| Objective | Max profit | Max count | Max value |
| Duration | Fixed (1 unit) | Variable | Variable |
| Deadline | Per job | End time | End time |
| Approach | Greedy | Greedy | DP |
| Time Complexity | O(n²) or O(n log n) | O(n log n) | O(n²) or O(n log n) |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Slot Indexing:** 0-indexed vs 1-indexed slots
2. **Deadline Beyond Max:** Handle deadlines exceeding job count
3. **Multiple Jobs Same Deadline:** Greedy handles by profit order
4. **All Same Deadline:** Only one job can be scheduled

### Edge Cases to Handle
- [ ] Empty job set
- [ ] All jobs have same deadline
- [ ] Deadline = 0 (impossible to schedule)
- [ ] Single job
- [ ] All jobs can be scheduled (no conflicts)

---

## 📖 References

1. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
2. Horowitz, E., Sahni, S. (1978). "Fundamentals of Computer Algorithms". Computer Science Press.
3. [GeeksforGeeks - Job Sequencing Problem](https://www.geeksforgeeks.org/job-sequencing-problem/)

---

## 🔗 Related Algorithms

- [Activity Selection](./activity-selection.md) - Interval scheduling
- [Fractional Knapsack](./fractional-knapsack.md) - Another greedy optimization
- [Weighted Job Scheduling](../03-dynamic-programming/weighted-job-scheduling.md) - DP variant
- [Disjoint Set Union](../04-data-structures/disjoint-set.md) - For O(n log n) optimization
