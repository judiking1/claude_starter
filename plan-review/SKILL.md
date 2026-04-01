---
name: plan-review
description: 코딩 전 아키텍처 리뷰 — Scope Challenge, 테스트 커버리지 다이어그램, Failure Mode 분석, 재사용 분석
user-invocable: true
---

# Plan Review Skill — 코딩 전 아키텍처 리뷰

코드 작성 전에 실행 계획을 검토합니다. 구조적 문제는 코딩 전에 잡는 것이 가장 저렴합니다.

## 트리거
- 구현 계획이 있을 때 (Plan Mode 진입 후)
- "이거 괜찮을까?", "구조 리뷰해줘", "시작 전에 확인"
- 복잡한 기능 구현 시작 전

## 워크플로우

### Step 0: Scope Challenge
계획의 범위가 적절한지 먼저 검증:

| 조건 | 액션 |
|------|------|
| 파일 8개 초과 | 범위 축소 토론 시작 |
| 새 클래스/모듈 2개+ | 기존 코드 재사용 가능성 검토 |
| 3개+ 디렉토리 걸침 | 작업 분할 제안 |

**Boil the Lake vs Ocean 판단**:
- 테스트 커버리지 100% → Lake (하라)
- 전체 시스템 재작성 → Ocean (하지 마라)

### Section 1: Architecture Review
데이터 흐름과 시스템 설계 검토:
- 컴포넌트 간 의존성 방향
- 상태 관리 전략 (Zustand store vs local state vs URL state)
- API 호출 패턴 (TanStack Query mutation/query)
- 에러 경로 (ErrorBoundary, Result 타입)

각 발견에 **Confidence Score (1-10)** 부여:
- 1-3: 추측 (증거 부족)
- 4-6: 가능성 있음 (부분 증거)
- 7-10: 확신 (코드에서 확인)

### Section 2: Code Quality
- 기존 코드와의 일관성
- CLAUDE.md 규칙 준수 예측
- 과도한 추상화 또는 부족한 추상화

### Section 3: Test Coverage Diagram
모든 코드 경로를 추적하여 ASCII 커버리지 다이어그램 생성:

```
handleSubmit()
├── validation passes  → [TESTED ✓]
│   ├── API success   → [TESTED ✓]
│   └── API failure   → [GAP ✗] confidence: 8
├── validation fails   → [TESTED ✓]
└── network error      → [GAP ✗] confidence: 9
```

각 GAP에 테스트 작성 방향 제안.

### Section 4: Performance Review
React 특화 성능 검토:
- N+1 렌더링 (불필요한 리렌더)
- 큰 번들 임포트 (전체 라이브러리 vs named import)
- 메모이제이션 필요 여부
- 가상화 필요 여부 (리스트 길이)

### Section 5: Failure Modes
새 코드 경로별 현실적 프로덕션 실패 시나리오:

| 코드 경로 | 실패 시나리오 | 에러 처리 | 테스트 |
|-----------|-------------|----------|--------|
| /api/users fetch | 네트워크 타임아웃 | Result 타입 | ✗ |
| form submit | 중복 제출 | debounce | ✗ |

### Section 6: Reuse Analysis
기존 코드에서 재사용 가능한 부분 식별:
- 기존 컴포넌트, 훅, 유틸리티
- 비슷한 패턴의 기존 구현
- 중복 코드 방지 전략

## 출력 형식

```
## Plan Review Summary

### Scope: ✅ 적정 / ⚠️ 확장 필요 / 🔴 축소 필요
- 파일 수: X개
- 영향 디렉토리: X개
- 재사용 가능 코드: X건

### Findings (우선순위순)
1. [P1] [Architecture] ... (confidence: 8/10)
2. [P2] [Testing] ... (confidence: 7/10)
...

### Test Coverage
[ASCII 다이어그램]

### Failure Modes
[테이블]

### NOT in scope (명시적 제외)
- ...

### 다음 단계
1. ...
2. ...
```

## 핵심 원칙
- **하나의 이슈당 하나의 AskUserQuestion**: 배치 질문 금지, 명시적 결정 강제
- **Confidence calibration**: 모든 발견에 1-10 점수
- **미결정 사항은 절대 묵살하지 않음**: "NOT in scope"에 명시
- **Regression rule**: 기존 동작이 깨지면 회귀 테스트 필수 (비협상)

## 한국어로 소통
리뷰 결과는 한국어. 코드 참조와 다이어그램은 영어.
