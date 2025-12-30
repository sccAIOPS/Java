# Prim's Algorithm (Minimum Spanning Tree)

> **Category:** Graph Algorithms  
> **Subcategory:** Spanning Tree  
> **Implementation:** [PrimMST.java](../../../src/main/java/com/thealgorithms/datastructures/graphs/PrimMST.java)

---

## 📚 Overview

Prim's algorithm is a **greedy algorithm** that finds a minimum spanning tree (MST) for a weighted undirected graph. The MST is a subset of edges that connects all vertices together without cycles and with the minimum possible total edge weight.

Named after Robert C. Prim, who rediscovered this algorithm in 1957 (originally discovered by Vojtěch Jarník in 1930), Prim's algorithm builds the MST by starting from an arbitrary vertex and repeatedly adding the smallest edge that connects a vertex in the tree to a vertex outside.

---

## 🔢 Mathematical Foundation

### Definition

Given a connected, undirected graph $G = (V, E)$ with edge weights $w: E \rightarrow \mathbb{R}^+$, a **Minimum Spanning Tree (MST)** is a tree $T \subseteq E$ such that:
1. $T$ connects all vertices (spans $V$)
2. $|T| = |V| - 1$ (tree property)
3. $\sum_{e \in T} w(e)$ is minimized

### Key Properties

- **Greedy Choice:** Always selects the minimum weight edge crossing the cut
- **Cut Property:** For any cut, the minimum weight crossing edge is in some MST
- **Tree Invariant:** Maintains a tree at each step
- **Optimal Substructure:** MST contains MSTs of subgraphs

### Mathematical Formulation

**Cut Property Theorem:**
For any cut $(S, V \setminus S)$ in graph $G$, let $e$ be the minimum weight edge crossing the cut. Then $e$ belongs to some MST of $G$.

**Prim's Selection Criterion:**
$$
e^* = \arg\min_{e=(u,v)} \{w(e) : u \in T, v \notin T\}
$$

#### Proof of Correctness

By induction on the number of edges added:
- **Base case:** Single vertex is a valid partial MST
- **Inductive step:** If $T$ is part of some MST, adding minimum cut edge keeps $T$ in some MST (by cut property)

---

## 📊 Complexity Analysis

| Metric | Adjacency Matrix | Binary Heap | Fibonacci Heap |
|--------|------------------|-------------|----------------|
| **Time** | $O(V^2)$ | $O(E \log V)$ | $O(E + V \log V)$ |
| **Space** | $O(V^2)$ | $O(V)$ | $O(V)$ |

### Detailed Analysis

**With Adjacency Matrix (Current Implementation):**
- Find minimum key: $O(V)$ per iteration
- Total iterations: $V$
- Update neighbors: $O(V)$ per iteration
- **Total: $O(V^2)$**

**With Binary Heap:**
- Extract minimum: $O(\log V)$ per iteration
- Decrease key: $O(\log V)$ per edge
- **Total: $O(E \log V)$**

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM PrimMST(G, source)
    INPUT: Graph G with V vertices (adjacency matrix), source vertex
    OUTPUT: MST edges and total weight
    
    // Initialize
    1. FOR each vertex v in G DO
    2.     key[v] ← ∞
    3.     parent[v] ← NULL
    4.     inMST[v] ← FALSE
    5. END FOR
    6. key[source] ← 0
    
    // Build MST with V vertices
    7. FOR count ← 0 TO V-1 DO
    8.     // Find minimum key vertex not in MST
    9.     u ← vertex with min key[v] where inMST[v] = FALSE
    10.    inMST[u] ← TRUE
    
    11.    // Update keys of adjacent vertices
    12.    FOR each vertex v adjacent to u DO
    13.        IF inMST[v] = FALSE AND weight(u,v) < key[v] THEN
    14.            parent[v] ← u
    15.            key[v] ← weight(u, v)
    16.        END IF
    17.    END FOR
    18. END FOR
    
    19. RETURN parent[], key[]
```

### Step-by-Step Walkthrough

Consider this weighted undirected graph:
```
        (2)
    0 ─────── 1
    │\        │
  (6)│ \(8)   │(3)
    │  \      │
    2───\─────3
      (5) \  /(7)
           \/
           4
```

| Step | u | MST Vertices | Edges Added | Total Weight |
|------|---|--------------|-------------|--------------|
| Init | - | {} | - | 0 |
| 1 | 0 | {0} | - | 0 |
| 2 | 1 | {0,1} | (0,1) | 2 |
| 3 | 3 | {0,1,3} | (1,3) | 5 |
| 4 | 2 | {0,1,3,2} | (2,3) | 10 |
| 5 | 4 | {0,1,3,2,4} | (3,4) | 17 |

**Final MST edges:** (0,1), (1,3), (2,3), (3,4)  
**Total weight:** 2 + 3 + 5 + 7 = 17

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Uses adjacency matrix representation
- Fixed vertex count (V=5 in implementation)
- Includes `minKey()` helper function for finding minimum
- Outputs parent array representing MST

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/graphs/PrimMST.java`

```java
// Find minimum key vertex not in MST
int minKey(int[] key, boolean[] mstSet) {
    int min = Integer.MAX_VALUE;
    int minIndex = -1;
    
    for (int v = 0; v < V; v++) {
        if (!mstSet[v] && key[v] < min) {
            min = key[v];
            minIndex = v;
        }
    }
    return minIndex;
}

// Main Prim's algorithm
void primMST(int[][] graph) {
    int[] parent = new int[V];
    int[] key = new int[V];
    boolean[] mstSet = new boolean[V];
    
    // Initialize all keys to infinity
    Arrays.fill(key, Integer.MAX_VALUE);
    key[0] = 0;
    parent[0] = -1;
    
    for (int count = 0; count < V - 1; count++) {
        int u = minKey(key, mstSet);
        mstSet[u] = true;
        
        for (int v = 0; v < V; v++) {
            if (graph[u][v] != 0 && !mstSet[v] 
                && graph[u][v] < key[v]) {
                parent[v] = u;
                key[v] = graph[u][v];
            }
        }
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Network Design
**Use Case:** Designing minimum cost network topology  
**Example:** Connecting offices with minimum cable length

### 2. Cluster Analysis
**Use Case:** Single-linkage hierarchical clustering  
**Example:** Image segmentation, customer grouping

### 3. Circuit Design
**Use Case:** Minimizing wire length in VLSI design  
**Example:** PCB trace routing, chip design

### 4. Approximation Algorithms
**Use Case:** Base for TSP and Steiner tree approximations  
**Example:** Logistics route planning

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Cisco | Network topology design |
| Intel/AMD | VLSI circuit design |
| AWS/Azure | Data center connectivity |
| Telecommunications | Fiber optic network design |
| Airlines | Hub-and-spoke route networks |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Prim's | Kruskal's | Borůvka's |
|--------|--------|-----------|-----------|
| Approach | Vertex-based | Edge-based | Component-based |
| Time (Matrix) | $O(V^2)$ | $O(E \log E)$ | $O(E \log V)$ |
| Time (Heap) | $O(E \log V)$ | $O(E \log E)$ | $O(E \log V)$ |
| Best For | Dense graphs | Sparse graphs | Parallel execution |
| Data Structure | Priority Queue | Union-Find | Union-Find |
| Parallelizable | Limited | Limited | Highly |

### When to Choose Prim's Algorithm

✅ **Use Prim's when:**
- Graph is dense ($E \approx V^2$)
- Already have adjacency matrix representation
- Need to start from a specific vertex
- Working with graphs that fit in memory

❌ **Don't use when:**
- Graph is very sparse
- Need parallel/distributed computation
- Edge list is the primary representation

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Disconnected Graphs:** MST only exists for connected graphs
2. **Zero-Weight Edges:** Should be handled correctly
3. **Duplicate Edge Weights:** May result in multiple valid MSTs
4. **Integer Overflow:** Sum of weights may exceed integer range

### Edge Cases to Handle

- [x] Single vertex graph (MST is empty)
- [x] Graph with single edge
- [x] Graph with all equal edge weights
- [x] Complete graph (many choices, same result)
- [ ] Disconnected graph (should report no MST)

### Potential Improvements to Implementation

```java
// Check for disconnected graph
boolean isConnected() {
    // Perform DFS/BFS from source
    // Return true if all vertices reachable
}

// Allow arbitrary vertex count
public PrimMST(int vertices) {
    this.V = vertices;
}
```

---

## 📖 References

1. Prim, R. C. (1957). "Shortest connection networks and some generalizations". *Bell System Technical Journal*. 36 (6): 1389–1401.
2. Jarník, V. (1930). "O jistém problému minimálním" [About a certain minimal problem]. *Práce Moravské Přírodovědecké Společnosti*.
3. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 23.
4. [Wikipedia: Prim's algorithm](https://en.wikipedia.org/wiki/Prim%27s_algorithm)

---

## 🔗 Related Algorithms

- [Kruskal's Algorithm](kruskal.md) - Edge-based MST approach
- [Dijkstra's Algorithm](../shortest-path/dijkstra.md) - Similar structure, different purpose
- [Borůvka's Algorithm](boruvka.md) - Parallel MST algorithm
- [Steiner Tree](steiner-tree.md) - MST variant with required vertices
