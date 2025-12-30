# Flood Fill Algorithm

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Graphics/Image Processing  
> **Implementation:** [`IterativeFloodFill.java`](../../src/main/java/com/thealgorithms/others/IterativeFloodFill.java)

---

## 📚 Overview

Flood Fill is an algorithm that determines and changes the area connected to a given node in a multi-dimensional array. It's commonly used in paint programs (bucket fill tool), maze solving, and image processing.

**Key Characteristics:**
- Explores connected regions
- Can be implemented recursively or iteratively
- BFS or DFS based
- 4-connected or 8-connected neighbors

---

## 🔢 Algorithm Foundation

### Connectivity Types

**4-Connected:** Only horizontal/vertical neighbors
```
    N
  W X E
    S
```

**8-Connected:** Including diagonal neighbors
```
  NW N NE
  W  X  E
  SW S SE
```

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(N) where N = pixels in region |
| Space (recursive) | O(N) stack |
| Space (iterative) | O(N) queue/stack |

---

## 🔄 Algorithm (Pseudocode)

### Recursive Version (DFS)
```
ALGORITHM FloodFillRecursive(image, x, y, targetColor, fillColor)
─────────────────────────────────────────────────────
    IF x < 0 OR x >= width THEN RETURN
    IF y < 0 OR y >= height THEN RETURN
    IF image[x][y] ≠ targetColor THEN RETURN
    IF targetColor = fillColor THEN RETURN
    
    image[x][y] ← fillColor
    
    FloodFillRecursive(image, x+1, y, targetColor, fillColor)
    FloodFillRecursive(image, x-1, y, targetColor, fillColor)
    FloodFillRecursive(image, x, y+1, targetColor, fillColor)
    FloodFillRecursive(image, x, y-1, targetColor, fillColor)
```

### Iterative Version (BFS)
```
ALGORITHM FloodFillIterative(image, x, y, targetColor, fillColor)
─────────────────────────────────────────────────────
    IF image[x][y] ≠ targetColor THEN RETURN
    IF targetColor = fillColor THEN RETURN
    
    queue ← new Queue()
    queue.enqueue((x, y))
    
    WHILE queue NOT empty DO
        (cx, cy) ← queue.dequeue()
        
        IF cx < 0 OR cx >= width THEN CONTINUE
        IF cy < 0 OR cy >= height THEN CONTINUE
        IF image[cx][cy] ≠ targetColor THEN CONTINUE
        
        image[cx][cy] ← fillColor
        
        queue.enqueue((cx+1, cy))
        queue.enqueue((cx-1, cy))
        queue.enqueue((cx, cy+1))
        queue.enqueue((cx, cy-1))
    END WHILE
```

### Step-by-Step Example

**Initial Grid (target=1, fill=2):**
```
0 0 0 0 0
0 1 1 1 0
0 1 1 0 0
0 1 0 0 0
0 0 0 0 0
```

**Start at (1,1):**
```
Step 1: Fill (1,1)    Step 2: Fill neighbors
0 0 0 0 0             0 0 0 0 0
0 2 1 1 0             0 2 2 1 0
0 1 1 0 0             0 2 2 0 0
0 1 0 0 0             0 2 0 0 0
0 0 0 0 0             0 0 0 0 0
```

**Final Result:**
```
0 0 0 0 0
0 2 2 2 0
0 2 2 0 0
0 2 0 0 0
0 0 0 0 0
```

---

## 💻 Implementation Notes

### Java Implementation (Iterative BFS)

```java
public class FloodFill {
    
    private static final int[] DX = {1, -1, 0, 0};
    private static final int[] DY = {0, 0, 1, -1};
    
    public static void fill(int[][] image, int x, int y, int newColor) {
        int rows = image.length;
        int cols = image[0].length;
        int targetColor = image[x][y];
        
        if (targetColor == newColor) return;
        
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{x, y});
        
        while (!queue.isEmpty()) {
            int[] cell = queue.poll();
            int cx = cell[0];
            int cy = cell[1];
            
            // Skip if out of bounds or already processed
            if (cx < 0 || cx >= rows || cy < 0 || cy >= cols) continue;
            if (image[cx][cy] != targetColor) continue;
            
            // Fill current cell
            image[cx][cy] = newColor;
            
            // Add neighbors
            for (int i = 0; i < 4; i++) {
                queue.offer(new int[]{cx + DX[i], cy + DY[i]});
            }
        }
    }
    
    // 8-connected version
    public static void fill8Connected(int[][] image, int x, int y, int newColor) {
        int[] dx = {1, -1, 0, 0, 1, 1, -1, -1};
        int[] dy = {0, 0, 1, -1, 1, -1, 1, -1};
        // Similar implementation with 8 directions
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/IterativeFloodFill.java`](../../src/main/java/com/thealgorithms/others/IterativeFloodFill.java)

---

## 🌍 Real-World Applications

### 1. Image Editing
**Use Case:** Paint bucket tool in Photoshop, MS Paint

### 2. Game Development
**Use Case:** Minesweeper uncovering, maze filling

### 3. Image Processing
**Use Case:** Region segmentation, object detection

### 4. Geographic Information Systems
**Use Case:** Identifying connected land masses

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| Boundary Fill | Fill until boundary color | Outlined regions |
| Scanline Fill | Line-by-line filling | More efficient |
| Pattern Fill | Fill with pattern | Textured fills |

---

## ⚖️ BFS vs DFS

| Aspect | BFS (Queue) | DFS (Stack/Recursion) |
|--------|-------------|----------------------|
| Fill pattern | Outward rings | Depth first |
| Memory | More for wide regions | Risk of stack overflow |
| Implementation | Iterative | Recursive or iterative |

---

## 🚨 Stack Overflow Prevention

Large regions can cause stack overflow with recursion:

```java
// Recursive - risky for large images
void floodFillRecursive(int[][] image, int x, int y, int target, int fill) {
    // May overflow for large connected regions
}

// Iterative - safe
void floodFillIterative(int[][] image, int x, int y, int target, int fill) {
    // Uses explicit stack/queue
}
```

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Same color fill | Infinite loop | Check target ≠ fill |
| Stack overflow | Large regions | Use iterative |
| Boundary check | ArrayIndexOutOfBounds | Validate coordinates |
| Revisiting | Performance | Mark visited cells |

---

## 📖 References

1. **"Computer Graphics: Principles and Practice"** - Foley et al.
2. **"Image Processing with Flood Fill"** - ACM Graphics

---

## 🔗 Related Algorithms

- [BFS](../../02-searching-algorithms/bfs.md)
- [DFS](../../02-searching-algorithms/dfs.md)
- [Connected Components](../../05-graph-algorithms/README.md)

---

*Last updated: December 30, 2025*
