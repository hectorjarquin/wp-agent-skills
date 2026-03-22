# QA Testing: <feature-slug>

**Profile:** Team
**Status:** Reviewed
**Reviewed By:** QA Lead

## 1. Scope and Strategy

In scope:
- main workflow verification
- dependency-aware behavior
- regression of critical paths

Test strategy:
- functional verification of FR-01 through FR-04
- integration verification where optional dependencies affect behavior
- regression testing before release decision

## 2. Environments

- Local developer environment
- Shared staging environment

## 3. Coverage Summary

| FR | Test Case IDs | Coverage |
|---|---|---|
| FR-01 | TC-001, TC-002 | Covered |
| FR-02 | TC-003 | Covered |
| FR-03 | TC-004 | Covered |
| FR-04 | TC-005 | Covered |

## 4. Test Case Example

### TC-004: Optional dependency path falls back safely

> Covers: FR-03

- Preconditions: optional dependency unavailable
- Steps:
  1. run the workflow that would normally use the dependency
  2. observe the returned behavior
- Expected result: workflow completes with approved fallback behavior

## 5. Readiness and Review

- Reviewer recorded: Yes
- Entry criteria confirmed: Yes
- Release readiness note: ready for UAT when critical tests pass