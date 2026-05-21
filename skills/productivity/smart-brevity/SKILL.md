---
name: smart-brevity
description: Use when writing or rewriting Slack announcements, emails, memos, meeting notes, proposals, or briefs. Triggered by "Smart Brevity로", "공지 써줘", "이메일 작성", "기획서 초안", "회의록 정리", "압축해줘", "smart brevity". NOT for PR descriptions, README, or code documentation.
---

# Smart Brevity 글쓰기

## Core Principle

**Tease → Tell → Why it matters → Go deeper.** 독자가 첫 줄에서 멈춰도 핵심을 이해해야 한다.

## Checklist

1. **모드 감지** — 입력 200자 이상이면 리라이팅, 이하(키워드·불릿)면 확장
2. **문서 종류 결정** — `slack` / `email-memo` / `proposal` (불명확하면 질문)
3. **필수 정보 확인** — audience·목적이 추론 불가면 한 번에 묶어 질문
4. **언어 결정** — 입력 언어 그대로 유지
5. **템플릿 적용** — `references/templates.md` 해당 종류 사용
6. **스타일 규칙 적용** — 한국어: `references/korean-style.md` / 영어: `references/english-style.md`
7. **자가 검증** — 아래 5개 체크 후 출력

## 자가 검증

- [ ] 헤드라인 1줄, 결론 선재 (Slack ≤15자 / 이메일 제목 ≤20자 / 기획서 제목 자유)
- [ ] 첫 단락 ≤80자
- [ ] "Why it matters" 섹션 존재 (**모든 문서 종류 필수**)
- [ ] Bullet ≤5개, 각 bullet ≤60자
- [ ] 능동태, 강한 동사, 형용사 최소화

## Anti-patterns — 이것이 보이면 수정

| ❌ 하지 말 것 | ✅ 대신 |
|---|---|
| "안녕하세요" / "감사합니다" 오프닝 | 헤드라인으로 바로 시작 |
| 이모지 (✅📢🙏 등) | 굵은 글씨로 강조 |
| Hedging ("아마", "것 같습니다", "수도 있습니다") | 단정형으로 |
| 헤드라인 17자 이상 | ≤15자, 동사형 |
| "Why it matters" 생략 ("어색하다"는 이유로) | 모든 문서에 필수, 이유는 아래 참조 |
| Bullet마다 "~합니다" 완전한 문장 | 명사구 또는 동사원형으로 |

## "Why it matters"는 왜 항상 필수인가

"실용적 문서에는 어색하다"는 판단은 Smart Brevity의 핵심 오해다.

독자는 바쁘다. 왜 읽어야 하는지 제시하지 않으면 건너뛴다. 짧은 Slack 공지조차 "왜 내가 지금 이걸 알아야 하는가"를 1문장으로 줘야 한다. 이 문장이 있어야 독자가 다음을 읽을 동기가 생긴다. 형식이 어색하게 느껴지면 **라벨 없이** 내용만 넣어도 된다 — 하지만 그 내용(독자의 이해관계)은 반드시 존재해야 한다.
