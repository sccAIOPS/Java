# Sudoku Solver

> **Category:** Algorithms > Backtracking > Constraint Satisfaction
> **Implementation:** [SudokuSolver.java](../../../src/main/java/com/thealgorithms/backtracking/SudokuSolver.java)

## Overview

A **Sudoku Solver** uses backtracking to fill a 9×9 grid with digits 1-9 such that each column, each row, and each of the nine 3×3 subgrids contain all digits from 1 to 9 exactly once.

Sudoku solving is a classic example of a **Constraint Satisfaction Problem (CSP)** that can be efficiently solved using backtracking with constraint propagation.

### Problem Definition

Given a partially filled 9×9 Sudoku grid, fill in the empty cells such that:
1. Each row contains digits 1-9 exactly once
2. Each column contains digits 1-9 exactly once
3. Each 3×3 subgrid contains digits 1-9 exactly once

## Mathematical Foundation

### Constraint Formulation

For a cell at position $(r, c)$ with value $v$:

**Row Constraint:**
$$\forall j \neq c: grid[r][j] \neq v$$

**Column Constraint:**
$$\forall i \neq r: grid[i][c] \neq v$$

**Subgrid Constraint:**
$$\forall (i, j) \in \text{subgrid}(r, c), (i,j) \neq (r,c): grid[i][j] \neq v$$

Where $\text{subgrid}(r, c)$ is the 3×3 box containing cell $(r, c)$.

### Subgrid Calculation

```
subgridRowStart = r - (r mod 3)
subgridColStart = c - (c mod 3)
```

### Search Space

- **Theoretical Maximum:** $9^{81}$ (9 choices for 81 cells)
- **With Constraints:** Much smaller due to constraint propagation
- **Typical:** Well-posed puzzles have unique solutions

## Complexity Analysis

### Time Complexity

| Scenario | Complexity |
|----------|------------|
| Worst Case | O(9^n) where n = empty cells |
| Typical (valid puzzle) | O(9^m) where m << n |
| With constraint propagation | Much better in practice |

### Space Complexity

| Component | Complexity |
|-----------|------------|
| Grid storage | O(81) = O(1) |
| Recursion stack | O(81) = O(1) |
| Total | O(1) - fixed size problem |

## Algorithm Pseudocode

### Basic Backtracking

```
SOLVE-SUDOKU(board):
    return SOLVE(board)

SOLVE(board):
    for row = 0 to 8:
        for col = 0 to 8:
            if board[row][col] == EMPTY:
                for num = 1 to 9:
                    if IS-VALID-PLACEMENT(board, row, col, num):
                        board[row][col] = num
                        
                        if SOLVE(board):
                            return true
                        
                        board[row][col] = EMPTY  // Backtrack
                
                return false  // No valid number found
    
    return true  // All cells filled successfully

IS-VALID-PLACEMENT(board, row, col, num):
    return not IS-IN-ROW(board, row, num)
       and not IS-IN-COLUMN(board, col, num)
       and not IS-IN-SUBGRID(board, row, col, num)
```

### Step-by-Step Walkthrough

```
Initial State:
┌───────┬───────┬───────┐
│ 5 3 . │ . 7 . │ . . . │
│ 6 . . │ 1 9 5 │ . . . │
│ . 9 8 │ . . . │ . 6 . │
├───────┼───────┼───────┤
│ 8 . . │ . 6 . │ . . 3 │
│ 4 . . │ 8 . 3 │ . . 1 │
│ 7 . . │ . 2 . │ . . 6 │
├───────┼───────┼───────┤
│ . 6 . │ . . . │ 2 8 . │
│ . . . │ 4 1 9 │ . . 5 │
│ . . . │ . 8 . │ . 7 9 │
└───────┴───────┴───────┘

Finding first empty cell: (0, 2)

Try 1: Check row 0 - not present
       Check col 2 - 8 present in row 2
       Check subgrid - not present
       1 is invalid (8 conflicts? No, 1 is valid)

Actually checking (0,2):
- Row 0: [5,3,_,_,7,_,_,_,_] - 1,2,4,6,8,9 available
- Col 2: [_,_,8,_,_,_,_,_,_] - 8 present
- Subgrid: [5,3,_,6,_,_,_,9,8] - 1,2,4,7 available

Valid candidates for (0,2): {1, 2, 4}

Try 1: Place 1 at (0,2), recurse...
       If success: done
       If fail: backtrack, try 2
```

## Implementation Details

### Java Implementation

```java
public final class SudokuSolver {
    private static final int GRID_SIZE = 9;
    private static final int SUBGRID_SIZE = 3;
    private static final int EMPTY_CELL = 0;

    public static boolean solveSudoku(int[][] board) {
        if (board == null || board.length != GRID_SIZE) {
            return false;
        }
        for (int row = 0; row < GRID_SIZE; row++) {
            if (board[row].length != GRID_SIZE) {
                return false;
            }
        }
        return solve(board);
    }

    private static boolean solve(int[][] board) {
        for (int row = 0; row < GRID_SIZE; row++) {
            for (int col = 0; col < GRID_SIZE; col++) {
                if (board[row][col] == EMPTY_CELL) {
                    for (int number = 1; number <= GRID_SIZE; number++) {
                        if (isValidPlacement(board, row, col, number)) {
                            board[row][col] = number;
                            if (solve(board)) {
                                return true;
                            }
                            board[row][col] = EMPTY_CELL; // Backtrack
                        }
                    }
                    return false; // No valid number found
                }
            }
        }
        return true; // Puzzle solved
    }
}
```

### Constraint Checking

```java
private static boolean isValidPlacement(int[][] board, int row, 
                                         int col, int number) {
    return !isNumberInRow(board, row, number)
        && !isNumberInColumn(board, col, number)
        && !isNumberInSubgrid(board, row, col, number);
}

private static boolean isNumberInSubgrid(int[][] board, int row, 
                                          int col, int number) {
    int subgridRowStart = row - row % SUBGRID_SIZE;
    int subgridColStart = col - col % SUBGRID_SIZE;

    for (int i = subgridRowStart; i < subgridRowStart + SUBGRID_SIZE; i++) {
        for (int j = subgridColStart; j < subgridColStart + SUBGRID_SIZE; j++) {
            if (board[i][j] == number) {
                return true;
            }
        }
    }
    return false;
}
```

## Optimization Techniques

### 1. Constraint Propagation

```
For each empty cell, maintain set of possible values.
When a value is placed:
  - Remove it from row/column/subgrid possibilities
  - If any cell has single possibility, fill it (Naked Single)
  - If value possible in only one cell of unit, fill it (Hidden Single)
```

### 2. Most Constrained Variable (MRV)

```java
// Choose cell with fewest possibilities
int[] findMostConstrainedCell(int[][] board) {
    int minChoices = 10;
    int[] bestCell = null;
    
    for (int r = 0; r < 9; r++) {
        for (int c = 0; c < 9; c++) {
            if (board[r][c] == 0) {
                int choices = countPossibilities(board, r, c);
                if (choices < minChoices) {
                    minChoices = choices;
                    bestCell = new int[]{r, c};
                }
            }
        }
    }
    return bestCell;
}
```

### 3. Bitmask Representation

```java
// Use bits to represent available numbers
int rowMask = 0, colMask = 0, boxMask = 0;
for (int i = 0; i < 9; i++) {
    if (board[row][i] != 0) rowMask |= (1 << board[row][i]);
    if (board[i][col] != 0) colMask |= (1 << board[i][col]);
}
// Available numbers: ~(rowMask | colMask | boxMask) & 0x3FE
```

## Real-World Applications

### 1. Constraint Satisfaction Frameworks
- General CSP solvers
- SAT problem encoding

### 2. Puzzle Generation
- Generating valid Sudoku puzzles
- Difficulty rating algorithms

### 3. Educational Tools
- Teaching backtracking algorithms
- Demonstrating constraint propagation

### 4. Benchmark Problems
- Algorithm performance testing
- Optimization technique comparison

## Difficulty Levels

| Level | Empty Cells | Backtracking Steps |
|-------|-------------|-------------------|
| Easy | 30-40 | Few or none |
| Medium | 40-50 | Moderate |
| Hard | 50-55 | Many |
| Expert | 55-60 | Extensive |

## Common Pitfalls and Edge Cases

### 1. Invalid Input Handling

```java
⚠️ Edge Case: Invalid board dimensions or values

public static boolean solveSudoku(int[][] board) {
    if (board == null || board.length != GRID_SIZE) {
        return false;
    }
    for (int row = 0; row < GRID_SIZE; row++) {
        if (board[row].length != GRID_SIZE) {
            return false;
        }
    }
    return solve(board);
}
```

### 2. Already Invalid Board

```
❌ Pitfall: Not checking initial board validity

Before solving, verify:
- No duplicate numbers in any row
- No duplicate numbers in any column
- No duplicate numbers in any subgrid
```

### 3. No Solution Exists

```
⚠️ Edge Case: Unsolvable puzzle

Some configurations have no solution:
- Return false when all possibilities exhausted
- May indicate invalid initial configuration
```

### 4. Multiple Solutions

```
⚠️ Edge Case: Non-unique solution

Standard Sudoku has unique solution, but:
- Puzzle with too few clues may have multiple
- Modify algorithm to find all or count solutions
```

### 5. Subgrid Index Calculation

```
❌ Common Bug: Off-by-one in subgrid calculation

// Wrong: Integer division rounds incorrectly
int subgridRow = row / 3;  // This is the subgrid index, not start

// Correct: Calculate starting position
int subgridRowStart = row - row % SUBGRID_SIZE;
int subgridColStart = col - col % SUBGRID_SIZE;
```

## Testing Strategies

### Test Cases

```java
@Test
void testSolvableBoard() {
    int[][] board = {
        {5,3,0,0,7,0,0,0,0},
        {6,0,0,1,9,5,0,0,0},
        // ... rest of board
    };
    assertTrue(SudokuSolver.solveSudoku(board));
    assertTrue(isValidSolution(board));
}

@Test
void testInvalidBoard() {
    int[][] board = new int[8][9];  // Wrong dimensions
    assertFalse(SudokuSolver.solveSudoku(board));
}

@Test
void testAlreadySolved() {
    int[][] solved = getValidSolvedBoard();
    assertTrue(SudokuSolver.solveSudoku(solved));
}
```

## Algorithm Variants

### Dancing Links (DLX)

- Represents Sudoku as exact cover problem
- Uses Knuth's Algorithm X
- Very efficient for solving

### SAT Solver Encoding

- Convert to Boolean satisfiability
- Use modern SAT solvers
- Can prove uniqueness

## References

### Academic
- Norvig, P. (2006). "Solving Every Sudoku Puzzle"
- Knuth, D. E. "Dancing Links"

### Related Algorithms
- [N-Queens](n-queens.md) - Similar backtracking approach
- [M-Coloring](m-coloring.md) - Graph constraint satisfaction
- [Maze Solver](maze-solver.md) - Path-finding with backtracking

---

*Last updated: Phase 2 Documentation*
