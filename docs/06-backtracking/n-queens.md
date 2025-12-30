# N-Queens Problem

> **Category:** Algorithms > Backtracking > Combinatorial Problems
> **Implementation:** [NQueens.java](../../../src/main/java/com/thealgorithms/backtracking/NQueens.java)

## Overview

The **N-Queens Problem** is a classic combinatorial problem that asks how to place N chess queens on an N×N chessboard such that no two queens threaten each other. This means no two queens can share the same row, column, or diagonal.

The problem is a fundamental example of **backtracking** algorithms and constraint satisfaction problems (CSP).

### Problem Variants

1. **Decision Problem:** Can N queens be placed on an N×N board?
2. **Counting Problem:** How many solutions exist for N queens?
3. **Optimization Problem:** Find one valid arrangement

## Mathematical Foundation

### Constraint Formulation

For queens at positions $(r_1, c_1)$ and $(r_2, c_2)$:

**Row Constraint:**
$$r_1 \neq r_2$$

**Column Constraint:**
$$c_1 \neq c_2$$

**Diagonal Constraint:**
$$|r_1 - r_2| \neq |c_1 - c_2|$$

### Number of Solutions

| N | Solutions | Unique Solutions |
|---|-----------|------------------|
| 1 | 1 | 1 |
| 2 | 0 | 0 |
| 3 | 0 | 0 |
| 4 | 2 | 1 |
| 5 | 10 | 2 |
| 6 | 4 | 1 |
| 7 | 40 | 6 |
| 8 | 92 | 12 |

### Search Space

- **Total Configurations:** $N^N$ (placing one queen per row)
- **With Permutations:** $N!$ (one queen per row and column)
- **After Pruning:** Significantly less due to diagonal constraints

## Complexity Analysis

### Time Complexity

| Approach | Complexity |
|----------|------------|
| Brute Force | O(N!) |
| Backtracking | O(N!) worst case |
| Optimized (bitwise) | O(N!) with smaller constants |

### Space Complexity

| Component | Complexity |
|-----------|------------|
| Board representation | O(N) (1D array) |
| Recursion stack | O(N) |
| Solutions storage | O(S × N²) where S = number of solutions |

## Algorithm Pseudocode

### Backtracking Solution

```
N-QUEENS(n):
    solutions = []
    columns = new int[n]  // columns[i] = row where queen is in column i
    SOLVE(n, solutions, columns, 0)
    return solutions

SOLVE(n, solutions, columns, col):
    if col == n:
        // All queens placed successfully
        ADD-SOLUTION(solutions, columns, n)
        return
    
    for row = 0 to n - 1:
        columns[col] = row
        if IS-SAFE(columns, row, col):
            SOLVE(n, solutions, columns, col + 1)

IS-SAFE(columns, row, col):
    for i = 0 to col - 1:
        diff = |columns[i] - row|
        if diff == 0:        // Same row
            return false
        if col - i == diff:  // Same diagonal
            return false
    return true
```

### Step-by-Step Walkthrough (N=4)

```
Starting with empty board (column = 0):

Step 1: Try row 0 in column 0
[Q . . .]
[. . . .]
[. . . .]
[. . . .]
✓ Valid, proceed to column 1

Step 2: Try row 0 in column 1 - Conflicts (same row)
        Try row 1 in column 1 - Conflicts (diagonal)
        Try row 2 in column 1
[Q . . .]
[. . . .]
[. Q . .]
[. . . .]
✓ Valid, proceed to column 2

Step 3: Try row 0 in column 2 - Conflicts (diagonal)
        Try row 1 in column 2 - Conflicts (diagonal)
        Try row 2 in column 2 - Conflicts (same row)
        Try row 3 in column 2
[Q . . .]
[. . . .]
[. Q . .]
[. . Q .]
✗ No valid position for column 3, BACKTRACK

Step 4: Try row 3 in column 1
[Q . . .]
[. . . .]
[. . . .]
[. Q . .]
✓ Valid, proceed to column 2

Step 5: Try row 1 in column 2
[Q . . .]
[. . Q .]
[. . . .]
[. Q . .]
✓ Valid, proceed to column 3

Step 6: Try row 2 in column 3
[Q . . .]
[. . Q .]
[. . . Q]  (conflicts)
[. Q . .]

Continue until solution found:
[. Q . .]
[. . . Q]
[Q . . .]
[. . Q .]
```

## Implementation Details

### Java Implementation

```java
public final class NQueens {
    
    public static List<List<String>> getNQueensArrangements(int queens) {
        List<List<String>> arrangements = new ArrayList<>();
        getSolution(queens, arrangements, new int[queens], 0);
        return arrangements;
    }

    private static void getSolution(int boardSize, 
                                    List<List<String>> solutions, 
                                    int[] columns, 
                                    int columnIndex) {
        if (columnIndex == boardSize) {
            // All queens placed - build solution string
            List<String> sol = new ArrayList<String>();
            for (int i = 0; i < boardSize; i++) {
                StringBuilder sb = new StringBuilder();
                for (int j = 0; j < boardSize; j++) {
                    sb.append(j == columns[i] ? "Q" : ".");
                }
                sol.add(sb.toString());
            }
            solutions.add(sol);
            return;
        }

        // Try placing queen in each row of current column
        for (int rowIndex = 0; rowIndex < boardSize; rowIndex++) {
            columns[columnIndex] = rowIndex;
            if (isPlacedCorrectly(columns, rowIndex, columnIndex)) {
                getSolution(boardSize, solutions, columns, columnIndex + 1);
            }
        }
    }

    private static boolean isPlacedCorrectly(int[] columns, 
                                              int rowIndex, 
                                              int columnIndex) {
        for (int i = 0; i < columnIndex; i++) {
            int diff = Math.abs(columns[i] - rowIndex);
            if (diff == 0 || columnIndex - i == diff) {
                return false;
            }
        }
        return true;
    }
}
```

### Key Design Decisions

1. **1D Array Representation:** `columns[i]` stores the row of queen in column `i`
2. **Column-by-Column Placement:** Guarantees one queen per column
3. **Early Pruning:** Check constraints after each placement

## Optimization Techniques

### 1. Bitwise Representation

```java
// Using bits to track attacked columns and diagonals
void solveWithBits(int n, int row, int cols, int diag1, int diag2) {
    if (row == n) {
        count++;
        return;
    }
    
    int available = ((1 << n) - 1) & ~(cols | diag1 | diag2);
    while (available != 0) {
        int pos = available & -available;  // Get lowest set bit
        available -= pos;
        solveWithBits(n, row + 1, 
                      cols | pos, 
                      (diag1 | pos) << 1, 
                      (diag2 | pos) >> 1);
    }
}
```

### 2. Symmetry Reduction

- Only explore half the first row
- Mirror solutions for the other half
- Reduces search space by ~50%

### 3. Iterative Approach

- Explicit stack instead of recursion
- Better memory control for large N

## Real-World Applications

### 1. Constraint Satisfaction Problems
- Resource allocation
- Scheduling systems
- Configuration management

### 2. Parallel Computing
- Load balancing across processors
- Task assignment without conflicts

### 3. Circuit Design
- VLSI component placement
- Avoiding interference patterns

### 4. Computer Vision
- Feature detection
- Non-maximum suppression patterns

## Comparison with Other Approaches

| Approach | Time | Space | Pros | Cons |
|----------|------|-------|------|------|
| Brute Force | O(N^N) | O(N²) | Simple | Extremely slow |
| Backtracking | O(N!) | O(N) | Efficient pruning | Still exponential |
| Genetic Algorithm | Variable | O(P×N) | Good for large N | Approximate |
| Min-Conflicts | O(N²) avg | O(N) | Fast for large N | May not find all |

## Common Pitfalls and Edge Cases

### 1. N = 2 or 3 (No Solution)

```
⚠️ Edge Case: Small boards with no solution

N = 2: Any queen placement attacks all other cells
N = 3: No valid configuration exists

Handle gracefully:
if (arrangements.isEmpty()) {
    System.out.println("No solution exists for N=" + queens);
}
```

### 2. Diagonal Check Error

```
❌ Common Bug: Incorrect diagonal detection

// Wrong: Only checks one diagonal
if (columns[i] == rowIndex + (columnIndex - i))

// Correct: Check both diagonals
int diff = Math.abs(columns[i] - rowIndex);
if (columnIndex - i == diff)  // Works for both diagonals
```

### 3. Off-by-One Errors

```
❌ Pitfall: Incorrect loop bounds

// Wrong
for (int i = 0; i <= columnIndex; i++)  // Checks against self

// Correct
for (int i = 0; i < columnIndex; i++)   // Only previous columns
```

### 4. Solution Uniqueness

```
⚠️ Note: Symmetrical solutions may or may not be counted separately

- Rotations (4 per solution)
- Reflections (2 per rotation)
- Up to 8 equivalent solutions may exist
```

## Testing Strategies

### Unit Test Cases

```java
@Test
void testNoSolutionForSmallBoards() {
    assertEquals(0, NQueens.getNQueensArrangements(2).size());
    assertEquals(0, NQueens.getNQueensArrangements(3).size());
}

@Test
void testKnownSolutionCounts() {
    assertEquals(1, NQueens.getNQueensArrangements(1).size());
    assertEquals(2, NQueens.getNQueensArrangements(4).size());
    assertEquals(10, NQueens.getNQueensArrangements(5).size());
    assertEquals(92, NQueens.getNQueensArrangements(8).size());
}

@Test
void testSolutionValidity() {
    List<List<String>> solutions = NQueens.getNQueensArrangements(8);
    for (List<String> solution : solutions) {
        assertTrue(isValidSolution(solution));
    }
}
```

## References

### Academic
- Dijkstra, E. W. (1972). "EWD 316: A Short Introduction to the Art of Programming"
- Bitner & Reingold (1975). "Backtrack Programming Techniques"

### Related Algorithms
- [Sudoku Solver](sudoku-solver.md) - Constraint satisfaction with backtracking
- [Knight's Tour](knights-tour.md) - Board traversal problem
- [M-Coloring](m-coloring.md) - Graph coloring with backtracking

---

*Last updated: Phase 2 Documentation*
