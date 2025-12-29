# 🔙 Backtracking Algorithms

> **Category:** Algorithm Design Paradigm  
> **Difficulty:** Intermediate to Advanced  
> **Prerequisites:** Recursion, Trees, Basic Problem-Solving

---

## 📚 Overview

Backtracking is an algorithmic technique that builds solutions incrementally, abandoning a path ("backtracking") as soon as it determines the path cannot lead to a valid solution. It's essentially a depth-first search with pruning.

### Key Concepts

1. **Choice:** At each step, make a choice from available options
2. **Constraint:** Check if current choice satisfies constraints
3. **Goal:** Check if we've reached a complete solution
4. **Backtrack:** Undo choice and try another if stuck

### When to Use Backtracking

- Finding all (or some) solutions to a constraint satisfaction problem
- Combinatorial optimization problems
- Puzzles and games with rules
- Problems where brute force is infeasible but pruning helps

---

## 📊 Classification

```
Backtracking Problems
├── Permutation Problems
│   ├── Permutations
│   ├── N-Queens
│   └── Sudoku Solver
│
├── Combination Problems
│   ├── Subsets
│   ├── Combination Sum
│   └── Partition Problems
│
├── Path Finding
│   ├── Maze Solving
│   ├── Knight's Tour
│   └── Hamiltonian Path
│
├── Constraint Satisfaction
│   ├── Graph Coloring
│   ├── Crossword Solver
│   └── Cryptarithmetic
│
└── Game Theory
    ├── Tic-Tac-Toe
    └── Chess Move Generation
```

---

## 🔄 General Template

```
ALGORITHM Backtrack(candidate)
─────────────────────────────────────────────────────
    IF is_solution(candidate) THEN
        output(candidate)
        RETURN
    END IF
    
    FOR each choice IN available_choices(candidate) DO
        IF is_valid(choice) THEN
            make_choice(choice)
            Backtrack(candidate)
            undo_choice(choice)  ← BACKTRACK
        END IF
    END FOR
```

### Java Template

```java
void backtrack(State state, List<Solution> results) {
    if (isSolution(state)) {
        results.add(state.copy());
        return;
    }
    
    for (Choice choice : getChoices(state)) {
        if (isValid(state, choice)) {
            makeChoice(state, choice);
            backtrack(state, results);
            undoChoice(state, choice);  // Backtrack
        }
    }
}
```

---

## 📈 Complexity Analysis

| Problem | Time | Space | Pruning Effectiveness |
|---------|------|-------|----------------------|
| [N-Queens](./n-queens.md) | O(N!) | O(N) | High - column/diagonal checks |
| [Sudoku](./sudoku-solver.md) | O(9^81) | O(81) | Very High - constraint propagation |
| [Permutations](./permutations.md) | O(N × N!) | O(N) | None - all permutations valid |
| [Subsets](./subsets.md) | O(2^N) | O(N) | None - all subsets valid |
| [Graph Coloring](./graph-coloring.md) | O(M^N) | O(N) | Medium - adjacency checks |
| [Knight's Tour](./knights-tour.md) | O(8^(N²)) | O(N²) | High - Warnsdorff's heuristic |

---

## 🎯 Problem-Solving Framework

### Step-by-Step Approach

1. **Define the State Space**
   - What constitutes a partial solution?
   - What parameters define current state?

2. **Identify Choices**
   - What decisions can be made at each step?
   - What's the branching factor?

3. **Define Constraints**
   - What makes a choice invalid?
   - How early can we prune?

4. **Determine Goal State**
   - When is the solution complete?
   - Do we need one solution or all?

5. **Implement Backtracking**
   - Make choice → Recurse → Undo choice

---

## 🔬 Mathematical Foundation

### State Space Tree

For N-Queens on 4×4 board:

```
                    Root
           /    /    \    \
          Q1   Q2    Q3   Q4    (Row 1: 4 choices)
         /|\   |     |    |\
        ...   ...   ...  ...    (Row 2: ≤4 choices each)
                                (Many pruned by constraints)
```

### Complexity Analysis

**Without Pruning:** All $b^d$ nodes explored
- $b$ = branching factor (choices per step)
- $d$ = depth (solution length)

**With Pruning:** Much fewer nodes
$$
\text{Effective nodes} = b^d \times p
$$
where $p$ = probability a path survives pruning

### N-Queens Analysis

Without pruning: $N^N$ possibilities  
With column constraint: $N!$ possibilities  
With diagonal pruning: Approximately $\frac{N!}{e}$ valid solutions

---

## 📁 Algorithms in This Section

| File | Problem | Status |
|------|---------|--------|
| [n-queens.md](./n-queens.md) | N-Queens Problem | 📋 Planned |
| [sudoku-solver.md](./sudoku-solver.md) | Sudoku Solver | 📋 Planned |
| [knights-tour.md](./knights-tour.md) | Knight's Tour | 📋 Planned |
| [graph-coloring.md](./graph-coloring.md) | M-Coloring Problem | 📋 Planned |
| [permutations.md](./permutations.md) | Generate Permutations | 📋 Planned |
| [subsets.md](./subsets.md) | Generate Subsets | 📋 Planned |
| [combination-sum.md](./combination-sum.md) | Combination Sum | 📋 Planned |
| [maze-solver.md](./maze-solver.md) | Maze Solving | 📋 Planned |

---

## 🌍 Real-World Applications

| Application | Problem Type | Example |
|-------------|--------------|---------|
| Puzzle Games | Constraint satisfaction | Sudoku apps |
| Route Planning | Path finding with constraints | Delivery routing |
| Resource Scheduling | Assignment problems | Course scheduling |
| Compiler Optimization | Register allocation | GCC, LLVM |
| Circuit Design | Component placement | VLSI design |
| Game AI | Move generation | Chess engines |
| Bioinformatics | Sequence alignment | Protein folding |

---

## ⚡ Optimization Techniques

### 1. Pruning Strategies
- **Constraint propagation:** Reduce domains early
- **Forward checking:** Check future variables
- **Arc consistency:** Ensure all constraints satisfiable

### 2. Variable/Value Ordering
- **MRV (Minimum Remaining Values):** Choose variable with fewest options
- **LCV (Least Constraining Value):** Choose value that rules out fewest options

### 3. Intelligent Backtracking
- **Conflict-directed backjumping:** Skip irrelevant variables
- **Nogood learning:** Remember failed combinations

---

## 📖 References

1. Russell, S. & Norvig, P. *"Artificial Intelligence: A Modern Approach"*, Chapter 6
2. Cormen, T. H., et al. *"Introduction to Algorithms"* (CLRS)
3. Knuth, D. E. *"The Art of Computer Programming"*, Volume 4A

---

[← Back to Main Index](../README.md)
