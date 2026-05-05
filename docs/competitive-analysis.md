# VietPay — Competitive Analysis

**Date:** 2026-04-29 · **Author:** Docs Manager Agent · **Status:** Locked for MVP scoping

---

## 1. Market Overview

Thị trường ví điện tử VN 2025: ~75M downloads, ~50M MAU active wallets. SBV cấp ~50 e-wallet licences. Top 5 chiếm 90% market share.

| Year | Total e-wallet MAU | Top player share |
|------|---------------------|-------------------|
| 2023 | 32M | MoMo 60% |
| 2024 | 41M | MoMo 58% |
| 2025 | 50M | MoMo 55% |
| 2026 (est) | 58M | MoMo 52% |

Trend: market còn growth nhưng tốc độ chậm lại. Gen Z (18-26) là segment mở nhất với new app.

## 2. Feature Matrix

| Feature | MoMo | ZaloPay | ShopeePay | ViettelPay | VNPay | **VietPay (MVP)** |
|---------|:----:|:-------:|:---------:|:----------:|:-----:|:----------:|
| **Core wallet** | | | | | | |
| Sign-up + eKYC | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Link bank account | ✅ (40+) | ✅ (35+) | ✅ (30+) | ✅ (20+) | ✅ (45+) | ✅ (10 top) |
| Wallet balance + topup | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **P2P Transfer** | | | | | | |
| Phone-based transfer | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |
| QR-based transfer | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Bank account transfer | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Group split bills** | partial | ❌ | ❌ | ❌ | ❌ | ✅ |
| **VietQR Payment** | | | | | | |
| Generate QR (receive) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Scan QR (pay) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Static + dynamic QR | ✅ | ✅ | ✅ | partial | ✅ | ✅ |
| **Bills & Topup** | | | | | | |
| Mobile prepaid topup | ✅ | ✅ | ✅ | ✅ (best) | ✅ | ❌ (P1) |
| Electricity / Water bills | ✅ | ✅ | partial | ✅ | ✅ | ❌ (P1) |
| Internet / Telecom | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ (P1) |
| **Smart Features** | | | | | | |
| **AI auto-categorize** | partial | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Goal-based savings** | partial | ❌ | ❌ | ❌ | ❌ | ✅ (P1) |
| Spending insights | basic | ❌ | basic | ❌ | basic | ✅ |
| Recurring payments | ✅ | partial | ❌ | partial | ✅ | ❌ (P2) |
| **Commerce** | | | | | | |
| In-app shopping | ✅ | ✅ | ✅ (best) | ❌ | ❌ | ❌ |
| Mini-games / lottery | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Livestream commerce | ✅ | partial | ❌ | ❌ | ❌ | ❌ |
| **Investment** | | | | | | |
| Mutual funds / stocks | ✅ | partial | ❌ | ❌ | ❌ | ❌ (P3) |
| Gold | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Crypto | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ (P3) |
| **UX Quality** | | | | | | |
| Onboarding < 3 min | ❌ | partial | partial | ❌ | partial | ✅ |
| Single-screen actions | ❌ | ❌ | partial | ❌ | partial | ✅ |
| Ad-free experience | ❌ | ❌ | ❌ | ❌ | partial | ✅ |
| Dark mode | partial | ❌ | partial | ❌ | partial | ✅ |
| **Trust & Security** | | | | | | |
| Biometric auth | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2FA / TOTP | partial | ❌ | ❌ | partial | ✅ | ✅ |
| Session control | basic | basic | basic | basic | ✅ | ✅ |

## 3. Strengths & Weaknesses

### MoMo (incumbent leader)
- ✅ Ecosystem rộng, merchant accept everywhere
- ✅ Brand trust, đã pass nhiều SBV audit
- ❌ App nặng (180MB), startup time chậm
- ❌ Banner ads + popup spam
- ❌ Không focus — game / shopping / utility / wallet trộn lẫn

### ZaloPay
- ✅ Tích hợp Zalo chat → social distribution
- ✅ User base sẵn từ Zalo (75M users)
- ❌ Không khác biệt rõ ngoài Zalo integration
- ❌ Feature thừa, scope creep

### ShopeePay
- ✅ Captive trong Shopee ecosystem
- ❌ Yếu khi outside Shopee — không có lý do dùng cho P2P
- ❌ Không có B2B / merchant tools độc lập

### ViettelPay
- ✅ Network của Viettel (telecom user → cross-sell)
- ❌ UX rất cũ, demographic hơi già
- ❌ Mạnh top-up nhưng không phải killer feature cho Gen Z

### VNPay
- ✅ Bank network sâu (45+ banks)
- ✅ Trust cao (B2B background)
- ❌ B2C brand yếu, Gen Z không nhận diện

## 4. Positioning Map

```
                 SOCIAL / FUN
                      ▲
                      │
       ZaloPay  ●     │     ● MoMo
                      │
   ──────────────────────────────────►  COMMERCE FOCUS
   PERSONAL FINANCE                      
                      │     ● ShopeePay
              VietPay │
                  ●   │
                      │
       VNPay   ●      │     ● ViettelPay
                      │
                 UTILITY / OLD
```

VietPay positioning: **clean personal finance + light social** — góc trống không ai chiếm.

## 5. Pricing Comparison

| Wallet | P2P transfer fee | Bank withdraw fee | Top-up fee | Premium tier |
|--------|------------------|--------------------|-----------|--------------|
| MoMo | Free | 11k VND | Free | None |
| ZaloPay | Free | 11k VND | Free | None |
| ShopeePay | Free | 11k VND | Free | None |
| ViettelPay | Free | 5k VND | Free | None |
| VNPay | Free | Variable | Free | None |
| **VietPay** | **Free** | **5k VND first 3/month, 11k after** | **Free** | **Premium 49k/month — instant withdraw, advanced insights, multi-goal** |

## 6. Conclusion — Where to Compete

**Compete ON:**
- Clean UX, ad-free, fast app
- Social P2P (split bills viral loop)
- AI insights / goal savings (Gen Z financial literacy)
- Niche but deep: P2P + QR + history excellently done

**Don't compete ON:**
- Ecosystem depth (MoMo wins, ngàn merchant)
- Mini-games / commerce (ShopeePay/MoMo wins)
- Bank network breadth (VNPay wins)

**Win condition:** Top 1 wallet trong segment Gen Z urban (18-30) trong 18 tháng. Target 500k MAU end of Y1.

---

## Open Questions

- Premium tier 49k/tháng có quá thấp / cao so với MoMo Plus (35k)? → cần survey
- Có outsource phần "AI categorization" cho Plaid-style API không hay tự build? → research separately
