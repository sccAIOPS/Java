# Closest Pair of Points

> **Category:** Divide and Conquer  
> **Subcategory:** Computational Geometry  
> **Implementation:** [`ClosestPair.java`](../../src/main/java/com/thealgorithms/divideandconquer/ClosestPair.java)

---

## 📚 Overview

The Closest Pair of Points problem finds the two points with the smallest Euclidean distance among a set of points in a plane. While a naive approach takes O(n²) time, the divide-and-conquer algorithm achieves O(n log n) time complexity, making it efficient for large point sets.

**Key Characteristics:**
- Divide and conquer achieves O(n log n) time
- Critical insight: strip checking is O(n), not O(n²)
- Foundation for computational geometry algorithms
- Applications in clustering, collision detection, graphics

---

## 🔢 Mathematical Foundation

### Definition

> **Formal Definition:** Given a set $P = \{p_1, p_2, ..., p_n\}$ of n points in 2D plane, find points $p_i, p_j$ such that $d(p_i, p_j) = \min_{k \neq l} d(p_k, p_l)$, where $d$ is Euclidean distance.

### Key Properties

| Property | Description | Formula |
|----------|-------------|---------|
| Euclidean Distance | Distance between two points | $d(p_i, p_j) = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$ |
| Strip Width | Width of middle strip | $2\delta$ where $\delta$ = min from subproblems |
| Strip Points | At most 6 points per strip point | Packing argument |

### Mathematical Formulation

**Euclidean Distance:**

$$
d(p_i, p_j) = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}
$$

**Divide and Conquer:**
1. Divide points by median x-coordinate
2. Solve recursively for left and right halves
3. $\delta = \min(\delta_L, \delta_R)$
4. Check strip of width $2\delta$ around dividing line

**Strip Property:**
For each point in the strip, only need to check at most 7 subsequent points (when sorted by y-coordinate).

### Proof of Strip Property

**Theorem:** Each point in the strip needs to be compared with at most 7 other points.

**Proof:**
1. Consider a $\delta \times 2\delta$ rectangle in the strip, split across the dividing line.
2. Each half ($\delta \times \delta$) can contain at most 4 points (by distance constraint).
3. Total: at most 8 points in the rectangle.
4. For a point $p$, only compare with points whose y-coordinate differs by at most $\delta$.
5. At most 7 such points (excluding $p$ itself). ∎

---

## 📊 Complexity Analysis

### Time Complexity

| Case | Complexity | When it occurs |
|------|------------|----------------|
| **All Cases** | $O(n \log n)$ | With proper implementation |

### Space Complexity

| Type | Complexity | Notes |
|------|------------|-------|
| **Auxiliary** | $O(n)$ | Sorted arrays |
| **Recursive Stack** | $O(\log n)$ | Recursion depth |

### Additional Properties

| Property | Value |
|----------|-------|
| **Deterministic** | Yes |
| **Optimal** | Yes (for comparison-based) |
| **Extends to Higher Dimensions** | Yes, with different constants |

### Detailed Analysis

**Recurrence Relation:**

$$
T(n) = 2T(n/2) + O(n)
$$

By Master Theorem: $T(n) = O(n \log n)$

**Where O(n) comes from:**
- Merging sorted arrays: O(n)
- Building strip: O(n)
- Checking strip: O(n) (at most 7 comparisons per point)

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM Closest-Pair(points)
─────────────────────────────────────────────────────
    INPUT:  Array of n points with (x, y) coordinates
    OUTPUT: Minimum distance and the two closest points
─────────────────────────────────────────────────────

    1. Px ← points sorted by x-coordinate
    2. Py ← points sorted by y-coordinate
    3. RETURN Closest-Pair-Rec(Px, Py)


ALGORITHM Closest-Pair-Rec(Px, Py)
─────────────────────────────────────────────────────

    1. n ← |Px|
    2. IF n ≤ 3 THEN
    3.     RETURN Brute-Force(Px)
    4. END IF
    
    5. // Divide
    6. mid ← n / 2
    7. midPoint ← Px[mid]
    8.
    9. // Split points maintaining y-sort
    10. (Qx, Qy) ← left half
    11. (Rx, Ry) ← right half
    
    12. // Conquer
    13. (δL, pairL) ← Closest-Pair-Rec(Qx, Qy)
    14. (δR, pairR) ← Closest-Pair-Rec(Rx, Ry)
    
    15. δ ← min(δL, δR)
    16. bestPair ← (δL < δR) ? pairL : pairR
    
    17. // Combine - Check strip
    18. strip ← points in Py with |x - midPoint.x| < δ
    19. (δS, pairS) ← Strip-Closest(strip, δ)
    
    20. IF δS < δ THEN
    21.     RETURN (δS, pairS)
    22. ELSE
    23.     RETURN (δ, bestPair)
    24. END IF


ALGORITHM Strip-Closest(strip, δ)
─────────────────────────────────────────────────────

    1. minDist ← δ
    2. closestPair ← null
    3. 
    4. FOR i ← 0 TO |strip| - 1 DO
    5.     // Only check next 7 points (y-sorted)
    6.     FOR j ← i + 1 TO min(i + 7, |strip| - 1) DO
    7.         IF strip[j].y - strip[i].y ≥ minDist THEN
    8.             BREAK  // No closer points possible
    9.         END IF
    10.        d ← distance(strip[i], strip[j])
    11.        IF d < minDist THEN
    12.            minDist ← d
    13.            closestPair ← (strip[i], strip[j])
    14.        END IF
    15.    END FOR
    16. END FOR
    17. RETURN (minDist, closestPair)
```

### Step-by-Step Walkthrough

**Example Input:** Points = {(2,3), (12,30), (40,50), (5,1), (12,10), (3,4)}

**Step 1: Sort by x-coordinate**
Px = [(2,3), (3,4), (5,1), (12,10), (12,30), (40,50)]

**Step 2: Divide**
- Left half: [(2,3), (3,4), (5,1)]
- Right half: [(12,10), (12,30), (40,50)]

**Step 3: Solve Left (Brute Force for n≤3)**
- d(2,3)(3,4) = √2 ≈ 1.41
- d(2,3)(5,1) = √13 ≈ 3.61
- d(3,4)(5,1) = √13 ≈ 3.61
- δL = √2, pairL = ((2,3), (3,4))

**Step 4: Solve Right (Brute Force)**
- d(12,10)(12,30) = 20
- d(12,10)(40,50) = √2384 ≈ 48.8
- d(12,30)(40,50) = √1184 ≈ 34.4
- δR = 20, pairR = ((12,10), (12,30))

**Step 5: Combine**
- δ = min(√2, 20) = √2 ≈ 1.41
- Strip center: x = 5 (or between 5 and 12)
- Strip width: 2√2 ≈ 2.83
- No points from right half within strip

**Result:** Closest pair = ((2,3), (3,4)), Distance = √2

**Visual Representation:**
```
y
50│                                    *
  │                                 (40,50)
30│              *
  │           (12,30)
  │
10│              *
  │           (12,10)
 4│   *
  │ (3,4)
 3│ *
  │(2,3)
 1│        *
  │     (5,1)
  └────┬────┬────┬────┬────┬────┬────┬─→ x
       2    5   12                  40
       
       │←─δ─→│
       Strip
```

---

## 💻 Implementation Notes

### Java Implementation Highlights
- Uses custom Point class or coordinate arrays
- Pre-sorts points by x and y coordinates
- Uses quick sort variants for initial sorting
- Brute force for base case (n ≤ 3)

### Code Reference
📁 **File:** `src/main/java/com/thealgorithms/divideandconquer/ClosestPair.java`

```java
// Key code snippet - Closest Pair
public double closestPair(Point[] points) {
    int n = points.length;
    
    // Sort by x-coordinate
    xQuickSort(points, 0, n - 1);
    
    return closestPairRec(points, 0, n - 1);
}

private double closestPairRec(Point[] points, int low, int high) {
    if (high - low <= 2) {
        return bruteForce(points, low, high);
    }
    
    int mid = (low + high) / 2;
    Point midPoint = points[mid];
    
    double dl = closestPairRec(points, low, mid);
    double dr = closestPairRec(points, mid + 1, high);
    double d = Math.min(dl, dr);
    
    // Build strip
    List<Point> strip = new ArrayList<>();
    for (int i = low; i <= high; i++) {
        if (Math.abs(points[i].x - midPoint.x) < d) {
            strip.add(points[i]);
        }
    }
    
    // Sort strip by y and check
    Collections.sort(strip, (a, b) -> Double.compare(a.y, b.y));
    
    return Math.min(d, stripClosest(strip, d));
}
```

---

## 🌍 Real-World Applications in Software Engineering

### 1. Collision Detection
**Use Case:** Detecting nearby objects in games/simulations  
**Example:** Physics engines detecting potential collisions

### 2. Geographic Information Systems (GIS)
**Use Case:** Finding nearest facilities, points of interest  
**Example:** Finding closest restaurant, hospital

### 3. Clustering Algorithms
**Use Case:** Initial step in hierarchical clustering  
**Example:** Agglomerative clustering starts with closest pairs

### 4. Computer Graphics
**Use Case:** Point simplification, mesh optimization  
**Example:** Reducing point cloud density

### 5. Air Traffic Control
**Use Case:** Detecting potential aircraft collisions  
**Example:** Monitoring minimum separation distances

### Industry Examples
| Company/Product | Application |
|-----------------|-------------|
| Unity/Unreal | Game physics collision detection |
| Google Maps | Nearest location queries |
| Amazon | Warehouse robot path planning |
| Airports | Aircraft separation monitoring |
| Social networks | Friend suggestion based on location |

---

## ⚖️ Comparison with Related Algorithms

| Aspect | D&C Closest Pair | Brute Force | KD-Tree | Grid-based |
|--------|-----------------|-------------|---------|------------|
| Time Complexity | O(n log n) | O(n²) | O(n log n) avg | O(n) expected |
| Space Complexity | O(n) | O(1) | O(n) | O(n) |
| Query Support | No | No | Yes | Limited |
| Higher Dimensions | Harder | Same | Good | Harder |
| Best For | One-time queries | Small n | Multiple queries | Uniform distribution |

---

## ⚠️ Common Pitfalls & Edge Cases

1. **Numerical Precision:** Use appropriate epsilon for floating-point comparison
2. **Duplicate Points:** Distance = 0, handle specially
3. **Collinear Points:** Algorithm still works correctly
4. **Strip Sorting:** Must maintain y-sort for efficiency

### Edge Cases to Handle
- [ ] n < 2 (undefined or error)
- [ ] n = 2 (trivial case)
- [ ] All points collinear
- [ ] Multiple pairs with same minimum distance
- [ ] Coincident points (distance = 0)

---

## 📖 References

1. Preparata, F.P., Shamos, M.I. (1985). "Computational Geometry: An Introduction". Springer.
2. Cormen, T.H., et al. (2009). "Introduction to Algorithms" (3rd ed.). MIT Press.
3. [Wikipedia - Closest Pair of Points Problem](https://en.wikipedia.org/wiki/Closest_pair_of_points_problem)

---

## 🔗 Related Algorithms

- [Convex Hull](../05-graph-algorithms/convex-hull.md) - Geometric algorithms
- [KD-Tree](../04-data-structures/trees/kd-tree.md) - Spatial data structure
- [Merge Sort](../01-sorting-algorithms/comparison-based/mergesort.md) - Similar D&C pattern
- [Voronoi Diagram](./voronoi-diagram.md) - Related geometric problem
