# A* (A-Star) Algorithm

> **Category:** Graph Algorithms  
> **Subcategory:** Pathfinding  
> **Implementation:** [AStar.java](../../../src/main/java/com/thealgorithms/datastructures/graphs/AStar.java)

---

## 📚 Overview

A* (pronounced "A-star") is a **best-first search algorithm** that finds the shortest path between nodes using heuristics to guide its search. It combines the benefits of Dijkstra's algorithm (guaranteed shortest path) with greedy best-first search (fast with good heuristics).

Developed by Peter Hart, Nils Nilsson, and Bertram Raphael in 1968 at Stanford Research Institute, A* is widely used in pathfinding and graph traversal, particularly in video games, robotics, and route planning.

---

## 🔢 Mathematical Foundation

### Definition

A* evaluates nodes using the function:
$$
f(n) = g(n) + h(n)
$$

Where:
- $f(n)$ = estimated total cost of path through node $n$
- $g(n)$ = actual cost from start to node $n$
- $h(n)$ = heuristic estimate of cost from $n$ to goal

### Key Properties

- **Admissibility:** If $h(n) \leq h^*(n)$ (never overestimates), A* finds optimal path
- **Consistency (Monotonicity):** If $h(n) \leq c(n, n') + h(n')$, A* is efficient
- **Optimality:** Guaranteed shortest path with admissible heuristic
- **Complete:** Will find a path if one exists (for finite graphs)

### Mathematical Formulation

**Node Selection:**
$$
n^* = \arg\min_{n \in \text{OpenSet}} f(n) = \arg\min_{n \in \text{OpenSet}} [g(n) + h(n)]
$$

**Path Cost Update:**
$$
g(n') = \min(g(n'), g(n) + c(n, n'))
$$

### Common Heuristics

| Heuristic | Formula | Best For |
|-----------|---------|----------|
| **Euclidean** | $\sqrt{(x_1-x_2)^2 + (y_1-y_2)^2}$ | Any direction movement |
| **Manhattan** | $|x_1-x_2| + |y_1-y_2|$ | Grid with 4-direction movement |
| **Chebyshev** | $\max(|x_1-x_2|, |y_1-y_2|)$ | Grid with 8-direction movement |
| **Octile** | $\max(dx, dy) + (\sqrt{2}-1) \cdot \min(dx, dy)$ | 8-direction with diagonal cost |

---

## 📊 Complexity Analysis

| Metric | Best Case | Average Case | Worst Case |
|--------|-----------|--------------|------------|
| **Time** | $O(1)$ | $O(b^d)$ | $O(b^d)$ |
| **Space** | $O(1)$ | $O(b^d)$ | $O(b^d)$ |

Where:
- $b$ = branching factor
- $d$ = depth of solution

### Detailed Analysis

**Time Complexity:**
- Depends heavily on heuristic quality
- Perfect heuristic: $O(d)$ - goes straight to goal
- Zero heuristic: $O(b^d)$ - becomes Dijkstra's

**Space Complexity:**
- Must store all generated nodes in worst case
- Memory is often the limiting factor

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM AStar(start, goal, h)
    INPUT: Start node, goal node, heuristic function h
    OUTPUT: Shortest path from start to goal
    
    // Initialize
    1. openSet ← {start}          // Nodes to explore
    2. closedSet ← {}             // Already explored
    3. g[start] ← 0               // Cost from start
    4. f[start] ← h(start, goal)  // Estimated total cost
    5. parent[start] ← null
    
    // Main loop
    6. WHILE openSet is not empty DO
    7.     current ← node in openSet with lowest f value
    8.     
    9.     IF current = goal THEN
    10.        RETURN ReconstructPath(parent, current)
    11.    END IF
    12.    
    13.    openSet.remove(current)
    14.    closedSet.add(current)
    15.    
    16.    FOR each neighbor of current DO
    17.        IF neighbor in closedSet THEN
    18.            CONTINUE
    19.        END IF
    20.        
    21.        tentativeG ← g[current] + cost(current, neighbor)
    22.        
    23.        IF neighbor not in openSet THEN
    24.            openSet.add(neighbor)
    25.        ELSE IF tentativeG >= g[neighbor] THEN
    26.            CONTINUE  // Not a better path
    27.        END IF
    28.        
    29.        // Best path so far
    30.        parent[neighbor] ← current
    31.        g[neighbor] ← tentativeG
    32.        f[neighbor] ← g[neighbor] + h(neighbor, goal)
    33.    END FOR
    34. END WHILE
    
    35. RETURN failure  // No path exists
```

### Step-by-Step Walkthrough

Consider pathfinding on a grid from S to G:
```
┌───┬───┬───┬───┬───┐
│ S │   │   │   │   │
├───┼───┼───┼───┼───┤
│   │ X │ X │   │   │
├───┼───┼───┼───┼───┤
│   │ X │   │   │   │
├───┼───┼───┼───┼───┤
│   │   │   │ X │ G │
└───┴───┴───┴───┴───┘
```

Using Manhattan distance heuristic:

| Step | Current | f(current) | Open Set | Action |
|------|---------|------------|----------|--------|
| 1 | S(0,0) | 0+7=7 | {S} | Start |
| 2 | S(0,0) | - | {(0,1), (1,0)} | Expand S |
| 3 | (0,1) | 1+6=7 | {(1,0), (0,2)} | Expand |
| ... | ... | ... | ... | ... |
| n | G(3,4) | 7+0=7 | {} | Goal reached |

**Path found:** S → (0,1) → (0,2) → (0,3) → (1,3) → (2,3) → (2,4) → (3,4) → G

---

## 💻 Implementation Notes

### Java Implementation Highlights

- Inner classes for `Graph`, `Edge`, and `PathAndDistance`
- Uses priority queue for efficient node selection
- Supports weighted directed graphs
- Returns both path and total distance

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/datastructures/graphs/AStar.java`

```java
public class AStar {
    
    // Graph representation
    static class Graph {
        ArrayList<ArrayList<Edge>> graph;
        // ...
    }
    
    // Edge with weight
    static class Edge {
        int to;
        int weight;
        // ...
    }
    
    // Result container
    static class PathAndDistance {
        int distance;
        ArrayList<Integer> path;
        // ...
    }
    
    // A* implementation
    public static PathAndDistance aStar(
            int start, int goal, Graph graph, int[] heuristic) {
        
        PriorityQueue<int[]> openSet = new PriorityQueue<>(
            Comparator.comparingInt(a -> a[1])  // Sort by f value
        );
        
        int[] gScore = new int[n];
        Arrays.fill(gScore, Integer.MAX_VALUE);
        gScore[start] = 0;
        
        int[] fScore = new int[n];
        Arrays.fill(fScore, Integer.MAX_VALUE);
        fScore[start] = heuristic[start];
        
        // Main loop
        while (!openSet.isEmpty()) {
            int current = openSet.poll()[0];
            
            if (current == goal) {
                return reconstructPath(parent, current, gScore[goal]);
            }
            
            for (Edge edge : graph.get(current)) {
                int tentativeG = gScore[current] + edge.weight;
                if (tentativeG < gScore[edge.to]) {
                    parent[edge.to] = current;
                    gScore[edge.to] = tentativeG;
                    fScore[edge.to] = tentativeG + heuristic[edge.to];
                    openSet.add(new int[]{edge.to, fScore[edge.to]});
                }
            }
        }
        
        return null;  // No path
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Video Game Pathfinding
**Use Case:** NPC movement, unit navigation in RTS games  
**Example:** Finding path around obstacles while considering terrain costs

### 2. Robotics Navigation
**Use Case:** Autonomous vehicle path planning  
**Example:** Warehouse robots finding optimal routes to shelves

### 3. GPS Navigation Systems
**Use Case:** Real-time route calculation  
**Example:** Google Maps, Waze computing driving directions

### 4. Puzzle Solving
**Use Case:** Solving sliding puzzles, Rubik's cube  
**Example:** 15-puzzle solver using misplaced tiles heuristic

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| Unity/Unreal | Game NPC pathfinding |
| Google Maps | Route optimization |
| Boston Dynamics | Robot navigation |
| Amazon Robotics | Warehouse automation |
| Chess Engines | Move search (with modifications) |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | A* | Dijkstra | BFS | Greedy Best-First |
|--------|-----|----------|-----|-------------------|
| Heuristic | Uses $h(n)$ | None (h=0) | None | Only $h(n)$ |
| Optimal | Yes (if admissible) | Yes | Yes (unweighted) | No |
| Complete | Yes | Yes | Yes | No |
| Time | $O(b^d)$ | $O(V^2)$ or $O(E \log V)$ | $O(V+E)$ | $O(b^m)$ |
| Best For | Pathfinding | All shortest paths | Unweighted | Fast but suboptimal |

### When to Choose A*

✅ **Use A* when:**
- Single-source to single-target path needed
- Good heuristic is available
- Optimal path is required
- Graph has positive edge weights

❌ **Don't use when:**
- Need all shortest paths (use Dijkstra)
- No good heuristic available
- Memory is severely limited (consider IDA*)
- Graph is unweighted (use BFS)

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Inadmissible Heuristic:** Overestimating can lead to suboptimal paths
2. **Memory Exhaustion:** Large state spaces can overwhelm memory
3. **Heuristic Cost:** Expensive heuristic computation slows search
4. **Tie-Breaking:** Poor tie-breaking can significantly affect performance

### Heuristic Selection Guide

```
IF movement is 4-directional (grid) THEN
    USE Manhattan distance
ELSE IF movement is 8-directional THEN
    USE Chebyshev or Octile distance
ELSE IF movement is free (any angle) THEN
    USE Euclidean distance
END IF
```

### Edge Cases to Handle

- [x] Start equals goal (return immediately)
- [x] No path exists (return failure)
- [x] Zero-weight edges
- [x] Large open spaces (many equal-cost paths)
- [x] Narrow corridors (heuristic less helpful)

### Performance Optimization Tips

```java
// 1. Use indexed priority queue for decrease-key operations
// 2. Precompute heuristic values
// 3. Use consistent tie-breaking (prefer lower g values)
// 4. Consider Jump Point Search for grid maps
```

---

## 📖 References

1. Hart, P. E., Nilsson, N. J., & Raphael, B. (1968). "A Formal Basis for the Heuristic Determination of Minimum Cost Paths". *IEEE Transactions on Systems Science and Cybernetics*. 4 (2): 100–107.
2. Russell, S., & Norvig, P. (2020). *Artificial Intelligence: A Modern Approach* (4th ed.). Pearson. Chapter 3.
3. Cormen, T. H., et al. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press.
4. [Wikipedia: A* search algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm)
5. [Red Blob Games: A* Tutorial](https://www.redblobgames.com/pathfinding/a-star/introduction.html)

---

## 🔗 Related Algorithms

- [Dijkstra's Algorithm](../shortest-path/dijkstra.md) - A* with h(n)=0
- [BFS](../../02-searching-algorithms/bfs.md) - Unweighted shortest path
- [IDA*](ida-star.md) - Memory-efficient A* variant
- [Jump Point Search](jps.md) - Grid-optimized A*
- [D* (Dynamic A*)](d-star.md) - For changing environments
