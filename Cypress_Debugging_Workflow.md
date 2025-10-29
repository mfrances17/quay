# Cypress Automated Debugging Workflow

## Overview
This document provides a complete guide for using the automated Cypress debugging workflow in Cursor. The workflow automates the process of skipping passing tests and focusing on failing tests for efficient debugging.

## Quick Reference - Sample Prompts

### Automated Workflow (Recommended)
Use these prompts in order for a streamlined debugging experience:

1. **Start Debugging Session:**
   ```
   "Debug [test-name] test failures"
   ```
   *Examples: "Debug org-list test failures", "Debug user-management test failures", "Debug repo-details test failures"*

   **Note:** This command will first run the test to discover any failures, then enter debugging mode if failures are found.

2. **Continue to Next Failure (repeat as needed):**
   ```
   "Move to next failing test in [test-name]"
   ```
   *Examples: "Move to next failing test in org-list", "Move to next failing test in user-management"*

3. **Finish Debugging Session:**
   ```
   "Remove all skips and rerun all tests in [test-name]"
   ```
   *Examples: "Remove all skips and rerun all tests in org-list", "Remove all skips and rerun all tests in repo-details"*

### Manual Control Workflow (Alternative)
Use these prompts for step-by-step control:

1. **Run Initial Test:**
   ```
   "Run [test-name] test"
   ```
   *Examples: "Run org-list test", "Run user-management test", "Run repo-details test"*

2. **Skip Passing Tests:**
   ```
   "Skip passing tests in [test-name]"
   ```
   *Examples: "Skip passing tests in org-list", "Skip passing tests in user-management"*

3. **Focus on First Failure:**
   ```
   "Focus on first failing test in [test-name]"
   ```
   *Examples: "Focus on first failing test in org-list", "Focus on first failing test in repo-details"*

4. **Move to Next Failure (repeat as needed):**
   ```
   "Move to next failing test in [test-name]"
   ```
   *Examples: "Move to next failing test in org-list", "Move to next failing test in user-management"*

5. **Finish Debugging Session:**
   ```
   "Remove all skips and rerun all tests in [test-name]"
   ```
   *Examples: "Remove all skips and rerun all tests in org-list", "Remove all skips and rerun all tests in repo-details"*

## Complete Workflow Example

### Scenario 1: Debugging [test-name].cy.ts with failing tests

```
You: "Debug [test-name] test failures"
System:
- Runs test and parses results
- Identifies passing tests: ['Test Name 1', 'Test Name 2']
- Identifies failing tests: ['Test Name 3', 'Test Name 4', 'Test Name 5']
- Creates modified file with passing tests skipped
- Focuses on first failing test ('Test Name 3')
- Provides debugging context and screenshots

You: [Fix the 'Test Name 3' test]

System:
- Re-runs test to verify fix
- If 'Test Name 3' now passes, automatically skips it by adding '.skip' to the test name
- Moves focus to next failing test ('Test Name 4')

You: [Fix the 'Test Name 4' test]

System:
- Re-runs test to verify fix
- If 'Test Name 4' now passes, automatically skips it by adding '.skip' to the test name
- Moves focus to next failing test ('Test Name 5')

You: [Fix the 'Test Name 5' test]

System:
- Re-runs test to verify fix
- If 'Test Name 5' now passes, automatically skips it by adding '.skip' to the test name
- All tests now pass

You: "Remove all skips and rerun all tests in [test-name]"
System:
- Removes '.skip' from all test names
- Re-runs the full test suite to verify all tests pass
- Confirms all fixes are working correctly
```

### Scenario 2: Debugging [test-name].cy.ts with no failures

```
You: "Debug [test-name] test failures"
System:
- Runs test and parses results
- Identifies passing tests: ['Test Name 1', 'Test Name 2', 'Test Name 3', 'Test Name 4', 'Test Name 5']
- Identifies failing tests: []
- Reports: "No test failures found. All tests are passing!"
- No debugging mode needed
```

## How It Works

### Test Status Identification
The system parses Cypress output to identify test status:
- `✓ Test Name` = passing test
- `1) Test Name` = failing test

### File Modification Pattern
- **Original:** `it('Test Name', () => { ... });`
- **Skipped:** `it.skip('Test Name', () => { ... });`
- **Focused:** Only one test without `.skip`, all others skipped
- **Final:** Remove '.skip' from all test names, then re-run the full test suite to verify all tests pass

### Dynamic Behavior
- After each fix, the system re-runs the test to verify the fix worked
- If the focused test now passes, it automatically skips it by adding '.skip' to the test name
- The system then moves focus to the next failing test
- This process repeats until all tests pass

## Benefits

### Time Savings
- **Faster test runs** - Skip passing tests to focus on failures
- **Focused debugging** - Work on one failure at a time
- **Automatic state management** - No manual file editing required

### Improved Workflow
- **Natural language control** - Use conversational commands
- **Automatic verification** - System confirms fixes work before moving on
- **Complete test coverage** - Final run verifies all tests pass together

### Safety Features
- **Preserves code fixes** - Never removes actual code changes
- **Backup protection** - Original file is backed up before modifications
- **Clear restoration** - Literal command removes only `.skip` statements

## Troubleshooting

### Common Issues
- **Test not found:** Ensure test file exists in `cypress/e2e/` directory
- **Command not recognized:** Check that the workflow is properly documented in quay-cursor.md
- **Tests still failing:** Use "Move to next failing test" to continue debugging

### Getting Help
- **Check test output** - Look for specific error messages in Cypress output
- **Review screenshots** - Check `cypress/screenshots/` directory for failure images
- **Verify fixes** - Use "Remove all skips and rerun all tests" to confirm all tests pass

## Best Practices

### Before Starting
- **Commit your changes** - Ensure you have a clean git state
- **Run tests once** - Verify the current test status
- **Review failures** - Understand what tests are failing and why

### During Debugging
- **Fix one test at a time** - Don't try to fix multiple tests simultaneously
- **Verify each fix** - Let the system confirm each fix works before moving on
- **Use screenshots** - Check failure screenshots for visual debugging clues

### After Debugging
- **Run final verification** - Always use "Remove all skips and rerun all tests"
- **Commit your fixes** - Save your working changes
- **Clean up** - Remove any temporary debugging code

---

*This workflow is designed to work with any Cypress test file in the Quay project. Replace `[test-name]` with your actual test file name (without the `.cy.ts` extension).*
