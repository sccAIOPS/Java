# Codebase Structure

## Directory Layout

```
Java/
├── src/
│   ├── main/java/com/thealgorithms/     # Main source code
│   └── test/java/com/thealgorithms/     # Test code
├── target/                               # Build output (gitignored)
├── .github/workflows/                    # CI/CD pipelines
├── pom.xml                              # Maven configuration
├── checkstyle.xml                       # Checkstyle rules
├── spotbugs-exclude.xml                 # SpotBugs exclusions
├── pmd-custom_ruleset.xml               # PMD custom rules
├── pmd-exclude.properties               # PMD exclusions
├── DIRECTORY.md                         # Algorithm directory listing
├── CONTRIBUTING.md                      # Contribution guidelines
└── README.md                            # Project documentation
```

## Algorithm Categories

All algorithms are organized under `src/main/java/com/thealgorithms/`:

- **audiofilters/** - Audio signal processing algorithms
- **backtracking/** - Backtracking algorithms (e.g., N-Queens, Sudoku)
- **bitmanipulation/** - Bit manipulation techniques
- **ciphers/** - Encryption and cipher algorithms
- **compression/** - Data compression algorithms
- **conversions/** - Unit and format conversions
- **datastructures/** - Data structure implementations (lists, trees, graphs, etc.)
- **devutils/** - Development utilities
- **divideandconquer/** - Divide and conquer algorithms
- **dynamicprogramming/** - Dynamic programming solutions
- **geometry/** - Geometric algorithms
- **graph/** - Graph algorithms (BFS, DFS, shortest path, etc.)
- **greedyalgorithms/** - Greedy algorithm implementations
- **io/** - Input/output utilities
- **lineclipping/** - Line clipping algorithms
- **maths/** - Mathematical algorithms and utilities
- **matrix/** - Matrix operations and algorithms
- **misc/** - Miscellaneous algorithms
- **others/** - Other uncategorized algorithms
- **physics/** - Physics simulations and calculations
- **puzzlesandgames/** - Puzzle and game solvers
- **randomized/** - Randomized algorithms
- **recursion/** - Recursive algorithms
- **scheduling/** - Scheduling algorithms
- **searches/** - Search algorithms (binary search, linear search, etc.)
- **slidingwindow/** - Sliding window technique algorithms
- **sorts/** - Sorting algorithms (bubble, merge, quick, heap, etc.)
- **stacks/** - Stack-based algorithms
- **strings/** - String manipulation algorithms
- **tree/** - Tree algorithms and data structures

## Test Structure
Tests mirror the main source structure:
- Located in `src/test/java/com/thealgorithms/`
- Each algorithm typically has a corresponding test file
- Test naming: `{AlgorithmName}Test.java`

## Build Outputs
- `target/classes/` - Compiled main code
- `target/test-classes/` - Compiled test code
- `target/site/jacoco/` - Code coverage reports
- `target/*.jar` - Built JAR files

## Configuration Files
- **pom.xml** - Maven project configuration, dependencies, plugins
- **checkstyle.xml** - Code style rules (based on Sun conventions)
- **spotbugs-exclude.xml** - Files/rules excluded from SpotBugs analysis
- **pmd-custom_ruleset.xml** - Custom PMD rules
- **pmd-exclude.properties** - PMD exclusion patterns