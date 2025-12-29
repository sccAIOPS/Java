# Task Completion Checklist

When completing a coding task in this project, ensure the following steps are performed:

## 1. Code Implementation
- [ ] Implement the algorithm or feature
- [ ] Follow the code style conventions
- [ ] Add proper Javadoc with:
  - Description
  - Time and space complexity (for algorithms)
  - `@param` tags
  - `@return` tag
  - `@author` tag

## 2. Testing
- [ ] Create or update test cases in `src/test/java`
- [ ] Ensure tests cover edge cases (empty, single element, normal cases)
- [ ] Run tests: `mvn test`
- [ ] Verify all tests pass

## 3. Code Quality Checks
Run all quality checks and fix any violations:
- [ ] Checkstyle: `mvn checkstyle:check`
- [ ] SpotBugs: `mvn spotbugs:check`
- [ ] PMD: `mvn pmd:check`

## 4. Build Verification
- [ ] Run full build: `mvn clean verify`
- [ ] Ensure no compilation errors
- [ ] Check code coverage if needed

## 5. Code Review
- [ ] Review code for clarity and educational value
- [ ] Ensure code follows project patterns
- [ ] Verify algorithm correctness
- [ ] Check that imports are minimal and necessary

## 6. Documentation
- [ ] Update DIRECTORY.md if adding new algorithm
- [ ] Add comments for complex logic
- [ ] Ensure class and method names are descriptive

## Quick Validation Command
Run this single command to validate everything:
```bash
mvn clean verify checkstyle:check spotbugs:check pmd:check
```

## Common Issues to Check
- Missing or incomplete Javadoc
- Unused imports
- Magic numbers (use constants)
- Missing test cases
- Style violations (indentation, braces, naming)