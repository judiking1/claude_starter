# Claude Code Starter

Claude Code를 최대한 활용하기 위한 개인 보일러플레이트입니다.

새 프로젝트를 시작할 때 이 레포의 설정 파일들을 가져오면, Claude Code가 나의 코딩 스타일과 규칙을 자동으로 인식하고 일관된 품질의 코드를 생성합니다.

> **Inspired by [gstack](https://github.com/garrytan/gstack)** — Builder principles, structured workflows, and safety guardrails adapted for React+Vite projects.

---

## 이게 뭔가요?

Claude Code는 터미널에서 실행하는 AI 코딩 도구입니다.
그런데 매번 새 프로젝트를 시작할 때마다 "TypeScript strict 써줘", "네이밍은 camelCase로 해줘" 같은 걸 반복해서 말해야 합니다.

**이 보일러플레이트는 그 문제를 해결합니다.**

프로젝트 폴더에 설정 파일들(`.claude/`, `CLAUDE.md`)을 넣어두면, Claude Code가 자동으로 읽어서 매번 같은 말을 할 필요가 없어집니다. 거기에 10개의 커스텀 명령어(`/스킬`)를 포함하고 있어서 프로젝트 시작, 코드 리뷰, QA, 배포까지 체계적으로 진행할 수 있습니다.

---

## 사용 시나리오: 롤러코스터 타이쿤 웹 버전 만들기

> "나는 롤러코스터 타이쿤을 웹으로 만들고 싶어. GitHub에 repo도 만들었어. 이제 어떻게 하지?"

이런 상황을 처음부터 끝까지 따라가 봅시다.

### 전제 조건

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview)가 설치되어 있어야 합니다
- [Node.js](https://nodejs.org/) (v18+)가 설치되어 있어야 합니다
- [pnpm](https://pnpm.io/)이 설치되어 있어야 합니다 (`npm install -g pnpm`)
- [Git](https://git-scm.com/)이 설치되어 있어야 합니다

### 방법 A: 이 레포를 복제해서 새 프로젝트로 시작 (추천)

가장 간단한 방법입니다. 이 보일러플레이트 자체를 프로젝트의 시작점으로 사용합니다.

```bash
# 1. 이 보일러플레이트를 내 프로젝트 이름으로 복제
git clone https://github.com/judiking1/claude_starter.git roller-coaster-tycoon
cd roller-coaster-tycoon

# 2. 보일러플레이트의 git 히스토리를 지우고 새로 시작
rm -rf .git
git init
git add .
git commit -m "first commit"

# 3. 내 GitHub repo와 연결 (이미 만들어둔 빈 repo가 있다면)
git remote add origin https://github.com/내아이디/roller-coaster-tycoon.git
git branch -M main
git push -u origin main
```

### 방법 B: 이미 만들어둔 repo에 보일러플레이트 파일만 복사

이미 코드가 있는 프로젝트에 보일러플레이트를 추가하고 싶을 때 사용합니다.

```bash
# 1. 보일러플레이트를 임시로 다운로드
git clone https://github.com/judiking1/claude_starter.git /tmp/claude_starter

# 2. 내 프로젝트 폴더로 이동
cd ~/projects/roller-coaster-tycoon

# 3. 필요한 파일들만 복사
cp /tmp/claude_starter/CLAUDE.md .
cp -r /tmp/claude_starter/.claude .
cp -r /tmp/claude_starter/docs .

# 4. 임시 폴더 삭제
rm -rf /tmp/claude_starter

# 5. 커밋
git add CLAUDE.md .claude docs
git commit -m "feat: add Claude Code boilerplate"
```

### 복제/복사 후 해야 할 것

```bash
# 1. CLAUDE.md에서 프로젝트 정보를 수정
#    [PROJECT_NAME] → roller-coaster-tycoon
#    [PROJECT_DESCRIPTION] → 롤러코스터 타이쿤 웹 버전

# 2. Claude Code를 실행
claude

# 3. 프로젝트 킥오프 시작!
> /project-kickoff roller-coaster-tycoon
```

`/project-kickoff`을 실행하면 Claude가 대화형으로 질문합니다:
- "이 프로젝트가 해결하려는 문제는?" → 롤러코스터 타이쿤을 웹에서 플레이하고 싶어
- "타겟 유저는?" → 레트로 게임 팬, 캐주얼 게이머
- "핵심 기능 3-5개는?" → 놀이공원 건설, 롤러코스터 디자인, 손님 시뮬레이션...
- "디자인 톤은?" → 픽셀아트 + 모던 UI
- ...등등

이 대화가 끝나면 Claude가 자동으로:
- 프로젝트 구조를 생성하고
- 필요한 패키지를 설치하고
- 기본 레이아웃 컴포넌트를 만들어줍니다.

---

## 내가 만든 게 아니라 Claude가 읽는 파일

이 보일러플레이트의 파일들은 **내가 직접 실행하는 것이 아닙니다.**
Claude Code가 자동으로 읽어서 행동 규칙으로 사용합니다.

```
내 프로젝트/
├── CLAUDE.md          ← Claude가 매번 자동으로 읽음 (내 규칙)
├── .claude/
│   ├── rules/         ← 관련 파일 작업 시 자동으로 읽힘
│   └── skills/        ← /명령어 입력 시에만 읽힘
├── src/               ← 내 코드 (여기는 내가 & Claude가 작업)
└── ...
```

| 파일/폴더 | 누가 읽나 | 언제 읽히나 |
|-----------|----------|-----------|
| `CLAUDE.md` | Claude | 매 대화 시작 시 자동 |
| `.claude/rules/*.md` | Claude | 관련 파일 편집 시 자동 |
| `.claude/skills/*/SKILL.md` | Claude | `/명령어` 입력 시에만 |
| `docs/` | Claude & 나 | 프로젝트 문서 참조 시 |

---

## 사용 가능한 명령어 (Skills)

Claude Code 채팅창에서 `/명령어`를 입력하면 해당 워크플로우가 실행됩니다.

### 프로젝트를 처음 시작할 때
```
/project-kickoff my-app    ← 프로젝트 설정 (컨셉, 디자인, 기술 결정)
/design-system init        ← 디자인 시스템 자동 구축 (버튼, 입력, 모달 등)
```

### 코드를 작성하기 전에
```
/plan-review               ← 구현 계획을 미리 검토 (아키텍처, 테스트 커버리지)
/team-lead 기능설명        ← 큰 기능을 여러 에이전트에게 분배하여 병렬 구현
```

### 코드를 작성한 후에
```
/review                    ← 코드 리뷰 (7개 전문가 관점, 자동 수정)
/qa                        ← 브라우저에서 직접 테스트 (Health Score 측정)
/ship                      ← 테스트 → 버전 → PR 생성까지 자동화
```

### 버그를 만났을 때
```
/investigate               ← 근본 원인 분석 (추측 금지, 가설 3회 제한)
/careful                   ← 위험한 명령어 실행 전 경고 (rm -rf, force push 등)
```

### 주간 회고
```
/retro                     ← Git 히스토리 기반 생산성 분석
/retro 30d                 ← 최근 30일 회고
```

---

## 추천 개발 워크플로우

```
프로젝트 시작
  │
  ▼
/project-kickoff my-app     ← 1단계: 프로젝트 설정
  │
  ▼
/design-system init         ← 2단계: 디자인 시스템
  │
  ▼
/plan-review                ← 3단계: 구현 전 아키텍처 검증
  │
  ▼
  코딩 (Claude와 대화하며)   ← 4단계: 기능 구현
  │
  ▼
/review                     ← 5단계: 코드 리뷰
  │
  ▼
/qa                         ← 6단계: QA 테스트
  │
  ▼
/ship                       ← 7단계: PR 생성 & 배포
  │
  ▼
/retro                      ← 8단계: 주간 회고 (선택)
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

| 영역 | 기술 | 선택 이유 |
|------|------|-----------|
| 프레임워크 | React + Vite | 빠른 HMR, 가벼운 번들 |
| 언어 | TypeScript (strict) | 타입 안전성, no-any |
| 스타일링 | Tailwind CSS v4 | 유틸리티 퍼스트, 빠른 개발 |
| 상태관리 | Zustand | 경량, 보일러플레이트 최소 |
| 서버 상태 | TanStack Query v5 | 캐싱, 자동 리페치 |
| 패키지 매니저 | pnpm | npm 대비 2-3배 빠름 |
| 린터/포매터 | Biome | ESLint+Prettier 대비 10-100배 빠름 |
| 배포 | Vercel | 자동 프리뷰 배포 |

> 프로젝트에 따라 `/project-kickoff` 단계에서 변경 가능합니다.

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
│   │   ├── components.md                  # src/components/ 작업 시
│   │   ├── performance.md                 # src/ 전체 작업 시
│   │   ├── error-handling.md              # src/services/ 작업 시
│   │   ├── design-system.md               # src/styles/ 작업 시
│   │   ├── git-workflow.md                # 항상 로드
│   │   └── safety.md                      # 항상 로드 (위험 명령 가드레일)
│   │
│   └── skills/                            # 커스텀 워크플로우 (호출 시에만 로드)
│       ├── project-kickoff/SKILL.md       # /project-kickoff
│       ├── team-lead/SKILL.md             # /team-lead
│       ├── design-system/SKILL.md         # /design-system
│       ├── review/SKILL.md                # /review
│       ├── investigate/SKILL.md           # /investigate
│       ├── careful/SKILL.md               # /careful
│       ├── ship/SKILL.md                  # /ship
│       ├── qa/SKILL.md                    # /qa
│       ├── retro/SKILL.md                 # /retro
│       └── plan-review/SKILL.md           # /plan-review
│
├── docs/
│   ├── usage-guide.md                     # 상세 사용 가이드
│   ├── quick-start.md                     # 빠른 시작 가이드
│   ├── session-orchestration.md           # 세션 팀 운영 가이드
│   └── adr/
│       └── 000-template.md                # 아키텍처 결정 기록 템플릿
│
└── README.md                              # 이 파일
```

---

## 자주 묻는 질문

### Q: Claude Code를 안 쓰면 이 파일들은 쓸모없나요?
네. 이 보일러플레이트는 Claude Code 전용입니다. VS Code의 Copilot이나 Cursor와는 다른 도구입니다.

### Q: React 말고 Next.js나 Vue를 쓰고 싶은데?
`CLAUDE.md`의 기술 스택 섹션과 `.claude/rules/`의 규칙 파일들을 수정하면 됩니다. 이 보일러플레이트는 React+Vite에 최적화되어 있지만, 규칙을 바꾸면 어떤 스택이든 적용 가능합니다.

### Q: 이 파일들이 내 프로젝트 빌드에 영향을 주나요?
아닙니다. `CLAUDE.md`, `.claude/`, `docs/`는 모두 Claude Code가 읽는 문서일 뿐입니다. 빌드나 실행에 전혀 영향을 주지 않습니다.

### Q: `/명령어`가 동작하지 않아요
Claude Code 채팅창에서만 동작합니다. 일반 터미널 명령어가 아닙니다. Claude Code를 실행한 후 (`claude` 명령어) 채팅 인터페이스에서 입력하세요.

---

## 상세 가이드

- **[사용 가이드](docs/usage-guide.md)** — 코드를 모르는 사람도 따라할 수 있는 상세 설명
- **[빠른 시작](docs/quick-start.md)** — 경험자용 간결한 가이드
- **[세션 오케스트레이션](docs/session-orchestration.md)** — 프로젝트 규모별 세션 구성 전략

---

## 라이선스

개인 사용 목적. 자유롭게 수정하여 사용하세요.
