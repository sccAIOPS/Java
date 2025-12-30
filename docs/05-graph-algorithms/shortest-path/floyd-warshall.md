# Floyd-Warshall Algorithm

> **Category:** Graph Algorithms  
> **Subcategory:** Shortest Path  
> **Implementation:** [FloydWarshall.java](../../../src/main/java/com/thealgorithms/datastructures/graphs/FloydWarshall.java)

---

## 📚 Overview

The Floyd-Warshall algorithm is a dynamic programming approach for finding the **shortest paths between all pairs of vertices** in a weighted graph. It works with both positive and negative edge weights and can detect negative-weight cycles.

Named after Robert Floyd and Stephen Warshall, this algorithm is particularly elegant due to its simplicity and its ability to solve the all-pairs shortest path problem in a single pass.

---

## 🔢 Mathematical Foundation

### Definition

Given a weighted graph $G = (V, E)$ with edge weights $w: E \rightarrow \mathbb{R}$, the Floyd-Warshall algorithm computes the shortest path distance $d(i, j)$ for all pairs of vertices $(i, j) \in V \times V$.

### Key Properties

- **All-Pairs Solution:** Computes shortest paths for every vertex pair simultaneously
- **Negative Weights:** Handles negative edge weights (but not negative cycles)
- **In-Place Update:** Uses single distance matrix with iterative updates
- **Transitivity:** Based on the transitive closure concept

### Mathematical Formulation

**Recurrence Relation:**
$$
d^{(k)}[i][j] = \min(d^{(k-1)}[i][j], d^{(k-1)}[i][k] + d^{(k-1)}[k][j])
$$

Where $d^{(k)}[i][j]$ is the shortest path from $i$ to $j$ using only intermediate vertices from set $\{1, 2, ..., k\}$.

**Base Case:**
$$
d^{(0)}[i][j] = \begin{cases}
0 & \text{if } i = j \\
w(i,j) & \text{if edge } (i,j) \text{ exists} \\
\infty & \text{otherwise}
\end{cases}
$$

#### Proof of Correctness

**Invariant:** After iteration $k$, $d[i][j]$ contains the shortest path from $i$ to $j$ using only vertices $\{1, 2, ..., k\}$ as intermediates.

**Induction:** Either the shortest path through $\{1,...,k\}$ goes through $k$ or it doesn't:
- If not through $k$: $d^{(k)}[i][j] = d^{(k-1)}[i][j]$
- If through $k$: $d^{(k)}[i][j] = d^{(k-1)}[i][k] + d^{(k-1)}[k][j]$

---

## 📊 Complexity Analysis

| Metric | Best Case | Average Case | Worst Case |
|--------|-----------|--------------|------------|
| **Time** | $O(V^3)$ | $O(V^3)$ | $O(V^3)$ |
| **Space** | $O(V^2)$ | $O(V^2)$ | $O(V^2)$ |

### Detailed Analysis

- **Three nested loops:** Each runs $V$ times
- **Total operations:** $V \times V \times V = O(V^3)$
- **No early termination:** Always runs full iterations

**Space Complexity:**
- Distance matrix: $O(V^2)$
- Path reconstruction matrix: $O(V^2)$ (optional)

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM FloydWarshall(G)
    INPUT: Graph G with V vertices (adjacency matrix representation)
    OUTPUT: dist[V][V] matrix of all shortest path distances
    
    // Initialize distance matrix
    1. FOR each pair (i, j) in V × V DO
    2.     IF i = j THEN
    3.         dist[i][j] ← 0
    4.     ELSE IF edge (i,j) exists THEN
    5.         dist[i][j] ← weight(i, j)
    6.     ELSE
    7.         dist[i][j] ← ∞
    8.     END IF
    9. END FOR
    
    // Main algorithm - consider each vertex as intermediate
    10. FOR k ← 0 TO V-1 DO
    11.     FOR i ← 0 TO V-1 DO
    12.         FOR j ← 0 TO V-1 DO
    13.             IF dist[i][k] + dist[k][j] < dist[i][j] THEN
    14.                 dist[i][j] ← dist[i][k] + dist[k][j]
    15.             END IF
    16.         END FOR
    17.     END FOR
    18. END FOR
    
    // Negative cycle detection
    19. FOR i ← 0 TO V-1 DO
    20.     IF dist[i][i] < 0 THEN
    21.         RETURN "Negative Cycle Detected"
    22.     END IF
    23. END FOR
    
    24. RETURN dist[][]
```

### Step-by-Step Walkthrough

Consider this weighted graph:
```
     (3)
  0 ──────→ 1
  │         │
(8)│       (1)│
  ↓    (-4)  ↓
  2 ←─────── 3
  │         ↑
  └──(7)────┘
```

**Initial Matrix (k=0):**
```
      0    1    2    3
  ┌─────────────────────┐
0 │  0    3    8    ∞  │
1 │  ∞    0    ∞    1  │
2 │  ∞    ∞    0    7  │
3 │  ∞   -4    ∞    0  │
  └─────────────────────┘
```

**After k=1 (using vertex 1 as intermediate):**
```
      0    1    2    3
  ┌─────────────────────┐
0 │  0    3    8    4  │  ← d[0][3] = min(∞, d[0][1]+d[1][3]) = 3+1 = 4
1 │  ∞    0    ∞    1  │
2 │  ∞    ∞    0    7  │
3 │  ∞   -4    ∞    0  │
  └─────────────────────┘
```

**Final Matrix (after all k iterations):**
```
      0    1    2    3
  ┌─────────────────────┐
0 │  0    3    8    4  │
1 │  ∞    0    ∞    1  │
2 │  ∞    3    0    4  │
3 │  ∞   -4    ∞    0  │
  └─────────────────────┘
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Uses adjacency matrix representation
- INFINITY constant (999) represents unreachable paths
- In-place matrix updates for space efficiency
- Includes distance matrix printing utility

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/graphs/FloydWarshall.java`

```java
public void floydwarshall(int[][] adjacencyMatrix) {
    // Main Floyd-Warshall algorithm
    for (int k = 0; k < numberofvertices; k++) {
        for (int i = 0; i < numberofvertices; i++) {
            for (int j = 0; j < numberofvertices; j++) {
                if (distanceMatrix[i][k] + distanceMatrix[k][j] 
                    < distanceMatrix[i][j]) {
                    distanceMatrix[i][j] = 
                        distanceMatrix[i][k] + distanceMatrix[k][j];
                }
            }
        }
    }
}
```

### Implementation Pattern

```java
// Create instance with number of vertices
FloydWarshall fw = new FloydWarshall(4);

// Run algorithm with adjacency matrix
fw.floydwarshall(adjacencyMatrix);

// Print result
fw.printDistanceMatrix();
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Computer Networking (Routing Tables)
**Use Case:** Pre-computing routing tables for all source-destination pairs  
**Example:** Large enterprise networks with static routing requirements

### 2. Geographic Information Systems (GIS)
**Use Case:** Computing all-pairs distances for city maps  
**Example:** GPS systems computing distances between all points of interest

### 3. Social Network Analysis
**Use Case:** Computing shortest connection paths between all users  
**Example:** LinkedIn "degrees of separation" features

### 4. Transitive Closure Problems
**Use Case:** Determining reachability between all pairs  
**Example:** Database query optimization for recursive relationships

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| OSPF Protocol | Link-state routing |
| Google Maps | All-pairs distance queries |
| LinkedIn | Connection path analysis |
| Oracle Database | Query plan optimization |
| Traffic Systems | All-pairs travel times |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Floyd-Warshall | Dijkstra (V times) | Johnson's | Bellman-Ford (V times) |
|--------|----------------|-------------------|-----------|----------------------|
| Time Complexity | $O(V^3)$ | $O(V^3)$ or $O(VE \log V)$ | $O(VE + V^2 \log V)$ | $O(V^2 E)$ |
| Space Complexity | $O(V^2)$ | $O(V)$ per run | $O(V^2)$ | $O(V)$ per run |
| Negative Weights | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes |
| Best For | Dense graphs | Sparse, non-negative | Sparse, negative | Negative cycle check |
| Implementation | Simple | Moderate | Complex | Simple |

### When to Choose Floyd-Warshall

✅ **Use Floyd-Warshall when:**
- Need all-pairs shortest paths
- Graph is dense ($E \approx V^2$)
- Graph has negative edge weights
- Simplicity is preferred over optimal performance

❌ **Don't use when:**
- Only need single-source shortest paths
- Graph is very sparse
- Graph is very large ($V > 1000$)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Integer Overflow:** When adding two large values approaching infinity
2. **Self-Loop Handling:** Ensure diagonal is initialized to 0
3. **Infinity Representation:** Choose value that won't cause overflow when added

### Overflow Prevention Pattern

```java
// Safe addition check
if (dist[i][k] != INFINITY && dist[k][j] != INFINITY) {
    if (dist[i][k] + dist[k][j] < dist[i][j]) {
        dist[i][j] = dist[i][k] + dist[k][j];
    }
}
```

### Edge Cases to Handle

- [x] Self-loops (diagonal elements)
- [x] Disconnected components (remain at INFINITY)
- [x] Negative edge weights
- [x] Negative cycles (detected via negative diagonal)
- [x] Single vertex graph

---

## 📖 References

1. Floyd, R. W. (1962). "Algorithm 97: Shortest Path". *Communications of the ACM*. 5 (6): 345.
2. Warshall, S. (1962). "A theorem on Boolean matrices". *Journal of the ACM*. 9 (1): 11–12.
3. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 25.
4. [Wikipedia: Floyd-Warshall algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm)

---

## 🔗 Related Algorithms

- [Dijkstra's Algorithm](dijkstra.md) - Single-source, non-negative weights
- [Bellman-Ford Algorithm](bellman-ford.md) - Single-source, negative weights
- [Johnson's Algorithm](johnsons-algorithm.md) - All-pairs using reweighting
- [Transitive Closure](../../../06-backtracking/README.md) - Reachability queries
