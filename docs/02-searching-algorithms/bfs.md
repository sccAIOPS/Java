# Breadth-First Search (BFS)

> **Category:** Searching Algorithms / Graph Algorithms  
> **Subcategory:** Graph Traversal  
> **Implementation:** [BreadthFirstSearch.java](../../src/main/java/com/thealgorithms/searches/BreadthFirstSearch.java)

---

## 📚 Overview

**Breadth-First Search (BFS)** is a graph traversal algorithm that explores vertices level by level, visiting all neighbors of the current vertex before moving to vertices at the next depth level. It uses a queue data structure and is fundamental for finding shortest paths in unweighted graphs, level-order tree traversal, and many network-related algorithms.

---

## 🔢 Mathematical Foundation

### Definition

BFS systematically explores a graph G = (V, E) starting from source vertex s:
- Visit s at level 0
- Visit all vertices adjacent to level k vertices at level k+1
- Continue until all reachable vertices are visited

### Key Properties

- **Shortest Path:** BFS finds shortest path (minimum edges) in unweighted graphs
- **Level Discovery:** Vertices are discovered in order of their distance from source
- **Complete:** BFS visits all reachable vertices

### Distance Property

For any vertex v reachable from source s:
$$d(s, v) = \text{minimum number of edges from s to v}$$

BFS guarantees: when v is discovered, $d(s, v)$ equals its level number.

---

## 📊 Complexity Analysis

| Metric | Complexity | Notes |
|--------|------------|-------|
| **Time** | $O(V + E)$ | Visit each vertex and edge once |
| **Space** | $O(V)$ | Queue and visited set |
| **Queue Size** | $O(V)$ | Worst case all vertices in queue |

### Detailed Analysis

- Each vertex enqueued/dequeued at most once: O(V)
- Each edge examined at most twice (once per endpoint): O(E)
- Total: O(V + E)

For trees: O(n) where n is number of nodes

---

## 🔄 Algorithm (Pseudocode)

### Standard BFS
```
ALGORITHM BFS(graph, source)
    INPUT: Graph G, starting vertex source
    OUTPUT: Visited vertices in BFS order
    
    1. queue ← empty Queue
    2. visited ← empty Set
    3. result ← empty List
    4. queue.enqueue(source)
    5. visited.add(source)
    
    6. WHILE queue is not empty DO
    7.     current ← queue.dequeue()
    8.     result.add(current)
    9.     FOR EACH neighbor IN graph.adjacentVertices(current) DO
    10.        IF neighbor NOT IN visited THEN
    11.            visited.add(neighbor)
    12.            queue.enqueue(neighbor)
    13.        END IF
    14.    END FOR
    15. END WHILE
    
    16. RETURN result
```

### BFS for Shortest Path
```
ALGORITHM BFSShortestPath(graph, source, target)
    INPUT: Graph G, source vertex, target vertex
    OUTPUT: Shortest path from source to target
    
    1. queue ← empty Queue
    2. visited ← empty Set
    3. parent ← empty Map
    
    4. queue.enqueue(source)
    5. visited.add(source)
    6. parent[source] ← null
    
    7. WHILE queue is not empty DO
    8.     current ← queue.dequeue()
    9.     IF current = target THEN
    10.        RETURN reconstructPath(parent, target)
    11.    END IF
    12.    FOR EACH neighbor IN graph.adjacentVertices(current) DO
    13.        IF neighbor NOT IN visited THEN
    14.            visited.add(neighbor)
    15.            parent[neighbor] ← current
    16.            queue.enqueue(neighbor)
    17.        END IF
    18.    END FOR
    19. END WHILE
    
    20. RETURN null  // No path exists
```

### Step-by-Step Walkthrough

**Example Graph:**
```
    1 --- 2
    |     |
    3 --- 4 --- 5
          |
          6
```

**BFS from vertex 1:**
```
Level 0: Visit 1, Queue: [2, 3]
Level 1: Visit 2, Queue: [3, 4]
         Visit 3, Queue: [4]
Level 2: Visit 4, Queue: [5, 6]
Level 3: Visit 5, Queue: [6]
         Visit 6, Queue: []

BFS Order: 1 → 2 → 3 → 4 → 5 → 6
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Generic Implementation:** Works with any graph node type
- **Visited Tracking:** Uses Set for O(1) lookup
- **Optional Parameter:** Can specify target to find specific node
- **Returns List:** Maintains traversal order

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/searches/BreadthFirstSearch.java`

```java
public class BreadthFirstSearch<T> {
    
    public List<T> search(Graph<T> graph, T start) {
        List<T> result = new ArrayList<>();
        Set<T> visited = new HashSet<>();
        Queue<T> queue = new LinkedList<>();
        
        queue.offer(start);
        visited.add(start);
        
        while (!queue.isEmpty()) {
            T current = queue.poll();
            result.add(current);
            
            for (T neighbor : graph.getNeighbors(current)) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
        
        return result;
    }
    
    public Optional<T> search(Graph<T> graph, T start, T target) {
        if (start.equals(target)) {
            return Optional.of(target);
        }
        
        Set<T> visited = new HashSet<>();
        Queue<T> queue = new LinkedList<>();
        
        queue.offer(start);
        visited.add(start);
        
        while (!queue.isEmpty()) {
            T current = queue.poll();
            
            for (T neighbor : graph.getNeighbors(current)) {
                if (neighbor.equals(target)) {
                    return Optional.of(target);
                }
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
        
        return Optional.empty();
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Social Network Analysis
**Use Case:** Finding degrees of separation, friend recommendations  
**Example:** LinkedIn "People You May Know" based on connection distance

### 2. Web Crawlers
**Use Case:** Systematic website exploration  
**Example:** Google's web crawler discovers pages level by level

### 3. GPS Navigation
**Use Case:** Finding shortest routes in road networks  
**Example:** Maps applications finding quickest path (unweighted)

### 4. Network Broadcasting
**Use Case:** Packet routing, broadcast protocols  
**Example:** Flooding algorithms in network protocols

### 5. Puzzle Solving
**Use Case:** Finding minimum moves to solve puzzles  
**Example:** Sliding puzzle, Rubik's cube optimal solutions

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **LinkedIn** | Connection recommendations |
| **Google** | Web crawler (Googlebot) |
| **Facebook** | Social graph analysis |
| **Cisco** | Network routing protocols |
| **Game Engines** | Pathfinding AI |

---

## ⚖️ Comparison with DFS

| Feature | BFS | DFS |
|---------|-----|-----|
| **Data Structure** | Queue | Stack/Recursion |
| **Order** | Level-by-level | Depth-first |
| **Memory** | O(V) | O(V) worst |
| **Shortest Path** | ✅ Yes (unweighted) | ❌ No |
| **Space (Wide Graph)** | Higher | Lower |
| **Space (Deep Graph)** | Lower | Higher (stack overflow) |
| **Use Case** | Shortest path, levels | Connectivity, cycles |

### When to Use BFS

✅ **Use BFS when:**
- Finding shortest path in unweighted graph
- Level-order traversal needed
- Exploring all nodes at distance k
- Solution is likely near the source

❌ **Use DFS instead when:**
- Memory is limited for wide graphs
- Need to explore deep paths first
- Detecting cycles
- Topological sorting

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Not Marking Visited Before Enqueue:** Can add vertex multiple times
   - Solution: Mark visited when enqueuing, not when dequeuing

2. **Disconnected Graph:** Not all vertices reached
   - Solution: Run BFS from each unvisited vertex

3. **Memory for Wide Graphs:** Queue can grow large
   - Solution: Consider bidirectional BFS or iterative deepening

4. **Infinite Graphs:** BFS may not terminate
   - Solution: Add depth limit or cycle detection

### Edge Cases to Handle

- [x] Empty graph
- [x] Single vertex graph
- [x] Disconnected graph components
- [x] Source equals target
- [x] No path exists to target
- [x] Graph with cycles

---

## 📖 References

1. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 22
2. Moore, E.F. "The shortest path through a maze" (1959)
3. [Wikipedia: Breadth-First Search](https://en.wikipedia.org/wiki/Breadth-first_search)

---

## 🔗 Related Algorithms

- [Depth-First Search](./dfs.md) - Alternative traversal strategy
- [Dijkstra's Algorithm](../05-graph-algorithms/shortest-path/dijkstra.md) - Weighted shortest path
- [A* Search](../05-graph-algorithms/shortest-path/a-star.md) - Heuristic-guided search
- [Level Order Traversal](../04-data-structures/trees/level-order.md) - BFS on trees
- [Bidirectional BFS](./bidirectional-bfs.md) - Optimized for known target
