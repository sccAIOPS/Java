# Agent Usage Instructions

This directory contains GitHub Copilot agent definitions for the TheAlgorithms/Java repository. Each agent is specialized for a specific aspect of the software development lifecycle.

## Available Agents

### 1. Architecture Analyst (`@architecture-analyst`)

**Purpose:** Extract and document low-level architecture using UML diagrams in PlantUML format.

**Capabilities:**
- C4 Container and Component diagrams
- Class diagrams with inheritance hierarchies
- Sequence diagrams for algorithm flows
- State machine diagrams
- Entity-Relationship diagrams for data structures
- Communication diagrams

**Example Usage:**
```
@architecture-analyst Generate a class diagram for the datastructures/trees package
showing all tree implementations and their relationships.
```

---

### 2. Algorithm Analyst (`@algorithm-analyst`)

**Purpose:** Deep analysis of algorithms and data structures, identifying patterns, pitfalls, and optimizations.

**Capabilities:**
- Design pattern extraction (Strategy, Template Method, etc.)
- Time/Space complexity analysis
- Pitfall detection (performance, correctness, memory)
- Comparative analysis of similar algorithms
- Comprehensive improvement recommendations

**Example Usage:**
```
@algorithm-analyst Analyze the HeapSort implementation. Identify the design
patterns used, verify complexity claims, and suggest optimizations.
```

---

### 3. Planning Specialist (`@planning-specialist`)

**Purpose:** Transform requirements into implementation plans using BDD, SOLID, and KISS principles.

**Capabilities:**
- BDD scenario writing (Given-When-Then)
- SOLID principle application
- Functional and Non-Functional requirements definition
- Feature planning templates
- Risk identification

**Example Usage:**
```
@planning-specialist Plan the implementation of an AVL Tree data structure.
Include BDD scenarios for all CRUD operations and balancing.
```

---

### 4. System Designer (`@system-designer`)

**Purpose:** Create high-level and low-level system designs for new features.

**Capabilities:**
- Package architecture design
- Class and interface specifications
- Data structure design
- Algorithm specifications with pseudocode
- API design
- File structure planning

**Example Usage:**
```
@system-designer Design the class structure for a Graph implementation
supporting both adjacency list and matrix representations.
```

---

### 5. Implementation Engineer (`@implementation-engineer`)

**Purpose:** Implement features using TDD, ensuring code quality and test coverage.

**Capabilities:**
- TDD workflow (Red-Green-Refactor)
- Unit test writing
- Interface implementation
- Generic programming
- Maven build verification

**Example Usage:**
```
@implementation-engineer Implement the BinarySearch algorithm following
the design specification. Use TDD and ensure all quality checks pass.
```

---

### 6. Code Reviewer (`@code-reviewer`)

**Purpose:** Review code for quality, style compliance, and security.

**Capabilities:**
- Checkstyle validation
- SpotBugs bug detection
- PMD code analysis
- Security vulnerability scanning (Snyk-style)
- Code style review
- Comprehensive review reports

**Example Usage:**
```
@code-reviewer Review the new LinkedList implementation for code quality,
potential bugs, and security issues.
```

---

### 7. Test Engineer (`@test-engineer`)

**Purpose:** Comprehensive testing including unit, integration, and load testing.

**Capabilities:**
- Unit test development
- Parameterized testing
- Integration testing
- Performance testing (complexity verification)
- Load testing (Soak, Peak, Stress)
- Coverage analysis

**Example Usage:**
```
@test-engineer Create comprehensive tests for the PriorityQueue implementation
including performance benchmarks and stress tests.
```

---

## Workflow Recommendations

### For New Features

1. **Plan** → Use `@planning-specialist` to create requirements and BDD scenarios
2. **Design** → Use `@system-designer` to create architecture and class designs
3. **Implement** → Use `@implementation-engineer` for TDD-based implementation
4. **Review** → Use `@code-reviewer` to verify quality standards
5. **Test** → Use `@test-engineer` for comprehensive testing

### For Understanding Existing Code

1. **Architecture** → Use `@architecture-analyst` for UML diagrams
2. **Analysis** → Use `@algorithm-analyst` for deep technical analysis

### For Code Quality Issues

1. **Review** → Use `@code-reviewer` to identify issues
2. **Test** → Use `@test-engineer` to add missing tests

## Quick Reference

| Task | Agent |
|------|-------|
| Generate UML diagrams | `@architecture-analyst` |
| Analyze algorithm complexity | `@algorithm-analyst` |
| Find design patterns | `@algorithm-analyst` |
| Plan new feature | `@planning-specialist` |
| Design classes/interfaces | `@system-designer` |
| Implement with TDD | `@implementation-engineer` |
| Check code quality | `@code-reviewer` |
| Run security scan | `@code-reviewer` |
| Write tests | `@test-engineer` |
| Performance testing | `@test-engineer` |
