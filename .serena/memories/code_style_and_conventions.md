# Code Style and Conventions

## General Style
- Follows Sun Java coding conventions
- Checkstyle configuration based on `checkstyle.xml`
- Code must pass Checkstyle, SpotBugs, and PMD checks

## Naming Conventions
- **Classes**: PascalCase (e.g., `BubbleSort`, `SortUtils`)
- **Methods**: camelCase (e.g., `sort`, `swap`, `greater`)
- **Constants**: UPPER_SNAKE_CASE (implied from standard Java conventions)
- **Packages**: lowercase (e.g., `com.thealgorithms.sorts`)

## Documentation
- **Javadoc Required**: All public classes and methods should have Javadoc
- **Format**: Include:
  - Description of what the class/method does
  - `@param` for parameters
  - `@return` for return values
  - `@author` for contributors
  - `@see` for related classes
  - Time and Space complexity for algorithms

### Javadoc Example
```java
/**
 * Implements generic bubble sort algorithm.
 *
 * Time Complexity:
 * - Best case: O(n) – array is already sorted.
 * - Average case: O(n^2)
 * - Worst case: O(n^2)
 *
 * Space Complexity: O(1) – in-place sorting.
 *
 * @param array the array to be sorted.
 * @param <T> the type of elements in the array.
 * @return the sorted array.
 */
```

## Code Patterns
- Use **generics** extensively (e.g., `<T extends Comparable<T>>`)
- **Utility classes** should be final with private constructor
- Prefer **static methods** for utility functions
- Use **interfaces** for algorithm contracts (e.g., `SortAlgorithm`)
- Implement methods with `@Override` annotation

## Test Conventions
- Test classes in `src/test/java` mirror main code structure
- Test class naming: `{ClassName}Test.java`
- Use JUnit 5 (Jupiter) annotations: `@Test`
- Use AssertJ or JUnit assertions
- Test method naming: descriptive names (e.g., `bubbleSortEmptyArray`)

## Visibility
- Utility classes: `final` with private constructor
- Algorithm implementations: package-private or public based on need
- Utility methods: `public static` for reusability