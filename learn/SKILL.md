---
name: learn
description: 세션에서 발견한 패턴, 실수, 선호도를 기록하고 다음 세션에서 자동으로 활용
argument-hint: "review | prune | search <keyword>"
user-invocable: true
---

# Learn Skill — 세션 간 학습 관리

프로젝트에서 발견한 패턴, 실수, 결정 사항을 `.claude/learnings.md`에 기록합니다.
이 파일은 다음 세션에서 Claude가 자동으로 읽어, 같은 실수를 반복하지 않고 프로젝트에 맞는 판단을 합니다.

## 모드

### 1. `/learn` (인자 없음) — 현재 세션에서 배운 것 기록

현재 세션의 대화를 분석하여 학습할 만한 내용을 추출합니다.

**추출 대상:**
- 시도했다가 실패한 접근법과 그 이유
- 프로젝트에 특화된 패턴이나 규칙 발견
- 성능 관련 발견 (A 방식보다 B가 빠르다 등)
- 라이브러리/API의 예상과 다른 동작
- 사용자가 선호하는 코딩 스타일이나 결정
- 버그의 근본 원인과 해결 방법

**실행 순서:**
1. 현재 세션의 주요 발견 사항을 3-7개로 정리
2. 각 항목을 카테고리별로 분류: `pattern`, `pitfall`, `preference`, `performance`, `bugfix`
3. 사용자에게 보여주고 어떤 항목을 저장할지 확인 (AskUserQuestion)
4. 승인된 항목을 `.claude/learnings.md`에 추가

**기록 형식:**
```markdown
## [날짜]

### [카테고리] 제목
- **상황**: 어떤 맥락에서 발견했는지
- **학습**: 핵심 내용 (1-2문장)
- **근거**: 왜 이것이 맞는지 (선택)
```

**예시:**
```markdown
## 2026-04-01

### [pitfall] Zustand + immer 미들웨어 타입 추론 깨짐
- **상황**: store에 immer 미들웨어를 적용하자 TypeScript 타입 추론이 동작하지 않음
- **학습**: 이 프로젝트에서는 immer 대신 vanilla set()을 사용. 스프레드 연산자로 불변성 유지
- **근거**: immer의 Draft 타입과 Zustand의 제네릭이 충돌. TypeScript 5.x에서 해결 안 됨

### [performance] Canvas 2D vs WebGL 성능 비교
- **상황**: 놀이공원 그리드 100x100 타일 렌더링 테스트
- **학습**: 타일 1만개 이상은 Canvas 2D로 60fps 불가. WebGL 또는 OffscreenCanvas + Worker 필요
- **근거**: Chrome DevTools Performance 탭에서 프레임 드랍 확인

### [preference] 사용자는 한 파일 200줄 이하를 선호
- **상황**: GameGrid.tsx가 350줄이 되자 사용자가 분리 요청
- **학습**: 컴포넌트 파일은 200줄 이하로 유지. 초과 시 자동 분리 제안
```

### 2. `/learn review` — 축적된 학습 내용 확인

`.claude/learnings.md`의 전체 내용을 보여줍니다.

**표시 형식:**
```
=== 학습 내용 요약 ===

총 12개 항목 (pattern: 4, pitfall: 3, preference: 3, performance: 1, bugfix: 1)

최근 5개:
  [2026-04-01] [pitfall] Zustand + immer 타입 추론 깨짐
  [2026-04-01] [performance] Canvas 2D 1만 타일 한계
  [2026-03-28] [pattern] API 에러는 항상 ErrorBoundary로 위임
  [2026-03-28] [preference] 파일당 200줄 이하
  [2026-03-25] [bugfix] useEffect cleanup 누락으로 메모리 릭
```

### 3. `/learn prune` — 오래되거나 불필요한 항목 정리

학습 내용을 검토하고 정리합니다.

**실행 순서:**
1. `.claude/learnings.md` 전체를 읽음
2. 각 항목에 대해 판단:
   - **유지**: 여전히 유효한 학습
   - **제거 제안**: 프로젝트 변경으로 더 이상 해당 안 됨, 또는 `.claude/rules/`에 이미 반영됨
3. 제거 대상을 사용자에게 보여주고 확인 (AskUserQuestion)
4. 승인된 항목만 제거

**제거 기준:**
- 30일 이상 된 항목 중 rules/에 이미 반영된 것
- 삭제된 파일이나 라이브러리에 대한 학습
- 상충되는 항목 (최신 것만 유지)

### 4. `/learn search <keyword>` — 키워드로 검색

학습 내용에서 특정 키워드를 검색합니다.

```
> /learn search zustand
[2026-04-01] [pitfall] Zustand + immer 타입 추론 깨짐
[2026-03-20] [pattern] Zustand store는 도메인별로 분리
```

## 파일 위치

- **저장 위치**: 프로젝트 루트의 `.claude/learnings.md`
- **자동 로드**: Claude Code가 `.claude/` 내 파일을 세션 시작 시 인식
- **Git 커밋**: 팀과 공유하려면 커밋, 개인용이면 `.gitignore`에 추가

## 자동 학습 제안

세션 중 아래 상황이 발생하면, 세션 종료 전에 `/learn`을 제안합니다:
- `/investigate`로 버그를 해결한 후
- 같은 실수를 2회 이상 반복한 후
- 사용자가 접근법을 변경한 후 ("아 그거 말고 이렇게 해줘")
- 성능 문제를 발견하고 해결한 후

제안만 하고, 실행은 사용자가 결정합니다.

## 주의사항

- 학습 내용은 **사용자 확인 후에만** 저장 (자동 저장 없음)
- 민감한 정보(API 키, 비밀번호 등)는 기록하지 않음
- `.claude/rules/`로 승격할 만큼 중요한 학습은 prune 시 rules 이동을 제안

## 한국어로 소통
모든 학습 내용과 안내는 한국어.
