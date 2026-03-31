# Session Orchestration Guide

> Claude Code 세션을 팀처럼 운영하기 위한 가이드

---

## 핵심 원리

Claude Code에서 "팀 운영"은 두 가지 방식으로 가능합니다:

### 방식 1: 단일 세션 + Agent 서브에이전트 (권장)
```
[당신] ←→ [팀장 세션 (/team-lead)]
                ├── Agent: Frontend (worktree 격리)
                ├── Agent: Backend (worktree 격리)
                ├── Agent: Design (worktree 격리)
                └── Agent: QA (worktree 격리)
```

- **하나의 세션**에서 Agent tool로 서브에이전트를 동시 스폰
- 각 에이전트는 `isolation: "worktree"`로 **격리된 Git worktree**에서 작업
- 팀장(메인 세션)이 결과를 수집, 검증, 통합
- `/team-lead` 스킬로 이 패턴을 자동화

**장점**: 실시간 조율, 충돌 방지, 한 곳에서 전체 상황 파악
**적합**: 대부분의 프로젝트

### 방식 2: 다중 독립 세션
```
[당신] ←→ [Architect 세션] ←→ Git/docs로 소통
[당신] ←→ [Frontend 세션]  ←→ Git/docs로 소통
[당신] ←→ [Backend 세션]   ←→ Git/docs로 소통
```

- 별도의 터미널/창에서 각각 Claude 세션 실행
- **Git 브랜치와 docs/ 문서**로 세션 간 소통
- 각 세션 시작 시 역할을 명시하는 프롬프트 전달

**장점**: 각 세션이 깊은 컨텍스트 유지
**적합**: 장기간 진행되는 대규모 프로젝트

---

## 방식 1 상세: /team-lead 스킬 활용

### 사용법
```
/team-lead 사용자 인증 시스템을 구현해줘.
회원가입, 로그인, 비밀번호 재설정이 필요해.
```

### 팀장이 하는 일
1. **작업 분해**: 요청을 독립적 단위로 쪼갬
2. **에이전트 배정**: 각 단위에 맞는 에이전트 스폰
3. **병렬 실행**: 독립적 작업은 동시에, 의존적 작업은 순차적으로
4. **품질 검증**: 완료된 결과물을 CLAUDE.md 규칙으로 검증
5. **통합**: 모든 결과를 합치고 충돌 해결
6. **보고**: 한국어로 요약하여 사용자에게 전달

### 자율 피드백 구조
사용자의 의견이 아래 문서에 저장되어 있으면, 팀장이 이를 기준으로 자율적으로 판단:
- `CLAUDE.md` — 코딩 규칙, 성능 원칙
- `docs/project-brief.md` — 프로젝트 목적, 페르소나
- `docs/design-direction.md` — UX/UI 방향, 디자인 원칙
- `.claude/rules/` — 상세 코딩 규칙

**사용자 개입이 필요한 경우만** AskUserQuestion으로 질문합니다:
- 비즈니스 로직 결정 (기획 수준)
- 2가지 이상 동등한 접근법 중 선택
- 예상 외의 기술적 제약 발견 시

---

## 방식 2 상세: 독립 세션 운영

### 세션 시작 프롬프트 템플릿

**Architect 세션:**
```
너는 이 프로젝트의 아키텍트야. CLAUDE.md와 docs/ 문서를 읽어.
설계 결정은 docs/adr/에 기록하고, 다른 세션의 PR을 리뷰해줘.
```

**Frontend 세션:**
```
너는 프론트엔드 개발자야. CLAUDE.md와 docs/adr/을 읽어.
src/components/ui/의 공통 컴포넌트를 활용해서 [기능]을 구현해줘.
feat/[기능명] 브랜치에서 작업해.
```

**Backend 세션:**
```
너는 백엔드 개발자야. CLAUDE.md와 docs/adr/을 읽어.
API 설계를 기반으로 [기능]의 서버 사이드를 구현해줘.
feat/[기능명]-api 브랜치에서 작업해.
```

### 세션 간 동기화
```
docs/
├── adr/                   # 아키텍처 결정 (모든 세션 참조)
├── project-brief.md       # 프로젝트 개요 (읽기 전용)
├── design-direction.md    # 디자인 방향 (읽기 전용)
├── roadmap.md             # 진행 상황 (업데이트)
└── task-board.md          # 세션별 작업 현황 (업데이트)
```

---

## 프로젝트 규모별 권장 구성

### 소규모 (1-2주)
- 단일 세션, Skills 활용
- `/project-kickoff` → 바로 개발 시작

### 중규모 (1-2개월)
- `/team-lead`로 단일 세션 팀 운영
- 또는 Architect + Dev 2세션

### 대규모 (3개월+)
- `/team-lead` + 독립 세션 혼합
- PM 세션이 전체 조율
- Design/Frontend/Backend/QA 독립 세션

---

## 팁

- **worktree 활용**: 에이전트가 격리된 환경에서 작업하면 충돌 없음
- **docs/가 기억**: 세션이 종료되면 컨텍스트는 사라지지만, 문서는 남음
- **Memory도 활용**: 프로젝트 진행 상황을 memory에도 저장하면 세션 간 연속성 유지
- **ADR 기록**: 중요한 기술 결정은 docs/adr/에 기록하여 모든 세션이 참조
