# Architecture Traceability Matrix

## Purpose

This matrix maps architecture artifacts to upstream requirements.

## Rules

1. Every major architecture decision (AD-XX) must map to at least one FR-XX.
2. Any untraced major decision should be documented and followed up.
3. If a requirement has no architecture coverage, flag as a gap.

## Matrix

| Requirement ID | Requirement Summary | Architecture Item | Coverage Type | Notes |
|---|---|---|---|---|
| SYRS-01 | [Summary] | AV-01 | Direct | [Coverage note] |
| FR-18 | [Summary] | AD-01 | Direct | [Coverage note] |
| FR-19 | [Summary] | AD-02 | Direct | [Coverage note] |
| FR-20 | [Summary] | AV-02 | Indirect | [Coverage note] |

## Gaps

| Gap ID | Requirement or Architecture Item | Gap Description | Action Required |
|---|---|---|---|
| G-01 | [FR-XX] | [No architecture coverage] | [Add AD-XX or AV-XX] |
| G-02 | [AD-XX] | [No requirement mapping] | [Map to FR-XX or remove decision] |

## Review Notes

- [ ] Matrix reviewed
- [ ] Open gaps captured with follow-up action
- **Reviewer:** [Name]
- **Date:** [YYYY-MM-DD]
