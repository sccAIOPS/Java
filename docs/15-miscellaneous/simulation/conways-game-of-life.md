# Conway's Game of Life

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Cellular Automata  
> **Implementation:** [`Conway.java`](../../src/main/java/com/thealgorithms/others/Conway.java)

---

## 📚 Overview

Conway's Game of Life is a cellular automaton devised by mathematician John Conway in 1970. It's a zero-player game that evolves based on its initial state, requiring no further input. The game demonstrates how complex patterns can emerge from simple rules.

**Key Characteristics:**
- Turing complete
- Zero-player game (evolution follows rules)
- Emergent behavior from simple rules
- Demonstrates self-organization

---

## 🔢 Rules

For each cell in the grid:

### Birth
A dead cell with **exactly 3** live neighbors becomes alive.

### Survival
A live cell with **2 or 3** live neighbors survives.

### Death
- **Underpopulation:** Live cell with < 2 neighbors dies
- **Overpopulation:** Live cell with > 3 neighbors dies

### Summary Table

| Live Neighbors | Dead Cell → | Live Cell → |
|----------------|-------------|-------------|
| 0-1 | Dead | Dead (underpopulation) |
| 2 | Dead | Alive (survives) |
| 3 | Alive (birth) | Alive (survives) |
| 4+ | Dead | Dead (overpopulation) |

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time (per generation) | O(n × m) |
| Space | O(n × m) |

Where n × m = grid dimensions.

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM GameOfLife(grid)
─────────────────────────────────────────────────────
    INPUT:  grid - 2D array of cells (0=dead, 1=alive)
    OUTPUT: next generation grid
─────────────────────────────────────────────────────

    rows ← grid.rows
    cols ← grid.cols
    newGrid ← copy of grid
    
    FOR x ← 0 TO rows - 1 DO
        FOR y ← 0 TO cols - 1 DO
            neighbors ← countLiveNeighbors(grid, x, y)
            
            IF grid[x][y] = 1 THEN  // Live cell
                IF neighbors < 2 OR neighbors > 3 THEN
                    newGrid[x][y] ← 0  // Dies
                END IF
            ELSE  // Dead cell
                IF neighbors = 3 THEN
                    newGrid[x][y] ← 1  // Birth
                END IF
            END IF
        END FOR
    END FOR
    
    RETURN newGrid

FUNCTION countLiveNeighbors(grid, x, y)
    count ← 0
    FOR dx ← -1 TO 1 DO
        FOR dy ← -1 TO 1 DO
            IF dx = 0 AND dy = 0 THEN CONTINUE
            nx ← x + dx
            ny ← y + dy
            IF inBounds(nx, ny) AND grid[nx][ny] = 1 THEN
                count ← count + 1
            END IF
        END FOR
    END FOR
    RETURN count
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class GameOfLife {
    private int[][] grid;
    private int rows, cols;
    
    public GameOfLife(int rows, int cols) {
        this.rows = rows;
        this.cols = cols;
        this.grid = new int[rows][cols];
    }
    
    public void nextGeneration() {
        int[][] newGrid = new int[rows][cols];
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                int neighbors = countNeighbors(i, j);
                
                if (grid[i][j] == 1) {
                    // Live cell
                    newGrid[i][j] = (neighbors == 2 || neighbors == 3) ? 1 : 0;
                } else {
                    // Dead cell
                    newGrid[i][j] = (neighbors == 3) ? 1 : 0;
                }
            }
        }
        
        grid = newGrid;
    }
    
    private int countNeighbors(int x, int y) {
        int count = 0;
        int[] dx = {-1, -1, -1, 0, 0, 1, 1, 1};
        int[] dy = {-1, 0, 1, -1, 1, -1, 0, 1};
        
        for (int i = 0; i < 8; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx >= 0 && nx < rows && ny >= 0 && ny < cols) {
                count += grid[nx][ny];
            }
        }
        
        return count;
    }
    
    // In-place version using bit manipulation
    public void nextGenerationInPlace() {
        // Use bit 1 for current state, bit 2 for next state
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                int neighbors = countNeighborsLowBit(i, j);
                
                if (grid[i][j] == 1 && (neighbors == 2 || neighbors == 3)) {
                    grid[i][j] = 3;  // 11: alive now and next
                } else if (grid[i][j] == 0 && neighbors == 3) {
                    grid[i][j] = 2;  // 10: dead now, alive next
                }
            }
        }
        
        // Shift to get next state
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                grid[i][j] >>= 1;
            }
        }
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/Conway.java`](../../src/main/java/com/thealgorithms/others/Conway.java)

---

## 🎮 Common Patterns

### Still Lifes (Stable)
```
Block:     Beehive:      Loaf:
##         .##.          .##.
##         #..#          #..#
           .##.          .#.#
                         ..#.
```

### Oscillators
```
Blinker (period 2):
.#.    →    ...
.#.    →    ###
.#.    →    ...
```

### Spaceships (Movers)
```
Glider (moves diagonally):
.#.    .#.    ..#    ...    #..
..#    #.#    ..#    #.#    .##
###    .##    .##    ..##   #.#
```

### Methuselahs
Patterns that evolve for many generations before stabilizing:
- **R-pentomino:** Stabilizes after 1103 generations
- **Acorn:** Stabilizes after 5206 generations

---

## 🌍 Real-World Applications

### 1. Computer Science Education
**Use Case:** Teaching cellular automata, emergence

### 2. Artificial Life Research
**Use Case:** Studying self-replication, evolution

### 3. Cryptography
**Use Case:** Pattern-based random number generation

### 4. Graphics/Art
**Use Case:** Procedural content generation

---

## 🔬 Theoretical Significance

### Turing Completeness
The Game of Life can simulate any Turing machine, meaning it can compute anything computable.

### Universal Constructor
Paul Rendell (2000) built a pattern that can construct any other pattern (including copies of itself).

### Speed of Light
In Life, the maximum speed information can travel is 1 cell per generation (the "speed of light" c).

---

## ⚖️ Boundary Conditions

| Boundary | Description |
|----------|-------------|
| Dead edges | Cells outside grid are dead |
| Toroidal | Grid wraps around (donut shape) |
| Infinite | Expandable grid |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Modifying in place | Corrupts neighbor count | Use copy or bit encoding |
| Edge handling | Missing neighbors | Consistent boundary rules |
| Infinite growth | Memory overflow | Use sparse representation |
| Performance | Slow for large grids | Use HashLife algorithm |

---

## 📖 References

1. **"The Recursive Universe"** - Poundstone
2. **"Winning Ways for Your Mathematical Plays"** - Berlekamp et al.
3. **LifeWiki** - conwaylife.com/wiki

---

## 🔗 Related Algorithms

- [Flood Fill](./graphics/flood-fill.md)
- [Cellular Automata](./cellular-automata.md)
- [Agent-Based Modeling](./agent-based-modeling.md)

---

*Last updated: December 30, 2025*
