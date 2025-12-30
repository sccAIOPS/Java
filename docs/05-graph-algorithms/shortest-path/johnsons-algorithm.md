# Johnson's Algorithm

> **Category:** Graph Algorithms  
> **Subcategory:** Shortest Path (All-Pairs)  
> **Implementation:** [JohnsonsAlgorithm.java](../../../src/main/java/com/thealgorithms/datastructures/graphs/JohnsonsAlgorithm.java)

---

## 📚 Overview

**Johnson's algorithm** finds the shortest paths between **all pairs of vertices** in a weighted, directed graph that may contain **negative edge weights** (but no negative cycles). It combines the best of Bellman-Ford and Dijkstra's algorithms.

The algorithm works by first using Bellman-Ford to reweight the graph to eliminate negative edges, then running Dijkstra's algorithm from each vertex. This approach is more efficient than Floyd-Warshall for sparse graphs.

**Key Innovation:** Graph reweighting using a potential function $h(v)$ that transforms negative weights into non-negative ones while preserving shortest paths.

---

## 🔢 Mathematical Foundation

### Definition

Given a weighted directed graph $G = (V, E)$ with weight function $w: E \rightarrow \mathbb{R}$ (potentially negative), Johnson's algorithm computes the shortest path distance $\delta(u, v)$ between every pair of vertices $u, v \in V$.

### Key Concepts

#### Graph Reweighting

For a vertex potential function $h: V \rightarrow \mathbb{R}$, define the new weight function:

$$\hat{w}(u, v) = w(u, v) + h(u) - h(v)$$

**Property:** This reweighting preserves shortest paths. For any path $p$ from $u$ to $v$:

$$\hat{w}(p) = w(p) + h(u) - h(v)$$

Since $h(u) - h(v)$ is constant for all paths from $u$ to $v$, the shortest path in the original graph remains shortest in the reweighted graph.

#### Computing the Potential Function

Add a new vertex $s$ connected to all vertices with zero-weight edges, then run Bellman-Ford:

$$h(v) = \delta(s, v)$$

**Theorem:** If $G$ has no negative cycles, then:
$$\hat{w}(u, v) = w(u, v) + h(u) - h(v) \geq 0$$

**Proof:** By the shortest path property, $h(v) \leq h(u) + w(u, v)$, which rearranges to $w(u, v) + h(u) - h(v) \geq 0$.

### Algorithm Steps

1. **Add source vertex:** Create new vertex $s$ with edges $(s, v, 0)$ for all $v \in V$
2. **Run Bellman-Ford:** Compute $h(v) = \delta(s, v)$ for all $v$
3. **Reweight edges:** $\hat{w}(u, v) = w(u, v) + h(u) - h(v)$
4. **Run Dijkstra V times:** From each vertex on the reweighted graph
5. **Adjust results:** $\delta(u, v) = \hat{\delta}(u, v) - h(u) + h(v)$

---

## 📊 Complexity Analysis

| Metric | Complexity | Notes |
|--------|------------|-------|
| **Time** | $O(V^2 \log V + VE)$ | With binary heap Dijkstra |
| **Time** | $O(VE + V^2 \log V)$ | Alternative formulation |
| **Space** | $O(V^2)$ | For distance matrix |

### Detailed Breakdown

| Phase | Time Complexity |
|-------|-----------------|
| Bellman-Ford | $O(VE)$ |
| Graph Reweighting | $O(E)$ |
| Dijkstra × V | $O(V(V \log V + E))$ |
| Result Adjustment | $O(V^2)$ |
| **Total** | $O(V^2 \log V + VE)$ |

### Comparison with Other All-Pairs Algorithms

| Algorithm | Time | Space | Negative Weights |
|-----------|------|-------|------------------|
| Johnson's | $O(V^2 \log V + VE)$ | $O(V^2)$ | ✅ Yes |
| Floyd-Warshall | $O(V^3)$ | $O(V^2)$ | ✅ Yes |
| Dijkstra × V | $O(V(V^2))$ or $O(V(V \log V + E))$ | $O(V^2)$ | ❌ No |

**When Johnson's is Better:**
- Sparse graphs where $E = O(V)$: Johnson's is $O(V^2 \log V)$ vs Floyd-Warshall's $O(V^3)$
- When negative edges exist (can't use repeated Dijkstra directly)

**When Floyd-Warshall is Better:**
- Dense graphs where $E = O(V^2)$: Both are $O(V^3)$, but Floyd-Warshall has simpler implementation
- When only matrix representation is needed

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM JohnsonsAlgorithm(G, w)
    INPUT: Directed graph G = (V, E) with weight function w
    OUTPUT: |V| × |V| matrix D where D[u][v] = δ(u, v)
    
    // Step 1: Add new source vertex
    1. V' ← V ∪ {s}
    2. E' ← E ∪ {(s, v) : v ∈ V}
    3. w(s, v) ← 0 for all v ∈ V
    
    // Step 2: Run Bellman-Ford to get potentials
    4. (success, h) ← BellmanFord(G', w, s)
    5. IF NOT success THEN
    6.     RETURN "Negative cycle detected"
    7. END IF
    
    // Step 3: Reweight edges
    8. FOR each edge (u, v) ∈ E DO
    9.     ŵ(u, v) ← w(u, v) + h(u) - h(v)
    10. END FOR
    
    // Step 4: Run Dijkstra from each vertex
    11. FOR each vertex u ∈ V DO
    12.     D̂[u] ← Dijkstra(G, ŵ, u)
    13. END FOR
    
    // Step 5: Adjust distances back
    14. FOR each pair u, v ∈ V DO
    15.     D[u][v] ← D̂[u][v] - h(u) + h(v)
    16. END FOR
    
    17. RETURN D
```

### Step-by-Step Walkthrough

**Example Graph:**
```
    1
  (2)↗ ↘(-3)
  0      2
  (4)↘ ↗(-1)
    3
```

Edges: (0,1,2), (1,2,-3), (0,3,4), (3,2,-1)

**Step 1: Add source vertex s**
```
s → 0 (weight 0)
s → 1 (weight 0)
s → 2 (weight 0)
s → 3 (weight 0)
```

**Step 2: Bellman-Ford from s**
```
Iteration 0: h = [0, 0, 0, 0]  (from s)
Iteration 1: h = [0, 0, -3, 0] (via 0→1→2)
Iteration 2: h = [0, 0, -3, 0] (no change)
```

**Step 3: Reweight**
```
Original → Reweighted:
(0,1,2)  → 2 + 0 - 0 = 2
(1,2,-3) → -3 + 0 - (-3) = 0
(0,3,4)  → 4 + 0 - 0 = 4
(3,2,-1) → -1 + 0 - (-3) = 2
```

All weights are now non-negative! ✓

**Step 4: Run Dijkstra from each vertex**

**Step 5: Adjust back**
```
D[u][v] = D̂[u][v] - h[u] + h[v]
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

The implementation in `JohnsonsAlgorithm.java`:
- Uses adjacency matrix representation
- Converts to edge list for Bellman-Ford
- Implements all phases of the algorithm
- Throws exception on negative cycle detection

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/graphs/JohnsonsAlgorithm.java`

```java
public static double[][] johnsonAlgorithm(double[][] graph) {
    int numVertices = graph.length;
    double[][] edges = convertToEdgeList(graph);
    
    // Step 1-2: Bellman-Ford to compute potentials
    double[] modifiedWeights = bellmanFord(edges, numVertices);
    
    // Step 3: Reweight the graph
    double[][] reweightedGraph = reweightGraph(graph, modifiedWeights);
    
    // Step 4-5: Dijkstra from each source, adjusted
    double[][] shortestDistances = new double[numVertices][numVertices];
    for (int source = 0; source < numVertices; source++) {
        shortestDistances[source] = dijkstra(reweightedGraph, source, modifiedWeights);
    }
    
    return shortestDistances;
}
```

**Reweighting Implementation:**

```java
public static double[][] reweightGraph(double[][] graph, double[] modifiedWeights) {
    int numVertices = graph.length;
    double[][] reweightedGraph = new double[numVertices][numVertices];
    
    for (int i = 0; i < numVertices; i++) {
        for (int j = 0; j < numVertices; j++) {
            if (graph[i][j] != 0) {
                // New weight = original weight + h(u) - h(v)
                reweightedGraph[i][j] = graph[i][j] + modifiedWeights[i] - modifiedWeights[j];
            }
        }
    }
    return reweightedGraph;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Network Analysis and Routing
**Use Case:** Finding all shortest paths in networks with varying link costs  
**Example:** Telecommunications networks where some links have cost penalties (negative in optimization terms)

### 2. Transportation Logistics
**Use Case:** Computing optimal routes between all distribution centers  
**Example:** Supply chain optimization with incentive-based routing (negative costs for preferred routes)

### 3. Arbitrage Detection in Finance
**Use Case:** Finding profitable cycles in currency exchange networks  
**Example:** After detecting no negative cycles, compute all arbitrage-free exchange rates

### 4. Game AI Pathfinding
**Use Case:** Precomputing all-pairs distances for NPC navigation  
**Example:** Strategy games needing distance matrices for AI decision making

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Network Operators | All-pairs latency computation |
| Logistics Companies | Distance matrix for vehicle routing |
| Financial Institutions | Exchange rate path analysis |
| Game Studios | Precomputed pathfinding tables |
| Social Networks | Shortest path distance metrics |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Johnson's | Floyd-Warshall | Repeated Dijkstra |
|--------|-----------|----------------|-------------------|
| **Time** | $O(V^2 \log V + VE)$ | $O(V^3)$ | $O(V(V^2))$ or $O(V(V \log V + E))$ |
| **Best For** | Sparse + negative | Dense graphs | Sparse + non-negative |
| **Negative Weights** | ✅ Yes | ✅ Yes | ❌ No |
| **Negative Cycles** | Detects | Detects | N/A |
| **Implementation** | Complex | Simple | Simple |
| **Memory Access** | Irregular | Sequential | Irregular |

### When to Use Each

| Scenario | Best Algorithm |
|----------|---------------|
| Sparse graph, negative weights | **Johnson's** |
| Dense graph, any weights | Floyd-Warshall |
| Sparse graph, non-negative | Dijkstra × V |
| Need only one source | Dijkstra or Bellman-Ford |
| Simple implementation preferred | Floyd-Warshall |

---

## ⚠️ Common Pitfalls & Edge Cases

### 1. Negative Cycle Detection
```java
// The algorithm will throw exception if negative cycle exists
try {
    double[][] result = JohnsonsAlgorithm.johnsonAlgorithm(graph);
} catch (IllegalArgumentException e) {
    System.out.println("Graph contains negative cycle");
}
```

### 2. Integer Overflow in Reweighting
```java
// WRONG: May overflow when adding potentials
int newWeight = weight + h[u] - h[v];

// RIGHT: Use larger type or check bounds
double newWeight = weight + h[u] - h[v];
```

### 3. Sparse vs Dense Graph Choice
```
If E ≈ V²: Use Floyd-Warshall (simpler, same complexity)
If E ≈ V:  Use Johnson's (O(V² log V) vs O(V³))
```

### Edge Cases to Handle

- [x] Graph with no edges (identity matrix result)
- [x] Single vertex graph
- [x] Negative cycle (should throw/report)
- [x] Disconnected components (∞ distances)
- [x] Self-loops (should be handled)
- [x] Parallel edges (use minimum weight)

---

## 📖 References

1. Johnson, D. B. (1977). "Efficient Algorithms for Shortest Paths in Sparse Networks". *Journal of the ACM*. 24 (1): 1–13.
2. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 25.4.
3. [Wikipedia: Johnson's algorithm](https://en.wikipedia.org/wiki/Johnson%27s_algorithm)

---

## 🔗 Related Algorithms

- [Dijkstra's Algorithm](dijkstra.md) - Single-source, non-negative weights
- [Bellman-Ford Algorithm](bellman-ford.md) - Single-source, handles negative weights
- [Floyd-Warshall Algorithm](floyd-warshall.md) - All-pairs, simpler but slower for sparse graphs

---

*Last updated: Phase 3 Documentation*
