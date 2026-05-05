# QA Acceptance Test · VietPay MVP

**Date:** 2026-06-05 · **QA Lead:** Tuấn P.

## Acceptance Criteria

All user stories from PRD validated through scripted test cases.

## Results

| Story | Test Cases | Pass | Fail | Skip |
|---|---|---|---|---|
| US-01 Onboarding + eKYC | 12 | 12 | 0 | 0 |
| US-02 Bank account linking | 8 | 8 | 0 | 0 |
| US-03 P2P transfer (phone/QR) | 18 | 18 | 0 | 0 |
| US-04 VietQR scan + pay | 10 | 10 | 0 | 0 |
| US-05 Transaction history | 6 | 6 | 0 | 0 |
| US-06 Push notifications | 7 | 6 | 0 | 1 |
| US-07 Profile + settings | 9 | 9 | 0 | 0 |

**Total:** 70 test cases · 69 pass · 0 fail · 1 skip (Apple Watch — out of MVP scope)

## Cross-Device Testing

- iPhone 12, 13, 14, 15 (iOS 16, 17) ✓
- Pixel 6, 7 · Samsung S22, S23 (Android 12, 13, 14) ✓
- Tablet support: out of MVP scope

## Localization

- Vietnamese (default) ✓
- English ✓
- Numbers/currency formatted theo local convention ✓

## Accessibility

- VoiceOver/TalkBack: 92% screens pass
- Color contrast WCAG AA: ✓
- Min touch target 44pt: ✓

## Decision

**ACCEPTED** · 1 skipped test out of scope, 0 failures.
