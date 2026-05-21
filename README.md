# skills

Claude Code 스킬 모음.

## Install

```
npx skills@latest add 6hamtol/skills
```

```
/plugin install 6hamtol/skills
```

또는 Claude Code marketplace에서 `6hamtol/skills`를 검색해 추가.

## Skills

### Productivity

| 스킬 | 설명 |
|------|------|
| [smart-brevity](./skills/productivity/smart-brevity/SKILL.md) | Slack 공지·이메일·회의록·기획서를 Smart Brevity 원칙으로 작성·리라이팅 |
| [create-design-md](./skills/productivity/create-design-md/SKILL.md) | 브랜드/제품의 DESIGN.md(컬러·타이포·컴포넌트 토큰)를 대화로 생성 |
| [implement-design-system](./skills/productivity/implement-design-system/SKILL.md) | DESIGN.md를 Tailwind·shadcn 디자인 시스템 코드로 구현 |

## 구조

```
skills/
├── engineering/   공개 — 코드 작업 관련
├── productivity/  공개 — 코드 외 워크플로우
├── misc/          공개 — 가끔 쓰는 유틸성
├── personal/      비공개 — 환경 전용
├── in-progress/   비공개 — 작성 중 초안
└── deprecated/    비공개 — 폐기 예정
```

Public 스킬(engineering, productivity, misc)은 이 README와 `.claude-plugin/plugin.json` 양쪽에 반드시 등재.
Private 스킬(personal, in-progress, deprecated)은 두 파일 모두 미등재.
