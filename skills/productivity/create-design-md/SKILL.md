---
name: create-design-md
description: '브랜드·제품의 디자인 시스템을 정의하는 DESIGN.md를 생성. 트리거 조건: "DESIGN.md 만들어", "디자인 시스템 정의", "디자인 토큰", "컬러 팔레트 잡아줘", "타이포그래피 정의", "brand guidelines", "design tokens", "make a design system". NOT for: 기존 CSS·Tailwind 파일 직접 수정, 단순 색상 추천 (DESIGN.md 없이), README 작성, 코드 컴포넌트 리팩터링.'
---

# create-design-md

Generate a valid, well-structured DESIGN.md file that defines a product's design system.

## What is DESIGN.md?

A self-contained, plain-text design system document combining:
- **YAML frontmatter** — machine-readable design tokens (colors, typography, spacing, rounded corners, components)
- **Markdown body** — human-readable design rationale and usage guidance

See `references/spec.md` for the full format specification.

## Checklist

1. **Detect input language** — use the same language for all clarifying questions. Keep this guidance in English.
2. **Check specificity** — count how many of these 6 dimensions are known or strongly implied:

   | Dimension | Specific signal example |
   |---|---|
   | Brand name / product | "Kira", "Lumio Finance", "my portfolio" |
   | Personality / tone | professional, playful, minimal, bold, luxe |
   | Color direction | warm/cool, dark/light, vibrant, muted, monochromatic |
   | Typography style | geometric sans, editorial serif, technical mono |
   | Product type | SaaS dashboard, marketing site, e-commerce, mobile app |
   | Target audience | enterprise teams, Gen-Z, medical professionals |

3. **Apply the threshold rule:**
   - **Generate directly** if: ≥3 dimensions are known, **OR** the user names a reference brand (e.g., "like Linear", "Stripe 스타일")
   - **Ask first (mandatory)** if: <3 dimensions known and no reference brand. Use `AskUserQuestion` with the priority questions below; ask at most 3 at a time.

4. **Priority questions** (ask these first if missing):
   1. What is the product name and what does it do?
   2. What personality should the design convey? (offer 4 options: professional / minimal / bold-expressive / warm-friendly)
   3. What's the color direction? (offer: dark, light, warm-neutral, cool-neutral)

5. **Secondary questions** (only if the first round still leaves meaningful gaps):
   - Product type (web app, marketing site, mobile app, etc.)
   - Target audience
   - Any reference brands they admire?

6. **Generate DESIGN.md** — write to `DESIGN.md` in the current directory (unless user specifies another path). Section order:
   1. `## Overview` — brand personality, audience, emotional tone
   2. `## Colors` — palette description + token map
   3. `## Typography` — font levels (aim for 9–12 levels)
   4. `## Layout` — grid/spacing strategy
   5. `## Elevation & Depth` — shadow or tonal approach
   6. `## Shapes` — corner radius language
   7. `## Components` — button, input, card token definitions
   8. `## Do's and Don'ts` — 4–6 practical guardrails

7. **After writing**, briefly explain:
   - The 2–3 core design decisions made and why
   - Any tokens the user might want to customize first

## Self-check

Run these before writing the file:

- [ ] All colors are valid hex (`#RRGGBB`)
- [ ] All typography tokens include fontFamily, fontSize, fontWeight, lineHeight
- [ ] Spacing and rounded tokens use valid units (px, em, rem) or unitless numbers
- [ ] Component tokens use `{path.to.token}` references where appropriate
- [ ] All 8 sections present (or explicitly noted as omitted)
- [ ] Do's and Don'ts has ≥4 entries
- [ ] Token names follow recommended naming conventions
- [ ] Minimum components defined: `button-primary`, `button-primary-hover`, `button-secondary`, `input`, `card`

## Anti-patterns

| ❌ Avoid | ✅ Instead |
|---|---|
| Guessing when brand direction is unclear | Ask with AskUserQuestion (≤3 questions per round) |
| Using color names instead of hex | All colors as `#RRGGBB` |
| Generic token names (`color1`, `font1`) | Semantic names: `primary`, `body-md`, `rounded-lg` |
| Flat single `{colors.primary}` without shade variants | Include `primary-dark`/`primary-light` for hover states |
| Missing component hover/active variants | Always define `button-primary-hover` alongside `button-primary` |
| Empty `## Elevation & Depth` for flat designs | Explain the *alternative* hierarchy method (tonal layers, borders) |
| Asking all questions at once | 3 questions max per round |

## Token Conventions

**Colors:** `primary`, `secondary`, `tertiary`, `neutral`, `surface`, `on-surface`, `error`

**Typography:** `headline-display`, `headline-lg`, `headline-md`, `body-lg`, `body-md`, `body-sm`, `label-lg`, `label-md`, `label-sm`

**Rounded:** `none`, `sm`, `md`, `lg`, `xl`, `full`

**Spacing:** `xs`, `sm`, `md`, `lg`, `xl`, `base`, `gutter`, `margin`

## Reference

For the complete format specification, read `references/spec.md`.
