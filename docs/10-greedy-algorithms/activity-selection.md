# Activity Selection Problem

> **Category:** Greedy Algorithms  
> **Subcategory:** Interval Scheduling  
> **Implementation:** [`ActivitySelection.java`](../../src/main/java/com/thealgorithms/greedyalgorithms/ActivitySelection.java)

---

## 📚 Overview

The Activity Selection problem is a classic greedy algorithm problem that aims to select the maximum number of non-overlapping activities from a set of activities, each with a start and finish time. It demonstrates the greedy choice property where locally optimal choices lead to a globally optimal solution.

**Key Characteristics:**
- Greedy algorithm with proven optimal solution
- O(n log n) time complexity (dominated by sorting)
- Foundation for interval scheduling problems
- Direct application in resource allocation and scheduling

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given a set $S = \{a_1, a_2, ..., a_n\}$ of activities where activity $a_i$ has start time $s_i$ and finish time $f_i$, find a maximum-size subset of mutually compatible activities.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Compatibility | Activities don't overlap | $f_i \leq s_j$ or $f_j \leq s_i$ |
| Greedy Choice | Select earliest finishing activity | $\arg\min_i\{f_i\}$ |
| Optimal Substructure | Subproblems are also optimal | After choosing $a_1$, solve for $S' = \{a_i : s_i \geq f_1\}$ |

### Mathematical Formulation

**Objective Function:**

$$
\text{maximize } |A| \text{ subject to } \forall a_i, a_j \in A: \text{compatible}(a_i, a_j)
$$

**Greedy Strategy:**

$$
a^* = \arg\min_{a_i \in S} f_i
$$

After selecting $a^*$, recursively solve for remaining compatible activities.

### Proof of Correctness (Greedy Choice Property)

**Theorem:** If $S$ is a set of activities sorted by finish time, then the first activity $a_1$ is included in some optimal solution.

**Proof:**
1. Let $A$ be an optimal solution and $a_k$ be the activity with earliest finish time in $A$.
2. If $a_k = a_1$, we're done.
3. If $a_k \neq a_1$, then $f_1 \leq f_k$ (by sorting).
4. Construct $A' = (A - \{a_k\}) \cup \{a_1\}$.
5. Since $f_1 \leq f_k$, activity $a_1$ is compatible with all activities that $a_k$ was compatible with.
6. Therefore $|A'| = |A|$, and $A'$ is also optimal.
7. Thus, there exists an optimal solution containing $a_1$. ∎

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **Best** | $O(n)$ | Already sorted by finish time |
| **Average** | $O(n \log n)$ | Sorting dominates |
| **Worst** | $O(n \log n)$ | Always need to sort |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary Space** | $O(n)$ | For sorted array |
| **Output Space** | $O(k)$ | k = number of selected activities |

### Additional Properties

| Property | Value |
|----------|-------|
| **Greedy Algorithm** | Yes |
| **Optimal** | Yes (proven) |
| **Online** | No (needs all activities upfront) |

### Detailed Analysis

**Sorting Phase:** $O(n \log n)$
- Sort activities by finish time

**Selection Phase:** $O(n)$
- Single pass through sorted activities
- Each activity considered exactly once

**Total:** $O(n \log n)$ dominated by sorting

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Activity-Selection(activities)
─────────────────────────────────────────────────────
    INPUT:  Array of activities with (start, finish) times
    OUTPUT: Maximum set of non-overlapping activities
─────────────────────────────────────────────────────

    1. SORT activities by finish time in ascending order
    
    2. selected ← empty list
    3. lastFinish ← -∞
    
    4. FOR each activity a in sorted activities DO
    5.     IF a.start ≥ lastFinish THEN
    6.         selected.add(a)
    7.         lastFinish ← a.finish
    8.     END IF
    9. END FOR
    
    10. RETURN selected


ALGORITHM Activity-Selection-Recursive(activities, k, n)
─────────────────────────────────────────────────────
    INPUT:  Sorted activities, current index k, total n
    OUTPUT: Maximum compatible activities from k to n
─────────────────────────────────────────────────────

    1. m ← k + 1
    2. // Find first activity starting after activity k finishes
    3. WHILE m < n AND activities[m].start < activities[k].finish DO
    4.     m ← m + 1
    5. END WHILE
    
    6. IF m < n THEN
    7.     RETURN {activities[m]} ∪ Activity-Selection-Recursive(activities, m, n)
    8. ELSE
    9.     RETURN ∅
    10. END IF
```

### Step-by-Step Walkthrough

**Example Input:** 
| Activity | Start | Finish |
|----------|-------|--------|
| a₁ | 1 | 4 |
| a₂ | 3 | 5 |
| a₃ | 0 | 6 |
| a₄ | 5 | 7 |
| a₅ | 3 | 9 |
| a₆ | 5 | 9 |
| a₇ | 6 | 10 |
| a₈ | 8 | 11 |
| a₉ | 8 | 12 |
| a₁₀ | 2 | 14 |
| a₁₁ | 12 | 16 |

**After Sorting by Finish Time:**
| Activity | Start | Finish | Selected? |
|----------|-------|--------|-----------|
| a₁ | 1 | 4 | ✓ (lastFinish = 4) |
| a₂ | 3 | 5 | ✗ (3 < 4) |
| a₃ | 0 | 6 | ✗ (0 < 4) |
| a₄ | 5 | 7 | ✓ (5 ≥ 4, lastFinish = 7) |
| a₅ | 3 | 9 | ✗ (3 < 7) |
| a₆ | 5 | 9 | ✗ (5 < 7) |
| a₇ | 6 | 10 | ✗ (6 < 7) |
| a₈ | 8 | 11 | ✓ (8 ≥ 7, lastFinish = 11) |
| a₉ | 8 | 12 | ✗ (8 < 11) |
| a₁₀ | 2 | 14 | ✗ (2 < 11) |
| a₁₁ | 12 | 16 | ✓ (12 ≥ 11, lastFinish = 16) |

**Result:** {a₁, a₄, a₈, a₁₁} - 4 activities selected

**Visual Timeline:**
```
Time:    0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16
a₁:      ─────────▓▓▓▓▓▓
a₄:                  ▓▓▓▓▓▓▓
a₈:                           ▓▓▓▓▓▓▓▓▓▓
a₁₁:                                     ▓▓▓▓▓▓▓▓▓▓▓▓
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Sorts activities array by end time using Arrays.sort with custom comparator
- Uses single pass through sorted activities
- Returns indices of selected activities
- Clean, straightforward greedy implementation

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/greedyalgorithms/ActivitySelection.java`

```java
// Key code snippet - Activity Selection
public static List<Integer> activitySelection(int[] startTimes, int[] endTimes) {
    int n = startTimes.length;
    int[][] activities = new int[n][3];
    
    for (int i = 0; i < n; i++) {
        activities[i][0] = i;          // original index
        activities[i][1] = startTimes[i];
        activities[i][2] = endTimes[i];
    }
    
    // Sort by finish time
    Arrays.sort(activities, (a, b) -> a[2] - b[2]);
    
    List<Integer> selected = new ArrayList<>();
    int lastFinish = Integer.MIN_VALUE;
    
    for (int[] activity : activities) {
        if (activity[1] >= lastFinish) {
            selected.add(activity[0]);
            lastFinish = activity[2];
        }
    }
    
    return selected;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Meeting Room Scheduling
**Use Case:** Maximizing the number of meetings in a conference room  
**Example:** Google Calendar's "Find a time" feature

### 2. CPU Task Scheduling
**Use Case:** Scheduling non-preemptive tasks on a single processor  
**Example:** Batch job scheduling in operating systems

### 3. Event Planning
**Use Case:** Selecting maximum non-conflicting events  
**Example:** Festival/conference schedule optimization

### 4. Resource Allocation
**Use Case:** Allocating shared resources with time windows  
**Example:** Car/bike sharing systems

### 5. Classroom Assignment
**Use Case:** Assigning classrooms to course sections  
**Example:** University course scheduling

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Google Calendar | Meeting scheduling |
| Airlines | Aircraft turnaround scheduling |
| Amazon | Warehouse robot task allocation |
| TV Networks | Program scheduling |
| Hospitals | Operating room scheduling |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Activity Selection | Weighted Job Scheduling | Interval Partitioning |
|--------|-------------------|------------------------|----------------------|
| Objective | Max count | Max weight/value | Min resources |
| Approach | Greedy | Dynamic Programming | Greedy |
| Time Complexity | O(n log n) | O(n²) or O(n log n) | O(n log n) |
| Optimal | Yes | Yes | Yes |
| Use Case | Equal importance | Varying importance | Multiple resources |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Forgetting to Sort:** Must sort by finish time, not start time
2. **Tie Breaking:** When finish times are equal, any consistent order works
3. **Inclusive/Exclusive Boundaries:** Clarify if end time is inclusive
4. **Empty Input:** Return empty set

### Edge Cases to Handle
- [ ] Empty activity set
- [ ] Single activity (always selected)
- [ ] All activities overlap (select one)
- [ ] No activities overlap (select all)
- [ ] Activities with zero duration

---

## 📖 References

1. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press. Chapter 16.
2. Kleinberg, J., Tardos, E. (2005). "Algorithm Design". Addison-Wesley.
3. [Wikipedia - Activity Selection Problem](https://en.wikipedia.org/wiki/Activity_selection_problem)

---

## 🔗 Related Algorithms

- [Weighted Job Scheduling](./weighted-job-scheduling.md) - Jobs with different values
- [Interval Partitioning](./interval-partitioning.md) - Minimizing resources
- [Meeting Rooms Problem](./meeting-rooms.md) - Variant applications
- [Fractional Knapsack](./fractional-knapsack.md) - Another greedy algorithm
