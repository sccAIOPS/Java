# Dijkstra's Algorithm

> **Category:** Graph Algorithms  
> **Subcategory:** Shortest Path  
> **Implementation:** [DijkstraAlgorithm.java](../../../src/main/java/com/thealgorithms/datastructures/graphs/DijkstraAlgorithm.java)

---

## 📚 Overview

Dijkstra's algorithm is a **greedy algorithm** that solves the single-source shortest path problem for a weighted graph with **non-negative edge weights**. It finds the shortest path from a source vertex to all other vertices in the graph.

Named after Dutch computer scientist Edsger W. Dijkstra, who conceived it in 1956 and published it in 1959, this algorithm is one of the most fundamental graph algorithms in computer science.

---

## 🔢 Mathematical Foundation

### Definition

Given a weighted graph $G = (V, E)$ with non-negative edge weights $w: E \rightarrow \mathbb{R}^+$, Dijkstra's algorithm finds the shortest path distance $d(s, v)$ from source vertex $s$ to every vertex $v \in V$.

### Key Properties

- **Greedy Choice:** Always selects the vertex with minimum distance that hasn't been processed
- **Optimal Substructure:** Shortest path to any vertex contains shortest paths to intermediate vertices
- **Non-negative Weights Required:** Does not work correctly with negative edge weights

### Mathematical Formulation

The algorithm maintains a distance array $d[]$ where $d[v]$ represents the current best known distance from source $s$ to vertex $v$.

**Relaxation Operation:**
$$
d[v] = \min(d[v], d[u] + w(u, v))
$$

**Invariant:** After processing vertex $u$, $d[u]$ contains the shortest path distance from $s$ to $u$.

#### Proof of Correctness (Sketch)

The algorithm's correctness relies on the **greedy choice property**:
- When a vertex $u$ is selected (having minimum $d[u]$ among unprocessed vertices), $d[u]$ is already optimal
- This is because any alternative path to $u$ would go through an unprocessed vertex with distance ≥ $d[u]$

---

## 📊 Complexity Analysis

| Metric | Best Case | Average Case | Worst Case |
|--------|-----------|--------------|------------|
| **Time** | $O(V^2)$ | $O(V^2)$ | $O(V^2)$ |
| **Space** | $O(V)$ | $O(V)$ | $O(V)$ |

### Implementation Variants

| Implementation | Time Complexity | Best For |
|---------------|-----------------|----------|
| Adjacency Matrix | $O(V^2)$ | Dense graphs |
| Binary Heap + Adj List | $O((V+E) \log V)$ | Sparse graphs |
| Fibonacci Heap + Adj List | $O(E + V \log V)$ | Very large sparse graphs |

### Detailed Analysis

**Why $O(V^2)$ for matrix representation:**
- Outer loop runs $V-1$ times
- Finding minimum distance vertex: $O(V)$ per iteration
- Relaxation of all edges from current vertex: $O(V)$ per iteration
- Total: $O(V) \times O(V) = O(V^2)$

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Dijkstra(G, source)
    INPUT: Graph G with V vertices and non-negative edge weights, source vertex
    OUTPUT: Array dist[] where dist[v] = shortest distance from source to v
    
    1. FOR each vertex v in G DO
    2.     dist[v] ← ∞
    3.     processed[v] ← false
    4. END FOR
    5. dist[source] ← 0
    
    6. FOR count ← 0 TO V-1 DO
    7.     u ← vertex with minimum dist[u] among unprocessed vertices
    8.     processed[u] ← true
    
    9.     FOR each neighbor v of u DO
    10.        IF NOT processed[v] AND dist[u] + weight(u,v) < dist[v] THEN
    11.            dist[v] ← dist[u] + weight(u,v)
    12.        END IF
    13.    END FOR
    14. END FOR
    
    15. RETURN dist[]
```

### Step-by-Step Walkthrough

Consider the following graph with source = 0:

```
        10
    0 ------> 1
    |         |
  5 |         | 1
    v         v
    2 ------> 3
        2
```

| Step | Current | dist[0] | dist[1] | dist[2] | dist[3] | Processed |
|------|---------|---------|---------|---------|---------|-----------|
| Init | - | 0 | ∞ | ∞ | ∞ | {} |
| 1 | 0 | 0 | 10 | 5 | ∞ | {0} |
| 2 | 2 | 0 | 10 | 5 | 7 | {0,2} |
| 3 | 3 | 0 | 10 | 5 | 7 | {0,2,3} |
| 4 | 1 | 0 | 10 | 5 | 7 | {0,2,3,1} |

**Final Result:** Shortest distances from vertex 0: [0, 10, 5, 7]

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Uses adjacency matrix representation for graph storage
- Maintains `distances[]` array for shortest path distances
- Maintains `processed[]` boolean array to track visited vertices
- Returns distances array after computing all shortest paths

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/graphs/DijkstraAlgorithm.java`

```java
public int[] run(int[][] graph, int source) {
    int[] distances = new int[vertexCount];
    boolean[] processed = new boolean[vertexCount];
    
    Arrays.fill(distances, Integer.MAX_VALUE);
    distances[source] = 0;
    
    for (int count = 0; count < vertexCount - 1; count++) {
        int u = getMinDistanceVertex(distances, processed);
        processed[u] = true;
        
        for (int v = 0; v < vertexCount; v++) {
            if (!processed[v] && graph[u][v] != 0 
                && distances[u] != Integer.MAX_VALUE 
                && distances[u] + graph[u][v] < distances[v]) {
                distances[v] = distances[u] + graph[u][v];
            }
        }
    }
    return distances;
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. GPS Navigation Systems
**Use Case:** Finding shortest route between two locations  
**Example:** Google Maps uses variants of Dijkstra's algorithm with road network weights based on distance, traffic, and road conditions

### 2. Network Routing Protocols
**Use Case:** Routing packets through computer networks  
**Example:** OSPF (Open Shortest Path First) protocol uses Dijkstra's algorithm to compute routing tables in IP networks

### 3. Social Network Analysis
**Use Case:** Finding shortest connection path between users (degrees of separation)  
**Example:** LinkedIn's "How you're connected" feature

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Google Maps/Waze | Real-time navigation with traffic-aware routing |
| Cisco/Juniper | OSPF routing protocol implementation |
| Uber/Lyft | Driver-rider matching based on proximity |
| Amazon | Warehouse robot path planning |
| Telecom Networks | Call routing optimization |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Dijkstra | Bellman-Ford | Floyd-Warshall | A* |
|--------|----------|--------------|----------------|-----|
| Time Complexity | $O(V^2)$ or $O(E \log V)$ | $O(VE)$ | $O(V^3)$ | $O(E)$ (with good heuristic) |
| Negative Weights | ❌ No | ✅ Yes | ✅ Yes | ❌ No |
| Single Source | ✅ Yes | ✅ Yes | ❌ No (all pairs) | ✅ Yes |
| Heuristic | ❌ No | ❌ No | ❌ No | ✅ Yes |
| Best For | Non-negative weights | Negative edges | All-pairs | Point-to-point with heuristic |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Negative Edge Weights:** Algorithm fails silently with negative weights; use Bellman-Ford instead
2. **Integer Overflow:** When adding distances, check for overflow before comparison
3. **Disconnected Graphs:** Some vertices may remain with distance = ∞

### Edge Cases to Handle

- [x] Empty graph (no vertices)
- [x] Single vertex graph
- [x] Disconnected components (unreachable vertices)
- [x] Self-loops (edges from vertex to itself)
- [x] Parallel edges (multiple edges between same vertices)

---

## 📖 References

1. Dijkstra, E. W. (1959). "A note on two problems in connexion with graphs". *Numerische Mathematik*. 1: 269–271.
2. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 24.
3. [Wikipedia: Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

---

## 🔗 Related Algorithms

- [Bellman-Ford Algorithm](bellman-ford.md) - Handles negative weights
- [Floyd-Warshall Algorithm](floyd-warshall.md) - All-pairs shortest paths
- [A* Search Algorithm](../pathfinding/a-star.md) - Heuristic-based pathfinding
- [BFS](../../02-searching-algorithms/bfs.md) - Unweighted shortest paths
