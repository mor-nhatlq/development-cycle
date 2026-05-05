# Wireframe — Onboarding Flow

**Screens:** 7 · **Total time target:** < 3 min · **Drop-off target:** < 30% end-to-end
**Figma:** `vietpay-mvp / Onboarding`

---

## Flow Overview

```mermaid
flowchart LR
    A[01 Welcome] --> B[02 Phone]
    B --> C[03 OTP]
    C --> D[04 eKYC Intro]
    D --> E[05 ID Scan]
    E --> F[06 Selfie]
    F --> G[07 Set PIN]
    G --> H[08 Biometric<br/>optional]
    H --> I[Home Dashboard]
```

---

## Screen 01 — Welcome

```
┌────────────────────────────────────────┐
│                                        │
│                                        │
│           ┌─────────────┐              │
│           │   [LOGO]    │              │
│           │   VietPay   │              │
│           └─────────────┘              │
│                                        │
│         Ví điện tử thông minh          │
│         cho thế hệ trẻ Việt Nam        │
│                                        │
│                                        │
│         ┌──────────────────┐           │
│         │   ●  ○  ○        │ pagination│
│         └──────────────────┘           │
│                                        │
│   • Chuyển tiền P2P miễn phí           │
│   • Quét VietQR thanh toán mọi nơi     │
│   • AI phân loại chi tiêu tự động      │
│                                        │
│                                        │
│  ┌──────────────────────────────────┐  │
│  │        Bắt đầu sử dụng           │  │
│  └──────────────────────────────────┘  │
│                                        │
│         Đã có tài khoản? Đăng nhập     │
│                                        │
└────────────────────────────────────────┘
```

**Behavior:** swipe left/right để qua 3 onboarding slides. Auto-advance 5s.

---

## Screen 02 — Phone Number Entry

```
┌────────────────────────────────────────┐
│  ←  Đăng ký                            │
├────────────────────────────────────────┤
│                                        │
│   ●  ○  ○  ○  ○  ○  ○      step 1/7   │
│                                        │
│   Số điện thoại của bạn                │
│                                        │
│   Chúng tôi sẽ gửi mã OTP để xác       │
│   thực tài khoản                       │
│                                        │
│   ┌──────────────────────────────────┐ │
│   │ 🇻🇳 +84  │ 901 234 567          │ │
│   └──────────────────────────────────┘ │
│                                        │
│                                        │
│                                        │
│   ☐  Tôi đồng ý với Điều khoản &       │
│      Chính sách Bảo mật                │
│                                        │
│                                        │
│  ┌──────────────────────────────────┐  │
│  │           Gửi mã OTP             │  │
│  └──────────────────────────────────┘  │
│                                        │
└────────────────────────────────────────┘
```

**Validation:** chỉ accept VN format `+84 9xx xxx xxx`. Disable button đến khi checkbox + valid phone.

---

## Screen 03 — OTP Verification

```
┌────────────────────────────────────────┐
│  ←  Đăng ký                            │
├────────────────────────────────────────┤
│                                        │
│   ●  ●  ○  ○  ○  ○  ○      step 2/7   │
│                                        │
│   Nhập mã OTP                          │
│                                        │
│   Mã đã gửi tới  +84 901 234 567       │
│   Đổi số                               │
│                                        │
│   ┌────┬────┬────┬────┬────┬────┐      │
│   │ 1  │ 2  │ 3  │ _  │    │    │ OTP  │
│   └────┴────┴────┴────┴────┴────┘      │
│                                        │
│   Mã hết hạn trong 04:32               │
│                                        │
│                                        │
│         Chưa nhận được? Gửi lại        │
│         (sau 30s)                      │
│                                        │
│                                        │
│  ┌──────────────────────────────────┐  │
│  │            Xác nhận              │  │
│  └──────────────────────────────────┘  │
│                                        │
└────────────────────────────────────────┘
```

**Behavior:** auto-submit khi đủ 6 số. Auto-fill từ SMS (iOS / Android API).

---

## Screen 04 — eKYC Intro

```
┌────────────────────────────────────────┐
│  ←  Xác thực danh tính                 │
├────────────────────────────────────────┤
│                                        │
│   ●  ●  ●  ○  ○  ○  ○      step 3/7   │
│                                        │
│         ┌──────────────┐               │
│         │    [icon]    │               │
│         │   ID + Face  │               │
│         └──────────────┘               │
│                                        │
│   Xác thực danh tính (eKYC)            │
│                                        │
│   Theo quy định Ngân hàng Nhà nước,    │
│   bạn cần xác thực CMND/CCCD để sử     │
│   dụng dịch vụ ví điện tử.             │
│                                        │
│   Bạn sẽ cần:                          │
│   ✓  CMND hoặc CCCD (mặt trước + sau)  │
│   ✓  Camera để chụp ảnh selfie         │
│   ✓  Khoảng 2 phút                     │
│                                        │
│   🔒 Thông tin được mã hóa AES-256     │
│      lưu trữ an toàn theo SBV          │
│                                        │
│  ┌──────────────────────────────────┐  │
│  │           Bắt đầu xác thực       │  │
│  └──────────────────────────────────┘  │
│                                        │
└────────────────────────────────────────┘
```

---

## Screen 05 — ID Card Scan

```
┌────────────────────────────────────────┐
│  ←  Quét CMND/CCCD                     │
├────────────────────────────────────────┤
│                                        │
│   ●  ●  ●  ●  ○  ○  ○      step 4/7   │
│                                        │
│   Mặt trước CMND/CCCD                  │
│                                        │
│  ┌──────────────────────────────────┐  │
│  │                                  │  │
│  │   ╔══════════════════════════╗   │  │
│  │   ║                          ║   │  │
│  │   ║   [Camera viewfinder]    ║   │  │
│  │   ║                          ║   │  │
│  │   ║   Đặt CMND vào khung     ║   │  │
│  │   ║                          ║   │  │
│  │   ╚══════════════════════════╝   │  │
│  │                                  │  │
│  └──────────────────────────────────┘  │
│                                        │
│   • Đặt thẳng, đủ ánh sáng             │
│   • Không lóa, không che mặt           │
│   • Auto chụp khi căn đúng khung       │
│                                        │
│         🔦 Bật đèn   📷 Chụp thủ công   │
│                                        │
└────────────────────────────────────────┘
```

**Behavior:** auto-detect edges, auto-shoot khi confidence > 85%. Manual override có sẵn.

---

## Screen 06 — Selfie + Liveness

```
┌────────────────────────────────────────┐
│  ←  Chụp Selfie                        │
├────────────────────────────────────────┤
│                                        │
│   ●  ●  ●  ●  ●  ○  ○      step 5/7   │
│                                        │
│   Xác thực khuôn mặt                   │
│                                        │
│  ┌──────────────────────────────────┐  │
│  │                                  │  │
│  │      ╭──────────────╮            │  │
│  │      │              │            │  │
│  │      │  [Front cam] │            │  │
│  │      │   Oval guide │            │  │
│  │      │              │            │  │
│  │      ╰──────────────╯            │  │
│  │                                  │  │
│  └──────────────────────────────────┘  │
│                                        │
│   ➜ Vui lòng chớp mắt 2 lần            │
│                                        │
│   Progress: ▰▰▰▰▱▱▱▱  50%              │
│                                        │
│   • Giữ điện thoại thẳng               │
│   • Ánh sáng đầy đủ                    │
│   • Không đeo kính / khẩu trang        │
│                                        │
└────────────────────────────────────────┘
```

**Behavior:** liveness check qua 3 hành động random (chớp mắt / quay đầu / mỉm cười). VNPT API verify.

---

## Screen 07 — Set PIN

```
┌────────────────────────────────────────┐
│  ←  Tạo mã PIN                         │
├────────────────────────────────────────┤
│                                        │
│   ●  ●  ●  ●  ●  ●  ○      step 6/7   │
│                                        │
│   Tạo mã PIN 6 số                      │
│                                        │
│   PIN sẽ dùng để xác nhận mọi giao     │
│   dịch trong VietPay                   │
│                                        │
│           ●  ●  ●  ●  ●  ○             │
│                                        │
│       ┌─────┬─────┬─────┐              │
│       │  1  │  2  │  3  │              │
│       ├─────┼─────┼─────┤              │
│       │  4  │  5  │  6  │              │
│       ├─────┼─────┼─────┤              │
│       │  7  │  8  │  9  │              │
│       ├─────┼─────┼─────┤              │
│       │     │  0  │  ⌫  │              │
│       └─────┴─────┴─────┘              │
│                                        │
│   ⚠ Không dùng dãy số liên tục         │
│      hoặc lặp lại                      │
│                                        │
└────────────────────────────────────────┘
```

**Validation:** reject `123456`, `111111`, `654321`, etc. Confirm screen sau khi nhập lần 1.

---

## Screen 08 — Biometric (Optional)

```
┌────────────────────────────────────────┐
│     Bỏ qua  →                          │
├────────────────────────────────────────┤
│                                        │
│   ●  ●  ●  ●  ●  ●  ●      step 7/7   │
│                                        │
│         ┌──────────────┐               │
│         │   [Face ID]  │               │
│         │      hoặc    │               │
│         │  [Fingerprint]│              │
│         └──────────────┘               │
│                                        │
│   Bật xác thực sinh trắc học?          │
│                                        │
│   Đăng nhập nhanh hơn và bảo mật       │
│   hơn với Face ID / vân tay            │
│                                        │
│   ✓  Mở app không cần PIN              │
│   ✓  Xác nhận giao dịch nhanh          │
│   ✓  Bảo mật dấu vân tay không lưu     │
│      trên server VietPay               │
│                                        │
│  ┌──────────────────────────────────┐  │
│  │       Bật Face ID                │  │
│  └──────────────────────────────────┘  │
│                                        │
│         Để sau (Cài đặt > Bảo mật)     │
│                                        │
└────────────────────────────────────────┘
```

**Behavior:** detect device capability. Fallback Touch ID / Fingerprint nếu không có Face ID.

---

## Drop-off Analytics Plan

| Step | Drop-off threshold (alert if exceed) |
|------|---------------------------------------|
| 01 → 02 | 10% (intent confirmation) |
| 02 → 03 | 15% (OTP friction) |
| 03 → 04 | 5% |
| 04 → 05 | 20% (KYC intimidation, biggest drop expected) |
| 05 → 06 | 15% (camera issues) |
| 06 → 07 | 10% (selfie retry) |
| 07 → 08 | 5% |

**Total target end-to-end completion:** ≥ 50%
