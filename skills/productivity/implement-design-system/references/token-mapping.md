# DESIGN.md → Tailwind + CSS Variables 변환표

DESIGN.md frontmatter의 각 키를 Tailwind config와 globals.css에 매핑하는 규칙.

## colors

### CSS Variables (globals.css)

shadcn의 표준 CSS 변수 이름과 DESIGN.md 토큰의 권장 매핑:

| CSS 변수 | DESIGN.md 토큰 | 대체 후보 |
|---|---|---|
| `--background` | `colors.surface` | `colors.neutral` |
| `--foreground` | `colors.on-surface` | `colors.neutral-foreground` |
| `--primary` | `colors.primary` | — |
| `--primary-foreground` | `colors.on-primary` | 자동 대비(흰/검) |
| `--secondary` | `colors.secondary` | — |
| `--secondary-foreground` | `colors.on-secondary` | 자동 대비 |
| `--muted` | `colors.neutral` 연한 변형 | `colors.surface-variant` |
| `--muted-foreground` | `colors.secondary` | — |
| `--accent` | `colors.tertiary` | `colors.secondary` |
| `--accent-foreground` | `colors.on-tertiary` | 자동 대비 |
| `--destructive` | `colors.error` | `colors.danger` |
| `--destructive-foreground` | `colors.on-error` | 자동 대비 |
| `--border` | `colors.neutral` 어두운 변형 | `colors.outline` |
| `--input` | `--border`와 동일 | — |
| `--ring` | `colors.primary` | — |
| `--radius` | `rounded.md` | `rounded.lg` |

**hex → CSS 변수 형식 변환:**
DESIGN.md는 `#RRGGBB` hex를 사용한다. shadcn의 CSS 변수는 `<H> <S>% <L>%` (HSL 공간 값만) 형식이 일반적이다.

```css
/* DESIGN.md: colors.primary: "#1A73E8" */
/* 변환 결과: */
:root {
  --primary: 214 89% 52%;  /* hex #1A73E8 → HSL 214° 89% 52% */
}
```

자동 변환이 불가한 경우 hex를 그대로 사용해도 무방:
```css
:root {
  --primary: #1A73E8;
}
/* tailwind.config에서: colors: { primary: "var(--primary)" } */
```

**다크 모드**: DESIGN.md에 `colors.primary-dark`, `colors.surface-dark` 등 dark 계열 토큰이 있거나 `darkMode` 섹션이 있을 때만 `.dark { ... }` 블록 생성.

### tailwind.config — theme.extend.colors

```js
// 패턴: CSS 변수 참조
theme: {
  extend: {
    colors: {
      background: "hsl(var(--background))",
      foreground: "hsl(var(--foreground))",
      primary: {
        DEFAULT: "hsl(var(--primary))",
        foreground: "hsl(var(--primary-foreground))",
      },
      secondary: {
        DEFAULT: "hsl(var(--secondary))",
        foreground: "hsl(var(--secondary-foreground))",
      },
      destructive: {
        DEFAULT: "hsl(var(--destructive))",
        foreground: "hsl(var(--destructive-foreground))",
      },
      muted: {
        DEFAULT: "hsl(var(--muted))",
        foreground: "hsl(var(--muted-foreground))",
      },
      accent: {
        DEFAULT: "hsl(var(--accent))",
        foreground: "hsl(var(--accent-foreground))",
      },
      border: "hsl(var(--border))",
      input: "hsl(var(--input))",
      ring: "hsl(var(--ring))",
    }
  }
}
```

DESIGN.md에 추가 색상 토큰(예: `colors.brand`, `colors.success`)이 있으면 shadcn 표준 변수 외에 그대로 추가:
```js
colors: {
  // ... shadcn 표준
  brand: "var(--brand)",   // DESIGN.md colors.brand → CSS var로
  success: "var(--success)",
}
```

## typography

### tailwind.config — theme.extend.fontFamily

`typography.*.fontFamily`에서 고유한 폰트 패밀리를 수집해 등록:

```js
// DESIGN.md typography 예시:
// body-md: { fontFamily: "Inter", fontSize: "16px", ... }
// headline-lg: { fontFamily: "Cal Sans", fontSize: "32px", ... }

fontFamily: {
  sans: ["Inter", "sans-serif"],         // 가장 많이 쓰인 fontFamily를 sans 기본으로
  display: ["Cal Sans", "sans-serif"],   // 별도 디스플레이 폰트
}
```

### tailwind.config — theme.extend.fontSize

`typography.*` 레벨별로 fontSize 튜플로 등록:

```js
// DESIGN.md:
// headline-lg: { fontSize: "32px", lineHeight: 1.2, letterSpacing: "-0.02em" }

fontSize: {
  "headline-lg": ["32px", { lineHeight: "1.2", letterSpacing: "-0.02em" }],
  "headline-md": ["24px", { lineHeight: "1.3", letterSpacing: "-0.01em" }],
  "body-lg":     ["18px", { lineHeight: "1.6" }],
  "body-md":     ["16px", { lineHeight: "1.6" }],
  "body-sm":     ["14px", { lineHeight: "1.5" }],
  "label-lg":    ["14px", { lineHeight: "1.2", fontWeight: "500" }],
  "label-md":    ["12px", { lineHeight: "1.2", fontWeight: "500" }],
  "label-sm":    ["11px", { lineHeight: "1", letterSpacing: "0.05em" }],
}
```

fontWeight는 tailwind fontSize 튜플에 넣을 수 없으므로 별도 `fontWeight` extend 또는 유틸리티 클래스로 처리.

## rounded

```js
// DESIGN.md:
// rounded: { none: "0px", sm: "4px", md: "8px", lg: "12px", xl: "16px", full: "9999px" }

borderRadius: {
  none: "0px",
  sm:   "4px",
  md:   "var(--radius)",   // --radius CSS 변수와 연동 (shadcn 기본)
  lg:   "12px",
  xl:   "16px",
  full: "9999px",
}
```

`rounded.md` 값을 `--radius` CSS 변수 기본값으로 설정.

## spacing

```js
// DESIGN.md:
// spacing: { xs: "4px", sm: "8px", md: "16px", lg: "24px", xl: "32px", gutter: "24px" }

spacing: {
  xs:     "4px",
  sm:     "8px",
  md:     "16px",
  lg:     "24px",
  xl:     "32px",
  gutter: "24px",
}
```

기존 Tailwind 숫자형 spacing을 덮어쓰지 않도록 `extend` 아래에만 추가.

## components

`components.*` 토큰은 Tailwind config에 직접 등록하지 않는다.
대신 컴포넌트 파일의 `cva` 정의 시 참고 자료로 사용:

```ts
// DESIGN.md components.button-primary.background: "{colors.primary}"
// → cva에서:
const buttonVariants = cva(
  "bg-primary text-primary-foreground ...",
  { variants: { ... } }
)
```

## 머지 전략

기존 `tailwind.config`가 있을 때 처리 규칙:

1. `theme.extend` 아래에만 추가 → 기존 기본 테마 건드리지 않음
2. 동일한 키가 이미 있으면 사용자에게 노출:
   ```
   충돌 발견: borderRadius.lg = "10px" (기존) vs "12px" (DESIGN.md)
   어떻게 할까요? (유지 / 덮어쓰기 / 새 이름으로 추가)
   ```
3. 사용자가 "건너뜀"을 선택하면 해당 키는 기존 값 유지.

## globals.css 삽입 위치

파일 구조에 따라 CSS 변수 블록 위치 결정:

- shadcn `@layer base` 블록이 있으면 그 안에 삽입
- 없으면 파일 최상단 `@tailwind base;` 다음에 삽입
- `@import` 문 아래, 실제 스타일 위에 배치

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    /* DESIGN.md 토큰에서 생성된 CSS 변수 */
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    /* ... */
    --radius: 0.5rem;
  }

  .dark {
    /* 다크 팔레트가 DESIGN.md에 있을 때만 */
  }
}
```
