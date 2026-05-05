# Tech Debt Inventory · V1

Snapshot taken before V2 planning.

## Priority Matrix

| ID | Item | Severity | Effort | Pay-down in |
|---|---|---|---|---|
| TD-01 | Wallet+transfer share `vietpay` schema | **HIGH** | 3w | V2 |
| TD-02 | wallet-service god module (18k LOC) | **HIGH** | 2w | V2 |
| TD-03 | No idempotency on /api/v1/topups (legacy endpoint) | **HIGH** | 3d | V2 |
| TD-04 | Transfer service uses string concat in raw SQL (1 spot) | MED | 1d | hotfix |
| TD-05 | Hardcoded Sepay endpoint (no env override) | MED | 1d | V2 |
| TD-06 | No retry/backoff on FCM push | MED | 2d | V2 |
| TD-07 | E2E test flakiness ~5% (timing issues) | LOW | 5d | V2 |
| TD-08 | Inconsistent error response envelope across 6 services | LOW | 1w | V2 |
| TD-09 | Missing OpenAPI spec for 4 internal endpoints | LOW | 3d | V2 |

## High-Severity Detail

### TD-01 · Shared DB Schema
- **Why bad:** wallet-service direct SQL on `transfers` table → tight coupling
- **Plan:** strangler-fig migration W3-W7
- **Mitigation during cutover:** dual-write to both schemas, monitor consistency

### TD-02 · Wallet God Module
- **Why bad:** 18k LOC, mixes ledger + balance cache + analytics
- **Plan:** extract `ledger-service` + keep wallet-service as facade
- **Risk:** breaking changes to internal callers (12 services touch wallet)

### TD-03 · Topup Idempotency
- **Why bad:** retry without idem-key → potential double charge
- **Plan:** add `Idempotency-Key` header (24h Redis SETNX guard)
- **Backward compat:** if header missing, fallback to UUID + log warning

## Estimated V2 debt budget

- 22 dev-days allocated
- Owner: backend platform team
- Tracking: Linear project `VP-DEBT`
