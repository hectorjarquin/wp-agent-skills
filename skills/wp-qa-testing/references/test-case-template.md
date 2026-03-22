# Test Case Template

**Recommended filename:** `[feature-slug]-qa-testing.md`

**[Copy this template for each test case]**

---

## Test Case: [TC-XXX]

**SRS Requirement(s):** FR-XX, FR-YY (if multiple)

**Coverage Reference:** `> Covers: FR-XX, FR-YY`

**Title:** [Brief, clear description of what is being tested]

**Category:** [Functional / Integration / Performance / Security / Compatibility / Regression]

**Priority:** [Critical / High / Medium / Low]

**Created Date:** [Date]

**Author:** [Name]

---

## Preconditions

[What must be true BEFORE this test starts?]

- [ ] [Precondition 1: e.g., WordPress 6.5+ installed]
- [ ] [Precondition 2: e.g., Yoast SEO plugin active]
- [ ] [Precondition 3: e.g., FAQ block added to page]
- [ ] [Precondition 4: e.g., Test data loaded (10 FAQ items)]

---

## Test Environment

**WordPress Version:** [e.g., 6.5]

**PHP Version:** [e.g., 8.0]

**Browser:** [e.g., Chrome latest, Firefox latest]

**Active Plugins:** [List all, especially third-party]

**Browser Extensions:** [Any that might affect testing, e.g., ad blockers]

---

## Test Steps

[Numbered, clear, repeatable steps. Each step should be a single action.]

### Step 1: [Action]

**What to do:** [Clear, imperative instruction]

Example command or UI path:
```
Navigate to: wp-admin → Pages → [Page name]
```

**Expected intermediate result:** [What should you see after this step?]

### Step 2: [Action]

**What to do:** [Next action]

Example:
```
Click "Add Block"
Search for "FAQ"
```

**Expected intermediate result:** [What should appear?]

### Step 3: [Action]

[Continue for as many steps as needed]

---

## Expected Final Outcome

[After all steps, what should the final state be?]

**Visible outcome:**
- [What should appear on screen?]
- [What should the page/content look like?]

**Data/state outcome:**
- [What was created, changed, or deleted?]

**Performance outcome (if applicable):**
- Page loaded in < 2 seconds
- No console errors

**Verifiable proof:**
- [How to verify success? What to look for?]
- Example: "Open browser developer tools → Console → No error messages"

---

## Pass Criteria

[The test PASSES if ALL of the following are true]

- [ ] Step 1 result matches expected
- [ ] Step 2 result matches expected
- [ ] [Additional step checkboxes]
- [ ] Final outcome is achieved
- [ ] No unexpected error messages
- [ ] [Performance/security criteria if applicable]

---

## Fail Criteria

[The test FAILS if ANY of the following occur]

- [ ] Any step result differs from expected
- [ ] Error message appears
- [ ] Expected data is missing or incorrect
- [ ] Performance threshold exceeded (if applicable)
- [ ] [Other potential failure modes]

---

## Test Execution Record

### Execution 1

**Date Executed:** [Date]

**Executed By:** [Name]

**Environment:** [WordPress version, plugins, etc.]

**Result:** [PASS / FAIL / BLOCKED]

**Notes:** [Any observations, environment quirks, etc.]

[If FAIL, document]:
- **Actual Result:** [What actually happened?]
- **Expected Result:** [What was supposed to happen?]
- **Defect ID:** [Link to filed defect, if applicable]

---

### Execution 2

[Repeat the above for retests or regression runs]

**Date Executed:** [Date]

**Executed By:** [Name]

**Result:** [PASS / FAIL / BLOCKED]

**Fix Verification:** [If this was a retest: was the defect fixed? Details.]

---

## Notes & Issues

- [Any gotchas or tricks needed to execute this test]
- [Environment-specific issues to know about]
- [If certain plugins aren't installed, does this test still apply?]
- [Known flakiness or race conditions]

---

## Related Tests

[Other test cases that might interact with this one]

- TC-XXX (Tests related feature)
- TC-YYY (Tests edge case)

---

## Coverage Link

**SRS Requirement:** [FR-XX — Full requirement title]

[Link to SRS document]

---

**Test Case Status:** [Draft / Ready / Deprecated]

**Last Updated:** [Date]
