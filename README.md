# Claude Code Starter

> Claude Code에게 "코딩 규칙"과 "업무 프로세스"를 한 번에 심어주는 스킬 팩 + 프로젝트 템플릿

코드를 한 줄도 모르는 사람이 와도, 설치하고 `/init my-project` 한 번이면 준비 끝. 12개 슬래시 커맨드가 프로젝트 기획부터 배포까지 전 과정을 자동화합니다.

> **Inspired by [gstack](https://github.com/garrytan/gstack)** — Garry Tan(Y Combinator CEO)이 만든 23개 스킬 팩에서 빌더 철학과 워크플로우를 가져왔습니다. gstack이 "1인 풀스택 팀"을 지향한다면, claude_starter는 **한국어 사용자를 위한 가벼운 입문용 버전**입니다.

---

## Quick Start — 30초 설치

**필요한 것:** [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Git](https://git-scm.com/)

```bash
git clone --depth 1 https://github.com/judiking1/claude_starter.git ~/.claude/skills/claude_starter
cd ~/.claude/skills/claude_starter && ./setup
```

이것으로 끝. 이제 어떤 프로젝트에서든 Claude Code를 열면 12개 스킬을 사용할 수 있습니다.

---

## 실제로 어떻게 쓰나요? — 전체 시나리오

### 시나리오: "롤러코스터 타이쿤 웹 버전을 만들고 싶어"

```bash
# 1. 프로젝트 폴더 생성
mkdir roller-coaster-tycoon && cd roller-coaster-tycoon
git init

# 2. Claude Code 실행
claude
```

```
You:    /init roller-coaster-tycoon
Claude: ✓ CLAUDE.md, .claude/rules/ (7개), docs/ 복사 완료

You:    /project-kickoff roller-coaster-tycoon
Claude: [5단계 대화형 설정 시작]

        Phase 1 — "이 프로젝트가 해결하려는 문제는?"
You:    롤러코스터 타이쿤 게임을 브라우저에서 플레이할 수 있게 만들고 싶어

        Phase 2 — "디자인 톤은?"
You:    픽셀아트 + 모던 UI 혼합

        Phase 3 — "백엔드는?"
You:    없음. 순수 클라이언트 사이드

        Phase 4 — Premise Challenge:
Claude: "이 프로젝트에서 가장 위험한 가정: 브라우저에서
         시뮬레이션 성능이 충분할까? WebGL + Web Worker
         조합이 필요할 수 있습니다."

        Phase 5 — Alternatives:
Claude: 1. Minimal: Canvas 2D + 그리드 (Effort: M)
        2. Ideal: WebGL + ECS 아키텍처 (Effort: XL)
        3. Creative: Phaser.js 엔진 활용 (Effort: L)
        RECOMMENDATION: 3번으로 시작, 성능 한계 시 2번으로 마이그레이션

You:    3번으로 해줘
Claude: [프로젝트 구조, package.json, 기본 설정 자동 생성]

You:    놀이공원 그리드 시스템을 만들어줘.
        사용자가 타일을 클릭하면 건물을 배치할 수 있어야 해.
Claude: [2,400줄, 11개 파일 작성. ~8분]

You:    /review
Claude: [AUTO-FIX] import 정리 2건
        [AUTO-FIX] 누락된 반환 타입 3건
        [ASK] Grid 컴포넌트에서 useRef 대신 useState를 쓰고 있습니다.
              성능상 useRef로 바꿀까요? (신뢰도: 9/10)
You:    네

You:    /qa
Claude: [브라우저에서 앱을 직접 열고 테스트]
        ✓ 그리드 렌더링 정상
        ✗ 타일 클릭 시 건물 배치 후 그리드가 1px 밀림
        [수정 → 재검증 → ✓ 해결]
        Health Score: 82/100

You:    /ship
Claude: Tests: 12 passed
        Version: 0.1.0
        PR: github.com/judiking1/roller-coaster-tycoon/pull/1
```

**8개 커맨드로 아이디어 → 설계 → 구현 → 리뷰 → QA → 배포까지.** 코파일럿이 아니라 팀입니다.

---

## 이게 뭔가요?

Claude Code Starter는 두 부분으로 구성됩니다:

### 1. 글로벌 스킬 (PC에 1번 설치 → 모든 프로젝트에서 사용)

`~/.claude/skills/claude_starter/`에 설치됩니다. 어떤 프로젝트를 열든 `/ship`, `/qa`, `/review` 같은 커맨드를 바로 쓸 수 있습니다.

### 2. 프로젝트 템플릿 (`/init`으로 프로젝트마다 복사)

`CLAUDE.md`, `.claude/rules/`, `docs/`를 프로젝트에 복사합니다. Claude Code가 이 파일들을 읽어서 내 코딩 규칙을 따릅니다. 빌드나 실행에는 영향이 없습니다.

| 구분 | 설치 위치 | 설치 횟수 | 역할 |
|------|----------|----------|------|
| **글로벌 스킬** | `~/.claude/skills/claude_starter/` | PC에 **1번** | `/ship`, `/qa` 등 커맨드 |
| **프로젝트 템플릿** | 각 프로젝트 폴더 | 프로젝트마다 `/init` | 코딩 규칙, 문서 |

```
~/.claude/skills/claude_starter/     각 프로젝트/
├── init/SKILL.md         (/init)    ├── CLAUDE.md          ← Claude가 매 세션마다 읽음
├── ship/SKILL.md         (/ship)    ├── .claude/
├── review/SKILL.md       (/review)  │   ├── launch.json
├── qa/SKILL.md           (/qa)      │   └── rules/ (7개)   ← 관련 파일 작업 시 자동 로드
├── investigate/SKILL.md             └── docs/              ← 가이드, ADR 템플릿
├── ... (11개 스킬)
└── template/             ← /init이 여기서 읽어서 복사
```

---

## 스킬 상세 (12개)

### 프로젝트 시작

| 스킬 | 역할 | 하는 일 |
|------|------|--------|
| `/init` | **프로젝트 초기화** | 현재 폴더에 CLAUDE.md + rules 7개 + docs를 자동 복사. 프로젝트 이름 반영. 이후 어떤 프로젝트에서든 동일한 코딩 규칙이 적용됨 |
| `/project-kickoff` | **기획자 + PM** | 5단계 대화형 설정. 컨셉 → 페르소나 → 디자인 → Premise Challenge(핵심 가정 반박) → Alternatives(2-3개 접근법 비교). 프로젝트 구조 자동 생성 |
| `/design-system` | **디자이너** | `init`: 디자인 토큰 + 기본 UI 컴포넌트 세트 생성. `Button`: 특정 컴포넌트 추가. `audit`: 기존 컴포넌트 일관성 검사 |

### 코드 작성 전

| 스킬 | 역할 | 하는 일 |
|------|------|--------|
| `/plan-review` | **아키텍트** | 구현 계획의 범위 검증 (8파일 초과 시 경고). 데이터 흐름 다이어그램, 테스트 커버리지 맵, 실패 시나리오 분석. 코딩 전 설계 실수를 잡아줌 |
| `/team-lead` | **팀장** | 큰 기능을 독립적 단위로 분해 → 여러 에이전트가 병렬 구현 → 결과물 품질 검증 + 통합. 위험도 평가(WTF-likelihood) 포함 |

### 코드 작성 후

| 스킬 | 역할 | 하는 일 |
|------|------|--------|
| `/review` | **시니어 개발자** | 7개 전문가 관점(Testing, Security, Performance, Accessibility...)에서 병렬 리뷰. 사소한 문제는 자동 수정(AUTO-FIX). 심각한 문제는 질문(ASK). 각 발견에 신뢰도 점수(1-10) |
| `/qa` | **QA 엔지니어** | 브라우저에서 직접 앱을 열고 테스트. 클릭, 폼 입력, 네비게이션 등 사용자 시나리오 실행. 버그 발견 시 자동 수정 → 재검증. Health Score(0-100) 측정 |
| `/ship` | **릴리즈 엔지니어** | 테스트 전체 실행 → 버전 번호 결정 → CHANGELOG 생성 → 커밋을 논리적 단위로 분리(bisectable) → GitHub PR 생성. 한 커맨드로 배포 준비 완료 |

### 디버깅 & 안전

| 스킬 | 역할 | 하는 일 |
|------|------|--------|
| `/investigate` | **디버거** | 근본 원인 분석(추측으로 수정 금지). 가설 수립 → 검증을 최대 3회. 3회 실패 시 에스컬레이션. 관련 파일만 수정하는 Scope Lock |
| `/careful` | **안전 담당** | `rm -rf`, `DROP TABLE`, `git push -f` 등 위험 명령 감지 시 경고 + 확인 요청. `node_modules` 등 빌드 산출물은 안전하게 허용 |

### 회고 & 학습

| 스킬 | 역할 | 하는 일 |
|------|------|--------|
| `/retro` | **매니저** | Git 히스토리 기반 생산성 분석. 시간대별 작업 분포, feat/fix/refactor 비율, 핫스팟 파일, 포커스 점수, 연속 출근 streak |
| `/learn` | **메모리** | 세션에서 발견한 패턴·실수·선호도를 `.claude/learnings.md`에 기록. 다음 세션에서 자동 활용. `review`로 확인, `prune`으로 정리, `search`로 검색 |

---

## 추천 워크플로우 — 스프린트

gstack과 마찬가지로, 스킬은 스프린트 순서로 설계되어 있습니다:

**기획 → 설계 → 구현 → 리뷰 → 테스트 → 배포 → 회고**

```
/init  →  /project-kickoff  →  /design-system init  →  /plan-review
                                                            │
                                                            ▼
                                                     코딩 (Claude와 대화)
                                                     큰 기능은 /team-lead
                                                            │
                                                            ▼
                                              /review  →  /qa  →  /ship
                                                            │
                                                            ▼
                                                /retro (주간) + /learn
```

각 스킬은 이전 스킬의 산출물을 읽습니다. `/project-kickoff`이 만든 설정을 `/plan-review`가 참조하고, `/review`가 잡은 이슈를 `/ship`이 확인합니다.

---

## 빌더 원칙

이 보일러플레이트에 내장된 핵심 철학 ([gstack ETHOS](https://github.com/garrytan/gstack/blob/main/ETHOS.md)에서 영감):

- **Boil the Lake** — AI로 완전한 구현의 한계비용이 0에 가까움. 숏컷 대신 완전한 구현. 단, "lake"(달성 가능)과 "ocean"(끝없는 재작성)을 구분
- **Search Before Building** — 새로 만들기 전에 기존 코드·패턴·라이브러리 검색. 3가지 지식 계층: 확립된 패턴 → 현재 best practice → first-principles 추론
- **User Sovereignty** — AI는 제안, 사용자가 결정. 사용자만이 도메인 지식과 비즈니스 맥락을 앎
- **Anti-Sycophancy** — 포지션을 취하라. "That's interesting" → 왜 좋은지/나쁜지 판단. 증거 기반 반론

---

## 기본 기술 스택

| 영역 | 기술 | 비고 |
|------|------|------|
| 프레임워크 | React + Vite | SPA 기본 |
| 언어 | TypeScript (strict) | `any` 금지, 타입 가드 필수 |
| 스타일링 | Tailwind CSS v4 | 유틸리티 퍼스트 |
| 상태관리 | Zustand | 경량 전역 상태 |
| 서버 상태 | TanStack Query v5 | fetch 기반 |
| 패키지 매니저 | pnpm | 빠른 속도, 엄격한 의존성 |
| 린터/포매터 | Biome | 올인원 |

> **다른 스택을 쓰고 싶으면?** `/project-kickoff`에서 대화하며 바꿀 수 있습니다. 또는 `/init` 후 `CLAUDE.md`와 `.claude/rules/`를 수동으로 수정하세요.

---

## 설치 상세

### 요구 사항

| 도구 | 용도 | 설치 확인 |
|------|------|----------|
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | AI 코딩 에이전트 | `claude --version` |
| [Git](https://git-scm.com/) | 버전 관리 | `git --version` |
| [Node.js](https://nodejs.org/) v18+ | JavaScript 런타임 | `node --version` |
| [pnpm](https://pnpm.io/) | 패키지 매니저 | `pnpm --version` (없으면: `npm i -g pnpm`) |

### Step 1: 글로벌 설치 (PC에 한 번만)

```bash
git clone --depth 1 https://github.com/judiking1/claude_starter.git ~/.claude/skills/claude_starter
cd ~/.claude/skills/claude_starter && ./setup
```

설치하면 이런 메시지가 나옵니다:

```
=== Claude Code Starter Setup ===

설치 위치: ~/.claude/skills/claude_starter
발견된 스킬: 11개

사용 가능한 스킬:
  /init             — 프로젝트 템플릿 초기화
  /project-kickoff  — 새 프로젝트 시작
  /plan-review      — 아키텍처 리뷰
  /team-lead        — 병렬 에이전트 오케스트레이션
  /design-system    — 디자인 시스템 구축
  /review           — 코드 리뷰
  /qa               — QA 테스트
  /ship             — 자동 배포
  /investigate      — 체계적 디버깅
  /careful          — 위험 명령 가드레일
  /retro            — 주간 회고

설치 완료!

=== 다음 단계 ===

새 프로젝트를 시작하려면:
  cd ~/projects/my-project
  claude
  > /init my-project
  > /project-kickoff my-project
```

### Step 2: 새 프로젝트에서 사용

```bash
mkdir my-project && cd my-project
git init
claude
```

Claude Code 채팅창에서:
```
> /init my-project
> /project-kickoff my-project
```

### Step 3: 팀원에게 공유 (선택)

프로젝트 템플릿은 git에 커밋하면 팀원도 같은 규칙을 사용합니다:
```bash
git add CLAUDE.md .claude docs
git commit -m "feat: add Claude Code boilerplate"
git push
```

팀원은 글로벌 스킬만 별도 설치하면 됩니다:
```bash
git clone --depth 1 https://github.com/judiking1/claude_starter.git ~/.claude/skills/claude_starter
cd ~/.claude/skills/claude_starter && ./setup
```

---

## 이미 존재하는 프로젝트에 추가

```bash
cd ~/projects/my-existing-project
claude
> /init my-existing-project
```

기존 코드에 영향 없습니다. `CLAUDE.md`, `.claude/rules/`, `docs/`는 Claude Code가 읽는 문서일 뿐, 빌드·실행과 무관합니다.

> 이미 `.claude/`나 `CLAUDE.md`가 있으면 덮어쓸지 물어봅니다.

---

## GitHub repo 연결

```bash
# /init 후 GitHub에 push하려면
git add . && git commit -m "first commit"
git remote add origin https://github.com/내아이디/my-project.git
git branch -M main && git push -u origin main
```

---

## 파일 구조

```
claude_starter/
├── setup                      # 글로벌 설치 스크립트 (PC에 1번)
│
├── init/SKILL.md              # /init — 프로젝트 템플릿 초기화
├── careful/SKILL.md           # /careful — 위험 명령 가드레일
├── design-system/SKILL.md     # /design-system — 디자인 시스템
├── investigate/SKILL.md       # /investigate — 체계적 디버깅
├── learn/SKILL.md             # /learn — 세션 간 학습 관리
├── plan-review/SKILL.md       # /plan-review — 아키텍처 리뷰
├── project-kickoff/SKILL.md   # /project-kickoff — 프로젝트 시작
├── qa/SKILL.md                # /qa — QA 테스트
├── retro/SKILL.md             # /retro — 주간 회고
├── review/SKILL.md            # /review — 코드 리뷰
├── ship/SKILL.md              # /ship — 자동 배포
├── team-lead/SKILL.md         # /team-lead — 팀장 모드
│
├── template/                  # /init이 여기서 읽어서 프로젝트에 복사
│   ├── CLAUDE.md              # Claude 핵심 규칙 (프로젝트명, 스택, 컨벤션)
│   ├── .claude/
│   │   ├── launch.json        # dev 서버 설정
│   │   └── rules/             # 조건부 자동 로드 규칙 7개
│   │       ├── typescript.md
│   │       ├── components.md
│   │       ├── performance.md
│   │       ├── error-handling.md
│   │       ├── design-system.md
│   │       ├── git-workflow.md
│   │       └── safety.md
│   └── docs/
│       ├── usage-guide.md     # 초보자용 상세 가이드
│       ├── quick-start.md     # 경험자용 간결한 가이드
│       ├── session-orchestration.md
│       └── adr/000-template.md
│
└── README.md
```

---

## 업데이트

```bash
cd ~/.claude/skills/claude_starter
git pull
```

새 스킬이 추가되거나 기존 스킬이 개선되면 `git pull`로 바로 반영됩니다.

---

## gstack과의 비교

| | **claude_starter** | **gstack** |
|---|---|---|
| 대상 | 한국어 사용자, 입문자 | 영어, 숙련 개발자 |
| 스킬 수 | 11개 | 23개+ |
| 프로젝트 템플릿 | ✅ 포함 (CLAUDE.md, rules, docs) | ❌ 없음 (스킬만) |
| 기본 스택 | React + Vite + TypeScript | 프레임워크 무관 |
| 브라우저 자동화 | Claude Preview 기반 `/qa` | Playwright 기반 `/browse` |
| 설치 복잡도 | Git + Node.js + pnpm | Git + Bun + Node.js |
| 디자인 도구 | `/design-system` | `/design-consultation`, `/design-shotgun`, `/design-html` |
| 보안 감사 | `/careful` | `/cso` (OWASP + STRIDE) |
| 멀티 AI | ❌ | `/codex` (OpenAI 교차 리뷰) |
| 학습 기능 | `/learn` (패턴·실수·선호도 기록) | `/learn` (세션 간 학습) |

> **어떤 걸 쓸까요?** 처음이라면 claude_starter로 시작 → 익숙해지면 gstack으로 확장하는 것을 추천합니다. 두 개를 동시에 설치할 수도 있습니다.

---

## 자주 묻는 질문

### Q: `/명령어`가 동작하지 않아요
Claude Code 채팅창에서만 동작합니다. 일반 터미널 명령어가 아닙니다. `claude`를 실행한 후 입력하세요.

### Q: 기존에 프로젝트별로 사용하던 방식도 가능한가요?
네. `template/` 폴더의 내용을 직접 복사하고, 스킬도 `.claude/skills/`에 넣으면 이전처럼 프로젝트별로 독립적으로 사용할 수 있습니다.

### Q: React 말고 다른 프레임워크를 쓰고 싶으면?
`/init` 후 `CLAUDE.md`의 기술 스택 섹션과 `.claude/rules/`를 수정하세요. `/project-kickoff`에서 대화하며 바꿀 수도 있습니다.

### Q: 이 파일들이 빌드에 영향을 주나요?
아닙니다. `CLAUDE.md`, `.claude/rules/`, `docs/`는 Claude Code가 읽는 문서일 뿐 빌드·실행과 무관합니다.

### Q: .gitignore에 넣어야 하나요?
`CLAUDE.md`, `.claude/rules/`, `docs/`는 **넣지 마세요** — repo에 커밋해야 팀원도 같은 규칙을 사용합니다. `.claude/settings.local.json`만 `.gitignore`에 넣으세요.

### Q: Windows에서도 동작하나요?
네. Claude Code가 Windows를 지원하면 이 보일러플레이트도 동작합니다. Git Bash 또는 WSL에서 `./setup`을 실행하세요.

### Q: 팀에서 같이 쓸 수 있나요?
네. 프로젝트 템플릿은 repo에 커밋하면 팀원 모두 같은 규칙으로 작업합니다. 글로벌 스킬은 각자 설치해야 합니다.

### Q: gstack과 동시에 쓸 수 있나요?
네. `~/.claude/skills/claude_starter/`와 `~/.claude/skills/gstack/`은 독립적으로 동작합니다. 스킬 이름이 겹치면 둘 다 표시되며, 원하는 것을 선택할 수 있습니다.

---

## 상세 가이드

`/init` 후 프로젝트에 복사되는 문서들:
- **[상세 사용 가이드](template/docs/usage-guide.md)** — 코드를 모르는 사람도 따라할 수 있는 단계별 설명
- **[빠른 시작](template/docs/quick-start.md)** — 경험자용 간결한 가이드
- **[세션 오케스트레이션](template/docs/session-orchestration.md)** — 프로젝트 규모별 세션 구성

---

## 라이선스

MIT. 자유롭게 수정하여 사용하세요.
