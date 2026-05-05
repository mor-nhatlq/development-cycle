# VietPay Design System

Design tokens for VietPay mobile + web. Source of truth for color, typography, spacing, radius, elevation.

## Brand Colors

```
--brand-primary:   #00B074   /* VietPay green */
--brand-secondary: #0EA5E9   /* sky blue */
--success:         #16A34A
--warning:         #F59E0B
--danger:          #DC2626
--info:            #2563EB
```

## Neutrals

```
--bg-base:   #FFFFFF / #0A0E1A (dark)
--bg-card:   #F8FAFC / #0F1629 (dark)
--text-pri:  #0F172A / #E2E8F0 (dark)
--text-sec:  #64748B / #94A3B8 (dark)
--border:    #E2E8F0 / rgba(56,189,248,0.12) (dark)
```

## Typography

```
--font-display: "SF Pro Display", system-ui  (headings, display)
--font-body:    "Inter", system-ui            (body text, UI)
--font-mono:    "JetBrains Mono", monospace   (code, numerals)
```

Type scale (modular 1.25):
- xs `12px` · sm `14px` · base `16px` · lg `20px` · xl `25px` · 2xl `32px` · 3xl `40px`

## Spacing (4px grid)

`4 · 8 · 12 · 16 · 24 · 32 · 48 · 64`

## Radius

`sm 4px` · `md 8px` · `lg 12px` · `xl 16px` · `2xl 24px` · `full 9999px`

## Elevation

```
--shadow-sm: 0 1px 2px rgba(0,0,0,0.05)
--shadow-md: 0 4px 6px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.06)
--shadow-lg: 0 10px 15px rgba(0,0,0,0.10), 0 4px 6px rgba(0,0,0,0.05)
--shadow-xl: 0 20px 25px rgba(0,0,0,0.12), 0 10px 10px rgba(0,0,0,0.04)
```

## Component Anchors

- Button: 44px touch target, radius `lg`, primary fill `--brand-primary`
- Input: 48px height, radius `md`, focus ring `--brand-secondary`
- Card: radius `xl`, padding `16-24px`, shadow `md`
- Modal: max-width `560px` mobile / `720px` web, radius `2xl`

## Motion

- Duration: `fast 120ms` · `base 200ms` · `slow 320ms`
- Easing: `cubic-bezier(0.4, 0, 0.2, 1)` (standard)
