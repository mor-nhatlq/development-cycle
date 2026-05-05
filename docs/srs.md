# Software Requirements Specification — VietPay MVP

**Document version:** 1.0 · **Date:** 2026-04-30 · **Author:** Docs Manager Agent
**Status:** Approved by Product Owner + Compliance + Eng Lead

---

## 1. Introduction

### 1.1 Purpose
Tài liệu SRS định nghĩa requirements cho VietPay MVP — ví điện tử tập trung vào P2P transfer + VietQR payment cho thị trường Việt Nam.

### 1.2 Scope
**In scope (MVP):**
- Sign-up + eKYC (CMND/CCCD scan + selfie liveness)
- Link bank account (top 10 banks VN qua Sepay)
- P2P money transfer (phone / QR / bank account)
- VietQR payment (generate + scan)
- Transaction history với filter
- Push notifications (FCM + APNs)
- Biometric authentication (Face ID / Fingerprint)

**Out of scope (later phases):**
- Bill payment / mobile top-up (Phase 2)
- Goal-based savings (Phase 2)
- Investment products (Phase 3)
- Web app (Phase 3)

### 1.3 Definitions
- **VietQR:** chuẩn QR thanh toán quốc gia VN (Napas)
- **eKYC:** electronic Know Your Customer
- **SBV:** State Bank of Vietnam (Ngân hàng Nhà nước)
- **Idempotency Key:** UUID unique cho mỗi transaction request, tránh double-spend
- **Wallet:** ví VND của user trong VietPay (separate ledger)

---

## 2. Functional Requirements

### FR-001 — User Registration
**Priority:** P0 (MVP) · **Source:** US-001
- User register bằng số điện thoại VN (+84)
- OTP 6-digit, hiệu lực 5 phút
- Set PIN 6-digit cho transaction
- Enable biometric (optional, recommend during onboarding)

**Acceptance:**
- Hỗ trợ tất cả carrier VN (Viettel, VinaPhone, MobiFone, Vietnamobile)
- Rate limit: 3 OTP / số / giờ
- PIN không cho phép sequence (123456) hoặc repeat (111111)

### FR-002 — eKYC Verification
**Priority:** P0 · **Source:** US-002, SBV Circular 23/2019
- Scan CMND hoặc CCCD (front + back)
- Selfie với liveness detection (blink / turn head)
- OCR + face matching qua VNPT eKYC API
- Manual review queue nếu confidence < 90%

**Acceptance:**
- Pass rate ≥ 92% tự động (không manual review)
- KYC data encrypted AES-256-GCM at rest
- Lưu hồ sơ KYC 5 năm (per SBV)
- Rejection lý do rõ ràng cho user

### FR-003 — Link Bank Account
**Priority:** P0 · **Source:** US-003
- Hỗ trợ top 10 bank VN: Vietcombank, BIDV, VietinBank, Agribank, MB Bank, Techcombank, ACB, VPBank, TPBank, Sacombank
- Verify ownership qua Sepay micro-deposit (gửi 1k-3k VND, user xác nhận số tiền)
- Lưu masked account number, tokenize qua Sepay

**Acceptance:**
- Verification time < 60 giây
- Tối đa 3 bank account / user
- User có thể remove bank account (sau 24h grace period)

### FR-004 — Wallet Top-up
**Priority:** P0 · **Source:** US-004
- Top-up từ linked bank account
- Min: 10,000 VND, Max: 50,000,000 VND / lần
- Daily limit: 100,000,000 VND

**Acceptance:**
- Settle vào wallet trong < 30 giây
- Notification khi success / fail
- Idempotent qua Idempotency-Key header

### FR-005 — P2P Transfer (Phone)
**Priority:** P0 · **Source:** US-005
- Send VND tới VietPay user khác qua phone number
- Min: 10,000 VND, Max: 10,000,000 VND / giao dịch
- Daily limit: 100,000,000 VND
- Optional note (256 ký tự)

**Acceptance:**
- Recipient phone validated as VietPay user
- Biometric or PIN confirm required
- Idempotent (Idempotency-Key required)
- E2E latency < 3 giây
- Push notification cho recipient < 5 giây
- Cancellable trong 30 giây nếu recipient chưa "claim"

### FR-006 — P2P Transfer (Bank Account)
**Priority:** P0 · **Source:** US-006
- Send VND tới bank account ngoài VietPay
- Same limits as FR-005
- Routing qua Sepay (NAPAS 24/7)

**Acceptance:**
- Settle trong < 30 giây (banking hours), < 5 phút (off-hours)
- Fee: free first 3 / month, sau đó 5,000 VND / lần
- Receipt PDF có thể tải về

### FR-007 — VietQR Generate (Receive)
**Priority:** P0 · **Source:** US-007
- Generate static QR (link với wallet, không amount)
- Generate dynamic QR (specific amount + note + expiry 15 phút)
- Format: VietQR EMV chuẩn Napas

**Acceptance:**
- QR scan được bằng app bank khác (Vietcombank, BIDV, ...)
- Display rõ amount + note để verify
- Auto-refresh dynamic QR khi expire

### FR-008 — VietQR Scan (Pay)
**Priority:** P0 · **Source:** US-008
- Scan QR từ camera
- Parse merchant info + amount
- Confirm screen → biometric → execute

**Acceptance:**
- Camera permission flow rõ ràng
- Hỗ trợ flash mode (low light)
- Manual entry fallback nếu camera lỗi
- Edit amount cho static QR (không amount)

### FR-009 — Transaction History
**Priority:** P0 · **Source:** US-009
- List tất cả transactions, latest first
- Filter: date range, type (in/out), category
- Search: phone / amount / note
- Detail view: full transaction info + receipt

**Acceptance:**
- Cursor pagination, 20 items / page
- Initial load < 500ms
- Export CSV cho date range (max 90 days)
- Auto-categorize qua merchant database + ML

### FR-010 — Push Notifications
**Priority:** P0 · **Source:** US-010
- Notification cho: receive money, send success, top-up complete, security alert
- FCM (Android) + APNs (iOS)
- In-app inbox lưu 90 ngày
- Notification preferences (granular toggle per type)

**Acceptance:**
- Delivery rate ≥ 95% trong 30 giây
- Localization VN + EN
- Deep link tới relevant screen

### FR-011 — Security: Biometric Auth
**Priority:** P0 · **Source:** US-011
- Face ID / Touch ID (iOS) · Fingerprint / Face Unlock (Android)
- Required cho mọi transaction ≥ 500,000 VND
- Required cho login sau 24h idle
- Fallback PIN nếu biometric fail 3 lần

**Acceptance:**
- Enrolled trong onboarding (optional, can skip)
- Re-enroll khi đổi device
- Disable nếu device bị jailbreak / root

### FR-012 — Account Settings
**Priority:** P0 · **Source:** US-012
- Change PIN (với current PIN verify)
- Manage linked bank accounts
- Notification preferences
- Logout / logout all devices
- Delete account (30-day grace period, KYC data lưu 5 năm per SBV)

**Acceptance:**
- Change PIN gửi notification + email confirm
- Logout all clears all sessions trong < 5 giây
- Delete account flow có 3 bước confirm + email

---

## 3. Non-Functional Requirements

### NFR-001 — Performance
- API p50 latency < 100ms, p95 < 200ms, p99 < 500ms
- App cold start < 2s (iOS), < 3s (Android)
- Transaction history load < 500ms initial, < 200ms paginated

### NFR-002 — Availability
- SLA 99.9% uptime (43.8 phút downtime / tháng cho phép)
- Multi-AZ deployment, RTO < 5 phút, RPO < 1 phút
- Graceful degradation: nếu Sepay down, queue + retry với user notification

### NFR-003 — Scalability
- Hỗ trợ 100k concurrent users tại launch
- Scale horizontally tới 1M MAU không re-architect
- Database: read replica + connection pooling

### NFR-004 — Security
- TLS 1.3 cho client-server, TLS 1.2+ cho service-to-service
- Encryption at rest: AES-256-GCM (DB, S3, Redis snapshot)
- Secrets via AWS KMS, không hardcode
- Rate limiting: 100 req/min per user authenticated, 10 req/min unauthenticated
- OWASP Top 10 audit hàng quý
- Penetration test trước launch

### NFR-005 — Compliance
- SBV Circular 23/2019 + 17/2024 (e-wallet licensing)
- AML reporting: auto-flag giao dịch ≥ 400M VND đơn lẻ, hoặc tổng ≥ 600M / ngày
- PCI-DSS Level 1 (qua Sepay tokenization, không lưu PAN)
- Personal Data Protection Law NĐ 13/2023
- KYC retention 5 năm
- Audit log mọi truy cập PII (immutable, lưu S3 object lock)

### NFR-006 — Usability
- Vietnamese primary, English secondary
- Onboarding completable trong < 5 phút (target < 3)
- WCAG 2.1 Level AA compliance
- Support iOS 14+ và Android 8+ (cover 95% market)

### NFR-007 — Reliability
- Idempotent mọi mutation API (Idempotency-Key required)
- At-least-once delivery cho events qua Kafka, dedup ở consumer
- Distributed tracing qua DataDog APM
- Crash-free rate ≥ 99.5% (Sentry)

### NFR-008 — Maintainability
- Code coverage ≥ 80% (line), ≥ 70% (branch)
- File size < 200 LOC, function < 50 LOC
- Conventional commits, semver
- ADR (Architecture Decision Records) cho major decisions

---

## 4. Constraints

- **Budget:** 8 backend devs + 4 mobile devs + 2 designers + 1 PM + 1 QA × 12 tuần
- **Timeline:** Internal beta tuần 12, public launch tuần 16
- **Tech debt:** Không inherit codebase, build from scratch
- **Vendor lock-in tolerance:** AWS OK (infra), Sepay OK (fallback plan), VNPT eKYC OK (alternative FPT identified)

---

## 5. Assumptions

- Sepay sandbox env stable trong giai đoạn dev
- VNPT eKYC SLA 99.5% (per SLA contract)
- Apple Developer Account + Google Play Console đã sẵn sàng
- SBV licence apply parallel với dev, expected approve tuần 14

---

## 6. Acceptance Criteria for MVP Release

- ✅ All P0 FRs implemented + tested
- ✅ All NFRs validated (load test + security audit + compliance review)
- ✅ Code coverage ≥ 80% line
- ✅ Zero P0/P1 bugs open
- ✅ App Store + Play Store internal track approved
- ✅ SBV licence in hand
- ✅ Beta test với 500 internal users, NPS ≥ 40

---

## 7. Glossary

| Term | Definition |
|------|------------|
| MAU | Monthly Active Users |
| Wallet | Ledger account VND của user trong VietPay |
| Idempotency Key | UUID unique per transaction, prevent double-spend on retry |
| VietQR | Vietnam QR payment standard, managed by Napas |
| Sepay | Payment aggregator integration platform |
| eKYC | Electronic Know Your Customer (CMND/CCCD scan + liveness) |
| CMND | Chứng Minh Nhân Dân (old ID card format) |
| CCCD | Căn Cước Công Dân (new ID card format with chip) |
| SBV | State Bank of Vietnam (Ngân hàng Nhà nước) |
| Napas | National Payment Services VN (banking switch) |

---

## Open Questions

- Có cần support Apple Pay / Google Pay làm input ở MVP không? (Currently P1)
- Multi-language: Chinese / Korean cho expat segment? (Currently P3)
- Daily transfer limit có thể tăng cho premium tier? (Phase 2 decision)
