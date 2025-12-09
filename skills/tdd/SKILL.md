---
name: TDD
description: Strategies for Test Driven Development (TDD) when writing new or modifying existing code. The approach listed here is how the user specifically wants you to perform TDD.
---

# TDD Skill

Use this skill when implementing new features or methods that require testing.

## When to Use This Skill

- Implementing new features
- Adding new methods or classes
- Fixing bugs (write test that exposes bug first)
- Refactoring existing code (ensure tests pass before and after)

## Test-Driven Development Process

### 1. Write Test Descriptions First
- Before any implementation or tests, write clear descriptions of what should be tested
- Present the test descriptions to the user
- Get feedback/confirmation on the test cases before proceeding
- This ensures we're testing the right behavior

### 2. Write Tests with Red and Green Paths
- Each test case should have assertions that verify the expected behavior
- Include both positive tests (what should happen) and negative tests where appropriate
- Use clear, maintainable patterns (like `create_list` instead of loops)
- Follow the project's testing conventions (RSpec, FactoryBot, etc.)

### 3. Run Tests to Verify RED
- Execute the test suite before implementing anything
- Confirm all tests fail in the expected way (returning `nil`, missing methods, etc.)
- This proves the tests are actually testing something
- Report the failures to confirm we're in RED state

### 4. Implement the Minimum Code to Make Tests GREEN
- Write just enough code to make all tests pass
- Don't over-engineer or add unnecessary features
- Focus on satisfying the test requirements
- Keep the implementation simple and focused

### 5. Verify GREEN
- Run tests again to confirm they all pass
- This proves the implementation meets requirements
- Report the success

### 6. Run Full CI Suite
- Ensure the implementation fits within the broader system
- Fix any style violations, security issues, or integration problems
- Verify all existing tests still pass
- Run `bin/ci` or equivalent to validate everything

## Key Principles

- **Tests define behavior before code exists**
- **Build exactly what's needed, nothing more**
- **Prove it works through automated testing**
- **Red → Green → Refactor cycle**
