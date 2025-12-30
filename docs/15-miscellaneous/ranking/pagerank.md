# PageRank Algorithm

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Graph Ranking  
> **Implementation:** [`PageRank.java`](../../src/main/java/com/thealgorithms/others/PageRank.java)

---

## 📚 Overview

PageRank is an algorithm developed by Larry Page and Sergey Brin at Stanford University (1998), forming the basis of Google's original search engine. It measures the importance of web pages by analyzing the link structure of the web graph.

**Key Characteristics:**
- Random surfer model
- Iterative computation
- Handles dangling nodes
- Converges to steady state

---

## 🔢 Mathematical Foundation

### Basic Formula

For page $p_i$ with incoming links from pages in set $B_i$:

$$
PR(p_i) = \frac{1-d}{N} + d \sum_{p_j \in B_i} \frac{PR(p_j)}{L(p_j)}
$$

Where:
- $d$ = damping factor (typically 0.85)
- $N$ = total number of pages
- $L(p_j)$ = number of outgoing links from page $p_j$

### Matrix Form

$$
\mathbf{R} = d \cdot \mathbf{M} \cdot \mathbf{R} + \frac{1-d}{N} \cdot \mathbf{1}
$$

Where $\mathbf{M}$ is the column-stochastic adjacency matrix.

### Random Surfer Interpretation

- With probability $d$, follow a random outgoing link
- With probability $1-d$, jump to any random page

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time per iteration | O(E) where E = edges |
| Total time | O(k × E) where k = iterations to converge |
| Space | O(N) |

Typically converges in 50-100 iterations.

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM PageRank(graph, d, epsilon)
─────────────────────────────────────────────────────
    INPUT:  graph - web graph (pages and links)
            d - damping factor (0.85)
            epsilon - convergence threshold
    OUTPUT: PageRank values for all pages
─────────────────────────────────────────────────────

    N ← number of pages
    PR[] ← array of size N, initialized to 1/N
    
    REPEAT
        newPR[] ← array of size N
        
        FOR each page p DO
            sum ← 0
            FOR each page q linking to p DO
                sum ← sum + PR[q] / outDegree(q)
            END FOR
            
            newPR[p] ← (1-d)/N + d × sum
        END FOR
        
        diff ← ||newPR - PR||  // L1 norm
        PR ← newPR
        
    UNTIL diff < epsilon
    
    RETURN PR
```

### Step-by-Step Example

**Graph:**
```
A → B → C
↑   ↓
└───┘
```

**Iteration 0:** PR = [0.33, 0.33, 0.33]

**Iteration 1:** (d = 0.85)
- PR(A) = 0.15/3 + 0.85 × (PR(B)/1) = 0.05 + 0.85 × 0.33 = 0.33
- PR(B) = 0.15/3 + 0.85 × (PR(A)/1) = 0.05 + 0.85 × 0.33 = 0.33
- PR(C) = 0.15/3 + 0.85 × (PR(B)/1) = 0.05 + 0.85 × 0.33 = 0.33

After convergence: C has lower rank (no outgoing links affect its importance less due to reciprocal nature).

---

## 💻 Implementation Notes

### Java Implementation

```java
public class PageRank {
    private double dampingFactor;
    private double epsilon;
    private int maxIterations;
    
    public PageRank(double d, double epsilon, int maxIter) {
        this.dampingFactor = d;
        this.epsilon = epsilon;
        this.maxIterations = maxIter;
    }
    
    public double[] compute(int[][] graph) {
        int n = graph.length;
        double[] pr = new double[n];
        double[] newPr = new double[n];
        
        // Initialize
        Arrays.fill(pr, 1.0 / n);
        
        // Calculate out-degrees
        int[] outDegree = new int[n];
        for (int i = 0; i < n; i++) {
            outDegree[i] = graph[i].length;
        }
        
        // Build incoming links
        List<List<Integer>> inLinks = buildIncomingLinks(graph);
        
        for (int iter = 0; iter < maxIterations; iter++) {
            double diff = 0.0;
            
            for (int i = 0; i < n; i++) {
                double sum = 0.0;
                for (int j : inLinks.get(i)) {
                    if (outDegree[j] > 0) {
                        sum += pr[j] / outDegree[j];
                    }
                }
                newPr[i] = (1 - dampingFactor) / n + dampingFactor * sum;
                diff += Math.abs(newPr[i] - pr[i]);
            }
            
            // Swap arrays
            double[] temp = pr;
            pr = newPr;
            newPr = temp;
            
            if (diff < epsilon) break;
        }
        
        return pr;
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/PageRank.java`](../../src/main/java/com/thealgorithms/others/PageRank.java)

---

## 🌍 Real-World Applications

### 1. Search Engines
**Use Case:** Ranking web pages in search results

### 2. Social Networks
**Use Case:** Identifying influential users

### 3. Citation Analysis
**Use Case:** Ranking academic papers

### 4. Recommendation Systems
**Use Case:** Ranking products or content

### PageRank Variants

| Variant | Modification | Use Case |
|---------|--------------|----------|
| Personalized PR | Biased random jumps | Recommendations |
| Topic-Sensitive PR | Topic-specific | Specialized search |
| TrustRank | Manual seed pages | Spam detection |

---

## 🚨 Special Cases

### Dangling Nodes
Pages with no outgoing links need special handling:
- Option 1: Distribute PR equally to all pages
- Option 2: Add self-loop
- Option 3: Remove from calculation

### Spider Traps
Groups of pages that only link to each other:
- Damping factor $(1-d)$ ensures escape

---

## ⚖️ Choosing Damping Factor

| Damping Factor | Effect |
|----------------|--------|
| d = 0.85 | Standard (Google's original) |
| d → 1 | More link-focused, slower convergence |
| d → 0 | More uniform distribution |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Dead ends | Rank leaks | Handle dangling nodes |
| Spider traps | Rank accumulates | Use damping factor |
| Slow convergence | Many iterations | Good initial estimate |
| Numerical precision | Underflow | Use log space for large graphs |

---

## 📖 References

1. **"The Anatomy of a Large-Scale Search Engine"** - Brin & Page (1998)
2. **"The PageRank Citation Ranking"** - Page, Brin, et al.
3. **Google's PageRank Patent** - US Patent 6,285,999

---

## 🔗 Related Algorithms

- [HITS Algorithm](./hits-algorithm.md)
- [Graph Algorithms](../../05-graph-algorithms/README.md)
- [Markov Chains](../../08-mathematical-algorithms/README.md)

---

*Last updated: December 30, 2025*
