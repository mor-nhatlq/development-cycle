# V2 Test Coverage Plan

## Strategy

Tests gate cutover. No deploy without 95%+ coverage on touched code + green E2E.

## Coverage Targets

| Layer | Target | Current V1 |
|---|---|---|
| Unit tests | ≥ 95% lines, 90% branches | 96.2% / 91% |
| Integration tests | All service boundaries | 88% endpoints |
| Contract tests | All inter-service APIs | NEW for V2 |
| E2E happy path | 100% critical flows | 24/24 |
| E2E edge cases | ≥ 80% identified | 67% |

## Critical Test Suites

### Migration Tests (NEW)
- Dual-write consistency (read from both DBs, assert equal)
- Reconciliation job correctness (synthetic drift → auto-fix)
- Cutover dry-run (read traffic ramp 5% → 100% in staging)

### Bill Service Tests
- EVN lookup: valid customer code → bill returned
- EVN lookup: invalid → 404 with Vietnamese error message
- Pay bill: idempotency (same key returns same result)
- Pay bill: insufficient balance → 402 + topup CTA
- Pay bill: provider timeout 8s → queued + push notification

### Topup Service Tests
- Topup all carriers (Viettel/Vinaphone/Mobifone/Vietnamobile)
- Auto-refund on telco fail confirmation
- Provider failover (primary down → fallback selected)

### Wallet Split Tests
- Old wallet API still works during dual-write
- Internal callers (12 services) all green
- Ledger consistency: sum of entries = wallet balance

## Test Execution

- CI: every PR runs unit + integration (~6 min)
- Nightly: full E2E suite (~22 min)
- Pre-cutover: full suite + chaos test (DB failover, telco timeout)

## Sign-off Criteria

- ✅ All targets met
- ✅ Zero P0/P1 bugs open
- ✅ QA Lead sign-off
- ✅ Tester agent report green
