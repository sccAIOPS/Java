# Kruskal's Algorithm (Minimum Spanning Tree)

> **Category:** Graph Algorithms  
> **Subcategory:** Spanning Tree  
> **Implementation:** [Kruskal.java](../../../src/main/java/com/thealgorithms/datastructures/graphs/Kruskal.java)

---

## 📚 Overview

Kruskal's algorithm is a **greedy algorithm** that finds a minimum spanning tree (MST) for a connected weighted graph. Unlike Prim's algorithm which grows a single tree, Kruskal's algorithm builds a forest of trees, eventually combining them into a single MST.

Named after Joseph Kruskal who published it in 1956, this algorithm processes edges in order of increasing weight, adding each edge to the MST if it doesn't create a cycle.

---

## 🔢 Mathematical Foundation

### Definition

Given a connected, undirected graph $G = (V, E)$ with edge weights $w: E \rightarrow \mathbb{R}^+$, Kruskal's algorithm finds an MST by:
1. Sorting all edges by weight
2. Adding edges that don't form cycles

### Key Properties

- **Edge-Centric:** Processes edges rather than vertices
- **Forest Approach:** Starts with $V$ trees, merges until one tree
- **Cut Property:** Selected edge is always minimum crossing some cut
- **Union-Find:** Uses disjoint sets for cycle detection

### Mathematical Formulation

**Greedy Selection:**
$$
e^* = \arg\min_{e \in E \setminus T} \{w(e) : e \text{ doesn't form cycle with } T\}
$$

**Cycle Detection:**
Edge $(u, v)$ forms a cycle with $T$ if and only if $u$ and $v$ are in the same connected component.

#### Proof of Correctness

**Theorem:** Kruskal's algorithm produces an MST.

**Proof:**
1. When edge $e = (u, v)$ is added, let $S$ be the component containing $u$
2. Edge $e$ is the minimum edge crossing cut $(S, V \setminus S)$
3. By cut property, $e$ is in some MST
4. Since we never create cycles, result is a spanning tree
5. By induction, all selected edges are in some MST

---

## 📊 Complexity Analysis

| Metric | Time Complexity | Notes |
|--------|-----------------|-------|
| **Sorting Edges** | $O(E \log E)$ | Dominates complexity |
| **Union-Find Operations** | $O(E \cdot \alpha(V))$ | Nearly $O(E)$ |
| **Total Time** | $O(E \log E)$ | Equivalent to $O(E \log V)$ |
| **Space** | $O(V + E)$ | Edge list + Union-Find |

### Detailed Analysis

- **Sorting:** $O(E \log E) = O(E \log V^2) = O(2E \log V) = O(E \log V)$
- **Union-Find:** $O(E \cdot \alpha(V))$ where $\alpha$ is inverse Ackermann function
- Since $\alpha(V) \leq 4$ for any practical $V$, Union-Find is effectively $O(E)$

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Kruskal(G)
    INPUT: Graph G with V vertices and E edges
    OUTPUT: MST edge set T
    
    // Initialize
    1. T ← ∅  // MST edge set
    2. FOR each vertex v in G DO
    3.     MakeSet(v)  // Create singleton set
    4. END FOR
    
    // Sort edges by weight
    5. SORT edges E by weight in ascending order
    
    // Process edges
    6. FOR each edge (u, v, w) in sorted E DO
    7.     IF Find(u) ≠ Find(v) THEN  // Different components
    8.         T ← T ∪ {(u, v, w)}
    9.         Union(u, v)
    10.    END IF
    11.    IF |T| = V - 1 THEN  // MST complete
    12.        BREAK
    13.    END IF
    14. END FOR
    
    15. RETURN T
```

### Step-by-Step Walkthrough

Consider this weighted undirected graph:
```
        (4)
    A ─────── B
    │\       /│
  (8)│ \(11)/ │(8)
    │  \  /   │
    H───X────C
   (7) (1) (7)│
    │  / \   │(4)
  (6)│/(2) \│
    G ─────── F─(14)─E
        (9)  │(10)
             D
```

**Sorted Edges:** (1), (2), (4), (4), (6), (7), (7), (8), (8), (9), (10), (11), (14)

| Step | Edge | Weight | Action | Forest State |
|------|------|--------|--------|--------------|
| 1 | X-C | 1 | Add | {A}, {B}, {X,C}, ... |
| 2 | G-F | 2 | Add | {A}, {B}, {X,C}, {G,F}, ... |
| 3 | A-B | 4 | Add | {A,B}, {X,C}, {G,F}, ... |
| 4 | C-F | 4 | Add | {A,B}, {X,C,G,F}, ... |
| 5 | G-H | 6 | Add | {A,B}, {X,C,G,F,H}, ... |
| 6 | A-H | 7 | Add | {A,B,X,C,G,F,H}, ... |
| 7 | C-D | 7 | Add | {A,B,X,C,G,F,H,D}, ... |
| 8 | D-E | 10 | Add | All connected |

**MST Edges:** (X,C), (G,F), (A,B), (C,F), (G,H), (A,H), (C,D), (D,E)  
**Total Weight:** 1 + 2 + 4 + 4 + 6 + 7 + 7 + 10 = 41

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Uses `PriorityQueue` for edge sorting (min-heap)
- Inner `Edge` class implements `Comparable` for weight comparison
- Union-Find with path compression via `HashSet` membership

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/graphs/Kruskal.java`

```java
// Edge class with Comparable interface
private static class Edge implements Comparable<Edge> {
    int from, to, weight;
    
    @Override
    public int compareTo(Edge other) {
        return Integer.compare(this.weight, other.weight);
    }
}

// Main Kruskal's algorithm
public void kruskal(int[][] graph) {
    PriorityQueue<Edge> edges = new PriorityQueue<>();
    
    // Add all edges to priority queue
    for (int i = 0; i < V; i++) {
        for (int j = i + 1; j < V; j++) {
            if (graph[i][j] != 0) {
                edges.add(new Edge(i, j, graph[i][j]));
            }
        }
    }
    
    HashSet<Integer> visited = new HashSet<>();
    ArrayList<Edge> mst = new ArrayList<>();
    
    while (!edges.isEmpty() && mst.size() < V - 1) {
        Edge e = edges.poll();
        // Check if edge creates cycle
        // Add to MST if not
    }
}
```

### Union-Find Optimization

The current implementation can be improved with proper Union-Find:

```java
class UnionFind {
    int[] parent, rank;
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    void union(int x, int y) {
        int px = find(x), py = find(y);
        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Network Infrastructure Design
**Use Case:** Minimum cost cable/fiber optic layout  
**Example:** Connecting multiple buildings with minimum wiring

### 2. Clustering Algorithms
**Use Case:** K-clustering with maximum spacing  
**Example:** Stop Kruskal after $V - k$ edges for $k$ clusters

### 3. Image Segmentation
**Use Case:** Graph-based image segmentation  
**Example:** Felzenszwalb's algorithm uses Kruskal's for efficient merging

### 4. Maze Generation
**Use Case:** Generate random spanning trees for mazes  
**Example:** Randomized Kruskal's creates interesting maze patterns

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Telecommunications | Cable network design |
| Electric Utilities | Power grid optimization |
| Computer Vision | Image segmentation |
| Game Development | Procedural maze generation |
| Social Networks | Community detection |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | Kruskal's | Prim's | Borůvka's |
|--------|-----------|--------|-----------|
| Strategy | Edge sorting | Vertex growing | Component merging |
| Time (Matrix) | $O(E \log E)$ | $O(V^2)$ | $O(E \log V)$ |
| Time (Heap) | $O(E \log E)$ | $O(E \log V)$ | $O(E \log V)$ |
| Best For | Sparse graphs | Dense graphs | Parallel execution |
| Data Structure | Union-Find | Priority Queue | Union-Find |
| Edge Processing | Global sort | Local selection | Parallel selection |

### When to Choose Kruskal's Algorithm

✅ **Use Kruskal's when:**
- Graph is sparse ($E \ll V^2$)
- Edges are already sorted or in a stream
- Need to find MST progressively
- Using edge list representation

❌ **Don't use when:**
- Graph is very dense
- Need to grow MST from specific vertex
- Memory for edge sorting is limited

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Disconnected Graphs:** Algorithm produces MSF (forest) not MST
2. **Duplicate Edge Weights:** Multiple valid MSTs may exist
3. **Self-Loops:** Should be filtered out (add no value)
4. **Parallel Edges:** Keep only minimum weight edge

### Edge Cases to Handle

- [x] Single vertex graph (empty MST)
- [x] Two vertices, one edge
- [x] Graph with equal weight edges
- [x] Complete graph (many edges to process)
- [ ] Disconnected components (returns forest)

### Common Implementation Bug

```java
// BUG: Not checking if vertices already in same component
if (!visited.contains(edge.from) || !visited.contains(edge.to)) {
    mst.add(edge);  // This doesn't correctly detect cycles!
}

// CORRECT: Use Union-Find
if (find(edge.from) != find(edge.to)) {
    mst.add(edge);
    union(edge.from, edge.to);
}
```

---

## 📖 References

1. Kruskal, J. B. (1956). "On the shortest spanning subtree of a graph and the traveling salesman problem". *Proceedings of the American Mathematical Society*. 7 (1): 48–50.
2. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press. Chapter 23.
3. Tarjan, R. E. (1975). "Efficiency of a Good But Not Linear Set Union Algorithm". *Journal of the ACM*. 22 (2): 215–225.
4. [Wikipedia: Kruskal's algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm)

---

## 🔗 Related Algorithms

- [Prim's Algorithm](prim.md) - Vertex-based MST approach
- [Union-Find](../../04-data-structures/trees/union-find.md) - Cycle detection data structure
- [Borůvka's Algorithm](boruvka.md) - Parallel MST algorithm
- [Reverse-Delete Algorithm](reverse-delete.md) - Opposite approach to Kruskal's
