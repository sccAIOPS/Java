# Knight's Tour Problem

> **Category:** Algorithms > Backtracking > Board Problems
> **Implementation:** [KnightsTour.java](../../../src/main/java/com/thealgorithms/backtracking/KnightsTour.java)

## Overview

The **Knight's Tour** problem asks whether a chess knight can visit every square on an N×N chessboard exactly once. Starting from any square, the knight moves in an "L" shape (two squares in one direction and one square perpendicular) and must cover all squares without repetition.

This problem demonstrates **backtracking** combined with **Warnsdorff's heuristic** for efficient solutions.

### Problem Variants

1. **Open Tour:** Start and end on different squares
2. **Closed Tour:** End on a square that can reach the start (Hamiltonian cycle)
3. **Semi-magic Tour:** Row and column sums are equal

## Mathematical Foundation

### Knight Movement

A knight at position $(r, c)$ can move to 8 possible positions:

$$\{(r±1, c±2), (r±2, c±1)\}$$

The 8 moves as offsets:
```
(-2, -1), (-2, +1), (-1, -2), (-1, +2)
(+1, -2), (+1, +2), (+2, -1), (+2, +1)
```

### Existence of Solutions

| Board Size | Open Tour | Closed Tour |
|------------|-----------|-------------|
| 1×1 | Yes (trivial) | Yes |
| 2×2 | No | No |
| 3×3 | No | No |
| 4×4 | No | No |
| 5×5 | Yes | No |
| 6×6 | Yes | Yes |
| 7×7 | Yes | Yes |
| 8×8 | Yes | Yes |
| n×n (n≥5) | Yes | Yes (n≥6) |

### Total Squares to Visit

For an N×N board:
$$\text{Total squares} = N^2$$

For the implementation using a 12×12 grid with 2-cell border:
$$\text{Playable area} = (12-4)^2 = 64$$

## Complexity Analysis

### Time Complexity

| Approach | Complexity |
|----------|------------|
| Naive Backtracking | O(8^(N²)) |
| With Warnsdorff's Rule | O(N²) average |
| With Orphan Detection | Near O(N²) |

### Space Complexity

| Component | Complexity |
|-----------|------------|
| Board representation | O(N²) |
| Recursion stack | O(N²) |
| Move list per cell | O(8) = O(1) |

## Algorithm Pseudocode

### Backtracking with Warnsdorff's Rule

```
KNIGHT-TOUR(board, row, col, count):
    if count > total:
        return true  // All squares visited
    
    neighbors = GET-VALID-NEIGHBORS(board, row, col)
    
    if neighbors is empty and count ≠ total:
        return false  // Dead end
    
    // Sort neighbors by Warnsdorff's rule (fewest onward moves)
    SORT(neighbors by onward-move-count ascending)
    
    for each (nextRow, nextCol, _) in neighbors:
        board[nextRow][nextCol] = count
        
        if not ORPHAN-DETECTED(count, nextRow, nextCol):
            if KNIGHT-TOUR(board, nextRow, nextCol, count + 1):
                return true
        
        board[nextRow][nextCol] = 0  // Backtrack
    
    return false

ORPHAN-DETECTED(count, row, col):
    if count < total - 1:
        for each neighbor in GET-VALID-NEIGHBORS(board, row, col):
            if COUNT-NEIGHBORS(neighbor) == 0:
                return true  // Neighbor would become orphan
    return false
```

### Warnsdorff's Heuristic

```
Principle: Always move to the square with the 
           fewest onward moves (most constrained)

GET-VALID-NEIGHBORS(board, row, col):
    neighbors = []
    for each (dx, dy) in MOVES:
        newRow = row + dy
        newCol = col + dx
        if IS-VALID(newRow, newCol) and board[newRow][newCol] == 0:
            count = COUNT-NEIGHBORS(newRow, newCol)
            neighbors.add((newRow, newCol, count))
    return neighbors
```

## Implementation Details

### Java Implementation

```java
public final class KnightsTour {
    // 12x12 grid with 2-cell border around 8x8 playing area
    private static final int BASE = 12;
    
    // All 8 possible knight moves
    private static final int[][] MOVES = {
        {1, -2}, {2, -1}, {2, 1}, {1, 2},
        {-1, 2}, {-2, 1}, {-2, -1}, {-1, -2},
    };

    static int[][] grid;
    static int total;  // Total cells to visit

    public static void resetBoard() {
        grid = new int[BASE][BASE];
        total = (BASE - 4) * (BASE - 4);  // 64 for standard board
        
        // Mark border cells as invalid (-1)
        for (int r = 0; r < BASE; r++) {
            for (int c = 0; c < BASE; c++) {
                if (r < 2 || r > BASE - 3 || c < 2 || c > BASE - 3) {
                    grid[r][c] = -1;
                }
            }
        }
    }
}
```

### Solve Method with Warnsdorff's Rule

```java
static boolean solve(int row, int column, int count) {
    if (count > total) {
        return true;  // Tour complete
    }

    List<int[]> neighbor = neighbors(row, column);

    if (neighbor.isEmpty() && count != total) {
        return false;  // Dead end before completion
    }

    // Warnsdorff's rule: sort by fewest onward moves
    neighbor.sort(Comparator.comparingInt(a -> a[2]));

    for (int[] nb : neighbor) {
        int nextRow = nb[0];
        int nextCol = nb[1];
        grid[nextRow][nextCol] = count;
        
        if (!orphanDetected(count, nextRow, nextCol) 
            && solve(nextRow, nextCol, count + 1)) {
            return true;
        }
        
        grid[nextRow][nextCol] = 0;  // Backtrack
    }

    return false;
}
```

### Orphan Detection

```java
static boolean orphanDetected(int count, int row, int column) {
    if (count < total - 1) {
        List<int[]> neighbor = neighbors(row, column);
        for (int[] nb : neighbor) {
            if (countNeighbors(nb[0], nb[1]) == 0) {
                return true;  // This neighbor has no escape
            }
        }
    }
    return false;
}
```

## Visualization

### 8×8 Knight's Tour Example

```
 1  38  55  34   3  36  19  22
54  47   2  37  20  23   4  17
39  56  33  46  35  18  21  10
48  53  40  57  24  11  16   5
59  32  45  52  41  26   9  12
44  49  58  25  62  15   6  27
31  60  51  42  29   8  13  64
50  43  30  61  14  63  28   7
```

### Board Representation

```
Border cells marked -1:
┌────────────────────────┐
│ -1 -1 -1 -1 ... -1 -1  │  (rows 0-1: border)
│ -1 -1 -1 -1 ... -1 -1  │
│ -1 -1  0  0 ...  0 -1  │  (rows 2-9: playable)
│ -1 -1  0  0 ...  0 -1  │
│  ...                   │
│ -1 -1 -1 -1 ... -1 -1  │  (rows 10-11: border)
│ -1 -1 -1 -1 ... -1 -1  │
└────────────────────────┘
```

## Optimization Techniques

### 1. Warnsdorff's Rule

- Choose squares with fewest exit moves
- Drastically reduces backtracking
- Nearly linear time in practice

### 2. Orphan Detection

- Detect cells that would become unreachable
- Prune branches early
- Prevents creating dead-end paths

### 3. Symmetry Exploitation

- Use board symmetry to reduce search space
- Mirror solutions for efficiency

### 4. Divide and Conquer

- For large boards, solve quadrants separately
- Merge solutions using connecting moves

## Real-World Applications

### 1. Puzzle Games
- Knight's tour puzzles in chess magazines
- Educational software

### 2. Memory Training
- Used in memory championship competitions
- Developing spatial reasoning

### 3. Algorithm Education
- Teaching backtracking concepts
- Demonstrating heuristic importance

### 4. Circuit Design
- VLSI routing problems
- Covering designs

## Common Pitfalls and Edge Cases

### 1. Board Border Handling

```
⚠️ Edge Case: Knight moves off board

Solution: Use border padding (-1 values)
- 12×12 grid with 2-cell border
- Simplifies boundary checking
```

### 2. Starting Position Matters

```
⚠️ Note: Some starting positions are harder

For 5×5 board:
- Corner starts: harder to solve
- Center starts: easier solutions
```

### 3. Incomplete Tours

```
❌ Pitfall: Getting stuck before visiting all squares

Without Warnsdorff's rule:
- Naive backtracking often fails
- Exponential time for large boards

With Warnsdorff's rule:
- Usually finds solution quickly
- Rare backtracking needed
```

### 4. Move Direction Order

```
⚠️ Note: Move order affects performance

Standard MOVES array order is optimized for:
- Clockwise traversal
- Works well with Warnsdorff's rule
```

### 5. Count Off-by-One

```
❌ Common Bug: Incorrect termination condition

// Wrong
if (count == total)  // Miss last cell

// Correct  
if (count > total)   // All 64 cells covered (1-64)
```

## Testing Strategies

### Test Cases

```java
@Test
void testBoardInitialization() {
    KnightsTour.resetBoard();
    assertEquals(-1, KnightsTour.grid[0][0]);  // Border
    assertEquals(0, KnightsTour.grid[2][2]);   // Playable
}

@Test
void testSolvableFromCorner() {
    KnightsTour.resetBoard();
    KnightsTour.grid[2][2] = 1;  // Start position
    assertTrue(KnightsTour.solve(2, 2, 2));
}

@Test
void testAllSquaresVisited() {
    KnightsTour.resetBoard();
    KnightsTour.grid[2][2] = 1;
    KnightsTour.solve(2, 2, 2);
    
    int visitedCount = 0;
    for (int r = 2; r < 10; r++) {
        for (int c = 2; c < 10; c++) {
            if (KnightsTour.grid[r][c] > 0) visitedCount++;
        }
    }
    assertEquals(64, visitedCount);
}
```

## Comparison with Related Problems

| Problem | Board | Constraint | Solution Type |
|---------|-------|------------|---------------|
| Knight's Tour | N×N | Knight moves | Hamiltonian path |
| N-Queens | N×N | No attacks | Configuration |
| Sudoku | 9×9 | Unique digits | Assignment |
| Maze | M×N | No walls | Path finding |

## Algorithm Variants

### Closed Tour Finding

```java
// Modify to find closed tour (can return to start)
boolean isClosedTour(int startRow, int startCol) {
    // After finding tour, check if last position 
    // can reach start with a knight move
    int lastRow = findLastPosition()[0];
    int lastCol = findLastPosition()[1];
    
    for (int[] move : MOVES) {
        if (lastRow + move[1] == startRow && 
            lastCol + move[0] == startCol) {
            return true;
        }
    }
    return false;
}
```

## References

### Academic
- Warnsdorff, H. C. von (1823). "Des Rösselsprunges einfachste und allgemeinste Lösung"
- Schwenk, A. J. (1991). "Which Rectangular Chessboards Have a Knight's Tour?"

### Related Algorithms
- [N-Queens](n-queens.md) - Board configuration problem
- [Maze Solver](maze-solver.md) - Path finding
- [Hamiltonian Path](https://en.wikipedia.org/wiki/Hamiltonian_path) - Graph theory

---

*Last updated: Phase 2 Documentation*
