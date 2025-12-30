# M-Coloring Problem

> **Category:** Algorithms > Backtracking > Graph Coloring
> **Implementation:** [MColoring.java](../../../src/main/java/com/thealgorithms/backtracking/MColoring.java)

## Overview

The **M-Coloring Problem** determines whether a graph can be colored using at most M colors such that no two adjacent vertices share the same color. This is a fundamental problem in graph theory with applications in scheduling, register allocation, and map coloring.

### Problem Definition

Given:
- An undirected graph G = (V, E)
- A positive integer M (number of available colors)

Determine:
- Can we assign colors 1 to M to each vertex such that no edge connects two vertices of the same color?

## Mathematical Foundation

### Graph Coloring Terminology

**Chromatic Number χ(G):** The minimum number of colors needed to color graph G.

**k-colorable:** A graph is k-colorable if χ(G) ≤ k.

### Constraint Formulation

For vertices u and v connected by edge (u, v):
$$color(u) \neq color(v)$$

### Special Cases

| Graph Type | Chromatic Number |
|------------|------------------|
| Empty graph (no edges) | 1 |
| Complete graph K_n | n |
| Bipartite graph | 2 |
| Cycle (even vertices) | 2 |
| Cycle (odd vertices) | 3 |
| Planar graph | ≤ 4 (Four Color Theorem) |

### NP-Completeness

- Decision problem: "Is G k-colorable?" is NP-complete for k ≥ 3
- k = 2 (bipartite check) is polynomial: O(V + E)

## Complexity Analysis

### Time Complexity

| Approach | Complexity |
|----------|------------|
| Brute Force | O(M^V) |
| Backtracking | O(M^V) worst case |
| BFS-based (this impl) | O(V × E) |

### Space Complexity

| Component | Complexity |
|-----------|------------|
| Color array | O(V) |
| Visited array | O(V) |
| Queue (BFS) | O(V) |

## Algorithm Pseudocode

### Backtracking Approach

```
M-COLORING(graph, m, n):
    colors = new array[n] initialized to 0
    return SOLVE(graph, m, colors, 0)

SOLVE(graph, m, colors, vertex):
    if vertex == n:
        return true  // All vertices colored
    
    for color = 1 to m:
        if IS-SAFE(graph, colors, vertex, color):
            colors[vertex] = color
            if SOLVE(graph, m, colors, vertex + 1):
                return true
            colors[vertex] = 0  // Backtrack
    
    return false

IS-SAFE(graph, colors, vertex, color):
    for each neighbor in graph.adjacentTo(vertex):
        if colors[neighbor] == color:
            return false
    return true
```

### BFS-Based Approach (Current Implementation)

```
IS-COLORING-POSSIBLE(nodes, n, m):
    visited = new array[n+1] initialized to 0
    maxColors = 1
    
    for sv = 1 to n:
        if visited[sv] > 0:
            continue
        
        visited[sv] = 1
        queue = new Queue()
        queue.add(sv)
        
        while queue not empty:
            current = queue.poll()
            
            for each neighbor in nodes[current].edges:
                if nodes[current].color == nodes[neighbor].color:
                    nodes[neighbor].color += 1
                
                maxColors = max(maxColors, 
                               nodes[current].color, 
                               nodes[neighbor].color)
                
                if maxColors > m:
                    return false
                
                if visited[neighbor] == 0:
                    visited[neighbor] = 1
                    queue.add(neighbor)
    
    return true
```

## Implementation Details

### Java Implementation

```java
public final class MColoring {
    
    static boolean isColoringPossible(ArrayList<Node> nodes, int n, int m) {
        ArrayList<Integer> visited = new ArrayList<Integer>();
        for (int i = 0; i < n + 1; i++) {
            visited.add(0);
        }

        int maxColors = 1;

        for (int sv = 1; sv <= n; sv++) {
            if (visited.get(sv) > 0) {
                continue;
            }

            visited.set(sv, 1);
            Queue<Integer> q = new LinkedList<>();
            q.add(sv);

            while (q.size() != 0) {
                int top = q.peek();
                q.remove();

                for (int it : nodes.get(top).edges) {
                    if (nodes.get(top).color == nodes.get(it).color) {
                        nodes.get(it).color += 1;
                    }

                    maxColors = Math.max(maxColors, 
                        Math.max(nodes.get(top).color, nodes.get(it).color));

                    if (maxColors > m) {
                        return false;
                    }

                    if (visited.get(it) == 0) {
                        visited.set(it, 1);
                        q.add(it);
                    }
                }
            }
        }
        return true;
    }
}
```

### Node Structure

```java
class Node {
    int color;
    List<Integer> edges;
    
    Node() {
        this.color = 1;  // Default color
        this.edges = new ArrayList<>();
    }
}
```

## Visual Example

### Graph Coloring with M = 3

```
Original Graph:
    1 --- 2
    |   / |
    | /   |
    3 --- 4

Adjacency:
1: [2, 3]
2: [1, 3, 4]
3: [1, 2, 4]
4: [2, 3]

Coloring Process:
Step 1: Color node 1 with color 1
Step 2: Node 2 adjacent to 1, gets color 2
Step 3: Node 3 adjacent to 1,2, gets color 3
Step 4: Node 4 adjacent to 2,3, needs color 1

Result:
Node 1: Color 1 (Red)
Node 2: Color 2 (Green)
Node 3: Color 3 (Blue)
Node 4: Color 1 (Red)

    R --- G
    |   / |
    | /   |
    B --- R

✓ Valid 3-coloring
```

## Real-World Applications

### 1. Map Coloring
- Political maps (countries, states)
- Four Color Theorem application

### 2. Scheduling Problems
- Exam scheduling (no conflicts)
- Time slot allocation
- Meeting room assignment

### 3. Register Allocation
- Compiler optimization
- Assigning variables to CPU registers
- Graph coloring in interference graphs

### 4. Frequency Assignment
- Radio/TV channel allocation
- Cellular network frequencies
- Avoiding interference

### 5. Sudoku as Graph Coloring
- 9-coloring problem
- Cells as vertices, constraints as edges

## Algorithm Variants

### Greedy Coloring

```java
void greedyColoring(Graph g) {
    int[] result = new int[V];
    Arrays.fill(result, -1);
    result[0] = 0;  // First vertex gets first color
    
    boolean[] available = new boolean[V];
    
    for (int u = 1; u < V; u++) {
        Arrays.fill(available, true);
        
        // Mark colors of adjacent vertices as unavailable
        for (int neighbor : g.adj[u]) {
            if (result[neighbor] != -1) {
                available[result[neighbor]] = false;
            }
        }
        
        // Find first available color
        for (int c = 0; c < V; c++) {
            if (available[c]) {
                result[u] = c;
                break;
            }
        }
    }
}
```

### Welsh-Powell Algorithm

1. Sort vertices by degree (descending)
2. Assign first color to first uncolored vertex
3. Assign same color to all non-adjacent uncolored vertices
4. Repeat with next color until all colored

## Common Pitfalls and Edge Cases

### 1. Disconnected Graphs

```
⚠️ Edge Case: Graph with multiple components

Each component must be colored independently.
The implementation handles this with the outer loop:

for (int sv = 1; sv <= n; sv++) {
    if (visited.get(sv) > 0) continue;  // Skip if already visited
    // ... BFS from sv
}
```

### 2. Single Node

```
⚠️ Edge Case: Graph with one vertex

Always 1-colorable:
- No edges means no constraints
- Any M ≥ 1 is sufficient
```

### 3. Complete Graph

```
⚠️ Edge Case: K_n (complete graph on n vertices)

Requires exactly n colors:
- Every vertex adjacent to every other
- χ(K_n) = n
```

### 4. Bipartite Detection

```
✓ Optimization: Check if graph is bipartite first

If M ≥ 2 and graph is bipartite:
- Answer is immediately YES
- Can use simple BFS/DFS to check
```

### 5. Color Index Starting Point

```
❌ Pitfall: Inconsistent color indexing

This implementation uses 1-based colors:
- Colors: 1, 2, 3, ..., m
- Initialize all nodes with color = 1
```

## Testing Strategies

### Test Cases

```java
@Test
void testBipartiteGraph() {
    // Bipartite graph needs only 2 colors
    ArrayList<Node> graph = createBipartiteGraph(4);
    assertTrue(MColoring.isColoringPossible(graph, 4, 2));
}

@Test
void testCompleteGraph() {
    // K4 needs 4 colors
    ArrayList<Node> k4 = createCompleteGraph(4);
    assertFalse(MColoring.isColoringPossible(k4, 4, 3));
    assertTrue(MColoring.isColoringPossible(k4, 4, 4));
}

@Test
void testDisconnectedGraph() {
    // Two separate triangles
    ArrayList<Node> graph = createTwoTriangles();
    assertTrue(MColoring.isColoringPossible(graph, 6, 3));
}
```

## Comparison with Related Problems

| Problem | Question | Complexity |
|---------|----------|------------|
| M-Coloring | Can we color with M colors? | NP-complete (M≥3) |
| Chromatic Number | What is minimum M? | NP-hard |
| 2-Coloring | Is graph bipartite? | O(V+E) |
| Edge Coloring | Color edges, not vertices | NP-complete |

## References

### Academic
- Garey, M. R. & Johnson, D. S. (1979). "Computers and Intractability"
- Welsh, D. J. A. & Powell, M. B. (1967). "An Upper Bound for the Chromatic Number"

### Related Algorithms
- [N-Queens](n-queens.md) - Constraint satisfaction
- [Sudoku Solver](sudoku-solver.md) - 9-coloring variant
- [Graph Algorithms](../../05-graph-algorithms/README.md)

---

*Last updated: Phase 2 Documentation*
