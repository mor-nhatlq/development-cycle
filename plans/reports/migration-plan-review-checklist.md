# Migration Plan Review Checklist · V1 → V2

**Date:** 2026-06-15 · **Reviewers:** Tech Lead, PM, SRE Lead

## Strategy Review

- ✅ Strangler Fig + Dual Write strategy chosen
- ✅ 8-week timeline realistic
- ✅ Cutover window identified (Sunday 02:00-04:00 ICT)
- ✅ Rollback window 7 days (old DB read-only)

## Sections

### [1] Schema Design
- ✅ New per-service schemas defined
- ✅ Foreign key boundaries clean
- ✅ Index strategy documented

### [2] Dual-Write Implementation
- ✅ Idempotent writes to both DBs
- ✅ Failure mode: write to new, retry on old async
- ✅ Conflict resolution: new DB wins after T+1week

### [3] Reconciliation
- ✅ Hourly diff job (sum of balances, txn counts)
- ✅ Auto-fix tolerance: < 10 entries/hour
- ✅ Alert if drift > 0.01% wallet sum

### [4] Read Traffic Ramp
- ✅ 5% → 25% → 100% with 24h soak each
- ✅ Per-route override capability (config-driven)

### [5] Cutover
- ✅ Pre-cutover checklist (15 items)
- ✅ Go/No-Go criteria defined
- ✅ Communication plan (status page + in-app)

### [6] Rollback
- ✅ Toggle to old DB read-only
- ✅ Practiced in staging 3 times
- ✅ Max rollback time: 8 minutes

### [7] Risk Coverage
- ✅ All 10 risks from risk-register.md addressed
- ✅ Mitigations validated in staging

## Decision

**APPROVED** · proceed with V2 migration W3 cutover.
