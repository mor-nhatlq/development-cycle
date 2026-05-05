# Change Impact Map · V1 → V2

Mapping V1 modules → V2 changes.

## Module Impact Matrix

| Module | V1 Status | V2 Change | Risk |
|---|---|---|---|
| auth-service | stable | — | low |
| kyc-service | stable | + Vietin eKYC fallback | low |
| wallet-service | ⚠ god module | **SPLIT** into ledger-service + balance-cache | **HIGH** |
| transfer-service | stable | + multi-currency hooks | medium |
| payment-service | stable | + bill-payment adapter | medium |
| **NEW** bill-service | — | EVN/Internet/Water/TV adapters | medium |
| **NEW** topup-service | — | Viettel/Vinaphone/Mobifone | medium |
| notification-service | stable | + bill-paid template | low |

## Database Impact

```
V1 (shared `vietpay` schema):
  ├─ wallets        → SPLIT to wallet_db.wallets
  ├─ transfers      → SPLIT to transfer_db.transfers
  └─ transactions   → SPLIT to ledger_db.entries

V2 (per-service):
  ├─ wallet_db      (wallet-service owns)
  ├─ ledger_db      (ledger-service owns — NEW)
  ├─ transfer_db    (transfer-service owns)
  ├─ bill_db        (bill-service owns — NEW)
  └─ topup_db       (topup-service owns — NEW)
```

## API Contract Impact

- ✅ All V1 client APIs remain backward compatible (no breaking change)
- 🔄 Internal service APIs: 12 endpoints refactored
- ➕ 8 new endpoints added (bills + topup)

## Mobile App Impact

- New bottom-tab "Bills" added (replace placeholder)
- Topup entry point in home dashboard
- Settings: link new bill providers
- App version bump v1.0 → v2.0

## Estimated Blast Radius

- **Code touched:** ~32k LOC modified, ~18k LOC new
- **Services restarted (cutover):** 6 of 8
- **Estimated downtime:** 0 (dual-write strategy)
- **Rollback window:** 7 days (old DB read-only)
