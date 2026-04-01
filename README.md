# Claude Code Starter

Claude Code를 최대한 활용하기 위한 개인 보일러플레이트입니다.

새 프로젝트를 시작할 때 이 레포를 복제하면, Claude Code가 나의 코딩 스타일과 규칙을 자동으로 인식하고 일관된 품질의 코드를 생성합니다.

> **Inspired by [gstack](https://github.com/garrytan/gstack)** — Builder principles, structured workflows, and safety guardrails adapted for React+Vite projects.

---

## 빌더 원칙

이 보일러플레이트에 내장된 핵심 철학:

- **Boil the Lake**: AI로 완전한 구현의 한계비용이 0에 가까움 → 숏컷 대신 완전한 구현
- **Search Before Building**: 새로 만들기 전에 기존 코드·패턴·라이브러리 검색
- **User Sovereignty**: AI는 제안, 사용자가 결정
- **Anti-Sycophancy**: 포지션을 취하고, 증거 기반으로 판단

---

## 이 보일러플레이트가 해결하는 문제

| 문제 | 해결 |
|------|------|
| 매번 프로젝트 초기 설정에 시간 소모 | `/project-kickoff` 한 번으로 컨셉~코드 생성까지 |
| Claude가 내 스타일을 모름 | `CLAUDE.md` + `rules/`로 자동 인식 |
| 규칙이 길면 컨텍스트 낭비 | 핵심만 `CLAUDE.md`, 나머지는 필요 시 자동 로드 |
| 큰 기능 구현 시 비효율적 | `/team-lead`로 병렬 에이전트 오케스트레이션 |
| 디자인 시스템 매번 새로 구축 | `/design-system init`으로 자동 생성 |
| 버그 수정이 추측 기반 | `/investigate`로 근본 원인 분석 후 수정 |
| 위험한 명령을 실수로 실행 | `/careful`로 가드레일 활성화 |
| PR 프로세스가 수동적 | `/ship`으로 테스트~PR까지 자동 파이프라인 |
| 코드 리뷰가 체크리스트 수준 | `/review`로 Specialist Dispatch + Fix-First |
| QA를 사람이 직접 해야 함 | `/qa`로 Claude Preview 기반 자동 QA |
| 생산성 추적이 안 됨 | `/retro`로 Git 기반 주간 회고 |

---

## 파일 구조

```
claude_starter/
├── CLAUDE.md                              # 핵심 규칙 (매 세션 자동 로드)
│
├── .claude/
│   ├── launch.json                        # Claude Preview dev 서버 설정
│   │
│   ├── rules/                             # 상세 규칙 (관련 파일 작업 시 자동 로드)
│   │   ├── typescript.md                  # *.ts, *.tsx 작업 시
│   │   ├── components.md                  # src/components/, src/pages/ 작업 시
│   │   ├── performance.md                 # src/ 전체 작업 시
│   │   ├── error-handling.md              # src/services/, src/hooks/ 작업 시
│   │   ├── design-system.md               # src/components/, src/styles/ 작업 시
│   │   ├── git-workflow.md                # 항상 로드
│   │   └── safety.md                      # 항상 로드 (위험 명령 가드레일)
│   │
│   └── skills/                            # 커스텀 워크플로우 (호출 시에만 로드)
│       ├── project-kickoff/SKILL.md       # /project-kickoff — 프로젝트 시작
│       ├── team-lead/SKILL.md             # /team-lead — 병렬 에이전트 오케스트레이션
│       ├── design-system/SKILL.md         # /design-system — 디자인 시스템
│       ├── review/SKILL.md                # /review — 코드 리뷰
│       ├── investigate/SKILL.md           # /investigate — 체계적 디버깅
│       ├── careful/SKILL.md               # /careful — 위험 명령 가드레일
│       ├── ship/SKILL.md                  # /ship — 자동 배포 파이프라인
│       ├── qa/SKILL.md                    # /qa — QA 테스트
│       ├── retro/SKILL.md                 # /retro — 주간 회고
│       └── plan-review/SKILL.md           # /plan-review — 아키텍처 리뷰
│
├── docs/
│   ├── quick-start.md                     # 프로젝트 시작 가이드
│   ├── session-orchestration.md           # 세션 팀 운영 가이드
│   └── adr/
│       └── 000-template.md                # 아키텍처 결정 기록 템플릿
│
└── README.md                              # 이 파일
```

---

## 3단계 자동 로딩 시스템

Claude Code는 문서를 3가지 레벨로 로딩합니다:

### Level 1: 항상 로드 (CLAUDE.md)
매 세션 시작 시 자동으로 컨텍스트에 주입. 핵심 규칙만 담아 짧게 유지:
- 빌더 원칙 (Boil the Lake, Search Before Building, Anti-Sycophancy)
- Scope Control (5파일 초과 시 확인, 3회 실패 시 에스컬레이션)
- 기술 스택, 협업 방식, 네이밍 컨벤션

### Level 2: 조건부 자동 로드 (.claude/rules/)
관련 파일을 작업할 때만 자동으로 로드. 컨텍스트 윈도우를 효율적으로 사용:
- `typescript.md` — TS 파일 편집 시
- `components.md` — 컴포넌트 작업 시
- `safety.md` — 항상 (위험 명령 가드레일)

### Level 3: 호출 시에만 로드 (.claude/skills/)
`/명령어`로 직접 호출할 때만 로드되는 워크플로우. 평소에는 컨텍스트 0.

---

## Skills 목록

### 개발 흐름
| 스킬 | 용도 | 핵심 특징 |
|------|------|-----------|
| `/project-kickoff` | 새 프로젝트 시작 | Premise Challenge, Alternatives Generation |
| `/plan-review` | 코딩 전 아키텍처 리뷰 | Scope Challenge, Test Coverage Diagram |
| `/team-lead` | 병렬 에이전트 오케스트레이션 | Scope Control, WTF-likelihood |
| `/design-system` | 디자인 시스템 구축 | init/add/audit 모드 |

### 품질 관리
| 스킬 | 용도 | 핵심 특징 |
|------|------|-----------|
| `/review` | 코드 리뷰 | Specialist Dispatch, Fix-First Pipeline, Confidence Score |
| `/qa` | QA 테스트 | Claude Preview 기반, Health Score, 자동 수정 |
| `/ship` | 자동 배포 | 테스트 → 버전 → CHANGELOG → Bisectable Commits → PR |

### 디버깅 & 안전
| 스킬 | 용도 | 핵심 특징 |
|------|------|-----------|
| `/investigate` | 체계적 디버깅 | Root Cause First, 3-Hypothesis Rule, Scope Lock |
| `/careful` | 위험 명령 가드레일 | rm -rf, DROP, force push 등 감지 |

### 분석
| 스킬 | 용도 | 핵심 특징 |
|------|------|-----------|
| `/retro` | 주간 회고 | Git 기반 생산성 분석, 핫스팟, 포커스 점수 |

---

## 기본 기술 스택

| 영역 | 기술 | 선택 이유 |
|------|------|-----------|
| 프레임워크 | React + Vite | 빠른 HMR, 가벼운 번들 |
| 언어 | TypeScript (strict) | 타입 안전성, no-any |
| 스타일링 | Tailwind CSS v4 | 유틸리티 퍼스트, 빠른 개발 |
| 상태관리 | Zustand | 경량, 보일러플레이트 최소 |
| 서버 상태 | TanStack Query v5 | 캐싱, 자동 리페치 |
| 패키지 매니저 | pnpm | npm 대비 2-3배 빠름, 디스크 절약 |
| 린터/포매터 | Biome | ESLint+Prettier 대비 10-100배 빠름 |
| 배포 | Vercel | 자동 프리뷰 배포 |

> 프로젝트에 따라 `/project-kickoff` 단계에서 변경 가능합니다.

---

## 사용법

### 1. 복제

```bash
git clone https://github.com/judiking1/claude_starter.git my-project
cd my-project
rm -rf .git && git init
```

### 2. 플레이스홀더 교체

`CLAUDE.md`에서:
- `[PROJECT_NAME]` → 프로젝트 이름
- `[PROJECT_DESCRIPTION]` → 프로젝트 설명

### 3. Claude Code 실행

```bash
claude
```

```
> /project-kickoff my-project
```

이후 대화형으로 모든 초기 설정이 진행됩니다.

---

## 세션 오케스트레이션

프로젝트 규모에 따라 Claude 세션을 "팀"처럼 구성할 수 있습니다.

| 규모 | 구성 | 사용 스킬 |
|------|------|-----------|
| 소규모 (1-2주) | 단일 세션 | `/project-kickoff` → 바로 개발 |
| 중규모 (1-2개월) | 단일 세션 + 에이전트 팀 | `/team-lead` |
| 대규모 (3개월+) | 다중 독립 세션 + docs/ 동기화 | 역할별 세션 |

상세: [docs/session-orchestration.md](docs/session-orchestration.md)

---

## 라이선스

개인 사용 목적. 자유롭게 수정하여 사용하세요.
