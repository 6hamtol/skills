# Skills 저장소 관리 규칙

이 저장소에서 스킬을 추가·수정할 때 따를 규칙.

## 버킷 분류 기준

| 버킷 | 공개 | 용도 |
|------|------|------|
| `engineering/` | ✅ | 코드 작업 관련 (디버깅, TDD, 아키텍처 등) |
| `productivity/` | ✅ | 코드 외 워크플로우 (문서, 커뮤니케이션, 계획) |
| `misc/` | ✅ | 가끔 쓰는 유틸성 스킬 |
| `personal/` | ❌ | 본인 환경에만 의미 있는 스킬 (미배포) |
| `in-progress/` | ❌ | 아직 완성 전인 초안 |
| `deprecated/` | ❌ | 더 이상 쓰지 않지만 보관 중인 스킬 |

## 새 스킬 추가 시 체크리스트

Public 버킷(engineering, productivity, misc)에 스킬을 추가할 때:

1. `skills/<bucket>/<skill-name>/SKILL.md` 생성
2. `skills/<bucket>/README.md`에 한 줄 설명 + 링크 추가
3. 루트 `README.md`의 스킬 목록에 추가
4. `.claude-plugin/plugin.json`의 `skills` 배열에 경로 추가

Private 버킷이면 1번만 하고 2–4번은 생략.

## SKILL.md 형식

```markdown
---
name: <skill-name>
description: <용도 한 줄>. 트리거 조건: "키워드1", "키워드2", 상황 설명. NOT for: 제외 케이스.
---

# 스킬 제목
...
```

- `name`: 슬래시 커맨드 식별자 (kebab-case)
- `description`: 언제 이 스킬을 써야 하는지 트리거 조건을 명시. "NOT for:"로 제외 케이스 명시.
