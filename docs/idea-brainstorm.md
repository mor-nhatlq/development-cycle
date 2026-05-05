# VietPay — Idea Brainstorm

**Session:** 2026-04-28 · **Facilitator:** Brainstorm Agent · **Participants:** Product, Eng Lead, Design, Compliance
**Output:** Product positioning + scope MVP cho thị trường ví điện tử Việt Nam

---

## 1. Vision

> Ví điện tử cho Gen Z & Millennials Việt Nam — clean UX, social-first, AI insights — không phải Yet Another Bank App.

## 2. Target Users

### Primary persona — "Linh"
- 24 tuổi, marketing executive ở Saigon
- Income: 18M VND/tháng
- Pain: split bills với bạn bè khó (mỗi đứa 1 bank), app ngân hàng UX phức tạp, không biết tiền chạy đi đâu
- Behavior: mobile-native, dùng MoMo cho QR pay nhưng ghét UI lộn xộn
- Goals: chuyển tiền cho bạn bè nhanh, biết được mình tiêu vào gì, save cho 1 chuyến du lịch

### Secondary persona — "Tuấn"
- 28 tuổi, freelance designer
- Income: 25-40M VND/tháng (variable)
- Pain: client trả qua nhiều bank khác nhau, khó tracking thu nhập, cần invoice + QR payment
- Goals: nhận tiền dễ qua QR, tự động categorize income, export báo cáo

## 3. Pain Points

| Pain | Severity | Current Solution |
|------|----------|------------------|
| Split bills với bạn (Grab, ăn uống) | High | Mỗi đứa tự chuyển → mất thời gian, dễ quên |
| App bank UX phức tạp, nhiều bước | High | MoMo (đỡ hơn nhưng vẫn rối) |
| Không biết tiền tiêu vào đâu | Medium | Excel tự ghi (lười) |
| QR pay mỗi merchant 1 app | High | Dùng MoMo + ZaloPay song song |
| Save tiền cho mục tiêu cụ thể | Medium | Bank savings (không UX/no goal tracking) |
| Nhận tiền từ client → tracking | Medium | Tự nhập Excel |

## 4. Competitive Landscape

| Competitor | Strength | Weakness | MAU (est.) |
|-----------|----------|----------|------------|
| **MoMo** | Market leader, ecosystem rộng | UI lộn xộn, ads nhiều | 31M |
| **ZaloPay** | Tích hợp Zalo (chat) | Feature thừa, không focus | 12M |
| **ShopeePay** | Tích hợp Shopee | Chỉ mạnh trong commerce | 8M |
| **ViettelPay** | Mạng lưới Viettel | UX cũ, thiên về top-up | 6M |
| **VNPay Wallet** | Bank network | Brand yếu Gen Z | 4M |

## 5. Differentiation — VietPay USP

### Core differentiators
1. **Social-first P2P** — split bills tự động qua group chat, "tag bạn" trong transaction note
2. **AI auto-categorize** — phân loại transaction theo merchant + ML (ăn uống / di chuyển / mua sắm / hóa đơn)
3. **Goal-based savings** — "Save 5M cho Đà Lạt" với progress bar + auto-allocation
4. **Clean UX** — 1 màn hình, 1 hành động chính. Không banner ads.

### Anti-features (intentionally NOT building)
- ❌ Mini-games / lottery / livestream (MoMo có)
- ❌ E-commerce marketplace (ShopeePay có)
- ❌ Phone top-up làm trung tâm (ViettelPay có)
- ❌ Social feed / news (ZaloPay có)

## 6. Tech Stack Decision

### Mobile
- **React Native + Expo** — single codebase iOS+Android, 80% team familiar
- Alternative considered: Flutter (rejected — team learning curve)

### Backend
- **NestJS + TypeScript** — match mobile language, strong DI, modular
- Alternative: Go (rejected — slower dev, less talent pool tại VN)

### Payment Integration
- **Sepay** primary (VietQR + bank transfer aggregator)
- Fallback: VNPay direct integration cho top 5 banks

### eKYC
- **VNPT eKYC** chosen over FPT (research thêm trong Step 1)
- Reason: 0.05 USD/lần (vs 0.08), 97.2% accuracy CMND/CCCD scan

### Infra
- AWS (ap-southeast-1 Singapore + eu-central-1 Frankfurt cho EU users)
- ECS Fargate (no k8s overhead)
- RDS PostgreSQL 16 Multi-AZ
- ElastiCache Redis
- S3 + CloudFront cho assets

## 7. Compliance Constraints

- **SBV Circular 23/2019 + 17/2024** — KYC bắt buộc, lưu hồ sơ 5 năm
- **AML reporting** — auto-flag giao dịch ≥ 400M VND đơn lẻ hoặc tổng ≥ 600M/ngày
- **PCI-DSS Level 1** — không lưu PAN, dùng tokenization của Sepay
- **Personal Data Protection Law (NĐ 13/2023)** — encryption at rest AES-256, audit log truy cập PII

## 8. Success Metrics (MVP target — 6 tháng sau launch)

| Metric | Target | Measure |
|--------|--------|---------|
| MAU | 50,000 | Mixpanel |
| D7 retention | ≥ 35% | Mixpanel |
| D30 retention | ≥ 18% | Mixpanel |
| Avg transactions / MAU / month | ≥ 8 | DB query |
| NPS | ≥ 40 | In-app survey |
| App Store rating | ≥ 4.5 | iTunes Connect |
| Crash-free rate | ≥ 99.5% | Sentry |
| API p95 latency | < 200ms | DataDog |

## 9. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| SBV không cấp licence kịp | Medium | Critical | Apply ngay tuần 1, parallel với dev |
| Sepay outage trong launch week | Low | High | Fallback bank API trực tiếp top 3 banks |
| eKYC accuracy < 95% | Medium | High | Manual review queue cho rejection, fall back FPT |
| Race condition trong wallet (double-spend) | Medium | Critical | SELECT FOR UPDATE + chaos testing |
| Marketing CAC > 200k VND | Medium | Medium | Referral program + social split (viral loop) |

## 10. Decisions Locked

- ✅ MVP scope: P2P transfer + VietQR + transaction history (3 tháng dev)
- ✅ Mobile-only ở MVP (web app phase 2)
- ✅ Vietnam-only ở MVP (multi-region phase 3)
- ✅ VND-only ở MVP (multi-currency phase 3)
- ✅ Sepay primary, VNPT eKYC
- ✅ Brand color: VietPay green `#00B074`

## 11. Open Questions

- Có cần in-app chat để support split bills không, hay link qua Zalo/Messenger?
- Pricing: free for users — revenue model là gì? (interchange fee từ merchant QR? subscription premium tier?)
- Gọi vốn series A bao giờ — sau MVP hay sau 50k MAU?

---

**Next step:** Docs Manager Agent → draft SRS, wireframe các flow chính từ pain points trên.
