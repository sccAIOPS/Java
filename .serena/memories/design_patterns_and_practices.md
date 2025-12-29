# Design Patterns and Best Practices

## Common Design Patterns

### 1. Interface-Based Design
- Algorithm implementations often implement interfaces
- Example: `SortAlgorithm` interface for all sorting algorithms
- Allows polymorphic use of different algorithm implementations

```java
class BubbleSort implements SortAlgorithm {
    @Override
    public <T extends Comparable<T>> T[] sort(T[] array) {
        // implementation
    }
}
```

### 2. Utility Classes Pattern
- Static utility methods in final classes
- Private constructor to prevent instantiation
- Example: `SortUtils` class

```java
final class SortUtils {
    private SortUtils() {
        // Private constructor
    }
    
    public static <T> void swap(T[] array, int i, int j) {
        // implementation
    }
}
```

### 3. Generic Programming
- Extensive use of Java generics for type safety
- Bounded type parameters: `<T extends Comparable<T>>`
- Enables algorithms to work with any comparable type

### 4. Separation of Concerns
- Algorithm logic separated from utility functions
- Test code completely separate from implementation
- Clear package organization by algorithm category

## Coding Best Practices

### Educational Focus
- **Clarity over performance** - Code should be easy to understand
- **Comprehensive comments** - Explain algorithm steps
- **Time/Space complexity documentation** - Always document complexity
- **Multiple test cases** - Cover edge cases, normal cases, and boundary conditions

### Code Organization
- One public class per file
- Related algorithms grouped in same package
- Shared utilities in dedicated utility classes
- Tests mirror source structure

### Method Design
- Keep methods focused and small
- Use descriptive method names
- Prefer pure functions (no side effects) when possible
- Document parameters and return values

### Error Handling
- Use appropriate exceptions for invalid inputs
- Document exception conditions in Javadoc
- Validate inputs when necessary

### Testing Practices
- Test naming should describe what is being tested
- Use meaningful variable names in tests
- Test edge cases: empty input, single element, duplicates
- Use assertion libraries effectively (AssertJ, JUnit)

### Example Test Pattern
```java
@Test
public void algorithmNameEmptyArray() {
    // Arrange
    Integer[] inputArray = {};
    
    // Act
    Integer[] outputArray = algorithm.sort(inputArray);
    
    // Assert
    Integer[] expectedOutput = {};
    assertArrayEquals(expectedOutput, outputArray);
}
```

## Code Review Checklist
- [ ] Code follows project conventions
- [ ] Javadoc is complete with complexity analysis
- [ ] All quality checks pass
- [ ] Tests are comprehensive
- [ ] Code is readable and well-commented
- [ ] No unnecessary dependencies added
- [ ] Algorithm is correctly implemented