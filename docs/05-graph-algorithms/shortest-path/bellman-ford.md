# Bellman-Ford Algorithm

> **Category:** Graph Algorithms  
> **Subcategory:** Shortest Path  
> **Implementation:** [BellmanFord.java](../../../src/main/java/com/thealgorithms/datastructures/graphs/BellmanFord.java)

---

## 📚 Overview

The Bellman-Ford algorithm computes the shortest paths from a single source vertex to all other vertices in a weighted digraph. Unlike Dijkstra's algorithm, **Bellman-Ford can handle graphs with negative edge weights** and can detect negative-weight cycles.

Named after Richard Bellman and Lester Ford Jr., this algorithm is particularly useful in network routing protocols where link costs can be negative or need cycle detection.

---

## 🔢 Mathematical Foundation

### Definition

Given a weighted directed graph $G = (V, E)$ with edge weights $w: E \rightarrow \mathbb{R}$ (including negative weights), the Bellman-Ford algorithm finds shortest path distances $d(s, v)$ from source $s$ to all vertices $v \in V$, or reports that a negative-weight cycle exists.

### Key Properties

- **Handles Negative Weights:** Can process edges with negative weights
- **Negative Cycle Detection:** Detects if a negative-weight cycle exists
- **Dynamic Programming:** Uses iterative relaxation approach
- **Path Reconstruction:** Can reconstruct shortest paths using predecessor array

### Mathematical Formulation

**Relaxation Operation:**
$$
d[v] = \min(d[v], d[u] + w(u, v))
$$

**Negative Cycle Detection:**
After $V-1$ iterations, if any edge $(u, v)$ can still be relaxed:
$$
d[u] + w(u, v) < d[v] \Rightarrow \text{negative cycle exists}
$$

#### Proof of Correctness

**Lemma:** After $i$ iterations, $d[v]$ contains the shortest path using at most $i$ edges.

**Theorem:** A shortest path contains at most $V-1$ edges. Therefore, after $V-1$ iterations, all shortest paths are found (if no negative cycle exists).

---

## 📊 Complexity Analysis

| Metric | Best Case | Average Case | Worst Case |
|--------|-----------|--------------|------------|
| **Time** | $O(VE)$ | $O(VE)$ | $O(VE)$ |
| **Space** | $O(V)$ | $O(V)$ | $O(V)$ |

### Detailed Analysis

- **Outer loop:** $V-1$ iterations
- **Inner loop:** Relax all $E$ edges in each iteration
- **Total operations:** $(V-1) \times E = O(VE)$

**Space Complexity:**
- Distance array: $O(V)$
- Predecessor array: $O(V)$
- Edge list: $O(E)$ (input)

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM BellmanFord(G, source)
    INPUT: Graph G with V vertices and E edges, source vertex
    OUTPUT: dist[] array, or "Negative Cycle" if one exists
    
    1. FOR each vertex v in G DO
    2.     dist[v] ← ∞
    3.     predecessor[v] ← null
    4. END FOR
    5. dist[source] ← 0
    
    // Main relaxation loop
    6. FOR i ← 1 TO V-1 DO
    7.     FOR each edge (u, v) with weight w DO
    8.         IF dist[u] ≠ ∞ AND dist[u] + w < dist[v] THEN
    9.             dist[v] ← dist[u] + w
    10.            predecessor[v] ← u
    11.        END IF
    12.    END FOR
    13. END FOR
    
    // Negative cycle detection
    14. FOR each edge (u, v) with weight w DO
    15.     IF dist[u] ≠ ∞ AND dist[u] + w < dist[v] THEN
    16.         RETURN "Negative Cycle Detected"
    17.     END IF
    18. END FOR
    
    19. RETURN dist[], predecessor[]
```

### Step-by-Step Walkthrough

Consider graph with source = 0:
```
    0 ---(6)---> 1
    |            |
  (7)|         (-3)|
    v            v
    2 ---(9)---> 3
```

Edges: (0,1,6), (0,2,7), (1,3,-3), (2,3,9)

| Iteration | Edge | Action | dist[] |
|-----------|------|--------|--------|
| Init | - | Initialize | [0, ∞, ∞, ∞] |
| 1 | (0,1,6) | Relax | [0, 6, ∞, ∞] |
| 1 | (0,2,7) | Relax | [0, 6, 7, ∞] |
| 1 | (1,3,-3) | Relax | [0, 6, 7, 3] |
| 1 | (2,3,9) | No change | [0, 6, 7, 3] |
| 2 | All edges | No changes | [0, 6, 7, 3] |
| 3 | All edges | No changes | [0, 6, 7, 3] |
| Check | All edges | No negative cycle | ✓ |

**Final Result:** [0, 6, 7, 3]

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Uses Edge class to represent graph edges (source, destination, weight)
- Supports both interactive input and programmatic edge addition
- Includes path reconstruction using predecessor array
- Negative cycle detection with clear error reporting

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/graphs/BellmanFord.java`

```java
// Core relaxation loop
for (i = 0; i < v - 1; i++) {
    for (j = 0; j < e; j++) {
        if (dist[arr[j].u] != Integer.MAX_VALUE 
            && dist[arr[j].v] > dist[arr[j].u] + arr[j].w) {
            dist[arr[j].v] = dist[arr[j].u] + arr[j].w;
            p[arr[j].v] = arr[j].u;
        }
    }
}

// Negative cycle detection
for (j = 0; j < e; j++) {
    if (dist[arr[j].u] != Integer.MAX_VALUE 
        && dist[arr[j].v] > dist[arr[j].u] + arr[j].w) {
        System.out.println("Negative cycle");
        break;
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Routing Information Protocol (RIP)
**Use Case:** Distance-vector routing in computer networks  
**Example:** Cisco routers use Bellman-Ford for RIP implementation

### 2. Currency Arbitrage Detection
**Use Case:** Finding profitable currency exchange cycles  
**Example:** If USD→EUR→GBP→USD yields more than original amount, arbitrage exists (negative cycle)

### 3. Network Flow Analysis
**Use Case:** Finding minimum cost flows with potential negative costs  
**Example:** Supply chain optimization with refund/return scenarios

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Cisco/Juniper | RIP routing protocol |
| Bloomberg Terminal | Currency arbitrage detection |
| AWS/Azure | Network cost optimization |
| Supply Chain Systems | Logistics with returns |
| Financial Trading | Arbitrage opportunity detection |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Bellman-Ford | Dijkstra | Floyd-Warshall | SPFA |
|--------|--------------|----------|----------------|------|
| Time Complexity | $O(VE)$ | $O(V^2)$ or $O(E \log V)$ | $O(V^3)$ | $O(VE)$ average |
| Negative Weights | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes |
| Negative Cycle Detection | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes |
| Single Source | ✅ Yes | ✅ Yes | ❌ No | ✅ Yes |
| Best For | Negative edges | Non-negative | All pairs | Sparse graphs |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Integer Overflow:** When $d[u] = \text{MAX\_VALUE}$, adding weight causes overflow
2. **Negative Cycle Handling:** Algorithm only detects cycles; doesn't specify which vertices are affected
3. **Unreachable Vertices:** Remain at $\infty$; shouldn't be relaxed

### Edge Cases to Handle

- [x] Graph with negative edges but no negative cycles
- [x] Graph with negative cycle (should detect and report)
- [x] Disconnected graph components
- [x] Single vertex graph
- [x] Empty edge set

---

## 📖 References

1. Bellman, R. (1958). "On a routing problem". *Quarterly of Applied Mathematics*. 16: 87–90.
2. Ford, L. R. Jr. (1956). *Network Flow Theory*. RAND Corporation Paper P-923.
3. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 24.
4. [Wikipedia: Bellman-Ford algorithm](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm)

---

## 🔗 Related Algorithms

- [Dijkstra's Algorithm](dijkstra.md) - Faster but no negative weights
- [Floyd-Warshall Algorithm](floyd-warshall.md) - All-pairs shortest paths
- [Johnson's Algorithm](johnsons-algorithm.md) - All-pairs with reweighting
- [SPFA](spfa.md) - Optimized Bellman-Ford variant
