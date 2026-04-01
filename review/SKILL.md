---
name: review
description: 코드 리뷰 — Specialist Dispatch, Fix-First Pipeline, Confidence Score, Adversarial Review로 체계적 PR 리뷰
user-invocable: true
---

# Code Review Skill — 체계적 코드 리뷰

현재 변경사항을 CLAUDE.md 규칙 기준으로 리뷰합니다.
단순 체크리스트가 아닌, **Specialist Dispatch + Fix-First Pipeline**으로 구조적 문제를 잡습니다.

## 핵심 흐름

**Scope Detection → Plan Completion Audit → Specialist Dispatch → Fix-First Pipeline → Summary**

## 워크플로우

### Step 1: Scope Detection
```bash
git diff main...HEAD --stat
git diff main...HEAD
```
- 변경 파일 수, LOC, 영향 범위 파악
- **Scope Drift Detection**: 원래 의도(커밋 메시지/브랜치명) vs 실제 변경 비교
- 의도와 다른 변경이 있으면 경고

### Step 2: Plan Completion Audit
CLAUDE.md, TODOS.md, 또는 git 히스토리에서 원래 계획 추출:
- 계획된 항목 vs 실제 구현 대조
- 각 항목: **DONE** / **PARTIAL** / **NOT DONE** / **CHANGED**
- NOT DONE 항목은 왜 빠졌는지 조사

### Step 3: Specialist Dispatch
diff가 50줄 이상이면 전문가별 병렬 리뷰:

| 전문가 | 검사 항목 |
|--------|----------|
| **Testing** | 테스트 누락, 회귀 테스트, 커버리지 |
| **Maintainability** | 네이밍, 구조, 중복, 복잡도 |
| **Security** | XSS, injection, 인증/인가, 시크릿 노출 |
| **Performance** | 불필요한 리렌더, 번들 크기, N+1 패턴 |
| **API Contract** | 타입 안전성, 스키마 일치, 에러 처리 |
| **Accessibility** | ARIA, 키보드 네비게이션, 색상 대비 |
| **Design** | CLAUDE.md 디자인 규칙 준수, 일관성 |

각 발견에 **Confidence Score (1-10)** 부여:
- **증거 기반**: 코드에서 직접 확인한 것만
- **추측 금지**: 확인할 수 없으면 "unverified"로 표시

### Step 4: Fix-First Pipeline
모든 발견을 분류:

**AUTO-FIX** (즉시 적용):
- Import 순서 정리
- 누락된 타입 추가
- 사소한 네이밍 수정
- 불필요한 `as` 단언 제거

**ASK** (사용자 확인 필요):
- 아키텍처 변경 제안
- 성능 최적화 제안
- 테스트 추가 제안
- 보안 관련 수정

ASK 항목은 **하나의 배치 질문**으로 모아서 제시.

### Step 5: Summary

```
## Review Summary

### Scope
- 파일: X개 변경
- LOC: +X / -X
- Scope drift: 없음 / ⚠️ [설명]

### Plan Completion
- DONE: X / PARTIAL: X / NOT DONE: X

### Findings (심각도순)
| # | 심각도 | 분야 | 설명 | Confidence | 상태 |
|---|--------|------|------|------------|------|
| 1 | P1 | Security | ... | 9/10 | ASK |
| 2 | P2 | Performance | ... | 7/10 | AUTO-FIX |

### Auto-Fixed
- [적용된 자동 수정 목록]

### Actions Required
- [사용자 결정 필요 항목]
```

## 리뷰 체크리스트 (기본)

### 타입 안전성
- `any` 사용 여부
- 타입 가드 적절한 사용 여부
- `as` 단언 남용 여부
- 반환 타입 명시 여부

### 성능
- `useState` 대신 `useRef` 사용 가능한 곳
- 불필요한 리렌더 유발 패턴
- 큰 리스트의 가상화 여부
- React.memo / useMemo / useCallback 적절성

### 네이밍 & 컨벤션
- CLAUDE.md 네이밍 컨벤션 준수
- Import 순서
- 파일/폴더 구조 규칙 준수

### 에러 처리
- API 호출의 Result 타입 래핑
- ErrorBoundary 배치

### 접근성
- ARIA 속성
- 키보드 네비게이션
- alt 속성

## 핵심 규칙
- **커밋/PR 생성은 이 스킬의 역할이 아님** — 그것은 `/ship`의 역할
- **모든 발견에 file:line 포함**
- **Verification rule**: 안전하다는 증거를 인용하거나, "unverified"로 표시
- **문서 신선도 체크**: repo root의 .md 파일이 코드와 일치하는지 확인

## 한국어로 소통
리뷰 결과는 한국어로 요약. 코드 참조는 영어 그대로.
잘된 점은 구체적으로 인정, 개선점은 교육적으로 설명 (왜 개선해야 하는지 포함).
