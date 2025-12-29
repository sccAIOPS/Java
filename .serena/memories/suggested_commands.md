# Suggested Commands

## Maven Build Commands

### Build and Test
```bash
mvn clean verify
```
Cleans, compiles, runs tests, and generates reports.

### Build with Updates
```bash
mvn --batch-mode --update-snapshots verify
```
Build with latest snapshot dependencies (used in CI).

### Run Tests Only
```bash
mvn test
```
Runs all unit tests.

### Compile Only
```bash
mvn compile
```
Compiles the source code.

## Code Quality Checks

### Run All Quality Checks
```bash
mvn verify checkstyle:check spotbugs:check pmd:check
```
Runs tests and all static analysis tools.

### Checkstyle (Code Style)
```bash
mvn checkstyle:check
```
Verifies code follows style guidelines.

### SpotBugs (Bug Detection)
```bash
mvn spotbugs:check
```
Detects potential bugs and security issues.

### PMD (Code Analysis)
```bash
mvn pmd:check
```
Performs static code analysis.

## Code Coverage

### Generate Coverage Report
```bash
mvn jacoco:report
```
Generates code coverage report (after running tests).
Report location: `target/site/jacoco/index.html`

## Development Commands

### Clean Build Artifacts
```bash
mvn clean
```
Removes the `target` directory.

### Install to Local Repository
```bash
mvn install
```
Installs the JAR to local Maven repository.

### Package JAR
```bash
mvn package
```
Creates a JAR file in `target/` directory.

## Git Commands
Standard git commands for Linux:
```bash
git status
git add <files>
git commit -m "message"
git push
git pull
git log
git diff
```

## File System Commands (Linux)
```bash
ls -la          # List files with details
find . -name    # Find files by name
grep -r         # Search in files recursively
cd              # Change directory
pwd             # Print working directory
cat             # View file contents
less            # View file with pagination
```