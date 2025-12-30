# Depth-First Search (DFS)

> **Category:** Searching Algorithms / Graph Algorithms  
> **Subcategory:** Graph Traversal  
> **Implementation:** [DepthFirstSearch.java](../../src/main/java/com/thealgorithms/searches/DepthFirstSearch.java)

---

## 📚 Overview

**Depth-First Search (DFS)** is a graph traversal algorithm that explores as far as possible along each branch before backtracking. It uses a stack (explicitly or via recursion) and is fundamental for topological sorting, cycle detection, pathfinding, and solving puzzles like mazes. DFS naturally explores the depth of a graph structure.

---

## 🔢 Mathematical Foundation

### Definition

DFS systematically explores a graph G = (V, E) starting from source vertex s:
- Visit s, then recursively visit an unvisited neighbor
- Backtrack when no unvisited neighbors remain
- Continue until all reachable vertices are visited

### Discovery and Finish Times

DFS assigns timestamps to vertices:
- **Discovery time (d[v]):** When v is first visited
- **Finish time (f[v]):** When all of v's descendants are explored

### Key Properties

- **Parenthesis Theorem:** For any u, v: either [d[u], f[u]] and [d[v], f[v]] are disjoint, or one contains the other
- **White Path Theorem:** v is a descendant of u in DFS tree iff at time d[u], there's a path of white (unvisited) vertices from u to v

### Edge Classification

| Edge Type | Condition | Meaning |
|-----------|-----------|---------|
| **Tree Edge** | v discovered via u | Part of DFS tree |
| **Back Edge** | v is ancestor of u | Indicates cycle |
| **Forward Edge** | v is descendant of u (not tree edge) | Non-tree descendant |
| **Cross Edge** | Neither ancestor nor descendant | Between unrelated vertices |

---

## 📊 Complexity Analysis

| Metric | Complexity | Notes |
|--------|------------|-------|
| **Time** | $O(V + E)$ | Visit each vertex and edge once |
| **Space** | $O(V)$ | Recursion stack or explicit stack |
| **Stack Depth** | $O(V)$ | Worst case for linear graph |

### Detailed Analysis

- Each vertex is visited exactly once: O(V)
- Each edge is examined exactly twice (once per endpoint): O(E)
- Total: O(V + E)

---

## 🔄 Algorithm (Pseudocode)

### Recursive DFS
```
ALGORITHM DFS(graph, source)
    INPUT: Graph G, starting vertex source
    OUTPUT: Visited vertices in DFS order
    
    visited ← empty Set
    result ← empty List
    DFSVisit(graph, source, visited, result)
    RETURN result

ALGORITHM DFSVisit(graph, vertex, visited, result)
    1. visited.add(vertex)
    2. result.add(vertex)
    3. FOR EACH neighbor IN graph.adjacentVertices(vertex) DO
    4.     IF neighbor NOT IN visited THEN
    5.         DFSVisit(graph, neighbor, visited, result)
    6.     END IF
    7. END FOR
```

### Iterative DFS (Using Stack)
```
ALGORITHM DFSIterative(graph, source)
    INPUT: Graph G, starting vertex source
    OUTPUT: Visited vertices in DFS order
    
    1. stack ← empty Stack
    2. visited ← empty Set
    3. result ← empty List
    
    4. stack.push(source)
    
    5. WHILE stack is not empty DO
    6.     current ← stack.pop()
    7.     IF current NOT IN visited THEN
    8.         visited.add(current)
    9.         result.add(current)
    10.        FOR EACH neighbor IN graph.adjacentVertices(current) DO
    11.            IF neighbor NOT IN visited THEN
    12.                stack.push(neighbor)
    13.            END IF
    14.        END FOR
    15.    END IF
    16. END WHILE
    
    17. RETURN result
```

### DFS for Cycle Detection
```
ALGORITHM HasCycle(graph)
    visited ← empty Set
    recursionStack ← empty Set
    
    FOR EACH vertex IN graph.vertices DO
        IF DFSDetectCycle(graph, vertex, visited, recursionStack) THEN
            RETURN true
        END IF
    END FOR
    RETURN false

ALGORITHM DFSDetectCycle(graph, vertex, visited, recursionStack)
    IF vertex IN recursionStack THEN
        RETURN true  // Back edge found = cycle
    IF vertex IN visited THEN
        RETURN false
    
    visited.add(vertex)
    recursionStack.add(vertex)
    
    FOR EACH neighbor IN graph.adjacentVertices(vertex) DO
        IF DFSDetectCycle(graph, neighbor, visited, recursionStack) THEN
            RETURN true
        END IF
    END FOR
    
    recursionStack.remove(vertex)
    RETURN false
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

**DFS from vertex 1 (recursive):**
```
Visit 1 → explore neighbor 2
  Visit 2 → explore neighbor 4
    Visit 4 → explore neighbor 3
      Visit 3 → neighbor 1 visited, backtrack
    Back to 4 → explore neighbor 5
      Visit 5 → backtrack
    Back to 4 → explore neighbor 6
      Visit 6 → backtrack
    Back to 4 → backtrack
  Back to 2 → backtrack
Back to 1 → backtrack

DFS Order: 1 → 2 → 4 → 3 → 5 → 6
```

---

## 💻 Implementation Notes

### Java Implementation Highlights

- **Recursive Implementation:** Clean and intuitive
- **Generic Type:** Works with any node type
- **Visited Set:** Prevents revisiting nodes
- **Optional Target:** Can search for specific node

### Code Reference

📁 **File:** `src/main/java/com/thealgorithms/searches/DepthFirstSearch.java`

```java
public class DepthFirstSearch<T> {
    
    public List<T> search(Graph<T> graph, T start) {
        List<T> result = new ArrayList<>();
        Set<T> visited = new HashSet<>();
        dfsRecursive(graph, start, visited, result);
        return result;
    }
    
    private void dfsRecursive(Graph<T> graph, T vertex, 
                              Set<T> visited, List<T> result) {
        visited.add(vertex);
        result.add(vertex);
        
        for (T neighbor : graph.getNeighbors(vertex)) {
            if (!visited.contains(neighbor)) {
                dfsRecursive(graph, neighbor, visited, result);
            }
        }
    }
    
    public Optional<T> search(Graph<T> graph, T start, T target) {
        Set<T> visited = new HashSet<>();
        return dfsSearch(graph, start, target, visited);
    }
    
    private Optional<T> dfsSearch(Graph<T> graph, T vertex, 
                                   T target, Set<T> visited) {
        if (vertex.equals(target)) {
            return Optional.of(target);
        }
        
        visited.add(vertex);
        
        for (T neighbor : graph.getNeighbors(vertex)) {
            if (!visited.contains(neighbor)) {
                Optional<T> result = dfsSearch(graph, neighbor, target, visited);
                if (result.isPresent()) {
                    return result;
                }
            }
        }
        
        return Optional.empty();
    }
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Topological Sorting
**Use Case:** Build systems, task scheduling with dependencies  
**Example:** Maven/Gradle build order, course prerequisite planning

### 2. Cycle Detection
**Use Case:** Detecting circular dependencies  
**Example:** Deadlock detection in operating systems, circular import detection

### 3. Maze Solving
**Use Case:** Pathfinding in games and puzzles  
**Example:** Game AI, robot navigation algorithms

### 4. Connected Components
**Use Case:** Finding isolated groups in networks  
**Example:** Social network community detection

### 5. Strongly Connected Components
**Use Case:** Graph analysis and optimization  
**Example:** Compiler optimization, web page ranking

### Industry Examples

| Company/Product | Application |
|-----------------|-------------|
| **Git** | Commit graph traversal |
| **Maven/Gradle** | Dependency resolution (topological sort) |
| **Compilers** | Control flow analysis |
| **Game Engines** | AI pathfinding |
| **Operating Systems** | Deadlock detection |

---

## ⚖️ Comparison with BFS

| Feature | DFS | BFS |
|---------|-----|-----|
| **Data Structure** | Stack/Recursion | Queue |
| **Order** | Depth-first | Level-by-level |
| **Memory (Wide Graph)** | Lower | Higher |
| **Memory (Deep Graph)** | Higher | Lower |
| **Shortest Path** | ❌ No | ✅ Yes (unweighted) |
| **Topological Sort** | ✅ Natural | ❌ Not suitable |
| **Cycle Detection** | ✅ Back edges | ❌ Not direct |

### DFS Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| **Pre-order** | Process before children | Tree serialization |
| **Post-order** | Process after children | Expression evaluation |
| **In-order (binary tree)** | Left, node, right | BST sorted order |
| **Iterative Deepening** | DFS with depth limit | Memory-bounded search |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Stack Overflow:** Deep graphs cause recursion limit
   - Solution: Use iterative version with explicit stack

2. **Not Handling Disconnected Graphs:** Missing components
   - Solution: Run DFS from each unvisited vertex

3. **Cycle in Recursion Stack:** Infinite loop
   - Solution: Track recursion stack separately from visited

4. **Order of Neighbor Exploration:** Different orders give different results
   - Solution: Document expected behavior, sort neighbors if determinism needed

### Edge Cases to Handle

- [x] Empty graph
- [x] Single vertex graph
- [x] Disconnected graph components
- [x] Self-loops
- [x] Graphs with cycles
- [x] Very deep graphs (stack overflow risk)

---

## 📖 References

1. Cormen, T.H., et al. "Introduction to Algorithms" (CLRS), Chapter 22
2. Tarjan, R. "Depth-first search and linear graph algorithms" (1972)
3. [Wikipedia: Depth-First Search](https://en.wikipedia.org/wiki/Depth-first_search)

---

## 🔗 Related Algorithms

- [Breadth-First Search](./bfs.md) - Alternative traversal strategy
- [Topological Sort](../05-graph-algorithms/topological-sort.md) - DFS application
- [Kosaraju's Algorithm](../05-graph-algorithms/kosaraju.md) - SCC using DFS
- [Tarjan's Algorithm](../05-graph-algorithms/tarjan.md) - SCC in single DFS
- [Backtracking](../06-backtracking/README.md) - DFS for constraint satisfaction
