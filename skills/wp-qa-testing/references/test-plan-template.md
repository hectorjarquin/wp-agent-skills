# Test Plan: [FEATURE_NAME / COMPONENT_NAME]

**Recommended filename:** `[feature-slug]-qa-testing.md`

**Document Version:** 1.0  
**Date Created:** [Date]  
**Last Updated:** [Date]  
**Prepared By:** [Name and Role]  
**Reviewed By:** [Name and Role]  

---

## Executive Summary

[One paragraph: What is being tested? Why is this important? What are the key success criteria?]

---

## 1. Scope

### 1.1 What is being tested

[Features or components within scope]

- [Feature 1 — required by SRS FR-XX]
- [Feature 2 — required by SRS FR-YY]
- [Feature 3 — required by SRS FR-ZZ]

### 1.2 What is NOT being tested

[Out of scope, with brief rationale]

- [Out-of-scope item 1 — Reason]
- [Out-of-scope item 2 — Reason]

---

## 2. Testing Strategy

### 2.1 Overall Approach

[Describe the types of testing]

**Functional Testing:** Test each SRS Functional Requirement (FR) against its acceptance criteria  
**Integration Testing:** Test interactions between components and third-party plugins (e.g., Yoast)  
**Performance Testing:** [If applicable: Measure response time, throughput, etc.]  
**Security Testing:** [If applicable: Input validation, capability checks, data sanitization]  
**Compatibility Testing:** [Test against WordPress versions, PHP versions, plugins]  
**Regression Testing:** [Re-run critical tests on each build after features are complete]  

### 2.2 Testing Phases

| Phase | Focus | Duration | Frequency |
|-------|-------|----------|-----------|
| Phase 1 (Dev Testing) | Developers test features during implementation | Continuous | Per commit/merge |
| Phase 2 (QA Testing) | Systematic test case execution | 1–2 weeks | Once after dev complete |
| Phase 3 (Regression) | Re-test critical paths on final build | 3–5 days | Before release |

### 2.3 Risk-Based Prioritization

[If applicable: Identify high-risk areas and prioritize testing]

- **High Risk:** [Feature that is complex or has high business impact] → Test thoroughly
- **Medium Risk:** [Feature with moderate complexity] → Standard test coverage
- **Low Risk:** [Straightforward feature, isolated] → Basic test coverage

---

## 3. Test Environments

### 3.1 Local Development Environment

**Setup:**
- WordPress version(s): [e.g., 6.5, 6.6]
- PHP version(s): [e.g., 7.4, 8.0, 8.1]
- Active plugins: [List all relevant plugins, especially Yoast]
- Database: [Fresh install, test data fixtures, etc.]
- Browser(s): [Chrome latest, Firefox latest, Safari latest]

**How to set up:**
```bash
[Commands or instructions to set up environment]
```

**How to reset:**
```bash
[Commands or instructions to reset to clean state]
```

### 3.2 Staging Environment

**Location/URL:** [Staging URL]

**Setup:** [Same as above, or link to staging setup docs]

**Access:** [Who has access? How to provision?]

**Reset frequency:** [After each test cycle / On demand / Daily]

### 3.3 WordPress Playground (if applicable)

**Setup:** [URL or link to pre-configured Playground]

**Advantages:** Fast, disposable, reproducible

**Limitations:** No performance data, limited to browser environment

---

## 4. Test Coverage

### 4.1 Inline Coverage References: SRS Requirements → Test Cases

[Map each SRS Functional Requirement to at least one test case using inline references in each case, for example `> Covers: FR-18`]

| SRS FR | Description | Test Case ID(s) | Coverage |
|--------|-------------|-----------------|----------|
| FR-18 | [Description from SRS] | TC-001, TC-002 | ✓ Covered |
| FR-19 | [Description from SRS] | TC-003 | ✓ Covered |
| FR-20 | [Description from SRS] | [None] | ✗ GAP — needs test |
| FR-21 | [Description from SRS] | TC-004, TC-005, TC-006 | ✓ Covered |

[Identify and note any coverage gaps]

### 4.2 Quality Attributes (ISO 25010)

| Quality Attribute | Test Approach | Success Criteria |
|-------------------|----------------|-----------------|
| Functional Suitability | Execute all functional test cases | All SRS FR verified |
| Performance | Measure page load time, response time | < 2 seconds for [scenario] |
| Compatibility | Test on multiple WordPress/PHP versions | Works on [versions] |
| Security | Check input validation, capability checks | No XSS/SQL injection; security standards met |
| Usability | Manual verification of workflow clarity | Non-technical user can operate without error |
| Reliability | Test error handling, edge cases | 99% pass rate; graceful error handling |

---

## 5. Test Responsibilities

### 5.1 Roles

| Role | Responsibilities | Person/Team |
|------|-----------------|-------------|
| **Test Lead** | Plan, coordinate, report results | [Name] |
| **Developer (Testing)** | Execute unit/integration tests during development | [Names] |
| **QA Tester** | Execute formal test cases, document defects | [Name] |
| **Performance Tester** | Execute performance/load tests (if applicable) | [Name] |

### 5.2 Communication

- **Daily standup:** [Time and format]
- **Blocker escalation:** [Who to contact if tests are blocked?]
- **Defect reporting:** [How and where to report?]

---

## 6. Entry / Exit Criteria

### 6.1 Entry Criteria (When to START testing)

All of the following must be true:

- [ ] SRS is complete
- [ ] Code is ready for testing (unit tests pass, build is stable)
- [ ] Test environment is set up and verified
- [ ] Test cases are written and reviewed
- [ ] Test data / fixtures are prepared

**Review Note:** [Who confirmed entry criteria are met?]

### 6.2 Exit Criteria (When to STOP and RELEASE)

All of the following must be true:

- [ ] All critical and high-priority test cases have been executed
- [ ] Pass rate ≥ [X]% (e.g., 95%)
- [ ] All critical defects have been resolved
- [ ] All high-severity defects are resolved
- [ ] Medium/low-severity defects have been or will be [fixed before release / scheduled for post-release]
- [ ] Every SRS FR has at least one test case with inline `> Covers: FR-XX` coverage
- [ ] Release notes are prepared (list of known issues, if any)

**Release Decision Note:** [Who recorded release readiness and where?]

---

## 7. Test Cases

### 7.1 How Test Cases are Organized

[Test cases follow the format in `test-case-template.md`]

**Naming Convention:** TC-XXX (e.g., TC-001 tests FR-18, TC-002 tests FR-18 error case)

### 7.2 Test Case Summary

[Create a quick reference table]

| TC ID | SRS FR | Title | Type | Priority |
|-------|--------|-------|------|----------|
| TC-001 | FR-18 | Yoast detection on active site | Functional | Critical |
| TC-002 | FR-18 | Yoast detection when inactive | Functional | High |
| TC-003 | FR-19 | Schema suppression in Yoast mode | Functional | Critical |
| TC-004 | FR-20 | Native schema when Yoast inactive | Functional | High |
| TC-005 | FR-21 | Extension hook called with correct data | Functional | Medium |
| [More test cases...] | | | | |

---

## 8. Defect Management

### 8.1 Severity Levels

| Severity | Definition | Example | Impact |
|----------|-----------|---------|--------|
| **Critical** | Feature completely broken or data loss | Schema fails to render; FAQ content deleted | Blocks release |
| **High** | Feature unusable or major impact | Performance severely degraded; workflow broken | Must fix for release |
| **Medium** | Workaround exists; moderate impact | Minor UI inconsistency; occasional error | Fix before release or schedule for patch |
| **Low** | Cosmetic or edge-case issue | Typo in label; error on rare condition | Nice to fix; can defer |

### 8.2 Defect Workflow

1. **Find:** Tester encounters unexpected behavior
2. **Report:** File defect with steps to reproduce, actual vs. expected outcome
3. **Triage:** Assign severity; assign to developer
4. **Fix:** Developer resolves issue
5. **Verify:** Tester confirms fix works (retest)
6. **Close:** Defect marked resolved

**Defect Tracker:** [Link to issue tracking system]

---

## 9. Schedule

| Milestone | Date | Duration |
|-----------|------|----------|
| Test planning complete | [Date] | 1 week |
| Test environment ready | [Date] | 2 days |
| Test cases written | [Date] | 3 days |
| Development complete | [Date] | [As scheduled] |
| QA Testing phase | [Date] | 1–2 weeks |
| Fix & retest phase | [Date] | [As needed] |
| Release decision | [Date] | 1 day |

---

## 10. Risks & Mitigation

| Risk | Impact | Likelihood | Mitigation |
|------|--------|-----------|-----------|
| [Risk 1: e.g., Test environment not ready] | Delays testing | [High/Med/Low] | [Set up environment early; use backup] |
| [Risk 2: e.g., Tester unavailable] | Gaps in coverage | [High/Med/Low] | [Cross-train backup tester] |
| [Risk 3: e.g., Third-party plugin incompatibility] | Undiscovered defects | [High/Med/Low] | [Test with all compatible versions] |

---

## 11. Tools & Automation

### 11.1 Testing Tools

- **Test Case Management:** [Tool name, e.g., "Spreadsheet" or "TestRail"]
- **Defect Tracking:** [Tool name, e.g., "GitHub Issues"]
- **Automated Testing:** [If applicable, e.g., "PHPUnit for unit tests"]
- **Performance Monitoring:** [If applicable, e.g., "Query Monitor, WordPress Playground"]

### 11.2 Continuous Verification

[If applicable]

- Unit tests run on every commit (via CI/CD)
- [Other automated checks]

---

## 12. Review Summary

**Test Plan Reviewed By:**

| Role | Name | Date | Note |
|------|------|------|-----------|
| Test Lead | [Name] | [Date] | [Review complete] |
| Development Lead | [Name] | [Date] | [Review complete] |
| Project Manager | [Name] | [Date] | [Review complete] |

---

## Appendix A: SRS Functional Requirements Reference

[List the SRS Functional Requirements that this test plan covers, for quick reference]

- FR-18: [Title and brief description]
- FR-19: [Title and brief description]
- FR-20: [Title and brief description]
- FR-21: [Title and brief description]

[Source: SRS document link]

---

## Appendix B: Test Case Templates

[Reference to test-case-template.md location]

---

**Document Status:** [Draft / Reviewed / In Use]

**Last Updated:** [Date]

**Traceability:** [SRS: FR-18 through FR-21]
