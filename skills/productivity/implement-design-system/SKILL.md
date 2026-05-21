---
name: implement-design-system
description: DESIGN.md를 기반으로 Tailwind·shadcn·Radix UI 디자인 시스템을 실제 코드로 구현. 트리거 조건: "디자인 시스템 구현", "DESIGN.md 적용", "shadcn 셋업", "tailwind 토큰 적용", "implement design system", "디자인 시스템 코드로", "@DESIGN.md 기반으로". NOT for: DESIGN.md 자체 작성(→ create-design-md 스킬 사용), 컴포넌트 한두 개만 추가, 기존 디자인 시스템 마이그레이션.
---

# implement-design-system

DESIGN.md의 디자인 토큰을 읽어 Tailwind config, globals.css, shadcn 컴포넌트까지 일괄 생성한다.

## Checklist

### 1. DESIGN.md 존재 확인

- 인자로 경로를 받았으면 그 경로를 사용. 없으면 CWD에서 `DESIGN.md` 탐색.
- 파일이 없으면 **즉시 중단**. 입력 언어에 맞게 메시지 출력:
  - 한국어: "DESIGN.md 파일을 찾을 수 없습니다. 먼저 `/create-design-md` 스킬로 파일을 생성해 주세요."
  - 영어: "DESIGN.md not found. Please run `/create-design-md` first to generate the file."
- **중단 후 추가 작업 없음.** 스킬 종료.

### 2. DESIGN.md 파싱

- YAML frontmatter(`---` 블록)를 추출해 `colors`, `typography`, `rounded`, `spacing`, `components` 토큰을 수집한다.
- frontmatter 자체가 없으면 사용자에게 알리고 중단 (토큰 없이는 Tailwind 매핑 불가).
- 토큰 매핑 규칙은 `references/token-mapping.md`를 따른다.
- 파싱된 토큰 요약(색상 수, 타이포 레벨 수, 컴포넌트 토큰 수)을 한 줄로 표시.

### 3. 프로젝트 환경 감지

다음 항목을 순서대로 확인하고 결과를 사용자에게 한 줄로 요약한다.

**프레임워크 감지** (`package.json`의 `dependencies`/`devDependencies`):
- `next` 키 존재 → Next.js (App Router: `app/`, Pages: `pages/` 구분)
- `vite` 키 존재 → Vite + React
- `react-scripts` 키 존재 → CRA
- `@remix-run/react` 존재 → Remix
- 미감지 → 사용자에게 한 번 물어보고 진행

**패키지 매니저 감지** (lockfile 우선):
- `pnpm-lock.yaml` → pnpm
- `yarn.lock` → yarn
- `bun.lockb` → bun
- 없으면 → npm

**기존 파일 존재 여부 체크**:
- `tailwind.config.{js,ts}` → 있으면 머지 모드
- `app/globals.css` 또는 `src/index.css` → CSS 변수 삽입 위치 결정
- `components.json` → 있으면 shadcn 이미 초기화됨 (init 건너뜀)

### 4. 기술 스택 확인 — `AskUserQuestion`

기본 스택:
```
tailwindcss  shadcn/ui  @radix-ui/*  lucide-react
class-variance-authority  sonner  overlay-kit
```

이미 `package.json`에 있는 항목은 "(이미 설치됨)"으로 표시. 세 가지 옵션으로 확인:
- **(A) 기본 스택 그대로** — 위 7개 라이브러리를 기준으로 진행
- **(B) 현재 환경 우선** — 이미 설치된 것만 사용, 나머지 건너뜀
- **(C) 직접 지정** — 사용자에게 빼거나 추가할 항목 입력 요청

스택 결정 결과에 따라 이후 단계에서:
- `sonner` 제외 → `Toast` 컴포넌트 생성 건너뜀
- `overlay-kit` 제외 → Dialog OverlayProvider 와이어링 건너뜀

### 5. 컴포넌트 목록 확인 — `AskUserQuestion` (multiSelect, 카테고리 단위)

각 카테고리를 하나씩 `AskUserQuestion`(multiSelect)으로 확인한다. 기본값은 전체 선택 (Domain-specific 카테고리 제외).

**Form & Input**
`Button` `Input` `Textarea` `Label` `Select` `Checkbox` `RadioGroup` `Switch` `Combobox` `MultiCombobox` `DatePicker` `TimePicker` `Calendar`

**Overlay & Navigation**
`Tabs` `Tooltip` `Popover` `DropdownMenu` `Dialog` `Drawer` `ScrollArea` `Separator` `Breadcrumb` `StepIndicator`

**Display & Feedback**
`Badge` `Tag` `Callout` `Card` `Spinner` `Skeleton` `Progress` `EmptyState` `Stat` `DefinitionList` `IndicatorDot` `Toast`

**Layout & Composite**
`Topbar` `Sidebar` `StickyActionBar` `DataTable`

**Domain-specific** (기본: 미선택)
`AiChat` — 사용자가 명시할 때만 포함. 추가 도메인 컴포넌트 이름도 자유 입력.

### 6. 의존성 설치

설치할 패키지 목록을 한 화면에 보여준 뒤 사용자 confirm을 받는다. 선택한 스택 + 선택한 컴포넌트 기반으로 최소 필요 패키지만 포함.

감지된 패키지 매니저로 실행:
```bash
# 예시 (pnpm 감지 시)
pnpm add tailwindcss @radix-ui/react-* class-variance-authority lucide-react sonner overlay-kit
```

shadcn 초기화:
```bash
npx shadcn@latest init
```
`components.json`이 이미 있으면 건너뜀.

실패 시: 명령어를 출력하고 "직접 실행 후 재개할 수 있습니다"라고 안내.

### 7. Tailwind config · globals.css 패치

`references/token-mapping.md`의 변환표를 따라 DESIGN.md 토큰을 코드로 변환한다.

**`tailwind.config.{js,ts}` 패치**:
- `theme.extend.colors` — DESIGN.md `colors.*` 토큰을 CSS 변수 참조로 추가
- `theme.extend.fontFamily` — `typography.*.fontFamily` 수집 (중복 제거)
- `theme.extend.fontSize` — `typography.*` 레벨별 `[size, { lineHeight, letterSpacing }]`
- `theme.extend.borderRadius` — `rounded.*`
- `theme.extend.spacing` — `spacing.*`

기존 파일이 있으면 **덮어쓰지 않고 머지**. 충돌하는 키(같은 이름의 기존 토큰)가 있으면 사용자에게 나열 후 결정 요청(유지 / 덮어쓰기).

**`globals.css` (또는 `src/index.css`) 패치**:
shadcn 표준 CSS 변수 블록을 DESIGN.md 색상으로 교체:
```css
:root {
  --background: <surface or neutral>;
  --foreground: <on-surface>;
  --primary: <primary>;
  --primary-foreground: <on-primary or contrast>;
  --secondary: <secondary>;
  --secondary-foreground: <on-secondary or contrast>;
  --destructive: <error>;
  --border: <neutral-variant>;
  --input: <neutral-variant>;
  --ring: <primary>;
  --radius: <rounded.md>;
}
```
DESIGN.md에 다크 팔레트가 정의된 경우에만 `.dark { ... }` 블록 추가.

### 8. 컴포넌트 생성 — shadcn CLI + 토큰 패치

사용자가 선택한 컴포넌트를 순서대로 처리.

**shadcn registry에 있는 컴포넌트** (`references/component-catalog.md` 참고):
```bash
npx shadcn@latest add button input textarea label select checkbox radio-group switch
npx shadcn@latest add tabs tooltip popover dropdown-menu dialog drawer scroll-area separator breadcrumb
npx shadcn@latest add badge card skeleton progress calendar
npx shadcn@latest add data-table
```

각 추가 후 생성된 파일에서 `cva` variant의 색상·radius·font 값을 DESIGN.md 토큰 참조 클래스(`bg-primary`, `text-foreground`, `rounded-[var(--radius)]` 등)로 교체.

**shadcn registry에 없는 컴포넌트** — `references/component-catalog.md`의 코드 스켈레톤으로 `components/ui/<name>.tsx`를 직접 생성:
- `Combobox`, `MultiCombobox`
- `DatePicker`, `TimePicker`
- `StepIndicator`
- `EmptyState`, `Stat`, `DefinitionList`, `IndicatorDot`
- `Topbar`, `Sidebar`, `StickyActionBar`
- `Tag` (Badge variant 확장), `Callout` (Alert variant 확장)
- `Spinner`
- `Toast` (sonner `<Toaster>` 래퍼)
- `AiChat` (선택 시)

컴포넌트 파일 출력 경로는 `components.json`의 `aliases.components` 값 우선. 없으면 `components/ui/`.

### 9. 루트 프로바이더 와이어링

**Next.js App Router** (`app/layout.tsx`):
- `sonner` 선택 → `<Toaster />` 추가 (client component 내부에 위치)
- `overlay-kit` 선택 → `<OverlayProvider>` 로 children 감싸기

**Next.js Pages Router** (`pages/_app.tsx`), Vite (`main.tsx`), Remix (`app/root.tsx`):
- 각 엔트리 파일에 동일한 Provider 삽입 (파일이 없으면 생성 전 사용자에게 확인)

이미 Provider가 설정돼 있으면 건너뜀 (코드 내 `OverlayProvider` / `Toaster` 문자열로 감지).

### 10. 자가 검증 · 완료 안내

다음 Self-check 항목을 모두 통과한 후 사용자에게 완료 메시지 출력.

변경된 파일 목록(생성/수정)을 요약 표시. 다음 명령 안내:
```bash
# 타입 체크
pnpm typecheck   # 또는 tsc --noEmit

# 개발 서버
pnpm dev
```

데모 페이지 생성 제안 (원할 때만):
- Next.js: `app/(playground)/design-system/page.tsx`
- Vite: `src/pages/design-system.tsx`

## Self-check

- [ ] DESIGN.md 존재 확인 후에만 진행했는가
- [ ] YAML frontmatter 파싱 성공 (실패 시 중단)
- [ ] 기술 스택 사용자 확인 거쳤는가
- [ ] 컴포넌트 목록 사용자 확인 거쳤는가 (카테고리별)
- [ ] 기존 tailwind.config · globals.css 덮어쓰지 않고 머지했는가
- [ ] 충돌 키는 사용자에게 보여주고 결정 받았는가
- [ ] shadcn CLI로 추가한 컴포넌트의 cva variant에 DESIGN.md 토큰 클래스를 적용했는가
- [ ] registry에 없는 컴포넌트는 `references/component-catalog.md` 스켈레톤으로 생성했는가
- [ ] Provider(sonner Toaster, overlay-kit OverlayProvider) 와이어링 완료했는가
- [ ] 감지한 패키지 매니저 하나로 install 명령 통일했는가

## Anti-patterns

| ❌ Avoid | ✅ Instead |
|---|---|
| DESIGN.md 없이 진행 | 즉시 중단 + `create-design-md` 안내 |
| 기본 스택 확인 없이 install | 스택 목록 보여주고 사용자 confirm 후에만 |
| 카테고리 없이 25개 컴포넌트를 한 번에 묻기 | 카테고리별 multiSelect로 분할 |
| 기존 `tailwind.config` 덮어쓰기 | 머지 + 충돌 키 사용자 결정 |
| shadcn 기본 색상(`hsl(222.2 47.4% 11.2%)`) 그대로 | DESIGN.md 색상 토큰으로 CSS 변수 교체 |
| `npm`·`pnpm` 혼용 | lockfile 감지 결과로 통일 |
| Domain-specific 컴포넌트 기본 선택 | AiChat 등은 기본 미선택, 명시 시에만 포함 |
| frontmatter 없는 DESIGN.md로 계속 진행 | 파싱 실패 즉시 중단 + 안내 |

## References

- `references/token-mapping.md` — DESIGN.md YAML → tailwind.config + globals.css 변환표
- `references/component-catalog.md` — 컴포넌트별 shadcn 명령, 커스텀 스켈레톤, 토큰 패치 노트
