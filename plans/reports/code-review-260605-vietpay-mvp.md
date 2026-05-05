# Code Review Report · VietPay MVP

**Date:** 2026-06-05 · **Reviewer:** code-reviewer agent + Tech Lead

## Findings Summary

3 HIGH · 4 MED · 6 LOW

### [HIGH-01] Race condition · transfer.service.ts:67
2 concurrent transfers tới cùng wallet có thể double-credit.
**Fix:** SELECT FOR UPDATE trong DB transaction.

### [HIGH-02] SQL injection · transactions.repository.ts:42
String concat trong query → injection risk.
**Fix:** parameterized query.

### [HIGH-03] Missing idempotency check · api/transfers.controller.ts:23
Retry path không check `Idempotency-Key` → duplicate transfer.
**Fix:** wire idem-key middleware before service call.

### [MED-01] Missing input validation · kyc.controller.ts:18
National ID format không validate Vietnamese pattern.

### [MED-02] Hardcoded timeout · sepay.adapter.ts:45
8s timeout hardcoded → should be env-configurable.

### [MED-03] Logging PII · auth.service.ts:92
Phone number logged at INFO level → mask before log.

### [MED-04] Missing index · transfers table
Query `WHERE user_id AND created_at` slow → composite index needed.

### LOW
- 6 inconsistent error envelope formats
- 4 missing JSDoc on public APIs
- 2 unused imports

## Decision

**Status:** REWORK_REQUIRED on HIGH-* findings.
After fix → re-run tester → human final review.
