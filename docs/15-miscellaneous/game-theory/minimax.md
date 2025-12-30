# MiniMax Algorithm

> **Category:** Miscellaneous Algorithms  
> **Subcategory:** Game Theory  
> **Implementation:** [`MiniMaxAlgorithm.java`](../../src/main/java/com/thealgorithms/others/MiniMaxAlgorithm.java)

---

## 📚 Overview

MiniMax is a decision-making algorithm used in game theory for two-player zero-sum games. The algorithm assumes optimal play from both players: the maximizing player tries to maximize their score, while the minimizing player tries to minimize it.

**Key Characteristics:**
- Recursive depth-first search
- Assumes optimal opponent
- Complete game tree exploration
- Can be optimized with Alpha-Beta pruning

---

## 🔢 Algorithm Foundation

### Zero-Sum Games

In a zero-sum game, one player's gain equals the other's loss:
$$
\text{Score}_{\text{max}} + \text{Score}_{\text{min}} = 0
$$

### MiniMax Value

For game state $s$:
$$
\text{minimax}(s) = 
\begin{cases}
\text{utility}(s) & \text{if terminal state} \\
\max_{a} \text{minimax}(\text{result}(s, a)) & \text{if maximizer's turn} \\
\min_{a} \text{minimax}(\text{result}(s, a)) & \text{if minimizer's turn}
\end{cases}
$$

---

## 📊 Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(b^d) where b=branching factor, d=depth |
| Space | O(d) |

For chess: b ≈ 35, d ≈ 100, making pure MiniMax infeasible.

---

## 🔄 Algorithm (Pseudocode)

### Basic MiniMax
```
ALGORITHM MiniMax(state, depth, isMaximizing)
─────────────────────────────────────────────────────
    IF depth = 0 OR state is terminal THEN
        RETURN evaluate(state)
    END IF
    
    IF isMaximizing THEN
        maxEval ← -∞
        FOR each child IN getChildren(state) DO
            eval ← MiniMax(child, depth-1, FALSE)
            maxEval ← max(maxEval, eval)
        END FOR
        RETURN maxEval
    ELSE
        minEval ← +∞
        FOR each child IN getChildren(state) DO
            eval ← MiniMax(child, depth-1, TRUE)
            minEval ← min(minEval, eval)
        END FOR
        RETURN minEval
    END IF
```

### With Alpha-Beta Pruning
```
ALGORITHM MiniMaxAlphaBeta(state, depth, alpha, beta, isMaximizing)
─────────────────────────────────────────────────────
    IF depth = 0 OR state is terminal THEN
        RETURN evaluate(state)
    END IF
    
    IF isMaximizing THEN
        maxEval ← -∞
        FOR each child IN getChildren(state) DO
            eval ← MiniMaxAlphaBeta(child, depth-1, alpha, beta, FALSE)
            maxEval ← max(maxEval, eval)
            alpha ← max(alpha, eval)
            IF beta ≤ alpha THEN
                BREAK  // Beta cutoff
            END IF
        END FOR
        RETURN maxEval
    ELSE
        minEval ← +∞
        FOR each child IN getChildren(state) DO
            eval ← MiniMaxAlphaBeta(child, depth-1, alpha, beta, TRUE)
            minEval ← min(minEval, eval)
            beta ← min(beta, eval)
            IF beta ≤ alpha THEN
                BREAK  // Alpha cutoff
            END IF
        END FOR
        RETURN minEval
    END IF
```

### Step-by-Step Example

**Tic-Tac-Toe Position:**
```
X | O | X
---------
O | X | 
---------
  | O | 
```

**Game Tree (simplified):**
```
                    [Max: X's turn]
                    /      |      \
            [Min: O]   [Min: O]  [Min: O]
              / \         |         |
           [3]  [5]      [2]       [0]
           
Max chooses: position leading to 5 (best worst case)
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class MiniMax {
    
    public static int minimax(int[] board, int depth, boolean isMaximizing) {
        int score = evaluate(board);
        
        // Terminal conditions
        if (score == 10) return score - depth;  // Max wins
        if (score == -10) return score + depth; // Min wins
        if (!hasMovesLeft(board)) return 0;     // Draw
        
        if (isMaximizing) {
            int best = Integer.MIN_VALUE;
            for (int i = 0; i < 9; i++) {
                if (board[i] == 0) {
                    board[i] = 1;  // Max's move
                    best = Math.max(best, minimax(board, depth + 1, false));
                    board[i] = 0;  // Undo
                }
            }
            return best;
        } else {
            int best = Integer.MAX_VALUE;
            for (int i = 0; i < 9; i++) {
                if (board[i] == 0) {
                    board[i] = -1; // Min's move
                    best = Math.min(best, minimax(board, depth + 1, true));
                    board[i] = 0;  // Undo
                }
            }
            return best;
        }
    }
    
    // With Alpha-Beta Pruning
    public static int minimaxAB(int[] board, int depth, int alpha, int beta, 
                                 boolean isMaximizing) {
        int score = evaluate(board);
        
        if (score == 10 || score == -10 || !hasMovesLeft(board)) {
            return score;
        }
        
        if (isMaximizing) {
            int best = Integer.MIN_VALUE;
            for (int i = 0; i < 9; i++) {
                if (board[i] == 0) {
                    board[i] = 1;
                    best = Math.max(best, minimaxAB(board, depth + 1, 
                                                     alpha, beta, false));
                    board[i] = 0;
                    alpha = Math.max(alpha, best);
                    if (beta <= alpha) break;  // Pruning
                }
            }
            return best;
        } else {
            int best = Integer.MAX_VALUE;
            for (int i = 0; i < 9; i++) {
                if (board[i] == 0) {
                    board[i] = -1;
                    best = Math.min(best, minimaxAB(board, depth + 1, 
                                                     alpha, beta, true));
                    board[i] = 0;
                    beta = Math.min(beta, best);
                    if (beta <= alpha) break;  // Pruning
                }
            }
            return best;
        }
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/others/MiniMaxAlgorithm.java`](../../src/main/java/com/thealgorithms/others/MiniMaxAlgorithm.java)

---

## 🌍 Real-World Applications

### 1. Board Games
**Use Case:** Chess, Checkers, Tic-Tac-Toe, Connect Four

### 2. Game AI
**Use Case:** Video game opponent decision making

### 3. Decision Making
**Use Case:** Strategic planning under adversarial conditions

### Famous Implementations

| Game | Notable AI | Technique |
|------|------------|-----------|
| Chess | Deep Blue, Stockfish | MiniMax + Alpha-Beta |
| Checkers | Chinook | MiniMax + Endgame DB |
| Go | AlphaGo | MCTS + Neural Networks |

---

## 📈 Alpha-Beta Pruning Efficiency

| Scenario | Nodes Explored |
|----------|---------------|
| No pruning | O(b^d) |
| Random ordering | O(b^(3d/4)) |
| Optimal ordering | O(b^(d/2)) |

**Optimal ordering** can effectively double the searchable depth!

---

## ⚖️ MiniMax vs Other Game Algorithms

| Algorithm | Perfect Play | Memory | Speed |
|-----------|--------------|--------|-------|
| MiniMax | Yes (full depth) | Low | Slow |
| MCTS | Probabilistic | Medium | Fast |
| Expectimax | Chance games | Low | Medium |
| Negamax | Same as MiniMax | Low | Same |

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Slow without pruning | Exponential time | Use Alpha-Beta |
| Horizon effect | Missing deep threats | Iterative deepening |
| Poor evaluation | Bad decisions | Better heuristics |
| Memory for large games | Can't store tree | Use transposition table |

---

## 📖 References

1. **"Artificial Intelligence: A Modern Approach"** - Russell & Norvig
2. **"Game Tree Searching"** - Knuth & Moore (1975)
3. **Alpha-Beta Pruning Paper** - McCarthy et al.

---

## 🔗 Related Algorithms

- [Monte Carlo Tree Search](./mcts.md)
- [Expectimax](./expectimax.md)
- [A* Search](../../02-searching-algorithms/README.md)

---

*Last updated: December 30, 2025*
