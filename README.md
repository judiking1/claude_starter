# Claude Code Starter

Claude Code를 위한 스킬 팩 + 프로젝트 템플릿입니다.

PC에 한 번 설치하면 어떤 프로젝트에서든 `/ship`, `/qa`, `/review` 등 10개의 커스텀 스킬을 바로 사용할 수 있습니다. 새 프로젝트 시작 시에는 `init` 스크립트로 규칙 파일만 복사하면 됩니다.

> **Inspired by [gstack](https://github.com/garrytan/gstack)** — Builder principles, structured workflows, and safety guardrails adapted for React+Vite projects.

---

## 30초 설치

```bash
# PC에 한 번만 실행 (글로벌 설치)
git clone --depth 1 https://github.com/judiking1/claude_starter.git ~/.claude/skills/claude_starter
cd ~/.claude/skills/claude_starter && ./setup
```

이것으로 끝입니다. 이제 어떤 프로젝트에서든 Claude Code를 열면 10개 스킬을 사용할 수 있습니다.

---

## 새 프로젝트 시작하기

### 시나리오: "롤러코스터 타이쿤 웹 버전을 만들고 싶어"

```bash
# 1. 프로젝트 폴더 생성 (또는 기존 repo 사용)
mkdir roller-coaster-tycoon && cd roller-coaster-tycoon
git init

# 2. 프로젝트 템플릿 초기화 (규칙 파일 + 문서 복사)
~/.claude/skills/claude_starter/init roller-coaster-tycoon

# 3. Claude Code 실행
claude

# 4. 프로젝트 킥오프!
> /project-kickoff roller-coaster-tycoon
```

`init`이 하는 일:
- `CLAUDE.md` 복사 (Claude에게 내 코딩 규칙을 알려주는 파일)
- `.claude/rules/` 복사 (TypeScript, 성능, 에러 처리 등 상세 규칙)
- `docs/` 복사 (가이드, ADR 템플릿)
- 프로젝트 이름을 `CLAUDE.md`에 자동 반영

### 이미 존재하는 프로젝트에 추가

```bash
cd ~/projects/my-existing-project
~/.claude/skills/claude_starter/init my-existing-project
```

기존 코드에 영향 없습니다. `CLAUDE.md`, `.claude/rules/`, `docs/`만 추가됩니다.

### GitHub repo 연결

```bash
# init 후 GitHub에 push하려면
git add . && git commit -m "first commit"
git remote add origin https://github.com/내아이디/roller-coaster-tycoon.git
git branch -M main && git push -u origin main
```

---

## 이게 뭔가요?

이 보일러플레이트는 두 부분으로 구성됩니다:

### 1. 글로벌 스킬 (PC에 1번 설치, 모든 프로젝트에서 사용)

`~/.claude/skills/claude_starter/`에 설치되어, 어떤 프로젝트에서든 `/명령어`로 호출 가능합니다.

```
~/.claude/skills/claude_starter/
├── ship/SKILL.md              ← /ship
├── review/SKILL.md            ← /review
├── qa/SKILL.md                ← /qa
├── investigate/SKILL.md       ← /investigate
└── ...                        (10개 스킬)
```

### 2. 프로젝트 템플릿 (프로젝트마다 init으로 복사)

각 프로젝트에 복사되는 설정 파일들입니다. Claude Code가 이 파일들을 읽어서 내 코딩 규칙을 따릅니다.

```
내 프로젝트/
├── CLAUDE.md              ← Claude가 매번 자동으로 읽음 (핵심 규칙)
├── .claude/
│   ├── launch.json        ← dev 서버 설정
│   └── rules/             ← 관련 파일 작업 시 자동 로드
│       ├── typescript.md
│       ├── components.md
│       ├── safety.md
│       └── ...
└── docs/                  ← 프로젝트 문서
```

| 구분 | 설치 위치 | 설치 횟수 | 역할 |
|------|----------|----------|------|
| 글로벌 스킬 | `~/.claude/skills/claude_starter/` | PC에 1번 | `/ship`, `/qa` 등 커맨드 |
| 프로젝트 템플릿 | 각 프로젝트 폴더 | 프로젝트마다 `init` | 코딩 규칙, 문서 |

---

## 사용 가능한 스킬 (10개)

### 프로젝트를 처음 시작할 때
```
/project-kickoff my-app    ← 프로젝트 설정 (컨셉, 디자인, 기술 결정)
/design-system init        ← 디자인 시스템 자동 구축
```

### 코드를 작성하기 전에
```
/plan-review               ← 구현 계획 미리 검토 (아키텍처, 테스트 커버리지)
/team-lead 기능설명        ← 큰 기능을 여러 에이전트가 병렬로 구현
```

### 코드를 작성한 후에
```
/review                    ← 코드 리뷰 (7개 전문가 관점, 자동 수정)
/qa                        ← 브라우저에서 직접 테스트 (Health Score)
/ship                      ← 테스트 → 버전 → PR 생성까지 자동화
```

### 버그를 만났을 때
```
/investigate               ← 근본 원인 분석 (추측 금지, 가설 3회 제한)
/careful                   ← 위험 명령 실행 전 경고
```

### 주간 회고
```
/retro                     ← Git 히스토리 기반 생산성 분석
```

---

## 추천 워크플로우

```
/project-kickoff  →  /design-system init  →  /plan-review
        │
        ▼
     코딩 (Claude와 대화)
        │
        ▼
    /review  →  /qa  →  /ship
        │
        ▼
    /retro (주간)
```

---

## 빌더 원칙

이 보일러플레이트에 내장된 핵심 철학:

- **Boil the Lake**: AI로 완전한 구현의 한계비용이 0에 가까움 → 숏컷 대신 완전한 구현
- **Search Before Building**: 새로 만들기 전에 기존 코드·패턴·라이브러리 검색
- **User Sovereignty**: AI는 제안, 사용자가 결정
- **Anti-Sycophancy**: 포지션을 취하고, 증거 기반으로 판단

---

## 기본 기술 스택

| 영역 | 기술 |
|------|------|
| 프레임워크 | React + Vite |
| 언어 | TypeScript (strict) |
| 스타일링 | Tailwind CSS v4 |
| 상태관리 | Zustand |
| 서버 상태 | TanStack Query v5 |
| 패키지 매니저 | pnpm |
| 린터/포매터 | Biome |

> `/project-kickoff`에서 프로젝트에 맞게 조정 가능

---

## 파일 구조 (이 레포)

```
claude_starter/
├── setup                      # 글로벌 설치 스크립트 (PC에 1번)
├── init                       # 프로젝트 초기화 스크립트 (프로젝트마다)
│
├── careful/SKILL.md           # /careful — 위험 명령 가드레일
├── design-system/SKILL.md     # /design-system — 디자인 시스템
├── investigate/SKILL.md       # /investigate — 체계적 디버깅
├── plan-review/SKILL.md       # /plan-review — 아키텍처 리뷰
├── project-kickoff/SKILL.md   # /project-kickoff — 프로젝트 시작
├── qa/SKILL.md                # /qa — QA 테스트
├── retro/SKILL.md             # /retro — 주간 회고
├── review/SKILL.md            # /review — 코드 리뷰
├── ship/SKILL.md              # /ship — 자동 배포
├── team-lead/SKILL.md         # /team-lead — 팀장 모드
│
├── template/                  # 프로젝트 템플릿 (init이 복사)
│   ├── CLAUDE.md
│   ├── .claude/
│   │   ├── launch.json
│   │   └── rules/ (7개 규칙)
│   └── docs/
│       ├── usage-guide.md
│       ├── quick-start.md
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

---

## 자주 묻는 질문

### Q: `/명령어`가 동작하지 않아요
Claude Code 채팅창에서만 동작합니다. 일반 터미널 명령어가 아닙니다. `claude`를 실행한 후 입력하세요.

### Q: 기존에 프로젝트별로 사용하던 방식도 가능한가요?
네. `template/` 폴더의 내용을 직접 복사하고, 스킬도 `.claude/skills/`에 넣으면 이전처럼 프로젝트별로 독립적으로 사용할 수 있습니다.

### Q: React 말고 다른 프레임워크를 쓰고 싶으면?
`init` 후 `CLAUDE.md`의 기술 스택 섹션과 `.claude/rules/`를 수정하세요.

### Q: 이 파일들이 빌드에 영향을 주나요?
아닙니다. Claude Code가 읽는 문서일 뿐 빌드/실행과 무관합니다.

---

## 상세 가이드

`init` 실행 후 프로젝트에 복사되는 문서들:
- **[사용 가이드](template/docs/usage-guide.md)** — 코드를 모르는 사람도 따라할 수 있는 상세 설명
- **[빠른 시작](template/docs/quick-start.md)** — 경험자용 간결한 가이드
- **[세션 오케스트레이션](template/docs/session-orchestration.md)** — 프로젝트 규모별 세션 구성

---

## 라이선스

개인 사용 목적. 자유롭게 수정하여 사용하세요.
