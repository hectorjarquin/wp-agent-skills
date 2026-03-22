# User Acceptance Testing (UAT) Execution Record

**Recommended filename:** `[feature-slug]-ua-testing.md`

**Test Session ID:** UAT-[YYYYMMDD]-[Stakeholder Initial]

**Feature/Component:** [What is being tested?]

**Stakeholder/Tester:** [Name and role]

**Test Date:** [Date]

**Test Environment:** [URL, WordPress version, plugins]

---

## Acceptance Scenario

**Scenario ID:** AS-XXX

**Title:** [Scenario name from UAT plan]

**Business Value:** [Why does this scenario matter to the business?]

**Stakeholder:** [Who is testing this?]

---

## Scenario Preconditions

[What must be true before testing begins?]

- [ ] [Precondition 1]
- [ ] [Precondition 2]
- [ ] [Precondition 3]

**Preconditions verified by:** [Who confirmed the environment is ready?] at [Time]

---

## Scenario Test Steps & Observations

[Follow the scenario defined in the UAT plan. Record actual observations.]

### Step 1: [Action from scenario]

**Expected:** [What should happen]

**Actual:** [What actually happened]

**Screenshots/Evidence:**
- [Screenshot file name or description]
- [Any data capture needed]

**Status:** [ ] Pass  [ ] Fail

**Notes:** [Any observations or issues]

---

### Step 2: [Action from scenario]

**Expected:** [What should happen]

**Actual:** [What actually happened]

**Screenshots/Evidence:**
- [Screenshot file name or description]

**Status:** [ ] Pass  [ ] Fail

**Notes:** [Any observations]

---

[Repeat for all steps in the scenario]

---

## Acceptance Criteria Verification

[From the UAT plan: test each acceptance criterion]

### Criterion 1: [Acceptance criterion from plan]

**Measurement/Test:** [How to verify this]

**Result:** [PASS / FAIL]

**Evidence:** [How you know it passed/failed. Screenshots, data, etc.]

**Notes:** [Any caveats or context]

---

### Criterion 2: [Acceptance criterion from plan]

**Measurement/Test:** [How to verify this]

**Result:** [PASS / FAIL]

**Evidence:** [Screenshots, data, etc.]

---

[Repeat for each acceptance criterion]

---

## Overall Scenario Result

**Pass / Fail / Conditional**

Choose one:

- [ ] **PASS** — All steps and acceptance criteria met; no issues
- [ ] **FAIL** — One or more issues prevent acceptance; see defects below
- [ ] **CONDITIONAL** — Minor issues exist but acceptable for this release; see notes below

---

## Issues / Defects Found

[If any steps or criteria failed, document each issue]

### Issue 1

**Description:** [What went wrong?]

**Severity:** [Critical / High / Medium / Low]

**Steps to Reproduce:** [How to trigger the issue]

**Expected Behavior:** [What should happen]

**Actual Behavior:** [What actually happened]

**Screenshots/Evidence:** [Attach or describe]

**Impact on Acceptance:** [Does this block acceptance? Why?]

**Resolution Path:** [How should this be fixed?]

**Tracking:** [Link to defect tracker if filed]

---

### Issue 2

[Repeat the above format for each issue]

---

## Stakeholder Feedback

[Comments from the testing stakeholder]

**General Impression:** [Overall satisfaction with the feature]

**Strengths:** [What works well?]

**Concerns:** [Any hesitations or worries?]

**Questions:** [Anything unclear about the feature?]

**Suggestions:** [Any improvement ideas, even if out of scope?]

---

## Quality Attributes Assessment

[From ISO 25010 quality model, if applicable]

| Quality Attribute | Satisfactory? | Evidence / Notes |
|-------------------|---------------|-----------------|
| **Functional Suitability** | Yes / No / Partial | [Does it do what was promised?] |
| **Performance Efficiency** | Yes / No / Partial | [Is it fast enough?] |
| **Usability** | Yes / No / Partial | [Is it easy to use?] |
| **Reliability** | Yes / No / Partial | [Is it stable and dependable?] |
| **Security** | Yes / No / Partial | [Are data and permissions secure?] |
| **Compatibility** | Yes / No / Partial | [Does it work with other systems?] |

---

## Decision Date/Time

**Scenario tested on:** [Date] at [Time]

**Decision note:** [Recorded decision source, e.g., release note, issue comment]

---

## Next Steps

[ ] **GO** — Feature ready for production

[ ] **REWORK** — Issues must be resolved; retest needed

[ ] **HOLD** — Decision pending; retest date TBD at [Date]

**Decision made by:** [Name/Role]

**Date of decision:** [Date]

---

## Recorded Decision

**Tester Name:** [Print name]

**Tester Confirmation:** [Name or digital confirmation]

**Tester Role:** [Title]

**Date:** [Date]

**Contact if questions:** [Email or phone]

---

## Attachments

[List any screenshots, logs, or evidence files]

- [File 1: UAT-[ID]-screenshot-1.png]
- [File 2: UAT-[ID]-error-log.txt]
- [File 3: ...]

---

**Traceability:**

- Scenario: AS-XXX (from UAT Plan)
- Stakeholder Need: SN-XX (from StRS)
- Operational Scenario: OPS-XX (from OpsCon)
- SRS Functional Requirements: FR-XX, FR-YY (from SRS)

---

**Document Status:** [Draft / Submitted / Recorded]

**Last Updated:** [Date]
