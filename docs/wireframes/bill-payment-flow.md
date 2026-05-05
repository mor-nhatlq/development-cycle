# Wireframe · Bill Payment Flow

## User Story

User pay EVN electricity bill từ wallet, nhận confirm trong < 5s.

## Screens

### 1. Bill Category Picker
```
┌────────────────────────────┐
│  ← Pay Bills               │
├────────────────────────────┤
│  ⚡ Electricity (EVN)       │
│  📶 Internet                │
│  💧 Water                   │
│  📺 TV / Cable              │
│  📱 Mobile Topup            │
└────────────────────────────┘
```

### 2. Provider Lookup
```
┌────────────────────────────┐
│  ← EVN Electricity          │
├────────────────────────────┤
│  Customer Code              │
│  ┌──────────────────────┐   │
│  │ PE0123456789         │   │
│  └──────────────────────┘   │
│                             │
│  [   Look up bill   ]       │
└────────────────────────────┘
```

### 3. Bill Summary
```
┌────────────────────────────┐
│  Bill Detail                │
├────────────────────────────┤
│  Provider:  EVN HCMC        │
│  Period:    May 2026        │
│  Amount:    458,200 VND     │
│  Due:       2026-06-15      │
├────────────────────────────┤
│  Pay from: VietPay Wallet   │
│  Balance:  2,450,000 VND    │
│                             │
│  [    Confirm Pay   ]       │
└────────────────────────────┘
```

### 4. PIN / Biometric Confirm

Face ID / Fingerprint or 6-digit PIN.

### 5. Success
```
✓ Payment Successful
EVN HCMC · 458,200 VND
Reference: TXN-7H3K2P9X
[ View receipt ]  [ Done ]
```

## Edge Cases

- Bill not found → toast "Customer code không tồn tại trong hệ thống EVN"
- Insufficient balance → CTA "Top up wallet" (deeplink to topup flow)
- Sepay timeout > 8s → queue + push notification when settled
- Already paid → show receipt of previous payment
