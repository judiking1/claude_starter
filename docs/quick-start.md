# Quick Start Guide

> 이 보일러플레이트로 새 프로젝트를 시작하는 방법

---

## 1. 보일러플레이트 복제

```bash
# 이 레포를 복제
git clone <this-repo-url> my-project
cd my-project

# git 히스토리 초기화
rm -rf .git && git init
```

## 2. CLAUDE.md 커스터마이징

`CLAUDE.md`에서 플레이스홀더를 교체:
- `[PROJECT_NAME]` → 프로젝트 이름
- `[PROJECT_DESCRIPTION]` → 프로젝트 설명

## 3. 프로젝트 킥오프 (Claude와 함께)

```
claude

> /project-kickoff my-project
```

이 스킬이 대화형으로 다음을 설정합니다:
- 프로젝트 컨셉 & 타겟 페르소나
- UX/UI 디자인 방향
- 기술 스택 조정
- 폴더 구조 및 초기 코드 생성
- 세션 구성 전략

## 4. 수동으로 시작하는 경우

```bash
# pnpm 설치 (처음이라면)
npm install -g pnpm

# 프로젝트 생성
pnpm create vite . --template react-ts

# 핵심 패키지
pnpm add zustand @tanstack/react-query tailwindcss @tailwindcss/vite react-router-dom
pnpm add -D @biomejs/biome

# Biome 설정
pnpm biome init

# 폴더 구조
mkdir -p src/{components/{ui,layout},hooks,services,stores,types,utils,pages,styles,constants,assets}
```

## 5. 파일 구조 이해

```
claude_starter/
├── CLAUDE.md                              # 핵심 규칙 (매 세션 자동 로드, ~110줄)
├── .claude/
│   ├── launch.json                        # Claude Preview dev 서버 설정
│   ├── rules/                             # 상세 규칙 (관련 파일 작업 시 자동 로드)
│   │   ├── typescript.md                  # TS 파일 작업 시
│   │   ├── components.md                  # 컴포넌트 작업 시
│   │   ├── performance.md                 # src/ 작업 시
│   │   ├── error-handling.md              # 서비스/훅 작업 시
│   │   ├── design-system.md               # 스타일/컴포넌트 작업 시
│   │   └── git-workflow.md                # 항상
│   └── skills/                            # 커스텀 워크플로우 (호출 시에만 로드)
│       ├── project-kickoff/SKILL.md       # /project-kickoff
│       ├── team-lead/SKILL.md             # /team-lead
│       ├── design-system/SKILL.md         # /design-system
│       └── review/SKILL.md                # /review
├── docs/
│   ├── quick-start.md                     # 이 파일
│   ├── session-orchestration.md           # 세션 팀 운영 가이드
│   └── adr/
│       └── 000-template.md                # 아키텍처 결정 기록 템플릿
└── README.md
```

## 6. 주요 Skills 사용법

| 스킬 | 언제 사용 | 명령 |
|------|-----------|------|
| `/project-kickoff` | 프로젝트 시작 | `/project-kickoff my-app` |
| `/team-lead` | 큰 기능 구현 | `/team-lead 인증 시스템 구현해줘` |
| `/design-system` | UI 구축 | `/design-system init` |
| `/review` | 코드 점검 | `/review` |
| `/simplify` | 코드 품질 | `/simplify` |
| `/commit` | 커밋 | `/commit` |

## 7. pnpm 핵심 명령어

```bash
pnpm install          # 의존성 설치 (= npm install)
pnpm add <pkg>        # 패키지 추가 (= npm install <pkg>)
pnpm add -D <pkg>     # 개발 의존성 추가
pnpm dev              # dev 서버 시작 (= npm run dev)
pnpm build            # 빌드 (= npm run build)
pnpm dlx <cmd>        # 일회성 실행 (= npx <cmd>)
```
