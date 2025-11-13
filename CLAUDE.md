# CLAUDE.md - AI Assistant Guide for udemy-tdd

## Project Overview

This is a **Test-Driven Development (TDD) educational project** from the Udemy course "Test Driven Development for Professionals" (https://www.udemy.com/course/test-driven-development-for-professionals). The repository contains practical examples and exercises demonstrating TDD principles in Java.

**Key characteristics:**
- Educational codebase focused on TDD practices
- Multiple independent modules showcasing different TDD scenarios
- Uses classic Java testing tools (JUnit 4 + Mockito)
- Maven-based project structure

---

## Technology Stack

### Build & Runtime
- **Build Tool**: Apache Maven 3.9.11
- **Java Version**: 21.0.8 (OpenJDK)
- **Encoding**: UTF-8
- **Platform**: Linux (compatible with Unix-based systems)

### Dependencies
```xml
<!-- Testing Framework -->
- JUnit 4.12 (test scope)
- Mockito 1.10.19 (test scope)
```

**Note**: These are older versions suitable for the educational context. When suggesting upgrades, consider backward compatibility with the course materials.

---

## Codebase Structure

```
udemy-tdd/
├── pom.xml                      # Maven configuration
├── README.md                    # Project documentation
└── src/
    ├── main/java/com/udemy/     # Production code
    │   ├── discount/            # Discount calculation module
    │   ├── invoice/             # Invoice filtering module
    │   ├── numbers/             # Number comparison utilities
    │   └── roman/               # Roman numeral converter
    └── test/java/com/udemy/     # Test code (mirrors main structure)
        ├── discount/
        ├── invoice/
        ├── numbers/
        └── roman/
```

### Module Breakdown

1. **discount/** - Shopping basket discount logic
   - `DiscountApplier.java`: Applies product-specific and amount-based discounts
   - `Basket.java`: Shopping basket domain object
   - `BasketBuilder.java`: Test data builder pattern implementation
   - `Item.java`: Individual basket item
   - `ProposalGenerator.java`: Generates proposals with time-based logic
   - `Clock.java`: Time abstraction for testability

2. **invoice/** - Invoice filtering system
   - `InvoiceFilter.java`: Filters invoices by amount, customer, and date
   - `Invoice.java`: Invoice domain object
   - `InvoiceRepository.java`: Data access interface

3. **numbers/** - Number comparison utilities
   - `BiggerAndSmaller.java`: Finds min/max in number collections

4. **roman/** - Roman numeral converter
   - `RomanNumeral.java`: Converts Roman numerals to integers

---

## Code Conventions & Patterns

### Testing Conventions

#### Test Structure
All tests follow the **AAA pattern** (Arrange-Act-Assert):
```java
@Test
public void descriptiveName() {
    // Arrange: Set up test data and mocks
    Basket basket = new BasketBuilder()
        .w("MACBOOK", 100)
        .build();

    // Act: Execute the behavior
    applier.apply(basket);

    // Assert: Verify results
    Assert.assertEquals(expected, basket.getAmount(), 0.0001);
}
```

#### Test Organization
- **@Before**: Setup method runs before each test
- **Naming**: Test methods use descriptive names (e.g., `filterInvoicesHigherThan2000()`)
- **One Assertion Per Concept**: Tests focus on single behaviors
- **Delta for Doubles**: Use `0.0001` delta for floating-point comparisons

#### Mocking Strategy
```java
// Use Mockito for external dependencies
repo = Mockito.mock(InvoiceRepository.class);
Mockito.when(repo.all()).thenReturn(Arrays.asList(inv1, inv2));
```

### Code Patterns

#### 1. Builder Pattern for Test Data
```java
// Fluent API for test object creation
Basket basket = new BasketBuilder()
    .w("MACBOOK", 100)
    .w("IPHONE", 50)
    .build();
```
- Method `w()` = "with" (shorthand for fluent API)
- Returns `this` for method chaining

#### 2. Dependency Injection via Constructor
```java
public InvoiceFilter(InvoiceRepository invoices) {
    this.invoices = invoices;
}
```
- All dependencies injected through constructors
- Enables easy mocking and testing

#### 3. Strategy Pattern
- Multiple discount strategies in `DiscountApplier`
- Checked sequentially: product-based → amount-based

#### 4. Domain Objects
- Pure domain logic (e.g., `Basket`, `Invoice`)
- No framework dependencies
- Mutable state acceptable for educational examples

---

## Development Workflow

### Running Tests
```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=DiscountApplierTest

# Run with verbose output
mvn test -X
```

### Building the Project
```bash
# Clean and compile
mvn clean compile

# Clean and package
mvn clean package

# Verify (runs tests)
mvn verify
```

### Project Setup
```bash
# Install dependencies
mvn clean install

# Update dependencies
mvn dependency:resolve
```

---

## AI Assistant Guidelines

### When Working on This Codebase

#### 1. Respect the TDD Educational Context
- **DO**: Maintain test-first approach when adding features
- **DO**: Keep examples simple and focused on demonstrating TDD principles
- **DON'T**: Over-engineer solutions with complex frameworks
- **DON'T**: Suggest major refactorings that obscure learning objectives

#### 2. Testing Philosophy
- **Always write tests before implementation code**
- Use descriptive test names that explain the scenario
- Keep tests isolated and independent
- Prefer explicit setup over shared test fixtures when clarity improves

#### 3. Code Modifications
When modifying code:
1. Read the existing test first to understand intended behavior
2. Add new tests for new functionality
3. Ensure all existing tests pass
4. Maintain consistency with existing code style

#### 4. Adding New Features
Follow this sequence:
```bash
# 1. Write the test (RED phase)
# src/test/java/com/udemy/module/FeatureTest.java

# 2. Run test and verify it fails
mvn test -Dtest=FeatureTest

# 3. Implement minimal code to pass (GREEN phase)
# src/main/java/com/udemy/module/Feature.java

# 4. Run test and verify it passes
mvn test -Dtest=FeatureTest

# 5. Refactor if needed (REFACTOR phase)
# 6. Run full test suite
mvn test
```

#### 5. Mocking Guidelines
- Mock external dependencies (repositories, time, external services)
- Use `Mockito.mock()` for interface/class mocking
- Use `Mockito.when().thenReturn()` for stubbing
- Don't mock domain objects or value objects

#### 6. Common Tasks

**Adding a New Module:**
```bash
# 1. Create package structure
mkdir -p src/main/java/com/udemy/newmodule
mkdir -p src/test/java/com/udemy/newmodule

# 2. Create test class first (TDD!)
# 3. Create implementation class
# 4. Follow red-green-refactor cycle
```

**Refactoring Existing Code:**
1. Ensure all tests pass before starting
2. Refactor incrementally
3. Run tests after each small change
4. Use git commits to mark safe refactoring points

#### 7. Code Style
- **Indentation**: Tabs (as seen in existing code)
- **Braces**: Opening brace on same line
- **Naming**:
  - Classes: PascalCase
  - Methods: camelCase (descriptive names)
  - Test methods: descriptivePhrase (no "test" prefix needed)
- **Access modifiers**: Use private for internal methods
- **Imports**: Keep organized (IDE default ordering)

#### 8. Maven Troubleshooting
```bash
# Clean target directory
mvn clean

# Force update dependencies
mvn clean install -U

# Skip tests (use sparingly!)
mvn clean install -DskipTests

# Show dependency tree
mvn dependency:tree
```

#### 9. What NOT to Do
- Don't add Spring, Spring Boot, or heavy frameworks
- Don't replace JUnit 4 with JUnit 5 without discussion
- Don't add unnecessary abstractions
- Don't modify test data builders to be overly complex
- Don't add logging frameworks (use System.out if needed)
- Don't create interfaces without clear need

#### 10. File Locations Reference
When creating or modifying files:
- Production code: `src/main/java/com/udemy/{module}/`
- Test code: `src/test/java/com/udemy/{module}/`
- Resources: Not currently used in this project

---

## Testing Examples

### Unit Test with Mocks
```java
@Before
public void setUp() {
    repo = Mockito.mock(InvoiceRepository.class);
    filter = new InvoiceFilter(repo);
}

@Test
public void filterInvoicesHigherThan2000() {
    Invoice inv1 = new Invoice(Calendar.getInstance(), "CUSTOMER", 5000);
    Mockito.when(repo.all()).thenReturn(Arrays.asList(inv1));

    List<Invoice> result = filter.filter();

    Assert.assertEquals(1, result.size());
}
```

### Unit Test with Builder
```java
@Test
public void discountForMacAndIphoneTogether() {
    Basket basket = new BasketBuilder()
        .w("MACBOOK", 100)
        .w("IPHONE", 50)
        .build();

    applier.apply(basket);

    double expected = 150 * 0.85;
    Assert.assertEquals(expected, basket.getAmount(), 0.0001);
}
```

### Parameterized Testing Pattern (Manual)
```java
@Test
public void aSingleNumber() {
    Assert.assertEquals(1, roman.convert("I"));
    Assert.assertEquals(5, roman.convert("V"));
    Assert.assertEquals(10, roman.convert("X"));
}
```

---

## Quick Reference Commands

```bash
# Essential Maven commands
mvn clean test           # Clean and run all tests
mvn clean install        # Full build with dependency install
mvn test -Dtest=ClassName # Run specific test class
mvn compile              # Compile without testing

# Verify Java/Maven setup
mvn -v                   # Check Maven version
java -version            # Check Java version

# Git workflow (for AI assistants)
git status               # Check current state
git add .                # Stage all changes
git commit -m "message"  # Commit with message
git push -u origin <branch> # Push to remote branch
```

---

## Module-Specific Notes

### Discount Module
- Complex business rules for product combinations
- Both product-based and amount-based discounts
- Uses `Clock` abstraction for time-dependent logic
- Builder pattern extensively used in tests

### Invoice Module
- Demonstrates mocking external dependencies
- Multiple filtering criteria (amount, customer, date)
- Uses Calendar for date handling

### Roman Module
- Algorithm demonstration
- Edge cases: subtractive notation (IV = 4)
- Limited scope (I, V, X only)

### Numbers Module
- Simple utility class example
- Demonstrates basic TDD for algorithms

---

## Version Information

- **Project Version**: 1.0-SNAPSHOT
- **Group ID**: groupId (default, consider updating for real projects)
- **Artifact ID**: udemy-tdd
- **Maven Model Version**: 4.0.0

---

## Additional Resources

- **Course Link**: https://www.udemy.com/course/test-driven-development-for-professionals
- **JUnit 4 Docs**: https://junit.org/junit4/
- **Mockito Docs**: https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html
- **Maven Guide**: https://maven.apache.org/guides/

---

## Changelog

### 2025-11-13
- Initial CLAUDE.md creation
- Documented existing codebase structure and conventions
- Added comprehensive AI assistant guidelines
- Included testing patterns and workflow instructions
