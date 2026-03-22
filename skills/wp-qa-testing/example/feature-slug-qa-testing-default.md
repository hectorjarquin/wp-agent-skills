# QA Testing: <feature-slug>

**Profile:** Default
**Status:** Draft

## 1. Scope

- verify the primary workflow
- verify required validation behavior
- verify safe failure when dependencies are unavailable

## 2. Test Coverage

| FR | Test Case | Note |
|---|---|---|
| FR-01 | TC-001 | Primary workflow executes successfully |
| FR-02 | TC-002 | Invalid input is rejected |
| FR-03 | TC-003 | Dependency failure produces safe fallback |

## 3. Test Cases

### TC-001: Primary workflow succeeds

> Covers: FR-01

- Preconditions: supported environment is available
- Steps: trigger the main workflow
- Expected result: successful outcome is visible to the user

### TC-002: Validation failure is handled

> Covers: FR-02

- Preconditions: invalid or incomplete input exists
- Steps: submit the workflow with invalid input
- Expected result: user sees corrective guidance and no invalid state is saved

## 4. Exit Note

- No separate traceability matrix required for this example.
- Default profile uses inline coverage and a consolidated QA record.