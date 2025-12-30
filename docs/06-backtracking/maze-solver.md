# Maze Solver

> **Category:** Algorithms > Backtracking > Pathfinding
> **Implementation:** [MazeRecursion.java](../../../src/main/java/com/thealgorithms/backtracking/MazeRecursion.java)

## Overview

A **Maze Solver** uses recursive backtracking to find a path through a maze from a starting position to a target destination. The algorithm explores possible paths, marking visited cells and backtracking when dead ends are encountered.

This implementation demonstrates the fundamental backtracking pattern applied to grid-based pathfinding problems.

### Problem Definition

Given:
- A 2D grid representing a maze
- Starting position (typically top-left area)
- Target position (specified cell)
- Walls blocking certain cells

Find:
- A path from start to target, if one exists

## Mathematical Foundation

### Maze Representation

```
Cell values:
0 = Unvisited path (can traverse)
1 = Wall (blocked)
2 = Path (part of solution)
3 = Dead end (visited but not part of solution)
```

### Movement Directions

Four cardinal directions with offsets:
- **Down:** $(+1, 0)$
- **Right:** $(0, +1)$
- **Up:** $(-1, 0)$
- **Left:** $(0, -1)$

### Path Existence

A path exists if there is a sequence of adjacent cells:
$$(r_0, c_0) \rightarrow (r_1, c_1) \rightarrow ... \rightarrow (r_t, c_t)$$

Where:
- $(r_0, c_0)$ is the start
- $(r_t, c_t)$ is the target
- Each transition uses a valid move
- No cell is a wall

## Complexity Analysis

### Time Complexity

| Scenario | Complexity |
|----------|------------|
| Worst Case | O(4^(m×n)) |
| With visited marking | O(m × n) |
| Average | O(m × n) |

Where m = rows, n = columns.

### Space Complexity

| Component | Complexity |
|-----------|------------|
| Grid storage | O(m × n) |
| Recursion stack | O(m × n) worst case |
| Total | O(m × n) |

## Algorithm Pseudocode

### Basic Backtracking

```
SOLVE-MAZE(map):
    if SET-WAY(map, startRow, startCol):
        return map  // Solution found
    return null     // No solution

SET-WAY(map, i, j):
    // Check if target reached
    if map[targetRow][targetCol] == 2:
        return true
    
    // If current cell is unvisited path
    if map[i][j] == 0:
        map[i][j] = 2  // Mark as part of path
        
        // Try Down
        if SET-WAY(map, i + 1, j):
            return true
        
        // Try Right
        if SET-WAY(map, i, j + 1):
            return true
        
        // Try Up
        if SET-WAY(map, i - 1, j):
            return true
        
        // Try Left
        if SET-WAY(map, i, j - 1):
            return true
        
        // No direction worked - mark as dead end
        map[i][j] = 3
        return false
    
    return false  // Cell is wall, visited, or dead end
```

### Alternative Direction Strategy

```
SET-WAY-2(map, i, j):  // Up -> Right -> Down -> Left
    if map[targetRow][targetCol] == 2:
        return true
    
    if map[i][j] == 0:
        map[i][j] = 2
        
        // Try Up first
        if SET-WAY-2(map, i - 1, j):
            return true
        
        // Try Right
        if SET-WAY-2(map, i, j + 1):
            return true
        
        // Try Down
        if SET-WAY-2(map, i + 1, j):
            return true
        
        // Try Left
        if SET-WAY-2(map, i, j - 1):
            return true
        
        map[i][j] = 3
        return false
    
    return false
```

## Implementation Details

### Java Implementation

```java
public final class MazeRecursion {

    private MazeRecursion() {
    }

    /**
     * Solves maze using "down -> right -> up -> left" strategy
     */
    public static int[][] solveMazeUsingFirstStrategy(int[][] map) {
        if (setWay(map, 1, 1)) {
            return map;
        }
        return null;
    }

    /**
     * Solves maze using "up -> right -> down -> left" strategy
     */
    public static int[][] solveMazeUsingSecondStrategy(int[][] map) {
        if (setWay2(map, 1, 1)) {
            return map;
        }
        return null;
    }

    private static boolean setWay(int[][] map, int i, int j) {
        if (map[6][5] == 2) {  // Target position
            return true;
        }

        if (map[i][j] == 0) {
            map[i][j] = 2;  // Mark as path

            // Try directions: down, right, up, left
            if (setWay(map, i + 1, j)) {
                return true;
            } else if (setWay(map, i, j + 1)) {
                return true;
            } else if (setWay(map, i - 1, j)) {
                return true;
            } else if (setWay(map, i, j - 1)) {
                return true;
            }

            map[i][j] = 3;  // Mark as dead end
            return false;
        }
        return false;
    }
}
```

## Visual Example

### Maze Solving Process

```
Initial Maze (8×7):
┌─────────────┐
│ 1 1 1 1 1 1 1 │  1 = Wall
│ 1 0 0 0 0 0 1 │  0 = Path
│ 1 0 0 0 0 0 1 │  Start: (1,1)
│ 1 1 1 0 0 0 1 │  Target: (6,5)
│ 1 0 0 0 0 0 1 │
│ 1 0 0 0 0 0 1 │
│ 1 0 0 0 0 0 1 │
│ 1 1 1 1 1 1 1 │
└─────────────┘

Step-by-step solving:

Step 1: Start at (1,1), mark as 2
┌─────────────┐
│ 1 1 1 1 1 1 1 │
│ 1 2 0 0 0 0 1 │  ← Current
│ 1 0 0 0 0 0 1 │
│ 1 1 1 0 0 0 1 │
│ 1 0 0 0 0 0 1 │
│ 1 0 0 0 0 0 1 │
│ 1 0 0 0 0 T 1 │  T = Target
│ 1 1 1 1 1 1 1 │
└─────────────┘

Step 2: Try down (2,1), mark as 2
...

After backtracking from dead ends:
┌─────────────┐
│ 1 1 1 1 1 1 1 │
│ 1 2 3 3 3 3 1 │  3 = Dead end
│ 1 2 3 3 3 3 1 │
│ 1 1 1 2 3 3 1 │
│ 1 0 0 2 3 3 1 │
│ 1 0 0 2 2 2 1 │
│ 1 0 0 0 0 2 1 │  Solution found!
│ 1 1 1 1 1 1 1 │
└─────────────┘
```

## Movement Strategy Comparison

### Strategy 1: Down → Right → Up → Left

```
Characteristics:
- Prefers moving toward bottom-right
- Good for targets in lower-right quadrant
- May find longer paths for upper targets
```

### Strategy 2: Up → Right → Down → Left

```
Characteristics:
- Prefers moving toward top-right first
- Good for targets in upper portion
- Different path for same maze
```

### Example Difference

```
Same maze, different strategies:

Strategy 1 path:        Strategy 2 path:
   S→→→↓                  S→→→→→↓
   ↓   ↓                      ↓
   ↓→→→↓                      ↓
       ↓                  ↓←←←←
       ↓                  ↓
       T                  →→→T
```

## Real-World Applications

### 1. Robot Navigation
- Autonomous vehicles
- Warehouse robots
- Vacuum cleaners

### 2. Game Development
- NPC pathfinding
- Procedural dungeon generation
- Puzzle games

### 3. Network Routing
- Finding paths through network topology
- Alternative route discovery

### 4. Circuit Design
- PCB trace routing
- VLSI layout

### 5. Geographic Systems
- GPS navigation
- Map routing applications

## Optimization Techniques

### 1. BFS for Shortest Path

```java
// Use BFS instead of DFS for guaranteed shortest path
Queue<int[]> queue = new LinkedList<>();
queue.add(new int[]{startRow, startCol, 0});  // row, col, distance

while (!queue.isEmpty()) {
    int[] current = queue.poll();
    // Process and add neighbors
}
```

### 2. A* Algorithm

```
For optimal pathfinding with heuristics:
f(n) = g(n) + h(n)
- g(n): Cost from start to n
- h(n): Heuristic estimate to target
```

### 3. Bidirectional Search

```
Search from both start and target simultaneously:
- Meet in the middle
- Reduces search space significantly
```

## Common Pitfalls and Edge Cases

### 1. Hardcoded Target Position

```
⚠️ Issue: Current implementation has fixed target

if (map[6][5] == 2)  // Hardcoded target

Better approach:
private static boolean setWay(int[][] map, int i, int j, 
                               int targetRow, int targetCol) {
    if (map[targetRow][targetCol] == 2) {
        return true;
    }
    // ...
}
```

### 2. No Path Exists

```
⚠️ Edge Case: Impossible maze

- Start or target surrounded by walls
- Returns null (no solution)
- Handle gracefully in calling code:

int[][] solution = solveMazeUsingFirstStrategy(map);
if (solution == null) {
    System.out.println("No path exists");
}
```

### 3. Boundary Conditions

```
❌ Pitfall: Array index out of bounds

Current implementation assumes:
- Maze has wall borders (value 1)
- No explicit bounds checking needed

If no wall borders:
if (i < 0 || i >= rows || j < 0 || j >= cols) {
    return false;
}
```

### 4. Stack Overflow on Large Mazes

```
⚠️ Risk: Deep recursion on large mazes

For very large mazes (1000×1000+):
- Consider iterative solution with explicit stack
- Or increase JVM stack size: -Xss4m
```

### 5. Multiple Paths

```
⚠️ Note: Algorithm finds A path, not ALL paths

To find all paths:
- Don't return immediately on success
- Collect solutions and continue
- Backtrack and explore other options
```

## Testing Strategies

### Test Cases

```java
@Test
void testSimpleMaze() {
    int[][] maze = createSimpleMaze();
    int[][] result = MazeRecursion.solveMazeUsingFirstStrategy(maze);
    assertNotNull(result);
    assertEquals(2, result[6][5]);  // Target reached
}

@Test
void testNoSolution() {
    int[][] blocked = createBlockedMaze();
    int[][] result = MazeRecursion.solveMazeUsingFirstStrategy(blocked);
    assertNull(result);
}

@Test
void testBothStrategies() {
    int[][] maze1 = createMaze();
    int[][] maze2 = cloneMaze(maze1);
    
    int[][] result1 = MazeRecursion.solveMazeUsingFirstStrategy(maze1);
    int[][] result2 = MazeRecursion.solveMazeUsingSecondStrategy(maze2);
    
    // Both should find a solution
    assertNotNull(result1);
    assertNotNull(result2);
}
```

## Comparison with Related Algorithms

| Algorithm | Finds | Optimal | Time | Space |
|-----------|-------|---------|------|-------|
| DFS/Backtracking | Any path | No | O(V+E) | O(V) |
| BFS | Shortest path | Yes | O(V+E) | O(V) |
| A* | Shortest path | Yes | O(E) | O(V) |
| Dijkstra | Shortest path | Yes | O(E log V) | O(V) |

## References

### Academic
- Sedgewick, R. "Algorithms in Java"
- Cormen et al. "Introduction to Algorithms" - Graph Algorithms

### Related Algorithms
- [A* Search](../../05-graph-algorithms/pathfinding/a-star.md) - Optimal pathfinding
- [Knight's Tour](knights-tour.md) - Board traversal
- [N-Queens](n-queens.md) - Backtracking pattern

---

*Last updated: Phase 2 Documentation*
