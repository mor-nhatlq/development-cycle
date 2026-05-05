# VietPay Project Changelog

## [v1.0.0] — 2026-06-05 (MVP Launch)

### Added
- P2P money transfer với idempotency key (10M VND/giao dịch · 100M/ngày)
- Sepay VietQR payment integration
- VNPT eKYC adapter với liveness detection
- Wallet ledger service (PostgreSQL, atomic debit/credit)
- Transaction history với cursor pagination
- FCM + APNs push notifications
- Bank account linking (Vietcombank, Techcombank, MB)
- iOS + Android apps (TestFlight #142 · Play Store internal track)

### Security
- OWASP Top-10 audit clean
- AML/KYC compliance per SBV regulations
- PCI-DSS Level 1 prep
- Service-to-service mTLS enforced

### Performance
- Transfer p95 < 3s end-to-end
- 99.9% uptime SLA target
- Multi-region deployment (ap-southeast-1, eu-central-1)

## [v0.9.0-rc] — 2026-05-28

- Hardening sprint complete
- 218/220 tests passing (96% coverage)
- Smoke tests 24/24 ✓

## [v0.8.0-beta] — 2026-05-15

- All 8 MVP phases code complete
- Code review + security audit closed (3 HIGH findings fixed)

## [v0.1.0] — 2026-04-01

- Project kickoff · Planner Agent generated 12-week plan
- Tech stack approved (NestJS · PostgreSQL · React Native · Kafka)
